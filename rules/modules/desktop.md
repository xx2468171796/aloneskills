# 模块: 桌面端 (Desktop)

> **模块 ID**: `desktop`
> **依赖**: core
> **版本**: 见 `@rules/versions.md`

## 技术选型

| 技术 | 用途 |
|------|------|
| **Tauri v2** | 跨平台桌面应用 (Rust + WebView) |

## 约束

- `.tsx` 中**严禁** import `fs`/`path`/`os` — 文件操作必须通过 Tauri Command
- Rust 返回 `Result<T, String>`，前端必须捕获异常
- 桌面端功能封装到 `src/lib/tauri/`

## 目录结构

```
apps/<app-name>/
├── src-tauri/
│   ├── src/
│   │   ├── lib.rs         # Tauri Command 定义
│   │   └── main.rs        # 入口
│   ├── Cargo.toml
│   └── tauri.conf.json
├── src/lib/tauri/
│   └── commands.ts        # 前端调用封装
```

## Rust Command 示例

```rust
use tauri::command;
use std::fs;

#[command]
fn read_file(path: String) -> Result<String, String> {
    fs::read_to_string(&path).map_err(|e| e.to_string())
}
```

## 前端调用

```typescript
import { invoke } from "@tauri-apps/api/core"

export async function readLocalFile(path: string): Promise<string> {
  return invoke("read_file", { path })
}
```

## 构建命令

```bash
pnpm tauri dev      # 开发模式
pnpm tauri build    # 生产构建
```
