# Founders

**Buổi chia sẻ:** GrokBot dành cho Founder — ngày 1  
**Người dẫn dắt:** Shub, chuyên gia hỗ trợ các founder thiết lập hệ thống GrokBot.

Góc nhìn cốt lõi: Với tư cách là founder, bạn có ba nhiệm vụ thực thi mang tính sống còn đối với công ty — **duy trì sự tập trung**, **giữ vững velocity** cho mọi thứ team ship ra, và **bảo vệ chất lượng insight** (các quyết định, kế hoạch). Hãy bàn giao lại tất cả những việc thuộc diện cần-thiết-nhưng-không-quan-trọng. "Việc giải phóng tải nhận thức (mental load) mà nó mang lại có lẽ còn quan trọng hơn bất cứ điều gì tôi sắp chia sẻ hôm nay."

Đường cong trưởng thành (maturity curve) mà anh ấy áp dụng: chatbot → ephemeral agent dùng một lần rồi bỏ → **bot tích lũy giá trị (compound)** (bạn giữ lại, đầu tư vào chúng, chúng ngày càng giỏi hơn như một nhân sự thực thụ) → một bộ phận chức năng tự động hóa hoàn toàn.

---

## Bốn use case dành cho founder

1. **Chốt khách hàng** — "làm những việc không thể scale," ngoại trừ việc bạn thực sự có thể scale chúng.
2. **Theo dõi thay đổi sản phẩm** — nắm bắt chính xác những gì đã ship và chưa ship khi các engineer ship tính năng nhanh hơn mức bạn có thể theo dõi. (Câu chuyện của chính anh: demo một tính năng vốn đã bị gỡ bỏ vào sáng hôm đó, đứng chỉ tay vào khoảng không).
3. **Thích ứng với đối thủ cạnh tranh** — bot có thể tự đăng ký và trải nghiệm sản phẩm của đối thủ dưới danh tính của bạn.
4. **Ship feedback thần tốc** — biến feedback thành PR chỉ trong vài giờ.

---

## Đội ngũ bot

| Bot | Nhiệm vụ | Tích lũy (Compound) dựa trên |
|---|---|---|
| **CloseBot** | Mọi thứ liên quan đến khách hàng: chuẩn bị trước cuộc gọi, đúc kết sau cuộc gọi, support, activation, hợp đồng, pipeline, lịch hẹn | Transcript của từng cuộc gọi |
| **ProdBot** | Những gì đang live: PR, issue trên Linear, tự duyệt sản phẩm end-to-end trên máy tính riêng | Các quyết định sản phẩm được chia sẻ với bot |
| **StockBot** | Đối thủ: tự đăng ký tài khoản, dùng thử sản phẩm, đọc changelog, bài đăng X, tin tuyển dụng | Mọi đối thủ mà bot tìm hiểu |
| **ProtoBot** | Làm prototype và chuyển feedback của khách hàng thành PR. Chạy tài khoản GrokBot riêng trên VM để trực tiếp tương tác với sản phẩm thực tế | Ngữ cảnh sản phẩm (product context) |
| **YapBot** | Giao tiếp theo đúng phong cách của bạn. Học từ email, Slack, iMessage; tự cập nhật hàng tuần | Độ lệch giữa bản nháp và nội dung thực tế đã gửi (draft-vs-sent delta) |
| **Misc bot** | Nơi tiếp nhận các yêu cầu linh tinh để không làm nhiễu context của các bot khác | — |

Được chia sẻ công khai sau buổi nói chuyện: StockBot (qua mã QR).

---

## CloseBot, chia làm bốn phần

**Phần 1 — Chuẩn bị trước cuộc gọi (Call prep).** Chạy định kỳ hàng ngày dựa trên lịch làm việc. Với mỗi cuộc họp:

- Kéo dữ liệu product telemetry của tài khoản đó: mức độ sử dụng đang tăng tốc (tìm tính năng phù hợp để thúc đẩy) hay giảm sút (tìm ra lý do họ phản đối)?
- Nghiên cứu chân dung khách hàng và sản phẩm của họ làm gì.
- **Duyệt qua website của họ và chụp màn hình** — và gần như lần nào cũng tìm ra một bug ("banner cookie đang đè lên nút submit"). Anh mở đầu cuộc gọi bằng chính chi tiết đó. Rất actionable và chứng minh rõ bạn đã chủ động tìm hiểu kỹ về họ.
- Đưa ra khuyến nghị và các rủi ro cần lưu ý trong cuộc gọi.

Kết quả trả về là một file HTML tóm tắt chuẩn bị cuộc gọi. "Giao diện không cần quá hoàn hảo và điều đó cũng chẳng sao — tôi đâu có gửi nó cho ai khác." Đọc nó 5 phút trước giờ hẹn. Bạn chỉ có 15–20 phút với khách hàng; hãy đi thẳng vào vấn đề mấu chốt.

