# State của một màn paywall

Tên phần tử dùng xuyên suốt skill:

| Phần tử | Mô tả |
|---|---|
| `close` | nút X góc trên phải, thấy ngay. |
| `hero` | ảnh / Lottie / review wall / VIP card. 3 lớp (xem layouts.md). |
| `headline` + `sub` | 1 từ nhấn màu accent. Sub ngắn hoặc bỏ. |
| `trust` | badge số liệu (Trusted by 30M+ users, 4.6/5 · 54.3K+ reviews, Over 9 years of growth), review card, bảng Free/Pro, timeline trial. |
| `benefits` | 3–6 dòng icon + text, hoặc 3 chip. |
| `selector` | khối chọn gói, 1 trong 4 kiểu A/B/C/D. |
| `trial-ui` | mọi thứ nói về trial: tag trên card, pill "3-day free trial enabled", toggle "Free trial enabled", badge "NO PAYMENT NOW", "$0.00 for the next 3 days", timeline trial. |
| `charge` | 1–2 dòng nhỏ ngay trên CTA, nói đúng số tiền sẽ bị trừ và khi nào (= Deal line). |
| `cta` | nút chính, full width. |
| `legal` | "By continuing you agree to our Terms & Privacy Policy". |
| `terms-sheet` | bottom sheet 2 tab Terms / Privacy policies. |

## Ma trận state

Ký hiệu: ✓ hiện, ✗ ẩn, ◌ skeleton.

| State | trial-ui | giá trên card | charge | cta | legal | ghi chú |
|---|---|---|---|---|---|---|
| **1. Có free trial, gói focus** | ✓ | ✓ | `{price}/{period} after {days}-day free trial ends. Auto-renew. Cancel anytime.` | `Try for free` | ✓ | gói focus được chọn sẵn |
| **2. Có free trial, chọn gói khác** | ✓ chỉ trên gói có trial offer | ✓ | theo gói đang chọn: gói có trial → như 1; gói không có trial offer → `{price}/{period}. Auto-renew. Cancel anytime.` | gói có trial → `Try for free`; không → `Continue` | ✓ | tag trial dính vào card, không dính vào user |
| **3. Không đủ điều kiện trial** | ✗ toàn bộ | ✓ | `{price}/{period}. Auto-renew. Cancel anytime.` | `Continue` | ✓ | layout C: ẩn toggle, có thể thay bằng dòng mờ "Free trial unavailable" hoặc bỏ hẳn |
| **4. Gói lifetime** | ✗ | ✓ | `Billed {price}. One time payment.` | `Continue` | ✓ | card sub-line `One time purchased`; không có "auto-renew" ở đâu cả |
| **5. Chưa load được billing** | ◌ | ◌ | ✗ (hoặc ◌ 1 dòng) | nút xám, spinner, disabled | ✓ | headline, hero, trust, benefits giữ nguyên; card giữ khung + spinner nhỏ, không có text giá |
| **6. Terms sheet** | như state nền | như nền | như nền | như nền | ✓ | overlay tối 40–60%, sheet trắng bo góc trên, tab đang chọn gạch chân |
| **7a. Màn bé 360×640 default** | theo state 1 | ✓ | ✓ | ✓ | ✓ | hero cao tối đa ~40% màn; phải thấy card + CTA |
| **7b. Màn to 360×800 default** | theo state 1 | ✓ | ✓ | ✓ | ✓ | |
| **7c. Scroll max** | | | | | | nội dung cuộn hết; selector + charge + CTA + legal là khối pinned dưới nếu nội dung dài |
| **8. Localisation** | ✓ | giá nội tệ | dịch | dịch | dịch | cùng layout; kiểm tra text dài (DE, PT) và CJK (KR, JP) không vỡ card |
| ~~9. Load billing lỗi~~ (không dựng: app dùng timeout) | ✗ | ✗ | `We couldn't load the plans. Check your connection and try again.` | `Try again` | ✓ | chỉ giữ chuỗi để tham khảo |
| *10. Đang mở Google Play (tuỳ chọn)* | như nền | như nền | như nền | spinner + `Opening Google Play…`, disabled | ✓ | sau khi bấm CTA |

