# C. Cấu hình Docker Compose
## 1. Tạo thư mục ~/myapp
Chạy lệnh:
```
mkdir -p ~/myapp
```

<img width="1110" height="651" alt="image" src="https://github.com/user-attachments/assets/b7adbc54-f8d6-47ba-a2a6-c2cbd21d8ddf" />

## 2. Chuyển vào trong thư mục ~/myapp
```
cd ~/myapp
```
## 3. Tạo thư mục ./myweb
Đảm bảo đang đứng trong `~/myapp`, sau đó chạy:
```
mkdir -p ./myweb
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/9448ab5e-e686-41af-a46e-368f566b70c5" />

## 4. Tạo file ./myweb/index.html 
Tạo và chỉnh sửa file:
```
nano ./myweb/index.html
```

<img width="1139" height="634" alt="image" src="https://github.com/user-attachments/assets/77d4a764-958c-4585-bbae-73cf5477d927" />


Nhập nội dung :
```
  GNU nano 7.2                                       ./myweb/index.html *
<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Thông tin cá nhân</title>
</head>
<body>
  <h1>Thông tin cá nhân</h1>
  <ul>
    <li>Họ & tên: LANG NGUYEN MINH LUONG</li>
    <li>MSSV: K225480106044</li>
    <li>Lớp: K58KTP</li>
    <li>Trường: Đại học Kỹ thuật Công nghiệp Thái Nguyên</li>
    <li>Email: k225480106044@gmail.com</li>
  </ul>
</body>
</html>
```

Lưu và thoát nano:

- Lưu: `Ctrl + O` → `Enter`
- Thoát: `Ctrl + X`

## Tạo file docker-compose.yml (Node-RED + Nginx)
Tạo các thư mục cần thiết

Trong thư mục `~/myapp`, tạo các thư mục chứa dữ liệu Node-RED và cấu hình Nginx:
```
cd ~/myapp
mkdir -p ./nodered ./nginx
```
Tạo thư mục dữ liệu cho Node-RED và thư mục cấu hình cho Nginx
```
mkdir -p ./nodered ./nginx
```

Tạo file `./nginx/nginx.conf` để tránh lỗi mount
```
nano ./nginx/nginx.conf
```

Dán cấu hình tối thiểu:
```
events {}

http {
  server {
    listen 80;
    server_name localhost;

    location / {
      root /myweb;
      index index.html;
      try_files $uri $uri/ /index.html;
    }
  }
}
```

Lưu và thoát:
- Lưu: `Ctrl + O` → `Enter`
 Thoát: `Ctrl + X`

Tạo file `docker-compose.yml`
Tạo file:
```
nano docker-compose.yml
```
Nội dung file `docker-compose.yml`:
```
services:
  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data
    restart: unless-stopped

  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "80:80"
    volumes:
      - ./myweb:/myweb:ro
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - nodered
    restart: unless-stopped
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/54c08a91-8c5b-4b8e-adb2-93b80dbd16e0" />

Lưu và thoát: 
- Lưu: `Ctrl + O` → `Enter`
- Thoát: `Ctrl + X`
 ## 6. Cấu hình Nginx Reverse Proxy (`./nginx/nginx.conf`)
 ### Bước 1: Mở file cấu hình Nginx
 ```
cd ~/myapp
nano ./nginx/nginx.conf
```
### Bước 2: Cấu hình `nginx.conf`
```
events {}

http {
  server {
    # (1) Web server cổng 80
    listen 80;

    # (2) server_name là sub-domain 
    server_name app.luongvanhoc.io.vn;

    # (3) location / trỏ tới root là /myweb (web tĩnh)
    location / {
      root /myweb;
      index index.html index.htm;
      try_files $uri $uri/ /index.html;
    }

    # (4) location /api proxy_pass sang Node-RED (http_in)
    # Lưu ý quan trọng: KHÔNG có dấu "/" ở cuối proxy_pass để giữ nguyên đường dẫn /api/...
    location /api/ {
      proxy_pass http://nodered:1880;

      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
    }
  }
}
```
Lưu và thoát nano:
- Lưu: `Ctrl + O` → `Enter`
- Thoát: `Ctrl + X`

