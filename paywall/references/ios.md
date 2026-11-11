# Paywall iOS — guideline (rút từ mẻ Tistora / Camera Timestamp, 10/2026)

Bổ sung cho `SKILL.md` khi app là iOS. Phần chung (8 nhóm state, 3 slot chống lặp, copy, lint) vẫn áp nguyên. File này ghi chỗ iOS khác Android (chốt 28/10/2026), và bài học từ 4 vòng board của Tistora (22–27/10/2026). Các số 360×800 / 360×640 và mục 4b (bottom sheet Terms) trong `SKILL.md` chỉ cho Android.

## 1. Khung và cỡ màn (point, portrait)

- **Default 402×874** (iPhone 16 Pro / 17 / 17 Pro). Dùng cho prototype, hàng "Demo các gói giá", demo ngôn ngữ và frame Figma. Không dùng các khung không khớp máy nào (ví dụ 402×862).
- Hàng **Demo responsive = đủ 9 cỡ** iOS 26 còn hỗ trợ (iPhone 11 trở lên), không chỉ to/bé:

| Cỡ | Máy | Safe area trên | Dưới | Tai thỏ |
|---|---|---|---|---|
| 440×956 | 16 Pro Max, 17 Pro Max | 62 | 34 | Dynamic Island |
| 420×912 | iPhone Air | 68 | 34 | Dynamic Island |
| 430×932 | 14 Pro Max, 15 Plus, 15 Pro Max, 16 Plus | 59 | 34 | Dynamic Island |
| **402×874** | 16 Pro, 17, 17 Pro | 62 | 34 | Dynamic Island |
| 393×852 | 14 Pro, 15, 15 Pro, 16 | 59 | 34 | Dynamic Island |
| 390×844 | 12, 13, 14, 16e, 17e | 47 | 34 | notch |
| 414×896 | iPhone 11 | 48* | 34 | notch |
| 375×812 | 12 mini, 13 mini | 44* | 34 | notch |
| 375×667 | SE 2, SE 3 (nút Home) | 20 | 0 | không |

`*` số quen dùng, chưa tìm được nguồn xác nhận. Nguồn cho các số còn lại: iosref.com, useyourloaf.com, ios-resolution.com; Apple HIG cho quy tắc. Kiểm tra lại khi Apple ra máy mới.

- Khung cao ≤ 700pt (375×667) dùng bộ metric "màn bé" (row 32–36, card gói 46–48, gap 6–8). Các cỡ còn lại dùng metric màn to; phần dư rơi vào hero hoặc vùng cuộn, footer vẫn ghim đáy.
- Status bar vẽ đúng safe area trên; home indicator 134×5 trong vùng 34 dưới; SE không có home indicator, chừa 12.
- HIG: touch target mặc định 44×44pt (tối thiểu 28). HIG không cho số khoảng cách CTA → home indicator, dùng vùng 34 làm mức chừa.
- Board HTML: engine khung đọc một mảng `SIZES` (w, h, top, bot, cut) và đặt biến `--w/--h/--top/--bot` trên `.pw`, để một layout render được cả 9 cỡ.

## 2. Khác Android

