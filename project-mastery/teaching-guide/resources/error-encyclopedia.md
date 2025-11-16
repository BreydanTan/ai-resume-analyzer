# 错误百科 | Error Encyclopedia

## 📖 使用本百科

当你看到错误信息时：
1. 复制错误信息的关键部分
2. 在这里查找
3. 看解决方法
4. 如果还是不行，使用推荐的AI求助提示词

---

## 错误1: "command not found: node"

### 示例错误信息
```
bash: node: command not found
或
'node' is not recognized as an internal or external command
```

### 含义
电脑找不到Node.js程序。

### 原因
1. Node.js没有安装
2. Node.js安装后没有重启电脑
3. 环境变量配置错误

### 解决方法

**Windows用户**:
1. 访问 https://nodejs.org
2. 下载LTS版本
3. 双击安装，一直点Next
4. **重启电脑**
5. 重新打开终端，输入 `node --version`

**Mac/Linux用户**:
```bash
# 检查是否安装
which node

# 如果没有，用包管理器安装
# Mac:
brew install node

# Linux:
sudo apt-get install nodejs npm
```

### 预防措施
- 安装后一定要重启电脑
- 使用官方安装程序，不要手动配置

### AI求助提示词
```
我在运行 `node --version` 时看到 "command not found" 错误。

我的操作系统: [Windows/Mac/Linux]
我已经从 nodejs.org 下载并安装了Node.js。

怎么修复？
```

---

## 错误2: "npm ERR! 404"

### 示例错误信息
```
npm ERR! 404 Not Found - GET https://registry.npmjs.org/[package-name]
```

### 含义
npm找不到这个包，或者网络有问题。

### 原因
1. 包名拼写错误
2. npm源配置错误
3. 网络连接问题
4. 包已经被删除

### 解决方法

```bash
# 1. 检查npm源
npm config get registry
# 应该是: https://registry.npmjs.org/

# 2. 清除npm缓存
npm cache clean --force

# 3. 尝试重新安装
npm install
```

### 预防措施
- 复制包名时小心拼写
- 用官方的npm源

### AI求助提示词
```
运行 `npm install` 时出现 404 错误：

[粘贴完整错误信息]

我想安装的是什么包？它真的存在吗？
```

---

## 错误3: "Port 5173 already in use"

### 示例错误信息
```
Error: listen EADDRINUSE: address already in use :::5173
```

### 含义
有其他程序在使用端口5173。

### 原因
1. Vite开发服务器已经在运行
2. 其他应用使用了这个端口
3. 上次的进程没有完全关闭

### 解决方法

**简单方式** - 使用不同的端口：
```bash
npm run dev -- --port 3000
```

**完整解决方式**:

Windows:
```bash
# 找出占用5173端口的进程
netstat -ano | findstr :5173

# 杀死该进程(记录上面的PID)
taskkill /PID [PID] /F
```

Mac/Linux:
```bash
# 找出占用5173端口的进程
lsof -i :5173

# 杀死该进程(记录上面的PID)
kill -9 [PID]
```

### 预防措施
- 完全关闭Vite服务器（Ctrl+C）再启动新的
- 避免多个Vite实例运行

---

## 错误4: "Module not found"

### 示例错误信息
```
Error: Cannot find module 'react'
或
Failed to resolve '@/components/Button'
```

### 含义
代码引入的模块找不到。

### 原因
1. 没有运行 `npm install`
2. 包写错了名字
3. 路径别名配置错误
4. 文件/文件夹不存在

### 解决方法

```bash
# 1. 确保安装了依赖
npm install

# 2. 检查 tsconfig.json 的路径别名配置
# 应该有这样的配置:
{
  "compilerOptions": {
    "paths": {
      "~/*": ["./app/*"]
    }
  }
}

# 3. 重启开发服务器
# Ctrl+C 停止
# npm run dev 重新启动
```

### 检查清单
- [ ] 运行过 `npm install` 吗？
- [ ] 包名拼写对了吗？
- [ ] 导入路径对吗？
- [ ] 文件确实存在吗？

### AI求助提示词
```
看到这个错误：

[粘贴错误信息]

错误代码:
```typescript
[粘贴导入语句]
```

能帮我诊断吗？
```

---

## 错误5: "Cannot read property 'xxx' of undefined"

