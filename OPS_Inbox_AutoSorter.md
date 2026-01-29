# AI 自動歸檔執行手冊 (Runbook)

## 目的
此文件用於指導 AI 如何清空 `_Inbox/` 資料夾，並將檔案標準化後移動至正確位置。

## 執行邏輯 (Execution Logic)

1.  **來源掃描:**
    * 讀取 `_Inbox/` 資料夾內的所有檔案。
    * *注意：忽略任何系統隱藏檔 (.DS_Store等)。*

2.  **內容分析與路由 (Analyze & Route):**
    * 逐一讀取檔案內容。
    * 依據 `.cursorrules` 的定義，判斷其屬於：
        * `01_Discovery` (訪談、現況)
        * `02_Analysis` (分析、流程圖)
        * `03_Solution` (規格、報價)
        * `99_Archives` (無效或過期)

3.  **重新命名與標準化 (Rename & Refactor):**
    * **新檔名:** 依據 `YYYYMMDD_類型_關鍵字.md` 格式。
    * **內容重構:**
        * 若原始檔為 .txt 或非 Markdown，請轉為 Markdown。
        * **關鍵動作:** 必須在檔案最上方加入 YAML Frontmatter (參考 .cursorrules)。

4.  **產出交付物 (Output):**
    * 請撰寫一個 **Python 腳本 (`organize_script.py`)**。
    * 該腳本需包含：
        * 讀取原始檔。
        * 在記憶體中加上 YAML 檔頭。
        * 寫入到新的目標路徑。
        * 刪除 `_Inbox` 中的原始檔。