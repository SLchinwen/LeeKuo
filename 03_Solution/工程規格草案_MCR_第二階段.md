# 工程規格草案｜MCR 第二階段
- **日期：** 2026-01-29
- **版本：** v0.1
- **用途：** 工程設計草案（狀態機 / API / 資料表 / 權限）
- **範圍：** 回報×拍照×審核×請款×完工回報

## 1. 核心狀態機
### 1.1 回報狀態（Report Status）
- Draft → Reported → Approved
- Reported → Returned → Reported（可重新回報）

### 1.2 照片狀態（Photo Status）
- NotRequired / Pending / Completed
- **PhotoPending 可核定/請款**（與拍照流程分線）

### 1.3 關帳狀態（Close Status）
- Open → Closed（**請款關帳後不可更正**）

## 2. 主要資料表（草案）
### 2.1 equipment_process_report
- id
- equipment_id
- process_id
- report_status (Draft/Reported/Returned/Approved)
- photo_status (NotRequired/Pending/Completed)
- reported_by / reported_at
- approved_by / approved_at
- return_reason

### 2.2 equipment_photo
- id
- report_id
- file_url
- version (Current/History)
- uploaded_by / uploaded_at

### 2.3 completion_report
- id
- equipment_id
- completed_flag
- completed_by / completed_at
- reopen_reason
- reopen_at

### 2.4 billing_snapshot
- id
- period
- generated_by / generated_at
- filter_conditions (JSON)
- status (Open/Closed)

## 3. API 端點（草案）
### 3.1 回報
- POST `/reports` 建立/更新回報
- POST `/reports/{id}/submit` 提交回報
- POST `/reports/{id}/return` 退回（需原因）
- POST `/reports/{id}/approve` 核定

### 3.2 照片
- POST `/reports/{id}/photos` 上傳照片（Current/History）

### 3.3 完工回報
- POST `/completion-reports` 提交完工回報
- POST `/completion-reports/{id}/reopen` 重開修改

### 3.4 文件產出
- POST `/billing/exports` 依條件產出文件（記錄 metadata）

### 3.5 關帳
- POST `/billing/close` 合約關帳（請款關帳後鎖定）

## 4. 權限與角色
- 現場人員：回報、拍照
- 監工/主管：審核、退回
- 專案經理：完工回報、請款試算、送出請款
- 請款人員：文件產出與送出

## 5. 驗證規則
- need_photo=否 → 回報不可被照片阻擋
- need_photo=是 → 回報完成但 photo_status=Pending
- 退回需填原因
- 關帳後禁止任何變更

## 6. 待補資訊
- 實際資料表命名與欄位型別
- API 認證與授權方式
- 請款結算文件範本欄位
