# 04 — Phân công nhiệm vụ nhóm (Lab 02: AI Product Scoping)

> **Tên nhóm:** _Nhóm 50_
>
> | # | Họ và tên | Email | Branch | Vai trò |
> |---|---|---|---|---|
> | 1 | Nguyễn Văn Quốc | quoc.nvsoict@gmail.com | `nguyenvanquoc` | **Trưởng nhóm** + Chủ ý tưởng CareFollow — Problem Statement |
> | 2 | _[Họ tên]_ | _[email]_ | _[branch]_ | Workflow Mapping + Sơ đồ |
> | 3 | _[Họ tên]_ | _[email]_ | _[branch]_ | AI Fit (Rule vs LLM vs Agent) |
> | 4 | _[Họ tên]_ | _[email]_ | _[branch]_ | Future-State Flow (HITL + Fallback) |
> | 5 | _[Họ tên]_ | _[email]_ | _[branch]_ | Evaluate & Decision |
>


---

## 🎯 1. Bài toán chính của nhóm

**Vinmec CareFollow — Trợ lý theo dõi bệnh nhân sau điều trị**, trọng tâm deep-dive là **module Sắp xếp ưu tiên (Triage & Routing)**.

> Câu hỏi/triệu chứng của bệnh nhân sau điều trị (qua app MyVinmec / hotline) đang được xử lý theo thứ tự đến trước. Ca cần bác sĩ có thể bị chìm giữa hàng chục câu hỏi hành chính. AI phân loại mức ưu tiên P0–P3, tóm tắt kèm hồ sơ và chuyển đúng người; **AI không đưa lời khuyên y khoa**.

Hai module còn lại (Diễn giải lời dặn bác sĩ, Chăm sóc chủ động) chỉ nhắc ngắn trong Future-State Flow như **nguồn đầu vào** và **lộ trình mở rộng**.

Chi tiết ý tưởng: xem Quick Card #1 trong `01-problem-scan.md` (branch `nguyenvanquoc`).

---

## 👤 2. Phần CÁ NHÂN — mỗi thành viên tự làm trên branch riêng (40đ)

Không chia việc, không sao chép của nhau.

| File | Nội dung | Gate | Điểm |
|---|---|---:|---:|
| `01-problem-scan.md` | Bảng SCAN ≥ 5 bài toán (≥ 3 lenses) + 3 Quick Problem Cards của **riêng mình** | I1 | 15 |
| `starter-code/prompt_prototype.py` | SYSTEM_PROMPT + `evaluate_prompt()` + ≥ 3 adversarial tests, chạy ra đủ dòng ✅ Passed | I2 | 10 |
| `03-ai-log.md` | Phản ánh việc dùng AI: giúp gì, sai/hallucination ở đâu, đã sửa prompt/ranh giới thế nào | I3 | 15 |

**Tiến độ cá nhân** (mỗi người tự đánh dấu):

| Thành viên | 01-problem-scan | prompt_prototype.py | 03-ai-log |
|---|:-:|:-:|:-:|
| 1. Nguyễn Văn Quốc | ✅ | ✅ (cần chạy xác nhận) | ⬜ |
| 2. _[Họ tên]_ | ⬜ | ⬜ | ⬜ |
| 3. _[Họ tên]_ | ⬜ | ⬜ | ⬜ |
| 4. _[Họ tên]_ | ⬜ | ⬜ | ⬜ |
| 5. _[Họ tên]_ | ⬜ | ⬜ | ⬜ |

---

## 👥 3. Phần NHÓM — chia việc cho CareFollow (60đ)

Sản phẩm nhóm: **`02-deep-dive-report.md`** và **`04-workflow-diagram.png`**.

| # | Người phụ trách | Phần | Gate | Đầu ra cụ thể | Xong khi |
|---|---|---|---|---|---|
| 1 | **Nguyễn Văn Quốc** | **3.2 Problem Statement (6-field) & Metrics** + đoạn mở bài | G2 (20đ) | Bảng 6 trường: Actor, Current Workflow, Bottleneck, Business Impact, Success Metric (có số), Operational Boundary (được làm / tuyệt đối không / điểm cần duyệt) | Mọi metric có con số + cách đo; boundary có ít nhất 3 điều "tuyệt đối không" |
| 2 | **Thành viên 2** | **3.1 Current-State Workflow** + file `04-workflow-diagram.png` | G1 (20đ) | Sơ đồ: bệnh nhân nhắn/gọi → CSKH đọc, hỏi lại → chuyển điều dưỡng → bác sĩ → phản hồi. Đánh dấu 🔴 Bottleneck, 🔄 Handoff, thời gian từng bước, **Tổng = __ phút/lượt** | Có ≥ 1 🔴, ≥ 2 🔄, tổng thời gian khớp bảng số liệu chung (mục 4) |
| 3 | **Thành viên 3** | **3.3a AI-Fit Matrix: Rule vs LLM vs Agent** | G3 (5đ) | Bảng so sánh 3 phương án cho từng bước: từ khóa cảnh báo đỏ → Rule; câu mô tả mơ hồ → LLM; vì sao **không** dùng Agent (rủi ro tự hành động trong y tế) | Mỗi lựa chọn có lý do + chi phí/rủi ro |
| 4 | **Thành viên 4** | **3.3b Future-State Flow** | G3 (5đ) | Quy trình mới đánh dấu 🔵 AI Step, 🟢 Human Step (HITL), ↩️ Fallback: Rule P0 → LLM phân loại P1–P3 + tóm tắt → điều dưỡng điều phối duyệt hàng đợi; AI không chắc → nâng mức / chuyển người | Mọi bước AI đều có Fallback; có ngưỡng "phân vân → chọn mức cao hơn" |
| 5 | **Thành viên 5** | **Phase 5 Evaluate & Decision** | G4 (10đ) | Checklist 3 câu (dữ liệu, rủi ro, stakeholders) trả lời trung thực + quyết định GO / NOT YET / NO-GO kèm lý giải, lộ trình (gợi ý: NOT YET → GO hẹp ở 1 khoa ngoại sau mổ nhỏ) | Quyết định dựa trên bằng chứng; nói rõ dữ liệu còn thiếu |

