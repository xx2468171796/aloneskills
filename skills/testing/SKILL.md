---
name: testing
description: 测试规范技能。在需要编写单元测试、集成测试、E2E 测试、组件测试时使用此技能。适用于 Vitest + Playwright + React Testing Library 技术栈。
---

# 测试技能 (Testing Skill)

编写测试相关代码时，遵循以下规范。

## 适用技术栈

> 版本号见 `@rules/versions.md` | 所需模块: `testing`

- **Vitest** - 单元测试 / 集成测试
- **Playwright** - E2E 测试
- **React Testing Library** - 组件测试
- **MSW** - API Mock

## 测试目录结构

```
src/
├── lib/
│   ├── utils.ts
│   └── utils.test.ts        # 就近测试文件
├── actions/
│   ├── create.ts
│   └── create.test.ts       # Server Action 测试
tests/
├── e2e/                     # E2E 测试
│   ├── auth.spec.ts
│   └── crud.spec.ts
└── fixtures/                # 测试数据
    └── data.json
```

## 单元测试

### 配置 Vitest

```typescript
// vitest.config.ts
import { defineConfig } from "vitest/config"
import react from "@vitejs/plugin-react"
import { resolve } from "path"

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    globals: true,
    setupFiles: ["./tests/setup.ts"],
  },
  resolve: {
    alias: {
      "@": resolve(__dirname, "./src"),
    },
  },
})
```

### 工具函数测试

```typescript
// src/lib/utils.test.ts
import { describe, it, expect } from "vitest"
import { formatCurrency, validateEmail } from "./utils"

describe("formatCurrency", () => {
  it("should format number to currency", () => {
    expect(formatCurrency(1234.56)).toBe("$1,234.56")
  })

  it("should handle zero", () => {
    expect(formatCurrency(0)).toBe("$0.00")
  })
})
```

### Zod Schema 测试

```typescript
// src/schemas/user.test.ts
import { describe, it, expect } from "vitest"
import { createUserSchema } from "./user"

describe("createUserSchema", () => {
  it("should validate correct data", () => {
    const result = createUserSchema.safeParse({
      name: "John",
      email: "john@example.com",
    })
    expect(result.success).toBe(true)
  })

  it("should reject empty name", () => {
    const result = createUserSchema.safeParse({
      name: "",
      email: "test@example.com",
    })
    expect(result.success).toBe(false)
  })
})
```

## E2E 测试

### 配置 Playwright

```typescript
// playwright.config.ts
import { defineConfig, devices } from "@playwright/test"

export default defineConfig({
  testDir: "./tests/e2e",
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  reporter: "html",
  use: {
    baseURL: "http://localhost:3000",
    trace: "on-first-retry",
  },
  projects: [
    { name: "chromium", use: { ...devices["Desktop Chrome"] } },
  ],
  webServer: {
    command: "pnpm dev",
    url: "http://localhost:3000",
    reuseExistingServer: !process.env.CI,
  },
})
```

### E2E 测试示例

```typescript
// tests/e2e/auth.spec.ts
import { test, expect } from "@playwright/test"

test.describe("Authentication", () => {
  test("should login with valid credentials", async ({ page }) => {
    await page.goto("/login")

    await page.fill('input[name="email"]', "user@example.com")
    await page.fill('input[name="password"]', "password123")
    await page.click('button[type="submit"]')

    await expect(page).toHaveURL("/dashboard")
  })
})
```

## 运行测试

```bash
# 单元测试
pnpm test

# 监听模式
pnpm test -- --watch

# 覆盖率
pnpm test -- --coverage

# E2E 测试
pnpm playwright test

# E2E UI 模式
pnpm playwright test --ui
```

## 覆盖率要求

| 模块类型 | 最低覆盖率 |
|----------|------------|
| 工具函数 | 90% |
| Zod Schemas | 90% |
| Server Actions | 70% |
| UI 组件 | 50% |

## 核心规范

1. **测试文件就近放置** - `.test.ts` 与源文件同目录
2. **Mock 外部依赖** - 数据库、Auth、外部 API
3. **测试隔离** - 每个测试独立，不依赖执行顺序
4. **清晰的测试名称** - 使用 `should...when...` 格式
5. **覆盖边界情况** - 空值、异常、极端值
