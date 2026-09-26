# SDR

**Session:** GrokBot cho SDR — ngày 2  
**Người trình bày:** Simon, đội ngũ xAI go-to-market, SDR team.

Đây là session mang tư duy *hệ thống* (systems-minded) rõ nét nhất trong số chín session. Luận điểm của anh: prompt cho từng task đơn lẻ thì dễ nhưng vô nghĩa; giá trị thực sự nằm ở một pipeline đầu-cuối (end-to-end) — prospect → enrich → context → sequence → outbound — vận hành dựa trên các routine, với **duy nhất một bot mà bạn thực sự trò chuyện cùng.** "Những gì hiệu quả với chúng tôi hôm nay đã không còn hiệu quả của ba tháng trước." Do đó, hệ thống thiết lập phải vừa dễ thay đổi, vừa dễ chia sẻ với chi phí thấp.

---

## Các nguyên tắc thiết lập anh nêu ra ngay từ đầu

1. **Chief of staff là ưu tiên số một.** Đây là con bot đầu tiên bạn cần tạo.
2. **Xây dựng mọi bot khác *thông qua* chief of staff.** Khi bạn tạo một bot qua chief, nó sẽ nắm rõ mục đích và mục tiêu cuối cùng của bot đó, để sau này nó biết nên delegate cho ai — và khi nào một task cần hai bot trao đổi trực tiếp với nhau.
3. **Một bot cho mỗi platform, không phải cho mỗi task.** Bot là những thực thể chạy song song. Một email bot sẽ xử lý toàn bộ tác vụ email (draft, send, inbox). Chỉ dựng thêm outbound bot thứ hai nếu bạn outbound trên một platform thứ hai.
4. **Mã hóa màu theo chức năng (colour-code).** Cam = research, một màu khác = Shakespeare / copy, xanh lá = đội quân account-research. "Khi chief of staff của tôi nhắn tin cho một bot màu cam, nhìn thoáng qua là tôi biết ngay pipeline đang ở giai đoạn research." Mọi thứ đều được delegate từ chief, nên màu sắc là cách anh theo dõi pipeline.
5. **Đừng để hệ thống phình to vô tội vạ (don't sprawl).** "Tôi cam đoan rằng đã có lúc tôi tạo ra quá nhiều bot. Việc đó chỉ thêm hỗn loạn và hại nhiều hơn lợi." Trước khi tạo bot mới, hãy tự hỏi: *tại sao email bot không kiêm luôn vai trò quản lý inbox? Liệu việc này có nên chuyển thành một routine thay vì tạo bot mới không?*

---

## Đội ngũ bot

| Bot | Màu | Nhiệm vụ | Tools |
|---|---|---|---|
| **Simon-bot** | — | Chief of staff. **Con bot duy nhất anh trò chuyện cùng.** Mọi bot khác đều được dựng từ đây | Calendar, toàn bộ các bot bên dưới thông qua delegation |
| **Shakespeare** | copy | Email. Được train tỉ mỉ theo đúng văn phong của anh | Gmail |
| **Web Search** | cam | Public signals: gọi vốn, tin tuyển dụng, tin tức | Exa |
| **Simon soldiers** (một đội quân) | xanh lá | Các sub-bot low-context. Web Search chia nhỏ batch công việc cho chúng | — |
| **Army huddle** | — | Group chat: Web Search giao việc, các soldiers báo cáo kết quả | — |
| **Customer / Voice of Customer** | cam | Context từ các deal closed-won và closed-lost: lý do họ mua, pain point là gì, nhóm tính năng sản phẩm nào | Salesforce, call recordings |
| **PLG bot** | cam | Internal usage: ai đã sign-up, nhóm power users, tính năng họ dùng, closed-lost | Salesforce, product data |
| **Ample Market** | cam | Enrichment: tìm và **verify** email để tránh bounce và giữ deliverability luôn sạch | Ample Market |
| **Company research** | cam | Tech stack, tin tuyển dụng, org chart ("ai đang dẫn dắt SDR team của họ") | Sumble |
| **Inbox manager** | — | Xếp hạng Slack qua đêm, lời mời họp, email đối ngoại thành thứ tự ưu tiên xử lý | Slack, Gmail, calendar |

Tất cả cuối cùng đều đổ về một file **CSV** — bộ sequencer, prospect queue, dashboard và danh sách "những gì tôi cần action hôm nay." "Một UI trông rất gớm. Bạn không nên nhìn vào đây nhiều, thậm chí chẳng cần nhìn. Đó là bộ não của GrokBot. Hãy tin tưởng rằng nó nắm đủ context và chỉ kiểm tra khi bạn thực sự cần."

---

## Pipeline

**Xác định ICP trước tiên.** Đối với một công ty giai đoạn early-stage: hãy hỏi chief xem những title nào thường đưa deal vượt qua stage 1, title/ngành nghề nào phản hồi tốt nhất với cuộc hẹn đầu tiên, và đối tượng đó khác với economic buyer như thế nào. Dùng phương pháp reverse-engineer từ chính mục tiêu của bạn ("tôi có thể chốt được bao nhiêu S1").

**Prospect.** Web Search + soldiers quét 100–200 account để tìm các signal hoàn toàn mới (net-new). PLG bot kéo dữ liệu sign-up và usage. Company research kéo stack và cơ cấu tổ chức.

**Enrich.** Ample Market tìm và test email.

**Context.** Chi tiết đắt giá mà anh cho biết chưa từng thấy ở nơi nào khác: AE của bạn vừa có một cuộc gọi với account đó vào *ngày hôm qua*. Transcript đó là mỏ vàng và cần định hình nội dung message của ngày hôm nay. Con bot nắm trọn dữ liệu này: "họ đề cập đến việc bị quá tải công cụ (tool fatigue), quá tải AI (AI fatigue)" — hoặc "Simon bên AI team đang nghiên cứu đúng vấn đề này, hãy liên hệ với anh ấy." Phần lớn những insight đó thường bị bỏ xót và không bao giờ được action. Ở đây, chúng được đưa vào sequence tự động.

Ví dụ phối hợp cross-bot mà anh trình bày: một prospect account bị đánh dấu là closed-lost. Chief hỏi Voice of Customer lý do tại sao → "hồi đó chúng ta chưa hỗ trợ một tính năng quan trọng." Chief nhận thấy từ dữ liệu closed-won gần đây rằng **bây giờ chúng ta đã hỗ trợ rồi**. Account đó lập tức được **xếp hạng cao hơn** trong sequence — re-engage lại đúng vào thứ họ quan tâm. Sau đó chief bàn giao cho Shakespeare cùng với context đặc thù ngành FinTech để lên draft.

**Sequence.** Các đầu việc mỗi ngày đều đi kèm với Gmail draft ID. Anh review từng email một trong Gmail. Hoặc hỏi trực tiếp chief:

> Đánh giá giúp tôi thang điểm độ tự tin của bạn rằng email này chính xác, chặt chẽ và khả năng cao họ sẽ thực sự phản hồi?

Hiện tại anh đã đạt đến mức tương đối tin tưởng bot — nhưng anh nhấn mạnh: ở giai đoạn đầu, hãy đọc kỹ từng bản draft và đưa ra feedback thẳng thắn.

---

## Train Shakespeare — xây dựng văn phong (voice)

Ai cũng bảo "sync email của bạn đi." Anh còn đào sâu hơn thế, theo đúng thứ tự này:

1. Lọc ra **các email tôi đã gửi cho các công ty bên ngoài khớp với account trên Salesforce mà tôi là SDR phụ trách.** (Không phải toàn bộ sent mail.)
2. Từ tập dữ liệu đó, lọc ra những email **nhận được phản hồi tích cực**, bỏ qua các phản hồi tiêu cực.
3. Đặt **trọng số cao hơn cho các phản hồi tích cực gần đây.** Sản phẩm thay đổi liên tục sau vài tháng; cách truyền tải thông điệp từ ba tháng trước có thể đã lỗi thời.
4. Vẫn tham khảo các message cũ hơn để giữ lại chất giọng tự nhiên, gần gũi của con người.

Tiếp theo là vòng lặp đánh giá (critique loop). Các bản draft đầu tiên gửi về trông như template — cùng một mẫu email chỉ thay mỗi tên người và công ty. Thế nên:

> Cho tôi xem ví dụ về các email này.

Và với từng cái một: *đây là lý do vì sao email này dở tệ và đây là cách sửa cho nó hay hơn.* Lặp đi lặp lại liên tục cho đến khi nó thoát khỏi khuôn mẫu thông thường.

> Không được để bất kỳ email nào trông giống như một chiếc template.

---

## Các Routine

| Routine | Thời điểm | Nội dung |
|---|---|---|
| **50 prospect mới** | hàng ngày | Được xếp hạng ưu tiên. **Top 5** là nhóm cần action đầu tiên vào buổi sáng: ai đó đã liên hệ với sales nhưng chưa ai phản hồi, hoặc ai đó vừa tải tài liệu khớp với một cuộc hội thoại đang diễn ra (live conversation). |
| Chặn lịch (Calendar blocking) | hàng ngày | Chief có quyền truy cập calendar: tự động chặn **15 phút lúc 9 giờ sáng** cho top 5, và **1 tiếng lúc 1 giờ chiều** cho 45 prospect còn lại. Nó học được thói quen phân bổ thời gian của anh. |
| Xếp hạng Inbox | mỗi sáng | Slack, lời mời họp mới, email đối ngoại → sắp xếp theo thứ tự cần action ngay bây giờ. |
| Quét signal của account | **8 giờ sáng mỗi ngày trong tuần** | Quét toàn bộ account để tìm các signal mới: sign-up, tải nội dung, inbound. "Tốc độ tiếp cận lead (speed to lead). Ngay cả một signal yếu nếu được action ngay khi còn nóng cũng sẽ nhận được phản hồi cởi mở nhất." |
| **Salesforce trigger** | khi thay đổi stage | Nếu một account chuyển từ stage 0 → 1, **hủy sequence (un-sequence)** các contact đó. Chạy song song với sequencer để bạn dừng outbound một deal đang có tiến triển. |

Một lịch trình thay thế anh đưa ra trong phần Q&A: bàn giao 250 prospect vào sáng thứ Hai cho cả tuần, nếu mức 50 prospect/ngày vượt quá nhu cầu outbound của bạn.

---

## Skills

- **Prospecting skill** — toàn bộ thông tin anh cần cho một account riêng lẻ: title, ICP persona, owner, lý do tiếp cận, enrichment, LinkedIn, địa điểm. Chạy cho các account mới phát sinh vào giữa ngày; phần còn lại chạy theo các routine.
- **ICP skill** — góc nhìn cập nhật liên tục của đội ngũ về việc ai là người mua GrokBot, được nuôi bằng dữ liệu từ các deal stage 1+ và Voice of Customer. Được chủ đích giữ dưới dạng **skill thay vì memory**: định nghĩa này sẽ thay đổi rất nhiều, và một skill chỉ cần update một lần là toàn bộ hệ thống đều có thể gọi dùng.

---

## Mô hình đội quân (The army pattern)

Web Search cần kiểm tra 200 công ty. Nó không thể gánh hết việc đó trong một context duy nhất. Vì vậy: tạo một group chat **army huddle**, nơi Web Search ra lệnh "mỗi bot nhận 40 công ty, chạy song song," các soldiers thực thi, và kết quả được đẩy ngược về Web Search — không gửi cho chief, cũng không gửi cho Simon.

Tại sao lại dùng group chat thay vì nhắn tin trực tiếp (Q&A): anh có thể mở huddle ra để kiểm tra xem output của từng sub-agent có chính xác và vận hành trơn tru không; đồng thời hệ thống có thể mở rộng quy mô tùy ý từ 5 lên 20 soldiers vì các soldiers này thuộc dạng **low-context** — Web Search chỉ bàn giao chính xác những gì mỗi sub-agent cần.

---

## 3 bài học rút ra (Takeaways)

1. **Làm trọn vẹn từ đầu đến cuối (end to end).** Giai đoạn đầu sẽ khá vất vả khi phải prompt qua lại liên tục và dựng hệ thống. Hãy kiên trì vượt qua, train nó theo đúng phong cách của bạn, và quả ngọt nhận lại là toàn bộ workflow sẽ tự chạy mà không cần đến bạn.
2. **Làm việc có chủ đích (intentional).** Xây dựng mọi thứ thông qua chief. Các bot phải tự nói chuyện được với nhau. "Bạn không nên tự vắt kiệt sức mình bằng việc phải tập trung cao hơn và nhắn tin cho nhiều bot hơn — việc đó cực kỳ mệt mỏi."
3. **Tư duy có hệ thống.** Đặt dấu hỏi trước mỗi con bot mới định tạo. Biến các cuộc chat lặp đi lặp lại thành các routine.

---

## Những câu hỏi Q&A đáng lưu lại

- **Chi phí token cho 50 lead/ngày là bao nhiêu?** "Nó phụ thuộc vào rất nhiều yếu tố" — bạn kéo bao nhiêu cuộc gọi Gong, bao nhiêu context từ Databricks, bao nhiêu enrichment mới so với lượng bản ghi Salesforce bạn đã có sẵn. Hãy hỏi bot xem chi phí đang dồn vào đâu và tối ưu như thế nào. Có thể cân nhắc phương án 250 lead/tuần.
- **Cách nhanh nhất để thấy hiệu quả (time-to-value) nếu tôi chưa biết gì?** Hãy nói chuyện với chief ở mức tổng quan (high level) về workflow hiện tại của bạn. Nó sẽ chỉ ra cho bạn những khâu nào nên tách ra thành bot riêng biệt.

---

## Bản tóm tắt để áp dụng ngay

1. Chief of staff: Dựng mọi thứ khác từ đây. Chỉ trò chuyện với duy nhất con bot này.
2. Một bot cho mỗi platform. Áp dụng mã hóa màu sắc.
3. Dựng một voice bot được train từ *các email gửi đi gần đây, đúng địa bàn phụ trách, và có phản hồi tích cực* — sau đó critique liên tục cho đến khi không còn email nào trông như template.
4. Xếp hạng batch công việc hàng ngày với một nhóm nhỏ ưu tiên "action ngay"; để chief tự động chặn lịch trên calendar.
5. Quét signal vào mỗi buổi sáng. Cài đặt CRM trigger để tự động un-sequence.
6. Dùng một đội quân sub-bot để research theo batch, báo cáo kết quả về bot cha trong group chat.
7. Đưa bất cứ thứ gì hay biến động (như ICP) vào skill, không lưu vào memory.

Tài liệu liên quan: [`sales.md`](sales.vi.md) (PG là phiên bản quy mô cấp AE của quy trình này), [`post-sales.md`](post-sales.vi.md) (vẫn là nguyên tắc kỷ luật "một bot duy nhất tôi nói chuyện cùng", áp dụng cho giai đoạn sau bán hàng).
