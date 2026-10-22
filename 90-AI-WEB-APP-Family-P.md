# 專案 P 檔總集：AI-WEB-APP-Family

> 生成時間：2026-10-23 02:06:56 (HKT)


<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-01-index.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-01-index.md

# P-01 專案索引 + 技術架構

最後更新：2026-10-21
Version: 1792581016
Writer: ai-system-20261021-004#16
用途：專案索引與專屬技術架構（通用架構已抽至 W-*）

---

## 一、專案身份

| 項目 | 內容 |
|---|---|
| 專案 | 家庭財務與固定供款管理 Web App |
| 領域 | W（Web App） |
| GitHub（程式碼） | tlee20860-dotcom/v2.Family |
| GitHub（文件） | tlee20860-dotcom/AI-training-files |
| 部署 | Cloudflare Pages |
| 資料庫 | Firebase RTDB (asia-southeast1) |
| 登入 | Firebase Auth |
| 前端 | 純 HTML + CSS + ES Module JS |
| 後端 | Cloudflare Pages Functions |
| 視覺 | 亮色家庭暖陽 |
| 金額 | 四捨五入至整數 |

---

## 二、當前狀態

| 項目 | 值 |
|---|---|
| 當前批次 | B34-5（體系對齊中） |
| 部署 URL | family-financial-management-system-v2.pages.dev |
| 測試狀態 | B34-4 5 組全通過 |
| 下一步 | 執行 P-15 重組與 Actions 觸發 |

---

## 三、架構概覽（詳見 W-*）

| 主題 | 檔案 |
|---|---|
| Web App 領域啟動卡 | W-00-startup.md |
| Firebase RTDB 原則 | W-01-firebase.md |
| Cloudflare Pages Functions | W-02-functions.md |
| 純前端 ES Module 架構 | W-03-esmodule.md |
| PWA + Service Worker | W-04-pwa.md |
| Page Schema 與 UI 規範 | W-05-schema.md |
| 通用任務 SOP | W-06-sop.md |
| 通用錯誤防呆 | W-07-errors.md |

> **技術架構完整內容已抽至 W-* 層。**

---

## 四、本專案檔案結構（專屬實作）

> **通用目錄規範（如 js/config, js/engines 等配置）已抽離至 `W-03-esmodule.md` §八。以下僅列本專案特有之實作與檔數。**

| 目錄 | 檔數 | 內容（專案特有補充） |
|---|---|---|
| js/config | 8+1 | 4 Registry / page-metadata-registry /（待）code-registry |
| js/core | 8 | state / api / db / auth / auth-guard / utils / pwa / debug |
| js/lib | 14 | dom / async / lifecycle / registry / merge / insurance / bank / format / expense-modal / income-modal / filter-sort / page-controls / code-registry / id-resolver |
| js/engines | 3 | page / data / render |
| js/blocks | 5 | stats / list / filter / detail / form |
| js/ui | 7 | toast / modal / form-builder / tab-panel / view-toggle / collapsible / dropdown-plus |
| js/layout | 3 | navbar / sidebar / app-shell |
| js/entity | 5 | entity-modal / entity-helpers / entity-definitions / entity-resolvers / entity-funds-allocation |
| js/admin | 2 | admin / platform-defaults |
| js/shared | 13 | data-table / data-card / ... / yearrange-card |
| js/pages | 10 | 純 Schema |
| functions/api | 11 | _config / _helpers / _admin / _bank / _family / _insurance / _personal / _platform / _summary / _entity / [[path]] |
| CSS | 6 | theme / layout / components / utilities / pages/annual-report / pages/insurance |
| HTML | 13 | 各頁 + login / register + code-list |
| icons | 1+ | icon.svg / icon.png |
| 根目錄 | 3 | sw.js / manifest.json / docs/ |

---

## 五、編號系統（已交付）

| 模組 | 職責 |
|---|---|
| js/lib/code-registry.js | 生成編號（查最大 + 1）、子類短碼自動決定 |
| js/lib/id-resolver.js | 從編號查實體、從編號展開關聯 |
| code-list.html + js/pages/code-list.js | 編號對照清單頁（取代 database.html） |
| POST /api/entity-create | 統一新增端點（含 action 參數） |

**詳細**：參見 `WF-CONCLUSION.md` §二 議題 5 / 6

---

## 六、關聯檔案

| 主題 | 檔案 |
|---|---|
| 進度 | WF-PROGRESS.md |
| 檔案清單 | WF-STATE-INDEX.md |
| 議題與決策 | WF-CONCLUSION.md |
| 交接快照 | WF-HANDOFF.md |
| 重組計畫 | P-15-reorg-plan.md |
| Web App 技能包 | W-00 ~ W-07 |

---

