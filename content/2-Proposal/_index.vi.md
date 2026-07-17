---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# TASK MANAGEMENT SYSTEM 

## Kiến trúc AWS Serverless cho hệ thống quản lý công việc bảo mật 


### 1. Tóm tắt điều hành
Task Management System được thiết kế như một ứng dụng web bảo mật, dễ mở rộng và giảm tải vận hành cho việc tạo, giao, theo dõi và cập nhật công việc theo thời gian thực. Hệ thống sử dụng kiến trúc AWS Serverless để tập trung vào chức năng ứng dụng thay vì quản lý máy chủ, vá lỗi hệ điều hành hoặc dự phòng hạ tầng thủ công.

Frontend được phân phối qua Amazon CloudFront từ một Amazon S3 bucket riêng tư, Amazon Route 53 xử lý DNS, AWS Certificate Manager cung cấp chứng chỉ TLS, và AWS WAF bảo vệ lớp edge public. Người dùng xác thực qua Amazon Cognito và gửi GraphQL query, mutation, subscription đến AWS AppSync kèm JWT. Logic nghiệp vụ chạy trên AWS Lambda, dữ liệu task lưu trong Amazon DynamoDB có bật point-in-time recovery, còn log, metrics và alarm được tập trung tại Amazon CloudWatch. Quy trình triển khai được tự động hóa từ GitHub qua AWS CodePipeline, AWS CodeBuild và AWS CloudFormation.


### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại
* Nhiều nhóm nhỏ vẫn quản lý công việc bằng tin nhắn, bảng tính hoặc chủ rời rạc.
* Cách làm này khiến việc xác định người phụ trách, trạng thái hiện tại, mức ưu tiên và lịch sử thay đổi trở nên khó khăn.
* Khi số lượng công việc tăng, cập nhật thủ công dễ tạo dữ liệu trùng lặp, bỏ sót deadline và giảm khả năng theo dõi tiến độ.
* Ngoài ra, cách triển khai web truyền thống có thể tạo thêm nhiều việc vận hành không cần thiết cho một project thực tập hoặc project sinh viên: quản lý server, triển khai thủ công, backup database, cấu hình SSL và giám sát lỗi.

#### Giải pháp
Hệ thống cung cấp một ứng dụng quản lý công việc tập trung dựa trên các dịch vụ AWS được quản lý. Người dùng đăng nhập qua Cognito, sau đó thao tác với task thông qua AppSync GraphQL API. Query và mutation hỗ trợ xem danh sách task, tạo task, cập nhật task, giao việc và hoàn thành công việc. GraphQL subscription hỗ trợ cập nhật gắn thời gian thực để các thành viên thấy thay đổi mà không cần tải lại trang.

#### Lợi ích và hoàn vốn đầu tư
Hệ thống cải thiện khả năng quan sát tiến độ bằng cách tập trung dữ liệu công việc, trạng thái, người phụ trách và cập nhật vào một nơi. Các dịch vụ serverless giúp mô hình vận hành nhẹ hơn và kiểm soát chi phí tốt hơn vì phần lớn tài nguyên tính theo mức sử dụng. Tự động hóa triển khai giúp rút ngắn thời gian release, giảm lỗi cấu hình và làm project dễ bảo trì sau giai đoạn thực tập.


### 3. Kiến trúc giải pháp
Kiến trúc được chia theo các lớp serverless gồm phân phối frontend, xác thực, API, backend, dữ liệu, quan sát vận hành và CI/CD. Website request được phân giải bởi Route 53 và phân phối qua CloudFront từ S3 bucket riêng tư. Login request đi qua Cognito. Các request ứng dụng đã xác thực sử dụng AppSync GraphQL với JWT, sau đó AppSync gọi Lambda để xử lý nghiệp vụ và DynamoDB để lưu dữ liệu task. CloudWatch thu thập logs, metrics và alarms cho toàn bộ ứng dụng.

![Task Management System Architecture](images/proposal/task_management_architecture.png)

#### Dịch vụ AWS sử dụng
* **Amazon Route 53**: Quản lý DNS record cho domain của ứng dụng.
* **Amazon CloudFront**: Phân phối frontend toàn cầu và kết nối người dùng đến S3 private origin.
* **Amazon S3**: Lưu trữ static website assets trong private bucket.
* **AWS WAF**: Bảo vệ CloudFront distribution trước các kiểu tấn công web phổ biến.
* **AWS Certificate Manager**: Cấp và quản lý chứng chỉ TLS cho HTTPS.
* **Amazon Cognito**: Xử lý đăng ký, đăng nhập, phát hành JWT token và xác thực người dùng.
* **AWS AppSync**: Cung cấp GraphQL API cho query, mutation và subscription.
* **AWS Lambda**: Chạy logic nghiệp vụ quản lý task mà không cần quản lý server.
* **Amazon DynamoDB**: Lưu dữ liệu task, user, trạng thái và phân công; bật point-in-time recovery.
* **Amazon CloudWatch**: Thu thập logs, metrics và alarms phục vụ monitoring và troubleshooting.
* **AWS CodePipeline**: Tự động hóa triển khai từ thay đổi source code.
* **AWS CodeBuild**: Build và kiểm tra application artifacts.
* **AWS CloudFormation**: Khởi tạo và cập nhật hạ tầng dưới dạng code.
* **GitHub**: Lưu source code và kích hoạt CI/CD pipeline.

