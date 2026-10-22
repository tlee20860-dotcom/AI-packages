# AI 對話系統 - 體系狀態包

> 生成時間：2026-10-22 13:36:11 (HKT)

---
⚠️ **【給 AI 的強制前置檢查】**
請立刻確認：你是否已經讀取 `IP.md`、`AP.md`、`RP.md`、`UP.md`（體系四檔）？
1. 如果「否」或「不確定」，請立刻停止閱讀本檔，先去讀取缺漏的檔案，載入身份、流程、規則與偏好。
2. 如果「是」，請依據 `IP.md` 與 `AP.md` 輸出載入確認。
---


<!-- === FILE START: AI-General/docs/records/SYSTEM-HANDOFF.md === -->

## 📄 AI-General/docs/records/SYSTEM-HANDOFF.md

# SYSTEM-HANDOFF（體系層交接快照）

上限：80 行
用途：體系層對話交接快照
records_pending: []

---

## 一、當前會話摘要

| 欄位 | 值 |
|---|---|
| 對話編號 | ai-system-20261021-006 |
| 本次範圍 | 體系微核心化 v1.5 落地（四檔重構、R-05 新增、SP 擴六檔）、Phase 2 腳本升級（修補漏檔與 F-004 誤報） |
| 累計回覆 | 約 48 條 |
| 狀態 | 全數結案；已產出 STATE_SEAL 並重置 |

---

## 二、議題進度

**指向**：SYSTEM-CONCLUSION.md §一

**本對話重點結論**：
1. **微核心化定案**：v1.5 合併八裁決與四缺失，經兩份外部審查後落地。
2. **FSM 結構化**：T99 移出 transitions 改 `fallback_handler`；加 `boot_check`、`pre_condition_guard`；`evaluation_order: strict_top_down`。
3. **FastDev 自檢**：主走 GitHub Actions 正則，無 CI 時走提取式＋Ken 抽查。
4. **F-004 修正**：原「禁內層反引號」導致誤報，改為「檔案邊界防呆（首尾禁反引號）」。
5. **SP 擴為六檔**：新增 `SYSTEM-REJECTED.md`、`SYSTEM-DEBT.md`，採 APPEND_ONLY。

---

## 三、關鍵決策

**指向**：SYSTEM-CONCLUSION.md §三

**本次新增決策**：D-150～D-156。

---

## 四、重建條件（下一 AI 優先確認）

| # | 優先確認 |
|---|---|
| 1 | 是否使用最新四檔與 SP 六檔 |
| 2 | `records_pending` 是否為空；非空須先補寫 |
| 3 | `setup-structure.yml` 是否已將 R-05 與兩新檔正確打包 |

---

## 五、未完成事項

| 項目 | 狀態 |
|---|---|
| K-01：F-005 與 I／A／U／W／SP 內文 Version 矛盾 | 暫不處理（見 SYSTEM-DEBT.md） |
| K-03：R-00 契約表加 DSL 欄 | 待處理（見 SYSTEM-DEBT.md） |
| 跨模型對抗驗收（Phase 3） | 未啟動 |

---

## 六、下一步建議

| 順序 | 動作 |
|---|---|
| 1 | 跑一次 Actions，確認 Step 9.8 驗證報告為 0 錯誤 |
| 2 | 若需處理體系技術債，可開新對話處理 K-01／K-03 |
| 3 | 進入 WebApp 專案對話，處理 `entity-registry.js` 與 B34-5 業務開發 |

/* ═══ EOF ═══
F: AI-General/docs/records/SYSTEM-HANDOFF.md
V: 1792600038
W: ai-system-20261021-006#15
═══ */
<!-- === FILE END: AI-General/docs/records/SYSTEM-HANDOFF.md === -->

<!-- === FILE START: AI-General/docs/records/PROGRESS.md === -->

## 📄 AI-General/docs/records/PROGRESS.md

# 任務進度追蹤

