# Roster

Danh mục các vai trò agent được mô tả trong ba ngày diễn ra sự kiện — tổng cộng 69 vai trò, đã được loại bỏ trùng lặp theo *vai trò* chứ không theo tên bot. Steve, Cora, Gus, Craig, Simon-bot và Olive thực chất chỉ là một vai trò duy nhất (chief of staff) nhưng có sáu cái tên khác nhau.

Mỗi file đều gồm các phần chuẩn:

| Mục | Nội dung giải đáp |
|---|---|
| **Xuất hiện trên stream dưới tên** | Những bot cụ thể từng đảm nhận vai trò này và do ai sở hữu |
| **Phụ trách / Không phụ trách** | Phân làn công việc (swim lane). Danh sách *không phụ trách* là phần người ta hay bỏ qua nhất, nhưng đây lại chính là chốt chặn giúp bot không bị loãng phạm vi công việc |
| **Nguồn chân lý (Source of truth)** | Nơi bot truy xuất dữ liệu xác thực. Nếu thông tin không có trong đó, bot sẽ báo lại |
| **Cần duyệt cho** | Các điểm chốt chặn của con người (human gate). Ban đầu nên siết chặt hơn mức này rồi dần nới lỏng |
| **Kích hoạt / Đầu ra** | Điểm bắt đầu quy trình và kết quả trả về |
| **Lịch trình (Routines)** | Nhịp độ thực thi, nếu buổi stream có nêu rõ |
| **Mô tả vai trò** | Nội dung mô tả sẵn sàng để copy kèm `{PLACEHOLDERS}`. Đây chính là "phần hồn" / system prompt của bot |
| **Trích dẫn từ stream** | Tình huống phát sinh hoặc câu trích dẫn nguồn gốc của bot |

## Cách sử dụng một vai trò

1. Sao chép khối mô tả vai trò vào một bot mới. Điền đầy đủ các placeholder.
2. Cắt giảm danh sách *phụ trách* xuống đúng những gì bạn thực sự cần hôm nay. Đừng vội dựng đội ngũ support bốn bot ngay ngày đầu tiên — David cũng không làm thế.
3. Giữ nguyên danh sách *cần duyệt* đúng như bản mô tả cho đến khi bot tạo đủ độ tin cậy.
4. Mỗi khi cần điều chỉnh bot, hãy bổ sung quy tắc tổng quát vào phần mô tả của nó — đừng đưa tình huống vụn vặt vào. Xem thêm tại [`../AGENTS.md`](../AGENTS.vi.md).

Hầu hết các hệ thống được chia sẻ trên stream chỉ gồm khoảng 5–10 bot trong số này. Blake từng nhận xét: "Bạn không cần tới 45 bot đâu."
Simon cũng chia sẻ: "Đã có lúc tôi dùng quá nhiều bot. Kết quả chỉ thêm hỗn loạn."

---

## Điều phối (Orchestration)

