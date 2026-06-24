# icqq Skill

让 AI 代理通过 **`icqq` CLI**（或可选 **MCP**）操作 QQ：发消息、管群、处理请求等。

## 安装

```bash
npx skills add https://github.com/icqqjs/skills
```

通用 skills 目录（按宿主平台择一）：

| 平台 | 典型路径 |
|------|----------|
| Cursor | `~/.cursor/skills/` 或项目 `.cursor/skills/` |
| Claude Code | `~/.claude/skills/` |
| Codex / 其它 | 宿主文档中的 `skills` 或 `agents/skills` 目录 |

也可手动将本仓库 `icqq/` 目录复制到上述 skills 根目录。

## 前置条件

- Node.js ≥ 22
- `npm i -g @icqqjs/cli` 与 `@icqqjs/icqq`（或 `icqq setup`）
- 已 `icqq login` 或已 `icqq service install`

## 结构

```
icqq/
├── SKILL.md              # 入口：路由表 + 全局约定（代理先读这个）
├── agents/
│   └── openai.yaml       # UI 展示名、简短描述和默认提示
├── test-prompts.json     # Agent 回归提示（Darwin / 评测）
└── references/           # 按意图按需打开，不要一次全读
    ├── account.md        # 登录、系统服务、配置
    ├── json.md           # --json 脚本化输出
    ├── mcp.md            # MCP icqq_invoke
    ├── mcp-cli-map.md    # MCP action ↔ CLI（自动生成）
    ├── messaging.md
    ├── friends.md
    ├── groups.md
    ├── settings.md
    ├── requests.md
    ├── gfs.md
    └── extras.md         # 频道、OCR、RPC 等
```

## 代理怎么用

1. 匹配用户意图 → 打开 **一个** reference
2. 用文档里的命令在终端执行（或 MCP）
3. 失败查 SKILL.md「失败与排查」表，不预检守护进程

脚本化：`icqq --json …` 输出 JSON；失败 stderr `{"ok":false,"error":"..."}`。

人类用户命令大全见 CLI 仓库 [README.md](https://github.com/icqqjs/cli/blob/master/README.md)。

## License

ISC
