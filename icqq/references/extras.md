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
```

## 陌生人

```bash
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
```

## 频道（Guild）

```bash
icqq guild list
icqq guild info <guild_id>
icqq guild members <guild_id>
icqq guild channel list <guild_id>
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