| Việc | Android (SKILL.md) | iOS |
|---|---|---|
| Legal | 1 câu "By continuing you agree to our Terms & Privacy Policy", link mở bottom sheet | Hàng 3 link nhỏ cạnh nhau, căn giữa, **gạch chân**: **`Terms of Service   Privacy Policy   Restore`** (không câu dẫn, không dấu chấm giữa; SF Pro Regular 13/16 màu muted, gap 12; màn ≤700: 12/16 gap 10; cách CTA/dòng giá 12) |
| Restore | không có trên paywall (ở Settings) | **Có**, là link thứ 3 trong hàng legal, không thành dòng riêng. Bấm gọi StoreKit restore, board không cần frame riêng. Apple yêu cầu có cách restore trên màn đăng ký |
| Terms & Privacy | bottom sheet 2 tab trong app | **Không có sheet.** Link mở trang web trong Safari view (SFSafariViewController). **Không demo trên board**: không frame "Bấm Terms of Service", không mock Safari; trong prototype bấm chỉ ghi log |
| Mở store | "Đang mở Google Play" | "Opening App Store…" (dim + spinner), rồi paywall đóng |
| Store trong copy | Google Play | App Store; "Manage or cancel in Settings › Apple ID › Subscriptions" nếu cần nhắc |
| Card gói có trial | tag "3-day free trial", giá thật + chu kỳ | tag "3-day free trial", giá **0** (ví dụ `฿0`, không đơn vị chu kỳ), dòng phụ `{price}/{period} after trial ends` |
| Deal line | 1 câu đủ 6 việc | khi các fact trial đã nằm trên card thì chỉ còn `Auto-renews, cancel anytime.`; luôn chừa chỗ 2 dòng (32) để khối dưới không nhảy khi đổi gói |
| Dòng trấn an (xanh lá) | `No payment due now` chỉ khi trial, ẩn khi không | luôn hiện: `No payment due now` khi gói đang chọn có trial, `Cancel anytime` khi không (không đủ điều kiện trial, Yearly, Lifetime) |
| Cỡ chữ | số chẵn, line height Auto | theo text style của app (mục 3) |
| Status bar | Android | iOS (giờ trái, pin phải, Dynamic Island / notch theo cỡ) |

Lưu ý quy tắc đếm fact (SKILL.md §4): giá 0 + dòng "after trial ends" trên card là Label của trial; deal line vì thế không nhắc lại trial, tổng vẫn ≤ 2.

## 3. Design system: theo app, không theo skill

- Dùng text style của app. App iOS thường theo cỡ chữ Apple (28 / 22 / 17 / 15 / 13 / 12, line height cố định). Luật "cỡ chữ chẵn" trong `SKILL.md` §6b là của board Android, **không áp** cho app iOS có design system riêng.
- Màu, spacing, radius lấy từ variable của file Figma app trước khi dựng. Đọc bằng Figma MCP, chỉ đọc. Không đoán màu từ ảnh chụp app. Vòng đầu Tistora đoán accent cam, nhưng thật ra là xanh iOS #007AFF.
- Nếu design system chỉ có một mode (thường là sáng) thì paywall dùng đúng mode đó. Không tự dựng dark mode khi chưa được yêu cầu.
- Font: SF Pro cho UI (fallback `-apple-system`), SF Mono cho số kỹ thuật hoặc stamp. Font trang trí mới (viết tay, display đậm) chỉ dùng trên board concept, ghi licence vào docs của app.

Thông số đã chốt ở Tistora, dùng làm mặc định cho app iOS khác nếu chưa có quy định:

| Phần tử | Giá trị |
|---|---|
| CTA | pill, cao **56 ở mọi cỡ màn** (kể cả 375×667; màn bé thu hero, không thu CTA), màu brand của app, chữ 17 semibold |
| Dòng trấn an | **xanh lá toàn bộ, cả icon và chữ** (`state/success`). Lưu ý độ tương phản ~2:1 trên nền trắng, cần kiểm theo checklist SKILL.md §7 |
| Nhãn trial (ribbon, badge) | màu phụ riêng (Tistora: cam #E8890C), không dùng cho CTA |
| Kiểu card gói | tuỳ layout, không ép thống nhất |
| Nút X | thấy ngay ở frame thiết kế (Remote Config có thể delay, ghi riêng) |

## 4. Bố cục bảng iOS (HTML và Figma)

```
Hàng 0  Prototype (bấm thử): 1 khung 402×874 sống, chọn cỡ / trial / billing
Hàng 1  Demo các gói giá (402×874): Có trial | Không đủ điều kiện trial | (mỗi gói thêm 1 frame) | Đang load billing | Mở App Store
Hàng 2  Demo responsive: 9 cỡ ở mục 1, có trial, nhãn "cỡ · máy · safe area trên/dưới"
Hàng 3  Demo ngôn ngữ (402×874)
Hàng 4  Hero banner resource
```

Figma: Root 402×874, Status bar = safe area trên, Footer kết thúc trước vùng 34; không có frame Terms sheet.

## 5. Bài học dữ liệu (Tistora, 28/09–21/10/2026, ngoài VN)

- **CTR không phải nút thắt; sheet thanh toán Apple mới là chỗ rơi.**
  - 49% người thấy paywall bấm CTA, nhưng 84% người bấm huỷ ở sheet Apple (phần lớn `user_cancelled`).
  - Hệ quả: payrate (trial / người thấy) là thước đo chính, CTR chỉ là phụ.
  - Thiết kế phải làm cho sheet Apple không bất ngờ: giá, ngày trừ tiền và cách huỷ đã thấy trước khi bấm. Có thể dùng timeline trial, giá quy ra ngày, hoặc câu huỷ cụ thể.
- **Ngữ cảnh mạnh hơn giao diện.**
  - Cùng một gói, paywall chặn giữa việc (limit 8 ảnh) cho payrate 10.4%, onboarding chỉ 4.5%.
  - Paywall onboarding đặt trước màn chính làm tỉ lệ dùng tính năng lõi lần đầu tụt từ 70% xuống 50–61%.
- **Exit paywall chào cùng gói không thêm trial.** Nếu làm exit thì chào bậc khác.
- **Thang giá phải có bậc người ta mua được.**
  - Yearly và lifetime đắt gấp 4–10 lần đối thủ thì 0 lượt bán.
  - Mẻ test chỉ còn 1 gói weekly có trial cho tới khi có giá mới.
- **Mất tiền vì thẻ lỗi sau trial ngang với số trả tiền thành công** (51 vs 46). Việc này nằm ngoài paywall: bật Billing Grace Period và làm banner billing retry trong app. Paywall vẫn không có màn "billing failed" (chốt 13/10/2026).
- **Dung lượng test.**
  - App nhỏ (~25 người thấy paywall onboarding/ngày) không chạy được 10 nhánh song song.
  - Mẻ 10 paywall là một hàng đợi: 2–3 nhánh mỗi đợt, mỗi đợt 3–4 tuần, mỗi paywall chỉ đổi một biến.

## 6. Bài học style (phản hồi của designer qua 4 vòng board)

| Vòng | Hướng | Kết quả |
|---|---|---|
| 22/10 | 10 concept tối, accent cam đoán | Chỉ giữ 2 (onboarding work-proof, chặn ở limit). Yêu cầu light |
| 22/10 v2 | Light theo design system thật, 1 gói, 2 biến thể mỗi cái | Chốt cỡ chữ app, CTA 56, trấn an xanh lá |
| 27/10 | "Vui": mascot, sticker, neo-brutal, viết tay, confetti | **"Hoạt hình quá"**: không dùng mascot/sticker/viết tay cho app công cụ |
| 27/10 | Before/after với slider | **Nhặt vào giỏ**. Thêm animation: slider tự trượt trái ↔ phải theo sin, chu kỳ 5s, biên 12–88%; chạm thì kéo tay, thả 2s sau chạy tiếp từ vị trí đang dừng; tắt khi `prefers-reduced-motion` |
| 28/10 | Chốt PW Limit 01 | Ra nhánh iOS ở mục 1–4 |

Rút ra:
- App công cụ (camera, scanner, tiện ích) nên "vui" bằng chuyển động và chất liệu thật: giờ chạy, ảnh hiện dần, trước/sau. Không dùng nhân vật hay hoạt hình.
- Ảnh thật của đúng người dùng mục tiêu (ở đây: người Thái đang làm việc) mạnh hơn illustration.
- Ref Mobbin cho app camera/ảnh nền sáng rất ít. Phải lấy từ app tiện ích, Apple-like, app ảnh in, rồi lọc bỏ kiểu mascot.

## 7. Quy trình cho app iOS

1. Kéo số trước: phễu thấy → bấm CTA → trial theo từng vị trí. Tách CTR và trial/bấm để biết chỗ rơi.
2. Đọc design system Figma (variables, text styles, màn paywall hiện tại) và ghi `docs/design-tokens.md`.
3. Moodboard light, đa dạng nguồn (Mobbin + ảnh thật) → designer chọn hướng style.
4. Board concept: tách theo vị trí (ví dụ 5 onboarding + 5 limit), mỗi paywall một biến test, frame 402×874 (thêm 375×667 để soát màn bé).
5. Có "giỏ" (`basket.md`) để giữ paywall được nhặt qua các vòng, kèm thông số animation để bàn giao.
6. Paywall được chốt mới làm đủ 8 nhóm state theo `SKILL.md`, với khung và legal theo file này.
