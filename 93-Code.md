> 體系版本：SV14
> 生成時間：2026-11-07 00:26:31 (HKT)

# 專案全程式碼包：AI-WA-FS


<!-- === FILE START: ./functions/api/supabase/[[path]].js === -->

## 📄 ./functions/api/supabase/[[path]].js

```js
export async function onRequest(context) {
  const { request } = context;
  const url = new URL(request.url);

  // 1. 處理跨域預檢請求 (CORS)
  if (request.method === 'OPTIONS') {
    return new Response(null, {
      status: 204,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Methods': 'GET, POST, PUT, PATCH, DELETE, OPTIONS',
        'Access-Control-Allow-Headers': '*',
        'Access-Control-Max-Age': '86400',
      }
    });
  }

  // 2. 替換為你全新的 Supabase 網址
  const targetPath = url.pathname.replace('/api/supabase', '');
  const targetUrl = `https://ynfnheiefruwykbthqzd.supabase.co${targetPath}${url.search}`;

  // 3. 嚴格過濾 Header，避免 530 崩潰
  const newHeaders = new Headers();
  const allowed = ['apikey', 'authorization', 'content-type', 'accept', 'prefer', 'x-client-info'];
  allowed.forEach(h => {
    const val = request.headers.get(h);
    if (val) newHeaders.set(h, val);
  });

  // 4. 安全讀取 Body
  let body = null;
  if (!['GET', 'HEAD'].includes(request.method)) {
    body = await request.arrayBuffer();
  }

  try {
    // 5. 發送給 Supabase
    const response = await fetch(targetUrl, {
      method: request.method,
      headers: newHeaders,
      body: body
    });

    // 6. 重建 Response Header
    const resHeaders = new Headers(response.headers);
    resHeaders.delete('content-encoding');
    resHeaders.delete('content-length');
    resHeaders.set('Access-Control-Allow-Origin', '*');
    resHeaders.set('Access-Control-Allow-Headers', '*');

    return new Response(response.body, {
      status: response.status,
      statusText: response.statusText,
      headers: resHeaders
    });
    
  } catch (error) {
    return new Response(JSON.stringify({ error: error.message }), {
      status: 500,
      headers: { 'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*' }
    });
  }
}

/* ═══ EOF ═══
F: AI-WA-FS/functions/api/supabase/[[path]].js
V: 1793806000
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./functions/api/supabase/[[path]].js === -->

<!-- === FILE START: ./index.html === -->

## 📄 ./index.html

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>家庭入帳 V3</title>
  
  <!-- PWA 設定 -->
  <link rel="manifest" href="/manifest.webmanifest">
  <meta name="theme-color" content="#1e293b">
  
  <!-- iOS 專用 Meta (加到主畫面必備) -->
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="入帳 V3">
  <link rel="apple-touch-icon" href="/pwa-192x192.png">
  
  <!-- 字型載入 -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700&display=swap" rel="stylesheet">
</head>
<body>
  <div id="app"></div>
  <script type="module" src="/src/main.js"></script>
</body>
</html>
<!-- ═══ EOF ═══
F: AI-WA-FS/index.html
V: 1793810300
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./index.html === -->

<!-- === FILE START: ./meta.json === -->

## 📄 ./meta.json

```json
{
  "domain": "W",
  "pkg_no": "93",
  "code": "FS",
  "display": "家庭入帳V3",
  "_EOF": {
    "F": "AI-WA-FS/meta.json",
    "V": 1793804067,
    "W": "ai-wa-fs-001"
  }
}```

<!-- === FILE END: ./meta.json === -->

<!-- === FILE START: ./package.json === -->

## 📄 ./package.json

```json
{
  "name": "ai-wa-fs",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "@supabase/supabase-js": "^2.39.0",
    "element-plus": "^2.4.4",
    "exceljs": "^4.4.0",
    "file-saver": "^2.0.5",
    "lucide-vue-next": "^0.294.0",
    "pinia": "^2.1.7",
    "vue": "^3.3.10",
    "vue-router": "^4.2.5"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.5.0",
    "vite": "^5.0.5",
    "vite-plugin-pwa": "^0.17.4"
  },
  "_EOF": {
    "F": "AI-WA-FS/package.json",
    "V": 1793969000,
    "W": "ai-wa-fs-002"
  }
}```

<!-- === FILE END: ./package.json === -->

<!-- === FILE START: ./src/App.vue === -->

## 📄 ./src/App.vue

```vue
<template>
  <router-view v-if="$route.path === '/login'" />
  
  <el-container v-else class="app-layout">
    <!-- 🚨 手機端遮罩 -->
    <div v-if="isMobile && isMobileOpen" class="sidebar-mask" @click="isMobileOpen = false"></div>

    <!-- 側邊選單 -->
    <el-aside 
      :class="{ 
        'sidebar': true, 
        'is-collapsed': isCollapse && !isMobile,
        'is-mobile-hidden': isMobile && !isMobileOpen 
      }" 
      class="transition-all"
    >
      <div class="logo">
        <div class="logo-icon">
          <el-icon><Money /></el-icon>
        </div>
        <h2 v-if="!isCollapse || isMobile">家庭入帳 V3</h2>
      </div>
      
      <el-menu
        :default-active="$route.path"
        router
        background-color="transparent"
        text-color="#64748B"
        active-text-color="#6366F1"
        :collapse="isCollapse && !isMobile"
        @select="onMenuSelect"
        class="custom-menu"
      >
        <el-menu-item index="/">
          <el-icon><HomeFilled /></el-icon>
          <template #title>總覽儀表板</template>
        </el-menu-item>
        <el-menu-item index="/cashflow">
          <el-icon><Money /></el-icon>
          <template #title>收支流水</template>
        </el-menu-item>
        <el-menu-item index="/settlement">
          <el-icon><Tickets /></el-icon>
          <template #title>待辦中心</template>
        </el-menu-item>
        <el-menu-item index="/insurance">
          <el-icon><Umbrella /></el-icon>
          <template #title>保險供款</template>
        </el-menu-item>
        <el-menu-item index="/fund">
          <el-icon><TrendCharts /></el-icon>
          <template #title>基金投資</template>
        </el-menu-item>
        <el-menu-item index="/annual-report">
          <el-icon><Document /></el-icon>
          <template #title>年度報表</template>
        </el-menu-item>
        <el-menu-item index="/settings">
          <el-icon><Setting /></el-icon>
          <template #title>系統設定</template>
        </el-menu-item>
      </el-menu>
    </el-aside>

    <!-- 主內容區 -->
    <el-container class="main-wrapper">
      <el-header class="header">
        <el-button class="menu-toggle" text @click="toggleMenu">
          <el-icon :size="20"><Menu /></el-icon>
        </el-button>
        <div class="header-title">歡迎回來</div>
        <el-button type="danger" plain size="small" class="logout-btn" @click="handleLogout">登出</el-button>
      </el-header>
      
      <el-main class="main-content">
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { supabase } from '@/config/supabase'
import { HomeFilled, Money, Umbrella, Menu, TrendCharts, Document, Setting, Tickets } from '@element-plus/icons-vue'

const isCollapse = ref(false)
const isMobile = ref(false)
const isMobileOpen = ref(false)

const checkScreenSize = () => {
  isMobile.value = window.innerWidth < 768
  if (!isMobile.value) {
    isMobileOpen.value = false
  }
}

const toggleMenu = () => {
  if (isMobile.value) {
    isMobileOpen.value = !isMobileOpen.value
  } else {
    isCollapse.value = !isCollapse.value
  }
}

const onMenuSelect = () => {
  if (isMobile.value) {
    isMobileOpen.value = false
  }
}

const handleLogout = async () => {
  await supabase.auth.signOut()
  window.location.href = '/login'
}

onMounted(() => {
  checkScreenSize()
  window.addEventListener('resize', checkScreenSize)
})

onUnmounted(() => {
  window.removeEventListener('resize', checkScreenSize)
})
</script>

<style>
body { margin: 0; padding: 0; font-family: 'Noto Sans TC', sans-serif; background-color: #F4F7FB; }

/* 佈局基礎 */
.app-layout { display: flex; height: 100vh; overflow: hidden; width: 100vw; }
.main-wrapper { flex: 1; width: 0; overflow: hidden; display: flex; flex-direction: column; }

/* 側邊欄基礎樣式 - 白底 */
.sidebar { 
  background-color: #ffffff; 
  border-right: 1px solid rgba(0,0,0,0.04);
  z-index: 1000;
  width: 240px;
  display: flex;
  flex-direction: column;
}
.sidebar.is-collapsed {
  width: 64px;
}

