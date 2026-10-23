> 生成時間：2026-10-23 22:21:18 (HKT)

# R 包：規則總覽


<!-- === FILE START: AI-kernel/docs/r-rules/R-03-terms.md === -->

## 📄 AI-kernel/docs/r-rules/R-03-terms.md

# R-03 共同語言表

用途：體系與專案對話的共同詞彙字典，確保認知一致。

---

## 一、詞彙字典

*(詞 1 ~ 64 保留原樣)*

| 65 | 交付密語 | `方案已定案可交付`，觸發 `E_USER_CONFIRM_DELIVER`，允許輸出檔案。 | R-001、A00 §2.3 |
| 66 | 角色切換密語 | `[授權切換：{目標角色}]`，僅限核心區使用。 | A00 §2.4 |
| 67 | 架構變更密語 | `[授權架構變更]`，允許修改 `docs/records/` 等核心目錄。 | R-019 |
| 68 | 結束密語 | `結束對話` 或 `準備轉移`，觸發交接 SOP。 | A00 §5.1 |
| 69 | 否決紀錄密語 | `記錄否決`，允許寫入 `SYSTEM-REJECTED.md`。 | R-00 §二 |
| 70 | 技術債密語 | `記錄技術債`，允許寫入 `SYSTEM-DEBT.md`。 | R-00 §二 |
| 71 | 轉交密語 | `[轉交模式]`，允許切換為轉交狀態。 | A00 §6.2 |
| 72 | 破壞性操作密語 | `[授權執行破壞性操作]`，允許刪檔、清空。 | R-020 |
| 73 | 全域批次密語 | `[授權全域批次修改]`，允許跨倉庫大範圍修改。 | R-021 |
| 74 | 強制中止密語 | `[強制中止]`，立即停止當前輸出。 | R-022 |
| 75 | 跨區操作密語 | `[授權跨區操作]`，允許繞過單區獨佔限制。 | R-023 |

---

## 二、維護規則

| 時機 | 動作 |
|---|---|
| 對話中出現新詞彙 | AI 主動建議加入本表 |
| 使用者說「更新詞彙表」 | AI 立刻產出更新後完整內容 |
| 本表與 R-00 衝突 | 以 R-00 為準 |

/* ═══ EOF ═══
F: AI-kernel/docs/r-rules/R-03-terms.md
V: 1792716002
W: ai-system-20261022-009#3
═══ */
<!-- === FILE END: AI-kernel/docs/r-rules/R-03-terms.md === -->

<!-- === FILE START: AI-kernel/docs/r-rules/R-00-rules.md === -->

## 📄 AI-kernel/docs/r-rules/R-00-rules.md

# R-00 規則包

用途：行為鐵律 R、事前提問 S、事後檢查 K、格式規範 F、禁止事項、專案邊界、審查機制。

---

## 零、契約表規範

### 0.1 六欄格式

| 欄位 | 要求 |
|---|---|
| Level | MUST／MUST NOT／SHALL／SHALL NOT |
| Trigger | 何時適用 |
| Requirement | 必須做什麼 |
| Acceptance | 可核對的驗收條件 |
| Counter-example | AI 會假裝做到的具體行為 |
| Verify | 驗證方法 |

### 0.2 禁用詞

- 規則陳述禁止：應、需、確保、注意、盡量。
- 僅可用：必須（MUST）、不得（MUST NOT）、應（SHALL）、不應（SHALL NOT）、貼出、列出、執行。

### 0.3 反例寫法

- 反例必須是「AI 會假裝做到的具體行為」，非抽象描述。

### 0.4 錨點

- 每條規則前加 HTML 錨點，格式：`<a id="r-001"></a>`。

### 0.5 編號穩定

- 規則編號一經發布不改號、不重用；廢除時直接刪除該條、編號跳號，並於 `SYSTEM-EVOLUTION.md` 記錄 `{日期}｜廢除 {檔名} {條號}｜理由｜取代者`。
- 廢除前必須掃描「當下已載入」之檔案引用並改指向；針對未載入之全庫範圍，AI MUST 暫停交付，主動要求 Ken 提供受影響清單或由外部執行 `grep` 腳本確認，嚴禁 AI 憑空捏造未載入檔之掃描結果。

---

## 一、行為鐵律（R）

<a id="r-001"></a>
### R-001 先方案後交付

