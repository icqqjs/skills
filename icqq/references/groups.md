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
icqq group reaction add <gid> <seq> <表情id>
icqq group reaction remove <gid> <seq> <表情id>
```

`<seq>` 是群消息序列号，不是全局 `<message_id>`。

## 示例

```bash
icqq group list
icqq group send 67890 "大家好"
icqq group mute 67890 12345 -d 3600
icqq group kick 67890 99999 -b
icqq group sign 67890
icqq group announce 67890 "本周五团建"
icqq group reaction add 67890 123456 "128077"
```
