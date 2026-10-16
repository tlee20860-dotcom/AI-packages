# 專案包：web-app-family

> 生成時間：2026-10-16 08:05:38 UTC

## 📌 使用方式

本檔案為「web-app-family」專案的完整上下文包。
AI 已讀取通用規則包（PACKAGE-CORE.md），本包包含專案文件與程式碼。

---

# 💻 專案程式碼

---

## 📄 admin.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>平台管理 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">平台管理</h1>
            <p class="page-subtitle">管理所有家庭與平台預設資料庫</p>
          </div>
          <div id="page-header-actions">
            <button type="button" class="btn btn-danger" id="admin-logout-btn">
              <i data-lucide="log-out"></i> 登出
            </button>
          </div>
        </div>

        <div id="admin-error" class="banner banner-magenta" style="display:none;"></div>
        <div id="admin-tabs"></div>
        <div id="admin-panel-families" class="tab-panel" style="display:none;"></div>
        <div id="admin-panel-accounts" class="tab-panel" style="display:none;"></div>
        <div id="admin-panel-defaults" class="tab-panel" style="display:none;">
          <div class="banner mb-16">
            ℹ️ 此處的預設資料將套用於「新家庭初始化時」，修改後<b>不影響</b>已存在的家庭。
          </div>
          <div id="admin-defaults-root"></div>
        </div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/admin/admin.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { initAdminPage } from './js/admin/admin.js';

    const user = await initApp({
      activeHref: 'admin.html',
      title: '平台管理',
      requireFamily: false,
    });

    if (user) {
      initAdminPage();
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: admin.html
     Version: v103.0.31
     Batch: B26-4
     ═══════════════════════════════════════════ -->```

---

## 📄 annual-report.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>年度報表 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
  <link rel="stylesheet" href="./css/pages/annual-report.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">年度報表</h1>
            <p class="page-subtitle">全年總合與明細</p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <div id="annual-stats-root"></div>
        <div id="page-controls-root"></div>

        <div id="annual-summary-view"></div>
        <div id="annual-monthly-view" style="display:none;"></div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/annual-report.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/annual-report.js';

    const user = await initApp({ activeHref: 'annual-report.html', title: '年度報表' });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: annual-report.html
     Version: v103.0.29
     Batch: B26-3.2
     ═══════════════════════════════════════════ -->```

---

## 📄 code-list.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>編號對照清單 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">編號對照清單</h1>
            <p class="page-subtitle"></p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <!-- 篩選器（類型 + 編號狀態） -->
        <div id="cl-filter-root"></div>

        <!-- 主列表區 -->
        <div id="cl-list-root"></div>

        <!-- 空狀態 -->
        <div id="cl-empty-state" style="display:none;"></div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/code-list.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/code-list.js';

    const user = await initApp({ activeHref: 'code-list.html', title: '編號對照清單' });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: code-list.html
     Version: 1791900580
     Batch: B34-4
     Writer: A20261013-004#077
     ═══════════════════════════════════════════ -->```

---

## 📄 css/components.css

```
/* ============================================
   components.css — 卡片、按鈕、表格、Modal、表單
   -------------------------------------------------
   v103.0.41（B30-3d）：
     ✅ 主要按鈕改實色填充（藍底白字 / 紅底白字）
     ✅ 卡片加色條（左側 4px）
     ✅ Modal 標題加色條
     ✅ Tab 選中改實色
     ✅ Badge 改實色
     ✅ 表格 header 加深
   v103.0.39（B30-3-2）：
     ✅ 硬編碼改走變數
   ============================================ */

/* ================= GlassCard ================= */
.glass-card {
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-md);
  padding: 20px;
  transition: all var(--transition);
  position: relative;
  overflow: hidden;
  box-shadow: var(--shadow-sm);
}

.glass-card:hover {
  border-color: var(--glass-border-hover);
  box-shadow: var(--shadow-md);
}

.glass-card-title {
  font-size: 12px;
  color: var(--text-muted);
  letter-spacing: 1px;
  text-transform: uppercase;
  margin-bottom: 10px;
}

.glass-card-value {
  font-family: var(--font-mono);
  font-size: 26px;
  font-weight: 700;
  color: var(--primary);
  line-height: 1.2;
}

.glass-card-value.magenta { color: var(--danger); }
.glass-card-value.emerald { color: var(--accent-fresh); }
.glass-card-value.red     { color: var(--danger); }
.glass-card-value.orange  { color: var(--accent-warm); }
.glass-card-value.cyan    { color: var(--accent-sky); }

.glass-card-hint {
  font-size: 12px;
  color: var(--text-muted);
  margin-top: 6px;
}

/* ================= Page Banner ================= */
.page-banner { margin-bottom: 16px; }

/* ================= Page Header ================= */
.page-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-wrap: wrap;
  gap: 12px;
  margin-bottom: 24px;
}

.page-header > div:first-child,
.page-header > h1:first-child {
  flex: 1;
  min-width: 0;
}

.page-header-actions-group {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
  flex-shrink: 0;
}

@media (max-width: 640px) {
  .page-header-actions-group { gap: 6px; }
}

/* ================= 按鈕 ================= */
.btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 9px 16px;
  font-size: 14px;
  font-weight: 500;
  border-radius: var(--radius-sm);
  border: 1px solid var(--glass-border);
  background: var(--glass-bg);
  color: var(--text-primary);
  transition: all var(--transition);
  white-space: nowrap;
  font-family: inherit;
  cursor: pointer;
}

.btn:hover {
  background: var(--hover-bg);
  border-color: var(--glass-border-hover);
  box-shadow: var(--shadow-sm);
}

/* 🆕 主要按鈕：實色藍底白字 */
.btn-primary {
  background: var(--primary);
  border-color: var(--primary);
  color: #FFFFFF;
  font-weight: 600;
  box-shadow: var(--shadow-sm);
}
.btn-primary:hover {
  background: var(--primary-dark);
  border-color: var(--primary-dark);
  box-shadow: var(--shadow-md);
}

/* 危險按鈕：實色紅底白字 */
.btn-danger {
  background: var(--danger);
  border-color: var(--danger);
  color: #FFFFFF;
  font-weight: 600;
  box-shadow: var(--shadow-sm);
}
.btn-danger:hover {
  background: var(--danger-dark);
  border-color: var(--danger-dark);
  box-shadow: var(--shadow-md);
}

/* Magenta：橙底白字 */
.btn-magenta {
  background: var(--accent-warm);
  border-color: var(--accent-warm);
  color: #FFFFFF;
  font-weight: 600;
}
.btn-magenta:hover {
  background: #EA580C;
  border-color: #EA580C;
}

.btn-ghost {
  background: transparent;
  border-color: transparent;
  color: var(--text-secondary);
}
.btn-ghost:hover {
  background: var(--hover-bg);
  color: var(--primary);
  box-shadow: none;
}

.btn-sm { padding: 6px 10px; font-size: 12px; }

.btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
  box-shadow: none;
}

/* ================= 輸入框 ================= */
.input, .select {
  width: 100%;
  padding: 10px 12px;
  background: var(--input-bg);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-sm);
  color: var(--text-primary);
  font-size: 14px;
  font-family: inherit;
  transition: all var(--transition);
}

.input:focus, .select:focus {
  outline: none;
  border-color: var(--primary);
  box-shadow: 0 0 0 3px rgba(29, 78, 216, 0.15);
}

.input::placeholder { color: var(--text-muted); }

.field { margin-bottom: 14px; }

.field-label {
  display: block;
  font-size: 12px;
  color: var(--text-secondary);
  margin-bottom: 6px;
  letter-spacing: 0.3px;
  font-weight: 500;
}

/* ================= 通用表格 ================= */
.data-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
}

.data-table th {
  text-align: left;
  padding: 12px 14px;
  font-size: 11px;
  font-weight: 700;
  color: var(--text-secondary);
  letter-spacing: 1px;
  text-transform: uppercase;
  border-bottom: 2px solid var(--divider-strong);
  background: var(--hover-bg);
}

.data-table td {
  padding: 12px 14px;
  border-bottom: 1px solid var(--divider);
  color: var(--text-primary);
}

.data-table tr:hover td { background: var(--hover-bg); }

.data-table .num {
  font-family: var(--font-mono);
  text-align: right;
}

/* ================= 表頭排序 / 篩選 ================= */
.dt-th-sortable {
  position: relative;
  user-select: none;
}

.dt-th-label {
  display: inline-block;
  vertical-align: middle;
}

.dt-sort-btn,
.dt-filter-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  margin-left: 4px;
  padding: 0;
  border: none;
  border-radius: 3px;
  background: transparent;
  color: var(--text-muted);
  cursor: pointer;
  vertical-align: middle;
  transition: all 0.15s ease;
  font-family: inherit;
}

.dt-sort-btn:hover,
.dt-filter-btn:hover {
  background: var(--hover-bg-strong);
  color: var(--primary);
}

.dt-sort-btn.active {
  color: #FFFFFF;
  background: var(--primary);
}

.dt-filter-btn.active {
  color: #FFFFFF;
  background: var(--accent-warm);
}

.dt-filter-panel {
  background: var(--filter-panel-bg);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-sm);
  padding: 12px;
  min-width: 200px;
  box-shadow: var(--shadow-lg);
  animation: dtFilterIn 0.15s ease-out;
}

@keyframes dtFilterIn {
  from { opacity: 0; transform: translateY(-4px); }
  to   { opacity: 1; transform: translateY(0); }
}

.dt-filter-panel-title {
  font-size: 12px;
  color: var(--text-muted);
  letter-spacing: 1px;
  text-transform: uppercase;
  margin-bottom: 8px;
}

.dt-filter-panel-actions {
  display: flex;
  gap: 6px;
  justify-content: flex-end;
  margin-top: 10px;
}

@media (max-width: 640px) {
  .dt-sort-btn, .dt-filter-btn {
    width: 18px;
    height: 18px;
    margin-left: 2px;
  }
  .dt-filter-panel { min-width: 160px; }
}

/* ================= 進度條 ================= */
.progress {
  height: 8px;
  background: var(--divider);
  border-radius: 4px;
  overflow: hidden;
  position: relative;
}

.progress-bar {
  height: 100%;
  background: linear-gradient(90deg, var(--accent-sky), var(--primary));
  border-radius: 4px;
  transition: width 0.5s ease;
}

/* ================= Badge（實色） ================= */
.badge {
  display: inline-block;
  padding: 3px 9px;
  border-radius: 999px;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.5px;
  color: #FFFFFF;
}

.badge-success { background: var(--accent-fresh); }
.badge-pending { background: var(--accent-warm); }
.badge-info    { background: var(--accent-sky); }
.badge-magenta { background: var(--danger); }
.badge-muted   { background: var(--text-muted); }

/* ================= 空狀態 / 橫幅 ================= */
.empty-state {
  text-align: center;
  padding: 40px 20px;
  color: var(--text-muted);
  font-size: 14px;
}

.banner {
  padding: 12px 16px;
  border-radius: var(--radius-sm);
  border: 1px solid var(--glass-border);
  background: var(--hover-bg);
  font-size: 13px;
  margin-bottom: 16px;
  color: var(--text-secondary);
  border-left: 4px solid var(--primary);
}

.banner-magenta {
  border-color: rgba(220, 38, 38, 0.3);
  background: rgba(220, 38, 38, 0.06);
  color: var(--danger);
  border-left-color: var(--danger);
}

.banner-success {
  border-color: rgba(16, 185, 129, 0.3);
  background: rgba(16, 185, 129, 0.06);
  color: var(--accent-fresh);
  border-left-color: var(--accent-fresh);
}

.banner-error {
  border-color: rgba(220, 38, 38, 0.3);
  background: rgba(220, 38, 38, 0.06);
  color: var(--danger);
  border-left-color: var(--danger);
}

/* ================= Modal ================= */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: var(--overlay-bg);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  display: none;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 20px;
}
.modal-overlay.active { display: flex; }

.modal {
  width: 100%;
  max-width: 440px;
  background: var(--modal-bg);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-md);
  padding: 24px;
  box-shadow: var(--shadow-lg);
  animation: modalIn 0.2s ease-out;
}

@keyframes modalIn {
  from { opacity: 0; transform: translateY(10px) scale(0.98); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}

/* 🆕 Modal 標題加左色條 */
.modal-title {
  font-size: 18px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 16px;
  padding-left: 12px;
  border-left: 4px solid var(--primary);
  line-height: 1.3;
}

.modal-actions {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
  margin-top: 20px;
}

/* ================= 可摺疊卡片 ================= */
.collapsible-card {
  margin-bottom: 16px;
  padding: 0 !important;
  overflow: hidden;
}

.collapsible-card .collapsible-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 20px;
  cursor: pointer;
  user-select: none;
  transition: background 0.2s ease;
}

.collapsible-card .collapsible-header:hover {
  background: var(--hover-bg);
}

.collapsible-card .collapsible-header-title {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 14px;
  font-weight: 600;
  color: var(--text-primary);
}

.collapsible-card .collapsible-arrow {
  color: var(--text-muted);
  transition: transform 0.25s ease;
}

.collapsible-card.open .collapsible-arrow { transform: rotate(180deg); }

.collapsible-card .collapsible-body {
  padding: 0 20px 20px;
  border-top: 1px solid var(--divider);
  padding-top: 16px;
}

.collapsible-card-flat .collapsible-body {
  padding: 0;
  border-top: 1px solid var(--divider);
}
.collapsible-card-flat .collapsible-body > table { margin: 0; }
.collapsible-card-flat .collapsible-body .data-table thead th { border-top: none; }

/* ================= 表單直落 ================= */
.form-stack {
  display: flex;
  flex-direction: column;
  gap: 14px;
}
.form-stack > .field { margin-bottom: 0; }

.form-stack-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}
.form-stack-row > .field { margin-bottom: 0; }

.form-stack textarea.input {
  resize: vertical;
  min-height: 60px;
  font-family: inherit;
  line-height: 1.4;
}

@media (max-width: 640px) {
  .form-stack-row { grid-template-columns: 1fr; }
}

/* ================= 頁面篩選欄 ================= */
.page-filter-bar {
  display: flex;
  gap: 12px;
  align-items: flex-end;
  flex-wrap: wrap;
}

.page-filter-bar .filter-group {
  display: flex;
  flex-direction: column;
  gap: 4px;
  min-width: 100px;
}

.page-filter-bar .filter-group .field-label {
  font-size: 11px;
  margin-bottom: 0;
}

.page-filter-bar .select {
  padding: 6px 10px;
  font-size: 13px;
}

@media (max-width: 640px) {
  .page-filter-bar { gap: 8px; flex-direction: row; }
  .page-filter-bar .filter-group { min-width: auto; flex: 1; }
  .page-filter-bar .select { padding: 5px 8px; font-size: 12px; }
}

/* ================= View Toggle ================= */
.view-toggle-group {
  display: inline-flex;
  gap: 4px;
  padding: 3px;
  background: var(--divider);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-sm);
}

.view-toggle-group .btn {
  padding: 5px 12px;
  font-size: 12px;
  border: none;
}

.view-toggle-group .btn.active {
  background: var(--primary);
  color: #FFFFFF;
}

/* ================= Page Controls ================= */
.page-controls {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-bottom: 20px;
}

.page-controls-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
}

.page-controls-row.page-controls-full { display: block; }

.page-controls-left,
.page-controls-right {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
}

.page-controls-left {
  flex: 1;
  min-width: 0;
  justify-content: flex-start;
}

.page-controls-right {
  flex-shrink: 0;
  justify-content: flex-end;
}

.page-controls-filter { width: 100%; }

.page-controls-row.page-controls-period .select {
  padding: 6px 10px;
  font-size: 13px;
  width: auto;
}

@media (max-width: 640px) {
  .page-controls-row { gap: 8px; }
  .page-controls-left,
  .page-controls-right { gap: 6px; }
  .page-controls-row.page-controls-period .select {
    padding: 5px 8px;
    font-size: 12px;
  }
}

/* ================= 保險卡片 ================= */
.policy-card {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.policy-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 12px;
}

.policy-name {
  font-size: 16px;
  font-weight: 700;
  color: var(--text-primary);
  line-height: 1.3;
}

.policy-company {
  font-size: 11px;
  color: var(--accent-warm);
  letter-spacing: 1px;
  text-transform: uppercase;
  margin-top: 2px;
}

.policy-info-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  font-size: 12px;
}

.policy-info-item {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.policy-info-label {
  color: var(--text-muted);
  font-size: 10px;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.policy-info-value {
  color: var(--text-primary);
  font-family: var(--font-mono);
  font-size: 13px;
}

.policy-progress {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.policy-progress-text {
  display: flex;
  justify-content: space-between;
  font-size: 11px;
  color: var(--text-muted);
  font-family: var(--font-mono);
}

.policy-actions {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  margin-top: 4px;
}

/* ================= 保險表格 ================= */
.policy-table-wrapper { overflow: hidden; }

.policy-table-wrapper > div {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}

.policy-table {
  width: 100%;
  min-width: 1080px;
  table-layout: fixed;
  border-collapse: collapse;
  font-size: 13px;
}

.policy-table thead th {
  padding: 12px 8px;
  text-align: left;
  font-size: 11px;
  font-weight: 700;
  color: var(--text-secondary);
  letter-spacing: 0.5px;
  text-transform: uppercase;
  border-bottom: 2px solid var(--divider-strong);
  background: var(--hover-bg);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  vertical-align: middle;
}

.policy-table td {
  padding: 12px 8px;
  border-bottom: 1px solid var(--divider);
  color: var(--text-primary);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  vertical-align: middle;
}

.policy-table tbody tr:hover td { background: var(--hover-bg); }

.policy-table .num {
  text-align: right;
  font-family: var(--font-mono);
  font-size: 12px;
}

.policy-table thead th:nth-child(1)  { width: 36px; padding: 12px 4px; }
.policy-table thead th:nth-child(2)  { width: 85px; }
.policy-table thead th:nth-child(3)  { width: 60px; }
.policy-table thead th:nth-child(4)  { width: auto; }
.policy-table thead th:nth-child(5)  { width: 100px; }
.policy-table thead th:nth-child(6)  { width: 95px; }
.policy-table thead th:nth-child(7)  { width: 100px; }
.policy-table thead th:nth-child(8)  { width: 95px; }
.policy-table thead th:nth-child(9)  { width: 85px; }
.policy-table thead th:nth-child(10) { width: 130px; }
.policy-table thead th:nth-child(11) { width: 115px; }

.policy-table td.policy-name-cell {
  white-space: normal;
  word-break: break-word;
  line-height: 1.35;
  overflow: visible;
  text-overflow: clip;
}

.policy-table .progress-text {
  font-size: 11px;
  color: var(--text-muted);
  margin-bottom: 4px;
  font-family: var(--font-mono);
  white-space: nowrap;
}

.policy-table .btn-sm {
  padding: 4px 6px;
  font-size: 11px;
  margin-right: 2px;
}

/* ================= 明細行 ================= */
.insurance-table-detail-row > td {
  padding: 0 !important;
  background: var(--hover-bg);
  border-bottom: 1px solid var(--divider);
  overflow: hidden;
}

.detail-wrapper {
  padding: 12px;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  max-width: 100%;
}

/* ================= 年度區間 header ================= */
.insurance-year-block {
  margin-bottom: 8px;
  border: 1px solid var(--divider);
  border-radius: var(--radius-sm);
  overflow: hidden;
  min-width: 480px;
}

.insurance-year-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 12px;
  background: var(--hover-bg);
  cursor: pointer;
  user-select: none;
  transition: background 0.2s ease;
}

.insurance-year-header:hover { background: var(--hover-bg-strong); }

.ins-year-title {
  font-size: 13px;
  font-weight: 600;
  color: var(--text-primary);
}

.ins-year-right {
  display: flex;
  align-items: center;
  gap: 10px;
}

.ins-paid-sum {
  font-size: 11px;
  color: var(--text-muted);
}

.insurance-year-body { padding: 8px 12px; }

/* ================= 保險：每月 row ================= */
.insurance-month-row {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 6px 0;
  border-bottom: 1px solid var(--divider);
}

.insurance-month-row .ins-date {
  width: 70px;
  flex: 0 0 auto;
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--text-secondary);
}

.insurance-month-row .ins-paid-label {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 12px;
  white-space: nowrap;
  cursor: pointer;
  flex: 0 0 auto;
  min-width: 70px;
  color: var(--text-secondary);
}

.insurance-month-row .ins-paid {
  width: auto;
  cursor: pointer;
}

.insurance-month-row input.ins-amount {
  width: 110px;
  flex: 0 0 auto;
  padding: 4px 10px;
  font-size: 12px;
  text-align: right;
  background: var(--input-bg);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-sm);
  color: var(--text-primary);
  font-family: var(--font-mono);
}

.insurance-month-row input.ins-amount:focus {
  outline: none;
  border-color: var(--primary);
}

/* ================= 已供滿保單區塊 ================= */
.completed-section {
  border-color: rgba(16, 185, 129, 0.3) !important;
  background: rgba(16, 185, 129, 0.04) !important;
}
.completed-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  user-select: none;
}
.completed-arrow {
  transition: transform 0.25s ease;
  color: var(--accent-fresh);
}
.completed-section.open .completed-arrow { transform: rotate(180deg); }
.completed-body {
  margin-top: 16px;
  padding-top: 16px;
  border-top: 1px dashed rgba(16, 185, 129, 0.2);
}
.completed-body .policy-card { opacity: 0.85; }

.insurance-expand-btn { justify-content: space-between; }
.insurance-expand-body { animation: fadeIn 0.2s ease; }
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

/* ================= 綜合輸入中心共用表格 ================= */
.input-center-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
}

.input-center-table th {
  padding: 10px 10px;
  text-align: left;
  font-size: 11px;
  font-weight: 700;
  color: var(--text-secondary);
  letter-spacing: 0.5px;
  border-bottom: 2px solid var(--divider-strong);
  background: var(--hover-bg);
  white-space: nowrap;
}

.input-center-table td {
  padding: 10px 10px;
  border-bottom: 1px solid var(--divider);
  color: var(--text-primary);
  white-space: nowrap;
  vertical-align: middle;
}

.input-center-table tbody tr:hover td { background: var(--hover-bg); }

.input-center-table .num {
  text-align: right;
  font-family: var(--font-mono);
}

.input-center-table.wide { min-width: 720px; }

.input-center-table-wrapper {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  width: 100%;
}

/* ================= Tab 相關 ================= */
.tab-bar {
  display: flex;
  gap: 4px;
  padding: 4px;
  background: var(--divider);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-sm);
  overflow-x: auto;
  margin-bottom: 16px;
  scrollbar-width: thin;
}

.tab-bar .tab-btn {
  flex: 1;
  min-width: 100px;
  padding: 8px 14px;
  font-size: 13px;
  font-weight: 500;
  border-radius: var(--radius-sm);
  border: none;
  background: transparent;
  color: var(--text-secondary);
  cursor: pointer;
  white-space: nowrap;
  transition: all var(--transition);
  font-family: inherit;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
}

.tab-bar .tab-btn:hover {
  color: var(--primary);
  background: var(--hover-bg);
}

.tab-bar .tab-btn.active {
  background: var(--primary);
  color: #FFFFFF;
  font-weight: 700;
}

.tab-panel { display: none; }
.tab-panel.active { display: block; }

.tab-bar.tab-bar-wrap {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 4px;
}
.tab-bar.tab-bar-wrap .tab-btn {
  flex: none;
  min-width: 0;
  width: 100%;
}

@media (max-width: 640px) {
  .tab-bar { gap: 2px; padding: 3px; }
  .tab-bar .tab-btn {
    padding: 7px 10px;
    font-size: 12px;
    min-width: 80px;
  }
  .tab-bar.tab-bar-wrap { grid-template-columns: repeat(2, 1fr); }
  .tab-bar.tab-bar-wrap .tab-btn {
    padding: 8px 6px;
    font-size: 11px;
    min-width: 0;
  }
}

/* ================= 手機版優化 ================= */
@media (max-width: 640px) {
  .data-table th, .data-table td {
    padding: 8px 6px;
    font-size: 12px;
  }
  .data-table .badge {
    padding: 2px 6px;
    font-size: 10px;
    white-space: nowrap;
  }
  .policy-info-grid {
    grid-template-columns: 1fr 1fr;
    gap: 8px;
  }
  .policy-info-value {
    font-size: 12px;
    word-break: break-all;
  }
  .policy-name { font-size: 14px; }
  .policy-company { font-size: 10px; }
  .btn { padding: 8px 12px; font-size: 12px; }
  .btn-sm { padding: 4px 8px; font-size: 11px; }
  .badge { display: inline-block; white-space: nowrap; }
  .modal {
    padding: 16px;
    max-height: 85vh;
    overflow-y: auto;
  }
  .modal-title { font-size: 16px; }
  .policy-table { min-width: 480px; }
  .policy-table .hide-mobile { display: none; }
  .policy-table thead th:nth-child(1)  { width: 30px; padding: 8px 3px; }
  .policy-table thead th:nth-child(4)  { width: auto; min-width: 100px; }
  .policy-table thead th:nth-child(6)  { width: 80px; }
  .policy-table thead th:nth-child(8)  { width: 80px; }
  .policy-table thead th:nth-child(10) { width: 100px; }
  .policy-table thead th:nth-child(11) { width: 90px; }
  .policy-table thead th,
  .policy-table td {
    padding: 8px 5px;
    font-size: 11px;
  }
  .policy-table .num { font-size: 11px; }
  .policy-table .progress-text {
    font-size: 10px;
    white-space: nowrap;
    margin-bottom: 3px;
  }
  .policy-table .progress { height: 5px; }
  .policy-table .btn-sm {
    padding: 3px 5px;
    font-size: 10px;
    margin-right: 2px;
  }
  .detail-wrapper { padding: 8px; }
  .insurance-year-block { min-width: 360px; }
  .insurance-year-header { padding: 6px 8px; }
  .ins-year-title { font-size: 11px; }
  .ins-paid-sum { font-size: 10px; }
  .insurance-month-row { gap: 8px; padding: 4px 0; }
  .insurance-month-row .ins-date { width: 60px; font-size: 11px; }
  .insurance-month-row .ins-paid-label { font-size: 11px; gap: 4px; min-width: 60px; }
  .insurance-month-row input.ins-amount {
    width: 80px;
    padding: 3px 6px;
    font-size: 11px;
  }
  .insurance-month-row .ins-paid { transform: scale(0.9); }
  .collapsible-card .collapsible-header { padding: 12px 16px; }
  .collapsible-card .collapsible-body { padding: 0 16px 16px; padding-top: 14px; }
  .input-center-table th,
  .input-center-table td {
    padding: 8px 8px;
    font-size: 12px;
  }
  .data-card { padding: 16px; }
  .data-card-primary {
    font-size: 15px;
    font-weight: 700;
    color: var(--text-primary);
    margin-bottom: 10px;
    line-height: 1.35;
  }
  .data-card-fields {
    display: flex;
    flex-direction: column;
    gap: 6px;
    font-size: 13px;
  }
  .data-card-field {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 12px;
  }
  .data-card-label {
    color: var(--text-muted);
    font-size: 11px;
    flex-shrink: 0;
  }
  .data-card-value {
    text-align: right;
    word-break: break-word;
  }
  .data-card-footer {
    margin-top: 12px;
    padding-top: 12px;
    border-top: 1px dashed var(--divider-strong);
    display: flex;
    gap: 8px;
    justify-content: flex-end;
    flex-wrap: wrap;
  }
}

/* ================= data-card（桌面） ================= */
.data-card { padding: 16px; }
.data-card-primary {
  font-size: 15px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 10px;
  line-height: 1.35;
}
.data-card-fields {
  display: flex;
  flex-direction: column;
  gap: 6px;
  font-size: 13px;
}
.data-card-field {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
}
.data-card-label {
  color: var(--text-muted);
  font-size: 11px;
  flex-shrink: 0;
}
.data-card-value {
  text-align: right;
  word-break: break-word;
}
.data-card-footer {
  margin-top: 12px;
  padding-top: 12px;
  border-top: 1px dashed var(--divider-strong);
  display: flex;
  gap: 8px;
  justify-content: flex-end;
  flex-wrap: wrap;
}

/* ================= 橫排表格 ================= */
.data-table-scroll-wrapper {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  width: 100%;
}

.data-table:not(.mobile-cards) {
  min-width: 100%;
  table-layout: auto;
}

.data-table:not(.mobile-cards) th,
.data-table:not(.mobile-cards) td {
  white-space: nowrap;
}

@media (max-width: 640px) {
  .data-table:not(.mobile-cards) th,
  .data-table:not(.mobile-cards) td {
    padding: 8px 10px;
    font-size: 12px;
  }
  .data-table:not(.mobile-cards) .btn-sm {
    padding: 3px 6px;
    font-size: 10px;
  }
}

/* ================= 可摺疊表格 ================= */
.data-table-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  margin-bottom: 10px;
  flex-wrap: wrap;
}

.data-table-header-left {
  display: flex;
  align-items: center;
  gap: 8px;
  min-width: 0;
}

.data-table-header-left .btn { font-weight: 500; }

.data-table-header-right {
  display: flex;
  gap: 6px;
  align-items: center;
  flex-wrap: wrap;
}

.data-table .expand-col {
  width: 36px;
  padding: 4px !important;
  text-align: center;
}

.data-table .expand-col .btn {
  padding: 2px 6px;
  min-width: auto;
}

.data-table .detail-row > .detail-cell {
  padding: 0 !important;
  background: var(--hover-bg);
  border-bottom: 1px solid var(--divider);
}

.data-table .detail-cell > * { padding: 12px 16px; }

@media (max-width: 640px) {
  .data-table-header { gap: 8px; }
  .data-table-header-right .btn-sm span:not(.hide-mobile) { display: none; }
}

/* ================= 統一卡片網格 ================= */
.data-cards-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;
}

@media (max-width: 1024px) {
  .data-cards-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 640px) {
  .data-cards-grid { grid-template-columns: 1fr; }
}

/* ================= 統計卡 — 整合卡 ================= */
.stats-integrated {
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-md);
  padding: 0;
  overflow: hidden;
  box-shadow: var(--shadow-sm);
}

.stats-integrated-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1px;
  background: var(--divider-strong);
}

.stats-integrated-cell {
  padding: 16px 18px;
  background: var(--glass-bg);
  position: relative;
}

.stats-integrated-grid > .stats-integrated-cell[style*="grid-column: span 2"] {
  grid-column: span 2;
}

.stats-integrated-title {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 11px;
  color: var(--text-muted);
  letter-spacing: 1px;
  text-transform: uppercase;
  margin-bottom: 6px;
}

.stats-integrated-value {
  font-family: var(--font-mono);
  font-size: 20px;
  font-weight: 700;
  color: var(--primary);
  line-height: 1.2;
  word-break: break-all;
}

.stats-integrated-value.emerald { color: var(--accent-fresh); }
.stats-integrated-value.magenta { color: var(--danger); }
.stats-integrated-value.red     { color: var(--danger); }
.stats-integrated-value.orange  { color: var(--accent-warm); }
.stats-integrated-value.cyan    { color: var(--accent-sky); }

.stats-integrated-hint {
  font-size: 10px;
  color: var(--text-muted);
  margin-top: 4px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* ================= 統計卡 — 緊湊獨立卡 ================= */
.stats-compact-grid {
  display: grid;
  gap: 8px;
}
.stats-compact-grid-2 { grid-template-columns: repeat(2, 1fr); }
.stats-compact-grid-3 { grid-template-columns: repeat(3, 1fr); }
.stats-compact-grid-4 { grid-template-columns: repeat(4, 1fr); }

.stats-compact-card { padding: 12px 14px !important; }

.stats-compact-card .glass-card-title {
  font-size: 10px;
  letter-spacing: 0.5px;
  margin-bottom: 6px;
  line-height: 1.3;
}

.stats-compact-card .glass-card-value {
  font-size: 17px !important;
  line-height: 1.2;
  word-break: break-all;
}

.stats-compact-card .glass-card-hint {
  font-size: 10px;
  margin-top: 4px;
  line-height: 1.3;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* ================= 響應式 ================= */
@media (max-width: 1024px) {
  .stats-integrated-grid { grid-template-columns: repeat(2, 1fr); }
  .stats-compact-grid-4 { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 640px) {
  .stats-integrated-grid { grid-template-columns: repeat(2, 1fr); }
  .stats-integrated-cell { padding: 12px 12px; }
  .stats-integrated-value { font-size: 16px; }
  .stats-integrated-title { font-size: 10px; margin-bottom: 4px; }
  .stats-integrated-hint { font-size: 9px; }
  .stats-compact-grid-3,
  .stats-compact-grid-4 { grid-template-columns: repeat(2, 1fr); }
  .stats-compact-card { padding: 10px 12px !important; }
  .stats-compact-card .glass-card-value { font-size: 15px !important; }
}

/* ================= settings 個人化 Panel ================= */
.settings-personal-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(360px, 1fr));
  gap: 16px;
  align-items: start;
}

/* ================= Navbar 使用者資訊 ================= */
.navbar-user {
  display: flex;
  align-items: center;
  margin-left: auto;
}

.navbar-user-text {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 280px;
}

@media (max-width: 640px) {
  .navbar-title { display: none !important; }
  .navbar-user-text {
    font-size: 11px !important;
    margin-right: 4px !important;
    max-width: 100px;
  }
}
/* ================= Navbar 右側群組 ================= */
.navbar-right {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-left: auto;
}

/* DB 標籤 */
.navbar-db-tag {
  display: inline-flex;
  align-items: center;
  padding: 4px 10px;
  border-radius: 999px;
  font-size: 11px;
  font-weight: 600;
  font-family: var(--font-mono);
  background: var(--divider-strong);
  color: var(--text-secondary);
  letter-spacing: 0.3px;
  white-space: nowrap;
}

.navbar-db-tag-new {
  background: var(--primary);
  color: #FFFFFF;
}

/* 主題切換按鈕 */
.navbar-theme-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 34px;
  height: 34px;
  border-radius: var(--radius-sm);
  border: 1px solid var(--glass-border);
  background: var(--glass-bg);
  color: var(--text-secondary);
  cursor: pointer;
  transition: all var(--transition);
}

.navbar-theme-btn:hover {
  border-color: var(--glass-border-hover);
  color: var(--primary);
  box-shadow: var(--shadow-sm);
}

@media (max-width: 640px) {
  .navbar-db-tag {
    font-size: 10px;
    padding: 3px 8px;
    max-width: 120px;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .navbar-theme-btn {
    width: 30px;
    height: 30px;
  }
  .navbar-right { gap: 6px; }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: css/components.css
   Version: v103.0.41
   Batch: B30-3d
   ═══════════════════════════════════════════ */```

---

## 📄 css/layout.css

```
/* ============================================
   layout.css — App Shell 版型（v103.0.39）
   -------------------------------------------------
   v103.0.39（B30-3-2）：
     ✅ 硬編碼深色改走變數
     ✅ page-title 移除霓虹陰影
   v103.0.34（B29-5d）：
     ✅ .page-subtitle[data-prefix]::before
   ============================================ */

.app-shell {
  display: flex;
  min-height: 100vh;
  position: relative;
  z-index: 1;
}

/* ================= Sidebar ================= */
.sidebar {
  width: 260px;
  flex-shrink: 0;
  background: var(--sidebar-bg);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-right: 1px solid var(--glass-border);
  display: flex;
  flex-direction: column;
  transition: width var(--transition), transform var(--transition);
  overflow: hidden;
}

.sidebar-header {
  height: 64px;
  display: flex;
  align-items: center;
  padding: 0 20px;
  border-bottom: 1px solid var(--glass-border);
  flex-shrink: 0;
}

.sidebar-logo {
  font-family: var(--font-mono);
  font-weight: 700;
  font-size: 16px;
  color: var(--primary);
  white-space: nowrap;
  letter-spacing: 1px;
}

.sidebar-nav {
  flex: 1;
  min-height: 0;
  overflow-y: auto;
  overflow-x: hidden;
  padding: 12px 0 24px;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 11px 20px;
  color: var(--text-secondary);
  font-size: 14px;
  cursor: pointer;
  border-left: 3px solid transparent;
  transition: all var(--transition);
  white-space: nowrap;
  text-decoration: none;
}

.nav-item:hover {
  background: var(--hover-bg);
  color: var(--primary);
  text-decoration: none;
}

.nav-item.active {
  color: var(--primary);
  background: var(--hover-bg-strong);
  border-left-color: var(--primary);
}

.nav-item .nav-icon {
  width: 18px;
  height: 18px;
  flex-shrink: 0;
}

.nav-item .nav-label {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.nav-group { margin-bottom: 4px; }

.nav-group-title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 8px;
  padding: 10px 20px;
  color: var(--text-muted);
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  cursor: pointer;
  user-select: none;
  transition: color var(--transition);
}

.nav-group-title:hover { color: var(--primary); }

.nav-group-title .nav-group-label {
  display: flex;
  align-items: center;
  gap: 8px;
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.nav-group-title .nav-group-icon {
  width: 14px;
  height: 14px;
  flex-shrink: 0;
}

.nav-group-title .nav-group-arrow {
  width: 12px;
  height: 12px;
  flex-shrink: 0;
  color: var(--text-muted);
  transition: transform 0.25s ease, color var(--transition);
}

.nav-group-title:hover .nav-group-arrow { color: var(--primary); }

.nav-group-title.open .nav-group-arrow { transform: rotate(180deg); }

.nav-sub-group { margin: 2px 0; }

.nav-sub-title {
  padding-left: 32px;
  font-size: 11px;
  letter-spacing: 1px;
}

.nav-sub .nav-item {
  padding-left: 52px;
  font-size: 13px;
}

.sidebar.collapsed { width: 64px; }

.sidebar.collapsed .sidebar-logo,
.sidebar.collapsed .nav-label,
.sidebar.collapsed .nav-group-title,
.sidebar.collapsed .nav-sub-title {
  display: none;
}

.sidebar.collapsed .nav-item {
  justify-content: center;
  padding: 11px 0;
  border-left: none;
}

.sidebar.collapsed .nav-item.active {
  border-left: 3px solid var(--primary);
}

/* ================= Main Area ================= */
.main-area {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.navbar {
  height: 64px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 0 24px;
  background: var(--navbar-bg);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--glass-border);
  position: sticky;
  top: 0;
  z-index: 100;
}

.navbar-title {
  font-size: 16px;
  font-weight: 600;
  color: var(--text-primary);
  flex: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.navbar-user {
  display: flex;
  align-items: center;
}

.hamburger {
  background: transparent;
  border: 1px solid var(--glass-border);
  color: var(--primary);
  width: 38px;
  height: 38px;
  border-radius: var(--radius-sm);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition);
  flex-shrink: 0;
}

.hamburger:hover {
  border-color: var(--glass-border-hover);
  box-shadow: var(--shadow-sm);
}

.content {
  flex: 1;
  padding: 24px;
  max-width: 1400px;
  width: 100%;
  margin: 0 auto;
}

.page-header { margin-bottom: 24px; }

.page-title {
  font-size: 24px;
  font-weight: 700;
  color: var(--primary);
  margin-bottom: 4px;
}

.page-subtitle {
  font-size: 13px;
  color: var(--text-muted);
}

.page-subtitle[data-prefix]::before {
  content: attr(data-prefix);
}

.grid { display: grid; gap: 16px; }
.grid-2 { grid-template-columns: repeat(2, 1fr); }
.grid-3 { grid-template-columns: repeat(3, 1fr); }
.grid-4 { grid-template-columns: repeat(4, 1fr); }

@media (max-width: 1024px) {
  .grid-4 { grid-template-columns: repeat(2, 1fr); }
  .grid-3 { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 640px) {
  .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr; }
  .content { padding: 16px; }

  .sidebar {
    position: fixed !important;
    top: 0 !important;
    left: -260px !important;
    height: 100vh !important;
    width: 260px !important;
    z-index: 10001 !important;
    overflow: hidden !important;
    transition: left 0.3s ease !important;
  }
  .sidebar.mobile-open { left: 0 !important; }

  .sidebar.collapsed { width: 260px !important; }
  .sidebar.collapsed .sidebar-logo,
  .sidebar.collapsed .nav-label,
  .sidebar.collapsed .nav-group-title,
  .sidebar.collapsed .nav-sub-title {
    display: block;
  }
  .sidebar.collapsed .nav-item {
    justify-content: flex-start;
    padding: 11px 20px;
  }
  .sidebar.collapsed .nav-sub .nav-item {
    padding-left: 52px;
  }

  .sidebar-backdrop {
    display: none;
    position: fixed;
    top: 0;
    left: 260px;
    right: 0;
    bottom: 0;
    background: var(--overlay-bg);
    z-index: 10000 !important;
    cursor: pointer;
  }
  .sidebar-backdrop.active { display: block; }

  .navbar {
    padding: 0 12px;
    gap: 8px;
    z-index: 100 !important;
  }
  .navbar-title { display: none !important; }
  .navbar-user { margin-left: auto; }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: css/layout.css
   Version: v103.0.39
   Batch: B30-3-2
   ═══════════════════════════════════════════ */```

---

## 📄 css/pages/annual-report.css

```
/* ============================================
   annual-report.css — 年度報表專屬樣式（v103.0.39）
   ============================================
   v103.0.39（B30-3-2）：
     ✅ 硬編碼 / 霓虹色改走變數
   v103.0.22（B25-5.1）：
     ✅ 年份 / 月份切換器改為「3 顆按鈕」設計
   ============================================ */

/* ================= 控制列 ================= */
.report-controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
  margin-bottom: 16px;
}

/* ================= 年份切換 ================= */
.year-switcher {
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.year-switcher .year-btn {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  padding: 6px 12px;
  font-size: 13px;
  border-radius: var(--radius-sm);
  border: 1px solid var(--glass-border);
  background: var(--glass-bg);
  color: var(--text-secondary);
  transition: all var(--transition);
  cursor: pointer;
  font-family: inherit;
  white-space: nowrap;
}

.year-switcher .year-btn:hover:not(:disabled):not(.active) {
  color: var(--primary);
  border-color: var(--glass-border-hover);
}

.year-switcher .year-btn.active {
  background: var(--hover-bg-strong);
  border-color: var(--glass-border-hover);
  color: var(--primary);
  font-weight: 700;
  cursor: default;
}

.year-switcher .year-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.year-switcher .year-btn i { flex-shrink: 0; }
.year-switcher .year-btn span { font-family: var(--font-mono); }

/* ================= 月份切換 ================= */
.month-switcher {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  margin-bottom: 16px;
}

.month-switcher .month-btn {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  padding: 6px 12px;
  font-size: 13px;
  border-radius: var(--radius-sm);
  border: 1px solid var(--glass-border);
  background: var(--glass-bg);
  color: var(--text-secondary);
  transition: all var(--transition);
  cursor: pointer;
  font-family: inherit;
  white-space: nowrap;
}

.month-switcher .month-btn:hover:not(:disabled):not(.active) {
  color: var(--primary);
  border-color: var(--glass-border-hover);
}

.month-switcher .month-btn.active {
  background: var(--hover-bg-strong);
  border-color: var(--glass-border-hover);
  color: var(--primary);
  font-weight: 700;
  cursor: default;
}

.month-switcher .month-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.month-switcher .month-btn i { flex-shrink: 0; }
.month-switcher .month-btn span { font-family: var(--font-mono); }

/* ================= 全年總合表格 ================= */
.summary-table-wrapper {
  overflow-x: auto;
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-sm);
  background: var(--glass-bg);
  backdrop-filter: blur(var(--glass-blur));
}

.annual-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
  min-width: 900px;
}

.annual-table th,
.annual-table td {
  padding: 10px 12px;
  border-bottom: 1px solid var(--divider);
  text-align: left;
}

.annual-table thead th {
  font-size: 11px;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 1px;
  white-space: nowrap;
  background: var(--hover-bg);
}

.annual-table .num {
  text-align: right;
  font-family: var(--font-mono);
  white-space: nowrap;
}

.annual-table tr:hover td { background: var(--hover-bg); }

.annual-table .group-header td {
  background: var(--hover-bg-strong);
  color: var(--primary);
  font-weight: 700;
}

.annual-table .shared-row td {
  background: rgba(232, 93, 58, 0.06);
  color: var(--danger);
  font-weight: 600;
}

/* ================= 月度明細表格 ================= */
.monthly-table-wrapper {
  overflow-x: auto;
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-sm);
  background: var(--glass-bg);
  backdrop-filter: blur(var(--glass-blur));
}

.monthly-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
  min-width: 520px;
}

.monthly-table th,
.monthly-table td {
  padding: 10px 14px;
  border-bottom: 1px solid var(--divider);
  text-align: left;
}

.monthly-table thead th {
  font-size: 11px;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 1px;
  background: var(--hover-bg);
}

.monthly-table .num {
  text-align: right;
  font-family: var(--font-mono);
  white-space: nowrap;
}

.monthly-table tr:hover td { background: var(--hover-bg); }

.monthly-table .group-header td {
  background: var(--hover-bg-strong);
  color: var(--primary);
  font-weight: 700;
}

.monthly-table .subtotal-row td {
  font-weight: 700;
  color: var(--text-primary);
  border-top: 1px solid var(--divider-strong);
}

.monthly-table .total-row td {
  font-weight: 700;
  background: var(--hover-bg-strong);
}

.monthly-table .net-row td {
  font-weight: 700;
  background: rgba(232, 93, 58, 0.06);
}

/* ================= 手機版優化 ================= */
@media (max-width: 640px) {
  .annual-table th,
  .annual-table td {
    padding: 8px 8px;
    font-size: 11px;
  }
  .monthly-table th,
  .monthly-table td {
    padding: 8px 10px;
    font-size: 12px;
  }
  .year-switcher .year-btn {
    padding: 5px 9px;
    font-size: 12px;
    gap: 3px;
  }
  .month-switcher .month-btn {
    padding: 5px 9px;
    font-size: 12px;
    gap: 3px;
  }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: css/pages/annual-report.css
   Version: v103.0.39
   Batch: B30-3-2
   ═══════════════════════════════════════════ */```

---

## 📄 css/pages/insurance.css

```
/* ============================================
   insurance.css — 保險頁面專屬樣式（v103.0.39）
   ============================================
   v103.0.39（B30-3-2）：
     ✅ 硬編碼 / 霓虹色改走變數
   v101.10.0 修正：
     ✅ [P3-5] .policy-card::before 圓角統一
   ============================================ */

/* ============================================
   1. 保單卡片 hover 強化
   ============================================ */
.policy-card {
  position: relative;
  transition: all 0.3s ease;
}

.policy-card::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, var(--accent-sky), var(--accent-warm));
  border-radius: var(--radius-sm) var(--radius-sm) 0 0;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.policy-card:hover::before { opacity: 1; }

.policy-card:hover {
  transform: translateY(-2px);
  border-color: var(--glass-border-hover);
}

/* 已供滿卡片：綠頂邊 */
.completed-body .policy-card::before {
  background: linear-gradient(90deg, var(--accent-fresh), var(--accent-sky));
  opacity: 0.6;
}

/* ============================================
   2. 進度條
   ============================================ */
.policy-progress .progress-bar {
  background: linear-gradient(90deg, var(--accent-sky), var(--accent-warm));
}

.policy-progress .progress-bar[style*="width:100%"] {
  background: linear-gradient(90deg, var(--accent-fresh), #34D399);
}

/* ============================================
   3. 已供滿區塊裝飾
   ============================================ */
.completed-section .completed-header { position: relative; }

.completed-section .completed-header::after {
  content: "";
  position: absolute;
  bottom: -1px;
  left: 0;
  width: 60px;
  height: 2px;
  background: var(--accent-fresh);
  border-radius: 1px;
}

.completed-body .policy-card { filter: grayscale(15%); }
.completed-body .policy-card:hover { filter: grayscale(0%); }

/* ============================================
   4. 保單統計卡
   ============================================ */
#grand-total-premium,
#active-policy-count,
#stat-completed-policies {
  transition: all 0.3s ease;
}

/* ============================================
   5. 表格模式 hover
   ============================================ */
.policy-table tbody tr:hover td {
  background: var(--hover-bg-strong);
  border-bottom-color: var(--divider-strong);
}

.policy-table tbody tr.insurance-table-detail-row:hover td {
  background: var(--hover-bg);
}

/* ============================================
   6. 展開按鈕
   ============================================ */
.insurance-expand-btn {
  transition: all 0.25s ease;
  border: 1px solid transparent;
}

.insurance-expand-btn:hover {
  border-color: var(--glass-border);
  background: var(--hover-bg);
}

.insurance-expand-btn span { font-weight: 500; }

/* ============================================
   7. 年度區塊 header
   ============================================ */
.insurance-year-block {
  transition: border-color 0.2s ease;
}

.insurance-year-block:hover {
  border-color: var(--glass-border-hover);
}

.insurance-year-header { position: relative; }

.insurance-year-header::before {
  content: "";
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 3px;
  height: 60%;
  background: var(--primary);
  opacity: 0.5;
  border-radius: 0 2px 2px 0;
  transition: opacity 0.2s ease;
}

.insurance-year-header:hover::before { opacity: 1; }

/* ============================================
   8. 每月扣款列
   ============================================ */
.insurance-month-row {
  transition: background 0.15s ease;
  border-radius: var(--radius-sm);
}

.insurance-month-row:hover { background: var(--hover-bg); }

/* ============================================
   9. 空狀態插圖
   ============================================ */
#insurance-empty-state .empty-state i {
  color: var(--primary);
}

/* ============================================
   10. 手機版
   ============================================ */
@media (max-width: 640px) {
  .policy-progress .progress { height: 6px; }
  .policy-card:hover { transform: none; }
  .policy-card::before { opacity: 0.4; }
}

/* ============================================
   11. 列印友善
   ============================================ */
@media print {
  .policy-card,
  .policy-table,
  .glass-card {
    background: #fff !important;
    color: #000 !important;
    border: 1px solid #ccc !important;
    box-shadow: none !important;
  }
  .policy-progress .progress-bar {
    background: #52B788 !important;
  }
  .insurance-expand-btn,
  .btn {
    display: none !important;
  }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: css/pages/insurance.css
   Version: v103.0.39
   Batch: B30-3-2
   ═══════════════════════════════════════════ */```

---

## 📄 css/theme.css

```
/* ============================================
   theme.css — 家庭暖陽主題（Tailwind 色板 v103.0.41）
   ============================================
   v103.0.41（B30-3d）：
     ✅ 背景加深至 slate-200，與白卡對比明顯
   v103.0.40（B30-3c）：
     ✅ 色值改用 Tailwind 官方色板
   ============================================ */

/* ============================================
   亮色主題（預設）
   ============================================ */
:root,
[data-theme="light"] {
  --bg-deep:  #E2E8F0;   /* slate-200（加深） */
  --bg-navy:  #CBD5E1;   /* slate-300 */
  --bg-panel: #FFFFFF;

  --primary:      #1D4ED8;   /* blue-700 */
  --primary-dark: #1E40AF;   /* blue-800 */
  --accent-warm:  #F97316;   /* orange-500 */
  --accent-fresh: #10B981;   /* emerald-500 */
  --accent-sky:   #3B82F6;   /* blue-500 */
  --danger:       #DC2626;   /* red-600 */
  --danger-dark:  #B91C1C;   /* red-700 */

  --neon-cyan:    #3B82F6;
  --neon-magenta: #DC2626;
  --neon-emerald: #10B981;
  --neon-red:     #DC2626;
  --neon-orange:  #F97316;

  --glass-bg:           #FFFFFF;
  --glass-border:       #CBD5E1;              /* slate-300 */
  --glass-border-hover: #94A3B8;              /* slate-400 */
  --glass-shadow-hover: 0 4px 12px rgba(15, 23, 42, 0.10);
  --glass-blur:         0px;

  --input-bg:        #FFFFFF;
  --modal-bg:        #FFFFFF;
  --overlay-bg:      rgba(15, 23, 42, 0.5);
  --sidebar-bg:      #FFFFFF;
  --navbar-bg:       #FFFFFF;
  --filter-panel-bg: #FFFFFF;

  --hover-bg:        #F1F5F9;              /* slate-100 */
  --hover-bg-strong: #E2E8F0;              /* slate-200 */

  --divider:        #E2E8F0;               /* slate-200 */
  --divider-strong: #CBD5E1;               /* slate-300 */

  --shadow-sm: 0 1px 2px rgba(15, 23, 42, 0.08);
  --shadow-md: 0 4px 6px rgba(15, 23, 42, 0.10);
  --shadow-lg: 0 10px 15px rgba(15, 23, 42, 0.12);

  --text-primary:   #1E293B;   /* slate-800 */
  --text-secondary: #475569;   /* slate-600 */
  --text-muted:     #64748B;   /* slate-500 */

  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;

  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI",
               "PingFang TC", "Microsoft JhengHei", "Noto Sans TC", sans-serif;
  --font-mono: "JetBrains Mono", "SF Mono", "Consolas", "Menlo", monospace;

  --transition: 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

/* ============================================
   深色主題 — [data-theme="dark"]
   ============================================ */
[data-theme="dark"] {
  --bg-deep:  #0F172A;
  --bg-navy:  #1E293B;
  --bg-panel: #1E293B;

  --primary:      #60A5FA;
  --primary-dark: #3B82F6;
  --accent-warm:  #FBBF24;
  --accent-fresh: #34D399;
  --accent-sky:   #60A5FA;
  --danger:       #F87171;
  --danger-dark:  #EF4444;

  --neon-cyan:    #60A5FA;
  --neon-magenta: #F87171;
  --neon-emerald: #34D399;
  --neon-red:     #F87171;
  --neon-orange:  #FBBF24;

  --glass-bg:           #1E293B;
  --glass-border:       #334155;
  --glass-border-hover: #475569;
  --glass-shadow-hover: 0 4px 12px rgba(0, 0, 0, 0.4);
  --glass-blur:         0px;

  --input-bg:        #0F172A;
  --modal-bg:        #1E293B;
  --overlay-bg:      rgba(0, 0, 0, 0.7);
  --sidebar-bg:      #0F172A;
  --navbar-bg:       #0F172A;
  --filter-panel-bg: #1E293B;

  --hover-bg:        #334155;
  --hover-bg-strong: #475569;

  --divider:        #334155;
  --divider-strong: #475569;

  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.4);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.5);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.6);

  --text-primary:   #F1F5F9;
  --text-secondary: #CBD5E1;
  --text-muted:     #94A3B8;
}

/* ============================================
   基礎樣式
   ============================================ */
* { box-sizing: border-box; margin: 0; padding: 0; }

html, body {
  height: 100%;
  background: var(--bg-deep);
  color: var(--text-primary);
  font-family: var(--font-sans);
  font-size: 15px;
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
  transition: background var(--transition), color var(--transition);
}

body::before {
  content: "";
  position: fixed;
  inset: 0;
  background:
    radial-gradient(ellipse at 20% 0%,   rgba(249, 115, 22, 0.04), transparent 50%),
    radial-gradient(ellipse at 80% 100%, rgba(16, 185, 129, 0.04), transparent 50%);
  pointer-events: none;
  z-index: 0;
}

[data-theme="dark"] body::before {
  background:
    radial-gradient(ellipse at 20% 0%,   rgba(96, 165, 250, 0.06), transparent 50%),
    radial-gradient(ellipse at 80% 100%, rgba(52, 211, 153, 0.04), transparent 50%);
}

a { color: var(--primary); text-decoration: none; }
a:hover { text-decoration: underline; }

button { font-family: inherit; cursor: pointer; }

.mono { font-family: var(--font-mono); }

.text-cyan    { color: var(--accent-sky); }
.text-magenta { color: var(--danger); }
.text-emerald { color: var(--accent-fresh); }
.text-red     { color: var(--danger); }
.text-orange  { color: var(--accent-warm); }
.text-muted   { color: var(--text-muted); }

::-webkit-scrollbar { width: 8px; height: 8px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb {
  background: #CBD5E1;
  border-radius: 4px;
}
::-webkit-scrollbar-thumb:hover { background: #94A3B8; }

[data-theme="dark"] ::-webkit-scrollbar-thumb { background: #334155; }
[data-theme="dark"] ::-webkit-scrollbar-thumb:hover { background: #475569; }

/* ================= 認證頁面 ================= */
.auth-page {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 20px;
  position: relative;
  z-index: 1;
}

.auth-card {
  width: 100%;
  max-width: 400px;
  padding: 32px;
}

.auth-title {
  font-family: var(--font-mono);
  font-size: 24px;
  font-weight: 700;
  color: var(--primary);
  text-align: center;
  letter-spacing: 2px;
  margin-bottom: 8px;
}

.auth-subtitle {
  text-align: center;
  font-size: 13px;
  color: var(--text-muted);
  margin-bottom: 24px;
}

@media (max-width: 640px) {
  .auth-card { padding: 24px; }
  .auth-title { font-size: 20px; }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: css/theme.css
   Version: v103.0.41
   Batch: B30-3d
   ═══════════════════════════════════════════ */```

---

## 📄 css/utilities.css

```
/* ============================================
   utilities.css — 全站工具類樣式（v103.0.39）
   -------------------------------------------------
   v103.0.39（B30-3-2）：
     ✅ 硬編碼深色改走變數
   v101.6.6 修正：
     ✅ 移除 .tab-bar / .data-card 重複樣式
   ============================================ */

/* ============================================
   1. 名稱截斷
   ============================================ */
.truncate {
  display: inline-block;
  max-width: 12em;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  vertical-align: middle;
}

@media (max-width: 640px) {
  .truncate { max-width: 6em; }
}

.truncate-expandable {
  cursor: pointer;
  border-bottom: 1px dashed var(--glass-border-hover);
  transition: color 0.2s ease, border-color 0.2s ease;
}

.truncate-expandable:hover {
  color: var(--primary);
  border-bottom-color: var(--primary);
}

.truncate-expandable.expanded {
  max-width: none;
  overflow: visible;
  white-space: normal;
  word-break: break-word;
}

/* ============================================
   2. 手機卡片化表格
   ============================================ */
@media (max-width: 640px) {
  .data-table.mobile-cards {
    display: block;
    border: none;
    min-width: 0 !important;
  }
  .data-table.mobile-cards thead { display: none; }
  .data-table.mobile-cards tbody { display: block; }
  .data-table.mobile-cards tbody tr {
    display: block;
    margin-bottom: 10px;
    padding: 14px 16px;
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    border-radius: var(--radius-md);
    backdrop-filter: blur(var(--glass-blur));
    -webkit-backdrop-filter: blur(var(--glass-blur));
  }
  .data-table.mobile-cards tbody tr:hover td { background: transparent; }
  .data-table.mobile-cards td {
    display: block;
    padding: 4px 0;
    border: none;
    font-size: 13px;
    white-space: normal;
    overflow: visible;
    text-overflow: clip;
  }
  .data-table.mobile-cards td[data-label]::before {
    content: attr(data-label);
    display: inline-block;
    min-width: 70px;
    color: var(--text-muted);
    font-size: 11px;
    margin-right: 8px;
  }
  .data-table.mobile-cards td.hide-mobile,
  .data-table.mobile-cards th.hide-mobile {
    display: none !important;
  }
  .data-table.mobile-cards td[data-primary] {
    font-size: 14px;
    font-weight: 600;
    color: var(--text-primary);
    margin-bottom: 4px;
  }
  .data-table.mobile-cards td[data-primary]::before { display: none; }
  .data-table.mobile-cards td[data-label="金額"],
  .data-table.mobile-cards td.num {
    text-align: right;
    font-family: var(--font-mono);
    font-weight: 700;
  }
  .data-table.mobile-cards td[data-label="操作"] {
    display: flex;
    gap: 8px;
    margin-top: 8px;
    justify-content: flex-end;
    flex-wrap: wrap;
  }
  .data-table.mobile-cards td[data-label="操作"]::before { display: none; }
  .data-table.mobile-cards td[data-label="狀態"] {
    display: flex;
    align-items: center;
    gap: 6px;
  }
}

/* ============================================
   3. 間距工具
   ============================================ */
.mt-0  { margin-top: 0 !important; }
.mt-4  { margin-top: 4px !important; }
.mt-8  { margin-top: 8px !important; }
.mt-12 { margin-top: 12px !important; }
.mt-16 { margin-top: 16px !important; }
.mt-20 { margin-top: 20px !important; }
.mt-24 { margin-top: 24px !important; }

.mb-0  { margin-bottom: 0 !important; }
.mb-4  { margin-bottom: 4px !important; }
.mb-8  { margin-bottom: 8px !important; }
.mb-12 { margin-bottom: 12px !important; }
.mb-16 { margin-bottom: 16px !important; }
.mb-20 { margin-bottom: 20px !important; }
.mb-24 { margin-bottom: 24px !important; }

.ml-4  { margin-left: 4px !important; }
.ml-8  { margin-left: 8px !important; }
.mr-4  { margin-right: 4px !important; }
.mr-8  { margin-right: 8px !important; }

.p-0  { padding: 0 !important; }
.p-4  { padding: 4px !important; }
.p-8  { padding: 8px !important; }
.p-12 { padding: 12px !important; }
.p-16 { padding: 16px !important; }
.p-20 { padding: 20px !important; }

.gap-4  { gap: 4px !important; }
.gap-6  { gap: 6px !important; }
.gap-8  { gap: 8px !important; }
.gap-12 { gap: 12px !important; }
.gap-16 { gap: 16px !important; }
.gap-20 { gap: 20px !important; }

/* ============================================
   4. Flex / 對齊
   ============================================ */
.flex          { display: flex; }
.flex-col      { display: flex; flex-direction: column; }
.flex-center   { display: flex; align-items: center; justify-content: center; }
.flex-between  { display: flex; align-items: center; justify-content: space-between; }
.flex-wrap     { flex-wrap: wrap; }
.items-center  { align-items: center; }
.items-start   { align-items: flex-start; }
.items-end     { align-items: flex-end; }
.justify-end   { justify-content: flex-end; }
.justify-center { justify-content: center; }
.justify-between { justify-content: space-between; }
.flex-1        { flex: 1; }
.flex-shrink-0 { flex-shrink: 0; }

/* ============================================
   5. 文字
   ============================================ */
.text-center { text-align: center; }
.text-right  { text-align: right; }
.text-left   { text-align: left; }
.text-xs     { font-size: 11px; }
.text-sm     { font-size: 12px; }
.text-base   { font-size: 14px; }
.text-lg     { font-size: 16px; }
.text-xl     { font-size: 18px; }
.text-bold   { font-weight: 700; }
.text-mono   { font-family: var(--font-mono); }

.text-ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* ============================================
   6. 顯示
   ============================================ */
.hidden       { display: none !important; }
.block        { display: block !important; }
.inline-block { display: inline-block !important; }

/* ============================================
   7. 分隔線
   ============================================ */
.divider {
  height: 1px;
  background: var(--divider-strong);
  margin: 16px 0;
}

.divider-dashed {
  height: 0;
  border-top: 1px dashed var(--divider-strong);
  margin: 16px 0;
}

/* ============================================
   8. 快速摘要卡
   ============================================ */
.quick-summary-card {
  background: var(--glass-bg);
  backdrop-filter: blur(var(--glass-blur));
  -webkit-backdrop-filter: blur(var(--glass-blur));
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-lg);
  padding: 20px;
  margin-top: 16px;
}

.quick-summary-title {
  font-size: 13px;
  font-weight: 600;
  color: var(--primary);
  letter-spacing: 1px;
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.quick-summary-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.quick-summary-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 6px 0;
  font-size: 13px;
}

.quick-summary-row .label { color: var(--text-secondary); }

.quick-summary-row .value {
  font-family: var(--font-mono);
  font-weight: 600;
  color: var(--text-primary);
}

@media (max-width: 640px) {
  .quick-summary-card { padding: 16px; }
  .quick-summary-row { font-size: 12px; }
}

/* ============================================
   9. 內容區最小高度
   ============================================ */
.content-min-height {
  min-height: calc(100vh - 64px - 48px);
}

@media (max-width: 640px) {
  .content-min-height {
    min-height: calc(100vh - 64px - 32px);
  }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: css/utilities.css
   Version: v103.0.39
   Batch: B30-3-2
   ═══════════════════════════════════════════ */```

---

## 📄 finance-overview.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>銀行交易 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">銀行交易</h1>
            <p class="page-subtitle" id="finance-month-label">載入中…</p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <div id="finance-stats-root"></div>
        <div id="page-controls-root"></div>

        <div class="tab-bar tab-bar-wrap mb-16" id="finance-tabs">
          <button type="button" class="tab-btn active" data-tab="banks">
            <i data-lucide="landmark" style="width:14px;height:14px;"></i>
            <span>銀行清單</span>
          </button>
          <button type="button" class="tab-btn" data-tab="transactions">
            <i data-lucide="list" style="width:14px;height:14px;"></i>
            <span>交易記錄</span>
          </button>
        </div>

        <div id="finance-panel-banks" class="tab-panel" style="display:block;">
          <div id="finance-banks-root"></div>
        </div>

        <div id="finance-panel-transactions" class="tab-panel" style="display:none;">
          <div id="finance-txn-filter-root" class="mb-16"></div>
          <div id="finance-txn-root"></div>
        </div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/finance-overview.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/finance-overview.js';

    const user = await initApp({ activeHref: 'finance-overview.html', title: '銀行交易' });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: finance-overview.html
     Version: v103.0.29
     Batch: B26-3.2
     ═══════════════════════════════════════════ -->```

---

## 📄 functions/api/[[path]].js

```
// ============================================
// [[path]].js — API Catch-all 路由器（v103.0.45）
// 位置：functions/api/[[path]].js
// ============================================
// v103.0.45（B34-4）：
//   ✅ 加 /api/entity-create 分派（D-063）
// v103.0.44（B30-5）：
//   ✅ 每個 handler 呼叫 initConfig(env) 支援 DB 切換
// ============================================

import { initConfig } from './_config.js';

import {
  handleLookupFamily,
  handleFamilySettingsGet, handleFamilySettingsPost,
  handleFamilyAccountsGet, handleFamilyAccountsPost,
} from './_family.js';

import {
  handleAdminFamiliesGet, handleAdminFamiliesPost,
  handleAdminInitFamily,
  handleAdminMigrate,
} from './_admin.js';

import {
  handleBankAccountsGet, handleBankAccountsPost,
  handleBankTransactionsGet, handleBankTransactionsPost,
  handleClearBankBalances,
} from './_bank.js';

import {
  handlePersonalIncomeGet, handlePersonalIncomePost,
  handleMemberAdvancesGet, handleMemberAdvancesPost,
} from './_personal.js';

import {
  handlePlatformSettingsGet, handlePlatformSettingsPost,
  handlePlatformDefaultsGet, handlePlatformDefaultsPost,
} from './_platform.js';

import {
  handleSummary,
  handleAnnualSummary,
  handleSettlementsYear,
} from './_summary.js';

import { handleInsuranceSync } from './_insurance.js';

/* 🆕 v103.0.45（B34-4）：統一實體 CRUD */
import { handleEntityPost } from './_entity.js';

/* ============================================
   OPTIONS preflight
   ============================================ */
export async function onRequestOptions({ env }) {
  initConfig(env);
  return new Response(null, {
    status: 204,
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, PUT, PATCH, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type, Authorization',
      'Access-Control-Max-Age': '86400',
    },
  });
}

/* ============================================
   GET 分派
   ============================================ */
export async function onRequestGet({ request, env }) {
  initConfig(env);
  const url = new URL(request.url);
  const path = url.pathname;

  if (path === '/api/family-settings') return handleFamilySettingsGet(request);
  if (path === '/api/family-accounts') return handleFamilyAccountsGet(request);

  if (path === '/api/admin-families') return handleAdminFamiliesGet(request);

  if (path === '/api/summary')            return handleSummary(request);
  if (path === '/api/annual-summary')     return handleAnnualSummary(request);
  if (path === '/api/settlements-year')   return handleSettlementsYear(request);

  if (path === '/api/platform-settings')  return handlePlatformSettingsGet(request);
  if (path === '/api/platform-defaults')  return handlePlatformDefaultsGet(request);

  if (path === '/api/bank-accounts')      return handleBankAccountsGet(request);
  if (path === '/api/bank-transactions')  return handleBankTransactionsGet(request);

  if (path === '/api/personal-income')    return handlePersonalIncomeGet(request);
  if (path === '/api/member-advances')    return handleMemberAdvancesGet(request);

  return new Response(JSON.stringify({ ok: false, error: 'NOT_FOUND', message: `未知的 API：${path}` }), {
    status: 404,
    headers: { 'Content-Type': 'application/json' },
  });
}

/* ============================================
   POST 分派
   ============================================ */
export async function onRequestPost({ request, env }) {
  initConfig(env);
  const url = new URL(request.url);
  const path = url.pathname;

  if (path === '/api/lookup-family') return handleLookupFamily(request);

  if (path === '/api/family-settings') return handleFamilySettingsPost(request);
  if (path === '/api/family-accounts') return handleFamilyAccountsPost(request);

  if (path === '/api/admin-families')      return handleAdminFamiliesPost(request);
  if (path === '/api/admin-init-family')   return handleAdminInitFamily(request);
  if (path === '/api/admin-migrate')       return handleAdminMigrate(request);

  if (path === '/api/platform-settings')   return handlePlatformSettingsPost(request);
  if (path === '/api/platform-defaults')   return handlePlatformDefaultsPost(request);

  if (path === '/api/bank-accounts')       return handleBankAccountsPost(request);
  if (path === '/api/bank-transactions')   return handleBankTransactionsPost(request);
  if (path === '/api/clear-bank-balances') return handleClearBankBalances(request);

  if (path === '/api/personal-income')     return handlePersonalIncomePost(request);
  if (path === '/api/member-advances')     return handleMemberAdvancesPost(request);

  if (path === '/api/insurance-sync')      return handleInsuranceSync(request);

  /* 🆕 v103.0.45（B34-4）：統一實體 CRUD（D-063） */
  if (path === '/api/entity-create')       return handleEntityPost(request);

  return new Response(JSON.stringify({ ok: false, error: 'NOT_FOUND', message: `未知的 API：${path}` }), {
    status: 404,
    headers: { 'Content-Type': 'application/json' },
  });
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: functions/api/[[path]].js
   Version: 1791900940
   Batch: B34-4
   Writer: A20261013-004#083
   ═══════════════════════════════════════════ */```

---

## 📄 functions/api/_admin.js

```
// ============================================
// _admin.js — 平台管理 lib（v103.0.22）
// 位置：functions/api/_admin.js
// ============================================
// v103.0.22（B25-6）：
//   ✅ 加 handleAdminMigrate（一次性遷移舊資料）
// ============================================
import { dbGet, dbPut, dbDelete } from './_config.js';
import {
  authenticate, errorResponse, handleError,
  successResponse, requireFields,
} from './_helpers.js';

/* 🆕 v103.0.22：狀態正規化表（與 status-registry.js 對齊） */
const LEGACY_STATUS_MAP = {
  '未處理': 'pending',
  '未還款': 'pending',
  '未付款': 'pending',
  '未扣款': 'pending',
  '未轉入': 'pending',
  '已處理': 'done',
  '已還款': 'done',
  '已付款': 'done',
  '已扣款': 'done',
  '已轉入': 'done',
  '不適用': 'skipped',
};

function _normalizeStatus(raw) {
  if (raw == null || raw === '') return 'pending';
  if (raw === 'pending' || raw === 'done' || raw === 'skipped') return raw;
  return LEGACY_STATUS_MAP[raw] || 'pending';
}

const FALLBACK = {
  members: {
    mem_husband:  { name: '成員1', role: 'husband', order: 0 },
    mem_wife:     { name: '成員2', role: 'wife',    order: 1 },
    mem_son:      { name: '成員3', role: 'child',   order: 2 },
    mem_daughter: { name: '成員4', role: 'child',   order: 3 },
  },
  banks: {},
  insurance_companies: {
    comp_ftlife:     { name: '富通', order: 1 },
    comp_prudential: { name: '保誠', order: 2 },
    comp_fwd:        { name: 'FWD',  order: 3 },
    comp_aia:        { name: 'AIA',  order: 4 },
    comp_manulife:   { name: '宏利', order: 5 },
    comp_axa:        { name: 'AXA',  order: 6 },
  },
  payment_methods: {
    pm_cash:   { name: '現金',   order: 1 },
    pm_boc:    { name: '中銀',   order: 2 },
    pm_hsbc:   { name: '匯豐',   order: 3 },
    pm_hangs:  { name: '恆生',   order: 4 },
    pm_credit: { name: '信用卡', order: 5 },
  },
  expense_categories: {
    cat_medical:   { name: '醫療類',     order: 1 },
    cat_school:    { name: '學校類',     order: 2 },
    cat_insurance: { name: '保險類',     order: 3 },
    cat_fixed:     { name: '固定費用類', order: 4 },
    cat_other:     { name: '其他',       order: 5 },
  },
  expense_items: {
    item_med_01: { categoryKey: 'cat_medical',   name: '看病-一般' },
    item_med_02: { categoryKey: 'cat_medical',   name: '看病-專科' },
    item_med_03: { categoryKey: 'cat_medical',   name: '牙醫' },
    item_med_04: { categoryKey: 'cat_medical',   name: '藥費' },
    item_sch_01: { categoryKey: 'cat_school',    name: '學費' },
    item_sch_02: { categoryKey: 'cat_school',    name: '功課輔導班' },
    item_sch_03: { categoryKey: 'cat_school',    name: '興趣班' },
    item_sch_04: { categoryKey: 'cat_school',    name: '書本費' },
    item_sch_05: { categoryKey: 'cat_school',    name: '校車費' },
    item_ins_01: { categoryKey: 'cat_insurance', name: '住院保險' },
    item_ins_02: { categoryKey: 'cat_insurance', name: '人壽保險' },
    item_ins_03: { categoryKey: 'cat_insurance', name: '意外保險' },
    item_fix_01: { categoryKey: 'cat_fixed',     name: '水費' },
    item_fix_02: { categoryKey: 'cat_fixed',     name: '電費' },
    item_fix_03: { categoryKey: 'cat_fixed',     name: '煤氣費' },
    item_fix_04: { categoryKey: 'cat_fixed',     name: '管理費' },
    item_fix_05: { categoryKey: 'cat_fixed',     name: '房租' },
    item_oth_01: { categoryKey: 'cat_other',     name: '其他' },
  },
  statuses: {
    status_untreated: { name: '未處理', category: 'personal',  isDone: false, order: 1 },
    status_done:      { name: '已處理', category: 'personal',  isDone: true,  order: 2 },
    status_unrepaid:  { name: '未還款', category: 'personal',  isDone: false, order: 3 },
    status_repaid:    { name: '已還款', category: 'personal',  isDone: true,  order: 4 },
    status_unpaid:    { name: '未付款', category: 'fixed',     isDone: false, order: 5 },
    status_paid:      { name: '已付款', category: 'fixed',     isDone: true,  order: 6 },
    status_na:        { name: '不適用', category: 'fixed',     isDone: true,  order: 7 },
    status_unbilled:  { name: '未扣款', category: 'insurance', isDone: false, order: 8 },
    status_billed:    { name: '已扣款', category: 'insurance', isDone: true,  order: 9 },
  },
  options: {
    memberRoles: [
      { value: 'husband', label: '老公 / 丈夫' },
      { value: 'wife',    label: '老婆 / 妻子' },
      { value: 'child',   label: '子女' },
      { value: 'other',   label: '其他' },
    ],
    cycles: [
      { value: '每月',    label: '每月' },
      { value: '每2個月', label: '每2個月' },
      { value: '每季',    label: '每季' },
      { value: '每年',    label: '每年' },
      { value: '一次性',  label: '一次性' },
    ],
    policyTypes: [
      { value: 'normal',         label: '普通保險（住院 / 人壽 / 意外）' },
      { value: 'fund_insurance', label: '基金保險（投資型，月供）' },
    ],
    insurancePaymentTypes: [
      { value: '年繳',     label: '年繳' },
      { value: '月繳',     label: '月繳' },
      { value: '一次付款', label: '一次付款' },
    ],
    categoryOrder: ['銀行類', '醫療類', '學校類', '保險類', '固定費用類', '交通類', '其他'],
  },
  year_range: { startYear: null, futureYears: 5 },
  ui_constants: { nameMaxLenDesktop: 12, nameMaxLenMobile: 6, toastDuration: 2000 },
};

/* ============================================
   Handler 1：admin-families
   ============================================ */
export async function handleAdminFamiliesGet(request) {
  try {
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const list = await dbGet('platform/families', token);
    const families = Object.entries(list || {}).map(([uid, data]) => ({ uid, ...data }));
    families.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0));
    return successResponse({ families });
  } catch (err) {
    return handleError(err);
  }
}

export async function handleAdminFamiliesPost(request) {
  try {
    const body = await request.json();
    const { action, uid, name, email } = body || {};
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (action === 'add') {
      const missing = requireFields(body, ['uid', 'name']);
      if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid 或 name');

      const success = await dbPut(`platform/families/${uid}`, {
        name: String(name).trim(),
        ownerEmail: email ? String(email).trim() : '',
        createdAt: Date.now(),
      }, token);

      if (!success) return errorResponse('INTERNAL', 'Firebase 寫入失敗');
      return successResponse();
    }

    if (action === 'remove') {
      const missing = requireFields(body, ['uid']);
      if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid');

      let accounts = null;
      try { accounts = await dbGet(`platform/families/${uid}/memberAccounts`, token); } catch (e) {}
      const cleanups = [];
      if (accounts && typeof accounts === 'object') {
        Object.entries(accounts).forEach(([memberUid, acc]) => {
          cleanups.push(dbDelete(`platform/uid_index/${memberUid}`, token).catch(() => false));
          if (acc && acc.account) {
            cleanups.push(dbDelete(`platform/email_index/${acc.account}`, token).catch(() => false));
          }
        });
      }
      await Promise.all(cleanups);

      const ok1 = await dbDelete(`platform/families/${uid}`, token);
      const ok2 = await dbDelete(`families/${uid}`, token);
      if (!ok1 && !ok2) return errorResponse('INTERNAL', '刪除失敗');

      return successResponse({
        deletedPlatform: ok1,
        deletedFamilyData: ok2,
        cleanedIndexes: cleanups.length,
      });
    }

    return errorResponse('BAD_REQUEST', '未知的 action');
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 2：admin-init-family
   ============================================ */
export async function handleAdminInitFamily(request) {
  try {
    const body = await request.json();
    const { uid } = body || {};
    const missing = requireFields(body, ['uid']);
    if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid');

    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${uid}`;
    const existing = await dbGet(`${basePath}/members`, token);
    if (existing && Object.keys(existing).length > 0) {
      return successResponse({ skipped: true, message: '此家庭已有資料' });
    }

    const defaults = await _loadPlatformDefaults(token);
    const now = Date.now();
    const results = {
      members: false, banks: false, insurance_companies: false,
      payment_methods: false, expense_categories: false, expense_items: false,
      statuses: false, options: false, year_range: false, ui_constants: false,
    };

    const membersData = _withTimestamps(defaults.members, now);
    results.members = Object.keys(membersData).length > 0
      ? await dbPut(`${basePath}/members`, membersData, token) : true;

    results.banks = Object.keys(defaults.banks).length > 0
      ? await dbPut(`${basePath}/banks`, _withTimestamps(defaults.banks, now), token) : true;
    results.insurance_companies = Object.keys(defaults.insurance_companies).length > 0
      ? await dbPut(`${basePath}/insurance_companies`, _withTimestamps(defaults.insurance_companies, now), token) : true;
    results.payment_methods = Object.keys(defaults.payment_methods).length > 0
      ? await dbPut(`${basePath}/payment_methods`, _withTimestamps(defaults.payment_methods, now), token) : true;
    results.expense_categories = Object.keys(defaults.expense_categories).length > 0
      ? await dbPut(`${basePath}/expense_categories`, _withTimestamps(defaults.expense_categories, now), token) : true;

    const itemsData = _buildItemsWithCategoryId(defaults.expense_items, now);
    results.expense_items = Object.keys(itemsData).length > 0
      ? await dbPut(`${basePath}/expense_items`, itemsData, token) : true;

    results.statuses = Object.keys(defaults.statuses).length > 0
      ? await dbPut(`${basePath}/statuses`, _withTimestamps(defaults.statuses, now), token) : true;

    results.options = (defaults.options && Object.keys(defaults.options).length > 0)
      ? await dbPut(`${basePath}/settings/options`, defaults.options, token) : true;
    results.year_range = defaults.year_range
      ? await dbPut(`${basePath}/settings/year_range`, defaults.year_range, token) : true;
    results.ui_constants = defaults.ui_constants
      ? await dbPut(`${basePath}/settings/ui_constants`, defaults.ui_constants, token) : true;

    const failedSteps = Object.entries(results).filter(([, ok]) => ok === false).map(([k]) => k);
    if (failedSteps.length > 0) {
      return successResponse({ created: true, partial: true, failedSteps, results });
    }
    return successResponse({ created: true, results });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 3：admin-migrate（🆕 v103.0.22）
   ============================================ */
export async function handleAdminMigrate(request) {
  try {
    const body = await request.json();
    const { familyId, confirm } = body || {};
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (confirm !== 'MIGRATE_ALL') {
      return errorResponse('BAD_REQUEST', '必須提供確認參數 confirm="MIGRATE_ALL"');
    }

    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}`;
    const summary = {
      expenses:    { scanned: 0, normalized: 0 },
      payments:    { scanned: 0, normalized: 0 },
      policies:    { scanned: 0, patched: 0 },
      funds:       { scanned: 0, patched: 0 },
    };

    /* ---------- 1. 遷移 expenses 狀態 ---------- */
    try {
      const expensesSnap = await dbGet(`${basePath}/expenses`, token);
      const expensesUpd = {};
      if (expensesSnap && typeof expensesSnap === 'object') {
        Object.entries(expensesSnap).forEach(([y, months]) => {
          Object.entries(months || {}).forEach(([m, monthData]) => {
            Object.entries(monthData?.member_expenses || {}).forEach(([mid, items]) => {
              Object.entries(items || {}).forEach(([eid, e]) => {
                if (!e || typeof e !== 'object') return;
                summary.expenses.scanned++;

                const patch = {};
                const newStatus = _normalizeStatus(e.status);
                if (e.status !== newStatus) {
                  patch.status = newStatus;
                }
                if (e.isAutoLinked === undefined) {
                  patch.isAutoLinked = !!e.policyId;
                }
                if (Object.keys(patch).length > 0) {
                  expensesUpd[`${basePath}/expenses/${y}/${m}/member_expenses/${mid}/${eid}`] = { ...e, ...patch };
                  summary.expenses.normalized++;
                }
              });
            });
          });
        });
        if (Object.keys(expensesUpd).length > 0) {
          const ok = await dbPut(`${basePath}/_migrate_check_expenses`, true, token);
          // 使用 PATCH 混合更新：分批
          for (const [path, data] of Object.entries(expensesUpd)) {
            await dbPut(path, data, token);
          }
          await dbDelete(`${basePath}/_migrate_check_expenses`, token);
        }
      }
    } catch (err) {
      console.warn('[admin-migrate] expenses 遷移失敗：', err);
    }

    /* ---------- 2. 遷移 insurance_payments 狀態 ---------- */
    try {
      const paymentsSnap = await dbGet(`${basePath}/insurance_payments`, token);
      if (paymentsSnap && typeof paymentsSnap === 'object') {
        for (const [pid, years] of Object.entries(paymentsSnap)) {
          for (const [y, months] of Object.entries(years || {})) {
            for (const [m, p] of Object.entries(months || {})) {
              if (!p || typeof p !== 'object') continue;
              summary.payments.scanned++;
              const newStatus = _normalizeStatus(p.status);
              if (p.status !== newStatus) {
                await dbPut(`${basePath}/insurance_payments/${pid}/${y}/${m}`, { ...p, status: newStatus }, token);
                summary.payments.normalized++;
              }
            }
          }
        }
      }
    } catch (err) {
      console.warn('[admin-migrate] payments 遷移失敗：', err);
    }

    /* ---------- 3. 遷移 insurance_policies ---------- */
    try {
      const policiesSnap = await dbGet(`${basePath}/insurance_policies`, token);
      if (policiesSnap && typeof policiesSnap === 'object') {
        for (const [pid, p] of Object.entries(policiesSnap)) {
          if (!p || typeof p !== 'object') continue;
          summary.policies.scanned++;
          const patch = {};
          if (!Array.isArray(p.fundsAllocation)) patch.fundsAllocation = [];
          if (p.type === undefined) patch.type = 'normal';
          if (Object.keys(patch).length > 0) {
            await dbPut(`${basePath}/insurance_policies/${pid}`, { ...p, ...patch }, token);
            summary.policies.patched++;
          }
        }
      }
    } catch (err) {
      console.warn('[admin-migrate] policies 遷移失敗：', err);
    }

    /* ---------- 4. 遷移 funds ---------- */
    try {
      const fundsSnap = await dbGet(`${basePath}/funds`, token);
      if (fundsSnap && typeof fundsSnap === 'object') {
        for (const [fid, f] of Object.entries(fundsSnap)) {
          if (!f || typeof f !== 'object') continue;
          summary.funds.scanned++;
          const patch = {};
          if (f.type === undefined) patch.type = 'standalone';
          if (f.policyId === undefined) patch.policyId = '';
          if (f.initialYear === undefined) patch.initialYear = '';
          if (f.initialMonth === undefined) patch.initialMonth = '';
          if (Object.keys(patch).length > 0) {
            await dbPut(`${basePath}/funds/${fid}`, { ...f, ...patch }, token);
            summary.funds.patched++;
          }
        }
      }
    } catch (err) {
      console.warn('[admin-migrate] funds 遷移失敗：', err);
    }

    return successResponse({ migrated: true, familyId, summary });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   內部工具
   ============================================ */
async function _loadPlatformDefaults(token) {
  const [members, banks, companies, payments, categories, items, statuses, options, yearRange, uiConstants] =
    await Promise.all([
      dbGet('platform/defaults/members', token),
      dbGet('platform/defaults/banks', token),
      dbGet('platform/defaults/insurance_companies', token),
      dbGet('platform/defaults/payment_methods', token),
      dbGet('platform/defaults/expense_categories', token),
      dbGet('platform/defaults/expense_items', token),
      dbGet('platform/defaults/statuses', token),
      dbGet('platform/defaults/options', token),
      dbGet('platform/defaults/year_range', token),
      dbGet('platform/defaults/ui_constants', token),
    ]);

  return {
    members:             members    && Object.keys(members).length    ? members    : FALLBACK.members,
    banks:               banks      && Object.keys(banks).length      ? banks      : FALLBACK.banks,
    insurance_companies: companies  && Object.keys(companies).length  ? companies  : FALLBACK.insurance_companies,
    payment_methods:     payments   && Object.keys(payments).length   ? payments   : FALLBACK.payment_methods,
    expense_categories:  categories && Object.keys(categories).length ? categories : FALLBACK.expense_categories,
    expense_items:       items      && Object.keys(items).length      ? items      : FALLBACK.expense_items,
    statuses:            statuses   && Object.keys(statuses).length   ? statuses   : FALLBACK.statuses,
    options:             options    && Object.keys(options).length    ? options    : FALLBACK.options,
    year_range:          yearRange  || FALLBACK.year_range,
    ui_constants:        uiConstants || FALLBACK.ui_constants,
  };
}

function _withTimestamps(data, baseTs) {
  const out = {};
  let i = 0;
  Object.entries(data || {}).forEach(([key, val]) => {
    out[key] = { ...val, createdAt: val.createdAt || (baseTs + i) };
    i++;
  });
  return out;
}

function _buildItemsWithCategoryId(items, baseTs) {
  const out = {};
  let i = 0;
  Object.entries(items || {}).forEach(([key, val]) => {
    const categoryId = val.categoryKey || val.categoryId || '';
    out[key] = {
      name: val.name || '',
      categoryId,
      createdAt: val.createdAt || (baseTs + i),
    };
    i++;
  });
  return out;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: functions/api/_admin.js
   Version: v103.0.22
   Batch: B25-6
   ═══════════════════════════════════════════ */```

---

## 📄 functions/api/_bank.js

```
// ============================================
// _bank.js — 銀行 lib（v103.0.2）
// 位置：functions/api/_bank.js
// ============================================
import { dbGet, dbPut, dbPush, dbDelete } from './_config.js';
import {
  authenticate, errorResponse, handleError,
  successResponse, requireFields, objToList, roundInt,
} from './_helpers.js';

const VALID_TYPES = ['in', 'out', 'transfer'];
const VALID_CATEGORIES = ['contribution', 'expense', 'insurance', 'reimbursement', 'manual'];
const CONFIRM_TOKEN = 'CONFIRM_DELETE';

/* ============================================
   Handler 1：bank-accounts
   ============================================ */
export async function handleBankAccountsGet(request) {
  try {
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const auth = await authenticate(request, { needFamily: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const data = await dbGet(`families/${familyId}/bank_accounts`, token);
    const list = objToList(data || {}, (a, b) => {
      const oa = a.order != null ? a.order : Number.MAX_SAFE_INTEGER;
      const ob = b.order != null ? b.order : Number.MAX_SAFE_INTEGER;
      if (oa !== ob) return oa - ob;
      return (a.createdAt || 0) - (b.createdAt || 0);
    });
    return successResponse({ familyId, accounts: list, count: list.length });
  } catch (err) {
    return handleError(err);
  }
}

export async function handleBankAccountsPost(request) {
  try {
    const body = await request.json();
    const { familyId, action, id, data } = body || {};
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (!action) return errorResponse('MISSING_FIELDS', '缺少 action');

    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const familySnap = await dbGet(`platform/families/${familyId}`, token);
    if (!familySnap) return errorResponse('NOT_FOUND', `找不到家庭 ${familyId}`);

    const basePath = `families/${familyId}/bank_accounts`;

    if (action === 'create') {
      if (!data || typeof data !== 'object') return errorResponse('MISSING_FIELDS', '缺少 data');
      const missing = requireFields(data, ['name']);
      if (missing) return errorResponse('MISSING_FIELDS', '缺少 name');

      const clean = {
        name: String(data.name).trim(),
        type: ['family', 'personal'].includes(data.type) ? data.type : 'family',
        initialBalance: roundInt(data.initialBalance),
        initialYear: String(data.initialYear || new Date().getFullYear()),
        initialMonth: String(data.initialMonth || '01').padStart(2, '0'),
        order: Number(data.order) || 0,
        createdAt: Date.now(),
      };
      const newId = await dbPush(basePath, clean, token);
      if (!newId) return errorResponse('INTERNAL', '寫入失敗');
      return successResponse({ id: newId, data: clean });
    }

    if (action === 'update') {
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');
      if (!data || typeof data !== 'object') return errorResponse('MISSING_FIELDS', '缺少 data');

      const existing = await dbGet(`${basePath}/${id}`, token);
      if (!existing) return errorResponse('NOT_FOUND', '找不到此銀行帳號');

      const clean = {};
      if (data.name !== undefined) clean.name = String(data.name).trim();
      if (data.type !== undefined && ['family', 'personal'].includes(data.type)) clean.type = data.type;
      if (data.initialBalance !== undefined) clean.initialBalance = roundInt(data.initialBalance);
      if (data.initialYear !== undefined) clean.initialYear = String(data.initialYear);
      if (data.initialMonth !== undefined) clean.initialMonth = String(data.initialMonth).padStart(2, '0');
      if (data.order !== undefined) clean.order = Number(data.order) || 0;

      if (Object.keys(clean).length === 0) return errorResponse('BAD_REQUEST', '沒有任何欄位要更新');
      clean.updatedAt = Date.now();

      const ok = await dbPut(`${basePath}/${id}`, { ...existing, ...clean }, token);
      if (!ok) return errorResponse('INTERNAL', '更新失敗');
      return successResponse({ id, data: { ...existing, ...clean } });
    }

    if (action === 'remove') {
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');
      const existing = await dbGet(`${basePath}/${id}`, token);
      if (!existing) return errorResponse('NOT_FOUND', '找不到此銀行帳號');

      const txns = await dbGet(`${basePath}/${id}/transactions`, token);
      const txnCount = txns ? Object.keys(txns).length : 0;

      const refCount = await _countBankReferences(basePath, id, token);
      if (refCount > 0) {
        return errorResponse('CONFLICT',
          `此銀行帳號仍被 ${refCount} 筆記錄引用（支出 / 保險扣款），請先移除引用或改用「強制刪除」。`, 409);
      }

      const ok = await dbDelete(`${basePath}/${id}`, token);
      if (!ok) return errorResponse('INTERNAL', '刪除失敗');
      return successResponse({ removedId: id, removedTxnCount: txnCount });
    }

    return errorResponse('BAD_REQUEST', `未知的 action：${action}`);
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 2：bank-transactions
   ============================================ */
export async function handleBankTransactionsGet(request) {
  try {
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const bankId = url.searchParams.get('bankId');
    const action = url.searchParams.get('action') || 'list';
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const auth = await authenticate(request, { needFamily: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (action === 'listAll') {
      const allBanks = await dbGet(`families/${familyId}/bank_accounts`, token);
      const flat = [];
      for (const [bid, bankData] of Object.entries(allBanks || {})) {
        Object.entries(bankData.transactions || {}).forEach(([txnId, txn]) => {
          flat.push({ id: txnId, bankId: bid, bankName: bankData.name || '', ...txn });
        });
      }
      flat.sort((a, b) => (a.createdAt || 0) - (b.createdAt || 0));
      return successResponse({ familyId, transactions: flat, count: flat.length });
    }

    if (!bankId) return errorResponse('MISSING_FIELDS', '缺少 bankId');
    const data = await dbGet(`families/${familyId}/bank_accounts/${bankId}/transactions`, token);
    const list = objToList(data || {}, (a, b) => (a.createdAt || 0) - (b.createdAt || 0));
    return successResponse({ familyId, bankId, transactions: list, count: list.length });
  } catch (err) {
    return handleError(err);
  }
}

export async function handleBankTransactionsPost(request) {
  try {
    const body = await request.json();
    const { familyId, bankId, action, txnId, data } = body || {};
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (!bankId) return errorResponse('MISSING_FIELDS', '缺少 bankId');
    if (!action) return errorResponse('MISSING_FIELDS', '缺少 action');

    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const bankSnap = await dbGet(`families/${familyId}/bank_accounts/${bankId}`, token);
    if (!bankSnap) return errorResponse('NOT_FOUND', `找不到銀行帳號 ${bankId}`);

    const basePath = `families/${familyId}/bank_accounts/${bankId}/transactions`;

    if (action === 'create') {
      if (!data || typeof data !== 'object') return errorResponse('MISSING_FIELDS', '缺少 data');
      const missing = requireFields(data, ['type', 'amount']);
      if (missing) return errorResponse('MISSING_FIELDS', '缺少 type 或 amount');
      if (!VALID_TYPES.includes(data.type)) return errorResponse('BAD_REQUEST', `無效的 type：${data.type}`);

      const clean = {
        type: data.type,
        category: VALID_CATEGORIES.includes(data.category) ? data.category : 'manual',
        amount: roundInt(data.amount),
        date: data.date || new Date().toISOString().slice(0, 10),
        memberId: data.memberId || '',
        refId: data.refId || '', note: data.note || '',
        createdAt: Date.now(),
      };
      const newId = await dbPush(basePath, clean, token);
      if (!newId) return errorResponse('INTERNAL', '寫入失敗');
      return successResponse({ id: newId, data: clean });
    }

    if (action === 'update') {
      if (!txnId) return errorResponse('MISSING_FIELDS', '缺少 txnId');
      const existing = await dbGet(`${basePath}/${txnId}`, token);
      if (!existing) return errorResponse('NOT_FOUND', '找不到此交易');

      const clean = {};
      if (data.type !== undefined) {
        if (!VALID_TYPES.includes(data.type)) return errorResponse('BAD_REQUEST', `無效的 type：${data.type}`);
        clean.type = data.type;
      }
      if (data.category !== undefined) clean.category = VALID_CATEGORIES.includes(data.category) ? data.category : 'manual';
      if (data.amount !== undefined) clean.amount = roundInt(data.amount);
      if (data.date !== undefined) clean.date = String(data.date);
      if (data.memberId !== undefined) clean.memberId = String(data.memberId);
      if (data.refId !== undefined) clean.refId = String(data.refId);
      if (data.note !== undefined) clean.note = String(data.note);

      if (Object.keys(clean).length === 0) return errorResponse('BAD_REQUEST', '沒有任何欄位要更新');
      clean.updatedAt = Date.now();

      const ok = await dbPut(`${basePath}/${txnId}`, { ...existing, ...clean }, token);
      if (!ok) return errorResponse('INTERNAL', '更新失敗');
      return successResponse({ txnId, data: { ...existing, ...clean } });
    }

    if (action === 'remove') {
      if (!txnId) return errorResponse('MISSING_FIELDS', '缺少 txnId');
      const existing = await dbGet(`${basePath}/${txnId}`, token);
      if (!existing) return errorResponse('NOT_FOUND', '找不到此交易');

      let cleanedSource = null;
      try { cleanedSource = await _cleanupSourceRef({ familyId, bankId, txnId, txn: existing, token }); } catch (e) {}

      const ok = await dbDelete(`${basePath}/${txnId}`, token);
      if (!ok) return errorResponse('INTERNAL', '刪除失敗');
      return successResponse({ removedId: txnId, cleanedSource });
    }

    return errorResponse('BAD_REQUEST', `未知的 action：${action}`);
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 3：clear-bank-balances
   ============================================ */
export async function handleClearBankBalances(request) {
  try {
    const body = await request.json();
    const { familyId, confirm } = body || {};
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (confirm !== CONFIRM_TOKEN) {
      return errorResponse('BAD_REQUEST', '必須提供確認參數 confirm="CONFIRM_DELETE"');
    }

    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token, isSuper, isOwner } = auth;

    if (!isSuper && !isOwner) {
      return errorResponse('FORBIDDEN', '只有家庭擁有者才能執行此操作');
    }

    const existing = await dbGet(`families/${familyId}/bank_balances`, token);
    if (!existing) return successResponse({ cleared: false, message: '無舊銀行結餘資料' });

    let recordCount = 0;
    Object.values(existing).forEach((months) => {
      Object.values(months || {}).forEach((banks) => {
        recordCount += Object.keys(banks || {}).length;
      });
    });

    const ok = await dbDelete(`families/${familyId}/bank_balances`, token);
    if (!ok) return errorResponse('INTERNAL', '刪除失敗');
    return successResponse({
      cleared: true, recordCount,
      message: `已清除 ${recordCount} 筆舊銀行結餘紀錄`,
    });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   內部工具
   ============================================ */
async function _countBankReferences(bankAccountsBasePath, bankId, token) {
  const familyBase = bankAccountsBasePath.replace(/\/bank_accounts$/, '');
  let count = 0;

  try {
    const expensesSnap = await dbGet(`${familyBase}/expenses`, token);
    if (expensesSnap && typeof expensesSnap === 'object') {
      Object.values(expensesSnap).forEach((months) => {
        Object.values(months || {}).forEach((monthData) => {
          Object.values(monthData?.member_expenses || {}).forEach((items) => {
            Object.values(items || {}).forEach((e) => {
              if (e && e.bankId === bankId) count++;
            });
          });
        });
      });
    }
  } catch (err) { /* noop */ }

  try {
    const paymentsSnap = await dbGet(`${familyBase}/insurance_payments`, token);
    if (paymentsSnap && typeof paymentsSnap === 'object') {
      Object.values(paymentsSnap).forEach((years) => {
        Object.values(years || {}).forEach((months) => {
          Object.values(months || {}).forEach((p) => {
            if (p && p.bankId === bankId) count++;
          });
        });
      });
    }
  } catch (err) { /* noop */ }

  return count;
}

async function _cleanupSourceRef({ familyId, bankId, txnId, txn, token }) {
  const { category, refId, memberId, date } = txn;
  const base = `families/${familyId}`;

  if (category === 'expense' && refId && memberId && date) {
    const [y, m] = (date || '').split('-');
    if (y && m) {
      const mm = String(m).padStart(2, '0');
      const path = `${base}/expenses/${y}/${mm}/member_expenses/${memberId}/${refId}`;
      const src = await dbGet(path, token);
      if (src && src.txnId === txnId) {
        await dbPut(path, { ...src, txnId: '' }, token);
        return { type: 'expense', path, clearedTxnId: true };
      }
    }
  }

  if ((category === 'insurance' || category === 'reimbursement') && refId && date) {
    const [y, m] = (date || '').split('-');
    if (y && m) {
      const mm = String(m).padStart(2, '0');
      const path = `${base}/insurance_payments/${refId}/${y}/${mm}`;
      const src = await dbGet(path, token);
      if (src && src.txnId === txnId) {
        await dbPut(path, { ...src, txnId: '' }, token);
        return { type: 'insurance_payment', path, clearedTxnId: true };
      }
    }
  }

  if (category === 'contribution' && date) {
    return { type: 'contribution', note: '來源為 income 節點，無需清理 refId' };
  }

  return null;
}```

---

## 📄 functions/api/_config.js

```
// ============================================
// _config.js — Firebase REST 設定（v103.0.45）
// 位置：functions/api/_config.js
// ============================================
// v103.0.45（B30-5）：
//   ✅ 保留 FIREBASE_DB_URL / FIREBASE_API_KEY 的 let 匯出（相容舊引用）
//   ✅ initConfig(env) 動態更新
// ============================================

/* ============================================
   DB 目標（由 initConfig 設定）
   ============================================ */
const _DB_CONFIGS = {
  old: {
    url:    'https://family-fin-a6dd1-default-rtdb.asia-southeast1.firebasedatabase.app',
    apiKey: 'AIzaSyCQlrNdorKJI9xsqr4m4ME046lrubo9Y7I',
  },
  new: {
    url:    'https://family-3d0a5-default-rtdb.asia-southeast1.firebasedatabase.app',
    apiKey: 'AIzaSyBJCK83RQyHbol4yJ4UDo3pOUXpUgcW9hU',
  },
};

/* ⚠️ 用 export let（live binding），讓舊引用自動更新 */
export let FIREBASE_DB_URL  = _DB_CONFIGS.old.url;
export let FIREBASE_API_KEY = _DB_CONFIGS.old.apiKey;

export const SUPERADMIN_EMAIL = 'superadmin@familyfin.local';
export const IDENTITY_TOOLKIT_URL = 'https://identitytoolkit.googleapis.com/v1';

/* ============================================
   initConfig（每個 request 開頭呼叫）
   ============================================ */
export function initConfig(env) {
  const target = (env && env.DB_TARGET) || 'old';
  const cfg = _DB_CONFIGS[target] || _DB_CONFIGS.old;
  FIREBASE_DB_URL  = cfg.url;
  FIREBASE_API_KEY = cfg.apiKey;
}

/* 輔助取值（給 _helpers.js 用） */
export function getApiKey() { return FIREBASE_API_KEY; }
export function getDbUrl()  { return FIREBASE_DB_URL; }

/* ============================================
   REST 基本操作
   ============================================ */

export async function dbGet(path, token = '') {
  const url = `${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`;
  const res = await fetch(url);
  if (!res.ok) return null;
  return res.json();
}

export async function dbPut(path, data, token = '') {
  const url = `${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`;
  const res = await fetch(url, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data),
  });
  return res.ok;
}

export async function dbPatch(path, data, token = '') {
  const url = `${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`;
  const res = await fetch(url, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data),
  });
  return res.ok;
}

export async function dbPush(path, data, token = '') {
  const url = `${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`;
  const res = await fetch(url, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data),
  });
  if (!res.ok) return null;
  const result = await res.json();
  return result?.name || null;
}

export async function dbDelete(path, token = '') {
  const url = `${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`;
  const res = await fetch(url, { method: 'DELETE' });
  return res.ok;
}

/* ============================================
   Firebase Auth 操作
   ============================================ */

export async function dbSignUp(email, password) {
  try {
    const url = `${IDENTITY_TOOLKIT_URL}/accounts:signUp?key=${FIREBASE_API_KEY}`;
    const res = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password, returnSecureToken: false }),
    });
    const data = await res.json();
    if (!res.ok) {
      return { ok: false, error: data?.error?.message || 'UNKNOWN_ERROR' };
    }
    return { ok: true, uid: data.localId, email: data.email };
  } catch (err) {
    return { ok: false, error: String(err?.message || err) };
  }
}

export async function dbEmailExists(email) {
  return false;
}

/* ============================================
   回應工具
   ============================================ */

export function jsonResponse(data, status = 200) {
  return new Response(JSON.stringify(data), {
    status,
    headers: {
      'Content-Type': 'application/json',
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, PUT, PATCH, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type, Authorization',
      'Access-Control-Max-Age': '86400',
    },
  });
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: functions/api/_config.js
   Version: v103.0.45
   Batch: B30-5
   ═══════════════════════════════════════════ */```

---

## 📄 functions/api/_entity.js

```
// ============================================
// _entity.js — 統一實體 CRUD 端點（v1.0.2）
// 位置：functions/api/_entity.js
// ============================================
// v1.0.2（B34-4）：
//   ✅ 改用 REST 函式（dbGet / dbPush / dbPut / dbPatch / dbDelete）
//   ✅ 修正前一版誤用 db.ref(...) 的問題
// v1.0.1（B34-4）：
//   ✅ 修正 verifyFamilyAccess 呼叫方式
// v1.0.0（B34-4）：初版
// ============================================

import {
  dbGet,
  dbPut,
  dbPatch,
  dbPush,
  dbDelete,
} from './_config.js';

import {
  verifyFamilyAccess,
  extractToken,
  errorResponse,
  successResponse,
  handleError,
} from './_helpers.js';

/* ============================================
   entityType → Firebase 路徑
   ============================================ */
const ENTITY_PATH = {
  member:      'members',
  bank:        'banks',
  bankAccount: 'bank_accounts',
  category:    'expense_categories',
  item:        'expense_items',
  payment:     'payment_methods',
  company:     'insurance_companies',
  policy:      'insurance_policies',
  fund:        'funds',
};

/* ============================================
   entityType → 必填 extra 欄位
   ============================================ */
const REQUIRED_EXTRA = {
  member:      [],
  bank:        [],
  bankAccount: ['bankId', 'type', 'ownerType'],
  category:    ['shortCode'],
  item:        ['categoryId'],
  payment:     [],
  company:     [],
  policy:      ['type', 'memberId', 'firstStartYear', 'firstStartMonth', 'totalPolicyYears'],
  fund:        ['type'],
};

/* ============================================
   主入口
   ============================================ */
export async function handleEntityPost(request) {
  try {
    const body = await request.json();
    const { familyId, action, entityType, id, code, name, extra } = body;

    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    /* 驗證登入 + 權限 */
    const token = extractToken(request);
    const access = await verifyFamilyAccess(token, familyId);
    if (!access) return errorResponse('FORBIDDEN', '無權存取此家庭');

    if (!entityType || !ENTITY_PATH[entityType]) {
      return errorResponse('INVALID_BODY', `未知的 entityType：${entityType}`);
    }

    switch (action) {
      case 'create': return await _create(familyId, entityType, { code, name, extra }, token);
      case 'update': return await _update(familyId, entityType, id, { name, extra }, token);
      case 'delete': return await _delete(familyId, entityType, id, token);
      default:
        return errorResponse('INVALID_BODY', `未知的 action：${action}`);
    }
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Create
   ============================================ */
async function _create(familyId, entityType, { code, name, extra = {} }, token) {
  const required = REQUIRED_EXTRA[entityType] || [];
  const missing = required.filter((k) => extra[k] === undefined || extra[k] === '');
  if (missing.length > 0) {
    return errorResponse('MISSING_FIELDS', `缺少欄位：${missing.join(', ')}`);
  }
  if (!name) return errorResponse('MISSING_FIELDS', '缺少 name');
  if (!code) return errorResponse('MISSING_FIELDS', '缺少 code');

  const path = ENTITY_PATH[entityType];
  const listPath = `families/${familyId}/${path}`;

  /* 讀取現有列表（REST） */
  const list = (await dbGet(listPath, token)) || {};

  /* 重複檢查 */
  const codeDup = Object.entries(list).find(([_, v]) => v.code === code);
  if (codeDup) return errorResponse('CODE_DUPLICATE', `編號已存在：${code}`, 409);

  const nameDup = Object.entries(list).find(([_, v]) => v.name === name);
  if (nameDup) return errorResponse('NAME_DUPLICATE', `名稱重複：${name}`, 409);

  /* 建立（REST push） */
  const payload = {
    code,
    name,
    ..._normalizeExtra(entityType, extra),
    createdAt: Date.now(),
  };
  const newId = await dbPush(listPath, payload, token);
  if (!newId) return errorResponse('INTERNAL', '寫入失敗');

  return successResponse({ id: newId, code });
}

/* ============================================
   Update
   ============================================ */
async function _update(familyId, entityType, id, { name, extra = {} }, token) {
  if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');

  const path = ENTITY_PATH[entityType];
  const itemPath = `families/${familyId}/${path}/${id}`;

  /* 檢查存在（REST） */
  const existing = await dbGet(itemPath, token);
  if (!existing) return errorResponse('NOT_FOUND', `找不到：${id}`);

  /* 名稱重複檢查 */
  if (name) {
    const list = (await dbGet(`families/${familyId}/${path}`, token)) || {};
    const nameDup = Object.entries(list).find(([k, v]) => k !== id && v.name === name);
    if (nameDup) return errorResponse('NAME_DUPLICATE', `名稱重複：${name}`, 409);
  }

  /* 更新（REST patch） */
  const payload = {
    ...(name !== undefined ? { name } : {}),
    ...(Object.keys(extra).length > 0 ? _normalizeExtra(entityType, extra) : {}),
    updatedAt: Date.now(),
  };
  const ok = await dbPatch(itemPath, payload, token);
  if (!ok) return errorResponse('INTERNAL', '更新失敗');

  return successResponse({ id });
}

/* ============================================
   Delete
   ============================================ */
async function _delete(familyId, entityType, id, token) {
  if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');

  const path = ENTITY_PATH[entityType];
  const itemPath = `families/${familyId}/${path}/${id}`;

  const existing = await dbGet(itemPath, token);
  if (!existing) return errorResponse('NOT_FOUND', `找不到：${id}`);

  const ok = await dbDelete(itemPath, token);
  if (!ok) return errorResponse('INTERNAL', '刪除失敗');

  return successResponse({ id });
}

/* ============================================
   標準化 extra
   ============================================ */
function _normalizeExtra(entityType, extra) {
  const round = (v) => Math.round(Number(v) || 0);
  switch (entityType) {
    case 'member':
      return { role: extra.role || 'other', order: Number(extra.order) || 0 };
    case 'bank':
      return { order: Number(extra.order) || 0 };
    case 'bankAccount':
      return {
        type: extra.type,
        ownerType: extra.ownerType,
        ownerId: extra.ownerId || 'family',
        initialBalance: round(extra.initialBalance),
        initialYear: String(extra.initialYear || ''),
        initialMonth: String(extra.initialMonth || '01').padStart(2, '0'),
        order: Number(extra.order) || 0,
      };
    case 'category':
      return { shortCode: extra.shortCode, order: Number(extra.order) || 0 };
    case 'item':
      return { categoryId: extra.categoryId };
    case 'payment':
      return { order: Number(extra.order) || 0 };
    case 'company':
      return {};
    case 'policy':
      return {
        type: extra.type,
        memberId: extra.memberId,
        policyHolderId: extra.policyHolderId || extra.memberId,
        firstStartYear: Number(extra.firstStartYear),
        firstStartMonth: String(extra.firstStartMonth).padStart(2, '0'),
        totalPolicyYears: Number(extra.totalPolicyYears),
        totalPolicyPeriods: Number(extra.totalPolicyYears) * 12,
        monthlyPremium: round(extra.monthlyPremium),
        annualPremium: round(extra.annualPremium),
        fundsAllocation: Array.isArray(extra.fundsAllocation) ? extra.fundsAllocation : [],
      };
    case 'fund':
      return {
        type: extra.type,
        policyId: extra.policyId || '',
        cost: round(extra.cost),
        initialYear: String(extra.initialYear || ''),
        initialMonth: String(extra.initialMonth || '').padStart(2, '0'),
      };
    default:
      return {};
  }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: functions/api/_entity.js
   Version: 1791901060
   Batch: B34-4
   Writer: A20261013-004#087
   ═══════════════════════════════════════════ */```

---

## 📄 functions/api/_family.js

```
// ============================================
// _family.js — 家庭相關 lib（v103.0.2）
// 位置：functions/api/_family.js
// ============================================
// 說明：
//   - `_` 前綴 → CF Pages 不當路由，僅作為共用 lib
//   - 導出 3 個具名 handler，由 [[path]].js 呼叫
// ============================================

import {
  dbGet, dbPut, dbPush, dbDelete, dbSignUp,
  FIREBASE_API_KEY, IDENTITY_TOOLKIT_URL,
} from './_config.js';
import {
  authenticate, errorResponse, handleError,
  successResponse, requireFields, objToList,
  verifySuperAdmin, verifyFamilyAccess,
  verifyFamilyAccessByUid, extractToken,
} from './_helpers.js';

/* ============================================
   Handler 1：lookup-family
   ============================================ */
export async function handleLookupFamily(request) {
  try {
    const auth = await authenticate(request);
    if (auth instanceof Response) return auth;
    const { user, token } = auth;

    const result = await verifyFamilyAccessByUid(user.localId, token);
    if (!result) return errorResponse('NOT_FOUND', '此帳號不屬於任何家庭，請聯繫管理員');

    let familyName = '我的家庭';
    try {
      const snap = await dbGet(`platform/families/${result.familyId}`, token);
      if (snap?.name) familyName = snap.name;
    } catch (e) { /* noop */ }

    return successResponse({
      familyId: result.familyId,
      familyName,
      memberAccount: result.memberAccount || {
        email: user.email || '',
        displayName: '成員',
        role: 'member',
        canInput: true,
      },
      isLegacy: result.isLegacy,
    });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 2：family-settings
   ============================================ */
export async function handleFamilySettingsGet(request) {
  try {
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const auth = await authenticate(request, { needFamily: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}`;
    const [statuses, options, yearRange, uiConstants] = await Promise.all([
      dbGet(`${basePath}/statuses`, token),
      dbGet(`${basePath}/settings/options`, token),
      dbGet(`${basePath}/settings/year_range`, token),
      dbGet(`${basePath}/settings/ui_constants`, token),
    ]);

    return successResponse({
      familyId,
      statuses: objToList(statuses || {}, _byOrder),
      options: options || null,
      yearRange: yearRange || null,
      uiConstants: uiConstants || null,
    });
  } catch (err) {
    return handleError(err);
  }
}

export async function handleFamilySettingsPost(request) {
  try {
    const body = await request.json();
    const { familyId, action, id, data } = body || {};
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token } = auth;
    const basePath = `families/${familyId}`;

    if (action === 'update') {
      if (!data || typeof data !== 'object') return errorResponse('MISSING_FIELDS', '缺少 data');
      const results = {};
      if (data.options !== undefined) {
        results.options = await dbPut(`${basePath}/settings/options`, _sanitizeOptions(data.options), token);
      }
      if (data.yearRange !== undefined) {
        results.yearRange = await dbPut(`${basePath}/settings/year_range`, _sanitizeYearRange(data.yearRange), token);
      }
      if (data.uiConstants !== undefined) {
        results.uiConstants = await dbPut(`${basePath}/settings/ui_constants`, _sanitizeUIConstants(data.uiConstants), token);
      }
      if (Object.values(results).some((v) => v === false)) return errorResponse('INTERNAL', '部分寫入失敗');
      return successResponse({ results });
    }

    if (action === 'put-status') {
      if (!data || typeof data !== 'object') return errorResponse('MISSING_FIELDS', '缺少 data');
      if (!data.name) return errorResponse('MISSING_FIELDS', '缺少 data.name');
      const clean = {
        name: String(data.name).trim(),
        category: ['personal', 'fixed', 'insurance'].includes(data.category) ? data.category : 'personal',
        isDone: !!data.isDone,
        order: Number(data.order) || 0,
        createdAt: data.createdAt || Date.now(),
      };
      let savedId = id;
      if (id) {
        const ok = await dbPut(`${basePath}/statuses/${id}`, clean, token);
        if (!ok) return errorResponse('INTERNAL', '寫入失敗');
      } else {
        savedId = await dbPush(`${basePath}/statuses`, clean, token);
        if (!savedId) return errorResponse('INTERNAL', '寫入失敗');
      }
      return successResponse({ id: savedId, data: clean });
    }

    if (action === 'delete-status') {
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');
      const ok = await dbDelete(`${basePath}/statuses/${id}`, token);
      if (!ok) return errorResponse('INTERNAL', '刪除失敗');
      return successResponse({ deletedId: id });
    }

    return errorResponse('BAD_REQUEST', `未知的 action：${action || '(空)'}`);
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 3：family-accounts
   ============================================ */
export async function handleFamilyAccountsGet(request) {
  try {
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const token = extractToken(request);
    const superUser = await verifySuperAdmin(token);
    let authorized = false;
    if (superUser) authorized = true;
    else if (await verifyFamilyAccess(token, familyId)) authorized = true;

    if (!authorized) return errorResponse('FORBIDDEN', '無權存取此家庭的帳號列表');

    const snap = await dbGet(`platform/families/${familyId}/memberAccounts`, token);
    const accounts = objToList(snap || {}, (a, b) => (a.createdAt || 0) - (b.createdAt || 0));
    return successResponse({ familyId, accounts, count: accounts.length });
  } catch (err) {
    return handleError(err);
  }
}

export async function handleFamilyAccountsPost(request) {
  try {
    const body = await request.json();
    const { action, familyId, uid } = body || {};
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    switch (action) {
      case 'create':  return await _createAccount(body, token);
      case 'restore': return await _restoreAccount(body, token);
      case 'update':  return await _updateAccount(body, token);
      case 'remove':  return await _removeAccount(body, token);
      default:
        return errorResponse('BAD_REQUEST', `未知的 action：${action}`);
    }
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   內部實作
   ============================================ */
const _byOrder = (a, b) => (a.order || 0) - (b.order || 0);

function _sanitizeOptions(options) {
  if (!options || typeof options !== 'object') return {};
  const clean = {};
  ['memberRoles', 'cycles', 'policyTypes', 'insurancePaymentTypes', 'categoryOrder'].forEach((key) => {
    if (Array.isArray(options[key])) clean[key] = options[key];
  });
  return clean;
}

function _sanitizeYearRange(data) {
  if (!data || typeof data !== 'object') return { startYear: null, futureYears: 5 };
  return {
    startYear: data.startYear != null && data.startYear !== '' ? Number(data.startYear) : null,
    futureYears: Number(data.futureYears) || 5,
  };
}

function _sanitizeUIConstants(data) {
  if (!data || typeof data !== 'object') return {};
  const clean = {};
  if (data.nameMaxLenDesktop != null) clean.nameMaxLenDesktop = _toInt(data.nameMaxLenDesktop, 12);
  if (data.nameMaxLenMobile != null) clean.nameMaxLenMobile = _toInt(data.nameMaxLenMobile, 6);
  if (data.toastDuration != null) clean.toastDuration = _toInt(data.toastDuration, 2000);
  return clean;
}

function _toInt(v, fallback) {
  const n = Number(v);
  if (isNaN(n) || n < 0) return fallback;
  return Math.floor(n);
}

async function _createAccount(body, token) {
  const { familyId, account, password, displayName, role, canInput, memberId: inputMemberId } = body;
  const missing = requireFields(body, ['account', 'password', 'displayName']);
  if (missing) return errorResponse('MISSING_FIELDS', '缺少 account / password / displayName');

  const familySnap = await dbGet(`platform/families/${familyId}`, token);
  if (!familySnap) return errorResponse('NOT_FOUND', `找不到家庭 ${familyId}`);

  const accountStr = String(account).trim().toLowerCase();
  if (!/^[a-z0-9._-]+$/.test(accountStr)) {
    return errorResponse('BAD_REQUEST', '帳號只能包含小寫英數字、點、底線、連字號');
  }
  const pwdStr = String(password);
  if (pwdStr.length < 6) return errorResponse('BAD_REQUEST', '密碼至少 6 位');

  const roleStr = role === 'owner' ? 'owner' : 'member';
  const email = `${accountStr}@familyfin.local`;

  const existing = await dbGet(`platform/email_index/${accountStr}`, token);
  if (existing) return errorResponse('CONFLICT', `帳號 ${accountStr} 已存在於平台記錄`);

  const signUpResult = await dbSignUp(email, pwdStr);
  if (!signUpResult.ok) {
    if (signUpResult.error === 'EMAIL_EXISTS') {
      return errorResponse('EMAIL_EXISTS', `帳號「${accountStr}」已存在於 Firebase 系統`, 409);
    }
    return errorResponse('INTERNAL', `建立帳號失敗：${signUpResult.error}`);
  }

  const newUid = signUpResult.uid;
  const now = Date.now();
  const memberId = String(inputMemberId || '').trim()
    || await _resolveMemberId(familyId, displayName, token);

  const accountData = {
    email, account: accountStr,
    displayName: String(displayName).trim(),
    role: roleStr, canInput: canInput !== false, memberId, createdAt: now,
  };

  const ok1 = await dbPut(`platform/families/${familyId}/memberAccounts/${newUid}`, accountData, token);
  if (!ok1) return errorResponse('INTERNAL', `Firebase Auth 帳號已建立（${email}），但寫入平台記錄失敗`, 500);

  const ok2 = await dbPut(`platform/uid_index/${newUid}`, familyId, token);
  if (!ok2) {
    await dbDelete(`platform/families/${familyId}/memberAccounts/${newUid}`, token);
    return errorResponse('INTERNAL', `uid_index 寫入失敗`, 500);
  }

  await dbPut(`platform/email_index/${accountStr}`, { uid: newUid, familyId }, token);
  return successResponse({ uid: newUid, account: accountData });
}

async function _restoreAccount(body, token) {
  const { familyId, account, password, displayName, role, canInput, memberId: inputMemberId } = body;
  const missing = requireFields(body, ['account', 'password', 'displayName']);
  if (missing) return errorResponse('MISSING_FIELDS', '缺少必要欄位');

  const accountStr = String(account).trim().toLowerCase();
  const pwdStr = String(password);
  const email = `${accountStr}@familyfin.local`;

  const existingIndex = await dbGet(`platform/email_index/${accountStr}`, token);
  if (existingIndex) return errorResponse('CONFLICT', `帳號 ${accountStr} 已在平台記錄中`);

  const signInResult = await _signInWithPassword(email, pwdStr);
  if (!signInResult.ok) {
    if (signInResult.error === 'EMAIL_NOT_FOUND') return errorResponse('NOT_FOUND', '此帳號在 Firebase 系統中不存在');
    if (signInResult.error === 'INVALID_PASSWORD' || signInResult.error === 'INVALID_LOGIN_CREDENTIALS') {
      return errorResponse('FORBIDDEN', '舊密碼錯誤');
    }
    return errorResponse('INTERNAL', `驗證失敗：${signInResult.error}`);
  }

  const uid = signInResult.uid;
  const uidIndex = await dbGet(`platform/uid_index/${uid}`, token);
  if (uidIndex) return errorResponse('CONFLICT', `此帳號已屬於家庭 ${uidIndex}`);

  const now = Date.now();
  const memberId = String(inputMemberId || '').trim()
    || await _resolveMemberId(familyId, displayName, token);

  const accountData = {
    email, account: accountStr,
    displayName: String(displayName).trim(),
    role: role === 'owner' ? 'owner' : 'member',
    canInput: canInput !== false, memberId, createdAt: now, restoredAt: now,
  };

  const ok1 = await dbPut(`platform/families/${familyId}/memberAccounts/${uid}`, accountData, token);
  if (!ok1) return errorResponse('INTERNAL', '寫入 memberAccounts 失敗');

  const ok2 = await dbPut(`platform/uid_index/${uid}`, familyId, token);
  if (!ok2) {
    await dbDelete(`platform/families/${familyId}/memberAccounts/${uid}`, token);
    return errorResponse('INTERNAL', '寫入 uid_index 失敗');
  }

  await dbPut(`platform/email_index/${accountStr}`, { uid, familyId }, token);
  return successResponse({ restored: true, uid, account: accountData });
}

async function _updateAccount(body, token) {
  const { familyId, uid, displayName, role, canInput, memberId: inputMemberId } = body;
  if (!uid) return errorResponse('MISSING_FIELDS', '缺少 uid');

  const existing = await dbGet(`platform/families/${familyId}/memberAccounts/${uid}`, token);
  if (!existing) return errorResponse('NOT_FOUND', '找不到此成員帳號');

  const patch = {};
  if (displayName !== undefined) {
    const dn = String(displayName).trim();
    if (!dn) return errorResponse('BAD_REQUEST', '顯示名稱不可為空');
    patch.displayName = dn;
  }
  if (role !== undefined) {
    if (role !== 'owner' && role !== 'member') return errorResponse('BAD_REQUEST', 'role 必須是 owner 或 member');
    if (role === 'owner' && existing.role !== 'owner') return errorResponse('BAD_REQUEST', '不可將成員升級為 owner');
    patch.role = role;
  }
  if (canInput !== undefined) patch.canInput = canInput === true;
  if (inputMemberId !== undefined) patch.memberId = String(inputMemberId || '').trim();

  if (Object.keys(patch).length === 0) return errorResponse('BAD_REQUEST', '沒有任何欄位要更新');
  patch.updatedAt = Date.now();

  const ok = await dbPut(`platform/families/${familyId}/memberAccounts/${uid}`, { ...existing, ...patch }, token);
  if (!ok) return errorResponse('INTERNAL', '更新失敗');
  return successResponse({ uid, patch });
}

async function _removeAccount(body, token) {
  const { familyId, uid } = body;
  if (!uid) return errorResponse('MISSING_FIELDS', '缺少 uid');

  const existing = await dbGet(`platform/families/${familyId}/memberAccounts/${uid}`, token);
  if (!existing) return errorResponse('NOT_FOUND', '找不到此成員帳號');
  if (existing.role === 'owner') return errorResponse('FORBIDDEN', '不可移除此家庭的主帳號（owner）');

  const ok1 = await dbDelete(`platform/families/${familyId}/memberAccounts/${uid}`, token);
  if (!ok1) return errorResponse('INTERNAL', '移除 memberAccounts 失敗');

  await dbDelete(`platform/uid_index/${uid}`, token);
  if (existing.account) await dbDelete(`platform/email_index/${existing.account}`, token);

  return successResponse({ removedUid: uid, removedAccount: existing.account });
}

async function _resolveMemberId(familyId, displayName, token) {
  const dn = String(displayName || '').trim();
  if (!dn) return '';
  try {
    const members = await dbGet(`families/${familyId}/members`, token);
    if (!members || typeof members !== 'object') return '';
    const matched = Object.entries(members).find(([, m]) => String((m && m.name) || '').trim() === dn);
    return matched ? matched[0] : '';
  } catch (err) {
    return '';
  }
}

async function _signInWithPassword(email, password) {
  try {
    const url = `${IDENTITY_TOOLKIT_URL}/accounts:signInWithPassword?key=${FIREBASE_API_KEY}`;
    const res = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password, returnSecureToken: false }),
    });
    const data = await res.json();
    if (!res.ok) return { ok: false, error: data?.error?.message || 'UNKNOWN_ERROR' };
    return { ok: true, uid: data.localId, email: data.email };
  } catch (err) {
    return { ok: false, error: String(err?.message || err) };
  }
}```

---

## 📄 functions/api/_helpers.js

```
// ============================================
// _helpers.js — API 共用輔助（v103.0.44）
// 位置：functions/api/_helpers.js
// ============================================
// v103.0.44（B30-5）：
//   ✅ 改從 getApiKey() 讀（支援 DB 切換）
// v103.0.22（B25-4）：
//   ✅ H3：移除重複定義，改從 _config.js 導入
// ============================================

import { SUPERADMIN_EMAIL, getApiKey, IDENTITY_TOOLKIT_URL as IDENTITY_TOOLKIT_BASE, jsonResponse, dbGet } from './_config.js';

const IDENTITY_TOOLKIT_LOOKUP_URL = `${IDENTITY_TOOLKIT_BASE}/accounts:lookup`;

export function handleOptions() {
  return new Response(null, {
    status: 204,
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, PUT, PATCH, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type, Authorization',
      'Access-Control-Max-Age': '86400',
    },
  });
}

export const PLATFORM_RESOURCES = {
  members:     { path: 'members',             type: 'list' },
  banks:       { path: 'banks',               type: 'list' },
  companies:   { path: 'insurance_companies', type: 'list' },
  payments:    { path: 'payment_methods',     type: 'list' },
  categories:  { path: 'expense_categories',  type: 'list' },
  items:       { path: 'expense_items',       type: 'list' },
  statuses:    { path: 'statuses',            type: 'list' },
  options:     { path: 'options',             type: 'object' },
  yearRange:   { path: 'year_range',          type: 'object' },
  uiConstants: { path: 'ui_constants',        type: 'object' },
};

const _tokenCache = new Map();
const _TOKEN_CACHE_MS = 60 * 1000;

export function extractToken(request) {
  const authHeader = request.headers.get('Authorization') || '';
  return authHeader.replace('Bearer ', '').trim();
}

export async function verifyToken(token) {
  if (!token) return null;

  const cached = _tokenCache.get(token);
  if (cached && cached.expireAt > Date.now()) return cached.user;

  try {
    const res = await fetch(`${IDENTITY_TOOLKIT_LOOKUP_URL}?key=${getApiKey()}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ idToken: token }),
    });
    if (!res.ok) return null;

    const data = await res.json();
    const user = data?.users?.[0] || null;
    if (user) {
      _tokenCache.set(token, { user, expireAt: Date.now() + _TOKEN_CACHE_MS });
    }
    return user;
  } catch (err) {
    console.warn('[_helpers] verifyToken 失敗：', err);
    return null;
  }
}

export async function verifySuperAdmin(token) {
  const user = await verifyToken(token);
  if (!user) return null;
  if (user.email !== SUPERADMIN_EMAIL) return null;
  return user;
}

export async function verifyFamilyAccess(token, familyId) {
  if (!familyId) return null;

  const user = await verifyToken(token);
  if (!user) return null;

  const isSuper = user.email === SUPERADMIN_EMAIL;
  if (isSuper) return { user, isSuper: true, isOwner: true, canInput: true };

  if (user.localId === familyId) {
    const memberAccount = await _getMemberAccount(familyId, user.localId, token);
    if (!memberAccount) {
      return { user, isSuper: false, isOwner: false, canInput: false, isLegacy: true };
    }
    return {
      user,
      isSuper: false,
      isOwner: memberAccount.role === 'owner',
      canInput: memberAccount.canInput !== false,
      isLegacy: true,
    };
  }

  const memberAccount = await _getMemberAccount(familyId, user.localId, token);
  if (memberAccount) {
    return {
      user,
      isSuper: false,
      isOwner: memberAccount.role === 'owner',
      canInput: memberAccount.canInput !== false,
    };
  }

  return null;
}

export async function verifyFamilyAccessByUid(uid, token = '') {
  if (!uid) return null;

  const familyId = await dbGet(`platform/uid_index/${uid}`, token);
  if (familyId && typeof familyId === 'string') {
    const memberAccount = await _getMemberAccount(familyId, uid, token);
    return { familyId, memberAccount: memberAccount || null, isLegacy: false };
  }

  const familySnap = await dbGet(`platform/families/${uid}`, token);
  if (familySnap) {
    return {
      familyId: uid,
      memberAccount: {
        email: '',
        displayName: familySnap.name || '我的家庭',
        role: 'owner',
        canInput: true,
      },
      isLegacy: true,
    };
  }

  return null;
}

async function _getMemberAccount(familyId, uid, token = '') {
  try {
    return (await dbGet(`platform/families/${familyId}/memberAccounts/${uid}`, token)) || null;
  } catch (err) {
    return null;
  }
}

export function requireFields(body, fields) {
  if (!body || typeof body !== 'object') {
    return { ok: false, error: 'INVALID_BODY', fields };
  }
  const missing = fields.filter((f) => {
    const v = body[f];
    return v === '' || v == null;
  });
  if (missing.length > 0) return { ok: false, error: 'MISSING_FIELDS', fields: missing };
  return null;
}

export function errorResponse(code, message = '', status) {
  const statusMap = {
    UNAUTHORIZED: 401,
    FORBIDDEN: 403,
    MISSING_FIELDS: 400,
    INVALID_BODY: 400,
    BAD_REQUEST: 400,
    NOT_FOUND: 404,
    CONFLICT: 409,
    INTERNAL: 500,
  };
  const httpStatus = status || statusMap[code] || 500;
  const body = { ok: false, error: code };
  if (message) body.message = message;
  return jsonResponse(body, httpStatus);
}

export function handleError(err) {
  console.error('[_helpers] API error:', err);
  return errorResponse('INTERNAL', String(err?.message || err));
}

export function successResponse(data = {}) {
  return jsonResponse({ ok: true, ...data });
}

export async function authenticate(request, options = {}) {
  const { body, requiredFields, needSuperAdmin, needFamily } = options;
  const token = extractToken(request);

  if (requiredFields && body) {
    const missingResult = requireFields(body, requiredFields);
    if (missingResult) return errorResponse('MISSING_FIELDS', '', 400);
  }

  if (needSuperAdmin) {
    const user = await verifySuperAdmin(token);
    if (!user) return errorResponse('FORBIDDEN', '需要超級管理員權限');
    return { token, user, isSuper: true };
  }

  if (needFamily) {
    let familyId = body?.familyId;
    if (!familyId) {
      try {
        const url = new URL(request.url);
        familyId = url.searchParams.get('familyId');
      } catch (e) { /* noop */ }
    }

    if (!familyId) return errorResponse('MISSING_FIELDS', 'familyId 為必填');
    const result = await verifyFamilyAccess(token, familyId);
    if (!result) return errorResponse('FORBIDDEN', '無權存取此家庭');
    return {
      token,
      user: result.user,
      isSuper: result.isSuper,
      isOwner: result.isOwner,
      canInput: result.canInput,
      familyId,
    };
  }

  const user = await verifyToken(token);
  if (!user) return errorResponse('UNAUTHORIZED', '請重新登入');
  return { token, user, isSuper: user.email === SUPERADMIN_EMAIL };
}

export function filterEmpty(obj) {
  if (!obj || typeof obj !== 'object') return {};
  const out = {};
  Object.entries(obj).forEach(([k, v]) => { if (v != null) out[k] = v; });
  return out;
}

export function objToList(obj, sortFn) {
  const list = Object.entries(obj || {}).map(([id, x]) => ({ id, ...x }));
  if (typeof sortFn === 'function') list.sort(sortFn);
  return list;
}

export function roundInt(v) {
  return Math.round(Number(v) || 0);
}

export const LINKED_PREFIX = 'linked_';

export function buildLinkedKey(policyId) {
  return `${LINKED_PREFIX}${policyId}`;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: functions/api/_helpers.js
   Version: v103.0.44
   Batch: B30-5
   ═══════════════════════════════════════════ */```

---

## 📄 functions/api/_insurance.js

```
// _insurance.js — 保險同步 lib（v103.0.21）
import { dbGet, dbPut, dbDelete } from './_config.js';
import { errorResponse, handleError, successResponse, requireFields, roundInt, buildLinkedKey, authenticate } from './_helpers.js';

export async function handleInsuranceSync(request) {
  try {
    const body = await request.json();
    const { action, familyId, policyId, memberId, policyName, monthlyAverage, year, month, expenseStatusName, paymentStatusName, bankId, txnId, paymentMode, advanceHolderId } = body || {};
    const missing = requireFields(body, ['familyId', 'policyId', 'memberId', 'year', 'month']);
    if (missing) return errorResponse('MISSING_FIELDS', '', 400);

    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}`;
    const linkedKey = buildLinkedKey(policyId);
    const expensePath = `${basePath}/expenses/${year}/${month}/member_expenses/${memberId}/${linkedKey}`;
    const paymentPath = `${basePath}/insurance_payments/${policyId}/${year}/${month}`;
    const amount = roundInt(monthlyAverage);

    /* DELETE */
    if (action === 'delete') {
      await dbDelete(expensePath, token);
      await dbDelete(paymentPath, token);
      if (paymentMode === 'advance' && advanceHolderId) {
        try { await _refundAdvance({ basePath, policyId, advanceHolderId, amount, year, month, token }); } catch (e) { console.warn('[insurance] 退還代墊失敗：', e); }
      }
      /* 撤銷基金累積 */
      try { await _reverseFundAccumulation({ basePath, policyId, year, month, token }); } catch (e) { console.warn('[insurance] 撤銷基金累積失敗：', e); }
      return successResponse({ deleted: true });
    }

    /* UPSERT */
    const statuses = await dbGet(`${basePath}/statuses`, token) || {};
    const categories = await dbGet(`${basePath}/expense_categories`, token) || {};
    const firstCategoryId = _getFirstCategoryId(categories);

    const resolvedExpenseStatus = expenseStatusName || _findDoneStatus(statuses, 'personal') || '已還款';
    const resolvedPaymentStatus = paymentStatusName || _findDoneStatus(statuses, 'insurance') || '已扣款';

    const expense = {
      name: `${policyName} (平攤)`, amount, status: resolvedExpenseStatus, date: '',
      categoryId: firstCategoryId, itemId: '', paymentMethodId: '',
      bankId: bankId || '', txnId: txnId || '',
      isAutoLinked: true, policyId, createdAt: Date.now(),
    };
    const expenseOk = await dbPut(expensePath, expense, token);

    const paymentRecord = {
      status: resolvedPaymentStatus, amount,
      date: new Date().toISOString().slice(0, 10),
      bankId: bankId || '', txnId: txnId || '',
      paymentMode: paymentMode || 'direct',
    };
    const paymentOk = await dbPut(paymentPath, paymentRecord, token);

    if (paymentMode === 'advance' && advanceHolderId) {
      try { await _deductAdvance({ basePath, policyId, advanceHolderId, amount, year, month, token }); } catch (e) { console.warn('[insurance] 更新代墊記錄失敗：', e); }
    }

    /* 基金累積 */
    try { await _accumulateFunds({ basePath, policyId, year, month, token }); } catch (e) { console.warn('[insurance] 基金累積失敗：', e); }

    if (!expenseOk && !paymentOk) return errorResponse('INTERNAL', '寫入失敗，請檢查 Firebase 規則');

    return successResponse({ path: expensePath, expenseStatus: resolvedExpenseStatus, paymentStatus: resolvedPaymentStatus, categoryId: firstCategoryId, paymentMode: paymentMode || 'direct' });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   基金累積（依 policy.fundsAllocation）
   ============================================ */
async function _accumulateFunds({ basePath, policyId, year, month, token }) {
  const policy = await dbGet(`${basePath}/insurance_policies/${policyId}`, token);
  if (!policy || policy.type !== 'fund_insurance') return;
  const alloc = policy.fundsAllocation || [];
  if (alloc.length === 0) return;

  const monthlyPremium = roundInt(policy.monthlyPremium);
  const yk = String(year), mk = String(month).padStart(2, '0');

  for (const a of alloc) {
    const fundId = a.fundId;
    if (!fundId) continue;
    const addCost = Math.round(monthlyPremium * (Number(a.pct) || 0) / 100);
    const fundPath = `${basePath}/funds/${fundId}`;
    const fund = await dbGet(fundPath, token) || {};
    const snapPath = `${fundPath}/snapshots/${yk}/${mk}`;
    const snap = await dbGet(snapPath, token) || {};
    const newCum = roundInt(snap.cumulativeCost) + addCost;
    await dbPut(snapPath, {
      ...snap,
      cumulativeCost: newCum,
      contribution: addCost,
      createdAt: snap.createdAt || Date.now(),
      updatedAt: Date.now(),
    }, token);
    /* 標記 fund 為保險基金 */
    if (fund.type !== 'insurance') {
      await dbPut(fundPath, { ...fund, type: 'insurance', policyId }, token);
    }
  }
}

async function _reverseFundAccumulation({ basePath, policyId, year, month, token }) {
  const policy = await dbGet(`${basePath}/insurance_policies/${policyId}`, token);
  if (!policy || policy.type !== 'fund_insurance') return;
  const alloc = policy.fundsAllocation || [];
  if (alloc.length === 0) return;
  const monthlyPremium = roundInt(policy.monthlyPremium);
  const yk = String(year), mk = String(month).padStart(2, '0');

  for (const a of alloc) {
    const fundId = a.fundId;
    if (!fundId) continue;
    const subCost = Math.round(monthlyPremium * (Number(a.pct) || 0) / 100);
    const snapPath = `${basePath}/funds/${fundId}/snapshots/${yk}/${mk}`;
    const snap = await dbGet(snapPath, token);
    if (!snap) continue;
    const newCum = Math.max(0, roundInt(snap.cumulativeCost) - subCost);
    if (newCum === 0 && !snap.shares && !snap.nav) {
      await dbDelete(snapPath, token);
    } else {
      await dbPut(snapPath, { ...snap, cumulativeCost: newCum, contribution: 0, updatedAt: Date.now() }, token);
    }
  }
}

/* ============================================
   代墊
   ============================================ */
async function _deductAdvance({ basePath, policyId, advanceHolderId, amount, year, month, token }) {
  const advances = await dbGet(`${basePath}/member_advances/${advanceHolderId}`, token);
  if (!advances) return;
  const monthKey = `${year}-${String(month).padStart(2, '0')}`;
  for (const [advanceId, adv] of Object.entries(advances)) {
    if (adv.policyId !== policyId) continue;
    const paidMonths = (adv.paidMonths && typeof adv.paidMonths === 'object') ? { ...adv.paidMonths } : {};
    if (paidMonths[monthKey]) return;
    const newRemaining = Math.max(0, roundInt(adv.remainingAmount) - amount);
    paidMonths[monthKey] = true;
    await dbPut(`${basePath}/member_advances/${advanceHolderId}/${advanceId}`, { ...adv, remainingAmount: newRemaining, paidMonths, updatedAt: Date.now() }, token);
    return;
  }
}

async function _refundAdvance({ basePath, policyId, advanceHolderId, amount, year, month, token }) {
  const advances = await dbGet(`${basePath}/member_advances/${advanceHolderId}`, token);
  if (!advances) return;
  const monthKey = `${year}-${String(month).padStart(2, '0')}`;
  for (const [advanceId, adv] of Object.entries(advances)) {
    if (adv.policyId !== policyId) continue;
    const paidMonths = (adv.paidMonths && typeof adv.paidMonths === 'object') ? { ...adv.paidMonths } : {};
    if (!paidMonths[monthKey]) return;
    delete paidMonths[monthKey];
    const totalAmount = roundInt(adv.totalAmount);
    const newRemaining = Math.min(totalAmount, roundInt(adv.remainingAmount) + amount);
    await dbPut(`${basePath}/member_advances/${advanceHolderId}/${advanceId}`, { ...adv, remainingAmount: newRemaining, paidMonths, updatedAt: Date.now() }, token);
    return;
  }
}

function _getFirstCategoryId(categories) {
  const list = Object.entries(categories || {}).map(([id, c]) => ({ id, ...c })).sort((a, b) => (a.order || 0) - (b.order || 0));
  return list.length > 0 ? list[0].id : '';
}

function _findDoneStatus(statuses, category) {
  const list = Object.entries(statuses).map(([id, s]) => ({ id, ...s })).filter((s) => s.category === category && s.isDone).sort((a, b) => (a.order || 0) - (b.order || 0));
  if (list.length > 0) return list[0].name;
  const anyDone = Object.entries(statuses).map(([id, s]) => ({ id, ...s })).filter((s) => s.isDone).sort((a, b) => (a.order || 0) - (b.order || 0));
  return anyDone.length > 0 ? anyDone[0].name : null;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: functions/api/_insurance.js
   Version: v103.0.21
   Batch: B23
   ═══════════════════════════════════════════ */```

---

## 📄 functions/api/_personal.js

```
// ============================================
// _personal.js — 個人收入 / 代墊 lib（v103.0.2）
// 位置：functions/api/_personal.js
// ============================================
import { dbGet, dbPut, dbPush, dbDelete } from './_config.js';
import {
  authenticate, errorResponse, handleError,
  successResponse, requireFields, objToList, roundInt,
  verifyFamilyAccess,
} from './_helpers.js';

/* ============================================
   Handler 1：personal-income
   ============================================ */
export async function handlePersonalIncomeGet(request) {
  try {
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const memberId = url.searchParams.get('memberId');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const auth = await authenticate(request, { needFamily: true });
    if (auth instanceof Response) return auth;
    const { token, user } = auth;

    const accessResult = await verifyFamilyAccess(token, familyId);
    if (!accessResult) return errorResponse('FORBIDDEN', '無權存取');

    const isOwner = accessResult.isOwner;
    const isSuper = accessResult.isSuper;

    if (memberId && !isOwner && !isSuper) {
      const ownMemberId = await _resolveOwnMemberId(familyId, user.localId, token);
      if (!ownMemberId || ownMemberId !== memberId) {
        return errorResponse('FORBIDDEN', '無權讀取其他成員的收入');
      }
    }

    if (memberId) {
      const data = await dbGet(`families/${familyId}/personal_income/${memberId}`, token);
      return successResponse({ familyId, memberId, income: data || {} });
    }

    if (!isOwner && !isSuper) return errorResponse('FORBIDDEN', '需要 owner 權限才能讀取所有成員收入');
    const all = await dbGet(`families/${familyId}/personal_income`, token);
    return successResponse({ familyId, personalIncome: all || {} });
  } catch (err) {
    return handleError(err);
  }
}

export async function handlePersonalIncomePost(request) {
  try {
    const body = await request.json();
    const { familyId, memberId, year, month, amount, action } = body || {};
    const missing = requireFields(body, ['familyId', 'memberId', 'year', 'month', 'action']);
    if (missing) return errorResponse('MISSING_FIELDS', '缺少必要欄位');

    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token, user } = auth;

    const accessResult = await verifyFamilyAccess(token, familyId);
    if (!accessResult) return errorResponse('FORBIDDEN', '無權存取');

    const isOwner = accessResult.isOwner;
    const isSuper = accessResult.isSuper;

    if (!isOwner && !isSuper) {
      const ownMemberId = await _resolveOwnMemberId(familyId, user.localId, token);
      if (!ownMemberId || ownMemberId !== memberId) {
        return errorResponse('FORBIDDEN', '無權修改其他成員的收入');
      }
    }

    const mm = String(month).padStart(2, '0');
    const yyyy = String(year);
    const path = `families/${familyId}/personal_income/${memberId}/${yyyy}/${mm}`;

    if (action === 'save') {
      const num = roundInt(amount);
      if (num <= 0) {
        await dbDelete(path, token);
        return successResponse({ deleted: true });
      }
      const ok = await dbPut(path, num, token);
      if (!ok) return errorResponse('INTERNAL', '寫入失敗');
      return successResponse({ saved: true, amount: num });
    }

    if (action === 'remove') {
      const ok = await dbDelete(path, token);
      if (!ok) return errorResponse('INTERNAL', '刪除失敗');
      return successResponse({ removed: true });
    }

    return errorResponse('BAD_REQUEST', `未知的 action：${action}`);
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 2：member-advances
   ============================================ */
export async function handleMemberAdvancesGet(request) {
  try {
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const memberId = url.searchParams.get('memberId');
    const action = url.searchParams.get('action') || 'list';
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const auth = await authenticate(request, { needFamily: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (action === 'listAll') {
      const all = await dbGet(`families/${familyId}/member_advances`, token);
      const flat = [];
      Object.entries(all || {}).forEach(([mid, advances]) => {
        Object.entries(advances || {}).forEach(([advanceId, adv]) => {
          flat.push({ id: advanceId, memberId: mid, ...adv });
        });
      });
      flat.sort((a, b) => (a.createdAt || 0) - (b.createdAt || 0));
      return successResponse({ familyId, advances: flat, count: flat.length });
    }

    if (!memberId) return errorResponse('MISSING_FIELDS', '缺少 memberId');
    const data = await dbGet(`families/${familyId}/member_advances/${memberId}`, token);
    const list = objToList(data || {}, (a, b) => (a.createdAt || 0) - (b.createdAt || 0));
    return successResponse({ familyId, memberId, advances: list, count: list.length });
  } catch (err) {
    return handleError(err);
  }
}

export async function handleMemberAdvancesPost(request) {
  try {
    const body = await request.json();
    const { familyId, memberId, action, advanceId, data } = body || {};
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (!memberId) return errorResponse('MISSING_FIELDS', '缺少 memberId');
    if (!action) return errorResponse('MISSING_FIELDS', '缺少 action');

    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}/member_advances/${memberId}`;

    if (action === 'create') {
      if (!data || typeof data !== 'object') return errorResponse('MISSING_FIELDS', '缺少 data');
      const missing = requireFields(data, ['policyId', 'totalAmount']);
      if (missing) return errorResponse('MISSING_FIELDS', '缺少 policyId 或 totalAmount');
      const totalAmount = roundInt(data.totalAmount);
      if (totalAmount <= 0) return errorResponse('BAD_REQUEST', 'totalAmount 必須大於 0');

      const clean = {
        policyId: String(data.policyId),
        totalAmount,
        remainingAmount: totalAmount,
        startYear: String(data.startYear || ''),
        startMonth: String(data.startMonth || '').padStart(2, '0'),
        note: data.note || '',
        paidMonths: {},
        createdAt: Date.now(),
      };
      const newId = await dbPush(basePath, clean, token);
      if (!newId) return errorResponse('INTERNAL', '寫入失敗');
      return successResponse({ id: newId, data: clean });
    }

    if (action === 'update') {
      if (!advanceId) return errorResponse('MISSING_FIELDS', '缺少 advanceId');
      const existing = await dbGet(`${basePath}/${advanceId}`, token);
      if (!existing) return errorResponse('NOT_FOUND', '找不到此代墊記錄');

      if (data.remainingAmount !== undefined) {
        return errorResponse('FORBIDDEN',
          'remainingAmount 為系統自動計算欄位，不可直接修改。若需調整，請透過保險結算由系統自動處理。', 403);
      }

      const clean = {};
      if (data.policyId !== undefined) clean.policyId = String(data.policyId);
      if (data.totalAmount !== undefined) {
        const total = roundInt(data.totalAmount);
        if (total <= 0) return errorResponse('BAD_REQUEST', 'totalAmount 必須大於 0');
        clean.totalAmount = total;
        const oldTotal = roundInt(existing.totalAmount);
        const oldRemaining = roundInt(existing.remainingAmount);
        const paidSum = Math.max(0, oldTotal - oldRemaining);
        clean.remainingAmount = Math.max(0, total - paidSum);
      }
      if (data.startYear !== undefined) clean.startYear = String(data.startYear);
      if (data.startMonth !== undefined) clean.startMonth = String(data.startMonth).padStart(2, '0');
      if (data.note !== undefined) clean.note = String(data.note);

      if (Object.keys(clean).length === 0) return errorResponse('BAD_REQUEST', '沒有任何欄位要更新');
      clean.updatedAt = Date.now();
      const ok = await dbPut(`${basePath}/${advanceId}`, { ...existing, ...clean }, token);
      if (!ok) return errorResponse('INTERNAL', '更新失敗');
      return successResponse({ advanceId, data: { ...existing, ...clean } });
    }

    if (action === 'remove') {
      if (!advanceId) return errorResponse('MISSING_FIELDS', '缺少 advanceId');
      const existing = await dbGet(`${basePath}/${advanceId}`, token);
      if (!existing) return errorResponse('NOT_FOUND', '找不到此代墊記錄');
      const ok = await dbDelete(`${basePath}/${advanceId}`, token);
      if (!ok) return errorResponse('INTERNAL', '刪除失敗');
      return successResponse({ removedId: advanceId });
    }

    return errorResponse('BAD_REQUEST', `未知的 action：${action}`);
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   內部工具
   ============================================ */
async function _resolveOwnMemberId(familyId, uid, token) {
  const memberAccount = await dbGet(`platform/families/${familyId}/memberAccounts/${uid}`, token);
  if (memberAccount && memberAccount.memberId) return memberAccount.memberId;

  if (memberAccount && memberAccount.displayName) {
    try {
      const members = await dbGet(`families/${familyId}/members`, token);
      if (members && typeof members === 'object') {
        const dn = String(memberAccount.displayName).trim();
        const matched = Object.entries(members).find(([, m]) => String((m && m.name) || '').trim() === dn);
        if (matched) return matched[0];
      }
    } catch (e) { /* noop */ }
  }

  try {
    const members = await dbGet(`families/${familyId}/members`, token);
    if (members && typeof members === 'object') {
      const keys = Object.keys(members);
      if (keys.length === 1) return keys[0];
    }
  } catch (e) { /* noop */ }

  return '';
}```

---

## 📄 functions/api/_platform.js

```
// ============================================
// _platform.js — 平台設定 lib（v103.0.2）
// 位置：functions/api/_platform.js
// ============================================
import { dbGet, dbPut, dbPush, dbDelete } from './_config.js';
import {
  authenticate, errorResponse, handleError,
  successResponse, objToList, PLATFORM_RESOURCES,
} from './_helpers.js';

const FIELD_WHITELIST = {
  members:     ['name', 'role', 'order'],
  banks:       ['name', 'order'],
  companies:   ['name', 'order'],
  payments:    ['name', 'order'],
  categories:  ['name', 'order'],
  items:       ['name', 'categoryKey', 'categoryId'],
  statuses:    ['name', 'category', 'isDone', 'order'],
  options:     null,
  yearRange:   ['startYear', 'futureYears'],
  uiConstants: ['nameMaxLenDesktop', 'nameMaxLenMobile', 'toastDuration'],
};

const byOrderThenCreated = (a, b) => {
  const oa = a.order != null ? a.order : Number.MAX_SAFE_INTEGER;
  const ob = b.order != null ? b.order : Number.MAX_SAFE_INTEGER;
  if (oa !== ob) return oa - ob;
  return (a.createdAt || 0) - (b.createdAt || 0);
};

/* ============================================
   Handler 1：platform-settings（uiConstants）
   ============================================ */
export async function handlePlatformSettingsGet(request) {
  try {
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;
    const data = await dbGet('platform/defaults/ui_constants', token);
    return successResponse({ data: data || {} });
  } catch (err) {
    return handleError(err);
  }
}

export async function handlePlatformSettingsPost(request) {
  try {
    const body = await request.json();
    const { action, data } = body || {};
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (action !== 'update') return errorResponse('BAD_REQUEST', '未知的 action');
    if (!data || typeof data !== 'object') return errorResponse('MISSING_FIELDS', '缺少 data');

    const clean = {
      nameMaxLenDesktop: _toInt(data.nameMaxLenDesktop, 12),
      nameMaxLenMobile:  _toInt(data.nameMaxLenMobile, 6),
      toastDuration:     _toInt(data.toastDuration, 2000),
    };
    const ok = await dbPut('platform/defaults/ui_constants', clean, token);
    if (!ok) return errorResponse('INTERNAL', '寫入失敗');
    return successResponse({ data: clean });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 2：platform-defaults
   ============================================ */
export async function handlePlatformDefaultsGet(request) {
  try {
    const url = new URL(request.url);
    const action = url.searchParams.get('action') || 'list';
    const resource = url.searchParams.get('resource');

    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (action !== 'list') return errorResponse('BAD_REQUEST', 'GET 僅支援 action=list');
    const config = PLATFORM_RESOURCES[resource];
    if (!config) return errorResponse('BAD_REQUEST', `未知的 resource：${resource || '(空)'}`);

    const data = await dbGet(`platform/defaults/${config.path}`, token);
    if (config.type === 'object') {
      return successResponse({ resource, type: 'object', data: data || {} });
    }
    const list = objToList(data || {}, byOrderThenCreated);
    return successResponse({ resource, type: 'list', list });
  } catch (err) {
    return handleError(err);
  }
}

export async function handlePlatformDefaultsPost(request) {
  try {
    const body = await request.json();
    const { action, resource, id, data } = body || {};

    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const config = PLATFORM_RESOURCES[resource];
    if (!config) return errorResponse('BAD_REQUEST', `未知的 resource：${resource || '(空)'}`);
    const basePath = `platform/defaults/${config.path}`;

    if (action === 'put') {
      if (config.type !== 'list') return errorResponse('BAD_REQUEST', `${resource} 不支援 put`);
      if (!data || typeof data !== 'object') return errorResponse('MISSING_FIELDS', '缺少 data');
      const clean = _whitelist(data, FIELD_WHITELIST[resource]);
      clean.createdAt = data.createdAt || Date.now();
      let savedId = id;
      if (id) {
        const ok = await dbPut(`${basePath}/${id}`, clean, token);
        if (!ok) return errorResponse('INTERNAL', '寫入失敗');
      } else {
        savedId = await dbPush(basePath, clean, token);
        if (!savedId) return errorResponse('INTERNAL', '寫入失敗');
      }
      return successResponse({ id: savedId, data: clean });
    }

    if (action === 'delete') {
      if (config.type !== 'list') return errorResponse('BAD_REQUEST', `${resource} 不支援 delete`);
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');
      const ok = await dbDelete(`${basePath}/${id}`, token);
      if (!ok) return errorResponse('INTERNAL', '刪除失敗');
      return successResponse({ deletedId: id });
    }

    if (action === 'set') {
      if (config.type !== 'object') return errorResponse('BAD_REQUEST', `${resource} 不支援 set`);
      if (data == null) return errorResponse('MISSING_FIELDS', '缺少 data');
      const clean = _sanitizeObjectResource(resource, data);
      const ok = await dbPut(basePath, clean, token);
      if (!ok) return errorResponse('INTERNAL', '寫入失敗');
      return successResponse({ data: clean });
    }

    return errorResponse('BAD_REQUEST', `未知的 action：${action || '(空)'}`);
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   內部工具
   ============================================ */
function _whitelist(data, allowed) {
  if (!allowed) return { ...data };
  const clean = {};
  allowed.forEach((key) => { if (data[key] !== undefined) clean[key] = data[key]; });
  return clean;
}

function _sanitizeObjectResource(resource, data) {
  if (resource === 'yearRange') {
    return {
      startYear: data.startYear != null && data.startYear !== '' ? Number(data.startYear) : null,
      futureYears: Number(data.futureYears) || 5,
    };
  }
  if (resource === 'uiConstants') {
    return {
      nameMaxLenDesktop: _toInt(data.nameMaxLenDesktop, 12),
      nameMaxLenMobile:  _toInt(data.nameMaxLenMobile, 6),
      toastDuration:     _toInt(data.toastDuration, 2000),
    };
  }
  if (resource === 'options') {
    return {
      memberRoles:           Array.isArray(data.memberRoles)           ? data.memberRoles           : [],
      cycles:                Array.isArray(data.cycles)                ? data.cycles                : [],
      policyTypes:           Array.isArray(data.policyTypes)           ? data.policyTypes           : [],
      insurancePaymentTypes: Array.isArray(data.insurancePaymentTypes) ? data.insurancePaymentTypes : [],
      categoryOrder:         Array.isArray(data.categoryOrder)         ? data.categoryOrder         : [],
    };
  }
  return data;
}

function _toInt(v, fallback) {
  const n = Number(v);
  if (isNaN(n) || n < 0) return fallback;
  return Math.floor(n);
}```

---

## 📄 functions/api/_summary.js

```
// ============================================
// _summary.js — 摘要 / 年度 / 結算 lib（v103.0.2）
// 位置：functions/api/_summary.js
// ============================================
import { dbGet, jsonResponse } from './_config.js';
import {
  authenticate, errorResponse, handleError,
  objToList, roundInt,
} from './_helpers.js';

/* ============================================
   Handler 1：summary（單月）
   ============================================ */
export async function handleSummary(request) {
  try {
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const year = url.searchParams.get('year');
    const month = url.searchParams.get('month');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const auth = await authenticate(request, { needFamily: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}`;
    let prevY = Number(year);
    let prevM = Number(month) - 1;
    if (prevM < 1) { prevY -= 1; prevM = 12; }
    const prevMonthStr = String(prevM).padStart(2, '0');

    const [
      members, policies, expenses, income, funds, fixed,
      categories, items, banks, paymentMethods, statuses, settingsOptions,
      bankAccounts, personalIncome, memberAdvances,
    ] = await Promise.all([
      dbGet(`${basePath}/members`, token),
      dbGet(`${basePath}/insurance_policies`, token),
      year && month ? dbGet(`${basePath}/expenses/${year}/${month}/member_expenses`, token) : null,
      year && month ? dbGet(`${basePath}/income/${year}/${month}`, token) : null,
      dbGet(`${basePath}/funds`, token),
      year && month ? dbGet(`${basePath}/fixed_expenses/${year}/${month}`, token) : null,
      dbGet(`${basePath}/expense_categories`, token),
      dbGet(`${basePath}/expense_items`, token),
      dbGet(`${basePath}/banks`, token),
      dbGet(`${basePath}/payment_methods`, token),
      dbGet(`${basePath}/statuses`, token),
      dbGet(`${basePath}/settings/options`, token),
      dbGet(`${basePath}/bank_accounts`, token),
      dbGet(`${basePath}/personal_income`, token),
      dbGet(`${basePath}/member_advances`, token),
    ]);

    const membersObj = members || {};
    const statusesObj = statuses || {};
    const statusMap = {};
    Object.entries(statusesObj).forEach(([id, s]) => { if (s && s.name) statusMap[s.name] = { id, ...s }; });

    const perMember = {};
    let totalExpense = 0;
    Object.entries(expenses || {}).forEach(([memberId, list]) => {
      const itemsArr = objToList(list, (a, b) => (a.date || '').localeCompare(b.date || '')).map((e) => {
        const statusInfo = statusMap[e.status];
        return {
          id: e.id, name: e.name || '', amount: roundInt(e.amount),
          status: e.status || '未處理',
          statusIsDone: statusInfo ? !!statusInfo.isDone : false,
          date: e.date || '',
          categoryId: e.categoryId || '',
          categoryName: (categories || {})[e.categoryId]?.name || '',
          itemId: e.itemId || '',
          itemName: (items || {})[e.itemId]?.name || '',
          isAutoLinked: e.isAutoLinked || false,
          policyId: e.policyId || '',
          paymentMethodId: e.paymentMethodId || '',
          paymentMethodName: (paymentMethods || {})[e.paymentMethodId]?.name || '',
          bankId: e.bankId || '', txnId: e.txnId || '',
        };
      });
      const sum = itemsArr.reduce((s, e) => s + e.amount, 0);
      perMember[memberId] = {
        memberName: (membersObj[memberId]?.name) || '（未知成員）',
        itemCount: itemsArr.length, sum: roundInt(sum), items: itemsArr,
      };
      totalExpense += sum;
    });

    const fixedList = objToList(fixed || {}).map((x) => {
      const statusInfo = statusMap[x.status];
      return {
        id: x.id, name: x.name || '', amount: roundInt(x.amount),
        cycle: x.cycle || '每月', note: x.note || '',
        status: x.status || '未付款',
        statusIsDone: statusInfo ? !!statusInfo.isDone : false,
        paidDate: x.paidDate || '',
        categoryId: x.categoryId || '',
        categoryName: (categories || {})[x.categoryId]?.name || '其他',
        paymentMethodId: x.paymentMethodId || '',
        paymentMethodName: (paymentMethods || {})[x.paymentMethodId]?.name || '',
        memberId: x.memberId || 'shared',
        createdAt: x.createdAt || 0,
      };
    }).filter((x) => x.status !== '不適用');

    const fixedTotal = fixedList.reduce((s, x) => s + x.amount, 0);
    const fixedPendingList = fixedList.filter((x) => !x.statusIsDone);
    const fixedPendingTotal = fixedPendingList.reduce((s, x) => s + x.amount, 0);
    totalExpense += fixedTotal;

    const incomeBreakdown = {};
    let totalIncome = 0;
    Object.entries(income || {}).forEach(([key, val]) => {
      const num = roundInt(val);
      if (key === 'extra' || membersObj[key]) {
        incomeBreakdown[key] = num;
        totalIncome += num;
      }
    });

    let personalIncomeTotal = 0;
    const personalIncomeBreakdown = {};
    Object.entries(personalIncome || {}).forEach(([memberId, yearData]) => {
      const monthData = yearData?.[year] || {};
      const sum = Object.values(monthData).reduce((s, v) => s + roundInt(v), 0);
      if (sum > 0) { personalIncomeBreakdown[memberId] = sum; personalIncomeTotal += sum; }
    });

    const policyList = Object.values(policies || {});
    let yearlyInsuranceTotal = 0, monthlyInsuranceAverage = 0, activePolicyCount = 0;
    const curY = Number(year), curM = Number(month);

    policyList.forEach((p) => {
      const firstY = Number(p.firstStartYear) || 0;
      const firstM = Number(p.firstStartMonth) || 1;
      const totalYears = Number(p.totalPolicyYears) || 0;
      if (!firstY) return;
      const totalMonths = (curY - firstY) * 12 + (curM - firstM);
      if (totalMonths < 0) return;
      const periodIndex = Math.floor(totalMonths / 12) + 1;
      if (totalYears > 0 && periodIndex > totalYears) return;

      if (p.type === 'fund_insurance') {
        const mp = roundInt(p.monthlyPremium);
        if (mp <= 0) return;
        monthlyInsuranceAverage += mp;
        yearlyInsuranceTotal += mp * 12;
        activePolicyCount++;
        return;
      }
      const periodData = (p.periods || {})[String(periodIndex)];
      if (periodData) {
        monthlyInsuranceAverage += roundInt(periodData.monthlyAverage);
        yearlyInsuranceTotal += roundInt(periodData.annualPremium);
        activePolicyCount++;
      }
    });

    const paymentBreakdown = {};
    Object.values(perMember).forEach((m) => {
      (m.items || []).forEach((it) => {
        const pmName = it.paymentMethodName || '（未指定）';
        paymentBreakdown[pmName] = (paymentBreakdown[pmName] || 0) + it.amount;
      });
    });
    fixedList.forEach((f) => {
      const pmName = f.paymentMethodName || '（未指定）';
      paymentBreakdown[pmName] = (paymentBreakdown[pmName] || 0) + f.amount;
    });
    if (monthlyInsuranceAverage > 0) {
      paymentBreakdown['（保險扣款）'] = (paymentBreakdown['（保險扣款）'] || 0) + monthlyInsuranceAverage;
    }

    const bankAccountsList = Object.entries(bankAccounts || {}).map(([id, acc]) => ({ id, ...acc }));
    const allTransactions = [];
    Object.entries(bankAccounts || {}).forEach(([bid, bankData]) => {
      Object.entries(bankData.transactions || {}).forEach(([txnId, txn]) => {
        allTransactions.push({ id: txnId, bankId: bid, ...txn });
      });
    });
    const currentBankTotal = _calcBankTotal(bankAccountsList, allTransactions, year, month);
    const prevBankTotal = _calcBankTotal(bankAccountsList, allTransactions, prevY, prevMonthStr);
    const fundValue = Object.values(funds || {}).reduce((s, f) => s + roundInt(f.currentValue), 0);
    const totalAssets = currentBankTotal + fundValue;
    const availableFunds = prevBankTotal + totalIncome;
    const netBalance = totalIncome - totalExpense;

    const advancesList = [];
    let totalAdvanceRemaining = 0;
    Object.entries(memberAdvances || {}).forEach(([memberId, advances]) => {
      Object.entries(advances || {}).forEach(([advanceId, adv]) => {
        const remaining = roundInt(adv.remainingAmount);
        totalAdvanceRemaining += remaining;
        advancesList.push({ id: advanceId, memberId, ...adv, remainingAmount: remaining });
      });
    });

    return jsonResponse({
      ok: true, year, month,
      totalIncome: roundInt(totalIncome),
      totalExpense: roundInt(totalExpense),
      netBalance: roundInt(netBalance),
      personalIncomeTotal: roundInt(personalIncomeTotal),
      personalIncomeBreakdown,
      yearlyInsuranceTotal: roundInt(yearlyInsuranceTotal),
      monthlyInsuranceAverage: roundInt(monthlyInsuranceAverage),
      policyCount: activePolicyCount,
      totalAssets: roundInt(totalAssets),
      bankBalance: roundInt(currentBankTotal),
      bankAccounts: bankAccountsList,
      fundValue: roundInt(fundValue),
      fundCount: Object.keys(funds || {}).length,
      prevBankTotal: roundInt(prevBankTotal),
      availableFunds: roundInt(availableFunds),
      bankCount: bankAccountsList.length,
      memberAdvances: advancesList,
      totalAdvanceRemaining: roundInt(totalAdvanceRemaining),
      fixedTotal: roundInt(fixedTotal),
      fixedPendingTotal: roundInt(fixedPendingTotal),
      fixedPendingCount: fixedPendingList.length,
      fixedList, perMember,
      memberCount: Object.keys(membersObj).length,
      incomeBreakdown, paymentBreakdown, statusMap,
      options: settingsOptions || null,
    });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 2：annual-summary
   ============================================ */
export async function handleAnnualSummary(request) {
  try {
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const startYear = Number(url.searchParams.get('startYear'));
    const endYear = Number(url.searchParams.get('endYear'));

    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (!startYear || !endYear || startYear > endYear) return errorResponse('MISSING_FIELDS', 'startYear / endYear 無效');
    if (endYear - startYear > 10) return errorResponse('BAD_REQUEST', '年度範圍不可超過 10 年');

    const auth = await authenticate(request, { needFamily: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}`;
    const [expensesSnap, incomeSnap, policiesSnap, bankAccountsSnap, fundsSnap] = await Promise.all([
      dbGet(`${basePath}/expenses`, token),
      dbGet(`${basePath}/income`, token),
      dbGet(`${basePath}/insurance_policies`, token),
      dbGet(`${basePath}/bank_accounts`, token),
      dbGet(`${basePath}/funds`, token),
    ]);

    const expensesObj = expensesSnap || {};
    const incomeObj = incomeSnap || {};
    const policiesObj = policiesSnap || {};
    const bankAccountsObj = bankAccountsSnap || {};
    const fundsObj = fundsSnap || {};

    const policyList = Object.entries(policiesObj).map(([id, p]) => ({ id, ...p }));
    const bankAccountsList = Object.entries(bankAccountsObj).map(([id, acc]) => ({
      id, name: acc.name || '', type: acc.type || 'family',
      initialBalance: Number(acc.initialBalance) || 0,
      initialYear: acc.initialYear || '', initialMonth: acc.initialMonth || '',
    }));
    const allTransactions = [];
    Object.entries(bankAccountsObj).forEach(([bid, bankData]) => {
      Object.entries(bankData.transactions || {}).forEach(([txnId, txn]) => {
        allTransactions.push({ id: txnId, bankId: bid, ...txn });
      });
    });

    const years = [];
    for (let y = startYear; y <= endYear; y++) {
      years.push(_buildYearSummary(y, expensesObj, incomeObj, policyList, bankAccountsList, allTransactions, fundsObj));
    }
    return jsonResponse({ ok: true, startYear, endYear, years });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 3：settlements-year
   ============================================ */
export async function handleSettlementsYear(request) {
  try {
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const year = url.searchParams.get('year');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (!year || !/^\d{4}$/.test(year)) return errorResponse('MISSING_FIELDS', 'year 格式錯誤（yyyy）');

    const auth = await authenticate(request, { needFamily: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}`;
    const [expensesSnap, policiesSnap, paymentsSnap] = await Promise.all([
      dbGet(`${basePath}/expenses/${year}`, token),
      dbGet(`${basePath}/insurance_policies`, token),
      dbGet(`${basePath}/insurance_payments`, token),
    ]);

    const expensesObj = expensesSnap || {};
    const policiesObj = policiesSnap || {};
    const paymentsObj = paymentsSnap || {};

    const memberExpensesByMonth = {};
    for (let m = 1; m <= 12; m++) {
      const mm = String(m).padStart(2, '0');
      const monthData = expensesObj[mm]?.member_expenses || {};
      const rows = [];
      Object.entries(monthData).forEach(([memberId, items]) => {
        Object.entries(items || {}).forEach(([id, e]) => {
          rows.push({
            id, memberId, name: e.name || '', amount: roundInt(e.amount),
            status: e.status || '', date: e.date || '',
            categoryId: e.categoryId || '', itemId: e.itemId || '',
            paymentMethodId: e.paymentMethodId || '',
            isAutoLinked: !!e.isAutoLinked, policyId: e.policyId || '',
            bankId: e.bankId || '', txnId: e.txnId || '',
            repaidDate: e.repaidDate || '', createdAt: e.createdAt || 0,
          });
        });
      });
      memberExpensesByMonth[mm] = rows;
    }

    const policies = Object.entries(policiesObj).map(([id, p]) => ({
      id,
      type: p.type || 'normal', name: p.name || '', company: p.company || '',
      memberId: p.memberId || '', policyHolderId: p.policyHolderId || '',
      paymentMode: p.paymentMode || 'direct',
      advanceHolderId: p.advanceHolderId || '',
      firstStartYear: Number(p.firstStartYear) || 0,
      firstStartMonth: String(p.firstStartMonth || '01').padStart(2, '0'),
      totalPolicyYears: Number(p.totalPolicyYears) || 0,
      currentPeriodIndex: Number(p.currentPeriodIndex) || 1,
      periods: p.periods || {},
      monthlyPremium: roundInt(p.monthlyPremium),
      monthlyAverage: roundInt(p.monthlyAverage),
      annualPremium: roundInt(p.annualPremium),
      isCompleted: !!p.isCompleted,
    }));

    const paymentsCache = {};
    policies.forEach((p) => {
      const pYearPayments = paymentsObj[p.id]?.[year] || {};
      paymentsCache[p.id] = { [year]: pYearPayments };
    });

    return jsonResponse({ ok: true, year, memberExpensesByMonth, policies, paymentsCache });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   內部工具
   ============================================ */
function _buildYearSummary(year, expensesObj, incomeObj, policyList, bankAccounts, allTransactions, fundsObj) {
  const yearStr = String(year);
  const yearExpenses = expensesObj[yearStr] || {};
  const yearIncome = incomeObj[yearStr] || {};
  const monthly = [];
  let yearTotalIncome = 0, yearTotalExpense = 0, monthsWithData = 0;

  for (let m = 1; m <= 12; m++) {
    const mm = String(m).padStart(2, '0');
    const monthIncomeObj = yearIncome[mm] || {};
    let totalIncome = 0;
    Object.values(monthIncomeObj).forEach((v) => { totalIncome += roundInt(v); });

    const monthExpenses = yearExpenses[mm]?.member_expenses || {};
    let totalExpense = 0;
    Object.values(monthExpenses).forEach((memberData) => {
      Object.values(memberData || {}).forEach((e) => { totalExpense += roundInt(e.amount); });
    });

    let monthlyIns = 0;
    policyList.forEach((p) => {
      const firstY = Number(p.firstStartYear) || 0;
      const firstM = Number(p.firstStartMonth) || 1;
      const totalYears = Number(p.totalPolicyYears) || 0;
      if (!firstY) return;
      const totalMonths = (year - firstY) * 12 + (m - firstM);
      if (totalMonths < 0) return;
      const periodIndex = Math.floor(totalMonths / 12) + 1;
      if (totalYears > 0 && periodIndex > totalYears) return;
      if (p.type === 'fund_insurance') {
        monthlyIns += roundInt(p.monthlyPremium);
      } else {
        const periodData = (p.periods || {})[String(periodIndex)];
        if (periodData) monthlyIns += roundInt(periodData.monthlyAverage);
      }
    });

    if (totalExpense > 0) monthsWithData++;
    monthly.push({
      monthNum: m, month: mm,
      totalIncome: roundInt(totalIncome),
      totalExpense: roundInt(totalExpense),
      netBalance: roundInt(totalIncome - totalExpense),
      monthlyInsuranceAverage: roundInt(monthlyIns),
    });
    yearTotalIncome += totalIncome;
    yearTotalExpense += totalExpense;
  }

  let yearlyInsTotal = 0, monthlyInsAvg = 0, policyCount = 0;
  policyList.forEach((p) => {
    const firstY = Number(p.firstStartYear) || 0;
    const totalYears = Number(p.totalPolicyYears) || 0;
    if (!firstY || year < firstY) return;
    const periodIndex = year - firstY + 1;
    if (totalYears > 0 && periodIndex > totalYears) return;
    if (p.type === 'fund_insurance') {
      const mp = roundInt(p.monthlyPremium);
      if (mp <= 0) return;
      yearlyInsTotal += mp * 12;
      monthlyInsAvg += mp;
      policyCount++;
    } else {
      const periodData = (p.periods || {})[String(periodIndex)];
      if (periodData) {
        yearlyInsTotal += roundInt(periodData.annualPremium);
        monthlyInsAvg += roundInt(periodData.monthlyAverage);
        policyCount++;
      }
    }
  });

  const bankBalance = _calcBankTotal(bankAccounts, allTransactions, year, '12');
  const fundValue = Object.values(fundsObj).reduce((s, f) => s + roundInt(f.currentValue), 0);

  return {
    year,
    totalIncome: roundInt(yearTotalIncome),
    totalExpense: roundInt(yearTotalExpense),
    netBalance: roundInt(yearTotalIncome - yearTotalExpense),
    yearlyInsuranceTotal: roundInt(yearlyInsTotal),
    monthlyInsuranceAverage: roundInt(monthlyInsAvg),
    policyCount,
    bankBalance: roundInt(bankBalance),
    fundValue: roundInt(fundValue),
    totalAssets: roundInt(bankBalance + fundValue),
    avg: monthsWithData > 0 ? Math.round(yearTotalExpense / monthsWithData) : 0,
    monthly,
  };
}

function _calcBankTotal(bankAccounts, transactions, targetYear, targetMonth) {
  if (!targetYear || !targetMonth) return 0;
  const tY = Number(targetYear);
  const tM = Number(targetMonth);
  let total = 0;

  bankAccounts.forEach((acc) => {
    const init = Number(acc.initialBalance) || 0;
    const initY = Number(acc.initialYear) || 0;
    const initM = Number(acc.initialMonth) || 0;
    if (!initY || !initM) { total += init; return; }
    if (tY < initY || (tY === initY && tM < initM)) { total += init; return; }

    let balance = init;
    transactions.filter((t) => t.bankId === acc.id).forEach((txn) => {
      const date = txn.date || '';
      if (!date || date.length < 7) return;
      const [y, m] = date.split('-').map(Number);
      const afterInit = y > initY || (y === initY && m > initM);
      const beforeTarget = y < tY || (y === tY && m <= tM);
      if (!afterInit || !beforeTarget) return;
      const amount = Number(txn.amount) || 0;
      if (txn.type === 'in') balance += amount;
      else if (txn.type === 'out') balance -= amount;
      else if (txn.type === 'transfer') balance -= amount;
    });
    total += balance;
  });

  return total;
}```

---

## 📄 index.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>總覽儀表板 | 家庭財務</title>
  <link rel="preconnect" href="https://www.gstatic.com" crossorigin>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">總覽儀表板</h1>
            <p class="page-subtitle" id="dashboard-subtitle">載入中…</p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <div id="stats-cards-root"></div>
        <div id="page-controls-root"></div>
        <div id="annual-overview-root"></div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/dashboard.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/dashboard.js';

    const user = await initApp({ activeHref: 'index.html', title: '總覽儀表板' });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: index.html
     Version: v103.0.32
     Batch: B26-6
     ═══════════════════════════════════════════ -->```

---

## 📄 input-center.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>綜合輸入中心 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">綜合輸入中心</h1>
            <p class="page-subtitle"></p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <div id="ic-buttons-root" class="flex flex-wrap gap-8 mb-16">
          <button type="button" class="btn btn-primary" data-action="add-expense">
            <i data-lucide="plus"></i> 新增支出
          </button>
          <button type="button" class="btn btn-ghost" data-action="add-contribution">
            <i data-lucide="trending-up"></i> 家用轉入
          </button>
          <button type="button" class="btn btn-ghost" data-action="add-personal-income">
            <i data-lucide="wallet"></i> 個人收入
          </button>
        </div>

        <div id="input-center-root">
          <div id="ic-tab-bar-root" class="mb-16"></div>
          <div id="ic-panel-expense" class="tab-panel" style="display:none;"></div>
          <div id="ic-panel-insurance" class="tab-panel" style="display:none;"></div>
          <div id="ic-panel-fund" class="tab-panel" style="display:none;"></div>
          <div id="ic-panel-bank" class="tab-panel" style="display:none;"></div>
        </div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/input-center.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/input-center.js';

    const user = await initApp({ activeHref: 'input-center.html', title: '綜合輸入中心' });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: input-center.html
     Version: 1792200000
     Batch: B35-0
     Writer: A20261016-001#002
     ═══════════════════════════════════════════ -->```

---

## 📄 insurance.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>保險清單表 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
  <link rel="stylesheet" href="./css/pages/insurance.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">保險清單表</h1>
            <p class="page-subtitle" id="insurance-month"></p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <div id="insurance-stats-root"></div>
        <div id="page-controls-root"></div>

        <div class="banner mb-16">
          ℹ️ 每張保單依「保單年度」自動產生每月分攤。此頁顯示保單清單與明細；
          扣款狀態請至「結算清單」編輯。
        </div>

        <div class="glass-card completed-section" id="completed-section" style="margin-bottom:20px; display:none;">
          <div class="completed-header" id="completed-header">
            <div class="flex items-center gap-10">
              <i data-lucide="check-circle" style="width:18px;height:18px;color:var(--neon-emerald);"></i>
              <span style="font-weight:700; color:var(--neon-emerald);">已供滿保單</span>
              <span class="badge badge-success" id="completed-count">0</span>
            </div>
            <i data-lucide="chevron-down" class="completed-arrow"></i>
          </div>
          <div class="completed-body" id="completed-body" style="display:none;">
            <div class="grid grid-3" id="completed-grid"></div>
          </div>
        </div>

        <div id="insurance-card-view"></div>
        <div id="insurance-table-view" style="display:none;"></div>

        <div class="glass-card" id="insurance-empty-state" style="display:none;">
          <div class="empty-state">
            <i data-lucide="shield" style="width:48px;height:48px;opacity:0.4;"></i>
            <p style="margin-top:12px;">尚無保單資料</p>
            <button type="button" class="btn btn-primary" id="add-first-policy-btn" style="margin-top:12px;">
              <i data-lucide="plus"></i> 新增第一張保單
            </button>
          </div>
        </div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/insurance.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/insurance.js';

    const user = await initApp({ activeHref: 'insurance.html', title: '保險清單表' });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: insurance.html
     Version: v103.0.29
     Batch: B26-3.2
     ═══════════════════════════════════════════ -->```

---

## 📄 js/admin/admin.js

```
// ============================================
// _admin.js — 平台管理 lib（v103.0.23）
// 位置：functions/api/_admin.js
// ============================================
// v103.0.23（B31-1）：
//   ✅ 移除 _migrate_check_expenses 死代碼
//   ✅ 3 個 POST handler 權限驗證提前（避免資訊洩漏）
// v103.0.22（B25-6）：
//   ✅ 加 handleAdminMigrate
// ============================================
import { dbGet, dbPut, dbDelete } from './_config.js';
import {
  authenticate, errorResponse, handleError,
  successResponse, requireFields,
} from './_helpers.js';

const LEGACY_STATUS_MAP = {
  '未處理': 'pending',
  '未還款': 'pending',
  '未付款': 'pending',
  '未扣款': 'pending',
  '未轉入': 'pending',
  '已處理': 'done',
  '已還款': 'done',
  '已付款': 'done',
  '已扣款': 'done',
  '已轉入': 'done',
  '不適用': 'skipped',
};

function _normalizeStatus(raw) {
  if (raw == null || raw === '') return 'pending';
  if (raw === 'pending' || raw === 'done' || raw === 'skipped') return raw;
  return LEGACY_STATUS_MAP[raw] || 'pending';
}

const FALLBACK = {
  members: {
    mem_husband:  { name: '成員1', role: 'husband', order: 0 },
    mem_wife:     { name: '成員2', role: 'wife',    order: 1 },
    mem_son:      { name: '成員3', role: 'child',   order: 2 },
    mem_daughter: { name: '成員4', role: 'child',   order: 3 },
  },
  banks: {},
  insurance_companies: {
    comp_ftlife:     { name: '富通', order: 1 },
    comp_prudential: { name: '保誠', order: 2 },
    comp_fwd:        { name: 'FWD',  order: 3 },
    comp_aia:        { name: 'AIA',  order: 4 },
    comp_manulife:   { name: '宏利', order: 5 },
    comp_axa:        { name: 'AXA',  order: 6 },
  },
  payment_methods: {
    pm_cash:   { name: '現金',   order: 1 },
    pm_boc:    { name: '中銀',   order: 2 },
    pm_hsbc:   { name: '匯豐',   order: 3 },
    pm_hangs:  { name: '恆生',   order: 4 },
    pm_credit: { name: '信用卡', order: 5 },
  },
  expense_categories: {
    cat_medical:   { name: '醫療類',     order: 1 },
    cat_school:    { name: '學校類',     order: 2 },
    cat_insurance: { name: '保險類',     order: 3 },
    cat_fixed:     { name: '固定費用類', order: 4 },
    cat_other:     { name: '其他',       order: 5 },
  },
  expense_items: {
    item_med_01: { categoryKey: 'cat_medical',   name: '看病-一般' },
    item_med_02: { categoryKey: 'cat_medical',   name: '看病-專科' },
    item_med_03: { categoryKey: 'cat_medical',   name: '牙醫' },
    item_med_04: { categoryKey: 'cat_medical',   name: '藥費' },
    item_sch_01: { categoryKey: 'cat_school',    name: '學費' },
    item_sch_02: { categoryKey: 'cat_school',    name: '功課輔導班' },
    item_sch_03: { categoryKey: 'cat_school',    name: '興趣班' },
    item_sch_04: { categoryKey: 'cat_school',    name: '書本費' },
    item_sch_05: { categoryKey: 'cat_school',    name: '校車費' },
    item_ins_01: { categoryKey: 'cat_insurance', name: '住院保險' },
    item_ins_02: { categoryKey: 'cat_insurance', name: '人壽保險' },
    item_ins_03: { categoryKey: 'cat_insurance', name: '意外保險' },
    item_fix_01: { categoryKey: 'cat_fixed',     name: '水費' },
    item_fix_02: { categoryKey: 'cat_fixed',     name: '電費' },
    item_fix_03: { categoryKey: 'cat_fixed',     name: '煤氣費' },
    item_fix_04: { categoryKey: 'cat_fixed',     name: '管理費' },
    item_fix_05: { categoryKey: 'cat_fixed',     name: '房租' },
    item_oth_01: { categoryKey: 'cat_other',     name: '其他' },
  },
  statuses: {
    status_untreated: { name: '未處理', category: 'personal',  isDone: false, order: 1 },
    status_done:      { name: '已處理', category: 'personal',  isDone: true,  order: 2 },
    status_unrepaid:  { name: '未還款', category: 'personal',  isDone: false, order: 3 },
    status_repaid:    { name: '已還款', category: 'personal',  isDone: true,  order: 4 },
    status_unpaid:    { name: '未付款', category: 'fixed',     isDone: false, order: 5 },
    status_paid:      { name: '已付款', category: 'fixed',     isDone: true,  order: 6 },
    status_na:        { name: '不適用', category: 'fixed',     isDone: true,  order: 7 },
    status_unbilled:  { name: '未扣款', category: 'insurance', isDone: false, order: 8 },
    status_billed:    { name: '已扣款', category: 'insurance', isDone: true,  order: 9 },
  },
  options: {
    memberRoles: [
      { value: 'husband', label: '老公 / 丈夫' },
      { value: 'wife',    label: '老婆 / 妻子' },
      { value: 'child',   label: '子女' },
      { value: 'other',   label: '其他' },
    ],
    cycles: [
      { value: '每月',    label: '每月' },
      { value: '每2個月', label: '每2個月' },
      { value: '每季',    label: '每季' },
      { value: '每年',    label: '每年' },
      { value: '一次性',  label: '一次性' },
    ],
    policyTypes: [
      { value: 'normal',         label: '普通保險（住院 / 人壽 / 意外）' },
      { value: 'fund_insurance', label: '基金保險（投資型，月供）' },
    ],
    insurancePaymentTypes: [
      { value: '年繳',     label: '年繳' },
      { value: '月繳',     label: '月繳' },
      { value: '一次付款', label: '一次付款' },
    ],
    categoryOrder: ['銀行類', '醫療類', '學校類', '保險類', '固定費用類', '交通類', '其他'],
  },
  year_range: { startYear: null, futureYears: 5 },
  ui_constants: { nameMaxLenDesktop: 12, nameMaxLenMobile: 6, toastDuration: 2000 },
};

/* ============================================
   Handler 1：admin-families
   ============================================ */
export async function handleAdminFamiliesGet(request) {
  try {
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const list = await dbGet('platform/families', token);
    const families = Object.entries(list || {}).map(([uid, data]) => ({ uid, ...data }));
    families.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0));
    return successResponse({ families });
  } catch (err) {
    return handleError(err);
  }
}

export async function handleAdminFamiliesPost(request) {
  try {
    /* 🆕 v103.0.23：權限驗證提前 */
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const body = await request.json();
    const { action, uid, name, email } = body || {};

    if (action === 'add') {
      const missing = requireFields(body, ['uid', 'name']);
      if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid 或 name');

      const success = await dbPut(`platform/families/${uid}`, {
        name: String(name).trim(),
        ownerEmail: email ? String(email).trim() : '',
        createdAt: Date.now(),
      }, token);

      if (!success) return errorResponse('INTERNAL', 'Firebase 寫入失敗');
      return successResponse();
    }

    if (action === 'remove') {
      const missing = requireFields(body, ['uid']);
      if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid');

      let accounts = null;
      try { accounts = await dbGet(`platform/families/${uid}/memberAccounts`, token); } catch (e) {}
      const cleanups = [];
      if (accounts && typeof accounts === 'object') {
        Object.entries(accounts).forEach(([memberUid, acc]) => {
          cleanups.push(dbDelete(`platform/uid_index/${memberUid}`, token).catch(() => false));
          if (acc && acc.account) {
            cleanups.push(dbDelete(`platform/email_index/${acc.account}`, token).catch(() => false));
          }
        });
      }
      await Promise.all(cleanups);

      const ok1 = await dbDelete(`platform/families/${uid}`, token);
      const ok2 = await dbDelete(`families/${uid}`, token);
      if (!ok1 && !ok2) return errorResponse('INTERNAL', '刪除失敗');

      return successResponse({
        deletedPlatform: ok1,
        deletedFamilyData: ok2,
        cleanedIndexes: cleanups.length,
      });
    }

    return errorResponse('BAD_REQUEST', '未知的 action');
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 2：admin-init-family
   ============================================ */
export async function handleAdminInitFamily(request) {
  try {
    /* 🆕 v103.0.23：權限驗證提前 */
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const body = await request.json();
    const { uid } = body || {};
    const missing = requireFields(body, ['uid']);
    if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid');

    const basePath = `families/${uid}`;
    const existing = await dbGet(`${basePath}/members`, token);
    if (existing && Object.keys(existing).length > 0) {
      return successResponse({ skipped: true, message: '此家庭已有資料' });
    }

    const defaults = await _loadPlatformDefaults(token);
    const now = Date.now();
    const results = {
      members: false, banks: false, insurance_companies: false,
      payment_methods: false, expense_categories: false, expense_items: false,
      statuses: false, options: false, year_range: false, ui_constants: false,
    };

    const membersData = _withTimestamps(defaults.members, now);
    results.members = Object.keys(membersData).length > 0
      ? await dbPut(`${basePath}/members`, membersData, token) : true;

    results.banks = Object.keys(defaults.banks).length > 0
      ? await dbPut(`${basePath}/banks`, _withTimestamps(defaults.banks, now), token) : true;
    results.insurance_companies = Object.keys(defaults.insurance_companies).length > 0
      ? await dbPut(`${basePath}/insurance_companies`, _withTimestamps(defaults.insurance_companies, now), token) : true;
    results.payment_methods = Object.keys(defaults.payment_methods).length > 0
      ? await dbPut(`${basePath}/payment_methods`, _withTimestamps(defaults.payment_methods, now), token) : true;
    results.expense_categories = Object.keys(defaults.expense_categories).length > 0
      ? await dbPut(`${basePath}/expense_categories`, _withTimestamps(defaults.expense_categories, now), token) : true;

    const itemsData = _buildItemsWithCategoryId(defaults.expense_items, now);
    results.expense_items = Object.keys(itemsData).length > 0
      ? await dbPut(`${basePath}/expense_items`, itemsData, token) : true;

    results.statuses = Object.keys(defaults.statuses).length > 0
      ? await dbPut(`${basePath}/statuses`, _withTimestamps(defaults.statuses, now), token) : true;

    results.options = (defaults.options && Object.keys(defaults.options).length > 0)
      ? await dbPut(`${basePath}/settings/options`, defaults.options, token) : true;
    results.year_range = defaults.year_range
      ? await dbPut(`${basePath}/settings/year_range`, defaults.year_range, token) : true;
    results.ui_constants = defaults.ui_constants
      ? await dbPut(`${basePath}/settings/ui_constants`, defaults.ui_constants, token) : true;

    const failedSteps = Object.entries(results).filter(([, ok]) => ok === false).map(([k]) => k);
    if (failedSteps.length > 0) {
      return successResponse({ created: true, partial: true, failedSteps, results });
    }
    return successResponse({ created: true, results });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   Handler 3：admin-migrate
   ============================================ */
export async function handleAdminMigrate(request) {
  try {
    /* 🆕 v103.0.23：權限驗證提前 */
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const body = await request.json();
    const { familyId, confirm } = body || {};
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (confirm !== 'MIGRATE_ALL') {
      return errorResponse('BAD_REQUEST', '必須提供確認參數 confirm="MIGRATE_ALL"');
    }

    const basePath = `families/${familyId}`;
    const summary = {
      expenses:    { scanned: 0, normalized: 0 },
      payments:    { scanned: 0, normalized: 0 },
      policies:    { scanned: 0, patched: 0 },
      funds:       { scanned: 0, patched: 0 },
    };

    /* ---------- 1. 遷移 expenses 狀態 ---------- */
    try {
      const expensesSnap = await dbGet(`${basePath}/expenses`, token);
      const expensesUpd = {};
      if (expensesSnap && typeof expensesSnap === 'object') {
        Object.entries(expensesSnap).forEach(([y, months]) => {
          Object.entries(months || {}).forEach(([m, monthData]) => {
            Object.entries(monthData?.member_expenses || {}).forEach(([mid, items]) => {
              Object.entries(items || {}).forEach(([eid, e]) => {
                if (!e || typeof e !== 'object') return;
                summary.expenses.scanned++;

                const patch = {};
                const newStatus = _normalizeStatus(e.status);
                if (e.status !== newStatus) {
                  patch.status = newStatus;
                }
                if (e.isAutoLinked === undefined) {
                  patch.isAutoLinked = !!e.policyId;
                }
                if (Object.keys(patch).length > 0) {
                  expensesUpd[`${basePath}/expenses/${y}/${m}/member_expenses/${mid}/${eid}`] = { ...e, ...patch };
                  summary.expenses.normalized++;
                }
              });
            });
          });
        });
        /* 🆕 v103.0.23：移除死代碼 _migrate_check_expenses */
        for (const [path, data] of Object.entries(expensesUpd)) {
          await dbPut(path, data, token);
        }
      }
    } catch (err) {
      console.warn('[admin-migrate] expenses 遷移失敗：', err);
    }

    /* ---------- 2. 遷移 insurance_payments 狀態 ---------- */
    try {
      const paymentsSnap = await dbGet(`${basePath}/insurance_payments`, token);
      if (paymentsSnap && typeof paymentsSnap === 'object') {
        for (const [pid, years] of Object.entries(paymentsSnap)) {
          for (const [y, months] of Object.entries(years || {})) {
            for (const [m, p] of Object.entries(months || {})) {
              if (!p || typeof p !== 'object') continue;
              summary.payments.scanned++;
              const newStatus = _normalizeStatus(p.status);
              if (p.status !== newStatus) {
                await dbPut(`${basePath}/insurance_payments/${pid}/${y}/${m}`, { ...p, status: newStatus }, token);
                summary.payments.normalized++;
              }
            }
          }
        }
      }
    } catch (err) {
      console.warn('[admin-migrate] payments 遷移失敗：', err);
    }

    /* ---------- 3. 遷移 insurance_policies ---------- */
    try {
      const policiesSnap = await dbGet(`${basePath}/insurance_policies`, token);
      if (policiesSnap && typeof policiesSnap === 'object') {
        for (const [pid, p] of Object.entries(policiesSnap)) {
          if (!p || typeof p !== 'object') continue;
          summary.policies.scanned++;
          const patch = {};
          if (!Array.isArray(p.fundsAllocation)) patch.fundsAllocation = [];
          if (p.type === undefined) patch.type = 'normal';
          if (Object.keys(patch).length > 0) {
            await dbPut(`${basePath}/insurance_policies/${pid}`, { ...p, ...patch }, token);
            summary.policies.patched++;
          }
        }
      }
    } catch (err) {
      console.warn('[admin-migrate] policies 遷移失敗：', err);
    }

    /* ---------- 4. 遷移 funds ---------- */
    try {
      const fundsSnap = await dbGet(`${basePath}/funds`, token);
      if (fundsSnap && typeof fundsSnap === 'object') {
        for (const [fid, f] of Object.entries(fundsSnap)) {
          if (!f || typeof f !== 'object') continue;
          summary.funds.scanned++;
          const patch = {};
          if (f.type === undefined) patch.type = 'standalone';
          if (f.policyId === undefined) patch.policyId = '';
          if (f.initialYear === undefined) patch.initialYear = '';
          if (f.initialMonth === undefined) patch.initialMonth = '';
          if (Object.keys(patch).length > 0) {
            await dbPut(`${basePath}/funds/${fid}`, { ...f, ...patch }, token);
            summary.funds.patched++;
          }
        }
      }
    } catch (err) {
      console.warn('[admin-migrate] funds 遷移失敗：', err);
    }

    return successResponse({ migrated: true, familyId, summary });
  } catch (err) {
    return handleError(err);
  }
}

/* ============================================
   內部工具
   ============================================ */
async function _loadPlatformDefaults(token) {
  const [members, banks, companies, payments, categories, items, statuses, options, yearRange, uiConstants] =
    await Promise.all([
      dbGet('platform/defaults/members', token),
      dbGet('platform/defaults/banks', token),
      dbGet('platform/defaults/insurance_companies', token),
      dbGet('platform/defaults/payment_methods', token),
      dbGet('platform/defaults/expense_categories', token),
      dbGet('platform/defaults/expense_items', token),
      dbGet('platform/defaults/statuses', token),
      dbGet('platform/defaults/options', token),
      dbGet('platform/defaults/year_range', token),
      dbGet('platform/defaults/ui_constants', token),
    ]);

  return {
    members:             members    && Object.keys(members).length    ? members    : FALLBACK.members,
    banks:               banks      && Object.keys(banks).length      ? banks      : FALLBACK.banks,
    insurance_companies: companies  && Object.keys(companies).length  ? companies  : FALLBACK.insurance_companies,
    payment_methods:     payments   && Object.keys(payments).length   ? payments   : FALLBACK.payment_methods,
    expense_categories:  categories && Object.keys(categories).length ? categories : FALLBACK.expense_categories,
    expense_items:       items      && Object.keys(items).length      ? items      : FALLBACK.expense_items,
    statuses:            statuses   && Object.keys(statuses).length   ? statuses   : FALLBACK.statuses,
    options:             options    && Object.keys(options).length    ? options    : FALLBACK.options,
    year_range:          yearRange  || FALLBACK.year_range,
    ui_constants:        uiConstants || FALLBACK.ui_constants,
  };
}

function _withTimestamps(data, baseTs) {
  const out = {};
  let i = 0;
  Object.entries(data || {}).forEach(([key, val]) => {
    out[key] = { ...val, createdAt: val.createdAt || (baseTs + i) };
    i++;
  });
  return out;
}

function _buildItemsWithCategoryId(items, baseTs) {
  const out = {};
  let i = 0;
  Object.entries(items || {}).forEach(([key, val]) => {
    const categoryId = val.categoryKey || val.categoryId || '';
    out[key] = {
      name: val.name || '',
      categoryId,
      createdAt: val.createdAt || (baseTs + i),
    };
    i++;
  });
  return out;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: functions/api/_admin.js
   Version: v103.0.23
   Batch: B31-1
   ═══════════════════════════════════════════ */
```

---

## 📄 js/admin/platform-defaults.js

```
// ============================================
// platform-defaults.js — 平台預設資料庫（v103.0.2）
// 位置：js/admin/platform-defaults.js
// ============================================
// v103.0.2 修正：
//   ✅ 從 '../shared/toast.js' 改為 '../ui/toast.js'
//   ✅ 從 '../shared/tab-panel.js' 改為 '../ui/tab-panel.js'
//   ✅ 從 '../shared/form-builder.js' 改為 '../ui/form-builder.js'
//   ✅ 從 '../shared/modal.js' 改為 '../ui/modal.js'
// ============================================

import { api } from '../core/api.js';
import { escapeHtml } from '../core/utils.js';
import { showToast } from '../ui/toast.js';
import { initTabPanel } from '../ui/tab-panel.js';
import { buildForm } from '../ui/form-builder.js';
import { openModal, closeModal, openConfirm } from '../ui/modal.js';

/* ============================================
   Module 狀態
   ============================================ */
let _container = null;
let _tabPanel = null;

const _instances = {};

const EDIT_MODAL_ID = 'pd-edit-modal';

/* ============================================
   泛型清單 Tab 定義
   ============================================ */
const LIST_TABS = [
  {
    key: 'members',
    label: '成員',
    icon: 'users',
    resource: 'members',
    fields: [
      { id: 'name', label: '名稱', type: 'text', required: true, maxlength: 20 },
      { id: 'role', label: '角色', type: 'select', required: true,
        options: [
          { value: 'husband', label: '老公 / 丈夫' },
          { value: 'wife', label: '老婆 / 妻子' },
          { value: 'child', label: '子女' },
          { value: 'other', label: '其他' },
        ],
        includeEmpty: false,
      },
      { id: 'order', label: '排序', type: 'number', min: 0 },
    ],
    displayColumns: ['name', 'role', 'order'],
  },
  {
    key: 'banks',
    label: '銀行',
    icon: 'landmark',
    resource: 'banks',
    fields: [
      { id: 'name', label: '銀行名稱', type: 'text', required: true, maxlength: 20 },
      { id: 'order', label: '排序', type: 'number', min: 0 },
    ],
    displayColumns: ['name', 'order'],
  },
  {
    key: 'companies',
    label: '保險公司',
    icon: 'shield',
    resource: 'companies',
    fields: [
      { id: 'name', label: '公司名稱', type: 'text', required: true, maxlength: 30 },
      { id: 'order', label: '排序', type: 'number', min: 0 },
    ],
    displayColumns: ['name', 'order'],
  },
  {
    key: 'payments',
    label: '支付方式',
    icon: 'credit-card',
    resource: 'payments',
    fields: [
      { id: 'name', label: '支付方式名稱', type: 'text', required: true, maxlength: 20 },
      { id: 'order', label: '排序', type: 'number', min: 0 },
    ],
    displayColumns: ['name', 'order'],
  },
  {
    key: 'statuses',
    label: '狀態',
    icon: 'tag',
    resource: 'statuses',
    fields: [
      { id: 'name', label: '狀態名稱', type: 'text', required: true, maxlength: 20 },
      {
        id: 'category', label: '所屬類別', type: 'select', required: true, includeEmpty: false,
        options: [
          { value: 'personal', label: '個人支出' },
          { value: 'fixed', label: '固定支出' },
          { value: 'insurance', label: '保險' },
        ],
      },
      {
        id: 'isDone', label: '是否為「已完成」', type: 'select', required: true, includeEmpty: false,
        options: [
          { value: 'false', label: '否（未處理類）' },
          { value: 'true', label: '是（已完成類）' },
        ],
      },
      { id: 'order', label: '排序', type: 'number', min: 0 },
    ],
    displayColumns: ['name', 'category', 'order'],
    valueTransform: {
      toForm: (data) => ({ ...data, isDone: data.isDone ? 'true' : 'false' }),
      fromForm: (data) => ({ ...data, isDone: data.isDone === 'true' }),
    },
  },
];

/* ============================================
   Tab 定義（9 個）
   ============================================ */
const ALL_TABS = [
  { key: 'members',    label: '成員',       icon: 'users' },
  { key: 'banks',      label: '銀行',       icon: 'landmark' },
  { key: 'companies',  label: '保險公司',   icon: 'shield' },
  { key: 'payments',   label: '支付方式',   icon: 'credit-card' },
  { key: 'categories', label: '支出結構',   icon: 'tags' },
  { key: 'statuses',   label: '狀態',       icon: 'tag' },
  { key: 'options',    label: '下拉選項',   icon: 'list-ordered' },
  { key: 'yearRange',  label: '年份範圍',   icon: 'calendar' },
  { key: 'uiConstants',label: 'UI 常數',    icon: 'palette' },
];

/* ============================================
   主入口
   ============================================ */
export function initPlatformDefaults(containerId) {
  _container = document.getElementById(containerId);
  if (!_container) {
    console.warn(`⚠️ initPlatformDefaults: 找不到容器 #${containerId}`);
    return null;
  }

  _container.innerHTML = `
    <div id="pd-tabs-root"></div>
    ${ALL_TABS.map((t) => `
      <div id="pd-panel-${t.key}" class="tab-panel" style="display:none;"></div>
    `).join('')}
  `;

  _tabPanel = initTabPanel({
    containerId: 'pd-tabs-root',
    tabs: ALL_TABS.map((t) => ({
      key: t.key,
      label: t.label,
      icon: t.icon,
      panelId: `pd-panel-${t.key}`,
    })),
    defaultKey: 'members',
    storageKey: 'pd-tab',
    wrap: true,
    onChange: (key) => _activateTab(key),
  });

  if (_tabPanel) {
    _activateTab(_tabPanel.getCurrent());
  }

  return {
    refresh: () => {
      const key = _tabPanel?.getCurrent();
      if (key) _activateTab(key, true);
    },
    destroy: _destroy,
  };
}

/* ============================================
   啟用 Tab
   ============================================ */
async function _activateTab(key, force = false) {
  const panel = document.getElementById(`pd-panel-${key}`);
  if (!panel) return;

  if (!force && panel.dataset.loaded === '1' && _instances[key]?.refresh) {
    try { _instances[key].refresh(); } catch (e) { /* noop */ }
    return;
  }

  panel.dataset.loaded = '1';
  panel.innerHTML = `<div class="empty-state">載入中…</div>`;

  try {
    const listTab = LIST_TABS.find((t) => t.key === key);
    if (listTab) {
      _instances[key] = await _renderListTab(panel, listTab);
      return;
    }

    switch (key) {
      case 'categories':
        _instances[key] = await _renderCategoriesTab(panel);
        break;
      case 'options':
        _instances[key] = await _renderOptionsTab(panel);
        break;
      case 'yearRange':
        _instances[key] = await _renderYearRangeTab(panel);
        break;
      case 'uiConstants':
        _instances[key] = await _renderUIConstantsTab(panel);
        break;
      default:
        panel.innerHTML = `<div class="banner banner-error">未知的 Tab：${key}</div>`;
    }
  } catch (err) {
    console.error(`[platform-defaults] 載入 ${key} 失敗：`, err);
    panel.innerHTML = `<div class="banner banner-error">載入失敗：${escapeHtml(err.message)}</div>`;
  }
}

/* ============================================
   泛型清單 Tab
   ============================================ */
async function _renderListTab(panel, tabConfig) {
  panel.innerHTML = `
    <div id="pd-${tabConfig.key}-form-root" class="mb-16"></div>
    <div class="glass-card collapsible-card collapsible-card-flat" style="padding:0;">
      <div class="collapsible-header">
        <div class="collapsible-header-title">
          <i data-lucide="${tabConfig.icon}" style="width:16px;height:16px;"></i>
          <span>${escapeHtml(tabConfig.label)}清單 <span class="text-muted" id="pd-${tabConfig.key}-count" style="font-size:12px; margin-left:6px;"></span></span>
        </div>
      </div>
      <div class="collapsible-body" style="display:block;">
        <div id="pd-${tabConfig.key}-list"></div>
      </div>
    </div>
  `;

  const formApi = buildForm({
    containerId: `pd-${tabConfig.key}-form-root`,
    fields: _buildFormFields(tabConfig.fields),
    submitText: `新增${tabConfig.label}`,
    showCancel: false,
    showReset: true,
    resetText: '重置',
    onSubmit: async (data) => {
      const clean = _cleanFormData(data, tabConfig);
      try {
        await api.platformDefaults.put(tabConfig.resource, null, clean);
        showToast(`✅ 已新增`, 'success');
        formApi.reset();
        await refresh();
      } catch (err) {
        showToast('新增失敗：' + err.message, 'error');
      }
    },
  });

  let _list = [];
  let _listHandler = null;

  async function load() {
    try {
      const result = await api.platformDefaults.list(tabConfig.resource);
      _list = result.list || [];
    } catch (err) {
      _list = [];
    }
    renderList();
  }

  function renderList() {
    const listEl = document.getElementById(`pd-${tabConfig.key}-list`);
    const countEl = document.getElementById(`pd-${tabConfig.key}-count`);
    if (!listEl) return;

    if (countEl) countEl.textContent = `（共 ${_list.length} 筆）`;

    if (_list.length === 0) {
      listEl.innerHTML = `<div class="empty-state">尚無資料</div>`;
      return;
    }

    listEl.innerHTML = `
      <div style="overflow-x:auto;">
        <table class="data-table">
          <thead>
            <tr>
              ${tabConfig.displayColumns.map((col) => {
                const field = tabConfig.fields.find((f) => f.id === col);
                return `<th>${escapeHtml(field?.label || col)}</th>`;
              }).join('')}
              <th style="width:130px;">操作</th>
            </tr>
          </thead>
          <tbody>
            ${_list.map((row) => `
              <tr data-id="${row.id}">
                ${tabConfig.displayColumns.map((col) => {
                  const val = row[col];
                  return `<td>${_renderCellValue(val, col, tabConfig)}</td>`;
                }).join('')}
                <td>
                  <button type="button" class="btn btn-sm btn-ghost" data-action="edit" data-id="${row.id}">編輯</button>
                  <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-id="${row.id}">刪除</button>
                </td>
              </tr>
            `).join('')}
          </tbody>
        </table>
      </div>
    `;

    if (window.lucide) window.lucide.createIcons();
  }

  _listHandler = async (e) => {
    const btn = e.target.closest('button[data-action]');
    if (!btn) return;
    const id = btn.dataset.id;
    const action = btn.dataset.action;
    const row = _list.find((x) => x.id === id);
    if (!row) return;

    if (action === 'edit') {
      await _openEditModal({
        title: `編輯「${tabConfig.label}」`,
        resource: tabConfig.resource,
        id: row.id,
        fields: tabConfig.fields,
        initialData: tabConfig.valueTransform?.toForm ? tabConfig.valueTransform.toForm(row) : row,
        valueTransform: tabConfig.valueTransform,
        onSave: refresh,
      });
    } else if (action === 'delete') {
      const ok = await openConfirm(
        `確定要刪除「${row.name || id}」嗎？\n\n（不會影響已存在的家庭）`,
        { title: '刪除', okText: '刪除', okClass: 'btn-danger' }
      );
      if (!ok) return;
      try {
        await api.platformDefaults.remove(tabConfig.resource, id);
        showToast('✅ 已刪除', 'success');
        await refresh();
      } catch (err) {
        showToast('刪除失敗：' + err.message, 'error');
      }
    }
  };
  panel.addEventListener('click', _listHandler);

  async function refresh() {
    await load();
  }

  await load();

  return {
    refresh,
    destroy: () => {
      if (_listHandler) {
        panel.removeEventListener('click', _listHandler);
        _listHandler = null;
      }
      formApi.destroy();
    },
  };
}

/* ============================================
   統一編輯 Modal
   ============================================ */
async function _openEditModal({ title, resource, id, fields, initialData, valueTransform, onSave }) {
  let overlay = document.getElementById(EDIT_MODAL_ID);
  if (overlay) overlay.remove();

  overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = EDIT_MODAL_ID;
  overlay.innerHTML = `
    <div class="modal" style="max-width:480px;">
      <h2 class="modal-title">${escapeHtml(title)}</h2>
      <div id="pd-edit-modal-form-root"></div>
    </div>
  `;
  document.body.appendChild(overlay);

  const formApi = buildForm({
    containerId: 'pd-edit-modal-form-root',
    fields: _buildFormFields(fields),
    submitText: '儲存',
    showCancel: true,
    cancelText: '取消',
    onSubmit: async (data) => {
      const clean = _cleanFormData(data, { fields, valueTransform });
      try {
        await api.platformDefaults.put(resource, id, clean);
        showToast('✅ 已更新', 'success');
        closeModal(EDIT_MODAL_ID);
        if (onSave) await onSave();
      } catch (err) {
        showToast('更新失敗：' + err.message, 'error');
      }
    },
    onCancel: () => closeModal(EDIT_MODAL_ID),
  });

  const formData = {};
  fields.forEach((f) => {
    formData[`pd-${f.id}`] = initialData?.[f.id] ?? '';
  });
  formApi.setData(formData);

  overlay.addEventListener('click', (e) => {
    if (e.target === overlay) closeModal(EDIT_MODAL_ID);
  });

  openModal(EDIT_MODAL_ID);
}

/* ============================================
   表單欄位轉換
   ============================================ */
function _buildFormFields(fields) {
  return fields.map((f) => ({
    type: f.type,
    id: `pd-${f.id}`,
    label: f.label,
    required: f.required,
    maxlength: f.maxlength,
    min: f.min,
    max: f.max,
    step: f.step,
    options: f.options,
    includeEmpty: f.includeEmpty !== undefined ? f.includeEmpty : true,
    emptyText: f.emptyText || '— 請選擇 —',
    placeholder: f.placeholder,
    hint: f.hint,
  }));
}

function _cleanFormData(data, tabConfig) {
  const clean = {};
  tabConfig.fields.forEach((f) => {
    const key = `pd-${f.id}`;
    let val = data[key];

    if (f.type === 'number') {
      val = Number(val) || 0;
    } else if (f.type === 'text') {
      val = String(val || '').trim();
    }

    clean[f.id] = val;
  });

  if (tabConfig.valueTransform?.fromForm) {
    return tabConfig.valueTransform.fromForm(clean);
  }

  return clean;
}

function _renderCellValue(val, col, tabConfig) {
  if (val == null || val === '') return '<span class="text-muted">—</span>';

  if (typeof val === 'boolean') {
    return val ? '<span class="badge badge-success">是</span>' : '<span class="badge badge-muted">否</span>';
  }

  const field = tabConfig.fields.find((f) => f.id === col);
  if (field?.type === 'select' && field.options) {
    const opt = field.options.find((o) => String(o.value) === String(val));
    if (opt) return escapeHtml(opt.label);
  }

  return escapeHtml(String(val));
}

/* ============================================
   支出結構 Tab
   ============================================ */
async function _renderCategoriesTab(panel) {
  panel.innerHTML = `
    <div class="grid grid-2" style="gap:16px; align-items:start;">
      <div>
        <div id="pd-cat-form-root" class="mb-16"></div>
        <div class="glass-card collapsible-card collapsible-card-flat" style="padding:0;">
          <div class="collapsible-header">
            <div class="collapsible-header-title">
              <i data-lucide="tags" style="width:16px;height:16px;"></i>
              <span>支出類別 <span class="text-muted" id="pd-cat-count" style="font-size:12px; margin-left:6px;"></span></span>
            </div>
          </div>
          <div class="collapsible-body" style="display:block;">
            <div id="pd-cat-list"></div>
          </div>
        </div>
      </div>
      <div>
        <div id="pd-item-form-root" class="mb-16"></div>
        <div class="glass-card collapsible-card collapsible-card-flat" style="padding:0;">
          <div class="collapsible-header">
            <div class="collapsible-header-title">
              <i data-lucide="list" style="width:16px;height:16px;"></i>
              <span>支出項目 <span class="text-muted" id="pd-item-count" style="font-size:12px; margin-left:6px;"></span></span>
            </div>
          </div>
          <div class="collapsible-body" style="display:block;">
            <div id="pd-item-list"></div>
          </div>
        </div>
      </div>
    </div>
  `;

  let _categories = [];
  let _items = [];
  let _clickHandler = null;

  const catFormApi = buildForm({
    containerId: 'pd-cat-form-root',
    fields: [
      { type: 'text',   id: 'pd-cat-name',  label: '類別名稱', required: true, maxlength: 20 },
      { type: 'number', id: 'pd-cat-order', label: '排序', min: 0 },
    ],
    submitText: '新增類別',
    showCancel: false,
    showReset: true,
    resetText: '重置',
    onSubmit: async (data) => {
      const clean = {
        name: String(data['pd-cat-name'] || '').trim(),
        order: Number(data['pd-cat-order']) || 0,
      };
      if (!clean.name) return { field: 'pd-cat-name', message: '請填寫名稱' };
      try {
        await api.platformDefaults.put('categories', null, clean);
        showToast('✅ 已新增類別', 'success');
        catFormApi.reset();
        await refresh();
      } catch (err) {
        showToast('新增失敗：' + err.message, 'error');
      }
    },
  });

  const itemFormApi = buildForm({
    containerId: 'pd-item-form-root',
    fields: [
      { type: 'select', id: 'pd-item-cat',  label: '所屬類別', required: true, includeEmpty: true, emptyText: '— 請選擇 —' },
      { type: 'text',   id: 'pd-item-name', label: '項目名稱', required: true, maxlength: 30 },
    ],
    submitText: '新增項目',
    showCancel: false,
    showReset: true,
    resetText: '重置',
    onSubmit: async (data) => {
      const clean = {
        name: String(data['pd-item-name'] || '').trim(),
        categoryKey: data['pd-item-cat'] || '',
      };
      if (!clean.name || !clean.categoryKey) {
        return { field: 'pd-item-name', message: '請填寫完整' };
      }
      try {
        await api.platformDefaults.put('items', null, clean);
        showToast('✅ 已新增項目', 'success');
        itemFormApi.reset();
        await refresh();
      } catch (err) {
        showToast('新增失敗：' + err.message, 'error');
      }
    },
  });

  async function load() {
    try {
      const [catRes, itemRes] = await Promise.all([
        api.platformDefaults.list('categories'),
        api.platformDefaults.list('items'),
      ]);
      _categories = catRes.list || [];
      _items = itemRes.list || [];
    } catch (err) {
      _categories = [];
      _items = [];
    }

    itemFormApi.updateOptions('pd-item-cat',
      _categories.map((c) => ({ value: c.id, label: c.name })),
      { includeEmpty: true, emptyText: '— 請選擇 —' }
    );

    renderCategories();
    renderItems();
  }

  function renderCategories() {
    const listEl = document.getElementById('pd-cat-list');
    const countEl = document.getElementById('pd-cat-count');
    if (!listEl) return;
    if (countEl) countEl.textContent = `（${_categories.length}）`;

    if (_categories.length === 0) {
      listEl.innerHTML = `<div class="empty-state">尚無類別</div>`;
      return;
    }

    listEl.innerHTML = `
      <div style="overflow-x:auto;">
        <table class="data-table">
          <thead><tr><th>名稱</th><th class="num" style="width:60px;">排序</th><th style="width:130px;">操作</th></tr></thead>
          <tbody>
            ${_categories.map((c) => `
              <tr data-id="${c.id}">
                <td>${escapeHtml(c.name)}</td>
                <td class="num">${c.order || 0}</td>
                <td>
                  <button type="button" class="btn btn-sm btn-ghost" data-action="edit-cat" data-id="${c.id}">編輯</button>
                  <button type="button" class="btn btn-sm btn-danger" data-action="del-cat" data-id="${c.id}">刪除</button>
                </td>
              </tr>
            `).join('')}
          </tbody>
        </table>
      </div>
    `;
    if (window.lucide) window.lucide.createIcons();
  }

  function renderItems() {
    const listEl = document.getElementById('pd-item-list');
    const countEl = document.getElementById('pd-item-count');
    if (!listEl) return;
    if (countEl) countEl.textContent = `（${_items.length}）`;

    if (_items.length === 0) {
      listEl.innerHTML = `<div class="empty-state">尚無項目</div>`;
      return;
    }

    listEl.innerHTML = `
      <div style="overflow-x:auto;">
        <table class="data-table">
          <thead><tr><th>名稱</th><th style="width:100px;">類別</th><th style="width:130px;">操作</th></tr></thead>
          <tbody>
            ${_items.map((it) => {
              const cat = _categories.find((c) => c.id === it.categoryKey);
              return `
                <tr data-id="${it.id}">
                  <td>${escapeHtml(it.name)}</td>
                  <td><span class="badge badge-info">${escapeHtml(cat?.name || '—')}</span></td>
                  <td>
                    <button type="button" class="btn btn-sm btn-ghost" data-action="edit-item" data-id="${it.id}">編輯</button>
                    <button type="button" class="btn btn-sm btn-danger" data-action="del-item" data-id="${it.id}">刪除</button>
                  </td>
                </tr>
              `;
            }).join('')}
          </tbody>
        </table>
      </div>
    `;
    if (window.lucide) window.lucide.createIcons();
  }

  _clickHandler = async (e) => {
    const btn = e.target.closest('button[data-action]');
    if (!btn) return;
    const id = btn.dataset.id;
    const action = btn.dataset.action;

    switch (action) {
      case 'edit-cat': {
        const cat = _categories.find((c) => c.id === id);
        if (!cat) return;
        await _openEditModal({
          title: '編輯類別',
          resource: 'categories',
          id: cat.id,
          fields: [
            { type: 'text',   id: 'name',  label: '類別名稱', required: true, maxlength: 20 },
            { type: 'number', id: 'order', label: '排序', min: 0 },
          ],
          initialData: { name: cat.name, order: cat.order || 0 },
          onSave: refresh,
        });
        break;
      }
      case 'del-cat': {
        const cat = _categories.find((c) => c.id === id);
        if (!cat) return;
        const used = _items.filter((i) => i.categoryKey === id);
        if (used.length > 0) {
          showToast(`無法刪除：此類別下還有 ${used.length} 個項目`, 'warning');
          return;
        }
        const ok = await openConfirm(`確定要刪除類別「${cat.name}」嗎？`, { okText: '刪除', okClass: 'btn-danger' });
        if (!ok) return;
        try {
          await api.platformDefaults.remove('categories', id);
          showToast('✅ 已刪除', 'success');
          await refresh();
        } catch (err) {
          showToast('刪除失敗：' + err.message, 'error');
        }
        break;
      }
      case 'edit-item': {
        const it = _items.find((x) => x.id === id);
        if (!it) return;
        await _openEditModal({
          title: '編輯項目',
          resource: 'items',
          id: it.id,
          fields: [
            {
              type: 'select', id: 'categoryKey', label: '所屬類別', required: true, includeEmpty: true,
              options: _categories.map((c) => ({ value: c.id, label: c.name })),
            },
            { type: 'text', id: 'name', label: '項目名稱', required: true, maxlength: 30 },
          ],
          initialData: { categoryKey: it.categoryKey || '', name: it.name || '' },
          onSave: refresh,
        });
        break;
      }
      case 'del-item': {
        const it = _items.find((x) => x.id === id);
        if (!it) return;
        const ok = await openConfirm(`確定要刪除項目「${it.name}」嗎？`, { okText: '刪除', okClass: 'btn-danger' });
        if (!ok) return;
        try {
          await api.platformDefaults.remove('items', id);
          showToast('✅ 已刪除', 'success');
          await refresh();
        } catch (err) {
          showToast('刪除失敗：' + err.message, 'error');
        }
        break;
      }
    }
  };
  panel.addEventListener('click', _clickHandler);

  async function refresh() {
    await load();
  }

  await load();

  return {
    refresh,
    destroy: () => {
      if (_clickHandler) {
        panel.removeEventListener('click', _clickHandler);
        _clickHandler = null;
      }
      catFormApi.destroy();
      itemFormApi.destroy();
    },
  };
}

/* ============================================
   下拉選項 Tab
   ============================================ */
async function _renderOptionsTab(panel) {
  const GROUPS = [
    { key: 'memberRoles', label: '成員角色', type: 'value-label' },
    { key: 'cycles', label: '付款週期', type: 'value-label' },
    { key: 'policyTypes', label: '保單類型', type: 'value-label' },
    { key: 'insurancePaymentTypes', label: '保險付款類型', type: 'value-label' },
    { key: 'categoryOrder', label: '類別順序', type: 'string' },
  ];

  let _options = {};
  let _clickHandler = null;

  panel.innerHTML = `
    <div class="banner" style="margin-bottom:16px;">
      ℹ️ 此處定義「新家庭初始化時」的下拉選項預設值。
    </div>
    <div id="pd-options-list" class="grid grid-2" style="gap:16px; align-items:start;"></div>
  `;

  async function load() {
    try {
      const result = await api.platformDefaults.list('options');
      _options = result.data || {};
    } catch (err) {
      _options = {};
    }
    render();
  }

  function render() {
    const listEl = document.getElementById('pd-options-list');
    if (!listEl) return;

    listEl.innerHTML = GROUPS.map((g) => _renderGroupCard(g)).join('');

    if (window.lucide) window.lucide.createIcons();
  }

  function _renderGroupCard(group) {
    const list = _options[group.key] || [];
    const rows = list.map((item, i) => {
      const main = group.type === 'value-label'
        ? `<div style="font-weight:500;">${escapeHtml(item.label || '')}</div>
           <div style="font-size:11px; color:var(--text-muted); font-family:var(--font-mono);">${escapeHtml(item.value || '')}</div>`
        : `<div style="font-weight:500;">${escapeHtml(item)}</div>`;

      return `
        <div style="display:flex; align-items:center; gap:8px; padding:8px 10px; border-bottom:1px solid rgba(255,255,255,0.04);">
          <div style="flex:1; min-width:0;">${main}</div>
          <div style="display:flex; gap:2px;">
            <button type="button" class="btn btn-sm btn-ghost" data-action="edit-opt" data-group="${group.key}" data-index="${i}">編輯</button>
            <button type="button" class="btn btn-sm btn-danger" data-action="del-opt" data-group="${group.key}" data-index="${i}">刪除</button>
          </div>
        </div>
      `;
    }).join('');

    return `
      <div class="glass-card collapsible-card collapsible-card-flat">
        <div class="collapsible-header">
          <div class="collapsible-header-title">
            <i data-lucide="list-ordered" style="width:16px;height:16px;"></i>
            <span>${escapeHtml(group.label)} <span class="text-muted" style="font-size:12px;">（${list.length}）</span></span>
          </div>
          <button type="button" class="btn btn-sm btn-ghost" data-action="add-opt" data-group="${group.key}">
            <i data-lucide="plus" style="width:14px;height:14px;"></i> 新增
          </button>
        </div>
        <div class="collapsible-body" style="display:block; padding:0;">
          ${list.length === 0 ? `<div class="empty-state" style="padding:20px;">尚未設定</div>` : `<div>${rows}</div>`}
        </div>
      </div>
    `;
  }

  _clickHandler = async (e) => {
    const btn = e.target.closest('button[data-action]');
    if (!btn) return;
    const action = btn.dataset.action;
    const groupKey = btn.dataset.group;
    const index = Number(btn.dataset.index);
    const group = GROUPS.find((g) => g.key === groupKey);
    if (!group) return;

    if (action === 'add-opt') {
      await _openOptionEditModal(group, null, -1, _options, refresh);
    } else if (action === 'edit-opt') {
      await _openOptionEditModal(group, (_options[groupKey] || [])[index], index, _options, refresh);
    } else if (action === 'del-opt') {
      const item = (_options[groupKey] || [])[index];
      if (item == null) return;
      const displayName = group.type === 'value-label' ? (item.label || item.value) : item;
      const ok = await openConfirm(`確定要刪除「${displayName}」嗎？`, { okText: '刪除', okClass: 'btn-danger' });
      if (!ok) return;
      const newList = (_options[groupKey] || []).filter((_, i) => i !== index);
      try {
        await api.platformDefaults.set('options', { ..._options, [groupKey]: newList });
        showToast('✅ 已刪除', 'success');
        await refresh();
      } catch (err) {
        showToast('刪除失敗：' + err.message, 'error');
      }
    }
  };
  panel.addEventListener('click', _clickHandler);

  async function refresh() {
    await load();
  }

  await load();

  return {
    refresh,
    destroy: () => {
      if (_clickHandler) {
        panel.removeEventListener('click', _clickHandler);
        _clickHandler = null;
      }
    },
  };
}

async function _openOptionEditModal(group, item, index, allOptions, onSave) {
  const isAdd = index < 0;

  let overlay = document.getElementById(EDIT_MODAL_ID);
  if (overlay) overlay.remove();

  overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = EDIT_MODAL_ID;
  overlay.innerHTML = `
    <div class="modal" style="max-width:480px;">
      <h2 class="modal-title">${isAdd ? '新增' : '編輯'}「${escapeHtml(group.label)}」</h2>
      <div id="pd-edit-modal-form-root"></div>
    </div>
  `;
  document.body.appendChild(overlay);

  const fields = group.type === 'value-label'
    ? [
        { type: 'text', id: 'pd-opt-value', label: '值（英文 / 代碼）', required: true, maxlength: 40 },
        { type: 'text', id: 'pd-opt-label', label: '顯示名稱', required: true, maxlength: 40 },
      ]
    : [
        { type: 'text', id: 'pd-opt-value', label: '名稱', required: true, maxlength: 40 },
      ];

  const initialData = {};
  if (!isAdd && item != null) {
    if (group.type === 'value-label') {
      initialData['pd-opt-value'] = item.value || '';
      initialData['pd-opt-label'] = item.label || '';
    } else {
      initialData['pd-opt-value'] = item || '';
    }
  }

  buildForm({
    containerId: 'pd-edit-modal-form-root',
    fields,
    initialData,
    submitText: isAdd ? '新增' : '儲存',
    showCancel: true,
    cancelText: '取消',
    onSubmit: async (data) => {
      const list = [...(allOptions[group.key] || [])];

      if (group.type === 'value-label') {
        const value = String(data['pd-opt-value'] || '').trim();
        const label = String(data['pd-opt-label'] || '').trim();
        if (!value || !label) return { field: 'pd-opt-value', message: '請填寫完整' };

        if (list.some((x, i) => i !== index && x.value === value)) {
          return { field: 'pd-opt-value', message: '此值已存在' };
        }

        const newItem = { value, label };
        if (isAdd) list.push(newItem);
        else list[index] = newItem;
      } else {
        const value = String(data['pd-opt-value'] || '').trim();
        if (!value) return { field: 'pd-opt-value', message: '請填寫名稱' };
        if (list.some((x, i) => i !== index && x === value)) {
          return { field: 'pd-opt-value', message: '此名稱已存在' };
        }
        if (isAdd) list.push(value);
        else list[index] = value;
      }

      try {
        await api.platformDefaults.set('options', { ...allOptions, [group.key]: list });
        showToast('✅ 已儲存', 'success');
        closeModal(EDIT_MODAL_ID);
        await onSave();
      } catch (err) {
        showToast('儲存失敗：' + err.message, 'error');
      }
    },
    onCancel: () => closeModal(EDIT_MODAL_ID),
  });

  overlay.addEventListener('click', (e) => {
    if (e.target === overlay) closeModal(EDIT_MODAL_ID);
  });

  openModal(EDIT_MODAL_ID);
}

/* ============================================
   年份範圍 Tab
   ============================================ */
async function _renderYearRangeTab(panel) {
  let _data = { startYear: null, futureYears: 5 };

  panel.innerHTML = `
    <div class="banner" style="margin-bottom:16px;">
      ℹ️ 定義「新家庭初始化時」的預設年份範圍（家庭可自行覆蓋）。
    </div>
    <div class="glass-card" style="max-width:520px;">
      <div id="pd-yr-form-root"></div>
    </div>
  `;

  const formApi = buildForm({
    containerId: 'pd-yr-form-root',
    fields: [
      {
        type: 'number', id: 'pd-yr-start', label: '起始年份',
        min: 1900, max: 2200, step: 1,
        placeholder: '留空 = 當前年 - 3',
        hint: '若填寫的年份大於「當前年 - 3」，實際套用時會自動調整',
      },
      {
        type: 'number', id: 'pd-yr-future', label: '往後顯示年數',
        required: true, min: 0, max: 20, step: 1,
      },
    ],
    submitText: '儲存年份範圍',
    showCancel: false,
    showReset: false,
    beforeSubmit: (data) => {
      const startYear = (data['pd-yr-start'] === '' || data['pd-yr-start'] == null)
        ? null : Number(data['pd-yr-start']);
      const futureYears = Number(data['pd-yr-future']) || 5;

      if (startYear != null && (isNaN(startYear) || startYear < 1900 || startYear > 2200)) {
        return { field: 'pd-yr-start', message: '請填寫合理的年份' };
      }
      if (futureYears < 0 || futureYears > 20) {
        return { field: 'pd-yr-future', message: '請填寫 0 ~ 20 之間的數字' };
      }
      return true;
    },
    onSubmit: async (data) => {
      const startYear = (data['pd-yr-start'] === '' || data['pd-yr-start'] == null)
        ? null : Number(data['pd-yr-start']);
      const futureYears = Number(data['pd-yr-future']) || 5;

      try {
        await api.platformDefaults.set('yearRange', { startYear, futureYears });
        showToast('✅ 年份範圍已儲存', 'success');
      } catch (err) {
        showToast('儲存失敗：' + err.message, 'error');
      }
    },
  });

  async function load() {
    try {
      const result = await api.platformDefaults.list('yearRange');
      _data = result.data || { startYear: null, futureYears: 5 };
    } catch (err) {
      _data = { startYear: null, futureYears: 5 };
    }
    formApi.setData({
      'pd-yr-start': _data.startYear != null ? _data.startYear : '',
      'pd-yr-future': _data.futureYears || 5,
    });
  }

  await load();

  return {
    refresh: load,
    destroy: () => formApi.destroy(),
  };
}

/* ============================================
   UI 常數 Tab
   ============================================ */
async function _renderUIConstantsTab(panel) {
  panel.innerHTML = `
    <div class="banner" style="margin-bottom:16px;">
      ℹ️ 定義「新家庭初始化時」的預設 UI 常數（家庭可自行覆蓋）。
    </div>
    <div class="glass-card" style="max-width:520px;">
      <div id="pd-ui-form-root"></div>
    </div>
  `;

  const formApi = buildForm({
    containerId: 'pd-ui-form-root',
    fields: [
      {
        type: 'number', id: 'pd-ui-name-desktop', label: '名稱截斷長度（桌面）',
        required: true, min: 4, max: 40, step: 1,
      },
      {
        type: 'number', id: 'pd-ui-name-mobile', label: '名稱截斷長度（手機）',
        required: true, min: 2, max: 20, step: 1,
      },
      {
        type: 'number', id: 'pd-ui-toast', label: 'Toast 顯示時間（毫秒）',
        required: true, min: 500, max: 10000, step: 100,
      },
    ],
    submitText: '儲存 UI 常數',
    showCancel: false,
    showReset: false,
    onSubmit: async (data) => {
      const payload = {
        nameMaxLenDesktop: Number(data['pd-ui-name-desktop']),
        nameMaxLenMobile: Number(data['pd-ui-name-mobile']),
        toastDuration: Number(data['pd-ui-toast']),
      };
      try {
        await api.platformDefaults.set('uiConstants', payload);
        showToast('✅ UI 常數已儲存', 'success');
      } catch (err) {
        showToast('儲存失敗：' + err.message, 'error');
      }
    },
  });

  async function load() {
    try {
      const result = await api.platformDefaults.list('uiConstants');
      const data = result.data || {};
      formApi.setData({
        'pd-ui-name-desktop': data.nameMaxLenDesktop || 12,
        'pd-ui-name-mobile': data.nameMaxLenMobile || 6,
        'pd-ui-toast': data.toastDuration || 2000,
      });
    } catch (err) {
      formApi.setData({
        'pd-ui-name-desktop': 12,
        'pd-ui-name-mobile': 6,
        'pd-ui-toast': 2000,
      });
    }
  }

  await load();

  return {
    refresh: load,
    destroy: () => formApi.destroy(),
  };
}

/* ============================================
   銷毀
   ============================================ */
function _destroy() {
  Object.values(_instances).forEach((inst) => {
    try { inst?.destroy?.(); } catch (e) { /* noop */ }
  });

  if (_tabPanel) {
    try { _tabPanel.destroy(); } catch (e) { /* noop */ }
    _tabPanel = null;
  }

  document.getElementById(EDIT_MODAL_ID)?.remove();
}```

---

## 📄 js/blocks/detail-block.js

```
// ============================================
// detail-block.js — 明細展開區 Block（v103.0.0）
// 位置：js/blocks/detail-block.js
// ============================================
// 職責：
//   1. 依 row 表達式渲染單筆明細
//   2. row 為 null 時顯示空狀態
//   3. 支援巢狀欄位網格顯示
//
// Block 定義：
//   {
//     type: 'detail',
//     container: 'xxx-detail',
//     row: '$.selectedRow',                 // 表達式（必填）
//     fields: [                              // 顯示欄位
//       { label: '名稱', key: 'name' },
//       { label: '金額', key: 'amount', type: 'number' },
//       { label: '狀態', key: 'status', resolver: (val, row) => html },
//     ],
//     emptyText: '請選擇一筆資料',
//     title: '$.selectedRow.name',           // 選填，標題
//   }
// ============================================

import { esc } from '../lib/dom.js';
import { formatCellValue } from '../lib/format.js';
import { resolveExpr } from '../engines/render-engine.js';

/* ============================================
   mount
   ============================================ */
export function mount(block, ctx) {
  const container = document.getElementById(block.container);
  if (!container) {
    console.warn(`[detail-block] 找不到容器 #${block.container}`);
    return { onDepsChange: () => {}, destroy: () => {} };
  }

  function render() {
    const row = _resolveRow(block, ctx);

    if (row == null) {
      _renderEmpty(container, block.emptyText || '請選擇一筆資料');
      return;
    }

    const title = _resolveTitle(block, ctx, row);
    const fields = block.fields || [];

    container.innerHTML = `
      <div style="padding:12px 0;">
        ${title ? `<div style="font-size:15px; font-weight:700; color:var(--neon-cyan); margin-bottom:12px;">${esc(title)}</div>` : ''}
        <div style="display:grid; grid-template-columns:repeat(auto-fill, minmax(160px, 1fr)); gap:8px 20px; font-size:13px;">
          ${fields.map((f) => _renderField(f, row)).join('')}
        </div>
      </div>
    `;

    if (window.lucide) window.lucide.createIcons();
  }

  function onDepsChange() {
    render();
  }

  function destroy() {
    container.innerHTML = '';
  }

  /* 首次渲染 */
  render();

  return { onDepsChange, destroy };
}

/* ============================================
   欄位渲染
   ============================================ */
function _renderField(field, row) {
  const val = row[field.key];
  let content;

  if (typeof field.resolver === 'function') {
    try {
      content = field.resolver(val, row);
    } catch (err) {
      console.error(`[detail-block] resolver 失敗 [${field.key}]：`, err);
      content = '—';
    }
  } else {
    content = formatCellValue(val, field.type);
  }

  return `
    <div>
      <div style="font-size:11px; color:var(--text-muted); margin-bottom:2px;">${esc(field.label || field.key)}</div>
      <div style="color:var(--text-primary);">${content}</div>
    </div>
  `;
}

/* ============================================
   空狀態
   ============================================ */
function _renderEmpty(container, text) {
  container.innerHTML = `
    <div class="empty-state" style="padding:24px;">${esc(text)}</div>
  `;
}

/* ============================================
   解析 row / title
   ============================================ */
function _resolveRow(block, ctx) {
  if (!block.row) return null;
  if (typeof block.row === 'string') {
    try {
      return resolveExpr(block.row, ctx);
    } catch (e) {
      return null;
    }
  }
  if (typeof block.row === 'object') return block.row;
  return null;
}

function _resolveTitle(block, ctx, row) {
  if (block.title) {
    if (typeof block.title === 'string') {
      try {
        const resolved = resolveExpr(block.title, ctx);
        if (resolved != null) return String(resolved);
      } catch (e) { /* noop */ }
    }
    return String(block.title);
  }

  /* 若 row 有 name 或 title 屬性，自動使用 */
  if (row.name) return String(row.name);
  if (row.title) return String(row.title);
  return '';
}
```

---

## 📄 js/blocks/filter-block.js

```
// ============================================
// filter-block.js — 篩選列區 Block（v103.0.0）
// 位置：js/blocks/filter-block.js
// ============================================
// 職責：
//   1. 渲染年月 / 自訂篩選列
//   2. 變更時寫回 ctx.state（透過 ctx.setState）
//
// Block 定義：
//   {
//     type: 'filter',
//     container: 'xxx-filter',
//     fields: ['year', 'month'],           // 內建欄位
//     customFields: [                       // 自訂欄位（選填）
//       { id: 'source', label: '來源', options: [...] },
//     ],
//     bind: 'state.filters',                // 綁定路徑（必填）
//   }
// ============================================

import { esc } from '../lib/dom.js';
import { resolveExpr } from '../engines/render-engine.js';
import { getYearList } from '../config/app-config.js';
import { AppState } from '../core/state.js';

/* ============================================
   mount
   ============================================ */
export function mount(block, ctx) {
  const container = document.getElementById(block.container);
  if (!container) {
    console.warn(`[filter-block] 找不到容器 #${block.container}`);
    return { onDepsChange: () => {}, destroy: () => {} };
  }

  const bindPath = (block.bind || 'state.filters').replace(/^state\./, '');
  let _changeHandler = null;

  function render() {
    const currentFilters = _getFilters(ctx, bindPath);
    const fields = block.fields || ['year', 'month'];

    const parts = [];

    if (fields.includes('year')) {
      parts.push(_renderYearSelect(currentFilters.year));
    }
    if (fields.includes('month')) {
      parts.push(_renderMonthSelect(currentFilters.month));
    }

    (block.customFields || []).forEach((f) => {
      parts.push(_renderCustomSelect(f, currentFilters[f.id]));
    });

    container.innerHTML = `
      <div class="page-filter-bar">
        ${parts.join('')}
      </div>
    `;

    bindEvents();

    if (window.lucide) window.lucide.createIcons();
  }

  function bindEvents() {
    if (_changeHandler) {
      container.removeEventListener('change', _changeHandler);
    }

    _changeHandler = (e) => {
      const sel = e.target.closest('select[data-filter]');
      if (!sel) return;

      const key = sel.dataset.filter;
      const value = sel.value;

      _setFilter(ctx, bindPath, key, value);
    };

    container.addEventListener('change', _changeHandler);
  }

  function onDepsChange() {
    render();
  }

  function destroy() {
    if (_changeHandler) {
      container.removeEventListener('change', _changeHandler);
      _changeHandler = null;
    }
    container.innerHTML = '';
  }

  /* 首次渲染 */
  render();

  return { onDepsChange, destroy };
}

/* ============================================
   內建欄位渲染
   ============================================ */
function _renderYearSelect(currentValue) {
  const years = getYearList();
  const opts = years.map((y) => {
    const sel = String(y) === String(currentValue) ? 'selected' : '';
    return `<option value="${y}" ${sel}>${y} 年</option>`;
  }).join('');

  return `
    <div class="filter-group">
      <label class="field-label">年份</label>
      <select class="select" data-filter="year">${opts}</select>
    </div>
  `;
}

function _renderMonthSelect(currentValue) {
  let opts = `<option value="" ${currentValue === '' || currentValue === 'all' ? 'selected' : ''}>全部</option>`;
  for (let m = 1; m <= 12; m++) {
    const mm = String(m).padStart(2, '0');
    const sel = mm === String(currentValue) ? 'selected' : '';
    opts += `<option value="${mm}" ${sel}>${m} 月</option>`;
  }

  return `
    <div class="filter-group">
      <label class="field-label">月份</label>
      <select class="select" data-filter="month">${opts}</select>
    </div>
  `;
}

function _renderCustomSelect(field, currentValue) {
  const opts = (field.options || []).map((o) => {
    const sel = String(o.value) === String(currentValue) ? 'selected' : '';
    return `<option value="${esc(o.value)}" ${sel}>${esc(o.label)}</option>`;
  }).join('');

  return `
    <div class="filter-group">
      <label class="field-label">${esc(field.label || '')}</label>
      <select class="select" data-filter="${esc(field.id)}">${opts}</select>
    </div>
  `;
}

/* ============================================
   篩選讀寫
   ============================================ */
function _getFilters(ctx, bindPath) {
  try {
    const expr = `state.${bindPath}`;
    const val = resolveExpr(expr, ctx);
    return val || {};
  } catch (e) {
    return {};
  }
}

function _setFilter(ctx, bindPath, key, value) {
  const normalized = (key === 'month' && value === 'all') ? '' : value;

  try {
    if (typeof ctx.setState === 'function') {
      ctx.setState(`${bindPath}.${key}`, normalized);
    } else {
      /* fallback：直接寫入 ctx.state */
      const keys = bindPath.split('.');
      const target = keys.reduce((acc, k) => {
        if (acc[k] == null || typeof acc[k] !== 'object') acc[k] = {};
        return acc[k];
      }, ctx.state);
      target[key] = normalized;
    }
  } catch (err) {
    console.error('[filter-block] 寫入 state 失敗：', err);
  }
}
```

---

## 📄 js/blocks/form-block.js

```
// ============================================
// form-block.js — 表單 Modal 區 Block（v103.0.12）
// 位置：js/blocks/form-block.js
// ============================================
// v103.0.12 修正：
//   ✅ [L05] 移除未使用的 esc import
// ============================================

import { resolveExpr } from '../engines/render-engine.js';
import { openEntityModal } from '../entity/entity-modal.js';
import { getEntityDef } from '../entity/entity-definitions.js';
import { AppState } from '../core/state.js';

/* ============================================
   mount
   ============================================ */
export function mount(block, ctx) {
  const container = document.getElementById(block.container);
  if (!container) {
    console.warn(`[form-block] 找不到容器 #${block.container}`);
    return { onDepsChange: () => {}, destroy: () => {} };
  }

  const def = getEntityDef(block.entity);
  if (!def) {
    console.warn(`[form-block] 未知的 entity：${block.entity}`);
    return { onDepsChange: () => {}, destroy: () => {} };
  }

  let _clickHandler = null;

  function render() {
    const mode = block.mode || 'add';
    const userCanInput = AppState.getCanInput();

    if (!userCanInput) {
      container.innerHTML = '';
      return;
    }

    if (mode === 'edit') {
      const row = _resolveEditRow(block, ctx);
      if (row == null) {
        container.innerHTML = '';
        return;
      }
    }

    const label = block.buttonLabel || `${mode === 'edit' ? '編輯' : '新增'}${def.label}`;
    const cls = block.buttonClass || 'btn-primary';
    const icon = block.icon || (mode === 'edit' ? 'pencil' : 'plus');

    container.innerHTML = `
      <button type="button" class="btn ${cls}" data-form-action="open">
        <i data-lucide="${icon}"></i> ${label}
      </button>
    `;

    bindEvents();

    if (window.lucide) window.lucide.createIcons();
  }

  function bindEvents() {
    if (_clickHandler) {
      container.removeEventListener('click', _clickHandler);
    }

    _clickHandler = async (e) => {
      const btn = e.target.closest('button[data-form-action="open"]');
      if (!btn) return;
      e.preventDefault();
      await _openModal(block, ctx);
    };

    container.addEventListener('click', _clickHandler);
  }

  function onDepsChange() {
    render();
  }

  function destroy() {
    if (_clickHandler) {
      container.removeEventListener('click', _clickHandler);
      _clickHandler = null;
    }
    container.innerHTML = '';
  }

  /* 首次渲染 */
  render();

  return { onDepsChange, destroy };
}

/* ============================================
   開啟 Modal
   ============================================ */
async function _openModal(block, ctx) {
  const mode = block.mode || 'add';
  const allRows = _resolveAllRows(block, ctx);

  let id = null;
  let initialData = null;

  if (mode === 'edit') {
    const row = _resolveEditRow(block, ctx);
    if (!row) return;
    id = row.id;
  }

  await openEntityModal({
    entity: block.entity,
    mode,
    id,
    initialData,
    allRows,
    onSuccess: () => {
      try {
        if (typeof ctx.invalidate === 'function') {
          ctx.invalidate('__form_saved__');
        }
      } catch (err) {
        console.warn('[form-block] invalidate 失敗：', err);
      }
    },
  });
}

/* ============================================
   工具
   ============================================ */
function _resolveEditRow(block, ctx) {
  if (!block.editRow) {
    try {
      return ctx.state?.selectedRow || null;
    } catch (e) {
      return null;
    }
  }

  if (typeof block.editRow === 'string') {
    try {
      return resolveExpr(block.editRow, ctx);
    } catch (e) {
      return null;
    }
  }

  if (typeof block.editRow === 'object') return block.editRow;
  return null;
}

function _resolveAllRows(block, ctx) {
  if (!block.allRows) return [];
  if (typeof block.allRows === 'string') {
    try {
      const resolved = resolveExpr(block.allRows, ctx);
      return Array.isArray(resolved) ? resolved : [];
    } catch (e) {
      return [];
    }
  }
  if (Array.isArray(block.allRows)) return block.allRows;
  return [];
}```

---

## 📄 js/blocks/list-block.js

```
// list-block.js — 列表區（v103.0.18）
import { esc } from '../lib/dom.js';
import { formatCellValue } from '../lib/format.js';
import { resolveExpr } from '../engines/render-engine.js';
import { getColumns, getResolvers } from '../config/column-registry.js';
import { getEntityUi } from '../entity/entity-definitions.js';
import { AppState } from '../core/state.js';

/* 🆕 就地更新：同 HTML 不重繪 */
function _render(container, html) {
  if (container.__lastHtml === html) return;
  container.__lastHtml = html;
  container.innerHTML = html;
  if (window.lucide) window.lucide.createIcons();
}

export function mount(block, ctx) {
  const container = document.getElementById(block.container);
  if (!container) return { onDepsChange: () => {}, destroy: () => {} };

  const columnsKey = typeof block.columns === 'string' ? block.columns : (block.columnsKey || '');
  const columns = _resolveColumns(block, columnsKey);
  const resolvers = columnsKey ? getResolvers(columnsKey) : {};
  const tableId = block.tableId || columnsKey || block.container;
  const expandable = !!block.expandable;
  const emptyText = block.emptyText || '尚無資料';
  const _expanded = new Set();
  let _handler = null;

  function render() {
    const view = _resolveView(block, ctx);
    const rows = _getRows(block, ctx);
    if (!rows || rows.length === 0) {
      _render(container, `<div class="glass-card"><div class="empty-state">${esc(emptyText)}</div></div>`);
      return;
    }
    const html = view === 'card'
      ? _renderCards(block, ctx, rows, columns, resolvers)
      : _renderTable(block, ctx, rows, columns, resolvers, _expanded);
    _render(container, html);
    _bind();
  }

  function _bind() {
    if (_handler) container.removeEventListener('click', _handler);
    _handler = (e) => {
      const expandBtn = e.target.closest('button[data-action="toggle-expand"]');
      if (expandBtn) {
        e.preventDefault();
        const idx = Number(expandBtn.dataset.index);
        _expanded.has(idx) ? _expanded.delete(idx) : _expanded.add(idx);
        render();
        return;
      }
      const actionBtn = e.target.closest('button[data-action][data-index]');
      if (actionBtn && actionBtn.dataset.action !== 'toggle-expand') {
        const idx = Number(actionBtn.dataset.index);
        const rows = _getRows(block, ctx);
        const row = rows[idx];
        if (!row) return;
        const actions = typeof block.actions === 'function' ? (block.actions(row, ctx) || []) : [];
        const matched = actions.find((a) => (a.action || '') === actionBtn.dataset.action);
        if (matched && typeof matched.onClick === 'function') {
          e.stopPropagation();
          matched.onClick(row, ctx);
        }
      }
    };
    container.addEventListener('click', _handler);
  }

  render();

  return {
    onDepsChange: render,
    destroy: () => {
      if (_handler) container.removeEventListener('click', _handler);
      container.innerHTML = '';
      container.__lastHtml = '';
      _expanded.clear();
    },
  };
}

function _renderTable(block, ctx, rows, columns, resolvers, expanded) {
  const hasActions = typeof block.actions === 'function';
  const expandable = !!block.expandable;
  const head = `<thead><tr>${expandable ? '<th class="expand-col" style="width:36px;"></th>' : ''}${columns.map((c) => `<th class="${c.type === 'number' ? 'num' : ''}">${esc(c.label)}</th>`).join('')}${hasActions ? '<th style="width:160px;">操作</th>' : ''}</tr></thead>`;
  const body = rows.map((row, i) => {
    const main = `<tr data-row-index="${i}">
      ${expandable ? `<td class="expand-col"><button type="button" class="btn btn-sm btn-ghost" data-action="toggle-expand" data-index="${i}" style="padding:2px 6px;"><i data-lucide="${expanded.has(i) ? 'chevron-down' : 'chevron-right'}" style="width:14px;height:14px;"></i></button></td>` : ''}
      ${columns.map((col) => _cell(col, row, resolvers)).join('')}
      ${hasActions ? _actions(row, i, block, ctx) : ''}
    </tr>`;
    if (!expandable || typeof block.renderDetail !== 'function') return main;
    const total = columns.length + (expandable ? 1 : 0) + (hasActions ? 1 : 0);
    return main + `<tr class="detail-row" style="display:${expanded.has(i) ? 'table-row' : 'none'};"><td colspan="${total}" class="detail-cell">${block.renderDetail(row, ctx) || ''}</td></tr>`;
  }).join('');
  return `<div class="data-table-scroll-wrapper"><table class="data-table">${head}<tbody>${body}</tbody></table></div>`;
}

function _cell(col, row, resolvers) {
  const val = row[col.id];
  const r = resolvers[col.id];
  const content = r ? r(val, row) : formatCellValue(val, col.type);
  return `<td class="${col.type === 'number' ? 'num' : ''}">${content}</td>`;
}

function _actions(row, i, block, ctx) {
  const actions = typeof block.actions === 'function' ? (block.actions(row, ctx) || []) : [];
  if (actions.length === 0) return '<td></td>';
  return `<td>${actions.map((a) => `<button type="button" class="btn btn-sm ${esc(a.className || 'btn-ghost')}" data-action="${esc(a.action || 'custom')}" data-index="${i}">${a.icon ? `<i data-lucide="${esc(a.icon)}" style="width:14px;height:14px;"></i>` : ''}${esc(a.label || '')}</button>`).join('')}</td>`;
}

function _renderCards(block, ctx, rows, columns, resolvers) {
  const entityUi = block.entityKey ? getEntityUi(block.entityKey) : null;
  const primaryCol = entityUi?.primaryColumn || columns[0]?.id || 'name';
  const cardFields = entityUi?.cardFields || columns.slice(1, 4).map((c) => c.id);
  const colMap = {};
  columns.forEach((c) => { colMap[c.id] = c; });
  const hasActions = typeof block.actions === 'function';
  return `<div class="data-cards-grid">${rows.map((row, i) => {
    const fields = cardFields.map((id) => {
      const col = colMap[id];
      if (!col) return '';
      const val = row[id];
      const r = resolvers[id];
      const content = r ? r(val, row) : formatCellValue(val, col.type);
      return `<div class="data-card-field"><span class="data-card-label">${esc(col.label)}</span><span class="data-card-value">${content}</span></div>`;
    }).join('');
    const actions = hasActions ? (block.actions(row, ctx) || []) : [];
    const actionsHtml = actions.length > 0
      ? `<div class="data-card-footer">${actions.map((a) => `<button type="button" class="btn btn-sm ${esc(a.className || 'btn-ghost')}" data-action="${esc(a.action || 'custom')}" data-index="${i}">${a.icon ? `<i data-lucide="${esc(a.icon)}" style="width:14px;height:14px;"></i>` : ''}${esc(a.label || '')}</button>`).join('')}</div>`
      : '';
    return `<div class="glass-card data-card" data-row-index="${i}"><div class="data-card-primary">${esc(row[primaryCol] || row.name || '（未命名）')}</div><div class="data-card-fields">${fields}</div>${actionsHtml}</div>`;
  }).join('')}</div>`;
}

function _getRows(block, ctx) {
  if (!block.rows) return [];
  if (typeof block.rows === 'string') {
    const r = resolveExpr(block.rows, ctx);
    return Array.isArray(r) ? r : [];
  }
  return Array.isArray(block.rows) ? block.rows : [];
}

function _resolveColumns(block, key) {
  if (Array.isArray(block.columns)) return block.columns;
  if (key) return getColumns(key);
  return [];
}

function _resolveView(block, ctx) {
  if (block.view === 'table' || block.view === 'card') return block.view;
  return AppState.getCurrentView() === 'card' ? 'card' : 'table';
}```

---

## 📄 js/blocks/stats-block.js

```
// stats-block.js — 統計卡區（v103.0.18）
import { esc } from '../lib/dom.js';
import { resolveExpr } from '../engines/render-engine.js';
import { AppState } from '../core/state.js';
import { STORAGE_KEYS } from '../config/constants.js';

function _render(container, html) {
  if (container.__lastHtml === html) return;
  container.__lastHtml = html;
  container.innerHTML = html;
  if (window.lucide) window.lucide.createIcons();
}

export function mount(block, ctx) {
  const container = document.getElementById(block.container);
  if (!container) return { onDepsChange: () => {}, destroy: () => {} };

  let _unsubView = null, _resizeH = null;

  function render() {
    const mode = block.mode || _getStatsMode();
    const displayMode = _resolveDisplayMode(mode);
    const cards = _resolveCards(block, ctx);
    if (cards.length === 0) { container.innerHTML = ''; container.__lastHtml = ''; return; }
    const html = displayMode === 'C' ? _integrated(cards) : _compact(cards);
    _render(container, html);
  }

  render();
  if ((block.mode || _getStatsMode()) === 'auto') {
    _unsubView = AppState.on('view-change', render);
    _resizeH = _debounce(render, 200);
    window.addEventListener('resize', _resizeH);
  }

  return {
    onDepsChange: render,
    destroy: () => {
      if (_unsubView) { try { _unsubView(); } catch (e) {} }
      if (_resizeH) window.removeEventListener('resize', _resizeH);
      container.innerHTML = '';
      container.__lastHtml = '';
    },
  };
}

function _integrated(cards) {
  return `<div class="stats-integrated" style="margin-bottom:20px;"><div class="stats-integrated-grid">${cards.map(_integratedCell).join('')}</div></div>`;
}

function _integratedCell(c) {
  const icon = c.icon ? `<i data-lucide="${esc(c.icon)}" style="width:13px;height:13px;"></i>` : '';
  const title = icon ? `<div class="stats-integrated-title">${icon}<span>${esc(c.title || '')}</span></div>` : `<div class="stats-integrated-title"><span>${esc(c.title || '')}</span></div>`;
  const hint = c.hint ? `<div class="stats-integrated-hint">${esc(c.hint)}</div>` : '';
  return `<div class="stats-integrated-cell">${title}<div class="stats-integrated-value mono ${esc(c.valueClass || '')}">${c.value == null ? '—' : esc(String(c.value))}</div>${hint}</div>`;
}

function _compact(cards) {
  const cols = Math.min(4, Math.max(2, cards.length));
  return `<div class="stats-compact-grid stats-compact-grid-${cols}" style="margin-bottom:20px;">${cards.map(_compactCard).join('')}</div>`;
}

function _compactCard(c) {
  const icon = c.icon ? `<i data-lucide="${esc(c.icon)}" style="width:12px;height:12px;"></i>` : '';
  const title = icon ? `<div class="glass-card-title" style="display:flex;align-items:center;gap:4px;">${icon}<span>${esc(c.title || '')}</span></div>` : `<div class="glass-card-title">${esc(c.title || '')}</div>`;
  const hint = c.hint ? `<div class="glass-card-hint">${esc(c.hint)}</div>` : '';
  return `<div class="glass-card stats-compact-card">${title}<div class="glass-card-value mono ${esc(c.valueClass || '')}">${c.value == null ? '—' : esc(String(c.value))}</div>${hint}</div>`;
}

function _resolveCards(block, ctx) {
  let raw = block.cards;
  if (typeof raw === 'string') {
    try { raw = resolveExpr(raw, ctx); } catch (e) { return []; }
  }
  if (!Array.isArray(raw)) return [];
  return raw.map((c) => ({ ...c, value: _resolveVal(c.value, ctx), hint: _resolveVal(c.hint, ctx) }));
}

function _resolveVal(val, ctx) {
  if (typeof val !== 'string') return val;
  if (/^(\$\.|state\.|data\.|\$data\.|\$state\.)/.test(val)) {
    try { return resolveExpr(val, ctx); } catch (e) { return val; }
  }
  return val;
}

function _getStatsMode() {
  try {
    const s = localStorage.getItem(STORAGE_KEYS.STATS_MODE);
    if (s === 'integrated' || s === 'compact' || s === 'auto') return s;
  } catch (e) {}
  return 'auto';
}

function _resolveDisplayMode(mode) {
  if (mode === 'integrated') return 'C';
  if (mode === 'compact') return 'B';
  if (typeof window !== 'undefined' && window.innerWidth < 640) return 'B';
  return AppState.getCurrentView() === 'table' ? 'C' : 'B';
}

function _debounce(fn, wait) {
  let t = null;
  return function (...a) { clearTimeout(t); t = setTimeout(() => fn.apply(this, a), wait); };
}```

---

## 📄 js/config/app-config.js

```
// ============================================
// app-config.js — 平台預設 + 家庭覆蓋設定（v103.0.22）
// 位置：js/config/app-config.js
// ============================================
// v103.0.22（B25-5）：
//   ✅ 補 END OF FILE
// v102.0.0 修正：
//   ✅ [P3-3] _toObject / _mergeAll 空物件 fallback 補強
//   ✅ getOptions('paymentModes') 預設值
// ============================================

import { db } from './firebase-config.js';
import { ref, get, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import {
  DEFAULT_STATUSES,
  DEFAULT_OPTIONS,
  DEFAULT_YEAR_RANGE,
  DEFAULT_UI_CONSTANTS,
  LIMITS,
} from './constants.js';

let _familyId = '';
let _merged = null;
let _listeners = [];
let _unsubscribers = [];

let _platformDefaults = {
  statuses: null,
  options: null,
  yearRange: null,
  uiConstants: null,
};

let _familyOverrides = {
  statuses: null,
  options: null,
  yearRange: null,
  uiConstants: null,
};

/* ============================================
   初始化 / 銷毀
   ============================================ */
export async function initAppConfig(familyId = '') {
  _familyId = familyId || '';

  const [pStatuses, pOptions, pYearRange, pUI] = await Promise.all([
    _get('platform/defaults/statuses'),
    _get('platform/defaults/options'),
    _get('platform/defaults/year_range'),
    _get('platform/defaults/ui_constants'),
  ]);

  _platformDefaults = {
    statuses: pStatuses,
    options: pOptions,
    yearRange: pYearRange,
    uiConstants: pUI,
  };

  if (_familyId) {
    const [fStatuses, fOptions, fYearRange, fUI] = await Promise.all([
      _get(`families/${_familyId}/statuses`),
      _get(`families/${_familyId}/settings/options`),
      _get(`families/${_familyId}/settings/year_range`),
      _get(`families/${_familyId}/settings/ui_constants`),
    ]);

    _familyOverrides = {
      statuses: fStatuses,
      options: fOptions,
      yearRange: fYearRange,
      uiConstants: fUI,
    };
  } else {
    _familyOverrides = {
      statuses: null, options: null, yearRange: null, uiConstants: null,
    };
  }

  _merged = _mergeAll();
  _notifyAll();
  return _merged;
}

export function watchPlatformDefaults() {
  _unsubscribers = _unsubscribers.filter((entry) => {
    if (entry.scope === 'platform') {
      try { entry.unsub(); } catch (e) { /* noop */ }
      return false;
    }
    return true;
  });

  const paths = [
    ['statuses',    'platform/defaults/statuses'],
    ['options',     'platform/defaults/options'],
    ['yearRange',   'platform/defaults/year_range'],
    ['uiConstants', 'platform/defaults/ui_constants'],
  ];

  paths.forEach(([key, path]) => {
    const unsub = onValue(ref(db, path), (snap) => {
      _platformDefaults[key] = snap.val();
      _merged = _mergeAll();
      _notifyAll();
    });
    _unsubscribers.push({ scope: 'platform', unsub });
  });
}

export function watchFamilySettings() {
  if (!_familyId) return;

  _unsubscribers = _unsubscribers.filter((entry) => {
    if (entry.scope === 'family') {
      try { entry.unsub(); } catch (e) { /* noop */ }
      return false;
    }
    return true;
  });

  const paths = [
    ['statuses',    `families/${_familyId}/statuses`],
    ['options',     `families/${_familyId}/settings/options`],
    ['yearRange',   `families/${_familyId}/settings/year_range`],
    ['uiConstants', `families/${_familyId}/settings/ui_constants`],
  ];

  paths.forEach(([key, path]) => {
    const unsub = onValue(ref(db, path), (snap) => {
      _familyOverrides[key] = snap.val();
      _merged = _mergeAll();
      _notifyAll();
    });
    _unsubscribers.push({ scope: 'family', unsub });
  });
}

export function disposeAppConfig() {
  _unsubscribers.forEach((entry) => {
    try { entry.unsub(); } catch (e) { /* noop */ }
  });
  _unsubscribers = [];
  _listeners = [];
  _familyId = '';
  _merged = null;
}

/* ============================================
   對外查詢
   ============================================ */
export function getStatuses() {
  const raw = _merged?.statuses;

  if (!raw || typeof raw !== 'object' || Object.keys(raw).length === 0) {
    return Object.entries(_toObject(DEFAULT_STATUSES))
      .map(([id, s]) => ({ id, ...s }))
      .sort((a, b) => (a.order || 0) - (b.order || 0));
  }

  return Object.entries(raw)
    .map(([id, s]) => ({ id, ...s }))
    .sort((a, b) => (a.order || 0) - (b.order || 0));
}

export function getStatusesByCategory(category) {
  return getStatuses().filter((s) => s.category === category);
}

export function getDefaultStatus(category) {
  const list = getStatusesByCategory(category);
  return list.find((s) => !s.isDone) || list[0] || null;
}

export function findStatusByName(name) {
  return getStatuses().find((s) => s.name === name) || null;
}

export function isDoneStatus(name) {
  const s = findStatusByName(name);
  return s ? !!s.isDone : false;
}

export function getOptions(key) {
  const opts = _merged?.options;

  if (!opts || typeof opts !== 'object' || Object.keys(opts).length === 0) {
    return DEFAULT_OPTIONS[key] || [];
  }
  return opts[key] || DEFAULT_OPTIONS[key] || [];
}

export function getOptionsRaw() {
  const opts = _merged?.options;
  if (!opts || typeof opts !== 'object' || Object.keys(opts).length === 0) {
    return DEFAULT_OPTIONS;
  }
  return opts;
}

export function getYearRange() {
  const cfg = _merged?.yearRange || DEFAULT_YEAR_RANGE;
  const curY = new Date().getFullYear();

  let startYear;
  if (cfg.startYear != null && cfg.startYear !== '') {
    startYear = Math.min(Number(cfg.startYear), curY - LIMITS.YEAR_PAST_DEFAULT);
  } else {
    startYear = curY - LIMITS.YEAR_PAST_DEFAULT;
  }
  const futureYears = Number(cfg.futureYears) || LIMITS.YEAR_FUTURE_DEFAULT;

  return { startYear, endYear: curY + futureYears };
}

export function getYearList() {
  const { startYear, endYear } = getYearRange();
  const list = [];
  for (let y = startYear; y <= endYear; y++) list.push(y);
  return list;
}

export function getUIConstants() {
  const cfg = _merged?.uiConstants || DEFAULT_UI_CONSTANTS;
  return {
    nameMaxLenDesktop: Number(cfg.nameMaxLenDesktop) || LIMITS.NAME_MAX_LEN_DESKTOP,
    nameMaxLenMobile: Number(cfg.nameMaxLenMobile) || LIMITS.NAME_MAX_LEN_MOBILE,
    toastDuration: Number(cfg.toastDuration) || LIMITS.TOAST_DURATION_DEFAULT,
  };
}

export function onConfigChange(callback) {
  _listeners.push(callback);
  try { callback(_merged); } catch (e) { console.error('[app-config] listener error:', e); }
  return () => {
    _listeners = _listeners.filter((cb) => cb !== callback);
  };
}

/* ============================================
   內部工具
   ============================================ */
async function _get(path) {
  try {
    const snap = await get(ref(db, path));
    return snap.val();
  } catch (err) {
    console.warn(`[app-config] 讀取失敗 ${path}：`, err);
    return null;
  }
}

function _toObject(arr) {
  const obj = {};
  arr.forEach((item, i) => {
    obj[item.key || `default_${i}`] = { ...item };
  });
  return obj;
}

function _mergeAll() {
  return {
    statuses: _pickNonEmpty(_familyOverrides.statuses, _platformDefaults.statuses, _toObject(DEFAULT_STATUSES)),
    options: _mergeOptions(_familyOverrides.options, _platformDefaults.options),
    yearRange: _mergeObject(_familyOverrides.yearRange, _platformDefaults.yearRange, DEFAULT_YEAR_RANGE),
    uiConstants: _mergeObject(_familyOverrides.uiConstants, _platformDefaults.uiConstants, DEFAULT_UI_CONSTANTS),
  };
}

function _pickNonEmpty(familyObj, platformObj, fallback) {
  if (familyObj && typeof familyObj === 'object' && Object.keys(familyObj).length > 0) {
    return familyObj;
  }
  if (platformObj && typeof platformObj === 'object' && Object.keys(platformObj).length > 0) {
    return platformObj;
  }
  return fallback;
}

function _mergeOptions(familyOpts, platformOpts) {
  const hasFamily = familyOpts && typeof familyOpts === 'object' && Object.keys(familyOpts).length > 0;
  const hasPlatform = platformOpts && typeof platformOpts === 'object' && Object.keys(platformOpts).length > 0;

  const base = hasPlatform ? platformOpts : DEFAULT_OPTIONS;
  if (!hasFamily) return base;

  const merged = { ...base };
  Object.entries(familyOpts).forEach(([key, val]) => {
    if (val != null) merged[key] = val;
  });
  return merged;
}

function _mergeObject(familyObj, platformObj, fallback) {
  const hasFamily = familyObj && typeof familyObj === 'object' && Object.keys(familyObj).length > 0;
  const hasPlatform = platformObj && typeof platformObj === 'object' && Object.keys(platformObj).length > 0;

  const base = hasPlatform ? platformObj : fallback;
  if (!hasFamily) return base;
  return { ...base, ...familyObj };
}

function _notifyAll() {
  _listeners.forEach((cb) => {
    try { cb(_merged); } catch (e) { console.error('[app-config] listener error:', e); }
  });
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/config/app-config.js
   Version: v103.0.22
   Batch: B25-5
   ═══════════════════════════════════════════ */```

---

## 📄 js/config/column-registry.js

```
// column-registry.js — 表格欄位 SSOT（v103.0.35）
// v103.0.35（B34-4）：
//   ✅ 加 codeList 欄位定義
// v103.0.34（B29-1b）：既有
// ============================================
import { formatHKD, formatNumber } from '../lib/format.js';
import { esc as escapeHtml } from '../lib/dom.js';
import { resolveName } from './entity-registry.js';
import { BANK_TXN_TYPE_LABELS, BANK_TXN_CATEGORY_LABELS, BANK_TXN_CATEGORY_BADGES, BANK_ACCOUNT_TYPE_LABELS, RESERVED_IDS } from './constants.js';
import { badgeClass as statusBadgeClass, label as statusLabel } from './status-registry.js';

/* ============================================
   COLUMNS 定義
   ============================================ */
export const COLUMNS = {
  members: [
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 160, sortable: true },
    { id: 'role', label: '角色', defaultVisible: true, defaultWidth: 120, sortable: true, filterable: true },
    { id: 'order', label: '排序', defaultVisible: true, defaultWidth: 80, type: 'number', sortable: true },
  ],
  banks: [
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 160, sortable: true },
    { id: 'order', label: '排序', defaultVisible: true, defaultWidth: 80, type: 'number', sortable: true },
  ],
  bankAccounts: [
    { id: 'order', label: '排序', defaultVisible: true, defaultWidth: 60, sortable: true },
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 160, sortable: true },
    { id: 'type', label: '類型', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true, filterOptions: [
      { value: 'family', label: '家庭帳號' },
      { value: 'personal', label: '個人帳號' },
    ]},
    { id: 'initialBalance', label: '初始餘額', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'initialYM', label: '初始年月', defaultVisible: true, defaultWidth: 110 },
  ],
  categories: [
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 200, sortable: true },
    { id: 'order', label: '排序', defaultVisible: true, defaultWidth: 80, type: 'number', sortable: true },
  ],
  items: [
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 200, sortable: true },
    { id: 'categoryId', label: '所屬類別', defaultVisible: true, defaultWidth: 140, sortable: true, filterable: true },
  ],
  payments: [
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 160, sortable: true },
    { id: 'order', label: '排序', defaultVisible: true, defaultWidth: 80, type: 'number', sortable: true },
  ],
  statuses: [
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 140, sortable: true },
    { id: 'category', label: '所屬類別', defaultVisible: true, defaultWidth: 110, sortable: true, filterable: true },
    { id: 'isDone', label: '已完成', defaultVisible: true, defaultWidth: 90, sortable: true },
    { id: 'order', label: '排序', defaultVisible: true, defaultWidth: 70, type: 'number', sortable: true },
  ],
  companies: [
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 160, sortable: true },
  ],
  funds: [
    { id: 'name', label: '基金名稱', defaultVisible: true, defaultWidth: 200, sortable: true },
    { id: 'typeLabel', label: '類型', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true, filterOptions: [
      { value: 'insurance', label: '保險' },
      { value: 'standalone', label: '獨立' },
    ]},
    { id: 'cost', label: '投入成本', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'currentValue', label: '現時價值', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'pnl', label: '盈虧', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'units', label: '單位數', defaultVisible: true, defaultWidth: 100, type: 'number', sortable: true },
    { id: 'note', label: '備註', defaultVisible: false, defaultWidth: 180 },
  ],
  fundSnapshots: [
    { id: 'yearMonth', label: '年月', defaultVisible: true, defaultWidth: 90, sortable: true },
    { id: 'shares', label: '股數', defaultVisible: true, defaultWidth: 100, type: 'number', sortable: true },
    { id: 'nav', label: '股價', defaultVisible: true, defaultWidth: 100, type: 'number', sortable: true },
    { id: 'value', label: '現值', defaultVisible: true, defaultWidth: 110, type: 'number', sortable: true },
    { id: 'cumulativeCost', label: '累積供款', defaultVisible: true, defaultWidth: 110, type: 'number', sortable: true },
    { id: 'pnl', label: '盈虧', defaultVisible: true, defaultWidth: 110, type: 'number', sortable: true },
  ],
  insurance: [
    { id: 'name', label: '保單名稱', defaultVisible: true, defaultWidth: 200, sortable: true },
    { id: 'company', label: '保險公司', defaultVisible: true, defaultWidth: 120, sortable: true, filterable: true },
    { id: 'policyHolder', label: '持有人', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'member', label: '受保人', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'startDate', label: '開始日期', defaultVisible: true, defaultWidth: 110, sortable: true },
    { id: 'annualPremium', label: '本期年繳', defaultVisible: true, defaultWidth: 120, type: 'number', sortable: true },
    { id: 'totalPremium', label: '保單總供款', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'paidTotal', label: '已供款總額', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'progress', label: '進度', defaultVisible: true, defaultWidth: 140, sortable: true },
  ],
  insurancePolicies: [
    { id: 'name', label: '保單名稱', defaultVisible: true, defaultWidth: 200, sortable: true },
    { id: 'company', label: '保險公司', defaultVisible: true, defaultWidth: 120, sortable: true, filterable: true },
    { id: 'holderName', label: '持有人', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'insuredName', label: '受保人', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'annualPremium', label: '本期年繳', defaultVisible: true, defaultWidth: 120, type: 'number', sortable: true },
    { id: 'totalPremium', label: '保單總供款', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'progressText', label: '進度', defaultVisible: true, defaultWidth: 160, sortable: true },
  ],
  settlements: [
    { id: 'source', label: '來源', defaultVisible: true, defaultWidth: 90, sortable: true, filterable: true, filterOptions: [
      { value: 'personal', label: '🏷 個人支出' },
      { value: 'insurance', label: '🛡 保險扣款' },
    ]},
    { id: 'yearMonth', label: '年月', defaultVisible: true, defaultWidth: 90, sortable: true },
    { id: 'member', label: '成員', defaultVisible: true, defaultWidth: 90, sortable: true, filterable: true },
    { id: 'name', label: '項目名稱', defaultVisible: true, defaultWidth: 200, sortable: true },
    { id: 'amount', label: '金額', defaultVisible: true, defaultWidth: 110, type: 'number', sortable: true },
    { id: 'date', label: '日期', defaultVisible: true, defaultWidth: 100, sortable: true },
    { id: 'status', label: '狀態', defaultVisible: true, defaultWidth: 140, sortable: true, filterable: true },
  ],
  bankTransactions: [
    { id: 'date', label: '日期', defaultVisible: true, defaultWidth: 110, sortable: true },
    { id: 'bankName', label: '銀行', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'typeLabel', label: '類型', defaultVisible: true, defaultWidth: 90, sortable: true, filterable: true, filterOptions: [
      { value: 'in', label: '入帳' },
      { value: 'out', label: '出帳' },
      { value: 'transfer', label: '內部轉帳' },
    ]},
    { id: 'catLabel', label: '分類', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true, filterOptions: [
      { value: 'contribution', label: '家用轉入' },
      { value: 'expense', label: '支出' },
      { value: 'insurance', label: '保險' },
      { value: 'reimbursement', label: '代墊報銷' },
      { value: 'manual', label: '手動' },
    ]},
    { id: 'memberName', label: '成員', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'amount', label: '金額', defaultVisible: true, defaultWidth: 120, type: 'number', sortable: true },
    { id: 'note', label: '備註', defaultVisible: true, defaultWidth: 180 },
  ],
  recentExpense: [
    { id: 'date', label: '日期', defaultVisible: true, defaultWidth: 100, sortable: true },
    { id: 'memberName', label: '成員', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'name', label: '項目名稱', defaultVisible: true, defaultWidth: 200, sortable: true },
    { id: 'amount', label: '金額', defaultVisible: true, defaultWidth: 110, type: 'number', sortable: true },
    { id: 'status', label: '狀態', defaultVisible: true, defaultWidth: 120, sortable: true, filterable: true },
  ],
  memberReport: [
    { id: 'name', label: '成員', defaultVisible: true, defaultWidth: 140, sortable: true },
    { id: 'income', label: '家用轉入', defaultVisible: true, defaultWidth: 140, type: 'number', sortable: true },
    { id: 'personalIncome', label: '個人收入', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'expense', label: '支出', defaultVisible: true, defaultWidth: 140, type: 'number', sortable: true },
    { id: 'insurance', label: '保險', defaultVisible: true, defaultWidth: 140, type: 'number', sortable: true },
    { id: 'net', label: '淨額', defaultVisible: true, defaultWidth: 140, type: 'number', sortable: true },
  ],
  dashboardAnnual: [
    { id: 'year', label: '年度', defaultVisible: true, defaultWidth: 100, sortable: true },
    { id: 'totalIncome', label: '家庭收入', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'totalExpense', label: '總支出', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'insurance', label: '保險平攤', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'net', label: '淨餘額', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'avg', label: '每月平均', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
  ],
  annualSummary: [
    { id: 'name', label: '成員', defaultVisible: true, defaultWidth: 100, sortable: true },
    { id: 'income', label: '家用轉入', defaultVisible: true, defaultWidth: 120, type: 'number', sortable: true },
    { id: 'personalIncome', label: '個人收入', defaultVisible: true, defaultWidth: 120, type: 'number', sortable: true },
    { id: 'expense', label: '總支出', defaultVisible: true, defaultWidth: 120, type: 'number', sortable: true },
    { id: 'net', label: '淨結餘', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
  ],
  annualMonthly: [
    { id: 'date', label: '日期', defaultVisible: true, defaultWidth: 110, sortable: true },
    { id: 'memberName', label: '成員', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'categoryName', label: '類別', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'itemName', label: '項目', defaultVisible: true, defaultWidth: 120, sortable: true },
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 180, sortable: true },
    { id: 'amount', label: '金額', defaultVisible: true, defaultWidth: 110, type: 'number', sortable: true },
    { id: 'status', label: '狀態', defaultVisible: true, defaultWidth: 120, sortable: true, filterable: true },
  ],

  /* 🆕 v103.0.35（B34-4）：編號對照清單（code-list.html） */
  codeList: [
    { id: 'code', label: '編號', defaultVisible: true, defaultWidth: 180, sortable: true },
    { id: 'entityLabel', label: '類型', defaultVisible: true, defaultWidth: 120, sortable: true, filterable: true, filterOptions: [
      { value: '成員',     label: '成員' },
      { value: '銀行',     label: '銀行' },
      { value: '支出類別', label: '支出類別' },
      { value: '支出項目', label: '支出項目' },
      { value: '支付方式', label: '支付方式' },
      { value: '保險公司', label: '保險公司' },
      { value: '保單',     label: '保單' },
      { value: '基金',     label: '基金' },
      { value: '銀行帳號', label: '銀行帳號' },
    ]},
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 240, sortable: true },
  ],
};

/* ============================================
   RESOLVERS
   ============================================ */
export const RESOLVERS = {
  members: {
    role: (v) => escapeHtml(String(v || '—')),
    order: (v) => formatNumber(v),
  },
  banks: { order: (v) => formatNumber(v) },
  bankAccounts: {
    name: (v) => escapeHtml(v || '—'),
    type: (v) => {
      const l = BANK_ACCOUNT_TYPE_LABELS[v] || v || '—';
      const c = v === 'personal' ? 'badge-muted' : 'badge-info';
      return `<span class="badge ${c}">${escapeHtml(l)}</span>`;
    },
    initialBalance: (v) => `<span class="mono">${formatHKD(v)}</span>`,
    initialYM: (_, r) => `<span class="mono" style="font-size:12px;color:var(--text-muted);">${escapeHtml(r.initialYear || '—')}-${escapeHtml(r.initialMonth || '—')}</span>`,
  },
  categories: { order: (v) => formatNumber(v) },
  items: {
    categoryId: (v) => {
      if (!v) return '<span class="badge badge-muted">—</span>';
      const n = resolveName('categories', v) || v;
      return `<span class="badge badge-info">${escapeHtml(n)}</span>`;
    },
  },
  payments: { order: (v) => formatNumber(v) },
  statuses: {
    category: (v) => { const m = { personal: '個人支出', fixed: '固定支出', insurance: '保險' }; return escapeHtml(m[v] || v || '—'); },
    isDone: (v) => v ? '<span class="badge badge-success">是</span>' : '<span class="badge badge-muted">否</span>',
    order: (v) => formatNumber(v),
  },
  companies: { name: (v) => escapeHtml(v || '—') },
  funds: {
    name: (v) => escapeHtml(v || '（未命名）'),
    typeLabel: (v) => v === 'insurance' ? '<span class="badge badge-magenta">保險</span>' : '<span class="badge badge-info">獨立</span>',
    cost: (v) => `<span class="mono">${formatHKD(v)}</span>`,
    currentValue: (v) => `<span class="mono text-emerald">${formatHKD(v)}</span>`,
    pnl: (v, r) => {
      const p = (Number(r.currentValue) || 0) - (Number(r.cost) || 0);
      return `<span class="mono ${p >= 0 ? 'text-emerald' : 'text-red'}">${p >= 0 ? '+' : ''}${formatHKD(p)}</span>`;
    },
    units: (v) => `<span class="mono">${v || '—'}</span>`,
    note: (v) => escapeHtml(v || '—'),
  },
  fundSnapshots: {
    yearMonth: (_, r) => `${escapeHtml(r.year)}-${escapeHtml(r.month)}`,
    shares: (v) => `<span class="mono">${v || 0}</span>`,
    nav: (v) => `<span class="mono">${v || 0}</span>`,
    value: (v) => `<span class="mono text-cyan">${formatHKD(v)}</span>`,
    cumulativeCost: (v) => `<span class="mono text-magenta">${formatHKD(v)}</span>`,
    pnl: (v, r) => {
      const p = (Number(r.value) || 0) - (Number(r.cumulativeCost) || 0);
      return `<span class="mono ${p >= 0 ? 'text-emerald' : 'text-red'}">${p >= 0 ? '+' : ''}${formatHKD(p)}</span>`;
    },
  },
  insurance: {
    name: (v) => escapeHtml(v || '（未命名）'),
    company: (v) => escapeHtml(v || '—'),
    policyHolder: (v) => v ? escapeHtml(resolveName('members', v) || v) : '—',
    member: (v) => v ? escapeHtml(resolveName('members', v) || v) : '—',
    startDate: (_, r) => `<span class="mono" style="font-size:12px;">${escapeHtml(r.firstStartYear || '')}-${escapeHtml(r.firstStartMonth || '')}</span>`,
    annualPremium: (v) => `<span class="mono text-cyan">${formatHKD(v)}</span>`,
    totalPremium: (v) => `<span class="mono text-magenta">${formatHKD(v)}</span>`,
    paidTotal: (v) => `<span class="mono text-emerald">${formatHKD(v)}</span>`,
    progress: (_, r) => {
      const d = r.completedPeriods || 0;
      const t = r.totalPolicyPeriods || 0;
      const p = t > 0 ? Math.min(100, Math.round((d / t) * 100)) : 0;
      return `<div style="font-size:11px;color:var(--text-muted);font-family:var(--font-mono);margin-bottom:3px;">${d} / ${t} 期 (${p}%)</div><div class="progress" style="height:5px;"><div class="progress-bar" style="width:${p}%;"></div></div>`;
    },
  },
  insurancePolicies: {
    name: (v) => escapeHtml(v || '（未命名）'),
    company: (v) => escapeHtml(v || '—'),
    holderName: (_, r) => escapeHtml(resolveName('members', r.policyHolderId || r.memberId) || '—'),
    insuredName: (_, r) => escapeHtml(resolveName('members', r.memberId) || '—'),
    annualPremium: (v) => `<span class="mono text-cyan">${formatHKD(v)}</span>`,
    totalPremium: (v) => `<span class="mono text-magenta">${formatHKD(v)}</span>`,
    progressText: (_, r) => {
      if (r.type === 'fund_insurance') return '<span class="text-muted">基金保險</span>';
      const d = r.completedPeriods || 0;
      const t = r.totalPolicyPeriods || 0;
      const p = t > 0 ? Math.min(100, Math.round((d / t) * 100)) : 0;
      return `<div style="font-size:11px;color:var(--text-muted);font-family:var(--font-mono);">${d} / ${t} 期 (${p}%)</div><div class="progress" style="height:5px;"><div class="progress-bar" style="width:${p}%;"></div></div>`;
    },
  },
  settlements: {
    source: (_, r) => {
      const m = { personal: { cls: 'badge-info', label: r.memberId === 'shared' ? '🏠 家庭' : '🏷 個人' }, insurance: { cls: 'badge-success', label: '🛡 保險' } };
      const c = m[r.source] || m.personal;
      return `<span class="badge ${c.cls}">${c.label}</span>`;
    },
    yearMonth: (_, r) => `${escapeHtml(r.year)}-${escapeHtml(r.month)}`,
    member: (_, r) => r.memberId === RESERVED_IDS.SHARED_MEMBER ? '🏠 家庭共用' : (r.memberId ? escapeHtml(resolveName('members', r.memberId) || r.memberId) : '—'),
    name: (v) => escapeHtml(v || '—'),
    amount: (v) => formatHKD(v),
    date: (v) => escapeHtml(v || '—'),
    status: (v, r) => `<span class="badge ${statusBadgeClass(v)}">${escapeHtml(statusLabel(v, { source: r.source }))}</span>`,
  },
  bankTransactions: {
    date: (v) => `<span class="mono" style="font-size:12px;">${escapeHtml(v || '—')}</span>`,
    bankName: (v) => escapeHtml(v || '—'),
    typeLabel: (_, r) => `<span class="badge ${r.type === 'in' ? 'badge-success' : (r.type === 'transfer' ? 'badge-info' : 'badge-pending')}">${escapeHtml(BANK_TXN_TYPE_LABELS[r.type] || r.type)}</span>`,
    catLabel: (_, r) => `<span class="badge ${BANK_TXN_CATEGORY_BADGES[r.category] || 'badge-muted'}">${escapeHtml(BANK_TXN_CATEGORY_LABELS[r.category] || r.category)}</span>`,
    memberName: (v) => v ? escapeHtml(resolveName('members', v) || v) : '—',
    amount: (v, r) => `<span class="mono ${r.type === 'in' ? 'text-emerald' : 'text-red'}">${r.type === 'in' ? '+' : '-'}${formatHKD(v)}</span>`,
    note: (v) => escapeHtml(v || '—'),
  },
  recentExpense: {
    date: (v) => escapeHtml(v || '—'),
    memberName: (_, r) => r.memberId === RESERVED_IDS.SHARED_MEMBER ? '🏠 家庭共用' : escapeHtml(resolveName('members', r.memberId) || r.memberId || '—'),
    name: (v) => escapeHtml(v || '（未命名）'),
    amount: (v) => formatHKD(v),
    status: (v) => `<span class="badge ${v === 'done' ? 'badge-success' : 'badge-pending'}">${escapeHtml(v || '—')}</span>`,
  },
  memberReport: {
    name: (v, r) => escapeHtml(v) + (r.isShared ? ' <span class="badge badge-muted" style="font-size:10px;">🏠</span>' : ''),
    income: (_, r) => r.isShared ? '<span class="text-muted">—</span>' : `<span class="text-emerald">${formatHKD(r.income)}</span>`,
    personalIncome: (_, r) => (r.isShared || r.personalIncome === 0) ? '<span class="text-muted">—</span>' : `<span class="text-cyan">${formatHKD(r.personalIncome)}</span>`,
    expense: (_, r) => `<span class="text-red">${formatHKD(r.expense)}</span>`,
    insurance: (_, r) => r.insurance === 0 ? '<span class="text-muted">—</span>' : `<span class="text-magenta">${formatHKD(r.insurance)}</span>`,
    net: (_, r) => r.isShared ? '<span class="text-muted">—</span>' : `<span class="${r.net >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(r.net)}</span>`,
  },
  dashboardAnnual: {
    year: (v, r) => `${v} 年${r.isCurrent ? ' <span class="badge badge-info" style="font-size:10px;">今年</span>' : ''}`,
    totalIncome: (v) => `<span class="text-emerald">${formatHKD(v)}</span>`,
    totalExpense: (v) => `<span class="text-red">${formatHKD(v)}</span>`,
    insurance: (v) => `<span class="text-magenta">${formatHKD(v)}</span>`,
    net: (v) => `<span class="${v >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(v)}</span>`,
    avg: (v) => formatHKD(v),
  },
  annualSummary: {
    name: (v, r) => r.__isTotal ? `<b style="color:var(--neon-cyan);">${escapeHtml(v)}</b>` : (r.__isShared ? `<span class="text-magenta">${escapeHtml(v)}</span>` : escapeHtml(v)),
    income: (v) => v > 0 ? `<span class="text-emerald">${formatHKD(v)}</span>` : '<span class="text-muted">—</span>',
    personalIncome: (v) => v > 0 ? `<span class="text-cyan">${formatHKD(v)}</span>` : '<span class="text-muted">—</span>',
    expense: (v) => v > 0 ? `<span class="text-red">${formatHKD(v)}</span>` : '<span class="text-muted">—</span>',
    net: (v) => `<span class="${v >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(v)}</span>`,
  },
  annualMonthly: {
    date: (v) => `<span class="mono" style="font-size:12px;">${escapeHtml(v || '—')}</span>`,
    memberName: (v) => escapeHtml(v || '—'),
    categoryName: (v) => v ? `<span class="badge badge-info">${escapeHtml(v)}</span>` : '<span class="badge badge-muted">—</span>',
    itemName: (v) => escapeHtml(v || '—'),
    name: (v) => escapeHtml(v || '—'),
    amount: (v) => `<span class="mono text-red">${formatHKD(v)}</span>`,
    status: (v) => `<span class="badge ${v && v.startsWith('已') ? 'badge-success' : 'badge-pending'}">${escapeHtml(v || '—')}</span>`,
  },

  /* 🆕 v103.0.35（B34-4）：編號對照清單 */
  codeList: {
    code: (v) => v
      ? `<span class="mono" style="font-size:12px; color:var(--neon-cyan);">${escapeHtml(v)}</span>`
      : '<span class="mono" style="font-size:12px; color:var(--text-muted);">—</span>',
    entityLabel: (v) => `<span class="badge badge-info">${escapeHtml(v || '—')}</span>`,
    name: (v) => escapeHtml(v || '（未命名）'),
  },
};

/* ============================================
   對外 API
   ============================================ */
export function getColumns(key) {
  const c = COLUMNS[key];
  if (!c) { console.warn(`[column-registry] 未知 key：${key}`); return []; }
  return c.map((x) => ({ ...x }));
}

export function getResolvers(key) {
  const r = RESOLVERS[key];
  return r ? { ...r } : {};
}

export function getColumnsWith(key, ex = []) {
  return [...getColumns(key), ...ex];
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/config/column-registry.js
   Version: 1791901480
   Batch: B34-4
   Writer: A20261013-004#094
   ═══════════════════════════════════════════ */```

---

## 📄 js/config/constants.js

```
// ============================================
// constants.js — 全站常數集中管理（v103.0.39）
// 位置：js/config/constants.js
// ============================================
// v103.0.39（B34-4）：
//   ✅ ROUTES 加 CODE_LIST（database.html → code-list.html）
//   ✅ SIDEBAR_GROUPS 把「基礎資料庫」改為「編號對照清單」
// v103.0.38（B30-2）：
//   ✅ [關鍵] 加 DB_TARGET 解析（old / new）
// ============================================

/* ============================================
   0. DB 目標
   ============================================ */
const DB_TARGET_KEY = 'fin_db_target';

function _resolveDbTarget() {
  if (typeof window === 'undefined') return 'new';
  try {
    const params = new URLSearchParams(window.location.search);
    const fromUrl = params.get('db');
    if (fromUrl === 'old' || fromUrl === 'new') {
      localStorage.setItem(DB_TARGET_KEY, fromUrl);
      return fromUrl;
    }
    const saved = localStorage.getItem(DB_TARGET_KEY);
    if (saved === 'old' || saved === 'new') return saved;
  } catch (e) { /* noop */ }
  return 'new';
}

export const DB_TARGET = _resolveDbTarget();
const _P = `fin_${DB_TARGET}_`;

/* ============================================
   0.1 版本號
   ============================================ */
export const APP_VERSION = 'v103.0.39';
export const SW_VERSION = 'family-fin-v152';

/* ============================================
   1. localStorage Keys
   ============================================ */
export const STORAGE_KEYS = {
  YEAR: `${_P}year`,
  MONTH: `${_P}month`,
  FAMILY_ID: `${_P}family_id`,
  FAMILY_NAME: `${_P}family_name`,

  SIDEBAR_ORDER: `${_P}ui_sidebar_order`,
  SIDEBAR_COLLAPSED: `${_P}ui_sidebar_collapsed`,
  SIDEBAR_GROUPS: `${_P}ui_sidebar_groups`,
  MEMBERS_GROUP_OPEN: `${_P}ui_members_group_open`,

  STATS_MODE: `${_P}ui_stats_mode`,

  UI_PREFIX: `${_P}ui_`,
};

export const STORAGE_PREFIXES = {
  UI: `${_P}ui_`,
  COLUMN_SETTINGS: `${_P}ui_columns_`,
  COLLAPSE: `${_P}ui_collapse_`,
  VIEW: `${_P}ui_view_`,
  AUTH_CONTEXT: `${_P}auth_context_`,
  APP_CONFIG: `${_P}app_config_cache_`,
  BANK_CACHE: `${_P}bank_cache_`,
};

/* ============================================
   2. sessionStorage Keys
   ============================================ */
export const SESSION_KEYS = {
  AUTH_CONTEXT_PREFIX: `${_P}auth_context_`,
  APP_CONFIG_PREFIX: `${_P}app_config_cache_`,
  BANK_CACHE_PREFIX: `${_P}bank_cache_`,
};

export const AUTH_CONTEXT_TTL_MS = 5 * 60 * 1000;
export const APP_CONFIG_TTL_MS   = 5 * 60 * 1000;
export const BANK_CACHE_TTL_MS   = 2 * 60 * 1000;

/* ============================================
   3. 數值限制
   ============================================ */
export const LIMITS = {
  NAME_MAX_LEN_DESKTOP: 12,
  NAME_MAX_LEN_MOBILE: 6,
  NAME_INPUT_MAX: 60,
  NOTE_MAX: 200,
  AMOUNT_MAX: 1_000_000_000,
  YEAR_PAST_DEFAULT: 3,
  YEAR_FUTURE_DEFAULT: 5,
  TOAST_DURATION_DEFAULT: 2000,
  PAGE_FILTER_MONTH_DEFAULT: 'all',
  DEFAULT_TABLE_PAGE_SIZE: 20,
  RECENT_LIST_LIMIT: 20,
};

/* ============================================
   4. 保留 ID
   ============================================ */
export const RESERVED_IDS = {
  EXTRA_INCOME: 'extra',
  SHARED_MEMBER: 'shared',
};

/* ============================================
   5. 角色 / 來源
   ============================================ */
export const ROLES = {
  SUPERADMIN: 'superadmin',
  OWNER: 'owner',
  MEMBER: 'member',
};

export const SOURCES = {
  PERSONAL: 'personal',
  FIXED: 'fixed',
  INSURANCE: 'insurance',
  INCOME: 'income',
};

/* ============================================
   6. 保險連動前綴
   ============================================ */
export const LINKED_PREFIX = 'linked_';

export function buildLinkedKey(policyId) {
  return `${LINKED_PREFIX}${policyId}`;
}

export function isLinkedKey(key) {
  return typeof key === 'string' && key.startsWith(LINKED_PREFIX);
}

/* ============================================
   7. 銀行交易類型
   ============================================ */
export const BANK_TXN_TYPES = {
  IN: 'in',
  OUT: 'out',
  TRANSFER: 'transfer',
};

export const BANK_TXN_TYPE_LABELS = {
  in: '入帳',
  out: '出帳',
  transfer: '內部轉帳',
};

/* ============================================
   8. 銀行交易分類
   ============================================ */
export const BANK_TXN_CATEGORIES = {
  CONTRIBUTION: 'contribution',
  EXPENSE: 'expense',
  INSURANCE: 'insurance',
  REIMBURSEMENT: 'reimbursement',
  MANUAL: 'manual',
};

export const BANK_TXN_CATEGORY_LABELS = {
  contribution: '家用轉入',
  expense: '支出',
  insurance: '保險',
  reimbursement: '代墊報銷',
  manual: '手動',
};

export const BANK_TXN_CATEGORY_BADGES = {
  contribution: 'badge-success',
  expense: 'badge-pending',
  insurance: 'badge-magenta',
  reimbursement: 'badge-info',
  manual: 'badge-muted',
};

/* ============================================
   9. 銀行帳號類型
   ============================================ */
export const BANK_ACCOUNT_TYPES = {
  FAMILY: 'family',
  PERSONAL: 'personal',
};

export const BANK_ACCOUNT_TYPE_LABELS = {
  family: '家庭帳號',
  personal: '個人帳號',
};

/* ============================================
   10. 保單付款模式
   ============================================ */
export const PAYMENT_MODES = {
  DIRECT: 'direct',
  ADVANCE: 'advance',
};

export const PAYMENT_MODE_LABELS = {
  direct: '直接付款',
  advance: '代墊模式',
};

/* ============================================
   11. 平台資源對照表
   ============================================ */
export const PLATFORM_RESOURCES = {
  members:     { path: 'members',             type: 'list' },
  banks:       { path: 'banks',               type: 'list' },
  companies:   { path: 'insurance_companies', type: 'list' },
  payments:    { path: 'payment_methods',     type: 'list' },
  categories:  { path: 'expense_categories',  type: 'list' },
  items:       { path: 'expense_items',       type: 'list' },
  statuses:    { path: 'statuses',            type: 'list' },
  options:     { path: 'options',             type: 'object' },
  yearRange:   { path: 'year_range',          type: 'object' },
  uiConstants: { path: 'ui_constants',        type: 'object' },
};

/* ============================================
   12. 實體識別碼
   ============================================ */
export const ENTITY_KEYS = {
  MEMBER:   'member',
  BANK:     'bank',
  POLICY:   'policy',
  FUND:     'fund',
  CATEGORY: 'category',
  ITEM:     'item',
  PAYMENT:  'payment',
  STATUS:   'status',
};

/* ============================================
   12.1 編號前綴（D-008）
   ============================================ */
export const CODE_PREFIXES = {
  member:      'MEM',
  bank:        'PAY-BANK',
  bankAccount: 'PAY-BANK',
  category:    'CAT',
  item:        'ITEM',
  payment:     'PAY',
  company:     'COMP',
  policy:      'POL',
  fund:        'FUND',
  income:      'INC',
  bankTxn:     'TXN',
};

export const POLICY_SUBTYPE_MAP = {
  normal:         'NORM',
  fund_insurance: 'FUND',
  savings:        'SAVE',
};

/* ============================================
   13. 狀態 badge class 對照
   ============================================ */
export const STATUS_BADGE_CLASS = {
  done:    'badge-success',
  pending: 'badge-pending',
  skipped: 'badge-muted',
  info:    'badge-info',
};

export function getStatusBadgeClass(isDone, isSkipped = false) {
  if (isSkipped) return STATUS_BADGE_CLASS.skipped;
  return isDone ? STATUS_BADGE_CLASS.done : STATUS_BADGE_CLASS.pending;
}

/* ============================================
   14. policyHolderId fallback 規則
   ============================================ */
export const POLICY_HOLDER_FALLBACK = {
  primary: 'policyHolderId',
  fallback: 'memberId',
};

export function getPolicyEffectiveMemberId(policy) {
  if (!policy) return '';
  return policy.policyHolderId || policy.memberId || '';
}

/* ============================================
   15. 表格欄位設定前綴
   ============================================ */
export const COLUMN_SETTINGS_PREFIX = `${_P}ui_columns_`;

export function buildColumnSettingsKey(tableId) {
  return `${COLUMN_SETTINGS_PREFIX}${tableId}`;
}

/* ============================================
   16. 帳號網域
   ============================================ */
export const SUPERADMIN_DOMAIN = '@familyfin.local';
export const SUPERADMIN_EMAIL  = `superadmin${SUPERADMIN_DOMAIN}`;

/* ============================================
   17. 預設狀態清單
   ============================================ */
export const DEFAULT_STATUSES = [
  { key: 'status_untreated', name: '未處理', category: 'personal',  isDone: false, order: 1 },
  { key: 'status_done',      name: '已處理', category: 'personal',  isDone: true,  order: 2 },
  { key: 'status_unrepaid',  name: '未還款', category: 'personal',  isDone: false, order: 3 },
  { key: 'status_repaid',    name: '已還款', category: 'personal',  isDone: true,  order: 4 },
  { key: 'status_unpaid',    name: '未付款', category: 'fixed',     isDone: false, order: 5 },
  { key: 'status_paid',      name: '已付款', category: 'fixed',     isDone: true,  order: 6 },
  { key: 'status_na',        name: '不適用', category: 'fixed',     isDone: true,  order: 7 },
  { key: 'status_unbilled',  name: '未扣款', category: 'insurance', isDone: false, order: 8 },
  { key: 'status_billed',    name: '已扣款', category: 'insurance', isDone: true,  order: 9 },
];

/* ============================================
   18. 預設下拉選項
   ============================================ */
export const DEFAULT_OPTIONS = {
  memberRoles: [
    { value: 'husband', label: '老公 / 丈夫' },
    { value: 'wife',    label: '老婆 / 妻子' },
    { value: 'child',   label: '子女' },
    { value: 'other',   label: '其他' },
  ],
  cycles: [
    { value: '每月',    label: '每月' },
    { value: '每2個月', label: '每2個月' },
    { value: '每季',    label: '每季' },
    { value: '每年',    label: '每年' },
    { value: '一次性',  label: '一次性' },
  ],
  policyTypes: [
    { value: 'normal',         label: '普通類保險（住院 / 人壽 / 意外）' },
    { value: 'fund_insurance', label: '基金類保險（投資型，月供）' },
    { value: 'savings',        label: '儲蓄類保險' },
  ],
  insurancePaymentTypes: [
    { value: '年繳',     label: '年繳' },
    { value: '月繳',     label: '月繳' },
    { value: '一次付款', label: '一次付款' },
  ],
  paymentModes: [
    { value: 'direct',  label: '直接付款（家庭帳號扣款）' },
    { value: 'advance', label: '代墊模式（成員墊付，家庭月攤）' },
  ],
  categoryOrder: ['銀行類', '醫療類', '學校類', '保險類', '固定費用類', '交通類', '其他'],
};

export const DEFAULT_YEAR_RANGE = {
  startYear: null,
  futureYears: LIMITS.YEAR_FUTURE_DEFAULT,
};

export const DEFAULT_UI_CONSTANTS = {
  nameMaxLenDesktop: LIMITS.NAME_MAX_LEN_DESKTOP,
  nameMaxLenMobile:  LIMITS.NAME_MAX_LEN_MOBILE,
  toastDuration:     LIMITS.TOAST_DURATION_DEFAULT,
};

export const DEFAULT_COMPANIES = ['富通', '保誠', 'FWD', 'AIA', '宏利', 'AXA'];

export const DEFAULT_MEMBERS = [
  { name: '成員1', role: 'husband', order: 0 },
  { name: '成員2', role: 'wife',    order: 1 },
  { name: '成員3', role: 'child',   order: 2 },
  { name: '成員4', role: 'child',   order: 3 },
];

export const DEFAULT_CATEGORIES = [
  { key: 'cat_medical',   name: '醫療類',     shortCode: 'MED', order: 1 },
  { key: 'cat_school',    name: '學校類',     shortCode: 'SCH', order: 2 },
  { key: 'cat_insurance', name: '保險類',     shortCode: 'INS', order: 3 },
  { key: 'cat_fixed',     name: '固定費用類', shortCode: 'FIX', order: 4 },
  { key: 'cat_other',     name: '其他',       shortCode: 'OTH', order: 5 },
];

export const DEFAULT_ITEMS = [
  { categoryKey: 'cat_medical',   name: '看病-一般' },
  { categoryKey: 'cat_medical',   name: '看病-專科' },
  { categoryKey: 'cat_medical',   name: '牙醫' },
  { categoryKey: 'cat_medical',   name: '藥費' },
  { categoryKey: 'cat_school',    name: '學費' },
  { categoryKey: 'cat_school',    name: '功課輔導班' },
  { categoryKey: 'cat_school',    name: '興趣班' },
  { categoryKey: 'cat_school',    name: '書本費' },
  { categoryKey: 'cat_school',    name: '校車費' },
  { categoryKey: 'cat_insurance', name: '住院保險' },
  { categoryKey: 'cat_insurance', name: '人壽保險' },
  { categoryKey: 'cat_insurance', name: '意外保險' },
  { categoryKey: 'cat_fixed',     name: '水費' },
  { categoryKey: 'cat_fixed',     name: '電費' },
  { categoryKey: 'cat_fixed',     name: '煤氣費' },
  { categoryKey: 'cat_fixed',     name: '管理費' },
  { categoryKey: 'cat_fixed',     name: '房租' },
  { categoryKey: 'cat_other',     name: '其他' },
];

export const DEFAULT_PAYMENTS = [
  { name: '現金',   order: 1 },
  { name: '中銀',   order: 2 },
  { name: '匯豐',   order: 3 },
  { name: '恆生',   order: 4 },
  { name: '信用卡', order: 5 },
];

/* ============================================
   19. 側邊欄分類群組（依角色過濾）
   ============================================ */
const _ROLES_FAMILY = ['owner', 'member'];
const _ROLES_ALL    = ['owner', 'member', 'superadmin'];
const _ROLES_ADMIN  = ['superadmin'];

export const SIDEBAR_GROUPS = [
  {
    key: 'overview',
    label: '總覽',
    icon: 'home',
    defaultOpen: true,
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'home', label: '總覽儀表板', href: 'index.html' },
    ],
  },
  {
    key: 'record',
    label: '記錄中心',
    icon: 'pencil',
    defaultOpen: true,
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'pencil',   label: '綜合輸入中心', href: 'input-center.html' },
      { icon: 'database', label: '編號對照清單', href: 'code-list.html' },
    ],
  },
  {
    key: 'insurance-fund',
    label: '保險與基金',
    icon: 'shield',
    defaultOpen: false,
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'shield',     label: '保險清單表', href: 'insurance.html' },
      { icon: 'line-chart', label: '基金投資表', href: 'portfolio.html' },
    ],
  },
  {
    key: 'settle',
    label: '對帳',
    icon: 'clipboard-check',
    defaultOpen: false,
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'clipboard-check', label: '結算清單', href: 'settlements.html' },
    ],
  },
  {
    key: 'assets',
    label: '資產',
    icon: 'landmark',
    defaultOpen: false,
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'landmark', label: '銀行交易', href: 'finance-overview.html' },
    ],
  },
  {
    key: 'reports',
    label: '報表',
    icon: 'bar-chart-3',
    defaultOpen: false,
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'bar-chart-3', label: '年度報表', href: 'annual-report.html' },
      { icon: 'users',       label: '成員與家庭收入與支出明細', href: 'member-report.html' },
    ],
  },
  {
    key: 'platform',
    label: '平台管理',
    icon: 'server',
    defaultOpen: true,
    roles: _ROLES_ADMIN,
    items: [
      { icon: 'home', label: '家庭管理', href: 'admin.html' },
    ],
  },
  {
    key: 'system',
    label: '系統',
    icon: 'settings',
    defaultOpen: false,
    roles: _ROLES_ALL,
    items: [
      { icon: 'settings', label: '系統設定', href: 'settings.html' },
    ],
  },
];

export function getSidebarGroupsByRole(role) {
  if (!role) return SIDEBAR_GROUPS.filter((g) => (g.roles || []).includes('owner'));
  return SIDEBAR_GROUPS.filter((g) => (g.roles || []).includes(role));
}

/* ============================================
   20. 路由 / 頁面清單
   ============================================ */
export const ROUTES = {
  LOGIN:            'login.html',
  REGISTER:         'register.html',
  DASHBOARD:        'index.html',
  ADMIN:            'admin.html',
  INPUT_CENTER:     'input-center.html',
  DATABASE:         'database.html',    /* @deprecated 保留相容 */
  CODE_LIST:        'code-list.html',   /* 🆕 v103.0.39（B34-4） */
  INSURANCE:        'insurance.html',
  PORTFOLIO:        'portfolio.html',
  SETTLEMENTS:      'settlements.html',
  FINANCE_OVERVIEW: 'finance-overview.html',
  ANNUAL_REPORT:    'annual-report.html',
  MEMBER_REPORT:    'member-report.html',
  SETTINGS:         'settings.html',
};

/* ============================================
   21. 快速摘要卡類型
   ============================================ */
export const QUICK_SUMMARY_TYPES = {
  TOP_CATEGORIES:   'top-categories',
  RECENT_ACTIVITY:  'recent-activity',
  MEMBER_TREND:     'member-trend',
  PENDING_FIXED:    'pending-fixed',
  ASSET_PIE:        'asset-pie',
};

/* ============================================
   22. 狀態分類
   ============================================ */
export const STATUS_CATEGORIES = {
  PERSONAL:  'personal',
  FIXED:     'fixed',
  INSURANCE: 'insurance',
};

/* ============================================
   23. DB 顯示名稱
   ============================================ */
export const DB_LABELS = {
  old: 'family-fin',
  new: '2026-10-12-family',
};

export function getDbLabel() {
  return DB_LABELS[DB_TARGET] || DB_TARGET;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/config/constants.js
   Version: 1791901300
   Batch: B34-4
   Writer: A20261013-004#091
   ═══════════════════════════════════════════ */```

---

## 📄 js/config/entity-registry.js

```
// ============================================
// entity-registry.js — 名稱解析 SSOT（v103.0.22）
// 位置：js/config/entity-registry.js
// ============================================
// v103.0.22（B25-4）：
//   ✅ R6 修正：ENTITY_TYPE_MAP 移除未訂閱的 STATUS（無效對映）
// ============================================

import { ref, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { db } from './firebase-config.js';
import { AppState } from '../core/state.js';
import { ENTITY_KEYS } from './constants.js';

const PATHS = {
  members:       'members',
  banks:         'banks',
  bank_accounts: 'bank_accounts',
  categories:    'expense_categories',
  items:         'expense_items',
  companies:     'insurance_companies',
  payments:      'payment_methods',
};

function byOrderThenCreated(a, b) {
  const oa = a.order != null ? a.order : Number.MAX_SAFE_INTEGER;
  const ob = b.order != null ? b.order : Number.MAX_SAFE_INTEGER;
  if (oa !== ob) return oa - ob;
  return (a.createdAt || 0) - (b.createdAt || 0);
}

function byCreatedAt(a, b) {
  return (a.createdAt || 0) - (b.createdAt || 0);
}

const _lists = {};
const _nameMaps = {};
const _unsubscribers = [];
let _initialized = false;

export function init() {
  if (_initialized) return true;

  const familyId = AppState.getFamilyId();
  if (!familyId) {
    console.warn('[entity-registry] 尚未選擇家庭，略過初始化');
    return false;
  }

  Object.entries(PATHS).forEach(([key, path]) => {
    const r = ref(db, `families/${familyId}/${path}`);
    const sortFn = (key === 'members' || key === 'banks' || key === 'bank_accounts')
      ? byOrderThenCreated
      : byCreatedAt;

    const unsub = onValue(
      r,
      (snap) => {
        const val = snap.val() || {};
        const list = Object.entries(val).map(([id, x]) => ({ id, ...x }));
        list.sort(sortFn);

        _lists[key] = list;

        const map = {};
        list.forEach((x) => { map[x.id] = x.name || ''; });
        _nameMaps[key] = map;
      },
      (err) => {
        console.warn(`[entity-registry] 讀取 ${key} 失敗：`, err);
        _lists[key] = _lists[key] || [];
        _nameMaps[key] = _nameMaps[key] || {};
      }
    );

    _unsubscribers.push(unsub);
  });

  _initialized = true;
  return true;
}

export function destroy() {
  _unsubscribers.forEach((unsub) => {
    try { unsub(); } catch (e) { /* noop */ }
  });
  _unsubscribers.length = 0;

  Object.keys(_lists).forEach((k) => { delete _lists[k]; });
  Object.keys(_nameMaps).forEach((k) => { delete _nameMaps[k]; });

  _initialized = false;
}

export function isInitialized() {
  return _initialized;
}

export function resolveName(type, id, fallback = '') {
  if (!id) return fallback;
  const map = _nameMaps[type];
  if (!map) return fallback;
  return map[id] || fallback;
}

export function getList(type) {
  return [...(_lists[type] || [])];
}

export function getNameMap(type) {
  return { ...(_nameMaps[type] || {}) };
}

export function resolveNames(type, ids = []) {
  const result = {};
  ids.forEach((id) => { result[id] = resolveName(type, id, ''); });
  return result;
}

/* 🆕 v103.0.22：
   - STATUS / FUND 不在此 registry 訂閱 → 移除映射
   - POLICY 語意上指向 members（保單持有人 fallback）→ 保留 */
export const ENTITY_TYPE_MAP = {
  [ENTITY_KEYS.MEMBER]:   'members',
  [ENTITY_KEYS.BANK]:     'banks',
  [ENTITY_KEYS.CATEGORY]: 'categories',
  [ENTITY_KEYS.ITEM]:     'items',
  [ENTITY_KEYS.PAYMENT]:  'payments',
  [ENTITY_KEYS.POLICY]:   'members',
};

export function resolveEntityName(entityKey, id, fallback = '') {
  const type = ENTITY_TYPE_MAP[entityKey];
  if (!type) return fallback;
  return resolveName(type, id, fallback);
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/config/entity-registry.js
   Version: v103.0.22
   Batch: B25-4
   ═══════════════════════════════════════════ */```

---

## 📄 js/config/firebase-config.js

```
// ============================================
// firebase-config.js — Firebase 初始化設定（v103.0.38）
// 位置：js/config/firebase-config.js
// ============================================
// v103.0.38（B30-2）：
//   ✅ DB_TARGET 解析上移至 constants.js（SSOT）
//   ✅ 依 DB_TARGET 選對應 config
// v103.0.22（B25-5）：
//   ✅ 補 END OF FILE
// ============================================

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
import { getAuth } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
import { getDatabase } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { DB_TARGET } from './constants.js';

/* ---------- 兩套環境設定 ---------- */
const FIREBASE_CONFIGS = {
  old: {
    apiKey: "AIzaSyCQlrNdorKJI9xsqr4m4ME046lrubo9Y7I",
    authDomain: "family-fin-a6dd1.firebaseapp.com",
    databaseURL: "https://family-fin-a6dd1-default-rtdb.asia-southeast1.firebasedatabase.app",
    projectId: "family-fin-a6dd1",
    storageBucket: "family-fin-a6dd1.firebasestorage.app",
    messagingSenderId: "382147296734",
    appId: "1:382147296734:web:42bdbd7401001e75e1f14f",
    measurementId: "G-JKK4TZLPQC"
  },
  new: {
    apiKey: "AIzaSyBJCK83RQyHbol4yJ4UDo3pOUXpUgcW9hU",
    authDomain: "family-3d0a5.firebaseapp.com",
    databaseURL: "https://family-3d0a5-default-rtdb.asia-southeast1.firebasedatabase.app",
    projectId: "family-3d0a5",
    storageBucket: "family-3d0a5.firebasestorage.app",
    messagingSenderId: "109775445681",
    appId: "1:109775445681:web:84e974db35951d0a6ecc90"
  },
};

const firebaseConfig = FIREBASE_CONFIGS[DB_TARGET];

console.log(`[firebase-config] DB=${DB_TARGET} project=${firebaseConfig.projectId}`);

export const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getDatabase(app);

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/config/firebase-config.js
   Version: v103.0.38
   Batch: B30-2
   ═══════════════════════════════════════════ */```

---

## 📄 js/config/label-registry.js

```
// ============================================
// label-registry.js — 顯示文字 SSOT（v103.0.0）
// 位置：js/config/label-registry.js
// ============================================
// 職責：
//   1. 集中管理全站顯示文字（中文）
//   2. 提供 L(path, params) 取用 API
//   3. 支援參數替換（{name} / {n} 等）
//
// 設計原則：
//   - 任何 UI 文字只存在此處
//   - 依用途分類（common / pages / fields / actions / ...）
//   - 支援巢狀路徑（'pages.settlements'）
// ============================================

/* ============================================
   1. 文字表
   ============================================ */
export const LABELS = {

  /* ---------- 通用 ---------- */
  common: {
    yes:        '是',
    no:         '否',
    ok:         '確定',
    cancel:     '取消',
    save:       '儲存',
    delete:     '刪除',
    edit:       '編輯',
    add:        '新增',
    reset:      '重置',
    close:      '關閉',
    back:       '返回',
    confirm:    '確認',
    loading:    '載入中…',
    submitting: '處理中…',
    noData:     '尚無資料',
    unknown:    '未知',
    unnamed:    '（未命名）',
    all:        '全部',
    none:       '—',
  },

  /* ---------- 頁面標題 ---------- */
  pages: {
    dashboard:        '總覽儀表板',
    portfolio:        '基金投資表',
    annualReport:     '年度報表',
    settings:         '系統設定',
    financeOverview:  '銀行交易',
    memberReport:     '成員與家庭收入與支出明細',
    inputCenter:      '綜合輸入中心',
    database:         '基礎資料庫',
    settlements:      '結算清單',
    insurance:        '保險清單表',
    admin:            '平台管理',
    login:            '登入',
    register:         '註冊',
  },

  /* ---------- 欄位 ---------- */
  fields: {
    name:            '名稱',
    member:          '成員',
    year:            '年份',
    month:           '月份',
    date:            '日期',
    amount:          '金額',
    status:          '狀態',
    category:        '類別',
    item:            '項目',
    paymentMethod:   '支付方式',
    bank:            '銀行',
    bankAccount:     '銀行帳號',
    policy:          '保單',
    fund:            '基金',
    company:         '保險公司',
    note:            '備註',
    order:           '排序',
    role:            '角色',
    displayName:     '顯示名稱',
    account:         '帳號',
    password:        '密碼',
    createdAt:       '建立時間',
    updatedAt:       '更新時間',
    source:          '來源',
    type:            '類型',
    progress:        '進度',
  },

  /* ---------- 動作 ---------- */
  actions: {
    addMember:           '新增成員',
    addBank:             '新增銀行',
    addPolicy:           '新增保單',
    addFund:             '新增基金',
    addExpense:          '新增支出',
    addContribution:     '家用轉入',
    addPersonalIncome:   '個人收入',
    addTransaction:      '新增交易',
    addBankAccount:      '新增銀行帳號',
    syncAll:             '同步所有支出',
    exportExcel:         '匯出 Excel',
    logout:              '登出',
    manageAccounts:      '管理帳號',
    initFamily:          '初始化',
    deleteFamily:        '刪除',
    clearOldBalances:    '清除舊銀行結餘資料',
  },

  /* ---------- 確認對話框 ---------- */
  confirm: {
    logoutTitle:        '登出',
    logoutMessage:      '確定要登出嗎？',
    deleteTitle:        '刪除',
    deleteMemberTitle:  '刪除成員',
    deletePolicyTitle:  '刪除保單',
    deleteFundTitle:    '刪除基金',
    initFamilyTitle:    '初始化家庭',
    deleteFamilyTitle:  '刪除家庭',
    clearOldBalancesTitle: '清除舊銀行結餘',
  },

  /* ---------- Toast 訊息 ---------- */
  toast: {
    saved:           '✅ 已儲存',
    updated:         '✅ 已更新',
    added:           '✅ 已新增',
    deleted:         '✅ 已刪除',
    saveFailed:      '儲存失敗',
    updateFailed:    '更新失敗',
    addFailed:       '新增失敗',
    deleteFailed:    '刪除失敗',
    loadFailed:      '載入失敗',
    noChanges:       '沒有變更',
    networkError:    '網路錯誤',
    loginExpired:    '登入已過期，請重新登入',
  },

  /* ---------- 空狀態 ---------- */
  emptyState: {
    noMembers:       '尚無成員',
    noBanks:         '尚無銀行',
    noPolicies:      '尚無保單資料',
    noFunds:         '尚無基金持倉',
    noExpenses:      '尚無支出紀錄',
    noTransactions:  '尚無交易記錄',
    noData:          '尚無資料',
    noResults:       '此條件下無結果',
  },

  /* ---------- 側邊欄 / 導覽 ---------- */
  nav: {
    brand:           '◈ FAMILY.FIN',
    menuToggle:      '切換選單',
    yearLabel:       '年份',
    monthLabel:      '月份',
  },

  /* ---------- 角色 / 權限 ---------- */
  roles: {
    superadmin:      '超級管理員',
    owner:           '家庭擁有者',
    member:          '家庭成員',
    readonly:        '唯讀',
  },
};

/* ============================================
   2. 取用 API
   ============================================ */

/**
 * 依路徑取得文字，支援參數替換
 *
 * 用法：
 *   L('common.yes')                          → '是'
 *   L('pages.dashboard')                     → '總覽儀表板'
 *   L('toast.loadFailed', { name: '成員' })  → '載入失敗：成員'
 *
 * @param {string} path - 點分隔路徑，例如 'pages.dashboard'
 * @param {Object} [params] - 參數替換（{key} → value）
 * @returns {string}
 */
export function L(path, params) {
  if (!path) return '';

  const parts = String(path).split('.');
  let node = LABELS;

  for (const part of parts) {
    if (node == null) break;
    node = node[part];
  }

  // 找不到 → 回傳路徑本身（方便除錯）
  if (typeof node !== 'string') {
    console.warn(`[label-registry] 找不到文字路徑：${path}`);
    return path;
  }

  // 參數替換
  if (params && typeof params === 'object') {
    return node.replace(/\{(\w+)\}/g, (match, key) => {
      return params[key] != null ? String(params[key]) : match;
    });
  }

  return node;
}

/**
 * 是否存在指定路徑
 */
export function has(path) {
  const parts = String(path || '').split('.');
  let node = LABELS;
  for (const part of parts) {
    if (node == null || typeof node !== 'object') return false;
    node = node[part];
  }
  return typeof node === 'string';
}

/**
 * 取得整個分類（回傳複本）
 */
export function getGroup(groupName) {
  const group = LABELS[groupName];
  if (!group || typeof group !== 'object') return {};
  return { ...group };
}
```

---

## 📄 js/config/page-metadata-registry.js

```
// ============================================
// page-metadata-registry.js — 頁面 metadata 預設（v103.0.34）
// 位置：js/config/page-metadata-registry.js
// ============================================
// v103.0.34（B29-5a）：
//   ✅ 每頁預設 title / subtitleStatic
//   ✅ getPageKeyFromUrl（從 URL 取 pageKey）
//   ✅ getDefaultMetadata（依 pageKey 取預設）
// ============================================

/* ============================================
   頁面預設 metadata
   - pageKey = URL 檔名（不含 .html）
   - title：頁面標題
   - subtitleStatic：副標題靜態段（使用者可編輯）
   ============================================ */
export const PAGE_METADATA = {
  index: {
    title: '總覽儀表板',
    subtitleStatic: '',
  },
  'input-center': {
    title: '綜合輸入中心',
    subtitleStatic: '所有交易資料的統一輸入入口',
  },
  database: {
    title: '基礎資料庫',
    subtitleStatic: '管理基礎資料與選項',
  },
  insurance: {
    title: '保險清單表',
    subtitleStatic: '',
  },
  portfolio: {
    title: '基金投資表',
    subtitleStatic: '基金持倉與最新現值',
  },
  settlements: {
    title: '結算清單',
    subtitleStatic: '',
  },
  'finance-overview': {
    title: '銀行交易',
    subtitleStatic: '',
  },
  'annual-report': {
    title: '年度報表',
    subtitleStatic: '全年總合與明細',
  },
  'member-report': {
    title: '成員與家庭收入與支出明細',
    subtitleStatic: '',
  },
  settings: {
    title: '系統設定',
    subtitleStatic: '平台設定與個人化',
  },
};

/* ============================================
   對外 API
   ============================================ */

/**
 * 從 URL 取得 pageKey（不含 .html）
 * @returns {string}
 */
export function getPageKeyFromUrl() {
  try {
    const path = location.pathname.split('/').pop() || 'index.html';
    const key = path.replace(/\.html$/, '');
    return key || 'index';
  } catch (e) {
    return 'index';
  }
}

/**
 * 取得某頁的預設 metadata
 * @param {string} pageKey
 * @returns {{ title: string, subtitleStatic: string }}
 */
export function getDefaultMetadata(pageKey) {
  return PAGE_METADATA[pageKey] || { title: '', subtitleStatic: '' };
}

/**
 * 列出所有可編輯頁面（供 settings UI 用）
 * @returns {Array<{ key: string, title: string }>}
 */
export function listPages() {
  return Object.entries(PAGE_METADATA).map(([key, meta]) => ({
    key,
    title: meta.title,
  }));
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/config/page-metadata-registry.js
   Version: v103.0.34
   Batch: B29-5a
   ═══════════════════════════════════════════ */```

---

## 📄 js/config/status-registry.js

```
// ============================================
// status-registry.js — 狀態系統 SSOT（v103.0.0）
// 位置：js/config/status-registry.js
// ============================================
// 職責：
//   1. 定義標準狀態代碼（pending / done / skipped）
//   2. 讀取時將舊名稱正規化為新代碼（normalize）
//   3. 依來源提供顯示文字（label / options）
//   4. 提供 isDone / badgeClass 判定
//
// 設計原則：
//   - 全站狀態知識只存在此處
//   - 讀取時 normalize，寫入時直接寫新代碼
//   - 舊資料 100% 相容（LEGACY_MAP）
// ============================================

/* ============================================
   1. 標準狀態代碼
   ============================================ */
export const STATUS = {
  PENDING: 'pending',
  DONE:    'done',
  SKIPPED: 'skipped',
};

/* ============================================
   2. isDone 判定表
   ============================================ */
export const IS_DONE = {
  [STATUS.PENDING]: false,
  [STATUS.DONE]:    true,
  [STATUS.SKIPPED]: true,
};

/* ============================================
   3. 顯示文字（依來源）
   -------------------------------------------------
   - default：未指定來源時的通用文字
   - bySource：指定來源時的專屬文字
   ============================================ */
export const LABELS = {
  default: {
    [STATUS.PENDING]: '未處理',
    [STATUS.DONE]:    '已處理',
    [STATUS.SKIPPED]: '不適用',
  },
  bySource: {
    personal: {
      [STATUS.PENDING]: '未付款',
      [STATUS.DONE]:    '已付款',
      [STATUS.SKIPPED]: '不適用',
    },
    fixed: {
      [STATUS.PENDING]: '未付款',
      [STATUS.DONE]:    '已付款',
      [STATUS.SKIPPED]: '不適用',
    },
    insurance: {
      [STATUS.PENDING]: '未扣款',
      [STATUS.DONE]:    '已扣款',
      [STATUS.SKIPPED]: '不適用',
    },
    income: {
      [STATUS.PENDING]: '未轉入',
      [STATUS.DONE]:    '已轉入',
      [STATUS.SKIPPED]: '不適用',
    },
  },
};

/* ============================================
   4. 舊名稱 → 新代碼（相容層）
   -------------------------------------------------
   涵蓋 v102 之前所有曾出現過的狀態名稱
   ============================================ */
export const LEGACY_MAP = {
  // pending 類
  '未處理': STATUS.PENDING,
  '未還款': STATUS.PENDING,
  '未付款': STATUS.PENDING,
  '未扣款': STATUS.PENDING,
  '未轉入': STATUS.PENDING,

  // done 類
  '已處理': STATUS.DONE,
  '已還款': STATUS.DONE,
  '已付款': STATUS.DONE,
  '已扣款': STATUS.DONE,
  '已轉入': STATUS.DONE,

  // skipped 類
  '不適用': STATUS.SKIPPED,
};

/* ============================================
   5. 反查：新代碼 → 舊名稱（依來源）
   -------------------------------------------------
   供舊資料寫入 / 相容 API 使用（如 summary.js 仍讀 '已扣款'）
   ============================================ */
export const LABELS_BY_SOURCE = {
  personal:  { [STATUS.PENDING]: '未處理', [STATUS.DONE]: '已處理', [STATUS.SKIPPED]: '不適用' },
  fixed:     { [STATUS.PENDING]: '未付款', [STATUS.DONE]: '已付款', [STATUS.SKIPPED]: '不適用' },
  insurance: { [STATUS.PENDING]: '未扣款', [STATUS.DONE]: '已扣款', [STATUS.SKIPPED]: '不適用' },
  income:    { [STATUS.PENDING]: '未轉入', [STATUS.DONE]: '已轉入', [STATUS.SKIPPED]: '不適用' },
};

/* ============================================
   6. badge class 對照
   ============================================ */
export const BADGE_CLASS = {
  [STATUS.PENDING]: 'badge-pending',
  [STATUS.DONE]:    'badge-success',
  [STATUS.SKIPPED]: 'badge-muted',
};

/* ============================================
   7. 對外 API
   ============================================ */

/**
 * 正規化任意輸入為標準代碼
 * - 若已是新代碼 → 直接回傳
 * - 若為舊名稱 → 依 LEGACY_MAP 轉換
 * - 若為空 / 未知 → 回傳 pending
 *
 * @param {string} raw
 * @returns {'pending'|'done'|'skipped'}
 */
export function normalize(raw) {
  if (raw == null || raw === '') return STATUS.PENDING;

  // 已是新代碼
  if (raw === STATUS.PENDING || raw === STATUS.DONE || raw === STATUS.SKIPPED) {
    return raw;
  }

  // 舊名稱映射
  return LEGACY_MAP[raw] || STATUS.PENDING;
}

/**
 * 是否為「完成」狀態（含 skipped）
 * @param {string} raw
 * @returns {boolean}
 */
export function isDone(raw) {
  const code = normalize(raw);
  return !!IS_DONE[code];
}

/**
 * 取得顯示文字
 * @param {string} raw - 狀態（新代碼或舊名稱）
 * @param {Object} [opts]
 * @param {'personal'|'fixed'|'insurance'|'income'} [opts.source] - 來源
 * @returns {string}
 */
export function label(raw, opts = {}) {
  const code = normalize(raw);
  const source = opts.source;

  if (source && LABELS.bySource[source] && LABELS.bySource[source][code]) {
    return LABELS.bySource[source][code];
  }
  return LABELS.default[code] || LABELS.default[STATUS.PENDING];
}

/**
 * 取得下拉選項
 * @param {Object} [opts]
 * @param {'personal'|'fixed'|'insurance'|'income'} [opts.source]
 * @param {boolean} [opts.includeSkipped=false] - 是否包含 skipped
 * @returns {Array<{value: string, label: string}>}
 */
export function options(opts = {}) {
  const codes = [STATUS.PENDING, STATUS.DONE];
  if (opts.includeSkipped) codes.push(STATUS.SKIPPED);

  return codes.map((code) => ({
    value: code,
    label: label(code, { source: opts.source }),
  }));
}

/**
 * 取得 badge class
 * @param {string} raw
 * @returns {string}
 */
export function badgeClass(raw) {
  const code = normalize(raw);
  return BADGE_CLASS[code] || BADGE_CLASS[STATUS.PENDING];
}

/**
 * 新代碼 → 舊名稱（依來源）
 * 供需要寫入舊格式的相容 API 使用
 *
 * @param {string} code
 * @param {string} source
 * @returns {string}
 */
export function toLegacyName(code, source = 'personal') {
  const normalized = normalize(code);
  const map = LABELS_BY_SOURCE[source] || LABELS_BY_SOURCE.personal;
  return map[normalized] || map[STATUS.PENDING];
}

/**
 * 便利判斷：是否為待處理
 */
export function isPending(raw) {
  return normalize(raw) === STATUS.PENDING;
}

/**
 * 便利判斷：是否為已跳過
 */
export function isSkipped(raw) {
  return normalize(raw) === STATUS.SKIPPED;
}
```

---

## 📄 js/core/api.js

```
// ============================================
// api.js — Cloudflare Functions 呼叫封裝（v103.0.23）
// 位置：js/core/api.js
// ============================================
// v103.0.23（B34-4）：
//   ✅ 新增 api.entityCreate（統一 CRUD 端點，D-063）
//   ✅ 加 entityCreate 封裝
// v103.0.22（B25-6）：
//   ✅ 加 adminMigrate 封裝
// ============================================

import { AppState } from './state.js';
import { auth } from '../config/firebase-config.js';
import { STORAGE_KEYS, ROUTES } from '../config/constants.js';

export async function callApi(path, options = {}) {
  const user = auth.currentUser;
  const token = user ? await user.getIdToken() : '';

  const headers = {
    ...(options.headers || {}),
    'Authorization': `Bearer ${token}`,
  };

  let res;
  try {
    res = await fetch(path, { ...options, headers });
  } catch (err) {
    throw new Error(`網路錯誤（${path}）：${err.message}`);
  }

  if (res.status === 401) {
    localStorage.removeItem(STORAGE_KEYS.FAMILY_ID);
    localStorage.removeItem(STORAGE_KEYS.FAMILY_NAME);
    window.location.href = ROUTES.LOGIN;
    throw new Error('登入已過期，請重新登入');
  }

  if (res.status === 403) {
    const text = await res.text().catch(() => '');
    let parsed = null;
    try { parsed = JSON.parse(text); } catch (e) { /* noop */ }
    const msg = parsed?.message || text || '無權限存取';
    throw new Error(msg);
  }

  if (!res.ok) {
    const text = await res.text().catch(() => '');
    let parsed = null;
    try { parsed = JSON.parse(text); } catch (e) { /* noop */ }
    const msg = parsed?.message || parsed?.error || text || '未知錯誤';
    const err = new Error(msg);
    err.code = parsed?.error || 'UNKNOWN';
    err.status = res.status;
    throw err;
  }

  return res.json();
}

function getFamilyId() {
  const id = AppState.getFamilyId();
  if (!id) throw new Error('尚未選擇家庭');
  return id;
}

export const api = {
  lookupFamily: () =>
    callApi('/api/lookup-family', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({}),
    }),

  summary: (year, month) =>
    callApi(`/api/summary?familyId=${getFamilyId()}&year=${year}&month=${month}`),

  fetchAnnualSummary: async (year) => {
    const familyId = getFamilyId();
    const result = await callApi(
      `/api/annual-summary?familyId=${familyId}&startYear=${year}&endYear=${year}`
    );

    const yearData = result.years?.[0];
    if (!yearData) {
      return {
        year, monthly: [],
        totalIncome: 0, totalExpense: 0, netBalance: 0,
        yearlyInsuranceTotal: 0, totalAssets: 0, bankBalance: 0, fundValue: 0,
      };
    }

    const monthly = yearData.monthly || [];

    return {
      year,
      monthly,
      totalIncome: yearData.totalIncome || 0,
      totalExpense: yearData.totalExpense || 0,
      netBalance: yearData.netBalance || 0,
      yearlyInsuranceTotal: yearData.yearlyInsuranceTotal || 0,
      totalAssets: yearData.totalAssets || 0,
      bankBalance: yearData.bankBalance || 0,
      fundValue: yearData.fundValue || 0,
    };
  },

  fetchAnnualSummaryRange: async (startYear, endYear) => {
    const familyId = getFamilyId();
    return callApi(`/api/annual-summary?familyId=${familyId}&startYear=${startYear}&endYear=${endYear}`);
  },

  fetchSettlementsYear: async (year) => {
    const familyId = getFamilyId();
    return callApi(`/api/settlements-year?familyId=${familyId}&year=${year}`);
  },

  insuranceSync: (payload) =>
    callApi('/api/insurance-sync', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ ...payload, familyId: getFamilyId(), action: 'upsert' }),
    }),

  insuranceUnsync: (payload) =>
    callApi('/api/insurance-sync', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ ...payload, familyId: getFamilyId(), action: 'delete' }),
    }),

  /* 🆕 v103.0.23（B34-4）：統一實體 CRUD（D-063） */
  entityCreate: (payload) =>
    callApi('/api/entity-create', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ ...payload, familyId: getFamilyId() }),
    }),

  adminListFamilies: () =>
    callApi('/api/admin-families?action=list'),

  adminAddFamily: (uid, name, email) =>
    callApi('/api/admin-families', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ action: 'add', uid, name, email }),
    }),

  adminRemoveFamily: (uid) =>
    callApi('/api/admin-families', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ action: 'remove', uid }),
    }),

  adminInitFamily: (uid) =>
    callApi('/api/admin-init-family', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ uid }),
    }),

  /* 一次性遷移 */
  adminMigrate: (familyId) =>
    callApi('/api/admin-migrate', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ familyId, confirm: 'MIGRATE_ALL' }),
    }),

  familyAccounts: {
    list: (familyId) =>
      callApi(`/api/family-accounts?familyId=${familyId}&action=list`),

    create: (familyId, data) =>
      callApi('/api/family-accounts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action: 'create', familyId, ...data }),
      }),

    restore: (familyId, data) =>
      callApi('/api/family-accounts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action: 'restore', familyId, ...data }),
      }),

    update: (familyId, uid, data) =>
      callApi('/api/family-accounts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action: 'update', familyId, uid, ...data }),
      }),

    remove: (familyId, uid) =>
      callApi('/api/family-accounts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action: 'remove', familyId, uid }),
      }),
  },

  platformSettings: {
    get: () => callApi('/api/platform-settings'),

    update: (data) =>
      callApi('/api/platform-settings', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action: 'update', data }),
      }),
  },

  platformDefaults: {
    list: (resource) =>
      callApi(`/api/platform-defaults?action=list&resource=${resource}`),

    put: (resource, id, data) =>
      callApi('/api/platform-defaults', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action: 'put', resource, id, data }),
      }),

    remove: (resource, id) =>
      callApi('/api/platform-defaults', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action: 'delete', resource, id }),
      }),

    set: (resource, data) =>
      callApi('/api/platform-defaults', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action: 'set', resource, data }),
      }),
  },

  familySettings: {
    get: () =>
      callApi(`/api/family-settings?familyId=${getFamilyId()}`),

    update: (data) =>
      callApi('/api/family-settings', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), action: 'update', data }),
      }),

    putStatus: (id, data) =>
      callApi('/api/family-settings', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), action: 'put-status', id, data }),
      }),

    removeStatus: (id) =>
      callApi('/api/family-settings', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), action: 'delete-status', id }),
      }),
  },

  bankAccounts: {
    list: () =>
      callApi(`/api/bank-accounts?familyId=${getFamilyId()}&action=list`),

    create: (data) =>
      callApi('/api/bank-accounts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), action: 'create', data }),
      }),

    update: (id, data) =>
      callApi('/api/bank-accounts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), action: 'update', id, data }),
      }),

    remove: (id) =>
      callApi('/api/bank-accounts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), action: 'remove', id }),
      }),
  },

  bankTransactions: {
    list: (bankId) =>
      callApi(`/api/bank-transactions?familyId=${getFamilyId()}&bankId=${bankId}&action=list`),

    listAll: () =>
      callApi(`/api/bank-transactions?familyId=${getFamilyId()}&action=listAll`),

    create: (bankId, data) =>
      callApi('/api/bank-transactions', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), bankId, action: 'create', data }),
      }),

    update: (bankId, txnId, data) =>
      callApi('/api/bank-transactions', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), bankId, txnId, action: 'update', data }),
      }),

    remove: (bankId, txnId) =>
      callApi('/api/bank-transactions', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), bankId, txnId, action: 'remove' }),
      }),
  },

  personalIncome: {
    list: (memberId) =>
      callApi(`/api/personal-income?familyId=${getFamilyId()}&memberId=${memberId}&action=list`),

    save: (memberId, year, month, amount) =>
      callApi('/api/personal-income', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), memberId, year, month, amount, action: 'save' }),
      }),

    remove: (memberId, year, month) =>
      callApi('/api/personal-income', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), memberId, year, month, action: 'remove' }),
      }),
  },

  memberAdvances: {
    list: (memberId) =>
      callApi(`/api/member-advances?familyId=${getFamilyId()}&memberId=${memberId}&action=list`),

    listAll: () =>
      callApi(`/api/member-advances?familyId=${getFamilyId()}&action=listAll`),

    create: (memberId, data) =>
      callApi('/api/member-advances', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), memberId, action: 'create', data }),
      }),

    update: (memberId, advanceId, data) =>
      callApi('/api/member-advances', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), memberId, advanceId, action: 'update', data }),
      }),

    remove: (memberId, advanceId) =>
      callApi('/api/member-advances', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ familyId: getFamilyId(), memberId, advanceId, action: 'remove' }),
      }),
  },

  clearBankBalances: () =>
    callApi('/api/clear-bank-balances', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        familyId: getFamilyId(),
        confirm: 'CONFIRM_DELETE',
      }),
    }),
};

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/core/api.js
   Version: 1791900880
   Batch: B34-4
   Writer: A20261013-004#082
   ═══════════════════════════════════════════ */```

---

## 📄 js/core/auth-guard.js

```
// ============================================
// auth-guard.js — 路由守衛（v103.0.12）
// 位置：js/core/auth-guard.js
// ============================================
// v103.0.12（B31-1）：
//   ✅ 修正：fallback 不再用 user.uid 當 familyId
//      - 保留 AppState 既有 familyId（不污染 localStorage）
// v103.0.11：
//   ✅ [H10] fallback 改為「唯讀模式」（canInput = false）
// ============================================

import { watchAuth, isSuperAdmin } from './auth.js';
import { AppState } from './state.js';
import { api } from './api.js';
import { SESSION_KEYS, ROUTES, ROLES } from '../config/constants.js';

const AUTH_CONTEXT_CACHE_PREFIX = SESSION_KEYS.AUTH_CONTEXT_PREFIX;
const AUTH_CONTEXT_TTL_MS = 5 * 60 * 1000;

/* ============================================
   主入口
   ============================================ */
export function requireLogin({ requireFamily = true } = {}) {
  return new Promise((resolve) => {
    let resolved = false;

    const unsubscribe = watchAuth(async (user) => {
      if (resolved) return;
      resolved = true;

      try { unsubscribe(); } catch (e) { /* noop */ }

      /* ---------- 1. 未登入 ---------- */
      if (!user) {
        _clearAllAuthContexts();
        window.location.href = ROUTES.LOGIN;
        resolve(null);
        return;
      }

      AppState.setUser(user);
      const superAdmin = isSuperAdmin(user);
      AppState.setSuperAdmin(superAdmin);

      /* ---------- 2. superadmin ---------- */
      if (superAdmin) {
        AppState.setRole(ROLES.SUPERADMIN);
        AppState.setCanInput(true);
        AppState.setDisplayName('超級管理員');

        if (requireFamily && !AppState.getFamilyId()) {
          window.location.href = ROUTES.ADMIN;
          resolve(null);
          return;
        }

        resolve(user);
        return;
      }

      /* ---------- 3. 讀快取 ---------- */
      const cached = _readAuthContextCache(user.uid);
      if (cached) {
        _applyAuthContext(cached);
        resolve(user);
        return;
      }

      /* ---------- 4. 呼叫 lookupFamily ---------- */
      try {
        const result = await api.lookupFamily();

        if (result && result.familyId) {
          const ctx = {
            familyId: result.familyId,
            familyName: result.familyName || '我的家庭',
            memberAccount: result.memberAccount,
            ownerUid: result.isLegacy ? result.familyId : undefined,
          };

          _writeAuthContextCache(user.uid, ctx);
          _applyAuthContext(ctx);
          resolve(user);
          return;
        }
      } catch (err) {
        const status = err?.status;
        const code = err?.code;

        // 4a. 401
        if (status === 401 || code === 'UNAUTHORIZED') {
          resolve(null);
          return;
        }

        // 4b. 明確拒絕
        if (
          status === 403 || code === 'FORBIDDEN' ||
          status === 404 || code === 'NOT_FOUND'
        ) {
          console.warn('[auth-guard] lookupFamily 明確拒絕：', code, err.message);
          alert(
            '無法查詢您的家庭資訊。\n\n' +
            '若您認為這是錯誤，請聯繫平台管理員。'
          );
          window.location.href = ROUTES.LOGIN;
          resolve(null);
          return;
        }

        // 4c. 網路錯誤 / 5xx → fallback（唯讀）
        console.warn('[auth-guard] lookupFamily 網路失敗，將使用 fallback（唯讀模式）：', err.message);
      }

      /* ---------- 5. fallback（唯讀） ---------- */
      _applyFallbackContext(user);
      _notifyFallback();
      resolve(user);
    });
  });
}

/* ============================================
   內部：套用 Auth Context
   ============================================ */
function _applyAuthContext(ctx) {
  AppState.setAccountContext({
    familyId: ctx.familyId,
    familyName: ctx.familyName,
    memberAccount: ctx.memberAccount,
    ownerUid: ctx.ownerUid,
  });
}

/**
 * v103.0.12（B31-1）：
 * - fallback 不再呼叫 AppState.setFamily(user.uid, ...)
 *   因為新式帳號 uid ≠ familyId，會污染 localStorage 與查詢路徑
 * - 保留既有 familyId（AppState.init 已從 localStorage 載入）
 * - 只設定角色 / 權限 / 顯示名稱
 */
function _applyFallbackContext(user) {
  AppState.setRole(ROLES.MEMBER);
  AppState.setCanInput(false);
  AppState.setDisplayName(user.email?.split('@')[0] || '成員');
  AppState.setMemberAccount({
    email: user.email || '',
    displayName: AppState.getDisplayName(),
    role: ROLES.MEMBER,
    canInput: false,
  });
}

/**
 * v103.0.11 [H10]：提示使用者
 */
async function _notifyFallback() {
  try {
    const mod = await import('../ui/toast.js');
    if (mod && typeof mod.showToast === 'function') {
      mod.showToast('⚠️ 網路不穩，已進入唯讀模式', 'warning', 5000);
    }
  } catch (e) { /* noop */ }
}

/* ============================================
   內部：sessionStorage 快取
   ============================================ */
function _readAuthContextCache(uid) {
  if (!uid) return null;
  try {
    const raw = sessionStorage.getItem(`${AUTH_CONTEXT_CACHE_PREFIX}${uid}`);
    if (!raw) return null;

    const data = JSON.parse(raw);
    if (!data || !data.cachedAt) return null;

    if (Date.now() - data.cachedAt > AUTH_CONTEXT_TTL_MS) {
      sessionStorage.removeItem(`${AUTH_CONTEXT_CACHE_PREFIX}${uid}`);
      return null;
    }

    return data;
  } catch (e) {
    return null;
  }
}

function _writeAuthContextCache(uid, ctx) {
  if (!uid) return;
  try {
    sessionStorage.setItem(
      `${AUTH_CONTEXT_CACHE_PREFIX}${uid}`,
      JSON.stringify({ ...ctx, cachedAt: Date.now() })
    );
  } catch (e) { /* noop */ }
}

function _clearAllAuthContexts() {
  try {
    const keys = Object.keys(sessionStorage);
    keys.forEach((k) => {
      if (k.startsWith(AUTH_CONTEXT_CACHE_PREFIX)) {
        sessionStorage.removeItem(k);
      }
    });
  } catch (e) { /* noop */ }
}

export function clearAuthContextCache(uid) {
  if (!uid) return;
  try {
    sessionStorage.removeItem(`${AUTH_CONTEXT_CACHE_PREFIX}${uid}`);
  } catch (e) { /* noop */ }
}

export function clearAllAuthContexts() {
  _clearAllAuthContexts();
}

/* ============================================
   向後相容 API
   ============================================ */

export function isLoggedIn() {
  return !!AppState.currentUser;
}

export function checkSuperAdmin() {
  return !!AppState.isSuperAdmin;
}

export function canInput() {
  return !!AppState.canInput;
}

export function isFamilyOwner() {
  return AppState.role === ROLES.OWNER;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/core/auth-guard.js
   Version: v103.0.12
   Batch: B31-1
   ═══════════════════════════════════════════ */
```

---

## 📄 js/core/auth.js

```
// ============================================
// auth.js — 自訂帳號登入 / 登出 封裝（v103.0.0）
// 位置：js/core/auth.js
// ============================================
// v103.0.0 重構：
//   ✅ 版本號更新（無功能變更）
//   ✅ 匯入 ROUTES 統一登入導向
//   ✅ 保留 v101 全部功能
// ============================================

import { auth } from '../config/firebase-config.js';
import {
  signInWithEmailAndPassword,
  signOut,
  onAuthStateChanged,
} from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
import {
  SUPERADMIN_DOMAIN,
  SUPERADMIN_EMAIL,
  STORAGE_KEYS,
  ROUTES,
} from '../config/constants.js';

/* ============================================
   帳號 <-> Email 轉換
   ============================================ */

/**
 * 帳號 → Email
 * @param {string} account - 帳號（可不含 @）
 * @returns {string}
 */
export function toEmail(account) {
  if (!account) return '';
  return account.includes('@') ? account : `${account}${SUPERADMIN_DOMAIN}`;
}

/**
 * Email → 帳號
 * @param {string} email
 * @returns {string}
 */
export function toAccount(email) {
  if (!email) return '';
  return email.replace(SUPERADMIN_DOMAIN, '');
}

/* ============================================
   登入 / 登出
   ============================================ */

/**
 * 使用自訂帳號登入
 * @param {string} account - 帳號
 * @param {string} password - 密碼
 * @returns {Promise<Object>} Firebase user
 */
export async function loginWithCustomAccount(account, password) {
  const email = toEmail(account);
  const cred = await signInWithEmailAndPassword(auth, email, password);
  return cred.user;
}

/**
 * 登出並清理 localStorage
 */
export async function logout() {
  try {
    await signOut(auth);
  } catch (err) {
    console.warn('[auth] signOut 失敗：', err);
  }

  localStorage.removeItem(STORAGE_KEYS.FAMILY_ID);
  localStorage.removeItem(STORAGE_KEYS.FAMILY_NAME);
  window.location.href = ROUTES.LOGIN;
}

/* ============================================
   監聽 / 查詢
   ============================================ */

/**
 * 監聽登入狀態
 * @param {Function} callback - (user) => void
 * @returns {Function} unsubscribe
 */
export function watchAuth(callback) {
  return onAuthStateChanged(auth, callback);
}

/**
 * 取得顯示名稱（帳號部分）
 * @param {Object} user - Firebase user
 * @returns {string}
 */
export function getDisplayName(user) {
  return user ? toAccount(user.email) : '';
}

/**
 * 判斷是否為超級管理員
 * @param {Object} user - Firebase user
 * @returns {boolean}
 */
export function isSuperAdmin(user) {
  return !!user && user.email === SUPERADMIN_EMAIL;
}

/* ============================================
   匯出常數（給其他模組使用）
   ============================================ */
export { SUPERADMIN_DOMAIN, SUPERADMIN_EMAIL };
```

---

## 📄 js/core/db.js

```
// db.js — Firebase RTDB 讀寫封裝（v103.0.37）
// v103.0.37（B35-1）：
//   ✅ getFundSnapshotListOnce 每筆附 _rawPath（實際儲存路徑）
//   ✅ removeFundSnapshot 支援 rawPath 參數（修舊格式刪除失效）
// v103.0.36（B33-1）：
//   ✅ 基金快照改為以「估值日（YYYY-MM-DD）」為 key
//   ✅ 新增 getFundSnapshotListOnce() 回傳統一列表
//   ✅ 保留舊資料兼容（舊 snapshots/{year}/{month} 讀取時自動轉換）
//   ✅ 移除 saveFundSnapshot 中的 contribution / cumulativeCost
// ============================================
import { db } from '../config/firebase-config.js';
import { AppState } from './state.js';
import { RESERVED_IDS, buildLinkedKey, PLATFORM_RESOURCES } from '../config/constants.js';
import { normalize as normalizeStatus } from '../config/status-registry.js';
import { ref, onValue, push, set, update, remove, get } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

const roundInt = (v) => Math.round(Number(v) || 0);
const byCreatedAt = (a, b) => (a.createdAt || 0) - (b.createdAt || 0);
const byOrder = (a, b) => (a.order || 0) - (b.order || 0);
const byOrderThenCreated = (a, b) => {
  const oa = a.order != null ? a.order : Number.MAX_SAFE_INTEGER;
  const ob = b.order != null ? b.order : Number.MAX_SAFE_INTEGER;
  if (oa !== ob) return oa - ob;
  return (a.createdAt || 0) - (b.createdAt || 0);
};

export function familyPath(subpath) {
  const familyId = AppState.getFamilyId();
  if (!familyId) throw new Error('尚未選擇家庭');
  return `families/${familyId}/${subpath}`;
}
export function familyRef(subpath) { return ref(db, familyPath(subpath)); }

function listen(subpath, callback, onError) {
  const familyId = AppState.getFamilyId();
  if (!familyId) { if (onError) onError(new Error('尚未選擇家庭')); return () => {}; }
  return onValue(ref(db, `families/${familyId}/${subpath}`), callback, (err) => {
    console.error(`❌ Firebase 讀取失敗 [${subpath}]：`, err);
    if (onError) onError(err);
  });
}
function listenList(subpath, sortFn, callback, onError) {
  return listen(subpath, (snap) => {
    const val = snap.val() || {};
    const list = Object.entries(val).map(([id, x]) => ({ id, ...x }));
    list.sort(sortFn);
    callback(list);
  }, onError);
}

/* ============ 成員 ============ */
export function listenMembers(cb, err) { return listenList('members', byOrderThenCreated, cb, err); }
export async function getMembersOnce() {
  const snap = await get(familyRef('members'));
  return Object.entries(snap.val() || {}).map(([id, m]) => ({ id, ...m }));
}
export async function addMember(m) {
  const r = push(familyRef('members'));
  await set(r, { name: m.name || '', role: m.role || 'other', order: m.order != null ? Number(m.order) : 0, createdAt: Date.now() });
  return r.key;
}
export async function updateMember(id, p) {
  const c = {};
  if (p.name !== undefined) c.name = String(p.name).trim();
  if (p.role !== undefined) c.role = p.role;
  if (p.order !== undefined) c.order = Number(p.order) || 0;
  if (!Object.keys(c).length) return;
  await update(familyRef(`members/${id}`), c);
}
export async function removeMember(id) { await remove(familyRef(`members/${id}`)); }
export async function deleteMemberAndData(memberId) {
  const fid = AppState.getFamilyId(); if (!fid) throw new Error('尚未選擇家庭');
  const snap = await get(ref(db, `families/${fid}/expenses`));
  const up = {};
  Object.entries(snap.val() || {}).forEach(([y, ms]) => {
    Object.entries(ms || {}).forEach(([m, md]) => {
      if (md.member_expenses && md.member_expenses[memberId]) up[`families/${fid}/expenses/${y}/${m}/member_expenses/${memberId}`] = null;
    });
  });
  up[`families/${fid}/members/${memberId}`] = null;
  up[`families/${fid}/personal_income/${memberId}`] = null;
  await update(ref(db), up);
}

/* ============ 銀行（舊） ============ */
export function listenBanks(cb, err) { return listenList('banks', byOrderThenCreated, cb, err); }
export async function getBanksOnce() {
  const snap = await get(familyRef('banks'));
  const list = Object.entries(snap.val() || {}).map(([id, b]) => ({ id, ...b }));
  list.sort(byOrderThenCreated);
  return list;
}
export async function addBank(x) {
  const name = typeof x === 'string' ? x : (x?.name || '');
  const r = push(familyRef('banks'));
  await set(r, { name: String(name).trim(), order: 0, createdAt: Date.now() });
  return r.key;
}
export async function updateBank(id, p) {
  const c = {};
  if (typeof p === 'string') c.name = String(p).trim();
  else if (p && typeof p === 'object') {
    if (p.name !== undefined) c.name = String(p.name).trim();
    if (p.order !== undefined) c.order = Number(p.order) || 0;
  }
  if (!Object.keys(c).length) return;
  await update(familyRef(`banks/${id}`), c);
}
export async function removeBank(id) { await remove(familyRef(`banks/${id}`)); }
export async function deleteBankAndBalances(bid) {
  const fid = AppState.getFamilyId(); if (!fid) throw new Error('尚未選擇家庭');
  const snap = await get(ref(db, `families/${fid}/bank_balances`));
  const up = {};
  Object.entries(snap.val() || {}).forEach(([y, ms]) => {
    Object.entries(ms || {}).forEach(([m, bs]) => {
      if (bs && bs[bid]) up[`families/${fid}/bank_balances/${y}/${m}/${bid}`] = null;
    });
  });
  up[`families/${fid}/banks/${bid}`] = null;
  await update(ref(db), up);
}

/* ============ 銀行帳號 ============ */
export function listenBankAccounts(cb, err) { return listenList('bank_accounts', byOrderThenCreated, cb, err); }
export async function getBankAccountsOnce() {
  const snap = await get(familyRef('bank_accounts'));
  return Object.entries(snap.val() || {}).map(([id, b]) => ({ id, ...b }));
}
export async function addBankAccount(d) {
  const r = push(familyRef('bank_accounts'));
  await set(r, {
    name: String(d.name || '').trim(),
    type: d.type || 'family',
    ownerType: d.ownerType || 'family',
    ownerId: d.ownerId || 'family',
    isSystemCreated: !!d.isSystemCreated,
    initialBalance: roundInt(d.initialBalance),
    initialYear: String(d.initialYear || new Date().getFullYear()),
    initialMonth: String(d.initialMonth || '01').padStart(2, '0'),
    order: Number(d.order) || 0,
    createdAt: Date.now(),
  });
  return r.key;
}
export async function updateBankAccount(id, p) {
  const c = {};
  ['name', 'type', 'ownerType', 'ownerId'].forEach((k) => { if (p[k] !== undefined) c[k] = p[k]; });
  if (p.name !== undefined) c.name = String(p.name).trim();
  if (p.isSystemCreated !== undefined) c.isSystemCreated = !!p.isSystemCreated;
  if (p.initialBalance !== undefined) c.initialBalance = roundInt(p.initialBalance);
  if (p.initialYear !== undefined) c.initialYear = String(p.initialYear);
  if (p.initialMonth !== undefined) c.initialMonth = String(p.initialMonth).padStart(2, '0');
  if (p.order !== undefined) c.order = Number(p.order) || 0;
  if (!Object.keys(c).length) return;
  await update(familyRef(`bank_accounts/${id}`), c);
}
export async function removeBankAccount(id) { await remove(familyRef(`bank_accounts/${id}`)); }

/* ============ 銀行交易 ============ */
export function listenBankTransactions(bid, cb, err) {
  if (!bid) return () => {};
  return listenList(`bank_accounts/${bid}/transactions`, byCreatedAt, cb, err);
}
export function listenAllBankTransactions(cb, err) {
  return listen('bank_accounts', (snap) => {
    const val = snap.val() || {};
    const flat = [];
    Object.entries(val).forEach(([bid, bd]) => {
      Object.entries(bd.transactions || {}).forEach(([tid, txn]) => {
        flat.push({ id: tid, bankId: bid, bankName: bd.name || '', ...txn });
      });
    });
    flat.sort(byCreatedAt);
    cb(flat);
  }, err);
}
export async function getBankTransactionsOnce(bid) {
  if (!bid) return [];
  const snap = await get(familyRef(`bank_accounts/${bid}/transactions`));
  return Object.entries(snap.val() || {}).map(([id, t]) => ({ id, ...t })).sort(byCreatedAt);
}
export async function addBankTransaction(bid, d) {
  if (!bid) throw new Error('缺少 bankId');
  const r = push(familyRef(`bank_accounts/${bid}/transactions`));
  await set(r, {
    type: d.type || 'manual', category: d.category || 'manual',
    amount: roundInt(d.amount),
    date: d.date || new Date().toISOString().slice(0, 10),
    memberId: d.memberId || '', refId: d.refId || '', note: d.note || '',
    createdAt: Date.now(),
  });
  return r.key;
}
export async function updateBankTransaction(bid, tid, p) {
  if (!bid || !tid) throw new Error('缺少參數');
  const c = {};
  ['type', 'category', 'amount', 'date', 'memberId', 'refId', 'note'].forEach((k) => { if (p[k] !== undefined) c[k] = p[k]; });
  if (c.amount != null) c.amount = roundInt(c.amount);
  if (!Object.keys(c).length) return;
  c.updatedAt = Date.now();
  await update(familyRef(`bank_accounts/${bid}/transactions/${tid}`), c);
}
export async function removeBankTransaction(bid, tid) {
  if (!bid || !tid) throw new Error('缺少參數');
  await remove(familyRef(`bank_accounts/${bid}/transactions/${tid}`));
}

/* ============ 個人收入 ============ */
export function listenPersonalIncome(mid, cb, err) {
  if (!mid) return () => {};
  return listen(`personal_income/${mid}`, (snap) => cb(snap.val() || {}), err);
}
export function listenAllPersonalIncome(cb, err) { return listen('personal_income', (s) => cb(s.val() || {}), err); }
export async function getPersonalIncomeOnce(mid) {
  if (!mid) return {};
  const snap = await get(familyRef(`personal_income/${mid}`));
  return snap.val() || {};
}
export async function savePersonalIncome(mid, y, m, amt) {
  if (!mid || !y || !m) throw new Error('缺少參數');
  const n = roundInt(amt);
  const path = `personal_income/${mid}/${y}`;
  if (n > 0) await update(familyRef(path), { [String(m).padStart(2, '0')]: n });
  else await remove(familyRef(`${path}/${String(m).padStart(2, '0')}`));
}
export async function removePersonalIncomeEntry(mid, y, m) {
  if (!mid || !y || !m) throw new Error('缺少參數');
  await remove(familyRef(`personal_income/${mid}/${y}/${String(m).padStart(2, '0')}`));
}

/* ============ 成員代墊 ============ */
export function listenMemberAdvances(mid, cb, err) {
  if (!mid) return () => {};
  return listenList(`member_advances/${mid}`, byCreatedAt, cb, err);
}
export function listenAllMemberAdvances(cb, err) {
  return listen('member_advances', (snap) => {
    const val = snap.val() || {};
    const flat = [];
    Object.entries(val).forEach(([mid, ad]) => {
      Object.entries(ad || {}).forEach(([aid, adv]) => flat.push({ id: aid, memberId: mid, ...adv }));
    });
    flat.sort(byCreatedAt);
    cb(flat);
  }, err);
}
export async function getMemberAdvancesOnce(mid) {
  if (!mid) return [];
  const snap = await get(familyRef(`member_advances/${mid}`));
  return Object.entries(snap.val() || {}).map(([id, a]) => ({ id, ...a })).sort(byCreatedAt);
}
export async function addMemberAdvance(mid, d) {
  if (!mid) throw new Error('缺少 memberId');
  const r = push(familyRef(`member_advances/${mid}`));
  await set(r, {
    policyId: d.policyId || '',
    totalAmount: roundInt(d.totalAmount),
    remainingAmount: roundInt(d.totalAmount),
    startYear: String(d.startYear || ''),
    startMonth: String(d.startMonth || '').padStart(2, '0'),
    note: d.note || '',
    createdAt: Date.now(),
  });
  return r.key;
}
export async function updateMemberAdvance(mid, aid, p) {
  if (!mid || !aid) throw new Error('缺少參數');
  const c = {};
  ['policyId', 'totalAmount', 'remainingAmount', 'startYear', 'startMonth', 'note'].forEach((k) => { if (p[k] !== undefined) c[k] = p[k]; });
  if (c.totalAmount != null) c.totalAmount = roundInt(c.totalAmount);
  if (c.remainingAmount != null) c.remainingAmount = roundInt(c.remainingAmount);
  if (!Object.keys(c).length) return;
  c.updatedAt = Date.now();
  await update(familyRef(`member_advances/${mid}/${aid}`), c);
}
export async function removeMemberAdvance(mid, aid) {
  if (!mid || !aid) throw new Error('缺少參數');
  await remove(familyRef(`member_advances/${mid}/${aid}`));
}

/* ============ 銀行結餘（舊） ============ */
export function listenBankBalances(y, m, cb, err) {
  if (!y || !m) { const ym = AppState.getYearMonth(); y = ym.year; m = ym.month; }
  return listen(`bank_balances/${y}/${m}`, (s) => cb(s.val() || {}), err);
}
export async function getBankBalancesOnce(y, m) {
  const snap = await get(familyRef(`bank_balances/${y}/${m}`));
  return snap.val() || {};
}
export async function saveBankBalance(y, m, bid, amt) {
  if (!y || !m) { const ym = AppState.getYearMonth(); y = ym.year; m = ym.month; }
  await update(familyRef(`bank_balances/${y}/${m}`), { [bid]: { amount: roundInt(amt), updatedAt: Date.now() } });
}
export async function getPrevMonthBankTotal(y, m) {
  const yy = Number(y), mm = Number(m);
  let py = yy, pm = mm - 1;
  if (pm < 1) { py = yy - 1; pm = 12; }
  const snap = await get(familyRef(`bank_balances/${py}/${String(pm).padStart(2, '0')}`));
  return Object.values(snap.val() || {}).reduce((s, b) => s + roundInt(b.amount), 0);
}
export async function clearAllBankBalances() {
  const fid = AppState.getFamilyId(); if (!fid) throw new Error('尚未選擇家庭');
  await remove(ref(db, `families/${fid}/bank_balances`));
}

/* ============ 保險公司 ============ */
export function listenInsuranceCompanies(cb, err) { return listenList('insurance_companies', byCreatedAt, cb, err); }
export async function getInsuranceCompaniesOnce() {
  const snap = await get(familyRef('insurance_companies'));
  const list = Object.entries(snap.val() || {}).map(([id, c]) => ({ id, ...c }));
  list.sort(byCreatedAt);
  return list;
}
export async function addInsuranceCompany(name) {
  const r = push(familyRef('insurance_companies'));
  await set(r, { name: name || '', createdAt: Date.now() });
  return r.key;
}
export async function updateInsuranceCompany(id, nn) { await update(familyRef(`insurance_companies/${id}`), { name: nn }); }
export async function removeInsuranceCompany(id) { await remove(familyRef(`insurance_companies/${id}`)); }
export async function updatePolicyCompanyName(on, nn) {
  const fid = AppState.getFamilyId(); if (!fid) throw new Error('尚未選擇家庭');
  const snap = await get(ref(db, `families/${fid}/insurance_policies`));
  const ps = snap.val() || {}; const up = {};
  Object.entries(ps).forEach(([id, p]) => { if (p.company === on) up[`families/${fid}/insurance_policies/${id}/company`] = nn; });
  if (Object.keys(up).length) await update(ref(db), up);
  return Object.keys(up).length;
}

/* ============ 支付方式 ============ */
export function listenPaymentMethods(cb, err) { return listenList('payment_methods', byOrder, cb, err); }
export async function addPaymentMethod(pm) {
  const r = push(familyRef('payment_methods'));
  await set(r, { name: pm.name || '', order: Number(pm.order) || 0, createdAt: Date.now() });
  return r.key;
}
export async function updatePaymentMethod(id, p) {
  const c = {};
  if (p.name !== undefined) c.name = String(p.name).trim();
  if (p.order !== undefined) c.order = Number(p.order) || 0;
  if (!Object.keys(c).length) return;
  await update(familyRef(`payment_methods/${id}`), c);
}
export async function removePaymentMethod(id) { await remove(familyRef(`payment_methods/${id}`)); }
export async function getPaymentMethodsOnce() {
  const snap = await get(familyRef('payment_methods'));
  const list = Object.entries(snap.val() || {}).map(([id, p]) => ({ id, ...p }));
  list.sort(byOrder);
  return list;
}

/* ============ 支出類別 / 項目 ============ */
export function listenCategories(cb, err) { return listenList('expense_categories', byOrder, cb, err); }
export async function getCategoriesOnce() {
  const snap = await get(familyRef('expense_categories'));
  const list = Object.entries(snap.val() || {}).map(([id, c]) => ({ id, ...c }));
  list.sort(byOrder);
  return list;
}
export async function addCategory(c) {
  const r = push(familyRef('expense_categories'));
  await set(r, { name: c.name || '', order: Number(c.order) || 0, createdAt: Date.now() });
  return r.key;
}
export async function updateCategory(id, p) {
  const cl = {};
  if (p.name !== undefined) cl.name = String(p.name).trim();
  if (p.order !== undefined) cl.order = Number(p.order) || 0;
  if (!Object.keys(cl).length) return;
  await update(familyRef(`expense_categories/${id}`), cl);
}
export async function removeCategory(id) { await remove(familyRef(`expense_categories/${id}`)); }

export function listenItems(cb, err) { return listenList('expense_items', byCreatedAt, cb, err); }
export async function getItemsOnce() {
  const snap = await get(familyRef('expense_items'));
  return Object.entries(snap.val() || {}).map(([id, i]) => ({ id, ...i }));
}
export async function addItem(it) {
  const r = push(familyRef('expense_items'));
  await set(r, { name: it.name || '', categoryId: it.categoryId || '', createdAt: Date.now() });
  return r.key;
}
export async function updateItem(id, p) {
  const c = {};
  if (p.name !== undefined) c.name = String(p.name).trim();
  if (p.categoryId !== undefined) c.categoryId = p.categoryId;
  if (!Object.keys(c).length) return;
  await update(familyRef(`expense_items/${id}`), c);
}
export async function removeItem(id) { await remove(familyRef(`expense_items/${id}`)); }

/* ============ 狀態 ============ */
export function listenStatuses(cb, err) { return listenList('statuses', byOrder, cb, err); }
export async function getStatusesOnce() {
  const snap = await get(familyRef('statuses'));
  const list = Object.entries(snap.val() || {}).map(([id, s]) => ({ id, ...s }));
  list.sort(byOrder);
  return list;
}
export async function addStatus(d) {
  const r = push(familyRef('statuses'));
  await set(r, { name: d.name || '', category: d.category || 'personal', isDone: !!d.isDone, order: Number(d.order) || 0, createdAt: Date.now() });
  return r.key;
}
export async function updateStatus(id, p) {
  const c = {};
  if (p.name !== undefined) c.name = String(p.name).trim();
  if (p.category !== undefined) c.category = p.category;
  if (p.isDone !== undefined) c.isDone = !!p.isDone;
  if (p.order !== undefined) c.order = Number(p.order) || 0;
  if (!Object.keys(c).length) return;
  await update(familyRef(`statuses/${id}`), c);
}
export async function removeStatus(id) { await remove(familyRef(`statuses/${id}`)); }

/* ============ 家庭設定 ============ */
export function listenFamilyOptions(cb, err) { return listen('settings/options', (s) => cb(s.val() || {}), err); }
export async function saveFamilyOptions(o) { await set(familyRef('settings/options'), o || {}); }
export function listenYearRange(cb, err) { return listen('settings/year_range', (s) => cb(s.val() || {}), err); }
export async function saveYearRange(d) {
  await set(familyRef('settings/year_range'), {
    startYear: d.startYear != null ? Number(d.startYear) : null,
    futureYears: Number(d.futureYears) || 5,
  });
}
export function listenUIConstants(cb, err) { return listen('settings/ui_constants', (s) => cb(s.val() || {}), err); }
export async function saveUIConstants(d) { await set(familyRef('settings/ui_constants'), d || {}); }

/* ============ 頁面 metadata ============ */
export function listenPageMetadata(cb, err) {
  return listen('settings/page_metadata', (s) => cb(s.val() || {}), err);
}
export async function savePageMetadata(data) {
  await set(familyRef('settings/page_metadata'), data || {});
}

/* ============ 平台預設 ============ */
function platformPath(resource) {
  const c = PLATFORM_RESOURCES[resource];
  if (!c) throw new Error(`未知的平台資源：${resource}`);
  return `platform/defaults/${c.path}`;
}
export function listenPlatformResource(resource, cb, err) {
  const c = PLATFORM_RESOURCES[resource];
  if (!c) { console.warn(`[db] 未知平台資源：${resource}`); return () => {}; }
  return onValue(ref(db, platformPath(resource)), (snap) => {
    const val = snap.val();
    if (c.type === 'list' && val && typeof val === 'object' && !Array.isArray(val)) {
      const list = Object.entries(val).map(([id, x]) => ({ id, ...x }));
      list.sort(byOrderThenCreated);
      cb(list);
    } else cb(val || null);
  }, (e) => { console.error(`❌ 平台資源失敗 [${resource}]：`, e); if (err) err(e); });
}
export async function getPlatformResourceOnce(r) { const s = await get(ref(db, platformPath(r))); return s.val(); }
export async function putPlatformResource(r, id, d) {
  if (id) { await set(ref(db, `${platformPath(r)}/${id}`), d); return id; }
  const nr = push(ref(db, platformPath(r)));
  await set(nr, d); return nr.key;
}
export async function removePlatformResource(r, id) { await remove(ref(db, `${platformPath(r)}/${id}`)); }
export async function setPlatformResource(r, d) { await set(ref(db, platformPath(r)), d || {}); }

/* ============ 成員支出 ============ */
function expensePath(y, m, mid) { return `expenses/${y}/${m}/member_expenses/${mid}`; }
export function listenExpenses(y, m, mid, cb, err) {
  if (!y || !m) { const ym = AppState.getYearMonth(); y = ym.year; m = ym.month; }
  return listen(expensePath(y, m, mid), (snap) => {
    const list = Object.entries(snap.val() || {}).map(([id, e]) => ({ id, ...e }));
    list.sort(byCreatedAt);
    cb(list);
  }, err);
}
export function listenAllExpenses(cb, err) {
  return listen('expenses', (snap) => {
    const flat = [];
    Object.entries(snap.val() || {}).forEach(([y, ms]) => {
      Object.entries(ms || {}).forEach(([m, md]) => {
        Object.entries(md.member_expenses || {}).forEach(([mid, items]) => {
          Object.entries(items || {}).forEach(([id, exp]) => flat.push({ id, memberId: mid, year: y, month: m, ...exp }));
        });
      });
    });
    cb(flat);
  }, err);
}
export function listenAllMemberExpenses(y, m, cb, err) {
  if (!y || !m) { const ym = AppState.getYearMonth(); y = ym.year; m = ym.month; }
  return listen(`expenses/${y}/${m}/member_expenses`, (snap) => {
    const flat = [];
    Object.entries(snap.val() || {}).forEach(([mid, items]) => {
      Object.entries(items || {}).forEach(([id, exp]) => flat.push({ id, memberId: mid, ...exp }));
    });
    flat.sort(byCreatedAt);
    cb(flat);
  }, err);
}
export async function getAllMemberExpensesOnce(y, m) {
  const snap = await get(familyRef(`expenses/${y}/${m}/member_expenses`));
  const flat = [];
  Object.entries(snap.val() || {}).forEach(([mid, items]) => {
    Object.entries(items || {}).forEach(([id, exp]) => flat.push({ id, memberId: mid, ...exp }));
  });
  flat.sort(byCreatedAt);
  return flat;
}
export async function getMemberExpensesForYear(y, mid) {
  const fid = AppState.getFamilyId(); if (!fid) throw new Error('尚未選擇家庭');
  if (!y || !mid) return {};
  try {
    const snap = await get(ref(db, `families/${fid}/expenses/${y}`));
    const yd = snap.val() || {}; const out = {};
    for (let m = 1; m <= 12; m++) {
      const mm = String(m).padStart(2, '0');
      const me = yd[mm]?.member_expenses?.[mid] || {};
      const items = Object.entries(me).map(([id, e]) => ({ id, ...e }));
      items.sort(byCreatedAt);
      out[mm] = items;
    }
    return out;
  } catch (e) { return {}; }
}
export async function addExpense(y, m, mid, exp) {
  if (!y || !m) { const ym = AppState.getYearMonth(); y = ym.year; m = ym.month; }
  const r = push(familyRef(expensePath(y, m, mid)));
  await set(r, {
    name: exp.name || '', amount: roundInt(exp.amount),
    status: normalizeStatus(exp.status || '未處理'),
    date: exp.date || '',
    categoryId: exp.categoryId || '', itemId: exp.itemId || '',
    paymentMethodId: exp.paymentMethodId || '',
    bankId: exp.bankId || '', txnId: exp.txnId || '',
    isAutoLinked: exp.isAutoLinked || false, policyId: exp.policyId || '',
    createdAt: Date.now(),
  });
  return r.key;
}
export async function updateExpense(y, m, mid, eid, p) {
  if (!y || !m) { const ym = AppState.getYearMonth(); y = ym.year; m = ym.month; }
  const c = { ...p };
  if (c.amount != null) c.amount = roundInt(c.amount);
  if (c.status !== undefined) c.status = normalizeStatus(c.status);
  await update(familyRef(`${expensePath(y, m, mid)}/${eid}`), c);
}
export async function removeExpense(y, m, mid, eid) {
  if (!y || !m) { const ym = AppState.getYearMonth(); y = ym.year; m = ym.month; }
  await remove(familyRef(`${expensePath(y, m, mid)}/${eid}`));
}
export async function markMemberExpenseRepaid(y, m, mid, eid, sn) {
  await update(familyRef(`${expensePath(y, m, mid)}/${eid}`), {
    status: normalizeStatus(sn),
    repaidDate: sn && sn.startsWith('已') ? new Date().toISOString().slice(0, 10) : '',
  });
}
export async function batchUpdateExpenses(updates) {
  const fid = AppState.getFamilyId(); if (!fid) throw new Error('尚未選擇家庭');
  const fu = {};
  for (const item of updates) {
    const { oldYear, oldMonth, oldMemberId, expenseId, data } = item;
    const ny = data.year || oldYear, nm = data.month || oldMonth, nmid = data.memberId || oldMemberId;
    const ns = normalizeStatus(data.status || '未處理');
    const pc = ny !== oldYear || nm !== oldMonth || nmid !== oldMemberId;
    if (pc) {
      fu[`families/${fid}/expenses/${oldYear}/${oldMonth}/member_expenses/${oldMemberId}/${expenseId}`] = null;
      const nr = push(ref(db, `families/${fid}/expenses/${ny}/${nm}/member_expenses/${nmid}`));
      fu[`families/${fid}/expenses/${ny}/${nm}/member_expenses/${nmid}/${nr.key}`] = {
        name: data.name || '', amount: roundInt(data.amount), status: ns,
        date: data.date || '', categoryId: data.categoryId || '', itemId: data.itemId || '',
        paymentMethodId: data.paymentMethodId || '', bankId: data.bankId || '', txnId: data.txnId || '',
        isAutoLinked: false, createdAt: Date.now(),
      };
    } else {
      fu[`families/${fid}/expenses/${oldYear}/${oldMonth}/member_expenses/${oldMemberId}/${expenseId}`] = {
        name: data.name || '', amount: roundInt(data.amount), status: ns,
        date: data.date || '', categoryId: data.categoryId || '', itemId: data.itemId || '',
        paymentMethodId: data.paymentMethodId || '', bankId: data.bankId || '', txnId: data.txnId || '',
      };
    }
  }
  await update(ref(db), fu);
}

/* ============ 保險 ============ */
export function listenInsurancePolicies(cb, err) { return listenList('insurance_policies', byCreatedAt, cb, err); }
export async function getInsurancePoliciesOnce() {
  const snap = await get(familyRef('insurance_policies'));
  const list = Object.entries(snap.val() || {}).map(([id, p]) => ({ id, ...p }));
  list.sort(byCreatedAt);
  return list;
}
export async function addInsurancePolicy(p) {
  const r = push(familyRef('insurance_policies'));
  await set(r, {
    type: p.type || 'normal',
    memberId: p.memberId || '', policyHolderId: p.policyHolderId || '',
    name: p.name || '', company: p.company || '',
    paymentType: p.paymentType || '年繳',
    paymentMode: p.paymentMode || 'direct',
    advanceHolderId: p.advanceHolderId || '', advanceId: p.advanceId || '',
    firstStartYear: Number(p.firstStartYear) || 0,
    firstStartMonth: String(p.firstStartMonth || '01').padStart(2, '0'),
    totalPolicyYears: Number(p.totalPolicyYears) || 0,
    totalPolicyPeriods: Number(p.totalPolicyPeriods) || 0,
    totalPremium: roundInt(p.totalPremium),
    currentPeriodIndex: Number(p.currentPeriodIndex) || 1,
    monthlyPremium: roundInt(p.monthlyPremium),
    annualPremium: roundInt(p.annualPremium),
    account: p.account || '',
    fundsAllocation: Array.isArray(p.fundsAllocation) ? p.fundsAllocation : [],
    periods: p.periods || {},
    createdAt: Date.now(),
  });
  return r.key;
}
export async function updateInsurancePolicy(id, p) {
  const c = {};
  const f = ['type', 'memberId', 'policyHolderId', 'name', 'company', 'paymentType', 'paymentMode', 'advanceHolderId', 'advanceId', 'firstStartYear', 'firstStartMonth', 'totalPolicyYears', 'totalPolicyPeriods', 'totalPremium', 'currentPeriodIndex', 'monthlyPremium', 'annualPremium', 'account', 'periods', 'isCompleted', 'fundsAllocation'];
  f.forEach((k) => { if (p[k] !== undefined) c[k] = p[k]; });
  if (c.totalPremium != null) c.totalPremium = roundInt(c.totalPremium);
  if (c.monthlyPremium != null) c.monthlyPremium = roundInt(c.monthlyPremium);
  if (c.annualPremium != null) c.annualPremium = roundInt(c.annualPremium);
  if (!Object.keys(c).length) return;
  await update(familyRef(`insurance_policies/${id}`), c);
}
export async function removeInsurancePolicy(id) { await remove(familyRef(`insurance_policies/${id}`)); }
export async function deleteInsurancePolicyAndData(pid, mid) {
  const fid = AppState.getFamilyId(); if (!fid) throw new Error('尚未選擇家庭');
  let em = mid;
  if (!em) {
    try {
      const ps = await get(ref(db, `families/${fid}/insurance_policies/${pid}`));
      const p = ps.val();
      if (p) em = p.policyHolderId || p.memberId || '';
    } catch (e) {}
  }
  const psnap = await get(ref(db, `families/${fid}/insurance_payments/${pid}`));
  const pms = psnap.val() || {}; const up = {};
  const lk = buildLinkedKey(pid);
  if (em) {
    for (const [y, ms] of Object.entries(pms)) {
      for (const [m] of Object.entries(ms)) {
        up[`families/${fid}/expenses/${y}/${m}/member_expenses/${em}/${lk}`] = null;
      }
    }
  }
  up[`families/${fid}/insurance_payments/${pid}`] = null;
  up[`families/${fid}/insurance_policies/${pid}`] = null;
  await update(ref(db), up);
}
export async function addInsurancePeriod(pid, pi, pd) {
  await set(familyRef(`insurance_policies/${pid}/periods/${pi}`), {
    periodIndex: Number(pi),
    startYear: Number(pd.startYear),
    startMonth: String(pd.startMonth).padStart(2, '0'),
    annualPremium: roundInt(pd.annualPremium),
    monthlyAverage: roundInt(pd.monthlyAverage),
  });
}
export function listenInsurancePayment(pid, y, m, cb, err) {
  return listen(`insurance_payments/${pid}/${y}/${m}`, (s) => cb(s.val() || {}), err);
}
export async function getInsurancePaymentsOnce(pid) {
  const snap = await get(familyRef(`insurance_payments/${pid}`));
  return snap.val() || {};
}

/**
 * @deprecated v103.0.35（B31-1）
 * 前端已改用 `api.insuranceSync()`（連動基金 / 代墊 / 銀行）。
 * 本函式保留僅供舊呼叫端相容；新程式碼請勿使用。
 */
export async function saveInsurancePaymentBatch(pid, y, m, d) {
  await update(familyRef(`insurance_payments/${pid}/${y}/${m}`), {
    status: normalizeStatus(d.status || '已扣款'),
    amount: roundInt(d.amount),
    date: d.date || new Date().toISOString().slice(0, 10),
    bankId: d.bankId || '', txnId: d.txnId || '',
    updatedAt: Date.now(),
  });
}
export async function removeInsurancePaymentBatch(pid, y, m) {
  await remove(familyRef(`insurance_payments/${pid}/${y}/${m}`));
}

/* ============ 收入（家用轉入） ============ */
export function listenIncome(y, m, cb, err) {
  if (!y || !m) { const ym = AppState.getYearMonth(); y = ym.year; m = ym.month; }
  return listen(`income/${y}/${m}`, (s) => cb(s.val() || {}), err);
}
export async function getIncomeOnce(y, m) {
  const snap = await get(familyRef(`income/${y}/${m}`));
  return snap.val() || {};
}
export async function saveIncome(y, m, data) {
  if (!y || !m) { const ym = AppState.getYearMonth(); y = ym.year; m = ym.month; }
  const up = {};
  Object.entries(data || {}).forEach(([k, v]) => {
    const n = roundInt(v);
    up[k] = n > 0 ? n : null;
  });
  if (!Object.keys(up).length) return;
  await update(familyRef(`income/${y}/${m}`), up);
}
export function listenAllIncome(cb, err) {
  return listen('income', (snap) => {
    const flat = [];
    Object.entries(snap.val() || {}).forEach(([y, ms]) => {
      Object.entries(ms || {}).forEach(([m, data]) => {
        Object.entries(data || {}).forEach(([mid, amt]) => flat.push({ year: y, month: m, memberId: mid, amount: roundInt(amt) }));
      });
    });
    cb(flat);
  }, err);
}
export async function updateIncomeEntry(y, m, mid, amt) {
  const n = roundInt(amt);
  if (n > 0) await update(familyRef(`income/${y}/${m}`), { [mid]: n });
  else await remove(familyRef(`income/${y}/${m}/${mid}`));
}
export async function removeIncomeEntry(y, m, mid) { await remove(familyRef(`income/${y}/${m}/${mid}`)); }

/* ============ 基金 ============ */
export function listenFunds(cb, err) { return listenList('funds', byCreatedAt, cb, err); }
export async function getFundsOnce() {
  const snap = await get(familyRef('funds'));
  return Object.entries(snap.val() || {}).map(([id, f]) => ({ id, ...f }));
}
export async function addFund(f) {
  const r = push(familyRef('funds'));
  await set(r, {
    name: f.name || '',
    type: f.type || 'standalone',
    policyId: f.policyId || '',
    cost: roundInt(f.cost),
    currentValue: roundInt(f.currentValue),
    units: Number(f.units) || 0,
    initialYear: f.initialYear ? String(f.initialYear) : '',
    initialMonth: f.initialMonth ? String(f.initialMonth).padStart(2, '0') : '',
    note: f.note || '',
    createdAt: Date.now(),
  });
  return r.key;
}
export async function updateFund(id, p) {
  const c = {};
  if (p.name !== undefined) c.name = String(p.name).trim();
  if (p.type !== undefined) c.type = p.type;
  if (p.policyId !== undefined) c.policyId = p.policyId;
  if (p.cost !== undefined) c.cost = roundInt(p.cost);
  if (p.currentValue !== undefined) c.currentValue = roundInt(p.currentValue);
  if (p.units !== undefined) c.units = Number(p.units) || 0;
  if (p.initialYear !== undefined) c.initialYear = String(p.initialYear);
  if (p.initialMonth !== undefined) c.initialMonth = String(p.initialMonth).padStart(2, '0');
  if (p.note !== undefined) c.note = String(p.note || '').trim();
  if (!Object.keys(c).length) return;
  await update(familyRef(`funds/${id}`), c);
}
export async function removeFund(id) { await remove(familyRef(`funds/${id}`)); }

/* ============ 基金快照（v103.0.37：附 _rawPath） ============ */

/** 訂閱某基金的所有快照（回傳 {dateKey: {shares, nav, ...}}） */
export function listenFundSnapshots(fundId, cb, err) {
  return listen(`funds/${fundId}/snapshots`, (snap) => cb(snap.val() || {}), err);
}

/**
 * 取得某基金的所有快照，回傳按日期排序的列表
 * 每筆附 `_rawPath`（實際儲存路徑），供刪除時精確定位
 *
 * 相容兩種格式：
 *   新：{ 'YYYY-MM-DD': {...} }        → _rawPath = 'YYYY-MM-DD'
 *   舊：{ 'YYYY': { 'MM': {...} } }   → _rawPath = 'YYYY/MM'
 *   舊：{ 'YYYY': { 'MM': { 'DD': {...} } } } → _rawPath = 'YYYY/MM/DD'
 */
export async function getFundSnapshotListOnce(fundId) {
  const snap = await get(familyRef(`funds/${fundId}/snapshots`));
  const val = snap.val() || {};
  const list = [];
  Object.entries(val).forEach(([key, v]) => {
    if (/^\d{4}-\d{2}-\d{2}$/.test(key)) {
      /* 新格式：YYYY-MM-DD */
      list.push({ dateKey: key, _rawPath: key, ...v });
    } else if (/^\d{4}$/.test(key) && v && typeof v === 'object') {
      /* 舊格式：{year: {month: {...}}} */
      Object.entries(v).forEach(([month, snap2]) => {
        if (snap2 && typeof snap2 === 'object' && snap2.shares === undefined) {
          /* {year: {month: {day: {...}}}} */
          Object.entries(snap2).forEach(([day, snap3]) => {
            if (snap3 && typeof snap3 === 'object') {
              list.push({
                dateKey: `${key}-${month}-${String(day).padStart(2, '0')}`,
                _rawPath: `${key}/${month}/${day}`,
                ...snap3,
              });
            }
          });
        } else if (snap2 && typeof snap2 === 'object') {
          /* {year: {month: {...}}} */
          list.push({
            dateKey: `${key}-${month}-01`,
            _rawPath: `${key}/${month}`,
            ...snap2,
          });
        }
      });
    }
  });
  list.sort((a, b) => a.dateKey.localeCompare(b.dateKey));
  return list;
}

/** 儲存快照（dateKey 格式：YYYY-MM-DD），同一天覆蓋 */
export async function saveFundSnapshot(fundId, dateKey, data) {
  if (!/^\d{4}-\d{2}-\d{2}$/.test(dateKey)) throw new Error('dateKey 必須是 YYYY-MM-DD 格式');
  const path = `funds/${fundId}/snapshots/${dateKey}`;
  const existing = await get(familyRef(path));
  const ex = existing.val() || {};
  await set(familyRef(path), {
    shares: Number(data.shares) || 0,
    nav: Number(data.nav) || 0,
    value: roundInt(Number(data.shares || 0) * Number(data.nav || 0)),
    createdAt: ex.createdAt || Date.now(),
    updatedAt: Date.now(),
  });
}

/**
 * 刪除某一筆快照
 * v103.0.37：加第 3 參數 rawPath（實際儲存路徑），支援舊格式刪除
 * @param {string} fundId
 * @param {string} dateKey - 顯示用日期（YYYY-MM-DD）
 * @param {string} [rawPath] - 實際儲存路徑（由 getFundSnapshotListOnce 附帶）
 * @returns {Promise<boolean>}
 */
export async function removeFundSnapshot(fundId, dateKey, rawPath) {
  if (!/^\d{4}-\d{2}-\d{2}$/.test(dateKey)) throw new Error('dateKey 必須是 YYYY-MM-DD 格式');
  const path = rawPath || dateKey;
  await remove(familyRef(`funds/${fundId}/snapshots/${path}`));
  return true;
}

/* ============ 跨來源改狀態 ============ */
export async function updateEntityStatus(source, row, ns, isDone) {
  const today = new Date().toISOString().slice(0, 10);
  switch (source) {
    case 'personal': {
      const { memberId, expenseId } = row._ref;
      await updateExpense(row.year, row.month, memberId, expenseId, { status: ns, repaidDate: isDone ? today : '' });
      return;
    }
    case 'insurance': throw new Error('保險狀態請呼叫 api.insuranceSync()');
    case 'fixed': throw new Error('fixed_expenses 已廢除');
    default: throw new Error('未知的來源：' + source);
  }
}

/* ============ 側邊欄（已廢除） ============ */
export function listenSidebarOrder(cb, err) {
  return listen('settings/sidebar_order', (snap) => {
    const v = snap.val();
    if (!v) { cb(null); return; }
    const arr = Array.isArray(v) ? v : Object.values(v);
    cb(arr.filter((x) => typeof x === 'string'));
  }, err);
}
export async function saveSidebarOrder(order) {
  if (!Array.isArray(order)) throw new Error('order 必須是陣列');
  await set(familyRef('settings/sidebar_order'), order);
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/core/db.js
   Version: 1792203600
   Batch: B35-1
   Writer: A20261016-001#003
   ═══════════════════════════════════════════ */```

---

## 📄 js/core/debug.js

```
// ============================================
// debug.js — 手機除錯工具（v103.0.14）
// 位置：js/core/debug.js
// ============================================
// v103.0.14 修正：
//   ✅ [問題8] 用 localStorage 持久化「已啟用」狀態（跨頁保持）
// ============================================

const VCONSOLE_CDN = 'https://cdn.jsdelivr.net/npm/vconsole@3.15.0/dist/vconsole.min.js';
const STORAGE_KEY = '__debug_enabled__';

let _initialized = false;
let _scriptLoaded = false;
let _scriptLoadingPromise = null;

/* ============================================
   主入口
   ============================================ */
export function initDebug() {
  if (_initialized) return;
  _initialized = true;

  /* URL 參數 ?debug=1 強制啟用並持久化 */
  const params = new URLSearchParams(location.search);
  if (params.get('debug') === '1') {
    _setEnabled(true);
    forceEnableDebug();
    return;
  }

  /* URL 參數 ?debug=0 關閉並清除 */
  if (params.get('debug') === '0') {
    _setEnabled(false);
    return;
  }

  /* 🆕 讀 localStorage 持久化標記 */
  if (_isEnabled()) {
    forceEnableDebug();
  }
}

/* ============================================
   啟用
   ============================================ */
export async function forceEnableDebug() {
  if (window.vConsole) return true;
  _setEnabled(true);

  try {
    await _loadScript();
  } catch (err) {
    console.warn('[debug] vConsole CDN 載入失敗：', err);
    return false;
  }

  if (!window.VConsole) return false;

  try {
    window.vConsole = new window.VConsole({ maxLogNumber: 2000 });
    window.__vconsole_loaded__ = true;
    console.log('[debug] vConsole 已啟用（跨頁保持）');
    return true;
  } catch (err) {
    console.warn('[debug] vConsole 初始化失敗：', err);
    return false;
  }
}

/* ============================================
   關閉
   ============================================ */
export function forceDisableDebug() {
  _setEnabled(false);
  if (window.vConsole && typeof window.vConsole.destroy === 'function') {
    try { window.vConsole.destroy(); } catch (err) {}
  }
  window.vConsole = null;
  window.__vconsole_loaded__ = false;
  console.log('[debug] vConsole 已關閉');
  return true;
}

export function isDebugEnabled() {
  return !!(window.vConsole && window.__vconsole_loaded__);
}

/* ============================================
   內部
   ============================================ */
function _isEnabled() {
  try { return localStorage.getItem(STORAGE_KEY) === '1'; } catch (e) { return false; }
}

function _setEnabled(on) {
  try {
    if (on) localStorage.setItem(STORAGE_KEY, '1');
    else localStorage.removeItem(STORAGE_KEY);
  } catch (e) {}
}

function _loadScript() {
  if (_scriptLoaded && window.VConsole) return Promise.resolve();
  if (_scriptLoadingPromise) return _scriptLoadingPromise;

  _scriptLoadingPromise = new Promise((resolve, reject) => {
    const existing = document.querySelector(`script[src="${VCONSOLE_CDN}"]`);
    if (existing && window.VConsole) {
      _scriptLoaded = true;
      resolve();
      return;
    }
    const script = document.createElement('script');
    script.src = VCONSOLE_CDN;
    script.async = true;
    script.onload = () => { _scriptLoaded = true; resolve(); };
    script.onerror = () => { _scriptLoadingPromise = null; reject(new Error('CDN 載入失敗')); };
    document.head.appendChild(script);
  });

  return _scriptLoadingPromise;
}

export const disableDebug = forceDisableDebug;
export const enableDebug = forceEnableDebug;```

---

## 📄 js/core/pwa.js

```
// ============================================
// pwa.js — PWA 初始化（v103.0.0）
// 位置：js/core/pwa.js
// ============================================
// v103.0.0 重構：
//   ✅ 版本號更新（無功能變更）
//   ✅ 保留 v101.10.0 全部功能
//      - 修復「手機開 app 需開 2 次」問題（_hadController 判斷）
//      - 動態注入 manifest / theme-color / iOS meta
//      - Service Worker 註冊 + 更新檢查
// ============================================

let _pwaInitialized = false;
let _swRegistration = null;

/**
 * 追蹤是否已有 controller
 * - 首次註冊：頁面載入時無 controller，SW activate → clients.claim() 觸發 controllerchange
 *   → 此時 _hadController === false，不 reload（避免「開 2 次」）
 * - SW 更新：頁面載入時已有 controller，新 SW activate → 觸發 controllerchange
 *   → 此時 _hadController === true，reload 一次讓頁面用新 SW
 */
let _hadController = false;
let _refreshing = false;

/**
 * 初始化 PWA
 * - 動態注入 manifest / theme-color / mobile-web-app-capable
 * - 註冊 Service Worker
 * - 監聽更新
 */
export function initPWA() {
  if (_pwaInitialized) return;
  _pwaInitialized = true;

  if ('serviceWorker' in navigator) {
    _hadController = !!navigator.serviceWorker.controller;
  }

  _injectMetaTags();
  _registerServiceWorker();
}

/* ============================================
   動態注入 meta / link
   ============================================ */
function _injectMetaTags() {
  const head = document.head;

  if (!document.querySelector('link[rel="manifest"]')) {
    const link = document.createElement('link');
    link.rel = 'manifest';
    link.href = './manifest.json';
    head.appendChild(link);
  }

  if (!document.querySelector('meta[name="theme-color"]')) {
    const meta = document.createElement('meta');
    meta.name = 'theme-color';
    meta.content = '#080B11';
    head.appendChild(meta);
  }

  if (!document.querySelector('meta[name="mobile-web-app-capable"]')) {
    const meta = document.createElement('meta');
    meta.name = 'mobile-web-app-capable';
    meta.content = 'yes';
    head.appendChild(meta);
  }

  if (!document.querySelector('meta[name="apple-mobile-web-app-capable"]')) {
    const meta = document.createElement('meta');
    meta.name = 'apple-mobile-web-app-capable';
    meta.content = 'yes';
    head.appendChild(meta);
  }

  if (!document.querySelector('meta[name="apple-mobile-web-app-status-bar-style"]')) {
    const meta = document.createElement('meta');
    meta.name = 'apple-mobile-web-app-status-bar-style';
    meta.content = 'black-translucent';
    head.appendChild(meta);
  }

  if (!document.querySelector('meta[name="apple-mobile-web-app-title"]')) {
    const meta = document.createElement('meta');
    meta.name = 'apple-mobile-web-app-title';
    meta.content = 'FAMILY.FIN';
    head.appendChild(meta);
  }
}

/* ============================================
   Service Worker 註冊
   ============================================ */
function _registerServiceWorker() {
  if (!('serviceWorker' in navigator)) return;

  window.addEventListener('load', async () => {
    try {
      const reg = await navigator.serviceWorker.register('./sw.js', { scope: './' });
      _swRegistration = reg;
      console.log('✅ Service Worker 已註冊：', reg.scope);

      reg.update().catch(() => { /* noop */ });

      reg.addEventListener('updatefound', () => {
        const newWorker = reg.installing;
        if (!newWorker) return;
        newWorker.addEventListener('statechange', () => {
          if (newWorker.state === 'installed' && navigator.serviceWorker.controller) {
            console.log('🔄 新版本已就緒，下次開啟生效');
          }
        });
      });
    } catch (err) {
      console.warn('⚠️ Service Worker 註冊失敗：', err);
    }
  });

  /* ============================================
     controllerchange 智慧判斷
     ============================================ */
  navigator.serviceWorker.addEventListener('controllerchange', () => {
    if (!_hadController) {
      _hadController = true;
      return;
    }

    if (_refreshing) return;
    _refreshing = true;

    console.log('🔄 Service Worker 已更新，重新載入頁面');
    window.location.reload();
  });
}

/* ============================================
   對外 API
   ============================================ */

/**
 * 手動觸發 SW 更新檢查
 * @returns {Promise<void>}
 */
export async function checkForUpdate() {
  if (_swRegistration) {
    try {
      await _swRegistration.update();
    } catch (err) {
      console.warn('[pwa] 檢查更新失敗：', err);
    }
  }
}

/**
 * 取得 SW 註冊物件（debug 用）
 */
export function getSWRegistration() {
  return _swRegistration;
}
```

---

## 📄 js/core/state.js

```
// ============================================
// state.js — 全域狀態中心（v103.0.42）
// 位置：js/core/state.js
// ============================================
// v103.0.42（B30-3b）：
//   ✅ 加 theme 狀態 + setTheme / getTheme
//   ✅ init() 讀取 localStorage 主題
// ============================================

import { STORAGE_KEYS, ROLES } from '../config/constants.js';
import { L } from '../config/label-registry.js';

const THEME_KEY = 'fin_ui_theme';

export const AppState = {
  currentUser: null,
  isSuperAdmin: false,
  currentFamilyId: '',
  currentFamilyName: '',
  familyOwnerUid: '',
  role: '',
  canInput: false,
  displayName: '',
  memberAccount: null,
  currentMemberId: '',
  year: '',
  month: '',
  currentView: 'table',
  theme: 'light',
  _listeners: {},
  _initialized: false,

  init() {
    if (this._initialized) return;
    this._initialized = true;

    const savedYear = localStorage.getItem(STORAGE_KEYS.YEAR);
    const savedMonth = localStorage.getItem(STORAGE_KEYS.MONTH);
    const savedFamilyId = localStorage.getItem(STORAGE_KEYS.FAMILY_ID);
    const savedFamilyName = localStorage.getItem(STORAGE_KEYS.FAMILY_NAME);
    const savedTheme = localStorage.getItem(THEME_KEY);

    this.year = savedYear || String(new Date().getFullYear());
    this.month = savedMonth || 'all';
    this.theme = (savedTheme === 'dark' || savedTheme === 'light') ? savedTheme : 'light';

    if (savedFamilyId) {
      this.currentFamilyId = savedFamilyId;
      this.currentFamilyName = savedFamilyName || '';
    }

    this._applyTheme(this.theme);
  },

  destroy() {
    this._listeners = {};
    this.currentUser = null;
    this.isSuperAdmin = false;
    this.currentFamilyId = '';
    this.currentFamilyName = '';
    this.familyOwnerUid = '';
    this.role = '';
    this.canInput = false;
    this.displayName = '';
    this.memberAccount = null;
    this.currentMemberId = '';
    this.currentView = 'table';
    this._initialized = false;
  },

  /* ---------- 主題 ---------- */
  _applyTheme(theme) {
    if (typeof document !== 'undefined') {
      document.documentElement.setAttribute('data-theme', theme);
    }
  },

  setTheme(theme) {
    if (theme !== 'light' && theme !== 'dark') return;
    if (this.theme === theme) return;
    this.theme = theme;
    localStorage.setItem(THEME_KEY, theme);
    this._applyTheme(theme);
    this.emit('theme-change', theme);
  },

  getTheme() { return this.theme; },

  toggleTheme() {
    this.setTheme(this.theme === 'light' ? 'dark' : 'light');
  },

  /* ---------- 使用者 ---------- */
  setUser(user) {
    this.currentUser = user;
    this.emit('user-change', user);
  },

  setSuperAdmin(isSuper) {
    this.isSuperAdmin = !!isSuper;
    if (this.isSuperAdmin) {
      this.role = ROLES.SUPERADMIN;
      this.canInput = true;
    }
    this.emit('superadmin-change', this.isSuperAdmin);
  },

  /* ---------- 家庭 ---------- */
  setFamily(familyId, familyName, ownerUid) {
    this.currentFamilyId = familyId || '';
    this.currentFamilyName = familyName || '';
    if (ownerUid !== undefined) this.familyOwnerUid = ownerUid || '';

    if (this.currentFamilyId) {
      localStorage.setItem(STORAGE_KEYS.FAMILY_ID, this.currentFamilyId);
      localStorage.setItem(STORAGE_KEYS.FAMILY_NAME, this.currentFamilyName);
    } else {
      localStorage.removeItem(STORAGE_KEYS.FAMILY_ID);
      localStorage.removeItem(STORAGE_KEYS.FAMILY_NAME);
    }

    this.emit('family-change', {
      familyId: this.currentFamilyId,
      familyName: this.currentFamilyName,
    });
  },

  getFamilyId() { return this.currentFamilyId; },
  getFamilyName() { return this.currentFamilyName; },
  getFamilyOwnerUid() { return this.familyOwnerUid || this.currentFamilyId; },
  hasFamily() { return !!this.currentFamilyId; },

  clearFamily() {
    this.currentFamilyId = '';
    this.currentFamilyName = '';
    this.familyOwnerUid = '';
    localStorage.removeItem(STORAGE_KEYS.FAMILY_ID);
    localStorage.removeItem(STORAGE_KEYS.FAMILY_NAME);
    this.emit('family-change', { familyId: '', familyName: '' });
  },

  /* ---------- 帳號 ---------- */
  setRole(role) {
    this.role = role || '';
    this.emit('role-change', this.role);
  },

  getRole() { return this.role; },

  setCanInput(canInput) {
    this.canInput = !!canInput;
    this.emit('permission-change', { canInput: this.canInput });
  },

  getCanInput() { return this.canInput; },

  setDisplayName(displayName) {
    this.displayName = displayName || '';
    this.emit('displayname-change', this.displayName);
  },

  getDisplayName() { return this.displayName; },

  setMemberAccount(account) {
    this.memberAccount = account || null;

    if (account) {
      this.role = account.role || this.role;
      this.canInput = account.canInput !== false;
      this.displayName = account.displayName || this.displayName;
      if (account.memberId) {
        this.currentMemberId = account.memberId;
      }
    } else {
      this.role = '';
      this.canInput = false;
      this.displayName = '';
      this.currentMemberId = '';
    }

    this.emit('member-account-change', this.memberAccount);
  },

  getMemberAccount() { return this.memberAccount; },

  getCurrentMemberId() {
    if (this.currentMemberId) return this.currentMemberId;
    if (this.memberAccount?.memberId) return this.memberAccount.memberId;
    return '';
  },

  setCurrentMemberId(memberId) {
    this.currentMemberId = memberId || '';
    this.emit('current-member-change', this.currentMemberId);
  },

  setAccountContext({ familyId, familyName, memberAccount, ownerUid }) {
    this.setFamily(familyId, familyName, ownerUid);
    this.setMemberAccount(memberAccount);
  },

  getRoleLabel() {
    switch (this.role) {
      case ROLES.SUPERADMIN: return L('roles.superadmin');
      case ROLES.OWNER:      return L('roles.owner');
      case ROLES.MEMBER:     return L('roles.member');
      default:               return '成員';
    }
  },

  getCanInputLabel() {
    return this.canInput ? '可輸入' : '唯讀';
  },

  /* ---------- 年月 ---------- */
  setYearMonth(year, month) {
    this.year = String(year);
    this.month = String(month);
    localStorage.setItem(STORAGE_KEYS.YEAR, this.year);
    localStorage.setItem(STORAGE_KEYS.MONTH, this.month);
    this.emit('ym-change', { year: this.year, month: this.month });
  },

  getYearMonth() { return { year: this.year, month: this.month }; },
  isAnnualMode() { return this.month === 'all'; },

  getYearMonthLabel(sep = ' ') {
    if (this.month === 'all') return `${this.year} 年 全年`;
    return `${this.year} 年${sep}${this.month} 月`;
  },

  /* ---------- 檢視 ---------- */
  setCurrentView(view) {
    if (view !== 'table' && view !== 'card') return;
    if (this.currentView === view) return;
    this.currentView = view;
    this.emit('view-change', view);
  },

  getCurrentView() { return this.currentView; },

  /* ---------- 事件總線 ---------- */
  on(event, callback) {
    if (!this._listeners[event]) this._listeners[event] = [];
    this._listeners[event].push(callback);
    return () => {
      this._listeners[event] = (this._listeners[event] || []).filter((cb) => cb !== callback);
    };
  },

  off(event, callback) {
    if (!this._listeners[event]) return;
    this._listeners[event] = this._listeners[event].filter((cb) => cb !== callback);
  },

  emit(event, data) {
    (this._listeners[event] || []).forEach((cb) => {
      try { cb(data); } catch (err) {
        console.error(`[AppState] 事件 ${event} 回呼失敗：`, err);
      }
    });
  },
};

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/core/state.js
   Version: v103.0.42
   Batch: B30-3b
   ═══════════════════════════════════════════ */```

---

## 📄 js/core/utils.js

```
// ============================================
// utils.js — 通用工具（v103.0.22）
// 位置：js/core/utils.js
// ============================================
// v103.0.22（B25-4）：
//   ✅ H2 + L7：renderEmptyState 自寫 esc 改用 lib/dom.js
// v103.0.11 重構：
//   ✅ [H02] 格式化函式改為從 lib/format.js re-export
//   ✅ [H03] escapeHtml 改為從 lib/dom.js re-export（別名 esc）
//   ✅ [M07] getMemberDisplayName 回傳 '（未命名）'
// ============================================

import { LIMITS, RESERVED_IDS } from '../config/constants.js';
import { esc } from '../lib/dom.js';

export {
  formatHKD,
  formatNumber,
  roundHKD,
  clampAmount,
  formatPercent,
  formatCellValue,
  formatTransactionType,
  formatTransactionCategory,
  getCategoryBadgeClass,
} from '../lib/format.js';

export { esc as escapeHtml };

/* ============================================
   日期工具
   ============================================ */
export function todayISO() {
  return new Date().toISOString().slice(0, 10);
}

export function currentYearMonth() {
  const d = new Date();
  return {
    year: String(d.getFullYear()),
    month: String(d.getMonth() + 1).padStart(2, '0'),
  };
}

export function dateToStr(date) {
  if (!date) return '';
  const d = date instanceof Date ? date : new Date(date);
  if (isNaN(d.getTime())) return '';
  const y = d.getFullYear();
  const m = String(d.getMonth() + 1).padStart(2, '0');
  const dd = String(d.getDate()).padStart(2, '0');
  return `${y}-${m}-${dd}`;
}

export function parseDate(str) {
  if (!str) return null;
  const d = new Date(str);
  return isNaN(d.getTime()) ? null : d;
}

/* ============================================
   字串工具
   ============================================ */
export function truncate(name, maxLen) {
  if (!name) return '';
  const s = String(name);
  let len = maxLen;
  if (len == null) {
    len = (typeof window !== 'undefined' && window.innerWidth < 640)
      ? LIMITS.NAME_MAX_LEN_MOBILE
      : LIMITS.NAME_MAX_LEN_DESKTOP;
  }
  if (s.length <= len) return s;
  return s.slice(0, len) + '…';
}

export function safeParseInt(value, fallback = 0) {
  const n = parseInt(value, 10);
  return isNaN(n) ? fallback : n;
}

export function safeParseFloat(value, fallback = 0) {
  const n = parseFloat(value);
  return isNaN(n) ? fallback : n;
}

/* ============================================
   DOM 工具
   ============================================ */
export function qs(sel, parent = document) {
  return parent.querySelector(sel);
}

export function qsa(sel, parent = document) {
  return [...parent.querySelectorAll(sel)];
}

export function setText(target, text) {
  const el = typeof target === 'string' ? document.getElementById(target) : target;
  if (el) el.textContent = text;
}

/**
 * 🆕 v103.0.22：內部 esc 改用 lib/dom.js 的 esc（SSOT）
 */
export function renderEmptyState(container, message, options = {}) {
  const el = typeof container === 'string' ? document.getElementById(container) : container;
  if (!el) return;

  const { icon, actionHtml } = options;

  const iconHtml = icon
    ? `<i data-lucide="${esc(icon)}" style="width:48px;height:48px;opacity:0.4;"></i>`
    : '';

  el.innerHTML = `
    <div class="empty-state">
      ${iconHtml}
      <p style="margin-top:12px;">${esc(message)}</p>
      ${actionHtml || ''}
    </div>
  `;

  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   成員排序
   ============================================ */
export function sortMembers(members) {
  return [...(members || [])].sort((a, b) => {
    const oa = a.order != null ? a.order : Number.MAX_SAFE_INTEGER;
    const ob = b.order != null ? b.order : Number.MAX_SAFE_INTEGER;
    if (oa !== ob) return oa - ob;
    return (a.createdAt || 0) - (b.createdAt || 0);
  });
}

export function isExtraIncome(memberId) {
  return memberId === RESERVED_IDS.EXTRA_INCOME;
}

export function isSharedMember(memberId) {
  return memberId === RESERVED_IDS.SHARED_MEMBER;
}

export function getMemberDisplayName(memberId, members = []) {
  if (isExtraIncome(memberId)) return '額外收入';
  if (isSharedMember(memberId)) return '家庭共用';
  const m = members.find((x) => x.id === memberId);
  return m ? m.name : '（未命名）';
}

/* ============================================
   效能工具
   ============================================ */
export function debounce(fn, wait = 300) {
  let timer = null;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), wait);
  };
}

export function throttle(fn, wait = 300) {
  let last = 0;
  return function (...args) {
    const now = Date.now();
    if (now - last >= wait) {
      last = now;
      fn.apply(this, args);
    }
  };
}

/* ============================================
   雜項
   ============================================ */
export function deepClone(obj) {
  if (obj == null || typeof obj !== 'object') return obj;
  return JSON.parse(JSON.stringify(obj));
}

export function uid(prefix = 'id') {
  return `${prefix}_${Date.now().toString(36)}_${Math.random().toString(36).slice(2, 8)}`;
}

export function arrayToMap(arr, keyField = 'id') {
  const map = {};
  (arr || []).forEach((item) => {
    if (item && item[keyField] != null) map[item[keyField]] = item;
  });
  return map;
}

export function makeSortFn(sortKey, order = 'asc') {
  const dir = order === 'desc' ? -1 : 1;
  return (a, b) => {
    const va = a[sortKey];
    const vb = b[sortKey];

    if (va == null && vb == null) return 0;
    if (va == null) return 1;
    if (vb == null) return -1;

    if (typeof va === 'number' && typeof vb === 'number') {
      return (va - vb) * dir;
    }

    return String(va).localeCompare(String(vb), 'zh-HK') * dir;
  };
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/core/utils.js
   Version: v103.0.22
   Batch: B25-4
   ═══════════════════════════════════════════ */```

---

## 📄 js/engines/data-engine.js

```
// ============================================
// data-engine.js — 資料引擎（v103.0.11）
// 位置：js/engines/data-engine.js
// ============================================
// v103.0.11 修正：
//   ✅ [B03] resolvePath 支援 {__all__} 佔位符（自動移除該段路徑）
//   ✅ [B03] 新增 type: 'raw'（保留原始物件，不轉 list）
// ============================================

import {
  ref, onValue, get,
} from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { db } from '../config/firebase-config.js';
import { AppState } from '../core/state.js';

/* ============================================
   1. 路徑解析
   ============================================ */

/**
 * 解析路徑為完整 Firebase 路徑
 * - 自動加上 `families/{familyId}/` 前綴
 * - 替換 `{key}` 佔位符（從 params 取值）
 * - `{__all__}` 特殊佔位符：移除該段路徑（含前導 /）
 *
 * @param {string} rawPath - 如 'members' / 'expenses/{year}/{month}/member_expenses'
 * @param {Object} [params={}] - 佔位符對應值
 * @returns {string}
 */
export function resolvePath(rawPath, params = {}) {
  const familyId = AppState.getFamilyId();
  if (!familyId) throw new Error('尚未選擇家庭');

  if (!rawPath) throw new Error('路徑為必填');

  // 🆕 先處理 {__all__}（移除該段，含前導 /）
  let cleaned = String(rawPath).replace(/\/?\{__all__\}/g, '');

  // 再處理一般佔位符
  const resolved = cleaned.replace(/\{(\w+)\}/g, (_, key) => {
    if (params[key] == null || params[key] === '') {
      console.warn(`[data-engine] 路徑佔位符 {${key}} 未提供值`);
      return '';
    }
    return String(params[key]);
  });

  return `families/${familyId}/${resolved}`;
}

/* ============================================
   2. 訂閱
   ============================================ */

/**
 * 訂閱一個 Firebase 路徑
 *
 * @param {Object} cfg
 * @param {string} cfg.path - 相對路徑（可含 {key} / {__all__} 佔位符）
 * @param {'list'|'object'|'value'|'raw'} [cfg.type='object']
 * @param {Function} [cfg.transform] - (data) => newData
 * @param {Object} [cfg.params={}] - 佔位符對應值
 * @param {Function} callback - ({ type, data, raw, error }) => void
 * @returns {Function} unsubscribe
 */
export function subscribe(cfg, callback) {
  const {
    path,
    type = 'object',
    transform,
    params = {},
  } = cfg || {};

  let resolvedPath;
  try {
    resolvedPath = resolvePath(path, params);
  } catch (err) {
    console.warn('[data-engine] 路徑解析失敗：', err);
    callback({ type, data: type === 'list' ? [] : null, error: err });
    return () => {};
  }

  const r = ref(db, resolvedPath);

  const unsub = onValue(
    r,
    (snap) => {
      let data = snap.val();

      // 🆕 raw 模式：不轉換
      if (type === 'raw') {
        // 保留原始物件
      } else if (type === 'list') {
        data = _toList(data);
      }

      if (typeof transform === 'function') {
        try {
          data = transform(data, snap);
        } catch (err) {
          console.error(`[data-engine] transform 失敗 [${path}]：`, err);
        }
      }

      try {
        callback({ type, data, raw: snap });
      } catch (err) {
        console.error(`[data-engine] callback 失敗 [${path}]：`, err);
      }
    },
    (err) => {
      console.error(`[data-engine] 訂閱失敗 [${resolvedPath}]：`, err);
      callback({ type, data: type === 'list' ? [] : null, error: err });
    }
  );

  return unsub;
}

/* ============================================
   3. 一次性讀取
   ============================================ */

/**
 * 一次性讀取路徑
 *
 * @param {Object} cfg
 * @param {string} cfg.path
 * @param {'list'|'object'|'value'|'raw'} [cfg.type='object']
 * @param {Function} [cfg.transform]
 * @param {Object} [cfg.params={}]
 * @returns {Promise<*>}
 */
export async function fetchOnce(cfg) {
  const {
    path,
    type = 'object',
    transform,
    params = {},
  } = cfg || {};

  const resolvedPath = resolvePath(path, params);
  const snap = await get(ref(db, resolvedPath));

  let data = snap.val();
  if (type === 'raw') {
    // 保留
  } else if (type === 'list') {
    data = _toList(data);
  }
  if (typeof transform === 'function') {
    data = transform(data, snap);
  }

  return data;
}

/* ============================================
   4. 響應式物件（Proxy 深層代理）
   ============================================ */

/**
 * 建立響應式物件
 * - 深層代理：任何嵌套屬性變更皆觸發 onChange
 * - 相同值不觸發
 *
 * @param {Object} obj - 初始物件
 * @param {Function} onChange - (keyPath, value, oldValue) => void
 * @param {string} [pathPrefix='']
 * @returns {Proxy}
 */
export function makeReactive(obj, onChange, pathPrefix = '') {
  const handler = {
    set(target, key, value) {
      const keyPath = pathPrefix ? `${pathPrefix}.${key}` : String(key);
      const oldValue = target[key];

      if (oldValue === value) return true;

      if (value != null && typeof value === 'object' && !Array.isArray(value)) {
        target[key] = _deepProxy(value, onChange, keyPath);
      } else {
        target[key] = value;
      }

      try {
        onChange(keyPath, target[key], oldValue);
      } catch (err) {
        console.error('[data-engine] onChange 失敗：', err);
      }
      return true;
    },

    get(target, key) {
      return target[key];
    },

    deleteProperty(target, key) {
      const keyPath = pathPrefix ? `${pathPrefix}.${key}` : String(key);
      if (!(key in target)) return true;

      const oldValue = target[key];
      delete target[key];

      try {
        onChange(keyPath, undefined, oldValue);
      } catch (err) {
        console.error('[data-engine] onChange 失敗：', err);
      }
      return true;
    },
  };

  return _deepProxy(obj, onChange, pathPrefix, handler);
}

/**
 * 內部：深層代理工具
 */
function _deepProxy(obj, onChange, pathPrefix, customHandler) {
  const handler = customHandler || {
    set(target, key, value) {
      const keyPath = pathPrefix ? `${pathPrefix}.${key}` : String(key);
      const oldValue = target[key];
      if (oldValue === value) return true;

      if (value != null && typeof value === 'object' && !Array.isArray(value)) {
        target[key] = _deepProxy(value, onChange, keyPath);
      } else {
        target[key] = value;
      }

      try { onChange(keyPath, target[key], oldValue); } catch (err) {
        console.error('[data-engine] onChange 失敗：', err);
      }
      return true;
    },
    get(target, key) { return target[key]; },
    deleteProperty(target, key) {
      const keyPath = pathPrefix ? `${pathPrefix}.${key}` : String(key);
      if (!(key in target)) return true;
      const oldValue = target[key];
      delete target[key];
      try { onChange(keyPath, undefined, oldValue); } catch (err) {
        console.error('[data-engine] onChange 失敗：', err);
      }
      return true;
    },
  };

  if (obj && typeof obj === 'object' && !Array.isArray(obj)) {
    Object.keys(obj).forEach((key) => {
      const val = obj[key];
      if (val != null && typeof val === 'object' && !Array.isArray(val)) {
        obj[key] = _deepProxy(val, onChange, pathPrefix ? `${pathPrefix}.${key}` : key);
      }
    });
  }

  return new Proxy(obj, handler);
}

/* ============================================
   5. 工具函式
   ============================================ */

/**
 * 物件 / null → 陣列（含 id）
 */
function _toList(val) {
  if (!val || typeof val !== 'object') return [];
  return Object.entries(val).map(([id, x]) => {
    if (x != null && typeof x === 'object') return { id, ...x };
    return { id, value: x };
  });
}

/**
 * 深層取值（'a.b.c'）
 */
export function getNestedValue(obj, path) {
  if (obj == null || !path) return undefined;
  return String(path).split('.').reduce(
    (acc, key) => (acc == null ? undefined : acc[key]),
    obj
  );
}

/**
 * 深層設值（會觸發 Proxy onChange）
 */
export function setNestedValue(obj, path, value) {
  if (!path) return;
  const keys = String(path).split('.');
  const last = keys.pop();
  const target = keys.reduce((acc, key) => {
    if (acc[key] == null || typeof acc[key] !== 'object') acc[key] = {};
    return acc[key];
  }, obj);
  target[last] = value;
}```

---

## 📄 js/engines/page-engine.js

```
// ============================================
// page-engine.js — 頁面引擎（v103.0.36）
// 位置：js/engines/page-engine.js
// ============================================
// v103.0.36（B29-8）：
//   ✅ _applyPageMetadata 清空 subtitleEl.textContent（避免硬編碼殘留）
// v103.0.34（B29-5c）：
//   ✅ 應用頁面 metadata（title / subtitleStatic）
// ============================================

import { subscribe, makeReactive, getNestedValue } from './data-engine.js';
import { mountBlock, collectBlockDeps } from './render-engine.js';
import { AppState } from '../core/state.js';
import { listenPageMetadata } from '../core/db.js';
import { getPageKeyFromUrl, getDefaultMetadata } from '../config/page-metadata-registry.js';

/* ============================================
   錯誤橫幅
   ============================================ */
function _showError(msg) {
  try {
    let b = document.getElementById('__page_engine_error__');
    if (!b) {
      b = document.createElement('div');
      b.id = '__page_engine_error__';
      b.style.cssText = 'position:fixed;top:0;left:0;right:0;background:#F43F5E;color:#fff;padding:12px 16px;font-size:13px;font-family:monospace;z-index:99999;white-space:pre-wrap;max-height:50vh;overflow-y:auto;';
      document.body.appendChild(b);
    }
    b.textContent += `[page-engine] ${msg}\n`;
  } catch (e) {}
}

function _hideError() {
  const b = document.getElementById('__page_engine_error__');
  if (b) b.remove();
}

/* ============================================
   主入口
   ============================================ */
export async function createPage(schema) {
  if (!schema || typeof schema !== 'object') throw new Error('[page-engine] schema 為必填');
  _hideError();

  const ctx = {
    meta: { title: schema.title || '' },
    state: {}, data: {}, derived: {},
    _unsubs: [], _blockInstances: [], _dependents: {}, _stateUnsubs: [],
    _dataChangeCallbacks: [],
    _customMountInstance: null,
    _metaUnsub: null,
    invalidate: (key) => _onKeyChange(ctx, schema, key),
    setState: (keyPath, value) => _setState(ctx, keyPath, value),

    onDataChange: (fn) => {
      if (typeof fn !== 'function') return () => {};
      ctx._dataChangeCallbacks.push(fn);
      return () => {
        ctx._dataChangeCallbacks = ctx._dataChangeCallbacks.filter((f) => f !== fn);
      };
    },
  };

  try {
    ctx.state = makeReactive({ ...(schema.state || {}) }, (keyPath) => _onStateChange(ctx, schema, keyPath));
  } catch (err) { _showError('makeReactive 失敗：' + err.message); }

  _applyPageMetadata(ctx);

  _buildDependencyGraph(ctx, schema);
  _subscribeAllData(ctx, schema);
  _bindAppStateEvents(ctx, schema);
  _computeAllDerived(ctx, schema);

  try { await _mountAllBlocks(ctx, schema); } catch (err) { _showError('mount 失敗：' + err.message); }

  if (typeof schema.customMount === 'function') {
    try { ctx._customMountInstance = await schema.customMount(ctx); } catch (err) { _showError('customMount 失敗：' + err.message); }
  }

  return { ctx, destroy: () => _destroy(ctx), ready: Promise.resolve() };
}

/* ============================================
   應用頁面 metadata
   -------------------------------------------------
   - 從 URL 取 pageKey
   - 清空 HTML 硬編碼；套 registry 預設
   - 訂閱 Firebase 套自訂
   - subtitleStatic 存 data-prefix（CSS ::before 顯示）
   ============================================ */
function _applyPageMetadata(ctx) {
  const pageKey = getPageKeyFromUrl();
  const defaultMeta = getDefaultMetadata(pageKey);

  const titleEl = document.querySelector('.page-title');
  const subtitleEl = document.querySelector('.page-subtitle');

  /* 立即清空 + 套預設（避免 FOUC） */
  if (titleEl && defaultMeta.title) titleEl.textContent = defaultMeta.title;
  if (subtitleEl) {
    subtitleEl.textContent = '';
    if (defaultMeta.subtitleStatic) subtitleEl.dataset.prefix = defaultMeta.subtitleStatic;
    else subtitleEl.removeAttribute('data-prefix');
  }

  try {
    const unsub = listenPageMetadata((all) => {
      const custom = (all && all[pageKey]) || {};
      const title = (custom.title != null && custom.title !== '')
        ? custom.title
        : defaultMeta.title;
      const subStatic = (custom.subtitleStatic != null)
        ? custom.subtitleStatic
        : (defaultMeta.subtitleStatic || '');

      if (titleEl) titleEl.textContent = title;
      if (subtitleEl) {
        subtitleEl.textContent = '';
        if (subStatic) subtitleEl.dataset.prefix = subStatic;
        else subtitleEl.removeAttribute('data-prefix');
      }
    });
    ctx._metaUnsub = unsub;
  } catch (err) {
    console.warn('[page-engine] 載入頁面 metadata 失敗：', err);
  }
}

/* ============================================
   1. 依賴圖
   ============================================ */
function _buildDependencyGraph(ctx, schema) {
  ctx._dependents = {};
  const add = (d, n) => {
    if (!ctx._dependents[d]) ctx._dependents[d] = [];
    if (!ctx._dependents[d].includes(n)) ctx._dependents[d].push(n);
  };
  Object.entries(schema.derived || {}).forEach(([n, c]) => (c.deps || []).forEach((d) => add(d, n)));
}

/* ============================================
   2. data 訂閱
   ============================================ */
function _subscribeAllData(ctx, schema) {
  Object.entries(schema.data || {}).forEach(([key, cfg]) => {
    try {
      const unsub = subscribe(cfg, ({ data, error }) => {
        if (error) { console.warn(`[page-engine] data.${key} 錯誤：`, error); return; }
        ctx.data[key] = data;
        _onKeyChange(ctx, schema, 'data.' + key);
      });
      ctx._unsubs.push(unsub);
    } catch (err) { _showError(`訂閱 data.${key} 失敗：${err.message}`); }
  });
}

/* ============================================
   3. AppState 事件
   ============================================ */
function _bindAppStateEvents(ctx, schema) {
  const handle = () => {
    _resubscribeDataWithParams(ctx, schema);
    _computeAllDerived(ctx, schema);
    _notifyAllBlocks(ctx);
    _notifyDataChangeCallbacks(ctx, '__APP__');
  };
  try {
    ctx._stateUnsubs.push(AppState.on('ym-change', handle));
    ctx._stateUnsubs.push(AppState.on('family-change', handle));
  } catch (err) { /* noop */ }
}

function _resubscribeDataWithParams(ctx, schema) {
  const hasParam = Object.values(schema.data || {}).some((c) => /\{\w+\}/.test(c.path || ''));
  if (!hasParam) return;
  ctx._unsubs.forEach((u) => { try { u(); } catch (e) {} });
  ctx._unsubs = [];
  _subscribeAllData(ctx, schema);
}

/* ============================================
   4. 變更
   ============================================ */
function _onStateChange(ctx, schema, keyPath) {
  _onKeyChange(ctx, schema, `state.${keyPath}`);
}

function _onKeyChange(ctx, schema, changedKey) {
  const dirty = new Set();
  _markDirty(ctx, changedKey, dirty);
  if (dirty.size > 0) _recomputeDerivedSubset(ctx, schema, dirty);
  _notifyBlocksForChangedKeys(ctx, changedKey, dirty);
  _notifyDataChangeCallbacks(ctx, changedKey);
}

/* ============================================
   5. 通知
   ============================================ */
function _notifyDataChangeCallbacks(ctx, changedKey) {
  ctx._dataChangeCallbacks.forEach((fn) => {
    try { fn(changedKey); } catch (err) {
      console.error('[page-engine] onDataChange 失敗：', err);
    }
  });
}

function _notifyAllBlocks(ctx) {
  ctx._blockInstances.forEach(({ instance }) => {
    try { instance.onDepsChange?.(); } catch (e) {}
  });
}

function _notifyBlocksForChangedKeys(ctx, changedKey, dirtySet) {
  const affected = new Set(dirtySet);
  affected.add(changedKey);
  ctx._blockInstances.forEach(({ instance, deps }) => {
    const should = _hasAny(deps.derived, affected) ||
      (changedKey.startsWith('state.') && _hasAny(deps.state, affected));
    if (should && instance.onDepsChange) {
      try { instance.onDepsChange(); } catch (e) {}
    }
  });
}

function _hasAny(set, keys) {
  if (!set || set.size === 0) return false;
  for (const k of keys) if (set.has(k)) return true;
  return false;
}

/* ============================================
   6. derived
   ============================================ */
function _markDirty(ctx, key, dirtySet) {
  if (dirtySet.has(key)) return;
  dirtySet.add(key);
  (ctx._dependents[key] || []).forEach((d) => _markDirty(ctx, d, dirtySet));
}

function _computeAllDerived(ctx, schema) {
  const def = schema.derived || {};
  _topoSort(Object.keys(def), def).forEach((n) => _computeOne(ctx, schema, n, def[n]));
}

function _recomputeDerivedSubset(ctx, schema, dirtySet) {
  const def = schema.derived || {};
  _topoSort(Object.keys(def), def).forEach((n) => {
    if (dirtySet.has(n)) _computeOne(ctx, schema, n, def[n]);
  });
}

function _computeOne(ctx, schema, name, cfg) {
  const deps = cfg.deps || [];
  const def = schema.derived || {};
  try {
    const args = deps.map((d) => _resolveDep(ctx, d));
    const hasUndef = deps.some((d, i) => {
      if (d.startsWith('data.')) return args[i] === undefined;
      if (d in def) return args[i] === undefined;
      return false;
    });
    if (hasUndef) { ctx.derived[name] = undefined; return; }
    ctx.derived[name] = cfg.compute(...args);
  } catch (err) {
    console.error(`[page-engine] derived.${name} 失敗：`, err);
    ctx.derived[name] = undefined;
  }
}

function _resolveDep(ctx, dep) {
  if (!dep) return undefined;
  if (dep.startsWith('state.')) return getNestedValue(ctx.state, dep.slice(6));
  if (dep.startsWith('data.')) return getNestedValue(ctx.data, dep.slice(5));
  if (dep in ctx.derived) return ctx.derived[dep];
  if (dep in ctx.data) return ctx.data[dep];
  return undefined;
}

function _topoSort(names, defs) {
  const visited = new Set(), result = [];
  const visit = (n, stack) => {
    if (visited.has(n)) return;
    if (stack.has(n)) return;
    stack.add(n);
    ((defs[n] || {}).deps || []).forEach((d) => { if (d in defs) visit(d, stack); });
    stack.delete(n);
    visited.add(n);
    result.push(n);
  };
  names.forEach((n) => visit(n, new Set()));
  return result;
}

/* ============================================
   7. Blocks
   ============================================ */
async function _mountAllBlocks(ctx, schema) {
  for (const block of (schema.blocks || [])) {
    const inst = await mountBlock(block, ctx);
    if (!inst) { _showError(`block "${block.type}" 失敗`); continue; }
    ctx._blockInstances.push({ block, instance: inst, deps: collectBlockDeps(block) });
  }
}

/* ============================================
   8. state 設值
   ============================================ */
function _setState(ctx, keyPath, value) {
  if (!keyPath) return;
  const keys = String(keyPath).split('.');
  const last = keys.pop();
  const target = keys.reduce((acc, k) => {
    if (acc[k] == null || typeof acc[k] !== 'object') acc[k] = {};
    return acc[k];
  }, ctx.state);
  target[last] = value;
}

/* ============================================
   9. 銷毀
   ============================================ */
function _destroy(ctx) {
  if (ctx._customMountInstance?.destroy) {
    try { ctx._customMountInstance.destroy(); } catch (e) {}
  }
  if (ctx._metaUnsub) { try { ctx._metaUnsub(); } catch (e) {} ctx._metaUnsub = null; }
  ctx._blockInstances.forEach(({ instance }) => {
    try { instance.destroy?.(); } catch (e) {}
  });
  ctx._unsubs.forEach((u) => { try { u(); } catch (e) {} });
  ctx._stateUnsubs.forEach((u) => { try { u(); } catch (e) {} });
  ctx._blockInstances = [];
  ctx._unsubs = [];
  ctx._stateUnsubs = [];
  ctx._dataChangeCallbacks = [];
  ctx.data = {};
  ctx.derived = {};
  ctx._dependents = {};
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/engines/page-engine.js
   Version: v103.0.36
   Batch: B29-8
   ═══════════════════════════════════════════ */```

---

## 📄 js/engines/render-engine.js

```
// ============================================
// render-engine.js — 渲染引擎（v103.0.0）
// 位置：js/engines/render-engine.js
// ============================================
// 職責：
//   1. 解讀 Schema 中的表達式（'$.xxx' / 'state.xxx'）
//   2. 依 block.type 動態載入對應 Block 模組並掛載
//   3. 提供 block 依賴掃描（供 page-engine 通知）
//
// 設計原則：
//   - 表達式解析集中此處（不散落各 block）
//   - block 掛載採動態 import，未使用的 block 不載入
// ============================================

import { getNestedValue } from './data-engine.js';

/* ============================================
   1. 表達式解析
   ============================================ */

/**
 * 解析表達式
 *
 * 支援格式：
 *   '$.stats'       → ctx.derived.stats 或 ctx.data.stats
 *   'state.filters' → ctx.state.filters
 *   '$data.members' → ctx.data.members
 *   其他字串         → 原樣回傳
 *
 * @param {*} expr
 * @param {Object} ctx
 * @returns {*}
 */
export function resolveExpr(expr, ctx) {
  if (typeof expr !== 'string') return expr;
  if (!ctx) return expr;

  /* ---------- $.xxx → derived 優先，其次 data ---------- */
  if (expr.startsWith('$.')) {
    const key = expr.slice(2);
    if (ctx.derived && key in ctx.derived) return ctx.derived[key];
    if (ctx.data && key in ctx.data) return ctx.data[key];
    return undefined;
  }

  /* ---------- $data.xxx → data ---------- */
  if (expr.startsWith('$data.')) {
    const key = expr.slice(6);
    return getNestedValue(ctx.data, key);
  }

  /* ---------- $state.xxx → state ---------- */
  if (expr.startsWith('$state.')) {
    const key = expr.slice(7);
    return getNestedValue(ctx.state, key);
  }

  /* ---------- state.xxx → state ---------- */
  if (expr.startsWith('state.')) {
    return getNestedValue(ctx.state, expr.slice(6));
  }

  /* ---------- data.xxx → data ---------- */
  if (expr.startsWith('data.')) {
    return getNestedValue(ctx.data, expr.slice(5));
  }

  return expr;
}

/**
 * 解析物件中所有表達式（遞迴）
 * - 只處理字串值
 * - 物件 / 陣列遞迴
 *
 * @param {*} obj
 * @param {Object} ctx
 * @returns {*}
 */
export function resolveDeep(obj, ctx) {
  if (obj == null) return obj;

  if (typeof obj === 'string') {
    return resolveExpr(obj, ctx);
  }

  if (Array.isArray(obj)) {
    return obj.map((x) => resolveDeep(x, ctx));
  }

  if (typeof obj === 'object') {
    const out = {};
    Object.entries(obj).forEach(([k, v]) => {
      out[k] = resolveDeep(v, ctx);
    });
    return out;
  }

  return obj;
}

/* ============================================
   2. Block 掛載
   ============================================ */

/**
 * 掛載一個 Block
 *
 * @param {Object} block - { type, container, ... }
 * @param {Object} ctx
 * @returns {Promise<{onDepsChange: Function, destroy: Function}|null>}
 */
export async function mountBlock(block, ctx) {
  const { type } = block || {};
  if (!type) {
    console.warn('[render-engine] block.type 為必填');
    return null;
  }

  let mod;
  try {
    mod = await import(`../blocks/${type}-block.js`);
  } catch (err) {
    console.warn(`[render-engine] 找不到 block：${type}-block.js`, err);
    return null;
  }

  if (typeof mod.mount !== 'function') {
    console.warn(`[render-engine] block ${type} 沒有 export mount`);
    return null;
  }

  try {
    const instance = await mod.mount(block, ctx);
    return instance || { onDepsChange: () => {}, destroy: () => {} };
  } catch (err) {
    console.error(`[render-engine] block ${type} 掛載失敗：`, err);
    return null;
  }
}

/* ============================================
   3. Block 依賴掃描
   ============================================ */

const DERIVED_REF_RE = /\$\.([a-zA-Z_$][\w$]*)/g;
const STATE_REF_RE = /state\.([a-zA-Z_$][\w$]*(?:\.[a-zA-Z_$][\w$]*)*)/g;

/**
 * 掃描 Block 中所有表達式依賴
 *
 * @param {Object} block
 * @returns {{ derived: Set<string>, state: Set<string> }}
 */
export function collectBlockDeps(block) {
  const derived = new Set();
  const state = new Set();

  const scan = (obj) => {
    if (obj == null) return;

    if (typeof obj === 'string') {
      // 掃 $.xxx
      let m;
      DERIVED_REF_RE.lastIndex = 0;
      while ((m = DERIVED_REF_RE.exec(obj)) !== null) {
        derived.add(m[1]);
      }

      // 掃 state.xxx
      STATE_REF_RE.lastIndex = 0;
      while ((m = STATE_REF_RE.exec(obj)) !== null) {
        state.add(m[1]);
      }
      return;
    }

    if (Array.isArray(obj)) {
      obj.forEach(scan);
      return;
    }

    if (typeof obj === 'object') {
      Object.values(obj).forEach(scan);
    }
  };

  scan(block);

  return { derived, state };
}
```

---

## 📄 js/entity/entity-definitions.js

```
// entity-definitions.js — 實體表單定義 SSOT（v103.0.27）
// v103.0.27（B34-4）：
//   ✅ 各 select 欄位加 plusEntity / plusValueKey（D-029 / Q164 B / Q165 A）
// v103.0.26（B34-4）：
//   ✅ 移除死代碼 _createPolicyWithAdvance
// ============================================
import {
  addMember, updateMember, removeMember, deleteMemberAndData,
  addBank, updateBank, removeBank, deleteBankAndBalances,
  addInsurancePolicy, updateInsurancePolicy, removeInsurancePolicy, deleteInsurancePolicyAndData,
  addFund, updateFund, removeFund,
  addCategory, updateCategory, removeCategory,
  addItem, updateItem, removeItem,
  addPaymentMethod, updatePaymentMethod, removePaymentMethod,
  addStatus, updateStatus, removeStatus,
} from '../core/db.js';
import { ENTITY_KEYS, LIMITS } from '../config/constants.js';

const PAYMENT_TYPES = {
  ANNUAL: '年繳',
  MONTHLY: '月繳',
  ONCE: '一次付款',
};

const POLICY_SUBTYPES = {
  normal:         'NORM',
  fund_insurance: 'FUND',
  savings:        'SAVE',
};

function calcMonthlyExpense(data) {
  const policyType = data.type || 'normal';
  const paymentType = data.paymentType || PAYMENT_TYPES.ANNUAL;
  const ty = Number(data.totalPolicyYears) || 1;
  if (policyType === 'fund_insurance') return Math.round(Number(data.monthlyPremium) || 0);
  switch (paymentType) {
    case PAYMENT_TYPES.ANNUAL:  return Math.round((Number(data.annualPremium) || 0) / 12);
    case PAYMENT_TYPES.MONTHLY: return Math.round(Number(data.monthlyPremium) || 0);
    case PAYMENT_TYPES.ONCE:    return Math.round((Number(data.oncePremium) || 0) / ty / 12);
    default: return 0;
  }
}

const ENTITY_DEFS = {
  [ENTITY_KEYS.MEMBER]: {
    key: ENTITY_KEYS.MEMBER, label: '成員', labelPlural: '成員', icon: 'users',
    codePrefix: 'MEM',
    db: { create: addMember, update: updateMember, remove: removeMember, delete: deleteMemberAndData },
    fields: [
      { id: 'name', label: '名稱', type: 'text', required: true, maxlength: 20, placeholder: '例如：老公、梓舜' },
      { id: 'role', label: '角色', type: 'select', required: true, optionsSource: 'memberRoles' },
      { id: 'order', label: '排序', type: 'number-plain', min: 0, defaultValue: 0 },
    ],
    ui: {
      listColumns: ['name', 'role', 'order'], cardFields: ['role', 'order'], primaryColumn: 'name',
      searchFields: ['name'],
      sortOptions: [{ value: 'order-asc', label: '排序（小→大）' }, { value: 'name-asc', label: '名稱（A→Z）' }],
      defaultSort: 'order-asc', pageSize: LIMITS.DEFAULT_TABLE_PAGE_SIZE,
      canCreate: true, canEdit: true, canDelete: true,
      deleteConfirmText: (row) => `⚠️ 確定要刪除成員「${row.name}」嗎？\n\n這將會一併刪除該成員在所有月份的所有支出紀錄（含保險平攤），此操作無法復原。`,
    },
    beforeCreate: (data, allRows) => {
      const maxOrder = (allRows || []).reduce((max, m) => Math.max(max, m.order != null ? m.order : -1), -1);
      return { ...data, order: maxOrder + 1 };
    },
  },

  [ENTITY_KEYS.BANK]: {
    key: ENTITY_KEYS.BANK, label: '銀行', labelPlural: '銀行', icon: 'landmark',
    codePrefix: 'PAY-BANK',
    db: { create: addBank, update: updateBank, remove: removeBank, delete: deleteBankAndBalances },
    fields: [{ id: 'name', label: '銀行名稱', type: 'text', required: true, maxlength: 20, placeholder: '例如：中銀、匯豐' }],
    ui: {
      listColumns: ['name'], cardFields: ['name'], primaryColumn: 'name', searchFields: ['name'],
      sortOptions: [{ value: 'name-asc', label: '名稱（A→Z）' }, { value: 'name-desc', label: '名稱（Z→A）' }],
      defaultSort: 'name-asc', pageSize: LIMITS.DEFAULT_TABLE_PAGE_SIZE,
      canCreate: true, canEdit: true, canDelete: true,
      deleteConfirmText: (row) => `⚠️ 確定要刪除「${row.name}」嗎？\n\n這將會一併刪除該銀行在所有月份的結餘紀錄，此操作無法復原。`,
    },
    validate: (data, allRows, currentId) => {
      const name = (data.name || '').trim();
      if (allRows && allRows.some((b) => b.id !== currentId && b.name === name)) return { field: 'name', message: '此銀行名稱已存在' };
      return null;
    },
  },

  [ENTITY_KEYS.POLICY]: {
    key: ENTITY_KEYS.POLICY, label: '保單', labelPlural: '保單', icon: 'shield',
    codePrefix: 'POL',
    policySubtypes: POLICY_SUBTYPES,
    db: { create: addInsurancePolicy, update: updateInsurancePolicy, remove: removeInsurancePolicy, delete: deleteInsurancePolicyAndData },
    fields: [
      { id: 'type', label: '保單類型', type: 'select', required: true, optionsSource: 'policyTypes' },
      { id: 'memberId', label: '受保人', type: 'select', required: true, optionsSource: 'members', plusEntity: 'member' },
      { id: 'policyHolderId', label: '保單持有人', type: 'select', required: false, optionsSource: 'members', emptyText: '— 同受保人 —', hint: '若未選擇，預設與受保人相同', plusEntity: 'member' },
      { id: 'name', label: '保單名稱', type: 'text', required: true, maxlength: 60, placeholder: '例如：危疾+住院' },
      { id: 'company', label: '保險公司', type: 'select', required: false, optionsSource: 'companies', plusEntity: 'company', plusValueKey: 'name' },
      { id: 'paymentMode', label: '付款模式', type: 'select', required: true, optionsSource: 'paymentModes', defaultValue: 'direct' },
      { id: 'advanceHolderId', label: '代墊成員（若為代墊模式）', type: 'select', required: false, optionsSource: 'members', emptyText: '— 請選擇 —', hint: '僅在「代墊模式」時需要', plusEntity: 'member' },
      { id: 'firstStartYear', label: '保單開始年份', type: 'number-plain', required: true, min: 2000, max: 2100, defaultValue: () => new Date().getFullYear() },
      { id: 'firstStartMonth', label: '保單開始月份', type: 'select', required: true, optionsSource: 'months', defaultValue: '01' },
      { id: 'totalPolicyYears', label: '總供款年期', type: 'number-plain', required: true, min: 1, max: 50, defaultValue: 5 },
      { id: 'currentPeriodIndex', label: '當前第幾年度', type: 'number-plain', required: true, min: 1, max: 50, defaultValue: 1 },
      { id: 'paymentType', label: '付款類型', type: 'select', required: true, optionsSource: 'insurancePaymentTypes' },
      { id: 'account', label: '扣款帳戶（可選）', type: 'text', required: false, maxlength: 60 },
      { id: 'monthlyPremium', label: '月繳 / 月供金額（HK$）', type: 'number', required: false, min: 0, step: 1, defaultValue: 0, hint: '💡 月繳或基金保險（投資型）填此欄' },
      { id: 'annualPremium', label: '年繳保費（HK$）', type: 'number', required: false, min: 0, step: 1, defaultValue: 0, hint: '💡 年繳請填此欄（將平攤至每月）' },
      { id: 'oncePremium', label: '一次性付款金額（HK$）', type: 'number', required: false, min: 0, step: 1, defaultValue: 0, hint: '💡 一次付款請填此欄（將平攤至總年期每月）' },
      { id: 'fundsAllocation', label: '關聯基金', type: 'funds-allocation', required: false, hint: '💡 基金保險用：設定每月供款按比例分配給哪些基金' },
    ],
    ui: {
      listColumns: ['name', 'policyHolderId', 'memberId', 'company', 'currentPeriodIndex'],
      cardFields: ['policyHolderId', 'memberId', 'firstStartYear', 'annualPremium'], primaryColumn: 'name',
      searchFields: ['name', 'company'],
      sortOptions: [{ value: 'name-asc', label: '名稱（A→Z）' }, { value: 'startYear-asc', label: '開始年（舊→新）' }, { value: 'startYear-desc', label: '開始年（新→舊）' }],
      defaultSort: 'name-asc', pageSize: LIMITS.DEFAULT_TABLE_PAGE_SIZE,
      canCreate: true, canEdit: true, canDelete: true,
      deleteConfirmText: (row) => `⚠️ 確定要刪除保單「${row.name}」嗎？\n\n這將會一併刪除所有相關的扣款紀錄與成員支出，此操作無法復原。`,
    },
    validate: (data, allRows, currentId) => {
      const policyType = data.type || 'normal';
      const paymentType = data.paymentType || PAYMENT_TYPES.ANNUAL;
      const mp = Number(data.monthlyPremium) || 0;
      const ap = Number(data.annualPremium) || 0;
      const op = Number(data.oncePremium) || 0;
      const pm = data.paymentMode || 'direct';

      if (policyType === 'fund_insurance') {
        if (mp <= 0) return { field: 'monthlyPremium', message: '基金保險請填寫月供金額（大於 0）' };
        const alloc = data.fundsAllocation || [];
        if (alloc.length === 0) return { field: 'fundsAllocation', message: '基金保險請至少設定 1 個關聯基金' };
        const total = alloc.reduce((s, a) => s + (Number(a.pct) || 0), 0);
        if (Math.abs(total - 100) > 0.01) return { field: 'fundsAllocation', message: `比例總和需為 100%（目前 ${total}%）` };
      }

      if (policyType === 'normal' || policyType === 'savings') {
        if (paymentType === PAYMENT_TYPES.ANNUAL && ap <= 0) return { field: 'annualPremium', message: '年繳保費需大於 0' };
        if (paymentType === PAYMENT_TYPES.MONTHLY && mp <= 0) return { field: 'monthlyPremium', message: '月繳金額需大於 0' };
        if (paymentType === PAYMENT_TYPES.ONCE && op <= 0) return { field: 'oncePremium', message: '一次性付款金額需大於 0' };
      }

      if (pm === 'advance' && !data.advanceHolderId) return { field: 'advanceHolderId', message: '代墊模式需指定代墊成員' };
      return null;
    },
    fromForm: (data) => {
      const policyType = data.type || 'normal';
      const paymentType = data.paymentType || PAYMENT_TYPES.ANNUAL;
      const pm = data.paymentMode || 'direct';
      const sy = Number(data.firstStartYear);
      const sm = String(data.firstStartMonth || '01').padStart(2, '0');
      const cp = Number(data.currentPeriodIndex);
      const ty = Number(data.totalPolicyYears) || 1;
      const mp = Math.round(Number(data.monthlyPremium) || 0);
      const ap = Math.round(Number(data.annualPremium) || 0);
      const op = Math.round(Number(data.oncePremium) || 0);

      const monthlyExpense = calcMonthlyExpense(data);

      let annualPremium = 0;
      if (policyType === 'fund_insurance') {
        annualPremium = ap;
      } else if (paymentType === PAYMENT_TYPES.ANNUAL) {
        annualPremium = ap;
      } else if (paymentType === PAYMENT_TYPES.MONTHLY) {
        annualPremium = mp * 12;
      } else if (paymentType === PAYMENT_TYPES.ONCE) {
        annualPremium = Math.round(op / ty);
      }

      let totalPremium = 0;
      if (policyType === 'fund_insurance') {
        totalPremium = mp * 12 * ty;
      } else if (paymentType === PAYMENT_TYPES.ANNUAL) {
        totalPremium = ap * ty;
      } else if (paymentType === PAYMENT_TYPES.MONTHLY) {
        totalPremium = mp * 12 * ty;
      } else if (paymentType === PAYMENT_TYPES.ONCE) {
        totalPremium = op;
      }

      const range = _getPeriodRange(sy, sm, cp);
      const periods = {};
      periods[String(cp)] = {
        periodIndex: cp,
        startYear: range.startY,
        startMonth: range.startM,
        annualPremium,
        monthlyAverage: monthlyExpense,
      };

      return {
        type: policyType, memberId: data.memberId, policyHolderId: data.policyHolderId || data.memberId,
        name: data.name, company: data.company || '', paymentType, paymentMode: pm,
        advanceHolderId: pm === 'advance' ? (data.advanceHolderId || '') : '',
        firstStartYear: sy, firstStartMonth: sm, totalPolicyYears: ty, totalPolicyPeriods: ty * 12,
        totalPremium, currentPeriodIndex: cp, account: data.account || '',
        monthlyPremium: (policyType === 'fund_insurance' || paymentType === PAYMENT_TYPES.MONTHLY) ? mp : 0,
        annualPremium,
        oncePremium: paymentType === PAYMENT_TYPES.ONCE ? op : 0,
        fundsAllocation: policyType === 'fund_insurance' ? (data.fundsAllocation || []) : [],
        periods,
      };
    },
    toForm: (row) => {
      const cur = (row.periods || {})[String(row.currentPeriodIndex || 1)];
      return {
        ...row,
        annualPremium: row.annualPremium || (cur ? cur.annualPremium : 0),
        monthlyPremium: row.monthlyPremium || 0,
        oncePremium: row.oncePremium || 0,
        paymentMode: row.paymentMode || 'direct',
        advanceHolderId: row.advanceHolderId || '',
        fundsAllocation: row.fundsAllocation || [],
      };
    },
  },

  [ENTITY_KEYS.FUND]: {
    key: ENTITY_KEYS.FUND, label: '基金', labelPlural: '基金', icon: 'line-chart',
    codePrefix: 'FUND',
    db: { create: addFund, update: updateFund, remove: removeFund, delete: null },
    fields: [
      { id: 'name', label: '基金名稱', type: 'text', required: true, maxlength: 60, placeholder: '例如：富達環球股票基金' },
      { id: 'type', label: '基金類型', type: 'select', required: true, includeEmpty: false, options: [
        { value: 'standalone', label: '獨立基金' },
        { value: 'insurance', label: '保險基金（掛於保單）' },
      ], defaultValue: 'standalone' },
      { id: 'cost', label: '投入成本（HK$）', type: 'number', required: false, min: 0, step: 1, defaultValue: 0, hint: '💡 基金保險：此欄由系統自動累積（月供 × 已供款期數）' },
      { id: 'currentValue', label: '現時價值（HK$）', type: 'number', required: false, min: 0, step: 1, defaultValue: 0 },
      { id: 'units', label: '持有單位數（可選）', type: 'number-plain', required: false, min: 0, step: 0.0001, defaultValue: 0 },
      { id: 'initialYear', label: '開始年份', type: 'number-plain', required: false, min: 2000, max: 2100 },
      { id: 'initialMonth', label: '開始月份', type: 'select', required: false, optionsSource: 'months' },
      { id: 'note', label: '備註（可選）', type: 'text', required: false, maxlength: 60 },
    ],
    ui: {
      listColumns: ['name', 'cost', 'currentValue', 'units'], cardFields: ['cost', 'currentValue', 'units'],
      primaryColumn: 'name', searchFields: ['name'],
      sortOptions: [{ value: 'name-asc', label: '名稱（A→Z）' }, { value: 'value-desc', label: '現值（大→小）' }],
      defaultSort: 'name-asc', pageSize: LIMITS.DEFAULT_TABLE_PAGE_SIZE,
      canCreate: true, canEdit: true, canDelete: true,
      deleteConfirmText: (row) => `確定要刪除基金「${row.name}」嗎？`,
    },
  },

  [ENTITY_KEYS.CATEGORY]: {
    key: ENTITY_KEYS.CATEGORY, label: '支出類別', labelPlural: '支出類別', icon: 'tags',
    codePrefix: 'CAT',
    db: { create: addCategory, update: updateCategory, remove: removeCategory, delete: null },
    fields: [
      { id: 'name', label: '類別名稱', type: 'text', required: true, maxlength: 20, placeholder: '例如：醫療類' },
      { id: 'shortCode', label: '子類短碼', type: 'text', required: false, maxlength: 5, placeholder: '例如：MED' },
      { id: 'order', label: '排序', type: 'number-plain', min: 0, defaultValue: 0 },
    ],
    ui: {
      listColumns: ['name', 'order'], cardFields: ['order'], primaryColumn: 'name', searchFields: ['name'],
      sortOptions: [{ value: 'order-asc', label: '排序（小→大）' }, { value: 'name-asc', label: '名稱（A→Z）' }],
      defaultSort: 'order-asc', pageSize: LIMITS.DEFAULT_TABLE_PAGE_SIZE,
      canCreate: true, canEdit: true, canDelete: true,
      deleteConfirmText: (row) => `確定要刪除類別「${row.name}」嗎？`,
    },
    validate: (data, allRows, currentId) => {
      const name = (data.name || '').trim();
      if (allRows && allRows.some((c) => c.id !== currentId && c.name === name)) return { field: 'name', message: '此類別名稱已存在' };
      return null;
    },
  },

  [ENTITY_KEYS.ITEM]: {
    key: ENTITY_KEYS.ITEM, label: '支出項目', labelPlural: '支出項目', icon: 'list',
    codePrefix: 'ITEM',
    db: { create: addItem, update: updateItem, remove: removeItem, delete: null },
    fields: [
      { id: 'categoryId', label: '所屬類別', type: 'select', required: true, optionsSource: 'categories', plusEntity: 'category' },
      { id: 'name', label: '項目名稱', type: 'text', required: true, maxlength: 30, placeholder: '例如：看病-濕疹' },
    ],
    ui: {
      listColumns: ['name', 'categoryId'], cardFields: ['categoryId'], primaryColumn: 'name', searchFields: ['name'],
      sortOptions: [{ value: 'name-asc', label: '名稱（A→Z）' }],
      defaultSort: 'name-asc', pageSize: LIMITS.DEFAULT_TABLE_PAGE_SIZE,
      canCreate: true, canEdit: true, canDelete: true,
      deleteConfirmText: (row) => `確定要刪除項目「${row.name}」嗎？`,
    },
  },

  [ENTITY_KEYS.PAYMENT]: {
    key: ENTITY_KEYS.PAYMENT, label: '支付方式', labelPlural: '支付方式', icon: 'credit-card',
    codePrefix: 'PAY',
    db: { create: addPaymentMethod, update: updatePaymentMethod, remove: removePaymentMethod, delete: null },
    fields: [
      { id: 'name', label: '支付方式名稱', type: 'text', required: true, maxlength: 20, placeholder: '例如：現金、中銀' },
      { id: 'order', label: '排序', type: 'number-plain', min: 0, defaultValue: 0 },
    ],
    ui: {
      listColumns: ['name', 'order'], cardFields: ['order'], primaryColumn: 'name', searchFields: ['name'],
      sortOptions: [{ value: 'order-asc', label: '排序（小→大）' }, { value: 'name-asc', label: '名稱（A→Z）' }],
      defaultSort: 'order-asc', pageSize: LIMITS.DEFAULT_TABLE_PAGE_SIZE,
      canCreate: true, canEdit: true, canDelete: true,
      deleteConfirmText: (row) => `確定要刪除支付方式「${row.name}」嗎？`,
    },
    validate: (data, allRows, currentId) => {
      const name = (data.name || '').trim();
      if (allRows && allRows.some((p) => p.id !== currentId && p.name === name)) return { field: 'name', message: '此名稱已存在' };
      return null;
    },
  },

  [ENTITY_KEYS.STATUS]: {
    key: ENTITY_KEYS.STATUS, label: '狀態', labelPlural: '狀態', icon: 'tag',
    db: { create: addStatus, update: updateStatus, remove: removeStatus, delete: null },
    fields: [
      { id: 'name', label: '狀態名稱', type: 'text', required: true, maxlength: 20, placeholder: '例如：未處理、已還款' },
      { id: 'category', label: '所屬類別', type: 'select', required: true, optionsSource: 'statusCategories' },
      { id: 'isDone', label: '是否為「已完成」', type: 'select', required: true, optionsSource: 'booleanOptions', defaultValue: 'false' },
      { id: 'order', label: '排序', type: 'number-plain', min: 0, defaultValue: 0 },
    ],
    ui: {
      listColumns: ['name', 'category', 'isDone', 'order'], cardFields: ['category', 'isDone', 'order'],
      primaryColumn: 'name', searchFields: ['name'],
      sortOptions: [{ value: 'order-asc', label: '排序（小→大）' }, { value: 'category-asc', label: '類別' }],
      defaultSort: 'order-asc', pageSize: LIMITS.DEFAULT_TABLE_PAGE_SIZE,
      canCreate: true, canEdit: true, canDelete: true,
      deleteConfirmText: (row) => `確定要刪除狀態「${row.name}」嗎？`,
    },
    toForm: (row) => ({ ...row, isDone: row.isDone ? 'true' : 'false' }),
    fromForm: (data) => ({ ...data, isDone: data.isDone === 'true' }),
    validate: (data, allRows, currentId) => {
      const name = (data.name || '').trim();
      if (allRows && allRows.some((s) => s.id !== currentId && s.name === name)) return { field: 'name', message: '此狀態名稱已存在' };
      return null;
    },
  },
};

export function getEntityDef(key) { return ENTITY_DEFS[key] || null; }
export function getAllEntityKeys() { return Object.keys(ENTITY_DEFS); }
export function getAllEntityDefs() { return { ...ENTITY_DEFS }; }
export function getEntityFields(key) { const d = getEntityDef(key); return d ? d.fields : []; }
export function getEntityFormFields(key) { return getEntityFields(key).filter((f) => f.type !== 'hidden'); }
export function getEntityUi(key) { const d = getEntityDef(key); return d ? d.ui : null; }
export function getEntityCodePrefix(key) { const d = getEntityDef(key); return d ? d.codePrefix : null; }
export function resolveFieldDefault(field) {
  if (field.defaultValue == null) return '';
  if (typeof field.defaultValue === 'function') return field.defaultValue();
  return field.defaultValue;
}
export { calcMonthlyExpense, PAYMENT_TYPES, POLICY_SUBTYPES };

function _getPeriodRange(fy, fm, pi) {
  const d = new Date(Number(fy), Number(fm) - 1, 1);
  d.setMonth(d.getMonth() + (pi - 1) * 12);
  return { startY: d.getFullYear(), startM: String(d.getMonth() + 1).padStart(2, '0') };
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/entity/entity-definitions.js
   Version: 1791901900
   Batch: B34-4
   Writer: A20261013-004#102
   ═══════════════════════════════════════════ */```

---

## 📄 js/entity/entity-funds-allocation.js

```
// entity-funds-allocation.js — 保單關聯基金編輯（v103.0.22）
// v103.0.22（B25-2）：
//   ✅ R1 修正：選擇基金 Modal 走 ui/modal（openModal + closeModal）
//   ✅ 支援 defaultYear / defaultMonth（從保單起始年月）
//   ✅ 總和更新改用 data-role 穩定選擇器
//   ✅ 所有 button 加 type="button"
// ============================================
import { esc } from '../lib/dom.js';
import { showToast } from '../ui/toast.js';
import { openModal, closeModal } from '../ui/modal.js';
import { getFundsOnce } from '../core/db.js';

const SELECT_MODAL_ID = 'efa-select-fund-modal';

export function renderFundsAllocationField(root, initialAlloc, policyType, opts = {}) {
  const defaultYear = opts.defaultYear || new Date().getFullYear();
  const defaultMonth = opts.defaultMonth || '01';

  let alloc = Array.isArray(initialAlloc) ? initialAlloc.map((a) => ({ ...a })) : [];
  let fundsList = [];

  const loadFunds = async () => {
    try { fundsList = await getFundsOnce(); } catch (e) { fundsList = []; }
    paint();
  };

  const paint = () => {
    const isFundIns = policyType === 'fund_insurance';
    const total = alloc.reduce((s, a) => s + (Number(a.pct) || 0), 0);
    const totalClass = Math.abs(total - 100) < 0.01 ? 'text-emerald' : 'text-orange';

    root.innerHTML = `
      <div class="glass-card" style="padding:12px;background:rgba(8,11,17,0.4);">
        ${!isFundIns ? '<div class="banner mb-12" style="margin:0;">ℹ️ 只有「基金保險」才需設定關聯基金</div>' : ''}
        <div id="efa-list" style="margin-bottom:12px;"></div>
        <div class="flex flex-between items-center" style="margin-bottom:8px;">
          <span style="font-size:12px;color:var(--text-muted);">比例總和：<span data-role="total" class="${totalClass}" style="font-weight:700;">${total}%</span></span>
          <button type="button" class="btn btn-sm btn-ghost" data-action="add-alloc">
            <i data-lucide="plus" style="width:14px;height:14px;"></i> 新增基金
          </button>
        </div>
      </div>
    `;

    const listEl = root.querySelector('#efa-list');
    if (alloc.length === 0) {
      listEl.innerHTML = '<div class="text-muted" style="font-size:12px;text-align:center;padding:12px;">尚未設定</div>';
    } else {
      listEl.innerHTML = alloc.map((a, i) => {
        const fund = fundsList.find((f) => f.id === a.fundId);
        const fundName = fund ? fund.name : a.fundId;
        return `
          <div style="display:flex;align-items:center;gap:8px;padding:8px;border-bottom:1px solid rgba(255,255,255,0.04);" data-index="${i}">
            <span style="flex:1;font-size:13px;min-width:0;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;">${esc(fundName)}</span>
            <input type="number" data-role="pct" value="${a.pct || 0}" min="0" max="100" step="1"
                   style="width:70px;padding:4px 8px;font-size:12px;background:rgba(8,11,17,0.6);border:1px solid var(--glass-border);border-radius:var(--radius-sm);color:var(--text-primary);text-align:right;">
            <span style="font-size:11px;color:var(--text-muted);">%</span>
            <button type="button" class="btn btn-sm btn-danger" data-action="del-alloc" style="padding:2px 6px;">
              <i data-lucide="trash-2" style="width:12px;height:12px;"></i>
            </button>
          </div>
        `;
      }).join('');
    }

    if (window.lucide) window.lucide.createIcons();
  };

  root.addEventListener('input', (e) => {
    const pctInput = e.target.closest('input[data-role="pct"]');
    if (!pctInput) return;
    const row = pctInput.closest('[data-index]');
    const i = Number(row.dataset.index);
    alloc[i].pct = Number(pctInput.value) || 0;
    const total = alloc.reduce((s, a) => s + (Number(a.pct) || 0), 0);
    const totalEl = root.querySelector('[data-role="total"]');
    if (totalEl) {
      totalEl.textContent = `${total}%`;
      totalEl.className = Math.abs(total - 100) < 0.01 ? 'text-emerald' : 'text-orange';
      totalEl.style.fontWeight = '700';
    }
  });

  root.addEventListener('click', async (e) => {
    const btn = e.target.closest('button[data-action]');
    if (!btn) return;
    if (btn.dataset.action === 'add-alloc') {
      await _openSelectFundModal();
    } else if (btn.dataset.action === 'del-alloc') {
      const row = btn.closest('[data-index]');
      const i = Number(row.dataset.index);
      alloc.splice(i, 1);
      paint();
    }
  });

  const _openSelectFundModal = async () => {
    if (fundsList.length === 0) {
      showToast('尚無基金，請先至基金頁新增', 'warning', 4000);
      return;
    }
    const usedIds = new Set(alloc.map((a) => a.fundId));
    const available = fundsList.filter((f) => !usedIds.has(f.id));
    if (available.length === 0) {
      showToast('所有基金都已加入', 'info');
      return;
    }

    document.getElementById(SELECT_MODAL_ID)?.remove();

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = SELECT_MODAL_ID;
    overlay.innerHTML = `
      <div class="modal" style="max-width:400px;">
        <h2 class="modal-title">選擇基金</h2>
        <div style="max-height:50vh;overflow-y:auto;">
          ${available.map((f) => `
            <button type="button" class="btn btn-ghost" data-fund-id="${esc(f.id)}"
                    style="width:100%;justify-content:flex-start;margin-bottom:6px;">
              ${esc(f.name)}
            </button>
          `).join('')}
        </div>
        <div class="modal-actions" style="margin-top:16px;">
          <button type="button" class="btn btn-ghost" data-action="close">取消</button>
        </div>
      </div>
    `;
    document.body.appendChild(overlay);

    overlay.addEventListener('click', (e) => {
      if (e.target === overlay || e.target.closest('button[data-action="close"]')) {
        closeModal(SELECT_MODAL_ID);
        overlay.remove();
        return;
      }
      const fundBtn = e.target.closest('button[data-fund-id]');
      if (fundBtn) {
        alloc.push({
          fundId: fundBtn.dataset.fundId,
          pct: 0,
          fromYear: defaultYear,
          fromMonth: defaultMonth,
        });
        closeModal(SELECT_MODAL_ID);
        overlay.remove();
        paint();
      }
    });

    openModal(SELECT_MODAL_ID);
    if (window.lucide) window.lucide.createIcons();
  };

  paint();
  loadFunds();

  return () => alloc.map((a) => ({
    fundId: a.fundId,
    pct: Number(a.pct) || 0,
    fromYear: a.fromYear || defaultYear,
    fromMonth: a.fromMonth || defaultMonth,
  }));
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/entity/entity-funds-allocation.js
   Version: v103.0.22
   Batch: B25-2
   ═══════════════════════════════════════════ */```

---

## 📄 js/entity/entity-helpers.js

```
// ============================================
// entity-helpers.js — 通用實體 CRUD 輔助（v103.0.25）
// 位置：js/entity/entity-helpers.js
// ============================================
// v103.0.25（B34-4）：
//   ✅ [修正] 補回遺漏的 import：getMembersOnce / getBanksOnce
// v103.0.24（B34-4）：
//   ✅ createEntity 回傳 { id, code }
// ============================================

import {
  listenMembers, getMembersOnce,
  listenBanks, getBanksOnce,
  listenFunds,
  listenInsurancePolicies, getInsurancePoliciesOnce,
  listenInsuranceCompanies, getInsuranceCompaniesOnce,
  listenPaymentMethods, getPaymentMethodsOnce,
  listenCategories, getCategoriesOnce,
  listenItems, getItemsOnce,
  listenStatuses,
  listenBankAccounts, getBankAccountsOnce,
  addMemberAdvance,
  updateInsurancePolicy,
  updateEntityStatus as dbUpdateEntityStatus,
} from '../core/db.js';

import { api } from '../core/api.js';
import { getEntityDef, resolveFieldDefault } from './entity-definitions.js';
import { generateCodeFor } from '../lib/code-registry.js';
import { getOptions, getStatusesByCategory } from '../config/app-config.js';
import { sortMembers } from '../core/utils.js';
import { esc } from '../lib/dom.js';
import {
  getStatusBadgeClass,
  RESERVED_IDS,
} from '../config/constants.js';

/* ============================================
   1. CRUD（全走 API）
   ============================================ */

export async function createEntity(entityKey, data, allRows = []) {
  const def = getEntityDef(entityKey);
  if (!def) throw new Error(`未知的實體：${entityKey}`);

  let payload = def.fromForm ? def.fromForm(data) : { ...data };

  if (def.validate) {
    const err = def.validate(payload, allRows, null);
    if (err) throw new Error(err.message);
  }

  if (def.beforeCreate) {
    payload = def.beforeCreate(payload, allRows);
  }

  let code = null;
  if (def.codePrefix) {
    const r = await generateCodeFor(entityKey, payload);
    code = r.code;
  }

  const needsAdvance = entityKey === 'policy'
    && payload.paymentMode === 'advance'
    && payload.advanceHolderId
    && Number(payload.totalPremium) > 0;

  const result = await api.entityCreate({
    entityType: entityKey,
    action: 'create',
    code,
    name: payload.name,
    extra: _extractExtra(entityKey, payload),
  });

  if (!result?.id) throw new Error('新增失敗');

  if (needsAdvance) {
    try {
      const advanceId = await addMemberAdvance(payload.advanceHolderId, {
        policyId: result.id,
        totalAmount: Number(payload.totalPremium) || 0,
        remainingAmount: Number(payload.totalPremium) || 0,
        startYear: String(payload.firstStartYear || ''),
        startMonth: String(payload.firstStartMonth || '01').padStart(2, '0'),
        note: `代墊：${payload.name || ''}`,
      });
      if (advanceId) await updateInsurancePolicy(result.id, { advanceId });
    } catch (err) {
      console.warn('[entity-helpers] 建立代墊記錄失敗：', err);
    }
  }

  return { id: result.id, code: result.code || code };
}

export async function updateEntity(entityKey, id, data, allRows = []) {
  const def = getEntityDef(entityKey);
  if (!def) throw new Error(`未知的實體：${entityKey}`);
  if (!id) throw new Error('缺少 id');

  let payload = def.fromForm ? def.fromForm(data) : { ...data };

  if (def.validate) {
    const err = def.validate(payload, allRows, id);
    if (err) throw new Error(err.message);
  }

  await api.entityCreate({
    entityType: entityKey,
    action: 'update',
    id,
    name: payload.name,
    extra: _extractExtra(entityKey, payload),
  });

  const existingRow = (allRows || []).find((r) => r.id === id);
  return { id, code: existingRow?.code || null };
}

export async function removeEntity(entityKey, id) {
  if (!id) throw new Error('缺少 id');
  await api.entityCreate({
    entityType: entityKey,
    action: 'delete',
    id,
  });
}

export async function deleteEntity(entityKey, id, ...args) {
  const def = getEntityDef(entityKey);
  if (def?.db?.delete) {
    return def.db.delete(id, ...args);
  }
  return removeEntity(entityKey, id);
}

function _extractExtra(entityKey, payload) {
  const extra = {};
  const commonFields = ['name', 'code', 'createdAt', 'updatedAt'];
  Object.entries(payload || {}).forEach(([k, v]) => {
    if (!commonFields.includes(k)) extra[k] = v;
  });
  return extra;
}

/* ============================================
   2. 動態選項
   ============================================ */

export async function getDynamicOptions(source, context = {}) {
  if (!source) return [];

  try {
    switch (source) {
      case 'members':
      case 'membersWithShared': {
        const list = await getMembersOnce();
        return sortMembers(list).map((m) => ({ value: m.id, label: m.name }));
      }

      case 'categories': {
        const list = await getCategoriesOnce();
        return list.map((c) => ({ value: c.id, label: c.name }));
      }

      case 'items': {
        const list = await getItemsOnce();
        const filtered = context.categoryId
          ? list.filter((i) => i.categoryId === context.categoryId)
          : list;
        return filtered.map((i) => ({ value: i.id, label: i.name }));
      }

      case 'payments': {
        const list = await getPaymentMethodsOnce();
        return list.map((p) => ({ value: p.id, label: p.name }));
      }

      case 'companies': {
        const list = await getInsuranceCompaniesOnce();
        return list.map((c) => ({ value: c.name, label: c.name }));
      }

      case 'banks': {
        const list = await getBanksOnce();
        return list.map((b) => ({ value: b.id, label: b.name }));
      }

      case 'bankAccounts': {
        const list = await getBankAccountsOnce();
        return list.map((b) => ({ value: b.id, label: b.name }));
      }

      case 'statuses:personal':
        return _statusOptions('personal');
      case 'statuses:fixed':
        return _statusOptions('fixed');
      case 'statuses:insurance':
        return _statusOptions('insurance');

      case 'memberRoles':
        return getOptions('memberRoles');
      case 'policyTypes':
        return getOptions('policyTypes');
      case 'insurancePaymentTypes':
        return getOptions('insurancePaymentTypes');
      case 'paymentModes':
        return getOptions('paymentModes');
      case 'categoryOrder':
        return getOptions('categoryOrder');

      case 'statusCategories':
        return [
          { value: 'personal',  label: '個人支出' },
          { value: 'fixed',     label: '固定支出' },
          { value: 'insurance', label: '保險' },
        ];

      case 'booleanOptions':
        return [
          { value: 'false', label: '否（未處理類）' },
          { value: 'true',  label: '是（已完成類）' },
        ];

      case 'months': {
        const opts = [];
        for (let m = 1; m <= 12; m++) {
          opts.push({ value: String(m).padStart(2, '0'), label: `${m} 月` });
        }
        return opts;
      }

      default:
        console.warn(`[entity-helpers] 未知的 optionsSource：${source}`);
        return [];
    }
  } catch (err) {
    console.error(`[entity-helpers] 載入動態選項失敗 (${source})：`, err);
    return [];
  }
}

function _statusOptions(category) {
  const list = getStatusesByCategory(category);
  return list.map((s) => ({ value: s.name, label: s.name }));
}

/* ============================================
   3. 跨來源改狀態
   ============================================ */

export async function updateEntityStatus(source, row, newStatus) {
  const isDone = _isDoneStatus(newStatus, source);
  return dbUpdateEntityStatus(source, row, newStatus, isDone);
}

export function isDoneStatus(statusName, category) {
  return _isDoneStatus(statusName, category);
}

function _isDoneStatus(statusName, category) {
  if (!statusName) return false;
  try {
    const statuses = getStatusesByCategory(category);
    const found = statuses.find((s) => s.name === statusName);
    if (found) return !!found.isDone;
  } catch (e) {}
  return statusName.startsWith('已');
}

/* ============================================
   4. 狀態 badge 渲染
   ============================================ */

export function renderStatusBadge(statusName, category) {
  const isDone = _isDoneStatus(statusName, category);
  const isSkipped = statusName === '不適用';
  const cls = getStatusBadgeClass(isDone, isSkipped);
  const fallback = _getStatusFallback(category);
  return `<span class="badge ${cls}">${esc(statusName || fallback)}</span>`;
}

function _getStatusFallback(category) {
  switch (category) {
    case 'fixed':     return '未付款';
    case 'insurance': return '未扣款';
    case 'personal':
    default:          return '未處理';
  }
}

/* ============================================
   5. 資料轉換輔助
   ============================================ */

export function buildPayload(entityKey, formData) {
  const def = getEntityDef(entityKey);
  if (!def) throw new Error(`未知的實體：${entityKey}`);
  return def.fromForm ? def.fromForm(formData) : { ...formData };
}

export function buildInitialData(entityKey, row) {
  const def = getEntityDef(entityKey);
  if (!def) throw new Error(`未知的實體：${entityKey}`);
  return def.toForm ? def.toForm(row) : { ...row };
}

export function getFieldDefaults(entityKey) {
  const def = getEntityDef(entityKey);
  if (!def) return {};
  const result = {};
  def.fields.forEach((f) => {
    if (f.defaultValue != null) {
      result[f.id] = resolveFieldDefault(f);
    }
  });
  return result;
}

/* ============================================
   6. 監聽輔助
   ============================================ */

export function listenEntity(entityKey, cb, err) {
  switch (entityKey) {
    case 'member':      return listenMembers(cb, err);
    case 'bank':        return listenBanks(cb, err);
    case 'category':    return listenCategories(cb, err);
    case 'item':        return listenItems(cb, err);
    case 'payment':     return listenPaymentMethods(cb, err);
    case 'status':      return listenStatuses(cb, err);
    case 'company':     return listenInsuranceCompanies(cb, err);
    case 'policy':      return listenInsurancePolicies(cb, err);
    case 'fund':        return listenFunds(cb, err);
    case 'bankAccount': return listenBankAccounts(cb, err);
    default:
      console.warn(`[entity-helpers] listenEntity 不支援：${entityKey}`);
      return () => {};
  }
}

/* ============================================
   7. 便利函式
   ============================================ */

export async function getPoliciesOnce() {
  return getInsurancePoliciesOnce();
}

export function isReservedId(id) {
  return id === RESERVED_IDS.EXTRA_INCOME || id === RESERVED_IDS.SHARED_MEMBER;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/entity/entity-helpers.js
   Version: 1791902740
   Batch: B34-4
   Writer: A20261013-004#131
   ═══════════════════════════════════════════ */```

---

## 📄 js/entity/entity-modal.js

```
// entity-modal.js — 通用實體編輯 Modal（v103.0.24）
// v103.0.24（B34-4）：
//   ✅ createEntity / updateEntity 回傳 { id, code }（D-080）
//   ✅ onSuccess 回傳 { id, code, data }（統一格式）
// v103.0.23（B34-4）：
//   ✅ 新增後回傳新 code 給呼叫端
// v103.0.22（B25-2）：
//   ✅ 傳 defaultYear / defaultMonth 給 funds-allocation 欄位
// ============================================
import { buildForm } from '../ui/form-builder.js';
import { openModal, closeModal } from '../ui/modal.js';
import { showToast } from '../ui/toast.js';
import {
  createEntity,
  updateEntity,
  buildInitialData,
  getFieldDefaults,
} from './entity-helpers.js';
import { getEntityDef } from './entity-definitions.js';
import { renderFundsAllocationField } from './entity-funds-allocation.js';
import { esc } from '../lib/dom.js';

const MODAL_ID = 'entity-modal-root';
const FORM_ROOT_ID = 'entity-modal-form-root';
const TITLE_ID = 'entity-modal-title';
const ALLOC_ROOT_ID = 'em-funds-allocation-root';

export async function openEntityModal(options) {
  const {
    entity, mode = 'add', id = null, initialData = null,
    allRows = [], onSuccess, onCancel,
  } = options;
  const def = getEntityDef(entity);
  if (!def) { showToast(`未知的實體：${entity}`, 'error'); return null; }

  _ensureModal();
  const titleEl = document.getElementById(TITLE_ID);
  if (titleEl) titleEl.textContent = `${mode === 'edit' ? '編輯' : '新增'}${def.label}`;

  let initData = initialData;
  if (mode === 'edit' && id && !initData) {
    const row = allRows.find((r) => r.id === id);
    if (row) initData = buildInitialData(entity, row);
  }
  if (mode === 'add' && !initData) initData = getFieldDefaults(entity);

  const fields = def.fields.map((f) => {
    if (f.type === 'funds-allocation') {
      return {
        type: 'custom',
        id: f.id,
        html: `<div class="field">
          <label class="field-label">${esc(f.label || '')}</label>
          <div id="${ALLOC_ROOT_ID}"></div>
          ${f.hint ? `<div class="glass-card-hint" style="margin-top:4px;">${esc(f.hint)}</div>` : ''}
        </div>`,
      };
    }
    return f;
  });

  const formApi = buildForm({
    containerId: FORM_ROOT_ID,
    idPrefix: 'em-',
    fields,
    submitText: mode === 'edit' ? '儲存' : `新增${def.label}`,
    showCancel: true,
    cancelText: '取消',
    initialData: initData,
    beforeSubmit: (data) => {
      if (def.fields.some((f) => f.type === 'funds-allocation')) {
        const allocRoot = document.getElementById(ALLOC_ROOT_ID);
        if (allocRoot && allocRoot.__getValue) data.fundsAllocation = allocRoot.__getValue();
      }
      if (def.validate) {
        const err = def.validate(data, allRows, id);
        if (err) return err;
      }
      return true;
    },
    onSubmit: async (data) => {
      try {
        let result;
        if (mode === 'edit' && id) {
          /* ✅ v2：updateEntity 回傳 { id, code } */
          const r = await updateEntity(entity, id, data, allRows);
          result = { id: r.id, code: r.code, data };
          showToast(`✅ 已更新${def.label}`, 'success');
        } else {
          /* ✅ v2：createEntity 回傳 { id, code } */
          const r = await createEntity(entity, data, allRows);
          result = { id: r.id, code: r.code, data };
          showToast(`✅ 已新增${def.label}`, 'success');
        }
        closeModal(MODAL_ID);
        if (typeof onSuccess === 'function') onSuccess(result);
      } catch (err) {
        console.error('[entity-modal] 提交失敗：', err);
        showToast('操作失敗：' + (err.message || err), 'error');
      }
    },
    onCancel: () => {
      closeModal(MODAL_ID);
      if (typeof onCancel === 'function') onCancel();
    },
  });

  /* 基金分配欄位 */
  if (def.fields.some((f) => f.type === 'funds-allocation')) {
    const allocRoot = document.getElementById(ALLOC_ROOT_ID);
    if (allocRoot) {
      allocRoot.__getValue = renderFundsAllocationField(
        allocRoot,
        initData?.fundsAllocation || [],
        initData?.type || 'normal',
        {
          defaultYear: initData?.firstStartYear || new Date().getFullYear(),
          defaultMonth: initData?.firstStartMonth || '01',
        }
      );
    }
  }

  openModal(MODAL_ID);
  setTimeout(() => {
    const first = document.querySelector(`#${FORM_ROOT_ID} input, #${FORM_ROOT_ID} select`);
    if (first) first.focus();
  }, 100);

  return formApi;
}

export function openEntityAddModal(entity, options = {}) {
  return openEntityModal({ entity, mode: 'add', ...options });
}

export function openEntityEditModal(entity, id, options = {}) {
  return openEntityModal({ entity, mode: 'edit', id, ...options });
}

export function closeEntityModal() { closeModal(MODAL_ID); }

function _ensureModal() {
  let overlay = document.getElementById(MODAL_ID);
  if (overlay) {
    const root = document.getElementById(FORM_ROOT_ID);
    if (root) root.innerHTML = '';
    return;
  }
  overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = MODAL_ID;
  overlay.innerHTML = `
    <div class="modal" style="max-width:560px;max-height:90vh;overflow-y:auto;">
      <h2 class="modal-title" id="${TITLE_ID}">編輯</h2>
      <div id="${FORM_ROOT_ID}"></div>
    </div>
  `;
  document.body.appendChild(overlay);
  overlay.addEventListener('click', (e) => {
    if (e.target === overlay) closeModal(MODAL_ID);
  });
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/entity/entity-modal.js
   Version: 1791901240
   Batch: B34-4
   Writer: A20261013-004#090
   ═══════════════════════════════════════════ */```

---

## 📄 js/entity/entity-resolvers.js

```
// ============================================
// entity-resolvers.js — Entity 欄位顯示 SSOT（v103.0.22）
// 位置：js/entity/entity-resolvers.js
// ============================================
// v103.0.22（B25-4）：
//   ✅ M2：抽出重複的 name / memberName resolver
// ============================================

import { esc } from '../lib/dom.js';
import { formatHKD, formatNumber } from '../lib/format.js';
import { resolveName } from '../config/entity-registry.js';

/* 🆕 v103.0.22：抽出重複的模式 */
const _name = (v) => esc(v || '—');
const _nameOrDash = (v) => v ? esc(resolveName('members', v) || v) : '—';
const _num = (v) => formatNumber(v);

export const RESOLVERS = {
  member: {
    name: _name,
    role: _name,
    order: _num,
  },

  bank: {
    name: _name,
    order: _num,
  },

  policy: {
    name: (v) => esc(v || '（未命名）'),
    company: _name,
    policyHolderId: _nameOrDash,
    memberId: _nameOrDash,
    firstStartYear: (v, r) => esc(`${v || ''}-${r.firstStartMonth || ''}`),
    annualPremium: (v) => `<span class="mono text-cyan">${formatHKD(v)}</span>`,
    totalPremium: (v) => `<span class="mono text-magenta">${formatHKD(v)}</span>`,
    monthlyPremium: (v) => `<span class="mono">${formatHKD(v)}</span>`,
    type: (v) => v === 'fund_insurance' ? '基金保險' : '普通保險',
    paymentMode: (v) => v === 'advance' ? '代墊' : '直接',
    paymentType: _name,
    currentPeriodIndex: (v) => `<span class="mono">${v || 1}</span>`,
    totalPolicyYears: (v) => `<span class="mono">${v || 0}</span>`,
  },

  fund: {
    name: (v) => esc(v || '（未命名）'),
    cost: (v) => `<span class="mono">${formatHKD(v)}</span>`,
    currentValue: (v) => `<span class="mono text-emerald">${formatHKD(v)}</span>`,
    units: (v) => `<span class="mono">${v || '—'}</span>`,
    note: _name,
  },

  category: {
    name: _name,
    order: _num,
  },

  item: {
    name: _name,
    categoryId: (v) => v ? `<span class="badge badge-info">${esc(resolveName('categories', v) || v)}</span>` : '—',
  },

  payment: {
    name: _name,
    order: _num,
  },

  status: {
    name: _name,
    category: (v) => {
      const map = { personal: '個人支出', fixed: '固定支出', insurance: '保險' };
      return esc(map[v] || v || '—');
    },
    isDone: (v) => v ? '<span class="badge badge-success">是</span>' : '<span class="badge badge-muted">否</span>',
    order: _num,
  },
};

export function getEntityResolvers(entityKey) {
  const map = {
    member: 'member',
    bank: 'bank',
    policy: 'policy',
    fund: 'fund',
    category: 'category',
    item: 'item',
    payment: 'payment',
    status: 'status',
  };
  const key = map[entityKey];
  return key ? (RESOLVERS[key] || {}) : {};
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/entity/entity-resolvers.js
   Version: v103.0.22
   Batch: B25-4
   ═══════════════════════════════════════════ */```

---

## 📄 js/layout/app-shell.js

```
// ============================================
// app-shell.js — App Shell 初始化（v103.0.9）
// 位置：js/layout/app-shell.js
// ============================================
// v103.0.9 修正：
//   ✅ [關鍵] initDebug 改為動態 import + try-catch
//      - 即使 debug.js 不存在，app-shell 也不會崩潰
// ============================================

import { renderSidebar, destroySidebar } from './sidebar.js';
import { renderNavbar, destroyNavbar } from './navbar.js';
import { requireLogin } from '../core/auth-guard.js';
import { initPWA } from '../core/pwa.js';
import { AppState } from '../core/state.js';
import {
  initAppConfig,
  watchPlatformDefaults,
  watchFamilySettings,
  disposeAppConfig,
} from '../config/app-config.js';
import {
  initAllRegistries,
  destroyAllRegistries,
} from '../lib/registry.js';

let _pageCleanups = [];
let _initialized = false;

export async function initApp({
  activeHref = '',
  title = '',
  needAuth = true,
  requireFamily = true,
} = {}) {
  if (_initialized) {
    console.warn('[app-shell] initApp 已被呼叫過，忽略重複呼叫');
    return null;
  }
  _initialized = true;

  /* ---------- 0. Debug（vConsole）安全載入 ---------- */
  try {
    const mod = await import('../core/debug.js');
    if (mod && typeof mod.initDebug === 'function') {
      mod.initDebug();
    }
  } catch (err) {
    // debug.js 不存在時，不影響主流程
    console.warn('[app-shell] debug 載入失敗（可忽略）：', err.message);
  }

  /* ---------- 1. PWA ---------- */
  try {
    initPWA();
  } catch (err) {
    console.warn('[app-shell] PWA 初始化失敗：', err);
  }

  /* ---------- 2. AppState ---------- */
  AppState.init();

  /* ---------- 3. 登入驗證 ---------- */
  let user = null;
  if (needAuth) {
    try {
      user = await requireLogin({ requireFamily });
    } catch (err) {
      console.error('[app-shell] 登入驗證失敗：', err);
      return null;
    }
    if (!user) return null;
  }

  /* ---------- 4. app-config ---------- */
  try {
    await initAppConfig(AppState.getFamilyId());
    watchPlatformDefaults();
    if (AppState.getFamilyId()) {
      watchFamilySettings();
    }
  } catch (err) {
    console.warn('[app-shell] app-config 載入失敗：', err);
  }

  /* ---------- 5. Registry ---------- */
  try {
    initAllRegistries();
  } catch (err) {
    console.warn('[app-shell] Registry 初始化失敗：', err);
  }

  /* ---------- 6. Sidebar ---------- */
  const sidebarRoot = document.getElementById('sidebar-root');
  if (sidebarRoot) {
    try {
      await renderSidebar('sidebar-root', activeHref);
      console.log('[app-shell] Sidebar 渲染完成');
    } catch (err) {
      console.error('[app-shell] Sidebar 渲染失敗：', err);
      // 顯示錯誤在側邊欄位置
      sidebarRoot.innerHTML = `
        <div style="padding:16px; color:#F43F5E; font-size:12px; font-family:monospace;">
          ⚠️ Sidebar 渲染失敗：<br>${err.message}
        </div>
      `;
    }
  }

  /* ---------- 7. Navbar ---------- */
  try {
    renderNavbar('navbar-root', title, activeHref);
  } catch (err) {
    console.error('[app-shell] Navbar 渲染失敗：', err);
  }

  /* ---------- 8. Icons ---------- */
  if (window.lucide && typeof window.lucide.createIcons === 'function') {
    window.lucide.createIcons();
  }

  return user;
}

export function registerPageCleanup(fn) {
  if (typeof fn === 'function') {
    _pageCleanups.push(fn);
  }
}

export function destroyApp() {
  _pageCleanups.forEach((fn) => {
    try { fn(); } catch (e) {
      console.error('[app-shell] cleanup error:', e);
    }
  });
  _pageCleanups = [];

  try { destroySidebar(); } catch (e) { /* noop */ }
  try { destroyNavbar(); } catch (e) { /* noop */ }
  try { disposeAppConfig(); } catch (e) { /* noop */ }
  try { destroyAllRegistries(); } catch (e) { /* noop */ }

  AppState.destroy();
  _initialized = false;
}```

---

## 📄 js/layout/navbar.js

```
// ============================================
// navbar.js — 頂部導覽列（v103.0.42）
// 位置：js/layout/navbar.js
// ============================================
// v103.0.42（B30-3b）：
//   ✅ 加 DB 標籤（顯示當前連哪個 DB）
//   ✅ 加主題切換按鈕
// ============================================

import { AppState } from '../core/state.js';
import { getDisplayName } from '../core/auth.js';
import { ROUTES, ROLES, DB_TARGET, getDbLabel } from '../config/constants.js';
import { esc } from '../lib/dom.js';
import { toggleSidebar } from './sidebar.js';

let _eventBound = false;
let _unsubscribeUserChange = null;
let _unsubscribeFamilyChange = null;
let _unsubscribeMemberAccountChange = null;
let _unsubscribeThemeChange = null;
let _currentActiveHref = '';
let _hamburgerHandler = null;
let _themeHandler = null;

export function renderNavbar(containerId = 'navbar-root', title = '', activeHref = '') {
  const root = document.getElementById(containerId);
  if (!root) return;

  _currentActiveHref = activeHref;
  root.classList.add('navbar');
  const isSuper = AppState.isSuperAdmin;

  const adminBtn = isSuper
    ? `<a href="${ROUTES.ADMIN}" class="btn btn-sm btn-ghost" title="平台管理" style="padding:6px 10px;">
         <i data-lucide="settings" style="width:16px;height:16px;"></i>
       </a>`
    : '';

  /* DB 標籤 */
  const dbClass = DB_TARGET === 'new' ? 'navbar-db-tag navbar-db-tag-new' : 'navbar-db-tag';
  const dbHtml = `<span class="${dbClass}" title="當前連線資料庫">DB: ${esc(getDbLabel())}</span>`;

  /* 主題按鈕 */
  const theme = AppState.getTheme();
  const themeIcon = theme === 'dark' ? 'sun' : 'moon';
  const themeHtml = `<button type="button" class="navbar-theme-btn" id="navbar-theme-btn" aria-label="切換主題" title="切換主題">
    <i data-lucide="${themeIcon}" style="width:16px;height:16px;"></i>
  </button>`;

  root.innerHTML = `
    <button type="button" class="hamburger" id="hamburger-btn" aria-label="切換選單">
      <i data-lucide="menu"></i>
    </button>
    <div class="navbar-title">${esc(title)}</div>
    ${adminBtn}
    <div class="navbar-right">
      ${dbHtml}
      ${themeHtml}
      <div class="navbar-user" id="navbar-user"></div>
    </div>
  `;

  _renderUserInfo();
  _renderThemeIcon();

  if (!_eventBound) {
    _eventBound = true;
    _bindGlobalEvents(root);
  }

  if (window.lucide) window.lucide.createIcons();
}

function _renderUserInfo() {
  const box = document.getElementById('navbar-user');
  if (!box) return;

  const user = AppState.currentUser;
  if (!user) { box.innerHTML = ''; return; }

  const displayName = AppState.getDisplayName() || getDisplayName(user);
  const familyName = AppState.getFamilyName();
  const role = AppState.getRole();
  const canInput = AppState.getCanInput();
  const familyTag = familyName ? ` · ${familyName}` : '';

  let roleBadge = '';
  if (role === ROLES.SUPERADMIN) {
    roleBadge = ' <span class="badge badge-magenta" style="font-size:10px;">超級管理員</span>';
  } else if (role === ROLES.OWNER) {
    roleBadge = ' <span class="badge badge-info" style="font-size:10px;">👑</span>';
  } else if (role === ROLES.MEMBER) {
    roleBadge = ' <span class="badge badge-muted" style="font-size:10px;">成員</span>';
  }

  const readonlyBadge = (!canInput && role !== ROLES.SUPERADMIN)
    ? ' <span class="badge badge-pending" style="font-size:10px;">唯讀</span>'
    : '';

  box.innerHTML = `
    <span class="mono navbar-user-text" style="font-size:12px; color:var(--text-muted);">
      👤 ${esc(displayName)}${esc(familyTag)}${roleBadge}${readonlyBadge}
    </span>
  `;
}

function _renderThemeIcon() {
  const btn = document.getElementById('navbar-theme-btn');
  if (!btn) return;
  const theme = AppState.getTheme();
  const icon = theme === 'dark' ? 'sun' : 'moon';
  btn.innerHTML = `<i data-lucide="${icon}" style="width:16px;height:16px;"></i>`;
  if (window.lucide) window.lucide.createIcons();
}

function _bindGlobalEvents(root) {
  _hamburgerHandler = (e) => {
    if (!e.target.closest('#hamburger-btn')) return;
    e.preventDefault();
    e.stopPropagation();
    toggleSidebar();
  };
  root.addEventListener('click', _hamburgerHandler);

  _themeHandler = (e) => {
    if (!e.target.closest('#navbar-theme-btn')) return;
    e.preventDefault();
    AppState.toggleTheme();
    _renderThemeIcon();
  };
  root.addEventListener('click', _themeHandler);

  if (_unsubscribeUserChange) { try { _unsubscribeUserChange(); } catch (err) { /* noop */ } }
  _unsubscribeUserChange = AppState.on('user-change', () => _renderUserInfo());

  if (_unsubscribeFamilyChange) { try { _unsubscribeFamilyChange(); } catch (err) { /* noop */ } }
  _unsubscribeFamilyChange = AppState.on('family-change', () => _renderUserInfo());

  if (_unsubscribeMemberAccountChange) { try { _unsubscribeMemberAccountChange(); } catch (err) { /* noop */ } }
  _unsubscribeMemberAccountChange = AppState.on('member-account-change', () => _renderUserInfo());

  if (_unsubscribeThemeChange) { try { _unsubscribeThemeChange(); } catch (err) { /* noop */ } }
  _unsubscribeThemeChange = AppState.on('theme-change', () => _renderThemeIcon());
}

export function setNavbarTitle(title) {
  const el = document.querySelector('.navbar-title');
  if (el) el.textContent = title || '';
}

export function refreshNavbarUser() { _renderUserInfo(); }

export function destroyNavbar() {
  if (_unsubscribeUserChange) { try { _unsubscribeUserChange(); } catch (err) { /* noop */ } _unsubscribeUserChange = null; }
  if (_unsubscribeFamilyChange) { try { _unsubscribeFamilyChange(); } catch (err) { /* noop */ } _unsubscribeFamilyChange = null; }
  if (_unsubscribeMemberAccountChange) { try { _unsubscribeMemberAccountChange(); } catch (err) { /* noop */ } _unsubscribeMemberAccountChange = null; }
  if (_unsubscribeThemeChange) { try { _unsubscribeThemeChange(); } catch (err) { /* noop */ } _unsubscribeThemeChange = null; }

  const root = document.getElementById('navbar-root');
  if (root) {
    if (_hamburgerHandler) { try { root.removeEventListener('click', _hamburgerHandler); } catch (e) { /* noop */ } }
    if (_themeHandler) { try { root.removeEventListener('click', _themeHandler); } catch (e) { /* noop */ } }
  }
  _hamburgerHandler = null;
  _themeHandler = null;
  _eventBound = false;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/layout/navbar.js
   Version: v103.0.42
   Batch: B30-3b
   ═══════════════════════════════════════════ */```

---

## 📄 js/layout/sidebar.js

```
// ============================================
// sidebar.js — 左側導覽選單（v103.0.31）
// 位置：js/layout/sidebar.js
// ============================================
// v103.0.31（B26-4）：
//   ✅ 依角色過濾群組（superadmin 只看到平台管理 + 系統）
//   ✅ 用 getSidebarGroupsByRole 取代直接引用 SIDEBAR_GROUPS
// ============================================

import { SIDEBAR_GROUPS, STORAGE_KEYS, getSidebarGroupsByRole } from '../config/constants.js';
import { esc } from '../lib/dom.js';
import { AppState } from '../core/state.js';

/* ============================================
   Module 狀態
   ============================================ */
let _currentGroups = [];
let _filteredGroups = [];      /* 🆕 依角色過濾後的群組 */
let _activeHref = '';
let _eventsBound = false;
let _navEl = null;

/* ============================================
   主入口（同步函式）
   ============================================ */
export function renderSidebar(containerId = 'sidebar-root', activeHref = '') {
  const root = document.getElementById(containerId);
  if (!root) return;

  _activeHref = activeHref;

  root.classList.add('sidebar');

  root.innerHTML = `
    <div class="sidebar-header">
      <div class="sidebar-logo">◈ FAMILY.FIN</div>
    </div>
    <nav class="sidebar-nav" id="sidebar-nav-inner"></nav>
  `;

  _navEl = root.querySelector('#sidebar-nav-inner');

  /* 🆕 v103.0.31：依角色過濾 */
  const role = AppState.getRole();
  _filteredGroups = getSidebarGroupsByRole(role);

  _currentGroups = _filteredGroups.map((g) => ({
    ...g,
    isOpen: false,
  }));

  const openSet = _loadOpenGroupSet();
  _ensureGroupOpenFor(openSet, activeHref);
  _currentGroups.forEach((g) => {
    g.isOpen = openSet.has(g.key);
  });
  _saveOpenGroupSet(openSet);

  _bindGlobalEvents();
  _renderNav();

  try {
    const collapsed = localStorage.getItem(STORAGE_KEYS.SIDEBAR_COLLAPSED) === 'true';
    if (collapsed && window.innerWidth >= 640) root.classList.add('collapsed');
  } catch (e) { /* noop */ }
}

/* ============================================
   渲染
   ============================================ */
function _renderNav() {
  if (!_navEl) return;

  const html = _currentGroups.map((g) => _renderGroup(g)).join('');
  _navEl.innerHTML = html;

  if (window.lucide && typeof window.lucide.createIcons === 'function') {
    window.lucide.createIcons();
  }
}

function _renderGroup(group) {
  const arrowIcon = group.isOpen ? 'chevron-down' : 'chevron-right';
  const openClass = group.isOpen ? 'open' : '';

  const innerHtml = (group.items || []).map((item) => _renderNavItem(item)).join('');

  return `
    <div class="nav-group ${openClass}" data-group-key="${esc(group.key)}">
      <div class="nav-group-title collapsible ${openClass}" data-group-toggle="${esc(group.key)}">
        <span class="nav-group-label">
          <i data-lucide="${esc(group.icon || 'folder')}" class="nav-group-icon"></i>
          <span>${esc(group.label)}</span>
        </span>
        <i data-lucide="${arrowIcon}" class="nav-group-arrow"></i>
      </div>
      <div class="nav-group-body" style="display:${group.isOpen ? 'block' : 'none'};">
        ${innerHtml}
      </div>
    </div>
  `;
}

function _renderNavItem(item) {
  const isActive = item.href === _activeHref ? 'active' : '';
  return `
    <a class="nav-item ${isActive}" href="${esc(item.href)}">
      <i data-lucide="${esc(item.icon)}" class="nav-icon"></i>
      <span class="nav-label">${esc(item.label)}</span>
    </a>
  `;
}

/* ============================================
   事件綁定
   ============================================ */
function _bindGlobalEvents() {
  if (_eventsBound) return;
  _eventsBound = true;

  document.addEventListener('click', _globalClickHandler);
}

function _globalClickHandler(e) {
  const groupToggle = e.target.closest('[data-group-toggle]');
  if (groupToggle) {
    e.preventDefault();
    _toggleGroup(groupToggle.dataset.groupToggle);
    return;
  }

  if (e.target.classList.contains('sidebar-backdrop')) {
    closeMobileSidebar();
    return;
  }

  const navLink = e.target.closest('.sidebar .nav-item');
  if (navLink) {
    if (window.innerWidth < 640) {
      closeMobileSidebar();
    }
    return;
  }
}

function _toggleGroup(key) {
  const group = _currentGroups.find((g) => g.key === key);
  if (!group) return;

  group.isOpen = !group.isOpen;

  const openSet = new Set(
    _currentGroups.filter((g) => g.isOpen).map((g) => g.key)
  );
  _saveOpenGroupSet(openSet);

  const groupEl = _navEl.querySelector(`.nav-group[data-group-key="${key}"]`);
  if (!groupEl) {
    _renderNav();
    return;
  }

  const titleEl = groupEl.querySelector('.nav-group-title');
  const bodyEl = groupEl.querySelector('.nav-group-body');
  const arrowEl = titleEl.querySelector('.nav-group-arrow');

  if (group.isOpen) {
    groupEl.classList.add('open');
    titleEl.classList.add('open');
    if (bodyEl) bodyEl.style.display = 'block';
    if (arrowEl) arrowEl.setAttribute('data-lucide', 'chevron-down');
  } else {
    groupEl.classList.remove('open');
    titleEl.classList.remove('open');
    if (bodyEl) bodyEl.style.display = 'none';
    if (arrowEl) arrowEl.setAttribute('data-lucide', 'chevron-right');
  }

  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   Sidebar 開關（公開 API）
   ============================================ */

export function toggleSidebar() {
  const sidebar = document.querySelector('.sidebar');
  if (!sidebar) return;

  if (window.innerWidth < 640) {
    const isOpen = sidebar.classList.contains('mobile-open');
    if (isOpen) closeMobileSidebar();
    else openMobileSidebar();
  } else {
    sidebar.classList.toggle('collapsed');
    try {
      localStorage.setItem(
        STORAGE_KEYS.SIDEBAR_COLLAPSED,
        String(sidebar.classList.contains('collapsed'))
      );
    } catch (err) { /* noop */ }
  }
}

export function openMobileSidebar() {
  const sidebar = document.querySelector('.sidebar');
  if (!sidebar) return;
  sidebar.classList.add('mobile-open');

  let backdrop = document.querySelector('.sidebar-backdrop');
  if (!backdrop) {
    backdrop = document.createElement('div');
    backdrop.className = 'sidebar-backdrop';
    document.body.appendChild(backdrop);
  }
  backdrop.classList.add('active');
}

export function closeMobileSidebar() {
  const sidebar = document.querySelector('.sidebar');
  if (sidebar) sidebar.classList.remove('mobile-open');

  const backdrop = document.querySelector('.sidebar-backdrop');
  if (backdrop) backdrop.classList.remove('active');
}

/* ============================================
   銷毀
   ============================================ */
export function destroySidebar() {
  _navEl = null;

  if (_eventsBound) {
    document.removeEventListener('click', _globalClickHandler);
    _eventsBound = false;
  }

  closeMobileSidebar();

  _currentGroups = [];
  _filteredGroups = [];
  _activeHref = '';
}

/* ============================================
   內部：展開狀態管理（🆕 用過濾後群組）
   ============================================ */

function _loadOpenGroupSet() {
  try {
    const raw = localStorage.getItem(STORAGE_KEYS.SIDEBAR_GROUPS);
    if (!raw) return _getDefaultOpenSet();
    const arr = JSON.parse(raw);
    if (!Array.isArray(arr)) return _getDefaultOpenSet();
    return new Set(arr);
  } catch (e) {
    return _getDefaultOpenSet();
  }
}

function _saveOpenGroupSet(set) {
  try {
    localStorage.setItem(STORAGE_KEYS.SIDEBAR_GROUPS, JSON.stringify([...set]));
  } catch (e) { /* noop */ }
}

function _getDefaultOpenSet() {
  const set = new Set();
  _filteredGroups.forEach((g) => {
    if (g.defaultOpen) set.add(g.key);
  });
  return set;
}

function _ensureGroupOpenFor(set, activeHref) {
  if (!activeHref) return set;
  const g = _filteredGroups.find((group) =>
    (group.items || []).some((it) => it.href === activeHref)
  );
  if (g) set.add(g.key);
  return set;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/layout/sidebar.js
   Version: v103.0.31
   Batch: B26-4
   ═══════════════════════════════════════════ */```

---

## 📄 js/lib/async.js

```
// ============================================
// async.js — 非同步輔助（v103.0.2）
// 位置：js/lib/async.js
// ============================================
// v103.0.2 修正：
//   ✅ 從 '../shared/toast.js' 改為 '../ui/toast.js'
//   ✅ 從 '../shared/modal.js' 改為 '../ui/modal.js'
// ============================================

import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { esc } from './dom.js';

/* ============================================
   1. withToast
   ============================================ */
export async function withToast(successMsg, fn, opts = {}) {
  try {
    const result = await fn();
    if (successMsg && !opts.silent) {
      showToast(successMsg, 'success', opts.duration);
    }
    return { ok: true, result };
  } catch (err) {
    const msg = opts.errorMsg
      ? `${opts.errorMsg}：${err.message || err}`
      : (err.message || '操作失敗');
    if (!opts.silent) {
      showToast(msg, 'error', opts.duration);
    }
    return { ok: false, error: err };
  }
}

/* ============================================
   2. withConfirm
   ============================================ */
export async function withConfirm(message, fn, opts = {}) {
  const ok = await openConfirm(message, opts);
  if (!ok) return { cancelled: true };

  return await withToast(opts.successMsg, fn, opts);
}

/* ============================================
   3. withAsyncState
   ============================================ */
export async function withAsyncState(container, fn, opts = {}) {
  const root = typeof container === 'string'
    ? document.getElementById(container)
    : container;

  if (!root) {
    console.warn('[async] withAsyncState: 找不到容器', container);
    return await fn();
  }

  const loadingText = opts.loadingText || '載入中…';
  const errorPrefix = opts.errorPrefix || '載入失敗';

  root.innerHTML = `<div class="empty-state">${esc(loadingText)}</div>`;

  try {
    return await fn();
  } catch (err) {
    console.error('[async] withAsyncState 執行失敗：', err);
    root.innerHTML = `<div class="empty-state text-red">${esc(errorPrefix)}：${esc(err.message || err)}</div>`;
    throw err;
  }
}

/* ============================================
   4. safe
   ============================================ */
export async function safe(fn, fallback = null) {
  try {
    return await fn();
  } catch (err) {
    console.warn('[async.safe]', err);
    return fallback;
  }
}```

---

## 📄 js/lib/bank.js

```
// ============================================
// bank.js — 銀行業務輔助（v103.0.22）
// 位置：js/lib/bank.js
// ============================================
// v103.0.22（B25-4）：
//   ✅ M7：移除未使用的 checkBankSufficiency 佔位（無呼叫點）
// ============================================

import {
  addBankTransaction,
  updateBankTransaction,
  removeBankTransaction,
} from '../core/db.js';
import {
  BANK_TXN_TYPES,
  BANK_TXN_CATEGORIES,
  PAYMENT_MODES,
} from '../config/constants.js';
import { roundHKD, todayISO } from '../core/utils.js';

/* ============================================
   1. 銀行餘額計算（純函式）
   ============================================ */
export function calcBankBalance(bankAccount, transactions, targetYear, targetMonth) {
  if (!bankAccount) return 0;

  const initial = Number(bankAccount.initialBalance) || 0;
  const initY = Number(bankAccount.initialYear) || 0;
  const initM = Number(bankAccount.initialMonth) || 0;

  if (!initY || !initM) return initial;

  const tY = Number(targetYear);
  const tM = Number(targetMonth);

  if (tY < initY || (tY === initY && tM < initM)) {
    return initial;
  }

  let balance = initial;
  (transactions || []).forEach((txn) => {
    const date = txn.date || '';
    if (!date || date.length < 7) return;

    const [y, m] = date.split('-').map(Number);
    const afterInit = y > initY || (y === initY && m > initM);
    const beforeTarget = y < tY || (y === tY && m <= tM);

    if (!afterInit || !beforeTarget) return;

    const amount = Number(txn.amount) || 0;
    if (txn.type === BANK_TXN_TYPES.IN) {
      balance += amount;
    } else if (txn.type === BANK_TXN_TYPES.OUT) {
      balance -= amount;
    } else if (txn.type === BANK_TXN_TYPES.TRANSFER) {
      balance -= amount;
    }
  });

  return balance;
}

export function calcTotalBankBalance(bankAccounts, allTransactions, targetYear, targetMonth) {
  let total = 0;
  (bankAccounts || []).forEach((acc) => {
    const txns = (allTransactions || []).filter((t) => t.bankId === acc.id);
    total += calcBankBalance(acc, txns, targetYear, targetMonth);
  });
  return total;
}

export function getBankTransactions(allTransactions, bankId) {
  return (allTransactions || []).filter((t) => t.bankId === bankId);
}

/* ============================================
   2. 銀行交易建立
   ============================================ */
export async function createTransactionForExpense({
  bankId,
  memberId,
  amount,
  date,
  expenseId,
  note = '',
}) {
  if (!bankId || !amount) throw new Error('缺少必要參數');

  return addBankTransaction(bankId, {
    type: BANK_TXN_TYPES.OUT,
    category: BANK_TXN_CATEGORIES.EXPENSE,
    amount: roundHKD(amount),
    date: date || todayISO(),
    memberId: memberId || '',
    refId: expenseId || '',
    note: note || '',
  });
}

export async function createTransactionForInsurance({
  bankId,
  memberId,
  amount,
  date,
  policyId,
  note = '',
}) {
  if (!bankId || !amount) throw new Error('缺少必要參數');

  return addBankTransaction(bankId, {
    type: BANK_TXN_TYPES.OUT,
    category: BANK_TXN_CATEGORIES.INSURANCE,
    amount: roundHKD(amount),
    date: date || todayISO(),
    memberId: memberId || '',
    refId: policyId || '',
    note: note || '',
  });
}

export async function createTransactionForReimbursement({
  bankId,
  memberId,
  amount,
  date,
  policyId,
  note = '',
}) {
  if (!bankId || !amount) throw new Error('缺少必要參數');

  return addBankTransaction(bankId, {
    type: BANK_TXN_TYPES.TRANSFER,
    category: BANK_TXN_CATEGORIES.REIMBURSEMENT,
    amount: roundHKD(amount),
    date: date || todayISO(),
    memberId: memberId || '',
    refId: policyId || '',
    note: note || '',
  });
}

export async function createTransactionForContribution({
  bankId,
  memberId,
  amount,
  date,
  note = '',
}) {
  if (!bankId || !amount) throw new Error('缺少必要參數');

  return addBankTransaction(bankId, {
    type: BANK_TXN_TYPES.IN,
    category: BANK_TXN_CATEGORIES.CONTRIBUTION,
    amount: roundHKD(amount),
    date: date || todayISO(),
    memberId: memberId || '',
    refId: '',
    note: note || '',
  });
}

/* ============================================
   3. 支出編輯同步
   ============================================ */
export async function syncExpenseToBank({
  oldBankId,
  oldTxnId,
  newBankId,
  expenseData,
  expenseId,
}) {
  const hasOld = !!(oldBankId && oldTxnId);
  const hasNew = !!newBankId;

  if (hasOld && !hasNew) {
    try {
      await removeBankTransaction(oldBankId, oldTxnId);
    } catch (e) {
      console.warn('[bank] 刪除舊交易失敗：', e);
    }
    return '';
  }

  if (!hasOld && hasNew) {
    return await createTransactionForExpense({
      bankId: newBankId,
      memberId: expenseData.memberId,
      amount: expenseData.amount,
      date: expenseData.date,
      expenseId,
      note: expenseData.name || '',
    });
  }

  if (hasOld && hasNew && oldBankId === newBankId) {
    try {
      await updateBankTransaction(oldBankId, oldTxnId, {
        amount: roundHKD(expenseData.amount),
        date: expenseData.date,
        memberId: expenseData.memberId,
        note: expenseData.name || '',
      });
    } catch (e) {
      console.warn('[bank] 更新交易失敗：', e);
    }
    return oldTxnId;
  }

  if (hasOld && hasNew && oldBankId !== newBankId) {
    try {
      await removeBankTransaction(oldBankId, oldTxnId);
    } catch (e) {
      console.warn('[bank] 刪除舊交易失敗：', e);
    }
    return await createTransactionForExpense({
      bankId: newBankId,
      memberId: expenseData.memberId,
      amount: expenseData.amount,
      date: expenseData.date,
      expenseId,
      note: expenseData.name || '',
    });
  }

  return oldTxnId || '';
}

export async function cleanupExpenseBankTransaction(bankId, txnId) {
  if (!bankId || !txnId) return;
  try {
    await removeBankTransaction(bankId, txnId);
  } catch (e) {
    console.warn('[bank] 清理交易失敗：', e);
  }
}

/* ============================================
   4. 保險編輯同步
   ============================================ */
export async function createTransactionForInsurancePayment({
  paymentMode,
  bankId,
  memberId,
  amount,
  date,
  policyId,
  policyName = '',
}) {
  if (!bankId || !amount) {
    throw new Error('缺少 bankId 或 amount');
  }

  if (paymentMode === PAYMENT_MODES.ADVANCE) {
    return await createTransactionForReimbursement({
      bankId,
      memberId,
      amount,
      date,
      policyId,
      note: policyName ? `${policyName} (代墊還款)` : '',
    });
  }

  return await createTransactionForInsurance({
    bankId,
    memberId,
    amount,
    date,
    policyId,
    note: policyName || '',
  });
}

/* ============================================
   5. 交易分析
   ============================================ */
export function groupTransactionsByCategory(transactions) {
  const groups = {
    [BANK_TXN_CATEGORIES.CONTRIBUTION]:  { count: 0, amount: 0 },
    [BANK_TXN_CATEGORIES.EXPENSE]:       { count: 0, amount: 0 },
    [BANK_TXN_CATEGORIES.INSURANCE]:     { count: 0, amount: 0 },
    [BANK_TXN_CATEGORIES.REIMBURSEMENT]: { count: 0, amount: 0 },
    [BANK_TXN_CATEGORIES.MANUAL]:        { count: 0, amount: 0 },
  };

  (transactions || []).forEach((t) => {
    const cat = t.category || BANK_TXN_CATEGORIES.MANUAL;
    if (!groups[cat]) {
      groups[cat] = { count: 0, amount: 0 };
    }
    groups[cat].count++;
    groups[cat].amount += roundHKD(t.amount);
  });

  return groups;
}

export function filterTransactionsByMonth(transactions, year, month) {
  const prefix = `${year}-${String(month).padStart(2, '0')}`;
  return (transactions || []).filter((t) => (t.date || '').startsWith(prefix));
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/lib/bank.js
   Version: v103.0.22
   Batch: B25-4
   ═══════════════════════════════════════════ */```

---

## 📄 js/lib/code-registry.js

```
// ============================================
// code-registry.js — 編號生成 SSOT（v1.0.0）
// 位置：js/lib/code-registry.js
// ============================================
// 依 D-008 / D-059 / D-060：
//   ✅ 編號存實體節點；查最大 + 1
//   ✅ 無計數器；無 transaction
//   ✅ 保單子類短碼 3 種：NORM / FUND / SAVE
// ============================================

import {
  getMembersOnce,
  getBanksOnce,
  getBankAccountsOnce,
  getCategoriesOnce,
  getItemsOnce,
  getPaymentMethodsOnce,
  getInsuranceCompaniesOnce,
  getInsurancePoliciesOnce,
  getFundsOnce,
} from '../core/db.js';

/* ============================================
   1. 大類前綴對照（D-008）
   ============================================ */
export const PREFIX_MAP = {
  member:      'MEM',
  bank:        'PAY-BANK',
  bankAccount: 'PAY-BANK',   /* 子類由 extra 決定：{code}-{type} */
  category:    'CAT',
  item:        'ITEM',
  payment:     'PAY',
  company:     'COMP',
  policy:      'POL',
  fund:        'FUND',       /* 保險基金 → FUND-INS；獨立基金 → FUND */
  income:      'INC',
  bankTxn:     'TXN',
};

/* ============================================
   2. 保單子類短碼（D-060：3 種）
   ============================================ */
export const POLICY_SUBTYPE_MAP = {
  normal:         'NORM',   /* 普通類保險 */
  fund_insurance: 'FUND',   /* 基金類保險 */
  savings:        'SAVE',   /* 儲蓄類保險 */
};

/* ============================================
   3. 支出類別子類短碼（依 C-09 §1.4）
   ============================================ */
export const CATEGORY_SHORTCODES = {
  '學校類':    'SCH',
  '交通類':    'TRA',
  '醫療類':    'MED',
  '銀行類':    'BNK',
  '其他':      'OTH',
};

/* ============================================
   4. 核心生成演算法（D-008）
   ============================================
   步驟：
   1. 過濾出所有 code 以 {prefix}- 開頭的實體
   2. 提取序號部分（001, 002, ...）
   3. 取最大值 N
   4. 回傳 {prefix}-{N+1 補零到 3 位}
   5. 檢查是否重複 → 若重複，拋錯
   ============================================ */
export function generateCode(prefix, existingList) {
  if (!prefix) throw new Error('generateCode 缺少 prefix');
  if (!Array.isArray(existingList)) existingList = [];

  const prefixDash = prefix + '-';
  const seqs = [];

  existingList.forEach((item) => {
    const code = item?.code;
    if (!code || typeof code !== 'string') return;
    if (!code.startsWith(prefixDash)) return;

    /* 取 prefix- 之後的序號部分（可能為 001 或 SUB-001，依子類） */
    const rest = code.slice(prefixDash.length);
    const parts = rest.split('-');
    const seqPart = parts[parts.length - 1];
    const n = parseInt(seqPart, 10);
    if (!isNaN(n) && n > 0) seqs.push(n);
  });

  const maxN = seqs.length > 0 ? Math.max(...seqs) : 0;
  const nextN = maxN + 1;
  return prefixDash + String(nextN).padStart(3, '0');
}

/* ============================================
   5. 子類短碼生成（依 entityType + extra）
   ============================================ */
export function generateWithSubtype(prefix, subtype, existingList) {
  if (!subtype) return generateCode(prefix, existingList);
  return generateCode(`${prefix}-${subtype}`, existingList);
}

/* ============================================
   6. 依上下文自動決定子類短碼（D-010）
   ============================================ */
export function getSubtypeFromContext(entityType, extra = {}) {
  switch (entityType) {
    case 'policy': {
      const t = extra.type || 'normal';
      return POLICY_SUBTYPE_MAP[t] || 'NORM';
    }
    case 'category': {
      const name = (extra.name || '').trim();
      return CATEGORY_SHORTCODES[name] || null;
    }
    case 'fund': {
      return extra.type === 'insurance' ? 'INS' : null;
    }
    case 'bankAccount': {
      const parts = [];
      if (extra.bankCode) parts.push(extra.bankCode);
      if (extra.type) parts.push(extra.type === 'family' ? 'FAM' : 'PER');
      return parts.length > 0 ? parts.join('-') : null;
    }
    default:
      return null;
  }
}

/* ============================================
   7. 主入口：依 entityType 生成完整編號
   ============================================
   回傳：{ code, prefix, subtype }
   ============================================ */
export async function generateCodeFor(entityType, extra = {}) {
  const prefix = PREFIX_MAP[entityType];
  if (!prefix) throw new Error(`未知的 entityType：${entityType}`);

  const list = await _fetchExistingList(entityType);
  const subtype = getSubtypeFromContext(entityType, extra);

  let code;
  if (subtype) {
    code = generateWithSubtype(prefix, subtype, list);
  } else {
    code = generateCode(prefix, list);
  }

  /* 重複檢測（保險，正常情況不會重複） */
  if (checkDuplicate(code, list)) {
    throw new Error(`編號重複：${code}，請重試`);
  }

  return { code, prefix, subtype };
}

/* ============================================
   8. 重複檢測（D-009）
   ============================================ */
export function checkDuplicate(code, existingList) {
  if (!Array.isArray(existingList)) return false;
  return existingList.some((item) => item?.code === code);
}

/* ============================================
   9. 抓現有實體列表（Q127 A）
   ============================================ */
async function _fetchExistingList(entityType) {
  switch (entityType) {
    case 'member':      return await getMembersOnce();
    case 'bank':        return await getBanksOnce();
    case 'bankAccount': return await getBankAccountsOnce();
    case 'category':    return await getCategoriesOnce();
    case 'item':        return await getItemsOnce();
    case 'payment':     return await getPaymentMethodsOnce();
    case 'company':     return await getInsuranceCompaniesOnce();
    case 'policy':      return await getInsurancePoliciesOnce();
    case 'fund':        return await getFundsOnce();
    default:
      console.warn(`[code-registry] 未知 entityType：${entityType}`);
      return [];
  }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/lib/code-registry.js
   Version: 1791900400
   Batch: B34-4
   Writer: A20261013-004#076
   ═══════════════════════════════════════════ */```

---

## 📄 js/lib/dom.js

```
// ============================================
// dom.js — DOM 操作 SSOT（v103.0.0）
// 位置：js/lib/dom.js
// ============================================
// 職責：
//   1. 統一 DOM 選取 / 建立 / 逸出
//   2. 條件執行輔助（when / unless）
//   3. 取代散落各處的 querySelector / innerHTML 拼接
//
// 設計原則：
//   - 無依賴，純函式
//   - esc() 為全站 HTML 逸出 SSOT
// ============================================

/* ============================================
   1. 選取
   ============================================ */

/**
 * querySelector 單一
 * @param {string} sel
 * @param {Element|Document} [root=document]
 * @returns {Element|null}
 */
export function qs(sel, root = document) {
  if (!sel) return null;
  return (root || document).querySelector(sel);
}

/**
 * querySelectorAll → Array
 * @param {string} sel
 * @param {Element|Document} [root=document]
 * @returns {Element[]}
 */
export function qsa(sel, root = document) {
  if (!sel) return [];
  return [...(root || document).querySelectorAll(sel)];
}

/* ============================================
   2. HTML 逸出（SSOT）
   ============================================ */

const _ESCAPE_MAP = {
  '&': '&amp;',
  '<': '&lt;',
  '>': '&gt;',
  '"': '&quot;',
  "'": '&#39;',
};

/**
 * HTML 逸出
 * - null / undefined → fallback
 * - 其他 → 逸出字元
 *
 * @param {*} v
 * @param {string} [fallback='']
 * @returns {string}
 */
export function esc(v, fallback = '') {
  if (v == null) return fallback;
  return String(v).replace(/[&<>"']/g, (c) => _ESCAPE_MAP[c]);
}

/* ============================================
   3. 條件執行
   ============================================ */

/**
 * 若 cond 為真則執行 fn
 * @param {*} cond
 * @param {Function} fn
 * @returns {*} fn 的回傳值或 undefined
 */
export function when(cond, fn) {
  if (cond && typeof fn === 'function') return fn();
  return undefined;
}

/**
 * 若 cond 為假則執行 fn
 * @param {*} cond
 * @param {Function} fn
 * @returns {*} fn 的回傳值或 undefined
 */
export function unless(cond, fn) {
  if (!cond && typeof fn === 'function') return fn();
  return undefined;
}

/* ============================================
   4. 建立元素
   ============================================ */

/**
 * 建立 DOM 元素
 *
 * @param {string} tag
 * @param {Object} [attrs] - 屬性（class / style / dataset / on* 有特殊處理）
 * @param {Array|Node|string} [children]
 * @returns {HTMLElement}
 */
export function el(tag, attrs = {}, children = []) {
  const node = document.createElement(tag);

  if (attrs && typeof attrs === 'object') {
    Object.entries(attrs).forEach(([key, val]) => {
      if (val == null || val === false) return;

      if (key === 'class' || key === 'className') {
        node.className = val;
      } else if (key === 'style' && typeof val === 'object') {
        Object.assign(node.style, val);
      } else if (key === 'dataset' && typeof val === 'object') {
        Object.assign(node.dataset, val);
      } else if (key.startsWith('on') && typeof val === 'function') {
        node.addEventListener(key.slice(2).toLowerCase(), val);
      } else if (key === 'html') {
        node.innerHTML = val;
      } else if (key === 'text') {
        node.textContent = val;
      } else {
        node.setAttribute(key, val);
      }
    });
  }

  const kids = Array.isArray(children) ? children : [children];
  kids.forEach((c) => {
    if (c == null || c === false) return;
    if (c instanceof Node) node.appendChild(c);
    else node.appendChild(document.createTextNode(String(c)));
  });

  return node;
}
```

---

## 📄 js/lib/expense-modal.js

```
// ============================================
// expense-modal.js — 支出編輯 Modal（v103.0.23）
// v103.0.23（B34-4）：
//   ✅ m-pay 加 plusEntity: 'payment'
//   ✅ m-cat 加 plusEntity: 'category' + onPlusReload（類別變更後重載項目）
//   ✅ m-member 加 plusEntity: 'member' + onPlusReload（保留 shared）
// v103.0.22（B25-3a）：加 mode 參數（'data' | 'settle'）
// ============================================
import { buildForm } from '../ui/form-builder.js';
import { openModal, closeModal } from '../ui/modal.js';
import { showToast } from '../ui/toast.js';
import { addExpense, updateExpense, removeExpense } from '../core/db.js';
import { getDynamicOptions } from '../entity/entity-helpers.js';
import { AppState } from '../core/state.js';
import { RESERVED_IDS } from '../config/constants.js';
import { normalize as normalizeStatus, options as statusOptions } from '../config/status-registry.js';
import { esc } from '../lib/dom.js';

const MODAL_ID = 'app-expense-modal';

export async function openExpenseModal(opts = {}) {
  const { row = null, mode = 'data', onSuccess } = opts;
  const isEdit = !!row;
  const isSettle = mode === 'settle';

  document.getElementById(MODAL_ID)?.remove();

  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = MODAL_ID;
  overlay.innerHTML = `<div class="modal" style="max-width:560px;max-height:90vh;overflow-y:auto;"><h2 class="modal-title">${isSettle ? '結算支出' : (isEdit ? '編輯支出' : '新增支出')}</h2><div id="${MODAL_ID}-form-root"></div></div>`;
  document.body.appendChild(overlay);
  overlay.addEventListener('click', (e) => { if (e.target === overlay) { closeModal(MODAL_ID); overlay.remove(); } });

  const [members, categories, payments, banks] = await Promise.all([
    getDynamicOptions('members'),
    getDynamicOptions('categories'),
    getDynamicOptions('payments'),
    getDynamicOptions('bankAccounts'),
  ]);
  const memberOptions = [...members, { value: RESERVED_IDS.SHARED_MEMBER, label: '🏠 家庭共用' }];
  const { year, month } = AppState.getYearMonth();

  /* ============================================
     🆕 v103.0.23：+ 號重載邏輯（呼叫端自訂）
     ============================================ */
  const reloadMembers = async (result, api) => {
    const fresh = await getDynamicOptions('members');
    const opts = [...fresh, { value: RESERVED_IDS.SHARED_MEMBER, label: '🏠 家庭共用' }];
    api.updateOptions('m-member', opts, { includeEmpty: false });
    if (result.id) api.setFieldValue('m-member', result.id);
  };

  const reloadCategories = async (result, api) => {
    const fresh = await getDynamicOptions('categories');
    api.updateOptions('m-cat', fresh, { includeEmpty: true, emptyText: '— 請選擇類別 —' });
    if (result.id) {
      api.setFieldValue('m-cat', result.id);
      /* 類別變更後清空項目 */
      const itemEl = document.getElementById('m-item');
      if (itemEl) {
        itemEl.innerHTML = '<option value="">— 請先選擇類別 —</option>';
        itemEl.value = '';
      }
    }
  };

  const fields = isSettle ? [
    { type: 'select', id: 'm-status', label: '狀態', required: true, includeEmpty: false, options: statusOptions({ source: 'personal' }) },
    { type: 'select', id: 'm-pay', label: '支付方式', includeEmpty: true, options: payments, plusEntity: 'payment' },
    { type: 'select', id: 'm-bank', label: '銀行（可選）', includeEmpty: true, options: banks },
  ] : [
    { type: 'select', id: 'm-member', label: '成員', required: true, includeEmpty: false, options: memberOptions, plusEntity: 'member', onPlusReload: reloadMembers },
    { type: 'text', id: 'm-name', label: '項目名稱', required: true, maxlength: 60 },
    { type: 'number', id: 'm-amount', label: '金額（HK$）', required: true, min: 0, step: 1 },
    { type: 'text', id: 'm-date', label: '日期（YYYY-MM-DD）' },
    { type: 'select', id: 'm-cat', label: '類別', includeEmpty: true, options: categories, plusEntity: 'category', onPlusReload: reloadCategories },
    { type: 'select', id: 'm-item', label: '項目', includeEmpty: true, options: [] },
  ];

  const initialData = isSettle
    ? {
        'm-status': normalizeStatus(row?.status) || 'pending',
        'm-pay': row?.paymentMethodId || '',
        'm-bank': row?.bankId || '',
      }
    : (isEdit
        ? {
            'm-member': row.memberId || '',
            'm-name': row.name || '',
            'm-amount': row.amount || 0,
            'm-date': row.date || '',
            'm-cat': row.categoryId || '',
            'm-item': row.itemId || '',
          }
        : {
            'm-member': AppState.getCurrentMemberId() || memberOptions[0]?.value || '',
            'm-amount': 0,
            'm-date': new Date().toISOString().slice(0, 10),
          });

  buildForm({
    containerId: `${MODAL_ID}-form-root`,
    fields,
    submitText: isSettle ? '儲存' : (isEdit ? '儲存' : '新增'),
    showCancel: true,
    cancelText: '取消',
    initialData,
    onSubmit: async (data) => {
      try {
        if (isSettle) {
          const payload = {
            status: data['m-status'] || 'pending',
            paymentMethodId: data['m-pay'] || '',
            bankId: data['m-bank'] || '',
          };
          await updateExpense(row.year, row.month, row.memberId, row.id, payload);
          showToast('✅ 已更新結算狀態', 'success');
        } else {
          const memberId = data['m-member'];
          const payload = {
            name: data['m-name'],
            amount: Number(data['m-amount']) || 0,
            date: data['m-date'] || '',
            categoryId: data['m-cat'] || '',
            itemId: data['m-item'] || '',
          };
          if (isEdit) {
            if (memberId !== row.memberId) {
              await removeExpense(row.year, row.month, row.memberId, row.id);
              await addExpense(row.year, row.month, memberId, payload);
            } else {
              await updateExpense(row.year, row.month, memberId, row.id, payload);
            }
            showToast('✅ 已更新支出', 'success');
          } else {
            const y = year || String(new Date().getFullYear());
            const m = month === 'all' ? '01' : month;
            await addExpense(y, m, memberId, payload);
            showToast('✅ 已新增支出', 'success');
          }
        }
        closeModal(MODAL_ID);
        overlay.remove();
        if (typeof onSuccess === 'function') onSuccess();
      } catch (err) { showToast('儲存失敗：' + err.message, 'error'); }
    },
    onCancel: () => { closeModal(MODAL_ID); overlay.remove(); },
  });

  if (!isSettle) {
    const catEl = document.getElementById('m-cat');
    const itemEl = document.getElementById('m-item');
    if (catEl && itemEl) {
      const updateItems = async () => {
        const list = await getDynamicOptions('items', { categoryId: catEl.value });
        itemEl.innerHTML = '<option value="">— 請選擇項目 —</option>' +
          list.map((o) => `<option value="${esc(o.value)}">${esc(o.label)}</option>`).join('');
      };
      catEl.addEventListener('change', updateItems);
      if (isEdit && row.categoryId) {
        await updateItems();
        if (row.itemId) itemEl.value = row.itemId;
      }
    }
  }

  openModal(MODAL_ID);
  if (window.lucide) window.lucide.createIcons();
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/lib/expense-modal.js
   Version: 1791902020
   Batch: B34-4
   Writer: A20261013-004#103
   ═══════════════════════════════════════════ */```

---

## 📄 js/lib/filter-sort.js

```
// filter-sort.js — 通用篩選 / 排序（v103.0.18）

/**
 * 篩選 list
 * @param {Array} list
 * @param {Object} filters - { field: value }，空值忽略
 * @param {Object} [opts]
 * @param {string} [opts.startsWithField] - 前綴比對欄位（例如 year）
 */
export function applyFilters(list, filters, opts = {}) {
  if (!filters || Object.keys(filters).length === 0) return [...list];
  return list.filter((r) => {
    for (const [k, v] of Object.entries(filters)) {
      if (v == null || v === '') continue;
      if (k.endsWith('StartsWith')) {
        const f = k.slice(0, -10);
        if (!String(r[f] || '').startsWith(v)) return false;
      } else {
        if (r[k] !== v) return false;
      }
    }
    return true;
  });
}

/**
 * 排序 list
 * @param {Array} list
 * @param {string} mode - 排序模式
 * @param {Object} [modes] - 自訂模式
 */
export function applySort(list, mode, modes = {}) {
  const arr = [...list];
  const builtin = {
    'date-desc': (a, b) => (b.date || '').localeCompare(a.date || ''),
    'date-asc': (a, b) => (a.date || '').localeCompare(b.date || ''),
    'amount-desc': (a, b) => (Number(b.amount) || 0) - (Number(a.amount) || 0),
    'amount-asc': (a, b) => (Number(a.amount) || 0) - (Number(b.amount) || 0),
    'name-asc': (a, b) => String(a.name || '').localeCompare(String(b.name || ''), 'zh-HK'),
    'pending-first': (a, b) => {
      if (a.isDone !== b.isDone) return a.isDone ? 1 : -1;
      return (b.date || '').localeCompare(a.date || '');
    },
  };
  const fn = modes[mode] || builtin[mode];
  if (fn) arr.sort(fn);
  return arr;
}```

---

## 📄 js/lib/format.js

```
// ============================================
// format.js — 格式化工具 SSOT（v103.0.0）
// 位置：js/lib/format.js
// ============================================
// 來源：從 js/core/utils.js 抽出格式化函式（v103.0.0）
//
// 職責：
//   1. 金額格式化（HKD / 純數字 / 百分比）
//   2. 通用 cell 值格式化
//   3. 銀行交易類型 / 分類格式化
//
// 設計原則：
//   - 所有數字格式化一律走此處
//   - 純函式，無副作用
// ============================================

import { esc } from './dom.js';
import {
  LIMITS,
  BANK_TXN_TYPE_LABELS,
  BANK_TXN_CATEGORY_LABELS,
  BANK_TXN_CATEGORY_BADGES,
} from '../config/constants.js';

/* ============================================
   1. 金額格式化
   ============================================ */

/**
 * 格式化為 HK$ 顯示
 * @param {*} amount
 * @returns {string}
 */
export function formatHKD(amount) {
  if (amount == null || isNaN(amount)) return 'HK$ 0';
  const rounded = Math.round(Number(amount));
  return 'HK$ ' + rounded.toLocaleString('zh-HK');
}

/**
 * 格式化為純數字（千分位）
 * @param {*} amount
 * @returns {string}
 */
export function formatNumber(amount) {
  if (amount == null || isNaN(amount)) return '0';
  const rounded = Math.round(Number(amount));
  return rounded.toLocaleString('zh-HK');
}

/**
 * 四捨五入至整數
 * @param {*} value
 * @returns {number}
 */
export function roundHKD(value) {
  const n = Number(value);
  return isNaN(n) ? 0 : Math.round(n);
}

/**
 * 限制金額範圍（0 ~ AMOUNT_MAX）
 * @param {*} value
 * @returns {number}
 */
export function clampAmount(value) {
  const n = roundHKD(value);
  if (n < 0) return 0;
  if (n > LIMITS.AMOUNT_MAX) return LIMITS.AMOUNT_MAX;
  return n;
}

/**
 * 格式化百分比
 * @param {*} value
 * @param {number} [digits=2]
 * @returns {string}
 */
export function formatPercent(value, digits = 2) {
  const n = Number(value);
  if (isNaN(n)) return '0%';
  return `${n.toFixed(digits)}%`;
}

/* ============================================
   2. 通用 cell 值格式化（依欄位 type）
   ============================================ */

/**
 * 通用 cell 值格式化（SSOT）
 * @param {*} val
 * @param {'text'|'number'|'number-plain'|'date'|'select'} [type='text']
 * @returns {string} HTML
 */
export function formatCellValue(val, type) {
  if (val == null || val === '') return '<span class="text-muted">—</span>';

  switch (type) {
    case 'number':
      return formatHKD(val);
    case 'number-plain':
      return formatNumber(val);
    case 'date':
      return esc(String(val));
    case 'select':
    case 'text':
    default:
      return esc(String(val));
  }
}

/* ============================================
   3. 銀行交易格式化
   ============================================ */

/**
 * 交易類型顯示文字
 * @param {string} type
 * @returns {string}
 */
export function formatTransactionType(type) {
  return BANK_TXN_TYPE_LABELS[type] || type || '未知';
}

/**
 * 交易分類顯示文字
 * @param {string} category
 * @returns {string}
 */
export function formatTransactionCategory(category) {
  return BANK_TXN_CATEGORY_LABELS[category] || category || '未分類';
}

/**
 * 交易分類 badge class
 * @param {string} category
 * @returns {string}
 */
export function getCategoryBadgeClass(category) {
  return BANK_TXN_CATEGORY_BADGES[category] || 'badge-muted';
}
```

---

## 📄 js/lib/id-resolver.js

```
// ============================================
// id-resolver.js — 編號查詢 SSOT（v1.0.0）
// 位置：js/lib/id-resolver.js
// ============================================
// 依 D-011：
//   ✅ 從編號查實體（遍歷）
//   ✅ 從編號展開關聯（遍歷）
//   ✅ 不做「從名稱查編號」
// ============================================

import {
  getMembersOnce,
  getBanksOnce,
  getBankAccountsOnce,
  getCategoriesOnce,
  getItemsOnce,
  getPaymentMethodsOnce,
  getInsuranceCompaniesOnce,
  getInsurancePoliciesOnce,
  getFundsOnce,
  getAllMemberExpensesOnce,
} from '../core/db.js';

/* ============================================
   1. entityPath 對照
   ============================================ */
const ENTITY_PATH_FETCHER = {
  member:      getMembersOnce,
  bank:        getBanksOnce,
  bankAccount: getBankAccountsOnce,
  category:    getCategoriesOnce,
  item:        getItemsOnce,
  payment:     getPaymentMethodsOnce,
  company:     getInsuranceCompaniesOnce,
  policy:      getInsurancePoliciesOnce,
  fund:        getFundsOnce,
};

/* ============================================
   2. 從編號查實體（D-011 §3.1）
   ============================================
   決策：遍歷實體列表，比對 code
   原因：對齊議題 2「無索引」；實體數量少，成本低
   ============================================ */
export async function getEntityByCode(entityType, code) {
  if (!entityType || !code) return null;
  const fetcher = ENTITY_PATH_FETCHER[entityType];
  if (!fetcher) {
    console.warn(`[id-resolver] 未知 entityType：${entityType}`);
    return null;
  }
  const list = await fetcher();
  return list.find((item) => item.code === code) || null;
}

/* ============================================
   3. 從編號展開關聯（D-011 §3.3）
   ============================================
   決策：即時遍歷所有實體，找出引用此編號的
   使用場景：保單詳情顯示關聯基金、支付方法樹狀展開
   ============================================ */
export async function getRelatedEntities(code) {
  if (!code) return {};

  const [
    policies,
    funds,
    expenses,
    bankAccounts,
  ] = await Promise.all([
    getInsurancePoliciesOnce(),
    getFundsOnce(),
    _getAllExpensesSafe(),
    getBankAccountsOnce(),
  ]);

  const target = await _findEntityByCode(code, [
    ...policies,
    ...funds,
    ...bankAccounts,
  ]);

  if (!target) return {};

  const related = {
    target,
    policies: [],
    funds: [],
    expenses: [],
    bankAccounts: [],
  };

  /* 若 target 是保單 → 找關聯基金 */
  if (target.type !== undefined || target.firstStartYear !== undefined) {
    related.funds = funds.filter((f) => f.policyId === target.id);
  }

  /* 若 target 是基金 → 找所屬保單 */
  if (target.policyId) {
    related.policies = policies.filter((p) => p.id === target.policyId);
  }

  /* 找引用此編號的支出 */
  related.expenses = expenses.filter((e) =>
    e.policyId === target.id || e.categoryId === target.id || e.itemId === target.id
  );

  return related;
}

/* ============================================
   4. 內部：找實體（不分類型）
   ============================================ */
async function _findEntityByCode(code, lists) {
  for (const list of lists) {
    const found = list.find((item) => item.code === code);
    if (found) return found;
  }
  return null;
}

/* ============================================
   5. 內部：安全取得全年支出（不拋錯）
   ============================================ */
async function _getAllExpensesSafe() {
  try {
    const now = new Date();
    const y = now.getFullYear();
    const m = String(now.getMonth() + 1).padStart(2, '0');
    return await getAllMemberExpensesOnce(y, m);
  } catch (err) {
    console.warn('[id-resolver] 取得支出失敗：', err);
    return [];
  }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/lib/id-resolver.js
   Version: 1791900460
   Batch: B34-4
   Writer: A20261013-004#076
   ═══════════════════════════════════════════ */```

---

## 📄 js/lib/income-modal.js

```
// ============================================
// income-modal.js — 收入 Modal（v103.0.19）
// v103.0.19（B34-4）：
//   ✅ c-member / p-member 加 plusEntity: 'member' + onPlusReload
// ============================================
import { buildForm } from '../ui/form-builder.js';
import { openModal, closeModal } from '../ui/modal.js';
import { showToast } from '../ui/toast.js';
import { saveIncome, savePersonalIncome } from '../core/db.js';
import { getDynamicOptions } from '../entity/entity-helpers.js';
import { AppState } from '../core/state.js';

const CONTRIB_ID = 'app-contribution-modal';
const PERSONAL_ID = 'app-personal-income-modal';

/* 🆕 v103.0.19：成員 + 號重載 */
async function _reloadMembersInto(selectId, result, api) {
  const fresh = await getDynamicOptions('members');
  api.updateOptions(selectId, fresh, { includeEmpty: false });
  if (result.id) api.setFieldValue(selectId, result.id);
}

/**
 * 家用轉入 Modal
 */
export async function openContributionModal(opts = {}) {
  const { onSuccess } = opts;
  document.getElementById(CONTRIB_ID)?.remove();
  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = CONTRIB_ID;
  overlay.innerHTML = `<div class="modal" style="max-width:480px;"><h2 class="modal-title">家用轉入</h2><div id="${CONTRIB_ID}-form-root"></div></div>`;
  document.body.appendChild(overlay);
  overlay.addEventListener('click', (e) => { if (e.target === overlay) closeModal(CONTRIB_ID); });

  const members = await getDynamicOptions('members');
  const { year, month } = AppState.getYearMonth();
  const y = year || String(new Date().getFullYear());
  const m = month === 'all' ? '01' : month;

  buildForm({
    containerId: `${CONTRIB_ID}-form-root`,
    fields: [
      { type: 'select', id: 'c-member', label: '成員', required: true, includeEmpty: false, options: members,
        plusEntity: 'member',
        onPlusReload: (result, api) => _reloadMembersInto('c-member', result, api) },
      { type: 'number', id: 'c-amount', label: '金額（HK$）', required: true, min: 0, step: 1 },
    ],
    submitText: '儲存',
    showCancel: true,
    cancelText: '取消',
    onSubmit: async (data) => {
      try {
        await saveIncome(y, m, { [data['c-member']]: Number(data['c-amount']) || 0 });
        showToast(`✅ 已為 ${y}-${m} 新增家用轉入`, 'success');
        closeModal(CONTRIB_ID);
        if (typeof onSuccess === 'function') onSuccess();
      } catch (err) { showToast('儲存失敗：' + err.message, 'error'); }
    },
    onCancel: () => closeModal(CONTRIB_ID),
  });

  openModal(CONTRIB_ID);
  if (window.lucide) window.lucide.createIcons();
}

/**
 * 個人收入 Modal
 */
export async function openPersonalIncomeModal(opts = {}) {
  const { onSuccess } = opts;
  document.getElementById(PERSONAL_ID)?.remove();
  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = PERSONAL_ID;
  overlay.innerHTML = `<div class="modal" style="max-width:480px;"><h2 class="modal-title">個人收入</h2><div id="${PERSONAL_ID}-form-root"></div></div>`;
  document.body.appendChild(overlay);
  overlay.addEventListener('click', (e) => { if (e.target === overlay) closeModal(PERSONAL_ID); });

  const members = await getDynamicOptions('members');
  const { year, month } = AppState.getYearMonth();
  const y = year || String(new Date().getFullYear());
  const m = month === 'all' ? '01' : month;

  buildForm({
    containerId: `${PERSONAL_ID}-form-root`,
    fields: [
      { type: 'select', id: 'p-member', label: '成員', required: true, includeEmpty: false, options: members,
        plusEntity: 'member',
        onPlusReload: (result, api) => _reloadMembersInto('p-member', result, api) },
      { type: 'number', id: 'p-amount', label: '金額（HK$）', required: true, min: 0, step: 1 },
    ],
    submitText: '儲存',
    showCancel: true,
    cancelText: '取消',
    onSubmit: async (data) => {
      try {
        await savePersonalIncome(data['p-member'], y, m, Number(data['p-amount']) || 0);
        showToast(`✅ 已為 ${y}-${m} 新增個人收入`, 'success');
        closeModal(PERSONAL_ID);
        if (typeof onSuccess === 'function') onSuccess();
      } catch (err) { showToast('儲存失敗：' + err.message, 'error'); }
    },
    onCancel: () => closeModal(PERSONAL_ID),
  });

  openModal(PERSONAL_ID);
  if (window.lucide) window.lucide.createIcons();
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/lib/income-modal.js
   Version: 1791902080
   Batch: B34-4
   Writer: A20261013-004#103
   ═══════════════════════════════════════════ */```

---

## 📄 js/lib/insurance.js

```
// ============================================
// insurance.js — 保險計算模組（v103.0.23）
// 位置：js/lib/insurance.js
// ============================================
// v103.0.23（B30-7）：
//   ✅ 加 calcFundCostEstimate（基金成本估算：月供 × 已供月數）
//   ✅ 加 calcFundCostFromPayments（精確：從 payments 加總）
// v103.0.22（B25-4）：
//   ✅ H8：硬編碼 '已扣款' 改走 status-registry 的 isDone
// ============================================

import { getPolicyEffectiveMemberId } from '../config/constants.js';
import { isDone as isDoneStatus } from '../config/status-registry.js';

export function getPolicyHolderId(policy) {
  return getPolicyEffectiveMemberId(policy);
}

export function getPeriodRange(policy, periodIndex) {
  const firstY = Number(policy.firstStartYear) || 0;
  const firstM = Number(policy.firstStartMonth) || 1;

  const startDate = new Date(firstY, firstM - 1, 1);
  startDate.setMonth(startDate.getMonth() + (periodIndex - 1) * 12);

  const endDate = new Date(startDate);
  endDate.setMonth(endDate.getMonth() + 11);

  return {
    startY: startDate.getFullYear(),
    startM: String(startDate.getMonth() + 1).padStart(2, '0'),
    endY: endDate.getFullYear(),
    endM: String(endDate.getMonth() + 1).padStart(2, '0'),
    rangeText: `${startDate.getFullYear()}-${String(startDate.getMonth() + 1).padStart(2, '0')} ~ ${endDate.getFullYear()}-${String(endDate.getMonth() + 1).padStart(2, '0')}`,
  };
}

export function getPeriodInfo(policy, year, month) {
  const firstY = Number(policy.firstStartYear) || 0;
  const firstM = Number(policy.firstStartMonth) || 1;

  if (!firstY || firstY < 2000 || firstY > 2100) return null;
  if (!firstM || firstM < 1 || firstM > 12) return null;

  const startDate = new Date(firstY, firstM - 1, 1);
  const currentDate = new Date(Number(year), Number(month) - 1, 1);

  const diffMonths =
    (currentDate.getFullYear() - startDate.getFullYear()) * 12 +
    (currentDate.getMonth() - startDate.getMonth());

  if (diffMonths < 0) return null;

  const periodIndex = Math.floor(diffMonths / 12) + 1;

  const totalYears = Number(policy.totalPolicyYears) || 0;
  if (totalYears > 0 && periodIndex > totalYears) return null;

  return { periodIndex, ...getPeriodRange(policy, periodIndex) };
}

export function getPolicyAnnualPremium(policy, targetYear) {
  if (!policy) return 0;

  const year = Number(targetYear);
  const firstY = Number(policy.firstStartYear) || 0;

  if (!firstY || year < firstY) return 0;

  const periodIndex = year - firstY + 1;
  const totalYears = Number(policy.totalPolicyYears) || 0;

  if (totalYears > 0 && periodIndex > totalYears) return 0;

  if (policy.type === 'fund_insurance') {
    const mp = Number(policy.monthlyPremium) || 0;
    return Math.round(mp * 12);
  }

  const periods = policy.periods || {};
  const p = periods[String(periodIndex)];

  if (p && p.annualPremium) {
    return Math.round(Number(p.annualPremium));
  }

  const periodKeys = Object.keys(periods)
    .map(Number)
    .filter((n) => !isNaN(n) && n > 0)
    .sort((a, b) => a - b);

  if (periodKeys.length > 0) {
    const below = periodKeys.filter((k) => k <= periodIndex);
    const target = below.length > 0 ? below[below.length - 1] : periodKeys[0];
    const tp = periods[String(target)];
    if (tp && tp.annualPremium) return Math.round(Number(tp.annualPremium));
  }

  return Math.round(Number(policy.annualPremium) || 0);
}

export function resolveMonthlyAmount(policy, year, month, paymentData) {
  if (!policy) return 0;

  if (paymentData && paymentData.amount && Number(paymentData.amount) > 0) {
    return Math.round(Number(paymentData.amount));
  }

  if (policy.type === 'fund_insurance') {
    return Math.round(Number(policy.monthlyPremium) || 0);
  }

  const info = getPeriodInfo(policy, year, month);
  if (info) {
    const periodData = (policy.periods || {})[String(info.periodIndex)];
    if (periodData && periodData.monthlyAverage) {
      return Math.round(Number(periodData.monthlyAverage));
    }
  }

  return Math.round(Number(policy.monthlyAverage) || 0);
}

export function estimateMonthlyAmount(policy, year, month) {
  return resolveMonthlyAmount(policy, year, month, null);
}

export function getPolicyTotalPremium(policy) {
  if (!policy) return 0;

  if (policy.type === 'fund_insurance') {
    const monthly = Number(policy.monthlyPremium) || 0;
    const years = Number(policy.totalPolicyYears) || 0;
    return Math.round(monthly * 12 * years);
  }

  const totalYears = Number(policy.totalPolicyYears) || 0;

  if (totalYears === 0) {
    return Math.round(Number(policy.annualPremium) || 0);
  }

  const periods = policy.periods || {};
  const periodKeys = Object.keys(periods)
    .map(Number)
    .filter((n) => !isNaN(n) && n > 0)
    .sort((a, b) => a - b);

  let fallback = Math.round(Number(policy.annualPremium) || 0);
  if (!fallback && periodKeys.length > 0) {
    const last = periods[String(periodKeys[periodKeys.length - 1])];
    if (last && last.annualPremium) fallback = Math.round(Number(last.annualPremium));
  }

  let total = 0;
  for (let i = 1; i <= totalYears; i++) {
    const p = periods[String(i)];
    if (p && p.annualPremium) {
      total += Math.round(Number(p.annualPremium));
    } else {
      total += fallback;
    }
  }

  return total;
}

export function getPolicyPaidTotal(payments) {
  let total = 0;
  Object.values(payments || {}).forEach((yearData) => {
    Object.values(yearData || {}).forEach((mData) => {
      if (mData && isDoneStatus(mData.status)) {
        total += Math.round(Number(mData.amount) || 0);
      }
    });
  });
  return total;
}

export function countCompletedPeriods(payments) {
  let count = 0;
  Object.values(payments || {}).forEach((yearData) => {
    Object.values(yearData || {}).forEach((mData) => {
      if (mData && isDoneStatus(mData.status)) count++;
    });
  });
  return count;
}

export function isPolicyCompleted(policy) {
  if (!policy) return false;
  if (policy.isCompleted) return true;
  if (policy.type === 'fund_insurance') return false;

  const total = Number(policy.totalPolicyPeriods) || 0;
  if (total <= 0) return false;

  const done = Number(policy.completedPeriods) || 0;
  return done >= total;
}

export function calcProgress(policy) {
  const total = Number(policy.totalPolicyPeriods) || 0;
  const done = Number(policy.completedPeriods) || 0;
  if (total <= 0) return 0;
  return Math.min(100, Math.round((done / total) * 100));
}

export function computeEnrichedPolicies(policies, paymentsCache, targetYear) {
  const year = Number(targetYear) || new Date().getFullYear();

  return (policies || []).map((p) => {
    const payments = paymentsCache[p.id] || {};
    const completedPeriods = p.type === 'fund_insurance' ? 0 : countCompletedPeriods(payments);

    const currentAnnualPremium = getPolicyAnnualPremium(p, year);
    const totalPremium = getPolicyTotalPremium(p);
    const paidTotal = getPolicyPaidTotal(payments);

    const enriched = {
      ...p,
      completedPeriods,
      _payments: payments,
      _currentAnnualPremium: currentAnnualPremium,
      _totalPremium: totalPremium,
      _paidTotal: paidTotal,
      _targetYear: year,
    };

    enriched._isCompleted = isPolicyCompleted(enriched);

    return enriched;
  });
}

export function countCompletedPolicies(enriched) {
  return (enriched || []).filter((p) => p._isCompleted).length;
}

export function calcYearTotalPremium(enriched, year) {
  return (enriched || []).reduce((s, p) => s + getPolicyAnnualPremium(p, year), 0);
}

export function calcMonthlyTotalAverage(enriched, year, month) {
  let total = 0;
  (enriched || []).forEach((p) => {
    if (p.type === 'fund_insurance') {
      const info = getPeriodInfo(p, year, month);
      if (info) {
        total += Math.round(Number(p.monthlyPremium) || 0);
      }
      return;
    }
    const info = getPeriodInfo(p, year, month);
    if (!info) return;
    const periodData = (p.periods || {})[String(info.periodIndex)];
    if (periodData && periodData.monthlyAverage) {
      total += Math.round(Number(periodData.monthlyAverage));
    }
  });
  return total;
}

/* ============================================
   🆕 v103.0.23：基金成本計算（基金保險用）
   -------------------------------------------------
   基金保險的「基金投入成本」= 月供 × 已供月數
   分兩種情境：
     1. 估算（依當前年月推算）— calcFundCostEstimate
     2. 精確（依實際扣款記錄）  — calcFundCostFromPayments
   ============================================ */

/**
 * 基金成本估算（依保單起始年月至當前年月）
 * @param {object} policy - 保單
 * @param {string|number} year - 當前年
 * @param {string|number} month - 當前月
 * @returns {number}
 */
export function calcFundCostEstimate(policy, year, month) {
  if (!policy || policy.type !== 'fund_insurance') return 0;

  const monthly = Number(policy.monthlyPremium) || 0;
  if (monthly <= 0) return 0;

  const fy = Number(policy.firstStartYear) || 0;
  const fm = Number(policy.firstStartMonth) || 1;
  if (!fy) return 0;

  const startDate = new Date(fy, fm - 1, 1);
  const curDate = new Date(Number(year), Number(month) - 1, 1);

  const diffMonths =
    (curDate.getFullYear() - startDate.getFullYear()) * 12 +
    (curDate.getMonth() - startDate.getMonth());

  if (diffMonths < 0) return 0;

  /* 含首尾，例如 2021-10 ~ 2026-10 = 61 期 */
  const paidMonths = diffMonths + 1;

  return Math.round(monthly * paidMonths);
}

/**
 * 基金成本精確（依實際已扣款記錄加總）
 * @param {object} payments - insurance_payments/{policyId} 節點
 * @returns {number}
 */
export function calcFundCostFromPayments(payments) {
  return getPolicyPaidTotal(payments);
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/lib/insurance.js
   Version: v103.0.23
   Batch: B30-7
   ═══════════════════════════════════════════ */```

---

## 📄 js/lib/lifecycle.js

```
// ============================================
// lifecycle.js — 生命週期清理註冊器（v103.0.0）
// 位置：js/lib/lifecycle.js
// ============================================
// 職責：
//   1. 統一管理「需要清理的資源」
//   2. 集中一次性銷毀（避免記憶體洩漏）
//   3. 支援 add / addAll / run
//
// 使用場景：
//   - Firebase onValue 監聽
//   - addEventListener 監聽
//   - setInterval / setTimeout
//   - 任何需要 destroy 的物件
// ============================================

/**
 * 建立清理註冊器
 *
 * @returns {{
 *   add: (fn: Function) => Function,
 *   addAll: (fns: Function[]) => void,
 *   remove: (fn: Function) => void,
 *   size: () => number,
 *   run: () => void,
 *   isDestroyed: () => boolean,
 * }}
 */
export function createCleanupRegistry() {
  const _items = [];
  let _destroyed = false;

  return {
    /**
     * 新增清理函式
     * @param {Function} fn
     * @returns {Function} 原 fn（方便鏈式）
     */
    add(fn) {
      if (_destroyed) {
        console.warn('[lifecycle] 註冊器已銷毀，忽略新增');
        return fn;
      }
      if (typeof fn === 'function') _items.push(fn);
      return fn;
    },

    /**
     * 批次新增
     * @param {Function[]} fns
     */
    addAll(fns = []) {
      if (_destroyed) return;
      fns.forEach((fn) => {
        if (typeof fn === 'function') _items.push(fn);
      });
    },

    /**
     * 移除指定清理函式（不執行）
     * @param {Function} fn
     */
    remove(fn) {
      const idx = _items.indexOf(fn);
      if (idx >= 0) _items.splice(idx, 1);
    },

    /**
     * 目前待清理數量
     * @returns {number}
     */
    size() {
      return _items.length;
    },

    /**
     * 執行所有清理函式（僅一次）
     * - 個別失敗不影響其他
     * - 執行後清空
     */
    run() {
      if (_destroyed) return;
      _destroyed = true;

      const snapshot = _items.slice();
      _items.length = 0;

      snapshot.forEach((fn) => {
        try {
          fn();
        } catch (err) {
          console.warn('[lifecycle] 清理函式失敗：', err);
        }
      });
    },

    /**
     * 是否已銷毀
     * @returns {boolean}
     */
    isDestroyed() {
      return _destroyed;
    },
  };
}
```

---

## 📄 js/lib/merge.js

```
// ============================================
// merge.js — 結算資料合併邏輯（v103.0.23）
// 位置：js/lib/merge.js
// ============================================
// v103.0.23（B31-1）：
//   ✅ 加 AppState.on('family-change') 重置狀態名稱快取
//      - 避免切換家庭後使用舊家庭的狀態名稱
// v103.0.22（B25-4）：
//   ✅ H8：fallback 改走 status-registry.toLegacyName
// ============================================

import { getStatusesByCategory } from '../config/app-config.js';
import { RESERVED_IDS } from '../config/constants.js';
import { toLegacyName } from '../config/status-registry.js';
import { AppState } from '../core/state.js';

let _insuranceStatusNames = null;

/* 🆕 v103.0.23：切換家庭時重置快取 */
try {
  AppState.on('family-change', () => {
    _insuranceStatusNames = null;
  });
} catch (e) { /* noop */ }

function _getInsuranceStatusNames() {
  if (_insuranceStatusNames) return _insuranceStatusNames;
  try {
    const list = getStatusesByCategory('insurance');
    _insuranceStatusNames = new Set(list.map((s) => s.name));
  } catch (e) {
    _insuranceStatusNames = new Set(['未扣款', '已扣款']);
  }
  return _insuranceStatusNames;
}

export function resetInsuranceStatusCache() {
  _insuranceStatusNames = null;
}

export function mergeSettlementData({
  memberExpenses = [],
  insuranceRows = [],
  year,
  month,
}) {
  const rows = [];

  memberExpenses
    .filter((e) => !e.isAutoLinked)
    .forEach((e) => {
      rows.push(_buildPersonalRow(e, year, month));
    });

  const linkedMap = {};
  memberExpenses
    .filter((e) => e.isAutoLinked)
    .forEach((e) => {
      const policyId = e.policyId || _extractPolicyId(e.id);
      if (!policyId) return;
      linkedMap[policyId] = e;
    });

  const allPolicyIds = new Set([
    ...Object.keys(linkedMap),
    ...insuranceRows.map((p) => p.policyId),
  ]);

  allPolicyIds.forEach((policyId) => {
    const linked = linkedMap[policyId];
    const insuranceRow = insuranceRows.find((p) => p.policyId === policyId);

    if (linked) {
      rows.push(_buildLinkedInsuranceRow(linked, insuranceRow, year, month, policyId));
    } else if (insuranceRow) {
      rows.push(_buildInsuranceRow(insuranceRow, year, month));
    }
  });

  return rows;
}

function _buildPersonalRow(e, year, month) {
  const isShared = e.memberId === RESERVED_IDS.SHARED_MEMBER;

  return {
    key: `personal-${year}-${month}-${e.memberId}-${e.id}`,
    source: 'personal',
    sourceLabel: isShared ? '🏠 家庭' : '🏷 個人',
    year,
    month,
    memberId: e.memberId || '',
    name: e.name || '（未命名）',
    amount: Number(e.amount) || 0,
    status: e.status || '未處理',
    isDone: _isDoneStatus(e.status, 'personal'),
    date: e.date || '',
    categoryId: e.categoryId || '',
    itemId: e.itemId || '',
    paymentMethodId: e.paymentMethodId || '',
    bankId: e.bankId || '',
    txnId: e.txnId || '',
    isAutoLinked: false,
    _ref: {
      memberId: e.memberId,
      expenseId: e.id,
    },
  };
}

function _buildLinkedInsuranceRow(linked, insuranceRow, year, month, policyId) {
  const effectiveMemberId = insuranceRow?.policyHolderId
    || insuranceRow?.memberId
    || linked.memberId
    || '';

  const status = _resolveInsuranceStatus(insuranceRow?.status, linked.status);

  return {
    key: `insurance-${year}-${month}-${policyId}`,
    source: 'insurance',
    sourceLabel: '🛡 保險',
    year,
    month,
    memberId: effectiveMemberId,
    name: insuranceRow?.policyName || linked.name || '（未命名保單）',
    amount: Number(linked.amount) || 0,
    status,
    isDone: _isDoneStatus(status, 'insurance'),
    date: linked.date || insuranceRow?.date || '',
    categoryId: linked.categoryId || '',
    bankId: linked.bankId || insuranceRow?.bankId || '',
    txnId: linked.txnId || insuranceRow?.txnId || '',
    paymentMode: insuranceRow?.paymentMode || 'direct',
    advanceHolderId: insuranceRow?.advanceHolderId || '',
    isAutoLinked: true,
    _ref: {
      policyId,
      memberId: effectiveMemberId,
      linkedId: linked.id,
    },
  };
}

function _buildInsuranceRow(p, year, month) {
  const effectiveMemberId = p.policyHolderId || p.memberId || '';
  const displayName = p.policyName || '（未命名保單）';

  const status = _resolveInsuranceStatus(p.status);

  return {
    key: `insurance-${year}-${month}-${p.policyId}`,
    source: 'insurance',
    sourceLabel: '🛡 保險',
    year,
    month,
    memberId: effectiveMemberId,
    name: displayName,
    amount: Number(p.amount) || 0,
    status,
    isDone: _isDoneStatus(status, 'insurance'),
    date: p.date || '',
    categoryId: '',
    bankId: p.bankId || '',
    txnId: p.txnId || '',
    paymentMode: p.paymentMode || 'direct',
    advanceHolderId: p.advanceHolderId || '',
    isAutoLinked: false,
    _ref: {
      policyId: p.policyId,
      memberId: effectiveMemberId,
    },
  };
}

/**
 * v103.0.22：fallback 走 status-registry.toLegacyName（SSOT）
 */
function _resolveInsuranceStatus(...candidates) {
  const validNames = _getInsuranceStatusNames();
  for (const c of candidates) {
    if (!c) continue;
    if (validNames.has(c)) return c;
  }
  return toLegacyName('pending', 'insurance');
}

function _extractPolicyId(id) {
  if (!id || typeof id !== 'string') return '';
  if (id.startsWith('linked_')) return id.slice(7);
  return '';
}

function _isDoneStatus(statusName, category) {
  if (!statusName) return false;

  try {
    const statuses = getStatusesByCategory(category);
    const found = statuses.find((s) => s.name === statusName);
    if (found) return !!found.isDone;
  } catch (e) {
    // fallback
  }

  return statusName.startsWith('已');
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/lib/merge.js
   Version: v103.0.23
   Batch: B31-1
   ═══════════════════════════════════════════ */
```

---

## 📄 js/lib/page-controls.js

```
// ============================================
// page-controls.js — 統一控制列渲染（v103.0.34）
// 位置：js/lib/page-controls.js
// ============================================
// v103.0.34（B29-1a）：
//   ✅ 強制 viewToggle / sort 在 body（忽略 position: 'header'）
//   ✅ year + month 都在左側（月靠年）
// v103.0.33（B26-8）：
//   ✅ year / month 改 select；內部讀寫 AppState
// ============================================

import { esc } from './dom.js';
import { STORAGE_PREFIXES } from '../config/constants.js';
import { AppState } from '../core/state.js';
import { getYearList } from '../config/app-config.js';

/* 🆕 v103.0.34：強制在 body 的 type（不接受 position: 'header'） */
const _FORCE_BODY_TYPES = ['viewToggle', 'sort'];

export function renderPageControls(config) {
  const { containerId, headerContainerId, items = [] } = config || {};
  const root = document.getElementById(containerId);
  if (!root) {
    console.warn(`[page-controls] 找不到容器 #${containerId}`);
    return null;
  }
  const headerRoot = headerContainerId ? document.getElementById(headerContainerId) : null;

  items.forEach((it) => {
    if (!it.__pcId) {
      it.__pcId = it.id || `pc-${it.type}-${Math.random().toString(36).slice(2, 8)}`;
    }
  });

  let _destroyed = false;

  const paint = () => {
    if (_destroyed) return;

    /* 🆕 v103.0.34：header items 排除強制 body 的 type */
    const headerItems = items.filter((it) =>
      it.position === 'header' && !_FORCE_BODY_TYPES.includes(it.type)
    );
    const bodyItems = items.filter((it) =>
      it.position !== 'header' || _FORCE_BODY_TYPES.includes(it.type)
    );

    /* 渲染 header */
    if (headerRoot) {
      const headerHtml = headerItems.length > 0
        ? `<div class="page-header-actions-group">${headerItems.map(_renderItem).join('')}</div>`
        : '';
      if (headerRoot.__lastHtml !== headerHtml) {
        headerRoot.__lastHtml = headerHtml;
        headerRoot.innerHTML = headerHtml;
      }
    }

    /* 渲染 body */
    const _isToolbarItem = (it) =>
      it.type !== 'year' && it.type !== 'month' &&
      it.type !== 'filter' && it.type !== 'viewSwitcher';

    const toolbarLeft = bodyItems.filter((it) => _isToolbarItem(it) && it.align !== 'right');
    const toolbarRight = bodyItems.filter((it) => _isToolbarItem(it) && it.align === 'right');
    const yearItem = bodyItems.find((it) => it.type === 'year');
    const monthItem = bodyItems.find((it) => it.type === 'month');
    const fullRowItems = bodyItems.filter((it) => it.type === 'filter' || it.type === 'viewSwitcher');

    const parts = [];

    if (toolbarLeft.length > 0 || toolbarRight.length > 0) {
      parts.push(`
        <div class="page-controls-row">
          <div class="page-controls-left">${toolbarLeft.map(_renderItem).join('')}</div>
          <div class="page-controls-right">${toolbarRight.map(_renderItem).join('')}</div>
        </div>
      `);
    }

    /* 🆕 v103.0.34：year + month 都在左側 */
    if (yearItem || monthItem) {
      parts.push(`
        <div class="page-controls-row page-controls-period">
          <div class="page-controls-left">
            ${yearItem ? _renderYearSelect(yearItem) : ''}
            ${monthItem ? _renderMonthSelect(monthItem) : ''}
          </div>
        </div>
      `);
    }

    fullRowItems.forEach((it) => {
      parts.push(`<div class="page-controls-row page-controls-full">${_renderItem(it)}</div>`);
    });

    const html = parts.length > 0 ? `<div class="page-controls">${parts.join('')}</div>` : '';
    if (root.__lastHtml !== html) {
      root.__lastHtml = html;
      root.innerHTML = html;
    }

    _bindAll(root, items);
    if (headerRoot) _bindAll(headerRoot, items);

    if (window.lucide) window.lucide.createIcons();
  };

  paint();

  return {
    refresh: paint,
    destroy: () => {
      _destroyed = true;
      [root, headerRoot].forEach((c) => {
        if (!c) return;
        const oldClick = c.__pcListener;
        if (oldClick) c.removeEventListener('click', oldClick);
        const oldChange = c.__pcChangeListener;
        if (oldChange) c.removeEventListener('change', oldChange);
        c.__pcListener = null;
        c.__pcChangeListener = null;
        c.innerHTML = '';
        c.__lastHtml = '';
      });
    },
  };
}

export function destroyPageControls(instance) {
  if (instance && typeof instance.destroy === 'function') {
    instance.destroy();
  }
}

/* ============================================
   元素渲染（依 type 分派）
   ============================================ */
function _renderItem(item) {
  switch (item.type) {
    case 'add':           return _renderAdd(item);
    case 'export':        return _renderExport(item);
    case 'viewToggle':    return _renderViewToggle(item);
    case 'viewSwitcher':  return _renderViewSwitcher(item);
    case 'sort':          return _renderSort(item);
    case 'filter':        return _renderFilter(item);
    case 'custom':        return _renderCustom(item);
    default:
      console.warn(`[page-controls] 未知 type：${item.type}`);
      return '';
  }
}

function _renderAdd(item) {
  const { label = '新增', icon = 'plus', __pcId } = item;
  return `<button type="button" class="btn btn-primary" data-pc-action="add" data-pc-id="${esc(__pcId)}">
    <i data-lucide="${esc(icon)}"></i> ${esc(label)}
  </button>`;
}

function _renderExport(item) {
  const { label = '匯出', icon = 'file-spreadsheet', className = 'btn-magenta', __pcId } = item;
  return `<button type="button" class="btn ${esc(className)}" data-pc-action="export" data-pc-id="${esc(__pcId)}">
    <i data-lucide="${esc(icon)}"></i> ${esc(label)}
  </button>`;
}

function _renderViewToggle(item) {
  const {
    storageKey,
    defaultView = 'table',
    cardText = '卡片模式',
    tableText = '表格模式',
    cardValue = 'card',
    tableValue = 'table',
    __pcId,
  } = item;
  const key = `${STORAGE_PREFIXES.VIEW}${storageKey || 'page-controls-default'}`;
  let current = defaultView;
  try {
    const saved = localStorage.getItem(key);
    if (saved === cardValue || saved === tableValue) current = saved;
  } catch (e) { /* noop */ }

  return `<div class="view-toggle-group" data-pc-view-toggle data-pc-id="${esc(__pcId)}" data-pc-storage="${esc(key)}" data-pc-card-value="${esc(cardValue)}" data-pc-table-value="${esc(tableValue)}">
    <button type="button" class="btn btn-sm ${current === cardValue ? 'btn-primary' : 'btn-ghost'}" data-pc-view="${esc(cardValue)}">${esc(cardText)}</button>
    <button type="button" class="btn btn-sm ${current === tableValue ? 'btn-primary' : 'btn-ghost'}" data-pc-view="${esc(tableValue)}">${esc(tableText)}</button>
  </div>`;
}

function _renderViewSwitcher(item) {
  const { options = [], value = '', __pcId } = item;
  return `<div class="view-toggle-group" data-pc-view-switcher data-pc-id="${esc(__pcId)}">
    ${options.map((o) => `<button type="button" class="btn btn-sm ${o.value === value ? 'btn-primary' : 'btn-ghost'}" data-pc-switch="${esc(o.value)}">${esc(o.label)}</button>`).join('')}
  </div>`;
}

function _renderSort(item) {
  const { options = [], value = '', __pcId } = item;
  return `<select class="select" data-pc-sort data-pc-id="${esc(__pcId)}" style="width:auto; padding:8px 12px;">
    ${options.map((o) => `<option value="${esc(o.value)}" ${o.value === value ? 'selected' : ''}>${esc(o.label)}</option>`).join('')}
  </select>`;
}

function _renderFilter(item) {
  const { render } = item;
  if (typeof render !== 'function') return '';
  try {
    return `<div class="page-controls-filter">${render()}</div>`;
  } catch (err) {
    console.error('[page-controls] filter render 失敗：', err);
    return '';
  }
}

function _renderCustom(item) {
  const { render, html } = item;
  if (typeof render === 'function') {
    try { return render(); } catch (err) { console.error('[page-controls] custom render 失敗：', err); return ''; }
  }
  return html || '';
}

/* ============================================
   年 / 月 下拉清單
   ============================================ */
function _renderYearSelect(item) {
  const { __pcId } = item;
  const years = getYearList();
  const cur = String(AppState.year || new Date().getFullYear());

  return `<select class="select" data-pc-year data-pc-id="${esc(__pcId)}" style="width:auto; padding:8px 12px;">
    ${years.map((y) => {
      const ys = String(y);
      return `<option value="${ys}" ${ys === cur ? 'selected' : ''}>${ys} 年</option>`;
    }).join('')}
  </select>`;
}

function _renderMonthSelect(item) {
  const { includeAll = false, __pcId } = item;
  const curRaw = AppState.month === 'all' ? 'all' : String(AppState.month || '01').padStart(2, '0');

  const options = [];
  if (includeAll) {
    options.push({ value: 'all', label: '全部' });
  }
  for (let m = 1; m <= 12; m++) {
    const mm = String(m).padStart(2, '0');
    options.push({ value: mm, label: `${m} 月` });
  }

  return `<select class="select" data-pc-month data-pc-id="${esc(__pcId)}" style="width:auto; padding:8px 12px;">
    ${options.map((o) => `<option value="${esc(o.value)}" ${o.value === curRaw ? 'selected' : ''}>${esc(o.label)}</option>`).join('')}
  </select>`;
}

/* ============================================
   事件綁定
   ============================================ */
function _findByItem(items, type, id) {
  return items.find((it) => it.type === type && it.__pcId === id);
}

function _bindAll(container, items) {
  const oldClick = container.__pcListener;
  if (oldClick) container.removeEventListener('click', oldClick);
  const oldChange = container.__pcChangeListener;
  if (oldChange) container.removeEventListener('change', oldChange);

  const clickHandler = (e) => {
    const addBtn = e.target.closest('button[data-pc-action="add"]');
    if (addBtn) {
      const item = _findByItem(items, 'add', addBtn.dataset.pcId);
      if (item && typeof item.onClick === 'function') item.onClick();
      return;
    }

    const expBtn = e.target.closest('button[data-pc-action="export"]');
    if (expBtn) {
      const item = _findByItem(items, 'export', expBtn.dataset.pcId);
      if (item && typeof item.onClick === 'function') item.onClick();
      return;
    }

    const viewBtn = e.target.closest('[data-pc-view-toggle] button[data-pc-view]');
    if (viewBtn) {
      const group = viewBtn.closest('[data-pc-view-toggle]');
      const cardValue = group.dataset.pcCardValue;
      const tableValue = group.dataset.pcTableValue;
      const storageKey = group.dataset.pcStorage;
      const view = viewBtn.dataset.pcView;

      group.querySelectorAll('button').forEach((b) => {
        const isActive = b.dataset.pcView === view;
        b.classList.toggle('btn-primary', isActive);
        b.classList.toggle('btn-ghost', !isActive);
      });
      try { localStorage.setItem(storageKey, view); } catch (err) { /* noop */ }

      const item = _findByItem(items, 'viewToggle', group.dataset.pcId);
      if (item && typeof item.onChange === 'function') item.onChange(view);
      return;
    }

    const switchBtn = e.target.closest('[data-pc-view-switcher] button[data-pc-switch]');
    if (switchBtn) {
      const group = switchBtn.closest('[data-pc-view-switcher]');
      const value = switchBtn.dataset.pcSwitch;
      group.querySelectorAll('button').forEach((b) => {
        const isActive = b.dataset.pcSwitch === value;
        b.classList.toggle('btn-primary', isActive);
        b.classList.toggle('btn-ghost', !isActive);
      });
      const item = _findByItem(items, 'viewSwitcher', group.dataset.pcId);
      if (item && typeof item.onChange === 'function') item.onChange(value);
      return;
    }
  };

  const changeHandler = (e) => {
    const sortSel = e.target.closest('[data-pc-sort]');
    if (sortSel) {
      const item = _findByItem(items, 'sort', sortSel.dataset.pcId);
      if (item && typeof item.onChange === 'function') item.onChange(sortSel.value);
      return;
    }

    const yearSel = e.target.closest('[data-pc-year]');
    if (yearSel) {
      const y = yearSel.value;
      if (!y) return;
      if (String(AppState.year) === y) return;
      const curMonth = AppState.month === 'all' ? 'all' : AppState.month;
      AppState.setYearMonth(y, curMonth);
      return;
    }

    const monthSel = e.target.closest('[data-pc-month]');
    if (monthSel) {
      const m = monthSel.value;
      if (!m) return;
      if (String(AppState.month) === m) return;
      AppState.setYearMonth(AppState.year, m);
      return;
    }
  };

  container.addEventListener('click', clickHandler);
  container.addEventListener('change', changeHandler);
  container.__pcListener = clickHandler;
  container.__pcChangeListener = changeHandler;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/lib/page-controls.js
   Version: v103.0.34
   Batch: B29-1a
   ═══════════════════════════════════════════ */```

---

## 📄 js/lib/registry.js

```
// ============================================
// registry.js — Registry 統一初始化（v103.0.0）
// 位置：js/lib/registry.js
// ============================================
// 職責：
//   1. 統一初始化所有 Registry（status / entity / label / column）
//   2. 統一銷毀
//   3. 提供初始化狀態查詢
//
// 說明：
//   - status-registry / label-registry / column-registry 為純資料，無需初始化
//   - 僅 entity-registry 需要 Firebase 訂閱
// ============================================

import {
  init as initEntityRegistry,
  destroy as destroyEntityRegistry,
  isInitialized as isEntityRegistryInitialized,
} from '../config/entity-registry.js';

/* ============================================
   Module 狀態
   ============================================ */
let _initialized = false;

/* ============================================
   對外 API
   ============================================ */

/**
 * 初始化所有 Registry
 * - 已初始化 → 回傳 true
 * - 若尚未有 familyId → 回傳 false（可由呼叫端稍後重試）
 *
 * @returns {boolean}
 */
export function initAllRegistries() {
  if (_initialized) return true;

  const ok = initEntityRegistry();
  if (ok) {
    _initialized = true;
    console.log('✅ 所有 Registry 已初始化');
  } else {
    console.warn('[registry] entity-registry 初始化失敗（可能尚未選擇家庭）');
  }

  return ok;
}

/**
 * 銷毀所有 Registry
 */
export function destroyAllRegistries() {
  try {
    destroyEntityRegistry();
  } catch (err) {
    console.warn('[registry] destroy 失敗：', err);
  }
  _initialized = false;
}

/**
 * 查詢是否已初始化
 * @returns {boolean}
 */
export function isAllRegistriesReady() {
  return _initialized && isEntityRegistryInitialized();
}
```

---

## 📄 js/pages/annual-report.js

```
// annual-report.js — 年度報表（v103.0.29）
// v103.0.29（B26-3.2）：
//   ✅ export 移到 page-header 右側
//   ✅ year + month + viewSwitcher 保留在 body
// v103.0.27（B26-3）：
//   ✅ 改用 page-controls
// ============================================
import { formatHKD } from '../lib/format.js';
import { esc } from '../lib/dom.js';
import { AppState } from '../core/state.js';
import { RESERVED_IDS } from '../config/constants.js';
import { resolveName } from '../config/entity-registry.js';
import { renderPageControls } from '../lib/page-controls.js';

export default {
  title: '年度報表',
  data: {
    members:           { type: 'list', path: 'members' },
    allIncome:         { type: 'raw', path: 'income' },
    allExpenses:       { type: 'raw', path: 'expenses' },
    allPersonalIncome: { type: 'raw', path: 'personal_income' },
  },
  state: {
    year: String(AppState.year || new Date().getFullYear()),
    displayMonth: String(AppState.month === 'all' ? '01' : (AppState.month || '01')).padStart(2, '0'),
    view: 'summary',
  },
  derived: {
    membersRows: {
      deps: ['data.members', 'data.allIncome', 'data.allExpenses', 'data.allPersonalIncome', 'state.year'],
      compute: _annualRows,
    },
    monthlyGroups: {
      deps: ['data.allIncome', 'data.allExpenses', 'state.year', 'state.displayMonth'],
      compute: _monthlyGroups,
    },
    statsCards: {
      deps: ['data.allIncome', 'data.allExpenses', 'data.allPersonalIncome', 'state.year', 'state.view', 'state.displayMonth'],
      compute: _buildStats,
    },
  },
  blocks: [
    { type: 'stats', container: 'annual-stats-root', cards: '$.statsCards' },
  ],

  customMount: (ctx) => {
    let _controls = null;
    const _sumView = document.getElementById('annual-summary-view');
    const _monView = document.getElementById('annual-monthly-view');

    const paintSummary = () => {
      const rows = ctx.derived.membersRows;
      if (!rows || rows.length === 0) { _paint(_sumView, '<div class="glass-card"><div class="empty-state">載入中…</div></div>'); return; }
      const html = `<div class="summary-table-wrapper"><table class="annual-table"><thead><tr><th>成員</th><th class="num">家用轉入</th><th class="num">個人收入</th><th class="num">總支出</th><th class="num">淨結餘</th></tr></thead><tbody>${rows.map((r) => `<tr class="${r.__isTotal ? 'group-header' : (r.__isShared ? 'shared-row' : '')}"><td>${esc(r.name)}</td><td class="num">${formatHKD(r.income)}</td><td class="num">${formatHKD(r.personalIncome)}</td><td class="num">${formatHKD(r.expense)}</td><td class="num ${r.net >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(r.net)}</td></tr>`).join('')}</tbody></table></div>`;
      _paint(_sumView, html);
    };

    const paintMonthly = () => {
      const groups = ctx.derived.monthlyGroups || [];
      const { year, displayMonth } = ctx.state;
      if (groups.length === 0) { _paint(_monView, `<div class="glass-card"><div class="empty-state">${year}-${displayMonth} 尚無紀錄</div></div>`); return; }
      const body = groups.map((g) => `<div class="insurance-year-block" style="margin-bottom:10px;"><div class="insurance-year-header" style="cursor:default;"><span class="ins-year-title">${esc(g.title)}</span></div><div class="insurance-year-body">${g.rows.map((r) => `<div class="insurance-month-row" style="padding:6px 0;"><span style="flex:1;min-width:0;font-size:13px;">${esc(r.name)}</span><span class="mono" style="width:80px;text-align:right;font-size:12px;color:var(--text-secondary);">${r.month > 0 ? formatHKD(r.month) : '—'}</span><span class="mono" style="width:90px;text-align:right;font-size:12px;color:var(--neon-cyan);font-weight:600;">${formatHKD(r.year)}</span></div>`).join('')}<div class="insurance-month-row" style="padding:8px 0;border-top:1px solid rgba(0,240,255,0.15);margin-top:4px;"><span style="flex:1;font-weight:700;color:var(--neon-cyan);font-size:13px;">${esc(g.subtotal.label)}</span><span class="mono" style="width:80px;text-align:right;font-weight:700;color:var(--text-primary);">${formatHKD(g.subtotal.month)}</span><span class="mono" style="width:90px;text-align:right;font-weight:700;color:var(--neon-cyan);">${formatHKD(g.subtotal.year)}</span></div></div></div>`).join('');
      _paint(_monView, `<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;padding:8px 12px;background:rgba(0,240,255,0.04);border-radius:var(--radius-sm);"><span style="font-size:11px;color:var(--text-muted);letter-spacing:1px;">項目</span><span style="display:flex;gap:8px;"><span class="mono" style="width:80px;text-align:right;font-size:11px;color:var(--text-muted);">當月</span><span class="mono" style="width:90px;text-align:right;font-size:11px;color:var(--text-muted);">年度累計</span></span></div>${body}`);
    };

    const _paint = (el, html) => {
      if (!el || el.__lastHtml === html) return;
      el.__lastHtml = html;
      el.innerHTML = html;
    };

    const paint = () => { if (ctx.state.view === 'monthly') paintMonthly(); else paintSummary(); };

    /* 🆕 v103.0.29：export → header；year + month + viewSwitcher → body */
    _controls = renderPageControls({
      containerId: 'page-controls-root',
      headerContainerId: 'page-header-actions',
      items: [
        {
          type: 'export',
          position: 'header',
          label: '匯出 Excel',
          onClick: () => _exportCsv(ctx),
        },
        {
          type: 'year',
          year: ctx.state.year,
          onChange: (y) => {
            ctx.state.year = String(y);
            AppState.setYearMonth(String(y), AppState.month === 'all' ? ctx.state.displayMonth : AppState.month);
            paint();
          },
        },
        {
          type: 'month',
          month: ctx.state.displayMonth,
          onChange: (m) => {
            ctx.state.displayMonth = m;
            AppState.setYearMonth(ctx.state.year, m);
            paint();
          },
        },
        {
          type: 'viewSwitcher',
          value: ctx.state.view,
          options: [
            { value: 'summary', label: '全年總合' },
            { value: 'monthly', label: '月度明細' },
          ],
          onChange: (v) => {
            ctx.state.view = v;
            _sumView.style.display = v === 'summary' ? 'block' : 'none';
            _monView.style.display = v === 'monthly' ? 'block' : 'none';
            paint();
          },
        },
      ],
    });

    _sumView.style.display = ctx.state.view === 'summary' ? 'block' : 'none';
    _monView.style.display = ctx.state.view === 'monthly' ? 'block' : 'none';
    paint();

    const unsub = ctx.onDataChange((key) => {
      if (key === '__APP__') {
        const y = String(AppState.year || new Date().getFullYear());
        if (ctx.state.year !== y) ctx.state.year = y;
        const m = AppState.month === 'all' ? '01' : (AppState.month || '01');
        const mm = String(m).padStart(2, '0');
        if (ctx.state.displayMonth !== mm) ctx.state.displayMonth = mm;
        if (_controls) { try { _controls.refresh(); } catch (e) {} }
      }
      paint();
    });

    return {
      destroy: () => {
        unsub();
        if (_controls) { try { _controls.destroy(); } catch (e) {} }
      },
    };
  },
};

/* ============================================
   Stats 依 view 切換
   ============================================ */
function _buildStats(allIncome, allExpenses, allPersonalIncome, year, view, month) {
  if (view === 'monthly') return _monthlyStats(allIncome, allExpenses, allPersonalIncome, year, month);
  return _yearlyStats(allIncome, allExpenses, allPersonalIncome, year);
}

function _yearlyStats(allIncome, allExpenses, allPersonalIncome, year) {
  const yk = String(year || '');
  const yi = (allIncome || {})[yk] || {};
  let tI = 0;
  Object.values(yi).forEach((md) => { Object.values(md || {}).forEach((v) => { tI += Number(v) || 0; }); });

  let tPI = 0;
  Object.values(allPersonalIncome || {}).forEach((memberData) => {
    Object.values(((memberData || {})[yk] || {})).forEach((v) => { tPI += Number(v) || 0; });
  });

  const ye = (allExpenses || {})[yk] || {};
  let tE = 0;
  Object.values(ye).forEach((md) => {
    const me = (md && md.member_expenses) || {};
    Object.values(me).forEach((items) => {
      Object.values(items || {}).forEach((e) => { tE += Number(e?.amount) || 0; });
    });
  });

  const net = tI + tPI - tE;
  return [
    { title: `${year} 家用轉入`, value: formatHKD(tI), valueClass: 'emerald', hint: '家庭成員轉入加總', icon: 'trending-up' },
    { title: `${year} 個人收入`, value: formatHKD(tPI), valueClass: 'cyan', hint: '成員個人收入加總', icon: 'wallet' },
    { title: `${year} 年度總支出`, value: formatHKD(tE), valueClass: 'red', hint: '含家庭共用支出', icon: 'trending-down' },
    { title: `${year} 年度淨結餘`, value: formatHKD(net), valueClass: net >= 0 ? 'emerald' : 'red', hint: '轉入 + 個人收入 − 支出', icon: 'calculator' },
  ];
}

function _monthlyStats(allIncome, allExpenses, allPersonalIncome, year, month) {
  const yk = String(year || '');
  const mk = String(month || '').padStart(2, '0');
  const label = `${yk}-${mk}`;

  const yi = ((allIncome || {})[yk] || {})[mk] || {};
  let tI = 0;
  Object.values(yi).forEach((v) => { tI += Number(v) || 0; });

  let tPI = 0;
  Object.values(allPersonalIncome || {}).forEach((memberData) => {
    const yData = (memberData || {})[yk] || {};
    tPI += Number(yData[mk]) || 0;
  });

  const me = (((allExpenses || {})[yk] || {})[mk] || {}).member_expenses || {};
  let tE = 0;
  Object.values(me).forEach((items) => {
    Object.values(items || {}).forEach((e) => { tE += Number(e?.amount) || 0; });
  });

  const net = tI + tPI - tE;
  return [
    { title: `${label} 家用轉入`, value: formatHKD(tI), valueClass: 'emerald', hint: '該月轉入加總', icon: 'trending-up' },
    { title: `${label} 個人收入`, value: formatHKD(tPI), valueClass: 'cyan', hint: '該月個人收入加總', icon: 'wallet' },
    { title: `${label} 總支出`, value: formatHKD(tE), valueClass: 'red', hint: '該月支出加總', icon: 'trending-down' },
    { title: `${label} 淨結餘`, value: formatHKD(net), valueClass: net >= 0 ? 'emerald' : 'red', hint: '轉入 + 個人 − 支出', icon: 'calculator' },
  ];
}

/* ============================================
   Derived
   ============================================ */
function _annualRows(members, allIncome, allExpenses, allPersonalIncome, year) {
  const yk = String(year || '');
  const yi = (allIncome || {})[yk] || {};
  const ye = (allExpenses || {})[yk] || {};
  const rows = (members || []).map((m) => {
    let income = 0, pi = 0, exp = 0;
    Object.values(yi).forEach((md) => { income += Number(md && md[m.id]) || 0; });
    const mp = ((allPersonalIncome || {})[m.id] || {})[yk] || {};
    Object.values(mp).forEach((v) => { pi += Number(v) || 0; });
    Object.values(ye).forEach((md) => {
      const me = (md && md.member_expenses && md.member_expenses[m.id]) || {};
      Object.values(me).forEach((e) => { exp += Number(e && e.amount) || 0; });
    });
    return { id: m.id, name: m.name, income: Math.round(income), personalIncome: Math.round(pi), expense: Math.round(exp), net: Math.round(income + pi - exp) };
  });
  let se = 0;
  Object.values(ye).forEach((md) => {
    const s = (md && md.member_expenses && md.member_expenses[RESERVED_IDS.SHARED_MEMBER]) || {};
    Object.values(s).forEach((e) => { se += Number(e && e.amount) || 0; });
  });
  if (se > 0) rows.push({ id: RESERVED_IDS.SHARED_MEMBER, name: '家庭共用', income: 0, personalIncome: 0, expense: Math.round(se), net: -Math.round(se), __isShared: true });
  const total = rows.reduce((s, r) => ({ income: s.income + r.income, personalIncome: s.personalIncome + r.personalIncome, expense: s.expense + r.expense, net: s.net + r.net }), { income: 0, personalIncome: 0, expense: 0, net: 0 });
  rows.push({ id: '__total__', name: '總計', income: total.income, personalIncome: total.personalIncome, expense: total.expense, net: total.net, __isTotal: true });
  return rows;
}

function _monthlyGroups(allIncome, allExpenses, year, month) {
  const yk = String(year || ''), mk = String(month || '');
  const yi = (allIncome || {})[yk] || {};
  const ye = (allExpenses || {})[yk] || {};
  const groups = [];

  const incomeMonth = yi[mk] || {};
  const incomeYear = {};
  Object.values(yi).forEach((md) => Object.entries(md || {}).forEach(([mid, amt]) => { incomeYear[mid] = (incomeYear[mid] || 0) + (Number(amt) || 0); }));
  const incomeRows = Object.entries(incomeMonth).map(([mid, amt]) => ({ name: resolveName('members', mid) || mid, month: Number(amt) || 0, year: incomeYear[mid] || 0 }));
  if (incomeRows.length > 0) groups.push({ title: '【收入】', rows: incomeRows, subtotal: { label: '收入小計', month: incomeRows.reduce((s, r) => s + r.month, 0), year: incomeRows.reduce((s, r) => s + r.year, 0) } });

  const monthData = (ye[mk] && ye[mk].member_expenses) || {};
  Object.keys(monthData).forEach((mid) => {
    if (mid === RESERVED_IDS.SHARED_MEMBER) return;
    const memberName = resolveName('members', mid) || mid;
    const monthItems = monthData[mid] || {};
    const yearItems = {};
    Object.values(ye).forEach((md) => {
      const me = (md && md.member_expenses && md.member_expenses[mid]) || {};
      Object.entries(me).forEach(([id, e]) => {
        if (!yearItems[id]) yearItems[id] = { name: e.name || '', month: 0, year: 0 };
        yearItems[id].year += Number(e.amount) || 0;
      });
    });
    const rows = Object.entries(yearItems).map(([id, r]) => ({ name: r.name, month: monthItems[id] ? Number(monthItems[id].amount) || 0 : 0, year: r.year }));
    if (rows.length > 0) groups.push({ title: `【${memberName}】`, rows, subtotal: { label: `${memberName}小計`, month: rows.reduce((s, r) => s + r.month, 0), year: rows.reduce((s, r) => s + r.year, 0) } });
  });

  const sharedMonth = monthData[RESERVED_IDS.SHARED_MEMBER] || {};
  if (Object.keys(sharedMonth).length > 0) {
    const yearShared = {};
    Object.values(ye).forEach((md) => {
      const me = (md && md.member_expenses && md.member_expenses[RESERVED_IDS.SHARED_MEMBER]) || {};
      Object.entries(me).forEach(([id, e]) => {
        if (!yearShared[id]) yearShared[id] = { name: e.name || '', month: 0, year: 0 };
        yearShared[id].year += Number(e.amount) || 0;
      });
    });
    const rows = Object.entries(yearShared).map(([id, r]) => ({ name: r.name, month: sharedMonth[id] ? Number(sharedMonth[id].amount) || 0 : 0, year: r.year }));
    groups.push({ title: '【家庭共用】', rows, subtotal: { label: '家庭共用小計', month: rows.reduce((s, r) => s + r.month, 0), year: rows.reduce((s, r) => s + r.year, 0) } });
  }

  return groups;
}

function _exportCsv(ctx) {
  const rows = ctx.derived.membersRows || [];
  if (rows.length === 0) return;
  const year = ctx.state.year;
  const lines = [['成員', '家用轉入', '個人收入', '總支出', '淨結餘'].join(',')];
  rows.forEach((r) => lines.push([`"${String(r.name || '').replace(/"/g, '""')}"`, r.income, r.personalIncome, r.expense, r.net].join(',')));
  const csv = '\uFEFF' + lines.join('\n');
  const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = `annual-report-${year}.csv`;
  document.body.appendChild(a); a.click(); document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/annual-report.js
   Version: v103.0.29
   Batch: B26-3.2
   ═══════════════════════════════════════════ */```

---

## 📄 js/pages/code-list.js

```
// ============================================
// code-list.js — 編號對照清單頁（v1.0.1）
// 位置：js/pages/code-list.js
// ============================================
// v1.0.1（B34-4）：
//   ✅ 篩選器改 2 欄並排（外層 grid 2 列 + 內層 grid）（Q186 A / Q187 A）
// v1.0.0（B34-4）：初版
// ============================================

import { listenEntity } from '../entity/entity-helpers.js';
import { openEntityEditModal } from '../entity/entity-modal.js';
import { removeEntity } from '../entity/entity-helpers.js';
import { openConfirm } from '../ui/modal.js';
import { showToast } from '../ui/toast.js';
import { esc } from '../lib/dom.js';
import { AppState } from '../core/state.js';
import { ENTITY_KEYS } from '../config/constants.js';

/* ============================================
   涵蓋的實體類型
   ============================================ */
const ENTITY_TYPES = [
  { key: ENTITY_KEYS.MEMBER,   label: '成員',     prefix: 'MEM' },
  { key: ENTITY_KEYS.BANK,     label: '銀行',     prefix: 'PAY-BANK' },
  { key: ENTITY_KEYS.CATEGORY, label: '支出類別', prefix: 'CAT' },
  { key: ENTITY_KEYS.ITEM,     label: '支出項目', prefix: 'ITEM' },
  { key: ENTITY_KEYS.PAYMENT,  label: '支付方式', prefix: 'PAY' },
  { key: 'company',            label: '保險公司', prefix: 'COMP' },
  { key: ENTITY_KEYS.POLICY,   label: '保單',     prefix: 'POL' },
  { key: ENTITY_KEYS.FUND,     label: '基金',     prefix: 'FUND' },
  { key: 'bankAccount',        label: '銀行帳號', prefix: 'PAY-BANK' },
];

export default {
  title: '編號對照清單',
  data: {},
  state: {
    filterType: 'all',
    filterCodeState: 'all',
  },
  derived: {},
  blocks: [],

  customMount: (ctx) => {
    const allData = {};
    const unsubs = [];

    ENTITY_TYPES.forEach((t) => {
      const unsub = listenEntity(
        t.key,
        (list) => {
          allData[t.key] = list || [];
          _paint(ctx, allData);
        },
        (err) => console.warn(`[code-list] ${t.key} 訂閱失敗：`, err)
      );
      unsubs.push(unsub);
    });

    _renderSkeleton(ctx);
    _bindFilter(ctx);
    _bindListClick(ctx, allData);

    return {
      destroy: () => {
        unsubs.forEach((u) => { try { u && u(); } catch (e) {} });
      },
    };
  },
};

/* ============================================
   渲染骨架（v1.0.1：2 欄並排）
   ============================================ */
function _renderSkeleton(ctx) {
  const filterRoot = document.getElementById('cl-filter-root');
  if (filterRoot) {
    filterRoot.innerHTML = `
      <div class="glass-card" style="padding:12px;">
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:12px;">
          <div style="display:grid; grid-template-columns:72px 1fr; gap:8px; align-items:center;">
            <label style="font-size:12px; color:var(--text-muted); text-align:right;">類型：</label>
            <select class="select" id="cl-filter-type">
              <option value="all">全部</option>
              ${ENTITY_TYPES.map((t) => `<option value="${esc(t.key)}">${esc(t.label)}</option>`).join('')}
            </select>
          </div>
          <div style="display:grid; grid-template-columns:72px 1fr; gap:8px; align-items:center;">
            <label style="font-size:12px; color:var(--text-muted); text-align:right;">編號狀態：</label>
            <select class="select" id="cl-filter-code">
              <option value="all">全部</option>
              <option value="with-code">有編號</option>
              <option value="without-code">無編號</option>
            </select>
          </div>
        </div>
        ${AppState.isSuperAdmin ? `
          <div style="display:flex; justify-content:flex-end; margin-top:12px;">
            <button type="button" class="btn btn-sm btn-ghost" id="cl-permission-btn">
              <i data-lucide="shield" style="width:14px;height:14px;"></i> 權限設定
            </button>
          </div>
        ` : ''}
      </div>
    `;
    if (window.lucide) window.lucide.createIcons();
  }
}

/* ============================================
   綁定篩選器
   ============================================ */
function _bindFilter(ctx) {
  const typeSel = document.getElementById('cl-filter-type');
  const codeSel = document.getElementById('cl-filter-code');

  if (typeSel) {
    typeSel.addEventListener('change', () => {
      ctx.state.filterType = typeSel.value;
      _paint(ctx, ctx._allData);
    });
  }
  if (codeSel) {
    codeSel.addEventListener('change', () => {
      ctx.state.filterCodeState = codeSel.value;
      _paint(ctx, ctx._allData);
    });
  }
}

/* ============================================
   綁定列表點擊
   ============================================ */
function _bindListClick(ctx, allData) {
  const listRoot = document.getElementById('cl-list-root');
  if (!listRoot) return;

  listRoot.addEventListener('click', async (e) => {
    const btn = e.target.closest('button[data-action]');
    if (!btn) return;
    const { action, entity: entityType, id } = btn.dataset;
    if (!entityType || !id) return;

    if (action === 'edit') {
      const list = allData[entityType] || [];
      await openEntityEditModal(entityType, id, {
        allRows: list,
        onSuccess: () => showToast('✅ 已更新', 'success'),
      });
      return;
    }

    if (action === 'delete') {
      const list = allData[entityType] || [];
      const row = list.find((r) => r.id === id);
      const displayName = row?.name || id;
      const ok = await openConfirm(
        `⚠️ 確定要刪除「${displayName}」嗎？\n\n此操作無法復原。`,
        { title: '刪除', okText: '刪除', okClass: 'btn-danger' }
      );
      if (!ok) return;
      try {
        await removeEntity(entityType, id);
        showToast('✅ 已刪除', 'success');
      } catch (err) {
        showToast('刪除失敗：' + err.message, 'error');
      }
      return;
    }
  });
}

/* ============================================
   渲染主列表
   ============================================ */
function _paint(ctx, allData) {
  ctx._allData = allData;
  const listRoot = document.getElementById('cl-list-root');
  const emptyRoot = document.getElementById('cl-empty-state');
  if (!listRoot) return;

  const filterType = ctx.state.filterType;
  const filterCode = ctx.state.filterCodeState;

  const flatRows = [];
  ENTITY_TYPES.forEach((t) => {
    if (filterType !== 'all' && filterType !== t.key) return;
    const list = allData[t.key] || [];
    list.forEach((row) => {
      const hasCode = !!row.code;
      if (filterCode === 'with-code' && !hasCode) return;
      if (filterCode === 'without-code' && hasCode) return;
      flatRows.push({
        entityType: t.key,
        entityLabel: t.label,
        id: row.id,
        code: row.code || '',
        name: row.name || '（未命名）',
      });
    });
  });

  flatRows.sort((a, b) => {
    if (a.code && b.code) return a.code.localeCompare(b.code);
    if (a.code) return -1;
    if (b.code) return 1;
    return a.name.localeCompare(b.name, 'zh-HK');
  });

  if (flatRows.length === 0) {
    listRoot.innerHTML = '';
    if (emptyRoot) {
      emptyRoot.style.display = 'block';
      emptyRoot.innerHTML = '<div class="glass-card"><div class="empty-state">尚無資料</div></div>';
    }
    return;
  }
  if (emptyRoot) emptyRoot.style.display = 'none';

  listRoot.innerHTML = `
    <div class="glass-card" style="padding:0;">
      <div class="data-table-scroll-wrapper">
        <table class="data-table">
          <thead>
            <tr>
              <th style="width:180px;">編號</th>
              <th style="width:120px;">類型</th>
              <th>名稱</th>
              <th style="width:180px;">操作</th>
            </tr>
          </thead>
          <tbody>
            ${flatRows.map((r) => `
              <tr>
                <td><span class="mono" style="font-size:12px; color:${r.code ? 'var(--neon-cyan)' : 'var(--text-muted)'};">${esc(r.code || '—')}</span></td>
                <td><span class="badge badge-info">${esc(r.entityLabel)}</span></td>
                <td>${esc(r.name)}</td>
                <td>
                  <button type="button" class="btn btn-sm btn-ghost" data-action="edit" data-entity="${esc(r.entityType)}" data-id="${esc(r.id)}">
                    <i data-lucide="pencil" style="width:14px;height:14px;"></i> 編輯
                  </button>
                  <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-entity="${esc(r.entityType)}" data-id="${esc(r.id)}">
                    <i data-lucide="trash-2" style="width:14px;height:14px;"></i> 刪除
                  </button>
                </td>
              </tr>
            `).join('')}
          </tbody>
        </table>
      </div>
    </div>
  `;
  if (window.lucide) window.lucide.createIcons();
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/code-list.js
   Version: 1791902680
   Batch: B34-4
   Writer: A20261013-004#121
   ═══════════════════════════════════════════ */```

---

## 📄 js/pages/dashboard.js

```
// ============================================
// dashboard.js — 總覽儀表板（v103.0.35 Page Schema）
// 位置：js/pages/dashboard.js
// ============================================
// v103.0.35（B29-7b）：
//   ✅ 刪除副標題動態（subtitle derived + update 邏輯）
// ============================================

import { calcTotalBankBalance } from '../lib/bank.js';
import { formatHKD } from '../lib/format.js';
import { AppState } from '../core/state.js';
import { renderPageControls } from '../lib/page-controls.js';
import { getPolicyAnnualPremium } from '../lib/insurance.js';
import { getColumns, getResolvers } from '../config/column-registry.js';

export default {
  title: '總覽儀表板',
  data: {
    bankAccounts: { type: 'list', path: 'bank_accounts' },
    bankTransactions: { type: 'object', path: 'bank_accounts', transform: _flatten },
    members: { type: 'list', path: 'members' },
    allIncome: { type: 'raw', path: 'income' },
    allExpenses: { type: 'raw', path: 'expenses' },
    allPolicies: { type: 'list', path: 'insurance_policies' },
  },
  state: {
    currentYear: String(AppState.year || new Date().getFullYear()),
    currentMonth: String(AppState.month || '12'),
  },
  derived: {
    totalBankBalance: {
      deps: ['data.bankAccounts', 'data.bankTransactions', 'state.currentYear', 'state.currentMonth'],
      compute: (a, t, y, m) => calcTotalBankBalance(a || [], t || [], y, m === 'all' ? '12' : (m || '12')),
    },
    yearTotals: {
      deps: ['data.allIncome', 'data.allExpenses', 'state.currentYear'],
      compute: _computeYearTotals,
    },
    statsCards: {
      deps: ['totalBankBalance', 'data.bankAccounts', 'state.currentYear', 'yearTotals'],
      compute: _buildCards,
    },
    annualOverview: {
      deps: ['data.allIncome', 'data.allExpenses', 'data.allPolicies', 'state.currentYear'],
      compute: _computeAnnualOverview,
    },
  },
  blocks: [
    { type: 'stats', container: 'stats-cards-root', cards: '$.statsCards' },
  ],

  customMount: (ctx) => {
    let _controls = null;

    const syncYear = () => {
      const y = String(AppState.year || new Date().getFullYear());
      if (ctx.state.currentYear !== y) ctx.state.currentYear = y;
      const m = String(AppState.month || '12');
      if (ctx.state.currentMonth !== m) ctx.state.currentMonth = m;
    };

    const _paintAnnualOverview = () => {
      const root = document.getElementById('annual-overview-root');
      if (!root) return;

      const rows = ctx.derived.annualOverview || [];
      if (rows.length === 0) {
        if (root.__lastHtml !== '') {
          root.__lastHtml = '';
          root.innerHTML = '';
        }
        return;
      }

      const columns = getColumns('dashboardAnnual');
      const resolvers = getResolvers('dashboardAnnual');

      const html = `
        <div class="glass-card" style="padding:0; margin-bottom:20px;">
          <div class="collapsible-header" style="padding:14px 20px; cursor:default;">
            <div class="collapsible-header-title">
              <i data-lucide="calendar" style="width:16px;height:16px;"></i>
              <span>年度總覽（前後 2 年）</span>
            </div>
          </div>
          <div class="data-table-scroll-wrapper" style="border-top:1px solid rgba(255,255,255,0.05);">
            <table class="data-table">
              <thead>
                <tr>
                  ${columns.map((c) => `<th class="${c.type === 'number' ? 'num' : ''}">${c.label}</th>`).join('')}
                </tr>
              </thead>
              <tbody>
                ${rows.map((r) => `
                  <tr>
                    ${columns.map((c) => {
                      const val = r[c.id];
                      const resolver = resolvers[c.id];
                      const content = resolver ? resolver(val, r) : (val == null ? '—' : String(val));
                      return `<td class="${c.type === 'number' ? 'num' : ''}">${content}</td>`;
                    }).join('')}
                  </tr>
                `).join('')}
              </tbody>
            </table>
          </div>
        </div>
      `;

      if (root.__lastHtml !== html) {
        root.__lastHtml = html;
        root.innerHTML = html;
        if (window.lucide) window.lucide.createIcons();
      }
    };

    _paintAnnualOverview();

    _controls = renderPageControls({
      containerId: 'page-controls-root',
      headerContainerId: 'page-header-actions',
      items: [
        {
          type: 'custom',
          position: 'header',
          render: () => `<a class="btn btn-primary" href="annual-report.html"><i data-lucide="bar-chart-3"></i> 查看年度報表</a>`,
        },
      ],
    });

    const unsub = ctx.onDataChange((key) => {
      if (key === '__APP__') syncYear();
      _paintAnnualOverview();
    });

    return {
      destroy: () => {
        unsub();
        if (_controls) { try { _controls.destroy(); } catch (e) {} _controls = null; }
        const root = document.getElementById('annual-overview-root');
        if (root) { root.innerHTML = ''; root.__lastHtml = ''; }
      },
    };
  },
};

/* ============================================
   Helpers
   ============================================ */
function _flatten(raw) {
  const flat = [];
  Object.entries(raw || {}).forEach(([bankId, bank]) => {
    Object.entries(bank?.transactions || {}).forEach(([txnId, txn]) => {
      flat.push({ id: txnId, bankId, bankName: bank?.name || '', ...txn });
    });
  });
  return flat;
}

function _computeYearTotals(allIncome, allExpenses, year) {
  const yk = String(year || '');
  if (!yk) return { totalIncome: 0, totalExpense: 0, net: 0, monthCount: 0 };
  const yi = (allIncome || {})[yk] || {};
  let totalIncome = 0;
  Object.values(yi).forEach((md) => Object.values(md || {}).forEach((v) => { totalIncome += Number(v) || 0; }));
  const ye = (allExpenses || {})[yk] || {};
  let totalExpense = 0, monthCount = 0;
  Object.values(ye).forEach((md) => {
    const me = (md && md.member_expenses) || {};
    let s = 0;
    Object.values(me).forEach((items) => Object.values(items || {}).forEach((e) => { s += Number(e && e.amount) || 0; }));
    if (s > 0) monthCount++;
    totalExpense += s;
  });
  return {
    totalIncome: Math.round(totalIncome),
    totalExpense: Math.round(totalExpense),
    net: Math.round(totalIncome - totalExpense),
    monthCount,
  };
}

function _buildCards(totalBankBalance, accounts, year, yearTotals) {
  const list = Array.isArray(accounts) ? accounts : [];
  const bal = Number(totalBankBalance) || 0;
  const t = yearTotals || { totalIncome: 0, totalExpense: 0, net: 0, monthCount: 0 };
  return [
    { title: '家庭總餘額', value: formatHKD(bal), valueClass: bal >= 0 ? 'emerald' : 'red', hint: `共 ${list.length} 個銀行帳號`, icon: 'landmark' },
    { title: `${year} 家庭收入`, value: formatHKD(t.totalIncome), valueClass: 'emerald', hint: '家用轉入 + 額外收入', icon: 'trending-up' },
    { title: `${year} 年度總支出`, value: formatHKD(t.totalExpense), valueClass: 'red', hint: `${t.monthCount} 個月有紀錄`, icon: 'trending-down' },
    { title: `${year} 年度淨餘額`, value: formatHKD(t.net), valueClass: t.net >= 0 ? 'emerald' : 'red', hint: t.net >= 0 ? '收支平衡' : '支出大於收入', icon: 'wallet' },
  ];
}

function _computeAnnualOverview(allIncome, allExpenses, policies, currentYear) {
  const curY = Number(currentYear) || new Date().getFullYear();
  const startY = curY - 2;
  const endY = curY + 2;
  const rows = [];

  for (let y = startY; y <= endY; y++) {
    const yk = String(y);
    const yi = (allIncome || {})[yk] || {};
    const ye = (allExpenses || {})[yk] || {};

    let totalIncome = 0;
    Object.values(yi).forEach((md) => {
      Object.values(md || {}).forEach((v) => { totalIncome += Number(v) || 0; });
    });

    let totalExpense = 0;
    let monthsWithData = 0;
    Object.values(ye).forEach((md) => {
      const me = (md && md.member_expenses) || {};
      let monthSum = 0;
      Object.values(me).forEach((items) => {
        Object.values(items || {}).forEach((e) => { monthSum += Number(e && e.amount) || 0; });
      });
      if (monthSum > 0) monthsWithData++;
      totalExpense += monthSum;
    });

    let insurance = 0;
    (policies || []).forEach((p) => {
      try { insurance += getPolicyAnnualPremium(p, y) || 0; } catch (e) { /* noop */ }
    });

    const net = totalIncome - totalExpense - insurance;
    const avg = monthsWithData > 0 ? Math.round(totalExpense / monthsWithData) : 0;

    rows.push({
      year: y,
      totalIncome: Math.round(totalIncome),
      totalExpense: Math.round(totalExpense),
      insurance: Math.round(insurance),
      net: Math.round(net),
      avg,
      isCurrent: y === curY,
    });
  }

  return rows;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/dashboard.js
   Version: v103.0.35
   Batch: B29-7b
   ═══════════════════════════════════════════ */```

---

## 📄 js/pages/finance-overview.js

```
// finance-overview.js — 銀行交易（v103.0.35）
// v103.0.35（B29-7a）：
//   ✅ 刪除 syncSubtitle（動態副標題）
// ============================================
import { formatHKD } from '../lib/format.js';
import { calcTotalBankBalance, calcBankBalance } from '../lib/bank.js';
import { getColumns, getResolvers } from '../config/column-registry.js';
import { renderDataTable } from '../shared/data-table.js';
import { renderDataCard } from '../shared/data-card.js';
import { renderStatsCards } from '../shared/stats-cards.js';
import { renderPageControls } from '../lib/page-controls.js';
import { AppState } from '../core/state.js';
import { STORAGE_PREFIXES } from '../config/constants.js';

export default {
  title: '銀行交易',
  data: {
    bankAccounts:     { type: 'list', path: 'bank_accounts' },
    bankTransactions: { type: 'raw', path: 'bank_accounts', transform: _flatten },
  },
  state: {
    activeTab: 'banks',
    currentYear: String(AppState.year || new Date().getFullYear()),
    currentMonth: String(AppState.month || '12'),
    txFilters: { type: '', category: '', member: '' },
    view: 'table',
  },
  derived: {
    statsCards: {
      deps: ['data.bankAccounts', 'data.bankTransactions', 'state.currentYear', 'state.currentMonth'],
      compute: _buildStats,
    },
    filteredTxns: {
      deps: ['data.bankTransactions', 'state.txFilters', 'state.currentYear', 'state.currentMonth'],
      compute: _filterTxns,
    },
  },
  blocks: [],

  customMount: (ctx) => {
    let _statsApi = null, _tabH = null, _filterH = null;
    let _controls = null;
    let _view = 'table';

    try {
      const v = localStorage.getItem(`${STORAGE_PREFIXES.VIEW}finance-overview-view`);
      if (v === 'card' || v === 'table') _view = v;
    } catch (e) { /* noop */ }

    const syncState = () => {
      const y = String(AppState.year || new Date().getFullYear());
      const m = String(AppState.month || '12');
      if (ctx.state.currentYear !== y) ctx.state.currentYear = y;
      if (ctx.state.currentMonth !== m) ctx.state.currentMonth = m;
    };

    const render = () => {
      const statsRoot = document.getElementById('finance-stats-root');
      if (statsRoot) {
        if (_statsApi) { try { _statsApi.destroy(); } catch (e) {} }
        statsRoot.innerHTML = '';
        _statsApi = renderStatsCards({ container: 'finance-stats-root', cards: ctx.derived.statsCards || [], columns: 4 });
      }

      document.querySelectorAll('#finance-tabs .tab-btn').forEach((b) => b.classList.toggle('active', b.dataset.tab === ctx.state.activeTab));
      const bp = document.getElementById('finance-panel-banks');
      const tp = document.getElementById('finance-panel-transactions');
      if (bp) bp.style.display = ctx.state.activeTab === 'banks' ? 'block' : 'none';
      if (tp) tp.style.display = ctx.state.activeTab === 'transactions' ? 'block' : 'none';

      if (ctx.state.activeTab === 'banks') _paintBanks(ctx);
      else _paintTxns(ctx);
    };

    const _paintBanks = (ctx) => {
      const root = document.getElementById('finance-banks-root');
      if (!root) return;
      const accounts = ctx.data.bankAccounts || [];
      const txns = ctx.data.bankTransactions || [];
      const { year, month } = AppState.getYearMonth();
      const rows = accounts.map((acc) => {
        const accTxns = txns.filter((t) => t.bankId === acc.id);
        return { ...acc, _balance: calcBankBalance(acc, accTxns, year, month === 'all' ? '12' : month), _txnCount: accTxns.length };
      });
      if (rows.length === 0) {
        if (root.__lastHtml === '<empty>') return;
        root.__lastHtml = '<empty>';
        root.innerHTML = '<div class="glass-card"><div class="empty-state">尚無銀行帳號</div></div>';
        return;
      }
      root.__lastHtml = '';
      const columns = getColumns('bankAccounts');
      const resolvers = getResolvers('bankAccounts');
      if (_view === 'card') {
        renderDataCard({
          container: root,
          entityKey: '__bank_account__',
          rows,
          options: { gridClass: 'grid grid-3', columns, resolvers },
        });
      } else {
        renderDataTable({
          container: root,
          entityKey: '__bank_account__',
          rows,
          tableId: 'finance-banks-table',
          options: { columns, resolvers, storageKey: 'finance-banks-table' },
        });
      }
    };

    const _paintTxns = (ctx) => {
      const root = document.getElementById('finance-txn-root');
      if (!root) return;
      const rows = ctx.derived.filteredTxns || [];
      if (rows.length === 0) {
        if (root.__lastHtml === '<empty>') return;
        root.__lastHtml = '<empty>';
        root.innerHTML = '<div class="glass-card"><div class="empty-state">此條件下尚無交易記錄</div></div>';
        return;
      }
      root.__lastHtml = '';
      const columns = getColumns('bankTransactions');
      const resolvers = getResolvers('bankTransactions');
      if (_view === 'card') {
        renderDataCard({
          container: root,
          entityKey: '__bank_txn__',
          rows,
          options: { gridClass: 'grid grid-3', columns, resolvers },
        });
      } else {
        renderDataTable({
          container: root,
          entityKey: '__bank_txn__',
          rows,
          tableId: 'finance-txn-table',
          options: { columns, resolvers, storageKey: 'finance-txn-table' },
        });
      }
    };

    const tabsRoot = document.getElementById('finance-tabs');
    _tabH = (e) => {
      const btn = e.target.closest('button[data-tab]');
      if (!btn) return;
      ctx.state.activeTab = btn.dataset.tab;
      render();
    };
    tabsRoot?.addEventListener('click', _tabH);

    const filterRoot = document.getElementById('finance-txn-filter-root');
    if (filterRoot) {
      filterRoot.innerHTML = `<div class="page-filter-bar"><div class="filter-group"><label class="field-label">類型</label><select class="select" data-filter="type"><option value="">全部</option><option value="in">入帳</option><option value="out">出帳</option><option value="transfer">內部轉帳</option></select></div><div class="filter-group"><label class="field-label">分類</label><select class="select" data-filter="category"><option value="">全部</option><option value="contribution">家用轉入</option><option value="expense">支出</option><option value="insurance">保險</option><option value="reimbursement">代墊報銷</option><option value="manual">手動</option></select></div></div>`;
      _filterH = (e) => {
        const sel = e.target.closest('select[data-filter]');
        if (!sel) return;
        ctx.state.txFilters[sel.dataset.filter] = sel.value;
        render();
      };
      filterRoot.addEventListener('change', _filterH);
    }

    _controls = renderPageControls({
      containerId: 'page-controls-root',
      headerContainerId: 'page-header-actions',
      items: [
        {
          type: 'viewToggle',
          storageKey: 'finance-overview-view',
          defaultView: 'table',
          onChange: (v) => { _view = v; render(); },
        },
        { type: 'year' },
        { type: 'month' },
      ],
    });

    syncState();
    render();

    const unsub = ctx.onDataChange((key) => {
      if (key === '__APP__') {
        syncState();
        if (_controls) { try { _controls.refresh(); } catch (e) {} }
      }
      render();
    });

    return {
      destroy: () => {
        unsub();
        if (_statsApi) { try { _statsApi.destroy(); } catch (e) {} }
        if (_controls) { try { _controls.destroy(); } catch (e) {} }
        if (_tabH) tabsRoot?.removeEventListener('click', _tabH);
        if (_filterH) filterRoot?.removeEventListener('change', _filterH);
      },
    };
  },
};

function _flatten(raw) {
  const flat = [];
  Object.entries(raw || {}).forEach(([bankId, bank]) => {
    Object.entries(bank?.transactions || {}).forEach(([txnId, txn]) => {
      flat.push({ id: txnId, bankId, bankName: bank?.name || '', ...txn });
    });
  });
  return flat;
}

function _buildStats(accounts, txns, year, month) {
  const list = accounts || [], txnList = txns || [];
  const tM = month === 'all' ? '12' : (month || '12');
  const y = year || String(new Date().getFullYear());
  const total = calcTotalBankBalance(list, txnList, y, tM);
  const prefix = `${y}-${String(tM).padStart(2, '0')}`;
  const monthTxns = txnList.filter((t) => (t.date || '').startsWith(prefix));
  const inTotal = monthTxns.filter((t) => t.type === 'in').reduce((s, t) => s + (Number(t.amount) || 0), 0);
  const outTotal = monthTxns.filter((t) => t.type !== 'in').reduce((s, t) => s + (Number(t.amount) || 0), 0);
  return [
    { title: '家庭總餘額', value: formatHKD(total), valueClass: total >= 0 ? 'emerald' : 'red', hint: `共 ${list.length} 個銀行帳號`, icon: 'landmark' },
    { title: '本月入帳', value: formatHKD(inTotal), valueClass: 'emerald', hint: `${monthTxns.filter((t) => t.type === 'in').length} 筆`, icon: 'arrow-down-circle' },
    { title: '本月出帳', value: formatHKD(outTotal), valueClass: 'red', hint: `${monthTxns.filter((t) => t.type !== 'in').length} 筆`, icon: 'arrow-up-circle' },
    { title: '本月淨變動', value: formatHKD(inTotal - outTotal), valueClass: (inTotal - outTotal) >= 0 ? 'emerald' : 'red', hint: '入帳 − 出帳', icon: 'trending-up' },
  ];
}

function _filterTxns(txns, filters, year, month) {
  const list = txns || [];
  const y = year || String(new Date().getFullYear());
  const tM = month === 'all' ? null : (month || null);
  let out = list;
  if (tM) {
    const prefix = `${y}-${String(tM).padStart(2, '0')}`;
    out = out.filter((t) => (t.date || '').startsWith(prefix));
  } else {
    out = out.filter((t) => (t.date || '').startsWith(y));
  }
  if (filters.type) out = out.filter((t) => t.type === filters.type);
  if (filters.category) out = out.filter((t) => t.category === filters.category);
  if (filters.member) out = out.filter((t) => t.memberId === filters.member);
  return [...out].sort((a, b) => (b.date || '').localeCompare(a.date || ''));
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/finance-overview.js
   Version: v103.0.35
   Batch: B29-7a
   ═══════════════════════════════════════════ */```

---

## 📄 js/pages/input-center.js

```
// ============================================
// input-center.js — 綜合輸入中心（v103.0.35）
// ============================================
// v103.0.35（B35-0）：
//   ✅ 移除頂部「新增保單 / 新增基金」按鈕對應的 case（D-XXX）
//      - 入口改由「保險 / 基金 Tab」內的 entity-list-page 提供
//      - 移除已無用的 openEntityModal import
// v103.0.34（B29-4a）：
//   ✅ 刪除支出 Tab 內 banner
// v103.0.34（B29-3）：
//   ✅ columns / resolvers 改用 column-registry
// ============================================
import { initTabPanel } from '../ui/tab-panel.js';
import { initEntityListPage } from '../shared/entity-list-page.js';
import { renderDataTable } from '../shared/data-table.js';
import { renderDataCard } from '../shared/data-card.js';
import { openExpenseModal } from '../lib/expense-modal.js';
import { openContributionModal, openPersonalIncomeModal } from '../lib/income-modal.js';
import { openConfirm } from '../ui/modal.js';
import { showToast } from '../ui/toast.js';
import { initViewToggle } from '../ui/view-toggle.js';
import { listenAllExpenses, removeExpense } from '../core/db.js';
import { getColumns, getResolvers } from '../config/column-registry.js';
import { AppState } from '../core/state.js';
import { ENTITY_KEYS } from '../config/constants.js';

export default {
  title: '綜合輸入中心',
  data: {},
  state: { activeTab: 'expense', expenseView: 'table' },
  derived: {},
  blocks: [],

  customMount: (ctx) => {
    const TABS = [
      { key: 'expense', label: '支出', icon: 'receipt', panelId: 'ic-panel-expense' },
      { key: 'insurance', label: '保險', icon: 'shield', panelId: 'ic-panel-insurance' },
      { key: 'fund', label: '基金', icon: 'line-chart', panelId: 'ic-panel-fund' },
      { key: 'bank', label: '銀行', icon: 'landmark', panelId: 'ic-panel-bank' },
    ];
    const inst = {};
    let _btnH = null, _expUnsub = null, _expTableApi = null, _expCardApi = null;
    let _expToggle = null, _expRows = [];

    const loadTab = (key) => {
      if (inst[key]) { try { inst[key].refresh?.(); } catch (e) {} return; }
      const tabDef = TABS.find((t) => t.key === key);
      if (!tabDef) return;
      const panelEl = document.getElementById(tabDef.panelId);
      if (!panelEl) return;

      if (key === 'insurance') {
        inst[key] = initEntityListPage({ entity: ENTITY_KEYS.POLICY, containerId: tabDef.panelId, options: { defaultView: 'table', showViewToggle: true, storageKey: 'ic-insurance-view' } });
      } else if (key === 'fund') {
        inst[key] = initEntityListPage({ entity: ENTITY_KEYS.FUND, containerId: tabDef.panelId, options: { defaultView: 'table', showViewToggle: true, storageKey: 'ic-fund-view' } });
      } else if (key === 'bank') {
        inst[key] = initEntityListPage({ entity: ENTITY_KEYS.BANK, containerId: tabDef.panelId, options: { defaultView: 'table', showViewToggle: true, storageKey: 'ic-bank-view' } });
      } else if (key === 'expense') {
        /* 🆕 v103.0.34：移除 banner */
        panelEl.innerHTML = `
          <div class="flex flex-between items-center mb-16 flex-wrap gap-12">
            <div id="ic-expense-view-toggle"></div>
          </div>
          <div id="ic-expense-table-root"></div>
        `;
        _initExpenseViewToggle();
        _loadExpenses();
        inst[key] = {
          refresh: _loadExpenses,
          destroy: () => {
            if (_expUnsub) { try { _expUnsub(); } catch (e) {} _expUnsub = null; }
            _destroyExpenseLists();
            if (_expToggle) { try { _expToggle.destroy(); } catch (e) {} _expToggle = null; }
            panelEl.innerHTML = '';
          },
        };
      }
    };

    const _initExpenseViewToggle = () => {
      _expToggle = initViewToggle({
        containerId: 'ic-expense-view-toggle',
        storageKey: 'ic-expense-view',
        defaultView: 'table',
        onChange: (view) => { ctx.state.expenseView = view; _paintExpenses(); },
      });
      ctx.state.expenseView = _expToggle?.getView() || 'table';
    };

    const _destroyExpenseLists = () => {
      if (_expTableApi) { try { _expTableApi.destroy(); } catch (e) {} _expTableApi = null; }
      if (_expCardApi) { try { _expCardApi.destroy(); } catch (e) {} _expCardApi = null; }
    };

    const _loadExpenses = () => {
      if (_expUnsub) { try { _expUnsub(); } catch (e) {} _expUnsub = null; }
      _expUnsub = listenAllExpenses((rows) => {
        const { year } = AppState.getYearMonth();
        _expRows = (rows || []).filter((r) => String(r.year) === String(year)).sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0)).slice(0, 50);
        _paintExpenses();
      });
    };

    const _hooks = (canInput) => ({
      customActions: canInput ? (row) => [
        { label: '編輯', icon: 'pencil', className: 'btn-ghost', action: 'edit', onClick: () => openExpenseModal({ row, onSuccess: _loadExpenses }) },
        { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
          const ok = await openConfirm(`確定要刪除「${row.name}」嗎？`, { okText: '刪除', okClass: 'btn-danger' });
          if (!ok) return;
          try { await removeExpense(row.year, row.month, row.memberId, row.id); showToast('✅ 已刪除', 'success'); }
          catch (err) { showToast('刪除失敗：' + err.message, 'error'); }
        }},
      ] : () => [],
    });

    const _paintExpenses = () => {
      const root = document.getElementById('ic-expense-table-root');
      if (!root) return;
      _destroyExpenseLists();

      if (_expRows.length === 0) {
        root.innerHTML = '<div class="glass-card"><div class="empty-state">尚無支出紀錄</div></div>';
        return;
      }
      const canInput = AppState.getCanInput();
      const columns = getColumns('recentExpense');
      const resolvers = getResolvers('recentExpense');

      if (ctx.state.expenseView === 'card') {
        _expCardApi = renderDataCard({
          container: root,
          entityKey: '__recent_expense__',
          rows: _expRows,
          options: { gridClass: 'grid grid-3', columns, resolvers },
          hooks: _hooks(canInput),
        });
      } else {
        _expTableApi = renderDataTable({
          container: root,
          entityKey: '__recent_expense__',
          rows: _expRows,
          tableId: 'ic-recent-expense-table',
          options: { columns, resolvers, storageKey: 'ic-recent-expense-table' },
          hooks: _hooks(canInput),
        });
      }
    };

    const tabPanel = initTabPanel({
      containerId: 'ic-tab-bar-root',
      tabs: TABS,
      defaultKey: 'expense',
      storageKey: 'input-center-tab',
      onChange: (key) => { ctx.state.activeTab = key; loadTab(key); },
    });
    loadTab(tabPanel?.getCurrent() || 'expense');

    const btnRoot = document.getElementById('ic-buttons-root');
    if (btnRoot) {
      _btnH = (e) => {
        const btn = e.target.closest('button[data-action]');
        if (!btn) return;
        switch (btn.dataset.action) {
          case 'add-expense': openExpenseModal({ onSuccess: _loadExpenses }); break;
          case 'add-contribution': openContributionModal({ onSuccess: _loadExpenses }); break;
          case 'add-personal-income': openPersonalIncomeModal(); break;
        }
      };
      btnRoot.addEventListener('click', _btnH);
    }

    return {
      destroy: () => {
        Object.values(inst).forEach((i) => { try { i?.destroy?.(); } catch (e) {} });
        try { tabPanel?.destroy(); } catch (e) {}
        if (_btnH && btnRoot) { try { btnRoot.removeEventListener('click', _btnH); } catch (e) {} }
      },
    };
  },
};

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/input-center.js
   Version: 1792200000
   Batch: B35-0
   Writer: A20261016-001#002
   ═══════════════════════════════════════════ */```

---

## 📄 js/pages/insurance.js

```
// insurance.js — 保險清單表（v103.0.37）
// v103.0.37（B31-1）：
//   ✅ 修正：payments 快取失效 — 改為訂閱 insurance_payments 節點
//      - 移除 _lastIds id 快取邏輯（按扣款後不刷新）
// v103.0.36（B30-8）：
//   ✅ 保單卡片加「已供款總額」與「剩餘供款」兩欄
// ============================================
import { formatHKD } from '../lib/format.js';
import { esc } from '../lib/dom.js';
import { computeEnrichedPolicies, calcProgress, getPolicyHolderId } from '../lib/insurance.js';
import { familyRef } from '../core/db.js';
import { onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { resolveName } from '../config/entity-registry.js';
import { getColumns, getResolvers } from '../config/column-registry.js';
import { AppState } from '../core/state.js';
import { openEntityModal } from '../entity/entity-modal.js';
import { deleteEntity } from '../entity/entity-helpers.js';
import { openConfirm } from '../ui/modal.js';
import { showToast } from '../ui/toast.js';
import { renderStatsCards } from '../shared/stats-cards.js';
import { renderDataTable } from '../shared/data-table.js';
import { renderDataCard } from '../shared/data-card.js';
import { renderPageControls } from '../lib/page-controls.js';
import { ENTITY_KEYS, STORAGE_PREFIXES } from '../config/constants.js';

export default {
  title: '保險清單表',
  data: { policies: { type: 'list', path: 'insurance_policies' } },
  state: { view: 'card' },
  derived: {},
  blocks: [],

  customMount: (ctx) => {
    let _payments = {};
    let _statsApi = null;
    let _tableApi = null;
    let _cardApi = null;
    let _clickHandler = null;
    let _toggleHandler = null;
    let _controls = null;
    let _payUnsub = null;
    let _view = 'card';

    try {
      const saved = localStorage.getItem(`${STORAGE_PREFIXES.VIEW}insurance-view`);
      if (saved === 'card' || saved === 'table') _view = saved;
    } catch (e) { /* noop */ }

    const render = () => {
      const policies = ctx.data.policies || [];
      const { year } = AppState.getYearMonth();
      const targetYear = Number(year) || new Date().getFullYear();
      const enriched = computeEnrichedPolicies(policies, _payments, targetYear);

      const statsRoot = document.getElementById('insurance-stats-root');
      if (statsRoot) {
        if (_statsApi) { try { _statsApi.destroy(); } catch (e) {} }
        statsRoot.innerHTML = '';
        const totalPaid = enriched.reduce((s, p) => s + (p._paidTotal || 0), 0);
        _statsApi = renderStatsCards({
          container: 'insurance-stats-root',
          cards: [
            { title: `${targetYear} 年度保單總供款`, value: formatHKD(enriched.reduce((s, p) => s + (p._currentAnnualPremium || 0), 0)), valueClass: 'cyan', hint: `共 ${enriched.length} 張保單`, icon: 'wallet' },
            { title: '累計已供款總額', value: formatHKD(totalPaid), valueClass: 'emerald', hint: '所有保單的已扣款總和', icon: 'check-circle' },
            { title: '供款中保單', value: `${enriched.filter((p) => !p._isCompleted).length} 張`, valueClass: 'emerald', hint: '未供滿的保單數量', icon: 'shield' },
            { title: '已供滿保單', value: `${enriched.filter((p) => p._isCompleted).length} 張`, valueClass: 'emerald', hint: '已完成供款年期', icon: 'check-circle' },
          ],
          columns: 4,
        });
      }

      _paintSection(enriched.filter((p) => p._isCompleted));
      _paintActive(enriched.filter((p) => !p._isCompleted), policies.length);
      if (window.lucide) window.lucide.createIcons();
    };

    const _hooks = (canInput) => ({
      customActions: canInput ? (row) => [
        { label: '編輯', icon: 'pencil', className: 'btn-ghost', action: 'edit', onClick: () => _onEdit(row) },
        { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: () => _onDelete(row) },
      ] : () => [],
    });

    const _paintSection = (list) => {
      const s = document.getElementById('completed-section');
      const g = document.getElementById('completed-grid');
      const c = document.getElementById('completed-count');
      if (!s || !g) return;
      if (list.length === 0) { s.style.display = 'none'; return; }
      s.style.display = 'block';
      if (c) c.textContent = String(list.length);
      const html = list.map(_cardHtml).join('');
      if (g.__lastHtml === html) return;
      g.__lastHtml = html;
      g.innerHTML = html;
    };

    const _paintActive = (active, total) => {
      const cardEl = document.getElementById('insurance-card-view');
      const tableEl = document.getElementById('insurance-table-view');
      const emptyEl = document.getElementById('insurance-empty-state');
      if (!cardEl || !tableEl) return;
      if (total === 0) {
        if (emptyEl) emptyEl.style.display = 'block';
        cardEl.style.display = 'none';
        tableEl.style.display = 'none';
        return;
      }
      if (emptyEl) emptyEl.style.display = 'none';

      if (_tableApi) { try { _tableApi.destroy(); } catch (e) {} _tableApi = null; }
      if (_cardApi) { try { _cardApi.destroy(); } catch (e) {} _cardApi = null; }

      if (_view === 'table') {
        cardEl.style.display = 'none';
        tableEl.style.display = 'block';
        if (active.length === 0) {
          tableEl.innerHTML = '<div class="glass-card"><div class="empty-state">所有保單均已供滿</div></div>';
          return;
        }
        const canInput = AppState.getCanInput();
        const columns = getColumns('insurancePolicies');
        const resolvers = getResolvers('insurancePolicies');
        _tableApi = renderDataTable({
          container: tableEl,
          entityKey: ENTITY_KEYS.POLICY,
          rows: active,
          tableId: 'insurance-table',
          options: { columns, resolvers, storageKey: 'insurance-table' },
          hooks: _hooks(canInput),
        });
      } else {
        cardEl.style.display = 'block';
        tableEl.style.display = 'none';
        const html = active.length === 0
          ? '<div class="glass-card" style="text-align:center;padding:40px 20px;"><p style="color:var(--neon-emerald);">🎉 所有保單均已供滿</p></div>'
          : `<div class="grid grid-3">${active.map((p) => `<div class="glass-card policy-card">${_cardHtml(p)}</div>`).join('')}</div>`;
        if (cardEl.__lastHtml !== html) {
          cardEl.__lastHtml = html;
          cardEl.innerHTML = html;
        }
      }
    };

    const _cardHtml = (p) => {
      const holderId = getPolicyHolderId(p);
      const holder = resolveName('members', holderId) || holderId || '—';
      const insured = resolveName('members', p.memberId) || p.memberId || '—';
      const isFund = p.type === 'fund_insurance';
      const total = p.totalPolicyPeriods || 0;
      const done = p.completedPeriods || 0;
      const pct = calcProgress(p);
      const canInput = AppState.getCanInput();

      const paidTotal = p._paidTotal || 0;
      const totalPremium = p._totalPremium || 0;
      const remaining = Math.max(0, totalPremium - paidTotal);

      const actions = canInput ? `<div class="policy-actions"><button type="button" class="btn btn-sm btn-ghost" data-action="edit-policy" data-id="${p.id}"><i data-lucide="pencil" style="width:14px;height:14px;"></i> 編輯</button><button type="button" class="btn btn-sm btn-danger" data-action="delete-policy" data-id="${p.id}"><i data-lucide="trash-2" style="width:14px;height:14px;"></i> 刪除</button></div>` : '';
      const progressHtml = isFund ? '' : `<div class="policy-progress" style="margin-top:12px;"><div class="policy-progress-text"><span>整體供款進度</span><span>${done} / ${total} 期 (${pct}%)</span></div><div class="progress"><div class="progress-bar" style="width:${pct}%;"></div></div></div>`;

      return `<div class="policy-header"><div><div class="policy-name">${esc(p.name || '')}</div><div class="policy-company">${esc(p.company || '')} · ${isFund ? '基金保險' : '普通保險'}</div></div></div>
        <div class="policy-info-grid">
          <div class="policy-info-item"><span class="policy-info-label">保單持有人</span><span class="policy-info-value">${esc(holder)}</span></div>
          <div class="policy-info-item"><span class="policy-info-label">受保人</span><span class="policy-info-value">${esc(insured)}</span></div>
        </div>
        <div class="policy-info-grid" style="margin-top:10px;padding-top:10px;border-top:1px dashed rgba(255,255,255,0.08);">
          <div class="policy-info-item"><span class="policy-info-label">本期年繳</span><span class="policy-info-value text-cyan">${formatHKD(p._currentAnnualPremium)}</span></div>
          <div class="policy-info-item"><span class="policy-info-label">保單總供款</span><span class="policy-info-value text-magenta">${formatHKD(totalPremium)}</span></div>
        </div>
        <div class="policy-info-grid" style="margin-top:10px;padding-top:10px;border-top:1px dashed rgba(255,255,255,0.08);">
          <div class="policy-info-item"><span class="policy-info-label">已供款總額</span><span class="policy-info-value text-emerald" style="font-weight:700;">${formatHKD(paidTotal)}</span></div>
          <div class="policy-info-item"><span class="policy-info-label">剩餘供款</span><span class="policy-info-value">${formatHKD(remaining)}</span></div>
        </div>${progressHtml}${actions}`;
    };

    const _onEdit = (row) => {
      openEntityModal({ entity: ENTITY_KEYS.POLICY, mode: 'edit', id: row.id, allRows: ctx.data.policies || [] });
    };

    const _onDelete = async (row) => {
      const ok = await openConfirm(`⚠️ 確定要刪除保單「${row.name}」嗎？`, { title: '刪除保單', okText: '刪除', okClass: 'btn-danger' });
      if (!ok) return;
      try { await deleteEntity(ENTITY_KEYS.POLICY, row.id, getPolicyHolderId(row)); showToast('✅ 已刪除保單', 'success'); }
      catch (err) { showToast('刪除失敗：' + err.message, 'error'); }
    };

    _clickHandler = async (e) => {
      const btn = e.target.closest('button[data-action]');
      if (!btn || !AppState.getCanInput()) return;
      const policies = ctx.data.policies || [];
      const policy = policies.find((p) => p.id === btn.dataset.id);
      if (!policy) return;
      if (btn.dataset.action === 'edit-policy') _onEdit(policy);
      else if (btn.dataset.action === 'delete-policy') _onDelete(policy);
    };
    document.getElementById('insurance-card-view')?.addEventListener('click', _clickHandler);
    document.getElementById('completed-grid')?.addEventListener('click', _clickHandler);

    const headerEl = document.getElementById('completed-header');
    const bodyEl = document.getElementById('completed-body');
    const sectionEl = document.getElementById('completed-section');
    _toggleHandler = () => {
      if (!bodyEl || !sectionEl) return;
      const open = bodyEl.style.display === 'block';
      bodyEl.style.display = open ? 'none' : 'block';
      sectionEl.classList.toggle('open', !open);
    };
    if (headerEl) headerEl.addEventListener('click', _toggleHandler);

    _controls = renderPageControls({
      containerId: 'page-controls-root',
      headerContainerId: 'page-header-actions',
      items: [
        {
          type: 'add',
          label: '新增保單',
          onClick: () => openEntityModal({ entity: ENTITY_KEYS.POLICY, mode: 'add' }),
        },
        {
          type: 'viewToggle',
          storageKey: 'insurance-view',
          defaultView: 'card',
          onChange: (v) => { _view = v; render(); },
        },
        { type: 'year' },
        { type: 'month' },
      ],
    });

    const addFirstBtn = document.getElementById('add-first-policy-btn');
    const onAddFirst = () => openEntityModal({ entity: ENTITY_KEYS.POLICY, mode: 'add' });
    if (addFirstBtn) addFirstBtn.addEventListener('click', onAddFirst);

    /* ============================================
       🆕 v103.0.37（B31-1）：訂閱 insurance_payments
       -------------------------------------------------
       原因：原本用 _lastIds 快取 policy id 列表，
            但 insurance_payments 更新時 id 未變，
            導致「已供款總額 / 剩餘供款」不刷新。
       解：改為直接訂閱 insurance_payments 節點，
          任何變更（含 settlements 頁扣款）都觸發。
       ============================================ */
    try {
      _payUnsub = onValue(familyRef('insurance_payments'), (snap) => {
        _payments = snap.val() || {};
        render();
      }, (err) => {
        console.warn('[insurance] 訂閱 payments 失敗：', err);
      });
    } catch (e) {
      console.warn('[insurance] 無法訂閱 payments：', e);
    }

    const unsub = ctx.onDataChange((key) => {
      if (key === '__APP__' && _controls) {
        try { _controls.refresh(); } catch (e) {}
      }
      render();
    });

    render();

    return {
      destroy: () => {
        unsub();
        if (_payUnsub) { try { _payUnsub(); } catch (e) {} _payUnsub = null; }
        if (_statsApi) { try { _statsApi.destroy(); } catch (e) {} }
        if (_tableApi) { try { _tableApi.destroy(); } catch (e) {} }
        if (_cardApi) { try { _cardApi.destroy(); } catch (e) {} }
        if (_controls) { try { _controls.destroy(); } catch (e) {} }
        if (_clickHandler) {
          document.getElementById('insurance-card-view')?.removeEventListener('click', _clickHandler);
          document.getElementById('completed-grid')?.removeEventListener('click', _clickHandler);
        }
        if (_toggleHandler && headerEl) headerEl.removeEventListener('click', _toggleHandler);
        if (addFirstBtn) addFirstBtn.removeEventListener('click', onAddFirst);
      },
    };
  },
};

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/insurance.js
   Version: v103.0.37
   Batch: B31-1
   ═══════════════════════════════════════════ */
```

---

## 📄 js/pages/member-report.js

```
// member-report.js — 成員收支明細（v103.0.35）
// v103.0.35（B29-7b）：
//   ✅ 刪除 syncSubtitle（動態副標題）
// ============================================
import { formatHKD } from '../lib/format.js';
import { AppState } from '../core/state.js';
import { RESERVED_IDS, STORAGE_PREFIXES } from '../config/constants.js';
import { getColumns, getResolvers } from '../config/column-registry.js';
import { renderDataTable } from '../shared/data-table.js';
import { renderDataCard } from '../shared/data-card.js';
import { renderPageControls } from '../lib/page-controls.js';

export default {
  title: '成員與家庭收入與支出明細',
  data: {
    members:           { type: 'list', path: 'members' },
    allIncome:         { type: 'raw', path: 'income' },
    allExpenses:       { type: 'raw', path: 'expenses' },
    allPersonalIncome: { type: 'raw', path: 'personal_income' },
  },
  state: { currentYear: String(AppState.year || new Date().getFullYear()), view: 'table' },
  derived: {
    rows: { deps: ['data.members', 'data.allIncome', 'data.allExpenses', 'data.allPersonalIncome', 'state.currentYear'], compute: _buildRows },
    statsCards: { deps: ['rows', 'state.currentYear'], compute: _buildStats },
  },
  blocks: [{ type: 'stats', container: 'member-report-stats-root', cards: '$.statsCards' }],

  customMount: (ctx) => {
    let _tableApi = null, _cardApi = null;
    let _controls = null;
    let _view = 'table';

    try {
      const v = localStorage.getItem(`${STORAGE_PREFIXES.VIEW}member-report-view`);
      if (v === 'card' || v === 'table') _view = v;
    } catch (e) { /* noop */ }

    const paint = () => {
      const root = document.getElementById('member-list-root');
      if (!root) return;
      if (_tableApi) { try { _tableApi.destroy(); } catch (e) {} _tableApi = null; }
      if (_cardApi) { try { _cardApi.destroy(); } catch (e) {} _cardApi = null; }

      const rows = ctx.derived.rows || [];
      const columns = getColumns('memberReport');
      const resolvers = getResolvers('memberReport');

      if (_view === 'card') {
        _cardApi = renderDataCard({
          container: root,
          entityKey: '__member_report__',
          rows,
          options: { gridClass: 'grid grid-3', columns, resolvers },
        });
      } else {
        _tableApi = renderDataTable({
          container: root,
          entityKey: '__member_report__',
          rows,
          tableId: 'member-report-table',
          options: { columns, resolvers, storageKey: 'member-report-table' },
        });
      }
    };

    _controls = renderPageControls({
      containerId: 'page-controls-root',
      headerContainerId: 'page-header-actions',
      items: [
        {
          type: 'viewToggle',
          storageKey: 'member-report-view',
          defaultView: 'table',
          onChange: (v) => { _view = v; paint(); },
        },
        { type: 'year' },
        { type: 'month' },
      ],
    });

    paint();

    const unsub = ctx.onDataChange((key) => {
      if (key === '__APP__') {
        const y = String(AppState.year || new Date().getFullYear());
        if (ctx.state.currentYear !== y) ctx.state.currentYear = y;
        if (_controls) { try { _controls.refresh(); } catch (e) {} }
      }
      paint();
    });

    return {
      destroy: () => {
        unsub();
        if (_tableApi) { try { _tableApi.destroy(); } catch (e) {} }
        if (_cardApi) { try { _cardApi.destroy(); } catch (e) {} }
        if (_controls) { try { _controls.destroy(); } catch (e) {} }
      },
    };
  },
};

function _buildRows(members, allIncome, allExpenses, allPersonalIncome, year) {
  const yk = String(year || '');
  const yi = (allIncome || {})[yk] || {};
  const ye = (allExpenses || {})[yk] || {};
  const list = (members || []).map((m) => ({ id: m.id, name: m.name, isShared: false }));
  list.push({ id: RESERVED_IDS.SHARED_MEMBER, name: '家庭共用', isShared: true });

  return list.map((m) => {
    let income = 0, personalIncome = 0, expense = 0, insurance = 0;
    if (!m.isShared) {
      Object.values(yi).forEach((md) => { income += Number(md && md[m.id]) || 0; });
      const mp = ((allPersonalIncome || {})[m.id] || {})[yk] || {};
      Object.values(mp).forEach((v) => { personalIncome += Number(v) || 0; });
    }
    Object.values(ye).forEach((md) => {
      const me = (md && md.member_expenses && md.member_expenses[m.id]) || {};
      Object.values(me).forEach((e) => {
        const amt = Number(e && e.amount) || 0;
        if (e && e.isAutoLinked) insurance += amt; else expense += amt;
      });
    });
    return { ...m, income: Math.round(income), expense: Math.round(expense), insurance: Math.round(insurance), personalIncome: Math.round(personalIncome), net: Math.round(income + personalIncome - expense - insurance) };
  });
}

function _buildStats(rows, year) {
  const list = rows || [];
  const tI = list.reduce((s, r) => s + r.income, 0);
  const tPI = list.reduce((s, r) => s + r.personalIncome, 0);
  const tE = list.reduce((s, r) => s + r.expense, 0);
  const tIns = list.reduce((s, r) => s + r.insurance, 0);
  const net = tI + tPI - tE - tIns;
  return [
    { title: `${year} 家庭收入`, value: formatHKD(tI), valueClass: 'emerald', hint: '家用轉入加總', icon: 'trending-up' },
    { title: `${year} 個人收入`, value: formatHKD(tPI), valueClass: 'cyan', hint: '成員個人收入加總', icon: 'wallet' },
    { title: `${year} 年度總支出`, value: formatHKD(tE), valueClass: 'red', hint: '含家庭共用支出', icon: 'trending-down' },
    { title: `${year} 年度保險平攤`, value: formatHKD(tIns), valueClass: 'magenta', hint: '保險自動分攤加總', icon: 'shield' },
    { title: `${year} 年度淨餘額`, value: formatHKD(net), valueClass: net >= 0 ? 'emerald' : 'red', hint: '收入 + 個人 − 支出 − 保險', icon: 'calculator' },
  ];
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/member-report.js
   Version: v103.0.35
   Batch: B29-7b
   ═══════════════════════════════════════════ */```

---

## 📄 js/pages/portfolio.js

```
// ============================================
// portfolio.js — 基金投資表（v103.0.34）
// ============================================
// v103.0.34（B35-2）：
//   ✅ 修：歷史快照刪除失效（舊格式）— 傳 _rawPath
//   ✅ 新增：導入快照按鈕（貼 JSON / 上傳檔案）
// v103.0.33（B33-1）：
//   ✅ 快照改為以估值日（YYYY-MM-DD）為 key
//   ✅ 卡片顯示最新一筆快照（單位數 / 價格 USD / 總值 USD / 保單價值 HKD）
//   ✅ 支援歷史快照展開、可刪除
//   ✅ 支援任選兩筆快照對比
//   ✅ 成本累積邏輯不動（由保險保單自動分配）
// ============================================
import { formatHKD } from '../lib/format.js';
import { esc } from '../lib/dom.js';
import { openEntityModal } from '../entity/entity-modal.js';
import { deleteEntity } from '../entity/entity-helpers.js';
import { openConfirm, openModal, closeModal } from '../ui/modal.js';
import { buildForm } from '../ui/form-builder.js';
import { showToast } from '../ui/toast.js';
import { saveFundSnapshot, removeFundSnapshot, getFundSnapshotListOnce } from '../core/db.js';
import { AppState } from '../core/state.js';
import { renderPageControls } from '../lib/page-controls.js';
import { ENTITY_KEYS } from '../config/constants.js';
import { calcFundCostEstimate } from '../lib/insurance.js';

const SNAP_MODAL_ID = 'fund-snapshot-modal';
const HISTORY_MODAL_ID = 'fund-history-modal';
const IMPORT_MODAL_ID = 'fund-import-snapshot-modal';
const DEFAULT_FX = 7.8;

export default {
  title: '基金投資表',
  data: {
    funds: { type: 'list', path: 'funds' },
    policies: { type: 'list', path: 'insurance_policies' },
  },
  state: {},
  derived: {
    insuranceFunds: {
      deps: ['data.funds'],
      compute: (funds) => (funds || []).filter((f) => f.type === 'insurance'),
    },
    standaloneFunds: {
      deps: ['data.funds'],
      compute: (funds) => (funds || []).filter((f) => f.type !== 'insurance'),
    },
    statsCards: {
      deps: ['data.funds', 'data.policies'],
      compute: (funds, policies) => _buildStats(funds, policies),
    },
  },
  blocks: [
    { type: 'stats', container: 'portfolio-stats-root', cards: '$.statsCards' },
  ],

  customMount: (ctx) => {
    let _controls = null;
    let _importHandler = null;

    const headerEl = document.getElementById('page-header-actions');
    if (headerEl) {
      headerEl.innerHTML = `
        <button type="button" class="btn btn-magenta" id="portfolio-import-btn">
          <i data-lucide="upload"></i> 導入快照
        </button>
      `;
      _importHandler = () => {
        _openImportSnapshotModal(ctx.data.funds || [], render);
      };
      document.getElementById('portfolio-import-btn')?.addEventListener('click', _importHandler);
      if (window.lucide) window.lucide.createIcons();
    }

    const render = () => {
      _paintInsurance(ctx);
      _paintStandalone(ctx);
      if (window.lucide) window.lucide.createIcons();
    };

    const unsub = ctx.onDataChange((key) => {
      if (key.startsWith('data.funds') || key.startsWith('data.policies') || key === '__APP__') {
        if (key === '__APP__' && _controls) {
          try { _controls.refresh(); } catch (e) {}
        }
        render();
      }
    });

    render();

    _controls = renderPageControls({
      containerId: 'page-controls-root',
      items: [
        {
          type: 'add',
          label: '新增基金',
          onClick: () => openEntityModal({ entity: ENTITY_KEYS.FUND, mode: 'add', onSuccess: render }),
        },
        {
          type: 'year',
          year: AppState.year,
          onChange: (y) => AppState.setYearMonth(y, AppState.month === 'all' ? '01' : AppState.month),
        },
        {
          type: 'month',
          month: AppState.month === 'all' ? '01' : AppState.month,
          onChange: (m) => AppState.setYearMonth(AppState.year, m),
        },
      ],
    });

    const addFirstBtn = document.getElementById('add-first-fund-btn');
    const onAddFirst = () => openEntityModal({ entity: ENTITY_KEYS.FUND, mode: 'add', onSuccess: render });
    if (addFirstBtn) addFirstBtn.addEventListener('click', onAddFirst);

    const cardClickHandler = async (e) => {
      const btn = e.target.closest('button[data-action]');
      if (!btn) return;
      const id = btn.dataset.id;
      if (!id) return;
      const funds = ctx.data.funds || [];
      const fund = funds.find((f) => f.id === id);
      if (!fund) return;

      if (btn.dataset.action === 'edit-fund') {
        openEntityModal({ entity: ENTITY_KEYS.FUND, mode: 'edit', id, allRows: funds, onSuccess: render });
      } else if (btn.dataset.action === 'delete-fund') {
        const ok = await openConfirm(`確定要刪除基金「${fund.name}」嗎？`, { title: '刪除基金', okText: '刪除', okClass: 'btn-danger' });
        if (!ok) return;
        try { await deleteEntity(ENTITY_KEYS.FUND, id); showToast('✅ 已刪除基金', 'success'); }
        catch (err) { showToast('刪除失敗：' + err.message, 'error'); }
      }
    };

    const snapshotHandler = async (e) => {
      const btn = e.target.closest('button[data-action="snapshot"]');
      if (btn) {
        const fundId = btn.dataset.id;
        const fundName = btn.dataset.name || '';
        if (fundId) await _openSnapshotModal(fundId, fundName, render);
        return;
      }
      const histBtn = e.target.closest('button[data-action="history"]');
      if (histBtn) {
        const fundId = histBtn.dataset.id;
        const fundName = histBtn.dataset.name || '';
        if (fundId) await _openHistoryModal(fundId, fundName, render);
      }
    };

    const insRoot = document.getElementById('insurance-funds-list');
    const stdRoot = document.getElementById('standalone-funds-list');

    insRoot?.addEventListener('click', cardClickHandler);
    stdRoot?.addEventListener('click', cardClickHandler);
    document.addEventListener('click', snapshotHandler);

    return {
      destroy: () => {
        unsub();
        if (_controls) { try { _controls.destroy(); } catch (e) {} }
        if (addFirstBtn) addFirstBtn.removeEventListener('click', onAddFirst);
        insRoot?.removeEventListener('click', cardClickHandler);
        stdRoot?.removeEventListener('click', cardClickHandler);
        document.removeEventListener('click', snapshotHandler);
        if (_importHandler && headerEl) {
          document.getElementById('portfolio-import-btn')?.removeEventListener('click', _importHandler);
          headerEl.innerHTML = '';
          _importHandler = null;
        }
      },
    };
  },
};

/* ============================================
   反查機制：找出基金對應的保單
   ============================================ */
function _findPolicyByFund(fund, policies) {
  if (fund.policyId) {
    const p = (policies || []).find((x) => x.id === fund.policyId);
    if (p) return p;
  }
  return (policies || []).find((p) =>
    (p.fundsAllocation || []).some((a) => a.fundId === fund.id)
  ) || null;
}

/* ============================================
   為單一保單計算所有基金的投入成本（邏輯不動）
   ============================================ */
function _getCostMapForPolicy(policy, allFunds, targetYear, targetMonth) {
  const costMap = {};
  const policyFunds = (allFunds || []).filter(
    (f) => f.type === 'insurance' && _findPolicyByFund(f, [policy]) !== null
  );
  if (policyFunds.length === 0) return costMap;

  const totalPolicyCost = calcFundCostEstimate(policy, targetYear, targetMonth);
  const manualFunds = policyFunds.filter((f) => Number(f.cost) > 0);
  const manualTotal = manualFunds.reduce((s, f) => s + Number(f.cost), 0);
  manualFunds.forEach((f) => { costMap[f.id] = Number(f.cost); });

  const remainingCost = Math.max(0, totalPolicyCost - manualTotal);
  const dynamicFunds = policyFunds.filter((f) => Number(f.cost) <= 0);
  const dynamicTotalPct = dynamicFunds.reduce((s, f) => {
    const alloc = (policy.fundsAllocation || []).find((a) => a.fundId === f.id);
    return s + (alloc ? Number(alloc.pct) : 0);
  }, 0);

  dynamicFunds.forEach((f) => {
    const alloc = (policy.fundsAllocation || []).find((a) => a.fundId === f.id);
    const pct = alloc ? Number(alloc.pct) : 0;
    costMap[f.id] = dynamicTotalPct > 0 ? Math.round(remainingCost * (pct / dynamicTotalPct)) : 0;
  });

  return costMap;
}

/* ============================================
   保險基金區
   ============================================ */
function _paintInsurance(ctx) {
  const root = document.getElementById('insurance-funds-list');
  if (!root) return;
  const list = ctx.derived.insuranceFunds || [];
  if (list.length === 0) {
    root.innerHTML = '<div class="glass-card"><div class="empty-state">尚無保險基金</div></div>';
    return;
  }
  const policies = ctx.data.policies || [];
  const ym = AppState.getYearMonth();
  const y = ym.year || String(new Date().getFullYear());
  const m = ym.month === 'all' ? String(new Date().getMonth() + 1).padStart(2, '0') : ym.month;

  const costMaps = {};
  policies.forEach((p) => { costMaps[p.id] = _getCostMapForPolicy(p, list, y, m); });

  const rows = list.map((f) => {
    const policy = _findPolicyByFund(f, policies);
    const alloc = (policy?.fundsAllocation || []).find((a) => a.fundId === f.id);
    const pct = alloc ? alloc.pct : 0;
    const dynamicCost = (policy && costMaps[policy.id]) ? (costMaps[policy.id][f.id] || 0) : (Number(f.cost) || 0);
    return { fund: f, policyName: policy?.name || '（未關聯）', pct, dynamicCost };
  });
  const html = rows.map((r) => _cardHtml(r, true)).join('');
  if (root.__lastHtml === html) return;
  root.__lastHtml = html;
  root.innerHTML = html;

  /* 非同步填充最新快照 */
  rows.forEach((r) => _fillLatestSnapshot(r.fund.id));
}

/* ============================================
   獨立基金區
   ============================================ */
function _paintStandalone(ctx) {
  const root = document.getElementById('standalone-funds-list');
  if (!root) return;
  const list = ctx.derived.standaloneFunds || [];
  const empty = document.getElementById('fund-empty-state');
  if (list.length === 0) {
    if (empty) empty.style.display = 'block';
    root.innerHTML = '';
    root.__lastHtml = '';
    return;
  }
  if (empty) empty.style.display = 'none';
  const html = `<div class="data-cards-grid">${list.map((f) => _cardHtml({ fund: f }, false)).join('')}</div>`;
  if (root.__lastHtml === html) return;
  root.__lastHtml = html;
  root.innerHTML = html;

  list.forEach((f) => _fillLatestSnapshot(f.id));
}

/* ============================================
   非同步填充最新快照（單位數 / 價格 / 總值 / 保單價值）
   ============================================ */
async function _fillLatestSnapshot(fundId) {
  const placeholder = document.querySelector(`[data-snap-latest="${fundId}"]`);
  if (!placeholder) return;
  try {
    const snaps = await getFundSnapshotListOnce(fundId);
    const latest = snaps[snaps.length - 1];
    if (!latest) {
      placeholder.innerHTML = '<div class="snap-empty-hint">尚未輸入任何快照</div>';
      return;
    }
    const shares = Number(latest.shares) || 0;
    const nav = Number(latest.nav) || 0;
    const totalUsd = shares * nav;
    const totalHkd = totalUsd * DEFAULT_FX;
    placeholder.innerHTML = `
      <div class="policy-info-grid" style="margin-top:8px;">
        <div class="policy-info-item"><span class="policy-info-label">單位數目</span><span class="policy-info-value">${shares}</span></div>
        <div class="policy-info-item"><span class="policy-info-label">基金價格</span><span class="policy-info-value">USD ${nav} <span class="text-muted" style="font-size:11px;">(${esc(latest.dateKey)})</span></span></div>
        <div class="policy-info-item"><span class="policy-info-label">總價值</span><span class="policy-info-value">USD ${totalUsd.toFixed(2)}</span></div>
        <div class="policy-info-item"><span class="policy-info-label">保單貨幣價值</span><span class="policy-info-value text-emerald">HKD ${totalHkd.toFixed(2)}</span></div>
      </div>
    `;
  } catch (e) {
    placeholder.innerHTML = '<div class="snap-empty-hint">快照載入失敗</div>';
  }
}

/* ============================================
   卡片 HTML
   ============================================ */
function _cardHtml({ fund, policyName, pct, dynamicCost }, isInsurance) {
  const cost = isInsurance ? (dynamicCost != null ? dynamicCost : Number(fund.cost) || 0) : (Number(fund.cost) || 0);
  const value = Number(fund.currentValue) || 0;
  const pnl = value - cost;
  const pnlCls = pnl >= 0 ? 'text-emerald' : 'text-red';
  const pnlSign = pnl >= 0 ? '+' : '';
  const pnlPct = cost > 0 ? ((pnl / cost) * 100).toFixed(2) : '0.00';

  const meta = isInsurance
    ? `<div class="policy-company">${esc(policyName)} · 比例 ${pct}%</div>`
    : '';

  return `
    <div class="glass-card policy-card" data-fund-id="${esc(fund.id)}">
      <div class="policy-header">
        <div style="min-width:0;">
          <div class="policy-name">${esc(fund.name || '（未命名）')}</div>
          ${meta}
        </div>
      </div>
      <div data-snap-latest="${esc(fund.id)}">
        <div class="snap-empty-hint">載入中…</div>
      </div>
      <div class="policy-info-grid" style="margin-top:10px;padding-top:10px;border-top:1px dashed rgba(255,255,255,0.08);">
        <div class="policy-info-item"><span class="policy-info-label">投入成本</span><span class="policy-info-value">${formatHKD(cost)}</span></div>
        <div class="policy-info-item"><span class="policy-info-label">盈虧</span><span class="policy-info-value ${pnlCls}">${pnlSign}${formatHKD(pnl)} (${pnlPct}%)</span></div>
      </div>
      <div class="policy-actions">
        <button type="button" class="btn btn-sm btn-primary" data-action="snapshot" data-id="${esc(fund.id)}" data-name="${esc(fund.name || '')}">
          <i data-lucide="camera" style="width:14px;height:14px;"></i> 輸入快照
        </button>
        <button type="button" class="btn btn-sm btn-ghost" data-action="history" data-id="${esc(fund.id)}" data-name="${esc(fund.name || '')}">
          <i data-lucide="history" style="width:14px;height:14px;"></i> 查看歷史
        </button>
        <button type="button" class="btn btn-sm btn-ghost" data-action="edit-fund" data-id="${esc(fund.id)}">
          <i data-lucide="pencil" style="width:14px;height:14px;"></i> 編輯
        </button>
        <button type="button" class="btn btn-sm btn-danger" data-action="delete-fund" data-id="${esc(fund.id)}">
          <i data-lucide="trash-2" style="width:14px;height:14px;"></i> 刪除
        </button>
      </div>
    </div>
  `;
}

/* ============================================
   快照輸入 Modal（估值日 + USD 價格）
   ============================================ */
async function _openSnapshotModal(fundId, fundName, onDone) {
  document.getElementById(SNAP_MODAL_ID)?.remove();

  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = SNAP_MODAL_ID;
  overlay.innerHTML = `<div class="modal" style="max-width:560px;max-height:90vh;overflow-y:auto;"><h2 class="modal-title">輸入快照 — ${esc(fundName)}</h2><div id="${SNAP_MODAL_ID}-form-root"></div></div>`;
  document.body.appendChild(overlay);
  overlay.addEventListener('click', (e) => { if (e.target === overlay) closeModal(SNAP_MODAL_ID); });

  const today = new Date().toISOString().slice(0, 10);

  buildForm({
    containerId: `${SNAP_MODAL_ID}-form-root`,
    fields: [
      { type: 'date', id: 'snap-date', label: '估值日（截圖上的價格日期）', required: true, defaultValue: today },
      { type: 'number', id: 'snap-shares', label: '單位數目', required: true, min: 0, step: 0.0001, defaultValue: 0 },
      { type: 'number', id: 'snap-nav', label: '基金價格 (USD)', required: true, min: 0, step: 0.0001, defaultValue: 0 },
      { type: 'number', id: 'snap-total-usd', label: '總價值 (USD，自動計算)', required: false, disabled: true, defaultValue: 0 },
      { type: 'number', id: 'snap-total-hkd', label: '保單貨幣價值 (HKD，自動計算)', required: false, disabled: true, defaultValue: 0 },
    ],
    submitText: '儲存快照',
    showCancel: true,
    cancelText: '取消',
    onSubmit: async (data) => {
      try {
        const dateKey = data['snap-date'];
        const shares = Number(data['snap-shares']) || 0;
        const nav = Number(data['snap-nav']) || 0;
        await saveFundSnapshot(fundId, dateKey, { shares, nav });
        showToast(`✅ 已儲存 ${dateKey} 快照`, 'success');
        closeModal(SNAP_MODAL_ID);
        overlay.remove();
        if (typeof onDone === 'function') onDone();
      } catch (err) { showToast('儲存失敗：' + err.message, 'error'); }
    },
    onCancel: () => { closeModal(SNAP_MODAL_ID); overlay.remove(); },
  });

  const sharesEl = document.getElementById('snap-shares');
  const navEl = document.getElementById('snap-nav');
  const totalUsdEl = document.getElementById('snap-total-usd');
  const totalHkdEl = document.getElementById('snap-total-hkd');

  const calcValue = () => {
    const s = Number(sharesEl?.value) || 0;
    const n = Number(navEl?.value) || 0;
    const usd = s * n;
    if (totalUsdEl) totalUsdEl.value = usd.toFixed(2);
    if (totalHkdEl) totalHkdEl.value = (usd * DEFAULT_FX).toFixed(2);
  };
  sharesEl?.addEventListener('input', calcValue);
  navEl?.addEventListener('input', calcValue);

  openModal(SNAP_MODAL_ID);
  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   歷史快照 Modal（列表 + 刪除 + 任選兩筆對比）
   ============================================ */
async function _openHistoryModal(fundId, fundName, onDone) {
  document.getElementById(HISTORY_MODAL_ID)?.remove();

  const snaps = await getFundSnapshotListOnce(fundId);

  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = HISTORY_MODAL_ID;

  const rowsHtml = snaps.length === 0
    ? '<div class="empty-state">尚無任何快照</div>'
    : snaps.map((s) => {
        const usd = (Number(s.shares) || 0) * (Number(s.nav) || 0);
        const hkd = usd * DEFAULT_FX;
        /* ✅ v103.0.34：附帶 _rawPath，供刪除精確定位 */
        return `
          <tr>
            <td><input type="checkbox" class="hist-cb" data-date="${esc(s.dateKey)}"></td>
            <td>${esc(s.dateKey)}</td>
            <td>${s.shares}</td>
            <td>USD ${s.nav}</td>
            <td>USD ${usd.toFixed(2)}</td>
            <td>HKD ${hkd.toFixed(2)}</td>
            <td><button type="button" class="btn btn-xs btn-danger" data-del-date="${esc(s.dateKey)}" data-del-raw="${esc(s._rawPath)}">刪除</button></td>
          </tr>
        `;
      }).join('');

  overlay.innerHTML = `
    <div class="modal" style="max-width:760px;max-height:90vh;overflow-y:auto;">
      <h2 class="modal-title">歷史快照 — ${esc(fundName)}</h2>
      <div style="margin-bottom:12px;">
        <button type="button" class="btn btn-sm btn-primary" id="hist-compare-btn" disabled>對比已勾選的兩筆</button>
      </div>
      <div style="overflow-x:auto;">
        <table class="data-table" style="width:100%;">
          <thead>
            <tr>
              <th style="width:32px;"></th>
              <th>估值日</th>
              <th>單位數</th>
              <th>價格 (USD)</th>
              <th>總值 (USD)</th>
              <th>保單價值 (HKD)</th>
              <th style="width:64px;">操作</th>
            </tr>
          </thead>
          <tbody id="hist-tbody">${rowsHtml}</tbody>
        </table>
      </div>
      <div id="hist-compare-result" style="margin-top:16px;"></div>
      <div style="margin-top:16px;text-align:right;">
        <button type="button" class="btn btn-sm" id="hist-close-btn">關閉</button>
      </div>
    </div>
  `;
  document.body.appendChild(overlay);
  overlay.addEventListener('click', (e) => { if (e.target === overlay) _closeHistory(); });

  const tbody = document.getElementById('hist-tbody');
  const compareBtn = document.getElementById('hist-compare-btn');
  const compareResult = document.getElementById('hist-compare-result');

  const refreshCompareBtn = () => {
    const checked = tbody.querySelectorAll('.hist-cb:checked');
    compareBtn.disabled = checked.length !== 2;
  };

  tbody?.addEventListener('change', (e) => {
    if (e.target.classList.contains('hist-cb')) {
      const checked = tbody.querySelectorAll('.hist-cb:checked');
      if (checked.length > 2) {
        e.target.checked = false;
      }
      refreshCompareBtn();
    }
  });

  tbody?.addEventListener('click', async (e) => {
    const btn = e.target.closest('button[data-del-date]');
    if (!btn) return;
    e.preventDefault();
    e.stopPropagation();
    const dateKey = btn.dataset.delDate;
    const rawPath = btn.dataset.delRaw;
    const ok = await openConfirm(`確定要刪除 ${dateKey} 的快照嗎？`, { title: '刪除快照', okText: '刪除', okClass: 'btn-danger' });
    if (!ok) return;
    try {
      /* ✅ v103.0.34：傳 rawPath 修舊格式刪除失效 */
      await removeFundSnapshot(fundId, dateKey, rawPath);
      showToast('✅ 已刪除快照', 'success');
      _closeHistory();
      if (typeof onDone === 'function') onDone();
    } catch (err) { showToast('刪除失敗：' + err.message, 'error'); }
  });

  compareBtn?.addEventListener('click', () => {
    const checked = Array.from(tbody.querySelectorAll('.hist-cb:checked')).map((cb) => cb.dataset.date);
    if (checked.length !== 2) return;
    checked.sort();
    const a = snaps.find((s) => s.dateKey === checked[0]);
    const b = snaps.find((s) => s.dateKey === checked[1]);
    if (!a || !b) return;
    const aUsd = (Number(a.shares) || 0) * (Number(a.nav) || 0);
    const bUsd = (Number(b.shares) || 0) * (Number(b.nav) || 0);
    const aHkd = aUsd * DEFAULT_FX;
    const bHkd = bUsd * DEFAULT_FX;
    const diff = (x, y) => {
      const d = y - x;
      const pct = x > 0 ? ((d / x) * 100).toFixed(2) : '—';
      const sign = d >= 0 ? '+' : '';
      return `${sign}${d.toFixed(4)} (${sign}${pct}%)`;
    };
    compareResult.innerHTML = `
      <div class="glass-card">
        <div class="policy-name" style="margin-bottom:8px;">對比：${esc(a.dateKey)} → ${esc(b.dateKey)}</div>
        <div class="policy-info-grid">
          <div class="policy-info-item"><span class="policy-info-label">單位數</span><span class="policy-info-value">${a.shares} → ${b.shares}</span><span class="policy-info-value text-emerald">${diff(Number(a.shares) || 0, Number(b.shares) || 0)}</span></div>
          <div class="policy-info-item"><span class="policy-info-label">價格 (USD)</span><span class="policy-info-value">${a.nav} → ${b.nav}</span><span class="policy-info-value text-emerald">${diff(Number(a.nav) || 0, Number(b.nav) || 0)}</span></div>
          <div class="policy-info-item"><span class="policy-info-label">總值 (USD)</span><span class="policy-info-value">${aUsd.toFixed(2)} → ${bUsd.toFixed(2)}</span><span class="policy-info-value text-emerald">${diff(aUsd, bUsd)}</span></div>
          <div class="policy-info-item"><span class="policy-info-label">保單價值 (HKD)</span><span class="policy-info-value">${aHkd.toFixed(2)} → ${bHkd.toFixed(2)}</span><span class="policy-info-value text-emerald">${diff(aHkd, bHkd)}</span></div>
        </div>
      </div>
    `;
  });

  document.getElementById('hist-close-btn')?.addEventListener('click', _closeHistory);

  openModal(HISTORY_MODAL_ID);
  if (window.lucide) window.lucide.createIcons();
}

function _closeHistory() {
  const el = document.getElementById(HISTORY_MODAL_ID);
  if (el) el.remove();
  closeModal(HISTORY_MODAL_ID);
}

/* ============================================
   🆕 v103.0.34：導入基金快照 Modal
   ============================================ */
async function _openImportSnapshotModal(funds, onDone) {
  document.getElementById(IMPORT_MODAL_ID)?.remove();

  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = IMPORT_MODAL_ID;
  overlay.innerHTML = `
    <div class="modal" style="max-width:640px;max-height:90vh;overflow-y:auto;">
      <h2 class="modal-title">導入基金快照</h2>

      <div class="field" style="margin-bottom:14px;">
        <label class="field-label">目標基金</label>
        <select class="select" id="${IMPORT_MODAL_ID}-fund" style="width:100%;">
          <option value="">— 請選擇基金 —</option>
          ${funds.map((f) => `<option value="${esc(f.id)}">${esc(f.name || '（未命名）')}</option>`).join('')}
        </select>
      </div>

      <div class="field" style="margin-bottom:14px;">
        <label class="field-label">上傳 JSON 檔案（可選）</label>
        <input type="file" id="${IMPORT_MODAL_ID}-file" accept=".json,application/json" class="input" style="width:100%;">
      </div>

      <div class="field" style="margin-bottom:14px;">
        <label class="field-label">或直接貼上 JSON</label>
        <textarea class="input" id="${IMPORT_MODAL_ID}-text" rows="8"
          placeholder='[{"dateKey":"2024-01-15","shares":100.5,"nav":12.34}, ...]'
          style="width:100%;font-family:var(--font-mono);font-size:12px;resize:vertical;"></textarea>
      </div>

      <div id="${IMPORT_MODAL_ID}-status" style="font-size:12px; margin-bottom:12px; min-height:20px;"></div>

      <div class="modal-actions" style="display:flex; justify-content:flex-end; gap:10px;">
        <button type="button" class="btn btn-ghost" data-action="cancel">取消</button>
        <button type="button" class="btn btn-primary" data-action="import"><i data-lucide="upload" style="width:14px;height:14px;"></i> 開始導入</button>
      </div>
    </div>
  `;
  document.body.appendChild(overlay);

  overlay.addEventListener('click', (e) => {
    if (e.target === overlay || e.target.closest('[data-action="cancel"]')) {
      closeModal(IMPORT_MODAL_ID);
      overlay.remove();
    }
  });

  const fundSel = document.getElementById(`${IMPORT_MODAL_ID}-fund`);
  const fileInput = document.getElementById(`${IMPORT_MODAL_ID}-file`);
  const textArea = document.getElementById(`${IMPORT_MODAL_ID}-text`);
  const statusEl = document.getElementById(`${IMPORT_MODAL_ID}-status`);
  const importBtn = overlay.querySelector('[data-action="import"]');

  fileInput?.addEventListener('change', async () => {
    const file = fileInput.files?.[0];
    if (!file) return;
    try {
      const text = await file.text();
      textArea.value = text;
    } catch (err) {
      showToast('讀取檔案失敗：' + err.message, 'error');
    }
  });

  importBtn?.addEventListener('click', async () => {
    const fundId = fundSel.value;
    if (!fundId) { showToast('請選擇基金', 'warning'); return; }
    const text = (textArea.value || '').trim();
    if (!text) { showToast('請貼上 JSON 或上傳檔案', 'warning'); return; }

    let parsed;
    try {
      parsed = _parseSnapshotsJson(text);
    } catch (err) {
      statusEl.innerHTML = `<span style="color:var(--neon-red);">❌ ${esc(err.message)}</span>`;
      return;
    }

    const { valid, errors } = parsed;
    if (valid.length === 0) {
      statusEl.innerHTML = `<span style="color:var(--neon-red);">❌ 無有效資料</span>`;
      return;
    }

    importBtn.disabled = true;
    statusEl.innerHTML = `導入中…（0 / ${valid.length}）`;

    let ok = 0, fail = 0;
    const failDetails = [];
    for (const snap of valid) {
      try {
        await saveFundSnapshot(fundId, snap.dateKey, { shares: snap.shares, nav: snap.nav });
        ok++;
      } catch (err) {
        fail++;
        failDetails.push(`${snap.dateKey}：${err.message}`);
      }
      statusEl.innerHTML = `導入中…（${ok + fail} / ${valid.length}）`;
    }

    importBtn.disabled = false;

    const lines = [
      `<div style="color:${fail === 0 ? 'var(--neon-emerald)' : 'var(--neon-orange)'}; font-weight:700;">`,
      `✅ 成功 ${ok} 筆${fail > 0 ? ` / ❌ 失敗 ${fail} 筆` : ''}`,
      `</div>`,
    ];
    if (errors.length > 0) {
      lines.push(`<div style="margin-top:6px; color:var(--text-muted); font-size:11px;">格式錯誤 ${errors.length} 筆：</div>`);
      lines.push(`<div style="margin-top:4px; font-size:11px; color:var(--text-muted);">${errors.slice(0, 5).map(esc).join('<br>')}${errors.length > 5 ? `<br>…還有 ${errors.length - 5} 筆` : ''}</div>`);
    }
    if (failDetails.length > 0) {
      lines.push(`<div style="margin-top:6px; color:var(--text-muted); font-size:11px;">失敗明細：</div>`);
      lines.push(`<div style="margin-top:4px; font-size:11px; color:var(--text-muted);">${failDetails.slice(0, 5).map(esc).join('<br>')}${failDetails.length > 5 ? `<br>…還有 ${failDetails.length - 5} 筆` : ''}</div>`);
    }
    statusEl.innerHTML = lines.join('');

    if (ok > 0) {
      showToast(`✅ 導入完成：成功 ${ok} 筆`, 'success');
      if (typeof onDone === 'function') onDone();
    }
  });

  openModal(IMPORT_MODAL_ID);
  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   🆕 v103.0.34：解析快照 JSON
   -------------------------------------------------
   接受兩種格式：
     A. 純陣列：[{dateKey, shares, nav}, ...]
     B. 包裹：{ snapshots: [...] }
   回傳：{ valid: [...], errors: [...] }
   ============================================ */
function _parseSnapshotsJson(text) {
  let data;
  try {
    data = JSON.parse(text);
  } catch (e) {
    throw new Error('JSON 格式錯誤：' + e.message);
  }

  let arr;
  if (Array.isArray(data)) arr = data;
  else if (data && Array.isArray(data.snapshots)) arr = data.snapshots;
  else throw new Error('格式錯誤：需為陣列或 { snapshots: [...] }');

  const valid = [];
  const errors = [];

  arr.forEach((item, i) => {
    const idx = i + 1;
    if (!item || typeof item !== 'object') { errors.push(`第 ${idx} 筆：非物件`); return; }
    const { dateKey, shares, nav } = item;
    if (!dateKey || !/^\d{4}-\d{2}-\d{2}$/.test(String(dateKey))) {
      errors.push(`第 ${idx} 筆：日期格式錯誤（需 YYYY-MM-DD）`);
      return;
    }
    const s = Number(shares);
    const n = Number(nav);
    if (isNaN(s) || s < 0) { errors.push(`第 ${idx} 筆：shares 無效`); return; }
    if (isNaN(n) || n < 0) { errors.push(`第 ${idx} 筆：nav 無效`); return; }
    valid.push({ dateKey: String(dateKey), shares: s, nav: n });
  });

  return { valid, errors };
}

/* ============================================
   Stats（邏輯不動）
   ============================================ */
function _buildStats(funds, policies) {
  const list = funds || [];
  const pols = policies || [];
  const ym = AppState.getYearMonth();
  const y = ym.year || String(new Date().getFullYear());
  const m = ym.month === 'all' ? String(new Date().getMonth() + 1).padStart(2, '0') : ym.month;

  const costMaps = {};
  pols.forEach((p) => { costMaps[p.id] = _getCostMapForPolicy(p, list, y, m); });

  let cost = 0;
  list.forEach((f) => {
    if (f.type === 'insurance') {
      const policy = _findPolicyByFund(f, pols);
      if (policy && costMaps[policy.id]) {
        cost += costMaps[policy.id][f.id] || 0;
      } else {
        cost += Number(f.cost) || 0;
      }
    } else {
      cost += Number(f.cost) || 0;
    }
  });
  const value = list.reduce((s, f) => s + (Number(f.currentValue) || 0), 0);
  const pnl = value - cost;
  const pct = cost > 0 ? ((pnl / cost) * 100).toFixed(2) : '0.00';
  const sign = pnl >= 0 ? '+' : '';
  const insCount = list.filter((f) => f.type === 'insurance').length;
  const standaloneCount = list.filter((f) => f.type !== 'insurance').length;
  return [
    { title: '總投入成本', value: formatHKD(cost), hint: `共 ${list.length} 筆（保險 ${insCount} / 獨立 ${standaloneCount}）`, icon: 'wallet' },
    { title: '總現時價值', value: formatHKD(value), valueClass: 'emerald', hint: '最新現值加總', icon: 'line-chart' },
    { title: '總帳面盈虧', value: `${sign}${formatHKD(pnl)} (${pct}%)`, valueClass: pnl >= 0 ? 'emerald' : 'red', hint: pnl >= 0 ? '獲利中' : '虧損中', icon: pnl >= 0 ? 'trending-up' : 'trending-down' },
  ];
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/portfolio.js
   Version: 1792207200
   Batch: B35-2
   Writer: A20261016-001#005
   ═══════════════════════════════════════════ */```

---

## 📄 js/pages/settings.js

```
// ============================================
// settings.js — 系統設定（v103.0.36 Page Schema）
// 位置：js/pages/settings.js
// ============================================
// v103.0.36（B34-4）：
//   ✅ 個人化 Tab 加「年份範圍」卡（D-036）
//   ✅ 從 database-yearrange.js 搬入，改用 js/shared/yearrange-card.js
// v103.0.35（B31-1）：
//   ✅ _load 改用 listenPageMetadata（SSOT）
// v103.0.34（B29-5d）：
//   ✅ 個人化 Tab 加「頁面標題」卡
// ============================================

import { initTabPanel } from '../ui/tab-panel.js';
import { initBankAccountManager } from '../shared/bank-account-manager.js';
import { initEntityListPage } from '../shared/entity-list-page.js';
import { initYearRangeCard } from '../shared/yearrange-card.js';
import { AppState } from '../core/state.js';
import { logout } from '../core/auth.js';
import { api } from '../core/api.js';
import { STORAGE_KEYS, ENTITY_KEYS } from '../config/constants.js';
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { esc } from '../lib/dom.js';
import { listPages, getDefaultMetadata } from '../config/page-metadata-registry.js';
import { listenPageMetadata, savePageMetadata } from '../core/db.js';

export default {
  title: '系統設定',
  data: {},
  state: { activeTab: AppState.isSuperAdmin ? 'platform' : 'banks' },
  derived: {},
  blocks: [],

  customMount: (ctx) => {
    const isSuper = AppState.isSuperAdmin;
    const tabs = [];

    if (isSuper) {
      tabs.push({ key: 'platform', label: '平台設定', icon: 'settings', panelId: 'settings-panel-platform' });
    }
    tabs.push({ key: 'banks', label: '銀行帳號', icon: 'landmark', panelId: 'settings-panel-banks' });
    tabs.push({ key: 'personal', label: '個人化', icon: 'user', panelId: 'settings-panel-personal' });

    ctx._bankMgr = null;
    ctx._memberList = null;
    ctx._debugCleanup = null;
    ctx._personalCleanup = null;
    ctx._migrateCleanup = null;
    ctx._pageMetaCleanup = null;
    ctx._pageMetaUnsub = null;
    ctx._yearRangeApi = null;   /* 🆕 v103.0.36 */

    const tabPanel = initTabPanel({
      containerId: 'settings-tabs',
      tabs,
      defaultKey: isSuper ? 'platform' : 'banks',
      storageKey: 'settings-tab',
      onChange: (key) => {
        if (key === 'banks') {
          if (!ctx._bankMgr) {
            ctx._bankMgr = initBankAccountManager('bank-account-manager-root', {
              canInput: AppState.getCanInput(),
            });
          } else {
            try { ctx._bankMgr.refresh?.(); } catch (e) {}
          }
        }
        if (key === 'personal') {
          _initPersonalTab(ctx);
        }
      },
    });

    ctx._tabPanel = tabPanel;

    const initialKey = tabPanel?.getCurrent?.();
    if (initialKey === 'personal') _initPersonalTab(ctx);

    return {
      destroy: () => {
        try { tabPanel?.destroy(); } catch (e) {}
        if (ctx._bankMgr) { try { ctx._bankMgr.destroy?.(); } catch (e) {} ctx._bankMgr = null; }
        if (ctx._memberList) { try { ctx._memberList.destroy?.(); } catch (e) {} ctx._memberList = null; }
        if (ctx._personalCleanup) { try { ctx._personalCleanup(); } catch (e) {} ctx._personalCleanup = null; }
        if (ctx._debugCleanup) { try { ctx._debugCleanup(); } catch (e) {} ctx._debugCleanup = null; }
        if (ctx._migrateCleanup) { try { ctx._migrateCleanup(); } catch (e) {} ctx._migrateCleanup = null; }
        if (ctx._pageMetaCleanup) { try { ctx._pageMetaCleanup(); } catch (e) {} ctx._pageMetaCleanup = null; }
        if (ctx._pageMetaUnsub) { try { ctx._pageMetaUnsub(); } catch (e) {} ctx._pageMetaUnsub = null; }
        if (ctx._yearRangeApi) { try { ctx._yearRangeApi.destroy?.(); } catch (e) {} ctx._yearRangeApi = null; }
      },
    };
  },
};

/* ============================================
   個人化 Tab
   ============================================ */
function _initPersonalTab(ctx) {
  if (ctx._personalCleanup) {
    _renderDebugStatus();
    return;
  }

  const accEl = document.getElementById('settings-account');
  const roleEl = document.getElementById('settings-role');
  const famEl = document.getElementById('settings-family');
  const memberAcc = AppState.getMemberAccount() || {};

  if (accEl) accEl.value = memberAcc.account || AppState.currentUser?.email?.split('@')[0] || '—';
  if (roleEl) roleEl.value = AppState.getRoleLabel();
  if (famEl) famEl.value = AppState.getFamilyName() || '—';

  const logoutBtn = document.getElementById('settings-logout-btn');
  const onLogout = async () => {
    const ok = await openConfirm('確定要登出嗎？', { title: '登出', okText: '登出', okClass: 'btn-danger' });
    if (ok) logout();
  };
  if (logoutBtn) logoutBtn.addEventListener('click', onLogout);

  _renderStatsModeOptions();

  const saveBtn = document.getElementById('save-stats-mode-btn');
  const onSaveMode = () => {
    const selected = document.querySelector('input[name="stats-mode"]:checked');
    if (!selected) return;
    try {
      localStorage.setItem(STORAGE_KEYS.STATS_MODE, selected.value);
      showToast('✅ 已儲存顯示模式', 'success');
    } catch (e) {
      showToast('儲存失敗', 'error');
    }
  };
  if (saveBtn) saveBtn.addEventListener('click', onSaveMode);

  const memberRoot = document.getElementById('family-members-root');
  if (memberRoot && !ctx._memberList) {
    ctx._memberList = initEntityListPage({
      entity: ENTITY_KEYS.MEMBER,
      containerId: 'family-members-root',
      options: {
        defaultView: 'table',
        showViewToggle: false,
        storageKey: 'settings-members-view',
        mobileCardMode: true,
      },
    });
  }

  _injectDebugCard(ctx);
  _injectMigrateCard(ctx);
  _injectPageMetadataCard(ctx);
  _injectYearRangeCard(ctx);   /* 🆕 v103.0.36（B34-4） */

  ctx._personalCleanup = () => {
    if (logoutBtn) logoutBtn.removeEventListener('click', onLogout);
    if (saveBtn) saveBtn.removeEventListener('click', onSaveMode);
  };

  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   統計卡顯示模式
   ============================================ */
function _renderStatsModeOptions() {
  const root = document.getElementById('stats-mode-options');
  if (!root) return;

  let current = 'auto';
  try {
    current = localStorage.getItem(STORAGE_KEYS.STATS_MODE) || 'auto';
  } catch (e) {}

  const options = [
    { value: 'auto',        label: '自動（依裝置 / 檢視模式）' },
    { value: 'integrated',  label: '整合卡（單一大卡）' },
    { value: 'compact',     label: '緊湊卡（多個小卡）' },
  ];

  root.innerHTML = options.map((o) => `
    <label style="display:flex; align-items:center; gap:8px; cursor:pointer; padding:8px; border-radius:6px; background:rgba(255,255,255,0.03);">
      <input type="radio" name="stats-mode" value="${o.value}" ${o.value === current ? 'checked' : ''} style="width:auto; cursor:pointer;">
      <span style="font-size:13px;">${o.label}</span>
    </label>
  `).join('');
}

/* ============================================
   除錯工具卡
   ============================================ */
function _injectDebugCard(ctx) {
  const grid = document.querySelector('#settings-panel-personal .settings-personal-grid');
  if (!grid) return;

  if (document.getElementById('settings-debug-card')) {
    _renderDebugStatus();
    return;
  }

  const card = document.createElement('div');
  card.className = 'glass-card';
  card.id = 'settings-debug-card';
  card.style.gridColumn = '1 / -1';
  card.innerHTML = `
    <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:12px;">
      <i data-lucide="bug" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
      <span>除錯工具（開發者用）</span>
    </div>
    <p class="glass-card-hint mb-12">
      手機若無 DevTools，可手動開啟 vConsole 檢視日誌。開啟後會**跨頁保持**，
      直到你手動關閉。
    </p>
    <div id="settings-debug-status" style="margin-bottom:12px; font-size:13px;"></div>
    <div style="display:flex; gap:8px; flex-wrap:wrap;">
      <button type="button" class="btn btn-primary" id="settings-debug-enable">
        <i data-lucide="play" style="width:14px;height:14px;"></i> 開啟 vConsole
      </button>
      <button type="button" class="btn btn-ghost" id="settings-debug-disable">
        <i data-lucide="square" style="width:14px;height:14px;"></i> 關閉 vConsole
      </button>
    </div>
  `;
  grid.appendChild(card);

  if (window.lucide) window.lucide.createIcons();

  _renderDebugStatus();
  _bindDebugButtons(ctx);
}

async function _renderDebugStatus() {
  const statusEl = document.getElementById('settings-debug-status');
  if (!statusEl) return;

  try {
    const mod = await import('../core/debug.js');
    const enabled = mod.isDebugEnabled();

    statusEl.innerHTML = enabled
      ? '<span class="badge badge-success">● 已開啟</span> <span class="text-muted" style="font-size:12px; margin-left:6px;">vConsole 正在運行（右下角綠點）</span>'
      : '<span class="badge badge-muted">● 已關閉</span> <span class="text-muted" style="font-size:12px; margin-left:6px;">點擊「開啟 vConsole」以啟用</span>';
  } catch (err) {
    statusEl.innerHTML = '<span class="badge badge-pending">載入失敗</span>';
  }
}

function _bindDebugButtons(ctx) {
  const enableBtn = document.getElementById('settings-debug-enable');
  const disableBtn = document.getElementById('settings-debug-disable');

  const onEnable = async () => {
    if (enableBtn.disabled) return;
    enableBtn.disabled = true;
    const originalText = enableBtn.innerHTML;
    enableBtn.innerHTML = '<i data-lucide="loader" style="width:14px;height:14px;"></i> 載入中…';
    if (window.lucide) window.lucide.createIcons();

    try {
      const mod = await import('../core/debug.js');
      const ok = await mod.forceEnableDebug();
      if (ok) showToast('✅ vConsole 已開啟（跨頁保持）', 'success', 3000);
      else showToast('❌ 載入失敗，請檢查網路', 'error', 4000);
    } catch (err) {
      console.error('[settings] vConsole 開啟失敗：', err);
    } finally {
      enableBtn.disabled = false;
      enableBtn.innerHTML = originalText;
      if (window.lucide) window.lucide.createIcons();
      _renderDebugStatus();
    }
  };

  const onDisable = async () => {
    try {
      const mod = await import('../core/debug.js');
      mod.forceDisableDebug();
      showToast('vConsole 已關閉', 'info', 2000);
    } catch (err) {}
    _renderDebugStatus();
  };

  if (enableBtn) enableBtn.addEventListener('click', onEnable);
  if (disableBtn) disableBtn.addEventListener('click', onDisable);

  ctx._debugCleanup = () => {
    if (enableBtn) enableBtn.removeEventListener('click', onEnable);
    if (disableBtn) disableBtn.removeEventListener('click', onDisable);
  };
}

/* ============================================
   一次性遷移卡（superadmin）
   ============================================ */
function _injectMigrateCard(ctx) {
  if (!AppState.isSuperAdmin) return;

  const grid = document.querySelector('#settings-panel-personal .settings-personal-grid');
  if (!grid) return;
  if (document.getElementById('settings-migrate-card')) return;

  const card = document.createElement('div');
  card.className = 'glass-card';
  card.id = 'settings-migrate-card';
  card.style.gridColumn = '1 / -1';
  card.style.borderColor = 'rgba(251,146,60,0.3)';
  card.style.background = 'rgba(251,146,60,0.03)';
  card.innerHTML = `
    <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:12px; color:var(--neon-orange);">
      <i data-lucide="alert-triangle" style="width:16px;height:16px;"></i>
      <span>一次性資料遷移（超級管理員專用）</span>
    </div>
    <p class="glass-card-hint mb-12">
      將舊資料（狀態名稱、缺失欄位）正規化為新格式。
      <b>可重複執行</b>（已遷移的資料不會再次修改）。
    </p>
    <div class="field" style="margin-bottom:12px;">
      <label class="field-label">目標家庭</label>
      <select class="select" id="settings-migrate-family" style="max-width:400px;">
        <option value="">載入中…</option>
      </select>
    </div>
    <button type="button" class="btn btn-magenta" id="settings-migrate-btn">
      <i data-lucide="refresh-cw" style="width:14px;height:14px;"></i> 執行遷移
    </button>
    <div id="settings-migrate-status" style="margin-top:12px; font-size:12px; color:var(--text-muted);"></div>
  `;
  grid.appendChild(card);

  if (window.lucide) window.lucide.createIcons();

  const sel = document.getElementById('settings-migrate-family');
  const btn = document.getElementById('settings-migrate-btn');
  const statusEl = document.getElementById('settings-migrate-status');

  const _loadFamilies = async () => {
    try {
      const data = await api.adminListFamilies();
      const families = data.families || [];
      if (families.length === 0) {
        sel.innerHTML = '<option value="">（尚無家庭）</option>';
        return;
      }
      sel.innerHTML = families.map((f) =>
        `<option value="${esc(f.uid)}">${esc(f.name || f.uid)}（${esc(f.uid.slice(0, 8))}…）</option>`
      ).join('');
    } catch (err) {
      console.error('[settings] 載入家庭清單失敗：', err);
      sel.innerHTML = '<option value="">載入失敗</option>';
    }
  };
  _loadFamilies();

  const onMigrate = async () => {
    const familyId = sel.value;
    if (!familyId) {
      showToast('請先選擇家庭', 'error');
      return;
    }

    const familyName = sel.options[sel.selectedIndex]?.textContent || familyId;

    const ok = await openConfirm(
      `⚠️ 確定要對以下家庭執行一次性資料遷移嗎？\n\n` +
      `目標家庭：${familyName}\n\n` +
      `• 將舊狀態名稱改為新代碼\n` +
      `• 補齊缺失欄位（isAutoLinked / fundsAllocation / type 等）\n` +
      `• 可重複執行（已遷移的資料不會再改）`,
      { title: '一次性資料遷移', okText: '執行遷移', okClass: 'btn-danger' }
    );
    if (!ok) return;

    if (btn) btn.disabled = true;
    if (statusEl) statusEl.textContent = '遷移中…';

    try {
      const result = await api.adminMigrate(familyId);
      const s = result.summary || {};
      const lines = [
        `✅ 完成（${esc(familyName)}）`,
        `• 支出：掃描 ${s.expenses?.scanned || 0}，正規化 ${s.expenses?.normalized || 0}`,
        `• 保險付款：掃描 ${s.payments?.scanned || 0}，正規化 ${s.payments?.normalized || 0}`,
        `• 保單：掃描 ${s.policies?.scanned || 0}，補齊 ${s.policies?.patched || 0}`,
        `• 基金：掃描 ${s.funds?.scanned || 0}，補齊 ${s.funds?.patched || 0}`,
      ];
      if (statusEl) statusEl.innerHTML = lines.join('<br>');
      showToast('✅ 遷移完成', 'success');
    } catch (err) {
      if (statusEl) statusEl.textContent = '❌ 遷移失敗：' + err.message;
      showToast('遷移失敗：' + err.message, 'error');
    } finally {
      if (btn) btn.disabled = false;
    }
  };

  if (btn) btn.addEventListener('click', onMigrate);

  ctx._migrateCleanup = () => {
    if (btn) btn.removeEventListener('click', onMigrate);
  };
}

/* ============================================
   頁面標題卡
   ============================================ */
function _injectPageMetadataCard(ctx) {
  const grid = document.querySelector('#settings-panel-personal .settings-personal-grid');
  if (!grid) return;
  if (document.getElementById('settings-page-meta-card')) return;

  const pages = listPages();

  const card = document.createElement('div');
  card.className = 'glass-card';
  card.id = 'settings-page-meta-card';
  card.style.gridColumn = '1 / -1';
  card.innerHTML = `
    <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:12px;">
      <i data-lucide="type" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
      <span>頁面標題</span>
    </div>
    <p class="glass-card-hint mb-12">
      可自訂每個頁面的標題與副標題（副標題會加在動態資訊之前）。
    </p>

    <div class="field" style="margin-bottom:12px;">
      <label class="field-label">選擇頁面</label>
      <select class="select" id="settings-page-meta-page" style="max-width:400px;">
        ${pages.map((p) => `<option value="${esc(p.key)}">${esc(p.title)}</option>`).join('')}
      </select>
    </div>

    <div class="field" style="margin-bottom:12px;">
      <label class="field-label">頁面標題</label>
      <input class="input" type="text" id="settings-page-meta-title" maxlength="40" placeholder="輸入標題" style="max-width:400px;">
    </div>

    <div class="field" style="margin-bottom:12px;">
      <label class="field-label">副標題（靜態段）</label>
      <input class="input" type="text" id="settings-page-meta-sub" maxlength="60" placeholder="留空 = 使用預設" style="max-width:400px;">
      <div class="glass-card-hint" style="margin-top:4px;">
        💡 此段會加在動態資訊（如年月）之前
      </div>
    </div>

    <div style="display:flex; gap:8px; flex-wrap:wrap;">
      <button type="button" class="btn btn-primary" id="settings-page-meta-save">
        <i data-lucide="check" style="width:14px;height:14px;"></i> 儲存
      </button>
      <button type="button" class="btn btn-ghost" id="settings-page-meta-reset">
        <i data-lucide="rotate-ccw" style="width:14px;height:14px;"></i> 恢復預設
      </button>
    </div>

    <div id="settings-page-meta-status" style="margin-top:12px; font-size:12px; color:var(--text-muted);"></div>
  `;
  grid.appendChild(card);

  if (window.lucide) window.lucide.createIcons();

  const pageSel = document.getElementById('settings-page-meta-page');
  const titleInput = document.getElementById('settings-page-meta-title');
  const subInput = document.getElementById('settings-page-meta-sub');
  const saveBtn = document.getElementById('settings-page-meta-save');
  const resetBtn = document.getElementById('settings-page-meta-reset');
  const statusEl = document.getElementById('settings-page-meta-status');

  let _allMeta = {};

  const _applyFormFromMeta = (pageKey) => {
    const custom = _allMeta[pageKey] || {};
    const def = getDefaultMetadata(pageKey);
    titleInput.value = (custom.title != null && custom.title !== '') ? custom.title : def.title;
    subInput.value = (custom.subtitleStatic != null) ? custom.subtitleStatic : (def.subtitleStatic || '');
  };

  const _load = () => {
    if (ctx._pageMetaUnsub) {
      try { ctx._pageMetaUnsub(); } catch (e) { /* noop */ }
      ctx._pageMetaUnsub = null;
    }
    try {
      ctx._pageMetaUnsub = listenPageMetadata((all) => {
        _allMeta = all || {};
        _applyFormFromMeta(pageSel.value);
      });
    } catch (err) {
      console.warn('[settings] 載入頁面 metadata 失敗：', err);
      _applyFormFromMeta(pageSel.value);
    }
  };

  const onPageChange = () => _applyFormFromMeta(pageSel.value);
  pageSel.addEventListener('change', onPageChange);

  const onSave = async () => {
    const pageKey = pageSel.value;
    if (!pageKey) return;

    const title = titleInput.value.trim();
    const subtitleStatic = subInput.value;

    if (!title) {
      showToast('標題不可為空', 'warning');
      return;
    }

    if (saveBtn) saveBtn.disabled = true;
    if (statusEl) statusEl.textContent = '儲存中…';

    try {
      const next = { ..._allMeta };
      next[pageKey] = { title, subtitleStatic };
      await savePageMetadata(next);
      _allMeta = next;
      if (statusEl) statusEl.textContent = '✅ 已儲存';
      showToast('✅ 已儲存頁面標題', 'success');
    } catch (err) {
      if (statusEl) statusEl.textContent = '❌ 儲存失敗：' + err.message;
      showToast('儲存失敗：' + err.message, 'error');
    } finally {
      if (saveBtn) saveBtn.disabled = false;
    }
  };

  const onReset = async () => {
    const pageKey = pageSel.value;
    if (!pageKey) return;

    const ok = await openConfirm('確定要恢復此頁面的預設標題嗎？', { title: '恢復預設', okText: '恢復', okClass: 'btn-danger' });
    if (!ok) return;

    try {
      const next = { ..._allMeta };
      delete next[pageKey];
      await savePageMetadata(next);
      _allMeta = next;
      _applyFormFromMeta(pageKey);
      if (statusEl) statusEl.textContent = '✅ 已恢復預設';
      showToast('✅ 已恢復預設', 'success');
    } catch (err) {
      if (statusEl) statusEl.textContent = '❌ 恢復失敗：' + err.message;
      showToast('恢復失敗：' + err.message, 'error');
    }
  };

  if (saveBtn) saveBtn.addEventListener('click', onSave);
  if (resetBtn) resetBtn.addEventListener('click', onReset);

  _load();

  ctx._pageMetaCleanup = () => {
    pageSel.removeEventListener('change', onPageChange);
    if (saveBtn) saveBtn.removeEventListener('click', onSave);
    if (resetBtn) resetBtn.removeEventListener('click', onReset);
  };
}

/* ============================================
   🆕 v103.0.36（B34-4）：年份範圍卡（D-036）
   ============================================ */
function _injectYearRangeCard(ctx) {
  const grid = document.querySelector('#settings-panel-personal .settings-personal-grid');
  if (!grid) return;
  if (document.getElementById('settings-yearrange-card')) return;

  const card = document.createElement('div');
  card.className = 'glass-card';
  card.id = 'settings-yearrange-card';
  card.style.gridColumn = '1 / -1';
  card.innerHTML = `
    <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:12px;">
      <i data-lucide="calendar" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
      <span>年份範圍</span>
    </div>
    <div id="settings-yearrange-root"></div>
  `;
  grid.appendChild(card);

  if (window.lucide) window.lucide.createIcons();

  if (!ctx._yearRangeApi) {
    ctx._yearRangeApi = initYearRangeCard('settings-yearrange-root');
  }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/settings.js
   Version: 1791901600
   Batch: B34-4
   Writer: A20261013-004#096
   ═══════════════════════════════════════════ */```

---

## 📄 js/pages/settlements.js

```
// settlements.js — 結算清單（v103.0.35）
// v103.0.35（B29-7a）：
//   ✅ 刪除 syncSubtitle（動態副標題）
// ============================================
import { formatHKD } from '../lib/format.js';
import { esc } from '../lib/dom.js';
import { mergeSettlementData } from '../lib/merge.js';
import { applyFilters } from '../lib/filter-sort.js';
import {
  listenAllMemberExpenses,
  removeExpense,
  getInsurancePoliciesOnce,
  getInsurancePaymentsOnce,
  getBankAccountsOnce,
} from '../core/db.js';
import { api } from '../core/api.js';
import { estimateMonthlyAmount, getPeriodInfo } from '../lib/insurance.js';
import { resolveName } from '../config/entity-registry.js';
import { getColumns, getResolvers } from '../config/column-registry.js';
import { AppState } from '../core/state.js';
import { showToast } from '../ui/toast.js';
import { openConfirm, openModal, closeModal } from '../ui/modal.js';
import { openExpenseModal } from '../lib/expense-modal.js';
import { renderStatsCards } from '../shared/stats-cards.js';
import { renderDataTable } from '../shared/data-table.js';
import { renderDataCard } from '../shared/data-card.js';
import { renderPageControls } from '../lib/page-controls.js';
import { RESERVED_IDS, STORAGE_PREFIXES } from '../config/constants.js';

const INS_MODAL_ID = 'settle-insurance-modal';
const HIDE_COMPLETED_KEY = 'fin_ui_settle_hide_completed';

export default {
  title: '結算清單',
  data: {},
  state: {
    filters: { year: '', month: '', source: '' },
    hideCompleted: true,
    view: 'table',
  },
  derived: {},
  blocks: [],

  customMount: (ctx) => {
    let _rows = [], _filtered = [];
    let _memberExpenses = [], _insuranceRows = [];
    let _statsApi = null, _tableApi = null, _cardApi = null;
    let _monthlyUnsub = null, _controls = null;
    let _view = 'table';

    try {
      const saved = localStorage.getItem(HIDE_COMPLETED_KEY);
      ctx.state.hideCompleted = saved === null ? true : saved === 'true';
    } catch (e) { /* noop */ }

    try {
      const v = localStorage.getItem(`${STORAGE_PREFIXES.VIEW}settlement-view`);
      if (v === 'card' || v === 'table') _view = v;
    } catch (e) { /* noop */ }

    const _recompute = () => {
      const { year, month } = AppState.getYearMonth();
      _rows = mergeSettlementData({
        memberExpenses: _memberExpenses,
        insuranceRows: _insuranceRows,
        year: String(year),
        month,
      });
      render();
    };

    const _isInPolicyPeriod = (p, y, m) => {
      const info = getPeriodInfo(p, y, m);
      if (!info) return false;
      if (p.type === 'fund_insurance' && Number(p.monthlyPremium) <= 0) return false;
      return true;
    };

    const _loadInsuranceRows = async () => {
      const { year, month } = AppState.getYearMonth();
      const y = String(year);
      const m = month === 'all' ? null : String(month);
      const hideCompleted = ctx.state.hideCompleted === true;

      try {
        const policies = await getInsurancePoliciesOnce();
        const list = [];

        for (const p of policies) {
          if (hideCompleted && p.isCompleted) continue;

          let payments = {};
          try { payments = await getInsurancePaymentsOnce(p.id); } catch (e) {}

          const holder = p.policyHolderId || p.memberId || '';

          if (!m) {
            const yearPayments = payments?.[y] || {};
            for (const mm of Object.keys(yearPayments)) {
              const payment = yearPayments[mm];
              list.push({
                policyId: p.id,
                policyName: p.name || '（未命名保單）',
                policyHolderId: holder,
                memberId: p.memberId || '',
                status: payment?.status || '',
                amount: payment?.amount || 0,
                date: payment?.date || '',
                bankId: payment?.bankId || '',
                txnId: payment?.txnId || '',
                paymentMode: p.paymentMode || 'direct',
                advanceHolderId: p.advanceHolderId || '',
                month: mm,
              });
            }
            continue;
          }

          const payment = payments?.[y]?.[m];
          if (payment) {
            list.push({
              policyId: p.id,
              policyName: p.name || '（未命名保單）',
              policyHolderId: holder,
              memberId: p.memberId || '',
              status: payment?.status || '',
              amount: payment?.amount || 0,
              date: payment?.date || '',
              bankId: payment?.bankId || '',
              txnId: payment?.txnId || '',
              paymentMode: p.paymentMode || 'direct',
              advanceHolderId: p.advanceHolderId || '',
            });
            continue;
          }

          if (!_isInPolicyPeriod(p, y, m)) continue;

          const amount = estimateMonthlyAmount(p, y, m) || 0;
          list.push({
            policyId: p.id,
            policyName: p.name || '（未命名保單）',
            policyHolderId: holder,
            memberId: p.memberId || '',
            status: '',
            amount,
            date: '',
            bankId: '',
            txnId: '',
            paymentMode: p.paymentMode || 'direct',
            advanceHolderId: p.advanceHolderId || '',
          });
        }
        _insuranceRows = list;
      } catch (e) {
        console.error('[settlements] 載入保險資料失敗：', e);
        _insuranceRows = [];
      }
    };

    const reload = async () => {
      const { year, month } = AppState.getYearMonth();
      ctx.state.filters.year = String(year);
      ctx.state.filters.month = month === 'all' ? '' : String(month);

      if (_monthlyUnsub) { try { _monthlyUnsub(); } catch (e) {} }
      _monthlyUnsub = listenAllMemberExpenses(String(year), month, (memberExpenses) => {
        _memberExpenses = memberExpenses;
        _recompute();
      });
      await _loadInsuranceRows();
      _recompute();
    };

    const render = () => {
      _filtered = applyFilters(_rows, ctx.state.filters);
      _paintStats();
      _paintList();
    };

    const _paintStats = () => {
      const pending = _filtered.filter((r) => !r.isDone);
      const done = _filtered.filter((r) => r.isDone);
      const pt = pending.reduce((s, r) => s + (Number(r.amount) || 0), 0);
      const dt = done.reduce((s, r) => s + (Number(r.amount) || 0), 0);
      if (_statsApi) { try { _statsApi.destroy(); } catch (e) {} }
      _statsApi = renderStatsCards({
        container: 'settlement-stats-root',
        cards: [
          { title: '待處理', value: formatHKD(pt), valueClass: 'magenta', hint: `${pending.length} 筆`, icon: 'clock' },
          { title: '已處理', value: formatHKD(dt), valueClass: 'emerald', hint: `${done.length} 筆`, icon: 'check-circle' },
          { title: '總計', value: formatHKD(pt + dt), valueClass: 'cyan', hint: `${_filtered.length} 筆`, icon: 'calculator' },
        ],
        columns: 3,
      });
    };

    const _hooks = (canInput) => ({
      customActions: canInput ? (row) => {
        if (row.source === 'insurance') {
          return row.isDone
            ? [{ label: '取消扣款', icon: 'rotate-ccw', className: 'btn-ghost', action: 'ins-unsync', onClick: () => _onInsuranceUnsync(row, reload) }]
            : [{ label: '確認扣款', icon: 'check', className: 'btn-primary', action: 'ins-sync', onClick: () => _onInsuranceSync(row, reload) }];
        }
        return [
          { label: '結算', icon: 'check-circle', className: 'btn-ghost', action: 'settle', onClick: () => {
            openExpenseModal({
              row: { id: row._ref?.expenseId, year: row.year, month: row.month, memberId: row.memberId, status: row.isDone ? 'done' : 'pending', paymentMethodId: row.paymentMethodId || '', bankId: row.bankId || '' },
              mode: 'settle',
              onSuccess: reload,
            });
          }},
          { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
            const ok = await openConfirm(`確定要刪除「${row.name}」嗎？`, { okText: '刪除', okClass: 'btn-danger' });
            if (!ok) return;
            try {
              if (row._ref) await removeExpense(row.year, row.month, row._ref.memberId, row._ref.expenseId);
              showToast('✅ 已刪除', 'success'); reload();
            } catch (e) { showToast('刪除失敗：' + e.message, 'error'); }
          }},
        ];
      } : () => [],
    });

    const _paintList = () => {
      const root = document.getElementById('settlement-table-root');
      if (!root) return;
      if (_tableApi) { try { _tableApi.destroy(); } catch (e) {} _tableApi = null; }
      if (_cardApi) { try { _cardApi.destroy(); } catch (e) {} _cardApi = null; }

      if (_filtered.length === 0) {
        root.innerHTML = '<div class="glass-card"><div class="empty-state">此條件下無資料</div></div>';
        return;
      }

      const canInput = AppState.getCanInput();
      const columns = getColumns('settlements');
      const resolvers = getResolvers('settlements');

      if (_view === 'card') {
        _cardApi = renderDataCard({
          container: root,
          entityKey: '__settlement__',
          rows: _filtered,
          options: { gridClass: 'grid grid-3', columns, resolvers },
          hooks: _hooks(canInput),
        });
      } else {
        _tableApi = renderDataTable({
          container: root,
          entityKey: '__settlement__',
          rows: _filtered,
          tableId: 'settlement-table',
          options: { columns, resolvers, storageKey: 'settlement-table' },
          hooks: _hooks(canInput),
        });
      }
    };

    const { year, month } = AppState.getYearMonth();
    ctx.state.filters.year = String(year);
    ctx.state.filters.month = month === 'all' ? '' : String(month);

    _controls = renderPageControls({
      containerId: 'page-controls-root',
      headerContainerId: 'page-header-actions',
      items: [
        {
          type: 'viewToggle',
          storageKey: 'settlement-view',
          defaultView: 'table',
          onChange: (v) => { _view = v; _paintList(); },
        },
        { type: 'year' },
        { type: 'month', includeAll: true },
        {
          type: 'filter',
          render: () => `
            <div class="page-filter-bar">
              <div class="filter-group" style="flex-direction:row;align-items:center;gap:8px;min-width:auto;">
                <input type="checkbox" id="settle-hide-completed" style="width:auto;cursor:pointer;" ${ctx.state.hideCompleted ? 'checked' : ''}>
                <label for="settle-hide-completed" class="field-label" style="margin:0;cursor:pointer;">隱藏已供滿</label>
              </div>
            </div>
          `,
        },
      ],
    });

    const onFilterChange = (e) => {
      const cb = e.target.closest('#settle-hide-completed');
      if (cb) {
        ctx.state.hideCompleted = cb.checked;
        try { localStorage.setItem(HIDE_COMPLETED_KEY, String(cb.checked)); } catch (err) {}
        reload();
      }
    };
    const controlsRoot = document.getElementById('page-controls-root');
    controlsRoot?.addEventListener('change', onFilterChange);

    const unsub = ctx.onDataChange((key) => {
      if (key === '__APP__') {
        reload();
        if (_controls) { try { _controls.refresh(); } catch (e) {} }
      }
    });

    reload();

    return {
      destroy: () => {
        unsub();
        if (_monthlyUnsub) { try { _monthlyUnsub(); } catch (e) {} }
        if (_statsApi) { try { _statsApi.destroy(); } catch (e) {} }
        if (_tableApi) { try { _tableApi.destroy(); } catch (e) {} }
        if (_cardApi) { try { _cardApi.destroy(); } catch (e) {} }
        if (_controls) { try { _controls.destroy(); } catch (e) {} }
        if (controlsRoot && onFilterChange) controlsRoot.removeEventListener('change', onFilterChange);
      },
    };
  },
};

/* ============================================
   保險扣款：確認 / 取消
   ============================================ */
async function _onInsuranceSync(row, onSuccess) {
  const ok = await _openBankPickerModal(row);
  if (!ok) return;

  try {
    await api.insuranceSync({
      policyId: row._ref?.policyId,
      memberId: row._ref?.memberId || row.memberId,
      policyName: row.name,
      monthlyAverage: row.amount,
      year: row.year,
      month: row.month,
      bankId: ok.bankId || '',
      txnId: '',
      paymentMode: row.paymentMode || 'direct',
      advanceHolderId: row.advanceHolderId || '',
    });
    showToast('✅ 已確認扣款（連動支出 / 基金 / 代墊）', 'success');
    if (typeof onSuccess === 'function') onSuccess();
  } catch (err) {
    showToast('確認扣款失敗：' + err.message, 'error');
  }
}

async function _onInsuranceUnsync(row, onSuccess) {
  const ok = await openConfirm(
    `⚠️ 確定要取消「${row.name}」本月的扣款嗎？\n\n將同步撤銷：\n• 支出紀錄\n• 基金累積供款\n• 代墊扣減`,
    { title: '取消扣款', okText: '取消扣款', okClass: 'btn-danger' }
  );
  if (!ok) return;

  try {
    await api.insuranceUnsync({
      policyId: row._ref?.policyId,
      memberId: row._ref?.memberId || row.memberId,
      year: row.year,
      month: row.month,
      amount: row.amount,
      paymentMode: row.paymentMode || 'direct',
      advanceHolderId: row.advanceHolderId || '',
    });
    showToast('✅ 已取消扣款', 'success');
    if (typeof onSuccess === 'function') onSuccess();
  } catch (err) {
    showToast('取消扣款失敗：' + err.message, 'error');
  }
}

async function _openBankPickerModal(row) {
  document.getElementById(INS_MODAL_ID)?.remove();

  let banks = [];
  try { banks = await getBankAccountsOnce(); } catch (e) { banks = []; }

  return new Promise((resolve) => {
    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = INS_MODAL_ID;
    overlay.innerHTML = `
      <div class="modal" style="max-width:440px;">
        <h2 class="modal-title">確認扣款 — ${esc(row.name)}</h2>
        <div class="glass-card" style="padding:12px;background:rgba(8,11,17,0.4);margin-bottom:12px;">
          <div style="display:flex;justify-content:space-between;font-size:13px;">
            <span class="text-muted">金額</span>
            <span class="mono text-cyan" style="font-weight:700;">${formatHKD(row.amount)}</span>
          </div>
          <div style="display:flex;justify-content:space-between;font-size:13px;margin-top:6px;">
            <span class="text-muted">所屬年月</span>
            <span class="mono">${esc(row.year)}-${esc(row.month)}</span>
          </div>
        </div>
        <div class="field">
          <label class="field-label">出帳銀行（可選）</label>
          <select class="select" id="${INS_MODAL_ID}-bank">
            <option value="">— 不指定 —</option>
            ${banks.map((b) => `<option value="${esc(b.id)}">${esc(b.name)}</option>`).join('')}
          </select>
        </div>
        <div class="modal-actions" style="margin-top:16px;">
          <button type="button" class="btn btn-ghost" data-action="cancel">取消</button>
          <button type="button" class="btn btn-primary" data-action="ok"><i data-lucide="check" style="width:14px;height:14px;"></i> 確認扣款</button>
        </div>
      </div>
    `;
    document.body.appendChild(overlay);

    const close = (result) => {
      closeModal(INS_MODAL_ID);
      overlay.remove();
      resolve(result);
    };

    overlay.addEventListener('click', (e) => {
      if (e.target === overlay) { close(null); return; }
      const btn = e.target.closest('button[data-action]');
      if (!btn) return;
      if (btn.dataset.action === 'cancel') { close(null); return; }
      if (btn.dataset.action === 'ok') {
        const sel = document.getElementById(`${INS_MODAL_ID}-bank`);
        close({ bankId: sel?.value || '' });
      }
    });

    openModal(INS_MODAL_ID);
    if (window.lucide) window.lucide.createIcons();
  });
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/pages/settlements.js
   Version: v103.0.35
   Batch: B29-7a
   ═══════════════════════════════════════════ */```

---

## 📄 js/shared/bank-account-manager.js

```
// ============================================
// bank-account-manager.js — 銀行帳號管理元件（v103.0.11）
// 位置：js/shared/bank-account-manager.js
// ============================================
// v103.0.11 修正：
//   ✅ [H04] escapeHtml / formatHKD 改從 lib/ 導入
// ============================================

import { api } from '../core/api.js';
import { AppState } from '../core/state.js';
import { esc as escapeHtml } from '../lib/dom.js';
import { formatHKD } from '../lib/format.js';
import { showToast } from '../ui/toast.js';
import { openModal, closeModal, openConfirm } from '../ui/modal.js';
import { buildForm } from '../ui/form-builder.js';

/* ============================================
   Module 狀態
   ============================================ */
let _containerId = '';
let _accounts = [];
let _canInput = false;
let _listHandler = null;
let _formApi = null;

const EDIT_MODAL_ID = 'bank-account-edit-modal';

/* ============================================
   主入口
   ============================================ */
export function initBankAccountManager(containerId, options = {}) {
  _containerId = containerId;
  _canInput = options.canInput !== false;

  const root = document.getElementById(containerId);
  if (!root) {
    console.warn(`⚠️ initBankAccountManager: 找不到容器 #${containerId}`);
    return null;
  }

  _renderSkeleton();
  _bindEvents();
  _loadAccounts();

  return {
    refresh: _loadAccounts,
    destroy: _destroy,
  };
}

/* ============================================
   骨架
   ============================================ */
function _renderSkeleton() {
  const root = document.getElementById(_containerId);
  if (!root) return;

  const addBtnHtml = _canInput ? `
    <button type="button" class="btn btn-primary" id="${_containerId}-add-btn">
      <i data-lucide="plus" style="width:14px;height:14px;"></i> 新增銀行帳號
    </button>
  ` : '';

  const dangerZoneHtml = _canInput ? `
    <div class="glass-card" style="margin-top:16px; border-color:rgba(244,63,94,0.3); background:rgba(244,63,94,0.03);">
      <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:12px; color:var(--neon-red);">
        <i data-lucide="alert-triangle" style="width:14px;height:14px;"></i>
        <span>危險區域</span>
      </div>
      <p class="glass-card-hint mb-12">
        v102.0.0 銀行系統重構後，舊的「銀行結餘」資料已廢除。若有殘留舊資料，可在此一次清除。
      </p>
      <button type="button" class="btn btn-danger" id="${_containerId}-clear-old-btn">
        <i data-lucide="trash-2" style="width:14px;height:14px;"></i> 清除舊銀行結餘資料
      </button>
    </div>
  ` : '';

  root.innerHTML = `
    <div class="flex flex-between items-center flex-wrap gap-12 mb-16">
      <div class="text-muted" style="font-size:13px;">
        共 <span id="${_containerId}-count">0</span> 個銀行帳號
      </div>
      <div class="flex items-center gap-8 flex-wrap">
        ${addBtnHtml}
      </div>
    </div>

    <div id="${_containerId}-list"></div>

    ${dangerZoneHtml}
  `;

  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   事件綁定
   ============================================ */
function _bindEvents() {
  const root = document.getElementById(_containerId);
  if (!root) return;

  if (_canInput) {
    const addBtn = document.getElementById(`${_containerId}-add-btn`);
    if (addBtn) addBtn.addEventListener('click', _handleAdd);

    const clearBtn = document.getElementById(`${_containerId}-clear-old-btn`);
    if (clearBtn) clearBtn.addEventListener('click', _handleClearOldBalances);
  }

  const listEl = document.getElementById(`${_containerId}-list`);
  if (!listEl) return;

  if (_listHandler) {
    listEl.removeEventListener('click', _listHandler);
  }

  _listHandler = async (e) => {
    const btn = e.target.closest('button[data-action]');
    if (!btn) return;

    const action = btn.dataset.action;
    const id = btn.dataset.id;

    if (action === 'edit') {
      await _handleEdit(id);
    } else if (action === 'delete') {
      await _handleDelete(id);
    }
  };

  listEl.addEventListener('click', _listHandler);
}

/* ============================================
   載入帳號
   ============================================ */
async function _loadAccounts() {
  const listEl = document.getElementById(`${_containerId}-list`);
  const countEl = document.getElementById(`${_containerId}-count`);
  if (!listEl) return;

  listEl.innerHTML = '<div class="empty-state">載入中…</div>';

  try {
    const result = await api.bankAccounts.list();
    _accounts = result.accounts || [];
    if (countEl) countEl.textContent = String(_accounts.length);
    _renderList();
  } catch (err) {
    console.error('[bank-account-manager] 載入失敗：', err);
    listEl.innerHTML = `<div class="empty-state text-red">載入失敗：${escapeHtml(err.message)}</div>`;
  }
}

/* ============================================
   渲染列表
   ============================================ */
function _renderList() {
  const listEl = document.getElementById(`${_containerId}-list`);
  if (!listEl) return;

  if (_accounts.length === 0) {
    listEl.innerHTML = `
      <div class="glass-card">
        <div class="empty-state">
          <i data-lucide="landmark" style="width:48px;height:48px;opacity:0.4;"></i>
          <p style="margin-top:12px;">尚無銀行帳號</p>
          ${_canInput ? '<p style="margin-top:6px; font-size:12px; color:var(--text-muted);">請點擊上方「新增銀行帳號」建立第一個帳號</p>' : ''}
        </div>
      </div>
    `;
    if (window.lucide) window.lucide.createIcons();
    return;
  }

  listEl.innerHTML = `
    <div style="overflow-x:auto;">
      <table class="data-table">
        <thead>
          <tr>
            <th style="width:60px;">排序</th>
            <th>名稱</th>
            <th style="width:100px;">類型</th>
            <th class="num" style="width:130px;">初始餘額</th>
            <th style="width:110px;">初始年月</th>
            ${_canInput ? '<th style="width:160px;">操作</th>' : ''}
          </tr>
        </thead>
        <tbody>
          ${_accounts.map((acc) => _renderRow(acc)).join('')}
        </tbody>
      </table>
    </div>
  `;

  if (window.lucide) window.lucide.createIcons();
}

function _renderRow(acc) {
  const typeLabel = acc.type === 'personal' ? '👤 個人' : '🏠 家庭';
  const initYM = `${acc.initialYear || '—'}-${acc.initialMonth || '—'}`;

  const actionsCell = _canInput ? `
    <td>
      <button type="button" class="btn btn-sm btn-ghost" data-action="edit" data-id="${escapeHtml(acc.id)}">
        <i data-lucide="pencil" style="width:12px;height:12px;"></i> 編輯
      </button>
      <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-id="${escapeHtml(acc.id)}">
        <i data-lucide="trash-2" style="width:12px;height:12px;"></i> 刪除
      </button>
    </td>
  ` : '';

  return `
    <tr>
      <td class="mono" style="color:var(--text-muted);">${acc.order || 0}</td>
      <td style="font-weight:500;">${escapeHtml(acc.name || '')}</td>
      <td style="font-size:12px;">${typeLabel}</td>
      <td class="num mono">${formatHKD(acc.initialBalance)}</td>
      <td class="mono" style="font-size:12px; color:var(--text-muted);">${initYM}</td>
      ${actionsCell}
    </tr>
  `;
}

/* ============================================
   新增 / 編輯
   ============================================ */
async function _handleAdd() {
  if (!_canInput) return;
  await _openEditModal(null);
}

async function _handleEdit(id) {
  if (!_canInput) return;
  const acc = _accounts.find((a) => a.id === id);
  if (!acc) {
    showToast('找不到此銀行帳號', 'error');
    return;
  }
  await _openEditModal(acc);
}

async function _openEditModal(account) {
  const isEdit = !!account;
  document.getElementById(EDIT_MODAL_ID)?.remove();

  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = EDIT_MODAL_ID;
  overlay.innerHTML = `
    <div class="modal" style="max-width:520px; max-height:90vh; overflow-y:auto;">
      <h2 class="modal-title">${isEdit ? '編輯' : '新增'}銀行帳號</h2>
      <div id="${EDIT_MODAL_ID}-form-root"></div>
    </div>
  `;
  document.body.appendChild(overlay);

  overlay.addEventListener('click', (e) => {
    if (e.target === overlay) closeModal(EDIT_MODAL_ID);
  });

  const currentYear = new Date().getFullYear();
  const yearOptions = [];
  for (let y = currentYear - 3; y <= currentYear + 1; y++) {
    yearOptions.push({ value: String(y), label: `${y} 年` });
  }
  const monthOptions = [];
  for (let m = 1; m <= 12; m++) {
    const mm = String(m).padStart(2, '0');
    monthOptions.push({ value: mm, label: `${m} 月` });
  }

  const initialData = isEdit ? {
    name: account.name || '',
    type: account.type || 'family',
    initialBalance: account.initialBalance || 0,
    initialYear: account.initialYear || String(currentYear),
    initialMonth: account.initialMonth || '01',
    order: account.order || 0,
  } : {
    type: 'family',
    initialBalance: 0,
    initialYear: String(currentYear),
    initialMonth: '01',
    order: _accounts.length,
  };

  _formApi = buildForm({
    containerId: `${EDIT_MODAL_ID}-form-root`,
    fields: [
      { type: 'text', id: 'name', label: '銀行名稱', required: true, maxlength: 20, placeholder: '例如：中銀、匯豐' },
      {
        type: 'select', id: 'type', label: '類型', required: true, includeEmpty: false,
        options: [
          { value: 'family', label: '🏠 家庭帳號' },
          { value: 'personal', label: '👤 個人帳號' },
        ],
        defaultValue: 'family',
      },
      {
        type: 'number', id: 'initialBalance', label: '初始餘額（HK$）', required: true,
        min: 0, step: 1, placeholder: '0',
        hint: '此帳號在「初始年月」時的餘額',
      },
      {
        type: 'select', id: 'initialYear', label: '初始年份', required: true,
        includeEmpty: false, options: yearOptions,
      },
      {
        type: 'select', id: 'initialMonth', label: '初始月份', required: true,
        includeEmpty: false, options: monthOptions,
      },
      {
        type: 'number', id: 'order', label: '排序', min: 0, step: 1, defaultValue: 0,
        hint: '數字越小越前面',
      },
    ],
    submitText: isEdit ? '儲存' : '新增',
    showCancel: true,
    cancelText: '取消',
    initialData,
    beforeSubmit: (data) => {
      const name = (data.name || '').trim();
      if (!name) return { field: 'name', message: '請填寫銀行名稱' };
      const dup = _accounts.find((a) =>
        (isEdit ? a.id !== account.id : true) && a.name === name
      );
      if (dup) return { field: 'name', message: '此銀行名稱已存在' };
      return true;
    },
    onSubmit: async (data) => {
      try {
        const payload = {
          name: data.name.trim(),
          type: data.type,
          initialBalance: Number(data.initialBalance) || 0,
          initialYear: data.initialYear,
          initialMonth: data.initialMonth,
          order: Number(data.order) || 0,
        };

        if (isEdit) {
          await api.bankAccounts.update(account.id, payload);
          showToast('✅ 已更新銀行帳號', 'success');
        } else {
          await api.bankAccounts.create(payload);
          showToast('✅ 已新增銀行帳號', 'success');
        }
        closeModal(EDIT_MODAL_ID);
        await _loadAccounts();
      } catch (err) {
        showToast((isEdit ? '更新' : '新增') + '失敗：' + err.message, 'error');
      }
    },
    onCancel: () => closeModal(EDIT_MODAL_ID),
  });

  openModal(EDIT_MODAL_ID);
  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   刪除
   ============================================ */
async function _handleDelete(id) {
  if (!_canInput) return;
  const acc = _accounts.find((a) => a.id === id);
  if (!acc) return;

  const ok = await openConfirm(
    `⚠️ 確定要刪除「${acc.name}」嗎？\n\n若此帳號被支出 / 保險扣款引用，系統會拒絕刪除並提示。`,
    { title: '刪除銀行帳號', okText: '刪除', okClass: 'btn-danger' }
  );
  if (!ok) return;

  try {
    const result = await api.bankAccounts.remove(id);
    const txnCount = result.removedTxnCount || 0;
    showToast(
      txnCount > 0
        ? `✅ 已刪除（連同 ${txnCount} 筆交易）`
        : '✅ 已刪除',
      'success'
    );
    await _loadAccounts();
  } catch (err) {
    if (err.status === 409 || (err.message && err.message.includes('仍被'))) {
      showToast('此帳號仍被引用，請先移除引用', 'warning', 4000);
      _showConflictModal(acc, err.message);
      return;
    }
    showToast('刪除失敗：' + err.message, 'error');
  }
}

function _showConflictModal(account, message) {
  const MODAL_ID = 'bank-account-conflict-modal';
  document.getElementById(MODAL_ID)?.remove();

  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = MODAL_ID;
  overlay.style.zIndex = '1100';
  overlay.innerHTML = `
    <div class="modal" style="max-width:480px;">
      <h2 class="modal-title" style="color:var(--neon-orange);">
        <i data-lucide="alert-triangle" style="width:18px;height:18px;"></i>
        無法刪除
      </h2>
      <div class="banner" style="border-color:rgba(251,146,60,0.3); background:rgba(251,146,60,0.06); color:var(--neon-orange); margin-bottom:16px;">
        ${escapeHtml(message || '此銀行帳號仍被其他記錄引用。')}
      </div>
      <div style="font-size:13px; color:var(--text-secondary); line-height:1.7; margin-bottom:16px;">
        請依下列步驟處理：
        <ol style="padding-left:20px; margin-top:8px;">
          <li>前往「綜合輸入中心 → 銀行 Tab」查看此帳號的交易</li>
          <li>或前往「結算清單」編輯相關支出 / 保險扣款</li>
          <li>將「出帳銀行」改為其他帳號，或選擇「不關聯銀行」</li>
          <li>再回到此處重新刪除</li>
        </ol>
      </div>
      <div class="modal-actions">
        <button type="button" class="btn btn-primary" data-action="close">了解</button>
      </div>
    </div>
  `;
  document.body.appendChild(overlay);

  overlay.addEventListener('click', (e) => {
    if (e.target === overlay || e.target.closest('button[data-action="close"]')) {
      closeModal(MODAL_ID);
    }
  });

  openModal(MODAL_ID);
  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   清除舊銀行結餘
   ============================================ */
async function _handleClearOldBalances() {
  if (!_canInput) return;

  const ok = await openConfirm(
    '⚠️ 確定要清除所有「舊版銀行結餘」資料嗎？\n\n' +
    '這會刪除 families/{familyId}/bank_balances/ 下的所有資料。\n\n' +
    'v102.0.0 銀行系統重構後，舊資料已不再使用，此操作無法復原。',
    { title: '清除舊銀行結餘', okText: '確定清除', okClass: 'btn-danger' }
  );
  if (!ok) return;

  try {
    const result = await api.clearBankBalances();
    if (result.cleared) {
      showToast(`✅ 已清除 ${result.recordCount || 0} 筆舊資料`, 'success');
    } else {
      showToast('無舊資料可清除', 'info');
    }
  } catch (err) {
    showToast('清除失敗：' + err.message, 'error');
  }
}

/* ============================================
   銷毀
   ============================================ */
function _destroy() {
  const root = document.getElementById(_containerId);
  if (root) {
    const listEl = document.getElementById(`${_containerId}-list`);
    if (listEl && _listHandler) {
      listEl.removeEventListener('click', _listHandler);
    }
  }
  if (_formApi) { try { _formApi.destroy(); } catch (e) {} _formApi = null; }
  document.getElementById(EDIT_MODAL_ID)?.remove();
  document.getElementById('bank-account-conflict-modal')?.remove();
  _accounts = [];
  _listHandler = null;
  _containerId = '';
}```

---

## 📄 js/shared/column-settings.js

```
// ============================================
// column-settings.js — 表格欄位設定（v103.0.22）
// 位置：js/shared/column-settings.js
// ============================================
// v103.0.22（B25-2）：
//   ✅ H4：closeModal + overlay.remove 改用 destroyModal（統一封裝）
// v103.0.11 修正：
//   ✅ [H04] escapeHtml 改從 lib/dom.js 導入
//   ✅ [H05] 改用 ui/modal.js 的 openModal / closeModal
// ============================================

import { buildColumnSettingsKey } from '../config/constants.js';
import { esc as escapeHtml } from '../lib/dom.js';
import { openModal, destroyModal } from '../ui/modal.js';

/* ============================================
   主函式
   ============================================ */
export function initColumnSettings(options) {
  const {
    tableId,
    columns = [],
    defaultVisible,
  } = options;

  if (!tableId) throw new Error('initColumnSettings: tableId 為必填');
  if (!columns.length) throw new Error('initColumnSettings: columns 不可為空');

  const storageKey = buildColumnSettingsKey(tableId);

  const _defaultVisible = defaultVisible
    || columns.filter((c) => c.defaultVisible !== false).map((c) => c.id);
  const _defaultOrder = columns.map((c) => c.id);
  const _defaultWidths = {};
  columns.forEach((c) => {
    if (c.defaultWidth != null) _defaultWidths[c.id] = c.defaultWidth;
  });

  let _visible = [..._defaultVisible];
  let _order = [..._defaultOrder];
  let _widths = { ..._defaultWidths };

  _loadFromStorage();

  function _loadFromStorage() {
    try {
      const raw = localStorage.getItem(storageKey);
      if (!raw) return;
      const data = JSON.parse(raw);

      if (Array.isArray(data.visible)) {
        _visible = data.visible.filter((id) => columns.some((c) => c.id === id));
        columns.forEach((c) => {
          if (c.defaultVisible !== false && !_visible.includes(c.id)) {
            _visible.push(c.id);
          }
        });
      }

      if (Array.isArray(data.order)) {
        const validOrder = data.order.filter((id) => columns.some((c) => c.id === id));
        columns.forEach((c) => {
          if (!validOrder.includes(c.id)) validOrder.push(c.id);
        });
        _order = validOrder;
      }

      if (data.widths && typeof data.widths === 'object') {
        _widths = { ..._defaultWidths, ...data.widths };
      }
    } catch (e) {
      console.warn('[column-settings] 讀取失敗：', e);
    }
  }

  function _saveToStorage() {
    try {
      localStorage.setItem(storageKey, JSON.stringify({
        visible: _visible,
        order: _order,
        widths: _widths,
      }));
    } catch (e) {
      console.warn('[column-settings] 儲存失敗：', e);
    }
  }

  function getVisibleColumns() {
    return _order
      .filter((id) => _visible.includes(id))
      .map((id) => columns.find((c) => c.id === id))
      .filter(Boolean);
  }

  function getAllColumns() {
    return _order
      .map((id) => columns.find((c) => c.id === id))
      .filter(Boolean)
      .map((c) => ({
        ...c,
        visible: _visible.includes(c.id),
        width: _widths[c.id] ?? null,
      }));
  }

  function getWidth(columnId) {
    return _widths[columnId] ?? null;
  }

  function setVisible(ids) {
    _visible = ids.filter((id) => columns.some((c) => c.id === id));
    if (_visible.length === 0) {
      _visible = [_defaultOrder[0]];
    }
    _saveToStorage();
  }

  function setOrder(ids) {
    _order = ids.filter((id) => columns.some((c) => c.id === id));
    columns.forEach((c) => {
      if (!_order.includes(c.id)) _order.push(c.id);
    });
    _saveToStorage();
  }

  function setWidth(columnId, width) {
    if (width == null || width <= 0) {
      delete _widths[columnId];
    } else {
      _widths[columnId] = Number(width);
    }
    _saveToStorage();
  }

  function reset() {
    _visible = [..._defaultVisible];
    _order = [..._defaultOrder];
    _widths = { ..._defaultWidths };
    _saveToStorage();
  }

  function openPanel(options = {}) {
    _renderPanel(options.onChange);
  }

  function _renderPanel(onChange) {
    const MODAL_ID = `column-settings-panel-${tableId}`;
    let overlay = document.getElementById(MODAL_ID);
    if (overlay) overlay.remove();

    overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = MODAL_ID;
    overlay.innerHTML = `
      <div class="modal" style="max-width:480px; max-height:90vh; overflow-y:auto;">
        <h2 class="modal-title">欄位設定</h2>
        <p style="font-size:12px; color:var(--text-muted); margin-bottom:16px;">
          勾選顯示、用 ↑↓ 調整順序、輸入寬度（px）
        </p>
        <div id="${MODAL_ID}-list"></div>
        <div class="modal-actions" style="margin-top:20px;">
          <button type="button" class="btn btn-ghost" data-action="reset">
            <i data-lucide="rotate-ccw" style="width:14px;height:14px;"></i> 重置
          </button>
          <button type="button" class="btn btn-ghost" data-action="cancel">取消</button>
          <button type="button" class="btn btn-primary" data-action="save">儲存</button>
        </div>
      </div>
    `;
    document.body.appendChild(overlay);

    const listEl = overlay.querySelector(`#${MODAL_ID}-list`);
    const tempState = getAllColumns().map((c) => ({ ...c }));

    function renderList() {
      listEl.innerHTML = tempState.map((c, i) => {
        const isFirst = i === 0;
        const isLast = i === tempState.length - 1;

        return `
          <div class="column-settings-row" data-id="${escapeHtml(c.id)}" data-index="${i}"
               style="display:flex; align-items:center; gap:8px; padding:10px 8px; border-bottom:1px solid rgba(255,255,255,0.05);">
            <div style="display:flex; flex-direction:column; gap:2px; flex-shrink:0;">
              <button type="button" class="btn btn-sm btn-ghost" data-role="move-up"
                      ${isFirst ? 'disabled' : ''}
                      style="padding:2px 6px; line-height:1; min-width:auto;">
                <i data-lucide="chevron-up" style="width:14px;height:14px;"></i>
              </button>
              <button type="button" class="btn btn-sm btn-ghost" data-role="move-down"
                      ${isLast ? 'disabled' : ''}
                      style="padding:2px 6px; line-height:1; min-width:auto;">
                <i data-lucide="chevron-down" style="width:14px;height:14px;"></i>
              </button>
            </div>
            <label style="display:flex; align-items:center; gap:8px; flex:1; cursor:pointer; min-width:0;">
              <input type="checkbox" data-role="visible" ${c.visible ? 'checked' : ''}
                     style="width:auto; cursor:pointer; flex-shrink:0;">
              <span style="font-size:13px; word-break:break-word;">${escapeHtml(c.label)}</span>
            </label>
            <input type="number" data-role="width" value="${c.width ?? ''}"
                   placeholder="自動" min="40" max="500" step="10"
                   style="width:70px; padding:4px 8px; font-size:12px; background:rgba(8,11,17,0.6); border:1px solid var(--glass-border); border-radius:var(--radius-sm); color:var(--text-primary); flex-shrink:0;">
            <span style="font-size:11px; color:var(--text-muted); flex-shrink:0;">px</span>
          </div>
        `;
      }).join('');

      if (window.lucide) window.lucide.createIcons();
    }

    renderList();

    listEl.addEventListener('change', (e) => {
      const checkbox = e.target.closest('input[data-role="visible"]');
      if (checkbox) {
        const row = checkbox.closest('.column-settings-row');
        const id = row.dataset.id;
        const item = tempState.find((c) => c.id === id);
        if (item) item.visible = checkbox.checked;
        return;
      }
      const widthInput = e.target.closest('input[data-role="width"]');
      if (widthInput) {
        const row = widthInput.closest('.column-settings-row');
        const id = row.dataset.id;
        const item = tempState.find((c) => c.id === id);
        if (item) item.width = widthInput.value ? Number(widthInput.value) : null;
      }
    });

    listEl.addEventListener('click', (e) => {
      const btn = e.target.closest('button[data-role]');
      if (!btn) return;

      const role = btn.dataset.role;
      const row = btn.closest('.column-settings-row');
      if (!row) return;

      const index = Number(row.dataset.index);
      if (isNaN(index)) return;

      if (role === 'move-up' && index > 0) {
        [tempState[index - 1], tempState[index]] = [tempState[index], tempState[index - 1]];
        renderList();
      } else if (role === 'move-down' && index < tempState.length - 1) {
        [tempState[index], tempState[index + 1]] = [tempState[index + 1], tempState[index]];
        renderList();
      }
    });

    overlay.addEventListener('click', (e) => {
      const btn = e.target.closest('.modal-actions button[data-action]');
      if (btn) {
        const action = btn.dataset.action;

        if (action === 'reset') {
          reset();
          tempState.length = 0;
          tempState.push(...getAllColumns());
          renderList();
          if (typeof onChange === 'function') onChange();
          return;
        }

        if (action === 'cancel') {
          /* 🆕 v103.0.22：用 destroyModal 統一（closeModal + remove） */
          destroyModal(MODAL_ID);
          return;
        }

        if (action === 'save') {
          setVisible(tempState.filter((c) => c.visible).map((c) => c.id));
          setOrder(tempState.map((c) => c.id));
          tempState.forEach((c) => setWidth(c.id, c.width));
          destroyModal(MODAL_ID);
          if (typeof onChange === 'function') onChange();
          return;
        }
      }

      if (e.target === overlay) {
        destroyModal(MODAL_ID);
      }
    });

    openModal(MODAL_ID);
    if (window.lucide) window.lucide.createIcons();
  }

  return {
    getVisibleColumns,
    getAllColumns,
    getWidth,
    setVisible,
    setOrder,
    setWidth,
    reset,
    openPanel,
  };
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/shared/column-settings.js
   Version: v103.0.22
   Batch: B25-2
   ═══════════════════════════════════════════ */```

---

## 📄 js/shared/data-card.js

```
// ============================================
// data-card.js — 通用卡片渲染（v103.0.22）
// v103.0.22（B25-3a）：
//   ✅ R3 修正：hasActions 補上 customActions 判斷（anchor 0.21）
//   ✅ customActions 與引擎預設互斥
// ============================================
import { getEntityDef, getEntityUi } from '../entity/entity-definitions.js';
import { esc as escapeHtml } from '../lib/dom.js';
import { formatCellValue } from '../lib/format.js';

const LISTENER_KEY = '__dcClickListener';

export function renderDataCard(options) {
  const { container, entityKey, rows, options: extra = {}, hooks = {} } = options;
  const root = _resolveEl(container);
  if (!root) return null;

  const def = getEntityDef(entityKey);
  const customCols = extra.columns || extra.fields;
  const hasCustomCols = Array.isArray(customCols) && customCols.length > 0;
  if (!def && !hasCustomCols) return null;

  const effectiveDef = def || { fields: [] };
  const effectiveUi = def ? (getEntityUi(entityKey) || {}) : { canEdit: false, canDelete: false };
  const { gridClass = 'grid grid-3', resolvers = {} } = extra;

  /* 🆕 v103.0.22（anchor 0.21） */
  const hasCustomActions = typeof hooks.customActions === 'function';
  const hasBuiltin = (effectiveUi.canEdit !== false) || (effectiveUi.canDelete !== false);
  const allowBuiltin = !hasCustomActions || extra.disableBuiltinActions === false;
  const hasActions = hasCustomActions || (hasBuiltin && allowBuiltin);

  const fieldMap = {};
  (effectiveDef.fields || []).forEach((f) => { fieldMap[f.id] = f; });

  const colMap = {};
  (customCols || []).forEach((c) => { colMap[c.id] = c; });

  const primaryColumn = effectiveUi.primaryColumn || null;
  const cardFields = effectiveUi.cardFields
    || (customCols ? customCols.slice(1, 4).map((c) => c.id) : (effectiveDef.fields || []).filter((f) => f.id !== primaryColumn).slice(0, 3).map((f) => f.id));

  const _rows = typeof hooks.beforeRender === 'function' ? (hooks.beforeRender(rows) || rows) : rows;

  _paint();

  function _paint() {
    if (!_rows || _rows.length === 0) {
      _render(root, '<div class="glass-card"><div class="empty-state">尚無資料</div></div>');
      return;
    }
    const html = `<div class="${gridClass}" style="gap:12px;">${_rows.map((r, i) => _card(r, i)).join('')}</div>`;
    _render(root, html);
    _bind();
    if (typeof hooks.afterRender === 'function') hooks.afterRender(root);
  }

  function _card(row, i) {
    const header = typeof hooks.customHeader === 'function' ? (hooks.customHeader(row) || _defaultHeader(row)) : _defaultHeader(row);
    const body = typeof hooks.customBody === 'function' ? (hooks.customBody(row) || _defaultBody(row)) : _defaultBody(row);
    const footer = typeof hooks.customFooter === 'function' ? (hooks.customFooter(row) || _defaultFooter(row)) : _defaultFooter(row);
    return `<div class="glass-card data-card" data-row-index="${i}">${header}${body}${footer}</div>`;
  }

  function _defaultHeader(row) {
    const v = primaryColumn ? row[primaryColumn] : (row.name || '（未命名）');
    return `<div class="data-card-primary" style="font-size:15px;font-weight:700;color:var(--neon-cyan);margin-bottom:10px;word-break:break-word;">${escapeHtml(v || '（未命名）')}</div>`;
  }

  function _defaultBody(row) {
    if (!cardFields.length) return '';
    return `<div class="data-card-fields" style="display:flex;flex-direction:column;gap:6px;font-size:13px;">${cardFields.map((id) => {
      const field = fieldMap[id] || colMap[id];
      if (!field) return '';
      const val = row[id];
      const r = resolvers[id];
      const content = r ? r(val, row) : formatCellValue(val, field.type);
      return `<div class="data-card-field" style="display:flex;justify-content:space-between;gap:12px;"><span class="data-card-label" style="color:var(--text-muted);font-size:11px;flex-shrink:0;">${escapeHtml(field.label)}</span><span class="data-card-value" style="text-align:right;word-break:break-word;">${content}</span></div>`;
    }).join('')}</div>`;
  }

  function _defaultFooter(row) {
    if (!hasActions) return '';
    let h = '';
    if (hasCustomActions) {
      h += (hooks.customActions(row) || []).map((a) => `<button type="button" class="btn btn-sm ${escapeHtml(a.className || 'btn-ghost')}" data-action="${escapeHtml(a.action || 'custom')}">${a.icon ? `<i data-lucide="${escapeHtml(a.icon)}" style="width:14px;height:14px;"></i>` : ''}${escapeHtml(a.label)}</button>`).join('');
    }
    if (allowBuiltin) {
      if (effectiveUi.canEdit !== false) h += `<button type="button" class="btn btn-sm btn-ghost" data-action="edit"><i data-lucide="pencil" style="width:14px;height:14px;"></i> 編輯</button>`;
      if (effectiveUi.canDelete !== false) h += `<button type="button" class="btn btn-sm btn-danger" data-action="delete"><i data-lucide="trash-2" style="width:14px;height:14px;"></i> 刪除</button>`;
    }
    if (!h) return '';
    return `<div class="data-card-footer" style="margin-top:12px;padding-top:12px;border-top:1px dashed rgba(255,255,255,0.08);display:flex;gap:8px;justify-content:flex-end;flex-wrap:wrap;">${h}</div>`;
  }

  function _render(el, html) {
    if (el.__lastHtml === html) return;
    el.__lastHtml = html;
    el.innerHTML = html;
    if (window.lucide) window.lucide.createIcons();
  }

  function _bind() {
    const old = root[LISTENER_KEY];
    if (old) root.removeEventListener('click', old);
    const listener = (e) => {
      const btn = e.target.closest('button[data-action]');
      if (btn) {
        const card = btn.closest('[data-row-index]');
        if (!card) return;
        const row = _rows[Number(card.dataset.rowIndex)];
        if (!row) return;
        const action = btn.dataset.action;
        if (hasCustomActions) {
          const m = (hooks.customActions(row) || []).find((a) => (a.action || 'custom') === action);
          if (m?.onClick) { e.stopPropagation(); m.onClick(row); return; }
        }
        if (action === 'edit' && hooks.onEdit) { e.stopPropagation(); hooks.onEdit(row); return; }
        if (action === 'delete' && hooks.onDelete) { e.stopPropagation(); hooks.onDelete(row); return; }
        return;
      }
      const card = e.target.closest('.data-card[data-row-index]');
      if (card && hooks.onCardClick) hooks.onCardClick(_rows[Number(card.dataset.rowIndex)]);
    };
    root.addEventListener('click', listener);
    root[LISTENER_KEY] = listener;
  }

  return {
    container: root,
    refresh: _paint,
    destroy: () => {
      const old = root[LISTENER_KEY];
      if (old) root.removeEventListener('click', old);
      root.innerHTML = '';
      root.__lastHtml = '';
    },
  };
}

function _resolveEl(t) {
  if (typeof t === 'string') return document.getElementById(t);
  if (t instanceof HTMLElement) return t;
  return null;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/shared/data-card.js
   Version: v103.0.22
   Batch: B25-3a
   ═══════════════════════════════════════════ */```

---

## 📄 js/shared/data-table.js

```
// ============================================
// data-table.js — 通用表格渲染（v103.0.34）
// v103.0.34（B29-1c）：
//   ✅ 加表頭排序（3 態：升 / 降 / 清除）
//   ✅ 加表頭篩選（下拉面板）
//   ✅ 依 type 判斷可排序；依 filterOptions 可篩選
// ============================================
import { getEntityDef, getEntityUi } from '../entity/entity-definitions.js';
import { esc as escapeHtml } from '../lib/dom.js';
import { formatCellValue } from '../lib/format.js';
import { initColumnSettings } from './column-settings.js';

const LISTENER_KEY = '__dtClickListener';
const COLLAPSE_PREFIX = 'fin_ui_collapse_';
const SEARCH_INPUT_ID_PREFIX = '__dt_search_';

function _render(root, html) {
  if (root.__lastHtml === html) return false;
  root.__lastHtml = html;
  root.innerHTML = html;
  if (window.lucide) window.lucide.createIcons();
  return true;
}

export function renderDataTable(options) {
  const { container, entityKey, rows, tableId, options: extra = {}, hooks = {} } = options;
  const root = _resolveEl(container);
  if (!root) return null;

  const def = getEntityDef(entityKey);
  const customCols = extra.columns;
  const hasCustomCols = Array.isArray(customCols) && customCols.length > 0;
  if (!def && !hasCustomCols) return null;

  const effectiveDef = def || { fields: [] };
  const effectiveUi = def ? (getEntityUi(entityKey) || {}) : { canEdit: false, canDelete: false };
  const _tableId = tableId || entityKey;
  const {
    resolvers = {}, mobileCardMode = false, tableClass = '',
    expandable = false, renderDetail = null,
    collapsible = false, defaultCollapsed = false,
    headerActions = [], storageKey = _tableId,
  } = extra;

  const hasCustom = typeof hooks.customActions === 'function';
  const hasBuiltin = (effectiveUi.canEdit !== false) || (effectiveUi.canDelete !== false);
  const allowBuiltin = !hasCustom || extra.disableBuiltinActions === false;
  const shouldActions = hasCustom || (hasBuiltin && allowBuiltin);

  const finalClass = ['data-table', mobileCardMode ? 'mobile-cards' : '', tableClass].filter(Boolean).join(' ');
  const fieldMap = {};
  (effectiveDef.fields || []).forEach((f) => { fieldMap[f.id] = f; });

  let allCols;
  if (hasCustomCols) allCols = customCols.map((c) => ({ ...c }));
  else {
    const listCols = effectiveUi.listColumns || (effectiveDef.fields || []).map((f) => f.id);
    allCols = listCols.map((id) => {
      const f = fieldMap[id];
      return { id, label: f?.label || id, type: f?.type || 'text', defaultVisible: true };
    });
  }

  if (shouldActions && !allCols.some((c) => c.id === '__actions__')) {
    allCols.push({ id: '__actions__', label: '操作', type: 'actions', defaultVisible: true, defaultWidth: 180 });
  }

  const collapseKey = `${COLLAPSE_PREFIX}${storageKey}`;
  let _collapsed = false;
  if (collapsible) {
    try { const s = localStorage.getItem(collapseKey); _collapsed = s === null ? defaultCollapsed : s === 'true'; } catch (e) {}
  }

  const _expanded = new Set();
  const colSettings = initColumnSettings({ tableId: _tableId, columns: allCols });
  const _originalRows = typeof hooks.beforeRender === 'function' ? (hooks.beforeRender(rows) || rows) : rows;

  /* 🆕 v103.0.34：排序 / 篩選狀態 */
  let _sortState = { columnId: null, direction: null }; // null | 'asc' | 'desc'
  let _filterState = {}; // { columnId: value }

  /* 依 type 判斷是否可排序 */
  const _isSortable = (col) => {
    if (col.id === '__actions__' || col.type === 'actions') return false;
    if (col.sortable !== undefined) return !!col.sortable;
    return ['text', 'number', 'date', 'select'].includes(col.type || 'text');
  };

  /* 依 filterOptions 判斷是否可篩選 */
  const _isFilterable = (col) => {
    if (col.id === '__actions__' || col.type === 'actions') return false;
    if (col.filterable === true && Array.isArray(col.filterOptions)) return true;
    return false;
  };

  /* 計算篩選後 + 排序後的 rows */
  const _computeDisplayRows = () => {
    let result = [...(_originalRows || [])];

    /* 篩選 */
    Object.entries(_filterState).forEach(([colId, filterVal]) => {
      if (filterVal === '' || filterVal == null) return;
      const col = allCols.find((c) => c.id === colId);
      if (!col) return;

      if (typeof col.filterFn === 'function') {
        result = result.filter((row) => col.filterFn(row, filterVal));
      } else {
        result = result.filter((row) => String(row[colId] ?? '') === String(filterVal));
      }
    });

    /* 排序 */
    if (_sortState.columnId && _sortState.direction) {
      const col = allCols.find((c) => c.id === _sortState.columnId);
      if (col) {
        const dir = _sortState.direction === 'asc' ? 1 : -1;
        const type = col.type || 'text';
        result.sort((a, b) => {
          const va = a[_sortState.columnId];
          const vb = b[_sortState.columnId];
          if (type === 'number') {
            return ((Number(va) || 0) - (Number(vb) || 0)) * dir;
          }
          return String(va ?? '').localeCompare(String(vb ?? ''), 'zh-HK') * dir;
        });
      }
    }

    return result;
  };

  let _rows = _computeDisplayRows();

  _paint();

  function _paint() {
    _rows = _computeDisplayRows();
    const visible = colSettings.getVisibleColumns();
    if (!_rows || _rows.length === 0) {
      _render(root, `<div class="glass-card"><div class="empty-state">尚無資料</div></div>`);
      _bind();
      return;
    }
    const total = visible.length + (expandable ? 1 : 0);
    const left = collapsible
      ? `<button type="button" class="btn btn-sm btn-ghost" data-action="toggle-collapse"><i data-lucide="${_collapsed ? 'chevron-right' : 'chevron-down'}" style="width:14px;height:14px;"></i><span>共 ${_rows.length} 筆</span></button>`
      : `<span class="text-muted" style="font-size:12px;">共 ${_rows.length} 筆</span>`;
    const headerActionsHtml = headerActions.map((a) => `<button type="button" class="btn btn-sm ${escapeHtml(a.className || 'btn-ghost')}" data-action="${escapeHtml(a.action)}">${a.icon ? `<i data-lucide="${escapeHtml(a.icon)}" style="width:14px;height:14px;"></i>` : ''}<span>${escapeHtml(a.label)}</span></button>`).join('');
    const head = `<thead><tr>${expandable ? '<th class="expand-col" style="width:36px;"></th>' : ''}${visible.map((c) => _renderTh(c, colSettings)).join('')}</tr></thead>`;
    const body = _rows.map((row, i) => _rowHtml(row, i, visible, total)).join('');
    const html = `<div class="data-table-header"><div class="data-table-header-left">${left}</div><div class="data-table-header-right"><button type="button" class="btn btn-sm btn-ghost" data-action="column-settings" title="欄位設定"><i data-lucide="columns" style="width:14px;height:14px;"></i><span class="hide-mobile">欄位</span></button>${headerActionsHtml}</div></div><div class="data-table-body" style="display:${_collapsed ? 'none' : 'block'};"><div class="glass-card collapsible-card collapsible-card-flat" style="padding:0;"><div class="data-table-scroll-wrapper"><table class="${finalClass}">${head}<tbody>${body}</tbody></table></div></div></div>`;
    _render(root, html);
    _bind();
    if (typeof hooks.afterRender === 'function') hooks.afterRender(root);
  }

  /* 🆕 渲染表頭（含排序 / 篩選） */
  function _renderTh(col, colSettings) {
    const w = colSettings.getWidth(col.id);
    const sortable = _isSortable(col);
    const filterable = _isFilterable(col);
    const isSorted = _sortState.columnId === col.id;
    const sortIcon = isSorted
      ? (_sortState.direction === 'asc' ? 'chevron-up' : 'chevron-down')
      : 'chevrons-up-down';
    const isFiltered = _filterState[col.id] != null && _filterState[col.id] !== '';

    const sortHtml = sortable
      ? `<button type="button" class="dt-sort-btn ${isSorted ? 'active' : ''}" data-dt-sort="${escapeHtml(col.id)}" title="點擊排序"><i data-lucide="${sortIcon}" style="width:11px;height:11px;"></i></button>`
      : '';

    const filterHtml = filterable
      ? `<button type="button" class="dt-filter-btn ${isFiltered ? 'active' : ''}" data-dt-filter="${escapeHtml(col.id)}" title="篩選"><i data-lucide="filter" style="width:11px;height:11px;"></i></button>`
      : '';

    return `<th class="${col.type === 'number' ? 'num' : ''} ${sortable ? 'dt-th-sortable' : ''}" ${w ? `style="width:${w}px;"` : ''} data-dt-th="${escapeHtml(col.id)}">
      <span class="dt-th-label">${escapeHtml(col.label)}</span>
      ${sortHtml}${filterHtml}
    </th>`;
  }

  function _rowHtml(row, i, visible, total) {
    const main = `<tr data-row-index="${i}">${expandable ? `<td class="expand-col"><button type="button" class="btn btn-sm btn-ghost" data-action="toggle-expand" data-index="${i}" style="padding:2px 6px;"><i data-lucide="${_expanded.has(i) ? 'chevron-down' : 'chevron-right'}" style="width:14px;height:14px;"></i></button></td>` : ''}${visible.map((c, ci) => _cell(c, row, ci === 0)).join('')}</tr>`;
    if (!expandable || typeof renderDetail !== 'function') return main;
    return main + `<tr class="detail-row" data-detail-index="${i}" style="display:${_expanded.has(i) ? 'table-row' : 'none'};"><td colspan="${total}" class="detail-cell">${renderDetail(row)}</td></tr>`;
  }

  function _cell(col, row, isFirst) {
    if (col.id === '__actions__') return `<td data-label="操作">${_actions(col, row)}</td>`;
    if (typeof hooks.customCellRender === 'function') {
      const c = hooks.customCellRender(col, row);
      if (c !== null && c !== undefined) return `<td data-label="${escapeHtml(col.label)}" ${isFirst ? 'data-primary="1"' : ''}>${c}</td>`;
    }
    const val = row[col.id];
    const r = resolvers[col.id];
    const content = r ? r(val, row) : formatCellValue(val, col.type);
    return `<td class="${col.type === 'number' ? 'num' : ''}" ${isFirst ? 'data-primary="1"' : `data-label="${escapeHtml(col.label)}"`}>${content}</td>`;
  }

  function _actions(col, row) {
    let h = '';
    if (hasCustom) {
      h += (hooks.customActions(row) || []).map((a) => `<button type="button" class="btn btn-sm ${escapeHtml(a.className || 'btn-ghost')}" data-action="${escapeHtml(a.action || 'custom')}">${a.icon ? `<i data-lucide="${escapeHtml(a.icon)}" style="width:14px;height:14px;"></i>` : ''}${escapeHtml(a.label)}</button>`).join('');
    }
    if (allowBuiltin) {
      if (effectiveUi.canEdit !== false) h += `<button type="button" class="btn btn-sm btn-ghost" data-action="edit"><i data-lucide="pencil" style="width:14px;height:14px;"></i> 編輯</button>`;
      if (effectiveUi.canDelete !== false) h += `<button type="button" class="btn btn-sm btn-danger" data-action="delete"><i data-lucide="trash-2" style="width:14px;height:14px;"></i> 刪除</button>`;
    }
    return h;
  }

  /* 🆕 篩選面板 */
  function _openFilterPanel(col) {
    const panelId = `dt-filter-panel-${_tableId}`;
    document.getElementById(panelId)?.remove();

    const th = root.querySelector(`[data-dt-th="${col.id}"]`);
    if (!th) return;
    const rect = th.getBoundingClientRect();

    const curVal = _filterState[col.id] ?? '';
    const options = col.filterOptions || [];

    const overlay = document.createElement('div');
    overlay.className = 'dt-filter-overlay';
    overlay.id = panelId;
    overlay.style.position = 'fixed';
    overlay.style.top = `${rect.bottom + 4}px`;
    overlay.style.left = `${rect.left}px`;
    overlay.style.zIndex = '2000';

    overlay.innerHTML = `
      <div class="dt-filter-panel">
        <div class="dt-filter-panel-title">${escapeHtml(col.label)}</div>
        <select class="select" data-dt-filter-select style="width:100%; padding:6px 10px; font-size:13px;">
          <option value="">— 全部 —</option>
          ${options.map((o) => `<option value="${escapeHtml(o.value)}" ${String(o.value) === String(curVal) ? 'selected' : ''}>${escapeHtml(o.label)}</option>`).join('')}
        </select>
        <div class="dt-filter-panel-actions">
          <button type="button" class="btn btn-sm btn-ghost" data-dt-filter-clear>清除</button>
          <button type="button" class="btn btn-sm btn-primary" data-dt-filter-apply>套用</button>
        </div>
      </div>
    `;
    document.body.appendChild(overlay);

    overlay.addEventListener('click', (e) => {
      if (e.target === overlay) { overlay.remove(); return; }
      if (e.target.closest('[data-dt-filter-clear]')) {
        delete _filterState[col.id];
        overlay.remove();
        _paint();
        return;
      }
      if (e.target.closest('[data-dt-filter-apply]')) {
        const sel = overlay.querySelector('[data-dt-filter-select]');
        if (sel.value === '') {
          delete _filterState[col.id];
        } else {
          _filterState[col.id] = sel.value;
        }
        overlay.remove();
        _paint();
        return;
      }
    });

    /* 點擊外部關閉 */
    const outsideHandler = (e) => {
      if (!overlay.contains(e.target) && !e.target.closest('[data-dt-filter]')) {
        overlay.remove();
        document.removeEventListener('mousedown', outsideHandler);
      }
    };
    setTimeout(() => document.addEventListener('mousedown', outsideHandler), 0);
  }

  function _bind() {
    const old = root[LISTENER_KEY];
    if (old) root.removeEventListener('click', old);
    const listener = (e) => {
      /* 排序按鈕 */
      const sortBtn = e.target.closest('[data-dt-sort]');
      if (sortBtn) {
        e.stopPropagation();
        const colId = sortBtn.dataset.dtSort;
        if (_sortState.columnId !== colId) {
          _sortState = { columnId: colId, direction: 'asc' };
        } else if (_sortState.direction === 'asc') {
          _sortState = { columnId: colId, direction: 'desc' };
        } else if (_sortState.direction === 'desc') {
          _sortState = { columnId: null, direction: null };
        } else {
          _sortState = { columnId: colId, direction: 'asc' };
        }
        _paint();
        return;
      }

      /* 篩選按鈕 */
      const filterBtn = e.target.closest('[data-dt-filter]');
      if (filterBtn) {
        e.stopPropagation();
        const colId = filterBtn.dataset.dtFilter;
        const col = allCols.find((c) => c.id === colId);
        if (col) _openFilterPanel(col);
        return;
      }

      const colBtn = e.target.closest('button[data-action="toggle-collapse"]');
      if (colBtn) { _collapsed = !_collapsed; try { localStorage.setItem(collapseKey, String(_collapsed)); } catch (err) {} _paint(); return; }
      const expBtn = e.target.closest('button[data-action="toggle-expand"]');
      if (expBtn) { const i = Number(expBtn.dataset.index); _expanded.has(i) ? _expanded.delete(i) : _expanded.add(i); _paint(); return; }
      const hBtn = e.target.closest('.data-table-header-right button[data-action]');
      if (hBtn) {
        const a = hBtn.dataset.action;
        if (a === 'column-settings') { colSettings.openPanel({ onChange: _paint }); return; }
        const m = headerActions.find((x) => x.action === a);
        if (m?.onClick) { e.stopPropagation(); m.onClick(); return; }
        return;
      }
      const actBtn = e.target.closest('tbody button[data-action]');
      if (actBtn) {
        const a = actBtn.dataset.action;
        const tr = actBtn.closest('tr[data-row-index]');
        if (!tr) return;
        const row = _rows[Number(tr.dataset.rowIndex)];
        if (!row) return;
        if (hasCustom) {
          const m = (hooks.customActions(row) || []).find((x) => (x.action || 'custom') === a);
          if (m?.onClick) { e.stopPropagation(); m.onClick(row); return; }
        }
        if (a === 'edit' && hooks.onEdit) { e.stopPropagation(); hooks.onEdit(row); return; }
        if (a === 'delete' && hooks.onDelete) { e.stopPropagation(); hooks.onDelete(row); return; }
        return;
      }
      const tr = e.target.closest('tr[data-row-index]');
      if (tr && hooks.onRowClick) hooks.onRowClick(_rows[Number(tr.dataset.rowIndex)]);
    };
    root.addEventListener('click', listener);
    root[LISTENER_KEY] = listener;
  }

  return {
    container: root,
    table: root.querySelector('table'),
    refresh: _paint,
    destroy: () => {
      const old = root[LISTENER_KEY];
      if (old) root.removeEventListener('click', old);
      root.innerHTML = '';
      root.__lastHtml = '';
    },
  };
}

function _resolveEl(t) {
  if (typeof t === 'string') return document.getElementById(t);
  if (t instanceof HTMLElement) return t;
  return null;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/shared/data-table.js
   Version: v103.0.34
   Batch: B29-1c
   ═══════════════════════════════════════════ */```

---

## 📄 js/shared/date-helpers.js

```
// ============================================
// date-helpers.js — 全站共用年月下拉填充（v101）
// 位置：js/shared/date-helpers.js
// ============================================
// v101 修正：
//   ✅ 年份範圍改讀 app-config（settings/year_range）
//   ✅ 移除硬編碼 range = 5
//   ✅ 新增 useAppState 選項（自動從 AppState 讀預設值）
// ============================================

import { AppState } from '../core/state.js';
import { getYearList, getYearRange } from '../config/app-config.js';

/**
 * 填充年份下拉（範圍由 app-config 提供）
 *
 * @param {string|HTMLElement} target - 元素 ID 或元素本身
 * @param {Object} [config]
 * @param {string} [config.defaultValue] - 預設選中值
 * @param {boolean} [config.preserveValue=true] - 是否保留原選中值
 * @param {boolean} [config.includeAll=false] - 是否加入「全部」選項
 * @param {boolean} [config.useAppState=false] - 是否從 AppState 讀預設
 */
export function fillYearSelect(target, config = {}) {
  const {
    defaultValue,
    preserveValue = true,
    includeAll = false,
    useAppState = false,
  } = config;

  const sel = _resolveElement(target);
  if (!sel) {
    console.warn('⚠️ fillYearSelect: 找不到元素', target);
    return;
  }

  const cur = preserveValue ? sel.value : '';
  const years = getYearList();

  let html = includeAll ? `<option value="">全部</option>` : '';
  years.forEach((y) => {
    html += `<option value="${y}">${y} 年</option>`;
  });

  sel.innerHTML = html;

  // 優先順序：defaultValue > useAppState > preserveValue > 最後一年
  let targetVal = null;
  if (defaultValue != null) {
    targetVal = String(defaultValue);
  } else if (useAppState) {
    targetVal = String(AppState.year);
  } else if (preserveValue && cur && sel.querySelector(`option[value="${cur}"]`)) {
    targetVal = cur;
  }

  if (targetVal && sel.querySelector(`option[value="${targetVal}"]`)) {
    sel.value = targetVal;
  } else {
    // fallback：當前年（若在清單內）或最後一年
    const curY = String(new Date().getFullYear());
    if (sel.querySelector(`option[value="${curY}"]`)) {
      sel.value = curY;
    } else if (years.length > 0) {
      sel.value = String(years[years.length - 1]);
    }
  }
}

/**
 * 填充月份下拉
 *
 * @param {string|HTMLElement} target
 * @param {Object} [config]
 * @param {boolean} [config.includeAll=false] - 是否加入「全部」選項
 * @param {string} [config.allText='全部']
 * @param {string} [config.defaultValue] - 預設選中值（'01' ~ '12' 或 'all'）
 * @param {boolean} [config.preserveValue=true]
 * @param {boolean} [config.useAppState=false] - 從 AppState 讀（'all' → '01'）
 */
export function fillMonthSelect(target, config = {}) {
  const {
    includeAll = false,
    allText = '全部',
    defaultValue,
    preserveValue = true,
    useAppState = false,
  } = config;

  const sel = _resolveElement(target);
  if (!sel) {
    console.warn('⚠️ fillMonthSelect: 找不到元素', target);
    return;
  }

  const cur = preserveValue ? sel.value : '';

  let html = includeAll ? `<option value="all">${allText}</option>` : '';
  for (let m = 1; m <= 12; m++) {
    const mm = String(m).padStart(2, '0');
    html += `<option value="${mm}">${m} 月</option>`;
  }

  sel.innerHTML = html;

  let targetVal = null;
  if (defaultValue != null) {
    targetVal = String(defaultValue);
  } else if (useAppState) {
    // AppState.month === 'all' → '01'
    targetVal = AppState.month === 'all' ? '01' : String(AppState.month);
  } else if (preserveValue && cur && sel.querySelector(`option[value="${cur}"]`)) {
    targetVal = cur;
  }

  if (targetVal && sel.querySelector(`option[value="${targetVal}"]`)) {
    sel.value = targetVal;
  }
}

/**
 * 一次填充年份 + 月份
 *
 * @param {string} yearTarget
 * @param {string} monthTarget
 * @param {Object} [config]
 */
export function fillYearMonthSelects(yearTarget, monthTarget, config = {}) {
  fillYearSelect(yearTarget, config);
  fillMonthSelect(monthTarget, config);
}

/* ============================================
   資訊查詢
   ============================================ */

/**
 * 取得可用年份清單
 */
export function getAvailableYears() {
  return getYearList();
}

/**
 * 取得年份範圍
 * @returns {{startYear: number, endYear: number}}
 */
export function getYearRangeInfo() {
  return getYearRange();
}

/* ============================================
   內部工具
   ============================================ */
function _resolveElement(target) {
  if (typeof target === 'string') return document.getElementById(target);
  if (target instanceof HTMLElement) return target;
  return null;
}```

---

## 📄 js/shared/entity-list-page.js

```
// ============================================
// entity-list-page.js — 實體列表頁骨架（v103.0.16）
// 位置：js/shared/entity-list-page.js
// ============================================
// v103.0.16 修正：
//   ✅ 傳入 entity 的 resolvers 給 renderDataTable
// ============================================

import { getEntityDef, getEntityUi } from '../entity/entity-definitions.js';
import { getEntityResolvers } from '../entity/entity-resolvers.js';
import { esc as escapeHtml } from '../lib/dom.js';
import { AppState } from '../core/state.js';
import { renderDataTable } from './data-table.js';
import { renderDataCard } from './data-card.js';
import { openEntityModal } from '../entity/entity-modal.js';
import { openConfirm } from '../ui/modal.js';
import { createListenerGroup } from './listener-group.js';
import { handleSubmit } from './form-handler.js';
import { deleteEntity, listenEntity } from '../entity/entity-helpers.js';
import { initViewToggle } from '../ui/view-toggle.js';

export function initEntityListPage(options) {
  const { entity, containerId, hooks = {}, filters = null, options: extraOptions = {} } = options;
  const root = document.getElementById(containerId);
  if (!root) { console.warn(`⚠️ initEntityListPage: 找不到容器 #${containerId}`); return null; }

  const def = getEntityDef(entity);
  if (!def) { console.warn(`⚠️ initEntityListPage: 找不到實體 ${entity}`); return null; }

  const ui = getEntityUi(entity) || {};
  const {
    canCreate: rawCanCreate = true, canEdit: rawCanEdit = true,
    canDelete: rawCanDelete = true, deleteConfirmText,
  } = ui;

  const userCanInput = AppState.getCanInput();
  const canCreate = rawCanCreate && userCanInput;
  const canEdit = rawCanEdit && userCanInput;
  const canDelete = rawCanDelete && userCanInput;

  const {
    defaultView = 'table', showViewToggle = false, showHeader = true,
    storageKey = null, mobileCardMode = false,
  } = extraOptions;

  let _rows = [], _viewToggle = null, _currentView = defaultView;
  let _tableApi = null, _cardApi = null;
  const listenerGroup = createListenerGroup();

  _renderSkeleton();
  _startListening();

  return { refresh: _refresh, getRows: () => _rows, setView: _setView, destroy: _destroy };

  function _renderSkeleton() {
    const headerHtml = showHeader ? `
      <div class="entity-list-header flex flex-between items-center flex-wrap gap-12 mb-16">
        <div class="text-muted" style="font-size:13px;">${escapeHtml(def.label)}管理${!userCanInput ? '（唯讀模式）' : ''}</div>
        <div class="flex items-center gap-8 flex-wrap">
          ${showViewToggle ? `<div id="${containerId}-view-toggle"></div>` : ''}
          ${canCreate ? `<button type="button" class="btn btn-primary" id="${containerId}-add-btn"><i data-lucide="plus"></i> 新增${escapeHtml(def.label)}</button>` : ''}
        </div>
      </div>` : '';

    root.innerHTML = `${headerHtml}<div id="${containerId}-content"></div>`;

    if (showViewToggle) {
      _viewToggle = initViewToggle({
        containerId: `${containerId}-view-toggle`,
        storageKey: storageKey || `${entity}-view`,
        defaultView, cardText: '卡片', tableText: '表格', autoApply: false,
        onChange: (view) => { _currentView = view; _renderContent(); },
      });
      _currentView = _viewToggle.getView();
    }

    if (canCreate) {
      document.getElementById(`${containerId}-add-btn`)?.addEventListener('click', _handleAdd);
    }
    if (window.lucide) window.lucide.createIcons();
  }

  function _startListening() {
    listenerGroup.add(listenEntity(entity, (rows) => { _rows = rows || []; _renderContent(); }));
  }

  function _renderContent() {
    const contentEl = document.getElementById(`${containerId}-content`);
    if (!contentEl) return;
    if (_tableApi) { try { _tableApi.destroy(); } catch (e) {} _tableApi = null; }
    if (_cardApi) { try { _cardApi.destroy(); } catch (e) {} _cardApi = null; }

    const resolvers = getEntityResolvers(entity);

    if (_currentView === 'card') {
      _cardApi = renderDataCard({
        container: contentEl, entityKey: entity, rows: _rows,
        options: { ...extraOptions, resolvers },
        hooks: { ...hooks, onEdit: canEdit ? _handleEdit : undefined, onDelete: canDelete ? _handleDelete : undefined },
      });
    } else {
      _tableApi = renderDataTable({
        container: contentEl, entityKey: entity, rows: _rows,
        tableId: `${entity}-table`,
        options: { ...extraOptions, mobileCardMode, resolvers },
        hooks: { ...hooks, onEdit: canEdit ? _handleEdit : undefined, onDelete: canDelete ? _handleDelete : undefined },
      });
    }
  }

  function _handleAdd() {
    if (!canCreate) return;
    openEntityModal({ entity, mode: 'add', allRows: _rows });
  }

  function _handleEdit(row) {
    if (!canEdit) return;
    openEntityModal({ entity, mode: 'edit', id: row.id, allRows: _rows });
  }

  async function _handleDelete(row) {
    if (!canDelete) return;
    const confirmText = typeof deleteConfirmText === 'function'
      ? deleteConfirmText(row) : `確定要刪除「${row.name || row.id}」嗎？`;
    const ok = await openConfirm(confirmText, { title: `刪除${def.label}`, okText: '刪除', okClass: 'btn-danger' });
    if (!ok) return;

    let extraArgs = [];
    if (entity === 'policy') extraArgs = [row.policyHolderId || row.memberId];
    if (typeof hooks.getDeleteArgs === 'function') extraArgs = hooks.getDeleteArgs(row) || [];

    await handleSubmit({
      action: () => deleteEntity(entity, row.id, ...extraArgs),
      successMessage: `✅ 已刪除${def.label}`,
      onSuccess: () => { if (typeof hooks.afterDelete === 'function') hooks.afterDelete(row); },
    });
  }

  function _refresh() { _renderContent(); }
  function _setView(view) { _currentView = view; if (_viewToggle) _viewToggle.setView(view); _renderContent(); }

  function _destroy() {
    listenerGroup.destroy();
    if (_tableApi) { try { _tableApi.destroy(); } catch (e) {} }
    if (_cardApi) { try { _cardApi.destroy(); } catch (e) {} }
    if (_viewToggle) { try { _viewToggle.destroy(); } catch (e) {} }
    root.innerHTML = '';
  }
}

export function initEntityListPages(configs = []) {
  return configs.map((cfg) => initEntityListPage(cfg));
}```

---

## 📄 js/shared/form-handler.js

```
// ============================================
// form-handler.js — 提交封裝（v103.0.11）
// 位置：js/shared/form-handler.js
// ============================================
// v103.0.11 重構：
//   ✅ [H07] 標記 @deprecated，內部實作改走 lib/async.js 的 withToast
//   ✅ 對外 API 保持 100% 相容（handleSubmit / submitAndToast / submitSilent）
//   ✅ 未來版本將完全移除，呼叫端請改用 lib/async.js
// ============================================

import { withToast } from '../lib/async.js';

/**
 * @deprecated v103.0.11 — 請改用 `lib/async.js` 的 `withToast`
 *
 * 提交封裝
 * @param {Object} options
 * @param {Function} options.action - 執行函式（async）
 * @param {string} [options.successMessage='✅ 操作成功'] - 成功訊息
 * @param {string} [options.errorMessage] - 錯誤訊息前綴（若未提供，用 err.message）
 * @param {Function} [options.onSuccess] - 成功回呼 (result) => {}
 * @param {Function} [options.onError] - 失敗回呼 (err) => {}
 * @param {boolean} [options.silent=false] - 是否靜默（不顯示 Toast）
 * @returns {Promise<{ success, result?, error? }>}
 */
export async function handleSubmit(options) {
  const {
    action,
    successMessage = '✅ 操作成功',
    errorMessage,
    onSuccess,
    onError,
    silent = false,
  } = options;

  if (typeof action !== 'function') {
    console.warn('[form-handler] action 必須是函式');
    return { success: false, error: new Error('action 必須是函式') };
  }

  const { ok, result, error } = await withToast(
    silent ? null : successMessage,
    action,
    {
      silent,
      errorMsg: errorMessage,
    }
  );

  if (ok) {
    if (typeof onSuccess === 'function') {
      try { onSuccess(result); } catch (e) {
        console.error('[form-handler] onSuccess 失敗：', e);
      }
    }
    return { success: true, result };
  }

  if (typeof onError === 'function') {
    try { onError(error); } catch (e) {
      console.error('[form-handler] onError 失敗：', e);
    }
  }
  return { success: false, error };
}

/**
 * @deprecated v103.0.11 — 請改用 `withToast('✅ 已儲存', fn)`
 *
 * 提交並顯示成功訊息（簡化版）
 */
export function submitAndToast(action, message = '✅ 已儲存') {
  return handleSubmit({ action, successMessage: message });
}

/**
 * @deprecated v103.0.11 — 請改用 `withToast(null, fn, { silent: true })`
 *
 * 提交並靜默（不顯示 Toast）
 */
export function submitSilent(action) {
  return handleSubmit({ action, silent: true });
}```

---

## 📄 js/shared/listener-group.js

```
// ============================================
// listener-group.js — 訂閱管理（v103.0.11）
// 位置：js/shared/listener-group.js
// ============================================
// v103.0.11 重構：
//   ✅ [H06] 標記 @deprecated，內部實作改走 lib/lifecycle.js
//   ✅ 對外 API 保持 100% 相容（add / addAll / remove / count / destroy）
//   ✅ 未來版本將完全移除，呼叫端請改用 lib/lifecycle.js
// ============================================

import { createCleanupRegistry } from '../lib/lifecycle.js';

/* ============================================
   主函式（相容層）
   ============================================ */

/**
 * @deprecated v103.0.11 — 請改用 `lib/lifecycle.js` 的 `createCleanupRegistry`
 *
 * 建立訂閱群組
 * @returns {Object} { add, addAll, remove, count, destroy }
 */
export function createListenerGroup() {
  const registry = createCleanupRegistry();
  let _destroyed = false;

  return {
    /**
     * 新增一個訂閱
     * @param {Function} unsubscribe - onValue 回傳的取消函式
     * @returns {Function} 該訂閱的取消函式
     */
    add(unsubscribe) {
      if (_destroyed) {
        console.warn('[listener-group] 群組已銷毀，忽略新增訂閱');
        return () => {};
      }
      registry.add(unsubscribe);
      return unsubscribe;
    },

    /**
     * 批次新增訂閱
     * @param {Function[]} unsubscribeList
     */
    addAll(unsubscribeList = []) {
      if (_destroyed) return;
      registry.addAll(unsubscribeList);
    },

    /**
     * 移除指定訂閱（不執行，只移除）
     * @param {Function} unsubscribe
     */
    remove(unsubscribe) {
      registry.remove(unsubscribe);
    },

    /**
     * 目前訂閱數量
     */
    count() {
      return registry.size();
    },

    /**
     * 銷毀所有訂閱（執行每個 unsubscribe）
     */
    destroy() {
      if (_destroyed) return;
      _destroyed = true;
      registry.run();
    },
  };
}

/* ============================================
   便利函式
   ============================================ */

/**
 * @deprecated v103.0.11 — 請改用 `createCleanupRegistry()` + 手動包裝 onDestroy
 *
 * 建立「自動銷毀」的訂閱群組（用於頁面）
 * @param {Function} onDestroy - 額外清理回呼（選填）
 * @returns {Object}
 */
export function createPageListenerGroup(onDestroy) {
  const group = createListenerGroup();
  const originalDestroy = group.destroy;

  group.destroy = () => {
    originalDestroy();
    if (typeof onDestroy === 'function') {
      try { onDestroy(); } catch (e) {
        console.warn('[listener-group] onDestroy 失敗：', e);
      }
    }
  };

  return group;
}```

---

## 📄 js/shared/page-filter.js

```
// ============================================
// page-filter.js — 全站共用的頁面篩選欄（v103.0.11）
// 位置：js/shared/page-filter.js
// ============================================
// v103.0.11 修正：
//   ✅ [H04] 移除未使用的 escapeHtml import
// ============================================

import { AppState } from '../core/state.js';
import { getYearList } from '../config/app-config.js';

/**
 * 渲染頁面篩選欄
 * @param {Object} options
 * @param {string} options.containerId - 容器 ID（預設 'page-filter-root'）
 * @param {string[]} options.fields - ['year', 'month']
 * @param {Function} options.onChange - 篩選變更回呼 (filters) => {}
 * @param {Function} options.renderExtra - 額外欄位 HTML 產生器
 * @param {boolean} options.includeAllMonth - 月份是否含「全年」（預設 true）
 * @param {boolean} options.autoSyncAppState - 是否自動同步 AppState（預設 true）
 * @returns {Object|null} { root, getFilters, refresh, updateExtra, destroy }
 */
export function renderPageFilter(options = {}) {
  const {
    containerId = 'page-filter-root',
    fields = ['year', 'month'],
    onChange,
    renderExtra,
    includeAllMonth = true,
    autoSyncAppState = true,
  } = options;

  const root = document.getElementById(containerId);
  if (!root) {
    console.warn(`⚠️ renderPageFilter: 找不到容器 #${containerId}`);
    return null;
  }

  const { year: stateYear, month: stateMonth } = AppState.getYearMonth();

  const parts = [];

  if (fields.includes('year')) {
    const years = getYearList();
    let opts = '';
    years.forEach((y) => {
      const sel = String(y) === String(stateYear) ? 'selected' : '';
      opts += `<option value="${y}" ${sel}>${y} 年</option>`;
    });
    parts.push(`
      <div class="filter-group">
        <label class="field-label">年份</label>
        <select class="select" data-filter="year">${opts}</select>
      </div>
    `);
  }

  if (fields.includes('month')) {
    let opts = includeAllMonth ? `<option value="all">全部</option>` : '';
    for (let m = 1; m <= 12; m++) {
      const mm = String(m).padStart(2, '0');
      const sel = mm === String(stateMonth) ? 'selected' : '';
      opts += `<option value="${mm}" ${sel}>${m} 月</option>`;
    }
    parts.push(`
      <div class="filter-group">
        <label class="field-label">月份</label>
        <select class="select" data-filter="month">${opts}</select>
      </div>
    `);
  }

  const extraHtml = _renderExtraInto(renderExtra);

  root.innerHTML = `<div class="page-filter-bar">${parts.join('')}${extraHtml}</div>`;

  /* ============================================
     事件綁定
     ============================================ */
  const changeHandler = () => {
    const filters = collectFilters(root);
    if (autoSyncAppState && filters.year && filters.month) {
      AppState.setYearMonth(filters.year, filters.month);
    }
    if (typeof onChange === 'function') onChange(filters);
  };

  const bindSelects = () => {
    root.querySelectorAll('select[data-filter]').forEach((sel) => {
      sel.removeEventListener('change', changeHandler);
      sel.addEventListener('change', changeHandler);
    });
  };

  bindSelects();

  let unsubscribeYMEvent = null;
  if (autoSyncAppState) {
    unsubscribeYMEvent = AppState.on('ym-change', ({ year, month }) => {
      const ySel = root.querySelector('select[data-filter="year"]');
      const mSel = root.querySelector('select[data-filter="month"]');
      if (ySel && ySel.value !== String(year)) ySel.value = String(year);
      if (mSel && mSel.value !== String(month)) mSel.value = String(month);
    });
  }

  if (typeof onChange === 'function') {
    onChange(collectFilters(root));
  }

  if (window.lucide) window.lucide.createIcons();

  return {
    root,
    getFilters: () => collectFilters(root),

    refresh: () => {
      const filters = collectFilters(root);
      if (typeof onChange === 'function') onChange(filters);
    },

    updateExtra: (newRenderExtra) => {
      const bar = root.querySelector('.page-filter-bar');
      if (!bar) return;

      bar.querySelectorAll('[data-extra-group]').forEach((el) => el.remove());

      const newHtml = _renderExtraInto(newRenderExtra);
      if (newHtml) {
        const temp = document.createElement('div');
        temp.innerHTML = newHtml;
        const children = [...temp.children];
        children.forEach((el) => {
          el.setAttribute('data-extra-group', '1');
          bar.appendChild(el);
        });
      }

      bindSelects();

      if (window.lucide) window.lucide.createIcons();
    },

    destroy: () => {
      root.querySelectorAll('select[data-filter]').forEach((sel) => {
        sel.removeEventListener('change', changeHandler);
      });
      if (unsubscribeYMEvent) {
        try { unsubscribeYMEvent(); } catch (e) { /* noop */ }
      }
    },
  };
}

/* ============================================
   內部工具
   ============================================ */
function _renderExtraInto(renderExtra) {
  if (typeof renderExtra !== 'function') return '';

  const extra = renderExtra();
  if (!extra) return '';

  const wrapper = document.createElement('div');
  wrapper.setAttribute('data-extra-group', '1');
  wrapper.style.display = 'contents';

  wrapper.innerHTML = extra;
  return wrapper.outerHTML;
}

function collectFilters(root) {
  const filters = {};
  root.querySelectorAll('select[data-filter]').forEach((sel) => {
    filters[sel.dataset.filter] = sel.value;
  });
  return filters;
}

export function refreshPageFilter(instance) {
  if (instance && typeof instance.refresh === 'function') instance.refresh();
}```

---

## 📄 js/shared/quick-summary.js

```
// ============================================
// quick-summary.js — 快速摘要卡（v103.0.12）
// 位置：js/shared/quick-summary.js
// ============================================
// v103.0.12 修正：
//   ✅ [L03] _renderAssetPie → _renderAssetBar（實際為條狀圖，命名更精確）
//   ✅ [H04] escapeHtml / formatHKD 從 lib/ 導入
// ============================================

import { esc as escapeHtml } from '../lib/dom.js';
import { formatHKD } from '../lib/format.js';
import { QUICK_SUMMARY_TYPES } from '../config/constants.js';

/* ============================================
   主函式
   ============================================ */
export function renderQuickSummary(options) {
  const {
    containerId,
    type,
    title,
    icon = 'activity',
    data,
    onClick,
  } = options;

  const root = document.getElementById(containerId);
  if (!root) {
    console.warn(`⚠️ renderQuickSummary: 找不到容器 #${containerId}`);
    return null;
  }

  let titleText = title;
  let bodyHtml = '';

  switch (type) {
    case QUICK_SUMMARY_TYPES.TOP_CATEGORIES:
      titleText = titleText || '熱門類別';
      bodyHtml = _renderTopCategories(data);
      break;
    case QUICK_SUMMARY_TYPES.RECENT_ACTIVITY:
      titleText = titleText || '最近活動';
      bodyHtml = _renderRecentActivity(data);
      break;
    case QUICK_SUMMARY_TYPES.MEMBER_TREND:
      titleText = titleText || '成員趨勢';
      bodyHtml = _renderMemberTrend(data);
      break;
    case QUICK_SUMMARY_TYPES.PENDING_FIXED:
      titleText = titleText || '待處理固定支出';
      bodyHtml = _renderPendingFixed(data);
      break;
    case QUICK_SUMMARY_TYPES.ASSET_PIE:
      titleText = titleText || '資產配置';
      /* 🆕 [L03] 函式名稱改為 _renderAssetBar（實作為水平堆疊條） */
      bodyHtml = _renderAssetBar(data);
      break;
    default:
      titleText = titleText || '摘要';
      bodyHtml = '<div class="text-muted text-sm">無摘要資料</div>';
  }

  root.innerHTML = `
    <div class="quick-summary-card">
      <div class="quick-summary-title">
        <i data-lucide="${icon}" style="width:16px;height:16px;"></i>
        <span>${escapeHtml(titleText)}</span>
      </div>
      <div class="quick-summary-list">
        ${bodyHtml}
      </div>
    </div>
  `;

  if (window.lucide) window.lucide.createIcons();

  if (typeof onClick === 'function') {
    root.querySelectorAll('[data-summary-key]').forEach((el) => {
      el.style.cursor = 'pointer';
      el.addEventListener('click', () => {
        onClick(el.dataset.summaryKey, el);
      });
    });
  }

  return { container: root };
}

export function clearQuickSummary(containerId) {
  const root = document.getElementById(containerId);
  if (root) root.innerHTML = '';
}

/* ============================================
   1. Top 分類
   ============================================ */
function _renderTopCategories(data) {
  const list = (data || []).filter((x) => (Number(x.amount) || 0) > 0);
  if (list.length === 0) {
    return '<div class="text-muted text-sm">本月尚無支出紀錄</div>';
  }

  const max = Math.max(...list.map((x) => Number(x.amount) || 0)) || 1;

  return list.map((x) => {
    const amount = Number(x.amount) || 0;
    const pct = Math.round((amount / max) * 100);
    const key = x.categoryId || x.name;

    return `
      <div class="quick-summary-row" data-summary-key="${escapeHtml(key)}" style="display:block; padding:8px 0;">
        <div style="display:flex; justify-content:space-between; margin-bottom:4px;">
          <span class="label">${escapeHtml(x.name || '（未命名）')}</span>
          <span class="value">${formatHKD(amount)}</span>
        </div>
        <div class="progress" style="height:6px;">
          <div class="progress-bar" style="width:${pct}%;"></div>
        </div>
      </div>
    `;
  }).join('');
}

/* ============================================
   2. 最近活動
   ============================================ */
function _renderRecentActivity(data) {
  const list = (data || []).slice(0, 5);
  if (list.length === 0) {
    return '<div class="text-muted text-sm">尚無活動紀錄</div>';
  }

  return list.map((x, i) => {
    const key = x.key || `act-${i}`;
    const badgeHtml = x.badge
      ? `<span class="badge badge-info" style="margin-left:6px;">${escapeHtml(x.badge)}</span>`
      : '';
    const amountHtml = x.amount != null
      ? `<span class="value">${formatHKD(x.amount)}</span>`
      : '';

    return `
      <div class="quick-summary-row" data-summary-key="${escapeHtml(key)}">
        <div style="display:flex; flex-direction:column; gap:2px; flex:1; min-width:0;">
          <span class="label" style="color:var(--text-primary); font-weight:500;">
            ${escapeHtml(x.title || '')}${badgeHtml}
          </span>
          ${x.subtitle ? `<span class="label" style="font-size:11px;">${escapeHtml(x.subtitle)}</span>` : ''}
        </div>
        ${amountHtml}
      </div>
    `;
  }).join('');
}

/* ============================================
   3. 成員趨勢
   ============================================ */
function _renderMemberTrend(data) {
  if (!data || !data.members || data.members.length === 0) {
    return '<div class="text-muted text-sm">尚無趨勢資料</div>';
  }

  const months = data.months || [];
  const members = data.members;
  const maxVal = Math.max(
    ...members.flatMap((m) => m.values.map((v) => Number(v) || 0)),
    1
  );

  return members.map((m) => {
    const total = m.values.reduce((s, v) => s + (Number(v) || 0), 0);
    const bars = m.values.map((v) => {
      const pct = Math.round(((Number(v) || 0) / maxVal) * 100);
      return `<div style="flex:1; height:20px; background:rgba(0,240,255,0.08); border-radius:3px; position:relative; overflow:hidden;">
        <div style="position:absolute; bottom:0; left:0; right:0; height:${pct}%; background:linear-gradient(180deg, var(--neon-cyan), var(--neon-magenta));"></div>
      </div>`;
    }).join('');

    return `
      <div class="quick-summary-row" style="display:block; padding:8px 0;">
        <div style="display:flex; justify-content:space-between; margin-bottom:6px;">
          <span class="label">${escapeHtml(m.name)}</span>
          <span class="value">${formatHKD(total)}</span>
        </div>
        <div style="display:flex; gap:3px; align-items:flex-end;">${bars}</div>
        <div style="display:flex; justify-content:space-between; font-size:10px; color:var(--text-muted); margin-top:4px;">
          ${months.map((mm) => `<span style="flex:1; text-align:center;">${escapeHtml(mm)}</span>`).join('')}
        </div>
      </div>
    `;
  }).join('');
}

/* ============================================
   4. 待處理固定支出
   ============================================ */
function _renderPendingFixed(data) {
  const list = (data || []).filter((x) => x.status !== '已付款' && x.status !== '不適用');
  if (list.length === 0) {
    return '<div class="text-muted text-sm">✅ 本月固定支出全部已處理</div>';
  }

  const total = list.reduce((s, x) => s + (Number(x.amount) || 0), 0);

  const rows = list.slice(0, 5).map((x) => `
    <div class="quick-summary-row" data-summary-key="${escapeHtml(x.id || x.name)}">
      <span class="label">${escapeHtml(x.name || '')}${x.month ? ` · ${escapeHtml(String(x.month))}月` : ''}</span>
      <span class="value" style="color:var(--neon-orange);">${formatHKD(x.amount)}</span>
    </div>
  `).join('');

  const moreHint = list.length > 5
    ? `<div class="text-muted text-xs" style="text-align:center; margin-top:8px;">還有 ${list.length - 5} 筆待處理…</div>`
    : '';

  return `
    ${rows}
    ${moreHint}
    <div class="quick-summary-row" style="border-top:1px solid rgba(255,255,255,0.06); margin-top:8px; padding-top:8px;">
      <span class="label" style="font-weight:600; color:var(--text-primary);">未付款總計</span>
      <span class="value" style="color:var(--neon-orange); font-weight:700;">${formatHKD(total)}</span>
    </div>
  `;
}

/* ============================================
   5. 資產配置（水平堆疊條）
   -------------------------------------------------
   🆕 v103.0.12 [L03]：原 _renderAssetPie 改名為 _renderAssetBar
   ============================================ */
function _renderAssetBar(data) {
  if (!data || !data.items || data.items.length === 0) {
    return '<div class="text-muted text-sm">尚無資產資料</div>';
  }

  const items = data.items.filter((x) => (Number(x.amount) || 0) > 0);
  if (items.length === 0) {
    return '<div class="text-muted text-sm">尚無資產資料</div>';
  }

  const total = items.reduce((s, x) => s + (Number(x.amount) || 0), 0);
  const defaultColors = [
    'var(--neon-cyan)',
    'var(--neon-magenta)',
    'var(--neon-emerald)',
    'var(--neon-orange)',
    '#818cf8',
  ];

  const bar = items.map((x, i) => {
    const pct = (Number(x.amount) / total) * 100;
    const color = x.color || defaultColors[i % defaultColors.length];
    return `<div style="width:${pct}%; height:100%; background:${color};" title="${escapeHtml(x.name)} ${pct.toFixed(1)}%"></div>`;
  }).join('');

  const legend = items.map((x, i) => {
    const pct = ((Number(x.amount) / total) * 100).toFixed(1);
    const color = x.color || defaultColors[i % defaultColors.length];
    return `
      <div class="quick-summary-row">
        <span class="label" style="display:flex; align-items:center; gap:8px;">
          <span style="display:inline-block; width:10px; height:10px; border-radius:2px; background:${color};"></span>
          ${escapeHtml(x.name)}
          <span style="color:var(--text-muted); font-size:11px;">${pct}%</span>
        </span>
        <span class="value">${formatHKD(x.amount)}</span>
      </div>
    `;
  }).join('');

  return `
    <div style="width:100%; height:12px; border-radius:6px; overflow:hidden; display:flex; margin-bottom:12px; background:rgba(255,255,255,0.04);">
      ${bar}
    </div>
    ${legend}
    <div class="quick-summary-row" style="border-top:1px solid rgba(255,255,255,0.06); margin-top:8px; padding-top:8px;">
      <span class="label" style="font-weight:600; color:var(--text-primary);">總資產</span>
      <span class="value" style="color:var(--neon-emerald); font-weight:700;">${formatHKD(total)}</span>
    </div>
  `;
}

/* ============================================
   便捷：資料輔助產生器
   ============================================ */
export function buildTopCategoriesData(perMember, categories, topN = 5) {
  const catMap = {};
  categories.forEach((c) => { catMap[c.id] = c.name; });

  const totals = {};
  Object.values(perMember || {}).forEach((m) => {
    (m.items || []).forEach((it) => {
      const catId = it.categoryId || '__none__';
      const catName = catMap[catId] || '（未分類）';
      if (!totals[catId]) totals[catId] = { categoryId: catId, name: catName, amount: 0 };
      totals[catId].amount += Number(it.amount) || 0;
    });
  });

  return Object.values(totals)
    .sort((a, b) => b.amount - a.amount)
    .slice(0, topN);
}

export function buildRecentActivityData({ expenses = [], fixedExpenses = [], incomes = [] }, limit = 5) {
  const activities = [];

  expenses.forEach((e) => {
    activities.push({
      key: e.id,
      title: e.name || '支出',
      subtitle: e.date || '',
      amount: -(Number(e.amount) || 0),
      badge: e.isAutoLinked ? '保險' : '',
      _ts: e.createdAt || 0,
    });
  });

  fixedExpenses.forEach((f) => {
    activities.push({
      key: f.id,
      title: f.name || '固定支出',
      subtitle: f.paidDate || '',
      amount: -(Number(f.amount) || 0),
      badge: '固定',
      _ts: f.createdAt || 0,
    });
  });

  Object.entries(incomes || {}).forEach(([memberId, amount]) => {
    activities.push({
      key: `income-${memberId}`,
      title: memberId === 'extra' ? '額外收入' : `收入（${memberId}）`,
      subtitle: '',
      amount: Number(amount) || 0,
      badge: '收入',
      _ts: 0,
    });
  });

  return activities
    .sort((a, b) => b._ts - a._ts)
    .slice(0, limit)
    .map(({ _ts, ...rest }) => rest);
}```

---

## 📄 js/shared/select-helpers.js

```
// ============================================
// select-helpers.js — 全站共用下拉選項填充（v103.0.11）
// 位置：js/shared/select-helpers.js
// ============================================
// v103.0.11 修正：
//   ✅ [H04] escapeHtml 改從 lib/dom.js 導入
//   ✅ sortMembers 保留從 core/utils.js 導入（該函式仍定義於此）
// ============================================

import { esc as escapeHtml } from '../lib/dom.js';
import { sortMembers } from '../core/utils.js';

/* ============================================
   通用填充
   ============================================ */
export function fillSelect(target, options, config = {}) {
  const {
    valueKey = 'id',
    labelKey = 'name',
    includeEmpty = false,
    emptyText = '— 請選擇 —',
    preserveValue = true,
  } = config;

  const sel = _resolveElement(target);
  if (!sel) {
    console.warn('⚠️ fillSelect: 找不到元素', target);
    return;
  }

  const cur = preserveValue ? sel.value : '';

  let html = includeEmpty
    ? `<option value="">${escapeHtml(emptyText)}</option>`
    : '';

  html += (options || []).map((o) => {
    const val = o[valueKey];
    const label = o[labelKey];
    return `<option value="${escapeHtml(val)}">${escapeHtml(label)}</option>`;
  }).join('');

  sel.innerHTML = html;

  if (preserveValue && cur && (options || []).some((o) => String(o[valueKey]) === cur)) {
    sel.value = cur;
  }
}

/* ============================================
   業務專用填充
   ============================================ */
export function fillMemberSelect(target, members, config = {}) {
  const sorted = sortMembers(members);
  fillSelect(target, sorted, {
    valueKey: 'id',
    labelKey: 'name',
    includeEmpty: config.includeEmpty !== false,
    emptyText: config.emptyText || '— 請選擇 —',
    preserveValue: config.preserveValue !== false,
  });
}

export function fillCategorySelect(target, categories, config = {}) {
  const sorted = [...(categories || [])].sort((a, b) => (a.order || 0) - (b.order || 0));
  fillSelect(target, sorted, {
    valueKey: 'id',
    labelKey: 'name',
    includeEmpty: config.includeEmpty !== false,
    emptyText: config.emptyText || '— 請選擇類別 —',
    preserveValue: config.preserveValue !== false,
  });
}

export function fillItemSelect(target, items, categoryId, config = {}) {
  const filtered = categoryId
    ? (items || []).filter((i) => i.categoryId === categoryId)
    : (items || []);

  fillSelect(target, filtered, {
    valueKey: 'id',
    labelKey: 'name',
    includeEmpty: config.includeEmpty !== false,
    emptyText: config.emptyText || (categoryId ? '— 請選擇項目 —' : '— 請先選擇類別 —'),
    preserveValue: config.preserveValue !== false,
  });
}

export function fillPaymentSelect(target, payments, config = {}) {
  const sorted = [...(payments || [])].sort((a, b) => (a.order || 0) - (b.order || 0));
  fillSelect(target, sorted, {
    valueKey: 'id',
    labelKey: 'name',
    includeEmpty: config.includeEmpty !== false,
    emptyText: config.emptyText || '— 請選擇 —',
    preserveValue: config.preserveValue !== false,
  });
}

export function fillCompanySelect(target, companies, config = {}) {
  fillSelect(target, companies || [], {
    valueKey: 'id',
    labelKey: 'name',
    includeEmpty: config.includeEmpty !== false,
    emptyText: config.emptyText || '— 請選擇 —',
    preserveValue: config.preserveValue !== false,
  });
}

export function fillBankSelect(target, banks, config = {}) {
  fillSelect(target, banks || [], {
    valueKey: 'id',
    labelKey: 'name',
    includeEmpty: config.includeEmpty !== false,
    emptyText: config.emptyText || '— 請選擇銀行 —',
    preserveValue: config.preserveValue !== false,
  });
}

export function fillStatusSelect(target, statuses, config = {}) {
  let filtered = statuses || [];
  if (config.category) {
    filtered = filtered.filter((s) => s.category === config.category);
  }
  const sorted = [...filtered].sort((a, b) => (a.order || 0) - (b.order || 0));

  const options = sorted.map((s) => ({ value: s.name, label: s.name }));

  fillSelect(target, options, {
    valueKey: 'value',
    labelKey: 'label',
    includeEmpty: config.includeEmpty !== false,
    emptyText: config.emptyText || '— 請選擇 —',
    preserveValue: config.preserveValue !== false,
  });
}

export function fillYearListSelect(target, years, config = {}) {
  const options = (years || []).map((y) => ({
    value: String(y),
    label: `${y} 年`,
  }));
  fillSelect(target, options, {
    valueKey: 'value',
    labelKey: 'label',
    includeEmpty: config.includeEmpty === true,
    emptyText: config.emptyText || '— 請選擇 —',
    preserveValue: config.preserveValue !== false,
  });
}

/* ============================================
   內部工具
   ============================================ */
function _resolveElement(target) {
  if (typeof target === 'string') return document.getElementById(target);
  if (target instanceof HTMLElement) return target;
  return null;
}```

---

## 📄 js/shared/stats-cards.js

```
// ============================================
// stats-cards.js — 統計卡片渲染（v103.0.11）
// 位置：js/shared/stats-cards.js
// ============================================
// v103.0.11 修正：
//   ✅ [H04] escapeHtml 改從 lib/dom.js 導入
// ============================================

import { esc as escapeHtml } from '../lib/dom.js';
import { AppState } from '../core/state.js';
import { STORAGE_KEYS } from '../config/constants.js';

/* ============================================
   主函式
   ============================================ */
export function renderStatsCards(options) {
  const {
    container,
    cards = [],
    columns = 3,
    mb = 20,
    customFirstCell = null,
  } = options;

  const root = _resolveElement(container);
  if (!root) {
    console.warn('⚠️ renderStatsCards: 找不到容器', container);
    return null;
  }

  if (!cards.length && !customFirstCell) {
    root.innerHTML = '';
    return { container: root, refresh: () => {}, destroy: () => { root.innerHTML = ''; } };
  }

  let _unsubViewChange = null;
  let _resizeHandler = null;

  function _render() {
    const mode = _getStatsMode();
    const displayMode = _resolveDisplayMode(mode);

    if (displayMode === 'C') {
      _renderIntegrated(root, cards, mb, customFirstCell);
    } else {
      _renderCompact(root, cards, columns, mb, customFirstCell);
    }

    if (window.lucide) window.lucide.createIcons();
  }

  _render();

  if (_getStatsMode() === 'auto') {
    _unsubViewChange = AppState.on('view-change', _render);
    _resizeHandler = _debounce(() => _render(), 200);
    window.addEventListener('resize', _resizeHandler);
  }

  return {
    container: root,
    refresh: _render,
    destroy: () => {
      if (_unsubViewChange) {
        try { _unsubViewChange(); } catch (e) { /* noop */ }
        _unsubViewChange = null;
      }
      if (_resizeHandler) {
        window.removeEventListener('resize', _resizeHandler);
        _resizeHandler = null;
      }
      root.innerHTML = '';
    },
  };
}

/* ============================================
   模式解析
   ============================================ */
function _getStatsMode() {
  try {
    const saved = localStorage.getItem(STORAGE_KEYS.STATS_MODE);
    if (saved === 'integrated' || saved === 'compact' || saved === 'auto') return saved;
  } catch (e) { /* noop */ }
  return 'auto';
}

function _resolveDisplayMode(mode) {
  if (mode === 'integrated') return 'C';
  if (mode === 'compact') return 'B';
  if (typeof window !== 'undefined' && window.innerWidth < 640) return 'B';
  return AppState.getCurrentView() === 'table' ? 'C' : 'B';
}

/* ============================================
   方案 C：整合卡
   ============================================ */
function _renderIntegrated(root, cards, mb, customFirstCell) {
  const cellsHtml = [
    customFirstCell || '',
    ...cards.map((c) => _renderIntegratedCell(c)),
  ].join('');

  root.innerHTML = `
    <div class="stats-integrated" style="margin-bottom:${mb}px;">
      <div class="stats-integrated-grid">
        ${cellsHtml}
      </div>
    </div>
  `;
}

function _renderIntegratedCell(card) {
  const {
    title = '',
    value = '—',
    valueClass = '',
    hint = '',
    icon = '',
  } = card;

  const iconHtml = icon
    ? `<i data-lucide="${escapeHtml(icon)}" style="width:13px;height:13px;"></i>`
    : '';

  const titleHtml = iconHtml
    ? `<div class="stats-integrated-title">${iconHtml}<span>${escapeHtml(title)}</span></div>`
    : `<div class="stats-integrated-title"><span>${escapeHtml(title)}</span></div>`;

  const hintHtml = hint
    ? `<div class="stats-integrated-hint">${escapeHtml(hint)}</div>`
    : '';

  return `
    <div class="stats-integrated-cell">
      ${titleHtml}
      <div class="stats-integrated-value mono ${escapeHtml(valueClass)}">${_formatValue(value)}</div>
      ${hintHtml}
    </div>
  `;
}

/* ============================================
   方案 B：緊湊獨立卡
   ============================================ */
function _renderCompact(root, cards, columns, mb, customFirstCell) {
  const cols = columns || 3;
  const count = cards.length + (customFirstCell ? 1 : 0);
  const effectiveCols = Math.min(cols, count);

  let firstHtml = '';
  if (customFirstCell) {
    const sanitized = String(customFirstCell)
      .replace(/style="([^"]*)"/g, (m, s) => {
        const cleaned = s.replace(/grid-column\s*:\s*span\s*\d+\s*;?/g, '').trim();
        return cleaned ? `style="${cleaned}"` : '';
      })
      .replace(/class="stats-integrated-cell"/g, 'class="stats-compact-cell"');

    firstHtml = `
      <div class="glass-card stats-compact-card" style="grid-column: span 2; padding:0; overflow:hidden;">
        ${sanitized}
      </div>
    `;
  }

  root.innerHTML = `
    <div class="stats-compact-grid stats-compact-grid-${effectiveCols}" style="margin-bottom:${mb}px;">
      ${firstHtml}
      ${cards.map((c) => _renderCompactCard(c)).join('')}
    </div>
  `;
}

function _renderCompactCard(card) {
  const {
    title = '',
    value = '—',
    valueClass = '',
    hint = '',
    icon = '',
  } = card;

  const iconHtml = icon
    ? `<i data-lucide="${escapeHtml(icon)}" style="width:12px;height:12px;"></i>`
    : '';

  const titleHtml = iconHtml
    ? `<div class="glass-card-title" style="display:flex;align-items:center;gap:4px;">${iconHtml}<span>${escapeHtml(title)}</span></div>`
    : `<div class="glass-card-title">${escapeHtml(title)}</div>`;

  const hintHtml = hint
    ? `<div class="glass-card-hint">${escapeHtml(hint)}</div>`
    : '';

  return `
    <div class="glass-card stats-compact-card">
      ${titleHtml}
      <div class="glass-card-value mono ${escapeHtml(valueClass)}">${_formatValue(value)}</div>
      ${hintHtml}
    </div>
  `;
}

/* ============================================
   內部工具
   ============================================ */
function _formatValue(value) {
  if (value == null) return '—';
  if (typeof value === 'string') return value;
  return escapeHtml(String(value));
}

function _resolveElement(target) {
  if (typeof target === 'string') return document.getElementById(target);
  if (target instanceof HTMLElement) return target;
  return null;
}

function _debounce(fn, wait) {
  let timer = null;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), wait);
  };
}```

---

## 📄 js/shared/yearrange-card.js

```
// ============================================
// yearrange-card.js — 年份範圍卡（v103.0.1）
// 位置：js/shared/yearrange-card.js
// ============================================
// v103.0.1（B34-4）：
//   ✅ 從 database-yearrange.js 搬移（D-036）
//   ✅ 函式名 initYearRangeTab → initYearRangeCard
//   ✅ 供 settings.js 個人化 Tab 使用
// ============================================
import { listenYearRange, saveYearRange } from '../core/db.js';
import { getYearRange as getMergedYearRange } from '../config/app-config.js';
import { LIMITS } from '../config/constants.js';
import { esc } from '../lib/dom.js';
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { buildForm } from '../ui/form-builder.js';
import { createListenerGroup } from './listener-group.js';

let _container = null;
let _formApi = null;
let _currentRange = { startYear: null, futureYears: LIMITS.YEAR_FUTURE_DEFAULT };
let _hasFamilyOverride = false;
let _resetBtnHandler = null;

const listenerGroup = createListenerGroup();

export function initYearRangeCard(containerId) {
  _container = document.getElementById(containerId);
  if (!_container) return null;

  _container.innerHTML = _buildSkeleton();
  _renderForm();
  _bindEvents();

  listenerGroup.add(listenYearRange((data) => {
    if (data && (data.startYear != null || data.futureYears != null)) {
      _currentRange = {
        startYear: data.startYear != null ? Number(data.startYear) : null,
        futureYears: Number(data.futureYears) || LIMITS.YEAR_FUTURE_DEFAULT,
      };
      _hasFamilyOverride = true;
    } else {
      const merged = getMergedYearRange();
      _currentRange = { startYear: merged.startYear, futureYears: merged.endYear - new Date().getFullYear() };
      _hasFamilyOverride = false;
    }
    if (_formApi) _formApi.setData({
      'db-yr-start': _currentRange.startYear != null ? _currentRange.startYear : '',
      'db-yr-future': _currentRange.futureYears,
    });
    _refreshPreview();
  }));

  return { refresh: _refreshPreview, destroy: _destroy };
}

function _buildSkeleton() {
  return `
    <div class="banner" style="margin-bottom:16px;">
      ℹ️ 用於全站的年份下拉選單範圍。
      <strong>起始年份</strong>：往回至少顯示 3 年（自動調整）。
      <strong>往後顯示年數</strong>：當前年再往後顯示 N 年。
    </div>
    <div class="grid grid-2" style="gap:16px; align-items:start;">
      <div>
        <div class="glass-card">
          <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:16px;">
            <i data-lucide="calendar" style="width:16px;height:16px; color:var(--neon-cyan);"></i>
            <span>年份範圍設定</span>
          </div>
          <div id="db-yearrange-form-root"></div>
          <div style="margin-top:16px; padding-top:16px; border-top:1px dashed rgba(255,255,255,0.08); text-align:right;">
            <button type="button" class="btn btn-ghost btn-sm" id="db-yearrange-reset">
              <i data-lucide="rotate-ccw" style="width:14px;height:14px;"></i> 重置為平台預設
            </button>
          </div>
        </div>
      </div>
      <div>
        <div class="glass-card">
          <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:16px;">
            <i data-lucide="eye" style="width:16px;height:16px; color:var(--neon-cyan);"></i>
            <span>實際範圍預覽</span>
          </div>
          <div id="db-yearrange-preview"></div>
        </div>
      </div>
    </div>
  `;
}

function _renderForm() {
  const curY = new Date().getFullYear();
  _formApi = buildForm({
    containerId: 'db-yearrange-form-root',
    fields: [
      { type: 'number', id: 'db-yr-start', label: '起始年份', min: 1900, max: 2200, step: 1,
        placeholder: `例如：${curY - 3}（留空 = 當前年 - 3）`,
        hint: `若填寫的年份大於「當前年 - ${LIMITS.YEAR_PAST_DEFAULT}」，會自動調整為「當前年 - ${LIMITS.YEAR_PAST_DEFAULT}」` },
      { type: 'number', id: 'db-yr-future', label: '往後顯示年數', required: true, min: 0, max: 20, step: 1, placeholder: String(LIMITS.YEAR_FUTURE_DEFAULT) },
    ],
    submitText: '儲存設定', showCancel: false, showReset: false,
    beforeSubmit: _validateYearRange,
    onSubmit: _handleSubmit,
  });
  _formApi.setData({ 'db-yr-start': '', 'db-yr-future': LIMITS.YEAR_FUTURE_DEFAULT });
  _formApi.onFieldChange('db-yr-start', _refreshPreview);
  _formApi.onFieldChange('db-yr-future', _refreshPreview);
}

function _validateYearRange(data) {
  const { startYear, futureYears } = _parseFormValues(data);
  if (startYear != null && (isNaN(startYear) || startYear < 1900 || startYear > 2200))
    return { field: 'db-yr-start', message: '請填寫合理的年份（1900 ~ 2200）' };
  if (isNaN(futureYears) || futureYears < 0 || futureYears > 20)
    return { field: 'db-yr-future', message: '請填寫 0 ~ 20 之間的數字' };
  return true;
}

async function _handleSubmit(data) {
  const { startYear, futureYears } = _parseFormValues(data);
  try {
    await saveYearRange({ startYear, futureYears });
    _hasFamilyOverride = true;
    showToast('✅ 年份範圍已儲存', 'success');
    _refreshPreview();
  } catch (err) { showToast('儲存失敗：' + err.message, 'error'); }
}

function _bindEvents() {
  _resetBtnHandler = async () => {
    if (!_hasFamilyOverride) { showToast('目前已是平台預設', 'info'); return; }
    const ok = await openConfirm('確定要清除家庭自訂，改回使用平台預設嗎？', { title: '重置年份範圍', okText: '重置', okClass: 'btn-danger' });
    if (!ok) return;
    try {
      await saveYearRange({ startYear: null, futureYears: LIMITS.YEAR_FUTURE_DEFAULT });
      _hasFamilyOverride = false;
      showToast('✅ 已重置為平台預設', 'success');
    } catch (err) { showToast('重置失敗：' + err.message, 'error'); }
  };
  _container.querySelector('#db-yearrange-reset')?.addEventListener('click', _resetBtnHandler);
}

function _refreshPreview() {
  const previewEl = _container?.querySelector('#db-yearrange-preview');
  if (!previewEl) return;
  const { startYear, futureYears } = _parseFormValues();
  const curY = new Date().getFullYear();
  let actualStart, wasAdjusted = false;
  if (startYear != null && !isNaN(startYear)) {
    const minStart = curY - LIMITS.YEAR_PAST_DEFAULT;
    actualStart = Math.min(startYear, minStart);
    if (startYear > minStart) wasAdjusted = true;
  } else actualStart = curY - LIMITS.YEAR_PAST_DEFAULT;
  const actualEnd = curY + futureYears;
  const years = [];
  for (let y = actualStart; y <= actualEnd && years.length < 30; y++) years.push(y);
  const totalYears = actualEnd - actualStart + 1;
  const curYStr = String(curY);

  previewEl.innerHTML = `
    <div style="display:flex; flex-direction:column; gap:14px;">
      <div style="display:grid; grid-template-columns:1fr 1fr; gap:12px;">
        <div style="padding:12px; border:1px solid var(--glass-border); border-radius:var(--radius-md); background:rgba(8,11,17,0.4);">
          <div style="font-size:11px; color:var(--text-muted); text-transform:uppercase; letter-spacing:1px; margin-bottom:4px;">起始年</div>
          <div class="mono" style="font-size:20px; font-weight:700; color:var(--neon-cyan);">${actualStart}</div>
        </div>
        <div style="padding:12px; border:1px solid var(--glass-border); border-radius:var(--radius-md); background:rgba(8,11,17,0.4);">
          <div style="font-size:11px; color:var(--text-muted); text-transform:uppercase; letter-spacing:1px; margin-bottom:4px;">結束年</div>
          <div class="mono" style="font-size:20px; font-weight:700; color:var(--neon-cyan);">${actualEnd}</div>
        </div>
      </div>
      <div style="padding:10px 12px; border-radius:var(--radius-md); background:rgba(0,240,255,0.04); border:1px solid var(--glass-border); font-size:12px; color:var(--text-secondary);">
        共 <span class="mono text-cyan" style="font-weight:700;">${totalYears}</span> 個年份
        ${wasAdjusted ? `<br><span style="color:var(--neon-orange);">⚠️ 起始年已自動調整為 ${actualStart}</span>` : ''}
      </div>
      <div>
        <div style="font-size:11px; color:var(--text-muted); text-transform:uppercase; letter-spacing:1px; margin-bottom:8px;">年份清單</div>
        <div style="display:flex; flex-wrap:wrap; gap:6px;">
          ${years.map((y) => {
            const isCurrent = String(y) === curYStr;
            return `<span class="mono" style="padding:4px 10px; border-radius:var(--radius-sm); border:1px solid ${isCurrent ? 'rgba(0,240,255,0.5)' : 'var(--glass-border)'}; background:${isCurrent ? 'rgba(0,240,255,0.2)' : 'rgba(8,11,17,0.4)'}; color:${isCurrent ? 'var(--neon-cyan)' : 'var(--text-secondary)'}; font-size:12px; font-weight:${isCurrent ? '700' : '400'};">${y}</span>`;
          }).join('')}
          ${totalYears > 30 ? `<span class="text-muted" style="padding:4px 10px; font-size:12px;">…還有 ${totalYears - 30} 年</span>` : ''}
        </div>
      </div>
      <div style="font-size:11px; color:var(--text-muted); padding-top:10px; border-top:1px dashed rgba(255,255,255,0.08);">
        目前來源：${_hasFamilyOverride ? '<span class="badge badge-info">家庭自訂</span>' : '<span class="badge badge-muted">平台預設</span>'}
      </div>
    </div>
  `;
  if (window.lucide) window.lucide.createIcons();
}

function _parseFormValues(data) {
  let rawStart, rawFuture;
  const startEl = document.getElementById('db-yr-start');
  const futureEl = document.getElementById('db-yr-future');

  if (startEl || futureEl) {
    rawStart = startEl ? startEl.value : '';
    rawFuture = futureEl ? futureEl.value : '';
  } else if (data) {
    rawStart = data['db-yr-start'];
    rawFuture = data['db-yr-future'];
  } else if (_formApi) {
    rawStart = _formApi.getFieldValue('db-yr-start');
    rawFuture = _formApi.getFieldValue('db-yr-future');
  }

  const startYear = (rawStart === '' || rawStart == null) ? null : Number(rawStart);
  let futureYears = Number(rawFuture);
  if (isNaN(futureYears)) futureYears = LIMITS.YEAR_FUTURE_DEFAULT;
  return { startYear, futureYears };
}

function _destroy() {
  listenerGroup.destroy();
  if (_formApi) { try { _formApi.destroy(); } catch (e) {} _formApi = null; }
  if (_resetBtnHandler && _container) {
    _container.querySelector('#db-yearrange-reset')?.removeEventListener('click', _resetBtnHandler);
    _resetBtnHandler = null;
  }
  _container = null;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/shared/yearrange-card.js
   Version: 1791902620
   Batch: B34-4
   Writer: A20261013-004#118
   ═══════════════════════════════════════════ */```

---

## 📄 js/ui/collapsible.js

```
// ============================================
// collapsible.js — 可摺疊卡片（v103.0.0）
// 位置：js/ui/collapsible.js
// ============================================
// v103.0.0 重構：
//   ✅ 從 js/shared/collapsible-card.js 改名並移入 js/ui/
//   ✅ storageKey 統一使用 STORAGE_KEYS.UI_PREFIX
//   ✅ 新增 expandAll / collapseAll 便利方法
//   ✅ 保留 v101.5 全部功能
// ============================================

import { STORAGE_KEYS } from '../config/constants.js';

/* ============================================
   主函式
   ============================================ */

/**
 * 初始化可摺疊卡片
 * @param {string} cardId - 卡片 ID
 * @param {string} storageKey - localStorage 儲存 key（自動加前綴）
 * @param {boolean} [defaultOpen=false] - 預設是否展開
 * @returns {Object|null}
 */
export function initCollapsible(cardId, storageKey, defaultOpen = false) {
  const card = document.getElementById(cardId);
  if (!card) {
    console.warn(`⚠️ initCollapsible: 找不到 #${cardId}`);
    return null;
  }

  const header = card.querySelector('.collapsible-header');
  const body = card.querySelector('.collapsible-body');
  if (!header || !body) {
    console.warn(`⚠️ initCollapsible: #${cardId} 缺少 .collapsible-header 或 .collapsible-body`);
    return null;
  }

  const key = _normalizeKey(storageKey);

  const setState = (open) => {
    if (open) {
      card.classList.add('open');
      body.style.display = 'block';
    } else {
      card.classList.remove('open');
      body.style.display = 'none';
    }
    try {
      localStorage.setItem(key, String(open));
    } catch (e) { /* noop */ }
    _refreshIconsIn(card);
  };

  let savedOpen = null;
  try {
    savedOpen = localStorage.getItem(key);
  } catch (e) { /* noop */ }

  const initialOpen = savedOpen !== null ? savedOpen === 'true' : defaultOpen;
  setState(initialOpen);

  const clickHandler = () => {
    const newState = !card.classList.contains('open');
    setState(newState);
  };
  header.addEventListener('click', clickHandler);

  return {
    open:   () => setState(true),
    close:  () => setState(false),
    toggle: () => setState(!card.classList.contains('open')),
    isOpen: () => card.classList.contains('open'),

    destroy: () => {
      header.removeEventListener('click', clickHandler);
    },
  };
}

/**
 * 批次初始化多個卡片
 */
export function initCollapsibleList(configs = []) {
  return configs.map((cfg) =>
    initCollapsible(cfg.cardId, cfg.storageKey, cfg.defaultOpen || false)
  );
}

/**
 * 向後相容：舊 API 名稱
 * @deprecated 請改用 initCollapsible
 */
export const initCollapsibleCard = initCollapsible;

/**
 * 向後相容：舊 API 名稱
 * @deprecated 請改用 initCollapsibleList
 */
export const initCollapsibleCards = initCollapsibleList;

/* ============================================
   內部工具
   ============================================ */

function _normalizeKey(storageKey) {
  if (!storageKey) return `${STORAGE_KEYS.UI_PREFIX}collapsible-default`;
  return storageKey.startsWith(STORAGE_KEYS.UI_PREFIX)
    ? storageKey
    : `${STORAGE_KEYS.UI_PREFIX}${storageKey}`;
}

function _refreshIconsIn(container) {
  if (!window.lucide || typeof window.lucide.createIcons !== 'function') return;
  try {
    container.querySelectorAll('svg[data-lucide], i[data-lucide]').forEach((el) => {
      if (el.tagName === 'svg') el.remove();
    });
    window.lucide.createIcons({ nameAttr: 'data-lucide' });
  } catch (e) { /* noop */ }
}
```

---

## 📄 js/ui/dropdown-plus.js

```
// ============================================
// dropdown-plus.js — 下拉旁「+ 號」按鈕（v1.0.0）
// 位置：js/ui/dropdown-plus.js
// ============================================
// 依 D-029（B 層規則 1~3）：
//   ✅ 簡單實體 → openPrompt（只輸名稱）
//   ✅ 複雜實體 → openEntityModal
//   ✅ 新增成功 → 回傳 { id, code } → 呼叫端自動選中
// ============================================

import { openPrompt } from './modal.js';
import { openEntityAddModal } from '../entity/entity-modal.js';
import { createEntity } from '../entity/entity-helpers.js';
import { getEntityDef } from '../entity/entity-definitions.js';
import { showToast } from './toast.js';

/* ============================================
   1. 簡單 / 複雜實體分類（D-029 規則 1~2）
   ============================================
   簡單：只有 1~2 個欄位（name + order / role）
   複雜：需更多欄位或特殊流程
   ============================================ */
const SIMPLE_ENTITIES = [
  'member',
  'bank',
  'category',
  'payment',
  'company',
];

export function isSimpleEntity(entityType) {
  return SIMPLE_ENTITIES.includes(entityType);
}

/* ============================================
   2. 主入口：依 entityType 觸發 prompt 或 modal
   ============================================
   回傳：{ id, code } | null
   ============================================ */
export async function handlePlusClick(entityType, context = {}) {
  if (isSimpleEntity(entityType)) {
    return await _promptCreate(entityType, context);
  }
  return await _modalCreate(entityType, context);
}

/* ============================================
   3. 簡單實體：openPrompt
   ============================================ */
async function _promptCreate(entityType, context) {
  const def = getEntityDef(entityType);
  const label = def?.label || '項目';

  const name = await openPrompt({
    title: `新增${label}`,
    label: `${label}名稱`,
    placeholder: `請輸入${label}名稱`,
    maxlength: 30,
  });

  if (!name) return null;

  try {
    /* createEntity 內部會自動生成 code（依 codePrefix） */
    const result = await createEntity(entityType, { name, ...context }, []);
    showToast(`✅ 已新增${label}`, 'success');
    return result; /* { id, code } */
  } catch (err) {
    showToast('新增失敗：' + (err.message || err), 'error');
    return null;
  }
}

/* ============================================
   4. 複雜實體：openEntityModal
   ============================================ */
function _modalCreate(entityType, context) {
  return new Promise((resolve) => {
    openEntityAddModal(entityType, {
      initialData: Object.keys(context).length > 0 ? context : null,
      allRows: [],
      onSuccess: (result) => resolve(result), /* { id, code, data } */
      onCancel: () => resolve(null),
    });
  });
}

/* ============================================
   5. 為 select 欄位掛上 + 號按鈕
   ============================================
   呼叫端（form-builder）傳入：
   - selectEl: <select> 元素
   - entityType: 'member' 等
   - onCreated: (result) => Promise 或 void（成功後自動選中）
   ============================================ */
export function attachPlusButton(selectEl, entityType, onCreated) {
  if (!selectEl || !entityType) return null;

  const parent = selectEl.parentElement;
  if (!parent) return null;

  /* 防止重複插入 */
  if (parent.querySelector(`[data-plus-for="${entityType}"]`)) return null;

  /* 包成 flex 容器（select + 按鈕並排） */
  const wrapper = document.createElement('div');
  wrapper.style.display = 'flex';
  wrapper.style.gap = '6px';
  wrapper.style.alignItems = 'center';
  parent.insertBefore(wrapper, selectEl);
  wrapper.appendChild(selectEl);

  const btn = document.createElement('button');
  btn.type = 'button';
  btn.className = 'btn btn-sm btn-ghost';
  btn.setAttribute('data-plus-for', entityType);
  btn.title = `新增${getEntityDef(entityType)?.label || ''}`;
  btn.style.flexShrink = '0';
  btn.style.padding = '6px 10px';
  btn.innerHTML = '<i data-lucide="plus" style="width:14px;height:14px;"></i>';

  btn.addEventListener('click', async (e) => {
    e.preventDefault();
    e.stopPropagation();
    if (btn.disabled) return;
    btn.disabled = true;

    try {
      const result = await handlePlusClick(entityType);
      if (result && typeof onCreated === 'function') {
        await onCreated(result);
      }
    } catch (err) {
      console.error('[dropdown-plus] + 號失敗：', err);
      showToast('操作失敗：' + (err.message || err), 'error');
    } finally {
      btn.disabled = false;
    }
  });

  wrapper.appendChild(btn);
  if (window.lucide) window.lucide.createIcons();

  return btn;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/ui/dropdown-plus.js
   Version: 1791901780
   Batch: B34-4
   Writer: A20261013-004#100
   ═══════════════════════════════════════════ */```

---

## 📄 js/ui/form-builder.js

```
// ============================================
// form-builder.js — 通用動態表單建構器（v103.0.13）
// 位置：js/ui/form-builder.js
// ============================================
// v103.0.13（B34-4）：
//   ✅ plusEntity 支援 f.onPlusReload callback（呼叫端自訂重載邏輯）
//   ✅ 若同時有 optionsSource 但無 onPlusReload，走預設 getDynamicOptions
// v103.0.12（B34-4）：
//   ✅ 支援 f.plusEntity：下拉旁自動生成「+ 號」按鈕
// ============================================

import { esc } from '../lib/dom.js';
import { showToast } from './toast.js';
import { getDynamicOptions } from '../entity/entity-helpers.js';
import { handlePlusClick } from './dropdown-plus.js';

export function buildForm(options) {
  const {
    containerId,
    fields = [],
    idPrefix = '',
    submitText = '儲存',
    cancelText = '取消',
    resetText = '重置',
    showCancel = true,
    showReset = false,
    onSubmit,
    onCancel,
    onReset,
    beforeSubmit,
    autoValidate = true,
    initialData = null,
  } = options;

  const root = document.getElementById(containerId);
  if (!root) {
    console.warn(`⚠️ buildForm: 找不到容器 #${containerId}`);
    return null;
  }

  const fieldIdMap = {};
  fields.forEach((f) => {
    if (f.type === 'custom' || f.type === 'hidden') return;
    fieldIdMap[f.id] = idPrefix ? `${idPrefix}${f.id}` : f.id;
  });

  const fieldsHtml = _renderFields(fields, fieldIdMap);

  const buttons = [];
  if (showReset) {
    buttons.push(`<button type="button" class="btn btn-ghost" data-form-action="reset">${esc(resetText)}</button>`);
  }
  if (showCancel) {
    buttons.push(`<button type="button" class="btn btn-ghost" data-form-action="cancel">${esc(cancelText)}</button>`);
  }
  buttons.push(`<button type="submit" class="btn btn-primary" data-form-action="submit">
    <i data-lucide="check"></i> <span>${esc(submitText)}</span>
  </button>`);

  root.innerHTML = `
    <form class="form-builder-form" novalidate>
      <div class="form-stack">
        ${fieldsHtml}
      </div>
      <div class="form-builder-actions" style="display:flex; justify-content:flex-end; gap:10px; margin-top:16px;">
        ${buttons.join('')}
      </div>
    </form>
  `;

  const form = root.querySelector('form');
  const submitBtn = form.querySelector('[data-form-action="submit"]');
  const submitBtnLabel = submitBtn.querySelector('span');

  let _submitting = false;
  const _dynamicCleanups = [];

  form.addEventListener('submit', async (e) => {
    e.preventDefault();
    if (_submitting) return;
    _clearErrors();

    const data = _collectData(fields, fieldIdMap);

    if (autoValidate) {
      const error = _validateRequired(fields, data, fieldIdMap);
      if (error) {
        _showFieldError(error.field, error.message, fieldIdMap);
        showToast(error.message, 'warning');
        const el = document.getElementById(error.field);
        if (el) el.focus();
        return;
      }
    }

    if (typeof beforeSubmit === 'function') {
      let result;
      try {
        result = await beforeSubmit(data);
      } catch (err) {
        console.error('[form-builder] beforeSubmit 失敗：', err);
        showToast('驗證失敗：' + (err.message || err), 'error');
        return;
      }
      if (result === false) return;
      if (typeof result === 'string') {
        showToast(result, 'warning');
        return;
      }
      if (result && result.field && result.message) {
        const resolvedField = fieldIdMap[result.field] || result.field;
        _showFieldError(resolvedField, result.message, fieldIdMap);
        showToast(result.message, 'warning');
        return;
      }
    }

    if (typeof onSubmit !== 'function') return;

    _setSubmitting(true);
    try {
      await onSubmit(data, api);
    } catch (err) {
      console.error('[form-builder] 送出失敗：', err);
      showToast('送出失敗：' + (err.message || err), 'error');
    } finally {
      _setSubmitting(false);
    }
  });

  form.addEventListener('click', (e) => {
    const btn = e.target.closest('[data-form-action]');
    if (!btn) return;
    const action = btn.dataset.formAction;

    if (action === 'cancel' && typeof onCancel === 'function') {
      onCancel();
    } else if (action === 'reset') {
      form.reset();
      _clearErrors();
      if (typeof onReset === 'function') onReset();
    }
  });

  function _setSubmitting(isSubmitting) {
    _submitting = isSubmitting;
    submitBtn.disabled = isSubmitting;
    if (submitBtnLabel) {
      submitBtnLabel.textContent = isSubmitting ? '處理中…' : submitText;
    }
  }

  function _clearErrors() {
    form.querySelectorAll('.field-error').forEach((el) => el.remove());
    form.querySelectorAll('.field.has-error').forEach((el) => {
      el.classList.remove('has-error');
    });
  }

  function _showFieldError(fieldId, message, map) {
    const resolvedId = map && map[fieldId] ? map[fieldId] : fieldId;
    const el = document.getElementById(resolvedId);
    if (!el) return;
    const fieldEl = el.closest('.field');
    if (!fieldEl) return;
    fieldEl.classList.add('has-error');

    const old = fieldEl.querySelector('.field-error');
    if (old) old.remove();

    const errorDiv = document.createElement('div');
    errorDiv.className = 'field-error';
    errorDiv.style.cssText = 'color:var(--neon-red); font-size:11px; margin-top:4px;';
    errorDiv.textContent = message;
    fieldEl.appendChild(errorDiv);
  }

  async function _initDynamicFields() {
    for (const f of fields) {
      if (f.type === 'select' && f.optionsSource) {
        try {
          const opts = await getDynamicOptions(f.optionsSource);
          api.updateOptions(f.id, opts, {
            includeEmpty: f.includeEmpty !== false,
            emptyText: f.emptyText || '— 請選擇 —',
          });
        } catch (err) {
          console.warn(`[form-builder] 載入動態選項失敗 (${f.optionsSource})：`, err);
        }
      }

      if (initialData && initialData[f.id] !== undefined) {
        api.setFieldValue(f.id, initialData[f.id]);
      } else if (f.defaultValue != null) {
        const def = typeof f.defaultValue === 'function' ? f.defaultValue() : f.defaultValue;
        api.setFieldValue(f.id, def);
      }

      if (f.dependsOn) {
        const sourceId = fieldIdMap[f.dependsOn] || f.dependsOn;
        const sourceEl = document.getElementById(sourceId);
        if (sourceEl) {
          const handler = async () => {
            const sourceVal = sourceEl.value;
            if (f.optionsSource === 'items') {
              const opts = await getDynamicOptions('items', { categoryId: sourceVal });
              api.updateOptions(f.id, opts, {
                includeEmpty: true,
                emptyText: sourceVal ? '— 請選擇項目 —' : '— 請先選擇類別 —',
              });
            }
          };
          sourceEl.addEventListener('change', handler);
          _dynamicCleanups.push(() => sourceEl.removeEventListener('change', handler));
        }
      }

      /* v103.0.13（B34-4）：plusEntity → 綁定 + 號 */
      if (f.type === 'select' && f.plusEntity) {
        const finalId = fieldIdMap[f.id] || f.id;
        const btn = document.getElementById(`${finalId}-extra-btn`);
        if (btn) {
          const handler = async (e) => {
            e.preventDefault();
            e.stopPropagation();
            if (btn.disabled) return;
            btn.disabled = true;

            try {
              const result = await handlePlusClick(f.plusEntity);
              if (!result) return;

              /* v103.0.13：優先走 onPlusReload（呼叫端自訂） */
              if (typeof f.onPlusReload === 'function') {
                await f.onPlusReload(result, api);
              } else if (f.optionsSource) {
                /* 預設：重載選項 + 自動選中 */
                const opts = await getDynamicOptions(f.optionsSource);
                api.updateOptions(f.id, opts, {
                  includeEmpty: f.includeEmpty !== false,
                  emptyText: f.emptyText || '— 請選擇 —',
                });
                const valueKey = f.plusValueKey || 'id';
                const newValue = result[valueKey] || result.id;
                if (newValue) api.setFieldValue(f.id, newValue);
              }
            } catch (err) {
              console.error('[form-builder] plusEntity 失敗：', err);
            } finally {
              btn.disabled = false;
            }
          };
          btn.addEventListener('click', handler);
          _dynamicCleanups.push(() => btn.removeEventListener('click', handler));
        }
      }
    }
  }

  const api = {
    root,
    form,

    getData: () => _collectData(fields, fieldIdMap),

    setData: (data) => {
      Object.entries(data || {}).forEach(([k, v]) => {
        const resolvedId = fieldIdMap[k] || k;
        const el = document.getElementById(resolvedId);
        if (!el) return;
        if (el.type === 'checkbox') {
          el.checked = !!v;
        } else {
          el.value = v ?? '';
        }
      });
    },

    setFieldValue: (fieldId, value) => {
      const resolvedId = fieldIdMap[fieldId] || fieldId;
      const el = document.getElementById(resolvedId);
      if (!el) return;
      if (el.type === 'checkbox') {
        el.checked = !!value;
      } else {
        el.value = value ?? '';
      }
    },

    getFieldValue: (fieldId) => {
      const resolvedId = fieldIdMap[fieldId] || fieldId;
      const el = document.getElementById(resolvedId);
      if (!el) return null;
      if (el.type === 'checkbox') return el.checked;
      if (el.type === 'number') {
        const n = Number(el.value);
        return isNaN(n) ? 0 : n;
      }
      return el.value;
    },

    getFieldEl: (fieldId) => {
      const resolvedId = fieldIdMap[fieldId] || fieldId;
      return document.getElementById(resolvedId);
    },

    reset: () => {
      form.reset();
      _clearErrors();
      if (typeof onReset === 'function') onReset();
    },

    setError: (fieldId, message) => {
      _showFieldError(fieldId, message, fieldIdMap);
    },

    clearErrors: _clearErrors,

    updateOptions: (fieldId, options, config = {}) => {
      const resolvedId = fieldIdMap[fieldId] || fieldId;
      const el = document.getElementById(resolvedId);
      if (!el) return;
      const { includeEmpty = true, emptyText = '— 請選擇 —' } = config;
      const cur = el.value;
      let html = includeEmpty ? `<option value="">${esc(emptyText)}</option>` : '';
      html += options.map((o) =>
        `<option value="${esc(o.value)}">${esc(o.label)}</option>`
      ).join('');
      el.innerHTML = html;
      if (cur && options.some((o) => String(o.value) === cur)) el.value = cur;
    },

    onFieldChange: (fieldId, callback) => {
      const resolvedId = fieldIdMap[fieldId] || fieldId;
      const el = document.getElementById(resolvedId);
      if (!el) return () => {};
      el.addEventListener('change', callback);
      return () => el.removeEventListener('change', callback);
    },

    submit: () => {
      form.dispatchEvent(new Event('submit', { cancelable: true, bubbles: true }));
    },

    setSubmitting: _setSubmitting,

    destroy: () => {
      _dynamicCleanups.forEach((fn) => {
        try { fn(); } catch (e) { /* noop */ }
      });
      _dynamicCleanups.length = 0;
      root.innerHTML = '';
    },
  };

  _initDynamicFields().then(() => {
    if (window.lucide) window.lucide.createIcons();
  });

  if (window.lucide) window.lucide.createIcons();

  return api;
}

/* ============================================
   欄位渲染
   ============================================ */

function _renderFields(fields, fieldIdMap) {
  const output = [];
  let buffer = [];

  const flushBuffer = () => {
    if (buffer.length === 0) return;
    output.push(`<div class="form-stack-row">${buffer.join('')}</div>`);
    buffer = [];
  };

  fields.forEach((f) => {
    const html = _renderField(f, fieldIdMap);

    if (f.layout === 'half') {
      buffer.push(html);
      if (buffer.length >= 2) flushBuffer();
    } else {
      flushBuffer();
      output.push(html);
    }
  });

  flushBuffer();
  return output.join('');
}

function _renderField(f, fieldIdMap) {
  const {
    type,
    id,
    label,
    required = false,
    placeholder = '',
    options = [],
    min,
    max,
    step,
    disabled = false,
    extraBtn,
    hint,
  } = f;

  const finalId = fieldIdMap[id] || id;

  let inputHtml = '';

  if (type === 'select') {
    let opts = '';
    if (f.includeEmpty !== false) {
      opts += `<option value="">${esc(f.emptyText || '— 請選擇 —')}</option>`;
    }
    opts += options.map((o) =>
      `<option value="${esc(o.value)}">${esc(o.label)}</option>`
    ).join('');
    inputHtml = `<select class="select" id="${finalId}" ${required ? 'required' : ''} ${disabled ? 'disabled' : ''}>${opts}</select>`;
  } else if (type === 'number' || type === 'number-plain') {
    const attrs = [
      min != null ? `min="${min}"` : '',
      max != null ? `max="${max}"` : '',
      step != null ? `step="${step}"` : 'step="1"',
    ].filter(Boolean).join(' ');
    const inputClass = type === 'number' ? 'input mono' : 'input';
    inputHtml = `<input class="${inputClass}" id="${finalId}" type="number" ${attrs} ${required ? 'required' : ''} ${disabled ? 'disabled' : ''} placeholder="${esc(placeholder)}">`;
  } else if (type === 'text') {
    inputHtml = `<input class="input" id="${finalId}" type="text" ${required ? 'required' : ''} ${disabled ? 'disabled' : ''} placeholder="${esc(placeholder)}" maxlength="${f.maxlength || 60}">`;
  } else if (type === 'textarea') {
    inputHtml = `<textarea class="input" id="${finalId}" ${required ? 'required' : ''} ${disabled ? 'disabled' : ''} placeholder="${esc(placeholder)}" maxlength="${f.maxlength || 200}" rows="${f.rows || 3}"></textarea>`;
  } else if (type === 'checkbox') {
    return `
      <div class="field" style="display:flex; align-items:center; gap:8px;">
        <input type="checkbox" id="${finalId}" style="width:auto; cursor:pointer;" ${disabled ? 'disabled' : ''}>
        <label for="${finalId}" style="font-size:13px; color:var(--text-secondary); cursor:pointer;">${esc(label)}</label>
      </div>
    `;
  } else if (type === 'custom') {
    return `<div class="field">${f.html || ''}</div>`;
  } else if (type === 'hidden') {
    return `<input type="hidden" id="${finalId}" value="${esc(f.value || '')}">`;
  }

  const effectiveExtraBtn = extraBtn
    || (type === 'select' && f.plusEntity
      ? { icon: 'plus', title: `新增${esc(label || '')}` }
      : null);

  const inputWrapper = effectiveExtraBtn
    ? `<div style="display:flex; gap:8px;">
         <div style="flex:1;">${inputHtml}</div>
         <button type="button" class="btn btn-sm btn-ghost" id="${finalId}-extra-btn" style="padding:0 12px;" title="${esc(effectiveExtraBtn.title || '')}">
           <i data-lucide="${esc(effectiveExtraBtn.icon || 'plus')}" style="width:16px;height:16px;"></i>
         </button>
       </div>`
    : inputHtml;

  const hintHtml = hint
    ? `<div class="glass-card-hint" style="margin-top:4px;">${esc(hint)}</div>`
    : '';

  return `
    <div class="field">
      <label class="field-label" for="${finalId}">${esc(label)}${required ? ' *' : ''}</label>
      ${inputWrapper}
      ${hintHtml}
    </div>
  `;
}

function _collectData(fields, fieldIdMap) {
  const data = {};
  fields.forEach((f) => {
    if (f.type === 'custom') return;
    const finalId = fieldIdMap[f.id] || f.id;
    const el = document.getElementById(finalId);
    if (!el) return;

    if (f.type === 'checkbox') {
      data[f.id] = el.checked;
    } else if (f.type === 'number' || f.type === 'number-plain') {
      data[f.id] = el.value === '' ? 0 : (isNaN(Number(el.value)) ? 0 : Number(el.value));
    } else {
      data[f.id] = el.value;
    }
  });
  return data;
}

function _validateRequired(fields, data, fieldIdMap) {
  for (const f of fields) {
    if (!f.required) continue;
    if (f.type === 'custom' || f.type === 'hidden') continue;

    const val = data[f.id];
    const isEmpty = val === '' || val === null || val === undefined;

    if (isEmpty) {
      const finalId = fieldIdMap[f.id] || f.id;
      return {
        field: finalId,
        message: `請填寫「${f.label}」`,
      };
    }
  }
  return null;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/ui/form-builder.js
   Version: 1791901960
   Batch: B34-4
   Writer: A20261013-004#103
   ═══════════════════════════════════════════ */```

---

## 📄 js/ui/modal.js

```
// ============================================
// modal.js — Modal 開關 + 確認框 + 輕量輸入框（v103.0.24）
// 位置：js/ui/modal.js
// ============================================
// v103.0.24（B34-4）：
//   ✅ 新增 openPrompt（輕量輸入框，D-062 / Q126 / Q136）
//   ✅ openPrompt 開啟前先 resolve 舊 Promise（null）
// v103.0.23（B31-1）：
//   ✅ openConfirm 開啟前先 resolve 舊 Promise（false）
// v103.0.22（B25-2）：
//   ✅ openModal 自動呼叫 bindModalEscClose（首次）
// ============================================

import { esc } from '../lib/dom.js';

/* ============================================
   Modal 堆疊
   ============================================ */
const _stack = [];

/* ============================================
   開關
   ============================================ */

export function openModal(target, options = {}) {
  const { pushToStack = true } = options;
  const modal = _resolveElement(target);
  if (!modal) return;

  modal.classList.add('active');
  document.body.style.overflow = 'hidden';

  if (pushToStack && !_stack.includes(modal)) {
    _stack.push(modal);
  }

  bindModalEscClose();
}

export function closeModal(target) {
  const modal = _resolveElement(target);
  if (!modal) return;

  modal.classList.remove('active');

  const idx = _stack.indexOf(modal);
  if (idx >= 0) _stack.splice(idx, 1);

  if (_stack.length === 0) {
    document.body.style.overflow = '';
  }
}

export function closeAllModals() {
  document.querySelectorAll('.modal-overlay.active').forEach((m) => {
    m.classList.remove('active');
  });
  _stack.length = 0;
  document.body.style.overflow = '';
}

export function getTopModal() {
  return _stack.length > 0 ? _stack[_stack.length - 1] : null;
}

/* ============================================
   綁定工具
   ============================================ */

export function bindModalCancel(modalId, cancelBtnId) {
  const btn = document.getElementById(cancelBtnId);
  if (!btn) return;
  btn.addEventListener('click', (e) => {
    e.preventDefault();
    closeModal(modalId);
  });
}

export function bindModalBackdropClose(modalId) {
  const modal = document.getElementById(modalId);
  if (!modal) return;
  modal.addEventListener('click', (e) => {
    if (e.target === modal) closeModal(modalId);
  });
}

export function bindModalEscClose() {
  if (window._modalEscBound) return;
  window._modalEscBound = true;

  document.addEventListener('keydown', (e) => {
    if (e.key !== 'Escape') return;
    const top = getTopModal();
    if (!top) return;
    closeModal(top);
  });
}

export function bindModals(configs = []) {
  configs.forEach((cfg) => {
    if (cfg.cancelBtnId) bindModalCancel(cfg.modalId, cfg.cancelBtnId);
    bindModalBackdropClose(cfg.modalId);
  });
  bindModalEscClose();
}

/* ============================================
   確認對話框
   ============================================ */

let _confirmResolve = null;

export function openConfirm(message, options = {}) {
  const {
    title = '確認',
    okText = '確定',
    cancelText = '取消',
    okClass = 'btn-danger',
  } = options;

  /* 🆕 v103.0.23（B31-1）：
     若已有開啟中的 confirm，先 resolve 舊 Promise（false）
     避免舊呼叫端 await 永遠卡住 */
  const oldOverlay = document.getElementById('__confirm-modal');
  if (oldOverlay) {
    if (oldOverlay._cleanup) {
      try { oldOverlay._cleanup(); } catch (e) { /* noop */ }
    }
    oldOverlay.remove();
    document.body.style.overflow = '';
    if (_confirmResolve) {
      try { _confirmResolve(false); } catch (e) { /* noop */ }
      _confirmResolve = null;
    }
  }

  return new Promise((resolve) => {
    _confirmResolve = resolve;

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay active';
    overlay.id = '__confirm-modal';
    overlay.innerHTML = `
      <div class="modal" style="max-width:400px;">
        <h2 class="modal-title">${esc(title)}</h2>
        <div style="font-size:14px; line-height:1.6; color:var(--text-secondary); white-space:pre-wrap;">${esc(message)}</div>
        <div class="modal-actions">
          <button type="button" class="btn btn-ghost" data-confirm-action="cancel">${esc(cancelText)}</button>
          <button type="button" class="btn ${okClass}" data-confirm-action="ok">${esc(okText)}</button>
        </div>
      </div>
    `;

    document.body.appendChild(overlay);
    document.body.style.overflow = 'hidden';

    const handleClick = (e) => {
      const btn = e.target.closest('[data-confirm-action]');
      if (!btn) {
        if (e.target === overlay) _closeConfirm(false);
        return;
      }
      _closeConfirm(btn.dataset.confirmAction === 'ok');
    };

    const handleKey = (e) => {
      if (e.key === 'Escape') _closeConfirm(false);
      if (e.key === 'Enter') _closeConfirm(true);
    };

    overlay.addEventListener('click', handleClick);
    document.addEventListener('keydown', handleKey, { once: false });

    overlay._cleanup = () => {
      overlay.removeEventListener('click', handleClick);
      document.removeEventListener('keydown', handleKey);
    };
  });
}

function _closeConfirm(result) {
  const overlay = document.getElementById('__confirm-modal');
  if (overlay) {
    if (overlay._cleanup) overlay._cleanup();
    overlay.remove();
  }
  document.body.style.overflow = '';
  if (_confirmResolve) {
    _confirmResolve(result);
    _confirmResolve = null;
  }
}

/* ============================================
   🆕 v103.0.24（B34-4）：輕量輸入框（openPrompt）
   ============================================
   用途：簡單實體新增（只輸名稱）
   回傳：使用者輸入的字串（取消 → null）
   ============================================ */

let _promptResolve = null;

export function openPrompt(options = {}) {
  const {
    title = '請輸入',
    label = '',
    placeholder = '',
    defaultValue = '',
    okText = '確定',
    cancelText = '取消',
    maxlength = 60,
  } = options;

  /* 開新前先 resolve 舊 Promise（null） */
  const oldOverlay = document.getElementById('__prompt-modal');
  if (oldOverlay) {
    if (oldOverlay._cleanup) {
      try { oldOverlay._cleanup(); } catch (e) { /* noop */ }
    }
    oldOverlay.remove();
    document.body.style.overflow = '';
    if (_promptResolve) {
      try { _promptResolve(null); } catch (e) { /* noop */ }
      _promptResolve = null;
    }
  }

  return new Promise((resolve) => {
    _promptResolve = resolve;

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay active';
    overlay.id = '__prompt-modal';
    overlay.innerHTML = `
      <div class="modal" style="max-width:400px;">
        <h2 class="modal-title">${esc(title)}</h2>
        ${label ? `<label class="field-label" style="display:block; margin-bottom:6px;">${esc(label)}</label>` : ''}
        <input type="text" class="input" id="__prompt-input"
               placeholder="${esc(placeholder)}" value="${esc(defaultValue)}"
               maxlength="${maxlength}" style="width:100%; margin-bottom:16px;">
        <div class="modal-actions">
          <button type="button" class="btn btn-ghost" data-prompt-action="cancel">${esc(cancelText)}</button>
          <button type="button" class="btn btn-primary" data-prompt-action="ok">${esc(okText)}</button>
        </div>
      </div>
    `;

    document.body.appendChild(overlay);
    document.body.style.overflow = 'hidden';

    const input = overlay.querySelector('#__prompt-input');

    const handleClick = (e) => {
      const btn = e.target.closest('[data-prompt-action]');
      if (!btn) {
        if (e.target === overlay) _closePrompt(null);
        return;
      }
      if (btn.dataset.promptAction === 'cancel') {
        _closePrompt(null);
      } else {
        const v = (input.value || '').trim();
        if (!v) { input.focus(); return; }
        _closePrompt(v);
      }
    };

    const handleKey = (e) => {
      if (e.key === 'Escape') _closePrompt(null);
      if (e.key === 'Enter') {
        const v = (input.value || '').trim();
        if (!v) { input.focus(); return; }
        _closePrompt(v);
      }
    };

    overlay.addEventListener('click', handleClick);
    document.addEventListener('keydown', handleKey, { once: false });

    overlay._cleanup = () => {
      overlay.removeEventListener('click', handleClick);
      document.removeEventListener('keydown', handleKey);
    };

    setTimeout(() => {
      input.focus();
      if (defaultValue) input.select();
    }, 50);
  });
}

function _closePrompt(result) {
  const overlay = document.getElementById('__prompt-modal');
  if (overlay) {
    if (overlay._cleanup) overlay._cleanup();
    overlay.remove();
  }
  document.body.style.overflow = '';
  if (_promptResolve) {
    _promptResolve(result);
    _promptResolve = null;
  }
}

/* ============================================
   銷毀
   ============================================ */

export function destroyModal(target) {
  const modal = _resolveElement(target);
  if (!modal) return;
  closeModal(modal);
  modal.remove();
}

export function cleanAllModals() {
  closeAllModals();
  _stack.length = 0;
}

/* ============================================
   內部工具
   ============================================ */
function _resolveElement(target) {
  if (typeof target === 'string') return document.getElementById(target);
  if (target instanceof HTMLElement) return target;
  return null;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/ui/modal.js
   Version: 1791901420
   Batch: B34-4
   Writer: A20261013-004#093
   ═══════════════════════════════════════════ */```

---

## 📄 js/ui/tab-panel.js

```
// ============================================
// tab-panel.js — Tab 切換元件（v103.0.11）
// 位置：js/ui/tab-panel.js
// ============================================
// v103.0.11 修正：
//   ✅ [M05] localStorage key 前綴改用 STORAGE_PREFIXES.UI
// ============================================

import { esc } from '../lib/dom.js';
import { STORAGE_PREFIXES } from '../config/constants.js';

/* ============================================
   主函式
   ============================================ */
export function initTabPanel(options) {
  const {
    containerId,
    tabs = [],
    defaultKey,
    storageKey,
    wrap = false,
    onChange,
  } = options;

  const root = document.getElementById(containerId);
  if (!root) {
    console.warn(`⚠️ initTabPanel: 找不到容器 #${containerId}`);
    return null;
  }

  if (tabs.length === 0) {
    console.warn('⚠️ initTabPanel: tabs 陣列為空');
    return null;
  }

  const normalizedTabs = tabs.map((t) => ({
    ...t,
    panelId: t.panelId || `${containerId}-panel-${t.key}`,
  }));

  const initialKey = _resolveInitialKey(normalizedTabs, defaultKey, storageKey);

  const wrapClass = wrap ? ' tab-bar-wrap' : '';
  root.innerHTML = `
    <div class="tab-bar${wrapClass}">
      ${normalizedTabs.map((t) => _renderTabBtn(t, t.key === initialKey)).join('')}
    </div>
  `;

  const clickHandler = (e) => {
    const btn = e.target.closest('.tab-btn');
    if (!btn) return;
    const key = btn.dataset.tabKey;
    if (key) switchTo(key);
  };
  root.addEventListener('click', clickHandler);

  if (window.lucide) window.lucide.createIcons();

  let _currentKey = initialKey;

  function switchTo(key) {
    if (!normalizedTabs.some((t) => t.key === key)) return;
    if (key === _currentKey) return;

    _currentKey = key;

    root.querySelectorAll('.tab-btn').forEach((btn) => {
      btn.classList.toggle('active', btn.dataset.tabKey === key);
    });

    normalizedTabs.forEach((t) => {
      const panel = document.getElementById(t.panelId);
      if (!panel) return;
      panel.classList.toggle('active', t.key === key);
      panel.style.display = t.key === key ? 'block' : 'none';
    });

    if (storageKey) {
      try {
        localStorage.setItem(_fullKey(storageKey), key);
      } catch (e) { /* noop */ }
    }

    if (typeof onChange === 'function') {
      const tab = normalizedTabs.find((t) => t.key === key);
      try {
        onChange(key, tab);
      } catch (err) {
        console.error('[tab-panel] onChange error:', err);
      }
    }

    if (window.lucide) window.lucide.createIcons();
  }

  _applyInitialState(normalizedTabs, initialKey);

  if (typeof onChange === 'function') {
    const tab = normalizedTabs.find((t) => t.key === initialKey);
    try {
      onChange(initialKey, tab);
    } catch (err) {
      console.error('[tab-panel] initial onChange error:', err);
    }
  }

  return {
    root,
    tabs: normalizedTabs,
    getCurrent: () => _currentKey,
    switchTo,
    refresh: () => {
      const tab = normalizedTabs.find((t) => t.key === _currentKey);
      if (typeof onChange === 'function') onChange(_currentKey, tab);
    },
    destroy: () => {
      root.removeEventListener('click', clickHandler);
    },
  };
}

/* ============================================
   內部函式
   ============================================ */

function _renderTabBtn(tab, isActive) {
  const iconHtml = tab.icon
    ? `<i data-lucide="${esc(tab.icon)}" style="width:14px;height:14px;"></i>`
    : '';
  return `
    <button type="button" class="tab-btn ${isActive ? 'active' : ''}" data-tab-key="${esc(tab.key)}">
      ${iconHtml}
      <span>${esc(tab.label)}</span>
    </button>
  `;
}

/**
 * 🆕 v103.0.11：統一使用 STORAGE_PREFIXES.UI
 */
function _fullKey(storageKey) {
  if (!storageKey) return `${STORAGE_PREFIXES.UI}tab-default`;
  return storageKey.startsWith(STORAGE_PREFIXES.UI)
    ? storageKey
    : `${STORAGE_PREFIXES.UI}${storageKey}`;
}

function _resolveInitialKey(tabs, defaultKey, storageKey) {
  if (storageKey) {
    try {
      const saved = localStorage.getItem(_fullKey(storageKey));
      if (saved && tabs.some((t) => t.key === saved)) return saved;
    } catch (e) { /* noop */ }
  }
  if (defaultKey && tabs.some((t) => t.key === defaultKey)) return defaultKey;
  return tabs[0].key;
}

function _applyInitialState(tabs, activeKey) {
  tabs.forEach((t) => {
    const panel = document.getElementById(t.panelId);
    if (!panel) return;
    const isActive = t.key === activeKey;
    panel.classList.toggle('active', isActive);
    panel.style.display = isActive ? 'block' : 'none';
  });
}```

---

## 📄 js/ui/toast.js

```
// ============================================
// toast.js — Toast 提示（v103.0.22）
// 位置：js/ui/toast.js
// ============================================
// v103.0.22（B25-5）：
//   ✅ M5：MAX_TOASTS 超出時清 timer，避免 timer 洩漏
//   ✅ 補 END OF FILE
// ============================================

import { getUIConstants } from '../config/app-config.js';
import { LIMITS } from '../config/constants.js';

const COLORS = {
  success: 'rgba(16, 185, 129, 0.95)',
  error:   'rgba(244, 63, 94, 0.95)',
  info:    'rgba(0, 240, 255, 0.95)',
  warning: 'rgba(251, 146, 60, 0.95)',
};

const CONTAINER_ID = 'app-toast-container';
const MAX_TOASTS = 3;

/* 🆕 v103.0.22：每個 toast 記錄 timer，方便清理 */
const _timers = new WeakMap();

export function showToast(msg, type = 'success', duration) {
  if (duration == null) {
    try {
      const cfg = getUIConstants();
      duration = cfg.toastDuration;
    } catch (e) {
      duration = LIMITS.TOAST_DURATION_DEFAULT;
    }
  }

  const container = _getOrCreateContainer();

  /* 🆕 v103.0.22：超出上限時，先清 timer 再移除 */
  while (container.children.length >= MAX_TOASTS) {
    const first = container.firstChild;
    if (!first) break;
    _clearToastTimer(first);
    container.removeChild(first);
  }

  const toast = document.createElement('div');
  toast.className = 'app-toast';
  toast.style.cssText = `
    background: ${COLORS[type] || COLORS.success};
    color: #fff;
    padding: 12px 22px;
    border-radius: 8px;
    font-size: 14px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
    opacity: 0;
    transform: translateY(10px);
    transition: opacity 0.3s ease, transform 0.3s ease;
    max-width: 90%;
    text-align: center;
    pointer-events: none;
    margin-top: 8px;
  `;
  toast.textContent = msg;

  container.appendChild(toast);

  requestAnimationFrame(() => {
    toast.style.opacity = '1';
    toast.style.transform = 'translateY(0)';
  });

  /* 🆕 v103.0.22：記錄 timer */
  const outerTimer = setTimeout(() => {
    toast.style.opacity = '0';
    toast.style.transform = 'translateY(10px)';
    const innerTimer = setTimeout(() => {
      if (toast.parentNode) toast.parentNode.removeChild(toast);
      _timers.delete(toast);
    }, 300);
    _timers.set(toast, innerTimer);
  }, duration);

  _timers.set(toast, outerTimer);
}

export const toastSuccess = (msg, duration) => showToast(msg, 'success', duration);
export const toastError   = (msg, duration) => showToast(msg, 'error', duration);
export const toastInfo    = (msg, duration) => showToast(msg, 'info', duration);
export const toastWarning = (msg, duration) => showToast(msg, 'warning', duration);

/* ============================================
   內部工具
   ============================================ */

/**
 * 🆕 v103.0.22：清除單一 toast 的 timer
 * 注意：outerTimer 或 innerTimer 二擇一存於 WeakMap
 */
function _clearToastTimer(toast) {
  const t = _timers.get(toast);
  if (t) {
    clearTimeout(t);
    _timers.delete(toast);
  }
}

function _getOrCreateContainer() {
  let container = document.getElementById(CONTAINER_ID);
  if (container) return container;

  container = document.createElement('div');
  container.id = CONTAINER_ID;
  container.style.cssText = `
    position: fixed;
    bottom: 30px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 99999;
    display: flex;
    flex-direction: column;
    align-items: center;
    pointer-events: none;
    max-width: 90%;
  `;
  document.body.appendChild(container);
  return container;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/ui/toast.js
   Version: v103.0.22
   Batch: B25-5
   ═══════════════════════════════════════════ */```

---

## 📄 js/ui/view-toggle.js

```
// ============================================
// view-toggle.js — 卡片 / 表格切換（v103.0.22）
// 位置：js/ui/view-toggle.js
// ============================================
// v103.0.22（B25-5）：
//   ✅ R5：加 syncAppState 選項（預設 true）
//      - false 時不同步到 AppState（適用非標準 view，如 annual-report 的 summary/monthly）
// v103.0.19 修正：
//   ✅ 加 cardValue / tableValue 參數
// ============================================

import { esc } from '../lib/dom.js';
import { AppState } from '../core/state.js';
import { STORAGE_PREFIXES } from '../config/constants.js';

export function initViewToggle(options) {
  const {
    containerId,
    storageKey,
    defaultView = 'card',
    cardText = '卡片模式',
    tableText = '表格模式',
    cardValue = 'card',
    tableValue = 'table',
    onChange,
    autoApply = true,
    /* 🆕 v103.0.22：是否同步到 AppState（預設 true） */
    syncAppState = true,
  } = options;

  const root = document.getElementById(containerId);
  if (!root) {
    console.warn(`⚠️ initViewToggle: 找不到容器 #${containerId}`);
    return null;
  }

  const storageFullKey = _fullKey(storageKey);
  const validValues = [cardValue, tableValue];
  let _currentView = _loadView(storageFullKey, defaultView, validValues);

  /* 🆕 v103.0.22：只在 syncAppState + 標準 view 時才同步 */
  if (syncAppState && (_currentView === 'card' || _currentView === 'table')) {
    AppState.setCurrentView(_currentView);
  }

  root.innerHTML = `
    <div class="view-toggle-group">
      <button type="button" class="btn btn-sm ${_currentView === cardValue ? 'btn-primary' : 'btn-ghost'}" data-view="${esc(cardValue)}">${esc(cardText)}</button>
      <button type="button" class="btn btn-sm ${_currentView === tableValue ? 'btn-primary' : 'btn-ghost'}" data-view="${esc(tableValue)}">${esc(tableText)}</button>
    </div>
  `;

  if (autoApply) _applyIfStandard(_currentView);

  const clickHandler = (e) => {
    const btn = e.target.closest('button[data-view]');
    if (!btn) return;
    setView(btn.dataset.view);
  };
  root.addEventListener('click', clickHandler);

  if (window.lucide) window.lucide.createIcons();

  function setView(view) {
    if (!validValues.includes(view)) return;
    if (view === _currentView) return;

    _currentView = view;

    root.querySelectorAll('button[data-view]').forEach((btn) => {
      const isActive = btn.dataset.view === view;
      btn.classList.toggle('btn-primary', isActive);
      btn.classList.toggle('btn-ghost', !isActive);
    });

    try { localStorage.setItem(storageFullKey, view); } catch (e) { /* noop */ }

    if (autoApply) _applyIfStandard(view);

    /* 🆕 v103.0.22：只在 syncAppState + 標準 view 時才同步 */
    if (syncAppState && (view === 'card' || view === 'table')) {
      AppState.setCurrentView(view);
    }

    if (typeof onChange === 'function') {
      try { onChange(view); } catch (err) { console.error('[view-toggle] onChange error:', err); }
    }
  }

  return {
    root,
    getView: () => _currentView,
    setView,
    refresh: () => {
      if (autoApply) _applyIfStandard(_currentView);
      if (typeof onChange === 'function') onChange(_currentView);
    },
    destroy: () => {
      root.removeEventListener('click', clickHandler);
    },
  };
}

function _applyIfStandard(view) {
  if (view === 'card' || view === 'table') applyViewToDom(view);
}

export function buildToggleHTML(config = {}) {
  const {
    cardText = '卡片模式',
    tableText = '表格模式',
    cardValue = 'card',
    tableValue = 'table',
    currentView = cardValue,
  } = config;
  return `
    <div class="view-toggle-group">
      <button type="button" class="btn btn-sm ${currentView === cardValue ? 'btn-primary' : 'btn-ghost'}" data-view="${esc(cardValue)}">${esc(cardText)}</button>
      <button type="button" class="btn btn-sm ${currentView === tableValue ? 'btn-primary' : 'btn-ghost'}" data-view="${esc(tableValue)}">${esc(tableText)}</button>
    </div>
  `;
}

export function getSavedView(storageKey, defaultView = 'card') {
  const valid = ['card', 'table'];
  return _loadView(_fullKey(storageKey), defaultView, valid);
}

export function saveView(storageKey, view) {
  try { localStorage.setItem(_fullKey(storageKey), view); } catch (e) { /* noop */ }
}

export function applyViewToDom(view) {
  document.body.classList.toggle('view-card-mode', view === 'card');
  document.body.classList.toggle('view-table-mode', view === 'table');

  document.querySelectorAll('[data-view-card-only]').forEach((el) => {
    el.style.display = view === 'card' ? '' : 'none';
  });
  document.querySelectorAll('[data-view-table-only]').forEach((el) => {
    el.style.display = view === 'table' ? '' : 'none';
  });
}

function _fullKey(storageKey) {
  if (!storageKey) return `${STORAGE_PREFIXES.VIEW}default`;
  return storageKey.startsWith(STORAGE_PREFIXES.VIEW)
    ? storageKey
    : `${STORAGE_PREFIXES.VIEW}${storageKey}`;
}

function _loadView(fullKey, defaultValue, validValues) {
  try {
    const saved = localStorage.getItem(fullKey);
    if (validValues.includes(saved)) return saved;
  } catch (e) { /* noop */ }
  return defaultValue;
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: js/ui/view-toggle.js
   Version: v103.0.22
   Batch: B25-5
   ═══════════════════════════════════════════ */```

---

## 📄 login.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>登入 | 家庭財務</title>
  <link rel="preconnect" href="https://www.gstatic.com" crossorigin>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="dns-prefetch" href="https://family-fin-a6dd1-default-rtdb.asia-southeast1.firebasedatabase.app">
  <link rel="dns-prefetch" href="https://identitytoolkit.googleapis.com">
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body class="auth-page">
  <div class="glass-card auth-card">
    <h1 class="auth-title">◈ FAMILY.FIN</h1>
    <p class="auth-subtitle">家庭財務與固定供款管理</p>

    <form id="login-form">
      <div class="field">
        <label class="field-label" for="account">帳號</label>
        <input class="input" type="text" id="account" required placeholder="例如：superadmin" autocomplete="username">
      </div>
      <div class="field">
        <label class="field-label" for="password">密碼</label>
        <input class="input" type="password" id="password" required minlength="6" placeholder="至少 6 位" autocomplete="current-password">
      </div>

      <div id="login-error" class="banner banner-magenta" style="display:none;"></div>

      <button class="btn btn-primary" type="submit" id="login-btn" style="width:100%; justify-content:center;">登入</button>
    </form>

    <p style="margin-top:16px; font-size:13px; color:var(--text-muted); text-align:center;">
      沒有帳號？請聯繫平台管理員開通
    </p>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <script type="module">
    import { loginWithCustomAccount, isSuperAdmin } from './js/core/auth.js';
    import { AppState } from './js/core/state.js';
    import { initPWA } from './js/core/pwa.js';
    import { SESSION_KEYS } from './js/config/constants.js';

    initPWA();
    AppState.init();

    const form = document.getElementById('login-form');
    const errorBox = document.getElementById('login-error');
    const btn = document.getElementById('login-btn');

    form.addEventListener('submit', async (e) => {
      e.preventDefault();
      errorBox.style.display = 'none';
      btn.disabled = true;
      btn.textContent = '登入中…';

      const account = document.getElementById('account').value.trim();
      const password = document.getElementById('password').value;

      try {
        const user = await loginWithCustomAccount(account, password);
        AppState.setUser(user);
        AppState.setSuperAdmin(isSuperAdmin(user));

        /* 🆕 v103.0.12 [M14]：從 constants.js 讀取前綴（SSOT） */
        try {
          sessionStorage.removeItem(`${SESSION_KEYS.AUTH_CONTEXT_PREFIX}${user.uid}`);
        } catch (err) { /* noop */ }

        window.location.href = isSuperAdmin(user) ? 'admin.html' : 'index.html';
      } catch (err) {
        console.error(err);
        errorBox.textContent = '登入失敗：帳號或密碼錯誤';
        errorBox.style.display = 'block';
        btn.disabled = false;
        btn.textContent = '登入';
      }
    });
  </script>
</body>
</html>```

---

## 📄 manifest.json

```
{
  "name": "家庭財務與固定供款管理",
  "short_name": "FAMILY.FIN",
  "description": "全家財務、保險供款、基金資產、銀行交易即時管理",
  "start_url": "./index.html",
  "scope": "./",
  "display": "standalone",
  "orientation": "portrait",
  "background_color": "#080B11",
  "theme_color": "#080B11",
  "lang": "zh-Hant",
  "categories": ["finance", "productivity"],
  "icons": [
    {
      "src": "./icons/icon.svg",
      "sizes": "any",
      "type": "image/svg+xml",
      "purpose": "any maskable"
    }
  ]
}
```

---

## 📄 member-report.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>成員與家庭收入與支出明細 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">成員與家庭收入與支出明細</h1>
            <p class="page-subtitle" id="member-report-subtitle">載入中…</p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <div id="member-report-stats-root"></div>
        <div id="page-controls-root"></div>

        <div class="glass-card collapsible-card collapsible-card-flat" style="padding:0; margin-bottom:20px;">
          <div class="collapsible-header">
            <div class="collapsible-header-title">
              <i data-lucide="users" style="width:16px;height:16px;"></i>
              <span>成員清單</span>
            </div>
          </div>
          <div class="collapsible-body" style="display:block;">
            <div id="member-list-root"></div>
          </div>
        </div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/member-report.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/member-report.js';

    const user = await initApp({ activeHref: 'member-report.html', title: '成員與家庭收入與支出明細' });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: member-report.html
     Version: v103.0.29
     Batch: B26-3.2
     ═══════════════════════════════════════════ -->```

---

## 📄 portfolio.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>基金投資表 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">基金投資表</h1>
            <p class="page-subtitle">基金持倉與最新現值</p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <div id="portfolio-stats-root"></div>
        <div id="page-controls-root"></div>

        <div style="margin-bottom:20px;">
          <div class="flex items-center gap-8 mb-12">
            <i data-lucide="shield" style="width:16px;height:16px;color:var(--neon-magenta);"></i>
            <span style="font-weight:700;color:var(--neon-magenta);font-size:14px;">保險基金</span>
            <span class="text-muted" style="font-size:12px;">（掛於基金保險保單，每月自動累積供款）</span>
          </div>
          <div id="insurance-funds-list"></div>
        </div>

        <div style="margin-bottom:20px;">
          <div class="flex items-center gap-8 mb-12">
            <i data-lucide="line-chart" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
            <span style="font-weight:700;color:var(--neon-cyan);font-size:14px;">獨立基金</span>
            <span class="text-muted" style="font-size:12px;">（獨立購買，手動記錄）</span>
          </div>
          <div id="standalone-funds-list"></div>

          <div class="glass-card" id="fund-empty-state" style="display:none;">
            <div class="empty-state">
              <i data-lucide="line-chart" style="width:48px;height:48px;opacity:0.4;"></i>
              <p style="margin-top:12px;">尚無基金持倉</p>
              <button type="button" class="btn btn-primary" id="add-first-fund-btn" style="margin-top:12px;">
                <i data-lucide="plus"></i> 新增第一筆基金
              </button>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/portfolio.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/portfolio.js';

    const user = await initApp({ activeHref: 'portfolio.html', title: '基金投資表' });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: portfolio.html
     Version: v103.0.29
     Batch: B26-3.2
     ═══════════════════════════════════════════ -->```

---

## 📄 register.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>註冊 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body class="auth-page">
  <div class="glass-card auth-card" style="text-align:center;">
    <h1 class="auth-title">◈ FAMILY.FIN</h1>
    <p class="auth-subtitle">系統已關閉公開註冊</p>
    <div class="banner banner-magenta" style="margin:20px 0; text-align:left;">
      🔒 本系統為家庭專用。<br>
      請聯繫<b>超級管理員</b>（superadmin）為您開通帳號。
    </div>
    <a href="login.html" class="btn btn-primary" style="justify-content:center;">返回登入頁</a>
  </div>

  <script type="module">
    import { initPWA } from './js/core/pwa.js';
    initPWA();
  </script>
</body>
</html>
```

---

## 📄 settings.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>系統設定 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="page-header">
          <div>
            <h1 class="page-title">系統設定</h1>
            <p class="page-subtitle"></p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <div id="settings-tabs"></div>

        <div id="settings-panel-platform" class="tab-panel" style="display:none;">
          <div class="banner mb-16">
            ℹ️ 平台 UI 常數為全站預設值，家庭可在「基礎資料庫 → 下拉選項」中覆蓋。
          </div>
          <div class="glass-card" style="max-width:640px;">
            <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:16px;">
              <i data-lucide="settings" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
              <span>UI 常數</span>
            </div>
            <div id="settings-platform-form-root"></div>
          </div>
          <div class="banner mt-16" style="max-width:640px;">
            💡 平台「預設資料庫」的編輯請至 <a href="admin.html" style="color:var(--neon-cyan);">平台管理</a> 頁面。
          </div>
        </div>

        <div id="settings-panel-banks" class="tab-panel" style="display:none;">
          <div class="banner mb-16">
            ℹ️ 管理家庭的銀行帳號。每個帳號有「初始餘額」與「初始年月」，之後的餘額由交易自動計算。
          </div>
          <div id="bank-account-manager-root"></div>
        </div>

        <div id="settings-panel-personal" class="tab-panel" style="display:none;">
          <div class="settings-personal-grid">
            <div class="glass-card">
              <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:16px;">
                <i data-lucide="user" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
                <span>帳號資訊</span>
              </div>
              <div class="form-stack">
                <div class="field"><label class="field-label">登入帳號</label><input class="input mono" id="settings-account" disabled></div>
                <div class="field"><label class="field-label">角色</label><input class="input mono" id="settings-role" disabled></div>
                <div class="field"><label class="field-label">家庭名稱</label><input class="input" id="settings-family" disabled></div>
              </div>
              <div style="margin-top:16px; padding-top:16px; border-top:1px dashed rgba(255,255,255,0.08); text-align:right;">
                <button type="button" class="btn btn-danger" id="settings-logout-btn"><i data-lucide="log-out"></i> 登出</button>
              </div>
            </div>

            <div class="glass-card">
              <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:16px;">
                <i data-lucide="bar-chart-2" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
                <span>統計卡顯示模式</span>
              </div>
              <p class="glass-card-hint mb-16">調整頁面頂部統計卡的顯示樣式</p>
              <div id="stats-mode-options" style="display:flex; flex-direction:column; gap:8px;"></div>
              <div style="margin-top:16px; padding-top:16px; border-top:1px dashed rgba(255,255,255,0.08); text-align:right;">
                <button type="button" class="btn btn-primary" id="save-stats-mode-btn"><i data-lucide="check" style="width:14px;height:14px;"></i> 儲存顯示模式</button>
              </div>
            </div>

            <div class="glass-card" style="grid-column: 1 / -1;">
              <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:16px;">
                <i data-lucide="users" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
                <span>家庭成員</span>
              </div>
              <div id="family-members-root"></div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/settings.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/settings.js';

    const user = await initApp({ activeHref: 'settings.html', title: '系統設定', requireFamily: false });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: settings.html
     Version: v103.0.37
     Batch: B30-1
     ═══════════════════════════════════════════ -->```

---

## 📄 settlements.html

```
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>結算清單 | 家庭財務</title>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/layout.css">
  <link rel="stylesheet" href="./css/components.css">
  <link rel="stylesheet" href="./css/utilities.css">
</head>
<body>
  <div class="app-shell">
    <aside id="sidebar-root"></aside>
    <div class="main-area">
      <header id="navbar-root"></header>
      <main class="content content-min-height">
        <div class="banner mb-16">
          ℹ️ 整合「個人支出 / 家庭共用支出 / 保險扣款」三大來源。
        </div>

        <div class="page-header">
          <div>
            <h1 class="page-title">結算清單</h1>
            <p class="page-subtitle" id="settlement-month"></p>
          </div>
          <div id="page-header-actions"></div>
        </div>

        <div id="settlement-stats-root"></div>
        <div id="page-controls-root"></div>

        <div id="settlement-table-root"></div>
      </main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <link rel="modulepreload" href="./js/layout/app-shell.js">
  <link rel="modulepreload" href="./js/engines/page-engine.js">
  <link rel="modulepreload" href="./js/pages/settlements.js">

  <script type="module">
    import { initApp } from './js/layout/app-shell.js';
    import { createPage } from './js/engines/page-engine.js';
    import schema from './js/pages/settlements.js';

    const user = await initApp({ activeHref: 'settlements.html', title: '結算清單' });
    if (user) {
      await createPage(schema);
    }
  </script>
</body>
</html>

<!-- ═══════════════════════════════════════════
     END OF FILE
     File: settlements.html
     Version: v103.0.34
     Batch: B29-4a
     ═══════════════════════════════════════════ -->```

---

## 📄 sw.js

```
// ============================================
// sw.js — Service Worker（v103.0.32）
// 位置：sw.js
// ============================================
// v103.0.32（B35-3）：
//   ✅ CACHE_NAME 升版 v155 → v156（配合 portfolio 快照修正 + 導入功能）
// v103.0.31（B35-0）：
//   ✅ CACHE_NAME 升版 v154 → v155（配合 input-center 入口調整）
// v103.0.30（B34-4）：
//   ✅ 加 js/ui/dropdown-plus.js
//   ✅ CACHE_NAME 升版 v153 → v154
// v103.0.29（B34-4）：
//   ✅ 加 yearrange-card.js；移除 4 個 database-*.js
// ============================================

const CACHE_NAME = 'family-fin-v156';

const STATIC_ASSETS = [
  './css/theme.css',
  './css/layout.css',
  './css/components.css',
  './css/utilities.css',
  './css/pages/annual-report.css',
  './css/pages/insurance.css',

  './js/config/constants.js',
  './js/config/firebase-config.js',
  './js/config/app-config.js',
  './js/config/status-registry.js',
  './js/config/entity-registry.js',
  './js/config/label-registry.js',
  './js/config/column-registry.js',
  './js/config/page-metadata-registry.js',

  './js/core/state.js',
  './js/core/auth.js',
  './js/core/auth-guard.js',
  './js/core/api.js',
  './js/core/db.js',
  './js/core/utils.js',
  './js/core/pwa.js',
  './js/core/debug.js',

  './js/lib/dom.js',
  './js/lib/async.js',
  './js/lib/lifecycle.js',
  './js/lib/registry.js',
  './js/lib/merge.js',
  './js/lib/insurance.js',
  './js/lib/bank.js',
  './js/lib/format.js',
  './js/lib/expense-modal.js',
  './js/lib/income-modal.js',
  './js/lib/filter-sort.js',
  './js/lib/page-controls.js',
  './js/lib/code-registry.js',
  './js/lib/id-resolver.js',

  './js/engines/data-engine.js',
  './js/engines/render-engine.js',
  './js/engines/page-engine.js',

  './js/blocks/stats-block.js',
  './js/blocks/list-block.js',
  './js/blocks/filter-block.js',
  './js/blocks/detail-block.js',
  './js/blocks/form-block.js',

  './js/ui/toast.js',
  './js/ui/modal.js',
  './js/ui/form-builder.js',
  './js/ui/tab-panel.js',
  './js/ui/view-toggle.js',
  './js/ui/collapsible.js',
  './js/ui/dropdown-plus.js',

  './js/layout/navbar.js',
  './js/layout/sidebar.js',
  './js/layout/app-shell.js',

  './js/entity/entity-definitions.js',
  './js/entity/entity-helpers.js',
  './js/entity/entity-modal.js',
  './js/entity/entity-resolvers.js',
  './js/entity/entity-funds-allocation.js',

  './js/shared/bank-account-manager.js',
  './js/shared/column-settings.js',
  './js/shared/data-card.js',
  './js/shared/data-table.js',
  './js/shared/date-helpers.js',
  './js/shared/entity-list-page.js',
  './js/shared/form-handler.js',
  './js/shared/listener-group.js',
  './js/shared/page-filter.js',
  './js/shared/quick-summary.js',
  './js/shared/select-helpers.js',
  './js/shared/stats-cards.js',
  './js/shared/yearrange-card.js',

  './js/pages/dashboard.js',
  './js/pages/portfolio.js',
  './js/pages/annual-report.js',
  './js/pages/settings.js',
  './js/pages/finance-overview.js',
  './js/pages/member-report.js',
  './js/pages/input-center.js',
  './js/pages/code-list.js',
  './js/pages/settlements.js',
  './js/pages/insurance.js',

  './js/admin/admin.js',
  './js/admin/platform-defaults.js',

  './manifest.json',
  './icons/icon.svg',
];

self.addEventListener('install', (e) => {
  console.log('[SW] install (v103.0.32, cache=' + CACHE_NAME + ')');
  e.waitUntil(
    caches.open(CACHE_NAME).then((cache) =>
      Promise.all(
        STATIC_ASSETS.map((url) =>
          cache.add(url).catch((err) => console.warn('[SW] 快取失敗：', url, err.message))
        )
      )
    ).then(() => {
      console.log('[SW] install 完成，skipWaiting');
      return self.skipWaiting();
    })
  );
});

self.addEventListener('activate', (e) => {
  console.log('[SW] activate');
  e.waitUntil(
    caches.keys().then((keys) =>
      Promise.all(
        keys
          .filter((k) => k !== CACHE_NAME)
          .map((k) => {
            console.log('[SW] 清除舊快取：', k);
            return caches.delete(k);
          })
      )
    ).then(() => {
      console.log('[SW] activate 完成，clients.claim');
      return self.clients.claim();
    })
  );
});

self.addEventListener('fetch', (e) => {
  if (e.request.method !== 'GET') return;

  const url = new URL(e.request.url);
  if (url.origin !== self.location.origin) return;
  if (url.pathname.startsWith('/api/')) return;

  e.respondWith(_networkFirst(e.request));
});

async function _networkFirst(request) {
  const url = new URL(request.url);
  const isHtml = request.mode === 'navigate'
    || url.pathname.endsWith('.html')
    || url.pathname === '/'
    || url.pathname.endsWith('/');

  try {
    const res = await fetch(request);

    if (res && res.status === 200 && res.type === 'basic') {
      const cache = await caches.open(CACHE_NAME);
      cache.put(request, res.clone());
    }
    return res;
  } catch (err) {
    console.log('[SW] 網路失敗，嘗試快取：', url.pathname);
    const cached = await caches.match(request);
    if (cached) return cached;

    if (isHtml) {
      const fallback = await caches.match('./index.html');
      if (fallback) return fallback;
    }

    return new Response('離線中，請稍後再試', {
      status: 503,
      headers: { 'Content-Type': 'text/plain; charset=utf-8' },
    });
  }
}

/* ═══════════════════════════════════════════
   END OF FILE
   File: sw.js
   Version: 1792210800
   Batch: B35-3
   Writer: A20261016-001#006
   ═══════════════════════════════════════════ */```


/* ═══ EOF ═══ */
