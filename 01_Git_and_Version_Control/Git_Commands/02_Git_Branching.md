# 🌿 Git Branching — Explained with Simple Examples

Git **branching** lets you create independent lines of development within the same repository.
Think of a branch as a **parallel universe** where you can work without disturbing the main code.

---

## 🔹 What is a Git Branch?

A **branch** is a lightweight pointer to a specific commit.

* Default branch: `main` (or `master`)
* You can create multiple branches for:

  * New features
  * Bug fixes
  * Experiments
  * Releases

---

## 🔹 Why Branching is Important (Real-Life Use)

As a **DevOps Engineer**, you’ll often:

* Develop features without breaking production
* Fix bugs quickly
* Work in teams without conflicts

Typical workflow:

```
main → feature → testing → merge back to main
```

---

## 🔹 Basic Branch Commands

### 1️⃣ Check current branch

```bash
git branch
```

Output:

```text
* main
```

`*` shows the active branch.

---

### 2️⃣ Create a new branch

```bash
git branch feature-login
```

This creates a branch but **does not switch** to it.

---

### 3️⃣ Switch to a branch

```bash
git checkout feature-login
```

👉 Modern command:

```bash
git switch feature-login
```

---

### 4️⃣ Create and switch in one command

```bash
git checkout -b feature-login
```

or

```bash
git switch -c feature-login
```

---

## 🔹 Example: Feature Development Workflow

### Scenario

You want to add a **login feature** without touching `main`.

#### Step 1: Create branch

```bash
git checkout -b feature-login
```

#### Step 2: Make changes

```bash
vim login.py
git add login.py
git commit -m "Add login feature"
```

Now your commit exists **only** in `feature-login`.

---

### Step 3: Switch back to main

```bash
git checkout main
```

Your login feature is **not visible** here.

---

## 🔹 Merging a Branch

### Merge feature branch into main

```bash
git checkout main
git merge feature-login
```

✔️ Result:

* Code from `feature-login` is added to `main`
* Branch history preserved

---

## 🔹 Branching Visualization

```
main:     A --- B --- E
                \
feature-login:    C --- D
```

After merge:

```
main: A --- B --- C --- D --- E
```

---

## 🔹 Delete a Branch

### Delete after merge

```bash
git branch -d feature-login
```

### Force delete (not merged)

```bash
git branch -D feature-login
```

---

## 🔹 Remote Branches

### Push branch to remote

```bash
git push origin feature-login
```

### List remote branches

```bash
git branch -r
```

### Delete remote branch

```bash
git push origin --delete feature-login
```

---

## 🔹 Common Branching Strategies

### 1️⃣ Feature Branching

* One branch per feature

```
main → feature-login → merge
```

### 2️⃣ Git Flow (Popular in Enterprises)

* `main` → production
* `develop` → integration
* `feature/*`
* `release/*`
* `hotfix/*`

### 3️⃣ Trunk-Based Development

* Short-lived branches
* Frequent merges to `main`

---

## 🔹 Best Practices

✔️ Keep branches small
✔️ Name branches clearly
✔️ Merge frequently
✔️ Delete unused branches
✔️ Never commit directly to `main` in teams

---

## 🔹 Quick Summary

| Command                | Purpose         |
| ---------------------- | --------------- |
| `git branch`           | List branches   |
| `git branch name`      | Create branch   |
| `git checkout -b name` | Create & switch |
| `git merge branch`     | Merge branch    |
| `git branch -d name`   | Delete branch   |

---
