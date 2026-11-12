> 體系版本：SV14
> 生成時間：2026-11-12 19:12:15 (HKT)

# 專案全程式碼包：AI-WA-UPWO


<!-- === FILE START: ./.github/workflows/deploy.yml === -->

## 📄 ./.github/workflows/deploy.yml

```yml
name: Deploy to Cloudflare Pages

on:
  workflow_dispatch: # 改為僅能手動觸發

# 防止連續點擊觸發多次部署，節省資源
concurrency:
  group: "pages-deploy"
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      deployments: write
    name: Build and Deploy
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Install Dependencies
        # 若專案無 package-lock.json，使用 npm install 避免報錯
        run: npm install

      - name: Build Project
        run: npm run build

      - name: Deploy to Cloudflare Pages
        uses: cloudflare/wrangler-action@v3
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          command: pages deploy dist --project-name=ai-wa-upwo

# ═══ EOF ═══
# F: AI-WA-UPWO/.github/workflows/deploy.yml
# V: 1792922200
# W: UPGRADE-004
# ═══```

<!-- === FILE END: ./.github/workflows/deploy.yml === -->

<!-- === FILE START: ./.github/workflows/notify.yml === -->

## 📄 ./.github/workflows/notify.yml

```yml
name: Notify AI-Infra

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger AI-Infra Sync
        uses: peter-evans/repository-dispatch@v3
        with:
          token: ${{ secrets.CROSS_REPO_PAT }}
          repository: ${{ secrets.AI_INFRA_REPO }}
          event-type: update-package

# ═══ EOF ═══
# F: AI-WA-UPWO/.github/workflows/notify.yml
# V: 1792922100
# W: UPGRADE-004
# ═══```

<!-- === FILE END: ./.github/workflows/notify.yml === -->

<!-- === FILE START: ./index.html === -->

## 📄 ./index.html

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>UPGRADE-WO | 生產進度控制台</title>
  <meta name="description" content="先鋒印刷 - 生產進度控制台">
  <meta name="theme-color" content="#6366F1">
  
  <!-- PWA 設定檔 -->
  <link rel="manifest" href="/manifest.json">
  
  <!-- iOS 專用 PWA Meta -->
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
  <meta name="apple-mobile-web-app-title" content="UPGRADE">
  <link rel="apple-touch-icon" href="/icons/icon.png">
  
  <!-- 引入全域樣式 (由 Vite 處理) -->
  <link rel="stylesheet" href="/src/style.css">
</head>
<body class="bg-[#F4F7FB] text-slate-800 font-sans min-h-screen antialiased overflow-hidden">

  <!-- 初始載入畫面 -->
  <div id="app-loading" class="fixed inset-0 z-[999] flex flex-col items-center justify-center bg-[#F4F7FB] transition-opacity duration-300">
    <div class="w-12 h-12 rounded-xl bg-[#6366F1] flex items-center justify-center text-white shadow-lg shadow-indigo-500/30 mb-4 animate-bounce">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-layers"><path d="m12.83 2.18a2 2 0 0 0-1.66 0L2.6 6.08a1 1 0 0 0 0 1.83l8.58 3.91a2 2 0 0 0 1.66 0l8.58-3.9a1 1 0 0 0 0-1.83Z"/><path d="m22 17.65-9.17 4.16a2 2 0 0 1-1.66 0L2 17.65"/><path d="m22 12.65-9.17 4.16a2 2 0 0 1-1.66 0L2 12.65"/></svg>
    </div>
    <div class="text-sm font-medium text-slate-500 animate-pulse">系統載入中...</div>
  </div>

  <!-- 系統掛載點，確保在手機與桌機都能正確撐滿高度 -->
  <div id="app-root" class="w-full h-[100dvh] overflow-hidden"></div>

  <!-- 引入 SortableJS 用於支援手機觸控拖曳排序 -->
  <script src="https://cdn.jsdelivr.net/npm/sortablejs@latest/Sortable.min.js"></script>

  <!-- 系統啟動入口 (Vite 會自動處理 Module 依賴) -->
  <script type="module" src="/src/engine/main.js"></script>
</body>
</html>

<!--
F: AI-WA-UPWO/index.html
V: 1792946000
W: UPGRADE-005
-->```

<!-- === FILE END: ./index.html === -->

<!-- === FILE START: ./meta.json === -->

## 📄 ./meta.json

```json
{
  "domain": "W",
  "pkg_no": "92",
  "code": "UPGRADE",
  "display": "UPGRADE-WO"
}```

<!-- === FILE END: ./meta.json === -->

<!-- === FILE START: ./package.json === -->

## 📄 ./package.json

```json
{
  "name": "upgrade-wo",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "firebase": "^10.12.2",
    "jspdf": "^2.5.1",
    "lucide": "^0.395.0",
    "xlsx": "^0.18.5"
  },
  "devDependencies": {
    "autoprefixer": "^10.4.19",
    "postcss": "^8.4.38",
    "tailwindcss": "^3.4.4",
    "vite": "^5.3.1"
  }
}```

<!-- === FILE END: ./package.json === -->

<!-- === FILE START: ./postcss.config.js === -->

## 📄 ./postcss.config.js

```js
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};

/* ═══ EOF ═══
F: postcss.config.js
V: 1792903400
W: webapp-20261026-001#10
═══ */```

<!-- === FILE END: ./postcss.config.js === -->

<!-- === FILE START: ./public/manifest.json === -->

## 📄 ./public/manifest.json

```json
{
  "name": "UPGRADE-WO 生產進度控制台",
  "short_name": "UPGRADE",
  "description": "先鋒印刷 - 流水線進度控制台",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#F4F7FB",
  "theme_color": "#6366F1",
  "icons": [
    {
      "src": "/icons/icon.png",
      "sizes": "1024x1024",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ],
  "_EOF": {
    "F": "AI-WA-UPWO/public/manifest.json",
    "V": "1792923000",
    "W": "UPGRADE-004"
  }
}```

<!-- === FILE END: ./public/manifest.json === -->

<!-- === FILE START: ./public/sw.js === -->

## 📄 ./public/sw.js

```js
const SW_VERSION = 'v1.0.0';
const CACHE_NAME = `upgrade-wo-cache-${SW_VERSION}`;

// 安裝階段：跳過等待，立即啟用
self.addEventListener('install', (event) => {
  self.skipWaiting();
});

// 啟用階段：清理舊版快取
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames.map((cache) => {
          if (cache !== CACHE_NAME) {
            return caches.delete(cache);
          }
        })
      );
    })
  );
  self.clients.claim();
});

// 攔截請求：Network First, fallback to Cache
self.addEventListener('fetch', (event) => {
  // 只處理 GET 請求
  if (event.request.method !== 'GET') return;

  const url = new URL(event.request.url);

  // 排除跨域請求（如 Firebase API）與 Chrome 擴充功能
  if (url.origin !== location.origin) return;

  event.respondWith(
    fetch(event.request)
      .then((response) => {
        // 若請求成功，將結果存入快取
        if (response.ok) {
          const responseClone = response.clone();
          caches.open(CACHE_NAME).then((cache) => {
            cache.put(event.request, responseClone);
          });
        }
        return response;
      })
      .catch(() => {
        // 若網路失敗，嘗試從快取讀取
        return caches.match(event.request);
      })
  );
});

/* ═══ EOF ═══
F: AI-WA-UPWO/public/sw.js
V: 1792916300
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./public/sw.js === -->

<!-- === FILE START: ./src/config/constants.js === -->

## 📄 ./src/config/constants.js

```js
// 系統常數
export const SYSTEM_NAME = 'UPGRADE-WO';

// 預設狀態（當資料庫無資料時使用）
export const DEFAULT_STATUSES = ['排產中', '印刷', '裁切', '品檢', '出貨'];

// 路由名稱常數
export const ROUTES = {
  LOGIN: 'login',
  KANBAN: 'kanban',
  TABLE: 'table',
  OVERVIEW: 'overview',
  CATEGORY: 'category',
  SALESPERSON: 'salesperson',
  SETTINGS: 'settings'
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/config/constants.js
V: 1792916600
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/config/constants.js === -->

<!-- === FILE START: ./src/config/firebase-keys.js === -->

## 📄 ./src/config/firebase-keys.js

```js
export const FIREBASE_CONFIG = {
  apiKey: "AIzaSyCOuxzJXxwmPEDYLxg_VHAwPLYikNq8e1c",
  authDomain: "up-wo-87f60.firebaseapp.com",
  projectId: "up-wo-87f60",
  storageBucket: "up-wo-87f60.firebasestorage.app",
  messagingSenderId: "180994034713",
  appId: "1:180994034713:web:fc143f987d14df99102e88"
};

/* ═══ EOF ═══
F: src/config/firebase-keys.js
V: 1792903501
W: webapp-20261026-001#13
═══ */```

<!-- === FILE END: ./src/config/firebase-keys.js === -->

<!-- === FILE START: ./src/config/firebase.js === -->

## 📄 ./src/config/firebase.js

```js
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { FIREBASE_CONFIG } from "./firebase-keys.js";

// 初始化 Firebase App
const app = initializeApp(FIREBASE_CONFIG);

// 初始化並匯出 Firestore 實例
export const db = getFirestore(app);

/* ═══ EOF ═══
F: src/config/firebase.js
V: 1792903502
W: webapp-20261026-001#14
═══ */```

<!-- === FILE END: ./src/config/firebase.js === -->

<!-- === FILE START: ./src/config/state.js === -->

## 📄 ./src/config/state.js

```js
class State {
  constructor() {
    this.user = null;
    this.statuses = ['排產中', '印刷', '裁切', '品檢', '出貨'];
    this.navConfig = null; // 💡 新增：導覽選單配置
  }

  // ═══ 使用者相關 ═══
  setUser(user) {
    this.user = user;
    // 存入 sessionStorage 以便重新整理時保持登入狀態
    if (user) {
      sessionStorage.setItem('upgrade_user', JSON.stringify(user));
    } else {
      sessionStorage.removeItem('upgrade_user');
    }
  }

  getUser() {
    if (!this.user) {
      const stored = sessionStorage.getItem('upgrade_user');
      if (stored) {
        try {
          this.user = JSON.parse(stored);
        } catch (e) {
          this.user = null;
        }
      }
    }
    return this.user;
  }

  clearUser() {
    this.user = null;
    sessionStorage.removeItem('upgrade_user');
  }

  // ═══ 狀態相關 ═══
  setStatuses(statuses) {
    if (Array.isArray(statuses) && statuses.length > 0) {
      this.statuses = statuses;
    }
  }

  getStatuses() {
    return this.statuses;
  }

  // ═══ 導覽選單相關 ═══
  setNavConfig(config) {
    if (Array.isArray(config)) {
      this.navConfig = config;
    }
  }

  getNavConfig() {
    return this.navConfig;
  }
}

// 匯出單一實例 (Singleton)
export const AppState = new State();

/*
F: AI-WA-UPWO/src/config/state.js
V: 1793020000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/config/state.js === -->

<!-- === FILE START: ./src/db/read.js === -->

## 📄 ./src/db/read.js

```js
import { db } from "../config/firebase.js";
import { 
  collection, 
  getDocs, 
  onSnapshot, 
  doc, 
  getDoc 
} from "firebase/firestore";

/**
 * 一次性取得整個集合的資料
 * @param {string} collectionName 集合名稱
 * @returns {Promise<Array>} 包含 id 與資料的陣列
 */
export async function getCollection(collectionName) {
  try {
    const colRef = collection(db, collectionName);
    const snapshot = await getDocs(colRef);
    return snapshot.docs.map(doc => ({
      id: doc.id,
      ...doc.data()
    }));
  } catch (error) {
    console.error(`[DB Read Error] getCollection(${collectionName}):`, error);
    throw error;
  }
}

/**
 * 監聽整個集合的即時變更
 * @param {string} collectionName 集合名稱
 * @param {function} callback 回呼函式，接收包含 id 與資料的陣列
 * @returns {function} 取消監聽的函式
 */
export function listenCollection(collectionName, callback) {
  const colRef = collection(db, collectionName);
  
  const unsubscribe = onSnapshot(colRef, (snapshot) => {
    const data = snapshot.docs.map(doc => ({
      id: doc.id,
      ...doc.data()
    }));
    callback(data);
  }, (error) => {
    console.error(`[DB Read Error] listenCollection(${collectionName}):`, error);
  });

  return unsubscribe;
}

/**
 * 一次性取得單一文檔資料
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 * @returns {Promise<Object|null>} 資料物件或 null
 */
export async function getDocument(collectionName, docId) {
  try {
    const docRef = doc(db, collectionName, docId);
    const docSnap = await getDoc(docRef);
    
    if (docSnap.exists()) {
      return { id: docSnap.id, ...docSnap.data() };
    } else {
      return null;
    }
  } catch (error) {
    console.error(`[DB Read Error] getDocument(${collectionName}/${docId}):`, error);
    throw error;
  }
}

/**
 * 監聽單一文檔的即時變更
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 * @param {function} callback 回呼函式，接收資料物件或 null
 * @returns {function} 取消監聽的函式
 */
export function listenDocument(collectionName, docId, callback) {
  const docRef = doc(db, collectionName, docId);

  const unsubscribe = onSnapshot(docRef, (docSnap) => {
    if (docSnap.exists()) {
      callback({ id: docSnap.id, ...docSnap.data() });
    } else {
      callback(null);
    }
  }, (error) => {
    console.error(`[DB Read Error] listenDocument(${collectionName}/${docId}):`, error);
  });

  return unsubscribe;
}

/* ═══ EOF ═══
F: src/db/read.js
V: 1792903503
W: webapp-20261026-001#15
═══ */```

<!-- === FILE END: ./src/db/read.js === -->

<!-- === FILE START: ./src/db/write.js === -->

## 📄 ./src/db/write.js

```js
import { db } from "../config/firebase.js";
import { 
  doc, 
  setDoc, 
  updateDoc, 
  deleteDoc, 
  addDoc,
  collection 
} from "firebase/firestore";

/**
 * 指定 ID 寫入文檔（覆寫或建立）
 * 註：若需「防覆寫」邏輯，應在上層（如 parser.js）先使用 getDocument 檢查
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 * @param {Object} data 要寫入的資料
 */
export async function setDocument(collectionName, docId, data) {
  try {
    const docRef = doc(db, collectionName, docId);
    await setDoc(docRef, data);
    return true;
  } catch (error) {
    console.error(`[DB Write Error] setDocument(${collectionName}/${docId}):`, error);
    throw error;
  }
}

/**
 * 自動生成 ID 新增文檔
 * @param {string} collectionName 集合名稱
 * @param {Object} data 要寫入的資料
 * @returns {Promise<string>} 新增的文檔 ID
 */
export async function addDocument(collectionName, data) {
  try {
    const colRef = collection(db, collectionName);
    const docRef = await addDoc(colRef, data);
    return docRef.id;
  } catch (error) {
    console.error(`[DB Write Error] addDocument(${collectionName}):`, error);
    throw error;
  }
}

/**
 * 更新指定文檔的部分欄位
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 * @param {Object} data 要更新的欄位（僅更新傳入的 key）
 */
export async function updateDocument(collectionName, docId, data) {
  try {
    if (!docId) throw new Error("docId is required for update.");
    if (!data || Object.keys(data).length === 0) throw new Error("data is empty.");
    
    const docRef = doc(db, collectionName, docId);
    await updateDoc(docRef, data);
    return true;
  } catch (error) {
    console.error(`[DB Write Error] updateDocument(${collectionName}/${docId}):`, error);
    throw error;
  }
}

/**
 * 刪除指定文檔
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 */
export async function deleteDocument(collectionName, docId) {
  try {
    if (!docId) throw new Error("docId is required for delete.");
    
    const docRef = doc(db, collectionName, docId);
    await deleteDoc(docRef);
    return true;
  } catch (error) {
    console.error(`[DB Write Error] deleteDocument(${collectionName}/${docId}):`, error);
    throw error;
  }
}

/* ═══ EOF ═══
F: src/db/write.js
V: 1792903504
W: webapp-20261026-001#16
═══ */```

<!-- === FILE END: ./src/db/write.js === -->

<!-- === FILE START: ./src/domain/data-diff.js === -->

## 📄 ./src/domain/data-diff.js

```js
/**
 * 資料差異核對引擎（純函數，不碰 DOM、不碰 Firebase）
 * 職責：比對資料庫現有資料與 Excel 匯入的新資料，找出差異。
 */

/**
 * 將金額字串正規化（去除千分位逗號、空白，轉為數字）
 * 若無法轉換則回傳 null
 * @param {any} val 原始值
 * @returns {number|null}
 */
function normalizeAmount(val) {
  if (val === undefined || val === null || val === '') return null;
  
  // 移除逗號與空白
  const cleanStr = String(val).replace(/,/g, '').trim();
  if (cleanStr === '') return null;

  const num = Number(cleanStr);
  return isNaN(num) ? null : num;
}

/**
 * 比對單一工單的資料差異
 * @param {Object} dbOrder 資料庫現有工單資料
 * @param {Object} excelData Excel 匯入的工單資料 (通常是 formData 結構，即包含 data 物件)
 * @param {Array<string>} diffFields 要核對的欄位清單 (如 ['amount', 'quantity'])
 * @returns {Object|null} 若有差異，回傳差異物件；若無差異或無需比對，回傳 null
 */
export function checkDiff(dbOrder, excelData, diffFields = []) {
  // 如果沒有設定要核對的欄位，預設核對 amount
  const fieldsToCheck = diffFields.length > 0 ? diffFields : ['amount'];
  
  // 只針對手動輸入的工單進行差異標記保護 (Excel 來的就直接覆蓋了)
  if (dbOrder.source !== 'manual') {
    return null;
  }

  const diffs = {};

  fieldsToCheck.forEach(field => {
    // 取得兩邊的值 (可能是核心欄位或在 data 裡面)
    let dbVal = dbOrder[field];
    if (dbVal === undefined && dbOrder.data) {
      dbVal = dbOrder.data[field];
    }

    let excelVal = excelData[field];
    if (excelVal === undefined && excelData.data) {
      excelVal = excelData.data[field];
    }

    // 針對金額類欄位進行正規化比對
    if (field === 'amount' || field.includes('price') || field.includes('cost')) {
      const dbNum = normalizeAmount(dbVal);
      const excelNum = normalizeAmount(excelVal);

      // 若兩邊都有值，且不相等
      if (dbNum !== null && excelNum !== null && dbNum !== excelNum) {
        diffs[field] = {
          db: dbVal,
          excel: excelVal
        };
      }
    } else {
      // 一般文字欄位比對 (轉字串並去除前後空白)
      const dbStr = String(dbVal || '').trim();
      const excelStr = String(excelVal || '').trim();

      if (dbStr !== excelStr && excelStr !== '') {
        diffs[field] = {
          db: dbVal,
          excel: excelVal
        };
      }
    }
  });

  // 若找到差異，回傳差異物件
  if (Object.keys(diffs).length > 0) {
    return diffs;
  }

  return null;
}

/*
F: AI-WA-UPWO/src/domain/data-diff.js
V: 1792999000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/domain/data-diff.js === -->

<!-- === FILE START: ./src/domain/excel-writer.js === -->

## 📄 ./src/domain/excel-writer.js

```js
import * as XLSX from 'xlsx';

/**
 * 將網頁的工單狀態寫回 Excel 活頁簿
 * @param {Object} workbook XLSX 的 workbook 物件
 * @param {Array<string>} targetSheetNames 目標分頁名稱陣列
 * @param {Array<Object>} orders 要更新的工單資料陣列
 * @returns {Object} 更新後的 workbook 物件
 */
export function updateWorkbookState(workbook, targetSheetNames, orders) {
  const sheetsToUpdate = targetSheetNames.length > 0 ? targetSheetNames : workbook.SheetNames;
  
  // 建立單號對應的資料字典，方便快速查詢
  const orderMap = {};
  orders.forEach(o => {
    if (o.id) orderMap[String(o.id).trim()] = o;
  });

  const idKeywords = ['單號', '發票', '工單號碼', '登錄'];
  const cleanStr = (s) => String(s || '').replace(/[\s\u3000]+/g, '').trim();

  for (const sheetName of sheetsToUpdate) {
    const sheet = workbook.Sheets[sheetName];
    if (!sheet) continue;

    // 將分頁轉為二維陣列以尋找表頭與資料列
    const jsonData = XLSX.utils.sheet_to_json(sheet, { header: 1, defval: "" });
    if (jsonData.length === 0) continue;

    // 1. 尋找表頭行
    let headerIdx = -1;
    for (let i = 0; i < Math.min(jsonData.length, 20); i++) {
      const row = jsonData[i];
      if (!row) continue;
      const cleanRow = row.map(cleanStr);
      if (cleanRow.some(cell => idKeywords.includes(cell))) {
        headerIdx = i;
        break;
      }
    }

    if (headerIdx === -1) continue; // 找不到表頭，跳過此分頁

    const headers = jsonData[headerIdx];
    const cleanHeaders = headers.map(cleanStr);

    // 2. 定位核心欄位索引
    let idColIdx = cleanHeaders.findIndex(h => idKeywords.includes(h));
    if (idColIdx === -1) continue;

    let statusColIdx = cleanHeaders.findIndex(h => ['狀態', '生產進度', 'status'].includes(h));
    let routeColIdx = cleanHeaders.findIndex(h => ['製程路線', '製程路線圖管理', 'route'].includes(h));
    let areaColIdx = cleanHeaders.findIndex(h => ['區域', 'area'].includes(h));

    // 3. 若無狀態欄位，則在最後一欄新增
    let newColCount = headers.length;
    if (statusColIdx === -1) {
      statusColIdx = newColCount++;
      XLSX.utils.sheet_add_aoa(sheet, [['狀態']], { origin: { r: headerIdx, c: statusColIdx } });
    }
    if (routeColIdx === -1) {
      routeColIdx = newColCount++;
      XLSX.utils.sheet_add_aoa(sheet, [['製程路線']], { origin: { r: headerIdx, c: routeColIdx } });
    }
    if (areaColIdx === -1) {
      areaColIdx = newColCount++;
      XLSX.utils.sheet_add_aoa(sheet, [['區域']], { origin: { r: headerIdx, c: areaColIdx } });
    }

    // 4. 逐列更新資料
    for (let r = headerIdx + 1; r < jsonData.length; r++) {
      const row = jsonData[r];
      if (!row) continue;

      const rawId = row[idColIdx];
      const orderId = rawId !== undefined && rawId !== null ? String(rawId).trim() : "";
      
      if (!orderId) break; // 遇到空白單號，停止該分頁

      const orderData = orderMap[orderId];
      if (orderData) {
        // 寫入狀態
        if (orderData.status !== undefined) {
          XLSX.utils.sheet_add_aoa(sheet, [[orderData.status]], { origin: { r: r, c: statusColIdx } });
        }
        // 寫入製程路線 (轉為字串)
        if (orderData.route !== undefined) {
          const routeStr = Array.isArray(orderData.route) ? orderData.route.join(' → ') : String(orderData.route || '');
          XLSX.utils.sheet_add_aoa(sheet, [[routeStr]], { origin: { r: r, c: routeColIdx } });
        }
        // 寫入區域
        if (orderData.area !== undefined) {
          XLSX.utils.sheet_add_aoa(sheet, [[orderData.area]], { origin: { r: r, c: areaColIdx } });
        }
      }
    }
  }

  return workbook;
}

/**
 * 將更新後的 workbook 觸發下載
 * @param {Object} workbook 更新後的 workbook 物件
 * @param {string} fileName 下載的檔案名稱 (例如：2026 開單紀錄_已同步.xlsx)
 */
export function downloadWorkbook(workbook, fileName) {
  try {
    // 使用 XLSX 的 writeFile 直接觸發瀏覽器下載
    XLSX.writeFile(workbook, fileName, { bookType: 'xlsx' });
    return true;
  } catch (error) {
    console.error('[Excel Download Error]', error);
    throw error;
  }
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/domain/excel-writer.js
V: 1794186800
W: UPGRADE-009
═══ */```

<!-- === FILE END: ./src/domain/excel-writer.js === -->

<!-- === FILE START: ./src/domain/formula-engine.js === -->

## 📄 ./src/domain/formula-engine.js

```js
/**
 * 公式引擎（純函數，不碰 DOM、不碰 Firebase）
 * 職責：根據系統設定的公式與表單輸入值，計算出目標欄位的結果。
 * 
 * 公式語法：使用 {{欄位Key}} 作為佔位符，例如 "{{quantity}} * {{price}}"。
 */

/**
 * 將公式字串轉換為可執行的 JS 表達式字串
 * @param {string} formula 原始公式字串
 * @param {Object} formData 表單輸入資料 (key-value)
 * @returns {string} 轉換後的 JS 表達式
 */
function parseFormula(formula, formData) {
  // 使用正規表達式尋找 {{...}} 佔位符
  return formula.replace(/\{\{([^}]+)\}\}/g, (match, fieldKey) => {
    const key = fieldKey.trim();
    const rawValue = formData[key];

    // 若值為空或未定義，視為 0 以利計算
    if (rawValue === undefined || rawValue === null || rawValue === '') {
      return '0';
    }

    // 嘗試轉為數字。若可轉換，直接插入數字；若不可轉換（如文字），則包裹引號
    const num = Number(rawValue);
    if (!isNaN(num) && isFinite(num)) {
      return num;
    }

    // 非數字的情況，作為字串字面量傳入（防範語法錯誤）
    // 使用 JSON.stringify 確保特殊字元（如引號）被安全轉義
    return JSON.stringify(rawValue);
  });
}

/**
 * 計算所有設定的公式
 * @param {Object} formulas 公式設定物件，格式為 { targetKey: "formula string" }
 * @param {Object} formData 表單輸入資料，格式為 { fieldKey: value }
 * @returns {Object} 計算結果物件，格式為 { targetKey: result }
 */
export function evaluate(formulas, formData) {
  const results = {};

  if (!formulas || typeof formulas !== 'object') {
    return results;
  }

  for (const [targetKey, formula] of Object.entries(formulas)) {
    if (!formula || typeof formula !== 'string') {
      continue;
    }

    // 若公式中沒有任何佔位符，則直接跳過（可能是無效的字串）
    if (!formula.includes('{{')) {
      continue;
    }

    try {
      // 1. 解析公式，替換佔位符為實際值
      const expression = parseFormula(formula, formData);

      // 2. 動態執行表達式（使用 Function 建構子，避免全域汙染）
      // 注意：此處存在理論上的 XSS 風險，但公式由管理員在設定頁輸入，且系統僅供內部使用，風險可控。
      // 若需最高安全性，應改寫為 AST 解析，但目前以輕量實作為主。
      const result = new Function(`return (${expression});`)();

      // 3. 處理結果型別
      if (typeof result === 'boolean') {
        // 布林值轉為「是/否」以便顯示
        results[targetKey] = result ? '是' : '否';
      } else if (typeof result === 'number') {
        // 數字去除浮點數誤差，最多保留兩位小數
        if (Number.isInteger(result)) {
          results[targetKey] = String(result);
        } else {
          results[targetKey] = result.toFixed(2);
        }
      } else {
        results[targetKey] = String(result);
      }
    } catch (err) {
      console.warn(`[Formula Engine] 公式計算失敗: ${targetKey} -> ${formula}`, err);
      results[targetKey] = '計算錯誤';
    }
  }

  return results;
}

/*
F: AI-WA-UPWO/src/domain/formula-engine.js
V: 1792991000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/domain/formula-engine.js === -->

<!-- === FILE START: ./src/domain/global-search.js === -->

## 📄 ./src/domain/global-search.js

```js
/**
 * 全域搜尋引擎（純函數，不碰 DOM、不碰 Firebase）
 * 職責：跨工單所有欄位進行關鍵字搜尋，並支援進階條件過濾。
 */

/**
 * 將工單物件攤平為一維 key-value 物件，方便進行全欄位比對
 * @param {Object} order 工單物件
 * @returns {Object} 攤平後的物件
 */
function flattenOrder(order) {
  const flat = {};
  
  // 頂層屬性（排除物件型別與 materials 陣列，避免產生 [object Object] 汙染搜尋字串）
  Object.keys(order).forEach(key => {
    if (key !== 'data' && key !== 'diff' && key !== 'materials' && typeof order[key] !== 'object') {
      flat[key] = String(order[key] || '');
    }
  });

  // data 內的屬性
  if (order.data) {
    Object.keys(order.data).forEach(key => {
      flat[`data.${key}`] = String(order.data[key] || '');
    });
  }

  // 工單物料明細（materials 陣列）：將物料名稱與備註納入可搜尋範圍
  if (Array.isArray(order.materials)) {
    order.materials.forEach((mat, idx) => {
      if (mat && typeof mat === 'object') {
        if (mat.name !== undefined) flat[`materials.${idx}.name`] = String(mat.name || '');
        if (mat.note !== undefined) flat[`materials.${idx}.note`] = String(mat.note || '');
      }
    });
  }

  return flat;
}

/**
 * 執行搜尋與過濾
 * @param {Array<Object>} orders 所有工單陣列
 * @param {string} keyword 搜尋關鍵字
 * @param {Object} filters 進階篩選條件
 * @param {string} filters.status 狀態
 * @param {string} filters.category 類別
 * @param {string} filters.startDate 開始日期 (YYYY-MM-DD)
 * @param {string} filters.endDate 結束日期 (YYYY-MM-DD)
 * @returns {Array<Object>} 搜尋結果陣列（附帶 _highlights 標記命中欄位）
 */
export function searchOrders(orders, keyword = '', filters = {}) {
  let result = orders.filter(o => {
    const id = String(o.id || '');
    return id && !id.includes('***') && !id.includes('訂單');
  });

  // 1. 套用進階條件過濾
  if (filters.status) {
    result = result.filter(o => o.status === filters.status);
  }

  if (filters.category) {
    result = result.filter(o => {
      const cat = o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他';
      return cat.toUpperCase() === filters.category.toUpperCase();
    });
  }

  if (filters.startDate) {
    result = result.filter(o => {
      const dateStr = o.uploadDate || '';
      return dateStr >= filters.startDate;
    });
  }

  if (filters.endDate) {
    result = result.filter(o => {
      const dateStr = o.uploadDate || '';
      return dateStr <= filters.endDate;
    });
  }

  // 2. 套用關鍵字搜尋（若有輸入）
  const kw = keyword.trim().toLowerCase();
  if (kw) {
    result = result.filter(order => {
      const flat = flattenOrder(order);
      let matchedKeys = [];

      // 檢查每個欄位是否包含關鍵字
      Object.keys(flat).forEach(key => {
        if (flat[key].toLowerCase().includes(kw)) {
          matchedKeys.push(key);
        }
      });

      if (matchedKeys.length > 0) {
        // 將命中的欄位記錄在訂單物件上（供 UI 高亮使用）
        // 使用非標準屬性 _highlights 避免與資料庫欄位衝突
        order._highlights = matchedKeys;
        return true;
      }
      return false;
    });
  } else {
    // 若無關鍵字，清除舊的 highlight 標記
    result.forEach(order => { delete order._highlights; });
  }

  return result;
}

/*
F: AI-WA-UPWO/src/domain/global-search.js
V: 1793082000
W: UPGRADE-008
*/```

<!-- === FILE END: ./src/domain/global-search.js === -->

<!-- === FILE START: ./src/domain/parser.js === -->

## 📄 ./src/domain/parser.js

```js
import * as XLSX from 'xlsx';

/**
 * 將銷售員字串依據對照表進行標準化
 * @param {string} repString 原始字串 (例如: "王小明 / 123")
 * @param {Array} salesMapping 對照表 (例如: [{ name: "王小明", aliases: ["123"] }])
 * @returns {Array<string>} 標準化後的銷售員名稱陣列
 */
export function normalizeSalesRep(repString, salesMapping) {
  if (!repString || String(repString).trim() === "") return ["未填寫"];
  
  // 依 "/" 分割並去除空白
  const rawNames = String(repString).split('/').map(s => s.trim()).filter(Boolean);
  if (rawNames.length === 0) return ["未填寫"];

  const resultSet = new Set();

  for (const rawName of rawNames) {
    let matched = false;
    
    // 尋找對照表中符合的 name 或 aliases
    for (const mapItem of salesMapping) {
      const isNameMatch = mapItem.name === rawName;
      const isAliasMatch = mapItem.aliases && mapItem.aliases.includes(rawName);
      
      if (isNameMatch || isAliasMatch) {
        resultSet.add(mapItem.name);
        matched = true;
        break;
      }
    }

    // 若無對應，則直接使用原名
    if (!matched) {
      resultSet.add(rawName);
    }
  }

  return Array.from(resultSet);
}

/**
 * 將 Excel 日期序列號轉換為 YYYY-MM-DD 格式
 * @param {any} val 原始數值或字串
 * @returns {string} 格式化後的日期字串
 */
function formatExcelDate(val) {
  if (val === undefined || val === null || val === '') return "";
  
  // 如果已經是正常的日期字串 (包含 - 或 /)
  if (typeof val === 'string' && (val.includes('-') || val.includes('/'))) {
    if (val.includes('/')) {
      const parts = val.split('/');
      if (parts.length === 3) {
        if (parts[0].length === 4) return `${parts[0]}-${parts[1].padStart(2,'0')}-${parts[2].padStart(2,'0')}`;
        if (parts[2].length === 4) return `${parts[2]}-${parts[1].padStart(2,'0')}-${parts[0].padStart(2,'0')}`;
      }
    }
    return val.trim();
  }

  // 處理 Excel 序列號 (通常介於 20000 到 80000 之間)
  const num = Number(val);
  if (!isNaN(num) && num > 20000 && num < 80000) {
    // Excel 起始日為 1899-12-30
    const date = new Date((num - 25569) * 86400 * 1000);
    if (!isNaN(date.getTime())) {
      return date.toISOString().split('T')[0];
    }
  }

  // 其他無法解析的，直接轉字串回傳
  return String(val).trim();
}

/**
 * 解析 Excel 匯入的資料列（自動偵測表頭 + 動態 Schema）
 * @param {Array<string>} headers Excel 第一列的標題 (可能不是真正的表頭)
 * @param {Array<Array<any>>} rows Excel 的資料列（不含第一列）
 * @param {Array} salesMapping 銷售員對照表
 * @param {string} defaultStatus 預設狀態 (如 "排產中")
 * @param {Array} schema 系統欄位定義
 * @param {string} targetMonth 指定過濾的月份 (格式 YYYY-MM，若為空則不篩選)
 * @returns {Array<Object>} 標準化後的工單物件陣列
 */
export function parseExcelData(headers, rows, salesMapping, defaultStatus = "排產中", schema = [], targetMonth = "") {
  const nowStr = new Date().toISOString();
  const parsedOrders = [];

  const allData = [headers, ...rows];

  // 1. 自動偵測表頭行數 (掃描前 20 行，尋找包含「單號」、「發票」、「工單號碼」、「登錄」的行)
  const idKeywords = ['單號', '發票', '工單號碼', '登錄'];
  let headerIdx = -1;
  for (let i = 0; i < Math.min(allData.length, 20); i++) {
    const row = allData[i];
    if (!row) continue;
    const cleanRow = row.map(c => String(c || '').replace(/[\s\u3000]+/g, '').trim());
    if (cleanRow.some(cell => idKeywords.includes(cell))) {
      headerIdx = i;
      break;
    }
  }

  if (headerIdx === -1) {
    return [];
  }

  const actualHeaders = allData[headerIdx];
  const dataRows = allData.slice(headerIdx + 1);

  // 2. 建立表頭索引映射：強制清洗表頭的空白、換行與全形空白
  const cleanStr = (s) => String(s || '').replace(/[\s\u3000]+/g, '').trim();
  const cleanHeaders = actualHeaders.map(cleanStr);

  const columnMap = {};
  schema.forEach(field => {
    const targetLabel = cleanStr(field.label);
    const idx = cleanHeaders.findIndex(h => h === targetLabel);
    if (idx !== -1) {
      columnMap[field.key] = idx;
    }
  });

  // 核心欄位別名映射表 (用於識別匯出的 Excel 或公司系統報表)
  const coreAliases = {
    id: ['工單號碼', '單號', '發票', 'id', '登錄'],
    status: ['狀態', '生產進度', 'status'],
    route: ['製程路線', '製程路線圖管理', 'route'],
    area: ['區域', 'area'],
    dueDate: ['交貨日期', '交貨日', '預計日期', 'dueDate'],
    uploadDate: ['上傳日期', '開單日期', '日期', 'uploadDate'],
    salesReps: ['銷售員', 'salesRep', 'salesReps', '我方備考', 'SALES編號', 'SALES'],
    source: ['來源', 'source'],
    supplierName: ['外發商', '工單類別/外發商', '供應商', '外發廠商'],
    category: ['類別', 'category', '工單類別']
  };

  // 找出核心欄位在 Excel 中的索引
  const coreIndices = {};
  Object.keys(coreAliases).forEach(key => {
    const aliases = coreAliases[key];
    const idx = cleanHeaders.findIndex(h => aliases.includes(h));
    if (idx !== -1) {
      coreIndices[key] = idx;
    }
  });

  let idIdx = coreIndices['id'];
  if (idIdx === undefined) {
    idIdx = columnMap['id'];
    if (idIdx !== undefined) coreIndices['id'] = idIdx;
  }

  if (idIdx === undefined) {
    return [];
  }

  // 3. 逐列解析
  for (const row of dataRows) {
    if (!row || row.length === 0) continue;

    const rawIdVal = row[coreIndices['id']];
    const orderId = rawIdVal !== undefined && rawIdVal !== null ? String(rawIdVal).trim() : "";
    
    if (!orderId) {
      break;
    }

    // --- 解析核心欄位 ---
    
    // 上傳/開單日期 (uploadDate) - 先解析出來，因為要用來比對月份
    let uploadDate = nowStr.split('T')[0];
    if (coreIndices['uploadDate'] !== undefined && row[coreIndices['uploadDate']] !== undefined) {
      const formatted = formatExcelDate(row[coreIndices['uploadDate']]);
      if (formatted) uploadDate = formatted;
    }

    // 月份過濾防呆：若有指定月份，且開單日期不符合，則跳過該筆
    if (targetMonth && !uploadDate.startsWith(targetMonth)) {
      continue;
    }

    // 狀態 (status)
    let status = defaultStatus;
    if (coreIndices['status'] !== undefined && row[coreIndices['status']] !== undefined) {
      const val = String(row[coreIndices['status']]).trim();
      if (val) status = val;
    }

    // 製程路線 (route)
    let route = [];
    if (coreIndices['route'] !== undefined && row[coreIndices['route']] !== undefined) {
      const val = String(row[coreIndices['route']]).trim();
      if (val) {
        if (val.includes(',')) {
          route = val.split(',').map(s => s.trim()).filter(Boolean);
        } else if (val.includes('→')) {
          route = val.split('→').map(s => s.trim()).filter(Boolean);
        } else {
          route = [val];
        }
      }
    }

    // 區域 (area)
    let area = "";
    if (coreIndices['area'] !== undefined && row[coreIndices['area']] !== undefined) {
      const val = String(row[coreIndices['area']]).trim();
      if (val) area = val;
    }

    // 交貨日期 (dueDate)
    let dueDate = "";
    if (coreIndices['dueDate'] !== undefined && row[coreIndices['dueDate']] !== undefined) {
      dueDate = formatExcelDate(row[coreIndices['dueDate']]);
    }

    // 來源 (source)
    let source = "excel";
    if (coreIndices['source'] !== undefined && row[coreIndices['source']] !== undefined) {
      const val = String(row[coreIndices['source']]).trim();
      if (val === '手動' || val === 'manual') source = 'manual';
    }

    // 銷售員 (salesReps)
    let rawSalesRep = "";
    if (coreIndices['salesReps'] !== undefined && row[coreIndices['salesReps']] !== undefined) {
      rawSalesRep = String(row[coreIndices['salesReps']]);
    } else if (columnMap['salesRep'] !== undefined && row[columnMap['salesRep']] !== undefined) {
      rawSalesRep = String(row[columnMap['salesRep']]);
    }
    const normalizedReps = normalizeSalesRep(rawSalesRep, salesMapping);

    // 外發商 (supplierName)
    let supplierName = "";
    if (coreIndices['supplierName'] !== undefined && row[coreIndices['supplierName']] !== undefined) {
      supplierName = String(row[coreIndices['supplierName']]).trim();
    }

    // --- 收集動態欄位資料 ---
    const data = {};
    
    if (coreIndices['category'] !== undefined && row[coreIndices['category']] !== undefined) {
      const val = String(row[coreIndices['category']]).trim();
      if (val) {
        data['category'] = val.toUpperCase();
      }
    }
    
    if (!data['category']) {
      const categoryMatch = orderId.match(/^[a-zA-Z]+/);
      data['category'] = categoryMatch ? categoryMatch[0].toUpperCase() : '其他';
    }

    schema.forEach(field => {
      const isCoreHandled = ['id', 'status', 'route', 'area'].includes(field.key);
      if (!field.isSystem && !isCoreHandled && columnMap[field.key] !== undefined) {
        let val = row[columnMap[field.key]];
        
        // 💡 防呆：若該自訂欄位被定義為 date 類型，強制轉換為標準日期格式
        if (field.type === 'date' && val !== undefined && val !== null && val !== '') {
          val = formatExcelDate(val);
        }
        
        data[field.key] = val !== undefined && val !== null ? val : "";
      }
    });

    parsedOrders.push({
      id: orderId,
      status: status,
      route: route,
      area: area,
      salesReps: normalizedReps,
      supplierName: supplierName,
      uploadDate: uploadDate,
      statusUpdateDate: nowStr.split('T')[0],
      dueDate: dueDate,
      source: source,
      data: data
    });
  }

  return parsedOrders;
}

/**
 * 解析整份 Excel 活頁簿（支援指定分頁、多分頁合併與月份過濾）
 * @param {Object} workbook XLSX 的 workbook 物件
 * @param {Array<string>} targetSheetNames 指定要讀取的分頁名稱
 * @param {Array} salesMapping 銷售員對照表
 * @param {string} defaultStatus 預設狀態
 * @param {Array} schema 系統欄位定義
 * @param {string} targetMonth 指定過濾的月份 (格式 YYYY-MM)
 * @returns {Array<Object>} 合併後的工單物件陣列
 */
export function parseWorkbook(workbook, targetSheetNames = [], salesMapping, defaultStatus, schema, targetMonth = "") {
  const allParsedOrders = [];
  const sheetsToParse = targetSheetNames.length > 0 ? targetSheetNames : workbook.SheetNames;

  for (const sheetName of sheetsToParse) {
    if (!workbook.Sheets[sheetName]) continue;

    const sheet = workbook.Sheets[sheetName];
    const jsonData = XLSX.utils.sheet_to_json(sheet, { header: 1 });

    if (jsonData.length === 0) continue;

    const parsed = parseExcelData(
      jsonData[0], 
      jsonData.slice(1), 
      salesMapping, 
      defaultStatus, 
      schema,
      targetMonth
    );

    if (parsed.length > 0) {
      allParsedOrders.push(...parsed);
    }
  }

  // 去除重複單號
  const uniqueOrders = [];
  const seenIds = new Set();
  
  for (let i = allParsedOrders.length - 1; i >= 0; i--) {
    const order = allParsedOrders[i];
    if (!seenIds.has(order.id)) {
      seenIds.add(order.id);
      uniqueOrders.unshift(order);
    }
  }

  return uniqueOrders;
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/domain/parser.js
V: 1794188400
W: UPGRADE-009
═══ */```

<!-- === FILE END: ./src/domain/parser.js === -->

<!-- === FILE START: ./src/domain/stats.js === -->

## 📄 ./src/domain/stats.js

```js
/**
 * 將工單陣列依「狀態」分組
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} allStatuses 所有可能的狀態清單（確保空狀態也有空陣列）
 * @returns {Object} 以狀態為 key 的物件，value 為該狀態的工單陣列
 */
export function groupByStatus(orders, allStatuses = []) {
  const grouped = {};
  
  // 先初始化所有狀態為空陣列
  allStatuses.forEach(status => {
    grouped[status] = [];
  });

  // 遍歷工單進行分組
  orders.forEach(order => {
    const status = order.status || '未知狀態';
    if (!grouped[status]) {
      grouped[status] = [];
    }
    grouped[status].push(order);
  });

  return grouped;
}

/**
 * 將工單陣列依「銷售員」分組
 * @param {Array<Object>} orders 工單陣列
 * @returns {Object} 以銷售員為 key 的物件，value 為該銷售員的工單陣列
 */
export function groupBySalesRep(orders) {
  const grouped = {};

  orders.forEach(order => {
    const reps = order.salesReps || ['未填寫'];
    reps.forEach(rep => {
      if (!grouped[rep]) {
        grouped[rep] = [];
      }
      grouped[rep].push(order);
    });
  });

  return grouped;
}

/**
 * 將工單陣列依「類別 (發票前兩碼)」分組
 * @param {Array<Object>} orders 工單陣列
 * @returns {Object} 以類別為 key 的物件，value 為該類別的工單陣列
 */
export function groupByCategory(orders) {
  const grouped = {};

  orders.forEach(order => {
    // 優先從 data.category 抓取，若無則嘗試從 id 擷取
    let category = order.data && order.data['category'];
    
    if (!category) {
      const match = String(order.id).match(/^[a-zA-Z]+/);
      category = match ? match[0].toUpperCase() : '其他';
    }

    if (!grouped[category]) {
      grouped[category] = [];
    }
    grouped[category].push(order);
  });

  return grouped;
}

/**
 * 計算總體統計數據（用於 Overview 視圖或 Stats 卡片）
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} allStatuses 所有狀態清單
 * @returns {Object} 包含總數與各狀態數量的物件
 */
export function calculateStats(orders, allStatuses = []) {
  const stats = {
    total: orders.length,
    byStatus: {}
  };

  allStatuses.forEach(status => {
    stats.byStatus[status] = 0;
  });

  orders.forEach(order => {
    const status = order.status || '未知狀態';
    if (!stats.byStatus[status]) {
      stats.byStatus[status] = 0;
    }
    stats.byStatus[status]++;
  });

  return stats;
}

/**
 * 生成樞紐交叉表資料（用於 Overview 視圖上半部）
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} statuses 狀態清單（列）
 * @param {Array<string>} salesReps 銷售員清單（欄）
 * @returns {Object} 包含表頭與二維資料的物件
 */
export function generatePivotTable(orders, statuses = [], salesReps = []) {
  // 建立二維陣列結構
  const matrix = {};
  
  // 初始化矩陣
  statuses.forEach(status => {
    matrix[status] = {};
    salesReps.forEach(rep => {
      matrix[status][rep] = 0;
    });
  });

  // 填充數據
  orders.forEach(order => {
    const status = order.status || '未知狀態';
    const reps = order.salesReps || ['未填寫'];
    
    reps.forEach(rep => {
      // 如果該銷售員不在表頭清單中，動態加入
      if (!matrix[status]) {
        matrix[status] = {};
      }
      if (matrix[status][rep] === undefined) {
        matrix[status][rep] = 0;
      }
      matrix[status][rep]++;
    });
  });

  return matrix;
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/domain/stats.js
V: 1792911600
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/domain/stats.js === -->

<!-- === FILE START: ./src/engine/main.js === -->

## 📄 ./src/engine/main.js

```js
import { initRouter } from './router.js';
import { getCollection, listenDocument } from '../db/read.js';
import { updateDocument } from '../db/write.js';
import { AppState } from '../config/state.js';
import { initSyncEngine } from './sync-engine.js';
import '../ui/order-view-editor.js';

const defaultNavConfig = [
  { id: 'kanban', label: '工單進度表', icon: 'layout-dashboard', showSidebar: true, showTop: true, isEnabled: true },
  { id: 'search', label: '全域搜尋', icon: 'search', showSidebar: true, showTop: false, isEnabled: true },
  { id: 'table', label: '管理總表', icon: 'table', showSidebar: true, showTop: true, isEnabled: true },
  { id: 'overview', label: '總狀態', icon: 'pie-chart', showSidebar: true, showTop: true, isEnabled: true },
  { id: 'category', label: '類別視圖', icon: 'layers', showSidebar: true, showTop: true, isEnabled: true },
  { id: 'salesperson', label: '銷售員', icon: 'users', showSidebar: true, showTop: true, isEnabled: true },
  { id: 'area', label: '生產區域', icon: 'map-pin', showSidebar: true, showTop: false, isEnabled: true },
  { id: 'settings-material', label: '資料庫中心', icon: 'database', showSidebar: true, showTop: false, isEnabled: true },
  { id: 'settings', label: '生產管理設定', icon: 'settings', showSidebar: true, showTop: false, isEnabled: true },
  { id: 'settings-form', label: '工單管理設定', icon: 'sliders', showSidebar: true, showTop: false, isEnabled: true },
  { id: 'settings-system', label: '系統管理設定', icon: 'shield', showSidebar: true, showTop: false, isEnabled: true }
];

document.addEventListener('DOMContentLoaded', () => {
  const root = document.getElementById('app-root');
  
  if (!root) {
    console.error('[System Error] 找不到 #app-root 容器，無法啟動系統。');
    return;
  }

  const loadingEl = document.getElementById('app-loading');
  if (loadingEl) {
    loadingEl.classList.add('hidden');
  }

  try {
    initRouter();
    console.log('[System] UPGRADE-WO 系統啟動成功。');
    listenSettingsUpdates();
    fixLegacyDatesInBackground();
    registerServiceWorker();
    
    // 初始化全域 Excel 同步引擎 (恢復綁定狀態、啟動唯讀輪詢)
    initSyncEngine();
    
  } catch (error) {
    console.error('[System Error] 系統啟動失敗:', error);
    root.innerHTML = `
      <div class="min-h-screen flex items-center justify-center bg-bg text-slate-600">
        <div class="glass-card p-8 text-center max-w-md">
          <h2 class="text-xl font-bold text-rose-500 mb-2">系統啟動失敗</h2>
          <p class="text-sm">請檢查 Console 錯誤訊息，或重新整理頁面。</p>
        </div>
      </div>
    `;
  }
});

function listenSettingsUpdates() {
  listenDocument('settings', 'main', async (data) => {
    if (!data) return;
    
    let needSave = false;
    let updatePayload = {};
    
    if (data.statuses) AppState.setStatuses(data.statuses);
    
    // 1. 導覽列遷移
    if (data.nav_config && Array.isArray(data.nav_config)) {
      let configUpdated = false;
      const migratedConfig = data.nav_config.map(item => {
        if (item.id === 'admin-data') {
          configUpdated = true;
          return null; 
        }
        if (item.id === 'settings-material' && item.label === '物料管理') {
          item.label = '資料庫中心';
          item.icon = 'database';
          configUpdated = true;
        }
        return item;
      }).filter(Boolean);

      const newSettingsPages = ['settings-material', 'settings-system'];
      newSettingsPages.forEach(id => {
        if (!migratedConfig.some(c => c.id === id)) {
          const defaultItem = defaultNavConfig.find(c => c.id === id);
          if (defaultItem) {
            migratedConfig.push(defaultItem);
            configUpdated = true;
          }
        }
      });

      if (configUpdated) {
        updatePayload.nav_config = migratedConfig;
        needSave = true;
      }
      AppState.setNavConfig(data.nav_config);
    } else {
      AppState.setNavConfig(defaultNavConfig);
      updatePayload.nav_config = defaultNavConfig;
      needSave = true;
    }

    // 2. 低程式碼資料庫無損遷移
    let currentDbs = Array.isArray(data.custom_databases) ? [...data.custom_databases] : [];
    let dbsChanged = false;

    // 確保供應商存在
    if (!currentDbs.some(db => db.id === 'db_suppliers')) {
      currentDbs.push({
        id: 'db_suppliers',
        name: '外發供應商',
        fields: data.supplier_schema || [],
        items: data.suppliers || []
      });
      dbsChanged = true;
    }

    // 確保物料存在
    if (!currentDbs.some(db => db.id === 'db_materials')) {
      currentDbs.push({
        id: 'db_materials',
        name: '物料資料',
        fields: data.material_schema || [],
        items: data.materials || []
      });
      dbsChanged = true;
    }

    // 確保銷售員存在
    if (!currentDbs.some(db => db.id === 'db_sales')) {
      const salesItems = (data.sales_mapping || []).map((sm, i) => ({
        id: 'sm_' + i,
        name: sm.name,
        data: { aliases: (sm.aliases || []).join(', ') }
      }));
      currentDbs.push({
        id: 'db_sales',
        name: '銷售員對照表',
        fields: [{ key: 'aliases', label: '別名 (逗號隔開)', type: 'string' }],
        items: salesItems
      });
      dbsChanged = true;
    }

    if (dbsChanged) {
      updatePayload.custom_databases = currentDbs;
      needSave = true;
    }

    if (needSave) {
      try {
        await updateDocument('settings', 'main', updatePayload);
      } catch (err) {
        console.warn('無法儲存預設或遷移配置', err);
      }
    }
    
    window.dispatchEvent(new Event('settings-updated'));
    window.dispatchEvent(new Event('nav-config-updated'));
  });
}

function formatDate(val) {
  if (!val) return "";
  if (typeof val === 'string' && (val.includes('-') || val.includes('/'))) {
    return val.trim();
  }
  const num = Number(val);
  if (!isNaN(num) && num > 20000 && num < 80000) {
    const date = new Date((num - 25569) * 86400 * 1000);
    if (!isNaN(date.getTime())) {
      return date.toISOString().split('T')[0];
    }
  }
  return String(val).trim();
}

async function fixLegacyDatesInBackground() {
  try {
    const userStr = sessionStorage.getItem('upgrade_user');
    if (!userStr) return; 
    
    const orders = await getCollection('work_orders');
    let fixCount = 0;

    for (const order of orders) {
      let needsUpdate = false;
      const updates = {};

      if (order.uploadDate && (typeof order.uploadDate === 'number' || (typeof order.uploadDate === 'string' && !order.uploadDate.includes('-')))) {
        const formatted = formatDate(order.uploadDate);
        if (formatted !== order.uploadDate && formatted !== '') {
          updates.uploadDate = formatted;
          needsUpdate = true;
        }
      }

      if (order.dueDate && (typeof order.dueDate === 'number' || (typeof order.dueDate === 'string' && !order.dueDate.includes('-')))) {
        const formatted = formatDate(order.dueDate);
        if (formatted !== order.dueDate && formatted !== '') {
          updates.dueDate = formatted;
          needsUpdate = true;
        }
      }

      if (needsUpdate) {
        await updateDocument('work_orders', order.id, updates);
        fixCount++;
      }
    }

    if (fixCount > 0) {
      console.log(`[System] 背景自動修正了 ${fixCount} 筆工單的日期格式。`);
      window.dispatchEvent(new Event('data:dates-fixed'));
    }

  } catch (error) {
    console.warn('[System Background Task] 自動修正日期時發生錯誤:', error);
  }
}

function registerServiceWorker() {
  if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
      navigator.serviceWorker.register('/sw.js')
        .then((registration) => {
          console.log('[PWA] ServiceWorker 註冊成功，範圍為:', registration.scope);
        })
        .catch((error) => {
          console.warn('[PWA] ServiceWorker 註冊失敗:', error);
        });
    });
  }
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/engine/main.js
V: 1794187100
W: UPGRADE-009
═══ */```

<!-- === FILE END: ./src/engine/main.js === -->

<!-- === FILE START: ./src/engine/router.js === -->

## 📄 ./src/engine/router.js

```js
import { AppState } from '../config/state.js';
import { renderAppShell, updateNavbar } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { getDocument } from '../db/read.js';
import { updateDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';

// 引入各頁面模組
import LoginPage from '../pages/login.js';
import KanbanPage from '../pages/kanban.js';
import TablePage from '../pages/table.js';
import OverviewPage from '../pages/overview.js';
import SalespersonPage from '../pages/salesperson.js';
import CategoryPage from '../pages/category.js';
import AdminDataPage from '../pages/admin-data.js';
import AreaPage from '../pages/area.js';
import SearchPage from '../pages/search.js';

// 引入四大設定中心
import SettingsPage from '../pages/settings.js';
import SettingsFormPage from '../pages/settings-form.js';

// 💡 引入全新的資料庫中心與系統管理
import { renderDatabaseCenter } from '../pages/settings-database.js';
import { renderSystemUsers, renderSystemData } from '../pages/settings-system.js';

// 💡 將資料庫中心包裝成標準 Page 模組
const SettingsMaterialPage = {
  async render() {
    const user = AppState.getUser();
    if (!user || user.role !== 'admin') {
      document.getElementById('app-content').innerHTML = '<div class="p-8 text-center text-rose-500 font-bold">您沒有權限訪問此頁面</div>';
      return;
    }
    
    let currentSettings = {};
    try {
      currentSettings = await getDocument('settings', 'main') || {};
    } catch(e) {}
    
    renderPageLayout({
      title: '資料庫中心',
      subtitle: '建立與管理系統中所有自訂資料庫（如：外發商、物料、客戶）',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'settings-material-container'
    });

    const container = document.getElementById('settings-material-container');
    
    // 傳入儲存函數，僅更新資料庫相關欄位，保護原有資料
    const saveSettings = async (dataToSave) => {
      await updateDocument('settings', 'main', dataToSave);
      const updated = await getDocument('settings', 'main');
      renderDatabaseCenter(container, updated || {}, saveSettings);
    };

    renderDatabaseCenter(container, currentSettings, saveSettings);
  }
};

const SettingsSystemPage = {
  async render() {
    const user = AppState.getUser();
    if (!user || user.role !== 'admin') {
      document.getElementById('app-content').innerHTML = '<div class="p-8 text-center text-rose-500 font-bold">您沒有權限訪問此頁面</div>';
      return;
    }
    
    let currentSettings = {};
    try {
      currentSettings = await getDocument('settings', 'main') || {};
    } catch(e) {}

    // 獲取銷售員名單供權限設定使用
    let allSalesReps = [];
    try {
      const { getCollection } = await import('../db/read.js');
      const orders = await getCollection('work_orders');
      const reps = new Set();
      orders.forEach(o => (o.salesReps || []).forEach(r => { if(r && r !== '未填寫') reps.add(r); }));
      allSalesReps = Array.from(reps).sort();
    } catch(e) {}
    
    renderPageLayout({
      title: '系統管理設定',
      subtitle: '管理登入帳號權限與系統資料',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'settings-system-container'
    });

    const container = document.getElementById('settings-system-container');
    container.innerHTML = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden min-h-[500px] flex flex-col md:flex-row">
        <div class="w-full md:w-64 border-r border-slate-100 bg-slate-50/50 p-4 flex flex-col gap-1 flex-shrink-0">
          <button id="tab-sys-users" class="bg-white text-brand shadow-sm border border-slate-200/50 text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3">
            <i data-lucide="shield" class="w-4 h-4"></i> 帳號管理
          </button>
          <button id="tab-sys-data" class="text-slate-600 hover:bg-slate-100 border border-transparent text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3">
            <i data-lucide="database" class="w-4 h-4"></i> 資料管理
          </button>
        </div>
        <div class="flex-1 p-6 md:p-8" id="sys-content"></div>
      </div>
    `;

    const contentEl = container.querySelector('#sys-content');
    const btnUsers = container.querySelector('#tab-sys-users');
    const btnData = container.querySelector('#tab-sys-data');

    const saveSettings = async (dataToSave) => {
      await updateDocument('settings', 'main', dataToSave);
      const updated = await getDocument('settings', 'main');
      currentSettings = updated || {};
    };

    const renderUsers = () => {
      btnUsers.className = 'bg-white text-brand shadow-sm border border-slate-200/50 text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3';
      btnData.className = 'text-slate-600 hover:bg-slate-100 border border-transparent text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3';
      renderSystemUsers(contentEl, currentSettings, saveSettings, allSalesReps);
    };

    const renderData = () => {
      btnData.className = 'bg-white text-brand shadow-sm border border-slate-200/50 text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3';
      btnUsers.className = 'text-slate-600 hover:bg-slate-100 border border-transparent text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3';
      renderSystemData(contentEl, currentSettings, saveSettings);
    };

    btnUsers.addEventListener('click', renderUsers);
    btnData.addEventListener('click', renderData);

    // 預設顯示帳號管理
    renderUsers();
    
    // 渲染圖示
    import('lucide').then(({ createIcons, Shield, Database }) => {
      createIcons({ icons: { Shield, Database }, attrs: { class: 'w-4 h-4' } });
    });
  }
};

const routes = {
  login: LoginPage,
  kanban: KanbanPage,
  table: TablePage,
  overview: OverviewPage,
  salesperson: SalespersonPage,
  category: CategoryPage,
  'admin-data': AdminDataPage,
  area: AreaPage,
  search: SearchPage,
  settings: SettingsPage,
  'settings-form': SettingsFormPage,
  'settings-material': SettingsMaterialPage,
  'settings-system': SettingsSystemPage
};

let currentPageInstance = null;
let isShellRendered = false;

function handleRoute() {
  let hash = window.location.hash.replace('#', '') || 'kanban';
  const user = AppState.getUser();

  if (!user) {
    if (hash !== 'login') {
      window.location.hash = '#login';
      return;
    }
  } else {
    if (hash === 'login') {
      window.location.hash = user.role === 'worker' ? '#area' : (user.role === 'sales' ? '#salesperson' : '#kanban');
      return;
    }
  }

  if (user) {
    const role = user.role;

    if (role === 'worker' && hash !== 'area') {
      window.location.hash = '#area';
      return;
    }

    if (role === 'sales' && hash !== 'salesperson') {
      window.location.hash = '#salesperson';
      return;
    }

    const isSettingsPage = ['settings', 'settings-form', 'settings-material', 'settings-system', 'admin-data'].includes(hash);
    if (role === 'viewer' && isSettingsPage) {
      showToast('您沒有權限訪問此頁面', 'error');
      window.location.hash = '#kanban';
      return;
    }

    const navConfig = AppState.getNavConfig();
    if (navConfig) {
      const routeConfig = navConfig.find(r => r.id === hash);
      if (routeConfig && routeConfig.isEnabled === false) {
        showToast('該頁面已被停用', 'error');
        window.location.hash = '#table';
        return;
      }
    }
  }

  const PageClass = routes[hash];
  if (!PageClass) {
    window.location.hash = user ? (user.role === 'worker' ? '#area' : (user.role === 'sales' ? '#salesperson' : '#kanban')) : '#login';
    return;
  }

  if (hash === 'login') {
    isShellRendered = false;
    if (currentPageInstance && typeof currentPageInstance.destroy === 'function') {
      currentPageInstance.destroy();
    }
    currentPageInstance = PageClass;
    currentPageInstance.render();
  } else {
    if (!isShellRendered) {
      renderAppShell({
        user: user,
        onLogout: handleLogout,
        onNavigate: (route) => { window.location.hash = `#${route}`; }
      });
      isShellRendered = true;
    }

    updateNavbar(hash);

    if (currentPageInstance && typeof currentPageInstance.destroy === 'function') {
      currentPageInstance.destroy();
    }

    currentPageInstance = PageClass;
    currentPageInstance.render();
  }
}

function handleLogout() {
  AppState.clearUser();
  isShellRendered = false;
  window.location.hash = '#login';
  showToast('已登出系統', 'info');
}

export function initRouter() {
  window.addEventListener('hashchange', handleRoute);

  window.addEventListener('auth:login', () => {
    const user = AppState.getUser();
    if (user.role === 'worker') {
      window.location.hash = '#area';
    } else if (user.role === 'sales') {
      window.location.hash = '#salesperson';
    } else {
      window.location.hash = '#kanban';
    }
  });

  handleRoute();
}

/*
F: AI-WA-UPWO/src/engine/router.js
V: 1793087000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/engine/router.js === -->

<!-- === FILE START: ./src/engine/sync-engine.js === -->

## 📄 ./src/engine/sync-engine.js

```js
import * as XLSX from 'xlsx';
import { getDocument, getCollection } from '../db/read.js';
import { updateDocument, setDocument } from '../db/write.js';
import { parseWorkbook } from '../domain/parser.js';
import { updateWorkbookState, downloadWorkbook } from '../domain/excel-writer.js';
import { AppState } from '../config/state.js';
import { showToast } from '../ui/toast.js';

let currentFile = null;
let targetFileName = '';
let selectedSheets = [];
let targetMonth = '';
let isSyncing = false;

let currentState = { isBound: false, fileName: '', sheets: [] };
let stateListeners = [];

export function onSyncStateChange(callback) {
  stateListeners.push(callback);
  callback(currentState);
}

function notifyStateChange() {
  stateListeners.forEach(cb => cb(currentState));
}

export function initSyncEngine() {
  try {
    const configStr = localStorage.getItem('excel_sync_config');
    if (configStr) {
      const config = JSON.parse(configStr);
      targetFileName = config.fileName || '';
      selectedSheets = config.sheets || [];
      targetMonth = config.month || '';
      
      currentState = { isBound: true, fileName: targetFileName, sheets: selectedSheets };
      notifyStateChange();
    }
  } catch (err) {
    console.warn('[Sync Engine] 恢復設定失敗', err);
  }
}

// 新增：解除綁定功能
export function clearBinding() {
  currentFile = null;
  targetFileName = '';
  selectedSheets = [];
  targetMonth = '';
  localStorage.removeItem('excel_sync_config');
  currentState = { isBound: false, fileName: '', sheets: [] };
  notifyStateChange();
  showToast('已解除綁定，請重新上傳', 'success');
}

export function handleExcelUpload(file) {
  console.log('[Sync Engine] 收到上傳檔案', file.name);
  if (!file) return;

  if (!file.name.endsWith('.xlsx') && !file.name.endsWith('.xls')) {
    showToast('請選擇 Excel 檔案 (.xlsx 或 .xls)', 'error');
    return;
  }

  const reader = new FileReader();
  reader.onload = async (evt) => {
    try {
      const buffer = evt.target.result;
      const workbook = XLSX.read(buffer, { type: 'array' });
      
      // 只要檔案有選擇，一律跳出設定視窗，讓使用者重新確認分頁與月份
      showSetupModal(file.name, workbook.SheetNames, buffer);
    } catch (err) {
      console.error('[Sync Engine] 解析檔案失敗', err);
      showToast(`無法解析此 Excel 檔案: ${err.message}`, 'error');
    }
  };
  reader.readAsArrayBuffer(file);
}

function showSetupModal(fileName, sheetNames, buffer) {
  const overlay = document.createElement('div');
  overlay.className = 'fixed inset-0 bg-black/40 backdrop-blur-sm z-[100] flex items-center justify-center p-4';
  
  const modal = document.createElement('div');
  modal.className = 'bg-white border border-slate-100 shadow-soft rounded-[20px] w-full max-w-md flex flex-col max-h-[90vh] overflow-hidden';
  
  modal.innerHTML = `
    <div class="flex justify-between items-center p-5 border-b border-slate-100 flex-shrink-0">
      <h3 class="text-lg font-bold text-slate-900">設定同步分頁與月份</h3>
      <button id="close-setup-modal" class="text-slate-400 hover:text-slate-600">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
      </button>
    </div>
    <div class="p-5 text-slate-600 overflow-y-auto flex-1 flex flex-col gap-6">
      <p class="text-sm font-bold text-slate-800 mb-0">檔案：${fileName}</p>
      
      <div class="flex flex-col gap-2">
        <label class="text-sm font-bold text-slate-800">1. 選擇要讀取的工單分頁</label>
        <div class="flex flex-col gap-2 max-h-[200px] overflow-y-auto">
          ${sheetNames.map(name => `
            <label class="flex items-center gap-3 p-3 bg-slate-50 border border-slate-200 rounded-lg cursor-pointer hover:border-brand transition-colors">
              <input type="checkbox" class="sheet-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${name}" ${selectedSheets.includes(name) ? 'checked' : ''}>
              <span class="font-medium text-slate-700">${name}</span>
            </label>
          `).join('')}
        </div>
      </div>

      <div class="flex flex-col gap-2">
        <label class="text-sm font-bold text-slate-800">2. 指定匯入月份（選填）</label>
        <input type="month" id="month-select" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" value="${targetMonth}">
        <p class="text-xs text-slate-500">若留空，將匯入該分頁的所有月份資料。</p>
      </div>
    </div>
    <div class="p-4 border-t border-slate-100 flex justify-end gap-3 bg-white">
      <button id="cancel-setup-modal" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
      <button id="confirm-setup-modal" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">確認並同步</button>
    </div>
  `;
  
  overlay.appendChild(modal);
  document.body.appendChild(overlay);

  const closeModal = () => overlay.remove();
  document.getElementById('close-setup-modal').addEventListener('click', closeModal);
  document.getElementById('cancel-setup-modal').addEventListener('click', closeModal);

  document.getElementById('confirm-setup-modal').addEventListener('click', async () => {
    const checked = Array.from(modal.querySelectorAll('.sheet-checkbox:checked')).map(cb => cb.value);
    const selectedMonth = document.getElementById('month-select').value;
    
    if (checked.length === 0) {
      showToast('請至少選擇一個分頁', 'error');
      return;
    }

    targetFileName = fileName;
    selectedSheets = checked;
    targetMonth = selectedMonth;
    currentFile = buffer;
    
    localStorage.setItem('excel_sync_config', JSON.stringify({
      fileName: targetFileName,
      sheets: selectedSheets,
      month: targetMonth
    }));
    
    closeModal();
    
    currentState = { isBound: true, fileName: targetFileName, sheets: selectedSheets };
    notifyStateChange();
    
    showToast('設定成功，開始同步資料', 'success');
    await syncFromBuffer(buffer);
  });
}

export async function syncFromBuffer(buffer = currentFile) {
  if (!buffer || isSyncing) return;
  isSyncing = true;

  try {
    const workbook = XLSX.read(buffer, { type: 'array' });
    let salesMapping = [], statuses = AppState.getStatuses() || ['排產中'];
    let schema = [];
    let routeTemplates = [];
    
    try {
      const s = await getDocument('settings', 'main');
      if (s) { 
        salesMapping = s.sales_mapping || []; 
        statuses = s.statuses || statuses; 
        schema = s.schema || []; 
        routeTemplates = s.route_templates || [];
      }
    } catch (err) {
      console.warn('[Sync Engine] 無法取得 settings', err);
    }

    const parsed = parseWorkbook(workbook, selectedSheets, salesMapping, statuses[0], schema, targetMonth);
    let added = 0, updated = 0;

    for (const order of parsed) {
      if (!order.id) continue;
      
      const existing = await getDocument('work_orders', order.id);
      if (existing) {
        const hasChanged = JSON.stringify(existing.data) !== JSON.stringify(order.data) ||
                           existing.status !== order.status ||
                           JSON.stringify(existing.route) !== JSON.stringify(order.route) ||
                           existing.area !== order.area;
                           
        if (hasChanged) {
          await updateDocument('work_orders', order.id, { 
            status: order.status,
            route: order.route,
            area: order.area,
            salesReps: order.salesReps, 
            uploadDate: order.uploadDate,
            data: order.data 
          });
          updated++;
        }
      } else {
        const orderCategory = String(order.data?.category || '其他').toUpperCase().trim();
        let matchedTemplate = routeTemplates.find(t => {
          if (t.categories && Array.isArray(t.categories) && t.categories.length > 0) {
            return t.categories.map(c => String(c).toUpperCase().trim()).includes(orderCategory);
          }
          if (typeof t.category === 'string' && t.category.trim() !== '') {
            return t.category.toUpperCase().trim() === orderCategory;
          }
          return false;
        });
        
        if (!matchedTemplate) {
          matchedTemplate = routeTemplates.find(t => (!t.categories || t.categories.length === 0) && (!t.category || t.category.trim() === ''));
        }

        if (!order.route || order.route.length === 0) {
          if (matchedTemplate && matchedTemplate.steps && matchedTemplate.steps.length > 0) {
            order.route = matchedTemplate.steps.slice();
            order.status = matchedTemplate.steps[0];
          } else {
            order.route = statuses.slice();
            order.status = statuses[0];
          }
        }

        if (!order.source) order.source = 'excel';

        await setDocument('work_orders', order.id, order);
        added++;
      }
    }

    if (added > 0 || updated > 0) {
      showToast(`同步完成：新增 ${added}，更新 ${updated}`, 'success');
    } else {
      showToast('同步完成，但沒有新增或更新資料（請檢查分頁或月份）', 'info');
    }
  } catch (err) {
    console.error('[Sync From Excel Error]', err);
    showToast(`讀取 Excel 失敗: ${err.message}`, 'error');
  } finally {
    isSyncing = false;
  }
}

export async function downloadLatestExcel() {
  if (!currentFile) {
    showToast('請先上傳 Excel 檔案', 'error');
    return;
  }

  try {
    const workbook = XLSX.read(currentFile, { type: 'array' });
    const allOrders = await getCollection('work_orders');

    const updatedWorkbook = updateWorkbookState(workbook, selectedSheets, allOrders);

    const downloadName = targetFileName.replace(/\.(xlsx|xls)$/i, '_已同步.xlsx');
    downloadWorkbook(updatedWorkbook, downloadName);
    
    showToast('已下載最新進度，請覆蓋原檔', 'success');
  } catch (err) {
    console.error('[Download Error]', err);
    showToast(`下載失敗: ${err.message}`, 'error');
  }
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/engine/sync-engine.js
V: 1794188100
W: UPGRADE-009
═══ */```

<!-- === FILE END: ./src/engine/sync-engine.js === -->

<!-- === FILE START: ./src/lib/io.js === -->

## 📄 ./src/lib/io.js

```js
import * as XLSX from 'xlsx';
import { jsPDF } from 'jspdf';
import { groupByStatus } from '../domain/stats.js';

/**
 * 將工單資料匯出為 Excel 檔案
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} statuses 狀態清單
 * @param {string} filename 檔案名稱 (不含副檔名)
 * @param {Array} schema 系統欄位設定
 * @param {Array<string>} exportConfig 自訂匯出欄位 (若為 null 或空，則使用預設)
 */
export function exportToExcel(orders, statuses, filename = '工單清單', schema = [], exportConfig = null) {
  if (!orders || orders.length === 0) {
    throw new Error('無資料可匯出');
  }

  const wb = XLSX.utils.book_new();
  const groupedData = groupByStatus(orders, statuses);

  // 輔助函數：將 route 陣列轉為字串
  const formatRoute = (routeArr) => {
    if (Array.isArray(routeArr) && routeArr.length > 0) {
      return routeArr.join(' → ');
    }
    return '';
  };

  // 輔助函數：取得所有資料庫中出現的動態欄位 Key (用於「全部工單」)
  const getAllDataKeys = () => {
    const keys = new Set();
    orders.forEach(o => {
      if (o.data) {
        Object.keys(o.data).forEach(k => keys.add(k));
      }
    });
    return Array.from(keys);
  };
  const allDataKeys = getAllDataKeys();

  // ── 1. 建立「全部工單」Sheet (全欄位) ──
  const allDataToExport = orders.map(order => {
    const row = {
      '工單號碼': order.id,
      '狀態': order.status,
      '製程路線': formatRoute(order.route),
      '銷售員': (order.salesReps || []).join(', '),
      '開單日期': order.uploadDate || '',
      '交貨日期': order.dueDate || '',
      '狀態更新日': order.statusUpdateDate || '',
      '來源': order.source === 'manual' ? '手動' : 'Excel',
      '核對狀態': order.diff ? '有差異' : '正常'
    };

    // 加入 Schema 定義的欄位 (包含關聯欄位)
    schema.forEach(field => {
      if (!field.isSystem) {
        row[field.label] = order.data && order.data[field.key] !== undefined ? order.data[field.key] : '';
      }
    });

    // 加入 Schema 未定義但在資料中存在的欄位
    allDataKeys.forEach(key => {
      const schemaField = schema.find(f => f.key === key);
      if (!schemaField && key !== 'category') {
        row[key] = order.data && order.data[key] !== undefined ? order.data[key] : '';
      }
    });

    // 向下相容舊資料的 supplierName (若有在根目錄)
    if (order.supplierName && !row['外發供應商']) {
      row['外發供應商'] = order.supplierName;
    }

    return row;
  });

  const wsAll = XLSX.utils.json_to_sheet(allDataToExport);
  XLSX.utils.book_append_sheet(wb, wsAll, '全部工單');

  // ── 2. 建立「分狀態」Sheets (依自訂欄位) ──
  // 決定要輸出的欄位 (若無設定則 Fallback 到預設)
  let targetFields = [];
  if (exportConfig && exportConfig.length > 0) {
    targetFields = exportConfig.map(key => schema.find(f => f.key === key)).filter(Boolean);
  } else {
    // 預設欄位
    targetFields = [
      { key: 'id', label: '工單號碼' },
      { key: 'status', label: '狀態' },
      { key: 'salesReps', label: '銷售員' },
      { key: 'uploadDate', label: '開單日期' },
      { key: 'dueDate', label: '交貨日期' }
    ];
    // 加入所有自訂欄位
    schema.filter(f => !f.isSystem).forEach(f => targetFields.push(f));
  }

  for (const status of statuses) {
    const statusOrders = groupedData[status] || [];
    if (statusOrders.length === 0) continue;

    const dataToExport = statusOrders.map(order => {
      const row = {};
      
      targetFields.forEach(f => {
        if (f.key === 'id') {
          row['工單號碼'] = order.id;
        } else if (f.key === 'status') {
          row['狀態'] = order.status;
        } else if (f.key === 'salesReps') {
          row['銷售員'] = (order.salesReps || []).join(', ');
        } else if (f.key === 'uploadDate') {
          row['開單日期'] = order.uploadDate || '';
        } else if (f.key === 'dueDate') {
          row['交貨日期'] = order.dueDate || '';
        } else if (f.key === 'route') {
          row['製程路線'] = formatRoute(order.route);
        } else if (f.isSystem) {
          row[f.label] = order[f.key] || '';
        } else {
          row[f.label] = order.data && order.data[f.key] !== undefined ? order.data[f.key] : '';
        }
      });

      return row;
    });

    const ws = XLSX.utils.json_to_sheet(dataToExport);
    XLSX.utils.book_append_sheet(wb, ws, status.substring(0, 31));
  }

  // 觸發下載
  XLSX.writeFile(wb, `${filename}_${new Date().toISOString().slice(0,10)}.xlsx`);
}

/**
 * 將工單資料匯出為 PDF 檔案 (依狀態分頁)
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} statuses 狀態清單
 * @param {string} filename 檔案名稱 (不含副檔名)
 * @param {Array} schema 系統欄位設定
 */
export function exportToPDF(orders, statuses, filename = '工單清單', schema = []) {
  if (!orders || orders.length === 0) {
    throw new Error('無資料可匯出');
  }

  const doc = new jsPDF();
  const groupedData = groupByStatus(orders, statuses);
  
  let isFirstPage = true;

  for (const status of statuses) {
    const statusOrders = groupedData[status] || [];
    if (statusOrders.length === 0) continue;

    if (!isFirstPage) {
      doc.addPage();
    }
    isFirstPage = false;

    // 標題
    doc.setFontSize(16);
    doc.text(`工序狀態：${status}`, 14, 20);
    doc.setFontSize(10);
    doc.setTextColor(100);

    let yPos = 30;
    const lineHeight = 10;
    const pageHeight = doc.internal.pageSize.height;

    // 表頭
    doc.text('工單號碼', 14, yPos);
    doc.text('類別', 50, yPos);
    doc.text('銷售員', 70, yPos);
    doc.text('交貨日', 120, yPos);
    
    yPos += 5;
    doc.setLineWidth(0.5);
    doc.line(14, yPos, 196, yPos);
    yPos += 5;

    // 內容
    doc.setTextColor(0);
    for (const order of statusOrders) {
      if (yPos > pageHeight - 20) {
        doc.addPage();
        yPos = 20;
        // 重新繪製表頭
        doc.text('工單號碼', 14, yPos);
        doc.text('類別', 50, yPos);
        doc.text('銷售員', 70, yPos);
        doc.text('交貨日', 120, yPos);
        yPos += 5;
        doc.line(14, yPos, 196, yPos);
        yPos += 5;
      }

      const orderId = String(order.id).substring(0, 20);
      const cat = String(order.data?.category || '其他').substring(0, 10);
      const salesRep = (order.salesReps || []).join(', ').substring(0, 25);
      const dueDate = String(order.dueDate || '-').substring(0, 15);

      doc.text(orderId, 14, yPos);
      doc.text(cat, 50, yPos);
      doc.text(salesRep, 70, yPos);
      doc.text(dueDate, 120, yPos);

      // 組合額外資訊（包含自訂欄位）
      let extraInfoArr = [];
      const customFields = schema.filter(f => !f.isSystem);
      
      customFields.forEach(f => {
        const val = order.data && order.data[f.key] !== undefined ? order.data[f.key] : '-';
        if (val) {
          extraInfoArr.push(`${f.label}: ${val}`);
        }
      });

      if (extraInfoArr.length > 0) {
        yPos += 5;
        const extraInfo = extraInfoArr.join(' | ');
        doc.setFontSize(8);
        doc.setTextColor(100);
        // 限制字數避免超出邊界
        doc.text(extraInfo.substring(0, 120), 14, yPos);
        doc.setFontSize(10);
        doc.setTextColor(0);
        yPos += 5; // 多佔用一行
      }

      yPos += lineHeight;
    }
  }

  // 觸發下載
  doc.save(`${filename}_${new Date().toISOString().slice(0,10)}.pdf`);
}

/*
F: AI-WA-UPWO/src/lib/io.js
V: 1793086000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/lib/io.js === -->

<!-- === FILE START: ./src/pages/admin-data.js === -->

## 📄 ./src/pages/admin-data.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument, getCollection } from '../db/read.js';
import { deleteDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { renderOrderView } from '../ui/order-view.js';
import { createIcons, Database, Trash2 } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let schema = [];
let viewConfig = {};

export default {
  async render() {
    const user = AppState.getUser();
    if (!user || user.role !== 'admin') {
      document.getElementById('app-content').innerHTML = '<div class="p-8 text-center text-rose-500 font-bold">您沒有權限訪問此頁面</div>';
      return;
    }

    try {
      const settingsDoc = await getDocument('settings', 'main');
      if (settingsDoc) {
        schema = settingsDoc.schema || [];
        viewConfig = settingsDoc.view_configs || {};
      }
    } catch (err) {}

    renderPageLayout({
      title: '資料總管',
      subtitle: '最高權限資料檢視與清理',
      headerActions: `
        <button id="btn-clear-all" class="bg-rose-500 hover:bg-rose-600 text-white px-4 py-2 rounded-[10px] text-sm font-medium transition-colors shadow-sm flex items-center gap-2">
          <i data-lucide="trash-2" class="w-4 h-4"></i> 一鍵清空資料庫
        </button>
      `,
      stats: [],
      controls: '',
      listContainerId: 'admin-data-container'
    });

    document.getElementById('btn-clear-all')?.addEventListener('click', async () => {
      const confirmed = await openConfirm({
        title: '清空所有資料',
        message: '這將會永久刪除資料庫中的「所有」工單，包含所有狀態與自訂欄位，且無法復原。確定要繼續嗎？',
        isDanger: true,
        confirmText: '確定清空'
      });

      if (!confirmed) return;

      showToast('正在清空所有資料...', 'info');
      try {
        const orders = await getCollection('work_orders');
        let count = 0;
        for (const order of orders) {
          await deleteDocument('work_orders', order.id);
          count++;
        }
        showToast(`成功清空 ${count} 筆工單資料`, 'success');
      } catch (error) {
        console.error('Clear all data error:', error);
        showToast('清空失敗，請查看 Console', 'error');
      }
    });

    createIcons({ icons: { Database, Trash2 }, attrs: { class: 'w-4 h-4' } });

    window.addEventListener('refresh-admin-data', this.updateUI.bind(this));
    this.initDataListener();
  },

  initDataListener() {
    if (unsubscribe) unsubscribe();
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.updateUI();
    });
  },

  updateUI() {
    renderOrderView({
      containerId: 'admin-data-container',
      orders: currentOrders,
      visibleFields: schema.map(f => ({ ...f, isVisible: true })), // 管理員看全部欄位
      isAdmin: true,
      pageKey: 'admin-data',
      onAction: async (action, id) => {
        if (action === 'delete') {
          const confirmed = await openConfirm({
            title: '刪除工單',
            message: `確定要徹底刪除工單「${id}」嗎？此操作無法復原。`,
            isDanger: true,
            confirmText: '確認刪除'
          });
          if (confirmed) {
            try {
              await deleteDocument('work_orders', id);
              showToast(`工單 ${id} 已刪除`, 'success');
            } catch (err) {
              showToast('刪除失敗', 'error');
            }
          }
        } else if (action === 'status_change') {
          // 管理員頁面也允許直接改狀態
          const newStatus = window.prompt('請輸入目標狀態：');
          if (newStatus && newStatus.trim()) {
            const { updateDocument } = await import('../db/write.js');
            await updateDocument('work_orders', id, { status: newStatus.trim(), statusUpdateDate: new Date().toISOString() });
            showToast('已更新生產進度', 'success');
          }
        }
      }
    });
  },

  destroy() {
    if (unsubscribe) unsubscribe();
    window.removeEventListener('refresh-admin-data', this.updateUI.bind(this));
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/admin-data.js
V: 1792917700
W: UPGRADE-003
═══ */```

<!-- === FILE END: ./src/pages/admin-data.js === -->

<!-- === FILE START: ./src/pages/area.js === -->

## 📄 ./src/pages/area.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument } from '../db/read.js';
import { setDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { createIcons, Inbox, ChevronLeft, ChevronRight, MapPin, AlertCircle, Activity } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let areas = [];
let statuses = [];
let areaStatuses = [];
let userRole = 'viewer';
let userAllowedAreas = [];

/**
 * 渲染單一工單 chip（工單號 + 上/下工序按鈕）
 * 按鈕事件由 order-view-editor.js 的全域委派處理
 */
function renderOrderChip(order, allStatuses) {
  let route = order.route && order.route.length > 0 ? order.route : allStatuses;
  let idx = route.indexOf(order.status);
  if (idx === -1) idx = 0;
  const isFirst = idx <= 0;
  const isLast = idx >= route.length - 1;
  const routeStr = JSON.stringify(route).replace(/"/g, '&quot;');

  return `
    <div class="flex items-center gap-1 bg-white border border-slate-200 rounded-lg px-2 py-1 shadow-sm">
      <span class="text-sm font-medium text-slate-700">${order.id}</span>
      <button data-action="prev-step" data-id="${order.id}" data-route="${routeStr}" data-status="${order.status}"
        class="p-0.5 rounded text-slate-400 hover:bg-slate-100 transition-colors ${isFirst ? 'opacity-30 cursor-not-allowed' : ''}" ${isFirst ? 'disabled' : ''}>
        <i data-lucide="chevron-left" class="w-3 h-3"></i>
      </button>
      <button data-action="next-step" data-id="${order.id}" data-route="${routeStr}" data-status="${order.status}"
        class="p-0.5 rounded text-slate-400 hover:bg-slate-100 transition-colors ${isLast ? 'opacity-30 cursor-not-allowed' : ''}" ${isLast ? 'disabled' : ''}>
        <i data-lucide="chevron-right" class="w-3 h-3"></i>
      </button>
    </div>
  `;
}

/**
 * 渲染一組工序區塊（工序小標題 + 工單 chips）
 */
function renderStatusBlock(status, orders, allStatuses) {
  if (!orders || orders.length === 0) return '';
  return `
    <div>
      <div class="text-xs font-semibold text-slate-500 mb-1.5">${status}（${orders.length}）</div>
      <div class="flex flex-wrap gap-1.5">
        ${orders.map(o => renderOrderChip(o, allStatuses)).join('')}
      </div>
    </div>
  `;
}

/**
 * 取得工單類別
 */
function getOrderCategory(order) {
  if (order.data && order.data.category) {
    return String(order.data.category).toUpperCase();
  }
  const match = String(order.id).match(/^[a-zA-Z]+/);
  return match ? match[0].toUpperCase() : '其他';
}

export default {
  async render() {
    const user = AppState.getUser();
    if (!user) return;

    userRole = user.role;
    userAllowedAreas = Array.isArray(user.allowedAreas) ? user.allowedAreas : [];

    await this.loadSettings();

    renderPageLayout({
      title: '生產區域',
      subtitle: '依區域檢視工單狀態',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'area-container'
    });

    // 全域事件委派：處理區域狀態切換
    document.addEventListener('change', async (e) => {
      const select = e.target.closest('.area-status-select');
      if (select) {
        const areaId = select.dataset.areaId;
        const newStatus = select.value;
        
        // 權限檢查
        if (userRole !== 'admin' && userRole !== 'worker') return;

        try {
          const settingsDoc = await getDocument('settings', 'main');
          if (!settingsDoc) return;
          
          const targetAreaIndex = (settingsDoc.areas || []).findIndex(a => a.id === areaId);
          if (targetAreaIndex === -1) return;

          // 生產員檢查是否在允許的狀態清單內
          if (userRole === 'worker') {
            const allowed = settingsDoc.areas[targetAreaIndex].allowedStatuses || [];
            if (!allowed.includes(newStatus)) {
              showToast('您沒有權限切換至該狀態', 'error');
              return;
            }
          }

          settingsDoc.areas[targetAreaIndex].currentStatus = newStatus;
          await setDocument('settings', 'main', settingsDoc);
          
          showToast('區域狀態已更新', 'success');
        } catch (err) {
          console.error(err);
          showToast('更新失敗', 'error');
        }
      }
    });

    window.addEventListener('settings-updated', async () => {
      await this.loadSettings();
      this.updateUI();
    });
    window.addEventListener('refresh-area', this.updateUI.bind(this));

    this.initDataListener();
  },

  async loadSettings() {
    try {
      const settingsDoc = await getDocument('settings', 'main');
      if (settingsDoc) {
        areas = settingsDoc.areas || [];
        statuses = settingsDoc.statuses || [];
        areaStatuses = settingsDoc.areaStatuses || ['正常', '壞機', '休息中'];
      }
    } catch (err) {}
  },

  initDataListener() {
    if (unsubscribe) unsubscribe();
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.updateUI();
    });
  },

  updateUI() {
    const container = document.getElementById('area-container');
    if (!container) return;

    const allStatuses = AppState.getStatuses() || statuses;

    // 1. 過濾垃圾資料
    const cleanOrders = currentOrders.filter(o => {
      const id = String(o.id || '');
      return id && !id.includes('***') && !id.includes('訂單');
    });

    // 2. 建立「全域」工序 → 區域映射
    const statusToAreas = {};
    areas.forEach(area => {
      (area.statuses || []).forEach(s => {
        if (!statusToAreas[s]) statusToAreas[s] = [];
        statusToAreas[s].push(area);
      });
    });

    // 3. 權限過濾：決定「可見區域」
    let visibleAreas = areas;
    let showAllWarning = false;
    if (userRole === 'worker') {
      if (userAllowedAreas.length > 0) {
        visibleAreas = areas.filter(a => userAllowedAreas.includes(a.id));
      } else {
        // 生產員且未設定授權，提示管理員
        showAllWarning = true;
      }
    } else if (userRole === 'viewer') {
      // 一般使用者看到全部，但沒有編輯權限（用 UI 隱藏處理）
    }
    const visibleAreaIds = new Set(visibleAreas.map(a => a.id));

    // 4. 真正未綁定的工序
    const unassignedStatuses = allStatuses.filter(s => !statusToAreas[s]);

    // 5. 收集「未知工序」
    const knownStatuses = new Set([...allStatuses, ...Object.keys(statusToAreas)]);
    const unknownStatuses = new Set();
    cleanOrders.forEach(o => {
      if (o.status && !knownStatuses.has(o.status)) {
        unknownStatuses.add(o.status);
      }
    });

    // 6. 初始化分組容器
    const ordersByArea = {};
    visibleAreas.forEach(area => {
      ordersByArea[area.id] = {};
      (area.statuses || []).forEach(s => {
        ordersByArea[area.id][s] = [];
      });
    });

    const unassignedOrders = {};
    unassignedStatuses.forEach(s => {
      unassignedOrders[s] = [];
    });

    const unknownOrders = {};

    // 7. 分派工單
    cleanOrders.forEach(order => {
      const status = order.status;
      const orderCategory = getOrderCategory(order);
      const matchedAreas = statusToAreas[status] || [];
      
      let isAssignedToAnyVisibleArea = false;

      matchedAreas.forEach(area => {
        if (!visibleAreaIds.has(area.id)) return;

        // 相容類別檢查
        let areaCats = [];
        if (Array.isArray(area.categories)) {
          areaCats = area.categories.map(c => String(c).toUpperCase());
        } else if (typeof area.category === 'string' && area.category.trim() !== '') {
          areaCats = area.category.split(',').map(c => c.trim().toUpperCase()).filter(Boolean);
        }

        const isCategoryMatch = areaCats.length === 0 || areaCats.includes(orderCategory);

        if (isCategoryMatch && ordersByArea[area.id] && ordersByArea[area.id][status]) {
          ordersByArea[area.id][status].push(order);
          isAssignedToAnyVisibleArea = true;
        }
      });

      // 若該工序完全沒有被綁定到任何區域（或未授權），則進入「未分配區域」
      if (!isAssignedToAnyVisibleArea) {
        if (status && unassignedOrders[status]) {
          unassignedOrders[status].push(order);
        } else {
          const key = status || '__EMPTY__';
          if (!unknownOrders[key]) unknownOrders[key] = [];
          unknownOrders[key].push(order);
        }
      }
    });

    // 8. 組裝 HTML
    let html = '';

    if (showAllWarning) {
      html += `
        <div class="bg-amber-50 border border-amber-200 rounded-xl p-3 mb-4 flex items-start gap-2">
          <i data-lucide="alert-circle" class="w-5 h-5 text-amber-500 flex-shrink-0 mt-0.5"></i>
          <p class="text-sm text-amber-700">尚未配置區域權限，目前可看到全部區域。請聯繫管理員於「帳號管理」配置可見區域。</p>
        </div>
      `;
    }

    if (userRole === 'worker' && userAllowedAreas.length > 0 && visibleAreas.length === 0) {
      html += `
        <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft">
          <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
          <span class="text-base font-medium">您沒有被授權查看任何區域</span>
        </div>
      `;
      container.innerHTML = html;
      createIcons({ icons: { Inbox, AlertCircle }, attrs: { class: 'w-4 h-4' } });
      return;
    }

    // 8-1. 各區域卡片
    const areaCardsHtml = visibleAreas.map(area => {
      const areaOrders = ordersByArea[area.id] || {};
      const totalCount = Object.values(areaOrders).reduce((sum, arr) => sum + arr.length, 0);

      const statusBlocks = (area.statuses || []).map(status =>
        renderStatusBlock(status, areaOrders[status] || [], allStatuses)
      ).join('');

      // 類別標籤
      const areaCats = Array.isArray(area.categories) ? area.categories : (area.category ? area.category.split(',') : []);
      const categoryLabel = areaCats.length > 0 
        ? areaCats.map(c => `<span class="text-[10px] px-2 py-0.5 bg-blue-50 text-blue-600 rounded font-bold">${c}</span>`).join('')
        : '';

      // 狀態 Tag / 下拉選單
      const allowedSts = area.allowedStatuses || [];
      let statusHtml = '';
      
      if (userRole === 'admin' || (userRole === 'worker' && allowedSts.length > 0)) {
        // 可編輯狀態
        const options = (userRole === 'admin' ? areaStatuses : allowedSts)
          .map(st => `<option value="${st}" ${area.currentStatus === st ? 'selected' : ''}>${st}</option>`).join('');
        
        statusHtml = `
          <select class="area-status-select appearance-none bg-slate-100 border border-slate-200 text-xs font-bold text-slate-600 px-2 py-0.5 rounded cursor-pointer hover:bg-slate-200 transition-colors focus:outline-none focus:ring-2 focus:ring-brand/20 ml-auto" data-area-id="${area.id}">
            ${options}
          </select>
        `;
      } else {
        // 唯讀顯示（一般使用者或無授權狀態的生產員）
        statusHtml = `<span class="text-xs font-bold text-slate-500 bg-slate-100 px-2 py-0.5 rounded ml-auto">${area.currentStatus || '正常'}</span>`;
      }

      return `
        <div class="glass-card p-4 flex flex-col gap-3">
          <div class="flex items-center gap-2 flex-wrap border-b border-slate-100 pb-2">
            <i data-lucide="map-pin" class="w-5 h-5 text-brand"></i>
            <h2 class="text-base font-bold text-slate-800">${area.name}</h2>
            ${categoryLabel}
            <span class="text-xs px-2 py-0.5 bg-brand/10 text-brand rounded font-medium">${totalCount} 單</span>
            ${statusHtml}
          </div>
          ${totalCount === 0 ? `<div class="text-sm text-slate-400 py-2">暫無工單</div>` : statusBlocks}
        </div>
      `;
    }).join('');

    // 8-2. 未分配區域
    const unassignedBlocks = unassignedStatuses.map(status =>
      renderStatusBlock(status, unassignedOrders[status] || [], allStatuses)
    ).join('');

    const unknownBlocks = Object.keys(unknownOrders).map(status => {
      const label = status === '__EMPTY__' ? '（無工序）' : status;
      return renderStatusBlock(label, unknownOrders[status], allStatuses);
    }).join('');

    const unassignedTotal = Object.values(unassignedOrders).reduce((sum, arr) => sum + arr.length, 0)
      + Object.values(unknownOrders).reduce((sum, arr) => sum + arr.length, 0);

    let unassignedHtml = '';
    if (unassignedTotal > 0) {
      unassignedHtml = `
        <div class="glass-card p-4 flex flex-col gap-3 border-dashed border-2 border-slate-200 shadow-none">
          <div class="flex items-center gap-2">
            <i data-lucide="inbox" class="w-5 h-5 text-slate-400"></i>
            <h2 class="text-base font-bold text-slate-600">未分配區域</h2>
            <span class="text-xs px-2 py-0.5 bg-slate-100 text-slate-500 rounded font-medium ml-auto">${unassignedTotal} 單</span>
          </div>
          ${unassignedBlocks}
          ${unknownBlocks}
        </div>
      `;
    }

    // 8-3. 空狀態
    if (!areaCardsHtml && !unassignedHtml) {
      html += `
        <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft">
          <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
          <span class="text-base font-medium">目前尚無區域或工單資料</span>
        </div>
      `;
    } else {
      html += `<div class="grid grid-cols-1 lg:grid-cols-2 xl:grid-cols-3 gap-4">${areaCardsHtml}${unassignedHtml}</div>`;
    }

    container.innerHTML = html;
    createIcons({
      icons: { Inbox, ChevronLeft, ChevronRight, MapPin, AlertCircle, Activity },
      attrs: { class: 'w-4 h-4' }
    });
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
    window.removeEventListener('settings-updated', this.updateUI.bind(this));
    window.removeEventListener('refresh-area', this.updateUI.bind(this));
  }
};

/*
F: AI-WA-UPWO/src/pages/area.js
V: 1792983000
W: UPGRADE-005
*/```

<!-- === FILE END: ./src/pages/area.js === -->

<!-- === FILE START: ./src/pages/category.js === -->

## 📄 ./src/pages/category.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument } from '../db/read.js';
import { updateDocument, deleteDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { renderOrderView } from '../ui/order-view.js';
import { createIcons } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let schema = [];
let viewConfig = {};
let currentCategory = '';

export default {
  async render() {
    if (!AppState.getUser()) return;

    try {
      const settingsDoc = await getDocument('settings', 'main');
      if (settingsDoc) {
        schema = settingsDoc.schema || [];
        viewConfig = settingsDoc.view_configs || {};
      }
    } catch (err) {}

    renderPageLayout({
      title: '類別視圖',
      subtitle: '依發票前兩碼檢視工單狀態',
      headerActions: '',
      controls: `
        <div id="category-tabs" class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-5 lg:grid-cols-6 xl:grid-cols-7 gap-2 mb-4"></div>
      `,
      listContainerId: 'category-container'
    });

    window.addEventListener('refresh-category', this.updateUI.bind(this));
    window.addEventListener('settings-updated', this.updateUI.bind(this));

    this.initDataListener();
  },

  initDataListener() {
    if (unsubscribe) unsubscribe();
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      if (!currentCategory) {
        const cats = Array.from(new Set(currentOrders.map(o => o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他'))).sort();
        if (cats.length > 0) currentCategory = cats[0];
      }
      this.updateUI();
    });
  },

  updateUI() {
    const cats = Array.from(new Set(currentOrders.map(o => o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他'))).sort();
    
    if (cats.length > 0 && !cats.includes(currentCategory)) {
      currentCategory = cats[0];
    }

    const tabsContainer = document.getElementById('category-tabs');
    if (tabsContainer) {
      tabsContainer.innerHTML = cats.map(c => `
        <button class="w-full flex justify-between items-center px-3 py-2 rounded-xl text-sm font-medium border ${c === currentCategory ? 'bg-brand text-white border-brand' : 'bg-white text-slate-600 border-slate-200'}" data-cat="${c}">
          <span class="truncate pr-2">${c}</span>
          <span class="text-xs px-1.5 py-0.5 rounded-md flex-shrink-0 ${c === currentCategory ? 'bg-white/20' : 'bg-slate-100'}">${currentOrders.filter(o => (o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他') === c).length}</span>
        </button>
      `).join('');

      tabsContainer.querySelectorAll('button').forEach(btn => {
        btn.onclick = () => { currentCategory = btn.dataset.cat; this.updateUI(); };
      });
    }

    if (!currentCategory) return;

    const filtered = currentOrders.filter(o => (o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他') === currentCategory);

    // 依使用者設定的順序對齊欄位
    const visibleFields = (viewConfig['category'] || []).map(key => {
      const field = schema.find(f => f.key === key);
      return field ? Object.assign({}, field, { isVisible: true }) : null;
    }).filter(Boolean);

    renderOrderView({
      containerId: 'category-container',
      orders: filtered,
      visibleFields: visibleFields,
      isAdmin: AppState.getUser()?.role === 'admin',
      pageKey: 'category',
      isReadOnly: true, // 設定為唯讀
      onAction: async (action, id, newStatus) => {
        if (action === 'delete') {
          if (await openConfirm({ title: '刪除', message: `確定刪除 ${id}？`, isDanger: true })) {
            await deleteDocument('work_orders', id);
            showToast('已刪除', 'success');
          }
        } else if (action === 'status_change') {
          if (newStatus) {
            await updateDocument('work_orders', id, { status: newStatus, statusUpdateDate: new Date().toISOString() });
            showToast('已更新生產進度', 'success');
          }
        }
      }
    });
  },

  destroy() {
    if (unsubscribe) unsubscribe();
    window.removeEventListener('refresh-category', this.updateUI.bind(this));
    window.removeEventListener('settings-updated', this.updateUI.bind(this));
  }
};

/*
F: AI-WA-UPWO/src/pages/category.js
V: 1792963000
W: UPGRADE-005
*/```

<!-- === FILE END: ./src/pages/category.js === -->

<!-- === FILE START: ./src/pages/data-entry.js === -->

## 📄 ./src/pages/data-entry.js

```js
import { AppState } from '../config/state.js';
import { getDocument, getCollection, listenCollection } from '../db/read.js';
import { setDocument, updateDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { renderOrderEditor } from '../ui/order-editor.js';
import { createIcons, Search, Plus, FileText, ArrowLeft, Layers } from 'lucide';

let currentSettings = null;
let isViewMode = false;
let currentOrder = null;
let sidebarOrders = [];
let unsubscribeOrders = null;

export default {
  async render() {
    const user = AppState.getUser();
    if (!user) return;

    // 1. 載入系統設定
    try {
      currentSettings = await getDocument('settings', 'main');
    } catch (err) {
      showToast('無法載入系統設定', 'error');
      return;
    }

    if (!currentSettings) {
      showToast('系統設定為空，請先初始化', 'error');
      return;
    }

    // 2. 初始化頁面佈局
    renderPageLayout({
      title: '資料錄入',
      subtitle: '手動建立新工單或載入現有工單進行編輯',
      headerActions: `
        <div class="flex items-center gap-3">
          <button id="btn-new-order" class="bg-brand text-white border border-brand px-4 py-2 rounded-[10px] text-sm font-medium hover:bg-brand-hover flex items-center gap-2 shadow-sm transition-colors">
            <i data-lucide="plus" class="w-4 h-4"></i> 新增工單
          </button>
        </div>
      `,
      controls: '',
      listContainerId: 'data-entry-container'
    });

    createIcons({ icons: { Plus, Search, FileText, ArrowLeft, Layers }, attrs: { class: 'w-4 h-4' } });

    // 3. 繪製佈局容器
    const container = document.getElementById('data-entry-container');
    const layoutMode = currentSettings.form_layout || 'B'; // 預設為 B (分頁)

    if (layoutMode === 'C') {
      // 模式 C：左右對照
      container.innerHTML = `
        <div class="flex flex-col md:flex-row gap-4 h-[calc(100vh-200px)]">
          <!-- 左側：工單清單 -->
          <div class="w-full md:w-1/3 bg-white border border-slate-200 rounded-xl shadow-sm flex flex-col overflow-hidden">
            <div class="p-3 border-b border-slate-100 bg-slate-50">
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none text-slate-400">
                  <i data-lucide="search" class="w-4 h-4"></i>
                </div>
                <input type="text" id="sidebar-search" class="w-full pl-9 pr-3 py-2 bg-white border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" placeholder="搜尋單號...">
              </div>
            </div>
            <div class="flex-1 overflow-y-auto p-2 flex flex-col gap-1" id="sidebar-list">
              <div class="text-center text-slate-400 text-sm py-4">載入中...</div>
            </div>
          </div>
          <!-- 右側：表單區 -->
          <div class="w-full md:w-2/3 h-full" id="editor-container"></div>
        </div>
      `;
      
      // 啟動左側清單監聽
      this.initSidebarList();
      document.getElementById('sidebar-search')?.addEventListener('input', (e) => {
        this.filterSidebar(e.target.value);
      });

    } else {
      // 模式 A 或 B：單一表單區
      container.innerHTML = `
        <div class="max-w-5xl mx-auto h-[calc(100vh-200px)]">
          <div id="editor-container" class="h-full"></div>
        </div>
      `;
    }

    // 4. 綁定新增按鈕
    document.getElementById('btn-new-order')?.addEventListener('click', () => {
      this.loadNewOrder();
    });

    // 5. 初始載入表單 (預設為新增)
    this.loadNewOrder();
  },

  /**
   * 初始化左側清單 (模式 C 專用)
   */
  initSidebarList() {
    if (unsubscribeOrders) unsubscribeOrders();
    unsubscribeOrders = listenCollection('work_orders', (orders) => {
      // 過濾垃圾資料
      sidebarOrders = orders.filter(o => o.id && !String(o.id).includes('***'));
      // 由新到舊排序
      sidebarOrders.sort((a, b) => new Date(b.uploadDate || 0) - new Date(a.uploadDate || 0));
      this.renderSidebarList(sidebarOrders);
    });
  },

  renderSidebarList(orders) {
    const listEl = document.getElementById('sidebar-list');
    if (!listEl) return;

    if (orders.length === 0) {
      listEl.innerHTML = '<div class="text-center text-slate-400 text-sm py-4">無資料</div>';
      return;
    }

    listEl.innerHTML = orders.map(o => `
      <button class="w-full text-left p-3 rounded-lg border ${currentOrder?.id === o.id ? 'bg-brand/5 border-brand/30' : 'bg-white border-transparent hover:bg-slate-50'} transition-colors flex flex-col gap-1" data-id="${o.id}">
        <div class="flex justify-between items-center">
          <span class="font-bold text-slate-800 text-sm">${o.id}</span>
          <span class="text-[10px] px-1.5 py-0.5 rounded bg-slate-100 text-slate-500">${o.source === 'manual' ? '手動' : 'Excel'}</span>
        </div>
        <div class="flex justify-between items-center text-xs text-slate-500">
          <span>${o.data?.customerNameZh || o.data?.customCategory || '無客戶名'}</span>
          <span>${o.status || '無狀態'}</span>
        </div>
      </button>
    `).join('');

    listEl.querySelectorAll('button').forEach(btn => {
      btn.addEventListener('click', async () => {
        const id = btn.dataset.id;
        await this.loadOrder(id);
      });
    });
  },

  filterSidebar(keyword) {
    const kw = String(keyword).toLowerCase().trim();
    if (!kw) {
      this.renderSidebarList(sidebarOrders);
      return;
    }
    const filtered = sidebarOrders.filter(o => String(o.id).toLowerCase().includes(kw));
    this.renderSidebarList(filtered);
  },

  /**
   * 載入新工單表單 (清空)
   */
  loadNewOrder() {
    currentOrder = null;
    isViewMode = false;
    
    // 高亮左側清單 (若有)
    if (currentSettings.form_layout === 'C') {
      this.renderSidebarList(sidebarOrders); 
    }

    const container = document.getElementById('editor-container');
    if (!container) return;

    renderOrderEditor({
      container: container,
      schema: currentSettings.schema || [],
      formulas: currentSettings.formulas || {},
      viewConfig: currentSettings.view_configs?.['data-entry'] || [],
      routeTemplates: currentSettings.route_templates || [], // 💡 傳入製程模板
      layoutMode: currentSettings.form_layout || 'B',
      initialData: null,
      isViewMode: false,
      onSave: this.handleSave.bind(this),
      onCancel: () => {
        if (currentSettings.form_layout === 'C') {
          // 若在 C 模式，取消就清空右側
          this.loadNewOrder();
        } else {
          // 若在其他模式，取消就跳轉回總表或回首頁
          window.location.hash = '#kanban';
        }
      }
    });
  },

  /**
   * 載入指定工單
   */
  async loadOrder(id) {
    try {
      const orderDoc = await getDocument('work_orders', id);
      if (!orderDoc) {
        showToast('找不到該工單', 'error');
        return;
      }

      currentOrder = orderDoc;
      isViewMode = true;

      if (currentSettings.form_layout === 'C') {
        this.renderSidebarList(sidebarOrders);
      }

      this.renderEditor();
      showToast('已載入工單資料', 'success');
    } catch (err) {
      console.error(err);
      showToast('載入失敗', 'error');
    }
  },

  /**
   * 渲染表單 (依據當前 currentOrder 與 isViewMode)
   */
  renderEditor() {
    const container = document.getElementById('editor-container');
    if (!container) return;

    renderOrderEditor({
      container: container,
      schema: currentSettings.schema || [],
      formulas: currentSettings.formulas || {},
      viewConfig: currentSettings.view_configs?.['data-entry'] || [],
      routeTemplates: currentSettings.route_templates || [], // 💡 傳入製程模板
      layoutMode: currentSettings.form_layout || 'B',
      initialData: currentOrder,
      isViewMode: isViewMode,
      onSave: this.handleSave.bind(this),
      onCancel: () => {
        if (isViewMode) {
          // 若在檢視模式下取消，就清空表單
          this.loadNewOrder();
        } else {
          // 若在編輯模式下取消，就恢復為檢視模式
          isViewMode = true;
          this.renderEditor();
        }
      }
    });
  },

  /**
   * 處理儲存邏輯
   */
  async handleSave(formData) {
    // 1. 基本驗證
    if (!formData.id || String(formData.id).trim() === '') {
      showToast('工單號碼為必填欄位', 'error');
      return;
    }
    
    const orderId = String(formData.id).trim();
    formData.id = orderId; // 確保乾淨

    try {
      // 2. 檢查是否為新增 (若 currentOrder 為 null 或 id 不同)
      if (!currentOrder || currentOrder.id !== orderId) {
        // 進一步檢查資料庫是否已存在該 ID (防重複)
        const existing = await getDocument('work_orders', orderId);
        if (existing) {
          showToast(`工單 ${orderId} 已存在，已切換為編輯模式`, 'info');
          // 載入該工單資料，並切換為編輯模式
          currentOrder = existing;
          isViewMode = false;
          this.renderEditor();
          return;
        }

        // 確認為全新工單
        formData.source = 'manual';
        
        // 💡 若未設定路線與狀態，則給予預設值
        if (!formData.route || formData.route.length === 0) {
          formData.route = currentSettings.statuses ? [...currentSettings.statuses] : [];
          formData.status = currentSettings.statuses?.[0] || '排產中';
        }
        
        formData.uploadDate = formData.uploadDate || new Date().toISOString().split('T')[0];
        formData.statusUpdateDate = formData.uploadDate;

        await setDocument('work_orders', orderId, formData);
        showToast('工單新增成功', 'success');
      } else {
        // 3. 更新現有工單
        // 為了避免覆蓋非顯示欄位，這裡直接使用 formData (它包含了所有初始值與表單修改值)
        await updateDocument('work_orders', orderId, formData);
        showToast('工單更新成功', 'success');
      }

      // 4. 儲存後行為
      // 重新拉取最新資料以確保同步
      currentOrder = await getDocument('work_orders', orderId);
      isViewMode = true; // 切換為檢視模式
      
      if (currentSettings.form_layout === 'C') {
        // 更新左側清單的高亮
        this.renderSidebarList(sidebarOrders);
      }
      
      this.renderEditor();

    } catch (err) {
      console.error('[Save Error]', err);
      showToast('儲存失敗，請查看 Console', 'error');
    }
  },

  destroy() {
    if (unsubscribeOrders) {
      unsubscribeOrders();
      unsubscribeOrders = null;
    }
  }
};

/*
F: AI-WA-UPWO/src/pages/data-entry.js
V: 1793026000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/data-entry.js === -->

<!-- === FILE START: ./src/pages/kanban.js === -->

## 📄 ./src/pages/kanban.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument } from '../db/read.js';
import { updateDocument, setDocument, deleteDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { renderOrderView } from '../ui/order-view.js';
import { parseExcelData } from '../domain/parser.js';
import { checkDiff } from '../domain/data-diff.js';
import * as XLSX from 'xlsx';
import { createIcons } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let currentCategory = 'ALL';
let schema = [];
let viewConfig = {};
let route_templates = [];
let settingsCategories = [];
let diffFields = [];

export default {
  async render() {
    if (!AppState.getUser()) return;

    try {
      const settingsDoc = await getDocument('settings', 'main');
      if (settingsDoc) {
        schema = settingsDoc.schema || [];
        viewConfig = settingsDoc.view_configs || {};
        route_templates = settingsDoc.route_templates || [];
        settingsCategories = settingsDoc.categories || [];
        diffFields = settingsDoc.diff_fields || [];
      }
    } catch (err) {}

    renderPageLayout({
      title: '生產進度控制台',
      subtitle: '即時追蹤工單狀態與生產進度',
      headerActions: '', 
      controls: `
        <div id="category-tabs" class="flex flex-wrap gap-2 mb-4"></div>
      `,
      listContainerId: 'kanban-container'
    });

    window.addEventListener('refresh-kanban', this.updateUI.bind(this));
    window.addEventListener('settings-updated', async () => {
      try {
        const settingsDoc = await getDocument('settings', 'main');
        if (settingsDoc) {
          schema = settingsDoc.schema || [];
          viewConfig = settingsDoc.view_configs || {};
          route_templates = settingsDoc.route_templates || [];
          settingsCategories = settingsDoc.categories || [];
          diffFields = settingsDoc.diff_fields || [];
          this.updateUI();
        }
      } catch (err) {}
    });
    
    this.initDataListener();
  },

  initDataListener() {
    if (unsubscribe) unsubscribe();
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.updateUI();
    });
  },

  updateUI() {
    const orderCats = Array.from(new Set(currentOrders.map(o => o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他')));
    const combinedCats = settingsCategories.concat(orderCats);
    const allCats = Array.from(new Set(combinedCats)).filter(c => c !== 'ALL').sort();

    const tabsContainer = document.getElementById('category-tabs');
    if (tabsContainer) {
      const tabs = ['ALL'].concat(allCats);
      tabsContainer.innerHTML = tabs.map(c => {
        const label = c === 'ALL' ? '全部類別' : c;
        const count = c === 'ALL' 
          ? currentOrders.length 
          : currentOrders.filter(o => (o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他') === c).length;
        
        return `
          <button class="flex items-center gap-2 px-4 py-2 rounded-xl text-sm font-medium border ${c === currentCategory ? 'bg-brand text-white border-brand' : 'bg-white text-slate-600 border-slate-200'}" data-cat="${c}">
            <span>${label}</span>
            <span class="text-xs px-1.5 py-0.5 rounded-md ${c === currentCategory ? 'bg-white/20' : 'bg-slate-100'}">${count}</span>
          </button>
        `;
      }).join('');

      tabsContainer.querySelectorAll('button').forEach(btn => {
        btn.onclick = () => { currentCategory = btn.dataset.cat; this.updateUI(); };
      });
    }

    let filtered = currentOrders;
    if (currentCategory !== 'ALL') {
      filtered = filtered.filter(o => (o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他') === currentCategory);
    }

    const visibleFields = (viewConfig['kanban'] || []).map(key => {
      const field = schema.find(f => f.key === key);
      return field ? Object.assign({}, field, { isVisible: true }) : null;
    }).filter(Boolean);

    renderOrderView({
      containerId: 'kanban-container',
      orders: filtered,
      visibleFields: visibleFields,
      isAdmin: AppState.getUser()?.role === 'admin',
      pageKey: 'kanban',
      route_templates: route_templates,
      groupBy: 'status', // 確保看板保留狀態上下步功能
      onAction: async (action, id, newStatus) => {
        if (action === 'delete') {
          if (await openConfirm({ title: '刪除', message: `確定刪除 ${id}？`, isDanger: true })) {
            await deleteDocument('work_orders', id);
            showToast('已刪除', 'success');
          }
        } else if (action === 'status_change') {
          if (newStatus) {
            await updateDocument('work_orders', id, { status: newStatus, statusUpdateDate: new Date().toISOString() });
            showToast('已更新生產進度', 'success');
          }
        }
      }
    });
  },

  async processExcelFile(file, isAuto = false) {
    return new Promise((resolve) => {
      const reader = new FileReader();
      reader.onload = async (evt) => {
        try {
          const workbook = XLSX.read(new Uint8Array(evt.target.result), { type: 'array' });
          const jsonData = XLSX.utils.sheet_to_json(workbook.Sheets[workbook.SheetNames[0]], { header: 1 });
          
          if (jsonData.length === 0) {
            if (!isAuto) showToast('無資料', 'error');
            resolve();
            return;
          }

          if (!isAuto) showToast('解析中...', 'info');
          
          let salesMapping = [], statuses = AppState.getStatuses() || ['排產中'];
          let routeTemplates = [];
          let latestSettings = null;
          
          try {
            latestSettings = await getDocument('settings', 'main');
            if (latestSettings) { 
              salesMapping = latestSettings.sales_mapping || []; 
              statuses = latestSettings.statuses || statuses; 
              schema = latestSettings.schema || schema; 
              routeTemplates = latestSettings.route_templates || [];
              diffFields = latestSettings.diff_fields || [];
            }
          } catch (err) {}

          const parsed = parseExcelData(jsonData[0], jsonData.slice(1), salesMapping, statuses[0], schema);
          let added = 0, updated = 0, diffCount = 0;
          let newCategoriesAdded = false;
          let currentCategories = latestSettings && Array.isArray(latestSettings.categories) ? [...latestSettings.categories] : [];

          for (const order of parsed) {
            // 💡 自動同步類別：若發現新類別，則加入陣列
            const cat = String(order.data.category || '其他').toUpperCase().trim();
            if (cat && cat !== '其他' && !currentCategories.includes(cat)) {
              currentCategories.push(cat);
              newCategoriesAdded = true;
            }

            const existing = await getDocument('work_orders', order.id);
            if (existing) {
              if (existing.source === 'manual') {
                const diffResult = checkDiff(existing, order, diffFields);
                if (diffResult) {
                  await updateDocument('work_orders', order.id, { diff: diffResult });
                  diffCount++;
                }
              } else {
                // 防覆寫：不更新 status, route, dueDate (保留手動修改)，但更新 uploadDate (以 Excel 為準)
                await updateDocument('work_orders', order.id, { 
                  salesReps: order.salesReps, 
                  uploadDate: order.uploadDate,
                  data: order.data 
                });
                updated++;
              }
            } else {
              const orderCategory = cat;
              let matchedTemplate = routeTemplates.find(t => {
                // 強化比對邏輯，去除空白並轉大寫
                if (t.categories && Array.isArray(t.categories) && t.categories.length > 0) {
                  return t.categories.map(c => String(c).toUpperCase().trim()).includes(orderCategory);
                }
                if (typeof t.category === 'string' && t.category.trim() !== '') {
                  return t.category.toUpperCase().trim() === orderCategory;
                }
                return false;
              });
              
              if (!matchedTemplate) {
                matchedTemplate = routeTemplates.find(t => (!t.categories || t.categories.length === 0) && (!t.category || t.category.trim() === ''));
              }

              if (!order.route || order.route.length === 0) {
                if (matchedTemplate && matchedTemplate.steps && matchedTemplate.steps.length > 0) {
                  order.route = matchedTemplate.steps.slice();
                  order.status = matchedTemplate.steps[0];
                } else {
                  order.route = statuses.slice();
                  order.status = statuses[0];
                }
              }

              if (!order.source) order.source = 'excel';

              await setDocument('work_orders', order.id, order);
              added++;
            }
          }
          
          // 💡 儲存新增的類別 (使用 updateDocument 避免覆蓋其他設定)
          if (newCategoriesAdded) {
            currentCategories.sort();
            await updateDocument('settings', 'main', { categories: currentCategories });
            settingsCategories = currentCategories;
            showToast('已自動新增未知的工單類別', 'info');
          }

          let msg = isAuto ? '自動同步完成！' : '匯入完成！';
          if (added > 0 || updated > 0 || diffCount > 0) {
            msg += ` 新增 ${added}，更新 ${updated}`;
            if (diffCount > 0) msg += `，比對不符 ${diffCount}`;
            showToast(msg, 'success');
          }
          
          resolve();
          
        } catch (error) {
          console.error('[Import Error]', error);
          if (!isAuto) showToast(`失敗: ${error.message}`, 'error');
          resolve();
        }
      };
      reader.readAsArrayBuffer(file);
    });
  },

  destroy() {
    if (unsubscribe) unsubscribe();
    window.removeEventListener('refresh-kanban', this.updateUI.bind(this));
    window.removeEventListener('settings-updated', this.updateUI.bind(this));
  }
};

/*
F: AI-WA-UPWO/src/pages/kanban.js
V: 1793071000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/kanban.js === -->

<!-- === FILE START: ./src/pages/login.js === -->

## 📄 ./src/pages/login.js

```js
import { AppState } from '../config/state.js';
import { getCollection } from '../db/read.js';
import { showToast } from '../ui/toast.js';
import { createIcons, Layers, Lock, User } from 'lucide';

export default {
  async render() {
    // 登入頁不經過 App Shell，因此直接渲染到最外層的 app-root
    const content = document.getElementById('app-root');
    if (!content) return;

    content.innerHTML = `
      <div class="min-h-screen flex items-center justify-center bg-bg">
        <div class="glass-card w-full max-w-md p-8 sm:p-10 relative overflow-hidden">
          
          <!-- 裝飾背景 -->
          <div class="absolute top-0 left-0 w-full h-2 bg-gradient-to-r from-brand to-purple-500"></div>

          <div class="flex flex-col items-center mb-8">
            <div class="w-12 h-12 rounded-2xl bg-brand flex items-center justify-center text-white shadow-lg shadow-brand/30 mb-4">
              <i data-lucide="layers" class="w-7 h-7"></i>
            </div>
            <h1 class="text-2xl font-bold text-slate-900">UPGRADE-WO</h1>
            <p class="text-sm text-slate-500 mt-1">流水線進度控制台</p>
          </div>

          <form id="login-form" class="flex flex-col gap-5">
            <div class="flex flex-col gap-1.5">
              <label class="text-sm font-medium text-slate-700 ml-1">帳號</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3.5 flex items-center pointer-events-none text-slate-400">
                  <i data-lucide="user" class="w-5 h-5"></i>
                </div>
                <input type="text" id="username" required
                  class="w-full pl-11 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand/20 focus:border-brand transition-all"
                  placeholder="請輸入帳號">
              </div>
            </div>

            <div class="flex flex-col gap-1.5">
              <label class="text-sm font-medium text-slate-700 ml-1">密碼</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3.5 flex items-center pointer-events-none text-slate-400">
                  <i data-lucide="lock" class="w-5 h-5"></i>
                </div>
                <input type="password" id="password" required
                  class="w-full pl-11 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand/20 focus:border-brand transition-all"
                  placeholder="請輸入密碼">
              </div>
            </div>

            <button type="submit" id="login-btn" class="btn-primary w-full py-3 mt-2 flex justify-center items-center gap-2">
              <span>登入系統</span>
            </button>
          </form>
        </div>
      </div>
    `;

    createIcons({
      icons: { Layers, User, Lock },
      attrs: { class: 'w-5 h-5' }
    });

    this.bindEvents();
  },

  bindEvents() {
    const form = document.getElementById('login-form');
    const btn = document.getElementById('login-btn');

    form.addEventListener('submit', async (e) => {
      e.preventDefault();
      
      const usernameInput = document.getElementById('username').value.trim();
      const passwordInput = document.getElementById('password').value;
      
      if (!usernameInput || !passwordInput) {
        showToast('請填寫帳號與密碼', 'error');
        return;
      }

      // 變更按鈕狀態
      const originalBtnHtml = btn.innerHTML;
      btn.disabled = true;
      btn.innerHTML = `<span class="animate-pulse">驗證中...</span>`;

      try {
        // 從 settings 集合獲取使用者清單
        let usersList = [];
        try {
          const settingsData = await getCollection('settings');
          const userSetting = settingsData.find(s => s.id === 'users' || (s.users && Array.isArray(s.users)));
          if (userSetting) {
            usersList = userSetting.users || [];
          }
        } catch (err) {
          console.warn('無法讀取 settings，將使用預設帳密', err);
        }

        // 若資料庫完全沒有資料（全新專案），提供預設的 admin 帳號作為 Fallback
        if (usersList.length === 0) {
          usersList = [{ username: "admin", password: "123456", role: "admin", allowedAreas: [], allowedSales: [] }];
        }

        // 比對帳密
        const user = usersList.find(u => u.username === usernameInput && u.password === passwordInput);

        if (user) {
          // 登入成功：將 allowedAreas 與 allowedSales 一併存入 AppState
          showToast(`歡迎回來，${user.username}`, 'success');
          AppState.setUser({
            username: user.username,
            role: user.role,
            allowedAreas: Array.isArray(user.allowedAreas) ? user.allowedAreas : [],
            allowedSales: Array.isArray(user.allowedSales) ? user.allowedSales : []
          });
          
          // 觸發自訂事件，讓 router 接手後續畫面切換
          window.dispatchEvent(new CustomEvent('auth:login', { detail: user }));
        } else {
          // 登入失敗
          showToast('帳號或密碼錯誤', 'error');
          btn.disabled = false;
          btn.innerHTML = originalBtnHtml;
        }

      } catch (error) {
        console.error('Login error:', error);
        showToast('登入過程發生錯誤', 'error');
        btn.disabled = false;
        btn.innerHTML = originalBtnHtml;
      }
    });
  },

  destroy() {
    // 表單會隨 innerHTML 被清空，無需特別清理事件
  }
};

/*
F: AI-WA-UPWO/src/pages/login.js
V: 1792986000
W: UPGRADE-005
*/```

<!-- === FILE END: ./src/pages/login.js === -->

<!-- === FILE START: ./src/pages/overview.js === -->

## 📄 ./src/pages/overview.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument } from '../db/read.js';
import { updateDocument, deleteDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { renderOrderView } from '../ui/order-view.js';
import { createIcons, Inbox, PieChart, LayoutDashboard } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let schema = [];
let viewConfig = {};
let areas = [];

/**
 * 取得工單類別
 */
function getOrderCategory(order) {
  if (order.data && order.data.category) {
    return String(order.data.category).toUpperCase();
  }
  const match = String(order.id).match(/^[a-zA-Z]+/);
  return match ? match[0].toUpperCase() : '其他';
}

export default {
  async render() {
    if (!AppState.getUser()) return;

    try {
      const settingsDoc = await getDocument('settings', 'main');
      if (settingsDoc) {
        schema = settingsDoc.schema || [];
        viewConfig = settingsDoc.view_configs || {};
        areas = settingsDoc.areas || [];
      }
    } catch (err) {}

    renderPageLayout({
      title: '總狀態視圖',
      subtitle: '全廠區域狀態分佈與銷售員樞紐分析',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'overview-container'
    });

    window.addEventListener('refresh-overview', this.updateUI.bind(this));
    window.addEventListener('settings-updated', this.updateUI.bind(this));

    this.initDataListener();
  },

  initDataListener() {
    if (unsubscribe) unsubscribe();
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.updateUI();
    });
  },

  updateUI() {
    const container = document.getElementById('overview-container');
    if (!container) return;

    // 過濾垃圾資料
    const cleanOrders = currentOrders.filter(o => {
      const id = String(o.id || '');
      return id && !id.includes('***') && !id.includes('訂單');
    });

    if (cleanOrders.length === 0) {
      container.innerHTML = `
        <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft">
          <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
          <span class="text-base font-medium">目前尚無工單資料</span>
        </div>
      `;
      createIcons({ icons: { Inbox }, attrs: { class: 'w-12 h-12' } });
      return;
    }

    // 1. 建立「工序 -> 多個區域」映射
    const statusToAreas = {};
    areas.forEach(area => {
      (area.statuses || []).forEach(s => {
        if (!statusToAreas[s]) statusToAreas[s] = [];
        statusToAreas[s].push(area);
      });
    });

    // 2. 取得所有要顯示的列（區域名稱）
    const areaNames = areas.map(a => a.name);
    
    // 檢查是否有未分配到任何區域的工單（包含工序未綁定或類別不符）
    let hasUnassigned = false;

    // 3. 取得所有銷售員作為欄
    const allRepsSet = new Set();
    cleanOrders.forEach(o => {
      (o.salesReps || ['未填寫']).forEach(r => allRepsSet.add(r));
    });
    const allReps = Array.from(allRepsSet).sort();

    // 4. 生成樞紐分析資料 (區域 vs 銷售員)
    const pivotData = {};
    areaNames.forEach(areaName => {
      pivotData[areaName] = {};
      allReps.forEach(rep => { pivotData[areaName][rep] = 0; });
    });

    cleanOrders.forEach(order => {
      const reps = order.salesReps || ['未填寫'];
      const orderCategory = getOrderCategory(order);
      const orderStatus = order.status;
      
      const matchedAreas = statusToAreas[orderStatus] || [];
      let isAssignedToAnyVisibleArea = false;

      matchedAreas.forEach(area => {
        // 相容處理：檢查 area.categories（陣列）或 area.category（逗號分隔字串）
        let areaCats = [];
        if (Array.isArray(area.categories)) {
          areaCats = area.categories.map(c => String(c).toUpperCase());
        } else if (typeof area.category === 'string' && area.category.trim() !== '') {
          areaCats = area.category.split(',').map(c => c.trim().toUpperCase()).filter(Boolean);
        }

        // 如果區域沒有設定類別，或者包含該工單的類別
        const isCategoryMatch = areaCats.length === 0 || areaCats.includes(orderCategory);

        if (isCategoryMatch) {
          isAssignedToAnyVisibleArea = true;
          if (!pivotData[area.name]) pivotData[area.name] = {};
          
          reps.forEach(rep => {
            if (pivotData[area.name][rep] === undefined) pivotData[area.name][rep] = 0;
            pivotData[area.name][rep]++;
          });
        }
      });

      // 如果該工單不符合任何區域，標記為有未分配工單
      if (!isAssignedToAnyVisibleArea) {
        hasUnassigned = true;
        const unassignedName = '未分配區域';
        if (!pivotData[unassignedName]) {
          pivotData[unassignedName] = {};
          allReps.forEach(rep => { pivotData[unassignedName][rep] = 0; });
        }
        reps.forEach(rep => {
          if (pivotData[unassignedName][rep] === undefined) pivotData[unassignedName][rep] = 0;
          pivotData[unassignedName][rep]++;
        });
      }
    });

    // 若「未分配區域」有資料，將其加入列名中
    const finalAreaNames = areaNames.concat(hasUnassigned ? ['未分配區域'] : []);

    let pivotHtml = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden mb-6">
        <div class="px-6 py-4 border-b border-slate-100 bg-slate-50/50 flex items-center gap-2">
          <i data-lucide="pie-chart" class="w-5 h-5 text-brand"></i>
          <h2 class="font-bold text-slate-800">區域 / 銷售員 樞紐分析</h2>
        </div>
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse min-w-[600px]">
            <thead>
              <tr class="border-b border-slate-200/60 text-sm font-semibold text-slate-600 bg-slate-50/30">
                <th class="px-6 py-3 sticky left-0 bg-white/90 backdrop-blur z-10">生產區域</th>
                ${allReps.map(rep => `<th class="px-4 py-3 text-center whitespace-nowrap">${rep}</th>`).join('')}
                <th class="px-6 py-3 text-center text-brand">總計</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-slate-100 text-sm text-slate-700">
              ${finalAreaNames.map(areaName => {
                const rowData = pivotData[areaName] || {};
                let rowTotal = 0;
                
                const cellsHtml = allReps.map(rep => {
                  const count = rowData[rep] || 0;
                  rowTotal += count;
                  return `<td class="px-4 py-3 text-center ${count > 0 ? 'font-bold text-slate-800' : 'text-slate-300'}">${count || '-'}</td>`;
                }).join('');

                return `
                  <tr class="hover:bg-slate-50/50 transition-colors">
                    <td class="px-6 py-3 font-medium text-slate-700 sticky left-0 bg-white/90 backdrop-blur z-10">${areaName}</td>
                    ${cellsHtml}
                    <td class="px-6 py-3 text-center font-bold text-brand">${rowTotal}</td>
                  </tr>
                `;
              }).join('')}
            </tbody>
          </table>
        </div>
      </div>
    `;

    // 下半部：工單動態列表 (使用 order-view)
    let listHtml = `
      <div class="flex items-center gap-2 mb-4 mt-8">
        <i data-lucide="layout-dashboard" class="w-5 h-5 text-brand"></i>
        <h2 class="font-bold text-slate-800">工單狀態列表</h2>
      </div>
      <div id="overview-list-container"></div>
    `;

    container.innerHTML = pivotHtml + listHtml;

    createIcons({ icons: { PieChart, LayoutDashboard }, attrs: { class: 'w-5 h-5' } });

    // 依使用者設定的順序對齊欄位
    const visibleFields = (viewConfig['overview'] || ['id', 'status', 'salesReps', 'dueDate']).map(key => {
      const field = schema.find(f => f.key === key);
      return field ? Object.assign({}, field, { isVisible: true }) : null;
    }).filter(Boolean);

    // 渲染下半部的工單列表
    renderOrderView({
      containerId: 'overview-list-container',
      orders: cleanOrders,
      visibleFields: visibleFields,
      isAdmin: AppState.getUser()?.role === 'admin',
      pageKey: 'overview',
      isReadOnly: true,
      onAction: async (action, id, newStatus) => {
        if (action === 'delete') {
          if (await openConfirm({ title: '刪除', message: `確定刪除 ${id}？`, isDanger: true })) {
            await deleteDocument('work_orders', id);
            showToast('已刪除', 'success');
          }
        } else if (action === 'status_change') {
          if (newStatus) {
            await updateDocument('work_orders', id, { status: newStatus, statusUpdateDate: new Date().toISOString() });
            showToast('已更新生產進度', 'success');
          }
        }
      }
    });
  },

  destroy() {
    if (unsubscribe) unsubscribe();
    window.removeEventListener('refresh-overview', this.updateUI.bind(this));
    window.removeEventListener('settings-updated', this.updateUI.bind(this));
  }
};

/*
F: AI-WA-UPWO/src/pages/overview.js
V: 1792975000
W: UPGRADE-005
*/```

<!-- === FILE END: ./src/pages/overview.js === -->

<!-- === FILE START: ./src/pages/salesperson.js === -->

## 📄 ./src/pages/salesperson.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument } from '../db/read.js';
import { renderPageLayout } from '../ui/layout.js';
import { renderOrderView } from '../ui/order-view.js';
import { createIcons } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let schema = [];
let viewConfig = {};
let areas = [];
let currentRep = '';

export default {
  async render() {
    const user = AppState.getUser();
    if (!user) return;

    try {
      const settingsDoc = await getDocument('settings', 'main');
      if (settingsDoc) {
        schema = settingsDoc.schema || [];
        viewConfig = settingsDoc.view_configs || {};
        areas = settingsDoc.areas || [];
      }
    } catch (err) {}

    renderPageLayout({
      title: '銷售員視圖',
      subtitle: '依銷售員檢視名下工單與狀態分佈',
      headerActions: '',
      controls: `
        <div id="rep-tabs" class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-5 lg:grid-cols-6 xl:grid-cols-7 gap-2 mb-4"></div>
      `,
      listContainerId: 'salesperson-container'
    });

    window.addEventListener('refresh-salesperson', this.updateUI.bind(this));
    window.addEventListener('settings-updated', async () => {
      try {
        const settingsDoc = await getDocument('settings', 'main');
        if (settingsDoc) {
          schema = settingsDoc.schema || [];
          viewConfig = settingsDoc.view_configs || {};
          areas = settingsDoc.areas || [];
          this.updateUI();
        }
      } catch (err) {}
    });

    this.initDataListener();
  },

  initDataListener() {
    if (unsubscribe) unsubscribe();
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      
      const user = AppState.getUser();
      if (!user) return;
      
      const allowedSales = Array.isArray(user.allowedSales) ? user.allowedSales : [];
      
      // 提取所有銷售員
      let reps = Array.from(new Set(currentOrders.flatMap(o => o.salesReps || ['未填寫']))).sort();
      
      // 權限過濾：銷售員只能看到被授權的銷售員分頁
      if (user.role === 'sales') {
        if (allowedSales.length > 0) {
          reps = reps.filter(r => allowedSales.includes(r));
        }
      }

      // 如果第一次載入，自動選中第一個銷售員
      if (!currentRep || !reps.includes(currentRep)) {
        if (reps.length > 0) currentRep = reps[0];
        else currentRep = '';
      }
      
      this.updateUI(reps);
    });
  },

  updateUI(preCalculatedReps = null) {
    const user = AppState.getUser();
    if (!user) return;

    const allowedSales = Array.isArray(user.allowedSales) ? user.allowedSales : [];
    
    // 提取所有銷售員
    let reps = preCalculatedReps;
    if (!reps) {
      reps = Array.from(new Set(currentOrders.flatMap(o => o.salesReps || ['未填寫']))).sort();
      if (user.role === 'sales' && allowedSales.length > 0) {
        reps = reps.filter(r => allowedSales.includes(r));
      }
    }
    
    const tabsContainer = document.getElementById('rep-tabs');
    if (tabsContainer) {
      if (reps.length === 0) {
        tabsContainer.innerHTML = '<div class="text-slate-400 text-sm">無可見的銷售員資料</div>';
      } else {
        tabsContainer.innerHTML = reps.map(r => `
          <button class="w-full flex justify-between items-center px-3 py-2 rounded-xl text-sm font-medium border ${r === currentRep ? 'bg-brand text-white border-brand' : 'bg-white text-slate-600 border-slate-200'}" data-rep="${r}">
            <span class="truncate pr-2">${r}</span>
            <span class="text-xs px-1.5 py-0.5 rounded-md flex-shrink-0 ${r === currentRep ? 'bg-white/20' : 'bg-slate-100'}">${currentOrders.filter(o => (o.salesReps || ['未填寫']).includes(r)).length}</span>
          </button>
        `).join('');

        tabsContainer.querySelectorAll('button').forEach(btn => {
          btn.onclick = () => { currentRep = btn.dataset.rep; this.updateUI(reps); };
        });
      }
    }

    if (!currentRep) {
      const container = document.getElementById('salesperson-container');
      if (container) {
        container.innerHTML = `
          <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft mt-4">
            <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
            <span class="text-base font-medium">目前無銷售員資料</span>
          </div>
        `;
        createIcons({ icons: {}, attrs: { class: 'w-4 h-4' } });
      }
      return;
    }

    const filtered = currentOrders.filter(o => (o.salesReps || ['未填寫']).includes(currentRep));

    // 依使用者設定的順序對齊欄位
    let visibleFields = (viewConfig['salesperson'] || []).map(key => {
      const field = schema.find(f => f.key === key);
      return field ? Object.assign({}, field, { isVisible: true }) : null;
    }).filter(Boolean);

    // 強制加入「區域」欄位，插在「發票」之後（或最前面）
    if (!visibleFields.some(f => f.key === 'area_name')) {
      visibleFields.splice(1, 0, { key: 'area_name', label: '所在區域', isVisible: true });
    }

    renderOrderView({
      containerId: 'salesperson-container',
      orders: filtered,
      visibleFields: visibleFields,
      isAdmin: AppState.getUser()?.role === 'admin',
      pageKey: 'salesperson',
      isReadOnly: true, // 設定為唯讀
      groupBy: 'status', // 按工序分區塊顯示
      areas: areas, // 傳入區域資料供比對
      onAction: async (action, id, newStatus) => {
        // 唯讀模式下不會觸發這些
      }
    });
  },

  destroy() {
    if (unsubscribe) unsubscribe();
    window.removeEventListener('refresh-salesperson', this.updateUI.bind(this));
    window.removeEventListener('settings-updated', this.updateUI.bind(this));
  }
};

/*
F: AI-WA-UPWO/src/pages/salesperson.js
V: 1792988000
W: UPGRADE-005
*/```

<!-- === FILE END: ./src/pages/salesperson.js === -->

<!-- === FILE START: ./src/pages/search.js === -->

## 📄 ./src/pages/search.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument } from '../db/read.js';
import { renderPageLayout } from '../ui/layout.js';
import { renderOrderView } from '../ui/order-view.js';
import { searchOrders } from '../domain/global-search.js';
import { createIcons, Search, Filter, X } from 'lucide';

let unsubscribeOrders = null;
let allOrders = [];
let currentSettings = null;
let currentSearchResults = [];

export default {
  async render() {
    const user = AppState.getUser();
    if (!user) return;

    try {
      currentSettings = await getDocument('settings', 'main');
    } catch (err) {
      currentSettings = {};
    }

    renderPageLayout({
      title: '全域搜尋',
      subtitle: '跨欄位、跨頁面深度搜尋工單資料',
      headerActions: '',
      stats: [],
      controls: `
        <div class="bg-white p-4 rounded-2xl border border-slate-200/60 shadow-soft mb-6 flex flex-col gap-4">
          <!-- 搜尋列 -->
          <form id="search-form" class="flex items-center gap-2">
            <div class="relative flex-1">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none text-slate-400">
                <i data-lucide="search" class="w-5 h-5"></i>
              </div>
              <input type="text" id="search-keyword" class="w-full pl-10 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-base focus:outline-none focus:border-brand transition-colors" placeholder="輸入關鍵字搜尋單號、客戶、備註、金額等...">
            </div>
            <button type="submit" class="bg-brand text-white px-6 py-3 rounded-xl font-medium hover:bg-brand-hover shadow-md shadow-brand/20 transition-all flex items-center gap-2">
              搜尋
            </button>
            <button type="button" id="btn-toggle-filter" class="bg-white text-slate-600 border border-slate-200 px-4 py-3 rounded-xl font-medium hover:bg-slate-50 transition-colors flex items-center gap-2">
              <i data-lucide="filter" class="w-4 h-4"></i> 進階
            </button>
          </form>

          <!-- 進階篩選面板 (預設隱藏) -->
          <div id="advanced-filters" class="hidden grid grid-cols-1 md:grid-cols-4 gap-4 pt-4 border-t border-slate-100">
            <div class="flex flex-col gap-1.5">
              <label class="text-sm font-medium text-slate-700">狀態</label>
              <select id="filter-status" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand">
                <option value="">全部狀態</option>
                ${(currentSettings.statuses || []).map(s => `<option value="${s}">${s}</option>`).join('')}
              </select>
            </div>
            <div class="flex flex-col gap-1.5">
              <label class="text-sm font-medium text-slate-700">類別</label>
              <select id="filter-category" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand">
                <option value="">全部類別</option>
                ${(currentSettings.categories || []).map(c => `<option value="${c}">${c}</option>`).join('')}
              </select>
            </div>
            <div class="flex flex-col gap-1.5">
              <label class="text-sm font-medium text-slate-700">開始日期</label>
              <input type="date" id="filter-start-date" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand">
            </div>
            <div class="flex flex-col gap-1.5">
              <label class="text-sm font-medium text-slate-700">結束日期</label>
              <input type="date" id="filter-end-date" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand">
            </div>
          </div>
        </div>
        
        <div id="search-results-info" class="text-sm text-slate-500 mb-2 hidden"></div>
      `,
      listContainerId: 'search-results-container'
    });

    createIcons({ icons: { Search, Filter, X }, attrs: { class: 'w-4 h-4' } });

    this.bindEvents();
    this.initDataListener();
  },

  initDataListener() {
    if (unsubscribeOrders) unsubscribeOrders();
    unsubscribeOrders = listenCollection('work_orders', (orders) => {
      allOrders = orders.filter(o => {
        const id = String(o.id || '');
        return id && !id.includes('***') && !id.includes('訂單');
      });
      // 初始載入時不自動搜尋，除非已經有輸入條件
      this.doSearch();
    });
  },

  bindEvents() {
    const form = document.getElementById('search-form');
    const btnToggle = document.getElementById('btn-toggle-filter');
    const advancedPanel = document.getElementById('advanced-filters');

    // 切換進階面板
    btnToggle?.addEventListener('click', () => {
      advancedPanel.classList.toggle('hidden');
    });

    // 表單提交 (按下 Enter 或點擊搜尋)
    form?.addEventListener('submit', (e) => {
      e.preventDefault();
      this.doSearch();
    });
  },

  doSearch() {
    const keyword = document.getElementById('search-keyword')?.value || '';
    const filters = {
      status: document.getElementById('filter-status')?.value || '',
      category: document.getElementById('filter-category')?.value || '',
      startDate: document.getElementById('filter-start-date')?.value || '',
      endDate: document.getElementById('filter-end-date')?.value || ''
    };

    // 若無任何條件，預設顯示最新 50 筆
    const hasCondition = keyword.trim() !== '' || Object.values(filters).some(v => v !== '');

    let results = [];
    if (hasCondition) {
      results = searchOrders(allOrders, keyword, filters);
    } else {
      results = allOrders.slice(0, 50); // 預設顯示前 50 筆
    }

    currentSearchResults = results;
    
    // 更新結果數量提示
    const infoEl = document.getElementById('search-results-info');
    if (infoEl) {
      infoEl.classList.remove('hidden');
      if (hasCondition) {
        infoEl.innerHTML = `找到 <strong class="text-brand">${results.length}</strong> 筆符合條件的資料`;
      } else {
        infoEl.innerHTML = `目前顯示最新 <strong class="text-brand">${results.length}</strong> 筆資料（輸入條件以進行搜尋）`;
      }
    }

    this.renderResults(results);
  },

  renderResults(results) {
    // 取得顯示欄位 (使用管理總表的配置，或自訂義)
    const visibleFields = (currentSettings?.view_configs?.search || currentSettings?.view_configs?.table || ['id', 'status', 'customerNameZh', 'amount']).map(key => {
      const field = (currentSettings.schema || []).find(f => f.key === key);
      return field ? Object.assign({}, field, { isVisible: true }) : null;
    }).filter(Boolean);

    renderOrderView({
      containerId: 'search-results-container',
      orders: results,
      visibleFields: visibleFields,
      isAdmin: AppState.getUser()?.role === 'admin',
      pageKey: 'search',
      route_templates: currentSettings.route_templates || [],
      isReadOnly: true, // 搜尋結果僅供查看，不開放行內編輯
      onAction: async (action, id) => {
        // 僅處理刪除，若有的話
        if (action === 'delete') {
          // 此處不開放刪除，保留給管理總表
        }
      }
    });
  },

  destroy() {
    if (unsubscribeOrders) {
      unsubscribeOrders();
      unsubscribeOrders = null;
    }
  }
};

/*
F: AI-WA-UPWO/src/pages/search.js
V: 1793005000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/search.js === -->

<!-- === FILE START: ./src/pages/settings-area-status.js === -->

## 📄 ./src/pages/settings-area-status.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { createIcons, Plus, GripVertical, Trash2, Edit2 } from 'lucide';

/**
 * 渲染「區域狀態」管理頁籤
 * @param {HTMLElement} container 目標容器
 * @param {Object} currentSettings settings/main 的內容
 * @param {Function} saveSettings 儲存回呼（async）
 */
export function renderAreaStatuses(container, currentSettings, saveSettings) {
  // 若無資料，初始化預設值
  if (!currentSettings.areaStatuses) {
    currentSettings.areaStatuses = ['正常', '壞機', '休息中'];
  }
  
  const statuses = currentSettings.areaStatuses;

  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">區域狀態管理</h2>
        <p class="text-sm text-slate-500 mt-1">可拖曳排序。這些狀態將提供給「生產區域」切換使用（例如：正常、壞機）。</p>
      </div>
    </div>

    <div class="flex gap-2 mb-6">
      <input type="text" id="new-area-status-input" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand/20 focus:border-brand" placeholder="輸入新狀態名稱 (例如：維修中)">
      <button id="btn-add-area-status" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
        <i data-lucide="plus" class="w-4 h-4"></i> 新增
      </button>
    </div>

    <div class="inset-panel p-4 flex flex-col gap-2" id="area-status-list">
      ${statuses.length === 0 ? '<div class="text-slate-400 text-sm py-4 text-center">尚無狀態，請先新增</div>' : ''}
      ${statuses.map((status, index) => `
        <div class="flex items-center gap-3 bg-white p-3 rounded-xl border border-slate-100 shadow-sm area-status-item" data-index="${index}">
          <i data-lucide="grip-vertical" class="w-5 h-5 text-slate-300 cursor-grab"></i>
          <span class="flex-1 font-medium text-slate-700 area-status-name">${status}</span>
          <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-area-status" data-status="${status}">
            <i data-lucide="edit-2" class="w-4 h-4"></i>
          </button>
          <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-area-status" data-status="${status}">
            <i data-lucide="trash-2" class="w-4 h-4"></i>
          </button>
        </div>
      `).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, GripVertical, Trash2, Edit2 }, attrs: { class: 'w-4 h-4' } });

  // 初始化拖曳排序
  const statusList = document.getElementById('area-status-list');
  if (statusList && statuses.length > 1) {
    new Sortable(statusList, {
      animation: 150,
      handle: '.cursor-grab',
      ghostClass: 'opacity-50',
      forceFallback: true,
      fallbackTolerance: 3,
      onEnd: async () => {
        const newOrder = Array.from(statusList.children).map(el => el.querySelector('.area-status-name').textContent);
        currentSettings.areaStatuses = newOrder;
        await saveSettings();
        showToast('狀態排序已更新', 'success');
      }
    });
  }

  // 新增狀態
  document.getElementById('btn-add-area-status')?.addEventListener('click', async () => {
    const input = document.getElementById('new-area-status-input');
    const val = input.value.trim();
    if (!val) return;
    if (currentSettings.areaStatuses.includes(val)) {
      showToast('狀態已存在', 'error');
      return;
    }
    currentSettings.areaStatuses.push(val);
    await saveSettings();
    showToast('新增成功', 'success');
  });

  // 修改狀態
  document.querySelectorAll('.btn-edit-area-status').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const oldStatus = e.currentTarget.dataset.status;
      const newStatus = window.prompt(`修改狀態名稱（原為「${oldStatus}」）：`, oldStatus);
      if (!newStatus || newStatus.trim() === "" || newStatus === oldStatus) return;
      const trimmedNewStatus = newStatus.trim();

      if (currentSettings.areaStatuses.includes(trimmedNewStatus)) {
        showToast('該狀態名稱已存在', 'error');
        return;
      }

      const idx = currentSettings.areaStatuses.indexOf(oldStatus);
      if (idx !== -1) {
        currentSettings.areaStatuses[idx] = trimmedNewStatus;
        
        // 同步更新現有區域的 currentStatus (如果剛好正在使用該狀態)
        if (currentSettings.areas) {
          currentSettings.areas.forEach(area => {
            if (area.currentStatus === oldStatus) {
              area.currentStatus = trimmedNewStatus;
            }
          });
        }

        await saveSettings();
        showToast('修改成功', 'success');
      }
    });
  });

  // 刪除狀態
  document.querySelectorAll('.btn-del-area-status').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const status = e.currentTarget.dataset.status;
      const confirmed = await openConfirm({
        title: '刪除狀態',
        message: `確定要刪除「${status}」嗎？使用此狀態的區域將被重置回預設狀態。`,
        isDanger: true,
        confirmText: '確認刪除'
      });
      if (!confirmed) return;

      currentSettings.areaStatuses = currentSettings.areaStatuses.filter(s => s !== status);
      
      // 檢查是否有區域正在使用此狀態，若有則清空
      if (currentSettings.areas) {
        currentSettings.areas.forEach(area => {
          if (area.currentStatus === status) {
            area.currentStatus = '';
          }
        });
      }

      await saveSettings();
      showToast('刪除成功', 'success');
    });
  });
}

/*
F: AI-WA-UPWO/src/pages/settings-area-status.js
V: 1792977000
W: UPGRADE-005
*/```

<!-- === FILE END: ./src/pages/settings-area-status.js === -->

<!-- === FILE START: ./src/pages/settings-area.js === -->

## 📄 ./src/pages/settings-area.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm, openModal } from '../ui/modal.js';
import { createIcons, Plus, Trash2, Edit2, X, MapPin, GripVertical } from 'lucide';

/**
 * 渲染「區域管理」頁籤
 * - 新增／改名／刪除區域
 * - 區域支援綁定「負責類別」(categories) 與「允許狀態」(allowedStatuses)
 * - 拖曳排序區域（使用 SortableJS 支援手機觸控）
 * - 雙清單拖曳綁定工序（工序可重複綁定於多個區域）
 *
 * @param {HTMLElement} container 目標容器
 * @param {Object} currentSettings settings/main 的內容（含 areas、statuses、categories、areaStatuses）
 * @param {Function} saveSettings 儲存回呼（async）
 */
export function renderAreas(container, currentSettings, saveSettings) {
  const areas = currentSettings.areas || [];
  const statuses = currentSettings.statuses || [];
  const categories = currentSettings.categories || [];
  const areaStatuses = currentSettings.areaStatuses || ['正常', '壞機', '休息中'];

  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">區域管理</h2>
        <p class="text-sm text-slate-500 mt-1">可拖曳排序。綁定負責類別、工序與允許切換的狀態。</p>
      </div>
    </div>

    <div class="flex gap-2 mb-6">
      <input type="text" id="new-area-name" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand/20 focus:border-brand" placeholder="區域名稱 (例如：四色印刷機)">
      <button id="btn-add-area" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
        <i data-lucide="plus" class="w-4 h-4"></i> 新增區域
      </button>
    </div>

    <div class="flex flex-col gap-3" id="area-list">
      ${areas.length === 0 ? '<div class="text-slate-400 text-sm py-4 text-center">尚無區域，請先新增</div>' : ''}
      ${areas.map((area, index) => {
        const boundStatuses = area.statuses || [];
        const areaCats = Array.isArray(area.categories) ? area.categories : (area.category ? area.category.split(',').map(c=>c.trim()) : []);
        
        return `
          <div class="bg-white p-4 rounded-xl border border-slate-100 shadow-sm area-item" data-index="${index}">
            <div class="flex items-center justify-between mb-3">
              <div class="flex items-center gap-2 flex-wrap">
                <i data-lucide="grip-vertical" class="w-5 h-5 text-slate-300 cursor-grab"></i>
                <i data-lucide="map-pin" class="w-5 h-5 text-brand"></i>
                <span class="font-bold text-slate-800 area-name">${area.name}</span>
                ${areaCats.length > 0 
                  ? areaCats.map(c => `<span class="px-2 py-0.5 bg-blue-50 text-blue-600 text-[10px] font-bold rounded">類別：${c}</span>`).join('') 
                  : `<span class="px-2 py-0.5 bg-slate-100 text-slate-500 text-[10px] font-bold rounded">不分類別</span>`
                }
                <span class="text-xs px-2 py-0.5 bg-slate-100 text-slate-500 rounded">${boundStatuses.length} 個工序</span>
              </div>
              <div class="flex items-center gap-2">
                <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-area" data-index="${index}">
                  <i data-lucide="edit-2" class="w-4 h-4"></i>
                </button>
                <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-area" data-index="${index}">
                  <i data-lucide="trash-2" class="w-4 h-4"></i>
                </button>
              </div>
            </div>
            <div class="flex flex-wrap items-center gap-2 text-sm text-slate-600 bg-slate-50 p-3 rounded-lg">
              ${boundStatuses.length === 0 ? '<span class="text-slate-400">尚未綁定工序，點擊編輯加入</span>' : ''}
              ${boundStatuses.map((s, i) => `
                <span class="px-2 py-1 bg-white border border-slate-200 rounded-md"><span class="text-slate-400 mr-1 text-xs">${i+1}.</span>${s}</span>
                ${i < boundStatuses.length - 1 ? '<span class="text-slate-300">➔</span>' : ''}
              `).join('')}
            </div>
          </div>
        `;
      }).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, Trash2, Edit2, MapPin, GripVertical }, attrs: { class: 'w-4 h-4' } });

  // ── 初始化區域拖曳排序 (SortableJS) ──
  const areaList = document.getElementById('area-list');
  if (areaList && areas.length > 1) {
    new Sortable(areaList, {
      animation: 150,
      handle: '.cursor-grab',
      ghostClass: 'opacity-50',
      forceFallback: true,
      fallbackTolerance: 3,
      onEnd: async function (evt) {
        const newOrderIndexes = Array.from(areaList.children).map(el => parseInt(el.dataset.index));
        const newAreas = newOrderIndexes.map(i => currentSettings.areas[i]);
        currentSettings.areas = newAreas;
        await saveSettings();
        showToast('區域順序已更新', 'success');
      }
    });
  }

  // ── 新增區域 ──
  document.getElementById('btn-add-area')?.addEventListener('click', async () => {
    const input = document.getElementById('new-area-name');
    const name = input.value.trim();
    if (!name) {
      showToast('請輸入區域名稱', 'error');
      return;
    }
    if ((currentSettings.areas || []).some(a => a.name === name)) {
      showToast('區域名稱已存在', 'error');
      return;
    }
    const id = 'area_' + Date.now().toString(36);
    if (!currentSettings.areas) currentSettings.areas = [];
    currentSettings.areas.push({ id, name, categories: [], allowedStatuses: [], statuses: [], currentStatus: '' });
    await saveSettings();
    showToast('區域已新增', 'success');
  });

  // ── 刪除區域 ──
  document.querySelectorAll('.btn-del-area').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const idx = parseInt(e.currentTarget.dataset.index);
      const area = areas[idx];
      const confirmed = await openConfirm({
        title: '刪除區域',
        message: `確定要刪除「${area.name}」嗎？綁定的工序將回到未分配狀態。`,
        isDanger: true,
        confirmText: '確認刪除'
      });
      if (!confirmed) return;
      currentSettings.areas.splice(idx, 1);
      await saveSettings();
      showToast('區域已刪除', 'success');
    });
  });

  // ── 編輯區域（改名 + 類別 + 狀態 + 雙清單綁定工序） ──
  document.querySelectorAll('.btn-edit-area').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const idx = parseInt(e.currentTarget.dataset.index);
      const area = areas[idx];
      const areaCats = Array.isArray(area.categories) ? area.categories : (area.category ? area.category.split(',').map(c=>c.trim()) : []);
      const allowedSts = area.allowedStatuses || [];

      const content = `
        <div class="flex flex-col gap-4">
          <div class="flex flex-col md:flex-row gap-4">
            <div class="flex-1 flex flex-col gap-1">
              <label class="text-sm font-medium text-slate-700">區域名稱</label>
              <input type="text" id="edit-area-name" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" value="${area.name}">
            </div>
          </div>
          
          <div class="flex flex-col gap-1">
            <label class="text-sm font-medium text-slate-700">負責類別 (可多選，留空為通用)</label>
            <div class="flex flex-wrap gap-2">
              ${categories.length === 0 ? '<span class="text-xs text-slate-400">尚未建立任何類別</span>' : ''}
              ${categories.map(c => `
                <label class="flex items-center gap-1.5 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
                  <input type="checkbox" class="edit-area-cat-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${c}" ${areaCats.includes(c) ? 'checked' : ''}>
                  <span class="text-slate-700">${c}</span>
                </label>
              `).join('')}
            </div>
          </div>

          <div class="flex flex-col gap-1">
            <label class="text-sm font-medium text-slate-700">允許切換的區域狀態 (可多選)</label>
            <div class="flex flex-wrap gap-2">
              ${areaStatuses.length === 0 ? '<span class="text-xs text-slate-400">尚未建立區域狀態</span>' : ''}
              ${areaStatuses.map(st => `
                <label class="flex items-center gap-1.5 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
                  <input type="checkbox" class="edit-area-status-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${st}" ${allowedSts.includes(st) ? 'checked' : ''}>
                  <span class="text-slate-700">${st}</span>
                </label>
              `).join('')}
            </div>
          </div>

          <div class="flex flex-col gap-2 mt-2">
            <p class="text-sm text-slate-500">點擊左側加入右側，或拖曳右側調整順序。已綁定至其他區域的工序需先從該區域移除。</p>
            <div class="flex flex-col md:flex-row gap-4 h-[35vh]">
              <!-- 左側：可選工序 -->
              <div class="flex-1 bg-slate-50 border border-slate-200 rounded-xl p-3 flex flex-col">
                <h4 class="font-bold text-sm text-slate-700 mb-2">可選工序</h4>
                <div class="flex-1 overflow-y-auto flex flex-col gap-2" id="modal-available-list"></div>
              </div>
              <!-- 右側：已綁定工序順序 -->
              <div class="flex-1 bg-white border border-slate-200 rounded-xl p-3 flex flex-col shadow-sm">
                <h4 class="font-bold text-sm text-slate-700 mb-2">已綁定工序順序</h4>
                <div class="flex-1 overflow-y-auto flex flex-col gap-2" id="modal-selected-list"></div>
              </div>
            </div>
          </div>
        </div>
      `;

      const footer = `
        <div class="flex justify-end gap-3">
          <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
          <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">儲存</button>
        </div>
      `;

      const { close, element } = openModal({ title: `編輯區域`, content, footer, size: 'max-w-4xl' });

      let currentSteps = Array.from(area.statuses || []);
      let sortableInstance = null;

      const renderModalLists = () => {
        const availableList = element.querySelector('#modal-available-list');
        const selectedList = element.querySelector('#modal-selected-list');

        // 可選：不在 currentSteps 中的工序
        const availableStatuses = statuses.filter(s => !currentSteps.includes(s));

        if (availableStatuses.length === 0) {
          availableList.innerHTML = '<div class="text-slate-400 text-sm text-center py-4">所有工序已加入</div>';
        } else {
          availableList.innerHTML = availableStatuses.map(status => `
            <div class="flex items-center gap-3 bg-white p-3 rounded-lg border border-slate-200 cursor-pointer hover:border-brand transition-colors available-item" data-step="${status}">
              <span class="flex-1 font-medium text-slate-700">${status}</span>
              <i data-lucide="plus" class="w-4 h-4 text-slate-400"></i>
            </div>
          `).join('');
        }

        if (currentSteps.length === 0) {
          selectedList.innerHTML = '<div class="text-slate-400 text-sm text-center py-4">請從左側加入工序</div>';
        } else {
          selectedList.innerHTML = currentSteps.map((step, i) => `
            <div class="flex items-center gap-3 bg-slate-50 p-3 rounded-lg border border-slate-200 step-item" data-step="${step}">
              <i data-lucide="grip-vertical" class="w-4 h-4 text-slate-300 cursor-grab"></i>
              <span class="flex-1 font-medium text-slate-700"><span class="text-slate-400 mr-1">${i + 1}.</span>${step}</span>
              <button class="text-slate-400 hover:text-rose-500 btn-remove-step" data-step="${step}">
                <i data-lucide="x" class="w-4 h-4"></i>
              </button>
            </div>
          `).join('');
        }

        createIcons({ icons: { Plus, GripVertical, X }, attrs: { class: 'w-4 h-4' } });

        // 左側點擊加入
        availableList.querySelectorAll('.available-item').forEach(item => {
          item.addEventListener('click', () => {
            const step = item.dataset.step;
            if (!currentSteps.includes(step)) {
              currentSteps.push(step);
              renderModalLists();
            }
          });
        });

        // 右側移除
        selectedList.querySelectorAll('.btn-remove-step').forEach(btn => {
          btn.addEventListener('click', (ev) => {
            const step = ev.currentTarget.dataset.step;
            currentSteps = currentSteps.filter(s => s !== step);
            renderModalLists();
          });
        });

        // 銷毀舊的 Sortable 實例
        if (sortableInstance) {
          sortableInstance.destroy();
        }

        // 右側拖曳排序 (SortableJS)
        if (currentSteps.length > 0) {
          sortableInstance = new Sortable(selectedList, {
            animation: 150,
            handle: '.cursor-grab',
            ghostClass: 'opacity-50',
            forceFallback: true,
            fallbackTolerance: 3,
            onEnd: () => {
              currentSteps = Array.from(selectedList.children).map(el => el.dataset.step);
              renderModalLists();
            }
          });
        }
      };

      renderModalLists();

      element.querySelector('#modal-cancel-btn').addEventListener('click', close);

      element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
        const newName = element.querySelector('#edit-area-name').value.trim();
        if (!newName) {
          showToast('區域名稱不能為空', 'error');
          return;
        }
        if (currentSettings.areas.some((a, i) => i !== idx && a.name === newName)) {
          showToast('區域名稱已存在', 'error');
          return;
        }

        const newCats = Array.from(element.querySelectorAll('.edit-area-cat-checkbox:checked')).map(cb => cb.value);
        const newAllowedSts = Array.from(element.querySelectorAll('.edit-area-status-checkbox:checked')).map(cb => cb.value);

        currentSettings.areas[idx].name = newName;
        currentSettings.areas[idx].categories = newCats;
        currentSettings.areas[idx].allowedStatuses = newAllowedSts;
        currentSettings.areas[idx].statuses = currentSteps;
        
        // 確保有 currentStatus 欄位，若無則預設為第一個允許的狀態
        if (!currentSettings.areas[idx].currentStatus) {
          currentSettings.areas[idx].currentStatus = newAllowedSts.length > 0 ? newAllowedSts[0] : '';
        }

        await saveSettings();
        showToast('區域已更新', 'success');
        close();
      });
    });
  });
}

/*
F: AI-WA-UPWO/src/pages/settings-area.js
V: 1792982000
W: UPGRADE-005
*/```

<!-- === FILE END: ./src/pages/settings-area.js === -->

<!-- === FILE START: ./src/pages/settings-basic.js === -->

## 📄 ./src/pages/settings-basic.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm, openModal } from '../ui/modal.js';
import { getCollection } from '../db/read.js';
import { updateDocument } from '../db/write.js';
import { createIcons, Plus, GripVertical, Trash2, Edit2, Star, Settings2, X, Check } from 'lucide';

/**
 * 渲染狀態管理頁籤
 */
export function renderStatuses(container, currentSettings, saveSettings) {
  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">工序狀態管理</h2>
        <p class="text-sm text-slate-500 mt-1">可拖曳排序。修改名稱時會同步更新歷史工單與區域綁定。</p>
      </div>
    </div>
    
    <div class="flex gap-2 mb-6">
      <input type="text" id="new-status-input" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand/20 focus:border-brand" placeholder="輸入新狀態名稱&#8230;">
      <button id="btn-add-status" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
        <i data-lucide="plus" class="w-4 h-4"></i> 新增
      </button>
    </div>

    <div class="inset-panel p-4 flex flex-col gap-2" id="status-list">
      ${currentSettings.statuses.map((status, index) => `
        <div class="flex items-center gap-3 bg-white p-3 rounded-xl border border-slate-100 shadow-sm status-item" data-index="${index}">
          <i data-lucide="grip-vertical" class="w-5 h-5 text-slate-300 cursor-grab"></i>
          <span class="flex-1 font-medium text-slate-700 status-name">${status}</span>
          <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-status" data-status="${status}">
            <i data-lucide="edit-2" class="w-4 h-4"></i>
          </button>
          <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-status" data-status="${status}">
            <i data-lucide="trash-2" class="w-4 h-4"></i>
          </button>
        </div>
      `).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, GripVertical, Trash2, Edit2 }, attrs: { class: 'w-4 h-4' } });

  // 初始化 SortableJS（狀態排序）
  const statusList = document.getElementById('status-list');
  if (statusList) {
    new Sortable(statusList, {
      animation: 150,
      handle: '.cursor-grab',
      ghostClass: 'opacity-50',
      onEnd: async function (evt) {
        const newOrder = Array.from(statusList.children).map(el => el.querySelector('.status-name').textContent);
        currentSettings.statuses = newOrder;
        await saveSettings();
        showToast('排序已更新', 'success');
      }
    });
  }

  // 新增狀態
  document.getElementById('btn-add-status')?.addEventListener('click', async () => {
    const input = document.getElementById('new-status-input');
    const val = input.value.trim();
    if (!val) return;
    if (currentSettings.statuses.includes(val)) {
      showToast('狀態已存在', 'error');
      return;
    }
    currentSettings.statuses.push(val);
    await saveSettings();
    showToast('新增成功', 'success');
  });

  // 修改狀態
  document.querySelectorAll('.btn-edit-status').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const oldStatus = e.currentTarget.dataset.status;
      const newStatus = window.prompt(`修改狀態名稱（原為「${oldStatus}」）：`, oldStatus);
      if (!newStatus || newStatus.trim() === "" || newStatus === oldStatus) return;
      const trimmedNewStatus = newStatus.trim();

      showToast('正在更新狀態與歷史資料...', 'info');
      try {
        const idx = currentSettings.statuses.indexOf(oldStatus);
        if (idx !== -1) currentSettings.statuses[idx] = trimmedNewStatus;

        // 同步更新區域綁定中的工序名稱
        if (Array.isArray(currentSettings.areas)) {
          currentSettings.areas.forEach(area => {
            if (Array.isArray(area.statuses)) {
              area.statuses = area.statuses.map(s => s === oldStatus ? trimmedNewStatus : s);
            }
          });
        }

        await saveSettings();

        const orders = await getCollection('work_orders');
        let updateCount = 0;
        for (const order of orders) {
          if (order.status === oldStatus) {
            await updateDocument('work_orders', order.id, { status: trimmedNewStatus });
            updateCount++;
          }
        }
        showToast(`已修改狀態，並同步更新 ${updateCount} 筆工單`, 'success');
      } catch (err) {
        showToast('修改失敗，請稍後再試', 'error');
      }
    });
  });

  // 刪除狀態
  document.querySelectorAll('.btn-del-status').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const status = e.currentTarget.dataset.status;
      const confirmed = await openConfirm({
        title: '刪除狀態',
        message: `確定要刪除「${status}」嗎？系統將檢查是否有工單處於此狀態，並同步從區域綁定中移除。`,
        isDanger: true,
        confirmText: '檢查並刪除'
      });
      if (!confirmed) return;

      try {
        const orders = await getCollection('work_orders');
        const hasOrders = orders.some(o => o.status === status);
        if (hasOrders) {
          showToast(`無法刪除：仍有工單處於「${status}」狀態`, 'error');
          return;
        }
        currentSettings.statuses = currentSettings.statuses.filter(s => s !== status);

        // 同步從區域綁定中移除該工序
        if (Array.isArray(currentSettings.areas)) {
          currentSettings.areas.forEach(area => {
            if (Array.isArray(area.statuses)) {
              area.statuses = area.statuses.filter(s => s !== status);
            }
          });
        }

        await saveSettings();
        showToast('刪除成功', 'success');
      } catch (err) {
        showToast('檢查工單失敗', 'error');
      }
    });
  });
}

/**
 * 渲染製程路線圖管理頁籤（支援多選類別）
 */
export function renderRouteTemplates(container, currentSettings, saveSettings) {
  const categories = currentSettings.categories || [];

  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">製程路線圖管理</h2>
        <p class="text-sm text-slate-500 mt-1">建立生產路線並綁定類別。若多個類別共用相同路線，可同時勾選。</p>
      </div>
    </div>

    <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm mb-6 flex flex-col gap-3">
      <div class="flex flex-col sm:flex-row gap-2">
        <input type="text" id="new-template-name" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="路線圖名稱 (例如：卷裝LABEL生產流程)">
        <button id="btn-add-template" class="btn-primary px-4 py-2.5 text-sm flex items-center justify-center gap-2 whitespace-nowrap">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增路線圖
        </button>
      </div>
      <div class="flex flex-col gap-1.5">
        <label class="text-sm font-medium text-slate-700">對應類別 (可多選，全不選為通用預設)</label>
        <div class="flex flex-wrap gap-2">
          ${categories.length === 0 ? '<span class="text-xs text-slate-400">尚未建立任何類別</span>' : ''}
          ${categories.map(c => `
            <label class="flex items-center gap-1.5 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
              <input type="checkbox" class="new-tmpl-cat-cb w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${c}">
              <span class="text-slate-700">${c}</span>
            </label>
          `).join('')}
        </div>
      </div>
    </div>

    <div class="flex flex-col gap-3">
      ${currentSettings.route_templates.map((tmpl, index) => {
        // 💡 向下相容：舊資料可能是 category 字串
        const cats = Array.isArray(tmpl.categories) ? tmpl.categories : (tmpl.category ? [tmpl.category] : []);
        
        return `
        <div class="bg-white p-4 rounded-xl border border-slate-100 shadow-sm flex flex-col gap-3">
          <div class="flex items-center justify-between">
            <div class="flex items-center gap-2">
              <span class="font-bold text-slate-800 text-base">${tmpl.name}</span>
              ${cats.length > 0 
                ? cats.map(c => `<span class="px-2 py-0.5 bg-blue-50 text-blue-600 text-[10px] font-bold rounded">${c}</span>`).join('')
                : `<span class="px-2 py-0.5 bg-slate-100 text-slate-500 text-[10px] font-bold rounded">通用預設</span>`
              }
            </div>
            <div class="flex items-center gap-2">
              <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-template" data-index="${index}">
                <i data-lucide="edit-2" class="w-4 h-4"></i>
              </button>
              <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-template" data-index="${index}">
                <i data-lucide="trash-2" class="w-4 h-4"></i>
              </button>
            </div>
          </div>
          <div class="flex flex-wrap items-center gap-2 text-sm text-slate-600 bg-slate-50 p-3 rounded-lg">
            ${tmpl.steps.length === 0 ? '<span class="text-slate-400">無設定工序</span>' : ''}
            ${tmpl.steps.map((s, i) => `
              <span class="px-2 py-1 bg-white border border-slate-200 rounded-md"><span class="text-slate-400 mr-1 text-xs">${i+1}.</span>${s}</span>
              ${i < tmpl.steps.length - 1 ? '<span class="text-slate-300">➔</span>' : ''}
            `).join('')}
          </div>
        </div>
      `}).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, Trash2, Edit2 }, attrs: { class: 'w-4 h-4' } });

  // 新增模板
  document.getElementById('btn-add-template')?.addEventListener('click', async () => {
    const nameInput = document.getElementById('new-template-name');
    const name = nameInput.value.trim();
    
    if (!name) return;
    if (currentSettings.route_templates.some(t => t.name === name)) {
      showToast('名稱已存在', 'error');
      return;
    }
    
    // 💡 收集勾選的多個類別
    const selectedCats = Array.from(container.querySelectorAll('.new-tmpl-cat-cb:checked')).map(cb => cb.value);
    
    currentSettings.route_templates.push({ name, categories: selectedCats, steps: [] });
    await saveSettings();
    showToast('路線圖已新增，請編輯工序', 'success');
  });

  // 刪除模板
  document.querySelectorAll('.btn-del-template').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const idx = e.currentTarget.dataset.index;
      const confirmed = await openConfirm({
        title: '刪除路線圖',
        message: '確定要刪除此路線圖嗎？',
        isDanger: true
      });
      if (!confirmed) return;
      currentSettings.route_templates.splice(idx, 1);
      await saveSettings();
      showToast('已刪除', 'success');
    });
  });

  // 編輯模板（左右雙清單與基本資料）
  document.querySelectorAll('.btn-edit-template').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const idx = parseInt(e.currentTarget.dataset.index);
      const tmpl = currentSettings.route_templates[idx];
      const selectedCats = Array.isArray(tmpl.categories) ? tmpl.categories : (tmpl.category ? [tmpl.category] : []);
      
      const content = `
        <div class="flex flex-col gap-4 mb-2">
          <div class="flex flex-col gap-1">
            <label class="text-sm font-medium text-slate-700">路線圖名稱</label>
            <input type="text" id="edit-tmpl-name" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none" value="${tmpl.name}">
          </div>
          
          <div class="flex flex-col gap-1.5">
            <label class="text-sm font-medium text-slate-700">對應類別 (可多選，全不選為通用)</label>
            <div class="flex flex-wrap gap-2">
              ${categories.length === 0 ? '<span class="text-xs text-slate-400">尚未建立任何類別</span>' : ''}
              ${categories.map(c => `
                <label class="flex items-center gap-1.5 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
                  <input type="checkbox" class="edit-tmpl-cat-cb w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${c}" ${selectedCats.includes(c) ? 'checked' : ''}>
                  <span class="text-slate-700">${c}</span>
                </label>
              `).join('')}
            </div>
          </div>
        </div>

        <div class="flex flex-col gap-2 mt-4">
          <p class="text-sm text-slate-500">點擊左側加入右側，或長按左側把手拖曳調整順序。</p>
          <div class="flex flex-col md:flex-row gap-4 h-[40vh]">
            <!-- 左側：可選工序 -->
            <div class="flex-1 bg-slate-50 border border-slate-200 rounded-xl p-3 flex flex-col">
              <h4 class="font-bold text-sm text-slate-700 mb-2">可選工序</h4>
              <div class="flex-1 overflow-y-auto flex flex-col gap-2" id="modal-available-list"></div>
            </div>
            <!-- 右側：預計工序順序 -->
            <div class="flex-1 bg-white border border-slate-200 rounded-xl p-3 flex flex-col shadow-sm">
              <h4 class="font-bold text-sm text-slate-700 mb-2">預計工序順序</h4>
              <div class="flex-1 overflow-y-auto flex flex-col gap-2" id="modal-selected-list"></div>
            </div>
          </div>
        </div>
      `;

      const footer = `
        <div class="flex justify-end gap-3">
          <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
          <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">儲存</button>
        </div>
      `;

      const { close, element } = openModal({ title: `編輯路線圖`, content, footer, size: 'max-w-4xl' });
      
      let currentSteps = Array.from(tmpl.steps);
      let sortableInstance = null;

      const renderModalLists = () => {
        const availableList = element.querySelector('#modal-available-list');
        const selectedList = element.querySelector('#modal-selected-list');
        
        const availableStatuses = currentSettings.statuses.filter(s => !currentSteps.includes(s));

        // 渲染左側
        if (availableStatuses.length === 0) {
          availableList.innerHTML = '<div class="text-slate-400 text-sm text-center py-4">所有工序已加入</div>';
        } else {
          availableList.innerHTML = availableStatuses.map(status => `
            <div class="flex items-center gap-3 bg-white p-3 rounded-lg border border-slate-200 cursor-pointer hover:border-brand transition-colors available-item" data-step="${status}">
              <span class="flex-1 font-medium text-slate-700">${status}</span>
              <i data-lucide="plus" class="w-4 h-4 text-slate-400"></i>
            </div>
          `).join('');
        }

        // 渲染右側
        if (currentSteps.length === 0) {
          selectedList.innerHTML = '<div class="text-slate-400 text-sm text-center py-4">請從左側加入工序</div>';
        } else {
          selectedList.innerHTML = currentSteps.map((step, i) => `
            <div class="flex items-center gap-3 bg-slate-50 p-3 rounded-lg border border-slate-200 step-item" data-step="${step}">
              <i data-lucide="grip-vertical" class="w-4 h-4 text-slate-300 cursor-grab"></i>
              <span class="flex-1 font-medium text-slate-700"><span class="text-slate-400 mr-1">${i + 1}.</span>${step}</span>
              <button class="text-slate-400 hover:text-rose-500 btn-remove-step" data-step="${step}">
                <i data-lucide="x" class="w-4 h-4"></i>
              </button>
            </div>
          `).join('');
        }

        createIcons({ icons: { Plus, GripVertical, X }, attrs: { class: 'w-4 h-4' } });
        
        // 綁定左側點擊加入
        availableList.querySelectorAll('.available-item').forEach(item => {
          item.addEventListener('click', () => {
            const step = item.dataset.step;
            if (!currentSteps.includes(step)) {
              currentSteps.push(step);
              renderModalLists();
            }
          });
        });

        // 綁定右側移除
        selectedList.querySelectorAll('.btn-remove-step').forEach(btn => {
          btn.addEventListener('click', (ev) => {
            const step = ev.currentTarget.dataset.step;
            currentSteps = currentSteps.filter(s => s !== step);
            renderModalLists();
          });
        });

        // 銷毀舊的 Sortable 實例
        if (sortableInstance) {
          sortableInstance.destroy();
        }

        // 綁定右側拖曳 (使用 SortableJS，加入 forceFallback 解決手機滾動衝突)
        if (currentSteps.length > 0) {
          sortableInstance = new Sortable(selectedList, {
            animation: 150,
            handle: '.cursor-grab',
            ghostClass: 'opacity-50',
            forceFallback: true,
            fallbackTolerance: 3,
            onEnd: () => {
              currentSteps = Array.from(selectedList.children).map(el => el.dataset.step);
              renderModalLists(); // 重新渲染以更新序號
            }
          });
        }
      };

      renderModalLists();

      element.querySelector('#modal-cancel-btn').addEventListener('click', close);

      element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
        const newName = element.querySelector('#edit-tmpl-name').value.trim();
        const newCategories = Array.from(element.querySelectorAll('.edit-tmpl-cat-cb:checked')).map(cb => cb.value);
        
        if (!newName) {
          showToast('名稱不能為空', 'error');
          return;
        }

        currentSettings.route_templates[idx].name = newName;
        currentSettings.route_templates[idx].categories = newCategories;
        // 清除舊的單選欄位以避免混淆
        if (currentSettings.route_templates[idx].category) {
          delete currentSettings.route_templates[idx].category;
        }
        currentSettings.route_templates[idx].steps = currentSteps;
        
        await saveSettings();
        showToast('路線圖已更新', 'success');
        close();
      });
    });
  });
}

/**
 * 渲染銷售員對照表管理頁籤
 */
export function renderMapping(container, currentSettings, saveSettings) {
  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">銷售員對照表</h2>
        <p class="text-sm text-slate-500 mt-1">設定別名，匯入時自動對應。修改後會自動同步歷史工單。</p>
      </div>
    </div>

    <div class="flex gap-2 mb-6">
      <input type="text" id="new-rep-name" class="w-1/3 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="正式名稱 (例：王小明)">
      <input type="text" id="new-rep-alias" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="別名，用逗號隔開 (例：123, S-123)">
      <button id="btn-add-rep" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
        <i data-lucide="plus" class="w-4 h-4"></i> 新增
      </button>
    </div>

    <div class="flex flex-col gap-2">
      ${currentSettings.sales_mapping.length === 0 ? '<div class="text-slate-400 text-sm py-4 text-center">尚無對照表資料</div>' : ''}
      ${currentSettings.sales_mapping.map((rep, index) => `
        <div class="flex items-center gap-4 bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
          <div class="w-1/3 font-bold text-slate-800">${rep.name}</div>
          <div class="flex-1 flex flex-wrap gap-1">
            ${(rep.aliases || []).map(a => `<span class="px-2 py-0.5 bg-slate-100 text-slate-600 rounded text-xs">${a}</span>`).join('')}
          </div>
          <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-rep" data-index="${index}">
            <i data-lucide="edit-2" class="w-4 h-4"></i>
          </button>
          <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-rep" data-index="${index}">
            <i data-lucide="trash-2" class="w-4 h-4"></i>
          </button>
        </div>
      `).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, Trash2, Edit2 }, attrs: { class: 'w-4 h-4' } });

  document.getElementById('btn-add-rep')?.addEventListener('click', async () => {
    const name = document.getElementById('new-rep-name').value.trim();
    const aliasStr = document.getElementById('new-rep-alias').value.trim();
    if (!name || !aliasStr) {
      showToast('請輸入正式名稱與至少一個別名', 'error');
      return;
    }
    
    const aliases = aliasStr.split(',').map(s => s.trim()).filter(Boolean);
    currentSettings.sales_mapping.push({ name, aliases });
    await saveSettings();

    showToast('正在同步歷史工單...', 'info');
    try {
      const orders = await getCollection('work_orders');
      let updateCount = 0;
      for (const order of orders) {
        let reps = order.salesReps || [];
        let changed = false;
        aliases.forEach(alias => {
          if (reps.includes(alias)) {
            reps = reps.filter(r => r !== alias);
            reps.push(name);
            changed = true;
          }
        });
        if (changed) {
          await updateDocument('work_orders', order.id, { salesReps: Array.from(new Set(reps)) });
          updateCount++;
        }
      }
      showToast(`新增成功，同步更新了 ${updateCount} 筆工單`, 'success');
    } catch (err) {
      showToast('對照表已新增，但同步歷史資料失敗', 'error');
    }
  });

  // 修改為單一表單彈窗
  document.querySelectorAll('.btn-edit-rep').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const idx = e.currentTarget.dataset.index;
      const oldRep = currentSettings.sales_mapping[idx];

      const content = `
        <div class="flex flex-col gap-4">
          <div class="flex flex-col gap-1.5">
            <label class="text-sm font-medium text-slate-700">正式名稱</label>
            <input type="text" id="edit-rep-name" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:border-brand" value="${oldRep.name}">
          </div>
          <div class="flex flex-col gap-1.5">
            <label class="text-sm font-medium text-slate-700">別名 (用逗號隔開)</label>
            <input type="text" id="edit-rep-alias" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:border-brand" value="${(oldRep.aliases||[]).join(', ')}">
          </div>
        </div>
      `;

      const footer = `
        <div class="flex justify-end gap-3">
          <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200 transition-colors">取消</button>
          <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover shadow-md shadow-brand/20 active:scale-95 transition-all">儲存</button>
        </div>
      `;

      const { close, element } = openModal({ title: '編輯銷售員', content, footer });

      element.querySelector('#modal-cancel-btn').addEventListener('click', close);

      element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
        const newName = element.querySelector('#edit-rep-name').value.trim();
        const newAliasStr = element.querySelector('#edit-rep-alias').value.trim();

        if (!newName) {
          showToast('正式名稱不能為空', 'error');
          return;
        }

        if (newName === oldRep.name && newAliasStr === (oldRep.aliases||[]).join(', ')) {
          close();
          return;
        }

        const confirmBtn = element.querySelector('#modal-confirm-btn');
        confirmBtn.disabled = true;
        confirmBtn.textContent = '更新中...';

        showToast('正在更新設定與歷史資料...', 'info');
        try {
          const newAliases = newAliasStr.split(',').map(s => s.trim()).filter(Boolean);
          currentSettings.sales_mapping[idx] = { name: newName, aliases: newAliases };
          await saveSettings();

          const orders = await getCollection('work_orders');
          let updateCount = 0;
          for (const order of orders) {
            let reps = order.salesReps || [];
            let changed = false;
            if (reps.includes(oldRep.name)) {
              reps = reps.filter(r => r !== oldRep.name);
              reps.push(newName);
              changed = true;
            }
            (oldRep.aliases || []).forEach(alias => {
              if (reps.includes(alias)) {
                reps = reps.filter(r => r !== alias);
                reps.push(newName);
                changed = true;
              }
            });
            if (changed) {
              await updateDocument('work_orders', order.id, { salesReps: Array.from(new Set(reps)) });
              updateCount++;
            }
          }
          showToast(`修改成功，同步更新了 ${updateCount} 筆工單`, 'success');
          close();
        } catch (err) {
          console.error(err);
          showToast('修改失敗，請稍後再試', 'error');
          confirmBtn.disabled = false;
          confirmBtn.textContent = '儲存';
        }
      });
    });
  });

  document.querySelectorAll('.btn-del-rep').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const idx = e.currentTarget.dataset.index;
      currentSettings.sales_mapping.splice(idx, 1);
      await saveSettings();
      showToast('刪除成功', 'success');
    });
  });
}

/*
F: AI-WA-UPWO/src/pages/settings-basic.js
V: 1793046000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/settings-basic.js === -->

<!-- === FILE START: ./src/pages/settings-category.js === -->

## 📄 ./src/pages/settings-category.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { getCollection } from '../db/read.js';
import { createIcons, Plus, GripVertical, Trash2, Edit2, RefreshCw } from 'lucide';

/**
 * 渲染「類別管理」頁籤
 * @param {HTMLElement} container 目標容器
 * @param {Object} currentSettings settings/main 的內容（含 categories）
 * @param {Function} saveSettings 儲存回呼（async）
 */
export function renderCategories(container, currentSettings, saveSettings) {
  const categories = currentSettings.categories || [];

  container.innerHTML = `
    <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4">
      <div>
        <h2 class="text-lg font-bold text-slate-800">工單類別管理</h2>
        <p class="text-sm text-slate-500 mt-1">可拖曳排序。新增類別後，可用於過濾工單與設定區域。</p>
      </div>
      <div>
        <button id="btn-sync-categories" class="bg-white text-brand border border-brand/30 px-4 py-2 rounded-xl text-sm font-medium hover:bg-brand/5 flex items-center gap-2 shadow-sm transition-colors">
          <i data-lucide="refresh-cw" class="w-4 h-4"></i> 從工單同步類別
        </button>
      </div>
    </div>

    <div class="flex gap-2 mb-6">
      <input type="text" id="new-category-input" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand/20 focus:border-brand" placeholder="輸入新類別名稱 (例如：UG)">
      <button id="btn-add-category" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
        <i data-lucide="plus" class="w-4 h-4"></i> 新增
      </button>
    </div>

    <div class="inset-panel p-4 flex flex-col gap-2" id="category-list">
      ${categories.length === 0 ? '<div class="text-slate-400 text-sm py-4 text-center">尚無類別，請先新增</div>' : ''}
      ${categories.map((cat, index) => `
        <div class="flex items-center gap-3 bg-white p-3 rounded-xl border border-slate-100 shadow-sm category-item" data-index="${index}">
          <i data-lucide="grip-vertical" class="w-5 h-5 text-slate-300 cursor-grab"></i>
          <span class="flex-1 font-medium text-slate-700 category-name">${cat}</span>
          <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-category" data-category="${cat}">
            <i data-lucide="edit-2" class="w-4 h-4"></i>
          </button>
          <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-category" data-category="${cat}">
            <i data-lucide="trash-2" class="w-4 h-4"></i>
          </button>
        </div>
      `).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, GripVertical, Trash2, Edit2, RefreshCw }, attrs: { class: 'w-4 h-4' } });

  // 初始化拖曳排序
  const categoryList = document.getElementById('category-list');
  if (categoryList && categories.length > 1) {
    new Sortable(categoryList, {
      animation: 150,
      handle: '.cursor-grab',
      ghostClass: 'opacity-50',
      forceFallback: true,
      fallbackTolerance: 3,
      onEnd: async () => {
        const newOrder = Array.from(categoryList.children).map(el => el.querySelector('.category-name').textContent);
        currentSettings.categories = newOrder;
        await saveSettings();
        showToast('類別排序已更新', 'success');
      }
    });
  }

  // 💡 從工單同步類別
  document.getElementById('btn-sync-categories')?.addEventListener('click', async () => {
    showToast('正在掃描工單類別...', 'info');
    try {
      const orders = await getCollection('work_orders');
      const currentCats = new Set(categories);
      let addedCount = 0;

      orders.forEach(o => {
        let cat = '';
        if (o.data && o.data.category) {
          cat = String(o.data.category).toUpperCase();
        } else {
          const match = String(o.id).match(/^[a-zA-Z]+/);
          cat = match ? match[0].toUpperCase() : '其他';
        }

        if (cat && cat !== '其他' && !currentCats.has(cat)) {
          currentCats.add(cat);
          addedCount++;
        }
      });

      if (addedCount > 0) {
        currentSettings.categories = Array.from(currentCats).sort();
        await saveSettings();
        showToast(`已自動補齊 ${addedCount} 個新類別`, 'success');
      } else {
        showToast('所有工單類別皆已存在，無需補齊', 'info');
      }
    } catch (err) {
      console.error(err);
      showToast('同步失敗，請查看 Console', 'error');
    }
  });

  // 新增類別
  document.getElementById('btn-add-category')?.addEventListener('click', async () => {
    const input = document.getElementById('new-category-input');
    const val = input.value.trim().toUpperCase();
    if (!val) return;
    if (!currentSettings.categories) currentSettings.categories = [];
    if (currentSettings.categories.includes(val)) {
      showToast('類別已存在', 'error');
      return;
    }
    currentSettings.categories.push(val);
    await saveSettings();
    showToast('新增成功', 'success');
  });

  // 修改類別
  document.querySelectorAll('.btn-edit-category').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const oldCat = e.currentTarget.dataset.category;
      const newCat = window.prompt(`修改類別名稱（原為「${oldCat}」）：`, oldCat);
      if (!newCat || newCat.trim() === "" || newCat === oldCat) return;
      const trimmedNewCat = newCat.trim().toUpperCase();

      if (currentSettings.categories.includes(trimmedNewCat)) {
        showToast('該類別名稱已存在', 'error');
        return;
      }

      const idx = currentSettings.categories.indexOf(oldCat);
      if (idx !== -1) {
        currentSettings.categories[idx] = trimmedNewCat;
        await saveSettings();
        showToast('修改成功', 'success');
      }
    });
  });

  // 刪除類別
  document.querySelectorAll('.btn-del-category').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const cat = e.currentTarget.dataset.category;
      const confirmed = await openConfirm({
        title: '刪除類別',
        message: `確定要刪除「${cat}」嗎？此操作不會影響已存在的工單資料。`,
        isDanger: true,
        confirmText: '確認刪除'
      });
      if (!confirmed) return;

      currentSettings.categories = currentSettings.categories.filter(c => c !== cat);
      await saveSettings();
      showToast('刪除成功', 'success');
    });
  });
}

/*
F: AI-WA-UPWO/src/pages/settings-category.js
V: 1793070000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/settings-category.js === -->

<!-- === FILE START: ./src/pages/settings-database.js === -->

## 📄 ./src/pages/settings-database.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm, openModal } from '../ui/modal.js';
import { createIcons, Plus, Trash2, Edit2, Settings2, Database, GripVertical, X, Check } from 'lucide';

let activeDbId = '';

export function renderDatabaseCenter(container, currentSettings, saveSettings) {
  if (!Array.isArray(currentSettings.custom_databases)) {
    currentSettings.custom_databases = [];
  }

  const databases = currentSettings.custom_databases;

  // 如果沒有選中的 DB，自動選第一個
  if (databases.length > 0 && (!activeDbId || !databases.some(db => db.id === activeDbId))) {
    activeDbId = databases[0].id;
  }

  // 💡 統一的儲存與雙向同步邏輯
  const updateAndSave = async (newDbs) => {
    let payload = { custom_databases: newDbs };
    
    // 雙向同步回舊的 sales_mapping
    const salesDb = newDbs.find(db => db.id === 'db_sales');
    if (salesDb) {
      payload.sales_mapping = salesDb.items.map(item => {
        const aliasStr = item.data && item.data.aliases ? item.data.aliases : '';
        return {
          name: item.name,
          aliases: aliasStr.split(',').map(s => s.trim()).filter(Boolean)
        };
      });
    }

    await saveSettings(payload);
    
    // 局部更新畫面，不重新載入整個頁面
    currentSettings.custom_databases = newDbs;
    if (payload.sales_mapping) currentSettings.sales_mapping = payload.sales_mapping;
    
    renderDatabaseCenter(container, currentSettings, saveSettings);
  };

  // 渲染主框架
  container.innerHTML = `
    <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4">
      <div>
        <h2 class="text-lg font-bold text-slate-800">資料庫中心</h2>
        <p class="text-sm text-slate-500 mt-1">建立自訂資料庫（如：客戶、機台），並可於工單表單中綁定為下拉選單。</p>
      </div>
      <div>
        <button id="btn-add-database" class="btn-primary px-4 py-2 rounded-xl text-sm font-medium flex items-center gap-2 shadow-sm">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增資料庫
        </button>
      </div>
    </div>

    <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden min-h-[500px] flex flex-col md:flex-row">
      <!-- 左側選單 -->
      <div class="w-full md:w-56 border-b md:border-b-0 md:border-r border-slate-100 bg-slate-50/50 p-3 flex flex-row md:flex-col gap-1 flex-shrink-0 overflow-x-auto md:overflow-y-auto">
        ${databases.length === 0 ? '<div class="text-sm text-slate-400 p-4 text-center">尚無資料庫</div>' : ''}
        ${databases.map(db => `
          <button class="db-tab-btn whitespace-nowrap text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3 ${db.id === activeDbId ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'}" data-id="${db.id}">
            <i data-lucide="database" class="w-4 h-4"></i> ${db.name}
          </button>
        `).join('')}
      </div>

      <!-- 右側內容 -->
      <div class="flex-1 p-6 md:p-8 flex flex-col relative" id="db-content"></div>
    </div>
  `;

  createIcons({ icons: { Plus, Database }, attrs: { class: 'w-4 h-4' } });

  // 綁定 Tab 切換
  container.querySelectorAll('.db-tab-btn').forEach(btn => {
    btn.addEventListener('click', (e) => {
      activeDbId = e.currentTarget.dataset.id;
      renderDatabaseCenter(container, currentSettings, saveSettings);
    });
  });

  // 綁定新增資料庫
  document.getElementById('btn-add-database')?.addEventListener('click', () => {
    const content = `
      <div class="flex flex-col gap-4">
        <div class="flex flex-col gap-1.5">
          <label class="text-sm font-medium text-slate-700">資料庫名稱</label>
          <input type="text" id="new-db-name" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" placeholder="例如：客戶資料、供應商資料">
        </div>
      </div>
    `;
    const footer = `
      <div class="flex justify-end gap-3">
        <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
        <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">建立</button>
      </div>
    `;

    const { close, element } = openModal({ title: '新增資料庫', content, footer });

    element.querySelector('#modal-cancel-btn').addEventListener('click', close);
    element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
      const name = element.querySelector('#new-db-name').value.trim();
      if (!name) {
        showToast('請輸入名稱', 'error');
        return;
      }

      const newDb = {
        id: 'db_' + Date.now().toString(36),
        name: name,
        fields: [],
        items: []
      };

      const newDbs = [...databases, newDb];
      activeDbId = newDb.id;
      await updateAndSave(newDbs);
      showToast('資料庫已建立', 'success');
      close();
    });
  });

  // 💡 將渲染右側內容的邏輯寫在此處，確保能存取 currentSettings 與 updateAndSave
  const renderActiveDbContent = () => {
    const contentContainer = document.getElementById('db-content');
    if (!contentContainer) return;

    const db = currentSettings.custom_databases.find(d => d.id === activeDbId);
    if (!db) {
      contentContainer.innerHTML = `<div class="flex items-center justify-center h-full text-slate-400">請選擇或建立一個資料庫</div>`;
      return;
    }

    contentContainer.innerHTML = `
      <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4 border-b border-slate-100 pb-4">
        <div>
          <h3 class="text-lg font-bold text-slate-800">${db.name} <span class="text-xs font-normal text-slate-400 ml-2">共 ${db.items.length} 筆資料</span></h3>
          <p class="text-sm text-slate-500 mt-1">管理此資料庫的欄位與資料項目。</p>
        </div>
        <div class="flex gap-2">
          <button id="btn-manage-db-fields" class="bg-white text-slate-700 border border-slate-200 px-3 py-2 rounded-lg text-sm font-medium hover:bg-slate-50 flex items-center gap-2 shadow-sm">
            <i data-lucide="settings-2" class="w-4 h-4"></i> 設定欄位
          </button>
          <button id="btn-del-database" class="bg-rose-50 text-rose-600 border border-rose-100 px-3 py-2 rounded-lg text-sm font-medium hover:bg-rose-100 flex items-center gap-2 shadow-sm">
            <i data-lucide="trash-2" class="w-4 h-4"></i> 刪除資料庫
          </button>
        </div>
      </div>

      <div class="flex gap-2 mb-6">
        <input type="text" id="new-db-item-name" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:border-brand" placeholder="新增資料名稱 (主標題)">
        <button id="btn-add-db-item" class="btn-primary px-4 py-2.5 text-sm flex items-center justify-center gap-2 whitespace-nowrap">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增資料
        </button>
      </div>

      <div class="flex-1 overflow-y-auto">
        <div class="flex flex-col gap-2">
          ${db.items.length === 0 ? '<div class="text-slate-400 text-sm text-center py-8">尚無資料</div>' : ''}
          ${db.items.map((item, index) => {
            const summary = (db.fields || []).map(f => {
              const val = item.data ? item.data[f.key] : '';
              if (!val) return '';
              return `<span class="text-xs bg-slate-100 text-slate-600 px-2 py-0.5 rounded mr-2">${f.label}: ${val}</span>`;
            }).join('');

            return `
              <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm flex items-center justify-between group">
                <div class="flex flex-col gap-1">
                  <span class="font-bold text-slate-800">${item.name}</span>
                  <div class="flex flex-wrap gap-y-1">${summary}</div>
                </div>
                <div class="flex items-center gap-2 opacity-0 group-hover:opacity-100 transition-opacity">
                  <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg btn-edit-db-item" data-index="${index}">
                    <i data-lucide="edit-2" class="w-4 h-4"></i>
                  </button>
                  <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg btn-del-db-item" data-index="${index}">
                    <i data-lucide="trash-2" class="w-4 h-4"></i>
                  </button>
                </div>
              </div>
            `;
          }).join('')}
        </div>
      </div>
    `;

    createIcons({ icons: { Settings2, Trash2, Plus, Edit2 }, attrs: { class: 'w-4 h-4' } });

    // 刪除資料庫
    contentContainer.querySelector('#btn-del-database')?.addEventListener('click', async () => {
      const confirmed = await openConfirm({
        title: '刪除資料庫',
        message: `確定要刪除「${db.name}」嗎？這將會刪除底下所有資料，且無法復原。`,
        isDanger: true
      });
      if (!confirmed) return;

      const newDbs = currentSettings.custom_databases.filter(d => d.id !== activeDbId);
      activeDbId = newDbs.length > 0 ? newDbs[0].id : '';
      await updateAndSave(newDbs);
      showToast('資料庫已刪除', 'success');
    });

    // 設定欄位
    contentContainer.querySelector('#btn-manage-db-fields')?.addEventListener('click', () => {
      renderDbFieldsModal(db);
    });

    // 新增資料
    contentContainer.querySelector('#btn-add-db-item')?.addEventListener('click', () => {
      const nameInput = contentContainer.querySelector('#new-db-item-name');
      const name = nameInput.value.trim();
      if (!name) return;

      const newItem = { id: 'item_' + Date.now().toString(36), name: name, data: {} };
      const newDbs = JSON.parse(JSON.stringify(currentSettings.custom_databases));
      const targetDb = newDbs.find(d => d.id === activeDbId);
      targetDb.items.push(newItem);
      
      updateAndSave(newDbs).then(() => showToast('資料已新增', 'success'));
    });

    // 編輯資料
    contentContainer.querySelectorAll('.btn-edit-db-item').forEach(btn => {
      btn.addEventListener('click', (e) => {
        const idx = parseInt(e.currentTarget.dataset.index);
        const item = db.items[idx];
        const fields = db.fields || [];

        if (fields.length === 0) {
          showToast('該資料庫尚未設定自訂欄位，請先點擊「設定欄位」', 'info');
          return;
        }

        const content = `
          <div class="flex flex-col gap-4">
            <div class="flex flex-col gap-1.5">
              <label class="text-sm font-medium text-slate-700">資料名稱 (主標題)</label>
              <input type="text" id="edit-item-name" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" value="${item.name}">
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-2">
              ${fields.map(f => {
                const val = item.data ? item.data[f.key] : '';
                const inputType = f.type === 'number' ? 'number' : 'text';
                return `
                  <div class="flex flex-col gap-1.5">
                    <label class="text-sm font-medium text-slate-700">${f.label}</label>
                    <input type="${inputType}" class="edit-item-data-input w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" 
                      data-key="${f.key}" value="${val || ''}" placeholder="請輸入${f.label}">
                  </div>
                `;
              }).join('')}
            </div>
          </div>
        `;

        const footer = `
          <div class="flex justify-end gap-3">
            <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
            <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">儲存</button>
          </div>
        `;

        const { close, element } = openModal({ title: `編輯資料 - ${item.name}`, content, footer });

        element.querySelector('#modal-cancel-btn').addEventListener('click', close);
        element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
          const newName = element.querySelector('#edit-item-name').value.trim();
          if (!newName) {
            showToast('名稱不能為空', 'error');
            return;
          }

          const newData = {};
          element.querySelectorAll('.edit-item-data-input').forEach(input => {
            newData[input.dataset.key] = input.value;
          });

          const newDbs = JSON.parse(JSON.stringify(currentSettings.custom_databases));
          const targetItem = newDbs.find(d => d.id === activeDbId).items[idx];
          targetItem.name = newName;
          targetItem.data = newData;

          await updateAndSave(newDbs);
          showToast('資料已更新', 'success');
          close();
        });
      });
    });

    // 刪除資料
    contentContainer.querySelectorAll('.btn-del-db-item').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        const idx = parseInt(e.currentTarget.dataset.index);
        const confirmed = await openConfirm({
          title: '刪除資料',
          message: `確定要刪除「${db.items[idx].name}」嗎？`,
          isDanger: true
        });
        if (!confirmed) return;

        const newDbs = JSON.parse(JSON.stringify(currentSettings.custom_databases));
        newDbs.find(d => d.id === activeDbId).items.splice(idx, 1);
        await updateAndSave(newDbs);
        showToast('已刪除', 'success');
      });
    });
  };

  // 設定欄位彈窗
  const renderDbFieldsModal = (db) => {
    const content = `
      <div class="flex flex-col gap-4">
        <div class="flex gap-2">
          <input type="text" id="new-db-field-label" class="flex-1 px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none" placeholder="新增欄位名稱 (例如：電話)">
          <select id="new-db-field-type" class="w-24 px-2 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none">
            <option value="string">文字</option>
            <option value="number">數字</option>
          </select>
          <button id="btn-add-db-field" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium whitespace-nowrap">新增</button>
        </div>

        <div class="flex flex-col gap-2 mt-4" id="db-fields-list">
          ${(db.fields || []).map((f, i) => `
            <div class="flex items-center justify-between bg-white p-3 rounded-lg border border-slate-200">
              <div class="flex flex-col">
                <span class="font-bold text-slate-700">${f.label}</span>
                <span class="text-xs text-slate-400 font-mono">${f.key} (${f.type === 'number' ? '數字' : '文字'})</span>
              </div>
              <button class="p-1.5 text-slate-400 hover:text-rose-500 btn-del-db-field" data-key="${f.key}">
                <i data-lucide="trash-2" class="w-4 h-4"></i>
              </button>
            </div>
          `).join('')}
          ${(db.fields || []).length === 0 ? '<div class="text-slate-400 text-sm text-center py-4">尚未建立自訂欄位</div>' : ''}
        </div>
      </div>
    `;

    const footer = `
      <div class="flex justify-end gap-3">
        <button id="modal-close-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">完成</button>
      </div>
    `;

    const { close, element } = openModal({ title: `設定欄位 - ${db.name}`, content, footer });

    element.querySelector('#modal-close-btn').addEventListener('click', close);

    element.querySelector('#btn-add-db-field')?.addEventListener('click', async () => {
      const label = element.querySelector('#new-db-field-label').value.trim();
      const type = element.querySelector('#new-db-field-type').value;
      if (!label) return;

      const newDbs = JSON.parse(JSON.stringify(currentSettings.custom_databases));
      const targetDb = newDbs.find(d => d.id === db.id);
      if (!targetDb.fields) targetDb.fields = [];
      targetDb.fields.push({ key: 'field_' + Date.now().toString(36), label, type });
      
      await updateAndSave(newDbs);
      close();
      showToast('欄位已新增，請重新點擊「設定欄位」', 'success');
    });

    element.querySelectorAll('.btn-del-db-field').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        const key = e.currentTarget.dataset.key;
        
        const newDbs = JSON.parse(JSON.stringify(currentSettings.custom_databases));
        const targetDb = newDbs.find(d => d.id === db.id);
        targetDb.fields = targetDb.fields.filter(f => f.key !== key);
        
        await updateAndSave(newDbs);
        close();
        showToast('欄位已刪除', 'success');
      });
    });
  };

  // 執行渲染
  renderActiveDbContent();
}

/*
F: AI-WA-UPWO/src/pages/settings-database.js
V: 1793091000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/settings-database.js === -->

<!-- === FILE START: ./src/pages/settings-form.js === -->

## 📄 ./src/pages/settings-form.js

```js
import { AppState } from '../config/state.js';
import { listenDocument } from '../db/read.js';
import { updateDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { createIcons, LayoutDashboard, List, FileEdit, FunctionSquare, CheckSquare, Save, Plus, Trash2, Menu, GripVertical, Settings } from 'lucide';

// 引入拆分出去的模組
import { renderSchema, renderViewConfig } from './settings-schema.js';

let unsubscribe = null;
// 💡 修正：加入 custom_databases
export let currentSettings = { schema: [], view_configs: {}, form_layout: 'B', formulas: {}, diff_fields: ['amount'], nav_config: [], custom_databases: [] };
let activeTab = 'layout';

export default {
  async render() {
    const user = AppState.getUser();
    if (!user || user.role !== 'admin') {
      document.getElementById('app-content').innerHTML = '<div class="p-8 text-center text-rose-500 font-bold">您沒有權限訪問此頁面</div>';
      return;
    }

    renderPageLayout({
      title: '工單管理設定',
      subtitle: '管理系統欄位、視圖配置、表單佈局、公式與核對清單',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'settings-form-container'
    });

    this.renderTabs();
    this.initDataListener();
  },

  initDataListener() {
    unsubscribe = listenDocument('settings', 'main', (data) => {
      if (data) {
        currentSettings = {
          schema: data.schema || [],
          view_configs: data.view_configs || {},
          form_layout: data.form_layout || 'B',
          formulas: data.formulas || {},
          diff_fields: Array.isArray(data.diff_fields) ? data.diff_fields : ['amount'],
          nav_config: Array.isArray(data.nav_config) ? data.nav_config : [],
          custom_databases: Array.isArray(data.custom_databases) ? data.custom_databases : [] // 💡 補上這行
        };
      }
      this.renderTabs();
    });
  },

  renderTabs() {
    const container = document.getElementById('settings-form-container');
    if (!container) return;

    container.innerHTML = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden min-h-[500px] flex flex-col md:flex-row">
        
        <div class="w-full md:w-64 border-r border-slate-100 bg-slate-50/50 p-4 flex flex-col gap-1 flex-shrink-0 overflow-y-auto">
          <button class="tab-btn ${activeTab === 'layout' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="layout">
            <i data-lucide="layout-dashboard" class="w-4 h-4"></i> 表單佈局
          </button>
          <button class="tab-btn ${activeTab === 'schema' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="schema">
            <i data-lucide="list" class="w-4 h-4"></i> 欄位管理
          </button>
          <button class="tab-btn ${activeTab === 'view_config' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="view_config">
            <i data-lucide="layout" class="w-4 h-4"></i> 視圖配置
          </button>
          <button class="tab-btn ${activeTab === 'nav_config' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="nav_config">
            <i data-lucide="menu" class="w-4 h-4"></i> 頁面管理
          </button>
          <button class="tab-btn ${activeTab === 'formulas' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="formulas">
            <i data-lucide="function-square" class="w-4 h-4"></i> 公式設定
          </button>
          <button class="tab-btn ${activeTab === 'diff_fields' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="diff_fields">
            <i data-lucide="check-square" class="w-4 h-4"></i> 核對清單
          </button>
        </div>

        <div class="flex-1 p-6 md:p-8" id="settings-form-content"></div>
      </div>
    `;

    createIcons({
      icons: { LayoutDashboard, List, FileEdit, FunctionSquare, CheckSquare, Save, Plus, Trash2, Menu, GripVertical, Settings },
      attrs: { class: 'w-4 h-4' }
    });

    this.bindTabEvents();
    this.renderContent();
  },

  bindTabEvents() {
    document.querySelectorAll('.tab-btn').forEach(btn => {
      btn.addEventListener('click', (e) => {
        activeTab = e.currentTarget.dataset.tab;
        this.renderTabs();
      });
    });
  },

  renderContent() {
    const content = document.getElementById('settings-form-content');
    if (!content) return;

    if (activeTab === 'layout') {
      this.renderFormLayout(content);
    } else if (activeTab === 'schema') {
      renderSchema(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'view_config') {
      renderViewConfig(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'nav_config') {
      this.renderNavConfig(content);
    } else if (activeTab === 'formulas') {
      this.renderFormulas(content);
    } else if (activeTab === 'diff_fields') {
      this.renderDiffFields(content);
    }
  },

  renderFormLayout(container) {
    const mode = currentSettings.form_layout || 'B';
    
    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">表單佈局</h2>
          <p class="text-sm text-slate-500 mt-1">決定「工單編輯彈窗」的排版方式。</p>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <label class="border ${mode === 'A' ? 'border-brand bg-brand/5' : 'border-slate-200 bg-white'} rounded-xl p-4 cursor-pointer hover:border-brand transition-colors flex flex-col gap-2">
          <div class="flex items-center gap-2">
            <input type="radio" name="form_layout" value="A" ${mode === 'A' ? 'checked' : ''} class="w-4 h-4 text-brand focus:ring-brand">
            <span class="font-bold text-slate-800">模式 A：一頁到底</span>
          </div>
          <p class="text-xs text-slate-500">所有欄位平鋪顯示，適合欄位較少的情況。</p>
        </label>

        <label class="border ${mode === 'B' ? 'border-brand bg-brand/5' : 'border-slate-200 bg-white'} rounded-xl p-4 cursor-pointer hover:border-brand transition-colors flex flex-col gap-2">
          <div class="flex items-center gap-2">
            <input type="radio" name="form_layout" value="B" ${mode === 'B' ? 'checked' : ''} class="w-4 h-4 text-brand focus:ring-brand">
            <span class="font-bold text-slate-800">模式 B：分頁 (預設)</span>
          </div>
          <p class="text-xs text-slate-500">將欄位分類為基本、金額、狀態等分頁顯示。</p>
        </label>

        <label class="border ${mode === 'C' ? 'border-brand bg-brand/5' : 'border-slate-200 bg-white'} rounded-xl p-4 cursor-pointer hover:border-brand transition-colors flex flex-col gap-2">
          <div class="flex items-center gap-2">
            <input type="radio" name="form_layout" value="C" ${mode === 'C' ? 'checked' : ''} class="w-4 h-4 text-brand focus:ring-brand">
            <span class="font-bold text-slate-800">模式 C：左右對照</span>
          </div>
          <p class="text-xs text-slate-500">左側顯示工單清單，右側為詳細編輯表單。</p>
        </label>
      </div>
      
      <div class="mt-8 flex justify-end">
        <button id="btn-save-layout" class="btn-primary px-6 py-2.5 rounded-xl text-sm font-medium flex items-center gap-2">
          <i data-lucide="save" class="w-4 h-4"></i> 儲存佈局設定
        </button>
      </div>
    `;

    createIcons({ icons: { Save }, attrs: { class: 'w-4 h-4' } });

    document.getElementById('btn-save-layout')?.addEventListener('click', async () => {
      const selected = document.querySelector('input[name="form_layout"]:checked')?.value;
      if (selected) {
        currentSettings.form_layout = selected;
        await this.saveSettings();
        showToast('佈局設定已儲存', 'success');
      }
    });
  },

  renderNavConfig(container) {
    if (!currentSettings.nav_config || currentSettings.nav_config.length === 0) {
      currentSettings.nav_config = [
        { id: 'kanban', label: '工單進度表', icon: 'layout-dashboard', showSidebar: true, showTop: true, isEnabled: true },
        { id: 'search', label: '全域搜尋', icon: 'search', showSidebar: true, showTop: false, isEnabled: true },
        { id: 'table', label: '管理總表', icon: 'table', showSidebar: true, showTop: true, isEnabled: true },
        { id: 'overview', label: '總狀態', icon: 'pie-chart', showSidebar: true, showTop: true, isEnabled: true },
        { id: 'category', label: '類別視圖', icon: 'layers', showSidebar: true, showTop: true, isEnabled: true },
        { id: 'salesperson', label: '銷售員', icon: 'users', showSidebar: true, showTop: true, isEnabled: true },
        { id: 'area', label: '生產區域', icon: 'map-pin', showSidebar: true, showTop: false, isEnabled: true },
        { id: 'settings', label: '生產管理設定', icon: 'settings', showSidebar: true, showTop: false, isEnabled: true },
        { id: 'settings-form', label: '工單管理設定', icon: 'sliders', showSidebar: true, showTop: false, isEnabled: true },
        { id: 'admin-data', label: '資料總管', icon: 'database', showSidebar: true, showTop: false, isEnabled: true }
      ];
    }

    currentSettings.nav_config.forEach(item => {
      if (item.isEnabled === undefined) item.isEnabled = true;
    });

    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">頁面管理</h2>
          <p class="text-sm text-slate-500 mt-1">可拖曳排序。勾選決定該頁面是否啟用，以及是否顯示於側欄或頂部。</p>
        </div>
      </div>

      <div class="flex flex-col gap-2" id="nav-list">
        ${currentSettings.nav_config.map((item, index) => {
          const isCore = item.id === 'table';
          return `
          <div class="flex items-center gap-4 bg-white p-3 rounded-xl border border-slate-200 shadow-sm nav-item ${!item.isEnabled ? 'opacity-60' : ''}" data-index="${index}">
            <i data-lucide="grip-vertical" class="w-5 h-5 text-slate-300 cursor-grab"></i>
            <div class="flex-1 font-bold text-slate-700">${item.label} ${isCore ? '<span class="text-[10px] text-brand bg-brand/10 px-1.5 py-0.5 rounded ml-2">核心</span>' : ''}</div>
            <div class="flex items-center gap-6 mr-4">
              <label class="flex items-center gap-2 cursor-pointer text-sm ${isCore ? 'text-slate-400' : 'text-slate-600'}">
                <input type="checkbox" class="nav-enabled-cb w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" data-index="${index}" ${item.isEnabled ? 'checked' : ''} ${isCore ? 'disabled title="核心頁面不可停用"' : ''}>
                啟用
              </label>
              <label class="flex items-center gap-2 cursor-pointer text-sm text-slate-600">
                <input type="checkbox" class="nav-sidebar-cb w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" data-index="${index}" ${item.showSidebar ? 'checked' : ''}>
                顯示於側欄
              </label>
              <label class="flex items-center gap-2 cursor-pointer text-sm text-slate-600">
                <input type="checkbox" class="nav-top-cb w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" data-index="${index}" ${item.showTop ? 'checked' : ''}>
                顯示於頂部
              </label>
            </div>
          </div>
          `;
        }).join('')}
      </div>

      <div class="mt-8 flex justify-end">
        <button id="btn-save-nav" class="btn-primary px-6 py-2.5 rounded-xl text-sm font-medium flex items-center gap-2">
          <i data-lucide="save" class="w-4 h-4"></i> 儲存頁面管理
        </button>
      </div>
    `;

    createIcons({ icons: { Save, GripVertical }, attrs: { class: 'w-4 h-4' } });

    const navList = document.getElementById('nav-list');
    if (navList) {
      new Sortable(navList, {
        animation: 150,
        handle: '.cursor-grab',
        ghostClass: 'opacity-50',
        onEnd: () => {
          const newOrderIndexes = Array.from(navList.children).map(el => parseInt(el.dataset.index));
          const newConfig = newOrderIndexes.map(i => currentSettings.nav_config[i]);
          currentSettings.nav_config = newConfig;
          this.renderNavConfig(container);
          showToast('順序已更新，請點擊儲存', 'info');
        }
      });
    }

    container.querySelectorAll('.nav-enabled-cb').forEach(cb => {
      cb.addEventListener('change', (e) => {
        const idx = parseInt(e.target.dataset.index);
        currentSettings.nav_config[idx].isEnabled = e.target.checked;
      });
    });
    container.querySelectorAll('.nav-sidebar-cb').forEach(cb => {
      cb.addEventListener('change', (e) => {
        const idx = parseInt(e.target.dataset.index);
        currentSettings.nav_config[idx].showSidebar = e.target.checked;
      });
    });
    container.querySelectorAll('.nav-top-cb').forEach(cb => {
      cb.addEventListener('change', (e) => {
        const idx = parseInt(e.target.dataset.index);
        currentSettings.nav_config[idx].showTop = e.target.checked;
      });
    });

    document.getElementById('btn-save-nav')?.addEventListener('click', async () => {
      await this.saveSettings();
      window.dispatchEvent(new Event('nav-config-updated'));
      showToast('頁面管理已儲存', 'success');
    });
  },

  renderFormulas(container) {
    const editableFields = currentSettings.schema.filter(f => f.isEditable);

    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">公式設定</h2>
          <p class="text-sm text-slate-500 mt-1">設定欄位的自動計算公式。使用 {{欄位Key}} 作為變數。例如：{{quantity}} * {{price}}</p>
        </div>
      </div>

      <div class="flex flex-col gap-4">
        ${editableFields.map(f => {
          const formula = currentSettings.formulas[f.key] || '';
          return `
            <div class="flex flex-col md:flex-row md:items-center gap-4 bg-white p-4 rounded-xl border border-slate-200 shadow-sm">
              <div class="w-full md:w-1/3">
                <div class="font-bold text-slate-800">${f.label}</div>
                <div class="text-xs text-slate-400 font-mono">${f.key}</div>
              </div>
              <div class="flex-1">
                <input type="text" class="formula-input w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" 
                  data-key="${f.key}" 
                  value="${formula}" 
                  placeholder="留空表示手動輸入">
              </div>
            </div>
          `;
        }).join('')}
      </div>

      <div class="mt-8 flex justify-end">
        <button id="btn-save-formulas" class="btn-primary px-6 py-2.5 rounded-xl text-sm font-medium flex items-center gap-2">
          <i data-lucide="save" class="w-4 h-4"></i> 儲存公式
        </button>
      </div>
    `;

    createIcons({ icons: { Save }, attrs: { class: 'w-4 h-4' } });

    document.getElementById('btn-save-formulas')?.addEventListener('click', async () => {
      const newFormulas = {};
      document.querySelectorAll('.formula-input').forEach(input => {
        const key = input.dataset.key;
        const val = input.value.trim();
        if (val) newFormulas[key] = val;
      });
      currentSettings.formulas = newFormulas;
      await this.saveSettings();
      showToast('公式設定已儲存', 'success');
    });
  },

  renderDiffFields(container) {
    const allFields = currentSettings.schema;
    const diffFields = currentSettings.diff_fields || ['amount'];

    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">核對清單</h2>
          <p class="text-sm text-slate-500 mt-1">勾選的欄位在 Excel 匯入時，若與手動輸入不同將標記警示（保留手動輸入值）。</p>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-3">
        ${allFields.map(f => {
          if (f.key === 'id') return '';
          const checked = diffFields.includes(f.key);
          return `
            <label class="flex items-center gap-3 bg-white p-4 rounded-xl border border-slate-200 shadow-sm cursor-pointer hover:border-brand">
              <input type="checkbox" class="diff-checkbox w-5 h-5 text-brand rounded border-slate-300 focus:ring-brand" value="${f.key}" ${checked ? 'checked' : ''}>
              <div class="flex flex-col">
                <span class="font-bold text-slate-800">${f.label}</span>
                <span class="text-xs text-slate-400 font-mono">${f.key}</span>
              </div>
            </label>
          `;
        }).join('')}
      </div>

      <div class="mt-8 flex justify-end">
        <button id="btn-save-diff" class="btn-primary px-6 py-2.5 rounded-xl text-sm font-medium flex items-center gap-2">
          <i data-lucide="save" class="w-4 h-4"></i> 儲存核對清單
        </button>
      </div>
    `;

    createIcons({ icons: { Save }, attrs: { class: 'w-4 h-4' } });

    document.getElementById('btn-save-diff')?.addEventListener('click', async () => {
      const selected = Array.from(document.querySelectorAll('.diff-checkbox:checked')).map(cb => cb.value);
      currentSettings.diff_fields = selected;
      await this.saveSettings();
      showToast('核對清單已儲存', 'success');
    });
  },

  async saveSettings() {
    const dataToSave = {
      schema: currentSettings.schema,
      view_configs: currentSettings.view_configs,
      form_layout: currentSettings.form_layout,
      formulas: currentSettings.formulas,
      diff_fields: currentSettings.diff_fields,
      nav_config: currentSettings.nav_config
    };
    await updateDocument('settings', 'main', dataToSave);
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
  }
};

/*
F: AI-WA-UPWO/src/pages/settings-form.js
V: 1793093000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/settings-form.js === -->

<!-- === FILE START: ./src/pages/settings-material.js === -->

## 📄 ./src/pages/settings-material.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm, openModal } from '../ui/modal.js';
import { createIcons, Plus, Trash2, Edit2, GripVertical, X, Save } from 'lucide';

/**
 * 渲染物料管理頁面
 * @param {HTMLElement} container 目標容器
 * @param {Object} currentSettings settings/main 的內容
 * @param {Function} saveSettings 儲存回呼（只更新特定欄位）
 */
export function renderMaterials(container, currentSettings, saveSettings) {
  // 初始化資料結構
  if (!Array.isArray(currentSettings.materials)) currentSettings.materials = [];
  if (!Array.isArray(currentSettings.material_schema)) currentSettings.material_schema = [];

  const materials = currentSettings.materials;
  const categories = currentSettings.categories || [];

  container.innerHTML = `
    <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4">
      <div>
        <h2 class="text-lg font-bold text-slate-800">物料管理</h2>
        <p class="text-sm text-slate-500 mt-1">建立物料主檔，可綁定多個工單類別，並自訂專屬的詳細欄位。</p>
      </div>
      <div class="flex gap-2">
        <button id="btn-manage-mat-schema" class="bg-white text-slate-700 border border-slate-200 px-4 py-2 rounded-xl text-sm font-medium hover:bg-slate-50 flex items-center gap-2 shadow-sm">
          自訂物料欄位
        </button>
      </div>
    </div>

    <div class="bg-slate-50 border border-slate-200 rounded-2xl p-4 mb-6 flex flex-col gap-3">
      <div class="text-sm font-semibold text-slate-700">新增物料</div>
      <div class="flex flex-col sm:flex-row gap-2">
        <input type="text" id="new-mat-name" class="flex-1 px-4 py-2.5 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="物料名稱 (例如：110G-毛紙)">
        <button id="btn-add-mat" class="btn-primary px-4 py-2.5 text-sm flex items-center justify-center gap-2 whitespace-nowrap">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增
        </button>
      </div>
      <div class="flex flex-col gap-1.5 mt-2">
        <label class="text-sm font-medium text-slate-700">適用類別 (可多選，留空為通用)</label>
        <div class="flex flex-wrap gap-2">
          ${categories.length === 0 ? '<span class="text-xs text-slate-400">尚未建立任何類別</span>' : ''}
          ${categories.map(c => `
            <label class="flex items-center gap-1.5 px-3 py-1.5 bg-white border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
              <input type="checkbox" class="new-mat-cat-cb w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${c}">
              <span class="text-slate-700">${c}</span>
            </label>
          `).join('')}
        </div>
      </div>
    </div>

    <div class="flex flex-col gap-3">
      ${materials.length === 0 ? '<div class="text-slate-400 text-sm py-4 text-center">尚無物料資料</div>' : ''}
      ${materials.map((mat, index) => {
        const cats = Array.isArray(mat.categories) ? mat.categories : [];
        return `
          <div class="bg-white p-4 rounded-xl border border-slate-100 shadow-sm flex flex-col gap-2">
            <div class="flex items-center justify-between">
              <div class="flex items-center gap-2">
                <button class="font-bold text-brand hover:underline text-base btn-edit-mat-detail" data-index="${index}">${mat.name}</button>
                ${cats.length > 0 
                  ? cats.map(c => `<span class="px-2 py-0.5 bg-blue-50 text-blue-600 text-[10px] font-bold rounded">${c}</span>`).join('')
                  : `<span class="px-2 py-0.5 bg-slate-100 text-slate-500 text-[10px] font-bold rounded">通用</span>`
                }
              </div>
              <div class="flex items-center gap-2">
                <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-mat" data-index="${index}">
                  <i data-lucide="edit-2" class="w-4 h-4"></i>
                </button>
                <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-mat" data-index="${index}">
                  <i data-lucide="trash-2" class="w-4 h-4"></i>
                </button>
              </div>
            </div>
            <!-- 顯示自訂欄位的摘要 -->
            <div class="flex flex-wrap gap-4 text-sm text-slate-600 mt-1">
              ${(currentSettings.material_schema || []).map(field => {
                const val = mat.data ? mat.data[field.key] : '';
                if (!val) return '';
                return `<div><span class="text-slate-400 text-xs">${field.label}:</span> ${val}</div>`;
              }).join('')}
            </div>
          </div>
        `;
      }).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, Trash2, Edit2 }, attrs: { class: 'w-4 h-4' } });

  // 1. 新增物料
  document.getElementById('btn-add-mat')?.addEventListener('click', async () => {
    const nameInput = document.getElementById('new-mat-name');
    const name = nameInput.value.trim();
    if (!name) return;
    if (materials.some(m => m.name === name)) {
      showToast('物料名稱已存在', 'error');
      return;
    }
    
    const selectedCats = Array.from(container.querySelectorAll('.new-mat-cat-cb:checked')).map(cb => cb.value);
    
    const newMat = { 
      id: 'mat_' + Date.now().toString(36), 
      name, 
      categories: selectedCats,
      data: {}
    };
    
    materials.push(newMat);
    await saveSettings({ materials });
    showToast('物料已新增', 'success');
  });

  // 2. 刪除物料
  container.querySelectorAll('.btn-del-mat').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const idx = e.currentTarget.dataset.index;
      const confirmed = await openConfirm({
        title: '刪除物料',
        message: `確定要刪除「${materials[idx].name}」嗎？此操作不影響已存在的工單。`,
        isDanger: true
      });
      if (!confirmed) return;
      materials.splice(idx, 1);
      await saveSettings({ materials });
      showToast('物料已刪除', 'success');
    });
  });

  // 3. 編輯物料基本資料（名稱與類別）
  container.querySelectorAll('.btn-edit-mat').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const idx = parseInt(e.currentTarget.dataset.index);
      const mat = materials[idx];
      const selectedCats = Array.isArray(mat.categories) ? mat.categories : [];

      const content = `
        <div class="flex flex-col gap-4">
          <div class="flex flex-col gap-1.5">
            <label class="text-sm font-medium text-slate-700">物料名稱</label>
            <input type="text" id="edit-mat-name" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" value="${mat.name}">
          </div>
          <div class="flex flex-col gap-1.5">
            <label class="text-sm font-medium text-slate-700">適用類別 (可多選)</label>
            <div class="flex flex-wrap gap-2">
              ${categories.map(c => `
                <label class="flex items-center gap-1.5 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
                  <input type="checkbox" class="edit-mat-cat-cb w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${c}" ${selectedCats.includes(c) ? 'checked' : ''}>
                  <span class="text-slate-700">${c}</span>
                </label>
              `).join('')}
            </div>
          </div>
        </div>
      `;

      const footer = `
        <div class="flex justify-end gap-3">
          <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
          <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">儲存</button>
        </div>
      `;

      const { close, element } = openModal({ title: '編輯物料', content, footer });

      element.querySelector('#modal-cancel-btn').addEventListener('click', close);

      element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
        const newName = element.querySelector('#edit-mat-name').value.trim();
        if (!newName) {
          showToast('名稱不能為空', 'error');
          return;
        }
        const newCats = Array.from(element.querySelectorAll('.edit-mat-cat-cb:checked')).map(cb => cb.value);

        materials[idx].name = newName;
        materials[idx].categories = newCats;

        await saveSettings({ materials });
        showToast('物料已更新', 'success');
        close();
      });
    });
  });

  // 4. 填寫物料詳細資料 (點擊名稱)
  container.querySelectorAll('.btn-edit-mat-detail').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const idx = parseInt(e.currentTarget.dataset.index);
      const mat = materials[idx];
      const matSchema = currentSettings.material_schema || [];
      
      if (matSchema.length === 0) {
        showToast('尚未設定物料自訂欄位，請先點擊右上角「自訂物料欄位」', 'info');
        return;
      }

      const content = `
        <div class="flex flex-col gap-4">
          <p class="text-sm text-slate-500 mb-2">正在編輯：<strong class="text-slate-800">${mat.name}</strong></p>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            ${matSchema.map(f => {
              const val = mat.data ? mat.data[f.key] : '';
              const inputType = f.type === 'number' ? 'number' : 'text';
              return `
                <div class="flex flex-col gap-1.5">
                  <label class="text-sm font-medium text-slate-700">${f.label}</label>
                  <input type="${inputType}" class="mat-detail-input w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" 
                    data-key="${f.key}" value="${val || ''}" placeholder="請輸入${f.label}">
                </div>
              `;
            }).join('')}
          </div>
        </div>
      `;

      const footer = `
        <div class="flex justify-end gap-3">
          <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
          <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">儲存詳細資料</button>
        </div>
      `;

      const { close, element } = openModal({ title: '物料詳細資料', content, footer });

      element.querySelector('#modal-cancel-btn').addEventListener('click', close);

      element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
        if (!materials[idx].data) materials[idx].data = {};
        
        element.querySelectorAll('.mat-detail-input').forEach(input => {
          const key = input.dataset.key;
          materials[idx].data[key] = input.value;
        });

        await saveSettings({ materials });
        showToast('詳細資料已儲存', 'success');
        close();
      });
    });
  });

  // 5. 自訂物料欄位管理 (Schema)
  document.getElementById('btn-manage-mat-schema')?.addEventListener('click', () => {
    const matSchema = currentSettings.material_schema || [];

    const content = `
      <div class="flex flex-col gap-4">
        <div class="flex gap-2">
          <input type="text" id="new-schema-label" class="flex-1 px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none" placeholder="新增欄位名稱 (例如：單價)">
          <select id="new-schema-type" class="w-24 px-2 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none">
            <option value="string">文字</option>
            <option value="number">數字</option>
          </select>
          <button id="btn-add-schema" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium whitespace-nowrap">新增</button>
        </div>

        <div class="flex flex-col gap-2 mt-4" id="schema-list">
          ${matSchema.map((f, i) => `
            <div class="flex items-center justify-between bg-white p-3 rounded-lg border border-slate-200">
              <div class="flex flex-col">
                <span class="font-bold text-slate-700">${f.label}</span>
                <span class="text-xs text-slate-400 font-mono">${f.key} (${f.type === 'number' ? '數字' : '文字'})</span>
              </div>
              <button class="p-1.5 text-slate-400 hover:text-rose-500 btn-del-schema" data-key="${f.key}">
                <i data-lucide="trash-2" class="w-4 h-4"></i>
              </button>
            </div>
          `).join('')}
          ${matSchema.length === 0 ? '<div class="text-slate-400 text-sm text-center py-4">尚未建立自訂欄位</div>' : ''}
        </div>
      </div>
    `;

    const footer = `
      <div class="flex justify-end gap-3">
        <button id="modal-close-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">完成</button>
      </div>
    `;

    const { close, element } = openModal({ title: '自訂物料欄位', content, footer });

    element.querySelector('#modal-close-btn').addEventListener('click', close);

    // 新增 Schema
    element.querySelector('#btn-add-schema')?.addEventListener('click', async () => {
      const label = element.querySelector('#new-schema-label').value.trim();
      const type = element.querySelector('#new-schema-type').value;
      if (!label) return;

      const newSchema = Array.isArray(currentSettings.material_schema) ? [...currentSettings.material_schema] : [];
      newSchema.push({ key: 'mat_' + Date.now().toString(36), label, type });
      
      await saveSettings({ material_schema: newSchema });
      close(); // 關閉視窗，重新開啟以刷新列表 (簡單做法)
      showToast('欄位已新增，請重新點擊「自訂物料欄位」', 'success');
    });

    // 刪除 Schema
    element.querySelectorAll('.btn-del-schema').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        const key = e.currentTarget.dataset.key;
        const newSchema = (currentSettings.material_schema || []).filter(f => f.key !== key);
        
        await saveSettings({ material_schema: newSchema });
        close();
        showToast('欄位已刪除', 'success');
      });
    });
  });
}

/*
F: AI-WA-UPWO/src/pages/settings-material.js
V: 1793054000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/settings-material.js === -->

<!-- === FILE START: ./src/pages/settings-schema.js === -->

## 📄 ./src/pages/settings-schema.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { createIcons, Plus, Trash2, Edit2, LayoutDashboard, Table, PieChart, Layers, Users, GripVertical, X, Check, FileEdit, Download, Database } from 'lucide';

let activeViewPage = 'kanban';

/**
 * 渲染欄位管理頁籤
 */
export function renderSchema(container, currentSettings, saveSettings) {
  const sysFields = currentSettings.schema.filter(f => f.isSystem);
  const customFields = currentSettings.schema.filter(f => !f.isSystem);
  const dbs = currentSettings.custom_databases || [];

  // 生成資料庫選項
  const dbOptions = dbs.map(db => `<option value="${db.id}">${db.name}</option>`).join('');

  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">動態欄位管理</h2>
        <p class="text-sm text-slate-500 mt-1">定義系統欄位。可綁定關聯資料庫，自動生成下拉選單。</p>
      </div>
    </div>

    <div class="bg-slate-50 border border-slate-200 rounded-xl p-4 mb-6">
      <div class="text-sm font-semibold text-slate-700 mb-3">新增自訂欄位</div>
      <div class="flex flex-col sm:flex-row gap-2">
        <input type="text" id="new-field-label" class="flex-1 px-4 py-2.5 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="欄位名稱 (例如：外發廠商)">
        <select id="new-field-type" class="w-full sm:w-48 px-4 py-2.5 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none">
          <option value="string">文字輸入</option>
          <option value="number">數字輸入</option>
          <option value="date">日期輸入</option>
          <option value="relation">🔗 關聯資料庫 (下拉)</option>
        </select>
        <button id="btn-add-field" class="btn-primary px-4 py-2.5 text-sm flex items-center justify-center gap-2 whitespace-nowrap">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增
        </button>
      </div>
      
      <!-- 關聯資料庫選擇區塊 (預設隱藏) -->
      <div id="relation-target-wrapper" class="hidden mt-3 flex-col gap-1.5">
        <label class="text-sm font-medium text-slate-700">選擇要綁定的資料庫：</label>
        <select id="new-field-relation-db" class="w-full sm:w-64 px-4 py-2 bg-white border border-brand/50 rounded-xl text-sm focus:outline-none">
          ${dbs.length === 0 ? '<option value="">請先至資料庫中心建立資料庫</option>' : dbOptions}
        </select>
      </div>
    </div>

    <div class="mb-4 text-sm font-semibold text-slate-600">系統核心欄位</div>
    <div class="flex flex-col gap-2 mb-8">
      ${sysFields.map(f => `
        <div class="flex items-center gap-4 bg-slate-50 p-4 rounded-xl border border-slate-200">
          <div class="w-1/3 font-bold text-slate-700">${f.label}</div>
          <div class="w-1/3 text-slate-400 font-mono text-xs">${f.key}</div>
          <div class="w-1/3 flex justify-end items-center gap-4">
            <label class="flex items-center gap-2 cursor-pointer text-sm text-slate-600">
              <input type="checkbox" class="edit-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" data-key="${f.key}" ${f.isEditable ? 'checked' : ''}>
              允許行內編輯
            </label>
            <span class="text-xs text-slate-400">系統保護</span>
          </div>
        </div>
      `).join('')}
    </div>

    <div class="mb-4 text-sm font-semibold text-slate-600">自訂欄位</div>
    <div class="flex flex-col gap-2">
      ${customFields.length === 0 ? '<div class="text-slate-400 text-sm py-2">目前尚無自訂欄位</div>' : ''}
      ${customFields.map((f, index) => {
        const isRel = f.type === 'relation';
        const relDb = isRel ? dbs.find(d => d.id === f.db_id) : null;
        const typeLabel = isRel 
          ? `🔗 關聯：${relDb ? relDb.name : '未知資料庫'}` 
          : (f.type === 'date' ? '日期' : f.type === 'number' ? '數字' : '文字');

        return `
        <div class="flex items-center gap-4 bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
          <div class="w-1/3 font-bold text-slate-800 flex items-center gap-2">
            ${f.label}
            <span class="text-[10px] font-normal px-1.5 py-0.5 bg-slate-100 text-slate-500 rounded ${isRel ? 'text-brand bg-brand/10' : ''}">${typeLabel}</span>
          </div>
          <div class="w-1/3 flex items-center gap-4">
            <span class="text-slate-500 font-mono text-xs">${f.key}</span>
          </div>
          <div class="w-1/3 flex justify-end items-center gap-3">
            <label class="flex items-center gap-2 cursor-pointer text-sm text-slate-600 mr-2">
              <input type="checkbox" class="edit-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" data-key="${f.key}" ${f.isEditable ? 'checked' : ''}>
              允許編輯
            </label>
            <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-field" data-key="${f.key}" data-label="${f.label}">
              <i data-lucide="edit-2" class="w-4 h-4"></i>
            </button>
            <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-field" data-key="${f.key}">
              <i data-lucide="trash-2" class="w-4 h-4"></i>
            </button>
          </div>
        </div>
      `}).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, Trash2, Edit2, Database }, attrs: { class: 'w-4 h-4' } });

  // 切換欄位類型時，顯示/隱藏關聯資料庫選擇
  const typeSelect = document.getElementById('new-field-type');
  const relWrapper = document.getElementById('relation-target-wrapper');
  
  typeSelect?.addEventListener('change', (e) => {
    if (e.target.value === 'relation') {
      relWrapper.classList.remove('hidden');
      relWrapper.classList.add('flex');
    } else {
      relWrapper.classList.add('hidden');
      relWrapper.classList.remove('flex');
    }
  });

  document.getElementById('btn-add-field')?.addEventListener('click', async () => {
    const input = document.getElementById('new-field-label');
    const val = input.value.trim();
    const type = typeSelect.value;
    
    if (!val) return;
    
    let db_id = null;
    if (type === 'relation') {
      const dbSelect = document.getElementById('new-field-relation-db');
      db_id = dbSelect.value;
      if (!db_id) {
        showToast('請先建立並選擇關聯資料庫', 'error');
        return;
      }
    }
    
    const key = 'field_' + Date.now().toString(36);
    const newField = { key, label: val, type: type, isSystem: false, isEditable: true };
    if (db_id) newField.db_id = db_id;

    currentSettings.schema.push(newField);
    await saveSettings();
    showToast('新增成功，可至視圖配置勾選顯示', 'success');
  });

  // 行內編輯勾選
  document.querySelectorAll('.edit-checkbox').forEach(checkbox => {
    checkbox.addEventListener('change', async (e) => {
      const key = e.target.dataset.key;
      const isChecked = e.target.checked;
      
      const idx = currentSettings.schema.findIndex(f => f.key === key);
      if (idx !== -1) {
        currentSettings.schema[idx].isEditable = isChecked;
        await saveSettings();
        showToast(isChecked ? '已開啟行內編輯' : '已關閉行內編輯', 'success');
      }
    });
  });

  // 修改欄位名稱
  document.querySelectorAll('.btn-edit-field').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const key = e.currentTarget.dataset.key;
      const oldLabel = e.currentTarget.dataset.label;
      const newLabel = window.prompt(`修改欄位名稱（原為「${oldLabel}」）：`, oldLabel);
      if (!newLabel || newLabel.trim() === "" || newLabel === oldLabel) return;
      
      const idx = currentSettings.schema.findIndex(f => f.key === key);
      if (idx !== -1) {
        currentSettings.schema[idx].label = newLabel.trim();
        await saveSettings();
        showToast('修改成功', 'success');
      }
    });
  });

  // 刪除欄位
  document.querySelectorAll('.btn-del-field').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const key = e.currentTarget.dataset.key;
      const confirmed = await openConfirm({
        title: '刪除自訂欄位',
        message: `確定要刪除該欄位嗎？(歷史資料將保留，僅不再顯示)`,
        isDanger: true
      });
      if (!confirmed) return;
      
      currentSettings.schema = currentSettings.schema.filter(f => f.key !== key);
      Object.keys(currentSettings.view_configs).forEach(page => {
        currentSettings.view_configs[page] = currentSettings.view_configs[page].filter(k => k !== key);
      });
      
      await saveSettings();
      showToast('已刪除欄位', 'success');
    });
  });
}

/**
 * 渲染視圖配置頁籤（雙清單拖曳排序）
 */
export function renderViewConfig(container, currentSettings, saveSettings) {
  const pages = [
    { key: 'kanban', label: '工單進度表', icon: 'layout-dashboard' },
    { key: 'table', label: '管理總表', icon: 'table' },
    { key: 'overview', label: '總狀態視圖', icon: 'pie-chart' },
    { key: 'category', label: '類別視圖', icon: 'layers' },
    { key: 'salesperson', label: '銷售員視圖', icon: 'users' },
    { key: 'data-entry', label: '資料錄入表單', icon: 'file-edit' },
    { key: 'export', label: '匯出 Excel 欄位', icon: 'download' }
  ];

  if (!currentSettings.view_configs['export']) {
    currentSettings.view_configs['export'] = ['id', 'status', 'customerNameZh', 'amount', 'dueDate'];
  }

  const activeFields = currentSettings.view_configs[activeViewPage] || [];
  const visibleSchema = activeFields.map(k => currentSettings.schema.find(f => f.key === k)).filter(Boolean);
  const hiddenSchema = currentSettings.schema.filter(f => !activeFields.includes(f.key));

  container.innerHTML = `
    <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4">
      <div>
        <h2 class="text-lg font-bold text-slate-800">視圖配置與排序</h2>
        <p class="text-sm text-slate-500 mt-1">選擇頁面，並左右拖曳或點擊來決定顯示的欄位與順序。</p>
      </div>
      <select id="view-page-select" class="px-4 py-2 bg-slate-50 border border-slate-200 rounded-xl text-sm font-medium focus:outline-none">
        ${pages.map(p => `<option value="${p.key}" ${p.key === activeViewPage ? 'selected' : ''}>${p.label}</option>`).join('')}
      </select>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <!-- 已顯示欄位 -->
      <div class="bg-white border border-slate-200 rounded-2xl p-4 flex flex-col shadow-sm">
        <h3 class="font-bold text-slate-700 mb-4 flex items-center gap-2">
          <i data-lucide="check" class="w-4 h-4 text-emerald-500"></i> 已顯示欄位（可拖曳排序）
        </h3>
        <div class="flex flex-col gap-2 min-h-[200px]" id="visible-list">
          ${visibleSchema.length === 0 ? '<div class="text-slate-400 text-sm text-center py-8">無顯示欄位，請從右側加入</div>' : ''}
          ${visibleSchema.map(f => `
            <div class="flex items-center gap-3 bg-slate-50 p-3 rounded-xl border border-slate-200 cursor-grab visible-item" draggable="true" data-key="${f.key}">
              <i data-lucide="grip-vertical" class="w-5 h-5 text-slate-300"></i>
              <span class="flex-1 font-medium text-slate-700">${f.label} ${f.type === 'relation' ? '🔗' : ''}</span>
              <button class="p-1 text-slate-400 hover:text-rose-500 btn-remove-field" data-key="${f.key}">
                <i data-lucide="x" class="w-4 h-4"></i>
              </button>
            </div>
          `).join('')}
        </div>
      </div>

      <!-- 未顯示欄位 -->
      <div class="bg-slate-50 border border-slate-200/60 rounded-2xl p-4 flex flex-col">
        <h3 class="font-bold text-slate-600 mb-4 flex items-center gap-2">
          <i data-lucide="plus" class="w-4 h-4"></i> 未顯示欄位（點擊加入）
        </h3>
        <div class="flex flex-col gap-2">
          ${hiddenSchema.length === 0 ? '<div class="text-slate-400 text-sm text-center py-8">所有欄位已顯示</div>' : ''}
          ${hiddenSchema.map(f => `
            <div class="flex items-center gap-3 bg-white p-3 rounded-xl border border-slate-100 shadow-sm cursor-pointer hover:border-brand transition-colors hidden-item" data-key="${f.key}">
              <span class="flex-1 font-medium text-slate-600">${f.label} ${f.type === 'relation' ? '🔗' : ''}</span>
              <i data-lucide="plus" class="w-4 h-4 text-slate-400"></i>
            </div>
          `).join('')}
        </div>
      </div>
    </div>
  `;

  createIcons({ icons: { Check, Plus, GripVertical, X }, attrs: { class: 'w-4 h-4' } });

  document.getElementById('view-page-select')?.addEventListener('change', (e) => {
    activeViewPage = e.target.value;
    renderViewConfig(container, currentSettings, saveSettings);
  });

  container.querySelectorAll('.hidden-item').forEach(item => {
    item.addEventListener('click', async () => {
      const key = item.dataset.key;
      currentSettings.view_configs[activeViewPage].push(key);
      await saveSettings();
      showToast('已加入顯示', 'success');
    });
  });

  container.querySelectorAll('.btn-remove-field').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      e.stopPropagation();
      const key = btn.dataset.key;

      if (key === 'id') {
        showToast('「發票」為系統主鍵，不可從視圖中移除', 'error');
        return;
      }

      currentSettings.view_configs[activeViewPage] = currentSettings.view_configs[activeViewPage].filter(k => k !== key);
      await saveSettings();
      showToast('已從顯示移除', 'success');
    });
  });

  const list = document.getElementById('visible-list');
  if (list) {
    let draggedItem = null;

    list.querySelectorAll('.visible-item').forEach(item => {
      item.addEventListener('dragstart', function(e) {
        draggedItem = this;
        e.dataTransfer.effectAllowed = 'move';
        setTimeout(() => this.classList.add('opacity-50'), 0);
      });

      item.addEventListener('dragend', async function() {
        this.classList.remove('opacity-50');
        draggedItem = null;
        
        const newOrder = Array.from(list.querySelectorAll('.visible-item')).map(el => el.dataset.key);
        currentSettings.view_configs[activeViewPage] = newOrder;
        
        await saveSettings();
        showToast('順序已更新', 'success');
      });

      item.addEventListener('dragover', function(e) {
        e.preventDefault();
        if (this === draggedItem) return;
        const rect = this.getBoundingClientRect();
        const next = (e.clientY - rect.top) / (rect.bottom - rect.top) > 0.5;
        list.insertBefore(draggedItem, next ? this.nextSibling : this);
      });
    });
  }
}

/*
F: AI-WA-UPWO/src/pages/settings-schema.js
V: 1793082000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/settings-schema.js === -->

<!-- === FILE START: ./src/pages/settings-supplier.js === -->

## 📄 ./src/pages/settings-supplier.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm, openModal } from '../ui/modal.js';
import { createIcons, Plus, Trash2, Edit2, Settings2 } from 'lucide';

/**
 * 渲染外發供應商管理頁面
 * @param {HTMLElement} container 目標容器
 * @param {Object} currentSettings settings/main 的內容
 * @param {Function} saveSettings 儲存回呼（只更新特定欄位）
 */
export function renderSuppliers(container, currentSettings, saveSettings) {
  // 初始化資料結構
  if (!Array.isArray(currentSettings.suppliers)) currentSettings.suppliers = [];
  if (!Array.isArray(currentSettings.supplier_schema)) currentSettings.supplier_schema = [];

  const suppliers = currentSettings.suppliers;

  container.innerHTML = `
    <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4">
      <div>
        <h2 class="text-lg font-bold text-slate-800">外發供應商管理</h2>
        <p class="text-sm text-slate-500 mt-1">建立外發廠商名單，並可自訂專屬的聯絡資訊或備註欄位。</p>
      </div>
      <div class="flex gap-2">
        <button id="btn-manage-sup-schema" class="bg-white text-slate-700 border border-slate-200 px-4 py-2 rounded-xl text-sm font-medium hover:bg-slate-50 flex items-center gap-2 shadow-sm">
          <i data-lucide="settings-2" class="w-4 h-4"></i> 自訂供應商欄位
        </button>
      </div>
    </div>

    <div class="bg-slate-50 border border-slate-200 rounded-2xl p-4 mb-6 flex flex-col gap-3">
      <div class="text-sm font-semibold text-slate-700">新增供應商</div>
      <div class="flex flex-col sm:flex-row gap-2">
        <input type="text" id="new-sup-name" class="flex-1 px-4 py-2.5 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="供應商名稱 (例如：大華印刷廠)">
        <button id="btn-add-sup" class="btn-primary px-4 py-2.5 text-sm flex items-center justify-center gap-2 whitespace-nowrap">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增
        </button>
      </div>
    </div>

    <div class="flex flex-col gap-3">
      ${suppliers.length === 0 ? '<div class="text-slate-400 text-sm py-4 text-center">尚無供應商資料</div>' : ''}
      ${suppliers.map((sup, index) => `
        <div class="bg-white p-4 rounded-xl border border-slate-100 shadow-sm flex flex-col gap-2">
          <div class="flex items-center justify-between">
            <div class="flex items-center gap-2">
              <button class="font-bold text-brand hover:underline text-base btn-edit-sup-detail" data-index="${index}">${sup.name}</button>
            </div>
            <div class="flex items-center gap-2">
              <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-sup" data-index="${index}">
                <i data-lucide="edit-2" class="w-4 h-4"></i>
              </button>
              <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-sup" data-index="${index}">
                <i data-lucide="trash-2" class="w-4 h-4"></i>
              </button>
            </div>
          </div>
          <!-- 顯示自訂欄位的摘要 -->
          <div class="flex flex-wrap gap-4 text-sm text-slate-600 mt-1">
            ${(currentSettings.supplier_schema || []).map(field => {
              const val = sup.data ? sup.data[field.key] : '';
              if (!val) return '';
              return `<div><span class="text-slate-400 text-xs">${field.label}:</span> ${val}</div>`;
            }).join('')}
          </div>
        </div>
      `).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, Trash2, Edit2, Settings2 }, attrs: { class: 'w-4 h-4' } });

  // 1. 新增供應商
  document.getElementById('btn-add-sup')?.addEventListener('click', async () => {
    const nameInput = document.getElementById('new-sup-name');
    const name = nameInput.value.trim();
    if (!name) return;
    if (suppliers.some(s => s.name === name)) {
      showToast('供應商名稱已存在', 'error');
      return;
    }
    
    const newSup = { 
      id: 'sup_' + Date.now().toString(36), 
      name, 
      data: {}
    };
    
    suppliers.push(newSup);
    await saveSettings({ suppliers });
    showToast('供應商已新增', 'success');
  });

  // 2. 刪除供應商
  container.querySelectorAll('.btn-del-sup').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const idx = e.currentTarget.dataset.index;
      const confirmed = await openConfirm({
        title: '刪除供應商',
        message: `確定要刪除「${suppliers[idx].name}」嗎？此操作不影響已存在的工單。`,
        isDanger: true
      });
      if (!confirmed) return;
      suppliers.splice(idx, 1);
      await saveSettings({ suppliers });
      showToast('供應商已刪除', 'success');
    });
  });

  // 3. 編輯供應商基本資料（名稱）
  container.querySelectorAll('.btn-edit-sup').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const idx = parseInt(e.currentTarget.dataset.index);
      const sup = suppliers[idx];

      const content = `
        <div class="flex flex-col gap-4">
          <div class="flex flex-col gap-1.5">
            <label class="text-sm font-medium text-slate-700">供應商名稱</label>
            <input type="text" id="edit-sup-name" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" value="${sup.name}">
          </div>
        </div>
      `;

      const footer = `
        <div class="flex justify-end gap-3">
          <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
          <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">儲存</button>
        </div>
      `;

      const { close, element } = openModal({ title: '編輯供應商', content, footer });

      element.querySelector('#modal-cancel-btn').addEventListener('click', close);

      element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
        const newName = element.querySelector('#edit-sup-name').value.trim();
        if (!newName) {
          showToast('名稱不能為空', 'error');
          return;
        }
        
        suppliers[idx].name = newName;
        await saveSettings({ suppliers });
        showToast('供應商已更新', 'success');
        close();
      });
    });
  });

  // 4. 填寫供應商詳細資料 (點擊名稱)
  container.querySelectorAll('.btn-edit-sup-detail').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const idx = parseInt(e.currentTarget.dataset.index);
      const sup = suppliers[idx];
      const supSchema = currentSettings.supplier_schema || [];
      
      if (supSchema.length === 0) {
        showToast('尚未設定供應商自訂欄位，請先點擊右上角「自訂供應商欄位」', 'info');
        return;
      }

      const content = `
        <div class="flex flex-col gap-4">
          <p class="text-sm text-slate-500 mb-2">正在編輯：<strong class="text-slate-800">${sup.name}</strong></p>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            ${supSchema.map(f => {
              const val = sup.data ? sup.data[f.key] : '';
              const inputType = f.type === 'number' ? 'number' : 'text';
              return `
                <div class="flex flex-col gap-1.5">
                  <label class="text-sm font-medium text-slate-700">${f.label}</label>
                  <input type="${inputType}" class="sup-detail-input w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" 
                    data-key="${f.key}" value="${val || ''}" placeholder="請輸入${f.label}">
                </div>
              `;
            }).join('')}
          </div>
        </div>
      `;

      const footer = `
        <div class="flex justify-end gap-3">
          <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
          <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">儲存詳細資料</button>
        </div>
      `;

      const { close, element } = openModal({ title: '供應商詳細資料', content, footer });

      element.querySelector('#modal-cancel-btn').addEventListener('click', close);

      element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
        if (!suppliers[idx].data) suppliers[idx].data = {};
        
        element.querySelectorAll('.sup-detail-input').forEach(input => {
          const key = input.dataset.key;
          suppliers[idx].data[key] = input.value;
        });

        await saveSettings({ suppliers });
        showToast('詳細資料已儲存', 'success');
        close();
      });
    });
  });

  // 5. 自訂供應商欄位管理 (Schema)
  document.getElementById('btn-manage-sup-schema')?.addEventListener('click', () => {
    const supSchema = currentSettings.supplier_schema || [];

    const content = `
      <div class="flex flex-col gap-4">
        <div class="flex gap-2">
          <input type="text" id="new-sup-schema-label" class="flex-1 px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none" placeholder="新增欄位名稱 (例如：聯絡人)">
          <select id="new-sup-schema-type" class="w-24 px-2 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none">
            <option value="string">文字</option>
            <option value="number">數字</option>
          </select>
          <button id="btn-add-sup-schema" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium whitespace-nowrap">新增</button>
        </div>

        <div class="flex flex-col gap-2 mt-4" id="sup-schema-list">
          ${supSchema.map((f, i) => `
            <div class="flex items-center justify-between bg-white p-3 rounded-lg border border-slate-200">
              <div class="flex flex-col">
                <span class="font-bold text-slate-700">${f.label}</span>
                <span class="text-xs text-slate-400 font-mono">${f.key} (${f.type === 'number' ? '數字' : '文字'})</span>
              </div>
              <button class="p-1.5 text-slate-400 hover:text-rose-500 btn-del-sup-schema" data-key="${f.key}">
                <i data-lucide="trash-2" class="w-4 h-4"></i>
              </button>
            </div>
          `).join('')}
          ${supSchema.length === 0 ? '<div class="text-slate-400 text-sm text-center py-4">尚未建立自訂欄位</div>' : ''}
        </div>
      </div>
    `;

    const footer = `
      <div class="flex justify-end gap-3">
        <button id="modal-close-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">完成</button>
      </div>
    `;

    const { close, element } = openModal({ title: '自訂供應商欄位', content, footer });

    element.querySelector('#modal-close-btn').addEventListener('click', close);

    // 新增 Schema
    element.querySelector('#btn-add-sup-schema')?.addEventListener('click', async () => {
      const label = element.querySelector('#new-sup-schema-label').value.trim();
      const type = element.querySelector('#new-sup-schema-type').value;
      if (!label) return;

      const newSchema = Array.isArray(currentSettings.supplier_schema) ? [...currentSettings.supplier_schema] : [];
      newSchema.push({ key: 'sup_' + Date.now().toString(36), label, type });
      
      await saveSettings({ supplier_schema: newSchema });
      close(); // 關閉視窗，重新開啟以刷新列表
      showToast('欄位已新增，請重新點擊「自訂供應商欄位」', 'success');
    });

    // 刪除 Schema
    element.querySelectorAll('.btn-del-sup-schema').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        const key = e.currentTarget.dataset.key;
        const newSchema = (currentSettings.supplier_schema || []).filter(f => f.key !== key);
        
        await saveSettings({ supplier_schema: newSchema });
        close();
        showToast('欄位已刪除', 'success');
      });
    });
  });
}

/*
F: AI-WA-UPWO/src/pages/settings-supplier.js
V: 1793064000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/settings-supplier.js === -->

<!-- === FILE START: ./src/pages/settings-system.js === -->

## 📄 ./src/pages/settings-system.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm, openModal } from '../ui/modal.js';
import { getCollection } from '../db/read.js';
import { updateDocument, deleteDocument } from '../db/write.js';
import { createIcons, Plus, Trash2, Edit2, AlertCircle } from 'lucide';

/**
 * 渲染帳號管理頁面
 */
export function renderSystemUsers(container, currentSettings, saveSettings, allSalesReps = []) {
  const users = currentSettings.users || [];
  const areas = currentSettings.areas || [];

  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">帳號管理</h2>
        <p class="text-sm text-slate-500 mt-1">設定登入系統的帳號、角色與可見範圍。</p>
      </div>
    </div>

    <div class="bg-slate-50 border border-slate-200 rounded-2xl p-4 mb-6 flex flex-col gap-3">
      <div class="text-sm font-semibold text-slate-700">新增帳號</div>
      <div class="flex flex-col sm:flex-row gap-2">
        <input type="text" id="new-user-name" class="flex-1 px-4 py-2.5 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="帳號">
        <input type="text" id="new-user-pass" class="flex-1 px-4 py-2.5 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="密碼">
        <select id="new-user-role" class="w-full sm:w-40 px-4 py-2.5 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none font-medium">
          <option value="admin">管理員</option>
          <option value="worker">生產員</option>
          <option value="sales">銷售員</option>
          <option value="viewer">一般使用者</option>
        </select>
      </div>
      
      <div id="new-user-areas-wrapper" class="hidden flex-col gap-1.5">
        <label class="text-sm font-medium text-slate-600">可見區域（不勾選 = 全部區域）</label>
        <div class="flex flex-wrap gap-2">
          ${areas.length === 0 ? '<span class="text-xs text-slate-400">尚未建立任何區域</span>' : ''}
          ${areas.map(a => `
            <label class="flex items-center gap-1.5 px-3 py-1.5 bg-white border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
              <input type="checkbox" class="new-user-area-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${a.id}">
              <span class="text-slate-700">${a.name}</span>
            </label>
          `).join('')}
        </div>
      </div>

      <div id="new-user-sales-wrapper" class="hidden flex-col gap-1.5">
        <label class="text-sm font-medium text-slate-600">可見銷售員（不勾選 = 全部銷售員）</label>
        <div class="flex flex-wrap gap-2">
          ${allSalesReps.length === 0 ? '<span class="text-xs text-slate-400">目前無銷售員資料</span>' : ''}
          ${allSalesReps.map(s => `
            <label class="flex items-center gap-1.5 px-3 py-1.5 bg-white border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
              <input type="checkbox" class="new-user-sales-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${s}">
              <span class="text-slate-700">${s}</span>
            </label>
          `).join('')}
        </div>
      </div>

      <div class="flex justify-end mt-2">
        <button id="btn-add-user" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增帳號
        </button>
      </div>
    </div>

    <div class="flex flex-col gap-2">
      ${users.map((u, index) => {
        const uAreas = Array.isArray(u.allowedAreas) ? u.allowedAreas : [];
        const uSales = Array.isArray(u.allowedSales) ? u.allowedSales : [];
        
        const areaLabels = uAreas.map(id => {
          const a = areas.find(x => x.id === id);
          return a ? a.name : id;
        });

        let roleText = '一般使用者';
        let roleColor = 'bg-slate-100 text-slate-600';
        if (u.role === 'admin') { roleText = '管理員'; roleColor = 'bg-brand/10 text-brand'; }
        if (u.role === 'worker') { roleText = '生產員'; roleColor = 'bg-orange-100 text-orange-600'; }
        if (u.role === 'sales') { roleText = '銷售員'; roleColor = 'bg-emerald-100 text-emerald-600'; }

        return `
          <div class="flex flex-col md:flex-row md:items-center gap-4 bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
            <div class="w-full md:w-1/4 font-bold text-slate-800">${u.username}</div>
            <div class="w-full md:w-1/4 text-slate-500 font-mono text-sm">••••••••</div>
            <div class="w-full md:w-1/6">
              <span class="px-2 py-1 text-xs font-medium rounded-md ${roleColor}">${roleText}</span>
            </div>
            <div class="flex-1 flex flex-wrap gap-1">
              ${u.role === 'admin' || u.role === 'viewer'
                ? '<span class="text-xs text-slate-400">不受限制</span>'
                : (u.role === 'worker' 
                    ? (uAreas.length === 0 ? '<span class="text-xs text-slate-400">全區可見</span>' : areaLabels.map(l => `<span class="px-2 py-0.5 bg-slate-100 text-slate-600 rounded text-xs">${l}</span>`).join(''))
                    : (uSales.length === 0 ? '<span class="text-xs text-slate-400">全銷售員可見</span>' : uSales.map(s => `<span class="px-2 py-0.5 bg-slate-100 text-slate-600 rounded text-xs">${s}</span>`).join(''))
                  )
              }
            </div>
            <div class="flex items-center gap-1 justify-end">
              <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-user" data-index="${index}">
                <i data-lucide="edit-2" class="w-4 h-4"></i>
              </button>
              <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-user" data-index="${index}" ${u.username === 'admin' ? 'disabled title="無法刪除預設 admin"' : ''}>
                <i data-lucide="trash-2" class="w-4 h-4"></i>
              </button>
            </div>
          </div>
        `;
      }).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, Trash2, Edit2 }, attrs: { class: 'w-4 h-4' } });

  // 新增角色連動顯示
  document.getElementById('new-user-role')?.addEventListener('change', (e) => {
    const val = e.target.value;
    const areasWrap = document.getElementById('new-user-areas-wrapper');
    const salesWrap = document.getElementById('new-user-sales-wrapper');
    
    if (val === 'worker') {
      areasWrap.classList.remove('hidden'); areasWrap.classList.add('flex');
      salesWrap.classList.add('hidden'); salesWrap.classList.remove('flex');
    } else if (val === 'sales') {
      salesWrap.classList.remove('hidden'); salesWrap.classList.add('flex');
      areasWrap.classList.add('hidden'); areasWrap.classList.remove('flex');
    } else {
      areasWrap.classList.add('hidden'); areasWrap.classList.remove('flex');
      salesWrap.classList.add('hidden'); salesWrap.classList.remove('flex');
    }
  });

  // 新增使用者
  document.getElementById('btn-add-user')?.addEventListener('click', async () => {
    const name = document.getElementById('new-user-name').value.trim();
    const pass = document.getElementById('new-user-pass').value.trim();
    const role = document.getElementById('new-user-role').value;

    if (!name || !pass) {
      showToast('請填寫帳號與密碼', 'error');
      return;
    }
    if (users.some(u => u.username === name)) {
      showToast('帳號已存在', 'error');
      return;
    }

    let allowedAreas = [];
    let allowedSales = [];
    
    if (role === 'worker') {
      allowedAreas = Array.from(document.querySelectorAll('.new-user-area-checkbox:checked')).map(cb => cb.value);
    } else if (role === 'sales') {
      allowedSales = Array.from(document.querySelectorAll('.new-user-sales-checkbox:checked')).map(cb => cb.value);
    }

    users.push({ username: name, password: pass, role, allowedAreas, allowedSales });
    await saveSettings({ users });
    showToast('新增成功', 'success');
  });

  // 刪除使用者
  container.querySelectorAll('.btn-del-user').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      if (e.currentTarget.disabled) return;
      const idx = e.currentTarget.dataset.index;
      users.splice(idx, 1);
      await saveSettings({ users });
      showToast('刪除成功', 'success');
    });
  });

  // 編輯使用者
  container.querySelectorAll('.btn-edit-user').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const idx = parseInt(e.currentTarget.dataset.index);
      const user = users[idx];
      const userAreas = Array.isArray(user.allowedAreas) ? user.allowedAreas : [];
      const userSales = Array.isArray(user.allowedSales) ? user.allowedSales : [];

      const content = `
        <div class="flex flex-col gap-4">
          <div class="flex flex-col gap-1">
            <label class="text-sm font-medium text-slate-700">帳號</label>
            <input type="text" id="edit-user-name" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none" value="${user.username}" ${user.username === 'admin' ? 'disabled' : ''}>
          </div>
          <div class="flex flex-col gap-1">
            <label class="text-sm font-medium text-slate-700">密碼</label>
            <input type="text" id="edit-user-pass" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none" value="${user.password}">
          </div>
          <div class="flex flex-col gap-1">
            <label class="text-sm font-medium text-slate-700">角色</label>
            <select id="edit-user-role" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none font-medium" ${user.username === 'admin' ? 'disabled' : ''}>
              <option value="admin" ${user.role === 'admin' ? 'selected' : ''}>管理員</option>
              <option value="worker" ${user.role === 'worker' ? 'selected' : ''}>生產員</option>
              <option value="sales" ${user.role === 'sales' ? 'selected' : ''}>銷售員</option>
              <option value="viewer" ${user.role === 'viewer' ? 'selected' : ''}>一般使用者</option>
            </select>
          </div>
          
          <div id="edit-user-areas-wrapper" class="flex-col gap-1.5 ${user.role === 'worker' ? 'flex' : 'hidden'}">
            <label class="text-sm font-medium text-slate-700">可見區域（不勾選 = 全部區域）</label>
            <div class="flex flex-wrap gap-2">
              ${areas.length === 0 ? '<span class="text-xs text-slate-400">尚未建立任何區域</span>' : ''}
              ${areas.map(a => `
                <label class="flex items-center gap-1.5 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
                  <input type="checkbox" class="edit-user-area-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${a.id}" ${userAreas.includes(a.id) ? 'checked' : ''}>
                  <span class="text-slate-700">${a.name}</span>
                </label>
              `).join('')}
            </div>
          </div>

          <div id="edit-user-sales-wrapper" class="flex-col gap-1.5 ${user.role === 'sales' ? 'flex' : 'hidden'}">
            <label class="text-sm font-medium text-slate-700">可見銷售員（不勾選 = 全部銷售員）</label>
            <div class="flex flex-wrap gap-2">
              ${allSalesReps.length === 0 ? '<span class="text-xs text-slate-400">目前無銷售員資料</span>' : ''}
              ${allSalesReps.map(s => `
                <label class="flex items-center gap-1.5 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
                  <input type="checkbox" class="edit-user-sales-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${s}" ${userSales.includes(s) ? 'checked' : ''}>
                  <span class="text-slate-700">${s}</span>
                </label>
              `).join('')}
            </div>
          </div>
        </div>
      `;

      const footer = `
        <div class="flex justify-end gap-3">
          <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
          <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">儲存</button>
        </div>
      `;

      const { close, element } = openModal({ title: '編輯帳號', content, footer });

      element.querySelector('#edit-user-role')?.addEventListener('change', (ev) => {
        const val = ev.target.value;
        const areasWrap = element.querySelector('#edit-user-areas-wrapper');
        const salesWrap = element.querySelector('#edit-user-sales-wrapper');
        
        if (val === 'worker') {
          areasWrap.classList.remove('hidden'); areasWrap.classList.add('flex');
          salesWrap.classList.add('hidden'); salesWrap.classList.remove('flex');
        } else if (val === 'sales') {
          salesWrap.classList.remove('hidden'); salesWrap.classList.add('flex');
          areasWrap.classList.add('hidden'); areasWrap.classList.remove('flex');
        } else {
          areasWrap.classList.add('hidden'); areasWrap.classList.remove('flex');
          salesWrap.classList.add('hidden'); salesWrap.classList.remove('flex');
        }
      });

      element.querySelector('#modal-cancel-btn').addEventListener('click', close);

      element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
        const newName = element.querySelector('#edit-user-name').value.trim();
        const newPass = element.querySelector('#edit-user-pass').value.trim();
        const newRole = element.querySelector('#edit-user-role').value;

        if (!newName || !newPass) {
          showToast('帳號與密碼不能為空', 'error');
          return;
        }
        if (newName !== user.username && users.some(u => u.username === newName)) {
          showToast('帳號已存在', 'error');
          return;
        }

        let newAllowedAreas = [];
        let newAllowedSales = [];
        
        if (newRole === 'worker') {
          newAllowedAreas = Array.from(element.querySelectorAll('.edit-user-area-checkbox:checked')).map(cb => cb.value);
        } else if (newRole === 'sales') {
          newAllowedSales = Array.from(element.querySelectorAll('.edit-user-sales-checkbox:checked')).map(cb => cb.value);
        }

        users[idx] = {
          username: newName,
          password: newPass,
          role: newRole,
          allowedAreas: newAllowedAreas,
          allowedSales: newAllowedSales
        };

        await saveSettings({ users });
        showToast('已更新帳號', 'success');
        close();
      });
    });
  });
}

/**
 * 渲染資料管理頁面
 */
export function renderSystemData(container, currentSettings, saveSettings) {
  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">資料管理</h2>
        <p class="text-sm text-slate-500 mt-1">清除由 Excel 匯入的資料，或徹底刪除所有工單資料。</p>
      </div>
    </div>

    <div class="flex flex-col gap-6">
      <!-- 清空 Excel 資料 -->
      <div class="bg-rose-50 border border-rose-200 rounded-2xl p-6">
        <h3 class="text-rose-700 font-bold mb-2 flex items-center gap-2">
          <i data-lucide="alert-circle" class="w-5 h-5"></i> 清空 Excel 匯入資料
        </h3>
        <p class="text-sm text-rose-600 mb-4">
          點擊下方按鈕將會清空所有工單中「由 Excel 匯入」的資料。清空後，您可以重新匯入新的 Excel，系統會自動合併狀態。
        </p>
        <button id="btn-clear-excel-data" class="bg-rose-500 hover:bg-rose-600 text-white px-5 py-2.5 rounded-xl text-sm font-medium transition-colors shadow-sm flex items-center gap-2">
          <i data-lucide="trash-2" class="w-4 h-4"></i> 清空 Excel 資料
        </button>
      </div>

      <!-- 徹底刪除所有工單 -->
      <div class="bg-rose-100 border border-rose-300 rounded-2xl p-6">
        <h3 class="text-rose-800 font-bold mb-2 flex items-center gap-2">
          <i data-lucide="alert-circle" class="w-5 h-5"></i> 徹底刪除所有工單
        </h3>
        <p class="text-sm text-rose-700 mb-4">
          <strong>⚠️ 危險操作：</strong>這將會永久刪除資料庫中的「所有」工單，包含所有狀態與自訂欄位，且無法復原。請確認您已經備份資料。
        </p>
        <button id="btn-delete-all-orders" class="bg-rose-600 hover:bg-rose-700 text-white px-5 py-2.5 rounded-xl text-sm font-medium transition-colors shadow-sm flex items-center gap-2">
          <i data-lucide="trash-2" class="w-4 h-4"></i> 徹底刪除所有工單
        </button>
      </div>
    </div>
  `;

  createIcons({ icons: { Trash2, AlertCircle }, attrs: { class: 'w-4 h-4' } });

  document.getElementById('btn-clear-excel-data')?.addEventListener('click', async () => {
    const confirmed = await openConfirm({
      title: '清空 Excel 資料',
      message: '確定要清空嗎？此操作無法復原，但您手動填寫的狀態與交期會保留。',
      isDanger: true,
      confirmText: '確認清空'
    });
    if (!confirmed) return;

    showToast('正在清空資料', 'info');
    try {
      const orders = await getCollection('work_orders');
      let count = 0;
      for (const order of orders) {
        await updateDocument('work_orders', order.id, {
          salesReps: ["未填寫"],
          uploadDate: "",
          data: { category: order.data?.category || '' }
        });
        count++;
      }
      showToast(`成功清空 ${count} 筆工單的 Excel 資料`, 'success');
    } catch (error) {
      showToast('清空失敗，請稍後再試', 'error');
    }
  });

  document.getElementById('btn-delete-all-orders')?.addEventListener('click', async () => {
    const confirmed = await openConfirm({
      title: '徹底刪除所有工單',
      message: '這將會永久刪除資料庫中的「所有」工單資料，且無法復原。您確定要繼續嗎？',
      isDanger: true,
      confirmText: '我確定，徹底刪除'
    });
    if (!confirmed) return;

    showToast('正在刪除所有工單', 'info');
    try {
      const orders = await getCollection('work_orders');
      let count = 0;
      for (const order of orders) {
        await deleteDocument('work_orders', order.id);
        count++;
      }
      showToast(`成功刪除 ${count} 筆工單資料`, 'success');
    } catch (error) {
      console.error('Delete all data error:', error);
      showToast('刪除失敗，請查看 Console', 'error');
    }
  });
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/settings-system.js
V: 1794187200
W: UPGRADE-009
═══ */```

<!-- === FILE END: ./src/pages/settings-system.js === -->

<!-- === FILE START: ./src/pages/settings.js === -->

## 📄 ./src/pages/settings.js

```js
import { AppState } from '../config/state.js';
import { listenDocument, getDocument, getCollection } from '../db/read.js';
import { updateDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { createIcons, Tag, Users, Shield, FileSpreadsheet, List, Database, LayoutDashboard, Layers, MapPin, Activity, GitBranch, Truck } from 'lucide';

// 引入拆分出去的模組
import { renderStatuses, renderRouteTemplates } from './settings-basic.js';
import { renderAreas } from './settings-area.js';
import { renderCategories } from './settings-category.js';
import { renderAreaStatuses } from './settings-area-status.js';

let unsubscribe = null;
export let currentSettings = { statuses: [], sales_mapping: [], users: [], route_templates: [], areas: [], categories: [], areaStatuses: [], suppliers: [], supplier_schema: [] };
let activeTab = 'statuses';

export default {
  async render() {
    const user = AppState.getUser();
    if (!user || user.role !== 'admin') {
      document.getElementById('app-content').innerHTML = '<div class="p-8 text-center text-rose-500 font-bold">您沒有權限訪問此頁面</div>';
      return;
    }

    renderPageLayout({
      title: '生產管理設定',
      subtitle: '管理工序、類別、製程路線圖與區域',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'settings-container'
    });

    this.renderTabs();
    this.initDataListener();
  },

  initDataListener() {
    unsubscribe = listenDocument('settings', 'main', async (data) => {
      let needSave = false;
      if (data) {
        currentSettings = Object.assign({}, data, {
          statuses: data.statuses || ['排產中'],
          sales_mapping: data.sales_mapping || [],
          users: data.users || [{ username: 'admin', password: '123456', role: 'admin', allowedAreas: [], allowedSales: [] }],
          route_templates: data.route_templates || this.getDefaultRouteTemplates(data.statuses || ['排產中']),
          areas: Array.isArray(data.areas) ? data.areas : [],
          categories: Array.isArray(data.categories) ? data.categories : [],
          areaStatuses: Array.isArray(data.areaStatuses) ? data.areaStatuses : ['正常', '壞機', '休息中'],
          suppliers: Array.isArray(data.suppliers) ? data.suppliers : [],
          supplier_schema: Array.isArray(data.supplier_schema) ? data.supplier_schema : []
        });

        // 遷移：areas 不存在
        if (!Array.isArray(data.areas)) needSave = true;
        
        // 遷移：areaStatuses 不存在
        if (!Array.isArray(data.areaStatuses)) needSave = true;

        // 遷移：每個使用者補 allowedAreas, allowedSales 欄位，並升級舊角色
        if (Array.isArray(currentSettings.users)) {
          currentSettings.users.forEach(u => {
            if (!Array.isArray(u.allowedAreas)) {
              u.allowedAreas = [];
              needSave = true;
            }
            if (!Array.isArray(u.allowedSales)) {
              u.allowedSales = [];
              needSave = true;
            }
            if (u.role === 'user') {
              u.role = 'viewer';
              needSave = true;
            }
          });
        }

        // 遷移：如果 categories 是空的，掃描工單自動補齊
        if (!Array.isArray(data.categories) || data.categories.length === 0) {
          try {
            const orders = await getCollection('work_orders');
            const cats = new Set();
            orders.forEach(o => {
              let c = '';
              if (o.data && o.data.category) c = String(o.data.category).toUpperCase();
              else {
                const match = String(o.id).match(/^[a-zA-Z]+/);
                c = match ? match[0].toUpperCase() : '其他';
              }
              if (c && c !== '其他') cats.add(c);
            });
            if (cats.size > 0) {
              currentSettings.categories = Array.from(cats).sort();
              needSave = true;
            }
          } catch (e) {
            console.warn('自動掃描類別失敗', e);
          }
        }

        // 遷移：強制修正 uploadDate 的 Label 與屬性
        if (currentSettings.schema) {
          const uploadDateField = currentSettings.schema.find(f => f.key === 'uploadDate');
          if (uploadDateField) {
            if (uploadDateField.label !== '開單日期' || uploadDateField.isEditable !== false) {
              uploadDateField.label = '開單日期';
              uploadDateField.isEditable = false;
              needSave = true;
            }
          } else {
            currentSettings.schema.push({ key: 'uploadDate', label: '開單日期', type: 'date', isSystem: true, isEditable: false });
            needSave = true;
          }
        }

        // 資料遷移與修復：製程模板
        if (currentSettings.route_templates) {
          currentSettings.route_templates.forEach(t => {
            if (t.category === undefined) {
              t.category = '';
              needSave = true;
            }
            if ((t.category === '' || t.category === '通用') && (!t.steps || t.steps.length === 0)) {
              t.steps = Array.from(currentSettings.statuses);
              needSave = true;
            }
          });

          if (!currentSettings.route_templates.some(t => t.category === '' || t.category === '通用')) {
            currentSettings.route_templates.unshift({
              name: '預設常規路線',
              category: '',
              steps: Array.from(currentSettings.statuses)
            });
            needSave = true;
          }
        }

      } else {
        currentSettings = {
          statuses: ['排產中', '印刷', '裁切', '品檢', '出貨'],
          sales_mapping: [],
          users: [{ username: 'admin', password: '123456', role: 'admin', allowedAreas: [], allowedSales: [] }],
          route_templates: this.getDefaultRouteTemplates(),
          areas: [],
          categories: [],
          areaStatuses: ['正常', '壞機', '休息中'],
          suppliers: [],
          supplier_schema: []
        };
        needSave = true;
      }

      if (needSave) {
        // 只更新生產相關欄位，避免覆蓋其他設定（如表單、物料）
        await updateDocument('settings', 'main', {
          statuses: currentSettings.statuses,
          sales_mapping: currentSettings.sales_mapping,
          route_templates: currentSettings.route_templates,
          areas: currentSettings.areas,
          categories: currentSettings.categories,
          areaStatuses: currentSettings.areaStatuses,
          suppliers: currentSettings.suppliers,
          supplier_schema: currentSettings.supplier_schema
        });
      }

      AppState.setStatuses(currentSettings.statuses);
      this.renderTabs();
    });
  },

  getDefaultRouteTemplates(statuses = ['排產中', '印刷', '裁切', '品檢', '出貨']) {
    return [
      { name: '預設常規路線', category: '', steps: Array.from(statuses) }
    ];
  },

  renderTabs() {
    const container = document.getElementById('settings-container');
    if (!container) return;

    container.innerHTML = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden min-h-[500px] flex flex-col md:flex-row">
        
        <div class="w-full md:w-64 border-r border-slate-100 bg-slate-50/50 p-4 flex flex-col gap-1 flex-shrink-0 overflow-y-auto">
          <button class="tab-btn ${activeTab === 'statuses' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="statuses">
            <i data-lucide="tag" class="w-4 h-4"></i> 狀態管理
          </button>
          <button class="tab-btn ${activeTab === 'categories' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="categories">
            <i data-lucide="layers" class="w-4 h-4"></i> 類別管理
          </button>
          <button class="tab-btn ${activeTab === 'route_templates' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="route_templates">
            <i data-lucide="git-branch" class="w-4 h-4"></i> 製程路線圖
          </button>
          <button class="tab-btn ${activeTab === 'areas' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="areas">
            <i data-lucide="map-pin" class="w-4 h-4"></i> 區域管理
          </button>
          <button class="tab-btn ${activeTab === 'area_statuses' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="area_statuses">
            <i data-lucide="activity" class="w-4 h-4"></i> 區域狀態
          </button>
        </div>

        <div class="flex-1 p-6 md:p-8" id="settings-content"></div>
      </div>
    `;

    createIcons({
      icons: { Tag, Users, Layers, MapPin, Activity, GitBranch },
      attrs: { class: 'w-4 h-4' }
    });

    this.bindTabEvents();
    this.renderContent();
  },

  bindTabEvents() {
    document.querySelectorAll('.tab-btn').forEach(btn => {
      btn.addEventListener('click', (e) => {
        activeTab = e.currentTarget.dataset.tab;
        this.renderTabs();
      });
    });
  },

  renderContent() {
    const content = document.getElementById('settings-content');
    if (!content) return;

    if (activeTab === 'statuses') {
      renderStatuses(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'categories') {
      renderCategories(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'route_templates') {
      renderRouteTemplates(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'areas') {
      renderAreas(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'area_statuses') {
      renderAreaStatuses(content, currentSettings, this.saveSettings.bind(this));
    }
  },

  async saveSettings() {
    const dataToSave = {
      statuses: currentSettings.statuses,
      sales_mapping: currentSettings.sales_mapping,
      route_templates: currentSettings.route_templates,
      areas: currentSettings.areas,
      categories: currentSettings.categories,
      areaStatuses: currentSettings.areaStatuses
    };
    await updateDocument('settings', 'main', dataToSave);
    this.renderTabs();
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
  }
};

/*
F: AI-WA-UPWO/src/pages/settings.js
V: 1793088000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/pages/settings.js === -->

<!-- === FILE START: ./src/pages/table.js === -->

## 📄 ./src/pages/table.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument } from '../db/read.js';
import { deleteDocument, updateDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { exportToExcel, exportToPDF } from '../lib/io.js';
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { renderOrderView } from '../ui/order-view.js';
import { handleExcelUpload, downloadLatestExcel, onSyncStateChange, clearBinding } from '../engine/sync-engine.js';
import { createIcons, FileText, Upload, Save, X } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let schema = [];
let viewConfig = {};
let route_templates = [];
let currentCategory = 'ALL';
let currentSettings = null;

export default {
  async render() {
    if (!AppState.getUser()) return;

    try {
      currentSettings = await getDocument('settings', 'main');
      if (currentSettings) {
        schema = currentSettings.schema || [];
        viewConfig = currentSettings.view_configs || {};
        route_templates = currentSettings.route_templates || [];
      }
    } catch (err) {}

    renderPageLayout({
      title: '管理總表',
      subtitle: '所有工單的狀態、時間與流程追蹤列表',
      headerActions: `
        <div class="flex flex-wrap items-center gap-2">
          <div id="upload-wrapper" class="flex items-center border border-brand/30 rounded-[10px] overflow-hidden shadow-sm">
            <button id="btn-upload-excel" class="bg-white text-brand px-3 py-2 text-sm font-medium hover:bg-brand/5 flex items-center gap-2 transition-colors">
              <i data-lucide="upload" class="w-4 h-4"></i> 上傳 Excel
            </button>
            <button id="btn-unbind-excel" class="bg-slate-50 text-slate-400 hover:text-rose-500 hover:bg-rose-50 px-2 py-2 border-l border-brand/20 hidden transition-colors" title="解除綁定">
              <i data-lucide="x" class="w-4 h-4"></i>
            </button>
          </div>
          <input type="file" id="excel-upload-input" accept=".xlsx, .xls" class="hidden">
          
          <button id="btn-download-excel" class="bg-brand text-white border border-brand px-3 py-2 rounded-[10px] text-sm font-medium hover:bg-brand-hover flex items-center gap-2 shadow-sm transition-colors opacity-50 cursor-not-allowed" disabled>
            <i data-lucide="save" class="w-4 h-4"></i> 下載最新進度
          </button>
          
          <button id="btn-export-pdf" class="bg-white text-slate-700 border border-slate-200 px-3 py-2 rounded-[10px] text-sm font-medium hover:bg-slate-50 flex items-center gap-2 shadow-sm">
            <i data-lucide="file-text" class="w-4 h-4"></i> PDF
          </button>
        </div>
      `,
      controls: `
        <div class="flex items-center gap-2 mb-4">
          <span class="text-sm font-medium text-slate-600">類別：</span>
          <select id="filter-category" class="px-3 py-2 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none shadow-sm min-w-[120px]"></select>
        </div>
      `,
      listContainerId: 'table-container'
    });

    document.getElementById('btn-export-pdf')?.addEventListener('click', () => this.handleExport('pdf'));
    
    document.getElementById('filter-category')?.addEventListener('change', (e) => {
      currentCategory = e.target.value;
      this.updateUI();
    });

    document.getElementById('btn-upload-excel')?.addEventListener('click', () => {
      document.getElementById('excel-upload-input').click();
    });

    document.getElementById('btn-unbind-excel')?.addEventListener('click', () => {
      clearBinding();
    });

    document.getElementById('excel-upload-input')?.addEventListener('change', (e) => {
      const file = e.target.files[0];
      if (file) {
        handleExcelUpload(file);
      }
      e.target.value = ''; 
    });

    document.getElementById('btn-download-excel')?.addEventListener('click', () => {
      downloadLatestExcel();
    });

    onSyncStateChange((state) => {
      const uploadBtn = document.getElementById('btn-upload-excel');
      const unbindBtn = document.getElementById('btn-unbind-excel');
      const downloadBtn = document.getElementById('btn-download-excel');
      const uploadWrapper = document.getElementById('upload-wrapper');
      
      if (uploadBtn && unbindBtn && uploadWrapper) {
        if (state.isBound) {
          uploadBtn.innerHTML = `<i data-lucide="upload" class="w-4 h-4"></i> 已載入：${state.fileName}`;
          uploadBtn.classList.add('bg-brand/5');
          unbindBtn.classList.remove('hidden');
          uploadWrapper.classList.add('bg-brand/5');
        } else {
          uploadBtn.innerHTML = `<i data-lucide="upload" class="w-4 h-4"></i> 上傳 Excel`;
          uploadBtn.classList.remove('bg-brand/5');
          unbindBtn.classList.add('hidden');
          uploadWrapper.classList.remove('bg-brand/5');
        }
      }

      if (downloadBtn) {
        if (state.isBound) {
          downloadBtn.disabled = false;
          downloadBtn.classList.remove('opacity-50', 'cursor-not-allowed');
        } else {
          downloadBtn.disabled = true;
          downloadBtn.classList.add('opacity-50', 'cursor-not-allowed');
        }
      }

      try {
        createIcons({ icons: { Upload, Save, X }, attrs: { class: 'w-4 h-4' } });
      } catch (e) {
        console.warn('Icon render error:', e);
      }
    });

    window.addEventListener('refresh-table', this.updateUI.bind(this));
    
    window.addEventListener('settings-updated', async () => {
      try {
        const settingsDoc = await getDocument('settings', 'main');
        if (settingsDoc) {
          currentSettings = settingsDoc;
          schema = settingsDoc.schema || [];
          viewConfig = settingsDoc.view_configs || {};
          route_templates = settingsDoc.route_templates || [];
          this.updateUI();
        }
      } catch (err) {}
    });

    try {
      createIcons({ icons: { FileText, Upload, Save, X }, attrs: { class: 'w-4 h-4' } });
    } catch (e) {
      console.warn('Icon render error:', e);
    }

    this.initDataListener();
  },

  initDataListener() {
    if (unsubscribe) unsubscribe();
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.updateUI();
    });
  },

  updateUI() {
    const catSelect = document.getElementById('filter-category');
    if (catSelect) {
      const cats = Array.from(new Set(currentOrders.map(o => o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他'))).sort();
      catSelect.innerHTML = `<option value="ALL">全部類別</option>` + cats.map(c => `<option value="${c}" ${c === currentCategory ? 'selected' : ''}>${c}</option>`).join('');
    }

    let filtered = currentOrders;
    if (currentCategory !== 'ALL') {
      filtered = filtered.filter(o => (o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他') === currentCategory);
    }

    let visibleFields = (viewConfig['table'] || []).map(key => {
      const field = schema.find(f => f.key === key);
      return field ? Object.assign({}, field, { isVisible: true }) : null;
    }).filter(Boolean);

    if (!visibleFields.some(f => f.key === 'route_select')) {
      visibleFields.unshift({ key: 'route_select', label: '製程路線圖', isVisible: true });
    }

    renderOrderView({
      containerId: 'table-container',
      orders: filtered,
      visibleFields: visibleFields,
      isAdmin: AppState.getUser()?.role === 'admin',
      pageKey: 'table',
      route_templates: route_templates,
      onAction: async (action, id, newStatus) => {
        if (action === 'delete') {
          if (await openConfirm({ title: '刪除', message: `確定刪除 ${id}？`, isDanger: true })) {
            await deleteDocument('work_orders', id);
            showToast('已刪除', 'success');
          }
        } else if (action === 'status_change') {
          if (newStatus) {
            await updateDocument('work_orders', id, { status: newStatus, statusUpdateDate: new Date().toISOString() });
            showToast('已更新生產進度', 'success');
          }
        }
      }
    });
  },

  handleExport(type) {
    if (currentOrders.length === 0) return showToast('無資料可匯出', 'info');
    const statuses = AppState.getStatuses() || [];
    try {
      if (type === 'excel') exportToExcel(currentOrders, statuses, '工單總表', schema, viewConfig['export']);
      else exportToPDF(currentOrders, statuses, '工單總表', schema);
      showToast('匯出成功', 'success');
    } catch (err) {
      console.error(err);
      showToast('匯出失敗', 'error');
    }
  },

  destroy() {
    if (unsubscribe) unsubscribe();
    window.removeEventListener('refresh-table', this.updateUI.bind(this));
    window.removeEventListener('settings-updated', this.updateUI.bind(this));
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/table.js
V: 1794188200
W: UPGRADE-009
═══ */```

<!-- === FILE END: ./src/pages/table.js === -->

<!-- === FILE START: ./src/style.css === -->

## 📄 ./src/style.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  /* 實體感卡片 */
  .glass-card {
    @apply bg-white rounded-[20px] border border-black/[0.04] shadow-soft transition-all duration-300;
  }
  
  .glass-card:hover {
    @apply -translate-y-0.5 shadow-lg;
  }

  /* 內凹區塊 */
  .inset-panel {
    @apply bg-[#F8FAFC] rounded-xl shadow-inset-soft border border-black/[0.02];
  }

  /* 主按鈕 */
  .btn-primary {
    @apply bg-brand text-white font-medium rounded-[10px] shadow-md shadow-brand/20 transition-all duration-150;
  }
  
  .btn-primary:hover {
    @apply bg-brand-hover;
  }
  
  .btn-primary:active {
    @apply translate-y-0.5 scale-[0.98] shadow-sm;
  }

  /* 仿 iOS Segmented Control */
  .segmented-control {
    @apply bg-slate-200 p-1 rounded-xl inline-flex relative;
  }
  
  .segment-btn {
    @apply px-4 py-1.5 rounded-lg text-sm font-medium text-slate-500 transition-all duration-300 z-10 cursor-pointer select-none;
  }
  
  .segment-btn.active {
    @apply text-slate-900 bg-white shadow-sm;
  }
}

/* 隱藏滾動條 (Kanban 區域) */
.kanban-scroll::-webkit-scrollbar {
  display: none;
}

.kanban-scroll {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

/* 基礎全域設定 */
body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ═══ EOF ═══
F: src/style.css
V: 1792903520
W: webapp-20261026-001#32
═══ */```

<!-- === FILE END: ./src/style.css === -->

<!-- === FILE START: ./src/ui/layout.js === -->

## 📄 ./src/ui/layout.js

```js
import { AppState } from '../config/state.js';
import { createIcons, Layers, Bell, LogOut, LayoutDashboard, Table, PieChart, Users, Settings, Activity, ClipboardList, Database, MapPin, Menu, FileEdit, Search, Sliders, Package, Shield } from 'lucide';

// 💡 更新：加入四大設定中心的選單與圖示
const ALL_ROUTES = {
  kanban: { label: '工單進度表', icon: 'layout-dashboard', roles: ['admin', 'viewer'] },
  search: { label: '全域搜尋', icon: 'search', roles: ['admin', 'viewer'] },
  table: { label: '管理總表', icon: 'table', roles: ['admin', 'viewer'] },
  overview: { label: '總狀態', icon: 'pie-chart', roles: ['admin', 'viewer'] },
  category: { label: '類別視圖', icon: 'layers', roles: ['admin', 'viewer'] },
  salesperson: { label: '銷售員', icon: 'users', roles: ['admin', 'viewer', 'sales'] },
  area: { label: '生產區域', icon: 'map-pin', roles: ['admin', 'viewer', 'worker'] },
  'settings-material': { label: '物料管理', icon: 'package', roles: ['admin'] },
  settings: { label: '生產管理設定', icon: 'settings', roles: ['admin'] },
  'settings-form': { label: '工單管理設定', icon: 'sliders', roles: ['admin'] },
  'settings-system': { label: '系統管理設定', icon: 'shield', roles: ['admin'] },
  'admin-data': { label: '資料總管', icon: 'database', roles: ['admin'] }
};

const DEFAULT_NAV_CONFIG = Object.keys(ALL_ROUTES).map(key => ({
  id: key,
  label: ALL_ROUTES[key].label,
  icon: ALL_ROUTES[key].icon,
  showSidebar: true,
  showTop: ['kanban', 'table', 'overview', 'category', 'salesperson'].includes(key),
  isEnabled: true
}));

export function renderAppShell({ user, onLogout, onNavigate }) {
  const root = document.getElementById('app-root');
  if (!root) return;

  const isCollapsed = localStorage.getItem('sidebar_collapsed') === 'true';

  root.innerHTML = `
    <div class="flex h-screen w-full bg-bg overflow-hidden">
      <!-- 側邊欄 (Desktop) -->
      <aside id="desktop-sidebar" class="w-64 bg-white border-r border-slate-200/50 flex-col hidden md:flex z-20 ${isCollapsed ? 'md:hidden' : ''}">
        <div class="h-16 flex items-center justify-between px-4 border-b border-slate-100 flex-shrink-0">
          <div class="flex items-center gap-2">
            <div class="w-8 h-8 rounded-lg bg-brand flex items-center justify-center text-white shadow-sm shadow-brand/30">
              <i data-lucide="layers" class="w-5 h-5"></i>
            </div>
            <span class="font-bold text-lg tracking-tight text-slate-900">UPGRADE-WO</span>
          </div>
          <button id="btn-collapse-sidebar" class="p-1.5 text-slate-400 hover:bg-slate-100 rounded-lg transition-colors" title="收起側邊欄">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-panel-left-close"><rect width="18" height="18" x="3" y="3" rx="2" ry="2"/><path d="M9 3v18"/><path d="m16 15-3-3 3-3"/></svg>
          </button>
        </div>
        
        <nav class="flex-1 py-4 px-3 flex flex-col gap-1 overflow-y-auto" id="sidebar-nav"></nav>

        <div class="p-4 border-t border-slate-100 flex-shrink-0">
          <button id="btn-logout" class="flex items-center gap-3 w-full px-3 py-2.5 rounded-xl text-sm font-medium text-slate-600 hover:bg-rose-50 hover:text-rose-600 transition-colors">
            <i data-lucide="log-out" class="w-5 h-5"></i>
            登出系統
          </button>
        </div>
      </aside>

      <!-- 主內容區 -->
      <div class="flex-1 flex flex-col h-full overflow-hidden relative">
        <header class="h-16 bg-white/80 backdrop-blur-md border-b border-slate-200/50 flex items-center justify-between px-4 sm:px-6 z-10 flex-shrink-0">
          <div class="flex items-center gap-3">
            <button id="btn-expand-sidebar" class="hidden md:block p-2 -ml-2 text-slate-600 hover:bg-slate-100 rounded-lg transition-colors ${isCollapsed ? '' : 'md:hidden'}" title="展開側邊欄">
              <i data-lucide="menu" class="w-5 h-5"></i>
            </button>

            <div class="flex items-center gap-3 md:hidden">
              <button id="btn-mobile-menu" class="p-2 -ml-2 text-slate-600 hover:bg-slate-100 rounded-lg transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-menu"><line x1="4" x2="20" y1="12" y2="12"/><line x1="4" x2="20" y1="6" y2="6"/><line x1="4" x2="20" y1="18" y2="18"/></svg>
              </button>
              <div class="w-8 h-8 rounded-lg bg-brand flex items-center justify-center text-white shadow-sm">
                <i data-lucide="layers" class="w-5 h-5"></i>
              </div>
              <span class="font-bold text-slate-900">UPGRADE</span>
            </div>
          </div>

          <div class="hidden md:flex segmented-control" id="top-segmented-control"></div>

          <div class="flex items-center gap-3">
            <div class="text-right hidden sm:block">
              <div class="text-sm font-bold text-slate-700">${user.username}</div>
              <div class="text-[10px] text-slate-400 uppercase tracking-wider">${user.role}</div>
            </div>
            <div class="w-9 h-9 rounded-full bg-gradient-to-tr from-indigo-100 to-purple-100 border border-indigo-200 flex items-center justify-center text-indigo-700 font-bold text-sm shadow-sm cursor-pointer">
              ${user.username.charAt(0).toUpperCase()}
            </div>
          </div>
        </header>

        <main id="app-content" class="flex-1 overflow-y-auto p-4 sm:p-6 lg:p-8 flex flex-col gap-6"></main>
      </div>
    </div>

    <!-- 手機版側邊選單 (Drawer) -->
    <div id="mobile-drawer" class="fixed inset-0 z-50 hidden">
      <div class="absolute inset-0 bg-black/40 backdrop-blur-sm transition-opacity" id="drawer-overlay"></div>
      <aside class="absolute left-0 top-0 bottom-0 w-64 bg-white shadow-2xl transform -translate-x-full transition-transform duration-300 flex flex-col" id="drawer-panel">
        <div class="h-16 flex items-center px-6 border-b border-slate-100 justify-between flex-shrink-0">
          <div class="flex items-center gap-3">
            <div class="w-8 h-8 rounded-lg bg-brand flex items-center justify-center text-white shadow-sm">
              <i data-lucide="layers" class="w-5 h-5"></i>
            </div>
            <span class="font-bold text-lg text-slate-900">選單</span>
          </div>
          <button id="btn-close-drawer" class="p-2 text-slate-400 hover:bg-slate-100 rounded-lg">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-x"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
          </button>
        </div>
        <nav class="flex-1 py-4 px-3 flex flex-col gap-1 overflow-y-auto" id="mobile-sidebar-nav"></nav>
      </aside>
    </div>
  `;

  createIcons({
    icons: { Layers, Bell, LogOut, LayoutDashboard, Table, PieChart, Users, Settings, Database, MapPin, Menu, FileEdit, Search, Sliders, Package, Shield },
    attrs: { class: 'w-5 h-5' }
  });

  const desktopSidebar = document.getElementById('desktop-sidebar');
  const btnCollapse = document.getElementById('btn-collapse-sidebar');
  const btnExpand = document.getElementById('btn-expand-sidebar');

  btnCollapse?.addEventListener('click', () => {
    desktopSidebar.classList.add('md:hidden');
    btnExpand.classList.remove('md:hidden');
    localStorage.setItem('sidebar_collapsed', 'true');
  });

  btnExpand?.addEventListener('click', () => {
    desktopSidebar.classList.remove('md:hidden');
    btnExpand.classList.add('md:hidden');
    localStorage.setItem('sidebar_collapsed', 'false');
  });

  document.getElementById('btn-logout')?.addEventListener('click', onLogout);

  const drawer = document.getElementById('mobile-drawer');
  const panel = document.getElementById('drawer-panel');
  const overlay = document.getElementById('drawer-overlay');
  
  const openDrawer = () => {
    drawer.classList.remove('hidden');
    setTimeout(() => {
      overlay.classList.add('opacity-100');
      panel.classList.remove('-translate-x-full');
    }, 10);
  };
  
  const closeDrawer = () => {
    overlay.classList.remove('opacity-100');
    panel.classList.add('-translate-x-full');
    setTimeout(() => drawer.classList.add('hidden'), 300);
  };

  document.getElementById('btn-mobile-menu')?.addEventListener('click', openDrawer);
  document.getElementById('btn-close-drawer')?.addEventListener('click', closeDrawer);
  overlay?.addEventListener('click', closeDrawer);

  renderNavBar(user, onNavigate, closeDrawer);

  window.addEventListener('nav-config-updated', () => {
    renderNavBar(user, onNavigate, closeDrawer);
    const currentHash = window.location.hash.replace('#', '') || 'kanban';
    updateNavbar(currentHash);
  });
}

function renderNavBar(user, onNavigate, closeDrawer) {
  const sidebarNav = document.getElementById('sidebar-nav');
  const topControl = document.getElementById('top-segmented-control');
  const mobileNav = document.getElementById('mobile-sidebar-nav');

  if (!sidebarNav || !topControl || !mobileNav) return;

  sidebarNav.innerHTML = '';
  topControl.innerHTML = '';
  mobileNav.innerHTML = '';

  const navConfig = AppState.getNavConfig() || DEFAULT_NAV_CONFIG;
  const role = user.role;

  navConfig.forEach(item => {
    if (item.isEnabled === false) return;

    const routeDef = ALL_ROUTES[item.id];
    if (!routeDef) return;
    if (!routeDef.roles.includes(role)) return;

    if (item.showSidebar) {
      const btn = document.createElement('button');
      btn.className = 'nav-item flex items-center gap-3 w-full px-3 py-2.5 rounded-xl text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-brand transition-colors';
      btn.dataset.route = item.id;
      btn.innerHTML = `<i data-lucide="${item.icon}" class="w-5 h-5"></i>${item.label}`;
      btn.addEventListener('click', () => onNavigate(item.id));
      sidebarNav.appendChild(btn);

      const mobileBtn = document.createElement('button');
      mobileBtn.className = 'nav-item flex items-center gap-3 w-full px-3 py-2.5 rounded-xl text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-brand transition-colors';
      mobileBtn.dataset.route = item.id;
      mobileBtn.innerHTML = `<i data-lucide="${item.icon}" class="w-5 h-5"></i>${item.label}`;
      mobileBtn.addEventListener('click', () => {
        onNavigate(item.id);
        if (typeof closeDrawer === 'function') closeDrawer();
      });
      mobileNav.appendChild(mobileBtn);
    }

    if (item.showTop) {
      const segment = document.createElement('div');
      segment.className = 'segment-btn';
      segment.dataset.route = item.id;
      segment.textContent = item.label;
      segment.addEventListener('click', () => onNavigate(item.id));
      topControl.appendChild(segment);
    }
  });

  createIcons({
    icons: { LayoutDashboard, Table, PieChart, Users, Settings, Layers, Database, MapPin, FileEdit, Search, Sliders, Package, Shield },
    attrs: { class: 'w-5 h-5' }
  });
}

export function updateNavbar(activeRoute) {
  document.querySelectorAll('.nav-item').forEach(item => {
    if (item.dataset.route === activeRoute) {
      item.classList.add('bg-brand/5', 'text-brand', 'font-bold');
      item.classList.remove('text-slate-600');
    } else {
      item.classList.remove('bg-brand/5', 'text-brand', 'font-bold');
      item.classList.add('text-slate-600');
    }
  });

  document.querySelectorAll('#top-segmented-control .segment-btn').forEach(item => {
    if (item.dataset.route === activeRoute) {
      item.classList.add('active');
    } else {
      item.classList.remove('active');
    }
  });
}

export function renderPageLayout({ title, subtitle = '', headerActions = '', stats = [], controls = '', listContainerId = 'page-content' }) {
  const container = document.getElementById('app-content');
  if (!container) return;

  let statsHtml = '';
  if (stats && stats.length > 0) {
    statsHtml = `
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4 mb-2">
        ${stats.map(s => `
          <div class="glass-card p-5 flex flex-col gap-2">
            <span class="text-sm text-slate-500 font-medium flex items-center gap-2">
              <i data-lucide="${s.icon || 'activity'}" class="w-4 h-4 text-brand"></i> ${s.label}
            </span>
            <span class="text-3xl font-bold text-slate-900">${s.value}</span>
          </div>
        `).join('')}
      </div>
    `;
  }

  container.innerHTML = `
    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 mb-2">
      <div>
        <h1 class="text-2xl font-bold text-slate-900">${title}</h1>
        ${subtitle ? `<p class="text-sm text-slate-500 mt-1">${subtitle}</p>` : ''}
      </div>
      <div class="flex items-center gap-3">
        ${headerActions}
      </div>
    </div>

    ${statsHtml}

    ${controls ? `<div class="w-full">${controls}</div>` : ''}

    <div id="${listContainerId}" class="flex-1 flex flex-col relative w-full"></div>
  `;

  createIcons({
    icons: { Activity, ClipboardList },
    attrs: { class: 'w-4 h-4' }
  });
}

/*
F: AI-WA-UPWO/src/ui/layout.js
V: 1793059000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/ui/layout.js === -->

<!-- === FILE START: ./src/ui/modal.js === -->

## 📄 ./src/ui/modal.js

```js
import { createIcons, X } from 'lucide';

/**
 * 建立並顯示一個 Modal（三層架構：標題、滾動內容、固定底部）
 * @param {Object} options 設定選項
 * @param {string} options.title 標題
 * @param {string} options.content 內容 HTML
 * @param {string} options.footer 底部按鈕區 HTML（可選）
 * @param {string} options.size 寬度設定（可選，預設為 max-w-md，可傳入 max-w-4xl 等）
 * @param {function} options.onClose 關閉時的回呼
 * @returns {Object} 包含 close 方法的物件，可手動關閉
 */
export function openModal({ title, content, footer = '', size = 'max-w-md', onClose }) {
  // 建立遮罩與容器
  const overlay = document.createElement('div');
  overlay.className = 'fixed inset-0 bg-black/40 backdrop-blur-sm z-50 flex items-center justify-center transition-opacity duration-300 opacity-0 p-4';
  
  const modal = document.createElement('div');
  // 使用 flex flex-col 與 max-h-[90vh] 確保內容不會撐破螢幕，並支援自訂寬度 size
  modal.className = `bg-white border border-slate-100 shadow-soft rounded-[20px] w-full ${size} flex flex-col max-h-[90vh] transform transition-transform duration-300 scale-95 overflow-hidden`;
  
  modal.innerHTML = `
    <div class="flex justify-between items-center p-5 border-b border-slate-100 flex-shrink-0">
      <h3 class="text-lg font-bold text-slate-900">${title}</h3>
      <button id="modal-close-btn" class="text-slate-400 hover:text-slate-600 transition-colors bg-slate-50 hover:bg-slate-100 p-1.5 rounded-full">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
    </div>
    <div class="p-5 text-slate-600 overflow-y-auto flex-1 min-h-0">
      ${content}
    </div>
    ${footer ? `
      <div class="p-4 border-t border-slate-100 flex-shrink-0 bg-white">
        ${footer}
      </div>
    ` : ''}
  `;

  overlay.appendChild(modal);
  document.body.appendChild(overlay);

  createIcons({
    icons: { X },
    attrs: { class: 'w-5 h-5' }
  });

  // 進場動畫
  requestAnimationFrame(() => {
    overlay.classList.remove('opacity-0');
    modal.classList.remove('scale-95');
    modal.classList.add('scale-100');
  });

  // 關閉邏輯
  const close = () => {
    overlay.classList.add('opacity-0');
    modal.classList.remove('scale-100');
    modal.classList.add('scale-95');
    
    setTimeout(() => {
      overlay.remove();
      if (typeof onClose === 'function') onClose();
    }, 300); // 等待動畫結束
  };

  // 綁定事件
  modal.querySelector('#modal-close-btn').addEventListener('click', close);
  overlay.addEventListener('click', (e) => {
    if (e.target === overlay) close(); // 點擊遮罩關閉
  });

  return { close, element: modal };
}

/**
 * 顯示確認對話框 (Promise 包裝)
 * @param {Object} options 設定選項
 * @param {string} options.title 標題
 * @param {string} options.message 訊息內容
 * @param {string} options.confirmText 確認按鈕文字 (預設 '確認')
 * @param {string} options.cancelText 取消按鈕文字 (預設 '取消')
 * @param {boolean} options.isDanger 是否為危險操作 (紅色按鈕)
 * @returns {Promise<boolean>} 使用者點擊確認回傳 true，取消回傳 false
 */
export function openConfirm({ title, message, confirmText = '確認', cancelText = '取消', isDanger = false }) {
  return new Promise((resolve) => {
    const btnClass = isDanger 
      ? 'bg-rose-500 hover:bg-rose-600 text-white shadow-rose-500/20' 
      : 'bg-brand hover:bg-brand-hover text-white shadow-brand/20';

    const content = `
      <p class="text-slate-600">${message}</p>
    `;

    const footer = `
      <div class="flex justify-end gap-3">
        <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200 transition-colors">
          ${cancelText}
        </button>
        <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium transition-all active:scale-95 shadow-md ${btnClass}">
          ${confirmText}
        </button>
      </div>
    `;

    const { close, element } = openModal({
      title,
      content,
      footer,
      size: 'max-w-md',
      onClose: () => resolve(false) // 若透過右上角 X 或遮罩關閉，視為取消
    });

    // 覆寫按鈕事件
    element.querySelector('#modal-cancel-btn').addEventListener('click', () => {
      close();
    });

    element.querySelector('#modal-confirm-btn').addEventListener('click', () => {
      // 避免觸發 onClose 的 resolve(false)
      element.querySelector('#modal-close-btn').removeEventListener('click', close);
      element.parentElement.removeEventListener('click', close); 
      
      // 直接觸發動畫並移除
      element.parentElement.classList.add('opacity-0');
      element.classList.remove('scale-100');
      element.classList.add('scale-95');
      setTimeout(() => {
        element.parentElement.remove();
        resolve(true);
      }, 300);
    });
  });
}

/*
F: AI-WA-UPWO/src/ui/modal.js
V: 1792957000
W: UPGRADE-005
*/```

<!-- === FILE END: ./src/ui/modal.js === -->

<!-- === FILE START: ./src/ui/order-detail-modal.js === -->

## 📄 ./src/ui/order-detail-modal.js

```js
import { openModal } from './modal.js';
import { showToast } from './toast.js';
import { getDocument } from '../db/read.js';
import { setDocument, updateDocument } from '../db/write.js';
import { renderOrderEditor } from './order-editor.js';
import { createIcons } from 'lucide';

/**
 * 開啟工單詳細/編輯彈窗
 * @param {Object} options
 * @param {string|null} options.orderId 若為 null 則為新增
 * @param {Object} options.currentSettings 系統設定
 * @param {Function} options.onSaved 儲存成功後的回呼
 */
export async function openOrderModal({ orderId = null, currentSettings, onSaved }) {
  let initialData = null;
  
  if (orderId) {
    try {
      initialData = await getDocument('work_orders', orderId);
      if (!initialData) {
        showToast('找不到該工單', 'error');
        return;
      }
    } catch (err) {
      showToast('讀取工單失敗', 'error');
      return;
    }
  }

  const title = orderId ? '編輯工單' : '新增工單';

  const content = `<div id="modal-editor-container" class="w-full"></div>`;
  
  const { close, element } = openModal({ title, content, footer: '', size: 'max-w-5xl' });

  const container = element.querySelector('#modal-editor-container');

  renderOrderEditor({
    container: container,
    schema: currentSettings.schema || [],
    formulas: currentSettings.formulas || {},
    viewConfig: currentSettings.view_configs?.['data-entry'] || currentSettings.view_configs?.['table'] || [],
    routeTemplates: currentSettings.route_templates || [],
    customDatabases: currentSettings.custom_databases || [], // 💡 傳入自訂資料庫
    layoutMode: 'A',
    initialData: initialData,
    isViewMode: false,
    onSave: async (formData) => {
      const targetId = formData.id || orderId;
      
      if (!targetId || String(targetId).trim() === '') {
        showToast('工單號碼為必填', 'error');
        return;
      }

      try {
        // 💡 儲存前處理：將關聯欄位的 ID 轉換為名稱，讓匯出 Excel 時能顯示人類看得懂的文字
        (currentSettings.schema || []).forEach(field => {
          if (field.type === 'relation' && !field.isSystem && formData.data && formData.data[field.key]) {
            const val = formData.data[field.key];
            const db = (currentSettings.custom_databases || []).find(d => d.id === field.db_id);
            if (db) {
              const item = db.items.find(i => i.id === val || i.name === val);
              if (item) {
                formData.data[field.key] = item.name; // 轉存名稱
              }
            }
          }
        });

        if (!orderId) {
          const existing = await getDocument('work_orders', targetId);
          if (existing) {
            showToast(`工單 ${targetId} 已存在`, 'error');
            return;
          }

          formData.source = 'manual';
          if (!formData.route || formData.route.length === 0) {
            formData.route = currentSettings.statuses ? [...currentSettings.statuses] : [];
            formData.status = currentSettings.statuses?.[0] || '排產中';
          }
          formData.uploadDate = new Date().toISOString().split('T')[0];
          formData.statusUpdateDate = formData.uploadDate;

          await setDocument('work_orders', targetId, formData);
          showToast('工單新增成功', 'success');
        } else {
          const existing = await getDocument('work_orders', targetId);
          if (existing && existing.source === 'excel') {
            delete formData.uploadDate;
          }
          
          await updateDocument('work_orders', targetId, formData);
          showToast('工單更新成功', 'success');
        }

        if (onSaved) onSaved();
        close();
      } catch (err) {
        console.error('[Modal Save Error]', err);
        showToast('儲存失敗', 'error');
      }
    },
    onCancel: () => {
      close();
    }
  });
}

/*
F: AI-WA-UPWO/src/ui/order-detail-modal.js
V: 1793085000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/ui/order-detail-modal.js === -->

<!-- === FILE START: ./src/ui/order-editor.js === -->

## 📄 ./src/ui/order-editor.js

```js
import { createIcons, Save, RotateCcw } from 'lucide';
import { showToast } from './toast.js';
import { evaluate } from '../domain/formula-engine.js';

/**
 * 輔助函式：將可能是字串或陣列的 route 統一轉為陣列
 */
function normalizeRoute(route) {
  if (Array.isArray(route)) return route;
  if (typeof route === 'string') {
    if (route.includes(',')) return route.split(',').map(s => s.trim()).filter(Boolean);
    if (route.includes('→')) return route.split('→').map(s => s.trim()).filter(Boolean);
    return [route.trim()];
  }
  return [];
}

export function renderOrderEditor({ 
  container, 
  schema, 
  formulas, 
  viewConfig, 
  routeTemplates = [],
  customDatabases = [], // 💡 接收自訂資料庫
  layoutMode = 'B', 
  initialData = null, 
  isViewMode = false,
  onSave,
  onCancel
}) {
  if (!container) return;

  let displayFields = [];
  if (viewConfig && viewConfig.length > 0) {
    displayFields = viewConfig.map(key => schema.find(f => f.key === key)).filter(Boolean);
  } else {
    displayFields = schema.filter(f => f.isEditable);
  }

  const formData = {};
  displayFields.forEach(f => {
    if (initialData) {
      if (initialData.data && initialData.data[f.key] !== undefined) {
        formData[f.key] = initialData.data[f.key];
      } else if (initialData[f.key] !== undefined) {
        formData[f.key] = initialData[f.key];
      } else {
        formData[f.key] = '';
      }
    } else {
      formData[f.key] = '';
    }
  });

  const getOrderCategory = () => {
    return (initialData && initialData.data && initialData.data.category) 
      ? String(initialData.data.category).toUpperCase() 
      : (formData['id'] ? String(formData['id']).match(/^[a-zA-Z]+/)?.[0]?.toUpperCase() : '其他') || '其他';
  };

  let html = `
    <div class="bg-transparent flex flex-col">
      <div class="px-2 py-4 border-b border-slate-100 flex flex-col sm:flex-row sm:items-center justify-between flex-shrink-0 gap-3">
        <div class="flex items-center gap-3">
          <h2 class="text-lg font-bold text-slate-800">${initialData ? '編輯工單資料' : '新增工單資料'}</h2>
          <span class="text-xs px-2 py-0.5 bg-slate-100 border border-slate-200 text-slate-600 rounded">${isViewMode ? '檢視模式' : '編輯模式'}</span>
        </div>
        <div class="flex items-center gap-2">
          <button id="editor-btn-cancel" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-white border border-slate-200 hover:bg-slate-50 transition-colors">
            取消
          </button>
          <button id="editor-btn-save" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover shadow-md shadow-brand/20 transition-all flex items-center gap-2 ${isViewMode ? 'hidden' : ''}">
            <i data-lucide="save" class="w-4 h-4"></i> 儲存
          </button>
        </div>
      </div>

      <div class="py-4 relative" id="editor-body">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 sm:gap-6">
          ${displayFields.map(f => {
            const value = formData[f.key] !== undefined ? formData[f.key] : '';
            const disabledAttr = isViewMode ? 'disabled' : '';
            const baseClass = `w-full px-3 py-2.5 bg-white border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand transition-colors shadow-sm ${isViewMode ? 'opacity-70 bg-slate-100 cursor-not-allowed' : ''}`;

            // 1. 處理製程路線圖 (route)
            if (f.key === 'route') {
              const orderCategory = getOrderCategory();
              const availableTemplates = routeTemplates.filter(t => {
                if (!t.categories || t.categories.length === 0) return true;
                if (Array.isArray(t.categories)) return t.categories.map(c => String(c).toUpperCase()).includes(orderCategory);
                if (typeof t.category === 'string' && t.category.trim() !== '') return t.category.toUpperCase() === orderCategory;
                return false;
              });

              let currentMatchName = '';
              if (initialData && initialData.route) {
                const currentRouteArr = normalizeRoute(initialData.route);
                const matched = availableTemplates.find(t => JSON.stringify(t.steps) === JSON.stringify(currentRouteArr));
                if (matched) currentMatchName = matched.name;
              }

              return `
                <div class="flex flex-col gap-1.5">
                  <label class="text-sm font-medium text-slate-700">${f.label}</label>
                  <select data-key="${f.key}" class="${baseClass}" ${disabledAttr}>
                    <option value="" ${currentMatchName === '' ? 'selected' : ''}>(自訂流程 / 預設)</option>
                    ${availableTemplates.map(t => `<option value="${t.name}" ${t.name === currentMatchName ? 'selected' : ''}>${t.name}</option>`).join('')}
                  </select>
                </div>
              `;
            }

            // 2. 💡 處理關聯資料庫 (relation)
            if (f.type === 'relation') {
              const targetDb = customDatabases.find(db => db.id === f.db_id);
              const items = targetDb ? targetDb.items : [];
              
              // 處理舊資料可能是存 name 而不是 id 的情況
              const isIdMatch = items.some(item => item.id === value);
              const selectedItem = items.find(item => item.id === value || item.name === value);
              const selectedValue = selectedItem ? selectedItem.id : value;

              return `
                <div class="flex flex-col gap-1.5">
                  <label class="text-sm font-medium text-slate-700">${f.label} <span class="text-xs text-brand">🔗</span></label>
                  <select data-key="${f.key}" class="${baseClass}" ${disabledAttr}>
                    <option value="">(請選擇)</option>
                    ${!targetDb ? '<option value="">找不到關聯資料庫</option>' : ''}
                    ${items.map(item => `<option value="${item.id}" ${item.id === selectedValue ? 'selected' : ''}>${item.name}</option>`).join('')}
                  </select>
                </div>
              `;
            }

            // 3. 一般欄位渲染
            const inputType = f.type === 'date' ? 'date' : (f.type === 'number' ? 'number' : 'text');
            return `
              <div class="flex flex-col gap-1.5">
                <label class="text-sm font-medium text-slate-700">${f.label}</label>
                <input type="${inputType}" data-key="${f.key}" value="${value}" class="${baseClass}" placeholder="請輸入${f.label}" ${disabledAttr}>
              </div>
            `;
          }).join('')}
        </div>
      </div>
    </div>
  `;

  container.innerHTML = html;
  createIcons({ icons: { Save, RotateCcw }, attrs: { class: 'w-4 h-4' } });

  const bodyEl = container.querySelector('#editor-body');
  const inputs = bodyEl.querySelectorAll('input[data-key], select[data-key]');
  
  const handleInputChange = (e) => {
    const target = e.target;
    const key = target.dataset.key;
    if (!key) return;

    if (key === 'route') {
      const templateName = target.value;
      if (templateName) {
        const matchedTemplate = routeTemplates.find(t => t.name === templateName);
        if (matchedTemplate && matchedTemplate.steps) {
          formData['status'] = matchedTemplate.steps[0];
          const statusInput = bodyEl.querySelector(`[data-key="status"]`);
          if (statusInput) {
            statusInput.value = formData['status'];
            statusInput.classList.add('bg-emerald-50', 'text-emerald-700', 'border-emerald-200');
            setTimeout(() => statusInput.classList.remove('bg-emerald-50', 'text-emerald-700', 'border-emerald-200'), 600);
          }
          showToast(`已套用路線圖：${templateName}`, 'info');
        }
      }
      return;
    }

    formData[key] = target.value;

    if (formulas && Object.keys(formulas).length > 0) {
      const results = evaluate(formulas, formData);
      Object.entries(results).forEach(([resKey, resVal]) => {
        const resInput = bodyEl.querySelector(`[data-key="${resKey}"]`);
        if (resInput && resInput.value !== resVal) {
          resInput.value = resVal;
          formData[resKey] = resVal;
          resInput.classList.add('bg-emerald-50', 'text-emerald-700', 'border-emerald-200');
          setTimeout(() => resInput.classList.remove('bg-emerald-50', 'text-emerald-700', 'border-emerald-200'), 600);
        }
      });
    }
  };

  inputs.forEach(input => {
    input.addEventListener('input', handleInputChange);
    input.addEventListener('change', handleInputChange);
  });

  container.querySelector('#editor-btn-cancel')?.addEventListener('click', () => {
    if (onCancel) onCancel();
  });

  container.querySelector('#editor-btn-save')?.addEventListener('click', async () => {
    if (onSave) {
      const finalData = Object.assign({}, initialData); 
      if (!finalData.data) finalData.data = {};
      if (!finalData.id) finalData.id = formData['id'];

      const selectedTemplateName = bodyEl.querySelector('[data-key="route"]')?.value;
      if (selectedTemplateName) {
        const matchedTemplate = routeTemplates.find(t => t.name === selectedTemplateName);
        if (matchedTemplate) {
          finalData.route = matchedTemplate.steps;
        }
      } else {
        if (!finalData.route) finalData.route = []; 
      }
      
      if (formData['status']) finalData.status = formData['status'];

      displayFields.forEach(f => {
        if (f.key === 'route' || f.key === 'status') return;
        
        let val = formData[f.key];
        
        // 如果是關聯欄位，我們儲存 ID。但為了讓匯出顯示名稱，我們可以額外存一個 map 或是保留原本的字串？
        // 為了架構單純，這裡依然存 ID。若要顯示名稱，由 UI 讀取 customDatabases 轉換。
        // 但為了向下相容舊的 supplierName (存 name)，這裡不強制轉換，直接存 val。
        
        if (f.isSystem) {
          finalData[f.key] = val;
        } else {
          finalData.data[f.key] = val;
        }
      });

      await onSave(finalData);
    }
  });
}

/*
F: AI-WA-UPWO/src/ui/order-editor.js
V: 1793084000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/ui/order-editor.js === -->

<!-- === FILE START: ./src/ui/order-view-editor.js === -->

## 📄 ./src/ui/order-view-editor.js

```js
import { updateDocument, setDocument } from '../db/write.js';
import { getDocument, getCollection } from '../db/read.js';
import { showToast } from '../ui/toast.js';
import { openModal } from '../ui/modal.js';
import { openRouteBuilder } from './route-builder.js';
import { openOrderModal } from './order-detail-modal.js'; // 💡 引入新的彈窗
import { AppState } from '../config/state.js';
import { createIcons, GripVertical, Plus, X } from 'lucide';

/**
 * 輔助函式：將可能是字串或陣列的 route 統一轉為陣列
 */
function normalizeRoute(route) {
  if (Array.isArray(route)) return route;
  if (typeof route === 'string') {
    if (route.includes(',')) return route.split(',').map(s => s.trim()).filter(Boolean);
    if (route.includes('→')) return route.split('→').map(s => s.trim()).filter(Boolean);
    return [route.trim()];
  }
  return [];
}

/**
 * 取得工單類別 (優先從 data.category，其次從工單號前兩碼)
 */
function getOrderCategory(order) {
  if (order.data && order.data.category) {
    return String(order.data.category).toUpperCase();
  }
  const match = String(order.id).match(/^[a-zA-Z]+/);
  return match ? match[0].toUpperCase() : '其他';
}

/**
 * 處理行內編輯事件
 */
window.addEventListener('inline-edit', async (e) => {
  const { id, key, value } = e.detail;
  if (!id || !key) return;

  try {
    const updateData = {};
    
    // 系統核心欄位清單（直接更新根節點）
    const coreFields = ['id', 'status', 'salesReps', 'uploadDate', 'dueDate', 'statusUpdateDate', 'route'];
    
    if (coreFields.includes(key)) {
      updateData[key] = value;
    } else {
      // 動態欄位必須使用點記法 (dot notation) 更新 data 物件內的屬性
      updateData[`data.${key}`] = value;
    }

    // 💡 清除差異標記
    const docSnap = await getDocument('work_orders', id);
    if (docSnap && docSnap.diff && docSnap.diff[key]) {
      const newDiff = Object.assign({}, docSnap.diff);
      delete newDiff[key];
      updateData['diff'] = newDiff;
    }

    await updateDocument('work_orders', id, updateData);
    showToast(`已更新資料`, 'success');
    
  } catch (error) {
    console.error('[Inline Edit Error]', error);
    showToast('更新失敗，請檢查網路或稍後再試', 'error');
  }
});

/**
 * 處理下拉選單切換生產流程事件
 */
window.addEventListener('change-route', async (e) => {
  const { id, templateName } = e.detail;
  if (!id || !templateName) return;

  try {
    const settings = await getDocument('settings', 'main');
    const templates = settings.route_templates || [];
    const template = templates.find(t => t.name === templateName);

    if (!template) {
      showToast('找不到該流程模板', 'error');
      return;
    }

    const orderDoc = await getDocument('work_orders', id);
    if (!orderDoc) return;

    // 💡 修正：先正規化當前路線再比對狀態
    const currentRoute = normalizeRoute(orderDoc.route);
    let newStatus = orderDoc.status;
    
    // 如果新流程不包含當前狀態，則退回第一步
    if (!template.steps.includes(newStatus)) {
      newStatus = template.steps[0];
    }

    await updateDocument('work_orders', id, {
      route: template.steps,
      status: newStatus,
      statusUpdateDate: new Date().toISOString()
    });
    
    showToast(`已套用流程：${template.name}`, 'success');
  } catch (err) {
    console.error('[Change Route Error]', err);
    showToast('套用失敗', 'error');
  }
});

/**
 * 處理開啟快速新增流程彈窗事件
 */
window.addEventListener('open-route-builder', async (e) => {
  const { orderId, defaultCategory } = e.detail;
  
  try {
    const settings = await getDocument('settings', 'main');
    if (!settings) return;

    // 獲取現有的所有類別 (掃描所有工單)
    const allOrders = await getCollection('work_orders');
    const categories = new Set();
    allOrders.forEach(o => {
      const cat = getOrderCategory(o);
      if (cat !== '其他') categories.add(cat);
    });

    openRouteBuilder({
      currentSettings: settings,
      availableCategories: Array.from(categories),
      defaultCategory: defaultCategory,
      onSave: async (newTemplate) => {
        // 1. 儲存新模板到 settings
        const latestSettings = await getDocument('settings', 'main');
        if (!latestSettings.route_templates) latestSettings.route_templates = [];
        latestSettings.route_templates.push(newTemplate);
        await setDocument('settings', 'main', latestSettings);

        // 2. 套用到當前工單
        if (orderId) {
          await updateDocument('work_orders', orderId, {
            route: newTemplate.steps,
            status: newTemplate.steps[0],
            statusUpdateDate: new Date().toISOString()
          });
          showToast('已新增並套用流程', 'success');
        } else {
          showToast('已新增流程', 'success');
        }
        
        // 觸發刷新
        window.dispatchEvent(new Event('settings-updated'));
        const pageKey = window.location.hash.replace('#', '') || 'table';
        window.dispatchEvent(new Event(`refresh-${pageKey}`));
      }
    });
  } catch (err) {
    console.error(err);
    showToast('讀取資料失敗', 'error');
  }
});

/**
 * 處理全域點擊事件（點擊工單號碼彈窗、上/下工序與編輯製程）
 */
document.addEventListener('click', async (e) => {
  // 0. 點擊工單號碼，開啟詳細資料彈窗
  const detailBtn = e.target.closest('button[data-action="open-order-detail"]');
  if (detailBtn) {
    const id = detailBtn.dataset.id;
    try {
      const settingsDoc = await getDocument('settings', 'main');
      
      if (settingsDoc) {
        // 💡 呼叫新的彈窗，傳入 orderId 與設定
        openOrderModal({
          orderId: id,
          currentSettings: settingsDoc,
          onSaved: () => {
            // 儲存後觸發當前頁面刷新
            const pageKey = window.location.hash.replace('#', '') || 'table';
            window.dispatchEvent(new Event(`refresh-${pageKey}`));
          }
        });
      }
    } catch (err) {
      console.error('[Open Detail Error]', err);
      showToast('無法開啟詳細資料', 'error');
    }
    return;
  }

  // 1. 點擊上一個工序
  const prevBtn = e.target.closest('button[data-action="prev-step"]');
  if (prevBtn) {
    const id = prevBtn.dataset.id;
    const routeStr = prevBtn.dataset.route;
    const currentStatus = prevBtn.dataset.status;
    
    try {
      const route = JSON.parse(routeStr);
      const idx = route.indexOf(currentStatus);
      if (idx > 0) {
        const newStatus = route[idx - 1];
        await updateDocument('work_orders', id, { status: newStatus, statusUpdateDate: new Date().toISOString() });
        showToast(`已退回：${newStatus}`, 'success');
      }
    } catch (err) {
      console.error('[Prev Step Error]', err);
    }
    return;
  }

  // 2. 點擊下一個工序
  const nextBtn = e.target.closest('button[data-action="next-step"]');
  if (nextBtn) {
    const id = nextBtn.dataset.id;
    const routeStr = nextBtn.dataset.route;
    const currentStatus = nextBtn.dataset.status;
    
    try {
      const route = JSON.parse(routeStr);
      const idx = route.indexOf(currentStatus);
      if (idx !== -1 && idx < route.length - 1) {
        const newStatus = route[idx + 1];
        await updateDocument('work_orders', id, { status: newStatus, statusUpdateDate: new Date().toISOString() });
        showToast(`已進入：${newStatus}`, 'success');
      }
    } catch (err) {
      console.error('[Next Step Error]', err);
    }
    return;
  }

  // 3. 點擊編輯製程 ⚙️ (保留給管理總表或特定頁面使用)
  const editRouteBtn = e.target.closest('button[data-action="edit-route"]');
  if (editRouteBtn) {
    const id = editRouteBtn.dataset.id;
    const routeStr = editRouteBtn.dataset.route;
    const currentStatus = editRouteBtn.dataset.status;
    
    let currentRoute = [];
    try {
      currentRoute = normalizeRoute(JSON.parse(routeStr));
    } catch (e) {
      currentRoute = [];
    }

    const allStatuses = AppState.getStatuses() || [];

    // 獲取最新的設定與模板
    let routeTemplates = [];
    try {
      const settings = await getDocument('settings', 'main');
      if (settings && settings.route_templates) {
        routeTemplates = settings.route_templates;
      }
    } catch (err) {
      console.warn('無法讀取模板設定', err);
    }

    const templateOptions = routeTemplates.map((t, i) => {
      const label = t.categories && t.categories.length > 0 ? `[${t.categories.join(',')}] ${t.name}` : `[通用] ${t.name}`;
      return `<option value="${i}">${label}</option>`;
    }).join('');

    const content = `
      <div class="flex flex-col gap-4">
        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-2 mb-2">
          <p class="text-sm text-slate-500">點擊左側加入右側，或拖曳右側調整順序。</p>
          <div class="flex items-center gap-2 w-full sm:w-auto">
            <select id="modal-template-select" class="flex-1 sm:w-48 px-3 py-1.5 bg-white border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand shadow-sm">
              <option value="">-- 快速套用模板 --</option>
              ${templateOptions}
            </select>
          </div>
        </div>
        
        <div class="flex flex-col md:flex-row gap-4 h-[40vh]">
          <!-- 左側：可選工序 -->
          <div class="flex-1 bg-slate-50 border border-slate-200 rounded-xl p-3 flex flex-col">
            <h4 class="font-bold text-sm text-slate-700 mb-2">可選工序</h4>
            <div class="flex-1 overflow-y-auto flex flex-col gap-2" id="modal-available-list"></div>
          </div>
          <!-- 右側：預計工序順序 -->
          <div class="flex-1 bg-white border border-slate-200 rounded-xl p-3 flex flex-col shadow-sm">
            <h4 class="font-bold text-sm text-slate-700 mb-2">預計工序順序</h4>
            <div class="flex-1 overflow-y-auto flex flex-col gap-2" id="modal-selected-list"></div>
          </div>
        </div>
      </div>
    `;

    const footer = `
      <div class="flex justify-end gap-3">
        <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
        <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover">儲存</button>
      </div>
    `;

    const { close, element } = openModal({ title: `編輯製程路線 (${id})`, content, footer });
    
    let currentSteps = Array.from(currentRoute);
    let sortableInstance = null;

    const renderModalLists = () => {
      const availableList = element.querySelector('#modal-available-list');
      const selectedList = element.querySelector('#modal-selected-list');
      
      const availableStatuses = allStatuses.filter(s => !currentSteps.includes(s));

      // 渲染左側
      if (availableStatuses.length === 0) {
        availableList.innerHTML = '<div class="text-slate-400 text-sm text-center py-4">所有工序已加入</div>';
      } else {
        availableList.innerHTML = availableStatuses.map(status => `
          <div class="flex items-center gap-3 bg-white p-3 rounded-lg border border-slate-200 cursor-pointer hover:border-brand transition-colors available-item" data-step="${status}">
            <span class="flex-1 font-medium text-slate-700">${status}</span>
            <i data-lucide="plus" class="w-4 h-4 text-slate-400"></i>
          </div>
        `).join('');
      }

      // 渲染右側
      if (currentSteps.length === 0) {
        selectedList.innerHTML = '<div class="text-slate-400 text-sm text-center py-4">請從左側加入工序</div>';
      } else {
        selectedList.innerHTML = currentSteps.map((step, idx) => `
          <div class="flex items-center gap-3 bg-slate-50 p-3 rounded-lg border border-slate-200 step-item" data-step="${step}">
            <i data-lucide="grip-vertical" class="w-4 h-4 text-slate-300 cursor-grab"></i>
            <span class="flex-1 font-medium text-slate-700"><span class="text-slate-400 mr-1">${idx + 1}.</span>${step}</span>
            <button class="text-slate-400 hover:text-rose-500 btn-remove-step" data-step="${step}">
              <i data-lucide="x" class="w-4 h-4"></i>
            </button>
          </div>
        `).join('');
      }

      createIcons({ icons: { Plus, GripVertical, X }, attrs: { class: 'w-4 h-4' } });
      
      // 綁定左側點擊加入
      availableList.querySelectorAll('.available-item').forEach(item => {
        item.addEventListener('click', () => {
          const step = item.dataset.step;
          if (!currentSteps.includes(step)) {
            currentSteps.push(step);
            renderModalLists();
          }
        });
      });

      // 綁定右側移除
      selectedList.querySelectorAll('.btn-remove-step').forEach(btn => {
        btn.addEventListener('click', (ev) => {
          const step = ev.currentTarget.dataset.step;
          currentSteps = currentSteps.filter(s => s !== step);
          renderModalLists();
        });
      });

      // SortableJS 拖曳
      if (sortableInstance) sortableInstance.destroy();
      if (currentSteps.length > 0) {
        sortableInstance = new Sortable(selectedList, {
          animation: 150,
          handle: '.cursor-grab',
          ghostClass: 'opacity-50',
          forceFallback: true,
          fallbackTolerance: 3,
          onEnd: () => {
            currentSteps = Array.from(selectedList.children).map(el => el.dataset.step);
            renderModalLists();
          }
        });
      }
    };

    renderModalLists();

    // 快速套用模板變更事件
    element.querySelector('#modal-template-select').addEventListener('change', (ev) => {
      const idx = parseInt(ev.target.value);
      if (!isNaN(idx) && routeTemplates[idx]) {
        currentSteps = Array.from(routeTemplates[idx].steps);
        renderModalLists();
        showToast(`已套用模板：${routeTemplates[idx].name}`, 'info');
        ev.target.value = ''; // 重設下拉選單
      }
    });

    element.querySelector('#modal-cancel-btn').addEventListener('click', close);

    element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
      if (currentSteps.length === 0) {
        showToast('製程路線不能為空', 'error');
        return;
      }

      let newStatus = currentStatus;
      
      // 若原本的狀態不在新路線中，依規則退回上一個工序
      if (!currentSteps.includes(currentStatus)) {
        let fallbackStatus = currentSteps[0]; // 預設退回第一個
        
        // 尋找原本路線中當前狀態的上一個工序
        const oldIdx = currentRoute.indexOf(currentStatus);
        if (oldIdx > 0) {
          const prevStatus = currentRoute[oldIdx - 1];
          // 如果上一個工序存在於新路線中，就退回它
          if (currentSteps.includes(prevStatus)) {
            fallbackStatus = prevStatus;
          }
        }
        newStatus = fallbackStatus;
      }

      const confirmBtn = element.querySelector('#modal-confirm-btn');
      confirmBtn.disabled = true;
      confirmBtn.textContent = '儲存中...';

      try {
        await updateDocument('work_orders', id, { 
          route: currentSteps,
          status: newStatus,
          statusUpdateDate: new Date().toISOString()
        });
        showToast(`製程已更新${newStatus !== currentStatus ? `，狀態退回至：${newStatus}` : ''}`, 'success');
        close();
      } catch (err) {
        console.error(err);
        showToast('更新失敗，請稍後再試', 'error');
        confirmBtn.disabled = true;
        confirmBtn.textContent = '儲存';
      }
    });
    return;
  }
});

/*
F: AI-WA-UPWO/src/ui/order-view-editor.js
V: 1793036000
W: UPGRADE-007
*/```

<!-- === FILE END: ./src/ui/order-view-editor.js === -->

<!-- === FILE START: ./src/ui/order-view.js === -->

## 📄 ./src/ui/order-view.js

```js
import { AppState } from '../config/state.js';
import { createIcons, ArrowUpDown, ArrowUp, ArrowDown, Trash2, Inbox, Search, ChevronLeft, ChevronRight, Settings, Plus, AlertCircle } from 'lucide';

/**
 * 輔助函式：將可能是字串或陣列的 route 統一轉為陣列
 */
function normalizeRoute(route) {
  if (Array.isArray(route)) return route;
  if (typeof route === 'string') {
    if (route.includes(',')) return route.split(',').map(s => s.trim()).filter(Boolean);
    if (route.includes('→')) return route.split('→').map(s => s.trim()).filter(Boolean);
    return [route.trim()];
  }
  return [];
}

/**
 * 將 Excel 日期轉為 YYYY-MM-DD，並防呆過濾
 */
export function formatExcelDate(val) {
  if (!val) return "";
  
  if (typeof val === 'string') {
    if (val.includes('T')) return val.split('T')[0];
    if (val.includes('-') || val.includes('/')) {
      if (val.includes('/')) {
        const parts = val.split('/');
        if (parts.length === 3) {
          if (parts[0].length === 4) return `${parts[0]}-${parts[1].padStart(2,'0')}-${parts[2].padStart(2,'0')}`;
          if (parts[2].length === 4) return `${parts[2]}-${parts[1].padStart(2,'0')}-${parts[0].padStart(2,'0')}`;
        }
      }
      return val.trim();
    }
  }
  
  const num = Number(val);
  if (!isNaN(num) && num > 20000 && num < 80000) {
    const date = new Date((num - 25569) * 86400 * 1000);
    if (!isNaN(date.getTime())) return date.toISOString().split('T')[0];
  }
  return String(val).trim();
}

/**
 * 取得工單類別
 */
function getOrderCategory(order) {
  if (order.data && order.data.category) {
    return String(order.data.category).toUpperCase();
  }
  const match = String(order.id).match(/^[a-zA-Z]+/);
  return match ? match[0].toUpperCase() : '其他';
}

// 全域狀態暫存，避免切換頁面時遺失
const viewStates = {};

/**
 * 核心渲染器：完全數據驅動
 */
export function renderOrderView({ containerId, orders, visibleFields = [], isAdmin = false, pageKey, route_templates = [], isReadOnly = false, allowStatusChange = false, groupBy = null, onAction }) {
  const container = document.getElementById(containerId);
  if (!container) return;

  if (!viewStates[pageKey]) {
    viewStates[pageKey] = { viewType: 'table', sortKey: '', sortOrder: 'asc', filters: {} };
  }
  const state = viewStates[pageKey];

  // 1. 資料清洗
  let cleanOrders = orders.filter(o => {
    const id = String(o.id || '');
    return id && !id.includes('***') && !id.includes('訂單');
  });

  // 2. 依配置提取顯示欄位
  let displayFields = visibleFields.filter(f => f.isVisible);
  if (displayFields.length === 0) {
    displayFields = [
      { key: 'id', label: '發票', isVisible: true, isEditable: false },
      { key: 'status', label: '生產進度', isVisible: true, isEditable: false }
    ];
  }

  // 名詞替換
  displayFields = displayFields.map(f => {
    if (f.key === 'route_select' || f.key === 'route') {
      return Object.assign({}, f, { label: '製程路線圖' });
    }
    return f;
  });

  // 3. 全域事件委派
  if (!window._orderViewListenerAdded) {
    document.addEventListener('click', (e) => {
      const viewBtn = e.target.closest('[data-action="switch-view"]');
      if (viewBtn) {
        const key = viewBtn.dataset.pageKey;
        if (viewStates[key]) {
          viewStates[key].viewType = viewBtn.dataset.view;
          window.dispatchEvent(new Event(`refresh-${key}`));
        }
        return;
      }

      const th = e.target.closest('th[data-sort]');
      if (th) {
        const key = th.dataset.pageKey;
        const sortKey = th.dataset.sort;
        if (viewStates[key]) {
          if (viewStates[key].sortKey === sortKey) {
            viewStates[key].sortOrder = viewStates[key].sortOrder === 'asc' ? 'desc' : 'asc';
          } else {
            viewStates[key].sortKey = sortKey;
            viewStates[key].sortOrder = 'asc';
          }
          window.dispatchEvent(new Event(`refresh-${key}`));
        }
        return;
      }
      
      const actionBtn = e.target.closest('[data-action="delete"]');
      if (actionBtn) {
        if (onAction) onAction('delete', actionBtn.dataset.id);
      }

      const addRouteBtn = e.target.closest('[data-action="open-route-builder"]');
      if (addRouteBtn) {
        const id = addRouteBtn.dataset.id;
        const cat = addRouteBtn.dataset.category;
        window.dispatchEvent(new CustomEvent('open-route-builder', { 
          detail: { orderId: id, defaultCategory: cat } 
        }));
      }
    });

    document.addEventListener('input', (e) => {
      const searchInput = e.target.closest('input[data-action="global-search"]');
      if (searchInput) {
        const key = searchInput.dataset.pageKey;
        if (viewStates[key]) {
          viewStates[key].filters['__GLOBAL__'] = searchInput.value.toLowerCase();
          window.dispatchEvent(new Event(`refresh-${key}`));
        }
      }
    });

    document.addEventListener('change', (e) => {
      const inlineInput = e.target.closest('input[data-action="inline-edit"]');
      if (inlineInput) {
        const id = inlineInput.dataset.id;
        const key = inlineInput.dataset.key;
        const value = inlineInput.value;
        window.dispatchEvent(new CustomEvent('inline-edit', { detail: { id, key, value } }));
        return;
      }

      const routeSelect = e.target.closest('select[data-action="select-route"]');
      if (routeSelect) {
        const id = routeSelect.dataset.id;
        const templateName = routeSelect.value;
        window.dispatchEvent(new CustomEvent('change-route', { detail: { id, templateName } }));
        return;
      }
    });

    window._orderViewListenerAdded = true;
  }

  // 4. 處理排序與全域篩選
  if (state.sortKey) {
    // 尋找排序欄位的定義，以判斷是否為日期
    const sortFieldDef = displayFields.find(f => f.key === state.sortKey) || { key: state.sortKey };
    
    cleanOrders.sort((a, b) => {
      let valA = getOrderValue(a, sortFieldDef);
      let valB = getOrderValue(b, sortFieldDef);
      return (valA < valB ? -1 : 1) * (state.sortOrder === 'asc' ? 1 : -1);
    });
  }

  const globalFilter = state.filters['__GLOBAL__'];
  if (globalFilter) {
    cleanOrders = cleanOrders.filter(o => {
      return displayFields.some(f => {
        const val = String(getOrderValue(o, f) || '').toLowerCase();
        return val.includes(globalFilter);
      });
    });
  }

  const allStatuses = AppState.getStatuses() || [];
  const isKanban = groupBy === 'status' || pageKey === 'kanban';

  // 5. 生成 HTML
  const viewToggleHtml = `
    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-4">
      <div class="segmented-control">
        <button data-action="switch-view" data-view="card" data-page-key="${pageKey}" class="segment-btn ${state.viewType === 'card' ? 'active' : ''}">卡片</button>
        <button data-action="switch-view" data-view="table" data-page-key="${pageKey}" class="segment-btn ${state.viewType === 'table' ? 'active' : ''}">表格</button>
      </div>
      <div class="relative w-full sm:w-64">
        <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none text-slate-400">
          <i data-lucide="search" class="w-4 h-4"></i>
        </div>
        <input type="text" data-action="global-search" data-page-key="${pageKey}" value="${state.filters['__GLOBAL__'] || ''}" class="w-full pl-9 pr-4 py-1.5 bg-white border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand transition-colors shadow-sm" placeholder="搜尋所有欄位">
      </div>
    </div>
  `;

  if (cleanOrders.length === 0) {
    container.innerHTML = viewToggleHtml + `
      <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft mt-4">
        <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
        <span class="text-base font-medium">目前無工單資料</span>
      </div>
    `;
    createIcons({ icons: { Inbox }, attrs: { class: 'w-12 h-12' } });
    return;
  }

  // 輔助渲染函數：差異標記 HTML
  const renderDiffIcon = (order, fieldKey) => {
    if (order.diff && order.diff[fieldKey]) {
      return `<span class="text-rose-500 ml-1 cursor-help flex-shrink-0" title="Excel 值: ${order.diff[fieldKey].excel}"><i data-lucide="alert-circle" class="w-4 h-4 inline-block"></i></span>`;
    }
    return '';
  };

  // 輔助渲染函數：單一工單卡片
  const renderCard = (order) => {
    return `
      <div class="glass-card p-4 flex flex-col gap-2">
        ${displayFields.map(f => {
          const val = getOrderValue(order, f);
          const diffHtml = renderDiffIcon(order, f.key);
          
          if (f.key === 'id') {
            if (isReadOnly) {
              return `<div class="text-sm"><span class="text-slate-400">${f.label}:</span> <span class="font-bold text-slate-700 text-base">${val || '-'}</span></div>`;
            }
            return `<div class="text-sm"><span class="text-slate-400">${f.label}:</span> <button data-action="open-order-detail" data-id="${order.id}" class="font-bold text-brand hover:underline text-base">${val || '-'}</button></div>`;
          }
          
          if (f.key === 'status') {
            if ((isReadOnly && !allowStatusChange) || !isKanban) {
              return `<div class="text-sm flex items-center"><span class="text-slate-400">${f.label}:</span> <span class="font-bold text-slate-700 ml-1">${order.status}</span>${diffHtml}</div>`;
            }

            let route = order.route ? normalizeRoute(order.route) : allStatuses;
            if (route.length === 0) route = allStatuses;

            let idx = route.indexOf(order.status);
            if (idx === -1) idx = 0;
            const isFirst = idx <= 0;
            const isLast = idx >= route.length - 1;
            const prevStep = isFirst ? '' : route[idx - 1];
            const nextStep = isLast ? '' : route[idx + 1];
            const routeStr = JSON.stringify(route).replace(/"/g, '&quot;');

            return `
              <div class="text-sm flex flex-col gap-1">
                <span class="text-slate-400 flex items-center">${f.label}: ${diffHtml}</span>
                <div class="flex items-center gap-1 bg-slate-50 border border-slate-200 rounded-lg px-1 py-0.5 w-fit max-w-full">
                  <button data-action="prev-step" data-id="${order.id}" data-route="${routeStr}" data-status="${order.status}" 
                    class="flex items-center text-xs p-1 rounded text-slate-600 hover:bg-slate-200 transition-colors ${isFirst ? 'opacity-30 cursor-not-allowed' : ''}" ${isFirst ? 'disabled' : ''}>
                    <i data-lucide="chevron-left" class="w-4 h-4"></i>
                    <span class="truncate max-w-[60px]">${prevStep}</span>
                  </button>
                  <span class="font-bold text-slate-700 text-sm px-1 min-w-[60px] text-center truncate max-w-[100px]">${order.status}</span>
                  <button data-action="next-step" data-id="${order.id}" data-route="${routeStr}" data-status="${order.status}" 
                    class="flex items-center text-xs p-1 rounded text-slate-600 hover:bg-slate-200 transition-colors ${isLast ? 'opacity-30 cursor-not-allowed' : ''}" ${isLast ? 'disabled' : ''}>
                    <span class="truncate max-w-[60px]">${nextStep}</span>
                    <i data-lucide="chevron-right" class="w-4 h-4"></i>
                  </button>
                </div>
              </div>
            `;
          }

          if (f.key === 'route_select') {
            const orderCat = getOrderCategory(order);
            const availableTemplates = (route_templates || []).filter(t => {
              if (!t.categories || t.categories.length === 0) return true;
              return t.categories.includes(orderCat);
            });

            const currentRouteArr = order.route ? normalizeRoute(order.route) : [];
            let currentMatchName = '自訂流程';
            availableTemplates.forEach(t => {
              if (JSON.stringify(t.steps) === JSON.stringify(currentRouteArr)) {
                currentMatchName = t.name;
              }
            });

            return `
              <div class="text-sm flex flex-col gap-1">
                <span class="text-slate-400">${f.label}:</span>
                <span class="font-medium text-slate-700">${currentMatchName}</span>
              </div>
            `;
          }

          if (f.key === 'route') {
            let displayVal = normalizeRoute(order.route).join(' → ');
            return `<div class="text-sm flex flex-col gap-1"><span class="text-slate-400">${f.label}:</span> <span class="font-medium text-slate-700">${displayVal || '-'}</span>${diffHtml}</div>`;
          }

          return `<div class="text-sm flex items-center"><span class="text-slate-400">${f.label}:</span> <span class="font-medium text-slate-700 ml-1">${val || '-'}</span>${diffHtml}</div>`;
        }).join('')}
        ${isAdmin ? `
          <div class="mt-auto pt-2 border-t border-slate-100 flex justify-end">
            <button data-action="delete" data-id="${order.id}" class="text-xs text-rose-500 hover:bg-rose-50 px-2 py-1 rounded">刪除</button>
          </div>
        ` : ''}
      </div>
    `;
  };

  // 輔助渲染函數：工單表格
  const renderTable = (orderList) => {
    return `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-x-auto w-full mb-4">
        <table class="w-full text-left border-collapse min-w-full">
          <thead class="sticky top-0 bg-slate-50 z-10 shadow-sm">
            <tr class="border-b border-slate-200/60 text-sm font-semibold text-slate-600">
              ${displayFields.map(f => `
                <th class="px-4 py-3 cursor-pointer hover:bg-slate-100 transition-colors whitespace-nowrap min-w-[120px]" data-sort="${f.key}" data-page-key="${pageKey}">
                  <div class="flex items-center gap-1">${f.label} <i data-lucide="${state.sortKey === f.key ? (state.sortOrder === 'asc' ? 'arrow-up' : 'arrow-down') : 'arrow-up-down'}" class="w-3 h-3 ${state.sortKey === f.key ? 'text-brand' : 'text-slate-300'}"></i></div>
                </th>
              `).join('')}
              ${isAdmin ? `<th class="px-4 py-3 text-center whitespace-nowrap min-w-[100px]">操作</th>` : ''}
            </tr>
          </thead>
          <tbody class="divide-y divide-slate-100 text-sm text-slate-700">
            ${orderList.map(order => `
              <tr class="hover:bg-slate-50/50 transition-colors">
                ${displayFields.map(f => {
                  const val = getOrderValue(order, f);
                  const diffHtml = renderDiffIcon(order, f.key);

                  if (f.key === 'id') {
                    if (isReadOnly) {
                      return `<td class="px-4 py-3 whitespace-nowrap font-bold text-slate-700">${val || '-'}</td>`;
                    }
                    return `<td class="px-4 py-3 whitespace-nowrap"><button data-action="open-order-detail" data-id="${order.id}" class="font-bold text-brand hover:underline text-base">${val || '-'}</button></td>`;
                  }
                  
                  if (f.key === 'status') {
                    if ((isReadOnly && !allowStatusChange) || !isKanban) {
                      return `<td class="px-4 py-3 whitespace-nowrap font-bold text-slate-700">${order.status}${diffHtml}</td>`;
                    }

                    let route = order.route ? normalizeRoute(order.route) : allStatuses;
                    if (route.length === 0) route = allStatuses;

                    let idx = route.indexOf(order.status);
                    if (idx === -1) idx = 0;
                    const isFirst = idx <= 0;
                    const isLast = idx >= route.length - 1;
                    const prevStep = isFirst ? '' : route[idx - 1];
                    const nextStep = isLast ? '' : route[idx + 1];
                    const routeStr = JSON.stringify(route).replace(/"/g, '&quot;');

                    return `
                      <td class="px-4 py-3 whitespace-nowrap">
                        <div class="flex items-center gap-1 bg-slate-50 border border-slate-200 rounded-lg px-1 py-0.5 w-fit max-w-full">
                          <button data-action="prev-step" data-id="${order.id}" data-route="${routeStr}" data-status="${order.status}" 
                            class="flex items-center text-xs p-1 rounded text-slate-600 hover:bg-slate-200 transition-colors ${isFirst ? 'opacity-30 cursor-not-allowed' : ''}" ${isFirst ? 'disabled' : ''}>
                            <i data-lucide="chevron-left" class="w-4 h-4"></i>
                            <span class="truncate max-w-[60px]">${prevStep}</span>
                          </button>
                          <span class="font-bold text-slate-700 text-sm px-1 min-w-[60px] text-center truncate max-w-[100px]">${order.status}</span>
                          <button data-action="next-step" data-id="${order.id}" data-route="${routeStr}" data-status="${order.status}" 
                            class="flex items-center text-xs p-1 rounded text-slate-600 hover:bg-slate-200 transition-colors ${isLast ? 'opacity-30 cursor-not-allowed' : ''}" ${isLast ? 'disabled' : ''}>
                            <span class="truncate max-w-[60px]">${nextStep}</span>
                            <i data-lucide="chevron-right" class="w-4 h-4"></i>
                          </button>
                          ${diffHtml}
                        </div>
                      </td>
                    `;
                  }

                  if (f.key === 'route_select') {
                    const orderCat = getOrderCategory(order);
                    const availableTemplates = (route_templates || []).filter(t => {
                      if (!t.categories || t.categories.length === 0) return true;
                      return t.categories.includes(orderCat);
                    });

                    const currentRouteArr = order.route ? normalizeRoute(order.route) : [];
                    let currentMatchName = '自訂流程';
                    availableTemplates.forEach(t => {
                      if (JSON.stringify(t.steps) === JSON.stringify(currentRouteArr)) {
                        currentMatchName = t.name;
                      }
                    });

                    return `<td class="px-4 py-3 whitespace-nowrap text-slate-600">${currentMatchName}</td>`;
                  }
                  
                  if (f.key === 'route') {
                    let displayVal = normalizeRoute(order.route).join(' → ');
                    return `<td class="px-4 py-3 whitespace-nowrap text-slate-600">${displayVal || '-'}${diffHtml}</td>`;
                  }

                  return `<td class="px-4 py-3 whitespace-nowrap">${val || '-'}${diffHtml}</td>`;
                }).join('')}
                ${isAdmin ? `
                  <td class="px-4 py-3 text-center whitespace-nowrap">
                    <button data-action="delete" data-id="${order.id}" class="text-xs text-rose-500 hover:bg-rose-50 px-2 py-1 rounded">刪除</button>
                  </td>
                ` : ''}
              </tr>
            `).join('')}
          </tbody>
        </table>
      </div>
    `;
  };

  let contentHtml = '';

  if (groupBy === 'status') {
    const statuses = AppState.getStatuses() || [];
    const grouped = {};
    statuses.forEach(s => grouped[s] = []);
    
    cleanOrders.forEach(o => {
      const s = o.status || '未知狀態';
      if (!grouped[s]) grouped[s] = [];
      grouped[s].push(o);
    });

    contentHtml = Object.keys(grouped).map(status => {
      const groupOrders = grouped[status];
      if (groupOrders.length === 0) return '';
      
      let listHtml = '';
      if (state.viewType === 'card') {
        listHtml = `<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 pb-2">${groupOrders.map(o => renderCard(o)).join('')}</div>`;
      } else {
        listHtml = renderTable(groupOrders);
      }

      return `
        <div class="mb-8 p-4 bg-white/50 border border-slate-200/60 rounded-[20px] shadow-sm">
          <div class="flex items-center gap-2 mb-3 ml-2">
            <h3 class="text-lg font-bold text-slate-800">${status}</h3>
            <span class="text-xs px-2 py-0.5 bg-brand/10 text-brand rounded-full font-bold">${groupOrders.length} 單</span>
          </div>
          ${listHtml}
        </div>
      `;
    }).join('');

  } else {
    if (state.viewType === 'card') {
      contentHtml = `<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 pb-4">${cleanOrders.map(o => renderCard(o)).join('')}</div>`;
    } else {
      contentHtml = renderTable(cleanOrders);
    }
  }

  container.innerHTML = viewToggleHtml + contentHtml;
  createIcons({ icons: { ArrowUpDown, ArrowUp, ArrowDown, Search, ChevronLeft, ChevronRight, Settings, Plus, AlertCircle }, attrs: { class: 'w-4 h-4' } });
}

/**
 * 取得欄位值 (處理巢狀 data 與日期格式防呆)
 * @param {Object} order 工單物件
 * @param {Object|string} field 欄位定義物件或欄位 key 字串
 * @returns {any} 欄位值
 */
function getOrderValue(order, field) {
  let key, type;
  
  if (typeof field === 'string') {
    key = field;
  } else if (field && typeof field === 'object') {
    key = field.key;
    type = field.type;
  } else {
    return '';
  }

  let val;

  if (key === 'status') {
    val = order.status || '未知';
  } else if (order.data && order.data[key] !== undefined) {
    val = order.data[key];
  } else {
    val = order[key] !== undefined ? order[key] : '';
  }

  // 日期格式化防呆：若為 date 類型，或已知的核心日期欄位，強制格式化
  if (type === 'date' || key === 'uploadDate' || key === 'dueDate' || key === 'statusUpdateDate') {
    return formatExcelDate(val);
  }

  return val;
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/ui/order-view.js
V: 1794188000
W: UPGRADE-009
═══ */```

<!-- === FILE END: ./src/ui/order-view.js === -->

<!-- === FILE START: ./src/ui/route-builder.js === -->

## 📄 ./src/ui/route-builder.js

```js
import { openModal } from './modal.js';
import { showToast } from './toast.js';
import { createIcons, Plus, GripVertical, X } from 'lucide';

/**
 * 建立並顯示「生產流程編輯器」的彈窗
 * @param {Object} options
 * @param {Object} options.currentSettings 當前的 settings 資料
 * @param {Array<string>} options.availableCategories 可選的類別清單
 * @param {string} options.defaultCategory 預設帶入的類別
 * @param {Function} options.onSave 儲存回呼 async (newTemplate) => {}
 */
export function openRouteBuilder({ currentSettings, availableCategories = [], defaultCategory = '', onSave }) {
  const statuses = currentSettings.statuses || [];
  const categories = [...new Set(availableCategories)].sort();

  const content = `
    <div class="flex flex-col gap-4">
      <div class="flex flex-col gap-1">
        <label class="text-sm font-medium text-slate-700">流程名稱</label>
        <input type="text" id="builder-name" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand" placeholder="例如：標準生產流程">
      </div>
      
      <div class="flex flex-col gap-1">
        <label class="text-sm font-medium text-slate-700">對應類別 (可多選，留空為通用)</label>
        <div class="flex flex-wrap gap-2 max-h-[100px] overflow-y-auto p-1">
          ${categories.length === 0 ? '<span class="text-xs text-slate-400">目前尚無工單類別</span>' : ''}
          ${categories.map(c => `
            <label class="flex items-center gap-1.5 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-sm cursor-pointer hover:border-brand">
              <input type="checkbox" class="builder-cat-checkbox w-4 h-4 text-brand rounded border-slate-300 focus:ring-brand" value="${c}" ${c === defaultCategory ? 'checked' : ''}>
              <span class="text-slate-700">${c}</span>
            </label>
          `).join('')}
        </div>
      </div>

      <div class="flex flex-col gap-2">
        <p class="text-sm text-slate-500">點擊左側加入右側，或拖曳右側調整順序。</p>
        <div class="flex flex-col md:flex-row gap-4 h-[35vh]">
          <!-- 左側：可選工序 -->
          <div class="flex-1 bg-slate-50 border border-slate-200 rounded-xl p-3 flex flex-col">
            <h4 class="font-bold text-sm text-slate-700 mb-2">可選工序</h4>
            <div class="flex-1 overflow-y-auto flex flex-col gap-2" id="builder-available-list"></div>
          </div>
          <!-- 右側：預計工序順序 -->
          <div class="flex-1 bg-white border border-slate-200 rounded-xl p-3 flex flex-col shadow-sm">
            <h4 class="font-bold text-sm text-slate-700 mb-2">預計工序順序</h4>
            <div class="flex-1 overflow-y-auto flex flex-col gap-2" id="builder-selected-list"></div>
          </div>
        </div>
      </div>
    </div>
  `;

  const footer = `
    <div class="flex justify-end gap-3">
      <button id="builder-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200">取消</button>
      <button id="builder-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-white bg-brand hover:bg-brand-hover shadow-md shadow-brand/20 active:scale-95 transition-all">儲存並套用</button>
    </div>
  `;

  // 加入 size: 'max-w-4xl' 參數
  const { close, element } = openModal({ title: '新增生產流程', content, footer, size: 'max-w-4xl' });

  let currentSteps = [];
  let sortableInstance = null;

  const renderLists = () => {
    const availableList = element.querySelector('#builder-available-list');
    const selectedList = element.querySelector('#builder-selected-list');

    const availableStatuses = statuses.filter(s => !currentSteps.includes(s));

    if (availableStatuses.length === 0) {
      availableList.innerHTML = '<div class="text-slate-400 text-sm text-center py-4">所有工序已加入</div>';
    } else {
      availableList.innerHTML = availableStatuses.map(status => `
        <div class="flex items-center gap-3 bg-white p-3 rounded-lg border border-slate-200 cursor-pointer hover:border-brand transition-colors builder-available-item" data-step="${status}">
          <span class="flex-1 font-medium text-slate-700">${status}</span>
          <i data-lucide="plus" class="w-4 h-4 text-slate-400"></i>
        </div>
      `).join('');
    }

    if (currentSteps.length === 0) {
      selectedList.innerHTML = '<div class="text-slate-400 text-sm text-center py-4">請從左側加入工序</div>';
    } else {
      selectedList.innerHTML = currentSteps.map((step, idx) => `
        <div class="flex items-center gap-3 bg-slate-50 p-3 rounded-lg border border-slate-200 builder-step-item" data-step="${step}">
          <i data-lucide="grip-vertical" class="w-4 h-4 text-slate-300 cursor-grab"></i>
          <span class="flex-1 font-medium text-slate-700"><span class="text-slate-400 mr-1">${idx + 1}.</span>${step}</span>
          <button class="text-slate-400 hover:text-rose-500 builder-remove-step" data-step="${step}">
            <i data-lucide="x" class="w-4 h-4"></i>
          </button>
        </div>
      `).join('');
    }

    createIcons({ icons: { Plus, GripVertical, X }, attrs: { class: 'w-4 h-4' } });

    // 左側點擊加入
    availableList.querySelectorAll('.builder-available-item').forEach(item => {
      item.addEventListener('click', () => {
        const step = item.dataset.step;
        if (!currentSteps.includes(step)) {
          currentSteps.push(step);
          renderLists();
        }
      });
    });

    // 右側移除
    selectedList.querySelectorAll('.builder-remove-step').forEach(btn => {
      btn.addEventListener('click', (ev) => {
        const step = ev.currentTarget.dataset.step;
        currentSteps = currentSteps.filter(s => s !== step);
        renderLists();
      });
    });

    // Sortable
    if (sortableInstance) sortableInstance.destroy();
    if (currentSteps.length > 0) {
      sortableInstance = new Sortable(selectedList, {
        animation: 150,
        handle: '.cursor-grab',
        ghostClass: 'opacity-50',
        forceFallback: true,
        fallbackTolerance: 3,
        onEnd: () => {
          currentSteps = Array.from(selectedList.children).map(el => el.dataset.step);
          renderLists();
        }
      });
    }
  };

  renderLists();

  element.querySelector('#builder-cancel-btn').addEventListener('click', close);

  element.querySelector('#builder-confirm-btn').addEventListener('click', async () => {
    const name = element.querySelector('#builder-name').value.trim();
    const selectedCategories = Array.from(element.querySelectorAll('.builder-cat-checkbox:checked')).map(cb => cb.value);

    if (!name) {
      showToast('請輸入流程名稱', 'error');
      return;
    }
    if (currentSteps.length === 0) {
      showToast('流程工序不能為空', 'error');
      return;
    }

    const btn = element.querySelector('#builder-confirm-btn');
    btn.disabled = true;
    btn.textContent = '儲存中...';

    try {
      const newTemplate = {
        name,
        category: selectedCategories.join(','),
        categories: selectedCategories,
        steps: currentSteps
      };

      await onSave(newTemplate);
      close();
    } catch (err) {
      console.error(err);
      showToast('儲存失敗', 'error');
      btn.disabled = false;
      btn.textContent = '儲存並套用';
    }
  });
}

/*
F: AI-WA-UPWO/src/ui/route-builder.js
V: 1792959000
W: UPGRADE-005
*/```

<!-- === FILE END: ./src/ui/route-builder.js === -->

<!-- === FILE START: ./src/ui/toast.js === -->

## 📄 ./src/ui/toast.js

```js
import { createIcons, CheckCircle2, Info, AlertCircle, X } from 'lucide';

/**
 * 顯示全域 Toast 提示
 * @param {string} message 提示訊息
 * @param {string} type 類型：'success' | 'info' | 'error' (預設 'success')
 * @param {number} duration 顯示時間（毫秒），預設 3000
 */
export function showToast(message, type = 'success', duration = 3000) {
  // 確保容器存在
  let container = document.getElementById('toast-container');
  if (!container) {
    container = document.createElement('div');
    container.id = 'toast-container';
    // 置中頂部，z-index 設高，pointer-events-none 讓不點擊到時不擋路
    container.className = 'fixed top-6 left-1/2 -translate-x-1/2 z-[100] flex flex-col gap-2 pointer-events-none items-center';
    document.body.appendChild(container);
  }

  // 建立 Toast 元素
  const toast = document.createElement('div');
  toast.className = 'bg-white/90 backdrop-blur-md border border-slate-200 shadow-soft rounded-xl px-4 py-3 flex items-center gap-3 text-sm font-medium text-slate-700 min-w-[250px] pointer-events-auto transition-all duration-300 transform -translate-y-4 opacity-0';
  
  // 設定圖示與顏色
  let iconHtml = '';
  let iconColor = 'text-emerald-500';

  if (type === 'success') {
    iconHtml = '<i data-lucide="check-circle-2" class="w-5 h-5"></i>';
  } else if (type === 'info') {
    iconHtml = '<i data-lucide="info" class="w-5 h-5"></i>';
    iconColor = 'text-blue-500';
  } else if (type === 'error') {
    iconHtml = '<i data-lucide="alert-circle" class="w-5 h-5"></i>';
    iconColor = 'text-rose-500';
  }

  toast.innerHTML = `
    <div class="${iconColor}">${iconHtml}</div>
    <span class="flex-1">${message}</span>
  `;

  container.appendChild(toast);

  // 渲染 Lucide 圖示
  // 注意：如果在 Vue/React 中，這裡會是響應式的，但我們是純 JS
  createIcons({
    icons: { CheckCircle2, Info, AlertCircle, X },
    attrs: { class: 'w-5 h-5' }
  });

  // 觸發動畫 (從上滑入)
  requestAnimationFrame(() => {
    toast.classList.remove('-translate-y-4', 'opacity-0');
    toast.classList.add('translate-y-0', 'opacity-100');
  });

  // 自動銷毀
  setTimeout(() => {
    // 觸發滑出動畫
    toast.classList.remove('translate-y-0', 'opacity-100');
    toast.classList.add('-translate-y-4', 'opacity-0');
    
    // 動畫結束後移除 DOM
    toast.addEventListener('transitionend', () => {
      toast.remove();
      // 如果容器空了，也一併移除（可選，保持 DOM 乾淨）
      if (container.children.length === 0) {
        container.remove();
      }
    });
  }, duration);
}

/* ═══ EOF ═══
F: src/ui/toast.js
V: 1792903508
W: webapp-20261026-001#20
═══ */```

<!-- === FILE END: ./src/ui/toast.js === -->

<!-- === FILE START: ./tailwind.config.js === -->

## 📄 ./tailwind.config.js

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,html}"
  ],
  theme: {
    extend: {
      colors: {
        brand: {
          DEFAULT: '#6366F1',
          hover: '#4F46E5',
        },
        bg: '#F4F7FB',
        inset: '#F8FAFC',
      },
      boxShadow: {
        'soft': '0 4px 20px -2px rgba(0,0,0,0.05)',
        'inset-soft': 'inset 0 2px 4px 0 rgba(0,0,0,0.02)',
      },
      borderRadius: {
        '2xl': '20px',
      }
    }
  },
  plugins: [],
}

/* ═══ EOF ═══
F: tailwind.config.js
V: 1792903500
W: webapp-20261026-001#11
═══ */```

<!-- === FILE END: ./tailwind.config.js === -->

<!-- === FILE START: ./vite.config.js === -->

## 📄 ./vite.config.js

```js
import { fileURLToPath, URL } from 'node:url';
import { defineConfig } from 'vite';

// https://vitejs.dev/config/
export default defineConfig({
  resolve: {
    alias: {
      // 設定 @ 指向 src 目錄，避免深層相對路徑 (../../)
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  server: {
    port: 3000,
    open: true
  }
});

/* ═══ EOF ═══
F: vite.config.js
V: 1792902801
W: webapp-20261026-001#4
═══ */```

<!-- === FILE END: ./vite.config.js === -->
