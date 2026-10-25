# PR bodies

PR 文案集中放在这里，代码分支保持在仓库正常分支上。提交 PR 时从对应 `fix/*` 分支指向 upstream `master`。

| 文案 | 分支 | 目标 | Base | 状态 |
| --- | --- | --- | --- | --- |
| `pr-a-menu-body.md` | `fix/next-linux-role-menu-i18n` | Next / Linux 菜单本地化 | `06446375d9` | 待提交 |
| `pr-b-icon-body.md` | `fix/next-linux-window-icon` | Next / Linux 窗口图标 | `06446375d9` | 待提交 |
| `pr-c-linux-release-body.md` | `fix/next-linux-release-entry` | Next / Linux 发布入口 | `06446375d9` | 待提交 |
| `pr-d-linux-window-title-body.md` | `fix/stable-linux-window-title` | Stable / Linux 窗口标题 | `092e2ecef7` | 待提交 |
| `pr-e-linux-close-to-tray-body.md` | `fix/stable-linux-close-to-tray` | Stable / Linux 关闭到托盘 | `092e2ecef7` | 待提交 |

更新文案时同步更新此表；`Base` 用于记录分支起点，方便上游前进后重新 base/rebase。
