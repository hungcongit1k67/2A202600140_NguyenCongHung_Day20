# Roadmap Now / Next / Later — ShopReplyAI

- **Sinh viên:** Nguyễn Công Hùng — 2A202600140
- **Ngày:** Day 20 — Workshop 2
- **Nguyên tắc:** Mỗi ô là *vấn đề* (problem) — không phải *tính năng* (feature). KHÔNG ngày tháng. Chỉ thứ tự ưu tiên.

---

## 🟢 NOW — đang code, 1-3 tháng

### NOW-1
**Vấn đề:** Shop pilot mất ~2h/ngày trả lời câu hỏi lặp về giá / size / còn hàng / phí ship. Engine hiện chỉ chạy ổn định trên Facebook Messenger (30 shop), Zalo OA chưa lên — trong khi 100% shop nhỏ Việt Nam đều có khách hỏi qua Zalo.

**Đang code:** Mở rộng auto-reply engine sang **Zalo OA** + bật **after-hours fallback mode** (tối / livestream / ngoài giờ shop online). Mục tiêu outcome: giữ tổng inbox time của shop ≤ 35 phút/ngày trên cả 2 kênh, không chỉ Facebook.

---

### NOW-2
**Vấn đề:** Chủ shop sợ AI tự gửi câu trả lời sai về **đổi trả / khiếu nại / giảm giá sâu / cam kết bảo hành** → mất khách thay vì giữ khách. Hiện tại AI đang trả lời "all-or-nothing" — không có cơ chế chặn câu nhạy cảm. Đây là rào cản số 1 ngăn shop chuyển từ pilot sang trả phí.

**Đang code:** **Sensitive-message approval queue** — AI tự nhận diện intent nhạy cảm (return / refund / complaint / heavy discount), giữ câu trả lời lại, push notification cho chủ shop duyệt trong 5-10 phút trước khi gửi đi. Có audit log để shop review về sau.

---

## 🟡 NEXT — sẽ làm sau khi NOW xong, 3-6 tháng

### NEXT-1
**Vấn đề:** Shop bán đa kênh (Shopee + Facebook + TikTok Shop) phải cập nhật giá và tồn kho thủ công ở nhiều nơi. Khi giá lệch, AI lấy thông tin cũ → trả lời sai → khách mất tin tưởng và shop mất uy tín. ~50% shop pilot phản hồi đây là pain họ chấp nhận trả thêm tiền để giải quyết.

**Sẽ làm:** Catalog auto-sync giữa các kênh bán → AI luôn dùng dữ liệu mới nhất. Cách triển khai cụ thể (Open API, partner integration, hay scrape có consent) sẽ chốt khi research thêm — *xem dependency map về Shopee Open API access*.

---

### NEXT-2
**Vấn đề:** Chủ shop trả lời được tin nhắn rồi, nhưng không biết câu nào AI làm tốt, câu nào AI sai (chủ shop phải sửa lại). Hiện chỉ có log thô — chủ shop không có thời gian và không có công cụ để retrain AI sau pilot. Theo thời gian AI bị "đứng" độ chính xác.

**Sẽ làm:** Insight dashboard cho chủ shop — tỷ lệ tự động hóa, top intent câu hỏi, các câu chủ shop hay sửa lại. Cho phép shop tự retrain prompt / few-shot examples mà không cần dev support.

---

### NEXT-3
**Vấn đề:** Khi khách hỏi nhưng chưa thanh toán, hoặc khách bỏ giỏ giữa chừng, shop hay quên follow-up. Doanh thu rò rỉ ở đây cao hơn so với việc trả lời chậm. Shop pilot có yêu cầu chức năng nhắc đơn nhưng chưa có công cụ.

**Sẽ làm:** Mini "chốt đơn" hooks — AI nhắc khách chưa thanh toán sau 2-6 tiếng, gợi ý sản phẩm liên quan dựa trên câu hỏi gần nhất. Cần A/B test xem có thật sự tăng conversion (không chỉ thêm spam).

---

## 🔴 LATER — tầm nhìn dài, 6-18 tháng — *có thể bỏ*

### LATER-1
**Vấn đề:** Khách hàng Việt Nam (đặc biệt vùng tỉnh và mua hàng livestream) ngày càng nhắn voice message thay vì gõ chữ. AI hiện không hiểu voice → mất hẳn nhóm khách hàng này. Tỉ trọng voice message dự kiến tăng cùng tốc độ phổ cập livestream commerce.

**Có thể làm:** Tích hợp voice-to-text + AI hiểu được giọng vùng miền (miền Tây, miền Trung, Bắc Trung Bộ). Cần research thêm chi phí ASR cho tiếng Việt + accent — chưa rõ liệu margin có giữ được trên 50% không. **Có thể bỏ** nếu chi phí ASR khiến gross margin về dưới ngưỡng break-even.

---

### LATER-2
**Vấn đề:** Một số shop muốn AI có "cá tính riêng" cho thương hiệu — shop trẻ em nói khác shop mỹ phẩm cao cấp. Hiện AI dùng chung tone → cảm giác hơi máy móc với shop premium.

**Có thể làm:** Voice / tone training light per shop (RAG riêng hoặc fine-tune nhẹ). **Có thể bỏ** — RICE score thấp (5), chưa thấy shop nào sẵn sàng trả thêm tiền cho tính năng này. Có thể giải quyết "đủ tốt" bằng template prompt nâng cao mà không cần fine-tune riêng.

---

## Quy tắc đã tuân thủ

| Quy tắc | Ô nào? | Cách kiểm tra |
|---|---|---|
| Mỗi ô là vấn đề (problem), không phải tính năng | Tất cả | Mỗi ô bắt đầu với "Vấn đề:" mô tả pain |
| KHÔNG ngày tháng | Tất cả | Không có "tháng 8" / "Q4" / "ngày 15" — chỉ "1-3 tháng" / "khi NOW xong" |
| NOW có 1-3 items (focused) | NOW | 2 items — đủ tập trung, đủ tham vọng |
| NEXT có 2-4 items (medium horizon) | NEXT | 3 items — Catalog sync / Insight / Chốt đơn |
| LATER có honest disclaimer "có thể bỏ" | LATER | Cả 2 ô đều có "Có thể bỏ" với điều kiện rõ ràng |
| Ngôn ngữ business, không tech jargon | Tất cả | Không nhắc model accuracy / latency / API endpoint trong các ô |
