# leapx-ai.github.io — 项目上下文指南

> 本文件面向 AI 编码助手。若你正在阅读此文件，说明你可能需要修改、构建或部署这个博客站点。以下信息全部基于项目实际内容，无假设或推断。

---

## 项目概述

这是一个基于 **Jekyll** 的静态博客站点，使用 [**Chirpy**][chirpy] 主题（`jekyll-theme-chirpy ~> 7.3`）。

- **站点标题**：咏歌与凯旋
- **副标题/描述**：编程、AI与系统思考
- **作者**：Liu Zhao
- **GitHub 用户**：`leapx-ai`
- **语言**：网页语言为 `en`（英文布局），但正文内容以中文为主
- **部署目标**：GitHub Pages（通过 GitHub Actions 自动构建并部署）

[chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy

---

## 技术栈与关键配置

| 层级 | 技术/工具 | 说明 |
|------|-----------|------|
| 静态站点生成器 | Jekyll (Ruby) | 核心构建引擎 |
| 主题 | `jekyll-theme-chirpy` | 通过 Gemfile 引入，版本锁定 `~> 7.3, >= 7.3.1` |
| 标记渲染 | Kramdown + Rouge | 代码高亮；行号显示配置见 `_config.yml` |
| 测试工具 | `html-proofer ~> 5.0` | 检查站点内部链接与 HTML 有效性 |
| 部署平台 | GitHub Pages | 使用官方 `actions/deploy-pages@v4` |
| CI/CD | GitHub Actions | 工作流文件 `.github/workflows/pages-deploy.yml` |
| 开发容器 | Dev Container (Jekyll) | 镜像 `mcr.microsoft.com/devcontainers/jekyll:2-bullseye` |

### 关键配置文件

- **`_config.yml`** — Jekyll 主配置。包含站点元数据、SEO、社交链接、评论系统开关、PWA 开关、分页（`paginate: 10`）、Kramdown/Rouge 高亮规则、归档（`jekyll-archives`）等。
- **`Gemfile`** / **`Gemfile.lock`** — Ruby 依赖。主题和测试工具通过 Gemfile 管理。
- **`.github/workflows/pages-deploy.yml`** — 构建与部署工作流。触发条件：`push` 到 `main` 或 `master` 分支（排除 `.gitignore`、`README.md`、`LICENSE` 变更）。
- **`.editorconfig`** — 编码风格：UTF-8、2 空格缩进、LF 换行、Markdown 不裁剪尾部空格、YAML 使用双引号、JS/CSS 使用单引号。
- **`.vscode/settings.json`** — VS Code 格式化配置：Prettier 为默认格式化工具、`.html` 文件关联为 Liquid、Markdown 使用 `yzhang.markdown-all-in-one`、Shell 使用 `shfmt`。

---

## 项目结构

```
├── _config.yml              # Jekyll 主配置
├── Gemfile / Gemfile.lock   # Ruby 依赖
├── index.html               # 首页入口（front matter 指定 layout: home）
│
├── _posts/                  # 博客文章（Markdown）
│   └── YYYY-MM-DD-标题.md   # 文件名必须遵循 Jekyll 命名规范
│
├── _layouts/                # 自定义布局（覆盖主题默认）
│   ├── home.html            # 首页布局：展示文章列表、置顶文章、分页
│   └── post.html            # 文章页布局：TOC、元信息、分享、相关文章导航
│
├── _includes/               # 可复用 Liquid 片段
│   ├── metadata-hook.html
│   ├── post-description.html
│   └── sidebar.html
│
├── _tabs/                   # 独立页面（about / archives / categories / tags）
│   └── about.md             # 关于页面（当前内容为空，仅保留 front matter）
│
├── _data/                   # 数据文件
│   ├── contact.yml          # 侧边栏联系方式配置
│   └── share.yml            # 文章底部分享按钮配置
│
├── _plugins/                # 自定义 Jekyll 插件
│   └── posts-lastmod-hook.rb # 通过 Git 历史自动填充 `last_modified_at`
│
├── assets/                  # 静态资源
│   ├── css/
│   ├── img/                 # 头像、文章配图等
│   └── lib/
│
├── tools/                   # 本地开发脚本
│   ├── run.sh               # 启动 Jekyll 开发服务器（支持 --production、--host）
│   └── test.sh              # 生产构建 + html-proofer 测试
│
├── .github/workflows/       # CI/CD
│   └── pages-deploy.yml     # GitHub Pages 自动部署
│
├── .devcontainer/           # Dev Container 配置
│   ├── devcontainer.json
│   └── post-create.sh       # 容器创建后安装 Node.js、npm 依赖、shfmt、OMZ 插件
│
├── .vscode/                 # VS Code 工作区配置
│   ├── settings.json
│   ├── extensions.json      # 推荐安装 Remote-Containers
│   └── tasks.json           # 任务：Run Jekyll Server / Build Jekyll Site
│
└── _site/                   # 构建输出目录（.gitignore 排除）
```

### 代码组织说明

- **主题逻辑不在本仓库**：核心样式、脚本、布局由 `jekyll-theme-chirpy` Gem 提供。本仓库仅通过覆盖 `_layouts`、`_includes`、`_tabs` 等目录下的同名文件来自定义行为。
- **文章是主要资产**：所有内容创作发生在 `_posts/` 目录。文章使用标准 Jekyll front matter（`layout`、`title`、`date`、`categories`、`tags`、`pin`、`image` 等）。
- **自定义插件**：`_plugins/posts-lastmod-hook.rb` 在文章初始化时读取 Git 提交历史，若文件被修改过多次（`git rev-list --count HEAD` > 1），则自动将最近一次提交时间写入 `last_modified_at`。

---

## 构建与测试命令

### 本地开发

```bash
# 启动开发服务器（带 live reload，默认绑定 127.0.0.1）
bash tools/run.sh

# 指定主机（例如局域网调试）
bash tools/run.sh -H 0.0.0.0

# 以 production 模式启动（启用压缩、禁用 development 环境排除项）
bash tools/run.sh -p
```

> 底层命令：`bundle exec jekyll s -l -H <host>`。若检测到 Docker 环境，会自动追加 `--force_polling`。

### 生产构建与测试

```bash
# 构建到 _site 并运行 html-proofer 检查
bash tools/test.sh

# 指定自定义配置文件
bash tools/test.sh -c "_config.yml,_config_local.yml"
```

> 底层命令：
> ```bash
> JEKYLL_ENV=production bundle exec jekyll b -d "_site" -c "_config.yml"
> bundle exec htmlproofer _site --disable-external --ignore-urls "/^http:\/\/127.0.0.1/,/^http:\/\/0.0.0.0/,/^http:\/\/localhost/"
> ```

### VS Code 快捷方式

- `Ctrl+Shift+B`（macOS: `Cmd+Shift+B`）→ **Run Jekyll Server**（默认构建任务）
- 任务面板中还有 **Build Jekyll Site**（调用 `tools/test.sh`）

---

## 代码风格指南

项目遵循 `.editorconfig` 的强制约定：

- **缩进**：2 个空格（空格，非 Tab）
- **换行**：Unix 风格 `LF`
- **文件结尾**：必须保留空行（`insert_final_newline = true`）
- **尾部空格**：常规文件裁剪；**Markdown 文件不裁剪**（`trim_trailing_whitespace = false`）
- **引号**：
  - JS / CSS / SCSS → 单引号
  - YAML / YML → 双引号
- **字符集**：UTF-8

VS Code 已配置：
- 默认格式化器：Prettier
- `.html` 文件识别为 Liquid 语法
- Shell 脚本使用 `shfmt` 格式化
- Markdown 使用 `yzhang.markdown-all-in-one`

---

## 测试策略

- **静态分析**：`html-proofer` 检查构建后的 `_site` 目录，验证内部链接、图片引用、HTML 合法性。
- **外部链接**：CI 中通过 `--disable-external` 忽略外部链接检查，避免网络波动导致构建失败。
- **本地预览**：开发阶段通过 `tools/run.sh` 的 live reload 实时验证页面渲染。
- **无单元测试框架**：本项目为内容型静态站点，不依赖 RSpec/Jest 等单元测试。

---

## 部署流程

1. **触发**：向 `main` 或 `master` 分支推送代码（排除 `.gitignore`、`README.md`、`LICENSE` 变更）。
2. **构建**：
   - `actions/checkout@v4`（`fetch-depth: 0`，保留完整 Git 历史，供 `posts-lastmod-hook.rb` 使用）
   - `actions/configure-pages@v4` 设置 Pages 环境
   - `ruby/setup-ruby@v1`（Ruby 3.3，启用 Bundler 缓存）
   - `bundle exec jekyll b -d "_site${base_path}"`（`JEKYLL_ENV=production`）
3. **测试**：运行 `html-proofer`（与本地 `test.sh` 相同参数）。
4. **上传**：`actions/upload-pages-artifact@v3` 将 `_site` 作为 artifact。
5. **部署**：`actions/deploy-pages@v4` 发布到 GitHub Pages。

> 工作流权限：`contents: read`，`pages: write`，`id-token: write`。

---

## 安全与隐私注意事项

- **Google Analytics**：`_config.yml` 中已配置跟踪 ID `G-BFZZHSES54`。修改或删除时请确认隐私合规。
- **评论系统**：当前全局关闭（`comments.provider` 为空）。若启用，需配置 Disqus / Utterances / Giscus 的仓库与认证信息。
- **PWA**：已启用（`pwa.enabled: true`）并开启离线缓存（`cache.enabled: true`）。更新静态资源后注意缓存策略是否导致旧版本残留。
- **社交链接**：`_config.yml` 中的 `social.links` 和 `_data/contact.yml` 包含作者真实 GitHub 与邮箱信息，修改时避免泄露敏感联系方式。
- **无服务端逻辑**：全站为纯静态 HTML，无数据库、无 API 密钥暴露风险（除前端 Analytics ID 外）。

---

## 给 AI 助手的快速检查清单

当你需要修改本仓库时，请确认：

1. [ ] 是否需要在 `_posts/` 新建文章？文件名必须遵循 `YYYY-MM-DD-标题.md` 格式。
2. [ ] 是否修改了 `_config.yml`？若涉及 `baseurl` 或 `url`，请同步检查 `tools/test.sh` 的读取逻辑。
3. [ ] 是否新增或修改了自定义布局/包含片段？主题默认文件在 Gem 中，本仓库的 `_layouts` / `_includes` 会覆盖主题同名文件。
4. [ ] 是否引入新的 Ruby Gem？请更新 `Gemfile` 并运行 `bundle install`，确保 `Gemfile.lock` 同步更新。
5. [ ] 是否修改了插件代码？`_plugins/posts-lastmod-hook.rb` 依赖 Git 历史，确保 CI 中 `fetch-depth: 0` 不被关闭。
6. [ ] 是否需要验证构建？优先运行 `bash tools/test.sh`，确保 `html-proofer` 通过后再提交。

---

*本文件基于项目实际结构、配置和脚本生成。若项目结构发生重大变化，请同步更新此文件。*
