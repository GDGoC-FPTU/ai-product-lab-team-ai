# 📝 Phase 6 — AI Log & Reflection

**Nhóm / Cá nhân:** Vin Smart Future Engineering Team
**Dự án:** Trợ lý AI Co-pilot điều phối Xanh SM (Sự cố sạc pin)

## 1. Vai trò của AI trong quá trình giải quyết bài Lab
Trong quá trình thực hiện bài Lab hôm nay, nhóm đã tận dụng AI như một "Thought-Partner" (Người đồng hành tư duy) và "Coding Assistant" để tối ưu hóa thời gian:
- **Phase 1 & 2 (Scan & Assess):** Nhóm đã sử dụng AI để brainstorm các pain-point (điểm đau) thực tế trong vận hành của hệ sinh thái Vingroup. AI đã giúp đề xuất nhiều use-case thực tế cao (như bác sĩ Vinmec quá tải tóm tắt bệnh án, hay Xanh SM bị rớt cuốc do tài xế phải tự tra trạm sạc).
- **Phase 3 (Deep-Dive):** Khi vẽ quy trình (Workflow) và thiết lập ranh giới an toàn, nhóm đã đưa thẻ bài toán cho AI và yêu cầu nó phản biện dưới góc nhìn "Trưởng phòng Vận hành khó tính". AI đã chỉ ra lỗ hổng rủi ro cực lớn: Nếu xe còn dưới 5% pin mà vẫn cố bò đến trạm sạc cách xa 5km thì nguy cơ chết máy giữa đường gây tai nạn là rất cao. Nhờ đó nhóm đã bổ sung luật gọi Xe cứu hộ pin lưu động vào Operational Boundary.
- **Phase 4 (Prototyping):** AI hỗ trợ sửa lỗi cấu hình môi trường (file `.env`), tự động bắt lỗi khi thư viện SDK và phiên bản API model cũ bị ngừng hỗ trợ, đồng thời gợi ý chuyển sang model `gemini-3.5-flash` để đảm bảo code hoạt động trơn tru.

## 2. Điểm mạnh và Điểm yếu khi dùng AI
- **Điểm mạnh:** AI đẩy nhanh tốc độ lập trình và debug lên gấp 5 lần. Ở khả năng sáng tạo, AI đóng vai trò làm "Red-team" (Đội tấn công) cực kỳ tốt khi có thể tự sinh ra các Prompt hiểm hóc nhằm lừa mô hình phá vỡ ranh giới an toàn.
- **Điểm yếu:** AI thường thiếu đi bối cảnh thực tế ngoài đời thực (Ví dụ: khoảng cách 5km trên bản đồ AI cho là ngắn, nhưng trong nội thành Hà Nội giờ cao điểm thì tốn rất nhiều thời gian). Do đó, tư duy thực địa của con người vẫn là yếu tố quyết định.

## 3. Bài học cốt lõi rút ra
- Bất kể AI thông minh đến đâu, khi ứng dụng vào nghiệp vụ thực tế có độ rủi ro cao (như an toàn giao thông), tuyệt đối phải thiết lập cơ chế **Human-in-the-loop (HITL)**. Thẻ `[DRAFT_ONLY]` là một kiến trúc bảo vệ ranh giới cực kỳ hiệu quả để AI không tự ý vượt quyền con người.
- Việc kiểm thử độ bền vững của Prompt (Adversarial Testing) là kỹ năng bắt buộc để hệ thống không bị "jailbreak" (bẻ khóa) bởi người dùng hoặc tài xế cố tình trục lợi hệ thống.
