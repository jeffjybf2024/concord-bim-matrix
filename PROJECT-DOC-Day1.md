# BIM Matrix Day 1 — Project Doc
**項目:** BIM Matrix AI 自動化 — Day 1 任務  
**負責:** Jacky (Hermes Agent, VP Marketing)  
**日期:** 2026-06-15  
**指派來源:** JIM (@Openclaw260402_Agent_bot) 通過群組指派

---

## 一、綱要

### 背景
BIM Matrix V2.2 專案於 2026-06-14 經 Jeff 核准啟動，目標是將 Concord 既有的 43 項 BIM Uses 全面 AI 化，建立可複製的 AI Agent 業務流程。Day 0 已完成 BIM USE 分類表（AI 賦能比例、Prompt、YouTube 教學）。

### 目標
1. 建立 43 項 BIM USE 的報價框架（定價模型）
2. 補完 Day 0 未完成的 BIM 市場競品速覽

### 範圍
- 基於既有 43 項 BIM Uses 的 AI 比例與專業度
- 台灣政府採購網競爭資料 + 行業知識
- 出貨為 Markdown 資料文件，供內部討論與業務使用

---

## 二、過程

### 執行步驟
1. **尋找 Day 0 基礎資料**：在 `~/Concord/Projects/bim-matrix/` 下找到 `jacky-ai-empowerment-v1.md`（全 43 項，含 AI%、Prompt、YouTube）。同時發現已有 `bim-matrix-html/PROJECT_PLAN.html`。
2. **定價模型設計**：以 AI 比例為核心，劃分三個價格帶（Basic/Advanced/Premium），結合專案規模係數與套餐優惠，完成 43 項單價表與速算公式。
3. **競品速覽**：通過 delegate_task 交由子代理：
   - 以台灣政府採購網（web.pcc.gov.tw）搜索 20+ 組關鍵詞
   - 整合行業知識，產出台灣競爭者、全球 AI 平台、能力對比矩陣

### 遇到的問題與解決方案

| 問題 | 解決方案 |
|:---|:---|
| JIM 指派的路徑 `shared/concord-agency/bim-matrix/` 在本地找不到 | 在 `~/Concord/Projects/bim-matrix/` 下找到同款基礎資料，確認兩邊工作區的檔案已同步或存於不同路徑 |
| 子代理無法進行一般網路搜索（僅有台灣採購爬蟲） | 依賴行業知識補充全球競品資訊，並在文件中註明資料來源限制 |
| 台灣政府採購網 BIM 相關案例極少 | 轉向「空間資訊」「智慧建築」「數位孫生」等相關關鍵詞，發現大案例金額達 NT$6,700 萬 |

---

## 三、結果

### 成果清單
1. ✅ **`Day1_BIM_USE_Pricing_Framework.md`** — 43 項 BIM USE 報價框架
   - 三價格帶（Basic/Advanced/Premium）
   - 單項 NT$15,000–75,000
   - 含專案規模係數、套餐優惠、速算公式
   - 全周期 43 項優惠後約 NT$145 萬（中型專案）
2. ✅ **`Day1_BIM_Market_Competitor_Brief.md`** — BIM 市場競品速覽
   - 台灣 7+ 競爭對手快覽
   - 全球 12+ AI BIM 平台對標
   - 能力對比矩陣（台灣傳統 vs 全球平台 vs AI 新創）
   - 戰略要點與定價建議

### 待辨事項
- [ ] Jeff 審核定價水準與價格帶
- [ ] 與競品速覽交叉檢視，調整定價策略
- [ ] 製作報價計算器 / Excel 模板供業務使用
- [ ] 進一步收集競品定價頁面、新聞稿、Crunchbase 資料
- [ ] 製作 SWOT 矩陣與定位圖

### 經驗教訓
- 台灣政府採購網對單純「BIM」採購返回極少，需要擴展到「空間資訊」「智慧建築」「數位孫生」等關聯詞才能找到大案例。
- 兩個平台（Hermes/OpenClaw）的工作區檔案路徑可能不一致，建議以 Google Drive 為主要共用介面，避免路徑混淆。

---

**存放位置:** `~/Concord/Projects/bim-matrix/`
