## 改动 / Changes

Stable Linux 的原生窗口标题现在跟随当前页面/会话标题：

1. `dsh-plugin-desktop/src/window-options.ts`：Linux 的 `BrowserWindow` 初始标题使用 `spec.windowTitle`，不再留空。
2. `dsh-plugin-desktop/src/electron-shell-generation.ts`：`page-title-updated` 只在 Windows/macOS 上 `preventDefault()`，Linux 接受 renderer 的 `document.title`。
3. 测试：Linux 窗口初始标题；Linux 不拦截 `page-title-updated`；macOS 仍拦截（现有测试保留）。

Branch: `fix/stable-linux-window-title`
Base: `upstream/master` @ `092e2ecef750d10db43cedbab8e81fd48c8de38e`

## 动机 / Motivation

- Stable 的 Windows/macOS 使用 Desktop 自己的 frame，故意忽略 renderer 标题更新；Linux 使用原生 frame，标题来自 `document.title`。
- 同一策略在 Linux 上导致标题为空，切换会话后也不会更新。
- Web client 已经把标题设为 `${sessionTitle} — ${productTitle}`（`@deepseek-ai/dsh-client-ui-layout`），所以只需让 Linux 接收该更新，不需要改 renderer。

## 验证 / Verification

- `corepack yarn workspace dsh-plugin-desktop vitest run tests/window-options.spec.ts tests/electron-runtime.spec.ts`
- `corepack yarn workspace dsh-plugin-desktop typecheck`
- Cinnamon/X11 本地验证：空白标题变为 `DSH 插件安装命令报错原因 — DeepSeek Harness`；切换会话后标题跟随变化。
- macOS/Windows 的稳定标题行为不变。

Scope: Stable `dsh-plugin-desktop`. Upstream Beta 已迁移到 Next shell，不涉及本 PR。
