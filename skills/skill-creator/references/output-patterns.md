# 输出模式

## 模板模式

当输出需要严格格式时使用：

```markdown
## 模板

[字段名]: [值]
[字段名]: [值]
```

## 示例模式

当需要展示预期输出时使用：

```markdown
## 示例

输入: "用户登录"
输出: 
- 验证邮箱格式
- 检查用户存在
- 验证密码
- 创建 Session
```

## 代码模式

### TypeScript 函数

```typescript
/**
 * 函数描述
 * @param param1 - 参数1描述
 * @returns 返回值描述
 */
export function functionName(param1: Type): ReturnType {
  // 实现
}
```

### Server Action

```typescript
"use server"

import { z } from "zod"

const schema = z.object({
  field: z.string(),
})

export async function actionName(formData: FormData) {
  const validated = schema.safeParse({
    field: formData.get("field"),
  })
  
  if (!validated.success) {
    return { error: validated.error.flatten() }
  }
  
  // 业务逻辑
  
  return { success: true }
}
```

### React 组件

```tsx
interface Props {
  prop1: string
  prop2?: number
}

export function ComponentName({ prop1, prop2 = 0 }: Props) {
  return (
    <div className="...">
      {/* 内容 */}
    </div>
  )
}
```

## 检查清单模式

```markdown
## 检查清单

- [ ] 项目 1
- [ ] 项目 2
- [ ] 项目 3
```

## 表格模式

```markdown
| 列1 | 列2 | 列3 |
|-----|-----|-----|
| 值1 | 值2 | 值3 |
```
