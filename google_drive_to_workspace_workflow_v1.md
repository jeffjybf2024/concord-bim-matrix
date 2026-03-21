# Google Drive → Workspace 工作流 v1

## 目標

讓 Google Drive 也採用和 Dropbox 相同的工作模式：
- 先索引
- 再定位
- 必要時才帶檔進 workspace
- 使用 manifest 記錄來源與歸位

## 核心原則
- metadata-first
- on-demand import
- 不全量同步內容到本機
- 先索引，必要時才讀內容

## 建議的 workspace 結構

```text
incoming/google-drive/
working/google-drive/
outgoing/google-drive/
indexes/google-drive/
manifests/google-drive/
```

## Jeff 未來可以怎麼用
- 幫我在 Google Drive 找某份文件
- 把這份檔案帶進 workspace
- 幫我整理內容
- 做完後回存或另存新版本

## v1 重點
先把骨架與工作規則對齊 Dropbox，之後再補：
- 結構索引
- 檔案帶入實測
- 回寫規則
