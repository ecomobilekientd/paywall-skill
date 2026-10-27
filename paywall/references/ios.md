# Paywall iOS — guideline (rút từ mẻ Tistora / Camera Timestamp, 10/2026)

Bổ sung cho `SKILL.md` khi app là iOS. Phần chung (8 nhóm state, 3 slot chống lặp, copy, lint) vẫn áp nguyên. File này chỉ ghi chỗ iOS khác Android, và bài học từ 4 vòng board của Tistora (22–27/10/2026).

## 1. Design system: theo app, không theo skill

- Dùng text style của app. App iOS thường theo cỡ chữ Apple (28 / 22 / 17 / 15 / 13 / 12, line height cố định). Luật "cỡ chữ chẵn" trong `SKILL.md` §6b là của board Android/FlyNote, **không áp** cho app iOS có design system riêng. Kai chốt 27/10/2026.
- Màu, spacing, radius lấy từ variable của file Figma app trước khi dựng. Đọc bằng Figma MCP, chỉ đọc. Không đoán màu từ ảnh chụp app. Vòng đầu Tistora đoán accent cam, nhưng thật ra là xanh iOS #007AFF.
- Nếu design system chỉ có một mode (thường là sáng) thì paywall dùng đúng mode đó. Không tự dựng dark mode khi chưa được yêu cầu.
- Font: SF Pro cho UI (fallback `-apple-system`), SF Mono cho số kỹ thuật hoặc stamp. Font trang trí mới (viết tay, display đậm) chỉ dùng trên board concept, ghi licence vào `docs/licenses.md` của app.

## 2. Thông số đã chốt (Tistora, dùng làm mặc định cho app iOS khác nếu chưa có quy định)

| Phần tử | Giá trị |
|---|---|
| CTA | pill, cao **56 ở mọi cỡ màn** (kể cả 360×640; màn bé thu hero, không thu CTA), màu brand của app, chữ 17 semibold |
| Dòng trấn an ("No payment due now", "No commitment") | **xanh lá toàn bộ, cả icon và chữ** (`state/success`). Lưu ý độ tương phản ~2:1 trên nền trắng |
| Nhãn trial (ribbon, badge) | màu phụ riêng (Tistora: cam #E8890C), không dùng cho CTA |
| Kiểu card gói | tuỳ layout, không ép thống nhất |
| Nút X | thấy ngay ở frame thiết kế (Remote Config có thể delay, ghi riêng) |
| Status bar | iOS (giờ trái, pin phải, Dynamic Island), không dùng status bar Android |
| Store trong Terms sheet | `{store}` = App Store |

## 3. Bài học dữ liệu (Tistora, 28/09–21/10/2026, ngoài VN)

- **CTR không phải nút thắt; sheet thanh toán Apple mới là chỗ rơi.**
  - 49% người thấy paywall bấm CTA, nhưng 84% người bấm huỷ ở sheet Apple (phần lớn `user_cancelled`).
  - Hệ quả: payrate (trial / người thấy) là thước đo chính, CTR chỉ là phụ.
  - Thiết kế phải làm cho sheet Apple không bất ngờ: giá, ngày trừ tiền và cách huỷ đã thấy trước khi bấm. Có thể dùng timeline trial, giá quy ra ngày, hoặc câu huỷ cụ thể.
- **Ngữ cảnh mạnh hơn giao diện.**
  - Cùng một gói, paywall chặn giữa việc (limit 8 ảnh) cho payrate 10.4%, onboarding chỉ 4.5%.
  - Paywall onboarding đặt trước màn chính làm tỉ lệ dùng tính năng lõi lần đầu tụt từ 70% xuống 50–61%.
- **Exit paywall chào cùng gói không thêm trial** (abt_7). Nếu làm exit thì chào bậc khác.
- **Thang giá phải có bậc người ta mua được.**
  - Yearly và lifetime đắt gấp 4–10 lần đối thủ thì 0 lượt bán.
  - Mẻ test chỉ còn 1 gói weekly có trial cho tới khi có giá mới.
- **Mất tiền vì thẻ lỗi sau trial ngang với số trả tiền thành công** (51 vs 46). Việc này nằm ngoài paywall: bật Billing Grace Period và làm banner billing retry trong app. Paywall vẫn không có màn "billing failed" (chốt 13/10/2026).
- **Dung lượng test.**
  - App nhỏ (~25 người thấy paywall onboarding/ngày) không chạy được 10 nhánh song song.
  - Mẻ 10 paywall là một hàng đợi: 2–3 nhánh mỗi đợt, mỗi đợt 3–4 tuần, mỗi paywall chỉ đổi một biến.

## 4. Bài học style (phản hồi của Kai qua 4 vòng board)

| Vòng | Hướng | Kết quả |
|---|---|---|
| 22/10 | 10 concept tối, accent cam đoán | Chỉ giữ 2 (onboarding work-proof, chặn ở limit). Yêu cầu light |
| 22/10 v2 | Light theo design system thật, 1 gói, 2 biến thể mỗi cái | Chốt cỡ chữ app, CTA 56, trấn an xanh lá |
| 27/10 | "Vui": mascot, sticker, neo-brutal, viết tay, confetti | **"Hoạt hình quá"**: không dùng mascot/sticker/viết tay cho app công cụ |
| 27/10 | Before/after với slider | **Nhặt vào giỏ**. Thêm animation: slider tự trượt trái ↔ phải theo sin, chu kỳ 5s, biên 12–88%; chạm thì kéo tay, thả 2s sau chạy tiếp từ vị trí đang dừng; tắt khi `prefers-reduced-motion` |
| 27/10 | Minimal kiểu Apple | Đang chờ phản hồi |

Rút ra:
- App công cụ (camera, scanner, tiện ích) nên "vui" bằng chuyển động và chất liệu thật: giờ chạy, ảnh hiện dần, trước/sau. Không dùng nhân vật hay hoạt hình.
- Ảnh thật của đúng người dùng mục tiêu (ở đây: người Thái đang làm việc) mạnh hơn illustration.
- Ref Mobbin cho app camera/ảnh nền sáng rất ít. Phải lấy từ app tiện ích, Apple-like, app ảnh in, rồi lọc bỏ kiểu mascot.

## 5. Quy trình cho app iOS

1. Kéo số trước: phễu thấy → bấm CTA → trial theo từng vị trí. Tách CTR và trial/bấm để biết chỗ rơi.
2. Đọc design system Figma (variables, text styles, màn paywall hiện tại) và ghi `docs/design-tokens.md`.
3. Moodboard light, đa dạng nguồn (Mobbin + ảnh thật) → Kai chọn hướng style.
4. Board concept: tách theo vị trí (ví dụ 5 onboarding + 5 limit), mỗi paywall một biến test, frame 360×800 + 360×640.
5. Có "giỏ" (`basket.md`) để giữ paywall được nhặt qua các vòng, kèm thông số animation để bàn giao.
6. Paywall được chốt mới làm đủ 8 nhóm state theo `SKILL.md`.
