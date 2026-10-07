# 線 B：Grok／Grokbot 處理 Gist 的最低授權

決定日期：2026-10-07。帳號：sanhuang（Taz Huang）。

## 現況

- Grok 的 GitHub 連接器已登入 sanhuang。
- 可以建公開 repo、推檔。`sanhuang/handoff` 已在用。
- 建立 Gist 回 403：`Resource not accessible by integration`。連接器沒有 Gist 權限，不是檔案內容問題。
- Gist 不是 repo。GitHub 不能把 Gist 權限縮到 `handoff` 或任何單一 repo。

## 最低授權（現在只開這個）

只處理 Gist 時，只要帳號權限，不要加 repo 權限。

| 管道 | 最低授權 | 不要加 |
| --- | --- | --- |
| OAuth／classic | scope：`gist` | 不要加 `repo`、`workflow`、`admin:org`、`delete_repo` |
| Fine-grained PAT | Account → Gists = Read and write。Repository access = No access | 不要加 Contents、Administration、Secrets、Workflows |
| GitHub App（Grok／Grok Bot 安裝） | Account permissions → Gists = Read and write | Repository access 先維持現況；不要為了 Gist 改成 All repositories |

公開 Gist 與秘密 Gist 都走同一個 `gist` 寫入權限。沒有更小的「只能建公開 Gist」範圍。

## 兩個產品分開授權

- Grok 聊天連接器：grok.com → Connectors → GitHub。重連後只核准多出來的 Gists。
- Grok Bot：自己的 GitHub App／plugin。Bot 沒有獨立身分，權限不會超過登入者。安裝時 Gists 是帳號權限；repo 範圍仍可只選 `handoff`。兩邊不要互相當成已經授權。

## 日後才擴充

現在不要開。要做下列事時再加，而且能選 repo 就只選 `sanhuang/handoff`。

- 推檔、改 README：Contents read/write，只限 `handoff`
- 開 issue／PR：Issues、Pull requests，只限 `handoff`
- 刪 repo、改權限、看 secret：不預開

在 Gist 權限下來之前，跨裝置頁面繼續放 `sanhuang/handoff`，不稱作 Gist。沒有 GitHub 時改 rentry，也不得稱作 Gist。
