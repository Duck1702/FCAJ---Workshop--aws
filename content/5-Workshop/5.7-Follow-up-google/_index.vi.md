---
title: "5.7 Task, Minutes và Google Integration"
date: 2026-08-05
weight: 7
chapter: false
pre: " <b> 5.7 </b> "
---
# 5.7 TASK, MINUTES VÀ GOOGLE INTEGRATION

**Mục tiêu:** phân biệt capability mục tiêu với phần runnable. **Thời lượng:** 20–30 phút. **Điều kiện:** hoàn tất 5.6.

## Trạng thái hiện tại

| Phần | Trạng thái | Thao tác workshop |
|---|---|---|
| Minutes, Tasks, Dashboard | Handler skeleton trả `501` | Đọc contract/shared DTO và test; không lab cloud bắt buộc |
| Google OAuth/Calendar/Meet | Contract/kiến trúc đã chốt, adapter thật chưa hoàn chỉnh | Architecture walkthrough |
| Reminder/Scheduler/SES | Có resource/role trong IaC, chưa có bằng chứng lifecycle E2E | Optional inspection |

Theo SRS, Minutes chứa summary/discussion/decision/action item; action item chỉ thành Task sau xác nhận. Task đi `TODO → DOING → DONE`; dashboard tổng hợp việc cần chú ý. Với Google, meeting nội bộ phải tồn tại dù sync lỗi; Meet link chỉ hiển thị khi trạng thái sync `READY`. Reminder phải idempotent và bỏ qua meeting đã hủy.

**Xác minh:** mở `docs/api-contract.md`, `services/api/src/handlers/{minutes,tasks,dashboard}.ts` và integration adapter; quan sát `501` là expected boundary, không giả dữ liệu thành thành công. **Lỗi thường gặp:** coi organizer là global role, coi Google Meet là backend CampusMeet, hoặc commit OAuth secret. Không cấp scope rộng hơn cần thiết.

Tiếp theo: [5.8 Upload, Transcription và AI](../5.8-ai/).