用途：追蹤體系層任務的步驟進度（跨對話持續）

---

## 一、當前任務

### 任務十九：體系微核心化與腳本對齊（006）
| 欄位 | 值 |
|---|---|
| 任務名稱 | 微核心化 v1.5 落地、F-004 修正、Phase 2 腳本升級 |
| 開始時間 | 2026-10-21 |
| 當前階段 | 全數完成 |
| 完成度 | 100% |

### 任務十八：體系身份重構與腳本修復（005）
| 欄位 | 值 |
|---|---|
| 任務名稱 | WF-* 污染修復、腳本順序修復、全中文標頭、沙盒機制 |
| 開始時間 | 2026-10-21 |
| 完成度 | 100% |

---

## 二、步驟清單

### 任務十九：體系微核心化與腳本對齊
| # | 步驟 | 狀態 | 完成時間 |
|---|---|---|---|
| 1 | v1.5 規格定稿（合併八裁決與四缺失） | ✅ | 2026-10-22 |
| 2 | 重寫 I-00／A-00／R-00／R-03／U-00 | ✅ | 2026-10-22 |
| 3 | 新增 R-05-dsl-map（36 條映射） | ✅ | 2026-10-22 |
| 4 | SP 擴為六檔（新增 REJECTED／DEBT） | ✅ | 2026-10-22 |
| 5 | 修改 setup-structure.yml（補 R-05、兩新檔、Step 9.8） | ✅ | 2026-10-22 |
| 6 | 修改 validate_delivery.py（F-004 邊界防呆） | ✅ | 2026-10-22 |

### 任務十八：體系身份重構與腳本修復（承接）
*(步驟 1~5 已完成，省略)*

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

## 四、階段總結標記

| 日期 | 對話編號 | 完成任務 |
|---|---|---|
| 2026-10-21 | ai-system-20261021-004 | 任務十七（100%） |
| 2026-10-21 | ai-system-20261021-005 | 任務十八（100%） |
| 2026-10-22 | ai-system-20261021-006 | 任務十九（100%） |

/* ═══ EOF ═══
F: AI-General/docs/records/PROGRESS.md
V: 1792600037
W: ai-system-20261021-006#15
═══ */
<!-- === FILE END: AI-General/docs/records/PROGRESS.md === -->

<!-- === FILE START: AI-General/docs/records/SYSTEM-CONCLUSION.md === -->

## 📄 AI-General/docs/records/SYSTEM-CONCLUSION.md

# SYSTEM-CONCLUSION（體系層滾動結論）

用途：體系層滾動式累積結論（議題 + 設計 + 決策）

---

## 一、議題清單

| # | 議題 | 狀態 |
|---|---|---|
| 1~69 | 歷史議題 | ✅ |
| 70 | Version SSOT 與 Patch 工具缺陷 | ✅ |
| 71 | W 包重構與專案紀錄 WF 對齊 | ✅ |
| 72 | 體系微核心化 v1.5（DSL／FSM／STATE_SEAL） | ✅ |

---

## 二、設計結論

*(2.1~2.22 保留原樣)*

### 2.23 體系 v3 重構（003 定案）
*(原 10 點保留)*

### 2.24 W 包與自動化對齊（004 定案）
1. **W 包現代化**：擴充至 W-00~W-07（新增通用 SOP 與錯誤），完全拔除專案綁定字眼。
2. **紀錄分離**：專案紀錄檔對齊 AI-G 命名（WF-*）；決策與錯誤明細降級為 P-04/14/99。
3. **腳本自治**：`setup-structure.yml` 不只打包，還負責生成並回寫專案的 `WF-STATE-INDEX.md`。
4. **防撞機制**：專案 `notify.yml` 忽略 `WF-STATE-INDEX.md`，阻斷無限迴圈。

