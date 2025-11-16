# 第2阶段：理解结构 | Skeleton Stage

## 🎯 阶段目标：地图在我脑子里了！

**时间**: 3-5天 | **每天时间**: 2-3小时 | **难度**: ⭐⭐ 简单

> 在这一阶段，你将理解项目的"骨架"。知道文件在哪、怎么组织的、各部分做什么。

---

## 📋 前置检查

完成了第1阶段了吗？检查：
- [ ] 项目能在 `http://localhost:5173` 运行
- [ ] 能修改代码看到浏览器自动更新
- [ ] `npm run dev` 命令你能用

---

## 🗂️ 任务1: 理解目录结构

### 目标
用20分钟内画出项目的"地图"。

### 步骤1: 打开文件树

在VS Code中，看左边的文件树：

```
ai-resume-analyzer/
├── app/                    ← 主文件夹
│   ├── routes/            ← 页面
│   ├── components/        ← 组件
│   ├── lib/               ← 工具
│   ├── root.tsx           ← 主容器
│   ├── app.css            ← 样式
│   └── routes.ts          ← 路由配置
├── constants/             ← 常数
├── types/                 ← 类型定义
├── public/                ← 静态资源
├── package.json           ← 项目配置
├── vite.config.ts         ← 构建配置
├── tsconfig.json          ← TypeScript配置
├── Dockerfile             ← 容器配置
└── README.md              ← 说明文档
```

### 步骤2: 探索 app/ 文件夹

这是最重要的！双击展开 `app/`：

#### routes/ (页面)
```
routes/
├── home.tsx      ← 首页 (/)
├── auth.tsx      ← 登录页 (/auth)
├── upload.tsx    ← 上传页 (/upload)
├── resume.tsx    ← 详情页 (/resume/:id)
└── wipe.tsx      ← 清除数据页 (/wipe)
```

**它们分别对应什么？**
打开 `routes.ts` 看一下：

```typescript
export default [
  index("routes/home.tsx"),           // / → home
  route("/auth", "routes/auth.tsx"),  // /auth → auth
  // 等等
]
```

#### components/ (组件)
```
components/
├── Navbar.tsx         ← 顶部导航栏
├── ResumeCard.tsx     ← 简历卡片
├── ScoreCircle.tsx    ← 分数圆圈
├── ScoreBadge.tsx     ← 分数徽章
├── Summary.tsx        ← 摘要区域
├── Details.tsx        ← 详细分数
├── ATS.tsx            ← ATS分析
├── FileUploader.tsx   ← 文件上传器
├── Accordion.tsx      ← 可折叠面板
└── ... 更多
```

#### lib/ (工具)
```
lib/
├── puter.ts    ← 最重要！云服务包装 (456行)
├── pdf2img.ts  ← PDF转图片工具
└── utils.ts    ← 通用工具函数
```

### 步骤3: 记忆模式

记住这个模式：
```
一个页面 (如 home.tsx)
  ↓ 使用
多个组件 (如 ResumeCard, Navbar)
  ↓ 调用
工具函数 (如 puter.ts 中的函数)
```

### 任务完成标志

- [ ] 知道有5个页面 (home, auth, upload, resume, wipe)
- [ ] 知道有9个以上的组件
- [ ] 知道 lib/ 里放工具函数
- [ ] 能指出一个文件在项目中的位置

---

## 🔎 任务2: 找关键文件

### 任务目标
学会找"这个功能在哪个文件里"。

### 步骤1: 开一个页面 (home.tsx)

在 `app/routes/home.tsx` 右键 → "查看定义" 或直接打开。

你看到的应该是这样的结构：
```typescript
export const meta = () => [{ title: "Home" }]

export default function Home() {
  // ... 逻辑代码
  return (
    <div>
      {/* 显示简历列表 */}
    </div>
  )
}
```

**这个页面做什么?**
- 显示用户上传过的所有简历
- 每个简历显示为一张卡片
- 点卡片能看详情

### 步骤2: 开一个组件 (ResumeCard.tsx)

打开 `app/components/ResumeCard.tsx`。

你会看到：
```typescript
interface ResumeCardProps {
  resume: Resume
  // ... 其他props
}

export function ResumeCard({ resume }: ResumeCardProps) {
  return (
    <div className="...">
      {/* 卡片内容 */}
    </div>
  )
}
```

**这个组件做什么?**
- 接收一份简历的数据
- 显示为漂亮的卡片
- 包括图片、名称、分数

### 步骤3: 开工具 (puter.ts)

打开 `app/lib/puter.ts`。这是一个大文件(456行)！

