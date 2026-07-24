# 01 — Problem Scan

## 🔍 Phase 1 — SCAN (Cá nhân)

Hãy sử dụng 4 lenses để quét qua hoạt động vận hành của các công ty thành viên Vingroup và ghi lại ít nhất 5 bài toán thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày.
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn nhiều thời gian xử lý thủ công của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn.
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn.

### 📝 Bảng quét cơ hội của tôi:
| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | VinFast | Repetitive | AI tự động phân loại và xử lý ticket bảo hành xe dựa trên mô tả lỗi, giảm công việc phân loại thủ công của nhân viên CSKH.
| 2 | Xanh SM | Stakeholder Pain | AI tối ưu gợi ý điểm đón/trả khách dựa trên dữ liệu giao thông, lịch sử chuyến đi và vị trí thực tế, giảm tình trạng tài xế phải gọi điện xác nhận nhiều lần.
| 3 | Vinhomes | Time-consuming | AI hỗ trợ ban quản lý soạn thảo phản hồi cho các phản ánh cư dân (tiếng ồn, vệ sinh, bãi đỗ xe…), rồi nhân viên chỉ cần rà soát và gửi.
| 4 | Vinmec | AI-upgrade | AI Assistant hỗ trợ người bệnh đặt lịch khám, giải đáp câu hỏi về chuyên khoa, chuẩn bị trước khi khám và hướng dẫn sau khám 24/7 thay vì chatbot theo kịch bản cố định.
| 5 | Vincom Retail | Repetitive | AI tự động tổng hợp dữ liệu doanh thu, lượng khách và sự cố vận hành từ nhiều cửa hàng để tạo báo cáo hằng ngày cho ban quản lý.

---

## 🃏 Phase 2 — QUICK-ASSESS (Cá nhân)

Chọn top 3 bài toán từ bảng trên và hoàn thiện 3 Quick Problem Cards.

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                     │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại và xử lý ticket bảo hành xe dựa trên mô tả lỗi từ khách hàng.   │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH và kỹ sư bảo hành VinFast. │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận ticket lỗi xe từ khách hàng ──> 2. Đọc thông tin mô tả  │
│   3. Phân loại vấn đề và gắn nhãn lỗi ──> 4. Chuyển ticket đến đội kỹ thuật │
│   5. Soạn ghi chú hướng xử lý và trả lời khách hàng          │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Đọc mô tả và phân loại lỗi thủ công (⏱ 5-7 phút/lượt). │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Phân loại lỗi tự động và gợi ý hướng xử lý ban đầu. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? 80% ticket phân loại đúng trong <30s và giảm thời gian xử lý 50%. │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                     │
│                                                             │
│ Bài toán (1 câu): AI tối ưu gợi ý điểm đón/trả khách cho tài xế Xanh SM dựa trên dữ liệu giao thông và lịch sử chuyến đi. │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế điều phối và dispatcher Xanh SM. │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Dispatcher nhận yêu cầu đón khách ──> 2. Kiểm tra bản đồ và lịch sử chuyến đi  │
│   3. Đề xuất điểm đón/trả sơ bộ ──> 4. Gọi điện xác nhận hoặc chỉnh sửa đề xuất │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Gợi ý điểm đón/trả phù hợp thực tế và giao thông (⏱ 3-5 phút/lượt). │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Đề xuất điểm đón/trả tối ưu tự động kèm lý do và tính toán mức độ thuận tiện. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm 30% số lần gọi xác nhận lại và tăng 20% tỷ lệ điểm đón đúng lần đầu. │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                     │
│                                                             │
│ Bài toán (1 câu): AI hỗ trợ soạn thảo phản hồi cho các phản ánh cư dân Vinhomes, rồi nhân viên chỉ cần rà soát và gửi. │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên ban quản lý tòa nhà và bộ phận CSKH Vinhomes. │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận phản ánh cư dân qua email/ứng dụng ──> 2. Đọc nội dung và xác định chủ đề  │
│   3. Soạn trả lời thủ công ──> 4. Đánh giá và gửi phản hồi cho cư dân   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Soạn nội dung trả lời phù hợp tone công ty và đủ chi tiết (⏱ 8-10 phút/lượt). │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tạo bản nháp phản hồi phù hợp, rõ ràng, và đề xuất hành động tiếp theo. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Rút ngắn thời gian soạn phản hồi từ 10 phút xuống còn dưới 3 phút và đạt 90% tỷ lệ phản hồi hợp lý. │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────┘
```
