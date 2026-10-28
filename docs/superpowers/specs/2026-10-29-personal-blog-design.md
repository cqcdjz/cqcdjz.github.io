# 个人博客设计文档

- 日期:2026-10-29
- 主题:个人博客(技术文章为主 + 关于我 + 项目展示)
- 状态:已确认

## 1. 概述

搭建一个个人技术博客,视觉风格为「简约 + 未来科技/赛博朋克」的**深空玻璃**方向,使用 Astro 构建为静态站点,部署到 GitHub Pages。

## 2. 需求

- 内容定位:**技术文章/学习笔记为主**,附带「关于我」页和少量项目展示
- 视觉风格:**深空玻璃** —— 深蓝深空底 + 紫青渐变 + 玻璃拟态卡片,简约克制
- 部署:GitHub Pages(用户有 GitHub 账号,无自定义域名,使用 `username.github.io`)
- 交付物:成品博客 + 一份「搭建流程步骤文档」

## 3. 技术选型

| 项目 | 选择 | 说明 |
|------|------|------|
| 框架 | Astro | 静态站点生成器,组件化,Markdown 支持好,便于精细定制深空玻璃样式 |
| 内容 | Markdown + Content Collections | 文章和项目用 Markdown 管理 |
| 代码高亮 | Shiki | Astro 内置,配深色主题 |
| 部署 | GitHub Pages + GitHub Actions | 推送即自动构建部署 |

## 4. 架构

```
blog/
├── src/
│   ├── pages/              # 页面路由
│   │   ├── index.astro         # 首页(文章列表)
│   │   ├── posts/[slug].astro  # 文章详情页
│   │   ├── about.astro         # 关于我
│   │   └── projects.astro      # 项目展示
│   ├── content/
│   │   ├── posts/          # 文章(Markdown)
│   │   └── projects/       # 项目(Markdown)
│   ├── components/         # 可复用组件(导航、卡片、页脚)
│   ├── layouts/            # 布局模板
│   └── styles/             # 全局样式(深空玻璃主题)
├── public/                 # 静态资源(头像、favicon)
└── astro.config.mjs        # 站点配置 + GitHub Pages 部署配置
```

## 5. 页面结构

| 页面 | 路由 | 内容 |
|------|------|------|
| 首页 | `/` | 文章列表(玻璃拟态卡片),顶部简短自我介绍 |
| 文章页 | `/posts/[slug]` | 单篇文章 + 代码高亮 + 标签 |
| 关于我 | `/about` | 个人介绍、技术栈、联系方式 |
| 项目 | `/projects` | 少量项目卡片(名称/描述/链接) |

## 6. 视觉系统(深空玻璃)

- **背景**:深空蓝 `#0b1020`,可加细微渐变/星点氛围
- **强调色**:紫青渐变 `#22d3ee → #a78bfa`(标题、链接、hover)
- **卡片**:半透明白 `rgba(255,255,255,.06)` + `backdrop-filter: blur` + 细描边
- **字体**:现代无衬线正文 + 等宽字体做代码/标签点缀
- **整体**:暗色为主、大留白、克制地使用霓虹点缀

## 7. 数据流

- 文章用 Markdown 写,放在 `src/content/posts/`,通过 Content Collections 管理
- 每篇 Markdown 顶部有 frontmatter(标题、日期、标签、摘要)
- 首页读取所有文章 → 按日期倒序 → 渲染成玻璃拟态卡片列表
- 文章页按 slug 路由 → 渲染正文 + 代码高亮

## 8. 技术细节

- 代码高亮:Shiki,深色主题贴合深空玻璃
- 标签系统:frontmatter 中的 `tags`,在卡片和文章页展示
- 响应式:手机端自适应(导航折叠、卡片单列)
- SEO:每页 title/description + sitemap;RSS 暂不做(YAGNI)

## 9. 部署(GitHub Pages)

1. 本地初始化 git 仓库,推送到用户新建的 GitHub 仓库
2. 配置 GitHub Actions workflow(推送即 `astro build` 并部署)
3. 仓库 Settings → Pages 启用(Actions 作为来源)
4. 之后每次推送自动更新到 `username.github.io`

## 10. 错误处理与测试

- 构建失败:`astro build` 报错会在本地预览时暴露
- 自定义 404 页面(深空玻璃风格)
- 测试:以「构建通过 + 本地 `astro dev` 逐页预览」为准(静态站不需要单测)

## 11. 交付物

1. 可运行的 Astro 博客项目
2. 《搭建流程步骤文档》:每一步命令、每个文件用途、如何新增文章、如何改样式、如何部署
