## G. Câu hỏi về bài làm

### 1. Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?

Nginx đóng vai trò là một gateway trung gian giúp quản lý và điều phối request. Khi dùng Nginx, ta có thể:

* Route nhiều service khác nhau (Node-RED, Flask API, web tĩnh) qua các path như `/api`, `/`.
* Tăng bảo mật (ẩn service thật phía sau).
* Dễ mở rộng hệ thống sau này.

Nếu trỏ thẳng vào Node-RED thì:

* Không thể quản lý nhiều service cùng lúc.
* Khó mở rộng và cấu hình.

---

### 2. Sự khác biệt giữa việc Mount file và Mount thư mục trong Docker là gì?

* Mount file: chỉ ánh xạ 1 file cụ thể từ máy host vào container.
* Mount thư mục: ánh xạ toàn bộ thư mục (bao gồm nhiều file).

Ví dụ:

* File: `./nginx/default.conf:/etc/nginx/conf.d/default.conf`
* Thư mục: `./myweb:/usr/share/nginx/html`

---

### 3. Nếu thay đổi file index.html ở máy Ubuntu, nội dung trên web có thay đổi ngay không? Tại sao?

Có. Vì thư mục chứa `index.html` đã được mount trực tiếp vào container. Khi file trên host thay đổi thì container sẽ đọc nội dung mới ngay lập tức, không cần build lại.

---

### 4. restart: always và restart: unless-stopped để làm gì?

* `restart: always`: container luôn tự khởi động lại khi bị dừng hoặc khi Docker restart.
* `restart: unless-stopped`: tự restart trừ khi bị stop thủ công.

Giúp hệ thống chạy ổn định, tự phục hồi khi có lỗi.

---

### 5. Cách khai báo để tất cả services dùng chung 1 network? Lợi ích?

Khai báo:

```yaml
networks:
  mynetwork:

services:
  nginx:
    networks:
      - mynetwork

  nodered:
    networks:
      - mynetwork

  myapi:
    networks:
      - mynetwork
```

Lợi ích:

* Các container có thể gọi nhau bằng tên service (vd: `http://nginx`, `http://myapi`)
* Không cần dùng IP
* Dễ quản lý và mở rộng

---

### 6. Đưa Cloudflare Token vào .env và thêm vào .gitignore có ý nghĩa gì?

* `.env` dùng để lưu thông tin nhạy cảm (token, password).
* `.gitignore` giúp không đẩy các thông tin này lên GitHub.

=> Quan trọng vì:

* Tránh lộ token → người khác có thể chiếm quyền hệ thống.
* Đảm bảo bảo mật mã nguồn.

---

### 7. Tại sao nên thêm :ro khi mount file cấu hình Nginx?

`:ro` (read-only) giúp container chỉ đọc file, không thể ghi hoặc sửa đổi.

=> Lợi ích:

* Tăng bảo mật
* Tránh bị ghi đè file cấu hình

---

### 8. Khi dùng Cloudflare Tunnel có cần mở cổng không?

Không cần.

Cloudflare Tunnel tạo kết nối từ server ra ngoài (outbound), nên:

* Không cần mở port
* Không cần NAT
* An toàn hơn so với mở port trực tiếp

---

