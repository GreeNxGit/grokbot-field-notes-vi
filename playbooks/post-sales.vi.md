# Post-Sales

**Phiên chia sẻ:** GrokBot cho Post-Sales — ngày 3  
**Người trình bày:** Blake, AI Deployment Manager tại xAI. Sau khi chốt deal, anh đảm bảo khách hàng nhìn thấy ROI rõ ràng: "hàng tá email, ngập tràn tin nhắn Slack, Teams, cùng vô số cuộc họp và trao đổi."

Đây là mô hình cá nhân hóa nhất trong số chín case study, đồng thời là tuyên ngôn rõ ràng nhất về kiến trúc **one-bot-in-front** (một bot đứng đầu điều phối). Câu nói ấn tượng nhất buổi chia sẻ: **"Gus là người bạn thân nhất của tôi."**  
Blake chỉ nói chuyện với duy nhất một bot. Gus chịu trách nhiệm quản lý từ 10 đến 20 bot còn lại.

---

## Sáu use case điển hình

| Use case | Chức năng |
|---|---|
| **Morning status board** | Cố định cùng một khung giờ mỗi ngày: các sự cố phát sinh qua đêm, lịch họp trong ngày, những việc đã hứa làm hôm qua nhưng chưa xong. |
| **Call prep** | Trước cuộc họp 15–20 phút: danh sách người tham gia, chức danh, nội dung trao đổi lần trước, tính năng mới launch gần đây, gợi ý chủ đề nên đề cập, bất kỳ điểm nghẽn nào cần tháo gỡ. Phục vụ theo thời gian thực (just-in-time), không dồn hết thông tin vào một cục buổi sáng. |
| **Follow-up desk** | Ngay khi cuộc họp kết thúc: các bot nhận transcript từ Granola, trích xuất context, draft câu trả lời, tạo tài liệu liên quan. Xử lý trọn vẹn "khoảng việc giữa các cuộc họp." |
| **Promise keeper** | Giám sát những việc anh đã cam kết sẽ làm và nhắc nhở đúng lúc. |
| **Ask watch** | Giám sát những việc anh yêu cầu *người khác* làm nhưng chưa thấy phản hồi — tránh để chúng bị trôi vào hố đen của email và Slack. |
| **Account reset** | "Tình hình tài khoản Harbor hiện thế nào?" → nhận ngay một gói báo cáo tổng hợp: rủi ro, đầu mối liên hệ, blockers, các cam kết còn mở, hoạt động Slack, các bước tiếp theo. Luôn mở đầu bằng một câu đùa về không gian vũ trụ, vì anh chỉ kích hoạt lệnh này mỗi khi thấy căng thẳng. |

"Đáng lẽ tôi có thể tạo tới 20 slide như thế này."

---

## Đội ngũ bot (Roster)

| Bot | Vai trò | Ghi chú |
|---|---|---|
| **Gus** | Chief of staff | Điểm tiếp xúc duy nhất (single point of contact). "Tôi rất dễ bị phân tâm. Tôi không muốn phải quản lý một đội 10 bot. Gus quản lý 10 bot đó. Còn tôi chỉ quản lý một đến hai bot, tùy theo từng ngày." |
| **Frankie** | Follow-ups | Draft các tài liệu sau cuộc gọi, ví dụ: tài liệu ROI theo đúng nhận diện thương hiệu của khách hàng. |
| **Wally** | Voice | Được train dựa trên email và tin nhắn Slack anh đã gửi. Nắm rõ từng **persona**: Slack nội bộ viết chữ thường, thoải mái, thi thoảng kèm emoji; email gửi exec thì ngắn gọn, đi thẳng vào vấn đề, trang trọng. Bot biết anh là "người thích dùng dấu chấm than." |
| **Trudy** | Source of truth | Nắm tài liệu nội bộ trên nhiều nền tảng. Gus sẽ hỏi Trudy mỗi khi câu trả lời cần trích dẫn nguồn chuẩn xác. |
| **Scout** | Internal radar | Trước đây anh phải theo dõi cùng lúc 30–40 kênh Slack kèm các thông báo qua email. Scout gửi một bản tổng hợp hằng ngày kèm link liên kết. Đây là con bot "thứ hai" thỉnh thoảng nhắn tin trực tiếp cho anh. |
| **Franny** | Forms / VM work | Tự tạo Google Forms trực tiếp qua giao diện người dùng trên máy ảo (VM) riêng của nó. |
| **Harbor, Northwind, Brightline** | Mỗi account một bot | Mỗi bot nắm toàn bộ context của một account cụ thể. "Như thể có một nhân sự riêng chuyên trách cho account đó." Khuyến nghị áp dụng cho tệp khách hàng quy mô vừa và nhỏ; "nếu có tới 1.500 account thì cần giải pháp khác tối ưu hơn." |
| **Staff meeting** | Group chat | Gus triệu tập các bot chuyên trách lại để phản biện và thảo luận đưa ra quyết định. |