| Vai trò | Tóm tắt một dòng |
|---|---|
| [`Bot Factory (meta-bot)`](bot-factory.vi.md) | Khởi tạo các bot khác: soạn mô tả vai trò, đặt tên, review và đánh giá sức khỏe đội ngũ hiện có, đồng thời chẩn đoán điểm nghẽn trong workflow. |
| [`Chief of Staff`](chief-of-staff.vi.md) | Bot duy nhất giao tiếp trực tiếp với con người. Điều hướng mọi yêu cầu đến đúng bot chuyên trách, theo dõi ai đang làm việc gì và là kênh duy nhất được phép ping con người. |
| [`Engineering Manager`](engineering-manager.vi.md) | Tiếp nhận khối lượng công việc lớn, phân rã thành các task có scope rõ ràng, bàn giao cho engineer bot và chạy vòng lặp verify kết quả nhận về. Được định hướng không tự viết code. |
| [`Knowledge Base Manager`](knowledge-base-manager.vi.md) | Lắng nghe thụ động các đoạn chat của những bot khác và chọn lọc ghi lại các thông tin lâu dài vào knowledge base của team — có sự chấp thuận của con người, quản lý như một git log chứ không lưu trữ vô tội vạ. |
| [`Miscellaneous Bot (thùng rác)`](misc-bot.vi.md) | Nơi tiếp nhận mọi yêu cầu vụn vặt ngoài luồng để không làm loãng context của các bot chuyên trách. Có thể chuyển giao kinh nghiệm cho đúng bot khi thông tin đó trở nên quan trọng. |
| [`Playbook Owner (trưởng vận hành)`](playbook-owner.vi.md) | Sở hữu tài liệu sống chứa các tiêu chuẩn của team. Các bot khác chỉ đọc và không được phép chỉnh sửa. Mỗi quy tắc mới chỉ đưa vào một lần duy nhất và được thông báo tới toàn bộ bot. |
| [`Project Manager`](project-manager.vi.md) | Học cách con người điều phối một workflow gồm nhiều bot, sau đó tự vận hành end-to-end độc lập. Trở thành đầu mối liên hệ duy nhất cho workflow đó. |
| [`Self-Improvement Scan (tối ưu hóa bot)`](self-improvement-scan.vi.md) | Rà soát cách con người và các bot thực sự phối hợp trong tuần, đề xuất một giải pháp tự động hóa và nạp độ lệch giữa bản nháp với bản gửi thực tế (draft-vs-sent delta) trở lại cho voice bot. |
| [`Source of Truth`](source-of-truth.vi.md) | Trả lời câu hỏi dựa trên tài liệu chuẩn kèm trích dẫn nguồn. Các bot khác lấy đây làm căn cứ để đưa ra luận điểm; chief of staff sẽ truy vấn bot này khi cần câu trả lời chuẩn xác tuyệt đối. |
| [`Sub-Agent Army (chiến binh)`](sub-agent-army.vi.md) | Một nhóm sub-bot giống hệt nhau với context thấp được bot cha phân bổ công việc xử lý hàng loạt, báo cáo ngược lại cho bot cha trong group chat chung — tuyệt đối không gửi trực tiếp cho con người. |

## Kỹ thuật (Engineering)

| Vai trò | Tóm tắt một dòng |
|---|---|
| [`CI / Alert Auto-Fix (on-call bot)`](ci-autofix.vi.md) | Tuyến ứng phó đầu tiên khi CI báo đỏ, deploy lỗi và có alert. Điều tra nguyên nhân, khởi tạo cloud agent để sửa, merge theo policy và chỉ page con người nếu sự cố chưa được xử lý sau thời gian timeout. |
| [`Comment Cleanup`](comment-cleanup.vi.md) | Xóa bỏ các comment code thừa. Ra đời vì các agent thường mượn comment để biện minh cho giải pháp tạm bợ (workaround) thay vì xử lý tận gốc vấn đề. |
| [`Domain Engineer (UI / DevX / infra / …)`](domain-engineer.vi.md) | Mỗi domain có một engineer bot riêng kèm bộ nhớ riêng; bot này chuyển đổi task có scope rõ ràng thành lượt chạy cloud agent và trả về PR kèm bằng chứng nghiệm thu (proof). |
| [`Feedback → PR`](feedback-to-pr.vi.md) | Tiếp nhận feedback đã xác thực của khách hàng và chuyển thành PR trong vòng vài giờ, sử dụng chính sản phẩm thực tế để lấy context và thực hiện verification. |
| [`Founding Engineer (PR watcher)`](founding-engineer.vi.md) | Engineer đầu tiên trên repo mới: theo dõi mọi PR, merge các PR đã sẵn sàng, khởi tạo cloud agent xử lý từng bug cụ thể và có thể báo cáo tiến độ qua cuộc gọi thoại. |
| [`Kanban / Task-Board Updater`](kanban-updater.vi.md) | Giữ task board luôn chuẩn xác: chuyển card khi PR được merge, tạo card mới từ các quyết định và giúp bot khác nhận việc bằng cách theo dõi board. |
| [`Nightly Audit Engineer`](nightly-audit-engineer.vi.md) | Chạy cloud agent nghiên cứu trên toàn bộ codebase mỗi đêm, tìm ra mã kém chất lượng, khoảng trống module hóa, comment dư thừa cùng lỗ hổng bảo mật, và để sẵn các PR vào sáng hôm sau. |
| [`Playtester / QA`](playtester.vi.md) | Trực tiếp sử dụng sản phẩm — click trải nghiệm, dùng thử, cố tình làm crash — ngay trên máy tính riêng, sau đó báo cáo lỗi phát sinh trước khi merge PR hoặc sau khi deploy. |
| [`PR Reviewer`](pr-reviewer.vi.md) | Review từng PR về tính đúng đắn, rủi ro và thiếu sót test, kiểm tra xem đã đính kèm bằng chứng nghiệm thu yêu cầu chưa, sau đó tự động merge hoặc gửi trả lại. |
| [`Prototyper`](prototyper.vi.md) | Xây dựng nhanh các nguyên mẫu dùng một lần — inline HTML ngay trong chat hoặc dùng cloud agent trên branch nháp — nhằm giải đáp bài toán thiết kế chứ không dùng để phát hành thực tế. |
| [`Slack Mention Responder`](slack-mention-responder.vi.md) | Lắng nghe các lượt @-mention cùng DM trên Slack để xử lý — hoạt động như một internal tool gọn nhẹ không cần dashboard, hoặc đóng vai trò lớp triage lọc bớt việc cho con người. |
| [`Triage Validator`](triage-validator.vi.md) | Kiểm tra xem triage bot hiểu đúng ý kiến phản hồi hay chưa trước khi bất kỳ quy trình sửa lỗi tự động nào bắt đầu. Đóng vai trò lớp kiểm duyệt thứ hai giữa người dùng và code. |
| [`Triage`](triage.vi.md) | Đọc feedback gửi đến, tái hiện issue được báo cáo và tạo ticket đã xác thực — hoặc loại bỏ nếu không hợp lệ. Đồng thời giám sát nguy cơ prompt injection trong feedback. |

