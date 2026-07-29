---
title: " Bản đề xuất"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
#  DỰ ÁN CAMPUSMEET

## Hệ thống quản lý cuộc họp và công việc nhóm tích hợp Google Meet trên nền tảng AWS Serverless

---

# MỤC LỤC

1. Bối cảnh và động lực  
2. Tóm tắt giải pháp  
3. Mục tiêu và tiêu chí thành công  
4. Người dùng và phân quyền  
5. Phạm vi dự án  
6. Kiến trúc giải pháp AWS  
7. Thiết kế kỹ thuật  
8. Bảo mật và quyền riêng tư  
9. Độ tin cậy, giám sát và kiểm soát chi phí  
10. Kế hoạch thực hiện  
11. Hoạt động và sản phẩm bàn giao  
12. Phân công nhóm  
13. Rủi ro và phương án xử lý  
14. Lộ trình từ MVP đến môi trường hoàn chỉnh  
15. Điều kiện nghiệm thu  
16. Kết quả mong đợi  

---

# 1. BỐI CẢNH VÀ ĐỘNG LỰC

## 1.1. Bối cảnh

Các nhóm học tập, nhóm đồ án và nhóm dự án nhỏ thường sử dụng nhiều công cụ khác nhau để tổ chức công việc:

- Google Calendar để lưu lịch.
- Google Meet để thực hiện cuộc họp trực tuyến.
- Ứng dụng nhắn tin để gửi lời nhắc và trao đổi nhanh.
- Tài liệu riêng để ghi biên bản.
- Bảng công việc hoặc tin nhắn để theo dõi nhiệm vụ sau họp.

Việc sử dụng nhiều công cụ rời rạc làm cho thông tin bị phân tán. Thành viên khó xác định đâu là lịch họp chính thức, nội dung nào đã được thống nhất, ai chịu trách nhiệm cho từng công việc và thời hạn hoàn thành là khi nào.

Đặc biệt, quy trình sau cuộc họp thường không được quản lý chặt chẽ. Các quyết định và việc cần làm có thể chỉ tồn tại trong ghi chú hoặc tin nhắn, không được chuyển thành công việc có người phụ trách, mức ưu tiên, thời hạn và trạng thái theo dõi.

## 1.2. Vấn đề cần giải quyết

| Vấn đề | Hệ quả |
|---|---|
| Lịch, biên bản và công việc nằm ở nhiều nơi | Thành viên mất thời gian tìm kiếm và dễ bỏ sót thông tin |
| Tạo sự kiện Calendar và liên kết Meet thủ công | Người tổ chức phải thực hiện nhiều thao tác lặp lại |
| Không có quy trình thống nhất sau cuộc họp | Quyết định không được chuyển thành hành động cụ thể |
| Không có bảng tổng quan chung | Khó theo dõi cuộc họp sắp tới, công việc đến hạn và quá hạn |
| Thành viên vào họp trễ hoặc bỏ lỡ cuộc họp | Khó nắm lại nội dung đã trao đổi |
| Tài liệu và transcript dài | Người dùng khó tìm nhanh bằng chứng cho một kết luận |
| AI có thể tạo thông tin thiếu căn cứ | Biên bản và công việc có nguy cơ sai nếu không có citation và xác nhận |
| Dữ liệu cuộc họp có tính nhạy cảm | Cần consent, phân quyền, retention và quy trình xóa rõ ràng |

## 1.3. Động lực xây dựng

CampusMeet được đề xuất nhằm tạo ra một không gian làm việc nhẹ, tập trung và có kiểm soát cho toàn bộ vòng đời cuộc họp:

```text
Chuẩn bị
→ Lập lịch
→ Nhắc lịch
→ Họp trên Google Meet
→ Live transcription
→ Biên bản và quyết định
→ Công việc
→ Theo dõi tiến độ
→ Hỏi đáp có citation
```

CampusMeet không thay thế Google Meet và không tự xây dựng chức năng gọi video. Google Meet tiếp tục là nền tảng diễn ra cuộc họp; CampusMeet quản lý quy trình và dữ liệu liên quan trước, trong và sau cuộc họp.

---

# 2. TÓM TẮT GIẢI PHÁP

CampusMeet là ứng dụng web độc lập được triển khai trên AWS theo kiến trúc managed serverless.

Hệ thống cho phép:

1. Đăng ký và đăng nhập bằng Amazon Cognito.
2. Tạo nhóm và quản lý thành viên.
3. Thiết lập vai trò Thành viên hoặc Quản trị viên theo từng nhóm.
4. Tạo, sửa và hủy cuộc họp.
5. Quản lý agenda, người tổ chức và người tham dự.
6. Kết nối Google qua OAuth.
7. Tạo sự kiện Google Calendar và yêu cầu liên kết Google Meet.
8. Theo dõi trạng thái đồng bộ Calendar/Meet.
9. Thiết lập tối đa ba mốc nhắc cho một cuộc họp.
10. Gửi thông báo trong ứng dụng và email bổ sung.
11. Ghi biên bản, quyết định và action item.
12. Chuyển action item thành công việc có người phụ trách.
13. Theo dõi công việc qua bảng tổng quan cá nhân và nhóm.
14. Upload tài liệu hoặc audio trực tiếp lên S3 bằng presigned URL.
15. Chạy live transcription sau khi người dùng đồng ý và cấp quyền capture.
16. Hiển thị transcript có timestamp, confidence, ngôn ngữ và nhãn `Speaker N`.
17. Cho phép người có quyền chỉnh sửa và duyệt transcript.
18. Hỏi đáp trên tài liệu, transcript và biên bản với citation.
19. Hỏi đáp trong một cuộc họp, nhiều cuộc họp được chọn hoặc toàn bộ cuộc họp của cùng một nhóm.
20. Sinh bản nháp biên bản và task proposal từ transcript.
21. Yêu cầu người dùng xem trước, bổ sung thông tin và xác nhận trước khi tạo task chính thức.
22. Theo dõi log, metric, alarm và chi phí sử dụng AWS.

CampusMeet Web là giao diện chính. Google Meet Add-on, nếu được triển khai, chỉ là một side panel tải route của CampusMeet và gọi lại cùng API. Add-on không có API đặc quyền, business rule hoặc database riêng.

---

# 3. MỤC TIÊU VÀ TIÊU CHÍ THÀNH CÔNG

## 3.1. Mục tiêu nghiệp vụ