#### Thiết kế thành phần
* **Phân phối frontend**: Static frontend files được upload lên S3 private bucket và phân phối qua CloudFront. Route 53 ánh xạ custom domain tới CloudFront distribution, còn ACM bật HTTPS.
* **Bảo mật edge**: AWS WAF gắn với CloudFront để lọc traffic độc hại hoặc bất thường trước khi request đi sâu vào ứng dụng.
* **Xác thực**: Cognito quản lý người dùng và trả về JWT token sau khi đăng nhập. Frontend đính kèm token này vào GraphQL request.
* **Lớp API**: AppSync xác thực JWT và cung cấp GraphQL operations cho tạo task, cập nhật task, lọc danh sách và nhận sự kiện thời gian thực.
* **Logic backend**: Lambda triển khai các rule nghiệp vụ như kiểm tra quyền thao tác task, cập nhật trạng thái và chuẩn hóa response.
* **Lớp dữ liệu**: DynamoDB lưu dữ liệu task và bật point-in-time recovery để bảo vệ trước thao tác ghi hoặc xóa nhầm.
* **Vận hành**: CloudWatch nhận logs và metrics từ AppSync, Lambda, DynamoDB và pipeline triển khai. Alarm có thể thông báo khi xuất hiện lỗi hoặc usage bất thường.
* **Triển khai**: Developer push code lên GitHub. CodePipeline và CodeBuild đóng gói ứng dụng, sau đó CloudFormation cập nhật tài nguyên AWS và triển khai thay đổi frontend/backend.


### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai
1. Phân tích yêu cầu và thiết kế kiến trúc: Xác định vai trò người dùng, vòng đời task, GraphQL operations, ranh giới bảo mật và trách nhiệm của từng dịch vụ AWS.
2. Thiết lập infrastructure as code: Tạo CloudFormation templates cho S3, CloudFront, WAF, ACM, Cognito, AppSync, Lambda, DynamoDB, CloudWatch và CI/CD.
3. Phát triển frontend: Xây dựng giao diện, màn hình xác thực, danh sách task, chi tiết task, bộ lọc và cập nhật thời gian thực.
4. Phát triển backend và API: Định nghĩa GraphQL schema, kết nối resolver với Lambda, DynamoDB và pipeline triển khai.
5. Monitoring và security hardening: Cấu hình CloudWatch logs, metrics, alarms, IAM least privilege, DynamoDB PITR và WAF rules.
6. Kiểm thử và triển khai: Kiểm tra đăng nhập, CRUD task, subscription update, CI/CD deployment, rollback và các tình huống lỗi cơ bản.

#### Yêu cầu kỹ thuật
* Static web frontend có thể build và triển khai lên S3.
* Cognito user pool cho xác thực và phân quyền dựa trên JWT.
* AppSync GraphQL schema bao phủ task query, mutation và subscription.
* Lambda runtime cho logic backend và tích hợp DynamoDB.
* DynamoDB table design cho các access pattern liên quan đến task, user, project, status, priority và assignment.
* CloudFormation templates để triển khai hạ tầng có thể lập lại.
* CI/CD pipeline kết nối GitHub để tự động build và deploy.
* CloudWatch dashboard hoặc alarm để theo dõi lỗi và quan sát vận hành.


### 5. Lộ trình và mốc triển khai
* **Trước triển khai**: Nghiên cứu các dịch vụ AWS Serverless, so sánh phương án kiến trúc và chốt phạm vi project.
* **Tháng 1**: Thiết kế kiến trúc giải pháp, xác định data model, tạo CloudFormation templates ban đầu và cấu hình luồng hosting frontend cơ bản.
* **Tháng 2**: Triển khai Cognito authentication, AppSync GraphQL API, Lambda functions và DynamoDB persistence.
* **Tháng 3**: Hoàn thiện CI/CD, monitoring, WAF configuration, integration testing, tài liệu và phần trình bày cuối kỳ.
* **Sau triển khai**: Cải thiện UI/UX, bổ sung grouping theo team/project, tăng cường audit logging và tối ưu chi phí dựa trên usage thực tế.


