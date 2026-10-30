> 體系版本：SV11
> 生成時間：2026-10-30 10:31:08 (HKT)

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

  <!-- 系統啟動入口 (Vite 會自動處理 Module 依賴) -->
  <script type="module" src="/src/engine/main.js"></script>
</body>
</html>

<!-- ═══ EOF ═══
F: AI-WA-UPWO/index.html
V: 1792923100
W: UPGRADE-004
═══ -->```

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
}

// 匯出單一實例 (Singleton)
export const AppState = new State();

/* ═══ EOF ═══
F: src/config/state.js
V: 1792903521
W: webapp-20261026-001#33
═══ */```

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

<!-- === FILE START: ./src/domain/parser.js === -->

## 📄 ./src/domain/parser.js

```js
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
  if (!val) return "";
  
  // 如果已經是正常的日期字串 (包含 - 或 /)
  if (typeof val === 'string' && (val.includes('-') || val.includes('/'))) {
    return val.trim();
  }

  // 處理 Excel 序列號 (通常介於 20000 到 60000 之間)
  const num = Number(val);
  if (!isNaN(num) && num > 20000 && num < 80000) {
    // Excel 起始日為 1899-12-30
    const date = new Date((num - 25569) * 86400 * 1000);
    // 檢查是否為有效日期
    if (!isNaN(date.getTime())) {
      return date.toISOString().split('T')[0];
    }
  }

  // 其他無法解析的，直接轉字串回傳
  return String(val).trim();
}

/**
 * 解析 Excel 匯入的資料列（動態 Schema 版）
 * @param {Array<string>} headers Excel 第一列的標題
 * @param {Array<Array<any>>} rows Excel 的資料列（不含標題）
 * @param {Array} salesMapping 銷售員對照表
 * @param {string} defaultStatus 預設狀態 (如 "排產中")
 * @param {Array} schema 系統欄位定義
 * @returns {Array<Object>} 標準化後的工單物件陣列
 */
export function parseExcelData(headers, rows, salesMapping, defaultStatus = "排產中", schema = []) {
  const nowStr = new Date().toISOString();
  const parsedOrders = [];

  // 1. 建立表頭索引映射：強制清洗表頭的空白、換行與全形空白
  const cleanStr = (s) => String(s || '').replace(/[\s\u3000]+/g, '').trim();
  const cleanHeaders = headers.map(cleanStr);

  const columnMap = {};
  schema.forEach(field => {
    const targetLabel = cleanStr(field.label);
    const idx = cleanHeaders.findIndex(h => h === targetLabel);
    if (idx !== -1) {
      columnMap[field.key] = idx;
    }
  });

  // 2. 檢查必要的系統核心欄位是否存在
  if (columnMap['id'] === undefined) {
    throw new Error('找不到「工單號碼 (發票)」欄位，請確認 Excel 表頭或欄位設定。');
  }

  // 3. 逐列解析
  for (const row of rows) {
    // 跳過完全空白的列（防呆）
    if (!row || row.length === 0) continue;

    const rawIdVal = row[columnMap['id']];
    const orderId = rawIdVal !== undefined && rawIdVal !== null ? String(rawIdVal).trim() : "";
    
    // 💡 核心邏輯：遇到 A 欄（工單號碼）為空，直接中斷迴圈（完美剔除尾部的總計與備註）
    if (!orderId) {
      break;
    }

    // 解析日期（轉換 Excel 序列號）
    let uploadDate = nowStr;
    if (columnMap['uploadDate'] !== undefined && row[columnMap['uploadDate']] !== undefined) {
      uploadDate = formatExcelDate(row[columnMap['uploadDate']]);
    }

    // 解析銷售員
    let rawSalesRep = "";
    if (columnMap['salesRep'] !== undefined && row[columnMap['salesRep']] !== undefined) {
      rawSalesRep = String(row[columnMap['salesRep']]);
    }
    const normalizedReps = normalizeSalesRep(rawSalesRep, salesMapping);

    // 收集動態欄位資料 (非系統核心欄位)
    const data = {};
    
    // 💡 自動擷取工單號前兩碼英文作為類別
    const categoryMatch = orderId.match(/^[a-zA-Z]+/);
    data['category'] = categoryMatch ? categoryMatch[0].toUpperCase() : '其他';

    schema.forEach(field => {
      // 排除系統核心欄位 (id, uploadDate, salesRep 等)
      if (!field.isSystem && columnMap[field.key] !== undefined) {
        const val = row[columnMap[field.key]];
        data[field.key] = val !== undefined && val !== null ? val : "";
      }
    });

    parsedOrders.push({
      id: orderId,
      status: defaultStatus,
      salesReps: normalizedReps,
      uploadDate: uploadDate,
      statusUpdateDate: nowStr.split('T')[0], // 狀態更新日預設為今天
      dueDate: "", // 交期依需求為匯入後手動輸入
      data: data   // 動態欄位資料
    });
  }

  return parsedOrders;
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/domain/parser.js
V: 1792911500
W: UPGRADE-002
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
import '../ui/order-view-editor.js';

/**
 * 系統啟動入口
 * 負責等待 DOM 載入完成後，初始化路由、背景任務與 PWA
 */
document.addEventListener('DOMContentLoaded', () => {
  const root = document.getElementById('app-root');
  
  if (!root) {
    console.error('[System Error] 找不到 #app-root 容器，無法啟動系統。');
    return;
  }

  // 隱藏初始載入動畫
  const loadingEl = document.getElementById('app-loading');
  if (loadingEl) {
    loadingEl.classList.add('hidden');
  }

  try {
    // 1. 啟動路由系統
    initRouter();
    console.log('[System] UPGRADE-WO 系統啟動成功。');

    // 2. 全域監聽設定檔變更（確保工序、模板等即時同步）
    listenSettingsUpdates();

    // 3. 背景任務：自動修正歷史錯誤日期
    fixLegacyDatesInBackground();

    // 4. 註冊 Service Worker (PWA)
    registerServiceWorker();
    
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

/**
 * 全域監聽設定檔變更
 */
function listenSettingsUpdates() {
  listenDocument('settings', 'main', (data) => {
    if (!data) return;
    
    // 更新 AppState 中的全域狀態
    if (data.statuses) AppState.setStatuses(data.statuses);
    
    // 派發事件，通知所有前台頁面重新渲染
    window.dispatchEvent(new Event('settings-updated'));
  });
}

/**
 * 將 Excel 日期序列號轉換為 YYYY-MM-DD 格式
 */
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

/**
 * 背景自動修正歷史資料中的錯誤日期（例如 46300 轉為 YYYY-MM-DD）
 */
async function fixLegacyDatesInBackground() {
  try {
    const userStr = sessionStorage.getItem('upgrade_user');
    if (!userStr) return; // 未登入不執行
    
    const orders = await getCollection('work_orders');
    let fixCount = 0;

    for (const order of orders) {
      let needsUpdate = false;
      const updates = {};

      // 檢查 uploadDate
      if (order.uploadDate && (typeof order.uploadDate === 'number' || (typeof order.uploadDate === 'string' && !order.uploadDate.includes('-')))) {
        const formatted = formatDate(order.uploadDate);
        if (formatted !== order.uploadDate && formatted !== '') {
          updates.uploadDate = formatted;
          needsUpdate = true;
        }
      }

      // 檢查 dueDate
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
      // 觸發事件讓當前頁面重新渲染
      window.dispatchEvent(new Event('data:dates-fixed'));
    }

  } catch (error) {
    console.warn('[System Background Task] 自動修正日期時發生錯誤:', error);
  }
}

/**
 * 註冊 Service Worker 以啟用 PWA 功能
 */
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
V: 1792926100
W: UPGRADE-004
═══ */```

<!-- === FILE END: ./src/engine/main.js === -->

<!-- === FILE START: ./src/engine/router.js === -->

## 📄 ./src/engine/router.js

```js
import { AppState } from '../config/state.js';
import { renderAppShell, updateNavbar } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';

// 引入各頁面模組
import LoginPage from '../pages/login.js';
import KanbanPage from '../pages/kanban.js';
import TablePage from '../pages/table.js';
import OverviewPage from '../pages/overview.js';
import SalespersonPage from '../pages/salesperson.js';
import SettingsPage from '../pages/settings.js';
import CategoryPage from '../pages/category.js';
import AdminDataPage from '../pages/admin-data.js';

const routes = {
  login: LoginPage,
  kanban: KanbanPage,
  table: TablePage,
  overview: OverviewPage,
  salesperson: SalespersonPage,
  settings: SettingsPage,
  category: CategoryPage,
  'admin-data': AdminDataPage
};

let currentPageInstance = null;
let isShellRendered = false;

/**
 * 處理路由變更與守衛
 */
function handleRoute() {
  let hash = window.location.hash.replace('#', '') || 'kanban';
  const user = AppState.getUser();

  // 1. 登入守衛
  if (!user) {
    if (hash !== 'login') {
      window.location.hash = '#login';
      return;
    }
  } else {
    // 已登入卻試圖去登入頁
    if (hash === 'login') {
      window.location.hash = '#kanban';
      return;
    }
  }

  // 2. 權限守衛 (Settings 與 Admin-Data 僅限 admin)
  if ((hash === 'settings' || hash === 'admin-data') && user && user.role !== 'admin') {
    showToast('您沒有權限訪問此頁面', 'error');
    window.location.hash = '#kanban';
    return;
  }

  // 3. 渲染頁面
  const PageClass = routes[hash];
  if (!PageClass) {
    window.location.hash = user ? '#kanban' : '#login';
    return;
  }

  // 4. 處理 App Shell 與頁面切換
  if (hash === 'login') {
    isShellRendered = false; // 登出後重置狀態
    if (currentPageInstance && typeof currentPageInstance.destroy === 'function') {
      currentPageInstance.destroy();
    }
    currentPageInstance = PageClass;
    currentPageInstance.render();
  } else {
    // 需要 App Shell 的頁面
    if (!isShellRendered) {
      renderAppShell({
        user: user,
        onLogout: handleLogout,
        onNavigate: (route) => { window.location.hash = `#${route}`; }
      });
      isShellRendered = true;
    }

    // 更新導覽列高亮
    updateNavbar(hash);

    // 銷毀舊頁面
    if (currentPageInstance && typeof currentPageInstance.destroy === 'function') {
      currentPageInstance.destroy();
    }

    // 渲染新頁面
    currentPageInstance = PageClass;
    currentPageInstance.render();
  }
}