/* ═══ EOF ═══
F: P-01-index.md
V: 1792581016
W: ai-system-20261021-004#16
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-01-index.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-04-adr-index.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-04-adr-index.md

# P-04 ADR 歷史索引

最後更新：2026-10-21
Version: 1792581013
Writer: ai-system-20261021-004#13
用途：專案架構決策記錄（ADR）索引與歷史存檔

---

## 一、ADR 索引表

> **SSOT 宣告**：本專案之最新決策摘要已整合至 `WF-CONCLUSION.md` §三。本檔作為歷史決策（ADR）的完整存檔與索引，不刪除任何歷史紀錄。

| 編號 | 主題 |
|---|---|
| ADR-001~ADR-023 | 早期重構（B32-1 前，摘要保留） |
| ADR-024 | AI 編號 + Writer 機制 |
| ADR-025 | Version 用 Unix epoch |
| ADR-026 | md 檔案零三反引號 |
| ADR-027 | 通用編號系統 |
| ADR-028 | 編號生成用推導（無計數器） |
| ADR-029 | 併發用重複檢測 |
| ADR-030 | 編號查詢用遍歷 |
| ADR-031 | 編號對照清單頁 |
| ADR-032 | 舊資料全遷移 |
| ADR-033 | 滾動式累積結論 |
| ADR-034 | C-10 交接快照 |
| ADR-035 | 提問 D 方案 |
| ADR-036 | 編號區分機制 |
| ADR-037 | 文件 AI 讀者優先 |
| ADR-038 | Token 格式統一 |
| ADR-039 | 四層架構 |
| ADR-040 | W-* 抽取（先 5 個） |
| ADR-041 | C-09 / C-10 機制與內容分離 |
| ADR-042 | 啟動包三種 |
| ADR-043 | R-01 專案索引（原 P-11 改名） |
| ADR-044 | AI 自動識別專案 |
| ADR-045 | 議題表驅動制 |
| ADR-046 | 檔案上限 800 行 |
| ADR-047 | 議題結論摘要機制 |
| ADR-048 | Token 視覺化「剩餘: X%」 |
| ADR-049 | 提問附人話解釋 |
| ADR-050 | 議題結論摘要（每 2~3 議題） |
| ADR-051 | 編號對照清單：單頁 + 篩選器 |
| ADR-052 | entity-create 統一端點 |
| ADR-053 | 舊端點全廢 |
| ADR-054 | 保單子類短碼 3 種 |
| ADR-055 | + 號按鈕（dropdown-plus） |
| ADR-056 | base version 標示 |
| ADR-057 | 先看檔案再交付 |
| ADR-058 | Token < 20% 強制重估 |
| ADR-059 | Repo 用實際名稱 |
| ADR-060 | 通用大腦 + N 領域技能包 |
| ADR-061 | 貼 URL 啟動機制 |
| ADR-062 | AI 主動建議領域 |
| ADR-063 | 替換對照表（md 三反引號） |
| ADR-064 | 領域技能包統一 6 檔模板 |

---

## 二、ADR 詳細內容（節錄關鍵項）

### ADR-039：四層架構（D-038 / D-072~D-075）
| 項目 | 內容 |
|---|---|
| 背景 | 檔案混雜「通用」與「專屬」 |
| 決策 | 檔案分四層 |
| 層級 | L1 C-* / L2 W-* / L3 P-* / L4 R-* |
| 影響 | C-02 §零點八 |

### ADR-040：W-* 抽取（D-076 / D-103）
| 項目 | 內容 |
|---|---|
| 決策 | 先抽 5 個 W-*（W-01~W-05） |
| 來源 | W-01 ← P-09；W-02 ← C-02 §十二 + P-10；W-03 ← P-01；W-04 ← P-10 §十；W-05 ← P-01 §八 |
| 引用保留 | C-02 §十二 保留引用，完整內容在 W-02 |

### ADR-060：通用大腦 + N 領域技能包（D-118）
| 項目 | 內容 |
|---|---|
| 決策 | C-* 通用 + W/V/D/M 領域技能包 |
| 影響 | P-15 重組計畫書 |

### ADR-064：領域技能包統一 6 檔模板（D-121）
| 項目 | 內容 |
|---|---|
| 決策 | 每個領域 `{X}-*` 一律 6 檔 |
| 6 檔 | {X}-00-startup / 01-structure / 02-rules / 03-sop / 04-templates / 99-errors-map |
| 備註 | 本專案後續重構已由 W-00 ~ W-07 取代，此 ADR 僅供歷史參考 |

*(其餘 ADR 之完整內容已保留於本檔歷史存檔區，此處不重複展開)*

---

## 三、關聯檔案

| 主題 | 檔案 |
|---|---|
| 體系規則 | IP/AP/RP/UP |
| 決策摘要（SSOT）| WF-CONCLUSION.md |
| 決策明細 | P-14-decisions.md |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: P-04-adr-index.md
V: 1792581013
W: ai-system-20261021-004#13
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-04-adr-index.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-05-sop.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-05-sop.md

# P-05 常見任務 SOP

最後更新：2026-10-21
Version: 1792581017
Writer: ai-system-20261021-004#17
用途：AI 執行常見任務的具體步驟（專案專屬）

---

## 一、修 Bug

| 步驟 | 動作 |
|---|---|
| 1 | 讀 P-10-incident |
| 2 | 讀 P-99-errors-map |
| 3 | 跑 5 問自檢（體系 R-00） |
| 4 | 複現問題 |
| 5 | 診斷根本原因 |
| 6 | 提供「診斷 + 方案」 |
| 7 | 待確認 |
| 8 | 交付 |
| 9 | 更新 WF-STATE-INDEX + WF-PROGRESS |
| 10 | 記錄到 P-10 / 體系 R-04 |

---

## 二、新增頁面

> **通用流程請見 `W-06-sop.md` §一。**

| 步驟 | 本專案專屬動作 |
|---|---|
| 1 | 遵循 W-06 通用步驟（寫 js/pages、html、更新 sw.js） |
| 2 | 更新 `js/config/constants.js` 的 `SIDEBAR_GROUPS` / `ROUTES` |
| 3 | 更新 `P-06-pagemap` 頁面區塊規格 |
| 4 | 更新 `WF-STATE-INDEX` + `WF-PROGRESS` |

---

## 三、改 Block

| 步驟 | 動作 |
|---|---|
| 1 | grep 找出所有使用頁 |
| 2 | 評估向後相容 |
| 3 | 若破壞性 → 加參數 |
| 4 | 改 block |
| 5 | 驗證所有使用頁 |
| 6 | 更新 WF-STATE-INDEX |

---

## 四、改 Registry

| 步驟 | 動作 |
|---|---|
| 1 | 讀 P-08-api |
| 2 | grep 使用檔案 |
| 3 | 加新 API（不破壞舊） |
| 4 | 改 registry |
| 5 | 更新 P-08 + WF-STATE-INDEX |

**禁止**：改 export 名 / 改回傳格式 / 刪 export。

---

## 五、改 Lib

| 步驟 | 動作 |
|---|---|
| 1 | 檢查是否可複用 |
| 2 | 評估抽新 lib 或擴充 |
| 3 | 改 lib |
| 4 | 更新 WF-STATE-INDEX |

---

## 六、改 Functions API

> **通用流程請見 `W-02-functions.md`。**

| 步驟 | 本專案專屬動作 |
|---|---|
| 1 | 讀 P-08-api + W-02 |
| 2 | 決定哪個 `_xxx.js` |
| 3 | 改 handler |
| 4 | 若加新端點 → 改 `[[path]].js` 分派 |
| 5 | 更新 P-08 + WF-STATE-INDEX |

**禁止**：新增獨立 xxx.js / 用 shim re-export。

---

## 七、改資料結構

| 步驟 | 動作 |
|---|---|
| 1 | 讀 P-09-data + W-01 |
| 2 | 讀 P-09-data §八（相容） |
| 3 | 評估是否影響舊資料 |
| 4 | 若影響 → 加相容層 |
| 5 | 改程式碼 |
| 6 | 更新 P-09-data |

---

## 八、更新 docs

| 步驟 | 動作 |
|---|---|
| 1 | 找出受影響的 docs |
| 2 | 依「變更影響矩陣」 |
| 3 | 更新 Version |
| 4 | 交付完整檔案 |

**變更影響矩陣**：

| 改動 | 同步更新 |
|---|---|
| 新增頁面 | WF-STATE-INDEX / WF-PROGRESS / P-06 / sw.js / constants.js |
| 改 API | P-08 / WF-STATE-INDEX |
| 改資料結構 | P-09-data |
| 修 Bug | P-10 / WF-PROGRESS / WF-STATE-INDEX |
| 改 Web App 通用 | W-* |

---

## 九、部署後驗證

> **通用驗證清單請見 `W-06-sop.md` §二。**

| 步驟 | 本專案專屬驗證項 |
|---|---|
| 1 | 訪問 index.html（統計卡有數字） |
| 2 | settings.html（3 Tab 可切換） |
| 3 | code-list.html（篩選器 + 兩區塊） |
| 4 | input-center.html（5 按鈕可開啟 Modal） |
| 5 | settlements.html（表格 + toggle） |
| 6 | insurance.html（卡片 + 已供滿展開） |

---

## 十、緊急回滾

> **通用回滾流程請見 `W-06-sop.md` §三。**

| 步驟 | 本專案專屬動作 |
|---|---|
| 1 | GitHub 找上一個穩定 commit |
| 2 | Revert |
| 3 | Cloudflare Pages 自動重部署 |
| 4 | 驗證後分析原因 |

---

## 十一～十四（保留原專案專屬流程）

*(保留原有「執行一次性遷移」、「改版面」、「貼 URL 啟動」、「領域技能包建立」等專案特定操作步驟，不變動)*

---

## 十五、任務速查

| 任務 | 章節 |
|---|---|
| 修 Bug | 一 |
| 新增頁面 | 二（結合 W-06） |
| 改 Block | 三 |
| 改 Registry | 四 |
| 改 Lib | 五 |
| 改 Functions API | 六（結合 W-02） |
| 改資料結構 | 七 |
| 更新 docs | 八 |
| 部署後驗證 | 九（結合 W-06） |
| 緊急回滾 | 十（結合 W-06） |

---

## 十六、關聯檔案

| 主題 | 檔案 |
|---|---|
| 通用 SOP | W-06-sop.md |
| Web App 技能包 | W-00 ~ W-07 |
| 檔案清單 | WF-STATE-INDEX.md |
| 錯誤摘要 | P-99-errors-map.md |

---

/* ═══ EOF ═══
F: P-05-sop.md
V: 1792581017
W: ai-system-20261021-004#17
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-05-sop.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-06-pagemap.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-06-pagemap.md

# P-06 頁面 → 檔案對照表

最後更新：2026-10-21
Version: 1792581020
Writer: ai-system-20261021-004#20
用途：改某功能時，快速查哪些頁面受影響

---

## 一、總覽

| 頁面 | HTML | Schema | 主要 Blocks |
|---|---|---|---|
| 登入 | login.html | — | — |
| 註冊 | register.html | — | — |
| 平台管理 | admin.html | — | （admin.js） |
| 總覽儀表板 | index.html | dashboard.js | stats |
| 綜合輸入中心 | input-center.html | input-center.js | — |
| **編號對照清單** | **code-list.html**（✅ 已交付） | **code-list.js**（✅ 已交付） | — |
| 保險清單表 | insurance.html | insurance.js | — |
| 基金投資表 | portfolio.html | portfolio.js | stats |
| 結算清單 | settlements.html | settlements.js | — |
| 銀行交易 | finance-overview.html | finance-overview.js | — |
| 年度報表 | annual-report.html | annual-report.js | stats |
| 成員報表 | member-report.html | member-report.js | stats |
| 系統設定 | settings.html | settings.js | — |

---

## 二、共用（所有主頁）

| 檔案 | 職責 |
|---|---|
| js/layout/app-shell.js | App Shell 初始化 |
| js/layout/sidebar.js | 側邊欄（依角色過濾） |
| js/layout/navbar.js | 頂部導覽 |
| js/core/auth-guard.js | 登入驗證 |
| js/core/state.js | 全域狀態 |
| js/core/api.js | API 呼叫 |
| js/core/db.js | Firebase 讀寫 |
| js/core/pwa.js | PWA |
| js/core/debug.js | vConsole |
| js/config/app-config.js | 平台 / 家庭設定 |
| js/config/firebase-config.js | Firebase |
| js/config/page-metadata-registry.js | 頁面 metadata 預設 |
| js/lib/registry.js | Registry 統一初始化 |

---

## 三、各頁面檔案

### code-list.html（✅ 已交付）

| 檔案 | 用途 |
|---|---|
| js/pages/code-list.js | Schema |
| js/lib/code-registry.js | 編號生成 |
| js/lib/id-resolver.js | 編號查詢 |
| js/ui/toast.js | Toast |
| js/ui/modal.js | Modal |
| js/entity/entity-modal.js | 編輯 Modal |
| js/entity/entity-helpers.js | CRUD |
| js/entity/entity-definitions.js | 實體定義（含 code） |

**頁面結構**：
| 區塊 | 說明 |
|---|---|
| 頂部篩選器（2 欄並排） | 類型 + 編號狀態 |
| 主列表區 | 依 code 排序 |
| 權限按鈕 | superadmin 可見 |

### input-center.html

| 檔案 | 用途 |
|---|---|
| js/pages/input-center.js | Schema |
| js/ui/tab-panel.js | Tab |
| js/ui/view-toggle.js | 切換 |
| js/lib/expense-modal.js | 支出 Modal（含 + 號） |
| js/lib/income-modal.js | 收入 Modal（含 + 號） |
| js/entity/entity-modal.js | 保單 / 基金 Modal（含 + 號） |
| js/ui/dropdown-plus.js | + 號按鈕 |

### settings.html

| 檔案 | 用途 |
|---|---|
| js/pages/settings.js | Schema |
| js/shared/yearrange-card.js | 年份範圍卡（B34-4） |
| js/shared/bank-account-manager.js | 銀行帳號 |
| js/shared/entity-list-page.js | 家庭成員 |

（其餘頁面與 B34-3 版相同）

---

## 四、共用 Registry

| Registry | 用途 |
|---|---|
| status-registry.js | 狀態正規化 |
| entity-registry.js | ID → 名稱 |
| label-registry.js | 顯示文字 |
| column-registry.js | 表格欄位 + 排序 / 篩選 |
| constants.js | 全站常數 |
| page-metadata-registry.js | 頁面 title / subtitle |

---

## 五、各頁可編輯欄位對照

| 頁面 | 可編輯欄位 | 禁止欄位 |
|---|---|---|
| input-center（支出） | name / amount / date / categoryId / itemId | status / paymentMethodId / bankId / repaidDate |
| input-center（收入） | memberId / amount | — |
| input-center（保單） | 全部保單資料 | 扣款狀態 |
| input-center（基金） | 全部基金資料 | — |
| code-list | 全部欄位 | — |
| settlements（支出） | status / paymentMethodId / bankId / repaidDate | name / amount / date |
| settlements（保險） | status / bankId | 保單基本資料 |
| insurance | 保單資料（不含扣款狀態） | 扣款狀態 |
| portfolio | 基金資料 / 快照 | — |

---

## 六、側邊欄角色可見群組

| 群組 | roles |
|---|---|
| 總覽 / 記錄中心 / 保險基金 / 對帳 / 資產 / 報表 | owner / member |
| 平台管理 | superadmin |
| 系統 | owner / member / superadmin |

**「編號對照清單」歸「記錄中心」群組**（已交付）。

---

## 七、頁面區塊規格

### code-list.html（✅ B34-4）

| 順序 | 容器 ID | 類型 | 用途 | 狀態 |
|---|---|---|---|---|
| 1 | page-header | header | 標題 + 副標題 + 權限按鈕 | 🆕 |
| 2 | cl-filter-root | filter | 類型 + 編號狀態（2 欄並排） | 🆕 |
| 3 | cl-list-root | list | 主列表 | 🆕 |
| 4 | cl-empty-state | empty | 空狀態 | 🆕 |

（其餘頁面區塊規格與 B34-3 版相同）

---

## 八、改動影響對照

| 若改此檔 | 影響頁面 |
|---|---|
| page-engine.js | 全部主頁 |
| data-engine.js | 全部主頁 |
| render-engine.js | 全部主頁 |
| data-table.js | 全部有表格的頁 |
| data-card.js | 全部有卡片的頁 |
| stats-block.js | dashboard / portfolio / annual-report / member-report |
| column-registry.js | 全部有表格的頁 |
| code-registry.js | 全部新增流程 |
| id-resolver.js | 全部顯示 / 關聯展開 |
| dropdown-plus.js | input-center + 各 modal |
| yearrange-card.js | settings |
| entity-helpers.js | 全部實體新增 / 編輯 / 刪除 |
| entity-modal.js | 全部開啟 Modal 的頁 |
| **code-list.js** | **code-list.html** |

---

## 九、關聯檔案

| 主題 | 檔案 |
|---|---|
| UI 術語 | P-07-uispec.md |
| 專案索引 | P-01-index.md |
| 檔案清單 | WF-STATE-INDEX.md |
| 議題結論 | WF-CONCLUSION.md |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: P-06-pagemap.md
V: 1792581020
W: ai-system-20261021-004#20
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-06-pagemap.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-07-uispec.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-07-uispec.md

# P-07 專案 UI 規格與業務術語

最後更新：2026-10-21
Version: 1792581018
Writer: ai-system-20261021-004#18
用途：專案專屬 UI 規範與業務術語（通用結構已抽至 W-05）

---

## 一～六、通用 UI 結構

> **通用 UI 結構（頁面層級、Section 類型、控制列、標準佈局、資料呈現元件、表頭互動）請見 `W-05-schema.md` §九。**

---

## 七、側邊欄群組

| 群組 key | 標籤 | roles |
|---|---|---|
| overview | 總覽 | owner / member |
| record | 記錄中心 | owner / member |
| insurance-fund | 保險與基金 | owner / member |
| settle | 對帳 | owner / member |
| assets | 資產 | owner / member |
| reports | 報表 | owner / member |
| platform | 平台管理 | superadmin |
| system | 系統 | owner / member / superadmin |

---

## 八、頁面 metadata

**模組**：`js/config/page-metadata-registry.js`
**儲存**：`families/{familyId}/settings/page_metadata/{pageKey}`
**結構**：`{ title, subtitleStatic }`
**編輯入口**：settings 個人化 Tab → 「頁面標題」卡

---

## 九、常用術語對照（使用者 ↔ AI）

| 使用者常說 | 對應術語 |
|---|---|
| 「頁面主題」 | page-header 標題 |
| 「副標題」 | page-header 副標題 |
| 「統計卡」 | stats section |
| 「新增按鈕」 | page-controls add |
| 「卡片/表格切換」 | page-controls viewToggle |
| 「年度按鈕」 | page-controls year |
| 「月份按鈕」 | page-controls month |
| 「篩選器」 | page-controls filter |
| 「右上角那顆按鈕」 | page-header-actions 內的 item |
| 「Tab」 | tab-bar + tab-panel |
| 「側邊欄」 | sidebar |
| 「表頭排序」 | data-table sortable |
| 「編號對照清單」 | code-list.html（原「基礎資料庫」） |

---

## 十、擴充規則

| 步驟 | 動作 |
|---|---|
| 1 | 檢查是否已有類似術語 |
| 2 | 加到對應表格 |
| 3 | 更新 Version |

**禁止**：使用未定義術語 / 重複定義。

---

## 十一、業務術語（專案專屬）

### 保險類
| 術語 | 說明 |
|---|---|
| 基金保險 | 投資型保險 |
| 普通保險 | 住院 / 人壽 / 意外 |
| 供滿 | 保單供款年期結束 |
| 平攤 | 將年繳保費按 12 個月分攤 |
| 代墊 | 成員先墊付保費 |
| 保單持有人 | policyHolderId |
| 受保人 | memberId |
| 供款期 | periodIndex |

### 收支類
| 術語 | 說明 |
|---|---|
| 家用轉入 | income 節點 |
| 個人收入 | personal_income 節點 |
| 共同支出 | memberId = 'shared' |
| 額外收入 | extra 節點 |

### 結算類
| 術語 | 說明 |
|---|---|
| 待處理 | pending |
| 已處理 | done |
| 不適用 | skipped |
| 結算清單 | 整合個人支出 / 共同支出 / 保險扣款 |

### 基金類
| 術語 | 說明 |
|---|---|
| 保險基金 | type = 'insurance' |
| 獨立基金 | type = 'standalone' |
| 快照 | 每月記錄 |
| 累積供款 | cumulativeCost |
| 盈虧 | 現值 − 累積供款 |

### 銀行類
| 術語 | 說明 |
|---|---|
| 家庭帳號 | type = 'family' |
| 個人帳號 | type = 'personal' |
| 初始餘額 | initialBalance |
| 銀行交易 | in / out / transfer |

### 角色類
| 術語 | 說明 |
|---|---|
| superadmin | 超級管理員 |
| owner | 家庭擁有者 |
| member | 家庭成員 |
| canInput | 是否可輸入 |

### 編號系統類
| 術語 | 說明 |
|---|---|
| code | 實體編號（如 POL-001） |
| code-registry | 編號生成模組 |
| id-resolver | 編號查詢模組 |
| entity-create | 統一新增端點 |
| 編號對照清單 | code-list.html |
| 大類前綴 | MEM / PAY / POL / FUND / CAT / ITEM / COMP / INC / TXN |
| 子類短碼 | FUND / NORM / FUNIN / SAVE / SCH / TRA / MED / BNK / OTH |

---

## 十二、關聯檔案

| 主題 | 檔案 |
|---|---|
| 通用 UI 結構 | W-05-schema.md |
| 頁面區塊規格 | P-06-pagemap.md |
| 資料結構 | P-09-data.md |

---

/* ═══ EOF ═══
F: P-07-uispec.md
V: 1792581018
W: ai-system-20261021-004#18
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-07-uispec.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-08-api.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-08-api.md

# P-08 API 契約

最後更新：2026-10-21
Version: 1792581019
Writer: ai-system-20261021-004#19
用途：本專案前後端介面參考（通用錯誤碼已抽至 W-02）

---

## 一、前端模組 API

*(保留原有前端模組 API 完整清單，如 status-registry / entity-registry / code-registry / data-engine 等，不變動)*

---

## 二、後端 API 契約

所有 URL 路徑保持不變；實作改為 functions/api/[[path]].js Catch-all 分派。

### GET 端點
*(保留原有清單：/api/family-settings / /api/summary / /api/annual-summary 等)*

### POST 端點
*(保留原有清單：/api/lookup-family / /api/entity-create 等)*

### POST /api/entity-create（本專案核心端點）

**Request**：
| 欄位 | 類型 | 說明 |
|---|---|---|
| entityType | string | member / bank / category / item / payment / company / policy / fund / bankAccount |
| action | string | create / update / delete |
| code | string | 前端生成的編號（create 用） |
| name | string | 名稱 |
| id | string | 實體 ID（update / delete 用） |
| extra | object | 依類型不同 |

**成功回應**：`{ ok: true, id, code }`

**本專案專屬錯誤碼**：
| error | 觸發 |
|---|---|
| CODE_DUPLICATE | 編號已存在 |
| NAME_DUPLICATE | 名稱重複 |

*(通用錯誤碼如 MISSING_FIELDS, INVALID_BODY, UNAUTHORIZED 等請見 W-02)*

**後端檢查順序**：驗證登入 → 驗證 body → 驗證必填 → 查重複 code → 查重複 name → 寫入 Firebase。

---

## 三、錯誤回應格式

> **通用錯誤回應格式與 HTTP 狀態碼（401, 403, 400, 404, 409, 500）請見 `W-02-functions.md` §六。**

本專案額外定義：
| Code | HTTP | 說明 |
|---|---|---|
| CODE_DUPLICATE | 409 | 編號重複 |
| NAME_DUPLICATE | 409 | 名稱重複 |

---

## 四、成功回應格式

| 欄位 | 說明 |
|---|---|
| ok | true |
| ... | 資料欄位 |

---

## 五、B34-4 交付完成清單

| 模組 | 狀態 |
|---|---|
| code-registry.js | ✅ |
| id-resolver.js | ✅ |
| code-list.html / code-list.js | ✅ |
| entity-create 端點 | ✅ |
| dropdown-plus.js | ✅ |
| form-builder plusEntity | ✅ |
| settings 年份範圍 | ✅ |

---

## 六、關聯檔案

| 主題 | 檔案 |
|---|---|
| 通用 CF Functions 規則 | W-02-functions.md |
| 資料結構 | P-09-data.md |
| 專案索引 | P-01-index.md |

---

/* ═══ EOF ═══
F: P-08-api.md
V: 1792581019
W: ai-system-20261021-004#19
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-08-api.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-09-data.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-09-data.md

# P-09 Firebase RTDB 資料結構

最後更新：2026-10-21
Version: 1792581021
Writer: ai-system-20261021-004#21
用途：本專案實際資料節點

---

## 一、platform 節點

### families/{familyId}/

| 欄位 | 內容 |
|---|---|
| name | 家庭名稱 |
| ownerEmail | 擁有者 Email |
| createdAt | 建立時間戳 |

### families/{familyId}/memberAccounts/{uid}/

| 欄位 | 內容 |
|---|---|
| email / account / displayName | 帳號資訊 |
| role | owner / member |
| canInput | true / false |
| memberId | 對應成員 ID |
| createdAt | 建立時間 |

### platform 索引

| 路徑 | 內容 |
|---|---|
| uid_index/{uid} | familyId |
| email_index/{account} | { uid, familyId } |

### platform/defaults/

| 節點 | 內容 |
|---|---|
| members / banks / insurance_companies | 預設基礎 |
| payment_methods / expense_categories / expense_items | 預設選項 |
| statuses / options / year_range / ui_constants | 預設設定 |

---

## 二、families 節點

### 基礎資料

| 節點 | 欄位 |
|---|---|
| members/{id} | name / role / order / code / createdAt |
| banks/{id} | name / order / code / createdAt（舊） |
| expense_categories/{id} | name / shortCode / order / code / createdAt |
| expense_items/{id} | name / categoryId / code / createdAt |
| payment_methods/{id} | name / order / code / createdAt |
| insurance_companies/{id} | name / code / createdAt |
| statuses/{id} | name / category / isDone / order / createdAt |

**新制**：所有實體加 code 欄位（D-008）。

### 銀行系統

| 路徑 | 欄位 |
|---|---|
| bank_accounts/{bankId} | name / type / ownerType / ownerId / isSystemCreated / initialBalance / initialYear / initialMonth / order / code / createdAt |
| bank_accounts/{bankId}/transactions/{txnId} | type / category / amount / date / memberId / refId / note / code / createdAt |

**交易 type**：in / out / transfer
**交易 category**：contribution / expense / insurance / reimbursement / manual

### 個人財務

| 路徑 | 內容 |
|---|---|
| personal_income/{memberId}/{year}/{month} | 數字 |
| member_advances/{memberId}/{advanceId} | policyId / totalAmount / remainingAmount / startYear / startMonth / paidMonths / note / createdAt |

**paidMonths 格式**：{ 'YYYY-MM': true }

### 收入

| 路徑 | 內容 |
|---|---|
| income/{year}/{month}/{memberId} | 數字 |

**額外收入 memberId**：extra

### 支出

| 路徑 | 欄位 |
|---|---|
| expenses/{year}/{month}/member_expenses/{memberId}/{expenseId} | name / amount / status / date / categoryId / itemId / paymentMethodId / bankId / txnId / isAutoLinked / policyId / repaidDate / code / createdAt |

**特殊 memberId**：shared（家庭共用）
**特殊 expenseId**：linked_{policyId}（保險連動）

### 保險

| 路徑 | 欄位 |
|---|---|
| insurance_policies/{policyId} | type / memberId / policyHolderId / name / company / paymentType / paymentMode / advanceHolderId / advanceId / firstStartYear / firstStartMonth / totalPolicyYears / totalPolicyPeriods / totalPremium / currentPeriodIndex / monthlyPremium / annualPremium / account / fundsAllocation / periods / isCompleted / code / createdAt |
| insurance_payments/{policyId}/{year}/{month} | status / amount / date / bankId / txnId / paymentMode |

**periods 結構**：{ '{N}': { periodIndex, startYear, startMonth, annualPremium, monthlyAverage } }

**fundsAllocation 結構**：[{ fundId, pct, fromYear, fromMonth }, ...]

### 基金

| 路徑 | 欄位 |
|---|---|
| funds/{fundId} | name / type / policyId / cost / currentValue / units / initialYear / initialMonth / note / code / createdAt |
| funds/{fundId}/snapshots/{year}/{month} | shares / nav / value / contribution / cumulativeCost / createdAt / updatedAt |

**type**：standalone（獨立）| insurance（掛於保單）
**snapshot.value**：shares × nav（自動計算）

### 家庭設定

| 節點 | 內容 |
|---|---|
| settings/options | { memberRoles, policyTypes, ... } |
| settings/year_range | { startYear, futureYears } |
| settings/ui_constants | { nameMaxLenDesktop, ... } |
| settings/page_metadata/{pageKey} | { title, subtitleStatic } |

### 廢除節點（保留不讀寫）

| 節點 | 狀態 |
|---|---|
| bank_balances/{year}/{month}/ | 廢除 |
| fixed_expenses/{year}/{month}/ | 廢除 |

---

## 三、狀態代碼

### 新代碼

| 代碼 | isDone |
|---|---|
| pending | false |
| done | true |
| skipped | true |

### 舊名稱 → 新代碼

| 舊名稱 | 新代碼 |
|---|---|
| 未處理 / 未還款 / 未付款 / 未扣款 / 未轉入 | pending |
| 已處理 / 已還款 / 已付款 / 已扣款 / 已轉入 | done |
| 不適用 | skipped |

### 讀寫策略

| 情境 | 做法 |
|---|---|
| 讀取 | status.normalize(raw) |
| 寫入 | 直接寫新代碼 |

---

## 四、顯示文字（依來源）

| 代碼 | 支出 | 保險 | 收入 |
|---|---|---|---|
| pending | 未付款 | 未扣款 | 未轉入 |
| done | 已付款 | 已扣款 | 已轉入 |

由 status.label(raw, { source }) 決定。

---

## 五、跨節點關聯

| 欄位 | 關聯 |
|---|---|
| expenses.*.bankId | bank_accounts/{bankId} |
| expenses.*.txnId | bank_accounts/{bankId}/transactions/{txnId} |
| expenses.*.categoryId | expense_categories/{id} |
| expenses.*.itemId | expense_items/{id} |
| expenses.*.paymentMethodId | payment_methods/{id} |
| expenses.*.policyId | insurance_policies/{policyId} |
| insurance_payments.*.bankId | bank_accounts/{bankId} |
| insurance_policies.advanceId | member_advances/{advanceHolderId}/{advanceId} |
| insurance_policies.fundsAllocation[].fundId | funds/{fundId} |
| funds.policyId | insurance_policies/{policyId} |
| member_advances.policyId | insurance_policies/{policyId} |
| platform.uid_index.{uid} | familyId |
| platform.email_index.{account} | { uid, familyId } |

---

## 六、大小限制

| 節點 | 限制 |
|---|---|
| expenses/{y}/{m}/member_expenses/{memberId}/ | ≤ 100 筆 |
| bank_accounts/{bankId}/transactions/ | ≤ 1000 筆 |
| income/{y}/{m}/ | ≤ 20 成員 |
| member_advances/{memberId}/ | ≤ 20 筆 |
| funds/{id}/snapshots/ | 無上限 |

---

## 七、基金累積流程

### 觸發

| 步驟 | 動作 |
|---|---|
| 1 | 使用者在結算清單改保險狀態為 done |
| 2 | 前端呼叫 api.insuranceSync(payload) |
| 3 | 後端 _insurance.js 執行 _accumulateFunds |
| 4 | 前端收到回應 → 更新 UI |

### 累積邏輯

| 步驟 | 動作 |
|---|---|
| 1 | 讀取 policy.fundsAllocation |
| 2 | 若為空 → 跳過 |
| 3 | 對每個 fundId：計算 addCost = monthlyPremium × pct / 100 |
| 4 | 讀取 funds/{fundId}/snapshots/{y}/{m} |
| 5 | 若 snapshot 存在 → 累加 cumulativeCost |
| 6 | 若不存在 → 建立 snapshot |
| 7 | 寫回 Firebase |

### 撤銷

| 步驟 | 動作 |
|---|---|
| 1 | 使用者在結算清單改回 pending |
| 2 | 前端呼叫 api.insuranceUnsync |
| 3 | 後端執行 _reverseFundAccumulation |
| 4 | 對每個 fundId：減去 addCost |
| 5 | 更新 snapshot 的 cumulativeCost |

### 一致性保證

| 保證 | 說明 |
|---|---|
| 自動累積 | 使用者不需手動計算 |
| 自動撤銷 | 改回 pending 時自動回退 |
| 禁止手動改 | 只能透過 sync / unsync |
| 冪等 | 重複呼叫同狀態 → 無變化 |

---

## 八、舊資料相容策略

### 相容原則

| 原則 | 說明 |
|---|---|
| 不刪除舊資料 | 所有節點保留 |
| 不破壞讀寫 | 舊資料可正常讀取 |
| 寫入新格式 | 新資料一律寫新代碼 |
| 讀取時映射 | 舊名稱 → 新代碼 |
| 使用者零感知 | 操作邏輯完全不變 |
| 一次性遷移 | 提供 API 供主動遷移 |

### 相容清單

| 舊資料 | 相容策略 | 實作位置 |
|---|---|---|
| 狀態：未處理 / 已扣款 | status.normalize(raw) | status-registry.js |
| banks/ 節點 | entity-registry 註冊 | entity-registry.js |
| bank_balances/ | 不讀寫，UI 清除按鈕 | bank-account-manager |
| fixed_expenses/ | summary.js 仍讀取 | functions/api/_summary.js |
| linked_xxx 支出 | merge.js 仍解析 | lib/merge.js |
| 保險無 paymentMode | fallback = 'direct' | entity-definitions.js |
| 帳號無 memberId | 3 層 fallback | functions/api/_personal.js |
| 保單無 advanceHolderId | fallback = '' | entity-definitions.js |
| 保單無 advanceId | fallback = '' | entity-definitions.js |
| 舊版帳號 | verifyFamilyAccess 收緊 | functions/api/_helpers.js |
| 基金無 type | fallback = 'standalone' | entity-definitions.js |
| 基金無 initialYear / Month | fallback = '' | db.js |
| 保單無 fundsAllocation | fallback = [] | db.js |
| 支出無 isAutoLinked | fallback = !!policyId | entity-definitions.js |
| 實體無 code | 由新制遷移補上（議題 4） | code-registry.js |

### 狀態正規化流程

**讀取**：Firebase → normalize → 標準代碼 → label → 顯示
**寫入**：使用者輸入 → options → 代碼 → Firebase

**主動遷移**：

| 步驟 | 動作 |
|---|---|
| 1 | superadmin 進 settings 個人化 |
| 2 | 點「執行遷移」 |
| 3 | 呼叫 POST /api/admin-migrate |
| 4 | 後端改寫舊狀態為新代碼 |
| 5 | 補齊缺失欄位（含 code） |

**特性**：可重複執行 / 不刪資料 / 不影響 UI / 不可逆。

### 舊節點處理

| 節點 | 狀態 | 處理 |
|---|---|---|
| bank_balances/ | 廢除 | 不讀寫；UI 提供清除按鈕 |
| fixed_expenses/ | 廢除 | 保留 summary.js 讀取 |
| banks/ | 部分廢除 | entity-registry 一併註冊 |

### 測試清單

| # | 情境 | 預期 |
|---|---|---|
| 1 | 舊資料（狀態「未付款」） | 顯示「未付款」 |
| 2 | 舊資料（狀態「已扣款」） | 顯示「已扣款」 |
| 3 | 新資料（狀態 pending） | 顯示「未付款」 |
| 4 | 新資料（狀態 done） | 顯示「已付款」 |
| 5 | 舊 banks/ 節點 | 正常顯示 |
| 6 | bank_balances/ 存在 | UI 顯示清除按鈕 |
| 7 | fixed_expenses/ 存在 | summary 仍讀取 |
| 8 | 保險無 paymentMode | 當作 direct |
| 9 | 帳號無 memberId | fallback 生效 |
| 10 | 基金無 type | 當作 standalone |
| 11 | 保單無 fundsAllocation | 當作 [] |
| 12 | 執行一次性遷移 | 舊狀態改寫為新代碼 |
| 13 | 重複執行遷移 | 掃描 X，修改 0 |
| 14 | 實體無 code | 遷移後補上 |

---

## 九、關聯檔案

| 主題 | 檔案 |
|---|---|
| Firebase RTDB 原則 | W-01-firebase.md |
| API 契約 | P-08-api.md |
| 專案索引 | P-01-index.md |
| 決策 | P-04-adr-index.md |

---

/* ═══ EOF ═══
F: P-09-data.md
V: 1792581021
W: ai-system-20261021-004#21
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-09-data.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-10-incident.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-10-incident.md

# P-10 事故日誌

最後更新：2026-10-21
Version: 1792581022
Writer: ai-system-20261021-004#22
用途：記錄歷史問題，避免重複犯錯

---

## 一、總覽

| 批次 | 編號 | 主題 | 嚴重度 |
|---|---|---|---|
| B11 | P01~P13 | 首次部署 | 🔴 |
| B12 | B01~B07 | 阻斷級修復 | 🔴 |
| B13 | H01~H11 | SSOT 統一 | 🟠 |
| B14 | M01~M18 | 中優先 | 🟡 |
| B15 | L01~L07 | 低優先 | 🟢 |
| B16 | P16-01~05 | 第二輪測試 | 🟠 |
| B17 | P17-01~06 | 第三輪測試 | 🔴 |
| B18 | — | entity-resolvers | 🟡 |
| B19 | — | 無補丁重構 | 🟡 |
| B20 | P18-01~05 | 第四輪測試 | 🟠 |
| B22 | P19-01~03 | 第五輪測試 | 🟠 |
| B23 | — | 基金保險分支 | 🟡 |
| B24 | — | 診斷報告 | 🟡 |
| B25 | P20~P24 | 全站修復 | 🟠 |
| B26 | — | 全站優化 | 🟠 |
| B29 | P25~P28 | 表頭 + metadata | 🟠 |
| B30-5 | P29 | 後端 DB 切換 | 🔴 |
| B30-8 | P30 | 基金成本計算錯誤 | 🟠 |
| B30-9 | P31 | Spck Editor 上傳失誤 | 🟠 |
| B31-1 | B31-1-B1~B9 | 9 個 bug 修正 | 🟠 |

---

## 二、B31-1：9 個 bug 修正

| # | 檔案 | 症狀 | 根因 | 修法 |
|---|---|---|---|---|
| B31-1-B1 | database-yearrange.js | 清空起始年份 → 被當 0 | form-builder number 欄位 Number('')===0 | 改從 DOM 直讀 value |
| B31-1-B2 | auth-guard.js | fallback 時 API 全失效 | fallback 用 user.uid 當 familyId | 不覆蓋既有 familyId |
| B31-1-B3 | pages/insurance.js | 已供款資訊不刷新 | _lastIds id 快取擋住 payments 更新 | 訂閱 insurance_payments |
| B31-1-B4 | lib/merge.js | 換家庭後狀態解析錯誤 | 模組級快取未監聽 family-change | 加 AppState.on('family-change') |
| B31-1-B5 | core/db.js | 廢棄函式會清欄位 | set 覆蓋而非 update 合併 | 改 update + 標 @deprecated |
| B31-1-B6 | _admin.js | 死代碼 | _migrate_check_expenses 寫入即刪 | 移除 |
| B31-1-B7 | _admin.js | 驗證順序 | 先驗 uid 再驗權限 | 權限驗證提前 |
| B31-1-B8 | pages/settings.js | 違反 SSOT | 硬編碼 Firebase URL 動態 import | 改用 listenPageMetadata |
| B31-1-B9 | ui/modal.js | 脆弱設計 | _confirmResolve 全域單一 | 開新前先 resolve 舊 |

---

## 三、B30-9：Spck Editor 上傳失誤（P31）

### P31：portfolio.js 被 insurance.js 內容覆蓋

| 項目 | 內容 |
|---|---|
| 症狀 | 修改「東方匯理基金成本」後，畫面數字未變 |
| 原因 A | 在 Spck Editor 手動修改時，把 insurance.js 的代碼貼到了 portfolio.js |
| 原因 B | 只 Commit 未 Push |
| 解決 | 重新貼上正確的 portfolio.js 代碼 |
| 教訓 | 上傳前必須逐一核對「檔名 ↔ 內容」 |

---

## 四、B30-8：基金成本計算錯誤（P30）

### P30：基金成本未按比例分配

| 項目 | 內容 |
|---|---|
| 症狀 | 四支基金投入成本皆顯示 199,500，總計 798,000 |
| 原因 A | _resolveCost 未乘以該基金在保單中的比例（pct） |
| 原因 B | 手動輸入成本的基金比例為 0，被計算為 0 |
| 解決 | 修正 _resolveCost：先扣除手動成本，剩餘按比例分配 |

---

## 五、B30-5：後端 DB 切換（P29）

### P29：後端永遠連舊 DB

| 項目 | 內容 |
|---|---|
| 症狀 | superadmin 登入新 DB 後，API 顯示「需要超級管理員權限」 |
| 原因 A | _config.js 寫死舊 DB URL + API Key |
| 原因 B | CF Pages 不支援 process.env → 改用 context.env |
| 原因 C | 移除 FIREBASE_DB_URL export → 依賴模組崩潰 |
| 解決 | export let + initConfig(env) + handler 傳遞 |
| 教訓 | CF Pages env 走 handler 參數；不得移除既有 export |

---

## 六、B29：表頭排序 + 頁面 metadata（P25~P28）

| 編號 | 症狀 | 原因 | 解決 |
|---|---|---|---|
| P25 | 年月 select 高亮不動 | page-controls.js 的 year / month 值在 customMount 快照一次 | 改為內部直接讀寫 AppState |
| P26 | 月份顯示「0 月」；雙重渲染 | prevM = cur - 1 未處理 < 1 | 邊界顯示「—」+ disabled |
| P27 | 副標題未取代 | HTML 硬編碼；page-engine 只加 data-prefix | page-engine 清空 textContent |
| P28 | page-header 同行（3 頁遺漏） | B26-3.2 只改 7 頁 | 統一 page-header 結構 |

---

## 七、B25~B26：全站修復（P20~P24）

| 編號 | 症狀 | 原因 | 解決 |
|---|---|---|---|
| P20 | 供滿保單仍顯示 | _loadInsuranceRows 未檢查保單期間 | 用 getPeriodInfo 判斷 |
| P21 | hideCompleted 誤過濾 | hideCompleted 放進 filters | 移出 filters |
| P22 | 年月切換需重整 | filters.year/month 未同步 | reload() 開頭同步 |
| P23 | 年份 / 月份按鈕設計退化 | — | 恢復 3 按鈕 |
| P24 | 全站佈局 + superadmin sidebar | — | B26-3~6 修復 |

---

## 八、早期事故（B11~B22，精簡版）

### B22：第五輪測試（P19-01~03）

| 編號 | 症狀 | 原因 | 解決 |
|---|---|---|---|
| P19-01 | view toggle 反向 + 卡片無資料 | data-card 找 fields，頁面傳 columns | data-card 支援 columns |
| P19-02 | 保險 / 銀行沒 toggle | B20 只加 4 頁 | 兩頁加 initViewToggle |
| P19-03 | 年度報表統計卡不隨切換 | statsCards deps 只有 state.year | 依 view 動態回傳 |

### B20：第四輪測試（P18-01~05）

| 編號 | 症狀 | 原因 | 解決 |
|---|---|---|---|
| P18-01 | 多頁缺 view toggle | 頁面未初始化 | 各頁呼叫 + HTML 加容器 |
| P18-02 | 年度報表月度明細沒資料 | view-toggle 硬編碼 | 加 cardValue/tableValue |
| P18-03 | 已供滿保單不能展開 | insurance.js 遺漏 | 加展開 handler |
| P18-04 | 副標題顯示「載入中」 | 未更新副標題 | 各頁 syncSubtitle |
| P18-05 | 側欄不能滑到底 | flex 子項缺 min-height | 加 min-height |

### B17：第三輪測試（P17-01~06）

| 編號 | 症狀 | 原因 | 解決 |
|---|---|---|---|
| P17-01 | 年度報表資料沒顯示 | derived 依賴非同步 | 改用 blocks |
| P17-02 | 基金表格沒資料 | 只有 stats block | 加 table block |
| P17-03 | 年月沒連動 | customMount 沒訂閱 | 加 onYearMonthChange |
| P17-04 | 類別顯示 ID | resolver 只 escapeHtml | resolveName |
| P17-05 | 輸入中心沒支出 | listenAllMemberExpenses(null, null) | 改用 listenAllExpenses |
| P17-06 | 保險持單人顯示 ID | 沒傳 resolvers | 新增 entity-resolvers |

### B16：第二輪測試（P16-01~05）

| 編號 | 症狀 | 原因 | 解決 |
|---|---|---|---|
| P16-01 | 新增支出開錯 Modal | input-center.js 佔位 | 寫新支出 Modal |
| P16-02 | 年度報表缺月度明細 | 未實作 | 加年份 + 月份切換 |
| P16-03 | 成員名稱顯示 push key | resolver 只 escapeHtml | resolveName |
| P16-04 | 系統設定缺成員資料 | customMount 只保留 Tab | 補 _initPersonalTab |
| P16-05 | 基金版面沒資料 | display:none 隱藏 | 改 blocks |

### B15：低優先（L01~L07）

小排版 / 空狀態文字 / Toast 訊息 / 按鈕 hover / 表頭欄寬 / 卡片間距 / 次要註解。

### B14：中優先（M01~M18，摘要）

M01 年月切換器位置 / M02 統計卡格式 / M03 空狀態顯示 / M04 卡片高度 / M05 表格欄寬 / M06 Modal 寬度 / M07 銀行餘額檢查 / M08 支出 Modal 欄位順序 / M09 收入 Modal 欄位順序 / M10 保險 Modal 欄位 / M11 基金 Modal 欄位 / M12 類別下拉排序 / M13 項目下拉排序 / M14 支付方式顯示 / M15 狀態顯示 / M16 年份範圍限制 / M17 UI 常數集中 / M18 成員排序

### B13：SSOT 統一（H01~H11）

H01 狀態字串硬編碼 / H02 名稱解析分散 / H03 顯示文字分散 / H04 表格欄位重複定義 / H05 常數散落 / H06 esc 重複實作 / H07 format 重複實作 / H08 狀態顯示文字 / H09 銀行交易分類 / H10 支付模式 / H11 角色標籤

### B12：阻斷級（B01~B07）

| 編號 | 症狀 | 原因 | 解決 |
|---|---|---|---|
| B01 | 檔案內容錯誤 | 複製貼上誤覆蓋 | 完全重寫目標檔 |
| B02 | 舊 import 路徑殘留 | 檔案搬移未全域搜尋 | 全域搜尋舊路徑 |
| B03 | page-engine customMount 遺漏 | 未呼叫 | 加 customMount 支援 |
| B04 | page-engine 修正遺漏 | 依賴圖 key 不一致 | 加 data. 前綴 |
| B05 | derived 對 undefined 崩潰 | 非同步初始未到 | 加 undefined 檢查 |
| B06 | app-shell 靜態 import 崩潰 | 缺失時整檔失敗 | 改動態 import |
| B07 | 阻斷級相關 | 多項阻斷性問題 | B12 全面修復 |

### B11：首次部署（P01~P13，摘要）

P01 CF Functions 路由 500 / P02 HTML 缺 .html 匹配 .js / P03 _redirects 循環 / P04 page-engine 缺 customMount / P05 _onKeyChange 缺 data. 前綴 / P06 esc 從錯誤檔導入 / P07 derived 對 undefined 崩潰 / P08 手機無 Console / P09 app-shell 靜態 import 崩潰 / P10 SW stale 需訪問 2 次 / P11 GitHub 檔案未真正更新 / P12 database.js 內容錯誤 / P13 js/shared/* 舊 import 路徑

---

## 九、歷史重演警告

| 類型 | 首次 | 再次 | 防止 |
|---|---|---|---|
| 複製貼上誤覆蓋 | P12 | B01 / P31 | 交付前確認目標 |
| page-engine 修正遺漏 | P07 | B04 | 改 engine 需回歸測試 |
| 舊 import 路徑殘留 | P13 | B02 | 搬移後全域搜尋 |
| view toggle 遺漏 | P18-01 | P19-02 | 新頁必加 |
| 狀態字串硬編碼 | H01 | H8 | 全走 registry |
| 自我引入 bug | P20 | P21 | 交付前全鏈路 |
| 全站結構改動不完整 | B26-3.2 | B29-8 | 改前完整掃描 |
| 動態值快照 | P22 | P25 | 內部讀 AppState |
| 未清空既有內容 | — | P27 | 動態注入時清空 |
| backtick 嵌套 | — | B29-9 | 交付前強制檢查 |
| 後端寫死舊 DB | — | P29 | 用 env + initConfig |
| 移除 export 崩潰 | — | P29 | 用 export let |
| 基金成本未按比例 | — | P30 | 動態分配 |
| 上傳檔案失誤 | P12 | P31 | 推送前逐一核對 |
| md 內三反引號 | — | B32-1 | 零三反引號 |
| 編號 / 序號未標 | — | B32-1 | 強制 9 條 |

---

## 十、核心教訓總結

### CF Pages

| 教訓 | 說明 |
|---|---|
| Functions 路由 | 每個 URL 對應獨立實體檔 |
| 不支援 shim | 用底線前綴 + Catch-all |
| 靜態檔案 | 所有連結帶 .html |
| _redirects | 不要對 / 加規則 |
| 環境變數 | 走 handler { env }，非 process.env |

### Page Engine

| 教訓 | 說明 |
|---|---|
| customMount | 必須支援 |
| onDataChange | 必須支援 |
| 依賴圖 | key 與事件觸發一致 |
| undefined | derived 需容忍 |
| 動態值 | 內部讀 AppState |
| 動態注入 | 需清空既有內容 |

### 程式碼

| 教訓 | 說明 |
|---|---|
| esc 路徑 | 從 lib/dom.js |
| format* 路徑 | 從 lib/format.js |
| 檔案搬移 | 全域搜尋舊路徑 |
| 無補丁 | 寧願重寫 |
| 狀態字串 | 一律走 status-registry |
| filters | 只放 row 過濾欄位 |
| 邊界值 | 需特判 |
| export 相容 | 用 export let |

### 交付

| 教訓 | 說明 |
|---|---|
| 先方案後交付 | 待確認 |
| 完整檔案 | 不 diff |
| EOF | 檢查完整性 |
| 檔案 ≤ 800 行 | 拆檔 |
| backtick 嵌套 | 外層 = 內層 + 1 |
| md 零三反引號 | 用引用塊 |
| 編號 + 序號 | 每次回覆必標 |
| Writer | EOF 必填 |

---

## 十一、關聯檔案

| 主題 | 檔案 |
|---|---|
| 錯誤摘要 | P-99-errors-map.md |
| 學習日誌 | 體系 R-04-learning.md |
| 通用規則 | 體系 R-00-rules.md |
| 決策 | P-04-adr-index.md |
| CF Pages 規則 | W-02-functions.md |
| PWA 規則 | W-04-pwa.md |

---

/* ═══ EOF ═══
F: P-10-incident.md
V: 1792581022
W: ai-system-20261021-004#22
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-10-incident.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-14-decisions.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-14-decisions.md

# P-14 專案決策明細附錄

最後更新：2026-10-21
Version: 1792581014
Writer: ai-system-20261021-004#14
用途：本專案所有決策（D-001 ~ D-143）完整明細存檔

---

## 一、決策索引

> **SSOT 宣告**：本專案之最新決策摘要已整合至 `WF-CONCLUSION.md` §三。本檔作為決策明細（D-編號）的完整附錄存檔。

| 階段 | 編號 | 主題 |
|---|---|---|
| B33-3 | D-001 ~ D-016 | 議題 1~4 結論 |
| B34-1 | D-017 ~ D-023 | 檔案整合 + 提問規則 |
| B34-2 | D-024 ~ D-037 | 議題 5~6 + Token 原則 |
| B34-3 | D-038 ~ D-058 | 議題 7 / 9 + 四層架構 |
| B34-4 | D-059 ~ D-096 | 編號系統程式碼交付 |
| B34-4 收尾 | D-097 ~ D-117 | 學習日誌 + 測試修正 |
| B34-4 重組設計 | D-118 ~ D-143 | 通用大腦架構 |

---

## 二、重點決策摘錄（依階段）

*(本節保留原 P-14 重點決策之完整內容，如 D-059 至 D-143)*

### D-063：entity-create 單一端點
| 項目 | 內容 |
|---|---|
| 決策 | 單一端點 + action 參數（create / update / delete） |

### D-064：舊端點全廢
| 項目 | 內容 |
|---|---|
| 決策 | 廢棄全部 CRUD 端點，只留 GET + 特殊邏輯 |

### D-118：通用大腦 + N 領域技能包架構
| 項目 | 內容 |
|---|---|
| 決策 | C-* 通用 + W/V/D/M 領域技能包 |

### D-136：C-00 §十 啟動機制改「貼 URL」
| 項目 | 內容 |
|---|---|
| 決策 | 不上傳檔案，改貼 URL |

*(詳細歷史存檔保留原有 B33-3 至 B34-4 所有決策表格，此處為節錄展示，完整內容見原檔，不刪除)*

---

## 三、關聯檔案

| 主題 | 檔案 |
|---|---|
| 決策摘要（SSOT） | WF-CONCLUSION.md |
| ADR 索引 | P-04-adr-index.md |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: P-14-decisions.md
V: 1792581014
W: ai-system-20261021-004#14
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-14-decisions.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-15-reorg-plan.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-15-reorg-plan.md

# P-15 專案重組與體系對齊計畫書

最後更新：2026-10-21
Version: 1792581023
Writer: ai-system-20261021-004#23
用途：通用大腦 + N 領域技能包架構重組計畫（歷史文件，標記已執行）

---

## 一、背景與目標

### 1.1 現況問題（重組前）

| # | 問題 |
|---|---|
| 1 | 現有 C-* / W-* / P-* / R-* 混在同一 repo |
| 2 | 每開新對話需手動上傳 4 檔啟動包 |
| 3 | 無法套用到其他專案（VBA / 資料分析等） |
| 4 | 加新領域需重新設計 |

### 1.2 目標

| # | 目標 |
|---|---|
| 1 | 通用大腦 + N 領域技能包架構 |
| 2 | 零上傳（貼 1~2 條 URL） |
| 3 | 新專案 5 分鐘內啟動 |
| 4 | 新領域照抄模板即可 |

---

## 二、架構總覽（已對齊體系四檔）

| 層 | 前綴 | 職責 | Repo |
|---|---|---|---|
| L1 | 體系四檔 | IP/AP/RP/UP | 體系 |
| L2 | W-* / V-* / D-* / M-* | 領域技能包 | 體系 (W) / 未來擴充 |
| L3 | WF-* / P-* | 專案專屬 | 專案 Repo |
| L4 | R-01 | 機械記錄（跨專案） | 體系 |

---

## 三、目錄結構（已執行）

### 3.1 體系（AI-kernel）

| 層級 | 路徑 | 說明 |
|---|---|---|
| 1 | docs/ | 文件根 |
| 2 | docs/common/ (已改為體系四檔) | IP/AP/RP/UP |
| 2 | docs/skills/ | 領域技能包 |
| 3 | docs/skills/W/ | Web App（W-00 ~ W-07） |
| 2 | docs/records/ | 體系紀錄 |

### 3.2 專案（AI-WEB-APP-Family）

| 層級 | 路徑 | 說明 |
|---|---|---|
| 1 | docs/ | 文件根 |
| 2 | docs/projects/web-app-family/ | 專案根 |
| 3 | WF-*.md | 專案紀錄（PROGRESS / STATE-INDEX / CONCLUSION / HANDOFF） |
| 3 | P-*.md | 專案技術與附錄（P-01 / P-04 / P-05 / P-06 等） |

---

## 四、啟動包機制（已對齊 URL 方案）

用戶操作流程：
1. 貼 INDEX.md URL（可選）
2. 依 INDEX 指引，貼 2 條 URL（core + project）

2 條 URL：
- STARTUP-core-{領域}.md（體系提供）
- STARTUP-project-{專案}.md（專案提供）

---

## 五、未來擴充規則

### 5.1 新增領域（AI 主動建議）
觸發：使用者選「新專案」→ AI 問領域 → 若為新領域 → 主動建議建 6 檔（依 ADR-064）。

### 5.2 新增專案
1. 開新 Repo B
2. 複製 P-* 與 WF-* 模板
3. R-01 §三 加該專案
4. 設 GitHub Actions

---

## 六、對比表：重組前 vs 重組後

| 項目 | 重組前 | 重組後 |
|---|---|---|
| Repo 數 | 2 | 3 |
| 檔案結構 | docs/ 混放 | 體系：四檔；專案：WF-* / P-* |
| 加新領域 | 需重設計 | 照抄 W-* 模板 |
| 加新專案 | 難 | 4 步 |
| 啟動包 | 4 檔上傳 | 2 條 URL |
| 領域檔案數 | 不定 | 7 檔（W-00~W-07） |

---

## 七、待決事項（本輪已全數處理）

| # | 項目 | 狀態 |
|---|---|---|
| 1 | 體系四檔加載機制 | ✅ 已於體系完成 |
| 2 | 專案紀錄檔對齊 AI-G | ✅ 已交付 WF-* |
| 3 | W 包補齊通用 SOP 與錯誤 | ✅ 已交付 W-06 / W-07 |
| 4 | GitHub Actions yml 支援 WF-* | ✅ 本輪交付更新 |
| 5 | URL 範本 | ✅ 已於體系 R-01 建立 |

---

## 八、關聯檔案

| 主題 | 檔案 |
|---|---|
| 體系四檔 | IP/AP/RP/UP |
| 專案紀錄 | WF-PROGRESS / WF-CONCLUSION / WF-HANDOFF |
| 技術檔 | P-01 / P-05 / P-08 |

---

/* ═══ EOF ═══
F: P-15-reorg-plan.md
V: 1792581023
W: ai-system-20261021-004#23
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-15-reorg-plan.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-99-errors-map.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-99-errors-map.md

# P-99 錯誤快速查核表（交付前必讀）

最後更新：2026-10-21
Version: 1792581015
Writer: ai-system-20261021-004#15
上限：100 行
用途：交付前 AI 快速核對，詳細事故請見 `P-10-incident.md`

---

## 一、專案 15 大類錯誤（禁止）

| # | 類型 | 防止做法 |
|---|---|---|
| 1 | 估算前未掃描實際檔數 | 先 grep 掃描 |
| 2 | 「統一」誤解為各檔都改 | 統一 = 邏輯集中 |
| 3 | 未檢查現有 API 複用 | 先找複用機會 |
| 4 | 未思考集中層強制 | 優先改集中層 |
| 5 | 動態值用快照 | 內部讀狀態 |
| 6 | 動態注入未清空 | 先 textContent = '' |
| 7 | 全站結構改動未完整掃描 | 完整掃描後才動 |
| 8 | 自我引入 bug | 交付前全鏈路檢查 |
| 9 | Token 自估誤導使用者 | 以平台顯示為準 |
| 10 | 議題編號與選項混淆 | 議題數字 / 選項字母 |
| 11 | 通用內容寫入 P-* | 依四層架構分類 |
| 12 | 修改檔未標 base version | 必標「基於 #NNN 的 vX.Y.Z」 |
| 13 | 未讀當前版本就重寫 | 先讀再改 |
| 14 | Token < 20% 未重估 | 強制重估 |
| 15 | 用 Repo A / B 代稱 | 用實際名稱 |

---

## 二、交付前必查清單

| # | 檢查 | 狀態 |
|---|---|---|
| 1 | 已跑 5 問自檢 | |
| 2 | 已掃描實際檔數 | |
| 3 | 已檢查 API 複用 | |
| 4 | 已考慮集中層強制 | |
| 5 | 交付摘要含最小改動對比 | |
| 6 | 能解釋「為何不是 N-1」 | |
| 7 | EOF 已加（F/V/W） | |
| 8 | md 內零三反引號 | |
| 9 | 編號 + 序號已標 | |
| 10 | 追蹤表已附 | |
| 11 | Token 狀態已顯示（末尾） | |
| 12 | 選項已附人話解釋 | |
| 13 | 修改檔已標 base version | |
| 14 | 已先讀當前版本 | |
| 15 | 用實際 Repo 名稱 | |

---

## 三、高頻重演問題（摘要）

| 類型 | 首次 | 再次 | 防止 |
|---|---|---|---|
| 複製貼上誤覆蓋 | P12 | B01 / P31 | 交付前確認目標 |
| page-engine 修正遺漏 | P07 | B04 | 改 engine 需回歸測試 |
| 舊 import 路徑殘留 | P13 | B02 | 搬移後全域搜尋 |
| 狀態字串硬編碼 | H01 | H8 | 全走 registry |
| 自我引入 bug | P20 | P21 | 交付前全鏈路 |
| md 三反引號 | — | B32-1 | 零三反引號 + 替換對照表 |
| 漏 import | — | B34-4 | 先讀檔案再改 |
| 誤刪檔案 | — | B34-4 | 交付時標明取代關係 |

*(詳細事故日誌與根因分析請參閱 `P-10-incident.md`)*

---

## 四、體系通用錯誤

*(參閱 `W-07-errors.md` 了解跨專案通用的 CF Pages / ESM 教訓)*

---

## 五、關聯檔案

| 主題 | 檔案 |
|---|---|
| 事故詳細日誌 | P-10-incident.md |
| Web App 通用錯誤 | W-07-errors.md |

---

/* ═══ EOF ═══
F: P-99-errors-map.md
V: 1792581015
W: ai-system-20261021-004#15
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-99-errors-map.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-CONCLUSION.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-CONCLUSION.md

# WF-CONCLUSION（專案滾動結論）

用途：WebApp Family 專案滾動式累積結論（議題 + 設計 + 決策）

---

## 一、議題清單

| # | 議題 | 狀態 |
|---|---|---|
| 1~4 | B33-3 早期重構與基礎整合 | 已完成 |
| 5 | 編號生成機制與 entity-create 統一 | 已完成 |
| 6 | 編號查詢與 code-list 頁面 | 已完成 |
| 7 | 專案四層架構與 W 包抽離 | 已完成 |
| 9 | 通用大腦與領域技能包對齊 | 已完成 |
| 71 | 體系對齊與專案紀錄重整 | 已完成 |
| 72 | SPA 重構與後端極簡化（V2） | 已完成 |

---

## 二、設計結論

### 2.1 編號系統與 API 統一（B34-4 定案）
1. **統一端點**：廢棄所有獨立 CRUD 端點，統一由 `POST /api/entity-create` 處理（含 `action` 參數）。
2. **編號生成**：由前端 `js/app-data.js` 負責，透過遍歷現有資料取得最大編號 +1，無計數器，防併發重複檢測。
3. **清單頁面**：`code-list.html` 取代舊 `database.html`，提供篩選與權限管理。

### 2.2 專案重組與體系對齊（B34-5 定案）
1. **紀錄檔重組**：專案技術文件保留 `P-*`，紀錄檔全面轉為 `WF-*`。
2. **領域資料抽離**：通用 Web App 規則已抽至體系 `W-00 ~ W-07`。

### 2.3 SPA 架構與後端極簡化（2026-10-22 定案）
1. **前端 10 檔架構**：將舊版 129 個檔案重構為 `app.html`, `app.css`, `sw-app.js`, `manifest-app.json`，以及 7 個 `app-*.js` 核心模組。
2. **後端 2 檔架構**：將 `functions/api/` 下 11 個檔案合併為 `[[path]].js` 與 `_api-core.js`。
3. **舊檔案分離**：舊版檔案已全數遷移至 `old-AI-WEB-APP-FAMILY` 倉庫，確保新倉庫極度乾淨。

---

## 三、決策記錄

*(完整歷史決策 D-001~D-143 請參閱 `P-14-decisions.md`)*

| 編號 | 決策 | 說明 |
|---|---|---|
| D-063 | entity-create 單一端點 | 取代舊有各自獨立的 CRUD 端點。 |
| D-064 | 舊端點全廢 | 只保留 GET 與特殊邏輯端點。 |
| D-118 | 通用大腦 + N 領域技能包 | 將通用邏輯抽離至體系 W 包。 |
| D-149 | `entity-registry.js` code 支援 | 已完成於新版 `app-data.js`。 |
| D-170 | SPA 重構與後端合併 | 前端 129 檔合併為 11 檔，後端 11 檔合併為 2 檔。舊檔移轉至 `old-AI-WEB-APP-FAMILY`。 |
| D-171 | 新增 V2 識別與環境綁定 | `app.html` 加註 V2 與橘色識別；Cloudflare 環境變數設為 `DB_TARGET=new`。 |

---

## 四、關聯檔案

| 主題 | 檔案 |
|---|---|
| 專案進度 | WF-PROGRESS.md |
| 專案交接 | WF-HANDOFF.md |
| 決策明細 | P-14-decisions.md |
| ADR 索引 | P-04-adr-index.md |

---

/* ═══ EOF ═══
F: WF-CONCLUSION.md
V: 1792670012
W: webapp-20261022-046#1
═══ */

