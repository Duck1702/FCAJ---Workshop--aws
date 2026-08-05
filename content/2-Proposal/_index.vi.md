---
title: "Bản đề xuất"
date: 2026-08-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# CAMPUSMEET — HỆ THỐNG QUẢN LÝ CUỘC HỌP THÔNG MINH TRÊN AWS

{{% notice info %}}
Tài liệu phân biệt **kiến trúc mục tiêu** theo SRS với **trạng thái hiện tại**: M1 và lõi M2 đã có code/test; M3 còn skeleton; M4 adapter thật chưa hoàn chỉnh; M5 có một phần API/AI Worker/IaC/test nhưng upload/live transcript và full cloud pipeline chưa hoàn tất E2E.
{{% /notice %}}

## 2.1 Tổng quan đề tài

CampusMeet phục vụ nhóm học tập, đồ án và dự án nhỏ cần quản lý thống nhất trước, trong và sau cuộc họp. Người dùng mục tiêu là thành viên, Group Admin, organizer và attendee. Giá trị chính là gom group, agenda, lịch, biên bản, nhiệm vụ, transcript và tri thức có citation vào một authorization boundary. Kiến trúc serverless được chọn để tránh máy chủ chạy liên tục, co giãn theo yêu cầu, dùng dịch vụ managed và triển khai lặp lại bằng IaC. CampusMeet không phải nền tảng gọi video; cuộc họp online diễn ra trên Google Meet.

## 2.2 Vấn đề cần giải quyết

- Lịch, agenda, organizer và attendee phân tán; khó xác định thông tin chính thức.
- Biên bản làm thủ công; quyết định và nhiệm vụ sau họp dễ bị bỏ sót.
- Transcript dài, khó tổng hợp và khó tìm quyết định qua nhiều cuộc họp.
- AI không citation dễ tạo kết luận không truy vết được.
- Nhắc lịch và Google Calendar/Meet chưa đồng nhất với bản ghi nội bộ.
- `groupId` do client gửi có thể gây truy cập chéo nhóm nếu backend không resolve resource và kiểm tra active membership.

## 2.3 Giải pháp đề xuất

| Module | Giải pháp |
|---|---|
| M1 | Cognito JWT; Identity, Group, Membership, Invitation; trusted identity và group authorization |
| M2 | Meeting CRUD/lifecycle, agenda, attendee, organizer, timeline, optimistic version và idempotent cancel |
| M3 | Minutes, quyết định/action item, Tasks và Dashboard |
| M4 | Google OAuth, Calendar event, Meet conference/link, sync status, Scheduler reminder và notification |
| M5 | Presigned upload, transcript, AIJob, AI Worker, Bedrock RAG nhiều meeting và citation |

CloudWatch cung cấp log/metric/alarm; IAM least privilege và AWS Budgets hỗ trợ vận hành/kiểm soát chi phí.

## 2.4 Mục tiêu

- **Tổng quát:** xây dựng MVP quản lý vòng đời họp trên AWS, không thay thế Google Meet.
- **Chức năng:** hoàn tất use case M1–M5 theo SRS; mọi mutation AI phải có preview/xác nhận.
- **Kỹ thuật:** quality gates `lint/typecheck/test/build/infra:validate` đạt; IaC có thể validate/build; async job có trạng thái/retry/idempotency.
- **Bảo mật:** mọi API group-scoped xác minh JWT, trusted identity, active membership, role và resource; negative cross-group test phải đạt.
- **Học tập:** chứng minh hiểu serverless, DynamoDB access pattern, observability, cost/cleanup và tích hợp external system bằng báo cáo, test và workshop.

Không đặt SLA hay ngưỡng latency khi repository chưa có bằng chứng đo.

## 2.5 Phạm vi

**Trong phạm vi:** M1–M5 theo SRS; React/Vite, S3/CloudFront, Cognito, HTTP API/Lambda, năm bảng DynamoDB, S3 user-content, Scheduler, SES, Step Functions/Transcribe/Bedrock/Knowledge Bases/S3 Vectors theo target/IaC; Google Meet là external platform; MVP serverless một Region.

