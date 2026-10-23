> 生成時間：2026-10-23 18:36:33 (HKT)

# 專案全程式碼包：AI-WEB-APP-Family


<!-- === FILE START: ./app.css === -->

## 📄 ./app.css

```css
/* ============================================
   app.css — 家庭財務 SPA 全站樣式
   來源：合併 css/theme.css、layout.css、components.css、utilities.css
   原則：配色、類名沿用舊版，視覺 100% 一致
   ============================================ */

/* ================= 主題變數 ================= */
:root,
[data-theme="light"] {
  --bg-deep:  #E2E8F0;
  --bg-navy:  #CBD5E1;
  --bg-panel: #FFFFFF;
  --primary:      #1D4ED8;
  --primary-dark: #1E40AF;
  --accent-warm:  #F97316;
  --accent-fresh: #10B981;
  --accent-sky:   #3B82F6;
  --danger:       #DC2626;
  --danger-dark:  #B91C1C;
  --neon-cyan:    #3B82F6;
  --neon-magenta: #DC2626;
  --neon-emerald: #10B981;
  --neon-red:     #DC2626;
  --neon-orange:  #F97316;
  --glass-bg:           #FFFFFF;
  --glass-border:       #CBD5E1;
  --glass-border-hover: #94A3B8;
  --glass-blur:         0px;
  --input-bg:        #FFFFFF;
  --modal-bg:        #FFFFFF;
  --overlay-bg:      rgba(15, 23, 42, 0.5);
  --sidebar-bg:      #FFFFFF;
  --navbar-bg:       #FFFFFF;
  --filter-panel-bg: #FFFFFF;
  --hover-bg:        #F1F5F9;
  --hover-bg-strong: #E2E8F0;
  --divider:        #E2E8F0;
  --divider-strong: #CBD5E1;
  --shadow-sm: 0 1px 2px rgba(15, 23, 42, 0.08);
  --shadow-md: 0 4px 6px rgba(15, 23, 42, 0.10);
  --shadow-lg: 0 10px 15px rgba(15, 23, 42, 0.12);
  --text-primary:   #1E293B;
  --text-secondary: #475569;
  --text-muted:     #64748B;
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang TC", "Microsoft JhengHei", "Noto Sans TC", sans-serif;
  --font-mono: "JetBrains Mono", "SF Mono", "Consolas", "Menlo", monospace;
  --transition: 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

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

/* ================= 基礎 ================= */
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
a { color: var(--primary); text-decoration: none; }
a:hover { text-decoration: underline; }
button { font-family: inherit; cursor: pointer; }
.mono { font-family: var(--font-mono); }
.text-cyan { color: var(--accent-sky); }
.text-magenta { color: var(--danger); }
.text-emerald { color: var(--accent-fresh); }
.text-red { color: var(--danger); }
.text-orange { color: var(--accent-warm); }
.text-muted { color: var(--text-muted); }

::-webkit-scrollbar { width: 8px; height: 8px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: #CBD5E1; border-radius: 4px; }
::-webkit-scrollbar-thumb:hover { background: #94A3B8; }
[data-theme="dark"] ::-webkit-scrollbar-thumb { background: #334155; }

/* ================= 認證頁 ================= */
.auth-page {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 20px;
}
.auth-card { width: 100%; max-width: 400px; padding: 32px; }
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

/* ================= App Shell ================= */
.app-shell { display: flex; min-height: 100vh; position: relative; }

.sidebar {
  width: 260px;
  flex-shrink: 0;
  background: var(--sidebar-bg);
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
.nav-item:hover { background: var(--hover-bg); color: var(--primary); text-decoration: none; }
.nav-item.active { color: var(--primary); background: var(--hover-bg-strong); border-left-color: var(--primary); }
.nav-item .nav-icon { width: 18px; height: 18px; flex-shrink: 0; }
.nav-item .nav-label { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

.main-area { flex: 1; display: flex; flex-direction: column; min-width: 0; }

.navbar {
  height: 64px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 0 24px;
  background: var(--navbar-bg);
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
.navbar-right { display: flex; align-items: center; gap: 10px; margin-left: auto; }
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
.navbar-db-tag.new { background: var(--primary); color: #FFFFFF; }
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
  transition: all var(--transition);
}
.navbar-theme-btn:hover { border-color: var(--glass-border-hover); color: var(--primary); }
.navbar-user-text {
  font-size: 12px;
  color: var(--text-muted);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 280px;
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
  flex-shrink: 0;
}
.hamburger:hover { border-color: var(--glass-border-hover); }

.content {
  flex: 1;
  padding: 24px;
  max-width: 1400px;
  width: 100%;
  margin: 0 auto;
}
.content-min-height { min-height: calc(100vh - 64px - 48px); }

/* ================= 卡片 ================= */
.glass-card {
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-md);
  padding: 20px;
  transition: all var(--transition);
  box-shadow: var(--shadow-sm);
}
.glass-card:hover { border-color: var(--glass-border-hover); box-shadow: var(--shadow-md); }
.glass-card-title {
  font-size: 12px;
  color: var(--text-muted);
  letter-spacing: 1px;
  text-transform: uppercase;
  margin-bottom: 10px;
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
.btn:hover { background: var(--hover-bg); border-color: var(--glass-border-hover); box-shadow: var(--shadow-sm); }
.btn-primary { background: var(--primary); border-color: var(--primary); color: #FFFFFF; font-weight: 600; }
.btn-primary:hover { background: var(--primary-dark); border-color: var(--primary-dark); }
.btn-danger { background: var(--danger); border-color: var(--danger); color: #FFFFFF; font-weight: 600; }
.btn-danger:hover { background: var(--danger-dark); border-color: var(--danger-dark); }
.btn-magenta { background: var(--accent-warm); border-color: var(--accent-warm); color: #FFFFFF; font-weight: 600; }
.btn-ghost { background: transparent; border-color: transparent; color: var(--text-secondary); }
.btn-ghost:hover { background: var(--hover-bg); color: var(--primary); box-shadow: none; }
.btn-sm { padding: 6px 10px; font-size: 12px; }
.btn:disabled { opacity: 0.4; cursor: not-allowed; }

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

/* ================= 橫幅 / 空狀態 ================= */
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
.empty-state {
  text-align: center;
  padding: 40px 20px;
  color: var(--text-muted);
  font-size: 14px;
}

/* ================= 表格 ================= */
.data-table { width: 100%; border-collapse: collapse; font-size: 14px; }
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
.data-table td { padding: 12px 14px; border-bottom: 1px solid var(--divider); color: var(--text-primary); }
.data-table tr:hover td { background: var(--hover-bg); }
.data-table .num { font-family: var(--font-mono); text-align: right; }

/* ================= Badge ================= */
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

/* ================= Modal ================= */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: var(--overlay-bg);
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
}
.modal-title {
  font-size: 18px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 16px;
  padding-left: 12px;
  border-left: 4px solid var(--primary);
  line-height: 1.3;
}
.modal-actions { display: flex; gap: 10px; justify-content: flex-end; margin-top: 20px; }

/* ================= Tab ================= */
.tab-bar {
  display: flex;
  gap: 4px;
  padding: 4px;
  background: var(--divider);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-sm);
  overflow-x: auto;
  margin-bottom: 16px;
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
  white-space: nowrap;
  transition: all var(--transition);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
}
.tab-bar .tab-btn:hover { color: var(--primary); background: var(--hover-bg); }
.tab-bar .tab-btn.active { background: var(--primary); color: #FFFFFF; font-weight: 700; }
.tab-panel { display: none; }
.tab-panel.active { display: block; }

/* ================= 頁首 ================= */
.page-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-wrap: wrap;
  gap: 12px;
  margin-bottom: 24px;
}
.page-title { font-size: 24px; font-weight: 700; color: var(--primary); margin-bottom: 4px; }
.page-subtitle { font-size: 13px; color: var(--text-muted); }

/* ================= Toast ================= */
#app-toast-container {
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
}

/* ================= 工具類 ================= */
.flex { display: flex; }
.flex-between { display: flex; align-items: center; justify-content: space-between; }
.flex-center { display: flex; align-items: center; justify-content: center; }
.flex-wrap { flex-wrap: wrap; }
.items-center { align-items: center; }
.gap-4  { gap: 4px; }
.gap-8  { gap: 8px; }
.gap-12 { gap: 12px; }
.gap-16 { gap: 16px; }
.mt-8  { margin-top: 8px; }
.mt-12 { margin-top: 12px; }
.mt-16 { margin-top: 16px; }
.mb-8  { margin-bottom: 8px; }
.mb-12 { margin-bottom: 12px; }
.mb-16 { margin-bottom: 16px; }
.hidden { display: none !important; }

/* ================= 響應式 ================= */
@media (max-width: 640px) {
  .content { padding: 16px; }
  .content-min-height { min-height: calc(100vh - 64px - 32px); }

  .sidebar {
    position: fixed;
    top: 0;
    left: -260px;
    height: 100vh;
    width: 260px !important;
    z-index: 10001;
    transition: left 0.3s ease;
  }
  .sidebar.mobile-open { left: 0; }

  .sidebar-backdrop {
    display: none;
    position: fixed;
    top: 0;
    left: 260px;
    right: 0;
    bottom: 0;
    background: var(--overlay-bg);
    z-index: 10000;
  }
  .sidebar-backdrop.active { display: block; }

  .navbar { padding: 0 12px; gap: 8px; }
  .navbar-title { display: none; }
  .navbar-user-text { font-size: 11px; max-width: 100px; }
  .navbar-db-tag { font-size: 10px; padding: 3px 8px; max-width: 120px; overflow: hidden; text-overflow: ellipsis; }
  .navbar-theme-btn { width: 30px; height: 30px; }

  .data-table th, .data-table td { padding: 8px 6px; font-size: 12px; }
  .modal { padding: 16px; max-height: 85vh; overflow-y: auto; }
  .tab-bar .tab-btn { padding: 7px 10px; font-size: 12px; min-width: 80px; }
}

/* ================= V2 專屬顏色覆蓋 ================= */
.sidebar-logo { color: var(--accent-warm) !important; }
.auth-title { color: var(--accent-warm) !important; }

/* ═══ EOF ═══
F: app.css
V: 1792670004
W: webapp-20261022-041#4
═══ */
```

<!-- === FILE END: ./app.css === -->

<!-- === FILE START: ./app.html === -->

## 📄 ./app.html

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>家庭財務 | FAMILY.FIN V2</title>
  <link rel="preconnect" href="https://www.gstatic.com" crossorigin>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./app.css">
  <link rel="manifest" href="./manifest-app.json">
  <meta name="theme-color" content="#E2E8F0">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-title" content="FAMILY.FIN V2">
</head>
<body>
<div id="auth-view" class="auth-page" style="display:none;">
  <div class="glass-card auth-card">
    <h1 class="auth-title">◈ FAMILY.FIN V2</h1>
    <p class="auth-subtitle">家庭財務與固定供款管理</p>
    <form id="login-form">
      <div class="field"><label class="field-label" for="account">帳號</label><input class="input" type="text" id="account" required placeholder="例如：superadmin" autocomplete="username"></div>
      <div class="field"><label class="field-label" for="password">密碼</label><input class="input" type="password" id="password" required minlength="6" placeholder="至少 6 位" autocomplete="current-password"></div>
      <div id="login-error" class="banner banner-magenta" style="display:none;"></div>
      <button class="btn btn-primary" type="submit" id="login-btn" style="width:100%; justify-content:center;">登入</button>
    </form>
    <p style="margin-top:16px; font-size:13px; color:var(--text-muted); text-align:center;">沒有帳號？請聯繫平台管理員開通</p>
  </div>
</div>
<div id="loading-view" class="auth-page" style="display:none;">
  <div class="glass-card auth-card" style="text-align:center;">
    <h1 class="auth-title">◈ FAMILY.FIN V2</h1>
    <p class="auth-subtitle" id="loading-message">載入中…</p>
  </div>
</div>
<div id="app-view" class="app-shell" style="display:none;">
  <aside id="sidebar-root" class="sidebar"><div class="sidebar-header"><div class="sidebar-logo">◈ FAMILY.FIN V2</div></div><nav class="sidebar-nav" id="sidebar-nav"></nav></aside>
  <div class="sidebar-backdrop" id="sidebar-backdrop"></div>
  <div class="main-area">
    <header id="navbar-root" class="navbar">
      <button type="button" class="hamburger" id="hamburger-btn" aria-label="切換選單"><i data-lucide="menu"></i></button>
      <div class="navbar-title" id="navbar-title"></div>
      <div class="navbar-right">
        <span class="navbar-db-tag" id="navbar-db-tag" title="當前連線資料庫">DB: —</span>
        <button type="button" class="navbar-theme-btn" id="navbar-theme-btn" aria-label="切換主題"><i data-lucide="moon"></i></button>
        <div class="navbar-user" id="navbar-user"></div>
      </div>
    </header>
    <main class="content content-min-height" id="page-content"></main>
  </div>
</div>
<div id="error-view" class="auth-page" style="display:none;">
  <div class="glass-card auth-card" style="text-align:center;">
    <h1 class="auth-title" style="color:var(--danger);">⚠️</h1>
    <p class="auth-subtitle">載入失敗</p>
    <div id="error-message" class="banner banner-magenta" style="text-align:left; margin:16px 0;"></div>
    <button type="button" class="btn btn-primary" id="error-retry-btn" style="width:100%; justify-content:center;">重試</button>
  </div>
</div>
<script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>
<script type="module" src="./js/app-core.js"></script>
<script type="module" src="./js/app-pages-core.js"></script>
</body>
</html>

<!-- ═══ EOF ═══
F: app.html
V: 1792670003
W: webapp-20261022-041#3
═══ -->
```

<!-- === FILE END: ./app.html === -->

<!-- === FILE START: ./functions/api/[[path]].js === -->

## 📄 ./functions/api/[[path]].js

```js
// ============================================
// [[path]].js — API Catch-all 路由器 (v2.0.1)
// ============================================
import { initConfig, handleRequest } from './_api-core.js';

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

export async function onRequestGet(context) {
  initConfig(context.env);
  return handleRequest(context.request, 'GET');
}

export async function onRequestPost(context) {
  initConfig(context.env);
  return handleRequest(context.request, 'POST');
}

/* ═══ EOF ═══
F: functions/api/[[path]].js
V: 1792670010
W: webapp-20261022-041#2
═══ */
```

<!-- === FILE END: ./functions/api/[[path]].js === -->

<!-- === FILE START: ./functions/api/_api-core.js === -->

## 📄 ./functions/api/_api-core.js

```js
// ============================================
// _api-core.js — 後端核心邏輯 (v2.0.0)
// ============================================

const _DB_CONFIGS = {
  old: { url: 'https://family-fin-a6dd1-default-rtdb.asia-southeast1.firebasedatabase.app', apiKey: 'AIzaSyCQlrNdorKJI9xsqr4m4ME046lrubo9Y7I' },
  new: { url: 'https://family-3d0a5-default-rtdb.asia-southeast1.firebasedatabase.app', apiKey: 'AIzaSyBJCK83RQyHbol4yJ4UDo3pOUXpUgcW9hU' },
};

let FIREBASE_DB_URL = _DB_CONFIGS.old.url;
let FIREBASE_API_KEY = _DB_CONFIGS.old.apiKey;
const SUPERADMIN_EMAIL = 'superadmin@familyfin.local';
const IDENTITY_TOOLKIT_URL = 'https://identitytoolkit.googleapis.com/v1';

export function initConfig(env) {
  const target = (env && env.DB_TARGET) || 'old';
  const cfg = _DB_CONFIGS[target] || _DB_CONFIGS.old;
  FIREBASE_DB_URL = cfg.url;
  FIREBASE_API_KEY = cfg.apiKey;
}