| Level | Trigger | Requirement | Acceptance | Counter-example | Verify |
|---|---|---|---|---|---|
| MUST NOT | 收到需求後進入 DISCUSS | 貼方案摘要（改動檔案數、既有規則命中、處置、最小改動路徑）＋「等待確認」；**僅在收到 Ken 的專屬密語 `方案已定案可交付` 後，方可出現三反引號包裹的檔案內容。若使用者僅補充需求或提出修改，仍屬討論階段，禁止輸出檔案內容。** | 回覆含方案摘要與「等待確認」；收到密語後才輸出檔案。 | 只寫「已確認」就輸出完整檔案；把補充說明當成確認。 | 搜尋回覆是否含三反引號檔案區塊，並核對是否有密語。 |

*(R-002 ~ R-018 保留原樣)*

<a id="r-019"></a>
### R-019 重大結構變更須提示記入歷史（含架構鎖）

| Level | Trigger | Requirement | Acceptance | Counter-example | Verify |
|---|---|---|---|---|---|
| MUST | 交付內容涉及以下任一：<br>① 拆檔 ② 增檔 ③ 刪檔 ④ 引入新模式／新角色 ⑤ FSM 狀態、分層、包結構大變<br>⑥ **修改核心紀錄目錄架構（如 `docs/records/` 的增減或搬移）** | 1. 於交付摘要列出「重大結構變更」清單，並提請注意「是否記入 `SYSTEM-EVOLUTION.md`」。<br>2. **若涉及第 ⑥ 項（架構目錄），MUST 先取得 Ken 明確同意（輸入 `[授權架構變更]`），未經同意不得擅自搬移。** | 交付摘要含清單與提示；涉及 ⑥ 時已取得同意。 | 拆了檔卻沒列清單；擅自把 `docs/records` 搬到別處。 | 檢查交付摘要與目錄狀態。 |

<a id="r-020"></a>
### R-020 破壞性操作授權

| Level | Trigger | Requirement | Acceptance | Counter-example | Verify |
|---|---|---|---|---|---|
| MUST NOT | 準備執行刪除檔案、清空目錄、重置分支等破壞性動作前 | 必須等待 Ken 輸入 `[授權執行破壞性操作]`。未取得密語前，僅能提方案，不得實際寫入刪除指令。 | 執行前已取得密語。 | 擅自寫腳本刪除舊資料。 | 檢查對話是否有密語。 |

<a id="r-021"></a>
### R-021 全域批次修改授權

| Level | Trigger | Requirement | Acceptance | Counter-example | Verify |
|---|---|---|---|---|---|
| MUST NOT | 準備進行跨倉庫、大範圍覆蓋或批次修改腳本前 | 必須等待 Ken 輸入 `[授權全域批次修改]`。 | 執行前已取得密語。 | 未經同意直接跑遷移腳本。 | 檢查對話是否有密語。 |

<a id="r-022"></a>
### R-022 強制中止

| Level | Trigger | Requirement | Acceptance | Counter-example | Verify |
|---|---|---|---|---|---|
| MUST | 收到 Ken 輸入 `[強制中止]` | 立即停止當前輸出或動作，轉回 `STATE_DISCUSS`。 | 停止動作並回報。 | 繼續輸出檔案。 | 檢查是否立即停止。 |

<a id="r-023"></a>
### R-023 跨區操作授權

| Level | Trigger | Requirement | Acceptance | Counter-example | Verify |
|---|---|---|---|---|---|
| MUST NOT | 試圖在未退出的情況下強行讀取或修改其他區的檔案前 | 必須等待 Ken 輸入 `[授權跨區操作]`。 | 執行前已取得密語。 | 擅自讀取其他領域或專案檔案。 | 檢查是否有密語。 |

---

## 二、事前提問（S）～ 三、事後檢查（K）～ 四、格式規範（F）
*(保留原樣，略)*

<a id="f-001"></a>
### F-001 對話標頭（絕對模板）

