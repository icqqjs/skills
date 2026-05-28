# 资料与群设置

## 个人

```bash
icqq set nickname <昵称>
icqq set gender <0|1|2>              # 0 未知 1 男 2 女
icqq set birthday <YYYYMMDD>
icqq set signature <签名>
icqq set description <说明>
icqq set avatar <图片路径>
icqq set online-status <码>          # 11在线 31离开 41隐身 50忙碌 60Q我 70勿扰
```

## 群

```bash
icqq group set name <gid> <群名>
icqq group set avatar <gid> <图片>
icqq group set card <gid> <uid> <群名片>
icqq group set title <gid> <uid> <头衔>
icqq group set admin <gid> <uid>     # 设管理员
icqq group set admin <gid> <uid> -r  # 取消管理员
icqq group set remark <gid> <群备注>
icqq group set anonymous <gid>       # 开启匿名
icqq group set anonymous <gid> -d    # 关闭匿名
icqq group set rate-limit <gid> <每分钟条数>
icqq group set join-type <gid> <1|2|3> [-q 问题] [-a 答案]
```

## 示例

```bash
icqq set signature "今天也要加油"
icqq set online-status 41
icqq group set card 67890 12345 "管理员"
icqq group set join-type 67890 3 -q "暗号是什么" -a "icqq"
```
