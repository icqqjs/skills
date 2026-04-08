# Group File System (GFS)

## List Files

```
icqq group fs list <gid>                # List root directory
icqq group fs list <gid> -p <pid>      # List subdirectory by parent ID
```

## Info & Stats

```
icqq group fs info <gid>               # View GFS usage stats (space, file count)
```

## Manage

```
icqq group fs mkdir <gid> <name>              # Create directory
icqq group fs delete <gid> <fid>              # Delete file/directory by ID
icqq group fs rename <gid> <fid> <name>       # Rename file/directory
```

The `<fid>` (file ID) is shown in `icqq group fs list` output.

## Examples

```bash
icqq group fs list 67890
icqq group fs info 67890
icqq group fs mkdir 67890 "会议资料"
icqq group fs rename 67890 abc123 "新文件名"
icqq group fs delete 67890 abc123
```