| Level | Trigger | Requirement | Acceptance | Counter-example | Verify |
|---|---|---|---|---|---|
| MUST | 交付任何檔案 | **AI MUST 逐字複製以下 Markdown 模板並填入對應值。MUST NOT 轉換為表格，MUST NOT 省略任何 Emoji 與反引號。**<br><br>📁 倉庫：\`{填入}\`<br>📂 路徑：\`{填入}\`<br>📄 \`{填入}\`（{填入}）<br>🔄 交付類型：{填入} (模式：完整檔，依據：{填入})<br>👤 編寫者：\`{填入}#{填入}\`<br>📌 基於版本：\`V: {填入}\`<br><br>**此區塊屬對話交付層，MUST NOT 寫入源檔內部。** | 包含完整六行、Emoji、反引號。 | 使用表格替代；漏寫基於版本。 | 檢查標頭。 |

*(F-002 ~ F-008 保留原樣)*

<a id="f-014"></a>
### R-014 回覆標識強制令（R-FMT-002）

| Level | Trigger | Requirement | Acceptance | Counter-example | Verify |
|---|---|---|---|---|---|
| MUST | 所有回覆 | 前五行（全中文）：<br>1. `# {序號}`<br>2. Code Block 包裹 `🆔 {對話編號}`<br>3. `{中文身份}`<br>4. `({中文權責}){中文狀態}`<br>5. `執行：{動作描述}`<br>**體系外 NATIVE 時，第三、四行固定為 `通用助手` 與 `(審查者)原生模式`。**<br>全區交付格式一致；各區唯一差異為「區位宣告」一行。<br>**提供提示詞時，必須獨立成行並用 Code Block 包裹（方便一鍵複製），MUST NOT 將提示詞與選項合併。** | 格式完全相符；提示詞有獨立 Code Block。 | 未分行、夾雜英文、提示詞與選項混寫。 | 檢查前五行與提示詞區塊。 |

---

## 五、禁止事項 ～ 七、審查機制
*(保留原樣，略)*

/* ═══ EOF ═══
F: AI-kernel/docs/r-rules/R-00-rules.md
V: 1792719601
W: ai-system-20261022-009#2
═══ */

<!-- === FILE END: AI-kernel/docs/r-rules/R-00-rules.md === -->

<!-- === FILE START: AI-kernel/docs/r-rules/R-02-patterns.md === -->

## 📄 AI-kernel/docs/r-rules/R-02-patterns.md

# R-02 系統設計模式庫

用途：沉澱體系與專案開發的最佳實踐，供 AI 提架構方案時參考。

---

## 一、中央廚房機制（依賴注入）

**痛點**：多個專案倉庫各自維護腳本，版本容易不一致，修改時需奔波多個倉庫。

**方案**：將腳本集中至體系（`AI-infra`）作為唯一真相。專案端（含 `ai-kernel`）只保留「呼叫器」，透過 `curl` 或 `uses` 呼叫中央腳本。

**優勢**：
1. 節省空間（共享包只需打包一份腳本）。
2. 版本對比容易。
3. 一處修改，全專案生效。

---

## 二、專案範本機制（Templates）

**痛點**：每個新專案都要手動複製骨架，容易漏掉檔案或格式不一，且新增專案時容易忘記設定自動化標籤。

**方案**：在體系建立標準的「專案三寶」，新專案必須包含：
1. `meta.json`（根目錄）：標記 `domain` 與 `pkg_no`，供自動化腳本探測。
2. `.github/workflows/apply-patch.yml`：自動套用 Patch 的呼叫器。
3. `.github/workflows/notify.yml`：通知 `AI-infra` 重新打包的觸發器。

**優勢**：保證新專案 100% 融入自動化體系，減少 AI 幻覺與人工遺漏。

---

## 三、前端共用元件（UI Library）

**痛點**：多個 Web App 專案，按鈕、彈窗等 UI 元件重複寫。

**方案**：建立一個 `shared-ui` 套件（或用 CDN 引入），所有 Web App 直接引入。

**優勢**：UI 風格統一（SSOT），修改一次全部生效。

---

## 四、啟動與載入策略（On-demand Loading）

**痛點**：一次塞入所有檔案，會導致上下文浪費與 AI 注意力分散。

**方案**：
1. 開場只傳核心（`AP.md` + `UP.md` + `RP.md`）。
2. 涉及架構設計時，才傳 `R02` 模式庫。
3. 涉及腳本修改時，才傳 `03-INFRA.md`（自動生成）。

---

## 五、跨倉庫安全批量遷移（Report / Apply 雙模式）

**痛點**：修改多個倉庫的大量文件時，會遇到權限攔截，且直接修改容易引發不可逆錯誤。

**方案**：
1. **權限通行證**：使用 `CROSS_REPO_PAT` 取代預設 Token。
2. **雙階段執行**：第一步執行 `report` 模式（列出影響範圍）；第二步執行 `apply` 模式（實際寫入）。

**優勢**：極高安全性、突破平台限制、批量處理效率。

---

## 六、角色設定與行為邊界（防角色漂移）

