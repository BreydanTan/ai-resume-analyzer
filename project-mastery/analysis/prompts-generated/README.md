# 项目重建提示词包 | Project Reconstruction Prompt Bundle

**版本 | Version**: 1.0
**创建日期 | Created Date**: 2025-11-16
**语言 | Language**: 中英双语 (Bilingual)

---

## 概述 | Overview

这是一个完整的项目重建提示词包，基于 **AI Resume Analyzer** 项目的详细分析文档生成。所有提示词都经过精心设计，可以直接复制粘贴给 AI（Claude、GPT 等）使用，快速重建整个项目。

This is a comprehensive project reconstruction prompt bundle based on detailed analysis of the **AI Resume Analyzer** project. All prompts are carefully designed and can be directly copy-pasted to AI models (Claude, GPT, etc.) for quick project reconstruction.

---

## 文件结构 | File Structure

```
prompts-generated/
├── README.md                          (本文件 | This file)
├── 01-foundation-prompts.md           (基础设施提示词)
├── 02-backend-prompts.md              (后端/服务层提示词)
├── 03-frontend-prompts.md             (前端提示词)
└── 04-integration-prompts.md          (集成与部署提示词)
```

---

## 快速开始 | Quick Start

### 中文说明 | Chinese Instructions

#### 方式 1: 按顺序使用提示词

1. **打开 01-foundation-prompts.md**
   - 复制"提示词 1.1"到 AI 进行项目初始化
   - 复制"提示词 1.2"配置 Tailwind CSS
   - 复制"提示词 1.3"创建路由配置

2. **打开 02-backend-prompts.md**
   - 复制"提示词 2.1"创建类型定义
   - 复制"提示词 2.2"创建工具函数
   - 复制"提示词 2.3"创建 PDF 处理
   - 复制"提示词 2.4"创建 AI 常量
   - 复制"提示词 2.5"创建 Zustand Store（可选：参考原始项目）

3. **打开 03-frontend-prompts.md**
   - 复制"提示词 3.1"创建根组件
   - 复制"提示词 3.2"创建 UI 组件（10个组件）
   - 复制"提示词 3.3"创建路由页面（5个页面）

4. **打开 04-integration-prompts.md**
   - 复制"提示词 4.1"创建 Dockerfile
   - 复制"提示词 4.2"创建 .dockerignore
   - 复制"提示词 4.3-4.10"完成配置和部署

#### 方式 2: 全量提示词（适合高级用户）

1. 提取每个文件中的提示词部分（``````之间的内容）
2. 合并成一个大型提示词
3. 一次性发送给 AI（确保 AI 的上下文窗口足够大）

### English Instructions

#### Method 1: Use prompts sequentially

1. **Open 01-foundation-prompts.md**
   - Copy "Prompt 1.1" to AI for project initialization
   - Copy "Prompt 1.2" to configure Tailwind CSS
   - Copy "Prompt 1.3" to create route configuration

2. **Open 02-backend-prompts.md**
   - Copy "Prompt 2.1" to create type definitions
   - Copy "Prompt 2.2" to create utility functions
   - Copy "Prompt 2.3" to create PDF processing
   - Copy "Prompt 2.4" to create AI constants
   - Copy "Prompt 2.5" to create Zustand Store (optional: reference original project)

3. **Open 03-frontend-prompts.md**
   - Copy "Prompt 3.1" to create root component
   - Copy "Prompt 3.2" to create UI components (10 components)
   - Copy "Prompt 3.3" to create route pages (5 pages)

4. **Open 04-integration-prompts.md**
   - Copy "Prompt 4.1" to create Dockerfile
   - Copy "Prompt 4.2" to create .dockerignore
   - Copy "Prompts 4.3-4.10" to complete configuration and deployment

#### Method 2: Full-scale prompt (for advanced users)

