# 群文件

`<fid>` 来自 `icqq group fs list` 输出。

## 浏览

```bash
icqq group fs list <gid>
icqq group fs list <gid> -p <父目录fid>
icqq group fs info <gid>
icqq group fs stat <gid> <fid>
```

## 管理

```bash
icqq group fs mkdir <gid> <目录名>
icqq group fs rename <gid> <fid> <新名>
icqq group fs move <gid> <fid> <目标目录fid>
icqq group fs delete <gid> <fid>
```

## 上传下载

```bash
icqq group fs upload <gid> <本地文件>
icqq group fs download <gid> <fid>     # 返回下载链接
icqq group fs forward <gid> <fid> <目标群gid>
icqq group fs forward-offline <gid> <fid> [--name 文件名]
```

## 示例

```bash
icqq group fs list 67890
icqq group fs mkdir 67890 "资料"
icqq group fs upload 67890 ./report.pdf
icqq group fs download 67890 abc123fid
icqq group fs forward-offline 67890 offline-fid --name "资料.pdf"
```

## 脚本化（`--json`）

浏览与文件操作均可脚本化；`list`/`stat`/`download` 等须带齐 `gid` 与 `fid`：

```bash
icqq --json group fs list 67890
icqq --json group fs stat 67890 <fid>
icqq --json group fs download 67890 <fid>
icqq --json group fs info 67890
```

上传/转发类成功时 stdout 为 action 返回体。见 [json.md](./json.md)。

**反模式**：省略 `fid` 或 `gid` 指望交互补全。
