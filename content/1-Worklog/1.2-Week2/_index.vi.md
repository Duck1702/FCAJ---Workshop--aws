---
title: "Worklog Tuần 2"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# TUẦN 2: XÂY DỰNG NỀN TẢNG AWS VÀ CẤU TRÚC CAMPUSMEET

## Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | - Tìm hiểu IAM User, IAM Group, IAM Role và IAM Policy.<br>- Tìm hiểu nguyên tắc least privilege.<br>- Thực hành kiểm tra danh tính AWS bằng AWS CLI.<br>- Thống nhất không chia sẻ access key giữa các thành viên. | 22/06/2026 | 22/06/2026 | [AWS IAM](https://cloudjourney.awsstudygroup.com/vi/1-explore/) |
| 3 | - Tìm hiểu VPC, subnet, route table, Internet Gateway, NAT Gateway và Security Group.<br>- Phân tích lý do CampusMeet MVP không cần tự xây dựng VPC hoặc NAT Gateway.<br>- Tìm hiểu EC2 và chi phí tài nguyên chạy liên tục. | 23/06/2026 | 23/06/2026 | [Amazon VPC và EC2](https://cloudjourney.awsstudygroup.com/vi/1-explore/) |
| 4 | - Tìm hiểu Amazon S3, bucket, object, Block Public Access, encryption và lifecycle.<br>- Tìm hiểu Amazon RDS và DynamoDB.<br>- So sánh database quan hệ với NoSQL theo access pattern. | 24/06/2026 | 24/06/2026 | [Khám phá dịch vụ AWS](https://cloudjourney.awsstudygroup.com/vi/1-explore/) |
| 5 | - Xây dựng cấu trúc monorepo CampusMeet gồm `apps`, `services`, `packages`, `infra`, `scripts` và `docs`.<br>- Xác định frontend React/TypeScript và backend Node.js/TypeScript.<br>- Chuẩn bị shared types và API contract. | 25/06/2026 | 25/06/2026 | [CampusMeet Repository](https://github.com/Ngct253/CampusMeet) |
| 6 | - Rà soát nền tảng Cognito authentication.<br>- Kiểm tra protected routes, API client và endpoint `/health`.<br>- Tìm hiểu GitHub Actions, AWS SAM và CloudFormation.<br>- Hoàn thành Worklog tuần 2. | 26/06/2026 | 26/06/2026 | [CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md)<br>[AWS SAM](https://docs.aws.amazon.com/serverless-application-model/) |

## Kết quả đạt được tuần 2

- Hiểu vai trò của IAM User, Group, Role và Policy.
- Hiểu nguyên tắc least privilege và không sử dụng credential dùng chung.
- Nắm được các thành phần cơ bản của Amazon VPC.
- Hiểu sự khác nhau giữa EC2 và kiến trúc serverless.
- Hiểu cách S3 lưu trữ và bảo vệ dữ liệu.
- Phân biệt RDS và DynamoDB ở mức cơ bản.
- Xây dựng được cấu trúc monorepo cho CampusMeet.
- Xác định ranh giới frontend, backend, shared package và infrastructure.
- Kiểm tra được nền tảng Cognito authentication và API health check.
- Có nền tảng CI và Infrastructure as Code bước đầu.

## Khó khăn gặp phải

- Nhiều thành viên có thể sửa cùng các file shared hoặc infrastructure.
- Dễ nhầm giữa template tồn tại và tài nguyên đã triển khai thành công.
- Cần thống nhất tên thư mục, kiểu dữ liệu và API contract.
- Cần kiểm soát credential khi làm việc trên máy cá nhân.
- Chưa có đầy đủ repository DynamoDB và API nghiệp vụ thật.

## Hướng xử lý

- Thống nhất cấu trúc repository trước khi triển khai tính năng.
- Dùng shared contract thay vì copy interface giữa frontend và backend.
- Chỉ xác nhận chức năng đã hoạt động khi có code, test, output và log.
- Mỗi thành viên dùng AWS profile riêng.
- Không đưa secret hoặc access key vào `.env` được commit.
- Mọi thay đổi infrastructure phải được review trước khi deploy.

## Kế hoạch tuần tiếp theo

- Phân tích Group, Membership và Invitation.
- Xác định vai trò Member và Admin.
- Thiết kế authorization boundary.
- Tìm hiểu sâu hơn về DynamoDB, CloudFront và CloudWatch.
- Thiết kế bảng `collaboration` và các access pattern.
- Chuẩn bị test từ chối truy cập chéo nhóm.