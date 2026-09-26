# Sales Engineering

**Session:** GrokBot cho Sales Engineer — ngày 2
**Người trình bày:** Amrita, field engineer tại xAI. Cô cũng là người thực hiện demo GrokBot 101 vào ngày 1.

Ba bot cô dùng hàng ngày: hai bot nằm trong project demo (Flylo, hãng hàng không giả định), một bot thực tế chuyên làm slide cho cô. Câu nói lặp đi lặp lại: **"Finished work is the best part."** (Điều tuyệt nhất là công việc đã hoàn thành). Bạn quay lại để nhận một kết quả đã xong xuôi, chứ không phải một hàng đợi các quyết định cần xử lý.

---

## Đội ngũ bot

| Bot | Nhiệm vụ | Quyền truy cập |
|---|---|---|
| **Mimi** | Curator chuyên xử lý slide và dẫn chứng từ khách hàng (customer proof points) | Master deck trên Google Slides trên máy của cô. Case study, benchmark, mọi thứ gói gọn ở một nơi. |
| **Sherlock** | Chuyên gia kỹ thuật | Các product repo (booking backend, booking frontend). Chạy ngầm bằng Cursor cloud agent. Được định hướng để **không bao giờ để lộ IP** và diễn đạt câu trả lời phù hợp với khách hàng phi kỹ thuật. |
| **Serena Williams** | Tình báo cạnh tranh (competitive intel) | Máy tính riêng của bot. Tự đăng ký và dùng thử các sản phẩm đối thủ. Đặt theo tên tay vợt tennis nổi tiếng với việc nghiên cứu kỹ đối thủ. |
| *Echo* (của Krista, được giới thiệu trong session Sales) | Cập nhật deck trực tiếp (live) trong lúc gọi điện | Xem [`sales.md`](sales.vi.md). |

Cộng thêm ba bot do **Sherlock tự khởi tạo (spin up)** khi được yêu cầu (chi tiết bên dưới).

---

## Mimi — tạo slide case-study từ bài blog

Các slide case-study của cô đều dùng chung một template: **logo, sau đó là problem → solution → impact → quote.** Cô đưa cho Mimi một bài blog của khách hàng (hoặc ghi chú từ cuộc gọi) và bảo:

> I just want you to do your job for Jellyfish, and here is the blog post.
*(Cứ làm việc của mình cho Jellyfish đi nhé, bài blog ở đây).*

Trực tiếp tại buổi demo, với một bài viết của Salesforce ("cut legacy code coverage time by 85%"):

> Can you create another one of these slides for Salesforce for me? Blog post here: [link]
*(Tạo thêm một slide tương tự cho Salesforce giúp tôi được không? Link bài blog ở đây: [link])*

Mimi đọc bài viết, trích xuất bốn phần, **vào trang thương hiệu của công ty để lấy đúng logo**, tự dựng slide trên máy của nó (bạn có thể thấy con trỏ chuột của bot di chuyển trong Slides), chèn vào deck và gửi lại ảnh chụp màn hình. Mất khoảng 10–15 phút. Sau đó:

> Great work. Can you do one for Grab?
*(Làm tốt lắm. Làm tiếp một slide cho Grab nhé?)*

Những việc cô thường làm với Mimi:

- Xử lý hàng loạt (batch): "làm Salesforce, làm Grab, làm thêm cái này nữa" trong một tin nhắn duy nhất; Mimi tự xử lý hết cả khi laptop gập lại.
- Trước cuộc gọi: "ẩn các case study không liên quan đến khách hàng này" — Uber sẽ quan tâm đến Grab chứ không phải Salesforce.
- Theo định kỳ: *gần đây có bài viết nào của khách hàng nhắc đến chúng ta đáng để đưa lên không? Nếu có thì tạo slide nhé.* Nhiều công ty đăng bài mà không báo trước cho bạn.

