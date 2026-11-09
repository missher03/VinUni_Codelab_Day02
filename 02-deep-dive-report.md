# 02 — Deep-Dive Report: Vinmec CareFollow — Sắp xếp ưu tiên câu hỏi bệnh nhân sau điều trị

> **Lab 02 — AI Product Scoping (Vin Smart Future)** · Phase 3 (DEEP-DIVE) + Phase 5 (EVALUATE) · **Nhóm 50**
>
> | # | Họ và tên | Email | Branch | Phụ trách trong báo cáo này |
> |---|---|---|---|---|
> | 1 | Nguyễn Văn Quốc (Trưởng nhóm) | quoc.nvsoict@gmail.com | `nguyenvanquoc` | Mở bài, 3.2 Problem Statement, ghép báo cáo |
> | 2 | Lê Quốc Khánh | _[bổ sung]_ | `lequockhanh` | 3.1 Current-State Workflow + `04-workflow-diagram.png` |
> | 3 | Lê Thanh Nguyên | _[bổ sung]_ | `lethanhnguyen` | 3.3a AI-Fit Matrix |
> | 4 | Nguyễn Mạnh Cường | _[bổ sung]_ | `nguyen-manh-cuong` | 3.3b Future-State Flow |
> | 5 | Nguyễn Minh Quân | minhquannm0905@gmail.com | `nguyen-minh-quan` | Phase 5 Evaluate & Decision |
>
> ⚠️ **Ghi chú về số liệu:** số liệu quy mô Vinmec có nguồn [1]. Mọi con số về thời gian xử lý, tỉ lệ và mục tiêu là **ước tính của nhóm** (Bảng số liệu chung trong `04_task.md`), **chưa đo trên dữ liệu thật của Vinmec**. Cách đo baseline được nêu ở mục 3.2.

---

## 🎯 Tóm tắt bài toán

Sau khi xuất viện hoặc sau thủ thuật, bệnh nhân Vinmec gửi câu hỏi và triệu chứng qua app MyVinmec hoặc hotline. Hiện tại mọi tin nhắn được xử lý **theo thứ tự đến trước**: một ca *"vết mổ sưng đỏ, sốt 38,5 độ"* có thể đứng sau hàng chục câu hỏi đổi lịch tái khám hay xin hóa đơn. Vinmec có quy mô lớn (17 bệnh viện và phòng khám, khoảng 5,7 triệu lượt khám ngoại trú [1]), nên lượng câu hỏi sau điều trị là đáng kể.

**CareFollow** là trợ lý theo dõi sau điều trị gồm 3 module: (a) sắp xếp ưu tiên câu hỏi, (b) diễn giải lời dặn bác sĩ, (c) chăm sóc chủ động. Báo cáo này đi sâu vào **module (a) — Sắp xếp ưu tiên (Triage & Routing)**, vì mọi câu hỏi phát sinh từ hai module còn lại đều đổ về đây:

> AI đọc tin nhắn, gắn hồ sơ, xếp mức ưu tiên **P0–P3**, tóm tắt và **chuyển đúng người** (cấp cứu / bác sĩ / điều dưỡng / CSKH). **AI không đưa lời khuyên y khoa và không tự trả lời bệnh nhân.**

**Quy ước mức ưu tiên (dùng chung toàn báo cáo):** **P0** khẩn cấp (xử lý ngay) · **P1** cần bác sĩ (< 2 giờ) · **P2** điều dưỡng (trong ngày) · **P3** CSKH (< 24 giờ).

---

## 🏗️ 3.1. Current-State Workflow Mapping

*Phụ trách: Lê Quốc Khánh* · Sơ đồ trực quan: [04-workflow-diagram.png](04-workflow-diagram.png)

