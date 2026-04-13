# G. Câu hỏi về bài làm?
## 1. Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?
Dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED vì: 
  - Khả năng điều hướng (Routing): Nginx đóng vai trò là cổng giao tiếp duy nhất (API Gateway). Khi hệ thống mở rộng, Nginx có thể phân luồng request dựa trên URL (ví dụ: /red vào Node-RED, /api vào Flask) mà chỉ cần một đường hầm duy nhất.
  - Tối ưu hiệu suất: Nginx được thiết kế để xử lý và phân phối các file tĩnh (HTML, CSS, JS) với tốc độ cao hơn rất nhiều so với Node-RED, giúp giảm tải cho application server.
  - Bảo mật và Quản lý luồng: Cung cấp khả năng cấu hình các lớp bảo mật bổ sung (như giới hạn tốc độ - rate limiting, chặn IP) và xử lý SSL/TLS nội bộ trước khi request đến được các service nhạy cảm.
    
## 3. Sự khác biệt giữa việc Mount file và Mount thư mục trong Docker là gì?
Cả hai đều là cơ chế Bind Mount nhưng sự tác động khác nhau: 
  - Mount thư mục (/host/dir:/container/dir): Ánh xạ toàn bộ nội dung của một thư mục từ máy host vào container. Bất kỳ thay đổi (thêm, sửa, xóa file) ở host hay container đều được đồng bộ hai chiều lập tức. Thường dùng cho thư mục chứa mã nguồn (source code) hoặc lưu trữ dữ liệu (database volumes).
  - Mount file (/host/file.conf:/container/file.conf): Chỉ ánh xạ duy nhất một tệp tin cụ thể. Phương pháp này thường được dùng để ghi đè hoặc cung cấp tệp tin cấu hình (như nginx.conf) vào container mà không làm mất đi các tệp tin hệ thống khác nằm cùng thư mục bên trong container.

## 4. Nếu thay đổi file index.html ở máy Ubuntu, nội dung trên web có thay đổi ngay không? Tại sao?
Nếu thay đổi file ```index.html``` ở máy ubuntu thì nội dung trên web có thay đổi. Vì: Khi sử dụng Bind Mount để ánh xạ thư mục chứa index.html vào container, Docker không tạo ra một bản sao của file này. Thay vào đó, nó tạo một liên kết trực tiếp đến Inode của file trên hệ thống tệp của máy Ubuntu (host). Do Nginx phục vụ trực tiếp từ liên kết này, mọi thao tác "Lưu" (Save) trên host sẽ ngay lập tức phản ánh lên web mà không cần khởi động lại container.

## 5. docker-compose.yml khai báo các services có phần restart: always hoặc restart: unless-stopped : chúng để làm gì?
Đây là các chính sách khởi động lại (Restart Policies) giúp duy trì tính sẵn sàng (High Availability) của dịch vụ:
  - ```restart: always```: Container sẽ luôn tự động khởi động lại nếu bị lỗi crash, hoặc khi Docker Daemon (service Docker trên host) khởi động lại, bất chấp trạng thái trước đó.
  - ```restart: unless-stopped```: Tương tự như ```always```, nhưng thông minh hơn: Nếu người quản trị chủ động dừng container bằng lệnh ```docker stop```, hệ thống sẽ ghi nhớ trạng thái này và không tự động bật lại container đó trong lần khởi động tiếp theo của server.

## 6. Cách khai báo để tất cả các services đều dùng chung 1 network? lợi ích của việc khai báo này là gì? Sửa đổi file docker-compose để tất cả các service đều dùng chung 1 network.
Khai báo để tất cả các services dùng chung một networks: 
```
version: '3.8'

services:
  nginx:
    # ...
    networks:
      - my_app_network

  nodered:
    # ...
    networks:
      - my_app_network

networks:
  my_app_network:
    driver: bridge
```
Lợi ích: Các container có thể gọi nhau thông qua tên service (ví dụ: http://nodered:1880) thay vì phải dùng địa chỉ IP tĩnh, giúp hệ thống linh hoạt và không bị lỗi khi IP thay đổi. Tạo ra một mạng cục bộ (LAN) riêng biệt, ngăn chặn các container không liên quan can thiệp vào luồng dữ liệu của ứng dụng.

## 7. Tìm cách đưa Cloudflare Token vào trong file .env rồi sau đó thêm .env vào file .gitignore trước khi push code lên github. Tại sao nói đây là điều quan trọng về bảo mật mã nguồn?
- Cách thực hiện: Tạo file .env chứa ```CLOUDFLARE_TOKEN=xxx```, gọi biến này trong file compose bằng cú pháp ```${CLOUDFLARE_TOKEN}```, và khai báo tên file .env vào trong .gitignore
- Tầm quan trọng về bảo mật: Token là mã định danh xác thực để thiết lập đường hầm vào thẳng mạng nội bộ của máy chủ, vượt qua mọi lớp Firewall. Nếu hard-code token trực tiếp vào file cấu hình và đẩy lên GitHub (đặc biệt là Public Repository), mã nguồn có nguy cơ bị rò rỉ (Credential Leakage). Tin tặc có thể dùng token này để truy cập trái phép vào server. Việc dùng .env kết hợp .gitignore đảm bảo token nhạy cảm chỉ tồn tại cục bộ trên máy chủ triển khai.
  
## 8. Tại sao chúng ta nên thêm hậu tố :ro khi mount file cấu hình Nginx?
- Hậu tố ```:ro``` có nghĩa là Read-Only (Chỉ đọc). Đảm bảo nguyên tắc "đặc quyền tối thiểu" (Least Privilege). Container Nginx chỉ được phép đọc cấu hình để chạy, nhưng không có quyền ghi hay sửa đổi file này trên máy host. Giả sử container bị tấn công chiếm quyền, kẻ gian cũng không thể ghi đè mã độc vào file cấu hình gốc trên máy chủ Ubuntu
  
## 9. Khi dùng Cloudflare Tunnel: có cần thiết phải mở cổng cho các service nữa không?
Khi dùng cloudeflare Tunnel không cần thiết phải mở cổng cho các service. Vì: 
- Cloudflare hoạt động theo nguyên lý: Cloudflare Tunnel sử dụng một daemon (cloudflared) để tạo kết nối chiều ra (outbound) từ máy chủ nội bộ tới mạng lưới của Cloudflare. Traffic từ Internet sẽ đi qua Cloudflare, vào đường hầm này và được chuyển tiếp đến mạng nội bộ của Docker.
- Bảo mật: Nhờ cơ chế này mà không cần mở bất kỳ cổng inbound nào (như 80 hay 443) trên Firewall của máy chủ host (ports: - "80:80"). Điều này giúp ẩn hoàn toàn địa chỉ IP Public của máy chủ, bảo vệ hệ thống khỏi các cuộc quét cổng (Port scanning) và tấn công DDoS trực tiếp. Chỉ cần các dịch vụ nội bộ kết nối cùng chung mạng với cloudflared là đủ.


