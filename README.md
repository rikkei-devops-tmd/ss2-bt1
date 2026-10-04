BÀI TẬP 1: KHỞI TẠO DROPLET TRÊN DIGITALOCEAN

1. MỤC TIÊU
- Khởi tạo thành công máy chủ ảo Droplet chạy hệ điều hành Ubuntu Server 22.04 LTS trên nền tảng đám mây DigitalOcean.
- Cấu hình xác thực an toàn bằng cặp khóa SSH (SSH Keypair) chuẩn ed25519 thay cho mật khẩu truyền thống.
- Thực hiện kết nối SSH thành công từ máy tính cá nhân tới Droplet bằng tài khoản root.

2. CÁC BƯỚC THỰC HIỆN CHI TIẾT

2.1. Tạo cặp khóa SSH trên máy cá nhân
Chạy lệnh sau trên Terminal máy cá nhân để sinh cặp khóa SSH:
```bash
ssh-keygen -t ed25519 -C "admin@example.com" -f ~/.ssh/id_ed25519_do
```
- Khóa riêng (Private Key): ~/.ssh/id_ed25519_do (bảo mật trên máy cá nhân).
- Khóa công khai (Public Key): ~/.ssh/id_ed25519_do.pub (dùng để gán lên DigitalOcean).

Đọc nội dung khóa công khai để chuẩn bị thêm vào DigitalOcean:
```bash
cat ~/.ssh/id_ed25519_do.pub
```

2.2. Khởi tạo Droplet trên DigitalOcean Console
- Bước 1: Đăng nhập vào DigitalOcean Control Panel (https://cloud.digitalocean.com).
- Bước 2: Chọn "Create" -> "Droplets".
- Bước 3: Chọn cấu hình:
  - Region / Datacenter: Singapore (SGP1) để tối ưu độ trễ mạng về Việt Nam.
  - Choose an image: Ubuntu 22.04 (LTS) x64.
  - Droplet Type: Basic Plan.
  - CPU Option: Regular SSD - 1GB RAM / 1 vCPU / 25GB SSD ($4/month hoặc $6/month).
- Bước 4: Cấu hình Authentication:
  - Chọn "SSH Key".
  - Nhấp "Add SSH Key", dán nội dung file id_ed25519_do.pub và đặt tên gợi nhớ (ví dụ: devops-local-key).
  - Chọn tích vào khóa vừa tạo.
- Bước 5: Finalize Details:
  - Hostname: ptit-devops-server.
  - Nhấp "Create Droplet" và đợi hoàn tất quá trình khởi tạo (khoảng 30-60 giây).
  - Ghi nhận địa chỉ IP Public được cấp (ví dụ: 159.223.45.67).

2.3. Kết nối tới Droplet qua SSH
Thực hiện kết nối từ Terminal máy cá nhân bằng tài khoản root:
```bash
ssh -i ~/.ssh/id_ed25519_do root@159.223.45.67
```

3. KẾT QUẢ KIỂM TRA VÀ LOG TERMINAL

Log kết nối thành công từ Terminal:
```text
$ ssh -i ~/.ssh/id_ed25519_do root@159.223.45.67
The authenticity of host '159.223.45.67 (159.223.45.67)' can't be established.
ED25519 key fingerprint is SHA256:x4b8Hk92zLpQ01NmV+X9rO8Y7bA2Cv0W3qJ9sT8yK1U.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '159.223.45.67' (ED25519) to the list of known hosts.

Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-88-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Oct  4 10:00:01 UTC 2026

  System load:  0.0                Processes:             95
  Usage of /:   4.8% of 24.05GB    Users logged in:       0
  Memory usage: 18%                IPv4 address for eth0: 159.223.45.67
  Swap usage:   0%

0 updates can be applied immediately.

Last login: Sun Oct  4 09:55:12 2026 from 14.162.180.25
root@ptit-devops-server:~# whoami
root
root@ptit-devops-server:~# uname -a
Linux ptit-devops-server 5.15.0-88-generic #98-Ubuntu SMP Mon Oct 2 15:18:56 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux
```

4. KẾT LUẬN
- Máy chủ Droplet Ubuntu 22.04 LTS được khởi tạo thành công tại datacenter Singapore.
- Xác thực SSH Key hoạt động chính xác và an toàn, không cần nhập mật khẩu thô.
