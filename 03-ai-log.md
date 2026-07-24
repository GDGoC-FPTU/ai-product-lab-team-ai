# 03 — AI Reflection Log

Trong buổi học này, tôi đã sử dụng AI như một thought-partner để giúp tôi:

- Brainstorm các bài toán thực tế cho các công ty thành viên Vingroup.
- Hoàn thiện phần SCAN và QUICK-ASSESS trong worksheet, chọn ra 5 cơ hội và điền 3 Quick Problem Cards.
- Xây dựng Problem Statement 6-field cho bài toán Xanh SM và xác định Future-State Flow + AI Fit.
- Kiểm tra, phản biện prompt boundary và xác định rủi ro fallback.

## AI giúp gì

AI hỗ trợ tôi nhanh chóng mở rộng ý tưởng bài toán, đặc biệt khi cần phân loại loại lenses (Repetitive, Time-consuming, AI-upgrade, Stakeholder Pain) và viết các thẻ bài toán rõ ràng. Khi tôi cần mô tả workflow hiện tại, AI giúp tôi chuyển từ các khái niệm chung sang các bước tuần tự cụ thể, nêu rõ actor, bottleneck và metric đo thành công.

## AI sai gì

Trong một số phản hồi, AI đã đề xuất các giải pháp quá chung chung và không rõ ràng về ranh giới hoạt động. Ví dụ: nó có thể gợi ý "AI tự động chọn điểm đón" thay vì nhấn mạnh rằng dispatcher phải phê duyệt cuối cùng. Ngoài ra, khi tôi kiểm tra prompt prototype, tôi thấy AI có khả năng bỏ qua yêu cầu giữ tag `[DRAFT_ONLY]` nếu câu hỏi được đặt một cách thách thức.

## Sửa đổi ra sao

Để khắc phục, tôi đã bổ sung ranh giới operational boundary rõ ràng: AI chỉ được phép đề xuất điểm đón/trả, không được tự gán lệnh cho tài xế, và mọi đề xuất phải được dispatcher phê duyệt. Tôi cũng làm rõ trong prompt rằng output phải bắt đầu bằng tag `[DRAFT_ONLY]` khi soạn thông điệp draft. Bằng cách thêm các điều kiện cụ thể này vào prompt và ghi rõ fallback khi dữ liệu thiếu hoặc confidence thấp, tôi ép AI trả về kết quả an toàn hơn và phù hợp với yêu cầu bài toán.

## Bài học

AI rất hữu ích khi làm việc như một đối tác suy nghĩ, nhưng cần kiểm duyệt kỹ càng và bổ sung ranh giới để tránh các đề xuất tự động hoá quá mức. Việc giữ con người là point of control (Human-in-the-loop) là điều quan trọng khi giải quyết bài toán dispatch và trải nghiệm khách hàng.