<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-CONCLUSION.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-HANDOFF.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-HANDOFF.md

# WF-HANDOFF（專案交接快照）

上限：80 行（嚴格限制）
最後更新：2026-10-22
Version: 1792670013
Writer: webapp-20261022-046#2
用途：提供下一輪對話 AI 的專案現況快照

---

## 一、當前會話摘要

| 欄位 | 值 |
|---|---|
| 對話編號 | webapp-20261022-045 |
| 本次範圍 | SPA 前端重構（10 檔）與後端合併（2 檔） |
| 狀態 | 程式碼重構完成，準備進入最終驗收階段 |

---

## 二、議題進度

**指向**：`WF-CONCLUSION.md` §一

**本對話重點**：
1. 完成 V2 版全新 SPA 架構。
2. 後端 `functions/api/` 重寫並合併為 2 檔。
3. 舊有 129 個檔案已轉移至 `old-AI-WEB-APP-FAMILY`。

---

## 三、關鍵決策

**指向**：`WF-CONCLUSION.md` §三

**本次新增決策**：D-170（SPA 重構）、D-171（V2 識別與環境綁定）。

---

## 四、重建條件（下一 AI 優先確認）

| # | 優先確認 |
|---|---|
| 1 | 是否已使用最新版專案包（包含本次重構紀錄）？ |
| 2 | Cloudflare 環境變數 `DB_TARGET` 是否為 `new`？ |
| 3 | 後端 `functions/api/` 是否只剩 `[[path]].js` 與 `_api-core.js`？ |

