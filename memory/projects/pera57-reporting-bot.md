# PERA57 回報機器人專案記錄

## 專案概述
- **目標**：為 PERA57 博彩站點建立營運數據自動回報系統
- **建立日期**：2026-04-07
- **版本**：v1.0
- **狀態**：✅ 架構完成，待測試部署

## 功能規格

### 核心功能
1. **📊 每小時財務數據回報**
   - 總入金、總出金、淨利潤
   - 活躍用戶數、遊戲數據
   - 入金成功率計算

2. **🌐 網站狀態監控**
   - 多電信商連線測試
   - 回應時間監控
   - 可用性檢查

3. **💰 入金成功率追蹤**
   - 實時成功率計算
   - 異常門檻警報（<85%）
   - 支付方式分析

4. **🚨 異常情況警報**
   - 即時異常檢測
   - 多層級警報（normal/high）
   - 自動重試機制

### 通知管道
- ✅ **Signal**：主要通知管道（PERA57 優先）
- ✅ **Telegram**：備用通知（已設定）
- 🔄 **Discord**：輔助通知（可選）

### Telegram 設定
- **Bot**：@pera57_report_bot
- **Token**：8764224944:AAHNTxmGsfCr77ct_wHaOWApg1Pe6q3vR2Q
- **群組**：P57回報機器人開發測試
- **群組 ID**：-5160138768
- **狀態**：✅ 測試成功，可發送財務報告

### 資料來源策略
- **優先**：API 呼叫（如果可用）
- **備用**：Chrome 網頁爬蟲（Puppeteer）
- **容錯**：模擬資料（測試模式）

## 技術架構

### 系統架構
```
數據收集層 → 處理分析層 → 通知推送層
   ↓           ↓           ↓
• API 呼叫    • 數據處理    • Telegram
• Chrome爬蟲  • 異常檢測    • Signal  
• 網站監控    • 報表生成    • Discord
```

### 技術棧
- **後端**：Node.js 18+, Express
- **任務排程**：node-cron
- **網頁爬蟲**：Puppeteer + Chrome
- **日誌**：Winston
- **容器化**：Docker + Docker Compose
- **部署**：VPS (128.199.249.195)

### 檔案結構
```
pera57-reporting-bot/
├── src/
│   ├── index.js                    # 主程式入口
│   ├── collectors/DataCollector.js # 數據收集器
│   ├── reports/ReportGenerator.js  # 報表生成器
│   ├── notifications/NotificationService.js # 通知服務
│   ├── utils/
│   │   ├── ConfigManager.js        # 設定管理
│   │   └── logger.js              # 日誌工具
│   └── test.js                    # 測試套件
├── scripts/
│   ├── deploy.sh                  # 部署腳本
│   └── test-local.sh             # 本地測試
├── docs/SETUP.md                  # 設置指南
├── docker-compose.yml             # Docker 編排
├── Dockerfile                     # 容器建置
└── .env.example                   # 環境變數範例
```

## 排程設定

### Cron 任務
- **每小時回報**：`0 * * * *`（每小時 0 分）
- **每日匯總**：`0 9 * * *`（每天 9:00）
- **網站監控**：`*/15 * * * *`（每 15 分鐘）

### 健康檢查
- **端點**：`:3001/health`
- **間隔**：30 秒
- **逾時**：10 秒

## 設定參數

### 警報門檻
```javascript
alerts: {
    depositSuccessRateThreshold: 85,      // 入金成功率 85%
    websiteResponseTimeThreshold: 5000,   // 網站回應 5 秒
    errorRateThreshold: 5,                // 錯誤率 5%
    minimumHourlyDeposits: 10             // 最少入金 10 筆/小時
}
```

### 監控設定
```javascript
monitoring: {
    websiteCheck: {
        interval: 15,        // 15 分鐘間隔
        timeout: 10000,      // 10 秒逾時
        retries: 3           // 重試 3 次
    }
}
```

## 部署環境

### 開發環境
- **位置**：Mac Mini M4 本機
- **用途**：程式開發、本地測試
- **指令**：`npm run dev`

### 生產環境
- **VPS**：128.199.249.195 (DigitalOcean SGP1)
- **容器**：Docker + Docker Compose
- **監控**：健康檢查 + 日誌
- **部署**：`./scripts/deploy.sh`

## 安全考量

### 敏感資料保護
- 🔒 API keys 存於環境變數
- 🔒 管理後台帳密加密存儲
- 🔒 通信使用 HTTPS
- 🔒 容器非 root 使用者執行

### 風險管控
- ⚠️ 博彩業法律風險敏感
- ⚠️ 避免敏感資料明文存儲
- ⚠️ 爬蟲頻率避免被封鎖
- ⚠️ API Rate Limit 管理

## 測試策略

### 測試覆蓋
- ✅ 設定檔驗證
- ✅ 數據收集模擬
- ✅ 報表生成測試
- ✅ 通知格式化測試
- ✅ 健康檢查端點
- ✅ Logger 功能測試

### 測試指令
```bash
npm test              # 執行所有測試
npm run test-local    # 本地完整測試
npm run health        # 健康檢查
```

## Signal API 整合 ✅

### 採用方案：signal-cli-rest-api
- **Docker 容器**：bbernhard/signal-cli-rest-api:latest
- **API 端點**：http://128.199.249.195:8080
- **部署位置**：VPS /opt/signal-api/
- **安全設定**：僅本機訪問 (127.0.0.1:8080)

### 整合功能
- ✅ Signal 群組訊息發送
- ✅ 健康檢查和連接測試
- ✅ 自動重試機制
- ✅ 完整測試腳本

### 部署工具
- `scripts/setup-signal-api.sh` - Signal API 部署腳本
- `scripts/test-signal.js` - 完整測試腳本
- `docs/SIGNAL_SETUP.md` - 詳細設置指南

## 後續規劃

### v1.1 計劃功能
- [ ] 歷史數據對比分析
- [ ] 更豐富的圖表視覺化
- [ ] WhatsApp 通知支援
- [ ] 多站點支援框架

### v2.0 長期目標
- [ ] 機器學習異常檢測
- [ ] 預測性分析報告
- [ ] 自動化運營建議
- [ ] 更多博彩站點接入

## 重要提醒

### 部署前檢查
1. ✅ 複製 `.env.example` 為 `.env`
2. ✅ 設定所有必要的環境變數
3. ✅ 測試 Telegram Bot Token
4. ✅ 驗證 PERA57 管理後台帳密
5. ✅ 執行本地測試確認無誤

### 維護要點
- 📝 定期檢查日誌檔案
- 🔄 監控 Chrome 進程避免殭屍進程
- 🔧 定期輪換 API Token
- 📊 檢視回報資料準確性

---
*建立者：Q1*
*建立日期：2026-04-07*
*最後更新：2026-04-07*