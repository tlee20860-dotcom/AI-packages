# S-01 領域技能包自動建構範本

最後更新：2026-10-15
Version: 2026101501
Batch: System-v1.0
Writer: A20261015-001#038
用途：當使用者要求建立新領域（如 Excel VBA、Python、文案撰寫）時，AI 依此範本自動生成該領域之專屬規範檔（格式：[領域前綴]-01-[名稱].md）

---

## 自動生成結構範本格式

### 1. 標頭與元數據
- 檔名規範：docs/[前綴]-01-[領域名稱].md
- 版本資訊：包含最後更新日期、Version 與 Writer 標記。

### 2. 領域專業標準規範
- 程式碼規範/語法要求/最佳實踐。
- 效能優化要求（如 Excel VBA 的 Application.ScreenUpdating = False）。
- 錯誤處理機制（如 On Error GoTo ErrorHandler）。

### 3. 交付與部署指引
- 標明代碼應貼上的目標位置或檔名結構（例如：標準模組 Standard Module / 工作表 Sheet1）。
- 提供 1-2-3 複製貼上與部署步驟。

---

/* ═══ EOF ═══
F: docs/S-01-skill-template.md
V: 2026101501
═══ */
