# Day 17 Submission — KingModerator

**Student:** Nguyễn Đông Hưng - 2A202600392  
**Product idea:** KingModerator là AI support assistant cho cộng đồng học tập, giúp mentor/admin trả lời câu hỏi lặp lại của học viên dựa trên tài liệu khóa học, có trích nguồn và chuyển câu khó cho người phụ trách.

---

## 1. MVP Boundary Sheet

**Riskiest Assumption:**

> Mentor/admin của cộng đồng học tập sẽ thật sự dùng KingModerator để giảm tải câu hỏi lặp lại, và học viên sẽ chấp nhận hỏi AI trước khi tag mentor/admin.

**In-Scope** (tối đa 3):

- [x] **Auto FAQ cho câu hỏi lặp lại**  
  Test giả định: phần lớn câu hỏi trong cộng đồng học tập là câu hỏi lặp lại và có thể trả lời bằng tài liệu sẵn có.

- [x] **AI Q&A theo tài liệu khóa học, có trích nguồn**  
  Test giả định: mentor/admin tin câu trả lời hơn nếu AI bám vào tài liệu chính thức và hiển thị nguồn.

- [x] **Human handoff cho câu hỏi khó hoặc AI không chắc**  
  Test giả định: mentor/admin sẽ yên tâm hơn nếu AI không tự trả lời bừa, mà biết chuyển câu hỏi khó cho người phụ trách.

**Out-of-Scope:**

- **Voice/video answerer cho bài giảng dài** — lý do bỏ: effort cao, chưa chứng minh học viên cần ở MVP.
- **Dashboard phân tích toàn bộ hành vi học tập** — lý do bỏ: cần dữ liệu thật sau khi có người dùng, chưa cần cho test ban đầu.
- **Tích hợp LMS phức tạp** — lý do bỏ: làm scope phình to, MVP nên bắt đầu bằng tài liệu + group/community.
- **Tự động chấm bài tập** — lý do bỏ: rủi ro sai cao, khác pain chính là trả lời câu hỏi lặp lại.
- **Thay thế mentor hoàn toàn** — lý do bỏ: sai positioning; sản phẩm là trợ giảng hỗ trợ, không phải người dạy chính.

**Non-Goals:**

- Không thay thế mentor/admin.
- Không tự động trả lời mọi câu hỏi nếu thiếu dữ liệu.
- Không đưa câu trả lời không có nguồn cho các câu hỏi quan trọng.
- Không làm LMS đầy đủ.
- Không cố giải toàn bộ bài toán học tập online trong MVP đầu tiên.
- Không ép khách hàng vào subscription cố định nếu usage còn thấp; pricing cần phản ánh mức sử dụng thật.

---

## 2. PRD Skeleton

### Problem Statement

> Mentor/admin của cộng đồng học tập online bị quá tải vì phải trả lời lặp lại các câu hỏi giống nhau về deadline, link tài liệu, cách nộp bài, lỗi thường gặp và quy định lớp. Điều này làm mentor mất thời gian cho việc support cơ bản, còn học viên thì phải chờ câu trả lời và dễ bị đứt nhịp học.

### Target User

> Mentor/admin/course creator đang vận hành cộng đồng học tập online trên Facebook Group, Zalo, Discord, Telegram hoặc Notion/Google Drive. Họ có tài liệu khóa học, bài tập, lịch học, rubric và thường xuyên bị học viên hỏi lại các câu giống nhau.

### User Stories

**Story 1:**  
> As a mentor/admin cộng đồng học tập, I want KingModerator tự trả lời các câu hỏi lặp lại của học viên, so that tôi giảm thời gian trả lời thủ công và tập trung vào feedback chất lượng hơn.

**Story 2:**  
> As a học viên trong cộng đồng học tập, I want nhận câu trả lời nhanh dựa trên tài liệu khóa học, so that tôi không phải chờ mentor/admin và không bị đứt nhịp học.

**Story 3:**  
> As a mentor/admin, I want AI chuyển câu hỏi khó hoặc không chắc cho tôi xử lý, so that tôi vẫn giữ quyền kiểm soát chất lượng câu trả lời.

