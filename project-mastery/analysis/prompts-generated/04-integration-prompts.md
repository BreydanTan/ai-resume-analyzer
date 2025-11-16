# 集成与部署提示词 | Integration & Deployment Prompts

**文档版本 | Document Version**: 1.0
**创建日期 | Created Date**: 2025-11-16
**语言 | Language**: 中英双语 (Bilingual)

---

## 提示词 4.1: 创建 Docker 配置 | Create Docker Configuration

### 目标 | Goal
创建 Dockerfile - 多阶段构建优化的生产镜像

### 中文提示词 | Chinese Prompt

```
请在项目根目录创建 Dockerfile，使用多阶段构建策略优化镜像大小：

# === 阶段 1: 开发依赖 ===
FROM node:20-alpine AS development-dependencies-env
COPY . /app
WORKDIR /app
RUN npm ci

# === 阶段 2: 生产依赖 ===
FROM node:20-alpine AS production-dependencies-env
COPY ./package.json package-lock.json /app/
WORKDIR /app
RUN npm ci --omit=dev

# === 阶段 3: 构建 ===
FROM node:20-alpine AS build-env
COPY . /app/
COPY --from=development-dependencies-env /app/node_modules /app/node_modules
WORKDIR /app
RUN npm run build

# === 阶段 4: 生产运行时 ===
FROM node:20-alpine
COPY ./package.json package-lock.json /app/
COPY --from=production-dependencies-env /app/node_modules /app/node_modules
COPY --from=build-env /app/build /app/build
WORKDIR /app
EXPOSE 3000
CMD ["npm", "run", "start"]
```

### 说明 | Explanation

**多阶段构建的优势**:
1. **阶段 1 (development-dependencies-env)**: 安装包括开发依赖在内的所有依赖
2. **阶段 2 (production-dependencies-env)**: 仅安装生产依赖（减少镜像大小 70-80%）
3. **阶段 3 (build-env)**: 使用阶段 1 的依赖进行构建
4. **阶段 4 (最终镜像)**: 仅包含运行时需要的文件

**最终镜像包含**:
- Node.js 20 Alpine 基础镜像
- package.json 和 package-lock.json
- 生产依赖 (node_modules)
- 构建输出 (/build)

**不包含**:
- TypeScript 源代码
- 开发依赖 (Vite, TypeScript 编译器)
- node_modules 的开发工具

**镜像大小估算**:
- 基础镜像: ~150 MB
- 生产依赖: ~200 MB
- 构建输出: ~50 MB
- 总计: ~400 MB

---

## 提示词 4.2: 创建 .dockerignore 文件 | Create .dockerignore File

### 目标 | Goal
减少 Docker 构建上下文大小

### 中文提示词 | Chinese Prompt

```
请在项目根目录创建 .dockerignore 文件：

node_modules
npm-debug.log
build
dist
.git
.gitignore
.github
.env
.env.local
.env.*.local
coverage
.vscode
.idea
*.swp
*.swo
*~
.DS_Store
.turbo
.next
out
```

### 说明 | Explanation

这个文件告诉 Docker 在复制文件时忽略哪些文件和目录，减少构建时间和镜像大小。

---

## 提示词 4.3: 创建 .env 配置文件 | Create Environment Configuration

### 目标 | Goal
配置环境变量文件

### 中文提示词 | Chinese Prompt

```
请创建以下环境配置文件：

### .env.example (示例配置文件)

# Puter 配置
# 注意：Puter.js 是客户端 SDK，不需要后端 API 密钥
# 所有配置通过浏览器自动处理

# 应用配置
VITE_APP_NAME=RESUMIND
VITE_APP_DESCRIPTION=AI Resume Analyzer

# API 端点（如果使用自定义后端）
# VITE_API_URL=http://localhost:3000

# 开发模式
VITE_DEV_TOOLS=true

### .env.local (本地开发配置 - 不提交到 Git)

# 复制 .env.example 到 .env.local 并修改需要的值

```

### 说明 | Explanation

- **VITE_** 前缀: 这些变量在构建时被包含到客户端代码中
- 所有敏感信息应该放在 .env.local 中（不提交到 Git）
- Puter.js 不需要环境变量配置，SDK 通过 CDN 自动加载

---

## 提示词 4.4: 创建 .gitignore 文件 | Create .gitignore File

### 目标 | Goal
配置 Git 忽略文件

### 中文提示词 | Chinese Prompt

```
请创建 .gitignore 文件：

# 依赖
node_modules
/.pnp
.pnp.js
package-lock.json
yarn.lock
pnpm-lock.yaml

# 构建输出
/build
/dist
/.next
/out

# 环境变量
.env
.env.local
.env.*.local

# IDE
.vscode
.idea
*.swp
*.swo
*~
.DS_Store
*.sublime-project
*.sublime-workspace

# 日志
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# 测试
coverage
.nyc_output

# 临时文件
.turbo
.cache
```

---

## 提示词 4.5: 创建 package.json 脚本 | Verify npm Scripts

### 目标 | Goal
验证和优化 npm 脚本

### 中文提示词 | Chinese Prompt

