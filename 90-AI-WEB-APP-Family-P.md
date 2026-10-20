# 專案 P 檔總集：AI-WEB-APP-Family

> 生成時間：2026-10-20 23:48:00 (HKT)


<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-01-index.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-01-index.md

---
name: web-app-family
display: 家庭 webapp
code: webapp
domain: W
package_no: 90
repo: tlee20860-dotcom/WEB-APP-Family
---

# P-01 專案索引 + 技術架構

最後更新：ai-system-20261017-004
Version: 1792222800
Batch: B34-5
Writer: ai-system-20261017-004#027
用途：專案索引（技術架構已抽至 W-*）

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
| 當前批次 | B34-4（完成 + 測試通過） |
| 部署 URL | family-financial-management-system-v2.pages.dev |
| 測試狀態 | 5 組全通過 |
| 下一步 | 執行 P-15 重組計畫 |

---

## 三、架構概覽（詳見 W-*）

| 主題 | 檔案 |
|---|---|
| Web App 領域啟動卡 | W-00-startup.md |
| Firebase RTDB 原則 | W-01-firebase.md |
| Cloudflare Pages Functions | W-02-functions.md |
| 純前端 ES Module 架構 | W-03-esmodule.md |
| PWA + Service Worker | W-04-pwa.md |
| Page Schema | W-05-schema.md |

> **技術架構完整內容已抽至 W-* 層。**

---

## 四、docs 目錄結構（P-15 重組後）

### 4.1 AI-training-files repo

| 層級 | 路徑 | 說明 |
|---|---|---|
| 1 | AI-training-files/ | repo 根 |
| 2 | docs/common/ | C-00 ~ C-10 |
| 2 | docs/skills/W/ | W-00 ~ W-05 |
| 2 | docs/records/ | R-01 |
| 2 | docs/projects/family-fin/ | P-01 ~ P-99 |
| 2 | .github/workflows/ | Actions |
| 1 | STARTUP-core-webapp.md | 自動產 |
| 1 | STARTUP-project-family-fin.md | 自動產 |
| 1 | INDEX.md | 自動產 |

### 4.2 v2.Family repo（程式碼）

| 層級 | 路徑 | 說明 |
|---|---|---|
| 1 | js/config/ | constants / registry |
| 1 | js/core/ | state / db / api / auth |
| 1 | js/lib/ | 通用 lib |
| 1 | js/engines/ | page / data / render |
| 1 | js/blocks/ | 5 大 block |
| 1 | js/ui/ | UI 元件 |
| 1 | js/layout/ | navbar / sidebar / app-shell |
| 1 | js/entity/ | entity 相關 |
| 1 | js/shared/ | 共用元件 |
| 1 | js/pages/ | 純 Schema |
| 1 | functions/api/ | CF Functions |
| 1 | css/ | 6 檔 |
| 1 | *.html | 13 檔 |

---

## 五、本專案檔案結構（實作）

| 目錄 | 檔數 | 內容 |
|---|---|---|
| js/config | 8+1 | constants / firebase-config / app-config / 4 Registry / page-metadata-registry /（待）code-registry |
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

## 六、編號系統（已交付）

| 模組 | 職責 |
|---|---|
| js/lib/code-registry.js | 生成編號（查最大 + 1）、子類短碼自動決定 |
| js/lib/id-resolver.js | 從編號查實體、從編號展開關聯 |
| code-list.html + js/pages/code-list.js | 編號對照清單頁（取代 database.html） |
| POST /api/entity-create | 統一新增端點（含 action 參數） |

**詳細**：P-12 §議題 5 / 6

---

## 七、關聯檔案

| 主題 | 檔案 |
|---|---|
| 進度 | P-02-progress.md |
| 檔案清單 | P-03-manifest.md |
| 決策 | P-04-decisions.md |
| 常見任務 | P-05-sop.md |
| 議題結論 | P-12-conclusion.md |
| 交接快照 | P-13-handoff.md |
| 重組計畫 | P-15-reorg-plan.md |
| Web App 技能包 | W-00 ~ W-05 |
| 機械記錄 | STATE-INDEX.md |

---

