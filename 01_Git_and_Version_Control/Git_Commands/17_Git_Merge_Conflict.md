
# **1. What is a Git Merge Conflict?**

A **merge conflict** happens when Git cannot automatically reconcile differences between two branches you are trying to merge. This usually occurs when the same file has been modified in both branches, and Git doesn’t know which version to keep.

---

## **2. Example Scenario**

Suppose we have a Git repository with a file `app.txt`:

```text
Hello World
This is Git tutorial.
```

### Step 1: Create a branch and make changes

```bash
git checkout -b feature-branch
```

Edit `app.txt` in `feature-branch`:

```text
Hello World
This is Git tutorial.
Feature branch change.
```

Commit the changes:

```bash
git add app.txt
git commit -m "Update app.txt in feature branch"
```

### Step 2: Make a conflicting change in main branch

Switch back to main:

```bash
git checkout main
```

Edit `app.txt` in `main` branch:

```text
Hello World
This is Git tutorial.
Main branch change.
```

Commit the change:

```bash
git add app.txt
git commit -m "Update app.txt in main branch"
```

---

### Step 3: Try to merge the feature branch

```bash
git merge feature-branch
```

Git will respond:

```
Auto-merging app.txt
CONFLICT (content): Merge conflict in app.txt
Automatic merge failed; fix conflicts and then commit the result.
```

---

## **3. Understanding the Conflict in `app.txt`**

Open `app.txt`:

```text
Hello World
This is Git tutorial.
<<<<<<< HEAD
Main branch change.
=======
Feature branch change.
>>>>>>> feature-branch
```

* `<<<<<<< HEAD` → your current branch (`main`)
* `=======` → separator between changes
* `>>>>>>> feature-branch` → incoming branch (`feature-branch`)

Git is telling you: *I don’t know which line to keep.*

---

## **4. Resolving the Merge Conflict**

You have options:

### Option 1: Keep main branch changes

```text
Hello World
This is Git tutorial.
Main branch change.
```

### Option 2: Keep feature branch changes

```text
Hello World
This is Git tutorial.
Feature branch change.
```

### Option 3: Combine both changes

```text
Hello World
This is Git tutorial.
Main branch change.
Feature branch change.
```

After editing, mark it as resolved:

```bash
git add app.txt
git commit -m "Resolved merge conflict between main and feature-branch"
```

---

## **5. Tips to Avoid Merge Conflicts**

1. Pull latest changes before working:

   ```bash
   git pull origin main
   ```
2. Communicate with your team to avoid editing the same lines.
3. Make smaller, more frequent commits.

---

✅ **Summary:**
A merge conflict occurs when Git cannot automatically merge changes in the same part of a file. You have to manually edit the file, choose which changes to keep, and commit the resolution.

---
