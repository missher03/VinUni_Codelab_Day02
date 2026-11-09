# 03 — AI Log & Reflection

> **Lab 02 — AI Product Scoping (Vin Smart Future)** · Phase 6 (REFLECTION, cá nhân)
>
> **Người viết:** Nguyễn Văn Quốc · branch `nguyenvanquoc` · Trưởng nhóm, Nhóm 50
>
> **Công cụ AI đã dùng:**
> * **Claude** (claude.ai, có quyền đọc/sửa thư mục repo trên máy): trợ lý đồng hành để đọc đề, tìm và phản biện bài toán, viết nháp file `.md`, viết code prototype, hỗ trợ thao tác Git.
> * **OpenAI gpt-4o-mini / Gemini 2.5 Flash**: mô hình được stress-test trong `starter-code/prompt_prototype.py` (script hỗ trợ cả hai, đọc khóa từ `.env`).

---

## 1. Nhật ký tương tác chính

| # | Tôi yêu cầu AI | AI làm được gì | Tôi kiểm tra / điều chỉnh gì |
|---|---|---|---|
| 1 | Đọc đề (README, worksheet, file PDF của lab) và cho biết nhiệm vụ cụ thể | Tóm tắt các phase, 4 file nộp, cách chia điểm cá nhân/nhóm, quy tắc Git (code `.py` chỉ ở branch cá nhân, `.md` mới lên `main`) | Tôi yêu cầu **xác định bài toán trước** rồi mới làm file, thay vì để AI viết luôn cả bài. |
| 2 | Đi sâu vào lĩnh vực Vinmec, rồi Vinhomes và VinFast | Đề xuất nhiều bài toán; ý tưởng chính là **CareFollow** (theo dõi bệnh nhân sau điều trị) | Tôi yêu cầu AI **đánh giá và phản biện** ý tưởng CareFollow, sau đó thu hẹp: chỉ đi sâu **module sắp xếp ưu tiên P0–P3**, hai module còn lại làm đầu vào và lộ trình mở rộng. |
| 3 | Kiểm tra các ý tưởng có bị trùng với thứ Vingroup đã làm không | Tra nguồn công khai, phát hiện Vinhomes đã có trợ lý ảo **ViVi**, Vinmec/VinBrain đã có **DrAid** đọc ảnh chẩn đoán | Loại 2 hướng này khỏi bảng SCAN và ghi lý do loại, kèm nguồn trong `01-problem-scan.md`. |
| 4 | Hoàn thành Phase 1–2 và ghi vào worksheet | Viết bảng SCAN 5 bài toán (đủ 4 lenses) và 3 Quick Problem Cards | Tôi chọn **#1 CareFollow, #5 VinFast, #4 Vinhomes** (3 công ty, 3 lenses khác nhau). Mọi số thời gian ghi rõ là **ước tính**. |
| 5 | Hoàn thiện `prompt_prototype.py`, đọc API key từ `.env` | Viết `SYSTEM_PROMPT` (thẻ `[DRAFT_ONLY]`, quy tắc pin < 5% → `dispatch_mobile_charger`), `evaluate_prompt()` và 3 adversarial test | Giữ `.env` trong `.gitignore`, **không đưa key vào code**. Thêm `sys.stdout.reconfigure(encoding="utf-8")` để in tiếng Việt trên Windows không lỗi. |
| 6 | Phân công nhóm 5 người (`04_task.md`) | Soạn bảng phân công theo gate, tiêu chí "xong khi", **bảng số liệu chung** | Tôi đổi vai trò giữa các thành viên cho hợp với từng người. Bảng số liệu chung bắt cả nhóm dùng cùng một bộ số, tránh mỗi phần một số. |
| 7 | Kiểm tra branch của các thành viên, tìm phần còn thiếu | Liệt kê ai đã nộp gì, phần nhóm nào chưa có, file nào sai đề tài | Phát hiện `02-deep-dive-report.md` và sơ đồ trên branch `lethanhnguyen` viết về **VinFast**, không phải CareFollow → không dùng khi ghép vào `main`. |
| 8 | Viết 3.2 Problem Statement và ghép `02-deep-dive-report.md` | Soạn bảng 6 trường, chép phần 3.3a của Nguyên, chuyển sơ đồ của Khánh thành bảng chữ | Đối chiếu từng con số với Bảng số liệu chung và sơ đồ 3.1. |
| 9 | Thao tác Git: đổi tài khoản, push, khôi phục file | Hướng dẫn cấu hình `user.name/email` riêng cho repo, gỡ thông tin đăng nhập cũ, tạo lại `01-problem-scan.md` từ worksheet khi tôi lỡ xóa | Xem mục 3 — đây là chỗ tôi phải tự kiểm tra kỹ nhất. |

---

## 2. AI đã giúp gì (cụ thể)

