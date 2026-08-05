---
title: "5.6 Thực hành M2 Meeting Management"
date: 2026-08-05
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---
# 5.6 THỰC HÀNH M2 MEETING MANAGEMENT

**Mục tiêu:** hoàn tất core user flow và negative authorization test. **Thời lượng:** 45–60 phút. **Điều kiện:** web/API chạy local hoặc stack đã được owner deploy; có hai user thuộc hai group khác nhau.

## 1. Group và meeting

Đăng nhập User A, tạo/chọn Group A với quyền Group Admin. Tạo meeting qua giao diện với title, thời gian tương lai, agenda, organizer và attendees là active members. Backend yêu cầu `Idempotency-Key` cho POST; giao diện/service hiện tại tự xử lý contract này.

**Mong đợi:** meeting được tạo một lần, gắn trusted `groupId`; attendee không active hoặc thời gian quá khứ bị từ chối. **Kiểm tra:** mở chi tiết và timeline; reload vẫn thấy dữ liệu nếu dùng DynamoDB thật.

## 2. Cập nhật optimistic version

Mở cùng meeting ở hai phiên. Phiên thứ nhất cập nhật agenda/attendees/organizer. Phiên thứ hai gửi version cũ.

**Mong đợi:** update hợp lệ tăng version; stale version trả conflict thay vì ghi đè. Kiểm tra timeline/audit fields và dữ liệu cuối.

## 3. Hủy idempotent

Hủy meeting với lý do phù hợp rồi lặp lại cùng yêu cầu.

**Mong đợi:** trạng thái `CANCELLED` được giữ, history không bị nhân đôi và không hard-delete. Meeting đã hủy vẫn có thể là historical AI source nếu source đã được duyệt và authorization vẫn hợp lệ.

## 4. Cross-group isolation

Đăng nhập User B chỉ thuộc Group B; thử mở `meetingId` của Group A hoặc thay `groupId` trên request.

**Mong đợi:** `403`/không tìm thấy theo contract, tuyệt đối không trả title, attendee, agenda hay timeline. Backend phải resolve `meetingId → trusted groupId`, sau đó kiểm tra active membership; không tin `groupId`, `userId` hoặc role từ client.

## Lỗi thường gặp và bảo mật

- `401`: token thiếu/hết hạn; đăng nhập lại, không dán token vào tài liệu.
- `403`: user không active hoặc thiếu Group Admin.
- `409`: optimistic version cũ hoặc idempotency conflict; reload record trước khi thử lại.
- `404/501`: application stack chưa deploy route mới; quay về test local, không coi là lỗi Cognito.
- Không dùng dữ liệu cá nhân thật; xóa test data theo owner policy.

Tiếp theo: [5.7 Tasks, Minutes và Google](../5.7-follow-up-google/).