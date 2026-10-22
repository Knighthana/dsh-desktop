## 改动 / Changes

Linux 应用菜单的 `editMenu` / `viewMenu` / `windowMenu` 及其 role 子项在 Linux 上不会跟随 Desktop 语言，永远显示英文（编辑/查看/窗口整棵菜单都是 `Edit` / `View` / `Window`，撤销/复制等子项同样）。Windows 已经通过 `resolveDesktopLocale` 做了中英双语，macOS 由系统自动本地化，唯独 Linux 缺口。

This PR expands Electron's role menus on Linux with the same bilingual zh/en strings Windows already uses via `resolveDesktopLocale`, and keeps bare roles on macOS so system-owned translations still apply.

## 动机 / Motivation

- `Menu.setApplicationMenu` 在 `native-desktop.ts` 的 `refresh()` 和 `main.ts` 里各有一处，都只挂了 bare `{ role: 'editMenu' | 'viewMenu' | 'windowMenu' }`。
- Electron 只在 macOS 上自动本地化 role 菜单；Linux 保持英文。
- 抽出 `roleApplicationMenuTemplate(appItems, language)`，Linux 展开为带 `t(zh, en)` 标签的 role 菜单；macOS 保持 bare role 不变。

## 验证 / Verification

- 扩展 `tests/menu-locale.spec.ts`：
  - macOS 仍返回 bare role 菜单
  - Linux 中文下：编辑/查看/窗口及其子项（撤销/重做/剪切/复制/粘贴/删除/全选/重新加载/放大/缩小/最小化/关闭等）标签正确
  - Linux 英文下：Edit / Undo 等
- 本地在 Cinnamon + Electron 44 上截图验证顶层与三棵子菜单均为中文（安装的 asar 补丁与本 PR 逻辑一致）。

Fixes the Linux half of the native-menu i18n gap (Windows already localized; macOS is system-localized).
