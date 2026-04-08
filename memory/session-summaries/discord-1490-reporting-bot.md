# Session Summary: #回報機器人 (1490873797358911631)
*Updated: 2026-04-08 14:44*

## 狀態
- **PERA57 回報機器人 4/7 日報已成功發送**（Signal + Telegram）
- 帳號 `peraaitest` 密碼更新為 `Aa654321@`（舊密碼被停用）

## 關鍵技術突破
1. **API 攔截法**：登入後首頁自動載入 30 天 dashboard-summary + dashboard-trend API 數據，從中提取指定日期的數據
2. **不需直接呼叫 API**：直接 API 呼叫需要特殊 auth header（`authorization: <token>` 不帶 Bearer + `merchantcode` + `environment` 等），用 `page.evaluate` + XHR 會被 `INVALID_TOKEN` 擋住
3. **正確做法**：用 Puppeteer 攔截 `page.on('response')` 自動拿到數據，無需手動呼叫 API

## 4/7 日報數據
- 新用戶：40, 活躍：1,673, 首充：14, 注充率：35%
- 存款人數：218, 金額：150,451
- 取款金額：157,972, 存提差額：-7,521
- 投注人數：379, 總投注：1,527,650.77, 派彩：1,463,561.01
- 遊戲盈利：64,089.76, 毛利：51,255.89, 紅利：7,778.31

## 待完成
- [ ] 缺少「總存款次數」和「投注筆數」（需從其他頁面抓取）
- [ ] 設定 cron 自動定時回報
- [ ] 整理成正式的 production script
- [ ] CS-Bot query module trigger 定義

## 檔案位置
- VPS: `/opt/pera57-reporting-bot/final-report3.js`（可用的報告腳本）
- `.env` 密碼已更新為 `Aa654321@`
- 後台 API: `ods-v2-dashboard-summary`, `ods-v2-dashboard-trend`（30 天範圍內的每日數據）