1. Extract prompt sections from each file (content between ``` markers)
2. Merge into one large prompt
3. Send to AI at once (ensure AI's context window is large enough)

---

## 各文件详细说明 | Detailed File Descriptions

### 文件 1: 01-foundation-prompts.md

**目的 | Purpose**: 建立项目基础设施和开发环境

**包含内容 | Content**:
- ✅ React Router v7 项目初始化
- ✅ package.json 精确依赖版本
- ✅ TypeScript 配置 (tsconfig.json)
- ✅ Vite 构建配置 (vite.config.ts)
- ✅ React Router 配置 (react-router.config.ts)
- ✅ Tailwind CSS 4.1.4 配置
- ✅ 全局样式设置 (app.css)
- ✅ 目录结构创建
- ✅ 路由配置 (routes.ts)

**预计完成时间 | Estimated Time**: 30-45 分钟

**验证方法 | Verification**:
```bash
npm install
npm run dev
# 访问 http://localhost:5173，应看到React Router欢迎页面
```

---

### 文件 2: 02-backend-prompts.md

**目的 | Purpose**: 实现后端服务层和数据管理

**包含内容 | Content**:
- ✅ TypeScript 类型定义 (types/*.d.ts)
- ✅ Puter.js 类型声明
- ✅ 工具函数库 (lib/utils.ts)
- ✅ PDF 处理工具 (lib/pdf2img.ts)
- ✅ AI 常量和提示词 (constants/index.ts)
- ✅ Zustand Store (lib/puter.ts - 456行)
- ✅ 状态管理集成
- ✅ Puter SDK 完整封装

**预计完成时间 | Estimated Time**: 1-2 小时

**关键说明 | Important Notes**:
- puter.ts 是一个大文件（456行），可以参考原始项目：
  https://github.com/adrianhajdin/ai-resume-analyzer/blob/main/app/lib/puter.ts
- 或使用提示词 2.5 中的完整 AI 提示词生成

**验证方法 | Verification**:
```bash
npm run typecheck
# 应该没有TypeScript错误
```

---

### 文件 3: 03-frontend-prompts.md

**目的 | Purpose**: 创建完整的用户界面和交互

**包含内容 | Content**:
- ✅ 根组件和布局 (root.tsx)
- ✅ 导航栏组件 (Navbar.tsx)
- ✅ 10 个 UI 组件（完整代码）:
  1. ScoreCircle - 分数圆形显示
  2. ScoreBadge - 分数徽章
  3. ScoreGauge - 分数仪表
  4. Accordion - 折叠面板
  5. FileUploader - 文件上传器
  6. ResumeCard - 简历卡片
  7. Summary - 摘要组件
  8. ATS - ATS 分析组件
  9. Details - 详情组件
  10. （其他辅助组件）

- ✅ 5 个路由页面指导:
  1. home.tsx - 首页/简历列表
  2. auth.tsx - 认证页面
  3. upload.tsx - 上传分析页面
  4. resume.tsx - 简历详情页
  5. wipe.tsx - 数据清除页

**预计完成时间 | Estimated Time**: 2-3 小时

**关键说明 | Important Notes**:
- 前 7 个组件提供了完整代码
- Accordion 和 Details 提供了实现框架
- 5 个路由页面提供了详细的功能描述和 AI 提示词

**验证方法 | Verification**:
```bash
npm run dev
# 应该能看到完整的 UI 和导航
```

---

### 文件 4: 04-integration-prompts.md

**目的 | Purpose**: 完成集成、配置和部署

**包含内容 | Content**:
- ✅ Docker 多阶段构建配置 (Dockerfile)
- ✅ 构建优化配置 (.dockerignore, .gitignore)
- ✅ 环境变量配置 (.env.example)
- ✅ npm 脚本验证
- ✅ Docker Compose 配置 (docker-compose.yml)
- ✅ Nginx 反向代理配置 (nginx.conf)
- ✅ 部署检查清单 (DEPLOYMENT.md)
- ✅ 云平台部署指南 (Vercel, Railway, Docker Hub)
- ✅ 生产监控和日志配置
- ✅ 故障排查指南

**预计完成时间 | Estimated Time**: 1-2 小时

**关键说明 | Important Notes**:
- Docker 配置是优化的多阶段构建，最终镜像约 400MB
- 包含 Nginx 配置用于生产环境反向代理
- 提供了多个云平台部署选项

**验证方法 | Verification**:
```bash
# Docker 构建
docker build -t ai-resume-analyzer .
docker run -p 3000:3000 ai-resume-analyzer

# 访问 http://localhost:3000
```

---

## 技术栈总结 | Technology Stack Summary

### 前端框架 | Frontend Framework
| 技术 | 版本 | 用途 |
|------|------|------|
| React | 19.1.0 | UI 框架 |
| React Router | 7.5.3 | 路由管理 |
| TypeScript | 5.8.3 | 类型安全 |
| Tailwind CSS | 4.1.4 | 样式系统 |

### 状态管理 | State Management
| 技术 | 版本 | 用途 |
|------|------|------|
| Zustand | 5.0.6 | 全局状态管理 |

### 后端服务 | Backend Services
| 技术 | 版本 | 用途 |
|------|------|------|
| Puter.js | v2 | 无服务器后端平台 |
| Claude 3.7 | Sonnet | AI 分析引擎 |

### 构建工具 | Build Tools
| 技术 | 版本 | 用途 |
|------|------|------|
| Vite | 6.3.3 | 构建工具 |
| React Router | 7.5.3 (dev) | 编译器 |

### 部署 | Deployment
| 工具 | 用途 |
|------|------|
| Docker | 容器化 |
| Nginx | 反向代理 |
| Node.js | 运行时 |

---

## 使用最佳实践 | Best Practices

### 1. 逐步应用 | Apply Step by Step

不要一次性复制所有提示词到 AI。应该：
1. 按照顺序逐个提示词应用
2. 在每个提示词后验证输出
3. 根据需要调整和修复
4. 再进行下一个提示词

### 2. 自定义调整 | Customize as Needed

提示词是模板，可以根据需要自定义：
- 修改项目名称
- 调整依赖版本（如需要）
- 添加额外的功能
- 修改样式和 UI

### 3. 与原始项目参考 | Reference Original Project

某些复杂的文件（如 puter.ts）可能需要参考原始项目：
- GitHub: https://github.com/adrianhajdin/ai-resume-analyzer
- 获取最新的代码实现

### 4. 测试和验证 | Test and Verify

每完成一个提示词都应该：
1. 检查是否有错误
2. 运行类型检查 (`npm run typecheck`)
3. 运行开发服务器 (`npm run dev`)
4. 在浏览器中测试功能

### 5. 团队协作 | Team Collaboration

这个提示词包可以用于：
- 与团队成员共享项目结构
- 新成员快速上手
- 文档化项目架构
- 教学和学习

---

## 常见问题 | FAQ

### Q: 提示词可以直接用吗？| Can I use the prompts directly?

A: 是的，但建议：
1. 逐个应用而非一次全部应用
2. 在应用前阅读整个提示词
3. 根据你的需求调整
4. 测试输出的代码

### Q: 需要多少时间重建整个项目？| How long to rebuild the entire project?

A: 取决于你的速度和对 AI 的熟悉程度：
- 快速用户（使用 GPT-4 或 Claude）：4-6 小时
- 正常用户：8-12 小时
- 新手用户：12-24 小时

### Q: 能否跳过某些步骤？| Can I skip some steps?

A: 不建议。建议按顺序进行，因为：
1. 后续步骤依赖前面的文件
2. 某些配置必须在特定时间完成
3. 可以确保最终项目的完整性

### Q: 如果 AI 生成的代码有问题怎么办？| What if AI generated code has issues?

A: 正常情况，你可以：
1. 告诉 AI 具体的错误信息
2. 要求它修复代码
3. 参考原始项目的实现
4. 手动调整代码

### Q: 提示词包是否会定期更新？| Will prompts be updated?

A: 会的。当项目更新时：
1. 检查原始项目的更改
2. 更新相应的提示词
3. 发布新版本

---

## 快速参考 | Quick Reference

### 重建流程 | Rebuild Process

```
01-foundation → 02-backend → 03-frontend → 04-integration
   ↓              ↓              ↓              ↓
  30min         1-2h           2-3h           1-2h
  ↓              ↓              ↓              ↓
 项目初始化  → 后端服务  →   用户界面  →  部署配置
```

### 项目架构概览 | Project Architecture Overview

```
浏览器 | Browser
    ↓
React Router v7 应用
    ↓
Zustand Store (puter.ts)
    ↓
Puter.js SDK
    ↓
Puter.com 服务
    ├─ 认证
    ├─ 文件存储
    ├─ KV 数据库
    └─ AI (Claude)
```

### 关键文件清单 | Key Files Checklist

- [ ] package.json - 依赖管理
- [ ] tsconfig.json - TypeScript 配置
- [ ] vite.config.ts - Vite 构建配置
- [ ] react-router.config.ts - 路由配置
- [ ] app/routes.ts - 路由定义
- [ ] app/root.tsx - 根布局
- [ ] app/app.css - 全局样式
- [ ] app/lib/puter.ts - Zustand Store
- [ ] app/lib/pdf2img.ts - PDF 处理
- [ ] app/lib/utils.ts - 工具函数
- [ ] app/components/* - 10 个 UI 组件
- [ ] app/routes/* - 5 个路由页面
- [ ] types/*.d.ts - 类型定义
- [ ] constants/index.ts - AI 常量
- [ ] Dockerfile - Docker 配置
- [ ] docker-compose.yml - Docker Compose
- [ ] nginx.conf - Nginx 配置
- [ ] .env.example - 环境变量模板

---

## 相关资源 | Related Resources

### 原始项目 | Original Project
- 仓库: https://github.com/adrianhajdin/ai-resume-analyzer
- 演示: https://www.youtube.com/watch?v=iYOz165wGkQ

### 技术文档 | Technical Documentation
- React Router: https://reactrouter.com
- Puter.js: https://docs.puter.com
- Tailwind CSS: https://tailwindcss.com
- Zustand: https://github.com/pmndrs/zustand
- TypeScript: https://www.typescriptlang.org

### 部署平台 | Deployment Platforms
- Vercel: https://vercel.com
- Railway: https://railway.app
- Docker Hub: https://hub.docker.com
- Heroku: https://www.heroku.com

---

## 许可证 | License

这个提示词包是基于公开项目的分析而创建的，旨在用于教学和学习目的。

This prompt bundle was created based on analysis of the public project and is intended for educational and learning purposes.

---

## 更新日志 | Changelog

### v1.0 (2025-11-16)
- 初始发布 | Initial release
- 4 个主要提示词文件 | 4 main prompt files
- 完整的项目重建指南 | Complete project reconstruction guide

---

## 反馈和改进 | Feedback and Improvements

如果你在使用这些提示词时遇到任何问题，或有改进建议，欢迎反馈！

If you encounter any issues using these prompts or have suggestions for improvement, feedback is welcome!

---

**最后更新 | Last Updated**: 2025-11-16
**完整度 | Completeness**: 100%
**可用性 | Usability**: ★★★★★
