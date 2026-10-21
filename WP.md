# W 包：Web App 領域技能

> 生成時間：2026-10-21 20:43:58 (HKT)


<!-- === FILE START: AI-kernel/docs/skills/W/W-00-startup.md === -->

## 📄 AI-kernel/docs/skills/W/W-00-startup.md

# W-00 領域啟動卡（Web App）

最後更新：2026-10-21
Version: 1792581001
Writer: ai-system-20261021-004#1
用途：Web App 領域的啟動卡（工具鏈 / 慣例 / 載入方式）

---

## 一、領域定位

| 項目 | 內容 |
|---|---|
| 領域名稱 | Web App |
| 領域前綴 | W |
| 適用範圍 | 純 HTML / CSS / JS 前端 + Serverless 後端 |
| 適用專案 | 所有依循本體系之 Web App 專案 |

---

## 二、技能包清單

| # | 檔案 | 職責 |
|---|---|---|
| 0 | W-00-startup.md（本檔） | 領域啟動卡 |
| 1 | W-01-firebase.md | Firebase RTDB 結構原則 |
| 2 | W-02-functions.md | Cloudflare Pages Functions 規則 |
| 3 | W-03-esmodule.md | 純前端 ES Module 架構 |
| 4 | W-04-pwa.md | PWA + Service Worker |
| 5 | W-05-schema.md | Web App 頁面 Schema 與 UI 規範 |
| 6 | W-06-sop.md | Web App 通用任務 SOP |
| 7 | W-07-errors.md | Web App 通用錯誤與防呆 |

---

## 三、工具鏈

| 層 | 技術 |
|---|---|
| 前端 | 純 HTML + CSS + ES Module JS（無框架） |
| 後端 | Cloudflare Pages Functions |
| 資料庫 | Firebase RTDB |
| 認證 | Firebase Auth |
| 部署 | Cloudflare Pages |
| 版控 | GitHub |

---

## 四、通用慣例

| # | 慣例 |
|---|---|
| 1 | 檔案結構：三層架構（資料 / 引擎 / 呈現） |
| 2 | SSOT 極大化 |
| 3 | 宣告式 > 命令式 |
| 4 | 檔案 ≤ 800 行 |
| 5 | 每個 .js / .html / .css / .md 結尾加 EOF |
| 6 | 無框架，不引入 React / Vue |

---

## 五、領域常見任務

| 任務 | 引用 |
|---|---|
| 新增頁面 | W-03 + W-05 + W-06 |
| 改資料結構 | W-01 |
| 改 Functions | W-02 |
| 改 PWA | W-04 |
| 改 Schema | W-05 |

---

## 六、領域常見錯誤

| # | 錯誤 | 正確 |
|---|---|---|
| 1 | 用 process.env（CF Pages 不支援） | handler 參數 { env } |
| 2 | 移除既有 export（依賴模組崩潰） | 用 export let 保留 |
| 3 | 動態值用快照 | 內部讀 AppState |
| 4 | 動態注入未清空 | 先 textContent = '' |
| 5 | 全站結構改動未完整掃描 | 改前完整掃描 |

---

## 七、未來擴充提示

若需要新增 Web App 技術（如 WebSocket / Web Worker），新增 `W-08-xxx.md` 並更新本清單。

---

## 八、關聯檔案

| 主題 | 檔案 |
|---|---|
| 體系四檔 | `IP.md`、`AP.md`、`RP.md`、`UP.md` |
| Firebase RTDB | W-01-firebase.md |
| CF Pages Functions | W-02-functions.md |
| ES Module | W-03-esmodule.md |
| PWA + SW | W-04-pwa.md |
| Page Schema | W-05-schema.md |
| 通用 SOP | W-06-sop.md |
| 通用錯誤 | W-07-errors.md |

---

/* ═══ EOF ═══
F: W-00-startup.md
V: 1792581001
W: ai-system-20261021-004#1
═══ */
<!-- === FILE END: AI-kernel/docs/skills/W/W-00-startup.md === -->

<!-- === FILE START: AI-kernel/docs/skills/W/W-01-firebase.md === -->

## 📄 AI-kernel/docs/skills/W/W-01-firebase.md

# W-01 Firebase RTDB 結構原則（Web App 通用）

最後更新：2026-10-21
Version: 1792581002
Writer: ai-system-20261021-004#2
用途：Firebase RTDB 通用結構原則（跨專案）

