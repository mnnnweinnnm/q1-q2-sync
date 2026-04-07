# CS-Bot（AI 客服）專案記錄

## 架構
- LiveChat Webhook → Node.js 中間層 → AI / 轉接真人
- **最終採 PAT 輪詢**（LiveChat Dev Platform UI 壞了，Webhook 走不通）

## 環境
- Telegram Bot: @q1_CS_bot
- CSR 群組: PERA57 CSR (chat_id: -1002143634379)
- VPS: 128.199.249.195 (DigitalOcean SGP1, Docker)
- Container: `pera57-cs-bot`，port 3000
- Health check: http://128.199.249.195:3000/health

## 重要憑證位置
- Telegram Bot token: `projects/cs-bot/.env`
- LiveChat Bearer: 瀏覽器 XHR 擷取（us-south1:FuSiP...）
- OAuth client: `f6c49ee2be0a4175df750e4d0988a765`
- Bot Agent ID: `aa5745c3c761782c321dcbca4a647693`

## VPS 檔案結構（/opt/cs-bot/）
- src/index.js — Express server + polling
- src/poller.js — LiveChat polling（每 5 秒）
- src/handler.js — 處理新聊天/訊息
- src/livechat.js — LiveChat API
- src/telegram.js — Telegram 通知
- src/logger.js — JSON log

---

## Changelog

### v1.0 — 初始部署（2026-04-01）
- Express server + LiveChat polling 架構
- AI 回覆（OpenAI）+ Telegram 轉接
- 基本粗口過濾

### v1.1 — 核心 Bug 修復（2026-04-04 17:00）
- **fix**: Bearer auth — 從 Basic（owner PAT）改為 Bearer（bot JWT token），解決 403
- **fix**: 事件偵測 — `last_event_id` 在 v3.6 永遠 null，改用 `last_event_per_type`
- **fix**: get_chat 回傳格式 — v3.6 回傳 `thread`（單數）非 `threads`
- **fix**: Token 互搶 — 加 apiCall wrapper 遇 401 自動 refresh
- **fix**: 客戶名稱 — sync 時抓 customer name，fallback "Customer"
- **fix**: 摘要 prompt — 只輸出對話中實際提到的欄位
- **fix**: 轉接目標 — Group 0 改 Group 11（測試期間）
- 部署檔案：livechat-v3.js、poller-v3.js、handler-v2.js
- **E2E 流程首次跑通** ✅

### v1.2 — 路由修復（2026-04-04 18:00）
- **fix**: 所有 chat 被路由到 Group 0 — 關閉 `continuous_chat` 和 `chat_between_groups`
- **fix**: Poller 加 botMember 過濾，只處理 bot 是成員的 chat
- **feat**: Bot 啟動時自動設 routing status = accepting_chats
- Review 通過：Docker OK、Token OK、Group 11 OK、Telegram OK、Knowledge base 8 檔 OK

### 已知問題
- ⚠️ 粗口過濾只用 exact match，拼錯的（fuxk、fck）可繞過
- ⚠️ 舊 chat 留在 Group 0，bot 無法處理

### 待辦
- [ ] 加強粗口過濾（fuzzy match）
- [ ] 重設 usdt-bet.com 管理員密碼
- [ ] 正式上線前壓力測試

---
*最後更新：2026-04-06*
