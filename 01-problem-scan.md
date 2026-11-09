# Phase 1 — SCAN & Phase 2 — QUICK-ASSESS

## Phase 1: Scan 5 bài toán thực tế theo 4 lenses

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | Xanh SM | Lặp lại | Tự động gợi ý lại điểm đón/điểm đến và phân bổ cuốc xe mới khi khách hàng thay đổi địa điểm giữa chừng. |
| 2 | Vinhomes | Tốn thời gian | Phân loại, gán ban quản lý và draft phản hồi ban đầu cho các phản ánh cư dân trên App Vinhomes Resident (mất 10-20 phút/lượt nếu làm thủ công). |
| 3 | Vinmec | AI có thể tốt hơn | Chatbot đầu vào triệu chứng của bệnh nhân để gợi ý chuyên khoa phù hợp và xếp lịch khám nhanh hơn quy trình gọi điện thủ công. |
| 4 | VinFast | Pain từ người khác | Khách hàng mô tả tiếng Việt về tiếng kêu, rung, hoặc lỗi xe điện, hệ thống cần tóm tắt nguyên nhân và gợi ý mã lỗi hoặc bước xử lý sơ bộ. |
| 5 | Vinpearl | Tốn thời gian | Quét và tổng hợp phản hồi khách hàng trên Booking/Google/Agoda để phát hiện các phàn nàn khẩn cấp (điều kiện phòng, thái độ nhân viên, an ninh) và routing cho manager. |

### Ghi chú nhanh về từng bài toán
- Xanh SM: là bài toán có dữ liệu rõ, tác động trực tiếp đến hiệu suất vận hành thời gian thực và dễ đo bằng thời gian xử lý/cuốc.
- Vinhomes: có tính lặp lại cao và nghẽn ở khâu routing/triage, phù hợp với LLM hoặc rule-based router.
- Vinmec: mang tính nhạy cảm về y tế, cần giới hạn ranh giới AI và Human-in-the-loop.
- VinFast: có khả năng nâng cấp bằng AI vì mô tả bằng ngôn ngữ tự nhiên của khách hàng rất phong phú nhưng cần kiểm soát kỹ thuật an toàn.
- Vinpearl: rất phù hợp để rà soát sentiment và phát hiện sự cố service trong thời gian thực.

---

## Phase 2: Quick Problem Cards (Top 3)

### QUICK PROBLEM CARD #1

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tự động gợi ý lại tuyến/điểm đón khi khách hàng  │
│ đổi địa điểm hoặc hủy đổi cuốc taxi điện giữa chừng.        │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế, điều phối viên, khách hàng     │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách báo điểm đón/điểm đến thay đổi                  │
│   → 2. Điều phối viên xem lại lộ trình hiện tại             │
│   → 3. So sánh với vị trí tài xế và khoảnh khắc thực tế     │
│   → 4. Gửi lại đề xuất điểm đón/điểm đến cho khách/tài xế │
│   → 5. Đặt lại cuốc hoặc gọi lại tài xế nếu cần             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4 (⏱ 8-12 phút/cuốc)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4: gợi ý lộ trình,  │
│ so khớp tài xế gần nhất, draft tin nhắn cho khách/tài xế    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian xử lý    │
│ từ 10 phút xuống dưới 2 phút/cuốc; giảm tỷ lệ cuốc bị hủy  │
│ do thay đổi địa điểm.                                        │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### QUICK PROBLEM CARD #2

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Phân loại và điều hướng phản ánh cư dân tại Vinhomes │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH, ban quản lý tòa nhà, cư dân │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân gửi phản ánh qua App Vinhomes Resident          │
│   → 2. CSKH đọc và đối chiếu loại phản ánh                 │
│   → 3. Tách riêng theo khu vực/ban quản lý                  │
│   → 4. Soạn phản hồi đầu tiên và assign cho bộ phận phù hợp │
│   → 5. Theo dõi xử lý và cập nhật cho cư dân               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4 (⏱ 15-20 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4: phân loại loại  │
│ sự cố, gợi ý ban quản lý, draft phản hồi sơ bộ              │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian xử lý từ │
│ 15 phút xuống dưới 3 phút/lượt; giảm sai lệch routing giữa │
│ các ban quản lý xuống dưới 5%.                              │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### QUICK PROBLEM CARD #3

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Triage và xếp lịch khám ban đầu cho bệnh nhân theo  │
│ triệu chứng nhập vào qua chatbot của Vinmec.                 │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bệnh nhân, nhân viên tổng đài, bác sĩ/điều│
│ phối khám                                                     │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Bệnh nhân mô tả triệu chứng qua chatbot hoặc cuộc gọi    │
│   → 2. Nhân viên tổng đài hỏi thêm để xác định chuyên khoa   │
│   → 3. Kiểm tra lịch khám và gợi ý thời gian phù hợp          │
│   → 4. Chuyển thông tin sang khoa/phòng khám                 │
│   → 5. Bệnh nhân xác nhận lịch và chuẩn bị hồ sơ             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1-3 (⏱ 10-15 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-3: trích xuất tri    │
│ệu chứng, gợi ý khoa, draft câu hỏi phụ, và đề xuất thời gian │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian xếp lịch │
│ từ 12 phút xuống dưới 3 phút/lượt; tăng tỷ lệ tạo lịch đúng   │
│ chuyên khoa lên trên 90%.                                     │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

## Lựa chọn bài toán ưu tiên

Top 3 bài toán trên đều có giá trị, nhưng bài toán ưu tiên để đi sâu nhất là:

1. Xanh SM: Tự động gợi ý lại tuyến/cuốc xe khi khách thay đổi điểm đón/điểm đến.
2. Vinhomes: Phân loại phản ánh cư dân và routing tự động.
3. Vinmec: Triage lịch khám theo triệu chứng.

Trong số đó, bài toán có độ khả thi cao và giá trị vận hành rõ nhất là "Tự động gợi ý lại tuyến/cuốc xe cho Xanh SM" vì:
- dữ liệu đầu vào tương đối có cấu trúc (GPS, điểm đón, lịch trình tài xế),
- tác động rất trực tiếp đến trải nghiệm khách hàng và tỷ lệ hủy chuyến,
- có thể đo lường bằng thời gian xử lý và tỷ lệ thành công,
- phù hợp với mô hình LLM + Rule hybrid, không cần khởi động quá phức tạp.

This is the final shortlisted problem for the next deep-dive stage.
