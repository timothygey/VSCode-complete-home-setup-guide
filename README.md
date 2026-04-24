# VS Code Complete Home Setup Guide

A collection of comprehensive, copy-paste-ready markdown guides for setting up a complete Windows development environment from scratch — covering VS Code configuration, Git/SSH, project scaffolding, and virtual environments.

> **Target OS:** Windows 10/11  
> **Author:** timothygey | Last Updated: April 2026

---

## 📚 Guides in this Repo

### 1. [`vscode-complete-setup-guide.md`](vscode-complete-setup-guide.md)
**VS Code Installation, Extensions & Configuration**

Everything needed to get VS Code fully configured on a clean Windows machine:

- Clean uninstall of VS Code (app, user data, registry, PATH)
- Roo Code AI assistant — installation and API provider configuration
- Obtaining API keys — Anthropic (Claude), OpenAI, Google Gemini, OpenRouter, and free local alternatives (Ollama, LM Studio)
- MCP Servers — what they are and when you need them
- Python language servers — Pylance vs Pyright explained
- Python 3 & Pylance — full installation walkthrough
- Upgrading Python on Windows — fixing PATH, updating pip, cleaning old versions
- clangd extension — C/C++ language server, supported languages, compilation database setup
- Rainbow CSV extension — CSV/TSV column highlighting and RBQL queries
- Remote - SSH extension — connecting to remote machines, SSH key auth, multi-host config
- GitHub in VS Code — clone, commit, push, pull requests, authentication

---

### 2. [`git-setup-guide.md`](git-setup-guide.md)
**Git Identity, SSH Authentication & Daily Workflow**

One-time Git configuration and SSH setup to authenticate with GitHub:

- Configuring Git identity (name, email) to match your GitHub account
- Setting the default branch name to `main`
- Configuring line endings for Windows (`core.autocrlf`)
- SSH key generation (`ed25519`) and linking to GitHub
- Verifying SSH authentication
- Daily Git workflow reference (add → commit → push → pull)
- Common Git commands cheat sheet (branching, merging, stashing, history)

---

### 3. [`vscode-github-project-guide.md`](vscode-github-project-guide.md)
**Creating Projects & Pushing to GitHub**

Step-by-step workflow for starting any new project in VS Code and getting it onto GitHub:

- Creating a new empty repository on GitHub
- Setting up `.gitignore` (with templates for general, C++, and Python projects)
- Initialising Git locally and linking to the remote
- First commit and push
- **Part 1 — General project setup** (any language/framework)
- **Part 2 — C++ project setup** (folder structure, `tasks.json`, `launch.json`)
- **Part 3 — Python project setup** (virtual environment, interpreter selection)
- Full Git version control commands reference
- Daily workflow quick-reference diagram

---

### 4. [`virtual-environment-guide.md`](virtual-environment-guide.md)
**Virtual Environments Across Languages & Platforms**

Comprehensive guide to isolating project dependencies so they never conflict:

- Quick-start setup for Python and Node.js (< 2 minutes)
- **Part 1 — Python virtual environments** (`venv`) — create, activate, freeze, reproduce
- **Part 2 — Node.js environment isolation** (`npm`, `nvm`, `.nvmrc`)
- **Part 3 — Other languages & platforms** (Ruby `bundler`, Java Maven/Gradle, Rust `cargo`, Docker)
- **Part 4 — Daily workflow** — resuming work in VS Code with an active environment
- **Part 5 — Troubleshooting** — missing dependencies, broken environments, interpreter mismatches
- **Part 6 — What NEVER to do** in a virtual environment
- **Part 7 — Q&A** — installing, upgrading, and managing packages

---

## 🗺️ Recommended Reading Order

If you're setting up a new machine from scratch, follow the guides in this order:

```
1. vscode-complete-setup-guide.md   ← Install & configure VS Code + extensions
        ↓
2. git-setup-guide.md               ← Configure Git identity & SSH authentication
        ↓
3. vscode-github-project-guide.md   ← Create your first project and push to GitHub
        ↓
4. virtual-environment-guide.md     ← Isolate dependencies for Python / Node.js projects
```

All commands and code snippets throughout the guides are copy-paste ready.
