# 01-problem-scan — AI Product Scoping

## Phase 1 — SCAN

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Xanh SM | Lặp lại | Tài xế báo pin thấp giữa đường, điều phối viên vẫn phải tra vị trí và trạm sạc bằng tay để tìm phương án an toàn. |
| 2 | Xanh SM | Tốn thời gian | Điều phối viên soạn tin nhắn chỉ đường/dặn dò từ dữ liệu GPS, % pin và trạng thái trạm sạc. |
| 3 | VinFast | Lặp lại | Đối chiếu hóa đơn và nhật ký sạc giữa các trạm sạc và xe điện theo ngày/tuần. |
| 4 | Vinhomes | AI-upgrade | Phân loại và phản hồi khiếu nại cư dân trên app thay cho các mẫu trả lời rập khuôn. |
| 5 | Vinmec | Pain từ người khác | Tóm tắt hồ sơ xuất viện và ghi chú lâm sàng để giảm thời gian bác sĩ nhập liệu thủ công. |

## Phase 2 — QUICK-ASSESS

### QUICK PROBLEM CARD #1
Bài toán: Điều phối sự cố pin thực địa cho tài xế Xanh SM.
Công ty thành viên: Xanh SM.
Ai đang đau (Actor)? Điều phối viên và tài xế đang chờ xử lý.
Workflow thủ công hiện tại: 1) nhận cuộc gọi -> 2) tra vị trí xe -> 3) tra trạm sạc phù hợp -> 4) soạn hướng dẫn -> 5) gửi nháp để duyệt.
Bước tốn thời gian/lỗi nhất: tra trạm sạc phù hợp và soạn hướng dẫn, khoảng 10 phút/lượt.
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 và 4.
Metric: giảm thời gian xử lý từ 15 phút xuống dưới 3 phút.
Quick Architecture: LLM.

### QUICK PROBLEM CARD #2
Bài toán: Soạn phản hồi nháp khi tài xế cần hướng dẫn gấp.
Công ty thành viên: Xanh SM.
Ai đang đau (Actor)? Điều phối viên tổng đài.
Workflow thủ công hiện tại: 1) nhận tình huống -> 2) đọc vị trí/pin -> 3) viết nháp tin nhắn -> 4) kiểm tra quy tắc an toàn -> 5) duyệt gửi.
Bước tốn thời gian/lỗi nhất: viết nháp đúng ngữ cảnh, khoảng 5-7 phút/lượt.
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3.
Metric: 90% nháp được tạo trong dưới 30 giây.
Quick Architecture: LLM.

### QUICK PROBLEM CARD #3
Bài toán: So khớp sự cố pin với phương án cứu hộ di động.
Công ty thành viên: Xanh SM.
Ai đang đau (Actor)? Dispatcher ca đêm.
Workflow thủ công hiện tại: 1) nhận cảnh báo -> 2) đọc % pin -> 3) kiểm tra khoảng cách tới trạm -> 4) quyết định trạm hay cứu hộ -> 5) log sự kiện.
Bước tốn thời gian/lỗi nhất: bước quyết định, khoảng 3 phút/lượt nhưng dễ nhầm dưới áp lực.
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 4.
Metric: 100% trường hợp pin < 5% phải ưu tiên mobile charger.
Quick Architecture: Rule.