| Mục tiêu | Kết quả mong muốn |
|---|---|
| Quản lý tập trung | Nhóm, thành viên, cuộc họp, biên bản và công việc được quản lý trong cùng hệ thống |
| Giảm thao tác thủ công | Hỗ trợ tạo Calendar event và yêu cầu liên kết Meet từ CampusMeet |
| Chuẩn hóa quy trình sau họp | Quyết định và action item được chuyển thành công việc có người phụ trách |
| Tăng khả năng theo dõi | Thành viên và quản trị viên nhìn thấy công việc đến hạn, quá hạn và tiến độ |
| Hỗ trợ người vào trễ | Tóm tắt nội dung đã diễn ra dựa trên live transcript |
| Tìm kiếm tri thức nhóm | Hỏi đáp trên nhiều cuộc họp trong cùng một nhóm |
| Bảo đảm AI có căn cứ | Mọi kết quả AI quan trọng có citation hoặc thông báo không đủ dữ liệu |
| Thể hiện dự án AWS hoàn chỉnh | Có IaC, logging, monitoring, alarm, cost control và cleanup |

## 3.2. Mục tiêu kỹ thuật

- Xây dựng hệ thống theo kiến trúc serverless.
- Không sử dụng máy chủ chạy liên tục trong MVP.
- Không yêu cầu EC2, ECS, EKS, RDS, VPC hoặc NAT Gateway.
- Xử lý file và AI bất đồng bộ.
- Không đưa binary lớn qua API Gateway hoặc Lambda payload.
- Kiểm soát truy cập theo `groupId`, membership, role và ACL.
- Không truy xuất dữ liệu AI chéo nhóm.
- Sử dụng Infrastructure as Code để triển khai lại hệ thống.
- Có cơ chế retry, idempotency và quan sát lỗi.
- Hỗ trợ tiếng Việt là ngôn ngữ benchmark chính của transcription.

## 3.3. Tiêu chí thành công của MVP

MVP được xem là thành công khi đáp ứng được các điều kiện sau:

1. Người dùng đăng ký, xác nhận tài khoản, đăng nhập và đăng xuất được.
2. Người dùng tạo nhóm và trở thành Quản trị viên nhóm.
3. Quản trị viên mời, xem và quản lý thành viên.
4. Người ngoài nhóm không thể truy cập dữ liệu của nhóm.
5. Quản trị viên tạo, sửa hoặc hủy cuộc họp.
6. Cuộc họp được lưu nội bộ kể cả khi tích hợp Google chưa thành công.
7. Trạng thái đồng bộ Google được hiển thị minh bạch.
8. Link Meet chỉ được hiển thị khi trạng thái đồng bộ là `READY`.
9. Hệ thống tạo và thực thi đúng các mốc nhắc lịch.
10. Reminder luôn tạo thông báo trong ứng dụng.
11. Lỗi gửi email không làm mất thông báo trong ứng dụng.
12. Biên bản có thể tạo action item và công việc.
13. Người phụ trách cập nhật công việc qua `TODO`, `DOING` và `DONE`.
14. Bảng tổng quan hiển thị cuộc họp và công việc cần chú ý.
15. File được upload trực tiếp lên S3 và được backend xác minh.
16. Live transcription hoạt động sau consent/cấp quyền.
17. Transcript có timestamp, confidence, language code và `Speaker N`.
18. Người có quyền chỉnh sửa và duyệt transcript.
19. Chatbot trả citation hoặc `insufficientContext=true`.
20. RAG không trả dữ liệu của nhóm khác.
21. AI chỉ tạo biên bản nháp và task proposal.
22. Task chỉ được tạo sau khi người có quyền xác nhận.
23. Có CloudWatch logs, metric và ít nhất một Alarm gửi qua SNS.
24. Có quy trình cleanup và kiểm tra chi phí sau demo.

---

# 4. NGƯỜI DÙNG VÀ PHÂN QUYỀN

## 4.1. Đối tượng sử dụng

CampusMeet hướng đến:

- Nhóm học tập.
- Nhóm làm đồ án môn học.
- Nhóm nghiên cứu sinh viên.
- Nhóm dự án nhỏ.
- Nhóm cộng tác có nhu cầu quản lý cuộc họp và nhiệm vụ ở mức nhẹ.

## 4.2. Vai trò nghiệp vụ

CampusMeet có hai vai trò chính theo từng nhóm:

### Thành viên nhóm

Thành viên có thể:

- Xem thông tin nhóm mà mình tham gia.
- Xem lịch họp và liên kết Meet hợp lệ.
- Xem tài liệu, transcript và biên bản mà mình có quyền.
- Chấp nhận hoặc từ chối lời mời.
- Cập nhật trạng thái công việc được giao.
- Xem bảng tổng quan cá nhân.
- Sử dụng chatbot trong phạm vi dữ liệu được phép.
- Gửi yêu cầu upload hoặc live transcription khi chính sách nhóm cho phép.

### Quản trị viên nhóm

Quản trị viên có toàn bộ quyền của Thành viên và có thể:

- Mời hoặc xóa thành viên.
- Tạo, sửa và hủy cuộc họp.
- Chọn người tổ chức và người tham dự.
- Kết nối Google khi là người tổ chức.
- Tạo và cập nhật biên bản.
- Tạo, giao và quản lý công việc.
- Xem bảng tổng quan toàn nhóm.
- Xác nhận task proposal do AI tạo.
- Yêu cầu phân tích tiến độ cấp nhóm.
- Quản lý các cấu hình liên quan đến retention và tích hợp.

Một người có thể là Quản trị viên của nhóm này nhưng chỉ là Thành viên của nhóm khác.

## 4.3. Nguyên tắc phân quyền

Amazon Cognito xác thực người dùng, nhưng Cognito không thay thế phân quyền nghiệp vụ.

Mỗi API liên quan đến nhóm phải thực hiện:

```text
JWT hợp lệ
→ Lấy user identity từ token
→ Xác định group hoặc meeting nội bộ
→ Kiểm tra active membership
→ Kiểm tra role
→ Kiểm tra quyền trên tài nguyên
→ Thực hiện thao tác
```

Backend không tin `userId`, role hoặc `groupId` do frontend tự khai báo.

---

# 5. PHẠM VI DỰ ÁN

## 5.1. Phạm vi MVP bắt buộc

### Xác thực và hồ sơ

- Đăng ký.
- Xác nhận email.
- Đăng nhập.
- Đăng xuất.
- Bảo vệ route.
- Hồ sơ người dùng.
- Múi giờ và tùy chọn thông báo.

### Nhóm và thành viên

- Tạo nhóm.
- Danh sách nhóm của người dùng.
- Chi tiết nhóm.
- Lời mời tham gia nhóm.
- Chấp nhận hoặc từ chối lời mời.
- Quản lý thành viên.
- Bảo vệ Quản trị viên cuối cùng.
- Kiểm tra quyền chéo nhóm.

