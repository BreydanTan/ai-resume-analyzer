# 基础设施提示词 | Foundation Infrastructure Prompts

**文档版本 | Document Version**: 1.0
**创建日期 | Created Date**: 2025-11-16
**语言 | Language**: 中英双语 (Bilingual)

---

## 提示词 1.1: React Router v7 项目初始化 | Initialize React Router v7 Project

### 目标 | Goal
创建基础 React Router v7 项目结构，安装精确版本依赖

### 中文提示词 | Chinese Prompt

```
请创建一个全新的 React Router v7 项目，使用以下精确版本的依赖和配置：

### 第一步：创建项目文件夹和初始化

创建项目目录并初始化 package.json：

{
  "name": "ai-resume-analyzer",
  "version": "0.0.1",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "react-router dev",
    "build": "react-router build",
    "start": "react-router-serve ./build/server/index.js",
    "typecheck": "tsc"
  },
  "dependencies": {
    "pdfjs-dist": "^5.3.93",
    "react": "^19.1.0",
    "react-dom": "^19.1.0",
    "react-dropzone": "^14.3.8",
    "zustand": "^5.0.6",
    "clsx": "^2.1.1",
    "tailwind-merge": "^3.3.1"
  },
  "devDependencies": {
    "@react-router/dev": "^7.5.3",
    "@tailwindcss/vite": "^4.1.4",
    "@types/node": "^20.0.0",
    "@types/react": "^19.0.0",
    "@types/react-dom": "^19.0.0",
    "react-router": "^7.5.3",
    "tailwindcss": "^4.1.4",
    "typescript": "^5.8.3",
    "vite": "^6.3.3",
    "vite-tsconfig-paths": "^5.1.4"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}

### 第二步：创建 TypeScript 配置文件

在项目根目录创建 tsconfig.json，内容如下：

{
  "compilerOptions": {
    "target": "ES2022",
    "useDefineForClassFields": true,
    "lib": [
      "ES2022",
      "DOM",
      "DOM.Iterable"
    ],
    "jsx": "react-jsx",
    "module": "ES2022",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "baseUrl": ".",
    "paths": {
      "~/*": [
        "./app/*"
      ]
    }
  },
  "include": [
    "app",
    "server.ts"
  ],
  "exclude": [
    "node_modules"
  ]
}

### 第三步：创建 Vite 配置文件

在项目根目录创建 vite.config.ts，内容如下：

import { reactRouter } from "@react-router/dev/vite";
import tailwindcss from "@tailwindcss/vite";
import { defineConfig } from "vite";
import tsconfigPaths from "vite-tsconfig-paths";

export default defineConfig({
  plugins: [tailwindcss(), reactRouter(), tsconfigPaths()],
});

### 第四步：创建 React Router 配置文件

在项目根目录创建 react-router.config.ts，内容如下：

import type { Config } from "@react-router/dev/config";

export default {
  ssr: true,
} satisfies Config;

### 第五步：创建目录结构

请创建以下目录结构：

ai-resume-analyzer/
├── app/
│   ├── routes/
│   │   ├── home.tsx
│   │   ├── auth.tsx
│   │   ├── upload.tsx
│   │   ├── resume.tsx
│   │   └── wipe.tsx
│   ├── components/
│   │   ├── Navbar.tsx
│   │   ├── FileUploader.tsx
│   │   ├── ResumeCard.tsx
│   │   ├── Summary.tsx
│   │   ├── Details.tsx
│   │   ├── ATS.tsx
│   │   ├── ScoreCircle.tsx
│   │   ├── ScoreGauge.tsx
│   │   ├── ScoreBadge.tsx
│   │   └── Accordion.tsx
│   ├── lib/
│   │   ├── puter.ts
│   │   ├── pdf2img.ts
│   │   └── utils.ts
│   ├── root.tsx
│   ├── routes.ts
│   └── app.css
├── types/
│   ├── index.d.ts
│   └── puter.d.ts
├── constants/
│   └── index.ts
├── public/
│   ├── icons/
│   ├── images/
│   └── readme/
├── package.json
├── tsconfig.json
├── vite.config.ts
├── react-router.config.ts
├── Dockerfile
└── README.md

完成后，请运行以下命令验证设置：
- npm install
- npm run dev

确保没有任何错误。
```

### English Prompt | 英文提示词

