# 后端/服务层提示词 | Backend/Service Layer Prompts

**文档版本 | Document Version**: 1.0
**创建日期 | Created Date**: 2025-11-16
**语言 | Language**: 中英双语 (Bilingual)

---

## 提示词 2.1: 创建类型定义文件 | Create Type Definition Files

### 目标 | Goal
定义所有 TypeScript 类型和接口

### 中文提示词 | Chinese Prompt

```
请创建 TypeScript 类型定义文件。

### 步骤 1: 创建 types/index.d.ts

import type { ReactNode } from "react";

// 用户类型
export interface PuterUser {
  username: string;
  email?: string;
  [key: string]: any;
}

// 简历类型
export interface Resume {
  id: string;
  companyName: string;
  jobTitle: string;
  jobDescription?: string;
  resumePath: string;
  imagePath: string;
  feedback: Feedback;
}

// 反馈数据类型
export interface Feedback {
  overallScore: number;
  ATS: ScoreSection;
  toneAndStyle: ScoreSection;
  content: ScoreSection;
  structure: ScoreSection;
  skills: ScoreSection;
}

// 分项评分类型
export interface ScoreSection {
  score: number;
  tips: Tip[];
}

// 建议类型
export interface Tip {
  type: "good" | "improve";
  tip: string;
  explanation?: string;
}

// 文件系统项类型
export interface FSItem {
  id: string;
  name: string;
  path: string;
  type?: string;
  size?: number;
  [key: string]: any;
}

// KV 存储项类型
export interface KVItem {
  key: string;
  value: string;
}

// AI 响应类型
export interface AIResponse {
  message: {
    content: string | Array<{ text: string }>;
    role?: string;
  };
  [key: string]: any;
}

// 路由错误响应
export interface RouteErrorResponse {
  status: number;
  statusText: string;
  data?: any;
}

// React Router Route Props
export interface Route {
  loaderData?: any;
  params?: Record<string, string>;
  children?: ReactNode;
}

export type {};
```

### 步骤 2: 创建 types/puter.d.ts

declare global {
  interface Window {
    puter: PuterSDK;
  }
}

interface PuterSDK {
  auth: {
    signIn(): Promise<void>;
    signOut(): Promise<void>;
    isSignedIn(): Promise<boolean>;
    getUser(): Promise<PuterUser>;
  };
  fs: {
    upload(files: File[] | Blob[]): Promise<FSItem | undefined>;
    read(path: string): Promise<Blob | undefined>;
    write(path: string, data: string | File | Blob): Promise<File | undefined>;
    delete(path: string): Promise<void>;
    readdir(path: string): Promise<FSItem[] | undefined>;
  };
  kv: {
    get(key: string): Promise<string | null>;
    set(key: string, value: string): Promise<boolean>;
    delete(key: string): Promise<boolean>;
    list(pattern: string, returnValues?: boolean): Promise<any>;
    flush(): Promise<boolean>;
  };
  ai: {
    chat(
      messages: ChatMessage[],
      options?: AIOptions
    ): Promise<AIResponse | undefined>;
    img2txt(
      image: string | File | Blob,
      testMode?: boolean
    ): Promise<string | undefined>;
  };
}

interface ChatMessage {
  role: "user" | "assistant" | "system";
  content:
    | string
    | Array<{
        type: "text" | "file" | "image";
        text?: string;
        puter_path?: string;
        image_url?: string;
      }>;
}

interface AIOptions {
  model?: string;
  [key: string]: any;
}

interface PuterUser {
  username: string;
  email?: string;
  [key: string]: any;
}

interface FSItem {
  id: string;
  name: string;
  path: string;
  type?: string;
  size?: number;
}

export {};
```
```

### 验证步骤 | Verification Steps

1. **检查类型定义 | Check type definitions**:
   ```bash
   npm run typecheck
   ```

2. **验证导入 | Verify imports**:
   - 检查是否能从 types 目录导入类型
   - 运行 `npm run dev` 确保没有类型错误

### 预期结果 | Expected Results

- ✅ types/index.d.ts 创建成功
- ✅ types/puter.d.ts 创建成功
- ✅ TypeScript 类型检查无错误
- ✅ 所有类型都能正确导入

---

