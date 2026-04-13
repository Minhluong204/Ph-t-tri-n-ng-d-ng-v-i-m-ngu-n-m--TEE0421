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
<img width="1120" height="623" alt="image" src="https://github.com/user-attachments/assets/e772332b-1323-4efc-95a7-7718c28eb435" />
