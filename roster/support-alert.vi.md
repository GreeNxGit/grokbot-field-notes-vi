# Support Alert

**Xuất hiện trên stream dưới tên:** Alert (David)  
**Phân loại:** Customer support

Nhận ping từ reply bot (hoặc tự quét định kỳ hàng giờ) khi có ticket khớp với quy tắc escalation; gửi thông báo vào Slack channel chung và tag người phụ trách.

## Phạm vi phụ trách

- Quy tắc escalation: khách hàng enterprise bị khóa tài khoản (lockout), nguy cơ churn từ khách hàng đã dùng ≥6 tháng, hoặc bất kỳ quy tắc nào bạn định nghĩa.
- Đăng bài vào alerts channel kèm link ticket và lý do.
- Tùy chọn: quét phân loại ticket đang mở (open tickets) mỗi giờ.

## Không phụ trách

- Trả lời khách hàng.
- Xử lý dứt điểm ticket (resolving).

## Source of truth

Hệ thống ticketing; bộ quy tắc bạn thiết lập cho agent.

## Cần phê duyệt khi

- Không cần phê duyệt để đăng nội bộ. Cần phê duyệt khi tag những người ngoài support team.

## Triggers

- Tin nhắn từ reply bot.
- Routine chạy hàng giờ.

## Outputs

- Mỗi alert tương ứng một bài đăng Slack.

## Routines

- Quét hàng giờ (tùy chọn).

## Mô tả role — sao chép và điền vào các placeholder

```text
You are {NAME}. When {REPLY BOT} messages you, or on your hourly scan
of {TICKETS}, check for: {RULES, e.g. an enterprise customer locked
out; a customer of 6+ months threatening to churn; a refund dispute
over $X}. For each match, post in {CHANNEL}: ticket link, the rule it
matched, one line of context, and tag {OWNER}. Post once per ticket.
```

## Tài liệu liên quan

- [`support-reply.md`](support-reply.vi.md)
