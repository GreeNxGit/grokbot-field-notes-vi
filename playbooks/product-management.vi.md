# Product Management

**Phiên:** GrokBot cho Product Manager — ngày 1  
**Người dẫn dắt:** Kevin De Parco và Roshan, product team tại xAI.

Khung tư duy bắt nguồn từ câu nói của DHH: *software is product management* (phần mềm chính là quản lý sản phẩm) — nó nên làm gì, phục vụ ai, làm thế nào, giao diện ra sao, đâu là các ưu tiên. Giờ đây, khi chi phí build phần mềm đã rất rẻ, mọi builder đều phải tự trả lời những câu hỏi đó. Phiên này chia sẻ về "cách chúng tôi build GrokBot bằng chính GrokBot" từ góc nhìn của tổ chức làm product.

Con số thống kê mở đầu ấn tượng: nội bộ xAI ghi nhận GrokBot đóng góp **tỷ lệ hai chữ số trong tổng số PR đã merged**, đồng thời hỗ trợ chính đội ngũ product tự ship PR lên production.

---

## Bốn đặc tính của một người đồng nghiệp (design brief)

Họ xây dựng sản phẩm xoay quanh những gì một đồng nghiệp giỏi thường làm, và đó cũng là cách họ kỳ vọng bạn sử dụng nó:

1. **Kết nối nhiều công cụ với nhau.** Linear/Jira, Salesforce, Notion, Figma — đồng nghiệp phối hợp xuyên suốt các công cụ này, chứ không chỉ gói gọn trong một nền tảng đơn lẻ.
2. **Context dài hạn (long-running context).** Họ học việc qua thực tế. Khởi đầu với ít context, được hướng dẫn xử lý task, tiếp nhận feedback và ngày càng tiến bộ.
3. **Tính độc lập.** Họ tự chủ hoàn thành task với các quyền truy cập riêng. Vì thế: mỗi bot đều sở hữu một máy tính riêng biệt.
4. **Giao tiếp qua messaging, không chờ đổi lượt (non-turn-taking).** Phản hồi liên tục, ngắt quãng linh hoạt (interrupt-driven), chia theo thread. Bạn có thể nhảy vào điều hướng ngay giữa chừng task.

Hai điểm mấu chốt cần ghi nhớ: giao **việc thật** cho bot và nhận lại **kết quả**, không phải những câu hỏi vụn vặt; bot sẽ **hoàn tất công việc** và chỉ quay lại khi thực sự cần phê duyệt.

---

## Đội ngũ bot

Công ty demo: **Flylo**, một hãng hàng không boutique giả định.

| Bot | Role | Ghi chú |
|---|---|---|
| **Cora** | Chief of staff | Có email, lịch làm việc, theo dõi thông tin trên Slack. Xây dựng mô hình phản ánh cách bạn làm việc thực tế. Dọn dẹp inbox và chỉ hiển thị những nội dung quan trọng. |
| **Emily** | Engineering manager | **Được huấn luyện không viết code.** Quản lý năm engineer bot. Tiếp nhận các khối lượng công việc lớn, phân rã công việc, phân quyền và chạy các vòng lặp kiểm chứng (verification loops) trên kết quả bàn giao. |
| Einstein, Igor, Nova, Larry, Eileen | IC engineer | Tự khởi chạy các cloud agent kèm một bản sao của repo mỗi khi cần chỉnh sửa code. |
| **Ashley** | Data science / analytics | Kết nối trực tiếp với data warehouse (Databricks, Snowflake — "tùy bạn lựa chọn"). Viết và thực thi SQL, trả về số liệu và biểu đồ. |
| **PMP** / "Pete" | Product assistant | Lập dự thảo PRD, tổng hợp customer insight. Trang bị skill viết PRD: phân loại rõ yêu cầu P0/P1/P2, tối ưu để chuyển thành code nhanh chóng thay vì tập trung lưu trữ tài liệu dài hạn. |
| **Pixel** | Designer | Được nạp tài liệu "cách trở thành một AI designer chuẩn S-tier", design system của công ty (trên Figma) cùng các file tham chiếu: font chữ, màu sắc, pattern, và danh sách các điều cấm kỵ được tích lũy theo thời gian ("không bao giờ đặt nút X ở góc trái"). |
| **Ray** | Recruiter | Sourcing ứng viên và quản lý pipeline tuyển dụng. Không demo trong phiên này. |

Các nhóm ở thanh sidebar: *Leadership* (ghim trên đầu), *Engineering team*, cùng các group chat **EngPod** (chạy standup cho dự án), **EPD** (bộ ba eng/product/design), **War room** (triage sự cố).

Các công cụ kết nối trong bản demo: Notion, Slack, Figma MCP, Gmail. Skill design chuẩn S-tier được lưu trữ song song.

