# RICE Matrix + 2x2 Value-Effort — ShopReplyAI

- **Sinh viên:** Nguyễn Công Hùng — 2A202600140
- **Ngày:** Day 20 — Workshop 1
- **Sản phẩm:** ShopReplyAI — AI chatbot cho shop online nhỏ tại Việt Nam (Facebook / Zalo / Shopee / TikTok Shop)
- **Bối cảnh:** Vừa kết thúc pilot 30 shop. Cần chọn 5 tính năng cốt lõi cho 1 quý tới (pre-Seed → Seed).

---

## 1. Bảng RICE — 5 tính năng cốt lõi từ PRD Day 17

> **Quy ước chấm điểm:**
> - Reach = số shop sẽ thực sự đụng tính năng trong 1 quý (đã discount 50% so với optimistic estimate). Base: 150 shop active dự kiến cuối quý (ramp 30 → 150).
> - Impact: 0.25 (tiny) / 0.5 (low) / 1 (medium) / 2 (high) / 3 (massive).
> - Confidence: chưa test = **50% MAX**. Có pilot data thật = 80%. Có A/B test xác thực = 100%.
> - Effort: person-month, đã nhân 1.5x (cộng QA, integration, post-launch monitoring).
> - **RICE Score = (R × I × C) / E**

| # | Tính năng | R (shop) | I | C | E (pm) | Score | Ghi chú chấm điểm |
|---|---|---:|---:|---:|---:|---:|---|
| F1 | **Auto-reply engine — Facebook Messenger + Zalo OA** | 150 | 3.0 | 0.80 | 4.0 | **90** | FB đã có pilot data 70% auto-rate (30 shop). Zalo OA chưa test → giữ C = 80% là honest, không đẩy lên 100%. Effort = 2.5 base × 1.5 multiplier. |
| F2 | **After-hours / 24-7 fallback mode** | 150 | 1.0 | 0.80 | 0.5 | **240** | Khi core engine xong, đây chỉ là toggle + queue layer. Pilot đã chạy → C = 80%. Effort gần 0 → score cao nhất. **Quick Win điển hình.** |
| F3 | **Sensitive-message human approval queue** (đổi trả / khiếu nại / giảm giá sâu) | 90 | 2.0 | 0.50 | 2.0 | **45** | ~60% shop pilot có nhu cầu (thấy trong feedback Day 19). **Chưa test** → C = 50% bắt buộc. Effort = build queue UI + push noti + audit log. |
| F4 | **Catalog auto-sync từ Shopee Open API** | 75 | 2.0 | 0.50 | 3.0 | **25** | Shopee Partners API access **chưa được approve** → C ≤ 50%. Reach discounted: chỉ ~50% shop đa kênh + Mixpanel benchmark 30-40% adoption first quarter. |
| F5 | **Voice-clone / custom tone training per shop** | 60 | 1.0 | 0.50 | 6.0 | **5** | Effort 6 person-month (fine-tune pipeline hoặc RAG riêng từng shop). Chưa có shop nào trả thêm tiền cho tính năng này → C = 50%. **Non-starter rõ ràng.** |

### Đọc kết quả

- **Top 2 ưu tiên:** F2 After-hours (240) và F1 Core auto-reply (90). F2 cao bất thường vì nó "ăn theo" F1 — chi phí cận biên gần 0 nhưng giá trị rõ.
- **F1 là Strategic Bet** — score chỉ 90 nhưng đây là core engine, không có nó thì không có sản phẩm.
- **F3 đáng làm sớm** dù score thấp hơn — bảo vệ shop khỏi "AI gây mất khách", giảm rủi ro pháp lý + uy tín.
- **F5 vứt** — Voice/tone training điểm 5, effort 6 tháng, chưa thấy willingness-to-pay. Founder dễ rơi vào bẫy "tính năng cool".

---

## 2. 2x2 Value-Effort Matrix

> Trục Value = Impact × Confidence (tác động thực, đã discount theo độ tin cậy).
> Trục Effort = Person-month đã multiply 1.5x.

