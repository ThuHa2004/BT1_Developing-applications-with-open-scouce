# D. Phần bonus không bắt buộc
# YÊU CẦU: 
1. Tạo thư mục ./myapi
2. Tạo fille ./myapi/app.py sử dụng Python + Flask để làm gì đó funny
3. Tạo file ./myapi/requirements.txt chứa các thư viện mà app.py sử dụng.
4. Tạo file ./myapi/Dockerfile để khai báo sử dụng Python 3.9 slim
```
 # Sử dụng phiên bản Python nhẹ (alpine) để giảm dung lượng image
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
5. Sửa đổi docker-compose để sử dụng myapp (xem phần tham khảo ở dưới)
6. Sửa đổi nginx/nginx.conf để /api trỏ tới service myapp cổng 9630
--------------
# BÀI LÀM
## Cấu trúc thư mục
```
myapi/
    ├── app.py
    ├── requirements.txt
    └── Dockerfile
```
## 1. Tạo thư mục ```myapi``` nằm cùng cấp với nginx
```
mkdir -p myapi
```
## 2. Tạo file ```myapi/app.py```, ```requirements.txt```, ```Dockerfile```
Sử dụng lệnh các lệnh sau để tạo: 
```
nano app.py
nano requirements.txt
nano Dockerfile
```
###  File ```myapi/app.py```
```
from flask import Flask, jsonify
import random

app = Flask(__name__)

quotes = [
    "Code chạy rồi, đừng chạm vào nó nữa!",
    "Lỗi này không phải tại em, tại Docker đấy.",
    "Hôm nay là một ngày đẹp trời để... fix bug.",
    "Bình tĩnh và CTRL + S.",
    "Nghệ thuật là ánh trăng lừa dối, Code là dòng chữ lừa người."
]

@app.route('/')
def home():
    return "Welcome to my Funny API! Try /joke or /health"

@app.route("/joke")
def joke():
    return jsonify({
        "joke": random.choice(quotes)
    })

@app.route("/health")
def health():
    return jsonify({
        "status": "OK"
    })

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=9630)
```

### Khai báo thư viện sử dụng trong file ```requirements.txt```
```
flask
```

### File ```myapi/Dockerfile``` để khai báo sử dụng Python 3.9 slim
```
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 9630

CMD ["python", "app.py"]
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5e2b59ea-e090-4aff-82bb-fb798506505a" />

## 3. Cấu hình lại file  ```docker-compose.yml``` để sử dụng ```myapp```
Thêm dịch vụ ```myapi```
```
 myapi:
    build: ./myapi
    container_name: myapi
    ports:
      - "9630:9630"
```

## 4. Cấu hình lại file ```nginx/nginx.conf```
Sửa dòng ```proxy_pass``` như dưới đây để chuyển hướng request từ ```api``` sang service ```myapi```
```
location /api/ {
  proxy_pass http://myapi:9630/;
}
```
## 5. Sau khi cấu hình lại file ```docker-compose.yml``` và ```nginx/nginx.conf``` xong thì chạy lại lệnh dưới đây để build lại thư mục ```myapi```
```
docker-compose up -d --build
```
### Sau khi chạy xong hoàn tất, truy cập vào các địa chỉ sau để kiểm tra:

```
http://172.20.10.2:9630
http://172.20.10.2/api/joke
http://172.20.10.2:9630/joke

```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/01350ee9-d76d-45c3-91c6-51b47941d910" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a99b9c95-0af9-40ba-8d5f-b57398e6787d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/96668b5d-eec2-481a-aeb3-035581db86e2" />






