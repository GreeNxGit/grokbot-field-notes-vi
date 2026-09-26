[![Grok Bot Guide by SpaceX Engineers](guide/cover.png)](guide/grok-bot-guide-by-spacex-engineers.pdf)

# Grok Bot Field Notes

Ba kỹ sư từ team xAI Grok Bot đã xây dựng và ra mắt một sản phẩm từ repo trống trong 72 giờ, live trực tiếp trên stream bằng chính nền tảng agent của họ: [Roshan Sadanani](https://www.linkedin.com/in/roshansadanani) (product), [Lauren Tan](https://www.linkedin.com/in/laurenelizabethtan) (engineering, tác giả của PStack), và [Matt Palmer](https://www.linkedin.com/in/matt-palmer) (developer experience). Repo này là toàn bộ những gì tôi đúc kết được sau ba ngày đó: một bản guide được thiết kế chỉn chu, bộ rules có thể áp dụng ngay cho agent của bạn, 9 role playbook, một danh mục các bot role, cùng log chi tiết về mọi lỗi đã xảy ra.

## Cấu trúc repo

| Đường dẫn | Nội dung |
|---|---|
| `AGENTS.md` | Bộ house rules cho coding agent. Đặt file này ở thư mục gốc (root) của repo để agent tự đọc. |
| `ANTIPATTERNS.md` | 40 lỗi đã gặp phải trực tiếp trên stream. Mỗi mục gồm: lỗi là gì, nguyên nhân, và rule rút ra từ đó. |
| `agents/` | Tài liệu tham khảo chi tiết hơn mà `AGENTS.md` dẫn tới: verification, orchestration, skills và routines, prompts. |
| `roster/` | 69 agent role, mỗi file là một role. Gồm: scope công việc (nhận gì và không nhận gì), nguồn lấy facts, tác vụ cần phê duyệt (approval), và phần mô tả (description) có thể copy paste ngay. |
| `playbooks/` | 9 workshop theo từng vai trò: engineering, PM, founders, sales engineering, sales, SDR, support, post-sales, marketing. Mỗi playbook bao gồm team bot, workflow triển khai thực tế, prompt, routine và các số liệu liên quan. |
| `guide/` | [*Grok Bot Guide by SpaceX Engineers*](guide/grok-bot-guide-by-spacex-engineers.pdf), tài liệu PDF dài 24 trang kể lại hành trình ba ngày dưới dạng một câu chuyện: mental model, software factory, case study, failure log, và bài toán economics. |
| `reference/` | Hai file ngắn: `ECONOMICS.md`, tổng hợp mọi chi phí và metrics thực tế cùng rule đi kèm; `PRODUCT.md`, các cơ chế của Grok Bot làm thay đổi cách thiết kế bot: memory, dữ liệu nào được chuyển khi duplicate và share, isolation, permissions. |
| `notes/` | Ghi chép có cấu trúc theo từng ngày: product facts, workflows, prompts, sự cố, số liệu, phân công nhân sự. Toàn bộ nội dung khác đều được tổng hợp từ đây. |

## Bắt đầu từ đâu

- Cần bộ rule áp dụng ngay cho agent: copy `AGENTS.md` vào thư mục root của repo.
- Agent liên tục nhờ bạn test hộ code: xem `agents/VERIFICATION.md`.
- Muốn thiết kế một team gồm nhiều agent thay vì chỉ prompt một agent đơn lẻ: xem `agents/ORCHESTRATION.md`.
- Cần các mẫu prompt thực tế đã được kiểm chứng hiệu quả: xem `agents/PROMPTS.md`.
- Cần bản job description của bot để copy dùng ngay: vào `roster/`, bắt đầu từ `roster/README.vi.md`.
- Cần setup cho vị trí cụ thể của bạn (ví dụ support hoặc sales): vào `playbooks/`, bắt đầu từ `playbooks/README.vi.md`.
- Muốn nắm được những lỗi thường gặp: xem `ANTIPATTERNS.md`.
- Muốn hiểu chi phí thực tế và token tiêu hao vào đâu: xem `reference/ECONOMICS.md`.
- Cân nhắc dữ liệu nào nên đưa vào memory, dữ liệu nào nằm ở description: xem `reference/PRODUCT.md`.
- Muốn đọc toàn bộ câu chuyện: xem [bản guide PDF](guide/grok-bot-guide-by-spacex-engineers.pdf).
- Muốn đối chiếu lại các số liệu và nhận định: xem `notes/`.

## Tóm gọn trong một đoạn

Giao cho mỗi agent đúng một nhiệm vụ cụ thể và đặt cho nó một cái tên. Thiết lập verification loop trước khi tạo agent thứ hai.

Bắt buộc agent phải reproduce được bug trước khi tiến hành fix, và luôn đính kèm bằng chứng xác minh cho mọi thay đổi. Khi agent làm sai, hãy rút ra nguyên tắc chung cốt lõi, tuyệt đối không ghi lại chi tiết một trường hợp cụ thể.

Audit định kỳ các routine hàng tuần, vì tần suất chạy chính là nơi ngốn nhiều chi phí nhất. Luôn duy trì chốt chặn kiểm duyệt thủ công (human gate) với các tác vụ database migration, deploy, thanh toán và cấp quyền permissions, bất kể loop tự động trước đó có chạy mượt mà đến đâu.

## License

MIT. Bạn có thể tự do copy bất kỳ tài liệu nào ở đây vào repo của mình.