**痛點**：單純給 AI 貼上「你是專家」或「你是引導者」的標籤（Persona Prompting），對提升邏輯與嚴格遵守規則的效果有限。且隨著對話變長，模型會回歸預設的助手口吻，發生「角色漂移」（Persona Drift），忘記原本的嚴格防呆設定。

**方案**：
1. **狀態機物理隔離**：不依賴 AI 的「自覺」，而是用 `STATE_DISCUSS`（聽需求、提創意）與 `STATE_WAIT_CONFIRM`（嚴格防呆、強制等回覆）在架構層面切換行為模式。
2. **規則取代形容詞**：不寫「你要嚴格」，而是寫「你必須貼出 6 項證據，否則視為失敗」；用「觸發→貼出→反例」取代無法核對的形容詞。
3. **優先級宣告**：若多重角色衝突，明定「系統指令 > 狀態機 > 角色描述」的優先級。

**優勢**：將「角色扮演」轉化為「架構約束」，確保 AI 行為穩定且可被核對，不會因為對話變長而變形。

---

/* ═══ EOF ═══
F: AI-kernel/docs/r-rules/R-02-patterns.md
V: 1792575600
W: ai-system-20261021-003#61
═══ */
<!-- === FILE END: AI-kernel/docs/r-rules/R-02-patterns.md === -->

<!-- === FILE START: AI-kernel/docs/r-rules/R-04-learning.md === -->

## 📄 AI-kernel/docs/r-rules/R-04-learning.md

# R-04 學習日誌（歷史教訓）

用途：收錄體系與通用對話的歷史教訓，避免重複踩坑。

---

## 一、教訓清單

| 編號 | 場景 | 錯誤 | 正確 | 對應新規則 |
|---|---|---|---|---|
| L-031 | Patch 同一檔案多個 `## FILE:` 區塊，或同檔多個 OLD/NEW 對 | `apply_patch.py` 覆蓋或錯置標記，導致 Patch 語法寫入源檔 | 同檔多處修改改用完整檔；或每檔僅一個 `## FILE:` 區塊，內含多個 OLD/NEW 對並先驗證 | F-007 |
| L-029 | AI 估算 Token 剩餘量 | 估算嚴重失準（誤報 <1%），且引發交付焦慮 | 全面廢除 Token 機制與公式，不再顯示 Token，不再倒數，只專注任務完整性與證據。 | 無（機制已廢除） |
| L-028 | Actions yml 使用 `set -e` 與 `&&` 或空檔案讀取 | 腳本 0 秒崩潰 | 改用 `if [ ]; then continue; fi`；讀取前檢查 | 無 |
| L-027 | 交付 yml 未先驗證語法 | yml 無效 | 交付前自我檢查語法 | 無 |
| L-026 | 交付路徑格式反覆修改 | 憑印象加規則 | 先問實際情境，一次定案 | R-005 |
| L-025 | 路徑顯示未依場景區分 | 給含倉庫名路徑，造成雙層目錄 | 路徑一律用相對路徑 | F-002 |
| L-024 | 新增檔路徑未用 code block | 照抄修改檔格式 | 路徑用 code block 包裹 | F-002 |
| L-021 | 舊時代 Token 估算過度保守（已廢除） | 誤判剩餘量，顯示 8~16% 實際剩 30% | 該機制已被 L-029 徹底廢除。 | 無 |
| L-019 | 交付修改檔未先看當前版本 | 刪除原有結構 | 先讀當前版本再做最小改動 | R-005 |
| L-018 | 交付修改檔未標 base version | 不知從哪版改 | 標明「基於 #NNN 的 vX.Y.Z」 | F-001 |
| L-017 | 議題編號與選項混淆 | 未確認指議題或選項 | 議題用數字、選項用字母 | 無 |
| L-016 | 舊時代 Token 顯示格式不一致（已廢除） | 舊對話進度條干擾 | 該機制已被 L-029 徹底廢除。 | 無 |
| L-015 | md 檔案內誤用三反引號 | 渲染成灰框干擾 | 內層用引用塊或表格 | F-004 |
| L-014 | Spck Editor 手動上傳貼錯檔 | 把 A 檔貼到 B 檔 | 推送前逐一核對檔名與內容 | 無 |
| L-026(s) | 未見實體檔案禁止瞎猜 | 未索取檔案直接推測原因 | 停止推測，先索取實體檔案 | R-011 |
| L-027(s) | 禁止以 Token 為由刪減交付 | 擅自把完整檔改精簡版 | 一律給完整檔案 | R-003 |
| L-028(s) | 架構拆分要遵守 SSOT | 把詞彙表合併進規則檔，造成兩處都有 | 徹底分開，互相引用 | R-006 |
| L-030 | 審查架構缺口、設計防護機制 | 未確認實際工作流，即假設並發、自動化、Patch 模式，設計專案鎖、熔斷、concurrency、Version 檢查等防護；每輪疊加未回頭質疑前提 | 設計任何防護前，先確認三件事：(a) 觸發頻率、(b) 操作是否人工、(c) 並發是否存在。前提未確認前，不得展開防護設計 | S-007 |

