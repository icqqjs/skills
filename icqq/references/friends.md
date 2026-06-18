# 好友

## 查询

```bash
icqq friend list
icqq friend view <uid>
icqq friend profile <uid>           # 详细资料卡
icqq friend avatar-url <uid>
icqq friend same-groups <uid>
icqq friend share-card <uid> <url> <title>
```

## 操作

```bash
icqq friend send <uid> <message>
icqq friend poke <uid>
icqq friend like <uid> [-t 次数]      # 1~20，默认 1
icqq friend remark <uid> <备注>
icqq friend delete <uid> [-b]         # -b 同时拉黑
icqq friend add <uid>
```

## 文件

```bash
icqq friend send-file <uid> <文件路径>
icqq friend forward-file <uid> <fid> [-g 目标群]
icqq friend file-info <uid> <fid>
icqq friend file-url <uid> <fid>
icqq friend recall-file <uid> <fid>
```

## 好友分组

```bash
icqq friend class list
icqq friend class add <分组名>
icqq friend class delete <id>
icqq friend class rename <id> <新名>
icqq friend class set <uid> <分组id>
```

## 示例

```bash
icqq friend list
icqq friend send 12345 "在吗"
icqq friend poke 12345
icqq friend like 12345 -t 10
icqq friend remark 12345 "同事-小王"
```
