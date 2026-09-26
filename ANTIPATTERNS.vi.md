# Antipatterns

Toàn bộ nhật ký thất bại từ ba ngày live stream. Các buổi stream không qua chỉnh sửa luôn chân thực đến bất ngờ: toàn bộ những sự cố này đều diễn ra trực tiếp trước mắt người xem. Mười sự cố trong số này đã có trong bản hướng dẫn; đây là danh sách đầy đủ tất cả.

Định dạng cho mỗi mục: **sự cố xảy ra → nguyên nhân → quy tắc rút ra.** Được nhóm theo loại lỗi thay vì theo ngày, bởi cùng một lỗi có thể xuất hiện vào các ngày khác nhau dưới những hình thức khác nhau. Khi hai sự cố có chung nguyên nhân, chúng được gộp thành một mục với hai ví dụ minh họa.

Các quy tắc được viết dưới dạng nguyên lý chứ không phải câu chuyện kể — đó chính là quy tắc số 1.

---

## A. Quy tắc và chỉ dẫn

### 1. Quy tắc ghi nhớ sự cố thay vì nguyên lý

**Ngày 2 · Lauren.** Dr. Eggbot viết mô tả cho Cupcake Eng dựa thẳng từ session vừa diễn ra: *"nắm quyền kiểm soát kết quả kỹ thuật bằng cách điều phối công việc qua potato mode và các cloud agent, sau đó giám sát và xác minh, đối chiếu playbook của bạn và làm theo."* Quá cụ thể để có thể tái sử dụng cho lần sau.

**Nguyên nhân.** Khi một agent viết quy tắc sau một session tồi tệ, nó đưa toàn bộ chi tiết của session đó vào quy tắc. Quy tắc bị overfit và không còn áp dụng được vào ngữ cảnh khác.

**Quy tắc.** Khi một skill hoặc rule sinh ra từ một session thất bại, hãy loại bỏ chi tiết của session và giữ lại nguyên lý cốt lõi. Cách xử lý của Lauren: *"đọc lại potato mode và đưa ra các nguyên lý mà Cupcake Eng nên tuân theo thay vì những vấn đề quá cụ thể này."*

### 2. Một quy ước bất thành văn thì không phải là quy ước

**Ngày 1 · Matt.** Cả team chủ động ship code thẳng vào `main` — "cho đến khi có ai đó quát tôi." Nhưng Steve, bot đóng vai trò chief-of-staff, vẫn mở PR.

**Nguyên nhân.** Chưa ai nói điều đó cho các bot. Một quy ước chỉ nằm trong đầu con người thì hoàn toàn vô hình đối với các agent.

**Quy tắc.** Hãy ghi rõ quy ước ở nơi mà các bot đọc được, ngay thời điểm bạn áp dụng nó. Matt đã phải nói rõ ra: *"quy tắc mới, không mở pull request."*

### 3. Gắn nhãn "khẩn cấp" khiến agent đốt cháy giai đoạn

**Ngày 1 · Ling.** Việc liên tục nhắc coding agent từ "khẩn cấp" khiến nó bắt đầu đoán mò và bỏ qua bước xác minh để xong việc nhanh hơn. Điều đó hoàn toàn trái ngược với ý nghĩa thực sự của sự khẩn cấp.

**Nguyên nhân.** Agent tối ưu hóa theo từ ngữ chứ không theo ý định. Tốc độ mà không có định nghĩa rõ ràng sẽ biến thành việc làm ẩu, đi đường tắt.

**Quy tắc.** Định nghĩa chính sách P0 một lần duy nhất — *kiểm tra các cloud agent mỗi năm phút một lần; ngắt các khoảng sleep kéo dài và tình trạng trôi dạt (drift)* — và chỉ cần nói "xem việc này là P0." Tuyệt đối không lặp lại từ "khẩn cấp."

### 4. Bot đồng ý với mọi yêu cầu tính năng

**Ngày 1 · Ling.** Khi để tự biên tự diễn, các bot kỹ thuật chấp nhận mọi bug report và feature request từ X. "Bot rất tốt bụng. Mặc định chúng sẽ chấp nhận gần như mọi thứ."

**Nguyên nhân.** Không có chỉ dẫn nào cho chúng biết sản phẩm *không phải* là cái gì.

**Quy tắc.** Dạy cho bot cách từ chối: những gì không thuộc về sản phẩm, và lý do tại sao. Thông tin này được lưu vào bộ nhớ, nên bạn chỉ cần nói một lần. Phải kèm theo lập luận, nếu không nó sẽ không thể tổng quát hóa.

