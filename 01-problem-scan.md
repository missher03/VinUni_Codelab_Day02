# 01 — Problem Scan & Quick Problem Cards

> **Tên nhóm:** _[điền tên nhóm]_
>
> | # | Họ và tên | Email | Vai trò |
> |---|---|---|---|
> | 1 | _[Họ tên]_ | _[email]_ | Thành viên |
> | 2 | Nguyễn Văn Quốc | quoc.nvsoict@gmail.com | Trưởng nhóm  |
> | 3 | _[Họ tên]_ | _[email]_ | Thành viên |
> | 4 | _[Họ tên]_ | _[email]_ | Thành viên |
>
> **Người thực hiện file này:** Nguyễn Văn Quốc — quoc.nvsoict@gmail.com

---

## 🔍 Phase 1 — SCAN

Quét vận hành của 3 công ty thành viên (Vinmec, Vinhomes, VinFast) bằng 4 lenses. Bối cảnh quy mô được lấy từ nguồn công khai (xem mục Nguồn); các con số thời gian/tổn thất là **ước tính**, cần đo baseline thực tế trước khi triển khai.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | **Vinmec** | Pain từ người khác | **CareFollow — theo dõi sau điều trị:** câu hỏi/triệu chứng của bệnh nhân sau xuất viện được xử lý theo thứ tự đến trước, ca cần bác sĩ bị chậm; bệnh nhân quên lời dặn. Giải pháp gồm 3 phần: (a) sắp xếp ưu tiên câu hỏi P0–P3 và chuyển đúng người, (b) diễn giải lời dặn bác sĩ thành kế hoạch chăm sóc dễ hiểu, (c) chăm sóc chủ động (nhắc thuốc theo đơn, hỏi thăm theo mốc, nhắc tái khám). |
| 2 | **Vinmec** | Lặp lại | **Hỏi đáp chuẩn bị trước khám:** hotline trả lời lặp lại cùng các câu hỏi (nhịn ăn trước xét nghiệm, chuẩn bị nội soi, giấy tờ BHYT, giá gói khám). AI chỉ trả lời từ tài liệu đã duyệt, không tư vấn bệnh. |
| 3 | **Vinhomes** | Lặp lại | **Phân loại & chuyển phản ánh cư dân** trên app Vinhomes Resident (mất nước, thang máy, ồn, vệ sinh…) đến đúng bộ phận, gắn mức khẩn và hạn xử lý theo Quy định xử lý khiếu nại của Vinhomes. |
| 4 | **Vinhomes** | Tốn thời gian | **Duyệt hồ sơ đăng ký thi công nội thất:** kỹ thuật BQL đối chiếu thủ công hạng mục và bản vẽ với nội quy tòa nhà; hồ sơ thiếu phải bổ sung nhiều vòng. |
| 5 | **VinFast** | AI có thể tốt hơn | **Chẩn đoán sơ bộ trước khi xe vào xưởng:** mô tả lỗi bằng tiếng Việt của khách chỉ được ghi tay; xe đến xưởng mới chẩn đoán và đặt phụ tùng, khách phải quay lại lần 2. |

**Độ phủ lenses:** cả 4 lenses đều được sử dụng (Pain từ người khác ×1, Lặp lại ×2, Tốn thời gian ×1, AI có thể tốt hơn ×1).

**Những hướng đã cân nhắc nhưng loại khỏi danh sách:**
- *Chatbot hỏi đáp tiện ích cho cư dân Vinhomes* — đã có trợ lý ảo ViVi trên app Resident, làm lại không tạo giá trị mới.
- *AI đọc ảnh chẩn đoán tại Vinmec* — Vinmec/VinBrain đã triển khai (DrAid, AI Trợ lý bác sĩ).
- *Nhắc lịch tái khám / uống thuốc đơn lẻ* — dữ liệu có cấu trúc, rule-based là đủ, không phải bài toán AI (được gộp làm phần "Rule" trong CareFollow).

---

## 🃏 Phase 2 — QUICK-ASSESS