### 示例错误信息
```
TypeError: Cannot read property 'feedback' of undefined
或
Cannot read properties of null (reading 'map')
```

### 含义
你试图访问一个不存在的对象的属性。

### 原因
1. 对象还没有加载完
2. 对象是null或undefined
3. 数据结构改变了
4. 异步操作未完成

### 解决方法

```typescript
// ❌ 不好的做法
const score = resume.feedback.overallScore  // 如果resume是undefined就crash

// ✅ 好的做法 - 检查是否存在
const score = resume?.feedback?.overallScore || 0

// ✅ 或者用if语句
if (resume && resume.feedback) {
  const score = resume.feedback.overallScore
}

// ✅ 或者等待加载完成
const [resume, setResume] = useState(null)
useEffect(() => {
  loadResume()  // 异步加载
}, [])

if (!resume) return <p>加载中...</p>  // 等待加载完成
```

### 预防措施
- 总是检查数据是否存在后再访问
- 使用可选链操作符 `?.`
- 提供默认值

---

## 错误6: "Unexpected token"

### 示例错误信息
```
SyntaxError: Unexpected token '}'
或
Unexpected token '<'
```

### 含义
代码的语法有错误。

### 常见原因
1. 缺少闭合括号 `{}` `()` `[]`
2. 分号位置错误
3. JSX标签没有正确关闭
4. 引号不匹配

### 解决方法

检查：
- [ ] 所有 `{` 都有对应的 `}`
- [ ] 所有 `(` 都有对应的 `)`
- [ ] 所有 `[` 都有对应的 `]`
- [ ] 所有 JSX `<Component>` 都有 `</Component>`
- [ ] 字符串引号匹配 `"..."` 或 `'...'`

### 示例修复

```typescript
// ❌ 错误：缺少 }
function MyComponent() {
  return (
    <div>Hello</div>
  )
  // 缺少这个 }

// ✅ 正确：
function MyComponent() {
  return (
    <div>Hello</div>
  )
}
```

---

## 错误7: "TypeError in puter.auth"

### 示例错误信息
```
Error: puter is not defined
或
Cannot read properties of undefined (reading 'auth')
```

### 含义
Puter.js SDK没有加载。

### 原因
1. Puter.js脚本加载失败
2. 网络问题
3. 浏览器拦截了脚本
4. 脚本加载时间太长

### 解决方法

1. **检查网络**：打开浏览器开发者工具 (F12)
   - 看Network标签
   - 查看 `https://js.puter.com/v2/` 是否加载成功

2. **等待脚本加载**：
```typescript
// 在使用 puter 前检查
if (typeof window.puter === 'undefined') {
  console.log('Puter还在加载...')
  return
}

// 或者等待加载完成
window.addEventListener('puter-init', () => {
  console.log('Puter已准备好')
})
```

3. **重新加载**：清除缓存后重新打开

---

## 错误8: "Puter authentication failed"

### 示例错误信息
```
Authentication failed: Invalid credentials
或
Error: Sign in was cancelled
```

### 含义
Puter登录失败。

### 原因
1. 账号或密码错误
2. 网络断了
3. Puter服务暂时不可用
4. 浏览器session超时

### 解决方法

1. 检查网络连接
2. 确保账号和密码正确
3. 清除浏览器Cookies：
   - F12打开开发者工具
   - Application → Cookies → 删除Puter相关的
4. 重新打开应用

---

## 错误9: "Cannot connect to localhost:5173"

### 示例错误信息
```
Connection refused
或
This site can't be reached
```

### 含义
浏览器无法连接到开发服务器。

### 原因
1. 开发服务器没有启动
2. 地址或端口错了
3. 防火墙阻止了连接
4. 服务器crash了

### 解决方法

1. **检查服务器是否运行**：
```bash
# 看到这样的输出说明正常:
VITE v6.3.3 dev server running at:
➜ Local: http://localhost:5173/
```

2. **检查防火墙**：
   - Windows防火墙允许Node.js
   - 杀毒软件不要拦截

3. **尝试其他端口**：
```bash
npm run dev -- --port 3000
# 然后访问 http://localhost:3000
```

---

## 错误10: "PDF upload failed"

### 示例错误信息
```
Error uploading file
或
File type not supported
```

### 含义
PDF上传失败。

### 原因
1. 文件不是真正的PDF
2. 文件太大
3. 网络中断
4. Puter文件系统故障

