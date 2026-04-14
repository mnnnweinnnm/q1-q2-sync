# Discord #網頁製作 Session 摘要
**更新時間：2026-04-06 12:51 (77% token usage)**

## 進行中的任務

### barya.online 頁面優化
- ✅ meta tags 已優化（SEO + OG + Twitter Card + PWA manifest）
- ✅ og-cover.png 已上傳修復（cPanel file_put_contents 有 open_basedir 限制，需用 `/tmp` → `copy()` workaround）
- ✅ barya.online 已加到 Cloudflare（zone: `f19dfd93998dea44ada9de7a34edccd4`）
- ✅ GoDaddy NS 已切到 CF（irena/owen.ns.cloudflare.com）
- ✅ CF 設定已套用（Full SSL, HTTPS, TLS 1.2, Brotli, Early Hints, Rocket Loader）
- ⏳ 使用者正在 opengraph.xyz 驗證 meta tags 效果
- 📌 下一步：可能加 GA4 流量追蹤

### Cloudflare 大整理（已完成）
- ✅ 183 個 zone 效能+安全設定批量套用（0 fail）
- ✅ 8 個 Paused zone 全部 unpause
- ✅ 26 個 A2 主機域名切 Full (strict) — 含 16 張 Origin Cert（15 年）
- ✅ mini88bet.com index.php 補回
- ✅ mini888ph.xyz 526 修復（改 Full 非 strict）

### WSAL 殘留清理（已完成）
- ✅ 33 個 unique docroot 全部掃完
- ✅ usdt-bet.com (28 opts) + pera57.live (16 opts) + 7bet-casino.com (16 opts) 清除
- ✅ 其他站無 WSAL 殘留

### mini888ph.xyz（已完成）
- ✅ CF zone active，網站 200 OK
- ✅ 動態 WP_HOME/WP_SITEURL 已注入 wp-config.php

## 重要密鑰
- **新 CF Token（完整權限）**：`[見 Keychain: cloudflare-api-token-2]`
- **barya.online zone**：`f19dfd93998dea44ada9de7a34edccd4`
- **mini888ph.xyz zone**：`24bfa25439d215aa0747dda44beea687`
- **cPanel**：sg1-ts5.a2hosting.com:2083 / shane042 / [見 Keychain: cpanel-api-token]
- **主 IP**：67.209.125.243
- **GoDaddy API**：Key=[見 Keychain: godaddy-api-key] / Secret=[見 Keychain: godaddy-api-secret]

## 使用者偏好
- 安全設定優先考慮網頁效能（SEO 站為主）
- 不開 Bot Fight Mode / High Security（擋爬蟲）
- 想要最省事的方案
- 非技術背景，需白話解釋
- barya.online landing page 位於 `~/.openclaw/workspace/sites/barya-landing/`

## 已知問題 / 注意事項
- cPanel `file_put_contents()` 對已存在的二進制檔有問題（open_basedir），需用 `/tmp` + `copy()` workaround
- cPanel `save_file_content` API 不能正確處理二進制（base64 upload → PHP decode 為正確流程）
- CF SSL Mode 不能統一切 strict（共用 docroot 的跳板域名需用 Full 非 strict）
- okebet-casino.com 有 301 redirect 到 okebet.live（CF 層或 WP 層設定）
- inode：567,015 / 600,000（剩 32,985）

## 待辦
- [ ] barya.online 加 GA4 流量追蹤（等使用者提供 Measurement ID 或開新帳號）
- [ ] SOP 已存 `memory/topics/domain-blocked-playbook.md`
- [ ] 主機遷移計劃已存 `memory/topics/hosting-migration-plan.md`
- [ ] CF Token 三個月後到期要提醒
