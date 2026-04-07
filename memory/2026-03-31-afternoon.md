# 2026-03-31 下午

## 初始設定
- 名字：Q1，風格：正經簡潔，emoji：😐
- 使用者：韋瀚，時區 Asia/Taipei，偏好繁體中文，非程式背景

## WhatsApp 設定與問題
- 串接號碼 +886926208099（韋瀚自己的號碼）
- 頻繁斷線，拖累整個 Gateway
- 17:00 已斷開連結

## Skills 安裝
- find-skills、remotion-best-practices、ai-image-generation（全域安裝）
- ai-image-generation 需要設定 infsh CLI + inference.sh 帳號（尚未設定）

## AI Trading
- 安裝 ai-trading skill（~/repos/ai-trading）
- 連接 Binance Testnet，API Key 已設定
- 回測均線交叉策略 BTCUSDT：3 週 +0.22%，勝率 45.5%
- 尚未啟動自動交易排程，等搬到 Mac Mini 再設定

## Discord 設定
- Bot 名稱 @Q1
- 韋瀚 Discord ID: 515130023532036096
- 開啟所有 Privileged Gateway Intents（Presence、Server Members、Message Content）
- 但狀態仍顯示 intents:content=limited

## 系統問題
- Gateway 今天頻繁掛掉，主要因為：
  1. 操作太密集（安裝 plugin、重啟多次）
  2. WhatsApp 連線不穩定
  3. Discord 權限改動後重啟
- 韋瀚多次遇到斷線，需要手動 `openclaw gateway restart` 恢復

## 對話規則變更
- 原本設定「對話結束回覆 1」，後來韋瀚要求移除此規則

## 待辦
- 搬到 Mac Mini 後：設定自動交易排程、重新連結 WhatsApp（如果需要）
- Telegram 尚未設定
- Moltbook Forum (https://moltbook.forum/) 已記在 TOOLS.md