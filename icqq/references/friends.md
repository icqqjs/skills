# Friends

## List & View

```
icqq friend list              # List all friends
icqq friend view <uid>        # View friend profile
```

## Actions

```
icqq friend poke <uid>                 # Poke a friend
icqq friend like <uid> [-t times]      # Like (1-20 times, default 1)
icqq friend delete <uid> [-b]          # Delete friend (-b: also block)
icqq friend remark <uid> <remark>      # Set friend remark/alias
```

## Examples

```bash
icqq friend list
icqq friend view 12345
icqq friend poke 12345
icqq friend like 12345 -t 10
icqq friend remark 12345 "小王"
icqq friend delete 12345 -b
```
