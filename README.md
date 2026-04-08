# icqq Skill

一个 AI 代理技能（Agent Skill），让 AI 代理能够通过 `icqq` CLI 操作 QQ 账号——发消息、管理好友与群组、处理请求、管理群文件等。

## 概述

当你在支持 Skills 的 AI 代理平台中描述 QQ 相关操作时，此技能会自动激活，引导代理加载对应的参考文档并在终端中执行正确的 `icqq` 命令。

### 安装

推荐使用 [skills CLI](https://github.com/nickstefan/skills) 一键安装：

```bash
npx skills add https://github.com/icqqjs/skills
```

该命令会自动检测当前环境中的 AI 代理平台，并将技能文件安装到正确位置。

<details>
<summary>手动安装</summary>

将 `icqq/` 目录（包含 `SKILL.md` 和 `references/`）复制到对应平台的 skills 目录中：

| 平台 | 安装位置 |
|------|----------|
| [GitHub Copilot](https://code.visualstudio.com/docs/copilot/copilot-extensibility-overview) (VS Code) | `~/.copilot/skills/icqq/` 或项目内 `.copilot/skills/icqq/` |
| [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/skills) | `~/.claude/skills/icqq/` 或项目内 `.claude/skills/icqq/` |
| 其他兼容平台 | 参考各平台 Skills 文档放置 `SKILL.md` 及 `references/` 目录 |

</details>

### 触发示例

> "给好友 12345 发一条消息"
> "把群 67890 全体禁言"
> "帮我签到所有群"
> "查看待处理的好友请求"

## 前置条件

| 要求 | 说明 |
|------|------|
| Node.js | >= 22 |
| [`@icqqjs/cli`](https://github.com/icqqjs/cli/pkgs/npm/cli) | 已全局安装（`npm i -g @icqqjs/cli`） |
| QQ 账号 | 已通过 `icqq login` 登录并保持守护进程运行 |
| AI 代理平台 | 任一支持 Skills 的平台（见上方安装说明） |

## 目录结构

```
skills/icqq/
├── README.md              # 本文件 — 技能说明文档
├── SKILL.md               # 技能入口 — 包含 YAML frontmatter 和调度逻辑
└── references/            # 模块化参考文档（按需加载）
    ├── messaging.md       # 发消息、撤回、聊天记录
    ├── friends.md         # 好友列表、查看、戳一戳、点赞、删除、备注
    ├── groups.md          # 群管理：禁言、踢人、公告、邀请、签到、精华、成员
    ├── settings.md        # 个人/群设置：昵称、头像、签名、群名片、群头衔
    ├── requests.md        # 好友/群请求处理
    ├── gfs.md             # 群文件系统：目录管理
    └── general.md         # 登录、状态、OCR、黑名单
```

## 工作原理

### 1. 技能匹配

`SKILL.md` 的 YAML frontmatter 中定义了 `description` 字段，AI 代理根据该描述判断用户意图是否匹配此技能：

```yaml
description: 'Operate QQ account via icqq CLI. Use when asked to: send QQ message,
  manage QQ groups, check QQ friends, poke friend, like friend, mute member, kick
  member, set nickname, view QQ profile, handle friend/group requests, manage group
  files, set group announcement, QQ签到, 发消息, 管群, 好友操作, 群文件.'
```

### 2. 模块化调度

代理不会一次性加载全部参考文档。`SKILL.md` 中的模块表指导代理**仅读取与用户意图相关的参考文件**：

| 用户意图 | 加载的参考文件 |
|----------|---------------|
| 发消息、撤回、聊天记录 | `references/messaging.md` |
| 好友操作 | `references/friends.md` |
| 群管理 | `references/groups.md` |
| 个人/群设置 | `references/settings.md` |
| 处理请求 | `references/requests.md` |
| 群文件 | `references/gfs.md` |
| 登录、状态、OCR、黑名单 | `references/general.md` |

### 3. 执行流程

```
用户描述操作 → 代理匹配 SKILL.md → 读取对应 references/*.md → 终端执行 icqq 命令 → 返回结果
```

关键设计决策：

- **`disable-model-invocation: true`** — 强制代理必须读取参考文件后再行动，不允许凭记忆猜测命令
- **非交互优先** — 代理始终使用 `icqq send`（非交互）而非 `icqq friend chat`（交互模式）
- **状态检查** — 不确定账号是否在线时，先执行 `icqq status`

## 命令速查

### 账号与通用

```bash
icqq login                              # 登录 QQ 并启动守护进程
icqq status                             # 查看状态
icqq stop                               # 停止守护进程
icqq profile                            # 查看个人资料
icqq ocr <image>                        # 图片文字识别
icqq blacklist list                     # 查看黑名单
```

### 消息

```bash
icqq send private <uid> <message>       # 发送私聊消息
icqq send group <gid> <message>         # 发送群消息
icqq recall <message_id>                # 撤回消息
icqq friend chat history <uid> [-c N]   # 查看私聊记录
icqq group chat history <gid> [-c N]    # 查看群聊记录
```

CQ 码语法：

| 语法 | 说明 |
|------|------|
| `[face:id]` | QQ 表情（id 0~348） |
| `[image:path]` | 图片（本地路径或 URL） |
| `[at:uid]` | @某人 |
| `[at:all]` | @全体成员 |
| `[dice]` | 骰子 |
| `[rps]` | 猜拳 |

混合示例：`icqq send group 67890 "你好[face:178]看图[image:/tmp/pic.jpg]"`

### 好友

```bash
icqq friend list                        # 好友列表
icqq friend view <uid>                  # 查看好友资料
icqq friend poke <uid>                  # 戳一戳
icqq friend like <uid> [-t times]       # 点赞（1-20次）
icqq friend remark <uid> <remark>       # 设置备注
icqq friend delete <uid> [-b]           # 删除好友（-b 同时拉黑）
```

### 群管理

```bash
icqq group list                         # 群列表
icqq group view <gid>                   # 查看群信息
icqq group member list <gid>            # 群成员列表
icqq group member view <gid> <uid>      # 查看成员资料
icqq group mute <gid> <uid> [-d sec]    # 禁言（默认 600s，-d 0 解禁）
icqq group mute-all <gid> [--off]       # 全体禁言/解禁
icqq group kick <gid> <uid> [-b]        # 踢人（-b 不再接受入群）
icqq group quit <gid>                   # 退群
icqq group poke <gid> <uid>             # 戳一戳
icqq group invite <gid> <uid>           # 邀请入群
icqq group sign <gid>                   # 群签到
icqq group announce <gid> <content>     # 发群公告
icqq group essence add <message_id>     # 设为精华消息
icqq group essence remove <message_id>  # 移除精华消息
```

### 群设置

```bash
icqq group set name <gid> <name>        # 修改群名
icqq group set avatar <gid> <file>      # 修改群头像
icqq group set card <gid> <uid> <card>  # 修改群名片
icqq group set title <gid> <uid> <title># 设置群头衔
icqq group set admin <gid> <uid> [-r]   # 设置/取消管理员
icqq group set remark <gid> <remark>    # 修改群备注
```

### 群文件

```bash
icqq group fs list <gid> [-p pid]       # 列出群文件
icqq group fs info <gid>                # 查看存储信息
icqq group fs mkdir <gid> <name>        # 新建目录
icqq group fs rename <gid> <fid> <name> # 重命名
icqq group fs delete <gid> <fid>        # 删除文件/目录
```

### 个人设置

```bash
icqq set nickname <name>                # 修改昵称
icqq set gender <0|1|2>                 # 性别（0 未知 / 1 男 / 2 女）
icqq set birthday <YYYYMMDD>            # 修改生日
icqq set signature <text>               # 修改签名
icqq set description <text>             # 修改简介
icqq set avatar <file>                  # 修改头像
icqq set online-status <code>           # 在线状态（11 在线 / 31 离开 / 41 隐身 / 50 忙碌 / 60 Q我吧 / 70 免打扰）
```

### 请求处理

```bash
icqq requests                           # 查看待处理请求
icqq request accept <flag>              # 接受好友请求
icqq request accept <flag> -g           # 接受群请求
icqq request reject <flag> [-g] [-r "理由"]  # 拒绝请求
```

## 约定

- `<uid>` — QQ 号码（整数）
- `<gid>` — 群号码（整数）
- `<fid>` — 文件/目录 ID（由 `icqq group fs list` 输出）
- `<flag>` — 请求标识（由 `icqq requests` 输出）
- 包含空格的字符串用引号包裹：`icqq send private 12345 "hello world"`
- 批量操作用 `&&` 串联

## 编写与维护参考文档

每个参考文件遵循统一格式：

1. **一级标题** — 模块名称
2. **二级标题** — 功能分类（如 List & View、Actions）
3. **代码块** — 命令语法，每行一个命令加注释
4. **Examples 段** — 可直接复制执行的完整示例

添加新命令时，将其放入最相关的参考文件中。如果是全新领域，创建新的 `references/<module>.md` 并在 `SKILL.md` 的模块表中注册。

## License

ISC
