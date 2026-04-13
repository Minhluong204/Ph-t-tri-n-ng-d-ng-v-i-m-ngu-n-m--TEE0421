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

<img width="1326" height="629" alt="image" src="https://github.com/user-attachments/assets/7a722b3a-cbc5-4509-8d2d-6c02aade335e" />
6. Ở bước Install and run connectors, chọn Docker và copy Tunnel Token

<img width="1353" height="618" alt="image" src="https://github.com/user-attachments/assets/c3661963-1191-4295-ab00-10adcc044f32" />


<img width="1366" height="633" alt="image" src="https://github.com/user-attachments/assets/35254c83-4a61-4035-abbb-025a72872757" />


<img width="1030" height="472" alt="image" src="https://github.com/user-attachments/assets/6294eba0-0547-4fe5-a98b-9490d5daf36b" />

## Convert lệnh docker run sang docker compose

<img width="1122" height="624" alt="image" src="https://github.com/user-attachments/assets/7fbc7590-35f8-410a-a5d1-8f0c3255d82a" />
