> 生成時間：2026-10-23 22:50:47 (HKT)

# 專案全程式碼包：AI-WEB-APP-Family


<!-- === FILE START: ./app.css === -->

## 📄 ./app.css

```css
/* ============================================
   app.css — 智能機械風格 (Intelligent Mecha)
   ============================================ */

:root {
  --bg-deep: #0B0F19;
  --bg-grid: rgba(0, 240, 255, 0.03);
  --bg-panel: #111827;
  --bg-panel-hover: #1F2937;
  
  --primary: #00F0FF;
  --primary-glow: rgba(0, 240, 255, 0.4);
  --accent: #FF9900;
  
  --text-main: #E2E8F0;
  --text-muted: #94A3B8;
  --text-dark: #0F172A;
  
  --border: #1E293B;
  --border-glow: rgba(0, 240, 255, 0.3);
  
  --font-mono: "JetBrains Mono", "Consolas", monospace;
  --font-sans: -apple-system, "Segoe UI", "Microsoft JhengHei", sans-serif;
  
  --radius: 2px; /* 機械風格：極小圓角 */
  --chamfer: 12px; /* 切角大小 */
  
  --shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  background-color: var(--bg-deep);
  background-image: 
    linear-gradient(var(--bg-grid) 1px, transparent 1px),
    linear-gradient(90deg, var(--bg-grid) 1px, transparent 1px);
  background-size: 30px 30px;
  color: var(--text-main);
  font-family: var(--font-sans);
  font-size: 14px;
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}

/* 掃描線效果 */
body::after {
  content: " ";
  display: block;
  position: fixed;
  top: 0; left: 0; bottom: 0; right: 0;
  background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
  z-index: 9999;
  background-size: 100% 2px, 3px 100%;
  pointer-events: none;
  opacity: 0.4;
}

/* ================= 版面配置 ================= */
.app-shell { display: flex; min-height: 100vh; }
.sidebar {
  width: 240px;
  background: var(--bg-panel);
  border-right: 1px solid var(--border);
  display: flex; flex-direction: column;
}
.main-area { flex: 1; display: flex; flex-direction: column; min-width: 0; }
.navbar {
  height: 60px; display: flex; align-items: center; padding: 0 20px;
  background: rgba(17, 24, 39, 0.9);
  border-bottom: 1px solid var(--border);
  backdrop-filter: blur(10px);
}
.content { flex: 1; padding: 20px; max-width: 1400px; margin: 0 auto; width: 100%; }

/* ================= 機械切角卡片 ================= */
.mecha-panel {
  background: var(--bg-panel);
  border: 1px solid var(--border);
  padding: 1px; /* 邊框粗細 */
  clip-path: polygon(
    var(--chamfer) 0, 100% 0, 
    100% calc(100% - var(--chamfer)), calc(100% - var(--chamfer)) 100%, 
    0 100%, 0 var(--chamfer)
  );
  position: relative;
  box-shadow: var(--shadow);
  transition: all 0.2s;
}
.mecha-panel:hover {
  border-color: var(--border-glow);
  box-shadow: 0 0 15px var(--primary-glow);
}
.mecha-content {
  background: var(--bg-panel);
  padding: 20px;
  height: 100%;
  clip-path: inherit;
}
.mecha-title {
  font-family: var(--font-mono);
  font-size: 12px; color: var(--primary);
  text-transform: uppercase; letter-spacing: 2px;
  margin-bottom: 12px; border-bottom: 1px dashed var(--border);
  padding-bottom: 8px; display: flex; justify-content: space-between;
}

/* ================= 按鈕與輸入 ================= */
.btn {
  font-family: var(--font-mono); font-size: 13px;
  padding: 8px 16px; cursor: pointer; border: none;
  display: inline-flex; align-items: center; gap: 8px;
  transition: all 0.2s;
  clip-path: polygon(8px 0, 100% 0, 100% calc(100% - 8px), calc(100% - 8px) 100%, 0 100%, 0 8px);
}
.btn-primary { background: var(--primary); color: var(--text-dark); font-weight: bold; }
.btn-primary:hover { background: #00D0E0; box-shadow: 0 0 10px var(--primary-glow); }
.btn-ghost { background: transparent; border: 1px solid var(--border); color: var(--text-main); }
.btn-ghost:hover { border-color: var(--primary); color: var(--primary); }
.btn-danger { background: var(--accent); color: var(--text-dark); font-weight: bold; }

.input, .select {
  width: 100%; background: rgba(0,0,0,0.3);
  border: 1px solid var(--border); color: var(--text-main);
  padding: 10px; font-family: var(--font-mono); font-size: 13px;
  outline: none; transition: border 0.2s;
}
.input:focus, .select:focus { border-color: var(--primary); box-shadow: 0 0 5px var(--primary-glow); }
.field { margin-bottom: 16px; }
.field-label { display: block; font-family: var(--font-mono); color: var(--text-muted); font-size: 11px; margin-bottom: 6px; text-transform: uppercase; }
.field-label::before { content: "> "; color: var(--primary); }

/* ================= 表格 ================= */
.data-table { width: 100%; border-collapse: collapse; font-family: var(--font-mono); font-size: 13px; }
.data-table th {
  text-align: left; padding: 12px; color: var(--primary);
  border-bottom: 2px solid var(--primary-glow); text-transform: uppercase; letter-spacing: 1px;
}
.data-table td { padding: 12px; border-bottom: 1px solid var(--border); }
.data-table tr:hover td { background: rgba(0, 240, 255, 0.05); }
.data-table .num { text-align: right; }

/* ================= 徽章與狀態 ================= */
.badge {
  display: inline-block; padding: 2px 8px; font-family: var(--font-mono); font-size: 11px;
  border: 1px solid currentColor; clip-path: polygon(4px 0, 100% 0, 100% calc(100% - 4px), calc(100% - 4px) 100%, 0 100%, 0 4px);
}
.badge-success { color: #10B981; background: rgba(16, 185, 129, 0.1); }
.badge-pending { color: var(--accent); background: rgba(255, 153, 0, 0.1); }
.badge-info { color: var(--primary); background: rgba(0, 240, 255, 0.1); }
.badge-muted { color: var(--text-muted); background: rgba(148, 163, 184, 0.1); }

/* ================= 側邊欄 ================= */
.sidebar-logo { font-family: var(--font-mono); font-size: 18px; font-weight: bold; color: var(--primary); padding: 20px; text-shadow: 0 0 10px var(--primary-glow); }
.nav-group-title { font-family: var(--font-mono); font-size: 11px; color: var(--text-muted); padding: 10px 20px; cursor: pointer; text-transform: uppercase; border-left: 2px solid transparent; }
.nav-group-title:hover { color: var(--primary); border-left-color: var(--primary); }
.nav-item { display: block; padding: 10px 20px 10px 30px; color: var(--text-main); text-decoration: none; font-size: 13px; border-left: 2px solid transparent; transition: all 0.2s; }
.nav-item:hover { background: rgba(0, 240, 255, 0.05); color: var(--primary); }
.nav-item.active { border-left-color: var(--primary); background: rgba(0, 240, 255, 0.1); color: var(--primary); }

/* ================= 對話框 ================= */
.modal-overlay { position: fixed; inset: 0; background: rgba(11, 15, 25, 0.8); backdrop-filter: blur(4px); display: none; align-items: center; justify-content: center; z-index: 1000; }
.modal-overlay.active { display: flex; }
.modal { width: 100%; max-width: 500px; background: var(--bg-panel); border: 1px solid var(--primary); box-shadow: 0 0 20px rgba(0,240,255,0.2); clip-path: polygon(15px 0, 100% 0, 100% calc(100% - 15px), calc(100% - 15px) 100%, 0 100%, 0 15px); padding: 24px; }
.modal-title { font-family: var(--font-mono); color: var(--primary); font-size: 16px; margin-bottom: 16px; text-transform: uppercase; border-bottom: 1px solid var(--border); padding-bottom: 10px; }
.modal-actions { display: flex; justify-content: flex-end; gap: 10px; margin-top: 20px; }

/* ================= Toast ================= */
#app-toast-container { position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%); z-index: 9999; display: flex; flex-direction: column; align-items: center; }
.app-toast { background: var(--bg-panel); border-left: 3px solid var(--primary); color: var(--primary); padding: 12px 24px; font-family: var(--font-mono); font-size: 13px; box-shadow: 0 4px 12px rgba(0,0,0,0.5); margin-top: 8px; animation: slideUp 0.3s; }
@keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }

/* ================= 雜項 ================= */
.text-emerald { color: #10B981; }
.text-red { color: #EF4444; }
.text-cyan { color: var(--primary); }
.text-muted { color: var(--text-muted); }
.mono { font-family: var(--font-mono); }
.hidden { display: none !important; }

/* ================= 響應式 ================= */
@media (max-width: 768px) {
  .sidebar { position: fixed; left: -240px; height: 100vh; z-index: 1001; transition: left 0.3s; }
  .sidebar.mobile-open { left: 0; }
  .content { padding: 12px; }
  .data-table th, .data-table td { padding: 8px; font-size: 12px; }
}

/* ═══ EOF ═══
F: app.css
V: 1792800000
W: webapp-20261023-001#20
═══ */```

<!-- === FILE END: ./app.css === -->

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

<!-- === FILE START: ./index.html === -->

## 📄 ./index.html

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FAMILY.FIN | 智能機械系統</title>
  <link rel="preconnect" href="https://www.gstatic.com" crossorigin>
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="stylesheet" href="./app.css">
  <link rel="manifest" href="./manifest-app.json">
  <meta name="theme-color" content="#0B0F19">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-title" content="FAMILY.FIN">
</head>
<body>
  <!-- 登入畫面 -->
  <div id="auth-view" class="auth-page" style="display:none; align-items:center; justify-content:center; min-height:100vh;">
    <div class="mecha-panel" style="width:100%; max-width:400px;">
      <div class="mecha-content" style="padding:32px;">
        <h1 style="font-family:var(--font-mono); color:var(--primary); text-align:center; margin-bottom:8px; font-size:24px; letter-spacing:2px; text-shadow:0 0 10px var(--primary-glow);">[ FAMILY.FIN ]</h1>
        <p style="text-align:center; color:var(--text-muted); font-size:12px; margin-bottom:24px;">智能機械系統終端</p>
        <form id="login-form">
          <div class="field">
            <label class="field-label" for="account">終端帳號</label>
            <input class="input" type="text" id="account" required placeholder="例如：superadmin" autocomplete="username">
          </div>
          <div class="field">
            <label class="field-label" for="password">存取密碼</label>
            <input class="input" type="password" id="password" required minlength="6" placeholder="請輸入密碼" autocomplete="current-password">
          </div>
          <div id="login-error" class="badge badge-magenta" style="display:none; width:100%; margin-bottom:16px; padding:8px;"></div>
          <button class="btn btn-primary" type="submit" id="login-btn" style="width:100%; justify-content:center; padding:12px;">連線登入</button>
        </form>
      </div>
    </div>
  </div>

  <!-- 載入畫面 -->
  <div id="loading-view" style="display:none; min-height:100vh; align-items:center; justify-content:center; flex-direction:column;">
    <div class="mono" style="color:var(--primary); font-size:18px; animation:pulse 1s infinite;">[ 系統連線中... ]</div>
    <div id="loading-message" style="color:var(--text-muted); font-size:12px; margin-top:12px;">驗證憑證</div>
  </div>

  <!-- 主程式畫面 -->
  <div id="app-view" class="app-shell" style="display:none;">
    <aside id="sidebar-root" class="sidebar">
      <nav class="sidebar-nav" id="sidebar-nav"></nav>
    </aside>
    <div class="main-area">
      <header id="navbar-root" class="navbar"></header>
      <main class="content content-min-height" id="page-content"></main>
    </div>
  </div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>
  <script type="module" src="./js/app-core.js"></script>
  
  <style>
    @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }
  </style>
