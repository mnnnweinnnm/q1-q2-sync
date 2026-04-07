# Discord DM (韋瀚) 摘要
> Session: agent:main:discord:direct:515130023532036096 | 83% | 2026-04-04

## 重點
- 韋瀚問了 Vercel 是什麼（已白話解釋）
- 檢查系統狀態：Gateway 正常、Discord 已連線、19 sessions
- 發現 #ai客服 頻道被切到 GPT-5（fallback 觸發後沒自動回來）
- 模型配置調整：先改 Sonnet+Opus → 後改回 Opus→GPT-5→Sonnet（方案 B）
- Gateway 多次重啟（plist 修改 + 模型切換導致）

## 最終模型配置
- 主力：Opus 4-6
- 第一備援：GPT-5
- 第二備援：Sonnet

*更新：2026-04-04 18:45*
