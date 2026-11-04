> 體系版本：SV13
> 生成時間：2026-11-05 07:17:00 (HKT)

# 專案全程式碼包：AI-WA-FS


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
  <div class="app-container">
    <div class="loading-box">
      <h1>家庭入帳 V3</h1>
      <p>系統初始化中... 準備載入資料庫</p>
      <el-button type="primary" @click="testConnection">測試按鈕</el-button>
    </div>
  </div>
</template>

<script setup>
import { ElMessage } from 'element-plus'

const testConnection = () => {
  ElMessage.success('Vue 與 Element Plus 已成功載入！')
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: 'Noto Sans TC', sans-serif;
  background-color: #F2F3F5;
  color: #333;
}
.app-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.loading-box {
  text-align: center;
  padding: 40px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}
</style>
<!-- ═══ EOF ═══
F: AI-WA-FS/src/App.vue
V: 1793804090
W: ai-wa-fs-001
═══ -->```

<!-- === FILE END: ./src/App.vue === -->

<!-- === FILE START: ./src/config/supabase.js === -->

## 📄 ./src/config/supabase.js

```js
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = 'https://vatdtxdgljowdwvwbzdmt.supabase.co'
const supabaseKey = 'sb_publishable_rY1TM0Zw5-VXisWCMo5Qkg_5M-Q-YOn'

export const supabase = createClient(supabaseUrl, supabaseKey)

/* ═══ EOF ═══
F: AI-WA-FS/src/config/supabase.js
V: 1793804080
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./src/config/supabase.js === -->

<!-- === FILE START: ./vite.config.js === -->

## 📄 ./vite.config.js

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { VitePWA } from 'vite-plugin-pwa'
import path from 'path'

export default defineConfig({
  plugins: [
    vue(),
    VitePWA({
      registerType: 'autoUpdate',
      includeAssets: ['favicon.ico', 'apple-touch-icon.png', 'masked-icon.svg'],
      manifest: {
        name: 'AI-WA-FS 家庭財務系統',
        short_name: 'AI-WA-FS',
        description: '家庭財務與保險基金管理',
        theme_color: '#0F172A',
        background_color: '#0F172A',
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
V: 1793078600
W: ai-wa-fs-001
═══ */```

<!-- === FILE END: ./vite.config.js === -->
