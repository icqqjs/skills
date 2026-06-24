# 群管理

## 查询

```bash
icqq group list
icqq group view <gid>
icqq group member list <gid>
icqq group member view <gid> <uid>
icqq group muted-list <gid>
icqq group at-all-remain <gid>
```

## 发消息与社交

```bash
icqq group send <gid> <message>
icqq group send -a <gid> <message>   # 匿名
icqq group send-temp <gid> <uid> <message>
icqq group send-long <gid> <message>
icqq group share-card <gid> <url> <title>
icqq group poke <gid> <uid>
icqq group invite <gid> <uid>       # 邀请好友入群
icqq group sign <gid>             # 签到
```

## 禁言与踢人

```bash
icqq group mute <gid> <uid> [-d 秒]   # 默认 600；-d 0 解禁
icqq group mute-all <gid>
icqq group mute-all <gid> --off       # 解除全体禁言
icqq group kick <gid> <uid> [-b]      # -b 拉黑入群
icqq group quit <gid>
icqq group screen-member <gid> <uid>
```

## 公告与精华

```bash
icqq group announce <gid> <内容>
icqq group essence add <message_id>
icqq group essence remove <message_id>
```

## 表态

```bash
icqq group reaction add <message_id> <表情id>
icqq group reaction remove <message_id> <表情id>
```

这里直接传群消息的 `<message_id>`，程序内部会自动解析对应的群号与消息序列号。

## 示例

```bash
icqq group list
icqq group send 67890 "大家好"
icqq group mute 67890 12345 -d 3600
icqq group kick 67890 99999 -b
icqq group sign 67890
icqq group announce 67890 "本周五团建"
icqq group reaction add "AAEAAQAAAA..." "128077"
```

## 脚本化（`--json`）

```bash
icqq --json group list
icqq --json group view 67890            # group_id, group_name, member_count, owner_id 等
icqq --json group member list 67890
icqq --json group member view 67890 12345   # user_id, card, role, level, join_time 等
icqq --json group muted-list 67890
icqq --json group at-all-remain 67890
```

`group view` / `group member view` 须同时传入群号与成员 QQ 号。详见 [json.md](./json.md)。
