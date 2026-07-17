---
title : "Tổng quan Workshop"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

### Mục tiêu
Trong workshop này, bạn sẽ kiểm tra các thành phần chính của hệ thống **Task Management System** đã triển khai trên AWS:

* Frontend static assets được upload lên Amazon S3.
* Người dùng được quản lý bằng Amazon Cognito User Pool.
* API chính sử dụng AWS AppSync GraphQL API với Cognito làm primary auth mode.
* Backend logic chạy bằng AWS Lambda.
* Dữ liệu board, task, user, notification và activity log được lưu trong Amazon DynamoDB.
* DynamoDB bật Point-in-Time Recovery (PITR) để bảo vệ dữ liệu trước thao tác ghi/xóa nhầm.
* Amazon CloudWatch thu thập log từ các Lambda functions và các thành phần vận hành.

### Kiến trúc tổng quan
![Hình](/images/workshop/tongquan.png)


### Luồng hoạt động chính
1. Developer build frontend và upload static files lên S3 bucket `taskmanager-frontend-dev-*`.
2. Người dùng đăng nhập qua Cognito User Pool `taskmanager-users-dev`.
3. Frontend gửi GraphQL query/mutation/subscription đến AppSync API `TaskManagerAPI-dev`.
4. AppSync gọi Lambda functions như `userManager-dev`, `boardManager-dev`, `taskProcessor-dev` và `streamProcessor-dev`.
5. Lambda đọc/ghi dữ liệu vào các bảng DynamoDB `TaskManager-Boards-dev`, `TaskManager-Tasks-dev`, `TaskManager-Users-dev`, `TaskManager-Notifications-dev` và `TaskManager-ActivityLogs-dev`.
6. CloudWatch ghi log để hỗ trợ troubleshooting và quan sát vận hành.

### Kết quả mong đợi
Sau khi hoàn thành workshop, bạn có thể giải thích được từng dịch vụ AWS trong hệ thống TaskManager, biết cách kiểm tra tài nguyên đã triển khai, xác minh dữ liệu DynamoDB, kiểm tra PITR và đọc log vận hành trong CloudWatch.
