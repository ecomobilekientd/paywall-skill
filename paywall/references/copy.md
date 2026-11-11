# Kho copy paywall (tiếng Anh, nguồn chuẩn)

Placeholder: `{app}` (tên hiển thị trong app), `{store}` (`Google Play` / `App Store`), `{price}` (có ký hiệu tiền, ví dụ `$4.99`), `{period}` (`week` / `month` / `year`), `{days}` (mặc định 3), `{plan}` (`Weekly` / `Monthly` / `Yearly` / `Lifetime`), `{date}` (ngày bắt đầu tính tiền), `{equiv}` (giá quy đổi, ví dụ `$12/month`).

Nguyên tắc: từ đơn giản, câu ngắn, không "AI-sounding", không dấu chấm than. Sentence case cho CTA ("Try for free", không "TRY FOR FREE"). App tự dịch; chỉ giao tiếng Anh.

## CTA

| Tình huống | CTA | Biến thể đã dùng |
|---|---|---|
| gói đang chọn có trial, user đủ điều kiện | `Try for free` | `Try with $0.00` (chỉ khi card đã hiện `$0.00`) |
| mọi trường hợp còn lại (subs không trial, lifetime) | `Continue` | |
| billing đang load | nút xám + spinner, không chữ hoặc `Loading plans…` | |
| sau khi bấm | spinner + `Opening Google Play…` | |

## 3 slot và 6 fact (chống lặp)

| Fact | Label (card/tab/toggle) | Deal line | Legal |
|---|---|---|---|
| chu kỳ | `/week` sau giá | có | không |
| giá | `$4.99` | có | không |
| trial mấy ngày | nhãn ngắn `3-day free trial` (tuỳ chọn) | có | không |
| sau trial trả bao nhiêu | không | có | không |
| auto-renew | không | có | không |
| cancel anytime | không | có | không |
| one time (lifetime) | nhãn ngắn `One time purchased` (tuỳ chọn) | có | không |

Sai điển hình (AI dựng, trước khi sửa): toggle `3-day free trial` + card `3-day free trial, then $3.99/week` + card `Renews automatically. Cancel anytime.` + dưới CTA `Auto-renew, cancel anytime` + đoạn legal nhắc lại trial và renew. Sửa đúng: toggle `3-day free trial` · card `Weekly · $3.99/week` · deal line `3-day free trial, then $3.99/week. Auto-renew. Cancel anytime.` · legal 1 dòng.

## Deal line (ngay trên CTA, chỉ có 1 dòng này, chữ 11–12, căn giữa)

Nội dung giống nhau, chỉ khác độ dài. Chọn bản theo chỗ còn trống: ở 360 dp, chữ 11.5, một dòng chứa ~60 ký tự. Ưu tiên bản vừa 1 dòng; khối giá còn cao thì được 2 dòng, dòng 2 luôn là "Auto-renew. Cancel anytime." Template tự chọn theo `CONFIG.dealMaxChars` (mặc định 64 ký tự cho 1 dòng; đặt `dealLines: 2` để dùng bản dài).

| Tình huống | Ngắn (≤ 60, 1 dòng) | Vừa (mặc định) | Dài (2 dòng cố định) |
|---|---|---|---|
| trial, subs | `{days} days free, then {price}/{period}. Auto-renew, cancel anytime.` | `{days}-day free trial, then {price}/{period}. Auto-renew. Cancel anytime.` | `{price}/{period} after {days}-day free trial ends.` ⏎ `Auto-renew. Cancel anytime.` |
| không trial, subs | `{price}/{period}. Auto-renew, cancel anytime.` | `{price}/{period}. Auto-renew. Cancel anytime.` | `{price}/{period}, billed today.` ⏎ `Auto-renew. Cancel anytime.` |
| lifetime | `{price} once. No renewals.` | `Billed {price}. One time payment.` | `One time purchased with {price}.` ⏎ `No subscription, no renewals.` |

Khi nào dùng bản nào: layout D một card, màn 640, ngôn ngữ dài (DE, PT, ID) → ngắn. Layout A/B/C ở 800 → vừa. Khối giá còn trống và muốn giá đứng đầu câu → dài.

