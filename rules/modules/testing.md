# 模块: 测试 (Testing)

> **模块 ID**: `testing`
> **依赖**: core
> **版本**: 见 `@rules/versions.md`

## 技术选型

| 技术 | 用途 |
|------|------|
| **Vitest** | 单元测试 + 组件测试 |
| **Playwright** | E2E 浏览器测试 |
| **Storybook** | 组件开发 + 视觉测试 |

## 约束

- 所有工具函数和 Zod Schema **必须有单元测试**
- E2E 测试覆盖核心用户流程
- Storybook 运行于 `packages/ui`，组件独立开发

## 覆盖率要求

| 模块类型 | 最低覆盖率 |
|----------|------------|
| 工具函数 | 90% |
| Zod Schemas | 90% |
| Server Actions | 70% |
| UI 组件 | 50% |

## 目录结构

```
src/
├── __tests__/              # 集成测试
├── components/
│   └── ui/
│       └── button.test.tsx # 组件测试紧邻组件

tests/
├── e2e/                    # Playwright E2E
│   ├── auth.spec.ts
│   └── ...
└── setup.ts                # 测试全局配置

packages/ui/
└── .storybook/             # Storybook 配置
```

## 命令

```bash
pnpm test              # 运行 Vitest
pnpm test:e2e          # 运行 Playwright
pnpm storybook         # 启动 Storybook
```
