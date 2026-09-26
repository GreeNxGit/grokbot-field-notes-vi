# ORCHESTRATION.md

**Dùng khi:** bạn đang thiết kế một team gồm nhiều agent thay vì prompt từng con đơn lẻ, hoặc roster của bạn đã phình to vượt quá khả năng kiểm soát.

---

## One bot, one job (Mỗi bot, một việc)

Lời khuyên được lặp lại nhiều nhất suốt ba ngày, từ mọi diễn giả một cách độc lập: Không dùng một trợ lý đa năng duy nhất — hãy xây dựng một roster gồm các chuyên gia chuyên biệt, gọi đích danh từng con.

Bốn lý do mô hình này vượt trội:

1. **Context luôn được giới hạn phạm vi.** Mỗi agent có context limit riêng. Một con đa năng ôm đồm bốn việc không liên quan sẽ nhanh chóng cạn context và bắt đầu "quên".
2. **Bạn nhớ được cần hỏi ai.** "Não người cũng không thể chứa hết cả pho tiểu thuyết — bạn cần biết chính xác mình nên tìm ai."
3. **Khi được gán một vai trò rõ ràng, model hoạt động hiệu quả hơn.** Vòng lặp phản hồi chỉ thực sự chặt chẽ khi phạm vi công việc đủ hẹp để phản hồi mang lại ý nghĩa.
4. **Chạy song song hoàn toàn miễn phí.** Kích hoạt năm chuyên gia cùng lúc, để chúng làm việc, rồi quay lại tổng hợp kết quả.

### Thiết lập một role

Một role tốt cần nêu rõ:

- một phạm vi phụ trách duy nhất và rõ ràng, đủ gọn để đặt tên
- các công cụ và nguồn dữ liệu cụ thể mà nó được phép dùng
- **cách tiếp cận** công việc ra sao, không chỉ dừng lại ở việc đó là việc gì
- những hành động nào bắt buộc phải có phê duyệt trước khi thực thi
- nguồn chân lý (source of truth) của nó — thành viên hoặc tài liệu nào có tính chuẩn mực
- lịch trình định kỳ, nếu công việc có tính chu kỳ

Tránh các role chung chung như "General Helper". Hãy đi vào ngách hẹp: *Talent Scout*, *Expense Manager*, *Bug Reproduction*, *Release Notes*.

### Viết theo nguyên tắc, không viết theo sự vụ

Cách phổ biến nhất khiến một role bị hỏng: nó được viết hoặc chỉnh sửa ngay sau một sự cố cụ thể, khiến các chi tiết tình huống bị đóng cứng vĩnh viễn vào định nghĩa.

Văn bản role tồi:

> "Nắm bối cảnh tổng quan về studio của chúng tôi. Một nhiệm vụ duy nhất: chịu trách nhiệm về kết quả kỹ thuật bằng cách orchestrate công việc qua [toolchain hiện tại] và các cloud agent, sau đó giám sát và verify. Đối chiếu với playbook và làm theo."

Cách khắc phục là nói thẳng điều bạn thực sự muốn:

> *"Đọc lại playbook và đưa ra các nguyên tắc mà role này cần tuân theo, thay vì tập trung vào những vấn đề quá chi tiết này."*

---

## Cái bẫy bot-sprawl (bùng phát bot vô tội vạ)

Tạo một agent mới thì rẻ, nhanh và thú vị. Nhưng đó chính là vấn đề.

> "Tôi cam đoan rằng có những lúc tôi đã tạo quá nhiều bot. Thành thật mà nói, nó chỉ làm mọi thứ thêm hỗn loạn. Bạn thực sự phải tự vấn: tại sao việc tạo một bot *mới* để làm điều này lại quan trọng?"

> "Hãy giữ team thật tinh gọn. Bạn không cần đến 45 con bot đâu."

**Nguyên tắc:** trước khi tạo một agent mới, hãy kiểm tra xem một chuyên gia hiện có liệu chỉ cần thêm một *skill* hoặc *routine* mới hay không. Mỗi agent mới đều tiêu tốn context, công setup, routine và sự chú ý của bạn — từng con một, mãi mãi về sau.