/* ═══ EOF ═══
F: docs/P-01-index.md
V: 1792222800
B(歷史): B34-5
W: ai-system-20261017-004#027
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-01-index.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-02-progress.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-02-progress.md

# P-02 交付進度

最後更新：B34-4
Version: 1791903880
Batch: B34-4
Writer: A20261013-004#171
狀態：B34-4 程式碼交付 + 測試通過 + 通用大腦架構設計完成
用途：交付批次追蹤

---

## 總覽

| 批次 | 主題 | 狀態 |
|---|---|---|
| B01 ~ B19 | 重構階段 | ✅ |
| B20 ~ B29 | 測試修復 | ✅ |
| B30-1 ~ B30-9 | 主題 + DB 切換 + 基金 | ✅ |
| B31-1 | 9 個 bug 修正 | ✅ |
| B32-1 | docs 全套重建 + 新機制 | ✅ |
| B32-2 | 修正兩層三反引號規則 | ✅ |
| B33-1 ~ B33-3 | 編號系統設計（議題 1~4）+ C-09 / C-10 建立 | ✅ |
| B34-1 | docs 整合更新（D-001~D-023） | ✅ |
| B34-2 | 議題 5 / 6 結論 + Token 計算原則 + 檔案上限 | ✅ |
| B34-3 | 議題 7 / 9 結論 + 四層架構大重構 | ✅ |
| **B34-4** | **編號系統程式碼交付 + 通用大腦架構設計** | **✅ 完成** |
| **B34-4-測試** | **程式碼整體驗證（5 組全通過）** | **✅ 通過** |

---

## B34-4 總交付清單

### 程式碼（20 檔）

**新增（7 檔）**

| # | 檔案 |
|---|---|
| 1 | js/lib/code-registry.js |
| 2 | js/lib/id-resolver.js |
| 3 | functions/api/_entity.js |
| 4 | code-list.html |
| 5 | js/pages/code-list.js |
| 6 | js/shared/yearrange-card.js |
| 7 | js/ui/dropdown-plus.js |

**修改（13 檔）**

| # | 檔案 |
|---|---|
| 1 | js/entity/entity-definitions.js |
| 2 | js/entity/entity-helpers.js |
| 3 | js/entity/entity-modal.js |
| 4 | js/core/api.js |
| 5 | functions/api/[[path]].js |
| 6 | js/config/constants.js |
| 7 | sw.js |
| 8 | js/config/column-registry.js |
| 9 | js/ui/modal.js |
| 10 | js/pages/settings.js |
| 11 | js/ui/form-builder.js |
| 12 | js/lib/expense-modal.js |
| 13 | js/lib/income-modal.js |

### docs（多次更新）

| 檔案 | 重點 |
|---|---|
| C-00 | §十 貼 URL + §十一 領域詢問 |
| C-02 | §6.5.4 替換對照表 + §十三之一 Repo 用實際名稱 |
| C-03 | 開場 SOP 加 INDEX 流程 |
| C-04 | §一點五 base version |
| C-05 | L-020 / L-021 |
| P-02 | 本檔 |
| P-03 | 更新版本清單 |
| P-13 | 重組進度 |
| P-15 | 重組計畫書 |
| R-01 | §三之一 Repo 清單 |
| W-00 | 新增（補齊 W-* 6 檔） |

---

## B34-4 測試結果（5 組）

| 組 | 項目 | 結果 |
|---|---|---|
| 1 | 核心頁面訪問（code-list / 側邊欄 / 篩選器） | ✅ |
| 2 | 篩選器並排 + 編號 + 保單 + 號 | ✅ |
| 3 | code-list CRUD + 支出 modal + 號 | ✅ |
| 4 | 收入 modal + 號 + 邊緣情境 | ✅ |
| 5 | 跳號 / 無 code 顯示 / 併發 / 權限 / 跨裝置同步 | ✅ |

**測試期間修正**：

| # | 問題 | 修正 |
|---|---|---|
| 1 | yearrange-card.js 誤刪 → settings 空白 | 補上傳（D-109） |
| 2 | code-list 篩選器未並排 | v1.0.1（D-111） |
| 3 | entity-helpers.js 漏 import → members / banks 下拉空 | v103.0.25（D-113） |

---

## B34-4 通用大腦架構設計（P-15）

**核心**：通用大腦（C-*）+ N 領域技能包（W / V / D / M / …）

**產出**：

| 檔案 | 內容 |
|---|---|
| P-15-reorg-plan.md | 重組計畫書 |
| C-00 §十 / §十一 | 貼 URL + 領域詢問 |
| C-02 §6.5.4 / §零點十 / §十三之一 | 替換對照表 + 領域建議 + Repo 實際名稱 |
| C-03 §一 / §五 / §七之四 / §七之五 | INDEX 流程 + 貼 URL |
| R-01 §三之一 | Repo 清單 |
| W-00-startup.md | Web App 領域啟動卡 |

---

## 待執行清單（B34-5，使用者手動）

| # | 項目 | 優先 |
|---|---|---|
| 1 | 上傳 `.github/workflows/setup-structure.yml` | 🔴 高 |
| 2 | 設定 Actions 權限（Read and write） | 🔴 高 |
| 3 | 執行 workflow | 🔴 高 |
| 4 | 驗證新結構（common / skills / records / projects） | 🔴 高 |
| 5 | 上傳新版 C-00 / C-02 / C-03 / R-01 / W-00 / P-13 / P-15 / P-02 / P-03 | 🔴 高 |
| 6 | 開新對話測試 INDEX URL | 🟡 中 |
| 7 | END OF FILE 補齊（~44 檔代碼） | 🟢 低 |
| 8 | 版本號統一（v103.0.xx → epoch） | 🟢 低 |
| 9 | entity-registry 加 code 支援 | 🟢 低 |

---

## 當前狀態

| 項目 | 值 |
|---|---|
| 當前批次 | B34-4（完成） |
| docs 檔數 | 26（目標結構） |
| 程式碼新增 | 7 檔 |
| 程式碼修改 | 13 檔 |
| 下一步 | 執行 P-15 重組 |

---

## 關聯檔案

| 主題 | 檔案 |
|---|---|
| 專案索引 | P-01-index.md |
| 檔案清單 | P-03-manifest.md |
| AI 名單 | R-01-writers.md |
| 議題結論 | P-12-conclusion.md |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: docs/P-02-progress.md
V: 1791903880
B(歷史): B34-4
W: A20261013-004#171
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-02-progress.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-03-manifest.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-03-manifest.md

# P-03 檔案清單

最後更新：B34-4
Version: 1791903820
Batch: B34-4
Writer: A20261013-004#170
用途：所有交付檔案版本 + 批次追蹤

---

## docs/（26 檔，目標結構）

### L1 AI 通用（C-*，10 檔）

| 檔案 | Version | Batch | Writer |
|---|---|---|---|
| C-00-startup.md | 1791903520 | B34-4 | A261013-004#166 |
| C-01-profile.md | 1791896060 | B34-1 | A261013-004#016 |
| C-02-rules.md | 1791903460 | B34-4 | A261013-004#165 |
| C-03-guide.md | 1791903580 | B34-4 | A261013-004#167 |
| C-04-delivery.md | 1791902320 | B34-4 | A261013-004#107 |
| C-05-learning.md | 1791902800 | B34-4 | A261013-004#136 |
| C-06-token-log.md | 1791897660 | B34-2 | A261013-004#036 |
| C-07-summary.md | 1791896360 | B34-1 | A261013-004#018 |
| C-09-rolling-mechanism.md | 1791898780 | B34-3 | A261013-004#057 |
| C-10-handoff-mechanism.md | 1791898840 | B34-3 | A261013-004#057 |

### L2 Web App 通用（W-*，6 檔）

| 檔案 | Version | Batch | Writer |
|---|---|---|---|
| W-00-startup.md | 1791903760 | B34-4 | A261013-004#169 |
| W-01-firebase.md | 1791899500 | B34-3 | A261013-004#065 |
| W-02-functions.md | 1791899560 | B34-3 | A261013-004#065 |
| W-03-esmodule.md | 1791899620 | B34-3 | A261013-004#065 |
| W-04-pwa.md | 1791899680 | B34-3 | A261013-004#065 |
| W-05-schema.md | 1791899740 | B34-3 | A261013-004#065 |

### L3 本專案（P-*，15 檔）

| 檔案 | Version | Batch | Writer |
|---|---|---|---|
| P-01-index.md | 1791898900 | B34-3 | A261013-004#058 |
| P-02-progress.md | 1791902860 | B34-4 | A261013-004#136 |
| P-03-manifest.md | 1791903820 | B34-4 | A261013-004#170 |
| P-04-decisions.md | 1791899080 | B34-3 | A261013-004#059 |
| P-05-sop.md | 1791899800 | B34-3 | A261013-004#067 |
| P-06-pagemap.md | 1791899980 | B34-3 | A261013-004#068 |
| P-07-uispec.md | 1791899860 | B34-3 | A261013-004#067 |
| P-08-api.md | 1791900040 | B34-3 | A261013-004#069 |
| P-09-data.md | 1791899140 | B34-3 | A261013-004#060 |
| P-10-incident.md | 1791899200 | B34-3 | A261013-004#061 |
| P-12-conclusion.md | 1791899320 | B34-3 | A261013-004#064 |
| P-13-handoff.md | 1791903640 | B34-4 | A261013-004#168 |
| P-14-decisions.md | 1791899380 | B34-3 | A261013-004#064 |
| P-15-reorg-plan.md | 1791903700 | B34-4 | A261013-004#168 |
| P-99-errors-map.md | 1791899920 | B34-3 | A261013-004#067 |

### L4 機械記錄（R-*，1 檔）

| 檔案 | Version | Batch | Writer |
|---|---|---|---|
| R-01-writers.md | 1791903400 | B34-4 | A261013-004#164 |

**註**：C-08 已刪除；C-09 / C-10 已改名機制版；P-11 已改名 R-01。

---

## docs/ 目標結構（P-15 重組後）

| 層級 | 路徑 | 檔案數 |
|---|---|---|
| 1 | docs/common/ | 10（C-*） |
| 1 | docs/skills/W/ | 6（W-*） |
| 1 | docs/records/ | 1（R-01） |
| 1 | docs/projects/family-fin/ | 15（P-*） |

---

## 程式碼（B34-4 交付，20 檔）

| 檔案 | 版本 | Batch | Writer |
|---|---|---|---|
| js/lib/code-registry.js | 1791900400 | B34-4 | A261013-004#076 |
| js/lib/id-resolver.js | 1791900460 | B34-4 | A261013-004#076 |
| functions/api/_entity.js | 1791901060 | B34-4 | A261013-004#087 |
| code-list.html | 1791900580 | B34-4 | A261013-004#077 |
| js/pages/code-list.js | 1791902680 | B34-4 | A261013-004#121 |
| js/ui/dropdown-plus.js | 1791901780 | B34-4 | A261013-004#100 |
| js/shared/yearrange-card.js | 1791902620 | B34-4 | A261013-004#118 |
| sw.js | 1791902140 | B34-4 | A261013-004#104 |
| js/entity/entity-definitions.js | 1791901900 | B34-4 | A261013-004#102 |
| js/entity/entity-helpers.js | 1791902740 | B34-4 | A261013-004#131 |
| js/entity/entity-modal.js | 1791901240 | B34-4 | A261013-004#090 |
| js/core/api.js | 1791900880 | B34-4 | A261013-004#082 |
| functions/api/[[path]].js | 1791900940 | B34-4 | A261013-004#083 |
| js/config/constants.js | 1791901300 | B34-4 | A261013-004#091 |
| js/config/column-registry.js | 1791901480 | B34-4 | A261013-004#094 |
| js/ui/modal.js | 1791901420 | B34-4 | A261013-004#093 |
| js/ui/form-builder.js | 1791901960 | B34-4 | A261013-004#103 |
| js/lib/expense-modal.js | 1791902020 | B34-4 | A261013-004#103 |
| js/lib/income-modal.js | 1791902080 | B34-4 | A261013-004#103 |
| js/pages/settings.js | 1791901600 | B34-4 | A261013-004#096 |

---

## 根目錄（2 檔 + 待產 3 檔）

| 檔案 | 狀態 |
|---|---|
| sw.js | ✅ |
| manifest.json | ✅ |
| STARTUP-core-webapp.md | 📋 待 Actions 產出 |
| STARTUP-project-family-fin.md | 📋 待 Actions 產出 |
| INDEX.md | 📋 待 Actions 產出 |

---

## GitHub Actions（1 檔）

| 檔案 | 狀態 |
|---|---|
| .github/workflows/setup-structure.yml | 📋 待上傳（#160 已交內容） |

---

## 統計

| 類別 | 檔案數 |
|---|---|
| docs/ | 26（目標結構） |
| js/* | 97 |
| functions/api/ | 11 |
| CSS | 6 |
| HTML | 13 |
| 根目錄 | 2 + 3（待產） |
| GitHub Actions | 1（待上傳） |
| **總計** | **156** |

---

## 待處理

| 項目 | 狀態 |
|---|---|
| P-15 重組執行（setup-structure.yml） | 📋 使用者手動 |
| W-00-startup.md 上傳 | 📋 使用者手動 |
| STARTUP-* / INDEX.md 產出 | 📋 待 Actions |
| END OF FILE 補齊（~44 檔代碼） | 📋 待處理 |
| 版本號統一（v103.0.xx） | 📋 待處理 |
| entity-registry 加 code 支援 | 📋 待驗證 |

---

## 更新記錄

| 批次 | 更新 |
|---|---|
| **B34-4** | **編號系統程式碼交付（20 檔）+ 通用大腦架構設計 + W-00 補齊** |
| B34-3 | 議題 7 / 9 + 四層架構（35 檔） |
| B34-2 | 議題 5 / 6 + Token 原則 + 上限 800 |
| B34-1 | docs 整合（D-001~D-023） |
| B33-1~3 | 編號系統 + C-09 / C-10 建立 |
| B32-2 | 兩層三反引號 |
| B32-1 | docs 重建（20 檔） |
| B31-1 | 9 個 bug 修正 |

---

## 關聯檔案

| 主題 | 檔案 |
|---|---|
| 專案索引 | P-01-index.md |
| 進度 | P-02-progress.md |
| 決策 | P-04-decisions.md |
| AI 名單 | R-01-writers.md |
| 議題結論 | P-12-conclusion.md |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: docs/P-03-manifest.md
V: 1791903820
B(歷史): B34-4
W: A20261013-004#170
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-03-manifest.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-04-decisions.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-04-decisions.md

# P-04 關鍵決策記錄

最後更新：B34-4
Version: 1791904180
Batch: B34-4
Writer: A20261013-004#177
用途：關鍵決策 + 重構史

---

## 決策索引

| 編號 | 主題 |
|---|---|
| ADR-001~ADR-023 | 早期（B32-1 前，摘要保留） |
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

## ADR-001 ~ ADR-023（摘要保留）

（原內容保留，略）

---

## ADR-024 ~ ADR-038（B32-1 ~ B34-2，摘要）

（原內容保留，略）

---

## ADR-039：四層架構（D-038 / D-072~D-075）

| 項目 | 內容 |
|---|---|
| 背景 | 檔案混雜「通用」與「專屬」 |
| 決策 | 檔案分四層 |
| 層級 | L1 C-* / L2 W-* / L3 P-* / L4 R-* |
| 影響 | C-02 §零點八 |

---

## ADR-040：W-* 抽取（D-076 / D-103）

| 項目 | 內容 |
|---|---|
| 決策 | 先抽 5 個 W-*（W-01~W-05） |
| 來源 | W-01 ← P-09；W-02 ← C-02 §十二 + P-10；W-03 ← P-01；W-04 ← P-10 §十；W-05 ← P-01 §八 |
| 引用保留 | C-02 §十二 保留引用，完整內容在 W-02 |

---

## ADR-041：C-09 / C-10 機制與內容分離（D-102 / D-103）

| 項目 | 內容 |
|---|---|
| 背景 | C-09 已達 620 行，超過合理上限 |
| 決策 | 機制留 C-*，內容搬 P-* |
| 拆分 | C-09-rolling-mechanism + P-12-conclusion；C-10-handoff-mechanism + P-13-handoff |

---

## ADR-042：啟動包三種（D-042~D-045）

| 項目 | 內容 |
|---|---|
| 決策 | 1. 本專案續用 / 2. 本專案新對話 / 3. 全新專案 |
| 本專案續用 | C-00 + R-01 + P-12 + P-13 |
| 本專案新對話 | C-00 + R-01 + P-12 |
| 全新專案 | C-00~C-07 + W-01~W-05 |

---

## ADR-043：R-01 專案索引（D-046 / D-058）

| 項目 | 內容 |
|---|---|
| 決策 | P-11-writers.md → R-01-writers.md |
| 新增章節 | §三 專案清單（含領域欄） |
| 影響 | C-00 §二 / C-02 §九 / C-03 |

---

## ADR-044：AI 自動識別專案（D-047 / D-049）

| 項目 | 內容 |
|---|---|
| 決策 | AI 依 P-01 內容判斷專案；新專案主動問使用者 |

---

## ADR-045：議題表驅動制（D-050~D-057）

| 項目 | 內容 |
|---|---|
| 決策 | AI 每次對話開場，主動提示下一議題 |
| 載體 | P-12 §一 |
| 規則寫入 | C-02 §零點九 + C-03 §七之三 |

---

## ADR-046：檔案上限 800 行（D-037）

| 項目 | 內容 |
|---|---|
| 決策 | 上限 500 → 800 行 |
| 影響 | C-02 §六 |

---

## ADR-047：議題結論摘要機制（D-030）

| 項目 | 內容 |
|---|---|
| 決策 | 每 2~3 議題結案，回覆末尾附「議題結論摘要」 |
| 性質 | AI 強制執行 |

---

## ADR-048：Token 視覺化「剩餘: X%」（D-022）

| 項目 | 內容 |
|---|---|
| 決策 | 格式固定「Token: {emoji} 剩餘: {pct}%」，位置一律回覆末尾 |

---

## ADR-049：提問附人話解釋（D-028）

| 項目 | 內容 |
|---|---|
| 決策 | 每題選項下方加「> 人話解釋：」 |
| 寫入 | C-02 §二之一 + C-01 §一 |

---

## ADR-050：議題結論摘要（D-030）

同 ADR-047。

---

## ADR-051：編號對照清單：單頁 + 篩選器（D-031 / D-032）

| 項目 | 內容 |
|---|---|
| 決策 | database.html → code-list.html；單頁 + 篩選器 + 兩區塊 |
| 影響 | P-12 §議題 6 |

---

## ADR-052：entity-create 統一端點（D-029 / D-063）

| 項目 | 內容 |
|---|---|
| 決策 | 單一端點 + action 參數（create / update / delete） |
| 檔案 | functions/api/_entity.js |
| 影響 | P-08 / P-12 §議題 5 |

---

## ADR-053：舊端點全廢（D-064 / D-029）

| 項目 | 內容 |
|---|---|
| 決策 | 廢棄全部 CRUD 端點，只留 GET + 特殊邏輯 |
| 時間點 | 程式碼交付批次 |

---

## ADR-054：保單子類短碼 3 種（D-060）

| 項目 | 內容 |
|---|---|
| 決策 | NORM（普通類）/ FUND（基金類）/ SAVE（儲蓄類） |
| 影響 | code-registry.js / entity-definitions.js |

---

## ADR-055：+ 號按鈕（D-029）

| 項目 | 內容 |
|---|---|
| 決策 | dropdown-plus.js + form-builder 支援 plusEntity |
| 影響 | 5 個 select 加 plusEntity |

---

## ADR-056：base version 標示（D-101）

| 項目 | 內容 |
|---|---|
| 決策 | 修改檔交付時標明「基於 #NNN 的 vX.Y.Z」 |
| 寫入 | C-04 §一點五 |

---

## ADR-057：先看檔案再交付（D-100）

| 項目 | 內容 |
|---|---|
| 決策 | 修改檔交付前必先讀當前版本 |
| 寫入 | C-02 §四之一 |

---

## ADR-058：Token < 20% 強制重估（D-102）

| 項目 | 內容 |
|---|---|
| 決策 | 估算 < 20% → 不得直接報告，必須重估 |
| 寫入 | C-02 §零點五 |

---

## ADR-059：Repo 用實際名稱（D-133）

| 項目 | 內容 |
|---|---|
| 決策 | 禁止 Repo A / Repo B 代稱 |
| 清單位置 | R-01 §三之一（SSOT） |
| 寫入 | C-02 §十三之一 |

---

## ADR-060：通用大腦 + N 領域技能包（D-118）

| 項目 | 內容 |
|---|---|
| 決策 | C-* 通用 + W/V/D/M 領域技能包 |
| 影響 | P-15 重組計畫書 |

---

## ADR-061：貼 URL 啟動機制（D-136）

| 項目 | 內容 |
|---|---|
| 決策 | 不上傳檔案，改貼 2 條 STARTUP URL |
| 機制 | GitHub Actions 自動合併 |
| 寫入 | C-00 §十 / C-03 §五 |

---

## ADR-062：AI 主動建議領域（D-137）

| 項目 | 內容 |
|---|---|
| 決策 | 使用者選「新專案」→ AI 先問領域 |
| 新領域 | AI 主動建議建 6 檔 |
| 寫入 | C-00 §十一 / C-02 §零點十 |

---

## ADR-063：替換對照表（D-128）

| 項目 | 內容 |
|---|---|
| 決策 | C-02 §6.5.4 加「替換對照表」 |
| 目的 | 一次性解決 md 三反引號問題 |

---

## ADR-064：領域技能包統一 6 檔模板（D-121）

| 項目 | 內容 |
|---|---|
| 決策 | 每個領域 `{X}-*` 一律 6 檔 |
| 6 檔 | {X}-00-startup / 01-structure / 02-rules / 03-sop / 04-templates / 99-errors-map |

---

## 重構歷程摘要

（原內容保留，略）

---

## 關聯檔案

| 主題 | 檔案 |
|---|---|
| 通用規則 | C-02-rules.md |
| 專案索引 | P-01-index.md |
| 事故日誌 | P-10-incident.md |
| 議題結論 | P-12-conclusion.md |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: docs/P-04-decisions.md
V: 1791904180
B(歷史): B34-4
W: A20261013-004#177
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-04-decisions.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-05-sop.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-05-sop.md

# P-05 常見任務 SOP

最後更新：B34-4
Version: 1791904300
Batch: B34-4
Writer: A20261013-004#179
用途：AI 執行常見任務的具體步驟

---

## 一、修 Bug

| 步驟 | 動作 |
|---|---|
| 1 | 讀 P-10-incident |
| 2 | 讀 P-99-errors-map |
| 3 | 跑 5 問自檢（C-02 §四） |
| 4 | 複現問題 |
| 5 | 診斷根本原因 |
| 6 | 提供「診斷 + 方案」 |
| 7 | 待確認 |
| 8 | 交付 |
| 9 | 更新 P-03 + P-02 |
| 10 | 記錄到 P-10 / C-05 |

---

## 二、新增頁面

| 步驟 | 動作 |
|---|---|
| 1 | 讀 W-03 + W-05 |
| 2 | 讀 P-01-index 檔案結構 |
| 3 | 檢查 P-03 是否可複用 |
| 4 | 寫 js/pages/xxx.js（Schema） |
| 5 | 寫 xxx.html |
| 6 | 更新 constants.js 的 SIDEBAR_GROUPS / ROUTES |
| 7 | 更新 sw.js 的 STATIC_ASSETS（見 W-04） |
| 8 | 更新 P-06-pagemap 頁面區塊規格 |
| 9 | 更新 P-03 + P-02 |

---

## 三、改 Block

| 步驟 | 動作 |
|---|---|
| 1 | grep 找出所有使用頁 |
| 2 | 評估向後相容 |
| 3 | 若破壞性 → 加參數 |
| 4 | 改 block |
| 5 | 驗證所有使用頁 |
| 6 | 更新 P-03 |

---

## 四、改 Registry

| 步驟 | 動作 |
|---|---|
| 1 | 讀 P-08-api |
| 2 | grep 使用檔案 |
| 3 | 加新 API（不破壞舊） |
| 4 | 改 registry |
| 5 | 更新 P-08 + P-03 |

**禁止**：改 export 名 / 改回傳格式 / 刪 export。

---

## 五、改 Lib

| 步驟 | 動作 |
|---|---|
| 1 | 檢查是否可複用 |
| 2 | 評估抽新 lib 或擴充 |
| 3 | 改 lib |
| 4 | 更新 P-03 |

**判斷準則**：

| 情境 | 做法 |
|---|---|
| 只有 1 頁用 | 留在原檔 |
| 2+ 頁用 | 抽 lib |
| 未來會擴充 | 抽 lib |

---

## 六、改 Functions API（見 W-02）

| 步驟 | 動作 |
|---|---|
| 1 | 讀 P-08-api + W-02 |
| 2 | 決定哪個 _xxx.js |
| 3 | 改 handler |
| 4 | 若加新端點 → 改 [[path]].js 分派 |
| 5 | 更新 P-08 + P-03 |

**禁止**：新增獨立 xxx.js / 用 shim re-export。

---

## 七、改資料結構（見 W-01）

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
| 3 | 更新 Version + Batch |
| 4 | 交付完整檔案 |

**變更影響矩陣**：

| 改動 | 同步更新 |
|---|---|
| 新增頁面 | P-03 / P-02 / P-06 / sw.js / constants.js |
| 改 API | P-08 / P-03 |
| 改資料結構 | P-09-data |
| 改 Registry API | P-08 / P-03 |
| 修 Bug | P-10 / P-02 / P-03 |
| 改 UI 元件 | P-07 / P-03 |
| 改 Web App 通用 | W-* |
| 新增領域 | C-00 §九 / R-01 §三之一 |

---

## 九、部署後驗證

| 步驟 | 動作 |
|---|---|
| 1 | 訪問主要頁面 |
| 2 | vConsole 檢查 Error |
| 3 | 切換年月 → 驗證連動 |
| 4 | 檢查 API |
| 5 | 手機測試 |

**驗證清單**：

| 頁面 | 檢查項 |
|---|---|
| index.html | 統計卡有數字 |
| settings.html | 3 Tab 可切換 |
| code-list.html | 篩選器 + 兩區塊 |
| input-center.html | 5 按鈕可開啟 Modal |
| settlements.html | 表格 + toggle |
| insurance.html | 卡片 + 已供滿展開 |

---

## 十、緊急回滾

| 步驟 | 動作 |
|---|---|
| 1 | GitHub 找上一個 commit |
| 2 | Revert |
| 3 | CF Pages 自動重部署 |
| 4 | 分析後重做 |

**禁止**：手動改線上檔 / 強制 push。

---

## 十一、執行一次性遷移

| 步驟 | 動作 |
|---|---|
| 1 | 進「系統設定 → 個人化」 |
| 2 | 找「一次性資料遷移」卡 |
| 3 | 選目標家庭 |
| 4 | 點「執行遷移」 |
| 5 | 確認 → 執行 |
| 6 | 查看統計 |

---

## 十二、改版面

| 步驟 | 動作 |
|---|---|
| 1 | 讀 P-06-pagemap「頁面區塊規格」 |
| 2 | 列出「變更前後對照」 |
| 3 | 待使用者確認 |
| 4 | 交付時在摘要標示 |

**允許 / 禁止**：

| 動作 | 允許 |
|---|---|
| 改區塊內部實作 | ✅ |
| 新增區塊（標示） | ✅ |
| 使用者明確要求刪除 | ✅ |
| 未確認刪除區塊 | ❌ |
| 未確認改容器 ID | ❌ |

---

## 十三、貼 URL 啟動（D-136）

### 13.1 使用者貼 2 條 STARTUP URL

| 步驟 | AI 動作 |
|---|---|
| 1 | 讀取 URL 內容 |
| 2 | 生成對話編號 |
| 3 | 提示下一議題 + 列 4 選項 |
| 4 | 依情境處理 |

### 13.2 使用者貼 INDEX.md URL

| 步驟 | AI 動作 |
|---|---|
| 1 | 讀取 INDEX.md |
| 2 | 列出所有專案 + 領域 |
| 3 | 問「要哪個專案」 |
| 4 | 給 2 條 STARTUP URL |
| 5 | 使用者貼 2 條 URL |
| 6 | 後續同 13.1 |

### 13.3 使用者選「新專案」

| 步驟 | AI 動作 |
|---|---|
| 1 | **問「這個專案屬於哪個領域？」** |
| 2 | 使用者回答（Web App / VBA / …） |
| 3a | 已有領域 → 直接開新專案 |
| 3b | 新領域 → 主動建議建 6 檔 |

---

## 十四、領域技能包建立 SOP（D-121）

**觸發**：使用者選「新專案」+ 該領域為新領域。

| 步驟 | AI 動作 |
|---|---|
| 1 | 依使用者描述產 `{X}-00-startup.md` 草稿 |
| 2 | 依通用知識產 `{X}-01-structure.md` |
| 3 | 依通用知識產 `{X}-02-rules.md` |
| 4 | 依通用知識產 `{X}-03-sop.md` |
| 5 | 依通用知識產 `{X}-04-templates.md` |
| 6 | 產 `{X}-99-errors-map.md`（初版可空） |
| 7 | 交付 6 檔，請使用者確認 |
| 8 | 依回饋迭代 |
| 9 | 更新 C-00 §九、R-01 §三之一 |
| 10 | 上傳至 docs/skills/{X}/ |

**6 檔模板**：見 C-00 §十一。

---

## 十五、任務速查

| 任務 | 章節 |
|---|---|
| 修 Bug | 一 |
| 新增頁面 | 二 |
| 改 Block | 三 |
| 改 Registry | 四 |
| 改 Lib | 五 |
| 改 Functions API | 六 |
| 改資料結構 | 七 |
| 更新 docs | 八 |
| 部署後驗證 | 九 |
| 緊急回滾 | 十 |
| 執行遷移 | 十一 |
| 改版面 | 十二 |
| 貼 URL 啟動 | 十三 |
| 建立領域技能包 | 十四 |

---

## 十六、關聯檔案

| 主題 | 檔案 |
|---|---|
| 通用規則 | C-02-rules.md |
| 流程指南 | C-03-guide.md |
| 檔案清單 | P-03-manifest.md |
| API 契約 | P-08-api.md |
| 資料結構 | P-09-data.md |
| 錯誤摘要 | P-99-errors-map.md |
| Web App 技能包 | W-00 ~ W-05 |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: docs/P-05-sop.md
V: 1791904300
B(歷史): B34-4
W: A20261013-004#179
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-05-sop.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-06-pagemap.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-06-pagemap.md

# P-06 頁面 → 檔案對照表

最後更新：B34-4
Version: 1791904480
Batch: B34-4
Writer: A20261013-004#183
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

**變更**：
- database.html → code-list.html（已交付）
- database-*.js → 全部廢棄（已完成）

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
| 檔案清單 | P-03-manifest.md |
| 議題結論 | P-12-conclusion.md |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: docs/P-06-pagemap.md
V: 1791904480
B(歷史): B34-4
W: A20261013-004#183
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-06-pagemap.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-07-uispec.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-07-uispec.md

# P-07 UI 規格書 + 業務術語

最後更新：B34-3
Version：1791899860
Batch: B34-3
Writer: A20261013-004#067
用途：UI 術語 + 頁面結構 + 業務術語

---

## 一、頁面結構層級

| 層級 | 術語 | 說明 | 範例 |
|---|---|---|---|
| L1 | Page（頁面） | 一個 .html | index.html |
| L2 | Section（區塊） | 頁面內垂直堆疊的功能區 | page-header / stats |
| L3 | Row（列） | Section 內水平排列 | toolbar row |
| L4 | Item（元素） | 最小單位 | button / select |

---

## 二、Section 類型

| 術語 | 中文 | 用途 | 標準位置 |
|---|---|---|---|
| page-banner | 提示橫幅 | 說明文字 | 頁面最上 |
| page-header | 頁首 | 標題 + 副標題 + 主要操作 | banner 之後 |
| page-header-actions | 頁首操作區 | 標題行右側 | page-header 內 |
| stats | 統計卡區 | 數字統計 | header 之後 |
| page-controls | 控制列 | 篩選 / 切換 / 年月 | stats 之後 |
| content | 內容區 | 表格 / 卡片 | 最底 |
| empty-state | 空狀態 | 無資料時顯示 | 內容區內 |
| tab-bar | 分頁列 | 同頁多分頁 | 各處 |
| tab-panel | 分頁內容 | Tab 對應內容 | 分頁列下方 |
| sidebar | 側邊欄 | 左側導覽 | 頁面左側 |
| navbar | 頂部導覽 | 使用者資訊 | 頁面頂部 |

---

## 三、控制列元素

| 術語 | type | 用途 | 位置 |
|---|---|---|---|
| add | add | 新增按鈕 | toolbar 左 |
| export | export | 匯出按鈕 | page-header 右 / toolbar 右 |
| viewToggle | viewToggle | 卡片 / 表格切換 | 強制 body |
| viewSwitcher | viewSwitcher | 檢視切換 | body 全行 |
| sort | sort | 排序下拉 | 強制 body |
| year | year | 年份下拉 | body 左側 |
| month | month | 月份下拉 | body 左側 |
| filter | filter | 篩選器 | body 全行 |
| custom | custom | 自訂按鈕 | 依需求 |

---

## 四、頁面標準佈局

| 順序 | Section | 內容 |
|---|---|---|
| 1 | page-banner | banner（若有） |
| 2 | page-header | 標題 + 副標題 + 主要操作 |
| 3 | stats | 統計卡 |
| 4 | page-controls | 新增 / 切換 / 年月 / 篩選 |
| 5 | content | 表格 / 卡片 |

---

## 五、資料呈現元件

| 術語 | 中文 | 判斷準則 |
|---|---|---|
| table | 表格 | 欄位多、需對齊比較 |
| card | 卡片 | 手機 / 少量欄位 |
| tab | 分頁 | 同頁多個獨立區塊 |
| collapsible | 可摺疊 | 次要內容 |
| modal | 彈窗 | 編輯 / 確認對話框 |
| form | 表單 | 資料輸入欄位 |
| badge | 標籤 | 狀態小標籤 |
| progress | 進度條 | 進度視覺化 |

---

## 六、表頭互動（data-table）

**表頭可排序**：

| 動作 | 結果 |
|---|---|
| 點欄位標題（首次） | 升序 |
| 點欄位標題（第二次） | 降序 |
| 點欄位標題（第三次） | 清除排序 |

**表頭可篩選**：

| 動作 | 結果 |
|---|---|
| 點欄位旁的 🔽 | 開下拉面板 |
| 選值 → 套用 | 篩選生效 |
| 點清除 | 清除篩選 |

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

**模組**：js/config/page-metadata-registry.js
**儲存**：families/{familyId}/settings/page_metadata/{pageKey}
**結構**：{ title, subtitleStatic }
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

## 十一、業務術語

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

### 編號系統類（新）

| 術語 | 說明 |
|---|---|
| code | 實體編號（如 POL-001） |
| code-registry | 編號生成模組 |
| id-resolver | 編號查詢模組 |
| entity-create | 統一新增端點 |
| 編號對照清單 | code-list.html（原基礎資料庫） |
| 大類前綴 | MEM / PAY / POL / FUND / CAT / ITEM / COMP / INC / TXN |
| 子類短碼 | FUND / NORM / FUNIN / SAVE / SCH / TRA / MED / BNK / OTH |

---

## 十二、關聯檔案

| 主題 | 檔案 |
|---|---|
| 頁面區塊規格 | P-06-pagemap.md |
| 通用規則 | C-02-rules.md |
| 控制列實作 | js/lib/page-controls.js |
| 表頭互動實作 | js/shared/data-table.js |
| 資料結構 | P-09-data.md |
| 議題結論 | P-12-conclusion.md |

---

/* ═══ EOF ═══
F: docs/P-07-uispec.md
V: 1791899860
B(歷史): B34-3
W: A20261013-004#067
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-07-uispec.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-08-api.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-08-api.md

# P-08 API 契約

最後更新：B34-4
Version: 1791904360
Batch: B34-4
Writer: A20261013-004#180
用途：前後端介面參考

---

## 一、前端模組 API

### status-registry.js

| API | 說明 |
|---|---|
| normalize(raw) | 任意輸入 → 標準代碼 |
| isDone(raw) | 是否完成 |
| label(raw, opts) | 顯示文字（依 source） |
| options(opts) | 下拉選項陣列 |
| badgeClass(raw) | badge class |
| toLegacyName(code, source) | 新代碼 → 舊名稱 |
| isPending(raw) | 是否 pending |
| isSkipped(raw) | 是否 skipped |

**常數**：STATUS / IS_DONE / LABELS / LEGACY_MAP

---

### entity-registry.js

| API | 說明 |
|---|---|
| init() | 初始化訂閱 |
| destroy() | 清理 |
| resolveName(type, id, fallback) | ID → 名稱 |
| getList(type) | 列表 |
| getNameMap(type) | { id: name } |
| isInitialized() | 是否已初始化 |
| resolveNames(type, ids) | 批次解析 |
| resolveEntityName(key, id, fallback) | 依 ENTITY_KEYS 解析 |

---

### label-registry.js

| API | 說明 |
|---|---|
| L(path, params) | 依路徑取得文字 |
| has(path) | 是否存在 |
| getGroup(name) | 取得整個分類 |

---

### column-registry.js

| API | 說明 |
|---|---|
| getColumns(key) | 欄位定義 |
| getResolvers(key) | 值轉換函式 |
| getColumnsWith(key, extra) | 追加欄位 |

**COLUMNS key（19 種）**：
members / banks / bankAccounts / categories / items / payments / statuses / companies / funds / fundSnapshots / insurance / insurancePolicies / settlements / bankTransactions / recentExpense / memberReport / dashboardAnnual / annualSummary / annualMonthly / **codeList（🆕 B34-4）**

---

### page-metadata-registry.js

| API | 說明 |
|---|---|
| getPageKeyFromUrl() | 從 URL 取 pageKey |
| getDefaultMetadata(pageKey) | 取預設 metadata |
| listPages() | 列出所有可編輯頁面 |

---

### entity-resolvers.js

| API | 說明 |
|---|---|
| getEntityResolvers(entityKey) | 取得該 entity 的 resolvers |

---

### entity-funds-allocation.js

| API | 說明 |
|---|---|
| renderFundsAllocationField(root, initialAlloc, policyType, opts) | 渲染關聯基金欄位 |

**opts**：{ defaultYear, defaultMonth }
**回傳**：getValue() 函式

---

### code-registry.js（✅ B34-4 已交付）

| API | 說明 |
|---|---|
| generateCode(prefix, 現有實體列表) | 生成編號（查最大 + 1） |
| generateWithSubtype(prefix, subtype, 實體列表) | 帶子類短碼 |
| checkDuplicate(code, 實體列表) | 檢查重複 |
| getSubtypeFromContext(entityType, extra) | 依上下文自動決定子類短碼 |
| generateCodeFor(entityType, extra) | **主入口**：依 entityType 生成完整編號 |

**常數**：

| 常數 | 內容 |
|---|---|
| PREFIX_MAP | member → MEM / policy → POL / … |
| POLICY_SUBTYPE_MAP | normal → NORM / fund_insurance → FUND / savings → SAVE |
| CATEGORY_SHORTCODES | 學校類 → SCH / 交通類 → TRA / 醫療類 → MED / 銀行類 → BNK / 其他 → OTH |

---

### id-resolver.js（✅ B34-4 已交付）

| API | 說明 |
|---|---|
| getEntityByCode(entityType, code) | 從編號查實體（遍歷） |
| getRelatedEntities(code) | 從編號展開所有關聯 |

**禁止**：從名稱查編號（不做）。

---

### constants.js

| 類別 | 常數 |
|---|---|
| 版本 | APP_VERSION / SW_VERSION |
| Storage | STORAGE_KEYS / STORAGE_PREFIXES / SESSION_KEYS |
| TTL | AUTH_CONTEXT_TTL_MS / APP_CONFIG_TTL_MS / BANK_CACHE_TTL_MS |
| 限制 | LIMITS |
| ID | RESERVED_IDS |
| 角色 | ROLES / SOURCES |
| 前綴 | LINKED_PREFIX |
| 銀行 | BANK_TXN_TYPES / BANK_TXN_CATEGORIES / BANK_ACCOUNT_TYPES |
| 支付 | PAYMENT_MODES |
| 實體 | ENTITY_KEYS |
| **編號前綴（🆕 B34-4）** | **CODE_PREFIXES / POLICY_SUBTYPE_MAP** |
| 狀態 | STATUS_BADGE_CLASS / STATUS_CATEGORIES |
| 導覽 | SIDEBAR_GROUPS / ROUTES |
| 預設 | DEFAULT_* |
| 平台 | PLATFORM_RESOURCES / SUPERADMIN_* |

| 函式 | 說明 |
|---|---|
| buildLinkedKey(policyId) | linked_ 前綴 |
| isLinkedKey(key) | 判定 |
| getPolicyEffectiveMemberId(policy) | 保單持有人 fallback |
| buildColumnSettingsKey(tableId) | 欄位設定 key |
| getStatusBadgeClass(isDone, isSkipped) | badge class |
| getSidebarGroupsByRole(role) | 依角色過濾群組 |

---

### page-engine.js

| API | 說明 |
|---|---|
| createPage(schema) | 建立頁面 → { ctx, destroy, ready } |

**ctx 提供**：

| 方法 | 說明 |
|---|---|
| ctx.onDataChange(fn) | 訂閱資料變更 |
| ctx.setState(keyPath, val) | 設 state |
| ctx.invalidate(key) | 觸發重算 |

---

### data-engine.js

| API | 說明 |
|---|---|
| subscribe(cfg, cb) | 訂閱 |
| fetchOnce(cfg) | 一次性讀取 |
| makeReactive(obj, cb) | 響應式代理 |
| resolvePath(raw, params) | 路徑解析 |
| getNestedValue(obj, path) | 深層取值 |
| setNestedValue(obj, path, val) | 深層設值 |

---

### render-engine.js

| API | 說明 |
|---|---|
| resolveExpr(expr, ctx) | 解析單一表達式 |
| resolveDeep(obj, ctx) | 遞迴解析 |
| mountBlock(block, ctx) | 掛載 block |
| collectBlockDeps(block) | 掃描依賴 |

---

### page-controls.js

| API | 說明 |
|---|---|
| renderPageControls(config) | 渲染控制列 |
| destroyPageControls(instance) | 清理 |

**內建 type**：add / export / viewToggle / viewSwitcher / sort / year / month / filter / custom

---

### data-table.js

| API | 說明 |
|---|---|
| renderDataTable(options) | 渲染表格（含表頭排序 / 篩選） |

**回傳**：{ container, table, refresh, destroy }

---

### data-card.js

| API | 說明 |
|---|---|
| renderDataCard(options) | 渲染卡片（含 customActions） |

---

### lib/expense-modal.js

| API | 說明 |
|---|---|
| openExpenseModal({ row, mode, onSuccess }) | mode: 'data' \| 'settle' |

---

### lib/income-modal.js

| API | 說明 |
|---|---|
| openContributionModal({ onSuccess }) | 家用轉入 |
| openPersonalIncomeModal({ onSuccess }) | 個人收入 |

---

### lib/filter-sort.js

| API | 說明 |
|---|---|
| applyFilters(list, filters, opts) | 篩選 |
| applySort(list, mode, modes) | 排序 |

---

### lib/dom.js

| API | 說明 |
|---|---|
| qs / qsa | 選取 |
| esc(v, fallback) | HTML 逸出（SSOT） |
| when / unless | 條件執行 |
| el(tag, attrs, children) | 建立元素 |

---

### lib/async.js

| API | 說明 |
|---|---|
| withToast / withConfirm / withAsyncState / safe | 非同步輔助 |

---

### lib/lifecycle.js

| API | 說明 |
|---|---|
| createCleanupRegistry() | 回傳 { add, addAll, remove, size, run, isDestroyed } |

---

### lib/format.js

| API | 說明 |
|---|---|
| formatHKD / formatNumber / roundHKD / clampAmount | 金額 |
| formatPercent | 百分比 |
| formatCellValue | 通用格式化 |
| formatTransactionType / formatTransactionCategory / getCategoryBadgeClass | 交易 |

---

### lib/registry.js

| API | 說明 |
|---|---|
| initAllRegistries() | 初始化全部 |
| destroyAllRegistries() | 銷毀全部 |
| isAllRegistriesReady() | 查詢狀態 |

---

### lib/merge.js

| API | 說明 |
|---|---|
| mergeSettlementData({ memberExpenses, insuranceRows, year, month }) | 結算合併 |
| resetInsuranceStatusCache() | 重置快取 |

---

### lib/insurance.js

（同前，未變）

---

### lib/bank.js

（同前，未變）

---

### core/db.js

（同前，未變，v103.0.36）

---

### core/debug.js

（同前，未變）

---

### core/state.js（AppState）

（同前，未變）

---

### core/api.js（✅ B34-4 加 entityCreate）

| API | 說明 |
|---|---|
| callApi(path, opts) | 通用呼叫 |
| api.lookupFamily() | 家庭查詢 |
| api.summary(y, m) | 單月摘要 |
| api.fetchAnnualSummary(y) | 年度聚合 |
| api.fetchAnnualSummaryRange(s, e) | 年度範圍 |
| api.fetchSettlementsYear(y) | 結算年度 |
| api.insuranceSync(payload) | 保險扣款觸發連動 |
| api.insuranceUnsync(payload) | 取消扣款 |
| **api.entityCreate(payload)** | **🆕 統一新增端點** |
| api.adminListFamilies / adminAddFamily / adminRemoveFamily / adminInitFamily | 平台家庭管理 |
| api.adminMigrate(familyId) | 一次性資料遷移 |
| api.familyAccounts.* | 家庭帳號 CRUD |
| api.platformSettings.* | 平台設定 |
| api.platformDefaults.* | 平台預設 CRUD |
| api.familySettings.* | 家庭設定 |
| api.bankAccounts.* | 銀行帳號 |
| api.bankTransactions.* | 銀行交易 |
| api.personalIncome.* | 個人收入 |
| api.memberAdvances.* | 成員代墊 |
| api.clearBankBalances() | 清除舊結餘 |

---

### ui/dropdown-plus.js（✅ B34-4 已交付）

| API | 說明 |
|---|---|
| isSimpleEntity(entityType) | 判定是否為簡單實體 |
| handlePlusClick(entityType, context) | 主入口：prompt 或 modal |
| attachPlusButton(selectEl, entityType, onCreated) | 為 select 掛 + 號 |

**簡單實體清單**：member / bank / category / payment / company

---

### ui/modal.js（✅ B34-4 加 openPrompt）

| API | 說明 |
|---|---|
| openModal(target, options) | 開啟 Modal |
| closeModal(target) | 關閉 |
| closeAllModals() | 全部關閉 |
| openConfirm(message, options) | 確認對話框 |
| **openPrompt(options)** | **🆕 輕量輸入框** |
| bindModalEscClose() | 綁定 ESC 關閉 |
| bindModalBackdropClose(modalId) | 綁定背景點擊關閉 |

---

### ui/form-builder.js（✅ B34-4 支援 plusEntity）

| API | 說明 |
|---|---|
| buildForm(options) | 建構表單 → api 物件 |

**api 提供**：getData / setData / setFieldValue / getFieldValue / getFieldEl / reset / setError / clearErrors / updateOptions / onFieldChange / submit / setSubmitting / destroy

**新支援欄位屬性**：

| 屬性 | 說明 |
|---|---|
| plusEntity | 自動生成 + 號按鈕 |
| plusValueKey | 自動選中的欄位（預設 'id'） |
| onPlusReload | 自訂重載 callback |

---

### shared/yearrange-card.js（✅ B34-4 已交付）

| API | 說明 |
|---|---|
| initYearRangeCard(containerId) | 初始化年份範圍卡 |

---

### entity-definitions.js（✅ B34-4 加 codePrefix）

（同前，加 codePrefix / policySubtypes 屬性）

---

### entity-helpers.js（✅ B34-4 全改走 API）

| API | 說明 |
|---|---|
| createEntity(entityKey, data, allRows) | 走 API + code-registry → 回傳 { id, code } |
| updateEntity(entityKey, id, data, allRows) | 走 API → 回傳 { id, code } |
| removeEntity(entityKey, id) | 走 API |
| deleteEntity(entityKey, id, ...args) | 有特殊 delete 則走本地 |
| getDynamicOptions(source, context) | 動態選項 |
| updateEntityStatus(source, row, newStatus) | 跨來源改狀態 |
| listenEntity(entityKey, cb, err) | 監聽 |

---

### entity-modal.js（✅ B34-4 回傳 { id, code }）

（同前，onSuccess 接收 { id, code, data }）

---

## 二、後端 API 契約

所有 URL 路徑保持不變；實作改為 functions/api/[[path]].js Catch-all 分派。

### GET 端點

| 路徑 | 說明 |
|---|---|
| /api/family-settings | 家庭設定 |
| /api/family-accounts | 家庭成員帳號 |
| /api/admin-families | 平台家庭列表 |
| /api/summary | 單月摘要 |
| /api/annual-summary | 年度聚合 |
| /api/settlements-year | 結算年度 |
| /api/platform-settings | 平台設定 |
| /api/platform-defaults | 平台預設資料庫 |
| /api/bank-accounts | 銀行帳號列表 |
| /api/bank-transactions | 銀行交易列表 |
| /api/personal-income | 個人收入列表 |
| /api/member-advances | 成員代墊列表 |

### POST 端點

| 路徑 | action | 說明 |
|---|---|---|
| /api/lookup-family | — | 查詢所屬家庭 |
| /api/family-settings | update / put-status / delete-status | 家庭設定 |
| /api/family-accounts | create / restore / update / remove | 帳號 CRUD |
| /api/admin-families | add / remove | 家庭 CRUD |
| /api/admin-init-family | — | 初始化家庭 |
| /api/admin-migrate | — | 一次性資料遷移 |
| /api/platform-settings | update | 平台設定 |
| /api/platform-defaults | put / delete / set | 平台預設 CRUD |
| /api/bank-accounts | create / update / remove | 銀行帳號 CRUD |
| /api/bank-transactions | create / update / remove | 銀行交易 CRUD |
| /api/clear-bank-balances | — | 清除舊結餘 |
| /api/personal-income | save / remove | 個人收入 |
| /api/member-advances | create / update / remove | 成員代墊 |
| /api/insurance-sync | upsert / delete | 保險同步 + 基金累積 |
| **/api/entity-create** | **create / update / delete** | **🆕 統一 CRUD** |

