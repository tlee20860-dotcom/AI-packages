# C-02 通用協作規則庫

最後更新：2026-10-15
Version: 2026101501
Batch: System-v1.0
用途：跨領域（Web App / Excel VBA / 腳本 / 文案）通用之 AI 交付與行為規範

---

## 一、跨領域交付規範 (Universal Delivery)

### 1.1 程式碼與巨集交付
- **Web App (`.js`, `.html`, `.css`, Cloudflare Workers)**：提供完整原始碼，並標註檔案路徑與檔名。
- **Excel VBA (`.bas`, `.cls`)**：提供 100% 完整 `Sub` / `Function` 模組代碼，並強制標註貼上位置（如：`標準模組 Standard Module`、`工作表 Sheet1` 或 `ThisWorkbook`）。
- **自動化腳本 (Python, Apps Script, AHK)**：提供可直接執行的完整腳本，並標註環境相容性與依賴套件。

### 1.2 檔案排版
- **流程/結構**：強制使用 Markdown 表格，避免跨平台字體換行導致撕裂。

---

## 二、專案隔離與向前相容機制 (Forward Compatibility)

1. **專案隔離**：不同專案各自維護獨立 GitHub Repo 與獨立的 `docs/P-03-manifest.md` 與 `docs/P-13-handoff.md`。
2. **規則升級相容**：當全域 `C-02` 更新時，舊專案代碼不作盲目重構。AI 讀取舊專案 `P-13` 時自動採用「向前相容模式」，僅套用最新的全量交付格式。

---

/* ═══ EOF ═══
F: docs/C-02-rules.md
V: 2026101501
═══ */
