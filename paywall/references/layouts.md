# Layout paywall

Khung: 360×800 dp chuẩn (một số app dùng 384). Kiểm tra thêm 360×640. Hệ màu và font dùng design system thật của app nếu có.

## Thứ tự dọc chuẩn

```
close (X, góc phải, hiện ngay)
hero (ảnh / Lottie / review wall)                ← 35–45% chiều cao ở 800, ≤40% ở 640
headline (+ sub)
trust (badge số liệu) và/hoặc benefits (3–6 dòng) ← có thể đổi chỗ với headline
selector (A/B/C/D)                                ┐
charge line                                       │ khối "pricing": pinned dưới khi nội dung dài,
cta                                               │ có layer lót blend từ content lên
legal                                             ┘
```

Màn bé (640): hero co lại, benefits có thể cắt còn 3 dòng hoặc cuộn; selector + CTA phải thấy ở default. Nếu không đủ chỗ, đặt scroll offset ban đầu để khối pricing lộ hết ("màn bé default scroll xuống một chút để hiển thị full phần gói giá").

## 4 kiểu selector

### A. Hai card ngang (radio)
Ví dụ: Weekly $4.99 | Yearly $14.99, pill `3-day free trial enabled` phía trên, tag `Most value` góc card; hoặc Monthly | Yearly với tag `3-day free trial` nằm trên viền card đang chọn.
Dùng khi: 2 gói, muốn so giá nhanh. Card chọn: viền accent + nền nhạt + radio đầy. Ẩn tag không được làm card đổi kích thước.

### B. List dọc (radio)
Ví dụ: Monthly $11.99 `Most popular`, `Show more plan` mở thêm Yearly $14.99 + Lifetime $29.99; hoặc Yearly $120/year `Only $12/month` + `75% OFF`, Monthly $18.18/month với giá neo `$218.16/year`.
Dùng khi: ≥3 gói hoặc cần dòng phụ dài (giá quy đổi, trial). Có thể thu gọn mặc định 1–2 gói, `Show more plan`.

### C. Tabs + toggle trial
Ví dụ: segment `Weekly | Yearly` trên cùng với tag `Most popular`, toggle `Free trial enabled`.
Dùng khi: muốn user tự bật trial (giảm tỷ lệ huỷ trial) hoặc test "trial opt-in". Không đủ điều kiện → ẩn toggle. Tab lifetime thay cho tabs subs khi bán IAP.

### D. Một card, không chọn
Ví dụ: Yearly $36.36 với ribbon trên card đổi theo state; hoặc `3-day free trial · then $4.99/week | $0.00 · for the next 3 days · NO PAYMENT NOW`.
Dùng khi: 1 gói focus (weekly trial onboarding), muốn CTA nhanh. State không trial: card thành `Weekly plan · Cancel anytime | $4.99 · charged today`.

Khuyến nghị mặc định cho onboarding paywall weekly-trial: D; cho paywall in-app có 2–3 gói: A (2 gói) hoặc B (≥3).

## Layout ngoài 4 kiểu trên

Bất kỳ layout nào cũng chấp nhận được nếu chỉ ra được 3 slot (Label / Deal line / Legal, xem SKILL.md §4) và dựng đủ 8 nhóm state. Ví dụ đã làm:

| Layout | Label ở đâu | Deal line | Vùng kể chuyện |
|---|---|---|---|
| toggle + 2 card | toggle `3-day free trial` + card `Weekly $3.99/week`, `Lifetime $59.99` | 1 dòng trên CTA | hero vật thể + wordmark |
| timeline | card weekly, timeline `Today $0.00 → Oct 9 $3.99/week` | 1 dòng trên CTA | headline "Here is exactly what you pay, and when." |
| lifetime track | card lifetime `Today $59.99 → After that nothing more to pay`, hàng phụ weekly | `Billed $59.99. One time payment.` | headline "Pro for life" |
| single + lifetime row | 1 dòng mô tả gói trong sub-headline, hàng lifetime | 1 dòng trên CTA | hero cảnh |

Khi tự do như vậy, bỏ các dòng trùng: không có dòng riêng dưới CTA ngoài Legal, card không ghi "Renews automatically" nếu deal line đã ghi.

## Hero

- 3 lớp xếp chồng: lớp dưới JPEG nền, lớp giữa Lottie chuyển động, lớp trên PNG trong suốt. Xuất riêng từng lớp cho dev.
- Layer lót neo cuối màn: gradient blend từ content xuống khối pricing.
- Hero có chữ (chữ tay, manga) phải có bản riêng từng ngôn ngữ. Ưu tiên hero không chữ hoặc chỉ ký hiệu + brand để khỏi dịch.
- Không dùng mockup UI app làm hero. Ảnh thật kích thước 360×(260–360) hoặc tỷ lệ 1:1 cho illustration. Ghi licence của ảnh/Lottie/font vào docs của app.
- Hero dạng dữ liệu: review wall cuộn, VIP card, headline xoay từ.

## Khối pricing

- Card: bo 12–16, viền 1–2 dp, trạng thái chọn = viền accent + nền tint 8–12%.
- Tag góc: chữ 10–11, nền accent hoặc đỏ cho % OFF, bám mép trên card, lệch phải.
- Charge line: 11–12 dp, màu muted, căn giữa, tối đa 2 dòng.
- CTA: cao 48–56, bo full hoặc 12, chữ 15–16 semibold. Loading: nền xám 30%, spinner 18 dp giữa.
- Legal: 10–11 dp, muted, phần Terms & Privacy Policy đậm hoặc gạch chân.

## Terms sheet

Bottom sheet cao ~55–60% màn, nền trắng (hoặc surface), bo góc trên 16–20, header: 2 tab + X; nội dung text 12 dp, mỗi đoạn cách 8. Overlay tối phía sau; paywall phía sau giữ nguyên state.

## Skeleton loading

- Card: giữ khung và bo góc, bên trong 1 spinner 16 dp căn giữa, không có text.
- CTA: nền xám, spinner.
- Pill/toggle trial: ẩn.
- Headline, hero, benefits, trust, legal: giữ nguyên.
