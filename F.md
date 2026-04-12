# F. Gỡ lỗi
# Yêu cầu:
1. Nếu có lỗi xẩy ra trong quá trình triển khai docker compose up -d
```
Kiểm tra nhanh: docker compose ps giúp biết container nào đang chạy xem log, ví dụ: docker logs mynginx docker logs myapi
```
2. Thêm healthcheck cho myapi trong file docker-compose.yml
```
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:9630"]
```
3. Giới hạn resource cho một service: (tránh việc 1 service chiếm quá nhiều ram)
```
deploy:
  resources:
    limits:
      memory: 512M
```
Sử dụng lệnh: docker compose stats để quan sát lượng ram sử dụng bởi mỗi service.

----------------------------
# Bài làm
## 1. Kiểm tra trạng thái và xem Log
- Khi hệ thống không chạy theo ý muốn, bị lỗi -> Kiểm tra danh sách container xem lỗi xảy ra ở container nào bằng lệnh:
```
docker-compose ps
```
<img width="1431" height="394" alt="image" src="https://github.com/user-attachments/assets/36ecb037-1fff-4ec3-bb17-7c75dc15e628" />

- Nếu cột ```status``` hiện ```Exit``` hoặc ```Restarting``` -> service đang bị lỗi 
- Xem Log (nhật ký vận hành) để tìm lỗi xuất hiện ở đâu để fix. Lệnh:
```
docker compose logs [tên_service]
```
Ví dụ:
```
docker logs nginx
docker logs nodered
docker logs myapi
```
<img width="1917" height="630" alt="image" src="https://github.com/user-attachments/assets/43decaeb-c40b-4f89-93df-82c11c218858" />

## 2. Thêm Healthcheck cho ```myapi```
- Healthcheck giúp Docker tự động biết khi nào service thực sự sẵn sàng làm việc.
- Mở file ```docker-compose.yml``` sửa lại phần ```myapi```:
```
  myapi:
    build: ./myapi
    container_name: myapi
    ports:
      - "9630:9630"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9630"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_speriod: 10s
```
<img width="1081" height="730" alt="image" src="https://github.com/user-attachments/assets/4b626f09-99e7-4b0d-9325-5dd0b64d49ba" />

## 3. Giới hạn Resource và theo dõi chỉ số
- Nếu một service bị lỗi "rò rỉ bộ nhớ", nó có thể nuốt hết RAM của máy ảo Ubuntu và làm treo hệ thống. Vì vậy cần cấu hình giới hạn trong file ```docker-compose.yml```
- Trong file ```docker-compose.yml``` thêm đoạn sau vào phần ```myapi``` để giới hạn bộ nhớ sử dụng của ```myapi```:
```
myapi:
 deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c4b8a4f4-d56f-45df-9511-07b36a04f4b2" />

### Để quan sát lượng Ram sử dụng của mỗi service. Chạy lệnh:
```
docker compose stats
```
Lệnh này sẽ hiển thị một bảng cập nhật liên tục các thông số: 
<img width="1153" height="195" alt="image" src="https://github.com/user-attachments/assets/8046f39b-4b38-4b7e-8381-b6ea900fe9f9" />

- Cột LIMIT của ```nginx``` và ```nodered``` hiện ```2.778GiB``` -> 2 service này được dùng toàn bộ RAM của máy ảo Ubuntu.
- Cột LIMIT của ```myapi``` bị khóa ở mức ```512MiB```. 

## 4. Một số các lỗi thường gặp và cách xử lý 

| Tên lỗi | Nguyên nhân | Cách xử lý |
| :--- | :--- | :--- |
| **Restarting(Container liên tục khởi động lại)** | Sai cấu hình file nginx.conf hoặc lỗi cú pháp trong mã nguồn ứng dụng. | Sử dụng lệnh `docker compose logs [tên_service]` để kiểm tra và sửa lại file cấu hình. |
| **Port already allocated** | Trùng port | Đổi port hoặc dùng `docker rm -f` xóa container cũ |
| **404 API Not Found** | Sai đường dẫn proxy_pass trong Nginx hoặc thiếu dấu `/` ở cuối URL khi chuyển hướng. | Kiểm tra lại phần `location` trong `nginx.conf`, đảm bảo đường dẫn khớp với API và thêm `/` nếu cần thiết. |
| **Cannot connect** | Service chưa chạy | Kiểm tra trạng thái bằng `docker compose ps` |
| **(unhealthy)** | Thiếu curl hoặc khởi động chậm | Cài curl vào Dockerfile và tăng `start_period` |
| **Memory Limit** | Vượt ngưỡng 512MB RAM | Giám sát bằng `docker compose stats` |
