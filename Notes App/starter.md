# Notes App - Quick Starter Guide
*Get up and running in 5 minutes*

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ installed
- npm/yarn/pnpm package manager
- VS Code with Vue.js extensions

### 1. Create Project
```bash
# Create Vue 3 + TypeScript project
npm create vue@latest notes-app
cd notes-app

# Install dependencies
npm install
```

### 2. Install Dependencies
```bash
# Core Vue.js dependencies
npm install vue@^3.4.0 vue-router@^4.2.0 pinia@^2.1.0 @vueuse/core@^10.0.0

# UI Framework
npm install vuetify@^3.4.0 @mdi/font@^7.4.0

# Rich Text Editor
npm install @tiptap/vue-3@^2.1.0 @tiptap/starter-kit@^2.1.0
npm install @tiptap/extension-image@^2.1.0 @tiptap/extension-link@^2.1.0
npm install @tiptap/extension-markdown@^2.1.0 @tiptap/extension-history@^2.1.0

# State Management & Data Fetching
npm install @tanstack/vue-query@^5.0.0

# File Upload & Storage
npm install vue-dropzone@^3.0.0

# Utilities
npm install date-fns@^2.30.0 lodash-es@^4.17.21 uuid@^9.0.0 crypto-js@^4.2.0

# Development Dependencies
npm install -D @types/lodash-es@^4.17.0 @types/uuid@^9.0.0 @types/crypto-js@^4.2.0
npm install -D eslint@^8.0.0 @typescript-eslint/eslint-plugin@^6.0.0
npm install -D @typescript-eslint/parser@^6.0.0 eslint-plugin-vue@^9.0.0
npm install -D prettier@^3.0.0 eslint-config-prettier@^9.0.0
npm install -D vitest@^1.0.0 @vue/test-utils@^2.4.0
npm install -D jsdom@^23.0.0 @testing-library/jest-dom@^6.0.0
npm install -D @testing-library/user-event@^14.0.0
npm install -D typescript@^5.0.0 @vitejs/plugin-vue@^4.0.0
npm install -D vite@^5.0.0 postcss@^8.4.0 autoprefixer@^10.4.0
npm install -D sass@^1.69.0 husky@^8.0.0 lint-staged@^15.0.0
```

### 3. Setup Project Structure
```bash
# Create directory structure
mkdir -p src/components/ui
mkdir -p src/components/note
mkdir -p src/components/notebook
mkdir -p src/components/layout
mkdir -p src/composables
mkdir -p src/stores
mkdir -p src/services/api
mkdir -p src/services/storage
mkdir -p src/services/external
mkdir -p src/utils
mkdir -p src/types
mkdir -p src/plugins
mkdir -p src/assets/images
mkdir -p src/assets/icons
mkdir -p src/assets/fonts
mkdir -p src/assets/styles
mkdir -p src/router
mkdir -p src/views
```

### 4. Copy Configuration Files

#### TypeScript Config
```bash
cat > tsconfig.json << 'EOF'
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
EOF
```

#### Vite Config
```bash
cat > vite.config.ts << 'EOF'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { fileURLToPath, URL } from 'node:url'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: '@import "@/assets/styles/variables.scss";'
      }
    }
  }
})
EOF
```

#### ESLint Config
```bash
cat > .eslintrc.js << 'EOF'
module.exports = {
  root: true,
  env: {
    node: true,
    browser: true,
    es2021: true
  },
  extends: [
    'eslint:recommended',
    '@vue/eslint-config-typescript',
    'plugin:vue/vue3-recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier'
  ],
  parser: 'vue-eslint-parser',
  parserOptions: {
    ecmaVersion: 2021,
    sourceType: 'module',
    parser: '@typescript-eslint/parser'
  },
  plugins: ['vue', '@typescript-eslint'],
  rules: {
    'vue/multi-word-component-names': 'off',
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/no-explicit-any': 'warn'
  }
}
EOF
```

