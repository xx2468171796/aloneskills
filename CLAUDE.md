# Voice-to-MindMap 项目指令

## 项目概述

语音转思维导图工具 — 录制客户对话，上传音频，AI 转写 + 结构化，生成可编辑思维导图。
面向台湾软件定制化市场，帮助顾问快速把客户需求对话转化为结构化文档。

## 技术栈（快查）

- **框架**: Next.js 16 + React 19 + TypeScript 6.0
- **UI**: Tailwind CSS v4 + shadcn/ui (CLI v4)
- **状态**: Zustand v5 + TanStack Query v5
- **验证**: Zod v4 + React Hook Form v7
- **AI**: Groq Whisper (STT) + Claude API (结构化)
- **图表**: next-ai-draw-io (思维导图)
- **构建**: pnpm v10 + Turborepo v2.8 + Biome v2.3
- **测试**: Vitest v4 + Playwright

完整版本清单: `rules/versions.md`

## 模块化规则系统

配置文件: `rules/stack.config.json`

**当前激活模块**: `ai-services`, `testing`
**未激活（MVP 后按需开启）**: `db`, `auth`, `desktop`

### 规则加载顺序

1. `rules/core.md` — 核心基座
2. `rules/ai-behavior.md` — AI 行为准则
3. `rules/coding-standards.md` — 编码规范
4. `rules/stack.config.json` — 检查激活模块
5. 激活模块对应的 `rules/modules/*.md`
6. `rules/技能自动评估规则.md` — 技能路由

## OMC 智能体路由

本项目使用 oh-my-claudecode 多智能体编排。任务分配优先级：

| 任务类型 | 推荐智能体 | 模型 |
|----------|-----------|------|
| AI 服务集成 (Groq/Claude API) | `document-specialist` → `executor` | sonnet |
| 架构决策 | `architect` / `analyst` | opus |
| UI 页面开发 | `designer` → `executor` | sonnet |
| 代码审查 | `code-reviewer` | opus |
| Bug 排查 | `debugger` / `tracer` | sonnet |
| 测试编写 | `test-engineer` | sonnet |
| 代码精简 | `code-simplifier` | opus |
| 安全审查 | `security-reviewer` | sonnet |
| Git 操作 | `git-master` | sonnet |

### OMC 技能映射

| 场景 | OMC 技能 |
|------|----------|
| 多步骤实现 | `/ralph` 或 `/autopilot` |
| 并行任务 | `/ultrawork` 或 `/team` |
| 计划评审 | `/plan --consensus` |
| 代码清理 | `/ai-slop-cleaner` |
| 深度分析 | `/sciomc` |

## 强制约束

- **Server Actions only** — 严禁 API Routes
- **shadcn/ui** — 默认组件库
- **Tailwind only** — 禁止 CSS 文件
- **Zod v4** — 全链路验证
- **TypeScript Strict** — 严禁 `any`
- **pnpm** — 唯一包管理器
- **AI API 在 Server Actions 中调用** — 严禁客户端直接调用
- **Adapter 模式** — AI 服务封装到 `src/lib/ai/providers/`

## 目录结构

```
apps/<app>/src/
├── app/                    # Next.js App Router
├── components/
│   ├── ui/                 # shadcn/ui 组件
│   ├── shared/             # 通用组合组件
│   └── features/           # 业务模块
├── actions/
│   └── ai/                 # AI 相关 (transcribe, structure)
├── hooks/
├── lib/
│   ├── ai/
│   │   ├── providers/      # Claude / Groq Adapter
│   │   └── prompts/        # 提示词模板
│   └── utils/
├── schemas/                # Zod Schemas
├── store/                  # Zustand Stores
└── types/
```

## 沟通语言

中文沟通。代码和变量名英文，注释和文档中文。

## 常用命令

```bash
pnpm dev                    # 开发服务器
pnpm build                  # 生产构建
pnpm test                   # Vitest 测试
pnpm lint                   # Biome 检查
pnpm dlx shadcn@latest add  # 添加组件
```
