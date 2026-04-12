
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

- Kết quả : IPv4 `ens33` : 192.168.1.12
  
<img width="1066" height="159" alt="image" src="https://github.com/user-attachments/assets/3ac0664f-03e0-499c-936e-646246d4a4ff" />

#### f.SSH từ Windows CMD vào Ubuntu
Trên Windows mở CMD và chạy:
```
ssh admin1@192.168.1.12
```
Lần đầu kết nối sẽ hỏi xác nhận fingerprint:
- gõ yes → Enter
- Sau đó nhập password → Enter

<img width="1365" height="767" alt="image" src="https://github.com/user-attachments/assets/139e5316-321f-4422-9385-857c78f6768d" />

Kết quả sau khi SSH thành công sẽ thấy:

- Welcome to Ubuntu 24.04.4 LTS ...
- prompt dạng: `admin1@ubuntu-server:~$`

<img width="1365" height="767" alt="image" src="https://github.com/user-attachments/assets/42d499c7-1496-44b7-99ea-3437231b04d0" />

 ### 2.Tìm hiểu các lệnh cơ bản của ubuntu
 #### a.Đăng nhập Ubuntu
 Từ Windows CMD:
 ```
ssh admin1@192.168.1.12
```
Sau khi đăng nhập thành công sẽ thấy prompt dạng:
```
admin1@ubuntu-server:~$
```
#### b.Thực hành các lệnh
- Kiểm tra thư mục hiện tại + liệt kê file
```
pwd
ls
ls -l
```

<img width="283" height="118" alt="image" src="https://github.com/user-attachments/assets/1703ea7e-697f-46a6-9427-cea0dd2b86ce" />

B1:Tạo thư mục làm bài

Tạo thư mục `B2` và thư mục con `data`:
```
mkdir -p B2/data
```
Kiểm tra lại 
```
ls -l
```

<img width="550" height="220" alt="image" src="https://github.com/user-attachments/assets/99df78c4-375d-4fa2-9909-967b0fd9b196" />

B2: Chuyển thư mục làm việc: cd path
```
cd B2
pwd
ls
```

<img width="1365" height="765" alt="image" src="https://github.com/user-attachments/assets/33f7e82f-7d92-489e-a237-ecada3370945" />

B3: Tạo và sửa file bằng nano
Tạo file `note.txt`:
```
nano note.txt
```
Nhập nội dung :
- `Bai tap Thay Cop`
- `Deadline :13/04/2026`

Lưu và thoát:

- Nhấn `CTRL + O` → nhấn `Enter` để xác nhận lưu
- Nhấn `CTRL + X` để thoát

Kiểm tra lại :
`ls -l`

<img width="1360" height="768" alt="image" src="https://github.com/user-attachments/assets/381b06c9-be62-439b-afdc-23eee7a144ae" />

B4: Copy file 

Copy `note.txt` sang thư mục `data` với tên mới `note_copy.txt`:
`
cp note.txt data/note_copy.txt
`

Kiểm tra:
```
ls -l
ls -l data
```
<img width="1356" height="767" alt="image" src="https://github.com/user-attachments/assets/87ba9bef-29f8-4b2e-ac0a-714107354d93" />

B5: Thay đổi quyền file

Xem quyền hiện tại:
```
ls -l note.txt
```
Đổi quyền `note.txt` thành `644`:
```
sudo chmod 644 note.txt
ls -l note.txt
```
Đổi quyền file copy thành `777`:
```
sudo chmod 777 data/note_copy.txt
ls -l data/note_copy.txt
```

<img width="1115" height="623" alt="image" src="https://github.com/user-attachments/assets/41f9f5fb-2c36-406f-8adb-2b4cab5700c7" />

B6: Edit file bằng sudo nano
```
sudo nano data/note_copy.txt
```
Thêm 1 dòng ví dụ:
```
Da sua file
```

Lưu và thoát:
- `CTRL + O` → `Enter`
- `CTRL + X`
  
<img width="1111" height="627" alt="image" src="https://github.com/user-attachments/assets/6c67f34b-31a5-4c82-be62-c440cdad6eee" />


B7: Xem IP của máy Ubuntu
`
ip -4 addr
`

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/3684a65f-bb0b-4405-a419-e894ed20b7d2" />

### 3. Cài đặt Docker cho Ubuntu 24.04.4 LTS
#### Cập nhật hệ thống và cài gói cần thiết
```
sudo apt update
sudo apt install -y ca-certificates curl gnupg
```
#### Thêm GPG key của Docker
```
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

<img width="769" height="111" alt="image" src="https://github.com/user-attachments/assets/dd849c23-6fec-4c3c-b931-36fcc1df7e29" />

#### Thêm Docker repository
```
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo $VERSION_CODENAME) stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Cập nhật lại package list:
```
sudo apt update
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/2b6bc823-58a8-4755-b3a9-dc9d2f85eca3" />

#### Cài Docker Engine + Docker Compose plugin
```
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Bật Docker chạy ngay và tự khởi động cùng hệ thống:
```
sudo systemctl enable --now docker
```

### 4. Kiểm tra Docker đã cài thành công
```
docker --version
docker compose version
sudo systemctl status docker
```
Kết quả:
- Có version của Docker
- Có version của `docker compose`
- `systemctl status docker` báo `Active: active (running)`
(nhấn `q` để thoát màn hình status)