```text
 Bệnh nhân nhắn app MyVinmec / gọi hotline: "mổ ruột thừa 5 ngày, vết mổ rỉ nước vàng"
        │
        ▼
┌──────────────────┐      ┌──────────────────┐ 🔄H1 ┌──────────────────┐ 🔄H2 ┌──────────────────┐      ┌──────────────────┐
│ B1. Gửi câu hỏi/ │ ───▶ │ B2. CSKH đọc,    │ ───▶ │ B3. Điều dưỡng   │ ───▶ │ B4. Bác sĩ xem   │ ───▶ │ B5. CSKH phản hồi│
│ triệu chứng      │      │ hỏi lại, tra hồ  │      │ đánh giá sơ bộ,  │      │ triệu chứng, đưa │      │ bệnh nhân        │
│ Ai: Bệnh nhân    │      │ sơ   🔴          │      │ phân loại mức độ │      │ hướng xử lý  🔴  │      │ Ai: CSKH         │
│ ⏱ 2 phút         │      │ ⏱ 5 phút         │      │ ⏱ 4 phút         │      │ ⏱ 10 phút        │      │ ⏱ 3 phút         │
└──────────────────┘      └──────────────────┘      └──────────────────┘      └──────────────────┘      └──────────────────┘
                                                                                ▲
                                     Hàng đợi FIFO: ca P1 chờ ~2–4 giờ mới tới bác sĩ (ước tính)
```

| # | Bước | Actor | Công cụ | ⏱ Thời gian thao tác | Đầu vào → Đầu ra |
|---|---|---|---|---:|---|
| B1 | Gửi câu hỏi / triệu chứng sau xuất viện | Bệnh nhân | App MyVinmec, hotline | 2 phút | Câu hỏi → Yêu cầu hỗ trợ |
| B2 🔴 | Đọc tin nhắn, hỏi lại thông tin, tra hồ sơ khám | CSKH | Hệ thống CSKH, hồ sơ bệnh nhân | 5 phút | Yêu cầu → Hồ sơ ca |
| 🔄 H1 | *Chuyển CSKH → điều dưỡng khoa* | | Điện thoại / nhóm chat | | Mất ngữ cảnh, điều dưỡng phải đọc lại |
| B3 | Đánh giá sơ bộ, phân loại mức độ | Điều dưỡng | Hồ sơ ca | 4 phút | Hồ sơ → Đề xuất xử lý |
| 🔄 H2 | *Chuyển điều dưỡng → bác sĩ* | | Điện thoại / nhóm chat | | Chờ bác sĩ rảnh |
| B4 🔴 | Xem triệu chứng, đọc lại lịch sử trao đổi, đưa hướng xử lý | Bác sĩ | Hồ sơ, lịch sử tin nhắn | 10 phút | Đề xuất → Kết luận/chỉ định |
| B5 | Phản hồi bệnh nhân (hướng dẫn chăm sóc, lịch tái khám nếu cần) | CSKH | Điện thoại / tin nhắn | 3 phút | Kết luận → Phản hồi |

**Tổng cộng = 24 phút thao tác/lượt** cho một ca đi hết tới bác sĩ, trong đó **khâu tiếp nhận – phân loại – chuyển (B2 + B3) ≈ 9 phút**, khớp mức **~8–12 phút/lượt** của Bảng số liệu chung. Chưa tính **thời gian chờ trong hàng đợi**: ca P1 hiện mất **~2–4 giờ** mới tới tay bác sĩ (ước tính).

**Bottleneck 🔴:**
- **B2:** CSKH phải hỏi lại nhiều lần vì bệnh nhân mô tả triệu chứng chưa đủ, không chuẩn hóa; CSKH không có chuyên môn để biết ca nào gấp.
- **B4:** bác sĩ xử lý theo thứ tự đến trước nên ca cần tư vấn gấp có thể bị chậm; bác sĩ phải đọc lại toàn bộ lịch sử trao đổi trước khi quyết định.

---

## 📋 3.2. Problem Statement (6-field) & Metrics