Dấu hiệu cho thấy bạn đang có quá nhiều bot: bạn không nhớ nổi một con trong số đó làm gì; hai con giẫm chân lên nhau; bạn phải chuyển tiếp tin nhắn giữa chúng thủ công; hoặc có những agent cả tuần rồi bạn chưa hề mở ra.

---

## Bốn pattern khi vận hành trên năm agent

### 1. The chief of staff (Tham mưu trưởng)

Một agent duy nhất mà bạn trao đổi gần như liên tục. Nó điều phối công việc tới các chuyên gia và báo cáo kết quả lại cho bạn.

- **Khởi tạo mọi agent khác *thông qua* nó.** Nhờ đó, nó có đủ context để điều phối chính xác về sau.
- Nó **tự onboard các agent mới** — nhắn cho chúng context và các tiêu chuẩn để bạn không phải lặp lại với từng nhân sự mới.
- Nó nắm rõ "ai đang làm gì", giúp các chuyên gia không phải gánh thêm phần overhead đó.
- Có người thực tế đã vận hành 15–20 chuyên gia chỉ dưới quyền một chief duy nhất mà không cần tầng trung gian nào.

Không phải ai cũng hợp pattern này. Một số người thích nói chuyện trực tiếp với từng chuyên gia. Hãy thử nghiệm; nếu nó chỉ làm tăng thêm một bước trung gian mà không mang lại giá trị, hãy bỏ qua.

### 2. The playbook broadcast (Phát thanh playbook)

Một agent nắm giữ tài liệu sống về các tiêu chuẩn của team. Mọi tiêu chuẩn mới chỉ cần báo cho **nó** một lần duy nhất, và nó sẽ phổ biến lại cho toàn bộ các agent còn lại.

> "Bạn chỉ cần nghĩ về những gì cần làm một lần... lần tới khi cần thêm một workflow mới, chúng sẽ cập nhật vào playbook và toàn bộ kỹ sư đều nắm được mà không cần bạn phải đi dặn từng người."

Ví dụ về các tiêu chuẩn cần đưa vào:
- mọi PR đều phải đính kèm screenshot cho UI, số liệu perf cho backend
- công việc khẩn cấp được định nghĩa và xử lý như thế nào (xem bên dưới)
- những gì bắt buộc phải có con người phê duyệt

### 3. The staff meeting (Họp giao ban)

Kéo nhiều agent vào một thread để tranh luận về một quyết định dưới nhiều góc nhìn khác nhau, sau đó để chief tổng hợp lại thành đề xuất cuối cùng.

- **Chỉ thị cho chúng phải phản biện.** "Sẽ chẳng giúp ích gì nếu chúng chỉ biết đồng tình."
- Chỉ dùng cách này để ra quyết định, không dùng cho công việc thường nhật — các group thread rất tốn lời và nhanh chóng làm chi phí tăng vọt.

### 4. The sub-agent army (Đội quân sub-agent)

Một agent cấp trung chia nhỏ một tác vụ batch lớn cho một bầy worker ít context xử lý, kết quả sau đó được trả về cho một parent agent để audit. Ví dụ: research 200 tài khoản, fuzz 40 flow, tóm tắt 100 tài liệu. Rất dễ scale up và scale down linh hoạt.

---

## Chính sách thường trực hiệu quả hơn hối thúc

Liên tục giục agent "việc này khẩn cấp đấy" chỉ khiến nó bỏ qua các bước cần thiết và bắt đầu đoán mò. Thay vào đó, hãy định nghĩa chính sách một lần duy nhất:

> *"Thiết lập một routine kiểm tra các agent đang chạy mỗi năm phút một lần. Xem chúng có bị lệch hướng không — chạy sleep quá lâu, đi chệch mục tiêu, hoặc xử lý quá an toàn. Hãy can thiệp và nắn lại ngay khi phát hiện chúng lệch hướng."*