Chọn **top 3** từ danh sách SCAN: **#1 Vinmec CareFollow**, **#5 VinFast Pre-Diagnosis**, **#4 Vinhomes Duyệt hồ sơ thi công** — 3 công ty khác nhau, 3 lenses khác nhau. Hai bài #2 và #3 không làm thẻ vì #3 cùng dạng "phân loại → chuyển người" với #1, còn #2 có phạm vi hẹp và giá trị thấp hơn.

```text
┌────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 — Vinmec CareFollow                      │
│                                                                │
│ Bài toán (1 câu): Câu hỏi/triệu chứng của bệnh nhân sau điều   │
│   trị được xử lý theo thứ tự đến trước, nên ca cần bác sĩ bị   │
│   chìm giữa câu hỏi hành chính; bệnh nhân về nhà quên lời dặn. │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes     │
│                     [x] Vinmec   [ ] Khác                      │
│                                                                │
│ Ai đang đau (Actor)? Điều dưỡng/CSKH tiếp nhận (đọc và chuyển  │
│   thủ công), bác sĩ điều trị (bị hỏi cả việc không cần bác     │
│   sĩ), bệnh nhân sau xuất viện (chờ lâu, lo lắng).             │
│                                                                │
│ Workflow thủ công hiện tại:                                    │
│   1. Bệnh nhân nhắn app MyVinmec / gọi hotline                 │
│   ──> 2. CSKH đọc, hỏi lại thông tin, tra hồ sơ khám           │
│   ──> 3. Chuyển điều dưỡng khoa (qua điện thoại/nhóm chat)     │
│   ──> 4. Điều dưỡng tự trả lời hoặc chờ bác sĩ rảnh            │
│   ──> 5. Phản hồi bệnh nhân                                    │
│                                                                │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (~8-12 phút/lượt,    │
│   ước tính). Hàng đợi FIFO: ca sốt sau mổ có thể đứng sau hàng │
│   chục câu hỏi đổi lịch.                                       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: Rule bắt từ    │
│   khóa cảnh báo đỏ → P0 ngay; LLM phân loại P1-P3, tóm tắt câu │
│   hỏi kèm đơn thuốc + lời dặn, chuyển đúng người (bác sĩ /     │
│   điều dưỡng / CSKH).                                          │
│                                                                │
│ Đo thành công bằng gì (Metric có số)?                          │
│   - Recall ca P0/P1 ≥ 98% trên bộ test đã gán nhãn             │
│   - Ca P1 đến tay bác sĩ: từ ~2-4 giờ ──> dưới 30 phút         │
│   - Giảm 40% câu hỏi hành chính (P3) phải qua điều dưỡng       │
│                                                                │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent    │
│   (Rule cảnh báo đỏ + LLM phân loại; điều dưỡng điều phối      │
│     duyệt hàng đợi)                                            │
└────────────────────────────────────────────────────────────────┘
```

```text
┌────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2 — VinFast Pre-Diagnosis                  │
│                                                                │
│ Bài toán (1 câu): Mô tả lỗi xe bằng tiếng Việt của khách chỉ   │
│   được ghi tay khi đặt lịch, xe đến xưởng mới chẩn đoán và mới │
│   đặt phụ tùng, khiến khách phải quay lại hoặc xe nằm xưởng    │
│   chờ hàng.                                                    │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes     │
│                     [ ] Vinmec   [ ] Khác                      │
│                                                                │
│ Ai đang đau (Actor)? Cố vấn dịch vụ (ghi nhận, hỏi lại), kỹ    │
│   thuật viên (chẩn đoán từ đầu), chủ xe (mất 2 lượt đến        │
│   xưởng).                                                      │
│                                                                │
│ Workflow thủ công hiện tại:                                    │
│   1. Khách gọi hotline / đặt lịch trên app, mô tả lỗi          │
│   ──> 2. Cố vấn dịch vụ ghi tự do vào phiếu hẹn                │
│   ──> 3. Xe đến xưởng, kỹ thuật viên chẩn đoán                 │
│   ──> 4. Thiếu phụ tùng → đặt hàng, hẹn khách quay lại         │
│   ──> 5. Sửa chữa, giao xe                                     │
│                                                                │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (~30-45 phút chẩn    │
│   đoán + 1-3 ngày chờ phụ tùng, ước tính).                     │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2: LLM chuẩn hóa mô │
│   tả, gợi ý top-3 nhóm lỗi (gầm / điện / pin / phần mềm), câu  │
│   hỏi cần hỏi thêm khách, phụ tùng dự kiến. Rule: triệu chứng  │
│   an toàn (phanh, lái, cảnh báo pin/nhiệt) → chuyển cứu hộ     │
│   ngay, không qua LLM.                                         │
│                                                                │
│ Đo thành công bằng gì (Metric có số)?                          │
│   - ≥ 70% ca: lỗi thật nằm trong top-3 nhóm lỗi AI gợi ý       │
│   - Giảm 30% số ca khách phải quay lại lần 2 vì thiếu phụ tùng │
│   - 100% triệu chứng an toàn được chuyển cứu hộ/kỹ thuật       │
│                                                                │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent    │
│   (AI chỉ gợi ý; kỹ thuật viên quyết định chẩn đoán cuối cùng) │
└────────────────────────────────────────────────────────────────┘
```

