# Lab 02 - 01 Problem Scan

## Phase 1 - SCAN

### 1. List bài toán của tôi

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Xanh SM | Repetitive | Điều phối viên phải tra cứu thủ công vị trí, trạng thái pin và xe gần nhất để route lại chuyến khi tài xế hủy hoặc đổi điểm đón, lặp lại nhiều lần trong ngày. |
| 2 | Vinhomes | Time-consuming | Nhân viên CSKH phải đọc và soạn phản hồi cho các khiếu nại 1-star của cư dân, sau đó mới gửi cho quản lý duyệt. |
| 3 | Vinmec | AI-upgrade | Nhân viên nhập liệu hoặc thư ký y khoa phải tóm tắt ghi chú khám, đơn thuốc và hướng dẫn ra viện từ dữ liệu bác sĩ ghi tay hoặc ghi âm. |
| 4 | VinFast | Repetitive | Bộ phận vận hành phải đối soát hóa đơn sạc điện, log trạm sạc và mã giao dịch mỗi ngày để phát hiện lệch dữ liệu hoặc sai phí. |
| 5 | Vinpearl / VinWonders | Stakeholder Pain | Bộ phận CSKH phải trả lời các câu hỏi lặp lại về đổi vé, giờ mở cửa, chính sách hoàn tiền và tình trạng đặt chỗ, khiến khách chờ lâu. |

---

## Phase 2 - QUICK-ASSESS

### Quick Problem Card #1

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Điều phối viên Xanh SM phải route lại chuyến│
│ khi tài xế hủy hoặc thay đổi điểm đón trong thời gian rất gấp│
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên, tài xế, và khách chờ xe│
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận cuộc gọi/hủy chuyến -> 2. Kiểm tra vị trí xe       │
│   -> 3. Tìm tài xế gần nhất -> 4. Gửi lại thông tin chuyến   │
│   -> 5. Xác nhận với khách/tài xế                           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4 (⏱ 8-12 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Gợi ý tài xế phù hợp,  │
│ ưu tiên tuyến gần nhất, và draft tin nhắn thông báo          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian route  │
│ lại chuyến từ 10 phút xuống dưới 2 phút, giảm tỷ lệ hủy lại  │
│ sau khi phân công xuống dưới 5%                              │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #2

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): CSKH Vinhomes phải viết phản hồi cho các   │
│ đánh giá 1-star và khiếu nại cư dân theo đúng chính sách      │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH và quản lý duyệt phản hồi│
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đọc ticket đánh giá thấp -> 2. Phân loại vấn đề         │
│   -> 3. Tra chính sách nội bộ -> 4. Soạn phản hồi            │
│   -> 5. Chờ duyệt rồi gửi                                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 10-15 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tóm tắt complaint, đề  │
│ xuất giọng điệu phù hợp, và draft phản hồi theo policy       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian soạn   │
│ phản hồi từ 12 phút xuống dưới 3 phút, tăng tỷ lệ phản hồi  │
│ đúng policy lên trên 95%                                    │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #3

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Tóm tắt ghi chú khám và hướng dẫn ra viện  │
│ từ dữ liệu bác sĩ để chuẩn hóa hồ sơ bệnh án tại Vinmec      │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ, thư ký y khoa, và nhân viên hồ sơ│
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Bác sĩ ghi chú khám -> 2. Thư ký chép lại                │
│   -> 3. Tóm tắt triệu chứng/đơn thuốc -> 4. Kiểm tra lại      │
│   -> 5. Lưu vào hồ sơ điện tử                                │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4 (⏱ 15-20 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Trích xuất thông tin   │
│ chính, tóm tắt nội dung, tạo bản nháp để bác sĩ duyệt        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian nhập   │
│ liệu từ 20 phút xuống dưới 5 phút, giảm lỗi thiếu thông tin  │
│ xuống dưới 3%                                                │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

### Quick Problem Card #4

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                       │
│                                                             │
│ Bài toán (1 câu): Đối soát hóa đơn sạc điện và log trạm sạc  │
│ tại VinFast để phát hiện lệch dữ liệu hoặc sai phí           │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên vận hành, kế toán đối soát   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận hóa đơn -> 2. So khớp mã giao dịch -> 3. Đối chiếu │
│   log trạm sạc -> 4. Ghi nhận sai lệch -> 5. Gửi xử lý       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4 (⏱ 10-15 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tự động gắn cặp dữ liệu│
│ và phát hiện outlier để nhân viên kiểm tra                   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian đối    │
│ soát mỗi lô từ 15 phút xuống dưới 4 phút, tăng tỷ lệ phát   │
│ hiện sai lệch lên trên 98%                                  │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #5

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #5                                       │
│                                                             │
│ Bài toán (1 câu): CSKH Vinpearl / VinWonders trả lời nhanh   │
│ các câu hỏi lặp lại về đổi vé, hoàn tiền, giờ mở cửa, đặt chỗ│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [x] Khác (Ghi rõ) Vinpearl │
│                                                             │
│ Ai đang đau (Actor)? Khách hàng, nhân viên CSKH             │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách hỏi -> 2. Nhân viên tra FAQ/chính sách            │
│   -> 3. Soạn câu trả lời -> 4. Gửi lại cho khách             │
│   -> 5. Nếu phức tạp thì chuyển cấp trên                    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 5-8 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Gợi ý câu trả lời và  │
│ trích đúng chính sách từ FAQ nội bộ                         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian phản  │
│ hồi từ 8 phút xuống dưới 1 phút, tăng tỷ lệ phản hồi nhất   │
│ quán lên trên 90%                                           │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
