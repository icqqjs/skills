---
name: icqq
description: 通过 icqq CLI 或 MCP 操作 QQ：发私聊/群消息、撤回、聊天记录；好友与群管理；好友/群请求；群文件；资料与设置；登录与系统服务。用户提到 QQ、企鹅、好友、群、消息、撤回、禁言、踢人、签到、加群、群文件、登录、守护进程时使用。
argument-hint: 用一句话说明操作，例如「给好友 12345 发你好」或「禁言群 11111 的用户 67890 十分钟」
disable-model-invocation: true
---

# icqq QQ 操作技能

## 先选执行方式

| 方式 | 何时用 |
|------|--------|
| **终端 `icqq …`** | 默认；代理在 shell 里执行 |
| **MCP `icqq_invoke`** | 宿主已配置 HTTP MCP 且工具可用时，可减少拼命令；见 [mcp.md](./references/mcp.md) |

无论哪种方式，**都必须先读下方对应参考文档再执行**，不要凭记忆猜命令。

## 执行流程（3 步）

1. **对表选模块** → 打开一个 `references/*.md`
2. **从文档复制命令** → 替换 `<uid>` `<gid>` 等占位符
3. **运行并回报** → 失败就直接贴错误；**不要**事先 `service status` 探测

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

- **发消息**：用 `icqq friend send` / `icqq group send`（非交互）。不要用 `friend chat` / `group chat`（交互式，代理用不了）。
- **多账号**：`icqq -u <uin> …` 或 `ICQQ_CURRENT_UIN=<uin>`；不写则用 `config.currentUin`。
- **CQ 码**（写在消息字符串里）：`[face:id]` `[image:路径或URL]` `[at:uid]` `[at:all]` `[dice]` `[rps]`
- **含空格**：双引号包裹，如 `icqq friend send 12345 "你好 世界"`
- **批量**：`cmd1 && cmd2`

## 常见意图 → 第一条命令

| 意图 | 命令 |
|------|------|
| 发私聊 | `icqq friend send <uid> "<消息>"` |
| 发群聊 | `icqq group send <gid> "<消息>"` |
| 好友列表 | `icqq friend list` |
| 群列表 | `icqq group list` |
| 禁言 | `icqq group mute <gid> <uid> -d <秒>` |
| 待处理请求 | `icqq requests` |
| 群签到 | `icqq group sign <gid>` |
| 看服务状态 | `icqq service status` |

细节与更多命令见各 reference 文件。
