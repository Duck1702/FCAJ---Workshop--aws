---
title: "Bản đề xuất"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## 1. Tên dự án

CampusMeet — Nền tảng quản lý cuộc họp và cộng tác có AI trên AWS

## 2. Tổng quan dự án

CampusMeet là ứng dụng web cho nhóm sinh viên quản lý thành viên, cuộc họp, biên bản, task, nhắc lịch và tri thức cuộc họp. Live transcription có sự đồng ý và artifact được phép hỗ trợ hỏi đáp có citation, nháp biên bản và TaskProposal. AI không tự ý thay đổi dữ liệu chính thức.

Repository nguồn có frontend React/Vite, cấu trúc Lambda API TypeScript, shared contract, test, AWS SAM template, data foundation năm bảng DynamoDB, API contract, SRS và sơ đồ AWS. Đề xuất này phát triển baseline đó thành kế hoạch AWS theo giai đoạn; không xem thành phần mục tiêu là đã vận hành.

## 3. Bài toán cần giải quyết

Thông tin họp nằm rải rác trong chat, lịch, tài liệu và ghi chú. Quyết định dễ thất lạc, người vào trễ thiếu ngữ cảnh, reminder không nhất quán và tìm kiếm qua nhiều meeting chậm. AI không an toàn nếu thiếu citation, truy xuất chéo nhóm, suy đoán speaker hoặc tạo task chưa được duyệt.

## 4. Mục tiêu và người dùng

CampusMeet phục vụ nhóm đồ án, câu lạc bộ, Group Admin, Meeting Organizer, thành viên, Minute Taker và người vận hành. Mục tiêu là hoàn thiện cộng tác có xác thực; transcription có consent; segment Speaker N hiệu chỉnh được; câu trả lời, biên bản và task proposal có citation; retrieval cô lập theo nhóm; tích hợp Google Calendar gọn; và kiến trúc serverless ít vận hành, có monitoring và kiểm soát chi phí.

## 5. Phạm vi chức năng

Phạm vi gồm xác thực; group, invitation và role; vòng đời meeting, agenda, attendee và trạng thái Google sync; reminder/notification; minutes, task, dashboard; upload an toàn; live transcription; duyệt transcript; AIJob bất đồng bộ; grounded chat; minutes draft; TaskProposal có citation; và phân tích tiến độ nhóm. Mutation chính thức phải qua authorization, preview, confirm và idempotency.

### 5.1. MVP và tương lai

MVP gồm nghiệp vụ cộng tác cốt lõi, reminder một lần, email tùy chọn, upload, AIJob, transcription chạy nền sau consent ưu tiên vi-VN, duyệt transcript, RAG có citation theo meeting hiện tại/các meeting chọn/toàn nhóm, draft minutes, task proposal có confirm, monitoring, cảnh báo chi phí và cleanup.

Tương lai gồm meeting định kỳ, Slack/Discord, Google Meet artifact nâng cao, reranking và Meet side panel. MVP loại trừ video-call riêng, suy đoán danh tính speaker, đánh giá cá nhân và AI tự động mutation.

## 6. Trạng thái hiện tại

| Trạng thái | Bằng chứng trong repository |
| --- | --- |
| **Implemented** | App shell/route React-Vite; UI Cognito và auth test; shared contract; API utility; /health và /me dựa trên JWT; CI/test; SAM target template; năm bảng DynamoDB mã hóa, on-demand có GSI, TTL và PITR tùy chọn. |
| **Incomplete** | Trang chức năng chủ yếu dùng mock; group/meeting repository ném NotImplementedError; đa số endpoint là skeleton; authorization chưa xong; SAM còn TODO về CORS, IAM, domain, alarm và deployment role. |
| **Proposed** | Deploy AWS, user-content S3, upload validation, Transcribe, AIJob worker, transcript, Bedrock RAG/citation, Calendar, Scheduler/SES, SQS DLQ, Parameter Store, dashboard và budget alarm. |