**Phần 2 — Sau cuộc gọi.** Bot đọc transcript từ Granola và ghi nhận điểm nào tạo được ấn tượng, điểm nào không. Nếu bạn pitch về một tính năng ghi chú nhưng họ không quan tâm, tài liệu chuẩn bị lần tới sẽ không đặt nó lên đầu nữa. Sau hai đến ba tuần, bạn thậm chí có thể tận dụng dữ liệu từ các cuộc gọi của *những người khác*.

**Phần 3 — Support.** Các founder thường để việc này lại sau cùng và bị nó làm cho kiệt sức. Yếu tố cốt lõi giúp hệ thống vận hành hiệu quả: hãy kết nối bot với các phần tiềm ẩn rủi ro — dữ liệu của bạn, hệ thống **billing** — vì đó mới chính là nơi đội support thực sự gặp bế tắc.

**Phần 4 — Kích hoạt người dùng (Activation).** Bạn nắm rõ đâu là "khoảnh khắc wow" trong sản phẩm của mình. Khi sự kiện activation được kích hoạt (trong bản demo: một template được chia sẻ với đồng nghiệp), bot sẽ tự động gửi email tặng credit ("$1,000, một ví dụ khá điên rồ") để người dùng cảm thấy được tưởng thưởng ngay tại thời điểm đó. Không cần phải theo dõi thủ công từng hành trình người dùng, cũng không cần tự xây thêm công cụ.

Ngoài ra: xử lý phản hồi hợp đồng qua lại, tạo pipeline, quản lý lịch trình. "Tôi không còn phải bận tâm về 99% hành trình này nữa." Anh ấy chỉ chủ động giữ lại một phần việc vì bản thân vẫn thích trực tiếp trò chuyện với khách hàng.

---

## ProdBot

> Give me a daily rundown of Flylo. (Cho tôi bản tóm tắt tình hình hàng ngày của Flylo.)

Dành cho hãng hàng không trong bản demo. Bot đọc các PR và issue trên Linear, sau đó **đăng nhập vào sản phẩm và tự mình trải nghiệm** trên máy tính riêng của nó, map các thay đổi kỹ thuật với những gì nó nhìn thấy trên thực tế. Kết quả: những gì đã ship, những gì bị unship, và **những quyết định đáng để lưu tâm một cách có chủ đích** — các quyết định vi mô mà team đã ngầm đưa ra trong quá trình ship nhanh. Đính kèm: ảnh chụp màn hình và video ghi lại quá trình bot duyệt qua trang web ("cách nhanh nhất để xác minh xem một tính năng đã hoàn thiện tốt hay chưa"). Kết nối bot với hệ thống metrics, nó sẽ trực tiếp chỉ ra hiệu quả thực tế của những lần ship thay vì bắt bạn phải dán mắt vào dashboard.

---

## StockBot

> Run a competitor pulse on Notion and Craft. (Chạy khảo sát nhịp độ đối thủ cạnh tranh trên Notion và Craft.)

Với từng đối thủ: tìm thông tin về họ, đăng ký bằng email dùng một lần, hoàn tất quy trình onboarding, sử dụng sản phẩm ("nó sẽ viết nội dung trực tiếp trên Craft"), đọc changelog, bài đăng X, vị trí tuyển dụng ("một cách cực kỳ hiệu quả để bắt tín hiệu xem họ đang build cái gì"), và tìm hiểu quy mô team. Đầu ra: một bài bóc tách chi tiết (teardown) dạng HTML kèm theo video. Craft đang không tuyển người; Notion thì đang tuyển dụng ồ ạt (anh đã yêu cầu bot cắt ngắn bớt thông tin).

Các routine chạy định kỳ vài ngày một lần. **Nếu không có thông tin đáng giá, bot sẽ hoàn toàn giữ im lặng.** "Một trong những dạng bot chạy nền (ambient bot) thực sự rất quan trọng mà bạn luôn nên duy trì bên cạnh."

Phần nâng cao tùy chọn ("có thể hơi mang tính gián điệp doanh nghiệp, bạn cứ tùy ý áp dụng và hãy tự chịu trách nhiệm"): nếu từ CRM bạn biết một khách hàng đã churn sang đối thủ Y, hãy để bot gửi email hỏi họ lý do. Đừng cố thuyết phục họ quay lại ngay trong email đó. Chỉ cần lấy được tín hiệu thực tế.

---

## ProtoBot

> Pull the most recent customer feedback. (Lấy feedback khách hàng gần đây nhất.)