### Quản lý cuộc họp

- Tạo cuộc họp nháp hoặc đã lên lịch.
- Tiêu đề và mô tả.
- Thời gian bắt đầu và kết thúc.
- Agenda.
- Người tổ chức.
- Người tham dự.
- Sửa cuộc họp.
- Hủy cuộc họp.
- Danh sách và timeline cuộc họp.
- Trạng thái vòng đời cuộc họp.

### Google Calendar và Google Meet

- OAuth Authorization Code Flow.
- Kết nối và ngắt kết nối Google.
- Tạo Google Calendar event.
- Gửi `conferenceData.createRequest`.
- Sử dụng `requestId` riêng để chống tạo trùng.
- Lưu `googleEventId`.
- Theo dõi trạng thái:

```text
NOT_REQUESTED
PENDING
READY
FAILED_RETRYABLE
ACTION_REQUIRED
```

- Cập nhật hoặc hủy event khi cuộc họp thay đổi.
- Đồng bộ artifact hậu họp khi thực sự khả dụng.
- Upload/capture fallback khi artifact không khả dụng.

### Nhắc lịch và thông báo

- Tối đa ba mốc nhắc cho mỗi cuộc họp.
- EventBridge Scheduler one-time schedule.
- Reminder Lambda.
- Thông báo trong ứng dụng.
- Email SES là kênh bổ sung.
- Không gửi nhắc cho cuộc họp đã hủy.
- Retry và idempotency.

### Biên bản và công việc

- Tóm tắt cuộc họp.
- Nội dung thảo luận.
- Quyết định.
- Action item.
- Tạo task từ action item.
- Người phụ trách.
- Mức ưu tiên.
- Hạn hoàn thành tùy chọn.
- Trạng thái `TODO`, `DOING`, `DONE`.
- Lịch sử thay đổi task.
- Bảng tổng quan cá nhân.
- Bảng tổng quan nhóm.

### Vận hành

- Log có cấu trúc.
- `requestId`.
- CloudWatch metrics.
- CloudWatch Alarm.
- SNS notification.
- AWS Budgets.
- IaC.
- Smoke test.
- Checklist cleanup.

## 5.2. Phạm vi AI MVP bắt buộc

### Upload an toàn

- Thành viên được phép upload tài liệu hoặc audio.
- API cấp presigned URL.
- Browser upload trực tiếp lên S3.
- Backend xác minh object bằng `HeadObject`.
- Kiểm tra MIME, kích thước, checksum và object key.
- Chỉ nguồn ở trạng thái `READY` mới được đưa vào AI.
- Binary không đi qua API Gateway hoặc Lambda payload.

### Live transcription

- Chỉ bắt đầu sau thao tác consent và cấp quyền capture rõ ràng.
- Chạy nền trong phiên họp.
- Hỗ trợ ngôn ngữ được cấu hình hoặc chế độ tự phát hiện khi provider hỗ trợ.
- Ưu tiên kiểm thử chất lượng tiếng Việt `vi-VN`.
- Partial segment chỉ hiển thị tạm thời.
- Chỉ final segment mới được lưu và sử dụng ở downstream.
- Segment chứa:

```text
startMs
endMs
text
confidence
languageCode
speakerLabel
sequence
version
```

- `speakerLabel` chỉ có dạng `Speaker 1`, `Speaker 2`,...
- Hệ thống không tự nhận diện danh tính speaker.
- Retry segment phải idempotent theo session và sequence.
- Khi stream lỗi, hệ thống thông báo thiếu dữ liệu thay vì suy đoán nội dung.

### Transcript

- Lưu transcript và final segments.
- Phát lại theo timestamp khi có audio hợp lệ.
- Chỉnh sửa nội dung.
- Chỉnh sửa language hoặc speaker label.
- Optimistic version.
- Lưu lịch sử thay đổi.
- Cho phép duyệt transcript trước khi ingestion.

### AIJob

Mọi công việc dài sử dụng trạng thái:

```text
QUEUED
PROCESSING
COMPLETED
FAILED
CANCELLED
```

API trả `202 Accepted` cùng `aiJobId`. Client theo dõi tiến trình thay vì giữ HTTP request mở trong thời gian dài.

### RAG và citation

CampusMeet hỗ trợ ba phạm vi:

```text
CURRENT_MEETING
SELECTED_MEETINGS
WHOLE_GROUP
```

Quy tắc retrieval:

1. Xác thực JWT.
2. Kiểm tra active membership.
3. Xác định `groupId` được phép.
4. Kiểm tra mọi meeting được chọn thuộc cùng group.
5. Lọc bắt buộc `groupId` và `approved=true`.
6. Thêm tập `meetingId` khi người dùng chọn phạm vi.
7. Retrieval trước model, không retrieve toàn cục rồi mới lọc.
8. Không trả raw S3 key hoặc presigned URL trong citation.
9. Khi không đủ nguồn, trả `insufficientContext=true`.

Citation phải có khả năng chỉ ra:

- Tên cuộc họp.
- Ngày cuộc họp.
- Loại nguồn.
- Tên tài liệu hoặc biên bản.
- `Speaker N`.
- Timestamp hoặc segment.
- Đường dẫn nội bộ mà người dùng có quyền mở.

### Biên bản và task proposal

AI có thể tạo:

- Bản nháp biên bản.
- Chủ đề đã thảo luận.
- Quyết định thực sự được nêu.
- Action item.
- Task proposal có citation.

AI không được tự bịa:

- Người phụ trách.
- Deadline.
- Mức ưu tiên.
- Nội dung không có trong nguồn.

Khi thiếu trường bắt buộc, proposal phải trả `missingFields`.

Quy trình thực thi:

```text
AI sinh proposal
→ Backend kiểm tra schema
→ Frontend hiển thị preview
→ Người dùng sửa/bổ sung
→ Người có quyền xác nhận
→ Backend kiểm tra lại membership và role
→ Task API chuẩn tạo task idempotent
→ Ghi audit log
```

### Phân tích tiến độ nhóm

AI chỉ diễn giải số liệu được backend tính từ task và meeting trong một nhóm.

AI không được:

- Chấm điểm thành viên.
- Xếp hạng thành viên.
- Suy diễn thái độ hoặc năng lực cá nhân.
- Đổi trạng thái task.
- So sánh dữ liệu giữa các nhóm.

## 5.3. Hạng mục nên có