*Phụ trách: Nguyễn Văn Quốc*

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | **Người vận hành hằng ngày:** nhân viên **CSKH** tiếp nhận tin nhắn MyVinmec/hotline và **điều dưỡng điều phối** của khoa (người sẽ duyệt hàng đợi đã sắp xếp). **Người nhận kết quả:** bác sĩ điều trị (P0/P1), điều dưỡng (P2), CSKH (P3). **Người chịu ảnh hưởng:** bệnh nhân sau xuất viện/sau thủ thuật. |
| **2. Current Workflow** | Bệnh nhân nhắn app/gọi hotline → CSKH đọc, hỏi lại, tra hồ sơ (5 phút) → chuyển điều dưỡng khoa qua điện thoại/nhóm chat → điều dưỡng đánh giá sơ bộ (4 phút) → chuyển bác sĩ khi cần (10 phút) → CSKH phản hồi bệnh nhân (3 phút). Tổng **24 phút thao tác/lượt** (sơ đồ 3.1). Hàng đợi xử lý **theo thứ tự đến trước (FIFO)**, không có bước xếp mức ưu tiên; công cụ là hệ thống CSKH, hồ sơ bệnh nhân, điện thoại và nhóm chat. |
| **3. Bottleneck** | Khâu **đọc – hiểu – phân loại – chuyển** (B2 + B3, ~**8–12 phút/lượt**). Đây là bước **xử lý ngôn ngữ tự nhiên**: bệnh nhân mô tả bằng tiếng Việt đời thường, thiếu thông tin (*"vết mổ hơi rỉ nước vàng"*, *"uống thuốc xong thấy lâng lâng"*). CSKH không có chuyên môn để nhận ra ca gấp, nên ca cần bác sĩ bị **chìm giữa câu hỏi hành chính** (đổi lịch, hóa đơn, giấy tờ). Hậu quả dồn sang B4: bác sĩ nhận ca muộn và phải đọc lại toàn bộ lịch sử. |
| **4. Business Impact** | **An toàn người bệnh:** ca P1 hiện mất **~2–4 giờ** mới tới bác sĩ (ước tính); một biến chứng sau mổ (nhiễm trùng vết mổ, chảy máu) phát hiện muộn có thể dẫn tới tái nhập viện hoặc cấp cứu. **Chi phí nhân sự:** mỗi lượt tốn ~8–12 phút nhân sự chỉ ở khâu tiếp nhận – chuyển. Ví dụ giả định một bệnh viện nhận **200 lượt/ngày** thì riêng khâu này tốn **~27–40 giờ công/ngày**; điều dưỡng và bác sĩ còn mất thời gian cho câu hỏi P3 lẽ ra CSKH tự trả lời được. **Trải nghiệm & uy tín:** bệnh nhân chờ lâu, lo lắng, gọi lại nhiều lần làm hàng đợi càng dài; ảnh hưởng hình ảnh dịch vụ cao cấp của Vinmec. |
| **5. Success Metric** | **Chất lượng phân loại (đo offline trước khi dùng thật):** **recall ca P0/P1 ≥ 98%** trên bộ câu hỏi đã ẩn danh, do bác sĩ/điều dưỡng gán nhãn P0–P3. **Tốc độ:** **ca P1 đến tay bác sĩ < 30 phút** (hiện ~2–4 giờ); tin nhắn được gắn mức ưu tiên + tóm tắt trong **< 10 giây**. **Hiệu quả:** **giảm 40% câu hỏi P3** phải qua điều dưỡng. **Chỉ số theo dõi an toàn:** tỉ lệ *under-triage* (xếp thấp hơn nhãn) càng gần 0 càng tốt, từng ca bị review. **Cách đo baseline:** trong 2 tuần trước khi triển khai, ghi thời điểm bệnh nhân gửi → thời điểm bác sĩ mở ca, và tỉ lệ câu hỏi theo loại, trên log của khoa thí điểm. |
| **6. Operational Boundary** | **✅ AI được phép:** đọc tin nhắn và hồ sơ được cấp (chỉ đọc); gắn mức P0–P3; tóm tắt câu hỏi kèm đơn thuốc và lời dặn; ghi lại thông tin còn thiếu; **nâng** mức ưu tiên khi thấy dấu hiệu nặng. **⛔ AI tuyệt đối không được:** (1) đưa lời khuyên y khoa, chẩn đoán, liều thuốc hay lời trấn an kiểu *"không sao đâu"*; (2) tự gửi tin nhắn hoặc tự gọi cho bệnh nhân; (3) **hạ** mức P0 mà Rule cảnh báo đỏ đã gán; (4) tự đổi/hủy lịch tái khám hay đóng yêu cầu; (5) bịa thông tin không có trong tin nhắn/hồ sơ; (6) xếp ưu tiên theo hạng khách hàng thay vì mức độ lâm sàng. **🟢 Điểm cần người duyệt:** điều dưỡng điều phối duyệt hàng đợi đã sắp xếp và có quyền đổi mức; ca AI **phân vân hoặc độ tin cậy thấp → tự chọn mức cao hơn** và gắn cờ cho điều dưỡng; mọi phản hồi chuyên môn do bác sĩ/điều dưỡng quyết định; danh sách từ khóa cảnh báo đỏ do bác sĩ của khoa duyệt. |