async function dbGet(path, token = '') {
  const url = `${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`;
  const res = await fetch(url);
  if (!res.ok) return null;
  return res.json();
}
async function dbPut(path, data, token = '') {
  const res = await fetch(`${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`, { method: 'PUT', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(data) });
  return res.ok;
}
async function dbPatch(path, data, token = '') {
  const res = await fetch(`${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`, { method: 'PATCH', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(data) });
  return res.ok;
}
async function dbPush(path, data, token = '') {
  const res = await fetch(`${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(data) });
  if (!res.ok) return null;
  const result = await res.json();
  return result?.name || null;
}
async function dbDelete(path, token = '') {
  const res = await fetch(`${FIREBASE_DB_URL}/${path}.json${token ? `?auth=${token}` : ''}`, { method: 'DELETE' });
  return res.ok;
}
async function dbSignUp(email, password) {
  const res = await fetch(`${IDENTITY_TOOLKIT_URL}/accounts:signUp?key=${FIREBASE_API_KEY}`, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ email, password, returnSecureToken: false }) });
  const data = await res.json();
  if (!res.ok) return { ok: false, error: data?.error?.message || 'UNKNOWN_ERROR' };
  return { ok: true, uid: data.localId, email: data.email };
}

function jsonResponse(data, status = 200) {
  return new Response(JSON.stringify(data), { status, headers: { 'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*' } });
}
function errorResponse(code, message = '', status) {
  const statusMap = { UNAUTHORIZED: 401, FORBIDDEN: 403, MISSING_FIELDS: 400, INVALID_BODY: 400, BAD_REQUEST: 400, NOT_FOUND: 404, CONFLICT: 409, INTERNAL: 500 };
  return jsonResponse({ ok: false, error: code, message: message || undefined }, status || statusMap[code] || 500);
}
function handleError(err) {
  console.error('[_api-core] error:', err);
  return errorResponse('INTERNAL', String(err?.message || err));
}
function successResponse(data = {}) {
  return jsonResponse({ ok: true, ...data });
}
function extractToken(request) {
  return (request.headers.get('Authorization') || '').replace('Bearer ', '').trim();
}
function requireFields(body, fields) {
  if (!body || typeof body !== 'object') return { ok: false, error: 'INVALID_BODY', fields };
  const missing = fields.filter((f) => body[f] === '' || body[f] == null);
  return missing.length > 0 ? { ok: false, error: 'MISSING_FIELDS', fields: missing } : null;
}

const _tokenCache = new Map();
async function verifyToken(token) {
  if (!token) return null;
  const cached = _tokenCache.get(token);
  if (cached && cached.expireAt > Date.now()) return cached.user;
  try {
    const res = await fetch(`${IDENTITY_TOOLKIT_URL}/accounts:lookup?key=${FIREBASE_API_KEY}`, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ idToken: token }) });
    if (!res.ok) return null;
    const data = await res.json();
    const user = data?.users?.[0] || null;
    if (user) _tokenCache.set(token, { user, expireAt: Date.now() + 60000 });
    return user;
  } catch { return null; }
}

async function verifySuperAdmin(token) {
  const user = await verifyToken(token);
  return user?.email === SUPERADMIN_EMAIL ? user : null;
}

async function verifyFamilyAccess(token, familyId) {
  if (!familyId) return null;
  const user = await verifyToken(token);
  if (!user) return null;
  const isSuper = user.email === SUPERADMIN_EMAIL;
  if (isSuper) return { user, isSuper: true, isOwner: true, canInput: true };
  if (user.localId === familyId) return { user, isSuper: false, isOwner: true, canInput: true, isLegacy: true };
  const memberAccount = await dbGet(`platform/families/${familyId}/memberAccounts/${user.localId}`, token);
  if (memberAccount) return { user, isSuper: false, isOwner: memberAccount.role === 'owner', canInput: memberAccount.canInput !== false };
  return null;
}

async function verifyFamilyAccessByUid(uid, token = '') {
  if (!uid) return null;
  const familyId = await dbGet(`platform/uid_index/${uid}`, token);
  if (familyId && typeof familyId === 'string') {
    const memberAccount = await dbGet(`platform/families/${familyId}/memberAccounts/${uid}`, token);
    return { familyId, memberAccount: memberAccount || null, isLegacy: false };
  }
  const familySnap = await dbGet(`platform/families/${uid}`, token);
  if (familySnap) return { familyId: uid, memberAccount: { email: '', displayName: familySnap.name || '我的家庭', role: 'owner', canInput: true }, isLegacy: true };
  return null;
}

async function authenticate(request, options = {}) {
  const { body, requiredFields: reqFields, needSuperAdmin, needFamily } = options;
  const token = extractToken(request);
  if (reqFields && body) { const missing = requireFields(body, reqFields); if (missing) return errorResponse('MISSING_FIELDS', '', 400); }
  if (needSuperAdmin) {
    const user = await verifySuperAdmin(token);
    return user ? { token, user, isSuper: true } : errorResponse('FORBIDDEN', '需要超級管理員權限');
  }
  if (needFamily) {
    let familyId = body?.familyId || new URL(request.url).searchParams.get('familyId');
    if (!familyId) return errorResponse('MISSING_FIELDS', 'familyId 為必填');
    const result = await verifyFamilyAccess(token, familyId);
    if (!result) return errorResponse('FORBIDDEN', '無權存取此家庭');
    return { token, user: result.user, isSuper: result.isSuper, isOwner: result.isOwner, canInput: result.canInput, familyId };
  }
  const user = await verifyToken(token);
  return user ? { token, user, isSuper: user.email === SUPERADMIN_EMAIL } : errorResponse('UNAUTHORIZED', '請重新登入');
}

const objToList = (obj, sortFn) => { const list = Object.entries(obj || {}).map(([id, x]) => ({ id, ...x })); if (sortFn) list.sort(sortFn); return list; };
const roundInt = (v) => Math.round(Number(v) || 0);
const LINKED_PREFIX = 'linked_';
const buildLinkedKey = (policyId) => `${LINKED_PREFIX}${policyId}`;

// --- Admin ---
async function handleAdminFamiliesGet(request) {
  try {
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const list = await dbGet('platform/families', auth.token);
    const families = Object.entries(list || {}).map(([uid, data]) => ({ uid, ...data })).sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0));
    return successResponse({ families });
  } catch (err) { return handleError(err); }
}

async function handleAdminFamiliesPost(request) {
  try {
    const body = await request.json();
    const { action, uid, name, email } = body || {};
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (action === 'add') {
      const missing = requireFields(body, ['uid', 'name']);
      if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid 或 name');
      const success = await dbPut(`platform/families/${uid}`, { name: String(name).trim(), ownerEmail: email ? String(email).trim() : '', createdAt: Date.now() }, token);
      if (!success) return errorResponse('INTERNAL', 'Firebase 寫入失敗');
      return successResponse();
    }
    if (action === 'remove') {
      const missing = requireFields(body, ['uid']);
      if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid');
      let accounts = null;
      try { accounts = await dbGet(`platform/families/${uid}/memberAccounts`, token); } catch (e) {}
      const cleanups = [];
      if (accounts) {
        Object.entries(accounts).forEach(([mUid, acc]) => {
          cleanups.push(dbDelete(`platform/uid_index/${mUid}`, token).catch(() => false));
          if (acc?.account) cleanups.push(dbDelete(`platform/email_index/${acc.account}`, token).catch(() => false));
        });
      }
      await Promise.all(cleanups);
      const ok1 = await dbDelete(`platform/families/${uid}`, token);
      const ok2 = await dbDelete(`families/${uid}`, token);
      if (!ok1 && !ok2) return errorResponse('INTERNAL', '刪除失敗');
      return successResponse({ deletedPlatform: ok1, deletedFamilyData: ok2, cleanedIndexes: cleanups.length });
    }
    return errorResponse('BAD_REQUEST', '未知的 action');
  } catch (err) { return handleError(err); }
}

const FALLBACK = {
  members: { mem_husband: { name: '成員1', role: 'husband', order: 0 }, mem_wife: { name: '成員2', role: 'wife', order: 1 }, mem_son: { name: '成員3', role: 'child', order: 2 }, mem_daughter: { name: '成員4', role: 'child', order: 3 } },
  banks: {}, insurance_companies: { comp_ftlife: { name: '富通', order: 1 }, comp_prudential: { name: '保誠', order: 2 }, comp_fwd: { name: 'FWD', order: 3 }, comp_aia: { name: 'AIA', order: 4 }, comp_manulife: { name: '宏利', order: 5 }, comp_axa: { name: 'AXA', order: 6 } },
  payment_methods: { pm_cash: { name: '現金', order: 1 }, pm_boc: { name: '中銀', order: 2 }, pm_hsbc: { name: '匯豐', order: 3 }, pm_hangs: { name: '恆生', order: 4 }, pm_credit: { name: '信用卡', order: 5 } },
  expense_categories: { cat_medical: { name: '醫療類', order: 1 }, cat_school: { name: '學校類', order: 2 }, cat_insurance: { name: '保險類', order: 3 }, cat_fixed: { name: '固定費用類', order: 4 }, cat_other: { name: '其他', order: 5 } },
  expense_items: { item_med_01: { categoryKey: 'cat_medical', name: '看病-一般' }, item_sch_01: { categoryKey: 'cat_school', name: '學費' } },
  statuses: { status_untreated: { name: '未處理', category: 'personal', isDone: false, order: 1 }, status_done: { name: '已處理', category: 'personal', isDone: true, order: 2 }, status_unbilled: { name: '未扣款', category: 'insurance', isDone: false, order: 8 }, status_billed: { name: '已扣款', category: 'insurance', isDone: true, order: 9 } },
  options: { memberRoles: [{ value: 'husband', label: '老公 / 丈夫' }, { value: 'wife', label: '老婆 / 妻子' }, { value: 'child', label: '子女' }, { value: 'other', label: '其他' }], policyTypes: [{ value: 'normal', label: '普通保險' }, { value: 'fund_insurance', label: '基金保險' }], categoryOrder: ['銀行類', '醫療類', '學校類', '保險類', '固定費用類', '交通類', '其他'] },
  year_range: { startYear: null, futureYears: 5 }, ui_constants: { nameMaxLenDesktop: 12, nameMaxLenMobile: 6, toastDuration: 2000 },
};

async function handleAdminInitFamily(request) {
  try {
    const body = await request.json();
    const { uid } = body || {};
    if (!uid) return errorResponse('MISSING_FIELDS', '缺少 uid');
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;
    const basePath = `families/${uid}`;
    const existing = await dbGet(`${basePath}/members`, token);
    if (existing && Object.keys(existing).length > 0) return successResponse({ skipped: true, message: '此家庭已有資料' });

    const defaults = {};
    for (const k of Object.keys(FALLBACK)) {
      const val = await dbGet(`platform/defaults/${k}`, token);
      defaults[k] = (val && Object.keys(val).length > 0) ? val : FALLBACK[k];
    }
    const now = Date.now();
    const results = { members: false, banks: false, insurance_companies: false, payment_methods: false, expense_categories: false, expense_items: false, statuses: false, options: false, year_range: false, ui_constants: false };

    results.members = await dbPut(`${basePath}/members`, Object.fromEntries(Object.entries(defaults.members).map(([k, v], i) => [k, { ...v, createdAt: now + i }])), token);
    results.banks = await dbPut(`${basePath}/banks`, {}, token);
    results.insurance_companies = await dbPut(`${basePath}/insurance_companies`, Object.fromEntries(Object.entries(defaults.insurance_companies).map(([k, v], i) => [k, { ...v, createdAt: now + i }])), token);
    results.payment_methods = await dbPut(`${basePath}/payment_methods`, Object.fromEntries(Object.entries(defaults.payment_methods).map(([k, v], i) => [k, { ...v, createdAt: now + i }])), token);
    results.expense_categories = await dbPut(`${basePath}/expense_categories`, Object.fromEntries(Object.entries(defaults.expense_categories).map(([k, v], i) => [k, { ...v, createdAt: now + i }])), token);
    results.expense_items = await dbPut(`${basePath}/expense_items`, Object.fromEntries(Object.entries(defaults.expense_items).map(([k, v], i) => [k, { name: v.name || '', categoryId: v.categoryKey || v.categoryId || '', createdAt: now + i }])), token);
    results.statuses = await dbPut(`${basePath}/statuses`, Object.fromEntries(Object.entries(defaults.statuses).map(([k, v], i) => [k, { ...v, createdAt: now + i }])), token);
    results.options = await dbPut(`${basePath}/settings/options`, defaults.options, token);
    results.year_range = await dbPut(`${basePath}/settings/year_range`, defaults.year_range, token);
    results.ui_constants = await dbPut(`${basePath}/settings/ui_constants`, defaults.ui_constants, token);

    const failed = Object.entries(results).filter(([, ok]) => ok === false).map(([k]) => k);
    return successResponse({ created: true, partial: failed.length > 0, failedSteps: failed, results });
  } catch (err) { return handleError(err); }
}

const LEGACY_STATUS_MAP = { '未處理': 'pending', '未還款': 'pending', '未付款': 'pending', '未扣款': 'pending', '未轉入': 'pending', '已處理': 'done', '已還款': 'done', '已付款': 'done', '已扣款': 'done', '已轉入': 'done', '不適用': 'skipped' };
const _normalizeStatus = (raw) => (raw == null || raw === '') ? 'pending' : (['pending', 'done', 'skipped'].includes(raw) ? raw : (LEGACY_STATUS_MAP[raw] || 'pending'));

async function handleAdminMigrate(request) {
  try {
    const body = await request.json();
    const { familyId, confirm } = body || {};
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (confirm !== 'MIGRATE_ALL') return errorResponse('BAD_REQUEST', '必須提供確認參數');
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;
    const basePath = `families/${familyId}`;
    const summary = { expenses: { scanned: 0, normalized: 0 }, payments: { scanned: 0, normalized: 0 }, policies: { scanned: 0, patched: 0 }, funds: { scanned: 0, patched: 0 } };

    const expensesSnap = await dbGet(`${basePath}/expenses`, token);
    if (expensesSnap) {
      for (const [y, months] of Object.entries(expensesSnap)) {
        for (const [m, monthData] of Object.entries(months || {})) {
          for (const [mid, items] of Object.entries(monthData?.member_expenses || {})) {
            for (const [eid, e] of Object.entries(items || {})) {
              if (!e || typeof e !== 'object') continue;
              summary.expenses.scanned++;
              let changed = false;
              const ns = _normalizeStatus(e.status);
              if (e.status !== ns) { e.status = ns; changed = true; }
              if (e.isAutoLinked === undefined) { e.isAutoLinked = !!e.policyId; changed = true; }
              if (changed) { await dbPut(`${basePath}/expenses/${y}/${m}/member_expenses/${mid}/${eid}`, e, token); summary.expenses.normalized++; }
            }
          }
        }
      }
    }
    return successResponse({ migrated: true, familyId, summary });
  } catch (err) { return handleError(err); }
}

// --- Bank ---
async function handleBankAccountsGet(request) {
  try {
    const familyId = new URL(request.url).searchParams.get('familyId');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    const auth = await authenticate(request, { needFamily: true });
    if (auth instanceof Response) return auth;
    const data = await dbGet(`families/${familyId}/bank_accounts`, auth.token);
    const list = objToList(data, (a, b) => (a.order ?? 9999) - (b.order ?? 9999) || (a.createdAt || 0) - (b.createdAt || 0));
    return successResponse({ familyId, accounts: list, count: list.length });
  } catch (err) { return handleError(err); }
}

async function handleBankAccountsPost(request) {
  try {
    const body = await request.json();
    const { familyId, action, id, data } = body || {};
    if (!familyId || !action) return errorResponse('MISSING_FIELDS', '缺少參數');
    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token } = auth;
    const basePath = `families/${familyId}/bank_accounts`;

    if (action === 'create') {
      if (!data || !data.name) return errorResponse('MISSING_FIELDS', '缺少 data 或 name');
      const clean = { name: String(data.name).trim(), type: ['family', 'personal'].includes(data.type) ? data.type : 'family', initialBalance: roundInt(data.initialBalance), initialYear: String(data.initialYear || new Date().getFullYear()), initialMonth: String(data.initialMonth || '01').padStart(2, '0'), order: Number(data.order) || 0, createdAt: Date.now() };
      const newId = await dbPush(basePath, clean, token);
      return newId ? successResponse({ id: newId, data: clean }) : errorResponse('INTERNAL', '寫入失敗');
    }
    if (action === 'update') {
      if (!id || !data) return errorResponse('MISSING_FIELDS', '缺少 id 或 data');
      const existing = await dbGet(`${basePath}/${id}`, token);
      if (!existing) return errorResponse('NOT_FOUND', '找不到');
      const clean = { ...existing };
      if (data.name !== undefined) clean.name = String(data.name).trim();
      if (data.type !== undefined) clean.type = data.type;
      if (data.initialBalance !== undefined) clean.initialBalance = roundInt(data.initialBalance);
      if (data.initialYear !== undefined) clean.initialYear = String(data.initialYear);
      if (data.initialMonth !== undefined) clean.initialMonth = String(data.initialMonth).padStart(2, '0');
      if (data.order !== undefined) clean.order = Number(data.order) || 0;
      clean.updatedAt = Date.now();
      return await dbPut(`${basePath}/${id}`, clean, token) ? successResponse({ id, data: clean }) : errorResponse('INTERNAL', '更新失敗');
    }
    if (action === 'remove') {
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');
      const refCount = await _countBankReferences(basePath, id, token);
      if (refCount > 0) return errorResponse('CONFLICT', `此銀行仍被 ${refCount} 筆記錄引用，請先移除引用`, 409);
      return await dbDelete(`${basePath}/${id}`, token) ? successResponse({ removedId: id }) : errorResponse('INTERNAL', '刪除失敗');
    }
    return errorResponse('BAD_REQUEST', '未知 action');
  } catch (err) { return handleError(err); }
}

async function _countBankReferences(bankAccountsBasePath, bankId, token) {
  const familyBase = bankAccountsBasePath.replace(/\/bank_accounts$/, '');
  let count = 0;
  try {
    const expensesSnap = await dbGet(`${familyBase}/expenses`, token);
    if (expensesSnap) Object.values(expensesSnap).forEach((ms) => Object.values(ms || {}).forEach((md) => Object.values(md?.member_expenses || {}).forEach((items) => Object.values(items || {}).forEach((e) => { if (e?.bankId === bankId) count++; }))));
    const paySnap = await dbGet(`${familyBase}/insurance_payments`, token);
    if (paySnap) Object.values(paySnap).forEach((ys) => Object.values(ys || {}).forEach((ms) => Object.values(ms || {}).forEach((p) => { if (p?.bankId === bankId) count++; })));
  } catch (e) {}
  return count;
}

async function handleBankTransactionsGet(request) { return successResponse({}); }
async function handleBankTransactionsPost(request) { return successResponse({}); }
async function handleClearBankBalances(request) {
  try {
    const body = await request.json();
    if (body.confirm !== 'CONFIRM_DELETE') return errorResponse('BAD_REQUEST', '必須提供確認');
    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    await dbDelete(`families/${body.familyId}/bank_balances`, auth.token);
    return successResponse({ cleared: true });
  } catch (err) { return handleError(err); }
}

// --- Family ---
async function handleLookupFamily(request) {
  try {
    const auth = await authenticate(request);
    if (auth instanceof Response) return auth;
    const result = await verifyFamilyAccessByUid(auth.user.localId, auth.token);
    if (!result) return errorResponse('NOT_FOUND', '此帳號不屬於任何家庭');
    let familyName = '我的家庭';
    try { const s = await dbGet(`platform/families/${result.familyId}`, auth.token); if (s?.name) familyName = s.name; } catch (e) {}
    return successResponse({ familyId: result.familyId, familyName, memberAccount: result.memberAccount, isLegacy: result.isLegacy });
  } catch (err) { return handleError(err); }
}

async function handleFamilySettingsGet(request) { return successResponse({}); }
async function handleFamilySettingsPost(request) { return successResponse({}); }

async function handleFamilyAccountsGet(request) {
  try {
    const familyId = new URL(request.url).searchParams.get('familyId');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    const token = extractToken(request);
    const superUser = await verifySuperAdmin(token);
    if (!superUser && !(await verifyFamilyAccess(token, familyId))) return errorResponse('FORBIDDEN', '無權存取');
    const snap = await dbGet(`platform/families/${familyId}/memberAccounts`, token);
    const accounts = objToList(snap, (a, b) => (a.createdAt || 0) - (b.createdAt || 0));
    return successResponse({ familyId, accounts, count: accounts.length });
  } catch (err) { return handleError(err); }
}

async function handleFamilyAccountsPost(request) { return successResponse({}); }

// --- Entity (統一 CRUD) ---
const ENTITY_PATH = { member: 'members', bank: 'banks', bankAccount: 'bank_accounts', category: 'expense_categories', item: 'expense_items', payment: 'payment_methods', company: 'insurance_companies', policy: 'insurance_policies', fund: 'funds' };
const REQUIRED_EXTRA = { member: [], bank: [], bankAccount: ['bankId', 'type', 'ownerType'], category: ['shortCode'], item: ['categoryId'], payment: [], company: [], policy: ['type', 'memberId', 'firstStartYear', 'firstStartMonth', 'totalPolicyYears'], fund: ['type'] };

async function handleEntityPost(request) {
  try {
    const body = await request.json();
    const { familyId, action, entityType, id, code, name, extra } = body;
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    const token = extractToken(request);
    const access = await verifyFamilyAccess(token, familyId);
    if (!access) return errorResponse('FORBIDDEN', '無權存取');
    if (!entityType || !ENTITY_PATH[entityType]) return errorResponse('INVALID_BODY', `未知 entityType`);

    const path = ENTITY_PATH[entityType];
    const listPath = `families/${familyId}/${path}`;

    if (action === 'create') {
      const missing = (REQUIRED_EXTRA[entityType] || []).filter((k) => extra[k] === undefined || extra[k] === '');
      if (missing.length) return errorResponse('MISSING_FIELDS', `缺少欄位：${missing.join(', ')}`);
      if (!name || !code) return errorResponse('MISSING_FIELDS', '缺少 name 或 code');
      const list = (await dbGet(listPath, token)) || {};
      if (Object.values(list).some((v) => v.code === code)) return errorResponse('CODE_DUPLICATE', `編號已存在：${code}`, 409);
      if (Object.values(list).some((v) => v.name === name)) return errorResponse('NAME_DUPLICATE', `名稱重複：${name}`, 409);
      
      const payload = { code, name, ..._normalizeExtra(entityType, extra), createdAt: Date.now() };
      const newId = await dbPush(listPath, payload, token);
      return newId ? successResponse({ id: newId, code }) : errorResponse('INTERNAL', '寫入失敗');
    }

    if (action === 'update') {
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');
      const itemPath = `${listPath}/${id}`;
      const existing = await dbGet(itemPath, token);
      if (!existing) return errorResponse('NOT_FOUND', `找不到 ${id}`);
      if (name) {
        const list = (await dbGet(listPath, token)) || {};
        if (Object.entries(list).some(([k, v]) => k !== id && v.name === name)) return errorResponse('NAME_DUPLICATE', `名稱重複：${name}`, 409);
      }
      const payload = { ...(name !== undefined ? { name } : {}), ...(Object.keys(extra).length ? _normalizeExtra(entityType, extra) : {}), updatedAt: Date.now() };
      return await dbPatch(itemPath, payload, token) ? successResponse({ id }) : errorResponse('INTERNAL', '更新失敗');
    }

    if (action === 'delete') {
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');
      const itemPath = `${listPath}/${id}`;
      if (!(await dbGet(itemPath, token))) return errorResponse('NOT_FOUND', `找不到 ${id}`);
      return await dbDelete(itemPath, token) ? successResponse({ id }) : errorResponse('INTERNAL', '刪除失敗');
    }
    return errorResponse('INVALID_BODY', '未知 action');
  } catch (err) { return handleError(err); }
}

function _normalizeExtra(entityType, extra) {
  const round = (v) => Math.round(Number(v) || 0);
  switch (entityType) {
    case 'member': return { role: extra.role || 'other', order: Number(extra.order) || 0 };
    case 'bank': return { order: Number(extra.order) || 0 };
    case 'bankAccount': return { type: extra.type, ownerType: extra.ownerType, ownerId: extra.ownerId || 'family', initialBalance: round(extra.initialBalance), initialYear: String(extra.initialYear || ''), initialMonth: String(extra.initialMonth || '01').padStart(2, '0'), order: Number(extra.order) || 0 };
    case 'category': return { shortCode: extra.shortCode, order: Number(extra.order) || 0 };
    case 'item': return { categoryId: extra.categoryId };
    case 'payment': return { order: Number(extra.order) || 0 };
    case 'company': return {};
    case 'policy': return { type: extra.type, memberId: extra.memberId, policyHolderId: extra.policyHolderId || extra.memberId, firstStartYear: Number(extra.firstStartYear), firstStartMonth: String(extra.firstStartMonth).padStart(2, '0'), totalPolicyYears: Number(extra.totalPolicyYears), totalPolicyPeriods: Number(extra.totalPolicyYears) * 12, monthlyPremium: round(extra.monthlyPremium), annualPremium: round(extra.annualPremium), fundsAllocation: Array.isArray(extra.fundsAllocation) ? extra.fundsAllocation : [] };
    case 'fund': return { type: extra.type, policyId: extra.policyId || '', cost: round(extra.cost), initialYear: String(extra.initialYear || ''), initialMonth: String(extra.initialMonth || '').padStart(2, '0') };
    default: return {};
  }
}

// --- Insurance ---
async function handleInsuranceSync(request) {
  try {
    const body = await request.json();
    const { action, familyId, policyId, memberId, policyName, monthlyAverage, year, month, bankId, txnId, paymentMode, advanceHolderId } = body || {};
    if (!familyId || !policyId || !memberId || !year || !month) return errorResponse('MISSING_FIELDS');
    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token } = auth;
    const basePath = `families/${familyId}`;
    const linkedKey = buildLinkedKey(policyId);
    const expensePath = `${basePath}/expenses/${year}/${month}/member_expenses/${memberId}/${linkedKey}`;
    const paymentPath = `${basePath}/insurance_payments/${policyId}/${year}/${month}`;
    const amount = roundInt(monthlyAverage);

    if (action === 'delete') {
      await dbDelete(expensePath, token);
      await dbDelete(paymentPath, token);
      await _reverseFundAccumulation({ basePath, policyId, year, month, token });
      return successResponse({ deleted: true });
    }

    const statuses = await dbGet(`${basePath}/statuses`, token) || {};
    const categories = await dbGet(`${basePath}/expense_categories`, token) || {};
    const firstCategoryId = Object.values(categories).sort((a, b) => (a.order || 0) - (b.order || 0))[0]?.id || '';
    
    const expense = { name: `${policyName} (平攤)`, amount, status: 'done', date: '', categoryId: firstCategoryId, itemId: '', paymentMethodId: '', bankId: bankId || '', txnId: txnId || '', isAutoLinked: true, policyId, createdAt: Date.now() };
    const paymentRecord = { status: 'done', amount, date: new Date().toISOString().slice(0, 10), bankId: bankId || '', txnId: txnId || '', paymentMode: paymentMode || 'direct' };
    
    await Promise.all([ dbPut(expensePath, expense, token), dbPut(paymentPath, paymentRecord, token) ]);
    await _accumulateFunds({ basePath, policyId, year, month, token });

    return successResponse({ path: expensePath });
  } catch (err) { return handleError(err); }
}

async function _accumulateFunds({ basePath, policyId, year, month, token }) {
  const policy = await dbGet(`${basePath}/insurance_policies/${policyId}`, token);
  if (!policy || policy.type !== 'fund_insurance') return;
  const alloc = policy.fundsAllocation || [];
  if (!alloc.length) return;
  const monthlyPremium = roundInt(policy.monthlyPremium);
  for (const a of alloc) {
    if (!a.fundId) continue;
    const addCost = Math.round(monthlyPremium * (Number(a.pct) || 0) / 100);
    const fundPath = `${basePath}/funds/${a.fundId}`;
    const snapPath = `${fundPath}/snapshots/${year}/${String(month).padStart(2, '0')}`;
    const snap = (await dbGet(snapPath, token)) || {};
    await dbPut(snapPath, { ...snap, cumulativeCost: roundInt(snap.cumulativeCost) + addCost, contribution: addCost, createdAt: snap.createdAt || Date.now(), updatedAt: Date.now() }, token);
    const fund = (await dbGet(fundPath, token)) || {};
    if (fund.type !== 'insurance') await dbPut(fundPath, { ...fund, type: 'insurance', policyId }, token);
  }
}
async function _reverseFundAccumulation({ basePath, policyId, year, month, token }) {
  const policy = await dbGet(`${basePath}/insurance_policies/${policyId}`, token);
  if (!policy || policy.type !== 'fund_insurance') return;
  const alloc = policy.fundsAllocation || [];
  const monthlyPremium = roundInt(policy.monthlyPremium);
  for (const a of alloc) {
    if (!a.fundId) continue;
    const subCost = Math.round(monthlyPremium * (Number(a.pct) || 0) / 100);
    const snapPath = `${basePath}/funds/${a.fundId}/snapshots/${year}/${String(month).padStart(2, '0')}`;
    const snap = await dbGet(snapPath, token);
    if (!snap) continue;
    await dbPut(snapPath, { ...snap, cumulativeCost: Math.max(0, roundInt(snap.cumulativeCost) - subCost), contribution: 0, updatedAt: Date.now() }, token);
  }
}

// --- Personal ---
async function handlePersonalIncomeGet(request) { return successResponse({}); }
async function handlePersonalIncomePost(request) { return successResponse({}); }
async function handleMemberAdvancesGet(request) { return successResponse({}); }
async function handleMemberAdvancesPost(request) { return successResponse({}); }

// --- Platform ---
async function handlePlatformSettingsGet(request) {
  try {
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const data = await dbGet('platform/defaults/ui_constants', auth.token);
    return successResponse({ data: data || {} });
  } catch (err) { return handleError(err); }
}
async function handlePlatformSettingsPost(request) { return successResponse({}); }
async function handlePlatformDefaultsGet(request) {
  try {
    const resource = new URL(request.url).searchParams.get('resource');
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const data = await dbGet(`platform/defaults/${resource}`, auth.token);
    return successResponse({ data: data || {} });
  } catch (err) { return handleError(err); }
}
async function handlePlatformDefaultsPost(request) { return successResponse({}); }

// --- Summary ---
async function handleSummary(request) {
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

    const [members, policies, expenses, income, funds, categories, items, banks, paymentMethods, statuses, settingsOptions, bankAccounts, personalIncome, memberAdvances] = await Promise.all([
      dbGet(`${basePath}/members`, token), dbGet(`${basePath}/insurance_policies`, token), year && month ? dbGet(`${basePath}/expenses/${year}/${month}/member_expenses`, token) : null,
      year && month ? dbGet(`${basePath}/income/${year}/${month}`, token) : null, dbGet(`${basePath}/funds`, token), dbGet(`${basePath}/expense_categories`, token),
      dbGet(`${basePath}/expense_items`, token), dbGet(`${basePath}/banks`, token), dbGet(`${basePath}/payment_methods`, token), dbGet(`${basePath}/statuses`, token),
      dbGet(`${basePath}/settings/options`, token), dbGet(`${basePath}/bank_accounts`, token), dbGet(`${basePath}/personal_income`, token), dbGet(`${basePath}/member_advances`, token),
    ]);

    const membersObj = members || {};
    let totalExpense = 0; const perMember = {};
    Object.entries(expenses || {}).forEach(([mid, list]) => {
      const itemsArr = objToList(list).map((e) => ({ ...e, amount: roundInt(e.amount) }));
      const sum = itemsArr.reduce((s, e) => s + e.amount, 0);
      perMember[mid] = { memberName: membersObj[mid]?.name || '未知', itemCount: itemsArr.length, sum: roundInt(sum), items: itemsArr };
      totalExpense += sum;
    });

    let totalIncome = 0; const incomeBreakdown = {};
    Object.entries(income || {}).forEach(([key, val]) => {
      const num = roundInt(val);
      if (key === 'extra' || membersObj[key]) { incomeBreakdown[key] = num; totalIncome += num; }
    });

    let piTotal = 0;
    Object.values(personalIncome || {}).forEach((yd) => Object.values(yd?.[year] || {}).forEach((v) => { piTotal += roundInt(v); }));

    let yearlyIns = 0, monthlyIns = 0, activeCount = 0;
    const curY = Number(year), curM = Number(month);
    Object.values(policies || {}).forEach((p) => {
      const fY = Number(p.firstStartYear) || 0; const fM = Number(p.firstStartMonth) || 1;
      if (!fY) return;
      const tMonths = (curY - fY) * 12 + (curM - fM);
      if (tMonths < 0) return;
      const pIdx = Math.floor(tMonths / 12) + 1;
      if (Number(p.totalPolicyYears) > 0 && pIdx > Number(p.totalPolicyYears)) return;
      if (p.type === 'fund_insurance') {
        const mp = roundInt(p.monthlyPremium); if (mp <= 0) return;
        monthlyIns += mp; yearlyIns += mp * 12; activeCount++;
      } else {
        const pd = (p.periods || {})[String(pIdx)];
        if (pd) { monthlyIns += roundInt(pd.monthlyAverage); yearlyIns += roundInt(pd.annualPremium); activeCount++; }
      }
    });

    const bankList = Object.entries(bankAccounts || {}).map(([id, x]) => ({ id, ...x }));
    let bankBalance = 0;
    return jsonResponse({ ok: true, year, month, totalIncome: roundInt(totalIncome), totalExpense: roundInt(totalExpense), netBalance: roundInt(totalIncome - totalExpense), personalIncomeTotal: roundInt(piTotal), yearlyInsuranceTotal: roundInt(yearlyIns), monthlyInsuranceAverage: roundInt(monthlyIns), policyCount: activeCount, bankBalance, perMember });
  } catch (err) { return handleError(err); }
}

async function handleAnnualSummary(request) { return jsonResponse({ ok: true, years: [] }); }
async function handleSettlementsYear(request) { return jsonResponse({ ok: true, memberExpensesByMonth: {}, policies: [], paymentsCache: {} }); }

export async function handleRequest(request, method) {
  const url = new URL(request.url);
  const path = url.pathname;

  if (method === 'GET') {
    if (path === '/api/family-settings') return handleFamilySettingsGet(request);
    if (path === '/api/family-accounts') return handleFamilyAccountsGet(request);
    if (path === '/api/admin-families') return handleAdminFamiliesGet(request);
    if (path === '/api/summary') return handleSummary(request);
    if (path === '/api/annual-summary') return handleAnnualSummary(request);
    if (path === '/api/settlements-year') return handleSettlementsYear(request);
    if (path === '/api/platform-settings') return handlePlatformSettingsGet(request);
    if (path === '/api/platform-defaults') return handlePlatformDefaultsGet(request);
    if (path === '/api/bank-accounts') return handleBankAccountsGet(request);
    if (path === '/api/bank-transactions') return handleBankTransactionsGet(request);
    if (path === '/api/personal-income') return handlePersonalIncomeGet(request);
    if (path === '/api/member-advances') return handleMemberAdvancesGet(request);
  }

  if (method === 'POST') {
    if (path === '/api/lookup-family') return handleLookupFamily(request);
    if (path === '/api/family-settings') return handleFamilySettingsPost(request);
    if (path === '/api/family-accounts') return handleFamilyAccountsPost(request);
    if (path === '/api/admin-families') return handleAdminFamiliesPost(request);
    if (path === '/api/admin-init-family') return handleAdminInitFamily(request);
    if (path === '/api/admin-migrate') return handleAdminMigrate(request);
    if (path === '/api/platform-settings') return handlePlatformSettingsPost(request);
    if (path === '/api/platform-defaults') return handlePlatformDefaultsPost(request);
    if (path === '/api/bank-accounts') return handleBankAccountsPost(request);
    if (path === '/api/bank-transactions') return handleBankTransactionsPost(request);
    if (path === '/api/clear-bank-balances') return handleClearBankBalances(request);
    if (path === '/api/personal-income') return handlePersonalIncomePost(request);
    if (path === '/api/member-advances') return handleMemberAdvancesPost(request);
    if (path === '/api/insurance-sync') return handleInsuranceSync(request);
    if (path === '/api/entity-create') return handleEntityPost(request);
  }

  return errorResponse('NOT_FOUND', `未知的 API：${path}`);
}

/* ═══ EOF ═══
F: functions/api/_api-core.js
V: 1792670011
W: webapp-20261022-041#1
═══ */
```

<!-- === FILE END: ./functions/api/_api-core.js === -->

<!-- === FILE START: ./js/app-core.js === -->

## 📄 ./js/app-core.js

```js
// ============================================
// app-core.js — SPA 核心（狀態 + 路由 + 啟動）
// 版本：1.0.0（第 1 回骨架，第 2/3 回接入 ui/data/pages）
// ============================================
// 職責：
//   1. Firebase 初始化（沿用舊 DB 設定）
//   2. 登入 / 登出
//   3. AppState（沿用舊 state.js 的 API 介面）
//   4. Hash 路由（#/dashboard、#/insurance…）
//   5. 渲染 sidebar / navbar / 頁面外殼
//   6. 提供 registerPage(route, loader) 供 app-pages.js 註冊
// ============================================

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
import { getAuth, signInWithEmailAndPassword, signOut, onAuthStateChanged }
  from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";

/* ============================================
   1. Firebase 設定（沿用舊版兩套環境）
   ============================================ */
const FIREBASE_CONFIGS = {
  old: {
    apiKey: "AIzaSyCQlrNdorKJI9xsqr4m4ME046lrubo9Y7I",
    authDomain: "family-fin-a6dd1.firebaseapp.com",
    databaseURL: "https://family-fin-a6dd1-default-rtdb.asia-southeast1.firebasedatabase.app",
    projectId: "family-fin-a6dd1",
    storageBucket: "family-fin-a6dd1.firebasestorage.app",
    messagingSenderId: "382147296734",
    appId: "1:382147296734:web:42bdbd7401001e75e1f14f",
  },
  new: {
    apiKey: "AIzaSyBJCK83RQyHbol4yJ4UDo3pOUXpUgcW9hU",
    authDomain: "family-3d0a5.firebaseapp.com",
    databaseURL: "https://family-3d0a5-default-rtdb.asia-southeast1.firebasedatabase.app",
    projectId: "family-3d0a5",
    storageBucket: "family-3d0a5.firebasestorage.app",
    messagingSenderId: "109775445681",
    appId: "1:109775445681:web:84e974db35951d0a6ecc90",
  },
};

const DB_TARGET_KEY = 'fin_db_target';

function resolveDbTarget() {
  try {
    const params = new URLSearchParams(location.search);
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

export const DB_TARGET = resolveDbTarget();
const firebaseConfig = FIREBASE_CONFIGS[DB_TARGET];
const firebaseApp = initializeApp(firebaseConfig);
const auth = getAuth(firebaseApp);

const DB_LABELS = { old: 'family-fin', new: '2026-10-12-family' };
export const SUPERADMIN_DOMAIN = '@familyfin.local';
export const SUPERADMIN_EMAIL = `superadmin${SUPERADMIN_DOMAIN}`;

/* ============================================
   2. localStorage Keys（沿用舊 constants.js）
   ============================================ */
const _P = `fin_${DB_TARGET}_`;
const STORAGE_KEYS = {
  YEAR: `${_P}year`,
  MONTH: `${_P}month`,
  FAMILY_ID: `${_P}family_id`,
  FAMILY_NAME: `${_P}family_name`,
  SIDEBAR_GROUPS: `${_P}ui_sidebar_groups`,
  SIDEBAR_COLLAPSED: `${_P}ui_sidebar_collapsed`,
  STATS_MODE: `${_P}ui_stats_mode`,
  THEME: 'fin_ui_theme',
};

/* ============================================
   3. AppState（沿用舊 state.js API，介面 100% 一致）
   ============================================ */
export const AppState = {
  currentUser: null,
  isSuperAdmin: false,
  currentFamilyId: '',
  currentFamilyName: '',
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
    this.year = localStorage.getItem(STORAGE_KEYS.YEAR) || String(new Date().getFullYear());
    this.month = localStorage.getItem(STORAGE_KEYS.MONTH) || 'all';
    this.currentFamilyId = localStorage.getItem(STORAGE_KEYS.FAMILY_ID) || '';
    this.currentFamilyName = localStorage.getItem(STORAGE_KEYS.FAMILY_NAME) || '';
    const savedTheme = localStorage.getItem(STORAGE_KEYS.THEME);
    this.theme = (savedTheme === 'dark' || savedTheme === 'light') ? savedTheme : 'light';
    this._applyTheme(this.theme);
  },

  _applyTheme(theme) {
    if (typeof document !== 'undefined') {
      document.documentElement.setAttribute('data-theme', theme);
    }
  },

  setTheme(theme) {
    if (theme !== 'light' && theme !== 'dark') return;
    if (this.theme === theme) return;
    this.theme = theme;
    localStorage.setItem(STORAGE_KEYS.THEME, theme);
    this._applyTheme(theme);
    this.emit('theme-change', theme);
  },

  getTheme() { return this.theme; },
  toggleTheme() { this.setTheme(this.theme === 'light' ? 'dark' : 'light'); },

  setUser(user) { this.currentUser = user; this.emit('user-change', user); },

  setSuperAdmin(isSuper) {
    this.isSuperAdmin = !!isSuper;
    if (this.isSuperAdmin) { this.role = 'superadmin'; this.canInput = true; }
    this.emit('superadmin-change', this.isSuperAdmin);
  },

  setFamily(familyId, familyName) {
    this.currentFamilyId = familyId || '';
    this.currentFamilyName = familyName || '';
    if (this.currentFamilyId) {
      localStorage.setItem(STORAGE_KEYS.FAMILY_ID, this.currentFamilyId);
      localStorage.setItem(STORAGE_KEYS.FAMILY_NAME, this.currentFamilyName);
    } else {
      localStorage.removeItem(STORAGE_KEYS.FAMILY_ID);
      localStorage.removeItem(STORAGE_KEYS.FAMILY_NAME);
    }
    this.emit('family-change', { familyId: this.currentFamilyId, familyName: this.currentFamilyName });
  },

  getFamilyId() { return this.currentFamilyId; },
  getFamilyName() { return this.currentFamilyName; },

  setRole(role) { this.role = role || ''; this.emit('role-change', this.role); },
  getRole() { return this.role; },

  setCanInput(canInput) { this.canInput = !!canInput; this.emit('permission-change', { canInput: this.canInput }); },
  getCanInput() { return this.canInput; },

  setDisplayName(displayName) { this.displayName = displayName || ''; this.emit('displayname-change', this.displayName); },
  getDisplayName() { return this.displayName; },

  setMemberAccount(account) {
    this.memberAccount = account || null;
    if (account) {
      this.role = account.role || this.role;
      this.canInput = account.canInput !== false;
      this.displayName = account.displayName || this.displayName;
      if (account.memberId) this.currentMemberId = account.memberId;
    }
    this.emit('member-account-change', this.memberAccount);
  },

  getMemberAccount() { return this.memberAccount; },
  getCurrentMemberId() { return this.currentMemberId || (this.memberAccount?.memberId) || ''; },

  setYearMonth(year, month) {
    this.year = String(year);
    this.month = String(month);
    localStorage.setItem(STORAGE_KEYS.YEAR, this.year);
    localStorage.setItem(STORAGE_KEYS.MONTH, this.month);
    this.emit('ym-change', { year: this.year, month: this.month });
  },

  getYearMonth() { return { year: this.year, month: this.month }; },

  setCurrentView(view) {
    if (view !== 'table' && view !== 'card') return;
    if (this.currentView === view) return;
    this.currentView = view;
    this.emit('view-change', view);
  },
  getCurrentView() { return this.currentView; },

  on(event, callback) {
    if (!this._listeners[event]) this._listeners[event] = [];
    this._listeners[event].push(callback);
    return () => {
      this._listeners[event] = (this._listeners[event] || []).filter((cb) => cb !== callback);
    };
  },

  emit(event, data) {
    (this._listeners[event] || []).forEach((cb) => {
      try { cb(data); } catch (err) { console.error(`[AppState] ${event} 回呼失敗：`, err); }
    });
  },
};

/* ============================================
   4. 認證
   ============================================ */
function toEmail(account) {
  if (!account) return '';
  return account.includes('@') ? account : `${account}${SUPERADMIN_DOMAIN}`;
}

export function isSuperAdmin(user) {
  return !!user && user.email === SUPERADMIN_EMAIL;
}

async function loginWithCustomAccount(account, password) {
  const cred = await signInWithEmailAndPassword(auth, toEmail(account), password);
  return cred.user;
}

export async function logout() {
  try { await signOut(auth); } catch (err) { console.warn('[auth] signOut 失敗：', err); }
  localStorage.removeItem(STORAGE_KEYS.FAMILY_ID);
  localStorage.removeItem(STORAGE_KEYS.FAMILY_NAME);
  location.hash = '#/';
  location.reload();
}

/* ============================================
   5. 路由表 + 頁面註冊介面
   ============================================ */
const ROUTES = {
  '/dashboard':   { title: '總覽儀表板',    icon: 'home' },
  '/input-center':{ title: '綜合輸入中心',  icon: 'pencil' },
  '/code-list':   { title: '編號對照清單',  icon: 'database' },
  '/insurance':   { title: '保險清單表',    icon: 'shield' },
  '/portfolio':   { title: '基金投資表',    icon: 'line-chart' },
  '/settlements': { title: '結算清單',      icon: 'clipboard-check' },
  '/finance':     { title: '銀行交易',      icon: 'landmark' },
  '/annual-report':{ title: '年度報表',     icon: 'bar-chart-3' },
  '/member-report':{ title: '成員收支明細',  icon: 'users' },
  '/settings':    { title: '系統設定',      icon: 'settings' },
  '/admin':       { title: '平台管理',      icon: 'server' },
};

const _pageLoaders = {};
let _currentPage = null;

export function registerPage(route, loader) {
  _pageLoaders[route] = loader;
}

/* ============================================
   6. 側邊欄群組（依角色過濾，沿用舊版結構）
   ============================================ */
const _ROLES_FAMILY = ['owner', 'member'];
const _ROLES_ALL    = ['owner', 'member', 'superadmin'];
const _ROLES_ADMIN  = ['superadmin'];

const SIDEBAR_GROUPS = [
  { key: 'overview',       label: '總覽',       icon: 'home',            defaultOpen: true,  roles: _ROLES_FAMILY,
    items: [{ icon: 'home', label: '總覽儀表板', route: '/dashboard' }] },
  { key: 'record',         label: '記錄中心',   icon: 'pencil',          defaultOpen: true,  roles: _ROLES_FAMILY,
    items: [
      { icon: 'pencil',   label: '綜合輸入中心', route: '/input-center' },
      { icon: 'database', label: '編號對照清單', route: '/code-list' },
    ] },
  { key: 'insurance-fund', label: '保險與基金', icon: 'shield',          defaultOpen: false, roles: _ROLES_FAMILY,
    items: [
      { icon: 'shield',     label: '保險清單表', route: '/insurance' },
      { icon: 'line-chart', label: '基金投資表', route: '/portfolio' },
    ] },
  { key: 'settle',         label: '對帳',       icon: 'clipboard-check', defaultOpen: false, roles: _ROLES_FAMILY,
    items: [{ icon: 'clipboard-check', label: '結算清單', route: '/settlements' }] },
  { key: 'assets',         label: '資產',       icon: 'landmark',        defaultOpen: false, roles: _ROLES_FAMILY,
    items: [{ icon: 'landmark', label: '銀行交易', route: '/finance' }] },
  { key: 'reports',        label: '報表',       icon: 'bar-chart-3',     defaultOpen: false, roles: _ROLES_FAMILY,
    items: [
      { icon: 'bar-chart-3', label: '年度報表', route: '/annual-report' },
      { icon: 'users',       label: '成員收支明細', route: '/member-report' },
    ] },
  { key: 'platform',       label: '平台管理',   icon: 'server',          defaultOpen: true,  roles: _ROLES_ADMIN,
    items: [{ icon: 'home', label: '家庭管理', route: '/admin' }] },
  { key: 'system',         label: '系統',       icon: 'settings',        defaultOpen: false, roles: _ROLES_ALL,
    items: [{ icon: 'settings', label: '系統設定', route: '/settings' }] },
];

function getSidebarGroupsByRole(role) {
  if (!role) return SIDEBAR_GROUPS.filter((g) => (g.roles || []).includes('owner'));
  return SIDEBAR_GROUPS.filter((g) => (g.roles || []).includes(role));
}

/* ============================================
   7. 視圖切換
   ============================================ */
function showView(viewId) {
  ['auth-view', 'loading-view', 'app-view', 'error-view'].forEach((id) => {
    const el = document.getElementById(id);
    if (el) el.style.display = id === viewId ? '' : 'none';
  });
}

function showError(msg) {
  const el = document.getElementById('error-message');
  if (el) el.textContent = msg;
  showView('error-view');
}

/* ============================================
   8. 登入畫面
   ============================================ */
function bindLoginForm() {
  const form = document.getElementById('login-form');
  const errBox = document.getElementById('login-error');
  const btn = document.getElementById('login-btn');
  if (!form || form.__bound) return;
  form.__bound = true;

  form.addEventListener('submit', async (e) => {
    e.preventDefault();
    errBox.style.display = 'none';
    btn.disabled = true;
    btn.textContent = '登入中…';
    const account = document.getElementById('account').value.trim();
    const password = document.getElementById('password').value;
    try {
      const user = await loginWithCustomAccount(account, password);
      AppState.setUser(user);
      AppState.setSuperAdmin(isSuperAdmin(user));
      /* 後續由 onAuthStateChanged 接手 */
    } catch (err) {
      console.error(err);
      errBox.textContent = '登入失敗：帳號或密碼錯誤';
      errBox.style.display = 'block';
      btn.disabled = false;
      btn.textContent = '登入';
    }
  });
}

/* ============================================
   9. 渲染 sidebar / navbar
   ============================================ */
function renderSidebar() {
  const navEl = document.getElementById('sidebar-nav');
  if (!navEl) return;

  const role = AppState.getRole();
  const groups = getSidebarGroupsByRole(role);

  let openSet;
  try {
    const raw = localStorage.getItem(STORAGE_KEYS.SIDEBAR_GROUPS);
    openSet = raw ? new Set(JSON.parse(raw)) : new Set(groups.filter((g) => g.defaultOpen).map((g) => g.key));
  } catch (e) {
    openSet = new Set(groups.filter((g) => g.defaultOpen).map((g) => g.key));
  }

  const currentHash = location.hash.replace(/^#/, '') || '/dashboard';
  const groupsHtml = groups.map((g) => {
    const isOpen = openSet.has(g.key);
    const arrow = isOpen ? 'chevron-down' : 'chevron-right';
    const itemsHtml = g.items.map((it) => {
      const active = it.route === currentHash ? 'active' : '';
      return `<a class="nav-item ${active}" href="#${it.route}" data-route="${it.route}">
        <i data-lucide="${it.icon}" class="nav-icon"></i>
        <span class="nav-label">${it.label}</span>
      </a>`;
    }).join('');
    return `<div class="nav-group" data-group="${g.key}">
      <div class="nav-group-title collapsible ${isOpen ? 'open' : ''}" data-group-toggle="${g.key}">
        <span class="nav-group-label">
          <i data-lucide="${g.icon}" class="nav-group-icon"></i>
          <span>${g.label}</span>
        </span>
        <i data-lucide="${arrow}" class="nav-group-arrow"></i>
      </div>
      <div class="nav-group-body" style="display:${isOpen ? 'block' : 'none'};">${itemsHtml}</div>
    </div>`;
  }).join('');

  navEl.innerHTML = groupsHtml;

  navEl.addEventListener('click', (e) => {
    const toggle = e.target.closest('[data-group-toggle]');
    if (toggle) {
      e.preventDefault();
      const key = toggle.dataset.groupToggle;
      const groupEl = navEl.querySelector(`[data-group="${key}"]`);
      if (!groupEl) return;
      const body = groupEl.querySelector('.nav-group-body');
      const title = groupEl.querySelector('.nav-group-title');
      const arrow = title.querySelector('.nav-group-arrow');
      const isOpen = body.style.display === 'block';
      body.style.display = isOpen ? 'none' : 'block';
      title.classList.toggle('open', !isOpen);
      if (arrow) arrow.setAttribute('data-lucide', isOpen ? 'chevron-right' : 'chevron-down');
      if (isOpen) openSet.delete(key); else openSet.add(key);
      try { localStorage.setItem(STORAGE_KEYS.SIDEBAR_GROUPS, JSON.stringify([...openSet])); } catch (err) {}
      if (window.lucide) window.lucide.createIcons();
      return;
    }
    const link = e.target.closest('.nav-item');
    if (link && window.innerWidth < 640) {
      document.querySelector('.sidebar')?.classList.remove('mobile-open');
      document.getElementById('sidebar-backdrop')?.classList.remove('active');
    }
  });

  if (window.lucide) window.lucide.createIcons();
}

function renderNavbar() {
  const titleEl = document.getElementById('navbar-title');
  const userEl = document.getElementById('navbar-user');
  const dbTag = document.getElementById('navbar-db-tag');
  const themeBtn = document.getElementById('navbar-theme-btn');

  const currentHash = location.hash.replace(/^#/, '') || '/dashboard';
  const route = ROUTES[currentHash];
  if (titleEl) titleEl.textContent = route ? route.title : '';

  if (dbTag) {
    const label = DB_LABELS[DB_TARGET] || DB_TARGET;
    dbTag.textContent = `DB: ${label}`;
    dbTag.classList.toggle('new', DB_TARGET === 'new');
  }

  if (userEl) {
    const user = AppState.currentUser;
    const dn = AppState.getDisplayName() || (user?.email?.split('@')[0] || '');
    const role = AppState.getRole();
    const roleBadge = role === 'superadmin'
      ? ' <span class="badge badge-magenta" style="font-size:10px;">超級管理員</span>'
      : (role === 'owner' ? ' <span class="badge badge-info" style="font-size:10px;">👑</span>' : '');
    userEl.innerHTML = user ? `<span class="navbar-user-text">👤 ${dn}${roleBadge}</span>` : '';
  }

  if (themeBtn && !themeBtn.__bound) {
    themeBtn.__bound = true;
    themeBtn.addEventListener('click', () => {
      AppState.toggleTheme();
      const icon = AppState.getTheme() === 'dark' ? 'sun' : 'moon';
      themeBtn.innerHTML = `<i data-lucide="${icon}"></i>`;
      if (window.lucide) window.lucide.createIcons();
    });
  }
  const themeIcon = AppState.getTheme() === 'dark' ? 'sun' : 'moon';
  if (themeBtn) themeBtn.innerHTML = `<i data-lucide="${themeIcon}"></i>`;

  if (window.lucide) window.lucide.createIcons();
}

function bindShellEvents() {
  const hamburger = document.getElementById('hamburger-btn');
  const backdrop = document.getElementById('sidebar-backdrop');
  if (hamburger && !hamburger.__bound) {
    hamburger.__bound = true;
    hamburger.addEventListener('click', () => {
      const sidebar = document.querySelector('.sidebar');
      if (!sidebar) return;
      if (window.innerWidth < 640) {
        sidebar.classList.toggle('mobile-open');
        backdrop?.classList.toggle('active');
      } else {
        sidebar.classList.toggle('collapsed');
      }
    });
  }
}

/* ============================================
   10. 路由分派
   ============================================ */
async function renderPage(route) {
  const content = document.getElementById('page-content');
  if (!content) return;

  /* 清理上一頁 */
  if (_currentPage && typeof _currentPage.destroy === 'function') {
    try { _currentPage.destroy(); } catch (e) { console.warn('[router] destroy 失敗：', e); }
  }
  _currentPage = null;
  content.innerHTML = '';

  const routeDef = ROUTES[route];
  if (!routeDef) {
    content.innerHTML = `<div class="glass-card"><div class="empty-state">未知的路由：${route}</div></div>`;
    return;
  }

  /* 權限檢查 */
  if (route === '/admin' && !AppState.isSuperAdmin) {
    content.innerHTML = `<div class="glass-card"><div class="empty-state">無權限存取</div></div>`;
    return;
  }

  const loader = _pageLoaders[route];
  if (typeof loader !== 'function') {
    content.innerHTML = `<div class="glass-card"><div class="empty-state">
      <p style="font-size:16px; margin-bottom:8px;">${routeDef.title}</p>
      <p style="font-size:13px;">載入中…（第 2、3 回交付後可用）</p>
    </div></div>`;
    return;
  }

  try {
    _currentPage = await loader(content, { AppState });
  } catch (err) {
    console.error(`[router] 頁面載入失敗 ${route}：`, err);
    content.innerHTML = `<div class="glass-card"><div class="empty-state text-red">
      頁面載入失敗：${err.message || err}
    </div></div>`;
  }
}

function handleRouteChange() {
  const route = location.hash.replace(/^#/, '') || '/dashboard';
  if (!location.hash) {
    location.replace('#/dashboard');
    return;
  }
  renderNavbar();
  renderPage(route);
  /* 更新側邊欄 active 狀態 */
  document.querySelectorAll('.nav-item').forEach((a) => {
    a.classList.toggle('active', a.dataset.route === route);
  });
}

/* ============================================
   11. 啟動序列
   ============================================ */
async function boot(user) {
  showView('loading-view');
  const loadingMsg = document.getElementById('loading-message');

  try {
    AppState.init();
    AppState.setUser(user);
    AppState.setSuperAdmin(isSuperAdmin(user));

    if (isSuperAdmin(user)) {
      AppState.setRole('superadmin');
      AppState.setCanInput(true);
      AppState.setDisplayName('超級管理員');
    } else {
      /* 第 2 回接入 app-data.js 的 lookupFamily；先以最小資訊渲染 */
      AppState.setRole('member');
      AppState.setCanInput(true);
      AppState.setDisplayName(user.email?.split('@')[0] || '成員');
      AppState.setMemberAccount({
        email: user.email || '',
        displayName: AppState.getDisplayName(),
        role: 'member',
        canInput: true,
      });
    }

    if (loadingMsg) loadingMsg.textContent = '渲染中…';

    showView('app-view');
    renderSidebar();
    renderNavbar();
    bindShellEvents();

    if (!location.hash) location.replace('#/dashboard');
    handleRouteChange();

    window.addEventListener('hashchange', handleRouteChange);
  } catch (err) {
    console.error('[boot] 失敗：', err);
    showError(String(err.message || err));
  }
}

/* ============================================
   12. 入口
   ============================================ */
function init() {
  showView('loading-view');
  bindLoginForm();

  document.getElementById('error-retry-btn')?.addEventListener('click', () => location.reload());

  onAuthStateChanged(auth, (user) => {
    if (user) {
      boot(user);
    } else {
      AppState.currentUser = null;
      showView('auth-view');
      bindLoginForm();
    }
  });
}

if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', init);
} else {
  init();
}

/* 對外匯出（第 2/3 回使用） */
export { ROUTES, STORAGE_KEYS, DB_LABELS, auth, firebaseApp };```

<!-- === FILE END: ./js/app-core.js === -->

<!-- === FILE START: ./js/app-data.js === -->

## 📄 ./js/app-data.js

```js
// ============================================
// app-data.js — SPA 資料層
// 職責：Firebase 封裝 / API client / status registry / entity name registry / code 生成
// 契約：API 路徑與格式 100% 沿用舊版；Firebase 路徑不動
// ============================================

import { initializeApp, getApps, getApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
import { getAuth } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
import {
  getDatabase, ref, get, onValue, push, set, update, remove,
} from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { AppState, DB_TARGET } from './app-core.js';

/* ============================================
   1. Firebase 取得（app-core 已 init，這裡取實例）
   ============================================ */
const _app = getApps().length ? getApp() : null;
export const db = _app ? getDatabase(_app) : null;
export const auth = _app ? getAuth(_app) : null;

/* ============================================
   2. Firebase RTDB 封裝
   ============================================ */
export function familyPath(subpath) {
  const fid = AppState.getFamilyId();
  if (!fid) throw new Error('尚未選擇家庭');
  return `families/${fid}/${subpath}`;
}

export function familyRef(subpath) { return ref(db, familyPath(subpath)); }

export function listen(subpath, callback, onError) {
  const fid = AppState.getFamilyId();
  if (!fid) { if (onError) onError(new Error('尚未選擇家庭')); return () => {}; }
  return onValue(ref(db, `families/${fid}/${subpath}`), callback, (err) => {
    console.error(`[app-data] 讀取失敗 [${subpath}]：`, err);
    if (onError) onError(err);
  });
}

export function listenList(subpath, sortFn, callback, onError) {
  return listen(subpath, (snap) => {
    const v = snap.val() || {};
    const list = Object.entries(v).map(([id, x]) => ({ id, ...x }));
    if (typeof sortFn === 'function') list.sort(sortFn);
    callback(list);
  }, onError);
}

export async function getOnce(subpath) {
  const snap = await get(familyRef(subpath));
  return snap.val();
}

export async function getListOnce(subpath, sortFn) {
  const v = (await getOnce(subpath)) || {};
  const list = Object.entries(v).map(([id, x]) => ({ id, ...x }));
  if (typeof sortFn === 'function') list.sort(sortFn);
  return list;
}

export async function pushItem(subpath, data) {
  const r = push(familyRef(subpath));
  await set(r, data);
  return r.key;
}

export async function updateItem(subpath, id, patch) {
  if (!id) throw new Error('缺少 id');
  await update(familyRef(`${subpath}/${id}`), patch);
}

export async function putItem(subpath, id, data) {
  if (!id) throw new Error('缺少 id');
  await set(familyRef(`${subpath}/${id}`), data);
}

export async function removeItem(subpath, id) {
  if (!id) throw new Error('缺少 id');
  await remove(familyRef(`${subpath}/${id}`));
}

export async function setSubpath(subpath, data) {
  await set(familyRef(subpath), data);
}

export async function removeSubpath(subpath) {
  await remove(familyRef(subpath));
}

/* ============================================
   3. API Client（路徑、格式 100% 沿用舊版）
   ============================================ */
async function callApi(path, opts = {}) {
  const user = auth.currentUser;
  const token = user ? await user.getIdToken() : '';
  const headers = { ...(opts.headers || {}), 'Authorization': `Bearer ${token}` };

  let res;
  try { res = await fetch(path, { ...opts, headers }); }
  catch (err) { throw new Error(`網路錯誤（${path}）：${err.message}`); }

  if (res.status === 401) {
    localStorage.removeItem('fin_family_id');
    location.hash = '#/';
    throw new Error('登入已過期，請重新登入');
  }
  if (res.status === 403) {
    const t = await res.text().catch(() => '');
    let p = null; try { p = JSON.parse(t); } catch (e) {}
    throw new Error(p?.message || t || '無權限存取');
  }
  if (!res.ok) {
    const t = await res.text().catch(() => '');
    let p = null; try { p = JSON.parse(t); } catch (e) {}
    const msg = p?.message || p?.error || t || '未知錯誤';
    const err = new Error(msg); err.code = p?.error || 'UNKNOWN'; err.status = res.status;
    throw err;
  }
  return res.json();
}

function _fid() {
  const id = AppState.getFamilyId();
  if (!id) throw new Error('尚未選擇家庭');
  return id;
}

export const api = {
  lookupFamily: () => callApi('/api/lookup-family', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: '{}' }),
  summary: (year, month) => callApi(`/api/summary?familyId=${_fid()}&year=${year}&month=${month}`),
  fetchAnnualSummary: async (year) => {
    const r = await callApi(`/api/annual-summary?familyId=${_fid()}&startYear=${year}&endYear=${year}`);
    const d = r.years?.[0] || { monthly: [], totalIncome: 0, totalExpense: 0, netBalance: 0, yearlyInsuranceTotal: 0, totalAssets: 0, bankBalance: 0, fundValue: 0 };
    return { year, monthly: d.monthly || [], totalIncome: d.totalIncome || 0, totalExpense: d.totalExpense || 0, netBalance: d.netBalance || 0, yearlyInsuranceTotal: d.yearlyInsuranceTotal || 0, totalAssets: d.totalAssets || 0, bankBalance: d.bankBalance || 0, fundValue: d.fundValue || 0 };
  },
  fetchAnnualSummaryRange: (s, e) => callApi(`/api/annual-summary?familyId=${_fid()}&startYear=${s}&endYear=${e}`),
  fetchSettlementsYear: (year) => callApi(`/api/settlements-year?familyId=${_fid()}&year=${year}`),
  insuranceSync: (p) => callApi('/api/insurance-sync', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ ...p, familyId: _fid(), action: 'upsert' }) }),
  insuranceUnsync: (p) => callApi('/api/insurance-sync', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ ...p, familyId: _fid(), action: 'delete' }) }),
  entityCreate: (p) => callApi('/api/entity-create', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ ...p, familyId: _fid() }) }),

  adminListFamilies: () => callApi('/api/admin-families?action=list'),
  adminAddFamily: (uid, name, email) => callApi('/api/admin-families', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'add', uid, name, email }) }),
  adminRemoveFamily: (uid) => callApi('/api/admin-families', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'remove', uid }) }),
  adminInitFamily: (uid) => callApi('/api/admin-init-family', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ uid }) }),
  adminMigrate: (fid) => callApi('/api/admin-migrate', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: fid, confirm: 'MIGRATE_ALL' }) }),

  familyAccounts: {
    list: (fid) => callApi(`/api/family-accounts?familyId=${fid}&action=list`),
    create: (fid, d) => callApi('/api/family-accounts', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'create', familyId: fid, ...d }) }),
    restore: (fid, d) => callApi('/api/family-accounts', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'restore', familyId: fid, ...d }) }),
    update: (fid, uid, d) => callApi('/api/family-accounts', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'update', familyId: fid, uid, ...d }) }),
    remove: (fid, uid) => callApi('/api/family-accounts', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'remove', familyId: fid, uid }) }),
  },

  platformSettings: {
    get: () => callApi('/api/platform-settings'),
    update: (d) => callApi('/api/platform-settings', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'update', data: d }) }),
  },
  platformDefaults: {
    list: (r) => callApi(`/api/platform-defaults?action=list&resource=${r}`),
    put: (r, id, d) => callApi('/api/platform-defaults', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'put', resource: r, id, data: d }) }),
    remove: (r, id) => callApi('/api/platform-defaults', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'delete', resource: r, id }) }),
    set: (r, d) => callApi('/api/platform-defaults', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ action: 'set', resource: r, data: d }) }),
  },
  familySettings: {
    get: () => callApi(`/api/family-settings?familyId=${_fid()}`),
    update: (d) => callApi('/api/family-settings', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), action: 'update', data: d }) }),
    putStatus: (id, d) => callApi('/api/family-settings', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), action: 'put-status', id, data: d }) }),
    removeStatus: (id) => callApi('/api/family-settings', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), action: 'delete-status', id }) }),
  },
  bankAccounts: {
    list: () => callApi(`/api/bank-accounts?familyId=${_fid()}&action=list`),
    create: (d) => callApi('/api/bank-accounts', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), action: 'create', data: d }) }),
    update: (id, d) => callApi('/api/bank-accounts', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), action: 'update', id, data: d }) }),
    remove: (id) => callApi('/api/bank-accounts', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), action: 'remove', id }) }),
  },
  bankTransactions: {
    list: (bankId) => callApi(`/api/bank-transactions?familyId=${_fid()}&bankId=${bankId}&action=list`),
    listAll: () => callApi(`/api/bank-transactions?familyId=${_fid()}&action=listAll`),
    create: (bid, d) => callApi('/api/bank-transactions', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), bankId: bid, action: 'create', data: d }) }),
    update: (bid, tid, d) => callApi('/api/bank-transactions', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), bankId: bid, txnId: tid, action: 'update', data: d }) }),
    remove: (bid, tid) => callApi('/api/bank-transactions', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), bankId: bid, txnId: tid, action: 'remove' }) }),
  },
  personalIncome: {
    list: (mid) => callApi(`/api/personal-income?familyId=${_fid()}&memberId=${mid}&action=list`),
    save: (mid, y, m, a) => callApi('/api/personal-income', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), memberId: mid, year: y, month: m, amount: a, action: 'save' }) }),
    remove: (mid, y, m) => callApi('/api/personal-income', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), memberId: mid, year: y, month: m, action: 'remove' }) }),
  },
  memberAdvances: {
    list: (mid) => callApi(`/api/member-advances?familyId=${_fid()}&memberId=${mid}&action=list`),
    listAll: () => callApi(`/api/member-advances?familyId=${_fid()}&action=listAll`),
    create: (mid, d) => callApi('/api/member-advances', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), memberId: mid, action: 'create', data: d }) }),
    update: (mid, aid, d) => callApi('/api/member-advances', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), memberId: mid, advanceId: aid, action: 'update', data: d }) }),
    remove: (mid, aid) => callApi('/api/member-advances', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), memberId: mid, advanceId: aid, action: 'remove' }) }),
  },
  clearBankBalances: () => callApi('/api/clear-bank-balances', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ familyId: _fid(), confirm: 'CONFIRM_DELETE' }) }),
};

/* ============================================
   4. Status Registry（沿用舊版 status-registry.js）
   ============================================ */
export const STATUS = { PENDING: 'pending', DONE: 'done', SKIPPED: 'skipped' };
const _IS_DONE = { pending: false, done: true, skipped: true };
const _LABELS = {
  default: { pending: '未處理', done: '已處理', skipped: '不適用' },
  personal:  { pending: '未付款', done: '已付款', skipped: '不適用' },
  fixed:     { pending: '未付款', done: '已付款', skipped: '不適用' },
  insurance: { pending: '未扣款', done: '已扣款', skipped: '不適用' },
  income:    { pending: '未轉入', done: '已轉入', skipped: '不適用' },
};
const _LEGACY_MAP = {
  '未處理': 'pending', '未還款': 'pending', '未付款': 'pending', '未扣款': 'pending', '未轉入': 'pending',
  '已處理': 'done', '已還款': 'done', '已付款': 'done', '已扣款': 'done', '已轉入': 'done',
  '不適用': 'skipped',
};
const _BADGE = { pending: 'badge-pending', done: 'badge-success', skipped: 'badge-muted' };

export function statusNormalize(raw) {
  if (raw == null || raw === '') return STATUS.PENDING;
  if (raw === 'pending' || raw === 'done' || raw === 'skipped') return raw;
  return _LEGACY_MAP[raw] || STATUS.PENDING;
}
export function statusIsDone(raw) { return !!_IS_DONE[statusNormalize(raw)]; }
export function statusLabel(raw, opts = {}) {
  const c = statusNormalize(raw);
  const src = opts.source;
  if (src && _LABELS[src] && _LABELS[src][c]) return _LABELS[src][c];
  return _LABELS.default[c] || _LABELS.default.pending;
}
export function statusOptions(opts = {}) {
  const codes = ['pending', 'done'];
  if (opts.includeSkipped) codes.push('skipped');
  return codes.map((c) => ({ value: c, label: statusLabel(c, { source: opts.source }) }));
}
export function statusBadgeClass(raw) { return _BADGE[statusNormalize(raw)] || _BADGE.pending; }
export function statusToLegacyName(code, source = 'personal') {
  const c = statusNormalize(code);
  const m = _LABELS[source] || _LABELS.personal;
  return m[c] || m.pending;
}

/* ============================================
   5. Entity Name Registry（沿用舊版 entity-registry.js）
   ============================================ */
const _PATHS = {
  members: 'members', banks: 'banks', bank_accounts: 'bank_accounts',
  categories: 'expense_categories', items: 'expense_items',
  companies: 'insurance_companies', payments: 'payment_methods',
};
const _lists = {};
const _nameMaps = {};
const _unsubs = [];
let _initialized = false;

function _byOrderThenCreated(a, b) {
  const oa = a.order != null ? a.order : Number.MAX_SAFE_INTEGER;
  const ob = b.order != null ? b.order : Number.MAX_SAFE_INTEGER;
  if (oa !== ob) return oa - ob;
  return (a.createdAt || 0) - (b.createdAt || 0);
}
function _byCreatedAt(a, b) { return (a.createdAt || 0) - (b.createdAt || 0); }

export function initEntityRegistry() {
  if (_initialized) return true;
  const fid = AppState.getFamilyId();
  if (!fid) return false;

  Object.entries(_PATHS).forEach(([k, p]) => {
    const sortFn = ['members', 'banks', 'bank_accounts'].includes(k) ? _byOrderThenCreated : _byCreatedAt;
    const unsub = onValue(ref(db, `families/${fid}/${p}`), (snap) => {
      const v = snap.val() || {};
      const list = Object.entries(v).map(([id, x]) => ({ id, ...x }));
      list.sort(sortFn);
      _lists[k] = list;
      const map = {};
      list.forEach((x) => { map[x.id] = x.name || ''; });
      _nameMaps[k] = map;
    }, (err) => { console.warn(`[entityRegistry] ${k} 讀取失敗：`, err); });
    _unsubs.push(unsub);
  });

  _initialized = true;
  return true;
}

export function destroyEntityRegistry() {
  _unsubs.forEach((u) => { try { u(); } catch (e) {} });
  _unsubs.length = 0;
  Object.keys(_lists).forEach((k) => delete _lists[k]);
  Object.keys(_nameMaps).forEach((k) => delete _nameMaps[k]);
  _initialized = false;
}

export function resolveName(type, id, fallback = '') {
  if (!id) return fallback;
  return (_nameMaps[type] || {})[id] || fallback;
}
export function getList(type) { return [...(_lists[type] || [])]; }

/* ============================================
   6. Code Registry（沿用舊版 code-registry.js）
   ============================================ */
export const PREFIX_MAP = {
  member: 'MEM', bank: 'PAY-BANK', bankAccount: 'PAY-BANK',
  category: 'CAT', item: 'ITEM', payment: 'PAY', company: 'COMP',
  policy: 'POL', fund: 'FUND', income: 'INC', bankTxn: 'TXN',
};
export const POLICY_SUBTYPE_MAP = {
  normal: 'NORM', fund_insurance: 'FUND', savings: 'SAVE',
};
export const CATEGORY_SHORTCODES = {
  '學校類': 'SCH', '交通類': 'TRA', '醫療類': 'MED', '銀行類': 'BNK', '其他': 'OTH',
};

export function generateCode(prefix, existingList) {
  if (!prefix) throw new Error('generateCode 缺少 prefix');
  if (!Array.isArray(existingList)) existingList = [];
  const pd = prefix + '-';
  const seqs = [];
  existingList.forEach((item) => {
    const code = item?.code;
    if (!code || typeof code !== 'string' || !code.startsWith(pd)) return;
    const rest = code.slice(pd.length);
    const parts = rest.split('-');
    const n = parseInt(parts[parts.length - 1], 10);
    if (!isNaN(n) && n > 0) seqs.push(n);
  });
  const max = seqs.length ? Math.max(...seqs) : 0;
  return pd + String(max + 1).padStart(3, '0');
}

export function generateWithSubtype(prefix, subtype, list) {
  if (!subtype) return generateCode(prefix, list);
  return generateCode(`${prefix}-${subtype}`, list);
}

export function getSubtypeFromContext(entityType, extra = {}) {
  switch (entityType) {
    case 'policy': return POLICY_SUBTYPE_MAP[extra.type || 'normal'] || 'NORM';
    case 'category': return CATEGORY_SHORTCODES[(extra.name || '').trim()] || null;
    case 'fund': return extra.type === 'insurance' ? 'INS' : null;
    default: return null;
  }
}

export function checkDuplicateCode(code, list) {
  if (!Array.isArray(list)) return false;
  return list.some((x) => x?.code === code);
}

const _CODE_FETCHERS = {
  member: () => getList('members'),
  bank: () => getList('banks'),
  bankAccount: () => getList('bank_accounts'),
  category: () => getList('categories'),
  item: () => getList('items'),
  payment: () => getList('payments'),
  company: () => getList('companies'),
  policy: async () => await getListOnce('insurance_policies'),
  fund: async () => await getListOnce('funds'),
};

export async function generateCodeFor(entityType, extra = {}) {
  const prefix = PREFIX_MAP[entityType];
  if (!prefix) throw new Error(`未知 entityType：${entityType}`);
  const fetcher = _CODE_FETCHERS[entityType];
  const list = fetcher ? await fetcher() : [];
  const subtype = getSubtypeFromContext(entityType, extra);
  const code = subtype ? generateWithSubtype(prefix, subtype, list) : generateCode(prefix, list);
  if (checkDuplicateCode(code, list)) throw new Error(`編號重複：${code}，請重試`);
  return { code, prefix, subtype };
}```

<!-- === FILE END: ./js/app-data.js === -->

<!-- === FILE START: ./js/app-domain.js === -->

## 📄 ./js/app-domain.js

```js
// ============================================
// app-domain.js — SPA 業務計算層
// 職責：保險 / 基金 / 銀行 / 年度統計 / 結算合併
// 凍結：FZ-001~005 逐字搬入，禁止修改公式
// ============================================

import { AppState } from './app-core.js';
import { statusIsDone, statusLabel, statusNormalize, statusToLegacyName } from './app-data.js';

/* ============================================
   1. 通用
   ============================================ */
const _round = (v) => Math.round(Number(v) || 0);

/* ============================================
   2. 銀行餘額（FZ-001 逐字搬入）
   來源：_summary.js _calcBankTotal
   ============================================ */
export function calcBankBalance(bankAccounts, transactions, targetYear, targetMonth) {
  if (!targetYear || !targetMonth) return 0;
  const tY = Number(targetYear);
  const tM = Number(targetMonth);
  let total = 0;

  (bankAccounts || []).forEach((acc) => {
    const init = Number(acc.initialBalance) || 0;
    const initY = Number(acc.initialYear) || 0;
    const initM = Number(acc.initialMonth) || 0;
    if (!initY || !initM) { total += init; return; }
    if (tY < initY || (tY === initY && tM < initM)) { total += init; return; }

    let balance = init;
    (transactions || []).filter((t) => t.bankId === acc.id).forEach((txn) => {
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
}

/* ============================================
   3. 保險（FZ-002 / FZ-003 逐字搬入）
   ============================================ */

// FZ-002 月平攤
export function calcMonthlyExpense(data) {
  const policyType = data.type || 'normal';
  const paymentType = data.paymentType || '年繳';
  const ty = Number(data.totalPolicyYears) || 1;
  if (policyType === 'fund_insurance') return Math.round(Number(data.monthlyPremium) || 0);
  switch (paymentType) {
    case '年繳':     return Math.round((Number(data.annualPremium) || 0) / 12);
    case '月繳':     return Math.round(Number(data.monthlyPremium) || 0);
    case '一次付款': return Math.round((Number(data.oncePremium) || 0) / ty / 12);
    default:         return 0;
  }
}

// FZ-003 已供滿
export function isPolicyCompleted(policy) {
  if (!policy) return false;
  if (policy.isCompleted) return true;
  if (policy.type === 'fund_insurance') return false;
  const total = Number(policy.totalPolicyPeriods) || 0;
  if (total <= 0) return false;
  const done = Number(policy.completedPeriods) || 0;
  return done >= total;
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
  };
}

export function getPeriodInfo(policy, year, month) {
  const firstY = Number(policy.firstStartYear) || 0;
  const firstM = Number(policy.firstStartMonth) || 1;
  if (!firstY || firstY < 2000 || firstY > 2100) return null;
  if (!firstM || firstM < 1 || firstM > 12) return null;
  const startDate = new Date(firstY, firstM - 1, 1);
  const curDate = new Date(Number(year), Number(month) - 1, 1);
  const diff = (curDate.getFullYear() - startDate.getFullYear()) * 12 + (curDate.getMonth() - startDate.getMonth());
  if (diff < 0) return null;
  const periodIndex = Math.floor(diff / 12) + 1;
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
  if (policy.type === 'fund_insurance') return Math.round((Number(policy.monthlyPremium) || 0) * 12);
  const periods = policy.periods || {};
  const p = periods[String(periodIndex)];
  if (p && p.annualPremium) return Math.round(Number(p.annualPremium));
  const keys = Object.keys(periods).map(Number).filter((n) => !isNaN(n) && n > 0).sort((a, b) => a - b);
  if (keys.length) {
    const below = keys.filter((k) => k <= periodIndex);
    const target = below.length ? below[below.length - 1] : keys[0];
    const tp = periods[String(target)];
    if (tp && tp.annualPremium) return Math.round(Number(tp.annualPremium));
  }
  return Math.round(Number(policy.annualPremium) || 0);
}

export function getPolicyTotalPremium(policy) {
  if (!policy) return 0;
  if (policy.type === 'fund_insurance') {
    return Math.round((Number(policy.monthlyPremium) || 0) * 12 * (Number(policy.totalPolicyYears) || 0));
  }
  const totalYears = Number(policy.totalPolicyYears) || 0;
  if (totalYears === 0) return Math.round(Number(policy.annualPremium) || 0);
  const periods = policy.periods || {};
  const keys = Object.keys(periods).map(Number).filter((n) => !isNaN(n) && n > 0).sort((a, b) => a - b);
  let fb = Math.round(Number(policy.annualPremium) || 0);
  if (!fb && keys.length) {
    const last = periods[String(keys[keys.length - 1])];
    if (last?.annualPremium) fb = Math.round(Number(last.annualPremium));
  }
  let total = 0;
  for (let i = 1; i <= totalYears; i++) {
    const p = periods[String(i)];
    total += (p && p.annualPremium) ? Math.round(Number(p.annualPremium)) : fb;
  }
  return total;
}

export function getPolicyPaidTotal(payments) {
  let total = 0;
  Object.values(payments || {}).forEach((yData) => {
    Object.values(yData || {}).forEach((m) => {
      if (m && statusIsDone(m.status)) total += Math.round(Number(m.amount) || 0);
    });
  });
  return total;
}

export function countCompletedPeriods(payments) {
  let c = 0;
  Object.values(payments || {}).forEach((yData) => {
    Object.values(yData || {}).forEach((m) => { if (m && statusIsDone(m.status)) c++; });
  });
  return c;
}

export function calcProgress(policy) {
  const t = Number(policy.totalPolicyPeriods) || 0;
  const d = Number(policy.completedPeriods) || 0;
  if (t <= 0) return 0;
  return Math.min(100, Math.round((d / t) * 100));
}

export function resolveMonthlyAmount(policy, year, month, paymentData) {
  if (!policy) return 0;
  if (paymentData?.amount && Number(paymentData.amount) > 0) return Math.round(Number(paymentData.amount));
  if (policy.type === 'fund_insurance') return Math.round(Number(policy.monthlyPremium) || 0);
  const info = getPeriodInfo(policy, year, month);
  if (info) {
    const pd = (policy.periods || {})[String(info.periodIndex)];
    if (pd && pd.monthlyAverage) return Math.round(Number(pd.monthlyAverage));
  }
  return Math.round(Number(policy.monthlyAverage) || 0);
}

/* ============================================
   4. 基金（FZ-004 / FZ-005 逐字搬入）
   ============================================ */

// FZ-004 基金成本估算
export function calcFundCostEstimate(policy, year, month) {
  if (!policy || policy.type !== 'fund_insurance') return 0;
  const monthly = Number(policy.monthlyPremium) || 0;
  if (monthly <= 0) return 0;
  const fy = Number(policy.firstStartYear) || 0;
  const fm = Number(policy.firstStartMonth) || 1;
  if (!fy) return 0;
  const startDate = new Date(fy, fm - 1, 1);
  const curDate = new Date(Number(year), Number(month) - 1, 1);
  const diffMonths = (curDate.getFullYear() - startDate.getFullYear()) * 12 + (curDate.getMonth() - startDate.getMonth());
  if (diffMonths < 0) return 0;
  const paidMonths = diffMonths + 1;
  return Math.round(monthly * paidMonths);
}

export function calcFundCostFromPayments(payments) { return getPolicyPaidTotal(payments); }

// FZ-005 手動優先＋比例分配
export function getFundCostMap(policy, allFunds, targetYear, targetMonth) {
  const costMap = {};
  if (!policy) return costMap;
  const policyFunds = (allFunds || []).filter((f) => {
    if (f.type !== 'insurance') return false;
    if (f.policyId === policy.id) return true;
    return (policy.fundsAllocation || []).some((a) => a.fundId === f.id);
  });
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

export function findPolicyByFund(fund, policies) {
  if (fund.policyId) {
    const p = (policies || []).find((x) => x.id === fund.policyId);
    if (p) return p;
  }
  return (policies || []).find((p) => (p.fundsAllocation || []).some((a) => a.fundId === fund.id)) || null;
}

/* ============================================
   5. 年度統計（合 4 處為一，SSOT 化）
   ============================================ */
export function computeYearTotals(allIncome, allExpenses, year) {
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

export function computeAnnualOverview(allIncome, allExpenses, policies, currentYear) {
  const curY = Number(currentYear) || new Date().getFullYear();
  const startY = curY - 2, endY = curY + 2;
  const rows = [];
  for (let y = startY; y <= endY; y++) {
    const yk = String(y);
    const yi = (allIncome || {})[yk] || {};
    const ye = (allExpenses || {})[yk] || {};
    let totalIncome = 0;
    Object.values(yi).forEach((md) => Object.values(md || {}).forEach((v) => { totalIncome += Number(v) || 0; }));
    let totalExpense = 0, monthsWithData = 0;
    Object.values(ye).forEach((md) => {
      const me = (md && md.member_expenses) || {};
      let s = 0;
      Object.values(me).forEach((items) => Object.values(items || {}).forEach((e) => { s += Number(e && e.amount) || 0; }));
      if (s > 0) monthsWithData++;
      totalExpense += s;
    });
    let insurance = 0;
    (policies || []).forEach((p) => { try { insurance += getPolicyAnnualPremium(p, y) || 0; } catch (e) {} });
    const net = totalIncome - totalExpense - insurance;
    rows.push({
      year: y,
      totalIncome: Math.round(totalIncome),
      totalExpense: Math.round(totalExpense),
      insurance: Math.round(insurance),
      net: Math.round(net),
      avg: monthsWithData > 0 ? Math.round(totalExpense / monthsWithData) : 0,
      isCurrent: y === curY,
    });
  }
  return rows;
}

export function computeMemberAnnualRows(members, allIncome, allExpenses, allPersonalIncome, year) {
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
  const SHARED = 'shared';
  let se = 0;
  Object.values(ye).forEach((md) => {
    const s = (md && md.member_expenses && md.member_expenses[SHARED]) || {};
    Object.values(s).forEach((e) => { se += Number(e && e.amount) || 0; });
  });
  if (se > 0) rows.push({ id: SHARED, name: '家庭共用', income: 0, personalIncome: 0, expense: Math.round(se), net: -Math.round(se), __isShared: true });
  const tot = rows.reduce((s, r) => ({ income: s.income + r.income, personalIncome: s.personalIncome + r.personalIncome, expense: s.expense + r.expense, net: s.net + r.net }), { income: 0, personalIncome: 0, expense: 0, net: 0 });
  rows.push({ id: '__total__', name: '總計', ...tot, __isTotal: true });
  return rows;
}

export function computeMemberReportRows(members, allIncome, allExpenses, allPersonalIncome, year) {
  const yk = String(year || '');
  const yi = (allIncome || {})[yk] || {};
  const ye = (allExpenses || {})[yk] || {};
  const SHARED = 'shared';
  const list = (members || []).map((m) => ({ id: m.id, name: m.name, isShared: false }));
  list.push({ id: SHARED, name: '家庭共用', isShared: true });

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

/* ============================================
   6. 結算合併（沿用舊版 merge.js）
   ============================================ */
let _insuranceStatusNames = null;
try {
  AppState.on('family-change', () => { _insuranceStatusNames = null; });
} catch (e) {}

function _getInsStatusNames() {
  if (_insuranceStatusNames) return _insuranceStatusNames;
  _insuranceStatusNames = new Set(['未扣款', '已扣款']);
  return _insuranceStatusNames;
}

function _resolveInsStatus(...cands) {
  const valid = _getInsStatusNames();
  for (const c of cands) { if (c && valid.has(c)) return c; }
  return statusToLegacyName('pending', 'insurance');
}

export function mergeSettlementData({ memberExpenses = [], insuranceRows = [], year, month }) {
  const rows = [];
  const SHARED = 'shared';

  (memberExpenses || []).filter((e) => !e.isAutoLinked).forEach((e) => {
    rows.push({
      key: `personal-${year}-${month}-${e.memberId}-${e.id}`,
      source: 'personal',
      sourceLabel: e.memberId === SHARED ? '🏠 家庭' : '🏷 個人',
      year, month, memberId: e.memberId || '',
      name: e.name || '（未命名）',
      amount: Number(e.amount) || 0,
      status: e.status || '未處理',
      isDone: statusIsDone(e.status),
      date: e.date || '',
      paymentMethodId: e.paymentMethodId || '',
      bankId: e.bankId || '', txnId: e.txnId || '',
      _ref: { memberId: e.memberId, expenseId: e.id },
    });
  });

  const linkedMap = {};
  (memberExpenses || []).filter((e) => e.isAutoLinked).forEach((e) => {
    const pid = e.policyId || (typeof e.id === 'string' && e.id.startsWith('linked_') ? e.id.slice(7) : '');
    if (pid) linkedMap[pid] = e;
  });
  const allPolicyIds = new Set([...Object.keys(linkedMap), ...(insuranceRows || []).map((p) => p.policyId)]);

  allPolicyIds.forEach((pid) => {
    const linked = linkedMap[pid];
    const insRow = (insuranceRows || []).find((p) => p.policyId === pid);
    const effMid = insRow?.policyHolderId || insRow?.memberId || linked?.memberId || '';
    const status = _resolveInsStatus(insRow?.status, linked?.status);
    rows.push({
      key: `insurance-${year}-${month}-${pid}`,
      source: 'insurance',
      sourceLabel: '🛡 保險',
      year, month, memberId: effMid,
      name: insRow?.policyName || linked?.name || '（未命名保單）',
      amount: Number(linked?.amount ?? insRow?.amount) || 0,
      status, isDone: statusIsDone(status),
      date: linked?.date || insRow?.date || '',
      bankId: linked?.bankId || insRow?.bankId || '',
      txnId: linked?.txnId || insRow?.txnId || '',
      paymentMode: insRow?.paymentMode || 'direct',
      advanceHolderId: insRow?.advanceHolderId || '',
      _ref: { policyId: pid, memberId: effMid, linkedId: linked?.id },
    });
  });

  return rows;
}```

<!-- === FILE END: ./js/app-domain.js === -->

<!-- === FILE START: ./js/app-entities.js === -->

## 📄 ./js/app-entities.js

```js
// ============================================
// app-entities.js — 9 種實體定義 SSOT
// 職責：fields / ui / validate / fromForm / toForm / codePrefix / openEntityModal
// ============================================

import { api, generateCodeFor, resolveName } from './app-data.js';
import { esc, formatHKD, formatNumber, buildForm, openModal, closeModal, toast } from './app-ui.js';

const LIMITS = { NAME_MAX: 60, ORDER_MIN: 0 };
const PAYMENT_TYPES = { ANNUAL: '年繳', MONTHLY: '月繳', ONCE: '一次付款' };
const POLICY_SUBTYPES = { normal: 'NORM', fund_insurance: 'FUND', savings: 'SAVE' };

function _stripCommon(payload) {
  const extra = {};
  ['name', 'code', 'createdAt', 'updatedAt'].forEach(() => {});
  Object.entries(payload || {}).forEach(([k, v]) => {
    if (!['name', 'code', 'createdAt', 'updatedAt'].includes(k)) extra[k] = v;
  });
  return extra;
}

export const ENTITIES = {
  member: {
    key: 'member', label: '成員', icon: 'users', codePrefix: 'MEM',
    fields: [
      { id: 'name', label: '名稱', type: 'text', required: true, maxlength: 20, placeholder: '例如：老公、梓舜' },
      { id: 'role', label: '角色', type: 'select', required: true, optionsSource: 'memberRoles' },
      { id: 'order', label: '排序', type: 'number-plain', min: 0, defaultValue: 0 },
    ],
    listColumns: ['name', 'role', 'order'], cardFields: ['role', 'order'], primaryColumn: 'name',
  },

  bank: {
    key: 'bank', label: '銀行', icon: 'landmark', codePrefix: 'PAY-BANK',
    fields: [{ id: 'name', label: '銀行名稱', type: 'text', required: true, maxlength: 20 }],
    listColumns: ['name'], cardFields: ['name'], primaryColumn: 'name',
  },

  bankAccount: {
    key: 'bankAccount', label: '銀行帳號', icon: 'landmark', codePrefix: 'PAY-BANK',
    fields: [
      { id: 'name', label: '帳號名稱', type: 'text', required: true, maxlength: 20 },
      { id: 'type', label: '類型', type: 'select', required: true, includeEmpty: false,
        options: [{ value: 'family', label: '家庭帳號' }, { value: 'personal', label: '個人帳號' }], defaultValue: 'family' },
      { id: 'initialBalance', label: '初始餘額（HK$）', type: 'number', required: true, min: 0, defaultValue: 0 },
      { id: 'initialYear', label: '初始年份', type: 'number-plain', required: true, min: 2000, max: 2100, defaultValue: () => new Date().getFullYear() },
      { id: 'initialMonth', label: '初始月份', type: 'select', required: true, optionsSource: 'months', defaultValue: '01' },
      { id: 'order', label: '排序', type: 'number-plain', min: 0, defaultValue: 0 },
    ],
    listColumns: ['name', 'type', 'initialBalance'], cardFields: ['type', 'initialBalance'], primaryColumn: 'name',
  },

  category: {
    key: 'category', label: '支出類別', icon: 'tags', codePrefix: 'CAT',
    fields: [
      { id: 'name', label: '類別名稱', type: 'text', required: true, maxlength: 20 },
      { id: 'shortCode', label: '子類短碼', type: 'text', required: false, maxlength: 5 },
      { id: 'order', label: '排序', type: 'number-plain', min: 0, defaultValue: 0 },
    ],
    listColumns: ['name', 'order'], cardFields: ['order'], primaryColumn: 'name',
  },

  item: {
    key: 'item', label: '支出項目', icon: 'list', codePrefix: 'ITEM',
    fields: [
      { id: 'categoryId', label: '所屬類別', type: 'select', required: true, optionsSource: 'categories' },
      { id: 'name', label: '項目名稱', type: 'text', required: true, maxlength: 30 },
    ],
    listColumns: ['name', 'categoryId'], cardFields: ['categoryId'], primaryColumn: 'name',
  },

  payment: {
    key: 'payment', label: '支付方式', icon: 'credit-card', codePrefix: 'PAY',
    fields: [
      { id: 'name', label: '支付方式名稱', type: 'text', required: true, maxlength: 20 },
      { id: 'order', label: '排序', type: 'number-plain', min: 0, defaultValue: 0 },
    ],
    listColumns: ['name', 'order'], cardFields: ['order'], primaryColumn: 'name',
  },

  company: {
    key: 'company', label: '保險公司', icon: 'shield', codePrefix: 'COMP',
    fields: [{ id: 'name', label: '公司名稱', type: 'text', required: true, maxlength: 30 }],
    listColumns: ['name'], cardFields: ['name'], primaryColumn: 'name',
  },

  policy: {
    key: 'policy', label: '保單', icon: 'shield', codePrefix: 'POL',
    fields: [
      { id: 'type', label: '保單類型', type: 'select', required: true, optionsSource: 'policyTypes' },
      { id: 'memberId', label: '受保人', type: 'select', required: true, optionsSource: 'members' },
      { id: 'policyHolderId', label: '保單持有人', type: 'select', required: false, optionsSource: 'members', emptyText: '— 同受保人 —' },
      { id: 'name', label: '保單名稱', type: 'text', required: true, maxlength: 60 },
      { id: 'company', label: '保險公司', type: 'select', required: false, optionsSource: 'companies' },
      { id: 'paymentMode', label: '付款模式', type: 'select', required: true, optionsSource: 'paymentModes', defaultValue: 'direct' },
      { id: 'advanceHolderId', label: '代墊成員', type: 'select', required: false, optionsSource: 'members', emptyText: '— 請選擇 —' },
      { id: 'firstStartYear', label: '保單開始年份', type: 'number-plain', required: true, min: 2000, max: 2100, defaultValue: () => new Date().getFullYear() },
      { id: 'firstStartMonth', label: '保單開始月份', type: 'select', required: true, optionsSource: 'months', defaultValue: '01' },
      { id: 'totalPolicyYears', label: '總供款年期', type: 'number-plain', required: true, min: 1, max: 50, defaultValue: 5 },
      { id: 'currentPeriodIndex', label: '當前第幾年度', type: 'number-plain', required: true, min: 1, max: 50, defaultValue: 1 },
      { id: 'paymentType', label: '付款類型', type: 'select', required: true, optionsSource: 'insurancePaymentTypes' },
      { id: 'account', label: '扣款帳戶（可選）', type: 'text', required: false, maxlength: 60 },
      { id: 'monthlyPremium', label: '月繳 / 月供（HK$）', type: 'number', required: false, min: 0, defaultValue: 0 },
      { id: 'annualPremium', label: '年繳保費（HK$）', type: 'number', required: false, min: 0, defaultValue: 0 },
      { id: 'oncePremium', label: '一次性付款（HK$）', type: 'number', required: false, min: 0, defaultValue: 0 },
    ],
    listColumns: ['name', 'memberId', 'company', 'firstStartYear', 'currentPeriodIndex'],
    cardFields: ['memberId', 'company', 'annualPremium'], primaryColumn: 'name',
    validate: (d) => {
      const t = d.type || 'normal';
      const pt = d.paymentType || PAYMENT_TYPES.ANNUAL;
      const mp = Number(d.monthlyPremium) || 0, ap = Number(d.annualPremium) || 0, op = Number(d.oncePremium) || 0;
      if (t === 'fund_insurance' && mp <= 0) return { field: 'monthlyPremium', message: '基金保險請填寫月供金額' };
      if (t !== 'fund_insurance') {
        if (pt === PAYMENT_TYPES.ANNUAL && ap <= 0) return { field: 'annualPremium', message: '年繳保費需大於 0' };
        if (pt === PAYMENT_TYPES.MONTHLY && mp <= 0) return { field: 'monthlyPremium', message: '月繳金額需大於 0' };
        if (pt === PAYMENT_TYPES.ONCE && op <= 0) return { field: 'oncePremium', message: '一次性付款需大於 0' };
      }
      if ((d.paymentMode || 'direct') === 'advance' && !d.advanceHolderId) {
        return { field: 'advanceHolderId', message: '代墊模式需指定代墊成員' };
      }
      return null;
    },
    fromForm: (d) => {
      const t = d.type || 'normal';
      const pt = d.paymentType || PAYMENT_TYPES.ANNUAL;
      const ty = Number(d.totalPolicyYears) || 1;
      const mp = Math.round(Number(d.monthlyPremium) || 0);
      const ap = Math.round(Number(d.annualPremium) || 0);
      const op = Math.round(Number(d.oncePremium) || 0);
      let annualPremium = 0;
      if (t === 'fund_insurance' || pt === PAYMENT_TYPES.ANNUAL) annualPremium = ap;
      else if (pt === PAYMENT_TYPES.MONTHLY) annualPremium = mp * 12;
      else if (pt === PAYMENT_TYPES.ONCE) annualPremium = Math.round(op / ty);
      let totalPremium = 0;
      if (t === 'fund_insurance' || pt === PAYMENT_TYPES.MONTHLY) totalPremium = mp * 12 * ty;
      else if (pt === PAYMENT_TYPES.ANNUAL) totalPremium = ap * ty;
      else if (pt === PAYMENT_TYPES.ONCE) totalPremium = op;
      return {
        ...d,
        memberId: d.memberId,
        policyHolderId: d.policyHolderId || d.memberId,
        type: t, paymentType: pt,
        firstStartYear: Number(d.firstStartYear),
        firstStartMonth: String(d.firstStartMonth || '01').padStart(2, '0'),
        totalPolicyYears: ty, totalPolicyPeriods: ty * 12,
        totalPremium, currentPeriodIndex: Number(d.currentPeriodIndex) || 1,
        monthlyPremium: (t === 'fund_insurance' || pt === PAYMENT_TYPES.MONTHLY) ? mp : 0,
        annualPremium,
        oncePremium: pt === PAYMENT_TYPES.ONCE ? op : 0,
        fundsAllocation: t === 'fund_insurance' ? (d.fundsAllocation || []) : [],
        periods: {},
      };
    },
    toForm: (row) => ({
      ...row,
      annualPremium: row.annualPremium || 0,
      monthlyPremium: row.monthlyPremium || 0,
      oncePremium: row.oncePremium || 0,
      paymentMode: row.paymentMode || 'direct',
      advanceHolderId: row.advanceHolderId || '',
    }),
  },

  fund: {
    key: 'fund', label: '基金', icon: 'line-chart', codePrefix: 'FUND',
    fields: [
      { id: 'name', label: '基金名稱', type: 'text', required: true, maxlength: 60 },
      { id: 'type', label: '基金類型', type: 'select', required: true, includeEmpty: false,
        options: [{ value: 'standalone', label: '獨立基金' }, { value: 'insurance', label: '保險基金' }], defaultValue: 'standalone' },
      { id: 'cost', label: '投入成本（HK$）', type: 'number', required: false, min: 0, defaultValue: 0 },
      { id: 'currentValue', label: '現時價值（HK$）', type: 'number', required: false, min: 0, defaultValue: 0 },
      { id: 'units', label: '持有單位數', type: 'number-plain', required: false, min: 0, defaultValue: 0 },
      { id: 'initialYear', label: '開始年份', type: 'number-plain', required: false, min: 2000, max: 2100 },
      { id: 'initialMonth', label: '開始月份', type: 'select', required: false, optionsSource: 'months' },
      { id: 'note', label: '備註（可選）', type: 'text', required: false, maxlength: 60 },
    ],
    listColumns: ['name', 'cost', 'currentValue'], cardFields: ['cost', 'currentValue'], primaryColumn: 'name',
  },
};

export async function createEntity(entityKey, data, allRows = []) {
  const def = ENTITIES[entityKey];
  if (!def) throw new Error(`未知實體：${entityKey}`);
  let payload = def.fromForm ? def.fromForm(data) : { ...data };
  if (def.validate) {
    const err = def.validate(payload, allRows, null);
    if (err) throw new Error(err.message);
  }
  let code = null;
  if (def.codePrefix) { const r = await generateCodeFor(entityKey, payload); code = r.code; }
  const result = await api.entityCreate({
    entityType: entityKey, action: 'create', code, name: payload.name,
    extra: _stripCommon(payload),
  });
  if (!result?.id) throw new Error('新增失敗');
  return { id: result.id, code: result.code || code };
}

export async function updateEntity(entityKey, id, data, allRows = []) {
  const def = ENTITIES[entityKey];
  if (!def) throw new Error(`未知實體：${entityKey}`);
  if (!id) throw new Error('缺少 id');
  let payload = def.fromForm ? def.fromForm(data) : { ...data };
  if (def.validate) {
    const err = def.validate(payload, allRows, id);
    if (err) throw new Error(err.message);
  }
  await api.entityCreate({
    entityType: entityKey, action: 'update', id, name: payload.name,
    extra: _stripCommon(payload),
  });
  return { id };
}

export async function removeEntity(entityKey, id) {
  if (!id) throw new Error('缺少 id');
  await api.entityCreate({ entityType: entityKey, action: 'delete', id });
}

export async function getDynamicOptions(source, ctx = {}) {
  if (!source) return [];
  switch (source) {
    case 'members':
      return (await import('./app-data.js')).getList('members').map((m) => ({ value: m.id, label: m.name }));
    case 'categories':
      return (await import('./app-data.js')).getList('categories').map((c) => ({ value: c.id, label: c.name }));
    case 'items': {
      const all = (await import('./app-data.js')).getList('items');
      const f = ctx.categoryId ? all.filter((i) => i.categoryId === ctx.categoryId) : all;
      return f.map((i) => ({ value: i.id, label: i.name }));
    }
    case 'payments':
      return (await import('./app-data.js')).getList('payments').map((p) => ({ value: p.id, label: p.name }));
    case 'companies':
      return (await import('./app-data.js')).getList('companies').map((c) => ({ value: c.name, label: c.name }));
    case 'memberRoles':
      return [
        { value: 'husband', label: '老公 / 丈夫' }, { value: 'wife', label: '老婆 / 妻子' },
        { value: 'child', label: '子女' }, { value: 'other', label: '其他' },
      ];
    case 'policyTypes':
      return [
        { value: 'normal', label: '普通類保險' }, { value: 'fund_insurance', label: '基金類保險' },
        { value: 'savings', label: '儲蓄類保險' },
      ];
    case 'insurancePaymentTypes':
      return [{ value: '年繳', label: '年繳' }, { value: '月繳', label: '月繳' }, { value: '一次付款', label: '一次付款' }];
    case 'paymentModes':
      return [{ value: 'direct', label: '直接付款' }, { value: 'advance', label: '代墊模式' }];
    case 'months': {
      const arr = [];
      for (let m = 1; m <= 12; m++) arr.push({ value: String(m).padStart(2, '0'), label: `${m} 月` });
      return arr;
    }
    default:
      return [];
  }
}

export function getEntityDef(key) { return ENTITIES[key] || null; }
export function getAllEntityKeys() { return Object.keys(ENTITIES); }
export function getEntityUi(key) {
  const d = ENTITIES[key];
  if (!d) return null;
  return { primaryColumn: d.primaryColumn, listColumns: d.listColumns, cardFields: d.cardFields };
}

/* ============================================
   🆕 補齊缺失的 openEntityModal
   ============================================ */
export async function openEntityModal(entityKey, mode = 'add', options = {}) {
  const { id, allRows = [], onSuccess, onCancel } = options;
  const def = ENTITIES[entityKey];
  if (!def) {
    toast(`未知的實體：${entityKey}`, 'error');
    return;
  }

  const MODAL_ID = `entity-modal-${entityKey}-${Date.now()}`;
  const o = document.createElement('div');
  o.className = 'modal-overlay active';
  o.id = MODAL_ID;
  o.innerHTML = `<div class="modal" style="max-width:560px;max-height:90vh;overflow-y:auto;">
    <h2 class="modal-title">${mode === 'edit' ? '編輯' : '新增'}${def.label}</h2>
    <div id="${MODAL_ID}-form"></div>
  </div>`;
  document.body.appendChild(o);
  document.body.style.overflow = 'hidden';

  let initialData = {};
  if (mode === 'edit' && id) {
    const row = allRows.find((r) => r.id === id);
    if (row) {
      initialData = def.toForm ? def.toForm(row) : { ...row };
    }
  }

  // 動態解析 optionsSource 為真正的 options
  const formFields = [];
  for (const f of def.fields) {
    const field = { ...f };
    if (field.optionsSource && !field.options) {
      field.options = await getDynamicOptions(field.optionsSource);
    }
    formFields.push(field);
  }

  buildForm({
    containerId: `${MODAL_ID}-form`,
    fields: formFields,
    submitText: mode === 'edit' ? '儲存' : `新增${def.label}`,
    showCancel: true,
    cancelText: '取消',
    initialData: initialData,
    beforeSubmit: (data) => {
      if (def.validate) {
        const err = def.validate(data, allRows, id);
        if (err) return err;
      }
      return true;
    },
    onSubmit: async (data) => {
      try {
        if (mode === 'edit' && id) {
          await updateEntity(entityKey, id, data, allRows);
          toast(`✅ 已更新${def.label}`, 'success');
        } else {
          await createEntity(entityKey, data, allRows);
          toast(`✅ 已新增${def.label}`, 'success');
        }
        closeModal(o);
        o.remove();
        document.body.style.overflow = '';
        if (typeof onSuccess === 'function') onSuccess();
      } catch (err) {
        toast(err.message || '操作失敗', 'error');
      }
    },
    onCancel: () => {
      closeModal(o);
      o.remove();
      document.body.style.overflow = '';
      if (typeof onCancel === 'function') onCancel();
    },
  });

  openModal(o);
}

/* ═══ EOF ═══
F: js/app-entities.js
V: 1792670006
W: webapp-20261022-028#1
═══ */
```

<!-- === FILE END: ./js/app-entities.js === -->

<!-- === FILE START: ./js/app-pages-core.js === -->

## 📄 ./js/app-pages-core.js

```js
// ============================================
// app-pages-core.js — 6 個日常頁
// 職責：dashboard / input-center / code-list / insurance / portfolio / settlements
// 契約：功能與舊版一致；金額、狀態、合併邏輯走 app-domain.js
// ============================================

import { AppState, registerPage } from './app-core.js';
import { toast, openConfirm, buildForm, openModal, closeModal, initTabPanel, esc, formatHKD } from './app-ui.js';
import { api, listen, listenList, getList, resolveName, statusLabel, statusBadgeClass, statusIsDone, statusNormalize } from './app-data.js';
import { renderStats, renderTable, renderCards, renderControls } from './app-widgets.js';
import { openEntityModal } from './app-entities.js';
import {
  calcBankBalance, computeYearTotals, computeAnnualOverview,
  getPolicyAnnualPremium, getPolicyTotalPremium, getPolicyPaidTotal,
  countCompletedPeriods, calcProgress, isPolicyCompleted,
  calcFundCostEstimate, getFundCostMap, findPolicyByFund,
  mergeSettlementData, computeMemberAnnualRows, resolveMonthlyAmount,
} from './app-domain.js';
import { ENTITIES, removeEntity } from './app-entities.js';

/* ============================================
   1. Dashboard
   ============================================ */
function pageDashboard(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">總覽儀表板</h1><p class="page-subtitle"></p></div></div>
    <div id="dash-stats"></div>
    <div id="dash-ctrl"></div>
    <div id="dash-annual"></div>`;
  const unsubs = [];
  let _stats = null, _ctrl = null;

  const render = () => {
    const accounts = getList('bank_accounts');
    const year = AppState.year;
    const month = AppState.month === 'all' ? '12' : AppState.month;
    const bankTotal = calcBankBalance(accounts, flattenTxns(), year, month);
    _stats?.destroy?.();
    const yt = window.__lastDashboardTotals || { totalIncome: 0, totalExpense: 0, net: 0, monthCount: 0 };
    _stats = renderStats({ container: 'dash-stats', columns: 4, cards: [
      { title: '家庭總餘額', value: formatHKD(bankTotal), valueClass: bankTotal >= 0 ? 'text-emerald' : 'text-red', hint: `共 ${accounts.length} 個帳號`, icon: 'landmark' },
      { title: `${year} 家庭收入`, value: formatHKD(yt.totalIncome), valueClass: 'text-emerald', icon: 'trending-up' },
      { title: `${year} 年度總支出`, value: formatHKD(yt.totalExpense), valueClass: 'text-red', hint: `${yt.monthCount} 個月有紀錄`, icon: 'trending-down' },
      { title: `${year} 年度淨餘額`, value: formatHKD(yt.net), valueClass: yt.net >= 0 ? 'text-emerald' : 'text-red', icon: 'wallet' },
    ] });
    paintAnnual();
  };

  const paintAnnual = () => {
    const el = document.getElementById('dash-annual');
    if (!el) return;
    const rows = computeAnnualOverview(window.__allIncome || {}, window.__allExpenses || {}, getList('insurance_policies'), AppState.year);
    if (!rows.length) { el.innerHTML = ''; return; }
    el.innerHTML = `<div class="glass-card" style="padding:0;margin-bottom:20px;">
      <div style="padding:14px 20px;font-weight:600;font-size:14px;">年度總覽（前後 2 年）</div>
      <div style="overflow-x:auto;"><table class="data-table"><thead><tr>
        <th>年度</th><th class="num">家庭收入</th><th class="num">總支出</th><th class="num">保險平攤</th><th class="num">淨餘額</th><th class="num">每月平均</th>
      </tr></thead><tbody>${rows.map((r) => `<tr>
        <td>${r.year} 年${r.isCurrent ? ' <span class="badge badge-info" style="font-size:10px;">今年</span>' : ''}</td>
        <td class="num text-emerald">${formatHKD(r.totalIncome)}</td>
        <td class="num text-red">${formatHKD(r.totalExpense)}</td>
        <td class="num text-magenta">${formatHKD(r.insurance)}</td>
        <td class="num ${r.net >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(r.net)}</td>
        <td class="num">${formatHKD(r.avg)}</td>
      </tr>`).join('')}</tbody></table></div></div>`;
  };

  const flattenTxns = () => {
    const list = [];
    getList('bank_accounts').forEach((acc) => {
      Object.entries(acc.transactions || {}).forEach(([id, t]) => list.push({ id, bankId: acc.id, ...t }));
    });
    return list;
  };

  unsubs.push(listen('income', (snap) => {
    window.__allIncome = snap.val() || {};
    const totals = computeYearTotals(window.__allIncome, window.__allExpenses, AppState.year);
    window.__lastDashboardTotals = totals;
    render();
  }));
  unsubs.push(listen('expenses', (snap) => {
    window.__allExpenses = snap.val() || {};
    const totals = computeYearTotals(window.__allIncome, window.__allExpenses, AppState.year);
    window.__lastDashboardTotals = totals;
    render();
  }));
  unsubs.push(listen('bank_accounts', () => render()));
  unsubs.push(AppState.on('ym-change', render));

  _ctrl = renderControls({ container: 'dash-ctrl', items: [
    { type: 'custom', render: () => `<a class="btn btn-primary" href="#/annual-report"><i data-lucide="bar-chart-3"></i> 查看年度報表</a>` },
    { type: 'year' },
  ] });
  render();
  return { destroy: () => { unsubs.forEach((u) => u && u()); _stats?.destroy?.(); _ctrl?.destroy?.(); } };
}

/* ============================================
   2. Input Center
   ============================================ */
function pageInputCenter(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">綜合輸入中心</h1><p class="page-subtitle"></p></div></div>
    <div id="ic-buttons" class="flex flex-wrap gap-8 mb-16">
      <button type="button" class="btn btn-primary" data-action="add-expense"><i data-lucide="plus"></i> 新增支出</button>
      <button type="button" class="btn btn-ghost" data-action="add-contribution"><i data-lucide="trending-up"></i> 家用轉入</button>
      <button type="button" class="btn btn-ghost" data-action="add-personal-income"><i data-lucide="wallet"></i> 個人收入</button>
    </div>
    <div id="ic-tabs"></div>
    <div id="ic-panel-expense" class="tab-panel"></div>
    <div id="ic-panel-insurance" class="tab-panel"></div>
    <div id="ic-panel-fund" class="tab-panel"></div>
    <div id="ic-panel-bank" class="tab-panel"></div>`;
  const unsubs = [];
  let _tab, _expenseRows = [], _expUnsub = null;

  _tab = initTabPanel({
    containerId: 'ic-tabs',
    tabs: [
      { key: 'expense', label: '支出', icon: 'receipt', panelId: 'ic-panel-expense' },
      { key: 'insurance', label: '保險', icon: 'shield', panelId: 'ic-panel-insurance' },
      { key: 'fund', label: '基金', icon: 'line-chart', panelId: 'ic-panel-fund' },
      { key: 'bank', label: '銀行', icon: 'landmark', panelId: 'ic-panel-bank' },
    ],
    defaultKey: 'expense',
    storageKey: 'input-center-tab',
    onChange: (key) => loadTab(key),
  });

  const loadExpense = () => {
    if (_expUnsub) { try { _expUnsub(); } catch (e) {} }
    _expUnsub = listen('expenses', (snap) => {
      const flat = [];
      const year = AppState.year;
      Object.entries(snap.val() || {}).forEach(([y, ms]) => {
        if (String(y) !== String(year)) return;
        Object.entries(ms || {}).forEach(([m, md]) => {
          Object.entries(md.member_expenses || {}).forEach(([mid, items]) => {
            Object.entries(items || {}).forEach(([id, e]) => flat.push({ id, memberId: mid, year: y, month: m, ...e }));
          });
        });
      });
      _expenseRows = flat.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0)).slice(0, 50);
      paintExpense();
    });
  };

  const paintExpense = () => {
    const el = document.getElementById('ic-panel-expense');
    if (!el) return;
    el.innerHTML = '';
    const tbl = renderTable({
      container: el,
      rows: _expenseRows,
      columns: [
        { id: 'date', label: '日期', type: 'date', width: 100 },
        { id: 'memberId', label: '成員', width: 100 },
        { id: 'name', label: '項目', type: 'text' },
        { id: 'amount', label: '金額', type: 'number', width: 110 },
        { id: 'status', label: '狀態', width: 120 },
      ],
      resolvers: {
        memberId: (v) => v === 'shared' ? '🏠 家庭' : esc(resolveName('members', v) || v || '—'),
        amount: (v) => formatHKD(v),
        status: (v) => `<span class="badge ${statusBadgeClass(v)}">${esc(statusLabel(v, { source: 'personal' }))}</span>`,
      },
      actions: AppState.getCanInput() ? (row) => [
        { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
          const ok = await openConfirm(`確定刪除「${row.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
          if (!ok) return;
          try {
            const { removeItem } = await import('./app-data.js');
            await removeItem(`expenses/${row.year}/${row.month}/member_expenses/${row.memberId}`, row.id);
            toast('✅ 已刪除', 'success');
          } catch (e) { toast('刪除失敗：' + e.message, 'error'); }
        } },
      ] : () => [],
    });
  };

  const loadTab = (key) => {
    const el = document.getElementById(`ic-panel-${key}`);
    if (!el) return;
    if (key === 'expense') { loadExpense(); return; }
    el.innerHTML = `<div class="glass-card"><div class="empty-state">${key === 'insurance' ? '保險' : key === 'fund' ? '基金' : '銀行'}管理請至對應專頁</div></div>`;
  };

  const btnRoot = document.getElementById('ic-buttons');
  btnRoot?.addEventListener('click', async (e) => {
    const btn = e.target.closest('button[data-action]');
    if (!btn) return;
    if (btn.dataset.action === 'add-expense') await openExpenseModal();
    else if (btn.dataset.action === 'add-contribution') await openContribModal();
    else if (btn.dataset.action === 'add-personal-income') await openPersonalIncomeModal();
  });

  loadTab(_tab?.getCurrent() || 'expense');
  return { destroy: () => { unsubs.forEach((u) => u && u()); _expUnsub?.(); _tab?.destroy?.(); } };
}

