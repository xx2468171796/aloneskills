---
name: nextjs-fullstack
description: Next.js 全栈项目技术栈配置。适用于 Next.js 16 + React 19 + Tailwind CSS v4 + shadcn/ui + Prisma + Auth.js 的现代全栈项目。
---

# Next.js 全栈技术栈

现代 React 全栈项目的标准技术栈配置。

## 技术栈组成

### 前端 (Frontend)

| 技术 | 版本 | 用途 |
|------|------|------|
| **Next.js** | 16+ | App Router + Server Components |
| **React** | 19 | UI 框架 |
| **TypeScript** | 5.x | 类型安全 (Strict Mode) |
| **Tailwind CSS** | v4 | 样式引擎 |
| **shadcn/ui** | latest | 组件库 |
| **Lucide React** | latest | 图标库 |
| **Zustand** | v5 | 客户端状态管理 |
| **TanStack Query** | v5 | 服务端状态管理 |
| **React Hook Form** | latest | 表单处理 |
| **Zod** | latest | 数据验证 |

### 后端 (Backend)

| 技术 | 版本 | 用途 |
|------|------|------|
| **Server Actions** | - | 去 API 化设计 |
| **Prisma** | v6+ | ORM |
| **PostgreSQL** | v17 | 主数据库 |
| **Auth.js** | v5 | 身份认证 |
| **Redis** | v7 | 缓存 / Session |

### 构建 (Build)

| 技术 | 版本 | 用途 |
|------|------|------|
| **Turbopack** | - | 开发构建 |
| **Standalone** | - | 生产构建 |
| **Docker** | - | 容器化 |
| **Tauri** | v2 | 桌面端 (可选) |

### 开发工具 (DevTools)

| 技术 | 版本 | 用途 |
|------|------|------|
| **pnpm/yarn** | latest | 包管理 |
| **Biome** | latest | Lint + Format |
| **Vitest** | latest | 单元测试 |
| **Playwright** | latest | E2E 测试 |
| **Husky** | latest | Git Hooks |
| **Commitlint** | latest | 提交规范 |

## 项目初始化

### 1. 创建项目

```bash
npx create-next-app@latest my-app --typescript --tailwind --app --src-dir
cd my-app
```

### 2. 安装依赖

```bash
# 核心依赖
pnpm add @tanstack/react-query zustand zod react-hook-form @hookform/resolvers

# Prisma
pnpm add -D prisma
pnpm add @prisma/client

# Auth.js
pnpm add next-auth@beta @auth/prisma-adapter

# 工具
pnpm add bcryptjs
pnpm add -D @types/bcryptjs
```

### 3. 配置 shadcn/ui

```bash
npx shadcn@latest init
npx shadcn@latest add button input card form dialog table toast
```

### 4. 配置 Biome

```bash
pnpm add -D @biomejs/biome
npx biome init
```

### 5. 配置 Vitest

```bash
pnpm add -D vitest @vitejs/plugin-react jsdom @testing-library/react
```

## 目录结构

```
src/
├── app/                    # Next.js App Router
│   ├── (auth)/            # 认证相关页面
│   ├── (dashboard)/       # 仪表板页面
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── ui/                # shadcn/ui 组件
│   ├── shared/            # 业务通用组件
│   └── features/          # 业务特定组件
├── actions/               # Server Actions
├── hooks/                 # Custom Hooks
├── lib/
│   ├── db/               # Prisma Client + Repositories
│   ├── auth/             # Auth.js 配置
│   └── utils/            # 工具函数
├── schemas/               # Zod Schemas
├── store/                 # Zustand Stores
└── types/                 # TypeScript 类型
prisma/
└── schema.prisma
```

## 配置文件模板

### next.config.ts

```typescript
import type { NextConfig } from "next"

const nextConfig: NextConfig = {
  output: "standalone",
  experimental: {
    serverActions: {
      bodySizeLimit: "2mb",
    },
  },
}

export default nextConfig
```

### tsconfig.json

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

### biome.json

```json
{
  "organizeImports": { "enabled": true },
  "linter": {
    "enabled": true,
    "rules": { "recommended": true }
  },
  "formatter": {
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 2
  }
}
```

## 环境变量模板

```env
# .env.example

# Database
DATABASE_URL="postgres://user:password@localhost:5432/mydb"

# Redis
REDIS_URL="redis://localhost:6379"

# Auth
AUTH_SECRET="your-auth-secret-min-32-chars"
NEXTAUTH_URL="http://localhost:3000"

# Optional: OAuth
GITHUB_CLIENT_ID=""
GITHUB_CLIENT_SECRET=""
GOOGLE_CLIENT_ID=""
GOOGLE_CLIENT_SECRET=""
```

## 相关技能

使用此技术栈时，激活以下技能：

- `ui-components` - UI 组件开发
- `ui-ux-pro-max` - UI/UX 设计
- `db-operations` - 数据库操作
- `auth` - 用户认证
- `testing` - 测试规范
- `build-standards` - 构建规范