**Trưởng nhóm (Nguyễn Văn Quốc)** thêm các việc:
- Ghép 5 phần thành một file `02-deep-dive-report.md` thống nhất giọng văn, kiểm tra số liệu khớp nhau.
- Thêm phần đầu thông tin nhóm (tên nhóm, họ tên + email 5 thành viên) vào **mọi** file `.md` trên `main`.
- Họp chọn bản `01-problem-scan.md` và `03-ai-log.md` tốt nhất để đưa vào `main`.
- Chạy `python autograder/autograder.py --section-a` trên `main` → 4/4 file PASS.
- Điền form nộp bài (chỉ trưởng nhóm): https://forms.gle/nzwFKAooNbb7iot36

---

## 📏 4. Bảng số liệu chung (chốt trước khi viết — mọi phần phải dùng đúng số này)

*Tất cả là **ước tính**, ghi rõ "ước tính" trong báo cáo và nêu cách đo baseline.*

| Chỉ số | Giá trị chốt | Dùng ở phần |
|---|---|---|
| Thời gian CSKH đọc + hỏi lại + tra hồ sơ | ~5–7 phút/lượt | 3.1, 3.2 |
| Thời gian chuyển điều dưỡng/bác sĩ | ~3–5 phút/lượt | 3.1, 3.2 |
| Tổng thời gian xử lý thủ công | ~8–12 phút/lượt | 3.1, 3.2 |
| Thời gian ca P1 đến tay bác sĩ (hiện tại) | ~2–4 giờ | 3.2, 5 |
| Mục tiêu: ca P1 đến tay bác sĩ | < 30 phút | 3.2, 3.3b, 5 |
| Mục tiêu: recall ca P0/P1 trên bộ test gán nhãn | ≥ 98% | 3.2, 3.3a, 5 |
| Mục tiêu: giảm câu hỏi P3 phải qua điều dưỡng | −40% | 3.2, 5 |
| Mức ưu tiên | P0 khẩn cấp (ngay) · P1 bác sĩ (< 2h) · P2 điều dưỡng (trong ngày) · P3 CSKH (< 24h) | tất cả |

Muốn đổi số → báo cả nhóm và cập nhật bảng này trước.

---

## 🗓️ 5. Thứ tự & mốc thời gian

| Bước | Việc | Ai | Mốc |
|---|---|---|---|
| 1 | Quốc trình bày CareFollow (5 phút), chốt phạm vi = module sắp xếp ưu tiên, chốt bảng số liệu mục 4 | Cả nhóm | _[giờ]_ |
| 2 | Vẽ sơ đồ Current-State (các phần khác dựa vào đây) | Thành viên 2 | _[giờ]_ |
| 3 | Viết song song 3.2 / 3.3a / 3.3b / Phase 5 trên branch riêng | 1, 3, 4, 5 | _[giờ]_ |
| 4 | Hoàn thành phần cá nhân (01, .py, 03) | Mỗi người | _[giờ]_ |
| 5 | Ghép `02-deep-dive-report.md`, review chéo, sửa số liệu | Trưởng nhóm + cả nhóm | _[giờ]_ |
| 6 | Đưa 4 file `.md`/`.png` vào `main`, chạy autograder, nộp form | Trưởng nhóm | _[giờ]_ |

---

## 🔀 6. Quy tắc Git của nhóm

- Mỗi người làm trên **branch riêng**, không push thẳng lên `main`.
- `git add` **đúng file mình làm** — không dùng `git add .` (tránh kéo theo `README.md`, `.gitignore`, `autograder.py` bị đổi ký tự xuống dòng, và tránh lộ `.env`).
- Commit theo mẫu: `Feat: <mô tả> by <ten>` — commit nhỏ, nhiều lần để thể hiện đóng góp.
- **Không đưa file `.py` vào `main`** (code chấm cá nhân trên branch).
- Trưởng nhóm lấy file `.md` vào `main`:
  ```bash
  git checkout main && git pull origin main
  git checkout <branch-thanh-vien> -- 01-problem-scan.md 03-ai-log.md
  git checkout <branch-truong-nhom> -- 02-deep-dive-report.md 04-workflow-diagram.png
  git commit -m "Chore: Select and merge best markdown reports into main"
  git push origin main
  ```
- Tên file nộp phải **đúng tuyệt đối** và nằm ở **thư mục gốc**: `01-problem-scan.md`, `02-deep-dive-report.md`, `03-ai-log.md`, `04-workflow-diagram.png` (hoặc `.jpg/.jpeg/.pdf`).
