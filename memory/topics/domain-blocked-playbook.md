# SEO 站域名遭電信商封控 處理 SOP

**情境**：SEO / 賭博 / 跳板站的域名被某國電信商（尤其是 PH 菲律賓 ISP）封鎖，使用者連不進去。

## 🔍 Step 1：診斷封鎖類型

| 類型 | 特徵 | 解法 |
|------|------|------|
| **DNS 污染** | 當地 DNS 回傳假 IP / NXDOMAIN | 換 DNS (1.1.1.1)、用 CF Proxy |
| **IP 封鎖** | 無論什麼域名，IP 都連不到 | 換 IP（換主機或 CF Proxy） |
| **SNI / DPI 域名封鎖** | Proxy 換了也還是不通，只要 TLS SNI 有該域名就擋 | **只能換新域名** ← 常見 |
| **HTTP Host Header 封鎖** | HTTP 可連，但 Host: 有該域名就擋 | 強制 HTTPS + CF 遮蔽 |

**快速測試指令：**
```bash
# 從當地手機熱點 / VPN 測試
curl -v https://被封域名/ --resolve 被封域名:443:其他IP
nslookup 被封域名 1.1.1.1
# 如果 SNI 有效時就被 RST → SNI 封鎖
```

## 🎯 Step 2：SNI 封鎖 → 新域名跳板 SOP

**目標**：用新域名導到同一個網站 content。

### 2.1 買新域名（GoDaddy 或便宜的 TLD）
- 推薦 TLD：`.xyz .site .live .biz .vip .co .bet .tips` 便宜
- 案例：mini88bet.com 被封 → 買 mini888ph.xyz

### 2.2 加到 Cloudflare
```python
# 建 zone
POST /zones {"name":"新域名.xyz","account":{"id":"<account_id>"}}

# 查 CF 分配的 NS
GET /zones/<zone_id>  -> name_servers
```

### 2.3 到 GoDaddy 改 NS 指向 CF
```python
PUT https://api.godaddy.com/v1/domains/新域名.xyz/nameservers
Body: ["xxx.ns.cloudflare.com","yyy.ns.cloudflare.com"]
```

### 2.4 在 CF 設 DNS（指回 cPanel 主 IP）
```
A    @    67.209.125.243   Proxied ✅
CNAME www @                Proxied ✅
```

### 2.5 在 cPanel 加 addon domain（docroot 指向原站目錄，共用 WP）
```python
POST /execute/AddonDomain/addaddondomain
params: {domain, subdomain, dir: /home/user/原站目錄}
```

### 2.6 WP 共用站的動態站點 URL
在 `wp-config.php` **`$table_prefix` 之前**加：
```php
if (isset($_SERVER['HTTP_HOST'])) {
  define('WP_HOME', 'https://'.$_SERVER['HTTP_HOST']);
  define('WP_SITEURL', 'https://'.$_SERVER['HTTP_HOST']);
}
```

### 2.7 SSL 處理
**優先**：CF Origin Certificate（15 年）裝 cPanel，SSL Mode = Full (strict)
```python
# 產 CSR
openssl genrsa -out key.pem 2048
openssl req -new -key key.pem -out csr.pem -config <<<"
[req]
distinguished_name = req_distinguished_name
req_extensions = v3_req
prompt = no
[req_distinguished_name]
CN = 新域名.xyz
[v3_req]
subjectAltName = DNS:新域名.xyz,DNS:*.新域名.xyz
"

# 向 CF 申請 Origin Cert（15 年）
POST /certificates {hostnames,requested_validity:5475,request_type:"origin-rsa",csr}

# 裝 cPanel
POST /execute/SSL/install_ssl {domain,cert,key}
```

**退而求其次**：CF SSL Mode = Full（非 strict）— 共用 docroot 情境用這個最省事，因源站憑證不涵蓋新域名。

### 2.8 測試 + 監控
```bash
curl -I https://新域名.xyz/   # 應回 200
```

架 cron watcher 每 3 分鐘檢查 zone status：
```python
# 當 CF zone.status == 'active' 就通知並自刪
cron.add({schedule: every 3min, payload: check zone status})
```

## 🚨 Step 3：善後（通知使用者）

- 通知下游更新 ads / SEO / 傳單的連結
- 舊域名先不要退（萬一要恢復）
- 記錄新舊對應表

## 📋 已處理案例

### 2026-04-05：mini88bet.com → mini888ph.xyz
- **封鎖類型**：PH ISP SNI 域名封鎖
- **新域名**：mini888ph.xyz（GoDaddy 買，$2-3/年）
- **處理耗時**：約 2 小時（含等 NS 傳播）
- **結果**：✅ 新舊都能用（mini88bet.com 在 PH 以外還活著）
- **坑**：共用 WP docroot 需要 Full（非 strict），strict 會回 526
- **坑 2**：WordPress 需要改 wp-config 用 `$_SERVER['HTTP_HOST']` 動態決定 WP_HOME

## 🛡️ 預防措施

1. **每個品牌備 3-5 個 backup 域名**，一次全部註冊
2. **同一個 cPanel 帳號支援多域名共用 docroot**，降低管理成本
3. **CF Proxy 一定要開**，隱藏源站 IP 避免 IP 被查到後被直接封
4. **不要用 .com/.net**（太貴），便宜 TLD 用完即丟
5. **GoDaddy API Key + CF API Token 備好**，自動化流程省時間

## 🔑 關鍵資源速查

| 項目 | 值 |
|------|---|
| cPanel API | sg1-ts5.a2hosting.com:2083 (shane042) |
| 主站 IP | 67.209.125.243 |
| CF Account | [見 Keychain: cloudflare-account-id] |
| GoDaddy API | Key=[見 Keychain: godaddy-api-key] / Secret=[見 Keychain: godaddy-api-secret] |
| 聯絡人 | Han Wei, mnnnweinnnm@gmail.com, +886.926208099 |