Các dòng khác: loading = trống hoặc skeleton 1 dòng; huỷ giữa chừng (toast) = `Purchase not completed. You have not been charged.`; thành công = `Premium is on` / `Thank you. Enjoy {app} Premium.`

## Card gói

| Phần | Chuỗi |
|---|---|
| tên | `Weekly` `Monthly` `Yearly` `Lifetime` (hoặc `Weekly Plan`… nếu layout cần dài) |
| giá | `{price}` + `/{period}` nhỏ hơn. Lifetime: chỉ `{price}` |
| sub-line có trial | `{days}-day free trial` / `{days}-day free trial enabled` / `First {days} days free` |
| sub-line không trial | `Auto renew, cancel anytime` / `Cancel anytime` / `Only {equiv}` (yearly) / `{equiv}` làm giá neo cho monthly (`$218.16/year`) |
| sub-line lifetime | `One time purchased` |
| sub-line kiểu D không trial | `Cancel anytime` bên trái, `charged today` dưới giá |
| kiểu D có trial | trái: `{days}-day free trial` + `then {price}/{period}`; phải: `$0.00` + `for the next {days} days`; badge `NO PAYMENT NOW` |

Tag góc card (tối đa 1): `Most popular` · `Most value` · `Best value` · `75% OFF` · `Saved xx%` · `MOST FAVORITE` (chỉ khi theme chữ hoa) · `NO PAYMENT NOW`.

Toggle (kiểu C): on = `Free trial enabled`; off = `Not sure yet? Enable free trial`; không đủ điều kiện = ẩn toggle, hoặc `Free trial unavailable`.

Mở rộng danh sách (kiểu B có >2 gói): `Show more plan` / `Show less plan`.

## Headline (để lấy giọng, không copy nguyên)

- `Take your <notes> further.` · `Take your <work> to the next level`
- `Get unlimited access now` + sub nêu một con số thật của app
- `Turn your <idea> into real <progress>`
- `Use the <app>, not the <ads>.` (hero chữ tay: "I just want to use the app, not watch ads.")
- `Clean space to work, without <distractions>`
- `Welcome to the club.` (hero VIP card)
- `<Shop smarter> every day.` (từ nhấn xoay: Scan anything / Shop healthier / Stay safe)
- `Join 5M+ happy users`

Quy tắc: 1 từ hoặc cụm nhấn màu accent (`<...>`), tối đa 2 dòng, nói lợi ích chứ không nói "Premium".

## Trust

- `Trusted by 30M+ users` · `Over 9 years of growth` · `5B+ notes created`
- `4.6/5` + `54.3K+ reviews` + `Trusted by 5M+ users worldwide` (laurel 2 bên)
- Review card: 5 sao, 1–2 câu, tên người. 3–4 card cuộn ngang hoặc 2 cột.
- Bảng Free / Pro: 5–7 dòng, cột Free dùng ✕ hoặc khoá, cột Pro dùng ✓.
- Timeline trial: `Today` → `Unlock full access to all premium features`; `1 day before trial ends` → `We'll send you a reminder so you can decide`; `After {days} days` → `You'll be charged on {date}. Cancel anytime before then.`

Số liệu lấy từ app thật (Play Console, App Store Connect, review thật). Không bịa.

## Benefits (mẫu)

App ghi chú: `Unlimited notes, checklists & folders` · `AI-powered voice notes` · `Enjoy an ad-free experience` · `PDF export for easy sharing` · `Unlock all premium themes & backgrounds`
App quét mã: `AI Scan · Know anything` · `Safe links · Every scan` · `No ads · Ever again`

## Dòng pháp lý (Legal, đúng 1 dòng)

`By continuing you agree to our **Terms & Privacy Policy**` (phần đậm bấm được, mở bottom sheet) — hoặc link riêng `Our Terms & Privacy Policy`. Có mặt ở mọi state, kể cả loading. Không ghép thêm câu nào về giá, trial, renew vào dòng này.

Không đặt `Restore purchase` ở paywall; restore nằm ở Settings.

## Bottom sheet Terms & Privacy (chuẩn cho mọi app)

Header: 2 tab `Terms` | `Privacy policies.` + nút X. Tab đang chọn gạch chân. Body tab Terms (chuẩn, thay `{app}` và `{store}`; tên app lấy từ tên hiển thị trong app):

