# Groups

## List & View

```
icqq group list                        # List all groups
icqq group view <gid>                  # View group info
icqq group member list <gid>           # List group members
icqq group member view <gid> <uid>     # View member info
```

## Mute

```
icqq group mute <gid> <uid> [-d seconds]   # Mute member (default 600s, -d 0 to unmute)
icqq group mute-all <gid>                  # Mute all
icqq group mute-all <gid> --off            # Unmute all
```

## Kick & Quit

```
icqq group kick <gid> <uid> [-b]      # Kick member (-b: block rejoin)
icqq group quit <gid>                 # Quit group
```

## Social

```
icqq group poke <gid> <uid>           # Poke group member
icqq group invite <gid> <uid>         # Invite friend to group
icqq group sign <gid>                 # Group check-in
```

## Announcement & Essence

```
icqq group announce <gid> <content>        # Post announcement
icqq group essence add <message_id>        # Set essence message
icqq group essence remove <message_id>     # Remove essence message
```

## Examples

```bash
icqq group list
icqq group view 67890
icqq group member list 67890
icqq group member view 67890 12345
icqq group mute 67890 12345 -d 3600
icqq group kick 67890 12345 -b
icqq group announce 67890 "今晚8点开会"
icqq group sign 67890
```
