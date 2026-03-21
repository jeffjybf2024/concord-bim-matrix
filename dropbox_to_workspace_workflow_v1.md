# Dropbox → Workspace 工作流 v1

## 目標

建立一套穩定的工作方式，讓 Joe 能夠：
- 先索引 Dropbox 的檔案結構
- 讓 Jeff 用自然語意查詢檔案位置與用途
- 在需要時把檔案帶進 workspace 工作區
- 工作完成後把成果回寫或歸位
- 降低檔案找不到、版本混亂、搬錯位置的風險

---

## 核心原則

### 1. 預設「複製到工作區」，不是直接搬走
預設不直接把原始檔從 Dropbox 移走。

優先順序：
1. **索引與定位**
2. **複製到工作區處理**
3. **完成後回寫 / 另存新版本 / 歸位**

這樣比較安全，也不容易破壞既有資料夾邏輯。

### 2. 原始路徑必須被記錄
每次從 Dropbox 帶進 workspace，都要記錄：
- 原始 Dropbox 路徑
- 帶入時間
- workspace 暫存位置
- 是否修改
- 預計回寫路徑
- 最終狀態

### 3. 先做「結構索引」，再做「內容索引」
不要一開始就想把所有檔案全文吃掉。

先做：
- 路徑
- 檔名
- 資料夾層級
- 更新時間
- 檔案大小
- 副檔名

之後再逐步補：
- PDF 摘要
- 文件標題
- 關鍵詞
- OCR / 圖片內容

### 4. 自然語意是入口，不是魔法
Jeff 可以直接用自然語意提問，但背後仍依賴：
- 結構索引
- 檔名 / 關鍵字匹配
- 內容摘要
- 搬運紀錄

### 5. 本機只同步索引，不全量同步 Dropbox 內容
Jeff 偏好：
- 本機保留 Dropbox 的檔案名單 / 索引 / manifest
- 只有在實際需要時，才把單一檔案或小範圍內容帶進 workspace

這代表預設模式應為：
- **metadata-first**
- **on-demand import**

而不是把 Dropbox 全量拉到本機。

---

## 建議的 workspace 結構

```text
workspace/
  incoming/
    dropbox/
  working/
    dropbox/
  outgoing/
    dropbox/
  indexes/
    dropbox/
  manifests/
    dropbox/
```

### 各資料夾用途

#### `incoming/dropbox/`
剛從 Dropbox 帶進來的原始副本。

#### `working/dropbox/`
正在編輯、分析、整理中的工作檔。

#### `outgoing/dropbox/`
準備回寫 Dropbox 的結果檔。

#### `indexes/dropbox/`
Dropbox 結構索引、目錄快照、查詢索引。

#### `manifests/dropbox/`
搬運記錄、來源對照、回寫對照表。

---

## 建議的操作流程

### A. 查詢流程
當 Jeff 問：
- 那份 BIM 報價表在哪
- 找最近更新的合約
- 找台積電薪資申請資料

Joe 的流程：
1. 查 Dropbox 索引
2. 回傳最可能的路徑候選
3. 視需要補充：更新時間 / 類型 / 所在資料夾
4. 讓 Jeff 決定要不要帶進 workspace

---

### B. 帶入 workspace 流程
當 Jeff 說：
- 把這份檔案拉進 workspace
- 幫我把某資料夾帶進來整理

Joe 的流程：
1. 從 Dropbox 複製檔案到 `incoming/dropbox/`
2. 複製工作副本到 `working/dropbox/`
3. 建立 manifest 記錄
4. 回報：已帶入、原始路徑、工作位置

---

### C. 編輯與處理流程
當檔案已在 workspace：
1. 優先在 `working/dropbox/` 操作
2. 保留 `incoming/dropbox/` 原始副本
3. 修改完成後輸出到 `outgoing/dropbox/`
4. 更新 manifest 狀態

---

### D. 回寫 Dropbox 流程
當 Jeff 說：
- 放回原本位置
- 另存到某個專案資料夾
- 覆蓋原檔

Joe 的流程：
1. 讀 manifest 找到原始路徑
2. 確認是：
   - 覆蓋原檔
   - 另存新版本
   - 回存副本
3. 回寫 Dropbox
4. 更新 manifest 為 `returned` / `archived` / `replaced`

---

## manifest 建議格式

每次帶檔案進 workspace，都建立一筆記錄，例如：

```json
{
  "id": "dbx-20260321-001",
  "source": {
    "provider": "dropbox",
    "path": "/公司資料/報價單/BIM專業服務報價表240801.xls"
  },
  "workspace": {
    "incoming": "incoming/dropbox/BIM專業服務報價表240801.xls",
    "working": "working/dropbox/BIM專業服務報價表240801.xlsx",
    "outgoing": "outgoing/dropbox/BIM專業服務報價表240801_v2.xlsx"
  },
  "status": "working",
  "created_at": "2026-03-21T18:56:00+08:00",
  "modified": true,
  "return_target": "/公司資料/02_業務銷售/報價單/"
}
```

---

## Jeff 可以怎麼自然語意使用

### 查詢檔案位置
- 幫我找 BIM 報價表
- 找公司資料裡最新的服務建議書
- Rover 交接資料中和南港高中有關的檔案在哪

### 帶入工作區
- 把這份檔案拉進 workspace
- 把這個資料夾的 PDF 帶進來
- 幫我把報價單相關資料拉進工作區

### 處理完成後回寫
- 幫我放回原本位置
- 幫我另存到公司資料/報價單
- 幫我不要覆蓋原檔，存成 v2

---

## 建議的索引分階段策略

### Phase 1：結構索引
內容包含：
- path
- name
- type
- modified time
- size
- parent folder

### Phase 2：常用區索引
優先針對：
- 公司資料
- 個人資料
- 常用專案資料夾

### Phase 3：內容摘要索引
對高價值內容做：
- PDF 摘要
- 文件標題抽取
- 關鍵字摘要

### Phase 4：常見問法優化
把 Jeff 常問的查詢型問題，轉成穩定查找模式。

---

## 風險控制

### 不直接做的事
- 未經確認就覆蓋原檔
- 未經確認就刪除 Dropbox 檔案
- 未經確認就搬移大量資料夾

### 必須先確認的事
- 是否覆蓋原檔
- 是否改變 Dropbox 原始位置
- 是否大量批次搬運
- 是否涉及共用資料夾 / 團隊資料夾

---

## 工作流 v1 最適合先做的兩件事

### 1. 建立 Dropbox 結構索引
至少先把 `公司資料` 做成可查詢索引。

### 2. 建立 manifest 機制
讓之後任何帶進 workspace 的檔案都有來源與歸位資訊。

---

## 結論

這套工作流的核心不是「把 Dropbox 當硬碟拖來拖去」，而是：

> **讓 Joe 先成為你的檔案定位與搬運助理，再逐步進化成內容理解與回寫助理。**

先有索引、再有搬運、再有內容理解，這樣穩定且可擴充。