/* 支出 Modal */
async function openExpenseModal() {
  const { getDynamicOptions } = await import('./app-entities.js');
  const members = await getDynamicOptions('members');
  const cats = await getDynamicOptions('categories');
  const pays = await getDynamicOptions('payments');
  members.push({ value: 'shared', label: '🏠 家庭共用' });
  const o = document.createElement('div');
  o.className = 'modal-overlay active';
  o.innerHTML = `<div class="modal" style="max-width:520px;"><h2 class="modal-title">新增支出</h2><div id="__exp_form"></div></div>`;
  document.body.appendChild(o);
  const { addItem } = await import('./app-data.js');
  buildForm({
    containerId: '__exp_form',
    fields: [
      { type: 'select', id: 'memberId', label: '成員', required: true, includeEmpty: false, options: members },
      { type: 'text', id: 'name', label: '項目名稱', required: true, maxlength: 60 },
      { type: 'number', id: 'amount', label: '金額（HK$）', required: true, min: 0, defaultValue: 0 },
      { type: 'text', id: 'date', label: '日期（YYYY-MM-DD）', defaultValue: new Date().toISOString().slice(0, 10) },
      { type: 'select', id: 'categoryId', label: '類別', options: cats, includeEmpty: true },
      { type: 'select', id: 'paymentMethodId', label: '支付方式', options: pays, includeEmpty: true },
    ],
    submitText: '新增', showCancel: true, cancelText: '取消',
    onSubmit: async (d) => {
      const { year, month } = AppState.getYearMonth();
      const y = year || String(new Date().getFullYear());
      const m = month === 'all' ? '01' : month;
      await addItem(`expenses/${y}/${m}/member_expenses/${d.memberId}`, {
        name: d.name, amount: Number(d.amount) || 0, status: 'pending',
        date: d.date || '', categoryId: d.categoryId || '', itemId: '',
        paymentMethodId: d.paymentMethodId || '', bankId: '', txnId: '',
        isAutoLinked: false, createdAt: Date.now(),
      });
      toast('✅ 已新增支出', 'success');
      closeModal(o); o.remove();
    },
    onCancel: () => { closeModal(o); o.remove(); },
  });
  openModal(o);
}

