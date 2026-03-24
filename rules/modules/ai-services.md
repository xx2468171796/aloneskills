# 模块: AI 服务 (AI Services)

> **模块 ID**: `ai-services`
> **依赖**: core
> **版本**: 见 `@rules/versions.md`

## 技术选型

| 技术 | 用途 | SDK |
|------|------|-----|
| **Anthropic Claude** | LLM 文本处理、结构化输出 | `@anthropic-ai/sdk` |
| **Groq Whisper** | 语音转文字 (STT) | `groq-sdk` |
| **OpenAI** | GPT / Whisper / TTS | `openai` |
| **Vercel AI SDK** | 统一 AI 接口层 | `ai` |

## 约束

- AI API 调用**必须在 Server Actions 中**，严禁在客户端直接调用
- API Key 通过环境变量注入，**严禁硬编码**
- AI 服务封装到 `src/lib/ai/`，使用 Adapter 模式（方便切换提供商）
- 大文件（音频/视频）先上传到服务端，再由 Server Action 调用 AI API
- 流式输出使用 Vercel AI SDK 的 `streamText` / `streamObject`

## 目录结构

```
src/lib/ai/
├── providers/
│   ├── claude.ts           # Anthropic Claude Adapter
│   ├── groq.ts             # Groq Whisper Adapter
│   └── openai.ts           # OpenAI Adapter
├── prompts/
│   ├── system.ts           # 系统提示词模板
│   └── templates.ts        # 行业/场景提示词
└── index.ts                # 统一导出

src/actions/ai/
├── transcribe.ts           # 语音转文字
├── structure.ts            # 文本结构化
└── generate.ts             # 内容生成
```

## 环境变量

```env
ANTHROPIC_API_KEY=""
GROQ_API_KEY=""
OPENAI_API_KEY=""
```

## Adapter 模式示例

```typescript
// src/lib/ai/providers/claude.ts
import Anthropic from "@anthropic-ai/sdk"

const client = new Anthropic()

export async function structureText(text: string, systemPrompt: string) {
  const response = await client.messages.create({
    model: "claude-sonnet-4-20250514",
    max_tokens: 4096,
    system: systemPrompt,
    messages: [{ role: "user", content: text }],
  })
  return response
}
```
