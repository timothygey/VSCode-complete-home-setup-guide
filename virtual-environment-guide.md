# Virtual Environment Guide

> A comprehensive guide to setting up, using, and troubleshooting virtual environments across multiple languages and platforms.
> Author: timothygey | Created: 24 April 2026

---

## Table of Contents

- [⚡ Quick Start — First-Time Setup](#-quick-start--first-time-setup)
- [What is a Virtual Environment & Why Use One](#what-is-a-virtual-environment--why-use-one)
- [Part 1 — Python Virtual Environments](#part-1--python-virtual-environments)
- [Part 2 — Node.js Environment Isolation](#part-2--nodejs-environment-isolation)
- [Part 3 — Other Languages & Platforms](#part-3--other-languages--platforms)
- [Part 4 — Daily Workflow: Resuming Work in VS Code](#part-4--daily-workflow-resuming-work-in-vs-code)
- [Part 5 — Troubleshooting: Missing Dependencies & Broken Environments](#part-5--troubleshooting-missing-dependencies--broken-environments)
- [Part 6 — What NEVER to Do in a Virtual Environment](#part-6--what-never-to-do-in-a-virtual-environment)
- [Part 7 — Q&A: Installing, Upgrading & Managing Packages](#part-7--qa-installing-upgrading--managing-packages)

---

## ⚡ Quick Start — First-Time Setup

> **Skip the theory — just get running.** Copy and paste these commands to create your first virtual environment and start working. For detailed explanations, see the sections below.

### Python Project — Quick Setup (< 2 Minutes)

**1. Open your project folder in VS Code**, then open the terminal (`` Ctrl + ` ``).

**2. Create the virtual environment:**
```bash
python -m venv venv
```

**3. Activate it:**

| Platform | Command |
|---|---|
| Windows CMD | `venv\Scripts\activate` |
| Windows PowerShell | `.\venv\Scripts\Activate.ps1` |
| Git Bash / Linux / macOS | `source venv/bin/activate` |

You should see `(venv)` appear in your terminal prompt.

**4. Upgrade pip:**
```bash
python -m pip install --upgrade pip
```

**5. Install your packages:**
```bash
pip install flask requests numpy    # replace with your packages
```

**6. Save your dependencies:**
```bash
pip freeze > requirements.txt
```

**7. Add `venv/` to `.gitignore`:**

Create a `.gitignore` file (or add to your existing one):
```gitignore
venv/
__pycache__/
.env
```

**8. Set VS Code interpreter (one-time):**
- Press `Ctrl + Shift + P` → type **"Python: Select Interpreter"**
- Select: `Python 3.x.x ('venv': venv) .\venv\Scripts\python.exe`

**✅ Done!** You're now working in an isolated environment.

---

### Node.js Project — Quick Setup (< 2 Minutes)

**1. Open your project folder in VS Code**, then open the terminal (`` Ctrl + ` ``).

**2. Initialize the project:**
```bash
npm init -y
```

**3. Install packages:**
```bash
npm install express       # replace with your packages
```

**4. Add `node_modules/` to `.gitignore`:**
```gitignore
node_modules/
.env
```

**5. (Optional) Pin your Node version with nvm:**
```bash
node --version > .nvmrc
```

**✅ Done!** Node.js isolates packages per-project automatically via `node_modules/`.

---

### Quick Start Cheat Sheet

```
FIRST TIME SETUP                          EVERY TIME YOU RETURN
──────────────────                        ────────────────────
python -m venv venv                       venv\Scripts\activate  (Win CMD)
venv\Scripts\activate  (Win CMD)          .\venv\Scripts\Activate.ps1  (Win PS)
source venv/bin/activate  (Linux/Mac)     source venv/bin/activate  (Linux/Mac)
python -m pip install --upgrade pip       python --version  (verify)
pip install your-packages                 pip install -r requirements.txt  (if new deps)
pip freeze > requirements.txt             (start coding!)
(add venv/ to .gitignore)
(select interpreter in VS Code)
```

> For the full explanation of what these commands do and why, continue reading below.

---

## What is a Virtual Environment & Why Use One

### The Problem

Without virtual environments, every project on your machine shares the **same global packages and language version**. This causes:

- **Dependency conflicts** — Project A needs `library v1.0`, Project B needs `library v2.0`. Both can't coexist globally.
- **System pollution** — Installing packages globally clutters your system and can break OS-level tools.
- **"Works on my machine" syndrome** — Without a clear record of dependencies, others can't reproduce your environment.
- **Version lock-in** — You're stuck with one Python/Node version system-wide.

### The Solution

A virtual environment is an **isolated sandbox** for each project. Each sandbox has its own:
- Package installations (independent from other projects)
- Language runtime version (optionally)
- Configuration

### Analogy

Think of it like **separate toolboxes** for separate jobs:

```
Without Virtual Environments:
┌─────────────────────────────────────────┐
│            ONE BIG TOOLBOX              │
│  Project A's tools + Project B's tools  │
│  + System tools = CHAOS & CONFLICTS     │
└─────────────────────────────────────────┘

With Virtual Environments:
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│  Project A   │  │  Project B   │  │   System     │
│  Toolbox     │  │  Toolbox     │  │   Toolbox    │
│  Python 3.11 │  │  Python 3.13 │  │  (untouched) │
│  Django 4.2  │  │  Flask 3.0   │  │              │
└──────────────┘  └──────────────┘  └──────────────┘
```

### When You Need a Virtual Environment

| Situation | Need Virtual Env? |
|---|---|
| Working on any Python project | ✅ Always |
| Working on a Node.js project | ✅ Yes (`node_modules` provides this automatically) |
| Multiple projects with different dependency versions | ✅ Absolutely |
| Learning/experimenting with a new library | ✅ Yes — keep experiments isolated |
| Writing a one-off script | ⚠️ Optional but recommended |
| System administration tasks | ❌ Use system packages |

---

## Part 1 — Python Virtual Environments

Python has multiple tools for virtual environments. Here's a full comparison and setup guide for each.

### Comparison Table

| Tool | Built-in? | Best For | Manages Python Version? | Lock File? |
|---|---|---|---|---|
| **`venv`** | ✅ Yes (Python 3.3+) | Simple projects, beginners | ❌ No | ❌ (`requirements.txt` manually) |
| **`conda`** | ❌ Install separately | Data science, multi-language | ✅ Yes | ✅ `environment.yml` |
| **`pipenv`** | ❌ Install separately | Web dev, automatic lock files | ❌ No | ✅ `Pipfile.lock` |
| **`poetry`** | ❌ Install separately | Modern Python projects, publishing | ❌ No | ✅ `poetry.lock` |

---

### Option A — `venv` (Recommended for Beginners)

`venv` is built into Python — no installation needed. It's the simplest and most widely used option.

#### Step 1 — Create a Virtual Environment

Open your terminal in your project folder:

```bash
# Navigate to your project
cd C:\path\to\your\project

# Create a virtual environment named "venv"
python -m venv venv
```

This creates a `venv/` folder inside your project containing:
```
venv/
├── Include/          # C header files (for compiling extensions)
├── Lib/              # Installed packages go here
│   └── site-packages/
├── Scripts/          # (Windows) Activation scripts & python.exe
│   ├── activate
│   ├── activate.bat
│   ├── python.exe
│   └── pip.exe
└── pyvenv.cfg        # Configuration file
```

> **Naming convention:** The folder is commonly named `venv`, `.venv`, or `env`. Use `venv` for simplicity.

#### Step 2 — Activate the Virtual Environment

You **must activate** the environment before installing packages or running your code.

**Windows Command Prompt:**
```cmd
venv\Scripts\activate
```

**Windows PowerShell:**
```powershell
.\venv\Scripts\Activate.ps1
```

**Git Bash / Linux / macOS:**
```bash
source venv/bin/activate
```

**When activated**, your terminal prompt changes:
```
(venv) C:\Users\timothygey\my-project>
```

The `(venv)` prefix confirms you're inside the virtual environment.

#### Step 3 — Install Packages

With the environment activated, install packages using `pip`:

```bash
pip install requests flask numpy
```

These packages are installed **only inside `venv/`** — not globally.

#### Step 4 — Save Your Dependencies

Generate a `requirements.txt` file that records all installed packages:

```bash
pip freeze > requirements.txt
```

The file looks like:
```
Flask==3.0.0
numpy==1.26.2
requests==2.31.0
```

> **Important:** Commit `requirements.txt` to Git. Do NOT commit `venv/`.

#### Step 5 — Deactivate When Done

```bash
deactivate
```

The `(venv)` prefix disappears from your prompt.

#### Step 6 — Recreate the Environment (On Another Machine or After Deletion)

```bash
python -m venv venv
venv\Scripts\activate          # Windows CMD
pip install -r requirements.txt
```

This installs the exact same packages that were recorded.

---

### Option B — `conda` / Miniconda

Best for data science projects or when you need to manage multiple Python versions.

#### Install Miniconda

Download from: [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)

#### Step-by-Step

```bash
# Create a new environment with a specific Python version
conda create --name myproject python=3.12

# Activate it
conda activate myproject

# Install packages
conda install numpy pandas matplotlib
# or use pip for packages not in conda:
pip install some-package

# Save environment
conda env export > environment.yml

# Deactivate
conda deactivate

# Recreate from file
conda env create -f environment.yml

# List all environments
conda env list

# Delete an environment
conda env remove --name myproject
```

#### `environment.yml` Example

```yaml
name: myproject
channels:
  - defaults
dependencies:
  - python=3.12
  - numpy=1.26.2
  - pandas=2.1.4
  - pip:
    - some-pip-only-package==1.0.0
```

---

### Option C — `pipenv`

Combines `pip` + `venv` with automatic lock files.

```bash
# Install pipenv
pip install pipenv

# Create environment and install a package
cd my-project
pipenv install requests

# Activate the shell
pipenv shell

# Install dev dependencies
pipenv install --dev pytest

# Generate lock file (automatic)
# Pipfile.lock is created/updated on every install

# Install from Pipfile.lock (exact versions)
pipenv install --deploy

# Exit the shell
exit
```

---

### Option D — `poetry`

Modern dependency management with publishing support.

```bash
# Install poetry
pip install poetry

# Create a new project
poetry new my-project
# Or initialize in existing project:
cd my-project
poetry init

# Add dependencies
poetry add requests flask

# Add dev dependencies
poetry add --group dev pytest black

# Install all dependencies
poetry install

# Run a command inside the environment
poetry run python main.py

# Activate the shell
poetry shell

# Export to requirements.txt (for compatibility)
poetry export -f requirements.txt --output requirements.txt
```

---

## Part 2 — Node.js Environment Isolation

### How Node.js Isolation Works

Node.js uses `node_modules/` for **per-project package isolation** by default. Each project has its own `node_modules/` folder with its own copies of dependencies.

However, you may need **multiple Node.js versions** across projects. That's where `nvm` comes in.

### `nvm` — Node Version Manager

#### Install nvm

**Windows:** Download and install [nvm-windows](https://github.com/coreybutler/nvm-windows/releases) (run the installer `.exe`). After installation, restart your terminal.

**Linux/macOS:**
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

#### Step-by-Step

```bash
# List available Node versions
nvm list available        # Windows
nvm ls-remote             # Linux/macOS

# Install a specific Node version
nvm install 20.11.0

# Install the latest LTS
nvm install --lts

# Switch to a specific version
nvm use 20.11.0

# Set a default version
nvm alias default 20.11.0    # Linux/macOS
nvm use 20.11.0              # Windows (set as current)

# Check current version
node --version

# List installed versions
nvm list
```

#### `.nvmrc` File — Per-Project Node Version

Create a `.nvmrc` file in your project root:

```
20.11.0
```

Then anyone who clones the project can run:
```bash
nvm use
# Reads .nvmrc and switches to the correct version
```

### Node.js Package Isolation

```bash
# Initialize a project (creates package.json)
npm init -y

# Install dependencies (goes into node_modules/)
npm install express

# Install dev dependencies
npm install --save-dev jest

# Install from existing package.json
npm install

# Install exact versions from lock file
npm ci
```

#### Key Files

| File | Purpose | Commit to Git? |
|---|---|---|
| `package.json` | Dependency list with version ranges | ✅ Yes |
| `package-lock.json` | Exact locked versions | ✅ Yes |
| `node_modules/` | Installed packages | ❌ Never |

---

## Part 3 — Other Languages & Platforms

### Java — SDKMAN!

Manages multiple JDK versions and other JVM tools.

**Linux/macOS:**
```bash
# Install SDKMAN!
curl -s "https://get.sdkman.io" | bash

# List available Java versions
sdk list java

# Install a specific version
sdk install java 21.0.1-tem

# Switch versions
sdk use java 17.0.9-tem

# Set default
sdk default java 21.0.1-tem
```

**Windows:** SDKMAN! requires Git Bash or WSL. Alternatively, use [Jabba](https://github.com/shyiko/jabba) or manually download JDKs from [Adoptium](https://adoptium.net/) and manage PATH manually.

### Ruby — rbenv

**Linux/macOS:**
```bash
# Install rbenv
# macOS:
brew install rbenv

# Linux:
# Follow https://github.com/rbenv/rbenv#installation

# List available versions
rbenv install --list

# Install a version
rbenv install 3.3.0

# Set project-specific version (creates .ruby-version file)
rbenv local 3.3.0

# Set global default
rbenv global 3.3.0
```

**Windows:** Use [RubyInstaller](https://rubyinstaller.org/) to install and manage Ruby versions on Windows. Download the installer from the website and run it.

### Rust — rustup

Rust has built-in toolchain management. Works on **all platforms** including Windows.

**Linux/macOS:**
```bash
# Install Rust (includes rustup)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

**Windows:** Download and run the installer from [https://rustup.rs](https://rustup.rs). You may need to install [Visual Studio C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) first.

**All platforms (after installation):**
```bash
# Update to latest stable
rustup update stable

# Switch toolchains
rustup default stable
rustup default nightly

# Per-project toolchain (creates rust-toolchain.toml)
rustup override set 1.75.0
```

### Go — Built-in Module Isolation

Go modules provide per-project dependency isolation since Go 1.11:

```bash
# Initialize a module
go mod init github.com/timothygey/my-project

# Add a dependency
go get github.com/gin-gonic/gin

# Download all dependencies
go mod download

# Clean unused dependencies
go mod tidy
```

Key files: `go.mod` (commit ✅) and `go.sum` (commit ✅).

### Docker — The Ultimate Isolation

Docker containers provide **complete environment isolation** — not just packages, but the entire OS, runtime, and configuration.

```dockerfile
# Example Dockerfile for a Python project
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "main.py"]
```

```bash
# Build the container
docker build -t my-app .

# Run it
docker run my-app
```

Docker is ideal when you need:
- Identical environments across development, staging, and production
- Complex system dependencies (databases, message queues)
- Team-wide environment consistency

---

## Part 4 — Daily Workflow: Resuming Work in VS Code

Here's what to do every time you open VS Code and resume working on a project with a virtual environment.

### Python Project — Daily Start

#### Step 1 — Open Your Project

Open VS Code → **File → Open Folder** → select your project folder.

Or from the terminal:
```bash
code C:\path\to\your\project
```

#### Step 2 — Open the Terminal

Press `` Ctrl + ` `` to open the integrated terminal.

#### Step 3 — Activate the Virtual Environment

**Windows CMD:**
```cmd
venv\Scripts\activate
```

**Windows PowerShell:**
```powershell
.\venv\Scripts\Activate.ps1
```

**Git Bash / Linux / macOS:**
```bash
source venv/bin/activate
```

You should see `(venv)` appear in your terminal prompt.

#### Step 4 — Verify the Correct Environment

```bash
# Check which Python is being used
python --version
which python        # Linux/macOS/Git Bash
where python        # Windows CMD

# Should point to your venv, e.g.:
# C:\Users\timothygey\my-project\venv\Scripts\python.exe
```

#### Step 5 — VS Code Python Interpreter (One-Time Setup)

VS Code needs to know which Python interpreter to use for IntelliSense, linting, and debugging:

1. Press `Ctrl + Shift + P` (Command Palette)
2. Type: **"Python: Select Interpreter"**
3. Select the one inside your `venv`:
   ```
   Python 3.x.x ('venv': venv) .\venv\Scripts\python.exe
   ```

> VS Code remembers this setting per project. You only need to do this once.

#### Step 6 — VS Code Auto-Activation (Optional Setup)

To make VS Code automatically activate your venv when you open a terminal:

1. Open VS Code Settings (`Ctrl + ,`)
2. Search for `python.terminal.activateEnvironment`
3. Ensure it's set to **`true`** (this is the default)

With this enabled, opening a new terminal in VS Code automatically activates `venv`.

#### Step 7 — Work on Your Code

You're ready! Edit, run, and debug as normal. All `pip install` commands will install into the virtual environment.

#### Step 8 — Before Closing: Update Dependencies

If you installed new packages during the session:

```bash
pip freeze > requirements.txt
git add requirements.txt
git commit -m "update dependencies"
git push
```

#### Step 9 — Deactivate (Optional)

```bash
deactivate
```

> If you just close VS Code, the environment deactivates automatically.

---

### Node.js Project — Daily Start

#### Step 1 — Open Your Project in VS Code

#### Step 2 — Open the Terminal (`Ctrl + `` `)

#### Step 3 — Ensure Correct Node Version

```bash
# If using nvm with .nvmrc:
nvm use

# Verify
node --version
```

#### Step 4 — Install Dependencies (If Needed)

```bash
# If node_modules/ doesn't exist (fresh clone or deleted):
npm install

# Or for exact versions:
npm ci
```

#### Step 5 — Start Development

```bash
npm run dev          # or whatever your start script is
```

#### Step 6 — Before Closing: Update Dependencies

If you added new packages:
```bash
git add package.json package-lock.json
git commit -m "update dependencies"
git push
```

---

### Quick Daily Checklist

```
□ Open project in VS Code
□ Open terminal (Ctrl + `)
□ Activate virtual environment (Python) or verify Node version
□ Verify correct interpreter: python --version / node --version
□ Pull latest changes: git pull
□ Install any new dependencies: pip install -r requirements.txt / npm install
□ Work on your code
□ Save dependencies if changed: pip freeze > requirements.txt
□ Commit and push: git add . → git commit → git push
□ Deactivate (optional): deactivate
```

---

## Part 5 — Troubleshooting: Missing Dependencies & Broken Environments

### Symptom Checklist

| Symptom | Likely Cause | Solution |
|---|---|---|
| `ModuleNotFoundError: No module named 'xyz'` | Package not installed in venv | [Install missing package](#fix-1--install-the-missing-package) |
| `command not found: python` | Venv not activated or Python not in PATH | [Activate venv](#fix-2--activate-the-virtual-environment) |
| Wrong Python version running | Wrong interpreter selected | [Select correct interpreter](#fix-3--select-the-correct-interpreter) |
| `pip install` installs globally | Venv not activated | [Activate venv](#fix-2--activate-the-virtual-environment) |
| Packages conflict / version errors | Corrupted environment | [Recreate the environment](#fix-5--recreate-the-environment-from-scratch) |
| `No matching distribution found` | Python version incompatibility | [Check version compatibility](#fix-4--check-version-compatibility) |
| `node: command not found` | nvm not loaded or Node not installed | Restart terminal, run `nvm use` |
| `Cannot find module 'xyz'` (Node) | Missing `node_modules/` | Run `npm install` or `npm ci` |

---

### Fix 1 — Install the Missing Package

```bash
# Make sure venv is activated first!
pip install missing-package-name

# Or install everything from requirements.txt:
pip install -r requirements.txt
```

Verify it's installed:
```bash
pip list | grep package-name        # Linux/macOS
pip list | findstr package-name     # Windows CMD
```

---

### Fix 2 — Activate the Virtual Environment

The most common issue. If you see `(venv)` in your prompt, it's active. If not:

```bash
# Windows CMD
venv\Scripts\activate

# Windows PowerShell
.\venv\Scripts\Activate.ps1

# Git Bash / Linux / macOS
source venv/bin/activate
```

**PowerShell script execution error?**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

### Fix 3 — Select the Correct Interpreter

Check which Python is being used:

```bash
# Should point to your venv
which python        # Linux/macOS/Git Bash
where python        # Windows CMD
```

**Expected output:**
```
C:\Users\timothygey\my-project\venv\Scripts\python.exe
```

**If it points to the system Python** (e.g., `C:\Python312\python.exe`), your venv is not activated or VS Code is using the wrong interpreter.

**VS Code fix:**
1. `Ctrl + Shift + P` → "Python: Select Interpreter"
2. Choose the `venv` interpreter

---

### Fix 4 — Check Version Compatibility

Some packages require a minimum Python version:

```bash
# Check your Python version
python --version

# Check the package's requirements
pip install package-name --dry-run
```

If there's a version mismatch, you may need to:
- Create a new venv with a compatible Python version
- Or find an older version of the package: `pip install package-name==older.version`

---

### Fix 5 — Recreate the Environment from Scratch

When the environment is too broken to fix, delete and recreate it:

**Step 1 — Deactivate**
```bash
deactivate
```

**Step 2 — Delete the venv folder**

```bash
# Windows CMD
rmdir /s /q venv

# PowerShell
Remove-Item -Recurse -Force venv

# Linux/macOS
rm -rf venv
```

**Step 3 — Recreate**
```bash
python -m venv venv
venv\Scripts\activate          # Windows CMD
pip install -r requirements.txt
```

**Step 4 — Verify**
```bash
python --version
pip list
python -c "import your_main_module"
```

---

### Fix 6 — Clear pip Cache

Sometimes cached packages cause issues:

```bash
pip cache purge
pip install -r requirements.txt --no-cache-dir
```

---

### Fix 7 — Node.js: Recreate node_modules

**Linux/macOS/Git Bash:**
```bash
# Delete node_modules and lock file
rm -rf node_modules
rm package-lock.json
```

**Windows CMD:**
```cmd
rmdir /s /q node_modules
del package-lock.json
```

**Windows PowerShell:**
```powershell
Remove-Item -Recurse -Force node_modules
Remove-Item package-lock.json
```

**Then reinstall (all platforms):**
```bash
# Reinstall
npm install

# Or for exact versions (if lock file exists):
npm ci
```

---

### Fix 8 — Nuclear Option: Fresh Start Checklist

If everything seems broken:

**Step 1 — Deactivate:**
```bash
deactivate
```

**Step 2 — Delete the virtual environment:**

| Platform | Command |
|---|---|
| Linux/macOS/Git Bash | `rm -rf venv` |
| Windows CMD | `rmdir /s /q venv` |
| Windows PowerShell | `Remove-Item -Recurse -Force venv` |

**Step 3 — Check your Python installation:**
```bash
python --version               # Confirm Python is installed system-wide
```

**Step 4 — Recreate the virtual environment:**
```bash
python -m venv venv
```

**Step 5 — Activate:**

| Platform | Command |
|---|---|
| Windows CMD | `venv\Scripts\activate` |
| Windows PowerShell | `.\venv\Scripts\Activate.ps1` |
| Git Bash / Linux / macOS | `source venv/bin/activate` |

**Step 6 — Upgrade pip:**
```bash
python -m pip install --upgrade pip
```

**Step 7 — Reinstall dependencies:**
```bash
pip install -r requirements.txt
```

**Step 8 — Test:**
```bash
python main.py                 # or whatever your entry point is
```

**Step 9 — Verify everything is working:**
```bash
pip list
python -c "import flask; print(flask.__version__)"
```

---

## Part 6 — What NEVER to Do in a Virtual Environment

These are common mistakes that cause hard-to-debug issues. Avoid them at all costs.

---

### ❌ 1. NEVER Use `sudo pip install` Inside a Virtual Environment

```bash
# WRONG — installs globally, bypasses venv
sudo pip install requests

# CORRECT — no sudo needed inside venv
pip install requests
```

**Why:** `sudo` elevates to root, which uses the system Python, completely bypassing your virtual environment. Packages end up in the system Python's `site-packages`.

---

### ❌ 2. NEVER Commit `venv/` or `node_modules/` to Git

```gitignore
# These must ALWAYS be in .gitignore
venv/
.venv/
env/
node_modules/
```

**Why:**
- `venv/` can be 100-500MB — bloats your repository
- Contains **platform-specific binaries** — won't work on a different OS
- Contains **absolute paths** — breaks on other machines
- Can be recreated from `requirements.txt` or `package.json` in seconds

---

### ❌ 3. NEVER Install Packages Globally When a Venv Exists

```bash
# WRONG — pollutes global Python
pip install flask                  # (without venv activated)

# CORRECT — activate first, then install
venv\Scripts\activate
pip install flask
```

**Why:** Global packages create conflicts between projects and can break system tools that depend on specific versions.

---

### ❌ 4. NEVER Share a `venv/` Folder Between Machines

Don't copy `venv/` to another computer, USB drive, or cloud sync. Instead:

```bash
# On the original machine: export dependencies
pip freeze > requirements.txt

# On the new machine: recreate
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

**Why:** Virtual environments contain **absolute paths** and **platform-specific compiled files**. A venv created on Windows won't work on macOS/Linux and vice versa.

---

### ❌ 5. NEVER Manually Edit Files Inside `venv/` or `node_modules/`

```
venv/Lib/site-packages/flask/app.py    ← NEVER edit this
node_modules/express/lib/router.js     ← NEVER edit this
```

**Why:**
- Your changes are **lost** when you recreate the environment or run `pip install` / `npm install`
- You can't track or share these changes via Git
- You may introduce bugs that are impossible to reproduce

**Instead:** Fork the package, make your changes, and install from your fork.

---

### ❌ 6. NEVER Mix Package Managers in the Same Environment

```bash
# WRONG — mixing conda and pip carelessly
conda install numpy
pip install pandas
conda install scipy            # May overwrite pip's pandas installation

# CORRECT — use conda for everything possible, pip only as last resort
conda install numpy pandas scipy
pip install some-pip-only-package     # Only if not available in conda
```

**Why:** `conda` and `pip` don't know about each other's installations. Installing with one can silently overwrite packages installed by the other, leading to broken environments.

**If you must mix:** Install all `conda` packages first, then `pip` packages last. Never go back to `conda install` after using `pip`.

---

### ❌ 7. NEVER Delete `requirements.txt`, `package-lock.json`, or Lock Files

| File | Tool | Purpose |
|---|---|---|
| `requirements.txt` | pip | Records exact package versions |
| `Pipfile.lock` | pipenv | Locks exact versions + hashes |
| `poetry.lock` | poetry | Locks exact versions + hashes |
| `package-lock.json` | npm | Locks exact versions of Node packages |
| `yarn.lock` | yarn | Locks exact versions of Node packages |
| `go.sum` | Go | Checksums for Go modules |

**Why:** These files are the **only reliable way** to recreate your environment. Without them, `pip install` or `npm install` may install newer (potentially breaking) versions of dependencies.

---

### ❌ 8. NEVER Use System Python for Project Work

```bash
# WRONG — using system Python directly
C:\Python312\python.exe main.py
/usr/bin/python3 main.py

# CORRECT — always work inside a virtual environment
python -m venv venv
venv\Scripts\activate
python main.py
```

**Why:** System Python is for the operating system's own tools. Installing project packages into it can:
- Break OS utilities that depend on specific package versions
- Cause conflicts between unrelated projects
- Make it impossible to know which packages belong to which project

---

### ❌ 9. NEVER Hardcode Absolute Paths to Your Virtual Environment

```python
# WRONG — in your code or scripts
import sys
sys.path.append("C:/Users/timothygey/project/venv/Lib/site-packages")

# CORRECT — just activate the venv and imports work automatically
# No path manipulation needed
import flask   # Works when venv is activated
```

**Why:** Absolute paths break on every other machine, OS, and username.

---

### ❌ 10. NEVER Run `pip install` Outside a Virtual Environment Without Knowing It

Always verify before installing:

```bash
# Check: am I in a virtual environment?
# Look for (venv) in your prompt, or run:
python -c "import sys; print(sys.prefix)"

# If the output is your venv path → you're safe
# If it's the system Python path → STOP and activate your venv first
```

---

### Summary: Do's and Don'ts

| ✅ DO | ❌ DON'T |
|---|---|
| Create a venv for every project | Use system Python for project work |
| Activate before installing packages | Use `sudo pip install` in a venv |
| Keep `requirements.txt` updated | Commit `venv/` or `node_modules/` to Git |
| Recreate venv from lock files | Copy `venv/` between machines |
| Use one package manager per environment | Mix `conda` and `pip` carelessly |
| Add `venv/` to `.gitignore` | Edit files inside `venv/` manually |
| Commit lock files to Git | Delete lock files |
| Verify your interpreter with `which python` (Linux/macOS) or `where python` (Windows) | Assume the correct Python is being used |

---

## Part 7 — Q&A: Installing, Upgrading & Managing Packages

Common questions about managing packages inside a virtual environment.

---

### Q: How do I install the latest version of a package?

**A:** With your virtual environment activated, simply run `pip install` with the package name. By default, pip installs the **latest stable version**.

```bash
# Make sure your venv is activated first!
# (venv) should appear in your prompt

# Install the latest version of a single package
pip install flask

# Install multiple packages at once
pip install flask selenium numpy requests

# After installing, update your requirements file
pip freeze > requirements.txt
```

---

### Q: How do I upgrade a package to the latest version?

**A:** Use the `--upgrade` (or `-U`) flag:

```bash
# Upgrade a single package to the latest version
pip install --upgrade flask

# Upgrade multiple packages
pip install --upgrade flask selenium requests

# Upgrade pip itself (do this regularly!)
python -m pip install --upgrade pip

# Upgrade ALL packages in your environment at once
pip list --outdated --format=columns   # First, see what's outdated
```

**Upgrade all outdated packages — Linux/macOS/Git Bash:**
```bash
pip install --upgrade $(pip list --outdated --format=freeze | cut -d= -f1)
```

**Upgrade all outdated packages — Windows CMD:**
```cmd
pip list --outdated --format=freeze > outdated.txt
for /F "delims==" %%i in (outdated.txt) do pip install --upgrade %%i
del outdated.txt
```

**Upgrade all outdated packages — Windows PowerShell:**
```powershell
pip list --outdated --format=freeze | ForEach-Object { pip install --upgrade ($_ -split '==')[0] }
```

**After upgrading, always update your requirements file:**
```bash
pip freeze > requirements.txt
```

---

### Q: How do I install a specific version of a package?

**A:** Use the `==` operator to pin an exact version:

```bash
# Install an exact version
pip install flask==2.3.2
pip install selenium==4.15.0
pip install numpy==1.24.3

# Install a minimum version (this version or newer)
pip install flask>=2.3.0

# Install within a version range
pip install flask>=2.3.0,<3.0.0

# Install a version compatible with a given release (~= means same major.minor)
pip install flask~=2.3.0     # Equivalent to >=2.3.0, <2.4.0
```

#### Version Specifiers Explained

| Specifier | Meaning | Example |
|---|---|---|
| `==2.3.2` | Exactly this version | `pip install flask==2.3.2` |
| `>=2.3.0` | This version or newer | `pip install flask>=2.3.0` |
| `<=2.3.2` | This version or older | `pip install flask<=2.3.2` |
| `>=2.3.0,<3.0.0` | Between these versions | `pip install "flask>=2.3.0,<3.0.0"` |
| `~=2.3.0` | Compatible release (>=2.3.0, <2.4.0) | `pip install flask~=2.3.0` |
| `!=2.3.1` | Anything except this version | `pip install flask!=2.3.1` |

> **Note:** On Windows CMD, wrap version ranges in quotes: `pip install "flask>=2.3.0,<3.0.0"`

---

### Q: How do I check what version of a package is currently installed?

**A:**

```bash
# Check a specific package
pip show flask

# Output includes:
# Name: Flask
# Version: 3.0.0
# Location: C:\...\venv\Lib\site-packages
# Requires: Werkzeug, Jinja2, ...

# List all installed packages with versions
pip list

# List only outdated packages
pip list --outdated
```

---

### Q: How do I find which versions of a package are available?

**A:**

```bash
# See all available versions of a package
pip index versions flask

# If the above doesn't work (older pip), use:
pip install flask==       # intentionally invalid — pip shows all available versions in the error message
```

---

### Q: How do I downgrade a package to an older version?

**A:** Just install the specific older version — pip will automatically replace the current one:

```bash
# Currently have flask 3.0.0, want to go back to 2.3.2
pip install flask==2.3.2

# Verify
pip show flask
# Version: 2.3.2
```

---

### Q: How do I uninstall a package?

**A:**

```bash
# Uninstall a single package
pip uninstall flask

# Uninstall without confirmation prompt
pip uninstall -y flask

# Uninstall multiple packages
pip uninstall flask requests numpy

# After uninstalling, update your requirements file
pip freeze > requirements.txt
```

---

### Q: How do I install packages from a `requirements.txt` file?

**A:**

```bash
# Install all packages listed in requirements.txt
pip install -r requirements.txt

# Install with exact versions (recommended for reproducibility)
pip install -r requirements.txt --no-deps
```

#### Example `requirements.txt`:
```
flask==3.0.0
selenium==4.15.0
numpy==1.26.2
requests==2.31.0
```

---

### Q: How do I upgrade pip itself?

**A:** Always upgrade pip first when creating a new environment — old pip can cause issues:

```bash
python -m pip install --upgrade pip
```

> **Why `python -m pip` instead of just `pip`?** Using `python -m pip` guarantees you're using the pip associated with the correct Python interpreter, avoiding PATH confusion.

---

### Q: How do I install a package from GitHub directly?

**A:**

```bash
# Install from a GitHub repo (latest on main branch)
pip install git+https://github.com/username/repo.git

# Install from a specific branch
pip install git+https://github.com/username/repo.git@branch-name

# Install from a specific tag/version
pip install git+https://github.com/username/repo.git@v1.0.0

# Install from a specific commit
pip install git+https://github.com/username/repo.git@abc1234
```

---

### Q: How do I install Node.js packages at a specific version?

**A:**

```bash
# Install latest
npm install express

# Install a specific version
npm install express@4.18.2

# Install the latest within a major version
npm install express@4

# Upgrade a package to the latest
npm install express@latest

# Check outdated packages
npm outdated

# Update all packages to latest allowed by package.json
npm update

# Check what's installed
npm list --depth=0
```

---

### Quick Reference Table

| Task | Python (pip) | Node.js (npm) |
|---|---|---|
| Install latest | `pip install flask` | `npm install express` |
| Install specific version | `pip install flask==2.3.2` | `npm install express@4.18.2` |
| Upgrade to latest | `pip install --upgrade flask` | `npm install express@latest` |
| Downgrade | `pip install flask==older` | `npm install express@older` |
| Uninstall | `pip uninstall flask` | `npm uninstall express` |
| List installed | `pip list` | `npm list --depth=0` |
| Check outdated | `pip list --outdated` | `npm outdated` |
| Install from file | `pip install -r requirements.txt` | `npm install` (from package.json) |
| Save to file | `pip freeze > requirements.txt` | Automatic (package.json) |
| Upgrade manager | `python -m pip install --upgrade pip` | `npm install -g npm@latest` |
