# Windows 一般帳號安裝 Python（免系統管理員）

適用：公司筆電只有一般使用者、不能用系統管理員。  
目的：之後本機試跑 AI 代理或腳本。不影響 kubectl、Harbor、叢集上的 Java 服務。

建議版本：Python 3.13。某套件明確不支援時再用 3.12。不要用已停止支援的 3.10。

## 1. 安裝 uv（使用者目錄，不必系統管理員）

開啟 PowerShell（不要用系統管理員），整行貼上後執行：

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

看到成功訊息後，關掉這個視窗，再新開一個 PowerShell，讓 PATH 生效。

uv 會放在：

```text
%USERPROFILE%\.local\bin
```

## 2. 安裝 Python 3.13

```powershell
uv python install 3.13
uv python pin 3.13
uv run python -c "import sys; print(sys.executable); print(sys.version)"
```

應看到版本列含 `3.13`。直譯器在：

```text
%LOCALAPPDATA%\uv\python
```

## 3. 每個小專案各自隔離，不要裝進全域

```powershell
cd $env:USERPROFILE\work
mkdir agent-try -Force
cd agent-try
uv init
uv add httpx
uv run python -c "import httpx; print(httpx.__version__)"
```

之後在該目錄執行腳本：

```powershell
uv run python main.py
```

## 4. 若公司擋了從網路執行腳本

改走官網安裝程式，只裝給目前使用者：

1. 瀏覽器打開 https://www.python.org/downloads/release/python-31316/
2. 下載 Windows installer (64-bit)
3. 安裝時勾選 Add python.exe to PATH
4. 選 Customize，確認路徑在 `%LOCALAPPDATA%\Programs\Python`
5. 不要勾選 for all users，不要勾選安裝到 C:\Program Files

裝完到「設定 → 應用程式 → 進階應用程式設定 → 應用程式執行別名」，把 App Installer 的 python.exe 與 python3.exe 關掉。否則打 python 會開到 Microsoft Store。

## 5. 先不要做的事

- 不要為了 kubectl 或 Harbor 另裝 Java
- 不要用系統管理員身份安裝
- 端點若有 AppLocker / WDAC，使用者目錄的 exe 仍可能被擋；被擋就停，改向資訊單位申請