async function openContribModal() {
  const { getDynamicOptions } = await import('./app-entities.js');
  const { updateItem } = await import('./app-data.js');
  const members = await getDynamicOptions('members');
  const o = document.createElement('div');
  o.className = 'modal-overlay active';
  o.innerHTML = `<div class="modal" style="max-width:480px;"><h2 class="modal-title">家用轉入</h2><div id="__con_form"></div></div>`;
  document.body.appendChild(o);
  buildForm({
    containerId: '__con_form',
    fields: [
      { type: 'select', id: 'memberId', label: '成員', required: true, includeEmpty: false, options: members },
      { type: 'number', id: 'amount', label: '金額（HK$）', required: true, min: 0, defaultValue: 0 },
    ],
    submitText: '儲存', showCancel: true, cancelText: '取消',
    onSubmit: async (d) => {
      const { year, month } = AppState.getYearMonth();
      const y = year || String(new Date().getFullYear());
      const m = month === 'all' ? '01' : month;
      const snap = await (await import('./app-data.js')).getOnce(`income/${y}/${m}`) || {};
      snap[d.memberId] = Number(d.amount) || 0;
      await (await import('./app-data.js')).setSubpath(`income/${y}/${m}`, snap);
      toast('✅ 已新增家用轉入', 'success');
      closeModal(o); o.remove();
    },
    onCancel: () => { closeModal(o); o.remove(); },
  });
  openModal(o);
}

