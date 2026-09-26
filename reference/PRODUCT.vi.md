# PRODUCT.md

Những đặc điểm cốt lõi của Grok Bot làm thay đổi cách bạn thiết kế một bot. Đây không phải danh sách tính năng; marketplace và tài liệu chính thức từ xAI mới là nguồn thông tin chuẩn xác nhất cho phần đó. Tài liệu này ghi lại những điểm được chia sẻ trên stream vào tháng 9 năm 2026, tập trung vào những yếu tố tác động đến memory, isolation, sharing và permissions. Những điểm các diễn giả nói trái ngược nhau đều đã được đối chiếu và ghi chú lại cách giải quyết.

---

## Model tư duy

Bot là một cộng sự có định danh cụ thể với một nhiệm vụ duy nhất, không phải một chat thread thông thường. Bạn quay lại làm việc với chính bot đó. Nó có memory riêng, context limit riêng và máy tính riêng. Mọi nguyên tắc thiết kế trong repo này đều bắt nguồn từ 3 sự thật trên.

---

## Memory

- Tính theo từng bot, long-lived, có thể chỉnh sửa. Lưu trữ trên S3. Không chia sẻ dữ liệu giữa các bot trừ khi bot này gửi message cho bot kia.
- Steering có hiệu lực lâu dài. Chỉ cần dặn một lần "Không bao giờ dùng họ của tôi", bot sẽ nhớ mãi mãi.
- Bạn có thể yêu cầu bot quên một thông tin nào đó và nó sẽ làm theo. Hãy chủ động dọn dẹp: giữ lại những thông tin không còn cần thiết sẽ tốn token trên từng turn.
- Mỗi bot có context limit riêng. Đây là lý do nên chia nhỏ role thay vì nhồi nhét vào một bot: bot phải chuyển đổi qua lại giữa quá nhiều task sẽ nhanh chóng cạn context, trong khi bot chuyên trách phạm vi hẹp thì không.
- Hỗ trợ multitasking trong cùng một thread; bot có thể xử lý đồng thời hai yêu cầu mà không bỏ sót yêu cầu đầu tiên.

---

## Cơ chế chuyển giao: Những gì được giữ lại và loại bỏ

Ba thao tác sau nghe tương tự nhau nhưng cách thức hoạt động hoàn toàn khác biệt:

| Thao tác | Instructions | Memory | Skills | Credentials, chat history |
|---|---|---|---|---|
| **Duplicate** bot | copy | **trống** | vẫn được share chung | không |
| **Share as template** (team hoặc public) | copy | copy core memory, loại bỏ dữ liệu riêng của workspace | copy kèm first-party plugins | không |
| **Teach a skill** trên bất kỳ bot nào | — | — | **khả dụng cho mọi bot trong org** | — |

Ngày 1 có thông tin rằng việc chia sẻ sẽ copy "memories, context, instructions, first-party plugins"; ngày 3 lại đính chính rằng hệ thống "loại bỏ thông tin nhạy cảm và dữ liệu riêng của workspace, chỉ giữ core memory." Cả hai cách nói đều đúng bản chất: template chỉ mang theo những gì bot cần biết để phục vụ công việc, chứ không mang theo những gì bot biết về cá nhân bạn.

Hệ quả: hãy đưa kiến thức chuyên môn phục vụ công việc vào description và skills, không đưa vào memory tích lũy qua chat. Memory là phần sẽ mất khi duplicate và sẽ bị lọc bỏ khi share.

---

## Isolation

- Mỗi bot chạy trên một Linux VM riêng biệt. Bot này không thể can thiệp vào máy tính của bot khác. Hai bot cùng chỉnh sửa các slide khác nhau trên cùng một deck sẽ không bị xung đột.
- Các bot trong cùng một account dùng chung file system, nhưng không chia sẻ memory hay context.
- Tính năng messaging giữa các bot xuyên suốt các account khác nhau chưa có tại thời điểm stream.
- Local execution là một toggle tùy chọn theo từng account. Nhóm phát triển ưu tiên dùng cloud VM hơn: hỗ trợ chạy song song, không chiếm focus trên laptop của bạn, và vẫn tiếp tục chạy khi gập máy.

---

## Permissions

- Hệ thống tích hợp sẵn một classifier để đánh giá rủi ro của từng action và hỏi ý kiến người dùng trước các thao tác nhạy cảm. Trên stream, bot đã từ chối dựng form cho đến khi xác minh chắc chắn form đó không thu thập PII.
- Bạn có thể thiết lập các quy tắc rõ ràng theo từng loại action: "không bao giờ gửi email mà không hỏi trước", "tự do tạo slide", "deploy to production: luôn luôn phải hỏi trước."
- Enterprise admin có thể whitelist và blacklist các trang web cũng như MCP cho từng bot.
- Credentials được nhập qua form bảo mật hoặc 1Password. Bot không bao giờ nhìn thấy password; xAI cũng vậy. Trong khi đó, một đối thủ được demo trên stream lại in thẳng password dưới dạng plain text.
- Bạn có thể takeover màn hình của bot để thực hiện login và giải CAPTCHA. Giải pháp được khuyến nghị khi gặp CAPTCHA là block trang web đó, không phải tìm cách vượt qua lớp kiểm tra.

---

## Description đóng vai trò là system prompt

Field description vừa là persona vừa là instructions cho bot. Một bot-factory bot sẽ tự sinh nội dung này cho các bot mới và thường có xu hướng overfit vào đúng kịch bản ban đầu mà nó được tạo ra; hãy chỉnh sửa sớm. Labels chỉ là các tag hiển thị bên ngoài để bạn dễ nhớ vai trò của từng bot (ví dụ bot tên "Tater" làm nhiệm vụ gì).

---

## Các giới hạn bộc lộ trên stream

- Chỉ hỗ trợ Linux. Công cụ nào không tương thích với Linux và cũng không được expose dưới dạng MCP thì hoàn toàn không thể sử dụng.
- Bot chạy trên nhiều máy gặp tình trạng bị lẫn lộn dữ liệu; đội ngũ phát triển đã ghi nhận và đang xử lý lỗi này.
- Tính năng voice chỉ mới dừng ở mức transcribe-only vào ngày 1, và được nâng cấp lên hai chiều (two-way) vào ngày 3.
- Chưa có migration path từ các công cụ agent khác ngoài việc import template và trỏ bot vào một context có sẵn.
- Group chat hoạt động được nhưng tất cả bot đều sẽ phản hồi từng message. Xem thêm [ECONOMICS.md](ECONOMICS.vi.md).
