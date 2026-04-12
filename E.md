# E. Triển khai (level test) ứng dụng
# YÊU CẦU:
1. Chuyển vào trong thư mục ~/myapp
2. Gõ lệnh để docker compose chạy: sẽ run tất cả các service khai báo trong file docker-compose.yml
```
Lợi ích: Chỉ cần docker-compose up -d là toàn bộ hệ thống (Web + Node-RED + Tunnel) tự chạy,
```

3. Kiểm tra các container đang chạy trong docker, nếu có cái nào bị restart cần tìm lỗi rồi edit lại docker-compose.yml
4. Kiểm tra kiểm thử các service đang chạy độc lập thông qua ip và port của nó: ví dụ mở trình duyệt ip_ubuntu:1880 để check nodered đã chạy chưa
5. Sử dụng nodered: kéo nodered http_in , http_response, function : để tạo api get đơn giản (dùng cho /api proxy_pass của nginx)
6. Sửa file ./myweb/index.html : thêm code html+js để sử dụng được api đã khai báo proxy_pass (thực ra là sử dụng nodered http_in hoặc sử dụng service myapi)
--------------
# BÀI LÀM
## 1. Khởi chạy hệ thống 
- Chuyển vào thư mục ```~myapp```
```
cd myapp
```
- Chạy docker compose để run tất cả các service đã khai báo trong file docker-compose.yml
```
docker-compose up -d
```
- Sau khi chạy lệnh này, toàn bộ hệ thống Web, Node-Red, Myapi tự chạy. 

## 2. Kiểm tra các container đang chạy trong docker
Chạy lệnh ```docker-compose ps``` để kiểm tra trạng thái các container trong docker
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b6132289-19f9-4f14-a2ee-30fc2922190f" />

-> Không có service nào bị lỗi

Nếu có lỗi xảy ra, ví dụ cột status hiện ```restarting``` hoặc ```Exited```  thì chạy lệnh xem log của service đó để tìm lỗi.
Ví dụ:
- Lệnh xem log của nodered:
```
docker-compose logs nodered
```
- Sau khi xem lỗi xong mở lại file ```docker-compose.yml``` để fix lỗi

## 3. Kiểm thử các service đang chạy độc lập
- Node-RED: **http://172.20.10.2:1880**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c7cfd34f-6114-487a-9e0e-be2501a28cf2" />

- Web (Nginx): **http://172.20.10.2:80**
<img width="1913" height="1034" alt="image" src="https://github.com/user-attachments/assets/bbbc568e-2c40-4557-bbf4-8fb13c585d8b" />

- Myapi: **http://172.20.10.2:9630**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5058c823-0b37-4cc0-9d8a-b887b67cc992" />

-> Tất cả các service đều chạy được 

## 4. Tạo API đơn giản trên Node-RED
- Truy cập vào giao diện Node-RED cổng 1880, kéo thả các node và viết code đơn giản để tạo API để nginx có thể nói chuyện được với Node-RED
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/318ffa8e-795b-49b8-97fc-1945e7164de7" />

## 5. Sửa file ```index.html``` để gọi API
- Lệnh: ```nano myweb/index.html```
- Thêm đoạn sau vào file ```index.html```
```
<script>
        async function callNodeRed() {
            const resultElement = document.getElementById('api-result');
            resultElement.innerText = "Đang kết nối...";

            try {
                const response = await fetch('/api/test-api');
                
                if (!response.ok) {
                    throw new Error('Không thể kết nối API (Lỗi ' + response.status + ')');
                }

                const data = await response.json();
                
                resultElement.innerText = "Dữ liệu: " + JSON.stringify(data.message || data);
            } catch (error) {
                resultElement.innerText = "Lỗi: " + error.message;
                console.error("API Error:", error);
            }
        }
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/22d16b7e-4b71-4749-81c1-b3fa1dea6743" />