/**
 * 處理登出
 */
function handleLogout() {
  AppState.clearUser(); // 假設 state.js 有這個方法
  isShellRendered = false;
  window.location.hash = '#login';
  showToast('已登出系統', 'info');
}

/**
 * 初始化路由
 */
export function initRouter() {
  // 監聽 Hash 變更
  window.addEventListener('hashchange', handleRoute);

  // 監聽登入事件 (由 login.js 觸發)
  window.addEventListener('auth:login', () => {
    // 登入成功後，強制導向首頁
    window.location.hash = '#kanban';
  });

  // 首次載入
  handleRoute();
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/engine/router.js
V: 1792917800
W: UPGRADE-003
═══ */```

<!-- === FILE END: ./src/engine/router.js === -->

<!-- === FILE START: ./src/lib/io.js === -->

## 📄 ./src/lib/io.js

```js
import * as XLSX from 'xlsx';
import { jsPDF } from 'jspdf';
import { groupByStatus } from '../domain/stats.js';

/**
 * 將工單資料匯出為 Excel 檔案 (依狀態分 Sheet，包含動態欄位)
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} statuses 狀態清單
 * @param {string} filename 檔案名稱 (不含副檔名)
 * @param {Array} schema 系統欄位設定 (用於抓取自訂欄位)
 */
export function exportToExcel(orders, statuses, filename = '工單清單', schema = []) {
  if (!orders || orders.length === 0) {
    throw new Error('無資料可匯出');
  }

  const groupedData = groupByStatus(orders, statuses);
  const wb = XLSX.utils.book_new();

  // 過濾出非系統的自訂欄位
  const customFields = schema.filter(f => !f.isSystem);

  for (const status of statuses) {
    const statusOrders = groupedData[status] || [];
    if (statusOrders.length === 0) continue;

    // 整理要輸出的欄位
    const dataToExport = statusOrders.map(order => {
      // 基礎固定欄位
      const row = {
        '工單號碼': order.id,
        '類別': order.data?.category || '其他',
        '狀態': order.status,
        '銷售員': (order.salesReps || []).join(', '),
        '上傳日期': order.uploadDate || '',
        '狀態更新日': order.statusUpdateDate || '',
        '交貨日期': order.dueDate || ''
      };

      // 動態加入自訂欄位
      customFields.forEach(field => {
        row[field.label] = order.data && order.data[field.key] !== undefined ? order.data[field.key] : '';
      });

      return row;
    });

    // 建立工作表
    const ws = XLSX.utils.json_to_sheet(dataToExport);
    // 將工作表加入活頁簿，並以狀態名稱命名（Excel sheet 名稱限制 31 字元）
    XLSX.utils.book_append_sheet(wb, ws, status.substring(0, 31));
  }

  // 觸發下載
  XLSX.writeFile(wb, `${filename}_${new Date().toISOString().slice(0,10)}.xlsx`);
}

/**
 * 將工單資料匯出為 PDF 檔案 (依狀態分頁，包含動態欄位)
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

      // 如果有自訂欄位，可以在下方印出（避免版面太擠）
      const customFields = schema.filter(f => !f.isSystem);
      if (customFields.length > 0) {
        yPos += 5;
        const extraInfo = customFields.map(f => {
          const val = order.data && order.data[f.key] !== undefined ? order.data[f.key] : '-';
          return `${f.label}: ${val}`;
        }).join(' | ');
        doc.setFontSize(8);
        doc.setTextColor(100);
        doc.text(extraInfo.substring(0, 100), 14, yPos);
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

/* ═══ EOF ═══
F: AI-WA-UPWO/src/lib/io.js
V: 1792912500
W: UPGRADE-002
═══ */```

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

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/category.js
V: 1792926700
W: UPGRADE-004
═══ */```

<!-- === FILE END: ./src/pages/category.js === -->

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
import * as XLSX from 'xlsx';
import { createIcons, Upload } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let currentStatus = '排產中';
let currentCategory = 'ALL';
let schema = [];
let viewConfig = {};

export default {
  async render() {
    if (!AppState.getUser()) return;

    try {
      const settingsDoc = await getDocument('settings', 'main');
      if (settingsDoc) {
        schema = settingsDoc.schema || [];
        viewConfig = settingsDoc.view_configs || {};
        currentStatus = (settingsDoc.statuses && settingsDoc.statuses[0]) || '排產中';
      }
    } catch (err) {}

    renderPageLayout({
      title: '生產進度控制台',
      subtitle: '即時追蹤工單狀態與生產進度',
      headerActions: `
        <div class="flex items-center gap-3">
          <button id="btn-import-excel" class="bg-white text-slate-700 border border-slate-200 px-4 py-2 rounded-[10px] text-sm font-medium hover:bg-slate-50 flex items-center gap-2 shadow-sm">
            <i data-lucide="upload" class="w-4 h-4"></i> 匯入
          </button>
          <input type="file" id="excel-import-input" accept=".xlsx, .xls" class="hidden">
        </div>
      `,
      controls: `
        <div id="status-tabs" class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-5 lg:grid-cols-6 xl:grid-cols-7 gap-2 mb-4"></div>
        <div class="flex items-center gap-2 mb-4">
          <span class="text-sm font-medium text-slate-600">類別：</span>
          <select id="filter-category" class="px-3 py-2 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none shadow-sm min-w-[120px]"></select>
        </div>
      `,
      listContainerId: 'kanban-container'
    });

    document.getElementById('btn-import-excel')?.addEventListener('click', () => document.getElementById('excel-import-input').click());
    document.getElementById('excel-import-input')?.addEventListener('change', (e) => this.handleFileSelect(e));
    document.getElementById('filter-category')?.addEventListener('change', (e) => {
      currentCategory = e.target.value;
      this.updateUI();
    });

    window.addEventListener('refresh-kanban', this.updateUI.bind(this));
    window.addEventListener('settings-updated', this.updateUI.bind(this));
    createIcons({ icons: { Upload }, attrs: { class: 'w-4 h-4' } });

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
    const statuses = AppState.getStatuses() || [];
    const tabsContainer = document.getElementById('status-tabs');
    if (tabsContainer) {
      tabsContainer.innerHTML = statuses.map(s => `
        <button class="w-full flex justify-between items-center px-3 py-2 rounded-xl text-sm font-medium border ${s === currentStatus ? 'bg-brand text-white border-brand' : 'bg-white text-slate-600 border-slate-200'}" data-status="${s}">
          <span class="truncate pr-2">${s}</span>
          <span class="text-xs px-1.5 py-0.5 rounded-md flex-shrink-0 ${s === currentStatus ? 'bg-white/20' : 'bg-slate-100'}">${currentOrders.filter(o => o.status === s).length}</span>
        </button>
      `).join('');

      tabsContainer.querySelectorAll('button').forEach(btn => {
        btn.onclick = () => { currentStatus = btn.dataset.status; this.updateUI(); };
      });
    }

    const catSelect = document.getElementById('filter-category');
    if (catSelect) {
      const cats = Array.from(new Set(currentOrders.map(o => o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他'))).sort();
      catSelect.innerHTML = `<option value="ALL">全部類別</option>` + cats.map(c => `<option value="${c}" ${c === currentCategory ? 'selected' : ''}>${c}</option>`).join('');
    }

    let filtered = currentOrders.filter(o => o.status === currentStatus);
    if (currentCategory !== 'ALL') {
      filtered = filtered.filter(o => (o.data?.category || o.id.match(/^[a-zA-Z]+/)?.[0] || '其他') === currentCategory);
    }

    // 依使用者設定的順序對齊欄位
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

  async handleFileSelect(e) {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = async (evt) => {
      try {
        const workbook = XLSX.read(new Uint8Array(evt.target.result), { type: 'array' });
        const jsonData = XLSX.utils.sheet_to_json(workbook.Sheets[workbook.SheetNames[0]], { header: 1 });
        if (jsonData.length < 2) return showToast('無資料', 'error');

        showToast('解析中...', 'info');
        let salesMapping = [], statuses = AppState.getStatuses() || ['排產中'];
        let routeTemplates = [];
        
        try {
          const s = await getDocument('settings', 'main');
          if (s) { 
            salesMapping = s.sales_mapping || []; 
            statuses = s.statuses || statuses; 
            schema = s.schema || schema; 
            routeTemplates = s.route_templates || [];
          }
        } catch (err) {}

        const parsed = parseExcelData(jsonData[0], jsonData.slice(1), salesMapping, statuses[0], schema);
        let added = 0, updated = 0;

        for (const order of parsed) {
          const existing = await getDocument('work_orders', order.id);
          if (existing) {
            // 若存在，僅更新 Excel 相關欄位，保留狀態與製程
            await updateDocument('work_orders', order.id, { salesReps: order.salesReps, uploadDate: order.uploadDate, data: order.data });
            updated++;
          } else {
            // 新工單：依工單類別尋找對應的模板
            const orderCategory = order.data.category || '其他';
            
            // 優先找類別相符的，再找通用預設 (category 為空)
            let matchedTemplate = routeTemplates.find(t => t.category === orderCategory);
            if (!matchedTemplate) {
              matchedTemplate = routeTemplates.find(t => !t.category || t.category === '');
            }

            // 若找到模板且不為空，套用模板；否則套用全站狀態
            if (matchedTemplate && matchedTemplate.steps && matchedTemplate.steps.length > 0) {
              order.route = matchedTemplate.steps.slice();
              order.status = matchedTemplate.steps[0];
            } else {
              order.route = statuses.slice();
              order.status = statuses[0];
            }

            await setDocument('work_orders', order.id, order);
            added++;
          }
        }
        showToast(`匯入完成！新增 ${added}，更新 ${updated}`, 'success');
        e.target.value = '';
      } catch (error) {
        showToast(`失敗: ${error.message}`, 'error');
      }
    };
    reader.readAsArrayBuffer(file);
  },

  destroy() {
    if (unsubscribe) unsubscribe();
    window.removeEventListener('refresh-kanban', this.updateUI.bind(this));
    window.removeEventListener('settings-updated', this.updateUI.bind(this));
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/kanban.js
V: 1792926500
W: UPGRADE-004
═══ */```

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
          usersList = [{ username: "admin", password: "123456", role: "admin" }];
        }

        // 比對帳密
        const user = usersList.find(u => u.username === usernameInput && u.password === passwordInput);

        if (user) {
          // 登入成功
          showToast(`歡迎回來，${user.username}`, 'success');
          AppState.setUser({ username: user.username, role: user.role });
          
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

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/login.js
V: 1792909602
W: UPGRADE-002
═══ */```

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
import { groupByStatus, calculateStats, generatePivotTable } from '../domain/stats.js';
import { createIcons, Inbox, PieChart, LayoutDashboard } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let schema = [];
let viewConfig = {};

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
      title: '總狀態視圖',
      subtitle: '全廠工單狀態分佈與銷售員樞紐分析',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'overview-container'
    });

    window.addEventListener('refresh-overview', this.updateUI.bind(this));

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

    const statuses = AppState.getStatuses() || [];

    // 1. 上半部：狀態 / 銷售員 樞紐分析
    const allRepsSet = new Set();
    cleanOrders.forEach(o => {
      (o.salesReps || ['未填寫']).forEach(r => allRepsSet.add(r));
    });
    const allReps = Array.from(allRepsSet).sort();
    const pivotData = generatePivotTable(cleanOrders, statuses, allReps);

    let pivotHtml = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden mb-6">
        <div class="px-6 py-4 border-b border-slate-100 bg-slate-50/50 flex items-center gap-2">
          <i data-lucide="pie-chart" class="w-5 h-5 text-brand"></i>
          <h2 class="font-bold text-slate-800">狀態 / 銷售員 樞紐分析</h2>
        </div>
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse min-w-[600px]">
            <thead>
              <tr class="border-b border-slate-200/60 text-sm font-semibold text-slate-600 bg-slate-50/30">
                <th class="px-6 py-3 sticky left-0 bg-white/90 backdrop-blur z-10">工序狀態</th>
                ${allReps.map(rep => `<th class="px-4 py-3 text-center whitespace-nowrap">${rep}</th>`).join('')}
                <th class="px-6 py-3 text-center text-brand">總計</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-slate-100 text-sm text-slate-700">
              ${statuses.map(status => {
                const rowData = pivotData[status] || {};
                let rowTotal = 0;
                
                const cellsHtml = allReps.map(rep => {
                  const count = rowData[rep] || 0;
                  rowTotal += count;
                  return `<td class="px-4 py-3 text-center ${count > 0 ? 'font-bold text-slate-800' : 'text-slate-300'}">${count || '-'}</td>`;
                }).join('');

                return `
                  <tr class="hover:bg-slate-50/50 transition-colors">
                    <td class="px-6 py-3 font-medium text-slate-700 sticky left-0 bg-white/90 backdrop-blur z-10">${status}</td>
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

    // 2. 下半部：工單動態列表 (使用 order-view)
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
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/overview.js
V: 1792920300
W: UPGRADE-004
═══ */```

<!-- === FILE END: ./src/pages/overview.js === -->

<!-- === FILE START: ./src/pages/salesperson.js === -->

## 📄 ./src/pages/salesperson.js

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
let currentRep = '';

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
      title: '銷售員視圖',
      subtitle: '依銷售員檢視名下工單與狀態分佈',
      headerActions: '',
      controls: `
        <div id="rep-tabs" class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-5 lg:grid-cols-6 xl:grid-cols-7 gap-2 mb-4"></div>
      `,
      listContainerId: 'salesperson-container'
    });

    window.addEventListener('refresh-salesperson', this.updateUI.bind(this));
    window.addEventListener('settings-updated', this.updateUI.bind(this));

    this.initDataListener();
  },

  initDataListener() {
    if (unsubscribe) unsubscribe();
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      // 如果第一次載入，自動選中第一個銷售員
      if (!currentRep) {
        const reps = Array.from(new Set(currentOrders.flatMap(o => o.salesReps || ['未填寫']))).sort();
        if (reps.length > 0) currentRep = reps[0];
      }
      this.updateUI();
    });
  },

  updateUI() {
    // 提取所有銷售員
    const reps = Array.from(new Set(currentOrders.flatMap(o => o.salesReps || ['未填寫']))).sort();
    
    // 若當前選中的銷售員不存在了，重置為第一個
    if (reps.length > 0 && !reps.includes(currentRep)) {
      currentRep = reps[0];
    }

    const tabsContainer = document.getElementById('rep-tabs');
    if (tabsContainer) {
      tabsContainer.innerHTML = reps.map(r => `
        <button class="w-full flex justify-between items-center px-3 py-2 rounded-xl text-sm font-medium border ${r === currentRep ? 'bg-brand text-white border-brand' : 'bg-white text-slate-600 border-slate-200'}" data-rep="${r}">
          <span class="truncate pr-2">${r}</span>
          <span class="text-xs px-1.5 py-0.5 rounded-md flex-shrink-0 ${r === currentRep ? 'bg-white/20' : 'bg-slate-100'}">${currentOrders.filter(o => (o.salesReps || ['未填寫']).includes(r)).length}</span>
        </button>
      `).join('');

      tabsContainer.querySelectorAll('button').forEach(btn => {
        btn.onclick = () => { currentRep = btn.dataset.rep; this.updateUI(); };
      });
    }

    if (!currentRep) return;

    const filtered = currentOrders.filter(o => (o.salesReps || ['未填寫']).includes(currentRep));

    // 依使用者設定的順序對齊欄位
    const visibleFields = (viewConfig['salesperson'] || []).map(key => {
      const field = schema.find(f => f.key === key);
      return field ? Object.assign({}, field, { isVisible: true }) : null;
    }).filter(Boolean);

    renderOrderView({
      containerId: 'salesperson-container',
      orders: filtered,
      visibleFields: visibleFields,
      isAdmin: AppState.getUser()?.role === 'admin',
      pageKey: 'salesperson',
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
    window.removeEventListener('refresh-salesperson', this.updateUI.bind(this));
    window.removeEventListener('settings-updated', this.updateUI.bind(this));
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/salesperson.js
V: 1792926800
W: UPGRADE-004
═══ */```

<!-- === FILE END: ./src/pages/salesperson.js === -->

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
        <p class="text-sm text-slate-500 mt-1">可拖曳排序。修改名稱時會同步更新歷史工單。</p>
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
        <div class="flex items-center gap-3 bg-white p-3 rounded-xl border border-slate-100 shadow-sm status-item" draggable="true" data-index="${index}">
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
        message: `確定要刪除「${status}」嗎？系統將檢查是否有工單處於此狀態。`,
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
        await saveSettings();
        showToast('刪除成功', 'success');
      } catch (err) {
        showToast('檢查工單失敗', 'error');
      }
    });
  });

  // 拖曳排序
  const list = document.getElementById('status-list');
  if (list) {
    let draggedItem = null;

    list.querySelectorAll('.status-item').forEach(item => {
      item.addEventListener('dragstart', function() {
        draggedItem = this;
        setTimeout(() => this.classList.add('opacity-50'), 0);
      });
      item.addEventListener('dragend', function() {
        this.classList.remove('opacity-50');
        draggedItem = null;
        const newOrder = Array.from(list.querySelectorAll('.status-item')).map(el => el.querySelector('.status-name').textContent);
        currentSettings.statuses = newOrder;
        const saveHandler = async () => {
          await saveSettings();
          showToast('排序已更新', 'success');
        };
        saveHandler.call(this);
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

/**
 * 渲染製程模板管理頁籤
 */
export function renderRouteTemplates(container, currentSettings, saveSettings) {
  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">製程模板管理</h2>
        <p class="text-sm text-slate-500 mt-1">建立生產路線並綁定類別，匯入工單時會自動套用對應類別的模板。</p>
      </div>
    </div>

    <div class="flex flex-col sm:flex-row gap-2 mb-6">
      <input type="text" id="new-template-name" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="模板名稱 (例如：GF生產流程)">
      <input type="text" id="new-template-category" class="w-full sm:w-48 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="對應類別 (例：GF)">
      <button id="btn-add-template" class="btn-primary px-4 py-2.5 text-sm flex items-center justify-center gap-2 whitespace-nowrap">
        <i data-lucide="plus" class="w-4 h-4"></i> 新增模板
      </button>
    </div>

    <div class="flex flex-col gap-3">
      ${currentSettings.route_templates.map((tmpl, index) => `
        <div class="bg-white p-4 rounded-xl border border-slate-100 shadow-sm flex flex-col gap-3">
          <div class="flex items-center justify-between">
            <div class="flex items-center gap-2">
              <span class="font-bold text-slate-800 text-base">${tmpl.name}</span>
              ${tmpl.category 
                ? `<span class="px-2 py-0.5 bg-blue-50 text-blue-600 text-[10px] font-bold rounded">對應類別：${tmpl.category}</span>` 
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
      `).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, Trash2, Edit2 }, attrs: { class: 'w-4 h-4' } });

  // 新增模板
  document.getElementById('btn-add-template')?.addEventListener('click', async () => {
    const nameInput = document.getElementById('new-template-name');
    const catInput = document.getElementById('new-template-category');
    const name = nameInput.value.trim();
    const category = catInput.value.trim().toUpperCase();
    
    if (!name) return;
    if (currentSettings.route_templates.some(t => t.name === name)) {
      showToast('模板名稱已存在', 'error');
      return;
    }
    currentSettings.route_templates.push({ name, category, steps: [] });
    await saveSettings();
    showToast('模板已新增，請編輯工序', 'success');
  });

  // 刪除模板
  document.querySelectorAll('.btn-del-template').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const idx = e.currentTarget.dataset.index;
      const confirmed = await openConfirm({
        title: '刪除模板',
        message: '確定要刪除此模板嗎？',
        isDanger: true
      });
      if (!confirmed) return;
      currentSettings.route_templates.splice(idx, 1);
      await saveSettings();
      showToast('模板已刪除', 'success');
    });
  });

  // 編輯模板（左右雙清單與基本資料）
  document.querySelectorAll('.btn-edit-template').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const idx = parseInt(e.currentTarget.dataset.index);
      const tmpl = currentSettings.route_templates[idx];
      
      const content = `
        <div class="flex flex-col md:flex-row gap-4 mb-2">
          <div class="flex-1 flex flex-col gap-1">
            <label class="text-sm font-medium text-slate-700">模板名稱</label>
            <input type="text" id="edit-tmpl-name" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none" value="${tmpl.name}">
          </div>
          <div class="flex-1 flex flex-col gap-1">
            <label class="text-sm font-medium text-slate-700">對應類別 (留空為通用)</label>
            <input type="text" id="edit-tmpl-category" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none" value="${tmpl.category || ''}" placeholder="例：GF">
          </div>
        </div>

        <div class="flex flex-col gap-2 mt-4">
          <p class="text-sm text-slate-500">點擊左側加入右側，或拖曳右側調整順序。</p>
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

      const { close, element } = openModal({ title: `編輯模板`, content, footer });
      
      let currentSteps = Array.from(tmpl.steps);

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
          selectedList.innerHTML = currentSteps.map((step, idx) => `
            <div class="flex items-center gap-3 bg-slate-50 p-3 rounded-lg border border-slate-200 cursor-grab step-item" draggable="true" data-step="${step}">
              <i data-lucide="grip-vertical" class="w-4 h-4 text-slate-300"></i>
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

        // 綁定右側拖曳
        let dragged = null;
        selectedList.querySelectorAll('.step-item').forEach(item => {
          item.addEventListener('dragstart', function() {
            dragged = this;
            setTimeout(() => this.classList.add('opacity-50'), 0);
          });
          item.addEventListener('dragend', function() {
            this.classList.remove('opacity-50');
            dragged = null;
            currentSteps = Array.from(selectedList.querySelectorAll('.step-item')).map(el => el.dataset.step);
            renderModalLists(); // 重新渲染以更新序號
          });
          item.addEventListener('dragover', function(ev) {
            ev.preventDefault();
            if (this === dragged) return;
            const rect = this.getBoundingClientRect();
            const next = (ev.clientY - rect.top) / (rect.bottom - rect.top) > 0.5;
            selectedList.insertBefore(dragged, next ? this.nextSibling : this);
          });
        });
      };

      renderModalLists();

      element.querySelector('#modal-cancel-btn').addEventListener('click', close);

      element.querySelector('#modal-confirm-btn').addEventListener('click', async () => {
        const newName = element.querySelector('#edit-tmpl-name').value.trim();
        const newCategory = element.querySelector('#edit-tmpl-category').value.trim().toUpperCase();
        
        if (!newName) {
          showToast('模板名稱不能為空', 'error');
          return;
        }

        currentSettings.route_templates[idx].name = newName;
        currentSettings.route_templates[idx].category = newCategory;
        currentSettings.route_templates[idx].steps = currentSteps;
        
        await saveSettings();
        showToast('模板已更新', 'success');
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

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/settings-basic.js
V: 1792927200
W: UPGRADE-004
═══ */```

<!-- === FILE END: ./src/pages/settings-basic.js === -->

<!-- === FILE START: ./src/pages/settings-schema.js === -->

## 📄 ./src/pages/settings-schema.js

```js
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { createIcons, Plus, Trash2, Edit2, LayoutDashboard, Table, PieChart, Layers, Users, GripVertical, X, Check } from 'lucide';

let activeViewPage = 'kanban';

/**
 * 渲染欄位管理頁籤
 */
export function renderSchema(container, currentSettings, saveSettings) {
  const sysFields = currentSettings.schema.filter(f => f.isSystem);
  const customFields = currentSettings.schema.filter(f => !f.isSystem);

  container.innerHTML = `
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-bold text-slate-800">動態欄位管理</h2>
        <p class="text-sm text-slate-500 mt-1">定義系統欄位。新增後可至「視圖配置」決定顯示於何處。</p>
      </div>
    </div>

    <div class="flex flex-col sm:flex-row gap-2 mb-6">
      <input type="text" id="new-field-label" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="新增欄位名稱 (例如：利潤)">
      <select id="new-field-type" class="w-full sm:w-32 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none">
        <option value="string">文字</option>
        <option value="number">數字</option>
        <option value="date">日期</option>
      </select>
      <button id="btn-add-field" class="btn-primary px-4 py-2.5 text-sm flex items-center justify-center gap-2 whitespace-nowrap">
        <i data-lucide="plus" class="w-4 h-4"></i> 新增
      </button>
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
      ${customFields.map((f, index) => `
        <div class="flex items-center gap-4 bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
          <div class="w-1/3 font-bold text-slate-800 flex items-center gap-2">
            ${f.label}
            <span class="text-[10px] font-normal px-1.5 py-0.5 bg-slate-100 text-slate-500 rounded">${f.type === 'date' ? '日期' : f.type === 'number' ? '數字' : '文字'}</span>
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
      `).join('')}
    </div>
  `;

  createIcons({ icons: { Plus, Trash2, Edit2 }, attrs: { class: 'w-4 h-4' } });

  document.getElementById('btn-add-field')?.addEventListener('click', async () => {
    const input = document.getElementById('new-field-label');
    const typeSelect = document.getElementById('new-field-type');
    const val = input.value.trim();
    const type = typeSelect.value;
    
    if (!val) return;
    
    const key = 'field_' + Date.now().toString(36);
    currentSettings.schema.push({ key, label: val, type: type, isSystem: false, isEditable: true });
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
    { key: 'salesperson', label: '銷售員視圖', icon: 'users' }
  ];

  // 取得當前頁面已顯示的欄位（保持自訂順序）
  const activeFields = currentSettings.view_configs[activeViewPage] || [];
  const visibleSchema = activeFields.map(k => currentSettings.schema.find(f => f.key === k)).filter(Boolean);
  
  // 取得未顯示的欄位（存在於 schema 但不在 activeFields 中）
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
              <span class="flex-1 font-medium text-slate-700">${f.label}</span>
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
              <span class="flex-1 font-medium text-slate-600">${f.label}</span>
              <i data-lucide="plus" class="w-4 h-4 text-slate-400"></i>
            </div>
          `).join('')}
        </div>
      </div>
    </div>
  `;

  createIcons({ icons: { Check, Plus, GripVertical, X }, attrs: { class: 'w-4 h-4' } });

  // 頁面切換
  document.getElementById('view-page-select')?.addEventListener('change', (e) => {
    activeViewPage = e.target.value;
    renderViewConfig(container, currentSettings, saveSettings);
  });

  // 加入欄位 (從未顯示區)
  container.querySelectorAll('.hidden-item').forEach(item => {
    item.addEventListener('click', async () => {
      const key = item.dataset.key;
      currentSettings.view_configs[activeViewPage].push(key);
      await saveSettings();
      showToast('已加入顯示', 'success');
    });
  });

  // 移除欄位 (從已顯示區)
  container.querySelectorAll('.btn-remove-field').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      e.stopPropagation(); // 防止觸發拖曳
      const key = btn.dataset.key;
      currentSettings.view_configs[activeViewPage] = currentSettings.view_configs[activeViewPage].filter(k => k !== key);
      await saveSettings();
      showToast('已從顯示移除', 'success');
    });
  });

  // 拖曳排序
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
        
        // 收集新順序
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

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/settings-schema.js
V: 1792921400
W: UPGRADE-004
═══ */```

<!-- === FILE END: ./src/pages/settings-schema.js === -->

<!-- === FILE START: ./src/pages/settings.js === -->

## 📄 ./src/pages/settings.js

```js
import * as XLSX from 'xlsx';
import { AppState } from '../config/state.js';
import { listenDocument, getDocument, getCollection } from '../db/read.js';
import { setDocument, updateDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { openConfirm, openModal } from '../ui/modal.js';
import { parseExcelData } from '../domain/parser.js';
import { createIcons, Upload, Plus, Trash2, GripVertical, FileSpreadsheet, Users, Tag, Shield, X, List, Database, Edit2, AlertCircle, LayoutDashboard, Table, PieChart, Layers, Check } from 'lucide';

// 引入拆分出去的模組
import { renderStatuses, renderMapping, renderRouteTemplates } from './settings-basic.js';
import { renderSchema, renderViewConfig } from './settings-schema.js';

let unsubscribe = null;
export let currentSettings = { statuses: [], sales_mapping: [], users: [], schema: [], view_configs: {}, route_templates: [] };
let activeTab = 'statuses';

export default {
  async render() {
    const user = AppState.getUser();
    if (!user || user.role !== 'admin') {
      document.getElementById('app-content').innerHTML = '<div class="p-8 text-center text-rose-500 font-bold">您沒有權限訪問此頁面</div>';
      return;
    }

    renderPageLayout({
      title: '系統設定',
      subtitle: '管理工序狀態、銷售員對照表、系統欄位與帳號',
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
        currentSettings = {
          statuses: data.statuses || ['排產中'],
          sales_mapping: data.sales_mapping || [],
          users: data.users || [{ username: 'admin', password: '123456', role: 'admin' }],
          schema: data.schema || this.getDefaultSchema(),
          view_configs: data.view_configs || this.getDefaultViewConfigs(),
          route_templates: data.route_templates || this.getDefaultRouteTemplates(data.statuses || ['排產中'])
        };
        
        // 自動補齊核心欄位與視圖配置
        const requiredSysFields = [
          { key: 'id', label: '發票', type: 'string', isSystem: true, isEditable: false },
          { key: 'uploadDate', label: '日期', type: 'date', isSystem: true, isEditable: false },
          { key: 'salesRep', label: '我方備考', type: 'string', isSystem: true, isEditable: true },
          { key: 'status', label: '生產進度', type: 'string', isSystem: true, isEditable: false },
          { key: 'dueDate', label: '交貨日期', type: 'date', isSystem: true, isEditable: true },
          { key: 'statusUpdateDate', label: '更新日期', type: 'date', isSystem: true, isEditable: false },
          { key: 'route', label: '製程路線', type: 'string', isSystem: true, isEditable: false }
        ];

        requiredSysFields.forEach(req => {
          if (!currentSettings.schema.some(f => f.key === req.key)) {
            currentSettings.schema.push(req);
            needSave = true;
          }
        });

        Object.keys(currentSettings.view_configs).forEach(page => {
          if (!currentSettings.view_configs[page].includes('status')) {
            currentSettings.view_configs[page].unshift('status');
            needSave = true;
          }
          if (!currentSettings.view_configs[page].includes('dueDate') && page !== 'kanban') {
            currentSettings.view_configs[page].push('dueDate');
            needSave = true;
          }
        });

        if (!currentSettings.view_configs['overview']) {
          currentSettings.view_configs['overview'] = ['id', 'status', 'salesRep', 'dueDate'];
          needSave = true;
        }

        // 💡 資料遷移與修復：確保模板皆有 category 欄位，且通用預設模板不會是空的
        let hasEmptyDefault = false;
        currentSettings.route_templates.forEach(t => {
          if (t.category === undefined) {
            t.category = '';
            needSave = true;
          }
          // 如果發現通用模板（category 為空）卻沒有工序，將其補齊
          if ((t.category === '' || t.category === '通用') && (!t.steps || t.steps.length === 0)) {
            t.steps = [...currentSettings.statuses];
            hasEmptyDefault = true;
            needSave = true;
          }
        });

        // 如果完全沒有通用預設模板，強制建立一個
        if (!currentSettings.route_templates.some(t => t.category === '' || t.category === '通用')) {
          currentSettings.route_templates.unshift({
            name: '預設常規路線',
            category: '',
            steps: [...currentSettings.statuses]
          });
          needSave = true;
        }

      } else {
        currentSettings = {
          statuses: ['排產中', '印刷', '裁切', '品檢', '出貨'],
          sales_mapping: [],
          users: [{ username: 'admin', password: '123456', role: 'admin' }],
          schema: this.getDefaultSchema(),
          view_configs: this.getDefaultViewConfigs(),
          route_templates: this.getDefaultRouteTemplates()
        };
        needSave = true;
      }
      
      if (needSave) {
        await setDocument('settings', 'main', currentSettings);
      }

      AppState.setStatuses(currentSettings.statuses);
      this.renderTabs();
    });
  },

  getDefaultSchema() {
    return [
      { key: 'id', label: '發票', type: 'string', isSystem: true, isEditable: false },
      { key: 'uploadDate', label: '日期', type: 'date', isSystem: true, isEditable: false },
      { key: 'salesRep', label: '我方備考', type: 'string', isSystem: true, isEditable: true },
      { key: 'status', label: '生產進度', type: 'string', isSystem: true, isEditable: false },
      { key: 'dueDate', label: '交貨日期', type: 'date', isSystem: true, isEditable: true },
      { key: 'statusUpdateDate', label: '更新日期', type: 'date', isSystem: true, isEditable: false },
      { key: 'route', label: '製程路線', type: 'string', isSystem: true, isEditable: false },
      { key: 'amount', label: '金額', type: 'number', isSystem: false, isEditable: true },
      { key: 'customCategory', label: '類別', type: 'string', isSystem: false, isEditable: true },
      { key: 'customerNameZh', label: '中文名稱', type: 'string', isSystem: false, isEditable: true },
      { key: 'customerNameEn', label: '名稱', type: 'string', isSystem: false, isEditable: true }
    ];
  },

  getDefaultRouteTemplates(statuses = ['排產中', '印刷', '裁切', '品檢', '出貨']) {
    return [
      { name: '預設常規路線', category: '', steps: [...statuses] }
    ];
  },

  getDefaultViewConfigs() {
    return {
      kanban: ['id', 'status', 'customerNameZh', 'salesRep', 'dueDate'],
      table: ['id', 'customCategory', 'status', 'uploadDate', 'dueDate', 'amount', 'customerNameZh', 'customerNameEn'],
      category: ['id', 'customCategory', 'status', 'dueDate'],
      salesperson: ['id', 'customCategory', 'status', 'dueDate'],
      overview: ['id', 'status', 'salesRep', 'dueDate']
    };
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
          <button class="tab-btn ${activeTab === 'route_templates' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="route_templates">
            <i data-lucide="layers" class="w-4 h-4"></i> 製程模板
          </button>
          <button class="tab-btn ${activeTab === 'mapping' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="mapping">
            <i data-lucide="users" class="w-4 h-4"></i> 對照表管理
          </button>
          <button class="tab-btn ${activeTab === 'schema' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="schema">
            <i data-lucide="list" class="w-4 h-4"></i> 欄位管理
          </button>
          
          <div class="border-t border-slate-200/60 my-2"></div>
          
          <button class="tab-btn ${activeTab === 'view_config' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="view_config">
            <i data-lucide="layout-dashboard" class="w-4 h-4"></i> 視圖配置
          </button>
          
          <div class="border-t border-slate-200/60 my-2"></div>
          
          <button class="tab-btn ${activeTab === 'users' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="users">
            <i data-lucide="shield" class="w-4 h-4"></i> 帳號管理
          </button>
          <button class="tab-btn ${activeTab === 'data' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="data">
            <i data-lucide="database" class="w-4 h-4"></i> 資料管理
          </button>
          <button class="tab-btn ${activeTab === 'import' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="import">
            <i data-lucide="file-spreadsheet" class="w-4 h-4"></i> 匯入 Excel
          </button>
        </div>

        <div class="flex-1 p-6 md:p-8" id="settings-content"></div>
      </div>
    `;

    createIcons({
      icons: { Tag, Users, Shield, FileSpreadsheet, List, Database, LayoutDashboard, Layers },
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

    // 委派給拆分出去的模組
    if (activeTab === 'statuses') {
      renderStatuses(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'route_templates') {
      renderRouteTemplates(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'mapping') {
      renderMapping(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'schema') {
      renderSchema(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'view_config') {
      renderViewConfig(content, currentSettings, this.saveSettings.bind(this));
    } else if (activeTab === 'users') {
      this.renderUsers(content);
    } else if (activeTab === 'data') {
      this.renderDataManagement(content);
    } else if (activeTab === 'import') {
      this.renderImport(content);
    }
  },

  renderUsers(container) {
    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">帳號管理</h2>
          <p class="text-sm text-slate-500 mt-1">設定登入系統的帳號與角色。</p>
        </div>
      </div>

      <div class="flex gap-2 mb-6">
        <input type="text" id="new-user-name" class="w-1/3 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="帳號">
        <input type="text" id="new-user-pass" class="w-1/3 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="密碼">
        <select id="new-user-role" class="w-1/6 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none">
          <option value="user">一般使用者</option>
          <option value="admin">管理員</option>
        </select>
        <button id="btn-add-user" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增
        </button>
      </div>

      <div class="flex flex-col gap-2">
        ${currentSettings.users.map((u, index) => `
          <div class="flex items-center gap-4 bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
            <div class="w-1/3 font-bold text-slate-800">${u.username}</div>
            <div class="w-1/3 text-slate-500 font-mono text-sm">••••••••</div>
            <div class="w-1/6">
              <span class="px-2 py-1 text-xs font-medium rounded-md ${u.role === 'admin' ? 'bg-brand/10 text-brand' : 'bg-slate-100 text-slate-600'}">${u.role}</span>
            </div>
            <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-user" data-index="${index}" ${u.username === 'admin' ? 'disabled title="無法刪除預設 admin"' : ''}>
              <i data-lucide="trash-2" class="w-4 h-4"></i>
            </button>
          </div>
        `).join('')}
      </div>
    `;

    createIcons({ icons: { Plus, Trash2 }, attrs: { class: 'w-4 h-4' } });

    document.getElementById('btn-add-user')?.addEventListener('click', async () => {
      const name = document.getElementById('new-user-name').value.trim();
      const pass = document.getElementById('new-user-pass').value.trim();
      const role = document.getElementById('new-user-role').value;
      
      if (!name || !pass) {
        showToast('請填寫帳號與密碼', 'error');
        return;
      }
      if (currentSettings.users.some(u => u.username === name)) {
        showToast('帳號已存在', 'error');
        return;
      }
      currentSettings.users.push({ username: name, password: pass, role });
      await this.saveSettings();
      showToast('新增成功', 'success');
    });

    document.querySelectorAll('.btn-del-user').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        if (e.currentTarget.disabled) return;
        const idx = e.currentTarget.dataset.index;
        currentSettings.users.splice(idx, 1);
        await this.saveSettings();
        showToast('刪除成功', 'success');
      });
    });
  },

  renderDataManagement(container) {
    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">資料管理</h2>
          <p class="text-sm text-slate-500 mt-1">清除由 Excel 匯入的資料，保留您手動輸入的狀態與交期。</p>
        </div>
      </div>

      <div class="bg-rose-50 border border-rose-200 rounded-2xl p-6">
        <h3 class="text-rose-700 font-bold mb-2 flex items-center gap-2">
          <i data-lucide="alert-circle" class="w-5 h-5"></i> 危險操作區
        </h3>
        <p class="text-sm text-rose-600 mb-4">
          點擊下方按鈕將會清空所有工單中「由 Excel 匯入」的資料。清空後，您可以重新匯入新的 Excel，系統會自動合併狀態。
        </p>
        <button id="btn-clear-excel-data" class="bg-rose-500 hover:bg-rose-600 text-white px-5 py-2.5 rounded-xl text-sm font-medium transition-colors shadow-sm flex items-center gap-2">
          <i data-lucide="trash-2" class="w-4 h-4"></i> 清空 Excel 匯入資料
        </button>
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

      showToast('正在清空資料...', 'info');
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
  },

  renderImport(container) {
    container.innerHTML = `
      <div class="flex flex-col items-center justify-center h-full min-h-[300px] border-2 border-dashed border-slate-200 rounded-2xl bg-slate-50/50 p-8 text-center relative" id="drop-zone">
        <input type="file" id="excel-input" accept=".xlsx, .xls" class="absolute inset-0 w-full h-full opacity-0 cursor-pointer">
        <div class="w-16 h-16 bg-white rounded-2xl shadow-sm flex items-center justify-center mb-4 text-brand">
          <i data-lucide="file-spreadsheet" class="w-8 h-8"></i>
        </div>
        <h3 class="text-lg font-bold text-slate-800 mb-2">點擊或拖曳 Excel 檔案至此</h3>
        <p class="text-sm text-slate-500 max-w-sm">系統將自動解析並存入資料庫。若單號已存在，將會更新 Excel 資料並保留手動狀態。</p>
      </div>
    `;

    createIcons({ icons: { FileSpreadsheet }, attrs: { class: 'w-8 h-8' } });

    const fileInput = document.getElementById('excel-input');
    fileInput?.addEventListener('change', async (e) => {
      const file = e.target.files[0];
      if (!file) return;
      
      const reader = new FileReader();
      reader.onload = async (evt) => {
        try {
          const data = new Uint8Array(evt.target.result);
          const workbook = XLSX.read(data, { type: 'array' });
          const firstSheetName = workbook.SheetNames[0];
          const worksheet = workbook.Sheets[firstSheetName];
          
          const jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1 });
          if (jsonData.length < 2) {
            showToast('Excel 檔案沒有資料', 'error');
            return;
          }

          const headers = jsonData[0];
          const rows = jsonData.slice(1);
          showToast('解析中，請稍候...', 'info');

          const parsedOrders = parseExcelData(headers, rows, currentSettings.sales_mapping, currentSettings.statuses[0] || '排產中', currentSettings.schema);

          let addedCount = 0;
          let updatedCount = 0;

          for (const order of parsedOrders) {
            const existing = await getDocument('work_orders', order.id);
            if (existing) {
              await updateDocument('work_orders', order.id, {
                salesReps: order.salesReps,
                uploadDate: order.uploadDate,
                data: order.data
              });
              updatedCount++;
            } else {
              await setDocument('work_orders', order.id, order);
              addedCount++;
            }
          }

          showToast(`匯入完成！新增 ${addedCount} 筆，更新 ${updatedCount} 筆。`, 'success');
          fileInput.value = ''; 
        } catch (error) {
          console.error('Import Error:', error);
          showToast(`匯入失敗：${error.message || '請檢查檔案格式'}`, 'error');
        }
      };
      reader.readAsArrayBuffer(file);
    });
  },

  async saveSettings() {
    await setDocument('settings', 'main', currentSettings);
    this.renderTabs();
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/settings.js
V: 1792927100
W: UPGRADE-004
═══ */```

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
import { createIcons, Download, FileText } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let schema = [];
let viewConfig = {};
let currentCategory = 'ALL';

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
      title: '管理總表',
      subtitle: '所有工單的狀態與時間追蹤列表',
      headerActions: `
        <div class="flex items-center gap-2">
          <button id="btn-export-excel" class="bg-white text-slate-700 border border-slate-200 px-3 py-2 rounded-[10px] text-sm font-medium hover:bg-slate-50 flex items-center gap-2 shadow-sm">
            <i data-lucide="download" class="w-4 h-4"></i> Excel
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

    document.getElementById('btn-export-excel')?.addEventListener('click', () => this.handleExport('excel'));
    document.getElementById('btn-export-pdf')?.addEventListener('click', () => this.handleExport('pdf'));
    document.getElementById('filter-category')?.addEventListener('change', (e) => {
      currentCategory = e.target.value;
      this.updateUI();
    });

    window.addEventListener('refresh-table', this.updateUI.bind(this));
    window.addEventListener('settings-updated', this.updateUI.bind(this));
    createIcons({ icons: { Download, FileText }, attrs: { class: 'w-4 h-4' } });

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

    // 依使用者設定的順序對齊欄位
    const visibleFields = (viewConfig['table'] || []).map(key => {
      const field = schema.find(f => f.key === key);
      return field ? Object.assign({}, field, { isVisible: true }) : null;
    }).filter(Boolean);

    renderOrderView({
      containerId: 'table-container',
      orders: filtered,
      visibleFields: visibleFields,
      isAdmin: AppState.getUser()?.role === 'admin',
      pageKey: 'table',
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
      if (type === 'excel') exportToExcel(currentOrders, statuses, '工單總表', schema);
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
V: 1792926600
W: UPGRADE-004
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
import { createIcons, Layers, Bell, LogOut, LayoutDashboard, Table, PieChart, Users, Settings, Activity, ClipboardList, Database } from 'lucide';

/**
 * 繪製 App Shell 骨架（側邊欄、頂部導覽列）
 * @param {Object} options
 * @param {Object} options.user 當前登入使用者 (需含 username, role)
 * @param {function} options.onLogout 登出回呼
 * @param {function} options.onNavigate 切換視圖回呼 (接收 route)
 */
export function renderAppShell({ user, onLogout, onNavigate }) {
  const root = document.getElementById('app-root');
  if (!root) return;

  // 清空並建立新的骨架
  root.innerHTML = `
    <div class="flex h-screen w-full bg-bg overflow-hidden">
      <!-- 側邊欄 (Desktop) -->
      <aside class="w-64 bg-white border-r border-slate-200/50 flex-col hidden md:flex z-20">
        <div class="h-16 flex items-center px-6 border-b border-slate-100">
          <div class="flex items-center gap-3">
            <div class="w-8 h-8 rounded-lg bg-brand flex items-center justify-center text-white shadow-sm shadow-brand/30">
              <i data-lucide="layers" class="w-5 h-5"></i>
            </div>
            <span class="font-bold text-lg tracking-tight text-slate-900">UPGRADE-WO</span>
          </div>
        </div>
        
        <nav class="flex-1 py-4 px-3 flex flex-col gap-1 overflow-y-auto" id="sidebar-nav">
          <!-- 導覽按鈕由 renderNavItems 注入 -->
        </nav>

        <div class="p-4 border-t border-slate-100">
          <button id="btn-logout" class="flex items-center gap-3 w-full px-3 py-2.5 rounded-xl text-sm font-medium text-slate-600 hover:bg-rose-50 hover:text-rose-600 transition-colors">
            <i data-lucide="log-out" class="w-5 h-5"></i>
            登出系統
          </button>
        </div>
      </aside>

      <!-- 主內容區 -->
      <div class="flex-1 flex flex-col h-full overflow-hidden relative">
        <!-- 頂部導覽 (Mobile & Desktop) -->
        <header class="h-16 bg-white/80 backdrop-blur-md border-b border-slate-200/50 flex items-center justify-between px-4 sm:px-6 z-10">
          <!-- Mobile Logo & Menu Button -->
          <div class="flex items-center gap-3 md:hidden">
            <button id="btn-mobile-menu" class="p-2 -ml-2 text-slate-600 hover:bg-slate-100 rounded-lg transition-colors">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-menu"><line x1="4" x2="20" y1="12" y2="12"/><line x1="4" x2="20" y1="6" y2="6"/><line x1="4" x2="20" y1="18" y2="18"/></svg>
            </button>
            <div class="w-8 h-8 rounded-lg bg-brand flex items-center justify-center text-white shadow-sm">
              <i data-lucide="layers" class="w-5 h-5"></i>
            </div>
            <span class="font-bold text-slate-900">UPGRADE</span>
          </div>

          <!-- 中間：iOS Segmented Control (視圖切換) - Desktop -->
          <div class="hidden md:flex segmented-control" id="top-segmented-control">
            <!-- 由 renderNavItems 動態注入 -->
          </div>

          <!-- 右側：使用者資訊 -->
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

        <!-- 頁面渲染容器 -->
        <main id="app-content" class="flex-1 overflow-y-auto p-4 sm:p-6 lg:p-8 flex flex-col gap-6">
          <!-- 各頁面內容會注入這裡 -->
        </main>
      </div>
    </div>

    <!-- 手機版側邊選單 (Drawer) -->
    <div id="mobile-drawer" class="fixed inset-0 z-50 hidden">
      <div class="absolute inset-0 bg-black/40 backdrop-blur-sm transition-opacity" id="drawer-overlay"></div>
      <aside class="absolute left-0 top-0 bottom-0 w-64 bg-white shadow-2xl transform -translate-x-full transition-transform duration-300 flex flex-col" id="drawer-panel">
        <div class="h-16 flex items-center px-6 border-b border-slate-100 justify-between">
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
        <nav class="flex-1 py-4 px-3 flex flex-col gap-1 overflow-y-auto" id="mobile-sidebar-nav">
          <!-- 由 renderNavItems 注入 -->
        </nav>
      </aside>
    </div>
  `;

  // 渲染圖示
  createIcons({
    icons: { Layers, Bell, LogOut, LayoutDashboard, Table, PieChart, Users, Settings, Database },
    attrs: { class: 'w-5 h-5' }
  });

  // 綁定事件
  document.getElementById('btn-logout')?.addEventListener('click', onLogout);

  // 手機版抽屜選單事件
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

  // 渲染導覽選單
  renderNavItems(user.role, onNavigate, closeDrawer);
}

/**
 * 繪製導覽選單項目
 */
function renderNavItems(role, onNavigate, closeDrawer) {
  const routes = [
    { id: 'kanban', label: '工單進度表', icon: 'layout-dashboard', roles: ['admin', 'user'] },
    { id: 'table', label: '管理總表', icon: 'table', roles: ['admin', 'user'] },
    { id: 'overview', label: '總狀態', icon: 'pie-chart', roles: ['admin', 'user'] },
    { id: 'category', label: '類別視圖', icon: 'layers', roles: ['admin', 'user'] },
    { id: 'salesperson', label: '銷售員', icon: 'users', roles: ['admin', 'user'] },
    { id: 'settings', label: '系統設定', icon: 'settings', roles: ['admin'] },
    { id: 'admin-data', label: '資料總管', icon: 'database', roles: ['admin'] }
  ];

  const sidebarNav = document.getElementById('sidebar-nav');
  const topControl = document.getElementById('top-segmented-control');
  const mobileNav = document.getElementById('mobile-sidebar-nav');

  routes.forEach(route => {
    if (!route.roles.includes(role)) return;

    // 側邊欄項目
    if (sidebarNav) {
      const btn = document.createElement('button');
      btn.className = 'nav-item flex items-center gap-3 w-full px-3 py-2.5 rounded-xl text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-brand transition-colors';
      btn.dataset.route = route.id;
      btn.innerHTML = `
        <i data-lucide="${route.icon}" class="w-5 h-5"></i>
        ${route.label}
      `;
      btn.addEventListener('click', () => onNavigate(route.id));
      sidebarNav.appendChild(btn);
    }

    // 手機版側邊欄項目
    if (mobileNav) {
      const btn = document.createElement('button');
      btn.className = 'nav-item flex items-center gap-3 w-full px-3 py-2.5 rounded-xl text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-brand transition-colors';
      btn.dataset.route = route.id;
      btn.innerHTML = `
        <i data-lucide="${route.icon}" class="w-5 h-5"></i>
        ${route.label}
      `;
      btn.addEventListener('click', () => {
        onNavigate(route.id);
        if (typeof closeDrawer === 'function') closeDrawer();
      });
      mobileNav.appendChild(btn);
    }

    // 頂部 Segmented Control 項目 (排除 admin-data 以免太擠)
    if (topControl && route.id !== 'admin-data') {
      const segment = document.createElement('div');
      segment.className = 'segment-btn';
      segment.dataset.route = route.id;
      segment.textContent = route.label;
      segment.addEventListener('click', () => onNavigate(route.id));
      topControl.appendChild(segment);
    }
  });

  createIcons({
    icons: { LayoutDashboard, Table, PieChart, Users, Settings, Layers, Database },
    attrs: { class: 'w-5 h-5' }
  });
}

/**
 * 更新當前選中的導覽狀態
 * @param {string} activeRoute 
 */
export function updateNavbar(activeRoute) {
  // 更新側邊欄與手機版側邊欄
  document.querySelectorAll('.nav-item').forEach(item => {
    if (item.dataset.route === activeRoute) {
      item.classList.add('bg-brand/5', 'text-brand', 'font-bold');
      item.classList.remove('text-slate-600');
    } else {
      item.classList.remove('bg-brand/5', 'text-brand', 'font-bold');
      item.classList.add('text-slate-600');
    }
  });

  // 更新頂部 Segmented Control
  document.querySelectorAll('#top-segmented-control .segment-btn').forEach(item => {
    if (item.dataset.route === activeRoute) {
      item.classList.add('active');
    } else {
      item.classList.remove('active');
    }
  });
}

/**
 * 統一的頁面佈局渲染
 * @param {Object} options
 * @param {string} options.title 頁面標題
 * @param {string} options.subtitle 副標題
 * @param {string} options.headerActions 右上角操作按鈕 HTML
 * @param {Array} options.stats 統計卡片資料
 * @param {string} options.controls 控制列 HTML
 * @param {string} options.listContainerId 內容容器 ID
 */
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
    <!-- 標題與操作區 -->
    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 mb-2">
      <div>
        <h1 class="text-2xl font-bold text-slate-900">${title}</h1>
        ${subtitle ? `<p class="text-sm text-slate-500 mt-1">${subtitle}</p>` : ''}
      </div>
      <div class="flex items-center gap-3">
        ${headerActions}
      </div>
    </div>

    <!-- 統計卡片 -->
    ${statsHtml}

    <!-- 控制列 -->
    ${controls ? `<div class="w-full">${controls}</div>` : ''}

    <!-- 主內容容器 -->
    <div id="${listContainerId}" class="flex-1 flex flex-col relative w-full">
      <!-- 內容由各頁面自行渲染 -->
    </div>
  `;

  // 重新渲染圖示（補上缺少的參數）
  createIcons({
    icons: { Activity, ClipboardList },
    attrs: { class: 'w-4 h-4' }
  });
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/ui/layout.js
V: 1792917900
W: UPGRADE-003
═══ */```

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
 * @param {function} options.onClose 關閉時的回呼
 * @returns {Object} 包含 close 方法的物件，可手動關閉
 */
export function openModal({ title, content, footer = '', onClose }) {
  // 建立遮罩與容器
  const overlay = document.createElement('div');
  overlay.className = 'fixed inset-0 bg-black/40 backdrop-blur-sm z-50 flex items-center justify-center transition-opacity duration-300 opacity-0 p-4';
  
  const modal = document.createElement('div');
  // 使用 flex flex-col 與 max-h-[90vh] 確保內容不會撐破螢幕
  modal.className = 'bg-white border border-slate-100 shadow-soft rounded-[20px] w-full max-w-md flex flex-col max-h-[90vh] transform transition-transform duration-300 scale-95 overflow-hidden';
  
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

/* ═══ EOF ═══
F: AI-WA-UPWO/src/ui/modal.js
V: 1792927000
W: UPGRADE-004
═══ */```

<!-- === FILE END: ./src/ui/modal.js === -->

<!-- === FILE START: ./src/ui/order-view-editor.js === -->

## 📄 ./src/ui/order-view-editor.js

```js
import { updateDocument } from '../db/write.js';
import { getDocument } from '../db/read.js';
import { showToast } from '../ui/toast.js';
import { openModal } from '../ui/modal.js';
import { AppState } from '../config/state.js';
import { createIcons, GripVertical, Plus, X } from 'lucide';

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

    await updateDocument('work_orders', id, updateData);
    showToast(`已更新資料`, 'success');
    
  } catch (error) {
    console.error('[Inline Edit Error]', error);
    showToast('更新失敗，請檢查網路或稍後再試', 'error');
  }
});

/**
 * 處理全域點擊事件（上/下工序與編輯製程）
 */
document.addEventListener('click', async (e) => {
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

  // 3. 點擊編輯製程 ⚙️
  const editRouteBtn = e.target.closest('button[data-action="edit-route"]');
  if (editRouteBtn) {
    const id = editRouteBtn.dataset.id;
    const routeStr = editRouteBtn.dataset.route;
    const currentStatus = editRouteBtn.dataset.status;
    
    let currentRoute = [];
    try {
      currentRoute = JSON.parse(routeStr);
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
      const label = t.category ? `[${t.category}] ${t.name}` : `[通用] ${t.name}`;
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
          <div class="flex items-center gap-3 bg-slate-50 p-3 rounded-lg border border-slate-200 cursor-grab step-item" draggable="true" data-step="${step}">
            <i data-lucide="grip-vertical" class="w-4 h-4 text-slate-300"></i>
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

      // 綁定右側拖曳
      let dragged = null;
      selectedList.querySelectorAll('.step-item').forEach(item => {
        item.addEventListener('dragstart', function() {
          dragged = this;
          setTimeout(() => this.classList.add('opacity-50'), 0);
        });
        item.addEventListener('dragend', function() {
          this.classList.remove('opacity-50');
          dragged = null;
          currentSteps = Array.from(selectedList.querySelectorAll('.step-item')).map(el => el.dataset.step);
          renderModalLists(); // 重新渲染以更新序號
        });
        item.addEventListener('dragover', function(ev) {
          ev.preventDefault();
          if (this === dragged) return;
          const rect = this.getBoundingClientRect();
          const next = (ev.clientY - rect.top) / (rect.bottom - rect.top) > 0.5;
          selectedList.insertBefore(dragged, next ? this.nextSibling : this);
        });
      });
    };

    renderModalLists();

    // 快速套用模板變更事件
    element.querySelector('#modal-template-select').addEventListener('change', (ev) => {
      const idx = parseInt(ev.target.value);
      if (!isNaN(idx) && routeTemplates[idx]) {
        currentSteps = [...routeTemplates[idx].steps];
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
        confirmBtn.disabled = false;
        confirmBtn.textContent = '儲存';
      }
    });
    return;
  }
});

/* ═══ EOF ═══
F: AI-WA-UPWO/src/ui/order-view-editor.js
V: 1792927300
W: UPGRADE-004
═══ */```

<!-- === FILE END: ./src/ui/order-view-editor.js === -->

<!-- === FILE START: ./src/ui/order-view.js === -->

## 📄 ./src/ui/order-view.js

```js
import { AppState } from '../config/state.js';
import { createIcons, ArrowUpDown, ArrowUp, ArrowDown, Trash2, Inbox, Search, ChevronLeft, ChevronRight, Settings } from 'lucide';

/**
 * 將 Excel 日期轉為 YYYY-MM-DD，並防呆過濾
 */
export function formatExcelDate(val) {
  if (!val) return "";
  if (typeof val === 'string') {
    if (val.includes('T')) return val.split('T')[0];
    if (val.includes('-') || val.includes('/')) return val.trim();
  }
  const num = Number(val);
  if (!isNaN(num) && num > 20000 && num < 80000) {
    const date = new Date((num - 25569) * 86400 * 1000);
    if (!isNaN(date.getTime())) return date.toISOString().split('T')[0];
  }
  return String(val).trim();
}

// 全域狀態暫存，避免切換頁面時遺失
const viewStates = {};

/**
 * 核心渲染器：完全數據驅動
 */
export function renderOrderView({ containerId, orders, visibleFields = [], isAdmin = false, pageKey, onAction }) {
  const container = document.getElementById(containerId);
  if (!container) return;

  if (!viewStates[pageKey]) {
    viewStates[pageKey] = { viewType: 'table', sortKey: '', sortOrder: 'asc', filters: {} };
  }
  const state = viewStates[pageKey];

  // 1. 資料清洗：過濾垃圾資料 (含 *** 或無單號)
  let cleanOrders = orders.filter(o => {
    const id = String(o.id || '');
    return id && !id.includes('***') && !id.includes('訂單');
  });

  // 2. 依配置提取顯示欄位 (若無配置，保底顯示 id 與 status)
  let displayFields = visibleFields.filter(f => f.isVisible);
  if (displayFields.length === 0) {
    displayFields = [
      { key: 'id', label: '發票', isVisible: true, isEditable: false },
      { key: 'status', label: '生產進度', isVisible: true, isEditable: false }
    ];
  }

  // 3. 全域事件委派 (確保全站按鈕絕不失效)
  if (!window._orderViewListenerAdded) {
    // 點擊事件
    document.addEventListener('click', (e) => {
      // 切換視圖
      const viewBtn = e.target.closest('[data-action="switch-view"]');
      if (viewBtn) {
        const key = viewBtn.dataset.pageKey;
        if (viewStates[key]) {
          viewStates[key].viewType = viewBtn.dataset.view;
          window.dispatchEvent(new Event(`refresh-${key}`));
        }
        return;
      }

      // 排序
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
      
      // 動作按鈕 (刪除)
      const actionBtn = e.target.closest('[data-action="delete"]');
      if (actionBtn) {
        if (onAction) onAction('delete', actionBtn.dataset.id);
      }
    });

    // 輸入事件 (全域搜尋)
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

    // 改變事件 (行內編輯)
    document.addEventListener('change', (e) => {
      // 行內編輯（日期或文字）
      const inlineInput = e.target.closest('input[data-action="inline-edit"]');
      if (inlineInput) {
        const id = inlineInput.dataset.id;
        const key = inlineInput.dataset.key;
        const value = inlineInput.value;
        
        // 拋出全域事件，由 order-view-editor.js 接手處理存檔
        window.dispatchEvent(new CustomEvent('inline-edit', { 
          detail: { id, key, value } 
        }));
      }
    });

    window._orderViewListenerAdded = true;
  }

  // 4. 處理排序與全域篩選
  if (state.sortKey) {
    cleanOrders.sort((a, b) => {
      let valA = getOrderValue(a, state.sortKey);
      let valB = getOrderValue(b, state.sortKey);
      return (valA < valB ? -1 : 1) * (state.sortOrder === 'asc' ? 1 : -1);
    });
  }

  const globalFilter = state.filters['__GLOBAL__'];
  if (globalFilter) {
    cleanOrders = cleanOrders.filter(o => {
      // 檢查所有顯示欄位是否包含關鍵字
      return displayFields.some(f => {
        const val = String(getOrderValue(o, f.key) || '').toLowerCase();
        return val.includes(globalFilter);
      });
    });
  }

  const allStatuses = AppState.getStatuses() || [];

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
        <input type="text" data-action="global-search" data-page-key="${pageKey}" value="${state.filters['__GLOBAL__'] || ''}" class="w-full pl-9 pr-4 py-1.5 bg-white border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand transition-colors shadow-sm" placeholder="搜尋所有欄位&#8230;">
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

  let contentHtml = '';
  if (state.viewType === 'card') {
    // 動態卡片
    contentHtml = `<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 pb-4">` + 
      cleanOrders.map(order => `
        <div class="glass-card p-4 flex flex-col gap-2">
          ${displayFields.map(f => {
            const val = getOrderValue(order, f.key);
            
            if (f.key === 'status') {
              let route = order.route && order.route.length > 0 ? order.route : allStatuses;
              let idx = route.indexOf(order.status);
              if (idx === -1) idx = 0;
              const isFirst = idx <= 0;
              const isLast = idx >= route.length - 1;
              const prevStep = isFirst ? '' : route[idx - 1];
              const nextStep = isLast ? '' : route[idx + 1];
              const routeStr = JSON.stringify(route).replace(/"/g, '&quot;');

              return `
                <div class="text-sm flex flex-col gap-1">
                  <span class="text-slate-400">${f.label}:</span>
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
                    <div class="w-px h-4 bg-slate-200 mx-1"></div>
                    <button data-action="edit-route" data-id="${order.id}" data-route="${routeStr}" data-status="${order.status}" 
                      class="p-1 rounded text-slate-400 hover:text-brand hover:bg-white transition-colors" title="編輯製程">
                      <i data-lucide="settings" class="w-4 h-4"></i>
                    </button>
                  </div>
                </div>
              `;
            }
            
            // 判斷是否可編輯
            if (f.isEditable) {
              const inputType = f.type === 'date' ? 'date' : (f.type === 'number' ? 'number' : 'text');
              const displayVal = f.type === 'date' ? formatExcelDate(val) : val;
              
              return `
                <div class="text-sm flex flex-col gap-1">
                  <span class="text-slate-400">${f.label}:</span>
                  <input type="${inputType}" data-action="inline-edit" data-id="${order.id}" data-key="${f.key}" value="${displayVal || ''}" 
                    class="w-full px-2 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:border-brand transition-colors">
                </div>
              `;
            }
            
            return `<div class="text-sm"><span class="text-slate-400">${f.label}:</span> <span class="font-medium text-slate-700">${val || '-'}</span></div>`;
          }).join('')}
          ${isAdmin ? `
            <div class="mt-auto pt-2 border-t border-slate-100 flex justify-end">
              <button data-action="delete" data-id="${order.id}" class="text-xs text-rose-500 hover:bg-rose-50 px-2 py-1 rounded">刪除</button>
            </div>
          ` : ''}
        </div>
      `).join('') + `</div>`;
  } else {
    // 動態表格
    contentHtml = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-x-auto w-full">
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
            ${cleanOrders.map(order => `
              <tr class="hover:bg-slate-50/50 transition-colors">
                ${displayFields.map(f => {
                  if (f.key === 'status') {
                    let route = order.route && order.route.length > 0 ? order.route : allStatuses;
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
                          <div class="w-px h-4 bg-slate-200 mx-1"></div>
                          <button data-action="edit-route" data-id="${order.id}" data-route="${routeStr}" data-status="${order.status}" 
                            class="p-1 rounded text-slate-400 hover:text-brand hover:bg-white transition-colors" title="編輯製程">
                            <i data-lucide="settings" class="w-4 h-4"></i>
                          </button>
                        </div>
                      </td>
                    `;
                  }
                  
                  // 判斷是否可編輯
                  if (f.isEditable) {
                    const inputType = f.type === 'date' ? 'date' : (f.type === 'number' ? 'number' : 'text');
                    const displayVal = f.type === 'date' ? formatExcelDate(getOrderValue(order, f.key)) : getOrderValue(order, f.key);
                    
                    return `
                      <td class="px-2 py-2 whitespace-nowrap">
                        <input type="${inputType}" data-action="inline-edit" data-id="${order.id}" data-key="${f.key}" value="${displayVal || ''}" 
                          class="w-full px-2 py-1.5 bg-transparent border border-transparent hover:border-slate-200 focus:border-brand focus:bg-white rounded-lg text-sm transition-colors outline-none">
                      </td>
                    `;
                  }
                  
                  return `<td class="px-4 py-3 whitespace-nowrap">${getOrderValue(order, f.key) || '-'}</td>`;
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
  }

  container.innerHTML = viewToggleHtml + contentHtml;
  createIcons({ icons: { ArrowUpDown, ArrowUp, ArrowDown, Search, ChevronLeft, ChevronRight, Settings }, attrs: { class: 'w-4 h-4' } });
}

/**
 * 取得欄位值 (處理巢狀 data 與特殊日期格式)
 */
function getOrderValue(order, key) {
  if (key === 'uploadDate' || key === 'dueDate' || key === 'statusUpdateDate') {
    return formatExcelDate(order[key]);
  }
  if (key === 'status') {
    return order.status || '未知'; 
  }
  if (order.data && order.data[key] !== undefined) {
    return order.data[key];
  }
  return order[key] !== undefined ? order[key] : '';
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/ui/order-view.js
V: 1792925500
W: UPGRADE-004
═══ */```

<!-- === FILE END: ./src/ui/order-view.js === -->

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