```
Please create a brand new React Router v7 project with the following exact versions and configuration:

### Step 1: Create Project Directory and Initialize

Create a project directory and initialize package.json with:

{
  "name": "ai-resume-analyzer",
  "version": "0.0.1",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "react-router dev",
    "build": "react-router build",
    "start": "react-router-serve ./build/server/index.js",
    "typecheck": "tsc"
  },
  "dependencies": {
    "pdfjs-dist": "^5.3.93",
    "react": "^19.1.0",
    "react-dom": "^19.1.0",
    "react-dropzone": "^14.3.8",
    "zustand": "^5.0.6",
    "clsx": "^2.1.1",
    "tailwind-merge": "^3.3.1"
  },
  "devDependencies": {
    "@react-router/dev": "^7.5.3",
    "@tailwindcss/vite": "^4.1.4",
    "@types/node": "^20.0.0",
    "@types/react": "^19.0.0",
    "@types/react-dom": "^19.0.0",
    "react-router": "^7.5.3",
    "tailwindcss": "^4.1.4",
    "typescript": "^5.8.3",
    "vite": "^6.3.3",
    "vite-tsconfig-paths": "^5.1.4"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}

### Step 2: Create TypeScript Configuration

Create tsconfig.json at project root:

{
  "compilerOptions": {
    "target": "ES2022",
    "useDefineForClassFields": true,
    "lib": [
      "ES2022",
      "DOM",
      "DOM.Iterable"
    ],
    "jsx": "react-jsx",
    "module": "ES2022",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "baseUrl": ".",
    "paths": {
      "~/*": [
        "./app/*"
      ]
    }
  },
  "include": [
    "app",
    "server.ts"
  ],
  "exclude": [
    "node_modules"
  ]
}

### Step 3: Create Vite Configuration

Create vite.config.ts at project root:

import { reactRouter } from "@react-router/dev/vite";
import tailwindcss from "@tailwindcss/vite";
import { defineConfig } from "vite";
import tsconfigPaths from "vite-tsconfig-paths";

export default defineConfig({
  plugins: [tailwindcss(), reactRouter(), tsconfigPaths()],
});

### Step 4: Create React Router Configuration

Create react-router.config.ts at project root:

import type { Config } from "@react-router/dev/config";

export default {
  ssr: true,
} satisfies Config;

### Step 5: Create Directory Structure

Create the following directory structure:

ai-resume-analyzer/
├── app/
│   ├── routes/
│   │   ├── home.tsx
│   │   ├── auth.tsx
│   │   ├── upload.tsx
│   │   ├── resume.tsx
│   │   └── wipe.tsx
│   ├── components/
│   │   ├── Navbar.tsx
│   │   ├── FileUploader.tsx
│   │   ├── ResumeCard.tsx
│   │   ├── Summary.tsx
│   │   ├── Details.tsx
│   │   ├── ATS.tsx
│   │   ├── ScoreCircle.tsx
│   │   ├── ScoreGauge.tsx
│   │   ├── ScoreBadge.tsx
│   │   └── Accordion.tsx
│   ├── lib/
│   │   ├── puter.ts
│   │   ├── pdf2img.ts
│   │   └── utils.ts
│   ├── root.tsx
│   ├── routes.ts
│   └── app.css
├── types/
│   ├── index.d.ts
│   └── puter.d.ts
├── constants/
│   └── index.ts
├── public/
│   ├── icons/
│   ├── images/
│   └── readme/
├── package.json
├── tsconfig.json
├── vite.config.ts
├── react-router.config.ts
├── Dockerfile
└── README.md

After completion, run these commands to verify:
- npm install
- npm run dev

Make sure there are no errors.
```

### 验证步骤 | Verification Steps

1. **运行开发服务器 | Start dev server**:
   ```bash
   npm install
   npm run dev
   ```

2. **检查输出 | Check output**:
   ```
   ✓ ready in 123ms
   ➜  Local:   http://localhost:5173/
   ```

3. **验证 TypeScript | Verify TypeScript**:
   ```bash
   npm run typecheck
   ```

4. **构建验证 | Build verification**:
   ```bash
   npm run build
   ```

### 预期结果 | Expected Results

- ✅ 无错误消息
- ✅ Vite 开发服务器启动成功
- ✅ React Router 路由配置识别正确
- ✅ TypeScript 编译无错误
- ✅ 所有依赖安装成功

---

## 提示词 1.2: 创建全局样式和 Tailwind 配置 | Configure Global Styles & Tailwind

### 目标 | Goal
创建 app.css 全局样式文件和 Tailwind CSS 4.1.4 配置

### 中文提示词 | Chinese Prompt