#### Prettier Config
```bash
cat > .prettierrc << 'EOF'
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "none",
  "printWidth": 80,
  "endOfLine": "lf"
}
EOF
```

### 5. Create Essential Files

#### Vuetify Plugin
```bash
cat > src/plugins/vuetify.ts << 'EOF'
import 'vuetify/styles'
import { createVuetify } from 'vuetify'
import * as components from 'vuetify/components'
import * as directives from 'vuetify/directives'
import { aliases, mdi } from 'vuetify/iconsets/mdi'
import '@mdi/font/css/materialdesignicons.css'

export default createVuetify({
  components,
  directives,
  icons: {
    defaultSet: 'mdi',
    aliases,
    sets: {
      mdi
    }
  },
  theme: {
    defaultTheme: 'light',
    themes: {
      light: {
        colors: {
          primary: '#1C6EA4',
          secondary: '#33A1E0',
          accent: '#FFF9AF',
          error: '#f44336',
          warning: '#ff9800',
          info: '#33A1E0',
          success: '#4caf50'
        }
      },
      dark: {
        colors: {
          primary: '#33A1E0',
          secondary: '#1C6EA4',
          accent: '#FFF9AF',
          error: '#f44336',
          warning: '#ff9800',
          info: '#33A1E0',
          success: '#4caf50'
        }
      }
    }
  }
})
EOF
```

#### Pinia Plugin
```bash
cat > src/plugins/pinia.ts << 'EOF'
import { createPinia } from 'pinia'

export default createPinia()
EOF
```

#### Router Setup
```bash
cat > src/router/index.ts << 'EOF'
import { createRouter, createWebHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'

const routes: RouteRecordRaw[] = [
  {
    path: '/',
    name: 'Dashboard',
    component: () => import('@/views/DashboardView.vue')
  },
  {
    path: '/note/:id?',
    name: 'NoteDetail',
    component: () => import('@/views/NoteDetailView.vue')
  },
  {
    path: '/notebooks',
    name: 'Notebooks',
    component: () => import('@/views/NotebooksView.vue')
  },
  {
    path: '/search',
    name: 'Search',
    component: () => import('@/views/SearchView.vue')
  },
  {
    path: '/settings',
    name: 'Settings',
    component: () => import('@/views/SettingsView.vue')
  },
  {
    path: '/auth',
    name: 'Auth',
    component: () => import('@/views/AuthView.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
EOF
```

#### Main App
```bash
cat > src/main.ts << 'EOF'
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import pinia from './plugins/pinia'
import vuetify from './plugins/vuetify'

const app = createApp(App)

app.use(pinia)
app.use(router)
app.use(vuetify)

app.mount('#app')
EOF
```

#### App Component
```bash
cat > src/App.vue << 'EOF'
<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-app-bar-title>Notes App</v-app-bar-title>
    </v-app-bar>

    <v-navigation-drawer app>
      <v-list>
        <v-list-item to="/" prepend-icon="mdi-home" title="Dashboard" />
        <v-list-item to="/notebooks" prepend-icon="mdi-book" title="Notebooks" />
        <v-list-item to="/search" prepend-icon="mdi-magnify" title="Search" />
        <v-list-item to="/settings" prepend-icon="mdi-cog" title="Settings" />
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
// App component
</script>
EOF
```