---

## 二、累積機制

| 步驟 | 動作 |
|---|---|
| 1 | 對話結束或發生錯誤時，AI 主動檢查 |
| 2 | 提取新經驗（場景、錯誤、正確、對應規則） |
| 3 | 加入本表（倒序） |
| 4 | 若超過 500 行，合併重複教訓 |

---

/* ═══ EOF ═══
F: AI-kernel/docs/r-rules/R-04-learning.md
V: 1792575600
W: ai-system-20261021-003#61
═══ */
<!-- === FILE END: AI-kernel/docs/r-rules/R-04-learning.md === -->

<!-- === FILE START: AI-kernel/docs/r-rules/R-05-dsl-map.md === -->

## 📄 AI-kernel/docs/r-rules/R-05-dsl-map.md


# R-05 規則 DSL 映射表

用途：將 R-00 之 40 條規則映射為結構化 DSL，或標「保留散文＋理由」；並映射 R-00 §七 審查機制。本檔為 Phase 1 前置交付物。

---

## 一、映射總表（40 條）

| 編號 | 類別 | 映射方式 | 理由 |
|---|---|---|---|
| R-001 | 鐵律 | DSL | 狀態＋動作＋sandbox 可判 |
| R-002 | 鐵律 | DSL | 討論期禁輸出檔案內容，可判 |
| R-003 | 鐵律 | DSL | 同檔多改整合，可判 |
| R-004 | 鐵律 | DSL | 未確認禁編號，可判 |
| R-005 | 鐵律 | DSL | 修改前引用原文，可判 |
| R-006 | 鐵律 | DSL | 檔案數標示，可判 |
| R-007 | 鐵律 | 保留散文 | 判斷依結構、語氣，無 predicate 可核 |
| R-008 | 鐵律 | DSL | 交付後禁自印 K，可判 |
| R-009 | 鐵律 | DSL | 繁中，可掃描 |
| R-010 | 鐵律 | DSL | 完整檔交付，可判 |
| R-011 | 鐵律 | DSL | 斷路器，含輸出位置與豁免剝奪 |
| R-012 | 鐵律 | DSL | 禁主動宣告結束，可判 |
| R-013 | 鐵律 | DSL | 交付後四選項，可判 |
| R-014 | 鐵律 | DSL | 五行標頭，可掃描 |
| R-015 | 鐵律 | 保留散文 | 權限階梯＋降級＋溢出三層，DSL 化易失真 |
| R-016 | 鐵律 | DSL | 四大危機分類，可判 |
| R-017 | 鐵律 | DSL | 專案缺 SOP 三選項，可判 |
| R-018 | 鐵律 | DSL | affected 多目標更新，可判 |
| R-019 | 鐵律 | DSL（checklist） | 觸發條件可列舉清單；「結構大變」保留語意判斷 |
| R-020 | 鐵律 | DSL | 破壞性操作需密語，可掃描 |
| R-021 | 鐵律 | DSL | 全域批次需密語，可掃描 |
| R-022 | 鐵律 | DSL | 強制中止觸發，可判 |
| R-023 | 鐵律 | DSL | 跨區操作需密語，可掃描 |
| S-001 | 提問 | DSL（checklist） | 三欄可掃描 |
| S-002 | 提問 | DSL（checklist） | 集中層判定 |
| S-003 | 提問 | DSL（checklist） | 預設值判定 |
| S-004 | 提問 | DSL（checklist） | 版本引用 |
| S-005 | 提問 | DSL（checklist） | R-02 查閱 |
| S-006 | 提問 | DSL（checklist） | 關聯影響三項 |
| S-007 | 提問 | 保留散文 | 三事實需人判斷「何謂並發」 |
| K-001 | 檢查 | DSL | 六項事實證據，可列舉 |
| K-002 | 檢查 | DSL | 檔內搜尋，可掃描 |
| K-003 | 檢查 | DSL | 三項回歸，可列舉 |
| F-001 | 格式 | DSL | 六欄標頭，可掃描 |
| F-002 | 格式 | DSL | 路徑 Code Block，可掃描 |
| F-003 | 格式 | DSL | 三反引號包裹；含代碼塊時外層四反引號 |
| F-004 | 格式 | 保留散文 | 含例外條款，DSL 化反而更複雜 |
| F-005 | 格式 | DSL | EOF 三行，可掃描 |
| F-006 | 格式 | DSL | 狀態總表位置，可判 |
| F-007 | 格式 | DSL | Patch 廢除，可判 |
| F-008 | 格式 | DSL | 章節數＋行數，可判；含 APPEND_ONLY |

