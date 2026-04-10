
# B.Cài đặt Ubuntu + Docker
## 1.Cài ubuntu 24.04.4 LTS + SSH
### a. Chuẩn bị file
- Tải file ISO: Ubuntu 24.04.4 LTS (Desktop) từ trang Ubuntu.
- Link tải: https://releases.ubuntu.com/24.04.4/
- Cài công cụ : VMware Workstation
- link tải:https://download.com.vn/vmware-workstation-8587
### b.Tạo máy ảo Ubuntu trên VMware
#### 1.Create a New Virtual Machine
<img width="1348" height="487" alt="Ảnh chụp màn hình 2026-04-10 065834" src="https://github.com/user-attachments/assets/26cd9d93-6a3c-444d-9578-43e7562c3a72" />

#### 2.Chọn Installer disc image file (iso) → trỏ đến ISO Ubuntu Server

 <img width="509" height="489" alt="image" src="https://github.com/user-attachments/assets/5d73c3dd-985f-49ad-8778-a027abac1299" />
<img width="665" height="479" alt="image" src="https://github.com/user-attachments/assets/3cfb6d42-8496-4f61-91e4-b8906adbcb3e" />

#### 3.Guest OS: Linux → Ubuntu 64-bit
#### 4.Cấu hình gợi ý:
- CPU: 2 cores
- RAM: 2–4 GB
- Disk: 20–30 

<img width="578" height="476" alt="image" src="https://github.com/user-attachments/assets/02a95b44-4a24-4ca9-8d7f-66a39b8d4e46" />

#### 5.Network Adapter: chọn Bridged 
<img width="829" height="712" alt="image" src="https://github.com/user-attachments/assets/0cf3c2e8-fa11-483f-b960-38daab2b4c40" />

#### 6.Finish → Power on máy ảo
### c.Cài Ubuntu 24.04.4 Live Server
#### 1.chọn Try or Install Ubuntu Server
#### 2.Language: chọn English
#### 3.Keyboard configuration:
- Layout: English (US)
- Variant: English (US)
- Chọn Done
#### 4.Network: để DHCP tự nhận IP → Done
#### 5.Proxy configuration: để trống → Done
#### 6. Storage:
- Chọn Use an entire disk
- Chọn Done và xác nhận Continue
#### 7.Profile configuration (tạo user để đăng nhập và SSH):
- Your name: `Minh Luong`
- Your server’s name (hostname): ubuntu-server
- Pick a username:admin1
- Choose a password / Confirm: đặt mật khẩu
- Done
#### 8. chờ cài xong
### d.Tháo ISO sau khi cài
 VMware → VM → Settings → CD/DVD
 - bỏ tick Connected
 - bỏ tick Connect at power on
 Quay lại VM và nhấn Enter để tiếp tục reboot

 <img width="650" height="385" alt="image" src="https://github.com/user-attachments/assets/7e83356e-266a-49f5-9893-9c8ee683c967" />
 
 <img width="773" height="720" alt="image" src="https://github.com/user-attachments/assets/9afb9ac8-f6ec-4e37-9982-8a0df21c61db" />

 #### e.Đăng nhập Ubuntu Server cài SSH và lấy IP
1. Sau khi máy boot vào Ubuntu Server (tty), đăng nhập user đã tạo admin1

 <img width="1153" height="623" alt="image" src="https://github.com/user-attachments/assets/4548ab22-62ba-4b26-ab7c-b4883d2df78c" />

2. Cài và bật SSH server
   
   ```sudo systemctl status ssh```
   
Tiếp tục chạy

```
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
sudo systemctl status ssh 
```

<img width="1340" height="768" alt="image" src="https://github.com/user-attachments/assets/78d6d5f0-aeb8-425a-948b-77d4ccefc79d" />

Kiểm tra IP:
```
ip -4 addr
```

- Kết quả : IPv4 `ens33` : 192.168.1.17
<img width="879" height="260" alt="image" src="https://github.com/user-attachments/assets/c057ec74-e073-47ee-997f-5a6371a3682c" />

