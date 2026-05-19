---
name: draft-post
description: >
  從一個文章主題開始，自動研究官方資料、產出大綱、撰寫初稿並放到 source/_drafts/。
  觸發語句包含「寫草稿」「draft」「draft-post」「/draft-post」「幫我寫一篇」「幫我起一個草稿」
  「寫一篇關於…的文章」「寫一篇…的草稿」。
  使用 WebSearch / WebFetch 蒐集官方來源，套用過往語氣與中文排版規則。
  固定五段式結構：Why / What / How / 注意事項 / 參考資料。
  不會自動 commit，也不會跑 hexo publish 或 build。
---

# Draft Post（nick-blog 專案版）

從一個技術主題開始，幫 nick-blog 產出一篇可審核的草稿，落到 `source/_drafts/`。

> 此 skill 只負責「研究 + 撰寫初稿」。發佈動作由姊妹 skill `publish-draft` 處理。
> 兩個 skill 解耦的好處：你可以在任何 session 觸發 `/publish-draft`，不必跟 `/draft-post` 綁定。

## 何時觸發

使用者輸入下列任一語句時觸發：

- `/draft-post`、`/draft-post <主題>`
- 「幫我寫一篇關於 X 的草稿」「幫我起一個 X 的草稿」「寫一篇 X 的文章」
- 「draft X」「draft-post X」
- 明確說要把成果放到 `source/_drafts/`

若使用者只說「幫我整理 X 的筆記」「memo X」而沒提到要發到部落格，**不要**自動觸發；先問清楚。

## Pre-flight

執行流程前先確認：

1. 當前工作目錄是 `/Users/knst/Documents/Nick/Project/nick-blog`。若不是，停下來告知使用者。
2. `source/_drafts/` 目錄存在。若不存在，先 `mkdir -p source/_drafts`。
3. Read 以下 style anchor 文章前 50 行，校準語氣與排版風格（不要每篇都全部讀完，前 50 行抓開頭與第一段論述就夠）：
   - `source/_posts/2020-02-19-物件導向設計原則-SOLID-2-Open-Closed-Principle.md`
   - `source/_posts/2020-02-18-物件導向設計原則-SOLID-1-Single-Responsibility-Principle.md`
   - `source/_posts/2018-12-01-Android-Gaussian-blur-Effect.md`

## 執行流程

### Step 1：釐清需求

使用 AskUserQuestion 一次性問 3–5 題（不要分多次問）：

| 題目 | 預設選項 |
|---|---|
| 目標讀者 | 初學者 / 中階開發者 / 進階 |
| 預期長度 | 短篇筆記（≈ 500 字） / 標準教學（≈ 1500 字） / 深度文（≈ 3000 字） |
| 核心角度 | 原理解析 / 實作教學 / 比較選型 / 心得 / 踩坑紀錄 |
| 是否含 code example | 要 / 不要 / 視內容而定 |
| 既有相關文章是否連結 | 要（請列出可能候選） / 不用 |

若使用者已在觸發指令中明示部分答案（例：「幫我寫一篇給初學者看的 RecyclerView 教學」），不要再問已知資訊。

### Step 2：研究

優先順序明確：

1. **官方來源**（必有）
   - Android：`developer.android.com`
   - iOS / Apple：`developer.apple.com`
   - Web：`developer.mozilla.org`（MDN）
   - Hexo：`hexo.io`、NexT 主題的 GitHub README
   - 程式語言官方文件、套件 GitHub repo 的 README / Wiki
2. **主流社群**（補充）
   - Stack Overflow 高票答案
   - 官方部落格 / RFC / GitHub Issue 討論
3. **個人部落格 / Medium**（佐證用，需在文中標示「社群討論」）

**做法**：

- 用 WebSearch 找 5–10 個來源候選。
- 用 WebFetch 撈 top 3–5 篇關鍵內容。
- 整理成 markdown 表格存到 `source/_drafts/.research/<slug>.md`：

  ```markdown
  | 事實 / 觀點 | 來源 URL | 來源權威 | 取得日期 |
  |---|---|---|---|
  | RecyclerView 取代 ListView 的主因是 ViewHolder pattern 強制化 | https://developer.android.com/... | 官方 | 2026-05-19 |
  ```