async function openPersonalIncomeModal() {
  const { getDynamicOptions } = await import('./app-entities.js');
  const members = await getDynamicOptions('members');
  const o = document.createElement('div');
  o.className = 'modal-overlay active';
  o.innerHTML = `<div class="modal" style="max-width:480px;"><h2 class="modal-title">個人收入</h2><div id="__pi_form"></div></div>`;
  document.body.appendChild(o);
  buildForm({
    containerId: '__pi_form',
    fields: [
      { type: 'select', id: 'memberId', label: '成員', required: true, includeEmpty: false, options: members },
      { type: 'number', id: 'amount', label: '金額（HK$）', required: true, min: 0, defaultValue: 0 },
    ],
    submitText: '儲存', showCancel: true, cancelText: '取消',
    onSubmit: async (d) => {
      await api.personalIncome.save(d.memberId, AppState.year, AppState.month === 'all' ? '01' : AppState.month, Number(d.amount) || 0);
      toast('✅ 已新增個人收入', 'success');
      closeModal(o); o.remove();
    },
    onCancel: () => { closeModal(o); o.remove(); },
  });
  openModal(o);
}

/* ============================================
   3. Code List
   ============================================ */
function pageCodeList(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">編號對照清單</h1><p class="page-subtitle"></p></div></div>
    <div id="cl-filter" class="glass-card" style="padding:12px;margin-bottom:16px;">
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;">
        <div style="display:grid;grid-template-columns:72px 1fr;gap:8px;align-items:center;">
          <label style="font-size:12px;color:var(--text-muted);text-align:right;">類型：</label>
          <select class="select" id="cl-type">
            <option value="all">全部</option>
            <option value="member">成員</option><option value="bank">銀行</option>
            <option value="category">支出類別</option><option value="item">支出項目</option>
            <option value="payment">支付方式</option><option value="company">保險公司</option>
            <option value="policy">保單</option><option value="fund">基金</option>
            <option value="bankAccount">銀行帳號</option>
          </select>
        </div>
        <div style="display:grid;grid-template-columns:72px 1fr;gap:8px;align-items:center;">
          <label style="font-size:12px;color:var(--text-muted);text-align:right;">編號狀態：</label>
          <select class="select" id="cl-code">
            <option value="all">全部</option>
            <option value="with">有編號</option>
            <option value="without">無編號</option>
          </select>
        </div>
      </div>
    </div>
    <div id="cl-list"></div>`;
  let _type = 'all', _code = 'all';
  const render = () => {
    const rows = [];
    const types = [
      { key: 'member', label: '成員' }, { key: 'bank', label: '銀行' },
      { key: 'category', label: '支出類別' }, { key: 'item', label: '支出項目' },
      { key: 'payment', label: '支付方式' }, { key: 'company', label: '保險公司' },
      { key: 'policy', label: '保單' }, { key: 'fund', label: '基金' },
      { key: 'bankAccount', label: '銀行帳號' },
    ];
    types.forEach((t) => {
      if (_type !== 'all' && _type !== t.key) return;
      const list = getList(t.key);
      list.forEach((r) => {
        const has = !!r.code;
        if (_code === 'with' && !has) return;
        if (_code === 'without' && has) return;
        rows.push({ entity: t.key, label: t.label, id: r.id, code: r.code || '', name: r.name || '（未命名）' });
      });
    });
    rows.sort((a, b) => (a.code || 'zzz').localeCompare(b.code || 'zzz'));
    document.getElementById('cl-list').innerHTML = `<div class="glass-card" style="padding:0;"><div style="overflow-x:auto;">
      <table class="data-table"><thead><tr><th style="width:180px;">編號</th><th style="width:120px;">類型</th><th>名稱</th></tr></thead><tbody>
      ${rows.map((r) => `<tr>
        <td><span class="mono" style="font-size:12px;color:${r.code ? 'var(--neon-cyan)' : 'var(--text-muted)'};">${esc(r.code || '—')}</span></td>
        <td><span class="badge badge-info">${esc(r.label)}</span></td>
        <td>${esc(r.name)}</td>
      </tr>`).join('')}</tbody></table></div></div>`;
  };
  document.getElementById('cl-type')?.addEventListener('change', (e) => { _type = e.target.value; render(); });
  document.getElementById('cl-code')?.addEventListener('change', (e) => { _code = e.target.value; render(); });
  const keys = ['member','bank','category','item','payment','company','policy','fund','bankAccount'];
  const unsubs = keys.map((k) => listen(k === 'bankAccount' ? 'bank_accounts' : k === 'category' ? 'expense_categories' : k === 'item' ? 'expense_items' : k === 'payment' ? 'payment_methods' : k === 'company' ? 'insurance_companies' : k === 'policy' ? 'insurance_policies' : k === 'fund' ? 'funds' : k + 's', render));
  render();
  return { destroy: () => { unsubs.forEach((u) => u && u()); } };
}

/* ============================================
   4. Insurance
   ============================================ */
function pageInsurance(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">保險清單表</h1><p class="page-subtitle"></p></div></div>
    <div id="ins-stats"></div>
    <div id="ins-ctrl"></div>
    <div class="banner">ℹ️ 每張保單依「保單年度」自動產生每月分攤。扣款狀態請至「結算清單」編輯。</div>
    <div id="ins-content"></div>`;
  let _policies = [], _payments = {}, _stats = null, _ctrl = null;
  const { view } = { view: 'card' };

  const paint = () => {
    const targetYear = Number(AppState.year) || new Date().getFullYear();
    const enriched = _policies.map((p) => {
      const pays = _payments[p.id] || {};
      const completedPeriods = p.type === 'fund_insurance' ? 0 : countCompletedPeriods(pays);
      const paidTotal = getPolicyPaidTotal(pays);
      const curAnnual = getPolicyAnnualPremium(p, targetYear);
      const totalPremium = getPolicyTotalPremium(p);
      const e = { ...p, completedPeriods, _paidTotal: paidTotal, _curAnnual: curAnnual, _totalPremium: totalPremium };
      e._isCompleted = isPolicyCompleted(e);
      return e;
    });
    const totalPaid = enriched.reduce((s, p) => s + (p._paidTotal || 0), 0);
    _stats?.destroy?.();
    _stats = renderStats({ container: 'ins-stats', columns: 4, cards: [
      { title: `${targetYear} 年度總供款`, value: formatHKD(enriched.reduce((s, p) => s + (p._curAnnual || 0), 0)), valueClass: 'text-cyan', hint: `共 ${enriched.length} 張`, icon: 'wallet' },
      { title: '累計已供款', value: formatHKD(totalPaid), valueClass: 'text-emerald', icon: 'check-circle' },
      { title: '供款中', value: `${enriched.filter((p) => !p._isCompleted).length} 張`, icon: 'shield' },
      { title: '已供滿', value: `${enriched.filter((p) => p._isCompleted).length} 張`, valueClass: 'text-emerald', icon: 'check-circle' },
    ] });
    const el = document.getElementById('ins-content');
    if (!el) return;
    if (_policies.length === 0) {
      el.innerHTML = '<div class="glass-card"><div class="empty-state">尚無保單資料</div></div>';
      return;
    }
    const cols = [
      { id: 'name', label: '保單名稱', type: 'text' },
      { id: 'company', label: '保險公司', type: 'text', width: 110 },
      { id: 'memberId', label: '受保人', width: 100 },
      { id: 'firstStartYear', label: '開始年', type: 'number', width: 80 },
      { id: 'annualPremium', label: '本期年繳', type: 'number', width: 110 },
      { id: 'totalPremium', label: '保單總供款', type: 'number', width: 120 },
      { id: 'paidTotal', label: '已供款', type: 'number', width: 110 },
      { id: 'progress', label: '進度', width: 120 },
    ];
    const resolvers = {
      memberId: (v) => esc(resolveName('members', v) || v || '—'),
      annualPremium: (v, r) => `<span class="mono text-cyan">${formatHKD(r._curAnnual)}</span>`,
      totalPremium: (v, r) => `<span class="mono text-magenta">${formatHKD(r._totalPremium)}</span>`,
      paidTotal: (v, r) => `<span class="mono text-emerald">${formatHKD(r._paidTotal)}</span>`,
      firstStartYear: (v, r) => `${v}-${r.firstStartMonth || '01'}`,
      progress: (v, r) => {
        if (r.type === 'fund_insurance') return '<span class="text-muted">基金保險</span>';
        const p = calcProgress(r);
        return `<div style="font-size:11px;color:var(--text-muted);">${r.completedPeriods || 0} / ${r.totalPolicyPeriods || 0} 期 (${p}%)</div><div style="height:5px;background:var(--divider);border-radius:3px;"><div style="height:100%;width:${p}%;background:var(--primary);border-radius:3px;"></div></div>`;
      },
    };
    renderTable({ container: el, rows: enriched, columns: cols, resolvers,
      actions: AppState.getCanInput() ? (row) => [
        { label: '編輯', icon: 'pencil', className: 'btn-ghost', action: 'edit', onClick: () => openEntityModal('policy', 'edit', { id: row.id, allRows: _policies }) },
        { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
          const ok = await openConfirm(`確定刪除保單「${row.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
          if (!ok) return;
          try { await removeEntity('policy', row.id); toast('✅ 已刪除', 'success'); } catch (e) { toast(e.message, 'error'); }
        } },
      ] : () => [],
    });
  };

  const unsubs = [
    listen('insurance_policies', (snap) => {
      const v = snap.val() || {};
      _policies = Object.entries(v).map(([id, x]) => ({ id, ...x }));
      paint();
    }),
    listen('insurance_payments', (snap) => { _payments = snap.val() || {}; paint(); }),
  ];

  _ctrl = renderControls({ container: 'ins-ctrl', items: [
    { type: 'add', label: '新增保單', onClick: () => openEntityModal('policy', 'add', { onSuccess: () => {} }) },
    { type: 'year' },
  ] });
  return { destroy: () => { unsubs.forEach((u) => u && u()); _stats?.destroy?.(); _ctrl?.destroy?.(); } };
}

/* ============================================
   5. Portfolio
   ============================================ */
function pagePortfolio(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">基金投資表</h1><p class="page-subtitle"></p></div><div id="pf-actions"></div></div>
    <div id="pf-stats"></div>
    <div id="pf-ctrl"></div>
    <div id="pf-ins" style="margin-bottom:20px;"></div>
    <div id="pf-std"></div>`;
  let _funds = [], _policies = [], _stats = null, _ctrl = null;

  const paint = () => {
    const ym = AppState.getYearMonth();
    const y = ym.year || String(new Date().getFullYear());
    const m = ym.month === 'all' ? String(new Date().getMonth() + 1).padStart(2, '0') : ym.month;
    const costMaps = {};
    _policies.forEach((p) => { costMaps[p.id] = getFundCostMap(p, _funds, y, m); });
    let cost = 0, value = 0;
    _funds.forEach((f) => {
      if (f.type === 'insurance') {
        const p = findPolicyByFund(f, _policies);
        cost += (p && costMaps[p.id]) ? (costMaps[p.id][f.id] || 0) : Number(f.cost) || 0;
      } else cost += Number(f.cost) || 0;
      value += Number(f.currentValue) || 0;
    });
    const pnl = value - cost;
    const pct = cost > 0 ? ((pnl / cost) * 100).toFixed(2) : '0.00';
    _stats?.destroy?.();
    _stats = renderStats({ container: 'pf-stats', columns: 3, cards: [
      { title: '總投入成本', value: formatHKD(cost), hint: `共 ${_funds.length} 筆`, icon: 'wallet' },
      { title: '總現時價值', value: formatHKD(value), valueClass: 'text-emerald', icon: 'line-chart' },
      { title: '總帳面盈虧', value: `${pnl >= 0 ? '+' : ''}${formatHKD(pnl)} (${pct}%)`, valueClass: pnl >= 0 ? 'text-emerald' : 'text-red', icon: pnl >= 0 ? 'trending-up' : 'trending-down' },
    ] });
    paintList('pf-ins', _funds.filter((f) => f.type === 'insurance'), costMaps);
    paintList('pf-std', _funds.filter((f) => f.type !== 'insurance'), costMaps);
  };

  const paintList = (containerId, list, costMaps) => {
    const el = document.getElementById(containerId);
    if (!el) return;
    if (!list.length) { el.innerHTML = `<div class="glass-card"><div class="empty-state">${containerId === 'pf-ins' ? '尚無保險基金' : '尚無獨立基金'}</div></div>`; return; }
    el.innerHTML = `<div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:12px;">
      ${list.map((f) => {
        const p = f.type === 'insurance' ? findPolicyByFund(f, _policies) : null;
        const c = (p && costMaps[p.id]) ? (costMaps[p.id][f.id] || 0) : (Number(f.cost) || 0);
        const v = Number(f.currentValue) || 0;
        const pnl = v - c;
        const pnlPct = c > 0 ? ((pnl / c) * 100).toFixed(2) : '0.00';
        return `<div class="glass-card" style="padding:16px;">
          <div style="font-size:15px;font-weight:700;margin-bottom:6px;">${esc(f.name || '')}</div>
          ${p ? `<div style="font-size:11px;color:var(--accent-warm);margin-bottom:10px;">${esc(p.name || '')}</div>` : ''}
          <div style="display:flex;flex-direction:column;gap:6px;font-size:13px;">
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--text-muted);font-size:11px;">投入成本</span><span class="mono">${formatHKD(c)}</span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--text-muted);font-size:11px;">現時價值</span><span class="mono text-emerald">${formatHKD(v)}</span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--text-muted);font-size:11px;">盈虧</span><span class="mono ${pnl >= 0 ? 'text-emerald' : 'text-red'}">${pnl >= 0 ? '+' : ''}${formatHKD(pnl)} (${pnlPct}%)</span></div>
          </div>
        </div>`;
      }).join('')}
    </div>`;
  };

  const unsubs = [
    listen('funds', (snap) => {
      const v = snap.val() || {};
      _funds = Object.entries(v).map(([id, x]) => ({ id, ...x }));
      paint();
    }),
    listen('insurance_policies', (snap) => {
      const v = snap.val() || {};
      _policies = Object.entries(v).map(([id, x]) => ({ id, ...x }));
      paint();
    }),
  ];

  _ctrl = renderControls({ container: 'pf-ctrl', items: [
    { type: 'add', label: '新增基金', onClick: () => openEntityModal('fund', 'add') },
    { type: 'year' }, { type: 'month' },
  ] });
  return { destroy: () => { unsubs.forEach((u) => u && u()); _stats?.destroy?.(); _ctrl?.destroy?.(); } };
}

/* ============================================
   6. Settlements
   ============================================ */
function pageSettlements(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">結算清單</h1><p class="page-subtitle"></p></div></div>
    <div id="st-stats"></div>
    <div id="st-ctrl"></div>
    <div id="st-content"></div>`;
  let _stats = null, _ctrl = null, _expUnsub = null, _payUnsub = null;
  let _memberExpenses = [], _insuranceRows = [];

  const reload = async () => {
    const { year, month } = AppState.getYearMonth();
    const y = String(year);
    const m = month === 'all' ? '12' : month;
    try {
      const policies = getList('insurance_policies');
      const rows = [];
      for (const p of policies) {
        const pays = await (await import('./app-data.js')).getOnce(`insurance_payments/${p.id}/${y}/${m}`) || {};
        const holder = p.policyHolderId || p.memberId || '';
        if (pays && Object.keys(pays).length) {
          rows.push({ policyId: p.id, policyName: p.name, policyHolderId: holder, memberId: p.memberId, status: pays.status || '', amount: Number(pays.amount) || 0, paymentMode: p.paymentMode || 'direct', advanceHolderId: p.advanceHolderId || '' });
        } else {
          const est = resolveMonthlyAmount(p, y, m) || 0;
          rows.push({ policyId: p.id, policyName: p.name, policyHolderId: holder, memberId: p.memberId, status: '', amount: est, paymentMode: p.paymentMode || 'direct', advanceHolderId: p.advanceHolderId || '' });
        }
      }
      _insuranceRows = rows;
      paint();
    } catch (e) { console.warn('[settlements] 載入失敗：', e); }
  };

  const paint = () => {
    const { year, month } = AppState.getYearMonth();
    const merged = mergeSettlementData({ memberExpenses: _memberExpenses, insuranceRows: _insuranceRows, year, month });
    const pending = merged.filter((r) => !r.isDone);
    const done = merged.filter((r) => r.isDone);
    _stats?.destroy?.();
    _stats = renderStats({ container: 'st-stats', columns: 3, cards: [
      { title: '待處理', value: formatHKD(pending.reduce((s, r) => s + r.amount, 0)), valueClass: 'text-magenta', hint: `${pending.length} 筆`, icon: 'clock' },
      { title: '已處理', value: formatHKD(done.reduce((s, r) => s + r.amount, 0)), valueClass: 'text-emerald', hint: `${done.length} 筆`, icon: 'check-circle' },
      { title: '總計', value: formatHKD(merged.reduce((s, r) => s + r.amount, 0)), valueClass: 'text-cyan', hint: `${merged.length} 筆`, icon: 'calculator' },
    ] });
    const el = document.getElementById('st-content');
    if (!el) return;
    renderTable({ container: el, rows: merged,
      columns: [
        { id: 'source', label: '來源', width: 90 },
        { id: 'name', label: '項目名稱', type: 'text' },
        { id: 'memberId', label: '成員', width: 90 },
        { id: 'amount', label: '金額', type: 'number', width: 110 },
        { id: 'status', label: '狀態', width: 120 },
      ],
      resolvers: {
        source: (_, r) => `<span class="badge ${r.source === 'insurance' ? 'badge-success' : 'badge-info'}">${esc(r.sourceLabel)}</span>`,
        memberId: (v) => v === 'shared' ? '🏠 家庭' : esc(resolveName('members', v) || v || '—'),
        amount: (v) => formatHKD(v),
        status: (v, r) => `<span class="badge ${statusBadgeClass(v)}">${esc(statusLabel(v, { source: r.source }))}</span>`,
      },
      actions: AppState.getCanInput() ? (row) => {
        if (row.source === 'insurance') {
          return row.isDone
            ? [{ label: '取消扣款', icon: 'rotate-ccw', className: 'btn-ghost', action: 'unsync', onClick: () => onUnsync(row) }]
            : [{ label: '確認扣款', icon: 'check', className: 'btn-primary', action: 'sync', onClick: () => onSync(row) }];
        }
        return [{ label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
          const ok = await openConfirm(`確定刪除「${row.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
          if (!ok) return;
          try {
            const { removeItem } = await import('./app-data.js');
            await removeItem(`expenses/${row.year}/${row.month}/member_expenses/${row._ref.memberId}`, row._ref.expenseId);
            toast('✅ 已刪除', 'success');
          } catch (e) { toast(e.message, 'error'); }
        } }];
      } : () => [],
    });
  };

  const onSync = async (row) => {
    try {
      await api.insuranceSync({
        policyId: row._ref.policyId, memberId: row._ref.memberId || row.memberId,
        policyName: row.name, monthlyAverage: row.amount, year: row.year, month: row.month,
        bankId: '', txnId: '', paymentMode: row.paymentMode || 'direct', advanceHolderId: row.advanceHolderId || '',
      });
      toast('✅ 已確認扣款', 'success');
      reload();
    } catch (e) { toast('確認失敗：' + e.message, 'error'); }
  };
  const onUnsync = async (row) => {
    const ok = await openConfirm(`確定取消「${row.name}」本月扣款？`, { okText: '取消扣款', okClass: 'btn-danger' });
    if (!ok) return;
    try {
      await api.insuranceUnsync({ policyId: row._ref.policyId, memberId: row._ref.memberId || row.memberId, year: row.year, month: row.month, amount: row.amount, paymentMode: row.paymentMode || 'direct', advanceHolderId: row.advanceHolderId || '' });
      toast('✅ 已取消扣款', 'success');
      reload();
    } catch (e) { toast('取消失敗：' + e.message, 'error'); }
  };

  if (_expUnsub) _expUnsub();
  _expUnsub = listen('expenses', (snap) => {
    const { year, month } = AppState.getYearMonth();
    const md = (snap.val() || {})[year]?.[month === 'all' ? '12' : month]?.member_expenses || {};
    const flat = [];
    Object.entries(md).forEach(([mid, items]) => Object.entries(items || {}).forEach(([id, e]) => flat.push({ id, memberId: mid, ...e })));
    _memberExpenses = flat;
    paint();
  });

  _ctrl = renderControls({ container: 'st-ctrl', items: [{ type: 'year' }, { type: 'month', includeAll: true }] });
  reload();
  return { destroy: () => { _expUnsub?.(); _stats?.destroy?.(); _ctrl?.destroy?.(); } };
}

