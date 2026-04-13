# F. Gỡ lỗi (Debug)
## Nếu có lỗi xảy ra trong quá trình triển khai `docker compose up -d`
### Chạy triển khai
```
cd ~/myapp
docker compose up -d
```
### Kiểm tra nhanh container nào đang chạy / bị lỗi
```
docker compose ps
```
### Xem log của container/service để tìm lỗi
```
docker logs nginx
docker logs myapi
```
### Sửa cấu hình rồi chạy lại
- Sửa các file liên quan (`docker-compose.yml`, `./nginx/nginx.conf`, `./nodered/settings.js`, `./myapi/app.py`, …)
- Áp dụng lại:
Nếu chỉ sửa file cấu hình mount (nginx.conf, settings.js, …) thì restart service:
```
docker compose restart nginx
docker compose restart nodered
docker compose restart myapi
```
Nếu có sửa Dockerfile/requirements/code của myapi (cần build lại image):
```
docker compose up -d --build myapi
```
Sau đó kiểm tra lại:
```
docker compose ps
```
## Thêm healthcheck cho myapi trong file `docker-compose.yml`
### Thêm cấu hình healthcheck
Mở file:
```
nano ~/myapp/docker-compose.yml
```
Thêm vào service `myapi`:
```
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:9630"]
  interval: 10s
  timeout: 5s
  retries: 5
  start_period: 10s
```
<img width="1128" height="626" alt="image" src="https://github.com/user-attachments/assets/f8a4e7dc-8cd7-4d82-b74c-44ab61f180ac" />

### Áp dụng cấu hình
```
docker compose up -d
docker compose ps
```
## Giới hạn resource cho một service + quan sát RAM bằng `docker compose stats`
### Giới hạn RAM cho 1 service
Mở `nano ~/myapp/docker-compose.yml`
Thêm 
```
deploy:
  resources:
    limits:
      memory: 512M
```

<img width="1149" height="651" alt="image" src="https://github.com/user-attachments/assets/93a7c861-4c2a-4ac1-90f3-c08fbb4324f6" />

Chạy `docker compose up -d`

### Quan sát lượng RAM sử dụng của mỗi service
Chạy :`docker compose stats`

<img width="1128" height="277" alt="image" src="https://github.com/user-attachments/assets/841e8453-d38a-445b-aa1d-4c27ccb55cdd" />

Thoát:
- `Ctrl + C`