---

## 一、樹狀結構原則

| # | 原則 |
|---|---|
| 1 | 根節點分層（platform / families / 其他） |
| 2 | 家庭為獨立單位（families/{familyId}/） |
| 3 | 子節點按功能分類（members / expenses / ...） |
| 4 | 避免深層嵌套（≤ 6 層） |

---

## 二、路徑命名

| 規則 | 說明 |
|---|---|
| 小寫 + 底線 | expense_categories |
| 複數 | members / banks |
| 無縮寫 | expense_categories（非 exp_cat） |
| ID 用 push key | Firebase 自動生成 |

---

## 三、扁平化

| 概念 | 做法 |
|---|---|
| 1 對多 | 用路徑分隔，不用陣列 |
| 多對多 | 用索引節點 |
| 大資料 | 用子節點（避免單節點過大） |

---

## 四、索引節點

| 索引 | 用途 |
|---|---|
| platform/uid_index/{uid} | uid → familyId |
| platform/email_index/{account} | account → { uid, familyId } |

**原則**：索引只存對應關係，不存實體資料。

---

## 五、實時訂閱

| 模式 | 用途 |
|---|---|
| listen* | 持續訂閱（資料變更自動推送） |
| get*Once | 一次性讀取 |

**選擇原則**：

| 情境 | 用 |
|---|---|
| 頁面需即時更新 | listen |
| 一次性檢查 | once |
| 跨頁共用 | listen + registry 快取 |

---

## 六、讀寫分離

| 原則 | 說明 |
|---|---|
| 讀取 | normalize 舊資料 |
| 寫入 | 寫新格式 |
| 刪除 | 保留節點（軟刪除） |

---

## 七、限制

| 節點 | 建議上限 |
|---|---|
| 單筆列表 | ≤ 1000 筆 |
| 巢狀深度 | ≤ 6 層 |
| 單節點大小 | < 10 MB |

**超過時**：拆子節點或加索引。

---

## 八、關聯檔案

| 主題 | 檔案 |
|---|---|
| 本專案資料結構 | 詳見專案包（如 P-09-data.md） |
| 本專案 API | 詳見專案包（如 P-08-api.md） |

---

/* ═══ EOF ═══
F: W-01-firebase.md
V: 1792581002
W: ai-system-20261021-004#2
═══ */
<!-- === FILE END: AI-kernel/docs/skills/W/W-01-firebase.md === -->

<!-- === FILE START: AI-kernel/docs/skills/W/W-02-functions.md === -->

## 📄 AI-kernel/docs/skills/W/W-02-functions.md

# W-02 Cloudflare Pages Functions 規則（Web App 通用）

最後更新：2026-10-21
Version: 1792581003
Writer: ai-system-20261021-004#3
用途：CF Pages Functions 通用規則（跨專案）

---

## 一、檔案結構

| 規則 | 說明 |
|---|---|
| 底線前綴 | _config.js / _helpers.js / _admin.js |
| Catch-all | [[path]].js 統一分派 |
| 禁止獨立 xxx.js | 用底線前綴 |
| 禁止 shim re-export | CF 不支援跨檔 re-export |

---

## 二、環境變數

| 項目 | 說明 |
|---|---|
| 錯誤 | process.env.DB_TARGET |
| 正確 | handler 參數 { env } → env.DB_TARGET |
| 傳遞 | 每個 onRequest* 開頭呼叫 initConfig(env) |
| 生效條件 | 設定後必須重新部署 |

---

## 三、initConfig 模式

| 步驟 | 動作 |
|---|---|
| 1 | onRequest 開頭呼叫 initConfig(context.env) |
| 2 | _config.js 用 export let 保留舊引用 |
| 3 | 其他模組 import 該 let（live binding） |
| 4 | initConfig 更新 let 值 → 所有引用同步更新 |

**禁止**：直接 export const（無法更新）。

---

## 四、路由規則

