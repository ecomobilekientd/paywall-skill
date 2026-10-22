---
name: paywall
description: Kit chuẩn để thiết kế màn paywall (IAP / subscription) cho app mobile. Dùng skill này mỗi khi người dùng nhắc tới paywall, "pw", màn mua gói, màn subscription, màn chọn gói, free trial, onboarding paywall, pricing screen, hoặc hỏi về các state / case của paywall (có free trial, không đủ điều kiện free trial, đang load billing, chọn gói subs hay lifetime, popup Terms & Privacy). Kể cả khi chỉ xin copy, charge line, text policy, hay một concept paywall mới cũng phải đọc skill này trước. Skill cho ra đủ bộ state với copy chuẩn, text pháp lý chuẩn, dựng HTML board trước, Figma sau khi chốt.
---

# Paywall kit

Mục đích: mọi paywall mới đều ra cùng một bộ state, cùng quy tắc copy và cùng cách trình bày bảng, dù hero và style khác nhau. Skill rút từ hơn 20 bảng paywall đã làm cho các app note, QR scanner, flashlight (một phần do AI dựng rồi designer tinh chỉnh).

Đọc theo nhu cầu:
- `references/states.md` — bảng state đầy đủ, từng phần tử thay đổi thế nào ở mỗi state. Đọc trước khi dựng.
- `references/copy.md` — kho chuỗi chuẩn (CTA, deal line, tag, dòng pháp lý, nội dung bottom sheet Terms). Đọc khi viết copy.
- `references/layouts.md` — 4 kiểu chọn gói (A cards, B list, C tabs + toggle, D single plan), hero 3 lớp, responsive. Đọc khi chọn layout.
- `references/sources.md` — nguồn tham chiếu paywall (Mobbin MCP, gallery của team, web) và cách kéo 5–10 tham chiếu trước khi đề xuất. Đọc ở bước 2.
- `assets/board-template.html` — template HTML có engine render toàn bộ state từ một object CONFIG. Copy ra, sửa CONFIG và style, không viết engine mới.

## 0. Cần có gì để chạy

- Browser để mở HTML board (không cần server).
- Nguồn giá thật của app: Play Console, App Store Connect hoặc RevenueCat. Không bịa giá.
- Tuỳ chọn (cách kết nối ở SETUP.md của repo): Figma MCP (`use_figma`) để dựng lên Figma sau khi chốt; Mobbin connector để kéo tham chiếu; skill tạo ảnh (ví dụ `imagegen`) cho hero. Không có skill tạo ảnh thì dùng ảnh stock có licence hoặc Lottie, ghi licence vào docs của app.

## 1. Output mặc định

1. **HTML board trước**: một trang, mỗi state là một phone frame 360 dp, có nhãn tiếng Việt dưới mỗi frame (nhãn chuẩn ở mục 3). Dựng từ `assets/board-template.html`. Chỉ vẽ Figma sau khi designer đã xem board và chốt hướng.
2. **Figma sau khi "chốt"**: đưa lên file Figma của app, trong section mới, theo đúng bố cục bảng ở mục 6. Không vẽ lên section của người khác.
3. **Handoff cho dev** nếu được hỏi: chỉ logic, rule, case; không code. Bảng state ở `states.md` là tài liệu handoff.

## 2. Input phải có trước khi dựng

Hỏi gọn trong một tin nhắn nếu thiếu; tự suy nếu có thể suy từ app:

| Input | Ghi chú |
|---|---|
| App + placement | onboarding paywall, in-app paywall, feature-gate... Ảnh hưởng tới việc có nút X ngay hay không. |
| Danh sách gói | id, tên (Weekly / Monthly / Yearly / Lifetime), giá, chu kỳ, gói nào có trial offer, gói nào là focus. |
| Trial | số ngày (mặc định 3), gói nào được trial. Lifetime không bao giờ có trial. |
| Có lifetime không | nếu có thì phải có state riêng. |
| Kiểu chọn gói | A / B / C / D ở `layouts.md`. Nếu chưa rõ, đề xuất 2–3 phương án để designer chốt. |
| Hero | illustration / photo / abstract / Lottie. Không dùng mockup UI app. Ảnh thật, không placeholder, không khung xám. |
| Thị trường, ngôn ngữ | để demo 1 bản localisation (thường KR hoặc JP) và chọn giá hiển thị. |

