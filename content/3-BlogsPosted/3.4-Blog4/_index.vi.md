---
title: "Blog 4"
date: 2024-01-03
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Truy vấn S3 Access Logs ngay lập tức: Tạm biệt nỗi ám ảnh dọn dẹp dữ liệu Log – Những điều em học được từ AWS Storage Blog
> **Bài viết gốc:** *Query Amazon S3 access logs instantly with CloudWatch and S3 Tables*  
> **Nguồn:** AWS Storage Blog

## Lý do em chọn bài viết này

Trong quá trình thực tập và mày mò các dịch vụ AWS, em nhận ra rằng việc lưu trữ dữ liệu trên Cloud rất dễ, nhưng quản lý và biết chính xác *"ai đang làm gì với dữ liệu của mình"* lại là một bài toán hoàn toàn khác. Khi đọc các tài liệu về bảo mật, mình thấy mọi người luôn nhắc đến việc phải bật Log. Tuy nhiên, đọc Log như thế nào cho hiệu quả thì mình lại chưa nắm rõ.

Và để tìm hiểu sâu hơn để giải quyết, mình đã đọc được bài viết "Query Amazon S3 access logs instantly with CloudWatch and S3 Tables" trên AWS Storage Blog. Sau khi nghiên cứu, mình thực sự vỡ lẽ ra nhiều điều về cách tối ưu hóa quá trình giám sát dữ liệu, một kỹ năng rất cần thiết khi làm việc trong môi trường thực tế.

## 1. Bài toán rắc rối trước đây: Log thô cực kỳ khó đọc

Trước đây, khi bật tính năng lưu log truy cập trên Amazon S3 (Access Logs), dữ liệu trả về thường ở dạng bán cấu trúc (semi-structured) và nằm rải rác.

Để trả lời được câu hỏi đơn giản như *"Ai đã tải file này vào lúc mấy giờ?"*, chúng ta không thể query trực tiếp ngay được. Thay vào đó, đội ngũ kỹ sư phải cặm cụi xây dựng các hệ thống pipeline (ETL jobs) phức tạp, viết script để thu thập, phân tích (parse) và làm sạch dữ liệu trước khi truy vấn. Quá trình này vừa tốn thời gian, vừa phát sinh chi phí duy trì các tool ETL này.

## 2. Giải pháp mới từ AWS: Đẩy thẳng Log vào CloudWatch và S3 Tables

### Tự động cấu trúc hóa với CloudWatch Logs
Khi đẩy log vào CloudWatch, hệ thống tự động phân tích (parse) các dòng chữ lộn xộn thành các trường dữ liệu có cấu trúc. Mình có thể dùng **CloudWatch Logs Insights** để truy vấn (query) ngay lập tức chỉ trong vài giây, hoặc cài đặt cảnh báo (Alarms) chủ động nếu có truy cập bất thường.

Dưới đây là đoạn mã truy vấn với câu hỏi **"Who is accessing my data?"**:

```sql
stats count(*) as requests by requester, operation
| sort requests desc
```

Chỉ với hai dòng lệnh đơn giản này chạy trên CloudWatch Logs Insights, hệ thống sẽ ngay lập tức trả về danh sách các user/role (requester) đang thao tác (operation) trên dữ liệu, sắp xếp theo số lượng request giảm dần. Điều này cực kỳ hữu ích cho việc audit bảo mật (access reviews) mà không cần phải tốn công viết script lọc log thủ công như trước.

### Lưu trữ tối ưu với S3 Tables
Nếu muốn lưu trữ để phân tích nâng cao, AWS tự động xuất log dưới định dạng tối ưu (như Apache Parquet hoặc JSON) vào **S3 Tables**. Dữ liệu này đã sẵn sàng để truy vấn tốc độ cao (như dùng với Amazon Athena) mà không cần qua bất kỳ bước chuyển đổi nào.

## Những điều em học được

Bài học lớn nhất của mình sau bài viết này là cách AWS giúp tự động hóa hoàn toàn quy trình xử lý Log. Thay vì phải “còng lưng” bảo trì các đường ống dữ liệu (data pipeline) phức tạp chỉ để đọc Log, giờ đây chúng ta có thể tập trung vào việc truy vấn, lấy insights và bảo mật hệ thống ngay lập tức.

Đối với mình, đây là một hướng giải quyết rất thực tế và đáng giá. Mình muốn chia sẻ cho mọi người cùng biết nếu các bạn vướng cùng “nỗi ám ảnh” giống như mình. Hy vọng bài viết này giúp ích được phần nào cho lộ trình học AWS của các bạn.

## Tài liệu tham khảo

**Link bài viết tham khảo:** [Query Amazon S3 access logs instantly with CloudWatch and S3 Tables](https://aws.amazon.com/vi/blogs/storage/query-amazon-s3-access-logs-instantly-with-cloudwatch-and-s3-tables/)