Implemented nghĩa là có trong source/IaC, không đồng nghĩa đã deploy vào tài khoản AWS.

## 7. Kiến trúc triển khai AWS mục tiêu

![Kiến trúc AWS mục tiêu của CampusMeet](/FCAJ---Workshop--aws/images/2-Proposal/campusmeet-aws-architecture-Target%20MVP%20Architecture.drawio.png)

Amazon CloudFront phân phối React build từ private Amazon S3 frontend bucket qua Origin Access Control. Amazon Cognito cấp JWT để Amazon API Gateway HTTP API xác minh trước khi gọi AWS Lambda least privilege. Amazon DynamoDB on-demand lưu record nghiệp vụ; private S3 user-content bucket riêng lưu upload/artifact. Amazon Transcribe tạo live segment; Amazon Bedrock hỗ trợ grounded generation và RAG nhiều meeting.

Amazon EventBridge Scheduler gọi reminder, Amazon SES gửi email tùy chọn tới người nhận bên ngoài, Amazon SQS DLQ cô lập lỗi bất đồng bộ. AWS Systems Manager Parameter Store giữ cấu hình/secure reference. Amazon CloudWatch, Amazon SNS và AWS Budgets cung cấp quan sát/cảnh báo. GitHub Actions và AWS SAM hỗ trợ kiểm tra/phân phối. Google Calendar API là external system gọn. MVP không cần EC2, ECS/EKS, RDS, ALB, VPC hoặc NAT Gateway.

Luồng chính: CloudFront tải web; Cognito xác thực; HTTP API/Lambda kiểm tra identity, group role và record scope; Lambda query DynamoDB hoặc cấp presigned S3 URL phạm vi hẹp; speech/AI dài hạn thành AIJob; Scheduler gọi Reminder Lambda; CloudWatch alarm gửi SNS.

## 8. Kiến trúc frontend

apps/web dùng React, Vite, TypeScript, public/protected route, app shell, feature service và API client. Static output được đưa vào private frontend bucket. Runtime config chỉ chứa API/Cognito public value, không chứa credential/secret. Màn hình hiện có bao phủ auth, dashboard, group, meeting, task, notification và setting nhưng cần thay mock bằng API có JWT cùng trạng thái loading, empty, unauthorized và retry.

## 9. Kiến trúc backend và API

services/api có handler, DTO, domain port, adapter, response thống nhất, request ID và structured log. API Gateway là public boundary; Lambda stateless; dữ liệu bền vững thuộc DynamoDB/S3. Contract bao phủ health/profile, group/membership, meeting, minutes, task, dashboard, notification, integration, upload, transcript, AI job và AI interaction. Endpoint theo nhóm lấy identity từ JWT, kiểm tra membership/meeting scope, dùng pagination opaque và idempotency.

## 10. Xác thực và phân quyền

Cognito quản lý sign-up, confirm, sign-in và JWT. HTTP API authorizer kiểm tra issuer, audience, expiry; backend tiếp tục phân quyền admin, organizer, member và minute taker. Không tin userId từ client. Meeting chéo nhóm bị chặn trước read/retrieval/mutation. Lambda role theo least privilege, log loại dữ liệu nhạy cảm, Google credential nằm phía server qua secure Parameter Store reference.

## 11. Kiến trúc DynamoDB

| Bảng | Trách nhiệm |
| --- | --- |
| Identity | Profile, invitation, identity và integration reference. |
| Collaboration | Group, membership và quan hệ cộng tác. |
| MeetingData | Meeting, attendee, reminder, minutes, artifact và transcript. |
| TaskData | Task, view theo assignee/status và dashboard access pattern. |
| AIWork | AIJob, upload/transcription, retrieval metadata, proposal và idempotency. |

Các bảng source-controlled dùng PK/SK, GSI, on-demand billing, encryption, TTL và PITR tùy chọn. Query dùng key/GSI, không Scan. AI record luôn mang groupId/meetingId. Conditional write ngăn trùng invitation, upload complete, final segment, reminder và task sau confirm.