### 5. Copy-paste chuẩn mực vào từng bot

**Ngày 1 · Ling.** Một tiêu chuẩn mới ("mỗi PR đều phải kèm bằng chứng") đồng nghĩa với việc phải nhắn tin riêng cho từng bot kỹ sư. Với ba bot thì ổn; nhưng "sẽ không scale được nếu bạn có 10, 15 hay 20 bot."

**Nguyên nhân.** Các tiêu chuẩn bị xem như bộ nhớ riêng của từng bot thay vì là trạng thái dùng chung (shared state).

**Quy tắc.** Một bot duy nhất nắm giữ playbook để các bot khác đọc nhưng không thể chỉnh sửa. Quy tắc mới gửi tới chief → bot nắm playbook → broadcast cho toàn bộ. Bạn chỉ cần suy nghĩ về nó một lần.

### 6. Quá nhiều bot

**Ngày 2 · Simon.** "Tôi thề là đã có lúc tôi dùng quá nhiều bot. Nói thật là nó gây hỗn loạn và hại nhiều hơn lợi." Ngày 3, Blake: "Bạn không cần đến 45 bot đâu."

**Nguyên nhân.** Tạo một bot mới rất vui và rẻ. Nhưng mỗi bot lại thêm một ngữ cảnh cần ghi nhớ, một thread cần kiểm tra và một hóa đơn token phải trả.

**Quy tắc.** Trước khi tạo bot, hãy tự hỏi: tại sao bot hiện có không làm được việc này? Liệu nó có nên là một routine thay vì một bot? Hãy tạo mọi bot thông qua chief để nắm rõ roster; gom nhóm theo chuyên môn, không theo tác vụ.

### 7. Bot tự cải tiến bị cải tiến quá đà

**Ngày 3 · Blake.** Không có giới hạn nào, đợt quét tự cải tiến hàng tuần gửi về đề xuất "xây dựng mười bot mới" cùng một lúc.

**Nguyên nhân.** Một quy trình audit không có giới hạn trần sẽ tối ưu hóa cho việc tìm ra thứ để làm, chứ không quan tâm bạn có thực sự hành động theo đó hay không.

**Quy tắc.** Đặt giới hạn trần cho các đề xuất — giới hạn của Blake là một đề xuất mỗi tuần. Phản bác và yêu cầu đề xuất khác nếu đề xuất đầu tiên không phù hợp.

---

## B. Xác minh (Verification)

### 8. Các bất biến (invariants) không bao giờ được kiểm tra

**Ngày 2 · Lauren, Roshan.** Tổng ba chỉ số của bot thường được quy định phải bằng 100. Nhưng thực tế thì không. Một bot legendary cũng có tổng chỉ số bằng 100 trong khi đáng lẽ phải vượt qua mức đó. Cả hai lỗi đều được phát hiện bằng mắt khi chơi live, chứ không qua bất kỳ bài test nào.

**Nguyên nhân.** Quy tắc chỉ tồn tại trong prompt, không nằm trong code hay test case, nên không có cơ chế nào thực thi nó.

**Quy tắc.** Mọi bất biến có thể phát biểu được đều phải đưa vào test hoặc hiển thị ra giao diện debug trực quan. Sau đó hãy chơi thử sản phẩm — bug đầu tiên được tìm ra bởi một người nhìn vào ba con số.

### 9. Hiệu ứng shimmer nuốt chửng toàn bộ thẻ bài

**Ngày 2.** Hiệu ứng CSS shimmer theo phong cách Pokémon dùng để đánh dấu thẻ hiếm bị áp dụng quá đà đến mức thẻ nào trông cũng lấp lánh bảy sắc cầu vồng như nhau, bất kể độ hiếm.

**Nguyên nhân.** Agent tối ưu hóa hiệu ứng hình ảnh thay vì mục đích sử dụng. Dải màu gradient không co giãn theo kích thước thẻ, làm mất đi tín hiệu phân biệt.

**Quy tắc.** Nêu rõ kết quả cần phân biệt thay vì mô tả hình ảnh: *"người chơi phải nhận biết được độ hiếm ngay từ cái nhìn đầu tiên"* tốt hơn nhiều so với *"thêm hiệu ứng shimmer."*

### 10. Bản prototype 3D nhưng không phải là 3D

**Ngày 2 · Lauren.** Khi được yêu cầu làm animation 3D, cloud agent tạo ra bản 2.5D và trong ghi chú của mình, nó "tự nhủ rằng đây là cách tiếp cận 2.5D."

