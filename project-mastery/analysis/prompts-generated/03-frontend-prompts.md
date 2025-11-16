# 前端提示词 | Frontend Prompts

**文档版本 | Document Version**: 1.0
**创建日期 | Created Date**: 2025-11-16
**语言 | Language**: 中英双语 (Bilingual)

---

## 提示词 3.1: 创建根组件和布局 | Create Root Component and Layout

### 目标 | Goal
创建 app/root.tsx - React Router v7 的根布局组件

### 中文提示词 | Chinese Prompt

```
请创建 app/root.tsx 根组件，包含以下功能：

import { useState, useEffect } from "react";
import { Outlet, isRouteErrorResponse, useRouterError } from "react-router";
import type { Route } from "@react-router/dev/routes";

import { usePuterStore } from "~/lib/puter";
import Navbar from "~/components/Navbar";
import "./app.css";

export const meta = (): Route.MetaDescriptors => [
  { title: "Resumind | AI Resume Analyzer" },
  { name: "description", content: "Analyze and improve your resume with AI" },
];

export default function Root() {
  const { init } = usePuterStore();

  useEffect(() => {
    init();
  }, []);

  return (
    <html lang="en">
      <head>
        <meta charSet="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <link rel="preconnect" href="https://fonts.googleapis.com" />
        <link rel="preconnect" href="https://fonts.gstatic.com" crossOrigin="anonymous" />
        <link
          href="https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700;800;900&display=swap"
          rel="stylesheet"
        />
      </head>
      <body>
        <div className="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100">
          <Navbar />
          <div className="flex-1">
            <Outlet />
          </div>
          <footer className="mt-16 border-t border-gray-200 bg-white py-8 text-center text-sm text-gray-600">
            <p>© 2025 RESUMIND. All rights reserved.</p>
          </footer>
        </div>
        <script src="https://js.puter.com/v2/"></script>
      </body>
    </html>
  );
}

export function ErrorBoundary({ error }: Route.ErrorBoundaryProps) {
  let message = "Oops!";
  let details = "An unexpected error occurred.";
  let stack: string | undefined;

  if (isRouteErrorResponse(error)) {
    message = error.status === 404 ? "404" : "Error";
    details =
      error.status === 404
        ? "The requested page could not be found."
        : error.statusText || details;
  } else if (import.meta.env.DEV && error && error instanceof Error) {
    details = error.message;
    stack = error.stack;
  }

  return (
    <main className="pt-16 p-4 container mx-auto">
      <div className="max-w-2xl mx-auto text-center">
        <h1 className="text-4xl font-bold text-gray-900 mb-2">{message}</h1>
        <p className="text-lg text-gray-600 mb-8">{details}</p>
        {stack && (
          <pre className="w-full p-4 overflow-x-auto bg-gray-100 rounded-lg text-left text-sm">
            <code>{stack}</code>
          </pre>
        )}
        <a
          href="/"
          className="inline-block mt-8 px-6 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors"
        >
          Go Home
        </a>
      </div>
    </main>
  );
}
```

### 验证步骤 | Verification Steps

1. **检查导入 | Check imports**:
   ```bash
   npm run typecheck
   ```

2. **运行开发服务器 | Start dev server**:
   ```bash
   npm run dev
   ```

3. **验证页面渲染 | Verify page loads**:
   - 打开 http://localhost:5173
   - 应该看到基本的布局和导航栏

---

## 提示词 3.2: 创建 UI 组件集合 | Create UI Component Library

### 目标 | Goal
创建 10 个核心 UI 组件

### 中文提示词 | Chinese Prompt