* **Biến ý tưởng mơ hồ thành bài toán có ranh giới.** Ý tưởng ban đầu của CareFollow rất rộng (trả lời bệnh nhân, nhắc thuốc, giải thích lời dặn). Khi bị hỏi "AI được phép làm gì, tuyệt đối không được làm gì", bài toán thu lại thành: **AI chỉ phân loại, tóm tắt và chuyển người; không tư vấn y khoa, không tự nhắn bệnh nhân**.
* **Phản biện "Problem first, AI second".** AI chỉ ra phần nhắc lịch tái khám/uống thuốc là dữ liệu có cấu trúc, **Rule là đủ**, không cần LLM. Phần thật sự cần LLM chỉ là hiểu câu mô tả mơ hồ của bệnh nhân.
* **Kiến trúc an toàn cho y tế.** Ý tưởng **Rule bắt cảnh báo đỏ đi trước LLM** và **"phân vân thì chọn mức cao hơn"** giúp giảm rủi ro xếp ca nặng xuống thấp (under-triage).
* **Đối chiếu với đề và code chấm.** Nhờ đọc `autograder.py`, tôi biết system prompt phải chứa các từ khóa `draft_only`, `5%`, `dispatch_mobile_charger` và cần ít nhất 2–3 adversarial test.
* **Tiết kiệm thời gian ở việc lặp lại:** soạn bảng, khung báo cáo, kiểm tra branch của 4 thành viên.

---

## 3. AI sai / hallucination ở đâu và tôi đã xử lý thế nào

| # | Chỗ AI sai hoặc có thể gây sai | Tôi phát hiện thế nào | Cách sửa / ranh giới đặt ra |
|---|---|---|---|
| 1 | **Số liệu không có nguồn.** Khi viết Quick Card và Business Impact, AI đưa ra các con số thời gian (8–12 phút/lượt, 2–4 giờ chờ) như thể là thật. | Hỏi lại "số này lấy ở đâu?" → không có nguồn nội bộ Vinmec. | Ghi rõ **"ước tính"** ở mọi chỗ, gom vào một Bảng số liệu chung, và thêm **cách đo baseline**. Chỉ số quy mô Vinmec mới ghi nguồn. |
| 2 | **AI kết luận sai khi review code.** Lúc kiểm tra tiến độ, AI nói `prompt_prototype.py` của tôi chỉ dùng OpenAI nên sẽ mất điểm tiêu chí "dùng Gemini SDK". | Khi đọc lại code, script có nhánh dùng `google-genai` cho Gemini. | AI tự đính chính. Bài học: **yêu cầu AI dẫn dòng code cụ thể** trước khi tin một nhận xét kiểu "thiếu cái này". |
| 3 | **AI suýt báo sai lỗi số liệu của đồng đội.** AI cho rằng sơ đồ của Khánh (tổng 24 phút) mâu thuẫn với bảng chung (8–12 phút). | Đối chiếu từng bước: 8–12 phút chỉ là khâu CSKH + chuyển (B2 + B3 = 9 phút), còn 24 phút là cả quy trình tới bác sĩ. | Không bắt Khánh sửa; trong báo cáo ghi rõ hai con số đo hai phạm vi khác nhau. |
| 4 | **Thao tác Git sai tài khoản.** Commit/push bằng tài khoản `quocnv20214926` thay vì `missher03` (bị lỗi 403, và có lúc commit mang sai tên tác giả). | GitHub báo lỗi quyền / lịch sử commit hiện sai tên. | Đặt `git config user.name/email` **riêng cho từng repo**, xóa thông tin đăng nhập cũ trong Windows Credential Manager, chỉ push lên **branch cá nhân**. |
| 5 | **Rủi ro lộ dữ liệu.** Khi làm việc với `.env` và file nộp. | Chủ động kiểm tra trước mỗi lần commit. | Quy tắc cho cả nhóm: **không dùng `git add .`**, chỉ add đúng file mình làm; `.env` nằm trong `.gitignore`; không đưa file `.py` lên `main`. |

---

## 4. Ranh giới tôi đặt cho AI khi làm việc

1. **AI không chọn bài toán thay tôi.** AI đề xuất và phản biện; tôi quyết định chọn CareFollow và phạm vi module sắp xếp ưu tiên.
2. **Không bịa dữ liệu.** Số không có nguồn phải ghi "ước tính"; không tạo số liệu khảo sát hay phỏng vấn giả.
3. **Không làm thay phần của thành viên khác.** AI chỉ kiểm tra và liệt kê phần còn thiếu; 3.3b và Phase 5 vẫn do Cường và Quân viết.
4. **Mọi thao tác Git lên `main` do tôi tự chạy** sau khi đã xem `git status` và `git log`.

---

## 5. Bài học rút ra

* AI rất mạnh ở việc **đặt câu hỏi phản biện và dựng khung**, nhưng hay **nói chắc chắn ở những chỗ chưa kiểm chứng** (số liệu, nhận xét về code). Tôi phải luôn hỏi "dựa vào đâu?" và tự mở file gốc ra xem.
* Với sản phẩm y tế, câu hỏi quan trọng nhất không phải "AI làm được gì" mà là **"AI sai thì ai chịu, phát hiện thế nào"** — vì vậy Rule, HITL và Fallback quan trọng ngang LLM.
* Khi làm nhóm, một **bảng số liệu chung** và quy tắc Git rõ ràng giúp tránh phần lớn lỗi ghép báo cáo.
