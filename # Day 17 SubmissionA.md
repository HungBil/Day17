# Day 17 Submission

**Student:** Nguyễn Đông Hưng - 2A202600392 
**Date:** 24/4/2026  
**Product idea:** KopyMatch là công cụ giúp nhà đầu tư cá nhân kiểm tra độ tin cậy của trader hoặc tín hiệu trước khi xuống tiền.

---

## 1. MVP Boundary Sheet

**Riskiest Assumption:**
> Người dùng sẽ thật sự thêm bước “kiểm tra độ tin cậy” vào workflow trước khi xuống tiền, thay vì chỉ tiếp tục tin vào ảnh lợi nhuận, cộng đồng hoặc KOL.

**In-Scope** (tối đa 3):
- [ ] **Kiểm tra 1 trader hoặc 1 tín hiệu** — test giả định: người dùng có nhu cầu kiểm tra trước khi xuống tiền
- [ ] **Trả kết luận độ tin cậy sơ bộ** — test giả định: một kết luận ngắn gọn là đủ để tạo giá trị ban đầu
- [ ] **Hiển thị 1–3 lý do chính** — test giả định: người dùng cần giải thích ngắn để tin và hành động

**Out-of-Scope:**
- **So sánh nhiều trader cùng lúc** — lý do bỏ: chưa cần để test pain cốt lõi
- **Dashboard theo dõi dài hạn** — lý do bỏ: không cần cho MVP kiểm tra trước khi xuống tiền
- **Tích hợp sàn giao dịch** — lý do bỏ: làm scope phình to quá sớm
- **Chấm bot / khóa học / sàn giao dịch** — lý do bỏ: mở rộng object quá nhanh
- **API B2B** — lý do bỏ: chưa cần để test giá trị cốt lõi với B2C

**Non-Goals:**
- Không đưa lời khuyên đầu tư
- Không cam kết lợi nhuận
- Không bảo chứng tuyệt đối rằng trader hoặc tín hiệu là an toàn
- Không thay người dùng ra quyết định
- Không giải toàn bộ bài toán trust của thị trường trong MVP đầu tiên

---

## 2. PRD Skeleton

### Problem Statement
> Nhà đầu tư cá nhân mới hoặc bán chuyên trong cộng đồng trading online không có cách đáng tin để biết có nên tin một trader hoặc tín hiệu trước khi xuống tiền, khiến họ dễ ra quyết định mù mờ và chịu rủi ro mất tiền.

### Target User
> Nhà đầu tư cá nhân mới hoặc bán chuyên tại Việt Nam, thường theo dõi trader và tín hiệu trong các cộng đồng trading online, hay ra quyết định dựa trên ảnh lợi nhuận, bảng xếp hạng, KOL hoặc lời khuyên trong nhóm.

### User Stories

**Story 1:**  
> As a nhà đầu tư cá nhân mới hoặc bán chuyên, I want kiểm tra nhanh một trader hoặc tín hiệu trước khi xuống tiền, so that tôi tránh tin nhầm và giảm nguy cơ mất tiền.

**Story 2:**  
> As a nhà đầu tư cá nhân mới hoặc bán chuyên, I want hiểu ngắn gọn vì sao một trader hoặc tín hiệu bị đánh giá đáng nghi hay tạm đáng tin, so that tôi có thể tự cân nhắc rủi ro thay vì chỉ làm theo cộng đồng.

### AI-Specific

**Model Selection:**
- **Model:** Mô hình ngôn ngữ mạnh về phân loại, tóm tắt và giải thích ngắn gọn bằng tiếng Việt
- **Lý do chọn:** MVP cần kết luận độ tin cậy sơ bộ và giải thích 1–3 lý do chính rõ ràng, không cần agent phức tạp hay sinh nội dung dài
- **Trade-offs chấp nhận:** Kết quả chưa hoàn hảo, độ phủ dữ liệu còn hạn chế, ưu tiên tốc độ và tính dễ hiểu
- **Trade-offs không chấp nhận:** Trả kết luận quá chắc chắn khi dữ liệu yếu; nói như lời khuyên đầu tư; không giải thích được vì sao

**Data Requirements:**
- **Nguồn:** Dữ liệu công khai từ cộng đồng và nền tảng trading; dữ liệu mô tả công khai về trader / tín hiệu / claim hiệu suất; nguồn tham chiếu công khai; bộ quy tắc nội bộ về dấu hiệu cảnh báo và lý do chấm điểm
- **Owner:** Team KopyMatch
- **Update frequency:** Định kỳ theo đợt cập nhật dữ liệu công khai và khi có thêm case kiểm tra mới

**Fallback UX:**
- **Chiến lược:** Expectation Management + Graceful Handover
- **Trigger:** Khi AI không đủ dữ liệu, đầu vào mơ hồ, tín hiệu mâu thuẫn, hoặc độ tin cậy thấp
- **Hành động:** Hệ thống hiển thị “Không đủ dữ liệu để đánh giá đáng tin” hoặc “Cần kiểm tra thêm”, kèm 1–3 lý do cụ thể
- **User options:** Người dùng có thể nhập lại tên / link rõ hơn, đổi đối tượng cần kiểm tra, hoặc tự kiểm tra thêm thủ công thay vì dựa hoàn toàn vào kết quả