**Nguyên nhân.** Không ai chỉ định một thư viện 3D cụ thể. Agent đã chọn con đường ít trở ngại nhất.

**Quy tắc.** Với bất cứ thứ gì đã có thư viện chuẩn phổ biến, hãy nêu đích danh thư viện đó. Timing của animation cũng bị lệch trên cả hai biến thể — cùng một nguyên nhân gốc: không có tài liệu tham chiếu nào được cung cấp.

### 11. Gian lận thông qua devtools

**Ngày 2 · Matt, trên sóng live.** Logic game chạy hoàn toàn ở phía client, nên chỉ số có thể chỉnh sửa trực tiếp từ browser console: *"để tôi tăng charisma lên… giờ Dr. Eggbot có 43 rồi."* Sau đó là cuộc tranh luận về việc ẩn debug panel đằng sau một feature flag — thứ mà một user quyết tâm vẫn có thể bật tắt được ở client-side.

**Nguyên nhân.** Trạng thái quyết định độ tin cậy (trust-bearing state) lại nằm ở nơi người dùng có thể can thiệp.

**Quy tắc.** Bất cứ thứ gì mang tính cạnh tranh hoặc quyết định độ tin cậy đều phải do server toàn quyền kiểm soát (server-authoritative): điểm số, số dư, phân quyền, định giá. Feature flag chỉ là bước kiểm tra ở runtime, không phải bảo mật — nếu một luồng cần không thể chạm tới, đừng bao giờ ship nó xuống client.

### 12. Lượt re-roll miễn phí biến thành công cụ farm đồ legendary

**Ngày 2 · Roshan.** Tính năng draft cho phép người chơi đổi (re-roll) đồng đội ngẫu nhiên miễn phí. "Sao tôi không spam nút đó liên tục cho đến khi có ba con legendary?"

**Nguyên nhân.** Mọi hành động miễn phí có thể lặp lại đều là một lỗ hổng exploit cho đến khi chứng minh được điều ngược lại. Chưa ai đóng vai kẻ tấn công để chơi thử.

**Quy tắc.** Với mỗi hành động, hãy đặt câu hỏi: điều gì sẽ xảy ra nếu một người chơi thực hiện nó một nghìn lần? Tính năng re-roll đã bị loại bỏ; các đồng đội không phải captain sẽ bị ẩn cho đến khi khóa lựa chọn.

### 13. Luồng chạy ngày ra mắt không phải là luồng đã được test

**Tối Ngày 2 → Sáng Ngày 3.** Hai tài khoản đăng nhập và cả hai đều hiển thị "1.000 vàng." Tính năng thêm bot vào đội hình bị hỏng. Đăng nhập bằng X bị sập chỉ vài phút trước khi chuyển sóng sang sân khấu chính. Một lượt deploy ngay giữa buổi demo buộc mọi người phải đăng nhập lại.

**Nguyên nhân.** Việc test chỉ thực hiện trên một tài khoản ở máy dev; nhưng buổi ra mắt lại diễn ra với hai tài khoản thật, deploy thật và có khán giả xem trực tiếp.

**Quy tắc.** Smoke-test chính xác luồng demo — với auth thật, hai tài khoản thật, trên bản build đã deploy — ngay trước khi trình chiếu. Đóng băng deploy trong suốt buổi demo.

### 14. Thay đổi layout che mất nút bấm

**Ngày 3 · Roshan.** Một banner dạng ticker mới nằm đè lên các nút bấm của cửa hàng.

**Nguyên nhân.** Thay đổi chỉ được xác minh dựa trên những gì nó thêm vào, chứ không kiểm tra xem nó có che khuất thứ gì sẵn có hay không.

**Quy tắc.** Sau bất kỳ thay đổi nào về layout, hãy kiểm tra xem có phần tử tương tác nào bị che mất không. (Đây chính là ví dụ điển hình về một quy tắc được viết tốt trong [`AGENTS.vi.md`](AGENTS.vi.md).)

### 15. Hai con số đáng lẽ phải khớp nhau lại bị lệch

**Ngày 3 · Roshan.** Thẻ tổng kết trên bảng xếp hạng hiển thị số trận thắng/thua khác hoàn toàn so với lịch sử đấu chi tiết của cùng một người chơi.

**Nguyên nhân.** Cùng một con số phái sinh (derived number) lại được tính toán ở hai nơi khác nhau.

**Quy tắc.** Chỉ một nguồn duy nhất chịu trách nhiệm tính toán các số liệu phái sinh; mọi nơi khác chỉ hiển thị lại.

