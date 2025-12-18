# Git Reset 

`git reset` is used to **move HEAD (current branch pointer)** and optionally **undo changes** in the staging area (index) and working directory.

It is one of the most important (and dangerous 😄) Git commands.

---

## Basic Syntax

```bash
git reset [OPTION] <commit>
```

If `<commit>` is not provided, Git assumes `HEAD`.

---

## Three Core Reset Modes (Most Important)

These define **how much Git resets**:

| Mode                | HEAD | Index (Staging) | Working Directory |
| ------------------- | ---- | --------------- | ----------------- |
| `--soft`            | ✔    | ❌               | ❌                 |
| `--mixed` (default) | ✔    | ✔               | ❌                 |
| `--hard`            | ✔    | ✔               | ✔                 |

---

## 1️⃣ `git reset --soft`

### What it does

* Moves `HEAD` to a previous commit
* **Keeps changes staged**
* Code remains intact

### Use case

> You want to **rewrite commit history** but keep your changes ready to recommit.

### Example

```bash
git reset --soft HEAD~1
```

**Result**

* Last commit is removed
* Changes are still in **staging area**

```bash
git status
# Changes to be committed
```

✔ Common use: Fix commit message or squash commits.

---

## 2️⃣ `git reset --mixed` (DEFAULT)

### What it does

* Moves `HEAD`
* **Unstages files**
* Keeps changes in working directory

### Use case

> You committed too early and want to re-stage files differently.

### Example

```bash
git reset --mixed HEAD~1
```

or simply:

```bash
git reset HEAD~1
```

**Result**

* Commit removed
* Files are **unstaged**
* Code still exists

```bash
git status
# Changes not staged for commit
```

---

## 3️⃣ `git reset --hard` ⚠️ (Dangerous)

### What it does

* Moves `HEAD`
* Clears staging area
* **Deletes all local changes**

### Use case

> You want to completely discard changes.

### Example

```bash
git reset --hard HEAD~1
```

**Result**

* Commit removed
* All code changes gone forever

🚨 **Warning:** Cannot be recovered easily.

---

## Other Important Flags of `git reset`

---

## 4️⃣ `git reset <file>` (Unstage a file)

### What it does

* Removes file from staging area
* Keeps changes in working directory

### Example

```bash
git add app.js
git reset app.js
```

✔ Same as:

```bash
git restore --staged app.js
```

---

## 5️⃣ `git reset --hard <commit>`

### Move branch to a specific commit

```bash
git reset --hard abc123
```

Use when:

* You want to roll back to a **stable commit**

---

## 6️⃣ `git reset --keep`

### What it does

* Resets commits
* **Preserves uncommitted changes**
* Fails if conflicts exist

### Example

```bash
git reset --keep HEAD~1
```

✔ Safer than `--hard`

---

## 7️⃣ `git reset --merge`

### What it does

* Aborts a failed merge
* Keeps local changes

### Example

```bash
git reset --merge
```

Use when:

* Merge conflict happened
* You want to cancel merge safely

---

## 8️⃣ `git reset --patch` (Interactive Reset)

### What it does

* Lets you choose **hunks** to reset

### Example

```bash
git reset --patch
```

Git will ask:

```text
Unstage this hunk? [y,n,q,a,d]
```

✔ Useful for partial resets.

---

## Reset Using Commit References

| Reference     | Meaning            |
| ------------- | ------------------ |
| `HEAD`        | Current commit     |
| `HEAD~1`      | One commit before  |
| `HEAD~2`      | Two commits before |
| `commit_hash` | Specific commit    |

---

## Visual Example

### Commit History

```text
A — B — C — D (HEAD)
```

### After:

```bash
git reset --soft B
```

```text
A — B (HEAD)
```

Changes from `C` and `D` are **staged**.

---

## When NOT to Use `git reset`

❌ On shared branches (`main`, `develop`)
✔ Use `git revert` instead

---

## Reset vs Revert (Quick Comparison)

| Command      | History   | Safe for Remote |
| ------------ | --------- | --------------- |
| `git reset`  | Rewrites  | ❌               |
| `git revert` | Preserves | ✔               |

---

## Real-World DevOps Scenario (Quick)

> Accidentally committed secrets:

```bash
git reset --soft HEAD~1
git rm --cached secrets.env
git commit -m "Remove secrets"
```

---

## Summary Cheat Sheet

```bash
git reset --soft HEAD~1    # undo commit, keep staged
git reset --mixed HEAD~1   # undo commit, unstage
git reset --hard HEAD~1    # destroy everything
git reset file.txt         # unstage file
git reset --patch          # interactive
```

---

