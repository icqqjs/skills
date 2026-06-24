---
name: icqq
description: 通过 icqq CLI 或可用的 icqq MCP 操作 QQ，包括发私聊或群消息、撤回、查聊天记录、好友和群管理、请求处理、群文件、资料设置、登录、系统服务、配置、headless 告警推送、--json 脚本化、Webhook、OCR、频道和缓存。用户提到 QQ、icqq、好友、群、消息、撤回、禁言、踢人、签到、加群、群文件、登录、守护进程、service、switch、Bark、告警、alerts、MCP 或通知时使用。
---

# icqq QQ 操作技能

## 执行原则

- 默认用终端 `icqq ...` 执行；只有宿主已配置 icqq MCP 且工具可用时，才改用 MCP，见 [mcp.md](./references/mcp.md)。
- **CLI vs MCP**：① 宿主无 MCP 工具 → 只用 CLI；② 有 MCP 且用户要自动化/已在 MCP 上下文 → `icqq_list_actions` 再 `icqq_invoke`；③ 高影响动作 → 无论哪条路径都须 🔴 确认。
- 执行前必须先打开下方匹配的 `references/*.md`，从文档复制命令并替换占位符；不要凭记忆猜命令。
- 优先执行一条明确命令。不要把多个 `icqq` 命令串成一行。
- 用户已给出精确目标和动作时直接执行；目标、账号、群号或操作含义不清时先问一句确认。
- 🔴 **高影响动作**（删好友、踢人、全体禁言、退群、登出、卸载服务、`request accept`/`reject` 尤其带 `-g` 的入群）：目标或语义缺失时 **STOP**，先向用户确认再执行。
- 失败时回报关键错误，但先脱敏 `Authorization`、`GITHUB_TOKEN`、PAT、MCP token 和不必要的完整本地路径。
- 不要为了“预检”先跑 `icqq service status`；只有用户明确询问状态、配置 MCP、排查登录/服务问题，或相关命令失败后才查看状态。

## 执行流程

1. **选模块**：按路由表打开一个 reference；跨模块任务只打开必要的几个。
2. **组命令**：替换 `<uid>`、`<gid>`、`<message>` 等占位符；消息或名称含空格时用双引号。
3. **🛑 CHECKPOINT**：高影响或批量操作前，向用户复述将执行的**完整命令**（含 `-u`/`-g`），等确认后再执行。
4. **执行**：用非交互命令；文档标注 **仅交互** 的命令不要用于代理执行（见 [account.md](./references/account.md)）。
5. **回报**：简短说明成功结果；失败则查下方「失败与排查」表，给出脱敏错误和下一步。

前提：用户本机已 `icqq login` 或系统服务在跑。未登录时命令会失败，提示用户登录即可。

## 模块路由表

| 用户想做什么 | 打开 |
|--------------|------|
| 发消息、撤回、历史、消息详情、合并转发 | [messaging.md](./references/messaging.md) |
| 好友列表、资料、戳一戳、点赞、删好友、备注、好友文件 | [friends.md](./references/friends.md) |
| 群消息、禁言、踢人、公告、签到、精华、成员、邀请 | [groups.md](./references/groups.md) |
| 昵称、头像、签名、群名片、群头衔等设置 | [settings.md](./references/settings.md) |
| 好友请求、入群请求 | [requests.md](./references/requests.md) |
| 群文件目录、上传下载 | [gfs.md](./references/gfs.md) |
| 登录、登出、切换账号、系统服务、配置、headless 告警、故障排查 | [account.md](./references/account.md) |
| 脚本化 `--json` 输出与解析 | [json.md](./references/json.md) |
| MCP `icqq_invoke` / `icqq_list_actions` | [mcp.md](./references/mcp.md)（对照表 [mcp-cli-map.md](./references/mcp-cli-map.md)，**自动生成勿手改**） |
| 频道、OCR、黑名单、Webhook、补全、重载缓存等 | [extras.md](./references/extras.md) |

## 全局约定（必记）

| 占位符 | 含义 |
|--------|------|
| `<uid>` | QQ 号 |
| `<gid>` | 群号 |
| `<fid>` | 群文件 ID（来自 `icqq group fs list`） |
| `<flag>` | 请求标识（来自 `icqq requests`） |
| `<message>` | 消息内容，支持普通文本和 CQ 码 |

