# Agent Pack

Bộ quy tắc tích hợp sẵn (drop-in rules) dành cho các agent viết code và điều phối (orchestration), được đúc kết từ 3 ngày đội ngũ xAI Grok Bot build và launch sản phẩm live trực tiếp trên stream.

Đây không phải tài liệu sản phẩm. Đây là các phương pháp làm việc thực tế — những gì họ thực sự đã làm dưới áp lực thời gian, bao gồm cả những phần từng bị lỗi.

---

## Bộ tài liệu này gồm những gì

| File | Khi nào nên dùng |
|---|---|
| **`../AGENTS.vi.md`** | Luôn luôn dùng. Bộ quy tắc cốt lõi cho một agent làm việc trong repo. Bắt đầu từ đây. File này nằm ở thư mục gốc của repo để các agent tự động nhận diện. |
| **`VERIFICATION.vi.md`** | Thiết lập dự án để agent có thể tự kiểm tra công việc của mình. File mang lại hiệu quả đòn bẩy cao nhất trong pack. |
| **`ORCHESTRATION.vi.md`** | Khi bạn thiết kế một *team* gồm nhiều agent, chứ không phải chỉ prompt một agent đơn lẻ. |
| **`SKILLS-AND-ROUTINES.vi.md`** | Khi bạn muốn agent không cần phải nhắc lại cùng một chỉ dẫn lần thứ hai. |
| **`PROMPTS.vi.md`** | Khi bạn cần những cách diễn đạt thực sự hiệu quả. Bao gồm các pattern kèm thư viện prompt copy-paste. |

Hãy chọn những gì bạn cần. `AGENTS.vi.md` có thể hoạt động độc lập; các file trong thư mục này là tài liệu tham chiếu mà nó trỏ tới.

---

## Cài đặt

**Claude Code** — đặt `AGENTS.md` (hoặc `CLAUDE.md`) vào thư mục gốc của repo. Đặt thư mục này vào `.claude/` hoặc `docs/agents/` và tham chiếu đến nó từ file cốt lõi.

**Cursor** — đặt `AGENTS.md` ở thư mục gốc repo, hoặc chia nhỏ các phần vào `.cursor/rules/*.mdc`.

**Codex / Copilot / Windsurf / Cline** — hầu hết các công cụ này đều đọc `AGENTS.md` ở thư mục gốc repo. Kiểm tra tài liệu của từng công cụ để biết chính xác tên file được yêu cầu.

**Bất kỳ agent nào có system prompt** — paste nội dung `AGENTS.md` vào. File được viết để có thể đọc hiểu độc lập mà không cần ngữ cảnh nào khác.

---

## Tùy biến trước khi đưa vào thực tế

Bộ pack này được thiết kế có chủ ý theo quan điểm thực chiến (opinionated). Có hai điểm bạn cần điều chỉnh cho phù hợp với hệ thống của mình:

1. **Thiết lập mức độ tự chủ dựa trên phạm vi tác động rủi ro (blast radius).** Các con số về throughput đằng sau những thực tiễn này đến từ một dự án thử nghiệm dùng một lần kéo dài 72 giờ, nơi không ai đọc code. Nhưng cũng chính đội ngũ đó lại review từng PR một trên sản phẩm thực tế của họ. Hãy đọc kỹ `VERIFICATION.vi.md` → *The caveat, stated honestly* trước khi cấp quyền auto-merge.

2. **Điền các lệnh verification của riêng bạn.** `VERIFICATION.vi.md` mô tả cấu trúc của vòng lặp (loop), không phải lệnh CLI cụ thể của bạn. Không có nội dung nào khác trong pack hoạt động trơn tru nếu thiếu phần này.

---

## Tóm tắt trong một đoạn văn

Giao cho mỗi agent một nhiệm vụ duy nhất, cụ thể và đặt tên cho nó. Xây dựng verification loop trước khi tạo agent thứ hai. Yêu cầu agent tái hiện bug trước khi fix, và luôn đính kèm bằng chứng (proof) cho mọi thay đổi. Khi agent làm sai, hãy đúc kết thành nguyên tắc chung — đừng bao giờ kể lể câu chuyện cụ thể. Audit các routine hàng tuần, vì tần suất thực thi chính là nơi tiêu tốn ngân sách. Luôn giữ sự kiểm duyệt của con người (human gate) đối với migrations, deploys, chi phí và phân quyền, cho dù vòng lặp có đang hoạt động tốt đến đâu.

---

## Nguồn gốc

Được tổng hợp từ transcript của ba buổi livestream liên tiếp (~24 giờ tư liệu), trong đó ba kỹ sư đã build và ship một sản phẩm từ một repo trống bằng chính nền tảng agent của họ. Các trích dẫn đều là lời nói trực tiếp của họ. Các số liệu được nêu trực tiếp trên live và là các ước tính linh hoạt tại thời điểm đó.

File PDF đi kèm trong `../guide/` trình bày cùng nội dung dưới dạng tường thuật, bao gồm case study, log các lỗi gặp phải và phân tích bài toán kinh tế. Các ghi chú bóc tách có cấu trúc nằm trong `../notes/`.