#### Basic Views
```bash
# Dashboard
cat > src/views/DashboardView.vue << 'EOF'
<template>
  <v-container>
    <h1>Dashboard</h1>
    <p>Welcome to your Notes App!</p>
  </v-container>
</template>

<script setup lang="ts">
// TODO: Implement dashboard functionality
</script>
EOF

# Note Detail
cat > src/views/NoteDetailView.vue << 'EOF'
<template>
  <v-container>
    <h1>Note Detail</h1>
    <p>Note editor will go here</p>
  </v-container>
</template>

<script setup lang="ts">
// TODO: Implement note detail functionality
</script>
EOF

# Notebooks
cat > src/views/NotebooksView.vue << 'EOF'
<template>
  <v-container>
    <h1>Notebooks</h1>
    <p>Notebook management will go here</p>
  </v-container>
</template>

<script setup lang="ts">
// TODO: Implement notebooks functionality
</script>
EOF

# Search
cat > src/views/SearchView.vue << 'EOF'
<template>
  <v-container>
    <h1>Search</h1>
    <p>Search functionality will go here</p>
  </v-container>
</template>

<script setup lang="ts">
// TODO: Implement search functionality
</script>
EOF

# Settings
cat > src/views/SettingsView.vue << 'EOF'
<template>
  <v-container>
    <h1>Settings</h1>
    <p>Settings will go here</p>
  </v-container>
</template>

<script setup lang="ts">
// TODO: Implement settings functionality
</script>
EOF

# Auth
cat > src/views/AuthView.vue << 'EOF'
<template>
  <v-container>
    <h1>Authentication</h1>
    <p>Login/signup will go here</p>
  </v-container>
</template>

<script setup lang="ts">
// TODO: Implement authentication functionality
</script>
EOF
```

#### Style Variables
```bash
cat > src/assets/styles/variables.scss << 'EOF'
// Custom color palette
$primary-dark: #154D71;
$primary-medium: #1C6EA4;
$primary-light: #33A1E0;
$primary-accent: #FFF9AF;

// Color variations
$primary-dark-hover: #0f3a56;
$primary-medium-hover: #155a8a;
$primary-light-hover: #2a8bc7;
$primary-light-bg: #e6f4ff;
$primary-accent-hover: #fff7a0;

// Neutral colors
$neutral-10: #fafafa;
$neutral-20: #f5f5f5;
$neutral-30: #eeeeee;
$neutral-50: #9e9e9e;
$neutral-80: #424242;
$neutral-90: #212121;

// Semantic colors
$success-green: #4caf50;
$warning-orange: #ff9800;
$error-red: #f44336;
$info-blue: #2196f3;

// Surface colors
$surface-primary: #ffffff;
$surface-secondary: #fafafa;
$surface-tertiary: #f5f5f5;
$surface-accent: $primary-accent;
EOF
```

### 6. Update Package.json Scripts
```bash
npm pkg set scripts.dev="vite"
npm pkg set scripts.build="vue-tsc && vite build"
npm pkg set scripts.preview="vite preview"
npm pkg set scripts.test="vitest"
npm pkg set scripts.lint="eslint . --ext .vue,.js,.jsx,.cjs,.mjs,.ts,.tsx,.cts,.mts --fix --ignore-path .gitignore"
npm pkg set scripts.format="prettier --write src/"
```

### 7. Verify Setup
```bash
# Start development server
npm run dev

# In another terminal, run tests
npm run test

# Run linting
npm run lint

# Format code
npm run format
```

## ✅ Verification Checklist

- [ ] Project created with `npm create vue@latest`
- [ ] All dependencies installed successfully
- [ ] Directory structure created
- [ ] Configuration files in place
- [ ] Basic views created
- [ ] Development server starts (`npm run dev`)
- [ ] No TypeScript errors
- [ ] No ESLint errors
- [ ] Vuetify theme loads with custom colors
- [ ] Navigation works between views

## 🚀 Next Steps

After verification, follow the detailed [Development Plan](./development-plan.md) for:
- Type definitions
- Store implementation
- Component development
- Feature implementation
- Testing and deployment

## 📚 Project Documentation

- [Project Scope](./project-scope.md) - Project requirements and vision
- [Tech Stack](./tech-stack.md) - Technology choices and dependencies
- [Project Structure](./project-structure.md) - File organization and patterns
- [Design Guidelines](./design-guidelines.md) - UI/UX standards and components
- [Data Flow](./data-flow.md) - State management and data patterns
- [Development Plan](./development-plan.md) - Step-by-step implementation guide

---

**Time to complete**: ~5 minutes
**Status**: Ready for development