- `.research/` 目錄已被 `.gitignore`，不會入版控。

**退場條件**：若找不到至少 **2 個官方來源**，停下來回報使用者：

> 「主題『X』找不到足夠的官方來源，建議：
> 1. 縮小主題範圍（例：『X 的某個元件』）
> 2. 改寫成『心得文 / 踩坑紀錄』而非教學
> 3. 放棄這個主題
> 你希望怎麼處理？」

不要硬擠出來。

### Step 3：大綱 checkpoint（必須停下來等使用者確認）

整理大綱呈現給使用者，**不可跳過、不可直接寫稿**。

格式：

```
建議大綱：

# <文章標題候選 1>
（候選 2、候選 3 也列出）

## Why
- 解決什麼問題 / 帶來什麼效益
- （要點 1–3）

## What
- 定義 / 範圍
- 與相鄰概念的差異
- （要點 1–3）

## How
- 步驟 1：…
- 步驟 2：…
- （可含 code example 的小節）

## 注意事項
- 常見坑 1
- 邊界條件 2
- 效能 / 安全 / 相容性 3

## 參考資料
- 來源 1（官方）
- 來源 2（官方）
- 來源 3（社群討論，補充用）

預估字數：≈ XXXX 字
```

問使用者：「這個大綱你 OK 嗎？要調整哪邊？」等明確的「OK / 開始寫 / 可以了」才往下走。

### Step 4：撰寫初稿

#### 4.1 檔名與路徑

- 用今天日期作前綴：`YYYY-MM-DD-<slug>.md`（slug 是英文小寫 + 連字號，或保留與標題對應的中文）。
- 寫到 `source/_drafts/YYYY-MM-DD-<slug>.md`。
- **覆寫保護**：若同檔名已存在，**先停下來**問使用者：
  - 覆寫
  - 在 slug 後加 `-v2`
  - 改新 slug
- 使用 Write 工具寫入。

#### 4.2 Front matter

套 `scaffolds/draft.md`，**不含 `date`**（由 `publish-draft` 補上）：

```yaml
---
title: <文章標題>
tags:
  - <tag 1>
  - <tag 2>
---
```

至少 1 個 tag，建議 2–4 個。

#### 4.3 固定五段式結構

| 章節 | `##` 標題 | 內容 |
|---|---|---|
| 1 | `## Why` | 要解決什麼問題、可以獲得什麼效益、讀者為什麼要看下去 |
| 2 | `## What` | 這個東西 / 概念是什麼，定義、範圍、跟相鄰概念的差異 |
| 3 | `## How` | 怎麼做，步驟、code example、設定。可用 `###` 切細項 |
| 4 | `## 注意事項` | 常見坑、邊界條件、效能 / 安全 / 相容性提醒 |
| 5 | `## 參考資料` | 引用來源清單 + 取得日期 |

不要新增「前言」「結論」段。Why 段本身就是前言，注意事項收尾就是結論。

#### 4.4 引用規則

- 段落中引述事實時，inline 標註來源：`官方文件指出 [Activity lifecycle](https://developer.android.com/...) 會在…`。
- **不要**直接複製貼上 WebFetch 抓到的整段文字。必須改寫成自己的話，並標明來源。
- 文末「參考資料」用清單列出所有引用，每條附上取得日期：

  ```markdown
  ## 參考資料

  - [Activity lifecycle | Android Developers](https://developer.android.com/guide/components/activities/activity-lifecycle)（取得日期：2026-05-19）
  - [ViewModel overview | Android Developers](https://developer.android.com/topic/libraries/architecture/viewmodel)（取得日期：2026-05-19）
  ```

#### 4.5 語氣速查表

