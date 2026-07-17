---
title : "Kiểm tra AppSync GraphQL API"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

### Mục tiêu
Xác minh AWS AppSync API đã được tạo để frontend gọi GraphQL query, mutation và subscription.

### Kiểm tra API
1. Mở **AWS AppSync** console.
2. Chọn **APIs**.
3. Kiểm tra API `TaskManagerAPI-dev`.

![Hình](images/workshop/appsync.png)

Trong ảnh, AppSync có 1 API:
* **Name:** `TaskManagerAPI-dev`
* **API type:** GraphQL
* **Primary auth mode:** Amazon Cognito
* **HTTP endpoint:** endpoint HTTPS cho query/mutation
* **Realtime endpoint:** endpoint WebSocket cho subscription

### Vai trò của AppSync
AppSync là lớp API trung tâm của TaskManager:
* **Query:** lấy danh sách board, task, user hoặc notification.
* **Mutation:** tạo/cập nhật/xóa task, board hoặc user data.
* **Subscription:** nhận cập nhật real-time khi task thay đổi.
* **Authorization:** xác thực request bằng Cognito JWT token.

### Kết luận
GraphQL API đã được tạo thành công và kết nối đúng với Cognito làm phương thức xác thực chính.