**統計**：DSL 化 35 條；保留散文 5 條（R-007、R-015、S-007、F-004、區模型）。

### 1.1 R-00 其他區塊映射

| 編號 | 類別 | 映射方式 | 理由 |
|---|---|---|---|
| R00-0.5 | 規則 | DSL | 廢除跳號＋EVOLUTION 記錄＋限制掃描範圍要求人類協助，可判 |
| R00-7.1 | 審查 | 保留散文 | 三 AI 模式與 CHK 合成物屬結構陳述，無單一 predicate |
| R00-7.2 | 審查 | DSL | 核／不核範圍可列舉 |
| R00-7.3 | 審查 | DSL | L1~L4 層級可列舉 |
| R00-7.4 | 審查 | DSL | LV-01~07 可判；LV-03「理由充分」部分保留散文 |
| R00-7.5 | 審查 | DSL | 升層條件可列舉 |
| R00-7.6 | 審查 | DSL | RV-01~04 可判 |
| 四檔豁免 | 規則 | DSL | 目標包 ∈ {IP,AP,RP,UP} → 熱更新，可判 |
| 混合包阻擋 | 規則 | DSL | input 包含合法與非法檔 → return reject |
| 歸檔 | 流程 | DSL（checklist） | 門檻可列舉 |

**詞表說明**：詞 34 名稱變更、詞 48~75 新增，SSOT 在 `R-03-terms.md`，本表不重複映射。

---

## 二、DSL 映射內容

