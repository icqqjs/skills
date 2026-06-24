# 账号、登录与系统服务

## 安装与核心依赖

CLI 依赖 `@icqqjs/icqq`。缺少核心依赖时先用 `icqq setup`，不要手动把 `@icqqjs:registry` 写入全局 `~/.npmrc`。

```bash
icqq setup
icqq setup --token <PAT>
GITHUB_TOKEN=<PAT> icqq setup
```

## 登录与登出

```bash
icqq login                     # 交互式登录并启动守护进程（含 MCP/RPC 网络向导）
icqq login -r                  # 用已保存 token 快速重连
icqq login -q <uin> -r         # 指定账号快速重连
icqq logout                    # 登出并停止守护（token 作废）
icqq logout -k                 # 仅断开，保留 token（可再 login -r）
icqq logout <uin>              # 登出指定账号
icqq switch <uin>              # 切换 currentUin（守护进程未跑时会提示）
icqq profile                   # 当前账号资料
icqq requests                  # 待处理好友/群请求（见 requests.md）
```

**登录补充**：

- 首次 `icqq login` 会引导配置全局 MCP/RPC 开关；端口写入当前账号（留空或 `0` 自动选取不冲突端口）。
- 二次登录可为单账号覆盖 MCP/RPC 端口：`icqq -u <uin> config set mcp.http.port 3920`。
- **设备锁验证**：终端会引导选择 **手机短信验证**（有密保手机时）或 **浏览器打开链接验证**（↑↓ 选择 + 回车）。代理无法代填验证码，需用户在本机完成。
- **237 身份验证**：按终端提示在浏览器控制台注入设备信息（见登录流程输出）。

## 系统服务

每个 QQ 号一个 launchd plist / systemd unit。**不写 QQ 号 = 对 config.accounts 里全部账号操作**；写 QQ 号则只操作该号。

```bash
icqq service install           # 安装并启动（全部账号）
icqq service install <uin>     # 仅该账号
icqq service uninstall         # 卸载
icqq service start             # 启动（会清除 intentional shutdown 标记）
icqq service stop              # 停止（保留 plist/unit）
icqq service restart           # 重启（改 MCP 等配置后执行）
icqq service status            # 各账号服务 + 守护进程 + MCP 地址
icqq service status <uin>      # 仅该账号
```

`icqq service status` 还会提示：

- 服务 PID 与守护进程 PID 不一致（可能崩溃循环）
- 服务显示运行但守护进程 socket 不可达
- 多账号 MCP URL 重复 / 过期（⚠）

注意：`icqq logout` 不会阻止服务自动拉起；要彻底停服务需 `icqq service uninstall`。macOS 上正常 `logout` 会写入 `daemon.stopped`，launchd 不会自动重启，需再次 `service start`。

## 配置

```bash
icqq config get                    # 含 mcp / rpc 摘要
icqq config get mcp                # 全部 MCP 项（含解析后的默认值）
icqq config get mcp.enabled
icqq config set <key> <value>       # 全局
icqq -u <uin> config set mcp.http.port 3920   # 账号级端口覆盖
```

常用键：`currentUin`、`webhookUrl`、`notifyEnabled`、`mcp.enabled`、`mcp.http.host`、`mcp.http.port`（`0` = 自动）、`mcp.http.token`、`mcp.plugins`、`rpc.enabled`、`rpc.port`

## 多账号

```bash
icqq -u 12345 friend list
ICQQ_CURRENT_UIN=12345 icqq group send 67890 "hi"
```

## 故障排查速查

| 现象 | 处理 |
|------|------|
| 守护进程已在运行 | `icqq service stop` 或 `icqq logout -k` 后重试 |
| 守护进程未运行 | `icqq login -r` 或 `icqq service start` |
| MCP 端口冲突 | `icqq config set mcp.http.port 0` 后 `icqq service restart` |
| macOS logout 后服务仍重启 | 检查 `daemon.stopped`；需 `service start` 清除 |

## 示例

```bash
icqq setup
icqq login -r
icqq service status
icqq service restart
icqq config set mcp.enabled true
icqq config set mcp.http.token "your-secret"
icqq -u 12345 profile
icqq --json -u 12345 friend list
```

## 脚本化（`--json`）

### 仅交互（代理勿用）

下列命令需 TTY、验证码或逐步向导，**不要**加 `--json` 让代理执行：

- `icqq login`（首次登录、设备锁、滑块）
- `icqq setup`（依赖安装向导）
- `icqq service install` / `uninstall`（系统服务安装）
- `icqq logout`（登出并停守护进程）

快速重连 `icqq login -r` 在无额外验证时通常可非交互；若仍弹出验证，改由用户本机执行。

### 可脚本化示例

```bash
icqq --json profile
icqq --json -u 12345 config get mcp
icqq --json service status
icqq --json service status 12345
icqq --json switch 12345          # 仅切换 currentUin，不启动登录向导时
```

`config set` / `service start|stop|restart` 等变更类也可用 `--json` 看返回体。格式见 [json.md](./json.md)。

**反模式**：对 `login` 加 `--json` 指望代理完成验证码。
