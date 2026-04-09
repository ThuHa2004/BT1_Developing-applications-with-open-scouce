# A. Đăng ký tên miền xịn cho cá nhân
---------------
# YÊU CẦU: 
  1. Đăng ký Domain xịn
  2. Đăng ký tài khoản cloudflare
  3. Thêm Domain đã đăng ký vào trong cloudflare: Nhập 2 dòng namespace
  4. Nhập 2 dòng namespace của cloudfare vào trong trang quản lý DNS record của tên miền đăng ký (vd trên mắt bão)
----------------
# BÀI LÀM
## 1. Đăng ký Domain xịn
### Truy cập vào website của nhà cung cấp Domain (Ví dụ: mắt bão, iNET,...)
- Link website đăng ký: **https://www.matbao.net/ten-mien/ten-mien-mien-phi.html**
### Nhập tên miền để kiểm tra xem đã tồn tại hay chưa, nếu đã tồn tại rồi thì phải đổi tên miền khác
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0362985c-77b4-452f-9b48-e2e9dfa5cfe6" />

### Xác thực tài khoản 
<img width="1920" height="1080" alt="A_2" src="https://github.com/user-attachments/assets/8efcea84-c73b-48a6-be77-76cba21ad813" />

<img width="1920" height="1080" alt="A_3" src="https://github.com/user-attachments/assets/bc54b49b-26e8-41ed-8c42-b25ede57acdc" />

<img width="1920" height="1080" alt="A6" src="https://github.com/user-attachments/assets/52496307-8bf3-4c1b-90b5-114cb9587ec5" />

### Sau khi xác thực thành công, tên miền sẽ được đưa vào hàng đợi đăng ký, kiểm tra đủ điều kiện sẽ được kích hoạt
--------------------
## 2. Đăng ký tài khoản Cloudfare
### Truy cập vào cloudfare -> Nhấn Sign up -> Nhập email và password -> Tick Verify -> Nhấn create Account 
<img width="1914" height="1028" alt="image" src="https://github.com/user-attachments/assets/5230059e-1f43-4e78-88e0-e3aed6be47c6" />

--------------------
## 3. Thêm Domain đã đăng ký vào cloudfare
### Sau khi đã tạo xong tài khoản, đăng nhập vào và chọn Domains -> add a site -> Nhập domain đã đăng ký vào -> continue
<img width="1919" height="1023" alt="image" src="https://github.com/user-attachments/assets/57c708d8-05a9-4e4f-aae6-7827c91fc627" />

### Chọn Free plan
<img width="1917" height="985" alt="image" src="https://github.com/user-attachments/assets/36ffec75-6e11-4393-bf69-21184f983910" />

### Nhấn continue to activation
<img width="1912" height="978" alt="image" src="https://github.com/user-attachments/assets/d12b2ac2-8481-4421-b3c7-5e3af6ea9798" />

### Sau khi nhấn continue to activation sẽ chuyển sang trang có chứa 2 dòng namespace, copy 2 dòng đó lại.

---------------------
## 4. Nhập 2 dòng namespace của cloudflare vào trong trang quản lý DNS record của tên miền đăng ký
### Đăng nhập vào trang mắt bão *https://manage.matbao.net/domains*
### Sau khi đăng nhập vào, chọn tên miền -> quản lý tên miền
<img width="1919" height="1023" alt="image" src="https://github.com/user-attachments/assets/20179099-2402-43ba-bbba-10f0f89b1764" />

### Chọn quản lý chi tiết dịch vụ
<img width="1919" height="803" alt="Screenshot 2026-04-09 162435" src="https://github.com/user-attachments/assets/9fd012d6-67fb-4c5d-972f-1194f513da30" />

### Trong Servername chọn "Sử dụng Name Server tùy chỉnh" sau đó nhập 2 namespace của cloudfare vào -> Nhấn lưu thay đổi
<img width="1920" height="1080" alt="A4-2ok" src="https://github.com/user-attachments/assets/9b52d05f-c194-4bb1-9bb4-69cd6052097a" />

### Sau khi nhập 2 dòng namespace của cloudflare vào trong trang quản lý DNS record của tên miền đăng ký trên mắt bão, quay trở lại trang cloudfare kiểm tra, nếu status active => Đã kết nối thành công Domain với Cloudfare
<img width="1919" height="969" alt="image" src="https://github.com/user-attachments/assets/f52ee17b-da67-4d17-8fe4-60e5f6a26f82" />





