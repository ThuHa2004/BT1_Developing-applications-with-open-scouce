
# G. Triển khai ứng dụng đến End-user
# Yêu cầu:
1. Trong Cloudflare: Tạo tunnel (đường hầm), chọn loại triển khai cho docker
2. Convert lệnh docker run ... sang dạng docker compose
3. Khai báo kết quả convert vào trong file docker-compose.yml
4. Chạy lại docker compose
5. Public ứng dụng bằng cách thêm 1 router trỏ tới container đang chạy trong docker, dữ liệu sẽ đi qua tunnel, url dạng sub-domain
6. Kiểm tra url sub-domain đã hoạt động public cho mọi end-user
------------
# Bài làm:
Cấu trúc thư mục:
```
myapp/
├── docker-compose.yml
├── nginx/
│   └── nginx.conf
├── myweb/
│   └── index.html
└── nodered/ (sẽ tự sinh dữ liệu)
│   └── (có nhiều file tự sinh)
│   └── settings.js (file này cần edit để bắt nodered login)
```
## Tạo Tunnel trên Cloudfare
### 1. Truy cập vào **https://dash.cloudflare.com** => **Zero Trust** => **Networks** => **Connectors**
### 2. Chọn **Add a Tunnel** 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/36a992c2-1e18-4f10-bc77-ad812482826f" />

### 3. Đặt tên cho Tunnel và nhấn save
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/98a23759-ef0b-41c0-bd82-ca3081afeca4" />

### 4. Chọn device's operating system là docker
- Dãy ký tự dài bên dưới nằm sau ```--token``` chính là khóa để thông mạng ra ngoài
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c7015b37-dfe8-4598-b90a-3f12bbea6bf9" />

### 5. Cấu hình lại trong file ```docker-compose.yml```
- Thêm đoạn này vào file ```docker-compose.yml```
```
tunnel:
    container_name: cloudflared-tunnel
    image: cloudflare/cloudflared:latest
    restart: always
    command: tunnel --no-autoupdate run --token <TOKEN>
```
- Sau khi thêm xong chạy lại ```docker compose up -d``` sẽ thấy thêm container ```cloudflared-tunnel```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a98acd93-a94a-48fb-8b3f-b34229e36d13" />

### 6. Quay lại Cloudflare nhấn next, và đặt tên sub-domain để hoàn tất 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/13e3df4a-3ecf-4d7e-a166-3ccf40644e18" />

### 7. Sau khi nhấn ```complete setup``` sẽ thấy trang Connectors hiện trạng thái ```healthy``` là đường hầm đã thông
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bc52327a-c579-4698-8512-b44df85087ab" />

### 8. Kiểm tra. Truy cập vào web bằng tài khoản và thiết bị bất kỳ 
- web: **thuha_app.tranthithuha.id.vn**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5fca4b80-e293-4409-83f2-362771fbbdee" />

- API: **thuha_app.tranthithuha.id.vn/joke/**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e0ccccc2-6401-4180-9783-d06412b15be5" />

- API: **thuha_app.tranthithuha.id.vn/health/**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/057ed663-231e-46a8-a83b-aba80162f3cd" />