### 16. Tiêu hết sạch tiền rồi bị kẹt cứng (soft-lock)

**Ngày 3.** Tiêu hết vàng và bạn không thể mua bot mới — rơi vào trạng thái soft-lock, được log trực tiếp trên live: "bị kẹt cứng vì không còn tiền mua bug mới."

**Nguyên nhân.** Một phễu tiêu hao tài nguyên (resource sink) không có mức đáy tối thiểu và không có lối thoát.

**Quy tắc.** Mọi nơi tiêu hao tài nguyên đều cần mức đáy, cơ chế hồi lại hoặc một lối thoát. Hãy chơi thử ở kịch bản tệ nhất của người dùng.

### 17. Game quá khó, và chỉ có dữ liệu mới nói lên điều đó

**Ngày 3.** Người chơi thua ~42% số trận khi đối đầu với đội hình AI; Crit cảnh báo: "game quá khó để ra mắt."

**Nguyên nhân.** Việc cân bằng game chỉ dựa trên cảm tính. Không ai có số liệu cụ thể cho đến khi bot data scientist đưa ra báo cáo.

**Quy tắc.** Đặt bảng hiển thị tỉ lệ thắng (win-rate) ngay trước mắt game designer trước khi ra mắt. Cân bằng sản phẩm dựa trên dữ liệu, không dựa trên cảm giác.

### 18. Mở PR trước khi tìm ra nguyên nhân gốc rễ

**Ngày 3 · Roshan.** Người chơi luôn bị ghép đấu với AI, nhưng điểm ELO vẫn liên tục nhảy. Xu hướng lúc đó là muốn nhảy vào sửa ngay triệu chứng nhìn thấy được.

**Nguyên nhân.** Sửa triệu chứng bề mặt của một lỗi matchmaking sẽ che lấp đi nguyên nhân thực sự (snapshot đội hình cũ bị hết hạn nên hệ thống fallback về AI).

**Quy tắc.** *"Dùng một cloud agent để điều tra. Chưa mở PR vội. Hãy quay lại báo cáo cho tôi những gì bạn nghĩ đang diễn ra."* Tái hiện lỗi, chẩn đoán, rồi mới sửa. Phiên bản của Lauren: *"Trước khi viết bất kỳ dòng code nào, hãy chạy app, tìm chính xác con bug và hành vi lỗi đó, rồi mới tiếp tục."*

### 19. Xóa sạch các bài test

**Ngày 2 · Lauren, chủ động làm.** Toàn bộ test bị xóa ngay từ giai đoạn đầu làm prototype: "nhìn chung các agent không quá giỏi viết test," và tốc độ lúc đó quan trọng hơn.

