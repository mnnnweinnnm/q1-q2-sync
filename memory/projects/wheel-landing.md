# 輪盤落地頁（wheel.gameistan-promo.com）

## 基本資訊
- **建立日期**：2026-04-09
- **用途**：推廣引流頁，透過輪盤遊戲吸引客戶至 PERA57 註冊
- **域名**：wheel.gameistan-promo.com（GoDaddy，NS 已指向 Cloudflare）
- **部署位置**：VPS 128.199.249.195，/var/www/wheel.gameistan-promo/
- **技術**：純靜態 HTML/CSS/JS（無後端）

## 功能現況
- ✅ 6 秒輪盤旋轉動畫
- ✅ 7 個獎項（再轉一次、100、16、5000、1、9、77）
- ✅ 保證中獎（首次必中非「再轉一次」獎項）
- ✅ 噴金幣 + 彩色紙片特效
- ✅ 旋轉/滴答/中獎音效（Web Audio API，無外部檔案）
- ✅ 奢華賭場風配色（深綠 #19311e + 金 #ffb300 + 亮綠 #00b970）
- ✅ 響應式設計（支援手機/桌面）
- ✅ 註冊按鈕跳轉 pera57.live/register

## 待完善
- ❌ 無防刷機制（前端決定獎項，可繞過）
- ❌ 無電話收集
- ❌ 無會員對接
- ❌ 無後台統計

## 檔案位置
- 本地：`~/.openclaw/workspace/wheel-landing/index.html`
- VPS：`/var/www/wheel.gameistan-promo/index.html`

## DNS 設定
- GoDaddy A 記錄：`wheel.gameistan-promo.com` → `128.199.249.195`

## Caddy 配置（VPS）
```
wheel.gameistan-promo.com {
    root * /var/www/wheel.gameistan-promo
    file_server
    encode gzip
}
```

## 後續優化方向
1. **防刷**：IP 限制 + Cookie 記錄
2. **收集**：電話號碼收集 + SMS 發送
3. **對接**：與 PERA57 會員系統整合
4. **統計**：後台查看參與/轉化數據

## 完整平台擴充計畫
- **檔案**：`memory/projects/gameistan-platform.md`
- **狀態**：規劃中（2026-04-09 討論）
- **規模**：會員系統、金流、推薦、後台管理
- **預估時程**：3-5 個月開發