看最开始的部分：
```typescript
export const usePuterStore = create<PuterStore>((set) => ({
  auth: {
    isAuthenticated: false,
    user: null,
    signIn: async () => { ... },
    getUser: async () => { ... }
  },
  fs: {
    upload: async (files) => { ... },
    read: async (path) => { ... }
  },
  kv: {
    get: async (key) => { ... },
    set: async (key, value) => { ... }
  },
  ai: {
    feedback: async (path, instructions) => { ... }
  }
}))
```

**这个文件做什么?**
- 包装 Puter.js 的所有功能
- 提供给其他组件使用
- 是项目的"大脑"

### 任务完成标志

- [ ] 知道 home.tsx 显示简历列表
- [ ] 知道 ResumeCard.tsx 显示单个卡片
- [ ] 知道 puter.ts 包含所有云服务操作
- [ ] 能找到任何一个文件的位置

---

## 📊 任务3: 理解路由系统

### 任务目标
理解"不同URL显示不同页面"的工作原理。

### 步骤1: 打开 routes.ts

在 `app/routes.ts` 中看：
```typescript
export default [
  index("routes/home.tsx"),
  route("/auth", "routes/auth.tsx"),
  route("/upload", "routes/upload.tsx"),
  route("/resume/:id", "routes/resume.tsx"),
  route("/wipe", "routes/wipe.tsx"),
]
```

### 步骤2: 理解映射关系

```
URL                        页面文件
/                    ←  → routes/home.tsx
/auth                ←  → routes/auth.tsx
/upload              ←  → routes/upload.tsx
/resume/abc123       ←  → routes/resume.tsx (id = abc123)
/wipe                ←  → routes/wipe.tsx
```

### 步骤3: 在浏览器验证

1. 打开 `http://localhost:5173/`
   - 看到首页

2. 在地址栏改为 `http://localhost:5173/auth`
   - 页面变了！
   - 现在显示 auth.tsx 的内容

3. 改为 `http://localhost:5173/upload`
   - 又变了！
   - 现在显示 upload.tsx 的内容

4. 改为 `http://localhost:5173/wipe`
   - 又变了！

**这就是路由的作用！**

### 任务完成标志

- [ ] 理解 URL ← → 页面 的对应关系
- [ ] 能通过改URL看到不同页面
- [ ] 知道 `:id` 是什么意思(占位符，可变部分)

---

## 📖 任务4: 阅读一个完整组件

### 任务目标
从头读完一个文件，理解它做什么。

### 步骤1: 选择目标文件

打开 `app/components/ResumeCard.tsx`。

### 步骤2: 从上到下读

```typescript
// 第1部分: 导入
import { Link } from "react-router-dom"
import { Resume } from "~/types"
// 这说明: 这个文件需要Link和Resume类型

// 第2部分: 定义接口
interface ResumeCardProps {
  resume: Resume
  onDelete?: () => void
}
// 这说明: 这个组件接收什么数据

// 第3部分: 组件定义
export function ResumeCard({ resume, onDelete }: ResumeCardProps) {
  return (
    <div className="...">
      {/* 显示简历信息 */}
      <img src={resume.imagePath} />
      <h3>{resume.companyName}</h3>
      {/* ... 更多HTML */}
    </div>
  )
}
// 这说明: 组件怎么显示
```

### 步骤3: 提出问题

读完后问自己：
- [ ] 这个组件接收什么 Props?
- [ ] 它显示什么内容?
- [ ] 它调用了哪些其他组件?
- [ ] 用户能做什么操作?

### 步骤4: 用AI验证理解

用这个提示词：
```
我在读这个React组件的代码 [粘贴代码]

能简单地解释一下这个组件做什么吗？
用日常语言，不要太技术性。
```

### 任务完成标志

- [ ] 能用自己的话解释这个文件
- [ ] 理解 interface/Props 的作用
- [ ] 理解组件返回的 JSX
- [ ] 知道这个组件在哪个页面被使用

---

## 🔗 任务5: 追踪数据流

### 任务目标
理解"简历数据"从哪里来，怎么流动的。

### 步骤1: 找起点

简历列表显示在哪个页面？`routes/home.tsx`

打开它，找这样的代码：
```typescript
// 从某个地方获取简历数据
const resumes = await getResumes()  // 或类似的代码

return (
  <div>
    {resumes.map(resume => (
      <ResumeCard resume={resume} key={resume.id} />
    ))}
  </div>
)
```

### 步骤2: 追踪 getResumes() 函数

