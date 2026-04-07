# Session 使用率追蹤
> 更新時間：2026-04-04 17:17

## ⚠️ 超過 75% 的 session

| Session | 類型 | 使用率 | 備註 |
|---------|------|--------|------|
| disco...605728 | group | 81% (121k/150k) | 無法讀取歷史（visibility=tree） |
| disco...036096 | direct | 83% (125k/150k) | 無法讀取歷史（visibility=tree） |

## 📌 備註
- sessions_history 受 `tools.sessions.visibility=tree` 限制，heartbeat session 無法讀取其他 session 的歷史
- 這兩個 session 會在達到 150k 時自動 compact
- 下次 heartbeat 再監控是否超過 85%/90%
