# BAM — danh sách tính năng (khảo sát code 3/11, #1041)

Nguồn: đọc đầu từng module `hub/`, `mcp/`, `hooks/`, `ui/v2/`, `native/`, `scripts/`, `skills/`, rồi đối chiếu với `ui/v2/nav.js`, `ui/v2/core.js:21` và route trong `hub/server.ts`.
Cột "file:dòng" là dòng khai báo chính của module (dòng `export` đầu tiên hoặc dòng chú thích của việc); tên số `#NNN` là việc đã làm nó.
Cột "Màn hình" là chỗ chụp ảnh. Cây điều hướng trái (`ui/v2/nav.js:3-15`): **Phòng** (mỗi phòng có 7 tab: Chat · Việc · Luồng · Văn phòng 3D · Ghi chú · Thành viên · Console, `ui/v2/core.js:21`), **Chat riêng**, **Usage Account** (Mức dùng · Log), **Hệ thống** (Bear HUB · Addon · Xoá cache · nút nguồn), **Agent** (Thư mục · KPI · Worker · Skill), **Extra** (Office · Zalo Assistant · Code Bot), **Cài đặt** (8 nhóm, `ui/v2/settings.js:115`).
Chưa kiểm bằng cách chạy app: ảnh nào cần chụp thì đối chiếu lại với bản đã cài.

## 1. Phòng, topic, PM, tin nhắn

| Tính năng | Tác dụng | Màn hình | file:dòng |
|---|---|---|---|
| Phòng chung | Nhiều agent (Claude, Copilot, API) trò chuyện trong một phòng, kể cả khác repo, editor | Phòng › Chat | `hub/store.ts:8`, `hub/server.ts:7936` (`/api/rooms`) |
| Topic | Chủ đề riêng trong phòng; agent chỉ đọc topic mình đang theo | Phòng › Chat (dải topic) | `ui/v2/topics.js:1`, `ui/v2/topicload.js:1`, `mcp/tools.ts:25` (`room_topics`) |
| PM giữa hai agent | Phòng riêng chỉ hai agent, người dùng đọc được nhưng không viết | Phòng pm-… trong nav | `hub/server.ts:7369` (`/api/pm`), `mcp/tools.ts:22` |
| Chat riêng (DM) 1-1 | Nhắn riêng một agent; mọi agent đều có, kể cả chưa từng viết | Chat riêng | `ui/v2/dm.js:1`, `ui/v2/dm-live.js:1`, `hub/mirror.ts:1` |
| Quick chat ⚡ | Bảng nổi gọi một agent trợ lý một lần bấm, kéo thả được, thu thành bong bóng | Nổi trên mọi màn | `ui/v2/qchat.js:1` |
| Cửa sổ nổi / popup phòng | Mở phòng và chat riêng thành cửa sổ riêng, thu thành bubble | Bất kỳ phòng | `ui/v2/windows.js:2`, `ui/v2/dockdrag.js:1` |
| Gọi dậy bằng @tên | Chat rảnh tự dậy khi có người gõ `@tên` (hook Stop `asyncRewake`); chat bận nhận tin giữa hai lượt công cụ | Mọi phòng | `hooks/hooks.json:29`, `hooks/waiter.ts:1`, `hub/wake.ts:5` |
| Tin có hành động | Trả lời, copy, xuất ảnh, in, PDF; xem trước link trong app | Tin nhắn › menu | `ui/v2/msgpop.js:1`, `ui/v2/chat-render.js:1`, `hub/linkpeek.ts:20` |
| Thẻ hỏi chọn nhanh | Agent hỏi người dùng một câu trắc nghiệm, bấm trả lời một lần, cũng qua Telegram/Zalo | Chat phòng, chat riêng, popup | `hub/ask.ts:23`, `hub/routing.ts:29`, `hub/telegram-ask.ts:16`, `mcp/tools.ts:17` |
| Tệp đính kèm | Gửi file vào phòng, lưu theo hash, an toàn đường dẫn, có thùng rác | Ô nhập | `hub/attachments.ts:10`, `ui/v2/attach.js:1` |
| Gợi ý @, soạn nháp | Gõ @ chọn agent; nháp giữ khi đổi phòng | Ô nhập | `ui/v2/mention.js:1`, `ui/v2/drafts.js:1`, `ui/v2/composer.js:1` |
| Tìm kiếm | Tìm trong tin của phòng, danh sách ngày; Ctrl+K nhảy nhanh màn/phòng/việc | Thanh tìm, palette | `hub/roomsearch.ts:13`, `hub/server.ts:7356`, `ui/v2/palette.js:1`, `ui/v2/search.js:1` |
| Mốc xem file .md của phòng | Gom file .md đính kèm hoặc đường dẫn agent trích | Phòng › Ghi chú / chat | `hub/mdfiles.ts:10`, `hub/pathview.ts:15` |
| Thông báo hệ thống | Tin hệ thống một nơi, xoá hàng loạt | Phòng › Chat, nút ↻ ở "Phòng" | `hub/server.ts:9483`, `ui/v2/sysmsg.js:1` |
| Chế độ thông báo phòng | Mọi tin / chỉ khi @you / tắt | Menu phòng | `hub/store.ts:68` |
| Đổi tên, xoá, rời phòng | Menu chuột phải của app | Menu phòng | `ui/v2/ctxmenu.js:1`, `ui/v2/roomops.js:1`, `mcp/tools.ts:35` (`room_set_name`) |
| Lưu trữ / chuyển chat phòng | Sao chép dữ liệu phòng sang thư mục lưu; chuyển chat | Cài đặt › Hệ thống | `hub/archive.ts:13`, `hub/archive-worker.ts:4`, `hub/roomchat.ts:30`, `scripts/move-room-chats.mjs` |