**Ngoài phạm vi:** tự xây video conference, multi-region active-active, billing thương mại, AI tự phê duyệt, VPC/NAT/EC2/ECS/EKS/RDS và chức năng ngoài SRS.

## 2.6 Người dùng và use case

- Người dùng đã xác thực: profile, group/invitation, notification.
- Thành viên nhóm/attendee: xem meeting được phép, timeline, source và task được giao.
- Group Admin: quản lý thành viên, tạo/sửa/hủy meeting, Minutes/Task theo quyền.
- Organizer: trách nhiệm theo meeting, không phải global role; liên kết Google theo thiết kế.
- M1 tạo group/mời/accept/decline; M2 quản lý meeting; M3 minutes/tasks/dashboard; M4 sync/reminder; M5 upload/transcript/RAG/citation/draft.

## 2.7 Yêu cầu chức năng

| Module | Chức năng | Kết quả mong đợi |
|---|---|---|
| M1 | Xác thực, group, membership, invitation | Chỉ active member truy cập dữ liệu nhóm; Group Admin quản trị đúng quyền |
| M2 | CRUD/lifecycle, agenda, attendee, organizer, timeline | Meeting nhất quán, version conflict không ghi đè, cancel idempotent |
| M3 | Minutes, Tasks, Dashboard | Decision/action item truy vết; Task theo assignee/status; dashboard tổng hợp |
| M4 | OAuth, Calendar/Meet, reminder | Bản ghi nội bộ tồn tại khi sync lỗi; Meet URL chỉ ở `READY`; reminder không trùng |
| M5 | Upload, transcript, AIJob, RAG/citation | Binary đi thẳng S3; job async; source approved; câu trả lời có citation hoặc insufficient context |

## 2.8 Yêu cầu phi chức năng

Bảo mật và privacy theo group; availability/scaling dựa trên managed serverless nhưng không cam kết SLA; request path quan trọng dùng access pattern thay vì `Scan`; structured logs/request ID/metrics/alarm; module/repository/DTO tách để maintain; AI citation truy vết nguồn; quota, retention, Budgets và cleanup hỗ trợ cost awareness; dữ liệu, log và vector phải cách ly nhóm.

## 2.9 Kiến trúc đề xuất

![Kiến trúc AWS CampusMeet](/FCAJ---Workshop--aws/images/campusmeet/campusmeet-aws-architecture.png)

Browser nhận React build từ CloudFront với private S3 origin. Cognito phát JWT; API Gateway HTTP API chuyển request tới Lambda. Lambda resolve resource, authorize membership/role rồi truy cập DynamoDB. Upload dùng presigned URL tới S3 user-content; transcript/final source tạo AIJob, orchestration/AI Worker xử lý và ingest approved source vào Bedrock Knowledge Bases/S3 Vectors. RAG filter `groupId`/selected meetings trước generation và trả citation. Google OAuth/Calendar/Meet nằm ngoài AWS; Scheduler gọi Reminder Lambda và SES gửi email bổ sung. CloudWatch/SNS quan sát lỗi; Budgets cảnh báo chi phí.

## 2.10 Lựa chọn dịch vụ AWS

| Dịch vụ | Vai trò | Lý do |
|---|---|---|
| S3 + CloudFront | Static frontend/private origin; user content | Durable object storage và edge delivery |
| Cognito | User pool/JWT | Managed identity |
| API Gateway + Lambda | HTTP API/business logic/worker/reminder | Serverless, event-driven |
| DynamoDB | Năm bảng nghiệp vụ | Access-pattern driven, on-demand scaling |
| EventBridge Scheduler | One-time reminder | Managed scheduling |
| SES | Email bổ sung | Tách delivery khỏi in-app notification |
| Step Functions | Orchestration AI mục tiêu | Theo dõi bước/retry cho job dài |
| Transcribe | Speech-to-text mục tiêu | Managed transcription |
| Bedrock + Knowledge Bases + S3 Vectors | Generation, grounded retrieval/vector store | RAG có metadata/citation |
| CloudWatch + SNS | Logs, metrics, alarms | Observability/notification |
| IAM, SAM/CloudFormation, Budgets | Quyền, IaC, cost control | Least privilege, repeatability, cảnh báo |

