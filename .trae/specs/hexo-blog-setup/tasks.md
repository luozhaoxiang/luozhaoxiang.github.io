# Tasks

- [x] Task 1: 初始化 Hexo 博客项目
  - [x] SubTask 1.1: 在工作区根目录生成 `package.json`，声明 Hexo 核心依赖（hexo、hexo-cli、hexo-server、hexo-deployer-git）与脚本（server、clean、generate、deploy）
  - [x] SubTask 1.2: 创建 `_config.yml`，配置站点信息（title、subtitle、description、author、language: zh-CN、timezone: Asia/Shanghai、url、permalink）
  - [x] SubTask 1.3: 创建 `source/_posts/` 目录结构，并保留 Hexo 必需的 `source/_data/`、`source/about/index.md` 等约定目录
  - [x] SubTask 1.4: 创建 `.gitignore`（忽略 `node_modules/`、`public/`、`db.json`、`.deploy_git/`）和 `.nojekyll`

- [x] Task 2: 安装并配置 hexo-theme-cactus 暗色主题
  - [x] SubTask 2.1: 在 `package.json` 中添加 `hexo-theme-cactus` 依赖
  - [x] SubTask 2.2: 在 `_config.yml` 中设置 `theme: cactus`
  - [x] SubTask 2.3: 创建主题配置文件（`_config.cactus.yml`），设置 `colorscheme: dark`、导航菜单、社交链接、文章元信息显示项

- [x] Task 3: 编写 3 篇前端技术示例文章
  - [x] SubTask 3.1: 文章 A —— React 方向（如：React 18 并发渲染机制解析），含 front-matter（title、date、tags、categories）与代码示例
  - [x] SubTask 3.2: 文章 B —— TypeScript 方向（如：TypeScript 类型体操实战），含 front-matter 与代码示例
  - [x] SubTask 3.3: 文章 C —— CSS 方向（如：现代 CSS 容器查询入门），含 front-matter 与代码示例

- [x] Task 4: 配置 GitHub Pages 自动部署
  - [x] SubTask 4.1: 创建 `.github/workflows/deploy.yml`，触发条件为 `push` 到 `main`，使用 `actions/configure-pages`、`actions/upload-pages-artifact`、`actions/deploy-pages`
  - [x] SubTask 4.2: workflow 中安装 Node 20、`npm install`、`npx hexo generate`，上传 `public/` 作为 Pages artifact

- [x] Task 5: 配置 hexo-deployer-git 手动部署
  - [x] SubTask 5.1: 在 `_config.yml` 的 `deploy` 段配置 `type: git`、`repo` 使用占位符（注释提示替换为用户仓库地址）、`branch: gh-pages`

- [x] Task 6: 本地验证
  - [x] SubTask 6.1: 执行 `npm install` 安装依赖
  - [x] SubTask 6.2: 执行 `hexo generate` 验证构建（`npx hexo server` 启动后 `http://localhost:4000` 同样可访问；构建已产出 `public/index.html` 及 82 个文件，暗色主题 CSS `#1d1f21` 已应用，文章正文页正常生成）

# Task Dependencies
- Task 2 依赖 Task 1（需先有 `_config.yml`）
- Task 3 依赖 Task 1（需先有 `source/_posts/` 结构）
- Task 4、Task 5 依赖 Task 1、Task 2（部署需可构建产物）
- Task 6 依赖 Task 1 ~ Task 5 全部完成
