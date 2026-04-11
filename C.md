# C. Cấu hình docker compose
---------
# YÊU CẦU
1. Tạo thư mục: ~/myapp
2. Chuyển vào trong thư mục ~/myapp
3. Tạo thư mục: ./myweb
4. Tạo file ./myweb/index.html (với nội dung là thông tin cá nhân của em)
5. Tạo file docker-compose.yml để nó sẽ có các dịch vụ sau:
  - Khai báo sử dụng nodered/node-red, cổng 1880, dữ liệu nằm tại thư mục ./nodered
  - Khai báo sử dụng nginx, cổng 80, cấu hình trong file ./nginx/nginx.conf
  - Mount thư mục ./myweb thành thư mục /myweb trong nginx
  - Mount file ./nginx/nginx.conf vào file /etc/nginx/nginx.conf trong nginx
6. Edit file ./nginx/nginx.conf để:
  - Cấu hình web server cổng 80
  - server_name là sub-domain (sub-domain tuỳ ý của em)
  - location / trỏ tới root là thư mục /myweb
  - location /api dùng proxy_pass trỏ tới 1 (hoặc nhiều) node http_in của nodered
7. Edit file ./nodered/settings.js để nodered bắt buộc đăng nhập
  - Chạy docker-compose lần đầu để Node-RED tự sinh file cấu hình trong thư mục ./nodered, sau đó mới tiến hành sửa settings.js và restart lại container
-------------
# BÀI LÀM
## 1. Sử dụng lệnh sau để tạo thư mục ```~/myapp``` và chuyển vào trong thư mục đã tạo để tạo các thư mục con:
```
mkdir -p ~/myapp
cd ~/myapp
mkdir myweb nginx nodred
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/359df13c-5096-4337-a9fa-2ad9c28ea781" />

## 2. Sau khi đã tạo xong thư mục rồi, tiếp tục tạo file index.html
- Tạo file ```./myweb/index.html```
- Sử dụng lệnh ```nano ~/myapp/myweb/index.html``` để viết nội dung file. Sau khi viết xong nội dung file, nhấn phím ```Ctrl+O``` để lưu nội dung file, và tổ hợp phím ```Ctrl+X``` để thoát nano.
```
<head>
        <meta charset="UTF-8">
        <title>DevApp with open source</title>

        <style>
                body {
                        margin: 0;
                        padding: 0;
                        font-family: Arial, sans-serif;
                        background: linear-gradient(135deg, #74ebd5, #ACB6E5);
                        height: 100vh;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                }

                .card {
                        background: white;
                        padding: 30px 40px;
                        border-radius: 15px;
                        box-shaddow: 0 10px 25px rgba(0,0,0,0.2);
                        text-align: center;
                        width: 350px;
                        transition: 0.3s;

                .card:hover {
                        transform: translateY(-5px);
                        box-shaddow: 0 15px 35px rgba(0,0,0,0.3);
                }

                h1 {
                        color: #333;
                        margin-bottom: 20px;
                }

                p {
                        font-size: 16px;
                        color: #555;
                        margin: 10px 0;
                }

                .label {
                        font-weight: bold;
                        color: #222;
                }
        </style>
</head>

<body>
        <div class="card">
                <h1>Thong tin sinh vien</h1>
                <p><span class="label">Ho va ten:</span> Tran Thi Thu Ha </p>
                <p><span class="label">MSSV:</span> K225480106009 </p>
                <p><span class="label">Lop:</span> K58KTP.01 </p>
        </div>
</body>
</html>
```

## 3. Tạo file docker-compose.yml
- Sử dụng lệnh ```nano docker-compose.yaml``` để tạo file docker-compose.yml
- Viết khối lệnh dưới đây vào trong file docker-compose.yml
```
version: "3.8"

services:
  nodered:
    image: nodered/node-red
    container_name: nodered
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data

  nginx:
    image: nginx
    container_name: nginx
    ports:
      - "80:80"
    volumes:
      - ./myweb:/myweb
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - nodered
```
- Gõ lệnh ```ls``` hoặc ```cat docker-compose.yml``` để kiểm tra file docker-compose.yml vừa tạo
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a6c0d822-f93f-4a14-a3aa-2a01d324c5a4" />

## 4. Cấu hình cho nginx (nginx/nginx.conf)
- Cấu hình cho nginx như sau: 
```
events {}

http {
        server {
                listen 80;
                server_name thuha_app.local;

                location / {
                        root /myweb;
                        index index.html;
                }

                location /api/ {
                        proxy_pass http://nodered:1880/;
                        proxy_set_header Host $host;
                        proxy_set_header X-Real-IP $remote_addr;
                }
        }
}
```
<img width="1917" height="930" alt="image" src="https://github.com/user-attachments/assets/f426895f-de0f-4eed-b87c-69d4b3670032" />

## 5. Khởi chạy hệ thống
### Chạy lệnh sau để chạy docker:
```
docker-compose up -d
```
### Sau khi chạy docker lần đầu, Node-RED sẽ tự tạo ra file settings.js
### Cấu hình lại file ```settings.js``` để bắt đăng nhập
- Mở file cấu hình của nodered, tìm đoạn code dưới và bỏ hết dấu // ở đầu.
- Có thể đặt lại password theo ý của mình
```
 adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "$2a$08$zZWtXTja0fB1pzD4sHCMyOCMYz2Z6dNbM6tl8sJogENOMcxWV9DN.",
            permissions: "*"
        }]
    },
```
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/5f822203-347c-4bbc-a0d1-04acc4c3ca04" />

### Sau khi cấu hình lại file settings.js, cần restart lại để áp dụng các thay đổi
```
docker-compose restart
```

## 6. Kiểm tra lại kết quả
- Truy cập Node-RED: **http://172.20.10.2:1880**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7168fe19-ecb4-473b-ad3c-f4dbf831e36c" />

- Truy cập web: **http://172.20.10.2:80**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/29b9a322-ebbc-431d-8ed2-d8f9d9093e81" />