</body>
</html>```

<!-- === FILE END: ./index.html === -->

<!-- === FILE START: ./js/app-core.js === -->

## 📄 ./js/app-core.js

```js
// ============================================
// app-core.js — 狀態路由與核心啟動
// ============================================

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
import { getAuth, signInWithEmailAndPassword, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
import { render } from './app-ui.js';

export const FIREBASE_CONFIGS = {
  old: { apiKey: "AIzaSyCQlrNdorKJI9xsqr4m4ME046lrubo9Y7I", authDomain: "family-fin-a6dd1.firebaseapp.com", databaseURL: "https://family-fin-a6dd1-default-rtdb.asia-southeast1.firebasedatabase.app", projectId: "family-fin-a6dd1", storageBucket: "family-fin-a6dd1.firebasestorage.app", messagingSenderId: "382147296734", appId: "1:382147296734:web:42bdbd7401001e75e1f14f" },
  new: { apiKey: "AIzaSyBJCK83RQyHbol4yJ4UDo3pOUXpUgcW9hU", authDomain: "family-3d0a5.firebaseapp.com", databaseURL: "https://family-3d0a5-default-rtdb.asia-southeast1.firebasedatabase.app", projectId: "family-3d0a5", storageBucket: "family-3d0a5.firebasestorage.app", messagingSenderId: "109775445681", appId: "1:109775445681:web:84e974db35951d0a6ecc90" },
};

const DB_TARGET = localStorage.getItem('fin_db_target') || 'new';
export const firebaseApp = initializeApp(FIREBASE_CONFIGS[DB_TARGET]);
export const auth = getAuth(firebaseApp);
export const SUPERADMIN_EMAIL = 'superadmin@familyfin.local';

export const AppState = {
  currentUser: null, isSuperAdmin: false, currentFamilyId: '', currentFamilyName: '',
  role: '', canInput: false, displayName: '', memberAccount: null, currentMemberId: '',
  year: new Date().getFullYear().toString(), month: 'all', currentView: 'table',
  _listeners: {},
  init() {
    this.year = localStorage.getItem('fin_year') || this.year;
    this.month = localStorage.getItem('fin_month') || this.month;
    this.currentFamilyId = localStorage.getItem('fin_family_id') || '';
    this.currentFamilyName = localStorage.getItem('fin_family_name') || '';
  },
  setUser(u) { this.currentUser = u; this.emit('user-change'); },
  setFamily(id, name) { this.currentFamilyId = id; this.currentFamilyName = name; localStorage.setItem('fin_family_id', id); localStorage.setItem('fin_family_name', name); this.emit('family-change'); },
  setRole(r) { this.role = r; this.emit('role-change'); },
  setCanInput(v) { this.canInput = v; this.emit('permission-change'); },
  setMemberAccount(a) { this.memberAccount = a; if (a) { this.role = a.role || this.role; this.canInput = a.canInput !== false; this.displayName = a.displayName || ''; this.currentMemberId = a.memberId || ''; } this.emit('account-change'); },
  setYearMonth(y, m) { this.year = String(y); this.month = String(m); localStorage.setItem('fin_year', y); localStorage.setItem('fin_month', m); this.emit('ym-change'); },
  getYearMonth() { return { year: this.year, month: this.month }; },
  on(evt, cb) { if (!this._listeners[evt]) this._listeners[evt] = []; this._listeners[evt].push(cb); return () => { this._listeners[evt] = this._listeners[evt].filter(f => f !== cb); }; },
  emit(evt) { (this._listeners[evt] || []).forEach(cb => cb()); },
};

export const ROUTES = {
  '/dashboard': '總覽儀表板', '/input-center': '綜合輸入中心', '/code-list': '編號對照清單',
  '/insurance': '保險清單表', '/portfolio': '基金投資表', '/settlements': '結算清單',
  '/finance': '銀行交易', '/annual-report': '年度報表', '/member-report': '成員收支明細',
  '/settings': '系統設定', '/admin': '平台管理'
};

const _pageLoaders = {};
let _currentPageUnsub = null;

export function registerPage(route, mountFn) { _pageLoaders[route] = mountFn; }

export function navigate(route) {
  if (!route) route = '#/dashboard';
  const path = route.replace('#', '');
  
  // 清理舊頁面
  if (_currentPageUnsub) { _currentPageUnsub(); _currentPageUnsub = null; }
  
  // 更新側邊欄 active
  document.querySelectorAll('.nav-item').forEach(el => {
    el.classList.toggle('active', el.getAttribute('href') === '#' + path);
  });
  
  // 更新標題
  const titleEl = document.getElementById('navbar-title');
  if (titleEl) titleEl.textContent = ROUTES[path] || '';

  // 權限檢查
  if (path === '/admin' && !AppState.isSuperAdmin) {
    render('page-content', '<div class="mecha-panel"><div class="mecha-content text-red">無權限存取</div></div>');
    return;
  }

  const mount = _pageLoaders[path];
  if (typeof mount === 'function') {
    _currentPageUnsub = mount(document.getElementById('page-content'));
  } else {
    render('page-content', `<div class="mecha-panel"><div class="mecha-content">載入失敗：${path}</div></div>`);
  }
}

export async function login(account, password) {
  const email = account.includes('@') ? account : `${account}@familyfin.local`;
  const cred = await signInWithEmailAndPassword(auth, email, password);
  return cred.user;
}

export async function logout() {
  await signOut(auth);
  localStorage.removeItem('fin_family_id');
  localStorage.removeItem('fin_family_name');
  location.reload();
}

export function isSuperAdmin(user) { return user && user.email === SUPERADMIN_EMAIL; }

// 渲染側邊欄
function renderSidebar() {
  const navEl = document.getElementById('sidebar-nav');
  if (!navEl) return;
  const isSuper = AppState.isSuperAdmin;
  
  const groups = isSuper ? [
    { label: '平台', items: [ { path: '/admin', label: '家庭管理' }, { path: '/settings', label: '系統設定' } ] }
  ] : [
    { label: '記錄', items: [ { path: '/dashboard', label: '儀表板' }, { path: '/input-center', label: '輸入中心' }, { path: '/code-list', label: '編號清單' } ] },
    { label: '保險', items: [ { path: '/insurance', label: '保險清單' }, { path: '/portfolio', label: '基金投資' } ] },
    { label: '對帳', items: [ { path: '/settlements', label: '結算清單' }, { path: '/finance', label: '銀行交易' } ] },
    { label: '報表', items: [ { path: '/annual-report', label: '年度報表' }, { path: '/member-report', label: '成員明細' } ] },
    { label: '系統', items: [ { path: '/settings', label: '系統設定' } ] }
  ];

  let html = `<div class="sidebar-logo">[ FAMILY.FIN ]</div>`;
  groups.forEach(g => {
    html += `<div class="nav-group-title">${g.label}</div>`;
    g.items.forEach(it => {
      html += `<a class="nav-item" href="#${it.path}">${it.label}</a>`;
    });
  });
  render(navEl, html);
}

// 渲染導覽列
function renderNavbar() {
  const navEl = document.getElementById('navbar-root');
  if (!navEl) return;
  const user = AppState.currentUser;
  const role = AppState.role === 'superadmin' ? '管理員' : AppState.role;
  const html = `
    <button class="btn btn-ghost" id="hamburger-btn" style="display:none; padding:4px 8px;">≡</button>
    <div id="navbar-title" style="flex:1; font-family:var(--font-mono); font-size:16px; color:var(--primary);"></div>
    <div style="font-family:var(--font-mono); font-size:12px; color:var(--text-muted);">
      [ USER: ${user?.email?.split('@')[0] || '---'} | ROLE: ${role} ]
    </div>
    <button class="btn btn-ghost" onclick="import('./app-core.js').then(m=>m.logout())" style="margin-left:16px;">登出</button>
  `;
  render(navEl, html);
  
  // 手機版選單邏輯
  const ham = document.getElementById('hamburger-btn');
  if (window.innerWidth < 768) ham.style.display = 'block';
  ham?.addEventListener('click', () => {
    document.querySelector('.sidebar').classList.toggle('mobile-open');
  });
}

// 啟動
function init() {
  AppState.init();
  
  document.getElementById('login-form')?.addEventListener('submit', async (e) => {
    e.preventDefault();
    const btn = e.target.querySelector('button');
    btn.disabled = true; btn.textContent = '登入中...';
    try {
      await login(document.getElementById('account').value, document.getElementById('password').value);
    } catch (err) {
      const errEl = document.getElementById('login-error');
      errEl.textContent = '登入失敗：帳號或密碼錯誤';
      errEl.style.display = 'block';
      btn.disabled = false; btn.textContent = '登入';
    }
  });

  onAuthStateChanged(auth, async (user) => {
    const views = ['auth-view', 'loading-view', 'app-view'];
    if (user) {
      views.forEach(v => document.getElementById(v).style.display = 'none');
      document.getElementById('loading-view').style.display = 'flex';
      
      AppState.setUser(user);
      const isSuper = isSuperAdmin(user);
      
      if (isSuper) {
        AppState.setFamily('', '系統平台');
        AppState.setRole('superadmin');
        AppState.setCanInput(true);
        AppState.isSuperAdmin = true;
        bootApp();
      } else {
        // 呼叫後端取得家庭資訊
        try {
          const token = await user.getIdToken();
          const res = await fetch('/api/lookup-family', { method: 'POST', headers: { 'Authorization': `Bearer ${token}` } });
          const data = await res.json();
          if (data.ok && data.familyId) {
            AppState.setFamily(data.familyId, data.familyName);
            AppState.setMemberAccount(data.memberAccount);
            AppState.isSuperAdmin = false;
            bootApp();
          } else {
            alert('無法取得家庭資訊，請聯繫管理員');
            await logout();
          }
        } catch (err) {
          alert('網路錯誤，請重試');
          await logout();
        }
      }
    } else {
      AppState.currentUser = null;
      views.forEach(v => document.getElementById(v).style.display = 'none');
      document.getElementById('auth-view').style.display = 'flex';
    }
  });
}

async function bootApp() {
  document.getElementById('loading-view').style.display = 'none';
  document.getElementById('app-view').style.display = 'flex';
  
  renderSidebar();
  renderNavbar();
  
  // 初始化路由與載入器
  await import('./app-pages-loader.js');
  
  window.addEventListener('hashchange', () => navigate(location.hash));
  navigate(location.hash || '#/dashboard');
}

if (document.readyState === 'loading') document.addEventListener('DOMContentLoaded', init);
else init();

/* ═══ EOF ═══
F: js/app-core.js
V: 1792800001
W: webapp-20261023-001#21
═══ */```

<!-- === FILE END: ./js/app-core.js === -->

<!-- === FILE START: ./js/app-data.js === -->

## 📄 ./js/app-data.js

```js
// ============================================
// app-data.js — 資料層（API、常數、狀態、欄位定義）
// ============================================

import { AppState, auth } from './app-core.js';

/* ============================================
   1. 常數與狀態註冊
   ============================================ */
export const RESERVED_IDS = { EXTRA_INCOME: 'extra', SHARED_MEMBER: 'shared' };
export const BANK_TXN_TYPE_LABELS = { in: '入帳', out: '出帳', transfer: '內部轉帳' };
export const BANK_TXN_CATEGORY_LABELS = { contribution: '家用轉入', expense: '支出', insurance: '保險', reimbursement: '代墊報銷', manual: '手動' };

export const STATUS = { PENDING: 'pending', DONE: 'done', SKIPPED: 'skipped' };
const _IS_DONE = { pending: false, done: true, skipped: true };
const _LABELS = {
  default: { pending: '未處理', done: '已處理', skipped: '不適用' },
  personal:  { pending: '未付款', done: '已付款', skipped: '不適用' },
  insurance: { pending: '未扣款', done: '已扣款', skipped: '不適用' },
};

export function statusNormalize(raw) {
  if (!raw) return STATUS.PENDING;
  if (['pending', 'done', 'skipped'].includes(raw)) return raw;
  const map = { '未處理': 'pending', '未付款': 'pending', '未扣款': 'pending', '未還款': 'pending', '未轉入': 'pending', '已處理': 'done', '已付款': 'done', '已扣款': 'done', '已還款': 'done', '已轉入': 'done', '不適用': 'skipped' };
  return map[raw] || STATUS.PENDING;
}
export function statusIsDone(raw) { return !!_IS_DONE[statusNormalize(raw)]; }
export function statusLabel(raw, source = 'personal') {
  const code = statusNormalize(raw);
  return (_LABELS[source] || _LABELS.default)[code] || _LABELS.default[code];
}
export function statusBadgeClass(raw) {
  const code = statusNormalize(raw);
  return code === 'done' ? 'badge-success' : code === 'skipped' ? 'badge-muted' : 'badge-pending';
}

/* ============================================
   2. API Client
   ============================================ */
async function callApi(path, options = {}) {
  const user = auth.currentUser;
  const token = user ? await user.getIdToken() : '';
  const headers = { 'Content-Type': 'application/json', ...(options.headers || {}), 'Authorization': `Bearer ${token}` };

  let res;
  try {
    res = await fetch(path, { ...options, headers });
  } catch (err) {
    throw new Error(`網路錯誤：${err.message}`);
  }

  if (res.status === 401) { window.location.reload(); throw new Error('登入過期'); }
  if (!res.ok) {
    const text = await res.text().catch(() => '');
    let parsed = null; try { parsed = JSON.parse(text); } catch (e) {}
    throw new Error(parsed?.message || text || 'API 錯誤');
  }
  return res.json();
}

function _fid() { return AppState.currentFamilyId; }

export const api = {
  lookupFamily: () => callApi('/api/lookup-family', { method: 'POST' }),
  summary: (year, month) => callApi(`/api/summary?familyId=${_fid()}&year=${year}&month=${month}`),
  annualSummary: (s, e) => callApi(`/api/annual-summary?familyId=${_fid()}&startYear=${s}&endYear=${e}`),
  settlementsYear: (year) => callApi(`/api/settlements-year?familyId=${_fid()}&year=${year}`),
  
  insuranceSync: (p) => callApi('/api/insurance-sync', { method: 'POST', body: JSON.stringify({ ...p, familyId: _fid(), action: 'upsert' }) }),
  insuranceUnsync: (p) => callApi('/api/insurance-sync', { method: 'POST', body: JSON.stringify({ ...p, familyId: _fid(), action: 'delete' }) }),
  entityCreate: (p) => callApi('/api/entity-create', { method: 'POST', body: JSON.stringify({ ...p, familyId: _fid() }) }),

  // Admin
  adminListFamilies: () => callApi('/api/admin-families?action=list'),
  adminAddFamily: (uid, name, email) => callApi('/api/admin-families', { method: 'POST', body: JSON.stringify({ action: 'add', uid, name, email }) }),
  adminRemoveFamily: (uid) => callApi('/api/admin-families', { method: 'POST', body: JSON.stringify({ action: 'remove', uid }) }),
  
  // Settings
  platformDefaults: {
    list: (r) => callApi(`/api/platform-defaults?action=list&resource=${r}`),
    put: (r, id, d) => callApi('/api/platform-defaults', { method: 'POST', body: JSON.stringify({ action: 'put', resource: r, id, data: d }) }),
    remove: (r, id) => callApi('/api/platform-defaults', { method: 'POST', body: JSON.stringify({ action: 'delete', resource: r, id }) }),
  },
  familySettings: {
    get: () => callApi(`/api/family-settings?familyId=${_fid()}`),
    update: (d) => callApi('/api/family-settings', { method: 'POST', body: JSON.stringify({ familyId: _fid(), action: 'update', data: d }) }),
  },
  bankAccounts: {
    list: () => callApi(`/api/bank-accounts?familyId=${_fid()}&action=list`),
    create: (d) => callApi('/api/bank-accounts', { method: 'POST', body: JSON.stringify({ familyId: _fid(), action: 'create', data: d }) }),
    update: (id, d) => callApi('/api/bank-accounts', { method: 'POST', body: JSON.stringify({ familyId: _fid(), action: 'update', id, data: d }) }),
    remove: (id) => callApi('/api/bank-accounts', { method: 'POST', body: JSON.stringify({ familyId: _fid(), action: 'remove', id }) }),
  },
  bankTransactions: {
    listAll: () => callApi(`/api/bank-transactions?familyId=${_fid()}&action=listAll`),
    create: (bid, d) => callApi('/api/bank-transactions', { method: 'POST', body: JSON.stringify({ familyId: _fid(), bankId: bid, action: 'create', data: d }) }),
  },
  personalIncome: {
    save: (mid, y, m, a) => callApi('/api/personal-income', { method: 'POST', body: JSON.stringify({ familyId: _fid(), memberId: mid, year: y, month: m, amount: a, action: 'save' }) }),
  }
};

/* ============================================
   3. 欄位與解析定義 (DataTable/DataCard 使用)
   ============================================ */
export const COLUMNS = {
  codeList: [
    { id: 'code', label: '編號', width: 160 },
    { id: 'entityLabel', label: '類型', width: 120 },
    { id: 'name', label: '名稱' },
  ],
  settlements: [
    { id: 'sourceLabel', label: '來源', width: 100 },
    { id: 'yearMonth', label: '年月', width: 90 },
    { id: 'memberName', label: '成員', width: 100 },
    { id: 'name', label: '項目名稱' },
    { id: 'amount', label: '金額', type: 'number', width: 120 },
    { id: 'statusLabel', label: '狀態', width: 100 },
  ],
  bankTransactions: [
    { id: 'date', label: '日期', width: 110 },
    { id: 'bankName', label: '銀行', width: 100 },
    { id: 'typeLabel', label: '類型', width: 90 },
    { id: 'amount', label: '金額', type: 'number', width: 120 },
    { id: 'note', label: '備註' },
  ],
  memberReport: [
    { id: 'name', label: '成員', width: 140 },
    { id: 'income', label: '家用轉入', type: 'number' },
    { id: 'personalIncome', label: '個人收入', type: 'number' },
    { id: 'expense', label: '支出', type: 'number' },
    { id: 'insurance', label: '保險', type: 'number' },
    { id: 'net', label: '淨額', type: 'number' },
  ]
};

export function formatHKD(amount) {
  if (amount == null || isNaN(amount)) return 'HK$ 0';
  return 'HK$ ' + Math.round(Number(amount)).toLocaleString('zh-HK');
}

export const RESOLVERS = {
  codeList: {
    code: (v) => `<span class="mono text-cyan">${v || '—'}</span>`,
    entityLabel: (v) => `<span class="badge badge-info">${v}</span>`,
  },
  settlements: {
    amount: (v) => formatHKD(v),
    statusLabel: (v, r) => `<span class="badge ${statusBadgeClass(r.status)}">${v}</span>`,
    memberName: (v) => v === 'shared' ? '家庭' : v,
  },
  bankTransactions: {
    typeLabel: (v, r) => `<span class="badge ${r.type === 'in' ? 'badge-success' : 'badge-pending'}">${v}</span>`,
    amount: (v, r) => `<span class="mono ${r.type === 'in' ? 'text-emerald' : 'text-red'}">${r.type === 'in' ? '+' : '-'}${formatHKD(v)}</span>`,
  },
  memberReport: {
    income: (v) => `<span class="text-emerald">${formatHKD(v)}</span>`,
    expense: (v) => `<span class="text-red">${formatHKD(v)}</span>`,
    net: (v) => `<span class="${v >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(v)}</span>`,
  }
};

export function getColumns(key) { return COLUMNS[key] || []; }
export function getResolvers(key) { return RESOLVERS[key] || {}; }

/* ═══ EOF ═══
F: js/app-data.js
V: 1792800002
W: webapp-20261023-001#22
═══ */```

<!-- === FILE END: ./js/app-data.js === -->

<!-- === FILE START: ./js/app-domain.js === -->

## 📄 ./js/app-domain.js

```js
// ============================================
// app-domain.js — 業務邏輯計算層（純函數）
// ============================================

export function calcBankBalance(bankAccounts, transactions, targetYear, targetMonth) {
  if (!targetYear || !targetMonth) return 0;
  const tY = Number(targetYear), tM = Number(targetMonth);
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
      else balance -= amount;
    });
    total += balance;
  });
  return total;
}

export function getPeriodInfo(policy, year, month) {
  const firstY = Number(policy.firstStartYear) || 0;
  const firstM = Number(policy.firstStartMonth) || 1;
  if (!firstY || firstY < 2000 || firstY > 2100) return null;
  
  const startDate = new Date(firstY, firstM - 1, 1);
  const currentDate = new Date(Number(year), Number(month) - 1, 1);
  const diffMonths = (currentDate.getFullYear() - startDate.getFullYear()) * 12 + (currentDate.getMonth() - startDate.getMonth());
  if (diffMonths < 0) return null;

  const periodIndex = Math.floor(diffMonths / 12) + 1;
  const totalYears = Number(policy.totalPolicyYears) || 0;
  if (totalYears > 0 && periodIndex > totalYears) return null;
  return { periodIndex };
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
  
  const p = (policy.periods || {})[String(periodIndex)];
  if (p && p.annualPremium) return Math.round(Number(p.annualPremium));
  
  return Math.round(Number(policy.annualPremium) || 0);
}

export function getPolicyTotalPremium(policy) {
  if (!policy) return 0;
  if (policy.type === 'fund_insurance') {
    return Math.round((Number(policy.monthlyPremium) || 0) * 12 * (Number(policy.totalPolicyYears) || 0));
  }
  const totalYears = Number(policy.totalPolicyYears) || 0;
  if (totalYears === 0) return Math.round(Number(policy.annualPremium) || 0);

  let total = 0;
  for (let i = 1; i <= totalYears; i++) {
    const p = (policy.periods || {})[String(i)];
    total += (p && p.annualPremium) ? Math.round(Number(p.annualPremium)) : Math.round(Number(policy.annualPremium) || 0);
  }
  return total;
}

export function getPolicyPaidTotal(payments) {
  let total = 0;
  Object.values(payments || {}).forEach((yd) => {
    Object.values(yd || {}).forEach((m) => {
      if (m && m.status === 'done') total += Math.round(Number(m.amount) || 0);
    });
  });
  return total;
}

export function countCompletedPeriods(payments) {
  let c = 0;
  Object.values(payments || {}).forEach((yd) => {
    Object.values(yd || {}).forEach((m) => { if (m && m.status === 'done') c++; });
  });
  return c;
}

export function calcProgress(policy) {
  const t = Number(policy.totalPolicyPeriods) || 0;
  const d = Number(policy.completedPeriods) || 0;
  if (t <= 0) return 0;
  return Math.min(100, Math.round((d / t) * 100));
}

export function isPolicyCompleted(policy) {
  if (!policy) return false;
  if (policy.isCompleted) return true;
  if (policy.type === 'fund_insurance') return false;
  const t = Number(policy.totalPolicyPeriods) || 0;
  return t > 0 && Number(policy.completedPeriods) >= t;
}

export function calcFundCostEstimate(policy, year, month) {
  if (!policy || policy.type !== 'fund_insurance') return 0;
  const monthly = Number(policy.monthlyPremium) || 0;
  if (monthly <= 0) return 0;
  
  const fy = Number(policy.firstStartYear) || 0;
  const fm = Number(policy.firstStartMonth) || 1;
  if (!fy) return 0;

  const diffMonths = (Number(year) - fy) * 12 + (Number(month) - fm);
  if (diffMonths < 0) return 0;
  return Math.round(monthly * (diffMonths + 1));
}

