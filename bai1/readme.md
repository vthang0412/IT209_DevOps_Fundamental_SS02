# Bài 1 – Khởi tạo VPS và kết nối SSH

## 1. Mục tiêu

Thực hành khởi tạo một máy chủ VPS chạy Ubuntu Server trên nền tảng cloud và kết nối tới máy chủ bằng SSH Key thay vì sử dụng mật khẩu.

> Lưu ý: Bài thực hành được triển khai trên AZVPS thay cho DigitalOcean.

---

## 2. Thông tin VPS

| Thành phần   | Thông tin                  |
| ------------ | -------------------------- |
| Nhà cung cấp | AZVPS                      |
| Hệ điều hành | Ubuntu 22.04 LTS           |
| CPU          | 1 vCPU                     |
| RAM          | 2 GB                       |
| Storage      | 20 GB SSD/NVMe             |
| SSH Port     | 22                         |
| User         | root                       |
| Region       | Việt Nam/Singapore tùy VPS |

> Không công khai password hoặc private SSH key trong README.

---

## 3. Kiểm tra VPS

Sau khi tạo VPS, kiểm tra trạng thái VPS trên hệ thống quản lý AZVPS.

VPS ở trạng thái:

```text
Running
```

Ảnh minh họa:

```text
![AZVPS VPS](images/01-vps.png)
```

---

## 4. Tạo SSH Key

Trên máy tính cá nhân Windows, mở PowerShell và thực hiện:

```powershell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

SSH Key được tạo tại:

```text
C:\Users\Admin\.ssh\id_ed25519
C:\Users\Admin\.ssh\id_ed25519.pub
```

Trong đó:

* `id_ed25519`: Private Key
* `id_ed25519.pub`: Public Key

Kiểm tra:

```powershell
dir $env:USERPROFILE\.ssh
```

Ảnh minh họa:

```text
![SSH Key](images/02-ssh-key.png)
```

---

## 5. Cấu hình Public Key trên VPS

Đăng nhập VPS bằng tài khoản root:

```powershell
ssh root@<IP_VPS>
```

Tạo thư mục SSH:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Thêm Public Key vào:

```bash
nano ~/.ssh/authorized_keys
```

Sau đó thiết lập quyền:

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

## 6. Kết nối VPS bằng SSH Key

Sau khi cấu hình Public Key, thoát khỏi VPS:

```bash
exit
```

Từ máy tính cá nhân thực hiện:

```powershell
ssh -i $env:USERPROFILE\.ssh\id_ed25519 root@<IP_VPS>
```

Trong đó:

```text
<IP_VPS>
```

là địa chỉ IP của VPS AZVPS.

Kết quả:

```text
Welcome to Ubuntu 22.04 LTS

root@vps:~#
```

Ảnh minh họa:

```text
![SSH Login Success](images/03-ssh-success.png)
```

---

## 7. Kiểm tra phiên bản Ubuntu

Sau khi đăng nhập thành công:

```bash
lsb_release -a
```

Kết quả xác nhận hệ điều hành:

```text
Distributor ID: Ubuntu
Description: Ubuntu 22.04 LTS
Release: 22.04
Codename: jammy
```

---

## 8. Kiểm tra tài nguyên VPS

Kiểm tra CPU:

```bash
lscpu
```

Kiểm tra RAM:

```bash
free -h
```

Kiểm tra IP:

```bash
hostname -I
```

---

## 9. Kết quả

Đã thực hiện thành công:

* Khởi tạo VPS Ubuntu Server.
* Tạo cặp SSH Key trên máy cá nhân.
* Cấu hình Public Key trên VPS.
* Kết nối VPS bằng SSH.
* Đăng nhập bằng tài khoản `root`.
* Không cần nhập password khi đăng nhập bằng SSH Key.

## 10. Cấu trúc bài nộp

```text
homework/
└── session_02/
    └── ex1/
        ├── README.md
        └── images/
            ├── 01-vps.png
            ├── 02-ssh-key.png
            └── 03-ssh-success.png
```

