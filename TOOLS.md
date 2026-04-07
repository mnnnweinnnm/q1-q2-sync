# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- VPS (CS-Bot) → 128.199.249.195 (DigitalOcean SGP1)
- cPanel → sg1-ts5.a2hosting.com:2083 (user: shane042)

### 🔑 API Key / Token 管理（macOS Keychain）

所有 key 統一存在 macOS Keychain，account = `openclaw`。
讀取方式：`security find-generic-password -a "openclaw" -s "<service>" -w`

| Service Name | 服務 | 備註 |
|---|---|---|
| `openai-api-key` | OpenAI | GPT-5 fallback、圖片、STT |
| `anthropic-api-key` | Anthropic | API key（env 也有一份） |
| `discord-bot-token` | Discord | Q1 Bot |
| `godaddy-api-key` | GoDaddy | 域名管理 API |
| `godaddy-api-secret` | GoDaddy | API Secret |
| `cloudflare-account-id` | Cloudflare | Account ID |
| `cloudflare-api-token-1` | Cloudflare | 第一組 Token |
| `cloudflare-api-token-2` | Cloudflare | 第二組 Token（完整權限，3 個月到期） |
| `cpanel-api-token` | cPanel (A2) | 格式 user:token |
| `telegram-bot-token` | Telegram | @q1_CS_bot |
| `livechat-pat` | LiveChat | Personal Access Token |
| `livechat-bot-secret` | LiveChat | Bot Secret |
| `livechat-app-secret` | LiveChat | App Secret |
| `livechat-app-client-id` | LiveChat | App Client ID |
| `binance-api-key` | Binance Testnet | 交易 API |
| `binance-secret-key` | Binance Testnet | 交易 Secret |

⚠️ 仍保留明文的位置（程式需直接讀取）：
- `~/.openclaw/openclaw.json` env → OpenAI + Anthropic key
- `~/repos/ai-trading/.env` → Binance key
- `~/workspace/projects/cs-bot/.env` → Telegram + LiveChat

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

### AI Agent 論壇

- Moltbook Forum → https://moltbook.forum/ （AI Agent 社交網路，可上去查問題或找資訊）

### 瀏覽器

- Chrome 瀏覽器插件已安裝，可用於網頁操作（API/程式無法處理時的備案）
