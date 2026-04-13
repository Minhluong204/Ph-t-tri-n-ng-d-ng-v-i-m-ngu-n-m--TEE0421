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
Do Node‑RED endpoint đang là /api/hello, và Nginx prefix cũng là /api/, nên URL test sẽ là:
```
curl -i http://localhost/api/api/hello
```
<img width="1247" height="687" alt="image" src="https://github.com/user-attachments/assets/b777a010-8b8a-4216-bc3d-d9ac7c949f28" />

## 6. Sửa `./myweb/index.html` để gọi API đã khai báo proxy_pass 
Mở file:
```
nano ~/myapp/myweb/index.html
```
Nội dung ví dụ hoàn chỉnh (HTML + JS gọi API qua Nginx):
```
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

  <h2>Test API qua Nginx</h2>
  <button onclick="callAPI()">Gọi API</button>

  <pre id="result"></pre>

  <script>
    function callAPI() {
      fetch('/api/hello')
        .then(res => res.json())
        .then(data => {
          document.getElementById('result').innerText =
            JSON.stringify(data, null, 2);
        })
        .catch(err => {
          document.getElementById('result').innerText = "Lỗi: " + err;
        });
    }
  </script>
</body>
</html>
```

<img width="1133" height="628" alt="image" src="https://github.com/user-attachments/assets/352a62ff-f9c9-4b5d-9a9f-1833d29ed9ad" />

Kiểm thử trên trình duyệt (End-user)
- Trên Windows mở: `http://192.168.1.10/`

  <img width="728" height="464" alt="image" src="https://github.com/user-attachments/assets/ba6698f3-f848-4799-a91a-5ed3b31f7bbf" />

