# Kỹ thuật (Engineering)

**Phiên chia sẻ:** GrokBot for Engineers — Ngày 1  
**Người trình bày:** Ling Shi, kỹ sư phần mềm tại xAI. Tự tay xây dựng phiên bản đầu tiên của ứng dụng di động GrokBot chỉ trong ba tuần. Trước khi GrokBot ra đời như một orchestration layer, anh từng phải quản lý thủ công 15 Cursor cloud agent.

Tiền đề: agentic coding hiện đã hoạt động rất tốt. Điểm nghẽn giờ đây chuyển sang con người — người phải viết prompt, theo dõi transcript, can thiệp (nudge) khi agent đi chệch hướng, và liên tục giải thích lại cùng một tiêu chuẩn cho từng agent mới. Toàn bộ phiên chia sẻ xoay quanh việc đẩy tầng công việc đó xuống cho chính các bot đảm nhiệm.

---

## Đội ngũ bot (The team)

| Bot | Vai trò | Dữ liệu & ngữ cảnh nắm giữ |
|---|---|---|
| **Craig** (còn gọi là "Ling Xixi") | Chief of staff / Trưởng nhóm kỹ thuật | Nắm ai đang làm việc gì. Điều phối mọi request. Onboard bot mới theo cơ chế agent-to-agent. *Không* nắm chi tiết engineering workflow — đó là việc của Jenny. |
| **Cray** | Kỹ sư UI | Toàn bộ thông tin từng trao đổi về mảng UI, được lưu trữ bền vững (persisted) trong bộ nhớ riêng. |
| **Steve** | Kỹ sư DevX | Được onboard trực tiếp từ template marketplace *Nightly audit engineer*, sau đó đổi tên. Chịu trách nhiệm chạy đợt cleanup hàng đêm. |
| **Hogan1QR** | Kỹ sư Infra | Các chỉ dẫn về hạ tầng, được lưu trữ trong bộ nhớ riêng. |
| **Jenny** | Trưởng bộ phận vận hành (Head of operations) | **Nắm giữ playbook** trên Notion. Các bot kỹ sư khác chỉ được đọc, không có quyền chỉnh sửa. Thông báo mọi thay đổi đến từng bot kỹ sư. |
| **Fleet DB** (database trên Notion, không phải bot) | Sổ theo dõi task (Task ledger) | Quản lý mọi task cùng trạng thái hiện tại. Một bot khi quản lý 20 cloud agent sẽ tra cứu sổ này thay vì ôm toàn bộ ngữ cảnh của 20 agent vào context của mình. |

**Tại sao lại dùng ba bot kỹ sư chạy trên cùng một model thay vì chỉ một bot.** Bất kỳ bot nào trong số này cũng có thể làm được mọi tác vụ. Việc chia tách nhằm quản lý context, không phải vì năng lực: mỗi bot đều có giới hạn context và bộ nhớ riêng. Những gì bạn dặn Hogan sẽ nằm lại ở Hogan và được áp dụng vào lần tiếp theo Hogan nhận task, không cần nạp lại chỉ dẫn từ đầu. Việc bắt một bot đảo qua quá nhiều loại tác vụ sẽ làm tràn context của nó. Và trên thực tế, bạn hầu như không bao giờ nói chuyện trực tiếp với các bot kỹ sư — bạn chỉ làm việc với Craig.

---

## Các workflow thực tế (demo trên stream)

### 1. Unblock đồng nghiệp khi bạn đang ngủ hoặc trên chuyến bay

Bạn là code owner duy nhất của một service. Một đồng nghiệp ping bạn trên Slack.

1. Bot giám sát Slack để bắt các lượt ping tới bạn.
2. Đọc tin nhắn, xác định xem đây có phải là yêu cầu review hay không.
3. Kiểm tra PR dựa trên **tiêu chí review bạn đã thiết lập sẵn** — ví dụ: "bắt buộc phải đính kèm ảnh chụp màn hình", "test không được là test giả lập cho có".
4. Kích hoạt một code-scan / code-research skill, chạy bên trong một cloud agent ngay trong repo.
5. Phản hồi xác nhận lại trên Slack.

Nếu thực sự cần đến bạn, bạn chỉ cần mở điện thoại và trả lời bot: *"trông ổn đấy, approve đi."* Bot sẽ gửi lệnh approve và thông báo cho người gửi. Con người chỉ can thiệp đúng một lần duy nhất, ngay từ điện thoại.

### 2. Bug report trên X → PR

