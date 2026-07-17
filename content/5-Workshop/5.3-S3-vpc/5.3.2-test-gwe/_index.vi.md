---
title : "Kiểm tra kết quả upload frontend"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

### Mục tiêu
Xác minh rằng frontend artifacts đã có trong S3 bucket và sẵn sàng để tích hợp với CloudFront hoặc pipeline triển khai.

### Kiểm tra danh sách file
Trong tab **Files and folders**, xác nhận bucket có các file sau:
* `index.html`
* `favicon.svg`
* `icons.svg`
* `assets/index-*.js`
* `assets/index-*.css`

![Hình](images//workshop/s3-upload.png)

### Ý nghĩa của từng file
* `index.html`: entry point của single-page application.
* `assets/index-*.js`: mã JavaScript đã bundle, chứa logic giao diện và gọi API.
* `assets/index-*.css`: style của frontend.
* `favicon.svg` và `icons.svg`: static icon assets.

### Kết luận
Frontend đã được upload thành công. Ở bước tiếp theo, ta sẽ kiểm tra lớp xác thực, API GraphQL và backend Lambda mà frontend sử dụng.