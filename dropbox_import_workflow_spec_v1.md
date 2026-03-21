# Dropbox 檔案帶入工作區流程規格 v1

## 目的

定義 Joe 如何把 Dropbox 檔案：
- 找到
- 帶入 workspace
- 建立 manifest
- 交給 Jeff 使用或後續處理

---

## 標準流程

### Step 1：定位
Jeff 用自然語意提出需求，例如：
- 幫我找 BIM 報價表
- 找公司資料裡最新的服務建議書
- 找 Rover 交接資料裡和南港高中有關的檔案

Joe 應回傳：
- 最可能的路徑候選
- 檔名
- 所屬資料夾
- 必要時補充更新時間 / 大小 / 類型

---

### Step 2：確認帶入
當 Jeff 指定某份檔案後，Joe 執行：
1. 從 Dropbox 取得檔案下載連結或內容
2. 存到 `incoming/dropbox/`
3. 複製一份到 `working/dropbox/`
4. 建立 manifest

---

### Step 3：回報帶入結果
Joe 應明確回報：
- 來源 Dropbox 路徑
- incoming 路徑
- working 路徑
- manifest 路徑
- 目前狀態（通常是 `working`）

---

## 目錄規則

### `incoming/dropbox/`
放原始帶入副本，不直接修改。

### `working/dropbox/`
放工作中副本，允許修改。

### `outgoing/dropbox/`
放準備回寫 Dropbox 的結果檔。

### `manifests/dropbox/`
放來源與回寫對照資訊。

---

## manifest 最低欄位

```json
{
  "id": "dbx-20260321-001",
  "source": {
    "provider": "dropbox",
    "path": "/公司資料/路徑/檔案.ext"
  },
  "workspace": {
    "incoming": "incoming/dropbox/檔案.ext",
    "working": "working/dropbox/檔案.ext",
    "outgoing": "outgoing/dropbox/檔案_v2.ext"
  },
  "status": "working",
  "created_at": "2026-03-21T19:17:00+08:00",
  "modified": false,
  "return_target": "/公司資料/原始路徑/檔案.ext"
}
```

---

## Jeff 的標準指令範例

### 查詢
- 幫我找 BIM 報價表
- 幫我找最新的服務建議書
- 找出和台積電薪資請款有關的檔案

### 帶入
- 把這份檔案拉進 workspace
- 幫我把這個資料夾的 PDF 帶進來
- 把最新那份報價單帶進工作區

### 工作中
- 幫我摘要這份 PDF
- 幫我看內容重點
- 幫我整理這個 Excel 的欄位結構

---

## Joe 的回應標準

### 成功帶入時
應該說清楚：
- 哪個檔案被帶入
- 從哪裡來
- 現在在 workspace 哪裡
- manifest 在哪裡

### 找不到時
應該提供：
- 最接近候選
- 建議縮小範圍的方式
- 是否要改用關鍵字 / 資料夾範圍查詢

---

## v1 限制

- 目前已確認可做：查找、帶入、建立 manifest
- 目前未完整打通：把修改後檔案直接回寫 Dropbox
- 因此 v1 的重點是：**查找 + 帶入 + 工作區追蹤**

---

## 結論

從現在開始，Dropbox 檔案處理應遵守：

> **先定位，再帶入，再建立 manifest，再工作。**

不要跳步，不然檔案很快就會亂。 