export function getFundCostMap(policy, allFunds, targetYear, targetMonth) {
  const costMap = {};
  if (!policy) return costMap;
  const policyFunds = (allFunds || []).filter((f) => f.type === 'insurance' && (f.policyId === policy.id || (policy.fundsAllocation || []).some((a) => a.fundId === f.id)));
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

export function mergeSettlementData({ memberExpenses = [], insuranceRows = [], year, month }) {
  const rows = [];
  (memberExpenses || []).filter((e) => !e.isAutoLinked).forEach((e) => {
    rows.push({
      key: `personal-${e.id}`, source: 'personal', sourceLabel: '個人',
      year, month, memberId: e.memberId, memberName: e.memberId === 'shared' ? '家庭' : e.memberId,
      name: e.name, amount: Number(e.amount) || 0, status: e.status || 'pending',
      statusLabel: e.status === 'done' ? '已付款' : '未付款',
      _ref: { memberId: e.memberId, expenseId: e.id }
    });
  });

  const linkedMap = {};
  (memberExpenses || []).filter((e) => e.isAutoLinked).forEach((e) => {
    const pid = e.policyId || (e.id.startsWith('linked_') ? e.id.slice(7) : '');
    if (pid) linkedMap[pid] = e;
  });

  const allPolicyIds = new Set([...Object.keys(linkedMap), ...(insuranceRows || []).map((p) => p.policyId)]);
  allPolicyIds.forEach((pid) => {
    const linked = linkedMap[pid];
    const insRow = (insuranceRows || []).find((p) => p.policyId === pid);
    const effMid = insRow?.policyHolderId || insRow?.memberId || linked?.memberId || '';
    const status = insRow?.status === 'done' || linked?.status === 'done' ? 'done' : 'pending';
    
    rows.push({
      key: `insurance-${pid}`, source: 'insurance', sourceLabel: '保險',
      year, month, memberId: effMid, memberName: effMid,
      name: insRow?.policyName || linked?.name || '未知保單',
      amount: Number(linked?.amount || insRow?.amount) || 0, status,
      statusLabel: status === 'done' ? '已扣款' : '未扣款',
      _ref: { policyId: pid, memberId: effMid, linkedId: linked?.id }
    });
  });
  return rows;
}

export function computeYearTotals(allIncome, allExpenses, year) {
  const yk = String(year || '');
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
  return { totalIncome: Math.round(totalIncome), totalExpense: Math.round(totalExpense), net: Math.round(totalIncome - totalExpense), monthCount };
}

/* ═══ EOF ═══
F: js/app-domain.js
V: 1792800003
W: webapp-20261023-001#23
═══ */```

<!-- === FILE END: ./js/app-domain.js === -->

<!-- === FILE START: ./js/app-entities.js === -->

## 📄 ./js/app-entities.js

```js
// ============================================
// app-entities.js — 9 種實體定義 SSOT
// 職責：fields / ui / validate / fromForm / toForm / codePrefix / openEntityModal / handlePlusClick
// 版本：1.0.1（FIX-C：補齊 handlePlusClick 與簡單實體 prompt）
// ============================================

import { api, generateCodeFor, resolveName, getList, getOnce } from './app-data.js';
import { esc, formatHKD, formatNumber, buildForm, openModal, closeModal, toast, openPrompt } from './app-ui.js';

const LIMITS = { NAME_MAX: 60, ORDER_MIN: 0 };
const PAYMENT_TYPES = { ANNUAL: '年繳', MONTHLY: '月繳', ONCE: '一次付款' };
const POLICY_SUBTYPES = { normal: 'NORM', fund_insurance: 'FUND', savings: 'SAVE' };
const SIMPLE_ENTITIES = ['member', 'bank', 'category', 'payment', 'company'];

function _stripCommon(payload) {
  const extra = {};
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

/* ============================================
   CRUD 操作
   ============================================ */
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

/* ============================================
   動態選項
   ============================================ */
export async function getDynamicOptions(source, ctx = {}) {
  if (!source) return [];
  switch (source) {
    case 'members': return getList('members').map((m) => ({ value: m.id, label: m.name }));
    case 'categories': return getList('categories').map((c) => ({ value: c.id, label: c.name }));
    case 'items': {
      const all = getList('items');
      const f = ctx.categoryId ? all.filter((i) => i.categoryId === ctx.categoryId) : all;
      return f.map((i) => ({ value: i.id, label: i.name }));
    }
    case 'payments': return getList('payments').map((p) => ({ value: p.id, label: p.name }));
    case 'companies': return getList('companies').map((c) => ({ value: c.name, label: c.name }));
    case 'memberRoles':
      return [{ value: 'husband', label: '老公 / 丈夫' }, { value: 'wife', label: '老婆 / 妻子' }, { value: 'child', label: '子女' }, { value: 'other', label: '其他' }];
    case 'policyTypes':
      return [{ value: 'normal', label: '普通類保險' }, { value: 'fund_insurance', label: '基金類保險' }, { value: 'savings', label: '儲蓄類保險' }];
    case 'insurancePaymentTypes':
      return [{ value: '年繳', label: '年繳' }, { value: '月繳', label: '月繳' }, { value: '一次付款', label: '一次付款' }];
    case 'paymentModes':
      return [{ value: 'direct', label: '直接付款' }, { value: 'advance', label: '代墊模式' }];
    case 'months': {
      const arr = [];
      for (let m = 1; m <= 12; m++) arr.push({ value: String(m).padStart(2, '0'), label: `${m} 月` });
      return arr;
    }
    default: return [];
  }
}

/* ============================================
   🆕 FIX-C：handlePlusClick（下拉旁 + 號快速新增）
   ============================================ */
export async function handlePlusClick(entityType, context = {}) {
  if (SIMPLE_ENTITIES.includes(entityType)) {
    const def = ENTITIES[entityType];
    const label = def?.label || '項目';
    const name = await openPrompt({ title: `新增${label}`, label: `${label}名稱`, placeholder: `請輸入${label}名稱`, maxlength: 30 });
    if (!name) return null;

    try {
      const result = await createEntity(entityType, { name, ...context }, []);
      toast(`✅ 已新增${label}`, 'success');
      return result; /* { id, code } */
    } catch (err) {
      toast('新增失敗：' + (err.message || err), 'error');
      return null;
    }
  }

  /* 複雜實體：開啟 Modal，並以 Promise 等待結果 */
  return new Promise((resolve) => {
    openEntityModal(entityType, 'add', {
      initialData: Object.keys(context).length > 0 ? context : null,
      allRows: [],
      onSuccess: (result) => resolve(result),
      onCancel: () => resolve(null),
    });
  });
}

/* ============================================
   實體 Modal（完整版）
   ============================================ */
export async function openEntityModal(entityKey, mode = 'add', options = {}) {
  const { id, allRows = [], onSuccess, onCancel } = options;
  const def = ENTITIES[entityKey];
  if (!def) { toast(`未知的實體：${entityKey}`, 'error'); return; }

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
    if (row) initialData = def.toForm ? def.toForm(row) : { ...row };
  }

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
V: 1792720002
W: webapp-20261023-001#7
═══ */```

<!-- === FILE END: ./js/app-entities.js === -->

<!-- === FILE START: ./js/app-pages-core.js === -->

## 📄 ./js/app-pages-core.js

```js
// ============================================
// app-pages-core.js — 6 個日常頁
// 職責：dashboard / input-center / code-list / insurance / portfolio / settlements
// 版本：1.0.2（補齊 Dashboard / Insurance / Settlements 完整實作）
// ============================================

import { AppState, registerPage } from './app-core.js';
import { toast, openConfirm, buildForm, openModal, closeModal, initTabPanel, esc, formatHKD, openPrompt } from './app-ui.js';
import {
  api, listen, listenList, getList, resolveName, statusLabel, statusBadgeClass,
  statusIsDone, statusNormalize, saveFundSnapshot, removeFundSnapshot,
  getFundSnapshotListOnce, getColumns, getResolvers, getOnce, setSubpath, pushItem, removeItem
} from './app-data.js';
import { renderStats, renderTable, renderControls } from './app-widgets.js';
import { openEntityModal, ENTITIES, removeEntity, getDynamicOptions } from './app-entities.js';
import {
  calcBankBalance, computeYearTotals, computeAnnualOverview, getPolicyAnnualPremium,
  getPolicyTotalPremium, getPolicyPaidTotal, countCompletedPeriods, calcProgress,
  isPolicyCompleted, calcFundCostEstimate, getFundCostMap, findPolicyByFund,
  mergeSettlementData, computeMemberAnnualRows, resolveMonthlyAmount
} from './app-domain.js';

/* ============================================
   1. Dashboard
   ============================================ */
function pageDashboard(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">總覽儀表板</h1><p class="page-subtitle"></p></div></div>
    <div id="dash-stats"></div>
    <div id="dash-ctrl"></div>
    <div id="dash-annual"></div>`;
  let _stats = null, _ctrl = null;
  let _allIncome = {}, _allExpenses = {}, _accounts = [], _policies = [];

  const flattenTxns = () => {
    const list = [];
    _accounts.forEach((acc) => {
      Object.entries(acc.transactions || {}).forEach(([id, t]) => list.push({ id, bankId: acc.id, ...t }));
    });
    return list;
  };

  const render = () => {
    const year = AppState.year;
    const month = AppState.month === 'all' ? '12' : AppState.month;
    const bankTotal = calcBankBalance(_accounts, flattenTxns(), year, month);
    const yt = computeYearTotals(_allIncome, _allExpenses, year);

    _stats?.destroy?.();
    _stats = renderStats({ container: 'dash-stats', columns: 4, cards: [
      { title: '家庭總餘額', value: formatHKD(bankTotal), valueClass: bankTotal >= 0 ? 'text-emerald' : 'text-red', hint: `共 ${_accounts.length} 個帳號`, icon: 'landmark' },
      { title: `${year} 家庭收入`, value: formatHKD(yt.totalIncome), valueClass: 'text-emerald', icon: 'trending-up' },
      { title: `${year} 年度總支出`, value: formatHKD(yt.totalExpense), valueClass: 'text-red', hint: `${yt.monthCount} 個月有紀錄`, icon: 'trending-down' },
      { title: `${year} 年度淨餘額`, value: formatHKD(yt.net), valueClass: yt.net >= 0 ? 'text-emerald' : 'text-red', icon: 'wallet' },
    ] });
    paintAnnual();
  };

  const paintAnnual = () => {
    const el = document.getElementById('dash-annual');
    if (!el) return;
    const rows = computeAnnualOverview(_allIncome, _allExpenses, _policies, AppState.year);
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

  const unsubs = [
    listen('income', (snap) => { _allIncome = snap.val() || {}; render(); }),
    listen('expenses', (snap) => { _allExpenses = snap.val() || {}; render(); }),
    listen('bank_accounts', (snap) => { _accounts = Object.entries(snap.val() || {}).map(([id, x]) => ({ id, ...x })); render(); }),
    listen('insurance_policies', (snap) => { _policies = Object.entries(snap.val() || {}).map(([id, x]) => ({ id, ...x })); render(); }),
    AppState.on('ym-change', render)
  ];

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
    renderTable({
      container: el, rows: _expenseRows,
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
    
    el.innerHTML = `
      <div class="flex flex-between mb-16">
        <div class="text-muted" style="font-size:13px;">${key === 'insurance' ? '保單管理' : key === 'fund' ? '基金管理' : '銀行管理'}</div>
        <button type="button" class="btn btn-primary" id="ic-${key}-add-btn"><i data-lucide="plus"></i> 新增</button>
      </div>
      <div id="ic-${key}-list"></div>
    `;
    const entityType = key === 'insurance' ? 'policy' : key === 'fund' ? 'fund' : 'bank';
    document.getElementById(`ic-${key}-add-btn`)?.addEventListener('click', () => openEntityModal(entityType, 'add', { onSuccess: () => loadTab(key) }));

    const listPath = entityType === 'policy' ? 'insurance_policies' : entityType === 'fund' ? 'funds' : 'banks';
    const unSub = listen(listPath, (snap) => {
      const rows = Object.entries(snap.val() || {}).map(([id, x]) => ({ id, ...x }));
      const cols = entityType === 'policy' ? [
        { id: 'name', label: '保單名稱' }, { id: 'memberId', label: '受保人', width: 100 }, { id: 'company', label: '保險公司', width: 100 }
      ] : entityType === 'fund' ? [
        { id: 'name', label: '基金名稱' }, { id: 'cost', label: '投入成本', type: 'number' }, { id: 'currentValue', label: '現值', type: 'number' }
      ] : [
        { id: 'name', label: '銀行名稱' }
      ];
      renderTable({
        container: `ic-${key}-list`, rows, columns: cols,
        resolvers: {
          memberId: (v) => resolveName('members', v) || v || '—',
          cost: (v) => formatHKD(v), currentValue: (v) => formatHKD(v)
        },
        actions: (row) => [
          { label: '編輯', icon: 'pencil', className: 'btn-ghost', action: 'edit', onClick: () => openEntityModal(entityType, 'edit', { id: row.id, allRows: rows, onSuccess: () => loadTab(key) }) },
          { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
            const ok = await openConfirm(`確定刪除「${row.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
            if (!ok) return;
            try { await removeEntity(entityType, row.id); toast('✅ 已刪除', 'success'); } catch (e) { toast(e.message, 'error'); }
          } },
        ]
      });
    });
    unsubs.push(unSub);
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

/* --- 輔助 Modal --- */
async function openExpenseModal() {
  const members = await getDynamicOptions('members');
  const cats = await getDynamicOptions('categories');
  const pays = await getDynamicOptions('payments');
  members.push({ value: 'shared', label: '🏠 家庭共用' });
  
  const o = document.createElement('div');
  o.className = 'modal-overlay active';
  o.innerHTML = `<div class="modal" style="max-width:520px;"><h2 class="modal-title">新增支出</h2><div id="__exp_form"></div></div>`;
  document.body.appendChild(o);
  
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
      await pushItem(`expenses/${y}/${m}/member_expenses/${d.memberId}`, {
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
      const snap = await getOnce(`income/${y}/${m}`) || {};
      snap[d.memberId] = Number(d.amount) || 0;
      await setSubpath(`income/${y}/${m}`, snap);
      toast('✅ 已新增家用轉入', 'success');
      closeModal(o); o.remove();
    },
    onCancel: () => { closeModal(o); o.remove(); },
  });
  openModal(o);
}

async function openPersonalIncomeModal() {
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
  let _dataMap = {};

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
      _dataMap[t.key] = list;
      list.forEach((r) => {
        const has = !!r.code;
        if (_code === 'with' && !has) return;
        if (_code === 'without' && has) return;
        rows.push({ entity: t.key, entityLabel: t.label, id: r.id, code: r.code || '', name: r.name || '（未命名）' });
      });
    });
    rows.sort((a, b) => (a.code || 'zzz').localeCompare(b.code || 'zzz'));

    renderTable({
      container: 'cl-list', rows,
      columns: getColumns('codeList'),
      resolvers: getResolvers('codeList'),
      actions: AppState.getCanInput() ? (row) => [
        { label: '編輯', icon: 'pencil', className: 'btn-ghost', action: 'edit', onClick: () => {
          openEntityModal(row.entity, 'edit', { id: row.id, allRows: _dataMap[row.entity] });
        }},
        { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
          const ok = await openConfirm(`確定要刪除「${row.name}」嗎？`, { okText: '刪除', okClass: 'btn-danger' });
          if (!ok) return;
          try { await removeEntity(row.entity, row.id); toast('✅ 已刪除', 'success'); } catch (e) { toast(e.message, 'error'); }
        }},
      ] : () => [],
    });
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
    <div id="ins-content"></div>`;
  
  let _policies = [], _payments = {}, _stats = null, _ctrl = null;
  let _view = 'card';

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

    _paintSection(enriched.filter((p) => p._isCompleted));
    _paintActive(enriched.filter((p) => !p._isCompleted), enriched.length);
  };

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
    const el = document.getElementById('ins-content');
    if (!el) return;
    if (total === 0) {
      el.innerHTML = '<div class="glass-card"><div class="empty-state">尚無保單資料</div></div>';
      return;
    }

    if (active.length === 0) {
      el.innerHTML = '<div class="glass-card" style="text-align:center;padding:40px 20px;"><p style="color:var(--neon-emerald);">🎉 所有保單均已供滿</p></div>';
      return;
    }

    if (_view === 'table') {
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
      
      const elList = document.createElement('div');
      elList.id = 'ins-list-table';
      el.innerHTML = '';
      el.appendChild(elList);

      renderTable({ container: elList, rows: active, columns: cols, resolvers,
        actions: AppState.getCanInput() ? (row) => [
          { label: '編輯', icon: 'pencil', className: 'btn-ghost', action: 'edit', onClick: () => openEntityModal('policy', 'edit', { id: row.id, allRows: _policies }) },
          { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
            const ok = await openConfirm(`確定刪除保單「${row.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
            if (!ok) return;
            try { await removeEntity('policy', row.id); toast('✅ 已刪除', 'success'); } catch (e) { toast(e.message, 'error'); }
          } },
        ] : () => [],
      });
    } else {
      el.innerHTML = `<div class="grid grid-3">${active.map((p) => `<div class="glass-card policy-card">${_cardHtml(p)}</div>`).join('')}</div>`;
    }
  };

  const _cardHtml = (p) => {
    const holderId = p.policyHolderId || p.memberId || '';
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
        <div class="policy-info-item"><span class="policy-info-label">本期年繳</span><span class="policy-info-value text-cyan">${formatHKD(p._curAnnual)}</span></div>
        <div class="policy-info-item"><span class="policy-info-label">保單總供款</span><span class="policy-info-value text-magenta">${formatHKD(totalPremium)}</span></div>
      </div>
      <div class="policy-info-grid" style="margin-top:10px;padding-top:10px;border-top:1px dashed rgba(255,255,255,0.08);">
        <div class="policy-info-item"><span class="policy-info-label">已供款總額</span><span class="policy-info-value text-emerald" style="font-weight:700;">${formatHKD(paidTotal)}</span></div>
        <div class="policy-info-item"><span class="policy-info-label">剩餘供款</span><span class="policy-info-value">${formatHKD(remaining)}</span></div>
      </div>${progressHtml}${actions}`;
  };

  const _clickHandler = async (e) => {
    const btn = e.target.closest('button[data-action]');
    if (!btn || !AppState.getCanInput()) return;
    const policy = _policies.find((p) => p.id === btn.dataset.id);
    if (!policy) return;
    if (btn.dataset.action === 'edit-policy') openEntityModal('policy', 'edit', { id: policy.id, allRows: _policies });
    else if (btn.dataset.action === 'delete-policy') {
      const ok = await openConfirm(`確定刪除保單「${policy.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
      if (!ok) return;
      try { await removeEntity('policy', policy.id); toast('✅ 已刪除', 'success'); } catch (e) { toast(e.message, 'error'); }
    }
  };
  document.getElementById('ins-content')?.addEventListener('click', _clickHandler);
  document.getElementById('completed-grid')?.addEventListener('click', _clickHandler);

  const headerEl = document.getElementById('completed-header');
  const bodyEl = document.getElementById('completed-body');
  const sectionEl = document.getElementById('completed-section');
  const _toggleHandler = () => {
    if (!bodyEl || !sectionEl) return;
    const open = bodyEl.style.display === 'block';
    bodyEl.style.display = open ? 'none' : 'block';
    sectionEl.classList.toggle('open', !open);
  };
  if (headerEl) headerEl.addEventListener('click', _toggleHandler);

  _ctrl = renderControls({ container: 'ins-ctrl', items: [
    { type: 'add', label: '新增保單', onClick: () => openEntityModal('policy', 'add') },
    { type: 'viewToggle', storageKey: 'insurance-view', defaultView: 'card', onChange: (v) => { _view = v; paint(); } },
    { type: 'year' }, { type: 'month' },
  ] });

  const unsubs = [
    listen('insurance_policies', (snap) => {
      _policies = Object.entries(snap.val() || {}).map(([id, x]) => ({ id, ...x }));
      paint();
    }),
    listen('insurance_payments', (snap) => {
      _payments = snap.val() || {};
      paint();
    }),
  ];

  return { destroy: () => {
    unsubs.forEach((u) => u && u());
    if (_stats) _stats.destroy?.();
    if (_ctrl) _ctrl.destroy?.();
    document.getElementById('ins-content')?.removeEventListener('click', _clickHandler);
    if (headerEl) headerEl.removeEventListener('click', _toggleHandler);
  } };
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
          <div style="margin-top:12px;display:flex;gap:8px;justify-content:flex-end;">
            <button type="button" class="btn btn-sm btn-primary" data-snap-id="${f.id}" data-snap-name="${esc(f.name)}">快照</button>
            <button type="button" class="btn btn-sm btn-ghost" data-hist-id="${f.id}" data-hist-name="${esc(f.name)}">歷史</button>
          </div>
        </div>`;
      }).join('')}
    </div>`;

    el.querySelectorAll('[data-snap-id]').forEach((btn) => {
      btn.addEventListener('click', () => openSnapshotModal(btn.dataset.snapId, btn.dataset.snapName, paint));
    });
    el.querySelectorAll('[data-hist-id]').forEach((btn) => {
      btn.addEventListener('click', () => openHistoryModal(btn.dataset.histId, btn.dataset.histName, paint));
    });
  };

  const unsubs = [
    listen('funds', (snap) => { _funds = Object.entries(snap.val() || {}).map(([id, x]) => ({ id, ...x })); paint(); }),
    listen('insurance_policies', (snap) => { _policies = Object.entries(snap.val() || {}).map(([id, x]) => ({ id, ...x })); paint(); }),
  ];

  _ctrl = renderControls({ container: 'pf-ctrl', items: [
    { type: 'add', label: '新增基金', onClick: () => openEntityModal('fund', 'add') },
    { type: 'year' }, { type: 'month' },
  ] });
  return { destroy: () => { unsubs.forEach((u) => u && u()); _stats?.destroy?.(); _ctrl?.destroy?.(); } };
}

