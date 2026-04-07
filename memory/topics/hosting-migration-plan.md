# 主機遷移規劃（待處理）

**記錄日期**：2026-04-05
**背景**：A2 Hosting cPanel `shane042` 帳號吃 42+ 個博弈網站，inode 95%+、頻繁被駭（WP 後門、假管理員），用戶計畫逐步遷到別的服務商分散風險。

## 用戶需求
- 費用合理
- API 完整（自動化建站、管理）
- 適合博弈站多網域
- 亞洲使用者為主（菲律賓 PH）

## 推薦服務商（已調研）

### 🥇 Hetzner（歐洲）
- **方案**：CX22，€4.5/月（約 $5）
- **規格**：2 vCPU / 4GB / 40GB SSD
- **機房**：德國 / 芬蘭
- **API**：10/10（hcloud CLI、Terraform）
- **20TB 流量免費**
- **適用**：落地頁、跳板域名、非互動站
- **延遲**：菲律賓 ~270ms（但 CF Proxy 可補）

### 🥈 Vultr（亞洲）
- **方案**：$6/月（1 vCPU / 1GB / 25GB）
- **機房**：新加坡 / 東京 / 台灣
- **API**：完整（vultr-cli、Terraform）
- **適用**：WP 主站、互動站
- **延遲**：菲律賓 30~90ms

### 🥉 DigitalOcean
- $6/月起，新加坡機房
- 文件最好，但比 Vultr/Hetzner 貴

### ❌ GCP（不建議博弈站主站）
- **風險**：Google 對 gambling 風控嚴，帳號可能被停
- 費用：e2-small 台灣 ~$17/月
- Egress 貴（$0.12/GB 亞洲）
- **只適合**：非博弈 landing page / API 服務

### 🔧 Cloudways（託管）
- $11/月起，底層用 Vultr/DO
- 有 WP 管理面板 + API
- 適合不想碰 Linux 的人

## 建議架構（組合方案）

```
核心主站（WP 後台）
  → Vultr 新加坡 $6/月
  → 低延遲互動體驗

跳板/備用/落地頁
  → Hetzner 德國 €4.5/月一台
  → 裝 CloudPanel，一台放 20+ 個站

DNS + 防護
  → Cloudflare（免費 Plan 夠用）

域名註冊
  → GoDaddy / Namecheap（API 已串好）
```

**預估月成本**：約 $16（比 A2 便宜一半 + 風險分散）

## 延遲解決方案（關鍵）
- 所有站必須開 **Cloudflare Proxy 橘雲**
- CF 馬尼拉節點會快取靜態內容 → 菲律賓使用者無感
- 純靜態站 / landing page 放歐洲完全 OK
- 動態 WP 後端放新加坡/東京比較順

## 站類型分配建議

| 站類型 | 建議機房 |
|--------|---------|
| landing page / 落地頁 | Hetzner 歐洲 + CF |
| 跳板 / 重定向 (jump domain) | Hetzner 歐洲 + CF |
| 博弈 WP 主站 | Vultr 新加坡 / 東京 |
| 關鍵營運站 | GCP 台灣（貴但穩） |

## 下一步（待用戶決定）
- [ ] 試開 Hetzner CX22 一台，裝 CloudPanel 當沙盒
- [ ] 把空 addon domain 從 A2 逐步遷走
- [ ] 決定哪些站最先搬（建議從空/少內容的先）
- [ ] 確認菲律賓博弈站是否有法規/金流綁定主機
