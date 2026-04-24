# Git Setup Guide

> SSH authentication verified as **timothygey** ✅
> Guide created: 24 April 2026

---

## 1. Ensure Your Git Identity Matches GitHub

Check your current settings:

```bash
git config --global user.name
git config --global user.email
```

If these aren't set or don't match your GitHub account, set them:

```bash
git config --global user.name "Your Display Name"
git config --global user.email "your-github-email@example.com"
```

> **Tip:** Find your GitHub email at [https://github.com/settings/emails](https://github.com/settings/emails). If you have email privacy enabled, use the no-reply address GitHub provides (format: `12345678+username@users.noreply.github.com`).

---

## 2. Set Default Branch Name

```bash
git config --global init.defaultBranch main
```

> Already completed ✅

---

## 3. Set Line Endings (if on Windows)

```bash
git config --global core.autocrlf true
```

For Linux/macOS, use:

```bash
git config --global core.autocrlf input
```

---

## 4. Verify Everything

```bash
git config --global --list
```

You should see your name, email, default branch, and other settings listed.

---

## 5. You're Ready to Use Git!

### Clone a Repo (SSH)

```bash
git clone git@github.com:timothygey/your-repo.git
```

> Use the SSH URL format since you set up SSH.

### Create a New Repo Locally

```bash
mkdir my-project
cd my-project
git init
```

### Link a Local Project to GitHub

```bash
git remote add origin git@github.com:timothygey/your-repo.git
git push -u origin main
```

---

## Quick Reference — Daily Git Workflow

```
Edit Files  →  git add .  →  git commit -m "message"  →  git push  →  Code on GitHub
                                                                          ↓
Edit Files  ←                 git pull                  ←          GitHub Changes
```

### Common Commands

| Command | Description |
|---|---|
| `git add .` | Stage all changes |
| `git commit -m "your message"` | Commit with a message |
| `git push` | Push to GitHub |
| `git pull` | Pull latest from GitHub |
| `git status` | Check what's changed |
| `git log --oneline` | View commit history |
| `git branch` | List branches |
| `git checkout -b new-branch` | Create and switch to a new branch |
| `git merge branch-name` | Merge a branch into current branch |
| `git stash` | Temporarily save uncommitted changes |
| `git stash pop` | Restore stashed changes |

---

## SSH Setup Reference (Completed)

For future reference, these are the steps that were completed:

1. Generated SSH key:
   ```bash
   ssh-keygen -t ed25519 -C "your-email@example.com"
   ```

2. Added public key (`~/.ssh/id_ed25519.pub`) to [GitHub → Settings → SSH Keys](https://github.com/settings/keys)

3. Verified authentication:
   ```bash
   ssh -T git@github.com
   ```
   Result: `Hi timothygey! You've successfully authenticated, but GitHub does not provide shell access.`