async function openSnapshotModal(fundId, fundName, onDone) {
  const MODAL_ID = 'snap-modal';
  document.getElementById(MODAL_ID)?.remove();
  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay active';
  overlay.id = MODAL_ID;
  overlay.innerHTML = `<div class="modal" style="max-width:480px;"><h2 class="modal-title">輸入快照 - ${esc(fundName)}</h2><div id="${MODAL_ID}-form"></div></div>`;
  document.body.appendChild(overlay);

  buildForm({
    containerId: `${MODAL_ID}-form`,
    fields: [
      { type: 'text', id: 'dateKey', label: '估值日（YYYY-MM-DD）', required: true, defaultValue: new Date().toISOString().slice(0, 10) },
      { type: 'number', id: 'shares', label: '單位數目', required: true, min: 0, defaultValue: 0 },
      { type: 'number', id: 'nav', label: '基金價格 (USD)', required: true, min: 0, defaultValue: 0 },
    ],
    submitText: '儲存', showCancel: true, cancelText: '取消',
    onSubmit: async (data) => {
      try {
        await saveFundSnapshot(fundId, data.dateKey, { shares: data.shares, nav: data.nav });
        toast('✅ 已儲存快照', 'success');
        closeModal(overlay); overlay.remove();
        if (onDone) onDone();
      } catch (e) { toast('儲存失敗：' + e.message, 'error'); }
    },
    onCancel: () => { closeModal(overlay); overlay.remove(); },
  });
  openModal(overlay);
}

