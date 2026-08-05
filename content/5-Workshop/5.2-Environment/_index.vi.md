---
title: "5.2 Chuẩn bị môi trường"
date: 2026-08-05
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---
# 5.2 CHUẨN BỊ MÔI TRƯỜNG

**Mục tiêu:** chuẩn bị máy local và account lab an toàn. **Thời lượng:** 20–30 phút. **Điều kiện trước:** biết Git, Node.js và AWS cơ bản.

## Công cụ

Cài Git, Node.js 22 LTS, npm 10+, AWS CLI, AWS SAM CLI và PowerShell. Hugo chỉ dùng xây report, không thuộc CampusMeet lab.

```powershell
git --version
node --version
npm --version
aws --version
sam --version
```

Clone repository CampusMeet theo URL do giảng viên cung cấp, sau đó `cd CampusMeet`. Dùng Region đã được deployment owner thống nhất; runbook hiện dùng `ap-southeast-1`, nhưng phải kiểm tra Bedrock model/Transcribe/S3 Vectors availability trước AI lab.

## AWS và Google prerequisites

- Dùng account sandbox/dev, MFA và principal cá nhân; không dùng root hằng ngày.
- Chỉ deployment owner có quyền CloudFormation/SAM/IAM cần thiết; người học khác không tự dựng stack trùng.
- Chạy `aws login` và `aws sts get-caller-identity`; không chép output account ID vào báo cáo.
- AI lab cần model access/quota phù hợp. Google lab cần OAuth consent/client/scopes theo tài liệu nhóm; token/secret không đặt trong Git.

**Kiểm tra:** các lệnh version thành công, Git tree CampusMeet sạch, identity/Region đúng. **Lỗi thường gặp:** Node sai major, SAM chưa vào PATH, CLI dùng nhầm profile/Region. Không chạy deploy khi chưa review quyền và chi phí.

Tiếp theo: [5.3 Khảo sát source và validate](../5.3-source-validation/).