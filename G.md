# G. Triển khai ứng dụng đến End-user (Cloudflare Tunnel + Docker Compose)
## Tạo Cloudflare Tunnel (triển khai kiểu Docker)
1. Truy cập Cloudflare Zero Trust (Cloudflare One):
https://one.dash.cloudflare.com
2. Chọn đúng Account.

<img width="1359" height="738" alt="image" src="https://github.com/user-attachments/assets/9e618228-86df-45e2-b25d-19f70a07939a" />

3. Vào Networks → Overview.
4. Chọn Manage Tunnels → Create new cloudflared Tunnel

   <img width="1364" height="670" alt="image" src="https://github.com/user-attachments/assets/7da7a9e6-91fc-4789-9033-ae2bace32cea" />


5. Đặt tên tunnel (myapp-tunnel) → Save tunnel.

<img width="1366" height="644" alt="image" src="https://github.com/user-attachments/assets/215e837f-95f9-480c-b5c1-6e4b103d2bc4" />

6. Ở bước Install and run connectors, chọn Docker và copy Tunnel Token

<img width="1353" height="618" alt="image" src="https://github.com/user-attachments/assets/c3661963-1191-4295-ab00-10adcc044f32" />

## Convert lệnh docker run sang docker compose


## Convert lệnh docker run sang docker compose
Cloudflare cung cấp lệnh mẫu dạng:
```
docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token eyJhIjoiNzg4ZWQ5ZGY1NThhYzljNjE2MTZmMjIxNTdiNWExNTIiLCJ0IjoiNz ...
```
Thay vì chạy trực tiếp, ta đưa cấu hình này vào docker-compose.yml để quản lý cùng các container khác (nginx, nodered, myapi,…).

## Khai báo cloudflared vào `docker-compose.yml`
### 1 Tạo file `.env` để chứa token
Trong thư mục dự án, tạo file `.env`:
```
cd ~/myapp
nano .env
```
<img width="1130" height="640" alt="image" src="https://github.com/user-attachments/assets/2f84295e-f1d3-4ec2-ad44-20401596fcd3" />

### 2 Thêm service cloudflared vào `docker-compose.yml`
```nano docker-compose.yml```
Thêm service `cloudflared` :
```
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    command: tunnel --no-autoupdate --url http://nginx:80
    depends_on:
      - nginx
    restart: unless-stopped
```
<img width="1122" height="629" alt="image" src="https://github.com/user-attachments/assets/bbda0a89-df46-4dd1-b0bc-381b9a432ff6" />

### Chạy lại Docker Compose
```
cd ~/myapp
docker compose up -d
docker compose ps
```
<img width="1133" height="645" alt="image" src="https://github.com/user-attachments/assets/5947c2db-650f-4b7d-ae3f-9d2bc235f482" />

### Public ứng dụng bằng router/route trỏ về container (subdomain)
Trên Cloudflare Zero Trust:
- Networks → Tunnels → chọn tunnel myapp-tunnel
- Vào tab Published application routes
- Chọn Add a published application route
Điền:
Hostname
- Subdomain: trống
- Domain: minhluong204.id.vn
- Path: để trống (public toàn bộ website)

  
 <img width="1366" height="633" alt="image" src="https://github.com/user-attachments/assets/35254c83-4a61-4035-abbb-025a72872757" />

 
Service (Origin)
- Type: HTTP
- URL: nginx:80
Bấm Save / Complete setup.

<img width="1030" height="472" alt="image" src="https://github.com/user-attachments/assets/6294eba0-0547-4fe5-a98b-9490d5daf36b" />


### Kiểm tra URL đã public cho end-user
Truy cập từ trình duyệt:
`minhluong204.id.vn`

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/3ed9d486-3215-4576-afc6-fe657e3a3042" />

Kiểm tra API qua Nginx

<img width="1364" height="722" alt="image" src="https://github.com/user-attachments/assets/2ba7d7d2-a0b8-4430-adce-5d96a1b3524f" />