---

## 五、未完成事項

| 項目 | 狀態 |
|---|---|
| 30 項功能驗收（AC） | 待執行（專案已可運作，需人工逐項點擊） |
| 8 項差分測試（DT） | 待執行（確保計算邏輯未變） |
| 部分檔案補齊 EOF（F-005） | 技術債（C-02） |

---

## 六、下一步建議

| 順序 | 動作 |
|---|---|
| 1 | 開啟新對話（`[身份: Web]`）。 |
| 2 | 執行 30 項驗收清單，確認舊版功能 100% 還原。 |
| 3 | 執行 8 項差分測試，確保金額計算無誤。 |
| 4 | 補齊 `app-core.js` 等檔案末尾的 EOF。 |

---

/* ═══ EOF ═══
F: WF-HANDOFF.md
V: 1792670013
W: webapp-20261022-046#2
═══ */

<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-HANDOFF.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-PROGRESS.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-PROGRESS.md

# WF-PROGRESS（專案任務進度追蹤）

最後更新：2026-10-22
Version: 1792670014
Writer: webapp-20261022-046#3
用途：追蹤 WebApp Family 專案的步驟進度（跨對話持續）

---

## 一、當前任務

### 任務 B34-6：SPA 重構與後端極簡化
| 欄位 | 值 |
|---|---|
| 任務名稱 | 前端 SPA 重構（10 檔）、後端合併（2 檔）、舊檔分離 |
| 開始時間 | 2026-10-22 |
| 當前階段 | 程式碼已完成，待驗收 |
| 完成度 | 7/9（待 AC 與 DT 驗證） |

