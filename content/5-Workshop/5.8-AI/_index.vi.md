---
title: "5.8 Upload, Transcription và AI"
date: 2026-08-05
weight: 8
chapter: false
pre: " <b> 5.8 </b> "
---
# 5.8 UPLOAD, TRANSCRIPTION VÀ AI

**Mục tiêu:** hiểu security boundary và chạy validation/test hiện có. **Thời lượng:** 25–40 phút. **Điều kiện:** quality gates 5.3 đạt; AI cloud lab chỉ khi owner xác nhận model/quota/IaC.

{{% notice info %}}
Đây là **Advanced/Architecture walkthrough**. Upload và live transcript endpoints trong API contract chưa implement; không trình bày full pipeline là runnable.
{{% /notice %}}

## Luồng mục tiêu

1. API xác thực JWT, resolve `meetingId → trusted groupId`, kiểm tra active membership.
2. Browser upload trực tiếp qua presigned URL; backend `HeadObject` kiểm tra key/MIME/size/checksum.
3. Final transcript segment mới được lưu; partial chỉ hiển thị tạm, speaker dùng `Speaker N`.
4. AIJob chạy `QUEUED → PROCESSING → COMPLETED/FAILED/CANCELLED`; retry phải idempotent.
5. Chỉ source `approved=true` được ingest. Retrieval filter `groupId` trước generation và kiểm tra mọi selected `meetingId`.
6. RAG hỗ trợ current/selected/whole group, trả citation hoặc `insufficientContext=true`.
7. AI tạo Minutes draft/Task proposal; con người review và xác nhận qua standard API.

Meeting đã hủy có thể làm historical source nếu source được duyệt; không thể mutation meeting đó. Citation dùng URI nội bộ/timestamp/chunk, không lộ S3 key hay presigned URL.

## Lab an toàn

```powershell
npm run test -- services/api/tests/ai-request-service.test.ts
npm run test -- services/api/tests/ai-aws-adapters.test.ts
npm run test -- services/ai-worker/tests/aws-adapters.test.ts
npm run infra:validate
```

**Kết quả:** test chứng minh selected-meeting isolation, adapter/error mapping/citation theo fixture; không chứng minh cloud E2E. **Lỗi:** model access/Region, metadata filter sai, source chưa approved, job trùng khi retry. Không log transcript đầy đủ, prompt, token hoặc URL còn hiệu lực. AI/Transcribe/ingestion phát sinh chi phí.

Tiếp theo: [5.9 Monitoring và kiểm thử](../5.9-monitoring/).