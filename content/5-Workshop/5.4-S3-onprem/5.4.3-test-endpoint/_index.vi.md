---
title : "Kiểm tra Lambda functions"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

### Mục tiêu
Xác minh các Lambda functions xử lý backend logic của TaskManager đã được triển khai.

### Kiểm tra danh sách function
1. Mở **AWS Lambda** console.
2. Chọn **Functions**.
3. Kiểm tra các function thuộc project TaskManager.

![Hình](images/workshop/lambda.png)

Các function đã triển khai:
* `userManager-dev`: xử lý thông tin user và profile.
* `boardManager-dev`: xử lý board, thành viên board và quyền truy cập.
* `taskProcessor-dev`: xử lý task lifecycle như tạo, cập nhật, chuyển trạng thái.
* `streamProcessor-dev`: xử lý event/stream phục vụ cập nhật hoặc ghi log.

### Runtime và package
Trong ảnh, các function sử dụng:
* **Package type:** Zip
* **Runtime:** Node.js 18.x
* **Type:** Standard

### Vai trò trong kiến trúc
Lambda nhận request từ AppSync resolver, kiểm tra logic nghiệp vụ, sau đó đọc/ghi dữ liệu vào DynamoDB. Cách này giúp backend không cần quản lý server và có thể scale theo số lượng request.

### Kết luận
Các Lambda functions cần thiết đã tồn tại và sẵn sàng kết nối với AppSync/DynamoDB.