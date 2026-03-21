# Dropbox Manifests

這裡放 Dropbox → Workspace 搬運記錄。

每一筆 manifest 至少包含：
- source provider/path
- workspace incoming/working/outgoing 路徑
- status
- created_at
- modified
- return_target

## 狀態建議
- `imported`
- `working`
- `ready_to_return`
- `returned`
- `archived`
- `cancelled`
