---
title : "Chuẩn bị môi trường và IAM"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Mục tiêu
Phần này kiểm tra tài khoản AWS, Region, quyền IAM và các điều kiện cần trước khi xác minh hệ thống TaskManager.

### Region sử dụng
Workshop sử dụng Region **Asia Pacific (Singapore) - ap-southeast-1** cho các dịch vụ chính như Cognito, AppSync, Lambda, DynamoDB, S3 và CloudWatch.

### Kiểm tra IAM
1. Mở AWS Management Console.
2. Tìm dịch vụ **IAM**.
3. Kiểm tra dashboard IAM để xác nhận tài khoản đã có cấu hình bảo mật cơ bản.

![Hình](images/workshop/iam.png)

Trong ảnh trên, tài khoản đã có MFA cho root user và không có active access key cho root user. Đây là hai khuyến nghị bảo mật quan trọng khi làm việc với AWS account.

### Quyền cần thiết
Để triển khai và kiểm tra workshop này, IAM user/role cần quyền thao tác với các dịch vụ sau:
* Amazon S3: upload frontend assets.
* Amazon Cognito: xem User Pool và App Client.
* AWS AppSync: xem GraphQL API, endpoint và authorization mode.
* AWS Lambda: xem danh sách function và cấu hình runtime.
* Amazon DynamoDB: xem table, indexes, capacity mode và PITR.
* Amazon CloudWatch: xem log groups và log streams.
* IAM: xem role/policy liên quan đến Lambda, AppSync và deployment.

### Quy ước đặt tên tài nguyên
Các tài nguyên trong workshop dùng hậu tố `dev` để biểu thị môi trường development:
* `taskmanager-frontend-dev-*`
* `taskmanager-users-dev`
* `TaskManagerAPI-dev`
* `userManager-dev`
* `boardManager-dev`
* `taskProcessor-dev`
* `streamProcessor-dev`
* `TaskManager-Boards-dev`
* `TaskManager-Tasks-dev`
* `TaskManager-Users-dev`

### Checklist trước khi tiếp tục
* Đăng nhập đúng AWS account.
* Chọn đúng Region `ap-southeast-1`.
* Xác nhận IAM user/role có đủ quyền xem tài nguyên.
* Chuẩn bị frontend build output gồm `index.html`, JavaScript bundle, CSS bundle và static SVG assets.