## 提示词 2.2: 创建工具函数库 | Create Utility Functions

### 目标 | Goal
创建 app/lib/utils.ts 工具函数文件

### 中文提示词 | Chinese Prompt

```
请在 app/lib/utils.ts 创建以下工具函数：

import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

/**
 * 合并 Tailwind CSS 类名
 * 解决类名冲突，后面的类会覆盖前面的
 * @param inputs - CSS 类名数组
 * @returns 合并后的类名字符串
 */
export const cn = (...inputs: ClassValue[]) => {
  return twMerge(clsx(inputs));
};

/**
 * 格式化文件大小为易读的字符串
 * @param bytes - 文件大小（字节）
 * @returns 格式化后的字符串，如 "1.5 MB"
 */
export const formatSize = (bytes: number): string => {
  if (bytes === 0) return "0 Bytes";

  const k = 1024;
  const sizes = ["Bytes", "KB", "MB", "GB", "TB"];
  const i = Math.floor(Math.log(bytes) / Math.log(k));

  return (
    parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + " " + sizes[i]
  );
};

/**
 * 生成 UUID v4
 * 使用 Web Crypto API 生成加密安全的 UUID
 * @returns UUID 字符串
 */
export const generateUUID = (): string => {
  return crypto.randomUUID();
};

/**
 * 延迟执行
 * @param ms - 延迟毫秒数
 * @returns Promise
 */
export const sleep = (ms: number): Promise<void> => {
  return new Promise((resolve) => setTimeout(resolve, ms));
};

/**
 * 获取查询参数
 * @param key - 参数名称
 * @returns 参数值或 null
 */
export const getQueryParam = (key: string): string | null => {
  const params = new URLSearchParams(window.location.search);
  return params.get(key);
};

/**
 * 复制文本到剪贴板
 * @param text - 要复制的文本
 * @returns Promise
 */
export const copyToClipboard = async (text: string): Promise<boolean> => {
  try {
    await navigator.clipboard.writeText(text);
    return true;
  } catch (err) {
    console.error("Failed to copy:", err);
    return false;
  }
};

/**
 * 格式化日期
 * @param date - 日期对象或时间戳
 * @returns 格式化后的日期字符串
 */
export const formatDate = (date: Date | number): string => {
  const d = date instanceof Date ? date : new Date(date);
  return d.toLocaleDateString("en-US", {
    year: "numeric",
    month: "long",
    day: "numeric",
  });
};

/**
 * 检测移动设备
 * @returns 是否为移动设备
 */
export const isMobileDevice = (): boolean => {
  return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
    navigator.userAgent
  );
};

/**
 * 验证电子邮件格式
 * @param email - 邮箱地址
 * @returns 是否有效
 */
export const validateEmail = (email: string): boolean => {
  const emailRegex = /^[^\\s@]+@[^\\s@]+\\.[^\\s@]+$/;
  return emailRegex.test(email);
};

/**
 * 防抖函数
 * @param func - 要执行的函数
 * @param delay - 延迟毫秒数
 * @returns 防抖后的函数
 */
export const debounce = <T extends (...args: any[]) => any>(
  func: T,
  delay: number
) => {
  let timeoutId: NodeJS.Timeout;

  return (...args: Parameters<T>) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func(...args), delay);
  };
};
```

### 验证步骤 | Verification Steps

1. **检查导出 | Check exports**:
   ```bash
   npm run typecheck
   ```

2. **测试函数 | Test functions**:
   - 确保每个函数都能正确导入
   - 检查类型定义

### 预期结果 | Expected Results

- ✅ utils.ts 文件创建成功
- ✅ 所有函数都能导入和使用
- ✅ TypeScript 类型检查通过

---

## 提示词 2.3: 创建 PDF 处理工具 | Create PDF Processing Utility

### 目标 | Goal
创建 app/lib/pdf2img.ts PDF 转图片工具

### 中文提示词 | Chinese Prompt

