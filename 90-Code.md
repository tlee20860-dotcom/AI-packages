> 體系版本：SV11
> 生成時間：2026-10-30 23:56:48 (HKT)

# 專案全程式碼包：AI-WEB-APP-Family


<!-- === FILE START: ./.github/workflows/apply-patch.yml === -->

## 📄 ./.github/workflows/apply-patch.yml

```yml
name: Apply Patch (Project)
on:
  workflow_dispatch:
  push:
    paths:
      - '.github/PATCH.md'

permissions:
  contents: write

jobs:
  apply:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.x"

      - name: Download central script
        run: |
          curl -sL https://raw.githubusercontent.com/tlee20860-dotcom/AI-infra/main/.github/scripts/apply_patch.py -o /tmp/apply_patch.py

      - name: Run Patch
        run: python3 /tmp/apply_patch.py

      - name: Commit and push
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add -A
          git diff --cached --quiet && exit 0
          git commit -m "Apply PATCH.md [skip ci]"
          git push

# ═══ EOF ═══
# F: AI-WEB-APP-Family/.github/workflows/apply-patch.yml
# V: 1792358000
# W: ai-system-20261019-010#088
# ═══ EOF ═══```

<!-- === FILE END: ./.github/workflows/apply-patch.yml === -->

<!-- === FILE START: ./.github/workflows/cleanup.yml === -->

## 📄 ./.github/workflows/cleanup.yml

```yml
# ============================================
# cleanup.yml — 一鍵清空專案程式碼（保留骨架）
# 用途：手動觸發，清空所有舊程式碼，為重構鋪路
# ============================================

name: Cleanup Project

on:
  workflow_dispatch:
    inputs:
      confirm:
        description: '請輸入 DELETE_ALL 以確認清空 (此操作不可逆)'
        required: true
        default: ''

jobs:
  cleanup:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Safety Check
        run: |
          if [ "${{ github.event.inputs.confirm }}" != "DELETE_ALL" ]; then
            echo "❌ 確認碼錯誤，終止執行。請輸入 DELETE_ALL"
            exit 1
          fi
          echo "✅ 確認碼正確，準備執行清空。"

      - name: Cleanup Files
        run: |
          echo "🧹 開始清空專案程式碼..."
          
          # 使用 find 刪除所有檔案，排除白名單（.git, .github, meta.json, README.md）
          find . -mindepth 1 -maxdepth 1 \
            ! -name ".git" \
            ! -name ".github" \
            ! -name "meta.json" \
            ! -name "README.md" \
            -exec rm -rf {} +
            
          echo "✅ 清空完成，目前根目錄剩餘檔案："
          ls -la

      - name: Commit and Push
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          
          git add -A
          
          if git diff --cached --quiet; then
            echo "沒有變更，倉庫可能已經是空的。"
          else
            git commit -m "chore: cleanup all project files for rebuild"
            git push
            echo "✅ 清空結果已推送到倉庫。"
          fi

# ═══ EOF ═══
# F: .github/workflows/cleanup.yml
# V: 1792900100
# W: ai-system-20261024-012#1
# ═══ EOF ═══```

<!-- === FILE END: ./.github/workflows/cleanup.yml === -->

<!-- === FILE START: ./.github/workflows/move-api.yml === -->

## 📄 ./.github/workflows/move-api.yml

```yml
# ============================================
# move-api.yml — 自動將 ./api 搬移至 ./functions/api
# 用途：修正 Cloudflare Pages Functions 的目錄結構
# ============================================

name: Move API to Functions Directory

on:
  workflow_dispatch: # 僅允許手動觸發

jobs:
  move-api:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Move Directory
        run: |
          echo "開始搬移目錄..."
          if [ -d "api" ]; then
            mkdir -p functions
            # 優先使用 git mv 保留追蹤歷史
            git mv api functions/api || mv api functions/api
            echo "✅ 已將 api 搬移至 functions/api"
          else
            echo "⚠️ 找不到 api 資料夾，可能已經搬移過。"
          fi

      - name: Commit and Push
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          
          git add -A
          
          if git diff --cached --quiet; then
            echo "沒有變更，無需提交。"
          else
            git commit -m "chore: move api to functions/api for CF Pages"
            git push
            echo "✅ 變更已推送到倉庫。"
          fi

# ═══ EOF ═══
# F: .github/workflows/move-api.yml
# V: 1792900680
# W: ai-system-20261024-012#59
# ═══ EOF ═══```

<!-- === FILE END: ./.github/workflows/move-api.yml === -->

<!-- === FILE START: ./.github/workflows/notify.yml === -->

## 📄 ./.github/workflows/notify.yml

```yml
# ============================================
# notify.yml — 專案變更時通知 AI-infra
# ============================================
name: Notify AI-infra

on:
  push:
    branches: [main, master]
    paths-ignore:
      - '**/*.png'
      - '**/*.jpg'
      - '**/*.jpeg'
      - '**/*.gif'
      - '**/*.svg'
      - '**/*.ico'
      - '.gitignore'
      - 'LICENSE'
      - 'docs/projects/**/WF-STATE-INDEX.md' # 👈 新增這行，避免自動生成的索引檔觸發通知

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger AI-infra build
        run: |
          curl -X POST \
            -H "Accept: application/vnd.github.v3+json" \
            -H "Authorization: token ${{ secrets.CROSS_REPO_PAT }}" \
            https://api.github.com/repos/tlee20860-dotcom/AI-infra/dispatches \
            -d '{"event_type":"project-updated"}'
          echo "已通知 AI-infra 重新建置"```

<!-- === FILE END: ./.github/workflows/notify.yml === -->

<!-- === FILE START: ./admin/admin.js === -->

## 📄 ./admin/admin.js

```js
// ============================================
// admin.js — 平台管理（V2 重構版）
// 位置：admin/admin.js
// 說明：供超級管理員管理家庭清單與平台預設資料。
//       已移除手寫表格，全面改用 renderTable 引擎。
// ============================================

import { api } from '../domain/api-client.js';
import { openConfirm, openModal, closeModal } from '../ui/modal.js';
import { createForm } from '../ui/form.js';
import { showToast } from '../ui/toast.js';
import { formatHKD, esc } from '../ui/format.js';
import { renderTable } from '../ui/table.js';

export default {
  _families: [],

  async render() {
    const content = document.getElementById('app-content');
    if (!content) return;

    content.innerHTML = `
      <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px; flex-wrap:wrap; gap:12px;">
        <h1 style="font-size:24px; font-weight:700; color:var(--primary);">平台管理</h1>
        <button type="button" class="btn btn-primary" id="btn-add-family">
          <i data-lucide="plus" style="width:16px;height:16px;"></i> 新增家庭
        </button>
      </div>

      <div style="display:flex; gap:8px; margin-bottom:20px; border-bottom:1px solid var(--border-color);">
        <button type="button" class="btn btn-ghost" data-tab="families" style="border-bottom:2px solid var(--primary); border-radius:0; color:var(--primary);">家庭列表</button>
        <button type="button" class="btn btn-ghost" data-tab="defaults" style="border-radius:0; color:var(--text-secondary);">平台預設資料</button>
      </div>

      <div id="admin-tab-families">
        <div id="admin-families-list">
          <div style="text-align:center; padding:40px; color:var(--text-muted);">載入中…</div>
        </div>
      </div>

      <div id="admin-tab-defaults" style="display:none;">
        <div class="glass-card" style="padding:20px;">
          <h2 style="font-size:16px; font-weight:700; margin-bottom:16px;">UI 常數預設值</h2>
          <div id="admin-defaults-form-root"></div>
        </div>
      </div>
    `;

    if (window.lucide) window.lucide.createIcons();

    // 綁定 Tab 切換
    document.querySelectorAll('button[data-tab]').forEach(btn => {
      btn.addEventListener('click', (e) => {
        const tab = e.target.dataset.tab;
        document.querySelectorAll('button[data-tab]').forEach(b => {
          b.style.borderBottom = b.dataset.tab === tab ? '2px solid var(--primary)' : 'none';
          b.style.color = b.dataset.tab === tab ? 'var(--primary)' : 'var(--text-secondary)';
        });
        document.getElementById('admin-tab-families').style.display = tab === 'families' ? 'block' : 'none';
        document.getElementById('admin-tab-defaults').style.display = tab === 'defaults' ? 'block' : 'none';
        
        if (tab === 'defaults') this._loadDefaults();
      });
    });

    document.getElementById('btn-add-family')?.addEventListener('click', () => this._openAddFamilyModal());

    // 初始載入
    await this._loadFamilies();
  },

  async _loadFamilies() {
    const listRoot = document.getElementById('admin-families-list');
    if (!listRoot) return;

    try {
      const res = await api.adminListFamilies();
      this._families = res.families || [];
      
      if (this._families.length === 0) {
        listRoot.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">尚無家庭</div>';
        return;
      }

      const renderActions = (row) => `
        <button type="button" class="btn btn-sm btn-ghost" data-action="init" data-uid="${row.uid}">初始化</button>
        <button type="button" class="btn btn-sm btn-danger" data-action="remove" data-uid="${row.uid}">刪除</button>
      `;

      // 使用引擎渲染表格
      renderTable({
        container: listRoot,
        rows: this._families,
        columnsKey: 'adminFamilies',
        renderActions
      });

      // 改用 onclick 屬性賦值，覆蓋舊監聽，避免累積事件
      listRoot.onclick = async (e) => {
        const btn = e.target.closest('button[data-action]');
        if (!btn) return;
        const uid = btn.dataset.uid;
        const family = this._families.find(f => f.uid === uid);
        if (!family) return;

        if (btn.dataset.action === 'init') {
          await this._handleInit(uid, family.name);
        } else if (btn.dataset.action === 'remove') {
          await this._handleRemove(uid, family.name);
        }
      };

    } catch (err) {
      console.error('[admin] 載入家庭失敗：', err);
      listRoot.innerHTML = '<div style="color:var(--accent-danger); text-align:center; padding:20px;">載入失敗</div>';
    }
  },

  _openAddFamilyModal() {
    const modalId = 'add-family-modal';
    document.getElementById(modalId)?.remove();

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `
      <div class="modal" style="max-width:450px; background:var(--bg-surface);">
        <h2 class="modal-title">新增家庭</h2>
        <div id="family-form-root"></div>
      </div>
    `;
    document.body.appendChild(overlay);
    openModal(modalId);

    createForm({
      container: 'family-form-root',
      fields: [
        { id: 'uid', label: 'Family UID (通常為 Firebase Auth 的 uid)', type: 'text', required: true },
        { id: 'name', label: '家庭名稱', type: 'text', required: true },
        { id: 'email', label: '擁有者 Email', type: 'text' }
      ],
      submitText: '新增',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        try {
          await api.adminAddFamily(data.uid, data.name, data.email);
          showToast('✅ 已新增家庭', 'success');
          closeModal(modalId);
          overlay.remove();
          this._loadFamilies();
        } catch (err) {
          showToast('新增失敗：' + err.message, 'error');
        }
      }
    });
  },

  async _handleInit(uid, name) {
    const ok = await openConfirm(`要對家庭「${name}」執行初始化嗎？\n這會寫入平台預設資料（若已有資料會跳過）。`, { title: '初始化家庭' });
    if (!ok) return;
    try {
      await api.adminInitFamily(uid);
      showToast('✅ 初始化完成', 'success');
    } catch (err) {
      showToast('初始化失敗：' + err.message, 'error');
    }
  },

  async _handleRemove(uid, name) {
    const ok = await openConfirm(`⚠️ 確定要刪除家庭「${name}」嗎？\n此操作會一併刪除該家庭的所有資料，且無法復原。`, { title: '刪除家庭', okClass: 'btn-danger' });
    if (!ok) return;
    try {
      await api.adminRemoveFamily(uid);
      showToast('✅ 已刪除家庭', 'success');
      this._loadFamilies();
    } catch (err) {
      showToast('刪除失敗：' + err.message, 'error');
    }
  },

  async _loadDefaults() {
    const formRoot = document.getElementById('admin-defaults-form-root');
    if (!formRoot || formRoot.dataset.loaded) return;
    
    formRoot.innerHTML = '<div style="color:var(--text-muted);">載入中…</div>';
    
    try {
      const res = await api.platformSettings.get();
      const data = res.data || {};

      createForm({
        container: formRoot,
        fields: [
          { id: 'nameMaxLenDesktop', label: '名稱截斷長度 (桌面)', type: 'number', required: true },
          { id: 'nameMaxLenMobile', label: '名稱截斷長度 (手機)', type: 'number', required: true },
          { id: 'toastDuration', label: 'Toast 顯示時間 (毫秒)', type: 'number', required: true }
        ],
        initialData: data,
        submitText: '儲存設定',
        onCancel: null,
        onSubmit: async (formData) => {
          try {
            await api.platformSettings.update(formData);
            showToast('✅ 平台預設已更新', 'success');
          } catch (err) {
            showToast('更新失敗：' + err.message, 'error');
          }
        }
      });
      formRoot.dataset.loaded = '1';
    } catch (err) {
      formRoot.innerHTML = '<div style="color:var(--accent-danger);">載入失敗</div>';
    }
  },

  destroy() {}
};

/* ═══ EOF ═══
F: admin/admin.js
V: 1792901940
W: webapp-20261025-001#42
═══ */```

<!-- === FILE END: ./admin/admin.js === -->

<!-- === FILE START: ./config/app-config.js === -->

## 📄 ./config/app-config.js

```js
// ============================================
// app-config.js — 平台與家庭設定合併（V2 重構版）
// 位置：config/app-config.js
// 說明：讀取平台預設與家庭覆寫，合併為最終設定。
//       依賴 config/firebase.js 與 config/state.js。
// ============================================

import { db } from './firebase.js';
import { AppState } from './state.js';
import { LIMITS } from './constants.js';
import { ref, get, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

/* ============================================
   1. 預設值（當資料庫完全沒有資料時使用）
   ============================================ */
const FALLBACK_OPTIONS = {
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

const FALLBACK_YEAR_RANGE = {
  startYear: null,
  futureYears: LIMITS.YEAR_FUTURE_DEFAULT,
};

const FALLBACK_UI = {
  nameMaxLenDesktop: LIMITS.NAME_MAX_LEN_DESKTOP,
  nameMaxLenMobile:  LIMITS.NAME_MAX_LEN_MOBILE,
  toastDuration:     LIMITS.TOAST_DURATION,
};

/* ============================================
   2. 模組狀態
   ============================================ */
let _platformDefaults = { options: null, yearRange: null, uiConstants: null };
let _familyOverrides  = { options: null, yearRange: null, uiConstants: null };
let _merged = null;
let _listeners = [];
let _unsubscribers = [];

/* ============================================
   3. 初始化與監聽
   ============================================ */
export async function initAppConfig() {
  const familyId = AppState.getFamilyId();

  // 取得平台預設
  const [pOpts, pYearRange, pUI] = await Promise.all([
    _get('platform/defaults/options'),
    _get('platform/defaults/year_range'),
    _get('platform/defaults/ui_constants'),
  ]);
  _platformDefaults = { options: pOpts, yearRange: pYearRange, uiConstants: pUI };

  // 取得家庭覆寫
  if (familyId) {
    const [fOpts, fYearRange, fUI] = await Promise.all([
      _get(`families/${familyId}/settings/options`),
      _get(`families/${familyId}/settings/year_range`),
      _get(`families/${familyId}/settings/ui_constants`),
    ]);
    _familyOverrides = { options: fOpts, yearRange: fYearRange, uiConstants: fUI };
  } else {
    _familyOverrides = { options: null, yearRange: null, uiConstants: null };
  }

  _merged = _mergeAll();
  _notifyAll();
  return _merged;
}

export function watchConfigChanges() {
  _unsubscribers.forEach((unsub) => { try { unsub(); } catch (e) {} });
  _unsubscribers = [];
  const familyId = AppState.getFamilyId();

  const _watch = (path, targetKey, targetObj) => {
    const unsub = onValue(ref(db, path), (snap) => {
      targetObj[targetKey] = snap.val();
      _merged = _mergeAll();
      _notifyAll();
    });
    _unsubscribers.push(unsub);
  };

  // 監聽平台
  _watch('platform/defaults/options', 'options', _platformDefaults);
  _watch('platform/defaults/year_range', 'yearRange', _platformDefaults);
  _watch('platform/defaults/ui_constants', 'uiConstants', _platformDefaults);

  // 監聽家庭
  if (familyId) {
    _watch(`families/${familyId}/settings/options`, 'options', _familyOverrides);
    _watch(`families/${familyId}/settings/year_range`, 'yearRange', _familyOverrides);
    _watch(`families/${familyId}/settings/ui_constants`, 'uiConstants', _familyOverrides);
  }
}

export function disposeAppConfig() {
  _unsubscribers.forEach((unsub) => { try { unsub(); } catch (e) {} });
  _unsubscribers = [];
  _listeners = [];
  _merged = null;
}

/* ============================================
   4. 對外查詢 API
   ============================================ */
export function getOptions(key) {
  const opts = _merged?.options;
  if (!opts || Object.keys(opts).length === 0) {
    return FALLBACK_OPTIONS[key] || [];
  }
  return opts[key] || FALLBACK_OPTIONS[key] || [];
}

export function getYearRange() {
  const cfg = _merged?.yearRange || FALLBACK_YEAR_RANGE;
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
  const cfg = _merged?.uiConstants || FALLBACK_UI;
  return {
    nameMaxLenDesktop: Number(cfg.nameMaxLenDesktop) || LIMITS.NAME_MAX_LEN_DESKTOP,
    nameMaxLenMobile: Number(cfg.nameMaxLenMobile) || LIMITS.NAME_MAX_LEN_MOBILE,
    toastDuration: Number(cfg.toastDuration) || LIMITS.TOAST_DURATION,
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
   5. 內部工具
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

function _mergeAll() {
  return {
    options: _mergeOptions(_familyOverrides.options, _platformDefaults.options),
    yearRange: _mergeObject(_familyOverrides.yearRange, _platformDefaults.yearRange, FALLBACK_YEAR_RANGE),
    uiConstants: _mergeObject(_familyOverrides.uiConstants, _platformDefaults.uiConstants, FALLBACK_UI),
  };
}

function _mergeOptions(familyOpts, platformOpts) {
  const hasFamily = familyOpts && typeof familyOpts === 'object' && Object.keys(familyOpts).length > 0;
  const hasPlatform = platformOpts && typeof platformOpts === 'object' && Object.keys(platformOpts).length > 0;

  const base = hasPlatform ? platformOpts : FALLBACK_OPTIONS;
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

/* ═══ EOF ═══
F: config/app-config.js
V: 1792900210
W: ai-system-20261024-012#16
═══ EOF ═══ */```

<!-- === FILE END: ./config/app-config.js === -->

<!-- === FILE START: ./config/columns.js === -->

## 📄 ./config/columns.js

```js
// ============================================
// columns.js — 表格欄位與解析器 SSOT（V2 重構版）
// 位置：config/columns.js
// 說明：集中管理表格欄位結構與專屬的顯示解析（如金額、Badge）。
//       通用格式化依賴 ui/format.js。
// ============================================

import { esc, formatHKD, formatNumber } from '../ui/format.js';
import { resolveName } from './entity-registry.js';
import { RESERVED_IDS } from './constants.js';
import { badgeClass as statusBadgeClass, label as statusLabel } from './status.js';

/* ============================================
   1. 欄位定義 (結構)
   ============================================ */
export const COLUMNS = {
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
    { id: 'member', label: '成員', defaultVisible: true, defaultWidth: 90, sortable: true },
    { id: 'name', label: '項目名稱', defaultVisible: true, defaultWidth: 200, sortable: true },
    { id: 'amount', label: '金額', defaultVisible: true, defaultWidth: 110, type: 'number', sortable: true },
    { id: 'date', label: '日期', defaultVisible: true, defaultWidth: 100, sortable: true },
    { id: 'status', label: '狀態', defaultVisible: true, defaultWidth: 140, sortable: true, filterable: true },
  ],

  bankTransactions: [
    { id: 'date', label: '日期', defaultVisible: true, defaultWidth: 110, sortable: true },
    { id: 'bankName', label: '銀行', defaultVisible: true, defaultWidth: 100, sortable: true, filterable: true },
    { id: 'typeLabel', label: '類型', defaultVisible: true, defaultWidth: 90, sortable: true, filterable: true },
    { id: 'catLabel', label: '分類', defaultVisible: true, defaultWidth: 100, sortable: true },
    { id: 'memberName', label: '成員', defaultVisible: true, defaultWidth: 100, sortable: true },
    { id: 'amount', label: '金額', defaultVisible: true, defaultWidth: 120, type: 'number', sortable: true },
    { id: 'note', label: '備註', defaultVisible: true, defaultWidth: 180 },
  ],

  codeList: [
    { id: 'code', label: '編號', defaultVisible: true, defaultWidth: 180, sortable: true },
    { id: 'entityLabel', label: '類型', defaultVisible: true, defaultWidth: 120, sortable: true, filterable: true },
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 240, sortable: true },
  ],

  dashboardAnnual: [
    { id: 'year', label: '年度', defaultVisible: true, defaultWidth: 100, sortable: true },
    { id: 'totalIncome', label: '家庭收入', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'totalExpense', label: '總支出', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'insurance', label: '保險平攤', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'net', label: '淨餘額', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
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
    { id: 'income', label: '家用轉入', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'personalIncome', label: '個人收入', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'expense', label: '一般支出', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'insurance', label: '保險平攤', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'net', label: '淨結餘', defaultVisible: true, defaultWidth: 140, type: 'number', sortable: true },
  ],

  /* 🆕 管理員家庭列表 */
  adminFamilies: [
    { id: 'uid', label: 'UID', defaultVisible: true, defaultWidth: 220, sortable: true },
    { id: 'name', label: '名稱', defaultVisible: true, defaultWidth: 160, sortable: true },
    { id: 'ownerEmail', label: '擁有者 Email', defaultVisible: true, defaultWidth: 200, sortable: true },
    { id: 'createdAt', label: '建立時間', defaultVisible: true, defaultWidth: 170, sortable: true },
  ],

  /* 🆕 年度報表月度明細 */
  annualMonthly: [
    { id: 'month', label: '月份', defaultVisible: true, defaultWidth: 100, sortable: true },
    { id: 'totalIncome', label: '總收入', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'totalExpense', label: '總支出', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
    { id: 'netBalance', label: '淨餘額', defaultVisible: true, defaultWidth: 130, type: 'number', sortable: true },
  ],
};

/* ============================================
   2. 顯示解析器 (Renderers)
   ============================================ */
export const RESOLVERS = {
  bankAccounts: {
    name: (v) => esc(v || '—'),
    type: (v) => {
      const l = v === 'personal' ? '個人' : '家庭';
      const c = v === 'personal' ? 'badge-muted' : 'badge-info';
      return `<span class="badge ${c}">${esc(l)}</span>`;
    },
    initialBalance: (v) => `<span class="mono">${formatHKD(v)}</span>`,
    initialYM: (_, r) => `<span class="mono" style="font-size:12px;color:var(--text-muted);">${esc(r.initialYear || '—')}-${esc(r.initialMonth || '—')}</span>`,
  },

  funds: {
    name: (v) => esc(v || '（未命名）'),
    typeLabel: (v) => v === 'insurance' ? '<span class="badge badge-magenta">保險</span>' : '<span class="badge badge-info">獨立</span>',
    cost: (v) => `<span class="mono">${formatHKD(v)}</span>`,
    currentValue: (v) => `<span class="mono text-emerald">${formatHKD(v)}</span>`,
    pnl: (v, r) => {
      const p = (Number(r.currentValue) || 0) - (Number(r.cost) || 0);
      return `<span class="mono ${p >= 0 ? 'text-emerald' : 'text-red'}">${p >= 0 ? '+' : ''}${formatHKD(p)}</span>`;
    },
    units: (v) => `<span class="mono">${v || '—'}</span>`,
  },

  insurancePolicies: {
    name: (v) => esc(v || '（未命名）'),
    company: (v) => esc(v || '—'),
    holderName: (_, r) => esc(resolveName('members', r.policyHolderId || r.memberId) || '—'),
    insuredName: (_, r) => esc(resolveName('members', r.memberId) || '—'),
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
      if (r.source === 'insurance') return '<span class="badge badge-success">🛡 保險</span>';
      return r.memberId === RESERVED_IDS.SHARED_MEMBER 
        ? '<span class="badge badge-warning">🏠 家庭</span>' 
        : '<span class="badge badge-info">🏷 個人</span>';
    },
    yearMonth: (_, r) => `${esc(r.year)}-${esc(r.month)}`,
    member: (_, r) => r.memberId === RESERVED_IDS.SHARED_MEMBER 
      ? '🏠 家庭共用' 
      : (r.memberId ? esc(resolveName('members', r.memberId) || r.memberId) : '—'),
    name: (v) => esc(v || '—'),
    amount: (v) => formatHKD(v),
    date: (v) => esc(v || '—'),
    status: (v, r) => `<span class="badge ${statusBadgeClass(v)}">${esc(statusLabel(v, { source: r.source }))}</span>`,
  },

  bankTransactions: {
    date: (v) => `<span class="mono" style="font-size:12px;">${esc(v || '—')}</span>`,
    bankName: (v) => esc(v || '—'),
    typeLabel: (_, r) => {
      const map = { in: '入帳', out: '出帳', transfer: '轉帳' };
      const cls = r.type === 'in' ? 'badge-success' : (r.type === 'transfer' ? 'badge-info' : 'badge-pending');
      return `<span class="badge ${cls}">${esc(map[r.type] || r.type)}</span>`;
    },
    catLabel: (_, r) => {
      const map = { contribution: '家用轉入', expense: '支出', insurance: '保險', reimbursement: '代墊', manual: '手動' };
      return `<span class="badge badge-muted">${esc(map[r.category] || r.category)}</span>`;
    },
    memberName: (v) => v ? esc(resolveName('members', v) || v) : '—',
    amount: (v, r) => `<span class="mono ${r.type === 'in' ? 'text-emerald' : 'text-red'}">${r.type === 'in' ? '+' : '-'}${formatHKD(v)}</span>`,
    note: (v) => esc(v || '—'),
  },

  codeList: {
    code: (v) => v ? `<span class="mono" style="font-size:12px; color:var(--primary);">${esc(v)}</span>` : '<span class="mono" style="color:var(--text-muted);">—</span>',
    entityLabel: (v) => `<span class="badge badge-info">${esc(v || '—')}</span>`,
    name: (v) => esc(v || '（未命名）'),
  },

  dashboardAnnual: {
    year: (v, r) => `${v} 年${r.isCurrent ? ' <span class="badge badge-info" style="font-size:10px;">今年</span>' : ''}`,
    totalIncome: (v) => `<span class="text-emerald mono">${formatHKD(v)}</span>`,
    totalExpense: (v) => `<span class="text-red mono">${formatHKD(v)}</span>`,
    insurance: (v) => `<span class="text-magenta mono">${formatHKD(v)}</span>`,
    net: (v) => `<span class="${v >= 0 ? 'text-emerald' : 'text-red'} mono">${formatHKD(v)}</span>`,
  },

  recentExpense: {
    date: (v) => esc(v || '—'),
    memberName: (_, r) => r.memberId === RESERVED_IDS.SHARED_MEMBER ? '🏠 家庭共用' : esc(resolveName('members', r.memberId) || r.memberId || '—'),
    name: (v) => esc(v || '（未命名）'),
    amount: (v) => formatHKD(v),
    status: (v) => `<span class="badge ${statusBadgeClass(v)}">${esc(statusLabel(v))}</span>`,
  },

  memberReport: {
    name: (v, r) => r.id === RESERVED_IDS.SHARED_MEMBER
      ? '🏠 家庭共用'
      : esc(v || '（未知成員）'),
    income: (v) => `<span class="mono text-emerald">${formatHKD(v)}</span>`,
    personalIncome: (v) => `<span class="mono text-primary">${formatHKD(v)}</span>`,
    expense: (v) => `<span class="mono text-red">${formatHKD(v)}</span>`,
    insurance: (v) => `<span class="mono text-magenta">${formatHKD(v)}</span>`,
    net: (v) => `<span class="mono ${v >= 0 ? 'text-emerald' : 'text-red'}" style="font-weight:700;">${formatHKD(v)}</span>`,
  },

  /* 🆕 管理員家庭列表 */
  adminFamilies: {
    uid: (v) => `<span class="mono" style="font-size:12px;">${esc(v)}</span>`,
    name: (v) => esc(v || '（未命名）'),
    ownerEmail: (v) => `<span class="mono" style="font-size:12px; color:var(--text-muted);">${esc(v || '—')}</span>`,
    createdAt: (v) => `<span class="mono" style="font-size:12px; color:var(--text-muted);">${v ? new Date(v).toLocaleString('zh-HK') : '—'}</span>`,
  },

  /* 🆕 年度報表月度明細 */
  annualMonthly: {
    month: (v) => esc(v),
    totalIncome: (v) => `<span class="mono text-emerald">${formatHKD(v)}</span>`,
    totalExpense: (v) => `<span class="mono text-red">${formatHKD(v)}</span>`,
    netBalance: (v) => `<span class="mono ${v >= 0 ? 'text-emerald' : 'text-red'}">${formatHKD(v)}</span>`,
  },
};

/* ============================================
   3. 對外 API
   ============================================ */
export function getColumns(key) {
  const c = COLUMNS[key];
  if (!c) { console.warn(`[columns] 未知 key：${key}`); return []; }
  return c.map((x) => ({ ...x }));
}

export function getResolvers(key) {
  const r = RESOLVERS[key];
  return r ? { ...r } : {};
}

/* ═══ EOF ═══
F: config/columns.js
V: 1792901930
W: webapp-20261025-001#41
═══ */```

<!-- === FILE END: ./config/columns.js === -->

<!-- === FILE START: ./config/constants.js === -->

## 📄 ./config/constants.js

```js
// ============================================
// constants.js — 全站核心常數（V2 重構版）
// 位置：config/constants.js
// 說明：只保留全站共用常數、路由與側邊欄設定。
//       狀態定義移至 status.js；預設資料移至 app-config.js。
// ============================================

/* ============================================
   1. 版本與儲存鍵
   ============================================ */
export const APP_VERSION = 'v2.0.0';
export const SW_VERSION = 'family-fin-v2-v1';

export const STORAGE_KEYS = {
  YEAR: 'v2_ui_year',
  MONTH: 'v2_ui_month',
  THEME: 'v2_ui_theme',
  FAMILY_ID: 'v2_family_id',
  FAMILY_NAME: 'v2_family_name',
  VIEW_PREFIX: 'v2_ui_view_',
  SIDEBAR_STATE: 'v2_ui_sidebar',
};

export const SESSION_KEYS = {
  AUTH_CONTEXT: 'v2_auth_ctx_',
};

/* ============================================
   2. 數值限制
   ============================================ */
export const LIMITS = {
  NAME_MAX_LEN_DESKTOP: 12,
  NAME_MAX_LEN_MOBILE: 6,
  AMOUNT_MAX: 1000000000,
  YEAR_PAST_DEFAULT: 3,
  YEAR_FUTURE_DEFAULT: 5,
  TOAST_DURATION: 2000,
  PAGE_SIZE: 20,
};

/* ============================================
   3. 保留 ID 與角色
   ============================================ */
export const RESERVED_IDS = {
  EXTRA_INCOME: 'extra',
  SHARED_MEMBER: 'shared',
};

export const ROLES = {
  SUPERADMIN: 'superadmin',
  OWNER: 'owner',
  MEMBER: 'member',
};

/* ============================================
   4. SPA 路由（Hash 模式）
   ============================================ */
export const ROUTES = {
  LOGIN: '/login',
  DASHBOARD: '/dashboard',
  INPUT_CENTER: '/input-center',
  CODE_LIST: '/code-list',
  INSURANCE: '/insurance',
  PORTFOLIO: '/portfolio',
  BANK: '/bank',
  SETTLEMENTS: '/settlements',
  ANNUAL_REPORT: '/annual-report',
  MEMBER_REPORT: '/member-report',
  SETTINGS: '/settings',
  ADMIN: '/admin',
};

/* ============================================
   5. 側邊欄導覽（4 大分類 + 系統）
   ============================================ */
const _ROLES_FAMILY = [ROLES.OWNER, ROLES.MEMBER];
const _ROLES_ALL = [ROLES.OWNER, ROLES.MEMBER, ROLES.SUPERADMIN];
const _ROLES_ADMIN = [ROLES.SUPERADMIN];

export const SIDEBAR_GROUPS = [
  {
    key: 'dashboard',
    label: '總覽',
    icon: 'home',
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'home', label: '總覽儀表板', path: ROUTES.DASHBOARD },
    ],
  },
  {
    key: 'input',
    label: '輸入與清單',
    icon: 'pencil',
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'pencil', label: '綜合輸入中心', path: ROUTES.INPUT_CENTER },
      { icon: 'database', label: '編號對照清單', path: ROUTES.CODE_LIST },
    ],
  },
  {
    key: 'assets',
    label: '資產與保障',
    icon: 'shield',
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'shield', label: '保險清單表', path: ROUTES.INSURANCE },
      { icon: 'line-chart', label: '基金投資表', path: ROUTES.PORTFOLIO },
      { icon: 'landmark', label: '銀行交易', path: ROUTES.BANK },
      { icon: 'clipboard-check', label: '結算清單', path: ROUTES.SETTLEMENTS },
    ],
  },
  {
    key: 'reports',
    label: '報表',
    icon: 'bar-chart-3',
    roles: _ROLES_FAMILY,
    items: [
      { icon: 'bar-chart-3', label: '年度報表', path: ROUTES.ANNUAL_REPORT },
      { icon: 'users', label: '成員收支明細', path: ROUTES.MEMBER_REPORT },
    ],
  },
  {
    key: 'admin',
    label: '平台管理',
    icon: 'server',
    roles: _ROLES_ADMIN,
    items: [
      { icon: 'settings', label: '家庭與帳號', path: ROUTES.ADMIN },
    ],
  },
  {
    key: 'system',
    label: '系統',
    icon: 'settings',
    roles: _ROLES_ALL,
    items: [
      { icon: 'settings', label: '系統設定', path: ROUTES.SETTINGS },
    ],
  },
];

export function getSidebarGroupsByRole(role) {
  if (!role) return SIDEBAR_GROUPS.filter((g) => (g.roles || []).includes(ROLES.OWNER));
  return SIDEBAR_GROUPS.filter((g) => (g.roles || []).includes(role));
}

/* ═══ EOF ═══
F: config/constants.js
V: 1792900150
W: ai-system-20261024-012#10
═══ EOF ═══ */```

<!-- === FILE END: ./config/constants.js === -->

<!-- === FILE START: ./config/entity-registry.js === -->

## 📄 ./config/entity-registry.js

```js
// ============================================
// entity-registry.js — 實體名稱解析 SSOT（V2 重構版）
// 位置：config/entity-registry.js
// 說明：集中訂閱基礎實體（成員、銀行、類別等），
//       將 ID 轉為名稱，避免各頁面重複查詢。
// ============================================

import { ref, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { db } from './firebase.js';
import { AppState } from './state.js';

// 定義需要快取名稱的實體路徑
const PATHS = {
  members:       'members',
  banks:         'banks',
  bank_accounts: 'bank_accounts',
  categories:    'expense_categories',
  items:         'expense_items',
  companies:     'insurance_companies',
  payments:      'payment_methods',
};

const _lists = {};
const _nameMaps = {};
const _unsubscribers = [];
let _initialized = false;

/* ============================================
   1. 初始化與銷毀
   ============================================ */
export function initEntityRegistry() {
  if (_initialized) return true;

  const familyId = AppState.getFamilyId();
  if (!familyId) {
    console.warn('[entity-registry] 尚未選擇家庭，略過初始化');
    return false;
  }

  Object.entries(PATHS).forEach(([key, path]) => {
    const r = ref(db, `families/${familyId}/${path}`);
    
    // 排序邏輯：成員與銀行按 order，其餘按 createdAt
    const sortFn = (key === 'members' || key === 'banks' || key === 'bank_accounts')
      ? (a, b) => {
          const oa = a.order != null ? a.order : Number.MAX_SAFE_INTEGER;
          const ob = b.order != null ? b.order : Number.MAX_SAFE_INTEGER;
          return oa !== ob ? oa - ob : (a.createdAt || 0) - (b.createdAt || 0);
        }
      : (a, b) => (a.createdAt || 0) - (b.createdAt || 0);

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
  console.log('✅ Entity Registry 已初始化');
  return true;
}

export function destroyEntityRegistry() {
  _unsubscribers.forEach((unsub) => {
    try { unsub(); } catch (e) { /* noop */ }
  });
  _unsubscribers.length = 0;

  Object.keys(_lists).forEach((k) => { delete _lists[k]; });
  Object.keys(_nameMaps).forEach((k) => { delete _nameMaps[k]; });

  _initialized = false;
}

/* ============================================
   2. 查詢 API
   ============================================ */

/**
 * 取得實體名稱
 * @param {string} type - members / banks / categories 等
 * @param {string} id 
 * @param {string} [fallback='']
 * @returns {string}
 */
export function resolveName(type, id, fallback = '') {
  if (!id) return fallback;
  const map = _nameMaps[type];
  if (!map) return fallback;
  return map[id] || fallback;
}

/**
 * 取得完整實體列表（深拷貝）
 * @param {string} type 
 * @returns {Array}
 */
export function getList(type) {
  return [...(_lists[type] || [])];
}

/**
 * 取得名稱映射表（深拷貝）
 * @param {string} type 
 * @returns {Object}
 */
export function getNameMap(type) {
  return { ...(_nameMaps[type] || {}) };
}

/* ═══ EOF ═══
F: config/entity-registry.js
V: 1792900190
W: ai-system-20261024-012#14
═══ EOF ═══ */```

<!-- === FILE END: ./config/entity-registry.js === -->

<!-- === FILE START: ./config/firebase.js === -->

## 📄 ./config/firebase.js

```js
// ============================================
// firebase.js — Firebase 初始化與環境切換（V2 重構版）
// 位置：config/firebase.js
// 說明：預設連線新版 DB (new)。
//       支援 ?db=old 切換至舊版 DB 進行唯讀測試。
// ============================================

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
import { getAuth } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
import { getDatabase } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

/* ============================================
   1. 環境設定檔
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

/* ============================================
   2. 環境判定（預設 new）
   ============================================ */
function _resolveDbTarget() {
  if (typeof window === 'undefined') return 'new';
  
  const params = new URLSearchParams(window.location.search);
  const fromUrl = params.get('db');
  
  // 開發測試用：如果帶了 ?db=old，則連線舊庫
  if (fromUrl === 'old') return 'old';
  
  // 預設一律連線新版庫
  return 'new';
}

export const DB_TARGET = _resolveDbTarget();
export const IS_READONLY = new URLSearchParams(window.location.search).get('readonly') === '1';

const firebaseConfig = FIREBASE_CONFIGS[DB_TARGET];

console.log(`[Firebase] 初始化環境: ${DB_TARGET.toUpperCase()} (唯讀: ${IS_READONLY})`);

/* ============================================
   3. 初始化與匯出
   ============================================ */
export const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getDatabase(app);

/* ═══ EOF ═══
F: config/firebase.js
V: 1792900160
W: ai-system-20261024-012#11
═══ EOF ═══ */```

<!-- === FILE END: ./config/firebase.js === -->

<!-- === FILE START: ./config/state.js === -->

## 📄 ./config/state.js

```js
// ============================================
// state.js — 全域狀態中心（V2 重構版）
// 位置：config/state.js
// 說明：集中管理使用者、家庭、年月、主題與檢視模式。
//       內建輕量事件總線 (Pub/Sub)，供頁面監聽狀態變化。
// ============================================

import { STORAGE_KEYS, ROLES } from './constants.js';

export const AppState = {
  currentUser: null,
  isSuperAdmin: false,
  
  currentFamilyId: '',
  currentFamilyName: '',
  memberAccount: null,
  currentMemberId: '', // 🆕 補齊 V1 狀態
  
  role: '',
  canInput: false,
  displayName: '',
  
  year: String(new Date().getFullYear()),
  month: 'all',
  theme: 'dark',
  currentView: 'table',

  _listeners: {},
  _initialized: false,

  /* ============================================
     初始化與銷毀
     ============================================ */
  init() {
    if (this._initialized) return;
    this._initialized = true;

    const savedYear = localStorage.getItem(STORAGE_KEYS.YEAR);
    const savedMonth = localStorage.getItem(STORAGE_KEYS.MONTH);
    const savedFamilyId = localStorage.getItem(STORAGE_KEYS.FAMILY_ID);
    const savedFamilyName = localStorage.getItem(STORAGE_KEYS.FAMILY_NAME);
    const savedTheme = localStorage.getItem(STORAGE_KEYS.THEME);

    this.year = savedYear || String(new Date().getFullYear());
    this.month = savedMonth || 'all';
    this.theme = savedTheme === 'light' ? 'light' : 'dark';

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
    this.memberAccount = null;
    this.currentMemberId = ''; // 🆕 重置
    this.role = '';
    this.canInput = false;
    this.displayName = '';
    this.currentView = 'table';
    this._initialized = false;
  },

  /* ============================================
     主題
     ============================================ */
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

  toggleTheme() {
    this.setTheme(this.theme === 'dark' ? 'light' : 'dark');
  },

  /* ============================================
     使用者與權限
     ============================================ */
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

  /* 🆕 補齊 Member ID 相關 API */
  getCurrentMemberId() {
    if (this.currentMemberId) return this.currentMemberId;
    if (this.memberAccount && this.memberAccount.memberId) return this.memberAccount.memberId;
    return '';
  },

  setCurrentMemberId(memberId) {
    this.currentMemberId = memberId || '';
    this.emit('current-member-change', this.currentMemberId);
  },

  /* ============================================
     家庭資訊
     ============================================ */
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

    this.emit('family-change', {
      familyId: this.currentFamilyId,
      familyName: this.currentFamilyName,
    });
  },

  getFamilyId() { return this.currentFamilyId; },
  getFamilyName() { return this.currentFamilyName; },
  hasFamily() { return !!this.currentFamilyId; },

  /* ============================================
     年月與檢視
     ============================================ */
  setYearMonth(year, month) {
    this.year = String(year);
    this.month = String(month);
    localStorage.setItem(STORAGE_KEYS.YEAR, this.year);
    localStorage.setItem(STORAGE_KEYS.MONTH, this.month);
    this.emit('ym-change', { year: this.year, month: this.month });
  },

  getYearMonth() { return { year: this.year, month: this.month }; },
  
  isAnnualMode() { return this.month === 'all'; },

  setCurrentView(view) {
    if (view !== 'table' && view !== 'card') return;
    if (this.currentView === view) return;
    this.currentView = view;
    this.emit('view-change', view);
  },

  getCurrentView() { return this.currentView; },

  /* ============================================
     事件總線 (Pub/Sub)
     ============================================ */
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

/* ═══ EOF ═══
F: config/state.js
V: 1792901760
W: webapp-20261025-001#26
═══ */```

<!-- === FILE END: ./config/state.js === -->

<!-- === FILE START: ./config/status.js === -->

## 📄 ./config/status.js

```js
// ============================================
// status.js — 狀態系統 SSOT（V2 重構版）
// 位置：config/status.js
// 說明：集中管理狀態代碼（pending/done/skipped）與舊資料相容映射。
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
   2. 舊名稱 → 新代碼（相容層）
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
   3. 顯示文字（依來源）
   ============================================ */
const LABELS_DEFAULT = {
  [STATUS.PENDING]: '未處理',
  [STATUS.DONE]:    '已處理',
  [STATUS.SKIPPED]: '不適用',
};

const LABELS_BY_SOURCE = {
  personal:  { [STATUS.PENDING]: '未處理', [STATUS.DONE]: '已處理', [STATUS.SKIPPED]: '不適用' },
  fixed:     { [STATUS.PENDING]: '未付款', [STATUS.DONE]: '已付款', [STATUS.SKIPPED]: '不適用' },
  insurance: { [STATUS.PENDING]: '未扣款', [STATUS.DONE]: '已扣款', [STATUS.SKIPPED]: '不適用' },
  income:    { [STATUS.PENDING]: '未轉入', [STATUS.DONE]: '已轉入', [STATUS.SKIPPED]: '不適用' },
};

/* ============================================
   4. 核心 API
   ============================================ */

/**
 * 正規化任意輸入為標準代碼
 * @param {string} raw 
 * @returns {'pending'|'done'|'skipped'}
 */
export function normalize(raw) {
  if (raw == null || raw === '') return STATUS.PENDING;
  if (raw === STATUS.PENDING || raw === STATUS.DONE || raw === STATUS.SKIPPED) {
    return raw;
  }
  return LEGACY_MAP[raw] || STATUS.PENDING;
}

/**
 * 是否為「完成」狀態
 * @param {string} raw 
 * @returns {boolean}
 */
export function isDone(raw) {
  const code = normalize(raw);
  return code === STATUS.DONE || code === STATUS.SKIPPED;
}

/**
 * 取得顯示文字
 * @param {string} raw 
 * @param {Object} [opts]
 * @param {'personal'|'fixed'|'insurance'|'income'} [opts.source] 
 * @returns {string}
 */
export function label(raw, opts = {}) {
  const code = normalize(raw);
  const source = opts.source;

  if (source && LABELS_BY_SOURCE[source] && LABELS_BY_SOURCE[source][code]) {
    return LABELS_BY_SOURCE[source][code];
  }
  return LABELS_DEFAULT[code];
}

/**
 * 取得下拉選項
 * @param {Object} [opts]
 * @param {'personal'|'fixed'|'insurance'|'income'} [opts.source]
 * @param {boolean} [opts.includeSkipped=false]
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
  if (code === STATUS.DONE) return 'badge-success';
  if (code === STATUS.SKIPPED) return 'badge-muted';
  return 'badge-pending';
}

/* ═══ EOF ═══
F: config/status.js
V: 1792900170
W: ai-system-20261024-012#12
═══ EOF ═══ */```

<!-- === FILE END: ./config/status.js === -->

<!-- === FILE START: ./css/app.css === -->

## 📄 ./css/app.css

```css
/* ============================================
   app.css — 全站排版與元件樣式（完全對齊舊版）
   ============================================ */

/* ============================================
   1. 佈局 (Layout)
   ============================================ */
.app-shell {
  display: flex;
  min-height: 100vh;
  position: relative;
  background-color: var(--bg-deep);
}

.sidebar {
  width: 260px;
  flex-shrink: 0;
  background: var(--bg-surface);
  border-right: 1px solid var(--border-color);
  display: flex;
  flex-direction: column;
  z-index: 100;
}

.sidebar-header {
  height: 64px;
  display: flex;
  align-items: center;
  padding: 0 20px;
  border-bottom: 1px solid var(--border-color);
}

.sidebar-logo {
  font-family: var(--font-mono);
  font-weight: 700;
  font-size: 18px;
  color: var(--primary);
  letter-spacing: 1px;
}

.sidebar-nav {
  flex: 1;
  overflow-y: auto;
  padding: 12px 0;
}

.main-area {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.content {
  flex: 1;
  padding: 24px;
  max-width: 1400px;
  width: 100%;
  margin: 0 auto;
}

/* ============================================
   2. Navbar
   ============================================ */
.navbar {
  height: 64px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 0 24px;
  background: var(--bg-surface);
  border-bottom: 1px solid var(--border-color);
  position: sticky;
  top: 0;
  z-index: 90;
}

.hamburger { display: none; background: transparent; border: none; color: var(--text-primary); }
.navbar-title { font-size: 16px; font-weight: 600; flex: 1; }
.navbar-right { display: flex; align-items: center; gap: 12px; }

.navbar-db-tag {
  padding: 4px 12px;
  border-radius: 999px;
  font-size: 12px;
  font-family: var(--font-mono);
  background: var(--primary);
  color: #fff;
}

.navbar-theme-btn { background: transparent; border: none; color: var(--text-secondary); cursor: pointer; }
.navbar-user { display: flex; align-items: center; gap: 6px; font-size: 13px; color: var(--text-secondary); }
.navbar-logout { background: transparent; border: none; color: var(--accent-danger); cursor: pointer; padding: 4px; }

/* ============================================
   3. Sidebar 選單
   ============================================ */
.nav-group { margin-bottom: 4px; }
.nav-group-title {
  padding: 12px 20px;
  font-size: 13px;
  color: var(--text-secondary);
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.nav-group-label { display: flex; align-items: center; gap: 10px; }

.nav-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 20px 12px 44px;
  color: var(--text-secondary);
  font-size: 14px;
  text-decoration: none;
  border-left: 3px solid transparent;
}
.nav-item.root-item { padding-left: 20px; }
.nav-item:hover, .nav-item.active {
  background: var(--bg-hover);
  color: var(--primary);
}
.nav-item.active { border-left-color: var(--primary); font-weight: 500; }
.nav-icon { width: 18px; height: 18px; }

/* ============================================
   4. 基礎元件 (卡片、按鈕)
   ============================================ */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 20px;
  font-size: 14px;
  font-weight: 500;
  border-radius: var(--radius-sm);
  border: 1px solid var(--border-color);
  background: var(--bg-surface);
  color: var(--text-primary);
  cursor: pointer;
  text-decoration: none;
}

.btn-primary { background: var(--primary); border-color: var(--primary); color: #fff; font-weight: 600; }
.btn-danger { background: var(--accent-danger); border-color: var(--accent-danger); color: #fff; }
.btn-ghost { background: transparent; border-color: transparent; color: var(--text-secondary); }
.btn-sm { padding: 6px 12px; font-size: 12px; }

.input {
  padding: 10px 12px;
  background: var(--bg-surface);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-sm);
  color: var(--text-primary);
  font-size: 14px;
}

/* 卡片 (對齊舊版：白底、微陰影、圓角 12px) */
.glass-card {
  background: var(--bg-surface);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  box-shadow: 0 1px 3px rgba(0,0,0,0.05);
}

/* ============================================
   5. 表格 (Table) - 對齊截圖樣式
   ============================================ */
.table-container {
  background: var(--bg-surface);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  overflow: hidden;
}

.data-table { width: 100%; border-collapse: collapse; font-size: 14px; }

.data-table th {
  text-align: left;
  padding: 14px 16px;
  font-size: 13px;
  font-weight: 600;
  color: var(--text-primary);
  background: var(--bg-hover);
  border-bottom: 1px solid var(--border-color);
  white-space: nowrap;
}

.data-table td {
  padding: 16px;
  border-bottom: 1px solid var(--border-color);
  color: var(--text-primary);
}

.data-table tbody tr:last-child td { border-bottom: none; }
.data-table tbody tr:hover { background: var(--bg-hover); }
.data-table .num { text-align: right; font-family: var(--font-mono); font-weight: 600; }

/* Badge */
.badge { display: inline-block; padding: 4px 12px; border-radius: 999px; font-size: 12px; font-weight: 500; }
.badge-success { background: rgba(16, 185, 129, 0.1); color: var(--accent-success); }
.badge-danger { background: rgba(244, 63, 94, 0.1); color: var(--accent-danger); }
.badge-info { background: rgba(29, 78, 216, 0.1); color: var(--primary); }
.badge-warning { background: rgba(251, 191, 36, 0.1); color: var(--accent-warm); }
.badge-muted { background: var(--bg-hover); color: var(--text-secondary); }

/* 文字顏色 */
.text-emerald { color: var(--accent-success); }
.text-red { color: var(--accent-danger); }
.text-primary { color: var(--primary); }
.text-muted { color: var(--text-muted); }
.mono { font-family: var(--font-mono); }

/* ============================================
   6. Modal (彈出視窗) - 本次新增修復
   ============================================ */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(2px);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.2s ease, visibility 0.2s ease;
}

.modal-overlay.active {
  opacity: 1;
  visibility: visible;
}

.modal {
  background: var(--bg-surface);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-md);
  width: 100%;
  max-width: 500px;
  max-height: 90vh;
  overflow-y: auto;
  transform: translateY(20px);
  transition: transform 0.2s ease;
  padding: 24px;
  box-sizing: border-box;
}

.modal-overlay.active .modal {
  transform: translateY(0);
}

/* ============================================
   7. 響應式 (Mobile)
   ============================================ */
@media (max-width: 768px) {
  .sidebar { position: fixed; top: 0; left: 0; height: 100vh; transform: translateX(-100%); }
  .sidebar.mobile-open { transform: translateX(0); }
  .hamburger { display: flex; }
  .sidebar-backdrop { display: none; position: fixed; inset: 0; background: rgba(0, 0, 0, 0.5); z-index: 99; }
  .sidebar-backdrop.active { display: block; }
  .content { padding: 16px; }
  .navbar { padding: 0 16px; }
  .navbar-db-tag { display: none; }
  .data-table th, .data-table td { padding: 12px 10px; font-size: 13px; }
}

/* Navbar 相關 */
.navbar-user {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
  color: var(--text-secondary);
}
.navbar-logout {
  background: transparent;
  border: none;
  color: var(--accent-danger);
  cursor: pointer;
  display: flex;
  align-items: center;
  padding: 4px;
  margin-left: 4px;
}
.navbar-theme-btn {
  background: transparent;
  border: none;
  cursor: pointer;
  color: var(--text-secondary);
  display: flex;
  align-items: center;
}

/* ═══ EOF ═══
F: css/app.css
V: 1792901300
W: webapp-20261025-001#11
═══ */```

<!-- === FILE END: ./css/app.css === -->

<!-- === FILE START: ./css/theme.css === -->

## 📄 ./css/theme.css

```css
/* ============================================
   theme.css — 全局主題與變數（V2 重構版）
   位置：css/theme.css
   說明：預設為暗色科技風，支援 [data-theme="light"] 切換。
   ============================================ */

:root,
[data-theme="dark"] {
  /* 背景 */
  --bg-deep: #0F172A;      /* 深空黑 */
  --bg-surface: #1E293B;   /* 卡片/表面 */
  --bg-hover: #334155;     /* 懸停 */

  /* 文字 */
  --text-primary: #F1F5F9;
  --text-secondary: #CBD5E1;
  --text-muted: #94A3B8;

  /* 主色與點綴 */
  --primary: #00F0FF;      /* 賽博青 */
  --primary-dark: #00C4D1;
  --accent-warm: #FBBF24;  /* 橘黃 */
  --accent-danger: #F43F5E;/* 霓虹洋紅 */
  --accent-success: #10B981; /* 翠綠 */

  /* 邊框與分隔線 */
  --border-color: #334155;
  --border-light: rgba(255, 255, 255, 0.08);

  /* 陰影 */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.3);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.4);
  --shadow-glow: 0 0 10px rgba(0, 240, 255, 0.15);

  /* 圓角 */
  --radius-sm: 6px;
  --radius-md: 12px;
  --radius-lg: 16px;

  /* 字型 */
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang TC", "Microsoft JhengHei", sans-serif;
  --font-mono: "JetBrains Mono", "SF Mono", Consolas, Menlo, monospace;

  /* 動畫 */
  --transition: 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

[data-theme="light"] {
  /* 背景 */
  --bg-deep: #F8FAFC;      /* 極淺灰 */
  --bg-surface: #FFFFFF;   /* 純白 */
  --bg-hover: #F1F5F9;

  /* 文字 */
  --text-primary: #0F172A;
  --text-secondary: #475569;
  --text-muted: #94A3B8;

  /* 主色與點綴 */
  --primary: #1D4ED8;      /* 經典藍 */
  --primary-dark: #1E40AF;
  --accent-warm: #F97316;  
  --accent-danger: #E11D48;
  --accent-success: #059669;

  /* 邊框 */
  --border-color: #E2E8F0;
  --border-light: #F1F5F9;

  /* 陰影 */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-glow: 0 0 10px rgba(29, 78, 216, 0.1);
}

/* ============================================
   基礎重置與全站樣式
   ============================================ */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html, body {
  height: 100%;
  background-color: var(--bg-deep);
  color: var(--text-primary);
  font-family: var(--font-sans);
  font-size: 15px;
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
  transition: background-color var(--transition), color var(--transition);
}

body {
  overflow-x: hidden;
  /* 暗色模式下加入微弱的背景光暈 */
  background-image: 
    radial-gradient(circle at 15% 50%, rgba(0, 240, 255, 0.03) 0%, transparent 50%),
    radial-gradient(circle at 85% 30%, rgba(244, 63, 94, 0.03) 0%, transparent 50%);
  background-attachment: fixed;
}

a {
  color: var(--primary);
  text-decoration: none;
  transition: opacity var(--transition);
}
a:hover { opacity: 0.8; }

button {
  font-family: inherit;
  cursor: pointer;
  border: none;
  background: none;
}

.mono { font-family: var(--font-mono); }

/* 自訂捲軸 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
::-webkit-scrollbar-track {
  background: transparent;
}
::-webkit-scrollbar-thumb {
  background: var(--border-color);
  border-radius: 4px;
}
::-webkit-scrollbar-thumb:hover {
  background: var(--text-muted);
}

/* 載入動畫修復（對應 index.html） */
#app-loading { display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; }
.spinner { width: 40px; height: 40px; border: 4px solid var(--border-light); border-top-color: var(--primary); border-radius: 50%; animation: spin 1s linear infinite; margin-bottom: 16px; }
@keyframes spin { to { transform: rotate(360deg); } }

/* ═══ EOF ═══
F: css/theme.css
V: 1792900140
W: ai-system-20261024-012#9
═══ EOF ═══ */```

<!-- === FILE END: ./css/theme.css === -->

<!-- === FILE START: ./db/index.js === -->

## 📄 ./db/index.js

```js
// ============================================
// index.js — 資料層統一入口（V2 重構版）
// 位置：db/index.js
// 說明：將 read.js 與 write.js 的所有方法統一匯出。
//       外部模組只需 import { ... } from '../db/index.js'。
// ============================================

// 匯出讀取與監聽
export * from './read.js';

// 匯出寫入與更新
export * from './write.js';

// 匯出基礎路徑工具
export { getFamilyPath } from './read.js';

/* ═══ EOF ═══
F: db/index.js
V: 1792900240
W: ai-system-20261024-012#19
═══ EOF ═══ */```

<!-- === FILE END: ./db/index.js === -->

<!-- === FILE START: ./db/read.js === -->

## 📄 ./db/read.js

```js
// ============================================
// read.js — 資料讀取與監聽（V2 重構版）
// 位置：db/read.js
// 說明：集中處理 Firebase 的 onValue 監聽與 get 單次讀取。
//       大幅精簡了舊版 db.js 的樣板代碼。
// ============================================

import { db } from '../config/firebase.js';
import { AppState } from '../config/state.js';
import { ref, onValue, get } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

/* ============================================
   1. 通用工具
   ============================================ */
const _byCreatedAt = (a, b) => (a.createdAt || 0) - (b.createdAt || 0);
const _byOrder = (a, b) => (a.order || 0) - (b.order || 0);
const _byOrderThenCreated = (a, b) => {
  const oa = a.order != null ? a.order : Number.MAX_SAFE_INTEGER;
  const ob = b.order != null ? b.order : Number.MAX_SAFE_INTEGER;
  return oa !== ob ? oa - ob : (a.createdAt || 0) - (b.createdAt || 0);
};

export function getFamilyPath(subpath) {
  const familyId = AppState.getFamilyId();
  if (!familyId) throw new Error('尚未選擇家庭');
  return `families/${familyId}/${subpath}`;
}

/**
 * 監聽單一物件
 */
export function listen(subpath, callback, onError) {
  const familyId = AppState.getFamilyId();
  if (!familyId) { if (onError) onError(new Error('尚未選擇家庭')); return () => {}; }
  return onValue(ref(db, `families/${familyId}/${subpath}`), callback, (err) => {
    console.error(`[db/read] 讀取失敗 [${subpath}]：`, err);
    if (onError) onError(err);
  });
}

/**
 * 監聽列表（自動轉為 Array 並排序）
 */
export function listenList(subpath, sortFn, callback, onError) {
  return listen(subpath, (snap) => {
    const val = snap.val() || {};
    const list = Object.entries(val).map(([id, x]) => ({ id, ...x }));
    list.sort(sortFn);
    callback(list);
  }, onError);
}

/**
 * 單次讀取單一物件
 */
export async function getOnce(subpath) {
  const snap = await get(ref(db, getFamilyPath(subpath)));
  return snap.val() || null;
}

/**
 * 單次讀取列表
 */
export async function getListOnce(subpath, sortFn = _byCreatedAt) {
  const val = await getOnce(subpath) || {};
  const list = Object.entries(val).map(([id, x]) => ({ id, ...x }));
  list.sort(sortFn);
  return list;
}

/* ============================================
   2. 基礎實體讀取 (Members, Banks, Categories...)
   ============================================ */
export const listenMembers = (cb, err) => listenList('members', _byOrderThenCreated, cb, err);
export const getMembersOnce = () => getListOnce('members', _byOrderThenCreated);

export const listenBanks = (cb, err) => listenList('banks', _byOrderThenCreated, cb, err);
export const getBanksOnce = () => getListOnce('banks', _byOrderThenCreated);

export const listenBankAccounts = (cb, err) => listenList('bank_accounts', _byOrderThenCreated, cb, err);
export const getBankAccountsOnce = () => getListOnce('bank_accounts', _byOrderThenCreated);

export const listenCategories = (cb, err) => listenList('expense_categories', _byOrder, cb, err);
export const getCategoriesOnce = () => getListOnce('expense_categories', _byOrder);

export const listenItems = (cb, err) => listenList('expense_items', _byCreatedAt, cb, err);
export const getItemsOnce = () => getListOnce('expense_items', _byCreatedAt);

export const listenPaymentMethods = (cb, err) => listenList('payment_methods', _byOrder, cb, err);
export const getPaymentMethodsOnce = () => getListOnce('payment_methods', _byOrder);

export const listenInsuranceCompanies = (cb, err) => listenList('insurance_companies', _byCreatedAt, cb, err);
export const getInsuranceCompaniesOnce = () => getListOnce('insurance_companies', _byCreatedAt);

export const listenStatuses = (cb, err) => listenList('statuses', _byOrder, cb, err);
export const getStatusesOnce = () => getListOnce('statuses', _byOrder);

/* ============================================
   3. 核心業務資料讀取 (Policies, Funds, Expenses...)
   ============================================ */
export const listenInsurancePolicies = (cb, err) => listenList('insurance_policies', _byCreatedAt, cb, err);
export const getInsurancePoliciesOnce = () => getListOnce('insurance_policies', _byCreatedAt);

export const listenFunds = (cb, err) => listenList('funds', _byCreatedAt, cb, err);
export const getFundsOnce = () => getListOnce('funds', _byCreatedAt);

export const listenIncome = (y, m, cb, err) => listen(`income/${y}/${m}`, (s) => cb(s.val() || {}), err);
export const getIncomeOnce = async (y, m) => await getOnce(`income/${y}/${m}`) || {};

export const listenPersonalIncome = (mid, cb, err) => listen(`personal_income/${mid}`, (s) => cb(s.val() || {}), err);
export const getPersonalIncomeOnce = async (mid) => await getOnce(`personal_income/${mid}`) || {};

export const getInsurancePaymentsOnce = async (pid) => await getOnce(`insurance_payments/${pid}`) || {};

/* ============================================
   4. 全域展開讀取 (All Expenses, All Transactions)
   ============================================ */
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

export function listenAllBankTransactions(cb, err) {
  return listen('bank_accounts', (snap) => {
    const flat = [];
    Object.entries(snap.val() || {}).forEach(([bid, bd]) => {
      Object.entries(bd.transactions || {}).forEach(([tid, txn]) => {
        flat.push({ id: tid, bankId: bid, bankName: bd.name || '', ...txn });
      });
    });
    flat.sort(_byCreatedAt);
    cb(flat);
  }, err);
}

export function listenAllMemberAdvances(cb, err) {
  return listen('member_advances', (snap) => {
    const flat = [];
    Object.entries(snap.val() || {}).forEach(([mid, ad]) => {
      Object.entries(ad || {}).forEach(([aid, adv]) => flat.push({ id: aid, memberId: mid, ...adv }));
    });
    flat.sort(_byCreatedAt);
    cb(flat);
  }, err);
}

/* ============================================
   5. 基金快照（相容新舊格式）
   ============================================ */
export async function getFundSnapshotListOnce(fundId) {
  const val = await getOnce(`funds/${fundId}/snapshots`) || {};
  const list = [];
  Object.entries(val).forEach(([key, v]) => {
    if (/^\d{4}-\d{2}-\d{2}$/.test(key)) {
      // 新格式：YYYY-MM-DD
      list.push({ dateKey: key, _rawPath: key, ...v });
    } else if (/^\d{4}$/.test(key) && v && typeof v === 'object') {
      // 舊格式：{year: {month: {...}}}
      Object.entries(v).forEach(([month, snap2]) => {
        if (snap2 && typeof snap2 === 'object' && snap2.shares === undefined) {
          Object.entries(snap2).forEach(([day, snap3]) => {
            if (snap3 && typeof snap3 === 'object') {
              list.push({ dateKey: `${key}-${month}-${String(day).padStart(2, '0')}`, _rawPath: `${key}/${month}/${day}`, ...snap3 });
            }
          });
        } else if (snap2 && typeof snap2 === 'object') {
          list.push({ dateKey: `${key}-${month}-01`, _rawPath: `${key}/${month}`, ...snap2 });
        }
      });
    }
  });
  list.sort((a, b) => a.dateKey.localeCompare(b.dateKey));
  return list;
}

/* ═══ EOF ═══
F: db/read.js
V: 1792900220
W: ai-system-20261024-012#17
═══ EOF ═══ */```

<!-- === FILE END: ./db/read.js === -->

<!-- === FILE START: ./db/write.js === -->

## 📄 ./db/write.js

```js
// ============================================
// write.js — 資料寫入與更新（V2 重構版）
// 位置：db/write.js
// 說明：集中處理 Firebase 的 set / update / push / remove。
//       內建 IS_READONLY 保護機制，防止誤改舊版資料。
// ============================================

import { db, IS_READONLY } from '../config/firebase.js';
import { AppState } from '../config/state.js';
import { normalize as normalizeStatus } from '../config/status.js';
import { ref, set, update, push, remove } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

const roundInt = (v) => Math.round(Number(v) || 0);

export function familyRef(subpath) {
  const familyId = AppState.getFamilyId();
  if (!familyId) throw new Error('尚未選擇家庭');
  return ref(db, `families/${familyId}/${subpath}`);
}

function _checkReadOnly() {
  if (IS_READONLY) {
    throw new Error('目前為唯讀模式 (?readonly=1)，禁止寫入操作');
  }
}

/* ============================================
   1. 基礎原語 (Primitives)
   ============================================ */
export async function saveObject(path, data) {
  _checkReadOnly();
  await set(familyRef(path), data);
}

export async function updateObject(path, data) {
  _checkReadOnly();
  await update(familyRef(path), data);
}

export async function pushObject(path, data) {
  _checkReadOnly();
  const r = push(familyRef(path));
  await set(r, data);
  return r.key;
}

export async function removeObject(path) {
  _checkReadOnly();
  await remove(familyRef(path));
}

/* ============================================
   2. 成員與基礎實體
   ============================================ */
export async function addMember(m) {
  return pushObject('members', {
    name: m.name || '',
    role: m.role || 'other',
    order: m.order != null ? Number(m.order) : 0,
    createdAt: Date.now(),
  });
}

export async function updateMember(id, p) {
  const c = {};
  if (p.name !== undefined) c.name = String(p.name).trim();
  if (p.role !== undefined) c.role = p.role;
  if (p.order !== undefined) c.order = Number(p.order) || 0;
  if (!Object.keys(c).length) return;
  await updateObject(`members/${id}`, c);
}

export async function removeMember(id) {
  await removeObject(`members/${id}`);
}

export async function addBankAccount(d) {
  return pushObject('bank_accounts', {
    name: String(d.name || '').trim(),
    type: d.type || 'family',
    initialBalance: roundInt(d.initialBalance),
    initialYear: String(d.initialYear || new Date().getFullYear()),
    initialMonth: String(d.initialMonth || '01').padStart(2, '0'),
    order: Number(d.order) || 0,
    createdAt: Date.now(),
  });
}

export async function updateBankAccount(id, p) {
  const c = {};
  ['name', 'type'].forEach((k) => { if (p[k] !== undefined) c[k] = p[k]; });
  if (p.initialBalance !== undefined) c.initialBalance = roundInt(p.initialBalance);
  if (p.initialYear !== undefined) c.initialYear = String(p.initialYear);
  if (p.initialMonth !== undefined) c.initialMonth = String(p.initialMonth).padStart(2, '0');
  if (p.order !== undefined) c.order = Number(p.order) || 0;
  if (!Object.keys(c).length) return;
  await updateObject(`bank_accounts/${id}`, c);
}

export async function removeBankAccount(id) {
  await removeObject(`bank_accounts/${id}`);
}

/* ============================================
   3. 銀行交易 (新增補齊)
   ============================================ */
export async function addBankTransaction(bid, d) {
  if (!bid) throw new Error('缺少 bankId');
  return pushObject(`bank_accounts/${bid}/transactions`, {
    type: d.type || 'out',
    category: d.category || 'manual',
    amount: roundInt(d.amount),
    date: d.date || new Date().toISOString().slice(0, 10),
    memberId: d.memberId || '',
    refId: d.refId || '',
    note: d.note || '',
    createdAt: Date.now(),
  });
}

export async function updateBankTransaction(bid, tid, p) {
  if (!bid || !tid) throw new Error('缺少參數');
  const c = {};
  ['type', 'category', 'amount', 'date', 'memberId', 'refId', 'note'].forEach((k) => {
    if (p[k] !== undefined) c[k] = p[k];
  });
  if (c.amount != null) c.amount = roundInt(c.amount);
  if (!Object.keys(c).length) return;
  c.updatedAt = Date.now();
  await updateObject(`bank_accounts/${bid}/transactions/${tid}`, c);
}

export async function removeBankTransaction(bid, tid) {
  if (!bid || !tid) throw new Error('缺少參數');
  await removeObject(`bank_accounts/${bid}/transactions/${tid}`);
}

/* ============================================
   4. 支出紀錄
   ============================================ */
export async function addExpense(y, m, mid, exp) {
  return pushObject(`expenses/${y}/${m}/member_expenses/${mid}`, {
    name: exp.name || '',
    amount: roundInt(exp.amount),
    status: normalizeStatus(exp.status || 'pending'),
    date: exp.date || '',
    categoryId: exp.categoryId || '',
    itemId: exp.itemId || '',
    paymentMethodId: exp.paymentMethodId || '',
    bankId: exp.bankId || '',
    txnId: exp.txnId || '',
    isAutoLinked: exp.isAutoLinked || false,
    policyId: exp.policyId || '',
    createdAt: Date.now(),
  });
}

export async function updateExpense(y, m, mid, eid, p) {
  const c = { ...p };
  if (c.amount != null) c.amount = roundInt(c.amount);
  if (c.status !== undefined) c.status = normalizeStatus(c.status);
  await updateObject(`expenses/${y}/${m}/member_expenses/${mid}/${eid}`, c);
}

export async function removeExpense(y, m, mid, eid) {
  await removeObject(`expenses/${y}/${m}/member_expenses/${mid}/${eid}`);
}

/* ============================================
   5. 收入與代墊
   ============================================ */
export async function saveIncome(y, m, data) {
  const up = {};
  Object.entries(data || {}).forEach(([k, v]) => {
    const n = roundInt(v);
    up[k] = n > 0 ? n : null;
  });
  if (!Object.keys(up).length) return;
  await updateObject(`income/${y}/${m}`, up);
}

export async function savePersonalIncome(mid, y, m, amt) {
  const n = roundInt(amt);
  const path = `personal_income/${mid}/${y}`;
  if (n > 0) {
    await updateObject(path, { [String(m).padStart(2, '0')]: n });
  } else {
    await removeObject(`${path}/${String(m).padStart(2, '0')}`);
  }
}

export async function addMemberAdvance(mid, d) {
  if (!mid) throw new Error('缺少 memberId');
  return pushObject(`member_advances/${mid}`, {
    policyId: d.policyId || '',
    totalAmount: roundInt(d.totalAmount),
    remainingAmount: roundInt(d.totalAmount),
    startYear: String(d.startYear || ''),
    startMonth: String(d.startMonth || '').padStart(2, '0'),
    note: d.note || '',
    paidMonths: {},
    createdAt: Date.now(),
  });
}

export async function updateMemberAdvance(mid, aid, p) {
  if (!mid || !aid) throw new Error('缺少參數');
  const c = {};
  ['policyId', 'totalAmount', 'remainingAmount', 'startYear', 'startMonth', 'note', 'paidMonths'].forEach((k) => {
    if (p[k] !== undefined) c[k] = p[k];
  });
  if (c.totalAmount != null) c.totalAmount = roundInt(c.totalAmount);
  if (c.remainingAmount != null) c.remainingAmount = roundInt(c.remainingAmount);
  if (!Object.keys(c).length) return;
  c.updatedAt = Date.now();
  await updateObject(`member_advances/${mid}/${aid}`, c);
}

/* ============================================
   6. 基金快照與保單
   ============================================ */
export async function saveFundSnapshot(fundId, dateKey, data) {
  if (!/^\d{4}-\d{2}-\d{2}$/.test(dateKey)) throw new Error('dateKey 必須是 YYYY-MM-DD 格式');
  const path = `funds/${fundId}/snapshots/${dateKey}`;
  await saveObject(path, {
    shares: Number(data.shares) || 0,
    nav: Number(data.nav) || 0,
    value: roundInt(Number(data.shares || 0) * Number(data.nav || 0)),
    updatedAt: Date.now(),
  });
}

export async function removeFundSnapshot(fundId, dateKey, rawPath) {
  const path = rawPath || dateKey;
  await removeObject(`funds/${fundId}/snapshots/${path}`);
}

export async function updateInsurancePolicy(id, p) {
  const c = {};
  const allowed = ['type', 'memberId', 'policyHolderId', 'name', 'company', 'paymentType', 'paymentMode', 'advanceHolderId', 'advanceId', 'firstStartYear', 'firstStartMonth', 'totalPolicyYears', 'totalPolicyPeriods', 'totalPremium', 'currentPeriodIndex', 'monthlyPremium', 'annualPremium', 'account', 'periods', 'isCompleted', 'fundsAllocation'];
  allowed.forEach((k) => { if (p[k] !== undefined) c[k] = p[k]; });
  if (!Object.keys(c).length) return;
  await updateObject(`insurance_policies/${id}`, c);
}

/* ═══ EOF ═══
F: db/write.js
V: 1792900270
W: ai-system-20261024-012#22
═══ EOF ═══ */```

<!-- === FILE END: ./db/write.js === -->

<!-- === FILE START: ./domain/api-client.js === -->

## 📄 ./domain/api-client.js

```js
// ============================================
// api-client.js — 統一 API 呼叫封裝（V2 重構版）
// 位置：domain/api-client.js
// 說明：自動附帶 Firebase Token，處理 401/403 錯誤。
//       將舊版散落的 fetch 集中管理。
// ============================================

import { auth } from '../config/firebase.js';
import { AppState } from '../config/state.js';
import { STORAGE_KEYS, ROUTES } from '../config/constants.js';

/* ============================================
   1. 核心 fetch 封裝
   ============================================ */
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

  // 處理身分過期
  if (res.status === 401) {
    localStorage.removeItem(STORAGE_KEYS.FAMILY_ID);
    localStorage.removeItem(STORAGE_KEYS.FAMILY_NAME);
    window.location.hash = ROUTES.LOGIN;
    throw new Error('登入已過期，請重新登入');
  }

  // 處理權限不足
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

/* ============================================
   2. 常用 API 集合
   ============================================ */
function getFamilyId() {
  const id = AppState.getFamilyId();
  if (!id) throw new Error('尚未選擇家庭');
  return id;
}

export const api = {
  // 核心身分
  lookupFamily: () =>
    callApi('/api/lookup-family', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({}),
    }),

  // 總覽與報表
  summary: (year, month) =>
    callApi(`/api/summary?familyId=${getFamilyId()}&year=${year}&month=${month}`),

  fetchAnnualSummaryRange: async (startYear, endYear) =>
    callApi(`/api/annual-summary?familyId=${getFamilyId()}&startYear=${startYear}&endYear=${endYear}`),

  fetchSettlementsYear: async (year) =>
    callApi(`/api/settlements-year?familyId=${getFamilyId()}&year=${year}`),

  // 保險
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

  // 統一實體 CRUD (D-063)
  entityCreate: (payload) =>
    callApi('/api/entity-create', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ ...payload, familyId: getFamilyId() }),
    }),

  // 管理員專用
  adminListFamilies: () => callApi('/api/admin-families?action=list'),
  
  adminAddFamily: (uid, name, email) =>
    callApi('/api/admin-families', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ action: 'add', uid, name, email }),
    }),

  adminInitFamily: (uid) =>
    callApi('/api/admin-init-family', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ uid }),
    }),
};

/* ═══ EOF ═══
F: domain/api-client.js
V: 1792900320
W: ai-system-20261024-012#27
═══ EOF ═══ */```

<!-- === FILE END: ./domain/api-client.js === -->

<!-- === FILE START: ./domain/auth.js === -->

## 📄 ./domain/auth.js

```js
// ============================================
// auth.js — 身分驗證與權限（V2 重構版）
// 位置：domain/auth.js
// 說明：封裝 Firebase Auth 操作，處理自訂帳號轉換。
//       整合了舊版的 auth.js 與 auth-guard.js 核心功能。
// ============================================

import { auth } from '../config/firebase.js';
import { AppState } from '../config/state.js';
import { STORAGE_KEYS, ROLES } from '../config/constants.js';
import {
  signInWithEmailAndPassword,
  signOut,
  onAuthStateChanged,
} from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";

/* ============================================
   1. 帳號格式轉換 (自訂帳號 <-> Email)
   ============================================ */
export const SUPERADMIN_DOMAIN = '@familyfin.local';
export const SUPERADMIN_EMAIL = `superadmin${SUPERADMIN_DOMAIN}`;

export function toEmail(account) {
  if (!account) return '';
  return account.includes('@') ? account : `${account}${SUPERADMIN_DOMAIN}`;
}

export function toAccount(email) {
  if (!email) return '';
  return email.replace(SUPERADMIN_DOMAIN, '');
}

/* ============================================
   2. 登入與登出
   ============================================ */

/**
 * 使用自訂帳號登入
 */
export async function loginWithCustomAccount(account, password) {
  const email = toEmail(account);
  const cred = await signInWithEmailAndPassword(auth, email, password);
  return cred.user;
}

/**
 * 登出並清理本地狀態
 */
export async function logout() {
  try {
    await signOut(auth);
  } catch (err) {
    console.warn('[auth] signOut 失敗：', err);
  }

  AppState.destroy();
  localStorage.removeItem(STORAGE_KEYS.FAMILY_ID);
  localStorage.removeItem(STORAGE_KEYS.FAMILY_NAME);
  
  // 導向登入頁 (SPA 模式，通常在 router 處理，這裡先寫死)
  window.location.hash = '#/login';
  window.location.reload();
}

/* ============================================
   3. 監聽與權限判定
   ============================================ */

/**
 * 監聽登入狀態
 */
export function watchAuth(callback) {
  return onAuthStateChanged(auth, callback);
}

/**
 * 判斷是否為超級管理員
 */
export function isSuperAdmin(user) {
  return !!user && user.email === SUPERADMIN_EMAIL;
}

/**
 * 取得顯示名稱（帳號前綴）
 */
export function getDisplayName(user) {
  return user ? toAccount(user.email) : '';
}

/* ============================================
   4. 啟動時的身分初始化
   ============================================ */

/**
 * 在 App 啟動時，取得使用者的家庭資訊並寫入 AppState
 * 回傳 true 表示成功，false 表示未登入或無權限
 */
export async function initAuthContext() {
  return new Promise((resolve) => {
    let _resolved = false;

    const unsubscribe = watchAuth(async (user) => {
      if (_resolved) return;
      _resolved = true;
      try { unsubscribe(); } catch (e) {}

      if (!user) {
        resolve(false);
        return;
      }

      AppState.setUser(user);
      const superAdmin = isSuperAdmin(user);
      AppState.setSuperAdmin(superAdmin);

      if (superAdmin) {
        AppState.setRole(ROLES.SUPERADMIN);
        AppState.setCanInput(true);
        AppState.setDisplayName('超級管理員');
        // 超級管理員不需要家庭綁定
        resolve(true);
        return;
      }

      // 一般成員：向後端查詢家庭資訊
      try {
        const token = await user.getIdToken();
        const res = await fetch('/api/lookup-family', {
          method: 'POST',
          headers: { 
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${token}` 
          },
          body: JSON.stringify({})
        });

        if (!res.ok) throw new Error('查詢家庭失敗');
        
        const data = await res.json();
        if (data && data.familyId) {
          AppState.setFamily(data.familyId, data.familyName || '我的家庭');
          AppState.setMemberAccount(data.memberAccount);
          resolve(true);
        } else {
          resolve(false);
        }
      } catch (err) {
        console.error('[auth] 取得家庭資訊失敗：', err);
        resolve(false);
      }
    });
  });
}

/* ═══ EOF ═══
F: domain/auth.js
V: 1792900310
W: ai-system-20261024-012#26
═══ EOF ═══ */```

<!-- === FILE END: ./domain/auth.js === -->

<!-- === FILE START: ./domain/bank.js === -->

## 📄 ./domain/bank.js

```js
// ============================================
// bank.js — 銀行業務輔助（V2 重構版）
// 位置：domain/bank.js
// 說明：集中銀行餘額計算與交易生成。不包含 UI 渲染。
// ============================================

import { addBankTransaction } from '../db/write.js';
import { roundHKD } from '../ui/format.js'; // 假設未來的 format.js 有 roundHKD
import { todayISO } from '../ui/format.js';

export const BANK_TXN_TYPES = {
  IN: 'in',
  OUT: 'out',
  TRANSFER: 'transfer',
};

export const BANK_TXN_CATEGORIES = {
  CONTRIBUTION: 'contribution',
  EXPENSE: 'expense',
  INSURANCE: 'insurance',
  REIMBURSEMENT: 'reimbursement',
  MANUAL: 'manual',
};

/* ============================================
   1. 餘額計算
   ============================================ */

/**
 * 計算單一銀行帳號在特定年月的餘額
 * @param {object} bankAccount - 銀行帳號物件
 * @param {Array} transactions - 該帳號的所有交易紀錄
 * @param {string|number} targetYear 
 * @param {string|number} targetMonth 
 */
export function calcBankBalance(bankAccount, transactions, targetYear, targetMonth) {
  if (!bankAccount) return 0;

  const initial = Number(bankAccount.initialBalance) || 0;
  const initY = Number(bankAccount.initialYear) || 0;
  const initM = Number(bankAccount.initialMonth) || 0;

  // 若無初始年月，直接回傳初始餘額
  if (!initY || !initM) return initial;

  const tY = Number(targetYear);
  const tM = Number(targetMonth);

  // 若目標年月早於初始年月，直接回傳初始餘額
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
      balance -= amount; // 內部轉帳視為出帳（若需精確處理轉入帳號，需額外邏輯）
    }
  });

  return balance;
}

/**
 * 計算所有銀行帳號的總餘額
 */
export function calcTotalBankBalance(bankAccounts, allTransactions, targetYear, targetMonth) {
  let total = 0;
  (bankAccounts || []).forEach((acc) => {
    const txns = (allTransactions || []).filter((t) => t.bankId === acc.id);
    total += calcBankBalance(acc, txns, targetYear, targetMonth);
  });
  return total;
}

/* ============================================
   2. 交易生成輔助
   ============================================ */

/**
 * 為支出建立出帳交易
 */
export async function createTransactionForExpense({ bankId, memberId, amount, date, expenseId, note = '' }) {
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

/**
 * 為保險扣款建立出帳交易
 */
export async function createTransactionForInsurance({ bankId, memberId, amount, date, policyId, note = '' }) {
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

/**
 * 為家用轉入建立入帳交易
 */
export async function createTransactionForContribution({ bankId, memberId, amount, date, note = '' }) {
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

/* ═══ EOF ═══
F: domain/bank.js
V: 1792900260
W: ai-system-20261024-012#21
═══ EOF ═══ */```

<!-- === FILE END: ./domain/bank.js === -->

<!-- === FILE START: ./domain/fund.js === -->

## 📄 ./domain/fund.js

```js
// ============================================
// fund.js — 基金業務輔助（V2 重構版）
// 位置：domain/fund.js
// 說明：基金成本估算、盈虧計算與關聯保單反查。
//       將舊版散落在 Portfolio 頁面的業務邏輯抽離至此。
// ============================================

import { calcFundCostEstimate } from './insurance.js';

/**
 * 反查基金關聯的保單
 * 優先使用 fund.policyId，若無則比對保單的 fundsAllocation
 */
export function findPolicyByFund(fund, policies) {
  if (fund.policyId) {
    const p = (policies || []).find((x) => x.id === fund.policyId);
    if (p) return p;
  }
  return (policies || []).find((p) =>
    (p.fundsAllocation || []).some((a) => a.fundId === fund.id)
  ) || null;
}

/**
 * 為單一保單計算其關聯基金的「預估投入成本」
 * 回傳格式：{ [fundId]: cost }
 */
export function getCostMapForPolicy(policy, allFunds, targetYear, targetMonth) {
  const costMap = {};
  if (!policy) return costMap;

  // 找出屬於這張保單的所有保險基金
  const policyFunds = (allFunds || []).filter(
    (f) => f.type === 'insurance' && findPolicyByFund(f, [policy]) !== null
  );
  if (policyFunds.length === 0) return costMap;

  // 計算此保單的總累積成本
  const totalPolicyCost = calcFundCostEstimate(policy, targetYear, targetMonth);
  
  // 區分手動輸入成本與動態分配成本的基金
  const manualFunds = policyFunds.filter((f) => Number(f.cost) > 0);
  const manualTotal = manualFunds.reduce((s, f) => s + Number(f.cost), 0);
  manualFunds.forEach((f) => { costMap[f.id] = Number(f.cost); });

  const remainingCost = Math.max(0, totalPolicyCost - manualTotal);
  const dynamicFunds = policyFunds.filter((f) => Number(f.cost) <= 0);
  
  const dynamicTotalPct = dynamicFunds.reduce((s, f) => {
    const alloc = (policy.fundsAllocation || []).find((a) => a.fundId === f.id);
    return s + (alloc ? Number(alloc.pct) : 0);
  }, 0);

  // 依比例分配剩餘成本給動態基金
  dynamicFunds.forEach((f) => {
    const alloc = (policy.fundsAllocation || []).find((a) => a.fundId === f.id);
    const pct = alloc ? Number(alloc.pct) : 0;
    costMap[f.id] = dynamicTotalPct > 0 ? Math.round(remainingCost * (pct / dynamicTotalPct)) : 0;
  });

  return costMap;
}

/**
 * 計算單一基金的盈虧狀況
 * @param {object} fund 
 * @param {number} estimatedCost 
 */
export function calcFundPnl(fund, estimatedCost) {
  const cost = Number(estimatedCost) || 0;
  const value = Number(fund.currentValue) || 0;
  const pnl = value - cost;
  const pnlPct = cost > 0 ? ((pnl / cost) * 100).toFixed(2) : '0.00';

  return {
    cost,
    value,
    pnl,
    pnlPct: Number(pnlPct),
    isProfit: pnl >= 0,
  };
}

/* ═══ EOF ═══
F: domain/fund.js
V: 1792900300
W: ai-system-20261024-012#25
═══ EOF ═══ */```

<!-- === FILE END: ./domain/fund.js === -->

<!-- === FILE START: ./domain/insurance.js === -->

## 📄 ./domain/insurance.js

```js
// ============================================
// insurance.js — 保險計算核心（V2 重構版）
// 位置：domain/insurance.js
// 說明：保單期間、成本、進度等純計算邏輯。
//       不涉及 UI 渲染與資料庫讀寫，方便單獨測試。
// ============================================

import { isDone as isDoneStatus } from '../config/status.js';

/* ============================================
   1. 基礎屬性與期間計算
   ============================================ */

/**
 * 取得保單實質持有人（優先 policyHolderId，其次 memberId）
 */
export function getPolicyHolderId(policy) {
  if (!policy) return '';
  return policy.policyHolderId || policy.memberId || '';
}

/**
 * 取得特定「保單年度」的起訖年月
 * @param {object} policy 
 * @param {number} periodIndex - 第幾期（從 1 開始）
 */
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

/**
 * 取得指定年月落在哪一個保單年度
 * @param {object} policy 
 * @param {string|number} year 
 * @param {string|number} month 
 */
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

/* ============================================
   2. 金額計算
   ============================================ */

/**
 * 取得指定年度的年繳保費
 */
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

  // Fallback: 找最接近的設定，或回退到保單主檔的年繳金額
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

/**
 * 解析每月應繳金額（考量實際紀錄優先，其次推算）
 */
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

/**
 * 取得保單總供款額（合約上的總額）
 */
export function getPolicyTotalPremium(policy) {
  if (!policy) return 0;

  if (policy.type === 'fund_insurance') {
    const monthly = Number(policy.monthlyPremium) || 0;
    const years = Number(policy.totalPolicyYears) || 0;
    return Math.round(monthly * 12 * years);
  }

  const totalYears = Number(policy.totalPolicyYears) || 0;
  if (totalYears === 0) return Math.round(Number(policy.annualPremium) || 0);

  const periods = policy.periods || {};
  const periodKeys = Object.keys(periods).map(Number).filter((n) => !isNaN(n) && n > 0).sort((a, b) => a - b);

  let fallback = Math.round(Number(policy.annualPremium) || 0);
  if (!fallback && periodKeys.length > 0) {
    const last = periods[String(periodKeys[periodKeys.length - 1])];
    if (last && last.annualPremium) fallback = Math.round(Number(last.annualPremium));
  }

  let total = 0;
  for (let i = 1; i <= totalYears; i++) {
    const p = periods[String(i)];
    total += (p && p.annualPremium) ? Math.round(Number(p.annualPremium)) : fallback;
  }

  return total;
}

/* ============================================
   3. 狀態與進度
   ============================================ */

/**
 * 計算已繳總額（依 payments 紀錄）
 */
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

/**
 * 計算已供款期數（月）
 */
export function countCompletedPeriods(payments) {
  let count = 0;
  Object.values(payments || {}).forEach((yearData) => {
    Object.values(yearData || {}).forEach((mData) => {
      if (mData && isDoneStatus(mData.status)) count++;
    });
  });
  return count;
}

/**
 * 判斷保單是否已供滿
 */
export function isPolicyCompleted(policy) {
  if (!policy) return false;
  if (policy.isCompleted) return true;
  if (policy.type === 'fund_insurance') return false; // 基金保險通常無固定供滿期

  const total = Number(policy.totalPolicyPeriods) || 0;
  if (total <= 0) return false;

  const done = Number(policy.completedPeriods) || 0;
  return done >= total;
}

/**
 * 計算供款進度百分比
 */
export function calcProgress(policy) {
  const total = Number(policy.totalPolicyPeriods) || 0;
  const done = Number(policy.completedPeriods) || 0;
  if (total <= 0) return 0;
  return Math.min(100, Math.round((done / total) * 100));
}

/* ============================================
   4. 綜合封裝 (Enriched Policies)
   ============================================ */

/**
 * 將保單與其繳費紀錄合併，回傳包含計算結果的物件陣列
 */
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

/* ============================================
   5. 基金成本估算（供 Portfolio 使用）
   ============================================ */

/**
 * 依保單起始年月推算基金成本
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

  const paidMonths = diffMonths + 1;
  return Math.round(monthly * paidMonths);
}

/* ═══ EOF ═══
F: domain/insurance.js
V: 1792900250
W: ai-system-20261024-012#20
═══ EOF ═══ */```

<!-- === FILE END: ./domain/insurance.js === -->

<!-- === FILE START: ./domain/settlement.js === -->

## 📄 ./domain/settlement.js

```js
// ============================================
// settlement.js — 結算資料合併（V2 重構版）
// 位置：domain/settlement.js
// 說明：將「個人/家庭支出」與「保險自動扣款」合併為單一結算清單。
//       確保兩邊的資料格式一致，供結算頁面渲染。
// ============================================

import { RESERVED_IDS } from '../config/constants.js';
import { isDone as isDoneStatus, normalize as normalizeStatus } from '../config/status.js';

/* ============================================
   主入口：合併資料
   ============================================ */
export function mergeSettlementData({
  memberExpenses = [],
  insuranceRows = [],
  year,
  month,
}) {
  const rows = [];

  // 1. 處理一般支出（排除自動連結的保險支出）
  memberExpenses
    .filter((e) => !e.isAutoLinked)
    .forEach((e) => {
      rows.push(_buildPersonalRow(e, year, month));
    });

  // 2. 處理保險（包含手動建立的紀錄與連動的支出）
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
      // 有連動支出，優先使用支出紀錄的狀態
      rows.push(_buildLinkedInsuranceRow(linked, insuranceRow, year, month, policyId));
    } else if (insuranceRow) {
      // 只有保險扣款紀錄
      rows.push(_buildInsuranceRow(insuranceRow, year, month));
    }
  });

  return rows;
}

/* ============================================
   1. 一般支出
   ============================================ */
function _buildPersonalRow(e, year, month) {
  const isShared = e.memberId === RESERVED_IDS.SHARED_MEMBER;
  const statusCode = normalizeStatus(e.status);

  return {
    key: `personal-${year}-${month}-${e.memberId}-${e.id}`,
    source: 'personal',
    sourceLabel: isShared ? '🏠 家庭' : '🏷 個人',
    year,
    month,
    memberId: e.memberId || '',
    name: e.name || '（未命名）',
    amount: Number(e.amount) || 0,
    status: statusCode,
    isDone: isDoneStatus(statusCode),
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

/* ============================================
   2. 保險（有連動支出）
   ============================================ */
function _buildLinkedInsuranceRow(linked, insuranceRow, year, month, policyId) {
  const effectiveMemberId = insuranceRow?.policyHolderId
    || insuranceRow?.memberId
    || linked.memberId
    || '';

  // 優先使用保險扣款紀錄的狀態，若無則用連動支出的狀態
  const rawStatus = insuranceRow?.status || linked.status;
  const statusCode = normalizeStatus(rawStatus);

  return {
    key: `insurance-${year}-${month}-${policyId}`,
    source: 'insurance',
    sourceLabel: '🛡 保險',
    year,
    month,
    memberId: effectiveMemberId,
    name: insuranceRow?.policyName || linked.name || '（未命名保單）',
    amount: Number(linked.amount) || 0,
    status: statusCode,
    isDone: isDoneStatus(statusCode),
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

/* ============================================
   3. 保險（僅有扣款紀錄，尚未產生連動支出）
   ============================================ */
function _buildInsuranceRow(p, year, month) {
  const effectiveMemberId = p.policyHolderId || p.memberId || '';
  const statusCode = normalizeStatus(p.status);

  return {
    key: `insurance-${year}-${month}-${p.policyId}`,
    source: 'insurance',
    sourceLabel: '🛡 保險',
    year,
    month,
    memberId: effectiveMemberId,
    name: p.policyName || '（未命名保單）',
    amount: Number(p.amount) || 0,
    status: statusCode,
    isDone: isDoneStatus(statusCode),
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

/* ============================================
   輔助工具
   ============================================ */
function _extractPolicyId(id) {
  if (!id || typeof id !== 'string') return '';
  if (id.startsWith('linked_')) return id.slice(7);
  return '';
}

/* ═══ EOF ═══
F: domain/settlement.js
V: 1792900290
W: ai-system-20261024-012#24
═══ EOF ═══ */```

<!-- === FILE END: ./domain/settlement.js === -->

<!-- === FILE START: ./engine/main.js === -->

## 📄 ./engine/main.js

```js
// ============================================
// main.js — 系統啟動入口（V2 重構版）
// 位置：engine/main.js
// ============================================

import { auth } from '../config/firebase.js';
import { AppState } from '../config/state.js';
import { onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
import { initAppConfig, watchConfigChanges } from '../config/app-config.js';
import { initEntityRegistry } from '../config/entity-registry.js';
import { isSuperAdmin } from '../domain/auth.js';
import { initLayout, updateNavbar } from '../ui/layout.js';
import { initRouter } from './router.js';
import { ROUTES, ROLES } from '../config/constants.js';
import { showToast } from '../ui/toast.js';

if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('./sw.js').catch(err => console.warn('[PWA] SW 失敗', err));
  });
}

async function bootstrap() {
  AppState.init();

  // 1. 先建立骨架
  initLayout('app-root');

  onAuthStateChanged(auth, async (user) => {
    const loadingEl = document.getElementById('app-loading');
    
    if (user) {
      AppState.setUser(user);
      const superAdmin = isSuperAdmin(user);
      AppState.setSuperAdmin(superAdmin);

      // 超級管理員登入：不查家庭，不載入家庭設定，直接去管理頁
      if (superAdmin) {
        AppState.setRole(ROLES.SUPERADMIN);
        AppState.setDisplayName('超級管理員');
        updateNavbar();
        
        let initialPath = window.location.hash.replace('#', '');
        // 若超管當前不在 admin 路由，強制轉向 admin
        if (!initialPath || initialPath === ROUTES.LOGIN || initialPath === ROUTES.DASHBOARD) {
          window.location.hash = ROUTES.ADMIN;
          initialPath = ROUTES.ADMIN;
        }
        if (loadingEl) loadingEl.style.display = 'none';
        initRouter(initialPath);
        return;
      }

      // 一般成員登入：查詢家庭
      let hasFamily = false;
      try {
        const token = await user.getIdToken();
        const res = await fetch('/api/lookup-family', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json', 'Authorization': `Bearer ${token}` },
          body: JSON.stringify({})
        });
        
        if (res.ok) {
          const data = await res.json();
          if (data.familyId) {
            AppState.setFamily(data.familyId, data.familyName || '我的家庭');
            AppState.setMemberAccount(data.memberAccount);
            hasFamily = true;
          }
        } else {
          showToast(`載入家庭失敗 (${res.status})`, 'error');
        }
      } catch (err) {
        console.error('[main] 取得家庭資訊失敗：', err);
        showToast('網路異常，無法載入家庭資訊', 'error');
      }

      // 若一般成員無家庭，退回登入頁
      if (!hasFamily) {
        window.location.hash = ROUTES.LOGIN;
        if (loadingEl) loadingEl.style.display = 'none';
        initRouter(ROUTES.LOGIN);
        return;
      }

      // 初始化家庭相關配置與實體快取
      try {
        await initAppConfig();
        watchConfigChanges();
        initEntityRegistry();
      } catch (initErr) {
        console.error('[main] 設定初始化失敗：', initErr);
      }

      updateNavbar();
      
      // 一般成員決定初始路徑
      let initialPath = window.location.hash.replace('#', '');
      // 若一般成員誤入 admin 頁，強制轉向 dashboard
      if (!initialPath || initialPath === ROUTES.LOGIN || initialPath === ROUTES.ADMIN) {
        initialPath = ROUTES.DASHBOARD;
        window.location.hash = initialPath;
      }
      
      if (loadingEl) loadingEl.style.display = 'none';
      initRouter(initialPath);

    } else {
      // === 未登入 ===
      AppState.destroy();
      
      let initialPath = window.location.hash.replace('#', '');
      if (initialPath !== ROUTES.LOGIN) {
        initialPath = ROUTES.LOGIN;
        window.location.hash = initialPath;
      }
      if (loadingEl) loadingEl.style.display = 'none';
      initRouter(initialPath);
    }
  });
}

bootstrap().catch(err => {
  console.error('[main] 系統啟動失敗：', err);
  document.body.innerHTML = `<div style="padding:40px; text-align:center; color:var(--accent-danger);"><h2>系統啟動失敗</h2><p>${err.message}</p></div>`;
});

/* ═══ EOF ═══
F: engine/main.js
V: 1792901800
W: webapp-20261025-001#28
═══ */```

<!-- === FILE END: ./engine/main.js === -->

<!-- === FILE START: ./engine/router.js === -->

## 📄 ./engine/router.js

```js
// ============================================
// router.js — SPA 前端路由（V2 重構版）
// 位置：engine/router.js
// 說明：負責頁面載入、路由守衛（權限/登入判定）與頁面生命週期管理。
// ============================================

import { ROUTES, ROLES } from '../config/constants.js';
import { AppState } from '../config/state.js';
import { setContent, setPageTitle, updateSidebarActive } from '../ui/layout.js';

const ROUTES_MAP = {
  [ROUTES.DASHBOARD]:    { module: () => import('../pages/dashboard.js'),    title: '總覽儀表板', auth: true, family: true },
  [ROUTES.INPUT_CENTER]: { module: () => import('../pages/input-center.js'), title: '綜合輸入中心', auth: true, family: true },
  [ROUTES.CODE_LIST]:    { module: () => import('../pages/code-list.js'),    title: '編號對照清單', auth: true, family: true },
  [ROUTES.INSURANCE]:    { module: () => import('../pages/insurance.js'),    title: '保險清單表', auth: true, family: true },
  [ROUTES.PORTFOLIO]:    { module: () => import('../pages/portfolio.js'),    title: '基金投資表', auth: true, family: true },
  [ROUTES.BANK]:         { module: () => import('../pages/bank.js'),         title: '銀行交易', auth: true, family: true },
  [ROUTES.SETTLEMENTS]:  { module: () => import('../pages/settlements.js'),  title: '結算清單', auth: true, family: true },
  [ROUTES.ANNUAL_REPORT]:{ module: () => import('../pages/annual-report.js'),title: '年度報表', auth: true, family: true },
  [ROUTES.MEMBER_REPORT]:{ module: () => import('../pages/member-report.js'),title: '成員收支明細', auth: true, family: true },
  [ROUTES.SETTINGS]:     { module: () => import('../pages/settings.js'),     title: '系統設定', auth: true, family: false },
  [ROUTES.ADMIN]:        { module: () => import('../admin/admin.js'),        title: '平台管理', auth: true, family: false, role: ROLES.SUPERADMIN },
  [ROUTES.LOGIN]:        { module: () => import('../pages/login.js'),        title: '登入', auth: false, family: false },
};

let _currentPage = null;
let _isInitialized = false;

/**
 * 初始化路由
 * @param {string|boolean} initialPath - 若傳入字串，則強制執行該路徑；若傳 true，則依當前 hash 執行。
 */
export function initRouter(initialPath = null) {
  if (!_isInitialized) {
    window.addEventListener('hashchange', () => _handleRoute());
    _isInitialized = true;
  }
  
  if (typeof initialPath === 'string') {
    _handleRoute(initialPath);
  } else if (!window.location.hash) {
    window.location.hash = ROUTES.DASHBOARD;
  } else {
    _handleRoute();
  }
}

/**
 * 處理路由切換
 * @param {string|null} targetPath - 若為 null，則從 window.location.hash 取得。
 */
async function _handleRoute(targetPath = null) {
  // 確保 targetPath 是字串，防止 Event 物件傳入
  let path = typeof targetPath === 'string' ? targetPath : null;
  
  if (!path) {
    path = window.location.hash.replace('#', '') || ROUTES.DASHBOARD;
  }

  const routeConfig = ROUTES_MAP[path];
  
  if (!routeConfig) {
    _render404(path);
    return;
  }

  // ============================================
  // 路由守衛 (Route Guards)
  // ============================================
  
  const isLoggedIn = !!AppState.currentUser;
  const isSuper = AppState.getRole() === ROLES.SUPERADMIN;
  const hasFamily = !!AppState.getFamilyId();

  // 1. 若目標是登入頁，但已登入：直接踢回對應的首頁
  if (path === ROUTES.LOGIN && isLoggedIn) {
    window.location.hash = isSuper ? ROUTES.ADMIN : ROUTES.DASHBOARD;
    return;
  }

  // 2. 需要登入，但未登入
  if (routeConfig.auth && !isLoggedIn) {
    window.location.hash = ROUTES.LOGIN;
    return;
  }

  // 3. 需要特定角色 (如超級管理員)，但權限不符
  if (routeConfig.role && AppState.getRole() !== routeConfig.role) {
    // 不是超管卻想進 admin，踢回 dashboard
    window.location.hash = ROUTES.DASHBOARD;
    return;
  }

  // 4. 需要家庭，但沒有家庭 (且不是超管)
  if (routeConfig.family && !hasFamily) {
    if (isSuper) {
      // 超管沒有家庭是正常的，若超管誤入家庭頁面，導向管理頁
      window.location.hash = ROUTES.ADMIN;
    } else {
      // 一般成員沒有家庭，退回登入
      window.location.hash = ROUTES.LOGIN;
    }
    return;
  }

  // ============================================
  // 執行頁面切換
  // ============================================

  setPageTitle(routeConfig.title);
  updateSidebarActive(path);

  // 銷毀上一個頁面
  if (_currentPage && typeof _currentPage.destroy === 'function') {
    try { _currentPage.destroy(); } catch (e) { console.error('[router] 頁面銷毀失敗：', e); }
    _currentPage = null;
  }

  // 顯示載入中 (登入頁除外，避免閃爍)
  if (path !== ROUTES.LOGIN) {
    setContent('<div style="text-align:center; padding:40px; color:var(--text-muted);">載入中…</div>');
  }

  // 動態載入模組並渲染
  try {
    const module = await routeConfig.module();
    const page = module.default;

    if (!page || typeof page.render !== 'function') {
      throw new Error(`頁面模組 ${path} 未匯出 render 函數`);
    }

    _currentPage = page;
    await page.render();

  } catch (err) {
    console.error(`[router] 載入頁面 ${path} 失敗：`, err);
    setContent(`
      <div class="glass-card" style="text-align:center; padding:40px; margin:20px;">
        <h2 style="color:var(--accent-danger); margin-bottom:12px;">載入失敗</h2>
        <p class="text-muted">${err.message}</p>
        <p style="font-size:12px; margin-top:8px;">請檢查 Console 或重新整理頁面。</p>
        <button type="button" class="btn btn-ghost" style="margin-top:16px;" onclick="window.location.reload()">重新整理</button>
      </div>
    `);
  }
}

function _render404(path) {
  setContent(`
    <div class="glass-card" style="text-align:center; padding:40px; margin:20px;">
      <h1 style="font-size:48px; color:var(--text-muted); margin-bottom:16px;">404</h1>
      <h2 style="color:var(--text-primary); margin-bottom:12px;">找不到頁面</h2>
      <p class="text-muted">路徑 ${path} 不存在。</p>
      <a href="#${ROUTES.DASHBOARD}" class="btn btn-primary" style="margin-top:20px; display:inline-flex;">返回首頁</a>
    </div>
  `);
}

/* ═══ EOF ═══
F: engine/router.js
V: 1792901810
W: webapp-20261025-001#29
═══ */```

<!-- === FILE END: ./engine/router.js === -->

<!-- === FILE START: ./functions/api/[[path]].js === -->

## 📄 ./functions/api/[[path]].js

```js
// ============================================
// [[path]].js — API 路由總管（V2 重構版）
// 位置：api/[[path]].js
// 說明：Cloudflare Pages Functions 的 Catch-all 路由。
//       負責將請求分派給對應的 handler。
// ============================================

import { initConfig, jsonResponse } from './config.js';
import { 
  handleSummary, 
  handleAnnualSummary, 
  handleSettlementsYear,
  handleLookupFamily,
  handleFamilySettings,
  handleEntityPost,
  handleInsuranceSync,
  handleAdminFamilies,
  handleAdminInitFamily,
  handlePlatformSettings,
  handlePlatformDefaults,
  handleBankAccounts,
  handleBankTransactions
} from './handlers.js';

/* ============================================
   1. CORS 預檢處理
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
   2. GET 路由
   ============================================ */
export async function onRequestGet(context) {
  initConfig(context.env);
  const url = new URL(context.request.url);
  const path = url.pathname;

  switch (path) {
    case '/api/summary': return handleSummary(context);
    case '/api/annual-summary': return handleAnnualSummary(context);
    case '/api/settlements-year': return handleSettlementsYear(context);
    case '/api/family-settings': return handleFamilySettings(context);
    
    case '/api/admin-families': return handleAdminFamilies(context);
    case '/api/platform-settings': return handlePlatformSettings(context);
    case '/api/platform-defaults': return handlePlatformDefaults(context);
    
    case '/api/bank-accounts': return handleBankAccounts(context);
    case '/api/bank-transactions': return handleBankTransactions(context);
    
    default:
      return jsonResponse({ ok: false, error: 'NOT_FOUND', message: `未知的 GET API：${path}` }, 404);
  }
}

/* ============================================
   3. POST 路由
   ============================================ */
export async function onRequestPost(context) {
  initConfig(context.env);
  const url = new URL(context.request.url);
  const path = url.pathname;

  switch (path) {
    case '/api/lookup-family': return handleLookupFamily(context);
    case '/api/entity-create': return handleEntityPost(context);
    case '/api/insurance-sync': return handleInsuranceSync(context);
    
    case '/api/family-settings': return handleFamilySettings(context);
    
    case '/api/admin-families': return handleAdminFamilies(context);
    case '/api/admin-init-family': return handleAdminInitFamily(context);
    case '/api/platform-settings': return handlePlatformSettings(context);
    case '/api/platform-defaults': return handlePlatformDefaults(context);
    
    case '/api/bank-accounts': return handleBankAccounts(context);
    case '/api/bank-transactions': return handleBankTransactions(context);

    default:
      return jsonResponse({ ok: false, error: 'NOT_FOUND', message: `未知的 POST API：${path}` }, 404);
  }
}

/* ═══ EOF ═══
F: api/[[path]].js
V: 1792900350
W: ai-system-20261024-012#30
═══ EOF ═══ */```

<!-- === FILE END: ./functions/api/[[path]].js === -->

<!-- === FILE START: ./functions/api/config.js === -->

## 📄 ./functions/api/config.js

```js
// ============================================
// config.js — 後端 Firebase 設定與 REST 封裝（V2 重構版）
// 位置：api/config.js
// 說明：集中處理環境變數與 Firebase REST API 請求。
//       支援 DB_TARGET 切換 (old / new)。
// ============================================

/* ============================================
   1. 環境設定
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

// 使用 export let 以便在 initConfig 時動態更新
export let FIREBASE_DB_URL  = _DB_CONFIGS.new.url;
export let FIREBASE_API_KEY = _DB_CONFIGS.new.apiKey;

export const SUPERADMIN_EMAIL = 'superadmin@familyfin.local';
export const IDENTITY_TOOLKIT_URL = 'https://identitytoolkit.googleapis.com/v1';

/**
 * 初始化設定（由 handler 在每個請求開頭呼叫）
 */
export function initConfig(env) {
  const target = (env && env.DB_TARGET) || 'new';
  const cfg = _DB_CONFIGS[target] || _DB_CONFIGS.new;
  FIREBASE_DB_URL  = cfg.url;
  FIREBASE_API_KEY = cfg.apiKey;
}

export function getApiKey() { return FIREBASE_API_KEY; }
export function getDbUrl()  { return FIREBASE_DB_URL; }

/* ============================================
   2. Firebase REST API 封裝
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
   3. 回應工具
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

/* ═══ EOF ═══
F: api/config.js
V: 1792900330
W: ai-system-20261024-012#28
═══ EOF ═══ */```

<!-- === FILE END: ./functions/api/config.js === -->

<!-- === FILE START: ./functions/api/handlers-admin.js === -->

## 📄 ./functions/api/handlers-admin.js

```js
// ============================================
// handlers-admin.js — 管理員與平台設定（V2 重構版）
// 位置：api/handlers-admin.js
// 說明：包含家庭管理、平台預設資料（選項、狀態、UI常數）的 CRUD。
// ============================================

import { dbGet, dbPut, dbPush, dbDelete } from './config.js';
import {
  authenticate, errorResponse, successResponse, handleError,
  requireFields, extractToken, verifyFamilyAccess
} from './helpers.js';

/* ============================================
   1. 管理員：家庭列表 (admin-families)
   ============================================ */
export async function handleAdminFamilies(context) {
  try {
    const { request } = context;
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (request.method === 'GET') {
      const list = await dbGet('platform/families', token);
      const families = Object.entries(list || {}).map(([uid, data]) => ({ uid, ...data }));
      families.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0));
      return successResponse({ families });
    }

    if (request.method === 'POST') {
      const body = await request.json();
      const { action, uid, name, email } = body || {};

      if (action === 'add') {
        const missing = requireFields(body, ['uid', 'name']);
        if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid 或 name');
        const ok = await dbPut(`platform/families/${uid}`, { name: String(name).trim(), ownerEmail: email ? String(email).trim() : '', createdAt: Date.now() }, token);
        if (!ok) return errorResponse('INTERNAL', 'Firebase 寫入失敗');
        return successResponse();
      }

      if (action === 'remove') {
        const missing = requireFields(body, ['uid']);
        if (missing) return errorResponse('MISSING_FIELDS', '缺少 uid');

        // 清理帳號索引
        const accounts = await dbGet(`platform/families/${uid}/memberAccounts`, token);
        if (accounts) {
          for (const [memberUid, acc] of Object.entries(accounts)) {
            await dbDelete(`platform/uid_index/${memberUid}`, token);
            if (acc && acc.account) await dbDelete(`platform/email_index/${acc.account}`, token);
          }
        }
        
        await dbDelete(`platform/families/${uid}`, token);
        await dbDelete(`families/${uid}`, token);
        return successResponse({ deleted: true });
      }
    }
    return errorResponse('BAD_REQUEST', '未知的請求');
  } catch (err) { return handleError(err); }
}

/* ============================================
   2. 管理員：初始化家庭 (admin-init-family)
   ============================================ */
export async function handleAdminInitFamily(context) {
  try {
    const { request } = context;
    const body = await request.json();
    const { uid } = body || {};
    if (!uid) return errorResponse('MISSING_FIELDS', '缺少 uid');

    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${uid}`;
    const existing = await dbGet(`${basePath}/members`, token);
    if (existing && Object.keys(existing).length > 0) {
      return successResponse({ skipped: true, message: '此家庭已有資料' });
    }

    // 載入平台預設（若無則留空）
    const defaults = {
      members: await dbGet('platform/defaults/members', token) || {},
      categories: await dbGet('platform/defaults/expense_categories', token) || {},
      items: await dbGet('platform/defaults/expense_items', token) || {},
      payments: await dbGet('platform/defaults/payment_methods', token) || {},
      companies: await dbGet('platform/defaults/insurance_companies', token) || {},
      statuses: await dbGet('platform/defaults/statuses', token) || {},
    };

    const now = Date.now();
    const results = {};

    // 寫入基礎資料
    results.members = Object.keys(defaults.members).length > 0 ? await dbPut(`${basePath}/members`, defaults.members, token) : true;
    results.categories = Object.keys(defaults.categories).length > 0 ? await dbPut(`${basePath}/expense_categories`, defaults.categories, token) : true;
    results.items = Object.keys(defaults.items).length > 0 ? await dbPut(`${basePath}/expense_items`, defaults.items, token) : true;
    results.payments = Object.keys(defaults.payments).length > 0 ? await dbPut(`${basePath}/payment_methods`, defaults.payments, token) : true;
    results.companies = Object.keys(defaults.companies).length > 0 ? await dbPut(`${basePath}/insurance_companies`, defaults.companies, token) : true;
    results.statuses = Object.keys(defaults.statuses).length > 0 ? await dbPut(`${basePath}/statuses`, defaults.statuses, token) : true;

    return successResponse({ created: true, results });
  } catch (err) { return handleError(err); }
}

/* ============================================
   3. 平台設定 (platform-settings)
   ============================================ */
export async function handlePlatformSettings(context) {
  try {
    const { request } = context;
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (request.method === 'GET') {
      const data = await dbGet('platform/defaults/ui_constants', token);
      return successResponse({ data: data || {} });
    }

    if (request.method === 'POST') {
      const body = await request.json();
      if (body.action !== 'update') return errorResponse('BAD_REQUEST', '未知的 action');
      
      const clean = {
        nameMaxLenDesktop: _toInt(body.data.nameMaxLenDesktop, 12),
        nameMaxLenMobile:  _toInt(body.data.nameMaxLenMobile, 6),
        toastDuration:     _toInt(body.data.toastDuration, 2000),
      };
      await dbPut('platform/defaults/ui_constants', clean, token);
      return successResponse({ data: clean });
    }
  } catch (err) { return handleError(err); }
}

/* ============================================
   4. 平台預設資料 (platform-defaults)
   ============================================ */
const PLATFORM_RESOURCES = {
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

export async function handlePlatformDefaults(context) {
  try {
    const { request } = context;
    const url = new URL(request.url);
    const auth = await authenticate(request, { needSuperAdmin: true });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (request.method === 'GET') {
      const resource = url.searchParams.get('resource');
      const config = PLATFORM_RESOURCES[resource];
      if (!config) return errorResponse('BAD_REQUEST', `未知的 resource：${resource}`);

      const data = await dbGet(`platform/defaults/${config.path}`, token);
      if (config.type === 'object') {
        return successResponse({ resource, type: 'object', data: data || {} });
      }
      const list = Object.entries(data || {}).map(([id, x]) => ({ id, ...x })).sort((a, b) => (a.order || 0) - (b.order || 0));
      return successResponse({ resource, type: 'list', list });
    }

    if (request.method === 'POST') {
      const body = await request.json();
      const { action, resource, id, data } = body || {};
      const config = PLATFORM_RESOURCES[resource];
      if (!config) return errorResponse('BAD_REQUEST', `未知的 resource：${resource}`);

      const basePath = `platform/defaults/${config.path}`;

      if (action === 'put' && config.type === 'list') {
        let savedId = id;
        if (id) await dbPut(`${basePath}/${id}`, data, token);
        else savedId = await dbPush(basePath, { ...data, createdAt: Date.now() }, token);
        return successResponse({ id: savedId });
      }

      if (action === 'delete' && config.type === 'list') {
        await dbDelete(`${basePath}/${id}`, token);
        return successResponse({ deletedId: id });
      }

      if (action === 'set' && config.type === 'object') {
        await dbPut(basePath, data, token);
        return successResponse({ data });
      }
      return errorResponse('BAD_REQUEST', '未知的 action 或 resource 類型不符');
    }
  } catch (err) { return handleError(err); }
}

function _toInt(v, fallback) {
  const n = Number(v);
  return (isNaN(n) || n < 0) ? fallback : Math.floor(n);
}

/* ═══ EOF ═══
F: api/handlers-admin.js
V: 1792900390
W: ai-system-20261024-012#34
═══ EOF ═══ */```

<!-- === FILE END: ./functions/api/handlers-admin.js === -->

<!-- === FILE START: ./functions/api/handlers-core.js === -->

## 📄 ./functions/api/handlers-core.js

```js
// ============================================
// handlers-core.js — 核心業務處理（V2 重構版）
// 位置：api/handlers-core.js
// 說明：包含實體 CRUD、銀行、保險同步等核心邏輯。
// ============================================

import { dbGet, dbPut, dbPatch, dbPush, dbDelete } from './config.js';
import {
  authenticate, errorResponse, successResponse, handleError,
  requireFields, roundInt, extractToken, verifyFamilyAccess
} from './helpers.js';

/* ============================================
   1. 查詢家庭 (lookup-family)
   ============================================ */
export async function handleLookupFamily(context) {
  try {
    const { request } = context;
    const auth = await authenticate(request);
    if (auth instanceof Response) return auth;
    const { user, token } = auth;

    const familyId = await dbGet(`platform/uid_index/${user.localId}`, token);
    if (!familyId || typeof familyId !== 'string') {
      return errorResponse('NOT_FOUND', '此帳號不屬於任何家庭，請聯繫管理員');
    }

    let familyName = '我的家庭';
    const snap = await dbGet(`platform/families/${familyId}`, token);
    if (snap?.name) familyName = snap.name;

    const memberAccount = await dbGet(`platform/families/${familyId}/memberAccounts/${user.localId}`, token);

    return successResponse({
      familyId,
      familyName,
      memberAccount: memberAccount || {
        email: user.email || '',
        displayName: '成員',
        role: 'member',
        canInput: true,
      }
    });
  } catch (err) { return handleError(err); }
}

/* ============================================
   2. 家庭設定 (family-settings)
   ============================================ */
export async function handleFamilySettings(context) {
  try {
    const { request } = context;
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const auth = await authenticate(request, { needFamily: true, body: { familyId } });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (request.method === 'GET') {
      const basePath = `families/${familyId}`;
      const [statuses, options, yearRange, uiConstants] = await Promise.all([
        dbGet(`${basePath}/statuses`, token),
        dbGet(`${basePath}/settings/options`, token),
        dbGet(`${basePath}/settings/year_range`, token),
        dbGet(`${basePath}/settings/ui_constants`, token),
      ]);
      const statusList = Object.entries(statuses || {}).map(([id, s]) => ({ id, ...s })).sort((a, b) => (a.order || 0) - (b.order || 0));

      return successResponse({
        familyId,
        statuses: statusList,
        options: options || null,
        yearRange: yearRange || null,
        uiConstants: uiConstants || null,
      });
    }

    if (request.method === 'POST') {
      const body = await request.json();
      const { action, id, data } = body || {};
      const basePath = `families/${familyId}`;

      if (action === 'update') {
        const results = {};
        if (data.options !== undefined) results.options = await dbPut(`${basePath}/settings/options`, data.options, token);
        if (data.yearRange !== undefined) results.yearRange = await dbPut(`${basePath}/settings/year_range`, data.yearRange, token);
        if (data.uiConstants !== undefined) results.uiConstants = await dbPut(`${basePath}/settings/ui_constants`, data.uiConstants, token);
        return successResponse({ results });
      }
      
      if (action === 'put-status') {
        const clean = {
          name: String(data.name).trim(),
          category: ['personal', 'fixed', 'insurance'].includes(data.category) ? data.category : 'personal',
          isDone: !!data.isDone,
          order: Number(data.order) || 0,
          createdAt: data.createdAt || Date.now(),
        };
        let savedId = id;
        if (id) await dbPut(`${basePath}/statuses/${id}`, clean, token);
        else savedId = await dbPush(`${basePath}/statuses`, clean, token);
        return successResponse({ id: savedId, data: clean });
      }

      if (action === 'delete-status') {
        await dbDelete(`${basePath}/statuses/${id}`, token);
        return successResponse({ deletedId: id });
      }
    }
    return errorResponse('BAD_REQUEST', '未知的請求');
  } catch (err) { return handleError(err); }
}

/* ============================================
   3. 統一實體 CRUD (entity-create)
   ============================================ */
const ENTITY_PATH = {
  member: 'members', bank: 'banks', bankAccount: 'bank_accounts',
  category: 'expense_categories', item: 'expense_items', payment: 'payment_methods',
  company: 'insurance_companies', policy: 'insurance_policies', fund: 'funds',
};

export async function handleEntityPost(context) {
  try {
    const { request } = context;
    const body = await request.json();
    const { familyId, action, entityType, id, code, name, extra } = body;

    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    // 特殊處理：expense 路徑含年 / 月 / 成員三層，非 ENTITY_PATH 單層列表
    if (entityType === 'expense') {
      if (action !== 'update') {
        return errorResponse('INVALID_BODY', 'expense 僅支援 update（新增請走 db/write 的 addExpense）');
      }
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');

      const year = String(extra?.year || '');
      const month = String(extra?.month || '').padStart(2, '0');
      const memberId = String(extra?.memberId || '');

      if (!year || !month || month === '00' || !memberId) {
        return errorResponse('MISSING_FIELDS', 'expense 更新需 year / month / memberId');
      }
      if (!/^\d{4}$/.test(year)) {
        return errorResponse('INVALID_BODY', 'year 需為四位數字');
      }
      if (!/^(0[1-9]|1[0-2])$/.test(month)) {
        return errorResponse('INVALID_BODY', 'month 需為 01~12');
      }

      const expensePath = `families/${familyId}/expenses/${year}/${month}/member_expenses/${memberId}/${id}`;
      const existing = await dbGet(expensePath, token);
      if (!existing) return errorResponse('NOT_FOUND', `找不到支出：${id}`);

      const patch = {};
      if (extra.status !== undefined) patch.status = extra.status;
      if (extra.name !== undefined) patch.name = String(extra.name);
      if (extra.amount !== undefined) patch.amount = roundInt(extra.amount);
      if (extra.date !== undefined) patch.date = String(extra.date);
      if (!Object.keys(patch).length) {
        return errorResponse('INVALID_BODY', '無可更新欄位');
      }
      patch.updatedAt = Date.now();

      await dbPatch(expensePath, patch, token);
      return successResponse({ id });
    }

    if (!entityType || !ENTITY_PATH[entityType]) return errorResponse('INVALID_BODY', `未知的 entityType：${entityType}`);
    const listPath = `families/${familyId}/${ENTITY_PATH[entityType]}`;

    if (action === 'create') {
      if (!name) return errorResponse('MISSING_FIELDS', '缺少 name');
      if (!code) return errorResponse('MISSING_FIELDS', '缺少 code');
      const list = (await dbGet(listPath, token)) || {};
      
      if (Object.values(list).some((v) => v.code === code)) return errorResponse('CODE_DUPLICATE', `編號已存在：${code}`, 409);
      if (Object.values(list).some((v) => v.name === name)) return errorResponse('NAME_DUPLICATE', `名稱重複：${name}`, 409);

      const payload = { code, name, ...extra, createdAt: Date.now() };
      const newId = await dbPush(listPath, payload, token);
      return successResponse({ id: newId, code });
    }

    if (action === 'update') {
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');
      const itemPath = `${listPath}/${id}`;
      const existing = await dbGet(itemPath, token);
      if (!existing) return errorResponse('NOT_FOUND', `找不到：${id}`);

      if (name) {
        const list = (await dbGet(listPath, token)) || {};
        if (Object.entries(list).some(([k, v]) => k !== id && v.name === name)) {
          return errorResponse('NAME_DUPLICATE', `名稱重複：${name}`, 409);
        }
      }
      const payload = { ...(name !== undefined ? { name } : {}), ...extra, updatedAt: Date.now() };
      await dbPatch(itemPath, payload, token);
      return successResponse({ id });
    }

    if (action === 'delete') {
      if (!id) return errorResponse('MISSING_FIELDS', '缺少 id');
      await dbDelete(`${listPath}/${id}`, token);
      return successResponse({ id });
    }

    return errorResponse('INVALID_BODY', `未知的 action：${action}`);
  } catch (err) { return handleError(err); }
}

/* ============================================
   4. 銀行與交易
   ============================================ */
export async function handleBankAccounts(context) {
  try {
    const { request } = context;
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const auth = await authenticate(request, { needFamily: true, body: { familyId } });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}/bank_accounts`;
    if (request.method === 'GET') {
      const data = await dbGet(basePath, token) || {};
      const list = Object.entries(data).map(([id, x]) => ({ id, ...x })).sort((a, b) => (a.order || 0) - (b.order || 0));
      return successResponse({ accounts: list });
    }

    const body = await request.json();
    const { action, id, data } = body;
    if (action === 'create') {
      const clean = { name: String(data.name).trim(), type: data.type || 'family', initialBalance: roundInt(data.initialBalance), initialYear: String(data.initialYear), initialMonth: String(data.initialMonth), order: Number(data.order) || 0, createdAt: Date.now() };
      const newId = await dbPush(basePath, clean, token);
      return successResponse({ id: newId, data: clean });
    }
    if (action === 'update') {
      await dbPatch(`${basePath}/${id}`, data, token);
      return successResponse({ id });
    }
    if (action === 'remove') {
      await dbDelete(`${basePath}/${id}`, token);
      return successResponse({ removedId: id });
    }
    return errorResponse('BAD_REQUEST', '未知 action');
  } catch (err) { return handleError(err); }
}

export async function handleBankTransactions(context) {
  try {
    const { request } = context;
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const bankId = url.searchParams.get('bankId');
    const action = url.searchParams.get('action') || 'list';

    const auth = await authenticate(request, { needFamily: true, body: { familyId } });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    if (request.method === 'GET') {
      if (action === 'listAll') {
        const allBanks = await dbGet(`families/${familyId}/bank_accounts`, token) || {};
        const flat = [];
        Object.entries(allBanks).forEach(([bid, bd]) => {
          Object.entries(bd.transactions || {}).forEach(([tid, txn]) => flat.push({ id: tid, bankId: bid, bankName: bd.name || '', ...txn }));
        });
        return successResponse({ transactions: flat });
      }
      const data = await dbGet(`families/${familyId}/bank_accounts/${bankId}/transactions`, token) || {};
      const list = Object.entries(data).map(([id, x]) => ({ id, ...x }));
      return successResponse({ transactions: list });
    }

    const body = await request.json();
    const { txnId, data } = body;
    const basePath = `families/${familyId}/bank_accounts/${bankId}/transactions`;

    if (body.action === 'create') {
      const newId = await dbPush(basePath, { ...data, amount: roundInt(data.amount), createdAt: Date.now() }, token);
      return successResponse({ id: newId });
    }
    if (body.action === 'update') {
      await dbPatch(`${basePath}/${txnId}`, { ...data, updatedAt: Date.now() }, token);
      return successResponse({ txnId });
    }
    if (body.action === 'remove') {
      await dbDelete(`${basePath}/${txnId}`, token);
      return successResponse({ removedId: txnId });
    }
    return errorResponse('BAD_REQUEST', '未知 action');
  } catch (err) { return handleError(err); }
}

/* ============================================
   5. 保險同步
   ============================================ */
export async function handleInsuranceSync(context) {
  try {
    const { request } = context;
    const body = await request.json();
    const { action, familyId, policyId, memberId, policyName, monthlyAverage, year, month, bankId, txnId, paymentMode, advanceHolderId } = body;
    
    const auth = await authenticate(request, { needFamily: true, body });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}`;
    const linkedKey = `linked_${policyId}`;
    const expensePath = `${basePath}/expenses/${year}/${month}/member_expenses/${memberId}/${linkedKey}`;
    const paymentPath = `${basePath}/insurance_payments/${policyId}/${year}/${month}`;
    const amount = roundInt(monthlyAverage);

    if (action === 'delete') {
      await dbDelete(expensePath, token);
      await dbDelete(paymentPath, token);
      if (paymentMode === 'advance' && advanceHolderId) {
        await _refundAdvance(basePath, policyId, advanceHolderId, amount, year, month, token);
      }
      await _reverseFundAccumulation(basePath, policyId, year, month, token);
      return successResponse({ deleted: true });
    }

    const statuses = await dbGet(`${basePath}/statuses`, token) || {};
    const firstCategoryId = _getFirstCategoryId(await dbGet(`${basePath}/expense_categories`, token));
    const resolvedExpenseStatus = _findDoneStatus(statuses, 'personal') || '已還款';
    const resolvedPaymentStatus = _findDoneStatus(statuses, 'insurance') || '已扣款';

    await dbPut(expensePath, {
      name: `${policyName} (平攤)`, amount, status: resolvedExpenseStatus, date: '',
      categoryId: firstCategoryId, bankId: bankId || '', txnId: txnId || '',
      isAutoLinked: true, policyId, createdAt: Date.now(),
    }, token);

    await dbPut(paymentPath, {
      status: resolvedPaymentStatus, amount, date: new Date().toISOString().slice(0, 10),
      bankId: bankId || '', txnId: txnId || '', paymentMode: paymentMode || 'direct',
    }, token);

    if (paymentMode === 'advance' && advanceHolderId) {
      await _deductAdvance(basePath, policyId, advanceHolderId, amount, year, month, token);
    }
    await _accumulateFunds(basePath, policyId, year, month, token);

    return successResponse({ synced: true });
  } catch (err) { return handleError(err); }
}

/* --- 內部輔助：基金與代墊 --- */
async function _accumulateFunds(basePath, policyId, year, month, token) {
  const policy = await dbGet(`${basePath}/insurance_policies/${policyId}`, token);
  if (!policy || policy.type !== 'fund_insurance' || !policy.fundsAllocation) return;
  const monthlyPremium = roundInt(policy.monthlyPremium);
  for (const a of policy.fundsAllocation) {
    if (!a.fundId) continue;
    const addCost = Math.round(monthlyPremium * (Number(a.pct) || 0) / 100);
    const snapPath = `${basePath}/funds/${a.fundId}/snapshots/${year}/${String(month).padStart(2, '0')}`;
    const snap = await dbGet(snapPath, token) || {};
    await dbPatch(snapPath, { cumulativeCost: roundInt(snap.cumulativeCost) + addCost, updatedAt: Date.now() }, token);
  }
}
async function _reverseFundAccumulation(basePath, policyId, year, month, token) {
  const policy = await dbGet(`${basePath}/insurance_policies/${policyId}`, token);
  if (!policy || policy.type !== 'fund_insurance' || !policy.fundsAllocation) return;
  const monthlyPremium = roundInt(policy.monthlyPremium);
  for (const a of policy.fundsAllocation) {
    if (!a.fundId) continue;
    const subCost = Math.round(monthlyPremium * (Number(a.pct) || 0) / 100);
    const snapPath = `${basePath}/funds/${a.fundId}/snapshots/${year}/${String(month).padStart(2, '0')}`;
    const snap = await dbGet(snapPath, token);
    if (snap) await dbPatch(snapPath, { cumulativeCost: Math.max(0, roundInt(snap.cumulativeCost) - subCost) }, token);
  }
}
async function _deductAdvance(basePath, policyId, holderId, amount, year, month, token) {
  const advances = await dbGet(`${basePath}/member_advances/${holderId}`, token);
  if (!advances) return;
  const monthKey = `${year}-${String(month).padStart(2, '0')}`;
  for (const [id, adv] of Object.entries(advances)) {
    if (adv.policyId !== policyId) continue;
    const paid = { ...(adv.paidMonths || {}) };
    if (paid[monthKey]) return;
    paid[monthKey] = true;
    await dbPatch(`${basePath}/member_advances/${holderId}/${id}`, { remainingAmount: Math.max(0, roundInt(adv.remainingAmount) - amount), paidMonths: paid, updatedAt: Date.now() }, token);
    return;
  }
}
async function _refundAdvance(basePath, policyId, holderId, amount, year, month, token) {
  const advances = await dbGet(`${basePath}/member_advances/${holderId}`, token);
  if (!advances) return;
  const monthKey = `${year}-${String(month).padStart(2, '0')}`;
  for (const [id, adv] of Object.entries(advances)) {
    if (adv.policyId !== policyId) continue;
    const paid = { ...(adv.paidMonths || {}) };
    if (!paid[monthKey]) return;
    delete paid[monthKey];
    await dbPatch(`${basePath}/member_advances/${holderId}/${id}`, { remainingAmount: Math.min(roundInt(adv.totalAmount), roundInt(adv.remainingAmount) + amount), paidMonths: paid, updatedAt: Date.now() }, token);
    return;
  }
}
function _getFirstCategoryId(categories) {
  const list = Object.entries(categories || {}).map(([id, c]) => ({ id, ...c })).sort((a, b) => (a.order || 0) - (b.order || 0));
  return list.length > 0 ? list[0].id : '';
}
function _findDoneStatus(statuses, category) {
  const list = Object.entries(statuses).map(([id, s]) => ({ id, ...s })).filter((s) => s.category === category && s.isDone);
  return list.length > 0 ? list[0].name : null;
}

/* ═══ EOF ═══
F: api/handlers-core.js
V: 1792901200
W: webapp-20261025-001#1
═══ */```

<!-- === FILE END: ./functions/api/handlers-core.js === -->

<!-- === FILE START: ./functions/api/handlers-report.js === -->

## 📄 ./functions/api/handlers-report.js

```js
// ============================================
// handlers-report.js — 報表與統計處理（V2 重構版）
// 位置：api/handlers-report.js
// 說明：包含總覽儀表板、年度報表、與結算清單的資料聚合。
// ============================================

import { dbGet } from './config.js';
import {
  authenticate, errorResponse, successResponse, handleError, roundInt
} from './helpers.js';

/* ============================================
   1. 單月總覽 (summary)
   ============================================ */
export async function handleSummary(context) {
  try {
    const { request } = context;
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const year = url.searchParams.get('year');
    const month = url.searchParams.get('month');
    
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');

    const auth = await authenticate(request, { needFamily: true, body: { familyId } });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}`;
    
    // 平行讀取所需資料
    const [
      members, policies, expenses, income, funds, 
      categories, items, banks, paymentMethods, statuses, 
      bankAccounts, personalIncome, memberAdvances
    ] = await Promise.all([
      dbGet(`${basePath}/members`, token),
      dbGet(`${basePath}/insurance_policies`, token),
      dbGet(`${basePath}/expenses/${year}/${month}/member_expenses`, token),
      dbGet(`${basePath}/income/${year}/${month}`, token),
      dbGet(`${basePath}/funds`, token),
      dbGet(`${basePath}/expense_categories`, token),
      dbGet(`${basePath}/expense_items`, token),
      dbGet(`${basePath}/banks`, token),
      dbGet(`${basePath}/payment_methods`, token),
      dbGet(`${basePath}/statuses`, token),
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
      const itemsArr = Object.entries(list || {}).map(([id, e]) => {
        const statusInfo = statusMap[e.status];
        return {
          id, name: e.name || '', amount: roundInt(e.amount),
          status: e.status || '未處理',
          statusIsDone: statusInfo ? !!statusInfo.isDone : false,
          date: e.date || '',
          categoryName: (categories || {})[e.categoryId]?.name || '',
          paymentMethodName: (paymentMethods || {})[e.paymentMethodId]?.name || '',
        };
      }).sort((a, b) => (a.date || '').localeCompare(b.date || ''));
      
      const sum = itemsArr.reduce((s, e) => s + e.amount, 0);
      perMember[memberId] = {
        memberName: (membersObj[memberId]?.name) || '（未知成員）',
        itemCount: itemsArr.length, sum: roundInt(sum), items: itemsArr,
      };
      totalExpense += sum;
    });

    // 收入統計
    let totalIncome = 0;
    Object.entries(income || {}).forEach(([key, val]) => {
      const num = roundInt(val);
      if (key === 'extra' || membersObj[key]) totalIncome += num;
    });

    // 個人收入統計
    let personalIncomeTotal = 0;
    Object.entries(personalIncome || {}).forEach(([memberId, yearData]) => {
      const monthData = yearData?.[year] || {};
      personalIncomeTotal += Object.values(monthData).reduce((s, v) => s + roundInt(v), 0);
    });

    // 保單概況
    let yearlyInsuranceTotal = 0, monthlyInsuranceAverage = 0, activePolicyCount = 0;
    const curY = Number(year), curM = Number(month);

    Object.values(policies || {}).forEach((p) => {
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

    // 銀行總額
    const allTransactions = [];
    Object.entries(bankAccounts || {}).forEach(([bid, bankData]) => {
      Object.entries(bankData.transactions || {}).forEach(([txnId, txn]) => {
        allTransactions.push({ id: txnId, bankId: bid, ...txn });
      });
    });
    
    const currentBankTotal = _calcBankTotal(bankAccounts || {}, allTransactions, year, month);
    const fundValue = Object.values(funds || {}).reduce((s, f) => s + roundInt(f.currentValue), 0);
    const totalAssets = currentBankTotal + fundValue;

    // 代墊總額
    let totalAdvanceRemaining = 0;
    const advancesList = [];
    Object.entries(memberAdvances || {}).forEach(([memberId, advances]) => {
      Object.entries(advances || {}).forEach(([advanceId, adv]) => {
        const remaining = roundInt(adv.remainingAmount);
        totalAdvanceRemaining += remaining;
        advancesList.push({ id: advanceId, memberId, ...adv, remainingAmount: remaining });
      });
    });

    return successResponse({
      year, month,
      totalIncome: roundInt(totalIncome),
      totalExpense: roundInt(totalExpense),
      netBalance: roundInt(totalIncome - totalExpense),
      personalIncomeTotal: roundInt(personalIncomeTotal),
      yearlyInsuranceTotal: roundInt(yearlyInsuranceTotal),
      monthlyInsuranceAverage: roundInt(monthlyInsuranceAverage),
      policyCount: activePolicyCount,
      totalAssets: roundInt(totalAssets),
      bankBalance: roundInt(currentBankTotal),
      fundValue: roundInt(fundValue),
      memberAdvances: advancesList,
      totalAdvanceRemaining: roundInt(totalAdvanceRemaining),
      perMember,
      statusMap,
    });
  } catch (err) { return handleError(err); }
}

/* ============================================
   2. 年度報表 (annual-summary)
   ============================================ */
export async function handleAnnualSummary(context) {
  try {
    const { request } = context;
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const startYear = Number(url.searchParams.get('startYear'));
    const endYear = Number(url.searchParams.get('endYear'));

    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (!startYear || !endYear || startYear > endYear) return errorResponse('MISSING_FIELDS', 'startYear / endYear 無效');
    if (endYear - startYear > 10) return errorResponse('BAD_REQUEST', '年度範圍不可超過 10 年');

    const auth = await authenticate(request, { needFamily: true, body: { familyId } });
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

    const years = [];
    for (let y = startYear; y <= endYear; y++) {
      years.push(_buildYearSummary(y, expensesSnap || {}, incomeSnap || {}, policiesSnap || {}, bankAccountsSnap || {}, fundsSnap || {}));
    }

    return successResponse({ startYear, endYear, years });
  } catch (err) { return handleError(err); }
}

/* ============================================
   3. 結算年度 (settlements-year)
   ============================================ */
export async function handleSettlementsYear(context) {
  try {
    const { request } = context;
    const url = new URL(request.url);
    const familyId = url.searchParams.get('familyId');
    const year = url.searchParams.get('year');
    if (!familyId) return errorResponse('MISSING_FIELDS', '缺少 familyId');
    if (!year || !/^\d{4}$/.test(year)) return errorResponse('MISSING_FIELDS', 'year 格式錯誤（yyyy）');

    const auth = await authenticate(request, { needFamily: true, body: { familyId } });
    if (auth instanceof Response) return auth;
    const { token } = auth;

    const basePath = `families/${familyId}`;
    const [expensesSnap, policiesSnap, paymentsSnap] = await Promise.all([
      dbGet(`${basePath}/expenses/${year}`, token),
      dbGet(`${basePath}/insurance_policies`, token),
      dbGet(`${basePath}/insurance_payments`, token),
    ]);

    const memberExpensesByMonth = {};
    for (let m = 1; m <= 12; m++) {
      const mm = String(m).padStart(2, '0');
      const monthData = (expensesSnap || {})[mm]?.member_expenses || {};
      const rows = [];
      Object.entries(monthData).forEach(([memberId, items]) => {
        Object.entries(items || {}).forEach(([id, e]) => {
          rows.push({ id, memberId, name: e.name || '', amount: roundInt(e.amount), status: e.status || '', date: e.date || '', isAutoLinked: !!e.isAutoLinked, policyId: e.policyId || '' });
        });
      });
      memberExpensesByMonth[mm] = rows;
    }

    const policies = Object.entries(policiesSnap || {}).map(([id, p]) => ({
      id, type: p.type || 'normal', name: p.name || '', company: p.company || '',
      memberId: p.memberId || '', policyHolderId: p.policyHolderId || '', paymentMode: p.paymentMode || 'direct',
      firstStartYear: Number(p.firstStartYear) || 0, firstStartMonth: String(p.firstStartMonth || '01').padStart(2, '0'),
      totalPolicyYears: Number(p.totalPolicyYears) || 0, periods: p.periods || {},
      monthlyPremium: roundInt(p.monthlyPremium), monthlyAverage: roundInt(p.monthlyAverage), annualPremium: roundInt(p.annualPremium),
    }));

    const paymentsCache = {};
    policies.forEach((p) => {
      const pYearPayments = (paymentsSnap || {})[p.id]?.[year] || {};
      paymentsCache[p.id] = { [year]: pYearPayments };
    });

    return successResponse({ year, memberExpensesByMonth, policies, paymentsCache });
  } catch (err) { return handleError(err); }
}

/* ============================================
   輔助函式
   ============================================ */
function _buildYearSummary(year, expensesObj, incomeObj, policiesObj, bankAccountsObj, fundsObj) {
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

    if (totalExpense > 0) monthsWithData++;
    monthly.push({ monthNum: m, month: mm, totalIncome: roundInt(totalIncome), totalExpense: roundInt(totalExpense), netBalance: roundInt(totalIncome - totalExpense) });
    yearTotalIncome += totalIncome;
    yearTotalExpense += totalExpense;
  }

  const allTransactions = [];
  Object.entries(bankAccountsObj).forEach(([bid, bankData]) => {
    Object.entries(bankData.transactions || {}).forEach(([txnId, txn]) => {
      allTransactions.push({ id: txnId, bankId: bid, ...txn });
    });
  });
  const bankBalance = _calcBankTotal(bankAccountsObj, allTransactions, year, '12');
  const fundValue = Object.values(fundsObj).reduce((s, f) => s + roundInt(f.currentValue), 0);

  return {
    year, totalIncome: roundInt(yearTotalIncome), totalExpense: roundInt(yearTotalExpense),
    netBalance: roundInt(yearTotalIncome - yearTotalExpense),
    bankBalance: roundInt(bankBalance), fundValue: roundInt(fundValue), totalAssets: roundInt(bankBalance + fundValue),
    avg: monthsWithData > 0 ? Math.round(yearTotalExpense / monthsWithData) : 0,
    monthly,
  };
}

function _calcBankTotal(bankAccounts, transactions, targetYear, targetMonth) {
  if (!targetYear || !targetMonth) return 0;
  const tY = Number(targetYear), tM = Number(targetMonth);
  let total = 0;

  Object.values(bankAccounts || {}).forEach((acc) => {
    const init = Number(acc.initialBalance) || 0;
    const initY = Number(acc.initialYear) || 0, initM = Number(acc.initialMonth) || 0;
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
      else if (txn.type === 'out' || txn.type === 'transfer') balance -= amount;
    });
    total += balance;
  });
  return total;
}

/* ═══ EOF ═══
F: api/handlers-report.js
V: 1792900380
W: ai-system-20261024-012#33
═══ EOF ═══ */```

<!-- === FILE END: ./functions/api/handlers-report.js === -->

<!-- === FILE START: ./functions/api/handlers.js === -->

## 📄 ./functions/api/handlers.js

```js
// ============================================
// handlers.js — 後端處理函式統一出口（V2 重構版）
// 位置：api/handlers.js
// 說明：將業務邏輯拆分到不同模組，這裡負責統一匯出，
//       讓 [[path]].js 只需要認這一個檔案。
// ============================================

export * from './handlers-core.js';
export * from './handlers-report.js';
export * from './handlers-admin.js';

/* ═══ EOF ═══
F: api/handlers.js
V: 1792900360
W: ai-system-20261024-012#31
═══ EOF ═══ */```

<!-- === FILE END: ./functions/api/handlers.js === -->

<!-- === FILE START: ./functions/api/helpers.js === -->

## 📄 ./functions/api/helpers.js

```js
// ============================================
// helpers.js — 後端共用輔助（V2 重構版）
// 位置：api/helpers.js
// 說明：集中處理 Token 驗證、權限檢查與標準化回應格式。
// ============================================

import { 
  FIREBASE_API_KEY, 
  SUPERADMIN_EMAIL, 
  IDENTITY_TOOLKIT_URL, 
  jsonResponse, 
  dbGet 
} from './config.js';

const IDENTITY_TOOLKIT_LOOKUP_URL = `${IDENTITY_TOOLKIT_URL}/accounts:lookup`;

// 輕量 Token 快取（避免同一請求重複驗證，生命週期 1 分鐘）
const _tokenCache = new Map();
const _TOKEN_CACHE_MS = 60 * 1000;

/* ============================================
   1. 權杖與身分驗證
   ============================================ */

export function extractToken(request) {
  const authHeader = request.headers.get('Authorization') || '';
  return authHeader.replace('Bearer ', '').trim();
}

export async function verifyToken(token) {
  if (!token) return null;

  const cached = _tokenCache.get(token);
  if (cached && cached.expireAt > Date.now()) return cached.user;

  try {
    const res = await fetch(`${IDENTITY_TOOLKIT_LOOKUP_URL}?key=${FIREBASE_API_KEY}`, {
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
    console.warn('[api/helpers] verifyToken 失敗：', err);
    return null;
  }
}

export async function verifySuperAdmin(token) {
  const user = await verifyToken(token);
  if (!user) return null;
  if (user.email !== SUPERADMIN_EMAIL) return null;
  return user;
}

/**
 * 檢查使用者是否有權存取特定家庭
 */
export async function verifyFamilyAccess(token, familyId) {
  if (!familyId) return null;

  const user = await verifyToken(token);
  if (!user) return null;

  const isSuper = user.email === SUPERADMIN_EMAIL;
  if (isSuper) return { user, isSuper: true, isOwner: true, canInput: true };

  // 查詢該使用者是否在此家庭的 memberAccounts 中
  const memberAccount = await dbGet(`platform/families/${familyId}/memberAccounts/${user.localId}`, token);
  
  if (memberAccount) {
    return {
      user,
      isSuper: false,
      isOwner: memberAccount.role === 'owner',
      canInput: memberAccount.canInput !== false,
      memberAccount
    };
  }

  return null;
}

/* ============================================
   2. 驗證封裝 (Middleware)
   ============================================ */

/**
 * 統一的身分驗證封裝
 */
export async function authenticate(request, options = {}) {
  const { needSuperAdmin, needFamily } = options;
  const token = extractToken(request);

  if (needSuperAdmin) {
    const user = await verifySuperAdmin(token);
    if (!user) return errorResponse('FORBIDDEN', '需要超級管理員權限');
    return { token, user, isSuper: true };
  }

  if (needFamily) {
    let familyId = '';
    try {
      const body = await request.clone().json();
      familyId = body.familyId;
    } catch (e) { /* 可能不是 JSON */ }
    
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

/* ============================================
   3. 回應與錯誤處理
   ============================================ */

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

export function successResponse(data = {}) {
  return jsonResponse({ ok: true, ...data });
}

export function handleError(err) {
  console.error('[api/helpers] API error:', err);
  return errorResponse('INTERNAL', String(err?.message || err));
}

/* ============================================
   4. 通用工具
   ============================================ */
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

export function roundInt(v) {
  return Math.round(Number(v) || 0);
}

/* ═══ EOF ═══
F: api/helpers.js
V: 1792900340
W: ai-system-20261024-012#29
═══ EOF ═══ */```

<!-- === FILE END: ./functions/api/helpers.js === -->

<!-- === FILE START: ./index.html === -->

## 📄 ./index.html

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>FAMILY.FIN V2</title>
  
  <link rel="manifest" href="./manifest.json">
  <meta name="theme-color" content="#0F172A">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="FAMILY V2">
  
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="preconnect" href="https://www.gstatic.com" crossorigin>
  <link rel="dns-prefetch" href="https://identitytoolkit.googleapis.com">

  <!-- 引入主題與排版樣式 -->
  <link rel="stylesheet" href="./css/theme.css">
  <link rel="stylesheet" href="./css/app.css">
</head>
<body>
  <div id="app-loading" style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh;">
    <div class="spinner"></div>
    <div style="font-family: monospace; color: #00F0FF; letter-spacing: 1px;">SYSTEM INITIALIZING...</div>
  </div>

  <div id="app-root" style="display: none;"></div>

  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>
  <script type="module" src="./engine/main.js"></script>
</body>
</html>

<!-- ═══ EOF ═══
F: index.html
V: 1792900620
W: ai-system-20261024-012#57
═══ EOF ═══ -->```

<!-- === FILE END: ./index.html === -->

<!-- === FILE START: ./lib/expense-modal.js === -->

## 📄 ./lib/expense-modal.js

```js
// ============================================
// expense-modal.js — 支出編輯 Modal
// 位置：lib/expense-modal.js
// 說明：處理「新增支出」與「編輯支出」的完整表單。
//       包含類別與項目聯動、成員/類別/支付方式的「+」快速新增。
// ============================================

import { createForm } from '../ui/form.js';
import { openModal, closeModal } from '../ui/modal.js';
import { showToast } from '../ui/toast.js';
import { getList } from '../config/entity-registry.js';
import { addExpense, updateExpense, removeExpense } from '../db/write.js';
import { AppState } from '../config/state.js';
import { RESERVED_IDS } from '../config/constants.js';
import { todayISO } from '../ui/format.js';

const MODAL_ID = 'app-expense-modal';

/**
 * 開啟支出 Modal
 * @param {Object} opts
 * @param {Object} [opts.row] - 若為編輯模式，傳入現有資料
 * @param {Function} [opts.onSuccess] - 成功後的回呼
 */
export async function openExpenseModal(opts = {}) {
  try {
    const { row = null, onSuccess } = opts;
    const isEdit = !!row;

    const existingModal = document.getElementById(MODAL_ID);
    if (existingModal) existingModal.remove();

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = MODAL_ID;
    overlay.innerHTML = `
      <div class="modal" style="max-width:560px; max-height:90vh; overflow-y:auto;">
        <h2 class="modal-title" style="font-size:18px; font-weight:700; margin-bottom:16px; color:var(--text-primary); border-left:4px solid var(--primary); padding-left:12px;">
          ${isEdit ? '編輯支出' : '新增支出'}
        </h2>
        <div id="${MODAL_ID}-form-root"></div>
      </div>
    `;
    document.body.appendChild(overlay);
    
    overlay.addEventListener('click', (e) => {
      if (e.target === overlay) { closeModal(MODAL_ID); overlay.remove(); }
    });

    // 準備選項資料（加入防呆，確保拿到的是陣列）
    const members = getList('members') || [];
    const memberOpts = members.map(m => ({ value: m.id, label: m.name }));
    memberOpts.push({ value: RESERVED_IDS.SHARED_MEMBER, label: '🏠 家庭共用' });

    const categories = getList('categories') || [];
    const catOpts = categories.map(c => ({ value: c.id, label: c.name }));

    const payments = getList('payments') || [];
    const payOpts = payments.map(p => ({ value: p.id, label: p.name }));

    const banks = getList('bank_accounts') || [];
    const bankOpts = banks.map(b => ({ value: b.id, label: b.name }));

    // 初始項目選項
    const items = getList('items') || [];
    let initialItemOpts = [];
    if (isEdit && row.categoryId) {
      initialItemOpts = items.filter(i => i.categoryId === row.categoryId).map(i => ({ value: i.id, label: i.name }));
    }

    // 表單欄位定義
    const fields = [
      { 
        id: 'memberId', 
        label: '成員', 
        type: 'select', 
        required: true, 
        options: memberOpts,
        plusEntity: 'member',
        onPlusReload: (result, formEl) => _reloadSelect(formEl, 'memberId', 'members', '請選擇成員', result)
      },
      { id: 'name', label: '項目名稱', type: 'text', required: true, placeholder: '例如：買書、看醫生' },
      { id: 'amount', label: '金額 (HK$)', type: 'number', required: true, min: 0, step: 1 },
      { id: 'date', label: '日期 (YYYY-MM-DD)', type: 'date', required: false },
      { 
        id: 'categoryId', 
        label: '類別', 
        type: 'select', 
        required: false, 
        options: catOpts,
        plusEntity: 'category',
        onPlusReload: (result, formEl) => _reloadSelect(formEl, 'categoryId', 'categories', '請選擇類別', result)
      },
      { 
        id: 'itemId', 
        label: '項目', 
        type: 'select', 
        required: false, 
        options: initialItemOpts
      },
      { 
        id: 'paymentMethodId', 
        label: '支付方式', 
        type: 'select', 
        required: false, 
        options: payOpts,
        plusEntity: 'payment',
        onPlusReload: (result, formEl) => _reloadSelect(formEl, 'paymentMethodId', 'payments', '請選擇支付方式', result)
      },
      { 
        id: 'bankId', 
        label: '出帳銀行 (可選)', 
        type: 'select', 
        required: false, 
        options: bankOpts 
      }
    ];

    // 取得預設成員 ID（相容 V1/V2 寫法）
    const defaultMemberId = AppState.getCurrentMemberId 
      ? AppState.getCurrentMemberId() 
      : (AppState.getMemberAccount() ? AppState.getMemberAccount().memberId : '');

    // 預設值
    const initialData = isEdit ? {
      memberId: row.memberId || '',
      name: row.name || '',
      amount: row.amount || 0,
      date: row.date || '',
      categoryId: row.categoryId || '',
      itemId: row.itemId || '',
      paymentMethodId: row.paymentMethodId || '',
      bankId: row.bankId || '',
    } : {
      memberId: defaultMemberId || (memberOpts.length > 0 ? memberOpts[0].value : ''),
      amount: 0,
      date: todayISO(),
    };

    createForm({
      container: `${MODAL_ID}-form-root`,
      fields,
      initialData,
      submitText: isEdit ? '儲存' : '新增',
      onCancel: () => { closeModal(MODAL_ID); overlay.remove(); },
      onSubmit: async (data) => {
        try {
          const payload = {
            ...data,
            amount: Math.round(Number(data.amount) || 0), // 金額防呆
          };

          const { year, month } = AppState.getYearMonth();
          const y = year || String(new Date().getFullYear());
          const m = month === 'all' ? String(new Date().getMonth() + 1).padStart(2, '0') : month;

          if (isEdit) {
            if (payload.memberId !== row.memberId) {
              await removeExpense(row.year, row.month, row.memberId, row.id);
              await addExpense(row.year, row.month, payload.memberId, payload);
            } else {
              await updateExpense(row.year, row.month, row.memberId, row.id, payload);
            }
            showToast('✅ 已更新支出', 'success');
          } else {
            await addExpense(y, m, payload.memberId, payload);
            showToast('✅ 已新增支出', 'success');
          }

          closeModal(MODAL_ID);
          overlay.remove();
          if (typeof onSuccess === 'function') onSuccess();
        } catch (err) {
          console.error('[expense-modal] 儲存失敗：', err);
          showToast('儲存失敗：' + err.message, 'error');
        }
      }
    });

    // 綁定類別與項目的聯動邏輯
    const catEl = document.getElementById('field-categoryId');
    const itemEl = document.getElementById('field-itemId');
    if (catEl && itemEl) {
      catEl.addEventListener('change', () => {
        const catId = catEl.value;
        const currentItems = getList('items') || [];
        const filtered = catId ? currentItems.filter(i => i.categoryId === catId) : [];
        
        itemEl.innerHTML = '<option value="">— 請選擇項目 —</option>' +
          filtered.map(i => `<option value="${i.id}">${i.name}</option>`).join('');
        
        itemEl.value = '';
      });
    }

    openModal(MODAL_ID);
    if (window.lucide) window.lucide.createIcons();
    
  } catch (err) {
    console.error('[expense-modal] 初始化失敗：', err);
    // 將錯誤往外拋，讓呼叫端 (input-center.js) 的 try-catch 捕捉並 alert
    throw err;
  }
}

/* ============================================
   內部輔助：重新載入下拉選單（供 + 號使用）
   ============================================ */
function _reloadSelect(formEl, fieldId, listType, defaultLabel, result) {
  const el = formEl.querySelector(`#field-${fieldId}`);
  if (!el) return;

  // 從 registry 拿取當下快取
  const list = getList(listType) || [];
  const opts = list.map(x => ({ value: x.id, label: x.name }));
  
  if (fieldId === 'memberId') {
    opts.push({ value: RESERVED_IDS.SHARED_MEMBER, label: '🏠 家庭共用' });
  }

  // 將剛新增的 result 補進去，避免 Firebase 非同步延遲導致看不到
  if (result && result.id && !opts.some(o => o.value === result.id)) {
    opts.push({ value: result.id, label: result.name });
  }

  const curVal = el.value;
  let html = `<option value="">— ${defaultLabel} —</option>`;
  html += opts.map(o => `<option value="${o.value}">${o.label}</option>`).join('');
  
  el.innerHTML = html;
  
  // 如果原本沒選，或有新加入的，直接選中新加入的
  if (result && result.id) {
    el.value = result.id;
  } else if (!curVal && opts.length > 0) {
    el.value = opts[opts.length - 1].value;
  } else {
    el.value = curVal;
  }
}

/* ═══ EOF ═══
F: lib/expense-modal.js
V: 1792901770
W: webapp-20261025-001#27
═══ */```

<!-- === FILE END: ./lib/expense-modal.js === -->

<!-- === FILE START: ./manifest.json === -->

## 📄 ./manifest.json

```json
{
  "name": "家庭財務與固定供款管理 V2",
  "short_name": "FAMILY V2",
  "description": "全家財務、保險供款、基金資產、銀行交易即時管理 (重構版)",
  "start_url": "./index.html",
  "scope": "./",
  "display": "standalone",
  "orientation": "portrait",
  "background_color": "#0F172A",
  "theme_color": "#0F172A",
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
}```

<!-- === FILE END: ./manifest.json === -->

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

<!-- === FILE START: ./pages/annual-report.js === -->

## 📄 ./pages/annual-report.js

```js
// ============================================
// annual-report.js — 年度報表
// 位置：pages/annual-report.js
// 說明：顯示全年總合與月度明細，支援匯出 CSV。
//       使用 renderPageLayout 統一排版，並將月度明細改為引擎渲染。
// ============================================

import { AppState } from '../config/state.js';
import { api } from '../domain/api-client.js';
import { getYearList } from '../config/app-config.js';
import { renderPageLayout } from '../ui/layout.js';
import { renderTable } from '../ui/table.js';
import { showToast } from '../ui/toast.js';
import { formatHKD, esc } from '../ui/format.js';

export default {
  _data: null,
  _view: 'monthly', // monthly | summary

  async render() {
    const currentYear = AppState.getYearMonth().year;
    const years = getYearList();

    // 1. 準備 Header Actions
    const headerActions = `
      <button type="button" class="btn btn-primary" id="btn-export-csv">
        <i data-lucide="file-spreadsheet" style="width:16px;height:16px;"></i> 匯出 CSV
      </button>
    `;

    // 2. 準備控制列 HTML
    const controlsHtml = `
      <div style="display:flex; align-items:center; gap:12px; flex-wrap:wrap;">
        <select class="input" id="report-year" style="padding:6px 28px 6px 10px; width:auto;">
          ${years.map(y => `<option value="${y}" ${String(y) === currentYear ? 'selected' : ''}>${y} 年</option>`).join('')}
        </select>
        <div style="display:flex; gap:4px; background:var(--bg-surface); border:1px solid var(--border-color); border-radius:var(--radius-md); padding:4px;">
          <button type="button" class="btn" data-view="monthly" style="padding:6px 12px; border:none; border-radius:var(--radius-sm); transition:all 0.2s;">月度明細</button>
          <button type="button" class="btn" data-view="summary" style="padding:6px 12px; border:none; border-radius:var(--radius-sm); transition:all 0.2s;">全年總合</button>
        </div>
      </div>
    `;

    // 3. 繪製骨架
    renderPageLayout({
      title: '年度報表',
      headerActions,
      stats: [],
      controls: controlsHtml,
      listContainerId: 'report-table-container'
    });

    // 4. 初始化 UI 狀態
    this._updateViewButtons();
    
    // 5. 綁定事件
    document.getElementById('report-year')?.addEventListener('change', (e) => {
      const y = e.target.value;
      AppState.setYearMonth(y, AppState.month);
      this._loadData(Number(y));
    });

    document.querySelectorAll('button[data-view]').forEach(btn => {
      btn.addEventListener('click', (e) => {
        const v = e.target.dataset.view;
        if (this._view === v) return;
        this._view = v;
        this._updateViewButtons();
        this._renderTable();
      });
    });

    document.getElementById('btn-export-csv')?.addEventListener('click', () => this._exportCsv());

    // 6. 載入資料
    await this._loadData(Number(currentYear));
  },

  _updateViewButtons() {
    document.querySelectorAll('button[data-view]').forEach(b => {
      const isActive = b.dataset.view === this._view;
      b.style.background = isActive ? 'var(--primary)' : 'transparent';
      b.style.color = isActive ? '#fff' : 'var(--text-secondary)';
      b.style.fontWeight = isActive ? '600' : '500';
    });
  },

  async _loadData(year) {
    const tableRoot = document.getElementById('report-table-container');
    if (!tableRoot) return;

    tableRoot.innerHTML = '<div style="padding:20px; text-align:center; color:var(--text-muted);">載入資料中…</div>';

    try {
      const res = await api.fetchAnnualSummaryRange(year, year);
      if (!res || !res.years || res.years.length === 0) {
        throw new Error('查無資料');
      }

      this._data = res.years[0];
      this._updateStats();
      this._renderTable();

    } catch (err) {
      console.error('[annual-report] 載入失敗：', err);
      showToast('載入報表失敗', 'error');
      tableRoot.innerHTML = '<div style="color:var(--accent-danger); padding:20px; text-align:center;">載入失敗</div>';
    }
  },

  _updateStats() {
    const statsRoot = document.getElementById('page-stats-container');
    if (!statsRoot || !this._data) return;

    statsRoot.style.display = 'grid';
    const d = this._data;
    const pnlCls = d.netBalance >= 0 ? 'var(--accent-success)' : 'var(--accent-danger)';

    statsRoot.innerHTML = [
      { title: '年度總收入', val: formatHKD(d.totalIncome), color: 'var(--accent-success)', icon: 'trending-up' },
      { title: '年度總支出', val: formatHKD(d.totalExpense), color: 'var(--accent-danger)', icon: 'trending-down' },
      { title: '年度保險平攤', val: formatHKD(d.yearlyInsuranceTotal || 0), color: 'var(--accent-warm)', icon: 'shield' },
      { title: '年度淨餘額', val: formatHKD(d.netBalance), color: pnlCls, icon: 'wallet' }
    ].map(card => `
      <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
        <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
          <i data-lucide="${card.icon}" style="width:14px;height:14px; flex-shrink:0;"></i> 
          <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${card.title}</span>
        </div>
        <div class="mono" style="font-size:20px; font-weight:700; color:${card.color}; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
          ${card.val}
        </div>
      </div>
    `).join('');

    if (window.lucide) window.lucide.createIcons();
  },

  _renderTable() {
    const container = document.getElementById('report-table-container');
    if (!container || !this._data) return;

    const monthlyData = this._data.monthly || [];
    
    if (this._view === 'monthly') {
      if (monthlyData.length === 0) {
        container.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">尚無月度資料</div>';
        return;
      }

      const rows = monthlyData.map(m => ({
        month: `${m.monthNum} 月`,
        totalIncome: m.totalIncome,
        totalExpense: m.totalExpense,
        netBalance: m.netBalance
      }));

      // 🆕 改用引擎渲染
      renderTable({
        container,
        rows,
        columnsKey: 'annualMonthly',
        emptyText: '尚無月度資料'
      });

    } else {
      // 全年總合（單行統計表，保留手寫結構，因為它是特殊報表而非列表）
      container.innerHTML = `
        <div class="glass-card" style="padding:0; overflow:hidden;">
          <div style="overflow-x:auto;">
            <table class="data-table" style="width:100%; border-collapse:collapse; min-width:700px;">
              <thead>
                <tr>
                  <th style="white-space:nowrap;">年度</th>
                  <th class="num" style="white-space:nowrap;">總收入</th>
                  <th class="num" style="white-space:nowrap;">總支出</th>
                  <th class="num" style="white-space:nowrap;">保險平攤</th>
                  <th class="num" style="white-space:nowrap;">銀行餘額</th>
                  <th class="num" style="white-space:nowrap;">基金現值</th>
                  <th class="num" style="white-space:nowrap;">淨餘額</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td style="white-space:nowrap;">${this._data.year} 年</td>
                  <td class="num text-emerald mono" style="white-space:nowrap;">${formatHKD(this._data.totalIncome)}</td>
                  <td class="num text-red mono" style="white-space:nowrap;">${formatHKD(this._data.totalExpense)}</td>
                  <td class="num text-magenta mono" style="white-space:nowrap;">${formatHKD(this._data.yearlyInsuranceTotal || 0)}</td>
                  <td class="num mono" style="white-space:nowrap;">${formatHKD(this._data.bankBalance || 0)}</td>
                  <td class="num mono" style="white-space:nowrap;">${formatHKD(this._data.fundValue || 0)}</td>
                  <td class="num mono ${this._data.netBalance >= 0 ? 'text-emerald' : 'text-red'}" style="white-space:nowrap;">${formatHKD(this._data.netBalance)}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      `;
    }
  },

  _exportCsv() {
    if (!this._data) return;
    const monthlyData = this._data.monthly || [];
    if (monthlyData.length === 0) {
      showToast('無資料可匯出', 'warning');
      return;
    }

    const year = this._data.year;
    const lines = [['月份', '總收入', '總支出', '淨餘額'].join(',')];
    
    monthlyData.forEach(m => {
      lines.push([`${m.monthNum}月`, m.totalIncome, m.totalExpense, m.netBalance].join(','));
    });

    const csv = '\uFEFF' + lines.join('\n');
    const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `annual-report-${year}.csv`;
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
    showToast('✅ 已匯出 CSV', 'success');
  },

  destroy() {}
};

/* ═══ EOF ═══
F: pages/annual-report.js
V: 1792901950
W: webapp-20261025-001#43
═══ */```

<!-- === FILE END: ./pages/annual-report.js === -->

<!-- === FILE START: ./pages/bank.js === -->

## 📄 ./pages/bank.js

```js
// ============================================
// bank.js — 銀行交易頁面
// 位置：pages/bank.js
// 說明：顯示銀行帳號清單、交易明細，並計算即時餘額。
//       已完全套用 renderPageLayout 解決排版問題。
// ============================================

import { AppState } from '../config/state.js';
import { listenBankAccounts, listenAllBankTransactions } from '../db/index.js';
import { api } from '../domain/api-client.js';
import { calcBankBalance, calcTotalBankBalance } from '../domain/bank.js';
import { renderTable, renderCards } from '../ui/table.js';
import { renderViewToggle, renderYearMonth } from '../ui/controls.js';
import { renderPageLayout } from '../ui/layout.js';
import { openModal, closeModal, openConfirm } from '../ui/modal.js';
import { createForm } from '../ui/form.js';
import { showToast } from '../ui/toast.js';
import { formatHKD, esc } from '../ui/format.js';
import { getColumns, getResolvers } from '../config/columns.js';

export default {
  _unsubAccounts: null,
  _unsubTxns: null,
  _accounts: [],
  _txns: [],

  async render() {
    // 1. 準備控制列 HTML
    const controlsHtml = `
      <div style="display:flex; align-items:center; gap:12px; flex-wrap:wrap;">
        <button type="button" class="btn btn-primary" id="btn-add-txn">
          <i data-lucide="plus" style="width:14px;height:14px;"></i> 新增交易
        </button>
        <button type="button" class="btn btn-ghost" id="btn-add-account">
          <i data-lucide="plus" style="width:14px;height:14px;"></i> 新增帳號
        </button>
        <div id="bank-view-toggle"></div>
      </div>
      <div id="bank-controls-period"></div>
    `;

    // 2. 繪製骨架
    renderPageLayout({
      title: '銀行交易',
      stats: [],
      controls: controlsHtml,
      listContainerId: 'bank-content-container'
    });

    // 3. 綁定控制列事件
    renderYearMonth('bank-controls-period', { includeAllMonth: true }, () => {
      this._renderData();
    });

    renderViewToggle('bank-view-toggle', () => {
      this._renderData();
    });

    document.getElementById('btn-add-account').onclick = () => this._openAccountModal();
    document.getElementById('btn-add-txn').onclick = () => this._openTxnModal();

    // 4. 監聽資料
    this._unsubAccounts = listenBankAccounts((accounts) => {
      this._accounts = accounts;
      this._renderData();
    });

    this._unsubTxns = listenAllBankTransactions((txns) => {
      this._txns = txns;
      this._renderData();
    });
  },

  _renderData() {
    const contentRoot = document.getElementById('bank-content-container');
    const statsRoot = document.getElementById('page-stats-container');
    if (!contentRoot || !statsRoot) return;

    const { year, month } = AppState.getYearMonth();
    const y = year || String(new Date().getFullYear());
    const tM = month === 'all' ? '12' : (month || '12');
    const currentView = AppState.getCurrentView() || 'table';

    const totalBalance = calcTotalBankBalance(this._accounts, this._txns, y, tM);
    
    const prefix = `${y}-${String(tM).padStart(2, '0')}`;
    const monthTxns = month === 'all' 
      ? this._txns.filter(t => (t.date || '').startsWith(y))
      : this._txns.filter(t => (t.date || '').startsWith(prefix));

    const inTotal = monthTxns.filter(t => t.type === 'in').reduce((s, t) => s + (Number(t.amount) || 0), 0);
    const outTotal = monthTxns.filter(t => t.type !== 'in').reduce((s, t) => s + (Number(t.amount) || 0), 0);
    const netChange = inTotal - outTotal;

    // 更新統計卡
    statsRoot.style.display = 'grid';
    statsRoot.innerHTML = [
      { title: '家庭總餘額', val: formatHKD(totalBalance), color: totalBalance >= 0 ? 'var(--accent-success)' : 'var(--accent-danger)', icon: 'landmark', hint: '所有帳號加總' },
      { title: '本月入帳', val: formatHKD(inTotal), color: 'var(--accent-success)', icon: 'arrow-down-circle', hint: `${monthTxns.filter(t => t.type === 'in').length} 筆` },
      { title: '本月出帳', val: formatHKD(outTotal), color: 'var(--accent-danger)', icon: 'arrow-up-circle', hint: `${monthTxns.filter(t => t.type !== 'in').length} 筆` },
      { title: '本月淨變動', val: formatHKD(netChange), color: netChange >= 0 ? 'var(--accent-success)' : 'var(--accent-danger)', icon: 'trending-up', hint: '入帳 - 出帳' }
    ].map(card => `
      <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
        <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
          <i data-lucide="${card.icon}" style="width:14px;height:14px; flex-shrink:0;"></i> 
          <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${card.title}</span>
        </div>
        <div class="mono" style="font-size:20px; font-weight:700; color:${card.color}; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
          ${card.val}
        </div>
        <div style="font-size:11px; color:var(--text-muted); overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${esc(card.hint)}</div>
      </div>
    `).join('');

    // 渲染清單內容
    contentRoot.innerHTML = `
      <div style="margin-bottom:24px;">
        <h3 style="font-size:15px; margin-bottom:12px; color:var(--text-primary);">銀行帳號清單</h3>
        <div id="bank-accounts-list"></div>
      </div>
      <div>
        <h3 style="font-size:15px; margin-bottom:12px; color:var(--text-primary);">交易記錄</h3>
        <div id="bank-txns-list"></div>
      </div>
    `;

    const accRoot = document.getElementById('bank-accounts-list');
    const txnRoot = document.getElementById('bank-txns-list');

    // 渲染帳號 (使用動態餘額)
    const accCols = getColumns('bankAccounts');
    accCols.push({ id: 'dynamicBalance', label: '當前餘額', type: 'number', sortable: true });
    const accResolvers = getResolvers('bankAccounts');
    accResolvers.dynamicBalance = (_, row) => {
      const bal = calcBankBalance(row, this._txns.filter(t => t.bankId === row.id), y, tM);
      return `<span class="mono" style="font-weight:700; color:${bal >= 0 ? 'var(--text-primary)' : 'var(--accent-danger)'};">${formatHKD(bal)}</span>`;
    };

    if (currentView === 'card') {
      renderCards({ container: accRoot, rows: this._accounts, columnsKey: 'bankAccounts', customResolvers: accResolvers, customColumns: accCols });
    } else {
      renderTable({ container: accRoot, rows: this._accounts, columnsKey: 'bankAccounts', customResolvers: accResolvers, customColumns: accCols });
    }

    // 渲染交易明細
    const sortedTxns = [...monthTxns].sort((a, b) => (b.date || '').localeCompare(a.date || ''));
    if (currentView === 'card') {
      renderCards({ container: txnRoot, rows: sortedTxns, columnsKey: 'bankTransactions', emptyText: '本月尚無交易紀錄' });
    } else {
      renderTable({ container: txnRoot, rows: sortedTxns, columnsKey: 'bankTransactions', emptyText: '本月尚無交易紀錄' });
    }

    if (window.lucide) window.lucide.createIcons();
  },

  _openAccountModal() {
    const modalId = 'account-modal';
    document.getElementById(modalId)?.remove();
    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `<div class="modal" style="max-width:400px; background:var(--bg-surface);"><h2 class="modal-title">新增銀行帳號</h2><div id="account-form-root"></div></div>`;
    document.body.appendChild(overlay);
    openModal(modalId);

    createForm({
      container: 'account-form-root',
      fields: [
        { id: 'name', label: '帳號名稱', type: 'text', required: true },
        { id: 'type', label: '類型', type: 'select', required: true, options: [{ value: 'family', label: '家庭帳號' }, { value: 'personal', label: '個人帳號' }] },
        { id: 'initialBalance', label: '初始餘額', type: 'number', required: true },
        { id: 'initialYear', label: '初始年份', type: 'number', required: true },
        { id: 'initialMonth', label: '初始月份', type: 'number', required: true, min: 1, max: 12 }
      ],
      initialData: { type: 'family', initialBalance: 0, initialYear: new Date().getFullYear(), initialMonth: 1 },
      submitText: '新增',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        try {
          await api.bankAccounts.create(data);
          showToast('✅ 已新增銀行帳號', 'success');
          closeModal(modalId);
          overlay.remove();
        } catch (err) { showToast('新增失敗：' + err.message, 'error'); }
      }
    });
  },

  _openTxnModal() {
    const modalId = 'txn-modal';
    document.getElementById(modalId)?.remove();
    if (this._accounts.length === 0) { showToast('請先新增銀行帳號', 'warning'); return; }
    const accOptions = this._accounts.map(a => ({ value: a.id, label: a.name }));

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `<div class="modal" style="max-width:450px; background:var(--bg-surface);"><h2 class="modal-title">新增交易</h2><div id="txn-form-root"></div></div>`;
    document.body.appendChild(overlay);
    openModal(modalId);

    createForm({
      container: 'txn-form-root',
      fields: [
        { id: 'bankId', label: '銀行帳號', type: 'select', required: true, options: accOptions },
        { id: 'type', label: '類型', type: 'select', required: true, options: [{ value: 'in', label: '入帳' }, { value: 'out', label: '出帳' }, { value: 'transfer', label: '內部轉帳' }] },
        { id: 'category', label: '分類', type: 'select', required: true, options: [{ value: 'contribution', label: '家用轉入' }, { value: 'expense', label: '支出' }, { value: 'insurance', label: '保險' }, { value: 'reimbursement', label: '代墊報銷' }, { value: 'manual', label: '手動' }] },
        { id: 'amount', label: '金額', type: 'number', required: true },
        { id: 'date', label: '日期', type: 'date', required: true },
        { id: 'note', label: '備註', type: 'text' }
      ],
      initialData: { type: 'out', category: 'manual', date: new Date().toISOString().slice(0, 10) },
      submitText: '新增',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        try {
          const { bankId, ...txnData } = data;
          await api.bankTransactions.create(bankId, txnData);
          showToast('✅ 已新增交易', 'success');
          closeModal(modalId);
          overlay.remove();
        } catch (err) { showToast('新增失敗：' + err.message, 'error'); }
      }
    });
  },

  destroy() {
    if (this._unsubAccounts) { try { this._unsubAccounts(); } catch (e) {} this._unsubAccounts = null; }
    if (this._unsubTxns) { try { this._unsubTxns(); } catch (e) {} this._unsubTxns = null; }
  }
};

/* ═══ EOF ═══
F: pages/bank.js
V: 1792901920
W: webapp-20261025-001#40
═══ */```

<!-- === FILE END: ./pages/bank.js === -->

<!-- === FILE START: ./pages/code-list.js === -->

## 📄 ./pages/code-list.js

```js
// ============================================
// code-list.js — 編號對照清單
// 位置：pages/code-list.js
// 說明：整合所有基礎實體的編號與名稱，方便對照與搜尋。
// ============================================

import {
  getMembersOnce, getBanksOnce, getBankAccountsOnce,
  getCategoriesOnce, getItemsOnce, getPaymentMethodsOnce,
  getInsuranceCompaniesOnce, getInsurancePoliciesOnce, getFundsOnce
} from '../db/index.js';
import { renderTable, renderCards } from '../ui/table.js';
import { renderViewToggle } from '../ui/controls.js';
import { renderPageLayout } from '../ui/layout.js';
import { AppState } from '../config/state.js';
import { showToast } from '../ui/toast.js';
import { esc } from '../ui/format.js';

export default {
  _allRows: [],

  async render() {
    // 1. 準備控制列 HTML
    const controlsHtml = `
      <div style="display:flex; align-items:center; gap:12px; flex-wrap:wrap;">
        <div id="code-view-toggle"></div>
      </div>
      <div style="display:flex; align-items:center; gap:8px; flex-wrap:wrap;">
        <label style="font-size:13px; color:var(--text-secondary); white-space:nowrap;">篩選類型：</label>
        <select class="input" id="code-filter-type" style="padding:8px; min-width:120px;">
          <option value="all">全部</option>
          <option value="member">成員</option>
          <option value="bank">銀行</option>
          <option value="bankAccount">銀行帳號</option>
          <option value="category">支出類別</option>
          <option value="item">支出項目</option>
          <option value="payment">支付方式</option>
          <option value="company">保險公司</option>
          <option value="policy">保單</option>
          <option value="fund">基金</option>
        </select>
      </div>
    `;

    // 2. 呼叫統一引擎繪製骨架
    renderPageLayout({
      title: '編號對照清單',
      stats: [], 
      controls: controlsHtml,
      listContainerId: 'code-list-container'
    });

    // 3. 綁定事件
    renderViewToggle('code-view-toggle', () => {
      const filterType = document.getElementById('code-filter-type').value;
      this._renderList(filterType);
    });

    document.getElementById('code-filter-type')?.addEventListener('change', (e) => {
      this._renderList(e.target.value);
    });

    // 4. 載入資料
    await this._loadData();
  },

  async _loadData() {
    const listRoot = document.getElementById('code-list-container');
    if (!listRoot) return;

    listRoot.innerHTML = '<div style="text-align:center; padding:40px; color:var(--text-muted);">載入資料中…</div>';

    try {
      const [
        members, banks, bankAccounts, categories, items, 
        payments, companies, policies, funds
      ] = await Promise.all([
        getMembersOnce(), getBanksOnce(), getBankAccountsOnce(),
        getCategoriesOnce(), getItemsOnce(), getPaymentMethodsOnce(),
        getInsuranceCompaniesOnce(), getInsurancePoliciesOnce(), getFundsOnce()
      ]);

      this._allRows = [];

      const _pushRows = (dataList, entityType, entityLabel) => {
        (dataList || []).forEach(item => {
          this._allRows.push({
            entityType,
            entityLabel,
            id: item.id,
            code: item.code || '',
            name: item.name || '（未命名）'
          });
        });
      };

      _pushRows(members, 'member', '成員');
      _pushRows(banks, 'bank', '銀行');
      _pushRows(bankAccounts, 'bankAccount', '銀行帳號');
      _pushRows(categories, 'category', '支出類別');
      _pushRows(items, 'item', '支出項目');
      _pushRows(payments, 'payment', '支付方式');
      _pushRows(companies, 'company', '保險公司');
      _pushRows(policies, 'policy', '保單');
      _pushRows(funds, 'fund', '基金');

      this._allRows.sort((a, b) => {
        if (a.code && b.code) return a.code.localeCompare(b.code);
        if (a.code) return -1;
        if (b.code) return 1;
        return a.name.localeCompare(b.name, 'zh-HK');
      });

      this._renderList('all');

    } catch (err) {
      console.error('[code-list] 載入失敗：', err);
      showToast('載入編號清單失敗', 'error');
      listRoot.innerHTML = '<div style="color:var(--accent-danger); text-align:center; padding:20px;">載入失敗</div>';
    }
  },

  _renderList(filterType = 'all') {
    const container = document.getElementById('code-list-container');
    if (!container) return;

    let displayData = this._allRows;
    if (filterType !== 'all') {
      displayData = displayData.filter(r => r.entityType === filterType);
    }

    if (displayData.length === 0) {
      container.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">無符合條件的資料</div>';
      return;
    }

    const currentView = AppState.getCurrentView() || 'table';

    if (currentView === 'card') {
      renderCards({ container, rows: displayData, columnsKey: 'codeList', emptyText: '尚無資料' });
    } else {
      renderTable({ container, rows: displayData, columnsKey: 'codeList', emptyText: '尚無資料' });
    }
  },

  destroy() {}
};

/* ═══ EOF ═══
F: pages/code-list.js
V: 1792901050
W: ai-system-20261024-012#7
═══ EOF ═══ */```

<!-- === FILE END: ./pages/code-list.js === -->

<!-- === FILE START: ./pages/dashboard.js === -->

## 📄 ./pages/dashboard.js

```js
// ============================================
// dashboard.js — 總覽儀表板
// 位置：pages/dashboard.js
// 說明：顯示年度統計與前後 2 年的總覽表格。
//       已完全套用 renderPageLayout 解決排版問題。
// ============================================

import { AppState } from '../config/state.js';
import { api } from '../domain/api-client.js';
import { showToast } from '../ui/toast.js';
import { formatHKD, esc } from '../ui/format.js';
import { renderTable } from '../ui/table.js';
import { renderPageLayout } from '../ui/layout.js';

export default {
  _unsubYM: null,

  async render() {
    // 1. 準備 Header Actions
    const headerActions = `
      <a href="#/annual-report" class="btn btn-primary" style="display:inline-flex; align-items:center; gap:6px; text-decoration:none;">
        <i data-lucide="bar-chart-3" style="width:16px;height:16px;"></i> 查看年度報表
      </a>
    `;

    // 2. 呼叫統一引擎繪製骨架 (先傳入空 stats，載入後再更新)
    renderPageLayout({
      title: '總覽儀表板',
      headerActions,
      stats: [],
      listTitle: '年度總覽（前後 2 年）',
      listContainerId: 'dashboard-annual-table'
    });

    this._unsubYM = AppState.on('ym-change', () => {
      this._loadData();
    });

    await this._loadData();
  },

  async _loadData() {
    const { year } = AppState.getYearMonth();
    const statsRoot = document.getElementById('page-stats-container');
    const tableRoot = document.getElementById('dashboard-annual-table');
    
    if (!statsRoot || !tableRoot) return;

    tableRoot.innerHTML = '<div style="padding:20px; text-align:center; color:var(--text-muted);">載入年度資料…</div>';

    try {
      const y = Number(year);
      const annualRes = await api.fetchAnnualSummaryRange(y - 2, y + 2);
      
      const currentYearData = (annualRes.years || []).find(d => d.year === y) || {
        totalIncome: 0, totalExpense: 0, netBalance: 0, bankBalance: 0, fundValue: 0, monthly: []
      };

      const monthCount = (currentYearData.monthly || []).filter(m => m.totalExpense > 0).length;

      // 更新統計卡 (使用統一引擎結構)
      this._updateStats(statsRoot, currentYearData, year, monthCount);

      // 渲染表格
      if (annualRes && annualRes.years) {
        const tableRows = annualRes.years.map(yData => ({
          year: yData.year,
          totalIncome: yData.totalIncome,
          totalExpense: yData.totalExpense,
          insurance: yData.yearlyInsuranceTotal || 0,
          net: yData.netBalance,
          isCurrent: yData.year === y
        }));

        renderTable({
          container: 'dashboard-annual-table',
          rows: tableRows,
          columnsKey: 'dashboardAnnual',
          emptyText: '無年度資料'
        });
      } else {
        tableRoot.innerHTML = '<div style="color:var(--text-muted); text-align:center; padding:20px;">無年度資料</div>';
      }

      if (window.lucide) window.lucide.createIcons();
    } catch (err) {
      console.error('[dashboard] 載入失敗：', err);
      showToast('載入總覽資料失敗', 'error');
      statsRoot.innerHTML = '<div style="color:var(--accent-danger); padding:20px; grid-column:1/-1; text-align:center;">載入失敗</div>';
      tableRoot.innerHTML = '';
    }
  },

  _updateStats(statsRoot, data, year, monthCount) {
    statsRoot.style.display = 'grid';
    // 由於 layout.js 已經設定了 grid，這裡只需塞入子元素
    const totalAssets = (data.bankBalance || 0) + (data.fundValue || 0);

    statsRoot.innerHTML = [
      { 
        title: '家庭總餘額', 
        val: totalAssets, 
        color: totalAssets >= 0 ? 'var(--accent-success)' : 'var(--accent-danger)', 
        icon: 'landmark', 
        hint: `共 1 個銀行帳號` // 假設性提示
      },
      { 
        title: `${year} 家庭收入`, 
        val: data.totalIncome || 0, 
        color: 'var(--accent-success)', 
        icon: 'trending-up', 
        hint: '家用轉入 + 額外收入' 
      },
      { 
        title: `${year} 年度總支出`, 
        val: data.totalExpense || 0, 
        color: 'var(--accent-danger)', 
        icon: 'trending-down', 
        hint: `${monthCount} 個月有紀錄` 
      },
      { 
        title: `${year} 年度淨餘額`, 
        val: data.netBalance || 0, 
        color: (data.netBalance || 0) >= 0 ? 'var(--accent-success)' : 'var(--accent-danger)', 
        icon: 'wallet', 
        hint: (data.netBalance || 0) >= 0 ? '收支平衡' : '支出大於收入' 
      }
    ].map(card => `
      <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
        <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
          <i data-lucide="${card.icon}" style="width:14px;height:14px; flex-shrink:0;"></i> 
          <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${card.title}</span>
        </div>
        <div class="mono" style="font-size:20px; font-weight:700; color:${card.color}; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
          ${formatHKD(card.val)}
        </div>
        <div style="font-size:11px; color:var(--text-muted); overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">
          ${esc(card.hint)}
        </div>
      </div>
    `).join('');
  },

  destroy() {
    if (this._unsubYM) {
      try { this._unsubYM(); } catch (e) { /* noop */ }
      this._unsubYM = null;
    }
  }
};

/* ═══ EOF ═══
F: pages/dashboard.js
V: 1792901890
W: webapp-20261025-001#37
═══ */```

<!-- === FILE END: ./pages/dashboard.js === -->

<!-- === FILE START: ./pages/input-center.js === -->

## 📄 ./pages/input-center.js

```js
// ============================================
// input-center.js — 綜合輸入中心
// 位置：pages/input-center.js
// 說明：整合新增操作與各類別清單預覽。
// ============================================

import { AppState } from '../config/state.js';
import { 
  listenAllExpenses, 
  getInsurancePoliciesOnce, 
  getFundsOnce, 
  getBankAccountsOnce 
} from '../db/index.js';
import { saveIncome, savePersonalIncome, removeExpense } from '../db/write.js';
import { getList as getMembersList } from '../config/entity-registry.js';
import { renderTable, renderCards } from '../ui/table.js';
import { renderViewToggle, renderYearMonth } from '../ui/controls.js';
import { renderPageLayout } from '../ui/layout.js';
import { openModal, closeModal, openConfirm } from '../ui/modal.js';
import { createForm } from '../ui/form.js';
import { showToast } from '../ui/toast.js';
import { todayISO, formatHKD, esc } from '../ui/format.js';
import { RESERVED_IDS } from '../config/constants.js';
import { openExpenseModal } from '../lib/expense-modal.js';

export default {
  _unsubExpenses: null,
  _expenses: [],
  _activeTab: 'expense', // expense | insurance | fund | bank
  _otherData: { insurance: [], fund: [], bank: [] },

  async render() {
    // 1. 準備 Header Actions (新增按鈕)
    const headerActions = `
      <div style="display:flex; flex-wrap:wrap; gap:12px;">
        <button type="button" class="btn btn-primary" id="btn-add-expense">
          <i data-lucide="plus" style="width:16px;height:16px;"></i> 新增支出
        </button>
        <button type="button" class="btn btn-ghost" id="btn-add-contribution">
          <i data-lucide="trending-up" style="width:16px;height:16px;"></i> 家用轉入
        </button>
        <button type="button" class="btn btn-ghost" id="btn-add-income">
          <i data-lucide="wallet" style="width:16px;height:16px;"></i> 個人收入
        </button>
      </div>
    `;

    // 2. 準備控制列 (Tab + 年月 + 檢視切換)
    const controlsHtml = `
      <div id="ic-tabs" style="display:flex; align-items:center; gap:8px; overflow-x:auto; padding-bottom:4px; width:100%;">
        <button type="button" class="btn" data-tab="expense">支出</button>
        <button type="button" class="btn" data-tab="insurance">保險</button>
        <button type="button" class="btn" data-tab="fund">基金</button>
        <button type="button" class="btn" data-tab="bank">銀行</button>
      </div>
      <div style="display:flex; align-items:center; gap:12px; margin-top:12px; flex-wrap:wrap;">
        <div id="ic-view-toggle"></div>
        <div id="ic-controls-period"></div>
      </div>
    `;

    // 3. 呼叫統一引擎繪製骨架
    renderPageLayout({
      title: '綜合輸入中心',
      headerActions,
      stats: [],
      controls: controlsHtml,
      listTitle: '資料清單',
      listContainerId: 'ic-content-container'
    });

    // 4. 初始化 Tab 與按鈕狀態
    this._updateTabStyles();
    
    // 綁定 Tab 事件
    document.getElementById('ic-tabs')?.addEventListener('click', (e) => {
      const btn = e.target.closest('button[data-tab]');
      if (!btn) return;
      this._activeTab = btn.dataset.tab;
      this._updateTabStyles();
      this._renderCurrentTab();
    });

    // 5. 渲染年月與切換器
    renderYearMonth('ic-controls-period', { includeAllMonth: true }, () => {
      this._renderCurrentTab();
    });

    renderViewToggle('ic-view-toggle', () => {
      this._renderCurrentTab();
    });

    // 6. 綁定操作按鈕（加入 try-catch 以捕捉報錯）
    const btnExpense = document.getElementById('btn-add-expense');
    if (btnExpense) {
      btnExpense.onclick = async () => {
        try {
          await openExpenseModal({ onSuccess: () => this._renderExpenseTab() });
        } catch (err) {
          console.error(err);
          alert('開啟新增支出失敗，錯誤訊息：\n' + err.message + '\n\n請將此訊息截圖回報。');
        }
      };
    }

    const btnContrib = document.getElementById('btn-add-contribution');
    if (btnContrib) {
      btnContrib.onclick = () => this._openIncomeModal('contribution');
    }

    const btnIncome = document.getElementById('btn-add-income');
    if (btnIncome) {
      btnIncome.onclick = () => this._openIncomeModal('personal');
    }

    // 7. 載入資料
    const { year } = AppState.getYearMonth();
    this._unsubExpenses = listenAllExpenses((list) => {
      this._expenses = list.filter(e => e.year === String(year));
      if (this._activeTab === 'expense') this._renderExpenseTab();
    });

    // 背景載入其他 Tab 的資料
    this._loadOtherTabsData();
  },

  _updateTabStyles() {
    const tabsRoot = document.getElementById('ic-tabs');
    if (!tabsRoot) return;
    
    tabsRoot.querySelectorAll('button[data-tab]').forEach(btn => {
      const isActive = btn.dataset.tab === this._activeTab;
      btn.className = isActive ? 'btn btn-primary' : 'btn btn-ghost';
    });
  },

  async _loadOtherTabsData() {
    try {
      this._otherData.insurance = await getInsurancePoliciesOnce();
      this._otherData.fund = await getFundsOnce();
      this._otherData.bank = await getBankAccountsOnce();
      if (this._activeTab !== 'expense') this._renderCurrentTab();
    } catch (e) {
      console.error('[input-center] 載入其他資料失敗：', e);
    }
  },

  _renderCurrentTab() {
    if (this._activeTab === 'expense') this._renderExpenseTab();
    else if (this._activeTab === 'insurance') this._renderInsuranceTab();
    else if (this._activeTab === 'fund') this._renderFundTab();
    else if (this._activeTab === 'bank') this._renderBankTab();
  },

  _renderExpenseTab() {
    const listRoot = document.getElementById('ic-content-container');
    if (!listRoot) return;
    const titleEl = document.getElementById('page-list-title') || document.querySelector('h2');
    if (titleEl) titleEl.textContent = '最近支出紀錄';

    const { year, month } = AppState.getYearMonth();
    let displayData = this._expenses.filter(e => e.year === String(year));
    if (month !== 'all') {
      const mm = String(month).padStart(2, '0');
      displayData = displayData.filter(e => e.month === mm);
    }
    displayData.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0));

    if (displayData.length === 0) {
      listRoot.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">此期間尚無支出紀錄</div>';
      return;
    }

    const renderActions = (row) => `
      <button type="button" class="btn btn-sm btn-ghost" data-action="edit" data-id="${row.id}" data-member="${row.memberId}">編輯</button>
      <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-id="${row.id}" data-member="${row.memberId}">刪除</button>
    `;

    const currentView = AppState.getCurrentView() || 'table';
    if (currentView === 'card') {
      renderCards({ container: listRoot, rows: displayData, columnsKey: 'recentExpense', renderActions });
    } else {
      renderTable({ container: listRoot, rows: displayData, columnsKey: 'recentExpense', renderActions });
    }

    // 改用 onclick 屬性賦值，覆蓋舊監聽，避免多次切換 Tab 時累積事件
    listRoot.onclick = async (e) => {
      const btn = e.target.closest('button[data-action]');
      if (!btn) return;
      const id = btn.dataset.id;
      const memberId = btn.dataset.member;
      const expense = displayData.find(x => x.id === id);
      if (!expense) return;

      if (btn.dataset.action === 'delete') {
        const ok = await openConfirm(`確定要刪除「${expense.name}」嗎？`, { title: '刪除支出', okClass: 'btn-danger' });
        if (!ok) return;
        try {
          await removeExpense(expense.year, expense.month, memberId, id);
          showToast('✅ 已刪除支出', 'success');
        } catch (err) { showToast('刪除失敗：' + err.message, 'error'); }
      } else if (btn.dataset.action === 'edit') {
        try {
          await openExpenseModal({ row: expense, onSuccess: () => this._renderExpenseTab() });
        } catch (err) {
          console.error(err);
          alert('開啟編輯支出失敗，錯誤訊息：\n' + err.message);
        }
      }
    };
  },

  _renderInsuranceTab() {
    const listRoot = document.getElementById('ic-content-container');
    if (!listRoot) return;
    const titleEl = document.getElementById('page-list-title') || document.querySelector('h2');
    if (titleEl) titleEl.textContent = '保險清單 (唯讀預覽)';

    const data = this._otherData.insurance;
    if (data.length === 0) {
      listRoot.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">尚無保險資料</div>';
      return;
    }

    const currentView = AppState.getCurrentView() || 'table';
    if (currentView === 'card') {
      renderCards({ container: listRoot, rows: data, columnsKey: 'insurancePolicies' });
    } else {
      renderTable({ container: listRoot, rows: data, columnsKey: 'insurancePolicies' });
    }
  },

  _renderFundTab() {
    const listRoot = document.getElementById('ic-content-container');
    if (!listRoot) return;
    const titleEl = document.getElementById('page-list-title') || document.querySelector('h2');
    if (titleEl) titleEl.textContent = '基金清單 (唯讀預覽)';

    const data = this._otherData.fund;
    if (data.length === 0) {
      listRoot.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">尚無基金資料</div>';
      return;
    }

    const currentView = AppState.getCurrentView() || 'table';
    if (currentView === 'card') {
      renderCards({ container: listRoot, rows: data, columnsKey: 'funds' });
    } else {
      renderTable({ container: listRoot, rows: data, columnsKey: 'funds' });
    }
  },

  _renderBankTab() {
    const listRoot = document.getElementById('ic-content-container');
    if (!listRoot) return;
    const titleEl = document.getElementById('page-list-title') || document.querySelector('h2');
    if (titleEl) titleEl.textContent = '銀行帳號 (唯讀預覽)';

    const data = this._otherData.bank;
    if (data.length === 0) {
      listRoot.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">尚無銀行帳號</div>';
      return;
    }

    const currentView = AppState.getCurrentView() || 'table';
    if (currentView === 'card') {
      renderCards({ container: listRoot, rows: data, columnsKey: 'bankAccounts' });
    } else {
      renderTable({ container: listRoot, rows: data, columnsKey: 'bankAccounts' });
    }
  },

  _openIncomeModal(type) {
    const modalId = 'add-income-modal';
    document.getElementById(modalId)?.remove();

    const isContrib = type === 'contribution';
    const members = getMembersList('members');
    const memberOpts = members.map(m => ({ value: m.id, label: m.name }));
    if (isContrib) memberOpts.push({ value: RESERVED_IDS.EXTRA_INCOME, label: '額外收入' });

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `
      <div class="modal" style="max-width:400px; background:var(--bg-surface);">
        <h2 style="font-size:18px; font-weight:700; margin-bottom:16px; color:var(--text-primary);">${isContrib ? '家用轉入' : '個人收入'}</h2>
        <div id="income-form-root"></div>
      </div>
    `;
    document.body.appendChild(overlay);
    openModal(modalId);

    createForm({
      container: 'income-form-root',
      fields: [
        { id: 'memberId', label: '成員', type: 'select', required: true, options: memberOpts },
        { id: 'amount', label: '金額', type: 'number', required: true }
      ],
      submitText: '儲存',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        try {
          const { year, month } = AppState.getYearMonth();
          const y = year;
          const m = month === 'all' ? String(new Date().getMonth() + 1).padStart(2, '0') : month;
          if (isContrib) await saveIncome(y, m, { [data.memberId]: Number(data.amount) });
          else await savePersonalIncome(data.memberId, y, m, Number(data.amount));
          showToast(`✅ 已新增${isContrib ? '家用轉入' : '個人收入'}`, 'success');
          closeModal(modalId);
          overlay.remove();
        } catch (err) { showToast('新增失敗：' + err.message, 'error'); }
      }
    });
  },

  destroy() {
    if (this._unsubExpenses) { try { this._unsubExpenses(); } catch (e) {} this._unsubExpenses = null; }
  }
};

/* ═══ EOF ═══
F: pages/input-center.js
V: 1792901740
W: webapp-20261025-001#24
═══ */```

<!-- === FILE END: ./pages/input-center.js === -->

<!-- === FILE START: ./pages/insurance.js === -->

## 📄 ./pages/insurance.js

```js
// ============================================
// insurance.js — 保險清單表
// 位置：pages/insurance.js
// 說明：顯示保單清單與明細，包含已供滿區塊。
//       已移除 entity-helpers 幽靈依賴，全面使用 V2 API。
// ============================================

import { AppState } from '../config/state.js';
import { listenInsurancePolicies, getInsurancePaymentsOnce } from '../db/index.js';
import { api } from '../domain/api-client.js';
import { computeEnrichedPolicies, getPolicyHolderId } from '../domain/insurance.js';
import { renderTable, renderCards } from '../ui/table.js';
import { renderViewToggle, renderYearMonth } from '../ui/controls.js';
import { renderPageLayout } from '../ui/layout.js'; 
import { openModal, closeModal, openConfirm } from '../ui/modal.js';
import { createForm } from '../ui/form.js';
import { showToast } from '../ui/toast.js';
import { formatHKD, esc } from '../ui/format.js';
import { getMembersOnce } from '../db/index.js';
import { resolveName } from '../config/entity-registry.js';

export default {
  _unsubPolicies: null,
  _policies: [],
  _payments: {},

  async render() {
    // 1. 準備 Header Actions
    const headerActions = `
      <button type="button" class="btn btn-primary" id="btn-add-policy">
        <i data-lucide="plus" style="width:16px;height:16px;"></i> 新增保單
      </button>
    `;

    // 2. 準備控制列
    const controlsHtml = `
      <div style="display:flex; align-items:center; gap:12px; flex-wrap:wrap;">
        <div id="insurance-controls-view"></div>
      </div>
      <div id="insurance-controls-period"></div>
    `;

    // 3. 繪製骨架
    renderPageLayout({
      title: '保險清單表',
      headerActions,
      stats: [], 
      controls: controlsHtml,
      listContainerId: 'insurance-list-container'
    });

    // 4. 渲染控制列事件
    renderYearMonth('insurance-controls-period', { includeAllMonth: false }, () => {
      this._renderListAndStats();
    });
    
    renderViewToggle('insurance-controls-view', () => {
      this._renderListAndStats();
    });
    
    document.getElementById('btn-add-policy')?.addEventListener('click', () => {
      this._openPolicyModal();
    });

    // 5. 監聽資料
    this._unsubPolicies = listenInsurancePolicies(async (policies) => {
      this._policies = policies;
      await this._loadPaymentsAndRender();
    });
  },

  async _loadPaymentsAndRender() {
    const paymentsCache = {};
    for (const p of this._policies) {
      try { paymentsCache[p.id] = await getInsurancePaymentsOnce(p.id); } catch (e) { paymentsCache[p.id] = {}; }
    }
    this._payments = paymentsCache;
    this._renderListAndStats();
  },

  _renderListAndStats() {
    const listRoot = document.getElementById('insurance-list-container');
    const statsRoot = document.getElementById('page-stats-container');
    if (!listRoot) return;

    const { year } = AppState.getYearMonth();
    const targetYear = Number(year) || new Date().getFullYear();
    const currentView = AppState.getCurrentView() || 'table';

    const enriched = computeEnrichedPolicies(this._policies, this._payments, targetYear);
    const active = enriched.filter(p => !p._isCompleted);
    const completed = enriched.filter(p => p._isCompleted);

    const totalAnnual = enriched.reduce((s, p) => s + (p._currentAnnualPremium || 0), 0);
    const totalPaid = enriched.reduce((s, p) => s + (p._paidTotal || 0), 0);

    // 更新統計卡 (防破版樣式)
    if (statsRoot) {
      statsRoot.style.display = 'grid';
      statsRoot.innerHTML = [
        { title: `${targetYear} 年度保單總供款`, val: formatHKD(totalAnnual), color: 'var(--primary)', icon: 'calendar', hint: `共 ${enriched.length} 張` },
        { title: '累計已供款總額', val: formatHKD(totalPaid), color: 'var(--accent-success)', icon: 'check-circle', hint: '已扣款總和' },
        { title: '供款中保單', val: `${active.length} 張`, color: 'var(--text-primary)', icon: 'shield', hint: '未供滿' },
        { title: '已供滿保單', val: `${completed.length} 張`, color: 'var(--accent-success)', icon: 'check-circle', hint: '已完成' }
      ].map(card => `
        <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
          <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
            <i data-lucide="${card.icon}" style="width:14px;height:14px; flex-shrink:0;"></i> 
            <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${card.title}</span>
          </div>
          <div class="mono" style="font-size:20px; font-weight:700; color:${card.color}; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
            ${esc(card.val)}
          </div>
          <div style="font-size:11px; color:var(--text-muted); overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${esc(card.hint)}</div>
        </div>
      `).join('');
    }

    // 組合：已供滿區塊 + 列表區塊
    let html = '';

    if (completed.length > 0) {
      html += `
        <div class="glass-card" style="margin-bottom:20px; border-color:rgba(16,185,129,0.3); background:rgba(16,185,129,0.04);">
          <div style="padding:14px 20px; display:flex; justify-content:space-between; align-items:center; cursor:pointer;" id="completed-header">
            <div style="display:flex; align-items:center; gap:8px;">
              <i data-lucide="check-circle" style="width:18px;height:18px;color:var(--accent-success);"></i>
              <span style="font-weight:700; color:var(--accent-success);">已供滿保單</span>
              <span class="badge badge-success">${completed.length}</span>
            </div>
            <i data-lucide="chevron-down" id="completed-arrow" style="transition:transform 0.3s;"></i>
          </div>
          <div id="completed-body" style="display:none; padding:0 20px 20px; border-top:1px dashed rgba(16,185,129,0.2);">
            <div class="data-cards-grid" style="display:grid; grid-template-columns:repeat(auto-fill, minmax(280px, 1fr)); gap:16px; padding-top:16px;">
              ${completed.map(p => this._cardHtml(p)).join('')}
            </div>
          </div>
        </div>
      `;
    }

    html += `
      <div style="margin-bottom:16px;">
        <h2 style="font-size:16px; color:var(--text-secondary);">供款中保單 (${active.length})</h2>
      </div>
      <div id="active-list-container"></div>
    `;
    
    listRoot.innerHTML = html;

    // 綁定折疊事件
    const header = document.getElementById('completed-header');
    const body = document.getElementById('completed-body');
    const arrow = document.getElementById('completed-arrow');
    if (header && body) {
      header.onclick = () => {
        const isOpen = body.style.display === 'block';
        body.style.display = isOpen ? 'none' : 'block';
        if (arrow) arrow.style.transform = isOpen ? 'rotate(0deg)' : 'rotate(180deg)';
      };
    }

    // 渲染供款中列表
    const activeRoot = document.getElementById('active-list-container');
    if (activeRoot) {
      if (active.length === 0) {
        activeRoot.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">所有保單均已供滿</div>';
      } else {
        const renderActions = (row) => `
          <button type="button" class="btn btn-sm btn-ghost" data-action="edit" data-id="${row.id}">編輯</button>
          <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-id="${row.id}">刪除</button>
        `;

        if (currentView === 'card') {
          renderCards({ container: activeRoot, rows: active, columnsKey: 'insurancePolicies', renderActions });
        } else {
          renderTable({ container: activeRoot, rows: active, columnsKey: 'insurancePolicies', renderActions });
        }

        activeRoot.onclick = (e) => {
          const btn = e.target.closest('button[data-action]');
          if (!btn) return;
          const id = btn.dataset.id;
          const policy = this._policies.find(p => p.id === id);
          if (!policy) return;
          if (btn.dataset.action === 'edit') this._openPolicyModal(policy);
          else if (btn.dataset.action === 'delete') this._deletePolicy(policy);
        };
      }
    }

    // 綁定已供滿卡片的事件
    listRoot.onclick = (e) => {
      const btn = e.target.closest('button[data-action]');
      if (!btn) return;
      if (btn.closest('#active-list-container')) return; // 避開供款中列表
      
      const id = btn.dataset.id;
      const policy = this._policies.find(p => p.id === id);
      if (!policy) return;
      if (btn.dataset.action === 'edit') this._openPolicyModal(policy);
      else if (btn.dataset.action === 'delete') this._deletePolicy(policy);
    };

    if (window.lucide) window.lucide.createIcons();
  },

  _cardHtml(p) {
    const holderId = getPolicyHolderId(p);
    const holder = resolveName('members', holderId) || holderId || '—';
    const insured = resolveName('members', p.memberId) || p.memberId || '—';
    const isFund = p.type === 'fund_insurance';
    
    const actions = `
      <div style="display:flex; gap:8px; justify-content:flex-end; margin-top:12px;">
        <button type="button" class="btn btn-sm btn-ghost" data-action="edit" data-id="${p.id}">編輯</button>
        <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-id="${p.id}">刪除</button>
      </div>
    `;

    return `
      <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:12px;">
        <div>
          <div style="font-size:15px; font-weight:700; color:var(--primary); line-height:1.3;">${esc(p.name)}</div>
          <div style="font-size:11px; color:var(--accent-warm); margin-top:4px;">${esc(p.company || '')} · ${isFund ? '基金保險' : '普通保險'}</div>
        </div>
        <div style="display:flex; flex-direction:column; gap:6px; font-size:13px;">
          <div style="display:flex; justify-content:space-between;"><span style="color:var(--text-muted);">持有人</span><span>${esc(holder)}</span></div>
          <div style="display:flex; justify-content:space-between;"><span style="color:var(--text-muted);">受保人</span><span>${esc(insured)}</span></div>
          <div style="display:flex; justify-content:space-between; border-top:1px dashed var(--border-color); padding-top:6px;"><span style="color:var(--text-muted);">本期年繳</span><span class="mono text-primary">${formatHKD(p._currentAnnualPremium)}</span></div>
          <div style="display:flex; justify-content:space-between;"><span style="color:var(--text-muted);">已供款</span><span class="mono text-emerald">${formatHKD(p._paidTotal)}</span></div>
        </div>
        ${actions}
      </div>
    `;
  },

  async _openPolicyModal(policy = null) {
    const isEdit = !!policy;
    const modalId = 'policy-modal';
    document.getElementById(modalId)?.remove();

    const members = await getMembersOnce();
    const memberOpts = members.map(m => ({ value: m.id, label: m.name }));

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `
      <div class="modal" style="max-width:600px; background:var(--bg-surface); max-height:90vh; overflow-y:auto;">
        <h2 class="modal-title">${isEdit ? '編輯' : '新增'}保單</h2>
        <div id="policy-form-root"></div>
      </div>
    `;
    document.body.appendChild(overlay);
    openModal(modalId);

    const fields = [
      { id: 'name', label: '保單名稱', type: 'text', required: true },
      { id: 'company', label: '保險公司', type: 'text' },
      { id: 'type', label: '保單類型', type: 'select', required: true, options: [
        { value: 'normal', label: '普通保險' },
        { value: 'fund_insurance', label: '基金保險' }
      ]},
      { id: 'memberId', label: '受保人', type: 'select', required: true, options: memberOpts },
      { id: 'policyHolderId', label: '持有人 (留空同受保人)', type: 'select', options: memberOpts },
      { id: 'firstStartYear', label: '開始年份', type: 'number', required: true },
      { id: 'firstStartMonth', label: '開始月份', type: 'number', required: true, min: 1, max: 12 },
      { id: 'totalPolicyYears', label: '總供款年期', type: 'number', required: true },
      { id: 'annualPremium', label: '年繳保費 (普通保險)', type: 'number' },
      { id: 'monthlyPremium', label: '月繳金額 (基金保險)', type: 'number' },
      { id: 'paymentMode', label: '付款模式', type: 'select', required: true, options: [
        { value: 'direct', label: '直接付款' },
        { value: 'advance', label: '代墊模式' }
      ]}
    ];

    createForm({
      container: 'policy-form-root',
      fields,
      initialData: isEdit ? { ...policy } : { type: 'normal', firstStartYear: new Date().getFullYear(), firstStartMonth: 1, paymentMode: 'direct' },
      submitText: isEdit ? '儲存' : '新增',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        try {
          const payload = {
            ...data,
            firstStartYear: Number(data.firstStartYear),
            firstStartMonth: String(data.firstStartMonth).padStart(2, '0'),
            totalPolicyYears: Number(data.totalPolicyYears),
            annualPremium: Number(data.annualPremium) || 0,
            monthlyPremium: Number(data.monthlyPremium) || 0,
            totalPolicyPeriods: Number(data.totalPolicyYears) * 12,
            policyHolderId: data.policyHolderId || data.memberId
          };

          if (isEdit) {
            await api.entityCreate({ action: 'update', entityType: 'policy', id: policy.id, name: data.name, extra: payload });
            showToast('✅ 保單已更新', 'success');
          } else {
            await api.entityCreate({ action: 'create', entityType: 'policy', code: 'POL-TEMP', name: data.name, extra: payload });
            showToast('✅ 保單已新增', 'success');
          }
          closeModal(modalId);
          overlay.remove();
        } catch (err) {
          showToast('操作失敗：' + err.message, 'error');
        }
      }
    });
  },

  async _deletePolicy(policy) {
    const ok = await openConfirm(`⚠️ 確定要刪除保單「${policy.name}」嗎？`, { title: '刪除保單', okText: '刪除', okClass: 'btn-danger' });
    if (!ok) return;
    try {
      // 🆕 移除 entity-helpers，改用 API
      await api.entityCreate({ action: 'delete', entityType: 'policy', id: policy.id });
      showToast('✅ 已刪除保單', 'success');
    } catch (err) {
      showToast('刪除失敗：' + err.message, 'error');
    }
  },

  destroy() {
    if (this._unsubPolicies) { try { this._unsubPolicies(); } catch (e) {} this._unsubPolicies = null; }
  }
};

/* ═══ EOF ═══
F: pages/insurance.js
V: 1792901960
W: webapp-20261025-001#44
═══ */```

<!-- === FILE END: ./pages/insurance.js === -->

<!-- === FILE START: ./pages/login.js === -->

## 📄 ./pages/login.js

```js
// ============================================
// login.js — 登入頁面（V2 重構版）
// ============================================

import { loginWithCustomAccount } from '../domain/auth.js';
import { showToast } from '../ui/toast.js';
import { esc } from '../ui/format.js';

export default {
  async render() {
    // 隱藏選單
    const sidebar = document.querySelector('.sidebar');
    const navbar = document.querySelector('.navbar');
    if (sidebar) sidebar.style.display = 'none';
    if (navbar) navbar.style.display = 'none';

    const content = document.getElementById('app-content');
    if (!content) return;

    content.innerHTML = `
      <div style="display:flex; align-items:center; justify-content:center; min-height:80vh; padding:20px;">
        <div class="glass-card" style="width:100%; max-width:400px; padding:32px; background:var(--bg-surface);">
          <div style="text-align:center; margin-bottom:24px;">
            <h1 style="font-family:var(--font-mono); font-size:28px; font-weight:700; color:var(--primary); letter-spacing:2px;">◈ FAMILY.FIN</h1>
            <p style="font-size:13px; color:var(--text-muted); margin-top:8px;">家庭財務與固定供款管理 (V2)</p>
          </div>

          <form id="login-form" novalidate>
            <div class="field" style="margin-bottom:16px;">
              <label class="field-label" for="account" style="display:block; font-size:12px; color:var(--text-secondary); margin-bottom:6px;">帳號</label>
              <input class="input" type="text" id="account" required placeholder="例如：superadmin" style="width:100%; padding:10px; background:var(--bg-deep); border:1px solid var(--border-color); border-radius:var(--radius-sm); color:var(--text-primary);">
            </div>
            
            <div class="field" style="margin-bottom:24px;">
              <label class="field-label" for="password" style="display:block; font-size:12px; color:var(--text-secondary); margin-bottom:6px;">密碼</label>
              <input class="input" type="password" id="password" required minlength="6" placeholder="至少 6 位" style="width:100%; padding:10px; background:var(--bg-deep); border:1px solid var(--border-color); border-radius:var(--radius-sm); color:var(--text-primary);">
            </div>

            <div id="login-error" style="display:none; color:var(--accent-danger); font-size:13px; margin-bottom:16px; text-align:center;"></div>

            <button class="btn btn-primary" type="submit" id="login-btn" style="width:100%; justify-content:center; padding:10px;">
              登入
            </button>
          </form>

          <p style="margin-top:24px; font-size:12px; color:var(--text-muted); text-align:center;">
            沒有帳號？請聯繫平台管理員開通
          </p>
        </div>
      </div>
    `;

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
        await loginWithCustomAccount(account, password);
        
        showToast('登入成功，正在載入系統…', 'success');
        setTimeout(() => {
          window.location.reload();
        }, 500);
        
      } catch (err) {
        console.error('[login] 登入失敗：', err);
        errorBox.textContent = '登入失敗：帳號或密碼錯誤';
        errorBox.style.display = 'block';
        btn.disabled = false;
        btn.textContent = '登入';
      }
    });
  },

  destroy() {
    const sidebar = document.querySelector('.sidebar');
    const navbar = document.querySelector('.navbar');
    if (sidebar) sidebar.style.display = 'flex';
    if (navbar) navbar.style.display = 'flex';
  }
};

/* ═══ EOF ═══
F: pages/login.js
V: 1792900670
W: ai-system-20261024-012#57
═══ EOF ═══ */```

<!-- === FILE END: ./pages/login.js === -->

<!-- === FILE START: ./pages/member-report.js === -->

## 📄 ./pages/member-report.js

```js
// ============================================
// member-report.js — 成員收支明細
// 位置：pages/member-report.js
// 說明：顯示各成員的收支與保險平攤狀況。
//       已完全套用 renderPageLayout 解決排版問題。
// ============================================

import { AppState } from '../config/state.js';
import { api } from '../domain/api-client.js';
import { getMembersOnce } from '../db/index.js';
import { renderTable, renderCards } from '../ui/table.js';
import { renderViewToggle, renderYearMonth } from '../ui/controls.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { formatHKD, esc } from '../ui/format.js';
import { RESERVED_IDS } from '../config/constants.js';

export default {
  _data: [],
  _unsubYM: null,

  async render() {
    // 1. 準備控制列 HTML
    const controlsHtml = `
      <div style="display:flex; align-items:center; gap:12px; flex-wrap:wrap;">
        <div id="mr-view-toggle"></div>
      </div>
      <div id="mr-controls-period"></div>
    `;

    // 2. 呼叫統一引擎繪製骨架
    renderPageLayout({
      title: '成員收支明細',
      stats: [], 
      controls: controlsHtml,
      listContainerId: 'mr-list-container'
    });

    // 3. 綁定控制列事件
    this._unsubYM = AppState.on('ym-change', () => this._loadData());

    renderYearMonth('mr-controls-period', { includeAllMonth: true }, () => {
      this._loadData();
    });

    renderViewToggle('mr-view-toggle', () => {
      this._renderListAndStats();
    });

    // 4. 載入資料
    await this._loadData();
  },

  async _loadData() {
    const listRoot = document.getElementById('mr-list-container');
    if (!listRoot) return;

    listRoot.innerHTML = '<div style="text-align:center; padding:40px; color:var(--text-muted);">載入資料中…</div>';

    const { year, month } = AppState.getYearMonth();

    try {
      // 取得當月（或全年）的 summary
      const s = await api.summary(year, month);
      
      // 取得成員列表
      const members = await getMembersOnce();

      // 處理每位成員的資料
      const rows = members.map(m => {
        const mData = s.perMember[m.id] || { sum: 0, items: [] };
        
        // 計算該成員的保險平攤與一般支出
        let insurance = 0;
        let expense = 0;
        mData.items.forEach(item => {
          if (item.isAutoLinked) insurance += item.amount;
          else expense += item.amount;
        });

        // 取得該成員的收入與個人收入
        const income = Number(s.incomeBreakdown?.[m.id]) || 0;
        const personalIncome = Number(s.personalIncomeBreakdown?.[m.id]) || 0;
        
        const net = income + personalIncome - expense - insurance;

        return {
          id: m.id,
          name: m.name,
          income,
          personalIncome,
          expense,
          insurance,
          net
        };
      });

      // 加入「家庭共用」 (如果有支出)
      const sharedData = s.perMember[RESERVED_IDS.SHARED_MEMBER] || { sum: 0, items: [] };
      let sharedIns = 0, sharedExp = 0;
      sharedData.items.forEach(item => {
        if (item.isAutoLinked) sharedIns += item.amount;
        else sharedExp += item.amount;
      });
      
      if (sharedExp > 0 || sharedIns > 0) {
        rows.push({
          id: RESERVED_IDS.SHARED_MEMBER,
          name: '🏠 家庭共用',
          income: 0,
          personalIncome: 0,
          expense: sharedExp,
          insurance: sharedIns,
          net: -(sharedExp + sharedIns)
        });
      }

      this._data = rows;
      this._renderListAndStats();

    } catch (err) {
      console.error('[member-report] 載入失敗：', err);
      showToast('載入成員報表失敗', 'error');
      listRoot.innerHTML = '<div style="color:var(--accent-danger); padding:20px; text-align:center;">載入失敗</div>';
    }
  },

  _renderListAndStats() {
    const listRoot = document.getElementById('mr-list-container');
    const statsRoot = document.getElementById('page-stats-container');
    if (!listRoot || !this._data) return;

    const currentView = AppState.getCurrentView() || 'table';
    const rows = this._data;

    // 更新統計卡 (防破版樣式)
    if (statsRoot) {
      statsRoot.style.display = 'grid';
      
      const tIncome = rows.reduce((sum, r) => sum + r.income, 0);
      const tPersonal = rows.reduce((sum, r) => sum + r.personalIncome, 0);
      const tExpense = rows.reduce((sum, r) => sum + r.expense, 0);
      const tIns = rows.reduce((sum, r) => sum + r.insurance, 0);

      statsRoot.innerHTML = [
        { title: '家庭收入', val: tIncome, color: 'var(--accent-success)', icon: 'trending-up', hint: '家用轉入' },
        { title: '個人收入', val: tPersonal, color: 'var(--primary)', icon: 'wallet', hint: '成員個人' },
        { title: '一般支出', val: tExpense, color: 'var(--accent-danger)', icon: 'trending-down', hint: '包含家庭共用' },
        { title: '保險平攤', val: tIns, color: 'var(--accent-warm)', icon: 'shield', hint: '自動扣款' },
        { title: '淨結餘', val: tIncome + tPersonal - tExpense - tIns, color: (tIncome + tPersonal - tExpense - tIns) >= 0 ? 'var(--accent-success)' : 'var(--accent-danger)', icon: 'calculator', hint: '收入 - 支出' }
      ].map(card => `
        <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
          <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
            <i data-lucide="${card.icon}" style="width:14px;height:14px; flex-shrink:0;"></i> 
            <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${card.title}</span>
          </div>
          <div class="mono" style="font-size:20px; font-weight:700; color:${card.color}; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
            ${formatHKD(card.val)}
          </div>
          <div style="font-size:11px; color:var(--text-muted); overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${esc(card.hint)}</div>
        </div>
      `).join('');
      
      // 如果有 5 張卡片，在手機版上可能 2x3 或 1x5，在桌面版上預設 grid 會自動排。這裡不強制改 grid-template。
      if (window.lucide) window.lucide.createIcons();
    }

    if (rows.length === 0) {
      listRoot.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">尚無資料</div>';
      return;
    }

    if (currentView === 'card') {
      renderCards({ container: listRoot, rows, columnsKey: 'memberReport', emptyText: '尚無資料' });
    } else {
      renderTable({ container: listRoot, rows, columnsKey: 'memberReport', emptyText: '尚無資料' });
    }
  },

  destroy() {
    if (this._unsubYM) { try { this._unsubYM(); } catch (e) {} this._unsubYM = null; }
  }
};

/* ═══ EOF ═══
F: pages/member-report.js
V: 1792901910
W: webapp-20261025-001#39
═══ */```

<!-- === FILE END: ./pages/member-report.js === -->

<!-- === FILE START: ./pages/portfolio.js === -->

## 📄 ./pages/portfolio.js

```js
// ============================================
// portfolio.js — 基金投資表（V2 引擎版）
// 位置：pages/portfolio.js
// 說明：整合保險基金與獨立基金，支援歷史快照與對比。
//       已移除 entity-modal 幽靈依賴，並將動態 import 改為靜態引入。
// ============================================

import { AppState } from '../config/state.js';
import { 
  listenFunds, 
  listenInsurancePolicies, 
  getFundSnapshotListOnce, 
  saveFundSnapshot, 
  removeFundSnapshot,
  saveObject,
  pushObject,
  removeObject
} from '../db/index.js';
import { openModal, closeModal, openConfirm } from '../ui/modal.js';
import { createForm } from '../ui/form.js';
import { showToast } from '../ui/toast.js';
import { renderPageLayout } from '../ui/layout.js';
import { formatHKD, esc } from '../ui/format.js';
import { calcFundCostEstimate } from '../domain/insurance.js';

const DEFAULT_FX = 7.8;

export default {
  _unsubFunds: null,
  _unsubPolicies: null,
  _funds: [],
  _policies: [],
  _snapshots: {},

  async render() {
    // 1. 準備 Header Actions
    const headerActions = `
      <div style="display:flex; gap:12px; flex-wrap:wrap;">
        <button type="button" class="btn btn-primary" id="btn-add-fund">
          <i data-lucide="plus" style="width:16px;height:16px;"></i> 新增基金
        </button>
        <button type="button" class="btn btn-magenta" id="btn-import-snap">
          <i data-lucide="upload" style="width:16px;height:16px;"></i> 導入快照
        </button>
      </div>
    `;

    // 2. 準備控制列（年月）
    const controlsHtml = `<div id="portfolio-controls-period"></div>`;

    // 3. 呼叫統一引擎繪製骨架
    renderPageLayout({
      title: '基金投資表',
      headerActions,
      stats: [],
      controls: controlsHtml,
      listContainerId: 'portfolio-content-container'
    });

    // 4. 綁定按鈕
    document.getElementById('btn-add-fund').onclick = () => this._openFundModal();
    document.getElementById('btn-import-snap').onclick = () => this._openImportSnapshotModal();

    // 5. 初始化年月（僅作顯示用）
    const periodRoot = document.getElementById('portfolio-controls-period');
    if (periodRoot) {
      const ym = AppState.getYearMonth();
      periodRoot.innerHTML = `<span class="badge badge-info">估值基準：${ym.year} 年 ${ym.month === 'all' ? '12' : ym.month} 月</span>`;
    }

    // 6. 訂閱資料
    this._unsubFunds = listenFunds((funds) => {
      this._funds = funds;
      this._loadSnapshotsAndRender();
    });

    this._unsubPolicies = listenInsurancePolicies((policies) => {
      this._policies = policies;
      this._renderContent();
    });
  },

  async _loadSnapshotsAndRender() {
    const snapMap = {};
    for (const f of this._funds) {
      try {
        const snaps = await getFundSnapshotListOnce(f.id);
        snapMap[f.id] = snaps.length > 0 ? snaps[snaps.length - 1] : null;
      } catch (e) { snapMap[f.id] = null; }
    }
    this._snapshots = snapMap;
    this._renderContent();
  },

  _renderContent() {
    const container = document.getElementById('portfolio-content-container');
    if (!container) return;

    const { year, month } = AppState.getYearMonth();
    const y = year || String(new Date().getFullYear());
    const m = month === 'all' ? String(new Date().getMonth() + 1).padStart(2, '0') : month;

    const costMaps = {};
    this._policies.forEach(p => { costMaps[p.id] = _getCostMapForPolicy(p, this._funds, y, m); });

    let totalCost = 0;
    let totalValue = 0;

    const displayFunds = this._funds.map(f => {
      const snap = this._snapshots[f.id];
      const currentValue = snap ? (Number(snap.shares) * Number(snap.nav) * DEFAULT_FX) : (Number(f.currentValue) || 0);
      let estimatedCost = 0;
      
      if (f.type === 'insurance') {
        const policy = _findPolicyByFund(f, this._policies);
        if (policy && costMaps[policy.id]) estimatedCost = costMaps[policy.id][f.id] || 0;
        else estimatedCost = Number(f.cost) || 0;
      } else {
        estimatedCost = Number(f.cost) || 0;
      }
      
      totalCost += estimatedCost;
      totalValue += currentValue;
      
      return { ...f, currentValue: Math.round(currentValue), cost: estimatedCost };
    });

    const totalPnl = totalValue - totalCost;

    // 更新統計卡
    this._updateStats(totalCost, totalValue, totalPnl);

    const insFunds = displayFunds.filter(f => f.type === 'insurance');
    const standaloneFunds = displayFunds.filter(f => f.type !== 'insurance');

    container.innerHTML = `
      <div style="margin-bottom:24px;">
        <div style="display:flex; align-items:center; gap:8px; margin-bottom:12px;">
          <i data-lucide="shield" style="width:16px;height:16px;color:var(--accent-danger);"></i>
          <span style="font-weight:700;color:var(--accent-danger);font-size:14px;">保險基金</span>
          <span class="text-muted" style="font-size:12px;">（掛於基金保險保單，每月自動累積供款）</span>
        </div>
        <div id="ins-funds-list" class="data-cards-grid" style="display:grid; grid-template-columns:repeat(auto-fill, minmax(280px, 1fr)); gap:16px;">
          ${insFunds.length ? insFunds.map(f => this._cardHtml(f, _findPolicyByFund(f, this._policies))).join('') : '<div class="text-muted" style="padding:20px;">尚無保險基金</div>'}
        </div>
      </div>

      <div>
        <div style="display:flex; align-items:center; gap:8px; margin-bottom:12px;">
          <i data-lucide="line-chart" style="width:16px;height:16px;color:var(--primary);"></i>
          <span style="font-weight:700;color:var(--primary);font-size:14px;">獨立基金</span>
        </div>
        <div id="std-funds-list" class="data-cards-grid" style="display:grid; grid-template-columns:repeat(auto-fill, minmax(280px, 1fr)); gap:16px;">
          ${standaloneFunds.length ? standaloneFunds.map(f => this._cardHtml(f, null)).join('') : '<div class="text-muted" style="padding:20px;">尚無獨立基金</div>'}
        </div>
      </div>
    `;

    this._bindCardEvents(container);
    if (window.lucide) window.lucide.createIcons();
  },

  _updateStats(cost, value, pnl) {
    const statsRoot = document.getElementById('page-stats-container');
    if (!statsRoot) return;
    statsRoot.style.display = 'grid';
    
    statsRoot.innerHTML = `
      <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
        <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
          <i data-lucide="wallet" style="width:14px;height:14px; flex-shrink:0;"></i> 
          <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">總投入成本</span>
        </div>
        <div class="mono" style="font-size:20px; font-weight:700; color:var(--text-primary); white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
          ${formatHKD(cost)}
        </div>
      </div>
      <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
        <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
          <i data-lucide="line-chart" style="width:14px;height:14px; flex-shrink:0;"></i> 
          <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">總現時價值</span>
        </div>
        <div class="mono" style="font-size:20px; font-weight:700; color:var(--primary); white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
          ${formatHKD(value)}
        </div>
      </div>
      <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
        <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
          <i data-lucide="trending-up" style="width:14px;height:14px; flex-shrink:0;"></i> 
          <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">總帳面盈虧</span>
        </div>
        <div class="mono" style="font-size:20px; font-weight:700; color:${pnl >= 0 ? 'var(--accent-success)' : 'var(--accent-danger)'}; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
          ${pnl >= 0 ? '+' : ''}${formatHKD(pnl)}
        </div>
      </div>
    `;
  },

  _cardHtml(f, policy) {
    const pnl = f.currentValue - f.cost;
    const pnlCls = pnl >= 0 ? 'text-emerald' : 'text-red';
    const pnlSign = pnl >= 0 ? '+' : '';
    const pnlPct = f.cost > 0 ? ((pnl / f.cost) * 100).toFixed(2) : '0.00';
    
    const meta = policy ? `<div style="font-size:11px; color:var(--text-muted); margin-top:4px;">關聯保單：${esc(policy.name)}</div>` : '';

    return `
      <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:12px;">
        <div>
          <div style="font-size:15px; font-weight:700; color:var(--primary); line-height:1.3;">${esc(f.name)}</div>
          ${meta}
        </div>
        <div style="display:flex; flex-direction:column; gap:6px; font-size:13px;">
          <div style="display:flex; justify-content:space-between;"><span style="color:var(--text-muted);">投入成本</span><span class="mono">${formatHKD(f.cost)}</span></div>
          <div style="display:flex; justify-content:space-between;"><span style="color:var(--text-muted);">現時價值</span><span class="mono text-emerald">${formatHKD(f.currentValue)}</span></div>
          <div style="display:flex; justify-content:space-between; border-top:1px dashed var(--border-color); padding-top:6px;"><span style="color:var(--text-muted);">盈虧</span><span class="mono ${pnlCls}">${pnlSign}${formatHKD(pnl)} (${pnlPct}%)</span></div>
        </div>
        <div style="display:flex; gap:8px; justify-content:flex-end; margin-top:8px;">
          <button type="button" class="btn btn-sm btn-primary" data-action="snap" data-id="${f.id}" data-name="${esc(f.name)}">記錄快照</button>
          <button type="button" class="btn btn-sm btn-ghost" data-action="history" data-id="${f.id}" data-name="${esc(f.name)}">歷史</button>
          <button type="button" class="btn btn-sm btn-ghost" data-action="edit" data-id="${f.id}">編輯</button>
          <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-id="${f.id}">刪除</button>
        </div>
      </div>
    `;
  },

  _bindCardEvents(container) {
    container.onclick = async (e) => {
      const btn = e.target.closest('button[data-action]');
      if (!btn) return;
      const id = btn.dataset.id;
      const name = btn.dataset.name || '';
      const action = btn.dataset.action;

      if (action === 'snap') await this._openSnapshotModal(id, name);
      else if (action === 'history') await this._openHistoryModal(id, name);
      else if (action === 'edit') this._openFundModal(this._funds.find(f => f.id === id));
      else if (action === 'delete') {
        const ok = await openConfirm(`確定要刪除基金「${name}」嗎？`, { okClass: 'btn-danger' });
        if (ok) {
          await removeObject(`funds/${id}`);
          showToast('✅ 已刪除基金', 'success');
        }
      }
    };
  },

  _openFundModal(fund = null) {
    const isEdit = !!fund;
    const modalId = 'fund-modal';
    document.getElementById(modalId)?.remove();

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `<div class="modal" style="max-width:500px; background:var(--bg-surface);"><h2 class="modal-title">${isEdit ? '編輯' : '新增'}基金</h2><div id="${modalId}-root"></div></div>`;
    document.body.appendChild(overlay);
    openModal(modalId);

    createForm({
      container: `${modalId}-root`,
      fields: [
        { id: 'name', label: '基金名稱', type: 'text', required: true },
        { id: 'type', label: '類型', type: 'select', required: true, options: [{ value: 'standalone', label: '獨立基金' }, { value: 'insurance', label: '保險基金' }] },
        { id: 'cost', label: '手動投入成本 (保險基金填0)', type: 'number', required: true },
        { id: 'currentValue', label: '目前現值 (若無快照則用此)', type: 'number' }
      ],
      initialData: isEdit ? fund : { type: 'standalone', cost: 0, currentValue: 0 },
      submitText: isEdit ? '儲存' : '新增',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        if (isEdit) {
          await saveObject(`funds/${fund.id}`, { ...fund, ...data, cost: Number(data.cost) || 0, currentValue: Number(data.currentValue) || 0 });
        } else {
          await pushObject('funds', { ...data, cost: Number(data.cost) || 0, currentValue: Number(data.currentValue) || 0, createdAt: Date.now() });
        }
        showToast('✅ 基金已儲存', 'success');
        closeModal(modalId); overlay.remove();
      }
    });
  },

  async _openSnapshotModal(fundId, fundName) {
    const modalId = 'snap-modal';
    document.getElementById(modalId)?.remove();

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `<div class="modal" style="max-width:500px; background:var(--bg-surface);"><h2 class="modal-title">輸入快照 — ${esc(fundName)}</h2><div id="${modalId}-root"></div></div>`;
    document.body.appendChild(overlay);
    openModal(modalId);

    createForm({
      container: `${modalId}-root`,
      fields: [
        { id: 'dateKey', label: '估值日 (YYYY-MM-DD)', type: 'date', required: true },
        { id: 'shares', label: '單位數目', type: 'number', required: true, step: 0.0001 },
        { id: 'nav', label: '基金價格 (USD)', type: 'number', required: true, step: 0.0001 },
        { id: 'totalUsd', label: '總價值 (USD，自動計算)', type: 'number', disabled: true },
        { id: 'totalHkd', label: '保單價值 (HKD，自動計算)', type: 'number', disabled: true }
      ],
      initialData: { dateKey: new Date().toISOString().slice(0,10), shares: 0, nav: 0 },
      submitText: '儲存快照',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        await saveFundSnapshot(fundId, data.dateKey, { shares: Number(data.shares), nav: Number(data.nav) });
        showToast(`✅ 已儲存 ${data.dateKey} 快照`, 'success');
        closeModal(modalId); overlay.remove();
        this._loadSnapshotsAndRender();
      }
    });

    const formEl = document.querySelector(`#${modalId}-root form`);
    const sEl = formEl.querySelector('#field-shares');
    const nEl = formEl.querySelector('#field-nav');
    const calc = () => {
      const usd = (Number(sEl.value)||0) * (Number(nEl.value)||0);
      formEl.querySelector('#field-totalUsd').value = usd.toFixed(2);
      formEl.querySelector('#field-totalHkd').value = (usd * DEFAULT_FX).toFixed(2);
    };
    sEl.addEventListener('input', calc);
    nEl.addEventListener('input', calc);
  },

  async _openHistoryModal(fundId, fundName) {
    const snaps = await getFundSnapshotListOnce(fundId);
    const modalId = 'history-modal';
    document.getElementById(modalId)?.remove();

    const rows = snaps.length === 0 ? '<tr><td colspan="6" style="text-align:center;">尚無快照</td></tr>' : snaps.map(s => {
      const usd = s.shares * s.nav;
      const hkd = usd * DEFAULT_FX;
      return `<tr>
        <td><input type="checkbox" class="hist-cb" data-date="${s.dateKey}"></td>
        <td>${s.dateKey}</td>
        <td>${s.shares}</td>
        <td>USD ${s.nav}</td>
        <td>HKD ${hkd.toFixed(2)}</td>
        <td><button type="button" class="btn btn-sm btn-danger" data-del="${s.dateKey}" data-raw="${s._rawPath}">刪除</button></td>
      </tr>`;
    }).join('');

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `
      <div class="modal" style="max-width:700px; background:var(--bg-surface);">
        <h2 class="modal-title">歷史快照 — ${esc(fundName)}</h2>
        <div style="margin-bottom:12px;"><button type="button" class="btn btn-sm btn-primary" id="btn-compare" disabled>對比勾選的兩筆</button></div>
        <table class="data-table" style="width:100%; font-size:12px;">
          <thead><tr><th></th><th>日期</th><th>單位數</th><th>價格</th><th>總值(HKD)</th><th>操作</th></tr></thead>
          <tbody id="hist-body">${rows}</tbody>
        </table>
        <div id="compare-result" style="margin-top:16px;"></div>
        <div style="text-align:right; margin-top:16px;"><button type="button" class="btn btn-ghost" id="btn-close-hist">關閉</button></div>
      </div>
    `;
    document.body.appendChild(overlay);
    openModal(modalId);

    const body = document.getElementById('hist-body');
    const btnCompare = document.getElementById('btn-compare');
    const resultEl = document.getElementById('compare-result');

    body.addEventListener('change', () => {
      const checked = body.querySelectorAll('.hist-cb:checked');
      if (checked.length > 2) { event.target.checked = false; return; }
      btnCompare.disabled = checked.length !== 2;
    });

    body.addEventListener('click', async (e) => {
      const btn = e.target.closest('button[data-del]');
      if (!btn) return;
      if (await openConfirm(`確定刪除 ${btn.dataset.del} 的快照？`, { okClass: 'btn-danger' })) {
        await removeFundSnapshot(fundId, btn.dataset.del, btn.dataset.raw);
        showToast('✅ 已刪除', 'success');
        closeModal(modalId); overlay.remove();
        this._loadSnapshotsAndRender();
      }
    });

    btnCompare.onclick = () => {
      const dates = Array.from(body.querySelectorAll('.hist-cb:checked')).map(cb => cb.dataset.date).sort();
      const a = snaps.find(s => s.dateKey === dates[0]);
      const b = snaps.find(s => s.dateKey === dates[1]);
      if (!a || !b) return;
      
      const aHkd = a.shares * a.nav * DEFAULT_FX;
      const bHkd = b.shares * b.nav * DEFAULT_FX;
      const diffHkd = bHkd - aHkd;
      const pct = aHkd > 0 ? (diffHkd / aHkd * 100).toFixed(2) : 0;
      
      resultEl.innerHTML = `
        <div class="glass-card" style="padding:12px; background:var(--bg-deep);">
          <div style="font-weight:700; margin-bottom:8px;">對比：${a.dateKey} ➜ ${b.dateKey}</div>
          <div style="display:flex; justify-content:space-between; font-size:13px; margin-bottom:4px;">
            <span>單位數</span><span>${a.shares} ➜ ${b.shares}</span>
          </div>
          <div style="display:flex; justify-content:space-between; font-size:13px; margin-bottom:4px;">
            <span>價格</span><span>USD ${a.nav} ➜ USD ${b.nav}</span>
          </div>
          <div style="display:flex; justify-content:space-between; font-size:13px; border-top:1px dashed var(--border-color); padding-top:4px;">
            <span>總值 (HKD)</span>
            <span class="${diffHkd >= 0 ? 'text-emerald' : 'text-red'}">
              ${aHkd.toFixed(2)} ➜ ${bHkd.toFixed(2)} (${diffHkd >= 0 ? '+' : ''}${diffHkd.toFixed(2)}, ${pct}%)
            </span>
          </div>
        </div>
      `;
    };

    document.getElementById('btn-close-hist').onclick = () => { closeModal(modalId); overlay.remove(); };
  },

  _openImportSnapshotModal() {
    const modalId = 'import-snap-modal';
    document.getElementById(modalId)?.remove();
    const funds = this._funds;
    
    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `
      <div class="modal" style="max-width:600px; background:var(--bg-surface);">
        <h2 class="modal-title">導入基金快照</h2>
        <div class="form-stack">
          <div class="field">
            <label class="field-label">目標基金</label>
            <select class="input" id="imp-fund">
              <option value="">— 請選擇 —</option>
              ${funds.map(f => `<option value="${f.id}">${esc(f.name)}</option>`).join('')}
            </select>
          </div>
          <div class="field">
            <label class="field-label">上傳 JSON (格式：[{"dateKey":"2024-01-01","shares":100,"nav":12.5}])</label>
            <input type="file" id="imp-file" accept=".json" class="input">
          </div>
          <div class="field">
            <label class="field-label">或直接貼上 JSON</label>
            <textarea class="input" id="imp-text" rows="6" style="font-family:monospace; font-size:12px;"></textarea>
          </div>
          <div id="imp-status" style="font-size:12px; color:var(--text-muted);"></div>
          <div style="display:flex; justify-content:flex-end; gap:8px; margin-top:12px;">
            <button type="button" class="btn btn-ghost" id="imp-cancel">取消</button>
            <button type="button" class="btn btn-primary" id="imp-submit">開始導入</button>
          </div>
        </div>
      </div>
    `;
    document.body.appendChild(overlay);
    openModal(modalId);

    const fileInput = document.getElementById('imp-file');
    const textArea = document.getElementById('imp-text');

    fileInput.onchange = async () => {
      const file = fileInput.files[0];
      if (file) textArea.value = await file.text();
    };

    document.getElementById('imp-cancel').onclick = () => { closeModal(modalId); overlay.remove(); };

    document.getElementById('imp-submit').onclick = async () => {
      const fundId = document.getElementById('imp-fund').value;
      if (!fundId) { showToast('請選擇基金', 'warning'); return; }
      const text = textArea.value.trim();
      if (!text) { showToast('請貼上 JSON', 'warning'); return; }

      try {
        const data = JSON.parse(text);
        if (!Array.isArray(data)) throw new Error('JSON 必須是陣列');
        
        let ok = 0, fail = 0;
        const statusEl = document.getElementById('imp-status');
        statusEl.textContent = '導入中…';
        
        for (const item of data) {
          if (!item.dateKey || item.shares === undefined || item.nav === undefined) { fail++; continue; }
          try {
            await saveFundSnapshot(fundId, item.dateKey, { shares: Number(item.shares), nav: Number(item.nav) });
            ok++;
          } catch (e) { fail++; }
        }
        
        statusEl.innerHTML = `<span class="text-emerald">✅ 成功 ${ok} 筆</span>${fail > 0 ? `, <span class="text-red">失敗 ${fail} 筆</span>` : ''}`;
        showToast('✅ 導入完成', 'success');
        this._loadSnapshotsAndRender();
      } catch (err) {
        showToast('JSON 解析失敗：' + err.message, 'error');
      }
    };
  },

  destroy() {
    if (this._unsubFunds) { try { this._unsubFunds(); } catch (e) {} this._unsubFunds = null; }
    if (this._unsubPolicies) { try { this._unsubPolicies(); } catch (e) {} this._unsubPolicies = null; }
  }
};

/* ============================================
   輔助邏輯
   ============================================ */
function _findPolicyByFund(fund, policies) {
  if (fund.policyId) {
    const p = (policies || []).find((x) => x.id === fund.policyId);
    if (p) return p;
  }
  return (policies || []).find((p) => (p.fundsAllocation || []).some((a) => a.fundId === fund.id)) || null;
}

function _getCostMapForPolicy(policy, allFunds, targetYear, targetMonth) {
  const costMap = {};
  if (!policy) return costMap;
  const policyFunds = (allFunds || []).filter((f) => f.type === 'insurance' && _findPolicyByFund(f, [policy]) !== null);
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

/* ═══ EOF ═══
F: pages/portfolio.js
V: 1792901980
W: webapp-20261025-001#46
═══ */```

<!-- === FILE END: ./pages/portfolio.js === -->

<!-- === FILE START: ./pages/settings.js === -->

## 📄 ./pages/settings.js

```js
// ============================================
// settings.js — 系統設定頁面（V2 引擎版）
// 位置：pages/settings.js
// 說明：整合帳號資訊、統計模式、成員管理、年份範圍、頁面標題與除錯工具。
//       已完全拔除 shared/ 依賴，改用原生引擎。
// ============================================

import { AppState } from '../config/state.js';
import { logout } from '../domain/auth.js';
import { openConfirm, openModal, closeModal } from '../ui/modal.js';
import { showToast } from '../ui/toast.js';
import { esc, formatHKD } from '../ui/format.js';
import { ROLES, STORAGE_KEYS, ENTITY_KEYS } from '../config/constants.js';
import { initTabPanel } from '../ui/tab-panel.js';
import { createForm } from '../ui/form.js';
import { renderTable } from '../ui/table.js';
import { listen, saveObject, listenMembers, listenBankAccounts, removeObject } from '../db/index.js';
import { listPages, getDefaultMetadata } from '../config/page-metadata-registry.js';
import { api } from '../domain/api-client.js';

export default {
  _unsubMeta: null,
  _unsubYear: null,
  _unsubMembers: null,
  _unsubBanks: null,

  async render() {
    const content = document.getElementById('app-content');
    if (!content) return;

    // 1. 建立 Tab 骨架
    const isSuper = AppState.isSuperAdmin;
    const tabs = [];
    if (isSuper) tabs.push({ key: 'platform', label: '平台設定', icon: 'settings', panelId: 'settings-panel-platform' });
    tabs.push({ key: 'banks', label: '銀行帳號', icon: 'landmark', panelId: 'settings-panel-banks' });
    tabs.push({ key: 'personal', label: '個人化', icon: 'user', panelId: 'settings-panel-personal' });

    content.innerHTML = `
      <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px; flex-wrap:wrap; gap:12px;">
        <h1 style="font-size:24px; font-weight:700; color:var(--primary);">系統設定</h1>
      </div>
      <div id="settings-tabs"></div>
      
      <div id="settings-panel-platform" class="tab-panel" style="display:none;">
        <div class="banner mb-16">ℹ️ 平台 UI 常數為全站預設值，家庭可在「預設資料庫」中覆蓋。</div>
      </div>

      <div id="settings-panel-banks" class="tab-panel" style="display:none;">
        <div class="banner mb-16">ℹ️ 管理家庭的銀行帳號。每個帳號有「初始餘額」與「初始年月」。</div>
        <div style="display:flex; justify-content:flex-end; margin-bottom:12px;">
          <button type="button" class="btn btn-primary" id="btn-add-bank"><i data-lucide="plus" style="width:14px;height:14px;"></i> 新增銀行</button>
        </div>
        <div id="settings-banks-list"></div>
      </div>

      <div id="settings-panel-personal" class="tab-panel" style="display:none;">
        <div class="settings-personal-grid" style="display:grid; grid-template-columns:repeat(auto-fit, minmax(320px, 1fr)); gap:16px;">
          <!-- 帳號資訊 -->
          <div class="glass-card">
            <div class="glass-card-title" style="margin-bottom:16px; display:flex; align-items:center; gap:8px;">
              <i data-lucide="user" style="width:16px;height:16px;color:var(--primary);"></i> 帳號資訊
            </div>
            <div class="form-stack">
              <div class="field"><label class="field-label">登入帳號</label><input class="input mono" id="settings-account" disabled></div>
              <div class="field"><label class="field-label">角色</label><input class="input mono" id="settings-role" disabled></div>
              <div class="field"><label class="field-label">家庭名稱</label><input class="input" id="settings-family" disabled></div>
            </div>
            <div style="margin-top:16px; padding-top:16px; border-top:1px dashed var(--border-color); text-align:right;">
              <button type="button" class="btn btn-danger" id="settings-logout-btn"><i data-lucide="log-out" style="width:14px;height:14px;"></i> 登出</button>
            </div>
          </div>

          <!-- 統計卡顯示模式 -->
          <div class="glass-card">
            <div class="glass-card-title" style="margin-bottom:16px; display:flex; align-items:center; gap:8px;">
              <i data-lucide="bar-chart-2" style="width:16px;height:16px;color:var(--primary);"></i> 統計卡顯示模式
            </div>
            <div id="stats-mode-options" style="display:flex; flex-direction:column; gap:8px;"></div>
            <div style="margin-top:16px; padding-top:16px; border-top:1px dashed var(--border-color); text-align:right;">
              <button type="button" class="btn btn-primary" id="save-stats-mode-btn"><i data-lucide="check" style="width:14px;height:14px;"></i> 儲存模式</button>
            </div>
          </div>

          <!-- 年份範圍 -->
          <div class="glass-card" style="grid-column: 1 / -1;">
            <div class="glass-card-title" style="margin-bottom:16px; display:flex; align-items:center; gap:8px;">
              <i data-lucide="calendar" style="width:16px;height:16px;color:var(--primary);"></i> 年份範圍設定
            </div>
            <div id="yearrange-form-root"></div>
          </div>

          <!-- 頁面標題 -->
          <div class="glass-card" style="grid-column: 1 / -1;">
            <div class="glass-card-title" style="margin-bottom:16px; display:flex; align-items:center; gap:8px;">
              <i data-lucide="type" style="width:16px;height:16px;color:var(--primary);"></i> 頁面標題客製化
            </div>
            <div id="pagemeta-form-root"></div>
          </div>

          <!-- 除錯工具 -->
          <div class="glass-card" style="grid-column: 1 / -1;">
            <div class="glass-card-title" style="margin-bottom:12px; display:flex; align-items:center; gap:8px;">
              <i data-lucide="bug" style="width:16px;height:16px;color:var(--primary);"></i> 除錯工具 (vConsole)
            </div>
            <p class="glass-card-hint mb-12">手機若無 DevTools，可手動開啟 vConsole 檢視日誌。</p>
            <div id="debug-status" style="margin-bottom:12px; font-size:13px;"></div>
            <div style="display:flex; gap:8px;">
              <button type="button" class="btn btn-primary" id="debug-enable-btn">開啟 vConsole</button>
              <button type="button" class="btn btn-ghost" id="debug-disable-btn">關閉</button>
            </div>
          </div>

          <!-- 家庭成員 -->
          <div class="glass-card" style="grid-column: 1 / -1;">
            <div class="glass-card-title" style="margin-bottom:16px; display:flex; align-items:center; gap:8px;">
              <i data-lucide="users" style="width:16px;height:16px;color:var(--primary);"></i> 家庭成員
            </div>
            <div style="display:flex; justify-content:flex-end; margin-bottom:12px;">
              <button type="button" class="btn btn-primary" id="btn-add-member"><i data-lucide="plus" style="width:14px;height:14px;"></i> 新增成員</button>
            </div>
            <div id="family-members-root"></div>
          </div>
        </div>
      </div>
    `;

    // 2. 初始化 Tab 切換
    const tabPanel = initTabPanel({
      containerId: 'settings-tabs',
      tabs,
      defaultKey: isSuper ? 'platform' : 'banks',
      storageKey: 'settings-tab',
      onChange: (key) => {
        if (key === 'banks') this._renderBanksTab();
        if (key === 'personal') this._initPersonalTab();
      }
    });

    // 3. 若預設為個人化或銀行，直接初始化
    if (tabPanel.getCurrent() === 'personal') this._initPersonalTab();
    if (tabPanel.getCurrent() === 'banks') this._renderBanksTab();
  },

  /* ============================================
     銀行帳號管理 (拔除 initBankAccountManager)
     ============================================ */
  _renderBanksTab() {
    const root = document.getElementById('settings-banks-list');
    if (!root) return;

    if (this._unsubBanks) this._unsubBanks();
    this._unsubBanks = listenBankAccounts((accounts) => {
      const renderActions = (row) => `
        <button type="button" class="btn btn-sm btn-ghost" data-action="edit" data-id="${row.id}">編輯</button>
        <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-id="${row.id}">刪除</button>
      `;
      
      renderTable({
        container: root,
        rows: accounts,
        columnsKey: 'bankAccounts',
        renderActions
      });

      root.onclick = async (e) => {
        const btn = e.target.closest('button[data-action]');
        if (!btn) return;
        const id = btn.dataset.id;
        const row = accounts.find(a => a.id === id);
        if (!row) return;

        if (btn.dataset.action === 'delete') {
          const ok = await openConfirm(`確定要刪除「${row.name}」嗎？`, { okClass: 'btn-danger' });
          if (ok) {
            await removeObject(`bank_accounts/${id}`);
            showToast('✅ 已刪除帳號', 'success');
          }
        } else if (btn.dataset.action === 'edit') {
          this._openBankModal(row);
        }
      };
    });

    document.getElementById('btn-add-bank').onclick = () => this._openBankModal();
  },

  _openBankModal(bank = null) {
    const isEdit = !!bank;
    const modalId = 'bank-modal-settings';
    document.getElementById(modalId)?.remove();

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `<div class="modal" style="max-width:400px;"><h2 class="modal-title">${isEdit ? '編輯' : '新增'}銀行</h2><div id="${modalId}-form"></div></div>`;
    document.body.appendChild(overlay);
    openModal(modalId);

    createForm({
      container: `${modalId}-form`,
      fields: [
        { id: 'name', label: '帳號名稱', type: 'text', required: true },
        { id: 'type', label: '類型', type: 'select', required: true, options: [{ value: 'family', label: '家庭' }, { value: 'personal', label: '個人' }] },
        { id: 'initialBalance', label: '初始餘額', type: 'number', required: true },
        { id: 'initialYear', label: '初始年份', type: 'number', required: true },
        { id: 'initialMonth', label: '初始月份', type: 'number', required: true, min: 1, max: 12 }
      ],
      initialData: isEdit ? bank : { type: 'family', initialBalance: 0, initialYear: new Date().getFullYear(), initialMonth: 1 },
      submitText: isEdit ? '儲存' : '新增',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        try {
          if (isEdit) {
            await saveObject(`bank_accounts/${bank.id}`, { ...bank, ...data });
          } else {
            const { pushObject } = await import('../db/write.js');
            await pushObject('bank_accounts', { ...data, createdAt: Date.now() });
          }
          showToast('✅ 已儲存', 'success');
          closeModal(modalId); overlay.remove();
        } catch (err) { showToast('錯誤：' + err.message, 'error'); }
      }
    });
  },

  /* ============================================
     個人化 Tab
     ============================================ */
  _initPersonalTab() {
    // 1. 帳號資訊
    const accEl = document.getElementById('settings-account');
    const roleEl = document.getElementById('settings-role');
    const famEl = document.getElementById('settings-family');
    const memberAcc = AppState.getMemberAccount() || {};

    if (accEl) accEl.value = memberAcc.account || (AppState.currentUser?.email ? AppState.currentUser.email.split('@')[0] : '—');
    if (roleEl) roleEl.value = AppState.getRole() === ROLES.SUPERADMIN ? '超級管理員' : (AppState.getRole() === ROLES.OWNER ? '擁有者' : '成員');
    if (famEl) famEl.value = AppState.getFamilyName() || '—';

    const logoutBtn = document.getElementById('settings-logout-btn');
    logoutBtn.onclick = async () => {
      if (await openConfirm('確定要登出嗎？', { title: '登出', okClass: 'btn-danger' })) logout();
    };

    // 2. 統計模式
    const statsRoot = document.getElementById('stats-mode-options');
    if (statsRoot) {
      let curMode = 'auto';
      try { curMode = localStorage.getItem(STORAGE_KEYS.STATS_MODE) || 'auto'; } catch (e) {}
      const modes = [
        { value: 'auto', label: '自動（依裝置 / 檢視模式）' },
        { value: 'integrated', label: '整合卡（單一大卡）' },
        { value: 'compact', label: '緊湊卡（多個小卡）' }
      ];
      statsRoot.innerHTML = modes.map(m => `
        <label style="display:flex; align-items:center; gap:8px; cursor:pointer; padding:8px; background:var(--bg-deep); border-radius:6px;">
          <input type="radio" name="stats-mode" value="${m.value}" ${m.value === curMode ? 'checked' : ''} style="cursor:pointer;">
          <span style="font-size:13px;">${m.label}</span>
        </label>
      `).join('');

      document.getElementById('save-stats-mode-btn').onclick = () => {
        const selected = document.querySelector('input[name="stats-mode"]:checked');
        if (selected) {
          localStorage.setItem(STORAGE_KEYS.STATS_MODE, selected.value);
          showToast('✅ 已儲存顯示模式', 'success');
        }
      };
    }

    // 3. 年份範圍
    const yearFormRoot = document.getElementById('yearrange-form-root');
    if (yearFormRoot) {
      if (this._unsubYear) this._unsubYear();
      this._unsubYear = listen('settings/year_range', (snap) => {
        const data = snap.val() || {};
        if (yearFormRoot.querySelector('form')) {
          yearFormRoot.querySelector('#yr-start').value = data.startYear || '';
          yearFormRoot.querySelector('#yr-future').value = data.futureYears || 5;
        }
      });

      createForm({
        container: yearFormRoot,
        fields: [
          { id: 'yr-start', label: '起始年份 (留空=當前-3)', type: 'number', min: 1900, max: 2200 },
          { id: 'yr-future', label: '往後顯示年數', type: 'number', required: true, min: 0, max: 20 }
        ],
        submitText: '儲存年份範圍',
        onCancel: null,
        onSubmit: async (data) => {
          const sy = data['yr-start'] === '' ? null : Number(data['yr-start']);
          const fy = Number(data['yr-future']) || 5;
          await saveObject('settings/year_range', { startYear: sy, futureYears: fy });
          showToast('✅ 年份範圍已更新', 'success');
        }
      });
    }

    // 4. 頁面標題
    const pageFormRoot = document.getElementById('pagemeta-form-root');
    if (pageFormRoot) {
      const pages = listPages();
      let allMeta = {};
      
      if (this._unsubMeta) this._unsubMeta();
      this._unsubMeta = listen('settings/page_metadata', (snap) => {
        allMeta = snap.val() || {};
        const pageSel = pageFormRoot.querySelector('#pm-page');
        if (pageSel) _applyPageMeta(pageFormRoot, pageSel.value, allMeta, pages);
      });

      pageFormRoot.innerHTML = `
        <div class="form-stack">
          <div class="field">
            <label class="field-label">選擇頁面</label>
            <select class="input" id="pm-page">
              ${pages.map(p => `<option value="${esc(p.key)}">${esc(p.title)}</option>`).join('')}
            </select>
          </div>
          <div class="field"><label class="field-label">頁面標題</label><input class="input" id="pm-title" type="text" maxlength="40"></div>
          <div class="field"><label class="field-label">副標題 (靜態段)</label><input class="input" id="pm-sub" type="text" maxlength="60"></div>
          <div style="display:flex; gap:8px; justify-content:flex-end;">
            <button type="button" class="btn btn-ghost" id="pm-reset">恢復預設</button>
            <button type="button" class="btn btn-primary" id="pm-save">儲存標題</button>
          </div>
        </div>
      `;

      pageFormRoot.querySelector('#pm-page').onchange = (e) => {
        _applyPageMeta(pageFormRoot, e.target.value, allMeta, pages);
      };

      pageFormRoot.querySelector('#pm-save').onclick = async () => {
        const pk = pageFormRoot.querySelector('#pm-page').value;
        const title = pageFormRoot.querySelector('#pm-title').value.trim();
        const sub = pageFormRoot.querySelector('#pm-sub').value;
        if (!title) { showToast('標題不可為空', 'warning'); return; }
        
        const next = { ...allMeta, [pk]: { title, subtitleStatic: sub } };
        await saveObject('settings/page_metadata', next);
        showToast('✅ 頁面標題已更新', 'success');
      };

      pageFormRoot.querySelector('#pm-reset').onclick = async () => {
        const pk = pageFormRoot.querySelector('#pm-page').value;
        const next = { ...allMeta };
        delete next[pk];
        await saveObject('settings/page_metadata', next);
        _applyPageMeta(pageFormRoot, pk, next, pages);
        showToast('✅ 已恢復預設', 'success');
      };
    }

    // 5. vConsole 除錯工具
    const debugStatus = document.getElementById('debug-status');
    const updateDebugUI = async () => {
      try {
        const mod = await import('../core/debug.js');
        debugStatus.innerHTML = mod.isDebugEnabled() ? '<span class="badge badge-success">● 已開啟</span>' : '<span class="badge badge-muted">● 已關閉</span>';
      } catch (e) { debugStatus.innerHTML = '載入失敗'; }
    };
    updateDebugUI();

    document.getElementById('debug-enable-btn').onclick = async () => {
      const mod = await import('../core/debug.js');
      await mod.forceEnableDebug();
      updateDebugUI();
    };
    document.getElementById('debug-disable-btn').onclick = async () => {
      const mod = await import('../core/debug.js');
      mod.forceDisableDebug();
      updateDebugUI();
    };

    // 6. 家庭成員 (拔除 initEntityListPage)
    const memberRoot = document.getElementById('family-members-root');
    if (memberRoot) {
      if (this._unsubMembers) this._unsubMembers();
      this._unsubMembers = listenMembers((members) => {
        const renderActions = (row) => `
          <button type="button" class="btn btn-sm btn-ghost" data-action="edit" data-id="${row.id}">編輯</button>
          <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-id="${row.id}">刪除</button>
        `;
        renderTable({
          container: memberRoot,
          rows: members,
          columnsKey: 'memberReport', // 借用 memberReport 的欄位定義（name, net...），或自訂
          customColumns: [
            { id: 'name', label: '名稱' },
            { id: 'role', label: '角色' },
            { id: 'order', label: '排序' }
          ],
          customResolvers: {
            name: (v) => esc(v),
            role: (v) => esc(v),
            order: (v) => `<span class="mono">${v || 0}</span>`
          },
          renderActions
        });

        memberRoot.onclick = async (e) => {
          const btn = e.target.closest('button[data-action]');
          if (!btn) return;
          const id = btn.dataset.id;
          const row = members.find(m => m.id === id);
          if (!row) return;

          if (btn.dataset.action === 'delete') {
            const ok = await openConfirm(`確定要刪除成員「${row.name}」嗎？`, { okClass: 'btn-danger' });
            if (ok) {
              await removeObject(`members/${id}`);
              showToast('✅ 已刪除成員', 'success');
            }
          } else if (btn.dataset.action === 'edit') {
            this._openMemberModal(row);
          }
        };
      });

      document.getElementById('btn-add-member').onclick = () => this._openMemberModal();
    }

    if (window.lucide) window.lucide.createIcons();
  },

  _openMemberModal(member = null) {
    const isEdit = !!member;
    const modalId = 'member-modal-settings';
    document.getElementById(modalId)?.remove();

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `<div class="modal" style="max-width:400px;"><h2 class="modal-title">${isEdit ? '編輯' : '新增'}成員</h2><div id="${modalId}-form"></div></div>`;
    document.body.appendChild(overlay);
    openModal(modalId);

    createForm({
      container: `${modalId}-form`,
      fields: [
        { id: 'name', label: '成員名稱', type: 'text', required: true },
        { id: 'role', label: '角色', type: 'text', required: false, placeholder: '例如：husband' },
        { id: 'order', label: '排序', type: 'number', required: false }
      ],
      initialData: isEdit ? member : { order: 0 },
      submitText: isEdit ? '儲存' : '新增',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        try {
          if (isEdit) {
            await saveObject(`members/${member.id}`, { ...member, ...data });
          } else {
            const { pushObject } = await import('../db/write.js');
            await pushObject('members', { ...data, createdAt: Date.now() });
          }
          showToast('✅ 已儲存', 'success');
          closeModal(modalId); overlay.remove();
        } catch (err) { showToast('錯誤：' + err.message, 'error'); }
      }
    });
  },

  destroy() {
    if (this._unsubMeta) { try { this._unsubMeta(); } catch (e) {} this._unsubMeta = null; }
    if (this._unsubYear) { try { this._unsubYear(); } catch (e) {} this._unsubYear = null; }
    if (this._unsubMembers) { try { this._unsubMembers(); } catch (e) {} this._unsubMembers = null; }
    if (this._unsubBanks) { try { this._unsubBanks(); } catch (e) {} this._unsubBanks = null; }
  }
};

/* ============================================
   內部輔助：套用頁面標題
   ============================================ */
function _applyPageMeta(formRoot, pageKey, allMeta, pages) {
  const custom = allMeta[pageKey] || {};
  const def = getDefaultMetadata(pageKey);
  formRoot.querySelector('#pm-title').value = custom.title || def.title;
  formRoot.querySelector('#pm-sub').value = custom.subtitleStatic !== undefined ? custom.subtitleStatic : (def.subtitleStatic || '');
}

/* ═══ EOF ═══
F: pages/settings.js
V: 1792901970
W: webapp-20261025-001#45
═══ */```

<!-- === FILE END: ./pages/settings.js === -->

<!-- === FILE START: ./pages/settlements.js === -->

## 📄 ./pages/settlements.js

```js
// ============================================
// settlements.js — 結算清單（V2 引擎版）
// 位置：pages/settlements.js
// 說明：整合一般支出與保險扣款，提供確認與取消功能。
//       使用 renderPageLayout 統一排版，恢復 V1 的結算模式。
// ============================================

import { AppState } from '../config/state.js';
import { api } from '../domain/api-client.js';
import { mergeSettlementData } from '../domain/settlement.js';
import { renderTable, renderCards } from '../ui/table.js';
import { renderViewToggle, renderYearMonth } from '../ui/controls.js';
import { renderPageLayout } from '../ui/layout.js';
import { openConfirm, openModal, closeModal } from '../ui/modal.js';
import { createForm } from '../ui/form.js';
import { showToast } from '../ui/toast.js';
import { formatHKD, esc } from '../ui/format.js';
import { removeExpense, updateExpense } from '../db/write.js';
import { getBankAccountsOnce } from '../db/index.js';

const HIDE_COMPLETED_KEY = 'fin_ui_settle_hide_completed';

export default {
  _currentYear: '',
  _currentMonth: '',
  _data: [],
  _unsubYM: null,

  async render() {
    // 1. 準備控制列 HTML
    const controlsHtml = `
      <div style="display:flex; align-items:center; gap:12px; flex-wrap:wrap;">
        <div style="display:flex; align-items:center; gap:8px;">
          <input type="checkbox" id="settle-hide-done" style="cursor:pointer;">
          <label for="settle-hide-done" style="font-size:13px; color:var(--text-secondary); cursor:pointer;">隱藏已處理</label>
        </div>
        <div id="settle-view-toggle"></div>
      </div>
      <div id="settle-controls-period"></div>
    `;

    // 2. 呼叫統一引擎繪製骨架
    renderPageLayout({
      title: '結算清單',
      stats: [],
      controls: controlsHtml,
      listContainerId: 'settle-list-container'
    });

    // 3. 綁定控制列事件
    this._unsubYM = AppState.on('ym-change', () => this._loadData());

    renderYearMonth('settle-controls-period', { includeAllMonth: true }, () => {
      this._loadData();
    });

    renderViewToggle('settle-view-toggle', () => {
      this._renderListAndStats();
    });

    document.getElementById('settle-hide-done')?.addEventListener('change', () => {
      this._renderListAndStats();
    });

    // 4. 載入資料
    await this._loadData();
  },

  async _loadData() {
    const listRoot = document.getElementById('settle-list-container');
    if (!listRoot) return;

    listRoot.innerHTML = '<div style="text-align:center; padding:40px; color:var(--text-muted);">載入資料中…</div>';

    const { year, month } = AppState.getYearMonth();
    this._currentYear = String(year);
    this._currentMonth = month;

    try {
      const res = await api.fetchSettlementsYear(year);
      const memberExpenses = [];
      
      if (month !== 'all') {
        const mm = String(month).padStart(2, '0');
        (res.memberExpensesByMonth[mm] || []).forEach(e => memberExpenses.push({ ...e, year, month: mm }));
      } else {
        Object.entries(res.memberExpensesByMonth).forEach(([mm, list]) => {
          list.forEach(e => memberExpenses.push({ ...e, year, month: mm }));
        });
      }

      const insuranceRows = [];
      (res.policies || []).forEach(p => {
        const payments = res.paymentsCache[p.id]?.[year] || {};
        if (month !== 'all') {
          const mm = String(month).padStart(2, '0');
          const pay = payments[mm];
          if (pay) insuranceRows.push({ policyId: p.id, policyName: p.name, policyHolderId: p.policyHolderId, memberId: p.memberId, status: pay.status, amount: pay.amount, date: pay.date, paymentMode: p.paymentMode, advanceHolderId: p.advanceHolderId });
        } else {
          Object.entries(payments).forEach(([mm, pay]) => {
            insuranceRows.push({ policyId: p.id, policyName: p.name, policyHolderId: p.policyHolderId, memberId: p.memberId, status: pay.status, amount: pay.amount, date: pay.date, paymentMode: p.paymentMode, advanceHolderId: p.advanceHolderId, month: mm });
          });
        }
      });

      this._data = mergeSettlementData({ memberExpenses, insuranceRows, year: this._currentYear, month: this._currentMonth });
      this._renderListAndStats();

    } catch (err) {
      console.error('[settlements] 載入失敗：', err);
      showToast('載入結算資料失敗', 'error');
      listRoot.innerHTML = '<div style="color:var(--accent-danger); padding:20px; text-align:center;">載入失敗</div>';
    }
  },

  _renderListAndStats() {
    const listRoot = document.getElementById('settle-list-container');
    const statsRoot = document.getElementById('page-stats-container');
    if (!listRoot || !this._data) return;

    const hideDone = document.getElementById('settle-hide-done')?.checked;
    const currentView = AppState.getCurrentView() || 'table';

    let displayData = this._data;
    if (hideDone) displayData = displayData.filter(r => !r.isDone);

    const pending = displayData.filter(r => !r.isDone);
    const done = displayData.filter(r => r.isDone);
    const pt = pending.reduce((s, r) => s + (Number(r.amount) || 0), 0);
    const dt = done.reduce((s, r) => s + (Number(r.amount) || 0), 0);

    // 更新統計卡
    if (statsRoot) {
      statsRoot.style.display = 'grid';
      statsRoot.innerHTML = [
        { title: '待處理金額', val: formatHKD(pt), color: 'var(--accent-danger)', hint: `${pending.length} 筆`, icon: 'clock' },
        { title: '已處理金額', val: formatHKD(dt), color: 'var(--accent-success)', hint: `${done.length} 筆`, icon: 'check-circle' },
        { title: '總計', val: formatHKD(pt + dt), color: 'var(--primary)', hint: `${displayData.length} 筆`, icon: 'calculator' }
      ].map(card => `
        <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
          <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
            <i data-lucide="${card.icon}" style="width:14px;height:14px; flex-shrink:0;"></i> 
            <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${card.title}</span>
          </div>
          <div class="mono" style="font-size:20px; font-weight:700; color:${card.color}; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
            ${card.val}
          </div>
          <div style="font-size:11px; color:var(--text-muted); overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${card.hint}</div>
        </div>
      `).join('');
    }

    if (displayData.length === 0) {
      listRoot.innerHTML = '<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">此條件下無資料</div>';
      return;
    }

    const renderActions = (row) => {
      if (row.source === 'insurance') {
        return row.isDone
          ? `<button type="button" class="btn btn-sm btn-ghost" data-action="ins-unsync" data-id="${row._ref.policyId}">取消扣款</button>`
          : `<button type="button" class="btn btn-sm btn-primary" data-action="ins-sync" data-id="${row._ref.policyId}">確認扣款</button>`;
      }
      return `
        <button type="button" class="btn btn-sm btn-ghost" data-action="settle" data-id="${row._ref.expenseId}" data-member="${row.memberId}">結算</button>
        <button type="button" class="btn btn-sm btn-danger" data-action="delete" data-id="${row._ref.expenseId}" data-member="${row.memberId}">刪除</button>
      `;
    };

    if (currentView === 'card') {
      renderCards({ container: listRoot, rows: displayData, columnsKey: 'settlements', renderActions });
    } else {
      renderTable({ container: listRoot, rows: displayData, columnsKey: 'settlements', renderActions });
    }

    // 綁定事件（使用 onclick 防重複綁定）
    listRoot.onclick = async (e) => {
      const btn = e.target.closest('button[data-action]');
      if (!btn) return;
      const action = btn.dataset.action;
      const id = btn.dataset.id;
      const row = displayData.find(r => (r._ref && (r._ref.policyId === id || r._ref.expenseId === id)));
      if (!row) return;

      if (action === 'ins-sync') this._handleInsuranceSync(row);
      else if (action === 'ins-unsync') this._handleInsuranceUnsync(row);
      else if (action === 'settle') this._handleSettle(row);
      else if (action === 'delete') this._handleDelete(row);
    };

    if (window.lucide) window.lucide.createIcons();
  },

  async _handleInsuranceSync(row) {
    const bankId = await this._openBankPickerModal(row);
    if (bankId === undefined) return; // 使用者取消

    try {
      await api.insuranceSync({
        policyId: row._ref.policyId, memberId: row.memberId, policyName: row.name,
        monthlyAverage: row.amount, year: row.year, month: row.month,
        bankId: bankId || '', paymentMode: row.paymentMode || 'direct', advanceHolderId: row.advanceHolderId || ''
      });
      showToast('✅ 已確認扣款', 'success');
      this._loadData();
    } catch (err) { showToast('失敗：' + err.message, 'error'); }
  },

  async _handleInsuranceUnsync(row) {
    const ok = await openConfirm(`確定要取消「${row.name}」的扣款嗎？`, { title: '取消扣款', okClass: 'btn-danger' });
    if (!ok) return;
    try {
      await api.insuranceUnsync({
        policyId: row._ref.policyId, memberId: row.memberId, year: row.year, month: row.month,
        amount: row.amount, paymentMode: row.paymentMode || 'direct', advanceHolderId: row.advanceHolderId || ''
      });
      showToast('✅ 已取消扣款', 'success');
      this._loadData();
    } catch (err) { showToast('失敗：' + err.message, 'error'); }
  },

  _handleSettle(row) {
    const modalId = 'settle-expense-modal';
    document.getElementById(modalId)?.remove();
    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay';
    overlay.id = modalId;
    overlay.innerHTML = `<div class="modal" style="max-width:400px; background:var(--bg-surface);"><h2 class="modal-title">結算 — ${esc(row.name)}</h2><div id="settle-form-root"></div></div>`;
    document.body.appendChild(overlay);
    openModal(modalId);

    createForm({
      container: 'settle-form-root',
      fields: [
        { id: 'status', label: '狀態', type: 'select', required: true, options: [
          { value: 'done', label: '已處理' },
          { value: 'pending', label: '未處理' }
        ]}
      ],
      initialData: { status: row.isDone ? 'done' : 'pending' },
      submitText: '儲存',
      onCancel: () => { closeModal(modalId); overlay.remove(); },
      onSubmit: async (data) => {
        try {
          await updateExpense(row.year, row.month, row.memberId, row._ref.expenseId, { status: data.status });
          showToast('✅ 結算狀態已更新', 'success');
          closeModal(modalId);
          overlay.remove();
          this._loadData();
        } catch (err) { showToast('更新失敗：' + err.message, 'error'); }
      }
    });
  },

  async _handleDelete(row) {
    const ok = await openConfirm(`確定要刪除「${row.name}」嗎？`, { okClass: 'btn-danger' });
    if (!ok) return;
    try {
      await removeExpense(row.year, row.month, row.memberId, row._ref.expenseId);
      showToast('✅ 已刪除', 'success');
      this._loadData();
    } catch (err) { showToast('刪除失敗：' + err.message, 'error'); }
  },

  _openBankPickerModal(row) {
    return new Promise(async (resolve) => {
      const modalId = 'bank-picker-modal';
      document.getElementById(modalId)?.remove();
      
      let banks = [];
      try { banks = await getBankAccountsOnce(); } catch (e) {}

      const overlay = document.createElement('div');
      overlay.className = 'modal-overlay';
      overlay.id = modalId;
      overlay.innerHTML = `
        <div class="modal" style="max-width:400px; background:var(--bg-surface);">
          <h2 class="modal-title">確認扣款 — ${esc(row.name)}</h2>
          <div style="padding:12px; background:var(--bg-deep); border-radius:var(--radius-sm); margin-bottom:16px;">
            <div style="display:flex; justify-content:space-between; font-size:13px; margin-bottom:4px;">
              <span class="text-muted">金額</span><span class="mono text-primary font-weight-bold">${formatHKD(row.amount)}</span>
            </div>
            <div style="display:flex; justify-content:space-between; font-size:13px;">
              <span class="text-muted">所屬年月</span><span class="mono">${esc(row.year)}-${esc(row.month)}</span>
            </div>
          </div>
          <div class="field">
            <label class="field-label">出帳銀行 (可選)</label>
            <select class="input" id="${modalId}-bank">
              <option value="">— 不指定 —</option>
              ${banks.map(b => `<option value="${b.id}">${esc(b.name)}</option>`).join('')}
            </select>
          </div>
          <div style="display:flex; justify-content:flex-end; gap:12px; margin-top:20px;">
            <button type="button" class="btn btn-ghost" id="${modalId}-cancel">取消</button>
            <button type="button" class="btn btn-primary" id="${modalId}-ok">確認扣款</button>
          </div>
        </div>
      `;
      document.body.appendChild(overlay);
      openModal(modalId);

      const close = (val) => { closeModal(modalId); overlay.remove(); resolve(val); };
      overlay.querySelector(`#${modalId}-cancel`).onclick = () => close(undefined);
      overlay.querySelector(`#${modalId}-ok`).onclick = () => {
        const sel = overlay.querySelector(`#${modalId}-bank`);
        close(sel.value);
      };
    });
  },

  destroy() {
    if (this._unsubYM) { try { this._unsubYM(); } catch (e) {} this._unsubYM = null; }
  }
};

/* ═══ EOF ═══
F: pages/settlements.js
V: 1792901880
W: webapp-20261025-001#36
═══ */```

<!-- === FILE END: ./pages/settlements.js === -->

<!-- === FILE START: ./sw.js === -->

## 📄 ./sw.js

```js
// ============================================
// sw.js — Service Worker（V2 重構版）
// 位置：sw.js
// 說明：快取名稱加入 v2 標記，避免與舊版衝突。
//       目前為骨架，後續階段會陸續補上靜態資源清單。
// ============================================

const CACHE_NAME = 'family-fin-v2-v1';

const STATIC_ASSETS = [
  './manifest.json',
  './meta.json',
  // 後續階段會加入 css、js 等檔案
];

self.addEventListener('install', (e) => {
  console.log(`[SW] install (${CACHE_NAME})`);
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

/* ═══ EOF ═══
F: sw.js
V: 1792901730
W: webapp-20261025-001#23
═══ */```

<!-- === FILE END: ./sw.js === -->

<!-- === FILE START: ./ui/controls.js === -->

## 📄 ./ui/controls.js

```js
// ============================================
// controls.js — 共用控制列與篩選器（V2 重構版）
// 位置：ui/controls.js
// ============================================

import { AppState } from '../config/state.js';
import { getYearList } from '../config/app-config.js';
import { STORAGE_KEYS } from '../config/constants.js';
import { esc } from './format.js';

/* ============================================
   1. 年月選擇器
   ============================================ */
export function renderYearMonth(containerId, options = {}, onChange) {
  const root = document.getElementById(containerId);
  if (!root) return;

  const { includeAllMonth = false } = options;
  const years = getYearList();
  const curY = String(AppState.year);
  const curM = AppState.month === 'all' ? 'all' : String(AppState.month).padStart(2, '0');

  let monthOpts = includeAllMonth ? `<option value="all" ${curM === 'all' ? 'selected' : ''}>全部</option>` : '';
  for (let m = 1; m <= 12; m++) {
    const mm = String(m).padStart(2, '0');
    monthOpts += `<option value="${mm}" ${curM === mm ? 'selected' : ''}>${m} 月</option>`;
  }

  root.innerHTML = `
    <div class="flex items-center gap-8">
      <select class="input" id="${containerId}-year" style="width:auto; padding:6px 28px 6px 10px;">
        ${years.map((y) => `<option value="${y}" ${String(y) === curY ? 'selected' : ''}>${y} 年</option>`).join('')}
      </select>
      <select class="input" id="${containerId}-month" style="width:auto; padding:6px 28px 6px 10px;">
        ${monthOpts}
      </select>
    </div>
  `;

  const ySel = document.getElementById(`${containerId}-year`);
  const mSel = document.getElementById(`${containerId}-month`);

  const handleChange = () => {
    const y = ySel.value;
    const m = mSel.value;
    AppState.setYearMonth(y, m);
    if (typeof onChange === 'function') onChange(y, m);
  };

  ySel.addEventListener('change', handleChange);
  mSel.addEventListener('change', handleChange);
}

/* ============================================
   2. 檢視切換 (表格 / 卡片)
   ============================================ */
export function renderViewToggle(containerId, onChange) {
  const root = document.getElementById(containerId);
  if (!root) return;

  // 每次渲染都先清空舊的監聽器
  root.innerHTML = '';

  const curView = AppState.getCurrentView() || 'table';

  // 繪製按鈕結構
  root.innerHTML = `
    <div class="flex items-center gap-4" style="background:var(--bg-surface); border:1px solid var(--border-color); border-radius:var(--radius-md); padding:4px;">
      <button type="button" class="btn" data-view="table" style="padding:6px 12px; border:none; border-radius:var(--radius-sm); transition:all 0.2s;">
        表格模式
      </button>
      <button type="button" class="btn" data-view="card" style="padding:6px 12px; border:none; border-radius:var(--radius-sm); transition:all 0.2s;">
        卡片模式
      </button>
    </div>
  `;

  const btnTable = root.querySelector('button[data-view="table"]');
  const btnCard = root.querySelector('button[data-view="card"]');

  // 更新按鈕樣式的函數
  const updateBtnStyles = (view) => {
    if (view === 'table') {
      btnTable.style.background = 'var(--primary)';
      btnTable.style.color = '#fff';
      btnCard.style.background = 'transparent';
      btnCard.style.color = 'var(--text-secondary)';
    } else {
      btnCard.style.background = 'var(--primary)';
      btnCard.style.color = '#fff';
      btnTable.style.background = 'transparent';
      btnTable.style.color = 'var(--text-secondary)';
    }
  };

  // 初始化樣式
  updateBtnStyles(curView);

  // 點擊事件
  root.addEventListener('click', (e) => {
    const btn = e.target.closest('button[data-view]');
    if (!btn) return;
    const view = btn.dataset.view;
    
    // 如果點擊的是當前已經選中的狀態，就不做任何事
    if (view === AppState.getCurrentView()) return;

    // 更新 AppState 與按鈕樣式
    AppState.setCurrentView(view);
    updateBtnStyles(view);

    // 觸發外部回呼（讓頁面重新渲染列表）
    if (typeof onChange === 'function') onChange(view);
  });
}

/* ============================================
   3. 通用搜尋與篩選列
   ============================================ */
export function renderFilterBar(containerId, fields, onFilterChange) {
  const root = document.getElementById(containerId);
  if (!root) return;

  root.innerHTML = `
    <div class="flex flex-wrap items-center gap-12" style="margin-bottom:16px;">
      ${fields.map((f) => {
        if (f.type === 'select') {
          return `
            <div class="flex flex-col gap-4">
              <label style="font-size:11px; color:var(--text-muted);">${esc(f.label)}</label>
              <select class="input" data-filter="${f.id}" style="width:auto; padding:6px 28px 6px 10px; font-size:13px;">
                <option value="">全部</option>
                ${(f.options || []).map((o) => `<option value="${esc(o.value)}">${esc(o.label)}</option>`).join('')}
              </select>
            </div>
          `;
        }
        if (f.type === 'text') {
          return `
            <div class="flex flex-col gap-4">
              <label style="font-size:11px; color:var(--text-muted);">${esc(f.label)}</label>
              <input class="input" type="text" data-filter="${f.id}" placeholder="搜尋..." style="width:auto; min-width:150px; padding:6px 10px; font-size:13px;">
            </div>
          `;
        }
        return '';
      }).join('')}
    </div>
  `;

  const handleInput = () => {
    const filters = {};
    root.querySelectorAll('[data-filter]').forEach((el) => {
      filters[el.dataset.filter] = el.value;
    });
    if (typeof onFilterChange === 'function') onFilterChange(filters);
  };

  root.addEventListener('change', (e) => {
    if (e.target.closest('[data-filter]')) handleInput();
  });
  root.addEventListener('input', (e) => {
    if (e.target.closest('input[type="text"]')) {
      clearTimeout(root._timer);
      root._timer = setTimeout(handleInput, 300);
    }
  });
}

/* ═══ EOF ═══
F: ui/controls.js
V: 1792900880
W: ai-system-20261024-012#79
═══ EOF ═══ */```

<!-- === FILE END: ./ui/controls.js === -->

<!-- === FILE START: ./ui/dropdown-plus.js === -->

## 📄 ./ui/dropdown-plus.js

```js
// ============================================
// dropdown-plus.js — 下拉旁「+ 號」按鈕邏輯
// 位置：ui/dropdown-plus.js
// 說明：處理簡單實體（只需名稱）的快速新增。
//       複雜實體（如保單、基金）目前不在此處理，交由專屬頁面。
// ============================================

import { openPrompt } from './modal.js';
import { showToast } from './toast.js';
import { api } from '../domain/api-client.js';

/* ============================================
   1. 簡單實體分類
   ============================================ */
const SIMPLE_ENTITIES = {
  member:   { label: '成員', prefix: 'MEM' },
  category: { label: '類別', prefix: 'CAT' },
  payment:  { label: '支付方式', prefix: 'PAY' },
  company:  { label: '保險公司', prefix: 'COMP' },
};

export function isSimpleEntity(entityType) {
  return !!SIMPLE_ENTITIES[entityType];
}

/* ============================================
   2. 主入口
   ============================================ */
export async function handlePlusClick(entityType) {
  const def = SIMPLE_ENTITIES[entityType];
  
  if (!def) {
    showToast(`此功能暫不支援快速新增「${entityType}」，請至對應頁面新增`, 'info');
    return null;
  }

  // 1. 彈出輸入框
  const name = await openPrompt({
    title: `新增${def.label}`,
    label: `${def.label}名稱`,
    placeholder: `請輸入${def.label}名稱`,
    maxlength: 20,
  });

  if (!name) return null; // 使用者取消

  // 2. 生成臨時 code（格式：PREFIX-時間戳）
  const code = `${def.prefix}-${Date.now().toString(36).toUpperCase()}`;

  // 3. 呼叫 API 新增
  try {
    const res = await api.entityCreate({
      entityType,
      action: 'create',
      code,
      name,
      extra: {},
    });

    if (res && res.ok) {
      showToast(`✅ 已新增${def.label}`, 'success');
      return { id: res.id, name, code };
    } else {
      throw new Error(res?.message || '未知錯誤');
    }
  } catch (err) {
    console.error(`[dropdown-plus] 新增 ${entityType} 失敗：`, err);
    showToast(`新增失敗：${err.message}`, 'error');
    return null;
  }
}

/* ═══ EOF ═══
F: ui/dropdown-plus.js
V: 1792901510
W: webapp-20261025-001#13
═══ */```

<!-- === FILE END: ./ui/dropdown-plus.js === -->

<!-- === FILE START: ./ui/form.js === -->

## 📄 ./ui/form.js

```js
// ============================================
// form.js — 通用動態表單建構器（V2 引擎版）
// 位置：ui/form.js
// 說明：支援 dependsOn 聯動、custom 自訂區塊、plusEntity。
//       徹底取代 V1 破碎的表單邏輯。
// ============================================

import { esc } from './format.js';
import { handlePlusClick } from './dropdown-plus.js';

/**
 * 建立並渲染表單
 * @param {Object} options
 * @param {string|HTMLElement} options.container - 容器
 * @param {Array} options.fields - 欄位設定陣列
 * @param {Object} [options.initialData] - 初始資料
 * @param {string} [options.submitText] - 送出按鈕文字
 * @param {string} [options.cancelText] - 取消按鈕文字
 * @param {Function} options.onSubmit - (data) => Promise|void 送出回呼
 * @param {Function} [options.onCancel] - 取消回呼
 */
export function createForm(options) {
  const {
    container,
    fields = [],
    initialData = {},
    submitText = '儲存',
    cancelText = '取消',
    onSubmit,
    onCancel
  } = options;

  const root = _resolveEl(container);
  if (!root) {
    console.error('[ui/form] 找不到容器', container);
    return null;
  }

  // 生成欄位 HTML
  const fieldsHtml = fields.map((f) => _renderField(f, initialData[f.id])).join('');

  root.innerHTML = `
    <form class="app-form" novalidate style="display:flex; flex-direction:column; gap:16px;">
      ${fieldsHtml}
      
      <div class="form-actions" style="display:flex; justify-content:flex-end; gap:12px; margin-top:12px;">
        ${onCancel ? `<button type="button" class="btn btn-ghost" data-form-action="cancel">${esc(cancelText)}</button>` : ''}
        <button type="submit" class="btn btn-primary" data-form-action="submit">
          <i data-lucide="check"></i> <span>${esc(submitText)}</span>
        </button>
      </div>
    </form>
  `;

  const formEl = root.querySelector('form');
  const submitBtn = formEl.querySelector('[data-form-action="submit"]');
  const submitBtnSpan = submitBtn.querySelector('span');

  let _isSubmitting = false;

  // 綁定「+」按鈕事件
  _bindPlusEvents(fields, formEl);
  
  // 綁定聯動事件 (dependsOn)
  _bindDependencies(fields, formEl);

  // 表單送出
  formEl.addEventListener('submit', async (e) => {
    e.preventDefault();
    if (_isSubmitting) return;

    // 清除舊錯誤
    formEl.querySelectorAll('.field-error').forEach(el => el.remove());
    formEl.querySelectorAll('.field').forEach(el => el.style.borderColor = 'transparent');

    const data = _collectData(fields, formEl);
    
    // 基本驗證
    const error = _validate(fields, data, formEl);
    if (error) return;

    if (typeof onSubmit === 'function') {
      _isSubmitting = true;
      submitBtn.disabled = true;
      submitBtnSpan.textContent = '處理中…';
      
      try {
        await onSubmit(data);
      } catch (err) {
        console.error('[ui/form] 送出失敗：', err);
      } finally {
        _isSubmitting = false;
        submitBtn.disabled = false;
        submitBtnSpan.textContent = submitText;
      }
    }
  });

  // 取消按鈕
  if (onCancel) {
    formEl.querySelector('[data-form-action="cancel"]')?.addEventListener('click', onCancel);
  }

  if (window.lucide) window.lucide.createIcons();

  return {
    root,
    formEl,
    // 取得當前表單資料
    getData: () => _collectData(fields, formEl),
    // 重設表單
    reset: () => formEl.reset()
  };
}

/* ============================================
   內部實作：渲染欄位
   ============================================ */
function _renderField(f, value) {
  // 支援自訂區塊 (如基金分配器)
  if (f.type === 'custom') {
    const html = typeof f.html === 'function' ? f.html(value) : (f.html || '');
    return `
      <div class="field" id="field-wrapper-${f.id || 'custom'}">
        ${html}
      </div>
    `;
  }

  const { id, label, type = 'text', required, placeholder = '', options = [], min, max, step, plusEntity } = f;
  const reqMark = required ? '<span style="color:var(--accent-danger); margin-left:4px;">*</span>' : '';
  const valStr = value != null ? String(value) : '';

  let inputHtml = '';

  switch (type) {
    case 'select':
      inputHtml = `
        <select class="input" id="field-${id}" name="${id}" ${required ? 'required' : ''}>
          <option value="">— 請選擇 —</option>
          ${options.map(o => `<option value="${esc(o.value)}" ${String(o.value) === valStr ? 'selected' : ''}>${esc(o.label)}</option>`).join('')}
        </select>
      `;
      break;
    case 'textarea':
      inputHtml = `<textarea class="input" id="field-${id}" name="${id}" rows="3" placeholder="${esc(placeholder)}" ${required ? 'required' : ''}>${esc(valStr)}</textarea>`;
      break;
    case 'number':
      const attrs = [
        min != null ? `min="${min}"` : '',
        max != null ? `max="${max}"` : '',
        step != null ? `step="${step}"` : 'step="1"'
      ].join(' ');
      inputHtml = `<input class="input mono" type="number" id="field-${id}" name="${id}" value="${esc(valStr)}" placeholder="${esc(placeholder)}" ${attrs} ${required ? 'required' : ''}>`;
      break;
    case 'date':
      inputHtml = `<input class="input" type="date" id="field-${id}" name="${id}" value="${esc(valStr)}" ${required ? 'required' : ''}>`;
      break;
    case 'checkbox':
      return `
        <div class="field" style="display:flex; align-items:center; gap:8px;">
          <input type="checkbox" id="field-${id}" name="${id}" ${valStr === 'true' || value === true ? 'checked' : ''}>
          <label for="field-${id}" style="font-size:14px; cursor:pointer; color:var(--text-primary);">${esc(label)}${reqMark}</label>
        </div>
      `;
    default: // text
      inputHtml = `<input class="input" type="text" id="field-${id}" name="${id}" value="${esc(valStr)}" placeholder="${esc(placeholder)}" ${required ? 'required' : ''}>`;
  }

  // 若需要「+」按鈕，包裝成 flex 容器
  if (plusEntity) {
    inputHtml = `
      <div style="display:flex; gap:8px; align-items:center;">
        <div style="flex:1;">${inputHtml}</div>
        <button type="button" class="btn btn-ghost" id="plus-btn-${id}" style="padding:0 12px; height:42px; flex-shrink:0;">
          <i data-lucide="plus" style="width:18px;height:18px;"></i>
        </button>
      </div>
    `;
  }

  return `
    <div class="field" style="display:flex; flex-direction:column; gap:6px; border:1px solid transparent; border-radius:var(--radius-sm);">
      <label for="field-${id}" style="font-size:12px; color:var(--text-secondary); font-weight:500;">${esc(label)}${reqMark}</label>
      ${inputHtml}
    </div>
  `;
}

/* ============================================
   內部實作：綁定「+」按鈕事件
   ============================================ */
function _bindPlusEvents(fields, formEl) {
  fields.forEach(f => {
    if (!f.plusEntity) return;
    const btn = formEl.querySelector(`#plus-btn-${f.id}`);
    if (!btn) return;

    btn.addEventListener('click', async (e) => {
      e.preventDefault();
      e.stopPropagation();
      if (btn.disabled) return;
      btn.disabled = true;

      try {
        const result = await handlePlusClick(f.plusEntity);
        if (!result) return;

        if (typeof f.onPlusReload === 'function') {
          await f.onPlusReload(result, formEl);
        }
      } catch (err) {
        console.error('[ui/form] 快速新增失敗：', err);
      } finally {
        btn.disabled = false;
      }
    });
  });
}

/* ============================================
   內部實作：綁定聯動事件 (dependsOn)
   ============================================ */
function _bindDependencies(fields, formEl) {
  fields.forEach(f => {
    if (!f.dependsOn) return;
    
    const targetEl = formEl.querySelector(`#field-${f.id}`);
    const sourceEl = formEl.querySelector(`#field-${f.dependsOn}`);
    
    if (!targetEl || !sourceEl) return;

    sourceEl.addEventListener('change', () => {
      const sourceVal = sourceEl.value;
      
      // 清空當前選項
      targetEl.innerHTML = '<option value="">— 請先選擇上層項目 —</option>';
      targetEl.value = '';

      // 若有傳入動態載入回呼，則觸發
      if (typeof f.onDependsChange === 'function') {
        f.onDependsChange(sourceVal, targetEl, formEl);
      }
    });
  });
}

/* ============================================
   內部實作：收集資料
   ============================================ */
function _collectData(fields, formEl) {
  const data = {};
  fields.forEach(f => {
    if (f.type === 'custom') return;
    
    const el = formEl.querySelector(`#field-${f.id}`);
    if (!el) return;

    if (f.type === 'checkbox') {
      data[f.id] = el.checked;
    } else if (f.type === 'number') {
      data[f.id] = el.value === '' ? 0 : Number(el.value);
    } else {
      data[f.id] = el.value;
    }
  });
  return data;
}

/* ============================================
   內部實作：驗證
   ============================================ */
function _validate(fields, data, formEl) {
  for (const f of fields) {
    if (!f.required || f.type === 'custom') continue;

    const val = data[f.id];
    const isEmpty = val === '' || val === null || val === undefined;

    if (isEmpty) {
      const fieldWrapper = formEl.querySelector(`#field-${f.id}`)?.closest('.field');
      if (fieldWrapper) {
        fieldWrapper.style.borderColor = 'var(--accent-danger)';
        const err = document.createElement('div');
        err.className = 'field-error';
        err.style.cssText = 'color:var(--accent-danger); font-size:11px; margin-top:2px;';
        err.textContent = `請填寫「${f.label}」`;
        fieldWrapper.appendChild(err);
      }
      return true; // 驗證失敗
    }
  }
  return false; // 驗證成功
}

function _resolveEl(target) {
  if (typeof target === 'string') return document.getElementById(target);
  if (target instanceof HTMLElement) return target;
  return null;
}

/* ═══ EOF ═══
F: ui/form.js
V: 1792901830
W: webapp-20261025-001#31
═══ */```

<!-- === FILE END: ./ui/form.js === -->

<!-- === FILE START: ./ui/format.js === -->

## 📄 ./ui/format.js

```js
// ============================================
// format.js — 格式化與工具函式（V2 重構版）
// 位置：ui/format.js
// 說明：集中管理全站的金額、數字、日期格式化，以及 HTML 轉義。
//       防止 XSS 與確保顯示一致。
// ============================================

/* ============================================
   1. HTML 轉義 (防止 XSS)
   ============================================ */
const _ESCAPE_MAP = {
  '&': '&amp;',
  '<': '&lt;',
  '>': '&gt;',
  '"': '&quot;',
  "'": '&#39;',
};

export function esc(v, fallback = '') {
  if (v == null) return fallback;
  return String(v).replace(/[&<>"']/g, (c) => _ESCAPE_MAP[c]);
}

/* ============================================
   2. 金額與數字格式化
   ============================================ */

/**
 * 格式化為 HK$ 顯示
 */
export function formatHKD(amount) {
  if (amount == null || isNaN(amount)) return 'HK$ 0';
  const rounded = Math.round(Number(amount));
  return 'HK$ ' + rounded.toLocaleString('zh-HK');
}

/**
 * 格式化為純數字（千分位）
 */
export function formatNumber(amount) {
  if (amount == null || isNaN(amount)) return '0';
  const rounded = Math.round(Number(amount));
  return rounded.toLocaleString('zh-HK');
}

/**
 * 四捨五入至整數
 */
export function roundHKD(value) {
  const n = Number(value);
  return isNaN(n) ? 0 : Math.round(n);
}

/**
 * 格式化百分比
 */
export function formatPercent(value, digits = 2) {
  const n = Number(value);
  if (isNaN(n)) return '0%';
  return `${n.toFixed(digits)}%`;
}

/* ============================================
   3. 日期工具
   ============================================ */

/**
 * 取得今天的 YYYY-MM-DD
 */
export function todayISO() {
  return new Date().toISOString().slice(0, 10);
}

/**
 * 格式化日期字串
 */
export function formatDate(val) {
  if (!val) return '—';
  return esc(String(val));
}

/* ============================================
   4. 通用 Cell 格式化（供表格使用）
   ============================================ */
export function formatCellValue(val, type) {
  if (val == null || val === '') return '<span class="text-muted">—</span>';

  switch (type) {
    case 'number':
      return formatHKD(val);
    case 'number-plain':
      return formatNumber(val);
    case 'date':
      return formatDate(val);
    case 'select':
    case 'text':
    default:
      return esc(String(val));
  }
}

/* ============================================
   5. 其他輔助
   ============================================ */
export function debounce(fn, wait = 300) {
  let timer = null;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), wait);
  };
}

/* ═══ EOF ═══
F: ui/format.js
V: 1792900280
W: ai-system-20261024-012#23
═══ EOF ═══ */```

<!-- === FILE END: ./ui/format.js === -->

<!-- === FILE START: ./ui/layout.js === -->

## 📄 ./ui/layout.js

```js
// ============================================
// layout.js — SPA 骨架與導覽（V2 重構版）
// 位置：ui/layout.js
// 說明：統一頁面骨架、導覽列、側邊欄與防破版佈局引擎。
// ============================================

import { AppState } from '../config/state.js';
import { getSidebarGroupsByRole, ROLES, ROUTES } from '../config/constants.js';
import { logout } from '../domain/auth.js';
import { esc } from './format.js';

let _containerId = '';
let _isMobileOpen = false;

export function initLayout(containerId = 'app-root') {
  _containerId = containerId;
  const root = document.getElementById(_containerId);
  if (!root) return;

  _renderShell(root);
  _bindGlobalEvents();
  updateNavbar();
  updateSidebarActive(window.location.hash.replace('#', '') || ROUTES.DASHBOARD);

  const loading = document.getElementById('app-loading');
  if (loading) loading.style.display = 'none';
  root.style.display = 'block';
}

function _renderShell(root) {
  root.innerHTML = `
    <div class="app-shell">
      <div class="sidebar-backdrop" id="sidebar-backdrop"></div>
      
      <aside class="sidebar" id="app-sidebar">
        <div class="sidebar-header">
          <div class="sidebar-logo">◈ FAMILY.FIN</div>
        </div>
        <nav class="sidebar-nav" id="app-sidebar-nav"></nav>
      </aside>

      <div class="main-area">
        <header class="navbar">
          <button type="button" class="hamburger" id="app-hamburger">
            <i data-lucide="menu"></i>
          </button>
          
          <div class="navbar-title" id="app-navbar-title"></div>
          
          <div class="navbar-right">
            <button type="button" class="navbar-theme-btn" id="app-theme-toggle">
              <i data-lucide="moon" style="width:18px;height:18px;"></i>
            </button>
            <div class="navbar-user" id="app-user-info"></div>
          </div>
        </header>

        <main class="content" id="app-content" style="width:100%; max-width:100%; overflow-x:hidden; box-sizing:border-box;"></main>
      </div>
    </div>
  `;

  _updateSidebarNav();
  if (window.lucide) window.lucide.createIcons();
}

export function updateNavbar() {
  const titleEl = document.getElementById('app-navbar-title');
  const userEl = document.getElementById('app-user-info');
  const themeBtn = document.getElementById('app-theme-toggle');

  if (titleEl && AppState.currentFamilyName) {
    titleEl.textContent = AppState.currentFamilyName;
  } else if (titleEl && AppState.isSuperAdmin) {
    titleEl.textContent = '系統管理後台';
  }

  if (userEl) {
    const name = AppState.getDisplayName() || '訪客';
    const role = AppState.getRole();
    let badge = '';
    if (role === ROLES.SUPERADMIN) badge = '<span class="badge badge-danger" style="font-size:10px; padding:2px 6px;">管理員</span>';
    else if (role === ROLES.OWNER) badge = '<span class="badge badge-info" style="font-size:10px; padding:2px 6px;">👑 擁有者</span>';
    
    userEl.innerHTML = `
      <i data-lucide="user" style="width:16px;height:16px;"></i>
      <span style="white-space:nowrap; font-weight:500;">${esc(name)}</span>
      ${badge}
      <button type="button" id="app-logout-btn" class="navbar-logout" title="登出">
        <i data-lucide="log-out" style="width:18px;height:18px;"></i>
      </button>
    `;
    
    document.getElementById('app-logout-btn')?.addEventListener('click', async () => {
      const { openConfirm } = await import('./modal.js');
      const ok = await openConfirm('確定要登出嗎？', { title: '登出' });
      if (ok) logout();
    });
  }

  if (themeBtn) {
    const isDark = AppState.getTheme() === 'dark';
    themeBtn.innerHTML = `<i data-lucide="${isDark ? 'sun' : 'moon'}" style="width:18px;height:18px;"></i>`;
    themeBtn.onclick = () => {
      AppState.toggleTheme();
      updateNavbar();
    };
  }

  if (window.lucide) window.lucide.createIcons();
}

function _updateSidebarNav() {
  const navEl = document.getElementById('app-sidebar-nav');
  if (!navEl) return;

  const role = AppState.getRole();
  const groups = getSidebarGroupsByRole(role);

  navEl.innerHTML = groups.map((g) => {
    if (g.items.length === 1) {
      const item = g.items[0];
      return `
        <a class="nav-item root-item" href="#${item.path}" data-path="${item.path}">
          <i data-lucide="${item.icon}" class="nav-icon"></i>
          <span class="nav-label">${esc(item.label)}</span>
        </a>
      `;
    }

    return `
      <div class="nav-group">
        <div class="nav-group-title">
          <span class="nav-group-label">
            <i data-lucide="${g.icon}" class="nav-group-icon" style="width:16px;height:16px;"></i>
            <span>${esc(g.label)}</span>
          </span>
        </div>
        <div class="nav-group-body">
          ${g.items.map((item) => `
            <a class="nav-item" href="#${item.path}" data-path="${item.path}">
              <i data-lucide="${item.icon}" class="nav-icon"></i>
              <span class="nav-label">${esc(item.label)}</span>
            </a>
          `).join('')}
        </div>
      </div>
    `;
  }).join('');

  if (window.lucide) window.lucide.createIcons();
}

export function updateSidebarActive(currentPath) {
  document.querySelectorAll('.sidebar-nav .nav-item').forEach((el) => {
    const path = el.getAttribute('data-path');
    if (currentPath === path || (currentPath === '/' && path === ROUTES.DASHBOARD)) {
      el.classList.add('active');
    } else {
      el.classList.remove('active');
    }
  });
}

export function setContent(html) {
  const contentEl = document.getElementById('app-content');
  if (contentEl) {
    contentEl.innerHTML = html;
    if (window.lucide) window.lucide.createIcons();
  }
}

export function setPageTitle(title) {
  document.title = `${title} | FAMILY.FIN`;
}

/* ============================================
   統一頁面排版引擎（防破版核心）
   ============================================ */
export function renderPageLayout(config) {
  const content = document.getElementById('app-content');
  if (!content) return null;

  const {
    title,
    headerActions = '',
    stats = null,
    statsCols = 2,
    controls = '',
    listTitle = '',
    listContainerId = 'page-list-container'
  } = config;

  let statsHtml = '';
  if (stats && stats.length > 0) {
    const gridCols = window.innerWidth < 768 ? 2 : statsCols;
    statsHtml = `
      <div id="page-stats-container" style="display:grid; grid-template-columns:repeat(${gridCols}, 1fr); gap:16px; margin-bottom:24px; width:100%;">
        ${stats.map(card => `
          <div class="glass-card" style="padding:16px; display:flex; flex-direction:column; gap:6px; min-width:0; overflow:hidden;">
            <div style="font-size:12px; color:var(--text-muted); display:flex; align-items:center; gap:6px;">
              <i data-lucide="${card.icon || 'info'}" style="width:14px;height:14px; flex-shrink:0;"></i> 
              <span style="overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${esc(card.title)}</span>
            </div>
            <div class="mono" style="font-size:20px; font-weight:700; color:${card.color || 'var(--text-primary)'}; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">
              ${esc(card.val)}
            </div>
            ${card.hint ? `<div style="font-size:11px; color:var(--text-muted); overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">${esc(card.hint)}</div>` : ''}
          </div>
        `).join('')}
      </div>
    `;
  } else {
    statsHtml = `<div id="page-stats-container" style="display:none;"></div>`;
  }

  const controlsHtml = controls ? `
    <div style="display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:16px; flex-wrap:wrap; gap:12px; width:100%;">
      ${controls}
    </div>
  ` : '';

  const listTitleHtml = listTitle ? `
    <h2 style="font-size:16px; margin-bottom:12px; color:var(--text-secondary);">${esc(listTitle)}</h2>
  ` : '';

  // 約束最外層寬度，防止內部 table 撐破版面
  content.innerHTML = `
    <div style="width:100%; max-width:100%; overflow-x:hidden; display:flex; flex-direction:column; box-sizing:border-box;">
      <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px; flex-wrap:wrap; gap:12px; width:100%;">
        <h1 style="font-size:24px; font-weight:700; color:var(--primary); margin:0;">${esc(title)}</h1>
        <div style="flex-shrink:0;">${headerActions}</div>
      </div>

      ${statsHtml}
      ${controlsHtml}
      ${listTitleHtml}
      <div id="${listContainerId}" style="width:100%; max-width:100%;">
        <div style="text-align:center; padding:40px; color:var(--text-muted);">載入資料中…</div>
      </div>
    </div>
  `;

  if (window.lucide) window.lucide.createIcons();
  return document.getElementById(listContainerId);
}

function _bindGlobalEvents() {
  document.getElementById('app-hamburger')?.addEventListener('click', () => {
    _isMobileOpen = !_isMobileOpen;
    _toggleSidebar(_isMobileOpen);
  });

  document.getElementById('sidebar-backdrop')?.addEventListener('click', () => {
    _isMobileOpen = false;
    _toggleSidebar(false);
  });

  document.getElementById('app-sidebar')?.addEventListener('click', (e) => {
    if (e.target.closest('.nav-item') && window.innerWidth < 768) {
      _isMobileOpen = false;
      _toggleSidebar(false);
    }
  });

  AppState.on('theme-change', () => updateNavbar());
}

function _toggleSidebar(open) {
  const sidebar = document.getElementById('app-sidebar');
  const backdrop = document.getElementById('sidebar-backdrop');
  
  if (open) {
    sidebar?.classList.add('mobile-open');
    backdrop?.classList.add('active');
    document.body.style.overflow = 'hidden';
  } else {
    sidebar?.classList.remove('mobile-open');
    backdrop?.classList.remove('active');
    document.body.style.overflow = '';
  }
}

/* ═══ EOF ═══
F: ui/layout.js
V: 1792901820
W: webapp-20261025-001#30
═══ */```

<!-- === FILE END: ./ui/layout.js === -->

<!-- === FILE START: ./ui/modal.js === -->

## 📄 ./ui/modal.js

```js
// ============================================
// modal.js — 對話框與確認視窗（V2 重構版）
// 位置：ui/modal.js
// 說明：集中管理 Modal 開關、Promise 形式的確認框與輸入框。
// ============================================

import { esc } from './format.js';

/* ============================================
   1. 基礎 Modal 開關
   ============================================ */
const _stack = [];

export function openModal(target) {
  const modal = _resolveElement(target);
  if (!modal) return;

  modal.classList.add('active');
  document.body.style.overflow = 'hidden';

  if (!_stack.includes(modal)) {
    _stack.push(modal);
  }
  _bindEscClose();
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

export function destroyModal(target) {
  const modal = _resolveElement(target);
  if (!modal) return;
  closeModal(modal);
  modal.remove();
}

/* ============================================
   2. 確認對話框 (Confirm)
   ============================================ */
let _confirmResolve = null;

export function openConfirm(message, options = {}) {
  const {
    title = '確認',
    okText = '確定',
    cancelText = '取消',
    okClass = 'btn-danger',
  } = options;

  // 如果已有開啟中的 confirm，先強制結束它
  const oldOverlay = document.getElementById('__confirm-modal');
  if (oldOverlay) {
    if (oldOverlay._cleanup) oldOverlay._cleanup();
    oldOverlay.remove();
    document.body.style.overflow = '';
    if (_confirmResolve) {
      _confirmResolve(false);
      _confirmResolve = null;
    }
  }

  return new Promise((resolve) => {
    _confirmResolve = resolve;

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay active';
    overlay.id = '__confirm-modal';
    overlay.innerHTML = `
      <div class="modal" style="max-width:400px; background:var(--bg-surface);">
        <h2 class="modal-title" style="font-size:18px; margin-bottom:12px; color:var(--text-primary);">${esc(title)}</h2>
        <div style="font-size:14px; line-height:1.6; color:var(--text-secondary); white-space:pre-wrap; margin-bottom:20px;">${esc(message)}</div>
        <div class="modal-actions" style="display:flex; justify-content:flex-end; gap:10px;">
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
    document.addEventListener('keydown', handleKey);

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
   3. 輸入對話框 (Prompt)
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

  const oldOverlay = document.getElementById('__prompt-modal');
  if (oldOverlay) {
    if (oldOverlay._cleanup) oldOverlay._cleanup();
    oldOverlay.remove();
    document.body.style.overflow = '';
    if (_promptResolve) {
      _promptResolve(null);
      _promptResolve = null;
    }
  }

  return new Promise((resolve) => {
    _promptResolve = resolve;

    const overlay = document.createElement('div');
    overlay.className = 'modal-overlay active';
    overlay.id = '__prompt-modal';
    overlay.innerHTML = `
      <div class="modal" style="max-width:400px; background:var(--bg-surface);">
        <h2 class="modal-title" style="font-size:18px; margin-bottom:12px; color:var(--text-primary);">${esc(title)}</h2>
        ${label ? `<label class="field-label" style="display:block; font-size:12px; color:var(--text-secondary); margin-bottom:6px;">${esc(label)}</label>` : ''}
        <input type="text" class="input" id="__prompt-input"
               placeholder="${esc(placeholder)}" value="${esc(defaultValue)}"
               maxlength="${maxlength}" style="width:100%; padding:10px; background:var(--bg-deep); border:1px solid var(--border-color); border-radius:var(--radius-sm); color:var(--text-primary); margin-bottom:20px;">
        <div class="modal-actions" style="display:flex; justify-content:flex-end; gap:10px;">
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
    document.addEventListener('keydown', handleKey);

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
   內部工具
   ============================================ */
function _resolveElement(target) {
  if (typeof target === 'string') return document.getElementById(target);
  if (target instanceof HTMLElement) return target;
  return null;
}

let _escBound = false;
function _bindEscClose() {
  if (_escBound) return;
  _escBound = true;
  document.addEventListener('keydown', (e) => {
    if (e.key !== 'Escape') return;
    if (_stack.length === 0) return;
    closeModal(_stack[_stack.length - 1]);
  });
}

/* ═══ EOF ═══
F: ui/modal.js
V: 1792901700
W: webapp-20261025-001#20
═══ */```

<!-- === FILE END: ./ui/modal.js === -->

<!-- === FILE START: ./ui/table.js === -->

## 📄 ./ui/table.js

```js
// ============================================
// table.js — 通用資料表格引擎（V2 引擎版）
// 位置：ui/table.js
// 說明：支援表頭排序（3態）、表頭下拉篩選、欄位顯示設定。
//       包含表格與卡片兩種渲染模式，並具備防破版樣式。
// ============================================

import { getColumns, getResolvers } from '../config/columns.js';
import { esc } from './format.js';
import { openModal, closeModal, destroyModal } from './modal.js';

/* ============================================
   狀態儲存（每個表格獨立）
   ============================================ */
const _tableStates = {};

function _getTableState(key) {
  if (!_tableStates[key]) {
    _tableStates[key] = {
      sortCol: null,
      sortDir: null, // 'asc' | 'desc' | null
      filters: {},   // { colId: value }
      visibleCols: null // 若為 null 表示全顯示
    };
  }
  return _tableStates[key];
}

/* ============================================
   1. 渲染表格 (Table)
   ============================================ */
export function renderTable(options) {
  const root = _resolveEl(options.container);
  if (!root) return;

  const { rows = [], columnsKey, renderActions, onRowClick, emptyText = '尚無資料' } = options;
  const state = _getTableState(columnsKey);

  const allColumns = getColumns(columnsKey);
  const resolvers = getResolvers(columnsKey);
  const hasActions = typeof renderActions === 'function';

  // 初始化可見欄位
  if (!state.visibleCols) {
    state.visibleCols = allColumns.map(c => c.id);
  }

  // 1. 篩選
  let displayRows = [...rows];
  Object.entries(state.filters).forEach(([colId, val]) => {
    if (!val) return;
    displayRows = displayRows.filter(r => String(r[colId] ?? '') === String(val));
  });

  // 2. 排序
  if (state.sortCol && state.sortDir) {
    const dir = state.sortDir === 'asc' ? 1 : -1;
    const colDef = allColumns.find(c => c.id === state.sortCol);
    const isNum = colDef && colDef.type === 'number';

    displayRows.sort((a, b) => {
      const va = a[state.sortCol];
      const vb = b[state.sortCol];
      if (isNum) return ((Number(va) || 0) - (Number(vb) || 0)) * dir;
      return String(va ?? '').localeCompare(String(vb ?? ''), 'zh-HK') * dir;
    });
  }

  // 渲染骨架
  if (rows.length === 0) {
    root.innerHTML = `<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">${esc(emptyText)}</div>`;
    return;
  }

  const finalCols = allColumns.filter(c => state.visibleCols.includes(c.id));
  if (hasActions) finalCols.push({ id: '__actions__', label: '操作', type: 'actions' });

  // 渲染表頭
  const theadHtml = `
    <thead>
      <tr>
        ${finalCols.map(c => _renderTh(c, allColumns, state)).join('')}
      </tr>
    </thead>
  `;

  // 渲染資料列
  const tbodyHtml = `
    <tbody>
      ${displayRows.map((row, index) => {
        const tds = finalCols.map(col => {
          if (col.id === '__actions__') {
            return `<td style="text-align:right; white-space:nowrap;">${renderActions(row)}</td>`;
          }
          const val = row[col.id];
          const resolver = resolvers[col.id];
          let content = resolver ? resolver(val, row) : esc(val);
          if (content === undefined || content === null || content === '') content = '<span class="text-muted">—</span>';
          
          const isNum = col.type === 'number';
          return `<td class="${isNum ? 'num' : ''}" style="white-space:nowrap;">${content}</td>`;
        }).join('');

        return `<tr data-index="${index}" ${onRowClick ? 'style="cursor:pointer;"' : ''}>${tds}</tr>`;
      }).join('')}
    </tbody>
  `;

  // 工具列 (顯示筆數 + 欄位設定)
  const toolbarHtml = `
    <div style="display:flex; justify-content:space-between; align-items:center; padding:12px 16px; border-bottom:1px solid var(--border-color); background:var(--bg-surface);">
      <div style="font-size:13px; color:var(--text-muted);">共 ${displayRows.length} 筆</div>
      <button type="button" class="btn btn-sm btn-ghost" id="btn-col-settings-${columnsKey}">
        <i data-lucide="settings" style="width:14px;height:14px;"></i> 欄位設定
      </button>
    </div>
  `;

  root.innerHTML = `
    <div class="glass-card" style="padding:0; overflow:hidden; width:100%; max-width:100%;">
      ${toolbarHtml}
      <div class="data-table-scroll-wrapper" style="overflow-x:auto; -webkit-overflow-scrolling:touch;">
        <table class="data-table" style="width:100%; border-collapse:collapse; min-width:600px;">
          ${theadHtml}
          ${tbodyHtml}
        </table>
      </div>
    </div>
  `;

  // 綁定事件
  _bindTableEvents(root, state, allColumns, columnsKey, options);
}

/* ============================================
   2. 渲染卡片 (Cards)
   ============================================ */
export function renderCards(options) {
  const root = _resolveEl(options.container);
  if (!root) return;

  const { rows = [], columnsKey, renderActions, onRowClick, emptyText = '尚無資料' } = options;

  if (rows.length === 0) {
    root.innerHTML = `<div class="glass-card" style="text-align:center; padding:40px; color:var(--text-muted);">${esc(emptyText)}</div>`;
    return;
  }

  const columns = getColumns(columnsKey);
  const resolvers = getResolvers(columnsKey);
  const hasActions = typeof renderActions === 'function';
  
  const primaryCol = columns[0];
  const restCols = columns.slice(1);

  const cardsHtml = rows.map((row, index) => {
    const primaryVal = row[primaryCol.id];
    const primaryResolver = resolvers[primaryCol.id];
    const primaryContent = primaryResolver ? primaryResolver(primaryVal, row) : esc(primaryVal);

    const fieldsHtml = restCols.map((col) => {
      const val = row[col.id];
      const resolver = resolvers[col.id];
      let content = resolver ? resolver(val, row) : esc(val);
      if (content === undefined || content === null || content === '') content = '<span class="text-muted">—</span>';
      
      return `
        <div style="display:flex; justify-content:space-between; margin-bottom:6px; font-size:13px;">
          <span style="color:var(--text-muted);">${esc(col.label)}</span>
          <span style="font-weight:500; text-align:right;">${content}</span>
        </div>
      `;
    }).join('');

    const actionsHtml = hasActions 
      ? `<div style="margin-top:12px; padding-top:12px; border-top:1px dashed var(--border-color); display:flex; justify-content:flex-end; gap:8px;">${renderActions(row)}</div>` 
      : '';

    return `
      <div class="glass-card" data-index="${index}" style="padding:16px; margin-bottom:12px; width:100%; max-width:100%; ${onRowClick ? 'cursor:pointer;' : ''}">
        <div style="font-size:16px; font-weight:700; color:var(--primary); margin-bottom:12px; word-break:break-word;">${primaryContent}</div>
        <div>${fieldsHtml}</div>
        ${actionsHtml}
      </div>
    `;
  }).join('');

  root.innerHTML = `<div class="mobile-card-list" style="width:100%; max-width:100%;">${cardsHtml}</div>`;

  _bindRowClick(root, onRowClick, rows);
}

/* ============================================
   3. 內部工具：渲染表頭
   ============================================ */
function _renderTh(col, allColumns, state) {
  if (col.id === '__actions__') {
    return `<th style="width:150px; text-align:right; white-space:nowrap;">操作</th>`;
  }

  const isSorted = state.sortCol === col.id;
  const sortIcon = isSorted ? (state.sortDir === 'asc' ? 'arrow-up' : 'arrow-down') : 'arrow-up-down';
  const isFiltered = state.filters[col.id] != null && state.filters[col.id] !== '';
  
  // 判斷是否支援排序與篩選（根據 col 設定）
  const sortable = col.sortable !== false; // 預設允許
  const filterable = !!col.filterOptions; // 有選項才顯示漏斗

  return `
    <th class="${col.type === 'number' ? 'num' : ''}" style="white-space:nowrap;" data-col-id="${col.id}">
      <div style="display:flex; align-items:center; gap:4px; ${col.type === 'number' ? 'justify-content:flex-end;' : ''}">
        <span>${esc(col.label)}</span>
        ${sortable ? `
          <button type="button" class="btn-icon btn-sort ${isSorted ? 'text-primary' : 'text-muted'}" data-sort="${col.id}">
            <i data-lucide="${sortIcon}" style="width:12px;height:12px;"></i>
          </button>
        ` : ''}
        ${filterable ? `
          <button type="button" class="btn-icon btn-filter ${isFiltered ? 'text-primary' : 'text-muted'}" data-filter="${col.id}">
            <i data-lucide="filter" style="width:12px;height:12px;"></i>
          </button>
        ` : ''}
      </div>
    </th>
  `;
}

/* ============================================
   4. 內部工具：事件綁定
   ============================================ */
function _bindTableEvents(root, state, allColumns, columnsKey, options) {
  const { rows, columnsKey: key, onRowClick } = options;
  
  // 排序 / 篩選按鈕
  root.querySelectorAll('.btn-sort').forEach(btn => {
    btn.onclick = (e) => {
      e.stopPropagation();
      const colId = btn.dataset.sort;
      if (state.sortCol !== colId) {
        state.sortCol = colId; state.sortDir = 'asc';
      } else if (state.sortDir === 'asc') {
        state.sortDir = 'desc';
      } else {
        state.sortCol = null; state.sortDir = null; // 恢復原狀
      }
      renderTable(options); // 重新渲染
    };
  });

  root.querySelectorAll('.btn-filter').forEach(btn => {
    btn.onclick = (e) => {
      e.stopPropagation();
      _openFilterMenu(btn, allColumns.find(c => c.id === btn.dataset.filter), state, options);
    };
  });

  // 欄位設定
  const colBtn = root.querySelector(`#btn-col-settings-${key}`);
  if (colBtn) {
    colBtn.onclick = () => _openColumnSettings(allColumns, state, options);
  }

  // 列點擊
  if (onRowClick) {
    root.querySelectorAll('tbody tr[data-index]').forEach(tr => {
      tr.onclick = (e) => {
        if (e.target.closest('button')) return;
        const idx = Number(tr.dataset.index);
        const displayRows = _getDisplayRows(rows, state);
        onRowClick(displayRows[idx]);
      };
    });
  }

  if (window.lucide) window.lucide.createIcons();
}

/* ============================================
   5. 內部工具：篩選選單與欄位設定
   ============================================ */
function _getDisplayRows(rows, state) {
  let display = [...rows];
  Object.entries(state.filters).forEach(([colId, val]) => {
    if (val) display = display.filter(r => String(r[colId] ?? '') === String(val));
  });
  if (state.sortCol && state.sortDir) {
    const dir = state.sortDir === 'asc' ? 1 : -1;
    display.sort((a, b) => String(a[state.sortCol] ?? '').localeCompare(String(b[state.sortCol] ?? '')) * dir);
  }
  return display;
}

function _openFilterMenu(btnEl, colDef, state, options) {
  document.getElementById('__table-filter-menu')?.remove();
  const rect = btnEl.getBoundingClientRect();
  const curVal = state.filters[colDef.id] || '';

  const menu = document.createElement('div');
  menu.id = '__table-filter-menu';
  menu.style.cssText = `position:fixed; top:${rect.bottom + 4}px; left:${rect.left - 100}px; z-index:9999; background:var(--bg-surface); border:1px solid var(--border-color); border-radius:var(--radius-sm); box-shadow:var(--shadow-md); padding:12px; min-width:200px;`;
  
  menu.innerHTML = `
    <div style="font-size:12px; font-weight:600; margin-bottom:8px; color:var(--text-primary);">篩選：${esc(colDef.label)}</div>
    <select class="input" style="width:100%; margin-bottom:12px; padding:6px;" id="__filter-select">
      <option value="">— 全部 —</option>
      ${colDef.filterOptions.map(o => `<option value="${esc(o.value)}" ${String(o.value) === curVal ? 'selected' : ''}>${esc(o.label)}</option>`).join('')}
    </select>
    <div style="display:flex; justify-content:flex-end; gap:8px;">
      <button type="button" class="btn btn-sm btn-ghost" id="__filter-clear">清除</button>
      <button type="button" class="btn btn-sm btn-primary" id="__filter-apply">套用</button>
    </div>
  `;
  document.body.appendChild(menu);

  const closeMenu = () => menu.remove();
  document.addEventListener('mousedown', function closeHandler(e) {
    if (!menu.contains(e.target)) { closeMenu(); document.removeEventListener('mousedown', closeHandler); }
  });

  menu.querySelector('#__filter-apply').onclick = () => {
    const val = menu.querySelector('#__filter-select').value;
    if (val) state.filters[colDef.id] = val;
    else delete state.filters[colDef.id];
    closeMenu();
    renderTable(options);
  };
  menu.querySelector('#__filter-clear').onclick = () => {
    delete state.filters[colDef.id];
    closeMenu();
    renderTable(options);
  };
}

function _openColumnSettings(allColumns, state, options) {
  const modalId = '__col-settings-modal';
  document.getElementById(modalId)?.remove();

  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = modalId;
  
  overlay.innerHTML = `
    <div class="modal" style="max-width:400px; background:var(--bg-surface);">
      <h2 style="font-size:18px; font-weight:700; margin-bottom:16px; color:var(--text-primary);">欄位顯示設定</h2>
      <div style="max-height:60vh; overflow-y:auto; display:flex; flex-direction:column; gap:8px;">
        ${allColumns.map(c => `
          <label style="display:flex; align-items:center; gap:8px; cursor:pointer; padding:8px; border-radius:6px; background:var(--bg-deep);">
            <input type="checkbox" data-col="${c.id}" ${state.visibleCols.includes(c.id) ? 'checked' : ''} style="width:16px;height:16px;">
            <span style="font-size:14px; color:var(--text-primary);">${esc(c.label)}</span>
          </label>
        `).join('')}
      </div>
      <div style="display:flex; justify-content:flex-end; gap:12px; margin-top:20px;">
        <button type="button" class="btn btn-ghost" id="__col-cancel">取消</button>
        <button type="button" class="btn btn-primary" id="__col-save">儲存</button>
      </div>
    </div>
  `;
  document.body.appendChild(overlay);
  openModal(modalId);

  overlay.querySelector('#__col-cancel').onclick = () => { closeModal(modalId); overlay.remove(); };
  overlay.querySelector('#__col-save').onclick = () => {
    const checked = Array.from(overlay.querySelectorAll('input[type="checkbox"]:checked')).map(cb => cb.dataset.col);
    if (checked.length === 0) {
      alert('至少需保留一個欄位'); return;
    }
    state.visibleCols = checked;
    closeModal(modalId); overlay.remove();
    renderTable(options);
  };
}

/* ============================================
   輔助函式
   ============================================ */
function _resolveEl(target) {
  if (typeof target === 'string') return document.getElementById(target);
  if (target instanceof HTMLElement) return target;
  return null;
}

function _bindRowClick(root, onRowClick, rows) {
  if (onRowClick) {
    root.addEventListener('click', (e) => {
      if (e.target.closest('button')) return;
      const trOrCard = e.target.closest('[data-index]');
      if (trOrCard) {
        onRowClick(rows[Number(trOrCard.dataset.index)]);
      }
    });
  }
}

/* ═══ EOF ═══
F: ui/table.js
V: 1792901840
W: webapp-20261025-001#32
═══ */```

<!-- === FILE END: ./ui/table.js === -->

<!-- === FILE START: ./ui/toast.js === -->

## 📄 ./ui/toast.js

```js
// ============================================
// toast.js — 全局提示元件（V2 重構版）
// 位置：ui/toast.js
// 說明：提供輕量、美觀的彈出提示。無依賴，可獨立使用。
// ============================================

const COLORS = {
  success: 'rgba(16, 185, 129, 0.95)',  // 翠綠
  error:   'rgba(244, 63, 94, 0.95)',   // 洋紅
  info:    'rgba(0, 240, 255, 0.95)',   // 賽博青
  warning: 'rgba(251, 191, 36, 0.95)',  // 橘黃
};

const CONTAINER_ID = 'app-toast-container';
const MAX_TOASTS = 3;
const DEFAULT_DURATION = 2500;

// 記錄每個 Toast 的 Timer，避免記憶體洩漏
const _timers = new WeakMap();

export function showToast(msg, type = 'success', duration = DEFAULT_DURATION) {
  const container = _getOrCreateContainer();

  // 超過上限時，先清 timer 再移除
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
    padding: 10px 20px;
    border-radius: var(--radius-md, 12px);
    font-size: 14px;
    font-weight: 500;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
    opacity: 0;
    transform: translateY(10px);
    transition: opacity 0.3s ease, transform 0.3s ease;
    max-width: 90%;
    text-align: center;
    pointer-events: none;
    margin-top: 8px;
    white-space: pre-wrap;
    word-break: break-word;
  `;
  toast.textContent = msg;

  container.appendChild(toast);

  requestAnimationFrame(() => {
    toast.style.opacity = '1';
    toast.style.transform = 'translateY(0)';
  });

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

/* ═══ EOF ═══
F: ui/toast.js
V: 1792900400
W: ai-system-20261024-012#35
═══ EOF ═══ */```

<!-- === FILE END: ./ui/toast.js === -->