Lý do dùng template: "Tôi không muốn bot tạo ra thứ gì đó quá cẩu thả hoặc dài dòng. Tôi biết chính xác mình muốn bố cục slide ra sao." Nhiệm vụ của bot là lo nội dung, không phải layout. Mỗi tuần cô làm slide cho 15–20 khách hàng.

Sự cố trực tiếp: Mimi bị mất đăng nhập Google ngay giữa buổi demo (do có sự cố gián đoạn dịch vụ trước đó). Cách xử lý: giành quyền điều khiển máy của bot, đăng nhập rồi trả lại quyền cho nó.

---

## Sherlock — trả lời các câu hỏi kỹ thuật mà không cần làm phiền engineer

Mô tả cô cung cấp cho bot: *You're a technical expert on Flylo and you support the website flyloair.com.* Đính kèm các repo liên quan. *If there's a customer issue, I want you to investigate and communicate what could be wrong to a customer.*

Ví dụ một câu hỏi từ hôm trước:

> How do we handle a race condition — two people booking the same flight or the same seat at the same time?
*(Hệ thống xử lý race condition thế nào khi hai người cùng đặt chung một chuyến bay hoặc một ghế tại cùng một thời điểm?)*

Sherlock khởi chạy cloud agent trong repo backend lẫn frontend rồi quay lại với kết quả: cơ chế bảo vệ nằm ở Postgres; hai khách hàng không thể cùng confirm chỗ cuối cùng trong khoang; và tách riêng **phần cần trả lời cho khách hàng** — *chúng tôi giữ chỗ cuối cùng trong khoang 10 phút khi bạn bắt đầu checkout; nếu có người khác bắt đầu trước, bạn sẽ nhận được thông báo.* Từ đó: "hãy draft nội dung này thành email và gửi cho khách hàng này," bước gửi đi chỉ thực hiện khi có sự phê duyệt của cô.

---

## Serena — tình báo cạnh tranh bằng trải nghiệm sản phẩm thực tế

Cô chỉ đưa cho Serena một đoạn mô tả, không cần hướng dẫn từng bước (walkthrough). Prompt đầu tiên:

> What are some competitor products that you think are worth testing against Flylo?
*(Những sản phẩm đối thủ nào đáng để test đối chiếu với Flylo?)*

Serena liệt kê các lựa chọn và chủ động **nhắn tin cho Sherlock** mà không cần nhắc: "codebase đặt vé hiện tại của Flylo hỗ trợ được những gì — tìm kiếm, chọn hạng vé, ghế ngồi, hành lý, và các thiếu sót hiện tại?" Sherlock trích xuất thông tin từ code và phản hồi lại. Sau đó:

> Let's go with Southwest and Spirit. Let's see how those booking experiences compare to Flylo.
*(Chọn Southwest và Spirit nhé. Hãy xem trải nghiệm đặt vé của họ so với Flylo thế nào).*

Serena bắt đầu chạy luồng đặt vé của Southwest trên máy tính riêng của nó (hiển thị ở góc trên bên phải). Đang làm dở, Amrita điều hướng thêm:

> Can you see if there are any AI travel agent capabilities in any of the competitor tools that we should be aware of?
*(Kiểm tra xem công cụ của đối thủ có tính năng AI travel agent nào đáng chú ý không?)*

Serena tiếp nhận yêu cầu thứ hai **mà không bỏ dở yêu cầu đầu tiên** — nó trả lời câu hỏi về AI agent trong khi vẫn tiếp tục click qua các bước trên Southwest. Khả năng đa nhiệm ngay trong cùng một thread chính là điểm mấu chốt: "các công cụ khác thường quên mất câu hỏi đầu tiên ngay khi bạn đưa ra câu hỏi thứ hai."

Quy trình định kỳ cô gợi ý: *hàng tuần, hãy tóm tắt các cập nhật trên tech blog của Expedia và Skyscanner, đồng thời báo cho tôi nếu họ vừa ship tính năng gì đáng chú ý.*

---