## Sản phẩm & thiết kế (Product & design)

| Vai trò | Tóm tắt một dòng |
|---|---|
| [`Creative Director / Media Explorer`](creative-director.vi.md) | Khám phá các định hướng sáng tạo — âm nhạc, chuyển động, tài nguyên quảng cáo — dưới dạng code khi có thể, và gửi các phương án đề xuất lên không gian chung để team đánh giá (Notion, playground). |
| [`Critic`](critic.vi.md) | Đánh giá sản phẩm dựa trên tiêu chí chấm điểm (rubric) và chỉ ra điểm chưa ổn bằng văn phong rõ ràng, dễ hiểu trước khi ship. Chỉ đưa ra feedback; tuyệt đối không tự sửa sản phẩm. |
| [`Data Scientist / Analyst`](data-scientist.vi.md) | Giải đáp các câu hỏi về dữ liệu bằng văn phong trực quan bằng cách viết và chạy SQL trên data warehouse, trả về số liệu cùng biểu đồ, đồng thời đính chính khi con người hiểu sai biểu đồ. |
| [`Designer`](designer.vi.md) | Tạo mockup chuẩn nhận diện thương hiệu cực nhanh nhờ nắm rõ design system, tài liệu tham chiếu và danh sách các điều cấm kỵ được team đúc kết. Cung cấp nhiều phương án lựa chọn thay vì chỉ đưa ra một kết quả duy nhất. |
| [`Prioritizer`](prioritizer.vi.md) | Duy trì danh sách công việc tiếp theo được sắp xếp theo thứ tự ưu tiên (stack-ranked), tính điểm theo tác động (impact) và công sức bỏ ra (effort), đồng thời xếp hạng lại khi có ý tưởng hoặc feedback mới. |
| [`Product Changes Tracker`](product-changes-tracker.vi.md) | Cho biết tính năng nào đã ship, tính năng nào bị hủy bỏ và những quyết định ngầm nào đã được đưa ra bằng cách đọc PR, issue *kết hợp* tự trải nghiệm sản phẩm trực tiếp trên máy riêng. |
| [`Recruiter`](recruiter.vi.md) | Tìm nguồn ứng viên, quản lý hiring pipeline và soạn thảo nội dung tiếp cận (outreach) — hoặc ngược lại, tìm kiếm recruiter cùng các đầu mối liên hệ tiềm năng cho người đang tìm việc. |
| [`Spec / PRD Writer`](spec-writer.vi.md) | Chuyển hóa insight cùng context sản phẩm thành bản spec P0/P1/P2 sắc bén được tối ưu để tiến hành code nhanh nhất, sau đó lặp lại điều chỉnh dựa trên comment trong tài liệu. |

## Bán hàng & kỹ thuật bán hàng (Sales & sales engineering)

