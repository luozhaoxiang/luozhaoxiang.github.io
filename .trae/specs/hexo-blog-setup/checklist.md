# Checklist

- [x] `package.json` 存在，包含 hexo、hexo-cli、hexo-server、hexo-deployer-git、hexo-theme-cactus 依赖，并配置了 server/clean/generate/deploy 脚本
- [x] `_config.yml` 存在，站点 `language` 为 `zh-CN`，`timezone` 为 `Asia/Shanghai`，`url` 与 `permalink` 已配置，`theme: cactus`
- [x] `_config.cactus.yml` 存在，`colorscheme` 设为 `dark`，导航菜单与社交链接配置合理
- [x] `source/_posts/` 下至少存在 3 篇 Markdown 文章，每篇 front-matter 含 title、date、tags、categories
- [x] `.gitignore` 忽略了 `node_modules/`、`public/`、`db.json`、`.deploy_git/`
- [x] `.nojekyll` 文件存在于项目根目录
- [x] `.github/workflows/deploy.yml` 存在，触发于 push 到 main，包含安装依赖、hexo generate、上传 public/ 到 Pages 的步骤
- [x] `_config.yml` 的 `deploy` 段配置了 `type: git`、`branch: gh-pages`，repo 字段含可替换占位符与注释说明
- [x] 执行 `npm install` 成功，无致命错误
- [x] 执行 `hexo generate` 成功（82 files generated in 676 ms，退出码 0），`public/index.html` 已生成；暗色主题 CSS（`background-color: #1d1f21`）已应用，3 篇文章正文页、归档、分类、标签页均正常生成