Các báo cáo lỗi về sản phẩm được đăng trên X. Một bot sẽ theo dõi (thông qua luồng đồng bộ từ X về Slack), và với mỗi báo cáo: kiểm tra xem bước tái hiện lỗi (reproducer) có còn xuất hiện trên nhánh `main` hay không; nếu có, bot sẽ tự sửa và tạo PR. Quy trình tương tự cũng áp dụng cho security review và các yêu cầu "tìm reviewer phù hợp". Phương châm là: "Bot làm việc với bot; con người chỉ liên hệ với con người khi thực sự bất khả kháng."

### 3. Dọn dẹp code hàng đêm (Nightly cleanup) — 3 giờ sáng

Được cung cấp dưới dạng template trên marketplace ("Nightly audit engineer").

- Mỗi đêm vào lúc 3 giờ sáng (có chỗ Ling nói 4 giờ sáng — mốc nào cũng được), bot sẽ khởi chạy một research cloud agent để rà soát toàn bộ bề mặt monorepo.
- Bot quét các vấn đề: chất lượng code, những đoạn code cần module hoá nhưng chưa làm, comment cần được rút gọn, các hạng mục audit bảo mật ("những thứ chúng ta bỏ quên nhưng lại tối quan trọng để ngăn kẻ khác đánh cắp dữ liệu").
- Bot tự mở PR. Các cloud agent chỉ được phép merge **khi PR đi kèm bằng chứng kiểm thử end-to-end rõ ràng.** Nếu không, PR sẽ được giữ lại chờ duyệt.
- Bạn thức dậy và đã có sẵn một loạt PR hoàn chỉnh.

Lý do chọn chạy ban đêm: không có ai ship code lúc này nên gần như không có xung đột (conflict), và các thay đổi đều ở mức rủi ro thấp nhằm dọn dẹp phần code dư thừa.

### 4. Công cụ nội bộ qua Slack mention thay vì dựng dashboard

Vấn đề: thêm người dùng vào TestFlight qua email. Cách làm cũ: dựng dashboard, thêm auth, kết nối DB, deploy sau lớp màn hình đăng nhập, phân quyền. Làm bằng agent mất khoảng một đến hai tiếng — khá nhanh, nhưng tại sao phải phức tạp như vậy?

Cách làm mới: bất kỳ ai trong tổ chức chỉ cần `@mention` bot trên Slack kèm địa chỉ email. Một routine (hoặc webhook bắt tín hiệu từ Slack) sẽ tự động thêm họ vào TestFlight. Thiết lập duy nhất cần làm: cấp quyền truy cập an toàn vào TestFlight cho bot, kèm một prompt mô tả việc cần làm.

### 5. Tự động sửa chữa mọi thứ: CI, deploy, alert

- CI báo đỏ → bot phân tích nguyên nhân lỗi → khởi tạo một cloud agent để sửa → dựa vào chỉ dẫn của bạn để quyết định xem có được merge hay không → tiến hành merge.
- Hook tương tự được áp dụng cho lỗi deployment, test chập chờn (flaky test), hay bất kỳ cảnh báo nào có tín hiệu rõ ràng.
- **Kỹ sư on-call chỉ bị page báo thức nếu sự cố chưa được xử lý sau 10 phút.** "Phần lớn trường hợp GrokBot có thể tự xử lý xong gọn gàng trong vòng 10 phút."

### 6. Tự kiểm thử sản phẩm trên máy ảo riêng

Một người dùng báo lỗi không xem được các chuyến bay đã đặt trước đó trên flyloair.com (hãng hàng không giả định dùng để demo). Craig tự mở trang web trên VM của chính nó, click thao tác từng bước và báo cáo lại: trang `/trips` hiện chỉ là trang giữ chỗ (stub) trong khi thanh điều hướng vẫn trỏ link về đó. Lỗi được xác nhận chính xác trước khi bất kỳ dòng code nào bị đụng tới.

### 7. Quy trình leo thang P0, chỉ cần định nghĩa một lần

Liên tục hối thúc agent bằng từ "khẩn cấp" sẽ phản tác dụng: nó sẽ bắt đầu nhảy cóc các bước và đoán mò để xong việc sớm. Thay vào đó, hãy thiết lập một policy cố định cho P0:

> Một routine sẽ kiểm tra các cloud agent định kỳ mỗi 5 phút. Nếu một agent đi chệch hướng — ví dụ đang chạy một lệnh sleep dài như `sleep 300`, xa rời mục tiêu, hoặc xử lý quá dè dặt — hãy ngắt lệnh và can thiệp (nudge) nó ngay lập tức.

Sau khi thiết lập, bạn không bao giờ phải tự tay viết prompt hối thúc cloud agent nữa. Bot sẽ tự động gửi các chỉ dẫn tiếp theo.