**Kỷ luật thông báo (Notification discipline).** Không bot chuyên trách nào được phép ping trực tiếp cho anh. Gus nhắn tin cho Wally, Frankie, Harbor; các bot phản hồi lại cho Gus; chỉ duy nhất luồng trao đổi của Gus sáng đèn thông báo (chấm xanh). "Tôi không muốn bị làm phiền bởi đội ngũ 10 bot. Tôi chỉ muốn tập trung trao đổi với đầu mối duy nhất." Franny chỉ nói chuyện trực tiếp nếu anh chủ động nhắn trước cho Franny.

---

## Bản demo chi tiết từng bước

**1. Cử Gus tham gia cuộc họp.**

> Vào họp buổi internal learnings ngay bây giờ giúp tôi được không? Họp xong gửi bản tóm tắt các điểm chính nhé.

Gus mở Google Meet trên máy ảo, mute mic, tắt camera, nhập tên của anh và gõ vào khung chat: *chào mọi người, tôi là Gus, bot của Blake.* Sau cuộc họp, bot gửi lại các quyết định đã chốt (branding mới, link liên kết), các yêu cầu cần làm và người phụ trách. Anh không áp dụng điều này cho mọi cuộc gọi bừa bãi — "hãy cẩn trọng và chọn lọc kỹ các trường hợp để bot làm việc này."

**2. Gói tài liệu sau cuộc gọi (Post-call pack).**

> Tôi vừa họp xong với Harbor.

(Thông thường quy trình này tự động kích hoạt ngay khi transcript được đồng bộ; ở đây dùng prompt để phục vụ demo.) Gus nhắn tin cho **Wally** (voice), **Frankie** (follow-up), **Harbor** (account). Phản hồi nhận về: *"Gói tài liệu sau cuộc gọi với Harbor đã sẵn sàng. Chỉ là bản draft."* — bao gồm ba phần:

- Draft email trên Gmail gửi Maya và Priya, viết đúng giọng văn của anh ("Hey Maya! — chuẩn giọng của tôi").
- Draft tin nhắn Slack gửi Alex, AE phụ trách account, trong channel post-sales: *vừa trao đổi với Harbor xong, tuần tới sẽ gặp các champion mới, đang lên bản PDF phân tích ROI.*
- Bản **ROI one-pager** khách hàng yêu cầu trong cuộc họp, do Frankie tạo theo đúng branding của Flylo.

Anh chỉ cần chỉnh sửa nhẹ rồi bấm gửi. "Trước đây khâu này chắc mất ít nhất 45 phút."

**Chỉ draft (Drafts only)** là nguyên tắc bất di bất dịch: "Tôi đã quy định rất rõ ràng rằng bot tuyệt đối không bao giờ được tự ý gửi bất cứ thứ gì ra ngoài."

**3. Tác vụ trên máy ảo (VM work).** Nhắn trực tiếp cho Franny:

> Tạo giúp tôi form khảo sát ROI cho Northwind.

Franny mở Google Forms, tự click qua từng bước để dựng form — team và role, use case chính, tiêu chuẩn hoàn thành như thế nào — sau đó trả về share link và edit link, đồng thời hỏi anh có muốn gửi đi luôn không. Lưu ý của anh: tốc độ phụ thuộc vào tác vụ cụ thể, và nếu tạo form thường xuyên, hãy dùng tính năng *Teach a task* để bot không phải tự đoán các bước.

**4. Khởi động lại thông tin tài khoản (Account reset).**

> Tình hình tài khoản Harbor hiện thế nào?

Gus nhắn tin cho Harbor, Frankie và Scout, thông báo với anh "vẫn đang đợi Frankie và Scout phản hồi," rồi "giờ chỉ còn đợi Scout," sau đó trả kết quả: mở đầu bằng một câu đùa (*How do you organise a space party? You plan it.*), tiếp đến là các rủi ro hiện tại, danh sách nhân sự liên quan, blockers, các cam kết còn tồn đọng, hoạt động trên Slack và các bước tiếp theo cần triển khai.