## 12. Upload file an toàn

Client có quyền gửi upload intent cùng meeting, MIME, size và checksum. Lambda kiểm tra membership, policy, quota và server-owned key rồi cấp presigned URL ngắn hạn cho đúng object/header. Browser upload trực tiếp vào private S3. Completion xác minh metadata, size, checksum, ownership trước trạng thái READY; sau đó mới tạo AIJob idempotent. Bucket block public access, encryption, lifecycle; download cũng cần authorization và URL ngắn hạn.

## 13. Live transcription và lưu transcript

Sau consent/capture permission rõ ràng, CampusMeet khởi tạo phiên nền và hiển thị trạng thái/nguồn. Audio stream tới Amazon Transcribe theo ngôn ngữ chọn hoặc auto-identification được hỗ trợ; tiếng Việt là benchmark. Browser chỉ có microphone không bảo đảm thu đủ Google Meet audio.

Partial segment chỉ cập nhật UI. Final segment được lưu idempotent cùng meeting, timestamp, confidence, language, session và Speaker N ẩn danh. Người có quyền sửa/duyệt version; không suy luận người thật từ giọng nói. Artifact đã duyệt ở private S3; metadata/segment query được ở DynamoDB. Capture lỗi thì AI báo thiếu nguồn, không dựng phát biểu từ agenda/attendee.

## 14. Xử lý AIJob

Parse, hoàn tất transcription, indexing và generation tạo AIJob trạng thái QUEUED, PROCESSING, COMPLETED, FAILED hoặc CANCELLED. Worker claim có điều kiện, cập nhật progress, lưu lỗi an toàn và chỉ retry lỗi tạm thời. Lỗi lặp lại vào SQS DLQ để redrive có kiểm soát. Correlation ID nối API request, job, session và CloudWatch log.

## 15. Amazon Bedrock RAG nhiều meeting và citation

Chỉ tài liệu READY, live/approved transcript được phép và minutes đã duyệt là nguồn hợp lệ. Sau authorization, retrieval giới hạn ở CURRENT_MEETING, SELECTED_MEETINGS đã xác minh trong một nhóm hoặc WHOLE_GROUP. Bedrock chỉ nhận passage và source metadata, không truy cập table/bucket không giới hạn.

Mỗi câu trả lời cite meeting và section tài liệu hoặc Speaker N + timestamp; live text chưa duyệt được gắn nhãn. Khi nguồn thiếu/mâu thuẫn, AI nói rõ thay vì bịa. Minutes draft và TaskProposal giữ citation. Người có quyền bổ sung field bắt buộc, review/confirm trước khi Task API mutation idempotent.

## 16. Thông báo, reminder và Google Calendar

Lambda tạo/cập nhật EventBridge Scheduler schedule một lần. Khi chạy, Reminder Lambda kiểm tra lại meeting/membership, ghi in-app notification rồi tùy chọn gọi SES. Email lỗi không xóa record chính; lỗi cuối vào DLQ. SES identity/recipient phải được verify.

Google Calendar adapter gọn dùng server-side token tạo/cập nhật event và yêu cầu conference data khi hỗ trợ. CampusMeet lưu PENDING, READY, FAILED_RETRYABLE hoặc ACTION_REQUIRED. Hạn chế quota/quyền/artifact Google không chặn meeting; manual upload và consent-based transcription là fallback.

## 17. Bảo mật

Kiểm soát gồm default-deny theo nhóm; role IAM least privilege tách API/reminder/AI/deploy; S3 private mã hóa, OAC, block public access và presigned URL ngắn; DynamoDB encryption, conditional write, TTL/PITR; Parameter Store SecureString reference; schema validation, allowlist upload, size/checksum, output encoding, CORS hạn chế; consent/retention/version history; citation, authorization, preview, confirm và audit cho AI mutation.

## 18. Monitoring

