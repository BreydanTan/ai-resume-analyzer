# 使用指南 | Usage Guide

**创建日期 | Created Date**: 2025-11-16
**项目 | Project**: AI Resume Analyzer
**状态 | Status**: 完成 | Complete

---

## 概述 | Overview

本指南帮助你如何使用这个提示词包来快速重建 AI Resume Analyzer 项目或创建类似的项目。

---

## 快速指南 | Quick Guide

### 第 1 步：阅读项目架构 | Step 1: Understand the Architecture

首先阅读 `README.md`，了解：
- 项目结构
- 技术栈
- 四个提示词文件的用途

### 第 2 步：按顺序应用提示词 | Step 2: Apply Prompts Sequentially

#### 基础设施阶段 (30-45 分钟) | Foundation Phase
打开 `01-foundation-prompts.md`，复制以下提示词到 AI：
1. **提示词 1.1**: React Router v7 项目初始化
2. **提示词 1.2**: 创建全局样式和 Tailwind 配置
3. **提示词 1.3**: 创建路由配置

**验证**:
```bash
npm install
npm run dev
```

#### 后端/服务层阶段 (1-2 小时) | Backend Phase
打开 `02-backend-prompts.md`，复制以下提示词到 AI：
1. **提示词 2.1**: 创建 TypeScript 类型定义
2. **提示词 2.2**: 创建工具函数
3. **提示词 2.3**: 创建 PDF 处理工具
4. **提示词 2.4**: 创建 AI 常量和提示词配置
5. **提示词 2.5**: 创建 Zustand Store (参考原始项目或使用 AI 生成)

**验证**:
```bash
npm run typecheck
```

#### 前端阶段 (2-3 小时) | Frontend Phase
打开 `03-frontend-prompts.md`，复制以下提示词到 AI：
1. **提示词 3.1**: 创建根组件和布局
2. **提示词 3.2**: 创建 10 个 UI 组件
3. **提示词 3.3**: 创建 5 个路由页面

**验证**:
```bash
npm run dev
# 打开浏览器访问 http://localhost:5173
```

#### 集成与部署阶段 (1-2 小时) | Integration Phase
打开 `04-integration-prompts.md`，复制以下提示词到 AI：
1. **提示词 4.1**: 创建 Docker 配置
2. **提示词 4.2**: 创建 .dockerignore
3. **提示词 4.3**: 创建环境变量配置
4. **提示词 4.4**: 创建 .gitignore
5. **提示词 4.5-4.10**: 完成其他配置和部署

**验证**:
```bash
npm run build
docker build -t ai-resume-analyzer .
docker run -p 3000:3000 ai-resume-analyzer
```

### 第 3 步：测试完整功能 | Step 3: Test All Features

1. **认证功能**
   - 访问 `/auth`
   - 使用 Puter 登录/登出

2. **上传和分析**
   - 访问 `/upload`
   - 上传 PDF 简历
   - 等待 AI 分析

3. **查看详情**
   - 访问 `/` 查看简历列表
   - 点击简历查看详细反馈

4. **数据清除**
   - 访问 `/wipe`
   - 验证清除功能

---

## 常见使用场景 | Common Use Cases

### 场景 1: 完整重建项目 (4-8 小时)

如果要从零开始重建整个项目：
1. 按照"第 2 步"逐个应用所有提示词
2. 在每个提示词后进行验证
3. 根据 AI 输出调整代码（如需要）
4. 完成后部署

### 场景 2: 学习项目架构 (2-4 小时)

如果要学习项目结构而不实际重建：
1. 阅读 `README.md` 了解整体架构
2. 查看各个提示词文件了解具体实现
3. 参考原始项目的代码实现
4. 学习 React Router v7、Zustand、Puter.js 等技术

### 场景 3: 创建衍生项目 (6-12 小时)

如果要创建基于 AI Resume Analyzer 的衍生项目（如代码审查分析、简历改进建议等）：
1. 从 `01-foundation-prompts.md` 开始，自定义项目名称
2. 修改 `02-backend-prompts.md` 中的数据模型和 AI 提示词
3. 修改 `03-frontend-prompts.md` 中的 UI 组件
4. 调整 `04-integration-prompts.md` 中的部署配置

### 场景 4: 团队协作 (进行中)