### 2.25 體系微核心化 v1.5（006 定案）
1. **FSM 結構化**：T99 移出 transitions 改 `fallback_handler`；`evaluation_order: strict_top_down`；四步求值（boot_check → pending → transitions → fallback）。
2. **開場版本比對**：`boot_check` 抓「改過四檔卻未結案」；口頭決策遺失抓不到。
3. **pending 攔截**：`records_pending` 單一真相在 HANDOFF 檔頭；只擋交付，不擋討論。
4. **FastDev 自檢**：主走 GitHub Actions 正則；無 CI 時提取式＋Ken 抽查。
5. **R-011 補強**：物理關鍵字否定清單＋`domain_classification` 宣告。
6. **F-004 例外**：md 檔內含 YAML 時，外層 MUST 用四反引號。
7. **SP 擴為六檔**：新增 `SYSTEM-REJECTED.md`、`SYSTEM-DEBT.md`，皆 APPEND_ONLY。
8. **STATE_SEAL**：取代長篇交接詞，ssot 指向六檔，`rejected_options`／`tech_debts` 帶 RJ／K 編號。

---

## 三、決策記錄

*(D-198~D-343 保留原樣)*

| 編號 | 決策 | 說明 |
|---|---|---|
| D-144 | W 包擴充並中性化 | W-00~W-07；拔除特定專案字眼。 |
| D-145 | 專案紀錄檔改名 WF-* | 對齊 AI-G；P-02/03/12/13 廢除並轉型。 |
| D-146 | 體系代管專案索引生成 | `setup-structure.yml` 增加 Step 7.5 與 9。 |
| D-147 | 腳本防呆 | 加入 `if [ -d "docs/projects" ]` 避免空目錄崩潰。 |
| D-148 | R-00 F-007 語法補齊 | 納入 REGEX 與 REPLACE_ALL。 |
| D-149 | `entity-registry.js` code 留待專案 | 屬業務邏輯，由專案對話處理。 |
| D-150 | FSM 改 `fallback_handler` | T99 留 transitions 無法物理保證最後匹配。 |
| D-151 | 加 `boot_check` 開場版本比對 | 抓「改過四檔卻未結案」；口頭決策抓不到。 |
| D-152 | `records_pending` 單一真相在 HANDOFF | 其他三檔不重複加，避免多處真相。 |
| D-153 | SP 擴為六檔 | 新建 REJECTED／DEBT，皆 APPEND_ONLY。 |
| D-154 | F-004 加四反引號例外 | md 內含 YAML 時外層用 ````。 |
| D-155 | FastDev 自檢主走 CI、次走提取式 | 無 CI 時提取原文＋Ken 抽查。 |
| D-156 | R-05-dsl-map 新增 | 36 條映射：30 DSL、4 散文、2 部分 DSL。 |

**決策明細**：被否決方案見 `SYSTEM-REJECTED.md`（RJ-xxx）；技術債見 `SYSTEM-DEBT.md`（K-xx／C-xx）。

---

## 四、關聯檔案

| 主題 | 檔案 |
|---|---|
| 體系層交接 | SYSTEM-HANDOFF.md |
| 任務進度 | PROGRESS.md |
| 被否決方案台帳 | SYSTEM-REJECTED.md |
| 技術債台帳 | SYSTEM-DEBT.md |
| 檔案清單索引 | STATE-INDEX.md |
| 身份與信仰 | I-00-identity.md（合成物 IP） |
| 啟動引導 | A-00-guide.md（合成物 AP） |
| 核心規則 | R-00-rules.md（合成物 RP） |
| 規則 DSL 映射 | R-05-dsl-map.md（合成物 RP） |
| 使用者畫像 | U-00-profile.md（合成物 UP） |

/* ═══ EOF ═══
F: AI-General/docs/records/SYSTEM-CONCLUSION.md
V: 1792600033
W: ai-system-20261021-006#11
═══ */
<!-- === FILE END: AI-General/docs/records/SYSTEM-CONCLUSION.md === -->

<!-- === FILE START: AI-General/docs/records/SYSTEM-REJECTED.md === -->