CloudWatch structured log có request/correlation ID nhưng không chứa token hoặc raw transcript nhạy cảm. Metric/alarm bao phủ API error/latency, Lambda error/throttle, AIJob age/failure, Transcribe, Scheduler/SES, DLQ depth và Bedrock usage bất thường. Alarm gửi SNS operations topic. Runbook bao phủ retry, DLQ redrive, integration failure, recovery và cleanup.

## 19. CI/CD

GitHub Actions hiện chạy quality check. Pipeline mục tiêu cài dependency theo lockfile, lint, type-check, test, validate SAM, build frontend/Lambda và tạo artifact review được. AWS SAM deploy bằng environment parameter và GitHub OIDC role least privilege, không dùng key dài hạn. Production cần approval, smoke test, CloudFormation change review và rollback.

## 20. Kiểm soát chi phí

CloudFront, S3, Lambda, HTTP API, DynamoDB on-demand, Scheduler và job bất đồng bộ tránh server chạy rỗi. AWS Budgets/SNS cảnh báo; CloudWatch theo dõi phút Transcribe, Bedrock, storage và email. Giới hạn upload, retention, RAG scope, model/output, retry và concurrency. Lifecycle xóa artifact nonproduction; AI tốn phí có thể tắt độc lập. Budgets chỉ cảnh báo, không tự dừng tài nguyên.

## 21. Giai đoạn triển khai

| Giai đoạn | Deliverable |
| --- | --- |
| 1. Baseline | Chốt contract, status, threat model, access pattern, cost limit và kiến trúc. |
| 2. Core | Deploy năm bảng; hoàn thiện auth, authorization, repository, meeting, minutes, task và thay mock. |
| 3. Web | CloudFront, private S3/OAC, CORS hạn chế, smoke test và rollback. |
| 4. Automation | Calendar, Scheduler, notification, SES fallback, idempotency và DLQ. |
| 5. Content | User-content S3, upload verification, AIJob worker, retention và transcript. |
| 6. AI MVP | Transcribe, duyệt transcript, scoped Bedrock RAG, citation, minutes draft và TaskProposal có confirm. |
| 7. Operations | Tune alarm/budget, test bảo mật/chi phí, runbook, backup/restore và cleanup evidence. |

## 22. Rủi ro và giảm thiểu

| Rủi ro | Giảm thiểu |
| --- | --- |
| Nhầm skeleton là hệ thống đang chạy | Giữ bảng trạng thái; yêu cầu deployment evidence và smoke test. |
| Rò rỉ chéo nhóm | Authorize trước, encode group scope, kiểm tra mọi meeting ID và test isolation âm. |
| Audio thiếu/kém | Hiện capture state, benchmark ngôn ngữ, sửa/duyệt và trả lời thiếu nguồn trung thực. |
| Hallucination/mutation sai | Source allowlist, citation, grounded prompt, human confirm, idempotency và audit. |
| Upload không an toàn | MIME/size/checksum, server-owned key, expiry ngắn, safety state và không index trước READY. |
| Hạn chế Google/SES | Status rõ, retry giới hạn, manual upload, in-app fallback và verified identity. |
| Job trùng | Conditional claim, idempotency key, state machine, DLQ và controlled redrive. |
| Chi phí ngoài dự kiến | Budgets/SNS, usage dashboard, quota, retention, AI context/output giới hạn và không có server always-on. |

## 23. Kết quả mong đợi

MVP cung cấp một workspace bảo mật cho meeting, minutes, task, reminder, transcript và tri thức có citation. Dự án chứng minh kiến trúc AWS serverless, authorization cô lập nhóm, upload trực tiếp private, speech/AI bất đồng bộ, AI do con người kiểm soát, vận hành có quan sát, SAM delivery lặp lại và cost control đo được.

Thành công yêu cầu test chức năng/bảo mật, deployment evidence, câu trả lời có citation, không retrieval chéo nhóm, lỗi có thể phục hồi và tài liệu luôn phân biệt source implementation với trạng thái vận hành.
