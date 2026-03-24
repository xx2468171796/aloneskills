# 项目规则（已迁移）

> 此文件的内容已合并到模块化规则系统中：

| 旧内容 | 新位置 |
|--------|--------|
| 代码风格 / 命名规范 | `rules/coding-standards.md` |
| 技术约束 (✅/❌) | `rules/core.md` |
| 架构原则 | `rules/coding-standards.md` |
| Git 提交规范 | `rules/coding-standards.md` |
| 目录结构 | `rules/core.md` |
| 测试要求 | `rules/modules/testing.md` |
| 文件组织 | `rules/core.md` + `rules/modules/db.md` |

## 移植到新项目

```bash
# 复制整个 rules/ 目录到目标项目
cp -r aloneskills/rules/ <target-project>/rules/

# 然后编辑 stack.config.json 选择需要的模块
vi <target-project>/rules/stack.config.json
```
