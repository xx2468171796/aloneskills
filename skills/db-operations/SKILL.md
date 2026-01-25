---
name: db-operations
description: 数据库操作技能。在需要进行数据库查询、创建、更新、删除、迁移等操作时使用此技能。适用于 Prisma ORM + PostgreSQL + Repository 模式 + Zod 验证。
---

# 数据库操作技能 (Database Operations Skill)

执行数据库相关操作时，遵循以下流程和规范。

## 适用技术栈

- **Prisma** v5/v6+ - ORM 框架
- **PostgreSQL** / MySQL / SQLite - 数据库
- **Multi-Schema** - 子系统隔离 (可选)
- **Zod** - 输入验证
- **Repository Pattern** - 数据访问封装

## 数据库连接

```env
# .env
DATABASE_URL="postgres://user:password@host:port/database"
```

## Prisma Schema 示例

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(cuid())
  email     String   @unique
  password  String
  role      String   @default("user")
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  deletedAt DateTime?
}

model Post {
  id        String   @id @default(cuid())
  title     String
  content   String?
  authorId  String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  deletedAt DateTime?
}
```

## 数据库操作流程

### 1. Schema 变更

```bash
# 修改 schema.prisma 后
npx prisma migrate dev --name <migration-name>
npx prisma generate
```

### 2. Repository 模式

```typescript
// src/lib/db/repositories/user.repository.ts
import { prisma } from "@/lib/db"
import { Prisma } from "@prisma/client"

export class UserRepository {
  async findMany(where?: Prisma.UserWhereInput) {
    return prisma.user.findMany({
      where: {
        ...where,
        deletedAt: null, // 默认排除已删除
      },
      orderBy: { createdAt: "desc" },
    })
  }

  async findById(id: string) {
    return prisma.user.findUnique({
      where: { id },
    })
  }

  async create(data: Prisma.UserCreateInput) {
    return prisma.user.create({ data })
  }

  async update(id: string, data: Prisma.UserUpdateInput) {
    return prisma.user.update({
      where: { id },
      data,
    })
  }

  async softDelete(id: string) {
    return prisma.user.update({
      where: { id },
      data: { deletedAt: new Date() },
    })
  }
}

export const userRepository = new UserRepository()
```

### 3. Server Action 调用

```typescript
// src/actions/user/create.ts
"use server"

import { userRepository } from "@/lib/db/repositories/user.repository"
import { createUserSchema } from "@/schemas/user"
import { revalidatePath } from "next/cache"

export async function createUser(formData: FormData) {
  const validated = createUserSchema.safeParse({
    name: formData.get("name"),
    email: formData.get("email"),
  })

  if (!validated.success) {
    return { error: validated.error.flatten() }
  }

  await userRepository.create(validated.data)

  revalidatePath("/users")
  return { success: true }
}
```

## Zod Schema 定义

```typescript
// src/schemas/user.ts
import { z } from "zod"

export const createUserSchema = z.object({
  name: z.string().min(1, "名称不能为空"),
  email: z.string().email("请输入有效邮箱"),
})

export const updateUserSchema = createUserSchema.partial()

export type CreateUserInput = z.infer<typeof createUserSchema>
export type UpdateUserInput = z.infer<typeof updateUserSchema>
```

## 事务处理

```typescript
// 多表操作使用事务
await prisma.$transaction(async (tx) => {
  const order = await tx.order.create({ data: orderData })
  
  await tx.orderItem.createMany({
    data: items.map(item => ({
      orderId: order.id,
      ...item,
    })),
  })
  
  return order
})
```

## 核心规范

1. **Schema First** - 先改 schema.prisma，再生成迁移
2. **Repository 封装** - 禁止在组件中直接使用 Prisma
3. **Zod 验证** - 所有输入必须验证
4. **软删除** - 使用 deletedAt，禁止物理删除业务数据
5. **类型安全** - 使用 Prisma 生成的类型

## 禁止事项

- ❌ 直接在组件中调用 prisma
- ❌ 不经过 Zod 验证直接插入数据
- ❌ 物理删除用户数据
- ❌ 硬编码数据库连接字符串
- ❌ 跳过迁移直接修改数据库