- **句長**：以 30–60 字為主。冗長的複合句要拆成短句。
- **白話**：避免艱澀術語，必要時用一句白話解釋（例：「ViewHolder 的本質就是『先把元件抓好放著，等下要顯示時直接套用，不用每次都重新去 findViewById』」）。
- **保留英文原名**：`Android`、`iOS`、`Hexo`、`NexT`、`JavaScript`、`TypeScript`、`Node.js`、`npm`。
- **Android 元件名連寫**：`AlarmManager`、`JobScheduler`、`RecyclerView`、`ViewPager`（PascalCase）。
- **不用非正式縮寫**：`Ts → TypeScript`、`h5 → HTML5`、`nextjs → Next.js`。
- **程式碼**：fenced code 並標語言（` ```kotlin `、` ```bash `、` ```yaml ` 等）。

#### 4.6 中文排版

寫稿過程同步遵守 [chinese-copywriting](../chinese-copywriting/SKILL.md) 的規則速查表（中英間距、全 / 半形標點、專名大小寫）。

### Step 5：自動校稿

寫完後在同一輪自我複查一遍中文排版：

- 中英之間有空格
- 中文與數字之間有空格（除度數 / 百分比例外）
- 中文整句用全形 `，。！？：；「」（）`
- 英文整句用半形
- 沒有重複標點（`！！！！`）
- 專名大小寫正確

若有不確定的修正，列在「不確定處」交使用者決定。

### Step 6：回報摘要

格式：

```
草稿已建立：source/_drafts/2026-05-19-recyclerview-tutorial.md

字數：≈ 1820 字
引用來源：6 筆（4 官方、2 社群）
研究筆記：source/_drafts/.research/recyclerview-tutorial.md

下一步：
1. 本機預覽：npm run server -- --draft
   開 http://localhost:4000/Blog/ 確認
2. 審核 / 調整：直接編輯草稿檔，或請我修訂特定段落
3. 滿意後發佈：/publish-draft 2026-05-19-recyclerview-tutorial.md
```

## 不要做的事

- ❌ 自動 `git add` / `git commit` / `git push`
- ❌ 跑 `hexo build`、`hexo publish`、`hexo deploy`、`npm run deploy`、清 `public/`
- ❌ 直接複製 WebFetch 抓回來的整段文字（必須改寫並標來源）
- ❌ 在草稿 front matter 寫 `date` 欄位（由 `publish-draft` 補）
- ❌ 跳過 Step 3 大綱 checkpoint 直接寫初稿
- ❌ 沒找到 2 個官方來源還硬寫（要先回報退場條件）
- ❌ 覆寫已存在的草稿而不問
- ❌ 修改 `themes/next/`、`_config.yml`、`scaffolds/`
- ❌ 改新 / 改寫 `source/_posts/` 既有文章（這是發佈過的內容）

## 邊界案例

- **主題太籠統**（例：「寫一篇 Android 的文章」）：先用 Step 1 釐清，把它切到一個可寫的點。
- **主題太冷門 / 無官方資料**（例：某個只在 Stack Overflow 提過的小技巧）：套退場條件。
- **使用者已給結構**：尊重使用者的章節結構，不強套五段式。但內部仍套排版規則。
- **要含 code example**：code 直接寫在 fenced code block，並在前後段用白話解釋這段程式碼解決什麼問題、為什麼這樣寫。
- **同主題已有舊文**：在 Step 1 詢問是否要連結舊文，並在 Why 段提及「先前文章 [X](url) 提過 …，本篇延伸 …」。
- **使用者不滿意大綱**：根據回饋調整，再 checkpoint 一次。不要強推自己的版本。

## 範例：完整流程片段

```
你：/draft-post Android ViewModel 的生命週期

我：（Step 1 釐清）
[AskUserQuestion]
- 目標讀者：初學者 / 中階 / 進階
- 預期長度：短 / 標準 / 深度
- 角度：原理 / 實作 / 比較
- 是否含 code

你：中階、標準、原理、要 code

我：（Step 2 研究 → 寫到 .research/）
（Step 3 大綱）
建議大綱：
# Android ViewModel 生命週期：為什麼比 Activity 還長
## Why ...
## What ...
## How ...
## 注意事項 ...
## 參考資料 ...

預估 1500 字。OK 嗎？

你：OK

我：（Step 4 寫稿 → 寫到 source/_drafts/2026-05-19-android-viewmodel-lifecycle.md）
（Step 5 自查排版）
（Step 6 回報）
草稿已建立：source/_drafts/2026-05-19-android-viewmodel-lifecycle.md
...
```
