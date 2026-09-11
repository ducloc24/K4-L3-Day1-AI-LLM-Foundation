# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature = 0.0 và 0.5 thì có thể thấy câu trả lời khá là nhất quán với nhau. Khi nâng lên 1.0 và 1.5 mô hình mở rộng phạm vi lựa chọn từ vựng ngẫu nhiên hơn, giúp tạo ra ý tưởng mới và ngôn ngữ phong phú hơn

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Đối với chatbot hỗ trợ khách hàng, em sẽ để temperature khoảng từ 0.0 đến 0.3 vì tư vấn cần đảm bảo độ chính xác và độ nhất quán, đảm bảo 10 khách hỏi 10 câu hỏi giống nhau thì câu trả lời đều chính xác ngang nhau. Ngoài ra, để temperature như vậy cũng tối ưu tính an toàn và kiểm soát rủi ro.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> GPT-4o đắt khoảng 16.7 lần GPT-4o-mini. GPT-4o phù hợp với các câu hỏi phức tạp cần chất lượng cao, còn mini phù hợp với các tác vụ đơn giản như FAQ hoặc phân loại câu hỏi để tiết kiệm chi phí.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Persona giáo viên tiểu học sẽ trả lời đơn giản, ngắn gọn và dùng ví dụ dễ hiểu cho trẻ em. Persona chuyên gia tài chính sẽ dùng nhiều thuật ngữ kỹ thuật và giải thích chuyên sâu hơn. System prompt định hướng cách model sử dụng từ vựng, độ dài và ví dụ. Vì vậy, cùng một câu hỏi nhưng system prompt khác nhau sẽ tạo ra cách trả lời khác nhau.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với đoạn khoảng 100 từ, cách ước lượng số token là 100 / 0.75 ≈ 133 token. Số token thực tế từ tiktoken có thể khác do tokenizer không chỉ đếm theo từ. Tiếng Việt thường tốn nhiều token hơn tiếng Anh vì cách mã hóa từ và các ký tự có dấu có thể tạo thành nhiều token hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming phù hợp khi câu trả lời dài vì người dùng có thể nhìn thấy nội dung ngay khi model đang tạo, giúp giảm cảm giác phải chờ đợi. Non-streaming phù hợp với các câu trả lời ngắn hoặc khi cần nhận toàn bộ kết quả rồi mới xử lý tiếp.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
>Exponential backoff giúp các client retry ở những thời điểm khác nhau và giảm áp lực lên API khi hệ thống đang quá tải. Nếu hàng nghìn client cùng retry với delay cố định, chúng có thể retry đồng thời, làm API tiếp tục quá tải.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona của em là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt. Yêu cầu "ngắn gọn" giúp câu trả lời dễ đọc và chỉ định tiếng Việt giúp phản hồi phù hợp với người dùng.

System prompt:
"Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt."

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
>Hạn chế lớn nhất là history chỉ lưu tối đa 3 lượt hội thoại nên trợ lý có thể quên thông tin ở các lượt quá xa. Có thể cải thiện bằng cách lưu lịch sử dài hạn vào database và truy xuất những thông tin liên quan khi người dùng đặt câu hỏi mới.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
