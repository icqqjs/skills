# 脚本化输出（`--json`）

多数 IPC 命令支持全局 `--json`，供脚本与代理解析结构化结果。

## 用法

`--json` 为**全局参数**，放在子命令前：

```bash
icqq --json friend list
icqq --json -u 12345 group view 67890
icqq -u 12345 --json profile
```

不带 QQ 号参数的查询命令，省略交互选择器时必须**显式传入 ID**（如 `friend view 12345`），否则无法在非 TTY 环境运行。

## 成功与失败

| 情况 | 输出 | 退出码 |
|------|------|--------|
| 成功 | stdout：IPC 返回的 `data`（pretty JSON） | 0 |
| 失败 | stderr：`{"ok":false,"error":"..."}` | 1 |
| 守护进程未运行 | stderr 含「守护进程未运行」及 `icqq login` 提示 | 1 |

变更类命令（`send`、`mute`、`kick` 等）成功时 stdout 为 action 返回体；无结构化数据时可能为 `null` 或简短对象。

## 常用查询示例

```bash
# 列表
icqq --json friend list
icqq --json group list
icqq --json requests

# 资料（须带 ID，勿依赖交互选择）
icqq --json friend view 12345
icqq --json group view 67890
icqq --json group member view 67890 12345
icqq --json profile

# 消息
icqq --json msg get <message_id>
icqq --json friend chat history 12345 -c 20
icqq --json group chat history 67890 -c 20
icqq --json msg history-by-id <message_id> -c 10
```

## 解析建议

- 用 `jq` 或程序解析 stdout；**不要**把 stderr 当成功结果。
- 多账号场景始终加 `-u <uin>` 或 `ICQQ_CURRENT_UIN`，避免误用 `currentUin`。
- MCP 路径见 [mcp.md](./mcp.md)；CLI `--json` 与 MCP `icqq_invoke` 返回的 `data` 语义一致。

## 反模式

| 不要做 | 原因 |
|--------|------|
| `icqq friend view --json`（无 uid） | 会进入交互选择，脚本挂起 |
| 把 stderr 成功日志当 JSON 解析 | 失败才写 stderr |
| 未登录时批量 `--json` | 先 `icqq login -r` 或 `service start` |
