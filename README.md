# Zalo for Linux

A clean re-port of the **Zalo** desktop app (Vietnamese messenger by VNG) to
**Linux x64**, packaged as a `.deb` or Arch Linux package (`.pkg.tar.zst`). The bundle is extracted from the official
**macOS DMG** (`ZaloSetup-universal`), patched minimally, and its native modules
are rebuilt from source for Linux. Runs on **Electron 39** (Chromium 142) with
native Wayland.

> Zalo is a trademark of VNG Corporation. This project is **not affiliated with
> or endorsed by VNG**. It repackages the original bundle with minimal patches
> and rebuilds native modules from source for Linux. For personal use.

## Features

- **Native Wayland** (Electron 39) — reliable drag-and-drop ("Gửi nhanh"),
  including fast drags that broke under the old XWayland build.
- **Vietnamese input** via fcitx5/ibus.
- **JPEG-XL images** — Zalo stores received photos as `.jxl`; Chromium 142 can't
  decode them, so the app is forced onto the native RE'd decoders
  (`zjxl` + `zimage`) which render and thumbnail them correctly.
- **System tray** — icon, tooltip, context menu, unread badge, status switching,
  show/quit (the macOS tray, un-gated for Linux).
- **Window state** — opens maximized and remembers size/maximized across restart.
- **Native modules rebuilt from source** — SQLCipher (`sqlite3`), E2EE backup
  decrypt (`db-cross-v4`), `zfile`, `zjxl`, `zimage`, `v8-profiles`.

---

## Installation & Usage

### 1. Arch Linux / Manjaro (Quickest)

Chỉ cần clone repo và chạy `makepkg` để tự động tải phụ thuộc, biên dịch và cài đặt:

```bash
git clone [https://github.com/ngmvix2010/zalo-linux.git](https://github.com/ngmvix2010/zalo-linux.git)
cd zalo-linux
makepkg -si
