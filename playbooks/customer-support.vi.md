# Customer Support

**Phiên chia sẻ:** GrokBot for Customer Support — ngày 2  
**Người trình bày:** David, software engineer thuộc bộ phận user-ops tại xAI.

Buổi demo nặng tính thực hành và bám sát vận hành thực tế nhất trong cả chín phiên: gồm một ticketing system, một knowledge base phân tách public và internal, SOP, Stripe, Slack, cùng bốn bot xử lý ticket thực tế end-to-end — bao gồm cả việc thực thi refund.

Lưu ý của anh được nhắc lại hai lần: "Toàn bộ sản phẩm này còn rất mới. Tôi không nghĩ đã có một meta cố định hay một playbook rõ ràng nào. Hãy điều chỉnh bot khớp với cách bạn làm việc, chứ không phải ngược lại."

---

## Đội ngũ bot

Anh nhấn mạnh rằng bạn **không** nên bắt đầu ngay với bốn bot. Hãy khởi đầu với một bot có tên gọi chung chung, dạy nó một workflow duy nhất, rồi tách nhỏ ra khi scope mở rộng hoặc khi cần chạy song song hai tác vụ cùng lúc. Đây là cấu trúc cuối cùng anh thiết lập:

| Bot | Nhiệm vụ | Kết nối với |
|---|---|---|
| **Build** | Thiết lập và hạ tầng. Cài đặt các connector, tạo bảng evals và traces | Plane, Notion, Supabase (Postgres), Slack |
| **Reply** | Trả lời ticket trên ticketing system và giải đáp câu hỏi trên Slack theo quy trình bằng văn bản | Plane, Notion, Stripe, Slack |
| **Alert** | Nhận ping từ Reply khi phát sinh việc cần con người can thiệp ngay. Post vào kênh Slack alerts và tag anh vào | Slack |
| **Tune** | Tự cải thiện. Cập nhật knowledge base (cần phê duyệt). Review các ticket tuần trước để tìm điểm cần tối ưu | Notion, traces |

Công ty demo: Vẫn là Flylo. Chỉ dùng một SKU duy nhất để giữ mọi thứ đơn giản: gói subscription Wi-Fi trên máy bay, 20 USD/tháng, thanh toán định kỳ hàng tháng.

---

## Knowledge base (dùng Notion cho buổi demo)

Gồm ba phần, và việc phân tách này đóng vai trò then chốt:

1. **Public docs** — những gì người dùng có thể tìm thấy qua tìm kiếm: sản phẩm, auth, billing, FAQ. Reply có thể trích dẫn các tài liệu này.
2. **Internal policies** — những gì một nhân viên billing con người cần nắm nhưng không công khai ra ngoài. SOP cho refund: *đã đăng ký ≤ 14 ngày → duyệt, hủy, hoàn tiền. > 14 ngày → từ chối.*
3. **Quy trình của agent** — vòng lặp mà Reply thực thi trên mỗi ticket. Anh yêu cầu nó phải đọc lại quy trình này mỗi lần chạy để các cập nhật có hiệu lực ngay lập tức:

   > Đọc ticket → tìm knowledge liên quan → quyết định: phản hồi hay hand off → hành động → để lại note.

Chỉ cần thêm bước 7 vào tài liệu quy trình, nó sẽ lập tức live ngay ở ticket tiếp theo.

---

## Lộ trình crawl → walk → run

"Để một agent tự động trả lời khách hàng có thể đem lại cảm giác e ngại. Hãy nâng cấp dần theo từng nấc."

1. **Crawl** — chỉ đọc ticket. Tóm tắt, chỉ ra vấn đề cốt lõi (root issue), có thể soạn sẵn bản nháp phản hồi *cho bạn duyệt.*
2. **Walk** — thêm bản nháp dưới dạng **internal note** ngay trên ticket. Bạn sẽ xem qua trước khi nội dung được gửi đi.
3. **Run** — trả lời trực tiếp cho người dùng và thực thi các hành động thực tế (trên Stripe).

Đi đôi với lộ trình trên: **ưu tiên read-only trước, cấp quyền ghi (write) sau, và phân quyền riêng cho từng bot.** Một bot có thể được phép post tự động lên Slack; bot khác thì bắt buộc phải hỏi trước. Knowledge base là khu vực anh giữ quyền kiểm duyệt của con người ngay cả ở giai đoạn "run": nếu một nội dung sai sót lọt ra ngoài và 100 người cùng hỏi một vấn đề, đó sẽ trở thành một sự cố (incident).

---

## Quá trình xử lý 4 ticket thực tế

**Alex — "Tôi quên mật khẩu."** Tình huống cơ bản.

> Chào bạn, bạn có thể trả lời Alex trên Plane không?

Reply thực hiện đúng vòng lặp và để lại một **thinking note** trên ticket: *có thể phản hồi với độ tin cậy cao; xác định root issue; tài liệu nguồn được tham chiếu* — sau đó gửi phản hồi với các bước chính xác lấy từ Public → Auth → Quên mật khẩu.