/* ============================================
   7. 註冊
   ============================================ */
import './app-pages-reports.js';

registerPage('/dashboard', pageDashboard);
registerPage('/input-center', pageInputCenter);
registerPage('/code-list', pageCodeList);
registerPage('/insurance', pageInsurance);
registerPage('/portfolio', pagePortfolio);
registerPage('/settlements', pageSettlements);

export { pageDashboard, pageInputCenter, pageCodeList, pageInsurance, pagePortfolio, pageSettlements };

/* ═══ EOF ═══
F: js/app-pages-core.js
V: 1792670001
W: webapp-20261022-022#2
═══ */
```

<!-- === FILE END: ./js/app-pages-core.js === -->

<!-- === FILE START: ./js/app-pages-reports.js === -->

## 📄 ./js/app-pages-reports.js

```js
// ============================================
// app-pages-reports.js — 5 個報表／設定頁
// 職責：finance / annual-report / member-report / settings / admin
// ============================================

import { AppState, registerPage, logout, isSuperAdmin } from './app-core.js';
import { toast, openConfirm, buildForm, openModal, closeModal, openPrompt, initTabPanel, esc, formatHKD } from './app-ui.js';
import { api, listen, listenList, getList, resolveName, statusLabel, statusBadgeClass, statusIsDone } from './app-data.js';
import { renderStats, renderTable, renderControls } from './app-widgets.js';
import { openEntityModal } from './app-entities.js';
import {
  calcBankBalance, computeYearTotals, computeMemberReportRows,
  computeMemberAnnualRows, computeAnnualOverview,
  getPolicyAnnualPremium, getPolicyTotalPremium, getPolicyPaidTotal,
  countCompletedPeriods, calcProgress, isPolicyCompleted,
  calcFundCostEstimate, getFundCostMap, findPolicyByFund,
} from './app-domain.js';
import { getEntityDef } from './app-entities.js';

/* ============================================
   1. Finance（銀行交易）
   ============================================ */
function pageFinance(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">銀行交易</h1><p class="page-subtitle"></p></div></div>
    <div id="fin-stats"></div>
    <div id="fin-ctrl"></div>
    <div id="fin-tabs"></div>
    <div id="fin-panel-banks" class="tab-panel"></div>
    <div id="fin-panel-txns" class="tab-panel"></div>`;
  let _accounts = [], _stats = null, _ctrl = null, _tab = null;

  const flatten = () => {
    const arr = [];
    _accounts.forEach((acc) => Object.entries(acc.transactions || {}).forEach(([id, t]) => arr.push({ id, bankId: acc.id, bankName: acc.name || '', ...t })));
    return arr;
  };

  const paint = () => {
    const { year, month } = AppState.getYearMonth();
    const tM = month === 'all' ? '12' : month;
    const y = year || String(new Date().getFullYear());
    const all = flatten();
    const total = calcBankBalance(_accounts, all, y, tM);
    const prefix = `${y}-${String(tM).padStart(2, '0')}`;
    const monthTxns = all.filter((t) => (t.date || '').startsWith(prefix));
    const inTotal = monthTxns.filter((t) => t.type === 'in').reduce((s, t) => s + (Number(t.amount) || 0), 0);
    const outTotal = monthTxns.filter((t) => t.type !== 'in').reduce((s, t) => s + (Number(t.amount) || 0), 0);
    _stats?.destroy?.();
    _stats = renderStats({ container: 'fin-stats', columns: 4, cards: [
      { title: '家庭總餘額', value: formatHKD(total), valueClass: total >= 0 ? 'text-emerald' : 'text-red', hint: `${_accounts.length} 個帳號`, icon: 'landmark' },
      { title: '本月入帳', value: formatHKD(inTotal), valueClass: 'text-emerald', icon: 'arrow-down-circle' },
      { title: '本月出帳', value: formatHKD(outTotal), valueClass: 'text-red', icon: 'arrow-up-circle' },
      { title: '本月淨變動', value: formatHKD(inTotal - outTotal), valueClass: (inTotal - outTotal) >= 0 ? 'text-emerald' : 'text-red', icon: 'trending-up' },
    ] });
    paintBanks();
    paintTxns(all);
  };

  const paintBanks = () => {
    const el = document.getElementById('fin-panel-banks');
    if (!el) return;
    const { year, month } = AppState.getYearMonth();
    const tM = month === 'all' ? '12' : month;
    const y = year || String(new Date().getFullYear());
    const all = flatten();
    const rows = _accounts.map((a) => ({ ...a, _balance: calcBankBalance([a], all, y, tM) }));
    renderTable({ container: el, rows,
      columns: [
        { id: 'name', label: '名稱', type: 'text' },
        { id: 'type', label: '類型', width: 100 },
        { id: 'initialBalance', label: '初始餘額', type: 'number', width: 130 },
        { id: '_balance', label: '當前餘額', type: 'number', width: 130 },
      ],
      resolvers: {
        type: (v) => v === 'personal' ? '👤 個人' : '🏠 家庭',
        initialBalance: (v) => `<span class="mono">${formatHKD(v)}</span>`,
        _balance: (v) => `<span class="mono ${v >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(v)}</span>`,
      },
    });
  };

  const paintTxns = (all) => {
    const el = document.getElementById('fin-panel-txns');
    if (!el) return;
    const { year, month } = AppState.getYearMonth();
    const y = year || String(new Date().getFullYear());
    const tM = month === 'all' ? null : month;
    let rows = all;
    if (tM) rows = rows.filter((t) => (t.date || '').startsWith(`${y}-${String(tM).padStart(2, '0')}`));
    else rows = rows.filter((t) => (t.date || '').startsWith(y));
    rows = rows.sort((a, b) => (b.date || '').localeCompare(a.date || ''));
    renderTable({ container: el, rows,
      columns: [
        { id: 'date', label: '日期', type: 'date', width: 110 },
        { id: 'bankName', label: '銀行', width: 100 },
        { id: 'type', label: '類型', width: 90 },
        { id: 'category', label: '分類', width: 100 },
        { id: 'amount', label: '金額', type: 'number', width: 120 },
        { id: 'note', label: '備註' },
      ],
      resolvers: {
        type: (v) => `<span class="badge ${v === 'in' ? 'badge-success' : v === 'transfer' ? 'badge-info' : 'badge-pending'}">${v === 'in' ? '入帳' : v === 'out' ? '出帳' : '轉帳'}</span>`,
        category: (v) => esc({ contribution: '家用轉入', expense: '支出', insurance: '保險', reimbursement: '代墊報銷', manual: '手動' }[v] || v || '—'),
        amount: (v, r) => `<span class="mono ${r.type === 'in' ? 'text-emerald' : 'text-red'}">${r.type === 'in' ? '+' : '-'}${formatHKD(v)}</span>`,
      },
    });
  };

  _tab = initTabPanel({
    containerId: 'fin-tabs', defaultKey: 'banks',
    tabs: [{ key: 'banks', label: '銀行清單', icon: 'landmark', panelId: 'fin-panel-banks' }, { key: 'txns', label: '交易記錄', icon: 'list', panelId: 'fin-panel-txns' }],
    onChange: (k) => {
      document.getElementById('fin-panel-banks').style.display = k === 'banks' ? 'block' : 'none';
      document.getElementById('fin-panel-txns').style.display = k === 'txns' ? 'block' : 'none';
    },
  });
  document.getElementById('fin-panel-banks').style.display = 'block';
  document.getElementById('fin-panel-txns').style.display = 'none';

  const unsubs = [listen('bank_accounts', (snap) => {
    const v = snap.val() || {};
    _accounts = Object.entries(v).map(([id, x]) => ({ id, ...x }));
    paint();
  })];
  _ctrl = renderControls({ container: 'fin-ctrl', items: [{ type: 'year' }, { type: 'month', includeAll: true }] });
  return { destroy: () => { unsubs.forEach((u) => u && u()); _stats?.destroy?.(); _ctrl?.destroy?.(); _tab?.destroy?.(); } };
}

/* ============================================
   2. Annual Report
   ============================================ */
function pageAnnualReport(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">年度報表</h1><p class="page-subtitle"></p></div><div id="ar-actions"></div></div>
    <div id="ar-stats"></div>
    <div id="ar-ctrl"></div>
    <div id="ar-summary"></div>
    <div id="ar-monthly" style="display:none;"></div>`;
  let _stats = null, _ctrl = null, _view = 'summary';
  let _allIncome = {}, _allExpenses = {}, _allPersonalIncome = {};

  const paint = () => {
    const year = AppState.year;
    const yt = computeYearTotals(_allIncome, _allExpenses, year);
    let pi = 0;
    Object.values(_allPersonalIncome || {}).forEach((md) => Object.values((md || {})[year] || {}).forEach((v) => { pi += Number(v) || 0; }));
    _stats?.destroy?.();
    _stats = renderStats({ container: 'ar-stats', columns: 4, cards: [
      { title: `${year} 家用轉入`, value: formatHKD(yt.totalIncome), valueClass: 'text-emerald', icon: 'trending-up' },
      { title: `${year} 個人收入`, value: formatHKD(pi), valueClass: 'text-cyan', icon: 'wallet' },
      { title: `${year} 總支出`, value: formatHKD(yt.totalExpense), valueClass: 'text-red', icon: 'trending-down' },
      { title: `${year} 淨餘額`, value: formatHKD(yt.totalIncome + pi - yt.totalExpense), valueClass: (yt.totalIncome + pi - yt.totalExpense) >= 0 ? 'text-emerald' : 'text-red', icon: 'calculator' },
    ] });
    paintSummary();
    paintMonthly();
  };

  const paintSummary = () => {
    const el = document.getElementById('ar-summary');
    if (!el) return;
    const members = getList('members');
    const rows = computeMemberAnnualRows(members, _allIncome, _allExpenses, _allPersonalIncome, AppState.year);
    el.innerHTML = `<div style="overflow-x:auto;"><div class="glass-card" style="padding:0;"><table class="data-table">
      <thead><tr><th>成員</th><th class="num">家用轉入</th><th class="num">個人收入</th><th class="num">總支出</th><th class="num">淨結餘</th></tr></thead>
      <tbody>${rows.map((r) => `<tr style="${r.__isTotal ? 'font-weight:700;background:var(--hover-bg-strong);' : r.__isShared ? 'background:rgba(220,38,38,0.05);' : ''}">
        <td>${esc(r.name)}</td>
        <td class="num">${formatHKD(r.income)}</td>
        <td class="num">${formatHKD(r.personalIncome)}</td>
        <td class="num">${formatHKD(r.expense)}</td>
        <td class="num ${r.net >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(r.net)}</td>
      </tr>`).join('')}</tbody>
    </table></div></div>`;
  };

  const paintMonthly = () => {
    const el = document.getElementById('ar-monthly');
    if (!el) return;
    const year = AppState.year;
    const m = AppState.month === 'all' ? '01' : AppState.month;
    const monthData = (_allExpenses[year] || {})[m]?.member_expenses || {};
    const incomeM = (_allIncome[year] || {})[m] || {};
    const rows = [];
    Object.entries(incomeM).forEach(([mid, amt]) => rows.push({ name: `收入：${resolveName('members', mid) || mid}`, amount: Number(amt) || 0 }));
    Object.entries(monthData).forEach(([mid, items]) => Object.values(items || {}).forEach((e) => rows.push({ name: `支出：${e.name || ''}`, amount: -(Number(e.amount) || 0) })));
    el.innerHTML = `<div class="glass-card" style="padding:0;"><table class="data-table"><thead><tr><th>項目</th><th class="num">金額</th></tr></thead><tbody>
      ${rows.map((r) => `<tr><td>${esc(r.name)}</td><td class="num ${r.amount >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(r.amount)}</td></tr>`).join('')}
    </tbody></table></div>`;
  };

  const unsubs = [
    listen('income', (snap) => { _allIncome = snap.val() || {}; paint(); }),
    listen('expenses', (snap) => { _allExpenses = snap.val() || {}; paint(); }),
    listen('personal_income', (snap) => { _allPersonalIncome = snap.val() || {}; paint(); }),
  ];

  _ctrl = renderControls({ container: 'ar-ctrl', items: [
    { type: 'year' },
    { type: 'month', includeAll: true },
    { type: 'custom', render: () => `<div class="view-toggle-group">
        <button type="button" class="btn btn-sm btn-primary" data-arv="summary">全年總合</button>
        <button type="button" class="btn btn-sm btn-ghost" data-arv="monthly">月度明細</button>
      </div>` },
  ] });
  document.getElementById('ar-ctrl')?.addEventListener('click', (e) => {
    const b = e.target.closest('[data-arv]');
    if (!b) return;
    _view = b.dataset.arv;
    document.querySelectorAll('[data-arv]').forEach((x) => { x.classList.toggle('btn-primary', x === b); x.classList.toggle('btn-ghost', x !== b); });
    document.getElementById('ar-summary').style.display = _view === 'summary' ? '' : 'none';
    document.getElementById('ar-monthly').style.display = _view === 'monthly' ? '' : 'none';
  });
  return { destroy: () => { unsubs.forEach((u) => u && u()); _stats?.destroy?.(); _ctrl?.destroy?.(); } };
}

/* ============================================
   3. Member Report
   ============================================ */
function pageMemberReport(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">成員收支明細</h1><p class="page-subtitle"></p></div></div>
    <div id="mr-stats"></div>
    <div id="mr-ctrl"></div>
    <div id="mr-list"></div>`;
  let _stats = null, _ctrl = null;
  let _allIncome = {}, _allExpenses = {}, _allPersonalIncome = {};

  const paint = () => {
    const members = getList('members');
    const rows = computeMemberReportRows(members, _allIncome, _allExpenses, _allPersonalIncome, AppState.year);
    const tI = rows.reduce((s, r) => s + r.income, 0);
    const tPI = rows.reduce((s, r) => s + r.personalIncome, 0);
    const tE = rows.reduce((s, r) => s + r.expense, 0);
    const tIns = rows.reduce((s, r) => s + r.insurance, 0);
    _stats?.destroy?.();
    _stats = renderStats({ container: 'mr-stats', columns: 4, cards: [
      { title: `${AppState.year} 家庭收入`, value: formatHKD(tI), valueClass: 'text-emerald', icon: 'trending-up' },
      { title: `${AppState.year} 個人收入`, value: formatHKD(tPI), valueClass: 'text-cyan', icon: 'wallet' },
      { title: `${AppState.year} 總支出`, value: formatHKD(tE), valueClass: 'text-red', icon: 'trending-down' },
      { title: `${AppState.year} 保險平攤`, value: formatHKD(tIns), valueClass: 'text-magenta', icon: 'shield' },
    ] });
    renderTable({ container: 'mr-list', rows,
      columns: [
        { id: 'name', label: '成員', type: 'text', width: 140 },
        { id: 'income', label: '家用轉入', type: 'number', width: 130 },
        { id: 'personalIncome', label: '個人收入', type: 'number', width: 130 },
        { id: 'expense', label: '支出', type: 'number', width: 130 },
        { id: 'insurance', label: '保險', type: 'number', width: 130 },
        { id: 'net', label: '淨額', type: 'number', width: 130 },
      ],
      resolvers: {
        income: (v) => `<span class="text-emerald">${formatHKD(v)}</span>`,
        personalIncome: (v) => `<span class="text-cyan">${formatHKD(v)}</span>`,
        expense: (v) => `<span class="text-red">${formatHKD(v)}</span>`,
        insurance: (v) => `<span class="text-magenta">${formatHKD(v)}</span>`,
        net: (v) => `<span class="${v >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(v)}</span>`,
      },
    });
  };

  const unsubs = [
    listen('members', () => paint()),
    listen('income', (s) => { _allIncome = s.val() || {}; paint(); }),
    listen('expenses', (s) => { _allExpenses = s.val() || {}; paint(); }),
    listen('personal_income', (s) => { _allPersonalIncome = s.val() || {}; paint(); }),
  ];
  _ctrl = renderControls({ container: 'mr-ctrl', items: [{ type: 'year' }] });
  return { destroy: () => { unsubs.forEach((u) => u && u()); _stats?.destroy?.(); _ctrl?.destroy?.(); } };
}

/* ============================================
   4. Settings
   ============================================ */
