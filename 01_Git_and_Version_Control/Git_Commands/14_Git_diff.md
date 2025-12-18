

# **What is `git diff`?**

`git diff` is a Git command used to **show differences** between files, commits, branches, or the staging area.
It helps you see **what has changed** before committing or merging code.

Think of it as a way to "preview" changes in your code.

---

## **1. Show unstaged changes**

This shows the difference between **your working directory** and the **last commit** (HEAD).

```bash
git diff
```

**Example:**

Suppose you have a file `app.py`:

```python
# app.py
print("Hello World")
```

You change it to:

```python
print("Hello Git Diff")
```

Now run:

```bash
git diff
```

**Output:**

```diff
diff --git a/app.py b/app.py
index e69de29..f0a1c2b 100644
--- a/app.py
+++ b/app.py
@@ -1 +1 @@
-print("Hello World")
+print("Hello Git Diff")
```

✅ **Explanation:**

* `-` = line removed
* `+` = line added

---

## **2. Show staged changes**

If you **added changes to the staging area** (`git add`), use:

```bash
git diff --cached
```

**Example:**

```bash
git add app.py
git diff --cached
```

This will show **changes staged for commit**.

---

## **3. Compare two commits**

You can also compare changes between commits:

```bash
git diff <commit1> <commit2>
```

**Example:**

```bash
git diff abc123 def456
```

This shows the difference between two commits (`abc123` and `def456`).

---

## **4. Compare branches**

You can see what changes exist **between branches**:

```bash
git diff main feature-branch
```

This shows what changes exist in `feature-branch` compared to `main`.

---

## **5. Compare specific files**

```bash
git diff HEAD -- app.py
```

This shows the changes in `app.py` since the last commit.

---

## **6. Useful flags**

| Flag                    | Purpose                                       |
| ----------------------- | --------------------------------------------- |
| `--staged` / `--cached` | Show staged changes                           |
| `--name-only`           | Show only file names changed                  |
| `--stat`                | Show summary of changes (lines added/removed) |
| `--color-words`         | Show changes word by word                     |

**Example:**

```bash
git diff --stat
```

Output:

```
 app.py | 1 +
 1 file changed, 1 insertion(+)
```

---

### **Summary**

* `git diff` → unstaged changes
* `git diff --cached` → staged changes
* `git diff <commit1> <commit2>` → changes between commits
* `git diff branch1 branch2` → changes between branches
* `git diff --name-only` → only file names changed
* `git diff --stat` → summary of changes

---
