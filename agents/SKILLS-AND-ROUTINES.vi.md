# SKILLS-AND-ROUTINES.md

**Dùng khi:** bạn muốn agent không bao giờ cần được nhắc lại cùng một chỉ dẫn lần thứ hai.

Hai khối nền tảng cốt lõi: một **skill** định nghĩa *cách* làm một việc; một **routine** quyết định *khi nào* việc đó được thực thi. Luôn theo thứ tự này, và tuyệt đối không tạo trước khi công việc đã được thực hiện thủ công ít nhất một lần.

```
Làm thủ công  →  Đạt kết quả ổn định  →  Lưu thành skill  →  Gán cho một routine
```

Nhảy cóc thẳng lên tự động hóa đồng nghĩa với việc đóng gói một quy trình chưa từng được ai kiểm chứng.

---

## Ba cách tạo ra một skill

### 1. Bằng thị phạm (demonstration)

Tự ghi lại một lần bạn thao tác công việc; agent sẽ chuyển bản ghi đó thành một skill có thể tái sử dụng.

Rất phù hợp cho: workflow thiên về UI, tác vụ trên các công cụ không có API, dạng thao tác "click vào đây, rồi vào đây, rồi export".

**Chỉ riêng một lần thị phạm là chưa đủ.** Nó mới chỉ nắm được luồng chuẩn (happy path). Sau khi ghi lại, hãy bổ sung thủ công:
- logic ra quyết định (cần làm gì khi gặp trường hợp khác)
- xử lý lỗi (cần làm gì khi một bước bị fail)
- điểm chốt phê duyệt (bước nào bắt buộc phải có con người duyệt trước khi tiếp tục)

### 2. Bằng chỉnh sửa phản hồi (correction) — nguồn giá trị cao nhất

> "Bất cứ khi nào bạn thấy agent tư duy sai, đó nhiều khả năng là cơ hội tốt để xây dựng một skill khắc phục điểm đó."

Không phải là re-prompt né lỗi. Càng không phải tự âm thầm sửa lỗi bằng tay — làm vậy sẽ phá vỡ hoàn toàn vòng lặp cải tiến. Hiệu ứng tích lũy (compounding) chỉ xảy ra khi phần sửa đổi được lưu vào một nơi cố định lâu dài.

> "Nếu bạn lặp lại việc đó liên tục, bạn sẽ sở hữu những agent ngày càng hoàn thiện theo thời gian — thay vì tư duy 'tiện tay sửa luôn cho xong', cách khiến bạn đánh mất feedback loop."

### 3. Bằng cách tự viết

Khi quy trình đòi hỏi phần lớn là sự suy xét và đánh giá (judgment), hãy viết trực tiếp. Đọc luồng suy luận của agent là nguồn tư liệu rất tốt: *"rất nhiều skill tôi tạo ra đều bắt nguồn từ việc quan sát các block suy nghĩ (thinking blocks) này."*

---

## Cái bẫy overfit

Đây là nguyên nhân phổ biến nhất khiến một skill trở nên vô dụng, và nó xảy ra chính *vì* bạn viết nó đúng thời điểm nhạy cảm nhất — ngay sau khi sự cố vừa phát sinh.

> "Bất cứ khi nào một rule hoặc skill mới sinh ra từ vấn đề vừa gặp trong session chat đó, agent có xu hướng nhồi nhét toàn bộ chi tiết của phiên chat vào rule. Điều này làm skill giảm tính khả thi, vì nó đã bị overfit."

**Hãy viết nguyên lý. Hãy xóa câu chuyện.**

| Overfit | Khái quát hóa (General) |
|---|---|
| "Khi export invoice bị lỗi 502 ở lô tháng 3, retry 2 lần rồi email cho Dana." | "Khi gặp lỗi upstream tạm thời (transient failure), retry với cơ chế backoff; escalate cho owner phụ trách sau khi hết ngân sách retry." |
| "Không đặt banner phía trên nút mua hàng ở trang sản phẩm." | "Sau mọi thay đổi về layout, kiểm tra để đảm bảo không có interactive element nào bị che khuất." |
| "Dùng v3 endpoint vì v2 vừa bị hỏng vào thứ Ba tuần trước." | "Dùng endpoint hiện hành theo tài liệu; luôn kiểm tra docs trước khi mặc định phiên bản." |

