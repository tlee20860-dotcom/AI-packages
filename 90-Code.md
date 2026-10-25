> 生成時間：2026-10-25 08:56:10 (HKT)

# 專案全程式碼包：AI-WEB-APP-Family


<!-- === FILE START: ./index.html === -->

## 📄 ./index.html

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>FAMILY.FIN V2</title>
  
  <!-- PWA 設定 -->
  <link rel="manifest" href="./manifest.json">
  <meta name="theme-color" content="#0F172A">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="FAMILY V2">
  
  <!-- 預載 CDN 資源 -->
  <link rel="preconnect" href="https://unpkg.com" crossorigin>
  <link rel="preconnect" href="https://www.gstatic.com" crossorigin>
  <link rel="dns-prefetch" href="https://identitytoolkit.googleapis.com">

  <!-- 樣式 (P0 僅載入基礎主題) -->
  <link rel="stylesheet" href="./css/theme.css">
  
  <!-- 確保手機端不會有奇怪的白邊或滾動 -->
  <style>
    body { margin: 0; padding: 0; background-color: var(--bg-deep, #0F172A); color: var(--text-primary, #F1F5F9); font-family: sans-serif; }
    #app-loading { display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; }
    .spinner { width: 40px; height: 40px; border: 4px solid rgba(255,255,255,0.1); border-top-color: #00F0FF; border-radius: 50%; animation: spin 1s linear infinite; margin-bottom: 16px; }
    @keyframes spin { to { transform: rotate(360deg); } }
  </style>
</head>
<body>
  <!-- 載入畫面 -->
  <div id="app-loading">
    <div class="spinner"></div>
    <div style="font-family: monospace; color: #00F0FF; letter-spacing: 1px;">SYSTEM INITIALIZING...</div>
  </div>

  <!-- SPA 掛載點 -->
  <div id="app-root" style="display: none;"></div>

  <!-- 圖示庫 -->
  <script src="https://unpkg.com/lucide@0.462.0/dist/umd/lucide.min.js" defer></script>

  <!-- 主程式入口 (待後續階段建立 engine/main.js) -->
  <!-- <script type="module" src="./engine/main.js"></script> -->
</body>
</html>

<!-- ═══ EOF ═══
F: index.html
V: 1792900130
W: ai-system-20261024-012#8
═══ EOF ═══ -->```

<!-- === FILE END: ./index.html === -->

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
V: 1792900120
W: ai-system-20261024-012#4
═══ EOF ═══ */```

<!-- === FILE END: ./sw.js === -->