**Story 4:**  
> As a course creator, I want pricing gắn với mức sử dụng thật, so that cộng đồng nhỏ có thể bắt đầu rẻ còn cộng đồng lớn trả nhiều hơn khi nhận nhiều value hơn.

### AI-Specific

**Model Selection:**

- **Model:** LLM mạnh về tiếng Việt, truy xuất tài liệu, tóm tắt và trả lời ngắn gọn theo ngữ cảnh.
- **Lý do chọn:** MVP cần đọc tài liệu khóa học, hiểu câu hỏi của học viên, trả lời có trích nguồn và biết khi nào không nên tự trả lời.
- **Trade-offs chấp nhận:** Câu trả lời có thể chưa hoàn hảo trong giai đoạn đầu; cần mentor/admin duyệt hoặc sửa ở các câu hỏi khó.
- **Trade-offs không chấp nhận:** AI bịa nguồn, trả lời sai deadline/rubric, hoặc tự tin trả lời khi tài liệu không đủ.

**Data Requirements:**

- **Nguồn dữ liệu:** FAQ, slide, handbook, rubric, lịch học, yêu cầu bài tập, quy định lớp, link tài liệu, câu hỏi/câu trả lời cũ đã được mentor xác nhận.
- **Usage data cần tracking:** số câu hỏi AI xử lý, số câu hỏi được trả lời có nguồn, số câu handoff cho mentor, số học viên active, số tài liệu được dùng làm nguồn.
- **Owner:** Mentor/admin hoặc course creator của từng cộng đồng.
- **Update frequency:** Cập nhật mỗi khi khóa học thay đổi tài liệu, deadline, bài tập hoặc rubric.

**Fallback UX:**

- **Chiến lược:** Expectation Management + Human Handoff.
- **Trigger:** Khi AI không tìm thấy nguồn, câu hỏi ngoài phạm vi tài liệu, câu hỏi mơ hồ, câu trả lời có độ tự tin thấp, hoặc có nguy cơ ảnh hưởng đến điểm số/quy định.
- **Hành động:** Hiển thị thông báo: “Mình chưa đủ chắc để trả lời câu này. Mình đã chuyển cho mentor/admin kèm ngữ cảnh.”
- **User options:** Học viên có thể bổ sung thông tin, xem nguồn liên quan, hoặc chờ mentor/admin phản hồi.

### Success Metrics

- **Primary metric:** Tỷ lệ câu hỏi lặp lại được KingModerator xử lý mà mentor/admin không cần trả lời lại.
- **Ngưỡng thành công:** Giảm ít nhất **50–70%** câu hỏi FAQ cần mentor/admin trả lời thủ công trong cộng đồng pilot.
- **Business metric:** Usage per community — số câu hỏi AI xử lý/tháng/cộng đồng và tỷ lệ cộng đồng chấp nhận trả phí theo platform fee + usage fee.
- **Timeframe đo lường:** 30 ngày đầu trong pilot.

### Dependencies & Constraints

- Chất lượng tài liệu khóa học phải đủ rõ và đủ cấu trúc.
- Mentor/admin cần cung cấp FAQ, deadline, rubric và tài liệu chính thức.
- Cần tránh AI bịa câu trả lời hoặc trả lời không có nguồn.
- Cần cơ chế human handoff để giữ trust.
- Cần tracking usage rõ ràng để tính phí theo số câu hỏi AI xử lý, số học viên active hoặc số cộng đồng con.
- Tích hợp vào Facebook/Zalo/Discord có thể bị giới hạn bởi API hoặc policy từng nền tảng.

---

## 3. Hypothesis Table

### Hypothesis 1 — Auto FAQ

> Chúng tôi tin rằng tính năng Auto FAQ sẽ giúp mentor/admin giảm tải câu hỏi lặp lại trong cộng đồng học tập.

**Riskiest assumption:**  
Phần đủ lớn câu hỏi của học viên là lặp lại và có thể trả lời bằng tài liệu có sẵn.

**Cách test cheapest:**  
Chạy pilot với 1–3 cộng đồng học tập, lấy 50–100 câu hỏi gần nhất, phân loại xem bao nhiêu câu là FAQ lặp lại và thử để KingModerator trả lời bản nháp.