async function openHistoryModal(fundId, fundName, onDone) {
  const snaps = await getFundSnapshotListOnce(fundId);
  const MODAL_ID = 'hist-modal';
  document.getElementById(MODAL_ID)?.remove();
  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay active';
  overlay.id = MODAL_ID;
  overlay.innerHTML = `<div class="modal" style="max-width:600px;"><h2 class="modal-title">歷史快照 - ${esc(fundName)}</h2>
    <div style="max-height:50vh;overflow-y:auto;margin-bottom:16px;">
      <table class="data-table"><thead><tr><th>日期</th><th>單位數</th><th>價格</th><th>操作</th></tr></thead>
      <tbody>
        ${snaps.map((s) => `<tr>
          <td>${esc(s.dateKey)}</td><td>${s.shares}</td><td>${s.nav}</td>
          <td><button type="button" class="btn btn-sm btn-danger" data-del-date="${s.dateKey}" data-del-raw="${s._rawPath}">刪除</button></td>
        </tr>`).join('')}
      </tbody></table>
    </div>
    <div style="text-align:right;"><button type="button" class="btn btn-ghost" id="hist-close">關閉</button></div>
  </div>`;
  document.body.appendChild(overlay);

  overlay.querySelector('#hist-close')?.addEventListener('click', () => { closeModal(overlay); overlay.remove(); });
  overlay.querySelectorAll('[data-del-date]').forEach((btn) => {
    btn.addEventListener('click', async () => {
      const ok = await openConfirm(`確定刪除 ${btn.dataset.delDate} 快照？`, { okText: '刪除', okClass: 'btn-danger' });
      if (!ok) return;
      try {
        await removeFundSnapshot(fundId, btn.dataset.delDate, btn.dataset.delRaw);
        toast('✅ 已刪除', 'success');
        closeModal(overlay); overlay.remove();
        if (onDone) onDone();
      } catch (e) { toast('刪除失敗：' + e.message, 'error'); }
    });
  });

  openModal(overlay);
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
  let _stats = null, _ctrl = null, _expUnsub = null;
  let _memberExpenses = [], _insuranceRows = [];

  const reload = async () => {
    const { year, month } = AppState.getYearMonth();
    const y = String(year);
    const m = month === 'all' ? null : month;

    if (_expUnsub) { try { _expUnsub(); } catch (e) {} }
    _expUnsub = listen(`expenses/${y}/${m || '01'}/member_expenses`, (snap) => {
      _memberExpenses = [];
      Object.entries(snap.val() || {}).forEach(([mid, items]) => {
        Object.entries(items || {}).forEach(([id, e]) => _memberExpenses.push({ id, memberId: mid, ...e }));
      });
      _recompute();
    });

    try {
      const policies = getList('insurance_policies');
      const rows = [];
      for (const p of policies) {
        const pays = await getOnce(`insurance_payments/${p.id}/${y}/${m || '01'}`) || {};
        const holder = p.policyHolderId || p.memberId || '';
        if (pays && Object.keys(pays).length) {
          rows.push({ policyId: p.id, policyName: p.name, policyHolderId: holder, memberId: p.memberId, status: pays.status || '', amount: Number(pays.amount) || 0, paymentMode: p.paymentMode || 'direct', advanceHolderId: p.advanceHolderId || '' });
        } else {
          const est = resolveMonthlyAmount(p, y, m || '01') || 0;
          rows.push({ policyId: p.id, policyName: p.name, policyHolderId: holder, memberId: p.memberId, status: '', amount: est, paymentMode: p.paymentMode || 'direct', advanceHolderId: p.advanceHolderId || '' });
        }
      }
      _insuranceRows = rows;
    } catch (e) { console.warn('[settlements] 載入保險失敗', e); }
    _recompute();
  };

  const _recompute = () => {
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
            ? [{ label: '取消扣款', icon: 'rotate-ccw', className: 'btn-ghost', action: 'unsync', onClick: () => _onUnsync(row) }]
            : [{ label: '確認扣款', icon: 'check', className: 'btn-primary', action: 'sync', onClick: () => _onSync(row) }];
        }
        return [{ label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
          const ok = await openConfirm(`確定刪除「${row.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
          if (!ok) return;
          try {
            await removeItem(`expenses/${row.year}/${row.month}/member_expenses/${row._ref.memberId}`, row._ref.expenseId);
            toast('✅ 已刪除', 'success');
          } catch (e) { toast(e.message, 'error'); }
        } }];
      } : () => [],
    });
  };

  const _onSync = async (row) => {
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
  
  const _onUnsync = async (row) => {
    const ok = await openConfirm(`確定取消「${row.name}」本月扣款？`, { okText: '取消扣款', okClass: 'btn-danger' });
    if (!ok) return;
    try {
      await api.insuranceUnsync({ policyId: row._ref.policyId, memberId: row._ref.memberId || row.memberId, year: row.year, month: row.month, amount: row.amount, paymentMode: row.paymentMode || 'direct', advanceHolderId: row.advanceHolderId || '' });
      toast('✅ 已取消扣款', 'success');
      reload();
    } catch (e) { toast('取消失敗：' + e.message, 'error'); }
  };

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
V: 1792720005
W: webapp-20261023-001#11
═══ */
```

<!-- === FILE END: ./js/app-pages-core.js === -->

<!-- === FILE START: ./js/app-pages-loader.js === -->

## 📄 ./js/app-pages-loader.js

```js
// ============================================
// app-pages-loader.js — 路由頁面註冊
// ============================================

import { registerPage } from './app-core.js';

import { mount as mountDashboard } from './pages/dashboard.js';
import { mount as mountInputCenter } from './pages/input-center.js';
import { mount as mountCodeList } from './pages/code-list.js';
import { mount as mountInsurance } from './pages/insurance.js';
import { mount as mountPortfolio } from './pages/portfolio.js';
import { mount as mountSettlements } from './pages/settlements.js';
import { mount as mountFinance } from './pages/finance.js';
import { mount as mountAnnualReport } from './pages/annual-report.js';
import { mount as mountMemberReport } from './pages/member-report.js';
import { mount as mountSettings } from './pages/settings.js';
import { mount as mountAdmin } from './pages/admin.js';

registerPage('/dashboard', mountDashboard);
registerPage('/input-center', mountInputCenter);
registerPage('/code-list', mountCodeList);
registerPage('/insurance', mountInsurance);
registerPage('/portfolio', mountPortfolio);
registerPage('/settlements', mountSettlements);
registerPage('/finance', mountFinance);
registerPage('/annual-report', mountAnnualReport);
registerPage('/member-report', mountMemberReport);
registerPage('/settings', mountSettings);
registerPage('/admin', mountAdmin);

/* ═══ EOF ═══
F: js/app-pages-loader.js
V: 1792800200
W: webapp-20261023-001#47
═══ */```

<!-- === FILE END: ./js/app-pages-loader.js === -->

<!-- === FILE START: ./js/app-pages-reports.js === -->

## 📄 ./js/app-pages-reports.js

```js
// ============================================
// app-pages-reports.js — 5 個報表／設定頁
// 職責：finance / annual-report / member-report / settings / admin
// 版本：1.0.2（FIX-F：補齊設定頁四卡、vConsole 除錯、預設資料庫）
// ============================================

import { AppState, registerPage, logout } from './app-core.js';
import { toast, openConfirm, buildForm, openModal, closeModal, initTabPanel, esc, formatHKD, openPrompt } from './app-ui.js';
import {
  api, listen, listenList, getList, resolveName, statusLabel, statusBadgeClass, statusIsDone,
  listenYearRange, saveYearRange, listenPageMetadata, savePageMetadata, getYearRange, getColumns, getResolvers
} from './app-data.js';
import { renderStats, renderTable, renderControls } from './app-widgets.js';
import { openEntityModal, removeEntity, getDynamicOptions } from './app-entities.js';
import {
  calcBankBalance, computeYearTotals, computeMemberReportRows,
  computeMemberAnnualRows, computeAnnualOverview,
  getPolicyAnnualPremium, getPolicyTotalPremium, getPolicyPaidTotal,
  countCompletedPeriods, calcProgress, isPolicyCompleted,
  calcFundCostEstimate, getFundCostMap, findPolicyByFund,
} from './app-domain.js';

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
   4. Settings（GAP-10, 18, 23）
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
  let _tab = null, _bankUnsub = null, _memberUnsub = null, _yrUnsub = null, _metaUnsub = null;
  let _personalRendered = false;

  const loadBanks = () => {
    const el = document.getElementById('st-panel-banks');
    if (!el) return;
    el.innerHTML = `
      <div class="flex flex-between mb-16">
        <div class="text-muted" style="font-size:13px;">共 ${getList('bank_accounts').length} 個帳號</div>
        <button type="button" class="btn btn-primary" id="st-bank-add"><i data-lucide="plus"></i> 新增銀行帳號</button>
      </div>
      <div id="st-bank-list"></div>`;
      
    document.getElementById('st-bank-add')?.addEventListener('click', () => openEntityModal('bankAccount', 'add', { onSuccess: loadBanks }));
    
    renderTable({
      container: 'st-bank-list', rows: getList('bank_accounts'),
      columns: [
        { id: 'name', label: '名稱', type: 'text' },
        { id: 'type', label: '類型', width: 100 },
        { id: 'initialBalance', label: '初始餘額', type: 'number', width: 130 },
      ],
      resolvers: {
        type: (v) => v === 'personal' ? '👤 個人' : '🏠 家庭',
        initialBalance: (v) => `<span class="mono">${formatHKD(v)}</span>`,
      },
      actions: (row) => [
        { label: '編輯', icon: 'pencil', className: 'btn-ghost', action: 'edit', onClick: () => openEntityModal('bankAccount', 'edit', { id: row.id, allRows: getList('bank_accounts'), onSuccess: loadBanks }) },
        { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
          const ok = await openConfirm(`確定刪除「${row.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
          if (!ok) return;
          try { await removeEntity('bankAccount', row.id); toast('✅ 已刪除', 'success'); } catch (e) { toast(e.message, 'error'); }
        }},
      ],
    });
  };

  const loadPersonal = () => {
    const el = document.getElementById('st-panel-personal');
    if (!el) return;
    if (_personalRendered) return;
    _personalRendered = true;

    const acc = AppState.getMemberAccount() || {};
    const savedMode = localStorage.getItem(`fin_${AppState.getFamilyId()}_ui_stats_mode`) || 'auto';

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

      <div class="glass-card" style="grid-column:1/-1;">
        <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:12px;">
          <i data-lucide="calendar" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
          <span>年份範圍</span>
        </div>
        <div id="st-yr-root"></div>
      </div>

      <div class="glass-card" style="grid-column:1/-1;">
        <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:12px;">
          <i data-lucide="type" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
          <span>頁面標題</span>
        </div>
        <div class="field" style="margin-bottom:12px;">
          <label class="field-label">選擇頁面</label>
          <select class="select" id="meta-page" style="max-width:400px;">
            <option value="dashboard">總覽儀表板</option>
            <option value="portfolio">基金投資表</option>
            <option value="settings">系統設定</option>
          </select>
        </div>
        <div class="field" style="margin-bottom:12px;">
          <label class="field-label">標題</label>
          <input class="input" type="text" id="meta-title" maxlength="40" style="max-width:400px;">
        </div>
        <button type="button" class="btn btn-primary" id="meta-save">儲存</button>
      </div>

      <div class="glass-card" style="grid-column:1/-1;">
        <div class="glass-card-title" style="display:flex; align-items:center; gap:8px; margin-bottom:12px;">
          <i data-lucide="bug" style="width:16px;height:16px;color:var(--neon-cyan);"></i>
          <span>除錯工具（開發者用）</span>
        </div>
        <p class="glass-card-hint mb-12">手機若無 DevTools，可開啟 vConsole 檢視日誌。開啟後會跨頁保持。</p>
        <div id="settings-debug-status" style="margin-bottom:12px; font-size:13px;"><span class="badge badge-muted">● 已關閉</span></div>
        <div style="display:flex; gap:8px; flex-wrap:wrap;">
          <button type="button" class="btn btn-primary" id="settings-debug-enable"><i data-lucide="play" style="width:14px;height:14px;"></i> 開啟 vConsole</button>
        </div>
      </div>
    </div>`;

    document.getElementById('st-logout')?.addEventListener('click', async () => {
      const ok = await openConfirm('確定要登出嗎？', { title: '登出', okText: '登出', okClass: 'btn-danger' });
      if (ok) logout();
    });

    document.getElementById('save-stats-mode')?.addEventListener('click', () => {
      const selected = document.querySelector('input[name="stats-mode"]:checked');
      if (selected) {
        localStorage.setItem(`fin_${AppState.getFamilyId()}_ui_stats_mode`, selected.value);
        toast('✅ 已儲存顯示模式', 'success');
      }
    });

    renderTable({ container: 'st-members', rows: getList('members'),
      columns: [{ id: 'name', label: '名稱', type: 'text' }, { id: 'role', label: '角色', width: 120 }],
      resolvers: { role: (v) => esc(v || '—') },
    });

    // 年份範圍
    const yrRoot = document.getElementById('st-yr-root');
    _yrUnsub = listenYearRange((d) => {
      const curRange = d || { startYear: null, futureYears: 5 };
      yrRoot.innerHTML = `
        <div class="grid grid-2">
          <div class="field"><label class="field-label">起始年份</label><input class="input" type="number" id="yr-start" value="${curRange.startYear || ''}" placeholder="留空預設"></div>
          <div class="field"><label class="field-label">往後顯示年數</label><input class="input" type="number" id="yr-future" value="${curRange.futureYears}"></div>
        </div>
        <div style="text-align:right; margin-top:8px;"><button type="button" class="btn btn-primary" id="yr-save">儲存年份</button></div>
      `;
      document.getElementById('yr-save')?.addEventListener('click', async () => {
        const s = document.getElementById('yr-start').value;
        const f = document.getElementById('yr-future').value;
        try {
          await saveYearRange({ startYear: s ? Number(s) : null, futureYears: Number(f) });
          toast('✅ 年份範圍已儲存', 'success');
        } catch (e) { toast('儲存失敗：' + e.message, 'error'); }
      });
    });

    // 頁面標題
    const metaPage = document.getElementById('meta-page');
    const metaTitle = document.getElementById('meta-title');
    let allMeta = {};
    _metaUnsub = listenPageMetadata((all) => {
      allMeta = all || {};
      metaTitle.value = allMeta[metaPage.value]?.title || '';
    });
    metaPage?.addEventListener('change', () => { metaTitle.value = allMeta[metaPage.value]?.title || ''; });
    document.getElementById('meta-save')?.addEventListener('click', async () => {
      const page = metaPage.value;
      const title = metaTitle.value.trim();
      if (!title) return toast('標題不可為空', 'warning');
      try {
        await savePageMetadata({ ...allMeta, [page]: { title } });
        toast('✅ 標題已儲存', 'success');
      } catch (e) { toast('儲存失敗：' + e.message, 'error'); }
    });

    // 除錯工具（vConsole）
    document.getElementById('settings-debug-enable')?.addEventListener('click', async () => {
      const btn = document.getElementById('settings-debug-enable');
      btn.disabled = true; btn.textContent = '載入中…';
      try {
        const script = document.createElement('script');
        script.src = 'https://cdn.jsdelivr.net/npm/vconsole@3.15.0/dist/vconsole.min.js';
        script.onload = () => {
          window.vConsole = new window.VConsole({ maxLogNumber: 2000 });
          document.getElementById('settings-debug-status').innerHTML = '<span class="badge badge-success">● 已開啟</span>';
          btn.style.display = 'none';
          toast('✅ vConsole 已開啟', 'success');
        };
        script.onerror = () => { toast('❌ 載入失敗，請檢查網路', 'error'); btn.disabled = false; btn.textContent = '開啟 vConsole'; };
        document.head.appendChild(script);
      } catch (e) { toast('❌ 載入失敗', 'error'); btn.disabled = false; btn.textContent = '開啟 vConsole'; }
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

  return { destroy: () => { 
    _tab?.destroy?.(); 
    if (_bankUnsub) _bankUnsub(); 
    if (_memberUnsub) _memberUnsub(); 
    if (_yrUnsub) _yrUnsub();
    if (_metaUnsub) _metaUnsub();
    _personalRendered = false;
  } };
}

/* ============================================
   5. Admin（GAP-24）
   ============================================ */
function pageAdmin(root) {
  root.innerHTML = `
    <div class="page-header"><div><h1 class="page-title">平台管理</h1><p class="page-subtitle"></p></div>
      <div><button type="button" class="btn btn-danger" id="ad-logout"><i data-lucide="log-out"></i> 登出</button></div></div>
    <div class="banner">ℹ️ 管理所有家庭與平台預設資料庫</div>
    <div id="ad-tabs"></div>
    <div id="ad-panel-families" class="tab-panel"></div>
    <div id="ad-panel-defaults" class="tab-panel"></div>`;

  let _tab = null;

  const loadFamilies = async () => {
    const el = document.getElementById('ad-panel-families');
    if (!el) return;
    try {
      const r = await api.adminListFamilies();
      const families = r.families || [];
      renderTable({
        container: el, rows: families,
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
        actions: () => [
          { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async (row) => {
            const ok = await openConfirm(`確定刪除家庭「${row.name || row.uid}」？`, { okText: '刪除', okClass: 'btn-danger' });
            if (!ok) return;
            try { await api.adminRemoveFamily(row.uid); toast('✅ 已刪除', 'success'); loadFamilies(); } catch (e) { toast(e.message, 'error'); }
          }},
        ],
      });
    } catch (e) { toast('載入失敗：' + e.message, 'error'); }
  };

  const loadDefaults = () => {
    const el = document.getElementById('ad-panel-defaults');
    if (!el) return;
    
    el.innerHTML = `
      <div class="banner">ℹ️ 此處的預設資料將套用於「新家庭初始化時」，修改後不影響已存在的家庭。</div>
      <div id="ad-defaults-tabs"></div>
      <div id="ad-defaults-list"></div>
    `;
    
    const dTab = initTabPanel({
      containerId: 'ad-defaults-tabs',
      tabs: [
        { key: 'members', label: '成員', icon: 'users' },
        { key: 'companies', label: '保險公司', icon: 'shield' },
        { key: 'categories', label: '支出類別', icon: 'tags' },
      ],
      defaultKey: 'members',
      onChange: async (k) => {
        const listEl = document.getElementById('ad-defaults-list');
        listEl.innerHTML = '<div class="empty-state">載入中…</div>';
        try {
          const res = await api.platformDefaults.list(k);
          renderTable({
            container: listEl,
            rows: res.list || [],
            columns: [
              { id: 'name', label: '名稱' },
              { id: 'order', label: '排序', width: 80 },
            ],
            actions: (row) => [
              { label: '刪除', icon: 'trash-2', className: 'btn-danger', action: 'del', onClick: async () => {
                const ok = await openConfirm(`確定刪除「${row.name}」？`, { okText: '刪除', okClass: 'btn-danger' });
                if (!ok) return;
                try { await api.platformDefaults.remove(k, row.id); toast('✅ 已刪除', 'success'); dTab.switchTo(k); } catch (e) { toast(e.message, 'error'); }
              }},
            ],
          });
        } catch (e) { listEl.innerHTML = `<div class="text-red">載入失敗：${e.message}</div>`; }
      }
    });
  };

  _tab = initTabPanel({
    containerId: 'ad-tabs',
    tabs: [
      { key: 'families', label: '家庭列表', icon: 'home', panelId: 'ad-panel-families' },
      { key: 'defaults', label: '預設資料庫', icon: 'database', panelId: 'ad-panel-defaults' },
    ],
    defaultKey: 'families',
    onChange: (k) => { if (k === 'families') loadFamilies(); else loadDefaults(); },
  });
  loadFamilies();

  document.getElementById('ad-logout')?.addEventListener('click', async () => {
    const ok = await openConfirm('確定要登出嗎？', { title: '登出', okText: '登出', okClass: 'btn-danger' });
    if (ok) logout();
  });

  return { destroy: () => { _tab?.destroy?.(); root.innerHTML = ''; } };
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
V: 1792720501
W: webapp-20261023-001#18
═══ */```

<!-- === FILE END: ./js/app-pages-reports.js === -->

<!-- === FILE START: ./js/app-ui.js === -->

## 📄 ./js/app-ui.js

```js
// ============================================
// app-ui.js — UI 工具與純函數渲染
// ============================================

/* ============================================
   1. 基礎工具
   ============================================ */
export function esc(v) {
  if (v == null) return '';
  const map = { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' };
  return String(v).replace(/[&<>"']/g, (c) => map[c]);
}

/**
 * 核心渲染函數：防抖比對 + innerHTML 覆蓋
 */
export function render(container, html) {
  const el = typeof container === 'string' ? document.getElementById(container) : container;
  if (!el) return;
  if (el.__lastHtml === html) return; // 防抖優化
  el.__lastHtml = html;
  el.innerHTML = html;
  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   2. Toast 提示
   ============================================ */
export function toast(msg, type = 'success', duration = 3000) {
  let container = document.getElementById('app-toast-container');
  if (!container) {
    container = document.createElement('div');
    container.id = 'app-toast-container';
    document.body.appendChild(container);
  }
  const el = document.createElement('div');
  el.className = 'app-toast';
  if (type === 'error') el.style.borderLeftColor = '#EF4444';
  el.textContent = msg;
  container.appendChild(el);
  
  setTimeout(() => {
    el.style.opacity = '0';
    setTimeout(() => el.remove(), 300);
  }, duration);
}

/* ============================================
   3. Modal / Prompt
   ============================================ */
let _modalResolve = null;

export function openModal(title, htmlContent, actionsHtml) {
  document.getElementById('__custom-modal')?.remove();
  
  return new Promise((resolve) => {
    _modalResolve = resolve;
    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay active';
    overlay.id = '__custom-modal';
    overlay.innerHTML = `
      <div class="modal">
        <div class="modal-title">${esc(title)}</div>
        <div id="__modal-body">${htmlContent}</div>
        <div class="modal-actions">
          <button type="button" class="btn btn-ghost" data-action="cancel">取消</button>
          ${actionsHtml || `<button type="button" class="btn btn-primary" data-action="ok">確定</button>`}
        </div>
      </div>
    `;
    document.body.appendChild(overlay);
    
    overlay.addEventListener('click', (e) => {
      if (e.target === overlay || e.target.closest('[data-action="cancel"]')) {
        overlay.remove();
        _modalResolve(null);
      } else if (e.target.closest('[data-action="ok"]')) {
        overlay.remove();
        _modalResolve(true);
      }
    });
  });
}

export function closeModal() {
  document.getElementById('__custom-modal')?.remove();
  if (_modalResolve) { _modalResolve(null); _modalResolve = null; }
}

/* ============================================
   4. 事件委託機制
   ============================================ */
const _eventBus = {};

export function on(event, selector, handler) {
  if (!_eventBus[event]) {
    _eventBus[event] = [];
    document.addEventListener(event, (e) => {
      _eventBus[event].forEach(({ sel, fn }) => {
        const target = e.target.closest(sel);
        if (target) fn(e, target);
      });
    });
  }
  _eventBus[event].push({ sel: selector, fn: handler });
}

/* ============================================
   5. 表單建構器 (純字串生成)
   ============================================ */
export function buildFormHtml(fields, data = {}) {
  let html = '<div class="form-stack">';
  fields.forEach(f => {
    const val = data[f.id] != null ? data[f.id] : (f.defaultValue || '');
    html += `<div class="field">`;
    html += `<label class="field-label">${esc(f.label)}${f.required ? ' *' : ''}</label>`;
    
    if (f.type === 'select') {
      html += `<select class="select" id="${f.id}">`;
      if (f.includeEmpty) html += `<option value="">— 請選擇 —</option>`;
      (f.options || []).forEach(o => {
        html += `<option value="${esc(o.value)}" ${String(o.value) === String(val) ? 'selected' : ''}>${esc(o.label)}</option>`;
      });
      html += `</select>`;
    } else if (f.type === 'textarea') {
      html += `<textarea class="input" id="${f.id}" rows="3">${esc(val)}</textarea>`;
    } else {
      html += `<input class="input" type="${f.type || 'text'}" id="${f.id}" value="${esc(val)}" ${f.required ? 'required' : ''}>`;
    }
    html += `</div>`;
  });
  html += '</div>';
  return html;
}

export function getFormData(fields) {
  const res = {};
  fields.forEach(f => {
    const el = document.getElementById(f.id);
    if (el) {
      if (f.type === 'number') res[f.id] = el.value === '' ? 0 : Number(el.value);
      else res[f.id] = el.value;
    }
  });
  return res;
}

/* ═══ EOF ═══
F: js/app-ui.js
V: 1792800004
W: webapp-20261023-001#24
═══ */```

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

<!-- === FILE START: ./js/components/data-card.js === -->

## 📄 ./js/components/data-card.js

```js
// ============================================
// data-card.js — 卡片渲染組件
// ============================================

import { esc, render } from '../app-ui.js';

export function renderCards(containerId, rows, primaryKey, fields, resolvers = {}, actions = null) {
  const html = `
    <div style="display:grid; grid-template-columns:repeat(auto-fill, minmax(280px, 1fr)); gap:16px;">
      ${rows.map((row, i) => `
        <div class="mecha-panel">
          <div class="mecha-content">
            <div style="font-size:16px; font-weight:700; color:var(--primary); margin-bottom:12px; font-family:var(--font-mono);">
              ${esc(row[primaryKey])}
            </div>
            <div style="display:flex; flex-direction:column; gap:8px; margin-bottom:16px;">
              ${fields.map(f => {
                const val = row[f.id];
                const res = resolvers[f.id];
                return `
                  <div style="display:flex; justify-content:space-between; font-size:13px;">
                    <span style="color:var(--text-muted);">${esc(f.label)}</span>
                    <span>${res ? res(val, row) : esc(val)}</span>
                  </div>
                `;
              }).join('')}
            </div>
            ${actions ? `
              <div style="display:flex; gap:8px; justify-content:flex-end; border-top:1px dashed var(--border); padding-top:12px;">
                <button class="btn btn-ghost" data-action="edit" data-idx="${i}" style="padding:4px 10px; font-size:11px;">編輯</button>
                <button class="btn btn-danger" data-action="delete" data-idx="${i}" style="padding:4px 10px; font-size:11px;">刪除</button>
              </div>
            ` : ''}
          </div>
        </div>
      `).join('')}
    </div>
  `;
  render(containerId, html);
}```

<!-- === FILE END: ./js/components/data-card.js === -->

<!-- === FILE START: ./js/components/data-table.js === -->

## 📄 ./js/components/data-table.js

```js
// ============================================
// data-table.js — 表格渲染組件
// ============================================

import { esc, render } from '../app-ui.js';

export function renderTable(containerId, rows, columns, resolvers = {}, actions = null) {
  const html = `
    <div class="mecha-panel">
      <div class="mecha-content" style="padding:0; overflow-x:auto;">
        <table class="data-table">
          <thead>
            <tr>
              ${columns.map(c => `<th style="${c.width ? `width:${c.width}px` : ''}">${esc(c.label)}</th>`).join('')}
              ${actions ? `<th style="width:120px;">操作</th>` : ''}
            </tr>
          </thead>
          <tbody>
            ${rows.map((row, i) => `
              <tr>
                ${columns.map(c => {
                  const val = row[c.id];
                  const res = resolvers[c.id];
                  const content = res ? res(val, row) : esc(val);
                  return `<td class="${c.type === 'number' ? 'num' : ''}">${content}</td>`;
                }).join('')}
                ${actions ? `
                  <td>
                    <button class="btn btn-ghost" data-action="edit" data-idx="${i}" style="padding:2px 8px; font-size:11px;">編輯</button>
                    <button class="btn btn-ghost" data-action="delete" data-idx="${i}" style="padding:2px 8px; font-size:11px; color:#EF4444; border-color:#EF4444;">刪除</button>
                  </td>
                ` : ''}
              </tr>
            `).join('')}
            ${rows.length === 0 ? `<tr><td colspan="${columns.length + (actions?1:0)}" style="text-align:center; color:var(--text-muted); padding:20px;">無資料</td></tr>` : ''}
          </tbody>
        </table>
      </div>
    </div>
  `;
  render(containerId, html);
}```

<!-- === FILE END: ./js/components/data-table.js === -->

<!-- === FILE START: ./js/components/page-controls.js === -->

## 📄 ./js/components/page-controls.js

```js
// ============================================
// page-controls.js — 年月與檢視切換控制列
// ============================================

import { render } from '../app-ui.js';
import { AppState } from '../app-core.js';

export function renderControls(containerId, options = {}) {
  const { showYear = true, showMonth = true, showViewToggle = false } = options;
  const cur = AppState.getYearMonth();
  const years = [];
  const now = new Date().getFullYear();
  for (let y = now - 3; y <= now + 5; y++) years.push(y);

  const html = `
    <div style="display:flex; gap:12px; align-items:center; margin-bottom:20px; flex-wrap:wrap; justify-content:space-between;">
      <div style="display:flex; gap:12px; align-items:center;">
        ${showYear ? `
          <select class="select" data-ctrl="year" style="width:auto;">
            ${years.map(y => `<option value="${y}" ${y==cur.year?'selected':''}>${y} 年</option>`).join('')}
          </select>
        ` : ''}
        ${showMonth ? `
          <select class="select" data-ctrl="month" style="width:auto;">
            <option value="all" ${cur.month==='all'?'selected':''}>全部月份</option>
            ${Array.from({length:12},(_,i)=>i+1).map(m => {
              const val = String(m).padStart(2,'0');
              return `<option value="${val}" ${val===cur.month?'selected':''}>${m} 月</option>`;
            }).join('')}
          </select>
        ` : ''}
      </div>
      ${showViewToggle ? `
        <div style="display:flex; gap:4px; background:var(--bg-panel); padding:4px; border:1px solid var(--border);">
          <button class="btn ${AppState.currentView==='table'?'btn-primary':'btn-ghost'}" data-ctrl="view" data-val="table" style="padding:4px 12px; font-size:12px;">表格</button>
          <button class="btn ${AppState.currentView==='card'?'btn-primary':'btn-ghost'}" data-ctrl="view" data-val="card" style="padding:4px 12px; font-size:12px;">卡片</button>
        </div>
      ` : ''}
    </div>
  `;
  render(containerId, html);
}

export function bindControls(containerId, onChange) {
  const el = document.getElementById(containerId);
  if (!el) return;
  el.addEventListener('change', (e) => {
    const ctrl = e.target.closest('[data-ctrl]');
    if (!ctrl) return;
    if (ctrl.dataset.ctrl === 'year') {
      AppState.setYearMonth(ctrl.value, AppState.month);
      onChange();
    } else if (ctrl.dataset.ctrl === 'month') {
      AppState.setYearMonth(AppState.year, ctrl.value);
      onChange();
    }
  });
  el.addEventListener('click', (e) => {
    const btn = e.target.closest('[data-ctrl="view"]');
    if (!btn) return;
    AppState.currentView = btn.dataset.val;
    onChange();
  });
}```

<!-- === FILE END: ./js/components/page-controls.js === -->

<!-- === FILE START: ./js/components/stats-cards.js === -->

## 📄 ./js/components/stats-cards.js

```js
// ============================================
// stats-cards.js — 統計卡渲染組件
// ============================================

import { esc, render } from '../app-ui.js';

export function renderStats(containerId, cards) {
  const html = `
    <div style="display:grid; grid-template-columns:repeat(auto-fit, minmax(200px, 1fr)); gap:16px; margin-bottom:24px;">
      ${cards.map(c => `
        <div class="mecha-panel">
          <div class="mecha-content" style="padding:16px;">
            <div style="font-size:11px; color:var(--text-muted); font-family:var(--font-mono); text-transform:uppercase; margin-bottom:8px;">
              ${esc(c.title)}
            </div>
            <div class="mono ${c.color || ''}" style="font-size:20px; font-weight:700;">
              ${esc(c.value)}
            </div>
            ${c.hint ? `<div style="font-size:10px; color:var(--text-muted); margin-top:4px;">${esc(c.hint)}</div>` : ''}
          </div>
        </div>
      `).join('')}
    </div>
  `;
  render(containerId, html);
}```

<!-- === FILE END: ./js/components/stats-cards.js === -->

<!-- === FILE START: ./js/entity/entity-defs.js === -->

## 📄 ./js/entity/entity-defs.js

```js
// ============================================
// entity-defs.js — 實體定義 SSOT
// ============================================

export const ENTITIES = {
  member: {
    label: '成員', icon: 'users', prefix: 'MEM',
    fields: [
      { id: 'name', label: '名稱', type: 'text', required: true },
      { id: 'role', label: '角色', type: 'select', required: true, options: [
        { value: 'husband', label: '丈夫' }, { value: 'wife', label: '妻子' },
        { value: 'child', label: '子女' }, { value: 'other', label: '其他' }
      ]},
      { id: 'order', label: '排序', type: 'number', defaultValue: 0 }
    ]
  },
  bank: {
    label: '銀行', icon: 'landmark', prefix: 'PAY-BANK',
    fields: [{ id: 'name', label: '名稱', type: 'text', required: true }]
  },
  category: {
    label: '支出類別', icon: 'tags', prefix: 'CAT',
    fields: [
      { id: 'name', label: '名稱', type: 'text', required: true },
      { id: 'order', label: '排序', type: 'number', defaultValue: 0 }
    ]
  },
  item: {
    label: '支出項目', icon: 'list', prefix: 'ITEM',
    fields: [
      { id: 'name', label: '名稱', type: 'text', required: true },
      { id: 'categoryId', label: '所屬類別', type: 'select', required: true, optionsSource: 'categories' }
    ]
  },
  payment: {
    label: '支付方式', icon: 'credit-card', prefix: 'PAY',
    fields: [{ id: 'name', label: '名稱', type: 'text', required: true }]
  },
  company: {
    label: '保險公司', icon: 'shield', prefix: 'COMP',
    fields: [{ id: 'name', label: '名稱', type: 'text', required: true }]
  },
  policy: {
    label: '保單', icon: 'shield', prefix: 'POL',
    fields: [
      { id: 'name', label: '保單名稱', type: 'text', required: true },
      { id: 'type', label: '類型', type: 'select', required: true, options: [
        { value: 'normal', label: '普通保險' }, { value: 'fund_insurance', label: '基金保險' }
      ]},
      { id: 'memberId', label: '受保人', type: 'select', required: true, optionsSource: 'members' },
      { id: 'company', label: '保險公司', type: 'text' },
      { id: 'firstStartYear', label: '開始年份', type: 'number', required: true, defaultValue: new Date().getFullYear() },
      { id: 'totalPolicyYears', label: '供款年期', type: 'number', required: true, defaultValue: 5 },
      { id: 'annualPremium', label: '年繳保費', type: 'number', defaultValue: 0 },
      { id: 'monthlyPremium', label: '月供金額', type: 'number', defaultValue: 0 }
    ]
  },
  fund: {
    label: '基金', icon: 'line-chart', prefix: 'FUND',
    fields: [
      { id: 'name', label: '基金名稱', type: 'text', required: true },
      { id: 'type', label: '類型', type: 'select', required: true, options: [
        { value: 'standalone', label: '獨立基金' }, { value: 'insurance', label: '保險基金' }
      ]},
      { id: 'cost', label: '投入成本', type: 'number', defaultValue: 0 },
      { id: 'currentValue', label: '現時價值', type: 'number', defaultValue: 0 }
    ]
  },
  bankAccount: {
    label: '銀行帳號', icon: 'landmark', prefix: 'PAY-BANK',
    fields: [
      { id: 'name', label: '帳號名稱', type: 'text', required: true },
      { id: 'type', label: '類型', type: 'select', required: true, options: [
        { value: 'family', label: '家庭' }, { value: 'personal', label: '個人' }
      ]},
      { id: 'initialBalance', label: '初始餘額', type: 'number', defaultValue: 0 },
      { id: 'initialYear', label: '初始年份', type: 'number', defaultValue: new Date().getFullYear() }
    ]
  }
};

export function getEntityDef(key) { return ENTITIES[key]; }```

<!-- === FILE END: ./js/entity/entity-defs.js === -->

<!-- === FILE START: ./js/entity/entity-helpers.js === -->

## 📄 ./js/entity/entity-helpers.js

```js
// ============================================
// entity-helpers.js — 實體 CRUD 與編號生成
// ============================================

import { api } from '../app-data.js';
import { getEntityDef } from './entity-defs.js';

export async function generateCode(entityType, list) {
  const def = getEntityDef(entityType);
  if (!def || !def.prefix) return null;
  const codes = list.map(x => x.code).filter(Boolean);
  let max = 0;
  codes.forEach(c => {
    if (c.startsWith(def.prefix + '-')) {
      const n = parseInt(c.split('-').pop(), 10);
      if (n > max) max = n;
    }
  });
  return `${def.prefix}-${String(max + 1).padStart(3, '0')}`;
}

export async function createEntity(type, data, allRows = []) {
  const code = await generateCode(type, allRows);
  const extra = { ...data };
  delete extra.name;
  return api.entityCreate({ entityType: type, action: 'create', code, name: data.name, extra });
}

export async function updateEntity(type, id, data) {
  const extra = { ...data };
  delete extra.name;
  return api.entityCreate({ entityType: type, action: 'update', id, name: data.name, extra });
}

export async function removeEntity(type, id) {
  return api.entityCreate({ entityType: type, action: 'delete', id });
}```

<!-- === FILE END: ./js/entity/entity-helpers.js === -->

<!-- === FILE START: ./js/entity/entity-modal.js === -->

## 📄 ./js/entity/entity-modal.js

```js
// ============================================
// entity-modal.js — 實體新增/編輯彈窗
// ============================================

import { openModal, closeModal, buildFormHtml, getFormData, toast, esc } from '../app-ui.js';
import { getEntityDef } from './entity-defs.js';
import { createEntity, updateEntity } from './entity-helpers.js';
import { getList } from '../app-data.js';

export async function openEntityModal(type, mode, id = null, allRows = [], onSuccess = null) {
  const def = getEntityDef(type);
  if (!def) return;

  // 準備選項
  const fields = def.fields.map(f => {
    const field = { ...f };
    if (f.optionsSource) {
      const list = getList(f.optionsSource === 'categories' ? 'expense_categories' : f.optionsSource);
      field.options = list.map(x => ({ value: x.id, label: x.name }));
    }
    return field;
  });

  let data = {};
  if (mode === 'edit' && id) {
    data = allRows.find(x => x.id === id) || {};
  }

  const html = buildFormHtml(fields, data);
  
  const ok = await openModal(`${mode === 'edit' ? '編輯' : '新增'} ${def.label}`, html);
  if (!ok) return;

  const formData = getFormData(fields);
  try {
    if (mode === 'edit') {
      await updateEntity(type, id, formData);
      toast('✅ 已更新');
    } else {
      await createEntity(type, formData, allRows);
      toast('✅ 已新增');
    }
    closeModal();
    if (onSuccess) onSuccess();
  } catch (err) {
    toast('操作失敗：' + err.message, 'error');
  }
}```

<!-- === FILE END: ./js/entity/entity-modal.js === -->

<!-- === FILE START: ./js/entity/funds-allocation.js === -->

## 📄 ./js/entity/funds-allocation.js

```js
// ============================================
// funds-allocation.js — 保單關聯基金 UI
// ============================================

import { render, esc, toast } from '../app-ui.js';
import { getList } from '../app-data.js';

let _currentAlloc = [];

export function renderFundsAllocation(containerId, initial = [], onUpdate = null) {
  _currentAlloc = initial.map(x => ({ ...x }));
  const funds = getList('funds').filter(f => f.type === 'insurance' || f.type === 'standalone');

  const paint = () => {
    let totalPct = _currentAlloc.reduce((s, a) => s + Number(a.pct), 0);
    let html = `
      <div class="mecha-panel" style="margin-top:16px;">
        <div class="mecha-content">
          <div style="display:flex; justify-content:space-between; margin-bottom:12px;">
            <div class="field-label" style="margin:0;">關聯基金分配</div>
            <div class="mono ${totalPct === 100 ? 'text-emerald' : 'text-red'}">總和: ${totalPct}%</div>
          </div>
          <div id="alloc-list">
            ${_currentAlloc.map((a, i) => {
              const fund = funds.find(f => f.id === a.fundId);
              return `
                <div style="display:flex; gap:8px; margin-bottom:8px; align-items:center;">
                  <span style="flex:1; font-size:13px;">${esc(fund?.name || a.fundId)}</span>
                  <input type="number" class="input" style="width:80px;" value="${a.pct}" onchange="window.__updateFundPct(${i}, this.value)">
                  <span style="font-size:12px;">%</span>
                  <button class="btn btn-ghost" style="color:#EF4444;" onclick="window.__removeFundAlloc(${i})">移除</button>
                </div>
              `;
            }).join('')}
          </div>
          <button class="btn btn-ghost" onclick="window.__addFundAlloc()">+ 新增基金</button>
        </div>
      </div>
    `;
    render(containerId, html);
    if (onUpdate) onUpdate(_currentAlloc);
  };

  window.__addFundAlloc = () => {
    const used = _currentAlloc.map(a => a.fundId);
    const avail = funds.filter(f => !used.includes(f.id));
    if (!avail.length) return toast('無可用基金', 'error');
    _currentAlloc.push({ fundId: avail[0].id, pct: 0 });
    paint();
  };

  window.__updateFundPct = (idx, val) => {
    _currentAlloc[idx].pct = Number(val) || 0;
    paint();
  };

  window.__removeFundAlloc = (idx) => {
    _currentAlloc.splice(idx, 1);
    paint();
  };

  paint();
}

export function getFundsAllocationValue() {
  return _currentAlloc;
}```

<!-- === FILE END: ./js/entity/funds-allocation.js === -->

<!-- === FILE START: ./js/pages/admin.js === -->

## 📄 ./js/pages/admin.js

```js
// ============================================
// admin.js — 平台管理
// ============================================

import { AppState, logout } from '../app-core.js';
import { render, esc, toast, openModal, getFormData, buildFormHtml } from '../app-ui.js';
import { api } from '../app-data.js';

export function mount(container) {
  let state = { families: [] };

  const paint = () => {
    const html = `
      <div class="page-header" style="margin-bottom:20px; display:flex; justify-content:space-between; align-items:center;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 平台管理 ]</h1>
        <button class="btn btn-danger" id="btn-logout">登出</button>
      </div>
      <div style="margin-bottom:16px;">
        <button class="btn btn-primary" id="btn-add-family">+ 新增家庭</button>
      </div>
      <div class="mecha-panel">
        <div class="mecha-content" style="padding:0;">
          <table class="data-table">
            <thead><tr><th>UID</th><th>名稱</th><th>擁有者 Email</th><th>操作</th></tr></thead>
            <tbody>
              ${state.families.map(f => `
                <tr>
                  <td><span class="mono">${esc(f.uid)}</span></td>
                  <td>${esc(f.name)}</td>
                  <td>${esc(f.ownerEmail || '—')}</td>
                  <td>
                    <button class="btn btn-ghost" onclick="window.__delFam('${f.uid}')" style="color:#EF4444;">刪除</button>
                  </td>
                </tr>
              `).join('')}
            </tbody>
          </table>
        </div>
      </div>
    `;
    render(container, html);

    document.getElementById('btn-logout').addEventListener('click', logout);
    document.getElementById('btn-add-family').addEventListener('click', async () => {
      const fields = [
        { id: 'uid', label: 'Family UID', type: 'text', required: true },
        { id: 'name', label: '名稱', type: 'text', required: true },
        { id: 'email', label: 'Email (選填)', type: 'text' }
      ];
      const ok = await openModal('新增家庭', buildFormHtml(fields));
      if (!ok) return;
      const data = getFormData(fields);
      try {
        await api.adminAddFamily(data.uid, data.name, data.email);
        toast('✅ 已新增');
        loadFamilies();
      } catch (err) { toast(err.message, 'error'); }
    });

    window.__delFam = async (uid) => {
      if (!confirm(`確定刪除家庭 ${uid}？`)) return;
      try {
        await api.adminRemoveFamily(uid);
        toast('✅ 已刪除');
        loadFamilies();
      } catch (err) { toast(err.message, 'error'); }
    };
  };

  const loadFamilies = async () => {
    try {
      const res = await api.adminListFamilies();
      state.families = res.families || [];
      paint();
    } catch (err) { toast(err.message, 'error'); }
  };

  loadFamilies();
  return () => {};
}```

<!-- === FILE END: ./js/pages/admin.js === -->

<!-- === FILE START: ./js/pages/annual-report.js === -->

## 📄 ./js/pages/annual-report.js

```js
// ============================================
// annual-report.js — 年度報表
// ============================================

import { AppState } from '../app-core.js';
import { render, esc, toast } from '../app-ui.js';
import { renderStats } from '../components/stats-cards.js';
import { renderControls } from '../components/page-controls.js';
import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { formatHKD } from '../app-data.js';
import { computeYearTotals } from '../app-domain.js';

export function mount(container) {
  const db = getDatabase();
  const fid = AppState.currentFamilyId;
  const unsubs = [];
  let state = { income: {}, expenses: {}, personalIncome: {}, members: {} };
  let view = 'summary';

  const paint = () => {
    const { year } = AppState.getYearMonth();
    const yt = computeYearTotals(state.income, state.expenses, year);
    
    let piTotal = 0;
    Object.values(state.personalIncome).forEach(md => {
      Object.values(md[year] || {}).forEach(v => { piTotal += Number(v) || 0; });
    });

    const html = `
      <div class="page-header" style="margin-bottom:20px; display:flex; justify-content:space-between; align-items:center;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 年度報表 ]</h1>
        <button class="btn btn-ghost" id="btn-export">匯出 CSV</button>
      </div>
      <div id="stats-root"></div>
      <div id="controls-root"></div>
      <div style="display:flex; gap:8px; margin-bottom:20px;">
        <button class="btn ${view==='summary'?'btn-primary':'btn-ghost'}" data-view="summary">全年總合</button>
        <button class="btn ${view==='monthly'?'btn-primary':'btn-ghost'}" data-view="monthly">月度明細</button>
      </div>
      <div id="report-content"></div>
    `;
    render(container, html);

    renderStats('stats-root', [
      { title: '家用轉入', value: formatHKD(yt.totalIncome), color: 'text-emerald' },
      { title: '個人收入', value: formatHKD(piTotal), color: 'text-cyan' },
      { title: '總支出', value: formatHKD(yt.totalExpense), color: 'text-red' },
      { title: '淨餘額', value: formatHKD(yt.totalIncome + piTotal - yt.totalExpense), color: (yt.totalIncome + piTotal - yt.totalExpense) >= 0 ? 'text-emerald' : 'text-red' }
    ]);
    renderControls('controls-root', { showViewToggle: false, showMonth: false });

    container.querySelectorAll('[data-view]').forEach(btn => {
      btn.addEventListener('click', () => { view = btn.dataset.view; paint(); });
    });

    if (view === 'summary') {
      const rows = Object.entries(state.members).map(([mid, m]) => {
        let inc = 0, pi = 0, exp = 0;
        Object.values(state.income[year] || {}).forEach(md => { inc += Number(md[mid]) || 0; });
        Object.values(state.personalIncome[mid]?.[year] || {}).forEach(v => { pi += Number(v) || 0; });
        Object.values(state.expenses[year] || {}).forEach(md => {
          Object.values(md.member_expenses?.[mid] || {}).forEach(e => { exp += Number(e.amount) || 0; });
        });
        return { name: m.name, inc, pi, exp, net: inc + pi - exp };
      });
      const total = rows.reduce((s, r) => ({ inc: s.inc + r.inc, pi: s.pi + r.pi, exp: s.exp + r.exp, net: s.net + r.net }), { inc: 0, pi: 0, exp: 0, net: 0 });
      rows.push({ name: '總計', ...total, isTotal: true });

      const htmlStr = `
        <div class="mecha-panel">
          <div class="mecha-content" style="padding:0; overflow-x:auto;">
            <table class="data-table">
              <thead><tr><th>成員</th><th class="num">家用轉入</th><th class="num">個人收入</th><th class="num">總支出</th><th class="num">淨結餘</th></tr></thead>
              <tbody>
                ${rows.map(r => `
                  <tr style="${r.isTotal ? 'font-weight:bold; color:var(--primary);' : ''}">
                    <td>${esc(r.name)}</td>
                    <td class="num text-emerald">${formatHKD(r.inc)}</td>
                    <td class="num text-cyan">${formatHKD(r.pi)}</td>
                    <td class="num text-red">${formatHKD(r.exp)}</td>
                    <td class="num ${r.net >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(r.net)}</td>
                  </tr>
                `).join('')}
              </tbody>
            </table>
          </div>
        </div>
      `;
      render('report-content', htmlStr);
    } else {
      const m = AppState.month === 'all' ? '01' : AppState.month;
      const incM = state.income[year]?.[m] || {};
      const expM = state.expenses[year]?.[m]?.member_expenses || {};
      const rows = [];
      Object.entries(incM).forEach(([mid, amt]) => rows.push({ name: `收入 - ${state.members[mid]?.name || mid}`, amt: Number(amt) }));
      Object.entries(expM).forEach(([mid, items]) => {
        Object.values(items || {}).forEach(e => rows.push({ name: `支出 - ${e.name}`, amt: -Number(e.amount) }));
      });

      const htmlStr = `
        <div class="mecha-panel">
          <div class="mecha-content" style="padding:0;">
            <table class="data-table">
              <thead><tr><th>項目</th><th class="num">金額</th></tr></thead>
              <tbody>
                ${rows.length ? rows.map(r => `<tr><td>${esc(r.name)}</td><td class="num ${r.amt >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(r.amt)}</td></tr>`).join('') : '<tr><td colspan="2" class="text-muted" style="text-align:center;">此月無紀錄</td></tr>'}
              </tbody>
            </table>
          </div>
        </div>
      `;
      render('report-content', htmlStr);
    }

    document.getElementById('btn-export')?.addEventListener('click', () => {
      const rows = [];
      Object.entries(state.members).forEach(([mid, m]) => {
        let inc = 0, pi = 0, exp = 0;
        Object.values(state.income[year] || {}).forEach(md => { inc += Number(md[mid]) || 0; });
        Object.values(state.personalIncome[mid]?.[year] || {}).forEach(v => { pi += Number(v) || 0; });
        Object.values(state.expenses[year] || {}).forEach(md => {
          Object.values(md.member_expenses?.[mid] || {}).forEach(e => { exp += Number(e.amount) || 0; });
        });
        rows.push([m.name, inc, pi, exp, inc+pi-exp].join(','));
      });
      const csv = '\uFEFF' + '成員,轉入,個人收入,支出,淨額\n' + rows.join('\n');
      const url = URL.createObjectURL(new Blob([csv], { type: 'text/csv;charset=utf-8;' }));
      const a = document.createElement('a'); a.href = url; a.download = `annual-${year}.csv`; a.click();
    });
  };

  const paths = [['income', 'income'], ['expenses', 'expenses'], ['personalIncome', 'personal_income'], ['members', 'members']];
  paths.forEach(([key, p]) => {
    unsubs.push(onValue(ref(db, `families/${fid}/${p}`), (snap) => {
      state[key] = snap.val() || {};
      paint();
    }));
  });

  unsubs.push(AppState.on('ym-change', paint));
  paint();

  return () => unsubs.forEach(u => u && u());
}```

<!-- === FILE END: ./js/pages/annual-report.js === -->

<!-- === FILE START: ./js/pages/code-list.js === -->

## 📄 ./js/pages/code-list.js

```js
// ============================================
// code-list.js — 編號對照清單
// ============================================

import { AppState } from '../app-core.js';
import { render, esc } from '../app-ui.js';
import { renderTable } from '../components/data-table.js';
import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { openEntityModal } from '../entity/entity-modal.js';
import { removeEntity } from '../entity/entity-helpers.js';
import { toast } from '../app-ui.js';

const ENTITY_TYPES = [
  { key: 'member', label: '成員' }, { key: 'bank', label: '銀行' },
  { key: 'category', label: '類別' }, { key: 'item', label: '項目' },
  { key: 'payment', label: '支付' }, { key: 'company', label: '公司' },
  { key: 'policy', label: '保單' }, { key: 'fund', label: '基金' },
  { key: 'bankAccount', label: '帳號' }
];

export function mount(container) {
  const db = getDatabase();
  const fid = AppState.currentFamilyId;
  const unsubs = [];
  let state = {};
  let filterType = 'all';

  const paint = () => {
    const rows = [];
    ENTITY_TYPES.forEach(t => {
      if (filterType !== 'all' && filterType !== t.key) return;
      Object.entries(state[t.key] || {}).forEach(([id, x]) => {
        rows.push({ entity: t.key, entityLabel: t.label, id, code: x.code || '', name: x.name || '' });
      });
    });
    rows.sort((a, b) => (a.code || 'zzz').localeCompare(b.code || 'zzz'));

    const cols = [
      { id: 'code', label: '編號', width: 160 },
      { id: 'entityLabel', label: '類型', width: 120 },
      { id: 'name', label: '名稱' }
    ];
    const resolvers = {
      code: v => `<span class="mono text-cyan">${v || '—'}</span>`,
      entityLabel: v => `<span class="badge badge-info">${v}</span>`
    };

    const html = `
      <div class="page-header" style="margin-bottom:20px;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 編號對照清單 ]</h1>
      </div>
      <div style="margin-bottom:20px;">
        <select class="select" id="filter-type" style="width:200px;">
          <option value="all">全部類型</option>
          ${ENTITY_TYPES.map(t => `<option value="${t.key}" ${filterType===t.key?'selected':''}>${t.label}</option>`).join('')}
        </select>
      </div>
      <div id="cl-table"></div>
    `;
    render(container, html);
    
    document.getElementById('filter-type').addEventListener('change', e => { filterType = e.target.value; paint(); });
    
    renderTable('cl-table', rows, cols, resolvers, () => true);
    
    // 綁定表格編輯/刪除
    document.getElementById('cl-table').addEventListener('click', async (e) => {
      const btn = e.target.closest('button');
      if (!btn) return;
      const row = rows[Number(btn.dataset.idx)];
      if (btn.dataset.action === 'edit') {
        await openEntityModal(row.entity, 'edit', row.id, Object.entries(state[row.entity] || {}).map(([id, x]) => ({ id, ...x })));
      } else if (btn.dataset.action === 'delete') {
        if (!confirm(`確定刪除 ${row.name}？`)) return;
        try { await removeEntity(row.entity, row.id); toast('✅ 已刪除'); } catch (err) { toast(err.message, 'error'); }
      }
    });
  };

  const paths = {
    member: 'members', bank: 'banks', category: 'expense_categories', item: 'expense_items',
    payment: 'payment_methods', company: 'insurance_companies', policy: 'insurance_policies',
    fund: 'funds', bankAccount: 'bank_accounts'
  };
  Object.entries(paths).forEach(([key, p]) => {
    unsubs.push(onValue(ref(db, `families/${fid}/${p}`), (snap) => {
      state[key] = snap.val() || {};
      paint();
    }));
  });

  paint();
  return () => unsubs.forEach(u => u && u());
}```

<!-- === FILE END: ./js/pages/code-list.js === -->

<!-- === FILE START: ./js/pages/dashboard.js === -->

## 📄 ./js/pages/dashboard.js

```js
// ============================================
// dashboard.js — 總覽儀表板
// ============================================

import { AppState, auth } from '../app-core.js';
import { render, esc, toast } from '../app-ui.js';
import { renderStats } from '../components/stats-cards.js';
import { renderControls } from '../components/page-controls.js';
import { calcBankBalance, computeYearTotals } from '../app-domain.js';
import { formatHKD } from '../app-data.js';
import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

export function mount(container) {
  const db = getDatabase();
  const fid = AppState.currentFamilyId;
  const unsubs = [];
  let state = { income: {}, expenses: {}, bankAccounts: {}, policies: {} };

  const paint = () => {
    const { year } = AppState.getYearMonth();
    const yt = computeYearTotals(state.income, state.expenses, year);
    
    // 計算銀行餘額
    const accounts = Object.entries(state.bankAccounts).map(([id, x]) => ({ id, ...x }));
    const txns = [];
    accounts.forEach(a => {
      Object.entries(a.transactions || {}).forEach(([tid, t]) => txns.push({ id: tid, bankId: a.id, ...t }));
    });
    const bankTotal = calcBankBalance(accounts, txns, year, '12');

    // 年度總覽
    const annualRows = [];
    const curY = Number(year);
    for (let y = curY - 2; y <= curY + 2; y++) {
      const t = computeYearTotals(state.income, state.expenses, y);
      // 簡化保險總額計算（此處省略複雜保單平攤，僅用基本收支）
      annualRows.push({ year: y, inc: t.totalIncome, exp: t.totalExpense, net: t.net });
    }

    const html = `
      <div class="page-header" style="margin-bottom:20px;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 總覽儀表板 ]</h1>
      </div>
      <div id="stats-root"></div>
      <div id="controls-root"></div>
      
      <div class="mecha-panel" style="margin-top:20px;">
        <div class="mecha-content" style="padding:0;">
          <div class="mecha-title" style="padding:16px;">年度總覽 (前後2年)</div>
          <table class="data-table">
            <thead><tr><th>年度</th><th class="num">家庭收入</th><th class="num">總支出</th><th class="num">淨餘額</th></tr></thead>
            <tbody>
              ${annualRows.map(r => `
                <tr>
                  <td>${r.year} 年</td>
                  <td class="num text-emerald">${formatHKD(r.inc)}</td>
                  <td class="num text-red">${formatHKD(r.exp)}</td>
                  <td class="num ${r.net >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(r.net)}</td>
                </tr>
              `).join('')}
            </tbody>
          </table>
        </div>
      </div>
    `;
    render(container, html);

    renderStats('stats-root', [
      { title: '家庭總餘額', value: formatHKD(bankTotal), color: bankTotal >= 0 ? 'text-emerald' : 'text-red', hint: `共 ${accounts.length} 個帳號` },
      { title: `${year} 家庭收入`, value: formatHKD(yt.totalIncome), color: 'text-emerald' },
      { title: `${year} 總支出`, value: formatHKD(yt.totalExpense), color: 'text-red', hint: `${yt.monthCount} 個月有紀錄` },
      { title: `${year} 淨餘額`, value: formatHKD(yt.net), color: yt.net >= 0 ? 'text-emerald' : 'text-red' },
    ]);
    renderControls('controls-root', { showViewToggle: false });
  };

  // 訂閱資料
  const paths = [
    ['income', `families/${fid}/income`],
    ['expenses', `families/${fid}/expenses`],
    ['bankAccounts', `families/${fid}/bank_accounts`],
    ['policies', `families/${fid}/insurance_policies`],
  ];
  paths.forEach(([key, path]) => {
    unsubs.push(onValue(ref(db, path), (snap) => {
      state[key] = snap.val() || {};
      paint();
    }));
  });

  unsubs.push(AppState.on('ym-change', paint));
  paint();

  return () => {
    unsubs.forEach(u => u && u());
  };
}```

<!-- === FILE END: ./js/pages/dashboard.js === -->

<!-- === FILE START: ./js/pages/finance.js === -->

## 📄 ./js/pages/finance.js

```js
// ============================================
// finance.js — 銀行交易
// ============================================

import { AppState } from '../app-core.js';
import { render, esc, toast } from '../app-ui.js';
import { renderStats } from '../components/stats-cards.js';
import { renderControls } from '../components/page-controls.js';
import { renderTable } from '../components/data-table.js';
import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { formatHKD, BANK_TXN_TYPE_LABELS, BANK_TXN_CATEGORY_LABELS } from '../app-data.js';
import { calcBankBalance } from '../app-domain.js';

export function mount(container) {
  const db = getDatabase();
  const fid = AppState.currentFamilyId;
  const unsubs = [];
  let state = { bankAccounts: {}, members: {} };
  let activeTab = 'banks';

  const paint = () => {
    const { year, month } = AppState.getYearMonth();
    const m = month === 'all' ? '12' : month;
    
    const accounts = Object.entries(state.bankAccounts).map(([id, a]) => ({ id, ...a }));
    const txns = [];
    accounts.forEach(a => Object.entries(a.transactions || {}).forEach(([tid, t]) => txns.push({ id: tid, bankId: a.id, bankName: a.name, ...t })));
    const totalBalance = calcBankBalance(accounts, txns, year, m);

    const html = `
      <div class="page-header" style="margin-bottom:20px;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 銀行交易 ]</h1>
      </div>
      <div id="stats-root"></div>
      <div id="controls-root"></div>
      
      <div style="display:flex; gap:8px; margin-bottom:20px; border-bottom:1px solid var(--border); padding-bottom:10px;">
        <button class="btn ${activeTab === 'banks' ? 'btn-primary' : 'btn-ghost'}" data-tab="banks">銀行清單</button>
        <button class="btn ${activeTab === 'txns' ? 'btn-primary' : 'btn-ghost'}" data-tab="txns">交易記錄</button>
      </div>
      <div id="tab-content"></div>
    `;
    render(container, html);

    renderStats('stats-root', [
      { title: '家庭總餘額', value: formatHKD(totalBalance), color: totalBalance >= 0 ? 'text-emerald' : 'text-red', hint: `共 ${accounts.length} 個帳號` },
      { title: '本月入帳', value: formatHKD(txns.filter(t => t.type === 'in' && t.date?.startsWith(`${year}-${m}`)).reduce((s, t) => s + t.amount, 0)), color: 'text-emerald' },
      { title: '本月出帳', value: formatHKD(txns.filter(t => t.type !== 'in' && t.date?.startsWith(`${year}-${m}`)).reduce((s, t) => s + t.amount, 0)), color: 'text-red' }
    ]);
    renderControls('controls-root', { showViewToggle: false });

    container.querySelectorAll('[data-tab]').forEach(btn => {
      btn.addEventListener('click', () => { activeTab = btn.dataset.tab; paint(); });
    });

    if (activeTab === 'banks') {
      const cols = [
        { id: 'name', label: '名稱' },
        { id: 'type', label: '類型', width: 100 },
        { id: 'initialBalance', label: '初始餘額', type: 'number', width: 130 },
        { id: 'balance', label: '當前餘額', type: 'number', width: 130 }
      ];
      const resolvers = {
        type: v => v === 'personal' ? '個人' : '家庭',
        initialBalance: v => `<span class="mono">${formatHKD(v)}</span>`,
        balance: (v, r) => {
          const bal = calcBankBalance([r], txns.filter(t => t.bankId === r.id), year, m);
          return `<span class="mono ${bal >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(bal)}</span>`;
        }
      };
      const tabEl = document.createElement('div');
      tabEl.id = 'tab-content';
      tabEl.innerHTML = '<div id="bank-table"></div>';
      document.getElementById('tab-content').replaceWith(tabEl);
      renderTable('bank-table', accounts, cols, resolvers, () => false);
    } else {
      const monthPrefix = `${year}-${m}`;
      const filteredTxns = txns.filter(t => t.date?.startsWith(monthPrefix)).sort((a, b) => (b.date || '').localeCompare(a.date || ''));
      const cols = [
        { id: 'date', label: '日期', width: 110 },
        { id: 'bankName', label: '銀行', width: 100 },
        { id: 'typeLabel', label: '類型', width: 90 },
        { id: 'amount', label: '金額', type: 'number', width: 120 },
        { id: 'note', label: '備註' }
      ];
      const resolvers = {
        typeLabel: (v, r) => `<span class="badge ${r.type === 'in' ? 'badge-success' : 'badge-pending'}">${BANK_TXN_TYPE_LABELS[r.type] || r.type}</span>`,
        amount: (v, r) => `<span class="mono ${r.type === 'in' ? 'text-emerald' : 'text-red'}">${r.type === 'in' ? '+' : '-'}${formatHKD(v)}</span>`
      };
      const tabEl = document.createElement('div');
      tabEl.id = 'tab-content';
      tabEl.innerHTML = '<div id="txn-table"></div>';
      document.getElementById('tab-content').replaceWith(tabEl);
      renderTable('txn-table', filteredTxns, cols, resolvers, () => false);
    }
  };

  const paths = [['bankAccounts', 'bank_accounts'], ['members', 'members']];
  paths.forEach(([key, p]) => {
    unsubs.push(onValue(ref(db, `families/${fid}/${p}`), (snap) => {
      state[key] = snap.val() || {};
      paint();
    }));
  });

  unsubs.push(AppState.on('ym-change', paint));
  paint();

  return () => unsubs.forEach(u => u && u());
}```

<!-- === FILE END: ./js/pages/finance.js === -->

<!-- === FILE START: ./js/pages/input-center.js === -->

## 📄 ./js/pages/input-center.js

```js
// ============================================
// input-center.js — 綜合輸入中心
// ============================================

