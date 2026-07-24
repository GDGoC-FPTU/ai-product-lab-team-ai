# 02 — Deep Dive Report

## Quyết định lựa chọn

Bài toán được chọn để thực hiện Deep-Dive là:

- **AI tối ưu gợi ý điểm đón/trả khách cho Xanh SM** dựa trên dữ liệu giao thông, lịch sử chuyến đi và vị trí thực tế.

Nhóm chọn bài toán này vì nó vừa có tác động trực tiếp tới trải nghiệm khách hàng và hiệu suất điều phối, vừa phù hợp với khả năng áp dụng AI cho xử lý ngôn ngữ tự nhiên, ngữ cảnh địa lý và ưu tiên quyết định.

---

## Problem Statement (6-field)

1. **Actor / Operator**
   - Dispatcher điều phối và tài xế Xanh SM.
   - Dispatcher chịu trách nhiệm đọc yêu cầu đón khách, chọn điểm đón/trả hợp lý, và xác nhận với tài xế hoặc khách hàng.

2. **Current Workflow**
   - 1) Dispatcher nhận yêu cầu đón khách qua app/CRM.
   - 2) Kiểm tra bản đồ, dữ liệu giao thông, điều kiện địa hình và lịch sử chuyến đi tương tự.
   - 3) Đề xuất điểm đón/trả sơ bộ dựa trên kinh nghiệm.
   - 4) Gọi điện hoặc nhắn tin xác nhận với tài xế/khach hàng.
   - 5) Chỉnh sửa, ghi nhận và gửi yêu cầu cho tài xế.

3. **Bottleneck**
   - Bước tốn thời gian nhất là xác định điểm đón/trả chính xác với điều kiện giao thông và các yêu cầu thực tế.
   - Dispatcher thường phải gọi lại nhiều lần để xác nhận hoặc điều chỉnh vị trí, mất khoảng **3-5 phút/lượt**.
   - Điều này tạo ra độ trễ, sai sót trong lộ trình và giảm hiệu suất sử dụng đội xe.

4. **Business Impact**
   - Nếu dispatch không chính xác, Xanh SM mất thời gian xử lý lại, tăng chi phí vận hành và hạ thấp trải nghiệm khách hàng.
   - Ước tính: **20% thời gian điều phối bị lãng phí** cho xác nhận lại và **15% chuyến bị chọn điểm không phù hợp lần đầu**, dẫn tới trễ giờ hoặc huỷ chuyến.

5. **Success Metric**
   - Giảm **30% số lần gọi xác nhận lại** giữa dispatcher và tài xế/khách.
   - Tăng **20% tỷ lệ điểm đón chính xác lần đầu**.
   - Giảm thời gian chuẩn bị đề xuất điểm đón/trả từ trung bình **4 phút xuống dưới 2 phút**.

6. **Operational Boundary**
   - AI chỉ được phép **đề xuất danh sách điểm đón/trả tối ưu**; quyết định cuối cùng vẫn do dispatcher phê duyệt.
   - Không được phép tự động gán điểm đón/trả cho tài xế mà không có con người xác nhận.
   - Nếu dữ liệu thiếu, hoặc nếu model đưa ra nhiều tùy chọn mâu thuẫn, phải chuyển ngay về quy trình thủ công hoặc fallback rule-based.

---

## Future-State Flow & AI Fit

### Mức độ ứng dụng AI

- **AI Fit:** LLM Feature
- **Tại sao không Agent:** Giải pháp ưu tiên hỗ trợ người điều phối với đề xuất thông minh, không cần vòng lặp hành vi tự chủ.
- **Kết hợp Rule:** Dùng rule-based fallback khi dữ liệu không đầy đủ hoặc khi model confidence thấp.

### Text diagram quy trình tương lai

1. **Nhận yêu cầu đón khách** từ app/CRM.
2. **Thu thập dữ liệu**: vị trí khách, điểm đón khả thi, tình trạng giao thông, lịch sử chuyến tương tự, trạng thái tài xế.
3. **AI phân tích và gợi ý** 2-3 điểm đón/trả ưu tiên cùng lý do (ví dụ: độ an toàn, tiết kiệm thời gian, thuận tiện cho khách).
4. **Dispatcher xem xét và phê duyệt** điểm đón/trả tốt nhất. (Human-in-the-loop)
5. **Xác nhận với tài xế/khách hàng** và cập nhật lệnh điều phối.
6. **Hệ thống ghi lại kết quả** để học từ phản hồi và tinh chỉnh mô hình.

### Chú thích Human-in-the-loop

- Bước 4 là điểm phê duyệt chính: dispatcher giữ quyền quyết định cuối cùng.
- AI chỉ hỗ trợ gợi ý, không thay thế đánh giá con người.
- Mọi đề xuất đều phải được xem xét trước khi gửi đến tài xế.

### Fallback khi AI gặp lỗi

- Nếu AI xác định **confidence thấp**, hoặc dữ liệu giao thông/historic mâu thuẫn, chuyển sang:
  - Rule-based fallback: chọn điểm đón gần nhất tại các nút giao thông an toàn đã xác định trước.
  - Hoặc trả về một cảnh báo cho dispatcher: "Dữ liệu không đủ, vui lòng xử lý thủ công".
- Nếu AI đề xuất điểm không thực thi được (ví dụ: cổng vào bị cấm, đường cấm giờ), loại bỏ đề xuất và quay về quy trình thủ công.

---

## Evaluate

### AI Readiness Checklist

1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
   - Có dữ liệu yêu cầu đón khách, lịch sử chuyến, và các điểm đón/trả đã xác nhận.
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
   - Có; dispatcher phê duyệt cuối cùng và rule-based fallback được thiết lập.
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?
   - Có; dispatcher được giữ quyền kiểm soát và chỉ nhận hỗ trợ, nên thay đổi ít rủi ro.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future

[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

### Justification

- **Giải pháp khả thi kỹ thuật:** Dữ liệu vận hành đã có sẵn, và bài toán phù hợp với AI để xử lý ngữ cảnh địa lý + hành vi người dùng.
- **Rủi ro kiểm soát tốt:** Dispatcher vẫn giữ quyền phê duyệt, nên hệ thống không tự động ra lệnh nhạy cảm.
- **Impact rõ ràng:** Tiết kiệm thời gian điều phối, giảm số lần xác nhận lại, cải thiện trải nghiệm tài xế và khách.
- **Chi phí prototype vừa phải:** 1-2 kỹ sư/back-end + 1 product analyst trong 4 tuần để xây dựng MVP, thử nghiệm với dữ liệu dispatch hiện có.
- **Ước lượng chi phí:** Prototype MVP trong 4 tuần có thể hoàn thành với chi phí nhân sự tương đương khoảng 0.5 đến 1.0 engineer-month, cộng cloud inference nhỏ (dưới 300 USD/tháng nếu dùng model hosted nhẹ). 

> Kết luận: Bài toán đủ điều kiện GO vì không cần thay đổi lớn trong quy trình hiện tại, AI được dùng để hỗ trợ chứ không thay thế, và kiểm soát rủi ro rõ ràng.