**Ben — SSO / Okta.** Không có trong knowledge base mẫu.

> Thử trả lời Ben trên Plane xem.

*Không có public docs hay internal policy → độ tin cậy thấp → hand off.* Bot để lại note **và gửi tin nhắn cho Alert**, vì anh đã thiết lập sẵn một rule: khách hàng enterprise có dấu hiệu bị khóa tài khoản sẽ được escalate ngay. Alert post vào kênh Slack alerts và tag anh. Giám sát theo thời gian thực đối với các ticket quan trọng mà không cần phải đọc từng ticket một.

**Carter và Damon — yêu cầu hoàn tiền.** Hai người dùng; Carter vừa đăng ký hôm nay (gia hạn sau một tháng), Damon sẽ gia hạn sau khoảng 10 ngày nữa (tức đã dùng được khoảng 20 ngày). Theo SOP: duyệt cho Carter, từ chối Damon.

> Giờ hãy thử trả lời Carter và Damon. [paste kèm customer ID]

Cả hai phản hồi đều đã được gửi đi. **Carter: đã hủy gói và hoàn tiền — xác nhận thành công trên Stripe (active → cancelled, payment refunded).** Damon: từ chối hoàn tiền, *nhưng không dẫn nguyên văn quy tắc 14 ngày nội bộ* — diễn đạt khéo léo, đồng thời đề xuất lên lịch hủy dịch vụ vào cuối chu kỳ để không bị tính phí tiếp. Dữ liệu Stripe của Damon được giữ nguyên. Các hành động trên Stripe có thể thiết lập duyệt thủ công hoặc không; "bạn hoàn toàn có thể để nó chạy tự động."

**Elena — "Tôi có thể chia sẻ gói Wi-Fi không?"** Không có trong KB.

Reply phân loại root issue (chia sẻ gói truy cập), tìm kiếm nhưng không thấy câu trả lời, để lại note hand-off và **đề xuất nhờ Tune bổ sung vào knowledge base** — bot chủ động hỏi ý kiến chứ không tự tiện chỉnh sửa. Anh phê duyệt:

> Thêm mục chia sẻ gói truy cập vào FAQ. Nêu rõ là không được phép. Định dạng bằng chữ màu xanh lục.

Tune tiến hành cập nhật. Sau đó:

> Bạn có thể thử trả lời lại khi thông tin đã được bổ sung không?

Reply phản hồi với độ tin cậy cao và đính kèm link dẫn đến phần nội dung mới.

**Hỏi đáp nội bộ qua Slack.** Một đồng nghiệp phụ trách mảng billing hỏi cùng con bot đó:

> SOP hoàn tiền quy định thế nào?

Bot trả lời dựa trên chính sách *nội bộ*, vì người hỏi là nhân sự nội bộ. Cùng một KB, cùng một bot, nhưng đối tượng tiếp nhận khác nhau. "Các agent hỗ trợ khác thường chỉ tập trung trả lời khách hàng; việc tận dụng chính KB sẵn có để giải đáp cho đội ngũ nội bộ lại thường bị bỏ quên một cách đáng tiếc." Use case: Đội ngũ GTM chuẩn bị cho cuộc gọi về bản release hôm nay; hoặc một quản lý muốn kiểm tra lịch sử thay đổi của chính sách hoàn tiền theo thời gian.

Ghi chú ngoài lề từ phiên chạy: Bot đã xin quyền trước khi post lên Slack. Anh thiết lập "always allow". Ngoài ra, "về cuối nó thậm chí còn cẩn trọng hơn mức tôi mong đợi — nhưng với các thao tác ghi (write) thì cẩn thận như vậy lại tốt hơn."

---

## Use case cảnh báo (Alerting)

Bất kỳ ai trong team sở hữu một bot kết nối với ticketing system đều có thể ra lệnh:

> Hãy tạo một routine quét qua các ticket mỗi giờ một lần, phân loại xem có người dùng nào đang có nguy cơ rời bỏ dịch vụ (churn) mà đã là khách hàng từ sáu tháng trở lên hay không, rồi gửi cảnh báo về kênh Slack này.

Không cần đến sự can thiệp của kỹ sư.

---

## Evals và traces — cách debug hệ thống

Được thiết lập thông qua Supabase connector trên Build:

> Hiện bạn đã có quyền truy cập vào Postgres. Hãy tạo một bảng traces và một bảng evals. Ghi dữ liệu vào các bảng này mỗi khi tôi yêu cầu chạy eval hoặc tạo một eval mới, và mỗi khi bạn chạy trace hay xử lý một ticket — dù là nội bộ, bên ngoài, dry run, hay thậm chí chỉ là để lại một note.

