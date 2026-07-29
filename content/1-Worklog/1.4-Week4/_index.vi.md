---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
# TUẦN 4: QUẢN LÝ CUỘC HỌP VÀ KIẾN TRÚC SERVERLESS

## Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | - Tìm hiểu AWS Lambda.<br>- Tìm hiểu event, context, execution role, memory, timeout và cold start.<br>- Tìm hiểu cách Lambda ghi log lên CloudWatch.<br>- Phân tích lý do CampusMeet sử dụng Lambda thay cho máy chủ chạy liên tục. | 06/07/2026 | 06/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/) |
| 3 | - Tìm hiểu Amazon API Gateway HTTP API.<br>- Tìm hiểu route, method, integration, stage, CORS và JWT authorizer.<br>- Phân tích luồng gọi API từ React đến Lambda.<br>- Tìm hiểu cách API Gateway kiểm tra JWT của Amazon Cognito. | 07/07/2026 | 07/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[Amazon API Gateway Documentation](https://docs.aws.amazon.com/apigateway/) |
| 4 | - Tìm hiểu AWS CloudFormation và AWS SAM.<br>- Tìm hiểu template, resource, parameter và output.<br>- Thực hành đọc cấu trúc SAM template của CampusMeet.<br>- Tìm hiểu quy trình validate, build, tạo change set, deploy và rollback. | 08/07/2026 | 08/07/2026 | [AWS SAM Documentation](https://docs.aws.amazon.com/serverless-application-model/)<br>[CampusMeet AWS Deployment Guide](https://github.com/Ngct253/CampusMeet/blob/main/docs/huong-dan-trien-khai-aws.md) |
| 5 | - Phân tích các thực thể Meeting, Agenda, Attendee và Organizer.<br>- Xác định các trạng thái vòng đời của cuộc họp.<br>- Xác định quy tắc kiểm tra thời gian bắt đầu và kết thúc.<br>- Xác định người tham dự phải thuộc nhóm.<br>- Xác định Organizer là trách nhiệm theo từng cuộc họp, không phải vai trò toàn cục. | 09/07/2026 | 09/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md) |
| 6 | - Thiết kế access pattern cho Meeting, Agenda và Attendee trong bảng `meeting-data`.<br>- Xác định API tạo, sửa, hủy và liệt kê cuộc họp.<br>- Xây dựng các trường hợp kiểm thử thời gian, membership và authorization.<br>- Rà soát giao diện Meeting hiện tại và dữ liệu mock.<br>- Tổng hợp và hoàn thành Worklog tuần 4. | 10/07/2026 | 10/07/2026 | [DynamoDB Data Model CampusMeet](https://github.com/Ngct253/CampusMeet/blob/main/docs/dynamodb-data-model.md)<br>[CampusMeet Repository](https://github.com/Ngct253/CampusMeet) |

## Kết quả đạt được tuần 4

- Hiểu cách AWS Lambda xử lý request theo mô hình event-driven.
- Hiểu vai trò của Lambda execution role.
- Hiểu ảnh hưởng của memory, timeout và cold start.
- Hiểu vai trò của API Gateway trong kiến trúc serverless.
- Hiểu cách JWT authorizer kiểm tra token trước khi gọi Lambda.
- Hiểu quy trình quản lý hạ tầng bằng AWS SAM và CloudFormation.
- Biết rằng template tồn tại không đồng nghĩa hệ thống đã được triển khai thành công.
- Xác định được Meeting, Agenda, Attendee và Organizer.
- Xác định vòng đời cuộc họp dự kiến:

> DRAFT  
> → SCHEDULED  
> → IN_PROGRESS  
> → COMPLETED  
>  
> Hoặc chuyển sang CANCELLED khi cuộc họp bị hủy.

- Xác định thời gian kết thúc phải sau thời gian bắt đầu.
- Xác định cuộc họp đã lên lịch không được có thời gian bắt đầu trong quá khứ.
- Xác định Attendee phải là thành viên đang hoạt động trong nhóm.
- Xác định Organizer là người được chọn cho một cuộc họp cụ thể.
- Thiết kế access pattern:

> `PK=MEETING#<meetingId>, SK=META`  
> `PK=MEETING#<meetingId>, SK=ATTENDEE#<userId>`  
> `PK=MEETING#<meetingId>, SK=AGENDA#<order>#<agendaId>`

- Xác định Meeting API phải ánh xạ `meetingId` về `groupId` ở backend trước khi kiểm tra quyền.

## Khó khăn gặp phải

- Meeting là dependency của Reminder, Minutes, Task, Google Integration, Transcript và AI.
- Cần thống nhất contract trước khi nhiều thành viên triển khai song song.
- Không thể tin `groupId`, `userId` hoặc role do frontend gửi lên.
- Việc sửa hoặc hủy cuộc họp cần được thực hiện idempotent.
- Meeting lifecycle cần hạn chế các trạng thái chuyển đổi không hợp lệ.
- Một số màn hình Meeting vẫn đang sử dụng dữ liệu mock.
- Meeting API và DynamoDB repository thật chưa hoàn thiện.

## Hướng xử lý

- Backend luôn truy vấn Meeting để lấy `groupId` đáng tin cậy.
- Áp dụng authorization boundary từ module Membership.
- Sử dụng shared DTO trong `@campusmeet/shared`.
- Dùng conditional write khi cập nhật trạng thái Meeting.
- Dùng idempotency key cho thao tác có thể retry.
- Tách Handler, Middleware, Application Service, Domain và Repository.
- Không đặt truy vấn DynamoDB trực tiếp trong Handler.
- Viết kiểm thử Meeting của Group A không thể được truy cập bởi người chỉ thuộc Group B.
- Chỉ xác nhận luồng hoàn thiện khi có integration test, deployment output và CloudWatch Logs.

## Kế hoạch tuần tiếp theo

- Tìm hiểu Operational Excellence trên AWS.
- Tìm hiểu CloudWatch nâng cao, Amazon SNS và tagging.
- Phân tích Google OAuth Authorization Code Flow.
- Thiết kế Google Calendar event và yêu cầu Google Meet conference data.
- Xác định trạng thái đồng bộ Google.
- Thiết kế retry và idempotency cho Google integration.
- Xác định fallback khi Google artifact không khả dụng.

---


