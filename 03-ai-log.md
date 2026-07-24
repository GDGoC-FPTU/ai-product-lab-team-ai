# 03-ai-log

Trong buổi lab này, tôi dùng AI như một thought-partner để brainstorm các pain point vận hành, thu hẹp scope thành một use case có thể đo được, và viết lại prompt prototype để kiểm tra ranh giới an toàn. AI giúp tôi nhận ra nhanh pattern lặp lại giữa dữ liệu đầu vào, quy tắc quyết định, và phần nào nên để con người duyệt.

Điểm AI làm chưa tốt là nó có xu hướng trả lời quá tự tin và đôi khi đẩy bài toán sang hướng agent hoặc automation rộng hơn mức cần thiết. Nếu không chặn lại, nó dễ bỏ qua các ranh giới như không được gửi trực tiếp hoặc đề xuất phương án không phù hợp khi pin dưới ngưỡng nguy hiểm.

Tôi đã sửa bằng cách siết system prompt, bắt buộc output có tag [DRAFT_ONLY], và định nghĩa rõ trường hợp pin < 5% phải ưu tiên dispatch_mobile_charger thay vì gợi ý trạm sạc xa. Sau đó tôi thêm prompt tấn công để kiểm tra xem mô hình có bị ép vượt boundary hay không. Cách làm này giúp tôi thấy rõ AI hữu ích nhất ở vai trò tạo draft và phản biện, không phải nơi tự ra quyết định cuối cùng.
