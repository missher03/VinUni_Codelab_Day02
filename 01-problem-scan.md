# 01 — Problem Scan & Quick Problem Cards

> **Lab 02 — AI Product Scoping (Vin Smart Future)** · Phase 1 (SCAN) + Phase 2 (QUICK-ASSESS)
>
> ⚠️ **Ghi chú về số liệu:**
> * **Có nguồn [n]:** bối cảnh thực tế ở mục "Kiểm chứng thực tế" và danh sách nguồn cuối file, tra cứu ngày 09/11/2026.
> * **Không có nguồn:** các con số thời gian từng bước, sản lượng, tỉ lệ lỗi trong Quick Cards là **ước tính giả định**, chưa được xác minh bằng dữ liệu nội bộ Vingroup.
> * Ý tưởng bài toán tham khảo từ `03-inspiration-kit.md` của lab, sau đó được kiểm chứng với nguồn công khai.

---

## 🔍 Phase 1 — SCAN

Quét vận hành các công ty thành viên bằng **4 Lenses**: 🔁 Lặp lại · ⏱ Tốn thời gian · 🚀 AI có thể tốt hơn · 😣 Pain từ người khác.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | **VinFast** (Dịch vụ hậu mãi) | 🚀 AI có thể tốt hơn | Khách mô tả lỗi xe bằng tiếng Việt đời thường qua hotline/app (*"đi qua gờ giảm tốc kêu cụp cụp ở bánh trước"*). Cố vấn dịch vụ phải gọi lại hỏi thêm, tự đoán nhóm lỗi để đặt lịch xưởng → nếu tiếp nhận sai thì xếp sai khoang/sai phụ tùng, khách phải quay lại lần 2. |
| 2 | **Xanh SM** | 😣 Pain từ người khác | Khách nhắn điểm đón mơ hồ (*"cổng sau S2.05, chỗ cây ATM"*) khác vị trí ghim GPS. Tài xế phải gọi điện qua lại 2–3 lần, khách sốt ruột hủy chuyến. |
| 3 | **Vinhomes** | 🔁 Lặp lại | Ban quản lý tòa nhà đọc và chuyển tay các phản ánh gửi qua app Vinhomes Resident (mất nước, hỏng đèn, ồn, rác) đến đúng tổ kỹ thuật/an ninh/vệ sinh. |
| 4 | **Vinpearl** | ⏱ Tốn thời gian | Sales khách đoàn đọc email đặt phòng theo đoàn (Group Booking) của công ty lữ hành — mỗi email một định dạng — rồi nhập tay số phòng, loại phòng, ngày, yêu cầu đặc biệt vào PMS để kiểm tra quỹ phòng. |
| 5 | **Vinmec** | ⏱ Tốn thời gian | Bác sĩ/điều dưỡng soạn hướng dẫn chăm sóc sau xuất viện bằng ngôn ngữ dễ hiểu cho bệnh nhân từ bệnh án điện tử và chỉ định thuốc. |
| 6 | **VinFast** (Hạ tầng sạc) | 🔁 Lặp lại | Kế toán đối soát hằng tuần dữ liệu phiên sạc từ trạm đối tác với hóa đơn gửi về — so khớp từng dòng trên Excel. |

