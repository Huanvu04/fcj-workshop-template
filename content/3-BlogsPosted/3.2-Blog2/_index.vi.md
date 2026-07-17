---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# Claude Apps Gateway for AWS – Những điều em học được từ AWS Machine Learning Blog

> **Bài viết gốc:** *Introducing Claude Apps Gateway for AWS*  
> **Tác giả:** Dani Mitchell, Ayan Ray, Sofian Hamiti và Harshetha Narayan  
> **Nguồn:** AWS Machine Learning Blog

## Lý do em chọn bài viết này

Trong quá trình tìm hiểu về các dịch vụ AI trên AWS, em thường xuyên theo dõi AWS Machine Learning Blog để cập nhật những công nghệ mới.

Bài viết về Claude Apps Gateway giúp em nhận ra rằng khi triển khai AI trong doanh nghiệp, việc lựa chọn mô hình AI chỉ là một phần của bài toán. Doanh nghiệp còn cần quản lý tập trung về định danh người dùng, quyền truy cập, chính sách bảo mật, giám sát, định tuyến và kiểm soát chi phí.

Em chọn bài viết này để tổng hợp lại những kiến thức đã học và liên hệ với các dịch vụ AWS mà em được tiếp cận trong quá trình thực tập.

![Kiến trúc Claude Apps Gateway for AWS](/images/blog2/Claude-apps-gateway-architecture.png)

*Hình 1. Kiến trúc Claude Apps Gateway for AWS. (Nguồn: AWS Machine Learning Blog)*

## Ấn tượng đầu tiên

Ban đầu, em nghĩ Claude Apps Gateway chỉ là một công cụ giúp kết nối Claude với AWS.

Sau khi đọc bài viết, em hiểu rằng đây là một **control plane** được triển khai bởi doanh nghiệp nhằm quản lý tập trung Claude Code và Claude Desktop.

Đối với những tổ chức có nhiều lập trình viên, việc cấp phát thông tin xác thực, cấu hình môi trường và theo dõi chi phí cho từng người dùng là một thách thức. Claude Apps Gateway giúp giải quyết vấn đề này bằng một điểm quản lý tập trung.

---

## Claude Apps Gateway cung cấp những gì?

Claude Apps Gateway hoạt động như một lớp trung gian giữa người dùng và dịch vụ AI. Mọi yêu cầu đều đi qua Gateway trước khi được chuyển đến Amazon Bedrock hoặc Claude Platform on AWS.

Các chức năng chính gồm:

### Identity

- Hỗ trợ Enterprise Single Sign-On (OIDC).
- Phiên đăng nhập ngắn hạn.
- Không lưu trữ thông tin xác thực lâu dài trên máy người dùng.

### Policy

- Quản lý tập trung các mô hình được phép sử dụng.
- Kiểm soát quyền sử dụng công cụ.
- Thiết lập cấu hình mặc định.
- Áp dụng chính sách theo từng nhóm người dùng.

### Telemetry

- Thu thập dữ liệu sử dụng thông qua OTLP.
- Có thể tích hợp với Amazon CloudWatch.
- Hỗ trợ Amazon Managed Service for Prometheus.

### Routing

- Điều hướng yêu cầu đến Amazon Bedrock.
- Hoặc Claude Platform on AWS.
- Hỗ trợ triển khai Multi-Region và Multi-Account.

### Spend Caps

- Giới hạn chi tiêu theo ngày.
- Theo tuần.
- Theo tháng.
- Có thể áp dụng cho toàn bộ tổ chức, nhóm hoặc từng người dùng.

Theo em, Claude Apps Gateway có vai trò tương tự Amazon API Gateway nhưng dành cho các ứng dụng AI trong doanh nghiệp.

---

## Triển khai và đăng nhập

Gateway được triển khai dưới dạng stateless container và có thể chạy trên:

- Amazon ECS
- Amazon EKS
- Amazon EC2

Amazon RDS for PostgreSQL được sử dụng để lưu trữ trạng thái đăng nhập tạm thời và bộ đếm giới hạn tốc độ.

Để bảo vệ hệ thống, doanh nghiệp có thể sử dụng:

- Internal Application Load Balancer
- AWS Certificate Manager (ACM)

Thông tin cấu hình được lưu trong file YAML, còn các thông tin nhạy cảm được lưu thông qua biến môi trường.

Khi sử dụng Amazon Bedrock, container có thể xác thực bằng IAM Role mà không cần lưu trữ Access Key.

Lập trình viên chỉ cần thực hiện:

```bash
claude /login
```

Sau khi đăng nhập bằng hệ thống Single Sign-On của doanh nghiệp, mọi yêu cầu gửi đến Claude sẽ được xác thực, ghi nhận và quản lý theo chính sách đã được cấu hình.

---

## Những điều em học được

Điều em học được nhiều nhất từ bài viết là việc triển khai AI trong doanh nghiệp không chỉ dừng lại ở việc lựa chọn mô hình AI phù hợp.

Khi số lượng người dùng tăng lên, doanh nghiệp cần quan tâm đến:

- Quản lý danh tính người dùng.
- Chính sách bảo mật.
- Giám sát và theo dõi hệ thống.
- Quản lý chi phí.
- Điều hướng yêu cầu.
- Quản trị tập trung.

Trong thời gian thực tập AWS, em chủ yếu làm việc với các dịch vụ hạ tầng như Amazon EC2, Amazon S3, AWS Backup và AWS Storage Gateway.

Bài viết này giúp em có thêm góc nhìn rằng một hệ thống AI thực tế còn cần một lớp quản trị trung tâm để kiểm soát việc xác thực, phân quyền, giám sát, định tuyến và tối ưu ngân sách.

Ngoài ra, bài viết cũng giúp em hiểu rõ hai lựa chọn triển khai:

- **Amazon Bedrock** phù hợp khi doanh nghiệp muốn dữ liệu luôn nằm trong phạm vi bảo mật của AWS.
- **Claude Platform on AWS** mang lại trải nghiệm Claude đầy đủ trong khi vẫn tận dụng cơ chế xác thực và thanh toán của AWS.

---

## Kết luận

Đối với em, *Introducing Claude Apps Gateway for AWS* không chỉ đơn thuần là một bài giới thiệu sản phẩm mới.

Bài viết mang đến góc nhìn thực tế về cách doanh nghiệp triển khai AI một cách an toàn, có khả năng quản trị tập trung và kiểm soát tốt các yếu tố như định danh, chính sách, giám sát, định tuyến và chi phí.

Đây là những yếu tố sẽ ngày càng quan trọng khi các ứng dụng AI được đưa vào môi trường sản xuất.

---

## Tài liệu tham khảo

**Link bài viết tham khảo:** [Introducing Claude Apps Gateway for AWS](https://aws.amazon.com/blogs/machine-learning/introducing-claude-apps-gateway-for-aws/)