- Lời mời qua email hoặc liên kết.
- Nhật ký thao tác quan trọng.
- Lịch theo tuần hoặc tháng.
- CampusMeet Meet Add-on side panel.
- Đồng bộ recording hoặc transcript từ Google Meet khi khả dụng.
- Trợ lý chuẩn bị agenda và biểu mẫu có preview.
- Document Picture-in-Picture như progressive enhancement.

Các hạng mục này không được làm chậm luồng web chính.

## 5.4. Ngoài phạm vi MVP

CampusMeet không triển khai:

- Video call riêng.
- Audio call riêng.
- WebRTC hoặc TURN server.
- Chat thời gian thực giữa người dùng.
- Nhúng toàn bộ giao diện Google Meet vào CampusMeet.
- Tự ghi âm mà không có consent.
- Tự động nhận diện tên người nói.
- Voice biometrics.
- Phân tích video dài.
- Chấm điểm hoặc đánh giá cá nhân từ transcript.
- RAG chéo nhóm.
- AI mutation không qua xác nhận.
- Công cụ tùy ý do model tự chọn.
- Public Google Marketplace release trong baseline tám tuần.
- Kiến trúc dùng EC2, RDS, NAT Gateway hoặc Kubernetes cho MVP.

---

# 6. KIẾN TRÚC GIẢI PHÁP AWS

## 6.1. Sơ đồ kiến trúc

![Kiến trúc AWS mục tiêu của CampusMeet](/FCAJ---Workshop--aws/images/2-Proposal/campusmeet-aws-architecture-Target%20MVP%20Architecture.drawio.png)


## 6.2. Kiến trúc tổng thể

```text
CampusMeet Web / Meet Add-on
        │
        ├── HTTPS → Amazon CloudFront
        │               │
        │               └── OAC → Private S3 Frontend Bucket
        │
        ├── Amazon Cognito User Pool
        │
        └── JWT → Amazon API Gateway HTTP API
                         │
                         ▼
                    API Lambda
                         │
        ┌────────────────┼────────────────────┐
        │                │                    │
        ▼                ▼                    ▼
   DynamoDB       S3 User Content      Google Adapters
   5 tables       Presigned Upload     Calendar / Meet
        │
        ├── EventBridge Scheduler
        │           │
        │           ▼
        │      Reminder Lambda
        │           ├── In-app Notification
        │           └── Amazon SES
        │
        └── AWS Step Functions
                    │
                    ▼
              AI Worker Lambda
                    ├── Amazon Transcribe
                    ├── Amazon Bedrock
                    ├── Bedrock Knowledge Bases
                    └── Amazon S3 Vectors

CloudWatch → CloudWatch Alarm → Amazon SNS
AWS Budgets → Cost Alert
GitHub Actions → AWS SAM / CloudFormation
```

## 6.3. Thành phần bên ngoài AWS

Các hệ thống ngoài AWS được giữ gọn:

- Người dùng và trình duyệt.
- Google OAuth.
- Google Calendar API.
- Google Meet REST API.
- Google Meet Add-ons SDK.
- Người nhận email.
- Người phụ trách nhận cảnh báo.
- Nhà cung cấp STT thay thế, chỉ khi benchmark yêu cầu.

CampusMeet không phụ thuộc Google Meet artifact để hoàn thành chức năng cốt lõi. Khi recording hoặc transcript của Google không tồn tại, hệ thống sử dụng upload hoặc capture đã được người dùng đồng ý.

## 6.4. Dịch vụ AWS và vai trò

| Dịch vụ | Vai trò |
|---|---|
| Amazon CloudFront | Phân phối frontend qua HTTPS và cache static assets |
| Amazon S3 Frontend | Lưu React build dưới dạng private origin |
| Origin Access Control | Cho CloudFront truy cập S3 mà không public bucket |
| Amazon Cognito | Đăng ký, đăng nhập và phát hành JWT |
| API Gateway HTTP API | Public HTTPS entry point và JWT authorizer |
| AWS Lambda API | Điều phối application service, authorization và nghiệp vụ |
| Amazon DynamoDB | Lưu dữ liệu nghiệp vụ theo năm miền truy cập |
| Amazon S3 User Content | Lưu tài liệu, audio và nguồn AI |
| EventBridge Scheduler | Tạo one-time reminder |
| Reminder Lambda | Ghi thông báo và gửi email bổ sung |
| Amazon SES | Gửi email khi cấu hình cho phép |
| AWS Step Functions | Điều phối AIJob dài và có nhiều bước |
| AI Worker Lambda | Parse, chuẩn hóa, STT, ingestion và generation |
| Amazon Transcribe | Provider STT đầu tiên, ưu tiên benchmark `vi-VN` |
| Amazon Bedrock | Sinh câu trả lời, biên bản và proposal có căn cứ |
| Bedrock Knowledge Bases | Quản lý retrieval trên nguồn đã duyệt |
| Amazon S3 Vectors | Lưu vector và metadata phục vụ retrieval |
| Secrets Manager/SSM | Lưu secret hoặc token reference phía máy chủ |
| Amazon CloudWatch | Log, metric, dashboard và alarm |
| Amazon SNS | Gửi cảnh báo vận hành |
| AWS Budgets | Theo dõi và cảnh báo chi phí |
| AWS SAM/CloudFormation | Quản lý hạ tầng bằng mã nguồn |
| GitHub Actions | Quality gate và quy trình deployment được kiểm soát |

## 6.5. Frontend

Frontend CampusMeet sử dụng:

- React 19.
- TypeScript.
- Vite.
- React Router.
- TanStack Query.
- AWS Amplify cho tích hợp Cognito.

Frontend chịu trách nhiệm:

- Hiển thị giao diện.
- Quản lý trạng thái client.
- Lấy JWT từ Cognito.
- Gọi API bằng bearer token.
- Upload trực tiếp lên S3 bằng presigned URL.
- Hiển thị live transcript.
- Hiển thị citation.
- Hiển thị preview trước mutation AI.

Frontend không:

- Truy cập DynamoDB trực tiếp.
- Chứa AWS access key.
- Chứa Google refresh token.
- Gọi Bedrock bằng credential dài hạn.
- Thực hiện phân quyền nghiệp vụ thay backend.

## 6.6. Backend

Backend sử dụng Node.js 22 và TypeScript trên AWS Lambda.

Luồng nội bộ:

```text
Handler
→ Middleware
→ Application Service
→ Domain Port
→ Repository hoặc Integration Adapter
```

Trách nhiệm:

- Parse request.
- Xác thực ngữ cảnh.
- Kiểm tra membership và role.
- Validate schema.
- Thực hiện business rule.
- Quản lý transaction và idempotency.
- Truy cập repository.
- Gọi Google adapter.
- Tạo schedule.
- Tạo presigned URL.
- Khởi tạo AIJob.
- Chuẩn hóa error response.

