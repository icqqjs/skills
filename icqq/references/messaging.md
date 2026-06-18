# 消息

## 发送（代理必用非交互）

```bash
icqq friend send <uid> <message>       # 私聊
icqq group send <gid> <message>      # 群聊（-a 匿名）
icqq group send-temp <gid> <uid> <message>   # 群临时会话
icqq friend send-long <uid> <message>       # 私聊长消息
icqq group send-long <gid> <message>         # 群聊长消息
icqq msg history-by-id <message_id> [-c 条数]  # 以 msgid 锚点拉历史
```

不要用 `icqq friend chat` / `icqq group chat`（交互模式，代理无法使用）。

## CQ 码（写在 message 里）

```
[face:178]                 # 表情 id 0~348
[image:/path/to/a.jpg]     # 图片，或 URL
[at:12345]                 # @某人
[at:all]                   # @全体
[dice]  [rps]              # 骰子 / 猜拳
```

示例：

```bash
icqq group send 67890 "你好[face:21][image:https://example.com/p.png]"
```

## 撤回与详情

```bash
icqq recall <message_id>
icqq msg get <message_id>
icqq msg mark-read <message_id>
icqq forward get <message_id>          # 合并转发内容
```

## 聊天记录

```bash
icqq friend chat history <uid> [-c 条数]    # 默认 20
icqq group chat history <gid> [-c 条数]
```

## 示例

```bash
icqq friend send 12345 "你好"
icqq group send 67890 "通知：今晚开会"
icqq group send-temp 67890 12345 "你好，来自群临时会话"
icqq friend chat history 12345 -c 50
icqq recall abcdef123456
```
