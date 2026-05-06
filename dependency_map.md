# Dependency Map + Critical Path — ShopReplyAI

- **Sinh viên:** Nguyễn Công Hùng — 2A202600140
- **Ngày:** Day 20 — Workshop 4
- **Nguyên tắc cốt lõi:** *Mỗi Tier 1 dependency phải có Plan B sẵn sàng deploy trong 24-72 giờ. Plan B không phải document — phải là code đã viết, đã test.*

---

## 🔴 Tier 1 — Critical Dependencies (có thể giết sản phẩm trong 1 ngày)

### Dependency 1 — OpenAI API (GPT-4o-mini)

| | |
|---|---|
| **Vendor** | OpenAI Platform (mô hình GPT-4o-mini) |
| **Vai trò** | Bộ não trả lời tin nhắn — 100% câu trả lời AI hiện đi qua đây |
| **Worst-case scenario** | OpenAI siết rate limit (giống case AI Legal startup trong handbook), tăng giá 3-5x đột ngột, hoặc tạm chặn vùng VN do compliance reason. Sản phẩm down ngay lập tức cho 100% shop pilot. |

**🛡️ Plan B:**

1. **Multi-LLM abstraction layer** — đã thiết kế sẵn interface chung (`LLMProvider`). Code switch sang Anthropic Claude Haiku hoặc Google Gemini Flash trong **24 giờ**. Cả 2 đều có giá tương đương GPT-4o-mini, chất lượng tiếng Việt chấp nhận được cho intent đơn giản.
2. **Caching layer cho top 60% query** — câu hỏi lặp lại (giá, size, ship) được cache 24h → giảm ~60% LLM call → đệm cho lúc switch vendor.
3. **Vietnamese local LLM fallback** — research PhoGPT / Vistral tự host. Quality thấp hơn nhưng đảm bảo "always-on" cho intent core.

**💰 Cost của Plan B:**
- Abstraction layer: 2 tuần dev (~5M VND lương) + 2-3 ngày QA. *Phải build TRƯỚC khi commercial launch — không để đến lúc crisis.*
- Caching layer: 1 tuần dev (~2.5M VND).
- Local LLM research: 1 tháng nhưng có thể defer.
- **Tổng cost upfront ≈ 7-8M VND + ~1 tháng dev part-time.**

**⚠️ Cascading failure check:** Nếu OpenAI **VÀ** Anthropic cùng down (ví dụ: AWS US-East-1 outage, ảnh hưởng cả 2 vendor) → caching layer giữ ~60% chức năng. Trong 1-2 giờ, switch sang Gemini (Google Cloud) — region khác. Plan B chain còn work.

---

### Dependency 2 — Facebook / Meta Business Platform

| | |
|---|---|
| **Vendor** | Meta — Messenger Platform API + Page Access Token |
| **Vai trò** | Kênh giao tiếp số 1 — ~85% shop pilot dùng Facebook là kênh chính |
| **Worst-case scenario** | Meta đổi webhook policy đột ngột (đã từng xảy ra với chatbot 2018), hoặc Page Access Token revoked do shop bị flag spam, hoặc ShopReplyAI bị Meta App Review reject vì lý do compliance không lường trước. |

**🛡️ Plan B:**

1. **Zalo OA + Zalo Mini App** — đang là channel #2 trong roadmap NOW. Khi triển khai xong, không phụ thuộc 100% vào Meta. *Diversify channel risk.*
2. **Browser-extension fallback** — extension Chrome/Edge cho chủ shop, semi-automate bằng cách inject reply suggestion vào giao diện FB Messenger Web. Không phụ thuộc Page Access Token. Quality kém hơn nhưng "alive".
3. **Webhook + queue retry** — nếu Meta tạm chặn webhook, queue tin nhắn ở phía mình + retry với exponential backoff khi token được khôi phục.

