# Playbooks

Chín workshop theo từng vai trò trong ba ngày, mỗi workshop tương ứng một file. Mỗi buổi đều do một nhân sự xAI trực tiếp trình diễn các bot họ đang thực sự vận hành, live, trên một công ty demo. Mỗi playbook đều tuân theo cùng một cấu trúc:

**team** (ai phụ trách phần việc nào, kết nối với những gì) → **workflow khi vận hành trên stream** → **các prompt, giữ nguyên văn (verbatim) tối đa theo transcript** → **routine và cadence** → **các số liệu** → **những bài học họ chia sẻ** → **bản mẫu áp dụng (copy this)** (checklist thiết lập).

Đây là phần tài liệu dễ sao chép và áp dụng trực tiếp nhất trong repo. Bản hướng dẫn tổng quát tóm lược sáu playbook thành ba đoạn văn cho mỗi bên; còn tại đây, toàn bộ nội dung được trình bày đầy đủ chi tiết.

---

## Chín playbook

| Playbook | Người thực hiện | Ngày | Ý tưởng cốt lõi |
|---|---|---|---|
| [`engineering.md`](engineering.vi.md) | Ling Shi, engineer | 1 | Một playbook-owner bot phát thông báo tiêu chuẩn; P0 được định nghĩa một lần duy nhất; nightly cleanup với cơ chế merge kiểm soát bằng bằng chứng (proof-gated merge) |
| [`product-management.md`](product-management.vi.md) | Kevin De Parco + Roshan, product | 1 | Data → spec → design → EM chia nhỏ task → cloud agent, bot sẽ hiệu chỉnh cách đọc funnel của con người |
| [`founders.md`](founders.vi.md) | Shub, founder success | 1 | Bốn bot cho bốn đầu việc của founder; tích lũy thay vì loại bỏ; audit tần suất chạy routine |
| [`sales-engineering.md`](sales-engineering.vi.md) | Amrita, field engineer | 2 | Một repo-expert bot và một competitor bot tranh luận trong group chat; dạy task bằng cách ghi lại thao tác |
| [`sales.md`](sales.vi.md) | Krista + Mark Wright, GTM | 2 | Prospecting bằng personal hook từ X; bot xem webinar thay bạn; tự động cập nhật CRM từ transcript |
| [`sdr.md`](sdr.vi.md) | Simon, SDR | 2 | Dựng chief of staff trước, xây dựng mọi thứ qua nó; mỗi platform một bot; một đội ngũ sub-bot low-context |
| [`customer-support.md`](customer-support.vi.md) | David, user ops | 2 | Crawl → walk → run; phân tách KB thành public / internal / process; bảng traces và evals; $0.20–$2 cho mỗi ticket |
| [`post-sales.md`](post-sales.vi.md) | Blake, AI deployment | 3 | Một bot điều phối phía trước, không còn gì khác ping bạn; voice dump để bootstrap; tự cải thiện hàng tuần giới hạn ở một đề xuất |
| [`marketing.md`](marketing.vi.md) | Josh Kim, marketing | 3 | Sáu bot chạy một chiến dịch thẳng lên prod; sau đó một PM bot học các điểm bạn can thiệp rồi tiếp quản |

Nội dung không bao gồm: bài chia sẻ về RevOps / MarOps ngày 3 (Matthew, "hãy xây dựng tool, đừng đặt ra rule"). Phần này được đề cập trong [`../notes/day-3-notes.md`](../notes/day-3-notes.vi.md); nó không thuộc chín workshop theo vai trò.

---

## Những điểm chung đúc kết từ mọi buổi chia sẻ

Tổng hợp xuyên suốt cả chín workshop, các nguyên tắc tương tự luôn xuất hiện dù dùng từ ngữ khác nhau. Nếu chỉ lấy phần giao thoa cốt lõi:

1. **Một bot, một việc.** Bất kỳ presenter nào cũng nhấn mạnh điều này. Hãy khoanh vùng scope như một bản mô tả công việc (JD). Chỉ tách nhỏ khi scope phình to, không tách sớm hơn.
2. **Một bot giao tiếp trực tiếp với bạn.** Simon, Blake, Josh, Ling, Kevin đều thiết lập mô hình chief-of-staff / bot đầu mối (point-of-contact) cùng dàn chuyên gia chạy phía sau. Shub là ngoại lệ duy nhất đi ngược lại — anh thích nói chuyện trực tiếp với từng chuyên gia — và xem đó là sở thích cá nhân.
3. **Xây dựng các specialist bot *thông qua* chief bot** để nó nắm rõ ai làm việc gì. (Simon là người nêu rõ điều này nhất.)
4. **Nói một lần.** Chuỗi playbook của Ling, lượt quét tự cải thiện của Blake, triết lý "hạ sâu thêm một tầng" của Shub, quy tắc "khi bot tư duy sai, hãy dựng skill" của Roshan. Việc phải lặp lại một prompt là tín hiệu cho thấy hệ thống đang thiếu rule hoặc routine.
5. **Định hình voice trước.** Krista, Simon, Shub, Blake đều train voice bot dựa trên email thực tế đã gửi trước khi cho phép xuất bản bất kỳ nội dung nào. Công thức của Simon cụ thể nhất: đúng địa bàn (in-territory), phản hồi tích cực, ưu tiên trọng số gần nhất (recency-weighted), sau đó critique liên tục đến khi không còn cảm giác dùng template.
6. **Tạo bản nháp rồi mới gửi.** Lộ trình crawl → walk → run của David; nguyên tắc "chỉ draft, không bao giờ gửi" của Blake; "bắt đầu nhỏ và ít rủi ro" của Josh. Quyền ghi (write) phải được chứng minh qua năng lực thực tế.
7. **Routine: thiết lập ít hơn bạn nghĩ.** Krista: một hoặc hai lần một ngày. Blake: ba routine chạy mỗi 15 phút sẽ tạo ra hàng trăm tin nhắn mỗi ngày. Shub: mỗi 15 phút đồng nghĩa 100 lần chạy mỗi ngày; hãy chuyển sang dùng webhook. Kevin: nếu là no-op, không cần phản hồi gì cả.
8. **Bot trao đổi với bot; chỉ ping bạn khi thực sự quan trọng.** Các luồng thảo luận giữa các specialist bot không được làm sáng thanh sidebar của bạn.
9. **Để bot trực tiếp dùng sản phẩm.** Serena đặt vé máy bay Southwest. ProdBot duyệt qua toàn bộ trang web. Reply đọc trực tiếp ticket thực tế. Craig click thao tác trên flyloair.com. Chính máy tính của bot là vòng lặp kiểm chứng (verification loop).
10. **Yêu cầu bot tự thiết kế bot.** "Những bot nào sẽ giúp ích được cho bạn?" (Amrita), "hãy giúp tôi dựng một hệ thống" (Blake), "nút thắt cổ chai của chúng ta nằm ở đâu?" (Lauren, trong các build session).

---

## Danh sách bot theo tên qua các session

Phần tra cứu nhanh khi gặp một cái tên trong ghi chú và muốn biết nó thuộc session nào.

| Bot | Session | Vai trò |
|---|---|---|
| Craig / Ling Xixi, Cray, Steve, Hogan1QR, Jenny | engineering | chief, UI, DevX, infra, playbook owner |
| Cora, Emily, Einstein/Igor/Nova/Larry/Eileen, Ashley, PMP/Pete, Pixel, Ray | product-management | chief, EM, engineers, data, spec, design, tuyển dụng |
| CloseBot, ProdBot, StockBot, ProtoBot, YapBot, misc | founders | khách hàng, trạng thái sản phẩm, đối thủ, feedback→PR, voice, thùng rác |
| Mimi, Sherlock, Serena Williams, Battle Card Blair, Demo Drake, AI Radar | sales-engineering | slides, chuyên gia repo, tester đối thủ, và ba bot được Sherlock khởi tạo |
| Olive, PG, Echo, Customer Expert, Engineer | sales | chief, prospecting, live deck, kế hoạch account, câu trả lời kỹ thuật |
| Simon-bot, Shakespeare, Web Search, Simon soldiers, Customer, PLG, Ample Market, Company research, Inbox manager | sdr | chief, email voice, nghiên cứu, lực lượng bot, VoC, usage, enrichment, stack/org, triage |
| Build, Reply, Alert, Tune | customer-support | infra, tickets, leo thang sự cố (escalation), tự cải thiện |
| Gus, Frankie, Wally, Trudy, Scout, Franny, Harbor/Northwind/Brightline | post-sales | chief, follow-up, voice, nguồn chân lý (source of truth), radar, biểu mẫu, theo từng account |
| market researcher, product marketer, website ops, performance marketer, marketing analyst, project manager | marketing | team chiến dịch |

---

## Lưu ý

- **Flylo, XAir, Northwind, Harbor, Brightline, Craft** là các công ty và account demo. Mức giá, metric và nội dung ticket trong các bản demo này đều là dữ liệu giả lập phục vụ trình diễn.
- Các prompt được đánh dấu verbatim thể hiện chính xác theo những gì hệ thống auto-caption ghi nhận; các prompt đọc bằng giọng nói (dictated) đã được căn chỉnh nhẹ dấu câu. Khi prompt được dựng lại từ lời dẫn chuyện, nó sẽ được ghi chú là paraphrased (diễn giải lại).
- Tên sản phẩm trong phụ đề tự động bị nhận diện sai (GrokBot / Rockbot / Brockbot / Grock-Bot; "SpaceX AI"). Chúng đã được chuẩn hóa lại ở đây.
- Mọi con số đều được nêu trực tiếp tại buổi trình diễn và có thể thay đổi linh hoạt theo tình hình thực tế lúc đó.
