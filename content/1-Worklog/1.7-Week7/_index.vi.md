---
title: "Worklog Tuần 7"
date: 2026-07-31
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

# TUẦN 7: AI VERTICAL SLICE VÀ VẬN HÀNH

## Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|---|:---:|:---:|---|
| 2 | - Tìm hiểu Reliability Pillar trong AWS Well-Architected Framework.<br>- Tìm hiểu fault tolerance, retry, timeout, idempotency và khả năng phục hồi khi một thành phần gặp lỗi.<br>- Rà soát phạm vi AI vertical slice của CampusMeet.<br>- Xác định luồng tổng quát từ cuộc họp, consent, live transcription đến RAG và task proposal. | 27/07/2026 | 27/07/2026 | [AWS Optimization](https://cloudjourney.awsstudygroup.com/vi/3-optimize/)<br>[CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md) |
| 3 | - Tìm hiểu Performance Efficiency và cách lựa chọn cấu hình phù hợp cho workload serverless.<br>- Thực hiện spike live transcription tiếng Việt với Amazon Transcribe Streaming.<br>- Phân tích luồng tab audio, PCM, WebSocket, partial result, final result, stop và reconnect.<br>- Xác định quy tắc chỉ lưu final segment và sử dụng nhãn người nói ẩn danh `Speaker N`. | 28/07/2026 | 28/07/2026 | [AWS Optimization](https://cloudjourney.awsstudygroup.com/vi/3-optimize/)<br>[CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md) |
| 4 | - Tìm hiểu Cost Optimization và cách kiểm soát chi phí cho Transcribe, Bedrock, S3 và Lambda.<br>- Hoàn thiện thiết kế luồng S3 presigned upload.<br>- Xác định Attachment status, MIME allowlist, giới hạn dung lượng và checksum.<br>- Thiết kế complete-upload verification bằng `HeadObject` và nguyên tắc không tạo AIJob trùng khi retry. | 29/07/2026 | 29/07/2026 | [AWS Optimization](https://cloudjourney.awsstudygroup.com/vi/3-optimize/)<br>[CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md)<br>[CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture) |
| 5 | - Thiết kế AIJob bất đồng bộ bằng AWS Step Functions và AI Worker.<br>- Thiết kế normalized source, KnowledgeSource và quy trình ingestion.<br>- Phân tích Amazon Bedrock Knowledge Bases và S3 Vectors.<br>- Thiết kế RAG theo current meeting, selected meetings và whole group.<br>- Xác định metadata filter bắt buộc gồm `groupId`, `approved` và danh sách `meetingId` khi cần. | 30/07/2026 | 30/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md)<br>[CampusMeet DynamoDB v2](https://github.com/Ngct253/CampusMeet/blob/main/docs/dynamodb-data-model.md) |
| 6 | - Thiết kế luồng chatbot hỏi đáp và tóm tắt cho thành viên vào trễ.<br>- Thiết kế citation trỏ về meeting, transcript, timestamp hoặc tài liệu nội bộ.<br>- Thiết kế bản nháp biên bản và đề xuất Task có bước xem trước, xác nhận và idempotency.<br>- Chuẩn bị test chống truy xuất chéo nhóm, thiếu consent, segment trùng và AIJob trùng.<br>- Tổng hợp kết quả và hoàn thành Worklog tuần 7. | 31/07/2026 | 31/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md)<br>[CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture) |
## Kết quả đạt được tuần 7

- Tìm hiểu được các nguyên tắc cơ bản của Reliability, Performance Efficiency và Cost Optimization trên AWS.
- Hiểu vai trò của retry, timeout, idempotency và fault tolerance trong hệ thống serverless.
- Chốt được AI vertical slice của CampusMeet:

> Meeting và active membership  
> → Consent và quyền capture  
> → Live transcription  
> → Final transcript segment  
> → AIJob bất đồng bộ  
> → Normalized approved source  
> → Knowledge Base và S3 Vectors  
> → RAG có citation  
> → Minutes và Task proposal  
> → Người dùng xem trước và xác nhận

- Xác định live transcription phải chạy sau hành động cho phép rõ ràng của người dùng.
- Xác định signed streaming connection chỉ có hiệu lực trong thời gian ngắn.
- Xác định partial transcript chỉ được hiển thị tạm thời, không lưu vào database và không dùng để tạo citation.
- Xác định final transcript segment được lưu theo `sessionId` và `sequence` để tránh dữ liệu trùng.
- Xác định nhãn người nói chỉ sử dụng dạng ẩn danh như `Speaker 1`, `Speaker 2` và không tự động ánh xạ sang danh tính thành viên.
- Thiết kế được luồng upload an toàn:

> Frontend yêu cầu upload  
> → API kiểm tra JWT và active membership  
> → Tạo Attachment `PENDING_UPLOAD`  
> → Trả presigned URL ngắn hạn  
> → Browser upload trực tiếp lên Amazon S3  
> → Frontend gọi complete-upload  
> → Backend sử dụng `HeadObject` để xác minh  
> → Kiểm tra MIME, dung lượng, checksum và object key  
> → Attachment chuyển sang `READY` hoặc `REJECTED`  
> → Tạo đúng một AIJob khi phù hợp

- Xác định file nhị phân không được truyền qua API Gateway hoặc Lambda payload.
- Thiết kế được các trạng thái AIJob:

> `QUEUED`  
> `PROCESSING`  
> `COMPLETED`  
> `FAILED`  
> `CANCELLED`

- Xác định AIJob cần chạy bất đồng bộ để API không phải chờ tác vụ dài.
- Thiết kế normalized source và metadata phục vụ ingestion.
- Xác định chỉ các nguồn có trạng thái `approved=true` mới được đưa vào Knowledge Base.
- Xác định ba phạm vi truy vấn của RAG:

> Current meeting  
> Selected meetings  
> Whole group

- Xác định mọi truy vấn RAG phải kiểm tra active membership và áp dụng filter `groupId` trước khi gửi dữ liệu cho mô hình.
- Xác định truy vấn theo một số cuộc họp phải kiểm tra toàn bộ `meetingId` thuộc đúng nhóm.
- Xác định khi không có đủ nguồn, API phải trả `insufficientContext=true` thay vì tạo câu trả lời không có căn cứ.
- Thiết kế citation chứa tên cuộc họp, loại nguồn, timestamp hoặc tên tài liệu và liên kết nội bộ.
- Thiết kế bản nháp Meeting Minutes gồm Summary, Decision và Action Item.
- Xác định AI chỉ tạo Task proposal; Task thật chỉ được tạo sau khi người dùng xem trước và xác nhận.
- Chuẩn bị các trường hợp kiểm thử bảo mật, retry và idempotency cho AI vertical slice.

## Khó khăn gặp phải

- Live transcription trên trình duyệt phụ thuộc quyền capture và hành động cho phép của người dùng.
- Việc chuyển đổi tab audio thành PCM đúng sample rate và encoding cần được kiểm thử thực tế.
- Kết nối WebSocket có thể bị gián đoạn và cần hỗ trợ reconnect an toàn.
- Partial và final transcript có thể bị gửi lại hoặc đến không đúng thứ tự.
- Nội dung transcript, recording và tài liệu tải lên có thể chứa dữ liệu nhạy cảm.
- AIJob là tác vụ dài nên không phù hợp với request đồng bộ thông thường.
- Việc xây dựng Knowledge Base và S3 Vectors cần cấu hình đúng quyền truy cập và metadata.
- RAG nhiều cuộc họp có nguy cơ truy xuất dữ liệu chéo nhóm nếu filter được áp dụng không đúng.
- Citation phải hữu ích cho người dùng nhưng không được làm lộ raw S3 key hoặc presigned URL.
- Chi phí Transcribe, Bedrock và ingestion có thể tăng khi không có quota.
- Các chức năng AI phụ thuộc vào Group, Membership, Meeting và Task API đã hoạt động đúng.
- Không thể xem sơ đồ kiến trúc hoặc CloudFormation template là bằng chứng toàn bộ AI pipeline đã chạy thành công.

## Hướng xử lý

- Thực hiện spike live transcription trước khi triển khai toàn bộ pipeline.
- Chỉ bắt đầu capture sau khi người dùng đồng ý và thực hiện hành động rõ ràng.
- Không tự động chuyển sang nguồn microphone hoặc audio khác khi tab audio không khả dụng.
- Chỉ lưu final transcript segment.
- Dùng `sessionId`, `sequence` hoặc `ResultId` để chống ghi segment trùng.
- Tạo lại signed streaming URL mới khi reconnect thay vì tái sử dụng URL hết hạn.
- Sử dụng presigned URL để browser upload trực tiếp lên Amazon S3.
- Dùng `HeadObject` để xác minh metadata sau khi upload.
- Áp dụng MIME allowlist, giới hạn dung lượng và checksum.
- Dùng AIJob và Step Functions cho các tác vụ parse, transcription, normalize và ingestion.
- Dùng fake provider hoặc mock có kiểm soát khi dịch vụ AI thật chưa sẵn sàng.
- Bắt buộc filter `groupId` và `approved=true` trước khi gọi mô hình.
- Không retrieve toàn cục rồi mới lọc dữ liệu sau.
- Chuẩn hóa citation về URI nội bộ của CampusMeet.
- Không ghi audio, transcript đầy đủ, prompt, token, secret hoặc presigned URL còn hiệu lực vào CloudWatch Logs.
- Áp dụng quota cho số phút transcription, số lần ingestion và số token AI.
- Dùng CloudWatch Metrics và Alarms để theo dõi lỗi, thời gian xử lý và chi phí ước tính.
- Chỉ xác nhận AI vertical slice hoàn thành khi có code, test, deployment output, smoke test và CloudWatch Logs.

## Kế hoạch tuần tiếp theo

- Hoàn thiện demo AI vertical slice đầu cuối.
- Kiểm thử RAG cho current meeting, selected meetings và whole group.
- Kiểm thử bắt buộc filter `groupId` và `approved=true`.
- Kiểm thử người dùng Group B không truy xuất được dữ liệu của Group A.
- Kiểm thử transcript segment và AIJob không bị tạo trùng khi retry.
- Kiểm thử Task proposal chỉ tạo Task sau khi người dùng xác nhận.
- Kiểm thử prompt injection và nội dung tài liệu không đáng tin cậy.
- Hoàn thiện CloudWatch Metrics, Alarms và SNS notification.
- Tổng hợp chi phí ước tính cho Transcribe, Bedrock, S3, Lambda và DynamoDB.
- Rà soát retention, cleanup và backup dữ liệu.
- Hoàn thiện workshop, tài liệu triển khai và nội dung trình bày.
- Chuẩn bị demo và báo cáo tổng kết tuần 8.