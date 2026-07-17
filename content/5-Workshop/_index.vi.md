---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# TRIỂN KHAI TASK MANAGEMENT SYSTEM TRÊN AWS SERVERLESS

### Tổng quan

Workshop này hướng dẫn triển khai và kiểm tra hệ thống **Task Management System** theo kiến trúc serverless trên AWS. Hệ thống cho phép người dùng đăng nhập, quản lý board, tạo task, cập nhật trạng thái công việc và ghi nhận hoạt động vận hành thông qua các dịch vụ được quản lý bởi AWS.

Kiến trúc sử dụng Amazon S3 để lưu static frontend, Amazon Cognito để xác thực người dùng, AWS AppSync để cung cấp GraphQL API, AWS Lambda để xử lý logic nghiệp vụ, Amazon DynamoDB để lưu dữ liệu, IAM để phân quyền và Amazon CloudWatch để theo dõi log.

### Nội dung

1. [Tổng quan workshop](5.1-Workshop-overview/)
2. [Chuẩn bị môi trường và quyền IAM](5.2-Prerequiste/)
3. [Triển khai frontend với Amazon S3](5.3-S3-frontend/)
4. [Cấu hình authentication, API và backend](5.4-Backend-config/)
5. [Kiểm tra DynamoDB, indexes và PITR](5.5-DynamoDB-check/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)