```
请在项目中创建和配置 Tailwind CSS 样式：

### 步骤 1: 创建 app.css

在 app/ 目录下创建 app.css 文件，内容如下：

@import "tailwindcss";

/* 自定义 CSS 变量 */
:root {
  --color-primary: rgb(59, 130, 246);
  --color-success: rgb(34, 197, 94);
  --color-warning: rgb(234, 179, 8);
  --color-danger: rgb(239, 68, 68);
  --color-gray: rgb(107, 114, 128);
}

/* 自定义 Tailwind 组件 */
@layer components {
  .gradient-border {
    @apply border-2 border-transparent;
    background: linear-gradient(white, white) padding-box,
                linear-gradient(135deg, #667eea, #764ba2) border-box;
  }

  .gradient-bg {
    @apply bg-gradient-to-r;
    background-image: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  }

  .card {
    @apply rounded-lg border border-gray-200 bg-white p-6 shadow-sm;
  }

  .btn-primary {
    @apply inline-flex items-center justify-center rounded-lg bg-blue-600 px-6 py-2 text-white font-medium hover:bg-blue-700 transition-colors;
  }

  .btn-secondary {
    @apply inline-flex items-center justify-center rounded-lg border border-gray-300 px-6 py-2 text-gray-900 font-medium hover:bg-gray-50 transition-colors;
  }

  .input-field {
    @apply w-full rounded-lg border border-gray-300 px-4 py-2 text-gray-900 placeholder-gray-400 focus:border-blue-500 focus:ring-1 focus:ring-blue-500 outline-none transition-colors;
  }
}

/* 自定义实用类 */
@layer utilities {
  .text-gradient {
    @apply bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent;
  }

  .glass {
    @apply bg-white/10 backdrop-blur-md;
  }

  .shadow-glow {
    @apply shadow-lg shadow-blue-500/50;
  }

  .truncate-multiline {
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}

/* 全局样式 */
html {
  scroll-behavior: smooth;
}

body {
  @apply bg-gray-50 text-gray-900 font-sans antialiased;
}

/* 平滑过渡 */
* {
  @apply transition-colors duration-200;
}

/* 滚动条样式 */
::-webkit-scrollbar {
  @apply w-2;
}

::-webkit-scrollbar-track {
  @apply bg-gray-100;
}

::-webkit-scrollbar-thumb {
  @apply bg-gray-400 rounded-full hover:bg-gray-500;
}

### 步骤 2: 创建 tailwind.config.js

在项目根目录创建 tailwind.config.js：

/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./app/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: 'var(--color-primary)',
        success: 'var(--color-success)',
        warning: 'var(--color-warning)',
        danger: 'var(--color-danger)',
      },
      spacing: {
        '128': '32rem',
        '144': '36rem',
      },
      borderRadius: {
        '4xl': '2rem',
      },
      animation: {
        'spin-slow': 'spin 3s linear infinite',
        'pulse-slow': 'pulse 3s cubic-bezier(0.4, 0, 0.6, 1) infinite',
      },
      keyframes: {
        shimmer: {
          '0%': { backgroundPosition: '-1000px 0' },
          '100%': { backgroundPosition: '1000px 0' },
        },
      },
    },
  },
  plugins: [],
}

### 步骤 3: 在 root.tsx 中导入样式

在 app/root.tsx 中导入样式：

import './app.css'

完成后运行：
npm run dev

验证样式是否正确加载。
```

### English Prompt | 英文提示词

```
Please create and configure Tailwind CSS styling in the project:

### Step 1: Create app.css

Create app.css in the app/ directory with the following content:

@import "tailwindcss";

/* Custom CSS Variables */
:root {
  --color-primary: rgb(59, 130, 246);
  --color-success: rgb(34, 197, 94);
  --color-warning: rgb(234, 179, 8);
  --color-danger: rgb(239, 68, 68);
  --color-gray: rgb(107, 114, 128);
}

/* Custom Tailwind Components */
@layer components {
  .gradient-border {
    @apply border-2 border-transparent;
    background: linear-gradient(white, white) padding-box,
                linear-gradient(135deg, #667eea, #764ba2) border-box;
  }

  .gradient-bg {
    @apply bg-gradient-to-r;
    background-image: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  }

  .card {
    @apply rounded-lg border border-gray-200 bg-white p-6 shadow-sm;
  }

  .btn-primary {
    @apply inline-flex items-center justify-center rounded-lg bg-blue-600 px-6 py-2 text-white font-medium hover:bg-blue-700 transition-colors;
  }

  .btn-secondary {
    @apply inline-flex items-center justify-center rounded-lg border border-gray-300 px-6 py-2 text-gray-900 font-medium hover:bg-gray-50 transition-colors;
  }

  .input-field {
    @apply w-full rounded-lg border border-gray-300 px-4 py-2 text-gray-900 placeholder-gray-400 focus:border-blue-500 focus:ring-1 focus:ring-blue-500 outline-none transition-colors;
  }
}

/* Custom Utility Classes */
@layer utilities {
  .text-gradient {
    @apply bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent;
  }

  .glass {
    @apply bg-white/10 backdrop-blur-md;
  }

  .shadow-glow {
    @apply shadow-lg shadow-blue-500/50;
  }

  .truncate-multiline {
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}

/* Global Styles */
html {
  scroll-behavior: smooth;
}

body {
  @apply bg-gray-50 text-gray-900 font-sans antialiased;
}

/* Smooth Transitions */
* {
  @apply transition-colors duration-200;
}

/* Scrollbar Styling */
::-webkit-scrollbar {
  @apply w-2;
}

::-webkit-scrollbar-track {
  @apply bg-gray-100;
}

::-webkit-scrollbar-thumb {
  @apply bg-gray-400 rounded-full hover:bg-gray-500;
}

### Step 2: Create tailwind.config.js

Create tailwind.config.js at project root:

/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./app/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: 'var(--color-primary)',
        success: 'var(--color-success)',
        warning: 'var(--color-warning)',
        danger: 'var(--color-danger)',
      },
      spacing: {
        '128': '32rem',
        '144': '36rem',
      },
      borderRadius: {
        '4xl': '2rem',
      },
      animation: {
        'spin-slow': 'spin 3s linear infinite',
        'pulse-slow': 'pulse 3s cubic-bezier(0.4, 0, 0.6, 1) infinite',
      },
      keyframes: {
        shimmer: {
          '0%': { backgroundPosition: '-1000px 0' },
          '100%': { backgroundPosition: '1000px 0' },
        },
      },
    },
  },
  plugins: [],
}

### Step 3: Import Styles in root.tsx

Import styles in app/root.tsx:

import './app.css'

After completion, run:
npm run dev

Verify that styles are loaded correctly.
```