## Rule quyết định (viết cho dev)

```
eligible   = user chưa từng dùng trial của app (store trả về offer trial cho product)
plan.trial = gói này có trial offer trong Play Console / App Store Connect
lifetime   = gói là one-time purchase

billing chưa load            → state 5
billing lỗi                  → không có màn lỗi: giữ state 5 (loading) tới timeout của app
plan.lifetime                → state 4
eligible && plan.trial       → state 1/2 (trial)
else                         → state 3 (no trial) cho gói đó
```

Mặc định khi không có Remote Config: dùng gói focus và giá cứng trong app. Ghi rõ trên bảng "Gói giá mặc định khi không load được remote config".

## Từng kiểu selector phản ứng thế nào

| Kiểu | Trial hiện ở đâu | Khi không đủ điều kiện | Khi lifetime |
|---|---|---|---|
| A. 2 card ngang | tag nhỏ trên góc card (`3-day free trial`) hoặc pill trên hàng card (`3-day free trial enabled`) | ẩn tag/pill, card giữ nguyên kích thước (không nhảy layout) | card lifetime: sub-line `One time purchased`; thường chỉ 1 card hoặc 2 card lifetime |
| B. list dọc radio | sub-line trong card đang chọn `3-day free trial` / `3-day free trial enabled` | sub-line đổi thành giá quy đổi (`Only $12/month`) hoặc `Auto renewal, cancel anytime` | card `Lifetime · $X · One time purchased` |
| C. tabs + toggle | toggle `Free trial enabled` (on) / `Not sure yet? Enable free trial` (off) | ẩn toggle hoặc dòng mờ `Free trial unavailable` | tab đổi thành `Lifetime`, không toggle |
| D. 1 card | ribbon trên card `3-day free trial` + badge `NO PAYMENT NOW` + `$0.00 for the next 3 days` | card thành `Weekly plan · Cancel anytime · $4.99 charged today` | card `Lifetime · One time purchased · $X` |

## Biến thể ngoài 4 kiểu (đã gặp khi AI dựng một mẻ 12 paywall)

- **Toggle trial đổi gói**: layout weekly-trial + lifetime, có công tắc `3-day free trial`. Tắt công tắc = tự chuyển chọn sang Lifetime (nhãn `Tắt công tắc trial → tự chuyển gói Lifetime`). Bật lại = quay về Weekly trial. Công tắc là Label của fact trial, nên card weekly không ghi trial nữa.
- **Timeline thanh toán**: `Today · $0.00` → `Oct 9 · $3.99/week` (hoặc `Today · $59.99` → `After that · nothing more to pay`). Timeline là vùng kể chuyện, đã nói trial + giá sau trial; Deal line vẫn giữ nguyên câu đủ 6 việc; Label trên card không thêm nhãn trial nữa.
- **Lifetime-first**: gói focus là Lifetime (card to, `One time purchased`), weekly là hàng phụ `$3.99/week`. State `Chọn gói còn lại` = chọn weekly: CTA đổi `Continue` → `Try for free` nếu đủ điều kiện trial, deal line đổi theo. Chọn gói phụ không đổi layout, khối focus chỉ mờ đi.

## Nhãn frame trên bảng (giữ đúng chữ)

`Có free trial`, `Select gói week free trial`, `Select gói year`, `Không có free trial`, `TH gói month ko đủ đkien trial`, `Gói lifetime`, `Chưa load được billing`, `Bật term & policies`, `Chọn gói còn lại`, `Tắt công tắc trial → tự chuyển gói Lifetime`, `Default view màn to`, `Default view màn bé`, `Scroll max view màn to`, `Scroll max view màn bé`, `Demo tiếng Hàn`, `Gói week` (khi có bảng gói week riêng).
