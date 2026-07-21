---
title: "Blog 3"
date: 2024-01-02
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Xây dựng Oracle Database có tính sẵn sàng cao với Amazon FSx for NetApp ONTAP – Những điều em học được từ AWS Architecture Blog

> **Bài viết gốc:** *Building Highly Available Oracle Databases with Amazon FSx for NetApp ONTAP*  
> **Nguồn:** AWS Architecture Blog

## Lý do em chọn bài viết này

Trong quá trình tìm hiểu về các giải pháp lưu trữ và quản trị cơ sở dữ liệu lớn trên đám mây, việc đảm bảo tính sẵn sàng cao (High Availability) cho các hệ thống enterprise như Oracle Database luôn là một thách thức không hề nhỏ. 

Bài viết này thu hút em bởi cách giải quyết bài toán không chỉ bằng một dịch vụ riêng lẻ mà là sự kết hợp nhịp nhàng giữa nhiều dịch vụ AWS khác nhau để tạo nên một kiến trúc tự động hóa, có khả năng phục hồi nhanh chóng trước sự cố. Em lựa chọn nội dung này để tổng hợp lại kiến thức và liên hệ sâu sắc hơn với các dịch vụ lưu trữ hạ tầng mà em đang tiếp cận trong quá trình thực tập.

![Kiến trúc Oracle Database High Availability với Amazon FSx for NetApp ONTAP](/images/blog3/archblog.png)

*Hình 1. Kiến trúc tự động phục hồi cho Oracle Database sử dụng Amazon FSx for NetApp ONTAP. (Nguồn: AWS Architecture Blog)*

## Ấn tượng đầu tiên

Ban đầu, em nghĩ bài viết chỉ đơn thuần hướng dẫn cách cài đặt và cấu hình Oracle Database trên môi trường AWS EC2 thông thường. 

Tuy nhiên, sau khi đi sâu vào nội dung, em nhận ra bài viết tập trung giải quyết bài toán cốt lõi của doanh nghiệp: Làm thế nào để hệ thống duy trì tính liên tục và tự động khắc phục thảm họa (Disaster Recovery) một cách nhanh nhất mà không cần can thiệp thủ công rườm rà, đồng thời đảm bảo an toàn tuyệt đối cho dữ liệu thông qua cơ chế đồng bộ hóa thời gian thực.

## Các thành phần cốt lõi trong kiến trúc

Kiến trúc này sử dụng sự kết hợp tối ưu giữa năng lực tính toán linh hoạt và giải pháp lưu trữ chuyên dụng cao cấp:

### 1. Amazon FSx for NetApp ONTAP (FSxN) Multi-AZ
- Đóng vai trò là lớp lưu trữ trung tâm lưu trữ toàn bộ dữ liệu của Oracle Database.
- Kết nối tới các máy chủ EC2 thông qua giao thức iSCSI chuẩn công nghiệp.
- Tính năng **Synchronous Replication** (sao chép đồng bộ) giữa 2 Availability Zone (AZ1 và AZ2) đảm bảo dữ liệu luôn nhất quán và không bị mất mát kể cả khi một vùng dữ liệu gặp sự cố hoàn toàn.

### 2. EC2 Auto Scaling Group (Min=1, Max=1)
- Đảm bảo luôn có đúng một instance Oracle DB hoạt động. Nếu máy chủ Active ở AZ1 gặp sự cố, Auto Scaling sẽ tự động khởi tạo một instance Replacement ở AZ2.

### 3. AWS Backup & Quy trình tự động hóa (EventBridge + Lambda + SSM)
- **AWS Backup** thực hiện sao lưu định kỳ và sinh ra sự kiện hoàn thành (Completion event).
- **Amazon EventBridge** bắt sự kiện này và kích hoạt **AWS Lambda**.
- **AWS Lambda** cập nhật AMI ID mới nhất vào **AWS Systems Manager (SSM) Parameter Store**.
- Launch Template của Auto Scaling Group sẽ tham chiếu (`resolve:ssm`) đến Parameter Store này để đảm bảo máy chủ thay thế luôn được khởi chạy từ cấu hình và phiên bản bản vá mới nhất của hệ thống.

## Những điều em học được về RTO và RPO

Điều tâm đắc nhất em rút ra được từ bài viết này chính là hiểu rõ hơn về cách tối ưu hóa hai chỉ số cốt lõi trong thiết kế hệ thống tin cậy cao:

- **RPO (Recovery Point Objective - Mục tiêu điểm phục hồi):** Nhờ cơ chế Synchronous Replication Multi-AZ của FSx for NetApp ONTAP, dữ liệu được ghi đồng thời ở cả hai vùng, giúp RPO tiệm cận bằng 0 (không lo mất mát dữ liệu khi chuyển vùng).
- **RTO (Recovery Time Objective - Mục tiêu thời gian phục hồi):** Chuỗi tự động hóa từ EventBridge -> Lambda -> SSM kết hợp với Auto Scaling Group giúp giảm thiểu tối đa thời gian gián đoạn. Thay vì phải cấu hình lại thủ công từ đầu, máy chủ thay thế tại AZ2 tự động khởi chạy, liên kết lại với phân vùng lưu trữ qua iSCSI và đưa cơ sở dữ liệu hoạt động trở lại trong thời gian ngắn nhất.

## Kết luận

Bài viết *Building Highly Available Oracle Databases with Amazon FSx for NetApp ONTAP* mang đến một góc nhìn thực tế và vô cùng giá trị về kỹ thuật thiết kế kiến trúc đám mây chuẩn chỉnh. Nó chứng minh rằng tính sẵn sàng cao không chỉ là câu chuyện backup dữ liệu thông thường, mà là sự phối hợp chặt chẽ giữa lưu trữ đồng bộ, tự động hóa hạ tầng và quản lý cấu hình tập trung.

**Link bài viết tham khảo:** [Building Highly Available Oracle Databases with Amazon FSx for NetApp ONTAP](https://aws.amazon.com/vi/blogs/architecture/building-highly-available-oracle-databases-with-amazon-fsx-for-netapp-ontap/)