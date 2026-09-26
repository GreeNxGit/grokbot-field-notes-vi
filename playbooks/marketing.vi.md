# Marketing

**Session:** GrokBot for Marketing — ngày 3
**Người thực hiện:** Josh Kim, xAI marketing.

Một chiến dịch, từ nghiên cứu thị trường đến landing page trên production rồi đến báo cáo số liệu ads, được vận hành bởi sáu bot — và sau đó bot thứ bảy được giao nhiệm vụ học cách con người điều phối quy trình để tiếp quản toàn bộ việc orchestration. "Quản lý context, phối hợp các bên, điều phối luồng chạy của một chiến dịch — đó mới thực sự là nơi tay nghề chuyên môn phát huy giá trị, và là cốt lõi trong công việc của chúng tôi." Vì vậy, đó là phần được ủy quyền sau cùng.

Điều chỉnh mental model của anh dành cho dân marketing: chatbot là **thought partner** (cộng sự tư duy); còn đây là **doing partner** (cộng sự thực thi). "Chúng sẽ làm việc chính xác theo cách bạn làm."

---

## Đội ngũ (The team)

Được crowdsource từ nội bộ team xAI và trên marketplace ("ai có positioning bot tốt nhất, ai có ads bot xịn nhất"), sau đó gom lại. Sản phẩm demo: **XAir**, một hãng hàng không giả định đã có sẵn landing page, tính năng tìm chặng bay và một số định vị cơ bản.

| Bot | Nhiệm vụ | Quyền truy cập |
|---|---|---|
| **Market researcher** | Nghiên cứu sản phẩm và thị trường, tìm đối thủ, đọc website của họ, chỉ ra các khoảng trống thị trường | Web / trình duyệt riêng |
| **Product marketer** | Viết positioning brief, one-liner, packaging, value statement, dàn ý landing page, các biến thể ad copy | Google Docs, Google Sheets (MCP) |
| **Website ops** | Deploy landing page qua PR và push thẳng lên prod | Repo của marketing site (quyền write); bên dưới chạy Cursor cloud agent |
| **Performance marketer** | Dựng campaign shell và traffic copy lên hệ thống quảng cáo | Tài khoản Google Ads |
| **Marketing analyst** | Kéo kết quả về, phân tích, đưa ra đề xuất | Google Ads API |
| **Project manager** | Quan sát 5 bot còn lại cùng các can thiệp từ con người, sau đó vận hành chiến dịch end-to-end với vai trò đầu mối liên lạc duy nhất | Toàn bộ quyền truy cập, thông qua các bot khác |

---

## Chi tiết chiến dịch từng bước một

**1. Nghiên cứu thị trường** (ra lệnh bằng giọng nói):

> Này market researcher. Hãy nghiên cứu website của XAir, tìm hiểu sâu hơn về sản phẩm và thị trường chúng ta đang hoạt động. Sau đó làm một bài phân tích đối thủ: xác định và hiểu sâu về các đối thủ cạnh tranh, xem website marketing của họ, nắm bắt định vị của họ, và — quan trọng nhất — tìm ra các khoảng trống và cơ hội để chúng ta định vị chiến lược vượt lên họ trong chiến dịch marketing này.

Bot đã crawl dữ liệu từ website (gửi kèm ảnh screenshot từ trình duyệt của chính nó để cập nhật tiến độ), tìm ra các đối thủ cạnh tranh (những hãng hàng không có thật), và trả về các khoảng trống thị trường: **thời gian hữu ích (useful time), thiết kế lịch trình theo ngày (day design), các chuyến bay đường dài (long hauls)** — kèm đề xuất lấy thông điệp *thời gian hữu ích* làm trọng tâm trong mọi content.

**2. Positioning brief — kèm bước handoff.**

> Product marketer: hãy kết nối với market researcher bot để nhận handoff phần phân tích nó vừa thực hiện. Soạn một bản positioning brief: chiến lược go-to-market, các câu one-liner, định vị và packaging, value statement, kèm ví dụ cụ thể về cách triển khai trên 2-3 kênh marketing.

Bot đã nhắn tin cho researcher để lấy context ("Tôi vừa trao đổi vài tin nhắn với market researcher"), sau đó viết bản brief trực tiếp vào một **Google Doc**: tóm tắt research handoff, tệp khách hàng mục tiêu (*hành khách bay đường dài muốn lấy lại thời gian hữu ích*), định vị, ghi chú GTM, các one-liner theo từng góc tiếp cận, value statement và bản draft landing page cho quảng cáo trả phí. Josh chia sẻ rằng theo thời gian, các bot sẽ tự động chủ động handoff cho nhau; giai đoạn đầu thì ta vẫn cần ra lệnh cho chúng.

**3. Feedback giống hệt cách bạn góp ý cho người thật.** Anh để lại comment trực tiếp trong doc (*"đoạn này rất hay, đào sâu thêm vào hướng này"*), sau đó prompt:

> Tôi vừa để lại một vài comment trong Google Doc. Hãy đọc qua, tiếp thu feedback và cập nhật lại bản draft. Nhân tiện, hãy dựng luôn dàn ý chi tiết cho landing page. Đồng thời lên ý tưởng và draft một số chiến dịch Google Search để A/B test giữa các góc copy mà bạn vừa soạn.

Kết quả: các comment đã được resolve, dàn ý landing page hoàn chỉnh hơn, và một **bảng tính chứa các biến thể ad** — gồm tên biến thể, giả thuyết kiểm chứng, ad group, URL, copy — "bởi vì đó chính là cách dân performance marketing làm việc."

**4. Dựng campaign shell song song.**

> Performance marketer: bắt đầu tạo một campaign shell trên Google Ads. Tối ưu cho lượt click, vì chúng ta chuẩn bị test các góc copy và thông điệp mà product marketer đang draft.

Bot này đã được cấp quyền vào tài khoản Google Ads từ trước. Nó lấy các đoạn copy vừa được draft và dựng khung chiến dịch, liên tục gửi ảnh screenshot màn hình thao tác trên nền tảng theo thời gian thực.

**5. Deploy landing page.**

> Website ops: lấy nội dung landing page từ bản brief mới nhất mà product marketer vừa soạn và tạo một PR để đưa nó lên website thành landing page mới. Gửi screenshot tiến độ công việc để tôi theo dõi nhé.

Bot đọc brief, mở PR, gửi ảnh screenshot tiến độ, tạo bản preview, rồi **push thẳng lên prod** và gửi lại URL. "Tất cả đều diễn ra tại một nơi duy nhất quản lý toàn bộ công việc."

**6. Phân tích số liệu.** Anh không kích hoạt chiến dịch mới chạy live (việc đó cần ngân sách và thẻ thanh toán); thay vào đó anh dùng dữ liệu từ một chiến dịch đã chạy trước đó trong tuần.

> Marketing analyst: vào Google Ads, kéo dữ liệu từ đợt thử nghiệm thông điệp gần nhất về, phân tích và đưa ra bản TL;DR về các insight, kèm đề xuất cách đưa các insight này vào chiến lược marketing cũng như cập nhật lại các asset khác mà bạn thấy.

Kết quả trả về: biến thể chiến thắng rõ rệt (dựa trên thương hiệu, cam kết với khách hàng, khoảng cách giờ bay, chi phí mỗi ngày…), các chỉ số cốt lõi (chi tiêu, CTR, CVR) và các đề xuất hành động. "Vẫn có tranh luận về việc nên trao cho agent bao nhiêu quyền tự quyết định. Trong trường hợp này, bot đề xuất; team sẽ thảo luận và chốt."

**7. Bàn giao việc điều phối (orchestration).** "Các bạn có thể nhận thấy trong suốt buổi demo này, tôi là người trực tiếp điều phối mọi thứ. Đó là cái giá phải trả về mặt công sức."

> Project manager: hãy nghiên cứu và trao đổi với từng bot trong team để hiểu vai trò của từng con trong việc triển khai một chiến dịch. Đọc lại lịch sử hội thoại của chúng tôi để xem những chỗ nào tôi phải can thiệp, hướng dẫn hay đưa feedback, rồi đưa kinh nghiệm đó vào quy trình làm việc với chúng. Sau đó bắt đầu tự động hóa hoàn toàn quy trình này với 3 chiến dịch mới. Gửi screenshot và cập nhật tiến độ để tôi nắm được tình hình. Và quan trọng nhất: hãy đóng vai trò là đầu mối liên lạc duy nhất của tôi. Tôi không muốn phải nói chuyện với bất kỳ bot nào khác. Tôi chỉ muốn làm việc với một mình bạn để tránh bị phân mảnh context.

Bot PM đã phân tích 5 bot chuyên trách, lên danh sách 3 chiến dịch (nghiên cứu → định vị → landing page → …) và đưa vào hàng đợi. "Bạn không còn bị dội bom bởi 5 tin nhắn khác nhau từ 5 con bot nữa."

---

## Chia sẻ dưới dạng template

> Hãy tạo một bản template của chính bạn để tôi có thể chia sẻ cho người khác.

Bot sẽ đóng gói toàn bộ snapshot của nó — gồm context, memory, các routine xử lý — thành một cấu hình để đồng nghiệp (hoặc bất kỳ ai) có thể paste trực tiếp vào workspace của họ. "Họ có quyền truy cập chính xác vào cách mà bạn làm việc." Đây cũng chính là cách mà 6 bot ban đầu được tập hợp lại, và Josh cho biết các phiên bản hoàn thiện sau đó sẽ được đưa lên marketplace.

---

## Ghép một bot từ nhiều nguồn khác nhau (Q&A)

Cách anh xây dựng con bot product positioning, vốn là "sản phẩm của sự lười biếng":

1. Nói vài phút để định hình cách tư duy mà anh muốn ở bot.
2. Cho bot quét tone & voice từ website hiện có.
3. Tìm 2-3 bot ưng ý trên marketplace, **đưa URL của chúng cho bot**, rồi ra lệnh: *hãy scrape toàn bộ context và memory đó rồi áp dụng vào cách làm việc của bạn.*
4. Kết nối nó với **SuperMe** (một marketplace chuyên gia có plugin hỗ trợ) và yêu cầu: *thay vì làm nghiên cứu định tính diện rộng tốn kém, hãy kiểm chứng mọi việc bạn làm với các chuyên gia trên nền tảng này.*