## 3. Bộ state bắt buộc

Mỗi paywall phải có đủ các frame sau, đặt nhãn đúng như cột "Nhãn":

| # | State | Nhãn trên bảng | Điểm khác biệt chính |
|---|---|---|---|
| 1 | Trial, gói focus đang chọn | `Có free trial` | Tag/pill trial hiện, CTA "Try for free", charge line "…after {days}-day free trial ends" |
| 2 | Trial, chọn gói khác (nếu có ≥2 gói) | `Select gói year` (theo tên gói) | Mỗi gói một frame. Gói không có trial offer thì hiện như state 3 dù user đủ điều kiện |
| 3 | Không đủ điều kiện trial | `Không có free trial` | Ẩn toàn bộ tag/pill/toggle trial, CTA "Continue", charge line không nhắc trial |
| 4 | Chọn gói lifetime | `Gói lifetime` | Sub-line "One time purchased", CTA "Continue", không trial, không "auto-renew" |
| 5 | Chưa load được billing | `Chưa load được billing` | Giá và tag thành skeleton, CTA xám + spinner, headline/hero/legal giữ nguyên |
| 6 | Popup Terms & Privacy | `Bật term & policies` | Bottom sheet 2 tab Terms / Privacy policies, nút X, nội dung chuẩn trong `copy.md` |
| 7 | Responsive | `Default view màn to`, `Default view màn bé`, `Scroll max view màn to/bé` | 360×800 và 360×640; màn bé phải thấy giá + CTA ở default |
| 8 | Localisation | `Demo tiếng Hàn` (hoặc ngôn ngữ thị trường chính) | Cùng layout, chuỗi dịch, giá nội tệ |

Không làm màn `Load billing lỗi` (retry): app dùng timeout cho billing, giữ state loading tới khi timeout. Tuỳ chọn, thêm khi paywall có logic tương ứng: `Đang mở Google Play` (CTA spinner sau khi bấm), `Gói week` (bảng riêng khi test thêm plan). Chi tiết từng phần tử ở `references/states.md`.

## 4. Thông tin bắt buộc và cơ chế chống lặp (đọc kỹ)

Mỗi màn paywall phải nói đủ 6 việc cho gói đang chọn, và **mỗi việc chỉ nói ở đúng một chỗ**. AI hay lặp (một mẻ paywall do AI dựng: "3-day free trial" 3 lần, "auto-renew / cancel anytime" 3 lần trên cùng một màn). Chống lặp bằng 3 slot cố định:

| Slot | Ở đâu | Chứa gì | Không được chứa |
|---|---|---|---|
| **Label** | trên card / tab / toggle của gói | tên gói, giá + chu kỳ, và **tối đa 1 nhãn ngắn**: `3-day free trial` hoặc `Only $12/month` hoặc `One time purchased` | câu đầy đủ, "auto-renew", "cancel anytime" |
| **Deal line** | 1 dòng ngay trên CTA | **câu duy nhất nói đủ cả 6 việc**: trial mấy ngày → sau đó trả bao nhiêu / chu kỳ → auto-renew → cancel anytime. Lifetime: giá + one time payment. Có 3 bản ngắn/vừa/dài cùng nghĩa, chọn theo chỗ còn trống (copy.md) | gì khác |
| **Legal** | 1 dòng dưới CTA | `By continuing you agree to our Terms & Privacy Policy` (link mở bottom sheet) | bất kỳ fact nào về giá, trial, renew; không đoạn văn dài |

6 việc phải có (gói subs): (1) chu kỳ, (2) giá, (3) có trial thì mấy ngày, (4) hết trial trả bao nhiêu, (5) auto-renew, (6) cancel anytime. Gói lifetime: (1) giá, (2) one time payment, và tuyệt đối không có renew/trial/chu kỳ.