## Group chat: để hai bot cùng thống nhất roadmap

Cô đưa Sherlock và Serena vào chung một group chat (theo dõi tập trung ở một nơi thay vì phải click qua lại giữa các thread) và hỏi:

> What have you found is the key differentiator in our competitors that would be significantly low effort to build in the product?
*(Theo các bạn, điểm khác biệt then chốt ở đối thủ mà tốn rất ít công sức để đưa vào sản phẩm của chúng ta là gì?)*

Hai bot tự tranh luận dựa trên context riêng của từng bên:

- Sherlock: tính năng quản lý đặt chỗ (xem/đổi chuyến) là tiêu chuẩn bắt buộc (table stakes) ở mọi nơi — fix rất dễ. Và **calendar chọn ngày linh hoạt đã được code sẵn ở frontend nhưng chưa từng được gọi** — một điểm khác biệt kinh điển, cực kỳ dễ triển khai.
- Serena: chuyến bay khứ hồi và hành lý trông có vẻ lớn nhưng không hề tốn ít công sức; nó hỏi Sherlock xem giữa quản lý đặt vé và calendar thì cái nào khiến người dùng khó chịu hơn.

Sherlock tự `@`-tag Serena để hỏi thông tin cụ thể. Các bot sẽ tự làm việc đó mà không cần nhắc khi chúng đã nắm rõ bên nào nắm giữ dữ liệu gì. Từ đây: kết nối Google Docs / Confluence / bất kỳ nền tảng nào và để chúng tự viết product doc, hoặc mở một PR.

---

## Dạy tác vụ (Teach a task) — ghi lại trực tiếp

Cô giành quyền điều khiển màn hình của Sherlock, bấm **Teach a task**, và quay lại thao tác của mình: vào Google → tìm "expedia technical blog post" → vào trang Medium của họ → quét tìm các bài liên quan đến AI → mở một bài → cuộn trang. Tạm dừng, xong. Sau đó là một đoạn voice note lồng lên bản ghi:

> Notice that I specifically looked for AI-related blog posts in my competitor products, and I wanted to make sure that we are staying on top of our AI tooling inside of our own product. I'm going to try and apply this to as many competitors as possible.
*(Hãy để ý là tôi chủ ý tìm các bài blog về AI ở các sản phẩm đối thủ, vì tôi muốn đảm bảo chúng ta luôn đón đầu về công cụ AI ngay trong sản phẩm của mình. Tôi sẽ cố gắng áp dụng quy trình này cho càng nhiều đối thủ càng tốt).*

Sau đó: *hãy làm tương tự với Southwest, Spirit, Skyscanner, Google Flights.* Các skill sẽ được dùng chung giữa tất cả các bot, bất kể bạn dạy chúng ở đâu. "Một trong những cách mang lại đòn bẩy lớn nhất khi dùng GrokBot."

---

## Bot tự sinh ra bot

> Based on the work that you and @Serena are doing, what bots would be helpful for you to continue doing great work, specifically around competitive differentiation and sales engineering? Go ahead and spin up those bots for me.
*(Dựa trên những việc bạn và @Serena đang làm, những bot nào sẽ hữu ích để các bạn tiếp tục làm tốt công việc, đặc biệt là xoay quanh việc tạo khác biệt cạnh tranh và sales engineering? Hãy tự khởi tạo các bot đó đi).*

Sherlock tạo ra ba bot, kèm theo bản mô tả:

- **Battle Card Blair** — "kết hợp kết quả trải nghiệm thực tế sản phẩm đối thủ từ Serena với codebase Flylo từ Sherlock để tạo ra các battle card ngắn gọn, sẵn sàng cho SE: tuyên bố của đối thủ vs. thực tế của Flylo."
- **Demo Drake** — "chuyên gia về demo và kịch bản thuyết trình (talk track) cho sales engineering. Xây dựng kịch bản demo chắc chắn, không bịa đặt (no-hallucination) và các bài nói trong cuộc gọi nhằm đối chiếu trực tiếp nỗi đau của khách hàng với luồng thực tế (live flow) của Flylo. Mọi luận điểm đều dựa trên cơ sở từ Sherlock."
- **AI Radar** — "theo dõi công cụ AI của đối thủ từ các tech blog công khai." Tái sử dụng skill cô vừa dạy. Luôn lấy Sherlock làm nguồn chân lý (source of truth); chuyển giao phần test và user flow cho Serena.