**Chúng tôi sẽ biết mình đúng khi:**  
Ít nhất **50% câu hỏi lặp lại** được AI trả lời đủ tốt để mentor/admin không cần viết lại từ đầu.

---

### Hypothesis 2 — Source citation

> Chúng tôi tin rằng câu trả lời có trích nguồn sẽ khiến mentor/admin tin KingModerator hơn so với chatbot trả lời chung chung.

**Riskiest assumption:**  
Mentor/admin coi trích nguồn là yếu tố quan trọng để tin câu trả lời của AI.

**Cách test cheapest:**  
So sánh phản ứng mentor/admin giữa 2 phiên bản: câu trả lời không có nguồn và câu trả lời có link/đoạn nguồn từ tài liệu khóa học.

**Chúng tôi sẽ biết mình đúng khi:**  
Mentor/admin chọn bản có trích nguồn là bản “có thể dùng trong lớp thật” ở ít nhất **70% case test**.

---

### Hypothesis 3 — Human handoff

> Chúng tôi tin rằng human handoff sẽ làm mentor/admin yên tâm hơn, vì AI không cố trả lời khi không chắc.

**Riskiest assumption:**  
Mentor/admin không muốn AI tự động trả lời mọi thứ; họ cần cơ chế kiểm soát cho câu hỏi khó.

**Cách test cheapest:**  
Cho AI đánh dấu các câu hỏi “không chắc” và chuyển cho mentor/admin duyệt, sau đó đo tỷ lệ mentor đồng ý với quyết định handoff.

**Chúng tôi sẽ biết mình đúng khi:**  
Ít nhất **80% câu handoff** được mentor/admin đánh giá là “nên chuyển cho người phụ trách”.

---

### Hypothesis 4 — Hybrid pricing

> Chúng tôi tin rằng mô hình hybrid pricing sẽ dễ bán hơn subscription cố định, vì cộng đồng nhỏ có thể bắt đầu rẻ còn cộng đồng lớn trả theo mức sử dụng thật.

**Riskiest assumption:**  
Mentor/admin hiểu và chấp nhận pricing theo usage như số câu hỏi AI xử lý hoặc số học viên active.

**Cách test cheapest:**  
Đưa 3 lựa chọn pricing cho 5–10 mentor/course creator: subscription cố định, pay-as-you-go, và hybrid platform fee + usage fee.

**Chúng tôi sẽ biết mình đúng khi:**  
Ít nhất **60% mentor/course creator** chọn hybrid pricing là phương án công bằng và dễ bắt đầu nhất.

---

## 4. PMF Scorecard

**Aha Moment:**

> Mentor/admin thấy KingModerator tự xử lý được một cụm câu hỏi lặp lại trong group, học viên nhận câu trả lời đúng nguồn, và mentor không cần trả lời lại thủ công.

**Actionable Metric:**

> Tỷ lệ câu hỏi FAQ được xử lý thành công mà mentor/admin không cần can thiệp.

**Usage Metric:**

> Số câu hỏi AI xử lý/tháng/cộng đồng và số học viên active sử dụng KingModerator.

**PMF Method:**

> Aha Moment tracking + retention theo cộng đồng pilot.  
> Theo dõi xem sau 7–14 ngày, mentor/admin có tiếp tục thêm tài liệu, chỉnh FAQ và khuyến khích học viên hỏi KingModerator trước không.

**Ngưỡng thành công ban đầu:**

- Ít nhất **50–70% câu hỏi FAQ** được AI xử lý mà mentor không cần trả lời lại.
- **CSAT ≥ 4.2/5** từ học viên/mentor cho câu trả lời.
- Ít nhất **3/5 cộng đồng pilot** muốn tiếp tục dùng sau 30 ngày.
- Ít nhất **1–2 cộng đồng** sẵn sàng trả tiền hoặc cam kết thử gói trả phí.
- Ít nhất **60% mentor/course creator** được hỏi thấy hybrid pricing công bằng hơn subscription cố định.

**Vanity Metrics tôi sẽ không dùng:**

