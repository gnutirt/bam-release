# Install, update, VS Code

**English** · [Tiếng Việt](#tiếng-việt)

## Install
1. Download `BearAgentManager-Setup-<version>.exe` from [Releases](https://github.com/gnutirt/bam-release/releases/latest) and run it. Per-user install, no administrator rights, its own Node included.
2. Start Menu › **Bear Agent Manager**. The hub starts if needed and the app window opens (WinUI 3 + WebView2; if the native shell fails it falls back to a web window).
3. First-run wizard: pick your editor, safe add-ons, done. You can reopen it from Settings.
4. Add a Claude account: see [accounts](accounts.md).

To go from 1.0.4/1.0.5 to 1.0.6, run the Setup by hand once; after that the app updates itself.

Optional: **Settings › System › Autostart** runs the hub when you sign in to Windows (off by default).

## Update
The app checks `gnutirt/bam-release` and downloads the signed zip (`BearAgentManager-<version>-win-x64.zip`, `checksums.txt`, `checksums.txt.sig`; refused unless the signature and sha256 match). It unpacks the new version beside the old one and switches when no agent works; Settings › System › the *Cập nhật ứng dụng* box has *Download*, *Switch to* and *Go back*. The hub restarts on the switch, so background agents restart afterwards. Running the Setup over an install keeps `~/.bear-agent-manager/`. A release is published only after its Setup was installed and opened on a real machine.

## The app windows
Main window `BAM.exe`; rooms, direct chats, Quick chat and Zalo open as their own Windows windows. Closing the main window hides it to the tray (*Open* / *Exit*); one BAM per user.

## VS Code and Antigravity
Optional panel `bear-agent-manager-vscode-<version>.vsix` on the Releases page: *Extensions › Install from VSIX*, then **Developer: Reload Window**. It shows the hub's page inside the editor and can share chats and skills between the two editors. Nothing needs it to run.

## Move to a new machine
Settings › System exports and imports your configuration (secret keys are left out). Data folder: `~/.bear-agent-manager/` (see Settings › System). Keep it private: it holds the hub token.

## Clean up
Settings › System › Cleanup previews what can go (logs, attachments, room chat, old auto-jobs); tick groups, apply. Login and settings files are never touched. Temp files of agents live under `D:\tmp-bam` (or `C:\tmp-bam` without a D: drive).

Source map: [`_features.md`](_features.md) sections 8 and 9.

---

# Tiếng Việt

[English](#install-update-vs-code) · **Tiếng Việt**

## Cài đặt
1. Tải `BearAgentManager-Setup-<phiên bản>.exe` ở [Releases](https://github.com/gnutirt/bam-release/releases/latest) rồi chạy. Cài theo người dùng, không cần admin, kèm Node riêng.
2. Start Menu › **Bear Agent Manager**. Hub tự chạy nếu cần và cửa sổ app mở ra (WinUI 3 + WebView2; vỏ native lỗi thì quay về cửa sổ web).
3. Trình hướng dẫn lần đầu: chọn editor, addon an toàn, xong. Mở lại được từ Cài đặt.
4. Thêm tài khoản Claude: xem [accounts](accounts.md).

Từ 1.0.4/1.0.5 lên 1.0.6 chạy Setup bằng tay một lần; sau đó app tự cập nhật.

Tuỳ chọn: **Cài đặt › Hệ thống › Tự khởi động** chạy hub khi đăng nhập Windows (tắt sẵn).

## Cập nhật
App tìm bản mới ở `gnutirt/bam-release`, tải zip có ký (`BearAgentManager-<phiên bản>-win-x64.zip`, `checksums.txt`, `checksums.txt.sig`; chữ ký hoặc sha256 sai thì từ chối). Bản mới giải nén cạnh bản cũ, app chuyển khi không agent nào đang làm; Cài đặt › Hệ thống › Cập nhật ứng dụng có *Tải*, *Chuyển sang*, *Quay lại*. Hub khởi động lại khi chuyển, nên agent chạy nền chạy lại sau đó. Chạy Setup đè lên bản đã cài thì `~/.bear-agent-manager/` còn nguyên. Một bản chỉ được đưa lên khi Setup của nó đã cài và mở được trên máy thật.

## Cửa sổ của app
Cửa sổ chính `BAM.exe`; phòng, chat riêng, Quick chat và Zalo mở thành cửa sổ Windows riêng. Đóng cửa sổ chính thì ẩn xuống khay (*Mở BAM* / *Thoát*); mỗi người dùng một BAM.

## VS Code và Antigravity
Panel tuỳ chọn `bear-agent-manager-vscode-<phiên bản>.vsix` ở trang Releases: *Extensions › Install from VSIX*, rồi **Developer: Reload Window**. Nó hiện trang của hub trong editor và chia sẻ chat, skill giữa hai editor. Không có gì cần nó để chạy.

## Chuyển sang máy mới
Cài đặt › Hệ thống xuất và nhập cấu hình (bỏ khoá bí mật). Thư mục dữ liệu: `~/.bear-agent-manager/` (xem Cài đặt › Hệ thống). Giữ riêng tư: có token của hub.

## Dọn dẹp
Cài đặt › Hệ thống › Dọn dẹp cho xem trước thứ sắp xoá (log, tệp đính kèm, chat phòng, autojob cũ); tick nhóm rồi áp dụng. Tệp đăng nhập và cài đặt không bao giờ bị đụng. Tệp tạm của agent nằm dưới `D:\tmp-bam` (hoặc `C:\tmp-bam` nếu không có ổ D:).

Bản đồ nguồn: [`_features.md`](_features.md) mục 8 và 9.
