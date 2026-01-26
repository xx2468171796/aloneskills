# 项目规则与约束

## 代码风格

### 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| 变量/函数 | camelCase | `isUserLoggedIn`, `getUserById` |
| 类/类型/接口 | PascalCase | `UserProfile`, `CreateUserInput` |
| 常量 | UPPER_SNAKE_CASE | `MAX_PAGE_SIZE`, `API_TIMEOUT` |
| 文件 (组件) | kebab-case | `user-profile.tsx`, `data-table.tsx` |
| 文件 (工具) | camelCase | `formatDate.ts`, `validateEmail.ts` |

### 代码质量

- **命名即文档** - 变量名必须语义化 (`isUserLoggedIn` 而非 `flag`)
- **严禁魔法数字** - 常量必须有明确命名
- **严禁 `any`** - 全链路类型安全
- **童子军军规** - 修改代码时顺手修复周围问题

## 技术约束

### ✅ 必须遵守

| 规则 | 说明 |
|------|------|
| Server Actions | 所有后端交互必须用 `"use server"` |
| HeroUI First | 未明确指定时优先使用 HeroUI 组件 |
| Tailwind CSS | 样式必须使用 Tailwind |
| Schema First | 数据库变更必须先修改 `schema.prisma` |
| Zod 验证 | 所有输入必须 Zod 验证 |
| Repository 模式 | 数据库操作封装到 Repository |

### ❌ 严格禁止

| 规则 | 说明 |
|------|------|
| NO API Routes | 严禁创建 `/pages/api` 或 `/app/api` |
| No CSS Files | 严禁创建 `.css`/`.scss`/`.less` |
| No Node.js in UI | `.tsx` 中严禁 import `fs`/`path`/`os` |
| No Zustand for Server Data | Zustand 仅用于 UI 状态 |
| No any | 严禁使用 `any` 类型 |
| No 硬编码密钥 | 敏感信息必须走环境变量 |

## 架构原则

### 原子化设计 (Atomic Design)

```
components/
├── ui/           # Atoms - 基础 UI 元素
├── shared/       # Molecules - 组合组件
└── features/     # Organisms - 业务模块
```

- 组件不超过 **150 行**
- 超过则拆分为子组件

### 单一职责 (SRP)

| 层级 | 职责 |
|------|------|
| UI 组件 | 只负责展示 |
| Hooks | 封装业务逻辑 |
| Server Actions | 数据获取和处理 |
| Repository | 数据库操作 |

### 关注点分离 (SoC)

- ❌ 严禁在 UI 层直接编写 SQL
- ❌ 严禁在 UI 层编写复杂算法
- ✅ 复杂逻辑抽离到 `lib/` 或 Hooks

### DRY / YAGNI / KISS

- **DRY** - 逻辑重复 2 次必须重构
- **YAGNI** - 只实现当前需求，严禁过度封装
- **KISS** - 可读性 > 炫技

## Git 提交规范

### 提交信息格式

```
type(scope): message

# 示例
feat(auth): add user registration
fix(ui): correct button hover state
refactor(db): extract repository pattern
docs(readme): update installation guide
chore(deps): upgrade dependencies
```

### 类型说明

| 类型 | 说明 |
|------|------|
| `feat` | 新功能 |
| `fix` | Bug 修复 |
| `refactor` | 重构 (不改变行为) |
| `docs` | 文档变更 |
| `chore` | 构建/依赖/配置 |
| `test` | 测试相关 |
| `style` | 格式调整 |

## 应用到本项目的规则路径

将通用技术栈规则落地到具体项目时，建议在项目根目录创建如下目录结构，并把规则文件放入其中：

```
<project-root>/
└── .windsurf/
    └── rules/
        └── 1.md
```

### 推荐移动/复制方式

```bash
# 在目标项目根目录执行
mkdir -p .windsurf/rules
cp aloneskills/stacks/nextjs-fullstack/rules.md .windsurf/rules/1.md
```

> 说明：`.windsurf/rules/1.md` 为项目级规则入口文件，可按需拆分为多条规则文件。

## 文件组织

### Server Actions

```
src/actions/
├── auth/
│   ├── login.ts
│   └── register.ts
├── user/
│   ├── create.ts
│   └── update.ts
└── index.ts
```

### Zod Schemas

```
src/schemas/
├── auth.ts
├── user.ts
└── index.ts
```

### Repositories

```
src/lib/db/
├── client.ts          # Prisma Client
├── repositories/
│   ├── user.ts
│   └── order.ts
└── index.ts
```

## 测试要求

| 模块类型 | 最低覆盖率 |
|----------|------------|
| 工具函数 | 90% |
| Zod Schemas | 90% |
| Server Actions | 70% |
| UI 组件 | 50% |
