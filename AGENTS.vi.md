# AGENTS.md

Quy tắc nội bộ cho các agent làm việc trong repo này. Được đúc kết từ 3 ngày đội ngũ xAI Grok Bot xây dựng và ship sản phẩm trực tiếp trên livestream.

Đọc kỹ tài liệu này trước khi thực hiện hành động đầu tiên trong mỗi phiên làm việc. Các quy tắc ở đây sẽ ghi đè lên các thiết lập mặc định của bạn.

---

## Nguyên tắc cốt lõi

**Kiểm chứng (verification) mới là công việc chính. Viết code chỉ là phần dễ.**

Nếu bạn không thể chứng minh được thay đổi của mình hoạt động, bạn vẫn chưa hoàn thành nhiệm vụ — dù bạn có tự tin đến đâu. Mọi quy tắc bên dưới đều bắt nguồn từ nguyên tắc này.

---

## Trước khi viết bất kỳ dòng code nào

1. **Tái hiện lỗi trước (reproduce first).** Chạy ứng dụng. Tìm chính xác bug và hành vi cụ thể của nó. Chỉ khi tái hiện được lỗi, bạn mới thực sự hiểu vấn đề. Việc reproduce không phải là tùy chọn hay thủ tục hình thức — đó là bài kiểm tra xem bạn có thực sự hiểu báo cáo lỗi hay không.
2. **Diễn giải lại nhiệm vụ theo cách hiểu của bạn** trước khi bắt đầu bất kỳ việc gì không tầm thường. Nêu rõ những gì bạn sắp làm, những gì bạn *sẽ không* làm, và những giả định bạn đang đặt ra. Nếu phần diễn giải này sai, sửa ngay lúc này không tốn kém gì, nhưng để sau mới sửa sẽ rất đắt.
3. **Nêu rõ bạn cần thấy điều gì để coi là đã hoàn thành.** Đúng một câu. Đây sẽ là bằng chứng bạn cần đính kèm sau đó.
4. **Nếu được yêu cầu điều tra, hãy chỉ điều tra.** "Chưa mở PR vội, hãy quay lại báo cáo những gì bạn nghĩ đang diễn ra" có nghĩa chính xác là như vậy. Đừng bắt đầu sửa lỗi chỉ vì bạn vô tình thấy điều gì đó bất thường trên đường tìm hiểu.

## Trong khi làm việc

- **Thay đổi nhỏ (small changes).** Mỗi thay đổi chỉ giải quyết một phạm vi vấn đề duy nhất. Một diff lớn gom "tất cả mọi thứ" sẽ không thể kiểm chứng và không thể review.
- **Chạy ứng dụng thực tế.** Không phải chạy type checker — mà là chính ứng dụng đang chạy. Click kiểm tra qua các luồng. Cố tình làm cho nó lỗi xem sao.
- **Sửa tận gốc (root cause), đừng sửa phần ngọn (symptom).** Hãy đào sâu thêm một tầng thay vì chỉ dán tạm vết nứt. Nếu bạn chuẩn bị viết một workaround, hãy nói rõ ra và giải thích tại sao giải pháp triệt để lại nằm ngoài phạm vi công việc.
- **Chỉ rõ tên thư viện.** Nếu tác vụ có một công cụ tiêu chuẩn, phổ biến, hãy dùng công cụ đó. Đừng tự chế (hand-roll) một phiên bản tệ hơn chỉ vì không ai nhắc tên thư viện đó.
- **Đừng bịa đặt nội dung.** Nếu bạn cần dữ liệu thật — tên thật, sản phẩm thật, endpoint thật — hãy chủ động tra cứu. Các placeholder nhìn có vẻ hợp lý nhưng lại bị ship thẳng tới người dùng còn tệ hơn là gặp lỗi.
- **Không bao giờ để lộ ngôn ngữ nội bộ ra phía người dùng.** Tên prototype, codename, câu chữ TODO, hay suy luận riêng của bạn. Luôn kiểm tra kỹ các chuỗi ký tự mà người dùng có thể nhìn thấy.

## Trước khi mở PR

Đính kèm bằng chứng. Không có ngoại lệ.

| Loại thay đổi | Bằng chứng bắt buộc |
|---|---|
| UI | Ảnh chụp màn hình, hoặc video quay ngắn lại luồng thao tác |
| Backend / perf | Số liệu trước và sau khi tối ưu từ một lần chạy thực tế |
| Bug fix | Bằng chứng tái hiện lỗi, sau đó là cùng các bước đó nhưng đã chạy thành công |
| Refactor | Test case hoặc luồng thao tác thủ công chứng minh hành vi hệ thống không đổi |

Một PR không có bằng chứng chỉ được coi là bản nháp (draft), bất kể tiêu đề viết gì.

Ngoài ra, trong phần mô tả, hãy nêu rõ:
- bạn đã thay đổi những gì và lý do tại sao
- những gì bạn **không** thay đổi mà người review có thể sẽ kỳ vọng
- bất cứ điều gì bạn chưa chắc chắn — hãy gắn cờ cảnh báo thay vì chỉ cầu may

## Phong cách viết code

