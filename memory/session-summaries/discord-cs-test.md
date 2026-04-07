2026-04-06 · Discord #cs-test · 摘要（自動）

重點：
- 修復 Telegram 409、更新 CSR 群為超級群 ID、Bot 提權、輪詢 3s、語言規則強化
- Prompt 重寫：
  - 一律用英文回覆（覆蓋先前「跟隨客語」規則）
  - 絕對禁止：提供/捏造任何個人帳戶充值；以存款作為重設密碼或支援條件；付款指引一律指向官網/APP
  - 先收 username 再升級；未收 username 不得說「請稍候」
- 新增安全防火牆（程式層）：偵測到疑似帳號/充值指示或「重設密碼需存款」→ 強制輸出 [ESCALATE] 英文等待訊息並升級
- 升級摘要修正：
  - 先升級再回覆，避免 AI 回覆混入摘要
  - 摘要只取客戶訊息；Summary Prompt 以中文結構化字段（問題/帳號/金額/方式/截圖/時間）
  - Telegram 通知模板結構化，附 Chat ID
- 升級超時保護：2/5/10 分鐘節點 → 催促 CSR / 客戶進度更新 / 最終提醒
- 觀察：LiveChat 後台目前無法直接接管 bot 持有的進行中對話（考慮 transfer_chat 流程）

待辦/決策：
- 是否實作 LiveChat transfer_chat：Telegram 監控群回覆「/takeover」→ 轉接真人群組（需確認目標 group）
- 知識庫 faq-deposit 補充「只走官方渠道、不提供私人帳號」聲明（選做）

測試建議：
- 「I forgot my password」→ 不會出現任何存款／帳號號碼要求
- 問「bank account / deposit to 123…」→ 直接觸發 [ESCALATE] 英文等待訊息，Telegram 摘要應清楚列出客戶問題
