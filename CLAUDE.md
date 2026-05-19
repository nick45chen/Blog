# CLAUDE.md

此檔案提供給 Claude Code 在本倉庫工作時的快速指引。

## 專案簡介

- **類型**：Hexo 6.1.0 靜態網誌
- **主題**：NexT（位於 `themes/next/`，直接放在 repo 內，未使用 git submodule）
- **站點標題**：代碼玩家 Nick's Blog
- **作者 / 語系**：Nick Chen / `zh-tw`
- **發佈網址**：<https://nick45chen.github.io/Blog/>（root: `/Blog/`，project page 模式）
- **內容主題**：Android Material Design、Hexo 設定等技術筆記

## 常用指令

對應 `package.json` 中的 scripts：

```bash
npm install          # 安裝相依套件
npm run server       # 本機預覽（hexo server，預設 http://localhost:4000/Blog/）
npm run clean        # 清除 public/ 與 db.json
npm run build        # 產生靜態檔到 public/
npm run deploy       # hexo deploy（_config.yml 的 deploy.type 目前為空，實際部署交給 GitHub Actions）
```

升級 Hexo CLI / 依賴（詳見 `README.md`）：

```bash
npm i hexo-cli -g
npm install -g npm-upgrade
npm-upgrade
npm update
```

## 目錄結構

```
.
├── _config.yml                  # 站點設定（title / url / theme / permalink 等）
├── package.json
├── scaffolds/                   # 新文章/草稿/頁面範本（post.md, draft.md, page.md）
├── source/
│   └── _posts/                  # Markdown 文章，檔名格式：YYYY-MM-DD-title.md
├── themes/
│   └── next/                    # NexT 主題（主題設定檔：themes/next/_config.yml）
└── .github/workflows/pages.yml  # GitHub Actions 自動部署
```

## 新增文章流程

```bash
npx hexo new "文章標題"
```

- 會依 `scaffolds/post.md` 在 `source/_posts/` 產生檔案
- 檔名格式 `YYYY-MM-DD-title.md`（由 `_config.yml` 的 `new_post_name` 控制）
- 永久連結格式：`:year/:month/:day/:title/`
- `post_asset_folder: true` → 每篇文章會建立同名資料夾存放圖片

Front matter 範本：

```yaml
---
title: 文章標題
date: YYYY-MM-DD HH:mm:ss
tags:
---
```

寫完後在本機跑 `npm run server` 預覽，確認無誤再 push 到 `master`。

## 部署管線

**目前生效**：`.github/workflows/pages.yml`（GitHub Pages from Actions 官方模式）

- **觸發條件**：push 到 `master`，或在 Actions tab 上手動 `workflow_dispatch`
- **流程**：
  - `build` job：`actions/checkout@v4` → Node.js 20 → `actions/configure-pages@v5` → `npm install` → `npm run build` → `actions/upload-pages-artifact@v3` 把 `public/` 上傳成 artifact
  - `deploy` job：`actions/deploy-pages@v4` 直接由 artifact 部署到 GitHub Pages
- 使用內建的 `GITHUB_TOKEN` 與 `id-token` OIDC，不需額外 PAT
- 已加上 `concurrency: pages` 防止多次 push 同時部署
- **不再使用 `gh-pages` 分支**：GitHub Pages 的 Source 需設為 **GitHub Actions**（Settings → Pages → Build and deployment → Source）

## 重要設定備忘

- `_config.yml` 的 `url` 與 `root` 是 project page（子目錄）模式：
  - `url: https://nick45chen.github.io/Blog/`
  - `root: /Blog/`
  - 若改名或換 repo，要**同步更新這兩個欄位**，否則靜態資源連結會壞掉
- `theme: next` → 主題本身的設定在 `themes/next/_config.yml`
- `.gitignore` 已排除 `public/`、`node_modules/`、`db.json`、`.deploy*/`，不要把它們加進版控

## Claude 協作守則

- 預設用**繁體中文**回覆（與 commit message / 文章慣例一致）
- 除非使用者明示，**不要自動 commit**，只修改檔案
- 改動 `_config.yml` 或 `themes/next/_config.yml` 後，提醒使用者用 `npm run server` 在本機預覽再 push
- 修改文章 / 設定時，避免一併動到 `themes/next/` 內的主題原始碼，除非使用者明確要求客製主題
