# 2026-04-04~05 · Main Session (webchat/heartbeat) 摘要

- 狀態：context 120k/150k（80%）

## 近期完成
- OpenAI API key 收斂到 openclaw.json `env` 單一處 + Keychain 備份（providers/plist 明文已移除）
- 模型 fallback 方案 B：Opus 4-6 → GPT-5 → Sonnet 4
- STT：gpt-4o-transcribe（Discord 語音可辨識，約 $0.006/分鐘）
- TTS：voice-message skill（edge-tts + ffmpeg），台灣語音 `zh-TW-HsiaoChenNeural`
- Q1 頭像更新為寫實版仿生人風格（`~/.openclaw/workspace/q1-avatar.png`）
  - Discord bot 頭像已換
  - Webchat 透過 `openclaw agents set-identity --avatar q1-avatar.png` 已換
- 記憶架構維持現狀，2-3 週後再評估

## 韋瀚的資安需求
- 從事博彩業，擔心電腦被搜查
- FileVault 已開；機器 24h 運行不能關機
- VeraCrypt 不適合（加密磁區不能一直掛載）
- 改善方向：Keychain 存 API keys、螢幕鎖快捷鍵 `Ctrl+Cmd+Q`、短自動鎖定時間

## 待辦（延續）
- [ ] cs-bot/.env 的 token 可考慮移進 Keychain
- [ ] 瀏覽器/iCloud 敏感資料檢查（韋瀚按原排程，先不做）
- [ ] Discord #trading 自動回報 PnL 尚未設定
- [ ] 同事伺服器 Bot 邀請
- [ ] Binance Testnet 領測試幣

## 教訓
- gateway restart 連續觸發會讓 Gateway 卡在 draining 狀態
- 修改 launchd plist 會被 `openclaw gateway install` 覆寫，不要自改 plist
