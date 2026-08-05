---
title: "5.9 Monitoring và kiểm thử"
date: 2026-08-05
weight: 9
chapter: false
pre: " <b> 5.9 </b> "
---
# 5.9 MONITORING VÀ KIỂM THỬ

**Mục tiêu:** thu bằng chứng thành công/thất bại mà không lộ dữ liệu. **Thời lượng:** 20–30 phút. **Điều kiện:** có kết quả local hoặc stack được phép xem.

```powershell
npm run lint
npm run typecheck
npm run test
npm run build
npm run infra:validate
```

Ưu tiên xem test `authorization`, `meetings-cross-group-handler`, `meeting-service`, `dynamodb-meeting-repository`, `ai-request-service`, `ai-aws-adapters` và AI Worker adapters. Nếu stack đã deploy, mở CloudWatch log group từ CloudFormation outputs, lọc bằng request/job ID; kiểm tra API/AI Worker error và duration alarms/SNS nếu IaC đã tạo.

**Thành công:** health/auth/Meeting test đạt; user Group B không đọc Group A; retry không nhân meeting/job; log có request ID và trạng thái nhưng không có JWT, credential, transcript đầy đủ hay presigned URL. **Lỗi:** log group sai Region, chưa có invocation, alarm `INSUFFICIENT_DATA`, retention tạo chi phí. Template/alarm tồn tại không phải bằng chứng đã được kích hoạt thử.

Tiếp theo: [5.10 Cleanup](../5.10-cleanup/).