### Role cụ thể trong nhóm

* AI Engine & Heuristics.

Phần phụ trách cụ thể

* Xây dựng packages/ai-core: Đóng góp xây dựng chuỗi 9 AI Agents. Xây dựng lý việc chuyển ngữ cảnh giữa các bước, đảm bảo đầu ra của agent này (như Security Agent) là đầu vào chính xác cho agent tiếp theo.
* Tích hợp AI Provider: Xây dựng module để kết nối và làm việc với các LLM như Gemini hoặc OpenAI. xử lý các tác vụ như quản lý API keys
* Prompt Engineering & Heuristics: thiết kế, tinh chỉnh các system prompt cho từng agent (Planner, Risk Reviewer, Context...). Kết hợp LLM và cá Heuristic trả về (JSON format

SPEC mạnh nhất / yếu nhất

* Mạnh nhất: Prompt Engineering Structured Output. Sử dụng các kỹ thuật Few-shot prompting và định nghĩa JSON schema, nên đảm bảo LLM luôn nhả ra đúng format hệ thống cần và thực hiện kết hợp với Heuristics (ví dụ: tự động đánh cờ đỏ nếu file diff thuộc thư mục auth/ mà không cần gọi AI) giúp tiết kiệm token và tăng độ chính xác.
* Yếu nhất: Xử lý Context Window . Khi đối mặt với những "Monster PR" (thay đổi hàng trăm file code), ai-core đôi khi gặp khó khăn trong việc chunking phần diff. Nếu đưa vào quá nhiều vào prompt, LLM sẽ bị tràn context window dẫn đến nhiều rủi ro.

Đóng góp khác (ngoài coding chính)

*  Đóng góp đưa ý kiến về quyết định mỗi Agent cần được cung cấp những dữ liệu gì từ GitHub (chỉ cần diff, hay cần cả lịch sử commit?) để ra được quyết định tốt nhất mà không gây nhiễu.
* Test rất nhiều trường hợp prompt

Điều học được hoàn toàn mới

* Hiểu về thiết kế luồng Agentic Workflow và các phối hợp team trong quá trình Pull Request. -> Hiểu hơn về vận hành github trong 1 team
* Phải luôn code các lớp fallback vì không có gì đảm bảo AI sẽ luôn trả về kết quả giống nhau ở các lần chạy khác nhau.

Nếu làm lại, sẽ đổi gì?

* Thử nghiệm tích hợp GraphRAG thẳng vào ai-core. Thay vì cung cấp dạng text thuần túy cho Context Agent, việc sử dụng các graph db như Neo4j để lưu trữ mối quan hệ giữa các module trong source code giúp model biết được file session.ts này tác động đến bao nhiêu hàm khác. -> Có thể khả thi và output có thể tốt hơn.

AI giúp gì? Sai ở đâu?

* AI giúp: Hỗ trợ phân tích hàng loạt các file diff mẫu từ open-source để tìm ra các "pattern lỗi" phổ biến, tổng kết thành các heuristics đưa vào system prompt. 
*  debug các chuỗi JSON phức tạp bị lỗi syntax.
* AI sai: Thường viêt quá dài dòng, lan man xử lý một task lỗi. Đưa ra các response thường là bỏ file ,..