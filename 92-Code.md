> 體系版本：SV08
> 生成時間：2026-10-27 13:03:17 (HKT)

# 專案全程式碼包：AI-WA-UPWO


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
