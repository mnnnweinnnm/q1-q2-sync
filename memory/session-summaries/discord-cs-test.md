2026-04-08 · Discord #cs-test · 摘要（自動）

重點：
- 實作 LiveChat Prechat Form 資料提取：
  - 修復 `poller.js`，現在會解析 `filled_form` 事件，提取 `Username` 與 `Inquiry Type`
  - 修復 `handler.js`，將 Prechat 資料注入 AI 上下文，並讓歡迎訊息個人化
  - AI 現會優先使用 Prechat 填寫的 username，**不再重複詢問客戶 username**
- 知識庫與提示詞同步：
  - `brand-config.md` 語言規則全面改為「ALWAYS respond in ENGLISH」，解決與 `ai.js` 的衝突
  - 修正 `ai.js` 中 markdown (```) 導致的模板字串錯誤
- Transaction password 三段式流程修正：
  - 程式化回覆 Phase 1（索取四項資訊）現在也會優先從 Prechat 資料中提取 username
- 基礎建設修正：
  - VPS 的 `docker-compose.yml` 新增 `src/` 目錄的 volume mount，解決修改原始碼後未生效的問題
  - 修復 `handler.js` 裡 `chatAccumulatedFiles` 未宣告的 bug

待辦/決策：
- [ ] 觀察 Prechat 提取在斷線重連或異常事件序列中的穩定性
- [ ] 是否實作 LiveChat transfer_chat：Telegram 監控群回覆「/takeover」→ 轉接真人群組

測試建議：
- 開新無痕視窗進入對話，填寫 Prechat form 後發送「change transaction password」，確認機器人是否直接輸出四項資訊清單，而不重複詢問 username。