```
请验证 package.json 中的脚本配置。确保包含以下脚本：

{
  "scripts": {
    "dev": "react-router dev",
    "build": "react-router build",
    "start": "react-router-serve ./build/server/index.js",
    "typecheck": "tsc",
    "lint": "eslint app --ext .ts,.tsx",
    "preview": "react-router preview"
  }
}

### 脚本说明

1. **npm run dev** - 启动开发服务器（热更新）
   - 运行: react-router dev
   - 访问: http://localhost:5173

2. **npm run build** - 构建生产版本
   - 运行: react-router build
   - 输出: ./build 目录

3. **npm start** - 启动生产服务器
   - 运行: react-router-serve ./build/server/index.js
   - 端口: 3000（默认）

4. **npm run typecheck** - TypeScript 类型检查
   - 检查: app 目录中的所有 .ts/.tsx 文件

5. **npm run preview** - 预览生产构建
   - 本地测试生产版本

6. **npm run lint** - 代码质量检查
   - 可选：如果配置了 ESLint
```

### 验证步骤 | Verification Steps

```bash
# 1. 验证所有脚本都能正常运行
npm run typecheck
npm run build
npm start

# 2. 验证开发服务器
npm run dev

# 3. 构建 Docker 镜像
docker build -t ai-resume-analyzer .
docker run -p 3000:3000 ai-resume-analyzer
```

---

## 提示词 4.6: 创建 Docker Compose 配置 | Create Docker Compose Configuration

### 目标 | Goal
为本地开发创建 Docker Compose 配置

### 中文提示词 | Chinese Prompt

```
请在项目根目录创建 docker-compose.yml：

version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

  # 可选：Nginx 反向代理（用于生产环境）
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - app
    restart: unless-stopped
```

### 使用方法 | Usage

```bash
# 启动服务
docker-compose up -d

# 停止服务
docker-compose down

# 查看日志
docker-compose logs -f app

# 重建镜像
docker-compose up --build
```

---

## 提示词 4.7: 创建 nginx.conf 配置 | Create Nginx Configuration

### 目标 | Goal
创建 Nginx 反向代理配置

### 中文提示词 | Chinese Prompt

```
请创建 nginx.conf 文件（用于生产部署）：

events {
    worker_connections 1024;
}

http {
    upstream app {
        server app:3000;
    }

    gzip on;
    gzip_types text/plain text/css text/javascript application/json application/javascript;
    gzip_min_length 1024;

    server {
        listen 80;
        server_name _;

        # 重定向 HTTP 到 HTTPS（生产环境）
        # return 301 https://$server_name$request_uri;

        # 代理请求到应用
        location / {
            proxy_pass http://app;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # WebSocket 支持
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";

            # 超时配置
            proxy_connect_timeout 60s;
            proxy_send_timeout 60s;
            proxy_read_timeout 60s;
        }

        # 静态资源缓存
        location ~* \\.(js|css|png|jpg|jpeg|gif|svg|woff|woff2|ttf|eot)$ {
            expires 30d;
            add_header Cache-Control "public, immutable";
        }

        # 禁止访问某些文件
        location ~ /\\. {
            deny all;
        }
    }
}
```

---

## 提示词 4.8: 生成部署检查清单 | Create Deployment Checklist

### 目标 | Goal
验证所有部署前的准备

### 中文提示词 | Chinese Prompt

```
请在项目根目录创建 DEPLOYMENT.md 文件，包含部署检查清单：

# 部署检查清单 | Deployment Checklist

## 本地验证 | Local Verification

- [ ] npm install 成功
- [ ] npm run typecheck 通过
- [ ] npm run build 成功
- [ ] npm run dev 可以启动
- [ ] npm start 可以启动生产服务器

## Docker 验证 | Docker Verification

- [ ] docker build -t ai-resume-analyzer . 成功
- [ ] docker run -p 3000:3000 ai-resume-analyzer 成功运行
- [ ] 访问 http://localhost:3000 正常
- [ ] 所有主要功能都能工作

## 代码检查 | Code Review

- [ ] 没有 console.log 调试代码
- [ ] 没有 TODO 注释（或已完成）
- [ ] 所有导入都已使用
- [ ] 没有 TypeScript 错误
- [ ] 环境变量配置正确

## 安全检查 | Security Review

- [ ] 敏感信息不在代码中
- [ ] .env 文件不在 Git 中
- [ ] API 密钥安全管理
- [ ] CORS 配置正确
- [ ] 认证检查实现

## 性能优化 | Performance

- [ ] 构建输出文件大小合理
- [ ] 代码分割已配置
- [ ] 图片已优化
- [ ] 缓存策略已配置

## 配置检查 | Configuration

- [ ] Dockerfile 多阶段构建已配置
- [ ] docker-compose.yml 已准备
- [ ] nginx.conf 已配置
- [ ] package.json 脚本已验证

## 部署环境 | Deployment Environment

- [ ] Node.js 版本兼容（>= 18）
- [ ] 磁盘空间充足
- [ ] 网络连接正常
- [ ] Puter.com 可访问

## 文档 | Documentation

- [ ] README.md 已更新
- [ ] API 文档已准备
- [ ] 环境变量文档已准备
- [ ] 故障排查指南已准备

## 部署后验证 | Post-Deployment Verification

- [ ] 应用正常启动
- [ ] 所有路由可访问
- [ ] 认证功能工作
- [ ] 文件上传功能工作
- [ ] AI 分析功能工作
- [ ] 日志正常输出
- [ ] 错误被正确处理
```