### 6. Ước tính ngân sách
Project được thiết kế cho workload quy mô nhỏ trong giai đoạn thực tập, nên phần lớn usage dự kiến nằm trong mức chi phí thấp hoặc free tier. Ước tính cuối cùng nên được tính lại bằng AWS Pricing Calculator trước khi triển khai vì chi phí AWS phụ thuộc vào Region, traffic và plan được chọn.

#### Chi phí hạ tầng ước tính
* **Route 53**: khoảng 0,50 USD/tháng cho một public hosted zone, cộng thêm DNS query charges nếu có.
* **CloudFront và S3**: dự kiến rất thấp cho static assets và traffic nhỏ; CloudFront cũng có free plan với usage allowance hằng tháng.
* **Cognito**: dự kiến 0 USD/tháng cho nhóm người dùng nội bộ nằm trong free tier theo monthly active users.
* **AppSync**: dự kiến 0 USD/tháng khi còn trong free-tier usage query/mutation và real-time operations.
* **Lambda**: dự kiến 0 USD/tháng với lượng invocation thấp nằm trong Lambda free tier.
* **DynamoDB**: dự kiến 0 USD/tháng cho table nhỏ nằm trong free-tier storage/provisioned capacity, hoặc chi phí pay-per-request thấp nếu dùng on-demand mode.
* **CloudWatch**: dự kiến 0 USD/tháng nếu logs, metrics và alarms nằm trong free tier.
* **WAF**: là chi phí bảo mật định kỳ chính nếu bật riêng; ước tính khoảng 10-15 USD/tháng cho một Web ACL với bộ rule nhỏ và request volume thấp.
* **CodePipeline, CodeBuild và CloudFormation**: dự kiến thấp với pipeline nhỏ và số lần build giới hạn.

**Tổng ước tính**: khoảng 1-3 USD/tháng cho core serverless application traffic thấp khi chưa tính WAF riêng, hoặc khoảng 12-18 USD/tháng khi bật WAF.


### 7. Đánh giá rủi ro

#### Ma trận rủi ro
* Cấu hình sai authentication hoặc authorization: Ảnh hưởng cao, xác suất trung bình.
* Lỗi GraphQL resolver hoặc Lambda: Ảnh hưởng trung bình, xác suất trung bình.
* DynamoDB access pattern chưa phù hợp: Ảnh hưởng trung bình, xác suất trung bình.
* Xóa nhầm hoặc cập nhật sai dữ liệu: Ảnh hưởng cao, xác suất thấp.
* Chi phí phát sinh từ WAF, logs hoặc traffic tăng đột biến: Ảnh hưởng trung bình, xác suất thấp.
* CI/CD deployment failure: Ảnh hưởng trung bình, xác suất trung bình.

#### Chiến lược giảm thiểu
* Áp dụng IAM least privilege và kiểm tra Cognito JWT authorization trong AppSync.
* Dùng structured logging và CloudWatch alarms cho lỗi Lambda và AppSync.
* Thiết kế DynamoDB keys theo access pattern thực tế trước khi triển khai.
* Bật DynamoDB point-in-time recovery và hạn chế ghi dữ liệu thủ công trực tiếp ở môi trường production.
* Cấu hình AWS Budgets và rà soát CloudWatch log retention.
* Giữ thay đổi hạ tầng trong CloudFormation và kiểm thử pipeline trước khi nghiệm thu.

#### Kế hoạch dự phòng
* Rollback deployment lỗi thông qua CloudFormation và CodePipeline.
* Khôi phục DynamoDB records bằng point-in-time recovery nếu dữ liệu bị thay đổi ngoài ý muốn.
* Tạm thời tắt các tính năng không bắt buộc như subscription hoặc một số WAF rules nếu gây lỗi chi phí hoặc availability trong lúc kiểm thử.
* Chỉ dùng manual deployment như phương án ngắn hạn trong khi sửa CI/CD pipeline.


### 8. Kết quả kỳ vọng

#### Cải tiến kỹ thuật
Hệ thống cuối cùng cung cấp một ứng dụng quản lý công việc serverless có xác thực bảo mật, cập nhật GraphQL theo thời gian thực, triển khai tự động, monitoring tập trung và lưu trữ task data có khả năng khôi phục.

#### Giá trị dài hạn
Kiến trúc có thể tái sử dụng làm nền tảng cho các công cụ cộng tác nhóm, issue tracking hoặc ứng dụng workflow nội bộ trong tương lai. Project cũng thể hiện kỹ năng AWS thực tế ở các mảng frontend hosting, identity, serverless backend, NoSQL data modeling, monitoring, security và CI/CD automation.