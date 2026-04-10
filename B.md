# Cài đặt Ubuntu + Docker
# YÊU CẦU: 
## 1. Cài đặt hệ điều hành Ubuntu 24.04.4 LTS
- Sử dụng một trong các công cụ để giả lập: HyperV (có sẵn của windows), VirutualBox (Miễn phí), VM_Ware (Bản quyền).
- Download file iso để cài đặt.
- Cấu hình mạng trong Ubuntu (và công cụ để giả lập) để cho phép truy cập SSH vào Ubuntu từ cmd của windows.
## 2. Tìm hiểu các lệnh cơ bản của Ubuntu
Các lệnh cần tìm hiểu: 
- Liệt kê các file trong thư mục: ls
- Tạo thư mục: mkdir nameFolder
- Chuyển thư mục làm việc: cd path
- Copy file: cp file_nguồn path/file_đích
- Thay đổi quyền thao tác file: sudo chmod xxx filename
- Edit file: sudo nano tenfile: CTRL+o - lưu nội dung file sau khi edit; CTRL+x - thoát edit file
- Xem ip của máy Ubuntu: ip -4addr
## 3. Cài đặt docker cho Ubuntu
## 4. Kiểm tra phiên bản docker vừa cài đặt, kiểm tra phiên bản docker compose
## 5. Cấu hình để docker chạy được mà không cần tiền tố sudo
## 6. Tìm hiểu tập lệnh của docker và docker compose
## 7. Đảm bảo tường lửa trên Ubuntu đã cho phép cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)
-------------------
# BÀI LÀM
## I. Cài đặt hệ điều hành Unbuntu 24.04.4 LTS
### 1. Truy cập vào: https://ubuntu.com/download/desktop để download
- Chọn Download để tải Ubuntu 24.04.4 LTS về máy 
<img width="1919" height="1027" alt="image" src="https://github.com/user-attachments/assets/82f51722-db46-4156-85a8-318534309091" />

### 2. Sử dụng VM_Ware để giả lập
- Mở VM_Ware và 


