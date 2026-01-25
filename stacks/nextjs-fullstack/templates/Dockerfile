# ===========================================
# Next.js Standalone Dockerfile
# ===========================================

FROM node:20-alpine AS base

# ===========================================
# 依赖安装阶段
# ===========================================
FROM base AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app

# 复制依赖文件
COPY package.json yarn.lock* ./
RUN corepack enable && yarn install --immutable

# ===========================================
# 构建阶段
# ===========================================
FROM base AS builder
WORKDIR /app

COPY --from=deps /app/node_modules ./node_modules
COPY . .

# 禁用遥测
ENV NEXT_TELEMETRY_DISABLED=1

# 生成 Prisma Client
RUN npx prisma generate

# 构建应用
RUN corepack enable && yarn build

# ===========================================
# 生产运行阶段
# ===========================================
FROM base AS runner
WORKDIR /app

ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

# 创建非 root 用户
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

# 复制静态资源
COPY --from=builder /app/public ./public

# 设置权限
RUN mkdir .next
RUN chown nextjs:nodejs .next

# 复制构建产物
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

EXPOSE 3000
ENV PORT=3000
ENV HOSTNAME="0.0.0.0"

CMD ["node", "server.js"]
