# Git Squash 

**Git Squash** means **combining multiple commits into a single commit**.
It’s mainly used to **clean up commit history** before merging a branch.

👉 Common in **Pull Requests** and **feature branches**.

---

## Why Squash Commits?

* Keep Git history **clean & readable**
* Combine “WIP”, “fix typo”, “minor change” commits
* Present one meaningful commit for a feature

---

## When Do We Use Git Squash?

* Before merging a feature branch
* Before raising a Pull Request
* During code review cleanup

---

## Example Scenario

You are working on a feature branch:

```bash
git checkout -b feature/login
```

You made **multiple commits**:

```bash
git log --oneline
```

```
a3f9c12 Fix validation bug
c7b2a44 Update UI styles
e1d4f88 Add login API
9b0a111 Initial login page
```

👉 You want **all 4 commits → 1 clean commit**

---

## Method 1: Squash Using Interactive Rebase (Most Common)

### Step 1: Start Interactive Rebase

If you want to squash the **last 4 commits**:

```bash
git rebase -i HEAD~4
```

---

### Step 2: Rebase Editor Opens

You’ll see something like:

```
pick 9b0a111 Initial login page
pick e1d4f88 Add login API
pick c7b2a44 Update UI styles
pick a3f9c12 Fix validation bug
```

---

### Step 3: Change `pick` to `squash` (or `s`)

Keep the **first commit as pick**, squash the rest:

```
pick 9b0a111 Initial login page
squash e1d4f88 Add login API
squash c7b2a44 Update UI styles
squash a3f9c12 Fix validation bug
```

Save & exit (`:wq` in vim).

---

### Step 4: Edit Commit Message

Git will open another editor:

```
# This is a combination of 4 commits.
# The first commit message is:
Initial login page

# The commit messages of the squashed commits are:
Add login API
Update UI styles
Fix validation bug
```

Edit to something clean:

```
Add login feature with UI and validation
```

Save & exit.

---

### Step 5: Verify

```bash
git log --oneline
```

Output:

```
f5c8e77 Add login feature with UI and validation
```

🎉 **4 commits → 1 commit**

---

## Method 2: Squash While Merging (Pull Request Style)

```bash
git checkout main
git merge --squash feature/login
git commit -m "Add login feature"
```

✔️ Branch history stays clean
❌ Commit history from feature branch is lost

---

## Method 3: Squash Using GitHub UI

When creating a **Pull Request**:

* Choose **“Squash and merge”**
* All commits → 1 commit in `main`

Very common in teams.

---

## Important Notes ⚠️

### Force Push Required (If Already Pushed)

If you squashed commits that were already pushed:

```bash
git push origin feature/login --force
```

🔴 **Be careful** — force push rewrites history.

---

## Difference: Squash vs Merge

| Feature          | Squash   | Merge                 |
| ---------------- | -------- | --------------------- |
| Commit History   | Clean    | Full                  |
| Multiple Commits | ❌        | ✅                     |
| Best For         | Features | Long-running branches |

---

## Quick Summary

* **Git Squash = Combine multiple commits into one**
* Done using:

  * `git rebase -i`
  * `git merge --squash`
  * GitHub PR option
* Best practice before merging feature branches

---

