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
- **通知管道**：Signal（主要）、Telegram（備用）、Discord（輔助）
- **排程**：每小時回報（:00）、每日匯總（09:00）、網站監控（每 15 分鐘）
- **警報門檻**：入金成功率 <85%、網站回應 >5s、錯誤率 >5%
- **Signal API**：已部署 VPS `/opt/signal-api/` port 8081 ✅
- **Signal 號碼**：+527711647956（墨西哥），🔴 卡在 Captcha 驗證（需韋瀚手動完成）
- **VPS 架構**：CS-Bot(:3000) / Reporting-Bot(:3001) / Signal API(:8081) 完全隔離
- **狀態**：✅ 程式架構完成 → 🔴 等 Signal 註冊 → 待部署到 VPS → 待連接 PERA57 網站 → 待系統測試
- **詳細記錄**：`memory/projects/pera57-reporting-bot.md`

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