**Phủ lens:** cả 4 lens đều được sử dụng (🚀 #1 · 😣 #2 · 🔁 #3, #6 · ⏱ #4, #5).

### 🔎 Kiểm chứng thực tế (tra cứu nguồn công khai, 09/11/2026)

| # | Điều đã xác minh được | Điều CHƯA xác minh được | Ảnh hưởng tới đánh giá |
|---|---|---|---|
| 1 | VinFast có gần 400 xưởng dịch vụ (cuối 2025) [1]; đặt lịch qua hotline 1900 23 23 89 [2]; cam kết phụ tùng ≤ 24 giờ từ 01/09/2024 [3]. Ngành ô tô (Mỹ): >1/10 lần sửa không đạt ngay lần đầu, nguyên nhân thường từ bước tiếp nhận [4]. | Tỉ lệ tiếp nhận sai và thời gian xử lý phiếu thực tế tại VinFast. | Bài toán có cơ sở trong ngành. Tỉ lệ lỗi giả định hạ từ 25% xuống ~12% cho sát benchmark. |
| 2 | Xanh SM đã có **điểm đón cố định cho sự kiện**: gõ từ khoá "Diễu binh" để chọn nhanh 20 điểm đón/trả [6], và 3 điểm đón chính thức tại concert ở The Global City [7]. | Số liệu về chuyến bị hủy do lệch điểm đón. | Củng cố nhận định **No-AI trước** (điểm đón chọn từ danh sách), vì Xanh SM đang làm đúng hướng này. |
| 3 | Kênh tiếp nhận yêu cầu của Vinhomes gồm hotline 1900 2323 89 nhánh 4, email, và **App Vinhomes Resident**. Hạn xử lý: yêu cầu đơn giản trong **04 ngày làm việc**, yêu cầu phức tạp trong **06 ngày làm việc**, yêu cầu khẩn liên quan an toàn tính mạng thì xử lý ngay [8]. | Số phản ánh/ngày và thời gian phân loại thủ công. | SLA 4–6 ngày cho thấy có dư địa cải thiện, nhưng Vinhomes đã có quy trình phân cấp rõ, nên rule vẫn là bước đầu hợp lý. |
| 4 | Vinpearl nhận yêu cầu báo giá MICE/hội nghị qua trang của từng khách sạn [9]. | Quy trình xử lý email đặt phòng đoàn của đại lý lữ hành. | Giữ nguyên là giả định. |
| 5 | **Vinmec đã dùng AI (DrAid của VinBrain)** từ 08/2024 tại Trung tâm Ung bướu Vinmec Times City, giúp **tiết kiệm đến 80% thời gian tóm tắt hồ sơ bệnh án (5 phút → 1 phút)** [10]. Bài viết không nói riêng về tóm tắt xuất viện. | Việc soạn hướng dẫn xuất viện đã được tự động hoá hay chưa. | Một phần bài toán **đã có giải pháp nội bộ** → càng không nên chọn làm mới. |
| 6 | Mạng lưới V-GREEN có **hơn 150.000 cổng sạc** (03/2025), mục tiêu 500.000 cổng trong 3 năm [11]. | Quy trình và khối lượng đối soát hóa đơn với đối tác. | Quy mô lớn làm bài toán đối soát là có thật, nhưng dữ liệu có cấu trúc nên rule/script vẫn phù hợp hơn LLM. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3: **#1 (VinFast chẩn đoán sơ bộ), #2 (Xanh SM điểm đón), #4 (Vinpearl Group Booking).**

*Lý do không vào top 3:*
* **#3:** phân loại phản ánh có nhãn khá cố định, và Vinhomes đã có quy trình phân cấp mức độ yêu cầu [8] → bộ từ khóa/rule giải quyết được phần lớn.
* **#5:** thuộc mảng y tế, rủi ro an toàn bệnh nhân cao. Vinmec cũng đã có AI tóm tắt hồ sơ bệnh án nội bộ (DrAid) [10].
* **#6:** dữ liệu có cấu trúc → hợp với script đối soát (rule), không cần LLM.

### Card #1 — VinFast: Chẩn đoán sơ bộ từ mô tả lỗi tiếng Việt

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                            │
│                                                                  │
│ Bài toán: Biến mô tả lỗi tiếng Việt đời thường của khách thành   │
│ "phiếu tiếp nhận" có nhóm lỗi sơ bộ + câu hỏi làm rõ, để đặt lịch│
│ xưởng đúng khoang và chuẩn bị đúng phụ tùng ngay lần đầu.        │
│ Công ty thành viên: [x] VinFast (Dịch vụ hậu mãi)                │
│                                                                  │
│ Ai đang đau? Cố vấn dịch vụ (Service Advisor) quá tải cuộc gọi;  │
│ khách hàng phải mang xe đến xưởng 2 lần.                         │
│                                                                  │
│ Workflow thủ công hiện tại (5 bước):                             │
│   1. Tổng đài ghi chú mô tả lỗi của khách (gõ tự do)             │
│   → 2. Chuyển phiếu cho cố vấn dịch vụ tại xưởng                 │
│   → 3. Cố vấn gọi lại khách hỏi thêm triệu chứng                 │
│   → 4. Tra lịch sử xe/bulletin kỹ thuật, đoán nhóm lỗi           │
│   → 5. Đặt lịch khoang + đặt trước phụ tùng                      │
│                                                                  │
│ Bước nào tốn nhất? Bước 3–4 (⏱ ~12 phút/phiếu), và là nơi sinh   │
│ ra tiếp nhận sai (giả định ~12%; Mỹ: >1/10 không đạt lần đầu [4])│
│ AI hỗ trợ ở bước nào? Bước 3–4: chuẩn hoá mô tả → gợi ý nhóm lỗi │
│ + mức khẩn cấp + 3 câu hỏi làm rõ; cố vấn duyệt.                 │
│                                                                  │
│ Metric: Giảm thời gian cố vấn xử lý 1 phiếu từ ~18 ──> dưới 8 ph;│
│ giảm tỉ lệ quay lại lần 2 do tiếp nhận sai đi một nửa.           │
│                                                                  │
│ Quick Architecture: [ ] No AI [x] Rule (lọc an toàn) [x] LLM     │
│                     [ ] Agent                                    │
└──────────────────────────────────────────────────────────────────┘
```

### Card #2 — Xanh SM: Làm rõ điểm đón mơ hồ

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                            │
│                                                                  │
│ Bài toán: Tin nhắn mô tả điểm đón của khách không khớp ghim GPS, │
│ tài xế phải gọi điện nhiều lần để tìm khách.                     │
│ Công ty thành viên: [x] Xanh SM                                  │
│                                                                  │
│ Ai đang đau? Tài xế (mất thời gian, bị đánh giá thấp) và khách   │
│ hàng (chờ lâu → hủy chuyến).                                     │
│                                                                  │
│ Workflow thủ công hiện tại (4 bước):                             │
│   1. Khách đặt xe, ghim GPS + nhắn mô tả tự do                   │
│   → 2. Tài xế đọc tin khi đang lái, tự đối chiếu bản đồ          │
│   → 3. Gọi điện cho khách hỏi lại (1–3 cuộc)                     │
│   → 4. Đến điểm đón / khách hủy                                  │
│                                                                  │
│ Bước nào tốn nhất? Bước 3 (⏱ ~3–4 phút/chuyến bị lệch điểm đón)  │
│ AI hỗ trợ ở bước nào? Bước 2: trích địa danh (tòa, cổng, mốc) từ │
│ tin nhắn, so với ghim GPS, gợi ý điểm đón chuẩn để khách xác nhận│
│                                                                  │
│ Metric: Giảm số cuộc gọi tài xế–khách/chuyến lệch điểm đón từ    │
│ ~2 ──> dưới 0.5; giảm tỉ lệ hủy do không tìm thấy nhau 30%.      │
│                                                                  │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent         │
└──────────────────────────────────────────────────────────────────┘
```

### Card #3 — Vinpearl: Đọc email đặt phòng theo đoàn

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                            │
│                                                                  │
│ Bài toán: Email Group Booking mỗi đại lý một kiểu, sales phải đọc│
│ và nhập tay vào PMS trước khi biết còn đủ phòng hay không.       │
│ Công ty thành viên: [x] Khác: Vinpearl                           │
│                                                                  │
│ Ai đang đau? Nhân viên Sales khách đoàn; đại lý lữ hành chờ báo  │
│ giá lâu.                                                         │
│                                                                  │
│ Workflow thủ công hiện tại (4 bước):                             │
│   1. Nhận email/tệp đính kèm từ đại lý                           │
│   → 2. Đọc, gạch ý: ngày, số phòng từng loại, yêu cầu đặc biệt   │
│   → 3. Nhập tay vào PMS kiểm tra quỹ phòng                       │
│   → 4. Soạn email báo giá/giữ chỗ                                │
│                                                                  │
│ Bước nào tốn nhất? Bước 2–3 (⏱ ~25 phút/email)                   │
│ AI hỗ trợ ở bước nào? Bước 2: trích xuất thành bảng có cấu trúc  │
│ cho sales kiểm tra trước khi nhập.                               │
│                                                                  │
│ Metric: Giảm thời gian từ nhận email đến báo giá từ ~4 giờ ──>   │
│ dưới 1 giờ; sai sót nhập liệu < 2%.                              │
│                                                                  │
│ Quick Architecture: [x] No AI (form chuẩn) [ ] Rule [x] LLM      │
│                     [ ] Agent                                    │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🧪 Stress-test thẻ bài toán (CFO & Trưởng phòng Vận hành phản biện)

Mỗi thẻ được đem ra phản biện theo prompt gợi ý của worksheet: *"3 điểm yếu về logic/metric và vì sao rule-based có thể tốt hơn AI"*.

| Card | Phản biện mạnh nhất | Phản hồi / điều chỉnh |
|---|---|---|
| #1 VinFast | "Chẩn đoán xe là việc của kỹ thuật viên, AI đoán sai thì ai chịu?" · "Cây câu hỏi (decision tree) cố định có làm được không?" | AI **không chẩn đoán**, chỉ gợi ý nhóm lỗi để *đặt lịch*; KTV vẫn chẩn đoán thực tế. Cây câu hỏi cố định chỉ hợp với triệu chứng chuẩn; mô tả tự do của khách (âm thanh, cảm giác, ngữ cảnh) là phần rule làm kém → dùng **Rule cho lọc an toàn + LLM cho chuẩn hoá ngôn ngữ**. |
| #2 Xanh SM | "Sửa UX app (bắt chọn cổng/tòa từ danh sách) rẻ hơn nhiều." | Đúng một phần. Với khu đô thị đã có bản đồ chi tiết, **giải pháp No-AI** (chọn điểm đón từ danh sách) nên làm trước, và Xanh SM đã áp dụng cách này cho các sự kiện lớn [6][7]. LLM chỉ đáng cho phần tin nhắn tự do còn lại. |
| #3 Vinpearl | "Chỉ cần bắt đại lý điền form chuẩn là xong." | Hợp lý với đại lý lớn, khó với đại lý nhỏ/nước ngoài. Ưu tiên **form chuẩn (No AI)** trước; LLM là phương án bổ sung. |

---

## 🗳️ Lựa chọn đưa vào Deep-Dive

**Chọn: Card #1 — VinFast: Chẩn đoán sơ bộ từ mô tả lỗi tiếng Việt.**

* Pain đo được (thời gian xử lý phiếu, tỉ lệ khách quay lại lần 2) và gắn trực tiếp với trải nghiệm khách hàng + năng suất khoang xưởng.
* Bottleneck nằm đúng ở phần **xử lý ngôn ngữ tự nhiên** — thứ rule làm kém, LLM làm tốt.
* Rủi ro kiểm soát được: AI chỉ soạn phiếu nháp, cố vấn dịch vụ duyệt; các tình huống an toàn được rule chặn trước LLM.
* Card #2 và #3 đều có **giải pháp No-AI rẻ hơn** nên làm trước (POI picker, form chuẩn) → chưa phải lúc dùng AI.

Phân tích chi tiết: xem [02-deep-dive-report.md](02-deep-dive-report.md).

---

## 📚 Nguồn tham khảo

Đánh số dùng chung với `02-deep-dive-report.md`. ✅ = đã đọc trực tiếp trang nguồn để đối chiếu; 🔎 = lấy từ kết quả tìm kiếm, chưa đọc toàn văn.

| # | Nguồn | Kiểm tra |
|---|---|---|
| [1] | Tiền Phong, 15/05/2026 — [VinFast thúc đẩy chiến lược hậu mãi toàn cầu, mở rộng mạng lưới dịch vụ](https://tienphong.vn/vinfast-thuc-day-chien-luoc-hau-mai-toan-cau-mo-rong-mang-luoi-dich-vu-va-doi-tac-quoc-te-post1843460.tpo) | ✅ |
| [2] | FPT Shop — [Số tổng đài VinFast là bao nhiêu?](https://fptshop.com.vn/tin-tuc/danh-gia/so-tong-dai-vinfast-187990) | 🔎 |
| [3] | Thanh Tra, 27/08/2024 — [VinFast cam kết cung cấp phụ tùng hậu mãi trong vòng 24 giờ](https://thanhtra.com.vn/thong-tin-doanh-nghiep-49F64E63D/vinfast-cam-ket-cung-cap-phu-tung-hau-mai-tai-viet-nam-trong-vong-24-gio-20491958.html) | ✅ |
| [4] | CBT News, 13/08/2025 — [Fix it right the first time](https://www.cbtnews.com/fix-it-right-the-first-time-customers-have-little-tolerance-for-mistakes/) (dẫn số liệu J.D. Power, Mỹ) | ✅ |
| [5] | Repairer Driven News, 26/01/2021 — [J.D. Power 2021 U.S. Aftermarket Service Index](https://www.repairerdrivennews.com/2021/01/26/j-d-power-auto-repair-and-service-csi-study-highlights-risk-of-redos-lack-of-prompt-greeting/) | ✅ |
| [6] | Green SM — [Xanh SM triển khai 20 điểm đón trả khách xem Diễu binh](https://www.greensm.com/vn-vi/news/xanh-sm-trien-khai-20-diem-don-tra-khach-thuan-tien-xem-dieu-binh-dieu-hanh) | 🔎 |
| [7] | Green SM — [Hướng dẫn sử dụng XanhNow tại The Global City](https://www.greensm.com/vn-vi/news/huong-dan-su-dung-xanhnow-tai-the-global-city) | 🔎 |
| [8] | Vingroup — [Quy trình xử lý khiếu nại, yêu cầu của khách hàng (PDF)](https://ircdn.vingroup.net/storage/Uploads/Phat%20trien%20ben%20vung/Quy%20trinh%20xu%20ly%20khieu%20nai%20yeu%20cau%20cua%20KH.pdf) | ✅ |
| [9] | Vinpearl — [Phòng hội thảo](https://vinpearl.com/vi/phong-hoi-thao) | 🔎 |
| [10] | Vinmec, 12/12/2024 — [Đột phá công nghệ AI tại Vinmec: Tiết kiệm 80% thời gian xử lý hồ sơ y tế](https://www.vinmec.com/vie/bai-viet/dot-pha-cong-nghe-ai-tai-vinmec-tiet-kiem-80-thoi-gian-xu-ly-ho-so-y-te) | ✅ |
| [11] | VnEconomy, 31/03/2025 — [V-Green đặt mục tiêu xây dựng 500.000 cổng sạc ôtô điện](https://vneconomy.vn/v-green-dat-muc-tieu-xay-dung-500-000-cong-sac-oto-dien.htm) | ✅ |

**Giới hạn:** phần lớn nguồn tiếng Việt là bài báo/trang của chính doanh nghiệp (có tính quảng bá); [4], [5] là số liệu thị trường Mỹ. Không tìm thấy số liệu công khai về tỉ lệ lỗi, thời gian xử lý hay số yêu cầu/ngày của các công ty Vingroup.
