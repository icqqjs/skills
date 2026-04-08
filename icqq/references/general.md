# General

## Account

```
icqq login                     # Login QQ and start daemon
icqq login -r                  # Quick reconnect using saved config
icqq status                    # Check daemon/account status
icqq stop                      # Stop the daemon
icqq profile                   # View current account profile
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

## Examples

```bash
icqq status
icqq profile
icqq login -r
icqq ocr ./screenshot.png
icqq webhook set https://example.com/hook
icqq webhook
icqq webhook off
```
