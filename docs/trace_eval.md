# 📊 BÁO CÁO THU HOẠCH NGHIỆM THU BÀI LAB 3 (BƯỚC 3 — SUBMISSION ARTIFACT)

> **Họ và Tên Học viên:** [Điền Họ và Tên]  
> **Mã Sinh Viên / Mã Học viên:** [Điền MSSV]  
> **Chủ đề Lựa chọn:** [Điền tên chủ đề đã chọn từ docs/DANH_SACH_DE_TAI.md hoặc Đề tài Mở]  

---

## 1. BẢNG CHẤM ĐIỂM AGENTIC FIT SCORING MATRIX (ĐÁNH GIÁ CHỦ ĐỀ)

| Tiêu chí Đánh giá | Mức độ (1 - 5) | Giải trình chi tiết lý do chọn điểm |
| :--- | :---: | :--- |
| **1. Multi-step Reasoning** | 3 / 5 | Hệ thống có thể cần thực hiện nhiều bước: xác định nhu cầu → truy vấn dữ liệu GPA/lịch thi → tổng hợp thông tin → nếu cần thì tìm slot trống của Cố vấn → tạo lịch tư vấn. Tuy nhiên, các tác vụ như tra cứu GPA hoặc lịch thi riêng lẻ khá đơn giản. |
| **2. Tool Interaction** | 5 / 5 | Hệ thống cần kết nối với nhiều nguồn như hệ thống quản lý thông tin sinh viên, hệ thống lịch thi, database học vụ, calendar/booking system và có thể sử dụng MCP/API để thực hiện các thao tác tra cứu và đặt lịch. |
| **3. Dynamic Decision** | 4 / 5 | Bước tiếp theo có thể phụ thuộc vào kết quả trước đó. Ví dụ: nếu sinh viên hỏi "Đặt giúp tôi lịch gặp cố vấn sớm nhất" → Tìm lịch trống của cố vấn -> Khi có lịch trống phải kiểm tra lại lịch sinh viên -> nếu phù hợp thì đặt lịch, nếu không phải tiếp tục kiếm lịch khác. |
| **4. Long Horizon Goal** | 3 / 5 | Một phiên làm việc có thể bao gồm nhiều thao tác liên tiếp, đặc biệt khi mục tiêu là "giúp tôi sắp xếp việc học và đặt lịch với cố vấn". Tuy nhiên, đa số tác vụ vẫn hoàn thành trong vài bước, chưa cần duy trì mục tiêu qua một chuỗi dài state. |
| **TỔNG ĐIỂM AGENTIC FIT** | ** 15 / 20** | *Nếu tổng điểm > 12/20: Bài toán rất phù hợp triển khai Agentic System.* |

---

## 2. TRÍCH XUẤT KẾT QUẢ WATERFALL TRACE LOG (SAU KHI CHẠY TEST SUITE TRÊN API THẬT)

> ⚠️ **YÊU CẦU NGHIỆM THU:** Mở tệp `.env` điền `GEMINI_API_KEY` (hoặc `OPENAI_API_KEY`) để kết nối LLM thật trước khi thực thi `python src/app.py --all`. Bài nộp chỉ dùng Mock Offline Provider sẽ không đạt điểm nghiệm thực tế.

Dán 1 đoạn trích xuất log tiêu biểu từ file `docs/trace_waterfall.json` sinh ra từ phản hồi LLM API thật:

