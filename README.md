# Placement_Prep_2_Git
# Git & GitHub Notes — From Basics to Advanced 🌱

A structured set of notes on Git and GitHub — version control fundamentals, everyday commands, branching, and the GitHub-specific workflow. Written while learning, organized here so others can learn too.

## 📑 Table of Contents

- [Git vs GitHub](#git-vs-github)
- [Repository Basics](#repository-basics)
- [Basic Git Commands](#basic-git-commands)
- [The Three Areas of Git](#the-three-areas-of-git)
- [Viewing History & Changes](#viewing-history--changes)
- [Undoing Changes](#undoing-changes)
- [Git Branches](#git-branches)
- [Merging & Rebasing](#merging--rebasing)
- [Working with Remotes](#working-with-remotes)
- [Stashing](#stashing)
- [.gitignore](#gitignore)
- [Tags](#tags)
- [GitHub-Specific Concepts](#github-specific-concepts)
- [SSH vs HTTPS](#ssh-vs-https)
- [Good Commit Practices](#good-commit-practices)
- [Contributing](#contributing)

---

## Git vs GitHub

| | Git | GitHub |
|---|---|---|
| What it is | A **version control system** — a tool that helps track changes in code | A **website/platform** that lets developers store and manage their code using Git |
| Runs where | Locally, on your machine | In the cloud (remote) |
| Folder concept | Repository (repo) — a project tracked by Git | A repository hosted online, with extra features (issues, PRs, actions, etc.) |

> 💡 Other Git hosting platforms exist too — GitLab, Bitbucket — they all build on top of Git the same way GitHub does.

---

## Repository Basics

A **repository (repo)** is simply a folder whose changes are tracked by Git. On GitHub, "folder" = **repository**.

| Command | Purpose |
|---|---|
| `git init` | Used to create a new Git repo in the current folder |
| `git clone <url>` | Cloning a repo onto our local machine (downloads a full copy, including history) |
| `git status` | Displays the status of the code — what's staged, unstaged, or untracked |
| `ls -a` | Displays all the hidden files (useful to confirm the `.git` folder exists) |

---

## Basic Git Commands

| Command | Purpose |
|---|---|
| `git add <file>` | Adds new or changed files in our working directory to the Git staging area (`git add .` stages everything) |
| `git commit -m "message"` | It is the record of change — saves a snapshot of the staged changes with a message |
| `git push` | Upload local repo content to a remote repo |
| `git pull` | Used to fetch and download content from a remote repo, and immediately update the local repo to match that content |
| `git fetch` | Downloads content from a remote repo **without** merging it into your local branch (safer than `pull` when you want to review first) |

---

## The Three Areas of Git

Understanding these three areas makes everything else click:

```
Working Directory  --git add-->  Staging Area  --git commit-->  Local Repository  --git push-->  Remote Repository
```

| Area | Description |
|---|---|
| **Working Directory** | Your actual files on disk, as you're editing them |
| **Staging Area (Index)** | Files marked as "ready to be committed" via `git add` |
| **Local Repository** | Your committed history, stored locally (`.git` folder) |
| **Remote Repository** | The hosted copy (e.g. on GitHub) that others can access |

---

## Viewing History & Changes

| Command | Purpose |
|---|---|
| `git log` | Shows the commit history (author, date, message, hash) |
| `git log --oneline` | Compact, one-line-per-commit view |
| `git diff` | Shows changes between working directory and staging area |
| `git diff --staged` | Shows changes between staging area and last commit |
| `git show <commit>` | Shows the details of a specific commit |
| `git blame <file>` | Shows who last modified each line of a file, and in which commit |

---

## Undoing Changes

| Command | Purpose |
|---|---|
| `git restore <file>` | Discards changes in the working directory (reverts to last committed version) |
| `git restore --staged <file>` | Unstages a file (moves it back from staging to working directory) |
| `git reset --soft <commit>` | Moves HEAD to a commit, keeps changes staged |
| `git reset --mixed <commit>` | Moves HEAD to a commit, keeps changes but unstages them (default mode) |
| `git reset --hard <commit>` | Moves HEAD to a commit and **discards all changes** after it — use with caution |
| `git revert <commit>` | Creates a **new** commit that undoes the changes of a previous commit — safe for shared/public history |

> 💡 **Reset vs Revert**: `reset` rewrites history (don't use on commits already pushed/shared), `revert` adds a new commit on top — always safe to use on shared branches.

---

## Git Branches

A **branch** is an independent line of development, letting you work on features without affecting the main codebase.

| Command | Purpose |
|---|---|
| `git branch` | Lists all local branches |
| `git branch <name>` | Creates a new branch |
| `git checkout <name>` | Switches to an existing branch |
| `git checkout -b <name>` | Creates a new branch **and** switches to it in one step |
| `git switch <name>` | Modern alternative to `checkout` for switching branches |
| `git branch -d <name>` | Deletes a branch (only if fully merged) |
| `git branch -D <name>` | Force-deletes a branch (even if not merged) |

---

## Merging & Rebasing

| Command | Purpose |
|---|---|
| `git merge <branch>` | Combines the changes from `<branch>` into your current branch |
| `git rebase <branch>` | Re-applies your commits on top of `<branch>`, creating a linear history |

**Merge vs Rebase:**
- `merge` preserves the full branch history with a merge commit — safer for shared branches.
- `rebase` creates a cleaner, linear history — but rewrites commit hashes, so avoid rebasing commits that are already pushed and shared with others.

**Merge conflicts** happen when Git can't automatically combine changes (e.g. the same line was edited differently in two branches). Git marks the conflicting section in the file with `<<<<<<<`, `=======`, `>>>>>>>` — you manually resolve it, then `git add` the file and continue the merge/rebase.

---

## Working with Remotes

| Command | Purpose |
|---|---|
| `git remote -v` | Lists the remote repositories connected to your local repo |
| `git remote add origin <url>` | Connects a local repo to a remote repo, named `origin` |
| `git push -u origin <branch>` | Pushes a branch and sets it to track the remote branch (so future `git push` alone works) |
| `git push origin --delete <branch>` | Deletes a branch on the remote |

---

## Stashing

| Command | Purpose |
|---|---|
| `git stash` | Temporarily saves uncommitted changes so you can switch branches without committing half-done work |
| `git stash pop` | Reapplies the most recent stash and removes it from the stash list |
| `git stash list` | Shows all stashed changes |
| `git stash drop` | Deletes a specific stash without applying it |

---

## .gitignore

A `.gitignore` file tells Git which files/folders to **never track** — e.g. `node_modules/`, `.env`, `*.log`, build outputs. Prevents secrets, dependencies, and junk files from being committed.

```gitignore
# Example .gitignore
node_modules/
.env
*.log
dist/
__pycache__/
```

---

## Tags

Tags mark specific points in history — typically used for release versions (e.g. `v1.0.0`).

| Command | Purpose |
|---|---|
| `git tag <name>` | Creates a lightweight tag at the current commit |
| `git tag -a <name> -m "message"` | Creates an annotated tag (with message, author, date — recommended for releases) |
| `git push origin <tag>` | Pushes a tag to the remote |
| `git push origin --tags` | Pushes all tags to the remote |

---

## GitHub-Specific Concepts

| Concept | Description |
|---|---|
| **Fork** | Creates your own copy of someone else's repository under your account, so you can freely experiment and later propose changes |
| **Pull Request (PR)** | A request to merge your changes (usually from a fork or branch) into another branch/repo — the core of collaborative review on GitHub |
| **Issues** | GitHub's built-in tracker for bugs, feature requests, and tasks |
| **GitHub Actions** | Built-in CI/CD — automate builds, tests, and deployments using YAML workflows |
| **GitHub Pages** | Free static site hosting directly from a repository |
| **README.md** | The landing page of a repo — shown automatically on the repo's main page |
| **Releases** | Packaged, versioned snapshots of your project, usually tied to a Git tag |
| **Codeowners** | A file (`CODEOWNERS`) that auto-assigns reviewers to PRs based on which files changed |

---

## SSH vs HTTPS

GitHub repos can be cloned/pushed using either:

| | SSH | HTTPS |
|---|---|---|
| URL format | `git@github.com:user/repo.git` | `https://github.com/user/repo.git` |
| Auth | SSH key pair (no password needed after setup) | Username + Personal Access Token (PAT) |
| Best for | Frequent pushing, personal machines | Quick one-off clones, CI environments |

Set up SSH once with `ssh-keygen`, add the public key to GitHub under **Settings → SSH and GPG keys**, and you won't need to authenticate on every push.

---

## Good Commit Practices

- Write clear, present-tense commit messages: `"Fix login bug"` not `"fixed stuff"`.
- Keep commits **small and focused** — one logical change per commit.
- Use `git commit -m "type: short summary"` following [Conventional Commits](https://www.conventionalcommits.org/) (e.g. `feat:`, `fix:`, `docs:`, `refactor:`) for cleaner, machine-readable history.
- Never commit secrets, API keys, or `.env` files — use `.gitignore` from the start.
- Pull before you push, to avoid unnecessary conflicts.

---

## Contributing

Found an error or want to add something that's missing? PRs and issues are welcome — this is meant to be a living reference that grows as more people learn from it.

## License

Feel free to use, share, and adapt these notes for learning purposes.
