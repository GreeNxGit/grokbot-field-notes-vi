# Sales

**Session:** GrokBot for Sales — ngày 2
**Diễn giả:** Krista (phụ đề ghi là "Crystal") và Mark Wright, xAI
go-to-market. Krista là power user của team; các template của cô chính là nền tảng khởi đầu cho toàn bộ sales team còn lại.

Đường cong trưởng thành (maturity curve), phiên bản sales: đặt câu hỏi → các tác vụ "dọn dẹp file"
→ **"tạo pipeline cho tôi" / "sắp xếp cho tôi một cuộc họp với công ty X"** dưới dạng một mục tiêu ủy quyền duy nhất → một đội ngũ bot phối hợp làm việc cùng nhau.

Bài thuyết trình nhắm thẳng vào những phần việc sales mà chẳng ai muốn làm: tạo pipeline,
trả lời các câu hỏi kỹ thuật hóc búa ngay trong call, dọn dẹp Salesforce,
dự báo doanh số (forecasting).

---

## Đội ngũ bot

| Bot | Nhiệm vụ | Kết nối với |
|---|---|---|
| **Olive** (tên chú chó của cô) | Chief of staff. Công việc hàng ngày: chuẩn bị buổi sáng, chuẩn bị cho cuộc họp lúc 9h, draft email trả lời dưới dạng card | Gmail, Slack, calendar |
| **PG** | Outbound prospecting. Skill PG hiện đã có trên marketplace | Salesforce, X API, Gmail |
| **Echo** | Cập nhật deck trực tiếp từ những gì khách hàng nói trong call | Granola (hoặc Gong), Google Slides |
| **Customer Expert** | Hiểu rõ từng strategic account trong lòng bàn tay. Cập nhật account plan sau mỗi cuộc gọi | Notion (database of record), Slack channels, usage data, changelog |
| **Engineer** | Trả lời câu hỏi kỹ thuật live trong call, sẵn sàng gửi cho khách hàng | Codebase, Slack |
| Salesforce next-steps updater | Chuyển transcript cuộc gọi thành các cập nhật pipeline theo đúng format của cô | Granola, Gong, Gmail, Slack, Salesforce |
| Travel & expense, X bot, chat "học tập" | Utility | — |

---

## Olive — chief of staff

"Lúc ngồi xe buýt đến văn phòng là lúc tôi dễ căng thẳng nhất: không mở laptop, nhưng
Slack và email thì đổ về liên tục." Olive chuẩn bị lịch trình trong ngày, buổi họp 9h sáng và
draft các câu trả lời. Khách hàng yêu cầu một bản one-pager → Olive draft sẵn email dưới dạng
card; cô chỉ việc chỉnh sửa, gửi hoặc hủy.

Quy trình thường nhật: quét inbox buổi sáng. Nhịp độ của cô rất có chủ đích: **một hoặc hai lần
mỗi ngày, không hơn** — "nếu không thì sẽ rất nhiễu." Mark thì ngược lại
(chief of staff + routine dày đặc, tốn nhiều chi phí hơn) và bù đắp lại bằng cách hỏi chính
bot các mẹo tối ưu hóa.

Olive cũng có thể đóng vai trò điều phối (orchestration): khi cần làm slide deck, nó gọi deck bot; khi
xử lý thông tin khách hàng, nó gọi bot của khách hàng đó để lấy account data. Krista chủ yếu
nói chuyện trực tiếp với từng bot chuyên trách vì thấy cách này tốn ít token hơn.

---

## PG — prospecting với hook cá nhân hóa

Quy trình chạy tự động qua đêm:

1. Chọn **năm account** từ danh sách của cô trên Salesforce (hoặc do cô chỉ định). Bạn
   có thể chọn 100; cô thì chọn năm account mỗi ngày.
2. Chọn **năm contact** cho mỗi account.
3. Với mỗi contact, trích xuất **các hook cá nhân**, không phải sự kiện công ty. "Công ty bạn
   vừa gọi vốn thành công" — họ đã nhận hàng ngàn email như thế từ các vendor khác rồi.
   Thay vào đó: CTO vừa post trên X về việc xây dựng một software factory; CEO
   vừa đăng bài ra mắt sản phẩm. Kết nối **X API** — "X là nơi rất nhiều
   lãnh đạo và công ty chia sẻ thông tin."
4. Kéo dữ liệu intent: tăng trưởng công ty, vị trí tuyển dụng, bất cứ dữ liệu nào bạn yêu cầu.
5. Tìm podcast và webinar mà contact đó từng tham gia rồi **xem chúng**,
   trích xuất các trích dẫn liên quan đến giải pháp bạn đang bán. Trước đây cô thường tranh thủ nghe
   trên đường đi làm; nhưng nghe webinar 45 phút cho mỗi prospect thì không thể scale được.