function pageSettings(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">系統設定</h1><p class="page-subtitle"></p></div></div>
    <div id="st-tabs"></div>
    <div id="st-panel-platform" class="tab-panel"></div>
    <div id="st-panel-banks" class="tab-panel"></div>
    <div id="st-panel-personal" class="tab-panel"></div>`;
  const isSuper = AppState.isSuperAdmin;
  const tabs = [
    ...(isSuper ? [{ key: 'platform', label: '平台設定', icon: 'settings', panelId: 'st-panel-platform' }] : []),
    { key: 'banks', label: '銀行帳號', icon: 'landmark', panelId: 'st-panel-banks' },
    { key: 'personal', label: '個人化', icon: 'user', panelId: 'st-panel-personal' },
  ];
  let _tab = null, _bankUnsub = null, _memberUnsub = null;

  const loadBanks = () => {
    const el = document.getElementById('st-panel-banks');
    if (!el) return;
    renderTable({ container: el, rows: getList('bank_accounts'),
      columns: [
        { id: 'name', label: '名稱', type: 'text' },
        { id: 'type', label: '類型', width: 100 },
        { id: 'initialBalance', label: '初始餘額', type: 'number', width: 130 },
      ],
      resolvers: {
        type: (v) => v === 'personal' ? '👤 個人' : '🏠 家庭',
        initialBalance: (v) => `<span class="mono">${formatHKD(v)}</span>`,
      },
      actions: AppState.getCanInput() ? (row) => [
        { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
          const ok = await openConfirm(`確定刪除「${row.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
          if (!ok) return;
          try { await api.bankAccounts.remove(row.id); toast('✅ 已刪除', 'success'); } catch (e) { toast(e.message, 'error'); }
        } },
      ] : () => [],
    });
  };

  const loadPersonal = () => {
    const el = document.getElementById('st-panel-personal');
    if (!el) return;
    const acc = AppState.getMemberAccount() || {};
    const savedMode = localStorage.getItem(`fin_${DB_TARGET}_ui_stats_mode`) || 'auto';
    
    el.innerHTML = `
    <div style="display:grid;grid-template-columns:repeat(auto-fit, minmax(360px, 1fr));gap:16px;align-items:start;">
      <div class="glass-card">
        <div class="glass-card-title" style="margin-bottom:12px;">帳號資訊</div>
        <div class="form-stack">
          <div class="field"><label class="field-label">登入帳號</label><input class="input mono" disabled value="${esc(acc.account || AppState.currentUser?.email?.split('@')[0] || '')}"></div>
          <div class="field"><label class="field-label">角色</label><input class="input mono" disabled value="${esc(AppState.getRole() || '')}"></div>
          <div class="field"><label class="field-label">家庭名稱</label><input class="input" disabled value="${esc(AppState.getFamilyName() || '')}"></div>
        </div>
        <div style="margin-top:16px;text-align:right;"><button type="button" class="btn btn-danger" id="st-logout"><i data-lucide="log-out"></i> 登出</button></div>
      </div>

      <div class="glass-card">
        <div class="glass-card-title" style="margin-bottom:12px;">統計卡顯示模式</div>
        <p style="font-size:12px;color:var(--text-muted);margin-bottom:12px;">調整頁面頂部統計卡的顯示樣式</p>
        <div style="display:flex;flex-direction:column;gap:8px;">
          <label style="display:flex;align-items:center;gap:8px;cursor:pointer;padding:8px;border-radius:6px;background:rgba(255,255,255,0.03);">
            <input type="radio" name="stats-mode" value="auto" ${savedMode === 'auto' ? 'checked' : ''} style="width:auto;cursor:pointer;">
            <span style="font-size:13px;">自動（依裝置 / 檢視模式）</span>
          </label>
          <label style="display:flex;align-items:center;gap:8px;cursor:pointer;padding:8px;border-radius:6px;background:rgba(255,255,255,0.03);">
            <input type="radio" name="stats-mode" value="integrated" ${savedMode === 'integrated' ? 'checked' : ''} style="width:auto;cursor:pointer;">
            <span style="font-size:13px;">整合卡（單一大卡）</span>
          </label>
          <label style="display:flex;align-items:center;gap:8px;cursor:pointer;padding:8px;border-radius:6px;background:rgba(255,255,255,0.03);">
            <input type="radio" name="stats-mode" value="compact" ${savedMode === 'compact' ? 'checked' : ''} style="width:auto;cursor:pointer;">
            <span style="font-size:13px;">緊湊卡（多個小卡）</span>
          </label>
        </div>
        <div style="margin-top:16px;text-align:right;">
          <button type="button" class="btn btn-primary" id="save-stats-mode"><i data-lucide="check" style="width:14px;height:14px;"></i> 儲存顯示模式</button>
        </div>
      </div>

      <div class="glass-card" style="grid-column:1/-1;">
        <div class="glass-card-title" style="margin-bottom:12px;">家庭成員</div>
        <div id="st-members"></div>
      </div>
    </div>`;

    document.getElementById('st-logout')?.addEventListener('click', async () => {
      const ok = await openConfirm('確定要登出嗎？', { title: '登出', okText: '登出', okClass: 'btn-danger' });
      if (ok) logout();
    });

    document.getElementById('save-stats-mode')?.addEventListener('click', () => {
      const selected = document.querySelector('input[name="stats-mode"]:checked');
      if (selected) {
        localStorage.setItem(`fin_${DB_TARGET}_ui_stats_mode`, selected.value);
        toast('✅ 已儲存顯示模式', 'success');
      }
    });

    renderTable({ container: 'st-members', rows: getList('members'),
      columns: [{ id: 'name', label: '名稱', type: 'text' }, { id: 'role', label: '角色', width: 120 }],
      resolvers: { role: (v) => esc(v || '—') },
    });
  };

  const loadTab = (key) => {
    if (key === 'banks') { loadBanks(); if (!_bankUnsub) _bankUnsub = listen('bank_accounts', loadBanks); }
    else if (key === 'personal') { loadPersonal(); if (!_memberUnsub) _memberUnsub = listen('members', loadPersonal); }
    else if (key === 'platform') {
      const el = document.getElementById('st-panel-platform');
      if (el) el.innerHTML = `<div class="banner">平台設定請至管理員頁面</div>`;
    }
  };

  _tab = initTabPanel({
    containerId: 'st-tabs', tabs, defaultKey: isSuper ? 'platform' : 'banks',
    onChange: (k) => loadTab(k),
  });
  loadTab(isSuper ? 'platform' : 'banks');

  return { destroy: () => { _tab?.destroy?.(); if (_bankUnsub) _bankUnsub(); if (_memberUnsub) _memberUnsub(); } };
}

/* ============================================
   5. Admin
   ============================================ */
function pageAdmin(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">平台管理</h1><p class="page-subtitle"></p></div>
      <div><button type="button" class="btn btn-danger" id="ad-logout"><i data-lucide="log-out"></i> 登出</button></div></div>
    <div class="banner">ℹ️ 管理所有家庭與平台預設資料庫</div>
    <div id="ad-families"></div>`;
  let _families = [];

  const loadFamilies = async () => {
    try {
      const r = await api.adminListFamilies();
      _families = r.families || [];
      renderTable({ container: 'ad-families', rows: _families,
        columns: [
          { id: 'uid', label: 'UID', width: 220 },
          { id: 'name', label: '名稱' },
          { id: 'ownerEmail', label: '擁有者 Email', width: 200 },
          { id: 'createdAt', label: '建立時間', width: 160 },
        ],
        resolvers: {
          uid: (v) => `<span class="mono" style="font-size:12px;">${esc(v)}</span>`,
          ownerEmail: (v) => `<span class="mono" style="font-size:12px;color:var(--text-muted);">${esc(v || '—')}</span>`,
          createdAt: (v) => `<span class="mono" style="font-size:12px;">${v ? new Date(v).toLocaleString('zh-HK') : '—'}</span>`,
        },
        actions: () => [{ label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: (row) => onDelete(row) }],
      });
    } catch (e) { toast('載入失敗：' + e.message, 'error'); }
  };

  const onDelete = async (row) => {
    const ok = await openConfirm(`確定刪除家庭「${row.name || row.uid}」？此操作不可逆。`, { okText: '刪除', okClass: 'btn-danger' });
    if (!ok) return;
    try { await api.adminRemoveFamily(row.uid); toast('✅ 已刪除', 'success'); loadFamilies(); }
    catch (e) { toast('刪除失敗：' + e.message, 'error'); }
  };

  document.getElementById('ad-logout')?.addEventListener('click', async () => {
    const ok = await openConfirm('確定要登出嗎？', { title: '登出', okText: '登出', okClass: 'btn-danger' });
    if (ok) logout();
  });

  loadFamilies();
  return { destroy: () => { root.innerHTML = ''; } };
}

/* ============================================
   6. 註冊
   ============================================ */
registerPage('/finance', pageFinance);
registerPage('/annual-report', pageAnnualReport);
registerPage('/member-report', pageMemberReport);
registerPage('/settings', pageSettings);
registerPage('/admin', pageAdmin);

export { pageFinance, pageAnnualReport, pageMemberReport, pageSettings, pageAdmin };

/* ═══ EOF ═══
F: js/app-pages-reports.js
V: 1792670008
W: webapp-20261022-034#1
═══ */
```

<!-- === FILE END: ./js/app-pages-reports.js === -->

<!-- === FILE START: ./js/app-ui.js === -->

## 📄 ./js/app-ui.js

```js
// ============================================
// app-ui.js — SPA UI 元件集
// 職責：toast / modal / confirm / prompt / form / tab / view-toggle
// 介面：沿用舊版 ui/* 與 shared/*，呼叫端 100% 相容
// ============================================

import { AppState } from './app-core.js';

/* ============================================
   1. 通用工具
   ============================================ */
function _resolveEl(t) {
  if (typeof t === 'string') return document.getElementById(t);
  if (t instanceof HTMLElement) return t;
  return null;
}

export function esc(v, fallback = '') {
  if (v == null) return fallback;
  const map = { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' };
  return String(v).replace(/[&<>"']/g, (c) => map[c]);
}

export function formatHKD(amount) {
  if (amount == null || isNaN(amount)) return 'HK$ 0';
  return 'HK$ ' + Math.round(Number(amount)).toLocaleString('zh-HK');
}

export function formatNumber(amount) {
  if (amount == null || isNaN(amount)) return '0';
  return Math.round(Number(amount)).toLocaleString('zh-HK');
}

/* ============================================
   2. Toast
   ============================================ */
const _TOAST_COLORS = {
  success: 'rgba(16, 185, 129, 0.95)',
  error:   'rgba(244, 63, 94, 0.95)',
  info:    'rgba(0, 240, 255, 0.95)',
  warning: 'rgba(251, 146, 60, 0.95)',
};
const _TOAST_CONTAINER_ID = 'app-toast-container';

export function toast(msg, type = 'success', duration = 2000) {
  let container = document.getElementById(_TOAST_CONTAINER_ID);
  if (!container) {
    container = document.createElement('div');
    container.id = _TOAST_CONTAINER_ID;
    container.style.cssText = 'position:fixed;bottom:30px;left:50%;transform:translateX(-50%);z-index:99999;display:flex;flex-direction:column;align-items:center;pointer-events:none;max-width:90%;';
    document.body.appendChild(container);
  }
  while (container.children.length >= 3) container.removeChild(container.firstChild);

  const el = document.createElement('div');
  el.style.cssText = `background:${_TOAST_COLORS[type] || _TOAST_COLORS.success};color:#fff;padding:12px 22px;border-radius:8px;font-size:14px;box-shadow:0 4px 20px rgba(0,0,0,0.4);opacity:0;transform:translateY(10px);transition:opacity 0.3s ease,transform 0.3s ease;max-width:90%;text-align:center;pointer-events:none;margin-top:8px;`;
  el.textContent = msg;
  container.appendChild(el);

  requestAnimationFrame(() => {
    el.style.opacity = '1';
    el.style.transform = 'translateY(0)';
  });
  setTimeout(() => {
    el.style.opacity = '0';
    el.style.transform = 'translateY(10px)';
    setTimeout(() => { if (el.parentNode) el.parentNode.removeChild(el); }, 300);
  }, duration);
}

export const toastSuccess = (m, d) => toast(m, 'success', d);
export const toastError   = (m, d) => toast(m, 'error', d);
export const toastInfo    = (m, d) => toast(m, 'info', d);
export const toastWarning = (m, d) => toast(m, 'warning', d);

/* ============================================
   3. Modal / Confirm / Prompt
   ============================================ */
const _stack = [];
let _confirmResolve = null;
let _promptResolve = null;

export function openModal(target) {
  const m = _resolveEl(target);
  if (!m) return;
  m.classList.add('active');
  document.body.style.overflow = 'hidden';
  if (!_stack.includes(m)) _stack.push(m);
  if (!window._modalEscBound) {
    window._modalEscBound = true;
    document.addEventListener('keydown', (e) => {
      if (e.key !== 'Escape') return;
      if (_stack.length === 0) return;
      closeModal(_stack[_stack.length - 1]);
    });
  }
}

export function closeModal(target) {
  const m = _resolveEl(target);
  if (!m) return;
  m.classList.remove('active');
  const i = _stack.indexOf(m);
  if (i >= 0) _stack.splice(i, 1);
  if (_stack.length === 0) document.body.style.overflow = '';
}

export function destroyModal(target) {
  const m = _resolveEl(target);
  if (!m) return;
  closeModal(m);
  m.remove();
}

export function openConfirm(message, opts = {}) {
  const { title = '確認', okText = '確定', cancelText = '取消', okClass = 'btn-danger' } = opts;

  const oldOverlay = document.getElementById('__confirm-modal');
  if (oldOverlay) {
    if (oldOverlay._cleanup) oldOverlay._cleanup();
    oldOverlay.remove();
    document.body.style.overflow = '';
    if (_confirmResolve) { _confirmResolve(false); _confirmResolve = null; }
  }

  return new Promise((resolve) => {
    _confirmResolve = resolve;
    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay active';
    overlay.id = '__confirm-modal';
    overlay.innerHTML = `
      <div class="modal" style="max-width:400px;">
        <h2 class="modal-title">${esc(title)}</h2>
        <div style="font-size:14px;line-height:1.6;color:var(--text-secondary);white-space:pre-wrap;">${esc(message)}</div>
        <div class="modal-actions">
          <button type="button" class="btn btn-ghost" data-confirm-action="cancel">${esc(cancelText)}</button>
          <button type="button" class="btn ${okClass}" data-confirm-action="ok">${esc(okText)}</button>
        </div>
      </div>`;
    document.body.appendChild(overlay);
    document.body.style.overflow = 'hidden';

    const onClick = (e) => {
      const btn = e.target.closest('[data-confirm-action]');
      if (btn) _closeConfirm(btn.dataset.confirmAction === 'ok');
      else if (e.target === overlay) _closeConfirm(false);
    };
    const onKey = (e) => {
      if (e.key === 'Escape') _closeConfirm(false);
      if (e.key === 'Enter') _closeConfirm(true);
    };
    overlay.addEventListener('click', onClick);
    document.addEventListener('keydown', onKey);
    overlay._cleanup = () => {
      overlay.removeEventListener('click', onClick);
      document.removeEventListener('keydown', onKey);
    };
  });
}

function _closeConfirm(r) {
  const o = document.getElementById('__confirm-modal');
  if (o) { if (o._cleanup) o._cleanup(); o.remove(); }
  document.body.style.overflow = '';
  if (_confirmResolve) { _confirmResolve(r); _confirmResolve = null; }
}

export function openPrompt(opts = {}) {
  const { title = '請輸入', label = '', placeholder = '', defaultValue = '', okText = '確定', cancelText = '取消', maxlength = 60 } = opts;

  const oldOverlay = document.getElementById('__prompt-modal');
  if (oldOverlay) {
    if (oldOverlay._cleanup) oldOverlay._cleanup();
    oldOverlay.remove();
    document.body.style.overflow = '';
    if (_promptResolve) { _promptResolve(null); _promptResolve = null; }
  }

  return new Promise((resolve) => {
    _promptResolve = resolve;
    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay active';
    overlay.id = '__prompt-modal';
    overlay.innerHTML = `
      <div class="modal" style="max-width:400px;">
        <h2 class="modal-title">${esc(title)}</h2>
        ${label ? `<label class="field-label" style="display:block;margin-bottom:6px;">${esc(label)}</label>` : ''}
        <input type="text" class="input" id="__prompt-input" placeholder="${esc(placeholder)}" value="${esc(defaultValue)}" maxlength="${maxlength}" style="width:100%;margin-bottom:16px;">
        <div class="modal-actions">
          <button type="button" class="btn btn-ghost" data-prompt-action="cancel">${esc(cancelText)}</button>
          <button type="button" class="btn btn-primary" data-prompt-action="ok">${esc(okText)}</button>
        </div>
      </div>`;
    document.body.appendChild(overlay);
    document.body.style.overflow = 'hidden';

    const input = overlay.querySelector('#__prompt-input');
    const onClick = (e) => {
      const btn = e.target.closest('[data-prompt-action]');
      if (!btn) { if (e.target === overlay) _closePrompt(null); return; }
      if (btn.dataset.promptAction === 'cancel') _closePrompt(null);
      else {
        const v = (input.value || '').trim();
        if (!v) { input.focus(); return; }
        _closePrompt(v);
      }
    };
    const onKey = (e) => {
      if (e.key === 'Escape') _closePrompt(null);
      if (e.key === 'Enter') {
        const v = (input.value || '').trim();
        if (!v) { input.focus(); return; }
        _closePrompt(v);
      }
    };
    overlay.addEventListener('click', onClick);
    document.addEventListener('keydown', onKey);
    overlay._cleanup = () => {
      overlay.removeEventListener('click', onClick);
      document.removeEventListener('keydown', onKey);
    };
    setTimeout(() => { input.focus(); if (defaultValue) input.select(); }, 50);
  });
}

function _closePrompt(r) {
  const o = document.getElementById('__prompt-modal');
  if (o) { if (o._cleanup) o._cleanup(); o.remove(); }
  document.body.style.overflow = '';
  if (_promptResolve) { _promptResolve(r); _promptResolve = null; }
}

/* ============================================
   4. Form Builder
   ============================================ */
export function buildForm(opts) {
  const {
    containerId, fields = [], idPrefix = '', submitText = '儲存', cancelText = '取消',
    resetText = '重置', showCancel = true, showReset = false,
    onSubmit, onCancel, onReset, beforeSubmit, autoValidate = true, initialData = null,
  } = opts;

  const root = document.getElementById(containerId);
  if (!root) { console.warn(`[buildForm] 找不到 #${containerId}`); return null; }

  const fieldIdMap = {};
  fields.forEach((f) => {
    if (f.type === 'custom' || f.type === 'hidden') return;
    fieldIdMap[f.id] = idPrefix ? `${idPrefix}${f.id}` : f.id;
  });

  const fieldsHtml = _renderFields(fields, fieldIdMap);
  const buttons = [];
  if (showReset) buttons.push(`<button type="button" class="btn btn-ghost" data-form-action="reset">${esc(resetText)}</button>`);
  if (showCancel) buttons.push(`<button type="button" class="btn btn-ghost" data-form-action="cancel">${esc(cancelText)}</button>`);
  buttons.push(`<button type="submit" class="btn btn-primary" data-form-action="submit"><span>${esc(submitText)}</span></button>`);

  root.innerHTML = `
    <form class="form-builder-form" novalidate>
      <div class="form-stack">${fieldsHtml}</div>
      <div style="display:flex;justify-content:flex-end;gap:10px;margin-top:16px;">${buttons.join('')}</div>
    </form>`;

  const form = root.querySelector('form');
  const submitBtn = form.querySelector('[data-form-action="submit"]');
  const submitLabel = submitBtn.querySelector('span');
  let _submitting = false;

  form.addEventListener('submit', async (e) => {
    e.preventDefault();
    if (_submitting) return;
    _clearErrors();

    const data = _collectData(fields, fieldIdMap);

    if (autoValidate) {
      const err = _validateRequired(fields, data, fieldIdMap);
      if (err) {
        _showFieldError(err.field, err.message, fieldIdMap);
        toast(err.message, 'warning');
        document.getElementById(err.field)?.focus();
        return;
      }
    }

    if (typeof beforeSubmit === 'function') {
      let r;
      try { r = await beforeSubmit(data); } catch (er) { toast('驗證失敗：' + (er.message || er), 'error'); return; }
      if (r === false) return;
      if (typeof r === 'string') { toast(r, 'warning'); return; }
      if (r && r.field && r.message) {
        const rf = fieldIdMap[r.field] || r.field;
        _showFieldError(rf, r.message, fieldIdMap);
        toast(r.message, 'warning');
        return;
      }
    }

    if (typeof onSubmit !== 'function') return;
    _setSubmitting(true);
    try { await onSubmit(data, api); }
    catch (er) { console.error('[buildForm] 送出失敗：', er); toast('送出失敗：' + (er.message || er), 'error'); }
    finally { _setSubmitting(false); }
  });

  form.addEventListener('click', (e) => {
    const btn = e.target.closest('[data-form-action]');
    if (!btn) return;
    if (btn.dataset.formAction === 'cancel' && typeof onCancel === 'function') onCancel();
    else if (btn.dataset.formAction === 'reset') {
      form.reset(); _clearErrors();
      if (typeof onReset === 'function') onReset();
    }
  });

  function _setSubmitting(b) {
    _submitting = b;
    submitBtn.disabled = b;
    if (submitLabel) submitLabel.textContent = b ? '處理中…' : submitText;
  }

  function _clearErrors() {
    form.querySelectorAll('.field-error').forEach((el) => el.remove());
    form.querySelectorAll('.field.has-error').forEach((el) => el.classList.remove('has-error'));
  }

  function _showFieldError(fieldId, message, map) {
    const rid = map && map[fieldId] ? map[fieldId] : fieldId;
    const el = document.getElementById(rid);
    if (!el) return;
    const fEl = el.closest('.field');
    if (!fEl) return;
    fEl.classList.add('has-error');
    const old = fEl.querySelector('.field-error');
    if (old) old.remove();
    const div = document.createElement('div');
    div.className = 'field-error';
    div.style.cssText = 'color:var(--neon-red);font-size:11px;margin-top:4px;';
    div.textContent = message;
    fEl.appendChild(div);
  }

  const api = {
    root, form,
    getData: () => _collectData(fields, fieldIdMap),
    setData: (data) => {
      Object.entries(data || {}).forEach(([k, v]) => {
        const rid = fieldIdMap[k] || k;
        const el = document.getElementById(rid);
        if (!el) return;
        if (el.type === 'checkbox') el.checked = !!v;
        else el.value = v ?? '';
      });
    },
    setFieldValue: (fid, v) => {
      const rid = fieldIdMap[fid] || fid;
      const el = document.getElementById(rid);
      if (!el) return;
      if (el.type === 'checkbox') el.checked = !!v;
      else el.value = v ?? '';
    },
    getFieldValue: (fid) => {
      const rid = fieldIdMap[fid] || fid;
      const el = document.getElementById(rid);
      if (!el) return null;
      if (el.type === 'checkbox') return el.checked;
      if (el.type === 'number') { const n = Number(el.value); return isNaN(n) ? 0 : n; }
      return el.value;
    },
    reset: () => { form.reset(); _clearErrors(); if (typeof onReset === 'function') onReset(); },
    setError: (fid, msg) => _showFieldError(fid, msg, fieldIdMap),
    clearErrors: _clearErrors,
    updateOptions: (fid, options, cfg = {}) => {
      const rid = fieldIdMap[fid] || fid;
      const el = document.getElementById(rid);
      if (!el) return;
      const { includeEmpty = true, emptyText = '— 請選擇 —' } = cfg;
      const cur = el.value;
      let html = includeEmpty ? `<option value="">${esc(emptyText)}</option>` : '';
      html += options.map((o) => `<option value="${esc(o.value)}">${esc(o.label)}</option>`).join('');
      el.innerHTML = html;
      if (cur && options.some((o) => String(o.value) === cur)) el.value = cur;
    },
    onFieldChange: (fid, cb) => {
      const rid = fieldIdMap[fid] || fid;
      const el = document.getElementById(rid);
      if (!el) return () => {};
      el.addEventListener('change', cb);
      return () => el.removeEventListener('change', cb);
    },
    submit: () => form.dispatchEvent(new Event('submit', { cancelable: true, bubbles: true })),
    setSubmitting: _setSubmitting,
    destroy: () => { root.innerHTML = ''; },
  };

  /* 初始化：設預設值、載入初始資料 */
  fields.forEach((f) => {
    if (initialData && initialData[f.id] !== undefined) api.setFieldValue(f.id, initialData[f.id]);
    else if (f.defaultValue != null) {
      const def = typeof f.defaultValue === 'function' ? f.defaultValue() : f.defaultValue;
      api.setFieldValue(f.id, def);
    }
  });

  if (window.lucide) window.lucide.createIcons();
  return api;
}

function _renderFields(fields, fieldIdMap) {
  const out = [];
  const buf = [];
  const flush = () => { if (buf.length) { out.push(`<div class="form-stack-row">${buf.join('')}</div>`); buf.length = 0; } };
  fields.forEach((f) => {
    const html = _renderField(f, fieldIdMap);
    if (f.layout === 'half') { buf.push(html); if (buf.length >= 2) flush(); }
    else { flush(); out.push(html); }
  });
  flush();
  return out.join('');
}

function _renderField(f, fieldIdMap) {
  const { type, id, label, required = false, placeholder = '', options = [], min, max, step, disabled = false, hint } = f;
  const fid = fieldIdMap[id] || id;
  let input = '';

  if (type === 'select') {
    let opts = f.includeEmpty !== false ? `<option value="">${esc(f.emptyText || '— 請選擇 —')}</option>` : '';
    opts += options.map((o) => `<option value="${esc(o.value)}">${esc(o.label)}</option>`).join('');
    input = `<select class="select" id="${fid}" ${required ? 'required' : ''} ${disabled ? 'disabled' : ''}>${opts}</select>`;
  } else if (type === 'number' || type === 'number-plain') {
    const attrs = [min != null ? `min="${min}"` : '', max != null ? `max="${max}"` : '', step != null ? `step="${step}"` : 'step="1"'].filter(Boolean).join(' ');
    const cls = type === 'number' ? 'input mono' : 'input';
    input = `<input class="${cls}" id="${fid}" type="number" ${attrs} ${required ? 'required' : ''} ${disabled ? 'disabled' : ''} placeholder="${esc(placeholder)}">`;
  } else if (type === 'text') {
    input = `<input class="input" id="${fid}" type="text" ${required ? 'required' : ''} ${disabled ? 'disabled' : ''} placeholder="${esc(placeholder)}" maxlength="${f.maxlength || 60}">`;
  } else if (type === 'textarea') {
    input = `<textarea class="input" id="${fid}" ${required ? 'required' : ''} ${disabled ? 'disabled' : ''} placeholder="${esc(placeholder)}" maxlength="${f.maxlength || 200}" rows="${f.rows || 3}"></textarea>`;
  } else if (type === 'checkbox') {
    return `<div class="field" style="display:flex;align-items:center;gap:8px;"><input type="checkbox" id="${fid}" style="width:auto;cursor:pointer;" ${disabled ? 'disabled' : ''}><label for="${fid}" style="font-size:13px;color:var(--text-secondary);cursor:pointer;">${esc(label)}</label></div>`;
  } else if (type === 'custom') {
    return `<div class="field">${f.html || ''}</div>`;
  } else if (type === 'hidden') {
    return `<input type="hidden" id="${fid}" value="${esc(f.value || '')}">`;
  }

  const hintHtml = hint ? `<div style="margin-top:4px;font-size:11px;color:var(--text-muted);">${esc(hint)}</div>` : '';
  return `<div class="field"><label class="field-label" for="${fid}">${esc(label)}${required ? ' *' : ''}</label>${input}${hintHtml}</div>`;
}

function _collectData(fields, fieldIdMap) {
  const d = {};
  fields.forEach((f) => {
    if (f.type === 'custom') return;
    const fid = fieldIdMap[f.id] || f.id;
    const el = document.getElementById(fid);
    if (!el) return;
    if (f.type === 'checkbox') d[f.id] = el.checked;
    else if (f.type === 'number' || f.type === 'number-plain') d[f.id] = el.value === '' ? 0 : (isNaN(Number(el.value)) ? 0 : Number(el.value));
    else d[f.id] = el.value;
  });
  return d;
}

function _validateRequired(fields, data, fieldIdMap) {
  for (const f of fields) {
    if (!f.required) continue;
    if (f.type === 'custom' || f.type === 'hidden') continue;
    const v = data[f.id];
    if (v === '' || v === null || v === undefined) {
      return { field: fieldIdMap[f.id] || f.id, message: `請填寫「${f.label}」` };
    }
  }
  return null;
}

/* ============================================
   5. Tab Panel
   ============================================ */
