# E. Triển khai (level test) ứng dụng
## 1. Chuyển vào trong thư mục dự án
```
cd ~/myapp
pwd
```
## 2. Chạy Docker Compose (run tất cả services trong `docker-compose.yml`)
Chạy lệnh :
```
docker compose up -d
```

<img width="1118" height="632" alt="image" src="https://github.com/user-attachments/assets/5641f988-00fa-4449-969d-7a6663af9951" />

## 3. Kiểm tra các container đang chạy (phát hiện restart)
Kiểm tra trạng thái:
```
docker compose ps
```

<img width="1138" height="631" alt="image" src="https://github.com/user-attachments/assets/07defa1f-a160-4a5a-899e-3b404fdc11f3" />

## 4. Kiểm thử các service đang chạy độc lập theo IP và port
### Xem IP của Ubuntu
```
ip -4 addr
```

<img width="1119" height="635" alt="image" src="https://github.com/user-attachments/assets/91e04def-d7af-4e75-9f69-d3c3071ab43c" />

### Kiểm thử Nginx
```
curl http://localhost/api/
```
<img width="1366" height="733" alt="image" src="https://github.com/user-attachments/assets/c16b279a-902c-46b6-80d5-350e8c03b1e1" />

### Kiểm thử Node‑RED
Trên Ubuntu:
```
curl -I http://localhost:1880/
```
<img width="843" height="200" alt="image" src="https://github.com/user-attachments/assets/2d47ab67-9a25-4a91-b945-a12d989a18d9" />

Trên Windows mở trình duyệt:
- `http://192.168.1.10:1880/`

<img width="1366" height="708" alt="image" src="https://github.com/user-attachments/assets/bee4bf92-5d3d-42b5-8af7-d3cc1986cb1a" />

## 5. Sử dụng Node‑RED tạo API GET đơn giản (http_in → function → http_response)
### Tạo Flow trong Node‑RED
Mở Node‑RED:
`http://192.168.1.8:1880/`

Tạo 3 node và nối theo thứ tự:
1. http in
- Method: GET
- URL: /api/hello

2. function
   ```
   msg.payload = { ok: true, msg: "Hello from Node-RED" };
return msg;
```
3. http response
 Bấm Deploy để lưu flow.

### Test API trực tiếp qua Node‑RED
```
curl -i http://localhost:1880/api/hello
```

### Cấu hình Nginx `/api/` reverse proxy sang Node‑RED
Mở file:
```
nano ~/myapp/nginx/nginx.conf
```
Thêm/sửa block trong `server { ... }`:
```
server {
    listen 80;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }

    location /api/ {
        proxy_pass http://nodered:1880;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
Restart Nginx:
```
docker compose restart nginx
```

### Test API qua Nginx Reverse Proxy