```
Subscription gives you access to {app}'s pro features, all ads will be removed.

Subscription plans are automatically renewed and can be canceled anytime in Subscriptions on {store}.

You will not be charged if subscription is canceled before trial period ends.

Non-subscribed users can still use the application, however, some paid features may be limited.

Subscription is not required to use {app}.
```

Bản dài (dùng khi team legal muốn nêu mốc 24 giờ; vẫn chỉ nằm trong sheet):

```
Payment will be charged to your {store} account upon confirmation of purchase or at the end of the trial period (if applicable).
During the trial period, you won't be charged any fees. After the trial ends, the plan will automatically renew at the original price.
Subscription will automatically renew according to the subscription package you choose unless canceled 24 hours before the end of the current period or trial period (if you have a trial).
Non-subscribed users can still use the application, however, some paid features may be limited.
You can manage or cancel your subscription at any time in the Subscriptions in {store}.
```

Tab Privacy policies: link / nội dung policy của app.

## Localisation (bản nháp, chưa native review)

Template có sẵn một bộ KR trong `CONFIG.locale`. Mẫu thêm:

| | KR | JA |
|---|---|---|
| CTA trial | `무료로 시작하기` | `無料で試す` |
| CTA no-trial | `계속하기` | `続ける` |
| Deal line trial | `3일 무료 체험 후 월 $36.36. 자동 갱신, 언제든 취소 가능` | `3日間の無料トライアル終了後、自動的に¥2,400/月で更新されます。いつでも解約できます。` |
| Legal | `계속하면 이용 약관 및 개인정보처리방침에 동의하는 것으로 간주됩니다.` | `続行すると、利用規約とプライバシーポリシーに同意したことになります。` |

Luôn test DE/PT (chuỗi dài) và KR/JA (CJK) trước khi chốt layout.

## Ngắt dòng tiếng Nhật (JA) — chốt 04/11/2026

Tiếng Nhật không có dấu cách nên trình duyệt và app được phép ngắt giữa bất kỳ hai ký tự nào. Ngắt sai chỗ làm câu đọc sai nghĩa hoặc trông lỗi (ví dụ `無制` / `限`, `¥2,4` / `00/月`). Luật:

1. **Chỉ ngắt giữa các cụm (文節).** Mỗi dòng phải đọc được thành một cụm trọn nghĩa. Trợ từ đi liền với từ đứng trước: `メモを` / `無制限に`, không bao giờ để `を`, `に`, `で`, `は`, `が`, `の`, `も`, `と`, `へ` đứng đầu dòng.
2. **Không cắt các khối sau:** số + đơn vị (`3日間`, `¥2,400/月`), từ katakana (`トライアル`, `チェックリスト`), từ ghép kanji (`無制限`, `自動更新`, `広告なし`), tên app và tên gói (`{app} Pro`, `年間プラン`).
3. **Kinsoku:** không dòng nào bắt đầu bằng `、。」』）ー` hay kana nhỏ (`ゃゅょっァィ`), không dòng nào kết thúc bằng `「『（`.
4. **Headline ngắt tay bằng `\n`, ngay sau `。` hoặc `、`**, mỗi dòng một ý. Mẫu tốt: `頭の外へ。\nそして完了。`. Headline tiếng Nhật tối đa 2 dòng ở 360px; dài hơn thì rút copy, không giảm cỡ chữ.
5. **Không để mồ côi:** dòng cuối không được chỉ còn 1–2 ký tự (`。`, `す。`). Nếu bị thì chuyển điểm ngắt hoặc rút câu.
6. **Câu dài (subtitle, deal line, legal) để tự ngắt** nhưng board phải bật ngắt theo cụm: `.pw` có `lang="ja"` (đặt `CONFIG.locale.lang = 'ja'`), template đã có CSS `word-break: auto-phrase; line-break: strict`. Khối nào vẫn bị cắt thì bọc `<span class="nb">…</span>` (nowrap) quanh khối đó, hoặc thêm `<wbr>` ở điểm được phép ngắt.
7. **Soát trước khi giao:** xem frame JA ở 360px và ở state small, đọc từng dòng theo luật 1–5. Bản dịch chưa có người Nhật duyệt thì ghi rõ "chưa native review" trên board.
