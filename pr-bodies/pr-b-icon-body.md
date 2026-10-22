## 改动 / Changes

1. `dsh-desktop-next/src/main.ts`：Linux 上在 `BrowserWindow` 构造后调用 `window.setIcon(nativeImage…resize({width:128}))`（必要时再 64），显式发布 `_NET_WM_ICON`。
2. `dsh-desktop-next/build/app-icon.png`：1024×1024 **16-bit** RGBA → **8-bit** RGBA（1.8MB → 357KB）。Chromium 的 `nativeImage` 无法解码 16-bit PNG，这是任务栏/Alt-Tab 显示齿轮的根本原因。

## 动机 / Motivation

- `BrowserWindow` 的 `icon` 选项在 X11 上不可靠：16-bit 源图解码失败 → `_NET_WM_ICON` 为空 → Cinnamon/GNOME 面板回退到通用的 `application-x-executable` 齿轮图标。
- 本地复现：原始 16-bit 图标下 `_NET_WM_ICON` 为 0；转为 8-bit 并 `setIcon` 后面板显示 DSH 鲸鱼图标。
- Electron 44 对 ≥256px 的 `setIcon` 会静默丢弃，128/64 可靠。

关联 issue #1287（deb 安装后启动器与任务栏无图标）。

## 验证 / Verification

- X11 raw 原始 socket 读回 `_NET_WM_ICON`：有效 64×64 ARGB（4098 个 32-bit 字）。
- Cinnamon `grouped-window-list` 在重启 cinnamon 后正确显示鲸鱼图标，不再是齿轮。
- `Gio.DesktopAppInfo.new('dsh-desktop-next.desktop')` 的 icon/StartupWMClass 与窗口 WM_CLASS 一致。

Fixes #1287
