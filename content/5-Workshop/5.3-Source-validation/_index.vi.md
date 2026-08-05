---
title: "5.3 Khảo sát source code và validate dự án"
date: 2026-08-05
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---
# 5.3 KHẢO SÁT SOURCE CODE VÀ VALIDATE

**Mục tiêu:** hiểu monorepo và tạo baseline chất lượng. **Thời lượng:** 25–40 phút. **Điều kiện:** hoàn tất 5.2, đứng tại root CampusMeet.

`apps/web` là React/Vite; `services/api` là Lambda API; `services/ai-worker` xử lý AI; `packages/shared` giữ DTO/type; `infra` giữ SAM/CloudFormation; `docs` là contract; `scripts` chứa validation.

```powershell
npm install
npm run lint
npm run typecheck
npm run test
npm run build
npm run format:check
npm run infra:validate
```

Nếu SAM sẵn sàng, validate các template mà không deploy:

```powershell
npm run sam:validate:data -- --region ap-southeast-1
npm run sam:validate:app -- --region ap-southeast-1
npm run sam:build:app
```

**Kết quả mong đợi:** dependency được cài; quality gates và infra validator exit 0; build web được tạo. **Xác minh:** lưu tên lệnh và exit code, không lưu secret. **Lỗi thường gặp:** lockfile/Node không khớp, thiếu SAM/Docker cho build, model/IaC lint phụ thuộc Region. Không “sửa nhanh” contract chỉ để test xanh.

Tiếp theo: [5.4 Data foundation và backend](../5.4-backend/).