---

## Những bài học rút ra (3 điểm cốt lõi)

1. **Scope bot thật chuẩn.** Giống hệt như viết bản mô tả công việc (JD): coi bot như một nhân sự mới, phân định rõ phạm vi trách nhiệm (swim lane), giới hạn phạm vi thật chặt để chúng chuyên môn hóa sâu. "Đó là cách bạn tối ưu hóa hiệu suất tối đa từ một đội ngũ bot."
2. **Tin tưởng bot của bạn.** Chúng rất tham vọng, chủ động và khao khát làm việc. Hãy cấp quyền truy cập: tích hợp Slack và email trước tiên rồi yêu cầu, *hãy nghiên cứu toàn bộ context này và cho tôi biết bạn có thể làm gì giúp tôi. Hãy gánh bớt việc cho tôi.*
3. **Đầu tư cho bot.** Sự phối hợp giữa bot với bot sẽ ngày càng mượt mà tự nhiên hơn qua từng câu prompt; feedback, context và memory sẽ tích lũy lũy tiến theo thời gian.

---

## Những câu hỏi Q&A đáng lưu ý

- **Nên giữ con người can thiệp (human in the loop) ở khâu nào?** Tương tự như khi onboard một nhân sự junior: đừng giao chìa khóa ngay từ ngày đầu. Bắt đầu bằng một tác vụ đơn giản, cấp 1-2 công cụ, theo dõi kết quả, rồi mở rộng dần. Bạn có thể chọn "always allow" cho một quyền hạn nào đó khi đã thấy yên tâm. Trạng thái lý tưởng là có thật: các kỹ sư xAI hiện sở hữu "đội quân bot liên tục push code lên prod". Hãy bắt đầu từ quy mô nhỏ với rủi ro thấp.
- **Rào cản lớn nhất đối với marketer là gì?** Chính là mental model. Nếu bạn coi nó như một công cụ tìm kiếm hay một LLM thông thường, bạn sẽ chẳng thu được gì nhiều. Hãy tích hợp nó vào Slack và để nó làm tuyến triage đầu tiên: *nhắn cho Josh khi có việc thực sự quan trọng, các trường hợp còn lại thì tự phản hồi.*
- **Nên tổ chức theo role hay theo initiative?** Role là các vị trí chạy thường trực (always-on) và học hỏi liên tục; initiative thì có điểm bắt đầu và kết thúc. Hãy đưa các bot theo role vào một **group chat riêng cho từng initiative** — ví dụ lập một "tiger team" chuyên tối ưu chuyển đổi website.
- **Có tùy biến được tính cách không?** Mỗi bot đều có bộ nhớ riêng: bạn hoàn toàn có thể yêu cầu bot PM nói chuyện theo phong cách Harry Potter và gọi bạn là phù thủy nếu thích. Avatar có thể được tạo mới và làm chuyển động chỉ từ một từ khóa. Bạn có thể trỏ bot tới một agent đã dựng ở nền tảng khác và yêu cầu *sao chép lại hành vi này*.
- **Vấn đề dữ liệu EU / training dữ liệu?** Có sẵn các chế độ opt-in/opt-out cho quyền riêng tư dữ liệu; bảo mật doanh nghiệp được xử lý theo từng trường hợp cụ thể cùng đội ngũ chuyên trách.
- **Tận dụng context từ các công cụ code hiện có thế nào?** Chỉ cần trỏ bot vào codebase hiện tại; nó sẽ sử dụng tài nguyên đó làm harness. Về bản chất, website ops sử dụng các Cursor cloud agent chạy ngầm bên dưới.

---

## Quy trình mẫu áp dụng ngay

1. Thiết lập 6 role: research, positioning, web, performance, analysis, PM. Tận dụng các mẫu bot sẵn có trên thị trường.
2. Xâu chuỗi chúng lại bằng cách chỉ định mỗi bot tự kết nối và nhận handoff từ bot đứng trước.
3. Để lại feedback trực tiếp trong tài liệu, sau đó yêu cầu bot vào đọc và tiếp thu.
4. Xử lý song song: dựng campaign shell ngay trong lúc copy vẫn đang được viết.
5. Yêu cầu gửi screenshot cập nhật tiến độ đối với bất kỳ thao tác nào can thiệp vào live system.
6. Khi quy trình đã trơn tru, yêu cầu một bot PM học lại những điểm bạn từng phải can thiệp để nó tự động tiếp quản.
7. Chia sẻ kết quả dưới dạng template.

Tài liệu liên quan: [`post-sales.md`](post-sales.vi.md) và [`sdr.md`](sdr.vi.md) để tham khảo các mô hình thiết lập một đầu mối liên lạc khác; [`product-management.md`](product-management.vi.md) cho chuỗi quy trình data → spec → design → ship tương tự ở mảng product.