```text
┌────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3 — Vinhomes Duyệt hồ sơ thi công nội thất │
│                                                                │
│ Bài toán (1 câu): Hồ sơ đăng ký thi công nội thất của cư dân   │
│   phải được kỹ thuật BQL đối chiếu thủ công với nội quy, hồ sơ │
│   thiếu phải bổ sung nhiều vòng làm cư dân chờ nhiều ngày mới  │
│   được thi công.                                               │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes     │
│                     [ ] Vinmec   [ ] Khác                      │
│                                                                │
│ Ai đang đau (Actor)? Kỹ thuật viên BQL tòa nhà (duyệt thủ      │
│   công), lễ tân BQL (nhận hồ sơ), cư dân và nhà thầu (chờ giấy │
│   phép).                                                       │
│                                                                │
│ Workflow thủ công hiện tại:                                    │
│   1. Cư dân nộp đơn + mô tả hạng mục + bản vẽ + thông tin nhà  │
│     thầu                                                       │
│   ──> 2. Lễ tân BQL kiểm tra đủ giấy tờ                        │
│   ──> 3. Kỹ thuật đối chiếu hạng mục với nội quy (kết cấu, mặt │
│     đứng, PCCC, vị trí điều hòa, giờ thi công)                 │
│   ──> 4. Yêu cầu bổ sung hoặc duyệt, thu tiền cọc              │
│   ──> 5. Cấp giấy phép thi công                                │
│                                                                │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (~30-45 phút/hồ sơ;  │
│   trung bình 2-3 vòng bổ sung, kéo dài 3-5 ngày, ước tính).    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: Rule kiểm đủ   │
│   giấy tờ và giờ/ngày thi công; LLM đọc mô tả hạng mục, gắn cờ │
│   hạng mục rủi ro (đục tường, thay cửa sổ, dời cục nóng điều   │
│   hòa) và soạn sẵn danh sách cần bổ sung gửi cư dân một lần.   │
│                                                                │
│ Đo thành công bằng gì (Metric có số)?                          │
│   - Thời gian duyệt: từ 3-5 ngày ──> ≤ 1 ngày làm việc         │
│   - Số vòng bổ sung trung bình: từ 2-3 ──> ≤ 1                 │
│   - 0 hạng mục kết cấu bị bỏ sót trên bộ hồ sơ test            │
│                                                                │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent    │
│   (AI không tự cấp phép; kỹ thuật viên BQL ký duyệt cuối cùng) │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧐 Tự phản biện (góc nhìn CFO / Trưởng phòng Vận hành)

| Thẻ | Điểm yếu | Vì sao rule-based có thể đủ? | Cách xử lý |
|---|---|---|---|
| **#1 CareFollow** | Rủi ro lớn nhất là *under-triage* (xếp ca khẩn xuống thấp). Chưa có dữ liệu câu hỏi thật đã gán nhãn để chứng minh recall 98%. | Bộ từ khóa cảnh báo đỏ có thể bắt được phần lớn ca P0. | Giữ Rule làm lớp đầu tiên (P0 không qua LLM); LLM chỉ xử lý phần mô tả mơ hồ; ngưỡng bất đối xứng (phân vân → chọn mức cao hơn); ưu tiên lâm sàng luôn đứng trước hạng khách hàng. |
| **#2 VinFast** | Metric "top-3 chứa lỗi thật" phụ thuộc chất lượng mô tả của khách; khách mô tả mơ hồ thì AI cũng mơ hồ. | Cây câu hỏi cố định (decision tree) cho các lỗi phổ biến có thể làm được một phần. | Dùng LLM để sinh câu hỏi bổ sung thay vì chỉ đoán; đo trên lịch sử phiếu sửa chữa trước khi cam kết số. |
| **#3 Vinhomes** | Bản vẽ có nhiều định dạng (ảnh chụp tay, PDF), LLM đọc bản vẽ chưa đáng tin. | Checklist giấy tờ và giờ thi công hoàn toàn làm được bằng rule. | Giai đoạn đầu chỉ cho AI đọc **mô tả chữ** và gắn cờ; bản vẽ vẫn do kỹ thuật viên xem. |

---

## 🗳️ Đề xuất bài toán cho Deep-Dive nhóm

Đề xuất chọn **Card #1 — Vinmec CareFollow**, tập trung vào **module sắp xếp ưu tiên** (là lõi: mọi câu hỏi từ module diễn giải lời dặn và chăm sóc chủ động đều đổ về đây).

- **Vì sao chọn:** nỗi đau thật, tác động trực tiếp đến an toàn người bệnh; cho phép so sánh đủ Rule / LLM / Agent trong cùng một sản phẩm; ranh giới và Fallback rõ ràng.
- **Vì sao không chọn #2 (VinFast):** cần dữ liệu phiếu sửa chữa nội bộ để chứng minh metric, khó đánh giá trong phạm vi lab.
- **Vì sao không chọn #3 (Vinhomes):** giá trị tốt nhưng tác động hẹp (chỉ nhóm cư dân đang thi công); phù hợp làm dự án nhỏ sau.

---

## 📚 Nguồn tham khảo bối cảnh

- Vinmec — Tầm nhìn và sứ mệnh (quy mô: 1.505 giường, 5,7 triệu lượt ngoại trú, 17 bệnh viện & phòng khám): https://www.vinmec.com/vie/tam-nhin-va-su-menh
- Vinmec — Đột phá AI tiết kiệm 80% thời gian xử lý hồ sơ (DrAid tại Trung tâm Ung bướu Times City): https://www.vinmec.com/vie/bai-viet/dot-pha-cong-nghe-ai-tai-vinmec-tiet-kiem-80-thoi-gian-xu-ly-ho-so-y-te
- Medpro — Hướng dẫn đặt lịch khám Vinmec (tổng đài gọi lại xác nhận lịch): https://medpro.vn/tin-tuc/huong-dan-dat-lich-kham-vinmec-nhanh-chong-va-tien-loi
- Vinhomes — Quy định xử lý khiếu nại/yêu cầu của khách hàng (cập nhật 05/05/2025): https://market-files.vinhomes.vn/public-mngt/d/279c2b19-3e4d-406a-950b-be78102f50fa
- Mytour — Trợ lý ảo ViVi trên Vinhomes Resident (~350.000 cư dân, 26 khu đô thị tại thời điểm ra mắt): https://mytour.vn/vi/blog/bai-viet/vivi-tu-tro-ly-ao-tren-o-to-den-quan-gia-ao-cho-nha-thong-minh.html
- VnReview — Mạng lưới xưởng dịch vụ VinFast (115.916 xe bàn giao 6 tháng đầu 2026, gần 450 xưởng): https://vnreview.vn/threads/sau-ha-tang-sac-vinfast-hoa-giai-xong-noi-lo-cuoi-cung-cua-chu-xe-dien.87895/