| Vai trò | Tóm tắt một dòng |
|---|---|
| [`Account Specialist (mỗi account một bot)`](account-specialist.vi.md) | Bot chuyên trách nắm trọn vẹn context của một account: kế hoạch, các bên liên quan, gia hạn, tín hiệu thị trường, cam kết. Cập nhật account plan sau mỗi cuộc gọi và đánh dấu những việc cần bạn xử lý. |
| [`Battle Card Writer`](battle-card-writer.md) | Kết hợp kết quả phân tích thực tế từ competitor bot với thông tin codebase chuẩn xác từ technical expert để tạo ra các battle card ngắn gọn, sẵn sàng cho SE: tuyên bố của đối thủ vs. thực tế của chúng ta. |
| [`Call Prep / Close`](call-prep.vi.md) | Trước cuộc gọi 15–20 phút (hoặc từ tối hôm trước), bot gửi cho bạn danh sách người tham gia, diễn biến lần trước, mức độ sử dụng, các cập nhật vừa ship, đề xuất hành động và mọi bug trên trang của họ để mở đầu câu chuyện. |
| [`Case-Study / Slides Curator`](case-study-curator.vi.md) | Biến bài blog của khách hàng hoặc ghi chú cuộc gọi thành slide theo template cố định, tải đúng logo, chèn vào master deck và ẩn đi những thông tin không liên quan cho cuộc gọi tiếp theo. |
| [`Competitive Intel`](competitive-intel.vi.md) | Tự đăng ký và dùng thử sản phẩm của đối thủ trên máy tính riêng, đọc changelog, blog, bài đăng trên X và tin tuyển dụng của họ, sau đó báo cáo điểm khác biệt cùng các động thái bạn cần phản hồi. |
| [`CRM Updater (các bước tiếp theo)`](crm-updater.vi.md) | Lắng nghe cuộc gọi, đọc thread thảo luận và soạn nội dung cập nhật các bước tiếp theo chuẩn theo định dạng của bạn để bạn duyệt và lưu — đồng thời phản ứng kịp thời khi có thay đổi stage. |
| [`Demo Scripter / Talk Track`](demo-scripter.vi.md) | Xây dựng kịch bản demo chuẩn xác tuyệt đối (không bịa đặt) và bài nói cho cuộc gọi, giúp liên kết nỗi đau cụ thể của khách hàng với luồng sản phẩm thực tế, dựa trên thông tin gốc từ technical expert. |
| [`Enrichment & Company Research`](enrichment.vi.md) | Biến một cái tên thành email đã xác thực, biến một công ty thành tech stack, tin tuyển dụng và sơ đồ tổ chức — giúp hệ thống sequencer không bị bounce mail và thông điệp đến đúng người nhận. |
| [`ICP Researcher`](icp-researcher.vi.md) | Xác định chân dung người thực sự mua hàng — từ các deal thành công, VoC và dữ liệu sản phẩm — đúc kết thành các phân khúc và persona, đồng thời lưu giữ dưới dạng một skill vì dữ liệu này sẽ liên tục thay đổi. |
| [`Live Deck Curator`](live-deck-curator.vi.md) | Sau (hoặc trong) cuộc gọi discovery, bot lấy transcript và cập nhật deck theo đúng các use case cùng bước tiếp theo mà khách hàng vừa đề cập. |
| [`Prospector (outbound)`](prospector.vi.md) | Chọn lọc account và liên hệ, tìm kiếm điểm móc nối cá nhân hóa (các bài đăng trên X, podcast, webinar — xem kỹ chứ không chỉ đọc lướt), xếp hạng người cần tiếp cận và soạn thảo thông điệp bằng chính giọng văn của bạn. |
| [`Signal Scanner (web research)`](signal-scanner.vi.md) | Quét môi trường bên ngoài để tìm kiếm biến động trên toàn bộ danh sách account — gọi vốn, tuyển dụng, tin tức, bài đăng blog — mỗi ngày ở quy mô lớn bằng cách phân tán cho các sub-agent. |
| [`Technical Expert (dựa trên repo)`](technical-expert.vi.md) | Giải đáp câu hỏi "sản phẩm thực tế xử lý X như thế nào" trực tiếp từ codebase qua cloud agent và diễn đạt lại cho khách hàng — tuyệt đối không làm lộ tài sản trí tuệ (IP). |
| [`Usage Signals (PLG)`](usage-signals.vi.md) | Phân tích dữ liệu sử dụng sản phẩm để nhận diện ai vừa đăng ký, ai là power user, team nào đang áp dụng tính năng gì và account nào đang có độ tương tác cao. |
| [`Voice of Customer`](voice-of-customer.vi.md) | Lưu trữ lý do vì sao thắng hoặc thua deal — từ bản ghi cuộc gọi và CRM — nhờ đó hoạt động tiếp cận và xếp hạng có thể được tinh chỉnh sát với mối bận tâm thực tế của nhóm khách hàng tương tự. |
| [`Voice (viết theo giọng văn của bạn)`](voice.vi.md) | Học văn phong thực tế của bạn từ những nội dung bạn từng gửi — được lọc và gán trọng số cho các nội dung gần đây và hiệu quả — sau đó soạn thảo mọi văn bản gửi ra ngoài theo đúng giọng văn đó cho từng đối tượng người nhận. |

