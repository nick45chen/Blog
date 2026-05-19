# 代碼玩家 Nick's Blog

我的個人技術筆記，用 [Hexo](https://hexo.io/) + [NexT](https://theme-next.js.org/) 主題打造，內容涵蓋 Android、Material Design、Hexo 設定等開發隨筆。

線上閱讀：<https://nick45chen.github.io/Blog/>

## 技術棧

- Hexo 8.1.2
- NexT 主題（放在 `themes/next/`，未使用 git submodule）
- GitHub Actions 自動部署到 GitHub Pages

## 本機開發

```bash
npm install      # 安裝相依套件
npm run server   # 本機預覽，預設 http://localhost:4000/Blog/
npm run build    # 產生靜態檔到 public/
npm run clean    # 清除 public/ 與 db.json
```

新增文章：

```bash
npx hexo new "文章標題"
```

## 更新 Hexo CLI 與依賴

更新 Hexo CLI：

```bash
npm i hexo-cli -g
```

更新專案依賴（用 `npm-upgrade` 找出最新版本後一次升級）：

```bash
npm install -g npm-upgrade
npm-upgrade
npm update
```

## 其他

協作守則、目錄結構、部署流程、中文排版規則等細節請參考 [`CLAUDE.md`](./CLAUDE.md)。
