
# **1. What is `git log`?**

`git log` is used to **view the history of commits** in a Git repository. It shows details such as:

* Commit hash (SHA-1)
* Author
* Date
* Commit message

It’s the primary way to inspect past changes.

---

## **2. Basic Usage**

```bash
git log
```

**Output example:**

```
commit 9fceb02b213765bde8a9d7f1a3e0f7e12f1d8f8e
Author: Dheeraj Kumar <dheeraj@example.com>
Date:   Thu Dec 19 10:15:20 2025 +0530

    Fixed bug in login API
```

Here:

* **commit**: The commit hash (unique ID)
* **Author**: Who made the commit
* **Date**: When the commit was made
* **Message**: Commit message describing changes

---

## **3. Useful Flags for `git log`**

### **a. `--oneline`**

Shows each commit in **one line**, which is compact.

```bash
git log --oneline
```

Example output:

```
9fceb02 Fixed bug in login API
a1b2c3d Added user validation
d4e5f6a Initial commit
```

---

### **b. `--graph`**

Shows a **graphical representation** of the branch history.

```bash
git log --graph --oneline
```

Example output:

```
* 9fceb02 Fixed bug in login API
* a1b2c3d Added user validation
* d4e5f6a Initial commit
```

---

### **c. `--decorate`**

Shows **branch and tag names** along with commits.

```bash
git log --oneline --decorate
```

Example:

```
9fceb02 (HEAD -> main, origin/main) Fixed bug in login API
a1b2c3d Added user validation
```

---

### **d. `-p`**

Shows the **patch/diff** introduced by each commit.

```bash
git log -p
```

This is useful to see what exactly changed in each commit.

---

### **e. `--stat`**

Shows **summary of files changed** and insertions/deletions.

```bash
git log --stat
```

Example output:

```
commit 9fceb02
 Author: Dheeraj
 Date: Thu Dec 19 10:15:20 2025 +0530
 2 files changed, 10 insertions(+), 2 deletions(-)
```

---

### **f. `--author`**

Filter commits by a specific author.

```bash
git log --author="Dheeraj"
```

---

### **g. `--since` and `--until`**

Filter commits by date.

```bash
git log --since="2 weeks ago"
git log --until="2025-12-01"
```

---

### **h. `-n <number>`**

Limit the number of commits shown.

```bash
git log -n 5
```

Shows the last 5 commits only.

---

### **i. `--grep`**

Search commits by a keyword in the commit message.

```bash
git log --grep="login"
```

---

### **j. `--follow`**

Tracks history of a **specific file**, even if renamed.

```bash
git log --follow -- filename.txt
```

---

## **4. Combining Flags**

You can combine multiple flags for a clean, useful output:

```bash
git log --oneline --graph --decorate --all
```

* `--all` shows all branches
* Result: A compact graph showing all commits across branches with branch names

---

## **5. Practical Example**

Imagine a repo with the following commits:

```bash
git log --oneline --graph --decorate --all
```

Output:

```
* 9fceb02 (HEAD -> main) Fixed login bug
* a1b2c3d Added user validation
| * d4e5f6a (feature/signup) Created signup API
|/
* e7f8g9h Initial commit
```

Here you can see:

* The main branch has 3 commits
* A feature branch `feature/signup` branched off and merged back

---

### ✅ **Key Points**

* `git log` is **read-only**; it doesn’t change anything.
* Flags help **filter, format, or graph** commits.
* Very useful for **debugging**, understanding project history, and **code review**.

---