Cách anh xây dựng prompt này (Q&A): anh nói rõ cho Gus biết mình thường bị tắc nghẽn và quá tải ở đâu, sau đó liệt kê những gì mình cần nắm — rủi ro, v.v. — và để Gus tự hoàn thiện phần còn lại. "Hãy bắt đầu với một prompt sơ khai rồi tối ưu dần từ đó."

**5. Họp nội bộ các bot (Staff meeting).**

> Mở cuộc họp nội bộ thảo luận xem tôi nên dành một tiếng rảnh rỗi tiếp theo và duy nhất trong ngày hôm nay để làm gì.

Gus gửi câu hỏi vào nhóm. Anh đã cài đặt để các bot **luôn luôn phản biện lẫn nhau** — "nếu chúng chỉ đồng ý răm rắp thì chẳng mang lại giá trị gì, nhất là với AI." Northwind: *đừng dành thời gian cho chúng tôi, lượng usage có giảm nhẹ nhưng vẫn ổn.* Brightline: *cũng không cần ưu tiên chúng tôi.* Harbor: *hãy ưu tiên xử lý SSO.* Frankie: *cần bảo vệ hạn mức năng lực, việc này phải giải quyết ngay lập tức; sau đó mới đến SSO.* Scout đồng tình. Gus tổng hợp và đưa ra kết luận: ưu tiên xử lý SSO trước, sau đó gửi email bước tiếp theo cho Maya (đã draft sẵn), tạm gác các việc còn lại sang một bên.

Cách tiếp cận này cũng rất hiệu quả để khử bias: "Tôi không muốn hệ thống lúc nào cũng thiên vị một khách hàng nhất định. Hãy lắng nghe đầy đủ các góc nhìn khác nhau trong phòng họp."

---

## Cách dựng hệ thống này từ con số không — voice dump

Anh ghi lại một **bản voice memo dài 10–15 phút** trên điện thoại trong lúc đi bực dọc đi lại: *tôi tên là Blake, đây là công việc của tôi, đây là những điểm tôi thấy thích, đây là những việc tôi đang gặp khó khăn, đây là các quy trình đang bị gãy, đây là những điểm chúng ta có thể làm tốt hơn.* Sau đó anh dán bản transcript vào con bot đầu tiên:

> Tôi cần một hệ thống vận hành hiệu quả cho công việc của mình. Hãy giúp tôi xây dựng nó.

Con bot phản hồi lại với cấu trúc rõ ràng: *anh thích trao đổi trực tiếp với khách hàng — vậy hãy bảo vệ quỹ thời gian trên lịch cho việc đó. Quy trình này đang bị gãy. Anh đang mất quá nhiều thời gian check Slack — hãy để tôi giải phóng việc đó cho anh.*

"Hãy bắt đầu từ context. Có rất nhiều thứ đang nằm sẵn trong đầu bạn. Hãy lấy chúng ra khỏi đầu, đưa vào GrokBot, và nó sẽ lo phần còn lại." Nếu bot bị tắc: *đừng dừng lại cho đến khi tìm ra giải pháp.*

---

## Các routine tự động

| Routine | Thời điểm |
|---|---|
| Daily brief | 8:30 |
| Cam kết chưa hoàn thành | 9:00 và 13:00 |
| Friday dashboard | Thứ Sáu |
| **Self-improvement scan** | **Thứ Tư** |

Routine tự cải tiến (Self-improvement scan) gồm hai phần:

1. **Audit toàn bộ hệ thống.** Theo dõi những việc anh đang làm thủ công — tự gửi tài liệu, tự check Slack, những tác vụ anh chưa từng nhờ bot — và đề xuất phương án tự động hóa.
2. **Học giọng văn (Voice learning).** Thu thập **phần delta (độ lệch) giữa bản draft của bot và nội dung thực tế anh gửi đi**, sau đó chuyển cho Wally để cập nhật lại rules. "Nhờ đó bot liên tục được cải thiện và ngày càng chuẩn hơn."

Ban đầu anh không đặt giới hạn nên bot đề xuất hẳn "10 con bot mới cần dựng" — bị overcorrection. Hiện tại: **chỉ tối đa một đề xuất mỗi tuần.** Nếu anh không đồng ý, anh sẽ push back và yêu cầu đề xuất khác. "Tôi tự nâng cấp hệ thống ít nhất mỗi tuần một lần."

