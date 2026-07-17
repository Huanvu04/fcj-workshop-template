---
title : "Cấu hình authentication, API và backend"
date : 2024-01-01 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

### Tổng quan
Phần này kiểm tra các lớp chính phía sau frontend TaskManager:

* **Amazon Cognito**: quản lý người dùng và phát hành token đăng nhập.
* **AWS AppSync**: cung cấp GraphQL API cho frontend.
* **AWS Lambda**: xử lý logic nghiệp vụ cho user, board, task và stream processing.
* **Amazon CloudWatch**: ghi log để kiểm tra lỗi và hoạt động của backend.

### Luồng xác thực và API
1. Người dùng đăng nhập vào frontend.
2. Cognito xác thực và trả về JWT token.
3. Frontend gửi GraphQL request đến AppSync kèm token.
4. AppSync xác thực token và gọi Lambda resolver.
5. Lambda đọc/ghi dữ liệu trong DynamoDB.
6. CloudWatch ghi log phục vụ troubleshooting.

### Nội dung
* Kiểm tra Cognito User Pool
* Kiểm tra AppSync GraphQL API
* Kiểm tra Lambda functions
* Kiểm tra CloudWatch logs