```json
[
  {
    "step": 1,
    "query": "Tôi có mã sinh viên SV2026002 muốn gặp Academic Advisor để được tư vấn về việc đăng ký môn học kỳ tới. Hãy tìm Advisor phụ trách tôi và đặt cho tôi một lịch hẹn vào 20/9/2026 vào lúc 14:00.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "academic_query",
    "arguments": {
      "student_id": "SV2026002"
    },
    "observation": {
      "status": "SUCCESS",
      "student_id": "SV2026002",
      "data": {
        "full_name": "Trần Thị Bình",
        "class": "AI-K4",
        "gpa": 3.6,
        "email": "binh.tt@vinuni.edu.vn",
        "status": "Đang học",
        "advisor": "TS. Lê Thị B"
      }
    },
    "latency_ms": 3315.61
  },
  {
    "step": 3,
    "query": "Tôi có mã sinh viên SV2026002 muốn gặp Academic Advisor để được tư vấn về việc đăng ký môn học kỳ tới. Hãy tìm Advisor phụ trách tôi và đặt cho tôi một lịch hẹn vào 20/9/2026 vào lúc 14:00.\n\nObservation từ Tool academic_query:\n{\"status\": \"SUCCESS\", \"student_id\": \"SV2026002\", \"data\": {\"full_name\": \"Trần Thị Bình\", \"class\": \"AI-K4\", \"gpa\": 3.6, \"email\": \"binh.tt@vinuni.edu.vn\", \"status\": \"Đang học\", \"advisor\": \"TS. Lê Thị B\"}}\n\nHãy tiếp tục xử lý yêu cầu dựa trên Observation.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "schedule_appointment",
    "arguments": {
      "student_id": "SV2026002",
      "advisor_name": "TS. Lê Thị B",
      "datetime_str": "14:00 20/09/2026"
    },
    "observation": {
      "status": "SUCCESS",
      "booking_id": "BK-SV2026002-99",
      "student_id": "SV2026002",
      "datetime": "14:00 20/09/2026",
      "advisor": "TS. Lê Thị B",
      "message": "Đặt lịch thành công cho sinh viên SV2026002 với TS. Lê Thị B vào lúc 14:00 20/09/2026."
    },
    "latency_ms": 2854.03
  },
  {
    "step": 5,
    "query": "Tôi có mã sinh viên SV2026002 muốn gặp Academic Advisor để được tư vấn về việc đăng ký môn học kỳ tới. Hãy tìm Advisor phụ trách tôi và đặt cho tôi một lịch hẹn vào 20/9/2026 vào lúc 14:00.\n\nObservation từ Tool academic_query:\n{\"status\": \"SUCCESS\", \"student_id\": \"SV2026002\", \"data\": {\"full_name\": \"Trần Thị Bình\", \"class\": \"AI-K4\", \"gpa\": 3.6, \"email\": \"binh.tt@vinuni.edu.vn\", \"status\": \"Đang học\", \"advisor\": \"TS. Lê Thị B\"}}\n\nHãy tiếp tục xử lý yêu cầu dựa trên Observation.\n\nObservation từ Tool schedule_appointment:\n{\"status\": \"SUCCESS\", \"booking_id\": \"BK-SV2026002-99\", \"student_id\": \"SV2026002\", \"datetime\": \"14:00 20/09/2026\", \"advisor\": \"TS. Lê Thị B\", \"message\": \"Đặt lịch thành công cho sinh viên SV2026002 với TS. Lê Thị B vào lúc 14:00 20/09/2026.\"}\n\nHãy tiếp tục xử lý yêu cầu dựa trên Observation.",
    "action_type": "FINAL_ANSWER",
    "thought": "Gemini phản hồi trực tiếp bằng văn bản (không cần gọi công cụ).",
    "output": "Chào bạn **Trần Thị Bình** (Mã sinh viên: **SV2026002**),\n\nYêu cầu đặt lịch tư vấn học vụ của bạn đã hoàn tất thành công. Dưới đây là thông tin chi tiết về lịch hẹn:\n\n* **Cố vấn học tập (Academic Advisor):** TS. Lê Thị B\n* **Thời gian:** 14:00, ngày 20/09/2026\n* **Mã xác nhận đặt lịch:** BK-SV2026002-99\n* **Nội dung tư vấn:** Tư vấn đăng ký môn học cho học kỳ tới\n\nBạn vui lòng sắp xếp thời gian có mặt đúng giờ. Nếu có bất kỳ thay đổi nào hoặc cần hỗ trợ thêm, hãy báo lại cho Trợ lý Học vụ nhé. Chúc bạn một học kỳ học tập hiệu quả!",
    "latency_ms": 3440.34
  }
]
```

---

## 3. TỔNG KẾT KẾT QUẢ NGHIỆM THU & NỘP BÀI

- [ ] Đã điền API Key thật trong `.env` và xác nhận Agent chạy mượt mà trên LLM API thật (Gemini/OpenAI).
- **Tổng số Test Cases đã chạy thành công:** 5 / 5 test cases.
- **Số lượt gọi Tool qua MCP Server chính xác:** 4 lượt.
- **Kết quả đẩy Repo nộp bài:** [x] Đã Commit và Push mã nguồn thành công lên GitHub cá nhân.

---

> ✅ **HOÀN TẤT NỘP BÀI:** Sao chép đường link GitHub Repository cá nhân của bạn và dán vào ô nộp bài trên hệ thống LMS VLearn để hoàn tất Bài Lab 3!
