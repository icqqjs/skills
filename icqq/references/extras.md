# 其它能力

## 黑名单

```bash
icqq blacklist
```

## OCR

```bash
icqq ocr <图片路径>
```

## Webhook / 通知

```bash
icqq webhook
icqq webhook set <https://...>
icqq webhook off
icqq notify
icqq notify on
icqq notify off
```

## UID 转换

```bash
icqq convert uid <qq号>
icqq convert uin <uid>
icqq convert uids <uid1,uid2,...>   # 批量 UID→QQ
icqq convert uins <qq1,qq2,...>     # 批量 QQ→UID
icqq get cookies [domain]
icqq get csrf-token
icqq get online-status
icqq get refresh-nt-pic-rkey [-f]
```

## 陌生人

```bash
icqq stranger list
icqq stranger view <uid>
icqq stranger status <uid>
icqq stranger add-setting <uid>
```

## 密钥与资源 URL

```bash
icqq get client-key
icqq get pskey <domain>
icqq get video-url <fid> <md5>
```

## 漫游表情

```bash
icqq stamp list
icqq stamp delete
```

## 重载与缓存

```bash
icqq cache clean
icqq reload friends
icqq reload groups
icqq reload blacklist
icqq reload guilds
icqq reload strangers
```

## 频道（Guild）

`icqq channel <子命令>` 与 `icqq guild channel <子命令>` 等价；推荐完整路径 `guild channel`。

```bash
icqq guild list
icqq guild info <guild_id>
icqq guild members <guild_id>
icqq guild channel list <guild_id>
icqq guild channel view <guild_id> <channel_id>
icqq guild channel send <guild_id> <channel_id> <message>
icqq guild channel recall <guild_id> <channel_id> <seq>
```

## RPC 远程 IPC（进阶）

在 `config.json` 开启 `rpc.enabled` 后，守护进程监听 TCP；其它机器可用 `IpcClient.connectRpc` 连接。默认仅 `127.0.0.1`，远程需改 `rpc.host` 并注意安全。

```bash
icqq config set rpc.enabled true
icqq config set rpc.host 127.0.0.1
icqq config set rpc.port 9100
icqq service restart
```

## Shell 补全

```bash
icqq completion zsh
icqq completion bash
icqq completion fish
```

## 脚本化（`--json`）

下列查询/配置类命令支持 `--json`（须带齐 ID 或 URL）：

```bash
icqq --json stranger list
icqq --json stranger view 12345
icqq --json webhook
icqq --json notify
icqq --json guild list
icqq --json guild info <guild_id>
icqq --json reload friends
icqq --json cache clean
```

`webhook set` / `notify on|off` 为变更类，成功 stdout 为简短对象。密钥类（`get client-key`、`get pskey`）建议仅人工终端执行，**不要**写入自动化脚本日志。

**反模式**：无 `guild_id`/`uid` 时用 `--json` 触发选择器；在脚本中调用 `get client-key`。