---

### POST /api/entity-create（✅ B34-4 已交付）

**Request**：

| 欄位 | 類型 | 說明 |
|---|---|---|
| entityType | string | 見下表 |
| action | string | create / update / delete |
| code | string | 前端生成的編號（create 用） |
| name | string | 名稱 |
| id | string | 實體 ID（update / delete 用） |
| extra | object | 依類型不同 |

**entityType 清單與 extra 欄位**：

| entityType | extra 必填 | extra 可選 |
|---|---|---|
| member | — | role, order |
| bank | — | order |
| category | shortCode | order |
| item | categoryId | — |
| payment | — | order |
| company | — | — |
| policy | type, memberId, firstStartYear, firstStartMonth, totalPolicyYears | fundsAllocation, monthlyPremium, annualPremium |
| fund | type | policyId, cost, initialYear, initialMonth |
| bankAccount | bankId, type, ownerType | ownerId, initialBalance, initialYear, initialMonth |

**成功回應**：{ ok: true, id, code }

**失敗回應**：

| error | 觸發 |
|---|---|
| CODE_DUPLICATE | 編號已存在 |
| NAME_DUPLICATE | 名稱重複 |
| MISSING_FIELDS | 必填欄位缺少 |
| INVALID_BODY | JSON 格式錯誤 |
| UNAUTHORIZED | 未登入 |
| FORBIDDEN | 無權限 |
| NOT_FOUND | 找不到（update / delete） |
| INTERNAL | 後端例外 |

