# 02 - Deep Dive Report

## Thông tin nhóm

- Thành viên 1: Hồ Văn Thi
- Thành viên 2: Nguyễn Gia Thiều
- Thành viên 3: Lê Nguyễn Phi Trường
- Thành viên 4: Nguyễn Quốc Thịnh
- Thành viên 5: Trần Duy Trường
- Thành viên 6: Nguyễn Hoàng Minh

---

## Quyết định lựa chọn

Nhóm chọn bài toán **Xanh SM - xử lý sự cố hết pin thực địa của tài xế** để thực hiện deep-dive.

Lý do lựa chọn:
- Quy trình hiện tại rõ ràng, có nhiều bước thủ công.
- Bottleneck lớn nằm ở tra cứu vị trí trạm sạc và soạn hướng dẫn.
- Có thể đo lường hiệu quả bằng thời gian xử lý và độ chính xác hướng dẫn.
- Có giới hạn an toàn rõ ràng: không được gợi ý trạm sạc quá xa khi pin dưới ngưỡng nguy hiểm.

---

## 1. Problem Statement (6-field)

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Điều phối viên tại trung tâm điều vận Xanh SM. |
| **2. Current Workflow** | Khi tài xế báo pin yếu hoặc hết pin, điều phối viên phải mở bản đồ, tra vị trí GPS của xe, tìm trạm sạc VinFast còn trống gần nhất, tự soạn nội dung hướng dẫn và gửi cho tài xế qua app hoặc tin nhắn. Nếu trường hợp khẩn cấp, điều phối viên có thể phải gọi đội cứu hộ hỗ trợ. |
| **3. Bottleneck** | Bước tra cứu trạm sạc và soạn hướng dẫn chiếm nhiều thời gian nhất, vì phải đối chiếu vị trí, loại cổng sạc, khoảng cách di chuyển và ngữ cảnh giao thông. |
| **4. Business Impact** | Mỗi lượt xử lý thủ công mất khoảng 10-15 phút, gây chậm phản hồi cho tài xế, làm giảm khả năng đón khách và tăng áp lực cho điều phối viên trong giờ cao điểm. |
| **5. Success Metric** | Giảm thời gian xử lý một sự cố từ 15 phút xuống dưới 3 phút, đồng thời đạt trên 98% hướng dẫn đúng trạm sạc phù hợp và an toàn. |
| **6. Operational Boundary** | AI chỉ được hỗ trợ tra cứu, gợi ý và soạn nháp; không được tự động gửi tin nhắn nếu chưa có điều phối viên duyệt. Nếu pin dưới 5%, AI không được đề xuất trạm sạc xa quá 5km và phải ưu tiên phương án xe cứu hộ hoặc sạc pin di động. |

---

## 2. Current-State Workflow

```text
1. Tài xế báo sự cố pin yếu/hết pin
   -> 2. Điều phối viên nhận thông tin cuộc gọi/tin nhắn
   -> 3. Tra cứu vị trí xe trên bản đồ
   -> 4. Tra cứu trạm sạc VinFast còn trống gần nhất
   -> 5. Soạn hướng dẫn đường đi và nội dung nhắn cho tài xế
   -> 6. Gửi nháp cho tài xế / xác nhận với đội cứu hộ nếu cần
```

- Các handoff chính:
  - Tài xế -> điều phối viên
  - Điều phối viên -> hệ thống bản đồ / dashboard trạm sạc
  - Điều phối viên -> tài xế hoặc đội cứu hộ
- Tổng thời gian trung bình: khoảng 15 phút/lượt
- Bottleneck nặng nhất: bước 4 và 5

---

## 3. Future-State Flow & AI Fit

### AI Fit

Nhóm đánh giá bài toán này phù hợp với **LLM Feature** kết hợp rule-based guardrails.

Lý do:
- Quy trình có cấu trúc tương đối rõ.
- AI mạnh ở việc đọc ngữ cảnh, tóm tắt và draft câu hướng dẫn.
- Quy tắc an toàn về pin dưới 5% phải được kiểm soát chặt bằng rule cố định.

### Future-State Flow

```text
1. Tài xế báo pin yếu
   -> 2. Hệ thống tự lấy GPS xe, mức pin, loại xe
   -> 3. Rule engine kiểm tra ngưỡng an toàn
      -> nếu pin < 5%: chuyển sang phương án xe cứu hộ / sạc pin di động
      -> nếu pin >= 5%: chuyển sang bước gợi ý trạm sạc gần nhất
   -> 4. LLM tạo nháp tin nhắn hướng dẫn
   -> 5. Điều phối viên review (Human-in-the-loop)
   -> 6. Gửi cho tài xế
```

### Human-in-the-loop

- Điều phối viên phải duyệt mọi nội dung trước khi gửi.
- Nếu LLM trả kết quả mơ hồ hoặc thiếu dữ liệu, hệ thống phải chuyển sang chế độ nháp an toàn.

### Fallback

- Nếu API AI lỗi hoặc không tự tin, hệ thống dùng template rule-based:
  - yêu cầu tài xế chờ
  - ưu tiên xe cứu hộ
  - không tự ý gợi ý trạm quá xa

---

## 4. Evaluate

### AI Readiness Checklist

- [x] Chúng tôi có dữ liệu mẫu/logs đủ sạch để test
- [x] Rủi ro khi AI sai có thể kiểm soát bằng HITL và fallback
- [x] Stakeholders có thể chấp nhận thay đổi quy trình ở mức hợp lý

### Quyết định cuối cùng

- **GO**

### Justification

Nhóm quyết định GO vì bài toán có:
- phạm vi rõ ràng
- metric đo được
- giá trị vận hành cao
- ranh giới an toàn có thể mã hóa bằng rule

Chi phí triển khai ban đầu không quá lớn vì AI chủ yếu làm nhiệm vụ tra cứu ngữ cảnh và draft nội dung. Tuy nhiên, hệ thống bắt buộc phải có lớp kiểm soát để tránh gợi ý trạm sạc không an toàn khi pin dưới ngưỡng nguy hiểm.

