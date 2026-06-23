---
title: "Worklog Tuần 2"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# Tuần 2: Hoàn thiện Proposal, cấu trúc website và học Module 2

## Các công việc cần triển khai trong tuần này

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------- |
| 2   | - Chốt ý tưởng project: AI-Powered Personal Finance Budget Alert System on AWS.<br>- Xác định phạm vi chính của hệ thống: quản lý giao dịch, ngân sách, cảnh báo vượt ngân sách và phân tích AI.<br>- Xác định các dịch vụ AWS dự kiến sử dụng như Cognito, API Gateway, Lambda, DynamoDB, SNS, CloudWatch, Bedrock, IAM và AWS Budgets. | 22/06/2026   | 26/06/2026      | AWS Documentation, FCAJ Requirement   |
| 3   | - Hoàn thiện nội dung Proposal tiếng Việt và tiếng Anh.<br>- Thêm ảnh kiến trúc tổng quan của hệ thống vào phần Proposal.<br>- Sửa lỗi menu, header và sidebar cho website báo cáo.<br>- Chuẩn hóa cấu trúc song ngữ cho các mục tiếng Việt và tiếng Anh.                                                                                | 23/06/2026   | 26/06/2026      | Hugo Documentation, GitHub Repository |
| 4   | - Tạo README.md cho repository để mô tả project trên GitHub.<br>- Kiểm tra lại kế hoạch kiểm soát chi phí AWS trước khi triển khai tài nguyên.<br>- Hoàn thành nội dung học tập Module 2.<br>- Ghi chú các kiến thức quan trọng trong Module 2.                                                                                          | 24/06/2026   | 26/06/2026      | GitHub, Module 2                      |
| 5   | - Chuẩn bị nội dung tài liệu kỹ thuật cho project AWS.<br>- Viết các tài liệu ban đầu như requirements, architecture, data model, API endpoints, cost control và cleanup checklist.                                                                                                                                                      | 25/06/2026   | 26/06/2026      | AWS Documentation, Project Repository |
| 6   | - Chuẩn bị cho giai đoạn triển khai AWS ở tuần tiếp theo.<br>- Xác định thứ tự triển khai các dịch vụ: AWS Budget, IAM, Cognito, DynamoDB, Lambda, API Gateway, SNS, CloudWatch và Bedrock.<br>- Tổng hợp kết quả tuần 2, hoàn thiện Worklog Tuần 2 và lập kế hoạch cho tuần 3.                                                          | 26/06/2026   | 26/06/2026      | AWS Console, GitHub Repository        |

## Kết quả đạt được tuần 2

* Đã chốt được ý tưởng và phạm vi chính thức của project.
* Đã xác định các dịch vụ AWS sẽ sử dụng trong hệ thống.
* Hoàn thiện Proposal bằng tiếng Việt và tiếng Anh.
* Thêm ảnh kiến trúc tổng quan vào phần Proposal.
* Sửa các lỗi giao diện và sidebar của website báo cáo.
* Chuẩn hóa thêm cấu trúc song ngữ cho website.
* Tạo README.md để mô tả project trên GitHub.
* Hoàn thành nội dung học tập Module 2.
* Chuẩn bị các tài liệu kỹ thuật ban đầu cho project AWS.
* Xây dựng được kế hoạch triển khai AWS cho tuần tiếp theo.

## Minh chứng tuần 2

### 1. Proposal đã được cập nhật

![Proposal](/FCAJ---Workshop--aws/images/1-Worklog/week2/proposal.png)

### 2. Website Hugo chạy local

![Hugo Local Server](/FCAJ---Workshop--aws/images/1-Worklog/week2/hugo_local.png)

### 3. GitHub repository đã được cập nhật

![GitHub Repository](/FCAJ---Workshop--aws/images/1-Worklog/week2/github_repo.png)

### 4. Hoàn thành Module 2

![Module 2](/FCAJ---Workshop--aws/images/1-Worklog/week2/module-2.png)

## Khó khăn gặp phải

* Theme Hugo cũ phát sinh lỗi khi chạy với phiên bản Hugo mới.
* Sidebar tiếng Anh chưa hiển thị đầy đủ do thiếu một số file `_index.en.md`.
* Cần kiểm tra kỹ đường dẫn ảnh để ảnh minh chứng hiển thị đúng.
* Cần chú ý kiểm soát chi phí trước khi triển khai tài nguyên AWS thật.

## Hướng xử lý

* Sửa các file template của Hugo theme để website chạy ổn định hơn.
* Tạo thêm các file `_index.en.md` cho các mục tiếng Anh còn thiếu.
* Kiểm tra lại cấu trúc thư mục ảnh trong `static/images`.
* Chuẩn bị trước tài liệu cost control và cleanup checklist trước khi tạo tài nguyên AWS.

## Kế hoạch tuần tiếp theo

* Bắt đầu triển khai các dịch vụ AWS nền tảng.
* Cấu hình IAM user và quyền truy cập cần thiết.
* Tạo Amazon Cognito để quản lý người dùng.
* Thiết kế và tạo các bảng DynamoDB.
* Chuẩn bị Lambda functions cho giao dịch và ngân sách.
* Tiếp tục ghi lại minh chứng triển khai cho phần Workshop.