---

## Ba use case dành cho PM được đề cập

1. **Danh sách tập trung (attention list).** Bạn đã dành sự chú ý vào những gì trong buổi sáng / tuần / tháng này, đối chiếu (diff) với danh sách ưu tiên của bạn. Cora thực hiện việc này bằng cách quan sát quy trình làm việc thực tế.
2. **Nghiên cứu và context khách hàng.** Đặt câu hỏi truy vấn dữ liệu on-demand thay vì phải tự viết SQL và đi tìm các bảng dữ liệu đáng tin cậy.
3. **Shipping.** "Bạn không thể là một PM vào năm 2026 nếu không tập trung vào việc bàn giao phần mềm thực tế tới tay người dùng."

---

## Workflow thực tế khi chạy trên stream

Toàn bộ chu trình vận hành khép kín từ một câu hỏi về dữ liệu cho đến khi một cloud agent mở PR.

**1. Đặt câu hỏi cho data bot.**

> Có bao nhiêu người đã mua vé hôm qua trên mobile so với web?

Ashley: 1.400 vé, khoảng 58% web / 42% mobile.

> Có bao nhiêu gia đình đi chuyến bay? Trực quan hóa dữ liệu này bằng biểu đồ giúp chúng tôi.

Ashley trả về biểu đồ phân loại theo khách đi lẻ / cặp đôi / gia đình / nhóm, và chỉ ra 25% hành khách là các gia đình. Ghi chú của Roshan: "Giờ đây tôi có thể truy vấn dữ liệu và nhận biểu đồ on-demand" — đây là tính năng thường nhật anh thích nhất. Thiết lập nó thành một routine:

> Gửi cập nhật này cho tôi vào mỗi sáng lúc 6:00.

Đối với các đợt launch lớn, họ đã yêu cầu bot xuất **báo cáo hàng giờ** trực tiếp từ data store.

**2. Phát hiện vấn đề trong funnel — và được bot đính chính.**

Khi quan sát purchase funnel trên mobile mà Ashley đã trích xuất trước đó, team nhận định: "sụt giảm lớn ở bước chọn chỗ ngồi (seat selection)." Họ phản hồi ngay trong thread:

> Có vẻ như tỷ lệ sụt giảm rất lớn khi người dùng chọn chỗ ngồi trên mobile. Phối hợp với @PMP để tạo một product spec nhằm tối ưu hóa mobile funnel này.

Ashley nhắn tin cho PMP — và **đính chính nhận định của con người**: điểm rò rỉ lớn nhất thực ra nằm ở bước *tìm kiếm → chọn mức giá vé (fare selection)*, chứ không phải bước chọn chỗ ngồi. Nhóm đã đọc nhầm biểu đồ. Bot đã kịp thời phát hiện ra điểm này trước khi spec được viết ra.

**3. PRD.** PMP lập một bản PRD trên Notion gồm các hạng mục P0 và P1: thiết kế lại kết quả giá vé trên mobile, rút ngắn thời gian so sánh, minh bạch giá vé hiển thị ngay trên card.

Họ thực hiện theo kiểu one-shot trong buổi demo, nhưng nói thẳng: quy trình thông thường vẫn cần con người review và lặp lại (iteration). Bạn có thể để lại comment trên Notion (hoặc Google Docs) và tag bot vào — bot hoàn toàn đọc được comment. Một comment dạng "con số này có vẻ không chuẩn" chính là một chỉ dẫn thực sự cho bot.

**4. Bàn giao song song cho design và engineering.**

> Chuyển PRD này cho Emily và yêu cầu engineering team làm prototype cho các ý tưởng này. Đồng thời nhờ Pixel thiết kế từng hạng mục P0.

Hai luồng bàn giao được kích hoạt cùng lúc. Pixel phản hồi với Phương án A và Phương án B. Cả phòng biểu quyết chọn A.

> Chuyển bản mock đầu tiên cho Emily để cập nhật prototype.

**5. Engineering manager phân rã công việc.** Emily chia nhỏ các ưu tiên thành từng phần việc có scope cụ thể cho từng engineer và **trao đổi trực tiếp với từng người**, bổ sung thêm context. Quan sát của họ: "agent viết prompt cực kỳ tốt — thường giỏi hơn cả chúng ta trong việc xác định cần cung cấp context nào cho một agent khác." Hãy giao mục tiêu cho cả team và để chúng tự điều phối context cùng cách phân rã task.

**6. Cloud agent.** Nova hỏi xem có nên tạo PR không; họ xác nhận có. Nova khởi chạy một cloud agent trên bản sao local của repo (đã bao gồm sẵn các script cài đặt để có thể chạy và test được) rồi tiến hành giám sát agent này.