### Bước 3: Khởi động/restart Docker Compose để áp dụng cấu hình
```
docker compose up -d
```
Kiểm tra container:
```
docker compose ps
```
<img width="1109" height="307" alt="image" src="https://github.com/user-attachments/assets/63dc055e-b130-4cd0-9dc5-fb427fdd512f" />

### Bước 4: Tạo API trong Node-RED để test proxy /api
Mở Node-RED trên trình duyệt:
`http://192.168.1.12:1880/`

Tạo flow gồm 3 node:
1. http in
- Method: `GET`
- URL:` /api/hello`
2. function
  ```
  msg.payload = { ok: true, msg: "Hello from Node-RED" };
return msg;
```
3. http response
Nối: `http in` → `function` → `http response`, sau đó bấm `Deploy`.

<img width="1355" height="674" alt="image" src="https://github.com/user-attachments/assets/5fb6bded-37ca-4ecc-87fd-db5f5a0beb64" />

### Bước 5: Kiểm tra kết quả
1. Kiểm tra web tĩnh (location `/`)
```
curl -I http://localhost/
curl http://localhost/ | head -n 20
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/8f0f27e4-5c48-4e03-b36c-5db7c617cb3f" />

2. Kiểm tra proxy sang Node-RED (location `/api`)
```
curl http://localhost/api/hello
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/e96e58d1-7dc0-4cde-b619-8169aaa4b46e" />

## 7. Bắt buộc đăng nhập Node-RED (Edit `./nodered/settings.js`)
### 1. Chạy docker compose lần đầu để Node-RED tự sinh cấu hình trong `./nodered`
Chuyển vào thư mục dự án và chạy compose:
```
cd ~/myapp
docker compose up -d
```
Kiểm tra các container đang chạy:
```
docker compose ps
```
Kiểm tra Node-RED đã tự sinh dữ liệu và file cấu hình trong thư mục `./nodered`:
```
ls -la ./nodered | head
ls -la ./nodered/settings.js
```

<img width="1366" height="728" alt="image" src="https://github.com/user-attachments/assets/69fc9aed-4c40-432f-93f2-71f2c1953623" />

### 2.Tạo mật khẩu dạng hash (bcrypt) để dùng trong `settings.js`
Node-RED yêu cầu lưu mật khẩu ở dạng hash. Tạo hash bằng cách chạy lệnh sau:
```
docker exec -it nodered node -e "console.log(require('bcryptjs').hashSync(process.argv[1], 8));" '123'
```
Sau khi chạy, terminal sẽ in ra chuỗi hash $2b$08$7GYRDXh8Oni3X/aBfTbL4u1eakp1bHyH2K3ir2m/GTLHggwLY68Ya
Copy chuỗi hash này để dán vào `settings.js`.
### 3. Edit `./nodered/settings.js` để bật đăng nhập (adminAuth)
Mở file:
```
nano ./nodered/settings.js
```
Trong nano, tìm `adminAuth`:

Bấm `Ctrl + W`
- Gõ adminAuth rồi `Enter`
Tại block `adminAuth`, tiến hành:
- Bỏ comment (xoá dấu // ở đầu các dòng của block adminAuth nếu đang bị comment)
- Điền username và dán password (hash bcrypt) vừa tạo

<img width="1366" height="730" alt="image" src="https://github.com/user-attachments/assets/f1d22efc-8777-4dd7-bc6e-f284731d8f40" />

Lưu và thoát:

- Lưu: `Ctrl + O` → `Enter`
- Thoát: `Ctrl + X`

### 4. Restart Node-RED để áp dụng cấu hình
```
docker compose restart nodered
```
### 5. Kiểm tra kết quả
Mở trình duyệt:`http://192.168.1.12:1880/`

<img width="1347" height="710" alt="image" src="https://github.com/user-attachments/assets/0e2aa372-3ceb-49d5-80cc-c93c58ef8d3c" />

