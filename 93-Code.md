> 體系版本：SV13
> 生成時間：2026-11-06 08:19:09 (HKT)

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
  
  <link rel="manifest" href="/manifest.webmanifest">
  <meta name="theme-color" content="#0F172A">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="入帳 V3">
  
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
V: 1793804070
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
    <!-- 🚨 手機端遮罩：側邊欄打開時點擊遮罩可關閉 -->
    <div v-if="isMobile && !isCollapse" class="sidebar-mask" @click="isCollapse = true"></div>

    <!-- 側邊選單 -->
    <el-aside :width="isCollapse ? '64px' : '220px'" class="sidebar" :class="{ 'mobile-sidebar': isMobile }">
      <div class="logo">
        <h2 v-if="!isCollapse">家庭入帳 V3</h2>
      </div>
      <el-menu
        :default-active="$route.path"
        router
        background-color="#1e293b"
        text-color="#94a3b8"
        active-text-color="#38bdf8"
        :collapse="isCollapse"
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
        <el-menu-item index="/insurance">
          <el-icon><Umbrella /></el-icon>
          <template #title>保險供款</template>
        </el-menu-item>
        <el-menu-item index="/fund">
          <el-icon><TrendCharts /></el-icon>
          <template #title>基金投資</template>
        </el-menu-item>
      </el-menu>
    </el-aside>

    <!-- 主內容區 -->
    <el-container class="main-wrapper">
      <el-header class="header">
        <el-button class="menu-toggle" text @click="isCollapse = !isCollapse">
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
import { HomeFilled, Money, Umbrella, Menu, TrendCharts } from '@element-plus/icons-vue'

const isCollapse = ref(false)
const isMobile = ref(false)

// 偵測是否為手機版，並預設收起側邊欄
const checkScreenSize = () => {
  isMobile.value = window.innerWidth < 768
  if (isMobile.value) {
    isCollapse.value = true
  } else {
    isCollapse.value = false
  }
}