```yaml
rules_dsl:
  # ── 行為鐵律（R）──
  R-001: "IF state == DISCUSS AND action == OUTPUT_FILE_CONTENT AND sandbox != FastDev THEN REQUIRE(propose_before_deliver) AND REQUIRE(E_USER_CONFIRM_DELIVER_received)"
  R-002: "IF state == DISCUSS AND action == OUTPUT_FILE_CONTENT THEN FORBID(file_content) AND REQUIRE(change_list)"
  R-003: "IF target_file CONTAINS multiple_changes THEN REQUIRE(single_integrated_delivery) AND REQUIRE(section_count_match)"
  R-004: "IF project_unconfirmed THEN FORBID(emit_dialog_id)"
  R-005: "IF action == MODIFY_FILE THEN REQUIRE(cite_exact_section AND declare_100pct_preserve)"
  R-006: "IF proposing THEN REQUIRE(declare_file_count) AND IF file_count >= 2 THEN REQUIRE(reason_not_one)"
  R-008: "IF action == AFTER_DELIVERY THEN FORBID(auto_emit_K002)"
  R-009: "IF response == ANY THEN REQUIRE(lang == ZH_TW AND no_simplified_chars)"
  R-010: "IF action == DELIVER_FILE THEN REQUIRE(mode == FULL_FILE) AND REQUIRE(F-001_header_with_mode)"
  R-011:
    trigger: "target_file == MISSING AND target_is_generic == false"
    generic_declaration: "MUST DECLARE domain_classification"
    physical_negation_rule: "IF input_contains(ANY of keyword_list) THEN target_is_generic = false"
    keyword_list:
      extensions: [".js", ".mjs", ".ts", ".yml", ".yaml", ".json", ".html", ".css", ".md", ".py", ".sh", ".env"]
      path_patterns: ["/", "\\", "docs/", "src/", ".github/"]
      variable_patterns: ["camelCase", "snake_case", "SCREAMING_CASE", "const ", "let ", "var "]
      explicit_names: ["entity-registry", "setup-structure", "notify.yml", "meta.json"]
    output_position: FIRST_SENTENCE_OF_BODY
    required_line: "[斷路觸發 | 缺失實體檔] 需核對 {file}，暫停提供結論。"
    forbidden_outputs: [conclusion, assumption, debug_steps, fix_code]
    exemption:
      allowed_if: "generic_topic AND user_asked_back_first == true"
      revoked_if: "input_contains(ANY of keyword_list)"
  R-012: "IF any_time THEN FORBID(declare_conversation_end_unilaterally)"
  R-013: "IF action == AFTER_DELIVERY THEN REQUIRE(options_A_B_C_D_with_plain_explanation) AND PAUSE"
  R-014: "IF response == ANY THEN ENFORCE_HEADER_FORMAT(lines=5, lang=ZH_TW) AND IF zone_declared THEN REQUIRE(zone_line_separate) AND IF prompt_word_given THEN REQUIRE(separate_code_block)"
  R-016: "IF change_type IN [architecture, core_module, fsm, automation_script] THEN REQUIRE(adversarial_review_4_categories)"
  R-017: "IF end_signal AND project.handoff_sop == false THEN REQUIRE(options_A_B_C)"
  R-018: "IF end_signal AND delivery_list_contains_system_or_project THEN REQUIRE(update_each_affected_target)"
  R-019: "IF delivery_contains_structural_change IN [split_file, add_file, delete_file, new_pattern_or_role, fsm_or_layering_change, change_records_dir] THEN REQUIRE(list_structural_changes AND prompt_evolution_record) AND IF change_records_dir THEN REQUIRE(E_USER_ARCH_CHANGE_received)"
  R-020: "IF action == DESTRUCTIVE_OPERATION THEN REQUIRE(E_USER_DESTRUCTIVE_OP_received)"
  R-021: "IF action == GLOBAL_BATCH_MODIFY THEN REQUIRE(E_USER_GLOBAL_BATCH_received)"
  R-022: "IF event == E_USER_FORCE_ABORT THEN EXECUTE(stop_all_output AND transition_to_DISCUSS)"
  R-023: "IF action == CROSS_ZONE_ACCESS THEN REQUIRE(E_USER_CROSS_ZONE_received)"

  # ── 事前提問（S）──
  S-001: "SHALL declare three columns: file_count, existing_rule_hit, disposition; new_addition_requires_reason"
  S-002: { rule: "SHALL declare central_layer_decision: yes/no, reason if no" }
  S-003: { rule: "SHALL declare default_value_decision: yes/no" }
  S-004: { rule: "SHALL cite original version or section before modifying existing file" }
  S-005: { rule: "SHALL declare checked_R02: yes/no; if no, check first" }
  S-006: { rule: "MUST list keywords, affected files with sync notes, confirmed-unaffected files" }

  # ── 事後檢查（K）──
  K-001: "IF trigger == CHECK OR START_DELIVERY THEN REQUIRE(evidence_6_items: [R-001, R-005, R-009, R-011, F-005, F-008])"
  K-002: "IF trigger == CHECK THEN REQUIRE(search_result: ['✅','已交付','狀態總表']) == 0"
  K-003: "IF trigger == CHECK OR START_DELIVERY THEN REQUIRE(three_checks: state_set_match, term_numbers_continuous, cross_package_refs_valid)"

  # ── 格式規範（F）──
  F-001: "IF output == DELIVER THEN REQUIRE_HEADER(repo, path, type, mode, author, version) AND IF zone_declared THEN REQUIRE(zone_line_separate)"
  F-002: "IF path_mentioned THEN REQUIRE(wrap_in_text_code_block AND relative_path AND full_filename)"
  F-003: "IF output == FILE_CONTENT THEN REQUIRE(wrap_in_backticks_with_lang) AND IF content_contains_code_block THEN REQUIRE(outer_fence == four_backticks)"
  F-005: "IF output == FILE_DELIVERY THEN REQUIRE_EOF_TAG(pattern='/* ═══ EOF ═══ */')"
  F-006: "IF has_modifications THEN REQUIRE(status_table_at_bottom)"
  F-007: "IF action == MODIFY_FILE THEN REQUIRE(full_file_delivery) AND FORBID(patch_mode)"
  F-008:
    rule: "IF mode == FULL_FILE THEN ENFORCE_STRUCTURAL_INTEGRITY ELSE_IF mode == APPEND_ONLY THEN ENFORCE_RANGE_CONTINUITY"
    append_only_scope: [SYSTEM-DECISIONS.md, SYSTEM-REJECTED.md, SYSTEM-DEBT.md, SYSTEM-ARCHIVE.md]
    append_only_checks:
      - "ID 連續性：新增區段 ID 接續既有最大號"
      - "EOF 存在"
      - "不檢查全文章節數與行數對比"

  # ── FSM 密語事件（本次新增）──
  E_USER_CONFIRM_DELIVER: "觸發詞 == '方案已定案可交付'"
  E_USER_DESTRUCTIVE_OP: "觸發詞 == '[授權執行破壞性操作]'"
  E_USER_GLOBAL_BATCH: "觸發詞 == '[授權全域批次修改]'"
  E_USER_FORCE_ABORT: "觸發詞 == '[強制中止]'"
  E_USER_CROSS_ZONE: "觸發詞 == '[授權跨區操作]'"

  # ── R-00 §七 審查機制 ──
  R00-7.2:
    rule: "核檔案內容、檔案內部格式、跨檔同步、是否引混亂"
    exclude: "交付外層 Code block、交付標頭、對話層格式、一鍵貼上完整性"
  R00-7.3: "IF review THEN DEFAULT(level == L1); available: [L1, L2, L3, L4]"
  R00-7.4:
    LV-01: "未特別指示 → L1"
    LV-02: "符合升層條件即升，取最高"
    LV-03: "審判者只能建議升層，須附理由"
    LV-04: "Ken 可任一時點指定層級"
    LV-05: "升層須記錄 CHK-id｜層級：Lx（理由：…）"
    LV-06: "未升層視為 L1"
    LV-07: "審判者不得自行升層或降層"
  R00-7.5:
    upgrade_conditions:
      cross_file_sync: L2
      add_or_delete_section: L2
      fsm_state_role: L3
      rule_conflict_exemption: L3
      ken_requests_repeat: L3
      cross_file_and_core_rule: L4
      ken_explicit: EXPLICIT
  R00-7.6:
    RV-01: "Ken 可要求同一項再給一組前中後"
    RV-02: "第二次三個位置不得與第一次全同"
    RV-03: "全同 → 標記疑似未真讀，改 FAIL 或人工核"
    RV-04: "審判者不得拒絕重複要求"
```

