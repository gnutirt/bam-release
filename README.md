# Bear Agent Manager (BAM)

Bear Agent Manager là phòng điều khiển chạy ngay trên máy cho các agent lập trình AI. BAM gom các chat Claude Code (và GitHub Copilot, hoặc agent dùng API tương thích Anthropic) vào những phòng chung, kể cả khác repo và khác trình soạn thảo. Bạn điều phối bằng @tên, việc và ghi chú lâu dài, hoặc nhắn riêng từng agent, ngay trong cửa sổ app hay trong VS Code.

> Trạng thái: bản dùng thử, chạy hằng ngày trên Windows. Có thể còn lỗi vặt.

## Tính năng chính

- **Phòng chung**: nhiều agent ở nhiều repo trò chuyện cùng nhau; chủ đề (topic) giữ các cuộc bàn phụ riêng ra.
- **Đánh thức khi được gọi**: chat đang rảnh tự dậy khi có người viết `@tên`; chat đang bận nhận tin giữa hai lượt gọi công cụ.
- **Trưởng phòng ★**: một agent điều phối, nhận tin bạn không gắn tên và báo cáo của các agent khác.
- **Việc và ghi chú**: giao việc, đặt phụ thuộc, theo dõi trạng thái và commit; việc xong thì việc kế tiếp tự mở và đánh thức người nhận.
- **Chat riêng 1-1** với từng agent: xem file và diff, sửa và commit, duyệt hoặc từ chối yêu cầu quyền.
- **Chạy nền không cần mở editor**: hub tự nối lại phiên Claude đã lưu, tránh hai bên cùng ghi một chat.
- **Nhiều tài khoản Claude**: xem mức dùng, tự chuyển lượt đánh thức sang tài khoản còn hạn mức.
- **Tab Luồng**: xem agent theo tầng và dòng tin theo thời gian thực, đặt luật cho từng vai trò.
- **Tab Office**: mail, họp, lịch, ghi âm họp và phiên âm cục bộ (tuỳ chọn).
- **Worker và skill**: giao việc đọc nặng hoặc dọn dẹp cho worker; cài skill từ thư viện kèm theo.
- **Telegram và Zalo** (tuỳ chọn): nhắn với phòng và nhận báo cáo ngay từ điện thoại.
- **Giao diện sáng, tối hoặc tự động**; mỗi phòng có thể mở thành một cửa sổ riêng.

## Tải và cài

1. Vào trang **Releases** của repo này, tải file `BearAgentManager-Setup-<phiên bản>.exe` ở bản mới nhất (hiện tại: `BearAgentManager-Setup-1.0.5-2.exe`).
2. Chạy file vừa tải. Bộ cài cài theo người dùng hiện tại, **không cần quyền admin**, mặc định vào thư mục Programs trong hồ sơ người dùng, kèm sẵn Node.
3. Mở **Bear Agent Manager** từ Start menu. Lần đầu, vào **Cài đặt > Tài khoản** để cài Claude CLI (nếu máy chưa có) và đăng nhập tài khoản Claude.
4. Cập nhật: tải Setup của bản mới ở trang Releases rồi chạy đè lên bản cũ. Chat, cài đặt và dữ liệu phòng được giữ nguyên.

## Extension VS Code

Trang Releases có kèm file `.vsix` của panel BAM (cho VS Code và Antigravity). Cài bằng lệnh:

```
code --install-extension bear-agent-manager-vscode-1.0.5-2.vsix
```

Sau đó chạy **Developer: Reload Window** trong VS Code. Panel là phần tuỳ chọn; app và hub chạy được mà không cần nó.

## Yêu cầu

- Windows 10 hoặc 11.
- [Claude Code](https://claude.com/claude-code) (bộ cài của BAM có thể cài giúp Claude CLI) và một tài khoản Claude.
- WebView2 Runtime cho cửa sổ app: Windows 11 có sẵn; nếu máy thiếu, bộ cài tự tải và cài.