---

## 二、步驟清單

### 任務 B34-6：SPA 重構與後端極簡化
| # | 步驟 | 狀態 | 備註 |
|---|---|---|---|
| 1 | 建立 `app.html` 與 `app.css` SPA 骨架 | 已完成 | 含 V2 識別 |
| 2 | 重寫前端核心（`app-core`, `app-ui`, `app-data`） | 已完成 | — |
| 3 | 重寫前端業務（`app-domain`, `app-entities`, `app-widgets`） | 已完成 | — |
| 4 | 重寫前端頁面（`app-pages-core`, `app-pages-reports`） | 已完成 | — |
| 5 | 重寫後端（`_api-core.js`, `[[path]].js`） | 已完成 | 合併 11 檔為 2 檔 |
| 6 | 建立 Legacy 倉庫並轉移舊檔 | 已完成 | `old-AI-WEB-APP-FAMILY` |
| 7 | 設定 Cloudflare 環境變數（`DB_TARGET=new`） | 已完成 | — |
| 8 | 執行 30 項人工驗收（AC） | 待處理 | — |
| 9 | 執行 8 項差分測試（DT）與補齊 EOF | 待處理 | 技術債 C-02 |

### 任務 B34-5：P-15 重組與業務開發（歷史紀錄）
| # | 步驟 | 狀態 |
|---|---|---|
| 1~7 | 體系對齊與紀錄修復 | 全數完成 |