import { AppState } from '../app-core.js';
import { render, esc, toast, openModal, getFormData, buildFormHtml } from '../app-ui.js';
import { renderTable } from '../components/data-table.js';
import { getDatabase, ref, onValue, push, set, get } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { api, formatHKD, statusLabel, statusBadgeClass } from '../app-data.js';

export function mount(container) {
  const db = getDatabase();
  const fid = AppState.currentFamilyId;
  const unsubs = [];
  let activeTab = 'expense';
  let state = { expenses: {}, members: {}, categories: {}, payments: {} };

  const paint = () => {
    const html = `
      <div class="page-header" style="margin-bottom:20px;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 綜合輸入中心 ]</h1>
      </div>
      <div style="display:flex; gap:8px; margin-bottom:20px; border-bottom:1px solid var(--border); padding-bottom:10px;">
        ${['expense', 'insurance', 'fund', 'bank'].map(tab => `
          <button class="btn ${activeTab === tab ? 'btn-primary' : 'btn-ghost'}" data-tab="${tab}">
            ${tab === 'expense' ? '支出' : tab === 'insurance' ? '保險' : tab === 'fund' ? '基金' : '銀行'}
          </button>
        `).join('')}
      </div>
      <div id="tab-content"></div>
    `;
    render(container, html);
    
    // 綁定 Tab
    container.querySelectorAll('[data-tab]').forEach(btn => {
      btn.addEventListener('click', () => { activeTab = btn.dataset.tab; paint(); });
    });

    if (activeTab === 'expense') paintExpense();
    else render('tab-content', `<div class="mecha-panel"><div class="mecha-content">功能開發中，請至對應專頁操作</div></div>`);
  };

  const paintExpense = () => {
    const { year } = AppState.getYearMonth();
    const rows = [];
    Object.entries(state.expenses[year] || {}).forEach(([m, md]) => {
      Object.entries(md.member_expenses || {}).forEach(([mid, items]) => {
        Object.entries(items || {}).forEach(([id, e]) => {
          rows.push({ id, memberId: mid, month: m, ...e });
        });
      });
    });
    rows.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0));

    const cols = [
      { id: 'date', label: '日期', width: 100 },
      { id: 'memberId', label: '成員', width: 100 },
      { id: 'name', label: '項目' },
      { id: 'amount', label: '金額', type: 'number', width: 110 },
      { id: 'status', label: '狀態', width: 100 }
    ];
    const resolvers = {
      memberId: v => state.members[v]?.name || v,
      amount: v => formatHKD(v),
      status: v => `<span class="badge ${statusBadgeClass(v)}">${statusLabel(v)}</span>`
    };

    const tabEl = document.getElementById('tab-content');
    tabEl.innerHTML = `<button class="btn btn-primary" id="btn-add-expense" style="margin-bottom:16px;">+ 新增支出</button><div id="exp-table"></div>`;
    
    renderTable('exp-table', rows, cols, resolvers, () => true);

    document.getElementById('btn-add-expense').addEventListener('click', async () => {
      const fields = [
        { id: 'memberId', label: '成員', type: 'select', required: true, options: Object.entries(state.members).map(([id, m]) => ({ value: id, label: m.name })) },
        { id: 'name', label: '項目名稱', type: 'text', required: true },
        { id: 'amount', label: '金額', type: 'number', required: true },
        { id: 'date', label: '日期', type: 'text', defaultValue: new Date().toISOString().slice(0, 10) }
      ];
      const ok = await openModal('新增支出', buildFormHtml(fields));
      if (!ok) return;
      const data = getFormData(fields);
      const { year, month } = AppState.getYearMonth();
      const m = month === 'all' ? '01' : month;
      await push(ref(db, `families/${fid}/expenses/${year}/${m}/member_expenses/${data.memberId}`), {
        name: data.name, amount: Number(data.amount), status: 'pending', date: data.date, createdAt: Date.now()
      });
      toast('✅ 已新增');
    });
  };

  const paths = [['expenses', 'expenses'], ['members', 'members'], ['categories', 'expense_categories'], ['payments', 'payment_methods']];
  paths.forEach(([key, p]) => {
    unsubs.push(onValue(ref(db, `families/${fid}/${p}`), (snap) => {
      state[key] = snap.val() || {};
      if (key === 'expenses' || key === 'members') paint();
    }));
  });

  unsubs.push(AppState.on('ym-change', paint));
  paint();

  return () => unsubs.forEach(u => u && u());
}```

<!-- === FILE END: ./js/pages/input-center.js === -->

<!-- === FILE START: ./js/pages/member-report.js === -->

## 📄 ./js/pages/member-report.js

```js
// ============================================
// member-report.js — 成員收支明細
// ============================================