Bot lấy feedback về và bắt đầu xử lý ngay — trong bản demo, "nút chia sẻ template bot chưa đủ bắt mắt, khách hàng muốn nó phải nổi bật hơn" → một cloud agent lập tức được kích hoạt. Tầm nhìn: mọi kênh tiếp nhận feedback (X, hòm thư support, email) đều được đổ về một bot duy nhất chịu trách nhiệm mở PR. "Bạn sẽ thấy toàn bộ vòng lặp từ feedback → ship → deploy diễn ra chỉ trong vài giờ." Nhiệm vụ của bạn thu về đúng khâu ra quyết định — chọn xem nên tiếp thu feedback nào. "Nút thắt cổ chai lớn nhất nên là năng lực ra quyết định của chính bạn."

Vì ProtoBot có tài khoản GrokBot riêng trên VM, nó có thể trực tiếp điều khiển sản phẩm thực tế để QA những gì nó vừa ship. Tự xác minh mà không cần rời khỏi vòng lặp phát triển.

---

## YapBot

Một mục đích duy nhất: giao tiếp bằng đúng giọng điệu của anh ấy. Bot đọc toàn bộ những gì anh từng gửi và học phong cách hành văn của anh — cập nhật **hàng tuần**, không phải làm một lần là xong. Các bot khác chủ động kéo nó vào khi cần soạn nội dung dưới danh nghĩa của anh ("sau vài lần tôi khuyến khích, chúng tự biết cách phối hợp với các bot khác khi cần thiết"). Các email nhạy cảm vẫn được giữ ở dạng bản nháp; bot sẽ học từ sự khác biệt giữa bản nháp nó tạo ra và nội dung thực tế anh ấy gửi đi. "Tầm nhìn cuối cùng là bạn thậm chí không cần phải bận tâm suy nghĩ về việc đó nữa."

---

## Những bài học rút ra (từ slide của anh)

- **Hãy để bot của bạn tự do vận hành.** Cung cấp cho chúng quyền truy cập tối đa trong mức bạn cảm thấy an toàn. Đó là cách duy nhất để chúng xử lý công việc end-to-end.
- **Đừng vứt bỏ chúng sau khi điều chỉnh hướng đi.** Thói quen phản xạ từ các ephemeral agent là "context bị loãng/nhiễu rồi, reset làm lại từ đầu." Nhưng ở đây, context mà bạn tưởng là "lãng phí" thực chất chính là khoản đầu tư tích lũy. Hãy tin tưởng chúng như những nhân viên thực thụ.
- **Sự chủ đích sẽ mang lại quả ngọt.** Một prompt kiểu "hãy làm cái này xịn hơn đi" chắc chắn sẽ khiến bot bị kẹt. Càng đầu tư nhiều chủ đích và sự rõ ràng vào nhiệm vụ trong lần đầu tiên, bạn càng có thể lặp lại nó mượt mà về sau mà không gặp trục trặc.
- **Dành ra một đến hai tiếng để rà soát lại một ngày làm việc của bạn** và xác định những gì có thể ủy quyền. "Tôi biết nghe có vẻ ngớ ngẩn." Nhưng dù sao thì hãy cứ làm đi.

---

## Mẹo cho power-user (phần tối ưu token)

- **Duyệt web bằng browser rất tốn kém.** Hãy quan sát bot thực hiện tác vụ một lần trên browser, yêu cầu nó kiểm tra các network request vừa thực hiện, rồi từ đó về sau chỉ định nó gọi trực tiếp vào các API đó. Nhanh hơn và tiết kiệm chi phí hơn rất nhiều. Nguyên tắc chung: hãy hỏi chính bot cách tối ưu; nó sẽ tự tìm ra giải pháp.
- **Audit lại các routine của bạn.** Mọi người thường cho routine chạy 15 phút một lần "vì nó quan trọng" — con số đó lên tới gần 100 lượt chạy mỗi ngày. Hãy dùng một bot để audit tần suất của các routine. Ưu tiên webhook và tín hiệu inbound thay vì chạy theo lịch định kỳ một cách mù quáng.
- **Tạo một bot giọng nói (voice bot).** Đây là thứ bị đánh giá thấp nhất.
- **Import cookie của bạn** để bot luôn duy trì trạng thái đăng nhập, tránh việc bạn phải can thiệp giành quyền kiểm soát máy tính của nó.
- **Gom nhóm bot theo chuyên môn, không phải theo tác vụ.** Quá trình tích lũy giá trị (compounding) diễn ra bên trong từng domain theo những cách bạn không thể lường trước, vì vậy mọi thứ liên quan đến khách hàng nên được giao toàn bộ cho bot khách hàng.
- **Xây dựng một bot có nhiệm vụ duy nhất là tối ưu các bot khác** — thiết kế routine tốt hơn, phát hiện điểm nào bạn phải nhắc lệnh lần hai và lý do tại sao. Chỉ cần thiết lập một lần.
- **Yêu cầu bot chủ động quên bớt thông tin.** "Hãy quên toàn bộ quá trình chúng ta tạo ảnh đại diện đi. Đừng bao giờ bận tâm về chuyện đó nữa." Thao tác này giúp dọn sạch bộ nhớ và cải thiện hiệu quả sử dụng token.