Đánh giá của cô: giữ lại Blair và Drake; AI Radar bị trùng lặp với Serena và "tên không có vần điệu dễ thương." Điều hữu ích không nằm ở ba con bot, mà là việc mỗi con bot **đều biết ai là nguồn chân lý cho từng mảng việc.**

Nếu bạn bắt đầu từ con số không: *Tôi cần chuẩn bị demo cho một đợt POC cùng với AE trong vài tuần tới cho khách hàng này. Hãy khởi tạo ba bot mà bạn nghĩ là sẽ hữu ích.* "GrokBot ở đây không phải để tạo thêm gánh nặng quản lý cho bạn."

---

## Những con số ấn tượng

- 15–20 customer deck mỗi tuần — khối lượng công việc của Mimi.
- 10–15 phút — thời gian tạo một slide case-study từ bài blog, tính cả bước tìm logo.
- $20–30 — chi phí cho toàn bộ một deck case-study bán hàng, so với 4–5 tiếng làm thủ công (số liệu cô chia sẻ trong phần Q&A).
- 10 phút — thời gian giữ chỗ trong khoang mà Sherlock tìm thấy trong code.

---

## Những câu Q&A đáng lưu lại

- **Các website có ban bot không?** CAPTCHA và hệ thống phát hiện bot có thể chặn ở một số trang; không có cách vượt qua đảm bảo 100%. Quy tắc ngón tay cái cho doanh nghiệp: "nếu bạn không nên truy cập Facebook, hãy chặn luôn trên máy tính của bot" thay vì cố tìm cách né tránh hệ thống phát hiện.
- **MCP vs. computer use.** MCP đóng vai trò như "API cho agent" — nhanh hơn, dễ whitelist/blacklist hơn. Computer use là giải pháp dự phòng khi không có MCP (ví dụ như Power BI, MongoDB). Một công cụ không có MCP thì cũng không phải là rào cản.
- **Token.** Điều chỉnh độ dài phản hồi (verbosity) của bot để kiểm soát chi phí; tài nguyên compute cho computer use thì người dùng không tự tinh chỉnh được.
- **Các công cụ non-Linux không có MCP thì sao?** Hiện tại không dùng được. Trả lời dứt khoát là "không."

---

## Áp dụng ngay công thức này

1. Một bot làm slide tích hợp sẵn template của bạn. Cung cấp tài liệu nguồn, đừng chỉ đưa ra chỉ dẫn.
2. Một bot chuyên gia kỹ thuật gắn vào các repo của bạn, được định hướng không bao giờ làm lộ IP và luôn trả lời bằng ngôn ngữ của khách hàng. Đưa cho bot đúng câu hỏi mà khách hàng vừa hỏi bạn.
3. Một bot nghiên cứu đối thủ biết trực tiếp sử dụng sản phẩm, thay vì chỉ đọc tài liệu về chúng. Cho phép nó trao đổi với bot chuyên gia để đối chiếu thông tin chuẩn.
4. Đưa cả hai bot vào group chat khi cần ý kiến thống nhất về roadmap.
5. Quay lại một workflow bạn thường làm thủ công. Nói to những điểm cốt lõi cần lưu ý.
6. Hỏi các bot xem chúng cần thêm những bot nào hỗ trợ.

Tài liệu liên quan: [`sales.md`](sales.vi.md), [`founders.md`](founders.vi.md) (StockBot là bot đối thủ ở góc độ founder).
