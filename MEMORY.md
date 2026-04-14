# MEMORY.md — 長期記憶

> 精煉過的重要資訊，每月更新。日常細節在 memory/daily/。

## 韋瀚的專案

### AI Trading（網格交易）
- 啟動日：2026-04-01
- 環境：Binance Spot Testnet
- 策略：網格交易 BTCUSDT，每 5 分鐘自動執行（macOS launchd）
- 初始資金：10,000 USDT + 1 BTC @ 68,553.78
- 損益查詢：`cd ~/repos/ai-trading && npx tsx scripts/check-pnl.ts`
- 每日 heartbeat 回報損益到 Discord #trading

### CS-Bot（AI 客服）
- 架構：LiveChat PAT 輪詢 → Node.js 中間層 → AI / 轉接真人
- Telegram Bot：@q1_CS_bot（token: `8763022530:AAGs...E40`）
- VPS：128.199.249.195（Docker cs-bot + Caddy + tg-webhook.py）
- Telegram Webhook：`https://tg.pera-57-ph.com/webhook`（Caddy 自動 HTTPS）
- TG groups: CSR `-1002143634379`（觀察學習）, Test `-5205814380`（升級通知）
- TCG API：merchantCode `pera57f3`，relay 待開通
- 狀態：**AI 客服已上線運作**（Group 11 測試中），webhook 2026-04-06 架設完成
- 待辦：TCG relay 開通、BO Puppeteer、6 類新查詢、粗口 fuzzy match

### PERA57 回報機器人（營運數據自動回報）
- **目標**：定時回報 PERA57 財務數據、網站狀態、入金成功率，異常即時警報
- **建立日期**：2026-04-07
- **架構**：數據收集層（API / Puppeteer 爬蟲）→ 處理分析層 → 通知推送層
- **技術棧**：Node.js + Express + node-cron + Puppeteer + Docker
- **VPS 部署**：`/opt/pera57-reporting-bot/` port 3001（待部署）
- **通知管道**：Signal（主要）、Telegram（備用，已設定）、Discord（輔助）
- **Telegram Bot**：@pera57_report_bot（token: 8764224944:AAHNTxmGsfCr77ct_wHaOWApg1Pe6q3vR2Q）
- **Telegram 群組**：P57回報機器人開發測試（ID: -5160138768）
- **排程**：每小時回報（:00）、每日匯總（09:00）、網站監控（每 15 分鐘）
- **警報門檻**：入金成功率 <85%、網站回應 >5s、錯誤率 >5%
- **Signal API**：已部署 VPS `/opt/signal-api/` port 8081 ✅
- **Signal 號碼**：+527711647956（墨西哥），🔴 卡在 Captcha 驗證（需韋瀚手動完成）
- **VPS 架構**：CS-Bot(:3000) / Reporting-Bot(:3001) / Signal API(:8081) 完全隔離
- **狀態**：✅ 程式架構完成 → 🔴 等 Signal 註冊 → 待部署到 VPS → 待連接 PERA57 網站 → 待系統測試
- **詳細記錄**：`memory/projects/pera57-reporting-bot.md`

### Wheel Arcade（輪盤產品）
- 建立日：2026-04-12
- 目錄：`~/.openclaw/workspace/wheel-arcade/`
- 技術：Node.js + Express + PostgreSQL 16 (Docker) + Canvas
- 狀態：**P0+P1 完成**，PostgreSQL 遷移完成（原 SQLite）
- 線上：https://arcade.gameistan-promo.com/
- 後台：https://arcade.gameistan-promo.com/admin/
- VPS：128.199.249.195 (DO SGP1)，Caddy + systemd
- 下一步：Docker 容器化 → Hetzner SGP → Cloudflare → P2 支付
- 詳細記錄：`memory/projects/wheel-arcade.md`

### 網頁主機管理
- Cloudflare 183 zone 整理完、後門已清、Origin Cert 已簽
- 🔴 usdt-bet.com 管理員密碼未重設、FTP 密碼太弱
- 🔴 uploads 目錄需加 .htaccess 禁 PHP 執行
- 🟡 addon domain 閒置清理（inode 95%）
- 🟡 主機遷移方案：Hetzner + Vultr 已規劃，待拍板

## 系統設定

- Mac Mini M4，24 小時運行
- OpenClaw 2026.4.2，Gateway 跑在 127.0.0.1:18789
- Discord bot @Q1 運作中
- contextTokens 上限：150k（2026-04-02 設定）
- 記憶架構：daily → topics/projects → MEMORY.md → archive（每週整理）
- 模型 fallback：Opus 4-6 → GPT-5 → Sonnet 4
- Anthropic 驗證：OAuth token（優先）+ env API key（fallback）
- OpenAI API key：存在 openclaw.json env block + Keychain 備份
- TTS：voice-message skill（edge-tts, zh-TW-HsiaoChenNeural）
- STT：gpt-4o-transcribe（$0.006/min）
- Q1 頭像：寫實仿生人風格（q1-avatar.png）
- FileVault 已開啟

## 偏好與習慣

