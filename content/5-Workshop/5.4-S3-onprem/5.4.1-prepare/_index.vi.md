---
title : "Kiểm tra Cognito User Pool"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

### Mục tiêu
Xác minh Amazon Cognito User Pool đang được dùng làm lớp xác thực cho TaskManager.

### Kiểm tra danh sách User Pool
1. Mở Amazon Cognito console.
2. Chọn **User pools**.
3. Kiểm tra user pool `taskmanager-users-dev`.

![Hình](images/workshop/cognito.png)

Trong ảnh, tài khoản có 1 user pool tên `taskmanager-users-dev` ở Region `ap-southeast-1`.

### Kiểm tra thông tin User Pool
Mở user pool `taskmanager-users-dev` và xem tab **Overview**.

![Hình](images/workshop/cognito-overview.png)

Các thông tin quan trọng:
* User pool name: `taskmanager-users-dev`
* User pool ID: `ap-southeast-1_eYQym9Hoc`
* Estimated number of users: 5
* App client: `taskmanager-client-dev`
* Feature plan: Essentials
* Region: Asia Pacific (Singapore)

### Vai trò trong hệ thống
Cognito chịu trách nhiệm:
* Quản lý user đăng nhập.
* Phát hành JWT token.
* Cung cấp OIDC configuration URL và token signing key URL.
* Làm primary authentication mode cho AppSync API.

### Kết luận
User Pool đã được tạo và sẵn sàng để frontend sử dụng cho chức năng đăng nhập/xác thực.