Quy tắc đếm: mỗi fact xuất hiện tối đa 2 lần trên màn: 1 lần dạng nhãn ngắn trong Label (tuỳ chọn) + 1 lần trong Deal line (bắt buộc). Không có dòng thứ ba (không thêm dòng dưới CTA, không nhắc lại trong card thứ hai, không trong legal). Vùng kể chuyện (headline, hero, timeline "Today / Oct 9") được nhắc trial như một ý tưởng; khi đó vùng kể chuyện chính là nhãn ngắn, card **không** gắn tag trial nữa để tổng vẫn ≤ 2.

Template có hàm `lint()` đếm tự động và in cảnh báo dưới từng frame; không giao board khi còn cảnh báo.

### Copy (tóm tắt, đầy đủ ở copy.md)

- Copy in-app chỉ tiếng Anh, từ đơn giản; app tự dịch. Headline có 1 từ nhấn màu.
- CTA: có trial = **"Try for free"**, mọi gói không trial (kể cả lifetime) = **"Continue"**. Không "Subscribe", "Buy", "Start trial". "Try with $0.00" chỉ khi card đã hiện "$0.00".
- Không đặt "Restore purchase" ở footer paywall; restore nằm ở Settings.
- Tag chỉ dùng từ kho: `3-day free trial`, `Most popular`, `Most value`, `Best value`, `75% OFF`, `NO PAYMENT NOW`. Một card tối đa 1 tag góc + 1 nhãn ngắn.
- Trust: số thật của app. Không bịa số; chưa có thì hỏi.

## 4b. Terms & Privacy: luôn là bottom sheet

Mọi app dùng chung một kiểu: bấm link ở Legal → bottom sheet 2 tab `Terms` | `Privacy policies.`, nút X, nội dung 5 đoạn chuẩn trong `copy.md`. Thay `{app}` bằng tên hiển thị trong app, `{store}` = Google Play hoặc App Store theo nền tảng. Đoạn văn pháp lý dài (kiểu "Payment will be charged to your Google Play account…") chỉ được nằm trong sheet, không nằm trên paywall.

## 4c. Áp vào layout khác

Skill không khoá layout. 4 kiểu A/B/C/D trong `layouts.md` là điểm xuất phát; layout mới (timeline thanh toán, lifetime-first, toggle đổi gói, sheet đè app, long-scroll…) vẫn hợp lệ nếu khai báo được **3 slot ở đâu** và **8 nhóm state** vẫn dựng được. Cách làm:

1. Vẽ layout tự do, rồi chỉ ra: Label ở phần tử nào, Deal line ở đâu, Legal ở đâu, nút X ở đâu, phần nào là vùng kể chuyện.
2. Chạy qua bảng state ở `states.md`: với mỗi state, phần tử nào đổi. Nếu một state không dựng được (ví dụ không có chỗ cho skeleton) thì layout phải sửa.
3. Trong template: giữ engine, viết lại hàm `CONFIG.renderSelector(ctx)` cho khối chọn gói; charge/CTA/legal/sheet/lint dùng lại nguyên.

## 5. Quy trình

1. Thu input (mục 2). Nếu thiếu gói/giá, hỏi; không bịa giá.
2. Kéo 5–10 paywall tham chiếu cùng ngành / cùng mô hình giá theo `references/sources.md` (Mobbin MCP nếu có, rồi `gallery/index.md`), tóm tắt 1 bảng ngắn kèm link. Rồi đề xuất 2–3 hướng (layout + hero + headline) ngắn gọn, có ưu nhược, có khuyến nghị, nói rõ khác gì so với tham chiếu. Chờ designer chốt. Nếu đã chỉ rõ hướng thì bỏ bước này.
3. Copy `assets/board-template.html` sang thư mục dự án, sửa `CONFIG` và CSS theme. Hero: tạo ảnh thật (liệt kê ảnh cần tạo trước).
4. Mở board trong browser, kiểm tra bằng checklist mục 7, chụp ảnh gửi designer.
5. Sau "chốt": dựng Figma theo bố cục mục 6, tên frame = `PW <n>` hoặc theo convention file, nhãn tiếng Việt dưới frame, không timestamp hay ghi chú dài trên canvas.
6. Nếu được yêu cầu handoff: xuất bảng state + rule copy, ngắn.