/* Logo 區域 */
.logo { 
  height: 64px; 
  display: flex; 
  align-items: center; 
  padding: 0 20px; 
  gap: 12px;
  overflow: hidden; 
}
.logo-icon {
  width: 32px; height: 32px;
  background: linear-gradient(135deg, #6366F1, #06B6D4);
  border-radius: 8px;
  display: flex; justify-content: center; align-items: center;
  color: white; flex-shrink: 0;
}
.logo h2 { margin: 0; font-size: 18px; color: #1e293b; white-space: nowrap; font-weight: 700; }

/* 選單樣式覆寫 */
.custom-menu { border-right: none; padding: 10px; }
.el-menu-item {
  border-radius: 10px;
  margin-bottom: 4px;
  height: 44px;
  line-height: 44px;
  font-weight: 500;
}
.el-menu-item:hover {
  background-color: #F8FAFC !important;
}
.el-menu-item.is-active {
  background-color: #EEF2FF !important;
  font-weight: 700;
}
.el-menu-item.is-active .el-icon {
  color: #6366F1;
}

/* 頂部導覽列 - 毛玻璃效果 */
.header { 
  background-color: rgba(255, 255, 255, 0.8); 
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  display: flex; 
  align-items: center; 
  border-bottom: 1px solid rgba(0,0,0,0.04); 
  padding: 0 20px; 
  height: 64px; 
  flex-shrink: 0; 
  z-index: 999;
}
.header-title { font-weight: 700; color: #1e293b; margin-left: 10px; flex: 1; font-size: 16px; }
.menu-toggle { margin-right: 10px; color: #64748b; padding: 8px; border-radius: 8px; }
.menu-toggle:hover { background-color: #F1F5F9; }

.logout-btn { border-radius: 8px; border: none; background-color: #FEF2F2; color: #EF4444; }
.logout-btn:hover { background-color: #FEE2E2; }

/* 內容區 */
.main-content { 
  background-color: #F4F7FB; 
  padding: 24px; 
  overflow-y: auto; 
  overflow-x: hidden; 
  flex: 1; 
}

/* 過渡動畫 */
.transition-all { transition: width 0.3s, transform 0.3s; }

/* 🚨 手機端專用樣式 */
@media (max-width: 768px) {
  .sidebar {
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    box-shadow: 4px 0 24px rgba(0,0,0,0.1);
  }
  .sidebar.is-mobile-hidden {
    transform: translateX(-100%);
    width: 240px;
  }
  .sidebar-mask {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background: rgba(15, 23, 42, 0.4);
    backdrop-filter: blur(4px);
    z-index: 999;
  }
  .main-content {
    padding: 16px;
  }
}
</style>

<!-- ═══ EOF ═══
F: AI-WA-FS/src/App.vue
V: 1793971100
W: ai-wa-fs-002
═══ -->```

<!-- === FILE END: ./src/App.vue === -->

<!-- === FILE START: ./src/config/supabase.js === -->

## 📄 ./src/config/supabase.js

```js
import { createClient } from '@supabase/supabase-js'

// 🚀 使用代理路徑，避開電信商封鎖
const supabaseUrl = window.location.origin + '/api/supabase'

// 👇 這是你剛才提供的新 Key
const supabaseKey = 'sb_publishable_n31bORorOl534GTFooKJ4w_mcQQcRxt'

export const supabase = createClient(supabaseUrl, supabaseKey)

/* ═══ EOF ═══
F: AI-WA-FS/src/config/supabase.js
V: 1793806000
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./src/config/supabase.js === -->

<!-- === FILE START: ./src/main.js === -->

## 📄 ./src/main.js

```js
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import zhTw from 'element-plus/es/locale/lang/zh-tw'
import App from './App.vue'
import router from './router'
import './style.css'

// 🆕 引入 PWA 註冊函式
import { registerSW } from 'virtual:pwa-register'

const app = createApp(App)

app.use(ElementPlus, { locale: zhTw })
app.use(router)

app.mount('#app')

// 🆕 註冊 Service Worker 並設定自動更新
const updateSW = registerSW({
  onNeedRefresh() {
    if (confirm('發現新版本，是否立即更新？')) {
      updateSW(true)
    }
  },
  onOfflineReady() {
    console.log('✅ App 已可離線使用')
  },
})

/* ═══ EOF ═══
F: AI-WA-FS/src/main.js
V: 1793810900
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./src/main.js === -->

<!-- === FILE START: ./src/router/index.js === -->

## 📄 ./src/router/index.js

```js
import { createRouter, createWebHistory } from 'vue-router'
import { supabase } from '@/config/supabase'

const routes = [
  {
    path: '/login',
    name: 'Login',
    component: () => import('@/views/LoginView.vue'),
    meta: { requiresAuth: false }
  },
  {
    path: '/',
    name: 'Dashboard',
    component: () => import('@/views/DashboardView.vue'),
    meta: { requiresAuth: true }
  },
  {
    path: '/cashflow',
    name: 'Cashflow',
    component: () => import('@/views/CashflowView.vue'),
    meta: { requiresAuth: true }
  },
  {
    path: '/settlement',
    name: 'Settlement',
    component: () => import('@/views/SettlementView.vue'),
    meta: { requiresAuth: true }
  },
  {
    path: '/insurance',
    name: 'Insurance',
    component: () => import('@/views/InsuranceView.vue'),
    meta: { requiresAuth: true }
  },
  {
    path: '/fund',
    name: 'Fund',
    component: () => import('@/views/FundView.vue'),
    meta: { requiresAuth: true }
  },
  {
    path: '/annual-report',
    name: 'AnnualReport',
    component: () => import('@/views/AnnualReportView.vue'),
    meta: { requiresAuth: true }
  },
  {
    path: '/settings',
    name: 'Settings',
    component: () => import('@/views/SettingsView.vue'),
    meta: { requiresAuth: true }
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

router.beforeEach(async (to, from, next) => {
  const { data: { session } } = await supabase.auth.getSession()
  const requiresAuth = to.matched.some(record => record.meta.requiresAuth)
  
  if (requiresAuth && !session) next('/login')
  else if (to.path === '/login' && session) next('/')
  else next()
})

export default router

/* ═══ EOF ═══
F: AI-WA-FS/src/router/index.js
V: 1793810700
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./src/router/index.js === -->

<!-- === FILE START: ./src/style.css === -->

## 📄 ./src/style.css

```css
:root {
  /* 覆寫 Element Plus 主色調為 92 專案的靛藍色 */
  --el-color-primary: #6366F1;
  --el-color-primary-light-3: #818CF8;
  --el-color-primary-light-5: #A5B4FC;
  --el-color-primary-light-7: #C7D2FE;
  --el-color-primary-light-8: #E0E7FF;
  --el-color-primary-light-9: #EEF2FF;
  --el-color-primary-dark-2: #4F46E5;

  --bg-color: #F4F7FB;
  --text-main: #1e293b;
  --text-secondary: #64748b;
  
  /* 柔和陰影 */
  --card-shadow: 0 4px 20px -2px rgba(0, 0, 0, 0.05);
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
  font-family: 'Noto Sans TC', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  background-color: var(--bg-color);
  color: var(--text-main);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* 覆寫卡片樣式：大圓角、無邊框、柔和陰影、懸停上浮 */
.el-card {
  border-radius: 20px !important;
  border: 1px solid rgba(0, 0, 0, 0.03) !important;
  box-shadow: var(--card-shadow) !important;
  background-color: #ffffff;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.el-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.08) !important;
}
.el-card__header {
  border-bottom: 1px solid #f1f5f9 !important;
  padding: 18px 24px !important;
}
.el-card__body {
  padding: 24px !important;
}

/* 優化表格樣式 */
.el-table {
  --el-table-border-color: #f1f5f9;
  --el-table-header-bg-color: #F8FAFC;
  border-radius: 12px;
  overflow: hidden;
}
.el-table th.el-table__cell {
  font-weight: 600;
  color: #475569;
  padding: 14px 0;
}
.el-table td.el-table__cell {
  padding: 12px 0;
}

/* 優化對話框與彈窗 */
.el-dialog {
  border-radius: 20px !important;
  overflow: hidden;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25) !important;
}
.el-dialog__header {
  margin-right: 0;
  padding: 20px 24px;
  border-bottom: 1px solid #f1f5f9;
}
.el-dialog__title {
  font-weight: 700 !important;
  color: var(--text-main) !important;
}
.el-dialog__body {
  padding: 24px !important;
}

/* 重寫標籤樣式：無邊框、淡背景 */
.el-tag {
  border: none !important;
  border-radius: 6px !important;
  padding: 0 10px !important;
  font-weight: 500;
  background-color: #F1F5F9;
  color: #475569;
}
.el-tag--primary {
  background-color: var(--el-color-primary-light-9);
  color: var(--el-color-primary);
}
.el-tag--success {
  background-color: #ECFDF5;
  color: #10B981;
}
.el-tag--warning {
  background-color: #FFFBEB;
  color: #F59E0B;
}
.el-tag--danger {
  background-color: #FEF2F2;
  color: #EF4444;
}
.el-tag--info {
  background-color: #F8FAFC;
  color: #64748B;
}

/* 按鈕圓角與過渡 */
.el-button {
  border-radius: 10px !important;
  font-weight: 500;
  transition: all 0.2s ease;
}
.el-button:active {
  transform: scale(0.98);
}

/* 輸入框與下拉選單圓角 */
.el-input__wrapper, .el-select__wrapper, .el-textarea__inner {
  border-radius: 10px !important;
  box-shadow: 0 0 0 1px #E2E8F0 inset !important;
  background-color: #F8FAFC !important;
}
.el-input__wrapper.is-focus, .el-select__wrapper.is-focused {
  box-shadow: 0 0 0 2px var(--el-color-primary-light-7) inset !important;
  background-color: #FFFFFF !important;
}

/* 優化進度條 */
.el-progress-bar__outer {
  background-color: #F1F5F9 !important;
  border-radius: 10px;
}
.el-progress-bar__inner {
  border-radius: 10px;
}

/* 全局滾動條美化 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
::-webkit-scrollbar-thumb {
  background: #CBD5E1;
  border-radius: 4px;
}
::-webkit-scrollbar-track {
  background: transparent;
}

/* 確保手機端版面不會破版 */
#app {
  width: 100%;
  max-width: 100vw;
  overflow-x: hidden;
}

/* ═══ EOF ═══
F: AI-WA-FS/src/style.css
V: 1793971000
W: ai-wa-fs-002
═══ */```

<!-- === FILE END: ./src/style.css === -->

<!-- === FILE START: ./src/views/AnnualReportView.vue === -->

## 📄 ./src/views/AnnualReportView.vue

```vue
<template>
  <div class="annual-container">
    <div class="page-header">
      <div class="header-left">
        <h2>年度報表</h2>
        <el-date-picker 
          v-model="selectedYear" 
          type="year" 
          placeholder="選擇年份" 
          value-format="YYYY" 
          @change="fetchData" 
          class="year-picker"
        />
      </div>
      <div class="header-actions">
        <el-radio-group v-model="viewMode" class="view-toggle" @change="fetchData">
          <el-radio-button label="year">全年</el-radio-button>
          <el-radio-button label="month">單月</el-radio-button>
        </el-radio-group>
        <el-select v-if="viewMode === 'month'" v-model="selectedMonth" class="month-picker" @change="fetchData">
          <el-option v-for="m in 12" :key="m" :label="`${m} 月`" :value="String(m).padStart(2, '0')" />
        </el-select>
        <el-button type="success" plain icon="Download" @click="exportExcel" :loading="exporting" :disabled="monthlyData.length === 0">
          匯出明細
        </el-button>
      </div>
    </div>

    <!-- 統計卡片 -->
    <el-row :gutter="20" class="mb-24" v-loading="loading">
      <el-col :xs="12" :sm="6">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">{{ viewTitle }}總收入</div>
          <div class="stat-value text-green">HK$ {{ totalIncome.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="12" :sm="6">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">{{ viewTitle }}總支出</div>
          <div class="stat-value text-red">HK$ {{ totalExpense.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="12" :sm="6">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">{{ viewTitle }}保險支出</div>
          <div class="stat-value text-orange">HK$ {{ totalInsurance.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="12" :sm="6">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">{{ viewTitle }}淨結餘</div>
          <div class="stat-value" :class="netBalance >= 0 ? 'text-green' : 'text-red'">
            {{ netBalance >= 0 ? '+' : '' }} HK$ {{ netBalance.toLocaleString() }}
          </div>
        </el-card>
      </el-col>
    </el-row>

    <!-- 月度/單月明細表格 -->
    <el-card shadow="never" class="table-card" v-loading="loading">
      <div class="table-wrapper">
        <el-table :data="monthlyData" style="width: 100%; min-width: 600px;" stripe show-summary :summary-method="getSummaries">
          <el-table-column prop="month" label="月份" width="100">
            <template #default="scope">
              <strong>{{ scope.row.month }} 月</strong>
            </template>
          </el-table-column>
          <el-table-column prop="income" label="總收入" align="right">
            <template #default="scope">
              <span class="text-green">HK$ {{ scope.row.income.toLocaleString() }}</span>
            </template>
          </el-table-column>
          <el-table-column prop="expense" label="一般支出" align="right">
            <template #default="scope">
              <span class="text-red">HK$ {{ scope.row.expense.toLocaleString() }}</span>
            </template>
          </el-table-column>
          <el-table-column prop="insurance" label="保險供款" align="right">
            <template #default="scope">
              <span class="text-orange">HK$ {{ scope.row.insurance.toLocaleString() }}</span>
            </template>
          </el-table-column>
          <el-table-column prop="balance" label="當月結餘" align="right">
            <template #default="scope">
              <span :class="scope.row.balance >= 0 ? 'text-green' : 'text-red'" class="font-bold">
                {{ scope.row.balance >= 0 ? '+' : '' }} HK$ {{ scope.row.balance.toLocaleString() }}
              </span>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-card>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage } from 'element-plus'
import { Download } from '@element-plus/icons-vue'
import ExcelJS from 'exceljs'
import { saveAs } from 'file-saver'

const currentYear = new Date().getFullYear().toString()
const selectedYear = ref(currentYear)
const viewMode = ref('year') // 'year' | 'month'
const selectedMonth = ref(String(new Date().getMonth() + 1).padStart(2, '0')) // 01~12

const loading = ref(false)
const exporting = ref(false)

const monthlyData = ref([])
const rawIncomes = ref([])
const rawExpenses = ref([])
const rawInsurancePayments = ref([])
const policies = ref([])

const viewTitle = computed(() => {
  return viewMode.value === 'year' ? `年度` : `${parseInt(selectedMonth.value)} 月`
})

// 統計加總
const totalIncome = computed(() => monthlyData.value.reduce((sum, item) => sum + item.income, 0))
const totalExpense = computed(() => monthlyData.value.reduce((sum, item) => sum + item.expense, 0))
const totalInsurance = computed(() => monthlyData.value.reduce((sum, item) => sum + item.insurance, 0))
const netBalance = computed(() => totalIncome.value - totalExpense.value - totalInsurance.value)

const fetchData = async () => {
  loading.value = true
  
  try {
    // 構建查詢前綴
    const prefix = viewMode.value === 'year' 
      ? `${selectedYear.value}-%` 
      : `${selectedYear.value}-${selectedMonth.value}%`

    // 平行抓取：收入、支出(已處理)、保險(已轉)、保單主檔
    const [incRes, expRes, insRes, polRes] = await Promise.all([
      supabase.from('incomes').select('*').like('year_month', prefix),
      supabase.from('expenses').select('*').like('year_month', prefix).eq('status', '已處理'),
      supabase.from('insurance_payments').select('*').like('year_month', prefix).eq('status', '已轉'),
      supabase.from('insurance_policies').select('*')
    ])

    if (incRes.error) throw incRes.error
    if (expRes.error) throw expRes.error
    if (insRes.error) throw insRes.error

    rawIncomes.value = incRes.data || []
    rawExpenses.value = expRes.data || []
    rawInsurancePayments.value = insRes.data || []
    policies.value = polRes.data || []

    const policyMap = {}
    policies.value.forEach(p => { policyMap[p.id] = Number(p.monthly_average) })

    // 初始化資料結構
    let tempData = []
    if (viewMode.value === 'year') {
      tempData = Array.from({ length: 12 }, (_, i) => ({
        month: i + 1, income: 0, expense: 0, insurance: 0, balance: 0
      }))
    } else {
      const m = parseInt(selectedMonth.value)
      tempData = [{ month: m, income: 0, expense: 0, insurance: 0, balance: 0 }]
    }

    // 填入資料
    const getMonthIndex = (ym) => {
      const m = parseInt(ym.split('-')[1])
      return viewMode.value === 'year' ? m - 1 : 0
    }

    rawIncomes.value.forEach(item => {
      const idx = getMonthIndex(item.year_month)
      if (tempData[idx]) tempData[idx].income += Number(item.amount)
    })

    rawExpenses.value.forEach(item => {
      const idx = getMonthIndex(item.year_month)
      if (tempData[idx]) tempData[idx].expense += Number(item.amount)
    })

    rawInsurancePayments.value.forEach(item => {
      const idx = getMonthIndex(item.year_month)
      const amount = policyMap[item.policy_id] || 0
      if (tempData[idx]) tempData[idx].insurance += amount
    })

    // 計算結餘
    tempData.forEach(item => {
      item.balance = item.income - item.expense - item.insurance
    })

    monthlyData.value = tempData

  } catch (error) {
    ElMessage.error('載入報表失敗：' + error.message)
  } finally {
    loading.value = false
  }
}

// 表格底部加總列
const getSummaries = (param) => {
  const { columns } = param
  const sums = []
  columns.forEach((column, index) => {
    if (index === 0) {
      sums[index] = viewTitle.value + '總計'
      return
    }
    if (column.property === 'income') sums[index] = `HK$ ${totalIncome.value.toLocaleString()}`
    else if (column.property === 'expense') sums[index] = `HK$ ${totalExpense.value.toLocaleString()}`
    else if (column.property === 'insurance') sums[index] = `HK$ ${totalInsurance.value.toLocaleString()}`
    else if (column.property === 'balance') sums[index] = `HK$ ${netBalance.value.toLocaleString()}`
    else sums[index] = ''
  })
  return sums
}

// 匯出 Excel
const exportExcel = async () => {
  if (monthlyData.value.length === 0) return ElMessage.warning('無資料可匯出')
  exporting.value = true

  try {
    // 1. 整理匯出明細
    const policyMap = {}
    policies.value.forEach(p => { policyMap[p.id] = p })

    const allItems = []

    // 收集收入 (無對象分組，通常只有成員)
    rawIncomes.value.forEach(i => {
      allItems.push({
        year_month: i.year_month,
        type: '收入',
        payer: i.member_name || '未指定',
        name: i.name,
        category: '收入',
        amount: Number(i.amount),
        status: '已入帳'
      })
    })

    // 收集支出
    rawExpenses.value.forEach(e => {
      allItems.push({
        year_month: e.year_month,
        type: '支出',
        payer: e.payer_name || '未指定',
        name: e.name,
        category: e.category_name || '未分類',
        amount: Number(e.amount),
        status: e.status
      })
    })

    // 收集保險
    rawInsurancePayments.value.forEach(ip => {
      const p = policyMap[ip.policy_id]
      if (p) {
        allItems.push({
          year_month: ip.year_month,
          type: '保險',
          payer: '保險公司',
          name: `保險供款：${p.name}`,
          category: '保險',
          amount: Number(p.monthly_average),
          status: '已轉'
        })
      }
    })

    // 2. 建立 Excel 工作簿
    const workbook = new ExcelJS.Workbook()
    const worksheet = workbook.addWorksheet('收支明細')

    worksheet.columns = [
      { header: '月份', key: 'year_month', width: 12 },
      { header: '類型', key: 'type', width: 10 },
      { header: '對象', key: 'payer', width: 15 },
      { header: '項目名稱', key: 'name', width: 30 },
      { header: '類別', key: 'category', width: 15 },
      { header: '金額', key: 'amount', width: 15, style: { numFmt: '#,##0.00' } },
      { header: '狀態', key: 'status', width: 12 }
    ]

    // 表頭樣式
    const headerRow = worksheet.getRow(1)
    headerRow.font = { bold: true }
    headerRow.alignment = { vertical: 'middle', horizontal: 'center' }
    headerRow.fill = { type: 'pattern', pattern: 'solid', fgColor: { argb: 'FFE0E0E0' } }

    // 3. 按對象分組並寫入
    const grouped = {}
    allItems.forEach(item => {
      if (!grouped[item.payer]) grouped[item.payer] = []
      grouped[item.payer].push(item)
    })

    let currentRow = 2
    let grandTotal = 0

    // 遍歷每個對象
    for (const [payer, items] of Object.entries(grouped)) {
      let subtotal = 0

      items.forEach(item => {
        const row = worksheet.addRow(item)
        row.getCell('amount').alignment = { horizontal: 'right' }
        subtotal += item.amount
        currentRow++
      })

      grandTotal += subtotal

      // 寫入小計
      const subtotalRow = worksheet.addRow({
        year_month: '',
        type: '',
        payer: '',
        name: `${payer} 小計`,
        category: '',
        amount: subtotal,
        status: ''
      })
      
      worksheet.mergeCells(`A${currentRow}:D${currentRow}`)
      subtotalRow.getCell(4).alignment = { horizontal: 'right' }
      subtotalRow.getCell(6).alignment = { horizontal: 'right' }
      subtotalRow.font = { bold: true }
      subtotalRow.fill = { type: 'pattern', pattern: 'solid', fgColor: { argb: 'FFF2F2F2' } }
      
      currentRow++
    }

    // 4. 寫入總計
    const totalRow = worksheet.addRow({
      year_month: '',
      type: '',
      payer: '',
      name: `${viewTitle.value}總計`,
      category: '',
      amount: grandTotal,
      status: ''
    })
    worksheet.mergeCells(`A${currentRow}:D${currentRow}`)
    totalRow.getCell(4).alignment = { horizontal: 'right' }
    totalRow.getCell(6).alignment = { horizontal: 'right' }
    totalRow.font = { bold: true, color: { argb: 'FF6366F1' } }
    totalRow.fill = { type: 'pattern', pattern: 'solid', fgColor: { argb: 'FFEEF2FF' } }

    // 設置全局邊框
    worksheet.eachRow((row) => {
      row.eachCell((cell) => {
        cell.border = {
          top: { style: 'thin', color: { argb: 'FFDDDDDD' } },
          left: { style: 'thin', color: { argb: 'FFDDDDDD' } },
          bottom: { style: 'thin', color: { argb: 'FFDDDDDD' } },
          right: { style: 'thin', color: { argb: 'FFDDDDDD' } }
        }
      })
    })

    // 5. 導出檔案
    const buffer = await workbook.xlsx.writeBuffer()
    const blob = new Blob([buffer], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' })
    saveAs(blob, `收支明細_${viewTitle.value}.xlsx`)

    ElMessage.success('✅ 報表匯出成功')
  } catch (err) {
    console.error(err)
    ElMessage.error('匯出失敗：' + err.message)
  } finally {
    exporting.value = false
  }
}

onMounted(() => fetchData())
</script>

<style scoped>
.annual-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; flex-wrap: wrap; gap: 12px; }
.header-left { display: flex; align-items: center; gap: 12px; flex-wrap: wrap; }
.header-left h2 { margin: 0; color: #1e293b; font-size: 24px; font-weight: 700; }
.year-picker { width: 120px; }
.header-actions { display: flex; gap: 12px; align-items: center; flex-wrap: wrap; }
.month-picker { width: 100px; }

.mb-24 { margin-bottom: 24px; }
.stat-card { border-radius: 20px; background: #ffffff; border: none; }
.stat-label { font-size: 14px; color: #64748b; margin-bottom: 8px; }
.stat-value { font-size: 22px; font-weight: bold; }
.text-green { color: #10b981; }
.text-red { color: #ef4444; }
.text-orange { color: #f59e0b; }
.font-bold { font-weight: 700; }

.table-card { border-radius: 20px; border: none; overflow: hidden; }
.table-wrapper { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }
:deep(.el-table__footer-wrapper tbody td) {
  font-weight: bold;
  background-color: #F8FAFC;
  color: #1e293b;
}

@media (max-width: 768px) {
  .page-header { flex-direction: column; align-items: flex-start; }
  .header-actions { width: 100%; justify-content: space-between; }
  .stat-value { font-size: 18px; }
}
</style>

<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/AnnualReportView.vue
V: 1793971600
W: ai-wa-fs-002
═══ -->```

<!-- === FILE END: ./src/views/AnnualReportView.vue === -->

<!-- === FILE START: ./src/views/CashflowView.vue === -->

## 📄 ./src/views/CashflowView.vue

```vue
<template>
  <div class="cashflow-container">
    <div class="page-header">
      <h2>收支流水</h2>
      <div class="header-actions">
        <el-button type="primary" @click="openAddDialog">+ 新增紀錄</el-button>
      </div>
    </div>

    <!-- 統計卡片 (顯示所有歷史總和) -->
    <el-row :gutter="20" class="mb-20">
      <el-col :xs="12" :sm="12">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">總支出</div>
          <div class="stat-value text-red">HK$ {{ totalExpense.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="12" :sm="12">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">總收入</div>
          <div class="stat-value text-green">HK$ {{ totalIncome.toLocaleString() }}</div>
        </el-card>
      </el-col>
    </el-row>

    <!-- 資料顯示區 -->
    <el-card shadow="never" class="list-card">
      <el-tabs v-model="activeTab">
        <!-- 支出紀錄 -->
        <el-tab-pane label="支出紀錄" name="expense">
          <div class="tool-bar">
            <el-button type="warning" plain icon="MagicStick" :loading="importing" @click="importFixedExpenses">
              一鍵匯入固定支出
            </el-button>
          </div>

          <div v-if="expenses.length === 0" class="empty-state">
            <el-empty description="尚無任何支出紀錄" :image-size="80" />
          </div>

          <div v-else class="month-groups">
            <el-collapse v-model="activeExpenseMonths">
              <el-collapse-item 
                v-for="group in groupedExpenses" 
                :key="group.month" 
                :name="group.month"
              >
                <template #title>
                  <div class="group-title">
                    <span class="month-text">{{ group.month }}</span>
                    <span class="month-total text-red">總支出: HK$ {{ group.total.toLocaleString() }}</span>
                  </div>
                </template>
                
                <div class="table-wrapper">
                  <el-table :data="group.items" style="width: 100%; min-width: 600px;" stripe>
                    <el-table-column prop="name" label="項目名稱" min-width="150" />
                    <el-table-column prop="payer_name" label="對象" width="100" />
                    <el-table-column prop="category_name" label="類別" width="120">
                      <template #default="scope">
                        <el-tag size="small" type="info">{{ scope.row.category_name || '未分類' }}</el-tag>
                      </template>
                    </el-table-column>
                    <el-table-column prop="amount" label="金額" width="120" align="right">
                      <template #default="scope">
                        <span class="text-red font-bold">HK$ {{ scope.row.amount.toLocaleString() }}</span>
                      </template>
                    </el-table-column>
                    <el-table-column prop="status" label="狀態" width="100" align="center">
                      <template #default="scope">
                        <el-tag :type="scope.row.status === '已處理' ? 'success' : 'info'" size="small">{{ scope.row.status || '未處理' }}</el-tag>
                      </template>
                    </el-table-column>
                    <el-table-column label="操作" width="140" align="center">
                      <template #default="scope">
                        <el-button type="primary" link size="small" @click="toggleExpenseStatus(scope.row)">
                          {{ scope.row.status === '已處理' ? '取消' : '標記處理' }}
                        </el-button>
                        <el-button type="primary" link size="small" @click="openEditDialog(scope.row)">編輯</el-button>
                        <el-button type="danger" text size="small" @click="deleteRecord('expenses', scope.row.id)">刪除</el-button>
                      </template>
                    </el-table-column>
                  </el-table>
                </div>
              </el-collapse-item>
            </el-collapse>
          </div>
        </el-tab-pane>
        
        <!-- 收入紀錄 -->
        <el-tab-pane label="收入紀錄" name="income">
          <div v-if="incomes.length === 0" class="empty-state">
            <el-empty description="尚無任何收入紀錄" :image-size="80" />
          </div>

          <div v-else class="month-groups">
            <el-collapse v-model="activeIncomeMonths">
              <el-collapse-item 
                v-for="group in groupedIncomes" 
                :key="group.month" 
                :name="group.month"
              >
                <template #title>
                  <div class="group-title">
                    <span class="month-text">{{ group.month }}</span>
                    <span class="month-total text-green">總收入: HK$ {{ group.total.toLocaleString() }}</span>
                  </div>
                </template>
                
                <div class="table-wrapper">
                  <el-table :data="group.items" style="width: 100%; min-width: 400px;" stripe>
                    <el-table-column prop="name" label="收入名稱" min-width="150" />
                    <el-table-column prop="member_name" label="對象" width="100" />
                    <el-table-column prop="amount" label="金額" width="120" align="right">
                      <template #default="scope">
                        <span class="text-green font-bold">HK$ {{ scope.row.amount.toLocaleString() }}</span>
                      </template>
                    </el-table-column>
                    <el-table-column label="操作" width="120" align="center">
                      <template #default="scope">
                        <el-button type="primary" link size="small" @click="openEditDialog(scope.row)">編輯</el-button>
                        <el-button type="danger" text size="small" @click="deleteRecord('incomes', scope.row.id)">刪除</el-button>
                      </template>
                    </el-table-column>
                  </el-table>
                </div>
              </el-collapse-item>
            </el-collapse>
          </div>
        </el-tab-pane>
      </el-tabs>
    </el-card>

    <!-- 新增/編輯彈窗 -->
    <el-dialog v-model="dialogVisible" :title="dialogTitle" width="90%" max-width="500px">
      <el-form :model="form" label-position="top">
        <el-form-item label="紀錄月份" required>
          <el-date-picker v-model="form.year_month" type="month" placeholder="選擇月份" value-format="YYYY-MM" style="width: 100%" />
        </el-form-item>
        <el-form-item label="項目名稱" required>
          <el-input v-model="form.name" :placeholder="activeTab === 'expense' ? '例如：買餸、交租' : '例如：薪金、家用轉入'" />
        </el-form-item>
        <el-form-item label="金額 (HKD)" required>
          <el-input-number v-model="form.amount" :min="1" style="width: 100%" />
        </el-form-item>
        
        <el-form-item label="對象" required>
          <el-select v-model="form.member_name" style="width: 100%">
            <el-option v-for="m in members" :key="m.id" :label="m.name" :value="m.name" />
          </el-select>
        </el-form-item>
        
        <el-form-item label="類別" v-if="activeTab === 'expense'">
          <el-select v-model="form.category_name" style="width: 100%" allow-create filterable default-first-option>
            <el-option v-for="c in categories" :key="c.id" :label="c.name" :value="c.name" />
          </el-select>
        </el-form-item>
        
        <el-form-item label="狀態" v-if="activeTab === 'expense'">
          <el-radio-group v-model="form.status">
            <el-radio label="未處理">未處理</el-radio>
            <el-radio label="已處理">已處理</el-radio>
          </el-radio-group>
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" :loading="submitting" @click="submitForm">確認儲存</el-button>
      </template>
    </el-dialog>

    <!-- 匯入固定支出彈窗 -->
    <el-dialog v-model="importDialogVisible" title="匯入固定支出" width="90%" max-width="400px">
      <el-form label-position="top">
        <div class="preview-box" style="margin-bottom: 15px; background: #fffbeb; color: #d97706;">
          將為指定月份匯入設定好的固定支出模板。
        </div>
        <el-form-item label="選擇要匯入的月份" required>
          <el-date-picker v-model="importMonth" type="month" placeholder="選擇月份" value-format="YYYY-MM" style="width: 100%" />
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="importDialogVisible = false">取消</el-button>
        <el-button type="primary" :loading="importing" @click="executeImport">確認匯入</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage, ElMessageBox } from 'element-plus'
import { MagicStick } from '@element-plus/icons-vue'

const activeTab = ref('expense')
const expenses = ref([])
const incomes = ref([])
const members = ref([])
const categories = ref([])
const loading = ref(false)
const submitting = ref(false)
const importing = ref(false)

// 彈窗狀態
const dialogVisible = ref(false)
const isEdit = ref(false)

// 折疊面板展開狀態 (預設展開最近的兩個月)
const activeExpenseMonths = ref([])
const activeIncomeMonths = ref([])

// 匯入彈窗狀態
const importDialogVisible = ref(false)
const importMonth = ref(new Date().toISOString().slice(0, 7))

const dialogTitle = computed(() => {
  return isEdit.value ? '編輯紀錄' : `新增${activeTab.value === 'expense' ? '支出' : '收入'}`
})

const totalExpense = computed(() => expenses.value.reduce((sum, e) => sum + Number(e.amount), 0))
const totalIncome = computed(() => incomes.value.reduce((sum, i) => sum + Number(i.amount), 0))

// 分組邏輯
const groupData = (data, amountKey) => {
  const groups = {}
  data.forEach(item => {
    const m = item.year_month || '未知月份'
    if (!groups[m]) {
      groups[m] = { month: m, total: 0, items: [] }
    }
    groups[m].items.push(item)
    groups[m].total += Number(item[amountKey])
  })
  // 轉為陣列並按月份倒序
  return Object.values(groups).sort((a, b) => b.month.localeCompare(a.month))
}

const groupedExpenses = computed(() => groupData(expenses.value, 'amount'))
const groupedIncomes = computed(() => groupData(incomes.value, 'amount'))

const form = ref({ id: null, year_month: '', name: '', amount: 1, member_name: '', category_name: '', status: '未處理' })

const fetchDependencies = async () => {
  const [mRes, cRes] = await Promise.all([
    supabase.from('members').select('*').order('created_at', { ascending: true }),
    supabase.from('expense_categories').select('*').order('created_at', { ascending: true })
  ])
  if (mRes.data) members.value = mRes.data
  if (cRes.data) categories.value = cRes.data
}

const fetchData = async () => {
  loading.value = true
  
  const [expRes, incRes] = await Promise.all([
    supabase.from('expenses').select('*').order('year_month', { ascending: false }).order('created_at', { ascending: false }),
    supabase.from('incomes').select('*').order('year_month', { ascending: false }).order('created_at', { ascending: false })
  ])

  if (!expRes.error) expenses.value = expRes.data || []
  if (!incRes.error) incomes.value = incRes.data || []
  
  // 預設展開前 2 個月
  if (activeExpenseMonths.value.length === 0 && groupedExpenses.value.length > 0) {
    activeExpenseMonths.value = groupedExpenses.value.slice(0, 2).map(g => g.month)
  }
  if (activeIncomeMonths.value.length === 0 && groupedIncomes.value.length > 0) {
    activeIncomeMonths.value = groupedIncomes.value.slice(0, 2).map(g => g.month)
  }
  
  loading.value = false
}

const importFixedExpenses = () => {
  importMonth.value = new Date().toISOString().slice(0, 7)
  importDialogVisible.value = true
}

const executeImport = async () => {
  if (!importMonth.value) return ElMessage.warning('請選擇月份')
  
  try {
    importing.value = true
    const familyId = '00000000-0000-0000-0000-000000000001'

    const [fixedRes, currentRes] = await Promise.all([
      supabase.from('fixed_expenses').select('*'),
      supabase.from('expenses').select('name').eq('year_month', importMonth.value)
    ])

    if (fixedRes.error) throw fixedRes.error
    if (!fixedRes.data || fixedRes.data.length === 0) return ElMessage.warning('尚未設定任何固定支出模板')

    const existingNames = currentRes.data.map(e => e.name)
    const toInsert = []

    fixedRes.data.forEach(tpl => {
      if (!existingNames.includes(tpl.name)) {
        toInsert.push({
          family_id: familyId,
          year_month: importMonth.value,
          name: tpl.name,
          amount: tpl.amount,
          payer_name: tpl.payer_name,
          category_name: tpl.category_name,
          status: '未處理'
        })
      }
    })

    if (toInsert.length === 0) return ElMessage.info('該月份所有固定支出皆已匯入')

    const { error } = await supabase.from('expenses').insert(toInsert)
    if (error) throw error

    ElMessage.success(`✅ 成功匯入 ${toInsert.length} 筆固定支出至 ${importMonth.value}`)
    importDialogVisible.value = false
    fetchData()

  } catch (err) {
    ElMessage.error('匯入失敗：' + err.message)
  } finally {
    importing.value = false
  }
}

const toggleExpenseStatus = async (item) => {
  const newStatus = item.status === '已處理' ? '未處理' : '已處理'
  const { error } = await supabase.from('expenses').update({ status: newStatus }).eq('id', item.id)
  if (error) ElMessage.error('更新失敗：' + error.message)
  else {
    item.status = newStatus
  }
}

const openAddDialog = () => {
  isEdit.value = false
  const defaultMember = members.value.length > 0 ? members.value[0].name : ''
  const defaultCategory = categories.value.length > 0 ? categories.value[0].name : ''
  const currentMonth = new Date().toISOString().slice(0, 7)
  
  form.value = { 
    id: null, 
    year_month: currentMonth, 
    name: '', 
    amount: 1, 
    member_name: defaultMember, 
    category_name: defaultCategory, 
    status: '未處理' 
  }
  dialogVisible.value = true
}

const openEditDialog = (row) => {
  isEdit.value = true
  form.value = {
    id: row.id,
    year_month: row.year_month,
    name: row.name,
    amount: row.amount,
    member_name: activeTab.value === 'expense' ? row.payer_name : row.member_name,
    category_name: row.category_name || '',
    status: row.status || '未處理'
  }
  dialogVisible.value = true
}

const submitForm = async () => {
  if (!form.value.name || form.value.amount <= 0 || !form.value.year_month) return ElMessage.warning('請填寫完整資訊')
  if (!form.value.member_name) return ElMessage.warning('請選擇對象')
  
  submitting.value = true
  const familyId = '00000000-0000-0000-0000-000000000001'

  // 檢查是否為新分類，若是則自動存檔
  if (activeTab.value === 'expense' && form.value.category_name) {
    const isNewCategory = !categories.value.some(c => c.name === form.value.category_name)
    if (isNewCategory) {
      await supabase.from('expense_categories').insert([{
        family_id: familyId,
        name: form.value.category_name
      }])
      fetchDependencies()
    }
  }
  
  if (activeTab.value === 'expense') {
    const payload = {
      year_month: form.value.year_month,
      name: form.value.name,
      amount: form.value.amount,
      payer_name: form.value.member_name,
      category_name: form.value.category_name,
      status: form.value.status
    }

    let error
    if (isEdit.value) {
      const res = await supabase.from('expenses').update(payload).eq('id', form.value.id)
      error = res.error
    } else {
      const res = await supabase.from('expenses').insert([{
        ...payload,
        family_id: familyId
      }])
      error = res.error
    }

    if (error) ElMessage.error('儲存失敗：' + error.message)
    else {
      ElMessage.success(isEdit.value ? '✅ 支出已更新' : '✅ 支出已新增')
      dialogVisible.value = false
      fetchData()
    }
  } else {
    const payload = {
      year_month: form.value.year_month,
      name: form.value.name,
      amount: form.value.amount,
      member_name: form.value.member_name
    }

    let error
    if (isEdit.value) {
      const res = await supabase.from('incomes').update(payload).eq('id', form.value.id)
      error = res.error
    } else {
      const res = await supabase.from('incomes').insert([{
        ...payload,
        family_id: familyId
      }])
      error = res.error
    }

    if (error) ElMessage.error('儲存失敗：' + error.message)
    else {
      ElMessage.success(isEdit.value ? '✅ 收入已更新' : '✅ 收入已新增')
      dialogVisible.value = false
      fetchData()
    }
  }
  submitting.value = false
}

const deleteRecord = async (table, id) => {
  try {
    await ElMessageBox.confirm('確定要刪除這筆紀錄嗎？', '提示', { type: 'warning' })
    const { error } = await supabase.from(table).delete().eq('id', id)
    if (error) throw error
    ElMessage.success('已刪除')
    fetchData()
  } catch (err) {
    if (err !== 'cancel') ElMessage.error('刪除失敗')
  }
}

onMounted(() => {
  fetchData()
  fetchDependencies()
})
</script>

<style scoped>
.cashflow-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; }
.page-header h2 { margin: 0; color: #1e293b; font-size: 24px; font-weight: 700; }

.mb-20 { margin-bottom: 24px; }
.stat-card { border-radius: 20px; background: #ffffff; border: none; }
.stat-label { font-size: 14px; color: #64748b; margin-bottom: 6px; }
.stat-value { font-size: 24px; font-weight: bold; }
.text-red { color: #ef4444; }
.text-green { color: #10b981; }
.font-bold { font-weight: 600; }

.list-card { border-radius: 20px; border: none; overflow: hidden; }
.tool-bar { margin-bottom: 20px; display: flex; justify-content: flex-end; }

/* 折疊面板樣式 */
.month-groups :deep(.el-collapse) {
  border: none;
}
.month-groups :deep(.el-collapse-item__header) {
  background-color: #F8FAFC;
  border-radius: 12px;
  padding: 0 16px;
  margin-bottom: 12px;
  border-bottom: none;
  font-size: 15px;
}
.month-groups :deep(.el-collapse-item__wrap) {
  border-bottom: none;
  background-color: transparent;
}
.month-groups :deep(.el-collapse-item__content) {
  padding-bottom: 16px;
}
.group-title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding-right: 10px;
}
.month-text { font-weight: 700; color: #334155; }
.month-total { font-size: 13px; font-family: monospace; }

.table-wrapper { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }
.empty-state { padding: 40px 0; }

@media (max-width: 768px) {
  .page-header { flex-direction: column; align-items: flex-start; gap: 12px; }
  .header-actions { width: 100%; }
  .header-actions .el-button { width: 100%; }
  .tool-bar { justify-content: center; }
}
</style>

<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/CashflowView.vue
V: 1793971300
W: ai-wa-fs-002
═══ -->```

<!-- === FILE END: ./src/views/CashflowView.vue === -->

<!-- === FILE START: ./src/views/DashboardView.vue === -->

## 📄 ./src/views/DashboardView.vue

```vue
<template>
  <div class="dashboard-container" v-loading="loading">
    <div class="page-header">
      <h2>總覽儀表板</h2>
      <el-button type="primary" icon="DataLine" class="report-btn" @click="$router.push('/annual-report')">
        查看年度報表
      </el-button>
    </div>

    <!-- 家庭總餘額大卡 -->
    <el-row class="mb-20">
      <el-col :span="24">
        <el-card shadow="never" class="total-balance-card">
          <div class="card-icon">
            <el-icon><Money /></el-icon>
          </div>
          <div class="card-content">
            <div class="asset-label">家庭總餘額</div>
            <div class="asset-value">HK$ {{ totalBalance.toLocaleString() }}</div>
            <div class="asset-sub">共 1 個銀行帳號</div>
          </div>
        </el-card>
      </el-col>
    </el-row>

    <!-- 年度統計卡片 -->
    <el-row :gutter="20" class="mb-20">
      <el-col :xs="24" :sm="8" class="mb-mobile">
        <el-card shadow="never" class="stat-card income-card">
          <div class="stat-icon"><el-icon><TrendCharts /></el-icon></div>
          <div class="stat-info">
            <div class="stat-label">{{ currentYear }} 家庭總收入</div>
            <div class="stat-value text-green">HK$ {{ currentYearIncome.toLocaleString() }}</div>
            <div class="stat-sub">家用轉入 + 額外收入</div>
          </div>
        </el-card>
      </el-col>
      
      <el-col :xs="24" :sm="8" class="mb-mobile">
        <el-card shadow="never" class="stat-card expense-card">
          <div class="stat-icon"><el-icon><Bottom /></el-icon></div>
          <div class="stat-info">
            <div class="stat-label">{{ currentYear }} 年度總支出</div>
            <div class="stat-value text-red">HK$ {{ currentYearExpense.toLocaleString() }}</div>
            <div class="stat-sub">{{ activeMonthsCount }} 個月有紀錄</div>
          </div>
        </el-card>
      </el-col>

      <el-col :xs="24" :sm="8">
        <el-card shadow="never" class="stat-card balance-card">
          <div class="stat-icon"><el-icon><Wallet /></el-icon></div>
          <div class="stat-info">
            <div class="stat-label">{{ currentYear }} 年度淨餘額</div>
            <div class="stat-value" :class="currentYearNet >= 0 ? 'text-green' : 'text-red'">
              {{ currentYearNet >= 0 ? '+' : '' }} HK$ {{ currentYearNet.toLocaleString() }}
            </div>
            <div class="stat-sub">收支平衡</div>
          </div>
        </el-card>
      </el-col>
    </el-row>

    <!-- 年度總覽表格 -->
    <h3 class="section-title">年度總覽（前後 2 年）</h3>
    <el-card shadow="never" class="table-card">
      <div class="table-toolbar">
        <span class="total-count">共 {{ annualData.length }} 筆</span>
      </div>
      <div class="table-wrapper">
        <el-table :data="annualData" style="width: 100%; min-width: 500px;" stripe default-expand-all>
          <el-table-column prop="year" label="年度" width="140" sortable>
            <template #default="scope">
              <span class="font-bold text-slate-700">{{ scope.row.year }} 年</span>
              <el-tag v-if="scope.row.year === currentYear" size="small" type="primary" class="ml-2" effect="light">今年</el-tag>
            </template>
          </el-table-column>
          <el-table-column prop="income" label="家庭收入" align="right" sortable>
            <template #default="scope">
              <span class="text-green font-bold">HK$ {{ scope.row.income.toLocaleString() }}</span>
            </template>
          </el-table-column>
          <el-table-column prop="expense" label="總支出" align="right" sortable>
            <template #default="scope">
              <span class="text-red font-bold">HK$ {{ scope.row.expense.toLocaleString() }}</span>
            </template>
          </el-table-column>
          <el-table-column prop="net" label="淨餘額" align="right" sortable>
            <template #default="scope">
              <span :class="scope.row.net >= 0 ? 'text-green' : 'text-red'" class="font-bold">
                {{ scope.row.net >= 0 ? '+' : '' }} HK$ {{ scope.row.net.toLocaleString() }}
              </span>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-card>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage } from 'element-plus'
import { TrendCharts, Bottom, Wallet, DataLine, Money } from '@element-plus/icons-vue'

const loading = ref(false)

// 狀態
const totalBalance = ref(0)
const annualData = ref([])
const activeMonthsCount = ref(0)

const currentYear = new Date().getFullYear()

// 取得當前年度資料
const currentYearData = computed(() => {
  return annualData.value.find(d => d.year === currentYear) || { income: 0, expense: 0, net: 0 }
})

const currentYearIncome = computed(() => currentYearData.value.income)
const currentYearExpense = computed(() => currentYearData.value.expense)
const currentYearNet = computed(() => currentYearData.value.net)

const fetchDashboardData = async () => {
  loading.value = true
  
  try {
    // 1. 抓取最新總餘額
    const { data: balData } = await supabase
      .from('monthly_balances')
      .select('actual_balance')
      .order('year_month', { ascending: false })
      .limit(1)
      .maybeSingle()
    
    if (balData) totalBalance.value = Number(balData.actual_balance)

    // 2. 抓取所有收支紀錄 (用於計算年度)
    const [incRes, expRes, insRes, polRes] = await Promise.all([
      supabase.from('incomes').select('year_month, amount'),
      supabase.from('expenses').select('year_month, amount').eq('status', '已處理'),
      supabase.from('insurance_payments').select('year_month, policy_id').eq('status', '已轉'),
      supabase.from('insurance_policies').select('id, monthly_average')
    ])

    if (incRes.error) throw incRes.error
    if (expRes.error) throw expRes.error

    const policyMap = {}
    if (polRes.data) {
      polRes.data.forEach(p => { policyMap[p.id] = Number(p.monthly_average) })
    }

    // 3. 初始化前後 2 年的資料結構
    const tempYears = {}
    for (let i = -2; i <= 2; i++) {
      const y = currentYear + i
      tempYears[y] = { year: y, income: 0, expense: 0, net: 0 }
    }

    // 4. 計算收入
    if (incRes.data) {
      incRes.data.forEach(item => {
        if (!item.year_month) return
        const y = parseInt(item.year_month.split('-')[0])
        if (tempYears[y]) {
          tempYears[y].income += Number(item.amount)
        }
      })
    }

    // 5. 計算一般支出 (已處理)
    const monthsWithData = new Set()
    if (expRes.data) {
      expRes.data.forEach(item => {
        if (!item.year_month) return
        const y = parseInt(item.year_month.split('-')[0])
        if (tempYears[y]) {
          tempYears[y].expense += Number(item.amount)
          if (y === currentYear) monthsWithData.add(item.year_month)
        }
      })
    }

    // 6. 計算保險支出 (已轉)
    if (insRes.data) {
      insRes.data.forEach(item => {
        if (!item.year_month) return
        const y = parseInt(item.year_month.split('-')[0])
        const amount = policyMap[item.policy_id] || 0
        if (tempYears[y]) {
          tempYears[y].expense += amount
          if (y === currentYear) monthsWithData.add(item.year_month)
        }
      })
    }

    // 7. 計算淨額並轉為陣列
    const resultArray = []
    for (let i = -2; i <= 2; i++) {
      const y = currentYear + i
      tempYears[y].net = tempYears[y].income - tempYears[y].expense
      resultArray.push(tempYears[y])
    }

    activeMonthsCount.value = monthsWithData.size
    annualData.value = resultArray

  } catch (error) {
    console.error(error)
    ElMessage.error('載入總覽資料失敗')
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchDashboardData()
})
</script>

<style scoped>
.dashboard-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; }
.page-header h2 { margin: 0; color: #1e293b; font-size: 24px; font-weight: 700; }
.report-btn { border-radius: 10px; }

.mb-20 { margin-bottom: 24px; }
.mb-mobile { margin-bottom: 16px; }

/* 總餘額大卡 */
.total-balance-card {
  border-radius: 20px;
  border: none;
  background: linear-gradient(135deg, #1e293b 0%, #334155 100%);
  color: white;
}
.total-balance-card :deep(.el-card__body) {
  display: flex;
  align-items: center;
  padding: 30px;
}
.card-icon {
  width: 64px; height: 64px;
  border-radius: 16px;
  background: rgba(255,255,255,0.1);
  display: flex; justify-content: center; align-items: center;
  font-size: 32px; margin-right: 24px;
  color: #38bdf8;
}
.card-content { flex: 1; }
.asset-label { font-size: 15px; color: #94a3b8; margin-bottom: 8px; }
.asset-value { font-size: 36px; font-weight: bold; font-family: monospace; margin-bottom: 6px; }
.asset-sub { font-size: 13px; color: #64748b; }

/* 統計卡片 */
.stat-card { border-radius: 20px; border: none; position: relative; overflow: hidden; height: 100%; }
.stat-card :deep(.el-card__body) { display: flex; align-items: center; padding: 24px; height: 100%; }
.stat-icon { width: 56px; height: 56px; border-radius: 16px; display: flex; justify-content: center; align-items: center; font-size: 28px; margin-right: 20px; flex-shrink: 0; }
.stat-info { display: flex; flex-direction: column; }
.stat-label { font-size: 14px; color: #64748b; margin-bottom: 6px; }
.stat-value { font-size: 24px; font-weight: bold; white-space: nowrap; margin-bottom: 6px; }
.stat-sub { font-size: 12px; color: #94a3b8; }

.income-card .stat-icon { background: #ecfdf5; color: #10b981; }
.expense-card .stat-icon { background: #fef2f2; color: #ef4444; }
.balance-card .stat-icon { background: #EEF2FF; color: #6366F1; }

.text-green { color: #10b981; }
.text-red { color: #ef4444; }
.text-slate-700 { color: #334155; }
.font-bold { font-weight: 600; }

/* 年度表格 */
.section-title { margin: 0 0 16px 0; color: #1e293b; font-size: 18px; font-weight: 700; }
.table-card { border-radius: 20px; border: none; overflow: hidden; }
.table-toolbar { display: flex; justify-content: space-between; align-items: center; padding: 16px 24px; background: #F8FAFC; border-bottom: 1px solid #e2e8f0; }
.total-count { font-size: 14px; color: #64748b; }
.table-wrapper { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }

.ml-2 { margin-left: 8px; }

@media (max-width: 768px) {
  .page-header { flex-direction: column; align-items: flex-start; gap: 12px; }
  .report-btn { width: 100%; }
  .asset-value { font-size: 28px; }
  .stat-value { font-size: 20px; }
  .stat-icon { width: 48px; height: 48px; font-size: 24px; margin-right: 16px; }
}
</style>

<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/DashboardView.vue
V: 1793971200
W: ai-wa-fs-002
═══ -->```

<!-- === FILE END: ./src/views/DashboardView.vue === -->

<!-- === FILE START: ./src/views/FundView.vue === -->

## 📄 ./src/views/FundView.vue

```vue
<template>
  <div class="fund-container">
    <div class="page-header">
      <h2>基金投資表</h2>
      <div class="header-actions">
        <el-button @click="openFundDialog()">+ 新增基金</el-button>
        <el-button type="primary" @click="openSnapshotDialog()">+ 輸入快照</el-button>
      </div>
    </div>

    <!-- 總計卡片 -->
    <el-row :gutter="10" class="mb-20">
      <el-col :xs="12" :sm="8">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">總投入成本</div>
          <div class="stat-value">HK$ {{ totalCost.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="12" :sm="8">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">當前總現值</div>
          <div class="stat-value text-blue">HK$ {{ totalValue.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="24" :sm="8">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">總帳面盈虧</div>
          <div class="stat-value" :class="totalPnl >= 0 ? 'text-green' : 'text-red'">
            {{ totalPnl >= 0 ? '+' : '' }} HK$ {{ totalPnl.toLocaleString() }}
          </div>
        </el-card>
      </el-col>
    </el-row>

    <!-- 基金列表 -->
    <div v-if="loading" style="text-align: center; padding: 40px;">
      <el-icon class="is-loading" :size="30"><Loading /></el-icon>
    </div>
    <el-empty v-else-if="funds.length === 0" description="尚無基金，請點擊右上角新增。" />

    <div v-else class="fund-grid">
      <el-card v-for="fund in funds" :key="fund.id" class="fund-card" shadow="hover">
        <div class="card-header">
          <div>
            <h3>{{ fund.name }}</h3>
            <div class="linked-policy" v-if="fund.policy_id">
              <el-tag size="small" type="warning">關聯保單：{{ getPolicyName(fund.policy_id) }} ({{ fund.allocation_pct }}%)</el-tag>
            </div>
            <div class="linked-policy" v-else>
              <el-tag size="small" type="info">獨立基金</el-tag>
            </div>
          </div>
          <el-button type="danger" text size="small" @click="deleteFund(fund.id)">刪除</el-button>
        </div>
        
        <div class="card-body">
          <div class="info-row">
            <span class="label">推算投入成本</span>
            <span class="value">HK$ {{ getFundCost(fund).toLocaleString() }}</span>
          </div>
          <div class="info-row" v-if="fund.withdrawn_amount > 0">
            <span class="label text-orange">已提取/轉出金額</span>
            <span class="value text-orange">HK$ {{ Number(fund.withdrawn_amount).toLocaleString() }}</span>
          </div>
          <div class="info-row">
            <span class="label">當前現值</span>
            <span class="value text-blue">HK$ {{ getLatestValue(fund).toLocaleString() }}</span>
          </div>
          
          <div class="pnl-box" :class="getPnl(fund) >= 0 ? 'pnl-positive' : 'pnl-negative'">
            <span>真實盈虧</span>
            <strong>{{ getPnl(fund) >= 0 ? '+' : '' }} HK$ {{ getPnl(fund).toLocaleString() }}</strong>
          </div>
          
          <div class="latest-info" v-if="getLatestSnapshot(fund)">
            最新快照：{{ getLatestSnapshot(fund).year_month }} 
            ({{ getLatestSnapshot(fund).units }} 單位)
          </div>
        </div>

        <!-- 🆕 操作按鈕區 -->
        <div class="card-actions">
          <el-button type="primary" link size="small" @click="openHistoryDialog(fund)">查看歷史快照</el-button>
          <el-button type="warning" size="small" plain @click="openWithdrawDialog(fund)">記錄提款/轉出</el-button>
        </div>
      </el-card>
    </div>

    <!-- 新增基金彈窗 -->
    <el-dialog v-model="fundDialogVisible" title="新增基金" width="90%" max-width="450px">
      <el-form :model="fundForm" label-position="top">
        <el-form-item label="基金名稱" required>
          <el-input v-model="fundForm.name" placeholder="例如：駿利亨德森遠見基金" />
        </el-form-item>
        
        <el-form-item label="是否為基金保險的關聯基金？">
          <el-switch v-model="isLinkedToPolicy" active-text="關聯保單" inactive-text="獨立基金" />
        </el-form-item>

        <template v-if="isLinkedToPolicy">
          <el-form-item label="選擇關聯保單" required>
            <el-select v-model="fundForm.policy_id" style="width: 100%" placeholder="請選擇基金保險保單">
              <el-option v-for="p in fundPolicies" :key="p.id" :label="p.name" :value="p.id" />
            </el-select>
          </el-form-item>
          <el-form-item label="分配比例 (%)" required>
            <el-input-number v-model="fundForm.allocation_pct" :min="0" :max="100" style="width: 100%" />
          </el-form-item>
        </template>

        <template v-else>
          <el-form-item label="手動輸入成本 (HKD)" required>
            <el-input-number v-model="fundForm.manual_cost" :min="0" style="width: 100%" />
          </el-form-item>
        </template>
      </el-form>
      <template #footer>
        <el-button @click="fundDialogVisible = false">取消</el-button>
        <el-button type="primary" :loading="submitting" @click="submitFund">儲存</el-button>
      </template>
    </el-dialog>

    <!-- 輸入快照彈窗 -->
    <el-dialog v-model="snapshotDialogVisible" title="輸入基金快照" width="90%" max-width="450px">
      <el-form :model="snapshotForm" label-position="top">
        <el-form-item label="選擇基金" required>
          <el-select v-model="snapshotForm.fund_id" style="width: 100%">
            <el-option v-for="f in funds" :key="f.id" :label="f.name" :value="f.id" />
          </el-select>
        </el-form-item>
        <el-form-item label="月份" required>
          <el-date-picker v-model="snapshotForm.year_month" type="month" value-format="YYYY-MM" style="width: 100%" />
        </el-form-item>
        
        <el-row :gutter="10">
          <el-col :span="12">
            <el-form-item label="單位數" required>
              <el-input-number v-model="snapshotForm.units" :precision="4" :step="0.0001" style="width: 100%" />
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="基金價格 (USD)" required>
              <el-input-number v-model="snapshotForm.nav" :precision="4" :step="0.01" style="width: 100%" />
            </el-form-item>
          </el-col>
        </el-row>
        
        <el-form-item label="匯率 (USD to HKD)" required>
          <el-input-number v-model="snapshotForm.exchange_rate" :precision="4" :step="0.1" style="width: 100%" />
        </el-form-item>

        <div class="preview-box" v-if="snapshotForm.units && snapshotForm.nav">
          預估 HKD 現值：<strong>HK$ {{ computedSnapshotValue.toLocaleString() }}</strong>
        </div>
      </el-form>
      <template #footer>
        <el-button @click="snapshotDialogVisible = false">取消</el-button>
        <el-button type="primary" :loading="submitting" @click="submitSnapshot">儲存快照</el-button>
      </template>
    </el-dialog>

    <!-- 記錄提款彈窗 -->
    <el-dialog v-model="withdrawDialogVisible" title="記錄提款/轉出" width="90%" max-width="400px">
      <el-form label-position="top">
        <div class="preview-box" style="margin-bottom: 15px; background: #fffbeb; color: #d97706;">
          <span>將從「{{ currentFund?.name }}」扣除這筆金額。此操作不會影響你原本的投入成本，但會計入真實盈虧。</span>
        </div>
        <el-form-item label="提款/轉出金額 (HKD)" required>
          <el-input-number v-model="withdrawAmount" :min="1" style="width: 100%" />
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="withdrawDialogVisible = false">取消</el-button>
        <el-button type="primary" :loading="submitting" @click="submitWithdraw">確認記錄</el-button>
      </template>
    </el-dialog>

    <!-- 🆕 歷史快照彈窗 -->
    <el-dialog v-model="historyDialogVisible" :title="`歷史快照 - ${currentFund?.name}`" width="90%" max-width="600px">
      <div class="table-wrapper">
        <el-table :data="currentFundSnaps" style="width: 100%; min-width: 500px;" stripe>
          <el-table-column prop="year_month" label="月份" width="100" />
          <el-table-column prop="units" label="單位數" width="100" align="right" />
          <el-table-column prop="nav" label="價格 (USD)" width="100" align="right" />
          <el-table-column prop="exchange_rate" label="匯率" width="80" align="right" />
          <el-table-column prop="total_value_hkd" label="總值 (HKD)" align="right">
            <template #default="scope">
              <span class="font-bold text-blue">HK$ {{ Number(scope.row.total_value_hkd).toLocaleString() }}</span>
            </template>
          </el-table-column>
          <el-table-column label="操作" width="80" align="center">
            <template #default="scope">
              <el-button type="danger" link size="small" @click="deleteSnapshot(scope.row.id)">刪除</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <el-empty v-if="currentFundSnaps.length === 0" description="尚無快照紀錄" :image-size="60" />
    </el-dialog>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage, ElMessageBox } from 'element-plus'
import { Loading } from '@element-plus/icons-vue'

const funds = ref([])
const snapshots = ref([])
const policies = ref([])
const loading = ref(false)
const submitting = ref(false)

// 彈窗狀態
const fundDialogVisible = ref(false)
const snapshotDialogVisible = ref(false)
const withdrawDialogVisible = ref(false)
const historyDialogVisible = ref(false) // 🆕

// 表單狀態
const isLinkedToPolicy = ref(false)
const fundForm = ref({ name: '', policy_id: '', allocation_pct: 0, manual_cost: 0 })
const snapshotForm = ref({ fund_id: '', year_month: new Date().toISOString().slice(0, 7), units: 0, nav: 0, exchange_rate: 7.8 })

// 提款狀態
const currentFund = ref(null)
const withdrawAmount = ref(0)

// 🆕 歷史快照狀態
const currentFundSnaps = ref([])

const fundPolicies = computed(() => policies.value.filter(p => p.type === 'fund_insurance'))

const getPolicyName = (policyId) => {
  const p = policies.value.find(x => x.id === policyId)
  return p ? p.name : '未知保單'
}

const getFundCost = (fund) => {
  if (!fund.policy_id) return Number(fund.manual_cost) || 0
  const policy = policies.value.find(p => p.id === fund.policy_id)
  if (!policy) return Number(fund.manual_cost) || 0

  const start = new Date(policy.start_date)
  const now = new Date()
  const diffMonths = (now.getFullYear() - start.getFullYear()) * 12 + (now.getMonth() - start.getMonth())
  const paidMonths = Math.max(0, diffMonths + 1)

  const totalPolicyCost = Number(policy.monthly_average) * paidMonths
  const fundCost = totalPolicyCost * (Number(fund.allocation_pct) / 100)
  return Math.round(fundCost)
}

const getPnl = (fund) => {
  const currentVal = getLatestValue(fund)
  const withdrawn = Number(fund.withdrawn_amount) || 0
  const cost = getFundCost(fund)
  return (currentVal + withdrawn) - cost
}

const totalCost = computed(() => funds.value.reduce((sum, f) => sum + getFundCost(f), 0))
const totalValue = computed(() => funds.value.reduce((sum, f) => sum + getLatestValue(f), 0))
const totalPnl = computed(() => totalValue.value - totalCost.value)

const getLatestSnapshot = (fund) => {
  const fSnaps = snapshots.value.filter(s => s.fund_id === fund.id)
  if (fSnaps.length === 0) return null
  return fSnaps.sort((a, b) => b.year_month.localeCompare(a.year_month))[0]
}

const getLatestValue = (fund) => {
  const latest = getLatestSnapshot(fund)
  return latest ? Number(latest.total_value_hkd) : 0
}

const computedSnapshotValue = computed(() => {
  const v = (snapshotForm.value.units || 0) * (snapshotForm.value.nav || 0) * (snapshotForm.value.exchange_rate || 0)
  return Math.round(v)
})

const fetchData = async () => {
  loading.value = true
  const [fundRes, snapRes, polRes] = await Promise.all([
    supabase.from('funds').select('*').order('created_at', { ascending: false }),
    supabase.from('fund_snapshots').select('*'),
    supabase.from('insurance_policies').select('*').order('created_at', { ascending: false })
  ])
  if (!fundRes.error) funds.value = fundRes.data || []
  if (!snapRes.error) snapshots.value = snapRes.data || []
  if (!polRes.error) policies.value = polRes.data || []
  loading.value = false
}

// 🆕 打開歷史快照彈窗
const openHistoryDialog = (fund) => {
  currentFund.value = fund
  // 篩選該基金的所有快照，並按時間降序排列
  currentFundSnaps.value = snapshots.value
    .filter(s => s.fund_id === fund.id)
    .sort((a, b) => b.year_month.localeCompare(a.year_month))
  historyDialogVisible.value = true
}

// 🆕 刪除快照
const deleteSnapshot = async (id) => {
  try {
    await ElMessageBox.confirm('確定要刪除這筆快照嗎？這將會影響該月的盈虧計算。', '警告', { type: 'warning' })
    const { error } = await supabase.from('fund_snapshots').delete().eq('id', id)
    if (error) throw error
    ElMessage.success('已刪除')
    fetchData() // 重新抓取
    // 重新整理彈窗內的列表
    currentFundSnaps.value = currentFundSnaps.value.filter(s => s.id !== id)
  } catch (err) {
    if (err !== 'cancel') ElMessage.error('刪除失敗')
  }
}

const openWithdrawDialog = (fund) => {
  currentFund.value = fund
  withdrawAmount.value = 0
  withdrawDialogVisible.value = true
}

const submitWithdraw = async () => {
  if (withdrawAmount.value <= 0) return ElMessage.warning('請輸入大於 0 的金額')
  submitting.value = true
  
  const newWithdrawn = (Number(currentFund.value.withdrawn_amount) || 0) + withdrawAmount.value
  
  const { error } = await supabase.from('funds')
    .update({ withdrawn_amount: newWithdrawn })
    .eq('id', currentFund.value.id)

  submitting.value = false
  if (error) ElMessage.error('記錄失敗：' + error.message)
  else {
    ElMessage.success('✅ 已記錄提款')
    withdrawDialogVisible.value = false
    fetchData()
  }
}

const openFundDialog = () => {
  isLinkedToPolicy.value = false
  fundForm.value = { name: '', policy_id: '', allocation_pct: 0, manual_cost: 0 }
  fundDialogVisible.value = true
}

const submitFund = async () => {
  if (!fundForm.value.name) return ElMessage.warning('請輸入名稱')
  
  let payload = {
    family_id: '00000000-0000-0000-0000-000000000001',
    name: fundForm.value.name,
    policy_id: null,
    allocation_pct: 0,
    manual_cost: 0
  }

  if (isLinkedToPolicy.value) {
    if (!fundForm.value.policy_id || fundForm.value.allocation_pct <= 0) return ElMessage.warning('請選擇保單並填寫比例')
    payload.policy_id = fundForm.value.policy_id
    payload.allocation_pct = fundForm.value.allocation_pct
  } else {
    if (fundForm.value.manual_cost <= 0) return ElMessage.warning('請輸入投入成本')
    payload.manual_cost = fundForm.value.manual_cost
  }

  submitting.value = true
  const { error } = await supabase.from('funds').insert([payload])
  submitting.value = false
  
  if (error) ElMessage.error('失敗：' + error.message)
  else {
    ElMessage.success('✅ 基金已新增')
    fundDialogVisible.value = false
    fetchData()
  }
}

const openSnapshotDialog = () => {
  if (funds.value.length === 0) return ElMessage.warning('請先新增基金主檔')
  snapshotForm.value = {
    fund_id: funds.value[0].id,
    year_month: new Date().toISOString().slice(0, 7),
    units: 0, nav: 0, exchange_rate: 7.8
  }
  snapshotDialogVisible.value = true
}

const submitSnapshot = async () => {
  if (!snapshotForm.value.fund_id || !snapshotForm.value.year_month) return ElMessage.warning('請填寫完整')
  submitting.value = true
  const { error } = await supabase.from('fund_snapshots').insert([{
    fund_id: snapshotForm.value.fund_id,
    year_month: snapshotForm.value.year_month,
    units: snapshotForm.value.units,
    nav: snapshotForm.value.nav,
    exchange_rate: snapshotForm.value.exchange_rate,
    total_value_hkd: computedSnapshotValue.value
  }])
  submitting.value = false
  if (error) ElMessage.error('失敗：' + error.message)
  else {
    ElMessage.success('✅ 快照已儲存')
    snapshotDialogVisible.value = false
    fetchData()
  }
}

const deleteFund = async (id) => {
  try {
    await ElMessageBox.confirm('刪除基金將一併刪除所有快照，確定嗎？', '警告', { type: 'warning' })
    const { error } = await supabase.from('funds').delete().eq('id', id)
    if (error) throw error
    ElMessage.success('已刪除')
    fetchData()
  } catch (err) {
    if (err !== 'cancel') ElMessage.error('刪除失敗')
  }
}

onMounted(() => fetchData())
</script>

<style scoped>
.fund-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; flex-wrap: wrap; gap: 10px; }
.page-header h2 { margin: 0; color: #1e293b; }
.header-actions { display: flex; gap: 10px; }

.mb-20 { margin-bottom: 20px; }
.stat-card { border-radius: 8px; background: #f8fafc; }
.stat-label { font-size: 13px; color: #64748b; margin-bottom: 5px; }
.stat-value { font-size: 20px; font-weight: bold; }
.text-red { color: #ef4444; }
.text-green { color: #10b981; }
.text-blue { color: #3b82f6; }
.text-orange { color: #f59e0b; }
.font-bold { font-weight: 600; }

.fund-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 16px; }
.fund-card { border-radius: 12px; border: none; box-shadow: 0 2px 12px 0 rgba(0,0,0,0.05); }
.card-header { display: flex; justify-content: space-between; align-items: flex-start; border-bottom: 1px solid #f1f5f9; padding-bottom: 10px; margin-bottom: 15px; }
.card-header h3 { margin: 0 0 5px 0; font-size: 16px; color: #0f172a; }
.linked-policy { font-size: 12px; }

.card-body { display: flex; flex-direction: column; gap: 8px; }
.info-row { display: flex; justify-content: space-between; font-size: 14px; }
.label { color: #64748b; }
.value { font-weight: 600; }

.pnl-box { display: flex; justify-content: space-between; padding: 10px; border-radius: 6px; font-weight: bold; margin-top: 5px; }
.pnl-positive { background: #ecfdf5; color: #10b981; }
.pnl-negative { background: #fef2f2; color: #ef4444; }

.latest-info { font-size: 12px; color: #94a3b8; margin-top: 5px; text-align: right; }
.preview-box { background: #eff6ff; color: #1d4ed8; padding: 10px 15px; border-radius: 6px; display: flex; align-items: center; gap: 8px; font-size: 13px; }

/* 🆕 按鈕區排版 */
.card-actions { display: flex; justify-content: space-between; margin-top: 10px; border-top: 1px dashed #e2e8f0; padding-top: 10px; }

.table-wrapper { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }

@media (max-width: 768px) {
  .header-actions { width: 100%; justify-content: flex-end; margin-top: 10px; }
  .fund-grid { display: flex; flex-wrap: nowrap; overflow-x: auto; padding-bottom: 12px; gap: 12px; }
  .fund-card { flex: 0 0 85%; min-width: 280px; }
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/FundView.vue
V: 1793811100
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./src/views/FundView.vue === -->

<!-- === FILE START: ./src/views/InsuranceView.vue === -->

## 📄 ./src/views/InsuranceView.vue

```vue
<template>
  <div class="insurance-container">
    <div class="page-header">
      <h2>保險供款表</h2>
      <el-button type="primary" @click="openAddDialog">+ 新增保單</el-button>
    </div>

    <!-- 本月總供款額統計 -->
    <el-row :gutter="20" class="mb-24">
      <el-col :span="24">
        <el-card shadow="never" class="stat-card">
          <div class="stat-content">
            <div class="stat-icon"><el-icon><Money /></el-icon></div>
            <div class="stat-info">
              <div class="stat-label">本月保單總供款額 ({{ currentYearMonth }})</div>
              <div class="stat-value text-blue">HK$ {{ monthlyTotal.toLocaleString() }}</div>
            </div>
          </div>
        </el-card>
      </el-col>
    </el-row>

    <div v-if="loading" style="text-align: center; padding: 40px;">
      <el-icon class="is-loading" :size="30"><Loading /></el-icon>
    </div>
    
    <el-empty v-else-if="policies.length === 0" description="目前尚無保單資料，請點擊右上角新增。" />

    <div v-else class="policy-grid">
      <el-card v-for="p in policies" :key="p.id" class="policy-card" shadow="hover">
        <div class="card-header">
          <div class="title-area">
            <h3>{{ p.name }}</h3>
            <span class="company">{{ p.company || '未填寫公司' }}</span>
          </div>
          <div class="header-right">
            <el-tag :type="p.type === 'fund_insurance' ? 'warning' : 'primary'" size="small" style="margin-right: 8px;">
              {{ p.type === 'fund_insurance' ? '基金保險' : '普通保險' }}
            </el-tag>
            <el-dropdown trigger="click" @command="(cmd) => handleCommand(cmd, p)">
              <el-button text size="small"><el-icon><MoreFilled /></el-icon></el-button>
              <template #dropdown>
                <el-dropdown-menu>
                  <el-dropdown-item command="edit">編輯保單</el-dropdown-item>
                  <el-dropdown-item command="delete" style="color: #ef4444;">刪除保單</el-dropdown-item>
                </el-dropdown-menu>
              </template>
            </el-dropdown>
          </div>
        </div>

        <div class="card-body">
          <div class="info-row">
            <span class="label">本期月均</span>
            <span class="value text-blue">HK$ {{ p.monthly_average.toLocaleString() }}</span>
          </div>
          <div class="info-row">
            <span class="label">繳費方式</span>
            <span class="value">{{ p.payment_freq }} (HK$ {{ p.premium_amount.toLocaleString() }})</span>
          </div>
          
          <div class="progress-area">
            <div class="info-row" style="margin-bottom: 6px;">
              <span class="label">供款進度</span>
              <span class="value">{{ getProgress(p).text }}</span>
            </div>
            <el-progress 
              :percentage="getProgress(p).pct" 
              :show-text="false" 
              :stroke-width="8"
              :color="getProgress(p).pct >= 100 ? '#10b981' : '#6366F1'"
            />
          </div>
        </div>
      </el-card>
    </div>

    <!-- 新增/編輯保單彈窗 -->
    <el-dialog v-model="dialogVisible" :title="isEdit ? '編輯保單' : '新增保單'" width="90%" max-width="500px" :close-on-click-modal="false">
      <el-form :model="form" label-position="top">
        <el-form-item label="保單名稱" required>
          <el-input v-model="form.name" placeholder="例如：宏利儲蓄保險" />
        </el-form-item>
        <el-form-item label="保險公司">
          <el-input v-model="form.company" placeholder="例如：宏利" />
        </el-form-item>
        <el-row :gutter="12">
          <el-col :span="12">
            <el-form-item label="開始日期" required>
              <el-date-picker v-model="form.start_date" type="date" style="width: 100%" value-format="YYYY-MM-DD" />
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="供款年期 (年)" required>
              <el-input-number v-model="form.total_years" :min="1" style="width: 100%" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="12">
          <el-col :span="12">
            <el-form-item label="繳費頻率" required>
              <el-select v-model="form.payment_freq" style="width: 100%">
                <el-option label="月繳" value="月繳" />
                <el-option label="年繳" value="年繳" />
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item :label="`繳費金額 (${form.payment_freq})`" required>
              <el-input-number v-model="form.premium_amount" :min="0" style="width: 100%" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item label="保單類型" required>
          <el-radio-group v-model="form.type">
            <el-radio label="normal">普通保險</el-radio>
            <el-radio label="fund_insurance">基金保險</el-radio>
          </el-radio-group>
        </el-form-item>
        <div v-if="form.premium_amount > 0" class="preview-box">
          <el-icon><InfoFilled /></el-icon>
          <span>系統將自動平攤：每月約 <strong>HK$ {{ computedMonthlyAverage.toLocaleString() }}</strong></span>
        </div>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" :loading="submitting" @click="submitForm">確認儲存</el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage, ElMessageBox } from 'element-plus'
import { Loading, InfoFilled, MoreFilled, Money } from '@element-plus/icons-vue'

const policies = ref([])
const payments = ref([]) 
const loading = ref(true)
const dialogVisible = ref(false)
const submitting = ref(false)
const isEdit = ref(false)

const currentYearMonth = computed(() => {
  const d = new Date()
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}`
})

// 本月總供款額
const monthlyTotal = computed(() => {
  return policies.value.reduce((sum, p) => sum + Number(p.monthly_average), 0)
})

// 計算進度：已轉筆數 / 總期數
const getProgress = (p) => {
  const policyPayments = payments.value.filter(pay => pay.policy_id === p.id)
  const paidCount = policyPayments.filter(pay => pay.status === '已轉').length
  const totalPeriods = p.total_years * 12
  const pct = totalPeriods > 0 ? Math.min(100, Math.round((paidCount / totalPeriods) * 100)) : 0
  return {
    text: `${paidCount} / ${totalPeriods} 期 (${pct}%)`,
    pct: pct
  }
}

const form = ref({ id: null, name: '', company: '', start_date: '', total_years: 10, payment_freq: '月繳', premium_amount: 0, type: 'normal' })
const computedMonthlyAverage = computed(() => form.value.payment_freq === '年繳' ? Math.round((form.value.premium_amount || 0) / 12) : (form.value.premium_amount || 0))

const fetchData = async () => {
  loading.value = true
  const [policiesRes, paymentsRes] = await Promise.all([
    supabase.from('insurance_policies').select('*').order('created_at', { ascending: false }),
    supabase.from('insurance_payments').select('*')
  ])

  if (policiesRes.error) ElMessage.error('載入保單失敗')
  else policies.value = policiesRes.data || []

  if (paymentsRes.error) ElMessage.error('載入狀態失敗')
  else payments.value = paymentsRes.data || []

  loading.value = false
}

const openAddDialog = () => {
  isEdit.value = false
  form.value = { id: null, name: '', company: '', start_date: new Date().toISOString().split('T')[0], total_years: 10, payment_freq: '月繳', premium_amount: 0, type: 'normal' }
  dialogVisible.value = true
}

const handleCommand = (command, policy) => {
  if (command === 'edit') {
    isEdit.value = true
    form.value = { ...policy }
    dialogVisible.value = true
  } else if (command === 'delete') {
    deletePolicy(policy.id, policy.name)
  }
}

const deletePolicy = async (id, name) => {
  try {
    await ElMessageBox.confirm(`刪除保單「${name}」將會一併刪除其所有繳費紀錄，確定嗎？`, '警告', { type: 'warning', confirmButtonText: '確定刪除', cancelButtonText: '取消' })
    const { error } = await supabase.from('insurance_policies').delete().eq('id', id)
    if (error) throw error
    ElMessage.success('保單已刪除')
    fetchData()
  } catch (err) {
    if (err !== 'cancel') ElMessage.error('刪除失敗：' + (err.message || '未知錯誤'))
  }
}

const submitForm = async () => {
  if (!form.value.name || !form.value.start_date || form.value.premium_amount <= 0) return ElMessage.warning('請填寫完整資訊')
  submitting.value = true
  
  const payload = {
    name: form.value.name, 
    company: form.value.company, 
    start_date: form.value.start_date,
    total_years: form.value.total_years, 
    payment_freq: form.value.payment_freq,
    premium_amount: form.value.premium_amount, 
    monthly_average: computedMonthlyAverage.value,
    type: form.value.type
  }

  if (isEdit.value) {
    // 更新
    const { error } = await supabase.from('insurance_policies').update(payload).eq('id', form.value.id)
    if (error) ElMessage.error('更新失敗：' + error.message)
    else {
      ElMessage.success('✅ 保單已更新')
      dialogVisible.value = false
      fetchData()
    }
  } else {
    // 新增保單
    const familyId = '00000000-0000-0000-0000-000000000001'
    const { data: newPolicy, error } = await supabase.from('insurance_policies').insert([{
      family_id: familyId,
      ...payload
    }]).select().single()

    if (error) {
      ElMessage.error('新增失敗：' + error.message)
    } else {
      // 🆕 自動生成歷史待辦
      try {
        const start = new Date(form.value.start_date)
        const now = new Date()
        
        // 計算要生成到哪一個月
        const endMonthStr = currentYearMonth.value // 生成到當前月份
        
        let current = new Date(start.getFullYear(), start.getMonth(), 1)
        const end = new Date(now.getFullYear(), now.getMonth(), 1)
        
        const paymentsToInsert = []
        
        while (current <= end) {
          const yyyy = current.getFullYear()
          const mm = String(current.getMonth() + 1).padStart(2, '0')
          const ym = `${yyyy}-${mm}`
          
          paymentsToInsert.push({
            policy_id: newPolicy.id,
            year_month: ym,
            status: '未轉'
          })
          
          current.setMonth(current.getMonth() + 1)
        }

        if (paymentsToInsert.length > 0) {
          const { error: insErr } = await supabase.from('insurance_payments').insert(paymentsToInsert)
          if (insErr) console.error('自動生成歷史紀錄失敗', insErr)
        }
      } catch (e) {
        console.error('生成歷史邏輯錯誤', e)
      }

      ElMessage.success('✅ 保單已新增，並已生成歷史待辦')
      dialogVisible.value = false
      fetchData()
    }
  }
  submitting.value = false
}

onMounted(() => fetchData())
</script>

<style scoped>
.insurance-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; }
.page-header h2 { margin: 0; color: #1e293b; font-size: 24px; font-weight: 700; }

.mb-24 { margin-bottom: 24px; }

/* 統計卡片 */
.stat-card { border-radius: 20px; border: none; background: #ffffff; }
.stat-content { display: flex; align-items: center; padding: 8px; }
.stat-icon { width: 56px; height: 56px; border-radius: 16px; background: #EFF6FF; color: #3B82F6; display: flex; justify-content: center; align-items: center; font-size: 28px; margin-right: 20px; }
.stat-label { font-size: 14px; color: #64748b; margin-bottom: 4px; }
.stat-value { font-size: 24px; font-weight: bold; }
.text-blue { color: #3B82F6; }

/* 保單列表網格 */
.policy-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 20px; }
.policy-card { border-radius: 20px; border: 1px solid rgba(0,0,0,0.03); box-shadow: 0 4px 20px -2px rgba(0,0,0,0.05); }
.card-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 16px; border-bottom: 1px solid #f1f5f9; padding-bottom: 12px; }
.header-right { display: flex; align-items: center; }
.title-area h3 { margin: 0 0 6px 0; color: #1e293b; font-size: 18px; font-weight: 700; }
.company { font-size: 13px; color: #64748b; }
.card-body { display: flex; flex-direction: column; gap: 10px; }
.info-row { display: flex; justify-content: space-between; font-size: 14px; }
.label { color: #64748b; }
.value { font-weight: 600; color: #334155; }
.progress-area { margin-top: 12px; background: #F8FAFC; padding: 12px 16px; border-radius: 12px; }
.preview-box { background: #EEF2FF; color: #6366F1; padding: 12px 16px; border-radius: 12px; display: flex; align-items: center; gap: 8px; font-size: 14px; margin-top: 12px; }
</style>

<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/InsuranceView.vue
V: 1793971500
W: ai-wa-fs-002
═══ -->```

<!-- === FILE END: ./src/views/InsuranceView.vue === -->

<!-- === FILE START: ./src/views/LoginView.vue === -->

## 📄 ./src/views/LoginView.vue

```vue
<template>
  <div class="login-container">
    <!-- 背景裝飾 -->
    <div class="bg-circle circle-1"></div>
    <div class="bg-circle circle-2"></div>

    <el-card class="login-card">
      <div class="login-header">
        <div class="logo-icon">
          <el-icon><Wallet /></el-icon>
        </div>
        <h2>家庭入帳 V3</h2>
        <p>專屬家庭財務與資產管理</p>
      </div>
      
      <el-form @submit.prevent="handleLogin" class="login-form">
        <el-form-item>
          <el-input 
            v-model="account" 
            placeholder="請輸入帳號 (例如: admin)" 
            size="large" 
            prefix-icon="User"
            class="custom-input"
          />
        </el-form-item>
        <el-form-item>
          <el-input 
            v-model="password" 
            type="password" 
            placeholder="請輸入密碼" 
            size="large" 
            show-password 
            prefix-icon="Lock"
            class="custom-input"
            @keyup.enter="handleLogin"
          />
        </el-form-item>
        
        <el-button 
          type="primary" 
          native-type="submit" 
          :loading="loading" 
          class="login-btn" 
          size="large"
        >
          登入系統
        </el-button>
      </el-form>

      <!-- 錯誤訊息顯示 -->
      <transition name="el-fade-in">
        <div v-if="errorMsg" class="error-box">
          <el-icon><WarningFilled /></el-icon>
          <span>{{ errorMsg }}</span>
        </div>
      </transition>

    </el-card>
    
    <div class="footer-text">
      Powered by Vue 3 & Supabase
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage } from 'element-plus'
import { Wallet, WarningFilled } from '@element-plus/icons-vue'

const account = ref('')
const password = ref('')
const loading = ref(false)
const errorMsg = ref('')

const handleLogin = async () => {
  if (!account.value || !password.value) {
    ElMessage.warning('請輸入帳號與密碼')
    return
  }

  loading.value = true
  errorMsg.value = ''
  
  try {
    const cleanAccount = account.value.trim()
    const virtualEmail = cleanAccount.includes('@') ? cleanAccount : `${cleanAccount}@fs-app.com`

    const { data, error } = await supabase.auth.signInWithPassword({
      email: virtualEmail,
      password: password.value,
    })

    if (error) throw error
    
    ElMessage.success('登入成功！')
    window.location.reload()
    
  } catch (error) {
    errorMsg.value = error.message === 'Invalid login credentials' 
      ? '帳號或密碼錯誤，請重新輸入' 
      : '登入失敗：' + error.message
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.login-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
  position: relative;
  overflow: hidden;
  font-family: 'Noto Sans TC', sans-serif;
}

/* 背景光暈裝飾 */
.bg-circle {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  z-index: 0;
}
.circle-1 {
  width: 300px;
  height: 300px;
  background: rgba(59, 130, 246, 0.4);
  top: -10%;
  left: -10%;
}
.circle-2 {
  width: 400px;
  height: 400px;
  background: rgba(16, 185, 129, 0.2);
  bottom: -20%;
  right: -20%;
}

.login-card {
  width: 100%;
  max-width: 380px;
  padding: 30px 20px;
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  background: rgba(30, 41, 59, 0.7);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
  z-index: 1;
}

.login-header {
  text-align: center;
  margin-bottom: 30px;
}
.logo-icon {
  width: 60px;
  height: 60px;
  margin: 0 auto 15px;
  background: linear-gradient(135deg, #3b82f6, #06b6d4);
  border-radius: 16px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 32px;
  color: white;
  box-shadow: 0 8px 16px rgba(59, 130, 246, 0.3);
}
.login-header h2 {
  margin: 0 0 8px 0;
  color: #f8fafc;
  font-size: 24px;
  font-weight: 700;
  letter-spacing: 1px;
}
.login-header p {
  margin: 0;
  color: #94a3b8;
  font-size: 13px;
}

.login-form :deep(.el-input__wrapper) {
  background: rgba(15, 23, 42, 0.6);
  box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.1) inset;
  border-radius: 8px;
}
.login-form :deep(.el-input__inner) {
  color: #e2e8f0;
}
.login-form :deep(.el-input__prefix) {
  color: #64748b;
}

.login-btn {
  width: 100%;
  margin-top: 10px;
  height: 44px;
  font-size: 16px;
  font-weight: 600;
  border-radius: 8px;
  background: linear-gradient(90deg, #3b82f6, #06b6d4);
  border: none;
  transition: transform 0.2s, box-shadow 0.2s;
}
.login-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(59, 130, 246, 0.4);
}

.error-box {
  margin-top: 20px;
  padding: 10px;
  background: rgba(239, 68, 68, 0.1);
  border: 1px solid rgba(239, 68, 68, 0.2);
  border-radius: 8px;
  color: #fca5a5;
  font-size: 13px;
  display: flex;
  align-items: center;
  gap: 8px;
  justify-content: center;
}

.footer-text {
  position: absolute;
  bottom: 20px;
  color: #475569;
  font-size: 12px;
  z-index: 1;
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/LoginView.vue
V: 1793811500
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./src/views/LoginView.vue === -->

<!-- === FILE START: ./src/views/SettingsView.vue === -->

## 📄 ./src/views/SettingsView.vue

```vue
<template>
  <div class="settings-container">
    <div class="page-header">
      <h2>系統設定</h2>
    </div>

    <el-tabs v-model="activeTab" class="settings-tabs">
      
      <!-- 家庭成員管理 -->
      <el-tab-pane label="家庭成員" name="members">
        <div class="tab-content">
          <div class="action-bar">
            <span class="hint">新增後，記帳時的下拉選單會自動更新。</span>
            <el-button type="primary" @click="openDialog('member')">+ 新增成員</el-button>
          </div>
          <el-table :data="members" style="width: 100%" stripe v-loading="loading">
            <el-table-column prop="name" label="成員名稱" min-width="120" />
            <el-table-column prop="role" label="角色/關係" width="150" />
            <el-table-column label="操作" width="150" align="center">
              <template #default="scope">
                <el-button type="primary" link size="small" @click="openDialog('member', scope.row)">編輯</el-button>
                <el-button type="danger" link size="small" @click="deleteItem('members', scope.row.id)">刪除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </div>
      </el-tab-pane>

      <!-- 支出分類管理 -->
      <el-tab-pane label="支出分類" name="categories">
        <div class="tab-content">
          <div class="action-bar">
            <span class="hint">新增後，記帳時的分類選單會自動更新。</span>
            <el-button type="primary" @click="openDialog('category')">+ 新增分類</el-button>
          </div>
          <el-table :data="categories" style="width: 100%" stripe v-loading="loading">
            <el-table-column prop="name" label="分類名稱" min-width="120" />
            <el-table-column label="操作" width="150" align="center">
              <template #default="scope">
                <el-button type="primary" link size="small" @click="openDialog('category', scope.row)">編輯</el-button>
                <el-button type="danger" link size="small" @click="deleteItem('expense_categories', scope.row.id)">刪除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </div>
      </el-tab-pane>

      <!-- 固定支出模板 -->
      <el-tab-pane label="固定支出" name="fixed">
        <div class="tab-content">
          <div class="action-bar">
            <span class="hint">設定後，每月初可在「收支流水」一鍵匯入這些項目。</span>
            <el-button type="primary" @click="openDialog('fixed')">+ 新增模板</el-button>
          </div>
          <el-table :data="fixedExpenses" style="width: 100%" stripe v-loading="loading">
            <el-table-column prop="name" label="項目名稱" min-width="120" />
            <el-table-column prop="amount" label="金額" width="100" align="right">
              <template #default="scope">
                <span class="text-red font-bold">HK$ {{ scope.row.amount.toLocaleString() }}</span>
              </template>
            </el-table-column>
            <el-table-column prop="payer_name" label="對象" width="100" />
            <el-table-column prop="category_name" label="分類" width="120" />
            <el-table-column label="操作" width="150" align="center">
              <template #default="scope">
                <el-button type="primary" link size="small" @click="openDialog('fixed', scope.row)">編輯</el-button>
                <el-button type="danger" link size="small" @click="deleteItem('fixed_expenses', scope.row.id)">刪除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </div>
      </el-tab-pane>

      <!-- 🆕 資料管理（危險區域） -->
      <el-tab-pane label="資料管理" name="data">
        <div class="tab-content danger-zone">
          <h3 class="danger-title">⚠️ 危險區域 (Danger Zone)</h3>
          <p class="danger-desc">以下操作將會永久刪除資料庫中的紀錄，且無法復原。請謹慎操作。</p>

          <div class="danger-card">
            <div class="danger-info">
              <h4>清空所有記帳紀錄</h4>
              <p>刪除所有的收支流水、保險繳費狀態、基金快照與銀行對帳紀錄。保留成員與保單主檔。</p>
            </div>
            <el-button type="danger" plain @click="clearAllRecords" :loading="clearing">清空紀錄</el-button>
          </div>

          <div class="danger-card">
            <div class="danger-info">
              <h3>重置整個系統</h3>
              <p>刪除所有資料，包含成員、保單、基金、分類與所有流水。讓系統回到全新狀態。</p>
            </div>
            <el-button type="danger" @click="resetSystem" :loading="clearing">徹底重置</el-button>
          </div>

          <div class="info-card">
            <h4>資料狀態</h4>
            <ul class="data-stats">
              <li>家庭成員：{{ members.length }} 人</li>
              <li>支出分類：{{ categories.length }} 個</li>
              <li>固定支出模板：{{ fixedExpenses.length }} 個</li>
            </ul>
          </div>
        </div>
      </el-tab-pane>

    </el-tabs>

    <!-- 共用編輯彈窗 -->
    <el-dialog v-model="dialogVisible" :title="dialogTitle" width="90%" max-width="400px">
      <el-form :model="form" label-position="top">
        <el-form-item :label="formType === 'member' ? '成員名稱' : (formType === 'category' ? '分類名稱' : '項目名稱')" required>
          <el-input v-model="form.name" />
        </el-form-item>
        
        <el-form-item v-if="formType === 'member'" label="角色/關係">
          <el-input v-model="form.role" placeholder="例如：家庭成員" />
        </el-form-item>

        <template v-if="formType === 'fixed'">
          <el-form-item label="金額 (HKD)" required>
            <el-input-number v-model="form.amount" :min="1" style="width: 100%" />
          </el-form-item>
          <el-form-item label="對象" required>
            <el-select v-model="form.payer_name" style="width: 100%">
              <el-option v-for="m in members" :key="m.id" :label="m.name" :value="m.name" />
            </el-select>
          </el-form-item>
          <el-form-item label="分類" required>
            <el-select v-model="form.category_name" style="width: 100%">
              <el-option v-for="c in categories" :key="c.id" :label="c.name" :value="c.name" />
            </el-select>
          </el-form-item>
        </template>
      </el-form>
      <template #footer>
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" :loading="submitting" @click="submitForm">儲存</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage, ElMessageBox } from 'element-plus'

const activeTab = ref('members')
const members = ref([])
const categories = ref([])
const fixedExpenses = ref([])
const loading = ref(false)
const submitting = ref(false)
const clearing = ref(false)
const dialogVisible = ref(false)

const formType = ref('member')
const form = ref({ id: null, name: '', role: '', amount: 0, payer_name: '', category_name: '' })
const isEdit = ref(false)

const dialogTitle = computed(() => {
  const typeName = formType.value === 'member' ? '成員' : (formType.value === 'category' ? '分類' : '固定支出模板')
  return isEdit.value ? `編輯${typeName}` : `新增${typeName}`
})

const fetchData = async () => {
  loading.value = true
  const [mRes, cRes, fRes] = await Promise.all([
    supabase.from('members').select('*').order('created_at', { ascending: true }),
    supabase.from('expense_categories').select('*').order('created_at', { ascending: true }),
    supabase.from('fixed_expenses').select('*').order('created_at', { ascending: true })
  ])
  
  if (!mRes.error) members.value = mRes.data || []
  if (!cRes.error) categories.value = cRes.data || []
  if (!fRes.error) fixedExpenses.value = fRes.data || []
  
  loading.value = false
}

const openDialog = (type, row = null) => {
  formType.value = type
  if (row) {
    isEdit.value = true
    form.value = { 
      id: row.id, 
      name: row.name, 
      role: row.role || '', 
      amount: row.amount || 0,
      payer_name: row.payer_name || (members.value.length ? members.value[0].name : ''),
      category_name: row.category_name || (categories.value.length ? categories.value[0].name : '')
    }
  } else {
    isEdit.value = false
    form.value = { 
      id: null, 
      name: '', 
      role: '', 
      amount: 0,
      payer_name: members.value.length ? members.value[0].name : '',
      category_name: categories.value.length ? categories.value[0].name : ''
    }
  }
  dialogVisible.value = true
}

const submitForm = async () => {
  if (!form.value.name) return ElMessage.warning('請輸入名稱')
  submitting.value = true

  let table = ''
  let payload = { name: form.value.name }

  if (formType.value === 'member') {
    table = 'members'
    payload.role = form.value.role
  } else if (formType.value === 'category') {
    table = 'expense_categories'
  } else if (formType.value === 'fixed') {
    table = 'fixed_expenses'
    payload.amount = form.value.amount
    payload.payer_name = form.value.payer_name
    payload.category_name = form.value.category_name
  }

  if (isEdit.value) {
    const { error } = await supabase.from(table).update(payload).eq('id', form.value.id)
    if (error) ElMessage.error('更新失敗：' + error.message)
    else {
      ElMessage.success('✅ 已更新')
      dialogVisible.value = false
      fetchData()
    }
  } else {
    payload.family_id = '00000000-0000-0000-0000-000000000001'
    const { error } = await supabase.from(table).insert([payload])
    if (error) ElMessage.error('新增失敗：' + error.message)
    else {
      ElMessage.success('✅ 已新增')
      dialogVisible.value = false
      fetchData()
    }
  }
  submitting.value = false
}

const deleteItem = async (table, id) => {
  try {
    await ElMessageBox.confirm('刪除後無法復原，確定嗎？', '警告', { type: 'warning' })
    const { error } = await supabase.from(table).delete().eq('id', id)
    if (error) throw error
    ElMessage.success('已刪除')
    fetchData()
  } catch (err) {
    if (err !== 'cancel') ElMessage.error('刪除失敗')
  }
}

// 🆕 清空記帳紀錄
const clearAllRecords = async () => {
  try {
    await ElMessageBox.confirm('這將清空所有收支、保險狀態與基金快照，但會保留你的成員與主檔設定。確定要繼續嗎？', '高風險操作', { type: 'error', confirmButtonText: '確定清空' })
    clearing.value = true
    const familyId = '00000000-0000-0000-0000-000000000001'

    await Promise.all([
      supabase.from('expenses').delete().eq('family_id', familyId),
      supabase.from('incomes').delete().eq('family_id', familyId),
      supabase.from('insurance_payments').delete().neq('id', '00000000-0000-0000-0000-000000000000'),
      supabase.from('fund_snapshots').delete().neq('id', '00000000-0000-0000-0000-000000000000'),
      supabase.from('monthly_balances').delete().eq('family_id', familyId)
    ])

    ElMessage.success('✅ 記帳紀錄已全部清空')
  } catch (err) {
    if (err !== 'cancel') ElMessage.error('清空失敗')
  } finally {
    clearing.value = false
  }
}

// 🆕 徹底重置系統
const resetSystem = async () => {
  try {
    await ElMessageBox.confirm('【警告】這將刪除系統內「所有」的資料，包含成員、保單、基金與所有收支紀錄。系統將回到全新狀態。你確定嗎？', '徹底重置', { type: 'error', confirmButtonText: '我確定要重置' })
    clearing.value = true
    const familyId = '00000000-0000-0000-0000-000000000001'

    await Promise.all([
      supabase.from('expenses').delete().eq('family_id', familyId),
      supabase.from('incomes').delete().eq('family_id', familyId),
      supabase.from('insurance_payments').delete().neq('id', '00000000-0000-0000-0000-000000000000'),
      supabase.from('fund_snapshots').delete().neq('id', '00000000-0000-0000-0000-000000000000'),
      supabase.from('monthly_balances').delete().eq('family_id', familyId),
      supabase.from('fixed_expenses').delete().eq('family_id', familyId),
      supabase.from('funds').delete().eq('family_id', familyId),
      supabase.from('insurance_policies').delete().eq('family_id', familyId),
      supabase.from('expense_categories').delete().eq('family_id', familyId),
      supabase.from('members').delete().eq('family_id', familyId)
    ])

    ElMessage.success('✅ 系統已徹底重置')
    fetchData()
  } catch (err) {
    if (err !== 'cancel') ElMessage.error('重置失敗')
  } finally {
    clearing.value = false
  }
}

onMounted(() => {
  fetchData()
})
</script>

<style scoped>
.settings-container { width: 100%; }
.page-header { margin-bottom: 20px; }
.page-header h2 { margin: 0; color: #1e293b; }

.settings-tabs { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 12px rgba(0,0,0,0.05); }
.tab-content { padding-top: 10px; }
.action-bar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
.hint { font-size: 13px; color: #64748b; }
.text-red { color: #ef4444; }
.font-bold { font-weight: 600; }

/* 🆕 危險區域樣式 */
.danger-zone { max-width: 800px; }
.danger-title { color: #ef4444; margin-bottom: 5px; }
.danger-desc { color: #64748b; font-size: 14px; margin-bottom: 20px; }

.danger-card { border: 1px solid #fca5a5; background: #fef2f2; padding: 20px; border-radius: 8px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: center; gap: 20px; }
.danger-info h4 { margin: 0 0 5px 0; color: #b91c1c; font-size: 16px; }
.danger-info p { margin: 0; color: #991b1b; font-size: 13px; }

.info-card { border: 1px solid #e2e8f0; background: #f8fafc; padding: 20px; border-radius: 8px; }
.info-card h4 { margin: 0 0 10px 0; color: #1e293b; font-size: 16px; }
.data-stats { margin: 0; padding-left: 20px; color: #475569; font-size: 14px; line-height: 1.8; }

@media (max-width: 768px) {
  .action-bar { flex-direction: column; align-items: flex-start; gap: 10px; }
  .danger-card { flex-direction: column; align-items: flex-start; }
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/SettingsView.vue
V: 1793811400
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./src/views/SettingsView.vue === -->

<!-- === FILE START: ./src/views/SettlementView.vue === -->

## 📄 ./src/views/SettlementView.vue

```vue
<template>
  <div class="settlement-container">
    <div class="page-header">
      <h2>待辦中心</h2>
      <div class="header-actions">
        <el-button type="primary" @click="openAddExpenseDialog">+ 新增支出</el-button>
      </div>
    </div>

    <div v-if="loading" style="text-align: center; padding: 40px;">
      <el-icon class="is-loading" :size="30"><Loading /></el-icon>
    </div>

    <el-empty v-else-if="pendingGroups.length === 0 && doneItems.length === 0" description="目前沒有待辦項目，太棒了！" />

    <div v-else class="content-wrapper">
      <!-- 未辦事項區塊 -->
      <div v-if="pendingGroups.length > 0" class="pending-section">
        <div class="section-header">
          <h3>⏳ 未辦事項</h3>
          <div class="bulk-actions">
            <el-checkbox v-model="checkAll" @change="handleCheckAll" class="mr-2">全選</el-checkbox>
            <el-button type="success" :disabled="selectedItems.length === 0" @click="batchProcess" :loading="processing">
              批量標記已辦 ({{ selectedItems.length }})
            </el-button>
          </div>
        </div>

        <div class="groups-list">
          <el-card v-for="group in pendingGroups" :key="group.month" shadow="never" class="month-card">
            <template #header>
              <div class="month-header">
                <span class="month-title">{{ group.month }}</span>
                <span class="month-total text-red">待處理總額: HK$ {{ group.total.toLocaleString() }}</span>
              </div>
            </template>
            
            <div class="items-list">
              <div v-for="item in group.items" :key="item.id" class="todo-item">
                <div class="item-checkbox">
                  <el-checkbox-group v-model="selectedItems">
                    <el-checkbox :label="item.id">
                      <span style="display:none">選中</span>
                    </el-checkbox>
                  </el-checkbox-group>
                </div>
                <div class="item-info">
                  <div class="item-main">
                    <el-tag :type="item.type === 'insurance' ? 'warning' : 'info'" size="small" class="mr-2">
                      {{ item.type === 'insurance' ? '保險' : '支出' }}
                    </el-tag>
                    <span class="item-name">{{ item.name }}</span>
                    <span class="item-category" v-if="item.category">({{ item.category }})</span>
                  </div>
                  <div class="item-sub">
                    對象：{{ item.payer }} | 金額：<strong>HK$ {{ item.amount.toLocaleString() }}</strong>
                  </div>
                </div>
                <div class="item-actions">
                  <!-- 單獨處理按鈕 (可選) -->
                  <el-button type="success" plain size="small" @click="processSingle(item)">完成</el-button>
                </div>
              </div>
            </div>
          </el-card>
        </div>
      </div>

      <!-- 已辦事項區塊 -->
      <div v-if="doneItems.length > 0" class="done-section">
        <el-collapse v-model="activeDoneCollapse">
          <el-collapse-item name="done">
            <template #title>
              <h3 class="done-title">✅ 已辦事項 ({{ doneItems.length }})</h3>
            </template>
            <div class="table-wrapper">
              <el-table :data="doneItems" style="width: 100%; min-width: 500px;" stripe size="small">
                <el-table-column prop="year_month" label="月份" width="100" />
                <el-table-column prop="type" label="類型" width="80">
                  <template #default="scope">
                    <el-tag :type="scope.row.type === 'insurance' ? 'warning' : 'info'" size="small">
                      {{ scope.row.type === 'insurance' ? '保險' : '支出' }}
                    </el-tag>
                  </template>
                </el-table-column>
                <el-table-column prop="name" label="名稱" min-width="150" />
                <el-table-column prop="payer" label="對象" width="100" />
                <el-table-column prop="amount" label="金額" width="120" align="right">
                  <template #default="scope">
                    <span class="text-muted">HK$ {{ scope.row.amount.toLocaleString() }}</span>
                  </template>
                </el-table-column>
                <el-table-column label="操作" width="80" align="center">
                  <template #default="scope">
                    <el-button type="danger" link size="small" @click="cancelProcess(scope.row)">退回</el-button>
                  </template>
                </el-table-column>
              </el-table>
            </div>
          </el-collapse-item>
        </el-collapse>
      </div>
    </div>

    <!-- 新增支出彈窗 -->
    <el-dialog v-model="dialogVisible" title="新增支出" width="90%" max-width="500px">
      <el-form :model="form" label-position="top">
        <el-form-item label="紀錄月份" required>
          <el-date-picker v-model="form.year_month" type="month" placeholder="選擇月份" value-format="YYYY-MM" style="width: 100%" />
        </el-form-item>
        <el-form-item label="項目名稱" required>
          <el-input v-model="form.name" placeholder="例如：買餸、交租" />
        </el-form-item>
        <el-form-item label="金額 (HKD)" required>
          <el-input-number v-model="form.amount" :min="1" style="width: 100%" />
        </el-form-item>
        
        <el-form-item label="對象" required>
          <el-select v-model="form.member_name" style="width: 100%">
            <el-option v-for="m in members" :key="m.id" :label="m.name" :value="m.name" />
          </el-select>
        </el-form-item>
        
        <el-form-item label="類別" required>
          <el-select v-model="form.category_name" style="width: 100%" allow-create filterable default-first-option>
            <el-option v-for="c in categories" :key="c.id" :label="c.name" :value="c.name" />
          </el-select>
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" :loading="submitting" @click="submitForm">確認儲存</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage, ElMessageBox } from 'element-plus'
import { Loading } from '@element-plus/icons-vue'

const loading = ref(false)
const processing = ref(false)
const submitting = ref(false)

// 資料狀態
const rawExpenses = ref([])
const rawInsurancePayments = ref([])
const policies = ref([])
const members = ref([])
const categories = ref([])

// 勾選與折疊狀態
const selectedItems = ref([])
const checkAll = ref(false)
const activeDoneCollapse = ref([])

// 彈窗狀態
const dialogVisible = ref(false)
const form = ref({ year_month: '', name: '', amount: 1, member_name: '', category_name: '' })

// 轉換為統一的待辦格式
const allPendingItems = computed(() => {
  const items = []

  // 處理一般支出 (跨月，status = '未處理')
  rawExpenses.value.forEach(e => {
    if (e.status !== '已處理') {
      items.push({
        id: `exp_${e.id}`,
        realId: e.id,
        type: 'expense',
        year_month: e.year_month,
        name: e.name,
        payer: e.payer_name || '未指定',
        category: e.category_name,
        amount: Number(e.amount),
        created_at: e.created_at
      })
    }
  })

  // 處理保險 (跨月，status = '未轉')
  const policyMap = {}
  policies.value.forEach(p => { policyMap[p.id] = p })
  
  rawInsurancePayments.value.forEach(ip => {
    if (ip.status !== '已轉') {
      const p = policyMap[ip.policy_id]
      if (p) {
        items.push({
          id: `ins_${ip.id}`,
          realId: ip.id,
          type: 'insurance',
          year_month: ip.year_month,
          name: `保險供款：${p.name}`,
          payer: '保險公司', // 保險沒有對象，給個預設
          category: '保險',
          amount: Number(p.monthly_average),
          created_at: ip.created_at
        })
      }
    }
  })

  // 按年月由舊到新排序
  return items.sort((a, b) => a.year_month.localeCompare(b.year_month))
})

// 已辦事項 (只顯示最近一個月的，避免太長)
const doneItems = computed(() => {
  const items = []
  
  rawExpenses.value.forEach(e => {
    if (e.status === '已處理') {
      items.push({
        id: `exp_${e.id}`,
        realId: e.id,
        type: 'expense',
        year_month: e.year_month,
        name: e.name,
        payer: e.payer_name || '未指定',
        amount: Number(e.amount)
      })
    }
  })

  const policyMap = {}
  policies.value.forEach(p => { policyMap[p.id] = p })
  rawInsurancePayments.value.forEach(ip => {
    if (ip.status === '已轉') {
      const p = policyMap[ip.policy_id]
      if (p) {
        items.push({
          id: `ins_${ip.id}`,
          realId: ip.id,
          type: 'insurance',
          year_month: ip.year_month,
          name: `保險供款：${p.name}`,
          payer: '保險公司',
          amount: Number(p.monthly_average)
        })
      }
    }
  })
  // 由新到舊
  return items.sort((a, b) => b.year_month.localeCompare(a.year_month))
})

// 未辦事項分組
const pendingGroups = computed(() => {
  const groups = {}
  allPendingItems.value.forEach(item => {
    const m = item.year_month || '未知月份'
    if (!groups[m]) {
      groups[m] = { month: m, total: 0, items: [] }
    }
    groups[m].items.push(item)
    groups[m].total += item.amount
  })
  return Object.values(groups)
})

// 載入資料
const fetchData = async () => {
  loading.value = true
  
  try {
    const [expRes, ipRes, polRes, mRes, cRes] = await Promise.all([
      supabase.from('expenses').select('*'),
      supabase.from('insurance_payments').select('*'),
      supabase.from('insurance_policies').select('*'),
      supabase.from('members').select('*').order('created_at', { ascending: true }),
      supabase.from('expense_categories').select('*').order('created_at', { ascending: true })
    ])

    if (expRes.data) rawExpenses.value = expRes.data
    if (ipRes.data) rawInsurancePayments.value = ipRes.data
    if (polRes.data) policies.value = polRes.data
    if (mRes.data) members.value = mRes.data
    if (cRes.data) categories.value = cRes.data

  } catch (error) {
    ElMessage.error('載入待辦資料失敗')
  } finally {
    loading.value = false
  }
}

// 全選邏輯
const handleCheckAll = (val) => {
  if (val) {
    selectedItems.value = allPendingItems.value.map(i => i.id)
  } else {
    selectedItems.value = []
  }
}

// 批量處理
const batchProcess = async () => {
  if (selectedItems.value.length === 0) return
  
  try {
    await ElMessageBox.confirm(`確定要將選中的 ${selectedItems.value.length} 個項目標記為已辦嗎？`, '批量處理', { type: 'success' })
    processing.value = true

    const expenseIds = []
    const insuranceIds = []

    selectedItems.value.forEach(id => {
      if (id.startsWith('exp_')) expenseIds.push(id.replace('exp_', ''))
      if (id.startsWith('ins_')) insuranceIds.push(id.replace('ins_', ''))
    })

    const promises = []
    if (expenseIds.length > 0) {
      promises.push(supabase.from('expenses').update({ status: '已處理' }).in('id', expenseIds))
    }
    if (insuranceIds.length > 0) {
      promises.push(supabase.from('insurance_payments').update({ status: '已轉' }).in('id', insuranceIds))
    }

    await Promise.all(promises)

    ElMessage.success('✅ 已批量處理')
    selectedItems.value = []
    checkAll.value = false
    fetchData()

  } catch (err) {
    if (err !== 'cancel') ElMessage.error('處理失敗：' + err.message)
  } finally {
    processing.value = false
  }
}

// 單獨處理
const processSingle = async (item) => {
  try {
    processing.value = true
    if (item.type === 'expense') {
      await supabase.from('expenses').update({ status: '已處理' }).eq('id', item.realId)
    } else {
      await supabase.from('insurance_payments').update({ status: '已轉' }).eq('id', item.realId)
    }
    ElMessage.success('✅ 已完成')
    fetchData()
  } catch (err) {
    ElMessage.error('處理失敗')
  } finally {
    processing.value = false
  }
}

// 退回已辦
const cancelProcess = async (item) => {
  try {
    if (item.type === 'expense') {
      await supabase.from('expenses').update({ status: '未處理' }).eq('id', item.realId)
    } else {
      await supabase.from('insurance_payments').update({ status: '未轉' }).eq('id', item.realId)
    }
    ElMessage.success('已退回未辦')
    fetchData()
  } catch (err) {
    ElMessage.error('退回失敗')
  }
}

// 新增支出
const openAddExpenseDialog = () => {
  const defaultMember = members.value.length > 0 ? members.value[0].name : ''
  const defaultCategory = categories.value.length > 0 ? categories.value[0].name : ''
  form.value = { 
    year_month: new Date().toISOString().slice(0, 7), 
    name: '', 
    amount: 1, 
    member_name: defaultMember, 
    category_name: defaultCategory 
  }
  dialogVisible.value = true
}

const submitForm = async () => {
  if (!form.value.name || form.value.amount <= 0 || !form.value.year_month) return ElMessage.warning('請填寫完整資訊')
  if (!form.value.member_name) return ElMessage.warning('請選擇對象')
  
  submitting.value = true
  const familyId = '00000000-0000-0000-0000-000000000001'

  // 自動存檔新分類
  if (form.value.category_name) {
    const isNewCategory = !categories.value.some(c => c.name === form.value.category_name)
    if (isNewCategory) {
      await supabase.from('expense_categories').insert([{ family_id: familyId, name: form.value.category_name }])
      fetchData()
    }
  }
  
  const payload = {
    family_id: familyId,
    year_month: form.value.year_month,
    name: form.value.name,
    amount: form.value.amount,
    payer_name: form.value.member_name,
    category_name: form.value.category_name,
    status: '未處理'
  }

  const { error } = await supabase.from('expenses').insert([payload])
  
  submitting.value = false
  if (error) {
    ElMessage.error('儲存失敗：' + error.message)
  } else {
    ElMessage.success('✅ 支出已新增')
    dialogVisible.value = false
    fetchData()
  }
}

onMounted(() => fetchData())
</script>

<style scoped>
.settlement-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; }
.page-header h2 { margin: 0; color: #1e293b; font-size: 24px; font-weight: 700; }

.content-wrapper { display: flex; flex-direction: column; gap: 24px; }

/* 未辦事項區塊 */
.pending-section { width: 100%; }
.section-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; }
.section-header h3 { margin: 0; font-size: 18px; color: #334155; }
.bulk-actions { display: flex; align-items: center; }

.groups-list { display: flex; flex-direction: column; gap: 16px; }

.month-card { border-radius: 16px; border: 1px solid rgba(0,0,0,0.03); }
.month-card :deep(.el-card__header) { background-color: #F8FAFC; padding: 12px 20px; border-bottom: 1px solid #f1f5f9; }
.month-header { display: flex; justify-content: space-between; align-items: center; }
.month-title { font-weight: 700; color: #1e293b; font-size: 16px; }
.month-total { font-size: 14px; font-family: monospace; font-weight: 600; }

.items-list { display: flex; flex-direction: column; gap: 8px; }
.todo-item { display: flex; align-items: center; gap: 12px; padding: 12px; border-radius: 10px; background: #ffffff; border: 1px solid #f1f5f9; transition: background 0.2s; }
.todo-item:hover { background: #F8FAFC; }

.item-checkbox { flex-shrink: 0; }
.item-info { flex: 1; display: flex; flex-direction: column; gap: 4px; }
.item-main { display: flex; align-items: center; gap: 8px; font-weight: 600; color: #1e293b; font-size: 15px; }
.item-name { font-weight: 700; }
.item-category { font-size: 13px; color: #64748b; font-weight: normal; }
.item-sub { font-size: 13px; color: #64748b; }
.item-actions { flex-shrink: 0; }

.mr-2 { margin-right: 12px; }
.text-red { color: #ef4444; }

/* 已辦事項區塊 */
.done-section { margin-top: 20px; background: #ffffff; border-radius: 16px; border: 1px solid rgba(0,0,0,0.03); padding: 0 20px; }
.done-title { font-size: 16px; color: #334155; margin: 0; padding: 4px 0; }
.table-wrapper { width: 100%; overflow-x: auto; }
.text-muted { color: #94a3b8; text-decoration: line-through; }

@media (max-width: 768px) {
  .page-header { flex-direction: column; align-items: flex-start; gap: 12px; }
  .header-actions { width: 100%; }
  .header-actions .el-button { width: 100%; }
  .section-header { flex-direction: column; align-items: flex-start; gap: 12px; }
  .bulk-actions { width: 100%; justify-content: space-between; }
  .todo-item { flex-wrap: wrap; }
  .item-actions { width: 100%; text-align: right; margin-top: 8px; }
}
</style>

<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/SettlementView.vue
V: 1793971400
W: ai-wa-fs-002
═══ -->```

<!-- === FILE END: ./src/views/SettlementView.vue === -->

<!-- === FILE START: ./vite.config.js === -->

## 📄 ./vite.config.js

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { VitePWA } from 'vite-plugin-pwa'
import path from 'path'

export default defineConfig({
  base: '/',
  plugins: [
    vue(),
    VitePWA({
      registerType: 'autoUpdate',
      includeAssets: ['favicon.ico', 'apple-touch-icon.png', 'masked-icon.svg'],
      manifest: {
        name: '家庭入帳 V3',
        short_name: '入帳 V3',
        description: '家庭財務與保險基金管理系統',
        theme_color: '#1e293b',
        background_color: '#f1f5f9',
        display: 'standalone',
        icons: [
          {
            src: 'pwa-192x192.png',
            sizes: '192x192',
            type: 'image/png'
          },
          {
            src: 'pwa-512x512.png',
            sizes: '512x512',
            type: 'image/png'
          }
        ]
      }
    })
  ],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
  server: {
    port: 3000,
    open: true
  }
})

/* ═══ EOF ═══
F: AI-WA-FS/vite.config.js
V: 1793810200
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./vite.config.js === -->
