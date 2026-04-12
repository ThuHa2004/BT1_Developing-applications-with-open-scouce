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
- Cột ```status``` hiện ```Exit``` hoặc ```Restarting``` -> service đang bị lỗi 
- Xem Log (nhật ký vận hành) để tìm lỗi xuất hiện ở đâu để fix. Lệnh:
```
docker logs nginx
docker logs nodered
docker logs myapi
```

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

## 3. Giới hạn Resource và theo dõi chỉ số
- Nếu một service bị lỗi "rò rỉ bộ nhớ", nó có thể nuốt hết RAM của máy ảo Ubuntu và làm treo hệ thống. Vì vậy cần cấu hình giới hạn trong file ```docker-compose.yml```
- Trong file ```docker-compose.yml``` thêm đoạn sau vào phần ```myapi```:
```
myapi:
 deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
```

### Để quan sát lượng Ram sử dụng của mỗi service. Chạy lệnh:
```
docker compose stats
```
Lệnh này sẽ hiển thị một bảng cập nhật liên tục các thông số 