## Sau bán hàng & vận hành cá nhân (Post-sales & personal ops)

| Vai trò | Tóm tắt một dòng |
|---|---|
| [`Commitment Tracker (theo dõi lời hứa & yêu cầu)`](commitment-tracker.vi.md) | Quản lý hai danh sách: những gì bạn đã hứa sẽ làm, và những gì bạn đã nhờ người khác. Nhắc bạn danh sách đầu và đôn đốc danh sách sau, đảm bảo không có việc gì bị chìm nghỉm trong email và Slack. |
| [`Daily Digest`](daily-digest.vi.md) | Đọc newsletter, podcast và feed bạn đã đăng ký nhưng không có thời gian theo dõi, sau đó gửi một bản tóm tắt duy nhất vào mỗi buổi sáng tại nơi bạn dễ thấy nhất. |
| [`Follow-Up Desk`](follow-up-desk.vi.md) | Ngay khi cuộc gọi kết thúc: đọc transcript, soạn email phản hồi, nhắn Slack cho AE và chuẩn bị mọi thứ đã hứa trong cuộc gọi — dưới dạng bản nháp, viết theo đúng giọng văn của bạn. |
| [`Inbox Manager`](inbox-manager.vi.md) | Xếp hạng email, tin nhắn Slack và lời mời họp nhận được qua đêm theo thứ tự ưu tiên xử lý vào mỗi sáng, soạn sẵn phản hồi cho các việc thường nhật và chỉ leo thang những việc thực sự quan trọng. |
| [`Internal Radar`](internal-radar.vi.md) | Theo dõi 30–40 kênh nội bộ và email cập nhật mà bạn không xuể đọc, sau đó gửi một bản digest hàng ngày về các điểm mới và thông tin bạn cần nắm, kèm đường link chi tiết. |
| [`Meeting Attendee / Note Taker`](meeting-attendee.vi.md) | Thay mặt bạn tham gia cuộc gọi (tắt tiếng, tắt camera, tự giới thiệu), gửi lại các điểm cốt lõi và quyết định sau đó, đồng thời điều hướng các action item đến đúng bot phụ trách. |

## Chăm sóc khách hàng (Customer support)

| Vai trò | Tóm tắt một dòng |
|---|---|
| [`Support Alert`](support-alert.vi.md) | Được bot phản hồi ping (hoặc tự quét mỗi giờ) khi ticket khớp với quy tắc leo thang; gửi thông báo vào kênh Slack chung và tag con người vào xử lý. |
| [`Support Infra (build)`](support-infra.vi.md) | Thiết lập hệ thống support: cài đặt connector, tạo bảng eval và trace, kết nối knowledge base và dùng cloud agent để tự dựng connector còn thiếu khi chưa có sẵn. |
| [`Support Reply`](support-reply.vi.md) | Xử lý ticket theo quy trình chuẩn — đọc, tra cứu, quyết định trả lời hay bàn giao, hành động, để lại ghi chú — kèm cơ chế kiểm soát độ tin cậy, đồng thời trả lời câu hỏi nội bộ từ cùng một knowledge base. |
| [`Support Tuner (tự cải tiến)`](support-tuner.vi.md) | Đề xuất bổ sung knowledge base khi reply bot không trả lời được, rà soát lại ticket và trace tuần trước để tìm điểm cần cải thiện, sau đó tiến hành cập nhật khi được duyệt. |

## Marketing & tăng trưởng (Marketing & growth)

