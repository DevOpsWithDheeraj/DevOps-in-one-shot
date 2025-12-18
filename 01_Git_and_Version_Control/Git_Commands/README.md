# Git & GitHub – Complete Practical Guide

This guide explains **GitHub and advanced Git concepts** step by step with **commands, flags, and real examples**. Suitable for **DevOps / Software Engineers**.

---

## 1. What is GitHub?

**GitHub** is a **cloud-based platform** that hosts **Git repositories** and provides collaboration features like:

* Remote code hosting
* Pull Requests (PRs)
* Code review
* Issue tracking
* CI/CD integration (GitHub Actions)

👉 **Git** = Version control system (local)
👉 **GitHub** = Remote hosting + collaboration

### Basic Workflow

```bash
git clone <repo>
git add .
git commit -m "message"
git push origin main
```

---

## 2. Git Branching

A **branch** is an independent line of development.

### Common Commands

```bash
git branch                # list branches
git branch feature-login  # create branch
git checkout feature-login
git checkout -b feature-ui  # create + switch
git branch -d feature-login # delete branch
git branch -D feature-login # force delete
```

### Why Branching?

* Parallel development
* Safe experimentation
* Clean main branch

---

## 3. Git Pull Request (PR)

A **Pull Request** is a request to **merge one branch into another** (mostly into `main`).

### PR Flow

1. Create feature branch
2. Push branch to GitHub
3. Open Pull Request
4. Code review
5. Merge

### PR Merge Options

* **Merge commit** – keeps history
* **Squash & merge** – clean history
* **Rebase & merge** – linear history

---

## 4. Git in Visual Studio Code

VS Code has **built-in Git support**.

### Features

* Source Control panel
* Stage / Commit UI
* Diff viewer
* Branch switching
* Conflict resolution UI

### Terminal Usage

```bash
git status
git pull
git push
```

---

## 5. Git Reset

Used to **undo commits**.

### Types of Reset

#### 1️⃣ Soft Reset

```bash
git reset --soft HEAD~1
```

* Removes commit
* Keeps changes staged

#### 2️⃣ Mixed Reset (default)

```bash
git reset HEAD~1
```

* Keeps changes unstaged

#### 3️⃣ Hard Reset ⚠️

```bash
git reset --hard HEAD~1
```

* Deletes commit + changes

---

## 6. Git Commit --amend

Modify the **last commit**.

```bash
git commit --amend -m "new message"
```

Add forgotten files:

```bash
git add file.txt
git commit --amend
```

⚠️ Avoid amending pushed commits.

---

## 7. Git Cherry-Pick

Apply **specific commit(s)** from another branch.

```bash
git cherry-pick <commit-hash>
```

Multiple commits:

```bash
git cherry-pick a1b2c3 d4e5f6
```

Abort:

```bash
git cherry-pick --abort
```

---

## 8. Git Squash

Combine multiple commits into one.

```bash
git rebase -i HEAD~3
```

Change:

```
pick a1 commit1
squash b2 commit2
squash c3 commit3
```

Result → one clean commit.

---

## 9. Git Reorder (Interactive Rebase)

Change commit order.

```bash
git rebase -i HEAD~4
```

Reorder lines manually.

---

## 10. Git Drop (Delete Commit)

Remove a commit from history.

```bash
git rebase -i HEAD~3
```

Change `pick` to `drop`.

---

## 11. Git Exec

Execute command during rebase.

```bash
git rebase -i --exec "npm test" HEAD~5
```

Used for testing during rebases.

---

## 12. Git Bisect

Find the **commit that introduced a bug**.

```bash
git bisect start
git bisect bad
git bisect good v1.0
```

Mark commits:

```bash
git bisect good
git bisect bad
```

Finish:

```bash
git bisect reset
```

---

## 13. Git Stash

Temporarily save work.

```bash
git stash
git stash list
git stash apply
git stash pop
git stash drop
```

With message:

```bash
git stash push -m "wip login"
```

---

## 14. Git Diff

Show changes.

```bash
git diff             # working vs staging
git diff --staged    # staged vs last commit
git diff HEAD~1
```

File-specific:

```bash
git diff file.txt
```

---

## 15. Git Log

View commit history.

```bash
git log
git log --oneline
git log --graph --all
git log -p
```

Limit commits:

```bash
git log -5
```

---

## 16. Git Ignore (.gitignore)

Ignore files from tracking.

Example `.gitignore`:

```
node_modules/
.env
*.log
```

Already tracked files:

```bash
git rm --cached file.txt
```

---

## 17. Git Merge Conflict

Occurs when Git cannot auto-merge.

### Conflict Markers

```
<<<<<<< HEAD
local code
=======
incoming code
>>>>>>> branch
```

### Resolve Steps

1. Edit file
2. Remove markers
3. Add file

```bash
git add file.txt
git commit
```

Abort merge:

```bash
git merge --abort
```

---

## 18. Summary Table

| Feature     | Purpose        |
| ----------- | -------------- |
| Branch      | Parallel work  |
| PR          | Review & merge |
| Reset       | Undo commits   |
| Rebase      | Clean history  |
| Stash       | Temporary save |
| Bisect      | Bug hunting    |
| Cherry-pick | Select commits |

---
