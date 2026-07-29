---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
# TUẦN 6: BẢO MẬT, LUỒNG SAU HỌP VÀ NGUỒN DỮ LIỆU AI

## Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | - Tìm hiểu IAM Permission Boundary.<br>- Tìm hiểu IAM Policy Conditions và nguyên tắc explicit deny.<br>- Phân tích least privilege cho API Lambda, Reminder Lambda và AI Worker.<br>- Rà soát quyền truy cập DynamoDB, S3 và các dịch vụ tích hợp. | 20/07/2026 | 20/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[AWS IAM Documentation](https://docs.aws.amazon.com/iam/) |
| 3 | - Tìm hiểu AWS Security Hub, AWS WAF và AWS KMS.<br>- Tìm hiểu AWS Secrets Manager và Parameter Store.<br>- Ôn tập Amazon Cognito và JWT.<br>- Tìm hiểu GuardDuty, Macie và các nguyên tắc bảo mật Amazon S3. | 21/07/2026 | 21/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[AWS Security Documentation](https://docs.aws.amazon.com/security/) |
| 4 | - Thiết kế Reminder bằng Amazon EventBridge Scheduler.<br>- Thiết kế Reminder Lambda và in-app notification.<br>- Xác định Amazon SES là kênh email bổ sung.<br>- Thiết kế retry, idempotency và điều kiện bỏ qua Meeting đã hủy.<br>- Phân tích Meeting Minutes, Decision và Action Item. | 22/07/2026 | 22/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture) |
| 5 | - Phân tích Task và Dashboard.<br>- Xác định trạng thái Task gồm `TODO`, `DOING` và `DONE`.<br>- Thiết kế Task History và optimistic version.<br>- Xác định Personal Dashboard và Group Dashboard.<br>- Rà soát mô hình DynamoDB v2 gồm năm bảng vật lý. | 23/07/2026 | 23/07/2026 | [DynamoDB Data Model CampusMeet](https://github.com/Ngct253/CampusMeet/blob/main/docs/dynamodb-data-model.md)<br>[CampusMeet Team Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-trien-khai-nhom.md) |
| 6 | - Thiết kế S3 presigned upload và complete-upload verification.<br>- Xác định Attachment status, MIME, size và checksum validation.<br>- Phân tích Consent, Recording metadata và AIJob bất đồng bộ.<br>- Rà soát kiến trúc triển khai AWS.<br>- Cập nhật Proposal tiếng Việt, tiếng Anh và Worklog tuần 6. | 24/07/2026 | 24/07/2026 | [CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md)<br>[CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md) |

## Kết quả đạt được tuần 6

- Hiểu vai trò của IAM Permission Boundary.
- Hiểu cách sử dụng IAM Policy Conditions.
- Hiểu nguyên tắc explicit deny và least privilege.
- Phân tích được quyền cần thiết cho API Lambda, Reminder Lambda và AI Worker.
- Hiểu vai trò tổng quan của Security Hub, WAF, KMS, GuardDuty và Macie.
- Hiểu cách Secrets Manager và Parameter Store bảo vệ cấu hình phía máy chủ.
- Ôn lại cách Cognito phát hành JWT và API Gateway kiểm tra token.
- Hiểu các nguyên tắc bảo mật S3 như Block Public Access, encryption, lifecycle và presigned URL.
- Thiết kế được luồng Reminder:

> Quản trị viên cấu hình mốc nhắc  
> → Backend kiểm tra thời gian  
> → EventBridge Scheduler tạo one-time schedule  
> → Reminder Lambda được gọi  
> → Kiểm tra Meeting chưa bị hủy  
> → Tạo in-app notification  
> → Thử gửi email bằng SES  
> → Ghi trạng thái xử lý

- Xác định in-app notification là bắt buộc, còn email là kênh bổ sung.
- Thiết kế Meeting Minutes gồm Summary, Discussion, Decision và Action Item.
- Xác định Action Item có thể được chuyển thành Task sau khi xác nhận.
- Xác định Task gồm Title, Description, Assignee, Due Date, Priority, Status, Source Meeting và Version.
- Xác định trạng thái Task gồm `TODO`, `DOING` và `DONE`.
- Xác định Personal Dashboard và Group Dashboard.
- Chốt mô hình DynamoDB v2 gồm năm bảng:

> `identity`  
> `collaboration`  
> `meeting-data`  
> `task-data`  
> `ai-work`

- Thiết kế được luồng upload an toàn:

> Frontend yêu cầu upload  
> → API kiểm tra JWT và membership  
> → Tạo Attachment `PENDING_UPLOAD`  
> → Trả presigned URL ngắn hạn  
> → Browser upload trực tiếp lên S3  
> → Frontend gọi complete-upload  
> → Backend dùng `HeadObject` để xác minh  
> → Kiểm tra MIME, size, checksum và object key  
> → Attachment chuyển sang `READY` hoặc `REJECTED`  
> → Tạo AIJob khi phù hợp

- Xác định binary không được truyền qua API Gateway hoặc Lambda payload.
- Xác định Consent là bắt buộc trước khi recording hoặc live transcription.
- Xác định AIJob có các trạng thái `QUEUED`, `PROCESSING`, `COMPLETED`, `FAILED`, `CANCELLED`.
- Rà soát và hoàn thiện mô tả kiến trúc triển khai AWS.
- Cập nhật Proposal CampusMeet bằng tiếng Việt và tiếng Anh.

## Khó khăn gặp phải

- Tuần 6 có nhiều module phụ thuộc Group, Membership và Meeting.
- Reminder cần tránh gửi trùng khi Lambda được retry.
- Upload, recording và transcript chứa dữ liệu nhạy cảm.
- IAM Policy phải đủ quyền nhưng không được cấp quá rộng.
- File lớn không phù hợp để truyền qua API Gateway.
- AIJob cần xử lý bất đồng bộ và có trạng thái rõ ràng.
- Consent cần được ghi nhận trước khi capture âm thanh.
- Mô hình cũ có 17 bảng DynamoDB legacy và không thể xóa ngay.
- Cần phân biệt rõ thành phần đã triển khai, chưa hoàn thiện và kiến trúc mục tiêu.
- Reminder, Minutes, Task, Dashboard, Upload và AIJob vẫn cần application service, repository và integration test.

## Hướng xử lý

- Chia chức năng thành các vertical slice nhỏ.
- Merge shared contract trước khi triển khai frontend và backend dài.
- Kiểm tra membership và role trong mọi API liên quan đến group.
- Dùng EventBridge Scheduler cho reminder theo thời điểm cụ thể.
- Dùng idempotency và conditional write để tránh thông báo trùng.
- Dùng presigned URL để browser upload trực tiếp lên S3.
- Sử dụng `HeadObject` để xác minh file sau upload.
- Áp dụng MIME allowlist, giới hạn kích thước và checksum.
- Không lưu binary trong DynamoDB.
- Không ghi token, secret, transcript đầy đủ hoặc presigned URL còn hiệu lực vào log.
- Dùng AIJob và Step Functions cho tác vụ dài.
- Audit và backup bảng legacy trước khi cleanup.
- Chỉ xác nhận chức năng hoàn thành khi có code, test, deployment output và CloudWatch Logs.

## Kế hoạch tuần tiếp theo

- Thực hiện spike live transcription tiếng Việt.
- Hoàn thiện Attachment và complete-upload flow.
- Xây dựng Consent và Recording metadata.
- Xây dựng LiveTranscriptionSession.
- Lưu final transcript segment theo sequence.
- Xây dựng giao diện chỉnh sửa transcript.
- Chuẩn bị Step Functions và AI Worker.
- Thực hiện ingestion cho một Meeting.
- Chuẩn bị chatbot, RAG và citation.
- Kiểm thử ngăn truy xuất dữ liệu chéo nhóm.