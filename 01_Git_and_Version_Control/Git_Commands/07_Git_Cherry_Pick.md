# 🌿 Git Cherry-Pick 

**Git cherry-pick** is used to **apply a specific commit (or commits) from one branch into another branch**—without merging the entire branch.

Think of it as:

> “I like *this one commit* from that branch. Bring only this.”

---

## 📌 Basic Syntax

```bash
git cherry-pick <commit-hash>
```

---

## 🧠 When to Use Cherry-Pick

* Hotfix needs to go to `main` but was done in `dev`
* You want **only one feature commit**, not all branch changes
* Backporting a bug fix to older releases
* Clean, selective history control (very useful in DevOps workflows)

---

## 🧩 Simple Example

### Scenario

You have two branches:

* `main`
* `feature/login`

Commit history:

```text
feature/login:
A -- B -- C   (C = "Fix login API bug")
```

You want **only commit C** in `main`.

---

### Step 1: Switch to target branch (`main`)

```bash
git checkout main
```

---

### Step 2: Find commit hash

```bash
git log feature/login
```

Example output:

```text
commit c3a1f9e
Author: Dheeraj Kumar
Message: Fix login API bug
```

---

### Step 3: Cherry-pick the commit

```bash
git cherry-pick c3a1f9e
```

✅ Now commit `C` is applied to `main`.

---

## 🔁 Cherry-Pick Multiple Commits

### Pick multiple commits

```bash
git cherry-pick <hash1> <hash2>
```

---

### Pick a range of commits

```bash
git cherry-pick <start-hash>..<end-hash>
```

Example:

```bash
git cherry-pick a1b2c3..d4e5f6
```

👉 Picks all commits **after `a1b2c3` up to `d4e5f6`**

---

## ⚔️ Cherry-Pick with Conflict

If conflicts occur:

```bash
git status
```

Resolve conflicts → then:

```bash
git add .
git cherry-pick --continue
```

---

## ❌ Abort Cherry-Pick

If things go wrong:

```bash
git cherry-pick --abort
```

---

## ✏️ Edit Commit Message While Cherry-Picking

```bash
git cherry-pick -e <commit-hash>
```

---

## 🚫 Skip a Commit During Cherry-Pick (after conflict)

```bash
git cherry-pick --skip
```

---

## 🧪 Cherry-Pick Without Auto Commit

```bash
git cherry-pick -n <commit-hash>
```

(`-n` or `--no-commit`)

✔️ Applies changes but lets you review before committing.

---

## 🔄 Real DevOps Use Case (Common in Infosys-style workflows)

* Bug fixed in `dev`
* UAT running on `release`
* Production on `main`

👉 Instead of merging `dev` → `main`:

```bash
git checkout main
git cherry-pick <bugfix-commit>
```

Clean, safe, controlled 🔒

---

## ⚠️ Important Notes

* Cherry-pick **creates a new commit hash**
* Avoid cherry-picking **already merged commits** (can cause duplicates)
* Prefer merge/rebase for long-term branch sync

---

## 🧠 Cherry-Pick vs Merge

| Feature                | Cherry-Pick | Merge |
| ---------------------- | ----------- | ----- |
| Selective commits      | ✅ Yes       | ❌ No  |
| Preserves full history | ❌           | ✅     |
| New commit hash        | ✅           | ❌     |
| Best for hotfix        | ✅           | ❌     |

---

