# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi tăng temperature, câu trả lời có xu hướng đa dạng và khó dự đoán hơn. Temperature thấp phù hợp với tác vụ cần tính nhất quán và chính xác; temperature cao phù hợp với tác vụ sáng tạo nhưng có thể làm tăng nguy cơ thông tin thiếu chính xác. Trong thử nghiệm này, các mức temperature thấp chủ yếu trả lời về hang Sơn Đoòng, còn mức 1.5 chuyển sang chủ đề cà phê Việt Nam.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt từ 0.2 đến 0.5 để câu trả lời chính xác, nhất quán. Đồng thời không đặt 0.0 để chatbot trả lời tự nhiên hơn.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> GPT-4o tốn khoảng $105/ngày còn GPT-4o-mini tốn khoảng $6/ngày. GPT-4o xứng đáng cho các trường hợp phức tạp như phân tích khiếu nại nghiêm trọng hoặc tư vấn cần suy luận tốt; GPT-4o-mini phù hợp với câu hỏi thường gặp, tra cứu trạng thái đơn hàng và các yêu cầu đơn giản có số lượng lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Phản hồi theo vai trò giáo viên tiểu học thường ngắn, dùng từ vựng đơn giản và ví dụ gần gũi như một cuốn sổ được nhiều người cùng lưu giữ. Phản hồi của chuyên gia tài chính thường dài và chuyên sâu hơn, sử dụng các thuật ngữ như sổ cái phân tán, hàm băm, cơ chế đồng thuận và hợp đồng thông minh. System prompt định hướng vai trò, giọng điệu, mức độ chi tiết và cách lựa chọn từ ngữ của mô hình. Vì vậy, cùng một câu hỏi nhưng mô hình có thể điều chỉnh câu trả lời cho phù hợp với kiến thức và nhu cầu của từng đối tượng.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Hai kết quả chênh lệch khoảng 5,33 / 148 × 100 ≈ 3,6%. Ước lượng theo số từ khá gần trong ví dụ này, nhưng không luôn chính xác. Tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài vì tiếng Việt có dấu, nhiều ký tự Unicode và cách tách âm tiết bằng khoảng trắng; bộ mã hóa thường phải chia các từ hoặc ký tự này thành nhiều token nhỏ hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất với ứng dụng hội thoại hoặc tác vụ sinh văn bản dài, vì người dùng thấy nội dung xuất hiện ngay và cảm nhận thời gian chờ ngắn hơn. Non-streaming phù hợp hơn khi hệ thống cần nhận toàn bộ câu trả lời trước để kiểm tra an toàn, xác thực định dạng JSON, lưu vào cơ sở dữ liệu hoặc xử lý tiếp trước khi hiển thị.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Nếu hàng nghìn client đều retry với delay cố định 1 giây, chúng thường sẽ đồng bộ lại và cùng gửi request sau mỗi giây khiến cho server nhận hàng nghìn request mỗi giây và server luôn trong trạng thái quá tải. Exponential backoff giúp lượng request giãn ra theo thời gian từ đó giúp cho server có cơ hội được phục hồi.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Chuyên gia tư vấn chính xác, thực tế và nhất quán. Ví dụ: Bạn là AI Engineer cấp cao, chuyên thiết kế, xây dựng và vận hành các hệ thống AI/ML/LLM trong môi trường thực tế. Mục tiêu chính: Đưa ra giải pháp kỹ thuật chính xác, khả thi và phù hợp yêu cầu. Ưu tiên tính đơn giản, độ tin cậy, khả năng mở rộng và chi phí vận hành. Trả lời bằng tiếng Việt; giữ nguyên thuật ngữ kỹ thuật tiếng Anh khi cần. Nguyên tắc làm việc: Trước khi đề xuất, xác định rõ bài toán, dữ liệu đầu vào/đầu ra, ràng buộc, tiêu chí đánh giá và môi trường triển khai. Không bịa thông tin, API, kết quả benchmark hoặc tài liệu tham khảo. Khi thiếu thông tin quan trọng, nêu giả định rõ ràng hoặc đặt câu hỏi ngắn gọn. Phân biệt giữa giải pháp thử nghiệm (prototype) và giải pháp production. Khi đề xuất kiến trúc, giải thích ngắn gọn trade-off về độ chính xác, latency, chi phí, bảo mật và khả năng bảo trì. Cung cấp code mẫu sạch, có thể chạy được khi phù hợp; nêu dependencies, cấu hình và cách kiểm thử cơ bản. Với thông tin dễ thay đổi như model, SDK, API, giá dịch vụ hoặc phiên bản thư viện, cần kiểm tra nguồn chính thức trước khi kết luận. Luôn chỉ ra rủi ro: chất lượng dữ liệu, hallucination, bias, privacy, prompt injection, rate limit và quan sát hệ thống (monitoring). Trả lời đúng trọng tâm, có cấu trúc ngắn gọn: 1. Kết luận/khuyến nghị. 2. Cách triển khai. 3. Lưu ý hoặc trade-off quan trọng. Trả lời bằng tiếng Việt; giữ nguyên thuật ngữ kỹ thuật tiếng Anh khi cần”: giúp câu trả lời dễ hiểu với người dùng Việt Nam, nhưng vẫn tránh dịch gượng các thuật ngữ quen thuộc như RAG, fine-tuning, latency, rate limit. Điều này làm nội dung vừa tự nhiên vừa chính xác.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> trợ lý không có bộ nhớ dài hạn đáng tin cậy giữa các phiên làm việc. Vì vậy, nó có thể quên sở thích người dùng, quyết định kỹ thuật trước đó, hoặc bối cảnh dự án đã trao đổi. Cải thiện đề xuất: thêm long-term memory có kiểm soát. Cách triển khai ngắn gọn: Sau mỗi cuộc hội thoại, trích xuất các thông tin bền vững như ngôn ngữ ưu tiên, stack kỹ thuật, quy ước code, quyết định kiến trúc và yêu cầu dự án. Lưu chúng vào cơ sở dữ liệu theo user_id hoặc project_id, kèm thời gian cập nhật và nguồn hội thoại. Khi có câu hỏi mới, truy xuất các memory liên quan bằng semantic search rồi đưa vào system/context prompt. Cho người dùng xem, sửa hoặc xoá memory để tránh lưu sai hay lưu thông tin nhạy cảm. Đặt thời hạn hết hiệu lực và mức độ ưu tiên; thông tin mới hơn sẽ thay thế thông tin cũ.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
