# Q2 Webchat Session 摘要
> 更新：2026-04-09

## 本次 Session 重要事項

### 已完成
- Mac sleep 設定：電池 + 插電模式皆 `sleep 0`（螢幕打開不休眠，闔上才休眠）
- Q2 模型配置：primary `minimax/MiniMax-M2.5`，fallbacks: deepseek-reasoner → gemini-3.1-pro → gpt-5
- Q2 MiniMax API 串接完成：baseUrl `https://api.minimax.io/v1`，模型 MiniMax-M2.5 / M2.7
- Q2 Discord Bot 頭像更新成功（API PATCH）
- Q2 avatar 檔案複製到 workspace（修復破圖）
- Q1 Gateway 修復：PATH 問題（/usr/local/bin 不在 PATH）、重建 openclaw symlink
- Q1 模型修復：移除失敗的 anthropic 回退，改為 gemini-3.1-pro + gpt-5
- Q1 MiniMax API 測試正常（api.minimaxi.chat 和 api.minimax.io 都能用）
- 製作 LLM 模型比較表（2026年4月，含 Claude Opus/Sonnet、Gemini 2.5 Pro、DeepSeek-V3.2、GPT-4o）

### 待處理
- Q1 MiniMax 配置修正（模型 ID 大小寫：minimax-m2.5 → MiniMax-M2.5，baseUrl 可更新為 api.minimax.io）
- Q1 SSH 目前連不上（192.168.68.107 不通），需韋瀚確認 Mac Mini 狀態

### 關鍵發現
- MiniMax 官方 API baseUrl：`https://api.minimax.io/v1`（新）或 `https://api.minimaxi.chat/v1`（舊，仍可用）
- 模型 ID 需大小寫正確：`MiniMax-M2.5`（非 `minimax-m2.5`）
- MiniMax 最新模型：M2.7（204,800 context window）
- Q1 反應慢根因：Anthropic API 額度不足 → 每次回退都先失敗再切換
