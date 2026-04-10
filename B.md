
# B.Cài đặt Ubuntu + Docker
## 1.Cài ubuntu 24.04.4 LTS + SSH
### a. Chuẩn bị file
#### Tải file ISO: Ubuntu 24.04.4 LTS (Desktop) từ trang Ubuntu.
#### Link tải: https://releases.ubuntu.com/24.04.4/
#### Cài công cụ : VMware Workstation
#### link tải:https://download.com.vn/vmware-workstation-8587
### b.Tạo máy ảo Ubuntu trên VMware
#### 1.Create a New Virtual Machine
<img width="1348" height="487" alt="Ảnh chụp màn hình 2026-04-10 065834" src="https://github.com/user-attachments/assets/26cd9d93-6a3c-444d-9578-43e7562c3a72" />
#### 2.Chọn Installer disc image file (iso) → trỏ đến ISO Ubuntu Server
 <img width="509" height="489" alt="image" src="https://github.com/user-attachments/assets/5d73c3dd-985f-49ad-8778-a027abac1299" />
<img width="665" height="479" alt="image" src="https://github.com/user-attachments/assets/3cfb6d42-8496-4f61-91e4-b8906adbcb3e" />

#### 3.Guest OS: Linux → Ubuntu 64-bit
#### 4.Cấu hình gợi ý:
#### 	- CPU: 2 cores
####	 - RAM: 2–4 GB
#### 	- Disk: 20–30 

<img width="578" height="476" alt="image" src="https://github.com/user-attachments/assets/02a95b44-4a24-4ca9-8d7f-66a39b8d4e46" />

#### 5.Network Adapter: chọn Bridged 
<img width="829" height="712" alt="image" src="https://github.com/user-attachments/assets/0cf3c2e8-fa11-483f-b960-38daab2b4c40" />

#### 6.Finish → Power on máy ảo
### c.Cài Ubuntu 24.04.4 Live Server
#### 1.chọn Try or Install Ubuntu Server
#### 2.Language: chọn English
#### 3.Keyboard configuration:
#### - Layout: English (US)
#### - Variant: English (US)
#### Chọn Done
#### 4.Network: để DHCP tự nhận IP → Done
#### 5.Proxy configuration: để trống → Done
#### 6. Storage:
#### Chọn Use an entire disk
#### Chọn Done và xác nhận Continue
#### 7.Profile configuration (tạo user để đăng nhập và SSH):
- Your name: Minh Luong
- Your server’s name (hostname): ubuntu-server
- Pick a username:admin1
- Choose a password / Confirm: đặt mật khẩu
- Done
