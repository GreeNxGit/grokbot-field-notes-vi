# VERIFICATION.md

**Dùng khi:** thiết lập dự án để agent có thể tự kiểm tra công việc của mình, hoặc khi agent liên tục hoàn thành task bằng câu: "bạn chạy thử rồi báo lại xem có chạy được không nhé."

Câu nói đó là triệu chứng. File này chính là giải pháp.

---

## Tại sao đây là thứ đầu tiên cần xây dựng

Nếu không có vòng lặp kiểm chứng (verification loop), mọi task đều kết thúc bằng một vòng lặp qua bạn. Bạn biến thành bộ test suite. Khi có vòng lặp này, agent tự khép kín quy trình kiểm tra và bạn chỉ nhìn thấy kết quả đã hoàn tất.

Tiêu chí chung để xác định xem một công việc có an toàn để giao cho agent hay không:

> **Vòng lặp có thể kiểm chứng được không?** Có tín hiệu nào mà máy móc có thể đọc được để xác định xem việc này thành công hay thất bại không?

Lập trình là trường hợp dễ nhất — test, build và ứng dụng đang chạy đều tạo ra tín hiệu đó. Bất cứ việc gì chưa có tín hiệu này đều cần phải tạo ra nó trước khi trao quyền tự chủ an toàn.

---

## Một verification skill thực chất bao gồm những gì

### 1. Một CLI mà agent có thể vận hành

Không phải là các script ad-hoc được viết mới mỗi lần. Chúng tiêu tốn token, kết quả khác nhau giữa các lần chạy và không thể suy luận chắc chắn được.

> "Bạn cần một CLI, một script, hoặc công cụ nào đó cho phép các bot tương tác với ứng dụng một cách đáng tin cậy và có tính xác định (deterministic). Tốt nhất là chỉ cần một bộ công cụ chuẩn."

Xây dựng mỗi hành động có ý nghĩa bằng một lệnh, với tên gọi ổn định và output nhất quán:

```
./verify seed              # đưa ứng dụng về trạng thái xác định
./verify run <flow>        # chạy thử nghiệm end-to-end một flow cụ thể
./verify screenshot <page> # chụp ảnh bằng chứng
./verify check             # kiểm tra các invariant, thoát với mã lỗi khác 0 khi thất bại
```

Nguyên tắc cho CLI:
- deterministic — cùng input, cùng output
- luôn thoát với mã lỗi khác 0 khi thất bại
- in ra những gì nó đã thực hiện, không chỉ báo là pass hay fail
- an toàn khi chạy lặp lại nhiều lần

### 2. Một feature map

Mô tả ở định dạng máy đọc được về những gì ứng dụng có và cách truy cập tới đó, giúp agent khi điều hướng không phải đoán mò.

```yaml
# feature-map.yaml
checkout:
  path: /checkout
  requires_auth: true
  entry: click "Buy" on any product page
  flows:
    - happy_path: add item -> checkout -> pay -> confirmation
    - declined_card: add item -> checkout -> pay(declined) -> error state
  invariants:
    - order total always equals sum of line items
    - no order is created before payment succeeds
```

Cân nhắc cung cấp thêm phiên bản mà agent có thể đọc được cho các quy tắc sản phẩm của bạn — một trang `/rules`, hoặc endpoint dạng `llms.txt`. Nếu agent sử dụng sản phẩm của bạn, hãy viết tài liệu cho chúng như cách bạn viết cho con người.

### 3. Gọi lệnh theo tên chuẩn (Named invocation)

Đặt cho nó một cái tên và dùng đúng tên đó. Chẳng hạn như `/verify <project>`. Khi đã có, hãy dẫn chiếu nó trong mọi chỉ dẫn trao quyền tự chủ:

> "...dùng autopilot. Vì hệ thống hiện đã live trên production, điều tối quan trọng là chúng ta không làm gián đoạn người dùng. Do đó, hãy luôn kiểm chứng nghiêm ngặt bằng `/verify <project>` trước khi merge."

---

## Ba nguyên tắc cần thực thi

1. **Tái hiện lỗi trước khi sửa.** Chỉ khi agent tái hiện được bug, bạn mới có thể tin rằng nó đã hiểu đúng vấn đề. Chỉ dẫn thường trực:

   > *"Trước khi viết bất kỳ dòng code nào, hãy chạy app, tìm chính xác bug và hành vi lỗi, rồi mới tiến hành tiếp."*

2. **Bằng chứng phải đưa vào PR.** Screenshot hoặc video đối với UI. Số liệu đối với backend. Các bước tái hiện lỗi, sau đó là chính các bước đó chạy thành công, đối với bug fix. Agent có khả năng ghi lại phiên chạy của mình nên đính kèm bản ghi.

3. **Verification là cánh cổng merge.** Không phải là con người ngồi đọc diff. Đó là yếu tố tạo nên thông lượng (throughput) cao — nhưng hãy xem lưu ý bên dưới.

---

## Fuzzing và swarms

Khi đã có CLI, bạn có thể hướng nhiều agent cùng lúc vào nó: mỗi agent chạy app, click thử khắp nơi và cố gắng làm nó crash. Cách này tìm ra một nhóm bug khác hẳn so với con người tìm, và bạn chỉ tốn token.

Hai điều cần lưu ý:
- con người vẫn tìm thấy những bug mà swarm bỏ sót, và ngược lại — nên kết hợp cả hai
- một đợt chạy fuzzing sẽ **thực thi tác vụ thật** lên bất kỳ môi trường nào nó nhắm tới. Hãy trỏ nó vào môi trường dùng một lần (disposable environment), tuyệt đối không trỏ vào production

---

## Lưu ý quan trọng, nói một cách thẳng thắn

Auto-merge khi pass test (green) chỉ nên áp dụng tỷ lệ thuận với phạm vi ảnh hưởng (blast radius).

Đội ngũ từng ship 433 PR theo cách này đã nói thẳng: *"Nói thật lòng, tôi không xem code một chút nào cả."* Đó là một game ngắn hạn làm trong 72 giờ rồi bỏ. Còn trên sản phẩm thực tế, chính đội ngũ đó đọc từng PR và kiểm soát các anti-pattern bằng tay.

Hãy đặt mức độ tự chủ dựa trên cái giá phải trả khi xảy ra sai sót, chứ không phải dựa trên việc vòng lặp gần đây hoạt động trơn tru ra sao. Và dù thế nào đi nữa, hãy luôn giữ con người làm chốt chặn cho migration và deploy — một bản fix tự động từng làm sập production của họ vì một câu query SQL lỗi giữa luồng stream.

---

## Checklist

- [ ] Có một lệnh để đưa app về trạng thái xác định
- [ ] Có một lệnh để chạy thử từng critical flow
- [ ] Mọi lệnh đều thoát với mã lỗi khác 0 khi thất bại
- [ ] Có feature map và được cập nhật mới nhất
- [ ] Skill có tên gọi rõ ràng và các chỉ dẫn đều gọi đúng tên đó
- [ ] Fuzzing chỉ chạy trên môi trường disposable
- [ ] Migration và deploy vẫn bắt buộc phải có con người phê duyệt
