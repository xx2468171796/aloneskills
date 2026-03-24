# 编码规范 (Coding Standards)

> **适用**: 所有项目必选。与技术栈无关的通用编码规则。

---

## 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| 变量/函数 | camelCase | `isUserLoggedIn`, `getUserById` |
| 类/类型/接口 | PascalCase | `UserProfile`, `CreateUserInput` |
| 常量 | UPPER_SNAKE_CASE | `MAX_PAGE_SIZE`, `API_TIMEOUT` |
| 文件 (组件) | kebab-case | `user-profile.tsx`, `data-table.tsx` |
| 文件 (工具) | camelCase | `formatDate.ts`, `validateEmail.ts` |

---

## 架构原则

### 原子化设计 (Atomic Design)

```
components/
├── ui/           # Atoms - 基础 UI 元素
├── shared/       # Molecules - 组合组件
└── features/     # Organisms - 业务模块
```

- 组件不超过 **150 行**，超过则拆分

### 单一职责 (SRP)

| 层级 | 职责 |
|------|------|
| UI 组件 | 只负责展示 |
| Hooks | 封装业务逻辑 |
| Server Actions | 数据获取和处理 |
| Repository | 数据库操作 |

### 关注点分离 (SoC)

- 严禁在 UI 层直接编写 SQL
- 严禁在 UI 层编写复杂算法
- 复杂逻辑抽离到 `lib/` 或 Hooks

### 依赖倒置 (DIP)

- 外部服务必须使用 Adapter 模式

### DRY / YAGNI / KISS

- **DRY** — 逻辑重复 2 次必须抽离
- **YAGNI** — 只实现当前需求，严禁过度封装
- **KISS** — 可读性 > 炫技

---

## 代码质量

- **命名即文档** — 变量名必须语义化（`isUserLoggedIn` 而非 `flag`）
- **严禁魔法数字** — 常量必须有明确命名
- **严禁 `any`** — 全链路类型安全
- **童子军军规** — 修改代码时顺手修复周边类型/格式问题

---

## Git 提交规范

```
type(scope): message
```

| 类型 | 说明 | 示例 |
|------|------|------|
| `feat` | 新功能 | `feat(auth): add user registration` |
| `fix` | Bug 修复 | `fix(ui): correct button hover state` |
| `refactor` | 重构 | `refactor(db): extract repository pattern` |
| `docs` | 文档 | `docs(readme): update installation guide` |
| `chore` | 构建/依赖 | `chore(deps): upgrade dependencies` |
| `test` | 测试 | `test(auth): add login unit tests` |
| `style` | 格式 | `style(ui): fix indentation` |

---

## 测试覆盖率要求

| 模块类型 | 最低覆盖率 |
|----------|------------|
| 工具函数 | 90% |
| Zod Schemas | 90% |
| Server Actions | 70% |
| UI 组件 | 50% |
