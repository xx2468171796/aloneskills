---
name: build-standards
description: 打包构建规范技能。在需要配置开发环境、生产构建、Docker 镜像、桌面端、环境变量等时使用此技能。适用于 Next.js + Turbopack + Docker + Tauri 技术栈。
---

# 打包构建技能 (Build Standards Skill)

配置构建和部署相关功能时，遵循以下规范。

## 适用技术栈

- **Turbopack** / Webpack - 开发环境构建
- **Next.js Standalone** - 生产构建
- **Tauri v2** - 桌面端构建
- **Docker** - 容器化部署
- **pnpm** / Yarn / npm - 包管理

## 开发环境

### 启动开发服务器

```json
// package.json
{
  "scripts": {
    "dev": "next dev --turbo",
    "build": "next build",
    "start": "next start"
  }
}
```

```bash
# 启动开发 (推荐使用 --turbo)
npm run dev
```

### Next.js 配置

```typescript
// next.config.ts
import type { NextConfig } from "next"

const nextConfig: NextConfig = {
  output: "standalone", // 生产构建使用 standalone
  experimental: {
    serverActions: {
      bodySizeLimit: "2mb",
    },
  },
}

export default nextConfig
```

## 生产构建

### Standalone 模式

```bash
# 构建
npm run build

# 输出目录
.next/standalone/
├── server.js          # 入口文件
├── .next/             # 静态资源
└── node_modules/      # 最小化依赖
```

### Docker 构建

```dockerfile
# Dockerfile
FROM node:20-alpine AS base

# 依赖安装
FROM base AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

# 构建
FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# 生产镜像
FROM base AS runner
WORKDIR /app
ENV NODE_ENV=production

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs
EXPOSE 3000
ENV PORT=3000

CMD ["node", "server.js"]
```

```bash
# 构建镜像
docker build -t myapp:latest .

# 运行容器
docker run -p 3000:3000 --env-file .env myapp:latest
```

## Tauri 桌面端

### 项目结构

```
src-tauri/
├── src/
│   ├── lib.rs         # Tauri 命令定义
│   └── main.rs        # 入口
├── Cargo.toml
└── tauri.conf.json
```

### Rust Command 示例

```rust
// src-tauri/src/lib.rs
use tauri::command;
use std::fs;

#[command]
fn read_file(path: String) -> Result<String, String> {
    fs::read_to_string(&path).map_err(|e| e.to_string())
}

#[command]
fn write_file(path: String, content: String) -> Result<(), String> {
    fs::write(&path, content).map_err(|e| e.to_string())
}
```

### 前端调用

```typescript
// src/lib/tauri.ts
import { invoke } from "@tauri-apps/api/core"

export async function readLocalFile(path: string): Promise<string> {
  return invoke("read_file", { path })
}
```

### 构建桌面端

```bash
# 开发模式
npm run tauri dev

# 生产构建
npm run tauri build
```

## 环境变量

### 文件结构

```
.env.example      # 模板（提交到 Git）
.env.local        # 本地配置（不提交）
.env.production   # 生产配置（不提交）
```

### 环境变量模板

```env
# .env.example

# Database
DATABASE_URL=postgres://user:password@host:port/database

# Redis
REDIS_URL=redis://:password@host:port

# Auth
AUTH_SECRET=your-auth-secret
NEXTAUTH_URL=http://localhost:3000
```

### 类型安全环境变量

```typescript
// src/lib/env.ts
import { z } from "zod"

const envSchema = z.object({
  DATABASE_URL: z.string().url(),
  REDIS_URL: z.string().url(),
  AUTH_SECRET: z.string().min(32),
  NEXTAUTH_URL: z.string().url(),
})

export const env = envSchema.parse(process.env)
```

## 核心规范

1. **开发推荐用 Turbopack** - `next dev --turbo`
2. **生产用 Standalone** - `output: 'standalone'`
3. **Docker 多阶段构建** - 最小化镜像
4. **文件操作用 Tauri** - 桌面端禁止 Node.js fs
5. **环境变量 Zod 验证** - 类型安全