### Success Metrics
- **Primary metric:** Tỷ lệ người dùng quay lại kiểm tra lần thứ 2 trong vòng 7 ngày
- **Ngưỡng thành công:** Có một tỷ lệ đủ đáng kể người dùng dùng lại tính năng thay vì chỉ thử một lần cho biết
- **Timeframe đo lường:** 7 ngày đầu sau lần kiểm tra đầu tiên

### Dependencies & Constraints
- Chất lượng dữ liệu công khai còn phân mảnh
- Chưa có API dữ liệu cá nhân đầy đủ từ sàn
- Dữ liệu tự khai hoặc ảnh chụp màn hình chỉ là tín hiệu yếu
- Phải tránh để output bị hiểu thành lời khuyên đầu tư

---

## 3. Hypothesis Table

### Hypothesis 1 (cho tính năng In-Scope #1)
> "Chúng tôi tin rằng tính năng kiểm tra nhanh 1 trader hoặc 1 tín hiệu sẽ giúp nhà đầu tư cá nhân mới hoặc bán chuyên tránh quyết định mù mờ trước khi xuống tiền."  
> "Chúng tôi sẽ biết mình đúng khi thấy tỷ lệ người dùng quay lại kiểm tra lần thứ 2 trong vòng 7 ngày đạt ngưỡng đủ mạnh trong giai đoạn test đầu."

Riskiest assumption: Người dùng sẽ coi việc kiểm tra độ tin cậy là một bước đáng thêm vào workflow trước khi xuống tiền.  
Cách test cheapest: Cho người dùng nhập trader / tín hiệu và trả kết luận sơ bộ + lý do chính, rồi đo tỷ lệ quay lại dùng lần 2.

### Hypothesis 2 (nếu có)
> "Chúng tôi tin rằng hiển thị 1–3 lý do chính sẽ giúp người dùng tin vào kết quả hơn và sử dụng nó để tự cân nhắc rủi ro, thay vì chỉ xem điểm số."

Riskiest assumption: Giải thích ngắn gọn là đủ để tạo niềm tin ban đầu, không cần hệ thống phân tích quá sâu ở MVP.  
Cách test cheapest: So sánh phản ứng người dùng giữa bản chỉ có kết luận và bản có thêm lý do chính.

---

## 4. PMF Scorecard

**Aha Moment:**
> Người dùng chủ động quay lại kiểm tra trader hoặc tín hiệu lần thứ hai trước một quyết định đầu tư khác.

**Actionable Metric:**
> Tỷ lệ người dùng kiểm tra ít nhất 2 trader hoặc tín hiệu khác nhau trong vòng 7 ngày, hoặc quay lại dùng tính năng kiểm tra lần thứ 2 trong một phiên mới.

**PMF Method:**
> Aha Moment tracking  
> Ngưỡng thành công: có đủ số người dùng quay lại lần 2 để chứng minh đây không chỉ là hành vi tò mò mà là một bước thật trong workflow ra quyết định.

**Vanity Metrics tôi sẽ không dùng:**
- Số lượt sign up
- Số lượt truy cập landing page
- Số lượt xem kết quả một lần
- Tổng traffic từ cộng đồng hoặc KOL

---

## 5. AI Critique Log

**Điểm AI chỉ ra:**
1. **Customer còn hơi rộng** — Action: Accept — Lý do: đã thu hẹp về nhà đầu tư mới hoặc bán chuyên đang theo dõi trader / tín hiệu trong cộng đồng trading online
2. **Need còn có nguy cơ mang mùi solution** — Action: Partial — Lý do: đã viết lại need theo pain, nhưng vẫn cần giữ một số ngôn ngữ gần sản phẩm để nộp bài rõ hơn
3. **Moat và market sizing cần siết logic hơn** — Action: Accept — Lý do: đã chuyển moat sang cơ chế học sâu workflow và sửa TAM/SAM/SOM theo logic fact → assumption → obtainable target

**Thay đổi lớn nhất giữa Version A và Version B:**
> Hiện chưa có vì đây là bản A cuối buổi. Dự kiến bản B sẽ siết thêm object đầu tiên của MVP, cụ thể hóa fallback UX và làm rõ hơn metric chứng minh hành vi dùng thật thay vì tò mò.

---

## 6. Self-assessment

Mắt xích nào trong [MVP Boundary PRD Hypothesis PMF] bạn đang yếu nhất?  
> PMF và Market Logic đang là phần yếu nhất, vì hiện tại team mới có giả thuyết hợp lý về hành vi quay lại dùng, nhưng chưa có dữ liệu thực để chứng minh rằng người dùng sẽ thật sự thêm bước kiểm tra này vào workflow trước khi xuống tiền.

Open questions bạn muốn giải đáp tiếp:
1. Người dùng sẽ tin dạng bằng chứng nào nhất: điểm số, lý do ngắn, hay nguồn dữ liệu đằng sau?
2. Nên bắt đầu bằng object nào để MVP sắc nhất: trader hay tín hiệu?