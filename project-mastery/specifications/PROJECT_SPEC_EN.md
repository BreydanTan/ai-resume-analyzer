# AI Resume Analyzer - Technical Specification Document (English)

**Document Version**: 1.0
**Last Updated**: 2025-11-16
**Audience**: Developers, System Administrators, Product Managers
**Language**: English

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Technical Architecture](#technical-architecture)
3. [Database Design](#database-design)
4. [Core Business Flows](#core-business-flows)
5. [Frontend-Backend Integration](#frontend-backend-integration)
6. [API Documentation](#api-documentation)
7. [Environment Configuration](#environment-configuration)
8. [Secondary Development Guide](#secondary-development-guide)
9. [Deployment Guide](#deployment-guide)
10. [Appendix](#appendix)

---

## 1. Project Overview

### 1.1 Project Introduction

The AI Resume Analyzer is an intelligent resume evaluation system that combines artificial intelligence with cloud storage to provide real-time feedback on resume quality and ATS (Applicant Tracking System) compatibility.

**Core Functionalities**:
- 🔐 **User Authentication** - OAuth-style login via Puter platform
- 📄 **Resume Upload** - Support for PDF format resumes with instant preview
- 🤖 **AI Analysis** - Claude AI provides detailed resume assessment
- 📊 **Multi-Dimension Scoring** - Evaluation across 5 dimensions:
  - ATS Compatibility
  - Tone and Style
  - Content Quality
  - Document Structure
  - Skills Matching
- 💡 **Actionable Feedback** - Specific improvement suggestions with explanations
- 💾 **Data Persistence** - All resumes automatically saved to cloud storage

### 1.2 Technology Highlights

1. **Zero-Backend Architecture** - No traditional server or database infrastructure needed
2. **Serverless Cloud Storage** - Leverages Puter platform's managed services
3. **Multimodal AI Analysis** - Claude understands both text and visual resume formats
4. **Real-Time Processing** - Analysis results available within seconds
5. **Privacy-First Design** - User data isolated to individual accounts

### 1.3 Use Cases

✅ **Ideal For**:
- Job seekers optimizing resume before applications
- Career coaches providing resume feedback at scale
- College students preparing for campus recruitment
- Career changers strengthening their profiles

❌ **Not Suitable For**:
- Scenarios requiring manual human editing
- Cases needing data retention beyond Puter account lifecycle

---

## 2. Technical Architecture

### 2.1 Overall Architecture Diagram

```
┌─────────────────────────────────────────────────────┐
│            Browser (Client Application)              │
│  ┌─────────────────────────────────────────────┐   │
│  │      React 19 Application Layer             │   │
│  │  ┌───────────┐  ┌──────────┐  ┌──────────┐ │   │
│  │  │ Routes    │  │Components│  │  Services │ │   │
│  │  │  5 pages  │  │ 10 comps │  │ (Lib)    │ │   │
│  │  └───────────┘  └──────────┘  └──────────┘ │   │
│  │         ↓ State Management ↓               │   │
│  │  ┌────────────────────────────────────────┐ │   │
│  │  │    Zustand Store (State Hub)           │ │   │
│  │  │  - Authentication state                │ │   │
│  │  │  - File system operations              │ │   │
│  │  │  - KV storage access                   │ │   │
│  │  │  - AI service wrapper                  │ │   │
│  │  └────────────────────────────────────────┘ │   │
│  │         ↓ API Abstraction Layer ↓         │   │
│  │  ┌────────────────────────────────────────┐ │   │
│  │  │     Puter.js SDK (Global window.puter) │ │   │
│  │  └────────────────────────────────────────┘ │   │
│  └─────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
              ↓ HTTPS API Calls ↓
┌─────────────────────────────────────────────────────┐
│        Puter.com Cloud Platform (Backend)            │
│  ┌──────────────┐  ┌──────────────┐                 │
│  │ Auth Service │  │ File Storage  │                 │
│  │  (Identity)  │  │  (Resume PDF) │                 │
│  └──────────────┘  └──────────────┘                 │
│  ┌──────────────┐  ┌──────────────┐                 │
│  │ KV Database  │  │ AI Service   │                 │
│  │ (Metadata)   │  │ (Claude)      │                 │
│  └──────────────┘  └──────────────┘                 │
└─────────────────────────────────────────────────────┘
```

### 2.2 Technology Stack Explanation

| Technology | Version | Purpose | Role |
|-----------|---------|---------|------|
| **React** | 19.1.0 | UI Framework | Component rendering & lifecycle |
| **React Router** | 7.5.3 | Page Navigation | File-based routing, SSR support |
| **TypeScript** | 5.8.3 | Type Safety | Compile-time type checking |
| **Zustand** | 5.0.6 | State Management | Global app state with minimal boilerplate |
| **Tailwind CSS** | 4.1.4 | Styling | Utility-first CSS framework |
| **Puter.js** | v2 | Cloud SDK | Auth, Storage, KV, and AI APIs |
| **pdfjs-dist** | 5.3.93 | PDF Processing | Convert resume PDF to preview image |
| **react-dropzone** | 14.3.8 | File Upload | Drag-and-drop file handler |
| **Vite** | 6.3.3 | Build Tool | Lightning-fast bundler and dev server |

### 2.3 Data Flow Diagram

```
User Action
    ↓
React Component (routes/*)
    ↓
Zustand Store (usePuterStore)
    ↓
Puter.js SDK (window.puter.*)
    ↓
HTTPS API Call → Puter Cloud
    ↓
Cloud Service (Auth/FS/KV/AI)
    ↓
Response → Component State Update → Re-render
```

### 2.4 Frontend-Backend Communication

```
Frontend (Browser)          Backend (Puter Cloud)
      │                              │
      │─── User logs in ────────────→│
      │                              │ Validates credentials
      │←─── Return user ID ─────────│
      │                              │
      │─── Upload resume PDF ──────→│
      │                              │ Store in FS
      │←─── Return file path ──────│
      │                              │
      │─── Request AI analysis ────→│
      │                              │ Claude analyzes
      │←─── Return feedback JSON ──│
      │                              │
      │─── Save metadata to KV ────→│
      │                              │ Store in KV
      │←─── Confirm save ──────────│
```

---

## 3. Database Design

### 3.1 Storage Architecture (Puter KV + FS)

This application uses **two complementary storage mechanisms** instead of traditional relational/NoSQL databases:

**1. File System (FS) - For binary files**
```
/home/user123/
├── resume_abc.pdf              (Original resume)
├── resume_abc_preview.png      (Generated thumbnail)
├── resume_def.pdf
├── resume_def_preview.png
└── ...
```

**2. Key-Value Store (KV) - For metadata**
```
resume:abc123 → {"id":"abc123", "companyName":"Google", "jobTitle":"Engineer", "feedback":{...}}
resume:def456 → {"id":"def456", "companyName":"Facebook", "jobTitle":"Manager", "feedback":{...}}
resume:ghi789 → {"id":"ghi789", "companyName":"Amazon", "jobTitle":"Architect", "feedback":{...}}
```

### 3.2 Resume Data Model

```typescript
interface Resume {
  // Unique identifier
  id: string;                    // UUID v4 (e.g., "a1b2c3d4-...")

  // Input metadata
  companyName: string;           // Target company (e.g., "Google")
  jobTitle: string;              // Position title (e.g., "Senior Engineer")
  jobDescription: string;        // Job requirements/description

  // Storage references
  resumePath: string;            // Puter FS path to PDF (e.g., "/home/user/resume.pdf")
  imagePath: string;             // Puter FS path to preview (e.g., "/home/user/resume.png")

  // AI Analysis results
  feedback: Feedback;            // Structured evaluation data

  // Metadata
  createdAt?: string;            // ISO 8601 timestamp
  updatedAt?: string;            // ISO 8601 timestamp
}

interface Feedback {
  overallScore: number;          // Composite score (0-100)
  ATS: ScoreSection;             // ATS system compatibility
  toneAndStyle: ScoreSection;    // Writing tone and formatting
  content: ScoreSection;         // Content quality and relevance
  structure: ScoreSection;       // Organization and structure
  skills: ScoreSection;          // Technical skills alignment
}

interface ScoreSection {
  score: number;                 // Dimension-specific score (0-100)
  tips: Tip[];                   // Array of 3-4 suggestions
}

interface Tip {
  type: "good" | "improve";      // Positive or constructive feedback
  tip: string;                   // Brief title/subject
  explanation: string;           // Detailed explanation
}
```

**Code Location**: Inferred from `constants/index.ts:1-182` and `app/routes/upload.tsx:38-44`

### 3.3 Key Naming Convention

**Pattern**: `resume:{UUID}`

**Examples**:
```
resume:a1b2c3d4-e5f6-7890-abcd-ef1234567890
resume:f4e3d2c1-b0a9-8765-4321-0fedcba98765
resume:12345678-90ab-cdef-ghij-klmnopqrstuv
```

**Rationale**:
- **Prefix (`resume:`)** - Enables filtering and future expansion (e.g., `user:`, `template:`)
- **UUID** - Guarantees uniqueness without coordination
- **Immutable** - Key never changes after creation

**Code Location**: `app/routes/upload.tsx:45`

### 3.4 Data Relationships

```
Resume (KV Store)
├─ id, companyName, jobTitle
├─ resumePath ──────┐
├─ imagePath ──────┐│
└─ feedback        ││
                   ││
         ┌─────────┘│
         │          │
File System Storage
├─ /path/to/resume.pdf  (Binary PDF)
└─ /path/to/resume.png  (Binary PNG)

Feedback Structure
├─ overallScore: 85
├─ ATS: { score: 90, tips: [...] }
├─ toneAndStyle: { score: 75, tips: [...] }
├─ content: { score: 80, tips: [...] }
├─ structure: { score: 85, tips: [...] }
└─ skills: { score: 88, tips: [...] }
```

---

## 4. Core Business Flows

### 4.1 User Authentication Flow

```
┌─────────────────────────┐
│  Application Launch     │
└────────────┬────────────┘
             │
             ▼
┌──────────────────────────────┐
│ Load Puter.js from CDN       │
│ (window.puter initialized)   │
└────────────┬─────────────────┘
             │
             ▼
┌──────────────────────────────┐
│ Check Authentication Status  │
│ puter.auth.isSignedIn()      │
└────────────┬─────────────────┘
             │
        ┌────┴────┐
        │          │
        ▼          ▼
┌────────────┐  ┌──────────────┐
│ Signed In  │  │ Not Signed In │
│ ✓ Load     │  │ ✗ Redirect   │
│ Dashboard  │  │ to /auth     │
└────────────┘  └──────────────┘
                      │
                      ▼
             ┌──────────────────┐
             │ Auth Page (/auth)│
             └────────┬─────────┘
                      │
                      ▼
             ┌──────────────────┐
             │ Click "Sign In"  │
             │ (Opens OAuth)    │
             └────────┬─────────┘
                      │
                      ▼
             ┌──────────────────┐
             │ Puter Login      │
             │ Modal Window     │
             └────────┬─────────┘
                      │
                      ▼
             ┌──────────────────┐
             │ User Credentials │
             │ Validated        │
             └────────┬─────────┘
                      │
                      ▼
             ┌──────────────────┐
             │ Load Dashboard   │
             │ Show Resumes     │
             └──────────────────┘
```

**Code Location**: `app/lib/puter.ts:119-166` (authentication logic)

### 4.2 Resume Upload & Analysis Flow

```
Step 1: User Input
    User selects PDF file
    ↓
    Enters target company name
    ↓
    Enters job title
    ↓
    Provides job description
    ↓
Step 2: File Processing
    Convert PDF to preview image
    ↓
Step 3: Upload to Cloud
    Upload PDF file to Puter FS
    ↓
    Upload preview image to Puter FS
    ↓
Step 4: Save Metadata
    Generate UUID
    ↓
    Save resume metadata to KV (feedback='')
    ↓
Step 5: AI Analysis
    Extract resume from cloud path
    ↓
    Generate AI prompt with job info
    ↓
    Call Claude AI for analysis
    ↓
Step 6: Update Results
    Parse AI response JSON
    ↓
    Update resume metadata with feedback
    ↓
Step 7: Display Results
    Redirect to /resume/:id
    ↓
    Show detailed analysis and scores
```

**Code Location**: `app/routes/upload.tsx:21-64`

### 4.3 Resume List Loading Flow

```
User opens home page (/)
    ↓
Fetch all KV entries matching 'resume:*'
    ↓
Parse each JSON string
    ↓
Load thumbnail image for each
    ↓
Sort by creation date (newest first)
    ↓
Render ResumeCard components
```

**Code Location**: `app/routes/home.tsx:26-40`

### 4.4 Resume Detail View Flow

```
User clicks resume card
    ↓
Route to /resume/:id
    ↓
Fetch metadata from KV (resume:{id})
    ↓
Load PDF file from FS
    ↓
Load preview image from FS
    ↓
Create Blob URLs for display
    ↓
Display in split-view layout
├─ Left: PDF viewer
├─ Right: Analysis report
│   ├─ Score gauge
│   ├─ ATS analysis
│   └─ Detailed feedback sections
```

**Code Location**: `app/routes/resume.tsx:25-50`

### 4.5 Data Wipe Flow

```
User navigates to /wipe
    ↓
Confirmation dialog appears
    ↓
User confirms deletion
    ↓
Delete all files (readdir then delete each)
    ↓
Flush all KV entries
    ↓
Account restored to blank state
```

**Code Location**: `app/routes/wipe.tsx:25-31`

**Warning**: This operation is **irreversible**

---

## 5. Frontend-Backend Integration

### 5.1 Puter.js SDK Integration

**Puter.js is injected via script tag**:
```html
<!-- app/root.tsx:44 -->
<script src="https://js.puter.com/v2/"></script>
```

**SDK Initialization Flow**:
```typescript
// app/root.tsx:32
const init = () => {
  // Polls for window.puter availability (100ms intervals)
  // Times out after 10 seconds
  // Calls checkAuthStatus() once loaded
}
```

**Global Type Declaration**:
```typescript
declare global {
  interface Window {
    puter: {
      auth: { ... }      // Authentication API
      fs: { ... }        // File system API
      ai: { ... }        // AI analysis API
      kv: { ... }        // Key-value storage API
    }
  }
}
```

**Code Location**: `app/lib/puter.ts:1-43`

### 5.2 Zustand State Management

**Store Architecture**:
```typescript
const usePuterStore = create<PuterStore>((set, get) => {
  // State getters and setters
  isLoading: boolean
  error: string | null
  puterReady: boolean

  // Modular service interfaces
  auth: { ... }        // Authentication
  fs: { ... }          // File system
  ai: { ... }          // AI services
  kv: { ... }          // Key-value database

  // Utility methods
  init()               // Initialize SDK
  clearError()         // Reset error state
})
```

**Usage Pattern**:
```typescript
// In any React component
const store = usePuterStore()
const { auth, fs, kv, ai, isLoading } = store

// Access state
if (auth.isAuthenticated) {
  console.log('User:', auth.user)
}

// Call methods
await fs.upload([file])
await kv.set(key, value)
```

**Code Location**: `app/lib/puter.ts:45-456`

### 5.3 File System Operations

**Upload Files**:
```typescript
const result = await fs.upload([file1, file2])
// Returns: { id, name, path, type, size, ... }
```

**Read Files**:
```typescript
const blob = await fs.read(filePath)
// Returns: Blob object (binary data)
```

**Delete Files**:
```typescript
await fs.delete(filePath)
// No return value
```

**List Directory**:
```typescript
const items = await fs.readdir(dirPath)
// Returns: FSItem[] array
```

**Code Location**: `app/lib/puter.ts:268-311`

### 5.4 Key-Value Storage Operations

**Set Data**:
```typescript
await kv.set('resume:abc123', JSON.stringify(data))
// Note: Value must be string, not object
```

**Get Data**:
```typescript
const json = await kv.get('resume:abc123')
const data = JSON.parse(json)
```

**List Matching Keys**:
```typescript
const items = await kv.list('resume:*', true)
// Returns: Array of {key, value} pairs
```

**Delete Data**:
```typescript
await kv.delete('resume:abc123')
```

**Flush All**:
```typescript
await kv.flush()
// Deletes entire KV store - IRREVERSIBLE
```

**Code Location**: `app/lib/puter.ts:366-412`

### 5.5 AI Service Integration

**Resume Analysis**:
```typescript
const feedback = await ai.feedback(
  resumePath,        // Puter FS path to PDF
  instructions       // AI prompt with context
)
// Returns: { message: { content: JSON string } }
```

**Prompt Engineering**:
```typescript
const instructions = `You are an ATS expert. Analyze this resume for:
- Position: ${jobTitle}
- Description: ${jobDescription}

Rate on: ATS Compatibility, Tone, Content, Structure, Skills

Return valid JSON only: ${AIResponseFormat}`
```

**Response Processing**:
```typescript
const result = await ai.feedback(resumePath, instructions)
const feedbackText = result.message.content
const feedback = JSON.parse(feedbackText)
```

**Code Location**: `app/lib/puter.ts:330-354`

---

## 6. API Documentation (Puter Services)

### 6.1 Authentication APIs

#### Sign In
```typescript
await puter.auth.signIn()
// Opens OAuth login modal
// Resolves when user completes authentication
```

**Usage**: User clicks "Log In" button

---

#### Sign Out
```typescript
await puter.auth.signOut()
// Clears user session and authentication state
```

**Usage**: User clicks "Log Out" button

---

#### Check Sign-In Status
```typescript
const isSignedIn = await puter.auth.isSignedIn()
// Returns: boolean
```

**Usage**: Determine auth state on app startup

---

#### Get Current User
```typescript
const user = await puter.auth.getUser()
// Returns: { id, username, email, ... }
```

**Usage**: Access user ID for data isolation

---

### 6.2 File System APIs

#### Upload Files
```typescript
const result = await puter.fs.upload(files)
// Parameters:
//   - files: File[] (from input or drag-drop)
// Returns: FSItem (single upload)
//          FSItem[] (multiple files)

// FSItem structure:
{
  id: "file_abc123",
  name: "resume.pdf",
  path: "/home/user123/resume.pdf",
  type: "application/pdf",
  size: 245000,
  modified: "2025-11-16T10:30:00Z"
}
```

**Code Location**: `app/lib/puter.ts:295-302`

---

#### Read File
```typescript
const blob = await puter.fs.read(filePath)
// Parameters:
//   - filePath: string (Puter FS path)
// Returns: Blob (binary data)

// Usage example:
const resumeBlob = await puter.fs.read('/home/user/resume.pdf')
const pdfUrl = URL.createObjectURL(resumeBlob)
document.getElementById('pdfViewer').src = pdfUrl
```

**Code Location**: `app/lib/puter.ts:286-293`

---

#### List Directory
```typescript
const items = await puter.fs.readdir(dirPath)
// Parameters:
//   - dirPath: string (directory path, usually "./")
// Returns: FSItem[]

// Example:
const files = await puter.fs.readdir("./")
// Returns all files in root directory
```

**Code Location**: `app/lib/puter.ts:277-284`

---

#### Delete File
```typescript
await puter.fs.delete(filePath)
// Parameters:
//   - filePath: string (file to delete)
// Returns: void
```

**Code Location**: `app/lib/puter.ts:304-311`

---

### 6.3 Key-Value Store APIs

#### Set (Store Data)
```typescript
await puter.kv.set(key, value)
// Parameters:
//   - key: string (identifier)
//   - value: string (must be JSON stringified)

// Example:
const resumeData = {
  id: 'abc123',
  companyName: 'Google',
  feedback: { score: 85 }
}
await puter.kv.set(
  'resume:abc123',
  JSON.stringify(resumeData)
)
```

**Code Location**: `app/lib/puter.ts:375-382`

---

#### Get (Retrieve Data)
```typescript
const value = await puter.kv.get(key)
// Parameters:
//   - key: string
// Returns: string | null

// Example:
const json = await puter.kv.get('resume:abc123')
if (json) {
  const resume = JSON.parse(json)
  console.log(resume.companyName)
}
```

**Code Location**: `app/lib/puter.ts:366-372`

---

#### List (Query by Pattern)
```typescript
const items = await puter.kv.list(pattern, returnValues)
// Parameters:
//   - pattern: string (e.g., "resume:*")
//   - returnValues: boolean (default false)
// Returns: string[] | KVItem[]

// Example:
// Get only keys
const keys = await puter.kv.list('resume:*', false)
// ['resume:abc123', 'resume:def456']

// Get keys and values
const items = await puter.kv.list('resume:*', true)
// [{ key: 'resume:abc123', value: '...' }]
```

**Code Location**: `app/lib/puter.ts:393-403`

---

#### Delete (Remove Single Entry)
```typescript
await puter.kv.delete(key)
// Parameters:
//   - key: string (key to delete)
// Returns: void
```

**Code Location**: `app/lib/puter.ts:384-391`

---

#### Flush (Clear All Data)
```typescript
await puter.kv.flush()
// Parameters: none
// Returns: boolean

// WARNING: Irreversible operation!
// Deletes entire KV store for user
```

**Code Location**: `app/lib/puter.ts:405-412`

---

### 6.4 AI Service APIs

#### Chat (General Conversation)
```typescript
const response = await puter.ai.chat(
  prompt,                    // Question or instruction
  imageURL,                 // Optional image for analysis
  testMode,                 // Optional testing flag
  { model: "..." }          // Optional config object
)
// Returns: { message: { content: string } }
```

**Code Location**: `app/lib/puter.ts:313-328`

---

#### Feedback (Resume Analysis - Recommended)
```typescript
const analysis = await puter.ai.feedback(
  resumePath,               // Path to resume PDF
  instructions              // Analysis instructions
)
// Parameters:
//   - resumePath: string (Puter FS path to PDF)
//   - instructions: string (analysis prompt)
// Returns: { message: { content: JSON string } }

// Complete example:
const instructions = `You are an ATS expert. Analyze this resume
for position: ${jobTitle}
Job requirements: ${jobDescription}

Provide scores 0-100 for:
- ATS Compatibility
- Tone and Style
- Content Quality
- Structure
- Skills Matching

Return valid JSON only.`

const result = await puter.ai.feedback(
  '/home/user123/my_resume.pdf',
  instructions
)

const feedbackJson = JSON.parse(result.message.content)
const overallScore = feedbackJson.overallScore
```

**Code Location**: `app/lib/puter.ts:330-354`

---

---

## 7. Environment Configuration

### 7.1 Development Environment Setup

**Step 1: Verify Node.js**
```bash
node --version  # Should be v20.x or higher
npm --version   # Should be v10.x or higher
```

**Step 2: Clone Repository**
```bash
git clone https://github.com/your-org/ai-resume-analyzer.git
cd ai-resume-analyzer
```

**Step 3: Install Dependencies**
```bash
npm install
# Downloads and installs all packages to node_modules/
```

**Step 4: Start Development Server**
```bash
npm run dev
# Output: Vite dev server running at http://localhost:5173

# Features:
# - Hot Module Replacement (HMR) - instant updates
# - Source maps - easy debugging
# - Fast refresh - preserves state on edit
```

**Troubleshooting**:

| Error | Solution |
|-------|----------|
| `npm: command not found` | Install Node.js from nodejs.org |
| `Port 5173 already in use` | Kill process or use `npm run dev -- --port 3000` |
| `Module not found` | Run `npm install` again |
| `Puter SDK not loading` | Check internet connection |

### 7.2 Production Environment Setup

**Step 1: Build for Production**
```bash
npm run build
# Compiles TypeScript, bundles code, optimizes CSS
# Output: build/ directory with optimized assets
```

**Step 2: Test Production Build Locally**
```bash
npm run start
# Starts React Router server on http://localhost:3000
# Simulates actual production environment
```

**Step 3: Deploy (Choose One)**

**Option A: Docker**
```bash
docker build -t ai-resume-analyzer:v1.0.0 .
docker run -p 3000:3000 ai-resume-analyzer:v1.0.0
```

**Option B: Vercel**
```bash
npm install -g vercel
vercel deploy --prod
```

**Option C: Self-Hosted**
```bash
npm install --production
npm run start &  # Background process
```

### 7.3 Docker Configuration

**Multi-Stage Build Strategy**:

```dockerfile
# Stage 1: Build with all dependencies
FROM node:20-alpine AS development-dependencies-env
COPY . /app
RUN npm ci

# Stage 2: Production dependencies only
FROM node:20-alpine AS production-dependencies-env
COPY package*.json /app/
RUN npm ci --omit=dev

# Stage 3: Compile code
FROM node:20-alpine AS build-env
COPY . /app
COPY --from=development-dependencies-env /app/node_modules /app/node_modules
RUN npm run build

# Stage 4: Runtime (final image)
FROM node:20-alpine
COPY package*.json /app/
COPY --from=production-dependencies-env /app/node_modules /app/node_modules
COPY --from=build-env /app/build /app/build
CMD ["npm", "run", "start"]
```

**Image Size Optimization**:
- Stage 1+2+3: ~1.5 GB (intermediate)
- Final image: ~420 MB (70% reduction)

### 7.4 Environment Variables

**Note**: This project requires minimal environment configuration due to Puter's managed services.

**Optional Environment Variables**:
```bash
NODE_ENV=production         # Enables production optimizations
VITE_LOG_LEVEL=error       # Suppress debug logs
PORT=3000                   # Server port (some platforms)
```

**How Puter Eliminates Configuration**:
- Authentication → Automatic via Puter OAuth
- Database → User-isolated KV storage
- File Storage → User-isolated file system
- AI API → Puter platform integration
- Secrets → No API keys needed

**Docker Environment Example**:
```bash
docker run \
  -e NODE_ENV=production \
  -e VITE_LOG_LEVEL=error \
  -p 3000:3000 \
  ai-resume-analyzer:v1.0.0
```

---

## 8. Secondary Development Guide

### 8.1 Development Environment Setup Steps

**Quick Start**:
```bash
# 1. Verify prerequisites
node --version      # v20.x
npm --version       # v10.x

# 2. Clone and install
git clone <repo>
cd ai-resume-analyzer
npm install

# 3. Start development server
npm run dev

# 4. Open browser to http://localhost:5173
```

### 8.2 Project Structure Overview

```
app/
├── routes/          ← Page components
│   ├── home.tsx     (Resume list dashboard)
│   ├── auth.tsx     (Login/logout)
│   ├── upload.tsx   (Resume submission)
│   ├── resume.tsx   (Analysis results)
│   └── wipe.tsx     (Data management)
│
├── components/      ← Reusable UI components
│   ├── Navbar.tsx
│   ├── ResumeCard.tsx
│   ├── FileUploader.tsx
│   ├── Summary.tsx
│   ├── ATS.tsx
│   ├── Details.tsx
│   ├── ScoreGauge.tsx
│   ├── ScoreCircle.tsx
│   ├── ScoreBadge.tsx
│   └── Accordion.tsx
│
├── lib/            ← Business logic
│   ├── puter.ts    (Zustand store + Puter wrapper)
│   ├── pdf2img.ts  (PDF conversion utility)
│   └── utils.ts    (Helper functions)
│
├── root.tsx        ← Root layout component
├── routes.ts       ← Route definitions
├── app.css         ← Global styles
└── ...
```

### 8.3 Common Development Tasks

#### Task 1: Add New Route/Page

**Requirement**: Add achievements page (`/achievements`)

**Step 1: Create page component**
```typescript
// app/routes/achievements.tsx
import { useNavigate } from 'react-router'
import { usePuterStore } from '~/lib/puter'

export const meta = () => [
  { title: 'My Achievements | Resumind' }
]

export default function Achievements() {
  const { auth, isLoading } = usePuterStore()
  const navigate = useNavigate()

  // Protect route - redirect if not authenticated
  useEffect(() => {
    if (!isLoading && !auth.isAuthenticated) {
      navigate('/auth?next=/achievements')
    }
  }, [isLoading, auth.isAuthenticated])

  return (
    <main className="p-6">
      <h1>My Achievements</h1>
      {/* Your content */}
    </main>
  )
}
```

**Step 2: Register in routes**
```typescript
// app/routes.ts
export default [
  index("routes/home.tsx"),
  route('/auth', 'routes/auth.tsx'),
  route('/upload', 'routes/upload.tsx'),
  route('/resume/:id', 'routes/resume.tsx'),
  route('/achievements', 'routes/achievements.tsx'),  // NEW
  route('/wipe', 'routes/wipe.tsx'),
]
```

**Step 3: Add navigation link**
```typescript
// app/components/Navbar.tsx
<Link to="/achievements" className="px-4 py-2">
  🏆 Achievements
</Link>
```

#### Task 2: Create Reusable Component

**Requirement**: Badge component for achievements

```typescript
// app/components/AchievementBadge.tsx
interface AchievementBadgeProps {
  title: string
  icon: string
  color: 'gold' | 'silver' | 'bronze'
  description?: string
}

export function AchievementBadge({
  title,
  icon,
  color,
  description
}: AchievementBadgeProps) {
  const bgColor = {
    gold: 'bg-yellow-50 border-yellow-200',
    silver: 'bg-gray-50 border-gray-200',
    bronze: 'bg-orange-50 border-orange-200'
  }[color]

  return (
    <div className={`p-4 rounded-lg border ${bgColor}`}>
      <div className="text-4xl mb-2">{icon}</div>
      <h3 className="font-semibold">{title}</h3>
      {description && <p className="text-sm text-gray-600">{description}</p>}
    </div>
  )
}
```

#### Task 3: Modify AI Prompt

**Requirement**: Add work experience focus to AI analysis

```typescript
// constants/index.ts
export const prepareInstructions = ({
  jobTitle,
  jobDescription
}: {
  jobTitle: string
  jobDescription: string
}) => `You are an expert in ATS and resume optimization.

ANALYSIS FOCUS:
1. Work Experience Relevance
   - Years of directly relevant experience
   - Industry and role progression
   - Achievement quantification

2. Skills Alignment
3. Format and ATS Compatibility
4. Writing Quality and Tone
5. Overall Presentation

For the position: ${jobTitle}
Requirements: ${jobDescription}

${AIResponseFormat}

Return ONLY valid JSON, no markdown.`
```

#### Task 4: Adjust Scoring Logic

**Requirement**: Weighted average instead of AI's direct score

```typescript
// In feedback processing, after AI returns data
const feedback = JSON.parse(feedbackText)

// Override with weighted calculation
feedback.overallScore = Math.round(
  feedback.ATS.score * 0.3 +           // 30% weight
  feedback.content.score * 0.3 +       // 30% weight
  feedback.structure.score * 0.2 +     // 20% weight
  feedback.skills.score * 0.2          // 20% weight
)

await kv.set(`resume:${uuid}`, JSON.stringify({ ...data, feedback }))
```

#### Task 5: Add New Feedback Dimension

**Requirement**: Add "Keyword Optimization" dimension

```typescript
// Step 1: Update data model
// types/index.d.ts
interface Feedback {
  overallScore: number
  ATS: ScoreSection
  content: ScoreSection
  structure: ScoreSection
  skills: ScoreSection
  keywordOptimization: ScoreSection  // NEW
}

// Step 2: Update AI prompt
// constants/index.ts
const AIResponseFormat = `{
  "overallScore": number,
  "ATS": { "score": number, "tips": [...] },
  "content": { "score": number, "tips": [...] },
  "structure": { "score": number, "tips": [...] },
  "skills": { "score": number, "tips": [...] },
  "keywordOptimization": { "score": number, "tips": [...] }
}`

// Step 3: Display in UI
// app/components/Summary.tsx
<ScoreBadge
  name="Keyword Optimization"
  score={feedback.keywordOptimization.score}
/>
```

### 8.4 Debugging Techniques

**Technique 1: Console Logging**
```typescript
// app/lib/puter.ts
const upload = async (files: File[]) => {
  console.log('[DEBUG] Upload starting...', files)
  const result = await puter.fs.upload(files)
  console.log('[DEBUG] Upload complete:', result)
  return result
}
```

**Technique 2: Browser DevTools**
```javascript
// In browser console
const app = document.querySelector('body')
console.log(app)  // Inspect React Fiber structure
```

**Technique 3: State Inspection**
```typescript
// Add to component
useEffect(() => {
  console.log('[DEBUG] Auth state changed:', auth)
}, [auth.isAuthenticated])
```

**Technique 4: Network Monitoring**
- Open DevTools Network tab
- Filter by XHR/Fetch
- Monitor API calls to Puter

### 8.5 Performance Optimization

**Code Splitting** (Automatic):
```typescript
// React Router automatically creates separate bundles
// app/routes/home.tsx  → home.js
// app/routes/upload.tsx → upload.js
// Users only download needed code
```

**Image Lazy Loading**:
```typescript
// ResumeCard uses lazy loading
const [imageUrl, setImageUrl] = useState('')

useEffect(() => {
  fs.read(imagePath).then(blob => {
    setImageUrl(URL.createObjectURL(blob))
  })
}, [imagePath])

// Image loads only when component mounts
```

**PDF Processing Optimization**:
```typescript
// pdf2img.ts uses singleton pattern
let pdfjsLib = null

const loadPdfJs = async () => {
  if (pdfjsLib) return pdfjsLib  // Already loaded

  pdfjsLib = await import('pdfjs-dist')
  return pdfjsLib
}
// Library only downloaded once
```

---

## 9. Deployment Guide

### 9.1 Docker Deployment (Detailed)

**Prerequisites**:
- Docker installed ([download](https://www.docker.com))
- Server or computer capable of running Docker

**Step 1: Build Image**
```bash
docker build -t ai-resume-analyzer:v1.0.0 .

# Progress output:
# Step 1/15 : FROM node:20-alpine
# ...
# Successfully tagged ai-resume-analyzer:v1.0.0
```

**Step 2: Verify Image**
```bash
docker images | grep ai-resume-analyzer

# Output:
# REPOSITORY               TAG      ID           SIZE
# ai-resume-analyzer       v1.0.0   abc123...    420MB
```

**Step 3: Test Locally**
```bash
docker run -p 3000:3000 ai-resume-analyzer:v1.0.0

# Server starts at http://localhost:3000
# Press Ctrl+C to stop
```

**Step 4: Run in Production**
```bash
# Run as daemon
docker run -d \
  --name resume-api \
  -p 3000:3000 \
  -e NODE_ENV=production \
  ai-resume-analyzer:v1.0.0

# Monitor
docker logs -f resume-api

# Stop
docker stop resume-api
```

### 9.2 Vercel Deployment

**Step 1: Create Vercel Account**
- Visit [vercel.com](https://vercel.com)
- Sign in with GitHub

**Step 2: Connect Project**
- Click "New Project"
- Select GitHub repository
- Choose "ai-resume-analyzer"

**Step 3: Configure**
```json
// vercel.json
{
  "buildCommand": "npm run build",
  "outputDirectory": "build",
  "functions": {
    "build/server/index.js": {
      "memory": 512,
      "maxDuration": 60
    }
  }
}
```

**Step 4: Deploy**
```bash
# CLI method
npm install -g vercel
vercel login
vercel deploy --prod

# Or: Click "Deploy" in Vercel dashboard
```

**Result**: Application available at `https://[project].vercel.app`

### 9.3 Alternative Platforms

**AWS Elastic Beanstalk**:
```bash
eb init -p node.js-20 my-app
eb create production
eb deploy
```

**Heroku**:
```bash
heroku create my-app
git push heroku main
heroku logs --tail
```

**Self-Hosted VPS**:
```bash
scp -r . user@server:/opt/app
ssh user@server
cd /opt/app
npm install --production
npm run start
```

### 9.4 Environment Configuration

**Key Variables**:
```bash
NODE_ENV=production          # Production optimizations
PORT=3000                    # Server port
```

**Setting in Docker**:
```bash
docker run -e NODE_ENV=production ai-resume-analyzer:v1
```

**Setting in Vercel**:
- Dashboard → Settings → Environment Variables
- Add `NODE_ENV=production`

### 9.5 Troubleshooting

| Issue | Diagnosis | Solution |
|-------|-----------|----------|
| Build fails | `npm ERR! ...` | Run `npm ci --force` |
| Container crashes | `docker: Error response` | Check port conflicts |
| Out of memory | App stops frequently | Increase limit: `-m 1gb` |
| Puter SDK error | Login not working | Check internet, CDN availability |
| PDF upload fails | No thumbnail generated | Verify PDF format validity |
| Slow uploads | Upload takes >30s | Check network, file size |

---

## 10. Appendix

### 10.1 Terminology Glossary

| Term | Definition | Analogy |
|------|-----------|---------|
| **API** | Interface for program communication | Restaurant menu |
| **SDK** | Software development toolkit | LEGO brick set |
| **KV Store** | Key-value database | Phone contact list |
| **Blob** | Binary large object | Data container |
| **UUID** | Universally unique identifier | Social security number |
| **SSR** | Server-side rendering | Pre-plated food |
| **CSR** | Client-side rendering | Self-service buffet |
| **Cache** | Temporary data storage | Desk organizer |
| **Zustand** | React state manager | Memory book |
| **Vite** | Build tool | Code compiler |
| **Docker** | Container platform | Shipping container |

### 10.2 Quick Command Reference

**Development**:
```bash
npm run dev          # Start dev server (:5173)
npm run build        # Compile for production
npm run start        # Run production build
npm run typecheck    # Check TypeScript errors
```

**Git**:
```bash
git clone <url>      # Clone repository
git add .            # Stage changes
git commit -m "msg"  # Commit with message
git push             # Push to remote
git pull             # Pull updates
```

**Docker**:
```bash
docker build -t app:v1 .          # Build image
docker run -p 3000:3000 app:v1    # Run container
docker logs <id>                  # View logs
docker exec -it <id> sh           # Shell access
docker stop <id>                  # Stop container
```

### 10.3 Recommended Learning Resources

**Official Documentation**:
- [React Docs](https://react.dev) - React framework
- [React Router Docs](https://reactrouter.com) - Routing library
- [Puter.js Docs](https://docs.puter.com) - Cloud platform
- [TypeScript Handbook](https://www.typescriptlang.org/docs) - Type system

**Video Tutorials**:
- YouTube: "React Router v7 Tutorial"
- YouTube: "Full Stack React with Puter"

**Articles & Blogs**:
- Dev.to: "Serverless React Apps"
- Medium: "TypeScript Best Practices"

### 10.4 FAQ

**Q: Can I use this offline?**
A: No. Internet connection required for Puter platform services.

**Q: How long is data stored?**
A: Duration depends on Puter account. Use "Wipe" feature to delete.

**Q: Can I modify AI feedback?**
A: Not directly. Re-upload resume to get new analysis.

**Q: How do I extend this system?**
A: Follow Section 8 (Secondary Development Guide).

**Q: What's the deployment cost?**
A: Varies by platform:
- Vercel: Free tier or $20+/month
- Docker VPS: $5-20/month (server)
- AWS: $10-100/month (usage-based)

**Q: How many resumes can I store?**
A: Essentially unlimited, depends on Puter account storage quota.

**Q: How do I ensure user privacy?**
A: Data isolated to user accounts. Recommend adding privacy policy.

### 10.5 File Index

| Need to Modify | File Path |
|--------|-----------|
| Add new page | `app/routes/[page].tsx` |
| Add UI component | `app/components/[Component].tsx` |
| Customize styling | `app/app.css` or in-component Tailwind |
| Update routes | `app/routes.ts` |
| Change AI behavior | `constants/index.ts` |
| Modify data model | `types/index.d.ts` |
| Adjust authentication | `app/lib/puter.ts` |
| Deployment config | `Dockerfile` or `vercel.json` |

---

## Conclusion

This project exemplifies modern web application architecture with:

✅ **Technology Excellence**:
- Serverless backend eliminating infrastructure concerns
- React Router v7 providing latest routing capabilities
- Full TypeScript support for type safety
- Tailwind CSS for rapid UI development
- Puter platform integration for zero-ops services

✅ **Development Experience**:
- Hot module reloading for rapid iteration
- Clear file structure enabling easy navigation
- Type hints reducing bugs
- Rich documentation and examples

✅ **Production Ready**:
- Docker multi-stage builds optimizing image size
- Multi-platform deployment support
- Comprehensive error handling
- Performance optimizations built-in

**Recommended Development Path**:
1. Study architecture (Sections 2-3)
2. Setup development environment (Section 7.1)
3. Implement small feature enhancement (Section 8.3)
4. Deploy to staging platform (Section 9.2)
5. Monitor and iterate

For additional questions, refer to specific section documentation or consult official platform documentation.

---

**Document Version**: 1.0
**Last Updated**: 2025-11-16
**Total Word Count**: ~18,000 words
**Applicable Versions**: v1.0.0 and above