---

## Q&A đáng lưu lại

- **Làm thế nào để thiết lập bot một cách có chủ đích?** Liệt kê toàn bộ từng đầu việc bạn phải làm, sau đó phân nhóm lại. Giao cho mỗi bot một domain mà nó đóng vai trò là chuyên gia; để nó tự mở rộng khi bạn bắt đầu hỏi những thứ nằm ngoài phạm vi đó. Việc có thêm một bot đóng vai trò Chief of Staff ở tầng trên cùng hay không là tùy thuộc vào sở thích cá nhân — anh ấy không dùng ("tôi thích cảm giác kiểm soát chi tiết và sâu sát mọi thứ"). Nếu bạn muốn công việc được trừu tượng hóa hoàn toàn, hãy sử dụng một bot như vậy.
- **Quyết định mang tính tất định (deterministic) cho enterprise?** Bản thân các model không mang tính tất định. Hãy để một cloud agent viết code kèm cây quyết định (decision tree), sau đó dặn bot: mỗi khi cần đưa ra quyết định này, hãy nhìn vào lưu đồ và thực thi nó. Code ở đây đóng vai trò "mô phỏng tính tất định."
- **Group chat giữa các bot.** Điểm cộng: các bot nhiệt tình phối hợp với nhau để xử lý tác vụ phức tạp. Điểm trừ: chúng đều rất thích nói, chen ngang lời nhau và ngốn chi phí rất nhanh. Thông thường, bạn chỉ nên để một bot tag hai bot khác đúng một lần, thay vì ném tất cả vào chung một phòng chat.
- **Chất lượng trên Marketplace.** Được đội ngũ trực tiếp audit thủ công, đồng thời được review bởi các bot chuyên dùng để đánh giá bot. Để tự mình đánh giá: hãy hỏi nó "bạn làm được gì và làm bằng cách nào" trước khi cho chạy toàn bộ flow.
- **Các bot có chia sẻ bộ nhớ (memory) với nhau không?** Không. Chúng dùng chung một **hệ thống file (file system)** (một VM, mỗi bot một instance riêng biệt, tương tự như các màn hình desktop), vì vậy chúng có thể đọc file của nhau và sẽ chủ động làm điều đó. Context window hoàn toàn tách biệt.
- **Cloud agent so với GrokBot?** Bất cứ việc gì phức tạp bạn muốn ship ra mà cần quyền kiểm soát sâu đối với model: hãy dùng cloud agent. GrokBot chỉ đóng vai trò truyền đúng context liên quan sang và có thể QA lại kết quả đầu ra. "Dùng thử vài lần bạn sẽ tự hình thành trực giác."
- **Nhắn tin trực tiếp bot-to-bot xuyên tài khoản?** Tính năng này hiện chưa có.
- **Chạy thực thi trên máy local?** Settings → local execution. Hoạt động tốt. Tuy nhiên họ thiên về phương án chạy trên cloud hơn vì các ứng dụng chạy local thường chiếm focus màn hình và ngốn tài nguyên máy của bạn.

---

## Bản tóm tắt để áp dụng ngay

1. Mỗi bot đảm nhiệm một nhóm trách nhiệm của founder: khách hàng, trạng thái sản phẩm, đối thủ, feedback-to-PR, giọng điệu cá nhân, việc linh tinh.
2. Chuẩn bị trước cuộc gọi bằng routine tự động theo lịch hẹn; bao gồm cả bước duyệt qua trang web của *chính khách hàng*.
3. Rút ra bài học sau cuộc gọi từ transcript cho từng cuộc họp.
4. Quét nhịp độ đối thủ vài ngày một lần, giữ im lặng nếu không có thông tin thực sự đáng giá.
5. Gom các kênh feedback → một bot duy nhất → mở các PR. Bạn là người quyết định tính năng nào được ship.
6. Tự động học lại phong cách hành văn hàng tuần dựa trên nội dung bạn thực tế đã gửi.
7. Audit lại tần suất chạy routine; chuyển dần các schedule cố định sang cơ chế webhook.

Tài liệu liên quan: [`product-management.vi.md`](product-management.vi.md), [`sales-engineering.vi.md`](sales-engineering.vi.md) dành cho phiên bản SE của bot phân tích đối thủ cạnh tranh.
