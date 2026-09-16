# Hexo 前端技术博客 Spec

## Why
用户希望用 Hexo 搭建一个专注于前端技术分享的博客，并能一键部署到 GitHub Pages。当前工作区为空（用户已清空旧静态站点），需要从零搭建一套基于 Hexo 的、可维护、可自动部署的博客基础设施。要求使用简约的暗色主题。

## What Changes
- 在工作区初始化 Hexo 博客项目（`package.json`、`_config.yml`、`source/_posts/` 等）
- 安装并配置简约暗色主题：**hexo-theme-cactus**（默认暗色、极简、无卡片堆叠）
- 配置站点信息：标题、描述、语言（zh-CN）、作者、URL
- 创建若干前端技术示例文章（Markdown，含 front-matter：分类、标签、日期）
- 配置永久链接、分类、标签、归档、代码高亮等基础渲染
- 配置 GitHub Pages 部署：
  - 安装 `hexo-deployer-git`，支持 `hexo clean && hexo deploy` 一键推送至 `gh-pages` 分支
  - 新增 GitHub Actions workflow（`.github/workflows/deploy.yml`），在推送到 `main` 时自动构建并部署到 GitHub Pages
- 新增 `.nojekyll` 以避免 GitHub Pages 跳过 Hexo 生成的 `_next` 等下划线目录
- 新增 `.gitignore`（忽略 `node_modules/`、`public/`、`db.json` 等）

## Impact
- Affected specs: 无（首次创建）
- Affected code:
  - `_config.yml`（站点与部署配置）
  - `_config.cactus.yml`（主题配置，若主题支持独立配置）
  - `package.json`（Hexo 依赖与脚本）
  - `source/_posts/*.md`（示例文章）
  - `.github/workflows/deploy.yml`（CI 部署）
  - `.gitignore`、`.nojekyll`

## ADDED Requirements

### Requirement: Hexo 博客初始化
系统 SHALL 在工作区根目录初始化一个可运行的 Hexo 博客，执行 `npm install` 后可通过 `npx hexo server` 在本地 `http://localhost:4000` 预览。

#### Scenario: 本地预览
- **WHEN** 开发者执行 `npm install && npx hexo server`
- **THEN** 本地 4000 端口启动服务，浏览器访问可见暗色主题首页与文章列表

### Requirement: 简约暗色主题
系统 SHALL 使用 `hexo-theme-cactus` 作为主题，默认采用暗色（dark）配色方案，整体视觉简约：无多余卡片、留白充足、单一强调色、代码块高亮清晰。

#### Scenario: 主题生效
- **WHEN** 博客首页加载完成
- **THEN** 页面背景为深色，正文文字为浅色，导航与链接使用主题强调色，且无粉色/紫色等花哨配色

### Requirement: 前端技术示例文章
系统 SHALL 至少包含 3 篇前端技术示例文章，覆盖 React、TypeScript、CSS 等方向，每篇含合法 front-matter（title、date、tags、categories）。

#### Scenario: 文章列表渲染
- **WHEN** 访问首页或归档页
- **THEN** 示例文章按日期倒序展示，可点击进入正文，正文 Markdown 正确渲染（标题、列表、代码块）

### Requirement: GitHub Pages 自动部署
系统 SHALL 通过 GitHub Actions 在推送到 `main` 分支时自动执行 `hexo generate` 并部署 `public/` 目录到 GitHub Pages。

#### Scenario: 推送触发部署
- **WHEN** 开发者将代码推送到 `main` 分支
- **THEN** GitHub Actions workflow 自动运行，构建产物上传到 Pages，几分钟后站点更新为最新内容

### Requirement: 手动一键部署
系统 SHALL 支持通过 `hexo deploy` 命令将 `public/` 推送到 `gh-pages` 分支（基于 `hexo-deployer-git`），作为 CI 之外的备用部署方式。

#### Scenario: 手动部署
- **WHEN** 开发者执行 `npx hexo clean && npx hexo generate && npx hexo deploy`
- **THEN** 构建产物被推送到仓库的 `gh-pages` 分支
