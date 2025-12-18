
## **1. What is `git drop`?**

`git drop` is **not a standard Git command**. It is actually a **command used within an interactive rebase**. Its purpose is to **remove a commit from history**.

* Normally, you use it like this:

```bash
git rebase -i <base-commit>
```

* Then, in the interactive editor, you can **drop a commit** by changing its action from `pick` to `drop`.

---

### **2. When do you use `git drop`?**

* When you want to **completely remove a commit** from your branch history.
* Useful for cleaning up your branch before merging to `main` or `master`.

⚠️ **Warning:** Dropping commits rewrites history, so **don’t do it on shared branches**.

---

### **3. Step-by-Step Example**

#### **Scenario:**

You have this commit history:

```
a1b2c3 Commit 1
b2c3d4 Commit 2
c3d4e5 Commit 3
d4e5f6 Commit 4
```

You want to **remove "Commit 3"**.

---

#### **Step 1: Start an interactive rebase**

```bash
git rebase -i HEAD~4
```

* `HEAD~4` means you want to edit the last 4 commits.
* Your editor opens like this:

```
pick a1b2c3 Commit 1
pick b2c3d4 Commit 2
pick c3d4e5 Commit 3
pick d4e5f6 Commit 4
```

---

#### **Step 2: Drop the commit**

* Change the word `pick` to `drop` for the commit you want to remove:

```
pick a1b2c3 Commit 1
pick b2c3d4 Commit 2
drop c3d4e5 Commit 3
pick d4e5f6 Commit 4
```

---

#### **Step 3: Save and exit**

* Git will automatically remove the commit and reapply the rest.
* New history:

```
a1b2c3 Commit 1
b2c3d4 Commit 2
d4e5f6 Commit 4
```

✅ **Commit 3 is completely removed.**

---

### **4. Quick Tip**

If you know the commit hash, you can also remove it via **rebase onto its parent**:

```bash
git rebase --onto <parent-of-commit> <commit-to-drop> <branch>
```

But the interactive `drop` method is simpler and visual.

---

