---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# AWS Architecture Blog | Những điều em học được từ chiến lược Disaster Recovery của S&P Global

> **Bài viết gốc:** *S&P Global’s innovative disaster recovery strategy using Amazon FSx for NetApp ONTAP snapshots*  
> **Tác giả:** AWS Architecture Blog  
> **Nguồn:** AWS Official Blog

## Lý do em chọn bài viết này

Trong quá trình tìm hiểu về AWS, em muốn hiểu rõ hơn về **Disaster Recovery (DR)** thông qua một hệ thống thực tế thay vì chỉ học các khái niệm lý thuyết.

Khi đọc AWS Architecture Blog, em thấy bài viết chia sẻ cách **S&P Global Market Intelligence** xây dựng giải pháp Disaster Recovery cho nền tảng Capital IQ bằng **Amazon FSx for NetApp ONTAP**. Đây là một ví dụ rất thực tế về cách các doanh nghiệp lớn đảm bảo hệ thống luôn sẵn sàng ngay cả khi xảy ra sự cố.

Trước khi đọc bài viết, em nghĩ Disaster Recovery đơn giản chỉ là sao lưu dữ liệu và khôi phục khi gặp lỗi. Sau khi tìm hiểu, em nhận ra rằng một chiến lược DR hoàn chỉnh còn bao gồm thiết kế kiến trúc, cơ chế sao chép dữ liệu, quy trình chuyển đổi hệ thống và tối ưu thời gian phục hồi.

![Kiến trúc Disaster Recovery của S&P Global](images/blog1/sp-global-dr-architecture.png)

*Hình 1. Kiến trúc Disaster Recovery của S&P Global sử dụng Amazon FSx for NetApp ONTAP (Nguồn: AWS Architecture Blog).*

## Tổng quan bài viết

Bài viết giới thiệu cách **S&P Global Market Intelligence** triển khai giải pháp Disaster Recovery cho nền tảng Capital IQ trên AWS.

Mục tiêu của giải pháp là đảm bảo dịch vụ vẫn có thể hoạt động khi Region chính gặp sự cố.

Điểm nổi bật của kiến trúc là hệ thống có thể chuyển sang môi trường Disaster Recovery ở chế độ **read-only** trong **dưới 15 phút**. Khi cần thiết, môi trường này có thể được chuyển sang **read-write** để tiếp tục vận hành như hệ thống chính.

---

## Kiến trúc hệ thống

Giải pháp được xây dựng theo mô hình **Multi-Region**.

- **US-East-1** được sử dụng làm Primary Region.
- **US-West-2** là Disaster Recovery Region.
- SQL Server chạy trên Amazon EC2 kết hợp với Windows Server Failover Cluster.
- Mỗi Region đều sử dụng Amazon FSx for NetApp ONTAP.
- Dữ liệu được đồng bộ từ Region chính sang Region dự phòng thông qua **SnapMirror**.
- **FlexClone** được sử dụng để tạo bản sao từ snapshot mới nhất, giúp nhanh chóng cung cấp dữ liệu tại Region dự phòng.

---

## Các thành phần chính

### Amazon FSx for NetApp ONTAP

Amazon FSx for NetApp ONTAP là dịch vụ lưu trữ được AWS quản lý, cung cấp nhiều tính năng dành cho doanh nghiệp như Snapshot, Replication, Clone và High Availability.

### Snapshots

Snapshot giúp lưu lại trạng thái của dữ liệu tại một thời điểm xác định, tạo điểm khôi phục đáng tin cậy mà không ảnh hưởng đến hệ thống đang hoạt động.

### SnapMirror

SnapMirror là cơ chế đồng bộ dữ liệu giữa Region chính và Region dự phòng.

Theo bài viết, dữ liệu được sao chép liên tục nhằm đảm bảo **Recovery Point Objective (RPO)** luôn ở mức thấp, giúp giảm thiểu lượng dữ liệu có thể bị mất khi xảy ra sự cố.

### FlexClone

Đây là tính năng khiến em ấn tượng nhất.

Thay vì phải tạo lại toàn bộ dữ liệu, FlexClone có thể tạo gần như tức thời một bản sao có thể ghi từ snapshot hiện có. Điều này giúp rút ngắn đáng kể thời gian phục hồi và cho phép người dùng nhanh chóng truy cập dữ liệu.

---

## Quy trình khôi phục

Chiến lược Disaster Recovery gồm hai giai đoạn chính.

### 1. Chuyển đổi nhanh sang môi trường Read-only

- Tạo FlexClone từ snapshot mới nhất.
- Cho phép người dùng truy cập dữ liệu ở chế độ chỉ đọc trong thời gian rất ngắn.

### 2. Khôi phục hoàn toàn ở chế độ Read-write

- Dừng các thao tác ghi trên Region chính.
- Thực hiện đồng bộ dữ liệu cuối cùng bằng SnapMirror.
- Ngắt mối quan hệ sao chép dữ liệu.
- Chuyển môi trường Disaster Recovery thành hệ thống chính để tiếp tục vận hành.

Nhờ quy trình này, người dùng vẫn có thể truy cập dữ liệu quan trọng trong khi đội ngũ kỹ thuật hoàn tất quá trình khôi phục.

---

## Những điều em học được

Sau khi đọc bài viết, em nhận ra rằng việc thiết kế hệ thống trên AWS không chỉ dừng lại ở việc triển khai ứng dụng.

Khi xây dựng một hệ thống, cần phải cân nhắc thêm nhiều yếu tố như:

- Điều gì sẽ xảy ra nếu hệ thống gặp sự cố?
- Có thể mất bao nhiêu dữ liệu?
- Người dùng có còn truy cập được những dữ liệu quan trọng hay không?
- Hệ thống cần bao lâu để hoạt động trở lại?
- Quy trình Disaster Recovery đã được kiểm thử hay chưa?

Ngoài ra, em cũng hiểu rằng Disaster Recovery không chỉ đơn thuần là sao lưu dữ liệu mà là sự kết hợp giữa thiết kế kiến trúc, lưu trữ, đồng bộ dữ liệu, vận hành và đảm bảo tính liên tục của doanh nghiệp.

---

## Kết luận

Mặc dù em vẫn cần tìm hiểu thêm về Amazon FSx for NetApp ONTAP, SnapMirror và FlexClone, nhưng bài viết đã giúp em có cái nhìn thực tế về cách xây dựng một kiến trúc Disaster Recovery trên AWS.

Đây là một ví dụ rất hay cho thấy cách AWS kết hợp nhiều dịch vụ để tạo nên hệ thống có khả năng sẵn sàng cao, giảm thiểu thời gian gián đoạn và bảo vệ dữ liệu quan trọng của doanh nghiệp.

---

## Tài liệu tham khảo

**Link bài viết tham khảo:** [S&P Global’s innovative disaster recovery strategy using Amazon FSx for NetApp ONTAP snapshots](https://aws.amazon.com/blogs/architecture/sp-globals-innovative-disaster-recovery-strategy-using-amazon-fsx-for-netapp-ontap-snapshots/)
