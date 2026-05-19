---
name: publish-draft
description: >
  把 source/_drafts/ 中的草稿發佈到 source/_posts/，自動補 date front matter，
  並做發佈前 pre-flight 檢查（title/tags/TODO 殘留/連結格式/參考資料段落）。
  觸發語句包含「發佈草稿」「publish」「publish-draft」「/publish-draft」「把草稿發出去」
  「把草稿移到 posts」「準備發佈」。
  發佈前會自動跑一次中文排版校稿。
  不會自動 commit 或 push；最後提示使用者用 /hexo-redeploy 或手動 commit。
---

# Publish Draft（nick-blog 專案版）

把 `source/_drafts/` 的草稿移到 `source/_posts/`，補上 `date` front matter，並做發佈前檢查。

> 此 skill 只負責「草稿 → 正式文章」這一段。撰寫初稿請用姊妹 skill `draft-post`。
> 部署（push 觸發 GitHub Actions）由 `/hexo-redeploy` 或使用者手動處理。

## 何時觸發

使用者輸入下列任一語句時觸發：

- `/publish-draft`、`/publish-draft <檔名>`
- 「發佈草稿」「把草稿發出去」「把草稿移到 _posts」
- 「準備發佈」「準備發 X 那篇」
- 明確說要把 `source/_drafts/<檔名>` 移到 `source/_posts/`

若使用者說「commit」「push」「部署」「deploy」「redeploy」，**不要**自動觸發；那是 `git-commit` 或 `hexo-redeploy` 的職責。

## Pre-flight

1. 當前工作目錄是 `/Users/knst/Documents/Nick/Project/nick-blog`。
2. `source/_drafts/` 目錄存在且有 `.md` 檔。若空的，停下來告知使用者。

## 執行流程

### Step 1：挑檔

- 若使用者已指定檔名（例 `/publish-draft 2026-05-19-foo.md`），直接用。
- 若沒指定，列出 `source/_drafts/*.md` 候選清單請使用者選：

  ```
  待發佈的草稿：
  1. 2026-05-19-android-viewmodel-lifecycle.md
  2. 2026-05-18-hexo-next-customization.md

  請告訴我要發哪一篇（編號或檔名）。
  ```

- 不要一次發多篇。一次一篇，發完再問下一篇。

### Step 2：Pre-flight 檢查

Read 目標檔，依下表逐項檢查：

| 檢查項目 | 通過條件 | 不通過時 |
|---|---|---|
| `title` 存在 | front matter 有 `title` 且非空、非「未命名」 | 列出問題、暫停發佈 |
| `tags` 存在 | front matter 有至少 1 個 tag | 列出問題、暫停發佈 |
| 無占位符 | 全文無 `TODO`、`TKTK`、`XXX`、`FIXME` | 列出位置（行號）、暫停發佈 |
| 外部連結用 `https://` | 沒有 `http://`（除非是 localhost / 範例） | 列出位置、暫停發佈 |
| 「參考資料」段落存在 | 文末有 `## 參考資料` 或 `## References` | 列出問題、暫停發佈 |
| 標題章節完整 | 包含 `## Why`、`## What`、`## How`、`## 注意事項`、`## 參考資料`（順序不限） | 若缺，列出來請使用者確認是否例外（心得 / 筆記文可豁免）|

任一項不通過，輸出格式：

```
發佈前檢查未通過：

❌ tags 為空
❌ 第 87 行還有 TODO：「TODO: 補上效能比較數據」
⚠️ 缺少「## What」章節（如是心得文可豁免）

請先修正，再執行 /publish-draft。
```

並停下來等使用者處理。**不要**自動補欄位、不要自動刪 TODO。

### Step 3：自動校稿

通過 Step 2 後，套用 [chinese-copywriting](../chinese-copywriting/SKILL.md) 規則速查表跑一遍：

- 中英間距、全 / 半形標點、專名大小寫
- 不確定處列出來請使用者確認

修正後才進 Step 4。

### Step 4：發佈

執行：

```bash
cd /Users/knst/Documents/Nick/Project/nick-blog
npx hexo publish post "<檔名去掉日期前綴與副檔名>"
```

例如檔案 `2026-05-19-android-viewmodel-lifecycle.md`，指令是：

```bash
npx hexo publish post "android-viewmodel-lifecycle"
```

`hexo publish` 會：
- 把檔案從 `source/_drafts/` 移到 `source/_posts/`
- 自動補上 `date: YYYY-MM-DD HH:mm:ss` front matter

**Fallback**：如果 `hexo publish` 失敗（例：找不到對應的 draft、檔名格式問題），改用手動方式：

1. Read 草稿原檔
2. 用 Edit 在 front matter 補上 `date: <現在時間 YYYY-MM-DD HH:mm:ss>`
3. `mv source/_drafts/<檔名>.md source/_posts/<檔名>.md`

執行後用 `ls source/_posts/` 確認新檔案存在。

### Step 5：回報摘要

格式：

```
✅ 已發佈：source/_posts/2026-05-19-android-viewmodel-lifecycle.md

front matter:
- title: Android ViewModel 生命週期：為什麼比 Activity 還長
- date: 2026-05-19 14:32:11
- tags: [Android, ViewModel, Architecture]

下一步：
1. 本機預覽：npm run server
   開 http://localhost:4000/Blog/ 確認排版與連結
2. 確認無誤後，部署到 GitHub Pages：
   - 觸發 /hexo-redeploy（會 clean + build + commit + push）
   - 或手動 git add / commit / push
```

## 不要做的事

- ❌ 自動 `git add` / `git commit` / `git push`
- ❌ 自動跑 `npm run build`、`npm run deploy`、`/hexo-redeploy`
- ❌ Pre-flight 任一項不通過時硬發佈
- ❌ 自動補空白的 `tags` 欄位（要讓使用者主動決定 tag）
- ❌ 自動刪 TODO / TKTK 占位符（可能含未完成的內容）
- ❌ 一次發佈多篇（一次一篇）
- ❌ 修改文章正文內容（除中文排版校稿外）
- ❌ 修改 `themes/next/`、`_config.yml`、`scaffolds/`

## 邊界案例

- **草稿無 `## 參考資料` 段**：詢問使用者是否為心得 / 筆記文，要不要例外通過。
- **使用者指定的檔名找不到**：列出實際存在的草稿，請使用者重選。
- **檔名前綴日期與今天不同**：發佈時 `date` 用「現在時間」而非檔名日期。使用者若希望保留檔名日期，需明確要求。
- **檔名含中文 slug**：`hexo publish post "<slug>"` 對中文 slug 有時會失敗，遇到就走 Fallback（手動 Edit + mv）。
- **多個草稿同 slug 不同日期**：選擇最新日期的；或請使用者明確指定。

## 範例

```
你：/publish-draft

我：（列出候選）
待發佈的草稿：
1. 2026-05-19-android-viewmodel-lifecycle.md

請告訴我要發哪一篇。

你：1

我：（Step 2 檢查）
✅ title: Android ViewModel 生命週期...
✅ tags: [Android, ViewModel, Architecture]
✅ 無 TODO 占位符
✅ 外部連結都用 https://
✅ 「## 參考資料」段落存在
✅ 五段式結構完整

（Step 3 中文排版校稿 → 已套用）

（Step 4 執行 npx hexo publish post "android-viewmodel-lifecycle"）

（Step 5 回報）
✅ 已發佈：source/_posts/2026-05-19-android-viewmodel-lifecycle.md
...
```