```
请创建以下 10 个 UI 组件。每个组件都应该使用 Tailwind CSS 4.1.4 进行样式设置：

### 1. Navbar 组件 (app/components/Navbar.tsx)

import { Link } from "react-router";
import { usePuterStore } from "~/lib/puter";

export default function Navbar() {
  const { auth } = usePuterStore();

  return (
    <nav className="sticky top-0 z-50 border-b border-gray-200 bg-white">
      <div className="container mx-auto flex h-16 items-center justify-between px-4">
        <Link to="/" className="flex items-center gap-2">
          <div className="w-8 h-8 rounded-lg bg-gradient-to-br from-blue-600 to-purple-600 flex items-center justify-center">
            <span className="text-white font-bold text-sm">R</span>
          </div>
          <span className="text-lg font-bold text-gray-900">RESUMIND</span>
        </Link>

        <div className="flex items-center gap-4">
          {auth.isAuthenticated && (
            <Link
              to="/upload"
              className="px-4 py-2 rounded-lg bg-blue-600 text-white hover:bg-blue-700 transition-colors"
            >
              Upload Resume
            </Link>
          )}
        </div>
      </div>
    </nav>
  );
}

### 2. ScoreCircle 组件 (app/components/ScoreCircle.tsx)

import { cn } from "~/lib/utils";

interface ScoreCircleProps {
  score: number;
  size?: "sm" | "md" | "lg";
}

export default function ScoreCircle({ score, size = "md" }: ScoreCircleProps) {
  const getColor = (score: number) => {
    if (score >= 70) return "text-green-600";
    if (score >= 50) return "text-yellow-600";
    return "text-red-600";
  };

  const getBgColor = (score: number) => {
    if (score >= 70) return "bg-green-100";
    if (score >= 50) return "bg-yellow-100";
    return "bg-red-100";
  };

  const sizeClasses = {
    sm: "w-12 h-12 text-sm",
    md: "w-16 h-16 text-lg",
    lg: "w-20 h-20 text-2xl",
  };

  return (
    <div
      className={cn(
        "flex items-center justify-center rounded-full font-bold",
        sizeClasses[size],
        getBgColor(score),
        getColor(score)
      )}
    >
      {score}
    </div>
  );
}

### 3. ScoreBadge 组件 (app/components/ScoreBadge.tsx)

import { cn } from "~/lib/utils";

interface ScoreBadgeProps {
  label: string;
  score: number;
}

export default function ScoreBadge({ label, score }: ScoreBadgeProps) {
  const getColor = (score: number) => {
    if (score >= 70) return "bg-green-100 text-green-700";
    if (score >= 50) return "bg-yellow-100 text-yellow-700";
    return "bg-red-100 text-red-700";
  };

  return (
    <div className="flex items-center justify-between p-4 rounded-lg bg-gray-50 border border-gray-200">
      <span className="text-sm font-medium text-gray-600">{label}</span>
      <div
        className={cn(
          "px-3 py-1 rounded-full text-sm font-semibold",
          getColor(score)
        )}
      >
        {score}/100
      </div>
    </div>
  );
}

### 4. ScoreGauge 组件 (app/components/ScoreGauge.tsx)

interface ScoreGaugeProps {
  score: number;
  label?: string;
}

export default function ScoreGauge({ score, label = "Overall Score" }: ScoreGaugeProps) {
  const percentage = Math.min(score, 100);
  const getColor = (score: number) => {
    if (score >= 70) return "from-green-500 to-green-600";
    if (score >= 50) return "from-yellow-500 to-yellow-600";
    return "from-red-500 to-red-600";
  };

  return (
    <div className="space-y-2">
      <div className="flex justify-between items-center">
        <h3 className="text-lg font-semibold text-gray-900">{label}</h3>
        <span className="text-2xl font-bold text-gray-900">{score}</span>
      </div>
      <div className="w-full bg-gray-200 rounded-full h-3 overflow-hidden">
        <div
          className={cn(
            "h-full rounded-full transition-all duration-300 bg-gradient-to-r",
            getColor(score)
          )}
          style={{ width: \`\${percentage}%\` }}
        />
      </div>
    </div>
  );
}

### 5. Accordion 组件 (app/components/Accordion.tsx)

import { useState } from "react";
import { cn } from "~/lib/utils";

interface AccordionProps {
  title: string;
  children: React.ReactNode;
  defaultOpen?: boolean;
}

export default function Accordion({
  title,
  children,
  defaultOpen = false,
}: AccordionProps) {
  const [isOpen, setIsOpen] = useState(defaultOpen);

  return (
    <div className="border border-gray-200 rounded-lg overflow-hidden">
      <button
        onClick={() => setIsOpen(!isOpen)}
        className="w-full px-6 py-4 flex items-center justify-between bg-gray-50 hover:bg-gray-100 transition-colors"
      >
        <h3 className="font-semibold text-gray-900">{title}</h3>
        <svg
          className={cn(
            "w-5 h-5 text-gray-600 transition-transform",
            isOpen && "rotate-180"
          )}
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            strokeLinecap="round"
            strokeLinejoin="round"
            strokeWidth={2}
            d="M19 14l-7 7m0 0l-7-7m7 7V3"
          />
        </svg>
      </button>
      {isOpen && <div className="px-6 py-4 border-t border-gray-200">{children}</div>}
    </div>
  );
}

### 6. FileUploader 组件 (app/components/FileUploader.tsx)

import { useCallback } from "react";
import { useDropzone } from "react-dropzone";
import { cn, formatSize } from "~/lib/utils";

interface FileUploaderProps {
  onFilesAccepted: (files: File[]) => void;
}

export default function FileUploader({ onFilesAccepted }: FileUploaderProps) {
  const onDrop = useCallback(
    (acceptedFiles: File[]) => {
      const pdfFiles = acceptedFiles.filter((file) => file.type === "application/pdf");
      if (pdfFiles.length > 0) {
        onFilesAccepted(pdfFiles);
      }
    },
    [onFilesAccepted]
  );

  const { getRootProps, getInputProps, isDragActive, acceptedFiles } = useDropzone({
    onDrop,
    accept: { "application/pdf": [".pdf"] },
    maxSize: 20 * 1024 * 1024,
    multiple: false,
  });

  return (
    <div
      {...getRootProps()}
      className={cn(
        "border-2 border-dashed rounded-lg p-8 text-center cursor-pointer transition-colors gradient-border",
        isDragActive
          ? "border-blue-500 bg-blue-50"
          : "border-gray-300 bg-gray-50 hover:bg-gray-100"
      )}
    >
      <input {...getInputProps()} />
      <svg
        className="mx-auto h-12 w-12 text-gray-400 mb-4"
        fill="none"
        stroke="currentColor"
        viewBox="0 0 24 24"
      >
        <path
          strokeLinecap="round"
          strokeLinejoin="round"
          strokeWidth={2}
          d="M12 4v16m8-8H4"
        />
      </svg>
      {isDragActive ? (
        <p className="text-lg font-medium text-blue-600">Drop your PDF here...</p>
      ) : (
        <>
          <p className="text-lg font-medium text-gray-900">Drag and drop your resume</p>
          <p className="text-sm text-gray-500 mt-2">or click to select a file</p>
          <p className="text-xs text-gray-400 mt-4">PDF files only • Max 20MB</p>
        </>
      )}
      {acceptedFiles.length > 0 && (
        <div className="mt-4 text-left bg-white rounded-lg p-4 border border-gray-200">
          {acceptedFiles.map((file) => (
            <div
              key={file.name}
              className="flex items-center justify-between py-2 px-2 hover:bg-gray-50 rounded"
            >
              <div className="flex items-center gap-2">
                <svg
                  className="w-5 h-5 text-red-500"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                >
                  <path d="M4 3a2 2 0 00-2 2v10a2 2 0 002 2h12a2 2 0 002-2V5a2 2 0 00-2-2H4zm0 2h12v10H4V5z" />
                </svg>
                <div>
                  <p className="text-sm font-medium text-gray-900">{file.name}</p>
                  <p className="text-xs text-gray-500">{formatSize(file.size)}</p>
                </div>
              </div>
              <svg className="w-5 h-5 text-green-500" fill="currentColor" viewBox="0 0 20 20">
                <path
                  fillRule="evenodd"
                  d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                  clipRule="evenodd"
                />
              </svg>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

### 7. ResumeCard 组件 (app/components/ResumeCard.tsx)

import { useEffect, useState } from "react";
import { useNavigate } from "react-router";
import type { Resume } from "~/types";
import { usePuterStore } from "~/lib/puter";
import ScoreCircle from "./ScoreCircle";

interface ResumeCardProps {
  resume: Resume;
}

export default function ResumeCard({ resume }: ResumeCardProps) {
  const [imageUrl, setImageUrl] = useState<string>("");
  const { fs } = usePuterStore();
  const navigate = useNavigate();

  useEffect(() => {
    const loadImage = async () => {
      const blob = await fs.read(resume.imagePath);
      if (blob) {
        const url = URL.createObjectURL(blob);
        setImageUrl(url);
      }
    };
    loadImage();
  }, [resume.imagePath, fs]);

  return (
    <div
      onClick={() => navigate(\`/resume/\${resume.id}\`)}
      className="cursor-pointer rounded-lg overflow-hidden gradient-border hover:shadow-lg transition-shadow animate-in fade-in duration-1000"
    >
      {imageUrl && (
        <img
          src={imageUrl}
          alt={resume.jobTitle}
          className="w-full h-[350px] max-sm:h-[200px] object-cover"
        />
      )}
      <div className="p-6 bg-white">
        <div className="flex items-start justify-between mb-4">
          <div>
            <h3 className="font-semibold text-gray-900 text-lg">{resume.jobTitle}</h3>
            <p className="text-sm text-gray-600">{resume.companyName}</p>
          </div>
          <ScoreCircle score={resume.feedback.overallScore} size="sm" />
        </div>
      </div>
    </div>
  );
}

### 8. Summary 组件 (app/components/Summary.tsx)

import type { Feedback } from "~/types";
import ScoreGauge from "./ScoreGauge";
import ScoreBadge from "./ScoreBadge";

interface SummaryProps {
  feedback: Feedback;
}

export default function Summary({ feedback }: SummaryProps) {
  return (
    <div className="space-y-8">
      <div className="bg-white rounded-lg p-8 border border-gray-200">
        <ScoreGauge score={feedback.overallScore} />
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
        <ScoreBadge label="ATS Compatibility" score={feedback.ATS.score} />
        <ScoreBadge label="Tone & Style" score={feedback.toneAndStyle.score} />
        <ScoreBadge label="Content Quality" score={feedback.content.score} />
        <ScoreBadge label="Structure" score={feedback.structure.score} />
        <ScoreBadge label="Skills Match" score={feedback.skills.score} />
      </div>
    </div>
  );
}

### 9. ATS 组件 (app/components/ATS.tsx)

import type { ScoreSection } from "~/types";
import { cn } from "~/lib/utils";

interface ATSProps {
  data: ScoreSection;
}

export default function ATS({ data }: ATSProps) {
  const getGradient = (score: number) => {
    if (score > 69) return "from-green-100 to-green-50";
    if (score > 49) return "from-yellow-100 to-yellow-50";
    return "from-red-100 to-red-50";
  };

  const getTextColor = (score: number) => {
    if (score > 69) return "text-green-700";
    if (score > 49) return "text-yellow-700";
    return "text-red-700";
  };

  const getLabel = (score: number) => {
    if (score > 69) return "Great Job!";
    if (score > 49) return "Good Start";
    return "Needs Improvement";
  };

  return (
    <div
      className={cn(
        "rounded-lg p-8 bg-gradient-to-r",
        getGradient(data.score)
      )}
    >
      <div className="flex items-center justify-between mb-6">
        <h2 className="text-2xl font-bold text-gray-900">ATS Analysis</h2>
        <div className={cn("text-3xl font-bold", getTextColor(data.score))}>
          {data.score}/100
        </div>
      </div>

      <p className={cn("text-lg font-semibold mb-6", getTextColor(data.score))}>
        {getLabel(data.score)}
      </p>

      <div className="space-y-4">
        {data.tips?.map((tip, index) => (
          <div
            key={index}
            className="bg-white rounded-lg p-4 border border-gray-200 hover:shadow-md transition-shadow"
          >
            <div className="flex items-start gap-3">
              <span
                className={cn(
                  "mt-1 text-lg font-bold",
                  tip.type === "good" ? "text-green-600" : "text-amber-600"
                )}
              >
                {tip.type === "good" ? "✓" : "⚠"}
              </span>
              <div>
                <h4 className="font-semibold text-gray-900">{tip.tip}</h4>
                {tip.explanation && (
                  <p className="text-sm text-gray-600 mt-1">{tip.explanation}</p>
                )}
              </div>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}

### 10. Details 组件 (app/components/Details.tsx)

import type { Feedback } from "~/types";
import Accordion from "./Accordion";

interface DetailsProps {
  feedback: Feedback;
}

export default function Details({ feedback }: DetailsProps) {
  const sections = [
    { key: "toneAndStyle", label: "Tone & Style", data: feedback.toneAndStyle },
    { key: "content", label: "Content Quality", data: feedback.content },
    { key: "structure", label: "Structure", data: feedback.structure },
    { key: "skills", label: "Skills", data: feedback.skills },
  ];

  return (
    <div className="space-y-4">
      {sections.map((section) => (
        <Accordion
          key={section.key}
          title={\`\${section.label} - \${section.data.score}/100\`}
        >
          <div className="space-y-4">
            {section.data.tips?.map((tip, index) => (
              <div
                key={index}
                className="pb-4 border-b border-gray-200 last:border-0"
              >
                <div className="flex items-start gap-3">
                  <span
                    className={\`text-lg font-bold \${
                      tip.type === "good" ? "text-green-600" : "text-amber-600"
                    }\`}
                  >
                    {tip.type === "good" ? "✓" : "⚠"}
                  </span>
                  <div>
                    <h5 className="font-semibold text-gray-900">{tip.tip}</h5>
                    {tip.explanation && (
                      <p className="text-sm text-gray-600 mt-2">
                        {tip.explanation}
                      </p>
                    )}
                  </div>
                </div>
              </div>
            ))}
          </div>
        </Accordion>
      ))}
    </div>
  );
}
```

