---
name: icqq
description: 通过 icqq CLI 或可用的 icqq MCP 操作 QQ，包括发私聊或群消息、撤回、查聊天记录、好友和群管理、请求处理、群文件、资料设置、登录、系统服务、Webhook、OCR、频道和缓存。用户提到 QQ、icqq、好友、群、消息、撤回、禁言、踢人、签到、加群、群文件、登录、守护进程、MCP 或通知时使用。
---

# icqq QQ 操作技能

## 执行原则

- 默认用终端 `icqq ...` 执行；只有宿主已配置 icqq MCP 且工具可用时，才改用 MCP，见 [mcp.md](./references/mcp.md)。
- 执行前必须先打开下方匹配的 `references/*.md`，从文档复制命令并替换占位符；不要凭记忆猜命令。
- 优先执行一条明确命令。不要把多个 `icqq` 命令串成一行。
- 用户已给出精确目标和动作时直接执行；目标、账号、群号或操作含义不清时先问一句确认。
- 对删好友、踢人、全体禁言、退群、登出、卸载服务、同意或拒绝请求等高影响动作，确认目标缺失或语义含糊时必须先澄清。
- 失败时回报关键错误，但先脱敏 `Authorization`、`GITHUB_TOKEN`、PAT、MCP token 和不必要的完整本地路径。
- 不要为了“预检”先跑 `icqq service status`；只有用户明确询问状态、配置 MCP、排查登录/服务问题，或相关命令失败后才查看状态。

## 执行流程

1. **选模块**：按路由表打开一个 reference；跨模块任务只打开必要的几个。
2. **组命令**：替换 `<uid>`、`<gid>`、`<message>` 等占位符；消息或名称含空格时用双引号。
3. **执行**：用非交互命令；如果文档标注会进入交互模式，不要用于代理执行。
4. **回报**：简短说明成功结果；失败则给出脱敏后的错误和下一步建议。

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
| 登录、登出、切换账号、系统服务、配置 | [account.md](./references/account.md) |
| MCP `icqq_invoke` / `icqq_list_actions` | [mcp.md](./references/mcp.md) |
| 频道、OCR、黑名单、Webhook、重载缓存等 | [extras.md](./references/extras.md) |

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
- **CQ 码**（写在消息字符串里）：`[face:id]` `[image:路径或URL]` `[at:uid]` `[at:all]` `[dice]` `[rps]`
- **含空格**：双引号包裹，如 `icqq friend send 12345 "你好 世界"`
- **JSON 输出**：需要结构化结果时给命令加全局参数 `--json`。
- **安装依赖**：提示缺少 `@icqqjs/icqq` 时看 [account.md](./references/account.md) 的 `icqq setup`。

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

细节与更多命令见各 reference 文件。
