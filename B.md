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
- Chọn Download để tải file iso Ubuntu 24.04.4 LTS về máy 
<img width="1919" height="1027" alt="image" src="https://github.com/user-attachments/assets/82f51722-db46-4156-85a8-318534309091" />

### 2. Sử dụng VM_Ware để giả lập
#### Tạo máy ảo
- Mở VM_Ware, vào file chọn Create New Virtual Machine
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7761d24f-bd85-4030-88e5-4b4eaca4fc53" />

- Chọn Installer disc image file (iso), sau đó chọn file iso đã tải trước đó
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ecb31fc6-9460-4f6c-bb18-52ba51c8ea0b" />

- Chọn vị trí để lưu máy ảo
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/76dfe1da-0e9e-4fa2-b009-707af498ec03" />

- Cấu hình cho máy ảo
<img width="1913" height="1022" alt="image" src="https://github.com/user-attachments/assets/4ac39257-5000-47c6-bead-3ed7342334ed" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/25af85f2-20c2-4fa3-a3aa-434829ad04dc" />

<img width="1588" height="805" alt="image" src="https://github.com/user-attachments/assets/f964661b-d818-4d79-af11-7f06dd797ee7" />

<img width="1919" height="1032" alt="image" src="https://github.com/user-attachments/assets/527cc548-54d2-4a1b-999a-a6d69cf4245b" />

### 3. Cấu hình mạng SSH