**7. Vòng lặp kiểm chứng (verification loop).** IC engineer review kết quả đầu ra của cloud agent, sau đó chuyển cho QA hoặc Emily để chạy lớp kiểm tra thứ hai đối chiếu với mục tiêu ban đầu.

**Nên can thiệp (human in the loop) ở mức độ nào:** quyết định dựa trên mức độ rủi ro (stakes). Với thay đổi trên trang tài liệu (docs-site) hay chỉnh lại kích thước phần tử trong app — hãy để bot tự chạy. Với việc triển khai một thiết kế đúng theo spec — hãy can thiệp và yêu cầu xem demo hoặc prototype. Hai yếu tố hỗ trợ đắc lực: cung cấp cho agent môi trường để tự kiểm chứng output của chính nó, *đồng thời* trang bị công cụ giúp bạn dễ dàng kiểm chứng lại (ảnh chụp màn hình, bản ghi màn hình).

---

## Các mô hình team thay thế được gợi ý

Team tại Flylo chỉ là một ví dụ điển hình. Các mô hình khác họ từng quan sát thấy:

- Một team chuyên trách việc duy trì cập nhật **knowledge base về các requirement** và đưa dữ liệu đó vào sản phẩm — áp dụng cho các hệ thống phức tạp, nơi mỗi thay đổi nhỏ ở requirement đều kéo theo phản ứng dây chuyền.
- Một team chuyên **đồng bộ hóa nhiều repo (cross-repo sync)** trong một codebase phức tạp, đan xen.
- Một **builder bot** duy nhất đảm nhiệm tích hợp cả eng + product + design trong một. Cách này vẫn chạy tốt; họ chỉ tách riêng các role khi xử lý các trường hợp phức tạp.

---

## Những bài học kinh nghiệm xương máu (danh sách đúc kết)

1. **Agent định danh riêng với bộ nhớ tách biệt, học việc qua thực tế.** Chúng không hoàn hảo ngay ngày đầu tiên. Cần một giai đoạn onboarding để bạn trang bị skill, bổ sung context và hướng dẫn. Qua thời gian, chúng sẽ gánh vác được nhiều việc hơn.
2. **Giảm thiểu độ nhiễu (noise).** Khi thiết lập một routine, hãy dặn trước agent: *nếu đây là một no-op — không có gì quan trọng hoặc khẩn cấp — hãy tự xử lý hoặc đừng gửi thông báo cập nhật cho tôi.* Cora quản lý inbox và chỉ escalate khi thực sự cần thiết.
3. **Mô hình agent phân tầng đa cấp (agents all the way down).** Manager của các agent. Agent đứng ra điều phối các cloud agent. Đừng nghĩ rằng bản thân bạn phải tự tay gắn kết tất cả mọi thứ.

Ghi nhận thêm từ phần Q&A: mỗi bot sở hữu **bộ nhớ riêng (memory pool)**, kết hợp cùng một **bộ nhớ dùng chung (shared memory pool)** để các bot ghi vào mỗi khi có thông tin đáng để toàn team ghi nhớ. Việc phân tách theo role mang lại bộ nhớ chuyên sâu cho từng vai trò ("làm thế nào để trở thành một engineer thực sự giỏi theo thời gian"); các group chat sẽ kết nối chúng lại với nhau khi cần. Mục tiêu cuối cùng là bạn không còn phải tốn công sức suy nghĩ về việc "agent này đã biết những gì, mình có cần tóm tắt context lại không".

---

## Checklist để áp dụng ngay

1. Một data bot kết nối trực tiếp với data warehouse. Đặt câu hỏi bằng ngôn ngữ tự nhiên thông thường; chuyển các câu hỏi hữu ích thành morning routine.
2. Một spec bot với một kỹ năng chuyên biệt: phân loại P0/P1/P2 sắc bén, ngắn gọn, tối ưu để chuyển thành code nhanh chóng.
3. Một designer bot nạp sẵn design system của bạn cùng toàn bộ các điều cấm kỵ đã tích lũy.
4. Một EM bot không trực tiếp viết code. Nhiệm vụ là phân rã công việc, phân quyền và kiểm chứng.
5. Xâu chuỗi feedback theo thread: reply trực tiếp vào tin nhắn dữ liệu kèm yêu cầu mới, tag bot tiếp theo và để chúng tự trao đổi.
6. Hướng dẫn mọi routine cách xử lý khi gặp no-op: không làm gì cả.

Tài liệu liên quan: [`founders.md`](founders.vi.md) dành cho phiên bản ProdBot của founder và luồng feedback → PR; [`engineering.md`](engineering.vi.md) cho đầu còn lại của cùng pipeline này.