**💰 Cost của Plan B:**
- Zalo OA integration: 3 tuần dev (~7-8M VND). *Đã nằm trong NOW roadmap → không phải cost thêm.*
- Browser extension: 2 tuần dev (~5M VND). *Defer được tới khi cần.*
- Queue + retry layer: 1 tuần (~2.5M VND), build cùng lúc với core engine.
- **Có thể deploy Plan B trong 5-7 ngày khi crisis** (Zalo đã sẵn, browser extension là backup-of-backup).

---

### Dependency 3 — Shopee Open API (Partners Program)

| | |
|---|---|
| **Vendor** | Shopee Open Platform — Order API + Product API |
| **Vai trò** | Nguồn dữ liệu catalog cho shop bán đa kênh (~50% shop pilot bán cả Shopee). Cần thiết cho NEXT-1 (catalog auto-sync) trong roadmap. |
| **Worst-case scenario** | Shopee không approve Partners application (đang pending), hoặc revoke access sau 6 tháng do thay đổi chính sách (đã xảy ra với một số tool 2023). Catalog auto-sync feature down → shop đa kênh phải nhập tay → mất tính cạnh tranh. |

**🛡️ Plan B:**

1. **Manual catalog upload qua CSV/Excel** — fallback đơn giản, chủ shop tự upload danh mục sản phẩm. Quality thấp hơn (không real-time) nhưng vẫn work.
2. **Headless browser scrape có consent** — dùng credential của chính chủ shop (chủ shop đăng nhập Shopee Seller Center trên server riêng) để pull catalog mỗi 6h. *Phải có consent rõ ràng + tuân thủ ToS.*
3. **Tập trung TikTok Shop + Lazada Open Platform thay thế** — không bỏ trứng vào 1 giỏ Shopee.

**💰 Cost của Plan B:**
- CSV upload UI: 3 ngày dev (~1M VND).
- Headless scraper + consent flow + legal review: 2 tuần (~5M VND) + tham vấn luật sư về Nghị định 13/2023 (~3-5M VND).
- TikTok Shop integration research: 1 tuần (~2.5M VND).
- **Tổng ≈ 11-13M VND, deploy được trong 7 ngày cho CSV path.**

---

## 🟡 Tier 2 — Important (theo dõi)

| Dependency | Vendor | Risk | Mitigation |
|---|---|---|---|
| Cloud hosting | AWS Singapore region | Region down ~1-2 lần/năm | Multi-AZ trong cùng region; backup snapshot daily; có thể spin up GCP region asia-southeast1 trong 4-6h. |
| Payment gateway | VNPay / MoMo Business | Đổi phí giao dịch → giảm margin | Negotiate hợp đồng 12 tháng giá cố định; có alternate option Stripe / Paddle cho shop quốc tế. |
| Vietnamese PDPL compliance | Nghị định 13/2023 (PDPL) | Audit / fine | Legal review từ tháng 1 trước commercial launch. Privacy policy rõ ràng. Data localization Singapore region. |

---

## 🛣️ Critical Path cho 1 quý NOW

> **Nguyên tắc:** Trễ 1 ngày trên Critical Path = trễ launch 1 ngày. Khi pressure thời gian → **focus toàn bộ engineering effort vào Critical Path**.

