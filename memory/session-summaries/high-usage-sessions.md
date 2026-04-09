# Session 使用率追蹤
> 更新時間：2026-04-09 12:51

## ⚠️ 超過 75% 的 session

| Session | 類型 | 使用率 | 模型 | 年齡 | 備註 |
|---------|------|--------|------|------|------|
| disco...576842 | group | 81% (121k/150k) | gemini-3.1-pro-preview | 16h | 無法讀取歷史（visibility=tree） |

## 📌 備註
- sessions_history 受 `tools.sessions.visibility=tree` 限制，heartbeat session 無法讀取其他 session 的歷史
- 這個 session 會在達到 150k 時自動 compact
- 下次 heartbeat 再監控是否超過 85%/90%
- 上次檢查（2026-04-04）的兩個高使用率 sessions 已 compact 或關閉
