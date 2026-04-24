# VS Code Complete Setup & Configuration Guide

> **Target OS:** Windows 10/11  
> **Last Updated:** April 2026  
> **Contents:** Clean uninstall, Roo Code setup, API keys, Python & Pylance, Rainbow CSV, Remote SSH

---

## Table of Contents

1. [Clean Uninstall of VS Code (Windows)](#1-clean-uninstall-of-vs-code-windows)
2. [Setting Up Roo Code on VS Code](#2-setting-up-roo-code-on-vs-code)
3. [Obtaining an API Key for Personal Use](#3-obtaining-an-api-key-for-personal-use)
4. [MCP Servers — Do You Need Them?](#4-mcp-servers--do-you-need-them)
5. [Python Language Servers — clangd Equivalent for Python](#5-python-language-servers--clangd-equivalent-for-python)
6. [Installing Python 3 & Pylance on VS Code (Windows)](#6-installing-python-3--pylance-on-vs-code-windows)
7. [Upgrading Python on Windows](#7-upgrading-python-on-windows)
8. [What is the clangd Extension?](#8-what-is-the-clangd-extension)
9. [Rainbow CSV Extension](#9-rainbow-csv-extension)
10. [Remote - SSH Extension](#10-remote---ssh-extension)
11. [Using GitHub in VS Code](#11-using-github-in-vs-code)

---

## 1. Clean Uninstall of VS Code (Windows)

### Step 1: Uninstall the Application

1. Open **Settings → Apps → Apps & features** (or **Add or Remove Programs**)
2. Search for **"Microsoft Visual Studio Code"**
3. Click **Uninstall**
4. During the uninstall wizard, **check the box** that says *"Delete user data"* if it appears

Alternatively, run the uninstaller directly from:

```
C:\Users\<YourUser>\AppData\Local\Programs\Microsoft VS Code\unins000.exe
```

Or if installed system-wide:

```
C:\Program Files\Microsoft VS Code\unins000.exe
```

### Step 2: Delete User Data & Configuration Folders

The uninstaller typically does **not** remove these. Delete them manually.

| Folder | Path | Purpose |
|---|---|---|
| **User Settings & State** | `%APPDATA%\Code\` | Settings, keybindings, snippets, workspaces, state |
| **Extensions** | `%USERPROFILE%\.vscode\` | All installed extensions |
| **Cache** | `%LOCALAPPDATA%\Code\` | VS Code cache (GPU cache, logs) |

Open **Command Prompt** and run:

```cmd
:: User settings, state, workspaces
rd /s /q "%APPDATA%\Code"

:: Installed extensions
rd /s /q "%USERPROFILE%\.vscode"

:: Local cache data
rd /s /q "%LOCALAPPDATA%\Code"
```

**Expanded paths for reference** (replace `<YourUser>` with your Windows username):

| Variable | Typical Expansion |
|---|---|
| `%APPDATA%\Code\` | `C:\Users\<YourUser>\AppData\Roaming\Code\` |
| `%USERPROFILE%\.vscode\` | `C:\Users\<YourUser>\.vscode\` |
| `%LOCALAPPDATA%\Code\` | `C:\Users\<YourUser>\AppData\Local\Code\` |

### Step 3: Clean Up Registry Entries (Optional)

Open **Registry Editor** (`regedit`) and delete these keys if they exist:

| Registry Path | Purpose |
|---|---|
| `HKEY_CURRENT_USER\Software\Microsoft\Visual Studio Code` | VS Code user-level registry data |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Visual Studio Code` | System-level install entry |
| `HKEY_CURRENT_USER\Software\Classes\vscode` | URI protocol handler (`vscode://`) |

> ⚠️ **Back up your registry** before making manual edits.

### Step 4: Remove from PATH

1. Open **System Properties → Advanced → Environment Variables**
2. Under **User variables** and **System variables**, find the `Path` entry
3. Remove any entry pointing to the VS Code `bin` folder, such as:
   - `C:\Users\<YourUser>\AppData\Local\Programs\Microsoft VS Code\bin`
   - `C:\Program Files\Microsoft VS Code\bin`

### Step 5: Optional Additional Cleanup

| Item | Location |
|---|---|
| Start Menu shortcut | `%APPDATA%\Microsoft\Windows\Start Menu\Programs\Visual Studio Code\` |
| Desktop shortcut | `%USERPROFILE%\Desktop\Visual Studio Code.lnk` |
| Temp files | `%TEMP%\` — search for any `vscode-*` folders |

---

## 2. Setting Up Roo Code on VS Code

### Step 1: Install the Roo Code Extension

1. Open **VS Code**
2. Go to the **Extensions** panel (`Ctrl+Shift+X`)
3. Search for **"Roo Code"** (publisher: RooVeterinaryInc)
4. Click **Install**

Or install from the command line:

```bash
code --install-extension RooVeterinaryInc.roo-cline
```

After installation, a **Roo Code icon** (🦘) appears in your VS Code sidebar.

### Step 2: Configure an AI Provider (API Key)

Click the Roo Code icon in the sidebar. You'll be prompted to configure an API provider.

Supported providers:

| Provider | What You Need |
|---|---|
| **Anthropic (Claude)** | API key from [console.anthropic.com](https://console.anthropic.com) |
| **OpenAI** | API key from [platform.openai.com](https://platform.openai.com) |
| **Google Gemini** | API key from [aistudio.google.com](https://aistudio.google.com) |
| **OpenRouter** | API key from [openrouter.ai](https://openrouter.ai) |
| **AWS Bedrock** | AWS credentials with Bedrock access |
| **Ollama** | Local Ollama server (no API key needed) |
| **LM Studio** | Local LM Studio server (no API key needed) |
| **Azure OpenAI** | Azure endpoint + API key |

**To configure:**

1. Click the **gear icon** (⚙️) in the Roo Code panel
2. Select your **API Provider** from the dropdown
3. Enter your **API key**
4. Select the **model** (e.g., `claude-sonnet-4-20250514`, `gpt-4o`)
5. Click **Save**

### Step 3: Understand the Modes

| Mode | Purpose |
|---|---|
| 🏗️ **Architect** | Planning, design, system architecture |
| 💻 **Code** | Writing, modifying, refactoring code |
| ❓ **Ask** | Explanations, documentation, Q&A |
| 🪲 **Debug** | Troubleshooting, investigating errors |
| 🪃 **Orchestrator** | Complex multi-step project coordination |

Switch between modes using the **mode selector** at the top of the Roo Code panel.

### Step 4: (Optional) Configure MCP Servers

MCP servers extend Roo Code's capabilities (e.g., Jira, Confluence, Bitbucket).

Edit the MCP settings file at:

```
%APPDATA%\Code\User\globalStorage\rooveterinaryinc.roo-cline\settings\mcp_settings.json
```

Example `mcp_settings.json`:

```json
{
  "mcpServers": {
    "my-server": {
      "command": "npx",
      "args": ["-y", "@some/mcp-server"],
      "env": {
        "API_KEY": "your-key-here"
      }
    }
  }
}
```

### Step 5: (Optional) Custom Modes & Rules

- **Custom Instructions:** Add project-specific instructions via `.roo/rules/` files in your workspace root
- **Custom Modes:** Create custom modes with specific tool permissions and file restrictions
- **`.roorules` or `.clinerules`:** Place in your project root for persistent context

### Step 6: Start Using Roo Code

1. Click the **Roo Code icon** in the sidebar
2. Type your request (e.g., *"Create a REST API with Express"*)
3. Review and approve each action — Roo Code asks before modifying files or running commands

---

## 3. Obtaining an API Key for Personal Use

### Option 1: Anthropic (Claude) — Recommended

1. Go to **[console.anthropic.com](https://console.anthropic.com)**
2. **Sign up** with your email or Google account
3. Add a **payment method** (credit/debit card) — pay-as-you-go
4. Navigate to **Settings → API Keys**
5. Click **"Create Key"**, name it, and copy the key
6. In Roo Code, select **"Anthropic"** as provider and paste your key

**Pricing (approximate):**

| Model | Input | Output |
|---|---|---|
| Claude Sonnet 4 | ~$3 / 1M tokens | ~$15 / 1M tokens |
| Claude Haiku | ~$0.80 / 1M tokens | ~$4 / 1M tokens |
| Claude Opus 4 | ~$15 / 1M tokens | ~$75 / 1M tokens |

### Option 2: OpenRouter (One Key, Many Models)

1. Go to **[openrouter.ai](https://openrouter.ai)**
2. **Sign up** (Google, GitHub, or email)
3. Add **credits** (minimum $5 usually)
4. Go to **[openrouter.ai/keys](https://openrouter.ai/keys)**
5. Click **"Create Key"** and copy it
6. In Roo Code, select **"OpenRouter"** as provider and paste your key

### Option 3: OpenAI (GPT-4o, o1, etc.)

1. Go to **[platform.openai.com](https://platform.openai.com)**
2. **Sign up** or log in (separate from ChatGPT subscription)
3. Add a **payment method** at **Settings → Billing**
4. Go to **API Keys** → **"Create new secret key"**
5. Copy the key (you can only see it once!)
6. In Roo Code, select **"OpenAI"** as provider and paste your key

> ⚠️ A ChatGPT Plus subscription does **NOT** give you API access. The API is billed separately.

### Option 4: Google Gemini (Free Tier Available)

1. Go to **[aistudio.google.com](https://aistudio.google.com)**
2. Sign in with your **Google account**
3. Click **"Get API Key"**
4. Create a key (free tier available with rate limits)
5. In Roo Code, select **"Google Gemini"** as provider and paste your key

### Option 5: Free / Local Models (No API Key Needed)

**Ollama (local, free):**

1. Install from **[ollama.com](https://ollama.com)**
2. Run a model:

```bash
ollama run llama3
```

3. In Roo Code, select **"Ollama"** as provider — no key needed
4. Connects to `http://localhost:11434` automatically

**LM Studio (local, free):**

1. Install from **[lmstudio.ai](https://lmstudio.ai)**
2. Download a model and start the local server
3. In Roo Code, select **"LM Studio"** as provider

> ⚠️ Local models require decent hardware (8GB+ VRAM recommended).

### Comparison for Personal Use

| Provider | Free Tier? | Best For | Setup Difficulty |
|---|---|---|---|
| **Anthropic (Claude)** | No (pay-as-you-go) | Best coding results | Easy |
| **OpenRouter** | No (prepaid credits) | Trying multiple models | Easy |
| **Google Gemini** | Yes (with limits) | Zero-cost experimentation | Easy |
| **OpenAI** | No (pay-as-you-go) | GPT-4o / o1 models | Easy |
| **Ollama** | Yes (runs locally) | Fully offline, privacy | Medium |

**Recommendation:** Start with **Anthropic Claude Sonnet** — best balance of coding quality and cost. Typical session costs $0.10–$1.00. Set usage limits in the Anthropic console to cap spending.

---

## 4. MCP Servers — Do You Need Them?

**Short answer: No.** MCP servers are optional. Roo Code works fully with just an API key.

### Built-in Capabilities (No MCP Needed)

- ✅ Read and write files in your workspace
- ✅ Run terminal commands
- ✅ Search across your codebase
- ✅ Browse and understand your project structure
- ✅ Code, debug, plan, and answer questions

### When You Might Want MCP Servers

| MCP Server | Use Case |
|---|---|
| **Jira** | Read/create tickets |
| **Confluence** | Read/create wiki pages |
| **Bitbucket / GitHub** | Review PRs, manage repos |
| **Database** (Postgres, SQLite) | Run SQL queries directly |
| **Brave Search / Web** | Search the internet |
| **Filesystem (extended)** | Access files outside workspace |
| **Memory** | Persistent knowledge graph across sessions |

**Bottom line:** For personal coding on your laptop, **API key + Roo Code extension = everything you need**. Add MCP servers later if required.

---

## 5. Python Language Servers — clangd Equivalent for Python

### Pylance (Recommended)

| Detail | Info |
|---|---|
| **Extension name** | Pylance |
| **Publisher** | Microsoft |
| **Extension ID** | `ms-python.vscode-pylance` |
| **Based on** | Pyright (Microsoft's static type checker) |
| **License** | Proprietary (free to use in VS Code) |

**Features:**

| Feature | Description |
|---|---|
| IntelliSense | Rich auto-completions with type info |
| Type checking | Static type analysis (Pyright engine) |
| Go to definition | `F12` / `Ctrl+Click` |
| Find references | `Shift+F12` |
| Hover info | Type signatures, docstrings |
| Auto-imports | Suggests and adds import statements |
| Rename symbol | `F2` — renames across files |
| Inlay hints | Shows inferred types, parameter names |
| Semantic highlighting | Context-aware syntax coloring |

### Pyright (Open Source Alternative)

| Detail | Info |
|---|---|
| **Extension ID** | `ms-pyright.pyright` |
| **License** | MIT (fully open source) |

> ⚠️ Don't install both Pylance and Pyright — they conflict. Choose one.

### Pylance vs Pyright

| Feature | Pylance | Pyright |
|---|---|---|
| Core type checking | ✅ | ✅ (same engine) |
| Auto-imports | ✅ Enhanced | ✅ Basic |
| IntelliSense quality | ✅ Better | ✅ Good |
| Semantic highlighting | ✅ | ❌ |
| License | Proprietary (free) | MIT (open source) |
| Works outside VS Code | ❌ VS Code only | ✅ Any editor |

### Full Python Setup in VS Code

| Extension | Purpose | Equivalent to (C/C++) |
|---|---|---|
| **Python** (`ms-python.python`) | Core Python support, debugging | Runtime integration |
| **Pylance** (`ms-python.vscode-pylance`) | Language server (IntelliSense) | clangd |
| **Python Debugger** (`ms-python.debugpy`) | Debugging | GDB/LLDB |
| **Ruff** (`charliermarsh.ruff`) | Linting + formatting | clang-tidy + clang-format |

---

## 6. Installing Python 3 & Pylance on VS Code (Windows)

### Part 1 — Install Python 3

#### Step 1: Download Python

1. Go to **[python.org/downloads](https://www.python.org/downloads/)**
2. Click the yellow **"Download Python 3.x.x"** button
3. Save the installer (e.g., `python-3.13.x-amd64.exe`)

#### Step 2: Run the Installer

1. **Double-click** the downloaded `.exe` file
2. ⚠️ **CRITICAL:** Check the box at the bottom:
   > ☑️ **"Add python.exe to PATH"**
3. Click **"Install Now"**
4. Wait for installation to complete
5. Click **"Close"**

#### Step 3: Verify Python Installation

Open **Command Prompt** (`Win+R` → type `cmd` → Enter):

```cmd
python --version
```

Expected output: `Python 3.13.x`

```cmd
pip --version
```

Expected output: `pip 24.x.x from ...`

> If `python` is not recognized, you missed the "Add to PATH" checkbox. Re-run the installer → click **"Modify"** → ensure "Add to PATH" is checked.

### Part 2 — Install VS Code (Skip if Already Installed)

#### Step 4: Download VS Code

1. Go to **[code.visualstudio.com](https://code.visualstudio.com)**
2. Click **"Download for Windows"**
3. Run the installer

#### Step 5: Run the VS Code Installer

1. Accept the license agreement
2. Choose the install location (default is fine)
3. Check these options:
   - ☑️ Add "Open with Code" to file context menu
   - ☑️ Add "Open with Code" to directory context menu
   - ☑️ Add to PATH
4. Click **Install** → **Finish**

### Part 3 — Install the Python Extension (Includes Pylance)

#### Step 6: Open VS Code

Launch **Visual Studio Code**

#### Step 7: Open the Extensions Panel

Press `Ctrl+Shift+X`

#### Step 8: Install the Python Extension

1. Search for **`Python`**
2. Find the one by **Microsoft** (top result, millions of downloads)
   - Extension ID: `ms-python.python`
3. Click **"Install"**

> ✅ **Pylance is automatically installed** as a dependency. You do NOT need to install it separately.

#### Step 9: Verify Pylance is Installed

1. In Extensions, search for **"Pylance"**
2. It should be listed as **installed** (by Microsoft)

### Part 4 — Configure Python in VS Code

#### Step 10: Select the Python Interpreter

1. Press `Ctrl+Shift+P`
2. Type: **`Python: Select Interpreter`**
3. Select your installed Python version, e.g.:
   ```
   Python 3.13.x  ~\AppData\Local\Programs\Python\Python313\python.exe
   ```
4. The selected version appears in the **bottom-right** status bar

#### Step 11: Configure Type Checking (Optional)

1. Press `Ctrl+,` to open Settings
2. Search for: **`python.analysis.typeCheckingMode`**
3. Set to:
   - **`off`** — No type checking (default)
   - **`basic`** — Catches common errors (recommended)
   - **`strict`** — Full type enforcement

### Part 5 — Test Your Setup

#### Step 12: Create a Test File

1. Press `Ctrl+N` → `Ctrl+S` → save as **`hello.py`**
2. Type:

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"

message = greet("World")
print(message)
```

#### Step 13: Verify Features

- ✅ **Auto-completion** — suggestions appear as you type
- ✅ **Type hints** — hover over `message` to see inferred type `str`
- ✅ **Error detection** — try `greet(123)` to see a type error
- ✅ **Go to definition** — `Ctrl+Click` on `greet`

#### Step 14: Run the File

- Click the **▶ Play button** (top-right), or press `Ctrl+F5`
- Expected output:

```
Hello, World!
```

### Installed Components Summary

| Component | What It Is | How Installed |
|---|---|---|
| **Python 3.13** | Python runtime & interpreter | Manually from python.org |
| **pip** | Python package manager | Bundled with Python |
| **Python Extension** | Core VS Code Python support | VS Code Marketplace |
| **Pylance** | Language server (IntelliSense) | Auto-installed with Python ext |
| **Python Debugger** | Debugging support | Auto-installed with Python ext |

### Recommended Additional Extensions

| Extension | Purpose |
|---|---|
| **Ruff** | Ultra-fast linter + formatter |
| **autoDocstring** | Auto-generate Python docstrings |
| **Python Indent** | Fix Python indentation issues |

---

## 7. Upgrading Python on Windows

> **When to use this:** Follow this guide whenever a new stable Python version is released and you want to upgrade your system-wide Python installation.

### Python Release Cycle

Python releases a new minor version every October. Use this table to track support status:

| Version | Release | Status |
|---|---|---|
| `3.12` | October 2023 | Security fixes only |
| `3.13` | October 2024 | Bug fixes & security |
| `3.14` | October 2025 | Active (current stable) |
| `3.15` | ~October 2026 | In development |

Always install from the official site: **https://www.python.org/downloads/**

---

### Step 1 — Check Your Current Python Version

Open **Command Prompt** and run:

```cmd
python --version
pip --version
where python
```

Note the install path (e.g. `C:\Users\<YourUser>\AppData\Local\Programs\Python\Python313\`).

---

### Step 2 — Download the New Python Installer

1. Go to **https://www.python.org/downloads/**
2. Click the latest stable release (e.g. `Python 3.14.x`)
3. Scroll to **Files** and download: **Windows installer (64-bit)** → `python-3.14.x-amd64.exe`

---

### Step 3 — Run the Installer

1. **Right-click** the `.exe` → **Run as administrator**
2. On the first screen:
   - ✅ Check **"Add python.exe to PATH"**
   - ✅ Check **"Install launcher for all users"**
3. Click **"Customize installation"**
4. **Optional Features** — ensure all are checked (especially `pip`)
5. **Advanced Options**:
   - ✅ Check **"Install for all users"** (system-wide)
   - ✅ Check **"Add Python to environment variables"**
   - Install path: `C:\Program Files\Python314\`
6. Click **Install**

> **Note:** If the installer shows a **"Modify Setup"** screen instead of the normal install wizard, it means that version is already installed. Click **Cancel** — nothing needs to be done.

---

### Step 4 — Fix the PATH (if needed)

After install, open a **new** Command Prompt and run `python --version`. If it still shows the old version:

**Option A — GUI:**
1. Press `Win + R` → type `sysdm.cpl` → Enter
2. **Advanced** tab → **Environment Variables...**
3. Under **User variables**, select `Path` → **Edit**
4. Add these two entries (replace `314` with your new version number):
   - `C:\Users\<YourUser>\AppData\Local\Programs\Python\Python314\`
   - `C:\Users\<YourUser>\AppData\Local\Programs\Python\Python314\Scripts\`
5. Use **Move Up** to place both entries **above** the old Python entries
6. Click **OK** → **OK** → **OK**

**Option B — PowerShell (run as Administrator):**

```powershell
$oldPath = [Environment]::GetEnvironmentVariable("Path", "User")
$py314 = "C:\Users\$env:USERNAME\AppData\Local\Programs\Python\Python314"
$py314scripts = "$py314\Scripts"
$newPath = "$py314;$py314scripts;" + ($oldPath -replace [regex]::Escape($py314 + ";"), "" -replace [regex]::Escape($py314scripts + ";"), "")
[Environment]::SetEnvironmentVariable("Path", $newPath, "User")
Write-Host "Done! Open a new terminal and run: python --version"
```

---

### Step 5 — Verify Python is Updated

Open a **new** Command Prompt:

```cmd
python --version
where python
py --version
```

Expected output:
```
Python 3.14.x
C:\Users\<YourUser>\AppData\Local\Programs\Python\Python314\python.exe
Python 3.14.x
```

---

### Step 6 — Update pip

```cmd
python -m pip install --upgrade pip
pip --version
```

---

### Step 7 — (Optional) Reinstall Global Packages

Export your old packages **before** uninstalling the old Python:

```cmd
pip freeze > old_packages.txt
```

Then reinstall in the new Python:

```cmd
pip install -r old_packages.txt
```

> ⚠️ Some packages may not yet support the latest Python — check for errors and skip incompatible ones.

---

### Step 8 — (Optional) Recreate Project Virtual Environments

Virtual environments are tied to a specific Python version. For each project:

```cmd
cd C:\path\to\your\project

:: Delete old venv
rmdir /s /q .venv

:: Recreate with new Python
python -m venv .venv

:: Activate
.venv\Scripts\activate

:: Reinstall dependencies
pip install -r requirements.txt
```

---

### Step 9 — (Optional) Uninstall Old Python

1. Open **Settings → Apps → Installed Apps**
2. Search for the old Python version (e.g. `Python 3.13`)
3. Click the **three dots (⋯)** → **Uninstall**
4. Confirm, then verify the new Python still works:

```cmd
python --version
where python
```

---

## 8. What is the clangd Extension?

### Overview

**clangd** is a language server based on the LLVM/Clang compiler. It provides IDE features for C-family languages with compiler-grade accuracy.

### Supported Languages

| Language | File Extensions |
|---|---|
| **C** | `.c`, `.h` |
| **C++** | `.cpp`, `.cxx`, `.cc`, `.hpp`, `.hxx`, `.hh`, `.h` |
| **Objective-C** | `.m`, `.h` |
| **Objective-C++** | `.mm` |
| **CUDA** | `.cu`, `.cuh` |

> clangd does **not** support other languages. It is strictly for C-family languages.

### Installation

1. `Ctrl+Shift+X` → search **"clangd"**
2. Install the extension by **LLVM** (`llvm-vs-code-extensions.vscode-clangd`)

Or from command line:

```bash
code --install-extension llvm-vs-code-extensions.vscode-clangd
```

### Install the clangd Binary

**Let the extension install it:** When you first open a C/C++ file, it prompts to install automatically.

**Or install manually on Windows:**

```cmd
winget install LLVM.LLVM
```

Or:

```cmd
choco install llvm
```

### Disable Microsoft C/C++ IntelliSense (Important!)

If you have the Microsoft C/C++ extension installed, disable its IntelliSense to avoid conflicts:

Add to `settings.json`:

```json
{
    "C_Cpp.intelliSenseEngine": "disabled"
}
```

> Keep the Microsoft C/C++ extension for its **debugger** — just disable IntelliSense.

### Set Up a Compilation Database

clangd needs a `compile_commands.json` for accurate results:

| Build System | Command |
|---|---|
| **CMake** | `cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ..` |
| **Make (with Bear)** | `bear -- make` |
| **Meson** | Generated by default |

For simple projects, create a `.clangd` file in your project root:

```yaml
CompileFlags:
  Add:
    - -std=c++17
    - -Wall
    - -I/path/to/includes
```

### Features

| Feature | Description |
|---|---|
| Code completion | Fast, context-aware, with signature help |
| Diagnostics | Real-time errors and warnings (compiler-accurate) |
| Go to definition | `F12` or `Ctrl+Click` |
| Find references | `Shift+F12` |
| Hover info | Type info and docs |
| Code actions | Quick fixes, add includes, extract function |
| Rename symbol | `F2` — renames across files |
| Format code | Uses clang-format (reads `.clang-format` file) |
| Include insertion | Suggests missing `#include` headers |
| Inlay hints | Parameter names, deduced types |

### clangd vs Microsoft C/C++ Extension

| Feature | clangd | Microsoft C/C++ |
|---|---|---|
| Speed | Very fast | Slower on large projects |
| Accuracy | Compiler-grade | Good but less precise |
| Debugging | ❌ No debugger | ✅ Built-in debugger |
| Setup | Needs compile_commands.json | Often works without config |

---

## 9. Rainbow CSV Extension

### What It Does

**Rainbow CSV** color-codes each column in CSV/TSV files with a different color, making it easy to visually track columns.

### Supported File Types

| File Type | Delimiter | Auto-detected? |
|---|---|---|
| `.csv` | Comma (`,`) | ✅ Yes |
| `.tsv` | Tab (`\t`) | ✅ Yes |
| `.csv` (semicolons) | Semicolon (`;`) | ✅ Yes |
| `.psv` | Pipe (`\|`) | ✅ Yes |
| Custom | Any character | Manual selection |

### Step-by-Step Installation

#### Step 1: Open VS Code

Launch **Visual Studio Code**

#### Step 2: Open the Extensions Panel

Press **`Ctrl+Shift+X`**

#### Step 3: Search for Rainbow CSV

Type **`Rainbow CSV`** in the search bar

#### Step 4: Install

1. Find the one by **mechatroner** (`mechatroner.rainbow-csv`)
2. Click **"Install"**
3. No restart needed — activates immediately

#### Step 5: Test It

1. Press `Ctrl+N` → `Ctrl+S` → save as **`test.csv`**
2. Paste:

```csv
Name,Age,City,Department,Salary
Alice,30,New York,Engineering,85000
Bob,25,London,Marketing,62000
Charlie,35,Tokyo,Engineering,92000
Diana,28,Paris,Design,71000
```

3. Each column should appear in a **different color**

### Key Features

| Feature | Description |
|---|---|
| Column highlighting | Each column gets a unique color |
| Hover info | Shows column name and number |
| Column alignment | Aligns columns into table-like view |
| RBQL queries | SQL-like queries on CSV data |
| Lint/Validate | Detects inconsistent column counts |
| Convert formats | Convert between CSV, TSV, etc. |

### RBQL — Built-in Query Language

Press `Ctrl+Shift+P` → **"Rainbow CSV: RBQL"**:

```sql
-- Select specific columns (a1 = first column, a2 = second, etc.)
SELECT a1, a3 WHERE a2 > 30

-- Filter rows
SELECT * WHERE a4 == "Engineering"

-- Sort
SELECT * ORDER BY int(a5) DESC

-- Aggregate
SELECT a4, COUNT(*), AVG(int(a5)) GROUP BY a4
```

### Useful Commands

Access via `Ctrl+Shift+P`:

| Command | What It Does |
|---|---|
| Rainbow CSV: Align | Pads columns to line up vertically |
| Rainbow CSV: Shrink | Removes alignment padding |
| Rainbow CSV: RBQL | Opens the query interface |
| Rainbow CSV: Set Delimiter | Change the column delimiter |
| Rainbow CSV: CSVLint | Validates column count consistency |

---

## 10. Remote - SSH Extension

### What It Does

Opens folders and files on a **remote machine** (Linux server, cloud VM, etc.) directly in VS Code — as if they were local. Editing, terminal, debugging, and extensions all run remotely.

### Part 1 — Prerequisites

#### Step 1: Ensure SSH Client Is Installed

Windows 10/11 includes OpenSSH. Verify in **Command Prompt** or **PowerShell**:

```cmd
ssh -V
```

Expected: `OpenSSH_for_Windows_9.x.x`

> If not found:
> 1. **Settings → Apps → Optional Features**
> 2. Click **"Add a feature"**
> 3. Search **"OpenSSH Client"** → **Install**

#### Step 2: Test SSH Connection

```cmd
ssh username@remote-host-ip-or-hostname
```

Example:

```cmd
ssh john@192.168.1.100
```

If you get a terminal on the remote machine, you're ready. Type `exit` to disconnect.

### Part 2 — Install the Extension

#### Step 3: Open VS Code

Launch **Visual Studio Code**

#### Step 4: Open Extensions Panel

Press **`Ctrl+Shift+X`**

#### Step 5: Search and Install

1. Search for **`Remote - SSH`**
2. Find the one by **Microsoft** (`ms-vscode-remote.remote-ssh`)
3. Click **"Install"**

### Part 3 — Connect to a Remote Machine

#### Step 6: Open Remote Connection Dialog

Three ways:
- Click the **blue/green `><` icon** in the **bottom-left corner**
- Press `Ctrl+Shift+P` → type **`Remote-SSH: Connect to Host`**
- Press `F1` → type **`Remote-SSH: Connect to Host`**

#### Step 7: Enter SSH Connection Details

1. Click **"Connect to Host..."**
2. Click **"+ Add New SSH Host..."**
3. Enter the SSH command:

```
ssh username@remote-host-ip
```

Example:

```
ssh john@192.168.1.100
```

With a custom port:

```
ssh john@192.168.1.100 -p 2222
```

4. Select the SSH config file to update:

```
C:\Users\<YourUser>\.ssh\config
```

5. Click **"Connect"**

#### Step 8: Select the Remote OS

Choose: **Linux**, **Windows**, or **macOS**

#### Step 9: Enter Your Password

Type your password when prompted. You may be prompted multiple times during first connection.

#### Step 10: Wait for VS Code Server Setup

First connection installs a lightweight VS Code Server on the remote machine (~30-60 seconds). Subsequent connections are faster.

#### Step 11: Open a Remote Folder

1. Click **"Open Folder"** in the Explorer panel
2. Browse or type the remote path (e.g., `/home/john/projects/`)
3. Click **OK**

#### Step 12: Verify Connection

- **Bottom-left corner** shows: `SSH: remote-host-ip`
- **Explorer** shows files from the remote machine
- **Terminal** (`Ctrl+`` `) opens a shell on the remote machine

### Part 4 — Reconnecting

#### Step 13: Quick Reconnect

1. Click the **`><` icon** (bottom-left)
2. Click **"Connect to Host..."**
3. Select a saved host from the list

### Part 5 — SSH Key Authentication (No More Passwords)

#### Step 14: Generate an SSH Key Pair

Open **PowerShell** or **Command Prompt**:

```cmd
ssh-keygen -t ed25519 -C "your-email@example.com"
```

1. Press **Enter** for default location (`C:\Users\<YourUser>\.ssh\id_ed25519`)
2. Enter a passphrase (optional)
3. Two files created:
   - `C:\Users\<YourUser>\.ssh\id_ed25519` (private key — **never share**)
   - `C:\Users\<YourUser>\.ssh\id_ed25519.pub` (public key — safe to share)

#### Step 15: Copy Public Key to Remote Machine

**Method A — Using ssh-copy-id (Git Bash):**

```bash
ssh-copy-id username@remote-host-ip
```

**Method B — PowerShell:**

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh username@remote-host-ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

**Method C — Fully manual:**

1. Open `C:\Users\<YourUser>\.ssh\id_ed25519.pub` in Notepad
2. Copy the entire contents
3. SSH into the remote machine and run:

```bash
mkdir -p ~/.ssh
echo "PASTE_YOUR_PUBLIC_KEY_HERE" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

#### Step 16: Test Passwordless Connection

```cmd
ssh username@remote-host-ip
```

Should connect without a password prompt.

### Part 6 — SSH Config File (Multiple Hosts)

#### Step 17: Edit SSH Config

Location: `C:\Users\<YourUser>\.ssh\config`

Open via: `Ctrl+Shift+P` → **"Remote-SSH: Open SSH Configuration File"**

Example config:

```
Host my-server
    HostName 192.168.1.100
    User john
    Port 22
    IdentityFile ~/.ssh/id_ed25519

Host work-vm
    HostName 10.0.0.50
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

Host cloud-gpu
    HostName gpu-instance.cloud-provider.com
    User ubuntu
    IdentityFile ~/.ssh/id_ed25519
```

These names (`my-server`, `work-vm`, `cloud-gpu`) appear in VS Code's "Connect to Host" list.

### What Works Remotely

| Feature | Local or Remote? |
|---|---|
| File editing | Remote |
| Terminal | Remote |
| Debugging | Remote |
| Git | Remote |
| Extensions (most) | Remote |
| VS Code UI/theme | Local |
| Keyboard shortcuts | Local |

### Troubleshooting

| Issue | Solution |
|---|---|
| "Permission denied" | Check username/password or SSH key |
| Connection timeout | Verify remote IP is reachable (`ping remote-host-ip`) |
| "Could not establish connection" | Check SSH service on remote (`sudo systemctl status sshd`) |
| Password prompt repeats | Set up SSH key auth (Part 5) |
| Extensions not working remotely | Install extensions on the remote side |

---

## 11. Using GitHub in VS Code

### Prerequisites

1. **Install Git:** Download from [git-scm.com](https://git-scm.com)
2. **Create a GitHub account:** [github.com](https://github.com)
3. **Configure Git:**

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

### Built-in Git (No Extension Needed)

VS Code has Git built in. Access via the **Source Control** icon (`Ctrl+Shift+G`).

#### Clone a Repository

1. Press `Ctrl+Shift+P` → type **"Git: Clone"**
2. Paste the GitHub repo URL (e.g., `https://github.com/username/repo.git`)
3. Choose a local folder

#### Daily Workflow

| Action | How |
|---|---|
| See changes | Source Control icon — modified files appear |
| Stage files | Click `+` next to a file |
| Commit | Type message → `Ctrl+Enter` |
| Push | `...` menu → Push, or sync icon (🔄) in status bar |
| Pull | `...` → Pull, or sync icon |
| Create branch | Click branch name (bottom-left) → "Create new branch" |
| Switch branches | Click branch name (bottom-left) → select branch |
| View diff | Click changed file in Source Control |

### GitHub Extension (Enhanced Integration)

#### Install

1. `Ctrl+Shift+X` → search **"GitHub Pull Requests"**
2. Install by **GitHub**
3. Sign in to GitHub when prompted

#### Features

| Feature | How to Use |
|---|---|
| Create Pull Requests | Source Control → `...` → "Create Pull Request" |
| Review Pull Requests | PR icon in sidebar |
| View Issues | GitHub icon in sidebar |
| Checkout PR branches | Click a PR → "Checkout" |
| In-editor PR comments | See review comments inline |

### Authentication Options

**Option A — Browser Sign-In (Easiest):**
VS Code prompts "Sign in to GitHub" → click → authorize in browser

**Option B — Personal Access Token (PAT):**

1. GitHub → **Settings → Developer settings → Personal access tokens → Tokens (classic)**
2. Generate token with `repo` scope
3. Use as password when VS Code asks for credentials

**Option C — SSH Key:**

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Add the public key to **GitHub → Settings → SSH and GPG keys**. Clone using SSH URLs: `git@github.com:username/repo.git`

### Starting a New Project

1. Create project folder → open in VS Code
2. `Ctrl+Shift+P` → **Git: Initialize Repository**
3. Write code → Stage → Commit
4. Create a new empty repo on github.com
5. `Ctrl+Shift+P` → **Git: Add Remote** → paste GitHub URL
6. Click **Publish Branch** or Push

### Recommended Git Extensions

| Extension | Purpose |
|---|---|
| **GitHub Pull Requests and Issues** | PR reviews, issues, inline comments |
| **GitLens** | Git blame, history, line-by-line authorship |
| **Git Graph** | Visual branch/commit graph |

### Useful Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+Shift+G` | Open Source Control panel |
| `Ctrl+Shift+P` → "Git:" | Access all Git commands |
| Click branch name (bottom-left) | Switch/create branches |

---

## Quick Reference — All Extensions Mentioned

| Extension | ID | Purpose |
|---|---|---|
| **Roo Code** | `RooVeterinaryInc.roo-cline` | AI coding assistant |
| **Python** | `ms-python.python` | Python language support |
| **Pylance** | `ms-python.vscode-pylance` | Python IntelliSense |
| **clangd** | `llvm-vs-code-extensions.vscode-clangd` | C/C++ language server |
| **Rainbow CSV** | `mechatroner.rainbow-csv` | CSV/TSV column highlighting |
| **Remote - SSH** | `ms-vscode-remote.remote-ssh` | Remote development via SSH |
| **GitHub PRs & Issues** | `GitHub.vscode-pull-request-github` | GitHub integration |
| **GitLens** | `eamodio.gitlens` | Git blame & history |
| **Git Graph** | `mhutchie.git-graph` | Visual git graph |
| **Ruff** | `charliermarsh.ruff` | Python linter + formatter |

### Batch Install Command

```bash
code --install-extension RooVeterinaryInc.roo-cline
code --install-extension ms-python.python
code --install-extension llvm-vs-code-extensions.vscode-clangd
code --install-extension mechatroner.rainbow-csv
code --install-extension ms-vscode-remote.remote-ssh
code --install-extension GitHub.vscode-pull-request-github
code --install-extension eamodio.gitlens
code --install-extension mhutchie.git-graph
code --install-extension charliermarsh.ruff
```
