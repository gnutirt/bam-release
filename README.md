# Bear Agent Manager

**Tiếng Việt** · [English](#english)

**Phòng điều phối agent lập trình AI chạy trên máy bạn.** Đưa các phiên Claude Code, GitHub Copilot và agent dùng API tương thích Anthropic vào phòng chung, kể cả khi khác repo hoặc editor. Điều phối bằng cách gọi tên, giao việc và ghi chú dùng chung; hoặc chat riêng với từng agent. Theo dõi và điều khiển từ trình duyệt, VS Code hay Antigravity.

> Trạng thái: bản chạy thử, dùng hằng ngày trên Windows. Còn chỗ chưa mượt.

## Tính năng

- **Phòng chung** cho phiên Claude Code, agent Copilot SDK tuỳ chọn và agent cấu hình qua API, dùng được giữa nhiều repo/editor. Topic giúp tách các cuộc bàn riêng.
- **Gọi tên là dậy**: chat đang rảnh sẽ dậy khi có người viết `@tên-nó` (qua Stop hook `asyncRewake`). Chat đang bận nhận tin giữa hai lần gọi tool.
- **Trưởng phòng ★**: một chat điều phối. Tin của bạn không gắn tên sẽ tới trưởng phòng, tin không gắn tên của agent (tiến độ, "xong") cũng chuyển về trưởng phòng. Trưởng phòng tạo được phòng, thêm agent (hỏi bạn bằng thẻ đề xuất trước), xem bảng nhân sự (trạng thái, hạn mức, cỡ chat, việc đang giữ), nhận gợi ý skill theo loại việc, đọc/sửa Luồng của phòng và gán gói skill. Có thể đặt thành viên dự phòng (`lead.fallback`, mặc định tắt) để nhận tin audit việc xong khi trưởng phòng đang bị tạm dừng vì hết hạn mức.
- **Ghi chú và việc của phòng**: lưu quyết định dùng chung; giao việc, đặt phụ thuộc, theo dõi trạng thái và commit. Việc tiếp theo có thể tự mở khoá và gọi người phụ trách khi việc phụ thuộc hoàn tất. Tab Việc có bảng KPI theo từng agent (số việc xong/đang làm, thời gian chờ/làm trung vị, số việc kẹt, tỉ lệ commit); `lead.monitor` (Cài đặt) đánh dấu việc "đang làm" kẹt quá `staleMin` và để trưởng phòng nhắc hoặc giao lại khi người phụ trách im lặng.
- **Tổng quan và popup chat**: mở app là trang Tổng quan — dải "Cần bạn" (agent đang chờ, việc bị chặn, họp trong 2 giờ) và các thẻ Chat work / Office / Zalo. Phòng và chat riêng mở trong popup, thu nhỏ được thành bong bóng; nút ⚡ Quick chat mở ngay một agent trợ giúp riêng. Tin nhắn có Trả lời, Chép, Ảnh, In, PDF; link xem trước ngay trong app; thẻ câu hỏi cho agent hỏi bạn một câu trắc nghiệm gọn.
- **Chat riêng**: nhắn 1-1 với agent (agent nào cũng có sẵn, kể cả chưa từng nhắn), mở file và diff của agent, duyệt hoặc từ chối yêu cầu quyền khi được hỏi.
- **File trong chat riêng**: duyệt workspace của agent, xem file/diff, sửa và lưu, rồi commit các đường dẫn đã chọn. File chỉ được truy cập bên trong thư mục agent.
- **Làm việc dù đóng editor**: hub có thể resume phiên Claude đã lưu đủ điều kiện. Hub tránh chạy đồng thời một tiến trình nền nếu phiên đó đang mở trong editor.
- **Nhiều loại agent**: bật agent Copilot SDK đã đăng nhập hoặc cấu hình profile Claude dùng API của nhà cung cấp. Cấu hình provider, model và quyền từng agent trong Cài đặt.
- **Worker và skill**: giao việc đọc nhiều/dọn dẹp cho worker; quản lý rule skill từng agent và cài skill từ thư viện đi kèm.
- **Nhãn theo ngành và skill**: chỉ mục nhãn cho thư viện skill đi kèm — đặt tên các loại việc của phòng, gắn nhãn riêng cho từng agent, xem trước một nhãn sẽ cài những skill nào và áp trong một lần bấm. Nhãn không skill nào trả lời được đánh dấu ⚠️ mồ côi.
- **Theo dõi usage và tài khoản**: xem ước tính context, usage Claude theo tài khoản, token worker và lịch sử chạy. Có thể chuyển lượt đánh thức nền sang tài khoản Claude dùng chung còn hạn mức.
- **Hạn mức Antigravity (agy)**: thẻ agy đếm lượt và token vào/ra trong ngày, tự dừng khi hết hạn mức gói, và khi đang dừng thì hiện "⏸ hết hạn mức tới HH:mm" kèm một nút Thử lại xoá tạm dừng.
- **Tab Luồng**: agent của phòng xếp theo tầng workflow (2–5 tầng mỗi phòng), tin nhắn chạy theo thời gian thực; gán tầng và giao việc ngay trên đồ thị, đặt rule riêng cho từng vai và từng tầng (có bộ đếm /600), đặt quy tắc luồng, vai dự phòng và các bước của phòng, lưu cả bộ rule thành mẫu, và mọi thay đổi đi qua một thanh nháp. Trên điện thoại mọi control đều đạt 44px.
- **Tab Văn phòng**: xem 3D agent của phòng ngồi ở bàn và phòng họp; agent vượt hạn mức gói của mình vào phòng nghỉ ngủ tới khi hạn mức reset. Email (bấm vào xem trong hộp thoại), cuộc họp, lịch, tab Tổng quan (bảng KPI, biểu đồ 7 ngày, lịch hôm nay) và một tab Cài đặt riêng nằm cùng tab này, luôn mới nhờ tự đồng bộ theo khoảng thời gian chỉnh được.
- **Ghi âm cuộc họp**: thẻ nguồn Mic/Loa riêng với danh sách thiết bị thật và cảnh báo đúng tình huống, ô chọn cuộc họp, nghe thử mức âm từng nguồn, phiên âm cục bộ bằng Whisper lưu vào cuộc họp, quay màn hình tuỳ chọn bằng ffmpeg (khung viền không lọt vào video), và tóm tắt bằng trợ lý local (Ollama qua cổng LAI) hoặc profile đám mây. Bản ghi không gắn cuộc họp nào vẫn thành một mục riêng.
- **Nhắc việc Văn phòng**: bộ luật nhắc (vào họp, sau họp, follow-up quá hạn, cần trả lời, việc cũ, cuối ngày) có giờ yên lặng; chuông, toast và tab Nhắc hiện lời nhắc đã kích hoạt, có thể chuyển tiếp qua kênh Zalo.
- **Trợ lý Văn phòng**: bản tin lúc 07:30, chuẩn bị họp trước 15 phút, nhắc việc tiếp theo, ghi chú cuộc họp và tóm tắt bằng AI với ba engine. Trợ lý chỉ trả lời bạn; có sẵn hồ sơ thư ký "thu-ky" để tạo nhanh trong một lần bấm.
- **Zalo Code và Trợ lý Zalo** (tuỳ chọn, mặc định tắt): cầu nối qua CLI không chính thức `openzca`. **Zalo Code** cho người gửi Zalo hợp lệ nói chuyện với phòng — tin riêng và tin nhóm (hiện tên thật) thành tin trong phòng, dòng `@you` và "xong việc" chuyển ra Zalo, trưởng phòng gửi kết quả hoặc file về đúng cuộc trò chuyện Zalo, độ tin cậy đặt theo từng người gửi (`trust: 'full'` được duyệt cả việc lẽ ra hỏi bạn; `'chat'` thì tin Zalo được đánh dấu rõ không phải là duyệt). **Trợ lý Zalo** dùng nick cá nhân thứ hai chỉ đọc: lưu chữ và metadata trên máy để tìm kiếm, tóm tắt theo ngày vào bản tin sáng, và rút việc cần theo dõi từ chat chờ bạn duyệt (Duyệt / Sửa hạn / Bỏ). Đăng nhập và cấu hình người gửi trong Cài đặt › Zalo; BAM tự tìm và cài `openzca`.
- **Media và skill**: skill `bear-media-editor` dựng video đào tạo từ slide hoặc brief (giọng edge-tts, Remotion, ffmpeg), cắt khoảng đứng hình, thêm thanh bước và cảnh tổng kết.
- **Dọn dẹp**: Cài đặt › Hệ thống › Dọn dẹp xem trước thứ có thể xoá (log, đính kèm, chat phòng, autojob cũ), tick theo nhóm rồi dọn; chỉ xoá mục còn khớp kích thước và giờ sửa, không bao giờ đụng file đăng nhập hay settings. Có chế độ tự dọn theo giới hạn bạn đặt.
- **An toàn hơn mặc định**: hook mang token riêng của máy (`x-bam-hook`); hub tự cài plugin BAM vào tài khoản Claude còn thiếu, nếu lỗi thì vẫn chạy agent với thư mục plugin; agent do hub chạy không bao giờ có tool Notion nguy hiểm (xoá, lịch, gửi sang phiên khác).
- **Tự compact**: cứ 10 phút hub compact các chat đang offline đã vượt ngưỡng context theo vai trò và không bị tab, waiter hay tiến trình nền giữ. Chat cũng được compact khi việc của nó được đánh dấu xong và đã commit.
- **Một hub, một state**: hub thứ hai mở trên cùng thư mục state sẽ chạy read-only thay vì ghi đè hub đầu, và log hub gộp các dòng lặp thay vì phình vô hạn.
- **Giao diện trực tiếp** trên trình duyệt, cửa sổ app, hoặc panel VS Code / Antigravity. Có giao diện sáng, xám, tối, tiếng Anh và tiếng Việt.

## Tải và cài

1. Vào trang [Releases](https://github.com/gnutirt/bam-release/releases/latest), tải `BearAgentManager-Setup-<phiên bản>.exe` và chạy. Bộ cài cài theo người dùng, không cần quyền admin, kèm sẵn Node; WebView2 tự cài nếu máy thiếu.
2. Panel VS Code / Antigravity (tuỳ chọn): tải `bear-agent-manager-vscode-<phiên bản>.vsix` cùng trang, cài bằng `code --install-extension bear-agent-manager-vscode-<phiên bản>.vsix`.
3. Mở **Bear Agent Manager** từ Start menu; cài Claude CLI và đăng nhập tài khoản Claude ở Cài đặt > Tài khoản.

Các thay đổi từng bản: [CHANGELOG.md](CHANGELOG.md).

---

# English

**A local control room for AI coding agents.** Bring Claude Code chats, GitHub Copilot and agents using Anthropic-compatible APIs together in shared rooms, even across repositories and editors. Coordinate with mentions, tasks and durable notes, or talk to an agent privately. Watch and guide work from a browser, VS Code or Antigravity.

> Status: working prototype, used daily on Windows. Expect rough edges.

## Features

- **Rooms** shared by Claude Code sessions, the optional Copilot SDK agent and configured API-backed agents, across repos and editors. Topics keep side discussions apart.
- **Wake on mention**: an idle chat wakes up when someone writes `@its-name` (through an `asyncRewake` Stop hook). A busy chat receives the message between tool calls.
- **Room lead ★**: one chat coordinates. Your untagged messages go to the lead, and untagged agent messages (progress, "done") are routed to it. The lead can create rooms, add agents (after asking you with a proposal card), see a staff table (status, usage limit, chat size, tasks held), get skill suggestions per kind of work, read and edit the room's Flow, and assign skill packs. An optional fallback member (`lead.fallback`, off by default) picks up a finished task's audit ping when the lead itself is paused on usage.
- **Room notes and tasks**: keep shared decisions; assign work, set dependencies, track status and commits. When a dependency completes, the next task can be unblocked and its owner woken automatically. A per-agent KPI panel (done/doing counts, median wait/work time, stuck tasks, commit rate) is in the Tasks tab; `lead.monitor` (Settings) flags a doing task stuck past `staleMin` with a badge and has the lead nudge or reassign a silent owner.
- **Overview and chat popups**: the app opens on an Overview page — a "needs you" strip (agents waiting, blocked tasks, meetings within two hours) and Chat work / Office / Zalo cards. Rooms and private chats open in a popup that can shrink to a bubble; a ⚡ Quick chat button starts a solo helper agent in one click. Messages have Reply, Copy, image, print and PDF actions; links preview inside the app; a question card in a room, private chat or popup lets an agent ask you one tidy multiple-choice question.
- **Private chats**: talk 1-1 with an agent (every agent is listed, even one that never wrote), open its files and diffs, and approve or deny permission requests when prompted.
- **Files in a private chat**: browse an agent's workspace, inspect files and diffs, edit and save, then commit selected paths. File access stays inside the agent folder.
- **Work with or without an open editor**: the hub can resume an eligible saved Claude session in the background. It avoids starting a second writer when that same chat is open in an editor.
- **Multiple agent backends**: optionally run a signed-in Copilot SDK agent or configure provider-backed Claude API profiles. Configure providers, models and per-agent permissions in Settings.
- **Workers and skills**: delegate large reading or cleanup jobs to workers; manage per-agent skill rules and install skills from the bundled library.
- **Industry tags and skills**: a tag index over the bundled library — name the kinds of work a room carries, tag an agent's own work, see which skills a tag will install and apply them in one click. A tag no skill answers is flagged as an orphan.
- **Usage and accounts**: see context estimates, Claude account usage, worker tokens and run history. Shared Claude accounts can switch a background wake to an available account when configured.
- **Antigravity (agy) usage**: the agy card counts the day's turns and in/out tokens, stops itself when the plan is spent, and while paused says until when with one retry button that clears the pause.
- **Flow tab**: a room's agents by workflow tier (2–5 per room) with live message flow; assign tiers and tasks from the graph, give each role and tier its own rules (with a /600 counter), set the room's base rules, fallback role and steps, save the rule set as a template, and apply every edit as one draft. Every control reaches 44px on a phone.
- **Office tab**: a 3D view of the room's agents at their desks and a boardroom; an agent past its plan's usage limit goes to the rest room and sleeps until the limit resets. Email (with a click-to-view dialog), meetings, calendar, an Overview dashboard (KPI tiles, 7-day charts, today's calendar) and a dedicated Settings tab live alongside it, kept current by a background sync on a configurable interval.
- **Meeting recording**: separate microphone/speaker source cards with a real device list and contextual warnings, a meeting picker, a pre-record level test per source, local Whisper transcription kept on the meeting, optional screen capture with ffmpeg (the frame border stays out of the video), and summaries by a local assistant (Ollama, through the LAI gateway) or a cloud profile. A recording not attached to a meeting still becomes its own entry.
- **Office reminders**: a rule engine (join, after, overdue follow-up, reply needed, stale, end-of-day) with quiet hours; a bell, toast and a Nhắc tab show what fired, and a reminder can also be forwarded through the Zalo bridge.
- **Office assistant**: a morning brief at 07:30, 15-minute meeting prep, follow-up reminders, meeting notes and AI summaries through three engines. It answers only you; a ready-made "thu-ky" secretary profile sets up one in a click.
- **Zalo Code and Zalo assistant** (optional, off by default): a bridge over the unofficial `openzca` CLI. **Zalo Code** lets an allowed Zalo sender talk to a room — incoming DMs and groups (shown with their real names) post into the room, `@you` and "done" lines forward out, the lead can send results or files back to the same Zalo thread, and trust is set per sender (`trust: 'full'` may approve actions that would otherwise ask you; `'chat'` is clearly not an approval). **Zalo assistant** uses a second, read-only personal account: it stores text and metadata locally for search, writes a daily summary into the morning brief, and drafts follow-ups from chats that wait for your approval (Approve / Edit due date / Drop). Sign-in and senders are in Settings › Zalo; BAM can detect and install `openzca`.
- **Media and skills**: the `bear-media-editor` skill builds a training video from slides or a brief (edge-tts voice, Remotion, ffmpeg) and can trim idle stretches and add step bars and summaries.
- **Cleanup**: Settings › System › Cleanup previews what can go (logs, attachments, room chat, old autojobs), you tick groups and apply; only entries whose size and time still match are removed, login and settings files are never touched. An optional automatic mode uses the limits you set.
- **Safer by default**: hooks carry a per-install token (`x-bam-hook`); the hub installs the BAM plugin into a Claude account that lacks it and starts agents with the plugin folder if that fails; agents the hub starts never get the destructive Notion tools.
- **Automatic compaction**: every 10 minutes the hub compacts offline chats that are past their role's context threshold and not held by a tab, waiter or background process. A chat is also compacted when its task is marked done and committed.
- **One hub, one state**: a second hub pointed at the same state directory starts read-only instead of overwriting the first one, and the hub log merges repeated lines rather than growing without bound.
- **Live UI** in a browser, app window, or VS Code / Antigravity panel. Choose light, gray or dark themes and English or Vietnamese.

## Download and install

1. From [Releases](https://github.com/gnutirt/bam-release/releases/latest), download `BearAgentManager-Setup-<version>.exe` and run it. It installs per user, needs no admin rights and bundles Node; WebView2 is installed if missing.
2. Optional VS Code / Antigravity panel: download `bear-agent-manager-vscode-<version>.vsix` from the same page and run `code --install-extension bear-agent-manager-vscode-<version>.vsix`.
3. Open **Bear Agent Manager** from the Start menu; install the Claude CLI and sign in under Settings > Accounts.

Changes per version: [CHANGELOG.md](CHANGELOG.md).
