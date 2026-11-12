# Module 5: Git & GitHub — Distributed Version Control

**Git** is a free, open-source distributed version control system (DVCS) designed to track changes in source code across time and facilitate non-linear, multi-developer collaboration. **GitHub** is the world's leading cloud hosting platform for Git repositories, providing continuous integration, code review tools, issue tracking, and collaboration workflows.

---

## 📑 Table of Contents

1. [Introduction to Version Control](#1-introduction-to-version-control)
2. [Git Architecture: The Three Areas](#2-git-architecture-the-three-areas)
3. [First-Time Environment Setup](#3-first-time-environment-setup)
4. [The Fundamental Git Workflow](#4-the-fundamental-git-workflow)
5. [Branching and Merging Strategies](#5-branching-and-merging-strategies)
6. [Resolving Merge Conflicts](#6-resolving-merge-conflicts)
7. [Working with GitHub Remotes](#7-working-with-github-remotes)
8. [Undoing Mistakes & Time Travel](#8-undoing-mistakes--time-travel)
9. [The `.gitignore` File](#9-the-gitignore-file)
10. [GitHub Platform Essentials](#10-github-platform-essentials)
11. [Master Git Cheatsheet](#11-master-git-cheatsheet)

---

## 1. Introduction to Version Control

### Centralized vs. Distributed VCS:
- **Centralized VCS (e.g., SVN):** A single central server holds the entire version history. If the server goes offline, developers cannot commit or review history.
- **Distributed VCS (Git):** Every developer's local machine contains a complete, self-contained clone of the entire repository history. Commits, branch switching, and diffs run instantly offline.

```text
Centralized VCS (SVN)                  Distributed VCS (Git)

       [Server]                         [Remote: GitHub]
       /   |   \                            ^     ^
      /    |    \                           |     |
    [Dev1][Dev2][Dev3]                  [Dev1 Repo] <--> [Dev2 Repo]
  (Working copies only)               (Full local histories on each)
```

---

## 2. Git Architecture: The Three Areas

Git manages your files through three distinct local states before syncing to a remote server:

```text
+---------------------+       git add       +------------------+       git commit       +-------------------+
|  Working Directory  | ------------------> |  Staging Area    | ---------------------> | Local Repository  |
|  (Untracked/Edited) |                     |  (Index / Cache) |                        | (HEAD / Commits)  |
+---------------------+ <------------------ +------------------+ <--------------------- +-------------------+
                            git restore           git restore                             git reset / checkout
                             --staged
                                                         |
                                                         | git push
                                                         v
                                                +-------------------+
                                                | Remote (GitHub)   |
                                                +-------------------+
```

1. **Working Directory:** The sandbox where you edit, create, and delete files on your file system.
2. **Staging Area (Index):** A staging buffer that lets you assemble exact snapshots before committing.
3. **Local Repository (`.git/`):** The permanent history containing immutable commit snapshots identified by SHA-1/SHA-256 hashes.
4. **Remote Repository (GitHub):** The shared cloud replica for collaboration and backups.

---

## 3. First-Time Environment Setup

Before making your first commit, identify yourself to Git. This metadata is permanently stamped into every commit:

```bash
# 1. Set global user identity:
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 2. Set default branch to 'main':
git config --global init.defaultBranch main

# 3. Configure line-ending handling:
# On Windows:
git config --global core.autocrlf true
# On macOS / Linux:
git config --global core.autocrlf input

# 4. Verify configuration:
git config --list
```

---

## 4. The Fundamental Git Workflow

### Step 1: Initialize or Clone a Repository
```bash
# Initialize a new repository in current directory:
git init

# OR clone an existing GitHub repository:
git clone https://github.com/username/repository.git
```

### Step 2: Check Repository Status
```bash
git status
# Short concise status:
git status -s
```

### Step 3: Stage Changes
```bash
# Stage a specific file:
git add README.md

# Stage all modified and new files:
git add .

# Stage interactively chunk-by-chunk:
git add -p
```

### Step 4: Commit with a Clean Message
```bash
git commit -m "feat: add comprehensive LaTeX mathematical documentation"
```

### Step 5: View History Logs
```bash
# Detailed log:
git log

# Clean, single-line log with visual graph branches:
git log --oneline --graph --decorate --all
```

---

## 5. Branching and Merging Strategies

Branches allow you to develop features, test experiments, or fix bugs in isolation without destabilizing the production `main` branch.

```text
          (experiment-branch)  C3 --- C4
                             /          \  (git merge)
main:  C0 --------- C1 ----- C2 -------- C5 (merge commit)
```

### Branch Management Commands:
```bash
# List all local branches (* marks active branch):
git branch

# Create a new branch:
git branch feature/latex-tables

# Switch to the branch (modern syntax):
git switch feature/latex-tables
# (Legacy syntax: git checkout feature/latex-tables)

# Create and switch in a single command:
git switch -c feature/beamer-slides
# (Legacy syntax: git checkout -b feature/beamer-slides)

# Merge branch into current branch:
git switch main
git merge feature/beamer-slides

# Delete branch once merged:
git branch -d feature/beamer-slides
```

---

## 6. Resolving Merge Conflicts

A merge conflict occurs when two branches modify the **same lines of the same file** in different ways, and Git cannot automatically decide which version to keep.

### Conflict Markers in File:
When Git halts on a conflict, it marks the disputed lines directly in the file:

```text
<<<<<<< HEAD (Current branch: main)
\title{Laboratory Assignment 2: Technical Tools}
=======
\title{Computer Science Tools & Portfolio}
>>>>>>> feature/title-update (Incoming branch)
```

### Resolution Steps:
1. Open the conflicted file in your editor.
2. Manually edit the file to select the desired code and remove all marker lines (`<<<<<<<`, `=======`, `>>>>>>>`).
3. Save the resolved file.
4. Stage the resolved file:
   ```bash
   git add filename.tex
   ```
5. Complete the merge commit:
   ```bash
   git commit -m "fix: resolve title conflict between main and feature branch"
   ```

---

## 7. Working with GitHub Remotes

### 1. Linking a Local Repository to GitHub:
```bash
# Add remote named 'origin':
git remote add origin https://github.com/username/repository-name.git

# Verify remote configuration:
git remote -v
```

### 2. Pushing Code:
```bash
# First push (sets upstream tracking '-u'):
git push -u origin main

# Subsequent pushes:
git push
```

### 3. Pulling & Fetching Updates:
```bash
# Download remote metadata without modifying your working files:
git fetch origin

# Fetch and merge changes from GitHub in one command:
git pull origin main
```

---

## 8. Undoing Mistakes & Time Travel

| Scenario | Command | Effect |
|----------|---------|--------|
| Discard uncommitted changes in a file | `git restore <file>` | Reverts file back to last commit |
| Unstage a file accidentally added | `git restore --staged <file>` | Removes from staging, preserves file edits |
| Modify the last commit message or files | `git commit --amend -m "new message"` | Rewrites the latest commit |
| Temporarily shelve uncommitted work | `git stash` | Cleans working tree into a temporary stash |
| Restore shelved work | `git stash pop` | Reapplies stashed changes |
| Undo a public commit safely | `git revert <commit-hash>` | Creates a new commit that reverses past changes |
| Undo local commits (keep file changes) | `git reset --soft HEAD~1` | Moves HEAD back, leaves changes in staging |
| Discard local commits & ALL changes | `git reset --hard HEAD~1` | ⚠️ Permanently wipes changes back to commit |

---

## 9. The `.gitignore` File

The `.gitignore` file specifies intentionally untracked files that Git should ignore (e.g., compiler caches, temporary build artifacts, sensitive API credentials).

### Sample `.gitignore` for this Project:
```gitignore
# LaTeX temporary compilation artifacts
*.aux
*.log
*.out
*.toc
*.bbl
*.blg
*.fls
*.fdb_latexmk
*.synctex.gz

# Beamer temporary files
*.nav
*.snm
*.vrb

# Doxygen generated output
docs/html/
docs/latex/
Doxyfile.bak

# OS specific files
.DS_Store
Thumbs.db
desktop.ini

# IDE & Editor files
.vscode/
.idea/
*.swp
```

---

## 10. GitHub Platform Essentials

- **Forking:** Creating a personal server-side copy of someone else's repository on your GitHub account.
- **Pull Requests (PR):** Proposing changes from a feature branch or fork back into the upstream repository for code review, discussion, and automated CI testing.
- **Issues:** Tracking bugs, user stories, task assignments, and discussions.
- **Linking PRs to Issues:** Include keywords like `Closes #14` or `Fixes #22` in your commit message or PR description to automatically close the issue upon merging.

---

## 11. Master Git Cheatsheet

```bash
# Setup
git config --global user.name "Name"
git config --global user.email "email"

# Creation
git init
git clone <url>

# Local Changes
git status
git diff
git add <file>
git add .
git commit -m "commit message"

# History
git log --oneline --graph --all
git show <commit-hash>

# Branches
git branch
git switch -c <new-branch>
git switch <branch>
git merge <branch>
git branch -d <branch>

# Remotes
git remote add origin <url>
git push -u origin <branch>
git pull
git fetch

# Undoing
git restore <file>
git restore --staged <file>
git revert <commit-hash>
git stash
git stash pop
```
