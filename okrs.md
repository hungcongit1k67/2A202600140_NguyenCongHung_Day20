# OKRs Q1 — ShopReplyAI

- **Sinh viên:** Nguyễn Công Hùng — 2A202600140
- **Ngày:** Day 20 — Workshop 3
- **Phạm vi:** OKR cho 1 quý sắp tới (post-Seed, pre-commercial launch)
- **Cơ sở:** Lấy từ cột NOW của roadmap (mở rộng auto-reply Zalo + sensitive approval queue)

---

## 🎯 Objective

> **Trở thành "nhân viên inbox AI" mà shop online nhỏ Việt Nam sẵn sàng trả tiền hàng tháng để giữ.**

*Định tính. Không số. Truyền cảm hứng — chủ shop nhận ra đây là người làm việc cho họ, không phải chatbot công cụ.*

---

## 📊 3 Key Results — đủ Leading + Lagging + Quality

### KR1 — Leading (dự báo, sửa kịp)

> **200 shop active dùng app ≥ 5 ngày/tuần, trong đó ≥ 60% có ít nhất 1 phiên duyệt câu nhạy cảm trong tuần.**

| | |
|---|---|
| **Đo gì?** | Active retention week-over-week + tỉ lệ adoption tính năng approval queue |
| **Tại sao Leading?** | Nếu shop active 5 ngày/tuần và dùng cả approval queue → chứng tỏ họ đang vận hành thực sự, không chỉ "thử cho biết". Đây là **predictor số 1 của paying conversion**. |
| **Baseline hôm nay** | 30 shop pilot, ~70% active 5+ ngày/tuần, chưa có approval queue |
| **Cách sửa nếu trật** | Nếu retention < 60% → onboarding kém / sản phẩm không match nhu cầu thực → quay lại customer interview. Sửa được trước khi quá muộn. |

---

### KR2 — Lagging (kết quả business, xác nhận)

> **MRR đạt 30 triệu VND, từ ≥ 120 paying shop trả phí 199K-299K VND/tháng (ARPU trung bình 249K).**

| | |
|---|---|
| **Đo gì?** | Doanh thu định kỳ hàng tháng + paying customer count |
| **Tại sao Lagging?** | Đây là kết quả thật — VC đọc dòng này biết business có sống không. Nếu KR1 đạt mà KR2 trượt → product-market fit chưa real, có thể chỉ là "free user thích miễn phí". |
| **Baseline hôm nay** | 0 paying shop. 12/30 pilot shop nói sẵn sàng trả phí (Day 19 pitch memo). |
| **Aspirational ratio** | 70% sweet spot ≈ 84 shop / 21M VND MRR. Hit 100% (120/30M) coi là vượt kỳ vọng → set quý sau cao hơn. |

---

### KR3 — Quality (bảo vệ growth bền vững)

> **NPS ≥ 45 và monthly churn ≤ 5% sau tháng đầu.**

| | |
|---|---|
| **Đo gì?** | NPS qua survey trong app + churn rate (% shop hủy gói trên tổng paying base) |
| **Tại sao Quality?** | Có thể đạt KR1 + KR2 bằng growth ngắn hạn (KOL push, giảm giá) nhưng nếu shop trả tiền 1 tháng rồi bỏ → churn giết business. KR3 là cái phanh chống growth ảo. |
| **Baseline hôm nay** | Chưa đo NPS chính thức. 21/30 shop muốn tiếp tục sau pilot 14 ngày (~70% retention thô). |
| **Tại sao 45 và 5%?** | NPS 45 là "good" cho B2B SaaS giai đoạn early. Churn 5%/tháng tương đương ~46% annual — chấp nhận được cho SME segment, nhưng cần giảm dần. |

---

## ✅ Self-check theo rubric Day 20

| Tiêu chí 3 — OKR Outcome Focus | Đáp ứng? | Bằng chứng |
|---|---|---|
| Objective định tính, không số, truyền cảm hứng | ✓ | "Nhân viên inbox AI mà shop sẵn sàng trả tiền giữ" — định tính, có cảm xúc |
| 3 KR đủ Leading + Lagging + Quality | ✓ | KR1 retention (Leading) / KR2 MRR (Lagging) / KR3 NPS+churn (Quality) |
| Mỗi KR có số cụ thể | ✓ | 200 shop, ≥60%, 30M VND, 120 paying, NPS 45, churn 5% |
| KR aspirational (~70% sweet spot) | ✓ | Từ 30 → 200 active, 0 → 120 paying là 4-5x baseline — không safe nhưng không 10x ảo |
| Ngôn ngữ business, không tech jargon | ✓ | Không nhắc "model accuracy" / "API latency" / "GPT-4o-mini" / "uptime" — chỉ dùng metric kinh doanh |

---

## ⚠️ Anti-pattern đã tránh

| ❌ BAD (output-focused) | ✅ Đã tránh bằng cách… |
|---|---|
| "AI accuracy đạt 90%" | KR3 đo *NPS của chủ shop* — họ care về kết quả kinh doanh, không phải accuracy |
| "Ship 12 tính năng trong quý" | KR1 đo *active retention* — feature ship ra mà không ai dùng = vô nghĩa |
| "Code coverage 90%" / "Latency < 100ms" | Không xuất hiện ở KR nào — đây là engineering metric, không phải outcome |
| "Tăng đáng kể MRR" | KR2 ghi rõ **30 triệu VND, 120 paying shop** — số cụ thể, không mơ hồ |
| 3 KR cùng loại revenue | KR1 (behavior) / KR2 (revenue) / KR3 (NPS+churn) — đa dạng |

---

## 💡 Ghi chú cho Founder (đọc sáng)

> *"Mục tiêu của quý không phải là build cho xong feature. Mục tiêu là biến 30 shop pilot thành 120 shop trả tiền — và giữ được họ ở lại tháng thứ 2."*

VC reading this OKR sẽ đoán đúng:
- Business model: B2B SaaS subscription cho SME e-commerce
- Stage: Pre-commercial → first commercial quarter
- Risk chính: retention (KR3 protect), not acquisition
- North Star: paying customer count + retention, not MAU
