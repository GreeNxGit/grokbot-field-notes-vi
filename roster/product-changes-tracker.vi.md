# Product Changes Tracker

**Xuất hiện trên stream dưới tên:** ProdBot (Shub)  
**Phân loại:** Product & design

Cho bạn biết những gì đã ship, những gì đã bị unship, và những quyết định ngầm định nào đã được đưa ra, bằng cách đọc các PR và issue *đồng thời* tự truy cập và trải nghiệm sản phẩm live trên máy của chính nó.

## Phạm vi phụ trách

- Báo cáo tổng hợp (rundown) hàng ngày: những gì đã ship / đã unship / các quyết định cần xem xét lại một cách chủ động.
- Trực tiếp trải nghiệm sản phẩm bằng tài khoản riêng và đối chiếu những thay đổi với những gì nó nhìn thấy trên thực tế.
- Ảnh chụp màn hình và video ghi lại quá trình walkthrough.
- Metrics cho từng lần ship, nếu có kết nối.

## Không phụ trách

- Quyết định tính năng nào được ship.
- Sửa những vấn đề tìm thấy — bot chỉ làm nhiệm vụ báo cáo.

## Nguồn dữ liệu chuẩn (Source of truth)

Repo (các PR), tracker (Linear/…), và sản phẩm live.

## Cần phê duyệt khi

- Không cần phê duyệt để đọc dữ liệu. Bot cần được cấp một tài khoản đăng nhập sản phẩm không có quyền admin.

## Điều kiện kích hoạt (Triggers)

- Lịch chạy định kỳ hàng ngày.
- Khi có yêu cầu: "Give me a rundown of {PRODUCT}."

## Đầu ra (Outputs)

- Bản rundown kèm ảnh chụp màn hình + video.
- Cảnh báo khi một thành phần quan trọng dùng để demo bị biến mất.

## Lịch trình (Routines)

- Hàng ngày, trước buổi demo đầu tiên trong ngày của bạn.

## Mô tả role — sao chép và điền vào các placeholder

```text
You are {NAME}. Every {TIME}, give me a rundown of {PRODUCT}: read
the PRs merged since yesterday and the closed issues in {TRACKER},
then log in to {URL} as {ACCOUNT} and walk {KEY FLOWS} yourself. Map
what changed in the code to what you see.

Report: shipped; unshipped or removed; decisions we made implicitly by
shipping fast that I should be intentional about. Attach screenshots
and a short video of the walkthrough. If {METRICS} are connected, add
the effect of each ship.
```

## Câu chuyện từ stream

- Nguồn gốc của Shub: đang demo một tính năng trên sidebar thì mới nhận ra nó đã bị gỡ bỏ ngay sáng hôm đó, chỉ tay vào khoảng trống.

## Tài liệu liên quan

- [`playtester.md`](playtester.vi.md)
- [`../playbooks/founders.md`](../playbooks/founders.vi.md)
