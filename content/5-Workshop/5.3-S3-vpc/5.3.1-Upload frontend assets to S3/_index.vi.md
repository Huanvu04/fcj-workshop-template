---
title : "Upload frontend assets lên S3"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

### Mục tiêu
Upload frontend build output của TaskManager lên Amazon S3 để chuẩn bị cho bước phân phối giao diện web.

### Các bước thực hiện
1. Build frontend ở môi trường local hoặc CI/CD.
2. Mở Amazon S3 console tại Region `ap-southeast-1`.
3. Chọn bucket frontend có tên dạng `taskmanager-frontend-dev-*`.
4. Chọn **Upload** và thêm các file build output.
5. Xác nhận upload thành công.

### Kết quả
Bucket đã nhận đủ 5 file frontend:
* `index.html`
* JavaScript bundle trong thư mục `assets/`
* CSS bundle trong thư mục `assets/`
* `favicon.svg`
* `icons.svg`

![Hình](images/workshop/s3-upload.png)

Trong ảnh, quá trình upload hiển thị **Succeeded: 5 files, 611.4 KB (100%)**, nghĩa là frontend artifacts đã được đưa lên S3 thành công.

### Ghi chú bảo mật
Nếu kết nối với CloudFront, không nên public trực tiếp S3 bucket. Thay vào đó, dùng CloudFront Origin Access Control hoặc Origin Access Identity để chỉ CloudFront có quyền đọc object trong bucket.