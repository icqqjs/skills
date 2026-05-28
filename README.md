# icqq Skill

让 AI 代理通过 **`icqq` CLI**（或可选 **MCP**）操作 QQ：发消息、管群、处理请求等。

## 安装

```bash
npx skills add https://github.com/icqqjs/skills
```

或手动复制 `icqq/` 到平台的 skills 目录（如 `~/.cursor/skills/icqq/`）。

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
└── references/           # 按意图按需打开，不要一次全读
    ├── account.md        # 登录、系统服务、配置
    ├── mcp.md            # MCP icqq_invoke
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
3. 失败就回报错误，不预检守护进程

人类用户命令大全见仓库根目录 [README.md](../README.md)。

## License

ISC
