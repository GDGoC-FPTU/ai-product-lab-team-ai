# 01 — Problem Scan

## 🔍 Phase 1 — SCAN (Cá nhân)

Hãy sử dụng 4 lenses để quét qua hoạt động vận hành của các công ty thành viên Vingroup và ghi lại ít nhất 5 bài toán thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày.
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn nhiều thời gian xử lý thủ công của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn.
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn.

### 📝 Bảng quét cơ hội của tôi:
| # | Subsidiary (VinFast/Xanh SM...)  | Lens                    | Mô tả ngắn bài toán |
|---|----------------------------------|-------------------------|---------------------|
| 1 |      vinmec                      |   RAG (Generative AI)   |     Trợ lý ảo tìm kiếm và truy xuất nhanh phác đồ điều trị, hướng dẫn nội bộ (SOP) cho đội ngũ y bác sĩ.              |
| 2 |  vinmec                          | Computer Vision & LLMs  | Tự động hóa trích xuất và số hóa dữ liệu hồ sơ bệnh án, đơn thuốc ngoại viện vào hệ thống nội bộ.                       |
| 3 |  vinmec                          | NLP (Phân loại văn bản) | Đọc hiểu hồ sơ lâm sàng và tự động gán mã bệnh tật (ICD-10/11) để xử lý thanh toán bảo hiểm.                         |
| 4 |  vinmec                          | Predictive Analytics    | Dự đoán tỷ lệ hủy lịch(no-shows) và điều phối bệnh nhân tự động để tối ưu công suất bác sĩ và thiết bị (MRI, CT).       |
| 5 |  vinmec                          | Time-Series Forecasting | Phân tích chuỗi thời gian để dự báo nhu cầu vật tư y tế, tối ưu tồn kho và giảm lượng thuốc hết hạn.                   |

---

## 🃏 Phase 2 — QUICK-ASSESS (Cá nhân)

Chọn top 3 bài toán từ bảng trên và hoàn thiện 3 Quick Problem Cards.

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Số hóa tự động hồ sơ bệnh án ngoại viện.  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ, Điều dưỡng, Lễ tân phòng khám. │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận bản scan ──> 2. Đọc thủ công ──> 3. Nhập dữ liệu  │
│   vào hệ thống EMR ──> 4. Kiểm tra chéo lỗi chính tả.       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (⏱ 10-15 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3 (Dùng một  │
│ hệ thống dịch thuật từ hình ảnh sang văn bản nhận thức ngữ  │
│ cảnh kết hợp Computer Vision và LLM để tự động điền form).  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian nhập liệu từ 15 min ──> under 2 min/hồ sơ. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tự động gán mã bệnh tật ICD-10/11 cho HSBA│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên Coding y khoa, Kế toán BHYT. │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đọc hồ sơ ra viện ──> 2. Tra cứu mã ICD thủ công ──>   │
│   3. Gán mã vào hệ thống ──> 4. Làm hồ sơ gửi bảo hiểm.     │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (⏱ 20-30 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (Đọc tóm tắt   │
│ lâm sàng và tự động đề xuất Top 3 mã ICD chuẩn xác nhất).   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm tỷ lệ từ chối bồi thường BHYT từ 15% ──> under 5%.   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Trợ lý RAG tra cứu nhanh phác đồ nội bộ.  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị, Dược sĩ lâm sàng.     │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Có ca bệnh khó ──> 2. Mở kho SOP PDF ──> 3. Dùng Ctrl+F│
│   tìm keyword ──> 4. Đọc chắt lọc từ hàng chục trang text.  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 4 (⏱ 30-45 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4 (Hỏi đáp   │
│ ngữ nghĩa, truy xuất trích dẫn đúng đoạn văn bản cần tìm).  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian tra cứu phác đồ từ 30 min ──> under 1 min. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```