**Tóm tắt metric:**

| Chỉ số | Hiện tại (ước tính) | Mục tiêu | Cách đo |
|---|---|---|---|
| Recall ca P0/P1 | Không đo được (không có bước phân loại) | **≥ 98%** | Bộ test ẩn danh do bác sĩ/điều dưỡng gán nhãn |
| Ca P1 đến tay bác sĩ | ~2–4 giờ | **< 30 phút** | Thời điểm gửi → thời điểm bác sĩ mở ca |
| Thời gian tiếp nhận – phân loại – chuyển | ~8–12 phút/lượt | AI gắn nhãn + tóm tắt **< 10 giây**, người chỉ duyệt | Log hệ thống |
| Câu hỏi P3 phải qua điều dưỡng | Baseline đo 2 tuần | **−40%** | So với baseline |

---

## 🧠 3.3. Future-State Flow & AI Fit

### 3.3a. AI-Fit Matrix: Rule vs LLM vs Agent

*Phụ trách: Lê Thanh Nguyên*

#### 1. Kết luận AI Fit

- [x] **Rule / State-Machine** — cho mọi bước có quy tắc rõ, cần chắc chắn và kiểm toán được (bắt cảnh báo đỏ → P0, định tuyến theo mức, kiểm tra output của LLM).
- [x] **LLM Feature** — chỉ cho 2 bước cần hiểu ngôn ngữ tự nhiên: phân loại P1–P3 từ câu mô tả mơ hồ và tóm tắt câu hỏi kèm hồ sơ.
- [ ] **Agentic Loop** — **không dùng** (lý do ở mục 4).

➡️ **Kiến trúc chọn: Hybrid = Rule (lớp an toàn trước và sau) + LLM Feature (một bước ngôn ngữ ở giữa) + Human-in-the-loop (điều dưỡng điều phối duyệt hàng đợi).**

Nguyên tắc chia việc: *cái gì phải đúng 100% và giải thích được thì giao cho Rule; cái gì cần hiểu câu chữ của bệnh nhân thì giao cho LLM; cái gì là quyết định chuyên môn thì giữ cho con người.*

---

#### 2. Ma trận theo từng bước

Các bước bám theo luồng hiện trạng ở mục 3.1 (bệnh nhân nhắn/gọi → CSKH đọc, hỏi lại → điều dưỡng → bác sĩ → phản hồi), tách nhỏ phần "đọc và phân loại" vốn là bottleneck.