Một bài test thực tế hữu ích: **rule này liệu có còn dễ hiểu với một người không hề chứng kiến sự cố đó hay không?** Nếu nó chỉ có nghĩa khi biết ngữ cảnh sự cố cũ, nó đã bị overfit.

---

## Một skill tốt mô tả những gì

- khi nào skill được áp dụng — và khi nào *không*
- input đầu vào và quyền truy cập hệ thống cần thiết
- trình tự các bước thực hiện
- cách thức kiểm tra và verify kết quả
- định dạng output kỳ vọng
- những hành động nào bắt buộc phải có phê duyệt

---

## Routines

### Một routine cần định nghĩa những gì

- agent nào chịu trách nhiệm sở hữu (owner)
- lịch chạy (schedule) và múi giờ — **hoặc ưu tiên dùng event trigger**
- input được lấy từ đâu
- định dạng output kỳ vọng
- ranh giới cần xin phê duyệt
- phương án xử lý khi gặp sự cố
- **cần làm gì khi không có gì để báo cáo**

Ý cuối cùng này thường bị đánh giá thấp. Hãy chỉ định rõ cho routine rằng khi không có tác vụ phát sinh (no-op), nó nên xử lý âm thầm hoặc không phát thông báo. Nếu không, bạn sẽ tự tập cho mình thói quen phớt lờ thông báo, để rồi bỏ sót đúng vào ngày có sự cố thực sự xảy ra.

### Tần suất chính là ngân sách chi phí

Yếu tố chi phối chi phí lớn nhất trong hệ thống của mọi kỹ sư thực chiến. Ba routine chạy mỗi 15 phút đồng nghĩa với hàng trăm lượt chạy mỗi ngày.

- Luôn ưu tiên dùng **event hoặc webhook trigger** hơn là đặt lịch cố định
- Tần suất 1-2 lần mỗi ngày là đủ cho phần lớn nhu cầu báo cáo
- Polling mỗi 5 phút chỉ dành cho việc chủ động giám sát các tiến trình đang chạy, không dùng để canh chừng một sự kiện chỉ xảy ra 2 lần mỗi tuần
- **Audit định kỳ.** Hãy đặt một lịch nhắc lặp lại trên calendar của chính bạn để rà soát danh sách và loại bỏ những routine không mang lại giá trị tương xứng

### Connector hay browser

Một connector (như MCP, API plugin) sẽ nhanh hơn việc để bot điều khiển trình duyệt, chi phí trên mỗi tác vụ rẻ hơn, và dễ dàng cấu hình whitelist hoặc blacklist hơn. Điền một web form bằng cách click qua giao diện tốn kém hơn nhiều so với việc gọi trực tiếp API của form đó. Hãy dùng connector nếu có; browser chỉ là giải pháp dự phòng cuối cùng cho những trường hợp không có connector, chứ không phải lựa chọn mặc định.

### Test trên dữ liệu an toàn

Một lượt test run sẽ thực thi các tác vụ thực tế. Nó có thể làm thay đổi các file thật và gửi request tới các hệ thống thật. Lần đầu tiên chạy thử, hãy trỏ nó tới môi trường hoặc dữ liệu thử nghiệm dùng một lần (disposable).

---

## Routine tự cải tiến (Self-improvement routines)

Một routine hàng tuần để agent tự audit lại hoạt động của chính nó:

1. **System audit** — tuần này có công việc thủ công nào phát sinh mà lẽ ra có thể tự động hóa được không?
2. **Học văn phong/phong cách** — diff phần bạn đã chỉnh sửa với bản draft mà agent tạo ra, rồi nạp lại độ lệch (delta) đó vào hệ thống để bản nháp tiếp theo chuẩn xác hơn.

**Giới hạn output tối đa một đề xuất mỗi tuần.** Quá nhiều gợi ý tự cải tiến sẽ gây cảm giác spam và dễ bị phớt lờ, làm mất đi mục đích ban đầu.

---

## Checklist khi tạo một skill mới

- [ ] Công việc đã được thực hiện thủ công ít nhất một lần trước đó
- [ ] Nêu rõ khi nào áp dụng và khi nào không
- [ ] Bao gồm đầy đủ xử lý lỗi và cổng phê duyệt (approval gates), không chỉ mỗi happy path
- [ ] Không chứa bất kỳ chi tiết nào ám chỉ một sự cố cụ thể trong quá khứ
- [ ] Một người ngoài cuộc đọc vào cũng hiểu rõ từng dòng
- [ ] Nêu rõ phương pháp kiểm tra kết quả