| Vai trò | Tóm tắt một dòng |
|---|---|
| [`Growth Ideas Logger`](growth-ideas-logger.vi.md) | Ghi nhận các ý tưởng tăng trưởng ngay khi vừa được nói ra, lưu vào tài liệu growth playbook và sắp xếp thứ tự ưu tiên theo tác động và công sức thực hiện. |
| [`Market Researcher`](market-researcher.vi.md) | Nghiên cứu sản phẩm cùng thị trường của bạn, phân tích landing page của đối thủ và chỉ ra các khoảng trống định vị cũng như cơ hội — bot đầu tiên trong chuỗi chiến dịch. |
| [`Marketing Analyst`](marketing-analyst.vi.md) | Trích xuất kết quả thử nghiệm gần nhất từ nền tảng quảng cáo, chỉ ra phương án chiến thắng cùng các chỉ số then chốt, và đề xuất cách cập nhật chiến lược cũng như tài nguyên sáng tạo. |
| [`Performance Marketer`](performance-marketer.vi.md) | Dựng khung chiến dịch trên nền tảng quảng cáo, phân bổ các biến thể nội dung vào đó và gửi ảnh chụp màn hình trong quá trình thao tác — dừng lại trước bước chi tiền nếu chưa có bạn phê duyệt. |
| [`Product Marketer`](product-marketer.vi.md) | Tiếp nhận kết quả nghiên cứu và viết bản tóm tắt định vị (positioning brief) — thông điệp ngắn, đóng gói, tuyên bố giá trị — sau đó lên dàn ý landing page và các biến thể ad copy, liên tục điều chỉnh từ comment trong tài liệu. |
| [`Website Ops`](website-ops.vi.md) | Tiếp nhận dàn ý landing page đã duyệt và triển khai thực tế: tạo PR trên repo trang marketing, gửi ảnh chụp tiến độ và push lên môi trường production. |

## Vận hành, sự kiện & tài chính (Operations, events & finance)

| Vai trò | Tóm tắt một dòng |
|---|---|
| [`Bookkeeper / CFO`](bookkeeper.vi.md) | Theo dõi hóa đơn và chi phí, duy trì sổ sách kế toán cập nhật và cảnh báo ngân sách — hoàn toàn không có quyền chuyển tiền. |
| [`Contract / Policy Reviewer`](contract-reviewer.vi.md) | Review sơ bộ hợp đồng thuê địa điểm hoặc chính sách: gắn cờ các điều khoản bất thường, thiếu sót về mặt bảo vệ quyền lợi và mức giá chênh lệch so với thị trường — định dạng rõ ràng là bản nháp để chuyên gia con người hoàn thiện. |
| [`Event Planner`](event-planner.vi.md) | Quản lý ngân sách tổ chức cho sự kiện — địa điểm, F&B, nhân sự, AV, với marketing là một dòng chi phí riêng — và gửi thông số yêu cầu cho các sub-bot (địa điểm, giấy phép, hợp đồng). |
| [`Negotiator / Reseller`](negotiator.vi.md) | Đăng danh sách sản phẩm hoặc yêu cầu báo giá, sau đó đàm phán với đối tác trong khuôn khổ bạn thiết lập — mức giá sàn, quy tắc dừng đàm phán — và báo cáo lại khi có điểm vượt khung. |
| [`Permit / Red-Tape Researcher`](permit-researcher.vi.md) | Nghiên cứu giấy phép, chứng nhận và quy định cho từng loại sự kiện tại khu vực pháp lý cụ thể, sau đó lập checklist kèm thời gian chuẩn bị cần thiết. |
| [`Venue Scout`](venue-scout.vi.md) | Tìm kiếm địa điểm đáp ứng tiêu chí sự kiện, gửi RFP qua email và thương lượng trong phạm vi ngân sách bạn đã đặt ra. |

---

## Những vai trò xuất hiện trên stream nhưng *không* có ở đây

- **Grind / Cheater** — các bot đăng nhập và chơi game để cày thứ hạng trên bảng xếp hạng cho người dẫn chương trình. Đây chỉ là trò đùa; bot của Lauren thậm chí còn làm *tụt* hạng.
- **Personal tracker** (bot theo dõi đơn hàng, theo dõi hàng về, theo dõi chương trình truyền hình của Karen Cheng; bot tối ưu gói cước PG&E của Matthew Berman) — đây là tự động hóa phục vụ đời sống cá nhân, không phải vai trò trong team. Mô hình chung là kết hợp giữa `daily-digest` + `signal-scanner` áp dụng cho chính cuộc sống của bạn.
- **Voice agent của xAI** trên đường dây tiếp nhận phản hồi qua điện thoại — đây là tính năng sản phẩm, không được xem là bot theo định nghĩa ở đây.

## Lưu ý

Tên gọi, trích dẫn và nhịp độ thực thi được giữ đúng như trên stream. Các account demo (Harbor, Northwind, Brightline, Flylo, XAir) đều là giả định. Các bản mô tả được đúc kết từ những gì diễn giả chia sẻ về chức năng bot của họ; không có bản nào được công bố nguyên văn từng chữ, vì vậy hãy coi mỗi khối mô tả là một bản dựng lại để bạn tùy chỉnh, chứ không phải template hoàn chỉnh đã được kiểm thử sẵn.
