# Windows 一般帳號安裝 kubectl（免系統管理員）

適用：公司筆電只有一般使用者、不能用系統管理員。  
目的：用 kubectl 操作內部叢集上的 Harbor 與 Java 服務。不在本機裝 JDK。

Python 3.13 與本步驟無關，走另一份說明：`windows-python-user-install.md`。

## 1. 只裝客戶端，不裝執行環境

- 要裝的只有 `kubectl.exe`，放在 `%USERPROFILE%\bin`。
- kubeconfig 由資訊單位給。VPN 或跳板另外處理，不寫進這份說明。
- 不要為了這一步裝 JDK、Maven、Gradle。
- 不要用系統管理員。不要裝到 `C:\Program Files`。
- 不要用 Chocolatey、winget 當預設。這兩條常要系統管理員，或寫到使用者目錄以外。

叢集小版本若資訊單位有指定，跟指定版本。沒指定才用官方 stable。kubectl 與 apiserver 通常只容許相差一個小版本。

## 2. 目前這條：不要用 irm，改瀏覽器下載

`irm`、`Invoke-RestMethod`、從網路抓腳本再執行，實測受限。不要跑先前的 PowerShell 下載段。

1. 瀏覽器打開 https://dl.k8s.io/release/stable.txt ，抄下版本，例如 `v1.37.0`。資訊單位若指定叢集小版本，用那個，不用 stable。
2. 下載 `https://dl.k8s.io/release/<版本>/bin/windows/amd64/kubectl.exe`。ARM64 筆電把 `amd64` 改成 `arm64`。
3. 可選：同一路徑加 `.sha256`，用檔案總管對一下，或稍後在本機 `Get-FileHash`。不要為了校驗再跑 `irm`。
4. 把 `kubectl.exe` 放到 `%USERPROFILE%\bin`。目錄沒有就自己建。

PATH 用開始功能表搜尋「編輯帳戶的環境變數」，在使用者 Path 最前面加 `%USERPROFILE%\bin`。不要用 `setx PATH "%PATH%;..."`。關掉所有 PowerShell 再新開。

## 3. 只改使用者 PATH，不要用 setx 重寫整段 PATH

`setx PATH "%PATH%;..."` 會把系統 PATH 與使用者 PATH 展開後寫回，而且約 1024 字元會被截斷。用下面這段：

```powershell
$bin = Join-Path $env:USERPROFILE 'bin'
$current = [Environment]::GetEnvironmentVariable('Path', 'User')
$parts = @()
if ($current) { $parts = $current -split ';' | Where-Object { $_ -and ($_ -ne $bin) } }
$parts = @($bin) + $parts
[Environment]::SetEnvironmentVariable('Path', ($parts -join ';'), 'User')
```

關掉這個視窗，再新開一個 PowerShell。

## 4. 驗證

```powershell
where.exe kubectl
kubectl version --client
```

應看到：

```text
C:\Users\<你的帳號>\bin\kubectl.exe
```

版本列是 Client Version。這時還沒有 kubeconfig，連不上叢集是正常的。

若 `where.exe` 先出現 Docker Desktop 或其他路徑的 kubectl，代表那個目錄排在使用者 `bin` 前面。把 `%USERPROFILE%\bin` 留在使用者 PATH 最前，再重開視窗。

## 5. kubeconfig

資訊單位給的檔案放到：

```text
%USERPROFILE%\.kube\config
```

目錄不存在就自己建。不要把 token 或 kubeconfig 貼進聊天、Gist、短網址。

連上之後才做：

```powershell
kubectl config current-context
kubectl get ns
```

## 6. Harbor 與叢集上的 Java 服務

- 看日誌、套用清單、進容器、port-forward：只用 kubectl。不裝 JDK。
- Harbor 推映像：資訊單位允許容器 CLI 才在本機裝；否則由 CI 推，筆電只跑 kubectl。
- 本機要編譯或跑 JAR 時才另議可攜式 JDK，路徑仍在使用者目錄。那不是這一步。

## 7. 被擋就停

使用者目錄的 exe 若被 AppLocker 或 WDAC 擋下，停。改向資訊單位申請。不要找繞過方式。
