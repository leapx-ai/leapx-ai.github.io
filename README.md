# 咏歌与凯旋

这是个人静态博客「咏歌与凯旋」的站点仓库，部署在 GitHub Pages 上。

站点内容聚焦于编程、AI 与系统思考，基于 Jekyll 与 Chirpy 主题构建，并在主题之上保留少量站点级定制样式。

## 站点结构

```text
.
├── _posts/                 # 博客文章
├── _tabs/                  # 顶部/侧边导航页面：About、Archives、Tags、Categories
├── _layouts/               # 覆盖主题的页面布局
├── _includes/              # 覆盖主题的局部模板
├── assets/
│   ├── css/                # 站点级样式覆盖
│   └── img/                # 头像、二维码、文章图片等静态资源
├── tools/                  # 本地运行与测试脚本
├── _config.yml             # Jekyll 与站点配置
└── index.html              # 首页入口
```

## 内容维护

### 新增文章

文章放在 `_posts/` 目录，文件名遵循 Jekyll 约定：

```text
YYYY-MM-DD-title.md
```

常用 front matter 示例：

```yaml
---
title: 文章标题
date: 2026-06-08 10:00:00 +0800
categories: [AI, 编程]
tags: [agent, context]
description: 一句话摘要
---
```

### 静态图片

- 通用图片放在 `assets/img/`。
- 文章配图建议按日期放在 `assets/img/posts/YYYY-MM-DD/`。
- About 页公众号二维码使用 `assets/img/IMG_5269.JPG`。

### 样式定制

站点级样式集中在：

```text
assets/css/jekyll-theme-chirpy.scss
```

当前策略是保留 Chirpy 原有布局，只调整主题变量、阅读排版和少量站点组件，避免对主题组件做大量碎片化覆盖。

## 本地开发

安装依赖：

```bash
bundle install
```

启动本地预览：

```bash
bash tools/run.sh
```

或直接运行：

```bash
bundle exec jekyll serve --livereload
```

默认访问：

```text
http://127.0.0.1:4000/
```

## 构建与检查

本地构建：

```bash
bundle exec jekyll build
```

生产构建与链接检查：

```bash
bash tools/test.sh
```

如果构建时出现分类页输出冲突，通常是多篇文章的 `categories` 经过 slug 化后生成了相同路径，需要检查对应文章的分类命名。

## 部署

仓库通过 GitHub Actions 部署到 GitHub Pages：

```text
.github/workflows/pages-deploy.yml
```

推送到 `main` 或 `master` 分支后会自动执行：

1. 安装 Ruby/Bundler 依赖
2. 构建 Jekyll 静态站点
3. 运行 `htmlproofer`
4. 上传并部署到 GitHub Pages

## 维护原则

- 文章内容优先，避免为单篇文章引入复杂站点逻辑。
- 样式定制保持轻量，优先使用主题变量和全局排版节奏。
- 静态资源使用清晰路径，文章图片尽量按日期归档。
- 提交前建议至少运行一次 `bundle exec jekyll build`。

## 技术栈

- Jekyll
- jekyll-theme-chirpy
- GitHub Pages
- GitHub Actions
