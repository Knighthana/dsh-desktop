## 改动 / Changes

为 `dsh-desktop-next` 补上缺失的 Linux 发布入口：

1. **`package.json`**：新增 `dist:linux` / `check:linux-package` / `check:linux-package:platform`；`desktopName: dsh-desktop-next.desktop`；`build.linux` 从仅 `dir` 扩展为 AppImage + deb（`executableName`、`syncDesktopName`、`artifactName`、`extraResources` 保留）；新增 `build.deb` 块。
2. **`scripts/package.ts`**：新增 `linux` 模式，复用 Beta 的 `packageLinuxArtifacts`（Next 的 `run` 会把 preflight workspace 名改写为本包）。
3. **`scripts/package-linux.ts`**（新）：Next 专属 options 工厂，payload gate 走 `verifyNextRuntimeTree` + `preparePayload`。
4. **`scripts/verify-linux-artifacts.ts`**（新）：校验 AppImage/deb/可执行文件之外，**断言 deb 与 unpacked 树内含 `resources/runtime/x64/primary-runtime/runtime.json`**（#1428 的直接防线）。
5. **测试**：扩展 `package.spec.ts` 的 linux 配置断言；新增 `package-linux.spec.ts`（编排顺序）与 `verify-linux-artifacts.spec.ts`（含/不含 runtime 两组）。
6. **CI**：`desktop-linux` job matrix 加入 `dsh-desktop-next`。
7. **根 `package.json`**：新增 `dist:linux:next`。

## 动机 / Motivation

- #1451 给 Linux 的 electron-builder 补了 `extraResources`，但 Next **从未有过** `dist:linux` / `check:linux-package` / CI 覆盖。
- 结果：Next 的 deb 只能由手工构建树产出，绕过 `prepare:runtime` 与全部 gate，**不携带 primary-runtime** → 安装后 Host 启动即失败，进入恢复模式（#1428）。
- 本地复现：`2.0.19-next` deb 清单中 `resources/runtime` 条目数为 0；错误日志 `the bundled primary runtime is missing: …/resources/runtime/x64/primary-runtime`。
- Stable/Beta 已有完整的 `package-linux.ts` + verifier + CI；Next 缺口是结构性的。

## 验证 / Verification

- `package.spec.ts`：linux target 为 AppImage+deb、`desktopName`、`syncDesktopName`、`deb` 块、`dist:linux`/`check:linux-package` 脚本存在。
- `package-linux.spec.ts`：preflight → builder → verifier 顺序；`DSH_PACKAGE_CHECK_ALREADY_RAN` 跳过 preflight；压缩覆盖。
- `verify-linux-artifacts.spec.ts`：含 runtime 的 fixture 通过；deb 缺 runtime 或 unpacked 缺 runtime 均失败；带 `./` 前缀的成员路径也能匹配。

Fixes #1428