### 8. Đồng bộ tiêu chuẩn một lần duy nhất — chuỗi playbook

1. Nói cho **Craig** quy tắc mới.
2. Craig chuyển tiếp cho **Jenny**.
3. Jenny ghi nhận vào playbook trên Notion và **phát thông báo tới từng bot kỹ sư.**

Lần tới khi bạn bổ sung thêm một workflow mới ("phải làm bước này trước khi trigger deploy"), chuỗi hoạt động tương tự sẽ diễn ra. Bạn chỉ cần suy nghĩ về nó đúng một lần. "Cách vận hành này rất giống cơ cấu tổ chức của con người, nhưng được áp dụng ở quy mô agent."

### 9. Luôn có bằng chứng trên mỗi PR

Chỉ cần chỉ đạo Craig một lần, Jenny sẽ đưa ngay vào playbook:

> Mọi PR đều bắt buộc phải đi kèm bằng chứng xác thực — ảnh chụp màn hình cho các thay đổi UI, số liệu perf đối với các cải tiến về hiệu năng.

Ling không còn phải ngồi canh chừng các cloud agent nữa. Anh chỉ kiểm tra kết quả và bằng chứng đính kèm. Cloud agent sẽ tự gắn ảnh chụp màn hình (và có thể đính kèm cả video quay lại quá trình chạy) vào phần mô tả PR. Các tính năng bug-bot và security-comment sẽ tự động chạy bổ trợ ngay trên PR đó.

---

## Các prompt mẫu đã sử dụng

Onboard một bot mới thông qua một bot sẵn có:

> Hello, I have a new member in the team called Nightly. Rename them to Steve and tell them how the engineering workflows are enforced.

Craig sau đó sẽ nhắn cho Steve: cách board Notion vận hành, định nghĩa thế nào là code chuẩn sạch (clean), cách công việc được luân chuyển qua từng giai đoạn. Steve xác nhận và nạp vào bộ nhớ. Bạn không cần phải copy-paste bất kỳ thứ gì giữa các bot.

Điều tra nguyên nhân trước khi bắt tay vào sửa:

> I think there is an issue with flyloair.com that users cannot check their previously booked flight. Can you check if it's true?

Định nghĩa quy trình xử lý P0 (gần như nguyên văn):

> You need to set up a routine that checks cloud agents every five minutes. Check if they are off the track, such as running a sleep, running a long sleep like sleep 300, or going off our goal, being too conservative. We sometimes just need things moving a little faster. Interrupt and nudge them at the time you find them going off.

Sau đó, reply lại thông báo lỗi đã được xác nhận (trích dẫn lại tin nhắn để bot nắm đủ ngữ cảnh):

> Fix this issue urgently. Treat it as a P0.

Khởi tạo bot quản lý playbook:

> Onboard a new member called Jenny, head of operations. Jenny needs to manage a playbook for my engineering team, own the playbook, and avoid other engineers updating it.

Bổ sung một tiêu chuẩn:

> Another requirement for the playbook is to make sure every single PR comes with proofs, like screenshots for UI changes and perf metrics for performance improvements.

Dạy bot cách từ chối (diễn giải lại từ buổi chia sẻ): Theo mặc định, các bot sẽ đồng ý tiếp nhận mọi bug report và feature request. Hãy giải thích cho chúng biết *tại sao* một tính năng không phù hợp với sản phẩm, điều gì không nên làm và lý do đằng sau. Thông tin này sẽ được lưu vào bộ nhớ; bạn sẽ không phải lặp đi lặp lại câu: "Tôi không muốn cái này, thế này chưa đủ tinh gọn, chúng ta cần phải đơn giản hóa nó đi."

---

## Các routine và tần suất hoạt động

| Routine | Tần suất |
|---|---|
| Audit / dọn dẹp ban đêm | 3 giờ sáng hàng ngày |
| Kiểm tra cloud agent xử lý P0 | Mỗi 5 phút một lần, trong suốt thời gian P0 đang mở |
| Tự động sửa lỗi CI / deploy | Khi có tín hiệu (CI đỏ, deploy thất bại, alert) |
| Page báo kỹ sư on-call | Chỉ kích hoạt nếu quy trình auto-fix không xử lý được sau 10 phút |
| Tooling kích hoạt qua Slack mention | Theo webhook / tín hiệu từ Slack |
| Lên lịch deploy | Ví dụ "deploy lúc 6 giờ sáng mai" — bot tự ghi nhận thời gian và gọi tool đúng giờ |

---

## Những con số đáng chú ý