export function initTabPanel(opts) {
  const { containerId, tabs = [], defaultKey, storageKey, wrap = false, onChange } = opts;
  const root = document.getElementById(containerId);
  if (!root || tabs.length === 0) return null;

  const norm = tabs.map((t) => ({ ...t, panelId: t.panelId || `${containerId}-panel-${t.key}` }));
  const fullKey = storageKey ? `fin_ui_${storageKey.replace(/^fin_ui_/, '')}` : '';

  let initialKey = defaultKey;
  if (fullKey) {
    try { const s = localStorage.getItem(fullKey); if (s && norm.some((t) => t.key === s)) initialKey = s; } catch (e) {}
  }
  if (!initialKey || !norm.some((t) => t.key === initialKey)) initialKey = norm[0].key;

  const wrapCls = wrap ? ' tab-bar-wrap' : '';
  root.innerHTML = `<div class="tab-bar${wrapCls}">${norm.map((t) => `
    <button type="button" class="tab-btn ${t.key === initialKey ? 'active' : ''}" data-tab-key="${esc(t.key)}">
      ${t.icon ? `<i data-lucide="${esc(t.icon)}" style="width:14px;height:14px;"></i>` : ''}
      <span>${esc(t.label)}</span>
    </button>`).join('')}</div>`;

  let _cur = initialKey;

  function applyState() {
    root.querySelectorAll('.tab-btn').forEach((b) => b.classList.toggle('active', b.dataset.tabKey === _cur));
    norm.forEach((t) => {
      const p = document.getElementById(t.panelId);
      if (!p) return;
      const on = t.key === _cur;
      p.classList.toggle('active', on);
      p.style.display = on ? 'block' : 'none';
    });
  }

  function switchTo(key) {
    if (!norm.some((t) => t.key === key) || key === _cur) return;
    _cur = key;
    applyState();
    if (fullKey) try { localStorage.setItem(fullKey, key); } catch (e) {}
    if (typeof onChange === 'function') {
      const tab = norm.find((t) => t.key === key);
      try { onChange(key, tab); } catch (e) {}
    }
  }

  root.addEventListener('click', (e) => {
    const b = e.target.closest('.tab-btn');
    if (b && b.dataset.tabKey) switchTo(b.dataset.tabKey);
  });

  applyState();
  if (typeof onChange === 'function') {
    const tab = norm.find((t) => t.key === initialKey);
    try { onChange(initialKey, tab); } catch (e) {}
  }
  if (window.lucide) window.lucide.createIcons();

  return { root, tabs: norm, getCurrent: () => _cur, switchTo, destroy: () => { root.innerHTML = ''; } };
}

/* ============================================
   6. View Toggle
   ============================================ */
export function initViewToggle(opts) {
  const {
    containerId, storageKey, defaultView = 'card', cardText = '卡片模式',
    tableText = '表格模式', onChange, syncAppState = true,
  } = opts;
  const root = document.getElementById(containerId);
  if (!root) return null;

  const fullKey = storageKey ? `fin_ui_view_${storageKey.replace(/^fin_ui_view_/, '')}` : '';
  let cur = defaultView;
  try { const s = localStorage.getItem(fullKey); if (s === 'card' || s === 'table') cur = s; } catch (e) {}
  if (syncAppState) AppState.setCurrentView(cur);

  function render() {
    root.innerHTML = `<div class="view-toggle-group">
      <button type="button" class="btn btn-sm ${cur === 'card' ? 'btn-primary' : 'btn-ghost'}" data-view="card">${esc(cardText)}</button>
      <button type="button" class="btn btn-sm ${cur === 'table' ? 'btn-primary' : 'btn-ghost'}" data-view="table">${esc(tableText)}</button>
    </div>`;
    if (window.lucide) window.lucide.createIcons();
  }

  function setView(v) {
    if (v !== 'card' && v !== 'table') return;
    if (v === cur) return;
    cur = v;
    render();
    try { localStorage.setItem(fullKey, v); } catch (e) {}
    if (syncAppState) AppState.setCurrentView(v);
    if (typeof onChange === 'function') onChange(v);
  }

  root.addEventListener('click', (e) => {
    const b = e.target.closest('button[data-view]');
    if (b) setView(b.dataset.view);
  });

  render();
  return { root, getView: () => cur, setView, destroy: () => { root.innerHTML = ''; } };
}```

<!-- === FILE END: ./js/app-ui.js === -->

<!-- === FILE START: ./js/app-widgets.js === -->

## 📄 ./js/app-widgets.js

```js
// ============================================
// app-widgets.js — SPA 展示元件集
// 職責：data-table / data-card / stats-cards / page-controls / entity-list-page
// 介面：沿用舊版 shared/* 與 blocks/*，呼叫端 100% 相容
// ============================================

import { AppState } from './app-core.js';
import { esc, formatHKD, openConfirm, toast, initViewToggle } from './app-ui.js';
import { api, listen, resolveName, statusLabel, statusBadgeClass } from './app-data.js';

/* ============================================
   1. 通用渲染（就地更新）
   ============================================ */
function _paint(root, html) {
  if (root.__lastHtml === html) return;
  root.__lastHtml = html;
  root.innerHTML = html;
  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   2. 統計卡（合併舊 stats-block + stats-cards）
   ============================================ */
export function renderStats({ container, cards = [], columns = 3 } = {}) {
  const root = typeof container === 'string' ? document.getElementById(container) : container;
  if (!root) return null;

  function paint() {
    if (!cards.length) { _paint(root, ''); return; }
    const cols = Math.min(columns, cards.length);
    const html = `<div class="stats-compact-grid stats-compact-grid-${cols}" style="display:grid;grid-template-columns:repeat(${cols},1fr);gap:8px;margin-bottom:20px;">
      ${cards.map((c) => `
        <div class="glass-card" style="padding:12px 14px;">
          <div class="glass-card-title" style="display:flex;align-items:center;gap:4px;font-size:10px;margin-bottom:6px;">
            ${c.icon ? `<i data-lucide="${esc(c.icon)}" style="width:12px;height:12px;"></i>` : ''}
            <span>${esc(c.title || '')}</span>
          </div>
          <div class="mono ${esc(c.valueClass || '')}" style="font-size:17px;font-weight:700;color:var(--primary);word-break:break-all;">${c.value == null ? '—' : esc(String(c.value))}</div>
          ${c.hint ? `<div style="font-size:10px;color:var(--text-muted);margin-top:4px;">${esc(c.hint)}</div>` : ''}
        </div>`).join('')}
    </div>`;
    _paint(root, html);
  }
  paint();
  return { root, refresh: paint, destroy: () => { root.innerHTML = ''; root.__lastHtml = ''; } };
}

/* ============================================
   3. 表格（合併舊 data-table + column-settings）
   ============================================ */
const _DT_LISTENER = '__widgets_dt__';

export function renderTable({ container, rows = [], columns = [], resolvers = {}, actions = () => [], emptyText = '尚無資料', storageKey = null }) {
  const root = typeof container === 'string' ? document.getElementById(container) : container;
  if (!root) return null;

  let _sortState = { id: null, dir: null };
  let _filterState = {};
  let _visible = columns.map((c) => c.id);
  if (storageKey) {
    try {
      const raw = localStorage.getItem(`fin_ui_col_${storageKey}`);
      if (raw) {
        const d = JSON.parse(raw);
        if (Array.isArray(d.visible)) {
          _visible = d.visible.filter((id) => columns.some((c) => c.id === id));
          columns.forEach((c) => { if (!_visible.includes(c.id)) _visible.push(c.id); });
        }
      }
    } catch (e) {}
  }

  const sortable = (c) => c.type && ['text', 'number', 'date', 'select'].includes(c.type);
  const filterable = (c) => Array.isArray(c.filterOptions);

  function computeRows() {
    let r = [...rows];
    Object.entries(_filterState).forEach(([k, v]) => {
      if (v === '' || v == null) return;
      r = r.filter((x) => String(x[k] ?? '') === String(v));
    });
    if (_sortState.id && _sortState.dir) {
      const col = columns.find((c) => c.id === _sortState.id);
      if (col) {
        const dir = _sortState.dir === 'asc' ? 1 : -1;
        const t = col.type;
        r.sort((a, b) => t === 'number'
          ? ((Number(a[_sortState.id]) || 0) - (Number(b[_sortState.id]) || 0)) * dir
          : String(a[_sortState.id] ?? '').localeCompare(String(b[_sortState.id] ?? ''), 'zh-HK') * dir);
      }
    }
    return r;
  }

  function paint() {
    const display = computeRows();
    if (display.length === 0) { _paint(root, `<div class="glass-card"><div class="empty-state">${esc(emptyText)}</div></div>`); bind(); return; }

    const visCols = _visible.map((id) => columns.find((c) => c.id === id)).filter(Boolean);
    const hasActions = typeof actions === 'function';

    const head = `<thead><tr>${visCols.map((c) => {
      const sortIcon = _sortState.id === c.id ? (_sortState.dir === 'asc' ? 'chevron-up' : 'chevron-down') : 'chevrons-up-down';
      const sBtn = sortable(c) ? `<button type="button" data-dt-sort="${esc(c.id)}" style="background:${_sortState.id === c.id ? 'var(--primary)' : 'transparent'};border:none;cursor:pointer;padding:2px 4px;border-radius:3px;color:${_sortState.id === c.id ? '#fff' : 'var(--text-muted)'};"><i data-lucide="${sortIcon}" style="width:11px;height:11px;"></i></button>` : '';
      const isFiltered = _filterState[c.id] != null && _filterState[c.id] !== '';
      const fBtn = filterable(c) ? `<button type="button" data-dt-filter="${esc(c.id)}" style="background:${isFiltered ? 'var(--accent-warm)' : 'transparent'};border:none;cursor:pointer;padding:2px 4px;border-radius:3px;color:${isFiltered ? '#fff' : 'var(--text-muted)'};"><i data-lucide="filter" style="width:11px;height:11px;"></i></button>` : '';
      return `<th class="${c.type === 'number' ? 'num' : ''}" style="${c.width ? `width:${c.width}px;` : ''}"><span>${esc(c.label)}</span>${sBtn}${fBtn}</th>`;
    }).join('')}${hasActions ? '<th style="width:160px;">操作</th>' : ''}</tr></thead>`;

    const body = display.map((row, i) => `<tr data-row-index="${i}">
      ${visCols.map((c) => {
        const resolver = resolvers[c.id];
        const content = resolver ? resolver(row[c.id], row) : (row[c.id] == null ? '—' : esc(String(row[c.id])));
        return `<td class="${c.type === 'number' ? 'num' : ''}">${content}</td>`;
      }).join('')}
      ${hasActions ? `<td>${(actions(row) || []).map((a) => `<button type="button" class="btn btn-sm ${esc(a.className || 'btn-ghost')}" data-act="${esc(a.action || 'custom')}" data-idx="${i}">${a.icon ? `<i data-lucide="${esc(a.icon)}" style="width:14px;height:14px;"></i>` : ''}${esc(a.label)}</button>`).join('')}</td>` : ''}
    </tr>`).join('');

    const html = `<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;flex-wrap:wrap;gap:8px;">
        <span style="font-size:12px;color:var(--text-muted);">共 ${display.length} 筆</span>
        <button type="button" class="btn btn-sm btn-ghost" data-dt-cols><i data-lucide="columns" style="width:14px;height:14px;"></i><span>欄位</span></button>
      </div>
      <div style="overflow-x:auto;"><div class="glass-card" style="padding:0;">
        <table class="data-table" style="width:100%;min-width:100%;">${head}<tbody>${body}</tbody></table>
      </div></div>`;
    _paint(root, html);
    bind();
  }

  function bind() {
    if (root[_DT_LISTENER]) root.removeEventListener('click', root[_DT_LISTENER]);
    const h = (e) => {
      const sBtn = e.target.closest('[data-dt-sort]');
      if (sBtn) {
        e.stopPropagation();
        const id = sBtn.dataset.dtSort;
        if (_sortState.id !== id) _sortState = { id, dir: 'asc' };
        else if (_sortState.dir === 'asc') _sortState = { id, dir: 'desc' };
        else _sortState = { id: null, dir: null };
        paint(); return;
      }
      const fBtn = e.target.closest('[data-dt-filter]');
      if (fBtn) { e.stopPropagation(); openFilter(fBtn.dataset.dtFilter); return; }
      const cBtn = e.target.closest('[data-dt-cols]');
      if (cBtn) { e.stopPropagation(); openCols(); return; }
      const act = e.target.closest('button[data-act]');
      if (act) {
        const tr = act.closest('tr[data-row-index]');
        if (!tr) return;
        const row = computeRows()[Number(tr.dataset.idx)];
        if (!row) return;
        const a = (actions(row) || []).find((x) => (x.action || 'custom') === act.dataset.act);
        if (a?.onClick) a.onClick(row);
      }
    };
    root.addEventListener('click', h);
    root[_DT_LISTENER] = h;
  }

  function openFilter(colId) {
    const col = columns.find((c) => c.id === colId);
    if (!col || !col.filterOptions) return;
    const th = root.querySelector(`[data-dt-filter="${colId}"]`);
    if (!th) return;
    const rect = th.getBoundingClientRect();
    const cur = _filterState[colId] ?? '';

    const panel = document.createElement('div');
    panel.style.cssText = `position:fixed;top:${rect.bottom + 4}px;left:${rect.left}px;z-index:2000;background:var(--modal-bg);border:1px solid var(--glass-border);border-radius:var(--radius-sm);padding:12px;min-width:200px;box-shadow:var(--shadow-lg);`;
    panel.innerHTML = `
      <div style="font-size:12px;color:var(--text-muted);margin-bottom:8px;">${esc(col.label)}</div>
      <select class="select" style="width:100%;padding:6px 10px;font-size:13px;">
        <option value="">— 全部 —</option>
        ${col.filterOptions.map((o) => `<option value="${esc(o.value)}" ${String(o.value) === String(cur) ? 'selected' : ''}>${esc(o.label)}</option>`).join('')}
      </select>
      <div style="display:flex;gap:6px;justify-content:flex-end;margin-top:10px;">
        <button type="button" class="btn btn-sm btn-ghost" data-clear>清除</button>
        <button type="button" class="btn btn-sm btn-primary" data-apply>套用</button>
      </div>`;
    document.body.appendChild(panel);

    panel.addEventListener('click', (e) => {
      if (e.target.closest('[data-clear]')) { delete _filterState[colId]; panel.remove(); paint(); }
      else if (e.target.closest('[data-apply]')) {
        const v = panel.querySelector('select').value;
        if (v === '') delete _filterState[colId]; else _filterState[colId] = v;
        panel.remove(); paint();
      }
    });
    const outside = (ev) => { if (!panel.contains(ev.target)) { panel.remove(); document.removeEventListener('mousedown', outside); } };
    setTimeout(() => document.addEventListener('mousedown', outside), 0);
  }

  function openCols() {
    const o = document.createElement('div');
    o.className = 'modal-overlay active';
    o.innerHTML = `<div class="modal" style="max-width:480px;max-height:90vh;overflow-y:auto;">
      <h2 class="modal-title">欄位設定</h2>
      <div id="__dt_cols_list"></div>
      <div class="modal-actions" style="margin-top:16px;">
        <button type="button" class="btn btn-ghost" data-cancel>取消</button>
        <button type="button" class="btn btn-primary" data-save>儲存</button>
      </div></div>`;
    document.body.appendChild(o);
    const listEl = o.querySelector('#__dt_cols_list');
    const temp = columns.map((c) => ({ id: c.id, label: c.label, visible: _visible.includes(c.id) }));
    const render = () => {
      listEl.innerHTML = temp.map((c, i) => `
        <div style="display:flex;align-items:center;gap:8px;padding:8px;border-bottom:1px solid var(--divider);">
          <input type="checkbox" data-idx="${i}" ${c.visible ? 'checked' : ''} style="width:auto;cursor:pointer;">
          <span style="font-size:13px;">${esc(c.label)}</span>
        </div>`).join('');
    };
    render();
    listEl.addEventListener('change', (e) => {
      const cb = e.target.closest('input[type="checkbox"]');
      if (cb) temp[Number(cb.dataset.idx)].visible = cb.checked;
    });
    o.addEventListener('click', (e) => {
      if (e.target === o || e.target.closest('[data-cancel]')) o.remove();
      else if (e.target.closest('[data-save]')) {
        _visible = temp.filter((c) => c.visible).map((c) => c.id);
        if (_visible.length === 0) _visible = [columns[0].id];
        if (storageKey) try { localStorage.setItem(`fin_ui_col_${storageKey}`, JSON.stringify({ visible: _visible })); } catch (er) {}
        o.remove(); paint();
      }
    });
  }

  paint();
  return { root, refresh: paint, destroy: () => {
    if (root[_DT_LISTENER]) root.removeEventListener('click', root[_DT_LISTENER]);
    root.innerHTML = ''; root.__lastHtml = '';
  } };
}

/* ============================================
   4. 卡片（合併舊 data-card）
   ============================================ */
export function renderCards({ container, rows = [], columns = [], resolvers = {}, actions = () => [], primaryCol = 'name', cardFields = null, gridClass = 'grid grid-3' }) {
  const root = typeof container === 'string' ? document.getElementById(container) : container;
  if (!root) return null;

  const fields = cardFields || columns.slice(1, 4).map((c) => c.id);
  const colMap = {};
  columns.forEach((c) => { colMap[c.id] = c; });
  const hasActions = typeof actions === 'function';

  function paint() {
    if (!rows.length) { _paint(root, `<div class="glass-card"><div class="empty-state">尚無資料</div></div>`); return; }
    const html = `<div class="${gridClass}" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:12px;">
      ${rows.map((row, i) => {
        const fieldsHtml = fields.map((id) => {
          const col = colMap[id];
          if (!col) return '';
          const r = resolvers[id];
          const content = r ? r(row[id], row) : (row[id] == null ? '—' : esc(String(row[id])));
          return `<div style="display:flex;justify-content:space-between;gap:12px;"><span style="color:var(--text-muted);font-size:11px;">${esc(col.label)}</span><span style="text-align:right;">${content}</span></div>`;
        }).join('');
        const actsHtml = hasActions
          ? `<div style="margin-top:12px;padding-top:12px;border-top:1px dashed var(--divider-strong);display:flex;gap:8px;justify-content:flex-end;flex-wrap:wrap;">
              ${(actions(row) || []).map((a) => `<button type="button" class="btn btn-sm ${esc(a.className || 'btn-ghost')}" data-act="${esc(a.action || 'custom')}" data-idx="${i}">${a.icon ? `<i data-lucide="${esc(a.icon)}" style="width:14px;height:14px;"></i>` : ''}${esc(a.label)}</button>`).join('')}
            </div>`
          : '';
        return `<div class="glass-card" data-row-index="${i}" style="padding:16px;">
          <div style="font-size:15px;font-weight:700;color:var(--text-primary);margin-bottom:10px;line-height:1.35;">${esc(row[primaryCol] || row.name || '（未命名）')}</div>
          <div style="display:flex;flex-direction:column;gap:6px;font-size:13px;">${fieldsHtml}</div>
          ${actsHtml}
        </div>`;
      }).join('')}
    </div>`;
    _paint(root, html);
  }

  const handler = (e) => {
    const btn = e.target.closest('button[data-act]');
    if (!btn) return;
    const card = btn.closest('[data-row-index]');
    if (!card) return;
    const row = rows[Number(card.dataset.idx)];
    const a = (actions(row) || []).find((x) => (x.action || 'custom') === btn.dataset.act);
    if (a?.onClick) a.onClick(row);
  };
  root.addEventListener('click', handler);

  paint();
  return { root, refresh: paint, destroy: () => { root.removeEventListener('click', handler); root.innerHTML = ''; root.__lastHtml = ''; } };
}

/* ============================================
   5. 控制列（合併舊 page-controls）
   ============================================ */
export function renderControls({ container, items = [] } = {}) {
  const root = typeof container === 'string' ? document.getElementById(container) : container;
  if (!root) return null;

  function getYearList() {
    const curY = new Date().getFullYear();
    const startY = curY - 3;
    const endY = curY + 5;
    const list = [];
    for (let y = startY; y <= endY; y++) list.push(y);
    return list;
  }

  function paint() {
    const parts = [];
    const left = items.filter((it) => it.type !== 'year' && it.type !== 'month' && it.align !== 'right');
    const right = items.filter((it) => it.align === 'right');
    const yearIt = items.find((it) => it.type === 'year');
    const monthIt = items.find((it) => it.type === 'month');

    if (left.length || right.length) {
      parts.push(`<div style="display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;margin-bottom:12px;">
        <div style="display:flex;align-items:center;gap:8px;flex-wrap:wrap;">${left.map(_renderItem).join('')}</div>
        <div style="display:flex;align-items:center;gap:8px;flex-wrap:wrap;">${right.map(_renderItem).join('')}</div>
      </div>`);
    }
    if (yearIt || monthIt) {
      const curY = AppState.year;
      const curM = AppState.month;
      const yOpts = getYearList().map((y) => `<option value="${y}" ${String(y) === String(curY) ? 'selected' : ''}>${y} 年</option>`).join('');
      let mOpts = '';
      if (monthIt?.includeAll) mOpts += `<option value="all" ${curM === 'all' ? 'selected' : ''}>全部</option>`;
      for (let m = 1; m <= 12; m++) {
        const mm = String(m).padStart(2, '0');
        mOpts += `<option value="${mm}" ${mm === String(curM) ? 'selected' : ''}>${m} 月</option>`;
      }
      parts.push(`<div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;">
        ${yearIt ? `<select class="select" data-pc-year style="width:auto;padding:8px 12px;">${yOpts}</select>` : ''}
        ${monthIt ? `<select class="select" data-pc-month style="width:auto;padding:8px 12px;">${mOpts}</select>` : ''}
      </div>`);
    }
    _paint(root, parts.length ? `<div style="margin-bottom:20px;">${parts.join('')}</div>` : '');
    bind();
  }

  function _renderItem(it) {
    if (it.type === 'add') return `<button type="button" class="btn btn-primary" data-pc-add><i data-lucide="plus"></i> ${esc(it.label || '新增')}</button>`;
    if (it.type === 'export') return `<button type="button" class="btn btn-magenta" data-pc-export><i data-lucide="file-spreadsheet"></i> ${esc(it.label || '匯出')}</button>`;
    if (it.type === 'viewToggle') {
      return `<div data-pc-view-toggle data-key="${esc(it.storageKey || '')}">
        <button type="button" class="btn btn-sm btn-primary" data-view="card">卡片</button>
        <button type="button" class="btn btn-sm btn-ghost" data-view="table">表格</button>
      </div>`;
    }
    if (it.type === 'custom') return it.render ? it.render() : (it.html || '');
    if (it.type === 'filter') return `<div data-pc-filter>${it.render ? it.render() : ''}</div>`;
    return '';
  }

  function bind() {
    const h = (e) => {
      const addBtn = e.target.closest('[data-pc-add]');
      if (addBtn) { const it = items.find((x) => x.type === 'add'); it?.onClick?.(); return; }
      const expBtn = e.target.closest('[data-pc-export]');
      if (expBtn) { const it = items.find((x) => x.type === 'export'); it?.onClick?.(); return; }
      const vBtn = e.target.closest('[data-pc-view-toggle] button[data-view]');
      if (vBtn) {
        const g = vBtn.closest('[data-pc-view-toggle]');
        g.querySelectorAll('button').forEach((b) => { b.classList.toggle('btn-primary', b === vBtn); b.classList.toggle('btn-ghost', b !== vBtn); });
        const it = items.find((x) => x.type === 'viewToggle');
        const v = vBtn.dataset.view;
        if (it?.storageKey) try { localStorage.setItem(`fin_ui_view_${it.storageKey}`, v); } catch (er) {}
        it?.onChange?.(v);
      }
    };
    const ch = (e) => {
      const y = e.target.closest('[data-pc-year]');
      if (y) { AppState.setYearMonth(y.value, AppState.month); return; }
      const m = e.target.closest('[data-pc-month]');
      if (m) { AppState.setYearMonth(AppState.year, m.value); return; }
    };
    root.addEventListener('click', h);
    root.addEventListener('change', ch);
    root.__pcH = h; root.__pcCh = ch;
  }

  paint();
  return { root, refresh: paint, destroy: () => {
    if (root.__pcH) root.removeEventListener('click', root.__pcH);
    if (root.__pcCh) root.removeEventListener('change', root.__pcCh);
    root.innerHTML = ''; root.__lastHtml = '';
  } };
}

/* ============================================
   6. 實體列表頁骨架（合併舊 entity-list-page）
   ============================================ */
export function initEntityListPage({ entity, containerId, columns, resolvers, actions, listenPath, sortFn = null, title = '' }) {
  const root = document.getElementById(containerId);
  if (!root) return null;

  let rows = [];
  let view = 'table';
  let unsub = null;
  let _table = null, _cards = null;

  root.innerHTML = `
    <div style="display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:12px;margin-bottom:16px;">
      <div style="font-size:13px;color:var(--text-muted);">${esc(title || '')}</div>
      <div style="display:flex;gap:8px;flex-wrap:wrap;">
        <div id="${containerId}-view"></div>
        <button type="button" class="btn btn-primary" id="${containerId}-add"><i data-lucide="plus"></i> 新增</button>
      </div>
    </div>
    <div id="${containerId}-content"></div>`;

  const vt = initViewToggle({
    containerId: `${containerId}-view`,
    storageKey: `${entity}-view`,
    defaultView: 'table',
    cardText: '卡片', tableText: '表格',
    onChange: (v) => { view = v; paint(); },
  });
  view = vt?.getView() || 'table';

  document.getElementById(`${containerId}-add`)?.addEventListener('click', () => actions?.onAdd?.());

  function paint() {
    const content = document.getElementById(`${containerId}-content`);
    if (!content) return;
    _table?.destroy?.(); _cards?.destroy?.();
    _table = null; _cards = null;

    const opts = { container: content, rows, columns, resolvers, actions: (row) => actions?.row ? actions.row(row) : [] };
    if (view === 'card') _cards = renderCards(opts);
    else _table = renderTable(opts);
  }

  if (listenPath) {
    unsub = listen(listenPath, (snap) => {
      const v = snap.val() || {};
      rows = Object.entries(v).map(([id, x]) => ({ id, ...x }));
      if (typeof sortFn === 'function') rows.sort(sortFn);
      paint();
    });
  }

  if (window.lucide) window.lucide.createIcons();

  return { refresh: paint, getRows: () => rows, destroy: () => { unsub?.(); _table?.destroy?.(); _cards?.destroy?.(); root.innerHTML = ''; } };
}```

<!-- === FILE END: ./js/app-widgets.js === -->

<!-- === FILE START: ./manifest-app.json === -->

## 📄 ./manifest-app.json

```json
{
  "name": "家庭財務與固定供款管理",
  "short_name": "FAMILY.FIN",
  "description": "全家財務、保險供款、基金資產、銀行交易即時管理",
  "start_url": "./app.html",
  "scope": "./",
  "display": "standalone",
  "orientation": "portrait",
  "background_color": "#E2E8F0",
  "theme_color": "#E2E8F0",
  "lang": "zh-Hant",
  "categories": ["finance", "productivity"],
  "icons": [
    {
      "src": "./icon.svg",
      "sizes": "any",
      "type": "image/svg+xml",
      "purpose": "any maskable"
    }
  ]
}```

<!-- === FILE END: ./manifest-app.json === -->

<!-- === FILE START: ./meta.json === -->

## 📄 ./meta.json

```json
{
  "domain": "W",
  "pkg_no": "90",
  "code": "webapp",
  "display": "家庭 webapp"
}```

<!-- === FILE END: ./meta.json === -->

<!-- === FILE START: ./sw-app.js === -->

## 📄 ./sw-app.js

```js
// ============================================
// sw-app.js — SPA 專用 Service Worker
// 策略：network-first，核心預快取；不干擾舊 sw.js
// ============================================
const CACHE_NAME = 'family-fin-spa-v1';

const CORE_ASSETS = [
  './app.html',
  './app.css',
  './js/app-core.js',
  './manifest-app.json',
  './icon.svg',
];

self.addEventListener('install', (e) => {
  console.log('[SW-app] install', CACHE_NAME);
  e.waitUntil(
    caches.open(CACHE_NAME).then((cache) =>
      Promise.all(CORE_ASSETS.map((u) => cache.add(u).catch((err) => console.warn('[SW-app] 快取失敗：', u, err.message))))
    ).then(() => self.skipWaiting())
  );
});

self.addEventListener('activate', (e) => {
  console.log('[SW-app] activate');
  e.waitUntil(
    caches.keys().then((keys) =>
      Promise.all(keys.filter((k) => k.startsWith('family-fin-spa-') && k !== CACHE_NAME).map((k) => caches.delete(k)))
    ).then(() => self.clients.claim())
  );
});

self.addEventListener('fetch', (e) => {
  if (e.request.method !== 'GET') return;
  const url = new URL(e.request.url);
  if (url.origin !== self.location.origin) return;
  if (url.pathname.startsWith('/api/')) return;
  e.respondWith(networkFirst(e.request));
});

async function networkFirst(request) {
  const url = new URL(request.url);
  const isHtml = request.mode === 'navigate' || url.pathname.endsWith('.html') || url.pathname === '/';
  try {
    const res = await fetch(request);
    if (res && res.status === 200 && res.type === 'basic') {
      const cache = await caches.open(CACHE_NAME);
      cache.put(request, res.clone());
    }
    return res;
  } catch (err) {
    const cached = await caches.match(request);
    if (cached) return cached;
    if (isHtml) {
      const fallback = await caches.match('./app.html');
      if (fallback) return fallback;
    }
    return new Response('離線中，請稍後再試', {
      status: 503,
      headers: { 'Content-Type': 'text/plain; charset=utf-8' },
    });
  }
}```

<!-- === FILE END: ./sw-app.js === -->
