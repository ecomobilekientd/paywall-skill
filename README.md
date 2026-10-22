# Paywall skill

Skill cho Claude Code (và các agent đọc được `SKILL.md`) để thiết kế màn paywall IAP / subscription cho app mobile: đủ bộ state, copy chuẩn, text pháp lý chuẩn, HTML board trước, Figma sau khi chốt.

## Cài

Copy thư mục `paywall/` vào thư mục skills:

```bash
git clone https://github.com/ecomobilekientd/paywall-skill.git
cp -r paywall-skill/paywall ~/.claude/skills/paywall        # dùng cho mọi project
# hoặc
cp -r paywall-skill/paywall <repo>/.claude/skills/paywall    # chỉ trong một repo
```

Mở Claude Code, nhắc tới "paywall" hoặc "pw" là skill tự kích hoạt. Gọi tay: `/paywall`.

## Có gì trong đây

```
paywall/
  SKILL.md                    quy trình, 8 nhóm state, 3 slot chống lặp, checklist, quy chuẩn Figma
  references/states.md        ma trận state + rule cho dev
  references/copy.md          kho chuỗi: CTA, deal line, tag, legal, bottom sheet Terms
  references/layouts.md       4 kiểu chọn gói A/B/C/D, hero, pricing block
  references/sources.md       nguồn tham chiếu paywall (Mobbin MCP, gallery, web) + cách dùng
  assets/board-template.html  engine render mọi state từ một CONFIG, có lint chống lặp
  assets/img/hero-sample.svg  hero mẫu, thay bằng ảnh thật
gallery/                      paywall team đã làm hoặc sưu tầm (ảnh + index.md)
```

## Thử ngay

Mở `paywall/assets/board-template.html` trong browser: bảng demo cho một app ghi chú giả định hiện đủ state. Sửa `CONFIG` ở đầu `<script>` cho app của bạn.

## Cần có

- Browser. Không cần server.
- Giá thật từ Play Console / App Store Connect / RevenueCat.
- Tuỳ chọn: Figma MCP để dựng lên Figma, Mobbin MCP để kéo tham chiếu, skill tạo ảnh cho hero.

## Đóng góp

- Sửa rule: PR vào `paywall/`. Nói rõ lý do và ngày chốt trong mô tả PR.
- Thêm tham chiếu: ảnh + 1 dòng vào `gallery/index.md` (xem `references/sources.md`).