- 15 — số lượng cloud agent Ling từng phải quản lý thủ công trước khi có bot điều phối.
- 3 tuần — thời gian hoàn thành ứng dụng di động GrokBot đầu tiên bởi một lập trình viên duy nhất.
- 10 phút — thời gian chờ xử lý tự động trước khi page báo kỹ sư trực vì CI đỏ.
- 5 phút — chu kỳ polling kiểm tra tình trạng xử lý P0.
- 10–20 — ngưỡng số lượng bot mà tại đó việc copy-paste tiêu chuẩn vào từng bot không còn khả năng scale (đây là lý do cần chuỗi playbook).
- 20 — số lượng cloud agent một bot có thể quản lý mà không bị quá tải context, nhờ tận dụng cơ sở dữ liệu fleet DB.

---

## Những khâu vẫn cần đến con người

Được nêu rõ ràng, vì câu hỏi hiển nhiên luôn là "vậy vai trò của con người ở đâu?":

- Định hướng sản phẩm.
- Chi tiết thiết kế UI/UX.
- Những bài toán hóc búa mà bot chưa thể giải quyết trọn vẹn: vấn đề hiệu năng, thiết kế kiến trúc cho phiên bản tiếp theo "để codebase không bị tha hóa chất lượng về lâu dài."
- Huấn luyện bot biết cách từ chối.
- Đảm bảo bot không bao giờ bị nghẽn ở khâu xác thực (auth) — mọi bước auth trong chuỗi quy trình đều cần một luồng mượt mà để "con người unblock cho bot".

---

## Ba bài học cốt lõi, theo lời của Ling

1. **Hãy đối xử với chúng như những bạn thực tập sinh.** Khi bạn gặp khó khăn trong việc giao tiếp với bot, đừng vội vã tìm đến các skill hay câu lệnh gọi hàm phức tạp. Hãy trò chuyện như đang hướng dẫn một thực tập sinh tài năng — người chưa biết rõ phải bắt đầu từ đâu, nhưng sẽ hoàn thành công việc cực kỳ xuất sắc khi được chỉ dẫn đúng cách.
2. **Hạ bớt một tầng trừu tượng thay vì chỉ đi vá lỗi bề mặt (Sink one level further rather than fix the sink).** Nếu bạn nhận ra mình đang gõ cùng một prompt đến lần thứ mười, đó là sự lãng phí. Hãy phân tích xem bản chất công việc thực sự cần gì và đóng gói nó lại — đưa vào một playbook, hoặc giao cho một bot chuyên nhắc nhở các bot khác đúng thời điểm.
3. **Thiết lập một vòng lặp phản hồi (feedback loop) hoàn chỉnh.** Mọi tác vụ kỹ thuật đều cần một tín hiệu rõ ràng để phân định giữa thành công và thất bại, nếu không agent sẽ không thể biết khi nào cần đào sâu tiếp và khi nào nên dừng lại để merge. Vòng lặp đơn giản nhất là cho phép agent trực tiếp thao tác trên website thông qua cơ chế computer use. Phần cứng luôn có những tín hiệu phản hồi đặc thù của riêng nó.

---

## Áp dụng mô hình này ngay

1. Một bot chief-of-staff: Bạn chỉ trao đổi với bot này, không làm việc trực tiếp với các bot kỹ sư bên dưới.
2. Một bot kỹ sư cho từng domain riêng biệt (UI / DevX / Infra): Bộ nhớ của từng bot được chủ động tách biệt hoàn toàn.
3. Một bot vận hành (ops) nắm giữ tài liệu playbook mà các bot khác chỉ có quyền đọc, không có quyền sửa. Mọi tiêu chuẩn mới: Bạn → bot chief → bot ops → phát thông báo toàn hệ thống.
4. Một sổ theo dõi task (task ledger) độc lập, nằm ngoài context của các bot.
5. Định nghĩa quy trình P0 một lần. Định nghĩa yêu cầu về bằng chứng (proof) một lần. Định nghĩa điều kiện merge một lần.
6. Dọn dẹp code định kỳ hàng đêm với cơ chế auto-merge có rào chắn bằng chứng (proof-gated).
7. Gắn mọi tín hiệu cảnh báo bạn có (CI, deploy, alert, Slack mention) ưu tiên qua bot xử lý trước, con người tiếp nhận sau, đi kèm một khoảng thời gian timeout xác định.

Tài liệu liên quan: [`../agents/VERIFICATION.md`](../agents/VERIFICATION.vi.md),
[`../agents/ORCHESTRATION.md`](../agents/ORCHESTRATION.vi.md).