6. Xếp hạng danh sách contact xem ai nên được reach out trước.
7. Draft nội dung outreach trong Gmail dưới dạng các card có thể chỉnh sửa.

Trước khi gửi bất kỳ nội dung nào: **de-slop (lọc bỏ văn phong rác của AI).** Hãy nạp cho bot những bài viết bạn thấy ưng ý nhất —
quét email đã gửi trong Gmail, tin nhắn Slack, bài đăng trên X của bạn — để nó viết đúng giọng văn của bạn. "Tôi rất
kỹ tính về việc câu từ phải nghe giống giọng của chính mình."

Sau đó tiếp tục mở rộng: "bạn draft các email này rất tốt — giờ hãy draft các câu trả lời
trên LinkedIn, rồi tìm thêm 10 người nữa." Mỗi ngày bổ sung thêm khoảng ~20 người. Kết quả: "Tôi đã
book được nhiều cuộc họp hơn hẳn với các buyer cấp executive vì tôi nắm được điều gì thực sự
quan trọng với họ dựa trên những gì họ đăng tải."

---

## Echo — deck tự cập nhật ngay trong call

Chạy discovery call với tính năng ghi âm của Granola. Dừng Granola, chạy Echo. Nó sẽ lấy
transcript và cập nhật các slide — các use case đã thảo luận, các bước tiếp theo — nhờ đó bạn
rời khỏi discovery call với một slide deck phản ánh chính xác nội dung cuộc gọi. Mất khoảng hai phút.
Ngoài ra: "dịch slide này sang tiếng Nhật" cho khách hàng quốc tế.

Granola xử lý rất nhanh; Gong mất vài phút để load sau cuộc gọi, nên
Gong sẽ phù hợp hơn cho cuộc gọi *tiếp theo* hoặc email follow-up.

---

## Cập nhật next steps trên Salesforce

"Có ai thích cập nhật Salesforce không?" (Đúng một người giơ tay.)

Cô cung cấp cho bot format chuẩn của mình — **tên viết tắt, ngày tháng, kết quả đạt được của khách hàng, các bước
tiếp theo** — và bot sẽ lắng nghe các cuộc gọi trên Granola và Gong, lấy dữ liệu từ email và
Slack, draft bản cập nhật next-steps; cô review lại rồi bot sẽ push lên Salesforce.

---

## Customer Expert — mỗi strategic account một bot

Cô phụ trách một vài strategic account trên nhiều phòng ban, các kênh Slack nội bộ và
bên ngoài, các chuỗi email. Notion là database of record.
Sau mỗi cuộc gọi, bot sẽ cập nhật **account plan**: tín hiệu, kỳ hạn renewal sắp tới,
các bên liên quan cần tiếp cận, các cuộc gọi đã thực hiện, dự án đang triển khai, next steps.
Trích xuất dữ liệu sử dụng: "top 20 power user là ai?" — để team có thể thu thập feedback
trước kỳ renewal hoặc giới thiệu tính năng mới.

Tính năng mà cô cực kỳ thích: **changelog ↔ feature requests.** Khách hàng từng yêu cầu một
tính năng cách đây một tháng; lẽ ra cô đã quên mất. Khi tính năng được ship, bot sẽ gắn cờ:
*chúng ta vừa ship tính năng này, hãy reach out cho khách hàng đã yêu cầu trước đó.* "Cảm giác như thể chúng ta
xây dựng tính năng đó riêng cho họ vậy."

Nên dùng bot riêng cho từng client hay một chuyên gia chung cho tất cả client? Tùy sở thích. Cô dùng một bot cho mỗi
strategic account vì số lượng account cô quản lý không nhiều; các AE nắm hàng trăm account
sẽ có cách làm khác. Mark: dùng bot riêng cho từng khách hàng để theo dõi intent và ghi chú, có một chief of
staff điều phối, cộng thêm một "intent" bot chuyên dựng demo tức thì (on the fly).

---

## Engineer — trả lời ngay trong call, không cần đợi sau đó

> Bạn có thể gửi cho tôi câu trả lời dành cho khách hàng về cách thiết lập cloud agent không?
> Gửi kèm một vài kết quả thực tế mà Amplitude và Fair đã đạt được nữa nhé.

Nếu không có bot: phải kéo một kỹ sư vào call, hoặc đi tìm account manager phụ trách
Amplitude và Fair để hỏi. Khi có bot: các bước hướng dẫn được trích xuất trực tiếp từ codebase, kết quả
được lấy từ các account khác, format sẵn sàng để paste vào Slack vì "nó
đã hiểu rõ cách tôi làm việc." Mark: "đã bao nhiêu lần chúng ta phải nói *để tôi
tìm hiểu câu trả lời rồi phản hồi lại sau* — giờ đây chúng ta trả lời ngay trong call và đẩy nhanh
tiến độ bước tiếp theo."

