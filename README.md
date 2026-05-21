# DevVerse - 现代化技术博客

使用 **Astro 6** + **Tailwind CSS v4** 构建的高性能技术博客，采用玻璃拟态（Glassmorphism）+ 渐变科技风设计。

## ✨ 特性

- 🎨 **现代 UI**：玻璃拟态 + 动态渐变背景 + 浮动光球动画
- 🌙 **暗黑模式**：自动检测系统偏好，支持手动切换
- 📝 **Markdown / MDX**：用熟悉的语法写文章，支持嵌入 React/Vue 组件
- ⚡ **极致性能**：零 JS 默认输出，Lighthouse 接近满分
- 🔍 **全文搜索**：内置搜索功能（Ctrl/Cmd + K）
- 📊 **阅读进度条**：顶部渐变进度条
- 🏷️ **标签 & 分类**：灵活的分类体系
- 📱 **响应式**：移动端到桌面端完美适配
- 🔗 **SEO 友好**：自动生成 sitemap + Open Graph

## 🚀 快速开始

```bash
# 安装依赖
npm install

# 开发预览
npm run dev

# 构建生产版本
npm run build

# 预览生产版本
npm run preview
```

## ✍️ 如何写文章

在 `src/content/blog/` 目录下创建 `.md` 或 `.mdx` 文件：

```markdown
---
title: "你的文章标题"
description: "文章简短描述，一句话概括"
pubDate: 2026-05-21
tags: [标签1, 标签2, 标签3]
category: 分类名称
featured: true   # 是否在首页置顶显示
draft: false      # 草稿不会发布
heroImage: ""    # 可选：文章封面图 URL
---

这里是文章正文，支持 **Markdown** 语法。

## 二级标题

- 列表项
- 列表项

```javascript
// 代码块也完美支持
const blog = () => console.log('Hello!');
```
```

## 📁 项目结构

```
astro-tech-blog/
├── public/               # 静态资源（favicon 等）
├── src/
│   ├── components/       # Astro 组件
│   │   ├── Header.astro  # 导航栏 + 搜索 + 主题切换
│   │   ├── Footer.astro  # 页脚
│   │   ├── PostCard.astro # 文章卡片
│   │   ├── TagBadge.astro # 标签徽章
│   │   └── TOC.astro     # 文章目录
│   ├── content/
│   │   └── blog/         # 博客文章（Markdown/MDX）
│   ├── layouts/
│   │   ├── BaseLayout.astro  # 基础布局
│   │   └── PostLayout.astro   # 文章详情布局
│   ├── pages/            # 页面路由
│   │   ├── index.astro   # 首页
│   │   ├── about.astro   # 关于页
│   │   ├── blog/         # 博客列表 + 文章详情
│   │   ├── tags/         # 标签列表 + 标签详情
│   │   └── categories/   # 分类列表 + 分类详情
│   └── styles/
│       └── global.css    # 全局样式（Tailwind v4 配置）
├── astro.config.mjs      # Astro 配置
├── content.config.ts     # 内容集合配置
└── package.json
```

## 🎨 自定义配置

### 1. 修改站点信息

编辑 `astro.config.mjs` 中的 `site`：

```javascript
export default defineConfig({
  site: 'https://你的用户名.github.io',  // 改成你的域名
  // ...
});
```

### 2. 修改博客名称

搜索并替换所有 `DevVerse` 和 `Dev` 为你的名称。

### 3. 修改联系方式

编辑 `src/components/Footer.astro` 中的联系方式。

### 4. 自定义颜色

编辑 `src/styles/global.css` 中的 `@theme` 块：

```css
@theme {
  --color-primary-500: #6366f1;  // 改成你喜欢的颜色
  /* ... */
}
```

## 🚢 部署

### GitHub Pages

1. 创建 GitHub 仓库
2. 修改 `astro.config.mjs` 中的 `site`
3. 推送代码
4. 在仓库 Settings → Pages 中启用 GitHub Pages
5. 选择 `gh-pages` 分支（或使用 Actions 自动部署）

### Vercel / Netlify

零配置部署，直接连接 GitHub 仓库即可。

## 🛠️ 技术栈

| 技术 | 说明 |
|------|------|
| [Astro 6](https://astro.build) | 静态站点框架 |
| [Tailwind CSS v4](https://tailwindcss.com) | CSS 框架 |
| [MDX](https://mdxjs.com) | Markdown + JSX |
| [Shiki](https://shiki.matsu.io) | 代码高亮 |

## 📄 License

MIT
