# Phase 6 — AI Log & Reflection

## Bối cảnh sử dụng AI

Tôi dùng AI làm thought-partner để triển khai mục 3.3 cho CareFollow — module sắp xếp ưu tiên và chuyển câu hỏi sau điều trị của Vinmec đến đúng bộ phận. Tôi cung cấp bối cảnh bài toán, các mức P0–P3 và ranh giới quan trọng: AI không tư vấn y khoa, không tự trả lời bệnh nhân, chỉ hỗ trợ phân loại thông tin và routing.

## AI đã giúp gì?

AI giúp tôi chuyển yêu cầu thành một luồng tương lai có các bước tiếp nhận, kiểm tra dấu hiệu khẩn cấp, trích xuất/tóm tắt bằng LLM, áp dụng quy tắc routing, nhân viên rà soát và theo dõi SLA. AI cũng giúp làm rõ rằng đây không nên là một agent tự hành: rule/state-machine giữ vai trò quyết định điều phối, còn LLM chỉ hỗ trợ hiểu tin nhắn tiếng Việt tự do. Bảng routing P0–P3 và các nhánh fallback giúp tôi nhìn thấy những trường hợp quy trình thường dễ bỏ sót, như không ghép được hồ sơ hoặc dịch vụ AI bị lỗi.

## Điểm AI có thể sai hoặc tạo cảm giác chắc chắn quá mức

AI có thể diễn giải ví dụ “vết mổ sưng đỏ, sốt 38,5 độ” thành một mức ưu tiên cụ thể. Nhưng không có quy tắc lâm sàng chính thức hay bác sĩ xác nhận trong bài làm này để chứng minh mức đó là đúng. Vì vậy, ví dụ P1 trong báo cáo chỉ minh họa yêu cầu không để tin nhắn bị xếp sau câu hỏi hành chính; nó **không phải kết luận y khoa hoặc hướng dẫn triage đã được Vinmec phê duyệt**.

Ngoài ra, AI có thể đề xuất ngưỡng độ tin cậy, cách gán tuyến hoặc quy trình escalation nghe hợp lý nhưng chưa có dữ liệu vận hành, baseline hay xác nhận từ các khoa. Nếu trình bày các đề xuất đó như quy trình hiện hành của Vinmec, người đọc có thể nhầm giả định thiết kế với sự thật đã kiểm chứng.

## Tôi đã điều chỉnh ranh giới và cách dùng kết quả

- Giới hạn LLM ở việc trích xuất và tóm tắt trung tính; không chẩn đoán, đề xuất điều trị hay nhắn tin cho bệnh nhân.
- Đặt quy tắc khẩn cấp do nhân sự lâm sàng có thẩm quyền phê duyệt trước bước LLM; khi có dấu hiệu khẩn cấp thì cảnh báo và chuyển cấp cứu ngay, không chờ mô hình.
- Để rule engine áp dụng mức ưu tiên và tuyến nhận; nhân viên y tế/điều phối rà soát, có thể sửa hoặc nâng mức ưu tiên. AI không được tự hạ mức tín hiệu nguy hiểm.
- Khi thiếu dữ liệu, lỗi mô hình, kết quả không chắc chắn hoặc không chuyển được ticket, giữ nguyên tin nhắn, báo trạng thái lỗi và chuyển rà soát thủ công thay vì tạo kết quả “thành công” giả.
- Ghi rõ các ngưỡng chấp nhận và độ chính xác cần được xác lập trong pilot; không tuyên bố hệ thống đã được kiểm thử lâm sàng hay đạt chỉ số chất lượng.

## Điều tôi học được

AI hữu ích để cấu trúc vấn đề và gợi ý các nhánh xử lý, nhưng câu trả lời mạch lạc không đồng nghĩa với đúng về chuyên môn. Với bài toán y tế, tôi cần phân biệt rõ hỗ trợ vận hành với quyết định lâm sàng, kiểm tra mọi quy tắc ưu tiên với người có thẩm quyền và thử nghiệm trên dữ liệu đã được gán nhãn trước khi triển khai. Trong phạm vi buổi làm này, tôi mới hoàn thiện đề xuất flow; **chưa chạy thử LLM, chưa dùng dữ liệu bệnh nhân và chưa xác nhận quy tắc với Vinmec**, nên đây là thiết kế ban đầu chứ không phải giải pháp đã được chứng minh.