| 路徑 | 處理 |
|---|---|
| 靜態檔案 | 所有連結帶 .html |
| API | /api/* 由 [[path]].js 分派 |
| 根路徑 | _redirects 不要對 / 加規則 |

---

## 五、常見錯誤

| # | 錯誤 | 正確 |
|---|---|---|
| 1 | 每個 URL 對應獨立實體檔 | 用 Catch-all + 底線前綴 |
| 2 | 用 shim re-export | 直接用底線檔 |
| 3 | 忘記 initConfig | 每個 onRequest 開頭必呼叫 |
| 4 | 移除既有 export | 用 export let 保留 |
| 5 | 設定後未重新部署 | 必須重部署才生效 |

---

## 六、Response 格式與錯誤碼

| 欄位 | 值 |
|---|---|
| ok | true / false |
| error | ERROR_CODE（失敗時） |
| message | 錯誤訊息（失敗時） |

**通用 ERROR_CODE 對照**：

| Code | HTTP | 說明 |
|---|---|---|
| UNAUTHORIZED | 401 | 未登入 |
| FORBIDDEN | 403 | 無權限 |
| MISSING_FIELDS | 400 | 缺少欄位 |
| INVALID_BODY | 400 | 無效 body |
| BAD_REQUEST | 400 | 請求錯誤 |
| NOT_FOUND | 404 | 找不到 |
| CONFLICT | 409 | 衝突 |
| INTERNAL | 500 | 內部錯誤 |

*(註：各專案可依業務需求額外定義專屬錯誤碼，如 NAME_DUPLICATE)*

---

## 七、關聯檔案

| 主題 | 檔案 |
|---|---|
| 本專案 API 契約 | 詳見專案包（如 P-08-api.md） |
| 本專案事故 | 詳見專案包（如 P-10-incident.md） |
| Firebase 結構 | W-01-firebase.md |

---

/* ═══ EOF ═══
F: W-02-functions.md
V: 1792581003
W: ai-system-20261021-004#3
═══ */
<!-- === FILE END: AI-kernel/docs/skills/W/W-02-functions.md === -->

<!-- === FILE START: AI-kernel/docs/skills/W/W-03-esmodule.md === -->

## 📄 AI-kernel/docs/skills/W/W-03-esmodule.md

# W-03 純前端 ES Module 架構（Web App 通用）

最後更新：2026-10-21
Version: 1792581004
Writer: ai-system-20261021-004#4
用途：純 HTML + ES Module 架構通用原則與目錄配置

---

## 一、三層架構

| 層 | 職責 | 目錄 |
|---|---|---|
| 資料層 | Firebase RTDB | — |
| 引擎層 | page-engine / data-engine / render-engine | js/engines/ |
| 呈現層 | block / UI 元件 / DOM | js/blocks, js/ui |

**資料流**：

> Firebase RTDB → data-engine → page-engine → derived → block → DOM

---

## 二、核心架構原則

| 原則 | 說明 |
|---|---|
| SSOT 極大化 | 任何知識只存在一處 |
| 宣告式 > 命令式 | 頁面只描述「要什麼」 |
| 舊資料 100% 相容 | 讀取時 normalize |
| 操作邏輯不變 | 只改實作方式 |
| 無補丁 | 能宣告就不手寫 |

---

## 三、5 大 Registry

| Registry | 職責 |
|---|---|
| status-registry | 狀態系統 |
| entity-registry | 名稱解析 |
| label-registry | 顯示文字 |
| column-registry | 表格欄位 |
| constants | 全站常數 |

**附加**：page-metadata-registry

---

## 四、3 大引擎

| 引擎 | 職責 |
|---|---|
| page-engine | 解讀 Schema、生命週期、onDataChange |
| data-engine | Firebase 訂閱 + 衍生 |
| render-engine | 掛載 UI 區塊 |

---

## 五、5 大 Block

| Block | 職責 |
|---|---|
| stats-block | 統計卡 |
| list-block | 列表 |
| filter-block | 篩選列 |
| detail-block | 明細展開 |
| form-block | 表單 Modal |

---

## 六、Page Schema（見 W-05）

---

## 七、檔案規則

| 規則 | 說明 |
|---|---|
| 檔案 ≤ 800 行 | 超過拆檔 |
| 每檔獨立 | 不合併 |
| EOF 必加 | F/V/W 三行 |

---

## 八、標準目錄配置（通用）

