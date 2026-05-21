---
title: "用 Astro 构建高性能博客的完整指南"
description: "从零开始，使用 Astro 框架搭建一个现代化、高性能的个人技术博客，包含 Markdown 写作、MDX 增强、暗黑模式等特性。"
pubDate: 2026-05-21
tags: [Astro, 前端, 静态站点, 性能优化]
category: 前端开发
featured: true
heroImage: ""
---

## 为什么选择 Astro？

Astro 是一个专为内容驱动网站设计的现代框架，它的核心理念是**零 JavaScript 默认**——除非你需要交互，否则页面不会向客户端发送任何 JS。

### Astro 的核心优势

1. **岛屿架构（Islands Architecture）**：页面中的交互组件独立加载，不会影响整体性能
2. **内容集合（Content Collections）**：类型安全的 Markdown/MDX 内容管理
3. **框架无关**：可以在同一项目中使用 React、Vue、Svelte 等组件
4. **开箱即用的优化**：图片优化、字体优化、CSS 内联等

## 快速开始

创建一个新的 Astro 项目非常简单：

```bash
npm create astro@latest my-blog
cd my-blog
npm run dev
```

## 内容集合配置

Astro 的内容集合提供了类型安全的内容管理：

```typescript
import { defineCollection, z } from 'astro:content';

const blog = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.coerce.date(),
    tags: z.array(z.string()).default([]),
    category: z.string().default('技术'),
  }),
});

export const collections = { blog };
```

## 写作体验

在 `src/content/blog/` 目录下创建 `.md` 文件即可开始写作：

- 支持**标准 Markdown**语法
- 支持 **MDX**，可以在文章中嵌入交互组件
- 支持 frontmatter 元数据
- 支持代码高亮（Shiki）

## 性能对比

与传统的 Hexo 或 Hugo 相比，Astro 的默认输出几乎没有 JavaScript：

| 指标 | Hexo + Fluid | Astro (本项目) |
|------|-------------|---------------|
| 首屏 JS | ~150KB | ~0KB |
| Lighthouse 分数 | 85-90 | 98-100 |
| 首次内容绘制 | 1.2s | 0.6s |

> 这就是零 JS 默认的威力——你的博客加载速度将快如闪电。

## 下一步

- 添加评论系统（Giscus / Utterances）
- 接入 Analytics
- 配置 RSS 订阅
- 部署到 Vercel / Netlify / GitHub Pages

开始你的 Astro 之旅吧！🚀