## 2. Việc, ghi chú, trưởng phòng, Luồng

| Tính năng | Tác dụng | Màn hình | file:dòng |
|---|---|---|---|
| Bảng Việc | Giao việc, đặt phụ thuộc (`after`), theo dõi trạng thái và commit; việc xong mở việc kế tiếp, đánh thức người nhận | Phòng › Việc | `hub/tasks.ts:7`, `ui/v2/tasks.js:1`, `mcp/tools.ts:26` |
| Việc quá hạn, giám sát | Gắn nhãn việc doing kẹt quá `staleMin`, nhắc/giao lại | Cài đặt › Phòng & điều phối, tab Việc | `hub/leadsmart.ts:4` |
| KPI mỗi agent | Số việc, thời gian chờ/làm, tỉ lệ commit, việc kẹt | Agent › KPI, tab Việc | `ui/v2/agents.js:73` |
| Ghi chú phòng | Quyết định lâu dài ghi một lần, mọi agent trong phòng đọc | Phòng › Ghi chú | `mcp/tools.ts:36-38` (`room_note*`) |
| Kế hoạch (plan) | Planner lưu kế hoạch cho một phòng, bước theo việc | Phòng › Việc | `hub/plans.ts:11`, `mcp/tools.ts:27` |
| Trưởng phòng ★ | Một chat điều phối: nhận tin không gắn tên của bạn, tạo phòng, thêm agent (sau thẻ hỏi), xem bảng nhân sự | Phòng › Thành viên | `hub/lead.ts:11`, `hub/leadsmart.ts:4`, `mcp/tools.ts:52-66` (`room_create`, `room_team`, `room_agent_add`) |
| Luồng (Flow) | Agent theo tầng (2–5), dòng tin thời gian thực, luật mỗi vai trò/tầng, mẫu luật | Phòng › Luồng | `ui/v2/room-views.js:1` (`flowSVG`), `hub/flowrules.ts:20`, `mcp/tools.ts:40` (`room_flow`) |
| Chỉ mục thẻ ngành + skill | Đặt tên loại việc của phòng, gắn thẻ cho agent, xem skill của thẻ | Phòng › Luồng, Agent › Skill | `hub/tagoverlay.ts:18`, `hub/server.ts:6003` (`/api/tags`) |
| Bài học (lessons) | Sau khi xong việc, agent ghi bài học; trưởng phòng duyệt thành ghi chú hoặc bộ nhớ agent | Phòng › Việc, thẻ duyệt | `hub/lessons.ts:10`, `hub/lessons-routes.ts:6`, `mcp/tools.ts:84` |
| Nháp skill từ bài học | 3+ bài học cùng chủ đề → đề xuất viết SKILL.md | Thẻ duyệt | `hub/skilldrafts.ts:15`, `mcp/tools.ts:83` |
| Lỗi lặp có đếm | Gom lỗi theo chữ ký, kèm cách sửa, trích vào việc sau | Bảng Việc | `hub/errors.ts:6` |
| Văn phòng 3D của phòng | Thấy agent ngồi bàn, phòng họp, phòng nghỉ khi hết hạn mức | Phòng › Văn phòng 3D | `ui/v2/office3d.js:1`, `ui/office4/main.js` |
| Console của phiên | Xem lệnh/công cụ agent chạy và kết quả (chỉ đọc) | Phòng › Console | `hub/console.ts:13`, `ui/v2/console.js:1` |

## 3. Agent: chạy nền, nhiều backend, worker

