# AI 對話系統 - 體系狀態包


<!-- === FILE START: docs/records/PROGRESS.md === -->

## 📄 docs/records/PROGRESS.md

```
# 任務進度追蹤

最後更新：ai-system-20261018-007
Version: 1792301500
Writer: ai-system-20261018-007#109
用途：追蹤體系層任務的步驟進度（跨對話持續）

---

## 一、當前任務

### 任務一：四倉架構建置與共享包機制

| 欄位 | 值 |
|---|---|
| 任務名稱 | 四倉架構建置與共享包機制 |
| 開始時間 | 2026-10-16 |
| 當前階段 | 全部完成 |
| 完成度 | 12/12 |

### 任務二：B34-5 防呆強化

| 欄位 | 值 |
|---|---|
| 任務名稱 | B34-5 防呆強化（對話編號生成防呆） |
| 開始時間 | 2026-10-16 |
| 當前階段 | 全部完成 |
| 完成度 | 3/3 |

### 任務三：體系全面重構與雙 AI 路由建置

| 欄位 | 值 |
|---|---|
| 任務名稱 | 體系全面重構（v10a 雙軌 → v10b 切換 → 終極防呆） |
| 開始時間 | 2026-10-17 |
| 當前階段 | 全部完成 |
| 完成度 | 11/11 |

### 任務四：中央廚房與模式庫重構

| 欄位 | 值 |
|---|---|
| 任務名稱 | 修復 Action 變數、Patch 腳本、實作依賴注入與模式庫 |
| 開始時間 | 2026-10-18 |
| 當前階段 | 全部完成 |
| 完成度 | 5/5 |

### 任務五：廢除批次號與自動化遷移

| 欄位 | 值 |
|---|---|
| 任務名稱 | 廢除批次號、修正 setup 腳本、跨倉庫 EOF 遷移 |
| 開始時間 | 2026-10-18 |
| 當前階段 | 全部完成 |
| 完成度 | 8/8 |

---

## 二、步驟清單

### 任務三：體系重構與雙 AI 路由建置

| # | 步驟 | 狀態 | 完成時間 | 備註 |
|---|---|---|---|---|
| 1 | 交付 setup-structure.yml (v10a) | ✅ | 2026-10-17 | Action 綠燈 |
| 2 | 交付 C-00-startup.md | ✅ | 2026-10-17 | 移除 R-01 / PROJECT-LIST |
| 3 | 交付 C-02-rules.md | ✅ | 2026-10-17 | 加入 §六點六、D-161/162 |
| 4 | 交付 C-04-delivery.md | ✅ | 2026-10-17 | 加入兩階段確認 |
| 5 | 交付 C-11-terms.md | ✅ | 2026-10-17 | 新增詞彙 |
| 6 | 交付 SYSTEM-CONCLUSION.md | ✅ | 2026-10-17 | 新增 D-161~D-166 |
| 7 | 交付 SYSTEM-HANDOFF.md | ✅ | 2026-10-17 | 更新進度 |
| 8 | 交付 P-01-index.md (專案層) | ✅ | 2026-10-17 | 加 YAML frontmatter |
| 9 | 交付 setup-structure.yml (v10b) | ✅ | 2026-10-17 | 執行 Step 1 刪除舊檔 |
| 10 | 重構 Patch 腳本與 Workflow | ✅ | 2026-10-17 | 建立 .py，解決權限與迴圈 |
| 11 | 建立雙 AI 路由與熱更新 | ✅ | 2026-10-17 | 寫入 C-00、C-02、D-172/173 |

### 任務四：中央廚房與模式庫重構

| # | 步驟 | 狀態 | 完成時間 | 備註 |
|---|---|---|---|---|
| 1 | 修復 setup-structure.yml 變數錯位 | ✅ | 2026-10-18 | 補齊 repo 變數 |
| 2 | 重寫 apply_patch.py | ✅ | 2026-10-18 | 解決換行符與格式判定 |
| 3 | 實作依賴注入 (中央廚房) | ✅ | 2026-10-18 | 新增 03-INFRA.md，專案只留呼叫器 |
| 4 | 新增設計模式庫 (C-06) | ✅ | 2026-10-18 | 提架構前必看，加入 S-005 |
| 5 | 更新啟動機制與規則 | ✅ | 2026-10-18 | C-00 新增 On-demand 載入說明 |

### 任務五：廢除批次號與自動化遷移

| # | 步驟 | 狀態 | 完成時間 | 備註 |
|---|---|---|---|---|
| 1 | 更新 C-04、C-03、C-05 | ✅ | 2026-10-18 | 移除批次號，改用 epoch |
| 2 | 修正 setup-structure.yml | ✅ | 2026-10-18 | 移除 B 欄、修正 -d bug、加入自打包 |
| 3 | 建立 migrate_eof.py 與 workflow | ✅ | 2026-10-18 | 實作 report/apply 雙模式 |
| 4 | 更新 C-06 模式庫 | ✅ | 2026-10-18 | 加入 §五：跨倉庫安全批量遷移 |
| 5 | 修正 C-03 舊制啟動包名稱 | ✅ | 2026-10-18 | 改為 A00 + S00 |
| 6 | 升級 R-011 檔案取得鐵律 | ✅ | 2026-10-18 | 整合散落規則，禁止叫使用者查看 |
| 7 | Action 防連發優化 | ✅ | 2026-10-18 | 加入 cancel-in-progress: true |
| 8 | 完善轉移對話 SOP 與全局掃描 | ✅ | 2026-10-18 | C-03 §7.2 加入強制開新對話與殘留掃描 |

---

## 三、狀態定義

| 標記 | 意義 |
|---|---|
| ✅ | 已完成 |
| ⏳ | 待處理 |
| 💬 | 討論中 |
| ✏️ | 已交付待確認 |
| 🔄 | 需重新交付 |
| 🗑️ | 已廢棄 |

---

## 四、更新時機（B + C）

| 規則 | 說明 |
|---|---|
| 平時累積變動 | AI 在對話中記錄，但不立即請使用者貼 |
| 階段完成時 | AI 整合所有變動，一次更新 PROGRESS.md |
| 觸發條件 | 使用者主動說「更新進度」或 AI 判斷階段完成 |

---

## 五、分工說明

| 層級 | 追蹤範圍 | 檔案 |
|---|---|---|
| 體系層 | 遷移、共享包、規則更新 | PROGRESS.md（本檔） |
| 專案層 | 各專案任務 | 各專案 P-02、P-13 |

---

## 六、關聯檔案

| 主題 | 檔案 |
|---|---|
| 檔案狀態總表 | C-04 §六 |
| 交付批次 | 各專案 P-02 |
| 對話交接 | 各專案 P-13 |
| 規則 | C-02 §一 |

---

/* ═══ EOF ═══
F: AI-General/docs/records/PROGRESS.md
V: 1792301500
W: ai-system-20261018-007#109
═══ */
```