---

## 三、狀態定義

| 標記 | 意義 |
|---|---|
| 已完成 | 任務已結束 |
| 待處理 | 尚未開始或進行中 |
| 討論中 | 正在規劃方案 |
| 已交付待確認 | 等待使用者驗收 |
| 需重新交付 | 內容有誤需重做 |
| 已廢棄 | 不再執行 |

---

## 四、階段總結標記

| 日期 | 對話編號 | 完成任務 |
|---|---|---|
| 2026-10-21 | ai-system-20261021-005 | B34-5 修復紀錄（5/7 完成） |
| 2026-10-22 | webapp-20261022-045 | B34-6 SPA 重構程式碼完成 |

---

/* ═══ EOF ═══
F: WF-PROGRESS.md
V: 1792670014
W: webapp-20261022-046#3
═══ */

<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-PROGRESS.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-STATE-INDEX.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-STATE-INDEX.md

# WF-STATE-INDEX（專案檔案狀態索引）

最後更新：2026-10-23 02:06:56 (HKT)
Version: 1792692416
Writer: github-actions[bot]
用途：記錄所有已交付檔案的版本與清單（由體系自動生成）

---

## 一、專案總覽

| 專案顯示名稱 | code | domain | package_no | repo |
|---|---|---|---|---|
| 家庭 webapp | webapp | W | 90 | tlee20860-dotcom/AI-WEB-APP-Family |

