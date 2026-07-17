---
title : "Dọn dẹp tài nguyên"
date : 2026-07-17
weight : 6
chapter : false
pre : " <b> 5.6 </b> "
---

### Tổng kết Workshop
Chúc mừng bạn đã hoàn thành workshop TaskManager. Trong workshop này, bạn đã xác thực:
* Bảng điều khiển IAM và các khuyến nghị bảo mật cơ bản.
* S3 bucket chứa bản build frontend.
* Cognito User Pool `taskmanager-users-dev`.
* AppSync GraphQL API `TaskManagerAPI-dev`.
* Các hàm Lambda cho backend.
* Các bảng DynamoDB, Global Secondary Index và PITR.
* Nhóm log CloudWatch cho backend Lambda.

### Dọn dẹp tài nguyên
Nếu bạn không còn cần môi trường TaskManager, hãy dọn dẹp các tài nguyên để tránh phát sinh chi phí thêm.

1. Xóa các đối tượng frontend trong S3 bucket `taskmanager-frontend-dev-*`.
![Hình](/images/workshop/delete-s3-objects.png)

2. Xóa AppSync API `TaskManagerAPI-dev`.
![Hình](/images/workshop/delete-appsync-api.png)

3. Xóa các hàm Lambda `userManager-dev`, `boardManager-dev`, `taskProcessor-dev`, và `streamProcessor-dev`.
![Hình](/images/workshop/delete-lambda-functions.png)

4. Xóa các bảng DynamoDB `TaskManager-*` nếu dữ liệu không còn cần thiết.
![Hình](/images/workshop/delete-dynamodb-tables.png)

5. Xóa Cognito User Pool `taskmanager-users-dev`.
![Hình](/images/workshop/delete-cognito-user-pool.png)

6. Xóa các nhóm log CloudWatch `/aws/lambda/*Manager-dev`, `/aws/lambda/taskProcessor-dev`, và `/aws/lambda/streamProcessor-dev` nếu không còn cần nhật ký kiểm toán.
![Hình](/images/workshop/delete-cloudwatch-log-groups.png)

7. Xóa các vai trò (roles) hoặc chính sách (policies) IAM được tạo riêng cho dự án này nếu không còn sử dụng.
![Hình](/images/workshop/delete-iam-roles-policies.png)

{{% notice warning %}}
 Trước khi xóa các bảng DynamoDB hoặc Cognito User Pool, hãy đảm bảo rằng dữ liệu không còn cần thiết. PITR chỉ hỗ trợ trong cửa sổ khôi phục đã cấu hình khi việc khôi phục được xử lý chính xác.
{{% /notice %}}

### Kết luận
TaskManager là một ví dụ hoàn chỉnh về ứng dụng serverless trên AWS: lưu trữ frontend tĩnh, xác thực được quản lý, GraphQL API, điện toán Lambda, cơ sở dữ liệu DynamoDB và khả năng quan sát bằng CloudWatch.