// 點擊選單後，如果是手機版就自動收起
const onMenuSelect = () => {
  if (isMobile.value) {
    isCollapse.value = true
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
body { margin: 0; padding: 0; font-family: 'Noto Sans TC', sans-serif; }
.app-layout { height: 100vh; overflow: hidden; position: relative; }

/* 側邊欄基礎樣式 */
.sidebar { 
  background-color: #1e293b; 
  color: white; 
  transition: width 0.3s; 
  z-index: 1000;
}
.logo { height: 60px; display: flex; align-items: center; justify-content: center; border-bottom: 1px solid #334155; overflow: hidden; }
.logo h2 { margin: 0; font-size: 18px; color: #f8fafc; white-space: nowrap; }
.sidebar .el-menu { border-right: none; }

/* 主內容區 */
.main-wrapper { flex: 1; width: 0; overflow: hidden; display: flex; flex-direction: column; }
.header { background-color: white; display: flex; align-items: center; border-bottom: 1px solid #e2e8f0; padding: 0 15px; height: 60px; flex-shrink: 0; }
.header-title { font-weight: bold; color: #334155; margin-left: 10px; flex: 1; }
.main-content { background-color: #f1f5f9; padding: 15px; overflow-y: auto; flex: 1; }
.menu-toggle { margin-right: 10px; color: #64748b; padding: 8px; }

/* 🚨 手機端專用樣式 */
@media (max-width: 768px) {
  .mobile-sidebar {
    position: absolute;
    height: 100vh;
    left: 0;
    top: 0;
    box-shadow: 2px 0 8px rgba(0,0,0,0.2);
  }
  .sidebar-mask {
    position: absolute;
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
V: 1793807600
W: ai-wa-fs-001
═══ */```

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

const app = createApp(App)

app.use(ElementPlus, { locale: zhTw })
app.use(router)

app.mount('#app')

// 🚨 強制反註冊所有舊的 Service Worker，徹底清除快取干擾
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.getRegistrations().then(function(registrations) {
    for(let registration of registrations) {
      registration.unregister();
    }
  });
}

/* ═══ EOF ═══
F: AI-WA-FS/src/main.js
V: 1793806100
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
V: 1793807500
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./src/router/index.js === -->

<!-- === FILE START: ./src/style.css === -->

## 📄 ./src/style.css

```css
:root {
  --primary-color: #409EFF;
  --bg-color: #F2F3F5;
  --text-main: #303133;
  --text-secondary: #909399;
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
}

/* 覆寫 Element Plus 部分預設樣式，讓介面更清爽 */
.el-card {
  border-radius: 8px;
  border: none;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.05) !important;
}

/* 隱藏手機端滾動條，保持美觀 */
::-webkit-scrollbar {
  display: none;
}

/* ═══ EOF ═══
F: AI-WA-FS/src/style.css
V: 1793804110
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./src/style.css === -->

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
        <!-- 🆕 切換顯示模式 -->
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
          
          <!-- 模式一：表格視圖 -->
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
              <el-table-column label="操作" width="120" align="center">
                <template #default="scope">
                  <el-button type="primary" link size="small" @click="toggleExpenseStatus(scope.row)">
                    {{ scope.row.status === '已處理' ? '取消處理' : '標記處理' }}
                  </el-button>
                  <el-button type="danger" text size="small" @click="deleteRecord('expenses', scope.row.id)">刪除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </div>

          <!-- 模式二：看板視圖 -->
          <div v-else class="kanban-wrapper" v-loading="loading">
            <!-- 未處理區域 -->
            <div class="kanban-column">
              <div class="kanban-header pending">
                未處理 <el-tag size="small" type="info">{{ pendingExpenses.length }}</el-tag>
              </div>
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
                    <el-button type="primary" size="small" plain @click="toggleExpenseStatus(item)">標記已處理</el-button>
                    <el-button type="danger" size="small" text @click="deleteRecord('expenses', item.id)">刪除</el-button>
                  </div>
                </el-card>
                <el-empty v-if="pendingExpenses.length === 0" description="沒有未處理項目" :image-size="60" />
              </div>
            </div>

            <!-- 已處理區域 -->
            <div class="kanban-column">
              <div class="kanban-header done">
                已處理 <el-tag size="small" type="success">{{ doneExpenses.length }}</el-tag>
              </div>
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
                    <el-button type="danger" size="small" text @click="deleteRecord('expenses', item.id)">刪除</el-button>
                  </div>
                </el-card>
                <el-empty v-if="doneExpenses.length === 0" description="沒有已處理項目" :image-size="60" />
              </div>
            </div>
          </div>
        </el-tab-pane>
        
        <!-- 收入維持表格，因為通常不需要處理狀態 -->
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
              <el-table-column label="操作" width="80" align="center">
                <template #default="scope">
                  <el-button type="danger" text size="small" @click="deleteRecord('incomes', scope.row.id)">刪除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </div>
        </el-tab-pane>
      </el-tabs>
    </el-card>

    <!-- 新增彈窗 (保持不變) -->
    <el-dialog v-model="dialogVisible" :title="`新增${activeTab === 'expense' ? '支出' : '收入'}`" width="90%" max-width="500px">
      <el-form :model="form" label-position="top">
        <el-form-item label="項目名稱" required>
          <el-input v-model="form.name" :placeholder="activeTab === 'expense' ? '例如：買餸、交租' : '例如：薪金、家用轉入'" />
        </el-form-item>
        <el-form-item label="金額 (HKD)" required>
          <el-input-number v-model="form.amount" :min="1" style="width: 100%" />
        </el-form-item>
        <el-form-item label="對象" required>
          <el-select v-model="form.member_name" style="width: 100%">
            <el-option label="老公" value="老公" />
            <el-option label="老婆" value="老婆" />
            <el-option label="工人" value="工人" />
            <el-option label="家庭共用" value="家庭共用" />
          </el-select>
        </el-form-item>
        <el-form-item label="類別" v-if="activeTab === 'expense'">
          <el-select v-model="form.category_name" style="width: 100%" allow-create filterable default-first-option>
            <el-option label="日常飲食" value="日常飲食" />
            <el-option label="交通" value="交通" />
            <el-option label="租金" value="租金" />
            <el-option label="水電煤" value="水電煤" />
            <el-option label="保險" value="保險" />
            <el-option label="雜費" value="雜費" />
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

const d = new Date()
const selectedMonth = ref(`${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}`)

const activeTab = ref('expense')
const viewMode = ref('table') // 🆕 'table' 或 'kanban'
const expenses = ref([])
const incomes = ref([])
const loading = ref(false)
const dialogVisible = ref(false)
const submitting = ref(false)

const totalExpense = computed(() => expenses.value.reduce((sum, e) => sum + Number(e.amount), 0))
const totalIncome = computed(() => incomes.value.reduce((sum, i) => sum + Number(i.amount), 0))

// 🆕 區分未處理與已處理
const pendingExpenses = computed(() => expenses.value.filter(e => e.status !== '已處理'))
const doneExpenses = computed(() => expenses.value.filter(e => e.status === '已處理'))

const form = ref({ name: '', amount: 1, member_name: '家庭共用', category_name: '日常飲食', status: '未處理' })

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

// 🆕 一鍵切換處理狀態
const toggleExpenseStatus = async (item) => {
  const newStatus = item.status === '已處理' ? '未處理' : '已處理'
  const { error } = await supabase.from('expenses').update({ status: newStatus }).eq('id', item.id)
  if (error) ElMessage.error('更新失敗：' + error.message)
  else {
    item.status = newStatus // 畫面即時更新
  }
}

const openAddDialog = () => {
  form.value = { name: '', amount: 1, member_name: '家庭共用', category_name: '日常飲食', status: '未處理' }
  dialogVisible.value = true
}

const submitForm = async () => {
  if (!form.value.name || form.value.amount <= 0) return ElMessage.warning('請填寫完整資訊')
  submitting.value = true
  const familyId = '00000000-0000-0000-0000-000000000001'
  
  if (activeTab.value === 'expense') {
    const { error } = await supabase.from('expenses').insert([{
      family_id: familyId,
      year_month: selectedMonth.value,
      name: form.value.name,
      amount: form.value.amount,
      payer_name: form.value.member_name,
      category_name: form.value.category_name,
      status: form.value.status
    }])
    if (error) ElMessage.error('儲存失敗：' + error.message)
    else {
      ElMessage.success('✅ 支出已新增')
      dialogVisible.value = false
      fetchData()
    }
  } else {
    const { error } = await supabase.from('incomes').insert([{
      family_id: familyId,
      year_month: selectedMonth.value,
      name: form.value.name,
      amount: form.value.amount,
      member_name: form.value.member_name
    }])
    if (error) ElMessage.error('儲存失敗：' + error.message)
    else {
      ElMessage.success('✅ 收入已新增')
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

onMounted(() => fetchData())
</script>

<style scoped>
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

/* 表格橫向滑動 */
.table-wrapper { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }

/* 🆕 看板模式樣式 */
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

/* 手機端優化：看板變成上下排列 */
@media (max-width: 768px) {
  .kanban-wrapper { flex-direction: column; overflow-x: hidden; }
  .kanban-column { min-width: 100%; }
  .view-toggle { width: 100%; display: flex; justify-content: center; }
  .header-actions { width: 100%; justify-content: space-between; }
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/CashflowView.vue
V: 1793807300
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./src/views/CashflowView.vue === -->

<!-- === FILE START: ./src/views/DashboardView.vue === -->

## 📄 ./src/views/DashboardView.vue

```vue
<template>
  <div class="dashboard-container">
    <el-card class="header-card">
      <div class="header-content">
        <h2>工作台 (Dashboard)</h2>
        <el-button type="danger" plain @click="handleLogout">登出</el-button>
      </div>
      <p class="welcome-text">歡迎回來！這裡之後會顯示您的總資產與待辦事項。</p>
    </el-card>
  </div>
</template>

<script setup>
import { supabase } from '@/config/supabase'
import { ElMessage } from 'element-plus'

const handleLogout = async () => {
  const { error } = await supabase.auth.signOut()
  if (error) {
    ElMessage.error('登出失敗：' + error.message)
  } else {
    ElMessage.success('已安全登出')
    window.location.reload() // 重新載入以回到登入頁
  }
}
</script>

<style scoped>
.dashboard-container {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}
.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.header-content h2 {
  margin: 0;
  color: var(--text-main);
}
.welcome-text {
  margin: 0;
  color: var(--text-secondary);
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/DashboardView.vue
V: 1793804300
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
          <h3>{{ fund.name }}</h3>
          <el-button type="danger" text size="small" @click="deleteFund(fund.id)">刪除</el-button>
        </div>
        
        <div class="card-body">
          <div class="info-row">
            <span class="label">投入成本</span>
            <span class="value">HK$ {{ fund.manual_cost.toLocaleString() }}</span>
          </div>
          <div class="info-row">
            <span class="label">當前現值</span>
            <span class="value text-blue">HK$ {{ getLatestValue(fund).toLocaleString() }}</span>
          </div>
          
          <div class="pnl-box" :class="getPnl(fund) >= 0 ? 'pnl-positive' : 'pnl-negative'">
            <span>盈虧</span>
            <strong>{{ getPnl(fund) >= 0 ? '+' : '' }} HK$ {{ getPnl(fund).toLocaleString() }}</strong>
          </div>
          
          <div class="latest-info" v-if="getLatestSnapshot(fund)">
            最新快照：{{ getLatestSnapshot(fund).year_month }} 
            ({{ getLatestSnapshot(fund).units }} 單位 @ USD {{ getLatestSnapshot(fund).nav }})
          </div>
          <div class="latest-info" v-else>
            尚未輸入任何快照
          </div>
        </div>
      </el-card>
    </div>

    <!-- 新增基金彈窗 -->
    <el-dialog v-model="fundDialogVisible" title="新增基金" width="90%" max-width="400px">
      <el-form :model="fundForm" label-position="top">
        <el-form-item label="基金名稱" required>
          <el-input v-model="fundForm.name" placeholder="例如：駿利亨德森遠見基金" />
        </el-form-item>
        <el-form-item label="投入成本 (HKD)" required>
          <el-input-number v-model="fundForm.manual_cost" :min="0" style="width: 100%" />
        </el-form-item>
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
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage, ElMessageBox } from 'element-plus'
import { Loading } from '@element-plus/icons-vue'

const funds = ref([])
const snapshots = ref([])
const loading = ref(false)
const submitting = ref(false)
const fundDialogVisible = ref(false)
const snapshotDialogVisible = ref(false)

// 表單狀態
const fundForm = ref({ name: '', manual_cost: 0 })
const snapshotForm = ref({
  fund_id: '',
  year_month: new Date().toISOString().slice(0, 7),
  units: 0,
  nav: 0,
  exchange_rate: 7.8
})

// 總計計算
const totalCost = computed(() => funds.value.reduce((sum, f) => sum + Number(f.manual_cost), 0))
const totalValue = computed(() => funds.value.reduce((sum, f) => sum + getLatestValue(f), 0))
const totalPnl = computed(() => totalValue.value - totalCost.value)

// 取得某基金的最新快照
const getLatestSnapshot = (fund) => {
  const fSnaps = snapshots.value.filter(s => s.fund_id === fund.id)
  if (fSnaps.length === 0) return null
  return fSnaps.sort((a, b) => b.year_month.localeCompare(a.year_month))[0]
}

// 取得某基金最新現值
const getLatestValue = (fund) => {
  const latest = getLatestSnapshot(fund)
  return latest ? Number(latest.total_value_hkd) : 0
}

// 取得某基金盈虧
const getPnl = (fund) => {
  return getLatestValue(fund) - Number(fund.manual_cost)
}

// 預估快照總值
const computedSnapshotValue = computed(() => {
  const v = (snapshotForm.value.units || 0) * (snapshotForm.value.nav || 0) * (snapshotForm.value.exchange_rate || 0)
  return Math.round(v)
})

const fetchData = async () => {
  loading.value = true
  const [fundRes, snapRes] = await Promise.all([
    supabase.from('funds').select('*').order('created_at', { ascending: false }),
    supabase.from('fund_snapshots').select('*')
  ])
  if (!fundRes.error) funds.value = fundRes.data || []
  if (!snapRes.error) snapshots.value = snapRes.data || []
  loading.value = false
}

const openFundDialog = () => {
  fundForm.value = { name: '', manual_cost: 0 }
  fundDialogVisible.value = true
}

const submitFund = async () => {
  if (!fundForm.value.name) return ElMessage.warning('請輸入名稱')
  submitting.value = true
  const { error } = await supabase.from('funds').insert([{
    family_id: '00000000-0000-0000-0000-000000000001',
    name: fundForm.value.name,
    manual_cost: fundForm.value.manual_cost
  }])
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

.fund-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 16px; }
.fund-card { border-radius: 12px; border: none; box-shadow: 0 2px 12px 0 rgba(0,0,0,0.05); }
.card-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #f1f5f9; padding-bottom: 10px; margin-bottom: 15px; }
.card-header h3 { margin: 0; font-size: 16px; color: #0f172a; }

.card-body { display: flex; flex-direction: column; gap: 8px; }
.info-row { display: flex; justify-content: space-between; font-size: 14px; }
.label { color: #64748b; }
.value { font-weight: 600; }

.pnl-box { display: flex; justify-content: space-between; padding: 10px; border-radius: 6px; font-weight: bold; margin-top: 5px; }
.pnl-positive { background: #ecfdf5; color: #10b981; }
.pnl-negative { background: #fef2f2; color: #ef4444; }

.latest-info { font-size: 12px; color: #94a3b8; margin-top: 5px; text-align: right; }
.preview-box { background: #eff6ff; color: #1d4ed8; padding: 10px; border-radius: 6px; font-size: 14px; text-align: center; }

/* 手機端優化 */
@media (max-width: 768px) {
  .header-actions { width: 100%; justify-content: flex-end; margin-top: 10px; }
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/FundView.vue
V: 1793807700
W: ai-wa-fs-001
═══ */```

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
          <el-tag :type="p.type === 'fund_insurance' ? 'warning' : 'primary'" size="small">
            {{ p.type === 'fund_insurance' ? '基金保險' : '普通保險' }}
          </el-tag>
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
          
          <!-- 🆕 自動計算進度 -->
          <div class="progress-area">
            <div class="info-row" style="margin-bottom: 4px;">
              <span class="label">供款進度</span>
              <span class="value">{{ getProgress(p).text }}</span>
            </div>
            <el-progress :percentage="getProgress(p).pct" :show-text="false" :stroke-width="6" />
          </div>
        </div>

        <!-- 🆕 本月狀態切換 -->
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

    <!-- 新增保單彈窗 (與之前相同) -->
    <el-dialog v-model="dialogVisible" title="新增保單" width="90%" max-width="500px" :close-on-click-modal="false">
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
import { ElMessage } from 'element-plus'
import { Loading, InfoFilled } from '@element-plus/icons-vue'

const policies = ref([])
const payments = ref([]) // 🆕 儲存所有繳費紀錄
const loading = ref(true)
const dialogVisible = ref(false)
const submitting = ref(false)

// 取得當前年月字串 (例如 "2026-11")
const currentYearMonth = computed(() => {
  const d = new Date()
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}`
})

// 🆕 計算保單進度
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

// 🆕 取得該保單「本月」的狀態
const getCurrentPaymentStatus = (policyId) => {
  const record = payments.value.find(
    pay => pay.policy_id === policyId && pay.year_month === currentYearMonth.value
  )
  return record ? record.status : '未轉'
}

// 🆕 切換狀態（寫入或更新資料庫）
const togglePayment = async (policyId) => {
  const currentStatus = getCurrentPaymentStatus(policyId)
  const newStatus = currentStatus === '已轉' ? '未轉' : '已轉'
  
  // 找找看有沒有這個月的紀錄
  const existing = payments.value.find(
    pay => pay.policy_id === policyId && pay.year_month === currentYearMonth.value
  )

  let error
  if (existing) {
    // 更新
    const res = await supabase.from('insurance_payments').update({ status: newStatus }).eq('id', existing.id)
    error = res.error
  } else {
    // 新增
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
    fetchData() // 重新抓取資料刷新畫面
  }
}

// 表單相關
const form = ref({ name: '', company: '', start_date: new Date().toISOString().split('T')[0], total_years: 10, payment_freq: '月繳', premium_amount: 0, type: 'normal' })
const computedMonthlyAverage = computed(() => form.value.payment_freq === '年繳' ? Math.round((form.value.premium_amount || 0) / 12) : (form.value.premium_amount || 0))

// 獲取資料
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
  form.value = { name: '', company: '', start_date: new Date().toISOString().split('T')[0], total_years: 10, payment_freq: '月繳', premium_amount: 0, type: 'normal' }
  dialogVisible.value = true
}

const submitForm = async () => {
  if (!form.value.name || !form.value.start_date || form.value.premium_amount <= 0) return ElMessage.warning('請填寫完整資訊')
  submitting.value = true
  const { error } = await supabase.from('insurance_policies').insert([{
    family_id: '00000000-0000-0000-0000-000000000001',
    name: form.value.name, company: form.value.company, start_date: form.value.start_date,
    total_years: form.value.total_years, payment_freq: form.value.payment_freq,
    premium_amount: form.value.premium_amount, monthly_average: computedMonthlyAverage.value,
    type: form.value.type
  }])
  submitting.value = false
  if (error) ElMessage.error('儲存失敗：' + error.message)
  else {
    ElMessage.success('✅ 保單已新增')
    dialogVisible.value = false
    fetchData()
  }
}

onMounted(() => fetchData())
</script>

<style scoped>
.insurance-container { width: 100%; }
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
.page-header h2 { margin: 0; color: #1e293b; }
.policy-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 16px; }
.policy-card { border-radius: 12px; border: none; box-shadow: 0 2px 12px 0 rgba(0,0,0,0.05); }
.card-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 15px; border-bottom: 1px solid #f1f5f9; padding-bottom: 10px; }
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
V: 1793806900
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./src/views/InsuranceView.vue === -->

<!-- === FILE START: ./src/views/LoginView.vue === -->

## 📄 ./src/views/LoginView.vue

```vue
<template>
  <div class="login-container">
    <el-card class="login-card">
      <div class="login-header">
        <h2>家庭入帳 V3</h2>
        <p>請輸入您的專屬帳號登入</p>
      </div>
      
      <el-form @submit.prevent="handleLogin">
        <el-form-item>
          <el-input v-model="account" placeholder="請輸入帳號" size="large" />
        </el-form-item>
        <el-form-item>
          <el-input v-model="password" type="password" placeholder="請輸入密碼" size="large" show-password />
        </el-form-item>
        
        <el-button type="primary" native-type="submit" :loading="loading" class="login-btn" size="large">
          登入
        </el-button>
      </el-form>

      <!-- 🚨 這裡會把詳細錯誤印在畫面上，手機才看得到 -->
      <div v-if="errorMsg" style="margin-top: 20px; padding: 10px; background: #ffebee; color: #c62828; border-radius: 4px; font-size: 13px; word-break: break-all;">
        錯誤原因：{{ errorMsg }}
      </div>

    </el-card>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage } from 'element-plus'

const account = ref('')
const password = ref('')
const loading = ref(false)
const errorMsg = ref('') // 🚨 新增錯誤顯示變數

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
    // 🚨 將詳細錯誤寫入畫面
    errorMsg.value = error.message || JSON.stringify(error)
    ElMessage.error('登入失敗')
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.login-container { display: flex; justify-content: center; align-items: center; height: 100vh; background-color: #F2F3F5; }
.login-card { width: 100%; max-width: 400px; padding: 20px; }
.login-header { text-align: center; margin-bottom: 30px; }
.login-header h2 { margin: 0 0 10px 0; color: #303133; }
.login-header p { margin: 0; color: #909399; font-size: 14px; }
.login-btn { width: 100%; margin-top: 10px; }
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/LoginView.vue
V: 1793805800
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./src/views/LoginView.vue === -->

<!-- === FILE START: ./vite.config.js === -->

## 📄 ./vite.config.js

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

export default defineConfig({
  base: '/',
  plugins: [
    vue()
    // 暫時移除 VitePWA 外掛，避免開發階段網路請求被快取干擾
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
V: 1793078600
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./vite.config.js === -->
