# Wheel Arcade 專案記憶

## 概述
獨立 Arcade 風格輪盤產品，霓虹/像素美學（CRT 掃描線、8-bit 字體、深色背景＋發光元素）。
目標：菲律賓市場先行→多市場擴展，支援會員、支付、裂變功能。

## 關鍵狀態（2026-04-14）
- **P0+P1 已完成**：輪盤遊戲 + 會員系統 + VIP + 新手引擎 + 流水 + 完整後台
- **基礎設施**：已遷移至 PostgreSQL（Docker），準備 Docker 容器化 + 水平擴展
- **下一步**：Docker 化 → Hetzner 遷移 → Cloudflare → P2 支付

## 架構
- **前端**：原生 Canvas + Web Audio，無框架，build.js 生成 HTML
- **後端**：Node.js + Express，async/await，port 3010
- **資料庫**：PostgreSQL 16（Docker `wheel-arcade-pg`，127.0.0.1:5432）
- **Web Server**：Caddy（自動 HTTPS + `/api/*` 反向代理）
- **VPS**：DigitalOcean SGP1 128.199.249.195
- **域名**：`arcade.gameistan-promo.com`（開發用，GoDaddy DNS）

## 技術決策
1. Server-side RTP（前端只負責動畫）
2. SQLite → PostgreSQL（2026-04-14 遷移完成）
3. 餘額/獎券分離（Balance ↔ Tickets）
4. 新手引擎：3 階段充值保護（首充 108-110%，漸減至正常）
5. 1x 流水要求（以購券金額累計）
6. ticketPrice=2（PH 市場低門檻）
7. 小數獎品值（0.2, 0.5, 1.5 等）
8. Admin：帳號密碼登入 → Token（不再用 Key）

## 關鍵檔案
- 本地開發：`~/.openclaw/workspace/wheel-arcade/`
- Server 程式碼：`server/db.js`, `server/index.js`, `server/rtp-engine.js`, `server/newbie-engine.js`
- 前端建置：`build.js` → `dist/arcade/index.html`
- Admin 頁面：`dist/admin/` (index, members, history, settings, vip, audit)
- 部署腳本：`deploy.sh`

## 線上存取
- 遊戲：https://arcade.gameistan-promo.com/
- 簡報：https://arcade.gameistan-promo.com/pitch.html
- 後台：https://arcade.gameistan-promo.com/admin/

## 待辦
- [ ] Docker 容器化 API
- [ ] Hetzner SGP VPS（生產環境）
- [ ] Cloudflare DNS + proxy（IP 隱藏 + DDoS）
- [ ] 獨立生產域名
- [ ] P2 支付系統（GCash/Maya）
- [ ] P3 裂變系統（邀請碼 + 分享）
- [ ] OTP 驗證
- [ ] RTP 冷啟動優化（韋瀚考慮中）
- [ ] 券價切換機制（韋瀚考慮中）

## ⚠️ 上線前必做
- 更換 admin 密碼（目前 admin/admin2026）
- 管理後台 IP 白名單
- CORS 收緊（目前 `*`）
- Rate limiting
- 生產域名替換（不與其他專案共用）
- Cloudflare proxy 開啟（目前 DNS 曝露真實 IP）
