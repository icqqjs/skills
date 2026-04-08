# General

## Account

```
icqq login                     # Login QQ and start daemon (interactive wizard)
icqq login -r                  # Quick reconnect using saved token
icqq login -q <uid> -r         # Quick reconnect specific account
icqq status                    # Check all daemon/account statuses
icqq stop                      # Stop the daemon
icqq stop <uid>                # Stop specific daemon
icqq profile                   # View current account profile
icqq requests                  # View pending friend/group requests
```

## Config

```
icqq config get                # View all config
icqq config get <key>          # View specific config key (defaultUin, webhookUrl, notifyEnabled)
icqq config set <key> <value>  # Set config value
```

## Multi-Instance

Use `-u <uin>` global flag or `ICQQ_CURRENT_UIN` env to target a specific account.
Default falls back to `config.defaultUin`.

```
icqq -u 12345 profile          # View profile for account 12345
ICQQ_CURRENT_UIN=12345 icqq friend list
```

## Blacklist

```
icqq blacklist                 # View blacklist
```

## OCR

```
icqq ocr <image>               # OCR image text recognition (local file path)
```

## Webhook

```
icqq webhook                   # View current webhook config
icqq webhook set <url>         # Set webhook URL (daemon pushes events via POST)
icqq webhook off               # Disable webhook
```

## Notification

```
icqq notify                    # View notification status
icqq notify on                 # Enable system notifications
icqq notify off                # Disable system notifications
```

## Examples

```bash
icqq status
icqq profile
icqq requests
icqq login -r
icqq config get
icqq config set defaultUin 12345
icqq -u 12345 profile
icqq ocr ./screenshot.png
icqq webhook set https://example.com/hook
icqq notify on
```