```
                                  EFFORT
                       Low (≤ 2 pm)        High (> 2 pm)
                  ┌─────────────────────┬─────────────────────┐
                  │                     │                     │
                  │   QUICK WINS        │   STRATEGIC BETS    │
                  │   (làm NGAY)        │   (đầu tư dài hạn)  │
        High      │                     │                     │
       Value      │   ▲ F2 After-hours  │   ▲ F1 Core engine  │
   (I × C ≥ 1.0)  │     (240 — 0.5pm)   │     (FB+Zalo, 4pm)  │
                  │                     │                     │
                  │   ▲ F3 Approval Q   │                     │
                  │     (45 — 2pm)      │                     │
                  │                     │                     │
                  ├─────────────────────┼─────────────────────┤
                  │                     │                     │
                  │   FILL-INS          │   NON-STARTERS      │
                  │   (làm khi rảnh)    │   (VỨT vào sọt)     │
        Low       │                     │                     │
       Value      │   (không có)        │   ▲ F4 Catalog sync │
   (I × C < 1.0)  │                     │     (25 — 3pm)      │
                  │                     │                     │
                  │                     │   ▲ F5 Voice clone  │
                  │                     │     (5 — 6pm)       │
                  │                     │                     │
                  └─────────────────────┴─────────────────────┘
```

### Phân loại 4 quadrant

| Quadrant | Tính năng | Lý do |
|---|---|---|
| **Quick Win #1** | F2 — After-hours fallback | Effort 0.5pm, leverage core engine. Build momentum, lấy đà. |
| **Quick Win #2** | F3 — Sensitive approval queue | Effort 2pm. Bảo vệ shop khỏi rủi ro AI sai → tăng trust → giảm churn. |
| **Strategic Bet** | F1 — Core auto-reply engine (FB + Zalo) | 4pm effort, là moat của sản phẩm. Pitch Series A dựa trên cái này. |
| **Non-starter #1** | F4 — Catalog auto-sync Shopee | High effort (3pm) + low confidence (Shopee Partners chưa approve) + Reach hẹp. **Có thể chuyển NEXT** nếu Shopee mở API; còn nếu không → ngưng. |
| **Non-starter #2** | F5 — Voice clone / tone training | 6pm effort, score 5, không có willingness-to-pay. **Vứt thẳng.** |

> **Ghi chú riêng F4:** Đứng ở biên giới Strategic Bet ↔ Non-starter. Trong roadmap NEXT mình giữ nó như "có điều kiện" — nếu Shopee Open API approve trong 2 tháng, kéo lên; nếu không, drop hẳn.

---

## 3. Self-check theo rubric Day 20

| Tiêu chí Day 20 | Đáp ứng? | Bằng chứng |
|---|---|---|
| 5 tính năng cụ thể từ PRD | ✓ | F1-F5 đều mapping với in-scope/out-of-scope của PRD Day 17 |
| Confidence honest — chưa test = ≤ 50% | ✓ | F3, F4, F5 đều giữ C = 50% vì chưa pilot |
| Reach discounted realistic | ✓ | Áp Mixpanel 20-40% cho F4 (75/150); F5 chỉ ~40% shop quan tâm voice |
| Effort multiply 1.5x | ✓ | F1: 2.5 → 4.0 / F4: 2.0 → 3.0 / F5: 4.0 → 6.0 |
| 2x2 phân biệt rõ Quick Win / Strategic Bet / Non-starter | ✓ | Có ít nhất 1 mỗi loại + giải thích lý do |
| Có ≥ 1 Confidence ≤ 50% (rubric Outstanding) | ✓ | 3/5 features ở 50% — honest về uncertainty |

---

## 4. Bài học rút ra

1. **F2 (After-hours) là "ăn theo" F1** — đây là pattern điển hình: build core engine xong, các tính năng phụ thuộc gần như free. Cần xếp F1 vào NOW song song để mở khóa F2.
2. **F1 score chỉ 90 nhưng vẫn phải làm** — RICE đo *bang for buck*, không đo *strategic importance*. Founder cần đọc cả 2x2 + RICE table chứ không chỉ sort theo score.
3. **F5 là cám dỗ điển hình** — "Voice clone cho từng shop nghe rất cool". RICE giúp mình nhìn ra: cool ≠ đáng làm trước.
4. **F4 là dependency-hostage** — không phụ thuộc skill nội bộ mà phụ thuộc Shopee approve. Phải có Plan B (xem `dependency_map.md`).