## 📄 AI-General/docs/records/SYSTEM-REJECTED.md

# SYSTEM-REJECTED（被否決方案台帳）

用途：記錄被否決的方案與原因，供跨對話 AI 避免重提。
模式：APPEND_ONLY（只追加，不改舊條目）。
上限：無（追加式，長期累積）。

---

## 一、台帳

| 編號 | 主題 | 否決原因 | 來源對話 | 日期 |
|---|---|---|---|---|
| RJ-001 | v1.4 雙檔制（HANDOFF＋LOGS） | 與 Ken「records 維持四檔」指示衝突 | ai-system-20261021-006 | 2026-10-21 |
| RJ-002 | v1.4 四檔合併為 LOGS 單檔 | 同上，且 Ken 明確要求不併入 | ai-system-20261021-006 | 2026-10-21 |
| RJ-003 | 草稿 SEAL（每輪寫 draft_seal） | 每輪寫入過於頻繁，違 UP「一次交付完」偏好 | ai-system-20261021-006 | 2026-10-21 |
| RJ-004 | T99 留在 transitions 陣列 | 無法物理保證「最後才匹配」；改 fallback_handler | ai-system-20261021-006 | 2026-10-21 |
| RJ-005 | 純 B 範式（只寫不變式、刪流程） | 會造成交接順序無人管（資料遺失風險） | ai-system-20261021-006 | 2026-10-21 |

---

## 二、追加規則

| 時機 | 動作 |
|---|---|
| 對話中出現被否決方案 | AI 主動建議加入本表 |
| 使用者說「記錄否決」 | AI 追加一條，編號接續 |
| 與 SYSTEM-CONCLUSION 衝突 | 以本表為準（否決事實優先） |

---

## 三、編號規則

- 格式：`RJ-{三位數}`，從 001 起。
- 一經發布不改號；廢除時加註「已廢除，見 RJ-xxx」，不刪列。

/* ═══ EOF ═══
F: AI-General/docs/records/SYSTEM-REJECTED.md
V: 1792600030
W: ai-system-20261021-006#9
═══ */
<!-- === FILE END: AI-General/docs/records/SYSTEM-REJECTED.md === -->

<!-- === FILE START: AI-General/docs/records/SYSTEM-DEBT.md === -->

## 📄 AI-General/docs/records/SYSTEM-DEBT.md

# SYSTEM-DEBT（技術債台帳）

用途：記錄體系與專案的技術債，供跨對話追蹤。
模式：APPEND_ONLY（只追加，不改舊條目）。
上限：無（追加式，長期累積）。

---

## 一、台帳

| 編號 | 類別 | 描述 | 優先級 | 狀態 | 來源對話 |
|---|---|---|---|---|---|
| K-01 | 體系 | F-005 與 I／A／U／W／SP 內文 Version 矛盾 | 中 | 暫不處理（D-342） | ai-system-20261021-004 |
| C-01 | 專案 | 專案程式碼 END OF FILE 補齊（約 44 檔） | 低 | 待處理 | ai-system-20261021-004 |
| K-03 | 體系 | 36 條映射完成後，R-00 契約表加 DSL 欄 | 中 | 待處理 | ai-system-20261021-006 |

---

## 二、追加規則

| 時機 | 動作 |
|---|---|
| 對話中發現技術債 | AI 主動建議加入本表 |
| 使用者說「記錄技術債」 | AI 追加一條，編號接續 |
| 技術債解決 | 狀態改「已解決」，加註解決對話編號 |

---

## 三、編號規則

- 格式：`K-{二位數}`（體系）／`C-{二位數}`（專案），各自連續。
- 一經發布不改號；廢除時加狀態「已廢除」，不刪列。

---

## 四、優先級定義

| 級別 | 定義 |
|---|---|
| 高 | 會導致資料遺失或死鎖 |
| 中 | 影響可維護性或一致性 |
| 低 | 不影響運作，屬清理性質 |

