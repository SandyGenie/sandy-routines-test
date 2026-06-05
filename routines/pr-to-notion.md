# Routine：GitHub PR → Notion 子頁面

當有 GitHub Pull Request 事件發生時,把 PR 的資訊寫進 Notion
頁面「Rountine GitHub Test」底下的一個新子頁面。

- **觸發 (Trigger)**:GitHub Pull Request 事件(opened / reopened / synchronize / ready_for_review)。
- **目標 Notion 父頁面**:`376b726f8b83807da4a0de3548352c6d`
  (https://app.notion.com/p/376b726f8b83807da4a0de3548352c6d)

---

## Prompt(把以下內容設定為 Routine 的指令)

有一個 GitHub Pull Request 事件被觸發了。請完成以下工作:

1. 用 GitHub MCP 工具(`mcp__github__pull_request_read`)讀取這個 PR 的詳細資訊,
   至少蒐集:
   - PR 編號與標題
   - 作者(author)
   - 狀態(open / merged / closed、是否 draft)
   - 來源分支(head)與目標分支(base)
   - PR 連結(html_url)
   - 描述(body)
   - 變更摘要(changed files、additions、deletions、commits 數量)

2. 用 Notion MCP 工具(`mcp__Notion__notion-create-pages`)在父頁面
   `376b726f8b83807da4a0de3548352c6d` 底下**新增一個子頁面**,內容如下:

   - **頁面標題**:`PR #<編號> — <PR 標題>`
   - **頁面內容**(Notion Markdown):

     ```
     ## 基本資訊
     - **狀態**:<state>
     - **作者**:<author>
     - **分支**:`<head>` → `<base>`
     - **連結**:<html_url>
     - **建立時間**:<created_at>
     - **最後更新**:<updated_at>

     ## 變更摘要
     - 變更檔案數:<changed_files>
     - 新增/刪除行數:+<additions> / -<deletions>
     - Commits:<commits>

     ## 描述
     <body,若為空則寫「(無描述)」>
     ```

3. 建立完成後,回報新子頁面的網址。

### 注意事項
- 父頁面 ID 固定為 `376b726f8b83807da4a0de3548352c6d`,建立時用
  `parent: { type: "page_id", page_id: "376b726f8b83807da4a0de3548352c6d" }`。
- 每個 PR 事件都建立一個新的子頁面(不覆蓋既有頁面),方便保留歷史紀錄。
- 標題裡若有特殊字元,照原樣保留即可。
- 如果讀不到 PR 或 Notion 寫入失敗,清楚回報錯誤原因,不要靜默略過。