| # | Bước trong luồng sắp xếp ưu tiên | Rule | LLM | Agent | **Chọn** | Lý do chọn | Chi phí / độ trễ | Rủi ro chính & cách giảm |
|---|---|:-:|:-:|:-:|:-:|---|---|---|
| 1 | Nhận tin, xác định bệnh nhân, gắn hồ sơ (lần khám gần nhất, đơn thuốc, lời dặn) | ✅ | ⚠️ | ❌ | **Rule / tích hợp** | Dữ liệu có cấu trúc (mã bệnh nhân, SĐT, mã lần khám) → tra cứu là đủ, không cần AI. | ~0, < 1 giây | Ghép nhầm hồ sơ → bắt buộc khớp ≥ 2 định danh; không khớp thì CSKH xác minh. |
| 2 | **Bắt dấu hiệu cảnh báo đỏ → gán P0** | ✅ | ⚠️ | ❌ | **Rule** | P0 không được phép bỏ sót. Rule **chắc chắn, giải thích được, kiểm thử được** từng từ khóa; LLM là xác suất nên không được làm lớp duy nhất cho P0. | ~0, < 1 giây | Bệnh nhân diễn đạt khác từ khóa → bộ từ khóa có biến thể/không dấu/viết tắt do bác sĩ duyệt và cập nhật từ log; phần lọt qua vẫn còn lớp LLM ở bước 3 (chỉ được **nâng** mức). |
| 3 | **Phân loại P1 / P2 / P3** từ câu mô tả mơ hồ (*"vết mổ hơi rỉ nước vàng"*, *"uống thuốc xong thấy lâng lâng"*, *"cho hỏi đổi lịch tái khám"*) | ❌ | ✅ | ⚠️ | **LLM Feature** | Câu chữ tự do, đa dạng, không liệt kê hết được → Rule vỡ khi bệnh nhân diễn đạt khác. LLM đọc được ngữ cảnh + hồ sơ. Chỉ **1 lần gọi**, đầu ra là JSON cố định → không cần Agent. | Thấp, vài giây/lượt (không đáng kể so với mục tiêu P1 < 30 phút) | *Under-triage* (xếp ca nặng xuống thấp) → ngưỡng bất đối xứng: phân vân giữa 2 mức **chọn mức cao hơn**; LLM **không được hạ** mức Rule đã gán; điều dưỡng điều phối duyệt hàng đợi. |
| 4 | Tóm tắt câu hỏi kèm hồ sơ cho người nhận | ❌ | ✅ | ❌ | **LLM Feature** | Tiết kiệm thời gian đọc lại lịch sử trao đổi (bottleneck ở bước bác sĩ của 3.1). Tóm tắt là việc LLM làm tốt. | Thấp, gộp chung lần gọi với bước 3 | Tóm tắt sai/bịa → bản tóm tắt luôn kèm **nguyên văn tin nhắn** và đường dẫn hồ sơ gốc; chỉ được dùng dữ liệu có trong input. |
| 5 | Định tuyến: P0 → kênh cấp cứu/trực khoa · P1 → bác sĩ · P2 → điều dưỡng · P3 → CSKH | ✅ | ❌ | ⚠️ | **Rule** | Ánh xạ cố định từ mức ưu tiên sang người nhận/SLA. Không có gì cần "suy luận". | ~0 | Sai bảng ánh xạ → cấu hình do quản lý khoa duyệt, có log mỗi lần định tuyến. |
| 6 | Kiểm tra output của LLM (đúng schema, mức hợp lệ, không chứa lời khuyên y khoa) | ✅ | ❌ | ❌ | **Rule (validator)** | Không thể dùng LLM để tự kiểm LLM cho ranh giới an toàn. Validator là code, chạy giống nhau mọi lần. | ~0 | Lọt câu tư vấn → danh sách mẫu cấm (liều thuốc, "không sao đâu", "ngưng thuốc"…), vi phạm thì loại output và đi Fallback. |
| 7 | Trả lời chuyên môn cho bệnh nhân / quyết định xử trí | ❌ | ❌ | ❌ | **🟢 Con người** | Đây là quyết định y khoa → bác sĩ/điều dưỡng chịu trách nhiệm. AI chỉ chuẩn bị thông tin. | — | — |

**Chú thích:** ✅ phù hợp · ⚠️ làm được nhưng kém hơn/rủi ro hơn · ❌ không phù hợp.

---

#### 3. So sánh tổng thể 3 phương án (cho toàn module)

| Tiêu chí | Chỉ Rule | Rule + LLM Feature (**chọn**) | Agentic Loop |
|---|---|---|---|
| Hiểu mô tả mơ hồ, tiếng Việt đời thường | ❌ Từ khóa vỡ khi bệnh nhân nói khác | ✅ | ✅ |
| Không bỏ sót P0 | ✅ với các ca nói đúng từ khóa | ✅ Rule bắt P0 trước, LLM chỉ được nâng mức | ⚠️ Phụ thuộc chuỗi suy luận nhiều bước |
| Đạt mục tiêu recall P0/P1 ≥ 98% | ❌ Khó, vì nhiều ca P1 không có từ khóa rõ | ✅ Khả thi, **đo được** trên bộ test gán nhãn | ⚠️ Khó chứng minh: mỗi lần chạy có thể đi đường khác |
| Giải thích & kiểm toán (audit) | ✅ Rất rõ | ✅ Rule rõ; LLM có lý do + log input/output | ❌ Khó truy vết vì nhiều bước, nhiều tool |
| Rủi ro AI tự hành động sai | Thấp | Thấp — AI chỉ gán nhãn + tóm tắt, không gửi gì cho bệnh nhân | **Cao** — agent có thể tự nhắn bệnh nhân, đổi lịch, gọi tool |
| Chi phí & độ trễ | ~0 | Thấp, 1 lần gọi LLM/lượt, vài giây | Gấp nhiều lần (nhiều lần gọi/lượt), khó dự đoán |
| Bảo trì | Phải cập nhật bộ từ khóa liên tục | Từ khóa (P0) + prompt + taxonomy | Prompt, tool, quyền hạn, vòng lặp… phức tạp nhất |
| Kết luận | Không đủ (bỏ lọt P1 mơ hồ) | **Phù hợp nhất** | Không phù hợp lúc này |

