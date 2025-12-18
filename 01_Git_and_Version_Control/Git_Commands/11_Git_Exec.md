
## **1. `git rebase --exec`**

The `--exec` option lets you **run a shell command on each commit during a rebase**. This is very useful for things like:

* Running tests automatically on each commit
* Running a linter
* Automating repetitive tasks

**Syntax:**

```bash
git rebase -i --exec "<command>" <base-branch>
```

* `<command>` → Any shell command you want to run on each commit
* `<base-branch>` → Branch you are rebasing onto (often `main` or `master`)

---

### **Example 1: Running tests on each commit during rebase**

Suppose you are on a feature branch and want to rebase it on `main`, but ensure your tests pass on every commit:

```bash
git rebase -i --exec "npm test" main
```

Steps happening:

1. Git starts rebasing your commits onto `main`.
2. For each commit, it runs `npm test`.
3. If tests fail, Git stops the rebase, letting you fix the issue before continuing.

---

### **Example 2: Adding a linter check**

```bash
git rebase -i --exec "eslint ." main
```

* ESLint will run on each commit.
* This ensures no commit introduces linting errors.

---

## **2. `git cherry-pick --exec`**

Similarly, you can run a command while cherry-picking:

```bash
git cherry-pick <commit> --exec "<command>"
```

* Useful if you want to test or modify a commit during cherry-pick.

---

### **Key Points:**

* `--exec` is a **rebase/cherry-pick helper**, not a standalone command.
* Very powerful for CI/CD style workflows.
* Can save a lot of manual checking when cleaning up branches.

---
