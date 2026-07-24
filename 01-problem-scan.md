# Lab 02 — Problem Scan & Quick Cards
**Họ và tên:** Trương (AI Engineer — Vin Smart Future)  
**Công ty:** Vin Smart Future (Vingroup)  
**Ngày thực hiện:** 2026-07-24  

---

## 🔍 Phase 1 — SCAN: Danh sách bài toán vận hành Vingroup

Sử dụng 4 Lenses (Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác) để quét qua hoạt động vận hành của các công ty thành viên Vingroup và xác định 5 bottleneck thực tế:

| # | Subsidiary | Lens | Mô tả ngắn bài toán & Bottleneck thực tế |
|---|------------|------|------------------------------------------|
| 1 | **Xanh SM (GSM)** | Tốn thời gian & Pain | Xử lý sự cố sạc pin/hết pin thực địa: Điều phối viên tra cứu thủ công vị trí GPS xe, tra cứu trạm sạc VinFast còn trụ trống phù hợp và soạn tin nhắn hướng dẫn/điều cứu hộ (mất 15 phút/lượt). |
| 2 | **VinFast** | Lặp lại (Repetitive) | So khớp và kiểm toán tự động hóa đơn dịch vụ sạc điện đối tác hằng tuần giữa dữ liệu telemetry trụ sạc VinFast và hóa đơn nhà cung cấp hạ tầng. |
| 3 | **Vinhomes** | AI-upgrade & Tốn thời gian | Phân loại tự động và soạn thảo nháp phản hồi cá nhân hóa cho khiếu nại/đánh giá 1-star của cư dân trên ứng dụng Vinhomes Resident (hiện phản hồi chậm, rập khuôn, mất 12 tiếng). |
| 4 | **Vinmec** | Pain từ người khác | Tóm tắt tự động hồ sơ bệnh án và lịch sử xét nghiệm phức tạp để hỗ trợ bác sĩ lâm sàng soạn Báo cáo Xuất viện (Discharge Summary) (bác sĩ mất 20-30 phút/bệnh nhân). |
| 5 | **Xanh SM (GSM)** | Lặp lại (Repetitive) | Phân tích và tóm tắt tự động nguyên nhân hủy chuyến của khách hàng từ ghi âm cuộc gọi tổng đài và ghi chú của tài xế để nhận diện pattern lỗi hệ thống. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

### 📌 Quick Problem Card #1 — Xanh SM: Xử lý sự cố sạc pin thực địa

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                        │
│                                                                              │
│ Bài toán: Tài xế Xanh SM báo sự cố hết pin/pin yếu khẩn cấp cần hướng dẫn    │
│ trạm sạc phù hợp hoặc điều xe sạc pin di động (Mobile Charger).              │
│ Công ty thành viên: [x] Xanh SM (GSM)                                        │
│                                                                              │
│ Ai đang đau (Actor)? Tài xế (chờ đợi ngoài đường), Điều phối viên (quá tải). │
│                                                                              │
│ Workflow thủ công hiện tại (5 bước):                                         │
│   1. Tài xế gọi tổng đài điều vận báo hết pin/pin khẩn cấp.                  │
│   → 2. Điều phối viên tra cứu thủ công vị trí GPS của xe trên hệ thống.       │
│   → 3. Tra cứu trạm sạc VinFast lân cận còn trụ sạc trống & đúng loại cổng.  │
│   → 4. Soạn tin nhắn hướng dẫn đường đi chi tiết gửi qua ứng dụng Tài xế.     │
│   → 5. Điều động xe cứu hộ/sạc di động nếu lượng pin dưới 5%.                │
│                                                                              │
│ Bước nào tốn thời gian nhất? Bước 3 & 4 (⏱ 10-12 phút/lượt)                  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4                             │
│ (Tự động pull vị trí & trạm trống -> AI draft SMS chỉ dẫn & command cứu hộ)  │
│                                                                              │
│ Đo thành công bằng gì (Metric có số)?                                        │
│ Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/lượt (Giảm 80%).       │
│                                                                              │
│ Quick Architecture: [x] LLM Feature (Draft SMS & Dispatch Command với HITL)  │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

### 📌 Quick Problem Card #2 — Vinhomes: Tự động phân loại & Phản hồi khiếu nại cư dân

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                        │
│                                                                              │
│ Bài toán: Tự động phân loại, trích xuất thực thể và draft phản hồi cá nhân   │
│ hóa cho phản ánh/khiếu nại của cư dân trên App Vinhomes Resident.             │
│ Công ty thành viên: [x] Vinhomes                                             │
│                                                                              │
│ Ai đang đau (Actor)? Ban Quản lý Vinhomes (CSKH quá tải), Cư dân (chờ lâu).  │
│                                                                              │
│ Workflow thủ công hiện tại (4 bước):                                         │
│   1. Cư dân gửi ticket phản ánh về phí quản lý / tiếng ồn / kỹ thuật.       │
│   → 2. Nhân viên CSKH đọc thủ công, phân loại phòng ban xử lý.               │
│   → 3. Tra cứu quy định/chính sách và soạn email/tin nhắn phản hồi.          │
│   → 4. Gửi phản hồi và chuyển ticket cho bộ phận kỹ thuật/an ninh.          │
│                                                                              │
│ Bước nào tốn thời gian nhất? Bước 2 & 3 (⏱ 15 phút/ticket)                  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3                             │
│ (Phân loại tag tự động -> Trích xuất thông tin -> Soạn nháp câu trả lời)     │
│                                                                              │
│ Đo thành công bằng gì (Metric có số)?                                        │
│ Giảm thời gian phản hồi ban đầu cho cư dân từ 12 giờ ──> dưới 15 phút.       │
│                                                                              │
│ Quick Architecture: [x] LLM Feature (Classification + RAG Draft Response)    │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

### 📌 Quick Problem Card #3 — Vinmec: Tóm tắt hồ sơ bệnh án tự động (Discharge Summary)

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                        │
│                                                                              │
│ Bài toán: Tự động tóm tắt tiến trình điều trị, kết quả xét nghiệm và đơn    │
│ thuốc để hỗ trợ bác sĩ soạn Báo cáo Xuất viện cho bệnh nhân.                 │
│ Công ty thành viên: [x] Vinmec                                               │
│                                                                              │
│ Ai đang đau (Actor)? Bác sĩ điều trị (quá tải hành chính, thiếu thời gian).  │
│                                                                              │
│ Workflow thủ công hiện tại (4 bước):                                         │
│   1. Bác sĩ mở hồ sơ bệnh án điện tử (EMR) của bệnh nhân.                   │
│   → 2. Đọc lại toàn bộ lịch sử khám, kết quả xét nghiệm máu/x-quang, chẩn đoán.│
│   → 3. Tổng hợp thủ công và gõ Báo cáo Xuất viện (Discharge Summary).        │
│   → 4. In báo cáo và ký xác nhận đưa bệnh nhân.                              │
│                                                                              │
│ Bước nào tốn thời gian nhất? Bước 2 & 3 (⏱ 20-30 phút/bệnh nhân)            │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3                             │
│ (Trích xuất EMR data -> Summarize các chỉ số bất thường -> Draft báo cáo)    │
│                                                                              │
│ Đo thành công bằng gì (Metric có số)?                                        │
│ Giảm thời gian soạn Báo cáo Xuất viện từ 25 phút ──> dưới 5 phút/bệnh nhân. │
│                                                                              │
│ Quick Architecture: [x] LLM Feature (Structured Data Extraction & Summary)   │
└──────────────────────────────────────────────────────────────────────────────┘
```
