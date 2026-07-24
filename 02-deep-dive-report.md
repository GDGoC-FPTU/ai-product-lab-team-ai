# 02-deep-dive-report — Xanh SM Intelligent Dispatcher

**Họ và tên thành viên trong nhóm**
1. Hồ Văn Thi
2. Nguyễn Gia Thiều
3. Lê Nguyễn Phi Trường
4. Nguyễn Quốc Thịnh
5. Trần Duy Trường
6. Nguyễn Hoàng Minh

## Quyết định lựa chọn
Nhóm chọn bài toán **điều phối sự cố pin thực địa cho tài xế Xanh SM** vì đây là quy trình lặp lại, có dữ liệu đầu vào rõ ràng (vị trí xe, % pin, danh sách trạm sạc), và có thể chặn rủi ro bằng ranh giới vận hành định lượng được.

## 3.1 Current-State Workflow Mapping

```text
1. Tài xế gọi báo sự cố hết pin/thiếu pin.
   ⏱ 2 phút
   🔄 Handoff: tài xế -> điều phối viên

2. Điều phối viên tra vị trí GPS của xe trên dashboard nội bộ.
   ⏱ 2 phút
   🔄 Handoff: tổng đài -> bản đồ nội bộ

3. Điều phối viên mở danh sách trạm sạc để tìm trạm gần và còn trống.
   ⏱ 5 phút
   🔴 Bottleneck: tra cứu thủ công tốn thời gian nhất

4. Điều phối viên soạn tin nhắn hướng dẫn cho tài xế.
   ⏱ 5 phút
   🔴 Bottleneck: dễ sai ngữ cảnh, dễ viết dài dòng

5. Nếu pin quá thấp thì gọi xe cứu hộ/mobile charger.
   ⏱ 1 phút
   🔄 Handoff: điều phối viên -> đội cứu hộ

Tổng cộng: 15 phút/lượt.
```

## 3.2 Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên tại trung tâm vận hành Xanh SM. |
| **2. Current Workflow** | Khi tài xế báo pin thấp, điều phối viên mở bản đồ nội bộ để tìm vị trí xe, tra danh sách trạm sạc còn trống, soạn tin nhắn hướng dẫn và quyết định có gọi cứu hộ hay không. |
| **3. Bottleneck** | Bước tra trạm sạc phù hợp và viết hướng dẫn nháp theo đúng ngữ cảnh, vì phải nhìn nhiều màn hình và dễ sai dưới áp lực. |
| **4. Business Impact** | Mỗi sự cố làm mất khoảng 15 phút điều phối; vào giờ cao điểm, việc chậm phản hồi gây gián đoạn chuyến xe, tăng thời gian chờ của tài xế và làm giảm hiệu suất khai thác đội xe. |
| **5. Success Metric** | Giảm thời gian xử lý từ 15 phút xuống dưới 3 phút; tối thiểu 95% case có pin thấp được phân loại đúng phương án ngay từ lần đầu. |
| **6. Operational Boundary** | AI chỉ được tạo nháp và khuyến nghị; không được tự gửi tin. Nếu pin < 5%, AI không được đề xuất trạm cách xa quá 5km và phải ưu tiên mobile charger. Mọi hành động gửi đi phải có người duyệt. |

## 3.3 Future-State Flow & AI Fit

**AI Fit:** LLM Feature

```text
1. Tài xế báo sự cố.
   -> 2. System lấy GPS + % pin + trạng thái trạm sạc.
   -> 3. AI tạo draft hướng dẫn và đề xuất phương án.
   -> 4. Human reviewer duyệt.
   -> 5. Gửi nháp đã duyệt cho tài xế.
   -> Fallback: nếu AI không tự tin hoặc dữ liệu thiếu, dispatcher xử lý thủ công như hiện tại.
```

**Vai trò của AI:** tạo draft, tóm tắt dữ liệu đầu vào, khuyến nghị phương án.

**Human-in-the-loop:** duyệt trước khi gửi tin và phê duyệt phương án cứu hộ khi pin thấp.

**Fallback:** nếu mô hình trả kết quả mơ hồ, thiếu dữ liệu, hoặc không tuân thủ boundary thì chuyển về quy trình thủ công.

## 3.4 Evaluation

### AI Readiness Checklist
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test.
2. [x] Rủi ro khi AI sai nằm trong tầm kiểm soát qua HITL và fallback.
3. [x] Stakeholders có thể chấp nhận thay đổi vì quy trình hiện tại đang tốn nhiều thời gian.

### Quyết định cuối cùng
**GO**

### Justification
Bài toán này phù hợp để bắt đầu prototype vì scope hẹp, input cấu trúc rõ, và có ranh giới an toàn định lượng được. AI chỉ đóng vai trò tạo nháp và đề xuất, còn quyết định gửi hay không vẫn thuộc con người. So với xây agent tự trị, LLM Feature nhẹ hơn, dễ kiểm soát hơn, và chi phí thử nghiệm thấp hơn. Nếu dữ liệu thực tế cho thấy tỷ lệ case pin thấp quá phức tạp hoặc trạm sạc thay đổi liên tục, nhóm có thể nâng cấp dần sang rule + LLM hybrid thay vì mở rộng agentic loop ngay.
