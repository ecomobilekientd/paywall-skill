# Nguồn tham chiếu paywall

Mục đích: trước khi đề xuất hướng (SKILL.md §5 bước 2), kéo 5–10 paywall cùng ngành / cùng mô hình giá để có điểm so sánh thật, không dựa trí nhớ của model. Mỗi tham chiếu ghi: app, ngành, kiểu selector (A/B/C/D/khác), gói + trial, điểm đáng học, link.

## 1. Nguồn sống (ưu tiên, tự cập nhật)

| Nguồn | Cách dùng | Ghi chú |
|---|---|---|
| **Mobbin MCP** (`search_screens`, `search_flows`) | `search_screens(platform: "ios", query: "paywall with weekly trial and lifetime plan")`, lọc theo app hoặc ngành; `search_flows` để xem cả luồng onboarding → paywall | Cần tài khoản Mobbin và kết nối MCP. Mỗi kết quả cite `mobbin_url`. Ảnh tải từ `image_url` nếu cần dán vào board. |
| **Thư mục `gallery/` trong repo này** | Mỗi paywall team đã làm hoặc sưu tầm = 1 ảnh + 1 dòng trong `gallery/index.md` | Đây là nguồn lớn dần theo team. Xem quy tắc đóng góp ở dưới. |
| **Web search có nguồn** | Khi Mobbin thiếu ngành: tìm "<app> paywall screenshot", "subscription paywall <ngành> 2026" | Chỉ dùng kết quả có ảnh và URL thật; không có thì ghi "chưa có dữ liệu". |

## 2. Thư viện công khai (kiểm tra link trước khi dùng, có thể cần trả phí)

- mobbin.com — lớn nhất, có filter "Paywall" trong screen patterns.
- screensdesign.com — video + ảnh toàn bộ onboarding và paywall app iOS.
- pageflows.com — luồng theo task, có "Upgrading".
- scrnshts.club — ảnh App Store screenshots, dùng cho hero/positioning.
- superwall.com và adapty.io — template paywall của các SDK, cho thấy pattern đang được A/B nhiều.
- revenuecat.com — báo cáo "State of Subscription Apps" hàng năm: benchmark trial-to-paid, giá, chu kỳ theo ngành.

## 3. Quy tắc đóng góp vào `gallery/`

1. Ảnh: PNG/JPG 360–390 dp wide, tên `yyyy-mm-dd_<app>_<kiểu>.png` (kiểu = A/B/C/D/other).
2. Thêm 1 dòng vào `gallery/index.md` theo đúng cột của bảng.
3. Ảnh từ app người khác chỉ là tham khảo nội bộ, không dùng asset của họ trong sản phẩm.
4. Mỗi quý: người giữ skill dọn ảnh trùng, cập nhật "điểm đáng học" nếu đã có số liệu A/B.

## 4. Cách skill dùng nguồn

- Bước 2 trong SKILL.md: gọi Mobbin (hoặc đọc `gallery/index.md`) với ngành + mô hình giá của app, chọn 5–10 tham chiếu, tóm tắt trong 1 bảng ngắn kèm link rồi mới đề xuất 2–3 hướng.
- Không chép layout nguyên; mỗi hướng đề xuất phải chỉ ra khác gì so với tham chiếu và vì sao.
- Khi board được chốt, thêm paywall mới vào `gallery/` để lần sau có tham chiếu của chính team.