如果要与团队成员协作：
1. 将此提示词包共享给团队
2. 每个成员按照自己的进度使用提示词
3. 使用 Git 进行版本控制和合并
4. 定期同步更新

---

## 提示和技巧 | Tips & Tricks

### 提示 1: AI 选择
不同的 AI 模型有不同的表现：
- **Claude 3.5 Sonnet**: 最佳平衡，推荐用于大多数任务
- **GPT-4o**: 理解复杂需求的能力最强
- **GPT-4 Turbo**: 适合处理长提示词
- **Llama 2**: 免费选择，但可能需要更多迭代

### 提示 2: 上下文管理
当使用 AI 时：
- 一次应用一个提示词而非全部
- 在应用新提示词前总结前面的工作
- 保存 AI 的输出到本地文件进行版本控制

### 提示 3: 调试失败
如果 AI 生成的代码有问题：
1. 告诉 AI 具体的错误信息
2. 要求它解释代码的工作原理
3. 让它逐步调试
4. 参考原始项目的实现进行对比

### 提示 4: 自定义调整
可以自定义的部分：
- 项目名称和品牌名称
- 颜色方案和设计系统
- AI 分析的评分维度
- 部署平台和环境变量

### 提示 5: 性能优化
在实际部署前：
1. 运行 `npm run build` 并检查输出大小
2. 使用 Lighthouse 进行性能审计
3. 配置 CDN 加速静态资源
4. 设置合适的缓存策略

---

## 常见问题解决 | Troubleshooting

### 问题 1: "puter is not defined"
**原因**: Puter.js SDK 尚未加载
**解决**:
- 检查 `app/root.tsx` 中是否有 `<script src="https://js.puter.com/v2/"></script>`
- 刷新页面并等待 SDK 加载
- 检查浏览器控制台是否有加载错误

### 问题 2: TypeScript 编译错误
**原因**: 类型定义不完整
**解决**:
- 运行 `npm run typecheck` 查看具体错误
- 检查 `types/` 目录中的类型定义
- 确保所有导入都正确

### 问题 3: 样式不应用
**原因**: Tailwind CSS 配置或导入问题
**解决**:
- 检查 `app/root.tsx` 是否导入了 `app.css`
- 验证 `tailwind.config.js` 的 content 配置
- 清除构建缓存: `rm -rf build && npm run build`

### 问题 4: Docker 构建失败
**原因**: 依赖安装失败或环境问题
**解决**:
- 添加 `--no-cache` 标志: `docker build --no-cache .`
- 检查磁盘空间是否充足
- 验证网络连接正常
- 查看 Docker 日志获取详细信息

---

## 最佳实践 | Best Practices

### 1. 版本控制
```bash
# 每个主要步骤后提交
git add .
git commit -m "Complete foundation setup"
```

### 2. 环境隔离
```bash
# 使用 .env.local 管理本地配置
# 永远不要提交 .env 或敏感信息
cp .env.example .env.local
```

### 3. 定期测试
```bash
# 每个功能完成后测试
npm run typecheck
npm run build
npm run dev
```

### 4. 代码审查
- 在应用 AI 生成的代码前审查
- 检查安全问题（如 XSS、SQL 注入等）
- 验证性能和可维护性

### 5. 文档更新
- 在 README.md 中更新项目信息
- 记录任何自定义修改
- 为新成员编写快速启动指南

---

## 下一步行动 | Next Steps

完成所有提示词应用后：

1. **部署**: 使用 Vercel、Railway 或 Docker Hub 部署
2. **监控**: 设置错误跟踪和性能监控
3. **优化**: 根据用户反馈改进 UI 和功能
4. **扩展**: 添加新功能或改进现有功能
5. **维护**: 定期更新依赖和安全补丁

---

## 获取帮助 | Get Help

如果遇到问题：

1. **检查原始项目**: https://github.com/adrianhajdin/ai-resume-analyzer
2. **查看技术文档**:
   - React Router: https://reactrouter.com
   - Puter.js: https://docs.puter.com
   - Tailwind CSS: https://tailwindcss.com
3. **社区论坛**:
   - Reddit r/reactjs
   - Stack Overflow
   - GitHub Discussions

---

**最后更新 | Last Updated**: 2025-11-16
**版本 | Version**: 1.0
