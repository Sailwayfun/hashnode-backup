# Hashnode Backup Repository

這個 repository 用來接收 Hashnode 文章備份。

## 用途

- 備份 Hashnode 已發布文章到 GitHub
- 保留文章內容的版本歷史
- 方便後續檢查文章異動、做二次整理或搬遷

## 目前綁定的遠端

```bash
git@github.com:Sailwayfun/hashnode-backup.git
```

## 在 Hashnode 啟用 Backup to GitHub

1. 登入 Hashnode。
2. 進入你的 blog dashboard。
3. 打開 `GitHub` 分頁。
4. 找到 `Backup to GitHub`。
5. 安裝 `Hashnode GitHub App`。
6. 在 GitHub 授權頁面選擇帳號或 organization。
7. 選擇這個 repository：

```text
Sailwayfun/hashnode-backup
```

8. 完成 `Install` / `Authorize`。
9. 回到 Hashnode 後，按 `Back up all articles`。
10. 到 GitHub 確認文章檔案已經同步進來。

## 同步後的行為

- 既有文章可以先一次匯入
- 後續已發布文章的編輯和刪除會反映到 GitHub 備份
- 如果要備份草稿，需要使用 Hashnode 的 draft backup 功能

## 注意事項

- 不要改這個 repository 的名稱，否則 Hashnode 備份可能失效
- 這個功能是 `Hashnode -> GitHub` 備份，不是 `GitHub -> Hashnode` 發文
- 如果你要從 GitHub 發文到 Hashnode，應改用 `Publish from GitHub`

## 建議工作方式

1. 把這個 repo 當作備份來源，不直接手動改動文章檔案
2. 文章主編輯流程維持在 Hashnode
3. 需要追蹤差異時，用 GitHub commit history 檢查異動

## 初始化後可用指令

```bash
cd hashnode-backup
git status
git add README.md
git commit -m "Add README for Hashnode backup setup"
git push -u origin main
```

## 參考文件

- Hashnode GitHub Integration: https://docs.hashnode.com/blogs/blog-dashboard/github
- Backup articles to GitHub: https://docs.hashnode.com/help-center/github/how-to-backup-articles-to-github
- Publish from GitHub: https://docs.hashnode.com/help-center/github/how-to-set-up-github-as-source
