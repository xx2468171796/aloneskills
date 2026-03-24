---
name: auth
description: 用户认证与会话管理技能。在需要实现登录、注册、登出、Session 管理、OAuth 集成、密码重置等功能时使用此技能。适用于 Auth.js (NextAuth) v5 + Session 管理。
---

# 用户认证技能 (Auth Skill)

实现用户认证相关功能时，遵循以下流程和规范。

## 适用技术栈

> 版本号见 `@rules/versions.md` | 所需模块: `auth` + `db`

- **Auth.js** (NextAuth) - 身份认证框架
- **Redis** - Session 存储
- **Zod** - 输入验证
- **Prisma** - 用户数据存储
- **Server Actions** - 后端逻辑

## 核心流程

### 用户注册

```typescript
// src/actions/auth/register.ts
"use server"

import { registerSchema } from "@/schemas/auth"
import { prisma } from "@/lib/db"
import { hash } from "bcryptjs"

export async function register(formData: FormData) {
  const validated = registerSchema.safeParse({
    email: formData.get("email"),
    password: formData.get("password"),
  })
  
  if (!validated.success) {
    return { error: validated.error.flatten() }
  }
  
  const hashedPassword = await hash(validated.data.password, 12)
  
  await prisma.user.create({
    data: {
      email: validated.data.email,
      password: hashedPassword,
    },
  })
  
  return { success: true }
}
```

### 用户登录

```typescript
// src/actions/auth/login.ts
"use server"

import { signIn } from "@/lib/auth"
import { loginSchema } from "@/schemas/auth"

export async function login(formData: FormData) {
  const validated = loginSchema.safeParse({
    email: formData.get("email"),
    password: formData.get("password"),
  })
  
  if (!validated.success) {
    return { error: "Invalid credentials" }
  }
  
  await signIn("credentials", {
    email: validated.data.email,
    password: validated.data.password,
    redirectTo: "/dashboard",
  })
}
```

## Auth.js 配置

```typescript
// src/lib/auth.ts
import NextAuth from "next-auth"
import Credentials from "next-auth/providers/credentials"
import { prisma } from "@/lib/db"
import { compare } from "bcryptjs"

export const { handlers, auth, signIn, signOut } = NextAuth({
  providers: [
    Credentials({
      credentials: {
        email: { label: "Email", type: "email" },
        password: { label: "Password", type: "password" },
      },
      async authorize(credentials) {
        const user = await prisma.user.findUnique({
          where: { email: credentials.email as string },
        })
        
        if (!user) return null
        
        const isValid = await compare(
          credentials.password as string,
          user.password
        )
        
        if (!isValid) return null
        
        return { id: user.id, email: user.email, role: user.role }
      },
    }),
  ],
  callbacks: {
    async session({ session, token }) {
      if (token) {
        session.user.id = token.sub!
        session.user.role = token.role as string
      }
      return session
    },
    async jwt({ token, user }) {
      if (user) {
        token.role = user.role
      }
      return token
    },
  },
})
```

## Zod Schema 模板

```typescript
// src/schemas/auth.ts
import { z } from "zod"

export const loginSchema = z.object({
  email: z.string().email("请输入有效的邮箱地址"),
  password: z.string().min(8, "密码至少 8 位"),
})

export const registerSchema = loginSchema.extend({
  password: z
    .string()
    .min(8, "密码至少 8 位")
    .regex(/[A-Z]/, "需要包含大写字母")
    .regex(/[0-9]/, "需要包含数字"),
  confirmPassword: z.string(),
}).refine((data) => data.password === data.confirmPassword, {
  message: "两次密码不一致",
  path: ["confirmPassword"],
})
```

## 路由守卫中间件

```typescript
// src/middleware.ts
import { auth } from "@/lib/auth"
import { NextResponse } from "next/server"

const protectedRoutes = ["/dashboard", "/settings"]

export default auth((req) => {
  const { pathname } = req.nextUrl
  const session = req.auth
  
  if (protectedRoutes.some(r => pathname.startsWith(r))) {
    if (!session) {
      return NextResponse.redirect(new URL("/login", req.url))
    }
  }
  
  return NextResponse.next()
})

export const config = {
  matcher: ["/((?!api|_next/static|_next/image|favicon.ico).*)"],
}
```

## 约束规则

1. **严禁使用 API Routes** - 使用 Server Actions
2. **密码必须加密存储** - 使用 bcryptjs hash
3. **Session 使用安全存储** - Redis 或数据库
4. **输入必须 Zod 验证** - 前后端共用 Schema
5. **敏感信息走环境变量** - 禁止硬编码
