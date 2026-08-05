---
title: "5.5 Cấu hình authentication và frontend"
date: 2026-08-05
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---
# 5.5 AUTHENTICATION VÀ FRONTEND

**Mục tiêu:** kết nối web với Cognito/API đã được owner dựng. **Thời lượng:** 25–40 phút. **Điều kiện:** có ba output public, không cần nhận secret.

Owner validate/build auth theo runbook:

```powershell
sam validate --template-file infra/auth-integration.yaml --lint --region <YOUR_AWS_REGION>
npm run sam:build:auth
```

Tạo `apps/web/.env` cục bộ, tuyệt đối không commit:

```dotenv
VITE_COGNITO_USER_POOL_ID=<YOUR_USER_POOL_ID>
VITE_COGNITO_USER_POOL_CLIENT_ID=<YOUR_USER_POOL_CLIENT_ID>
VITE_API_BASE_URL=<YOUR_API_URL>
```

```powershell
npm run dev
```

Mở `http://localhost:5173`, đăng ký/xác nhận/đăng nhập theo cấu hình pool, rồi gọi health/profile/group. Với static hosting, build bằng `npm run build`; upload S3/CloudFront chỉ theo output và quy trình deployment đã review, không đoán bucket/distribution.

**Kết quả:** protected route hoạt động, request có JWT và API lấy identity từ claim tin cậy. **Kiểm tra:** refresh session, sign out, request thiếu/hỏng token trả `401`. **Lỗi:** URL có stage thừa, CORS AllowedOrigin sai, app client/user pool sai Region, quên restart Vite. Không log token.

Tiếp theo: [5.6 Meeting Management](../5.6-meetings/).