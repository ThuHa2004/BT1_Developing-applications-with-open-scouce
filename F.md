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
