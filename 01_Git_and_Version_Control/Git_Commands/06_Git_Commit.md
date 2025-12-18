
# What is `git commit`?

`git commit` **records changes from the staging area into the Git repository history**.

📌 Think of it as:

> “Taking a snapshot of staged changes with a message explaining *why*.”

---

## Basic Syntax

```bash
git commit [options]
```

---

## 1️⃣ `git commit -m`

### Add commit message inline

```bash
git commit -m "Add login API validation"
```

✔ Most commonly used
✔ Skips opening editor

---

## 2️⃣ `git commit -a`

### Stage and commit tracked files automatically

```bash
git commit -a -m "Fix typo in config file"
```

📌 What it does:

* Stages **only modified tracked files**
* ❌ Does NOT include new untracked files

Equivalent to:

```bash
git add -u
git commit
```

---

## 3️⃣ `git commit -am`

### Combine `-a` and `-m`

```bash
git commit -am "Update nginx configuration"
```

✔ Very common in daily DevOps work
❌ Won’t include new files

---

## 4️⃣ `git commit --amend`

### Modify last commit

### Case 1: Change commit message

```bash
git commit --amend -m "Correct commit message"
```

### Case 2: Add missed files

```bash
git add missing_file.yaml
git commit --amend
```

📌 Use before pushing
⚠️ Dangerous after push (rewrites history)

---

## 5️⃣ `git commit --no-edit`

### Amend commit without changing message

```bash
git add new_file.txt
git commit --amend --no-edit
```

✔ Keeps previous commit message
✔ Adds new changes

---

## 6️⃣ `git commit -v`

### Show diff in commit editor

```bash
git commit -v
```

✔ Displays changes being committed
✔ Helpful for code review before commit

---

## 7️⃣ `git commit -s`

### Sign off commit (DCO)

```bash
git commit -s -m "Add CI pipeline"
```

Adds:

```text
Signed-off-by: Dheeraj Kumar <email@example.com>
```

✔ Required in many open-source projects

---

## 8️⃣ `git commit --allow-empty`

### Create empty commit

```bash
git commit --allow-empty -m "Trigger CI pipeline"
```

📌 Used when:

* Triggering CI/CD
* Marking milestones

---

## 9️⃣ `git commit --allow-empty-message`

### Commit with no message (not recommended)

```bash
git commit --allow-empty-message -m ""
```

⚠️ Avoid unless explicitly required

---

## 🔟 `git commit -C <commit>`

### Reuse commit message from another commit

```bash
git commit -C abc1234
```

✔ Copies message
✔ Does NOT open editor

---

## 1️⃣1️⃣ `git commit -c <commit>`

### Reuse and edit commit message

```bash
git commit -c abc1234
```

✔ Opens editor
✔ Good for similar commits

---

## 1️⃣2️⃣ `git commit --fixup <commit>`

### Create fixup commit (used with autosquash)

```bash
git commit --fixup abc1234
```

Creates message:

```text
fixup! Original commit message
```

Used with:

```bash
git rebase -i --autosquash
```

✔ Clean history before PR

---

## 1️⃣3️⃣ `git commit --squash <commit>`

### Squash commit later during rebase

```bash
git commit --squash abc1234
```

✔ Combines commits cleanly

---

## 1️⃣4️⃣ `git commit --dry-run`

### Preview commit without creating it

```bash
git commit --dry-run
```

✔ Shows what would be committed
✔ Safe check

---

## 1️⃣5️⃣ `git commit --pathspec-from-file`

### Commit files listed in a file

```bash
git commit --pathspec-from-file=files.txt
```

📌 Rare, advanced usage

---

## 1️⃣6️⃣ `git commit --only`

### Commit only specified paths

```bash
git commit --only src/app.py -m "Update app logic"
```

✔ Ignores other staged files

---

## 1️⃣7️⃣ `git commit --include`

### Add specified files & commit

```bash
git commit --include config.yaml -m "Update config"
```

✔ Adds + commits in one step

---

## 1️⃣8️⃣ `git commit --quiet`

### Suppress output

```bash
git commit --quiet -m "Silent commit"
```

✔ Useful in scripts

---

## 1️⃣9️⃣ `git commit --author`

### Commit as a different author

```bash
git commit --author="John Doe <john@example.com>" -m "Initial commit"
```

✔ Used when migrating code

---

## 2️⃣0️⃣ `git commit --date`

### Override commit date

```bash
git commit --date="2024-01-01 10:00" -m "Backdated commit"
```

---

## 🔥 Most Used in Real Projects

| Command              | Usage                    |
| -------------------- | ------------------------ |
| `git commit -m`      | Normal commit            |
| `git commit -am`     | Fast tracked file commit |
| `git commit --amend` | Fix last commit          |
| `git commit -s`      | Open-source              |
| `git commit --fixup` | Clean PR history         |

---

## DevOps Tip for You 👨‍💻

As a **DevOps Engineer**, you’ll mostly use:

```bash
git commit -am "Update Helm values"
git commit --amend
git commit --fixup
```

These keep **PR history clean and professional**.

---