```
请在 app/lib/pdf2img.ts 创建 PDF 处理工具：

import type { pdfjs } from 'pdfjs-dist';

// 动态加载 PDF.js 库
let pdfjsLib: any = null;
let loadPromise: Promise<any> | null = null;

/**
 * 动态加载 PDF.js 库
 * 使用单例模式缓存库实例
 */
async function loadPdfJs(): Promise<any> {
  if (pdfjsLib) return pdfjsLib;
  if (loadPromise) return loadPromise;

  loadPromise = import('pdfjs-dist/build/pdf.mjs').then((module) => {
    pdfjsLib = module;
    // 设置 Web Worker 位置
    if (typeof pdfjsLib.GlobalWorkerOptions !== 'undefined') {
      pdfjsLib.GlobalWorkerOptions.workerSrc = '/pdf.worker.min.mjs';
    }
    return pdfjsLib;
  });

  return loadPromise;
}

/**
 * 将 PDF 文件转换为 PNG 图片
 * @param file - PDF 文件
 * @returns 包含转换后的 File 对象和元数据的对象
 */
export const convertPdfToImage = async (
  file: File
): Promise<{
  file: File | null;
  width?: number;
  height?: number;
}> => {
  try {
    const lib = await loadPdfJs();
    if (!lib) {
      throw new Error('Failed to load PDF.js');
    }

    const arrayBuffer = await file.arrayBuffer();

    // 获取 PDF 文档
    const pdf = await lib.getDocument({ data: arrayBuffer }).promise;

    // 获取第一页
    const page = await pdf.getPage(1);

    // 设置渲染分辨率（4倍）
    const viewport = page.getViewport({ scale: 4 });

    // 创建 Canvas
    const canvas = document.createElement('canvas');
    const context = canvas.getContext('2d');

    if (!context) {
      throw new Error('Failed to get canvas context');
    }

    canvas.width = viewport.width;
    canvas.height = viewport.height;

    // 配置高质量渲染
    context.imageSmoothingEnabled = true;
    context.imageSmoothingQuality = 'high';

    // 渲染页面到 Canvas
    await page.render({
      canvasContext: context,
      viewport: viewport,
    }).promise;

    // 转换为 PNG Blob
    return new Promise((resolve) => {
      canvas.toBlob(
        (blob) => {
          if (!blob) {
            resolve({ file: null });
            return;
          }

          const originalName = file.name.replace(/\\.pdf$/i, '');
          const imageFile = new File(
            [blob],
            \`\${originalName}.png\`,
            { type: 'image/png' }
          );

          resolve({
            file: imageFile,
            width: viewport.width,
            height: viewport.height,
          });
        },
        'image/png',
        1.0
      );
    });
  } catch (error) {
    console.error('PDF conversion failed:', error);
    return { file: null };
  }
};

/**
 * 验证 PDF 文件
 * @param file - 要验证的文件
 * @returns 是否为有效的 PDF
 */
export const validatePdfFile = (file: File): boolean => {
  // 检查 MIME 类型
  if (file.type !== 'application/pdf') {
    return false;
  }

  // 检查文件大小（限制为 20MB）
  const maxSize = 20 * 1024 * 1024;
  if (file.size > maxSize) {
    return false;
  }

  return true;
};

/**
 * 获取 PDF 的第一页作为缩略图
 * @param file - PDF 文件
 * @param scale - 缩放倍数（默认 2）
 * @returns 缩略图 Blob
 */
export const getPdfThumbnail = async (
  file: File,
  scale: number = 2
): Promise<Blob | null> => {
  try {
    const lib = await loadPdfJs();
    const arrayBuffer = await file.arrayBuffer();
    const pdf = await lib.getDocument({ data: arrayBuffer }).promise;
    const page = await pdf.getPage(1);
    const viewport = page.getViewport({ scale });

    const canvas = document.createElement('canvas');
    const context = canvas.getContext('2d');

    if (!context) return null;

    canvas.width = viewport.width;
    canvas.height = viewport.height;

    await page.render({
      canvasContext: context,
      viewport: viewport,
    }).promise;

    return new Promise((resolve) => {
      canvas.toBlob((blob) => {
        resolve(blob);
      }, 'image/png');
    });
  } catch (error) {
    console.error('Thumbnail generation failed:', error);
    return null;
  }
};
```

### 验证步骤 | Verification Steps

1. **检查 PDF 库加载 | Check PDF library loading**:
   ```bash
   npm run typecheck
   ```

2. **测试转换函数 | Test conversion function**:
   - 确保在上传页面中能正确调用 convertPdfToImage