---

## 提示词 4.9: 部署到云平台 | Deploy to Cloud Platform

### 目标 | Goal
配置云平台部署（Vercel / Railway / Heroku）

### 中文提示词 | Chinese Prompt

```
### 部署到 Vercel (推荐用于 React Router)

1. 连接 GitHub 仓库到 Vercel
2. 配置构建命令:
   - Build Command: npm run build
   - Output Directory: build
3. 环境变量配置
4. 自动部署

### 部署到 Railway

1. 创建 railway.json:

{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "dockerfile",
    "dockerfile": "Dockerfile"
  },
  "deploy": {
    "startCommand": "npm start",
    "healthcheckPath": "/",
    "healthcheckTimeout": 30
  }
}

2. 连接 GitHub 仓库
3. 设置环境变量
4. 部署

### 部署到 Docker Hub

1. 创建 Docker Hub 账户
2. 构建镜像:
   docker build -t yourusername/ai-resume-analyzer:latest .

3. 推送镜像:
   docker login
   docker push yourusername/ai-resume-analyzer:latest

4. 在任何支持 Docker 的平台运行:
   docker pull yourusername/ai-resume-analyzer:latest
   docker run -p 3000:3000 yourusername/ai-resume-analyzer:latest
```

---

## 提示词 4.10: 生产监控和日志 | Production Monitoring

### 目标 | Goal
配置生产环境监控

### 中文提示词 | Chinese Prompt

```
### 添加日志和监控

在 app/lib/puter.ts 中添加监控:

const trackEvent = (event: string, data?: any) => {
  const timestamp = new Date().toISOString();
  const logEntry = { timestamp, event, data };

  // 控制台输出（开发环境）
  if (import.meta.env.DEV) {
    console.log(logEntry);
  }

  // 发送到监控服务（生产环境）
  if (import.meta.env.PROD) {
    // 可以发送到 Sentry, LogRocket, Datadog 等
    // fetch('/api/logs', { method: 'POST', body: JSON.stringify(logEntry) })
  }
};

### 推荐的监控服务

1. **Sentry**: 错误监控
   - https://sentry.io

2. **LogRocket**: 会话重放
   - https://logrocket.com

3. **Datadog**: APM 监控
   - https://datadoghq.com

4. **Vercel Analytics**: 性能监控
   - https://vercel.com/analytics
```

---

## 完整部署流程 | Complete Deployment Process

### 步骤 1: 本地验证 | Step 1: Local Verification

```bash
npm install
npm run typecheck
npm run build
npm start
# 访问 http://localhost:3000
```

### 步骤 2: Docker 验证 | Step 2: Docker Verification

```bash
docker build -t ai-resume-analyzer .
docker run -p 3000:3000 ai-resume-analyzer
# 访问 http://localhost:3000
```

### 步骤 3: 推送到代码仓库 | Step 3: Push to Repository

```bash
git add .
git commit -m "Ready for deployment"
git push origin main
```

### 步骤 4: 部署到云平台 | Step 4: Deploy to Cloud

选择你喜欢的平台并按照相应的说明部署。

### 步骤 5: 验证生产环境 | Step 5: Verify Production

- 检查应用是否正常运行
- 验证所有功能是否工作
- 检查日志是否输出正常
- 测试 Puter.js 集成

### 步骤 6: 监控和维护 | Step 6: Monitoring & Maintenance

- 设置错误告警
- 定期检查日志
- 监控应用性能
- 及时修复问题

---

## 故障排查 | Troubleshooting

### 问题 1: Docker 构建失败

**原因**: 依赖安装失败
**解决**:
```bash
npm ci --verbose  # 查看详细日志
docker build -t ai-resume-analyzer . --no-cache
```

### 问题 2: Puter.js 加载失败

**原因**: CDN 不可用或网络问题
**解决**:
- 检查网络连接
- 验证 Puter.com 可访问
- 检查浏览器控制台错误

### 问题 3: 文件上传失败

**原因**: 权限问题或 Puter 账户问题
**解决**:
- 检查用户是否登录
- 验证 Puter 账户有效
- 查看浏览器控制台错误

### 问题 4: AI 分析失败

**原因**: Puter AI API 故障或余额不足
**解决**:
- 检查 Puter 账户余额
- 验证 API 密钥有效
- 查看 Puter 服务状态

---

本文档完成了项目的集成和部署配置。现在你已经拥有了完整的项目重建提示词！

**恭喜！ | Congratulations!**

你现在拥有了完整的项目重建提示词包，可以用来：
1. 从零开始重建项目
2. 教学和学习
3. 创建相似的项目
4. 与团队成员共享