| Tính năng | Tác dụng | Màn hình | file:dòng |
|---|---|---|---|
| Chạy nền không cần editor | Hub nối lại phiên Claude đã lưu, không mở hai bên cùng ghi một chat | Chat riêng, thanh trạng thái | `hub/revive.ts:21`, `hub/agents.ts:18`, `hub/store.ts:8` |
| Thêm agent không cần IDE | Tạo agent mới chỉ qua hub (D3) | Phòng › Thành viên | `hub/agents.ts:8`, `mcp/tools.ts:66` |
| Mở trong IDE | Mở tab VS Code / Antigravity của agent, chẩn đoán, diff | Menu agent | `hub/ide.ts:6`, `hub/ideaccount.ts:15`, `hub/ideinstalled.ts:16`, `mcp/tools.ts:86-89` |
| Copilot | Agent GitHub Copilot SDK, hồ sơ Copilot, chuyển giao khi lỗi | Cài đặt › Tài khoản & nhà cung cấp | `hub/copilot.ts:25`, `docs/copilot-profiles-and-dm.md` |
| Agent API tương thích Anthropic | Hồ sơ `claude-api`, provider/model/quyền theo từng agent, giá theo 1M token | Cài đặt › Tài khoản & nhà cung cấp | `hub/apiagent.ts:13`, `hub/providers.ts:37`, `hub/paidusage.ts:23` |
| Antigravity (agy) | `agy` làm agent: một lượt mỗi lần gọi, đếm lượt/token, tự dừng khi hết gói | Usage, thẻ agy | `hub/agy.ts:16`, `hub/usage-providers.ts:1`, `hub/server.ts:5219` |
| Worker / osin | Việc đọc nặng hoặc dọn dẹp giao cho model rẻ (Ollama LAN hoặc API miễn phí) | Agent › Worker | `hub/workers.ts:10`, `skills/worker/SKILL.md`, `skills/osin/SKILL.md`, `hub/autojobs.ts:10` |
| Duck / Muse | Thư ký/hỏi đáp: đọc, phân loại tin; Muse hỏi đáp qua Chrome | Phòng › Thành viên | `hub/duck.ts:7`, `hub/muse-member.ts:7`, `mcp/muse.ts:8` |
| Quyền theo agent | Duyệt hoặc từ chối yêu cầu quyền, danh sách cho phép/cấm | Chat riêng, Cài đặt › Agent | `hub/permissions.ts:11`, `mcp/perm.ts:3`, `hub/server.ts:7314` |
| Thư mục & tệp agent | Cây file, xem file, diff, sửa và commit đúng đường dẫn, không ra khỏi thư mục agent | Agent › Thư mục, chat riêng | `hub/files.ts:8`, `hub/filewrite.ts:10`, `hub/gitstate.ts:1`, `ui/v2/files.js:1` |
| Khối lượng chat theo thư mục | Gom chat theo thư mục làm việc | Agent › Thư mục | `hub/workspaces.ts:47` |
| Tự compact | Mỗi 10 phút nén chat offline vượt ngưỡng của vai trò, và khi việc xong+commit | Cài đặt › Bộ nhớ & compact | `hub/compact.ts:12`, `hub/gc.ts:78` |
| Bộ nhớ agent | Ghi nhớ riêng, có lọc bí mật trước khi ghi | Cài đặt › Bộ nhớ & compact | `hub/memory.ts:34`, `hub/memguard.ts:1` |
| bam-boss | Agent hệ thống do hub tự bật khi có tài khoản Claude, nhận tin từ Telegram | Chat riêng (bam-boss) | `hub/boss.ts:5`, `hub/server.ts:51` |
| bam-boss cứu hộ | Tin @you không được trả lời sau 3 phút → hub gửi Telegram 1 dòng + nút /switch, /wake | Telegram | `hub/bossrescue.ts:20` (có ở HEAD; bản làm việc đang xoá) |
| Win MCP có khoá | Điều khiển chuột/phím thật, xếp hàng xin phép | — | `hub/winmcp.ts:7`, `scripts/winmcp-lock.mjs` |
| Trình duyệt cho agent (bam-browser) | Playwright MCP đã ghim sẵn trong app, Chrome đăng nhập riêng | Cài đặt › Công cụ | `hub/browser.ts:13`, `hub/server.ts:5229` |

## 4. Nhiều tài khoản Claude, kho chat

