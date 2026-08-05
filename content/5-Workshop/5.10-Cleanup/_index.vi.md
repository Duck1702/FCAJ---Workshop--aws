---
title: "5.10 Cleanup"
date: 2026-08-05
weight: 10
chapter: false
pre: " <b> 5.10 </b> "
---
# 5.10 CLEANUP

**Mục tiêu:** dừng chi phí mà không xóa nhầm shared environment. **Thời lượng:** 15–30 phút. **Điều kiện:** deployment owner xác nhận chính xác stack/resource và retention.

{{% notice warning %}}
Không chạy lệnh xóa với placeholder. Người học không được xóa shared stack. Preview và xác nhận bucket/vector/knowledge source nào chứa dữ liệu cần giữ trước khi xóa.
{{% /notice %}}

## Thứ tự kiểm tra và dọn

1. Thu hồi/xóa Google test credential trong Google console nếu đã tạo; không commit credential.
2. Dừng live session/job; kiểm tra EventBridge Scheduler không còn one-time schedule mồ côi.
3. Xóa dữ liệu test trong user-content theo retention policy. CloudFormation không xóa được S3 bucket còn object/version.
4. Kiểm tra Bedrock Knowledge Base/data source, S3 Vectors bucket/index và ingestion job; dọn đúng dependency theo runbook/service console nếu AI resource thật đã tạo.
5. Owner xóa **application stack trước**, sau đó auth stack nếu không còn dùng, và data stack cuối cùng sau khi export/được phép xóa dữ liệu.
6. Kiểm tra CloudFront distribution/frontend bucket, Lambda/API/Cognito, Scheduler, SES configuration, SNS topic, CloudWatch alarms/log groups và DynamoDB tables đã biến mất hoặc được giữ có chủ đích.
7. Xem Billing/Cost Explorer và AWS Budgets; log retention có thể tiếp tục tính phí.

Runbook hiện yêu cầu cleanup có review nhưng không cung cấp một lệnh destroy an toàn dùng cho mọi môi trường; vì vậy workshop không bịa `sam delete`/stack name. Dùng CloudFormation Console hoặc lệnh owner đã review với chính xác `<YOUR_STACK_NAME>`.

**Xác minh hoàn tất:** CloudFormation không còn stack lab; bucket không còn object ngoài ý muốn; không còn schedule/job; Google token bị thu hồi; Billing/Budgets được kiểm tra. Ghi lại resource được chủ ý giữ và người chịu trách nhiệm.