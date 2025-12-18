

# **What is Git Reorder?**

In Git, **reordering commits** means changing the order of commits in your branch’s history. This is useful when you want your commits to be:

* Cleaner
* Logical
* Easier to understand before merging or pushing

Git itself doesn’t have a command called `git reorder`, but we **achieve this using `git rebase -i` (interactive rebase)**.

---

### **How Git Reorder Works**

1. You start an **interactive rebase** with a base commit.
2. Git opens a text editor showing your recent commits in order.
3. You can **reorder lines** (commits), **squash commits**, **edit messages**, or **drop commits**.
4. Git reapplies the commits in the order you specified.

---

### **Command Syntax**

```bash
git rebase -i <base-commit>
```

* `<base-commit>` is the commit **before the first commit you want to reorder**.
* Usually, you can use `HEAD~n` where `n` is the number of recent commits.

---

### **Example**

Suppose your commit history looks like this:

```
commit C3 - Fix typo in README
commit C2 - Add login feature
commit C1 - Initial commit
```

And you want the **login feature** to come **before** the typo fix.

#### Step 1: Start Interactive Rebase

```bash
git rebase -i HEAD~2
```

* We want to reorder the last 2 commits (`C2` and `C3`).

#### Step 2: Edit the Commit List

Git opens your editor like this:

```
pick abc123 Add login feature
pick def456 Fix typo in README
```

* `pick` = keep this commit as is.

#### Step 3: Reorder Commits

Change the order of lines:

```
pick def456 Fix typo in README
pick abc123 Add login feature
```

* Now `Fix typo in README` will be applied first.

#### Step 4: Save and Close Editor

Git will replay the commits in the new order.

#### Step 5: Verify History

```bash
git log --oneline
```

```
def456 Fix typo in README
abc123 Add login feature
C1 Initial commit
```

✅ The commits are now **reordered**.

---

### **Important Notes**

1. **Rewriting History**
   Reordering changes commit hashes because the history is rewritten.

   * Don’t reorder commits that have already been pushed to a shared branch.

2. **Conflict Resolution**
   If conflicts occur during rebase, Git will pause. Resolve conflicts, then:

   ```bash
   git rebase --continue
   ```

3. **Abort Rebase**
   If something goes wrong:

   ```bash
   git rebase --abort
   ```

---

### **Summary Table**

| Step              | Command / Action             |
| ----------------- | ---------------------------- |
| Start rebase      | `git rebase -i HEAD~n`       |
| Reorder commits   | Move `pick` lines in editor  |
| Save & close      | Reapply commits in new order |
| Resolve conflicts | `git rebase --continue`      |
| Abort rebase      | `git rebase --abort`         |

---