import { AppState } from '../app-core.js';
import { render, esc } from '../app-ui.js';
import { renderStats } from '../components/stats-cards.js';
import { renderControls } from '../components/page-controls.js';
import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { formatHKD } from '../app-data.js';

export function mount(container) {
  const db = getDatabase();
  const fid = AppState.currentFamilyId;
  const unsubs = [];
  let state = { members: {}, income: {}, expenses: {}, personalIncome: {} };

  const paint = () => {
    const { year } = AppState.getYearMonth();
    const rows = Object.entries(state.members).map(([mid, m]) => {
      let inc = 0, pi = 0, exp = 0, ins = 0;
      Object.values(state.income[year] || {}).forEach(md => { inc += Number(md[mid]) || 0; });
      Object.values(state.personalIncome[mid]?.[year] || {}).forEach(v => { pi += Number(v) || 0; });
      Object.values(state.expenses[year] || {}).forEach(md => {
        Object.values(md.member_expenses?.[mid] || {}).forEach(e => {
          if (e.isAutoLinked) ins += Number(e.amount) || 0;
          else exp += Number(e.amount) || 0;
        });
      });
      return { id: mid, name: m.name, inc, pi, exp, ins, net: inc + pi - exp - ins };
    });

    const total = rows.reduce((s, r) => ({ inc: s.inc+r.inc, pi: s.pi+r.pi, exp: s.exp+r.exp, ins: s.ins+r.ins }), { inc:0, pi:0, exp:0, ins:0 });
    
    const html = `
      <div class="page-header" style="margin-bottom:20px;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 成員收支明細 ]</h1>
      </div>
      <div id="stats-root"></div>
      <div id="controls-root"></div>
      <div class="mecha-panel" style="margin-top:20px;">
        <div class="mecha-content" style="padding:0; overflow-x:auto;">
          <table class="data-table">
            <thead><tr><th>成員</th><th class="num">家用轉入</th><th class="num">個人收入</th><th class="num">支出</th><th class="num">保險</th><th class="num">淨額</th></tr></thead>
            <tbody>
              ${rows.map(r => `
                <tr>
                  <td>${esc(r.name)}</td>
                  <td class="num text-emerald">${formatHKD(r.inc)}</td>
                  <td class="num text-cyan">${formatHKD(r.pi)}</td>
                  <td class="num text-red">${formatHKD(r.exp)}</td>
                  <td class="num text-orange">${formatHKD(r.ins)}</td>
                  <td class="num ${r.net >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(r.net)}</td>
                </tr>
              `).join('')}
            </tbody>
          </table>
        </div>
      </div>
    `;
    render(container, html);

    renderStats('stats-root', [
      { title: '家庭總收入', value: formatHKD(total.inc), color: 'text-emerald' },
      { title: '個人總收入', value: formatHKD(total.pi), color: 'text-cyan' },
      { title: '家庭總支出', value: formatHKD(total.exp), color: 'text-red' },
      { title: '保險總平攤', value: formatHKD(total.ins), color: 'text-orange' }
    ]);
    renderControls('controls-root', { showViewToggle: false, showMonth: false });
  };

  const paths = [['members', 'members'], ['income', 'income'], ['expenses', 'expenses'], ['personalIncome', 'personal_income']];
  paths.forEach(([key, p]) => {
    unsubs.push(onValue(ref(db, `families/${fid}/${p}`), (snap) => {
      state[key] = snap.val() || {};
      paint();
    }));
  });

  unsubs.push(AppState.on('ym-change', paint));
  paint();

  return () => unsubs.forEach(u => u && u());
}```

<!-- === FILE END: ./js/pages/member-report.js === -->

<!-- === FILE START: ./js/pages/portfolio.js === -->

## 📄 ./js/pages/portfolio.js

```js
// ============================================
// portfolio.js — 基金投資表
// ============================================

import { AppState } from '../app-core.js';
import { render, esc, toast, openModal, getFormData, buildFormHtml } from '../app-ui.js';
import { renderStats } from '../components/stats-cards.js';
import { renderControls } from '../components/page-controls.js';
import { getDatabase, ref, onValue, push, set, get } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { formatHKD } from '../app-data.js';
import { getFundCostMap, findPolicyByFund } from '../app-domain.js';
import { openEntityModal } from '../entity/entity-modal.js';