---

#### 4. Vì sao **không** dùng Agent

1. **Quy trình cố định, không cần tự lập kế hoạch.** Luồng luôn là: nhận tin → gắn hồ sơ → kiểm cảnh báo đỏ → phân loại → tóm tắt → định tuyến. Agent chỉ có giá trị khi phải tự quyết định bước tiếp theo; ở đây thứ tự đã biết trước.
2. **Hành động trong y tế khó đảo ngược.** Một agent có quyền gọi tool có thể tự trả lời bệnh nhân, đổi/hủy lịch tái khám hay đóng yêu cầu. Chỉ một lần trả lời sai (ví dụ *"triệu chứng này bình thường"*) cũng có thể làm chậm cấp cứu. Ranh giới của dự án là **AI không đưa lời khuyên y khoa và không tự liên hệ bệnh nhân** — agent đi ngược ranh giới này.
3. **Sai số cộng dồn qua nhiều bước.** Mỗi bước tự suy luận đều có xác suất sai; chuỗi càng dài, xác suất bỏ lọt ca nặng càng cao. Mục tiêu recall P0/P1 ≥ 98% cần một hệ thống **đơn giản, đo được**.
4. **Khó kiểm thử và chịu trách nhiệm.** Agent có thể đi đường khác nhau với cùng input → khó lập bộ test cố định, khó giải thích khi có sự cố. Trong y tế phải trả lời được câu hỏi "vì sao ca này bị xếp P2?".
5. **Chi phí và độ trễ không cần thiết.** Nhiều lần gọi model cho mỗi tin nhắn, trong khi một lần gọi LLM + Rule đã đủ.

*Khi nào xem xét lại:* chỉ sau khi module chạy ổn định và có dữ liệu đánh giá, có thể thử agent cho việc **hành chính P3** (ví dụ tra lịch trống để **soạn nháp** gợi ý đổi lịch cho CSKH duyệt) — vẫn không có quyền tự gửi hay tự thay đổi lịch.

---

#### 5. Ranh giới của bước LLM (đầu vào cho 3.3b Future-State Flow)

| Quy tắc | Nội dung |
|---|---|
| **Rule đi trước LLM** | Tin nhắn đã bị Rule gán P0 thì **không chờ LLM**: chuyển kênh khẩn ngay. LLM chỉ chạy để tóm tắt kèm theo (nếu kịp). |
| **Chỉ được nâng, không được hạ** | LLM không được hạ mức Rule đã gán. Nếu LLM thấy dấu hiệu nặng mà Rule bỏ sót → được nâng lên P0/P1. |
| **Phân vân → chọn mức cao hơn** | Nếu model trả về `uncertain_between` (ví dụ P1/P2) hoặc `confidence` dưới ngưỡng (đề xuất ban đầu 0.7, chốt lại sau khi đo trên bộ test) → lấy **mức cao hơn** và gắn cờ "AI không chắc" cho điều dưỡng điều phối. |
| **Không tư vấn y khoa** | Output không có trường nào gửi cho bệnh nhân; không được có liều thuốc, chẩn đoán, lời trấn an kiểu "không sao đâu". Validator (bước 6) chặn. |
| **Không bịa dữ liệu** | Tóm tắt chỉ dùng tin nhắn + hồ sơ được cấp; thiếu thông tin thì ghi "[chưa có trong hồ sơ]". |
| **↩️ Fallback khi LLM lỗi** | Timeout / lỗi API / JSON sai schema / vi phạm validator → **không tự gán mức**: đưa vào hàng "Chưa phân loại", xử lý **ngang P1** cho tới khi điều dưỡng điều phối xem (để giữ mục tiêu P1 < 30 phút). |
| **🟢 HITL** | Điều dưỡng điều phối xem hàng đợi đã sắp xếp, có thể đổi mức; mọi lần sửa mức được lưu làm dữ liệu đánh giá. |

