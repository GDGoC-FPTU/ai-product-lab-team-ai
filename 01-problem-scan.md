# 🔍 Báo cáo Phase 0 & Phase 1: Problem Scan & Scoping (Vin Smart Future)

## 🏛️ Bối cảnh & Phase 0: Worked Example — Xanh SM Intelligent Dispatcher

**Đơn vị:** Vin Smart Future (Vingroup) — Đội ngũ AI Engineering.  
**Dự án ví dụ (Worked Example):** Trợ lý AI Co-pilot hỗ trợ điều phối viên **Xanh SM (GSM)** xử lý sự cố sạc pin và xe cạn kiệt năng lượng thực địa.

---

## 🔍 Phase 1 — SCAN: Quét tìm cơ hội AI tại Vingroup (4 Lenses)

Dưới đây là bảng tổng hợp 6 bài toán/bottleneck vận hành thực tế tại các công ty thành viên thuộc Vingroup được phân tích qua **4 Lenses**:

| # | Subsidiary | Lens | Mô tả ngắn bài toán & Bottleneck |
|---|------------|------|-----------------------------------|
| 1 | **Xanh SM (GSM)** | Tốn thời gian | Điều phối viên xử lý thủ công các phản hồi khẩn cấp từ tài xế về sự cố hết pin/trạm sạc quá tải (mất 15-20 min/lượt). |
| 2 | **Xanh SM (GSM)** | Lặp lại | So khớp và tự động phân bổ lại cuốc xe khi khách hàng yêu cầu thay đổi lộ trình hoặc điểm đến giữa chuyến. |
| 3 | **VinFast** | Lặp lại | Đối chiếu và so khớp hóa đơn sạc điện, dữ liệu tiêu thụ năng lượng giữa trụ sạc VinFast và các trạm sạc đối tác thứ ba hằng tuần. |
| 4 | **Vinhomes** | AI-upgrade | Hệ thống tự động phân loại, trích xuất thông tin và gợi ý phản hồi khiếu nại của cư dân trên App Vinhomes Resident (thời gian xử lý hiện tại mất 12 tiếng). |
| 5 | **Vinmec** | Pain từ người khác | Bác sĩ mất quá nhiều thời gian viết tóm tắt hồ sơ xuất viện và tổng hợp bệnh án (tốn 20-30 phút/bệnh nhân, gây quá tải cho bác sĩ). |
| 6 | **Vinpearl** | Time-consuming | Tư vấn gói combo du lịch, đặt vé VinWonders và lập lịch trình vui chơi giải trí cá nhân hóa tự động cho khách hàng. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 bài toán từ danh sách SCAN để lập Quick Problem Cards:

### 1. QUICK PROBLEM CARD #1 — Xanh SM Xử lý sự cố sạc pin thực địa (Card tiêu biểu)
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo cáo sự cố sạc pin / hết pin    │
│ giữa đường cần điều phối cứu hộ hoặc trạm sạc gần nhất.     │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Tài xế (chờ đợi), Điều phối viên (quá tải)     │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài điều vận báo hết pin               │
│   → 2. Điều phối viên tra cứu định vị GPS xe trên bản đồ    │
│   → 3. Tra cứu thủ công các trạm sạc VinFast còn trụ trống   │
│   → 4. Viết tin nhắn chỉ dẫn/đường đi gửi qua App tài xế    │
│   → 5. Liên hệ đội xe cứu hộ nếu xe đã cạn kiệt pin (< 5%)  │
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 12 phút/lượt)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (Tự động hóa lấy vị trí -> Tra cứu trạm trống -> Draft tin) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút.      │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Tự động soạn chỉ dẫn)   │
└─────────────────────────────────────────────────────────────┘
```

---

### 2. QUICK PROBLEM CARD #2 — Vinhomes Phân loại & Phản hồi khiếu nại cư dân
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Tự động phân loại khiếu nại của cư dân trên App   │
│ Vinhomes Resident và dự thảo văn bản phản hồi sơ bộ.        │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Cư dân (chờ lâu), Ban Quản lý (quá tải ticket) │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh lên ứng dụng Vinhomes Resident     │
│   → 2. Nhân viên BQL đọc và phân loại thủ công tới bộ phận   │
│   → 3. Bộ phận chuyên trách kiểm tra & soạn câu trả lời     │
│   → 4. Ban quản lý phê duyệt và gửi phản hồi                │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 4 - 12 tiếng)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (Phân loại tự động ticket -> Soạn câu trả lời dự thảo)      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Rút ngắn thời gian phản hồi từ 12 tiếng ──> dưới 1 tiếng.   │
│                                                             │
│ Quick Architecture: [x] LLM Feature + Rule-based router     │
└─────────────────────────────────────────────────────────────┘
```

---

### 3. QUICK PROBLEM CARD #3 — Vinmec Tóm tắt hồ sơ xuất viện cho Bác sĩ
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Trích xuất diễn biến bệnh lý & tạo dự thảo Tóm     │
│ tắt hồ sơ xuất viện tự động cho y bác sĩ Vinmec.            │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ (tốn thời gian), Bệnh nhân (chờ lâu)    │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bác sĩ rà soát toàn bộ kết quả xét nghiệm đợt điều trị │
│   → 2. Tổng hợp thủ công các thông tin chính vào hồ sơ      │
│   → 3. Kê đơn thuốc ra viện và dặn dò tái khám              │
│   → 4. In và ký xác nhận hồ sơ xuất viện                    │
│                                                             │
│ Bước nào tốn nhất? Bước 1-2 (⏱ 25 phút/bệnh nhân)           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-2              │
│ (Trích xuất dữ liệu EHR -> Soạn dự thảo tóm tắt xuất viện)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian chuẩn bị hồ sơ từ 30 phút ──> dưới 5 phút.   │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Human-in-the-loop)    │
└─────────────────────────────────────────────────────────────┘
```