Áp dụng tương tự cho quy trình escalation, xử lý sự cố, và các tiêu chí xác định thế nào là blocking.

---

## Nấc thang autopilot

Phân quyền tự chủ theo từng nấc, và gọi tên nấc đó một cách rõ ràng trong instruction.

| Nấc | Ý nghĩa | Dùng khi |
|---|---|---|
| **Investigate** | Chẩn đoán, báo cáo lại, không chạm vào bất cứ thứ gì | Môi trường production, nguyên nhân chưa rõ ràng |
| **Draft** | Mở một PR, chờ con người review | Công việc thông thường |
| **Autopilot** | Implement, verify, tự merge khi test pass (green) | Phạm vi ảnh hưởng hẹp (low blast radius), vòng lặp verify tốt |
| **Full autopilot** | Lên plan, chia phase, implement, verify, merge | Chỉ dùng cho các dự án throwaway |

Hãy hạ bớt một nấc ngay khi hệ thống lên môi trường live. Team sáng tạo ra nấc thang này đã làm đúng như vậy vào ngày launch — *"có lẽ không nên để full autopilot, liệu có liều quá không?"* — bởi sản phẩm lúc đó đã nằm ngay trước mặt người dùng thực tế.

---

## Tổ chức roster

- **Chia section** để gom nhóm các agent như một sơ đồ tổ chức: Leadership, Engineering, War Room.
- **Gắn màu hoặc gắn nhãn theo chức năng**, thay vì theo tác vụ — research, outbound, account work — để chỉ cần liếc qua thread là biết ngay giai đoạn của pipeline.
- **Pin ba hoặc bốn con bạn thực sự dùng hàng ngày.** Nên ẩn phần còn lại thay vì xóa hẳn.

---

## Mô hình chi phí

Chi phí tính theo mức tiêu thụ: bạn trả tiền cho việc trao đổi với agent và việc chúng dùng tool. Bốn nguyên nhân chính gây tốn kém:

1. **Các routine chạy quá thường xuyên — nguồn rò rỉ lớn nhất.** "Nếu bạn có ba routine chạy mỗi 15 phút, số lượng tin nhắn sẽ lên tới hàng trăm tin mỗi ngày." Hãy audit tần suất hàng tuần. Ưu tiên dùng event trigger thay vì chạy theo schedule cố định. Thông thường, một đến hai lần mỗi ngày là đủ.
2. **Dùng browser automation khi đã có sẵn API.** Hãy quan sát agent thao tác một lần qua browser, yêu cầu nó kiểm tra các network request được kích hoạt, sau đó chuyển sang gọi thẳng các endpoint đó từ các lần sau.
3. **Để các group thread chạy ngầm liên tục.** Các agent trong một thread chung sẽ nói chèn lên nhau. Với công việc định kỳ thông thường, hãy ưu tiên trao đổi 1-1.
4. **Roster sprawl (phình to danh sách bot).** Càng nhiều agent đồng nghĩa với càng nhiều routine, tốn thêm context để thiết lập lại ban đầu, và dễ chồng chéo chức năng.

Các đòn bẩy bạn nắm quyền kiểm soát: mức độ ngắn gọn (verbosity), yêu cầu agent chủ động quên đi những context không còn cần thiết, và duy trì một agent duy nhất có nhiệm vụ chuyên tối ưu hóa routine cùng skill cho các con khác.

---

## Bài toán chưa ai giải được

> "Tôi hoàn thành được nhiều việc hơn bao giờ hết. Nhưng tôi cũng bận rộn hơn bao giờ hết."

Context-switching qua lại giữa hàng tá agent đang chạy đồng thời là một khoản chi phí thực sự mà hiện vẫn chưa có lời giải triệt để. Pattern chief-of-staff chỉ là giải pháp giảm thiểu tốt nhất hiện có, chứ không phải một bản fix triệt để. Hãy phân bổ sự chú ý của bạn cẩn trọng như cách bạn phân bổ token.
