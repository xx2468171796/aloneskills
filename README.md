# Alone Skills

模块化 AI 技能库 + 技术栈规则体系。为 AI 编码助手（Claude Code / Windsurf / Cursor）提供可复用的专业知识和工作流。

基于 [Agent Skills](https://agentskills.io/) 开放标准，兼容 [oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode) 多智能体编排。

## 核心特性

- **模块化规则** — 按需组合 `db` / `auth` / `desktop` / `ai-services` / `testing` 模块
- **版本集中管理** — `versions.md` 唯一真相源，改一处全局生效
- **配置驱动** — `stack.config.json` 一个文件选择项目需要的模块
- **9 个专业技能** — UI/UX、数据库、认证、测试、构建等
- **技能自动路由** — AI 根据任务自动激活对应技能，感知模块开关

## 适用技术栈

| 层级 | 技术 |
|------|------|
| **框架** | Next.js 16 + React 19 + TypeScript 6.0 |
| **UI** | Tailwind CSS v4 + shadcn/ui |
| **状态** | Zustand v5 + TanStack Query v5 |
| **后端** | Server Actions + Prisma v7 + Zod v4 |
| **认证** | Auth.js v5 |
| **桌面端** | Tauri v2（可选） |
| **构建** | pnpm v10 + Turborepo v2.8 + Biome v2.3 |
| **测试** | Vitest v4 + Playwright |

完整版本清单见 [`rules/versions.md`](./rules/versions.md)

## 快速开始

### 1. 复制规则到新项目

```bash
# 克隆仓库
git clone https://github.com/xx2468171796/aloneskills.git

# 复制规则目录到目标项目
cp -r aloneskills/rules/ <your-project>/rules/
cp aloneskills/CLAUDE.md <your-project>/CLAUDE.md
```

### 2. 选择模块

编辑 `rules/stack.config.json`，按需开启模块：

```json
{
  "name": "my-project",
  "modules": {
    "db": true,
    "auth": true,
    "desktop": false,
    "ai-services": false,
    "testing": true
  }
}
```

### 3. 开始开发

AI 助手自动读取 `CLAUDE.md` → 加载 `rules/core.md` → 检查 `stack.config.json` → 只遵循已激活模块的规则。

### 预设

| 预设 | 说明 | 配置 |
|------|------|------|
| [`nextjs-fullstack`](./stacks/nextjs-fullstack/) | 全栈预设（db + auth + desktop + testing） | [`stack.config.json`](./stacks/nextjs-fullstack/stack.config.json) |

## 目录结构

```
aloneskills/
├── CLAUDE.md                    # 项目指令（AI 助手自动加载）
├── manifest.json                # 技能清单 v2.0
├── package.json                 # pnpm 配置
├── pnpm-workspace.yaml
│
├── rules/                       # 模块化规则体系
│   ├── versions.md              # 版本唯一真相源
│   ├── core.md                  # 核心基座（必选）
│   ├── ai-behavior.md           # AI 行为准则（必选）
│   ├── coding-standards.md      # 编码规范（必选）
│   ├── stack.config.json        # 模块开关
│   ├── stack.config.schema.json # JSON Schema
│   └── modules/                 # 可选模块
│       ├── db.md                # Prisma + PostgreSQL + Redis
│       ├── auth.md              # Auth.js 认证
│       ├── desktop.md           # Tauri 桌面端
│       ├── ai-services.md       # AI API 集成
│       └── testing.md           # Vitest + Playwright
│
├── skills/                      # AI 技能
│   ├── ui-ux-pro-max/          # UI/UX 设计智能
│   ├── ui-components/          # UI 组件开发
│   ├── db-operations/          # 数据库操作
│   ├── auth/                   # 用户认证
│   ├── rbac/                   # 权限控制
│   ├── testing/                # 测试规范
│   ├── build-standards/        # 构建规范
│   ├── feature-evaluation/     # 功能评估
│   └── skill-creator/          # 技能创建指南
│
└── stacks/                      # 技术栈预设
    └── nextjs-fullstack/
        ├── STACK.md
        ├── stack.config.json
        └── templates/
```

## 技能列表

| 技能 | 描述 | 所需模块 |
|------|------|----------|
| [`ui-ux-pro-max`](./skills/ui-ux-pro-max/SKILL.md) | UI/UX 设计智能 | core |
| [`ui-components`](./skills/ui-components/SKILL.md) | UI 组件开发 (shadcn/ui) | core |
| [`db-operations`](./skills/db-operations/SKILL.md) | 数据库操作 (Prisma + Repository) | `db` |
| [`auth`](./skills/auth/SKILL.md) | 用户认证 (Auth.js + Session) | `auth` + `db` |
| [`rbac`](./skills/rbac/SKILL.md) | 角色权限控制 | `auth` |
| [`testing`](./skills/testing/SKILL.md) | 测试规范 (Vitest + Playwright) | `testing` |
| [`build-standards`](./skills/build-standards/SKILL.md) | 打包构建规范 | core |
| [`feature-evaluation`](./skills/feature-evaluation/SKILL.md) | 新功能评估 | core |
| [`skill-creator`](./skills/skill-creator/SKILL.md) | 创建新技能 | core |

## 在不同工具中使用

### Claude Code / OMC

将 `CLAUDE.md` 放在项目根目录，Claude Code 启动时自动加载。

```bash
cp aloneskills/CLAUDE.md <project>/CLAUDE.md
cp -r aloneskills/rules/ <project>/rules/
```

### Windsurf

```bash
mkdir -p .windsurf/workflows .windsurf/rules
cp aloneskills/skills/*/SKILL.md .windsurf/workflows/
cp aloneskills/rules/core.md .windsurf/rules/core.md
cp aloneskills/rules/coding-standards.md .windsurf/rules/coding-standards.md
```

### Cursor

```bash
cp aloneskills/rules/core.md .cursor/rules/core.md
cp aloneskills/rules/coding-standards.md .cursor/rules/coding-standards.md
```

## 技能工作原理

```
1. 发现 — AI 启动时加载 stack.config.json + 技能清单
       ↓
2. 路由 — 任务匹配技能触发词，检查模块是否激活
       ↓
3. 激活 — 加载对应 SKILL.md，引用 versions.md 获取版本
       ↓
4. 执行 — 按指令执行，遵循 core.md + coding-standards.md 约束
```

## 参考

- [Agent Skills 官方文档](https://agentskills.io/)
- [Agent Skills 规范](https://agentskills.io/specification)
- [oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode)
- [shadcn/ui](https://ui.shadcn.com/)
- [shadcn-admin](https://github.com/satnaing/shadcn-admin)

## 许可证

MIT License
