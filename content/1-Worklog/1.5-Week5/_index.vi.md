---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
## Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | - Tìm hiểu Operational Excellence trên AWS.<br>- Tìm hiểu runbook, automation, deployment review và smoke test.<br>- Tìm hiểu quy trình rollback và cleanup tài nguyên.<br>- Đối chiếu với hướng dẫn triển khai CampusMeet. | 13/07/2026 | 13/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[CampusMeet AWS Deployment Guide](https://github.com/Ngct253/CampusMeet/blob/main/docs/huong-dan-trien-khai-aws.md) |
| 3 | - Tìm hiểu CloudWatch Metrics và Alarms nâng cao.<br>- Tìm hiểu Amazon SNS và cơ chế gửi cảnh báo vận hành.<br>- Xác định các metric cần theo dõi như API error, Lambda error, duration và Google sync failure.<br>- Tìm hiểu cách đặt thời gian lưu CloudWatch Logs. | 14/07/2026 | 14/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[Amazon SNS Documentation](https://docs.aws.amazon.com/sns/) |
| 4 | - Tìm hiểu tagging và cách phân loại tài nguyên theo Project, Environment và Owner.<br>- Tìm hiểu AWS Systems Manager Parameter Store.<br>- Phân biệt cấu hình công khai với secret phía máy chủ.<br>- Xác định không lưu Google access token hoặc refresh token trong frontend. | 15/07/2026 | 15/07/2026 | [AWS Systems Manager Documentation](https://docs.aws.amazon.com/systems-manager/)<br>[The First Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 5 | - Phân tích Google OAuth Authorization Code Flow.<br>- Thiết kế luồng kết nối và ngắt kết nối tài khoản Google.<br>- Thiết kế luồng tạo Google Calendar event.<br>- Tìm hiểu `conferenceData.createRequest` và request ID.<br>- Xác định `googleEventId` và trạng thái đồng bộ. | 16/07/2026 | 16/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md) |
| 6 | - Thiết kế retry và idempotency cho Google integration.<br>- Phân tích khả năng đồng bộ recording hoặc transcript sau cuộc họp.<br>- Xác định phương án fallback bằng upload hoặc capture có consent.<br>- Xác định Google Meet Add-on chỉ sử dụng cùng API và database với CampusMeet Web.<br>- Cập nhật tài liệu kiến trúc và hoàn thành Worklog tuần 5. | 17/07/2026 | 17/07/2026 | [CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture)<br>[CampusMeet Repository](https://github.com/Ngct253/CampusMeet) |


## Kết quả đạt được tuần 5

- Hiểu các nguyên tắc Operational Excellence trên AWS.
- Hiểu vai trò của runbook, deployment review, smoke test, rollback và cleanup.
- Hiểu cách CloudWatch Alarm kết hợp với Amazon SNS để gửi cảnh báo.
- Biết cách sử dụng tag để phân loại và quản lý tài nguyên.
- Hiểu vai trò của Parameter Store trong việc lưu cấu hình phía máy chủ.
- Xác định CampusMeet không xây dựng hoặc sao chép Google Meet.
- Xác định Google Meet là nền tảng thực hiện cuộc gọi, còn CampusMeet quản lý quy trình cuộc họp.
- Thiết kế được luồng Google OAuth:

> Người dùng chọn kết nối Google  
> → Backend tạo OAuth state  
> → Người dùng cấp quyền  
> → Google callback về backend  
> → Backend xác minh state  
> → Lưu token hoặc secret reference phía máy chủ  
> → Frontend chỉ nhận trạng thái kết nối

- Thiết kế được luồng tạo Google Calendar event và yêu cầu conference data.
- Xác định các trạng thái đồng bộ:

> NOT_REQUESTED  
> PENDING  
> READY  
> FAILED_RETRYABLE  
> ACTION_REQUIRED

- Xác định chỉ hiển thị liên kết Google Meet khi trạng thái là `READY`.
- Xác định Google Meet artifact không phải lúc nào cũng tồn tại.
- Xác định cần fallback bằng upload hoặc capture có consent.
- Xác định Google Meet Add-on không có backend hoặc database riêng.

## Khó khăn gặp phải

- Google integration phụ thuộc tài khoản, quyền OAuth và chính sách Google Workspace.
- Liên kết Google Meet có thể chưa sẵn sàng ngay sau khi Calendar event được tạo.
- Recording hoặc transcript có thể không tồn tại.
- Retry không đúng có thể tạo nhiều Google Calendar event.
- Google access token và refresh token là dữ liệu nhạy cảm.
- Không thể xem contract hoặc adapter skeleton là bằng chứng tích hợp đã hoạt động.
- Việc kiểm thử phụ thuộc tài khoản Google phù hợp.

## Hướng xử lý

- Luôn tạo và lưu Meeting nội bộ trước khi gọi Google API.
- Sử dụng request ID riêng để chống tạo conference trùng.
- Lưu `googleEventId` để cập nhật event hiện có.
- Không tạo event mới chỉ vì request trước đó còn `PENDING`.
- Lưu trạng thái đồng bộ rõ ràng để frontend hiển thị.
- Lưu token phía máy chủ, không trả refresh token về trình duyệt.
- Không ghi token vào CloudWatch Logs.
- Dùng upload hoặc consent-based capture làm fallback.
- Giới hạn số lần retry và sử dụng exponential backoff khi phù hợp.
- Chỉ xác nhận integration hoàn thành khi có test thực tế và log kiểm chứng.

## Kế hoạch tuần tiếp theo

- Học phần Security trong First Cloud Journey.
- Tìm hiểu IAM Permission Boundary và IAM Policy Conditions.
- Tìm hiểu Security Hub, WAF, KMS và Secrets Manager.
- Thiết kế Reminder và Notification.
- Phân tích Meeting Minutes, Action Item, Task và Dashboard.
- Thiết kế S3 presigned upload.
- Xác định Consent, Recording metadata và AIJob.
- Rà soát mô hình DynamoDB v2 và kiến trúc AWS.

---