- Số lượt xem landing page.
- Số lượt đăng ký dùng thử nhưng không hỏi câu nào.
- Số câu hỏi AI trả lời tổng cộng mà không đo chất lượng.
- Số lượng tài liệu upload nhưng không có học viên dùng.
- Số lượt demo với mentor nhưng không có pilot thật.

---

## 5. AI Critique Log

**Điểm AI chỉ ra:**

1. **Customer cần hẹp hơn**  
   - **Action:** Accept  
   - **Lý do:** “Cộng đồng học tập” còn rộng. MVP nên tập trung vào mentor/admin của cohort học online có tài liệu và học viên hỏi lặp lại thường xuyên.  
   - **Sửa:** Target user được viết lại thành mentor/admin/course creator vận hành group học tập online có tài liệu, deadline, rubric và bài tập.

2. **MVP có nguy cơ làm quá nhiều**  
   - **Action:** Accept  
   - **Lý do:** Nếu làm cả dashboard, video, LMS, grading thì scope quá lớn.  
   - **Sửa:** MVP chỉ giữ Auto FAQ, Q&A theo tài liệu có trích nguồn, và Human handoff.

3. **Moat chưa phải model, mà là workflow/context**  
   - **Action:** Accept  
   - **Lý do:** OpenAI/Anthropic có thể có model tốt hơn, nên KingModerator phải defend bằng tài liệu riêng, workflow trong cộng đồng và lịch sử Q&A được mentor xác nhận.  
   - **Sửa:** AI-specific và hypothesis nhấn mạnh source citation, human handoff và community-specific context.

4. **Metric phải đo outcome, không đo output**  
   - **Action:** Accept  
   - **Lý do:** “Số câu AI trả lời” không chứng minh value. Cần đo số câu hỏi mentor không phải trả lời lại.  
   - **Sửa:** Primary metric chuyển thành tỷ lệ câu hỏi lặp lại được xử lý mà mentor/admin không cần can thiệp.

5. **Pricing nên phản ánh scale sử dụng**  
   - **Action:** Accept  
   - **Lý do:** Subscription cố định có thể làm cộng đồng nhỏ ngại bắt đầu, trong khi cộng đồng lớn dùng nhiều lại tạo chi phí cao hơn.  
   - **Sửa:** Bổ sung hypothesis hybrid pricing, usage metric và tracking requirement trong PRD.

**Thay đổi lớn nhất giữa version đầu và bản nộp:**

> Ban đầu KingModerator dễ bị hiểu là chatbot chung cho cộng đồng. Bản nộp đã siết lại thành AI support assistant cho cộng đồng học tập có tài liệu riêng, tập trung vào một workflow cụ thể: học viên hỏi câu lặp lại → AI trả lời theo nguồn → câu khó chuyển mentor. Business model cũng được chỉnh thành hybrid pricing: platform fee để mở quyền lợi, usage fee theo mức sử dụng thật.

---

## 6. Self-assessment

Mắt xích nào trong [MVP Boundary / PRD / Hypothesis / PMF] đang yếu nhất?

> PMF, willingness to pay và pricing metric là phần yếu nhất. Team có giả thuyết hợp lý rằng mentor/admin bị quá tải vì câu hỏi lặp lại, nhưng vẫn cần pilot thật để chứng minh họ sẽ dùng KingModerator thường xuyên, sẵn sàng trả tiền và chấp nhận pricing theo usage.

Open questions muốn giải đáp tiếp:

1. Cộng đồng học tập nào nên pilot đầu tiên: bootcamp AI/coding, khóa marketing, khóa tiếng Anh hay cohort creator?
2. Học viên có thật sự hỏi KingModerator trước khi tag mentor/admin không?
3. Mentor/admin cần mức kiểm soát nào để tin AI: duyệt trước mọi câu trả lời, chỉ duyệt câu khó, hay cho AI tự trả lời FAQ?
4. Pricing metric nào dễ bán nhất: số câu hỏi AI xử lý, số học viên active, số tài liệu, hay số cộng đồng con?
5. Kênh tích hợp đầu tiên nên là Facebook Group, Zalo, Discord hay web widget?
