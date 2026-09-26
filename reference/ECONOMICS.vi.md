# ECONOMICS.md

Toàn bộ chi phí và chỉ số được trích dẫn trên stream, tập hợp tại một nơi, đi kèm nguyên tắc tương ứng cho từng mục. Các số liệu là phát biểu trực tiếp của diễn giả vào tháng 9 năm 2026; hãy xem chúng là ước tính về mặt độ lớn (order-of-magnitude), không phải bảng báo giá cố định.

---

## Chi phí cho từng đầu việc

| Công việc | Chi phí | Tương đương làm thủ công | Nguồn |
|---|---|---|---|
| Bộ slide case-study bán hàng hoàn chỉnh | $20–30 | 4–5 giờ | Amrita, ngày 2 |
| Xử lý xong support ticket mức độ trung bình/phức tạp | $1–2 | con người tốn nhiều hơn mức này | David, ngày 2 |
| Support ticket thanh toán độ phức tạp thấp, sau khi gom nhóm (bucketing) và dùng batch script | ~$0.20 | — | David, ngày 2 |
| Các sản phẩm support-agent tương đương trên thị trường | $1–10 mỗi ticket | — | David, ngày 2 |
| Tìm gói cước PG&E rẻ hơn | ~60 giây chú ý | tiết kiệm ~$1,000/năm | Matthew Berman, ngày 2 |

Các số liệu hỗ trợ khách hàng là cặp so sánh giá trị nhất: cùng một đầu việc giảm từ $1–2 xuống còn $0.20 khi các ticket độ phức tạp thấp được phân loại vào nhóm riêng và xử lý bằng script thay vì tốn một lượt suy luận (reasoning pass). Chi phí phụ thuộc vào cách bạn thiết lập hệ thống, không nằm ở bản thân công cụ.

---

## Yếu tố làm hóa đơn tăng vọt

| Tác nhân | Số liệu trích dẫn | Nguyên tắc |
|---|---|---|
| Chạy routine định kỳ mỗi 15 phút | "tính ra 100 lần một ngày"; chạy 3 routine như vậy là "hàng trăm tin nhắn mỗi ngày" | Mặc định chỉ nên chạy 1 đến 2 lần/ngày. Ưu tiên webhook và tín hiệu gửi đến (inbound) thay vì polling liên tục trong vô vọng. |
| Bot trong group chat | "quá sốt sắng", "thích nói", "nói đè lên nhau", đốt tiền rất nhanh | Một bot chỉ tag hai bot khác đúng một lần. Chỉ dùng group chat khi thực sự cần tranh luận. |
| Thao tác UI qua browser của bot | click điền form trên web tốn kém hơn nhiều so với gọi API trực tiếp | Dùng connector nếu có sẵn. Browser chỉ là phương án dự phòng (fallback). |
| Quá nhiều bot | giữ team tinh gọn, "không cần đến 45 bot" | Mô hình một chief of staff quản lý 10–20 specialist chuyên biệt hoạt động hiệu quả; không ai cần đến các middle manager. |
| Script kiểm tra dùng một lần (throwaway) | viết lại mỗi lần chạy, tốn token, không thể tái lập | Xây dựng sẵn một CLI để các agent gọi dùng chung. |
| Agent dùng lệnh `sleep 300` | mất 4 phút chờ vô nghĩa cho các bài test chỉ cần 1 phút là xong | Giám sát các agent đang chạy; routine kiểm tra mỗi 5 phút là để bắt các trường hợp này, không phải để làm báo cáo. |

Tần suất và group chat chính là nơi ngốn token nhiều nhất. Mọi yếu tố khác chỉ là thứ yếu.

---

## Thông lượng (Throughput)

| Chỉ số | Số lượng | Nguồn |
|---|---|---|
| PR của PStack được merge lên production trong một tháng | 2,500 (~83/ngày) | Lauren, ngày 1 |
| Tỷ lệ PR được merge tại công ty thông qua Grok Bot | tỷ lệ phần trăm ở mức hai chữ số | Phiên PM, ngày 1 |
| Bản mobile app đầu tiên, một kỹ sư phát triển | 3 tuần | Ling, ngày 1 |
| Số lượng cloud agent một kỹ sư tự quản lý thủ công trước khi có orchestration | 15 | Ling, ngày 1 |
| CI tự động sửa lỗi: chỉ báo động (page) cho người thật sau | 10 phút không tự giải quyết được | ngày 1 |
| Số PR trong đợt build kéo dài 72 giờ | 433 | ngày 3 |
| Số commit từ một host trong 3 ngày | 157 | Roshan, ngày 3 |
| Nhân viên bot trải khắp 7 mảng kinh doanh | 22 | Jenny Co, ngày 1 |
| Prospect của SDR mỗi ngày | 50, xử lý top 5 mỗi buổi sáng | Simon, ngày 2 |

---

## Ngày launch, góc nhìn về quy mô

Thursday Arena lên live vào khoảng 10:00 AM giờ Pacific chỉ từ một bài post trên X.

| Thời điểm | Số lượng |
|---|---|
| Giờ đầu tiên launch | 1,908 game — khung giờ cao điểm nhất trong ngày |
| Duy trì trong buổi chiều | 400–500 game/giờ |
| Cuối ngày | ~4,000 game, hơn 6,000 trận đấu public, ~30,000 lượt xem trang, ~2,000 người dùng |
| Tỷ lệ chuyển đổi: Chơi thử (practice) → Đăng nhập qua X | ~8% |
| Cơ cấu phản hồi | 71% báo bug, 16% khen ngợi |
| Doanh thu | $0 (một lượt bid thử nghiệm $1 trên hệ thống đấu giá quảng cáo đang lỗi) |

Traffic: chia đều khoảng 50/50 giữa mobile và desktop, 44% đến từ iOS, referral gần như toàn bộ đến từ X. Có 8 lượt truy cập từ Bing.
