# HEARTBEAT.md

## 每日提醒事項

- [ ] **Tailscale 安裝 (Q1 及 Q2)**：建立跨網域私有連線 (今日 10:00 提醒已過期，需重新安排)
- [ ] **模型配置確認**：Gemini Flash ($3.50/M) 比 DeepSeek Reasoner ($0.70/M) 貴5倍，但支援圖片；維持目前配置合理
- [ ] **主機安全修補**：重設 usdt-bet.com 管理員密碼、加 uploads/.htaccess、更改 FTP 弱密碼
- [ ] **Google Sheets OAuth**：等待韋瀚點授權連結完成請款單自動化

### Token 異常消耗監控 🚨
- 檢查近 2 小時內是否有異常大量 token 消耗（>50k tokens/hour）
- 若發現異常：立即通知韋瀚，並停止可能的問題作業
- 常見原因：edit 工具失敗循環、全文翻譯作業、無限重試

### Anthropic OAuth Token 健康檢查
- 檢查 `openclaw models status` 的 anthropic 區塊
- 若 `usageStats.anthropic:default.errorCount` 持續增加（非額度問題），或出現 401/403 auth 錯誤
- **通知韋瀚**：OAuth token 可能完全失效，建議清掉 profile 改用 env API key

## Puppeteer 殭屍清理（每次 heartbeat 執行）

- [ ] 檢查超過 30 分鐘的 `Chrome for Testing` 進程
- [ ] 有的話直接 `kill`，並記錄殺了幾個
- 指令：`ps -eo pid,etime,comm | grep 'Chrome for Testing' | awk '$2 ~ /[0-9]+-/ || ($2 ~ /:/ && split($2,a,":") && (a[1]*60+a[2]) > 1800) {print $1}' | xargs kill -9 2>/dev/null`

## Session 滿載檢查（每次 heartbeat 執行）

- [ ] 執行 `openclaw status` 查看所有 session token 使用率
- [ ] 如果任何 session **首次超過 75%**：建立初始摘要
- [ ] 如果任何 session **超過 90%**：優先處理，簡單記錄到日誌即可
- [ ] **避免頻繁更新摘要檔案**，除非 session 內容有重大變化
- [ ] 清理超過 7 天的舊摘要（移到 memory/archive/ 或刪除）
- [ ] 真正重要的決策/待辦應同時寫入 memory/daily/ 或 MEMORY.md

## 每週一次：記憶整理（週一 heartbeat 時執行）

- [ ] 檢查 `memory/daily/` 超過 7 天的檔案
- [ ] 將重要內容整合進 `memory/topics/` 或 `memory/projects/`
- [ ] 精煉後更新 `MEMORY.md`
- [ ] 已整合的舊日誌移到 `memory/archive/`
