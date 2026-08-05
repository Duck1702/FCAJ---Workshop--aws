---
title: "5.1 Giới thiệu Workshop CampusMeet"
date: 2026-08-05
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---
# 5.1 GIỚI THIỆU WORKSHOP CAMPUSMEET

## Giới thiệu

CampusMeet giải quyết tình trạng lịch, agenda, người tham dự, biên bản và nhiệm vụ bị phân tán. Hệ thống quản lý vòng đời cuộc họp; Google Meet chỉ là nền tảng họp trực tuyến bên ngoài. Workshop chuyển bản đề xuất ở Phần 2 thành một lộ trình có thể kiểm tra bằng source, test, IaC và kết quả quan sát được.

## Mục tiêu

Sau workshop, người học có thể:

- nhận diện đúng luồng CloudFront/S3 → Cognito → API Gateway/Lambda → DynamoDB;
- cài dependency và chạy `lint`, `typecheck`, `test`, `build`, `infra:validate`;
- cấu hình ba biến frontend công khai mà không commit `.env`;
- thực hiện M1 và core Meeting flow, gồm kiểm tra truy cập chéo nhóm;
- giải thích upload, transcript, AIJob, RAG và citation mà không nhầm target architecture với deployment hoàn tất;
- tìm log CloudWatch và dọn tài nguyên có kiểm soát.

## Kiến trúc workshop

![Kiến trúc AWS của CampusMeet](/FCAJ---Workshop--aws/images/campusmeet/campusmeet-aws-architecture.png)

- **Edge:** React/TypeScript/Vite build được lưu ở S3 riêng tư và phân phối qua CloudFront.
- **Identity/API:** Cognito phát JWT; HTTP API gọi Lambda; backend xác minh active membership và role.
- **Data:** năm bảng DynamoDB vật lý; S3 user-content giữ binary.
- **AI:** Transcribe, Step Functions, AI Worker, Bedrock Knowledge Bases và S3 Vectors thuộc kiến trúc mục tiêu/IaC; full pipeline chưa có bằng chứng E2E trên AWS.
- **Integration:** Google OAuth, Calendar và Meet là hệ thống bên ngoài; Scheduler/Reminder/SES xử lý nhắc lịch theo thiết kế.
- **Operations:** CloudWatch, SNS, IAM, SAM/CloudFormation và AWS Budgets.

Không có VPC, subnet, NAT Gateway, EC2, ECS, EKS hoặc RDS trong baseline.

## Kịch bản xuyên suốt

1. Đăng nhập và tạo/tham gia group (core).
2. Group Admin quản lý lời mời/thành viên (core).
3. Tạo meeting với agenda, organizer và attendees (core).
4. Xem timeline; cập nhật bằng optimistic version; hủy hai lần để kiểm tra idempotency (core).
5. Dùng user thuộc group khác để xác minh `403`/không lộ dữ liệu (core).
6. Xem Task/Minutes/Dashboard (partial walkthrough vì handler còn `501`).
7. Xem Google OAuth/Calendar/Meet/reminder (architecture walkthrough).
8. Chạy test adapter AI và theo dõi AIJob/RAG/citation (advanced, không phải live cloud lab bắt buộc).

## Phạm vi

| Nhóm | Nội dung |
|---|---|
| Core lab | Chuẩn bị, quality gates, auth/group, Meeting CRUD/lifecycle và cross-group isolation |
| Optional AI lab | Test/mock AI request, AI Worker, Knowledge Base adapter và citation |
| Architecture-only | Live transcription, upload hoàn chỉnh, Google lifecycle, reminder end-to-end |
| Ngoài phạm vi | Nền tảng video riêng, production, multi-region active-active, billing thương mại |

## Thời lượng

| Module | Nội dung | Thời lượng |
|---|---|---:|
| 5.1–5.3 | Kiến trúc, môi trường, source/validation | 45–60 phút |
| 5.4–5.5 | Data/auth/backend/frontend có điều kiện | 45–75 phút |
| 5.6 | Core Meeting flow | 45–60 phút |
| 5.7–5.9 | Partial/AI/monitoring | 30–60 phút |
| 5.10 | Cleanup | 15–30 phút |

## Chi phí và kết quả

Transcribe, Bedrock, Knowledge Bases, S3 Vectors và log retention có thể không thuộc Free Tier. Sau workshop, người học phải có kết quả quality gates, bằng chứng auth/Meeting flow hoặc test local, một negative authorization check, log/stack output nếu đã deploy, và checklist cleanup. Giá cần kiểm tra lại bằng AWS Pricing Calculator trước buổi lab.

Tiếp theo: [5.2 Chuẩn bị môi trường](../5.2-environment/).