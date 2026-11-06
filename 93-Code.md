> 體系版本：SV14
> 生成時間：2026-11-06 20:07:16 (HKT)

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
    "V": 1793078500,
    "W": "ai-wa-fs-001"
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
        <h2 v-if="!isCollapse || isMobile">家庭入帳 V3</h2>
      </div>
      <el-menu
        :default-active="$route.path"
        router
        background-color="#1e293b"
        text-color="#94a3b8"
        active-text-color="#38bdf8"
        :collapse="isCollapse && !isMobile"
        @select="onMenuSelect"
      >
        <el-menu-item index="/">
          <el-icon><HomeFilled /></el-icon>
          <template #title>工作台</template>
        </el-menu-item>
        <el-menu-item index="/cashflow">
          <el-icon><Money /></el-icon>
          <template #title>收支流水</template>
        </el-menu-item>
        <el-menu-item index="/settlement">
          <el-icon><Tickets /></el-icon>
          <template #title>待轉帳表</template>
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
        <el-button type="danger" plain size="small" @click="handleLogout">登出</el-button>
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
// 🆕 引入 Tickets 圖示
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
body { margin: 0; padding: 0; font-family: 'Noto Sans TC', sans-serif; background-color: #f1f5f9; }

/* 佈局基礎 */
.app-layout { display: flex; height: 100vh; overflow: hidden; width: 100vw; }
.main-wrapper { flex: 1; width: 0; overflow: hidden; display: flex; flex-direction: column; }

/* 側邊欄基礎樣式 */
.sidebar { 
  background-color: #1e293b; 
  color: white; 
  z-index: 1000;
  width: 220px;
}
.sidebar.is-collapsed {
  width: 64px;
}
.logo { height: 60px; display: flex; align-items: center; justify-content: center; border-bottom: 1px solid #334155; overflow: hidden; }
.logo h2 { margin: 0; font-size: 18px; color: #f8fafc; white-space: nowrap; }
.sidebar .el-menu { border-right: none; }

/* 頂部導覽列 */
.header { background-color: white; display: flex; align-items: center; border-bottom: 1px solid #e2e8f0; padding: 0 15px; height: 60px; flex-shrink: 0; }
.header-title { font-weight: bold; color: #334155; margin-left: 10px; flex: 1; }
.menu-toggle { margin-right: 10px; color: #64748b; padding: 8px; }

/* 內容區 */
.main-content { 
  background-color: #f1f5f9; 
  padding: 15px; 
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
  }
  .sidebar.is-mobile-hidden {
    transform: translateX(-100%);
    width: 220px;
  }
  .sidebar-mask {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background: rgba(0,0,0,0.5);
    z-index: 999;
  }
  .main-content {
    padding: 10px;
  }
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/App.vue
V: 1793810800
W: ai-wa-fs-001
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
  --primary-color: #3b82f6;
  --bg-color: #f1f5f9;
  --text-main: #1e293b;
  --text-secondary: #64748b;
  --card-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
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

/* 覆寫 Element Plus 卡片樣式，統一圓角與陰影 */
.el-card {
  border-radius: 12px !important;
  border: none !important;
  box-shadow: var(--card-shadow) !important;
  background-color: #ffffff;
}

/* 優化 Element Plus 表格樣式 */
.el-table {
  --el-table-border-color: #f1f5f9;
  --el-table-header-bg-color: #f8fafc;
  border-radius: 8px;
  overflow: hidden;
}
.el-table th.el-table__cell {
  font-weight: 600;
  color: #475569;
}

/* 優化對話框與彈窗 */
.el-dialog {
  border-radius: 16px !important;
  overflow: hidden;
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

/* 隱藏手機端滾動條，保持畫面乾淨 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 4px;
}
::-webkit-scrollbar-track {
  background: transparent;
}

/* 全局按鈕圓角 */
.el-button {
  border-radius: 8px;
  font-weight: 500;
}

/* 確保手機端版面不會破版 */
#app {
  width: 100%;
  max-width: 100vw;
  overflow-x: hidden;
}

/* ═══ EOF ═══
F: AI-WA-FS/src/style.css
V: 1793804110
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./src/style.css === -->

<!-- === FILE START: ./src/views/AnnualReportView.vue === -->

## 📄 ./src/views/AnnualReportView.vue

```vue
<template>
  <div class="annual-container">
    <div class="page-header">
      <h2>年度報表</h2>
      <div class="header-actions">
        <el-date-picker 
          v-model="selectedYear" 
          type="year" 
          placeholder="選擇年份" 
          value-format="YYYY" 
          @change="fetchData" 
          class="year-picker"
        />
        <el-button type="success" plain icon="Download" @click="exportCSV" :disabled="monthlyData.length === 0">
          匯出 CSV
        </el-button>
      </div>
    </div>

    <!-- 年度總計卡片 -->
    <el-row :gutter="10" class="mb-20" v-loading="loading">
      <el-col :xs="12" :sm="6">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">年度總收入</div>
          <div class="stat-value text-green">HK$ {{ yearTotalIncome.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="12" :sm="6">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">年度總支出</div>
          <div class="stat-value text-red">HK$ {{ yearTotalExpense.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="12" :sm="6">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">保險總支出</div>
          <div class="stat-value text-orange">HK$ {{ yearTotalInsurance.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="12" :sm="6">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">年度淨結餘</div>
          <div class="stat-value" :class="yearNetBalance >= 0 ? 'text-green' : 'text-red'">
            {{ yearNetBalance >= 0 ? '+' : '' }} HK$ {{ yearNetBalance.toLocaleString() }}
          </div>
        </el-card>
      </el-col>
    </el-row>

    <!-- 月度明細表格 -->
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

// 取得當前年份
const currentYear = new Date().getFullYear().toString()
const selectedYear = ref(currentYear)
const loading = ref(false)

// 儲存12個月的資料
const monthlyData = ref([])

// 年度總計
const yearTotalIncome = computed(() => monthlyData.value.reduce((sum, item) => sum + item.income, 0))
const yearTotalExpense = computed(() => monthlyData.value.reduce((sum, item) => sum + item.expense, 0))
const yearTotalInsurance = computed(() => monthlyData.value.reduce((sum, item) => sum + item.insurance, 0))
const yearNetBalance = computed(() => yearTotalIncome.value - yearTotalExpense.value - yearTotalInsurance.value)

const fetchData = async () => {
  loading.value = true
  
  try {
    // 平行抓取該年份所有的收入、支出與保險轉帳紀錄
    const [incRes, expRes, insRes] = await Promise.all([
      supabase.from('incomes').select('year_month, amount').like('year_month', `${selectedYear.value}-%`),
      supabase.from('expenses').select('year_month, amount').like('year_month', `${selectedYear.value}-%`),
      supabase.from('insurance_payments')
        .select('year_month, policy_id, status')
        .like('year_month', `${selectedYear.value}-%`)
        .eq('status', '已轉')
    ])

    if (incRes.error) throw incRes.error
    if (expRes.error) throw expRes.error
    if (insRes.error) throw insRes.error

    // 抓取保單月均金額，用來計算保險支出
    const polRes = await supabase.from('insurance_policies').select('id, monthly_average')
    const policyMap = {}
    if (polRes.data) {
      polRes.data.forEach(p => { policyMap[p.id] = Number(p.monthly_average) })
    }

    // 初始化 1~12 月的空資料
    const tempData = Array.from({ length: 12 }, (_, i) => ({
      month: i + 1,
      income: 0,
      expense: 0,
      insurance: 0,
      balance: 0
    }))

    // 填入收入
    if (incRes.data) {
      incRes.data.forEach(item => {
        const m = parseInt(item.year_month.split('-')[1]) - 1
        if (tempData[m]) tempData[m].income += Number(item.amount)
      })
    }

    // 填入一般支出
    if (expRes.data) {
      expRes.data.forEach(item => {
        const m = parseInt(item.year_month.split('-')[1]) - 1
        if (tempData[m]) tempData[m].expense += Number(item.amount)
      })
    }

    // 填入保險支出
    if (insRes.data) {
      insRes.data.forEach(item => {
        const m = parseInt(item.year_month.split('-')[1]) - 1
        const amount = policyMap[item.policy_id] || 0
        if (tempData[m]) tempData[m].insurance += amount
      })
    }

    // 計算每月結餘 (總收入 - 一般支出 - 保險支出)
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

// 🆕 表格底部加總列
const getSummaries = (param) => {
  const { columns } = param
  const sums = []
  columns.forEach((column, index) => {
    if (index === 0) {
      sums[index] = '年度總計'
      return
    }
    if (column.property === 'income') {
      sums[index] = `HK$ ${yearTotalIncome.value.toLocaleString()}`
    } else if (column.property === 'expense') {
      sums[index] = `HK$ ${yearTotalExpense.value.toLocaleString()}`
    } else if (column.property === 'insurance') {
      sums[index] = `HK$ ${yearTotalInsurance.value.toLocaleString()}`
    } else if (column.property === 'balance') {
      sums[index] = `HK$ ${yearNetBalance.value.toLocaleString()}`
    } else {
      sums[index] = ''
    }
  })
  return sums
}

// 🆕 匯出 CSV
const exportCSV = () => {
  if (monthlyData.value.length === 0) return

  const headers = ['月份', '總收入(HKD)', '一般支出(HKD)', '保險供款(HKD)', '當月結餘(HKD)']
  const rows = monthlyData.value.map(d => [
    `${d.month}月`,
    d.income,
    d.expense,
    d.insurance,
    d.balance
  ])

  // 加上總計列
  rows.push([
    '年度總計',
    yearTotalIncome.value,
    yearTotalExpense.value,
    yearTotalInsurance.value,
    yearNetBalance.value
  ])

  const csvContent = [
    headers.join(','),
    ...rows.map(e => e.join(','))
  ].join('\n')

  // 加上 BOM 讓 Excel 正確顯示中文
  const blob = new Blob(['\uFEFF' + csvContent], { type: 'text/csv;charset=utf-8;' })
  const url = URL.createObjectURL(blob)
  const link = document.createElement('a')
  link.href = url
  link.setAttribute('download', `年度報表_${selectedYear.value}.csv`)
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
  URL.revokeObjectURL(url)
  ElMessage.success('✅ CSV 已下載')
}

onMounted(() => fetchData())
</script>

<style scoped>
.annual-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; flex-wrap: wrap; gap: 10px; }
.page-header h2 { margin: 0; color: #1e293b; }
.header-actions { display: flex; gap: 10px; align-items: center; }
.year-picker { width: 120px; }

.mb-20 { margin-bottom: 20px; }
.stat-card { border-radius: 8px; background: #f8fafc; }
.stat-label { font-size: 13px; color: #64748b; margin-bottom: 5px; }
.stat-value { font-size: 18px; font-weight: bold; }
.text-green { color: #10b981; }
.text-red { color: #ef4444; }
.text-orange { color: #f59e0b; }
.font-bold { font-weight: 600; }

.table-card { border-radius: 8px; border: none; overflow: hidden; }
.table-wrapper { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }
:deep(.el-table__footer-wrapper tbody td) {
  font-weight: bold;
  background-color: #f8fafc;
  color: #1e293b;
}

@media (max-width: 768px) {
  .stat-value { font-size: 16px; }
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/AnnualReportView.vue
V: 1793810500
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./src/views/AnnualReportView.vue === -->

<!-- === FILE START: ./src/views/CashflowView.vue === -->

## 📄 ./src/views/CashflowView.vue

```vue
<template>
  <div class="cashflow-container">
    <div class="page-header">
      <div class="header-left">
        <h2>收支流水</h2>
        <el-date-picker 
          v-model="selectedMonth" 
          type="month" 
          placeholder="選擇月份" 
          value-format="YYYY-MM" 
          @change="fetchData" 
          class="month-picker"
        />
      </div>
      
      <div class="header-actions">
        <el-radio-group v-model="viewMode" class="view-toggle">
          <el-radio-button label="table">表格</el-radio-button>
          <el-radio-button label="kanban">看板</el-radio-button>
        </el-radio-group>
        <el-button type="primary" @click="openAddDialog">+ 新增紀錄</el-button>
      </div>
    </div>

    <!-- 統計卡片 -->
    <el-row :gutter="10" class="mb-20">
      <el-col :xs="12" :sm="12">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">本月總支出</div>
          <div class="stat-value text-red">HK$ {{ totalExpense.toLocaleString() }}</div>
        </el-card>
      </el-col>
      <el-col :xs="12" :sm="12">
        <el-card shadow="never" class="stat-card">
          <div class="stat-label">本月總收入</div>
          <div class="stat-value text-green">HK$ {{ totalIncome.toLocaleString() }}</div>
        </el-card>
      </el-col>
    </el-row>

    <!-- 資料顯示區 -->
    <el-card shadow="never" class="list-card">
      <el-tabs v-model="activeTab">
        <el-tab-pane label="支出紀錄" name="expense">
          
          <div class="tool-bar">
            <el-button type="warning" plain icon="MagicStick" :loading="importing" @click="importFixedExpenses">
              一鍵匯入本月固定支出
            </el-button>
          </div>

          <div v-if="viewMode === 'table'" class="table-wrapper">
            <el-table :data="expenses" style="width: 100%; min-width: 600px;" stripe v-loading="loading">
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
                    {{ scope.row.status === '已處理' ? '取消處理' : '標記處理' }}
                  </el-button>
                  <el-button type="primary" link size="small" @click="openEditDialog(scope.row)">編輯</el-button>
                  <el-button type="danger" text size="small" @click="deleteRecord('expenses', scope.row.id)">刪除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </div>

          <div v-else class="kanban-wrapper" v-loading="loading">
            <div class="kanban-column">
              <div class="kanban-header pending">未處理 <el-tag size="small" type="info">{{ pendingExpenses.length }}</el-tag></div>
              <div class="kanban-list">
                <el-card v-for="item in pendingExpenses" :key="item.id" class="kanban-card" shadow="hover">
                  <div class="card-title-row">
                    <span class="title">{{ item.name }}</span>
                    <span class="amount text-red">HK$ {{ item.amount.toLocaleString() }}</span>
                  </div>
                  <div class="card-meta">
                    <el-tag size="small" type="info">{{ item.category_name || '未分類' }}</el-tag>
                    <span class="payer">{{ item.payer_name }}</span>
                  </div>
                  <div class="card-actions">
                    <el-button type="primary" size="small" plain @click="toggleExpenseStatus(item)">標記處理</el-button>
                    <el-button type="primary" size="small" text @click="openEditDialog(item)">編輯</el-button>
                    <el-button type="danger" size="small" text @click="deleteRecord('expenses', item.id)">刪除</el-button>
                  </div>
                </el-card>
                <el-empty v-if="pendingExpenses.length === 0" description="沒有未處理項目" :image-size="60" />
              </div>
            </div>

            <div class="kanban-column">
              <div class="kanban-header done">已處理 <el-tag size="small" type="success">{{ doneExpenses.length }}</el-tag></div>
              <div class="kanban-list">
                <el-card v-for="item in doneExpenses" :key="item.id" class="kanban-card done-card" shadow="hover">
                  <div class="card-title-row">
                    <span class="title text-muted">{{ item.name }}</span>
                    <span class="amount text-muted">HK$ {{ item.amount.toLocaleString() }}</span>
                  </div>
                  <div class="card-meta">
                    <el-tag size="small" type="info">{{ item.category_name || '未分類' }}</el-tag>
                    <span class="payer">{{ item.payer_name }}</span>
                  </div>
                  <div class="card-actions">
                    <el-button type="info" size="small" plain @click="toggleExpenseStatus(item)">取消處理</el-button>
                    <el-button type="primary" size="small" text @click="openEditDialog(item)">編輯</el-button>
                    <el-button type="danger" size="small" text @click="deleteRecord('expenses', item.id)">刪除</el-button>
                  </div>
                </el-card>
                <el-empty v-if="doneExpenses.length === 0" description="沒有已處理項目" :image-size="60" />
              </div>
            </div>
          </div>
        </el-tab-pane>
        
        <el-tab-pane label="收入紀錄" name="income">
          <div class="table-wrapper">
            <el-table :data="incomes" style="width: 100%; min-width: 400px;" stripe v-loading="loading">
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
        </el-tab-pane>
      </el-tabs>
    </el-card>

    <!-- 新增/編輯彈窗 -->
    <el-dialog v-model="dialogVisible" :title="dialogTitle" width="90%" max-width="500px">
      <el-form :model="form" label-position="top">
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
          <!-- 保留 allow-create 允許快速新增 -->
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
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage, ElMessageBox } from 'element-plus'
import { MagicStick } from '@element-plus/icons-vue'

const d = new Date()
const selectedMonth = ref(`${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}`)

const activeTab = ref('expense')
const viewMode = ref('table')
const expenses = ref([])
const incomes = ref([])
const members = ref([])
const categories = ref([])
const loading = ref(false)
const submitting = ref(false)
const importing = ref(false)
const dialogVisible = ref(false)
const isEdit = ref(false)

const dialogTitle = computed(() => {
  return isEdit.value ? '編輯紀錄' : `新增${activeTab.value === 'expense' ? '支出' : '收入'}`
})

const totalExpense = computed(() => expenses.value.reduce((sum, e) => sum + Number(e.amount), 0))
const totalIncome = computed(() => incomes.value.reduce((sum, i) => sum + Number(i.amount), 0))

const pendingExpenses = computed(() => expenses.value.filter(e => e.status !== '已處理'))
const doneExpenses = computed(() => expenses.value.filter(e => e.status === '已處理'))

const form = ref({ id: null, name: '', amount: 1, member_name: '', category_name: '', status: '未處理' })

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
  const familyId = '00000000-0000-0000-0000-000000000001'
  
  const [expRes, incRes] = await Promise.all([
    supabase.from('expenses').select('*').eq('year_month', selectedMonth.value).order('created_at', { ascending: false }),
    supabase.from('incomes').select('*').eq('year_month', selectedMonth.value).order('created_at', { ascending: false })
  ])

  if (!expRes.error) expenses.value = expRes.data || []
  if (!incRes.error) incomes.value = incRes.data || []
  
  loading.value = false
}

const importFixedExpenses = async () => {
  try {
    importing.value = true
    const familyId = '00000000-0000-0000-0000-000000000001'

    const [fixedRes, currentRes] = await Promise.all([
      supabase.from('fixed_expenses').select('*'),
      supabase.from('expenses').select('name').eq('year_month', selectedMonth.value)
    ])

    if (fixedRes.error) throw fixedRes.error
    if (!fixedRes.data || fixedRes.data.length === 0) return ElMessage.warning('尚未設定任何固定支出模板')

    const existingNames = currentRes.data.map(e => e.name)
    const toInsert = []

    fixedRes.data.forEach(tpl => {
      if (!existingNames.includes(tpl.name)) {
        toInsert.push({
          family_id: familyId,
          year_month: selectedMonth.value,
          name: tpl.name,
          amount: tpl.amount,
          payer_name: tpl.payer_name,
          category_name: tpl.category_name,
          status: '未處理'
        })
      }
    })

    if (toInsert.length === 0) return ElMessage.info('本月所有固定支出皆已匯入')

    const { error } = await supabase.from('expenses').insert(toInsert)
    if (error) throw error

    ElMessage.success(`✅ 成功匯入 ${toInsert.length} 筆固定支出`)
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
  form.value = { id: null, name: '', amount: 1, member_name: defaultMember, category_name: defaultCategory, status: '未處理' }
  dialogVisible.value = true
}

const openEditDialog = (row) => {
  isEdit.value = true
  form.value = {
    id: row.id,
    name: row.name,
    amount: row.amount,
    member_name: activeTab.value === 'expense' ? row.payer_name : row.member_name,
    category_name: row.category_name || '',
    status: row.status || '未處理'
  }
  dialogVisible.value = true
}

const submitForm = async () => {
  if (!form.value.name || form.value.amount <= 0) return ElMessage.warning('請填寫完整資訊')
  if (!form.value.member_name) return ElMessage.warning('請選擇對象')
  
  submitting.value = true
  const familyId = '00000000-0000-0000-0000-000000000001'

  // 🆕 檢查是否為新分類，若是則自動存檔
  if (activeTab.value === 'expense' && form.value.category_name) {
    const isNewCategory = !categories.value.some(c => c.name === form.value.category_name)
    if (isNewCategory) {
      await supabase.from('expense_categories').insert([{
        family_id: familyId,
        name: form.value.category_name
      }])
      fetchDependencies() // 重新抓取分類
    }
  }
  
  if (activeTab.value === 'expense') {
    const payload = {
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
        family_id: familyId,
        year_month: selectedMonth.value
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
        family_id: familyId,
        year_month: selectedMonth.value
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
/* 樣式同上，保持不變 */
.cashflow-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; flex-wrap: wrap; gap: 10px; }
.header-left { display: flex; align-items: center; flex-wrap: wrap; gap: 10px; }
.header-left h2 { margin: 0; color: #1e293b; }
.month-picker { width: 140px; }
.header-actions { display: flex; align-items: center; gap: 15px; flex-wrap: wrap; }
.view-toggle { margin-right: 10px; }

.mb-20 { margin-bottom: 20px; }
.stat-card { border-radius: 8px; background: #f8fafc; }
.stat-label { font-size: 13px; color: #64748b; margin-bottom: 5px; }
.stat-value { font-size: 20px; font-weight: bold; }
.text-red { color: #ef4444; }
.text-green { color: #10b981; }
.text-muted { color: #94a3b8; text-decoration: line-through; }
.font-bold { font-weight: 600; }
.list-card { border-radius: 8px; border: none; overflow: hidden; }

.tool-bar { margin-bottom: 15px; display: flex; justify-content: flex-end; }

.table-wrapper { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }

.kanban-wrapper {
  display: flex;
  gap: 16px;
  overflow-x: auto;
  padding-bottom: 10px;
  min-height: 400px;
}
.kanban-column {
  flex: 1;
  min-width: 280px;
  background-color: #f8fafc;
  border-radius: 8px;
  padding: 12px;
  display: flex;
  flex-direction: column;
  border: 1px solid #e2e8f0;
}
.kanban-header {
  font-weight: bold;
  font-size: 14px;
  margin-bottom: 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 10px;
  border-bottom: 2px solid #e2e8f0;
}
.kanban-header.pending { color: #ef4444; border-bottom-color: #fca5a5; }
.kanban-header.done { color: #10b981; border-bottom-color: #6ee7b7; }

.kanban-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
  flex: 1;
}
.kanban-card {
  border-radius: 6px;
  border: none;
  box-shadow: 0 1px 4px rgba(0,0,0,0.05);
}
.kanban-card.done-card { opacity: 0.8; }

.card-title-row { display: flex; justify-content: space-between; font-weight: bold; margin-bottom: 8px; font-size: 15px; }
.card-meta { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; font-size: 13px; color: #64748b; }
.card-actions { display: flex; justify-content: flex-end; gap: 8px; border-top: 1px dashed #e2e8f0; padding-top: 10px; }

@media (max-width: 768px) {
  .kanban-wrapper { flex-direction: column; overflow-x: hidden; }
  .kanban-column { min-width: 100%; }
  .view-toggle { width: 100%; display: flex; justify-content: center; }
  .header-actions { width: 100%; justify-content: space-between; }
  .tool-bar { justify-content: center; }
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/CashflowView.vue
V: 1793811200
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./src/views/CashflowView.vue === -->

<!-- === FILE START: ./src/views/DashboardView.vue === -->

## 📄 ./src/views/DashboardView.vue

```vue
<template>
  <div class="dashboard-container">
    <div class="page-header">
      <h2>工作台</h2>
      <span class="date-text">{{ currentMonthText }}</span>
    </div>

    <div v-loading="loading">
      <!-- 頂部核心資產看板 -->
      <el-row :gutter="15" class="mb-20">
        <el-col :xs="24" :sm="12">
          <el-card shadow="never" class="total-asset-card">
            <div class="asset-label">家庭當前總資產 (銀行結餘 + 基金)</div>
            <div class="asset-value">HK$ {{ totalAssets.toLocaleString() }}</div>
          </el-card>
        </el-col>
        <el-col :xs="24" :sm="12">
          <el-card shadow="never" class="asset-change-card">
            <div class="asset-label">本月淨資產變化</div>
            <div class="asset-value" :class="(totalIncome - totalExpense) >= 0 ? 'text-green' : 'text-red'">
              {{ (totalIncome - totalExpense) >= 0 ? '+' : '' }} HK$ {{ (totalIncome - totalExpense).toLocaleString() }}
            </div>
          </el-card>
        </el-col>
      </el-row>

      <!-- 收支與保費統計 -->
      <el-row :gutter="15" class="mb-20">
        <el-col :xs="12" :sm="6">
          <el-card shadow="never" class="stat-card income-card">
            <div class="stat-icon"><el-icon><TrendCharts /></el-icon></div>
            <div class="stat-info">
              <div class="stat-label">本月總收入</div>
              <div class="stat-value text-green">HK$ {{ totalIncome.toLocaleString() }}</div>
            </div>
          </el-card>
        </el-col>
        
        <el-col :xs="12" :sm="6">
          <el-card shadow="never" class="stat-card expense-card">
            <div class="stat-icon"><el-icon><Bottom /></el-icon></div>
            <div class="stat-info">
              <div class="stat-label">本月總支出</div>
              <div class="stat-value text-red">HK$ {{ totalExpense.toLocaleString() }}</div>
            </div>
          </el-card>
        </el-col>

        <el-col :xs="12" :sm="6">
          <el-card shadow="never" class="stat-card pending-card">
            <div class="stat-icon"><el-icon><Warning /></el-icon></div>
            <div class="stat-info">
              <div class="stat-label">待繳保費</div>
              <div class="stat-value text-orange">HK$ {{ pendingInsurance.toLocaleString() }}</div>
            </div>
          </el-card>
        </el-col>

        <el-col :xs="12" :sm="6">
          <el-card shadow="never" class="stat-card fund-card">
            <div class="stat-icon"><el-icon><Wallet /></el-icon></div>
            <div class="stat-info">
              <div class="stat-label">基金總現值</div>
              <div class="stat-value text-blue">HK$ {{ totalFundValue.toLocaleString() }}</div>
            </div>
          </el-card>
        </el-col>
      </el-row>

      <el-row :gutter="15">
        <!-- 銀行結餘對帳工具 -->
        <el-col :xs="24" :md="12" class="mb-20">
          <el-card shadow="never" class="panel-card">
            <template #header>
              <div class="panel-header">
                <span>本月結餘對帳</span>
                <el-tag v-if="isReconciled" type="success" size="small">✅ 已對帳</el-tag>
                <el-tag v-else type="danger" size="small">⚠️ 未對帳</el-tag>
              </div>
            </template>
            
            <div class="reconcile-box">
              <div class="calc-row">
                <span>上月實際結餘</span>
                <span class="mono">HK$ {{ prevBalance.toLocaleString() }}</span>
              </div>
              <div class="calc-row">
                <span>本月收支淨額</span>
                <span class="mono" :class="(totalIncome - totalExpense) >= 0 ? 'text-green' : 'text-red'">
                  {{ (totalIncome - totalExpense) >= 0 ? '+' : '' }} HK$ {{ (totalIncome - totalExpense).toLocaleString() }}
                </span>
              </div>
              <div class="calc-row highlight">
                <span>系統推算本月結餘</span>
                <span class="mono">HK$ {{ calculatedBalance.toLocaleString() }}</span>
              </div>

              <el-divider style="margin: 12px 0;" />

              <div class="input-row">
                <span class="label">月結單實際結餘：</span>
                <el-input-number v-model="inputActualBalance" :controls="false" style="width: 140px" placeholder="輸入金額" />
                <el-button type="primary" @click="saveActualBalance" :loading="savingBalance">儲存</el-button>
              </div>

              <div class="diff-row" v-if="inputActualBalance > 0">
                <span>差異金額：</span>
                <span :class="Math.abs(inputActualBalance - calculatedBalance) < 1 ? 'text-green font-bold' : 'text-red font-bold'">
                  HK$ {{ Math.abs(inputActualBalance - calculatedBalance).toLocaleString() }}
                </span>
                <span v-if="Math.abs(inputActualBalance - calculatedBalance) >= 1" class="text-muted" style="font-size:12px; margin-left:8px;">(可能有漏記)</span>
              </div>
            </div>
          </el-card>
        </el-col>

        <!-- 待處理事項 -->
        <el-col :xs="24" :md="12" class="mb-20">
          <el-card shadow="never" class="panel-card">
            <template #header>
              <div class="panel-header">
                <span>待處理事項</span>
              </div>
            </template>
            <div class="todo-list">
              <div class="todo-item" v-if="pendingInsuranceCount > 0">
                <el-tag type="warning" size="small">保險</el-tag>
                <span>有 {{ pendingInsuranceCount }} 張保單本月尚未轉帳</span>
                <el-button type="primary" link @click="$router.push('/insurance')">去處理</el-button>
              </div>
              <div class="todo-item" v-if="pendingExpenseCount > 0">
                <el-tag type="danger" size="small">收支</el-tag>
                <span>有 {{ pendingExpenseCount }} 筆支出尚未結算</span>
                <el-button type="primary" link @click="$router.push('/settlement')">去處理</el-button>
              </div>
              <el-empty v-if="pendingInsuranceCount === 0 && pendingExpenseCount === 0" description="太棒了！目前沒有待處理事項" :image-size="60" />
            </div>
          </el-card>
        </el-col>
      </el-row>

      <!-- 🆕 支出類別分析 -->
      <el-row v-if="categoryStats.length > 0">
        <el-col :span="24" class="mb-20">
          <el-card shadow="never" class="panel-card">
            <template #header>
              <div class="panel-header">
                <span>本月支出類別分析</span>
              </div>
            </template>
            <div class="category-list">
              <div class="category-item" v-for="cat in categoryStats" :key="cat.name">
                <div class="cat-header">
                  <span class="cat-name">{{ cat.name }}</span>
                  <span class="cat-amount">HK$ {{ cat.amount.toLocaleString() }} ({{ cat.percent }}%)</span>
                </div>
                <el-progress 
                  :percentage="cat.percent" 
                  :color="cat.color" 
                  :show-text="false" 
                  :stroke-width="10" 
                  style="margin-top: 5px;"
                />
              </div>
            </div>
          </el-card>
        </el-col>
      </el-row>

    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage } from 'element-plus'
import { TrendCharts, Bottom, Warning, Wallet } from '@element-plus/icons-vue'

const loading = ref(false)
const savingBalance = ref(false)

// 數據狀態
const totalIncome = ref(0)
const totalExpense = ref(0)
const pendingInsurance = ref(0)
const totalFundValue = ref(0)
const pendingInsuranceCount = ref(0)
const pendingExpenseCount = ref(0)

// 對帳狀態
const prevBalance = ref(0)
const actualBalance = ref(0)
const inputActualBalance = ref(0)

// 🆕 分類統計狀態
const categoryStats = ref([])
const categoryColors = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#ec4899', '#06b6d4']

const currentMonthText = computed(() => {
  const d = new Date()
  return `${d.getFullYear()} 年 ${d.getMonth() + 1} 月`
})

const currentYearMonth = computed(() => {
  const d = new Date()
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}`
})

const prevYearMonth = computed(() => {
  const d = new Date()
  d.setMonth(d.getMonth() - 1)
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}`
})

const totalAssets = computed(() => {
  const bankBalance = actualBalance.value > 0 ? actualBalance.value : calculatedBalance.value
  return bankBalance + totalFundValue.value
})

const calculatedBalance = computed(() => {
  return prevBalance.value + totalIncome.value - totalExpense.value
})

const isReconciled = computed(() => {
  return actualBalance.value > 0 && Math.abs(actualBalance.value - calculatedBalance.value) < 1
})

const fetchDashboardData = async () => {
  loading.value = true
  const familyId = '00000000-0000-0000-0000-000000000001'

  try {
    // 1. 抓取本月收支 (包含分類)
    const [incRes, expRes] = await Promise.all([
      supabase.from('incomes').select('amount').eq('year_month', currentYearMonth.value),
      supabase.from('expenses').select('amount, status, category_name').eq('year_month', currentYearMonth.value)
    ])

    if (incRes.data) totalIncome.value = incRes.data.reduce((sum, item) => sum + Number(item.amount), 0)
    
    if (expRes.data) {
      totalExpense.value = expRes.data.reduce((sum, item) => sum + Number(item.amount), 0)
      pendingExpenseCount.value = expRes.data.filter(item => item.status !== '已處理').length

      // 🆕 計算分類佔比
      const catMap = {}
      expRes.data.forEach(item => {
        const cName = item.category_name || '未分類'
        if (!catMap[cName]) catMap[cName] = 0
        catMap[cName] += Number(item.amount)
      })

      const sortedCats = Object.entries(catMap)
        .map(([name, amount], index) => ({
          name,
          amount,
          percent: Math.round((amount / totalExpense.value) * 100),
          color: categoryColors[index % categoryColors.length]
        }))
        .sort((a, b) => b.amount - a.amount)

      categoryStats.value = sortedCats
    }

    // 2. 抓取結餘資料
    const [prevRes, currRes] = await Promise.all([
      supabase.from('monthly_balances').select('actual_balance').eq('year_month', prevYearMonth.value).maybeSingle(),
      supabase.from('monthly_balances').select('actual_balance').eq('year_month', currentYearMonth.value).maybeSingle()
    ])

    prevBalance.value = prevRes.data ? Number(prevRes.data.actual_balance) : 0
    if (currRes.data) {
      actualBalance.value = Number(currRes.data.actual_balance)
      inputActualBalance.value = actualBalance.value
    }

    // 3. 抓取本月未轉保險
    const [policiesRes, paymentsRes] = await Promise.all([
      supabase.from('insurance_policies').select('id, monthly_average'),
      supabase.from('insurance_payments').select('policy_id').eq('year_month', currentYearMonth.value).eq('status', '已轉')
    ])

    if (policiesRes.data) {
      const paidPolicyIds = (paymentsRes.data || []).map(p => p.policy_id)
      const unpaidPolicies = policiesRes.data.filter(p => !paidPolicyIds.includes(p.id))
      pendingInsuranceCount.value = unpaidPolicies.length
      pendingInsurance.value = unpaidPolicies.reduce((sum, p) => sum + Number(p.monthly_average), 0)
    }

    // 4. 抓取基金總現值
    const [fundsRes, snapshotsRes] = await Promise.all([
      supabase.from('funds').select('id'),
      supabase.from('fund_snapshots').select('fund_id, total_value_hkd, year_month')
    ])

    if (fundsRes.data && snapshotsRes.data) {
      fundsRes.data.forEach(fund => {
        const fSnaps = snapshotsRes.data.filter(s => s.fund_id === fund.id)
        if (fSnaps.length > 0) {
          fSnaps.sort((a, b) => b.year_month.localeCompare(a.year_month))
          totalFundValue.value += Number(fSnaps[0].total_value_hkd)
        }
      })
    }

  } catch (error) {
    console.error('載入工作台失敗', error)
  } finally {
    loading.value = false
  }
}

const saveActualBalance = async () => {
  if (inputActualBalance.value === null || inputActualBalance.value < 0) return ElMessage.warning('請輸入正確金額')
  
  savingBalance.value = true
  const familyId = '00000000-0000-0000-0000-000000000001'

  try {
    const { error } = await supabase.from('monthly_balances').upsert({
      family_id: familyId,
      year_month: currentYearMonth.value,
      actual_balance: inputActualBalance.value
    }, { onConflict: 'family_id, year_month' })

    if (error) throw error
    
    actualBalance.value = inputActualBalance.value
    ElMessage.success('✅ 結餘已更新')
    
    if (isReconciled.value) {
      ElMessage.success('🎉 帳目完全相符，太棒了！')
    }
  } catch (err) {
    ElMessage.error('儲存失敗：' + err.message)
  } finally {
    savingBalance.value = false
  }
}

onMounted(() => {
  fetchDashboardData()
})
</script>

<style scoped>
.dashboard-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: flex-end; margin-bottom: 20px; }
.page-header h2 { margin: 0; color: #1e293b; }
.date-text { font-size: 14px; color: #64748b; font-weight: 500; }

.mb-20 { margin-bottom: 20px; }

/* 總資產卡片 */
.total-asset-card { border-radius: 12px; border: none; background: linear-gradient(135deg, #1e293b 0%, #334155 100%); color: white; }
.asset-label { font-size: 14px; color: #94a3b8; margin-bottom: 8px; }
.asset-value { font-size: 28px; font-weight: bold; font-family: monospace; }

.asset-change-card { border-radius: 12px; border: none; background: white; }
.asset-change-card .asset-label { color: #64748b; }
.asset-change-card .asset-value { font-size: 28px; font-family: monospace; }

/* 統計卡片樣式 */
.stat-card { border-radius: 12px; border: none; position: relative; overflow: hidden; }
.stat-card :deep(.el-card__body) { display: flex; align-items: center; padding: 20px 15px; }
.stat-icon { width: 48px; height: 48px; border-radius: 50%; display: flex; justify-content: center; align-items: center; font-size: 24px; margin-right: 15px; flex-shrink: 0; }
.stat-info { display: flex; flex-direction: column; }
.stat-label { font-size: 13px; color: #64748b; margin-bottom: 4px; }
.stat-value { font-size: 18px; font-weight: bold; white-space: nowrap; }

/* 各卡片專屬顏色 */
.income-card .stat-icon { background: #ecfdf5; color: #10b981; }
.expense-card .stat-icon { background: #fef2f2; color: #ef4444; }
.pending-card .stat-icon { background: #fffbeb; color: #f59e0b; }
.fund-card .stat-icon { background: #eff6ff; color: #3b82f6; }

.text-green { color: #10b981; }
.text-red { color: #ef4444; }
.text-orange { color: #f59e0b; }
.text-blue { color: #3b82f6; }
.text-muted { color: #94a3b8; }
.font-bold { font-weight: 600; }
.mono { font-family: 'Courier New', Courier, monospace; font-weight: 600; }

/* 面板樣式 */
.panel-card { border-radius: 12px; border: none; height: 100%; }
.panel-header { font-weight: bold; color: #1e293b; display: flex; justify-content: space-between; align-items: center; }
.todo-list { min-height: 100px; }
.todo-item { display: flex; align-items: center; gap: 10px; padding: 10px 0; border-bottom: 1px dashed #e2e8f0; font-size: 14px; }
.todo-item:last-child { border-bottom: none; }
.todo-item span { flex: 1; color: #475569; }

/* 對帳區樣式 */
.reconcile-box { display: flex; flex-direction: column; gap: 10px; }
.calc-row { display: flex; justify-content: space-between; font-size: 14px; color: #475569; }
.calc-row.highlight { font-weight: bold; color: #1e293b; font-size: 15px; margin-top: 5px; }
.input-row { display: flex; align-items: center; justify-content: space-between; gap: 10px; margin-top: 10px; }
.input-row .label { font-size: 14px; font-weight: bold; color: #334155; }
.diff-row { display: flex; align-items: center; margin-top: 10px; padding: 10px; background: #f8fafc; border-radius: 6px; }

/* 🆕 分類分析樣式 */
.category-list { display: flex; flex-direction: column; gap: 15px; }
.category-item { display: flex; flex-direction: column; }
.cat-header { display: flex; justify-content: space-between; font-size: 14px; color: #334155; font-weight: 500; }
.cat-amount { font-family: monospace; }

/* 手機端優化 */
@media (max-width: 768px) {
  .asset-value { font-size: 24px; }
  .stat-value { font-size: 15px; }
  .stat-icon { width: 36px; height: 36px; font-size: 18px; margin-right: 10px; }
  .input-row { flex-direction: column; align-items: stretch; }
  .input-row .el-input-number { width: 100% !important; margin-bottom: 10px; }
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/DashboardView.vue
V: 1793811300
W: ai-wa-fs-001
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
            <!-- 🆕 編輯與刪除按鈕 -->
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
            <div class="info-row" style="margin-bottom: 4px;">
              <span class="label">供款進度</span>
              <span class="value">{{ getProgress(p).text }}</span>
            </div>
            <el-progress :percentage="getProgress(p).pct" :show-text="false" :stroke-width="6" />
          </div>
        </div>

        <div class="card-footer">
          <div class="status-info">
            <span>本月狀態：</span>
            <el-tag :type="getCurrentPaymentStatus(p.id) === '已轉' ? 'success' : 'info'" size="small">
              {{ getCurrentPaymentStatus(p.id) }}
            </el-tag>
          </div>
          <el-button 
            :type="getCurrentPaymentStatus(p.id) === '已轉' ? 'default' : 'success'" 
            size="small" 
            @click="togglePayment(p.id)"
          >
            {{ getCurrentPaymentStatus(p.id) === '已轉' ? '取消轉帳' : '確認已轉' }}
          </el-button>
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
        <el-row :gutter="10">
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
        <el-row :gutter="10">
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
import { Loading, InfoFilled, MoreFilled } from '@element-plus/icons-vue'

const policies = ref([])
const payments = ref([]) 
const loading = ref(true)
const dialogVisible = ref(false)
const submitting = ref(false)
const isEdit = ref(false) // 🆕 判斷是否為編輯模式

const currentYearMonth = computed(() => {
  const d = new Date()
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}`
})

const getProgress = (p) => {
  const start = new Date(p.start_date)
  const now = new Date()
  const diffMonths = (now.getFullYear() - start.getFullYear()) * 12 + (now.getMonth() - start.getMonth())
  const currentPeriod = Math.max(0, diffMonths + 1)
  const totalPeriods = p.total_years * 12
  const pct = Math.min(100, Math.round((currentPeriod / totalPeriods) * 100))
  return {
    text: `${currentPeriod} / ${totalPeriods} 期 (${pct}%)`,
    pct: pct
  }
}

const getCurrentPaymentStatus = (policyId) => {
  const record = payments.value.find(
    pay => pay.policy_id === policyId && pay.year_month === currentYearMonth.value
  )
  return record ? record.status : '未轉'
}

const togglePayment = async (policyId) => {
  const currentStatus = getCurrentPaymentStatus(policyId)
  const newStatus = currentStatus === '已轉' ? '未轉' : '已轉'
  
  const existing = payments.value.find(
    pay => pay.policy_id === policyId && pay.year_month === currentYearMonth.value
  )

  let error
  if (existing) {
    const res = await supabase.from('insurance_payments').update({ status: newStatus }).eq('id', existing.id)
    error = res.error
  } else {
    const res = await supabase.from('insurance_payments').insert([{
      policy_id: policyId,
      year_month: currentYearMonth.value,
      status: newStatus
    }])
    error = res.error
  }

  if (error) {
    ElMessage.error('狀態更新失敗：' + error.message)
  } else {
    ElMessage.success(`已標記為 ${newStatus}`)
    fetchData() 
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

// 🆕 處理下拉選單的指令 (編輯/刪除)
const handleCommand = (command, policy) => {
  if (command === 'edit') {
    isEdit.value = true
    form.value = { ...policy }
    dialogVisible.value = true
  } else if (command === 'delete') {
    deletePolicy(policy.id, policy.name)
  }
}

// 🆕 刪除保單
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

  let error
  if (isEdit.value) {
    // 更新
    const res = await supabase.from('insurance_policies').update(payload).eq('id', form.value.id)
    error = res.error
  } else {
    // 新增
    const res = await supabase.from('insurance_policies').insert([{
      family_id: '00000000-0000-0000-0000-000000000001',
      ...payload
    }])
    error = res.error
  }

  submitting.value = false
  
  if (error) {
    ElMessage.error('儲存失敗：' + error.message)
  } else {
    ElMessage.success(isEdit.value ? '✅ 保單已更新' : '✅ 保單已新增')
    dialogVisible.value = false
    fetchData()
  }
}

onMounted(() => fetchData())
</script>

<style scoped>
/* 樣式保持不變，僅增加 header-right 以對齊按鈕 */
.insurance-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
.page-header h2 { margin: 0; color: #1e293b; }
.policy-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 16px; }
.policy-card { border-radius: 12px; border: none; box-shadow: 0 2px 12px 0 rgba(0,0,0,0.05); }
.card-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 15px; border-bottom: 1px solid #f1f5f9; padding-bottom: 10px; }
.header-right { display: flex; align-items: center; }
.title-area h3 { margin: 0 0 4px 0; color: #0f172a; font-size: 16px; }
.company { font-size: 12px; color: #64748b; }
.card-body { display: flex; flex-direction: column; gap: 8px; }
.info-row { display: flex; justify-content: space-between; font-size: 14px; }
.label { color: #64748b; }
.value { font-weight: 600; color: #334155; }
.text-blue { color: #1d4ed8; }
.progress-area { margin-top: 10px; background: #f8fafc; padding: 10px; border-radius: 6px; }
.card-footer { display: flex; justify-content: space-between; align-items: center; margin-top: 15px; padding-top: 15px; border-top: 1px dashed #e2e8f0; }
.status-info { font-size: 13px; color: #64748b; display: flex; align-items: center; gap: 6px; }
.preview-box { background: #eff6ff; color: #1d4ed8; padding: 10px 15px; border-radius: 6px; display: flex; align-items: center; gap: 8px; font-size: 14px; margin-top: 10px; }
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/InsuranceView.vue
V: 1793809400
W: ai-wa-fs-001
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
      <div class="header-left">
        <h2>每月待轉帳表</h2>
        <el-date-picker 
          v-model="selectedMonth" 
          type="month" 
          placeholder="選擇月份" 
          value-format="YYYY-MM" 
          @change="fetchData" 
          class="month-picker"
        />
      </div>
    </div>

    <div v-if="loading" style="text-align: center; padding: 40px;">
      <el-icon class="is-loading" :size="30"><Loading /></el-icon>
    </div>

    <el-empty v-else-if="groupedExpenses.length === 0" description="本月沒有待轉帳的項目，太棒了！" />

    <div v-else class="settlement-grid">
      <!-- 按對象分組顯示 -->
      <el-card v-for="group in groupedExpenses" :key="group.payer" class="payer-card" shadow="hover">
        <div class="payer-header">
          <div class="payer-info">
            <h3>{{ group.payer }}</h3>
            <span class="item-count">{{ group.items.length }} 筆待處理</span>
          </div>
          <div class="total-amount">
            <span>待轉總額</span>
            <strong class="text-red">HK$ {{ group.total.toLocaleString() }}</strong>
          </div>
        </div>

        <div class="item-list">
          <div class="item-row" v-for="item in group.items" :key="item.id">
            <div class="item-name">
              <span>{{ item.name }}</span>
              <el-tag size="small" type="info">{{ item.category_name || '未分類' }}</el-tag>
            </div>
            <div class="item-amount">HK$ {{ item.amount.toLocaleString() }}</div>
          </div>
        </div>

        <div class="payer-footer">
          <el-button type="success" plain style="width: 100%;" @click="markAllAsPaid(group)">
            ✅ 標記 {{ group.payer }} 全部已轉
          </el-button>
        </div>
      </el-card>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage } from 'element-plus'
import { Loading } from '@element-plus/icons-vue'

const d = new Date()
const selectedMonth = ref(`${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}`)
const expenses = ref([])
const loading = ref(false)

// 🆕 將未處理的支出按對象分組
const groupedExpenses = computed(() => {
  const pending = expenses.value.filter(e => e.status !== '已處理')
  
  const groups = {}
  pending.forEach(item => {
    const payer = item.payer_name || '未指定對象'
    if (!groups[payer]) {
      groups[payer] = { payer, total: 0, items: [] }
    }
    groups[payer].items.push(item)
    groups[payer].total += Number(item.amount)
  })

  return Object.values(groups).sort((a, b) => b.total - a.total)
})

const fetchData = async () => {
  loading.value = true
  const { data, error } = await supabase
    .from('expenses')
    .select('*')
    .eq('year_month', selectedMonth.value)
    .neq('status', '已處理') // 只抓取未處理的
    .order('created_at', { ascending: false })

  if (error) ElMessage.error('載入待轉帳資料失敗')
  else expenses.value = data || []
  
  loading.value = false
}

// 🆕 一鍵標記某人的所有項目為已轉
const markAllAsPaid = async (group) => {
  try {
    const ids = group.items.map(i => i.id)
    
    const { error } = await supabase
      .from('expenses')
      .update({ status: '已處理' })
      .in('id', ids)

    if (error) throw error
    ElMessage.success(`✅ 已將 ${group.payer} 的 ${ids.length} 筆項目標記為已轉`)
    fetchData()
  } catch (err) {
    ElMessage.error('更新失敗：' + err.message)
  }
}

onMounted(() => fetchData())
</script>

<style scoped>
.settlement-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; flex-wrap: wrap; gap: 10px; }
.header-left { display: flex; align-items: center; flex-wrap: wrap; gap: 10px; }
.header-left h2 { margin: 0; color: #1e293b; }
.month-picker { width: 140px; }

.settlement-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 16px; }
.payer-card { border-radius: 12px; border: none; box-shadow: 0 2px 12px 0 rgba(0,0,0,0.05); }

.payer-header { display: flex; justify-content: space-between; align-items: flex-end; border-bottom: 2px solid #f1f5f9; padding-bottom: 12px; margin-bottom: 12px; }
.payer-info h3 { margin: 0 0 4px 0; font-size: 18px; color: #0f172a; }
.item-count { font-size: 13px; color: #64748b; }
.total-amount { text-align: right; }
.total-amount span { display: block; font-size: 12px; color: #64748b; margin-bottom: 2px; }
.total-amount strong { font-size: 20px; }
.text-red { color: #ef4444; }

.item-list { max-height: 250px; overflow-y: auto; margin-bottom: 15px; }
.item-row { display: flex; justify-content: space-between; align-items: center; padding: 8px 0; border-bottom: 1px dashed #e2e8f0; font-size: 14px; }
.item-row:last-child { border-bottom: none; }
.item-name { display: flex; align-items: center; gap: 8px; color: #334155; }
.item-amount { font-weight: 600; color: #475569; font-family: monospace; }

.payer-footer { border-top: 1px dashed #e2e8f0; padding-top: 15px; }

@media (max-width: 768px) {
  .settlement-grid { grid-template-columns: 1fr; }
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/SettlementView.vue
V: 1793810600
W: ai-wa-fs-001
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