## 6. Bố cục bảng (HTML và Figma giống nhau)

```
Hàng 1  Demo responsive:   Default view màn to | Default view màn bé | Scroll max view màn to | Scroll max view màn bé | Bật term & policies
Hàng 2  Demo các gói giá:  Có free trial (1 frame/gói) | Không có free trial | Gói lifetime | Chưa load được billing | Demo tiếng Hàn
Hàng 3  Hero banner resource: các lớp hero (JPEG nền, Lottie, PNG trên), layer lót blend, background
```

Tiêu đề hàng dùng chữ lớn, nhãn frame chữ nhỏ ngay dưới frame. Không ghi chú giải thích dài trên canvas.

Khi có nhiều paywall trong một mẻ: mỗi paywall một hàng, cột = `Default` | `Chọn gói còn lại` | `Không đủ điều kiện trial` | `Tắt công tắc trial` (nếu có toggle) | `Chưa load được billing`, cột phải là resource ảnh + Lottie + link web demo.

## 6b. Quy chuẩn dựng Figma

- Spacing theo lưới 4: mọi gap, padding, radius, chiều cao control là bội số của 4, ưu tiên 4 · 8 · 12 · 16 (không dùng 5, 6, 9, 15, 58…).
- Line height của mọi text trên board paywall: **Auto**. (Nếu app có design system với text style line height cố định thì theo DS.)
- Cỡ chữ luôn là **số chẵn**. Ví dụ đã dùng: tiêu đề 26, phụ đề 14, chip 12, giá 14, CTA 20, dòng phí/pháp lý 12.
- Auto layout dựng tay theo vai trò, không chuyển thẳng từ DOM: Root (dọc, 360×h, clip) → Background (absolute) · Status bar · Hero (fill) · Content · Footer · Close (absolute, neo phải-trên) · Dim + Terms sheet (absolute, neo đáy). Đặt tên layer theo vai trò, không có frame bọc thừa.
- Sau khi dựng, chạy kiểm tra: spacing % 4 == 0, line height = Auto, cỡ chữ chẵn cho mọi node; còn lỗi thì sửa trước khi báo.
- Giá trị mẫu đã dùng: Content gap 16 (màn bé 8), Chip pad 4/12/4/8 gap 4, Plan card pad 16 radius 16, CTA cao 56 (màn bé 48) radius h/2, Charge gap 4, Terms sheet pad 24/16/16/16.
- HTML theo Figma: Figma là chuẩn, HTML board dùng cùng giá trị (CSS line-height `normal` tương ứng Auto). Sửa Figma thì sửa HTML theo.
- Tên paywall trên Figma: chỉ `PW <số>`, không ghi mã nội bộ hay tên người.

## 7. Checklist trước khi giao

- [ ] Đủ 8 nhóm state ở mục 3, nhãn đúng.
- [ ] Nút X thấy ngay ở frame đầu tiên (không delay, không ẩn) trừ khi brief nói khác.
- [ ] Ở 360×640 default view vẫn thấy giá và CTA không cần cuộn.
- [ ] State không trial: không còn chữ "free", "trial", "$0.00" ở bất cứ đâu trên màn.
- [ ] State lifetime: không có "auto-renew", "trial", "/year".
- [ ] State loading: không có giá cứng nào lọt ra; CTA không bấm được.
- [ ] Deal line khớp với gói đang chọn (đổi gói là đổi deal line) và là câu duy nhất nói đủ 6 việc.
- [ ] Không fact nào xuất hiện quá 2 lần; không có dòng phụ dưới CTA ngoài Legal; `lint()` không cảnh báo.
- [ ] Legal chỉ 1 dòng + link; đoạn văn dài nằm trong bottom sheet; tên app và store trong sheet đúng.
- [ ] Dòng pháp lý và popup Terms có mặt ở mọi state kể cả loading.
- [ ] Giá đúng với Play Console / App Store / RevenueCat của app; không bịa.
- [ ] Hero là ảnh thật hoặc Lottie, không khung xám.
