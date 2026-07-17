---
title : "Kiểm tra CloudWatch logs"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

### Mục tiêu
Kiểm tra các log groups trong Amazon CloudWatch để xác nhận backend có nơi ghi log vận hành.

### Kiểm tra Log groups
1. Mở **Amazon CloudWatch** console.
2. Vào **Logs** > **Log groups**.
3. Tìm các log group liên quan đến TaskManager.

![Hình](/images/workshop/cloudwatch.png)

Các log group quan trọng trong ảnh:
* `/aws/lambda/userManager-dev`
* `/aws/lambda/boardManager-dev`
* `/aws/lambda/taskProcessor-dev`
* `/aws/lambda/streamProcessor-dev`

### Ý nghĩa
CloudWatch giúp:
* Kiểm tra lỗi khi AppSync gọi Lambda.
* Theo dõi request backend.
* Debug logic tạo/cập nhật task.
* Kiểm tra log stream theo từng lần Lambda được invoke.
* Cấu hình retention để tránh chi phí log tăng không kiểm soát.

### Kết luận
CloudWatch đã có log groups cho các Lambda backend. Đây là nền tảng để giám sát, troubleshooting và tối ưu vận hành hệ thống TaskManager.