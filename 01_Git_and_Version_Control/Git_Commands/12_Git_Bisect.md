

# **What is Git Bisect?**

`git bisect` is a **binary search tool** in Git. It helps you **pinpoint the exact commit** where a bug or issue was introduced, by automatically checking commits in a smart order instead of manually checking each one.

Instead of looking through 100s of commits, Git Bisect lets you find the offending commit in **log₂(N)** steps.

---

## **How Git Bisect Works**

1. You tell Git which commit is **good** (working) and which is **bad** (buggy).
2. Git checks out a commit roughly **in the middle**.
3. You test that commit:

   * If it’s **good**, Git looks at the newer half of commits.
   * If it’s **bad**, Git looks at the older half of commits.
4. Git repeats until it **finds the exact commit** that introduced the bug.

---

## **Git Bisect Commands**

1. **Start Bisecting**

```bash
git bisect start
```

2. **Mark the Bad Commit**

```bash
git bisect bad
```

Usually, this is the current commit that has the bug.

3. **Mark a Good Commit**

```bash
git bisect good <commit-hash>
```

The commit hash should be a commit you know is working fine.

4. **Git will checkout a commit in the middle**

* You now **test this commit** in your code.
* If it has the bug:

```bash
git bisect bad
```

* If it’s fine:

```bash
git bisect good
```

5. **Repeat** until Git finds the offending commit.

6. **Finish Bisect**

```bash
git bisect reset
```

This returns you to your original branch.

---

## **Example**

Imagine you have 10 commits:

```
a1b2c3d (good)
b2c3d4e
c3d4e5f
d4e5f6g
e5f6g7h (bad)
f6g7h8i
g7h8i9j
h8i9j0k
i9j0k1l
```

1. Start bisect:

```bash
git bisect start
git bisect bad           # current commit is buggy
git bisect good a1b2c3d  # first known good commit
```

2. Git checks out the **middle commit** (say `d4e5f6g`).

3. Test `d4e5f6g`:

```bash
git bisect bad  # if the bug exists here
```

Git now checks out the middle of the **older commits**.

4. Test `b2c3d4e`:

```bash
git bisect good
```

Git now narrows down further to find `e5f6g7h` as the first bad commit.

5. Git outputs:

```
e5f6g7h is the first bad commit
```

6. Reset to your branch:

```bash
git bisect reset
```

---

### **Extra Tips**

* You can automate testing with a script:

```bash
git bisect run ./test_script.sh
```

Git will automatically mark commits as good or bad based on your script.

* Useful flags:

  * `git bisect visualize` → show current bisect range.
  * `git bisect log` → see the steps Git has done.

---

✅ **Summary:**
`git bisect` is like playing “guess the number” on commits. You keep halving the search space until you find the exact commit that introduced the bug. It’s **fast**, **efficient**, and saves a lot of manual debugging.

---