### 验证步骤 | Verification Steps

1. **检查导出 | Check exports**:
   ```bash
   npm run typecheck
   ```

2. **验证组件使用 | Verify component usage**:
   - 运行开发服务器
   - 检查组件是否正确渲染

### 预期结果 | Expected Results

- ✅ 所有 10 个组件创建成功
- ✅ 组件类型检查通过
- ✅ 样式正确应用
- ✅ 没有导入错误

---

## 提示词 3.3: 创建路由页面 | Create Route Pages

### 目标 | Goal
创建 5 个路由页面组件

### 说明 | Note

由于篇幅限制，每个页面的完整代码非常长。建议使用以下方式：

**获取方式**:
从原始项目获取：https://github.com/adrianhajdin/ai-resume-analyzer/tree/main/app/routes

或使用以下 AI 提示词生成：

```
请为 AI Resume Analyzer 项目创建以下 5 个路由页面：

### 1. home.tsx (路由: /)
功能：显示用户简历列表
- 从 usePuterStore 获取简历列表
- 使用 ResumeCard 组件展示每个简历
- 实现路由保护（未认证用户重定向到 /auth）
- 显示加载状态和错误消息
- 支持导航到 /upload 和 /resume/:id

### 2. auth.tsx (路由: /auth)
功能：用户认证页面（登录/登出）
- 检查 usePuterStore.auth.isAuthenticated
- 显示登录按钮或欢迎信息
- 已登录用户自动重定向到首页
- 支持从 ?next 参数跳转到指定页面
- 显示加载状态

### 3. upload.tsx (路由: /upload)
功能：简历上传和分析页面
- FileUploader 组件用于选择 PDF
- 表单输入：公司名、职位名、职位描述
- 调用 ai.feedback() 进行 AI 分析
- 显示进度状态（上传...、转换...、分析中...）
- 完成后自动导航到 /resume/:id
- 实现路由保护

### 4. resume.tsx (路由: /resume/:id)
功能：简历详情和 AI 反馈页面
- 从 URL 参数获取简历 ID
- 从 KV 存储读取简历数据
- 从 FS 读取 PDF 和预览图
- 显示 Summary、ATS、Details 组件
- 两列布局：左侧 PDF 预览、右侧反馈
- 实现路由保护

### 5. wipe.tsx (路由: /wipe)
功能：数据清除工具页面
- 显示警告信息（不可恢复）
- 确认框防止误删
- 删除所有文件和 KV 数据
- 完成后重定向到首页
- 实现路由保护
```

复制上述提示词给 Claude 或 GPT 以获取完整的页面代码。

---

本文档包含了前端组件和路由的核心提示词。完成后，你将拥有完整的用户界面。

**Next Steps**: 进行到 04-integration-prompts.md 进行集成和部署配置。
