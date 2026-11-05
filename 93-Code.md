> 體系版本：SV13
> 生成時間：2026-11-06 07:45:14 (HKT)

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
    <!-- 側邊選單 -->
    <el-aside width="auto" class="sidebar">
      <div class="logo">
        <h2>家庭入帳 V3</h2>
      </div>
      <el-menu
        :default-active="$route.path"
        router
        background-color="#1e293b"
        text-color="#94a3b8"
        active-text-color="#38bdf8"
        :collapse="isCollapse"
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
      </el-menu>
    </el-aside>

    <!-- 主內容區 -->
    <el-container class="main-wrapper">
      <el-header class="header">
        <el-button class="menu-toggle" text @click="isCollapse = !isCollapse">
          <el-icon><Menu /></el-icon>
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
import { ref, onMounted } from 'vue'
import { supabase } from '@/config/supabase'
import { ElMessage } from 'element-plus'
import { HomeFilled, Money, Umbrella, Menu } from '@element-plus/icons-vue'

const isCollapse = ref(false)

onMounted(() => {
  // 手機端預設收起側邊欄
  if (window.innerWidth < 768) {
    isCollapse.value = true
  }
})

const handleLogout = async () => {
  await supabase.auth.signOut()
  window.location.href = '/login'
}
</script>

<style>
body { margin: 0; padding: 0; font-family: 'Noto Sans TC', sans-serif; }
.app-layout { height: 100vh; overflow: hidden; }
.sidebar { background-color: #1e293b; color: white; transition: width 0.3s; }
.logo { height: 60px; display: flex; align-items: center; justify-content: center; border-bottom: 1px solid #334155; overflow: hidden; }
.logo h2 { margin: 0; font-size: 18px; color: #f8fafc; white-space: nowrap; }
.sidebar .el-menu { border-right: none; }
.main-wrapper { flex: 1; width: 100%; overflow: hidden; }
.header { background-color: white; display: flex; align-items: center; border-bottom: 1px solid #e2e8f0; padding: 0 15px; }
.header-title { font-weight: bold; color: #334155; margin-left: 10px; flex: 1; }
.main-content { background-color: #f1f5f9; padding: 20px; overflow-y: auto; }
.menu-toggle { margin-right: 10px; color: #64748b; }
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/App.vue
V: 1793806700
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
V: 1793804500
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
  <div>
    <h2>收支流水</h2>
    <el-card>
      <el-empty description="收支功能開發中..." />
    </el-card>
  </div>
</template>

<script setup>
</script>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/views/CashflowView.vue
V: 1793806600
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