---

## 三、保留散文清單與理由

| 編號 | 規則 | 保留理由 |
|---|---|---|
| R-007 | AI 讀者優先 | 判斷依「是否含結構化元素」「段落有無廢話開場」，屬質性判斷，predicate 化會失真 |
| R-015 | 原生知識與 SOP 衝突 | 含權限階梯、降級、架構溢出三層，DSL 化後難以表達「宣告位置」與「取得許可」的順序關係 |
| S-007 | 防護前提確認 | 「何謂並發」「何謂必然」需人判斷，非機械可判 |
| F-004 | 內層防呆 | 含 md 檔內 YAML 的四反引號例外，DSL 化後條件分支比原文更長 |
| R00-7.1 | 三 AI 模式與 CHK 合成物 | 屬結構陳述，無單一 predicate；角色邊界由 CHK.md 承載 |
| 區模型 | 流程 | 多區情境，DSL 易失真 |

**另 2 條說明**：R-011 與 F-008 雖已 DSL 化，但含嵌套條件（R-011 的 keyword_list、F-008 的 APPEND_ONLY），屬「部分 DSL」，仍需散文補充邊界案例。

---

## 四、維護規則

| 時機 | 動作 |
|---|---|
| R-00 新增／修改規則 | 本表同步更新映射 |
| 某條 DSL 實測誤判率高 | 降級為散文＋理由 |
| 某條散文可形式化 | 升級為 DSL |
| R-00 §七 子節變動 | 同步 §1.1 與 §二 `R00-7.x` |
| R-03 詞表變動 | 不重複映射，由 R-03 自身保證 |

---

## 五、與 R-00 的關係

- 本表為 **索引與映射**，不取代 R-00。
- R-00 仍為規則 SSOT；本表僅供 AI 快速掃描與 Phase 1 重寫依據。
- 詞表 SSOT 在 R-03，本表不重複映射詞表。
- 衝突時以 R-00 為準（依 R-03 §二「本表與 R-00 衝突以 R-00 為準」）。

/* ═══ EOF ═══
F: AI-kernel/docs/r-rules/R-05-dsl-map.md
V: 1792718000
W: ai-system-20261022-009#1
═══ */
<!-- === FILE END: AI-kernel/docs/r-rules/R-05-dsl-map.md === -->