---

## Sự chuyển dịch tư duy, qua lời kể của cô

"Khi mới bắt đầu, tôi đã nhầm lẫn khi dùng nó như một giao diện chat thông thường. Tôi bảo *hãy
đi nghiên cứu cái này* và nó trả về cho tôi các link để tôi tự bấm vào xem webinar. Tôi đã
chỉnh lại ngay: **không, hãy tự xem hết các webinar đó cho tôi rồi draft một email.**"

Hãy xem nó là một **doing partner (cộng sự thực thi)**, chứ không chỉ là thinking partner. Nó có một chiếc máy tính
riêng. Nó có thể làm việc xuyên đêm. Hãy để bot prospecting cày xới qua một danh sách dài
trong khi bạn ngủ và đón chào ngày mới với các bản draft sẵn sàng.

---

## Ba lời khuyên (kết luận của Mark)

1. **Kết nối tech stack của bạn.** Bất kể công cụ nào bạn dùng hàng ngày, hãy kết nối nó
   trước tiên. Đó là nơi tạo ra các khoảnh khắc "aha" nhanh nhất.
2. **Một bot, một nhiệm vụ.** Engineer, pipeline, travel & expense. Onboard chúng
   như các thành viên trong team; cung cấp đầy đủ tài liệu để mỗi bot hoàn thành xuất sắc nhiệm vụ đó.
3. **Thiết lập routine.** "Cài đặt và để nó tự chạy" — nhưng hãy tham khảo nhịp độ của Krista.

---

## Q&A đáng lưu ý

- **Token.** Hãy chạy ít routine hơn (từng có người đặt routine chạy mỗi 15 phút một lần). Sử dụng
  trực tiếp các bot chuyên trách thay vì điều hướng mọi thứ qua chief of
  staff. **Tận dụng MCP bất cứ nơi nào có sẵn** — Gong, Granola, Salesforce đều hỗ trợ
  sẵn — và chỉ dành tính năng computer use cho các công cụ chưa có MCP. "Rất nhiều
  công cụ sales chưa có MCP xịn."
- **Quản lý knowledge base khi dữ liệu phình to?** Tận dụng hiện trạng sẵn có. Với họ, mọi thứ
  nằm trên Notion và Slack, vì vậy cứ kết nối hai nền tảng đó. Salesforce là source of
  truth cho các deal. GrokBot đóng vai trò là **orchestration layer** bên trên Databricks,
  Salesforce, Notion, Slack — "chẳng có công ty nào gom hết toàn bộ dữ liệu về một nơi hoàn hảo cả."
- **Ủy quyền cho Cursor?** Chỉ cần gõ "spin up cloud agents" bằng ngôn ngữ
  tự nhiên; tài khoản Cursor của bạn đã được kết nối sẵn.
- **Giao tiếp với các phòng ban khác?** Vẫn dùng Slack như trước — nhưng bot có thể theo dõi
  kênh marketing thay cho bạn.
- **Chuẩn hóa setup cho cả team?** Lấy template của power user và
  chia sẻ lại để một sales rep mới vào có ngay năm bot sử dụng từ ngày đầu tiên. Tính năng role-based onboarding
  (GrokBot hỏi role của bạn và pre-load sẵn template) đang được triển khai.

---

## Bản tóm tắt áp dụng ngay

1. Chief of staff cho buổi sáng: chuẩn bị meeting, dọn inbox, draft câu trả lời. Chạy routine một
   đến hai lần mỗi ngày.
2. Bot prospecting: xử lý từng đợt nhỏ hàng ngày, tìm hook cá nhân trên X, xem
   webinar, xếp hạng prospect, draft outreach. Nhớ train giọng văn trước tiên.
3. Post-call bot giúp cập nhật CRM theo đúng format của *bạn* từ transcript.
4. Một bot chuyên trách cho mỗi strategic account, lưu account plan trên
   công cụ tài liệu của bạn, cập nhật sau mỗi call, liên tục theo dõi changelog.
5. Một bot engineer kết nối với codebase để trả lời kỹ thuật trực tiếp khi đang call.
6. Phản hồi cứng rắn ngay từ lần đầu tiên nếu bot chỉ ném cho bạn một mớ link.

Tài liệu liên quan: [`sales-engineering.md`](sales-engineering.vi.md) (Sherlock là phiên bản SE
của engineer bot), [`sdr.md`](sdr.vi.md) (phiên bản top-of-funnel
của PG, ở quy mô lớn).