## 2.11 Mô hình dữ liệu

Năm bảng vật lý là `identity`, `collaboration`, `meeting-data`, `task-data`, `ai-work`; đây là data model v2, không phải 17 bảng legacy. Mỗi module sở hữu entity/access pattern; composite key và sparse GSI phục vụ query. Backend luôn resolve `meetingId → trusted groupId` trước authorization. Không dùng `Scan` trong request path quan trọng; binary ở S3 và vector ở S3 Vectors/Knowledge Bases.

## 2.12 Bảo mật

Cognito JWT chỉ xác thực; backend vẫn lấy identity từ trusted claims, xác minh active membership/role và cách ly cross-group. IAM áp dụng least privilege. Frontend bucket và user-content private; CloudFront dùng origin access. Google token/secret lưu qua cơ chế tham chiếu secret, không trong Git/log/frontend env. Dữ liệu được mã hóa bằng khả năng managed service; log không chứa credential, JWT, transcript đầy đủ hoặc presigned URL. Retrieval chỉ dùng approved source, filter trước model và citation chỉ dẫn tới URI nội bộ được authorize.

## 2.13 Kế hoạch triển khai

| Giai đoạn | Nội dung | Kết quả đầu ra | Trạng thái hiện tại |
|---|---|---|---|
| M1 | Identity/group/membership/invitation/auth | UI, handler/repository/test, auth stack | Hoàn thành lõi; stack auth đã từng deploy/test |
| M2 | Meeting CRUD/lifecycle/timeline | Code/test/frontend flow | Hoàn thành lõi code; app stack chưa deploy E2E |
| M3 | Minutes/tasks/dashboard | Post-meeting workflow | Một phần; handler còn `501` |
| M4 | Google/reminder | OAuth/Calendar/Meet/Scheduler | Kế hoạch/architecture; adapter thật chưa hoàn chỉnh |
| M5 | Upload/transcript/AI/RAG | AIJob/worker/KB/citation | Một phần; có code/IaC/test, thiếu upload/live/E2E |
| Release | Smoke/security/operations/cleanup | Evidence và workshop | Đang triển khai |

## 2.14 Chi phí

Chi phí tăng theo request/compute Lambda, DynamoDB read/write/storage, S3 object/transfer, CloudFront transfer, log retention, lịch, email, phút Transcribe, Bedrock token/ingestion và vector storage. Pay-per-use/Free Tier không đồng nghĩa bằng 0. Thiết lập AWS Budgets, quota, retention và xóa resource workshop. Mọi con số chỉ là ví dụ sau khi kiểm tra AWS Pricing Calculator và Region tại thời điểm deploy.

## 2.15 Rủi ro và giảm thiểu

| Rủi ro | Giảm thiểu |
|---|---|
| Chi phí AI/transcription/log | Quota, Budget, retention, optional lab, cleanup |
| Quota/Region/model access | Preflight availability và fallback test/mock |
| Google OAuth/scopes | Redirect URI chính xác, least privilege, secret store, revoke token |
| Transcript nhạy cảm | Consent, private S3, encryption, retention, redact log |
| Cross-group access | Trusted group resolution, active membership, negative tests |
| Retry/idempotency/eventual consistency | Idempotency key, conditional/version write, observable job states |
| Frontend bundle size | Theo dõi Vite warning, code splitting khi có bằng chứng cần thiết |
| Service chưa hoàn thiện | Gắn status partial/planned; demo bằng test, không tuyên bố E2E |

## 2.16 Kết quả đầu ra

Source code CampusMeet; SRS; API contract; data model; architecture diagram; SAM/CloudFormation IaC; web application; test suite; AWS deployment guide; workshop guide; và báo cáo đánh giá nêu rõ bằng chứng, giới hạn implementation, bảo mật, chi phí và cleanup.