### 预期结果 | Expected Results

- ✅ pdf2img.ts 文件创建成功
- ✅ PDF 转换函数正确工作
- ✅ 没有库加载错误

---

## 提示词 2.4: 创建 AI 常量和提示词配置 | Create AI Constants and Prompt Configuration

### 目标 | Goal
创建 constants/index.ts AI 响应格式和提示词配置

### 中文提示词 | Chinese Prompt

```
请在 constants/index.ts 创建 AI 相关的常量和提示词配置：

import type { Feedback } from "~/types";

/**
 * AI 响应格式定义
 * 用于指导 Claude 返回结构化的 JSON 响应
 */
export const AIResponseFormat = \`{
  "overallScore": <number 0-100>,
  "ATS": {
    "score": <number 0-100>,
    "tips": [
      {
        "type": "good" | "improve",
        "tip": "<short title>",
        "explanation": "<detailed explanation>"
      }
    ]
  },
  "toneAndStyle": {
    "score": <number 0-100>,
    "tips": [
      {
        "type": "good" | "improve",
        "tip": "<short title>",
        "explanation": "<detailed explanation>"
      }
    ]
  },
  "content": {
    "score": <number 0-100>,
    "tips": [
      {
        "type": "good" | "improve",
        "tip": "<short title>",
        "explanation": "<detailed explanation>"
      }
    ]
  },
  "structure": {
    "score": <number 0-100>,
    "tips": [
      {
        "type": "good" | "improve",
        "tip": "<short title>",
        "explanation": "<detailed explanation>"
      }
    ]
  },
  "skills": {
    "score": <number 0-100>,
    "tips": [
      {
        "type": "good" | "improve",
        "tip": "<short title>",
        "explanation": "<detailed explanation>"
      }
    ]
  }
}\`;

/**
 * 生成 AI 提示词
 * 根据职位信息生成个性化的简历分析指令
 * @param jobTitle - 职位名称
 * @param jobDescription - 职位描述
 * @returns AI 提示词字符串
 */
export const prepareInstructions = ({
  jobTitle,
  jobDescription,
}: {
  jobTitle: string;
  jobDescription: string;
}): string => {
  return \`You are an expert in ATS (Applicant Tracking System) and resume analysis.
Please analyze and rate this resume and suggest how to improve it.
The rating can be low if the resume is bad.
Be thorough and detailed. Don't be afraid to point out any mistakes or areas for improvement.
If there is a lot to improve, don't hesitate to give low scores. This is to help the user to improve their resume.
If available, use the job description for the job user is applying to to give more detailed feedback.
If provided, take the job description into consideration.
The job title is: \${jobTitle}
The job description is: \${jobDescription}
Provide the feedback using the following format:
\${AIResponseFormat}
Return the analysis as a JSON object, without any other text and without the backticks.
Do not include any other text or comments.\`;
};

/**
 * 示例 AI 响应（用于测试/演示）
 */
export const EXAMPLE_FEEDBACK: Feedback = {
  overallScore: 78,
  ATS: {
    score: 82,
    tips: [
      {
        type: "good",
        tip: "Good use of standard resume format",
        explanation:
          "Your resume follows a clear, ATS-friendly format with standard section headers.",
      },
      {
        type: "improve",
        tip: "Add more specific metrics",
        explanation:
          "Include quantifiable achievements like '30% improvement in load time' rather than 'improved performance'.",
      },
    ],
  },
  toneAndStyle: {
    score: 75,
    tips: [
      {
        type: "good",
        tip: "Professional tone throughout",
        explanation: "The language is appropriate and maintains a professional voice.",
      },
      {
        type: "improve",
        tip: "Reduce passive voice",
        explanation:
          "Use active voice (e.g., 'Led team' instead of 'Was responsible for team leadership').",
      },
    ],
  },
  content: {
    score: 76,
    tips: [
      {
        type: "good",
        tip: "Relevant experience highlighted",
        explanation:
          "Your previous roles clearly demonstrate skills relevant to the position.",
      },
      {
        type: "improve",
        tip: "Add more technical details",
        explanation:
          "Specify which technologies and tools you used (e.g., React, Node.js, PostgreSQL).",
      },
    ],
  },
  structure: {
    score: 80,
    tips: [
      {
        type: "good",
        tip: "Clear section organization",
        explanation:
          "Your resume has well-defined sections that are easy to scan.",
      },
      {
        type: "improve",
        tip: "Improve bullet point consistency",
        explanation:
          "Some bullet points are too long. Aim for 1-2 lines each for readability.",
      },
    ],
  },
  skills: {
    score: 75,
    tips: [
      {
        type: "good",
        tip: "Skills section is relevant",
        explanation:
          "You've listed skills that match the job requirements well.",
      },
      {
        type: "improve",
        tip: "Add proficiency levels",
        explanation:
          "Indicate proficiency levels (Beginner/Intermediate/Advanced) for better clarity.",
      },
    ],
  },
};

/**
 * 分数范围对应的评价
 */
export const SCORE_LABELS: Record<string, Record<string, string>> = {
  excellent: {
    min: "80",
    max: "100",
    label: "Excellent",
    description: "Your resume is in great shape!",
    color: "green",
  },
  good: {
    min: "60",
    max: "79",
    label: "Good",
    description: "Your resume is solid with room for improvement.",
    color: "yellow",
  },
  needsWork: {
    min: "0",
    max: "59",
    label: "Needs Improvement",
    description: "Your resume needs significant improvement.",
    color: "red",
  },
};

/**
 * 建议类型标签
 */
export const TIP_LABELS = {
  good: {
    label: "Good",
    icon: "✓",
    color: "green",
  },
  improve: {
    label: "Improve",
    icon: "⚠",
    color: "amber",
  },
};

/**
 * 错误消息
 */
export const ERROR_MESSAGES = {
  FILE_TOO_LARGE: "File size exceeds 20MB limit",
  INVALID_FILE_TYPE: "Only PDF files are accepted",
  UPLOAD_FAILED: "Failed to upload file. Please try again.",
  ANALYSIS_FAILED: "Failed to analyze resume. Please try again.",
  AUTH_REQUIRED: "Please sign in to continue",
  INVALID_EMAIL: "Please enter a valid email address",
};

/**
 * 成功消息
 */
export const SUCCESS_MESSAGES = {
  UPLOAD_COMPLETE: "Resume uploaded successfully",
  ANALYSIS_COMPLETE: "Analysis complete",
  DATA_DELETED: "All data has been deleted successfully",
};
```

