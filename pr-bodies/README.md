# PR bodies — Next only

本仓库只维护 `dsh-desktop-next`（DSH NEXT）。Stable 已停止支持，不再提交 Stable PR，也不再保留 Stable 分支或文案。

`tracker` 跟随 `upstream/master`，因为 NEXT 位于同一 monorepo 的 `dsh-desktop-next/`；`master` 是 NEXT 的自用集成分支。提交 PR 时从对应 `fix/*` 分支指向 upstream `master`。

| 文案 | 分支 | 目标 | Base | 状态 |
| --- | --- | --- | --- | --- |
| `pr-a-menu-body.md` | `fix/next-linux-role-menu-i18n` | Next / Linux 菜单本地化 | `092e2ecef7` | 待提交 |
| `pr-b-icon-body.md` | `fix/next-linux-window-icon` | Next / Linux 窗口图标 | `092e2ecef7` | 待提交 |

更新文案时同步更新此表；`Base` 用于记录分支起点，方便上游前进后重新 base/rebase。