<!-- === FILE END: docs/records/PROGRESS.md === -->


<!-- === FILE START: docs/records/STATE-INDEX.md === -->

## 📄 docs/records/STATE-INDEX.md

```
# STATE-INDEX

## 一、專案總覽
| 專案顯示名稱 | code | domain | package_no | repo |
|---|---|---|---|---|
家庭 webapp|webapp|W|90|tlee20860-dotcom/WEB-APP-Family

## 二、檔案清單
| Repo | 檔案 | Version | Writer |
|---|---|---|---|
AI-General|docs/records/SYSTEM-HANDOFF.md|1792301500|ai-system-20261018-007#109
AI-General|docs/records/SYSTEM-CONCLUSION.md|1792301500|ai-system-20261018-007#109
AI-General|docs/records/PROGRESS.md|1792301500|ai-system-20261018-007#109
AI-General|docs/common/C-11-terms.md|1792301000|ai-system-20261018-007#106
AI-General|docs/common/C-06-patterns.md|1792301000|ai-system-20261018-007#106
AI-General|docs/common/C-02-rules.md|1792300500|ai-system-20261018-007#104
AI-General|docs/common/C-05-engine.md|1792300000|ai-system-20261018-007#102
AI-General|docs/common/C-04-output.md|1792300000|ai-system-20261018-007#102
AI-General|docs/common/C-03-flow.md|1792300000|ai-system-20261018-007#102
AI-General|docs/common/C-00-startup.md|1792299500|ai-system-20261018-007#098
AI-General|docs/common/C-05-learning/system.md|1792289500|ai-system-20261018-006#051
AI-General|docs/common/C-05-learning/general.md|1792226400|ai-system-20261017-004#032
AI-General|docs/skills/W/W-00-startup.md|1791903760|A20261013-004#169
AI-General|docs/skills/W/W-05-schema.md|1791899740|A20261013-004#065
AI-General|docs/skills/W/W-04-pwa.md|1791899680|A20261013-004#065
AI-General|docs/skills/W/W-03-esmodule.md|1791899620|A20261013-004#065
AI-General|docs/skills/W/W-02-functions.md|1791899560|A20261013-004#065
AI-General|docs/skills/W/W-01-firebase.md|1791899500|A20261013-004#065

## 三、歷史歸屬
| 檔案 | 舊版本 | 舊 Writer | 新版本 | 新 Writer | 取代時間 |
|---|---|---|---|---|---|

## 四、已廢除檔案
| 檔案 | 廢除時間 | 內容去向 |
|---|---|---|
| R-01-writers.md | 2026-10-17 | 內容分散至 C-02、P-01 YAML、STATE-INDEX |
```

