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
Tạo file ```./myweb/index.html```
Sử dụng lệnh ```nano ~/myapp/myweb/index.html``` để viết nội dung file.
Sau khi viết xong nội dung file, nhấn phím ```Ctrl+O``` để lưu nội dung file, và tổ hợp phím ```Ctrl+X``` để thoát
```
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <title>Thông tin cá nhân</title>

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
            box-shadow: 0 10px 25px rgba(0,0,0,0.2);
            text-align: center;
            width: 350px;
            transition: 0.3s;
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0,0,0,0.3);
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

        <h1>Thông tin sinh viên</h1>

        <p><span class="label">Họ và tên:</span> Trần Thị Thu Hà</p>
        <p><span class="label">MSV:</span> K225480106009 </p>
        <p><span class="label">Lop:</span> K58KTP.01</p>
    </div>
</body>
</html>
```

## 3. Tạo file docker-compose.yml
- Sử dụng lệnh ```nano ~/myapp/docker-compose.yaml``` để tạo file