| 目錄 | 建議檔數 | 內容職責 |
|---|---|---|
| `js/config` | 5~10 | 常數、環境配置、各類 Registry |
| `js/core` | 5~10 | 狀態管理 (state)、API 核心、登入驗證、共用工具 |
| `js/lib` | 10~20 | 通用邏輯（日期處理、格式化、Modal 封裝） |
| `js/engines` | 3 | page / data / render |
| `js/blocks` | 5 | stats / list / filter / detail / form |
| `js/ui` | 5~10 | 獨立 UI 元件（toast, modal, tabs） |
| `js/layout` | 3 | 外框佈局（navbar, sidebar, app-shell） |
| `js/entity` | 3~5 | 實體資料的解析、驗證與 Modal |
| `js/shared` | 5~15 | 跨頁面共用元件（表格、卡片） |
| `js/pages` | 依頁面數 | 各頁面的 Schema 定義 |
| `functions/api` | 依 API 數 | CF Pages Functions 後端邏輯 |
| `css/` | 4~6 | theme / layout / components / utilities |
| 根目錄 | — | `*.html` 頁面、`sw.js`、`manifest.json` |

---

## 九、關聯檔案

| 主題 | 檔案 |
|---|---|
| Page Schema | W-05-schema.md |
| PWA + SW | W-04-pwa.md |
| 本專案架構 | 詳見專案包（如 P-01-index.md） |

---

/* ═══ EOF ═══
F: W-03-esmodule.md
V: 1792581004
W: ai-system-20261021-004#4
═══ */
<!-- === FILE END: AI-kernel/docs/skills/W/W-03-esmodule.md === -->

<!-- === FILE START: AI-kernel/docs/skills/W/W-04-pwa.md === -->

## 📄 AI-kernel/docs/skills/W/W-04-pwa.md

# W-04 PWA + Service Worker 規則（Web App 通用）

最後更新：2026-10-21
Version: 1792581005
Writer: ai-system-20261021-004#5
用途：PWA + SW 通用規則

---

## 一、Service Worker 策略

| 策略 | 用途 |
|---|---|
| network-first | 開發階段（保新鮮） |
| cache-first | 正式階段（保速度） |
| stale-while-revalidate | 折衷 |

**選擇建議**：依專案需求決定，通常開發期使用 network-first，正式期可切換為 cache-first。

---

## 二、靜態資源

| 項目 | 說明 |
|---|---|
| STATIC_ASSETS | 清單集中一處 |
| 版本 | SW_VERSION 統一 |
| 更新 | 改版本號觸發更新 |

---

## 三、版本管理

| 項目 | 規則 |
|---|---|
| SW_VERSION | 全站統一 |
| STATIC_ASSETS | 新增檔案必加入 |
| 部署後 | 使用者訪問 2 次才生效（network-first） |

---

## 四、常見錯誤

| # | 錯誤 | 正確 |
|---|---|---|
| 1 | SW_VERSION 各檔不一致 | 統一 |
| 2 | 新增檔案未加 STATIC_ASSETS | 必加 |
| 3 | 忘記更新版本 | 每次部署更新 |

---

## 五、關聯檔案

| 主題 | 檔案 |
|---|---|
| 本專案 SW 實作 | 詳見專案包（如 sw.js / manifest.json） |
| 事故日誌 | 詳見專案包（如 P-10-incident.md） |

---

/* ═══ EOF ═══
F: W-04-pwa.md
V: 1792581005
W: ai-system-20261021-004#5
═══ */
<!-- === FILE END: AI-kernel/docs/skills/W/W-04-pwa.md === -->

<!-- === FILE START: AI-kernel/docs/skills/W/W-05-schema.md === -->

## 📄 AI-kernel/docs/skills/W/W-05-schema.md

# W-05 Web App 頁面 Schema 與 UI 規範（Web App 通用）

最後更新：2026-10-21
Version: 1792581006
Writer: ai-system-20261021-004#6
用途：Page Schema 通用規範 + UI 結構與術語

---

## 一、Schema 欄位

| 欄位 | 類型 | 說明 |
|---|---|---|
| title | string | 頁面標題 |
| data | object | Firebase 訂閱配置 |
| state | object | 響應式狀態初始值 |
| derived | object | 衍生資料計算 |
| blocks | array | UI 區塊 |
| onYearMonthChange | function | 年月變更 hook（可選） |
| customMount | function | 逃生艙（可選） |

---

## 二、data 配置

| 欄位 | 說明 |
|---|---|
| type | list / object / value / raw |
| path | 相對路徑（含 {key} / {__all__}） |
| transform | 資料轉換（可選） |
| params | 佔位符對應（可選） |

---

## 三、derived 配置

| 欄位 | 說明 |
|---|---|
| deps | 依賴的 data / state key 陣列 |
| compute | (ctx) => value 計算函式 |

---

## 四、blocks 常用類型

