# 账号、登录与系统服务

## 登录与登出

```bash
icqq login                     # 交互式登录并启动守护进程
icqq login -r                  # 用已保存 token 快速重连
icqq login -q <uin> -r         # 指定账号快速重连
icqq logout                    # 登出并停止守护（token 作废）
icqq logout -k                 # 仅断开，保留 token（可再 login -r）
icqq logout <uin>              # 登出指定账号
icqq switch <uin>              # 切换当前操作账号
icqq profile                   # 当前账号资料
icqq requests                  # 待处理好友/群请求（见 requests.md）
```

## 系统服务

每个 QQ 号一个 launchd plist / systemd unit。**不写 QQ 号 = 对 config.accounts 里全部账号操作**；写 QQ 号则只操作该号。

```bash
icqq service install           # 安装并启动（全部账号）
icqq service install <uin>     # 仅该账号
icqq service uninstall         # 卸载
icqq service start             # 启动
icqq service stop              # 停止（保留 plist/unit）
icqq service restart           # 重启（改 MCP 等配置后执行）
icqq service status            # 各账号服务 + 守护进程 + MCP 地址
icqq service status <uin>      # 仅该账号
```

注意：`icqq logout` 不会阻止服务自动拉起；要彻底停服务需 `icqq service uninstall`。

## 配置

```bash
icqq config get
icqq config get <key>
icqq config set <key> <value>
```

常用键：`currentUin`、`webhookUrl`、`notifyEnabled`、`mcp.enabled`、`mcp.http.port`、`mcp.http.token`、`rpc.enabled`

## 多账号

```bash
icqq -u 12345 friend list
ICQQ_CURRENT_UIN=12345 icqq group send 67890 "hi"
```

## 示例

```bash
icqq login -r
icqq service status
icqq service restart
icqq config set mcp.enabled true
icqq config set mcp.http.token "your-secret"
icqq -u 12345 profile
```
