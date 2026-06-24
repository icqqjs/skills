# 好友与群请求

## 查看

```bash
icqq requests    # 输出含 <flag>，用于 accept/reject
```

## 处理

```bash
icqq request accept <flag>              # 同意好友
icqq request accept <flag> -g           # 同意入群/邀请
icqq request reject <flag>              # 拒绝好友
icqq request reject <flag> -g           # 拒绝群
icqq request reject <flag> -g -r "理由" # 拒绝并附理由
```

## 示例

```bash
icqq requests
icqq request accept 1a2b3c4d
icqq request reject 5e6f7g8h -g -r "群已满"
```

## 脚本化（`--json`）

```bash
icqq --json requests    # 待处理好友/群请求列表，含 <flag>
```

处理请求（`accept`/`reject`）也可用 `--json` 获取操作结果。见 [json.md](./json.md)。
