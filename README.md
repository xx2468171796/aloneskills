# Alone Skills

基于 [Agent Skills](https://agentskills.io/) 开放标准的 AI 技能库，为 AI 助手提供专业领域知识和可复用工作流。

## 📦 包含技能

| 技能 | 描述 | 适用场景 |
|------|------|----------|
| [`ui-ux-pro-max`](./skills/ui-ux-pro-max/SKILL.md) | UI/UX 设计智能 | 设计、配色、字体、布局 |
| [`skill-creator`](./skills/skill-creator/SKILL.md) | 技能创建指南 | 创建新技能 |
| [`testing`](./skills/testing/SKILL.md) | 测试规范 | 单元/E2E/组件测试 |
| [`ui-components`](./skills/ui-components/SKILL.md) | UI 组件开发 | React + shadcn/ui |
| [`build-standards`](./skills/build-standards/SKILL.md) | 构建规范 | Next.js + Tauri |
| [`db-operations`](./skills/db-operations/SKILL.md) | 数据库操作 | Prisma + Repository |
| [`auth`](./skills/auth/SKILL.md) | 用户认证 | Auth.js + Session |
| [`rbac`](./skills/rbac/SKILL.md) | 权限控制 | RBAC 模式 |
| [`feature-evaluation`](./skills/feature-evaluation/SKILL.md) | 功能评估 | 开发前评估 |

## 🚀 快速开始

### 在项目中使用

1. 克隆仓库到项目目录：

```bash
git clone https://github.com/xx2468171796/aloneskills.git skills
```

2. 或作为 Git Submodule：

```bash
git submodule add https://github.com/xx2468171796/aloneskills.git skills
```

### 在 Windsurf 中使用

将技能复制到 `.windsurf/workflows/` 目录：

```bash
cp -r skills/*/SKILL.md .windsurf/workflows/
```

### 📦 移植到项目的路径映射

后续移植只需阅读本仓库并按以下路径拷贝到目标项目：

| 资源 | 仓库路径 | 目标项目路径 |
|------|----------|--------------|
| 技能 | `skills/<skill>/SKILL.md` | `.windsurf/workflows/<skill>.md` |
| 通用规则 | `stacks/nextjs-fullstack/rules.md` | `.windsurf/rules/project-rules.md` |

示例（在目标项目根目录执行）：

```bash
mkdir -p .windsurf/workflows .windsurf/rules
cp aloneskills/skills/*/SKILL.md .windsurf/workflows/
cp aloneskills/stacks/nextjs-fullstack/rules.md .windsurf/rules/project-rules.md
```

## 📖 技能工作原理

```
┌─────────────────────────────────────────────────────────────────┐
│  1. 发现 (Discovery)                                            │
│     AI 启动时加载所有技能的 name + description                   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  2. 激活 (Activation)                                           │
│     当任务匹配技能描述时，加载完整 SKILL.md 指令                   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  3. 执行 (Execution)                                            │
│     AI 按指令执行，按需加载 references/ 中的详细文档               │
└─────────────────────────────────────────────────────────────────┘
```

## 📁 目录结构

```
aloneskills/
├── README.md
├── manifest.json           # 技能清单
├── LICENSE
└── skills/
    ├── ui-ux-pro-max/
    │   ├── SKILL.md
    │   └── references/
    ├── skill-creator/
    │   ├── SKILL.md
    │   └── references/
    └── .../
```

## 🎯 适用技术栈

这些技能针对以下技术栈优化：

- **前端**: React / Next.js + Tailwind CSS + shadcn/ui
- **后端**: Server Actions / Prisma
- **认证**: Auth.js (NextAuth)
- **桌面端**: Tauri v2
- **测试**: Vitest + Playwright

## 🛠️ 技术栈模板

除了技能，本仓库还提供开箱即用的技术栈配置：

| 技术栈 | 说明 |
|--------|------|
| [`nextjs-fullstack`](./stacks/nextjs-fullstack/STACK.md) | Next.js 16 + React 19 + Tailwind + shadcn/ui + Prisma + Auth.js |

### 技术栈内容

```
stacks/nextjs-fullstack/
├── STACK.md              # 技术栈说明
├── rules.md              # 项目规则与约束
└── templates/
    ├── env.example       # 环境变量模板
    └── prisma-schema.prisma # Prisma 模板
```

## 📄 规范参考

- [Agent Skills 官方文档](https://agentskills.io/)
- [Agent Skills 规范](https://agentskills.io/specification)
- [示例技能](https://github.com/anthropics/skills)

## 📜 许可证

MIT License
