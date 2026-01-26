---
name: ui-ux-pro-max
description: AI 驱动的设计智能技能。当需要构建 UI/UX（设计、创建、实现、审查、修复、改进界面）时使用此技能。提供 UI 风格、配色方案、字体搭配、图表类型和 UX 指南的智能推荐。默认使用 HeroUI（未指定时优先）。
---

# UI UX Pro Max 技能

为前端项目提供设计智能，确保 UI/UX 的专业性和一致性。

## 适用技术栈

- **React** / Next.js / Vue / Svelte
- **Tailwind CSS** v3/v4
- **HeroUI**（默认）/ shadcn/ui / Radix UI
- **Lucide** / Heroicons

## 设计系统工作流

### Step 1: 分析需求

从用户请求中提取：
- **产品类型**: SaaS、电商、仪表板、落地页等
- **风格关键词**: 极简、专业、优雅、暗色模式等
- **行业**: 医疗、金融科技、教育、美容等

### Step 2: 选择 UI 风格

| 风格 | 适用场景 | Tailwind 关键词 |
|------|----------|-----------------|
| **Glassmorphism** | 现代 SaaS、金融仪表板 | `bg-white/10 backdrop-blur-lg border-white/20` |
| **Minimalism** | 企业应用、文档 | `bg-white text-slate-900 shadow-sm` |
| **Neumorphism** | 健康/冥想应用 | `shadow-[inset_0_2px_4px] bg-gray-100` |
| **Soft UI** | 现代企业应用 | `shadow-lg rounded-2xl bg-white/80` |
| **Bento Grid** | 仪表板、产品页 | `grid grid-cols-4 gap-4` |
| **Dark Mode** | 开发工具、夜间应用 | `bg-slate-900 text-slate-100` |

### Step 3: 选择配色方案

**行业配色推荐**:

| 行业 | 主色 | 辅助色 | 强调色 |
|------|------|--------|--------|
| 金融科技 | `#1E3A8A` 深蓝 | `#F1F5F9` 浅灰 | `#10B981` 绿 |
| 医疗健康 | `#0D9488` 青绿 | `#F0FDFA` 浅青 | `#3B82F6` 蓝 |
| 电商零售 | `#7C3AED` 紫 | `#FAF5FF` 浅紫 | `#F59E0B` 橙 |
| 教育培训 | `#2563EB` 蓝 | `#EFF6FF` 浅蓝 | `#22C55E` 绿 |
| 美容健康 | `#EC4899` 粉 | `#FDF2F8` 浅粉 | `#D4AF37` 金 |

### Step 4: 字体搭配

| 风格 | 标题字体 | 正文字体 | Google Fonts |
|------|----------|----------|--------------|
| **现代专业** | Inter | Inter | `family=Inter:wght@400;500;600;700` |
| **优雅奢华** | Playfair Display | Lato | `family=Playfair+Display:wght@400;700&family=Lato` |
| **科技极客** | JetBrains Mono | Inter | `family=JetBrains+Mono&family=Inter` |
| **友好活泼** | Poppins | Open Sans | `family=Poppins:wght@400;600&family=Open+Sans` |
| **中文优先** | Noto Sans SC | Noto Sans SC | `family=Noto+Sans+SC:wght@400;500;700` |

## 通用 UI 规则

### 图标规范

```tsx
// ✅ 正确 - 使用 Lucide React
import { User, Settings, LogOut } from "lucide-react"

<User className="h-5 w-5" />

// ❌ 错误 - 禁止使用
// 1. Emoji 作为图标: 🎨 🚀 ⚙️
// 2. 内联 SVG
```

### 交互规范

| 规则 | 正确 | 错误 |
|------|------|------|
| 可点击元素 | `cursor-pointer` | 默认光标 |
| 悬停反馈 | `hover:bg-muted transition-colors` | 无悬停效果 |
| 过渡时长 | `duration-200` (150-300ms) | `duration-1000` |
| 焦点状态 | `focus-visible:ring-2` | 无焦点指示 |

### 对比度规范

| 模式 | 文本色 | 背景色 | 对比度 |
|------|--------|--------|--------|
| Light | `text-slate-900` | `bg-white` | ≥4.5:1 |
| Light Muted | `text-slate-600` | `bg-white` | ≥4.5:1 |
| Dark | `text-slate-100` | `bg-slate-900` | ≥4.5:1 |

### 响应式断点

| 断点 | 宽度 | 用途 |
|------|------|------|
| 默认 | <640px | 手机 |
| `sm:` | ≥640px | 大手机 |
| `md:` | ≥768px | 平板 |
| `lg:` | ≥1024px | 笔记本 |
| `xl:` | ≥1280px | 桌面 |
| `2xl:` | ≥1536px | 大屏 |

## 交付前检查清单

### 视觉质量
- [ ] 无 Emoji 作为图标 (使用 Lucide/Heroicons)
- [ ] 图标尺寸一致 (`h-5 w-5` 或 `h-6 w-6`)
- [ ] 悬停状态不导致布局偏移
- [ ] 使用主题色而非硬编码

### 交互
- [ ] 所有可点击元素有 `cursor-pointer`
- [ ] 悬停提供视觉反馈
- [ ] 过渡平滑 (150-300ms)
- [ ] 焦点状态可见

### 亮/暗模式
- [ ] 浅色模式文本对比度足够 (≥4.5:1)
- [ ] 玻璃/透明元素在浅色模式可见
- [ ] 边框在两种模式都可见

### 布局
- [ ] 响应式: 375px, 768px, 1024px, 1440px
- [ ] 移动端无水平滚动

### 无障碍
- [ ] 图片有 alt 文本
- [ ] 表单输入有 label
- [ ] 颜色不是唯一指示器
- [ ] 尊重 `prefers-reduced-motion`

## 参考文档

- **UI 风格详解**: 见 `references/styles.md`
- **配色方案**: 见 `references/colors.md`
- **字体搭配**: 见 `references/typography.md`