搜索 getResumes() 是在哪里定义的。很可能在 `lib/puter.ts` 中。

### 步骤3: 在 puter.ts 中找它

```typescript
export const usePuterStore = create((set) => ({
  // ...
  getResumes: async () => {
    const items = await puter.kv.list('resume:*', true)
    // 从云端获取所有简历
    return items
  }
}))
```

### 步骤4: 理解完整流程

```
用户打开首页 (/)
  ↓
页面: home.tsx
  ↓
调用: getResumes()
  ↓
函数在: lib/puter.ts
  ↓
执行: puter.kv.list('resume:*')
  ↓
查询: 云端数据库
  ↓
返回: 所有简历数据
  ↓
显示: ResumeCard组件
  ↓
用户看到: 简历列表
```

### 任务完成标志

- [ ] 知道简历数据从哪里来
- [ ] 能画出"数据流"图
- [ ] 理解为什么要有 getResumes() 函数
- [ ] 知道 puter.kv.list() 做什么

---

## ✅ AI协作提示词

遇到问题？用这些：

### 我不懂这个文件的代码
```
我在 [文件路径] 中看到这段代码：

[粘贴代码块]

能用简单的语言解释一下这段代码做什么吗？
```

### 这个函数在哪里？
```
我找不到 [函数名] 函数的定义。
它应该在哪个文件里？

项目结构:
[描述你的文件结构]
```

### 这两个文件有什么关系？
```
[文件A] 和 [文件B] 有什么关系？
一个怎么使用另一个？

[粘贴两个文件的相关代码]
```

---

## 🎯 进阶挑战（可选）

### 挑战1: 画项目地图
用文字或图画描述整个项目结构。比如：
```
首页 (/)
  ↓
显示 ResumeCard 组件
  ↓
调用 getResumes() 从云端获取
  ↓
Puter.js 连接云平台
```

### 挑战2: 找出所有页面
列出所有5个页面，和它们各自的功能：
- [ ] home.tsx - ?
- [ ] auth.tsx - ?
- [ ] upload.tsx - ?
- [ ] resume.tsx - ?
- [ ] wipe.tsx - ?

### 挑战3: 列出主要组件
找出所有主要组件，和它们各自的作用：
- [ ] Navbar.tsx - ?
- [ ] ResumeCard.tsx - ?
- [ ] ...

### 挑战4: 修改路由
在 routes.ts 中看看，试着理解如果改了URL会怎样。

---

## 🎓 阶段总结

第2阶段学到了：

| 学习点 | 理解了什么 |
|--------|-----------|
| 文件结构 | app/, routes/, components/, lib/ 各自的作用 |
| 路由系统 | URL ← → 页面 的映射 |
| 组件概念 | 组件是可复用的UI块 |
| 数据流 | 数据怎么从云端流向页面 |
| 代码阅读 | 能自己打开文件看懂 |

---

## 📊 阶段自我评估

### 知识检查

| 问题 | 能答吗？ |
|------|----------|
| app/ 文件夹里有什么？ | ☐ 是 ☐ 否 |
| routes/ 文件夹做什么？ | ☐ 是 ☐ 否 |
| components/ 文件夹做什么？ | ☐ 是 ☐ 否 |
| lib/puter.ts 是什么？ | ☐ 是 ☐ 否 |
| 简历数据从哪里来？ | ☐ 是 ☐ 否 |
| routes.ts 做什么？ | ☐ 是 ☐ 否 |

### 实际操作检查

- [ ] 能找到任何一个文件
- [ ] 能打开并读懂一个组件
- [ ] 能通过改URL看到不同页面
- [ ] 能画出简单的数据流图
- [ ] 能用AI问出有意义的问题

---

## ➡️ 准备进入第3阶段

完成第2阶段后，你已经：
- ✅ 理解了文件结构
- ✅ 知道各部分做什么
- ✅ 能读懂代码文件

下一阶段，我们深入**数据库和存储**。准备好了吗？

👉 打开 **05-stage3-core-features.md** 继续！

---

## 🎉 成就已解锁

```
╔═══════════════════════════════════════╗
║ ✅ 成就: "项目地图掌握"              ║
║                                       ║
║ 你已经:                              ║
║ • 理解了文件结构                      ║
║ • 知道路由怎么工作                    ║
║ • 能读懂代码文件                      ║
║ • 理解了数据流动                      ║
║                                       ║
║ 从此，代码不再是黑魔法！              ║
╚═══════════════════════════════════════╝
```

记住：**理解结构是学编程的关键。** 你在正确的道路上！🚀
