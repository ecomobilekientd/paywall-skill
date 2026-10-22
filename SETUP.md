# Cài đặt và kết nối công cụ

Làm lần lượt từ bước 1. Bước 1–2 là bắt buộc; bước 3–5 tuỳ việc bạn cần.

| Bước | Công cụ | Bắt buộc? | Dùng cho |
|---|---|---|---|
| 1 | Claude Code | Có | chạy skill |
| 2 | Skill paywall | Có | quy trình, state, copy, board HTML |
| 3 | Figma MCP | Khi cần đưa lên Figma | dựng frame sau khi chốt |
| 4 | Mobbin connector | Nên có | kéo paywall tham chiếu |
| 5 | Công cụ tạo ảnh | Tuỳ chọn | hero thật cho board |

## 1. Claude Code

Cài một trong hai:
- **Desktop app** (Mac/Windows): tải ở https://claude.ai/download, đăng nhập, mở tab **Code**.
- **CLI**: làm theo https://docs.claude.com/en/docs/claude-code/setup, rồi chạy `claude` trong terminal để đăng nhập.

Cần gói Claude Pro, Max, Team hoặc Enterprise.

## 2. Cài skill

```bash
git clone https://github.com/ecomobilekientd/paywall-skill.git
```

```bash
mkdir -p ~/.claude/skills && cp -r paywall-skill/paywall ~/.claude/skills/paywall
```

Kiểm tra: mở phiên Claude Code mới, gõ `/paywall`. Nếu Claude đọc `SKILL.md` là xong.

Cập nhật bản mới sau này:

```bash
cd paywall-skill && git pull && cp -r paywall ~/.claude/skills/
```

## 3. Figma MCP (để dựng lên Figma)

Chọn một cách:

**Cách A, desktop app hoặc claude.ai** (dễ nhất): Settings → Connectors → tìm **Figma** → Connect → đăng nhập Figma và cho phép. Connector này dùng chung cho Claude Code trên cùng tài khoản.

**Cách B, CLI, plugin chính thức** (Figma khuyến nghị):

```bash
claude plugin install figma@claude-plugins-official
```

**Cách C, CLI, thêm server thủ công:**

```bash
claude mcp add --transport http figma https://mcp.figma.com/mcp
```

Với cách B và C: mở `claude`, gõ `/mcp`, chọn **figma** → Authenticate. Browser mở trang đăng nhập Figma, bấm cho phép.

Kiểm tra: dán link một frame Figma và hỏi "đọc frame này". Claude trả được tên layer là đã kết nối.

Lưu ý:
- Ghi lên Figma cần quyền edit file đó và seat Figma có Dev hoặc Full.
- Skill chỉ dựng Figma **sau khi** board HTML được chốt. Không có Figma vẫn làm được toàn bộ phần board.

Tài liệu: https://developers.figma.com/docs/figma-mcp-server/remote-server-installation

## 4. Mobbin connector (nguồn tham chiếu)

Cần tài khoản Mobbin gói Pro, Team hoặc Enterprise.

1. Desktop app hoặc claude.ai: Settings → Connectors → tìm **Mobbin** → Connect.
2. Đăng nhập Mobbin và cho phép.
3. Nếu công ty dùng Claude Team / Enterprise: admin phải thêm Mobbin cho tổ chức trước.

Claude Code dùng chung connector với desktop app và claude.ai, nên không cần cài riêng cho CLI.

Kiểm tra: hỏi "tìm 5 paywall iOS có weekly trial trên Mobbin". Claude trả danh sách kèm link mobbin.com là đã kết nối. Gallery ảnh có thể không hiện trong tab Code, nhưng Claude vẫn đọc được kết quả.

Không có Mobbin: skill tự dùng `gallery/` của repo và web search có nguồn.

Tài liệu: https://docs.mobbin.com/mcp/clients/claude-desktop-web

## 5. Ảnh hero (tuỳ chọn)

Skill yêu cầu hero là ảnh thật, không khung xám. Chọn một:
- Một skill tạo ảnh bạn đã có trong Claude Code.
- Ảnh stock có licence thương mại, hoặc Lottie. Ghi licence vào docs của app.
- Không có ảnh: để `hero-sample.svg` trong lúc duyệt layout, thay trước khi giao.

## 6. Chạy thử

Mở phiên mới và gõ:

```
Làm paywall onboarding cho app ghi chú. Gói Weekly $4.99 có 3-day trial, Yearly $29.99, Lifetime $49.99. Thị trường chính Hàn Quốc.
```

Kết quả đúng:
1. Claude kéo tham chiếu (Mobbin hoặc gallery) và đề xuất 2–3 hướng.
2. Sau khi bạn chốt, ra một file HTML board có đủ 11 frame, dòng lint dưới mỗi frame là "✓".

## Lỗi hay gặp

| Triệu chứng | Cách sửa |
|---|---|
| `/paywall` không có | Kiểm tra đường dẫn `~/.claude/skills/paywall/SKILL.md`, mở phiên mới |
| Figma báo không có quyền | Mở `/mcp` → figma → Authenticate lại; kiểm tra quyền edit file |
| Mobbin không thấy trong Connectors | Admin tổ chức chưa bật; hoặc tài khoản Mobbin là gói Free |
| Hero vỡ khi mở board | Đường dẫn ảnh tương đối sai; để ảnh cùng thư mục với HTML hoặc dùng data URI |