### 验证步骤 | Verification Steps

1. **检查导出 | Check exports**:
   ```bash
   npm run typecheck
   ```

2. **验证 AI 格式 | Verify AI format**:
   - 确保 AIResponseFormat 是有效的 JSON 结构
   - 检查 prepareInstructions 函数返回有效的提示词

### 预期结果 | Expected Results

- ✅ constants/index.ts 创建成功
- ✅ AI 提示词格式正确
- ✅ 所有常量都能导入使用

---

## 提示词 2.5: 创建 Zustand Store (完整的 puter.ts) | Create Complete Zustand Store

### 目标 | Goal
创建 app/lib/puter.ts - 完整的 456 行 Zustand 状态管理和 Puter SDK 封装

### 说明 | Note

由于 puter.ts 文件长度为 456 行，这是一个非常复杂的文件。建议通过以下方式获取：

**选项 1: 参考原始项目**
从以下 GitHub 链接获取完整代码：
https://github.com/adrianhajdin/ai-resume-analyzer/blob/main/app/lib/puter.ts

**选项 2: 使用 AI 生成**
将以下提示词复制粘贴给 Claude/GPT：

```
请根据以下规范创建一个完整的 Zustand store (puter.ts)，共 456 行代码：

### 核心要求
1. 使用 Zustand 5.0.6 创建全局状态管理 store
2. 完整封装 Puter.js SDK 的所有功能
3. 提供一致的错误处理和类型安全
4. 支持认证、文件系统、KV存储和AI服务

### 功能模块

#### 1. 全局类型声明 (Lines 3-43)
declare global {
  interface Window {
    puter: PuterSDK;
  }
}

interface PuterSDK {
  auth: { ... };
  fs: { ... };
  kv: { ... };
  ai: { ... };
}

#### 2. Store 接口定义 (Lines 45-126)
interface PuterStore {
  // 系统状态
  isLoading: boolean;
  error: string | null;
  puterReady: boolean;

  // 认证模块
  auth: {
    user: PuterUser | null;
    isAuthenticated: boolean;
    signIn: () => Promise<void>;
    signOut: () => Promise<void>;
    refreshUser: () => Promise<void>;
    checkAuthStatus: () => Promise<boolean>;
    getUser: () => PuterUser | null;
  };

  // 文件系统模块
  fs: {
    write: (path: string, data: string | File | Blob) => Promise<File | undefined>;
    read: (path: string) => Promise<Blob | undefined>;
    upload: (files: File[] | Blob[]) => Promise<FSItem | undefined>;
    delete: (path: string) => Promise<void>;
    readDir: (path: string) => Promise<FSItem[] | undefined>;
  };

  // AI 服务模块
  ai: {
    chat: (...args: any[]) => Promise<AIResponse | undefined>;
    feedback: (path: string, message: string) => Promise<AIResponse | undefined>;
    img2txt: (image: string | File | Blob, testMode?: boolean) => Promise<string | undefined>;
  };

  // KV 存储模块
  kv: {
    get: (key: string) => Promise<string | null | undefined>;
    set: (key: string, value: string) => Promise<boolean | undefined>;
    delete: (key: string) => Promise<boolean | undefined>;
    list: (pattern: string, returnValues?: boolean) => Promise<any | undefined>;
    flush: () => Promise<boolean | undefined>;
  };

  // 工具方法
  init: () => void;
  clearError: () => void;
}

#### 3. Helper 函数 (Lines 128-150)
const getPuter = (): PuterSDK | null => {
  return typeof window !== 'undefined' ? window.puter || null : null;
};

const setError = (message: string) => {
  // 更新错误状态
};

#### 4. 认证服务实现 (Lines 152-215)
- checkAuthStatus(): 检查登录状态
- signIn(): 用户登录
- signOut(): 用户登出
- refreshUser(): 刷新用户信息
- getUser(): 获取当前用户

#### 5. 文件系统服务实现 (Lines 217-312)
- read(path): 读取文件
- readDir(path): 读取目录
- write(path, data): 写入文件
- upload(files): 上传文件
- delete(path): 删除文件

#### 6. AI 服务实现 (Lines 314-356)
- chat(...args): 通用聊天接口
- feedback(path, message): 简历分析
  - 发送文件和提示词给 Claude 3.7 Sonnet
  - 处理 multimodal 输入
- img2txt(image, testMode): 图像转文字

#### 7. KV 存储实现 (Lines 358-413)
- get(key): 获取值
- set(key, value): 设置值
- delete(key): 删除键
- list(pattern, returnValues): 列出匹配的键
- flush(): 清空所有数据

#### 8. 初始化和 Store 导出 (Lines 415-456)
- init(): Zustand store 初始化方法
- 轮询 Puter SDK 加载（最长 10 秒）
- 自动检查认证状态
- 导出 usePuterStore hook

### 关键实现细节
1. 所有 API 调用都通过 getPuter() 获取 window.puter
2. 错误处理：检查 null 并设置 error 状态
3. 异步操作：使用 async/await
4. 状态更新：使用 set() 函数
5. 类型安全：所有函数都有完整的类型注解

### 依赖关系
- import { create } from "zustand";
- import type { PuterUser, FSItem, AIResponse } from "~/types";
- import { getQueryParam } from "./utils";
```

建议复制上述提示词给 Claude 或 GPT，它们会生成完整的 puter.ts 文件。

### 验证步骤 | Verification Steps

1. **检查导出 | Check exports**:
   ```bash
   npm run typecheck
   ```

2. **验证 Hook | Verify hook**:
   - 确保 usePuterStore 能够导入
   - 检查所有方法都可以访问

### 预期结果 | Expected Results

- ✅ puter.ts 文件创建成功（456 行）
- ✅ 所有 Puter SDK 方法都被正确封装
- ✅ TypeScript 类型检查通过
- ✅ 没有运行时错误

---

本文档包含了后端/服务层的核心提示词。完成后，你将拥有完整的状态管理和服务层。

**Next Steps**: 进行到 03-frontend-prompts.md 进行前端组件开发。