### 验证步骤 | Verification Steps

1. **检查样式 | Check styling**:
   - 打开 http://localhost:5173
   - 打开浏览器开发者工具
   - 检查是否有 CSS 加载

2. **验证 Tailwind | Verify Tailwind**:
   ```bash
   npm run build
   ```
   - 检查输出中是否有 CSS 文件

3. **测试自定义类 | Test custom classes**:
   - 确保 `.card`, `.btn-primary` 等可以使用

### 预期结果 | Expected Results

- ✅ app.css 正确加载
- ✅ Tailwind CSS 样式应用
- ✅ 自定义组件和工具类可用
- ✅ 没有 CSS 警告

---

## 提示词 1.3: 创建路由配置 | Set Up Route Configuration

### 目标 | Goal
创建 React Router v7 routes.ts 路由配置文件

### 中文提示词 | Chinese Prompt

```
请创建 React Router v7 的路由配置文件：

在 app/routes.ts 中创建以下内容：

import { index, route, type RouteConfig } from "@react-router/dev/routes";

export default [
  index("routes/home.tsx"),
  route('/auth', 'routes/auth.tsx'),
  route('/upload', 'routes/upload.tsx'),
  route('/resume/:id', 'routes/resume.tsx'),
  route('/wipe', 'routes/wipe.tsx'),
] satisfies RouteConfig;

说明：
- index("routes/home.tsx") - 首页路由，对应 "/"
- route('/auth', 'routes/auth.tsx') - 认证页面路由
- route('/upload', 'routes/upload.tsx') - 上传页面路由
- route('/resume/:id', 'routes/resume.tsx') - 动态参数路由，:id 为简历ID
- route('/wipe', 'routes/wipe.tsx') - 数据清除页面路由

这个配置文件是 React Router v7 的标准格式，定义了所有应用路由及其关联的组件文件。
```

### English Prompt | 英文提示词

```
Please create the React Router v7 route configuration file:

Create the following content in app/routes.ts:

import { index, route, type RouteConfig } from "@react-router/dev/routes";

export default [
  index("routes/home.tsx"),
  route('/auth', 'routes/auth.tsx'),
  route('/upload', 'routes/upload.tsx'),
  route('/resume/:id', 'routes/resume.tsx'),
  route('/wipe', 'routes/wipe.tsx'),
] satisfies RouteConfig;

Explanation:
- index("routes/home.tsx") - Home route, maps to "/"
- route('/auth', 'routes/auth.tsx') - Authentication page route
- route('/upload', 'routes/upload.tsx') - Resume upload page route
- route('/resume/:id', 'routes/resume.tsx') - Dynamic parameter route, :id is the resume ID
- route('/wipe', 'routes/wipe.tsx') - Data purge utility route

This configuration file is the standard format for React Router v7, defining all application routes and their associated component files.
```

---

本文档包含了项目基础设施的核心提示词，按依赖顺序排列。完成这些步骤后，你将拥有一个完整的开发环境，可以继续开发后端和前端组件。

**Next Steps**: 进行到 02-backend-prompts.md 进行后端服务实现。