<!-- === FILE END: docs/records/STATE-INDEX.md === -->


<!-- === FILE START: docs/records/SYSTEM-CONCLUSION.md === -->

## 📄 docs/records/SYSTEM-CONCLUSION.md

```
# SYSTEM-CONCLUSION（體系層滾動結論）

最後更新：ai-system-20261018-007
Version: 1792301500
Writer: ai-system-20261018-007#109
用途：體系層滾動式累積結論（議題 + 設計 + 決策）

---

## 一、議題清單

| # | 議題 | 狀態 |
|---|---|---|
| 1 | 啟動包步驟 4 改寫 | ✅ |
| 2 | 拆分啟動包 | ✅ |
| 3 | 包編號十位數類別制 | ✅ |
| 4 | C-05 拆三分檔 | ✅ |
| 5 | C-02 §19.1 補觸發 | ✅ |
| 6 | C-02 §19.2 補檢查 | ✅ |
| 7 | C-02 提示上傳 | ✅ |
| 8 | C-00 §十一 加 notify | ✅ |
| 9 | C-00 §十 改寫 | ✅ |
| 10 | notify.yml × 2 | ✅ |
| 11 | README + 索引 | ✅ |
| 12 | C-11 新增詞彙 | ✅ |
| 13 | C-01 核心目的 | ✅ |
| 14 | SYSTEM-CONCLUSION 新增 | ✅ |
| 15 | SYSTEM-HANDOFF 新增 | ✅ |
| 16 | 登記 L-023 | ✅ |
| 17 | 移除 ai-training | ✅ |
| 18 | 改名 AI-packages | ✅ |
| 19 | AI-training-files 移出掃描 | ✅ |
| 20 | 構建編號 | ✅ |
| 21 | 路徑格式修正 | ✅ |
| 22 | yml paths 修正 | ✅ |
| 23 | 舊檔清理 | ✅ |
| 24 | 檢查共享包內容 | ✅ |
| 25 | 廢除 R-01 與手動清單 | ✅ |
| 26 | 舊專案 P-* 對齊 | ✅ |
| 27 | Action 校驗機制 | ✅ |
| 28 | P-01 改用 YAML | ✅ |
| 29 | 新增專案防呆 | ✅ |
| 30 | 全體系精簡藍圖 | ✅ |
| 31 | 兩階段確認機制 | ✅ |
| 32 | Action 順序 B 腳本 | ✅ |
| 33 | 雙軌重構：先加後減 | ✅ |
| 34 | P-01 加 YAML | ✅ |
| 35 | v10b 切換版（廢除舊檔） | ✅ |
| 36 | C-00 新增專案先索取 Repo | ✅ |
| 37 | C-00 自動推斷專案 | ✅ |
| 38 | Patch 腳本與正則替換 | ✅ |
| 39 | 體系重構收尾與驗證 | ✅ |
| 40 | 雙 AI 路由與熱更新 | ✅ |
| 41 | 體系包直通修正 | ✅ |
| 42 | 修復 Action 變數錯位與 Patch 腳本 | ✅ |
| 43 | Infra 中央廚房重構 (03-INFRA.md) | ✅ |
| 44 | 新增設計模式庫 (C-06) | ✅ |
| 45 | 拆分 C-02 與 C-11 (SSOT) | ✅ |
| 46 | 廢除批次號 (B) 與自動化遷移 | ✅ |
| 47 | 跨倉庫安全批量遷移與權限突破 | ✅ |
| 48 | 完善轉移對話 SOP 與全局掃描 | ✅ |

---

## 二、設計結論

### 2.1 共享包體系

| 編號 | 檔案 | 類別 |
|---|---|---|
| 01 | 01-CORE-SYSTEM.md | 體系核心 |
| 02 | 02-CORE-PROJECT.md | 專案核心 |
| 03 | 03-INFRA.md | 基礎設施總包（中央廚房） |
| 06 | 06-PATTERNS.md | 設計模式庫 |
| 10 | 10-SKILLS-W.md | Web App 技能 |
| 20 | 20-SKILLS-V.md | VBA 技能（未來） |
| 30 | 30-SKILLS-D.md | 資料分析技能（未來） |
| 90 | 90-web-app-family.md | 家庭 webapp |
| 91+ | 未來專案 | 依序 |
| 99 | 99-CONCLUSION-ARCHIVE.md | 罕用 |

### 2.2 啟動機制

| 對話類型 | 上傳 |
|---|---|
| 體系層 | `A00-star-code.md` + `S00-ai-system.md` |
| 專案層 | `A00-star-code.md` + `90-{專案}.md` |
| 腳本修改 | `A00` + `03-INFRA.md` |
| 架構設計 | `A00` + `06-PATTERNS.md` |

### 2.3 共享包頂部資訊

- 生成時間：YYYY-MM-DD HH:mm:ss UTC
- 構建編號：#NNNN
- AI 讀取後必須回報並問是否為最新。

### 2.4 C-05 分檔

| 分檔 | 適用 |
|---|---|
| general.md | 所有對話 |
| system.md | 體系層 |
| w.md | Web App |

### 2.5 體系層上下通順

| 檔案 | 用途 |
|---|---|
| SYSTEM-CONCLUSION.md | 滾動結論（議題 + 決策） |
| SYSTEM-HANDOFF.md | 交接快照（≤ 80 行） |

### 2.6 全新精簡與自動化藍圖

| 動作 | 內容 | 狀態 |
|---|---|---|
| 廢除 | R-01、PROJECT-LIST | ✅ 已執行 |
| 維持分檔 | C-05~C-10 保留分檔，由 Action 合併 | ✅ |
| 自動化 | Action 生成 `STATE-INDEX.md` | ✅ |
| SSOT | P-01 頂部改用 YAML frontmatter | ✅ |
| 防呆自檢 | AI 交付大檔須附「完整性自檢表」 | ✅ |
| 兩階段確認 | 先問「確認完整」，再提示貼上 | ✅ |
| Action 替換 | 重構時採「先加後減」 | ✅ |
| Patch 腳本 | `.github/scripts/apply_patch.py` 支援正則 | ✅ 已上線 |
| 中央廚房 | 腳本統一由體系管理，專案只留呼叫器 | ✅ 已上線 |
| 模式庫 | 建立 C-06，提架構前必看 | ✅ 已上線 |

---

## 三、決策記錄

| 編號 | 決策 | 說明 |
|---|---|---|
| D-136~D-147 | （見前次紀錄，略） | 共享包與防呆機制 |
| D-148 | 廢除 R-01 | 手動記錄全面交給 Action |
| D-149 | FILE-INDEX 加編寫者彙總 | 已由 STATE-INDEX 取代 |
| D-150 | 新增 K 領域（discuss） | 純對話專案 |
| D-151 | 舊專案 P-* 對齊 | 刪除體系引用 |
| D-152 | 新增 Action 校驗 | 掃描 md 引用，缺漏拋警告 |
| D-153 | 廢除 PROJECTS.md | 各 P-01 為 SSOT |
| D-154 | 新增專案防呆 | 先索取 Repo，未取得不繼續 |
| D-155 | 維持 Action 即時觸發 | 不累積 push，保證即時性 |
| D-156 | 精簡防誤刪 | 重構前列「內容去向」與「不可刪清單」 |
| D-157 | C-04 加入自檢勾選表 | 交付前必須輸出完整檢查表並勾選 |
| D-158 | 兩階段確認機制 | 交付後先問「確認完整」，再提示「已更新」 |
| D-159 | Action 重構腳本化 | AI 產腳本，Action 執行「先刪舊、再改名」 |
| D-160 | 行數防呆 | Action 檢查暴減，防 AI 偷懶刪內容 |
| D-161 | 源檔分檔，Action 合併 | 禁止手動合併源檔，一律轉為「改 Action 配置」。 |
| D-162 | 兩階段確認機制（正式寫入 C-04） | 交付後先問「確認完整」，貼上後回「已更新」才算完成。 |
| D-163 | R-01 正式廢除 | 內容分散至 C-02、P-01 YAML、STATE-INDEX。 |
| D-164 | PROJECT-LIST 正式廢除 | 專案資訊改由 P-01 YAML + `PROJECT_REPOS` 提供。 |
| D-165 | P-01 加 YAML frontmatter | 頂部需加 name/code/domain/package_no/repo。 |
| D-166 | 專案清單由 yml 頂部 PROJECT_REPOS 提供 | 新增專案時，由使用者提供 Repo，AI 負責更新。 |
| D-170 | 建立 S/R/F/K 矩陣標準 | S-List（事前）、R-List（行為鐵律）、F-List（交付格式）、K-List（事後校驗），形成完整防呆閉環。 |
| D-171 | A00（大腦）與 S00（狀態）分離 | 大腦包（A00-star-code）專注規則與機制；狀態包（S00-ai-system）專注進度與交接，降低 AI 讀取負擔。 |
| D-172 | 人類作為中介路由（Human-in-the-loop） | 專案 AI 若需體系層（C-*）修改或確認，必須輸出「【轉交體系】」標籤。使用者作為唯一路由，負責複製貼上。專案 AI 亦可接收最新 A00 進行熱更新。 |
| D-173 | 轉交指令的 Code Block 規範 | 凡是「轉交指令」（如【轉交體系】或【轉交專案】），內容**必須**用 Markdown Code Block 包裹，防止手機複製時格式跑版。一般對話則嚴禁濫用 Code Block。 |
| D-174 | 體系包直通 | 若使用者上傳 A00 + S00 且明確表示要進行體系修正，AI 直接生成體系對話編號，不列專案清單、不列 4 選項，直接進入 STATE_DISCUSS。 |
| D-175 | 中央廚房（依賴注入） | 自動化腳本統一由 AI-General 管理（SSOT），專案端只保留呼叫器，共享包產出單一 `03-INFRA.md` 供對比。 |
| D-176 | 設計模式庫 (C-06) | 建立 `C-06-patterns.md`，並在 S-List 加入 S-005。涉及架構設計、新專案時，必先查閱此庫。 |
| D-177 | C-02 與 C-11 拆分 (SSOT) | 共同語言表獨立於 `C-11-terms.md`，`C-02-rules.md` 僅保留行為法則，不重複收錄詞彙。 |
| **D-178** | **廢除批次號機制** | 為消除與 epoch 的功能重疊，全面廢除 EOF 的 `B:` 欄位與交付批次號。 |
| **D-179** | **舊檔 EOF 遷移策略** | 舊檔的 `B: xxx` 統一由 `migrate_eof.py` 遷移為 `B(歷史): xxx`，保留資料。 |
| **D-180** | **跨倉庫安全遷移模式** | 建立 `migrate-eof.yml`，支援 `report` 與 `apply` 雙模式，並使用 `CROSS_REPO_PAT` 突破預設權限限制。 |
| **D-181** | **setup-structure.yml 自打包** | 修正 Step 6 專案層 `-d` 判定 bug，並將 `setup-structure.yml` 自身加入 03-INFRA 產出中。 |
| **D-182** | **手機友善通用格式** | 提問與選項全面採用「Q1 + 選項 + 💡解釋」格式，禁止使用寬表格防跑版。 |
| **D-183** | **30 輪大腦重整與 Token 阻斷** | 廢除 20/50 機制，改為 30 輪強制重讀 C-00/C-02；Token 僅供顯示，禁止影響 AI 邏輯。 |
| **D-184** | **結構完整性檢查 (F-008)** | 交付完整檔時必須盤點章節，禁止為省 Token 刪減段落。 |

---

## 四、關聯檔案

| 主題 | 檔案 |
|---|---|
| 體系層交接 | SYSTEM-HANDOFF.md |
| 啟動卡 | C-00-startup.md |
| 通用規則 | C-02-rules.md |
| 共同語言表 | C-11-terms.md |

---

/* ═══ EOF ═══
F: AI-General/docs/records/SYSTEM-CONCLUSION.md
V: 1792301500
W: ai-system-20261018-007#109
═══ */
```