/* ═══ EOF ═══
F: AI-General/docs/records/SYSTEM-DEBT.md
V: 1792600031
W: ai-system-20261021-006#9
═══ */
<!-- === FILE END: AI-General/docs/records/SYSTEM-DEBT.md === -->

<!-- === FILE START: AI-General/docs/records/STATE-INDEX.md === -->

## 📄 AI-General/docs/records/STATE-INDEX.md

# STATE-INDEX

> 生成時間：2026-10-22 13:36:11 (HKT)

## 一、專案總覽
| 專案顯示名稱 | code | domain | package_no | repo |
|---|---|---|---|---|
| 家庭 webapp | webapp | W | 90 | tlee20860-dotcom/AI-WEB-APP-Family |
| AI-OS 系統 | ai-os | E | 91 | tlee20860-dotcom/AI-OS |

## 二、檔案清單
| Repo | 檔案 | Version | Writer |
|---|---|---|---|
AI-General|docs/records/STATE-INDEX.md|1792643676|github-actions[bot]
AI-kernel|docs/r-rules/R-03-terms.md|1792606003|ai-system-20261022-007#72#3
AI-kernel|docs/r-rules/R-00-rules.md|1792606002|ai-system-20261022-007#72#2
AI-kernel|docs/a-guide/I-00-identity.md|1792606001|ai-system-20261022-007#72#1
AI-kernel|docs/review/ROLE-BOUNDARY.md|1792605003|ai-system-20261022-007#70#2
AI-kernel|docs/review/REVIEW-FORMAT.md|1792605003|ai-system-20261022-007#70#1
AI-kernel|docs/review/CHK-INDEX.md|1792605003|ai-system-20261022-007#70#3
AI-kernel|docs/review/CHK-E.md|1792605003|ai-system-20261022-007#70#4
AI-General|docs/records/SYSTEM-HANDOFF.md|1792600038|ai-system-20261021-006#15
AI-General|docs/records/PROGRESS.md|1792600037|ai-system-20261021-006#15
AI-General|docs/records/SYSTEM-CONCLUSION.md|1792600033|ai-system-20261021-006#11
AI-kernel|docs/a-guide/A-00-guide.md|1792600032|ai-system-20261021-006#10
AI-General|docs/records/SYSTEM-DEBT.md|1792600031|ai-system-20261021-006#9
AI-General|docs/records/SYSTEM-REJECTED.md|1792600030|ai-system-20261021-006#9
AI-kernel|docs/r-rules/R-05-dsl-map.md|1792600027|ai-system-20261021-006#6
AI-kernel|docs/u-profile/U-00-profile.md|1792600026|ai-system-20261021-006#5
AI-kernel|docs/skills/W/W-07-errors.md|1792581008|ai-system-20261021-004#8
AI-kernel|docs/skills/W/W-06-sop.md|1792581007|ai-system-20261021-004#7
AI-kernel|docs/skills/W/W-05-schema.md|1792581006|ai-system-20261021-004#6
AI-kernel|docs/skills/W/W-04-pwa.md|1792581005|ai-system-20261021-004#5
AI-kernel|docs/skills/W/W-03-esmodule.md|1792581004|ai-system-20261021-004#4
AI-kernel|docs/skills/W/W-02-functions.md|1792581003|ai-system-20261021-004#3
AI-kernel|docs/skills/W/W-01-firebase.md|1792581002|ai-system-20261021-004#2
AI-kernel|docs/skills/W/W-00-startup.md|1792581001|ai-system-20261021-004#1
AI-kernel|docs/r-rules/R-04-learning.md|1792575600|ai-system-20261021-003#61
AI-kernel|docs/r-rules/R-02-patterns.md|1792575600|ai-system-20261021-003#61

/* ═══ EOF ═══
F: AI-General/docs/records/STATE-INDEX.md
V: 1792647371
W: github-actions[bot]
═══ */

<!-- === FILE END: AI-General/docs/records/STATE-INDEX.md === -->
