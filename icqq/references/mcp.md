# MCP（守护进程内 HTTP）

MCP 与 QQ 守护进程**同进程**；需在配置里开启后重启守护进程。

## 启用

```bash
icqq config get mcp                            # 查看当前 MCP 配置
icqq config set mcp.enabled true
icqq config set mcp.http.token "your-secret"   # 建议设置
icqq service restart                          # 或重新 login
icqq service status                           # 查看 MCP URL
```

端点文件：`~/.icqq/<uin>/daemon.mcp`

## 工具

| 工具 | 作用 |
|------|------|
| `icqq_list_actions` | 列出所有可调用的 IPC action |
| `icqq_invoke` | 执行 action，`params` 为 JSON 对象 |

### 常用 action（与 CLI 等价）

| action | params 示例 |
|--------|-------------|
| `send_private_msg` | `{ "user_id": 12345, "message": "你好" }` |
| `send_group_msg` | `{ "group_id": 67890, "message": "大家好" }` |
| `list_friends` | `{}` |
| `list_groups` | `{}` |
| `group_mute` | `{ "group_id": 67890, "user_id": 12345, "duration": 600 }` |
| `get_system_msg` | `{}` |

完整列表以 `icqq_list_actions` 为准。禁止调用 `logout`。

## Cursor 配置示例

```json
{
  "mcpServers": {
    "icqq": {
      "url": "http://127.0.0.1:3920/mcp",
      "headers": { "Authorization": "Bearer your-secret" }
    }
  }
}
```

端口以 `icqq service status` 或 `daemon.mcp` 为准。

## 示例调用逻辑

1. 先 `icqq_list_actions` 确认 action 名
2. 再 `icqq_invoke` with `action` + `params`
3. 失败时把返回的错误原文给用户
