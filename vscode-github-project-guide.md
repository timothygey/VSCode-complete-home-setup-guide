# VS Code & GitHub Project Setup Guide

> A step-by-step guide to creating a new project, initializing Git, and pushing to GitHub.
> Covers general projects, C++ projects, Python projects, and a comprehensive Git commands reference.
> Author: timothygey | Created: 24 April 2026

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Part 1 — General Project Setup (Any Project Type)](#part-1--general-project-setup-any-project-type)
- [Part 2 — C++ Project Setup](#part-2--c-project-setup)
- [Part 3 — Python Project Setup](#part-3--python-project-setup)
- [Part 4 — Git Version Control Commands Reference](#part-4--git-version-control-commands-reference)
- [Daily Workflow Quick Reference](#daily-workflow-quick-reference)

---

## Prerequisites

Before starting, ensure you have:

- [x] **Git** installed — verify with `git --version`
- [x] **Git identity** configured:
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "your-github-email@example.com"
  ```
- [x] **Default branch** set to `main`:
  ```bash
  git config --global init.defaultBranch main
  ```
- [x] **SSH key** set up and linked to GitHub — verify with `ssh -T git@github.com`
- [x] **VS Code** installed
- [x] **GitHub account** ready

> For full Git/SSH setup instructions, see [git-setup-guide.md](git-setup-guide.md)

---

## Part 1 — General Project Setup (Any Project Type)

This section covers the universal steps for any project — whether it's a web app, IoT project, server deployment, game, mobile app, or anything else.

### Step 1 — Create a New Repository on GitHub

1. Go to [https://github.com/new](https://github.com/new)
2. Enter a **Repository name** (e.g., `my-project`)
3. Add an optional **Description**
4. Choose **Public** or **Private**
5. ⚠️ **Do NOT** check any of these:
   - ❌ Add a README file
   - ❌ Add .gitignore
   - ❌ Choose a license
6. Click **Create repository**
7. **Keep this page open** — you'll need the SSH URL from it

> **Why leave it empty?** Initializing with files on GitHub creates a commit history that conflicts with your local project's history, causing merge issues when you push.

---

### Step 2 — Create a `.gitignore` File

Before tracking files with Git, you need to tell it which files to **ignore** (build artifacts, secrets, IDE configs, etc.).

#### How to Create `.gitignore`

**Option A — VS Code (Easiest):**
1. Open your project folder in VS Code (File → Open Folder)
2. Right-click in the Explorer panel → **New File**
3. Name it `.gitignore` (including the leading dot)
4. Paste the appropriate template (see below)
5. Save (`Ctrl + S`)

**Option B — Terminal (Windows CMD):**
```cmd
cd C:\path\to\your\project
type nul > .gitignore
notepad .gitignore
```
Paste content, save, and close Notepad.

**Option C — Terminal (PowerShell):**
```powershell
cd C:\path\to\your\project
New-Item -Name ".gitignore" -ItemType File
notepad .gitignore
```

**Option D — Terminal (Git Bash / Linux / macOS):**
```bash
cd /path/to/your/project
touch .gitignore
nano .gitignore
```
Paste content, `Ctrl+O` to save, `Ctrl+X` to exit.

> **Windows Tip:** If Windows Explorer won't let you create a file starting with `.`, name it `.gitignore.` (with a trailing dot) — Windows removes the trailing dot automatically.

#### Common `.gitignore` Templates

GitHub maintains official templates for every language/framework:
👉 [https://github.com/github/gitignore](https://github.com/github/gitignore)

| Project Type | Template Link |
|---|---|
| C++ | [C++.gitignore](https://github.com/github/gitignore/blob/main/C%2B%2B.gitignore) |
| Python | [Python.gitignore](https://github.com/github/gitignore/blob/main/Python.gitignore) |
| Node.js | [Node.gitignore](https://github.com/github/gitignore/blob/main/Node.gitignore) |
| Java | [Java.gitignore](https://github.com/github/gitignore/blob/main/Java.gitignore) |
| Go | [Go.gitignore](https://github.com/github/gitignore/blob/main/Go.gitignore) |
| Rust | [Rust.gitignore](https://github.com/github/gitignore/blob/main/Rust.gitignore) |
| Unity | [Unity.gitignore](https://github.com/github/gitignore/blob/main/Unity.gitignore) |
| Visual Studio | [VisualStudio.gitignore](https://github.com/github/gitignore/blob/main/VisualStudio.gitignore) |

#### Universal Entries (Add These to Any Project)

These entries are **not language-specific** — they apply to every project type. After pasting your language-specific template (e.g., C++, Python, Node.js) into your `.gitignore` file, **append the following lines at the bottom of the same `.gitignore` file**. This ensures OS junk files, IDE settings, and secrets are always excluded regardless of your project type.

Copy and paste these lines at the end of your `.gitignore`:

```gitignore
# OS files
.DS_Store
Thumbs.db

# IDE / Editor
.vscode/
.idea/
*.swp
*.swo
*~

# Environment / Secrets
.env
.env.local
*.pem
*.key
```

> **In summary:** Your `.gitignore` should contain **language-specific patterns first** (from the templates above), followed by **these universal entries** at the bottom. You only need one `.gitignore` file — everything goes in the same file.

---

### Step 3 — Initialize Git

Open the terminal in VS Code (`Ctrl + `` `) and navigate to your project folder:

```bash
cd C:\path\to\your\project
git init
```

Output: `Initialized empty Git repository in .../your-project/.git/`

---

### Step 4 — Stage Your Files

```bash
git add .
```

Then verify what's staged:

```bash
git status
```

**Check the output carefully:**
- ✅ Source files, config files, documentation — should be listed
- ❌ Build artifacts, binaries, secrets — should NOT be listed

If unwanted files appear, update your `.gitignore`, then reset and re-add:

```bash
git rm -r --cached .
git add .
```

---

### Step 5 — Make Your First Commit

```bash
git commit -m "Initial commit"
```

---

### Step 6 — Connect to GitHub

Copy the SSH URL from the GitHub repo page you created in Step 1:

```bash
git remote add origin git@github.com:timothygey/your-repo-name.git
```

---

### Step 7 — Push to GitHub

```bash
git branch -M main
git push -u origin main
```

| Flag | Purpose |
|---|---|
| `-M main` | Renames the current branch to `main` (in case it defaulted to `master`) |
| `-u origin main` | Sets the upstream so future pushes only need `git push` |

---

### Step 8 — Verify on GitHub

Refresh your GitHub repo page — all your files should be visible.

---

### Visual Summary

```
Your Local Project Folder
    │
    │  1. Create .gitignore
    │  2. git init
    │  3. git add .
    │  4. git commit -m "Initial commit"
    │
    ▼
Local Git Repository
    │
    │  5. git remote add origin git@github.com:timothygey/repo.git
    │  6. git branch -M main
    │  7. git push -u origin main
    │
    ▼
GitHub Repository (Remote)
```

---

## Part 2 — C++ Project Setup

### Recommended Folder Structure

```
my-cpp-project/
├── .gitignore
├── README.md
├── CMakeLists.txt          # or Makefile
├── src/
│   ├── main.cpp
│   ├── utils.cpp
│   └── utils.h
├── include/
│   └── project_headers.h
├── tests/
│   └── test_main.cpp
└── build/                  # ← ignored by Git
```

### C++ `.gitignore`

Create `.gitignore` in your project root with this content:

```gitignore
# ===== Compiled Files =====
*.o
*.obj
*.exe
*.out
*.app
*.dll
*.so
*.dylib
*.a
*.lib

# ===== Build Directories =====
build/
bin/
Debug/
Release/
x64/
x86/
out/

# ===== Visual Studio =====
.vs/
*.vcxproj.user
*.suo
*.sdf
*.opensdf
*.db
*.ipch

# ===== VS Code =====
.vscode/

# ===== CMake =====
CMakeCache.txt
CMakeFiles/
cmake-build-*/
cmake_install.cmake
Makefile

# ===== OS Files =====
.DS_Store
Thumbs.db

# ===== IDE / Editor =====
*.swp
*.swo
*~
```

#### What Each Pattern Means

| Pattern | What It Ignores | Why |
|---|---|---|
| `*.o` / `*.obj` | Object files | Compiled from source — can be regenerated |
| `*.exe` / `*.out` | Executables | Build output — not source code |
| `build/` | Build directory | All generated build files |
| `.vs/` | Visual Studio settings | Personal IDE config — differs per developer |
| `CMakeFiles/` | CMake generated files | Auto-generated — not source code |
| `.vscode/` | VS Code workspace settings | Personal IDE config |

### Full Step-by-Step

1. **Create empty repo on GitHub** ([github.com/new](https://github.com/new)) — don't initialize with any files

2. **Open your C++ project folder in VS Code**
   ```
   File → Open Folder → select your project folder
   ```

3. **Create `.gitignore`**
   - Right-click in VS Code Explorer → New File → `.gitignore`
   - Paste the C++ template above → Save

4. **Open VS Code terminal** (`Ctrl + `` `)

5. **Initialize and push:**
   ```bash
   git init
   git add .
   git status
   ```
   Verify no `.exe`, `.o`, or `build/` files are listed, then:
   ```bash
   git commit -m "Initial commit"
   git remote add origin git@github.com:timothygey/your-cpp-repo.git
   git branch -M main
   git push -u origin main
   ```

6. **Verify on GitHub** — refresh the repo page

### Troubleshooting

**Problem:** `.gitignore` was created after `git add .` and unwanted files are tracked.
**Solution:**
```bash
git rm -r --cached .
git add .
git commit -m "Remove tracked files that should be ignored"
git push
```

**Problem:** Windows Explorer won't create a file named `.gitignore`
**Solution:** Use VS Code or terminal to create it (see [Step 2](#step-2--create-a-gitignore-file) above).

---

## Part 3 — Python Project Setup

### Recommended Folder Structure

```
my-python-project/
├── .gitignore
├── README.md
├── requirements.txt        # ← dependencies list
├── setup.py                # or pyproject.toml
├── .env                    # ← ignored by Git (secrets)
├── src/
│   ├── __init__.py
│   ├── main.py
│   └── utils.py
├── tests/
│   ├── __init__.py
│   └── test_main.py
└── venv/                   # ← ignored by Git
```

### Python `.gitignore`

Create `.gitignore` in your project root with this content:

```gitignore
# ===== Python Bytecode =====
__pycache__/
*.py[cod]
*$py.class

# ===== Virtual Environments =====
venv/
env/
.venv/
.env/
ENV/

# ===== Distribution / Packaging =====
dist/
build/
*.egg-info/
*.egg
*.whl
sdist/

# ===== Testing =====
.pytest_cache/
.coverage
htmlcov/
.tox/
.nox/

# ===== IDEs =====
.vscode/
.idea/
*.swp
*.swo
*~

# ===== Jupyter Notebooks =====
.ipynb_checkpoints/

# ===== Environment Variables / Secrets =====
.env
.env.local
*.pem
*.key

# ===== OS Files =====
.DS_Store
Thumbs.db

# ===== Logs =====
*.log

# ===== Database =====
*.db
*.sqlite3
```

#### What Each Pattern Means

| Pattern | What It Ignores | Why |
|---|---|---|
| `__pycache__/` | Python bytecode cache | Auto-generated when you run `.py` files |
| `*.py[cod]` | `.pyc`, `.pyo`, `.pyd` files | Compiled Python files — can be regenerated |
| `venv/` | Virtual environment folder | Contains installed packages — use `requirements.txt` instead |
| `dist/` / `*.egg-info/` | Package distribution files | Build output from `setup.py` |
| `.pytest_cache/` | Pytest cache | Auto-generated test cache |
| `.env` | Environment variables file | Contains secrets (API keys, passwords) |
| `.coverage` | Test coverage data | Generated by coverage tools |

### Setting Up Virtual Environment (Before Committing)

Always create a virtual environment for Python projects:

```bash
# Create virtual environment
python -m venv venv

# Activate it
# Windows CMD:
venv\Scripts\activate
# Windows PowerShell:
.\venv\Scripts\Activate.ps1
# Linux/macOS:
source venv/bin/activate

# Install your dependencies
pip install package-name

# Generate requirements.txt (commit this file!)
pip freeze > requirements.txt
```

> **Important:** `venv/` is ignored by Git, but `requirements.txt` is committed. This lets others recreate your environment with `pip install -r requirements.txt`.

### Full Step-by-Step

1. **Create empty repo on GitHub** ([github.com/new](https://github.com/new)) — don't initialize with any files

2. **Open your Python project folder in VS Code**
   ```
   File → Open Folder → select your project folder
   ```

3. **Create `.gitignore`**
   - Right-click in VS Code Explorer → New File → `.gitignore`
   - Paste the Python template above → Save

4. **Set up virtual environment** (if not already done)
   ```bash
   python -m venv venv
   venv\Scripts\activate          # Windows CMD
   pip install -r requirements.txt # if you already have one
   ```

5. **Generate `requirements.txt`** (if not already done)
   ```bash
   pip freeze > requirements.txt
   ```

6. **Open VS Code terminal** (`Ctrl + `` `)

7. **Initialize and push:**
   ```bash
   git init
   git add .
   git status
   ```
   Verify that `venv/`, `__pycache__/`, and `.env` are NOT listed, then:
   ```bash
   git commit -m "Initial commit"
   git remote add origin git@github.com:timothygey/your-python-repo.git
   git branch -M main
   git push -u origin main
   ```

8. **Verify on GitHub** — refresh the repo page

### Troubleshooting

**Problem:** `__pycache__/` or `venv/` is already tracked.
**Solution:**
```bash
git rm -r --cached __pycache__/
git rm -r --cached venv/
git commit -m "Remove cached files that should be ignored"
git push
```

**Problem:** PowerShell won't activate venv — "running scripts is disabled"
**Solution:**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
Then try activating again.

**Problem:** `pip freeze` shows too many packages.
**Solution:** Use `pipreqs` to generate only the packages your code actually imports:
```bash
pip install pipreqs
pipreqs . --force
```

---

## Part 4 — Git Version Control Commands Reference

A comprehensive reference of commonly used Git commands with a real-world example scenario for each.

---

### `git add` — Stage Changes

Stages files for the next commit. You choose what goes into the commit.

```bash
# Stage all changes
git add .

# Stage a specific file
git add src/main.py

# Stage interactively (pick individual hunks)
git add -p
```

**Scenario:** You modified 5 files but only want to commit changes to 2 of them right now.
```bash
git add src/auth.py src/login.py
git commit -m "fix: login authentication bug"
# The other 3 files remain uncommitted for a separate commit later
```

---

### `git commit` — Save a Snapshot

Records the staged changes as a new commit in your local history.

```bash
# Commit with a message
git commit -m "add user authentication module"

# Commit and automatically stage all tracked modified files (skip git add)
git commit -am "fix typo in README"

# Amend the last commit (change message or add forgotten files)
git commit --amend -m "updated commit message"

# Amend without changing the message (just add more files)
git add forgotten-file.py
git commit --amend --no-edit
```

**Scenario:** You just committed but forgot to include a file.
```bash
git add forgotten-file.py
git commit --amend --no-edit
# The forgotten file is now part of the previous commit — no extra commit created
```

---

### `git push` — Upload to Remote

Sends your local commits to the remote repository (GitHub).

```bash
# Push to the upstream branch
git push

# Push and set upstream (first time)
git push -u origin main

# Push a specific branch
git push origin feature/login
```

**Scenario:** You created a new branch locally and want to push it to GitHub for the first time.
```bash
git checkout -b feature/dark-mode
# ... make changes and commit ...
git push -u origin feature/dark-mode
# Now the branch exists on GitHub and is linked for future pushes
```

---

### `git push --force-with-lease` — Safe Force Push

Force pushes your branch but **only if no one else has pushed new commits** since your last fetch. Safer than `--force`.

```bash
git push --force-with-lease
```

**Scenario:** You rebased your feature branch onto the latest `main` (rewriting history) and need to update the remote branch, but want to make sure a teammate hasn't pushed to it in the meantime.
```bash
git checkout feature/api-refactor
git rebase main
# History was rewritten — normal push will be rejected
git push --force-with-lease
# If a teammate pushed to this branch since your last fetch, this FAILS safely
# If no one else pushed, it succeeds
```

> ⚠️ **Never use `--force`** on shared branches like `main`. Use `--force-with-lease` instead — it prevents accidentally overwriting a teammate's work.

---

### `git pull` — Download and Merge

Fetches changes from the remote and merges them into your current branch.

```bash
# Pull with merge (default)
git pull

# Pull with rebase (cleaner history)
git pull --rebase
```

**Scenario:** Your teammate pushed new commits to `main` while you were working. You need to get their changes before pushing yours.
```bash
git pull --rebase
# Fetches their commits, then replays your local commits on top
# Result: clean, linear history without merge commits
```

---

### `git fetch` — Download Without Merging

Downloads commits and branches from the remote but does **not** modify your working files. Lets you inspect changes before integrating them.

```bash
# Fetch all branches
git fetch

# Fetch a specific remote
git fetch origin

# Fetch and prune deleted remote branches
git fetch --prune
```

**Scenario:** You want to check what your teammate pushed to `main` before merging it into your branch.
```bash
git fetch origin
git log HEAD..origin/main --oneline
# Shows commits on remote main that you don't have locally
# Now you can decide: merge, rebase, or wait
```

---

### `git checkout` — Switch Branches or Restore Files

```bash
# Switch to an existing branch
git checkout main

# Create and switch to a new branch
git checkout -b feature/new-feature

# Restore a specific file to its last committed state (discard changes)
git checkout -- src/main.py
```

**Scenario:** You made experimental changes to a file and want to discard them — go back to the last committed version.
```bash
git checkout -- src/experiment.py
# The file is restored to its state at the last commit
# WARNING: This permanently discards your uncommitted changes to that file
```

> **Note:** Modern Git uses `git switch` for branch switching and `git restore` for file restoration, but `git checkout` still works for both.

---

### `git reset` — Undo Commits / Unstage Files

Moves the branch pointer backward, effectively "un-committing" changes.

```bash
# Unstage a file (keep changes in working directory)
git reset HEAD file.py

# Undo the last commit (keep changes staged)
git reset --soft HEAD~1

# Undo the last commit (keep changes unstaged)
git reset HEAD~1
# or equivalently:
git reset --mixed HEAD~1

# Undo the last commit AND discard all changes (DESTRUCTIVE)
git reset --hard HEAD~1
```

#### `git reset HEAD~X` — Undo X Commits from the Top

The `~X` notation means "go back X commits from the current position."

```bash
# Undo the last 3 commits (keep changes unstaged)
git reset HEAD~3

# Undo the last 5 commits (keep changes staged)
git reset --soft HEAD~5

# Undo the last 2 commits (DISCARD changes permanently)
git reset --hard HEAD~2
```

**Scenario:** You made 3 messy commits that should have been 1 clean commit.
```bash
git reset --soft HEAD~3
# All changes from those 3 commits are now staged
git commit -m "refactor: clean up authentication module"
# Now you have 1 clean commit instead of 3 messy ones
```

| Mode | Commits | Staging Area | Working Files |
|---|---|---|---|
| `--soft` | ❌ Removed | ✅ Kept (staged) | ✅ Kept |
| `--mixed` (default) | ❌ Removed | ❌ Unstaged | ✅ Kept |
| `--hard` | ❌ Removed | ❌ Discarded | ❌ Discarded |

> ⚠️ **Never use `git reset` on commits that have already been pushed** to a shared branch. Use `git revert` instead.

---

### `git revert` — Safely Undo a Published Commit

Creates a **new commit** that undoes the changes from a previous commit. Safe for shared branches because it doesn't rewrite history.

```bash
# Revert a specific commit
git revert <commit-hash>

# Revert the last commit
git revert HEAD

# Revert without auto-committing (lets you edit the revert)
git revert --no-commit <commit-hash>
```

**Scenario:** You pushed a commit to `main` that introduced a bug. You can't use `reset` because teammates have already pulled it.
```bash
git log --oneline
# a1b2c3d fix: update payment logic    ← this commit broke things
# d4e5f6g feat: add checkout page

git revert a1b2c3d
# Creates a NEW commit that undoes a1b2c3d
# History is preserved — safe for shared branches
git push
```

---

### `git rebase` — Rewrite History / Rebase onto Another Branch

Replays your commits on top of another branch, creating a clean linear history.

```bash
# Rebase current branch onto main
git rebase main

# Rebase onto remote main
git rebase origin/main
```

**Scenario:** You've been working on a feature branch, but `main` has moved forward. You want to update your branch to include the latest `main` changes with a clean history.
```bash
git checkout feature/dashboard
git fetch origin
git rebase origin/main
# Your feature branch commits are now replayed on top of the latest main
# If conflicts occur, resolve them, then:
git add .
git rebase --continue
# After rebasing, force push your feature branch:
git push --force-with-lease
```

#### Rebase vs Merge

| | `git merge main` | `git rebase main` |
|---|---|---|
| History | Creates a merge commit (non-linear) | Linear history (clean) |
| Safety | Safe for shared branches | ⚠️ Rewrites history — don't use on shared branches |
| When to use | Merging feature → main | Updating feature branch with latest main |

---

### `git rebase -i` (Interactive Rebase) — Edit, Squash, Reorder Commits

Opens an editor where you can modify, combine, reorder, or delete commits.

```bash
# Interactive rebase on the last 4 commits
git rebase -i HEAD~4

# Interactive rebase onto main
git rebase -i main
```

In the editor, you'll see:

```
pick a1b2c3d feat: add login page
pick d4e5f6g fix: typo in login
pick h7i8j9k fix: another typo
pick l0m1n2o feat: add logout button
```

Change the commands to:

```
pick a1b2c3d feat: add login page
squash d4e5f6g fix: typo in login        # combine into previous commit
squash h7i8j9k fix: another typo         # combine into previous commit
pick l0m1n2o feat: add logout button
```

**Available commands:**

| Command | Effect |
|---|---|
| `pick` | Keep the commit as-is |
| `squash` / `s` | Combine into previous commit (edit message) |
| `fixup` / `f` | Combine into previous commit (discard this message) |
| `reword` / `r` | Keep commit but edit the message |
| `edit` / `e` | Pause to amend the commit |
| `drop` / `d` | Delete the commit entirely |

**Scenario:** Before creating a pull request, you want to clean up your 6 messy commits into 2 logical ones.
```bash
git rebase -i HEAD~6
# In the editor:
#   pick   abc1234 feat: add user model
#   fixup  def5678 wip: still working on model
#   fixup  ghi9012 fix: model validation
#   pick   jkl3456 feat: add user API endpoint
#   fixup  mno7890 fix: endpoint error handling
#   fixup  pqr1234 style: clean up formatting
# Save and close — now you have 2 clean commits instead of 6

git push --force-with-lease
```

---

### `git stash` — Temporarily Shelve Changes

Saves your uncommitted changes and gives you a clean working directory. Useful for switching branches without committing half-done work.

```bash
# Stash current changes
git stash

# Stash with a description
git stash push -m "work in progress: login form"

# List all stashes
git stash list

# Restore the most recent stash (and remove from stash list)
git stash pop

# Restore a specific stash
git stash pop stash@{2}

# Restore without removing from stash list
git stash apply

# Delete a specific stash
git stash drop stash@{0}

# Delete all stashes
git stash clear
```

**Scenario:** You're mid-way through a feature when an urgent bug is reported on `main`. You need to switch branches but don't want to commit unfinished work.
```bash
git stash push -m "half-done login feature"
git checkout main
# ... fix the bug ...
git commit -am "hotfix: fix critical payment bug"
git push
git checkout feature/login
git stash pop
# Your half-done work is restored — continue where you left off
```

---

### `git log` — View Commit History

```bash
# Compact one-line view
git log --oneline

# Show graph with branches
git log --oneline --graph --all

# Show last 5 commits
git log -5

# Show commits by a specific author
git log --author="timothygey"

# Show commits that changed a specific file
git log -- src/main.py

# Show commits with diff
git log -p
```

**Scenario:** You want to find which commit introduced a specific change to a file.
```bash
git log --oneline -- src/payment.py
# Lists all commits that touched payment.py
# Find the commit hash, then:
git show a1b2c3d
# Shows the full diff of that commit
```

---

### `git diff` — View Changes

```bash
# Show unstaged changes
git diff

# Show staged changes (ready to commit)
git diff --staged

# Compare two branches
git diff main..feature/login

# Compare a specific file
git diff src/main.py

# Show only file names that changed
git diff --name-only main..feature/login
```

**Scenario:** Before committing, you want to review exactly what you've staged.
```bash
git diff --staged
# Shows line-by-line changes that will be included in the next commit
```

---

### `git cherry-pick` — Copy a Commit to Another Branch

Applies a specific commit from one branch to another.

```bash
git cherry-pick <commit-hash>
```

**Scenario:** A bug fix was committed on `feature/api` but needs to be applied to `main` immediately, without merging the entire feature branch.
```bash
git checkout main
git cherry-pick f3a7b2c
# The specific bug fix commit is now applied to main
git push
```

---

### `git bisect` — Find the Commit That Introduced a Bug

Binary search through commits to find which one broke something.

```bash
git bisect start
git bisect bad                  # Current commit is broken
git bisect good v1.0            # This older commit was working
# Git checks out a middle commit — test it, then:
git bisect good                 # or git bisect bad
# Repeat until Git identifies the exact commit
git bisect reset                # Exit bisect mode
```

**Scenario:** Something broke between last week's release and today. There are 200 commits in between and you need to find the exact one.
```bash
git bisect start
git bisect bad HEAD
git bisect good abc1234         # last known good commit
# Git checks out the middle commit — run your tests
# Mark as good or bad — Git narrows it down
# In ~7-8 steps (log2 of 200), you find the exact breaking commit
git bisect reset
```

---

### Quick Command Cheat Sheet

| Command | Purpose |
|---|---|
| `git add .` | Stage all changes |
| `git add -p` | Stage interactively (select hunks) |
| `git commit -m "msg"` | Commit with message |
| `git commit --amend` | Edit last commit |
| `git push` | Push to remote |
| `git push --force-with-lease` | Safe force push |
| `git pull` | Pull and merge |
| `git pull --rebase` | Pull and rebase |
| `git fetch` | Download without merging |
| `git checkout -b branch` | Create and switch branch |
| `git checkout -- file` | Discard file changes |
| `git reset HEAD~1` | Undo last commit (keep changes) |
| `git reset --hard HEAD~3` | Undo last 3 commits (discard changes) |
| `git revert <hash>` | Safely undo a published commit |
| `git rebase main` | Rebase onto main |
| `git rebase -i HEAD~4` | Interactive rebase last 4 commits |
| `git rebase origin/main` | Rebase onto remote main |
| `git stash` | Shelve uncommitted changes |
| `git stash pop` | Restore shelved changes |
| `git log --oneline` | Compact commit history |
| `git diff --staged` | View staged changes |
| `git cherry-pick <hash>` | Copy a commit to current branch |
| `git bisect start` | Binary search for a breaking commit |

---

## Daily Workflow Quick Reference

### Typical Daily Workflow

```
1. Pull latest changes
   └── git pull

2. Make your code changes
   └── (edit files in VS Code)

3. Stage changes
   └── git add .

4. Commit
   └── git commit -m "describe what you changed"

5. Push
   └── git push
```

### Branching Workflow (For Larger Projects)

```
1. Create a feature branch
   └── git checkout -b feature/my-feature

2. Make changes and commit
   └── git add . && git commit -m "add feature"

3. Push the branch
   └── git push -u origin feature/my-feature

4. Create a Pull Request on GitHub

5. After review, merge on GitHub

6. Switch back to main and pull
   └── git checkout main && git pull

7. Delete the merged branch locally
   └── git branch -d feature/my-feature
```