**Đầu ra mẫu của bước LLM (structured output):**

```json
{
  "priority": "P1",
  "rule_red_flags": [],
  "uncertain_between": ["P1", "P2"],
  "confidence": 0.64,
  "summary": "Bệnh nhân mổ ruột thừa 5 ngày trước, báo vết mổ rỉ dịch vàng từ sáng, chưa nêu sốt.",
  "missing_info": ["nhiệt độ hiện tại"],
  "route_to": "bac_si_dieu_tri",
  "requires_nurse_review": true
}
```

*Trong ví dụ:* model phân vân P1/P2 và confidence thấp → hệ thống chọn **P1** (mức cao hơn), gắn cờ "AI không chắc". Trường `route_to` chỉ là gợi ý; định tuyến chính thức do Rule ở bước 5 quyết định theo mức ưu tiên.

---

#### 6. Cách kiểm chứng lựa chọn này (gắn với metric chung)

| Chỉ số | Mục tiêu | Đo thế nào |
|---|---|---|
| Recall ca P0/P1 (Rule + LLM) | **≥ 98%** | Bộ câu hỏi đã ẩn danh, được bác sĩ/điều dưỡng gán nhãn P0–P3; chạy offline trước khi dùng thật. |
| Recall riêng lớp Rule trên ca P0 | Báo cáo riêng | Để biết phần nào do Rule bắt, phần nào nhờ LLM nâng mức → biết cần bổ sung từ khóa nào. |
| Tỉ lệ *under-triage* (xếp thấp hơn nhãn) | Càng gần 0 càng tốt, theo dõi từng ca | Mỗi ca bị xếp thấp được review nguyên nhân. |
| Tỉ lệ *over-triage* (xếp cao hơn nhãn) | Chấp nhận được nhưng phải theo dõi | Nếu quá cao, bác sĩ bị quá tải và mục tiêu P1 < 30 phút bị ảnh hưởng → điều chỉnh ngưỡng. |
| Ca P1 đến tay bác sĩ | **< 30 phút** (hiện ~2–4 giờ) | Thời điểm bệnh nhân gửi → thời điểm bác sĩ mở ca. |
| Câu hỏi P3 phải qua điều dưỡng | **−40%** | So với baseline đo trước khi triển khai. |
| Chi phí LLM | Thấp so với thời gian CSKH ~5–7 phút/lượt | Đếm token thực tế/lượt × bảng giá model tại thời điểm triển khai (chưa tính trong file này). |

---

#### 7. Giả định & điểm chưa xác minh

- Danh sách dấu hiệu cảnh báo đỏ cho P0 **chưa có**; cần bác sĩ của khoa thí điểm soạn và duyệt.
- **Chưa có bộ dữ liệu gán nhãn** để chứng minh recall ≥ 98% → liên quan trực tiếp tới quyết định ở Phase 5.
- Ngưỡng `confidence` (0.7) là đề xuất ban đầu, phải chốt lại sau khi chạy offline.
- Chưa tính chi phí theo bảng giá thật; chỉ khẳng định ở mức "một lần gọi LLM/lượt, rẻ hơn nhiều so với thời gian người".
- Chưa xác nhận MyVinmec/hotline có cho phép lấy tin nhắn và hồ sơ qua API chỉ-đọc hay không.

---

### 3.3b. Future-State Flow (🔵 AI Step · 🟢 Human Step · ↩️ Fallback)

*Phụ trách: Nguyễn Mạnh Cường* — **[Chờ nội dung từ branch `nguyen-manh-cuong`]**

---

## 🏁 Phase 5 — EVALUATE

*Phụ trách: Nguyễn Minh Quân* — **[Chờ nội dung từ branch `nguyen-minh-quan`]**

### AI Readiness Checklist

### Quyết định

### Justification

---

## 📚 Nguồn tham khảo

[1] Vinmec — Tầm nhìn và sứ mệnh (quy mô hệ thống): https://www.vinmec.com/vie/tam-nhin-va-su-menh