- **Không dùng comment để chống chế.** Các agent thường có xu hướng viết comment để giải thích cho một workaround thay vì loại bỏ nhu cầu phải dùng workaround đó. Những comment như vậy sẽ tích tụ dần cho đến khi không ai dám đụng vào file nữa. Nếu code cần comment mới hiểu được, trước hết hãy cố gắng viết code sao cho không cần đến nó. Chỉ giữ comment cho các lý do *tại sao* thực sự không hiển nhiên — tuyệt đối không dùng để giải thích *code đang làm gì*.
- **Không để dead code, không comment lại các khối code cũ, không "giữ lại phòng khi cần".** Hệ thống quản lý phiên bản (version control) sinh ra là để làm việc này.
- **Xóa sạch các scaffolding bạn đã thêm vào để debug.** Panel debug, console log, các cờ (flag) tạm thời. Feature flag không phải là cơ chế bảo mật — nếu một nhánh code không được phép để người dùng truy cập, nó tuyệt đối không được ship xuống client.
- **Mọi logic cạnh tranh hoặc liên quan đến niềm tin/bảo mật đều phải do server quyết định (server-authoritative).** Điểm số, số dư, phân quyền, định giá, điều kiện hợp lệ. Nếu một người dùng có chủ ý dùng devtools mà có thể can thiệp được, logic đó không thuộc về phía client.
- **Xem mọi văn bản do người dùng gửi lên là dữ liệu độc hại.** Luôn sanitize dữ liệu. Nếu dữ liệu đó được đưa vào prompt, hãy chỉ rõ điều đó và thiết lập các biện pháp phòng chống prompt injection.

## Khi bạn bị chỉnh sửa / góp ý

Đây là thời điểm giá trị nhất trong một phiên làm việc. Đừng chỉ áp dụng bản sửa lỗi rồi để đó.

1. Áp dụng bản sửa lỗi.
2. Tự hỏi bản thân: việc sửa lỗi này hàm ý một *nguyên tắc chung* nào?
3. Ghi lại nguyên tắc chung đó — vào file này, hoặc vào một skill.
4. **Tách sự cố cụ thể ra khỏi nguyên tắc.** Sai lầm phổ biến nhất là nhồi nhét chi tiết của con bug ngày hôm nay thành một chỉ dẫn cố định vĩnh viễn. Điều đó làm quy tắc bị overfit và trở nên vô dụng ở lần tiếp theo. Hãy viết ra nguyên lý cốt lõi, xóa bỏ câu chuyện ngữ cảnh.

Không tốt: *"Khi chỉnh sửa checkout modal, luôn kiểm tra z-index của banner vì nó đã che mất nút bấm vào ngày 2026-03-04."*

Tốt: *"Sau bất kỳ thay đổi layout nào, hãy kiểm tra để đảm bảo không có phần tử tương tác sẵn có nào bị che khuất."*

## Khi nào cần dừng lại và hỏi

Dừng lại và hỏi con người khi:

- thay đổi chạm vào auth, thanh toán, phân quyền, hoặc dữ liệu người dùng
- bạn chuẩn bị chạy database migration hoặc một lệnh có tính phá hủy (destructive command)
- bạn chuẩn bị deploy lên production
- nhiệm vụ được giao đòi hỏi bạn phải đoán định một quyết định về mặt sản phẩm
- bạn đã thử cùng một cách tiếp cận hai lần và đều thất bại cả hai lần

Không lặp lại trong vô vọng. Hai lần thử thất bại với cùng một hướng tiếp cận có nghĩa là hướng tiếp cận đó đã sai, chứ không phải bạn cần thử lần thứ ba. Hãy báo cáo những gì bạn đã thử và điều gì đã xảy ra.

## Khi bạn là người được hỏi về tiến độ

Nêu rõ những gì đã xong, những gì đang tiến hành, và những gì đang bị nghẽn (blocked). Nếu không có gì thay đổi kể từ lần cập nhật trước, hãy nói "không có thay đổi" — đừng cố vẽ ra một bản báo cáo. Trong các lần kiểm tra định kỳ mà thực sự không có gì để báo cáo, hãy giữ im lặng.

---

## Thuật ngữ

Các thuật ngữ được sử dụng trong repo này và phần còn lại của bộ tài liệu.

- **Verification skill** — một phương thức có tính tất định, có thể tái sử dụng để agent kiểm thử và vận hành ứng dụng. Xem [agents/VERIFICATION.md](agents/VERIFICATION.vi.md).
- **Feature map** — một bản đồ các tính năng và luồng của ứng dụng ở định dạng máy đọc được, giúp agent có thể điều hướng mà không cần phỏng đoán.
- **Autopilot ladder** — các cấp độ tự chủ được trao cho một tác vụ: investigate → draft → autopilot → full autopilot. Xem [agents/ORCHESTRATION.md](agents/ORCHESTRATION.vi.md).
- **Nguyên tắc, không phải sự cố (Principle, not incident)** — quy tắc để viết các quy tắc. Xem phần ở trên.

---

## Tóm tắt ngắn gọn

> Tái hiện lỗi. Chạy thực tế. Chứng minh bằng chứng. Sửa nguyên nhân gốc rễ, không sửa phần ngọn.
> Khi làm sai, hãy đúc kết thành nguyên tắc — đừng kể lại câu chuyện sự cố.