```
   ┌───────────────────────────────────────────────────────────────────┐
   │                    CRITICAL PATH (tuần tự, KHÔNG parallel)        │
   ├───────────────────────────────────────────────────────────────────┤
   │                                                                   │
   │   ① Data Pipeline ─────► ② Legal Compliance ─────► ③ Model       │
   │   (catalog ingest +      (Nghị định 13/2023        Setup +        │
   │   chat history,          PDPL VN, FB ToS,          Abstraction    │
   │   user consent)          Meta App Review)          layer (Plan B) │
   │   2 weeks                2-3 weeks                 2 weeks        │
   │                                                                   │
   │       ▼                                                           │
   │                                                                   │
   │   ④ API Integration ───► ⑤ Sensitive Approval ───► ⑥ Pilot       │
   │   (FB Messenger +        Queue + Audit Log         Launch        │
   │   Zalo OA + webhook      (NOW-2 từ roadmap)        (30 → 200     │
   │   queue)                                           shops, ramp)   │
   │   3 weeks                2 weeks                   4 weeks        │
   │                                                                   │
   │   ──────────────────────► CRITICAL PATH = ~15-16 weeks ──────────►│
   └───────────────────────────────────────────────────────────────────┘

   ┌───────────────────────────────────────────────────────────────────┐
   │              NON-CRITICAL (parallel, có buffer)                   │
   ├───────────────────────────────────────────────────────────────────┤
   │                                                                   │
   │   • Marketing site / landing page    → parallel với Model Setup   │
   │   • Insight dashboard UI             → parallel với API Integ     │
   │   • Onboarding video tutorial        → parallel với Pilot Launch  │
   │   • Documentation / Help center      → có thể làm sau cùng        │
   │   • Community / KOL outreach         → parallel với Pilot Launch  │
   │                                                                   │
   └───────────────────────────────────────────────────────────────────┘
```

### Tại sao Legal Compliance nằm SỚM trên Critical Path?

Đa số founder Việt Nam bỏ qua Legal đến tận lúc launch — đây là **lỗi đáng tiền nhất**:

- **Nghị định 13/2023 PDPL VN:** ShopReplyAI xử lý tin nhắn khách hàng = personal data. Cần consent flow + Privacy Policy + DPO contact ngay từ tháng 1, không phải tháng 6.
- **Meta App Review:** Để dùng Messenger Platform API ở scale > sandbox, phải qua App Review. Quy trình mất 2-6 tuần. Nếu reject → phải sửa code → review lại → block toàn bộ launch.
- **Shopee Partners application:** Pending từ tháng đầu — block NEXT-1 nếu không approve.

Nếu để Legal làm cuối, kịch bản xấu nhất là **đã build xong sản phẩm nhưng không launch được** trong 2-3 tháng vì chờ approve.

### Tại sao UI design KHÔNG nằm trên Critical Path?

UI design có buffer — có thể làm song song với Model Setup hoặc API Integration. Nếu UI trễ 1 tuần, tổng thời gian launch không tăng. *Đừng cho engineer làm UI polish khi Data Pipeline chưa xong* (bài học handbook §2.7).

---

## ✅ Self-check theo rubric Day 20

| Tiêu chí 4 — Risk Awareness | Đáp ứng? | Bằng chứng |
|---|---|---|
| 3 dependencies cụ thể với vendor name | ✓ | OpenAI / Meta-Facebook / Shopee Open API — đều có vendor cụ thể |
| Plan B có cost (tiền + thời gian) | ✓ | OpenAI: 7-8M VND, 1 tháng / Meta: 5-12M VND, 5-7 ngày deploy / Shopee: 11-13M VND, 7 ngày |
| Plan B deployable trong 7 ngày | ✓ | OpenAI: 24h switch vendor; Meta: 5-7 ngày; Shopee: 7 ngày CSV fallback |
| Critical Path có Legal Compliance | ✓ | Bước ② trên Critical Path — Nghị định 13/2023 + Meta App Review + Shopee Partners |
| Critical Path có Data Pipeline | ✓ | Bước ① — đầu tiên trên path |
| Có cascading failure analysis | ✓ | Dependency 1: phân tích kịch bản OpenAI + Anthropic cùng down → caching + Gemini Plan B |
| UI / Marketing parallel với Critical | ✓ | Block Non-Critical liệt kê rõ — marketing site, dashboard UI, docs, KOL đều parallel |

---

## 🎯 Câu chốt

> *"99% AI startup là ký sinh trùng. Mình cũng vậy — nhưng mình biết mình ký sinh trên ai, và đã viết sẵn Plan B cho từng vật chủ."*

Không có Plan B = không phải startup, là kinh doanh nhà thuê. ShopReplyAI có Plan B cho **cả 3 Tier 1 dependencies**, deploy được trong 24h-7 ngày tùy mức nghiêm trọng.