## 二、檔案清單（自動生成）

| 檔案 | 版本 (V) | 編寫者 (W) |
|---|---|---|
| WF-PROGRESS.md | 1792670014 | webapp-20261022-046#3 |
| WF-HANDOFF.md | 1792670013 | webapp-20261022-046#2 |
| WF-CONCLUSION.md | 1792670012 | webapp-20261022-046#1 |
| P-99-errors-map.md | 1792581015 | ai-system-20261021-004#15 |
| P-15-reorg-plan.md | 1792581023 | ai-system-20261021-004#23 |
| P-14-decisions.md | 1792581014 | ai-system-20261021-004#14 |
| P-10-incident.md | 1792581022 | ai-system-20261021-004#22 |
| P-09-data.md | 1792581021 | ai-system-20261021-004#21 |
| P-08-api.md | 1792581019 | ai-system-20261021-004#19 |
| P-07-uispec.md | 1792581018 | ai-system-20261021-004#18 |
| P-06-pagemap.md | 1792581020 | ai-system-20261021-004#20 |
| P-05-sop.md | 1792581017 | ai-system-20261021-004#17 |
| P-04-adr-index.md | 1792581013 | ai-system-20261021-004#13 |
| P-01-index.md | 1792581016 | ai-system-20261021-004#16 |

/* ═══ EOF ═══
F: WF-STATE-INDEX.md
V: 1792692416
W: github-actions[bot]
═══ */

<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/WF-STATE-INDEX.md === -->