<!-- === FILE END: docs/records/SYSTEM-CONCLUSION.md === -->


<!-- === FILE START: docs/records/SYSTEM-HANDOFF.md === -->

## 📄 docs/records/SYSTEM-HANDOFF.md

```
# SYSTEM-HANDOFF（體系層交接快照）

最後更新：ai-system-20261018-007
Version: 1792301500
Writer: ai-system-20261018-007#109
上限：80 行
用途：體系層對話交接快照

---

## 一、當前會話摘要

| 欄位 | 值 |
|---|---|
| 對話編號 | ai-system-20261018-007 |
| 本次範圍 | 廢除批次號、R-011 升級、防連發、完善交接 SOP |
| 累計回覆 | 109 條 |
| 狀態 | 體系機制全面完備，可交接 |

---

## 二、議題進度

**指向**：SYSTEM-CONCLUSION.md §一

**本對話重點結論**：
1. **廢除批次號**：全面改用 epoch，舊檔由遷移腳本改為 `B(歷史):`。
2. **權限突破**：改用 `CROSS_REPO_PAT` 解決無法修改 workflow 的問題。
3. **R-011 升級**：整合 6 處散落引用，升級為「檔案取得鐵律」。
4. **完善交接 SOP**：C-03 §7.2 規定必須開新對話並提供交接詞。
5. **全局一致性掃描**：交接前必須掃描舊名詞（已清除 C-00/C-11 舊啟動包名）。
6. **排版與防呆**：加入手機友善格式、30 輪大腦重整、F-008 結構完整性。

---

## 三、關鍵決策

**指向**：SYSTEM-CONCLUSION.md §三

**本次新增決策**：D-178 ~ D-184。

---

## 四、重建條件（下一 AI 優先確認）

| # | 優先確認 |
|---|---|
| 1 | `migrate_eof.py` 是否已經在 Actions 以 `apply` 模式成功執行過？ |
| 2 | webapp 對話是否已完成熱更新（讀取新 A00）？ |
| 3 | webapp 是否還有 `setup-structure.yml` Step 5 的轉交需求？ |

---

## 五、未完成事項與驗證方式

### 5.1 跨層待辦提醒（專案層）

| 專案 | 來源對話編號 | 未完成事項提醒 | 建議動作 |
|---|---|---|---|
| web-app-family | webapp-20261017-001 | 已發送暫停指令，等待熱更新。 | 確認是否需轉交 Step 5。 |

---

## 六、下一步建議

| 順序 | 動作 |
|---|---|
| 1 | 下載最新的 `A00` 與 `S00`。 |
| 2 | 將新 `A00` 熱更新給 webapp 對話。 |
| 3 | 若 webapp 有 Step 5 需求，由體系層接手修改 `setup-structure.yml`。 |

---

/* ═══ EOF ═══
F: AI-General/docs/records/SYSTEM-HANDOFF.md
V: 1792301500
W: ai-system-20261018-007#109
═══ */
```

<!-- === FILE END: docs/records/SYSTEM-HANDOFF.md === -->

