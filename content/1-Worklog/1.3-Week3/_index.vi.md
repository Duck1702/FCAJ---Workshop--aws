---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

# TUẦN 3: NHÓM, THÀNH VIÊN VÀ PHÂN QUYỀN

## Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | - Tìm hiểu Amazon DynamoDB: partition key, sort key, GSI, Query, Scan và on-demand capacity.<br>- Tìm hiểu cách thiết kế dữ liệu theo access pattern.<br>- Đối chiếu mô hình dữ liệu hiện tại của CampusMeet. | 29/06/2026 | 29/06/2026 | [DynamoDB](https://cloudjourney.awsstudygroup.com/vi/)<br>[CampusMeet DynamoDB v2](https://github.com/Ngct253/CampusMeet/blob/main/docs/dynamodb-data-model.md) |
| 3 | - Tìm hiểu Amazon CloudFront và cơ chế phân phối nội dung từ S3.<br>- Tìm hiểu Origin Access Control và private S3 origin.<br>- Phân tích kiến trúc frontend mục tiêu của CampusMeet. | 30/06/2026 | 30/06/2026 | [Amazon CloudFront](https://cloudjourney.awsstudygroup.com/vi/1-explore/)<br>[CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture) |
| 4 | - Tìm hiểu CloudWatch Logs, Metrics, Dashboard và Alarm.<br>- Phân tích các metric cần theo dõi cho API, Lambda và authorization.<br>- Xác định yêu cầu log không chứa token hoặc secret. | 01/07/2026 | 01/07/2026 | [Amazon CloudWatch](https://cloudjourney.awsstudygroup.com/vi/1-explore/) |
| 5 | - Phân tích entity Group, Membership và Invitation.<br>- Xác định Member và Group Administrator.<br>- Thiết kế luồng tạo group, creator trở thành Admin và luồng chấp nhận lời mời.<br>- Xác định các trường hợp phải trả `403`. | 02/07/2026 | 02/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md) |
| 6 | - Thiết kế authorization boundary dùng chung.<br>- Thiết kế access pattern cho bảng `collaboration`.<br>- Xác định transaction tạo group, membership và audit event.<br>- Chuẩn bị test cho quyền truy cập chéo nhóm và Admin cuối cùng.<br>- Tổng kết Worklog tuần 3. | 03/07/2026 | 03/07/2026 | [CampusMeet Team Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-trien-khai-nhom.md) |


## Kết quả đạt được tuần 3

- Hiểu cách DynamoDB tổ chức dữ liệu bằng partition key và sort key.
- Phân biệt được Query và Scan.
- Hiểu vai trò của Global Secondary Index.
- Hiểu lợi ích của chế độ On-Demand đối với hệ thống có lưu lượng chưa ổn định.
- Hiểu cách CloudFront phân phối frontend từ Amazon S3.
- Hiểu vai trò của Origin Access Control trong việc bảo vệ S3 frontend bucket.
- Nắm được các thành phần cơ bản của Amazon CloudWatch.
- Xác định được các thực thể Group, Membership, Invitation và Audit Event.
- Xác định được hai vai trò Member và Group Administrator.
- Xác định người tạo nhóm mặc định trở thành Group Administrator.
- Thiết kế được luồng kiểm tra quyền dùng chung:

> JWT hợp lệ  
> → Lấy danh tính người dùng từ token  
> → Xác định group liên quan  
> → Kiểm tra active membership  
> → Kiểm tra role hiện tại  
> → Kiểm tra quyền trên tài nguyên  
> → Thực hiện nghiệp vụ

- Thiết kế transaction tạo nhóm gồm Group, Creator Membership và Audit Event.
- Xác định các trường hợp kiểm thử truy cập chéo nhóm.
- Xác định quy tắc không được xóa hoặc hạ quyền quản trị viên cuối cùng.

## Khó khăn gặp phải

- DynamoDB cần được thiết kế theo access pattern, khác với cách mỗi thực thể tương ứng một bảng.
- Membership là thành phần phụ thuộc của nhiều module trong CampusMeet.
- Phân quyền ở frontend không đủ để bảo vệ dữ liệu.
- Nếu không sử dụng transaction, hệ thống có thể tạo Group nhưng thiếu Membership của quản trị viên.
- Retry request có thể tạo dữ liệu trùng nếu không có idempotency.
- Việc xóa quản trị viên cuối cùng có thể khiến nhóm không còn người quản lý.
- Handler Group và Membership vẫn cần application service và DynamoDB repository thật.

## Hướng xử lý

- Sử dụng bảng `collaboration` cho Group, Membership, Invitation và Audit Event.
- Sử dụng GSI để truy vấn danh sách nhóm của một người dùng.
- Không sử dụng Scan trong request nghiệp vụ thông thường.
- Dùng DynamoDB Transaction khi tạo Group và Membership ban đầu.
- Dùng conditional write và idempotency cho các mutation có thể được gửi lại.
- Xây dựng authorization boundary dùng chung.
- Backend lấy `userId` từ JWT, không tin `userId` hoặc role do frontend gửi.
- Kiểm tra active membership trước khi đọc hoặc thay đổi dữ liệu.
- Viết kiểm thử xác nhận người ngoài nhóm nhận `403 Forbidden`.
- Chỉ đánh dấu module hoàn thành khi repository thật và integration test hoạt động.

## Kế hoạch tuần tiếp theo

- Tìm hiểu AWS Lambda và Amazon API Gateway.
- Tìm hiểu AWS SAM và CloudFormation.
- Phân tích Meeting, Agenda, Attendee và Organizer.
- Xác định vòng đời của cuộc họp.
- Thiết kế API tạo, sửa, hủy và liệt kê cuộc họp.
- Thiết kế access pattern cho dữ liệu cuộc họp.
- Áp dụng authorization boundary vào Meeting API.

---