### 解决方法

1. **检查文件**：
   - 文件确实是PDF吗？
   - 文件大小多少MB？(通常限制在10-100MB)
   - 可以在其他地方打开吗？

2. **重新上传**：
   - 刷新页面
   - 选择相同的文件重新上传

3. **试试其他PDF**：
   - 用一个简单的小PDF测试
   - 确认功能是否正常

---

## 错误11: "AI analysis timeout"

### 示例错误信息
```
Timeout waiting for AI response
或
Request took too long
```

### 含义
AI分析花的时间太长。

### 原因
1. Claude AI响应慢
2. 网络慢
3. 简历太复杂
4. 服务过载

### 解决方法

1. **等待更长时间**：有时AI确实需要30秒
2. **简化简历**：非常长的简历可能导致超时
3. **检查网络**：确保网络连接稳定
4. **重试**：通常重试就能成功

---

## 错误12: "Data not saving"

### 示例错误信息
```
KV set failed
或
Save operation timed out
```

### 含义
数据无法保存到云端。

### 原因
1. 网络问题
2. 没有登录
3. Puter服务故障
4. 数据格式错误

### 解决方法

1. **检查登录**：
```javascript
const user = await puter.auth.getUser()
console.log('当前用户:', user)
```

2. **检查数据格式**：
```javascript
// 数据必须能被JSON序列化
const data = { name: "test" }
const json = JSON.stringify(data)  // 这能成功吗？
```

3. **检查网络**：打开浏览器Network标签，看请求是否成功

---

## 错误13: "Component rendering error"

### 示例错误信息
```
React error boundary caught an error
或
Uncaught Error in component
```

### 含义
React组件渲染时出错。

### 解决方法

1. **打开浏览器控制台** (F12) 看完整错误
2. **逐个检查**：
   - Props是否正确？
   - 状态是否初始化？
   - 数据是否存在？

3. **添加错误边界**：
```typescript
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

---

## 错误14: "Git conflicts"

### 示例错误信息
```
CONFLICT (content merge): [filename]
```

### 含义
多个分支改了同一个文件。

### 解决方法

```bash
# 1. 看冲突文件
git status

# 2. 打开文件，找到 <<<<<<< 标记
# 3. 手动选择要保留哪个版本
# 4. 删除冲突标记

# 5. 提交修复
git add .
git commit -m "解决冲突"
```

---

## 错误15: "Package version conflict"

### 示例错误信息
```
peer dep missing
或
conflicting peer dependencies
```

### 含义
不同的包需要不同版本的同一个库。

### 原因
1. 安装的包版本不兼容
2. package.json配置有问题

### 解决方法

```bash
# 1. 删除node_modules
rm -rf node_modules
# Windows: rmdir /s node_modules

# 2. 删除lock文件
rm package-lock.json
# Windows: del package-lock.json

# 3. 重新安装
npm install
```

---

## 快速诊断流程图

```
遇到错误
    ↓
看错误信息
    ↓
关键词是什么？
    ├─ "command not found" → 错误1
    ├─ "404" → 错误2
    ├─ "already in use" → 错误3
    ├─ "Module not found" → 错误4
    ├─ "Cannot read property" → 错误5
    ├─ "Unexpected token" → 错误6
    ├─ "puter is not defined" → 错误7
    ├─ "authentication failed" → 错误8
    ├─ "cannot connect" → 错误9
    ├─ "upload failed" → 错误10
    ├─ "timeout" → 错误11
    ├─ "not saving" → 错误12
    ├─ "rendering error" → 错误13
    ├─ "conflict" → 错误14
    └─ "peer dep" → 错误15
    ↓
按照解决方法尝试
    ↓
成功？ → 继续开发！
    ↓
失败？ → 使用AI求助
```

---

## 通用求助提示词

当上面的错误都找不到时，用这个：

```
我遇到了一个错误，不太确定原因。

错误信息:
[完整的错误信息]

我在做什么时出现的:
[具体操作]

我已经尝试过:
1. [方法1和结果]
2. [方法2和结果]

环境信息:
- Node: [版本]
- npm: [版本]
- OS: [系统]
- 浏览器: [浏览器]

能帮我诊断吗？
```

---

**记住**: 大多数错误都有解决方案。仔细读错误信息，通常里面有线索！🔍
