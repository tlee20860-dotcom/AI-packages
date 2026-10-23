> 生成時間：2026-10-23 09:47:09 (HKT)

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
