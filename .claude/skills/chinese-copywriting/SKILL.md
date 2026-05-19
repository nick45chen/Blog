---
name: chinese-copywriting
description: >
  套用中文文案排版規範（基於 sparanoid/chinese-copywriting-guidelines）檢查並修正
  nick-blog（Hexo + NexT, zh-tw）的文章排版：中英間距、全/半形標點、專有名詞大小寫等。
  觸發語句包含「中文排版」「排版檢查」「套用中文文案規範」「中英間距」「中文文案」
  「Chinese copywriting」「/chinese-copywriting」。
  目標檔案預設為 source/_posts/*.md；自動跳過 front matter、fenced code、行內 code、URL、HTML 標籤、Hexo tag plugin。
  不會自動 commit，不會跑 hexo build。
---

# Chinese Copywriting（nick-blog 專案版）

把 nick-blog 的 Hexo Markdown 文章排版調整為符合
[sparanoid/chinese-copywriting-guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines) 的規範。

> 本檔為 `nick-blog` 專案版，會覆寫全域同名 skill。除了通用規則外，多了 Hexo / NexT 特有的處理（front matter 欄位、tag plugin、圖片資料夾）。

## 何時觸發

使用者輸入下列任一語句時觸發：

- `/chinese-copywriting`
- 「幫我做中文排版」「中文排版檢查」「套用中文文案規範」
- 「中英間距」「全形標點」「修正排版」「中文 lint」
- 明確指向 `source/_posts/*.md` 並提到上述任何一項

若使用者只說「校稿」「潤稿」「改錯字」而沒提到排版，**不要**自動觸發；先問清楚。

## 執行流程

### Step 1：確認目標範圍

預設目標路徑：`/Users/knst/Documents/Nick/Project/nick-blog/source/_posts/`。

向使用者確認要處理：
- 單一檔案（檔名格式 `YYYY-MM-DD-title.md`）
- 全部 `source/_posts/*.md`
- 一段使用者剪貼的文字

若未指定，列出 `source/_posts/` 候選清單請使用者挑選；不要全跑。

### Step 2：讀檔並標出要跳過的區段

用 Read 讀取目標檔。處理時必須**跳過**以下區段，內容完全不動：

| 區段 | 識別方式 |
|---|---|
| YAML front matter | 檔頭 `---` 到下一個 `---` 之間（含 `title`、`date`、`tags`、`categories` 等所有欄位）|
| Fenced code block | `` ``` `` ... `` ``` `` 之間（含語言標籤行） |
| Indented code block | 連續 4 空格或 tab 起頭的段落 |
| 行內 code | `` `...` `` 內部文字 |
| URL | `https://...`、`http://...`、`mailto:...` |
| Markdown link 的 URL | `[text](url)` 的 `url` |
| Image 路徑 | `![alt](path)` 的 `path`，包含同名資料夾下的圖片參照 |
| HTML tag | `<...>` 區塊與屬性 |
| **Hexo tag plugin** | `{% ... %}` ... `{% end... %}` 之間，例如 `{% codeblock %}`、`{% youtube %}`、`{% raw %}` |
| 數學式 | `$...$`、`$$...$$` |

行內 code 兩側仍要套中英間距規則（如 `使用 \`class\` 關鍵字`）。

### Step 3：逐條套用規則

依下方「規則速查表」檢查並用 Edit 修正。每次 Edit 改一處或一類，避免大範圍 replace_all 出錯。

**修改限制**：
- 不要改寫作者的措辭、語氣、段落結構
- 不要 reflow 換行（保留原本的硬換行位置）
- 不要新增或刪除標點以外的字
- 既有已符合規範的不要重複加空格
- **不要動 front matter 欄位值**（即使 `title: 使用Android` 中英沒空格也不要改，避免影響永久連結）

### Step 4：輸出摘要

完成後給簡短摘要，例如：

```
已修改 source/_posts/2020-02-18-Android-Power-Optimization.md：
- 中英間距：12 處
- 半形→全形標點：5 處
- 品牌大小寫：2 處（github → GitHub）
不確定處：1 處（行 42「Job Scheduler」是否應為 `JobScheduler`），請確認

下一步建議：跑 `npm run server` 在本機預覽（http://localhost:4000/Blog/）確認排版無誤再 push。
```

不要自動 commit、不要跑 `hexo build`、`npm run deploy`。

## 規則速查表

### 1. 間距

| 規則 | ✅ 正確 | ❌ 錯誤 |
|---|---|---|
| 中英之間加空格 | `在 LeanCloud 上` | `在LeanCloud上` |
| 中文與數字之間加空格 | `花了 5000 元` | `花了5000元` |
| 數字與單位之間加空格 | `10 Gbps`、`20 TB` | `10Gbps`、`20TB` |
| **例外**：度數、百分比不加空格 | `90°`、`15%` | `90 °`、`15 %` |
| 全形標點兩側不加空格 | `iPhone，好開心` | `iPhone ，好開心` |
| 超連結與中文之間建議加空格 | `請 [提交 issue](#) 並通知` | （兩種寫法皆可，但加空格較佳）|

### 2. 標點符號

| 規則 | ✅ 正確 | ❌ 錯誤 |
|---|---|---|
| 中文整句用全形標點 `，。！？：；「」（）` | `嗨！你知道嗎？` | `嗨! 你知道嗎?` |
| 數字一律半形 | `1000` | `１０００` |
| 整句英文用半形標點 | `"Stay hungry, stay foolish."` | `"Stay hungry，stay foolish。"` |
| 不要重複標點 | `戰勝了巴西隊！` | `戰勝了巴西隊！！！！！` |
| zh-tw 引號用直角引號 | `「老師，『紊』是什麼意思？」` | `"老師，"紊"是..."` |

### 3. 專有名詞大小寫

維持品牌官方寫法。本部落格常見：

| 類別 | 正確 |
|---|---|
| 平台 / OS | `iOS`、`iPadOS`、`macOS`、`Android`、`Windows`、`Linux` |
| 服務 | `GitHub`、`GitLab`、`YouTube`、`Google Play`、`App Store` |
| Android 元件 | `Activity`、`Fragment`、`Service`、`Toolbar`、`AlarmManager`、`JobScheduler`、`WorkManager`、`RecyclerView`、`ViewPager` |
| Android 概念 | `Material Design`、`Job Scheduler`（功能名）vs `JobScheduler`（類別名）|
| 程式語言 / 工具 | `JavaScript`、`TypeScript`、`Java`、`Kotlin`、`Node.js`、`npm`、`Hexo`、`NexT`、`React`、`Vue.js` |
| 縮寫 | `HTML5`、`CSS3`、`API`、`URL`、`HTTP`、`HTTPS`、`JSON`、`SDK`、`UI`、`UX` |
| 連字號保留 | `Wi-Fi`、`e-mail`、`Material-UI` |

### 4. 避免不正式縮寫

| ❌ 不要 | ✅ 改成 |
|---|---|
| Ts、ts | TypeScript |
| h5 | HTML5 |
| RJS、reactjs | React |
| nextjs | Next.js |
| nodejs | Node.js |
| FED | 前端工程師 / front-end developer |

## nick-blog 特殊處理

### Front matter 不動

`---` 區段內的所有欄位（`title`、`date`、`tags`、`categories`、`comments` 等）一律保留原樣。
即使 `title` 字串本身違反排版規則，也**不要**改 —— 改 `title` 會影響永久連結與站內搜尋。
若發現 `title` 有問題，列在「不確定處」交使用者決定。

### Hexo tag plugin 不動

下列區段一律跳過：

```
{% codeblock lang:java %}
... 程式碼 ...
{% endcodeblock %}

{% youtube VIDEO_ID %}

{% raw %}
... 內容不做任何排版處理 ...
{% endraw %}
```

### 圖片與資源資料夾

`post_asset_folder: true`，每篇文章有同名資料夾。圖片參照如：

```
![架構圖](2020-02-18-Android-Power-Optimization/architecture.png)
```

路徑（括號內）不動；alt 文字（中括號內）套排版規則。

### 完成後建議

文章改完後提醒使用者：

```
建議下一步：
1. npm run server
2. 開 http://localhost:4000/Blog/ 確認排版
3. 確認無誤後 git add / commit / push（master 推上去後 GitHub Actions 會自動部署）
```

## 不要做的事

- ❌ 改動程式碼區塊、URL、front matter 欄位值
- ❌ 改動 `{% %}` Hexo tag plugin 區段
- ❌ 改寫作者語氣、措辭、段落結構
- ❌ 重新換行（reflow）已有段落
- ❌ 自動 `git add` / `git commit` / `git push`
- ❌ 跑 `hexo build`、`npm run deploy`、清 `public/`
- ❌ 修改 `themes/next/` 內任何主題原始碼
- ❌ 修改 `_config.yml`、`scaffolds/` 等專案設定

## 邊界案例

- **連結文字內也要套規則**：`[使用 GitHub](url)` 中的 `使用 GitHub` 套規則，URL 不動。
- **行內 code 兩側套規則，內部不動**：`使用 \`class\` 關鍵字` 中，` \`class\` ` 與中文之間要有空格。
- **既有半形符號需保留**：URL、檔案路徑、版本號（`v1.2.3`）、Email、程式碼片段、技術術語官方寫法（如 `Wi-Fi`、`e-mail`、`Node.js`）。
- **混淆字元**：發現 `１ ２ ３`（全形數字）一律改半形；發現 `，。` 出現在英文整句中要改為 `, .`。
- **Android 類別 vs 功能名**：`JobScheduler` 是類別名要連寫；`Job Scheduler` 是功能名要分寫。不確定時保留原樣並列入「不確定處」。
- **不確定時**：列在摘要的「不確定處」段，交使用者確認，不要自行決定。

## 範例 Before / After

**Before**：
```
使用AlarmManager每過15分鐘叫醒設備一次,前提是要當設備連上Wi-Fi或接上電源 (利大於弊).
```

**After**：
```
使用 AlarmManager 每過 15 分鐘叫醒設備一次，前提是要當設備連上 Wi-Fi 或接上電源（利大於弊）。
```

修正項目：
1. 中英間距 `使用AlarmManager` → `使用 AlarmManager`
2. 中數間距 `每過15分鐘` → `每過 15 分鐘`
3. 中英間距 `Wi-Fi或` → `Wi-Fi 或`
4. 半形 → 全形標點 `,` → `，`、`.` → `。`
5. 半形括號 → 全形 `(利大於弊)` → `（利大於弊）`