- **发消息**：用 `icqq friend send` / `icqq group send` / `icqq group send-temp`（非交互）。不要用 `friend chat` / `group chat`（交互式，代理用不了）。
- **多账号**：`icqq -u <uin> …` 或 `ICQQ_CURRENT_UIN=<uin>`；不写则用 `config.currentUin`。
- **账号级 MCP/RPC 端口**：`icqq -u <uin> config set mcp.http.port 3920`（见 [account.md](./references/account.md)）。
- **CQ 码**（写在消息字符串里）：`[face:id]` `[image:路径或URL]` `[at:uid]` `[at:all]` `[dice]` `[rps]`
- **含空格**：双引号包裹，如 `icqq friend send 12345 "你好 世界"`
- **JSON 输出**：多数 IPC 命令支持全局 `--json`（如 `icqq --json friend list`）。成功 stdout 为 JSON；失败 stderr 为 `{"ok":false,"error":"..."}`，退出码 `1`。详见 [json.md](./references/json.md)。
- **安装依赖**：提示缺少 `@icqqjs/icqq` 时看 [account.md](./references/account.md) 的 `icqq setup`。

## 失败与排查

| 触发条件 | 一线修复 | 仍失败兜底 |
|----------|----------|------------|
| 报错守护进程未运行 | `icqq login -r` 或 `icqq service start` | `icqq service status` 看守护进程/MCP；见 [account.md](./references/account.md) |
| 登录需设备锁验证 | 终端选 **手机短信** 或 **浏览器链接**（↑↓ + 回车） | 用户本机执行 `icqq login`，代理无法代填验证码 |
| MCP 端口冲突 / 多账号 URL 重复 | `icqq config set mcp.http.port 0` 后 `icqq service restart` | `icqq service status` 看 ⚠ 端口冲突提示 |
| macOS 服务 logout 后仍重启 | 正常退出会写 `daemon.stopped`；需 `icqq service start` 清除标记 | `icqq service uninstall` 后重装 |
| 服务器收不到告警 / 无登录链接 | `icqq config get alerts`；配置 `login.http.publicUrl`；改后 `service restart` | 见 [account.md](./references/account.md) 告警一节 |
| 缺少 `@icqqjs/icqq` | `icqq setup`（勿手改全局 `~/.npmrc`） | `icqq setup --token <PAT>` |
| MCP action 不存在或参数错 | 先 `icqq_list_actions` 再 `icqq_invoke` | 对照 [mcp-cli-map.md](./references/mcp-cli-map.md)；勿猜 action 名 |
| 子命令不存在 / `Unknown command` | 回对应 reference 复制命令，检查拼写与层级 | 不要凭记忆改子命令名 |

## 反模式（不要做）

| 反模式 | 为什么 | 正确做法 |
|--------|--------|----------|
| 用 `friend chat` / `group chat` 发消息 | 交互模式，代理无法完成 | `friend send` / `group send` |
| 多个 `icqq` 命令串一行 | 难归因、易部分失败 | 一条命令一次执行 |
| 未读 reference 就猜子命令 | 路由与参数易错 | 先打开对应 `references/*.md` |
| 高影响操作不确认 | 误删/误踢不可恢复 | 🔴 STOP，先问用户 |
| 为“保险”每次先 `service status` | 浪费轮次、违背 skill 约定 | 仅排查或用户询问时用 |
| MCP 凭记忆写 action/params | discovery 与 invoke 已统一 contract | 先 `icqq_list_actions` |
| 泄露 token / PAT / 完整 `~/.icqq` 路径 | 安全风险 | 回报前脱敏 |
| 手改 `mcp-cli-map.md` | 与源码漂移，CI 会失败 | 改 CLI 后跑 `pnpm generate:skill-map`（维护者） |

## 常见意图 → 第一条命令

| 意图 | 命令 |
|------|------|
| 发私聊 | `icqq friend send <uid> "<消息>"` |
| 发群聊 | `icqq group send <gid> "<消息>"` |
| 发群临时会话 | `icqq group send-temp <gid> <uid> "<消息>"` |
| 好友列表 | `icqq friend list` |
| 群列表 | `icqq group list` |
| 禁言 | `icqq group mute <gid> <uid> -d <秒>` |
| 待处理请求 | `icqq requests` |
| 群签到 | `icqq group sign <gid>` |
| 看服务状态 | `icqq service status` |
| 脚本化列表 | `icqq --json friend list` |

细节与更多命令见各 reference 文件。