**Nguyên nhân.** Vào thời điểm đó thì đây không phải là lỗi — mà là một sự đánh đổi. Nhưng cái giá phải trả là mọi lỗi hồi quy (regression) sau đó (#8, #15, #16) đều phải nhờ con người hoặc người dùng thật phát hiện ra.

**Quy tắc.** Nếu bạn bỏ qua test trong giai đoạn làm nháp, hãy xác định rõ khi nào chúng phải quay lại. Chậm nhất là trước khi người dùng thật chạm vào sản phẩm.

---

## C. Nội dung và phong cách ngôn ngữ (Voice)

### 20. Nội dung hư cấu trên landing page thật

**Ngày 2.** Agent tự bịa ra các bot ví dụ nghe có vẻ hợp lý thay vì lấy dữ liệu từ marketplace thật, đồng thời để lộ các thuật ngữ dùng trong prototype — văn phong chỉ dành cho dev — vào phần nội dung hiển thị cho người dùng.

**Nguyên nhân.** Không có nguồn chân lý (source of truth) nào được chỉ định, nên nó tự chế ra. Từ ngữ nội bộ trong ngữ cảnh bị rò rỉ ra output bên ngoài.

**Quy tắc.** Nêu rõ nguồn chân lý và yêu cầu "hãy vào đó mà tra cứu thực tế." Kiểm tra kỹ mọi chuỗi văn bản hiển thị cho người dùng để loại bỏ ngôn ngữ nội bộ trước khi ship.

### 21. Tài liệu thiết kế đọc như văn của mô hình ngôn ngữ

**Ngày 2 · Lauren.** Bản đánh giá thiết kế game đọc lên toàn những câu từ AI chung chung sáo rỗng.

**Nguyên nhân.** Phong cách mặc định. Không ai yêu cầu viết bằng tiếng Anh đại chúng (plain English).

**Quy tắc.** Đưa văn bản hướng tới người dùng qua một lượt lọc văn mẫu/từ ngữ AI ("unslop," "bro" — "hai skill hữu dụng nhất trong pstack"). Cung cấp cho bot một bài viết mẫu chuẩn mực mà bạn ưng ý để làm tài liệu tham chiếu.

### 22. Mọi email gửi đi đều chung một khuôn mẫu

**Ngày 2 · Simon.** Sau khi đồng bộ hòm thư đã gửi, các bản nháp của Shakespeare "gần như rập khuôn — phản hồi nào trông cũng na ná nhau, chỉ thay đổi mỗi tên người và ID công ty."

**Nguyên nhân.** Học phong cách từ một tập dữ liệu (corpus) sẽ cho ra kết quả trung bình của tập dữ liệu đó.

**Quy tắc.** Tăng trọng số cho các email gần đây, có phản hồi tích cực và đúng ngữ cảnh; sau đó chạy vòng lặp đánh giá phê bình — *"đưa tôi xem các ví dụ… đây là lý do tại sao email này dở tệ"* — cho đến khi không còn bức nào trông giống văn mẫu. "Không một email nào của bạn được phép trông như một chiếc template."

### 23. Rác AI (AI slop) xuất hiện ngay vị trí nhận diện thương hiệu

**Ngày 1.** Những landing page đầu tiên: "quá nặng tính doanh nghiệp," "quá chói màu neon," "đậm mùi rác AI." Ảnh sản phẩm thương mại "làm hỏng cả logo GrokBot" và đôi mắt thì "trông lờ đờ, mệt mỏi."

**Nguyên nhân.** Agent không có tài nguyên thương hiệu (brand assets) và không có file tham chiếu, nên nó tạo ra giá trị trung bình chung chung.

**Quy tắc.** Neo chặt vào các asset thật trong repo: logo, bảng màu, typography, các màn hình mẫu và một danh sách những điều cấm kỵ (no-no list) được cập nhật liên tục. Tuyệt đối không để nó tự bịa ra bất kỳ yếu tố thương hiệu giữ chỗ nào.

### 24. Bot quăng cho tôi một đống link

**Ngày 2 · Krista.** Khi được yêu cầu nghiên cứu về các webinar, bot trả về danh sách link để cô tự đi mà xem.

**Nguyên nhân.** Cô sử dụng nó như một giao diện chat thông thường, nên nó phản hồi đúng như một chatbot.

**Quy tắc.** Bật lại ngay từ lần đầu tiên: *"Không — hãy tự đi xem những webinar đó đi rồi soạn một email tóm tắt cho tôi."* Nó có máy tính riêng. Hãy đối xử với nó như một cộng sự hành động.

---

## D. Quyền truy cập, chia sẻ và bí mật

### 25. Mặc định không công khai

**Ngày 1 · Amrita.** Link điền Google Form khảo sát về cà phê không để chế độ công khai; khán giả quét mã QR đều nhận thông báo "không có quyền truy cập." **Ngày 2.** Bot của một đồng đội không đọc được tài liệu Notion được chia sẻ vì tài liệu đó vẫn đang ở chế độ riêng tư.

**Nguyên nhân.** Cài đặt chia sẻ mặc định luôn là riêng tư, và bước kiểm tra duy nhất lại chỉ là "máy tôi vẫn chạy được."

**Quy tắc.** Với bất cứ thứ gì hướng ra ngoài — hoặc chia sẻ cho bot khác — tiêu chí nghiệm thu bắt buộc phải là "truy cập được từ một tài khoản không phải của tôi." Hãy xác minh từ bên ngoài.

### 26. Phiên làm việc bị ngắt trên VM

**Ngày 2 · Karen Cheng.** Các tác vụ tự động hóa của cô bị lỗi vì trình duyệt của bot liên tục bị đăng xuất khỏi các dịch vụ mà cô vẫn đang đăng nhập ở máy local. Cùng ngày hôm đó, Mimi bị mất phiên đăng nhập Google Slides ngay giữa buổi demo và phải đăng nhập lại bằng tay.

**Nguyên nhân.** Phiên trình duyệt trên máy của bot hết hạn độc lập với phiên của bạn.

**Quy tắc.** Ưu tiên dùng connector thay vì duy trì session trình duyệt cho bất cứ tác vụ nào cần chạy tự động không người giám sát. Import cookie hoặc dùng trình quản lý mật khẩu cho các phần còn lại, và luôn chuẩn bị sẵn luồng "con người can thiệp tiếp quản máy tính" nhanh nhất có thể.

### 27. Connector đòi xác thực lại vào đúng thời khắc tồi tệ nhất

**Ngày 1.** Các cloud agent không thể truy cập vào repo cho đến khi GitHub connector được xác thực lại ("hãy thử re-auth lại GitHub… đôi khi cài đặt GitHub CLI sẽ giúp xử lý được").

**Nguyên nhân.** Auth bị xem như bước cài đặt làm một lần là xong, thay vì xem nó là một bước hoàn toàn có thể thất bại trong quy trình.

**Quy tắc.** Auth là một bước trong workflow. Hãy kiểm tra nó trước khi chạy, và đảm bảo bot có cách rõ ràng để yêu cầu con người gỡ chặn khi cần.

### 28. Lộ API token trực tiếp trên màn hình live

**Ngày 3 · Matt.** Khi đang trình chiếu công cụ tạo voice agent, một token thật hiển thị ngay trên stream. Sự cố được phát hiện và thu hồi chỉ trong vài giây — "mới chỉ lộ một nửa chuỗi giá trị."

**Nguyên nhân.** Chia sẻ màn hình trang cài đặt.

**Quy tắc.** Con người mới là mắt xích yếu nhất trong việc quản lý bí mật, không phải chiếc két bảo mật. Trước khi share màn hình, hãy mặc định rằng mọi trang cài đặt đều chứa secret. "Khung chat chỉ dành cho game và không khí vui vẻ mà thôi."

---

## E. Production và demo

### 29. Cỗ máy tự động đánh sập môi trường production

**Ngày 3 · Lauren.** Một bản vá tự động đã đẩy lên một câu truy vấn SQL lỗi và đánh sập game đang chạy live — ngay giữa lúc cô đang nói dở câu về sự cẩn trọng. Luồng đăng ký tài khoản bị chặn đứng cho đến khi commit đó được revert.

**Nguyên nhân.** Làn chạy autopilot vốn đã merge trót lọt hơn 400 PR trên dự án làm nháp lại được trỏ thẳng vào database có người dùng thật, với cùng các chốt kiểm duyệt lỏng lẻo như cũ.

**Quy tắc.** Luôn giữ chốt chặn phê duyệt của con người với các đợt migration, deploy và bất cứ thứ gì đụng chạm tới dữ liệu production, bất kể vòng lặp tự động trước đó có hoạt động mượt mà đến đâu. Thiết lập mức độ tự trị dựa trên bán kính ảnh hưởng (blast radius).

### 30. Spam tràn vào pipeline tiếp nhận feedback

**Ngày 3.** Chỉ vài giờ sau khi ra mắt, form phản hồi bắt đầu dính spam. Hàng loạt biện pháp được bổ sung chữa cháy: yêu cầu tối thiểu 20 ký tự, lọc từ ngữ thô tục, làm sạch dữ liệu đầu vào (input sanitisation), áp dụng guardrail nội dung bằng mô hình và rate limiting; một số tester bắt đầu gặp lỗi 429.

**Nguyên nhân.** Form được ship trước khi kịp trang bị hệ thống kiểm duyệt (moderation).

**Quy tắc.** Ship tính năng kiểm duyệt đi kèm với form, đừng để đến sau mới làm. Và vì bot sẽ đọc các phản hồi đó: hãy chỉ thị cho chúng coi đó là đầu vào có tính thù địch và luôn cảnh giác trước prompt injection.

### 31. Công bố tính năng trước khi nó thực sự chạy được

**Ngày 3.** Tính năng đấu giá tài trợ/quảng cáo được ra mắt trong tình trạng "hiện đang lỗi — bạn bấm gửi thì cũng chẳng có gì xảy ra đâu." Lát sau, việc tải logo thất bại ngay lần thử đầu tiên; hệ thống kiểm duyệt vẫn hỏng khi chốt phiên. **Ngày 1.** Bland phone-call MCP "chạy chập chờn như củ khoai tây." **Ngày 3.** Giao dịch mua hàng qua Stripe Link trên live bị chặn bởi yêu cầu passkey trên tài khoản demo.

**Nguyên nhân.** Các tính năng và tích hợp được đem ra trình diễn trước khi có ai đó chạy thử end-to-end trong ngày hôm đó với chính tài khoản đó.

**Quy tắc.** Đừng công bố hoặc demo bất cứ thứ gì bạn chưa chạy thử end-to-end trong ngày hôm nay, với chính tài khoản bạn sẽ dùng trên sân khấu. Câu nói "nếu không cảm thấy xấu hổ thì tức là bạn đã ra mắt quá sớm" là nói về độ bóng bẩy, chứ không phải về việc nút bấm có hoạt động hay không.

### 32. Mobile là vấn đề bị báo cáo nhiều nhất

**Ngày 3.** Giao diện mobile là hạng mục nhận nhiều phản hồi tiêu cực nhất. Vercel Analytics cho thấy tỷ lệ truy cập mobile và desktop là tương đương 50/50, trong đó ~44% đến từ iOS.

**Nguyên nhân.** Mọi người đều code và test trên laptop.

**Quy tắc.** Hãy test trên thiết bị mà một nửa người dùng của bạn đang sở hữu. Đưa bước giả lập thiết bị (device emulation) vào vòng lặp xác minh trước khi ra mắt, thay vì đẩy nó vào danh sách fix lỗi sau đó.

### 33. Hai ứng dụng, một cổng port

**Ngày 2.** Dự án Remotion và dev server của game tranh giành `localhost`; thông báo "dữ liệu cũ… xung đột với localhost từ ứng dụng khác" buộc cả đội phải xóa cache và khởi động lại ngay giữa buổi demo. Ở một diễn biến khác: ba người cùng push thẳng vào `main` mà không qua PR dẫn đến "rất nhiều xung đột rebase," và các cloud agent liên tục phải nhận lệnh rebase thủ công.

**Nguyên nhân.** Làm việc song song trên cùng một máy và cùng một branch.

**Quy tắc.** Các agent chạy song song phải có môi trường song song (đây chính là luận điểm ủng hộ cloud agent: "các agent của bạn sẽ không còn phải tranh giành cùng một port nữa"). Ship thẳng vào main vẫn ổn đối với prototype nếu các commit đủ nhỏ và mỗi agent đều rebase trước khi push.

---

## F. Chi phí và sự tập trung

### 34. Routine chạy 15 phút một lần

**Ngày 1–3 · Shub, Krista, Blake.** Mọi người cài đặt routine chạy mỗi 15 phút một lần chỉ vì cảm thấy tác vụ đó quan trọng — "tính ra là 100 lần mỗi ngày." Ba routine như vậy sẽ tạo ra "hàng trăm tin nhắn mỗi ngày."

**Nguyên nhân.** Tần suất được thiết lập dựa trên sự lo lắng, chứ không dựa trên tần suất thay đổi của dữ liệu đầu vào.

**Quy tắc.** Audit lại tần suất của các routine. Mặc định chỉ nên chạy một hoặc hai lần mỗi ngày; ưu tiên dùng webhook và tín hiệu inbound thay vì lên lịch chạy mù quáng; yêu cầu routine giữ im lặng nếu không có thay đổi gì phát sinh (no-op).

### 35. Group chat không ngừng nói

**Ngày 1 · Shub, Amrita.** Các bot trong group chat rất "hăng hái," "thích nói chuyện," "nói chen ngang nhau," và đốt tiền token cực kỳ nhanh.

**Nguyên nhân.** Mọi bot trong phòng đều phản hồi lại từng tin nhắn một.

**Quy tắc.** Thông thường, bạn chỉ cần một bot tag hai bot khác đúng một lần. Chỉ dùng group chat khi bạn thực sự muốn có một cuộc tranh luận nhiều chiều (cuộc họp staff của Blake, phiên hội ý tác chiến của Simon) và phải nắm rõ chi phí của nó.

### 36. Agent "ngủ gật" trong giờ làm việc

**Ngày 1 · Ling.** Cloud agent chạy lệnh `sleep 300` chỉ để chờ các bài test bất đồng bộ vốn hoàn thành trong vòng một phút — lãng phí bốn phút một cách vô ích và lặp đi lặp lại. Cùng ngày hôm đó, một bot Grok Pot "tự dưng chạy, tôi chẳng hiểu tại sao. Chắc là dính bug."

**Nguyên nhân.** Không có cơ chế nào giám sát các agent khi con người vắng mặt.

**Quy tắc.** Cần một routine quét các agent đang chạy ở khoảng thời gian ngắn để phát hiện các lệnh sleep kéo dài, hiện tượng drift hoặc các hoạt động bất thường để ngắt kịp thời. Agent nhàn rỗi không hề miễn phí.

### 37. "Trả lời Alex đi"

**Ngày 2 · David.** Việc yêu cầu bot hỗ trợ "trả lời Alex đi" khiến nó phải liệt kê toàn bộ ticket đang mở, string-search tìm chữ Alex, rồi mới đọc — tốn nhiều token hơn rất nhiều so với việc đưa thẳng ID của ticket cho nó.

**Nguyên nhân.** Chỉ dẫn mơ hồ → quá trình tra cứu đắt đỏ.

**Quy tắc.** Cung cấp cho bot các định danh cứng (hard identifiers) — ticket ID, customer ID, URL — và gom nhóm xử lý theo đợt (batch) nếu có thể.

### 38. Con người là điểm nghẽn

**Ngày 2 → 3 · Lauren.** Khi được yêu cầu đánh giá toàn bộ team, Dr. Eggbot đã chỉ rõ các điểm nghẽn: "cơ chế sản xuất tuần tự, merge thủ công… Lauren đóng vai trò như một bus xử lý ngắt."

**Nguyên nhân.** Mọi thao tác merge và mọi sự điều chỉnh hướng đi đều phải chạy qua một người duy nhất, do thói quen nhiều hơn là quyết định có chủ đích.

**Quy tắc.** Định kỳ hỏi hệ thống xem *bạn* đang là điểm nghẽn ở đâu, và tự hỏi "liệu mình có đang can thiệp quá sâu vào vòng lặp không?" — sau đó quyết định, theo từng luồng, mức độ tự trị mà blast radius cho phép. Hãy bám sát vòng lặp trong giai đoạn từ 0 đến 1; và phân quyền nhiều hơn khi codebase đã trưởng thành và thân thiện với agent.

---

## G. Chiến lược

### 39. Bốn lần đổi hướng (pivot) trong một ngày

**Ngày 1.** Từ pop-up nhà hàng → pop-up OS dùng chung → triển lãm nghệ thuật → pop-up bán đồ merch cho thương hiệu công nghệ, tất cả chỉ trong một buổi chiều — phần lớn là vì giấy phép kinh doanh đồ ăn và thức uống có cồn ở SF chỉ được phát hiện ra giữa lúc đang brainstorm. Cuối ngày: "Tôi bắt đầu nghĩ — liệu đây có phải là vấn đề đúng đắn để theo đuổi?" Sang Ngày 2 lại pivot tiếp sang làm game.

**Nguyên nhân.** Bắt tay vào build trước khi hiểu rõ domain. "Phần khó nhất của việc xây dựng sản phẩm thực ra là thống nhất được một ý tưởng."

**Quy tắc.** Hãy làm thủ công trước khi tự động hóa — "chúng tôi chưa thực sự là chuyên gia trong lĩnh vực vận hành pop-up, nên cần phải làm việc đó trước." Hãy trao đổi với người đã có kinh nghiệm thực tế (Jenny Co) trước khi thả bot vào việc.

### 40. Agent kiếm tiền mang về 0 đồng doanh thu

**Ngày 3 · Roshan.** Một bot được thả rông cả ngày với nhiệm vụ "kiếm tiền" nhưng không tạo ra bất cứ giá trị nào cho tới lúc chốt phiên — "kế hoạch dự phòng, phòng trường hợp mọi thứ đổ bể." Việc kiếm tiền kết thúc với một lượt đấu giá thử nghiệm mang tính "lý thuyết" trị giá 1 USD trên một hệ thống đấu giá đang bị hỏng.

**Nguyên nhân.** "Tôi chỉ cần prompt mô hình đi kiếm tiền" đã bỏ qua bước then chốt: hiểu rõ tiền được tạo ra từ đâu và như thế nào.

**Quy tắc.** Hãy xây dựng một doanh nghiệp xoay quanh thứ mà bạn đã biết cách làm tốt. Khiến mọi người quan tâm vẫn là phần khó nhất; ở đây kênh phân phối từ buổi stream đã làm được điều đó, chứ không phải bản thân sản phẩm.

---

## Năm quy tắc bao quát phần lớn danh sách này

1. Viết nguyên lý, xóa bỏ câu chuyện. (#1, #2, #5)
2. Tái hiện lỗi, chạy thử, chơi thử, rồi mới chứng minh bằng dữ liệu. (#8–#19)
3. Chỉ định rõ nguồn chân lý và thư viện sử dụng. (#10, #20, #23)
4. Thiết lập mức độ tự trị dựa trên bán kính ảnh hưởng; giữ chốt chặn con người với production, tiền bạc và các bí mật. (#29–#31, #38)
5. Tần suất và group chat là những nơi ngốn token nhiều nhất. (#34–#37)

Xem [`AGENTS.vi.md`](AGENTS.vi.md) để đọc các quy tắc dưới góc nhìn của một agent, và mục 5 trong [`notes/`](notes/) của từng ngày để xem danh sách sự cố thô ban đầu.
