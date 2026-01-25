# UI 风格详解

## 1. Glassmorphism (玻璃拟态)

**适用场景**: 现代 SaaS、金融仪表板、科技产品

```tsx
<div className="bg-white/10 backdrop-blur-lg border border-white/20 rounded-2xl shadow-xl">
  {/* 内容 */}
</div>
```

**关键属性**:
- `backdrop-blur-lg` - 模糊背景
- `bg-white/10` - 半透明白色 (暗色模式)
- `bg-white/80` - 半透明白色 (浅色模式)
- `border-white/20` - 半透明边框

---

## 2. Minimalism (极简主义)

**适用场景**: 企业应用、文档、管理后台

```tsx
<div className="bg-white shadow-sm border border-gray-200 rounded-lg p-6">
  {/* 内容 */}
</div>
```

**关键属性**:
- 大量留白
- 单色或双色配色
- 清晰的层次结构

---

## 3. Soft UI (柔和 UI)

**适用场景**: 现代企业应用、SaaS 仪表板

```tsx
<div className="bg-white/80 shadow-lg rounded-2xl border border-gray-100 p-6">
  {/* 内容 */}
</div>
```

---

## 4. Bento Grid (便当盒布局)

**适用场景**: 仪表板、产品功能页

```tsx
<div className="grid grid-cols-4 gap-4">
  <div className="col-span-2 row-span-2 bg-primary rounded-3xl p-6">
    {/* 主要内容 */}
  </div>
  <div className="col-span-1 bg-secondary rounded-3xl p-4">
    {/* 次要内容 */}
  </div>
</div>
```

---

## 5. Dark Mode (暗色模式)

**适用场景**: 开发工具、夜间应用

```tsx
<div className="bg-slate-900 text-slate-100 border border-slate-800 rounded-lg">
  {/* 内容 */}
</div>
```

---

## 6. Brutalism (粗野主义)

**适用场景**: 设计作品集、艺术项目

```tsx
<div className="bg-yellow-400 border-4 border-black shadow-[8px_8px_0_0_#000] p-6">
  {/* 内容 */}
</div>
```
