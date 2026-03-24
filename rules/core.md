# 核心基座 (Core Foundation)

> **适用**: 所有项目必选。版本号见 `@rules/versions.md`。

---

## 技术基座

| 层级 | 技术 | 说明 |
|------|------|------|
| **框架** | Next.js (App Router) | Server Components + Turbopack |
| **UI** | React | Server + Client Components |
| **语言** | TypeScript (Strict) | 全链路类型安全，严禁 `any` |
| **样式** | Tailwind CSS (Oxide) | 禁止 `.css`/`.scss`/`.less` 文件 |
| **组件库** | shadcn/ui | 默认组件库，CLI v4 按需安装组件 |
| **状态** | Zustand (UI) + TanStack Query (服务端) | 严格分离客户端/服务端状态 |
| **验证** | Zod | 全链路数据验证 |
| **表单** | React Hook Form + Zod | 表单状态管理 |
| **图标** | Lucide React | 统一图标方案 |
| **包管理** | pnpm (Workspaces) | 严格依赖管理 |
| **构建** | Turborepo + Turbopack | Monorepo 增量构建 |
| **Lint** | Biome | Lint + Format + Import 排序 |

---

## 强制约束

### 必须遵守

| 规则 | 说明 |
|------|------|
| **Server Actions** | 所有后端交互用 `"use server"`，**严禁 API Routes** |
| **shadcn/ui First** | 统一使用 shadcn/ui 组件库 |
| **Tailwind Only** | 样式必须用 Tailwind，禁止 CSS 文件 |
| **Zod 验证** | 所有外部输入必须经过 Zod 验证 |
| **TypeScript Strict** | `strict: true`，`noUncheckedIndexedAccess: true` |

### 严格禁止

| 规则 | 说明 |
|------|------|
| **NO API Routes** | 严禁 `/pages/api` 或 `/app/api` |
| **NO CSS Files** | 严禁 `.css`/`.scss`/`.less`（`globals.css` 除外） |
| **NO any** | 严禁 `any` 类型 |
| **NO useEffect fetch** | 数据获取用 TanStack Query，不用 useEffect |
| **NO 硬编码密钥** | 敏感信息必须走环境变量 |
| **NO Zustand for Server Data** | Zustand 仅用于 UI 状态 |

---

## 目录结构（基座）

```text
/
├── package.json            # pnpm Workspaces Root
├── pnpm-workspace.yaml     # pnpm Workspace 配置
├── turbo.json              # Turborepo Config
├── biome.json              # Biome Config
├── .env.example            # 环境变量模板
│
├── apps/
│   └── <app-name>/         # 应用（名称按项目定）
│       ├── src/
│       │   ├── app/                # Next.js App Router
│       │   ├── components/
│       │   │   ├── ui/             # Atoms - shadcn/ui 组件
│       │   │   ├── shared/         # Molecules - 通用组合
│       │   │   └── features/       # Organisms - 业务模块
│       │   ├── actions/            # Server Actions
│       │   ├── hooks/              # Custom Hooks
│       │   ├── lib/                # 工具函数 + 外部服务适配
│       │   ├── schemas/            # Zod Schemas
│       │   ├── store/              # Zustand Stores
│       │   └── types/              # TypeScript 类型
│       └── .env
│
│
└── packages/               # 共享包
    ├── ui/                 # 共享 UI 组件
    ├── config/             # 共享 TSConfig
    └── utils/              # 共享工具函数
```

---

## 配置模板

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
    "paths": { "@/*": ["./src/*"] }
  }
}
```

### biome.json

```json
{
  "organizeImports": { "enabled": true },
  "linter": { "enabled": true, "rules": { "recommended": true } },
  "formatter": { "enabled": true, "indentStyle": "space", "indentWidth": 2 }
}
```

### pnpm-workspace.yaml

```yaml
packages:
  - "apps/*"
  - "packages/*"
```

---

## React 19 最佳实践

- 表单提交使用 `action` 属性
- 使用 `useOptimistic` 实现 0 延迟反馈
- 使用 `useActionState` 管理 Server Action 状态
- 使用 Tailwind 响应式前缀做响应式设计