Handler không query DynamoDB trực tiếp và không chứa toàn bộ business workflow.

---

# 7. THIẾT KẾ KỸ THUẬT

## 7.1. Mô hình DynamoDB v2

CampusMeet sử dụng năm bảng vật lý:

| Bảng | Dữ liệu |
|---|---|
| `identity` | User, preference, Google integration reference, OAuth state và notification |
| `collaboration` | Group, membership, invitation và audit event |
| `meeting-data` | Meeting, attendee, agenda, minutes, reminder, attachment, recording, consent, live session, transcript và segment |
| `task-data` | Task, task history và các index dashboard |
| `ai-work` | AIJob, KnowledgeSource, conversation, message, citation, proposal và idempotency |

Tên bảng theo môi trường:

```text
campusmeet-<env>-identity
campusmeet-<env>-collaboration
campusmeet-<env>-meeting-data
campusmeet-<env>-task-data
campusmeet-<env>-ai-work
```

DynamoDB được thiết kế theo access pattern, không theo quy tắc một entity tương ứng một bảng.

Nguyên tắc:

- Composite `PK/SK`.
- Prefix theo entity.
- Sparse GSI.
- Query thay vì Scan.
- Conditional write.
- Transaction cho mutation nhiều item.
- Timestamp UTC theo ISO 8601.
- TTL chỉ cho dữ liệu tạm.
- Binary và normalized content nằm trong S3.
- Vector nằm trong Knowledge Base/S3 Vectors.
- Mọi item group-scoped giữ `groupId` để authorization và audit.

## 7.2. Trạng thái 17 bảng legacy

AWS account dev từng có 17 bảng được tạo trước khi data model được review.

Các bảng này:

- Không còn là source of truth.
- Không được dùng làm nền tảng cho repository mới.
- Không được xóa ngay.
- Phải được audit read-only.
- Phải backup/export nếu có dữ liệu.
- Chỉ được cleanup sau khi năm bảng v2 đã được triển khai, xác minh và smoke test.

Quy trình:

```text
Audit legacy
→ Xác định dữ liệu
→ Backup/export khi cần
→ Deploy 5 bảng v2
→ Implement repository
→ Smoke test
→ Xác minh không còn code dùng legacy
→ Review
→ Cleanup
```

## 7.3. Nhóm API chính

### Core API

```text
GET    /health
GET    /groups
POST   /groups
POST   /groups/:groupId/invitations
GET    /memberships
PATCH  /memberships
GET    /meetings
POST   /meetings
PATCH  /meetings
DELETE /meetings
GET    /minutes
POST   /minutes
GET    /tasks
POST   /tasks
PATCH  /tasks
GET    /dashboard
GET    /notifications
PATCH  /notifications
POST   /integrations/google
DELETE /integrations/google
```

### Artifact và transcript

```text
POST /meetings/:meetingId/google-artifacts/sync
POST /meetings/:meetingId/attachments
GET  /meetings/:meetingId/attachments
POST /meetings/:meetingId/recordings
GET  /meetings/:meetingId/recordings
POST /meetings/:meetingId/transcripts
GET  /meetings/:meetingId/transcripts
PATCH /meetings/:meetingId/transcripts
```

### Live transcription

```text
POST /meetings/:meetingId/live-transcription
GET  /meetings/:meetingId/live-transcription/:sessionId
POST /meetings/:meetingId/live-transcription/:sessionId/segments
POST /meetings/:meetingId/live-transcription/:sessionId/stop
```

### AI

```text
POST /meetings/:meetingId/ai/chat
POST /groups/:groupId/ai/search
POST /meetings/:meetingId/ai/minutes-draft
POST /meetings/:meetingId/ai/task-proposals
POST /ai/task-proposals/:id/confirm
POST /groups/:groupId/ai/progress-analysis
GET  /ai/jobs/:aiJobId
```

Meet Add-on dùng lại các endpoint này và không có API riêng.

## 7.4. Phản hồi API

Thành công:

```json
{
  "success": true,
  "data": {},
  "requestId": "request-id"
}
```