| Tính năng | Tác dụng | Màn hình | file:dòng |
|---|---|---|---|
| Danh sách tài khoản | Các thư mục `.claude*` là tài khoản ngang hàng, thêm/bớt, thêm bằng đăng nhập trình duyệt | Usage Account, Cài đặt › Tài khoản | `hub/accountreg.ts:16`, `hub/accounts.ts:34`, `ui/v2/acct-cards.js:1` |
| Đổi tài khoản khi hết hạn mức | Lượt đánh thức nền chuyển sang tài khoản còn hạn mức; /move; switch-all | Usage Account | `hub/accountswitch.ts:20` |
| Kho chat BAM + hardlink | Chat nằm trong kho của BAM, mỗi tài khoản có `projects/` thật chứa hardlink; không junction, không UAC | Ẩn (nền) | `hub/chatstore.ts:62` (#1025) |
| Mức dùng Claude | Hạn mức 5 giờ và tuần, đọc từ credentials, chỉ đọc | Usage Account › Mức dùng | `hub/usage.ts:9`, `hub/server.ts:7480`, `ui/v2/usage.js:1` |
| Token và chạy lại | Token worker, lịch sử chạy | Usage Account › Log | `hub/tokens.ts:4`, `hub/log.ts:1`, `ui/v2/usage-clock.js:1` |
| Cài Claude CLI | Phát hiện/cài CLI chính thức, tìm bản mới nhất trong các editor | Cài đặt › Tài khoản | `hub/claudeinstall.ts:19`, `hub/claudebin.ts:15` |
| Plugin tự cập nhật | Bản plugin BAM trong mọi tài khoản tự cập nhật, không nút | Nền | `hub/pluginupdate.ts:11`, `hub/plugins.ts:9` |
| Hook có mã riêng | Hook mang token cài riêng (`x-bam-hook`) | Nền | `hub/hooktokenenv.ts:13`, `hooks/hooks.json` |

## 5. Kênh điện thoại: Telegram, Zalo

| Tính năng | Tác dụng | Màn hình | file:dòng |
|---|---|---|---|
| Telegram: chat riêng với bot | Bot chat riêng = bam-boss; lệnh /start /list /switch /status /tasks /freeze /unfreeze /wake /help | Extra › Code Bot; Cài đặt › Zalo & Telegram | `hub/telegram.ts:67`, `hub/telegram.ts:81-91`, `hub/telegram-panel.ts:41` |
| Telegram: nhóm có topic | Mỗi phòng một topic; topic "Hệ thống" nhận thông báo (hạn mức, wake, lỗi, restart); /rooms /topic /bam_id | Telegram | `hub/telegram-out.ts:33`, `hub/telegram.ts:92-95`, `hub/channel.ts:7` |
| Telegram: thẻ hỏi và tệp | Nút bấm trả lời, gửi file an toàn, định dạng code/link | Telegram | `hub/telegram-ask.ts:16`, `hub/telegram-file.ts:10`, `hub/telegram-fmt.ts:13`, `mcp/tools.ts:81` |
| Telegram: /list, /switch | Xem hạn mức tài khoản, đổi toàn bộ agent sang tài khoản khác | Telegram | `hub/telegram.ts:31,396-397` |
| Zalo Code | Người gửi Zalo được phép nói với phòng, tin ra Zalo khi @you/done, tin gộp tránh spam | Extra › Code Bot, Cài đặt › Zalo | `hub/zalo.ts:28`, `hub/zalo-out.ts:29`, `hub/zalo-cmd.ts:26`, `mcp/tools.ts:79-80` |
| Zalo Assistant | Tài khoản Zalo thứ hai chỉ đọc: lưu tin cục bộ, tóm tắt cuối ngày, nháp việc cần theo dõi chờ duyệt | Extra › Zalo Assistant | `hub/zalo-assistant.ts:29`, `hub/zalo-summary.ts:21`, `hub/zalo-office.ts:26`, `ui/v2/zalochat.js:1` |
| Zalo: đăng nhập và cài openzca | Quét QR, cài/nhận CLI `openzca`, dịch vụ nền | Cài đặt › Zalo | `hub/zalo-auth.ts:39`, `hub/zalo-service.ts:64`, `hub/zalo-data.ts:31` |

### 5a. Telegram chi tiết (chỉ ghi cái có thật trong code)

Chỉ chủ máy (`allowFrom[0]`) được bot trả lời; người lạ bị đếm và bỏ qua, không phản hồi (`hub/telegram.ts:308-328,405-409`). Ghép nối bằng `/start <4 số>` từ cửa sổ thiết lập (`hub/telegram.ts:318-327`, mã sống 10 phút, 5 lần đoán).

**Chat riêng với bot (= bam-boss / agent nhận chat riêng)** — `hub/telegram.ts:392-403`

| Lệnh / nút | Tác dụng | file:dòng |
|---|---|---|
| `/start` | Hiện bảng nút điều khiển | `hub/telegram-panel.ts:68`, `hub/telegram-panel.ts:46-55` |
| `/status` | Agent bận/rảnh/tắt, hạn mức tài khoản đang ghim, phòng đóng băng | `hub/telegram-panel.ts:70` |
| `/tasks [phòng]` | Việc đang làm + bị chặn của một phòng (bỏ trống = phòng mặc định) | `hub/telegram-panel.ts:71` |
| `/freeze <phòng>`, `/unfreeze <phòng>` | Đóng/mở băng phòng; luôn hiện thẻ hỏi lại (nút Đóng băng/Mở băng/Huỷ) | `hub/telegram-panel.ts:72-77,87-97` |
| `/wake <tên agent>` | Gọi dậy agent đang tắt hoặc dừng vì hạn mức; hỏi lại vì tốn hạn mức, đi qua cả phòng đóng băng | `hub/telegram-panel.ts:78-84,99-104` |
| `/list` | Danh sách tài khoản Claude (tên che, mã ngắn, hạn mức 5 giờ và tuần) | `hub/telegram.ts:31,396,497` |
| `/switch` | Cùng danh sách, mỗi tài khoản một nút; bấm để đổi toàn bộ agent sang tài khoản đó | `hub/telegram.ts:397,497` |
| `/help` | Giải thích các nút và lệnh | `hub/telegram-panel.ts:28-39,69` |
| Nút 🔄 Khởi động lại hub | Hỏi lại; nếu có agent chạy nền thì liệt kê và mặc định Huỷ; bot báo "✅ Hub đã lên lại" khi xong | `hub/telegram-panel.ts:155-162,192-200`, `hub/telegram.ts:218` |
| Mọi chữ khác | Chuyển nguyên văn cho agent nhận chat riêng (quickchat); chưa có agent thì bot nhắc tạo | `hub/telegram.ts:399-402` |

Danh sách lệnh đăng ký với Telegram (menu `/`): `OWNER_COMMANDS` `hub/telegram.ts:81-91`; trong nhóm: `GROUP_COMMANDS` `hub/telegram.ts:92-97`.

**bam-boss giao việc cho agent** — bam-boss là agent tổng, tự bật khi đã đăng nhập tài khoản Claude (`hub/boss.ts:23-41`, model Opus `hub/boss.ts:6`). Lời nhắc hệ thống: đọc được mọi phòng, có quyền lead ở mọi phòng (giao việc, duyệt, theo dõi), không nằm trong phòng nào, chat riêng với chủ qua BAM và Telegram (`shared/i18n/vi.ts:1507`). Chủ nhắn việc ở chat riêng → bam-boss dùng các tool phòng (`room_tasks add`, `room_pm`…, `mcp/tools.ts`) giao cho agent; kết quả quay lại cùng chat. Chưa kiểm bằng chạy thật: luồng này suy ra từ lời nhắc và tool, không có lệnh `/giao` riêng.

**Nhóm có Topics (làm việc theo phòng)** — `hub/telegram.ts:329-377`

| Thao tác | Tác dụng | file:dòng |
|---|---|---|
| `/bam_id` (trong nhóm) | Trả ID nhóm + topic; supergroup thì tự lưu vào Cài đặt › Telegram | `hub/telegram.ts:331-336` |
| `/rooms` hoặc `#` | Liệt kê phòng | `hub/telegram.ts:351` |
| `/topic <phòng>` | Tạo topic cho phòng (bot cần là admin quản lý topic) | `hub/telegram.ts:352-359` |
| Gõ trong topic của phòng | Tin vào đúng phòng đó, kèm ảnh/tệp | `hub/telegram.ts:303-306,377,426-433` |
| `#phòng nội dung` (ngoài topic) | Chọn phòng gửi; `#phòng` một mình = chuyển sang phòng đó; `#?` = đang ở phòng nào | `hub/telegram.ts:365-376` |
| `#<số> 1,2` | Trả lời thẻ hỏi số `<số>` bằng gõ chữ | `hub/telegram.ts:366-367` |
| Topic "Hệ thống" | Nhận hạn mức, wake, lỗi, restart; topic mồ côi (phòng đã xoá) được báo, tin không rơi vào phòng mặc định | `hub/telegram.ts:281,361-362`, `hub/telegram-out.ts:33` |
| Tin đi ra topic | Lời chủ, câu hỏi, câu trả lời cho chủ, dòng @you, "✅ Việc #N xong"/"❌ lỗi"; chat của agent với nhau ở lại trong BAM (#938) | `hub/telegram-out.ts:20,39,118` |

**Duyệt ✅❌ và xác nhận**
- Thẻ hỏi trắc nghiệm của agent hiện thành nút bấm; bấm xong tin được sửa thành "✅ đã chọn" (`hub/telegram-ask.ts:16-72`, `hub/telegram.ts:620`, `hub/telegram-out.ts:413`).
- Lệnh nguy hiểm gửi từ Telegram (push, release, tag, xoá, rm -rf, chuyển tiền, gửi e-mail, mời họp…) không chạy ngay: bot yêu cầu trả lời `OK <4 số>` trong 10 phút, sai 3 lần thì huỷ (`hub/zalo.ts:154-157`, `hub/telegram.ts:347,380-424`).
- Việc xong/lỗi tự gửi lên topic với ✅/❌ (`hub/telegram-out.ts:39`).

**Gửi tệp và ảnh**
- Chủ gửi ảnh/tệp lên bot hoặc topic: BAM tải về làm tệp đính kèm cho phòng, tối đa 20 MB (giới hạn getFile của Bot API) (`hub/telegram.ts:26,426-433`).
- Agent gửi tệp về điện thoại bằng tool `telegram_send_file` (kiểm đường dẫn an toàn) (`hub/telegram-file.ts:11`, `mcp/tools.ts:81`).
- Tin dài/code/link được định dạng lại cho Telegram (`hub/telegram-fmt.ts:13`).

**Chạy agent từ điện thoại**: nhắn trong topic của phòng → agent trong phòng được đánh thức như tin trong app; agent tắt thì `/wake` hoặc nút ⏰ Gọi dậy; phòng bị chặn thì `/unfreeze`; đổi tài khoản khi hết hạn mức bằng `/switch`; app treo thì 🔄 Khởi động lại hub. Cứu hộ: tin @you không ai trả lời sau 3 phút → bot gửi 1 dòng kèm nút /switch, /wake (`hub/bossrescue.ts:20`, file đang bị xoá trong cây làm việc).

### 5b. Zalo trợ lý (chi tiết)
- **Zalo Code (nick bot)**: chỉ người gửi trong danh sách cho phép được nói với phòng; tin ra Zalo khi có @you/done, gộp theo cửa sổ thời gian; cùng cơ chế `OK <4 số>` cho lệnh nguy hiểm (`hub/zalo.ts:28,154-157`, `hub/zalo-out.ts:29`). Gửi tệp: tool `zalo_send_file` (`mcp/tools.ts:80`, `hub/zalo-out.ts` zaloCliSendFile).
- **Zalo Assistant (nick chính, chỉ đọc)**: lưu tin cục bộ, tóm tắt cuối ngày, nháp việc cần theo dõi chờ chủ duyệt; cấu trúc cấm gửi từ nick này (`hub/zalo-cmd.ts:1-18,56-60`, `hub/zalo-assistant.ts:29`, `hub/zalo-summary.ts:21`, `hub/zalo-office.ts:26`).
- Báo cáo Zalo cho agent: tool `zalo_report` (`mcp/tools.ts`).

## 6. Office, họp, ghi âm

| Tính năng | Tác dụng | Màn hình | file:dòng |
|---|---|---|---|
| Tổng quan Office | Thẻ KPI, biểu đồ 7 ngày, lịch hôm nay | Extra › Office › Tổng quan | `hub/office-stats.ts:33`, `ui/v2/extra.js:3` |
| Mail, lịch (Outlook) | Đồng bộ Outlook classic: mail và lịch (chỉ đọc) vào office.db, tìm toàn văn | Extra › Office | `hub/office-outlook.ts:33`, `hub/office-outlook-mail.ts:10`, `hub/office-search.ts:65`, `hub/officedb.ts:6` |
| Đồng bộ nền | Chu kỳ chỉnh được | Cài đặt › Công cụ | `hub/office-sync.ts:18` |
| Họp, tóm tắt AI | Biên bản và tóm tắt bằng ba engine (Ollama, LAI gateway, profile đám mây) | Extra › Office › Họp | `hub/office-summary.ts:18`, `hub/office-lai.ts:16`, `hub/localai.ts:11` |
| Ghi âm họp | Chọn mic/loa, kiểm mức, Whisper cục bộ, quay màn hình bằng ffmpeg | Extra › Office › Ghi âm | `hub/recording.ts:26`, `hub/recording-api.ts:24`, `hub/screenrec.ts:18` |
| Nhắc việc | Luật: tham dự, sau họp, quá hạn, cần trả lời, cũ, cuối ngày; giờ yên lặng; chuông+toast | Extra › Office › Nhắc | `hub/office-nudges.ts:17` |
| Trợ lý Office | Bản tin 07:30, chuẩn bị họp 15 phút, nhắc theo dõi; hồ sơ thư ký | Extra › Office | `hub/office-brief.ts:10`, `hub/office-assistant.ts:14`, `mcp/tools.ts:29-34` |
| Notion | Đọc trang gốc đã chia sẻ, xuất kết quả; token chỉ ghi | Cài đặt › Công cụ | `hub/notion.ts:10`, `hub/notion-routes.ts:6`, `ui/v2/notion-set.js:1` |
| local_summarize | Tóm tắt bằng Ollama trong máy, không công cụ, không khoá | Cài đặt › Công cụ | `hub/localai.ts:11`, `mcp/tools.ts:28` |

## 7. Kho skill, plugin, addon

| Tính năng | Tác dụng | Màn hình | file:dòng |
|---|---|---|---|
| Kho skill | Thư viện skill kèm theo (kế toán, xây dựng, pháp lý, MEP, solar…), cài/gỡ, danh mục | Agent › Skill | `hub/skilllib.ts:58`, `skill-library/CATALOG.md`, `hub/server.ts:5983` |
| Gói skill theo vai trò | Bộ skill áp một lần cho phòng/agent | Agent › Skill, Luồng | `hub/server.ts:5976` (`/api/skill-packs`), `skill-library/packs.json` |
| Luật skill theo agent | Bật/tắt/ẩn skill từng agent | Agent › Skill | `hub/skillmodes.ts:24`, `hub/skillhide.ts:6`, `ui/v2/skill-rules.js:1` |
| Gợi ý skill theo loại việc | Lead nhận gợi ý và đề xuất cài | Phòng › Luồng | `mcp/tools.ts:58-60` |
| Addon hub | Danh mục MCP server + skill, lập kế hoạch rồi áp dụng sau khi bạn bấm | Hệ thống › Addon | `hub/addons.ts:18`, `hub/installer.ts:18`, `ui/v2/addon.js:1` |
| MCP servers | Xem/thêm/xoá MCP của Claude từ Cài đặt | Cài đặt › Công cụ | `hub/mcpconfig.ts:11`, `hub/server.ts:5901` |
| Plugin của tài khoản | Danh sách và cài plugin qua `claude plugin` | Cài đặt › Tài khoản | `hub/plugins.ts:9` |
| Skill kèm: bear-media-editor, video đào tạo, zalo-code, setup | Dựng video đào tạo từ slide hoặc từ bản quay màn hình, setup máy mới | Skills trong Claude | `skills/bear-media-editor`, `skills/bear_video_slide_training`, `skills/bear-setup`, `skills/zalo-code` |

## 8. Hệ thống, dọn dẹp, cập nhật

| Tính năng | Tác dụng | Màn hình | file:dòng |
|---|---|---|---|
| Bear HUB | Trang chủ: việc cần bạn xử lý, cảnh báo kẹt, hoạt động gần đây | Hệ thống › Bear HUB | `ui/v2/hud.js:3` |
| Nút nguồn | Tắt hoàn toàn, Khởi động lại, Freeze toàn bộ | Cạnh logo | `ui/v2/power.js:1`, `hub/server.ts:7833,7844` |
| Dọn dẹp | Xem trước nhật ký/đính kèm/chat phòng/autojob sắp xoá, bấm nhóm rồi áp dụng; chế độ tự động; thùng rác; cắt transcript | Cài đặt › Hệ thống | `hub/cleanup.ts:15`, `hub/gc.ts:78`, `hub/server.ts:5100-5166` |
| Rác tạm về một gốc | Agent và hub dùng TEMP/TMP dưới `D:\tmp-bam\<agent>` (hoặc `C:\tmp-bam`), không đổ vào %TEMP% | Nền | `hub/tmproot.ts:8`, `hub/agents.ts:22`, `hub/revive.ts:138`, `hub/git.ts:6` (#1035) |
| Thư mục dữ liệu | Xem/mở/di chuyển thư mục dữ liệu BAM, bản ghi lỗi | Cài đặt › Hệ thống | `hub/datadir.ts:8`, `hub/server.ts:6191` |
| Nhật ký hub | JSONL một nơi, gộp dòng lặp, xem trực tiếp, lọc | Usage Account › Log | `hub/log.ts:1`, `hub/server.ts:5177,5193`, `ui/v2/console.js:1` |
| Độ bền | Ghi tệp bền (fsync trước rename), thử lại khoá Windows, ổ chậm không treo hub, chống crash, nhật ký thoát | Nền | `hub/atomic.ts:12`, `hub/slowdir.ts:8`, `hub/crashguard.ts:6`, `hub/exitlog.ts:7` |
| Một lượt một lúc cho Duck/Muse | Chuỗi khoá để Duck và Muse không đọc tab Chrome cùng lúc | Nền | `hub/lock.ts:6` |
| Giao diện | Sáng/xám/tối/tự động + nhiều chủ đề (Nord, Dracula…); tiếng Việt/Anh; lưu ở hub | Chân nav | `ui/v2/themes.js:2`, `shared/i18n/vi.ts`, `shared/i18n/en.ts` |
| Cài đặt (8 nhóm) | Tổng quan, Agent, Tài khoản & nhà cung cấp, Phòng & điều phối, Công cụ, Bộ nhớ & compact, Hệ thống, Zalo & Telegram; xuất/nhập JSON (bỏ khoá bí mật) | Cài đặt | `ui/v2/settings.js:115,213`, `ui/v2/settings-edit.js:99`, `hub/settings.ts:39` |
| Trình hướng dẫn lần đầu | 3 bước: editor, addon an toàn, xong; mở lại từ Cài đặt | /v2/setup | `ui/v2/setup.js:1`, `hub/ideinstalled.ts:16` |
| Tự khởi động | Hub chạy khi đăng nhập Windows (mặc định tắt) | Cài đặt › Hệ thống | `hub/autostart.ts:10` |
| Kiểm tra bản mới, tự cập nhật | App tìm bản mới ở `gnutirt/bam-release` (từ 1.0.6), tải Setup, chuẩn bị rồi cài | Cài đặt › Hệ thống | `hub/version.ts:47` (`RELEASE_REPO`), `hub/appupdate.ts:16`, `hub/server.ts:7783-7795` |
| Máy mới | Xuất/nhập cấu hình sang máy khác | Cài đặt › Hệ thống | `hub/server.ts:5632,5639`, `scripts/machine.mjs`, `skills/bear-setup` |

## 9. Ứng dụng Windows, VSIX, bộ cài

| Tính năng | Tác dụng | Màn hình | file:dòng |
|---|---|---|---|
| BamNative.exe | Vỏ WinUI 3 + WebView2: mỗi phòng một cửa sổ riêng, tray icon, tiêu đề theo theme, quay về WebView nếu native lỗi | Cửa sổ app | `native/bam-native/MainWindow.xaml.cs`, `TrayIcon.cs`, `WebViewRuntime.cs`, `scripts/build-native.mjs:2` |
| Vỏ app/PWA | Manifest + biểu tượng hub phục vụ, mở bằng chế độ app của Edge/Chrome | Cửa sổ app | `hub/appshell.ts:3`, `scripts/launch.mjs` |
| Bộ cài Setup.exe | Cài theo người dùng, không admin, kèm Node riêng; Inno Setup | Trình cài | `installer/bam.iss:32`, `scripts/build-installer.mjs`, `scripts/build-app.mjs`, `scripts/build-host.mjs` |
| Extension VSIX | Panel BAM trong VS Code / Antigravity, chia sẻ chat/skill giữa hai editor | VS Code | `vscode/extension.js`, `vscode/package.json`, `scripts/install-vscode.mjs`, `ui/v2/idelink.js:1` |
| Plugin Claude | Hook (SessionStart, Stop, UserPromptSubmit, Pre/PostToolUse, PreCompact, Notification, StopFailure, SessionEnd) + MCP `room`, `muse`, `bearperm` | Trong Claude Code | `hooks/hooks.json`, `.claude-plugin/plugin.json`, `.mcp.json`, `mcp/server.ts`, `mcp/tools.ts:13-89` |
| Script phát hành | Chạy toàn bộ test + smoke rồi bump version, CHANGELOG | CLI | `scripts/release.mjs`, `scripts/smoke.ts`, `scripts/safe-commit.mjs` |
| Chụp ảnh chuẩn web | Chụp màn /v2 cho audit | CLI | `scripts/shot-v2.mjs`, `scripts/screenshot.mjs` |

## 10. Khác tìm thấy

- **Quy ước BAMshort**: agent báo bằng 1 dòng + biểu tượng cố định (`docs/bamshort.md`, `hub/member.ts:61`).
- **Phòng đóng băng**: tạm dừng một phòng từ app hoặc Telegram (`/api/freeze-all`, `hub/server.ts:7844`).
- **An toàn đường dẫn**: file chỉ xem/mở khi đường dẫn nằm nguyên văn trong tin (`hub/pathview.ts:15`); chặn link vào mạng nội bộ (`hub/linkpeek.ts:20`).
- **Hạn chế kênh chat**: tin ra Zalo gộp trong một cửa sổ thời gian (`hub/zalo-out.ts:29`).

## Chưa kiểm / lưu ý

- Số dòng là dòng khai báo chính tại HEAD `8524e094` + cây làm việc ngày 3/11; mô-đun nào sửa tiếp sẽ lệch.
- `hub/bossrescue.ts` đang bị xoá trong cây làm việc (chưa commit) nhưng `hub/server.ts:52` vẫn import — kiểm lại trước khi chụp.
- Tôi chưa chạy app để xác nhận tên tab; tên lấy từ `ui/v2/nav.js` và `ui/v2/core.js`.
- `ui/v2/themes.js:5` còn danh sách theme dài hơn dòng đã đếm; chưa liệt kê hết tên.