export function mount(container) {
  const db = getDatabase();
  const fid = AppState.currentFamilyId;
  const unsubs = [];
  let state = { funds: {}, policies: {} };

  const paint = () => {
    const { year, month } = AppState.getYearMonth();
    const m = month === 'all' ? '12' : month;
    
    const funds = Object.entries(state.funds).map(([id, f]) => ({ id, ...f }));
    const policies = Object.entries(state.policies).map(([id, p]) => ({ id, ...p }));

    const costMaps = {};
    policies.forEach(p => { costMaps[p.id] = getFundCostMap(p, funds, year, m); });

    let totalCost = 0;
    let totalValue = 0;
    funds.forEach(f => {
      if (f.type === 'insurance') {
        const p = findPolicyByFund(f, policies);
        totalCost += (p && costMaps[p.id]) ? (costMaps[p.id][f.id] || 0) : (Number(f.cost) || 0);
      } else {
        totalCost += Number(f.cost) || 0;
      }
      totalValue += Number(f.currentValue) || 0;
    });
    const pnl = totalValue - totalCost;
    const pnlPct = totalCost > 0 ? ((pnl / totalCost) * 100).toFixed(2) : '0.00';

    const html = `
      <div class="page-header" style="margin-bottom:20px;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 基金投資表 ]</h1>
      </div>
      <div id="stats-root"></div>
      <div id="controls-root"></div>
      
      <div id="insurance-funds" style="margin-bottom:24px;"></div>
      <div id="standalone-funds"></div>
    `;
    render(container, html);

    renderStats('stats-root', [
      { title: '總投入成本', value: formatHKD(totalCost), hint: `共 ${funds.length} 筆` },
      { title: '總現時價值', value: formatHKD(totalValue), color: 'text-emerald' },
      { title: '總帳面盈虧', value: `${pnl >= 0 ? '+' : ''}${formatHKD(pnl)} (${pnlPct}%)`, color: pnl >= 0 ? 'text-emerald' : 'text-red' }
    ]);
    renderControls('controls-root', { showViewToggle: false });

    const insList = funds.filter(f => f.type === 'insurance');
    const stdList = funds.filter(f => f.type !== 'insurance');

    const cardHtml = (f) => {
      const p = f.type === 'insurance' ? findPolicyByFund(f, policies) : null;
      const cost = (p && costMaps[p.id]) ? (costMaps[p.id][f.id] || 0) : (Number(f.cost) || 0);
      const val = Number(f.currentValue) || 0;
      const pf = val - cost;
      const pfPct = cost > 0 ? ((pf / cost) * 100).toFixed(2) : '0.00';
      
      return `
        <div class="mecha-panel">
          <div class="mecha-content">
            <div style="font-size:16px; font-weight:700; color:var(--primary); margin-bottom:4px;">${esc(f.name)}</div>
            ${p ? `<div style="font-size:11px; color:var(--text-muted); margin-bottom:12px;">關聯保單：${esc(p.name)}</div>` : '<div style="margin-bottom:12px;"></div>'}
            
            <div style="display:flex; justify-content:space-between; font-size:12px; margin-bottom:6px;">
              <span style="color:var(--text-muted);">投入成本</span>
              <span class="mono">${formatHKD(cost)}</span>
            </div>
            <div style="display:flex; justify-content:space-between; font-size:12px; margin-bottom:6px;">
              <span style="color:var(--text-muted);">現時價值</span>
              <span class="mono text-emerald">${formatHKD(val)}</span>
            </div>
            <div style="display:flex; justify-content:space-between; font-size:12px; margin-bottom:12px;">
              <span style="color:var(--text-muted);">盈虧</span>
              <span class="mono ${pf >= 0 ? 'text-emerald' : 'text-red'}">${pf >= 0 ? '+' : ''}${formatHKD(pf)} (${pfPct}%)</span>
            </div>
            
            <div style="margin-top:16px; display:flex; gap:8px; justify-content:flex-end; border-top:1px dashed var(--border); padding-top:12px;">
              <button class="btn btn-ghost" onclick="window.__fundSnap('${f.id}', '${esc(f.name)}')" style="padding:4px 10px; font-size:11px;">快照</button>
              <button class="btn btn-ghost" onclick="window.__fundEdit('${f.id}')" style="padding:4px 10px; font-size:11px;">編輯</button>
            </div>
          </div>
        </div>
      `;
    };

    if (insList.length > 0) {
      render('insurance-funds', `
        <div class="mecha-title">保險基金</div>
        <div style="display:grid; grid-template-columns:repeat(auto-fill, minmax(280px, 1fr)); gap:16px;">
          ${insList.map(cardHtml).join('')}
        </div>
      `);
    } else {
      render('insurance-funds', `<div class="mecha-title">保險基金</div><div class="mecha-panel"><div class="mecha-content text-muted">尚無保險基金</div></div>`);
    }

    if (stdList.length > 0) {
      render('standalone-funds', `
        <div class="mecha-title">獨立基金</div>
        <div style="display:grid; grid-template-columns:repeat(auto-fill, minmax(280px, 1fr)); gap:16px;">
          ${stdList.map(cardHtml).join('')}
        </div>
      `);
    } else {
      render('standalone-funds', `<div class="mecha-title">獨立基金</div><div class="mecha-panel"><div class="mecha-content text-muted">尚無獨立基金</div></div>`);
    }

    window.__fundEdit = (id) => openEntityModal('fund', 'edit', id, funds, paint);
    window.__fundSnap = async (fundId, name) => {
      const fields = [
        { id: 'date', label: '估值日', type: 'text', required: true, defaultValue: new Date().toISOString().slice(0, 10) },
        { id: 'shares', label: '單位數目', type: 'number', required: true },
        { id: 'nav', label: '基金價格 (USD)', type: 'number', required: true }
      ];
      const ok = await openModal(`輸入快照 - ${name}`, buildFormHtml(fields));
      if (!ok) return;
      const data = getFormData(fields);
      await set(ref(db, `families/${fid}/funds/${fundId}/snapshots/${data.date}`), {
        shares: Number(data.shares), nav: Number(data.nav), createdAt: Date.now()
      });
      toast('✅ 已儲存快照');
    };
  };

  const paths = [['funds', 'funds'], ['policies', 'insurance_policies']];
  paths.forEach(([key, p]) => {
    unsubs.push(onValue(ref(db, `families/${fid}/${p}`), (snap) => {
      state[key] = snap.val() || {};
      paint();
    }));
  });

  unsubs.push(AppState.on('ym-change', paint));
  paint();

  return () => unsubs.forEach(u => u && u());
}```

<!-- === FILE END: ./js/pages/portfolio.js === -->

<!-- === FILE START: ./js/pages/settings.js === -->

## 📄 ./js/pages/settings.js

```js
// ============================================
// settings.js — 系統設定
// ============================================

import { AppState, logout } from '../app-core.js';
import { render, esc, toast, openModal, getFormData, buildFormHtml } from '../app-ui.js';
import { api } from '../app-data.js';
import { getDatabase, ref, onValue, set, get } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

export function mount(container) {
  const db = getDatabase();
  const fid = AppState.currentFamilyId;
  const unsubs = [];
  let activeTab = 'banks';
  let state = { bankAccounts: {}, members: {} };

  const paint = () => {
    const html = `
      <div class="page-header" style="margin-bottom:20px;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 系統設定 ]</h1>
      </div>
      <div style="display:flex; gap:8px; margin-bottom:20px; border-bottom:1px solid var(--border); padding-bottom:10px;">
        ${AppState.isSuperAdmin ? `<button class="btn ${activeTab==='platform'?'btn-primary':'btn-ghost'}" data-tab="platform">平台設定</button>` : ''}
        <button class="btn ${activeTab==='banks'?'btn-primary':'btn-ghost'}" data-tab="banks">銀行帳號</button>
        <button class="btn ${activeTab==='personal'?'btn-primary':'btn-ghost'}" data-tab="personal">個人化</button>
      </div>
      <div id="tab-content"></div>
    `;
    render(container, html);

    container.querySelectorAll('[data-tab]').forEach(btn => {
      btn.addEventListener('click', () => { activeTab = btn.dataset.tab; paint(); });
    });

    if (activeTab === 'banks') paintBanks();
    else if (activeTab === 'personal') paintPersonal();
    else if (activeTab === 'platform') paintPlatform();
  };

  const paintBanks = () => {
    const accounts = Object.entries(state.bankAccounts).map(([id, a]) => ({ id, ...a }));
    const html = `
      <div style="display:flex; justify-content:space-between; margin-bottom:16px;">
        <span class="text-muted">共 ${accounts.length} 個帳號</span>
        <button class="btn btn-primary" id="btn-add-bank">+ 新增銀行</button>
      </div>
      <div class="mecha-panel">
        <div class="mecha-content" style="padding:0;">
          <table class="data-table">
            <thead><tr><th>名稱</th><th>類型</th><th class="num">初始餘額</th><th style="width:100px;">操作</th></tr></thead>
            <tbody>
              ${accounts.map(a => `
                <tr>
                  <td>${esc(a.name)}</td>
                  <td>${a.type === 'personal' ? '個人' : '家庭'}</td>
                  <td class="num">${Number(a.initialBalance).toLocaleString()}</td>
                  <td><button class="btn btn-ghost" style="color:#EF4444;" onclick="window.__delBank('${a.id}')">刪除</button></td>
                </tr>
              `).join('')}
            </tbody>
          </table>
        </div>
      </div>
    `;
    render('tab-content', html);
    document.getElementById('btn-add-bank')?.addEventListener('click', () => {
      // 這裡簡化實作，實際可對接 entity-modal
      toast('請至實體新增彈窗操作'); 
    });
    window.__delBank = async (id) => {
      if (!confirm('確定刪除？')) return;
      await api.bankAccounts.remove(id);
      toast('✅ 已刪除');
    };
  };

  const paintPersonal = () => {
    const html = `
      <div style="display:grid; grid-template-columns:repeat(auto-fit, minmax(300px, 1fr)); gap:16px;">
        <div class="mecha-panel">
          <div class="mecha-content">
            <div class="mecha-title">帳號資訊</div>
            <div style="margin-bottom:12px; font-size:13px;">帳號：${esc(AppState.currentUser?.email?.split('@')[0] || '')}</div>
            <div style="margin-bottom:12px; font-size:13px;">角色：${esc(AppState.role)}</div>
            <button class="btn btn-danger" onclick="window.__logout()">登出</button>
          </div>
        </div>
        
        <div class="mecha-panel">
          <div class="mecha-content">
            <div class="mecha-title">除錯工具</div>
            <button class="btn btn-primary" onclick="window.__enableVConsole()">開啟 vConsole</button>
          </div>
        </div>
      </div>
    `;
    render('tab-content', html);
    window.__logout = () => logout();
    window.__enableVConsole = () => {
      const script = document.createElement('script');
      script.src = 'https://cdn.jsdelivr.net/npm/vconsole@3.15.0/dist/vconsole.min.js';
      script.onload = () => { window.vConsole = new window.VConsole(); toast('✅ vConsole 已開啟'); };
      document.head.appendChild(script);
    };
  };

  const paintPlatform = () => {
    render('tab-content', `<div class="mecha-panel"><div class="mecha-content text-muted">平台設定功能請至管理員頁面</div></div>`);
  };

  const unsubs2 = [
    onValue(ref(db, `families/${fid}/bank_accounts`), (snap) => { state.bankAccounts = snap.val() || {}; if(activeTab==='banks') paint(); })
  ];

  paint();
  return () => unsubs2.forEach(u => u && u());
}```

<!-- === FILE END: ./js/pages/settings.js === -->

<!-- === FILE START: ./js/pages/settlements.js === -->

## 📄 ./js/pages/settlements.js

```js
// ============================================
// settlements.js — 結算清單
// ============================================

import { AppState } from '../app-core.js';
import { render, esc, toast } from '../app-ui.js';
import { renderStats } from '../components/stats-cards.js';
import { renderControls } from '../components/page-controls.js';
import { renderTable } from '../components/data-table.js';
import { getDatabase, ref, onValue, get } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { formatHKD, statusBadgeClass, statusLabel, api } from '../app-data.js';
import { mergeSettlementData } from '../app-domain.js';

export function mount(container) {
  const db = getDatabase();
  const fid = AppState.currentFamilyId;
  const unsubs = [];
  let state = { expenses: {}, members: {} };
  let insuranceRows = [];

  const paint = () => {
    const { year, month } = AppState.getYearMonth();
    const m = month === 'all' ? '12' : month;

    // 處理 member expenses
    const memberExpenses = [];
    const mData = state.expenses[year]?.[m]?.member_expenses || {};
    Object.entries(mData).forEach(([mid, items]) => {
      Object.entries(items || {}).forEach(([id, e]) => memberExpenses.push({ id, memberId: mid, ...e }));
    });

    const merged = mergeSettlementData({ memberExpenses, insuranceRows, year, month: m });
    const pending = merged.filter(r => r.status === 'pending');
    const done = merged.filter(r => r.status === 'done');

    const html = `
      <div class="page-header" style="margin-bottom:20px;">
        <h1 class="page-title" style="color:var(--primary); font-family:var(--font-mono);">[ 結算清單 ]</h1>
      </div>
      <div id="stats-root"></div>
      <div id="controls-root"></div>
      <div id="st-table"></div>
    `;
    render(container, html);

    renderStats('stats-root', [
      { title: '待處理', value: formatHKD(pending.reduce((s, r) => s + r.amount, 0)), color: 'text-orange', hint: `${pending.length} 筆` },
      { title: '已處理', value: formatHKD(done.reduce((s, r) => s + r.amount, 0)), color: 'text-emerald', hint: `${done.length} 筆` },
      { title: '總計', value: formatHKD(merged.reduce((s, r) => s + r.amount, 0)), color: 'text-cyan', hint: `${merged.length} 筆` },
    ]);
    renderControls('controls-root', { showViewToggle: false });

    const cols = [
      { id: 'sourceLabel', label: '來源', width: 80 },
      { id: 'memberName', label: '成員', width: 90 },
      { id: 'name', label: '項目名稱' },
      { id: 'amount', label: '金額', type: 'number', width: 110 },
      { id: 'statusLabel', label: '狀態', width: 100 }
    ];
    const resolvers = {
      sourceLabel: v => `<span class="badge ${v === '保險' ? 'badge-magenta' : 'badge-info'}">${v}</span>`,
      memberName: v => state.members[v]?.name || (v === 'shared' ? '家庭' : v),
      amount: v => formatHKD(v),
      statusLabel: (v, r) => `<span class="badge ${statusBadgeClass(r.status)}">${v}</span>`
    };

    renderTable('st-table', merged, cols, resolvers, () => true);

    document.getElementById('st-table').addEventListener('click', async (e) => {
      const btn = e.target.closest('button');
      if (!btn) return;
      const row = merged[Number(btn.dataset.idx)];
      if (!row) return;

      if (btn.dataset.action === 'delete') {
        if (!confirm(`確定刪除 ${row.name}？`)) return;
        if (row.source === 'personal') {
          const { remove } = await import("https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js");
          await remove(ref(db, `families/${fid}/expenses/${row.year}/${row.month}/member_expenses/${row.memberId}/${row._ref.expenseId}`));
          toast('✅ 已刪除');
        }
      } else if (btn.dataset.action === 'edit' && row.source === 'insurance') {
        // 確認保險扣款
        try {
          if (row.status === 'pending') {
            await api.insuranceSync({ policyId: row._ref.policyId, memberId: row.memberId, policyName: row.name, monthlyAverage: row.amount, year: row.year, month: row.month });
            toast('✅ 已確認扣款');
            loadInsurance();
          } else {
            await api.insuranceUnsync({ policyId: row._ref.policyId, memberId: row.memberId, year: row.year, month: row.month, amount: row.amount });
            toast('✅ 已取消扣款');
            loadInsurance();
          }
        } catch (err) {
          toast(err.message, 'error');
        }
      }
    });
  };

  const loadInsurance = async () => {
    const { year, month } = AppState.getYearMonth();
    const m = month === 'all' ? '12' : month;
    const snap = await get(ref(db, `families/${fid}/insurance_policies`));
    const policies = snap.val() || {};
    const list = [];
    for (const [pid, p] of Object.entries(policies)) {
      const paySnap = await get(ref(db, `families/${fid}/insurance_payments/${pid}/${year}/${m}`));
      const pay = paySnap.val() || {};
      if (pay.amount || pay.status) {
        list.push({ policyId: pid, policyName: p.name, policyHolderId: p.policyHolderId || p.memberId, status: pay.status, amount: pay.amount || 0 });
      } else {
        list.push({ policyId: pid, policyName: p.name, policyHolderId: p.policyHolderId || p.memberId, status: 'pending', amount: p.monthlyPremium || 0 });
      }
    }
    insuranceRows = list;
    paint();
  };

  unsubs.push(onValue(ref(db, `families/${fid}/expenses`), (snap) => { state.expenses = snap.val() || {}; paint(); }));
  unsubs.push(onValue(ref(db, `families/${fid}/members`), (snap) => { state.members = snap.val() || {}; paint(); }));
  unsubs.push(AppState.on('ym-change', () => { paint(); loadInsurance(); }));

  paint();
  loadInsurance();

  return () => unsubs.forEach(u => u && u());
}```

<!-- === FILE END: ./js/pages/settlements.js === -->

<!-- === FILE START: ./manifest-app.json === -->

## 📄 ./manifest-app.json

```json
{
  "name": "FAMILY.FIN 智能機械系統",
  "short_name": "FAMILY.FIN",
  "description": "家庭財務、保險供款、基金資產、銀行交易即時管理終端",
  "start_url": "./index.html",
  "scope": "./",
  "display": "standalone",
  "orientation": "portrait",
  "background_color": "#0B0F19",
  "theme_color": "#0B0F19",
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
// sw-app.js — Service Worker (智能機械 v2.1)
// ============================================

const CACHE_NAME = 'family-fin-mecha-v2';

const STATIC_ASSETS = [
  './index.html',
  './app.css',
  './manifest-app.json',
  
  // Core
  './js/app-core.js',
  './js/app-data.js',
  './js/app-domain.js',
  './js/app-ui.js',
  './js/app-pages-loader.js',
  
  // Components
  './js/components/data-table.js',
  './js/components/data-card.js',
  './js/components/stats-cards.js',
  './js/components/page-controls.js',
  
  // Entity
  './js/entity/entity-defs.js',
  './js/entity/entity-helpers.js',
  './js/entity/entity-modal.js',
  './js/entity/funds-allocation.js',
  
  // Pages
  './js/pages/dashboard.js',
  './js/pages/input-center.js',
  './js/pages/code-list.js',
  './js/pages/insurance.js',
  './js/pages/portfolio.js',
  './js/pages/settlements.js',
  './js/pages/finance.js',
  './js/pages/annual-report.js',
  './js/pages/member-report.js',
  './js/pages/settings.js',
  './js/pages/admin.js',
];

self.addEventListener('install', (e) => {
  console.log('[SW] Install', CACHE_NAME);
  e.waitUntil(
    caches.open(CACHE_NAME).then((cache) =>
      Promise.all(
        STATIC_ASSETS.map((url) =>
          cache.add(url).catch((err) => console.warn('[SW] 快取失敗：', url, err.message))
        )
      )
    ).then(() => self.skipWaiting())
  );
});

self.addEventListener('activate', (e) => {
  console.log('[SW] Activate');
  e.waitUntil(
    caches.keys().then((keys) =>
      Promise.all(
        keys.filter((k) => k !== CACHE_NAME).map((k) => caches.delete(k))
      )
    ).then(() => self.clients.claim())
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
      const fallback = await caches.match('./index.html');
      if (fallback) return fallback;
    }
    return new Response('離線中，請稍後再試', { status: 503, headers: { 'Content-Type': 'text/plain; charset=utf-8' } });
  }
}```

<!-- === FILE END: ./sw-app.js === -->