**後端檢查順序**：

| # | 檢查 |
|---|---|
| 1 | 驗證登入 + 權限 |
| 2 | 驗證 body 格式 |
| 3 | 驗證必填欄位 |
| 4 | 查重複 code（create） |
| 5 | 查重複 name |
| 6 | 寫入 Firebase |

**檔案**：functions/api/_entity.js（v1.0.2）

---

### POST /api/admin-migrate

（同前，未變）

---

### Functions 檔案結構

| 檔案 | Handler 數 | 狀態 |
|---|---|---|
| [[path]].js | Catch-all 路由器 | ✅ v103.0.45 |
| _config.js | Firebase REST 設定 | ✅ |
| _helpers.js | API 共用輔助 | ✅ |
| _admin.js | 3 | ✅ |
| _bank.js | 3 | ✅ |
| _family.js | 3 | ✅ |
| _insurance.js | 1 | ✅ |
| _personal.js | 2 | ✅ |
| _platform.js | 2 | ✅ |
| _summary.js | 3 | ✅ |
| **_entity.js** | **1（entity-create）** | **✅ B34-4** |

---

## 三、錯誤回應格式

| 欄位 | 說明 |
|---|---|
| ok | false |
| error | ERROR_CODE |
| message | 錯誤訊息 |

### ERROR_CODE 對照

