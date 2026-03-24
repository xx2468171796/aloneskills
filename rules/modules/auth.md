# 模块: 认证 (Authentication)

> **模块 ID**: `auth`
> **依赖**: core, db
> **版本**: 见 `@rules/versions.md`

## 技术选型

| 技术 | 用途 |
|------|------|
| **Auth.js (NextAuth)** | 身份认证框架 |
| **Session** | 服务端 Session，存储于 Redis |
| **OAuth** | 第三方登录（GitHub, Google 等） |

## 约束

- Session 存储于外部 Redis，不用 JWT
- 认证检查封装到 `lib/auth/`，不在页面里直接写
- 敏感操作必须二次验证

## 目录结构

```
src/lib/auth/
├── config.ts              # Auth.js 配置
├── session.ts             # Session 工具函数
└── index.ts

src/app/(auth)/
├── login/page.tsx
├── register/page.tsx
└── layout.tsx
```

## 环境变量

```env
AUTH_SECRET="your-auth-secret-min-32-chars"
NEXTAUTH_URL="http://localhost:3000"
GITHUB_CLIENT_ID=""
GITHUB_CLIENT_SECRET=""
```
