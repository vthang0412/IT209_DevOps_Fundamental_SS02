# Bài 2 – Khởi tạo User thường và thiết lập đặc quyền quản trị

## 1. Mục tiêu

* Tạo user thường `devops` trên Ubuntu Server.
* Thêm user `devops` vào nhóm `sudo`.
* Sao chép SSH Key từ tài khoản `root` sang user `devops`.
* Thiết lập quyền cho thư mục `.ssh` và file `authorized_keys`.
* Kiểm tra đăng nhập SSH bằng user `devops`.
* Kiểm tra quyền quản trị bằng `sudo`.

## 2. Môi trường

* OS: Ubuntu Server
* User ban đầu: `root`
* User mới: `devops`
* SSH: Port 22

## 3. Tạo user devops

```bash
adduser devops
```

Kiểm tra:

```bash
id devops
```

## 4. Thêm devops vào nhóm sudo

```bash
usermod -aG sudo devops
```

Kiểm tra:

```bash
groups devops
```

Kết quả có nhóm:

```text
devops sudo
```

## 5. Sao chép SSH Key

Sử dụng `rsync` để sao chép cấu hình SSH từ root sang user `devops`:

```bash
rsync --archive --chown=devops:devops /root/.ssh /home/devops/
```

## 6. Phân quyền SSH

Phân quyền thư mục `.ssh`:

```bash
chmod 700 /home/devops/.ssh
```

Phân quyền file `authorized_keys`:

```bash
chmod 600 /home/devops/.ssh/authorized_keys
```

Đảm bảo quyền sở hữu:

```bash
chown -R devops:devops /home/devops/.ssh
```

Kiểm tra:

```bash
ls -ld /home/devops/.ssh
ls -l /home/devops/.ssh/authorized_keys
```

Kết quả mong đợi:

```text
drwx------ devops devops /home/devops/.ssh
-rw------- devops devops /home/devops/.ssh/authorized_keys
```

## 7. Kiểm tra quyền sudo

Chuyển sang user `devops`:

```bash
su - devops
```

Kiểm tra user:

```bash
whoami
```

Kết quả:

```text
devops
```

Kiểm tra quyền sudo:

```bash
sudo whoami
```

Nhập mật khẩu của user `devops`.

Kết quả:

```text
root
```

## 8. Kiểm tra đăng nhập SSH từ máy cá nhân

Từ máy Windows:

```powershell
ssh -i C:\Users\Admin\.ssh\id_ed25519 devops@<IP_VPS>
```

Sau khi đăng nhập:

```bash
whoami
```

Kết quả:

```text
devops
```

Tiếp tục kiểm tra:

```bash
sudo whoami
```

Kết quả:

```text
root
```

## 9. Kết luận

Đã hoàn thành việc tạo user thường `devops`, cấp quyền quản trị thông qua nhóm `sudo`, sao chép SSH Key và thiết lập quyền bảo mật cho `.ssh` và `authorized_keys`.

User `devops` có thể đăng nhập VPS bằng SSH Key và thực hiện các thao tác quản trị thông qua `sudo`, thay vì sử dụng trực tiếp tài khoản `root`.
