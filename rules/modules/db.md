# 模块: 数据库 (Database)

> **模块 ID**: `db`
> **依赖**: core
> **版本**: 见 `@rules/versions.md`

## 技术选型

| 技术 | 用途 |
|------|------|
| **Prisma** | ORM，Schema First |
| **PostgreSQL** | 主数据库 |
| **Redis** | 缓存 / Session / 队列 |

## 约束

- **Schema First** — 变更数据库先改 `schema.prisma`，再 `pnpm prisma migrate dev`
- **Repository 模式** — 数据库操作封装到 Repository，禁止在 UI 层直接写 SQL
- **Zod 共用** — Server Action 输入与表单共用 Zod Schema
- **连接外部化** — 通过环境变量连接，不包含 Docker 编排

## 目录结构

```
src/lib/db/
├── client.ts              # Prisma Client 实例
├── repositories/
│   ├── user.ts            # UserRepository
│   └── ...
└── index.ts               # 统一导出

prisma/
└── schema.prisma          # 数据库 Schema
```

## 环境变量

```env
DATABASE_URL="postgresql://user:pass@host:5432/dbname?schema=public"
REDIS_URL="redis://:password@host:6379/0"
```

## Prisma Client 模板

```typescript
// src/lib/db/client.ts
import { PrismaClient } from "@prisma/client"

const globalForPrisma = globalThis as unknown as { prisma: PrismaClient }

export const prisma = globalForPrisma.prisma ?? new PrismaClient()

if (process.env.NODE_ENV !== "production") globalForPrisma.prisma = prisma
```