<img width="1220" height="668" alt="image" src="https://github.com/user-attachments/assets/2049b659-4601-4b94-8cff-494b9bb12eb8" />

### 5. Cấu hình chạy Docker không cần sudo
Thêm user hiện tại vào nhóm `docker`:
```
sudo usermod -aG docker $USER
```
Áp dụng group mới (không cần reboot):
```
newgrp docker
```
Kiểm tra nhóm hiện tại:
```
groups
```
Kỳ vọng có chữ `docker`.

Test chạy Docker không cần sudo:
```
docker run --rm hello-world
```

Nếu thấy:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

=> Docker hoạt động đúng.

<img width="1170" height="686" alt="image" src="https://github.com/user-attachments/assets/99594bda-426b-43ff-8e78-ced0faa101e8" />

### 6. Tìm hiểu tập lệnh của docker và docker compose
#### 1. Docker
Kiểm tra phiên bản và thông tin Docker
```
docker --version
docker version
docker info
```
- `docker --version`: xem phiên bản Docker client
- `docker version`: xem chi tiết phiên bản client/server
- `docker info`: thông tin tổng quan Docker Engine (storage driver, số container, network…)

Quản lý Image
```
docker images
docker pull nginx:latest
docker rmi nginx:latest
docker image prune -a
```
- `docker images`: liệt kê các image đang có
- `docker pull <image>:<tag>`: tải image từ registry (thường là Docker Hub)
- `docker rmi <image>`: xoá image
- `docker image prune -a`: dọn image không dùng (cẩn thận vì có thể xoá nhiều image)

Quản lý Container
```
docker ps
docker ps -a
docker run --name web -d -p 80:80 nginx:latest
docker logs web
docker exec -it web bash
docker stop web
docker start web
docker restart web
docker rm web
```
- `docker ps`: xem container đang chạy
- `docker ps -a`: xem tất cả container (kể cả đã dừng)
- `docker run`: tạo + chạy container
Ví dụ: `-d` chạy nền, `--name` web đặt tên, `-p 80:80` map cổng host:container
- `docker logs <name`>: xem log
- `docker exec -it <name> bash`: vào container để thao tác
- `docker stop/start/restart`: dừng / chạy / restart container
- `docker rm <name>`: xoá container (phải stop trước, hoặc dùng `-f`)

Quản lý Volume
```
docker volume ls
docker volume create mydata
docker volume inspect mydata
docker volume rm mydata
```

Ghi chú:
- Volume giúp dữ liệu không mất khi xoá container.
- Thường dùng cho database, ứng dụng cần lưu dữ liệu.

 Quản lý Network 
 ```
docker network ls
docker network create mynet
docker network inspect mynet
docker network rm mynet
```

Ghi chú:
- Network giúp các container giao tiếp với nhau qua tên container/service.
- Khi dùng Docker Compose, network thường được tạo tự động.

#### 2.Docker Compose 

Kiểm tra phiên bản
```
docker compose version
```

Các lệnh Compose cơ bản
```
docker compose up -d
docker compose ps
docker compose logs -f
docker compose down
```

Giải thích:

- `up -d`: tạo và chạy tất cả service theo file `docker-compose.yml` / `compose.yaml`
- `ps`: xem trạng thái các service/container
- `logs -f`: xem log realtime
- `down`: dừng và xoá container + network do compose tạo
(muốn xoá cả volume dùng `docker compose down -v`)

Build image (khi có Dockerfile)
```
docker compose build
docker compose up -d --build
```

Stop / Start / Restart theo stack compose
```
docker compose stop
docker compose start
docker compose restart
```

Xem cấu hình sau khi compose xử lý biến môi trường
```
docker compose config
```

### 7.Mở firewall UFW cho cổng 80, 1880, 9630

Kiểm tra trạng thái UFW
```
sudo ufw status
```

- Nếu kết quả là `Statu: inactive`: UFW đang tắt (chưa chặn gì).
- Nếu kết quả là `Status: active`: UFW đang bật (đang áp dụng rule).

<img width="437" height="87" alt="image" src="https://github.com/user-attachments/assets/350aafc6-65d5-4b1b-832b-4a1a0d4b8e24" />


Cho phép SSH để tránh mất kết nối

Vì thao tác qua SSH nên cần cho phép SSH (port 22) trước khi bật UFW:
```
sudo ufw allow OpenSSH
```

<img width="460" height="69" alt="image" src="https://github.com/user-attachments/assets/10c28f12-86ca-4448-9923-42a1b2f6d774" />

Mở các cổng yêu cầu: 80, 1880, 9630
```
sudo ufw allow 80/tcp
sudo ufw allow 1880/tcp
sudo ufw allow 9630/tcp
```

<img width="1129" height="635" alt="image" src="https://github.com/user-attachments/assets/a83f946b-f0c6-4ab3-8c19-6400fe8d67f0" />

Bật UFW và cho phép tự khởi động cùng hệ thống
```
sudo ufw enable
```
Nhập `y` rồi `enter`

<img width="1160" height="649" alt="image" src="https://github.com/user-attachments/assets/17cbe12d-9365-40da-9656-73b715610fdb" />


Kiểm tra lại rule đã được áp dụng
```
sudo ufw status numbered
```

<img width="1165" height="644" alt="image" src="https://github.com/user-attachments/assets/044e7cfc-85f5-45aa-af08-d3b97df88423" />



