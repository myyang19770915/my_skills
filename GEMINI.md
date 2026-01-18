# Antigravity Project Rules for Gemini

此檔案定義了 AI 助理在協助 "Antigravity" 專案開發時的行為準則與技術規範。

## 1. 語言規範 (Language Standards)
- **主要語言**：所有對話、解釋、建議與回應內容，必須嚴格使用 **繁體中文 (Traditional Chinese)**。
- **專有名詞**：保留技術專有名詞的英文原文（例如：`uv`, `FastAPI`, `React`, `PowerShell`），但在必要時需用繁體中文補充說明。

## 2. Python 環境與工具鏈 (Environment & Toolchain)
- **核心工具**：強制使用 **[uv](https://github.com/astral-sh/uv)** 作為 Python 專案管理與虛擬環境工具。
- **虛擬環境操作**：
  - 建立環境：使用 `uv venv`。
  - 安裝套件：使用 `uv pip install <package>` 或 `uv add <package>`。
  - **禁止使用** 傳統的 `pip install` 或 `python -m venv`，除非特殊情況無法使用 `uv`。
- **程式碼執行**：
  - **優先建議**：使用 `uv run <script_name>.py` 來執行 Python 程式碼。這能確保自動使用正確的虛擬環境與相依性，無需手動啟用環境。
  - 若必須手動啟用環境，請引用 Windows 路徑：`.venv\Scripts\activate` (或 `.ps1`)。

## 3. 作業系統與終端機 (OS & Shell)
- **作業系統**：Windows 11。
- **終端機環境**：PowerShell。
- **指令語法注意事項**：
  - 提供的所有 Shell 指令必須是有效的 **PowerShell** 語法，而非 Bash 或 CMD。
  - **環境變數**：設定變數時使用 `$env:VARIABLE_NAME = 'Value'` (而非 `export` 或 `set`)。
  - **路徑分隔符**：在指令列範例中，路徑應使用反斜線 `\` (Backslash)。
  - **執行策略**：若涉及執行腳本遇到權限問題，應提示使用者檢查 `ExecutionPolicy`。
  - **指令串接**：
    - **優先使用分號 `;`** 串接指令，這是最安全的做法。
    - **避免使用 `&&`**：在 PowerShell 5.x 中 `&&` 完全不支援；在 PowerShell 7+ 中雖然支援，但與多行字串（如 git commit -m 包含換行的訊息）一起使用時會產生解析錯誤。
    - **最佳實踐**：將複雜指令分開執行，例如先執行 `git add -A`，再執行 `git commit -m "..."`，而非用 `&&` 串接。

## 4. 程式碼撰寫規範 (Coding Standards)
- **註釋語言**：程式碼中的所有註釋 (Comments)、Docstrings 必須使用 **繁體中文** 撰寫，解釋邏輯與功能。
- **路徑處理**：在 Python 程式碼中，強制使用 `pathlib` 模組來處理檔案路徑，以確保在 Windows 環境下的相容性與強健性 (Robustness)。
  - *Good*: `Path("data") / "file.txt"`
  - *Bad*: `"data/file.txt"` 或 `"data\\file.txt"`
- **錯誤處理**：針對 Windows 常見的編碼問題 (如 `cp950` vs `utf-8`)，在讀寫檔案時應明確指定 `encoding='utf-8'`。

## 5. 範例行為 (Example Behavior)

### 使用者提問：
"如何安裝 pandas 並執行 main.py？"

### AI 應回應：
1. **安裝指令 (PowerShell)**：
   ```powershell
   uv pip install pandas
   # 或者
   uv add pandas