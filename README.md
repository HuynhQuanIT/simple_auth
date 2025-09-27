# simple_auth

Ví dụ xác thực đơn giản với Node.js, Express và MongoDB sử dụng cookie.

## Mô tả

- Đăng nhập bằng username/password, server tạo cookie và lưu session vào MongoDB.
- Truy cập các route bảo vệ cần có cookie hợp lệ.
- Đăng xuất sẽ xóa session và cookie.

## Hướng dẫn sử dụng

### 1. Cài đặt

```sh
npm install
```

### 2. Chạy server

```sh
node simple_auth/cookie_auth.js
```

Server mặc định chạy ở: [http://localhost:3001](http://localhost:3001)

### 3. Test API với Postman


#### Các chức năng:
- Truy cập `http://localhost:3000/`  
![Welcome! Visit first public resource.](img\1.png)
![Welcome! Visit first public resource.](img\2.png)
- Truy cập `http://localhost:3000/public`: Trang public, không cần đăng nhập.
![Welcome! Visit second public resource.](img\8.png)
- Truy cập `http://localhost:3000/secure`: Yêu cầu xác thực Basic Auth.
  - Sử dụng header `Authorization: Basic <base64(username:password)>`.
  - Username mặc định: `admin`, Password: `12345`.
  - Nếu xác thực thành công sẽ nhận được thông báo truy cập thành công.
![You have accessed a protected resource](img\7.png)
## Test lỗi

## Các lỗi được xử lý trong code

### basic_auth.js
- Truy cập `/secure` không có header Authorization:
  - Trả về: `401 Authentication required.`
![Authentication required.](img\9.png)

- Truy cập `/secure` với sai username/password:
  - Trả về: `403 Access denied.`
![Access denied.](img\10.png)


### Chạy xác thực bằng cookie
```powershell
node cookie_auth.js
```

#### Các chức năng:
- Đăng nhập: Gửi POST tới `http://localhost:3001/login` với JSON body `{ "username": "admin", "password": "12345" }`.
  - Nếu thành công sẽ nhận được cookie xác thực.
![Logged in!](img\3.png)
![MongoDB cookie](img\4.png)

- Đăng nhập sai username/password:
  - Trả về: `401 Invalid credentials`
![Invalid credentials](img\11.png)

##
- Truy cập profile: Gửi GET tới `http://localhost:3001/profile` kèm cookie `auth_cookie_token`.
  - Nếu cookie hợp lệ sẽ nhận được thông tin người dùng.
![Welcome user 1, your cookie is valid.](img\5.png)

- Truy cập `/profile` không có cookie hoặc cookie không hợp lệ/hết hạn:
  - Trả về: `401 No cookie found` hoặc `401 Invalid or expired cookie`
  ![No cookie found](img\12.png)

  ![Invalid or expired cookie](img\13.png)

##
- Đăng xuất: Gửi POST tới `http://localhost:3001/logout` để xóa cookie xác thực.
![Logged out.](img\6.png)
![Not cookie MongoDB](img\14.png)

Các lỗi khác sẽ được trả về theo logic xử lý trong từng route.