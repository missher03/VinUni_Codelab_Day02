# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
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

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

### 📝 3 Quick Problem Cards của tôi:

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

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*


### 🧐 Tự phản biện (góc nhìn CFO / Trưởng phòng Vận hành)

| Thẻ | Điểm yếu | Vì sao rule-based có thể đủ? | Cách xử lý |
|---|---|---|---|
| **#1 CareFollow** | Rủi ro lớn nhất là *under-triage* (xếp ca khẩn xuống thấp). Chưa có dữ liệu câu hỏi thật đã gán nhãn để chứng minh recall 98%. | Bộ từ khóa cảnh báo đỏ có thể bắt được phần lớn ca P0. | Giữ Rule làm lớp đầu tiên (P0 không qua LLM); LLM chỉ xử lý phần mô tả mơ hồ; ngưỡng bất đối xứng (phân vân → chọn mức cao hơn); ưu tiên lâm sàng luôn đứng trước hạng khách hàng. |
| **#2 VinFast** | Metric "top-3 chứa lỗi thật" phụ thuộc chất lượng mô tả của khách; khách mô tả mơ hồ thì AI cũng mơ hồ. | Cây câu hỏi cố định (decision tree) cho các lỗi phổ biến có thể làm được một phần. | Dùng LLM để sinh câu hỏi bổ sung thay vì chỉ đoán; đo trên lịch sử phiếu sửa chữa trước khi cam kết số. |
| **#3 Vinhomes** | Bản vẽ có nhiều định dạng (ảnh chụp tay, PDF), LLM đọc bản vẽ chưa đáng tin. | Checklist giấy tờ và giờ thi công hoàn toàn làm được bằng rule. | Giai đoạn đầu chỉ cho AI đọc **mô tả chữ** và gắn cờ; bản vẽ vẫn do kỹ thuật viên xem. |

### 🗳️ Đề xuất bài toán cho Deep-Dive nhóm

Đề xuất chọn **Card #1 — Vinmec CareFollow**, tập trung vào **module sắp xếp ưu tiên** (là lõi: mọi câu hỏi từ module diễn giải lời dặn và chăm sóc chủ động đều đổ về đây).

- **Vì sao chọn:** nỗi đau thật, tác động trực tiếp đến an toàn người bệnh; cho phép so sánh đủ Rule / LLM / Agent trong cùng một sản phẩm; ranh giới và Fallback rõ ràng.
- **Vì sao không chọn #2 (VinFast):** cần dữ liệu phiếu sửa chữa nội bộ để chứng minh metric, khó đánh giá trong phạm vi lab.
- **Vì sao không chọn #3 (Vinhomes):** giá trị tốt nhưng tác động hẹp (chỉ nhóm cư dân đang thi công); phù hợp làm dự án nhỏ sau.

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Ai đang thực hiện tác vụ hằng ngày? |
| **2. Current Workflow** | Mô tả tóm tắt quy trình thủ công hiện tại và công cụ sử dụng. |
| **3. Bottleneck** | Bước nào chậm, lỗi, hoặc cần xử lý ngôn ngữ tự động nhiều nhất? |
| **4. Business Impact** | Tổn thất thực tế đo bằng thời gian, chi phí, hoặc SLA của Vingroup. |
| **5. Success Metric** | AI giải quyết được thì đạt ngưỡng số mấy? (Ví dụ: *"85% vé được phân loại dưới 10s"*). |
| **6. Operational Boundary** | AI được phép làm gì, TUYỆT ĐỐI không được làm gì, điểm nào cần duyệt? |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [ ] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** Tác vụ LLM xử lý.
  * 🟢 **Human Step (HITL):** Bước con người phê duyệt/review (Human-in-the-loop).
  * ↩️ **Fallback:** Kế hoạch dự phòng khi LLM trả về kết quả lỗi hoặc không tự tin.

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Để đảm bảo kỹ sư của Vin Smart Future luôn giữ vững năng lực lập trình, nhóm của bạn sẽ tiến hành **lập trình bản mẫu prompt** trực tiếp trên **Gemini 2.5 Flash** bằng Python để stress-test hệ thống.

### Hướng dẫn thực hiện:
1. Mở file [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) bằng VS Code/Cursor.
2. Hoàn thiện các nội dung sau:
   * **System Prompt:** Viết chỉ thị cực kỳ nghiêm ngặt quy định vai trò, nhiệm vụ, định dạng output và **Operational Boundary (Ranh giới cấm)** của mô hình.
   * **Structured Output:** Định nghĩa định dạng JSON output rõ ràng.
   * **Adversarial Test Cases:** Viết ít nhất 3 prompts "tấn công" (Adversarial inputs) cố tình dụ AI vượt ranh giới hoặc đưa ra câu trả lời không được phép để kiểm tra xem ranh giới của bạn có thực sự vững chắc.
3. Chạy file python:
   ```bash
   python3 prompt_prototype.py
   ```
4. Kiểm tra xem các ranh giới an toàn có bị LLM phá vỡ hay không và ghi lại kết quả vào worksheet.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [ ] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*