Mỗi lượt chạy ghi nhận: thời gian thực thi, các file bot đã xem qua, các file thực tế được sử dụng (thường là danh sách đã qua chọn lọc). Từ đó, bạn có thể định vị chính xác vị trí phát sinh lỗi. Evals là những bài test bạn cần chạy lại mỗi khi thay đổi bất kỳ thành phần nào. Và Tune sẽ đọc các trace này để tìm ra điểm cần cải thiện — "đây là một bước đột phá lớn."

---

## Các con số thống kê

- **1–2 USD mỗi ticket** đối với các ticket có độ phức tạp từ trung bình đến cao theo cách anh vận hành (chạy classifier trước mỗi ticket, ghi lại traces và evals đầy đủ).
- **Khoảng 0,20 USD mỗi ticket** đối với các ticket billing đơn giản ("người dùng chỉ yêu cầu hoàn tiền," "người dùng hỏi về email họ vừa nhận được") sau khi bạn phân nhóm (bucket) chúng: chạy script để gom nhóm trước, sau đó xử lý phản hồi đồng loạt cho cả nhóm.
- Các sản phẩm support agent cạnh tranh trên thị trường tính phí theo từng lượt xử lý (resolution), "dao động từ 1 đến 10 USD, tính theo cấp độ quy mô." Chi phí cho nhân sự con người: cao hơn rõ rệt.
- "Chi phí trên đạt được chỉ sau nửa ngày tinh chỉnh tối ưu."

---

## Q&A đáng lưu ý

- **Người dùng phi kỹ thuật có thể ghi dữ liệu thẳng vào production không?** Người thiết lập ban đầu (có chuyên môn kỹ thuật nhỉnh hơn hoặc nắm rõ kiến trúc) sẽ cung cấp cho những người khác một **template** đi kèm các guardrail có sẵn — ví dụ: "bạn không được tự ý cập nhật knowledge base." Giải pháp chặt chẽ hơn: lưu trữ KB trên **GitHub** thay vì Notion. Khi đó bạn có nhánh (branch), PR và code owner. Bot tự cải thiện sẽ phải mở PR, một bot phát hiện lỗi sẽ review, ping cho owner, và **chạy evals trên nhánh PR đó** trước khi phê duyệt. Cùng các thành phần cơ bản đó, nhưng dựng nên những chốt chặn thực sự an toàn.
- **Xử lý theo batch có tiết kiệm chi phí hơn không?** Có, và hãy cung cấp thông tin cụ thể. Lệnh "Trả lời Alex" sẽ khiến bot phải list toàn bộ ticket đang mở, tìm kiếm chuỗi ký tự "Alex", rồi mới đọc nội dung. Hãy cung cấp ticket ID trực tiếp. "Càng nhiều thông số chính xác để tra cứu nhanh thì càng tốt."
- **Hỗ trợ qua điện thoại thì sao?** Anh chưa triển khai; nhưng Matt trong buổi stream đã gắn số điện thoại cho bot (tham khảo luồng feedback cho voice agent ở ngày 3 tại [`../notes/day-3-notes.vi.md`](../notes/day-3-notes.vi.md)).
- **Nên bắt đầu từ đâu đối với một công ty mới trên stream?** Áp dụng nguyên lý 80/20: 20% use case tạo ra 80% khối lượng ticket. Hãy xác định nhóm đó, đưa chúng vào KB kèm SOP chi tiết, chạy evals liên tục cho đến khi bot xử lý trơn tru, *rồi mới* đưa bot ra tiếp nhận ticket thật.
- **Thiếu connector?** Hãy để GrokBot khởi tạo một cloud agent và tự build connector đó. Đừng thụ động chờ đợi roadmap hay phụ thuộc vào bên thứ ba.

---

## Checklist áp dụng ngay

1. Một bot, một workflow, sau đó mới chia nhỏ.
2. KB chia thành ba phần: public, internal và quy trình vận hành của chính agent. Bắt buộc bot phải đọc lại quy trình ở mỗi lượt chạy.
3. Crawl → walk → run. Read-only → notes → replies → actions.
4. Thiết lập rule cảnh báo gửi về kênh chung cho những ticket thực sự cần con người xử lý.
5. Kiểm duyệt thủ công đối với các thay đổi trên KB. Nếu KB có thể đưa vào git, hãy lưu trữ tại đó và sử dụng PR làm chốt chặn kiểm duyệt.
6. Ghi lại traces trên mọi lượt chạy ngay từ ngày đầu tiên; chạy evals cho mọi thay đổi.
7. Gom nhóm các ticket đơn giản và xử lý theo batch.
8. Tận dụng chính con bot đó để giải đáp các câu hỏi nội bộ cho đội ngũ của bạn.

Tài liệu liên quan: [`../agents/VERIFICATION.vi.md`](../agents/VERIFICATION.vi.md) — mô hình traces/evals ở đây chính là phiên bản tùy biến của verification loop dành riêng cho mảng customer support.
