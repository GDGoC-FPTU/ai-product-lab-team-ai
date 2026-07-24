# 🏗️ Phase 3 — DEEP-DIVE: Báo cáo phân tích chuyên sâu (Vin Smart Future)

Dựa trên việc đánh giá 3 Quick Problem Cards ở Phase 2, nhóm quyết định chọn bài toán **"Xanh SM - Xử lý sự cố sạc pin thực địa" (Card #1)** để tiến hành phân tích sâu (Deep-Dive).

---

## 3.1. Current-State Workflow Mapping
Quy trình tiếp nhận và xử lý sự cố hết pin thực địa hiện tại của điều phối viên Xanh SM (Dispatcher):

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn văn bản │
│ gọi sự cố    │ ──→ │ vị GPS xe   │ ──→ │ sạc VinFast  │ ──→ │ hướng dẫn    │
│              │     │              │     │ còn trụ trống│     │ gửi tài xế   │
│ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 5 phút 🔴  │
│ In: App Call │     │ In: Biển số  │     │ In: Vị trí GPS│     │ In: Info Trạm│
│ Out: Log     │     │ Out: Toạ độ  │     │ Out: Địa chỉ │     │ Out: SMS     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Gọi xe cứu   │
                                                               │ hộ (nếu cần) │
                                                               │ Ai: Dispatch │
                                                               │ ⏱ 1 phút     │
                                                               └──────────────┘
🔴 = Bottlenecks (Bước gây chậm và dễ sai sót nhất)
⏱ Tổng thời gian xử lý thủ công: ~15 phút/lượt.
```

---

## 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận trung tâm Xanh SM. |
| **2. Current Workflow** | Khi nhận báo cáo hết pin khẩn cấp từ tài xế, điều phối viên tự tra cứu vị trí xe trên bản đồ, mở hệ thống tìm trụ sạc VinFast trống gần nhất, viết thủ công tin nhắn chỉ dẫn đường đi và gọi xe cứu hộ lưu động nếu pin xe dưới 5%. Có tổng cộng 5 bước thủ công, mất trung bình 15 phút/lượt. |
| **3. Bottleneck** | **Bước 3 & Bước 4 (tốn 10-12 phút):** Thao tác tra cứu thủ công trụ sạc trống (phải khớp với dòng xe VF5/VFe34/VF8...) và tự gõ soạn thảo tin nhắn hướng dẫn đường đi chi tiết tiêu tốn phần lớn thời gian và dễ nhầm lẫn. |
| **4. Business Impact** | Hàng trăm sự cố pin thực địa mỗi ngày gây lãng phí hàng chục giờ làm việc của team điều vận. Làm tăng thời gian chết của xe, rò rỉ doanh thu (do không thể đón khách) và ảnh hưởng trực tiếp đến tâm lý tài xế cũng như trải nghiệm khách hàng chờ xe (SLA rớt). |
| **5. Success Metric** | 1. Giảm tổng thời gian tiếp nhận và phản hồi sự cố từ 15 phút xuống dưới 3 phút/lượt.<br>2. Tỉ lệ hướng dẫn đúng tọa độ và đúng chuẩn trụ sạc đạt trên 98%. |
| **6. Operational Boundary** | **Cho phép:** Truy xuất API tọa độ xe và API trạm sạc VinFast, tự động soạn thảo tin nhắn hướng dẫn định dạng nháp (`[DRAFT_ONLY]`).<br>**CẤM (Boundary):** Tuyệt đối không tự động gửi tin nhắn cho tài xế mà chưa có sự phê duyệt của con người (bắt buộc HITL); không đề xuất trạm sạc cách xa quá 5km nếu mức pin xe < 5%. |

---

## 3.3. Future-State Flow & AI Fit

* **Mức độ phù hợp AI (AI Fit):** Thuộc nhóm **LLM Feature**. Giải pháp AI thực hiện vai trò phân tích thông tin đầu vào, ra quyết định chọn trạm sạc và soạn thảo ngôn ngữ tự nhiên. Bắt buộc có Human-in-the-loop (HITL) phê duyệt cuối cùng, không xây dựng hệ thống Agentic Loop hoàn toàn tự trị do rủi ro an toàn thực địa (xe hết pin giữa cao tốc có thể gây tắc nghẽn).
* **Quy trình tương lai có tích hợp AI (Future-State):**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận tín hiệu│     │ 🔵 Auto-Pull │     │ 🔵 AI Draft  │     │ 🟢 Dispatcher│
│ báo hết pin  │ ──→ │ lấy tọa độ & │ ──→ │ tin nhắn hỗ  │ ──→ │ kiểm tra,    │
│ trên App     │     │ list trạm sạc│     │ trợ / cứu hộ │     │ duyệt & gửi  │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ↩️ Fallback:
                                                               Nếu AI trả kết quả
                                                               lỗi/thiếu, Dispatcher
                                                               bỏ qua AI draft và
                                                               làm thủ công như cũ.
```