| type | 必要欄位 |
|---|---|
| stats | container, cards |
| list | container, rows, columns |
| filter | container, fields, bind |

---

## 五、ctx 提供

| 方法 | 用途 |
|---|---|
| ctx.onDataChange(fn) | 訂閱資料變更 |
| ctx.setState(path, val) | 設 state |
| ctx.invalidate(key) | 觸發重算 |

---

## 六、customMount 回傳

| 欄位 | 說明 |
|---|---|
| destroy | 清理函式 |

---

## 七、HTML 結構

| 元素 | 說明 |
|---|---|
| head | 4 個 CSS |
| body | app-shell > sidebar-root + main-area |
| main-area | navbar-root + content |
| content | page-header + 各 block container |
| scripts | lucide + modulepreload + type="module" |

---

## 八、常見錯誤

| # | 錯誤 | 正確 |
|---|---|---|
| 1 | derived 對 undefined 崩潰 | 加檢查 |
| 2 | customMount 未回傳 destroy | 必回傳 |
| 3 | 動態值用快照 | 內部讀 AppState |
| 4 | 動態注入未清空 | 先 textContent = '' |

---

## 九、UI 結構與術語（通用）

### 9.1 頁面結構層級
| 層級 | 術語 | 說明 | 範例 |
|---|---|---|---|
| L1 | Page（頁面） | 一個 .html | index.html |
| L2 | Section（區塊） | 頁面內垂直堆疊的功能區 | page-header / stats |
| L3 | Row（列） | Section 內水平排列 | toolbar row |
| L4 | Item（元素） | 最小單位 | button / select |

### 9.2 Section 類型
| 術語 | 中文 | 用途 | 標準位置 |
|---|---|---|---|
| page-banner | 提示橫幅 | 說明文字 | 頁面最上 |
| page-header | 頁首 | 標題 + 副標題 + 主要操作 | banner 之後 |
| page-header-actions | 頁首操作區 | 標題行右側 | page-header 內 |
| stats | 統計卡區 | 數字統計 | header 之後 |
| page-controls | 控制列 | 篩選 / 切換 / 年月 | stats 之後 |
| content | 內容區 | 表格 / 卡片 | 最底 |
| empty-state | 空狀態 | 無資料時顯示 | 內容區內 |

### 9.3 控制列元素
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

### 9.4 頁面標準佈局
| 順序 | Section | 內容 |
|---|---|---|
| 1 | page-banner | banner（若有） |
| 2 | page-header | 標題 + 副標題 + 主要操作 |
| 3 | stats | 統計卡 |
| 4 | page-controls | 新增 / 切換 / 年月 / 篩選 |
| 5 | content | 表格 / 卡片 |

### 9.5 表頭互動
- 點欄位標題（首次）：升序。
- 點欄位標題（第二次）：降序。
- 點欄位標題（第三次）：清除排序。
- 點欄位旁的 🔽：開下拉面板進行篩選。

---

## 十、關聯檔案

| 主題 | 檔案 |
|---|---|
| ES Module 架構 | W-03-esmodule.md |
| 本專案頁面實作 | 詳見專案包（如 P-06 / P-07） |

---

/* ═══ EOF ═══
F: W-05-schema.md
V: 1792581006
W: ai-system-20261021-004#6
═══ */
<!-- === FILE END: AI-kernel/docs/skills/W/W-05-schema.md === -->

<!-- === FILE START: AI-kernel/docs/skills/W/W-06-sop.md === -->

## 📄 AI-kernel/docs/skills/W/W-06-sop.md

# W-06 Web App 通用任務 SOP

最後更新：2026-10-21
Version: 1792581007
Writer: ai-system-20261021-004#7
用途：Web App 通用任務的標準作業流程

---

## 一、新增頁面

| 步驟 | 動作 |
|---|---|
| 1 | 讀 W-03 + W-05 |
| 2 | 檢查專案架構是否可複用 |
| 3 | 寫 `js/pages/xxx.js`（Schema） |
| 4 | 寫 `xxx.html` |
| 5 | 更新側邊欄導覽設定（如 constants.js） |
| 6 | 更新 Service Worker 的 STATIC_ASSETS（見 W-04） |
| 7 | 更新專案對應的頁面區塊規格書 |
| 8 | 更新專案檔案清單與進度紀錄 |

---

## 二、部署後驗證

