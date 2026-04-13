# D.Bonus
## 1. Tạo thư mục `./myapi`
```
cd ~/myapp
mkdir -p ./myapi
```
## 2. Tạo file `./myapi/app.py`
Tạo file :
```
nano ./myapi/app.py
```

<img width="500" height="64" alt="image" src="https://github.com/user-attachments/assets/99c41ab2-5d9b-4f2c-8bb7-b9da5def5437" />

 Dán nội dung :
 ```
from flask import Flask, jsonify, request
import random
import datetime

app = Flask(__name__)

jokes = [
    "Tại sao lập trình viên thích mùa đông? Vì có nhiều 'bug' cần fix 😄",
    "Code không chạy? Đừng lo, đó là feature chưa được khám phá!",
    "Tôi không lười, tôi chỉ đang chạy ở chế độ tiết kiệm năng lượng.",
    "Debugging: Là nghệ thuật xóa bug mà mình vừa tạo ra.",
]

@app.route('/')
def home():
    return jsonify({
        "message": "Welcome to Funny API 🤡",
        "time": str(datetime.datetime.now())
    })

@app.route('/joke')
def get_joke():
    return jsonify({
        "joke": random.choice(jokes)
    })

@app.route('/roast', methods=['GET'])
def roast():
    name = request.args.get('name', 'bạn')
    roasts = [
        f"{name} code như viết văn nghị luận 😏",
        f"{name} chạy code mà bug chạy nhanh hơn 🏃",
        f"{name} debug 2 tiếng, fix bằng restart 😆",
    ]
    return jsonify({
        "roast": random.choice(roasts)
    })

@app.route('/luck')
def luck():
    percent = random.randint(0, 100)
    return jsonify({
        "luck": f"Hôm nay bạn may mắn {percent}% 🍀"
    })

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)

```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/6cd539a5-dcdf-4af9-9fb2-496f57d53bdf" />

Lưu/thoát:
- Lưu: `Ctrl + O` → `Enter`
- Thoát: `Ctrl + X`

## 3. Tạo file `./myapi/requirements.txt`
```
nano ./myapi/requirements.txt
```
Nội dung
```
flask
```
## 4. Tạo file `./myapi/Dockerfile`
```
nano ./myapi/Dockerfile
```
 nội dung:
 ```
# Sử dụng phiên bản Python nhẹ để giảm dung lượng image
FROM python:3.9-slim

# Thiết lập thư mục làm việc bên trong container
WORKDIR /app

# Sao chép file requirements vào và cài đặt thư viện
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Sao chép toàn bộ mã nguồn vào container
COPY . .

# Thông báo container sẽ chạy ở cổng 9630
EXPOSE 9630

# Lệnh khởi chạy ứng dụng
CMD ["python", "app.py"]
```
## 5. Sửa `docker-compose.yml` để chạy service `myapi`
```
nano ~/myapp/docker-compose.yml
```
Thêm service `myapi` (đảm bảo indent YAML đúng, myapi phải cùng cấp với nginx, nodered):
```
  myapi:
    build:
      context: ./myapi
      dockerfile: Dockerfile
    container_name: myapi
    ports:
      - "9630:9630"
    restart: unless-stopped
```

<img width="1123" height="635" alt="image" src="https://github.com/user-attachments/assets/31842417-e2f3-4e41-a409-7fddacc2ca36" />

Kiểm tra cấu hình compose:
```
cd ~/myapp
docker compose config
```
Build + chạy lại:
```
docker compose up -d --build
docker compose ps
```
Test `myapi` trực tiếp 
```
http://192.168.1.10:9630/
```

<img width="829" height="190" alt="image" src="https://github.com/user-attachments/assets/1d359b83-72ec-4aca-8364-25c2dc71f9e5" />

## 6. Sửa `nginx/nginx.conf` để `/api` trỏ tới `myapi` cổng 9630
Mở flie :
```
nano ~/myapp/nginx/nginx.conf
```
Sửa/Thay block location `/api`  thành:
```
location /api {
  proxy_pass http://myapi:9630/tinh-vat;
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto $scheme;
}
```
Restart nginx:
```
docker compose restart nginx
```
## Kiểm thử cuối cùng
Gọi API thông qua Nginx (port 80):
```
curl -i "http://192.168.1.10/api/"
```