| Code | HTTP | 說明 |
|---|---|---|
| UNAUTHORIZED | 401 | 未登入 |
| FORBIDDEN | 403 | 無權限 |
| MISSING_FIELDS | 400 | 缺少欄位 |
| INVALID_BODY | 400 | 無效 body |
| BAD_REQUEST | 400 | 請求錯誤 |
| NOT_FOUND | 404 | 找不到 |
| CONFLICT | 409 | 衝突 |
| CODE_DUPLICATE | 409 | 編號重複 |
| NAME_DUPLICATE | 409 | 名稱重複 |
| INTERNAL | 500 | 內部錯誤 |

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

**皆已交付並通過測試。**

---

## 六、關聯檔案

| 主題 | 檔案 |
|---|---|
| 資料結構 | P-09-data.md |
| 專案索引 | P-01-index.md |
| 議題結論 | P-12-conclusion.md |
| CF Functions 規則 | W-02-functions.md |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: docs/P-08-api.md
V: 1791904360
B(歷史): B34-4
W: A20261013-004#180
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-08-api.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-09-data.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-09-data.md

# P-09 Firebase RTDB 資料結構

最後更新：B34-3
Version: 1791899140
Batch: B34-3
Writer: A20261013-004#060
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
| 決策 | P-04-decisions.md |

---

