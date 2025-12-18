
# **What is Git Stash?**

`git stash` is a Git command that temporarily saves (or "stashes") your **uncommitted changes** in your working directory so you can work on something else, and then reapply those changes later.

* **Use case:**
  You’re working on a feature but suddenly need to switch to another branch to fix a bug. You don’t want to commit your incomplete work, so you stash it.

---

## **Basic Git Stash Commands**

### 1. **Stash your changes**

```bash
git stash
```

* This will stash **all tracked file changes** (both staged and unstaged).
* Your working directory will be clean (like after a commit).

> **Optional:** You can add a message to remember what you stashed:

```bash
git stash save "WIP: feature login page"
```

---

### 2. **View stashes**

```bash
git stash list
```

* Shows all stashed changes.
* Example output:

```
stash@{0}: WIP on main: 5f1a2b3 added login form
stash@{1}: WIP on main: 3c4d5e6 updated header
```

---

### 3. **Apply a stash**

```bash
git stash apply
```

* Reapplies the **latest stash** to your current branch without removing it from the stash list.
* To apply a specific stash:

```bash
git stash apply stash@{1}
```

---

### 4. **Pop a stash**

```bash
git stash pop
```

* Reapplies the latest stash **and removes it** from the stash list.
* Basically `apply + drop`.

---

### 5. **Drop a stash**

```bash
git stash drop stash@{0}
```

* Deletes a specific stash.

---

### 6. **Clear all stashes**

```bash
git stash clear
```

* Deletes all stashes.

---

### 7. **Stash untracked files**

By default, `git stash` only stashes tracked files. To include untracked files:

```bash
git stash -u
```

Or with a message:

```bash
git stash save -u "Stash including untracked files"
```

---

## **Example Scenario**

1. You’re on branch `main` and editing `app.js`.

```bash
git status
# Changes not staged for commit:
#   modified: app.js
```

2. Suddenly, a bug in `index.html` needs fixing on another branch. You stash your changes:

```bash
git stash save "WIP: app.js edits"
```

3. Switch to the bugfix branch:

```bash
git checkout bugfix
# Fix bug, commit changes
git commit -am "Fixed bug in index.html"
```

4. Go back to your work:

```bash
git checkout main
git stash list
# stash@{0}: WIP: app.js edits
```

5. Reapply your stashed changes:

```bash
git stash pop
```

* Your `app.js` changes are back and the stash is removed.

---

✅ **Key Points:**

* `stash` is temporary storage for **uncommitted changes**.
* `stash pop` removes it from stash, `stash apply` keeps it.
* Can stash untracked files with `-u` or `--include-untracked`.

---

