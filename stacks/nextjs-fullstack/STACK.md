---
name: nextjs-fullstack
description: Next.js 全栈项目预设。激活所有模块的完整技术栈配置。
---

# Next.js 全栈技术栈（预设）

> 这是一个**预设 (Preset)**，等价于在 `stack.config.json` 中开启所有模块。
> 版本号见 `@rules/versions.md`，核心约束见 `@rules/core.md`。

## 对应的 stack.config.json

```json
{
  "name": "fullstack-project",
  "modules": {
    "db": true,
    "auth": true,
    "desktop": true,
    "ai-services": false,
    "testing": true
  }
}
```

## 包含的规则文件

| 类型 | 文件 | 说明 |
|------|------|------|
| **必选** | `rules/core.md` | 核心基座 |
| **必选** | `rules/ai-behavior.md` | AI 助手行为 |
| **必选** | `rules/coding-standards.md` | 编码规范 |
| **必选** | `rules/versions.md` | 版本清单 |
| **模块** | `rules/modules/db.md` | Prisma + PostgreSQL + Redis |
| **模块** | `rules/modules/auth.md` | Auth.js 认证 |
| **模块** | `rules/modules/desktop.md` | Tauri 桌面端 |
| **模块** | `rules/modules/testing.md` | Vitest + Playwright + Storybook |

## 包含的技能

| 技能 | 说明 |
|------|------|
| `ui-components` | UI 组件开发 |
| `ui-ux-pro-max` | UI/UX 设计 |
| `db-operations` | 数据库操作 |
| `auth` | 用户认证 |
| `rbac` | 权限控制 |
| `testing` | 测试规范 |
| `build-standards` | 构建规范 |
| `feature-evaluation` | 功能评估 |
| `skill-creator` | 技能创建 |

## 快速启动

```bash
# 1. 复制规则到新项目
cp -r aloneskills/rules/ <project>/rules/

# 2. 使用全栈预设
cp aloneskills/stacks/nextjs-fullstack/stack.config.json <project>/rules/stack.config.json

# 3. 初始化项目
pnpm create next-app@latest --app --tailwind --typescript --src-dir

# 4. 初始化 shadcn/ui
pnpm dlx shadcn@latest init

# 5. 安装核心依赖
pnpm add zustand @tanstack/react-query react-hook-form zod lucide-react

# 5. 安装后端依赖
pnpm add prisma @prisma/client next-auth @auth/prisma-adapter

# 6. 安装开发工具
pnpm add -D turbo @biomejs/biome vitest @playwright/test storybook
```
