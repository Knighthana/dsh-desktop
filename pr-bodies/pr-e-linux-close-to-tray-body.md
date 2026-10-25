## 改动 / Changes

Linux Stable 关闭主窗口时隐藏到托盘，而不是最小化：

1. `dsh-plugin-desktop/src/electron-platform.ts`：Linux `hidesWindowOnClose` 从 `false` 改为 `true`，并更新策略注释。
2. `dsh-plugin-desktop/tests/electron-platform.spec.ts`：Linux 策略断言改为 hide。
3. `dsh-plugin-desktop/tests/electron-runtime.spec.ts`：Linux close 触发 `window.hide()`，不再 `minimize()`；退出流程仍直接关闭。

Branch: `fix/stable-linux-close-to-tray`
Base: `upstream/master` @ `092e2ecef750d10db43cedbab8e81fd48c8de38e`

## 动机 / Motivation

- Windows/macOS 关闭时隐藏，Linux 目前最小化，行为不一致。
- Stable 启动后会在 `electron-shell-generation.ts` 创建托盘，托盘点击可恢复窗口；关闭应保留 Host 和会话在后台运行，而不是把窗口留在任务栏。
- 与 Next 的 `closeToTray` 默认行为一致。

## 已知限制 / Known limitation

- Electron 无法判断 Linux 桌面是否真正显示 StatusNotifier/status area；`new Tray()` 在没有面板托盘区的桌面上也可能成功。
- 本 PR 假设启动时创建的托盘可访问，符合当前目标桌面；如果维护者希望保留旧的无托盘回退，可后续加 `closeToTray` 偏好或状态区检测。Next 已有 `closeToTray` 设置可参考。

## 验证 / Verification

- `corepack yarn workspace dsh-plugin-desktop vitest run tests/electron-platform.spec.ts tests/electron-runtime.spec.ts`
- `corepack yarn workspace dsh-plugin-desktop typecheck`
- Cinnamon/X11 本地验证：点关闭后窗口 `IsUnMapped`，Host 进程继续运行；托盘点击/再次启动恢复窗口。
- 退出应用时窗口正常销毁，不被隐藏逻辑拦截。

Scope: Stable `dsh-plugin-desktop`.
