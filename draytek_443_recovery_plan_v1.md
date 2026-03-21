# DrayTek 443 回收計畫 v1

## 目標

把 `https://www.site2000.com` 的 443 從 DrayTek 路由器登入頁手上收回來，讓：
- `www.site2000.com` 回到網站服務
- 路由器管理入口不再佔主網域 443
- 後續可以乾淨分出網站 / NAS / 管理入口

---

## 現況摘要

目前外部觀察到：
- `http://www.site2000.com` → nginx 網站
- `https://www.site2000.com` → 302 到 `/weblogin.htm`
- `https://www.site2000.com:5001` → Synology DSM
- 443 對外送出的憑證來自 **DrayTek / Vigor Router**

這代表：
> **443 現在被 DrayTek 路由器管理介面接走。**

---

## 回收的核心原則

### 原則 1：主網站應拿回 443
`https://www.site2000.com` 應回到網站伺服器或反向代理，而不是路由器登入頁。

### 原則 2：路由器管理入口不應佔主網域 443
路由器登入頁最好：
- 僅內網可見
- 或限制來源 IP
- 或改用 VPN / Tailscale 後才可見
- 或改成不與主站衝突的管理入口

### 原則 3：管理入口與公開網站要分流
未來建議：
- `www.site2000.com` → 公開網站
- `nas.site2000.com` / `admin.site2000.com` → NAS / 管理用途
- 路由器管理最好不走公開主網域

---

## 預期在 DrayTek 裡要檢查的地方

不同型號名稱可能略有差異，但通常要看這幾塊：

### 1. System Maintenance / Management / Remote Access
要檢查：
- Web 管理是否允許從 WAN 存取
- HTTPS 管理 port 是否為 443
- HTTP 管理 port 是否對外開啟

### 2. NAT / Open Ports / Port Redirection / Virtual Server
要檢查：
- 443 是否被轉發到路由器自己，而不是網站服務
- 5001 是否有明確轉發到 Synology
- 80 是否轉發到網站主機或 NAS / 反向代理

### 3. SSL VPN / Web Portal / User Login Page
有些 DrayTek 會把：
- SSL VPN
- user portal
- web login
綁到 443

要確認是不是這一類功能佔住 443。

---

## 實際回收步驟（建議順序）

### Step 1：確認 DrayTek 自身的遠端管理是否使用 443
如果是，先改成：
- 關閉 WAN 遠端管理
或
- 改成其他 port（且最好只允許內網 / 指定來源）

### Step 2：確認 443 應該轉給誰
你要先決定：
- 443 是要給網站伺服器？
- 還是給 NAS 反向代理？

目前依你的需求，較合理的是：
- `www.site2000.com:443` → 網站
- `5001` 暫時仍保留給 DSM

### Step 3：保留 5001，但不要把 443 指給路由器
也就是：
- DSM 可暫時繼續走 `:5001`
- 主站 443 回給網站

### Step 4：測試
外部重新驗：
- `https://www.site2000.com`
- `https://www.site2000.com:5001`

確認：
- 主站不再跳 `/weblogin.htm`
- DSM 不受影響

---

## 最低風險方案

### 第一階段（現在）
- 把路由器對外 443 拿掉
- 保留 DSM 走 5001
- 讓主站 443 回給網站

### 第二階段（之後）
- 規劃 `nas.site2000.com` 或 `admin.site2000.com`
- 把 DSM 移到更乾淨的子網域
- 收掉直接暴露的管理 port

---

## 高風險點

在沒搞清楚目前網站是跑在哪裡之前，不要直接亂改：
- 443 NAT 轉發目標
- 反向代理設定
- WAN 管理總開關

因為你有可能一刀砍下去，結果：
- 網站壞掉
- DSM 還在
- 路由器自己也進不去

所以一定要：
1. 先知道誰在提供網站
2. 再改 443 目標

---

## 我建議你接下來提供的資訊

如果要真正執行，我還需要至少其中一項：
- DrayTek 型號
- DrayTek 管理帳密
- 或你先登入 DrayTek，讓我一步一步帶你看設定

---

## 結論

DrayTek 443 回收的本質不是「改一個 port」而已，而是：

> **把主網站入口、路由器管理入口、NAS 管理入口重新切乾淨。**

目前最合理的短期目標是：
- `www.site2000.com` 拿回 443
- DSM 暫時繼續走 5001
- 路由器不要再佔主網域 443