/* ═══ EOF ═══
F: docs/P-09-data.md
V: 1791899140
B(歷史): B34-3
W: A20261013-004#060
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-09-data.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-10-incident.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-10-incident.md

# P-10 事故日誌

最後更新：B34-3
Version: 1791899200
Batch: B34-3
Writer: A20261013-004#061
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
| 學習日誌 | C-05-learning.md |
| 通用規則 | C-02-rules.md |
| 決策 | P-04-decisions.md |
| CF Pages 規則 | W-02-functions.md |
| PWA 規則 | W-04-pwa.md |

---

/* ═══ EOF ═══
F: docs/P-10-incident.md
V: 1791899200
B(歷史): B34-3
W: A20261013-004#061
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-10-incident.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-12-conclusion.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-12-conclusion.md

# P-12 本專案議題結論

最後更新：B34-3
Version: 1791899320
Batch: B34-3
Writer: A20261013-004#064
用途：本專案所有議題的結論（滾動式累積）

---

## 零、設計目標

### 0.1 一句話目標

> 導入通用編號系統，讓「編號 = 身份」，用戶零負擔，AI 一看就懂，徹底消除相容層與重構。

### 0.2 條列目標

| # | 目標 |
|---|---|
| 1 | 用戶操作極簡（只輸名稱，編號自動） |
| 2 | 編號永久不變，改名不斷鏈 |
| 3 | AI 看編號即懂結構（不需查表） |
| 4 | 適用於本專案與未來新專案 |
| 5 | 消除相容層、重組、重構 |

---

## 一、討論議題清單

### 1.1 核心議題

| # | 議題 | 狀態 | 位置 |
|---|---|---|---|
| 1 | 編號規則 | ✅ | §二.1 |
| 2 | 編號生成與儲存 | ✅ | §二.2 |
| 3 | 編號查詢與反查 | ✅ | §二.3 |
| 4 | 舊資料遷移 | ✅ | §二.4 |
| 5 | 統一入口設計 | ✅ | §二.5 |
| 6 | 編號對照清單 | ✅ | §二.6 |
| 7 | 四層架構 + 啟動包 + 自動識別 | ✅ | §二.7 |
| 8 | 滾動式結論納入 C-* | ✅ | §二.8 |
| 9 | 議題表驅動制 | ✅ | §二.9 |

### 1.2 衍生議題

| # | 議題 | 狀態 | 對應決策 |
|---|---|---|---|
| 10 | C-08 刪除 | ✅ | D-017 |
| 11 | Token 顯示格式統一 | ✅ | D-022 |
| 12 | 提問規則（D 方案） | ✅ | D-020 |
| 13 | 編號區分機制 | ✅ | D-021 |
| 14 | 文件 AI 讀者優先 | ✅ | D-023 |
| 15 | Token 計算以 API 為準 | ✅ | — |
| 16 | 人話解釋規則 | ✅ | D-028 |
| 17 | 議題結論摘要機制 | ✅ | D-030 |

---

## 二、設計結論

### 2.1 議題 1：編號規則

#### 核心原則

| # | 原則 |
|---|---|
| 1 | 編號完全自動生成，用戶不選擇、不輸入 |
| 2 | 編號在 UI 上完全隱形 |
| 3 | 編號一旦生成，永久不變 |
| 4 | 名稱可改，編號不動 |

#### 編號格式

> {大類}-{子類}-{序號3位}
> 無子類時：{大類}-{序號3位}

#### 大類前綴

| 大類 | 前綴 |
|---|---|
| 成員 | MEM |
| 支付方法 | PAY |
| 銀行帳號 | PAY-BANK-{碼}-{類型} |
| 保單 | POL |
| 保險基金 | FUND-INS |
| 獨立基金 | FUND |
| 支出類別 | CAT |
| 支出項目 | ITEM |
| 保險公司 | COMP |
| 收入 | INC |
| 銀行交易 | TXN |

#### 子類短碼

| 子類 | 短碼 | 範例 |
|---|---|---|
| 保單-基金投資型 | FUND | POL-FUND-001 |
| 保單-普通保險 | NORM | POL-NORM-001 |
| 保單-基金保險 | FUNIN | POL-FUNIN-001 |
| 保單-儲蓄型 | SAVE | POL-SAVE-001 |
| 支出-學校類 | SCH | CAT-SCH-001 |
| 支出-交通類 | TRA | CAT-TRA-001 |
| 支出-醫療類 | MED | CAT-MED-001 |
| 支出-銀行類 | BNK | CAT-BNK-001 |
| 支出-其他 | OTH | CAT-OTH-001 |

#### 支付方法樹狀結構

> 支付方法（PAY）
> ├─ 銀行類（PAY-BANK）
> │   ├─ 中銀
> │   │   ├─ 家庭共用帳號（PAY-BANK-BOC-FAM-001）
> │   │   └─ 個人帳號（PAY-BANK-BOC-PER-001）→ 所屬：老公
> │   └─ 匯豐
> │       └─ 家庭共用帳號（PAY-BANK-HSB-FAM-001）
> ├─ 現金（PAY-CASH-001）
> └─ PayMe（PAY-PAYME-001）

#### 用戶操作流程

> 所有新增實體：
> 欄位：[已選值 ▼] [+]
>        ↓
>     點 + 號 → 輸入名稱 → 自動生成編號 → 自動選中

#### 保單與關聯基金（同介面）

| 項目 | 說明 |
|---|---|
| 保險基金 | 只能在保單編輯頁新增 |
| 獨立基金 | 獨立新增 |
| 修改關聯基金 | 進保單編輯頁 |

#### 新增情境清單

| # | 情境 | 編號 | + 號 |
|---|---|---|---|
| 1 | 成員 | MEM | ✅ |
| 2 | 銀行 | PAY-BANK | ✅ |
| 3 | 銀行帳號 | PAY-BANK-{碼}-{類型} | ✅ |
| 4 | 支出類別 | CAT | ✅ |
| 5 | 支出項目 | ITEM（自動帶入類別） | ✅ |
| 6 | 支付方式 | PAY | ✅ |
| 7 | 保險公司 | COMP | ✅ |
| 8 | 保單 | POL（含關聯基金） | ✅ |
| 9 | 獨立基金 | FUND | ✅ |
| 10 | 下拉選項 | 欄位旁 + 號 | ✅ |

#### 統一規則

| # | 規則 |
|---|---|
| 1 | 所有可新增欄位都有 + 號 |
| 2 | 新增只輸入名稱 |
| 3 | 自動生成編號（用戶看不到） |
| 4 | 新增後自動選中 |
| 5 | 從屬關係自動帶入上層 |
| 6 | 從屬下拉只顯示上層項目 |
| 7 | 名稱不可重複 |

---

### 2.2 議題 2：編號生成與儲存

#### 核心設計：無計數器，用推導

| 項目 | 做法 |
|---|---|
| 編號儲存 | 實體自己的節點（funds/{id}/code） |
| 計數器節點 | 不需要 |
| transaction | 不需要 |
| 生成方式 | 查現有實體最大序號 + 1 |

#### 生成演算法

> generateCode(prefix, 現有實體列表)：
> 1. 過濾出所有 code 以 {prefix}- 開頭的實體
> 2. 提取序號部分（001, 002, ...）
> 3. 取最大值 N
> 4. 回傳 {prefix}-{N+1 補零到3位}
> 5. 檢查是否重複 → 若重複，提示用戶重試

#### 序號範圍

| 項目 | 做法 |
|---|---|
| 範圍 | 每家庭獨立（各家庭從 001 開始） |
| 位置 | families/{familyId}/funds/ 等節點 |
| 刪除後 | 不重用（跳號可接受） |

#### 子類短碼決定

| 情境 | 短碼來源 |
|---|---|
| 保單-基金投資型 | 依 type='fund_insurance' 自動帶 FUND |
| 支出-醫療類 | 依 category.shortCode 決定 |
| 保險基金 | 依上下文自動帶 FUND-INS |

> 用戶永遠不用選。

#### 併發衝突處理

| 情境 | 風險 | 對策 |
|---|---|---|
| 兩個裝置同時新增 | 拿到同一號 | 重複檢測 → 提示用戶重試 |
| 網路斷線重試 | 號碼跳號 | 可接受 |
| 刪除後再新增 | 是否重用舊號 | 不重用 |

> 核心決策：家庭系統併發機率極低，不引入 transaction 複雜度，用「重複檢測」補救。

#### 為什麼不用計數器？

| 對比項 | 計數器方案 | 推導方案 |
|---|---|---|
| Firebase 節點 | 需額外 counters/ | 不需 |
| 生成邏輯 | 需 transaction | 純查詢 |
| AI 理解 | 需懂計數器 + transaction | 只需懂「查最大 + 1」 |
| 崩潰風險 | 計數器損壞 → 全站編號失效 | 無額外依賴 |
| 維護成本 | 需同步 | 零 |

---

### 2.3 議題 3：編號查詢與反查

#### 從編號查實體

| 項目 | 做法 |
|---|---|
| 決策 | 遍歷實體列表，比對 code |
| 原因 | 對齊議題 2「無索引」；實體數量少，成本低 |
| API | getEntityByCode(familyId, entityPath, code) |

#### 從名稱查編號

| 項目 | 做法 |
|---|---|
| 決策 | 不做（日常不需要） |
| 例外 | 舊資料遷移時才需要 |

#### 從編號展開關聯

| 項目 | 做法 |
|---|---|
| 決策 | 即時遍歷所有實體，找出引用此編號的 |
| API | getRelatedEntities(familyId, code) |

#### 實際 API 清單（id-resolver.js）

| API | 用途 |
|---|---|
| getEntityByCode(familyId, entityPath, code) | 從編號查實體 |
| getRelatedEntities(familyId, code) | 從編號展開所有關聯 |

#### 編號對照清單頁

| 項目 | 內容 |
|---|---|
| 頁面名稱 | 編號對照清單 |
| 功能 | 列出所有編號、編輯、刪除 |
| 不做新增 | 新增走欄位旁 + 號 |

---

### 2.4 議題 4：舊資料遷移

#### 遷移範圍

全部遷移（保單 / 基金 / 成員 / 支付方式 / 類別 / 項目 / 支出紀錄 / 銀行交易 / 保險公司 / 銀行帳號）。

#### 舊 ID 處理

| 項目 | 決策 |
|---|---|
| 是否保留 legacyId | 不保留 |
| 原因 | 舊 ID 是隨機 push key，不可讀 |

#### 遷移工具形態

| 項目 | 決策 |
|---|---|
| 形態 | C+：AI 生成 JSON + 系統內建匯入按鈕 |

#### 遷移流程

| 步驟 | 誰 | 動作 |
|---|---|---|
| 1 | 你 | 從 Firebase Console 匯出 JSON |
| 2 | 你 | 上傳給 AI |
| 3 | AI | 分析舊結構，推導保單欄位 |
| 4 | AI | 產出新格式 JSON |
| 5 | 你 | 貼回系統匯入 |
| 6 | 你 | 其他支出重新輸入 |

#### AI 需推導的保單欄位

| 欄位 | 推導方式 |
|---|---|
| 已供金額 | 由 insurance_payments 統計 |
| 已供期數 | 由 insurance_payments 的月份數量 |
| 月供金額 | 直接讀 monthlyPremium |
| 年費 | 直接讀 annualPremium |
| 開始年度 | 直接讀 firstStartYear / firstStartMonth |
| 剩餘期數 | 總期數 − 已供期數 |

#### 新格式包含

| 項目 | 內容 |
|---|---|
| 每個實體的 code | 例如 FUND-001、POL-NORM-001 |
| 保單完整推導結果 | 已供金額 / 期數 / 剩餘期數 |
| 基金快照 | 從舊 snapshots 轉為 {dateKey} 格式 |
| 舊代碼映射 | 一次性記錄 |

---

### 2.5 議題 5：統一入口設計

#### 三個層面

| 層面 | 說明 | 結論 |
|---|---|---|
| A 使用者操作層 | 所有新增走「+ 號 → 只輸名稱」 | ✅ 議題 1 已含 |
| B 元件層 | 共用 entity-modal + code-registry | ✅ 本議題結論 |
| C 資料層 | 統一 API 端點 | ✅ 本議題結論 |

#### B 層：元件統一

| 元件 | 用途 |
|---|---|
| code-registry.js | 編號生成（唯一 SSOT） |
| entity-modal.js | 複雜實體新增 / 編輯 |
| entity-definitions.js | 8 實體定義（含 code 欄位） |

**新增元件規則**：

| # | 規則 |
|---|---|
| 1 | 簡單實體（成員 / 類別 / 公司 / 支付方式）→ prompt（只輸名稱） |
| 2 | 複雜實體（保單 / 基金）→ entity-modal |
| 3 | 樹狀實體（銀行帳號）→ entity-modal 帶 parent 參數 |
| 4 | 編號只能由 code-registry.js 生成 |
| 5 | modal 回傳新 code，呼叫端 setValue |
| 6 | 編輯 / 刪除入口：各頁 inline + 編號對照清單 |
| 7 | 保單關聯基金：同一 entity-modal，帶參數 |
| 8 | 銀行帳號：同一 entity-modal，帶 parent |
| 9 | 所有實體都有 code 欄位 |
| 10 | 下拉選項不給 code，只給名稱 |

#### C 層：統一 API 端點

**端點**：POST /api/entity-create

**請求格式**：

| 欄位 | 類型 | 說明 |
|---|---|---|
| entityType | string | 見下表 |
| code | string | 前端生成的編號 |
| name | string | 名稱 |
| extra | object | 依類型不同 |

**entityType 清單與 extra 欄位**：

| entityType | extra 必填 | extra 可選 |
|---|---|---|
| member | — | role, order |
| bank | — | order |
| category | shortCode | order |
| item | categoryId | — |
| payment | — | order |
| company | — | — |
| policy | type, memberId, firstStartYear, firstStartMonth, totalPolicyYears | fundsAllocation, monthlyPremium, annualPremium |
| fund | type | policyId, cost, initialYear, initialMonth |
| bankAccount | bankId, type, ownerType | ownerId, initialBalance, initialYear, initialMonth |

**成功回應**：{ ok: true, id, code }

**失敗回應**：

| error | 觸發 |
|---|---|
| CODE_DUPLICATE | 編號已存在 |
| NAME_DUPLICATE | 名稱重複 |
| MISSING_FIELDS | 必填欄位缺少 |
| INVALID_BODY | JSON 格式錯誤 |
| UNAUTHORIZED | 未登入 |
| FORBIDDEN | 無權限 |
| INTERNAL | 後端例外 |

**後端檢查順序**：

| # | 檢查 |
|---|---|
| 1 | 驗證登入 + 權限 |
| 2 | 驗證 body 格式 |
| 3 | 驗證必填欄位 |
| 4 | 查重複 code |
| 5 | 查重複 name |
| 6 | 寫入 Firebase |

#### 舊端點處理

| 項目 | 內容 |
|---|---|
| 處理 | 舊端點全部廢除，只留 entity-create |
| 保留 | GET 類查詢 + 特殊邏輯（insurance-sync / admin-migrate / lookup-family） |
| 舊資料 | 匯出 → AI 改格式 → 重新匯入 |
| 時間點 | 程式碼交付批次執行 |

#### 編號生成位置

| 項目 | 決策 |
|---|---|
| 位置 | 前端（呼叫 code-registry） |
| 後端 | 只存，只驗證重複 |
| 雙重檢查 | 前端 + 後端都檢查重複 |

---

### 2.6 議題 6：編號對照清單

#### 頁面基本資訊

| 項目 | 內容 |
|---|---|
| 檔名 | database.html → code-list.html（舊檔刪除） |
| 頁面名稱 | 編號對照清單 |
| 功能範圍 | 只做編輯 / 刪除，不做新增 |

#### 頁面結構（單頁 + 篩選器）

| 區塊 | 內容 |
|---|---|
| 頂部篩選器 | 類型 + 編號狀態（全部 / 有編號 / 無編號） |
| 主列表區 | 依 code 排序；每列：code + 名稱 + 編輯 + 刪除 |
| 權限按鈕 | 右上角「權限設定」（限 superadmin 可見） |
| 空狀態 | 無資料時顯示 |

#### 顯示規則

| 項目 | 決策 |
|---|---|
| 顯示範圍 | 分兩區：有編號 / 無編號 |
| code 欄位 | 顯示 |
| 排序 | 依 code |
| 編輯範圍 | 全部欄位能改 |
| 刪除 | 直接刪 |
| 權限 | superadmin 設定 |
| 與 + 號邊界 | 純管理 |

#### 連帶影響

| 項目 | 處置 |
|---|---|
| database.html | 刪除，改名 code-list.html |
| database.js / database-options.js / database-dropdowns.js | 全部廢棄 |
| database-yearrange.js | 廢棄；年份範圍搬到 settings |
| settings 頁 | 新增「年份範圍」區塊 |
| code-list.js | 新檔（待交付） |
| sw.js / constants.js / sidebar.js | 更新 |

---

### 2.7 議題 7：四層架構 + 啟動包 + 自動識別

#### 四層架構

| 層 | 前綴 | 職責 |
|---|---|---|
| L1 | C-* | AI 協作通用 |
| L2 | W-* | Web App 通用 |
| L3 | P-* | 本專案專屬 |
| L4 | R-* | 機械記錄 |

#### W-* 抽取（先 5 個）

| 新檔 | 來源 |
|---|---|
| W-01-firebase.md | P-09 |
| W-02-functions.md | C-02 §十二 + P-10 |
| W-03-esmodule.md | P-01 |
| W-04-pwa.md | P-10 §十 |
| W-05-schema.md | P-01 §八 |

**抽取規則**：C-02 保留引用（D-103）。

#### C-09 / C-10 機制與內容分離

| 檔案 | 職責 |
|---|---|
| C-09-rolling-mechanism.md | 滾動結論機制（通用） |
| C-10-handoff-mechanism.md | 交接快照機制（通用） |
| P-12-conclusion.md | 本專案結論（內容） |
| P-13-handoff.md | 本專案交接（內容） |

#### 啟動包三種

**啟動包 1：本專案續用**

> C-00 + R-01 + P-12 + P-13

**啟動包 2：本專案新對話**

> C-00 + R-01 + P-12

**啟動包 3：全新專案**

> C-00~C-07 + W-01~W-05

**寫入位置**：C-00 §十 + C-03 §十二。

#### R-01 專案索引

| 章節 | 內容 |
|---|---|
| §一 | 編號規則 |
| §二 | 生成流程 |
| §三 | 專案清單（新增） |
| §四 | 編寫者清單 |
| §五 | 檔案歸屬 |
| §六 | 歷史歸屬 |
| §七 | 使用方式 |
| §八 | 維護規則 |
| §九 | 關聯檔案 |

**§三 專案清單欄位**：專案名稱 / 起始日期 / 編寫者 / 對話列表。

**對話編號格式**：A{YYYYMMDD}-{序號}（不變）。

#### AI 自動識別專案

| 步驟 | 動作 |
|---|---|
| 1 | AI 讀 P-01-index.md 判斷當前專案 |
| 2 | 若 P-01 不存在 → 主動問「是否新增專案名稱？」 |
| 3 | 新專案 → 寫入 R-01 §三 |

**C-00 是否記專案名**：否（依 P-01 判斷）。

---

### 2.8 議題 8：滾動式結論納入 C-*

| 檔案 | 章節 |
|---|---|
| C-02-rules.md | §十四之一 |
| C-03-guide.md | §七之一 |
| C-04-delivery.md | §十四 |
| C-00-startup.md | §二（必讀 P-12） |

---

### 2.9 議題 9：議題表驅動制

#### 核心

AI 每次對話開場時，主動提示下一議題。

#### 規則

| 項目 | 規則 |
|---|---|
| 觸發 | 對話開場（讀 P-12 §一 後） |
| 動作 | 提示「建議下一議題：{依議題表}」 |
| 議題表位置 | P-12 §一 |
| 議題表結構 | 編號 / 議題 / 狀態 |
| 允許跳題 | 是，但 AI 需警告依賴 |
| 狀態更新 | 所有議題結案後一次更新 |

#### 寫入位置

| 檔案 | 章節 |
|---|---|
| C-02-rules.md | §零點九 |
| C-03-guide.md | §七之三 |

---

## 三、關聯檔案

| 主題 | 檔案 |
|---|---|
| 決策記錄 | P-14-decisions.md |
| 交接快照 | P-13-handoff.md |
| 滾動結論機制 | C-09-rolling-mechanism.md |
| 通用規則 | C-02-rules.md |
| 流程指南 | C-03-guide.md |

---

/* ═══ EOF ═══
F: docs/P-12-conclusion.md
V: 1791899320
B(歷史): B34-3
W: A20261013-004#064
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-12-conclusion.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-13-handoff.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-13-handoff.md

# P-13 本專案交接快照

最後更新：B34-4
Version: 1791903640
Batch: B34-4
Writer: A20261013-004#168
用途：本專案最新交接快照（≤ 80 行）

---

## 一、當前會話摘要

> A20261013-004（B34-1 ~ B34-4）完成 docs 四層架構重構 + 編號系統程式碼交付 + 5 組測試全通過 + 通用大腦架構設計（P-15 計畫書）。

---

## 二、議題進度

> 全部結案（詳見 P-12 §一）。

| 議題 | 狀態 |
|---|---|
| 1~9 | ✅ |
| 衍生 10~17 | ✅ |

---

## 三、關鍵決策

> 詳見 P-14（D-001 ~ D-138）。

**B34-4 重點**：

| 範圍 | 決策 |
|---|---|
| 交付 | D-059 ~ D-064 |
| 學習日誌 | D-097 ~ D-098 / D-113 ~ D-114 |
| 規則新增 | D-099 ~ D-102 |
| 測試修正 | D-109 / D-111 / D-113 |
| 重組架構 | D-118 ~ D-138 |

---

## 四、重建條件（下一 AI 優先確認清單）

| # | 確認項目 |
|---|---|
| 1 | 程式碼是否已完全上線（B34-4 共 20 檔） |
| 2 | 測試是否已完成（5 組全通過） |
| 3 | 重組計畫 P-15 是否已執行（setup-structure.yml 是否已上傳並執行） |
| 4 | 新版 C-00 / C-02 / C-03 / R-01 是否已上傳 |
| 5 | 下一個開發任務是什麼（無議題待討論） |

---

## 五、未完成事項

| 項目 | 下一步 |
|---|---|
| 重組計畫執行 | 上傳 `setup-structure.yml` → 執行 Actions |
| END OF FILE 補齊 | 待處理 |
| 版本號統一（v103.0.xx） | 待處理 |
| entity-registry 加 code 支援 | 待驗證 |
| 新功能 / 重構 | 待規劃 |

---

## 六、下一步建議

1. 開新對話，貼 `AI-training-files` repo 的 STARTUP URL（若已建立）
2. 優先：執行 P-15 重組計畫（上傳 `setup-structure.yml`）
3. 次之：END OF FILE 補齊 / 版本號統一
4. 最後：entity-registry code 支援驗證

---

## 七、待寫入 C-* 清單

| 決策 | 寫入位置 | 狀態 |
|---|---|---|
| D-001 ~ D-138 | C-02 / C-04 / C-05 / P-02 / P-03 / P-13 / R-01 | ✅ 完成 |
| D-136 ~ D-138 | C-00 / C-03 / P-15 | ✅ 完成 |

---

## 八、關聯檔案

| 主題 | 檔案 |
|---|---|
| 議題結論 | P-12-conclusion.md |
| 決策記錄 | P-14-decisions.md |
| 通用規則 | C-02-rules.md |
| 交付格式 | C-04-delivery.md |
| AI 名單 + Repo 清單 | R-01-writers.md |
| 重組計畫 | P-15-reorg-plan.md |

---

/* ═══ EOF ═══
F: docs/P-13-handoff.md
V: 1791903640
B(歷史): B34-4
W: A20261013-004#168
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-13-handoff.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-14-decisions.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-14-decisions.md

# P-14 本專案決策記錄

最後更新：B34-4
Version: 1791904000
Batch: B34-4
Writer: A20261013-004#174
用途：本專案所有決策（D-001 ~ D-143）

---

## 一、決策索引

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

## 二、B33-3 階段（D-001 ~ D-016）

（原內容保留，略）

---

## 三、B34-1 階段（D-017 ~ D-023）

（原內容保留，略）

---

## 四、B34-2 階段（D-024 ~ D-037）

（原內容保留，略）

---

## 五、B34-3 階段（D-038 ~ D-058）

（原內容保留，略）

---

## 六、B34-4 階段（D-059 ~ D-096）

### D-059：B34-4 交付範圍

| 項目 | 內容 |
|---|---|
| 決策 | 5 新增 + 9 修改 + 5 廢棄 + 1 搬遷 |
| 來源 | Q121~Q130（B34-4） |

### D-060：保單子類短碼 3 種

| 項目 | 內容 |
|---|---|
| 決策 | NORM / FUND / SAVE |

### D-061：舊資料 fallback 顯示名稱

| 項目 | 內容 |
|---|---|
| 決策 | 無 code → 顯示名稱，不自動補 |

### D-062：輕量輸入框內嵌 modal.js

| 項目 | 內容 |
|---|---|
| 決策 | openPrompt 內嵌 `ui/modal.js`（不獨立成檔） |

### D-063：entity-create 單一端點

| 項目 | 內容 |
|---|---|
| 決策 | 單一端點 + action 參數（create / update / delete） |

### D-064：舊端點全廢

| 項目 | 內容 |
|---|---|
| 決策 | 廢棄全部 CRUD 端點，只留 GET + 特殊邏輯 |

### D-065 ~ D-076：程式碼交付進度

（原內容保留，略）

### D-077：entity-registry code 支援待確認

| 項目 | 內容 |
|---|---|
| 決策 | 待驗證（B34-4 未處理） |

### D-078：`_entity.js` 改用 REST 函式

| 項目 | 內容 |
|---|---|
| 決策 | 修正 `db.ref(...)` 誤用，改用 `dbGet / dbPush / dbPatch / dbDelete` |

### D-079 ~ D-084：entity-modal / entity-helpers / modal 等

（原內容保留，略）

### D-085 ~ D-096：程式碼交付陸續

（原內容保留，略）

---

## 七、B34-4 收尾（D-097 ~ D-117）

### D-097：L-018 寫入 C-05

| 項目 | 內容 |
|---|---|
| 決策 | 交付未標 base version（教訓記錄） |

### D-098：L-019 寫入 C-05

| 項目 | 內容 |
|---|---|
| 決策 | 未先看檔案就重寫（教訓記錄） |

### D-099：C-02 §零點五 加「以平台顯示為準」

| 項目 | 內容 |
|---|---|
| 決策 | Token 估算以平台為準 |

### D-100：C-02 §四之一 新增「先看檔案再交付」

| 項目 | 內容 |
|---|---|
| 決策 | 修改檔交付前必先讀當前版本 |

### D-101：C-04 加 §一點五「base version 標示」

| 項目 | 內容 |
|---|---|
| 決策 | 修改檔必標「基於 #NNN 的 vX.Y.Z」 |

### D-102：Token 估算 < 20% 必須重估

| 項目 | 內容 |
|---|---|
| 決策 | 達門檻 → 不得直接報告，必須重估 |

### D-103 ~ D-105：C-04 / C-02 / P-02 等修正

（原內容保留，略）

### D-106：需刪檔案清單

| 項目 | 內容 |
|---|---|
| 決策 | 5 程式碼 + 4 docs（後使用者手動完成） |

### D-107：廢棄檔案已刪除

| 項目 | 內容 |
|---|---|
| 決策 | 使用者手動刪除完成 |

### D-108：網站已部署

| 項目 | 內容 |
|---|---|
| 決策 | 部署成功，正常運作 |

### D-109：yearrange-card.js 補上傳

| 項目 | 內容 |
|---|---|
| 決策 | 誤刪 → 補上傳修正 |

### D-110：系統設定恢復正常

| 項目 | 內容 |
|---|---|
| 決策 | yearrange-card.js 補上傳後恢復 |

### D-111：code-list.js v1.0.1

| 項目 | 內容 |
|---|---|
| 決策 | 篩選器改 2 欄並排 |

### D-112：下拉清單與資料遷移無關

| 項目 | 內容 |
|---|---|
| 決策 | 下拉讀現有資料，遷移補 code 欄位 |

### D-113：entity-helpers.js 補 import

| 項目 | 內容 |
|---|---|
| 決策 | 補回 getMembersOnce / getBanksOnce |

### D-114：L-019 教訓再次觸發

| 項目 | 內容 |
|---|---|
| 決策 | 強化「先看檔案」規則 |

### D-115：B34-4 程式碼整體驗證通過

| 項目 | 內容 |
|---|---|
| 決策 | 4 組全過（後 5 組全過） |

### D-116：L-021 寫入 C-05

| 項目 | 內容 |
|---|---|
| 決策 | Token 反覆過度保守（教訓記錄） |

### D-117：P-02 加測試通過記錄

| 項目 | 內容 |
|---|---|
| 決策 | B34-4 5 組測試全通過 |

---

## 八、B34-4 重組設計（D-118 ~ D-143）

### D-118：通用大腦 + N 領域技能包架構

| 項目 | 內容 |
|---|---|
| 決策 | C-* 通用 + W/V/D/M 領域技能包 |
| 來源 | 使用者提議 |

### D-119：「零上傳」在 DeepSeek 平台不可行

| 項目 | 內容 |
|---|---|
| 決策 | 最優為「貼 1~2 條 URL」 |

### D-120：舊專案需各自設定 STARTUP.md

| 項目 | 內容 |
|---|---|
| 決策 | 每個專案需各自 Actions |

### D-121：AI 自動建立新領域 6 檔

| 項目 | 內容 |
|---|---|
| 決策 | 使用者選「新專案」→ AI 問領域 → 新領域則產 6 檔 |

### D-122：架構定案

| 項目 | 內容 |
|---|---|
| 決策 | AI-training-files（共用）+ v2.Family（程式碼）；不另建 Repo B |
| 備註 | 原計畫為 Repo A + B，後簡化為單 repo 子資料夾 |

### D-123：領域結構

| 項目 | 內容 |
|---|---|
| 決策 | docs/common（C）+ docs/skills（W/V/D/M）+ docs/records（R）+ docs/projects（P） |

### D-124：啟動包 3 條 URL（原案）

| 項目 | 內容 |
|---|---|
| 決策 | 後簡化為 2 條 URL |

### D-125：INDEX.md 動態索引 + URL 範本存記事本

| 項目 | 內容 |
|---|---|
| 決策 | INDEX.md 列出所有專案 → 使用者選 → 貼 2 條 |

### D-126：P-15 重組計畫書產出

| 項目 | 內容 |
|---|---|
| 決策 | 完整計畫（架構 / 目錄 / Actions / 遷移） |

### D-127：P-15 §3.1 / §3.2 違反規則

| 項目 | 內容 |
|---|---|
| 決策 | 目錄樹誤用三反引號 |

### D-128：一次性解決方案

| 項目 | 內容 |
|---|---|
| 決策 | C-02 §6.5.4 加「替換對照表」 |

### D-129：P-15 §3.1 / §3.2 目錄樹改表格

| 項目 | 內容 |
|---|---|
| 決策 | 修正完成 |

### D-130：C-02 加 §6.5.4 + §零點十

| 項目 | 內容 |
|---|---|
| 決策 | 替換對照表 + AI 主動建議領域 |

### D-131：GitHub Actions 自動搬檔

| 項目 | 內容 |
|---|---|
| 決策 | 由 yml 執行 git mv，無需手動 |

### D-132：產出 3 檔

| 項目 | 內容 |
|---|---|
| 決策 | STARTUP-core-webapp / STARTUP-project-family-fin / INDEX |

### D-133：C-02 §十三之一 Repo 用實際名稱

| 項目 | 內容 |
|---|---|
| 決策 | 禁止 Repo A / Repo B 代稱 |

### D-134：R-01 §三之一 Repo 清單

| 項目 | 內容 |
|---|---|
| 決策 | 集中所有 repo 名稱 + URL |

### D-135：C-02 §十三之一 指向 R-01

| 項目 | 內容 |
|---|---|
| 決策 | SSOT：Repo 清單只在 R-01 §三之一 |

### D-136：C-00 §十 啟動機制改「貼 URL」

| 項目 | 內容 |
|---|---|
| 決策 | 不上傳檔案，改貼 URL |

### D-137：C-00 §十一 新專案領域詢問

| 項目 | 內容 |
|---|---|
| 決策 | 使用者選「新專案」→ AI 先問領域 |

### D-138：C-03 加 INDEX 流程

| 項目 | 內容 |
|---|---|
| 決策 | 開場 SOP / 對話流程 / 貼 URL 機制 |

### D-139：P-15 §十 / §十之一 更新

| 項目 | 內容 |
|---|---|
| 決策 | 待決事項改為待執行 + 加實際步驟 |

### D-140：W-00-startup.md 交付

| 項目 | 內容 |
|---|---|
| 決策 | 補齊 W-* 6 檔模板 |

### D-141：P-03 更新

| 項目 | 內容 |
|---|---|
| 決策 | 26 檔 docs + 20 檔程式碼 |

### D-142：P-02 更新

| 項目 | 內容 |
|---|---|
| 決策 | 加 B34-4 重組規劃記錄 |

### D-143：P-01 更新

| 項目 | 內容 |
|---|---|
| 決策 | 加領域 / Repo / docs 目錄結構 |

---

## 九、關聯檔案

| 主題 | 檔案 |
|---|---|
| 議題結論 | P-12-conclusion.md |
| 交接快照 | P-13-handoff.md |
| 重組計畫 | P-15-reorg-plan.md |
| ADR 索引 | P-04-decisions.md |
| 通用規則 | C-02-rules.md |

---

/* ═══ EOF ═══
F: docs/P-14-decisions.md
V: 1791904000
B(歷史): B34-4
W: A20261013-004#174
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-14-decisions.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-15-reorg-plan.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-15-reorg-plan.md

# P-15 AI 訓練體系重組計畫書

最後更新：B34-4
Version: 1791903160
Batch: B34-4
Writer: A20261013-004#156
用途：通用大腦 + N 領域技能包架構重組計畫

---

## 一、背景與目標

### 1.1 現況問題

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

## 二、架構總覽

### 2.1 四層架構

| 層 | 前綴 | 職責 | Repo |
|---|---|---|---|
| L1 | C-* | 通用大腦 | A（共用） |
| L2 | W-* / V-* / D-* / M-* | 領域技能包 | A（共用） |
| L3 | P-* | 專案專屬 | B（每專案） |
| L4 | R-01 | 機械記錄（跨專案） | A（共用） |

### 2.2 領域前綴分配

| 前綴 | 領域 | 狀態 |
|---|---|---|
| C | 通用大腦（非領域） | ✅ 已有 |
| W | Web App | ✅ 已有 |
| V | VBA / 桌面自動化 | 🔒 保留 |
| D | 資料分析 / 腳本 | 🔒 保留 |
| M | 文案 / 內容創作 | 🔒 保留 |
| E | Excel（純公式 / PQ） | 🔒 保留 |
| A | AutoHotkey | 🔒 保留 |
| S | SEO / 社群 | 🔒 保留 |
| N | Notion / 知識管理 | 🔒 保留 |
| X / Y / Z | 未分配 | 🔒 保留 |

### 2.3 啟動包機制

**用戶每次開新對話**：

| 步驟 | 動作 |
|---|---|
| 1 | 貼 INDEX.md URL（可選） |
| 2 | 依 INDEX 指引，貼 2 條 URL（core + project） |

**2 條 URL**：

| # | URL | 內容 |
|---|---|---|
| 1 | STARTUP-core-{領域}.md（Repo A） | C-00 + R-01 + 該領域技能包 |
| 2 | STARTUP-project-{專案}.md（Repo B） | P-12 + P-13 |

---

## 三、目錄結構

### 3.1 Repo A（共用，只有一個）

**名稱**：`AI-training-files`

**目錄表**：

| 層級 | 路徑 | 說明 |
|---|---|---|
| 1 | AI-training-files/ | repo 根 |
| 2 | docs/ | 文件根 |
| 3 | docs/common/ | C-00 ~ C-10 |
| 3 | docs/skills/ | 領域技能包 |
| 4 | docs/skills/W/ | Web App（W-00 ~ W-99） |
| 4 | docs/skills/V/ | VBA（未來） |
| 4 | docs/skills/D/ | 資料分析（未來） |
| 4 | docs/skills/M/ | 文案（未來） |
| 3 | docs/records/ | R-01 |
| 2 | .github/ | GitHub 設定 |
| 3 | .github/workflows/ | Actions |
| 4 | .github/workflows/build-core-startup.yml | 自動合併腳本 |
| 1 | STARTUP-core-webapp.md | 自動產 |
| 1 | STARTUP-core-vba.md | 自動產（未來） |
| 1 | INDEX.md | 自動產 |
| 1 | README.md | 手寫 |

### 3.2 Repo B（每專案一個）

**名稱**：`{專案名}-docs`（例：`family-fin-docs`）

**目錄表**：

| 層級 | 路徑 | 說明 |
|---|---|---|
| 1 | family-fin-docs/ | repo 根 |
| 2 | docs/ | 文件根 |
| 3 | docs/projects/ | 專案根 |
| 4 | docs/projects/family-fin/ | P-01 ~ P-99 |
| 2 | .github/ | GitHub 設定 |
| 3 | .github/workflows/ | Actions |
| 4 | .github/workflows/build-project-startup.yml | 自動合併腳本 |
| 1 | STARTUP-project-family-fin.md | 自動產 |
| 1 | README.md | 手寫 |

---

## 四、各層職責

### 4.1 L1 通用大腦（C-*）

| 檔案 | 職責 | 狀態 |
|---|---|---|
| C-00-startup.md | 啟動卡 | ✅ 已有 |
| C-01-profile.md | 使用者畫像 | ✅ 已有 |
| C-02-rules.md | 通用規則 | ✅ 已有 |
| C-03-guide.md | 流程指南 | ✅ 已有 |
| C-04-delivery.md | 交付格式 | ✅ 已有 |
| C-05-learning.md | 學習日誌 | ✅ 已有 |
| C-06-token-log.md | Token 記錄 | ✅ 已有 |
| C-07-summary.md | 階段總結 | ✅ 已有 |
| C-09-rolling-mechanism.md | 滾動結論機制 | ✅ 已有 |
| C-10-handoff-mechanism.md | 交接快照機制 | ✅ 已有 |

**變更**：路徑從 `docs/` → `docs/common/`

### 4.2 L2 領域技能包（統一 6 檔模板）

每個領域 `{X}-*` 一律 6 檔：

| # | 檔名 | 職責 |
|---|---|---|
| 1 | {X}-00-startup.md | 領域啟動卡（工具鏈 / 慣例） |
| 2 | {X}-01-structure.md | 專案結構 / 命名規則 |
| 3 | {X}-02-rules.md | 領域專屬規則 |
| 4 | {X}-03-sop.md | 常見任務 SOP |
| 5 | {X}-04-templates.md | 常用範本 / 樣板 |
| 6 | {X}-99-errors-map.md | 常見錯誤 |

**W-* 現有 5 檔需補 1 檔**（目前缺 W-00-startup.md）

### 4.3 L3 專案專屬（P-*）

| 檔案 | 職責 |
|---|---|
| P-01 ~ P-11 | 專案索引 / 進度 / 清單 / 決策等 |
| P-12-conclusion.md | 議題結論 |
| P-13-handoff.md | 交接快照 |
| P-14-decisions.md | 決策記錄 |
| P-99-errors-map.md | 錯誤摘要 |

### 4.4 L4 機械記錄（R-01）

| 欄位 | 內容 |
|---|---|
| 編號規則 | — |
| 生成流程 | — |
| 專案清單 | **加「領域」欄** |
| 編寫者清單 | — |
| 檔案歸屬 | — |

---

## 五、啟動包機制（詳細）

### 5.1 INDEX.md（動態索引）

**位置**：Repo A 根目錄

**內容表格**：

| 專案 | 領域 | 核心 STARTUP | 專案 STARTUP |
|---|---|---|---|
| 家庭 webapp | W | STARTUP-core-webapp.md | STARTUP-project-family-fin.md |
| （未來）VBA 會計 | V | STARTUP-core-vba.md | STARTUP-project-vba-accounting.md |

### 5.2 用戶操作流程

**首次或忘記 URL 時**：

| 步驟 | 動作 |
|---|---|
| 1 | 貼 INDEX.md URL |
| 2 | AI 讀完說「請選擇專案」 |
| 3 | 你說「家庭 webapp」 |
| 4 | AI 給 2 條 URL |
| 5 | 你貼 2 條 |

**日常（已記住 URL）**：

| 步驟 | 動作 |
|---|---|
| 1 | 貼 2 條 URL（core + project） |

### 5.3 URL 範本（給你存手機記事本）

| 專案 | 核心 URL | 專案 URL |
|---|---|---|
| 家庭 webapp | raw…/STARTUP-core-webapp.md | raw…/family-fin-docs/main/STARTUP-project-family-fin.md |

---

## 六、GitHub Actions 設計

### 6.1 Repo A：build-core-startup.yml

**觸發**：`docs/common/*` 或 `docs/skills/*` 或 `docs/records/*` 更新

**產出**：

| 檔 | 內容 |
|---|---|
| STARTUP-core-{領域}.md | 每個領域一份（C-00 + R-01 + 該領域 6 檔） |
| INDEX.md | 讀 R-01 §三 專案清單 → 產出索引表 |

### 6.2 Repo B：build-project-startup.yml

**觸發**：`docs/projects/{專案}/*` 更新

**產出**：

| 檔 | 內容 |
|---|---|
| STARTUP-project-{專案}.md | P-12 + P-13 |

---

## 七、遷移計畫

### 7.1 階段 1：Repo A 重組（現有）

| 步驟 | 動作 |
|---|---|
| 1 | 建 docs/common/，搬入 C-* |
| 2 | 建 docs/skills/W/，搬入 W-* |
| 3 | 建 docs/records/，搬入 R-01 |
| 4 | W-* 補 W-00-startup.md |
| 5 | C-00 更新路徑引用 |
| 6 | 加 build-core-startup.yml |

### 7.2 階段 2：Repo B 建立（家庭 webapp）

| 步驟 | 動作 |
|---|---|
| 1 | 新建 repo family-fin-docs |
| 2 | 建 docs/projects/family-fin/，搬入 P-* |
| 3 | 加 build-project-startup.yml |
| 4 | 移除 Repo A 內的 P-* |

### 7.3 階段 3：驗證

| 步驟 | 動作 |
|---|---|
| 1 | 手動觸發 Actions |
| 2 | 確認 STARTUP / INDEX 產出 |
| 3 | 貼 URL 測試新對話 |

---

## 八、未來擴充規則

### 8.1 新增領域（AI 主動建議，依 C-00 §十一）

**觸發**：使用者選「新專案」→ AI 問領域 → 若為新領域 → 主動建議建 6 檔

**AI 動作**：

| 步驟 | 動作 |
|---|---|
| 1 | 依使用者描述產草稿 |
| 2 | 提示「是否需要調整」 |
| 3 | 使用者確認 → 產出 6 檔 |
| 4 | 放 docs/skills/{X}/ |
| 5 | 更新 C-00 §九、R-01 §三 |

### 8.2 新增專案

| 步驟 | 動作 |
|---|---|
| 1 | 開新 Repo B |
| 2 | 複製 P-* 模板 |
| 3 | R-01 §三 加該專案（含領域欄） |
| 4 | 設 GitHub Actions |

---

## 九、對比表：現況 vs 重組後

| 項目 | 現況 | 重組後 |
|---|---|---|
| Repo 數 | 2 | 3 |
| 檔案結構 | docs/ 混放 | docs/common + skills + records |
| 加新領域 | 需重設計 | 照抄 6 檔模板 |
| 加新專案 | 難 | 4 步 |
| 啟動包 | 4 檔上傳 | 2 條 URL |
| 領域檔案數 | 不定 | 統一 6 檔 |
| GitHub Actions | 無 | 2 個 workflow |

---

## 十、待決事項

| # | 項目 | 狀態 |
|---|---|---|
| 1 | C-00 加 §十一「AI 主動建議領域」 | 📋 |
| 2 | C-00 §二 步驟改讀 INDEX.md | 📋 |
| 3 | R-01 §三 加「領域」欄 | 📋 |
| 4 | W-* 補 W-00-startup.md | 📋 |
| 5 | GitHub Actions yml（2 個） | 📋 |
| 6 | URL 範本（給使用者存） | 📋 |
| 7 | C-02 §六點五 加替換對照表（Q251） | 📋 |

---

## 十一、關聯檔案

| 主題 | 檔案 |
|---|---|
| 啟動卡 | C-00-startup.md |
| AI 名單 | R-01-writers.md |
| 議題結論 | P-12-conclusion.md |
| 交接快照 | P-13-handoff.md |
| 決策記錄 | P-14-decisions.md |

---

/* ═══ EOF ═══
F: docs/P-15-reorg-plan.md
V: 1791903160
B(歷史): B34-4
W: A20261013-004#156
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-15-reorg-plan.md === -->

<!-- === FILE START: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-99-errors-map.md === -->

## 📄 external/AI-WEB-APP-Family/docs/projects/web-app-family/P-99-errors-map.md

# P-99 錯誤摘要（交付前必讀）

最後更新：B34-4
Version: 1791904240
Batch: B34-4
Writer: A20261013-004#178
上限：100 行
用途：**交付前 AI 必讀**；防止重複犯錯

---

## 一、15 大類錯誤（禁止）

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
| 9 | Token 自估誤導使用者 | 以平台顯示為準（B34-2） |
| 10 | 議題編號與選項混淆 | 議題數字 / 選項字母（B34-2） |
| 11 | 通用內容寫入 P-* | 依四層架構分類（B34-3） |
| 12 | 修改檔未標 base version | 必標「基於 #NNN 的 vX.Y.Z」（B34-4） |
| 13 | 未讀當前版本就重寫 | 先讀再改（B34-4） |
| 14 | Token < 20% 未重估 | 強制重估（B34-4） |
| 15 | 用 Repo A / B 代稱 | 用實際名稱（B34-4） |

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
| 7 | EOF 已加（F/V/B/W） | |
| 8 | md 內零三反引號 | |
| 9 | 編號 + 序號已標 | |
| 10 | 追蹤表已附 | |
| 11 | Token 狀態已顯示（末尾） | |
| 12 | 選項已附人話解釋 | |
| 13 | **修改檔已標 base version** | |
| 14 | **已先讀當前版本** | |
| 15 | **用實際 Repo 名稱** | |

---

## 三、高頻重演問題

| 類型 | 首次 | 再次 | 防止 |
|---|---|---|---|
| 複製貼上誤覆蓋 | P12 | B01 / P31 | 交付前確認目標 |
| page-engine 修正遺漏 | P07 | B04 | 改 engine 需回歸測試 |
| 舊 import 路徑殘留 | P13 | B02 | 搬移後全域搜尋 |
| 狀態字串硬編碼 | H01 | H8 | 全走 registry |
| 自我引入 bug | P20 | P21 | 交付前全鏈路 |
| md 三反引號 | — | B32-1 / **B34-4** | 零三反引號 + 替換對照表 |
| 編號未標 | — | B32-1 | 強制 9 條 |
| Token 位置不一致 | — | B34-2 | 一律回覆末尾 |
| 議題編號混用 | — | B34-2 | 議題數字 / 選項字母 |
| Token 自估 | — | B34-2 / **B34-4** | 以平台為準 + < 20% 重估 |
| **漏 import** | — | **B34-4（entity-helpers）** | 先讀檔案再改 |
| **誤刪檔案** | — | **B34-4（yearrange-card）** | 交付時標明取代關係 |

---

## 四、詳細記錄

**完整事故**：P-10-incident.md
**學習日誌**：C-05-learning.md
**決策記錄**：P-14-decisions.md

---

## 五、關聯檔案

| 主題 | 檔案 |
|---|---|
| 事故日誌 | P-10-incident.md |
| 學習日誌 | C-05-learning.md |
| 通用規則 | C-02-rules.md |
| 決策記錄 | P-14-decisions.md |

---

/* ═══ EOF ═══
F: docs/P-99-errors-map.md
V: 1791904240
B(歷史): B34-4
W: A20261013-004#178
═══ */
<!-- === FILE END: external/AI-WEB-APP-Family/docs/projects/web-app-family/P-99-errors-map.md === -->
