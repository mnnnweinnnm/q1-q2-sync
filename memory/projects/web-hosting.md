# 網頁製作 & 主機管理 專案記錄

## 基礎設施
- **A2 Hosting**: sg1-ts5.a2hosting.com:2083，帳號 shane042
- **主 IP**: 67.209.125.243
- **Cloudflare**: 183 個 zones，Account ID: [見 Keychain: cloudflare-account-id]
- **GoDaddy**: 帳號有數百個網域，有 Production API Key
- **inode**: 567,015 / 600,000（95%）— 42 個 addon domain

## 認證資訊
- cPanel API token: `shane042:[見 Keychain: cpanel-api-token]`
- CF Token: `[見 Keychain: cloudflare-api-token-2]`（3 個月後到期要續）
- GoDaddy API Key: `[見 Keychain: godaddy-api-key]`
- FTP: `q1weihan@okebet-casino.com` / `asdf147852`（⚠️ 弱密碼待改）

---

## Changelog

### v1.0 — barya.online 落地頁上線（2026-04-05）
- **feat**: GoDaddy 買網域 → DNS 指向 A2 → cPanel addon domain
- **feat**: Barya bet 風格單頁落地頁（深色+金色、4 輪播、JILI/FC/PG logos）
- 原始碼：`~/.openclaw/workspace/sites/barya-landing/index.html`
- CTA：`https://www.barya-bet.com/m/home?affiliateCode=tiktok`
- AutoSSL 已啟動

### v1.1 — mini888ph.xyz 跳板域名（2026-04-05）
- **feat**: mini88bet.com 被菲律賓 ISP SNI/DPI 封鎖 → 換新域名繞過
- **feat**: mini888ph.xyz → CF proxy → 共用 mini88bet.com docroot
- **fix**: wp-config.php 注入動態 `$_SERVER['HTTP_HOST']` site URL
- SOP 存於 `memory/topics/domain-blocked-playbook.md`

### v1.2 — 主機安全掃描 & 後門清除（2026-04-05）
- **fix**: usdt-bet.com 發現 4 個 WP 後門（comment.php、content.php、prezet.php、gz payload）
- **fix**: 清除 DB 內 4 個惡意管理員（WordPress Support、bot、trumpweiss、admlnlx）
- **fix**: 輪換 wp-config.php 8 組 SALT KEYS
- **fix**: 刪除 7 個暴露的 wp-config.bak-a2.php
- **fix**: 停用 3 站壞掉的 WSAL 外掛
- **fix**: 清空 3 個 error_log

### v1.3 — Cloudflare 大整理（2026-04-05~06）
- **feat**: 183 zone 批量安全+效能設定（HTTPS、TLS 1.2、Brotli、Early Hints 等）
- **feat**: 8 個 Paused zone 恢復 Active
- **feat**: 26 個 zone 切 SSL Full (strict)，其中 16 個簽 CF Origin Cert（到期 2041）
- **fix**: mini88bet.com 403 — 補回被刪的 index.php
- **fix**: mini888ph.xyz 526 — 切回 Full（非 strict）
- **fix**: 7bet-casino.com WSAL 殘留 cron 清除（16 筆 options 刪除）

### v1.4 — cPanel inode 清理（2026-04-05）
- 清除 awstats/webalizer/analog 舊報表（4,445 檔、~1 GB）
- 清除 Roundcube 舊備份、過期 SSL 憑證
- inode: 571,497 → 567,015（-4,482）

---

## 已知問題
- ⚠️ inode 95% 接近上限，主因是 42 個 addon domain
- ⚠️ usdt-bet.com 6 位合法管理員密碼未重設
- ⚠️ uploads 目錄未加 .htaccess 禁 PHP 執行
- ⚠️ FTP 密碼太弱（asdf147852）
- ⚠️ CF Token 3 個月後到期

## 待辦
- [ ] 用戶確認哪些 addon domain 已停用 → 移除釋放 inode
- [ ] 重設 usdt-bet.com 管理員密碼
- [ ] 加 uploads/.htaccess 禁 PHP
- [ ] 改 FTP 密碼
- [ ] 考慮主機遷移（Vultr SG $6 + Hetzner DE €4.5 + CF）
- [ ] 其他站 WSAL 殘留批量清理

---
*最後更新：2026-04-06*