| 步驟 | 動作 |
|---|---|
| 1 | 訪問主要頁面 |
| 2 | 檢查 Console 是否有 Error |
| 3 | 切換連動元件（如年月） → 驗證連動 |
| 4 | 檢查 API 回應是否正常 |
| 5 | 手機 / 跨裝置測試 |

**通用驗證清單**：
- [ ] 登入 / 註冊流程正常
- [ ] 統計卡有數字
- [ ] 分頁 / 篩選器可運作
- [ ] 各 Modal 可正常開啟與關閉
- [ ] 表格與卡片視圖切換正常
- [ ] 空狀態顯示正確

---

## 三、緊急回滾

| 步驟 | 動作 |
|---|---|
| 1 | 至 GitHub 尋找上一個穩定的 commit |
| 2 | 執行 Revert 或強制將分支重置到該 commit |
| 3 | 等待部署平台（如 CF Pages）自動重部署 |
| 4 | 驗證線上版本是否恢復 |
| 5 | 分析事故原因後重做 |

**禁止**：手動修改線上檔案、強制 push 覆蓋歷史（除非極端情況）。

---

## 四、關聯檔案

| 主題 | 檔案 |
|---|---|
| ES Module 架構 | W-03-esmodule.md |
| PWA 與 SW | W-04-pwa.md |
| Page Schema | W-05-schema.md |

---

/* ═══ EOF ═══
F: W-06-sop.md
V: 1792581007
W: ai-system-20261021-004#7
═══ */
<!-- === FILE END: AI-kernel/docs/skills/W/W-06-sop.md === -->

<!-- === FILE START: AI-kernel/docs/skills/W/W-07-errors.md === -->

## 📄 AI-kernel/docs/skills/W/W-07-errors.md

# W-07 Web App 通用錯誤與防呆

最後更新：2026-10-21
Version: 1792581008
Writer: ai-system-20261021-004#8
用途：沉澱 Web App 開發過程中的通用錯誤與防範措施

---

## 一、通用開發錯誤（交付前檢查）

| # | 類型 | 防止做法 |
|---|---|---|
| 1 | 估算前未掃描實際檔數 | 先全域掃描（grep） |
| 2 | 「統一」誤解為各檔都改 | 統一 = 邏輯集中一處 |
| 3 | 未檢查現有 API 複用 | 先找複用機會 |
| 4 | 未思考集中層強制 | 優先改集中層 |
| 5 | 動態值用快照 | 內部讀取狀態（AppState） |
| 6 | 動態注入未清空 | 先執行 `textContent = ''` |
| 7 | 全站結構改動未完整掃描 | 完整掃描後才動 |
| 8 | 自我引入 bug | 交付前全鏈路檢查 |

---

## 二、Cloudflare Pages (CF Pages) 教條

| 教訓 | 說明 |
|---|---|
| Functions 路由 | 每個 URL 應對應獨立實體檔或統一 Catch-all |
| 不支援 shim | 用底線前綴（`_xxx.js`），避免跨檔 re-export |
| 靜態檔案 | 所有連結需帶 `.html` |
| _redirects | 不要對 `/` 加規則，避免無限迴圈 |
| 環境變數 | 走 handler `{ env }`，非 `process.env` |

---

## 三、Page Engine 與 ES Module 教條

| 教訓 | 說明 |
|---|---|
| customMount | 必須支援，供複雜邏輯逃生 |
| onDataChange | 必須支援非同步資料流 |
| 依賴圖 | key 與事件觸發必須一致 |
| undefined | derived 計算需容忍初始 undefined |
| 動態值 | 必須內部讀取最新狀態，不可快照 |
| 動態注入 | 需先清空既有內容 |
| esc 函式 | 必須從統一 `lib/dom.js` 導入 |
| format 函式 | 必須從統一 `lib/format.js` 導入 |
| 檔案搬移 | 必須全域搜尋並替換舊路徑 |
| 無補丁 | 寧願整檔重寫，減少修補風險 |

---

## 四、關聯檔案

| 主題 | 檔案 |
|---|---|
| 通用 SOP | W-06-sop.md |
| ES Module 架構 | W-03-esmodule.md |
| 體系學習日誌 | `R-04-learning.md` |

---

/* ═══ EOF ═══
F: W-07-errors.md
V: 1792581008
W: ai-system-20261021-004#8
═══ */
<!-- === FILE END: AI-kernel/docs/skills/W/W-07-errors.md === -->