Thất bại:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Thông báo an toàn cho người dùng",
    "details": {}
  },
  "requestId": "request-id"
}
```

Không trả về frontend:

- Stack trace.
- Access token.
- Refresh token.
- Credential.
- Raw S3 key nhạy cảm.
- Nội dung secret.
- Chi tiết lỗi provider không được làm sạch.

## 7.5. Luồng upload và AIJob

```text
Browser
→ Yêu cầu presigned URL
→ API kiểm tra membership và policy
→ API tạo Attachment PENDING_UPLOAD
→ Trả presigned URL
→ Browser upload trực tiếp S3
→ Browser gọi complete
→ Backend HeadObject/checksum/MIME/size
→ Attachment READY hoặc REJECTED
→ Tạo đúng một AIJob
→ Step Functions xử lý
→ Client theo dõi trạng thái
```

## 7.6. Luồng live transcription

```text
Người dùng consent
→ Chọn nguồn capture
→ Tạo LiveTranscriptionSession
→ Streaming STT
→ Partial segment hiển thị tạm
→ Final segment gửi API
→ Kiểm tra sessionId + sequence
→ Lưu meeting-data
→ Transcript editor
→ Duyệt
→ Ingestion
```

Live transcript là nguồn chuẩn duy nhất để xác định những gì đã được phát biểu. Agenda, danh sách người tham dự hoặc tài liệu đính kèm chỉ là nguồn ngữ cảnh, không được sử dụng để suy đoán ai đã nói gì.

## 7.7. Luồng RAG

```text
Câu hỏi
→ Kiểm tra JWT
→ Kiểm tra membership
→ Xác định group được phép
→ Xác minh meeting set
→ Filter groupId + approved + meetingId
→ Retrieve source
→ Bedrock generation
→ Chuẩn hóa citation
→ GroundedAnswer
```

Nguồn có thể gồm:

- Tài liệu `READY`.
- Transcript đã duyệt.
- Biên bản đã duyệt.
- Final live segment được phép cho current-meeting summary.

## 7.8. Luồng Google

```text
Quản trị viên kết nối Google
→ OAuth callback phía backend
→ Lưu token reference an toàn
→ Tạo meeting nội bộ
→ Gọi Google Calendar API
→ Yêu cầu conference data
→ Lưu googleEventId và trạng thái
→ Poll/retry có giới hạn
→ READY: hiển thị link
→ Không có artifact: fallback upload/capture
```

Không tạo Google event mới chỉ vì request cũ đang `PENDING`.

---

# 8. BẢO MẬT VÀ QUYỀN RIÊNG TƯ

## 8.1. Xác thực

- Amazon Cognito User Pool.
- JWT được kiểm tra tại API Gateway.
- Backend lấy identity từ token.
- Protected route frontend chỉ hỗ trợ UX, không thay thế backend authorization.

## 8.2. Phân quyền

- Kiểm tra membership cho mọi tài nguyên group-scoped.
- Kiểm tra role tại thời điểm mutation.
- Không tin role do client gửi.
- Kiểm tra meeting thuộc group.
- Kiểm tra attendee và assignee là active member.
- Không cho RAG truy xuất chéo group.
- Confirm proposal phải kiểm tra lại toàn bộ quyền.

## 8.3. IAM

- Nguyên tắc least privilege.
- Mỗi Lambda chỉ được truy cập bảng, bucket hoặc secret cần thiết.
- Không lưu AWS access key trong source code.
- Developer dùng profile riêng.
- Lambda sử dụng execution role.
- Không sử dụng root account cho triển khai hằng ngày.

## 8.4. S3

- Block Public Access.
- Mã hóa dữ liệu lưu trữ.
- Object key theo environment, group và meeting.
- Presigned URL có thời gian sống ngắn.
- MIME allowlist.
- Giới hạn kích thước.
- Checksum.
- Lifecycle và retention.
- Không public recording hoặc transcript.

## 8.5. Google OAuth

- Authorization Code Flow.
- Scope tối thiểu.
- Token lưu phía máy chủ.
- Browser chỉ nhận trạng thái kết nối.
- Token không được ghi log.
- Có chức năng ngắt kết nối.
- Hiển thị rõ dữ liệu mà CampusMeet có thể đọc.

## 8.6. Consent và media

Trước khi capture:

- Hiển thị mục đích.
- Hiển thị nguồn capture.
- Hiển thị loại dữ liệu được lưu.
- Hiển thị retention.
- Yêu cầu thao tác đồng ý.
- Có chỉ báo session đang hoạt động.
- Có điều khiển dừng.

CampusMeet không cam kết microphone có thể thu toàn bộ âm thanh Google Meet.

## 8.7. AI safety

- Xem nội dung tài liệu và transcript là dữ liệu không tin cậy.
- Không biến nội dung nguồn thành tool instruction.
- Tool name nằm trong server-side allowlist.
- Validate schema ở backend.
- Citation bắt buộc.
- Human-in-the-loop.
- Không mutation tự động.
- Idempotency.
- Audit.
- Rate limit và quota.
- Trả không đủ căn cứ khi retrieval không đủ.

## 8.8. Logging

Log không chứa:

- Audio.
- Transcript đầy đủ.
- Prompt đầy đủ có dữ liệu nhạy cảm.
- Google token.
- JWT.
- Presigned URL còn hiệu lực.
- Secret.
- Thông tin cá nhân không cần thiết.

---

# 9. ĐỘ TIN CẬY, GIÁM SÁT VÀ KIỂM SOÁT CHI PHÍ

## 9.1. Độ tin cậy

Các thao tác quan trọng cần:

- Idempotency key.
- Conditional write.
- Optimistic version.
- Retry có giới hạn.
- Exponential backoff khi phù hợp.
- Timeout.
- Trạng thái lỗi rõ ràng.
- Không thất bại im lặng.
- Dead-letter hoặc failure record cho tác vụ nền.
- Smoke test sau deployment.

Ví dụ:

- Retry tạo group không tạo nhóm trùng.
- Retry tạo Google event không tạo event trùng.
- Retry reminder không gửi trùng.
- Retry upload complete không tạo nhiều AIJob.
- Retry final segment không tạo segment trùng.
- Retry confirm proposal không tạo nhiều task.

## 9.2. CloudWatch

CloudWatch thu thập:

- API Gateway access log.
- Lambda invocation, error, duration và throttle.
- Request latency.
- Tỷ lệ 4xx và 5xx.
- Authorization failure.
- Google sync success, pending và failure.
- Reminder sent, skipped và email failure.
- AIJob queued, completed và failed.
- STT duration và failure.
- Token usage.
- Retrieval empty.
- Citation missing.
- Step Functions failure.
- SQS/DLQ message count nếu được sử dụng.
- Chi phí và quota AI theo môi trường.

## 9.3. Cảnh báo

Ít nhất các cảnh báo sau cần được xem xét:

- API 5xx vượt ngưỡng.
- Lambda error tăng.
- Reminder failure.
- Google sync failure.
- AIJob failure hoặc timeout.
- Transcription failure.
- Citation missing.
- Retrieval empty bất thường.
- Chi phí hoặc dự báo chi phí vượt ngưỡng.
- Có message trong dead-letter queue.

CloudWatch Alarm gửi sự kiện đến Amazon SNS để thông báo cho người phụ trách.

## 9.4. Mô hình chi phí

CampusMeet sử dụng các dịch vụ tính phí theo mức sử dụng và tránh tài nguyên chạy liên tục.

| Dịch vụ | Yếu tố chi phí | Biện pháp kiểm soát |
|---|---|---|
| CloudFront | Request và data transfer | Cache asset, nén và giới hạn dữ liệu demo |
| S3 | Dung lượng, request và transfer | Lifecycle, retention và xóa file demo |
| Cognito | Monthly active users | Dữ liệu người dùng demo nhỏ |
| API Gateway | Số request | Giảm polling không cần thiết |
| Lambda | Invocation, thời gian và bộ nhớ | Timeout ngắn, package nhỏ, xử lý bất đồng bộ |
| DynamoDB | Read/write request và storage | On-demand, Query/GSI, không Scan |
| EventBridge Scheduler | Số schedule và invocation | Xóa schedule cũ hoặc bị hủy |
| SES | Số email | Email chỉ là kênh bổ sung |
| CloudWatch | Log ingestion và retention | Đặt log retention, không log payload lớn |
| Step Functions | State transition | Workflow ngắn, tránh polling loop dài |
| Transcribe | Số phút audio | Quota phiên, giới hạn thời lượng demo |
| Bedrock | Input/output token | Giới hạn token, số query và model config |
| Knowledge Base/S3 Vectors | Storage, ingestion và query | Chỉ ingest nguồn đã duyệt; xóa source stale |
| Secrets Manager | Số secret và API call | Sử dụng SSM SecureString khi phù hợp |
| SNS | Số notification | Chỉ cảnh báo cần thiết |

Chi phí cụ thể chưa được cố định trong đặc tả vì phụ thuộc:

- Region.
- Model Bedrock được chọn.
- Embedding model.
- Số phút transcription.
- Số token.
- Số tài liệu.
- Dung lượng audio.
- Tần suất ingestion.
- Thời gian retention.

Trước deployment demo, nhóm phải:

1. Chốt giả định usage.
2. Chạy AWS Pricing Calculator.
3. Thiết lập AWS Budgets.
4. Đặt quota cho STT và Bedrock.
5. Ghi token/phút/chi phí ước tính theo AIJob.
6. Rà soát Cost Explorer sau demo.
7. Cleanup tài nguyên không còn sử dụng.

Không đưa ra một con số chi phí tháng cố định nếu chưa kèm giả định và bảng tính.

---

# 10. KẾ HOẠCH THỰC HIỆN

## 10.1. Phương pháp

Dự án được triển khai theo vertical slice thay vì xây toàn bộ frontend rồi mới làm backend.

Mỗi vertical slice gồm:

```text
Shared contract
→ Domain rule
→ Application service
→ Repository/integration
→ API
→ Frontend
→ Test
→ IaC khi cần
→ Smoke test
→ Evidence
```

Các dependency chưa hoàn thành có thể dùng fake adapter trong test/local. Fake không được hard-code vào production handler.

## 10.2. Lộ trình tám tuần

| Tuần | Trọng tâm | Sản phẩm bàn giao | Tiêu chí kết thúc |
|---|---|---|---|
| 1 | Phân tích và thống nhất | SRS, wireframe, kiến trúc, data model, kế hoạch IAM và chi phí | Nhóm thống nhất baseline |
| 2 | Nền tảng | Monorepo, CI, IaC skeleton, Cognito, CloudFront/S3, API skeleton, UI shell | Đăng nhập và health check có bằng chứng |
| 3 | Nhóm và thành viên | Group, membership, invitation và authorization boundary | Test truy cập chéo nhóm trả 403 |
| 4 | Cuộc họp cốt lõi | Meeting, agenda, attendee, organizer, lifecycle và timeline | Luồng meeting nội bộ đầu-cuối |
| 5 | Google integration | OAuth, Calendar event, conference request, sync status, retry và artifact spike | Có integration thật hoặc fallback rõ |
| 6 | Sau họp và nguồn AI | Reminder, notification, minutes, task, dashboard, presigned upload, consent và recording metadata | Minutes → Task → Dashboard; upload không qua API payload |
| 7 | AI vertical slice | Live STT, transcript editor, chatbot, draft minutes/task, Knowledge Source và citation | Một meeting chạy đầu-cuối với citation |
| 8 | Hoàn thiện | Multi-meeting RAG, ACL test, progress analysis, prompt-injection test, alarm, cost và cleanup | Không rò dữ liệu chéo nhóm; demo và evidence hoàn chỉnh |

## 10.3. Thứ tự merge đề xuất

1. Data model và IaC v2.
2. Shared error, pagination và idempotency.
3. Group/membership authorization boundary.
4. Meeting boundary.
5. Minutes, task và dashboard.
6. Google integration.
7. Upload, live transcript và AIJob.
8. Knowledge Source, RAG và citation.
9. Reminder và notification integration.
10. Security, observability, cost và cleanup rehearsal.

---

# 11. HOẠT ĐỘNG VÀ SẢN PHẨM BÀN GIAO

| Giai đoạn | Hoạt động | Sản phẩm bàn giao |
|---|---|---|
| Phân tích | Xác định vấn đề, người dùng, use case, phạm vi và ràng buộc | SRS và proposal |
| Thiết kế | Thiết kế AWS architecture, API và data access pattern | Draw.io, API contract, data model |
| Nền tảng | Thiết lập monorepo, CI, Cognito và IaC | Repository chạy được và quality gates |
| Core M1 | Group, invitation, membership và authorization | Vertical slice nhóm |
| Core M2 | Meeting, agenda, attendee và lifecycle | Vertical slice cuộc họp |
| Core M3 | Minutes, tasks, dashboard và notification | Luồng sau họp |
| Integration M4 | Google OAuth, Calendar, Meet artifact và reminder | Integration adapter và fallback |
| AI M5 | Upload, consent, live transcript, AIJob, RAG và proposal | AI vertical slice có citation |
| Kiểm thử | Unit, integration, security, end-to-end và operational test | Test report và evidence |
| Triển khai | SAM/CloudFormation change set, deploy và smoke test | AWS environment |
| Vận hành | Logs, metrics, alarm, cost và cleanup | Dashboard, alarm và runbook |
| Báo cáo | Worklog, workshop và trình bày | Website báo cáo song ngữ |

---

# 12. PHÂN CÔNG NHÓM

| Thành viên | Outcome chính | Phạm vi |
|---|---|---|
| M1 | Group và authorization | Group, membership, invitation, role và authorization helper |
| M2 | Meeting | Meeting, agenda, attendee, organizer, lifecycle và timeline |
| M3 | Sau họp | Minutes, action item, task, dashboard và progress snapshot |
| M4 | Google và reminder | OAuth, Calendar/Meet adapter, artifact sync, Scheduler, notification và SES |
| M5 | Upload, transcript và AI | Attachment, recording consent, live STT, AIJob, Knowledge Source, RAG, citation và proposal |

Các phần shared contract, router, IAM và IaC cần được review chéo. Ownership xác định người chịu trách nhiệm outcome, không phải quyền sở hữu độc quyền file.

Mỗi thành viên cần có:

- Pull request hoặc commit rõ ràng.
- Unit hoặc integration test.
- Một trường hợp lỗi hoặc biên.
- Bằng chứng CloudWatch.
- Worklog mô tả phần việc.
- Xác nhận cleanup tài nguyên liên quan.

---

# 13. RỦI RO VÀ PHƯƠNG ÁN XỬ LÝ

| Rủi ro | Ảnh hưởng | Phương án |
|---|---|---|
| OAuth hoặc redirect URI cấu hình sai | Cao | Kiểm thử sớm; giữ core CRUD độc lập |
| Link Meet còn PENDING | Trung bình | Hiển thị trạng thái; retry giới hạn; không tạo link giả |
| Google artifact không tồn tại | Cao | Upload hoặc capture fallback |
| SES sandbox hạn chế email | Trung bình | In-app notification là bắt buộc |
| Phạm vi bị mở rộng | Cao | Khóa MoSCoW và gate từng tuần |
| Rò dữ liệu chéo nhóm | Cao | Authorization tập trung và test 403 |
| Tạo event/reminder trùng | Cao | Idempotency và conditional write |
| Không capture đủ âm thanh | Cao | Nêu rõ nguồn capture và không cam kết ngầm |
| STT tiếng Việt không chính xác | Cao | Benchmark, confidence, editor và human review |
| Sai speaker label | Trung bình | Chỉ dùng `Speaker N`, không đoán danh tính |
| Prompt injection | Cao | Untrusted source, allowlist, schema, confirm và audit |
| RAG retrieve nhầm nhóm | Cao | Filter `groupId` trước retrieval và test chéo nhóm |
| AI hallucination | Cao | Citation, draft status và insufficient context |
| Tạo task sai | Cao | Missing fields, preview và xác nhận |
| Chi phí AI tăng | Trung bình | Quota phút/token, Budgets và cost metadata |
| Xóa nhầm dữ liệu legacy | Cao | Audit, backup và review trước cleanup |
| Template tồn tại nhưng app chưa hoạt động | Cao | Chỉ xác nhận bằng integration/smoke test và logs |
| Nhóm làm tài liệu muộn | Trung bình | Thu thập evidence sau mỗi sprint |

---

# 14. LỘ TRÌNH TỪ MVP ĐẾN MÔI TRƯỜNG HOÀN CHỈNH

## 14.1. Trạng thái hiện tại

Tại thời điểm lập đề xuất:

| Hạng mục | Trạng thái |
|---|---|
| Cognito authentication | Đã có nền tảng và từng được kiểm thử integration; stack thử trước đó đã cleanup |
| React frontend | Đã có cấu trúc nhưng nhiều màn hình nghiệp vụ còn mock |
| API `/health` | Đã có handler trả `200` |
| API nghiệp vụ | Phần lớn còn skeleton và trả `501 Not Implemented` |
| Authorization theo nhóm | Chưa hoàn thiện |
| DynamoDB repository | Chưa hoàn thiện ở các vertical slice |
| DynamoDB v2 | Đã chốt thiết kế năm bảng |
| 17 bảng legacy | Cần audit/backup trước cleanup |
| Upload và live transcript | Contract và kiến trúc đã chốt, chưa triển khai hoàn chỉnh |
| Bedrock RAG | Kiến trúc mục tiêu, chưa được xem là vận hành |
| Full application stack | Chưa production-ready |
| CI | Có quality gates cơ bản |
| CD | Cần hoàn thiện deployment, smoke test và rollback |

## 14.2. Các giai đoạn triển khai

### Giai đoạn 1 – Data foundation

- Audit 17 bảng legacy.
- Backup/export khi có dữ liệu.
- Preview CloudFormation change set.
- Deploy năm bảng v2.
- Verify schema, GSI, TTL và tag.
- Không xóa legacy.

### Giai đoạn 2 – Core application

- Group và membership.
- Meeting.
- Minutes.
- Tasks.
- Dashboard.
- Notification.
- Repository thật.
- Cross-group test.

### Giai đoạn 3 – External integration

- Google OAuth.
- Calendar event.
- Conference request.
- Sync lifecycle.
- Reminder.
- SES fallback.
- Google artifact sync.

### Giai đoạn 4 – AI source

- S3 user-content.
- Presigned upload.
- Consent.
- Recording metadata.
- Live session.
- Transcript segment.
- Transcript editor.
- AIJob.

### Giai đoạn 5 – Grounded AI

- Normalized Knowledge Source.
- Bedrock Knowledge Base.
- S3 Vectors.
- Current-meeting chat.
- Selected-meeting RAG.
- Whole-group RAG.
- Citation.
- Minutes draft.
- Task proposal.
- Progress analysis.

### Giai đoạn 6 – Release candidate

- Security tests.
- Cross-group RAG tests.
- Idempotency tests.
- Prompt-injection tests.
- CloudWatch dashboard.
- Alarm/SNS.
- Cost review.
- Retention.
- Cleanup rehearsal.
- Demo evidence.

---

# 15. ĐIỀU KIỆN NGHIỆM THU

Dự án chỉ được nghiệm thu khi:

1. Lint, typecheck, test và build thành công.
2. Hạ tầng có thể tái lập bằng AWS SAM/CloudFormation.
3. Không có secret hoặc credential trong repository.
4. Hoàn thành luồng:

```text
Đăng nhập
→ Tạo nhóm
→ Mời thành viên
→ Tạo cuộc họp
→ Đồng bộ Google
→ Nhắc lịch
→ Biên bản
→ Task
→ Dashboard
```

5. Hoàn thành AI vertical slice:

```text
Upload/consent
→ Live transcription
→ Transcript
→ Chat/tóm tắt
→ Minutes draft
→ Task proposal
→ Citation
→ Confirm
→ Task chính thức
```

6. RAG hỗ trợ current, selected và whole-group trong cùng một nhóm.
7. Test chứng minh group A không đọc được nguồn group B.
8. AI không nhận diện hoặc đánh giá cá nhân.
9. AI không tự mutation dữ liệu.
10. Email lỗi không làm mất in-app notification.
11. Có ít nhất một cảnh báo CloudWatch/SNS được kiểm thử.
12. Log không chứa token, secret hoặc nội dung nhạy cảm.
13. Có số liệu transcription, token hoặc cost metadata cho demo.
14. Có checklist retention và cleanup.
15. Có bằng chứng từ từng thành viên.
16. Tài liệu và sơ đồ phản ánh đúng trạng thái Implemented, Incomplete và Proposed.

---

# 16. KẾT QUẢ MONG ĐỢI

Sau khi hoàn thành MVP, CampusMeet cung cấp một quy trình thống nhất cho cuộc họp nhóm:

- Thành viên biết cuộc họp nào sắp diễn ra.
- Người tổ chức giảm thao tác tạo lịch và liên kết.
- Thành viên được nhắc đúng thời điểm.
- Nội dung cuộc họp được lưu dưới dạng transcript có thể chỉnh sửa.
- Người vào trễ có thể nắm phần nội dung đã diễn ra.
- Biên bản và quyết định có bằng chứng nguồn.
- Action item được chuyển thành task qua bước xác nhận.
- Quản trị viên theo dõi tiến độ của nhóm.
- Người dùng hỏi đáp trên nhiều cuộc họp mà không rò dữ liệu chéo nhóm.
- Hệ thống có log, metric, alarm, cost control và cleanup.
- Toàn bộ hạ tầng có thể được triển khai lại bằng mã nguồn.

CampusMeet chứng minh việc kết hợp các dịch vụ AWS serverless, tích hợp Google và AI có kiểm soát để giải quyết một bài toán thực tế. Giá trị cốt lõi của hệ thống không nằm ở việc thay thế Google Meet, mà ở việc biến mỗi cuộc họp thành một quy trình có kế hoạch, bằng chứng, trách nhiệm và khả năng theo dõi rõ ràng.