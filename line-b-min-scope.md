# 線 B：最小權限改存單一公開 repo

決定日期：2026-10-07。帳號：sanhuang（Taz Huang）。

## 決定

- Gist 權限是帳號級。GitHub 不能把 Gist 縮到單一 repo。
- 先前用連接器建立 Gist 回 403，整合權限不足，沒有寫入任何 Gist。
- 要最小權限時，不開 Gist。改存 sanhuang 下單一公開 repo：`sanhuang/handoff`。
- 這個 repo 只放可跨裝置手動輸入的公開說明。不放密鑰、kubeconfig、token、密碼。
- 沒有 GitHub 登入時，仍改走 rentry 公開頁。rentry 不是 Gist，回覆時不得稱作 Gist。

## 之後出檔路徑

1. 切對話切片，寫 UTF-8 Markdown 到 `artifacts/<slug>.md`。
2. 推到 `sanhuang/handoff` 的 `main`，路徑 `<slug>.md`。
3. 公開頁用 GitHub 上該檔的頁面，再轉短網址。
4. 短網址放第一行，原頁第二行，本機檔第三行。

## 既有公開頁（rentry，不是 Gist）

Windows 一般帳號安裝 Python 的說明仍在 rentry。同一份也放進本 repo。

- 短網址：https://tinyurl.com/23kcpkha
- 原頁：https://rentry.co/win-py-user
- 本機檔：artifacts/windows-python-user-install.md
