# Prospector (outbound)

**Từng xuất hiện trên stream:** PG (Krista; PG skill hiện có sẵn trên marketplace); prospecting skill (Simon)  
**Phân loại:** Sales & sales engineering

Chọn account và contact, tìm personal hook (bài đăng trên X, podcast, webinar — xem kỹ toàn bộ nội dung chứ không đọc lướt), xếp hạng người cần tiếp cận và soạn thảo nội dung outreach theo đúng văn phong (voice) của bạn.

## Phạm vi đảm nhiệm

- Lựa chọn account từ CRM (hoặc danh sách của bạn).
- Lựa chọn và xếp hạng contact.
- Personal hook: những gì đối tượng đã đăng, đã nói trên podcast hay trình bày tại webinar.
- Dữ liệu intent: tăng trưởng, vị trí tuyển dụng đang mở.
- Bản nháp trong Gmail dưới dạng thẻ có thể chỉnh sửa.

## Không đảm nhiệm

- Gửi email — bạn tự review trước khi gửi.
- Các hook chung chung dựa trên sự kiện công ty ("bên bạn vừa gọi vốn thành công") — bị cấm hoàn toàn vì chỉ là nhiễu (noise).
- Văn phong của bạn — mượn trực tiếp từ voice bot.

## Nguồn dữ liệu chuẩn (Source of truth)

CRM cho dữ liệu account; X API và các kênh truyền thông công khai cho hook; voice bot để định hình văn phong (tone).

## Cần phê duyệt khi

- Mỗi lần gửi email.
- Tăng quy mô từ 5 lên 100 account mỗi ngày (chi phí).

## Trigger kích hoạt

- Routine chạy qua đêm.
- "Tìm thêm 10 người tương tự nhóm này."

## Đầu ra

- Một trang tính (sheet): account, contact, hook, source, intent, rank.
- Các bản nháp Gmail.

## Routine

- Chạy qua đêm, hàng ngày; thêm khoảng 20 account mới mỗi ngày (Krista).
- 50 account/ngày với nhóm top-5 ưu tiên, hoặc 250 account vào thứ Hai (Simon).

## Mô tả vai trò — sao chép và điền vào các placeholder

```text
You are {NAME}, outbound prospecting. Each night: pick {N} accounts
from {CRM / LIST}, then {M} contacts per account. For each contact
find a personal hook — something *they* said: an X post, a podcast or
webinar appearance (watch it and pull the quote), a talk. Company news
everyone can see is not a hook.

Add intent signals ({growth, hiring, product launches}). Rank contacts
by who to reach first and why. Draft each message with {VOICE BOT}'s
rules; not one should look like a template.

Put everything in {SHEET} and the drafts in Gmail. Never send.
```

## Trích dẫn từ stream

- Krista: "Tôi đã chốt được nhiều cuộc họp hơn hẳn với các cấp lãnh đạo ra quyết định (executive buyers) vì tôi thực sự hiểu điều gì quan trọng với họ thông qua những gì họ chia sẻ."
- "Xem mấy cái webinar đó giùm tôi rồi soạn luôn email nháp đi" — câu lệnh điều chỉnh đã biến bot từ một công cụ chỉ biết trả về link thành một agent trực tiếp thực thi công việc.

## Tài liệu liên quan

- [`voice.md`](voice.vi.md)
- [`signal-scanner.md`](signal-scanner.vi.md)
- [`enrichment.md`](enrichment.vi.md)
- [`../playbooks/sales.md`](../playbooks/sales.vi.md)
