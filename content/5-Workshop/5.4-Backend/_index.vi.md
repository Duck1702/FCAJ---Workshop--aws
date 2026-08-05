---
title: "5.4 Triển khai data foundation và backend"
date: 2026-08-05
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---
# 5.4 DATA FOUNDATION VÀ BACKEND

**Mục tiêu:** preview quy trình IaC và chỉ deploy khi được ủy quyền. **Thời lượng:** 30–45 phút. **Điều kiện:** quality gates đạt; deployment owner đã review account, Region, IAM, cost và `infra/parameters.example.json`.

{{% notice warning %}}
Application stack chưa được CampusMeet chứng minh deploy E2E. Core workshop có thể dừng ở validate/build. Không deploy chỉ vì template hợp lệ.
{{% /notice %}}

Data stack phải đi trước application stack. Dùng đúng runbook `docs/huong-dan-trien-khai-aws.md`: validate `infra/data-foundation.yaml`, tạo change set với `--no-execute-changeset`, review không có replace/delete ngoài dự kiến, rồi deployment owner mới execute. Tên stack/parameter phải theo môi trường đã thống nhất; không tự bịa tên để né lỗi.

Sau deploy được phép, chạy helper có sẵn:

```powershell
powershell -NoProfile -File scripts/verify-data-foundation.ps1 `
  -Region <YOUR_AWS_REGION> `
  -TablePrefix <YOUR_TABLE_PREFIX> `
  -ExpectedAccountId <YOUR_ACCOUNT_ID>
```

Backend app chỉ được xét sau:

```powershell
npm run infra:validate
npm run sam:validate:app -- --region <YOUR_AWS_REGION>
npm run sam:build:app
```

**Kết quả/xác minh:** năm bảng `identity`, `collaboration`, `meeting-data`, `task-data`, `ai-work`; stack output/CloudFormation status hợp lệ; không Scan ở request path quan trọng. **Lỗi:** stack prefix sai, IAM `PassRole`, bucket/model parameter chưa có, resource Region chưa hỗ trợ. Quay lại change set/runbook, không sửa Console tùy tiện.

Tiếp theo: [5.5 Authentication và frontend](../5.5-auth-frontend/).