---

## Những con số thực tế

- 75% thời gian hành chính dành cho các cuộc họp, do anh chủ động lựa chọn.
- Trước đây: ~6 tiếng họp cộng với 3–4 tiếng xử lý việc phát sinh xen kẽ = ngày làm việc 9–10 tiếng. Hiện tại: vẫn số lượng cuộc họp đó, nhưng vừa vặn trong 8 tiếng.
- Gus "hoạt động rất tốt khi quản lý tới 15 đến 20" bot cấp dưới. Chưa cần đến tầng quản lý trung gian (middle-manager). Nếu hệ thống bắt đầu quá tải, hãy bổ sung thêm một tầng — và nhớ làm rõ cấu trúc tổ chức này trong quá trình voice dump.
- Cài 3 routine chạy liên tục mỗi 15 phút một lần = "hàng trăm tin nhắn mỗi ngày." Đừng làm vậy.
- "Bạn không cần đến 45 con bot đâu."

---

## Q&A đáng lưu ý

- **Chi phí (Cost).** Được thiết kế để tối ưu chi phí, nhưng: điều khiển toàn bộ UI trên máy ảo (tạo form bằng cách click chuột) tốn kém hơn so với việc gọi trực tiếp qua API (dùng AI tích hợp sẵn của Google Forms rồi paste câu hỏi vào). Các routine chạy poll liên tục là nguyên nhân chính đẩy chi phí lên. Hãy giữ đội ngũ bot tinh gọn và chuyên biệt hóa.
- **Các phần mềm khác?** Tận dụng MCP/API nếu có sẵn; nếu không thì cài đặt trực tiếp phần mềm đó lên máy ảo.
- **Liệu bot có thay thế con người sở hữu account không?** "Hy vọng là không bao giờ." Con người luôn làm tốt nhất các công việc tiếp xúc trực tiếp với khách hàng. Mục tiêu là loại bỏ những đầu việc không cần đến bàn tay con người để từng giờ làm việc của con người mang lại giá trị cao nhất.
- **Hệ thống có giúp cải thiện tỷ lệ giữ chân khách hàng (retention) không?** Nó phát hiện được những việc mà trước đây anh không có thời gian để ý: một bản micro-ship khách hàng đang đợi → bot draft sẵn email và gắn cờ cảnh báo → duyệt gửi → củng cố niềm tin. Bot Harbor liên tục theo dõi account Harbor giúp phát hiện issue từ sớm. Khách hàng cảm thấy được chăm sóc chu đáo, cá nhân hóa hơn vì anh có đủ thời gian dành cho họ.
- **Các bot có tự chéo kiểm tra (verify) lẫn nhau không?** Không chạy peer-to-peer. Mỗi bot có verification loop riêng; Gus sẽ chịu trách nhiệm cross-check tổng thể trên tất cả các bot.
- **Có cần nhiều tầng quản lý bot không?** Hiện tại là chưa cần; tham khảo mốc 15–20 bot ở trên.

---

## Áp dụng ngay mô hình này

1. Dành 15 phút voice-dump toàn bộ công việc của bạn. Yêu cầu AI thiết kế một hệ thống vận hành.
2. Thiết lập mô hình one-bot-in-front. Tuyệt đối không để bot nào khác ping trực tiếp cho bạn.
3. Tạo một voice bot với các persona riêng biệt cho từng đối tượng giao tiếp.
4. Triển khai mô hình mỗi account một bot nếu danh mục khách hàng ở quy mô vừa và nhỏ.
5. Luôn áp dụng nguyên tắc drafts only đối với bất kỳ nội dung nào gửi ra cho khách hàng.
6. Cài đặt routine theo dõi cam kết cá nhân (promise keeper) và giám sát đầu việc giao cho người khác (ask watch) vào mỗi buổi sáng và chiều.
7. Thiết lập quét tự cải tiến (self-improvement scan) hằng tuần, giới hạn tối đa một đề xuất, đồng thời lấy delta giữa bản draft và bản gửi thực tế để nạp dữ liệu cho voice bot.
8. Tổ chức staff meeting giữa các bot khi cần ra quyết định; yêu cầu chúng phải luôn phản biện lẫn nhau.

Tài liệu liên quan: [`sdr.md`](sdr.vi.md) và [`marketing.md`](marketing.vi.md) cho hai mô hình "single point of contact" khác; [`founders.md`](founders.vi.md) cho YapBot, phiên bản Wally dành riêng cho founder.