- 繁體中文、簡潔溝通
- 非程式背景，技術名詞需白話解釋
- 偏好直接給方案，不要問太多確認
- **工具優先順序：API / 程式處理 → Chrome 網頁操作**（能用 API 就不開瀏覽器）

## 教訓

- Session context 過大會導致 API timeout，設 contextTokens 控制
- 重要討論結束後要立即寫 memory，不要等 heartbeat
- memory 日誌要寫在 daily/ 不是根目錄
- Session 超過 75% 時自動摘要存檔，新 session 啟動時讀取摘要以維持脈絡連續性
- 不要隨便停 gateway — 等於切斷通訊
- Gateway restart 連續觸發會卡在 draining 狀態
- 修改 launchd plist 會被 `openclaw gateway install` 覆寫
- **任何修改 config/檔案前先問韋瀚確認**（不要自行動作）
- 韋瀚從事博彩業，有法律風險顧慮，敏感資料盡量不存明文

## Discord 頻道

- #一般 (1488407779331801130)
- #系統設定 (1488436839814795366)
- #每日靈感 (1488456178882314290)
- 韋瀚 Discord ID: 515130023532036096

---
## 安全警示

- 2026-04-05: Ars Technica 報導 OpenClaw 未驗證遠端 admin 漏洞，建議假設已被入侵、輪換憑證
- 已通知韋瀚，待他決定要不要執行輪換

*最後更新：2026-04-07*

## Promoted From Short-Term Memory (2026-04-09)

<!-- openclaw-memory-promotion:memory:memory/2026-04-04.md:226:240 -->
- - 明文敏感檔案：openclaw.json（API keys）、cs-bot/.env、memory/、main.sqlite - 重要習慣：關機 > 睡眠（睡眠狀態金鑰在記憶體） - 狀態：韋瀚考慮中，尚未安裝 VeraCrypt ## CS-Bot 重大突破（17:00-17:35） - **Bot 成功回覆客戶訊息！** 韋瀚確認收到 bot 回覆 - 修復的 bug 清單： 1. **Bearer auth**：從 Basic（owner PAT）改為 Bearer（bot JWT token），解決 403 "Requester is not user of the chat" 2. **事件偵測**：`list_chats` 的 `last_thread_summary.last_event_id` 在 v3.6 永遠是 null，改用 `last_event_per_type` 取最新 event ID 3. **get_chat 回傳格式**：v3.6 回傳 `thread`（單數）不是 `threads`（複數），導致取不到 events → bot 偵測到新訊息但讀不到內容 4. **Token 互搶**：每次 `issue_bot_token` 會讓舊 token 失效，加了 apiCall wrapper 遇 401 自動 refresh 5. **客戶名稱**：sync 時沒抓 customer name，fallback 到 "Customer"。加了 `setCustomerName` 在 sync 時設定 6. **摘要 prompt**：原本強制輸出「水單：已提供/未提供」，改成只輸出對話中實際提到的欄位 7. **轉接目標**：LiveChat transfer 從 Group 0 改 Group 11（測試期間），Telegram 通知改發到 PERA CSR Test 群 - 部署檔案：livechat-v3.js、poller-v3.js、handler-v2.js → VPS `/opt/cs-bot/src/` [score=0.808 recalls=10 avg=0.449 source=memory/2026-04-04.md:226-240]

## Promoted From Short-Term Memory (2026-04-09)

<!-- openclaw-memory-promotion:memory:memory/2026-04-04.md:238:256 -->
- 6. **摘要 prompt**：原本強制輸出「水單：已提供/未提供」，改成只輸出對話中實際提到的欄位 7. **轉接目標**：LiveChat transfer 從 Group 0 改 Group 11（測試期間），Telegram 通知改發到 PERA CSR Test 群 - 部署檔案：livechat-v3.js、poller-v3.js、handler-v2.js → VPS `/opt/cs-bot/src/` - **現在 E2E 流程可用**：客戶開 chat → 填表 → bot 歡迎 → 客戶發訊息 → AI 回覆 → 需要時 escalate 到 Telegram test 群 - 韋瀚正在持續測試中 ## CS-Bot 路由修復 & 全面 Review（18:00-19:30） - **核心 bug**：所有 chat（包括用 `direct.lc.chat/15555654/11` 進入的）都被路由到 Group 0，而非 Group 11 - 原因：`continuous_chat=1` 和 `chat_between_groups=1` 設定讓有歷史的客戶繼續分配到上次的 group（Group 0） - 新客戶（無歷史）正確進 Group 11；有歷史的全部到 Group 0 - **修復動作**： 1. 關閉 `continuous_chat` 和 `chat_between_groups`（LiveChat API v2） 2. Poller 加 botMember 過濾 — 只處理 bot 是成員的 chat，避免處理 Group 0 的 chat 導致 403 3. Bot 啟動時自動設 routing status = accepting_chats - **完整 Review 結果（19:13）**： - ✅ Docker 運行正常、health check OK - ✅ Bot token 自動簽發 + 401 自動刷新 - ✅ Group 11 設定正確（PERA57 bot = first priority） - ✅ Group 11 Greetings 文字都是 PERA57 GAMING [score=0.802 recalls=7 avg=0.423 source=memory/2026-04-04.md:238-256]
