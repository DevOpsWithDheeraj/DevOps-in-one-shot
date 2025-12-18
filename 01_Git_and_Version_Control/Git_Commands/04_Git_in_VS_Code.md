# What is Git in Visual Studio Code?

**Git in VS Code** is the built-in source control feature that lets you:

* Track file changes
* Create commits
* Create & switch branches
* Pull / Push code to GitHub, GitLab, Bitbucket
* Resolve merge conflicts

👉 You can do **almost everything without typing Git commands**, directly from the UI.

---

## How VS Code Uses Git (Behind the Scenes)

VS Code is a **Git client**:

* It uses **Git installed on your system**
* VS Code provides a **GUI on top of Git CLI**

So commands like `git add`, `git commit`, `git pull`, `git push` are executed automatically when you click buttons.

---

## Prerequisites

1. **Git installed**

   ```bash
   git --version
   ```
2. **VS Code installed**
3. A **Git repository** (local or cloned)

---

## Example Project Setup

```bash
mkdir demo-repo
cd demo-repo
git init
code .
```

This opens the folder in VS Code.

---

## Source Control Panel in VS Code

📍 Click **Source Control icon (🔀)** on the left sidebar
or press:

```text
Ctrl + Shift + G
```

You’ll see:

* Changed files
* Staging area
* Commit message box
* Branch info
* Sync / Pull / Push buttons

---

## Example 1: Track File Changes

### Step 1: Create a file

```bash
touch app.txt
```

Add content:

```text
Hello Git from VS Code
```

### What VS Code Shows

* File appears under **Changes**
* Marked with **M (Modified)** or **U (Untracked)**

---

## Example 2: Stage Changes (git add)

### Option 1: Stage Single File

* Click **➕** next to the file

Equivalent Git command:

```bash
git add app.txt
```

### Option 2: Stage All Files

* Click **➕** next to “Changes”

Equivalent:

```bash
git add .
```

---

## Example 3: Commit Changes

1. Enter commit message:

   ```
   Initial commit
   ```
2. Click **✔ Commit**

Equivalent Git command:

```bash
git commit -m "Initial commit"
```

---

## Example 4: Clone Repository in VS Code

### Using UI

1. Press `Ctrl + Shift + P`
2. Type **Git: Clone**
3. Paste repo URL
4. Select folder

### Using Terminal

```bash
git clone https://github.com/user/repo.git
code repo
```

---

## Example 5: Push Code to GitHub

After commit:

* Click **Sync Changes / Push**

Equivalent:

```bash
git push origin main
```

👉 If credentials are asked repeatedly, use:

```bash
git config --global credential.helper manager
```

---

## Example 6: Pull Latest Changes

Click **Pull** or **Sync**

Equivalent:

```bash
git pull origin main
```

---

## Example 7: Create & Switch Branch

### Create New Branch

1. Click branch name (bottom-left)
2. **Create new branch**
3. Enter name:

   ```
   feature/login
   ```

Equivalent:

```bash
git checkout -b feature/login
```

---

### Switch Branch

* Click branch name → select branch

Equivalent:

```bash
git checkout main
```

---

## Example 8: View Git Diff (Code Changes)

Click on a file → VS Code shows:

* **Red** → removed lines
* **Green** → added lines

Equivalent:

```bash
git diff
```

---

## Example 9: Undo Changes

### Discard File Changes

* Right-click file → **Discard Changes**

Equivalent:

```bash
git checkout -- app.txt
```

---

### Undo Last Commit (Soft Reset)

1. Open Command Palette
2. **Git: Undo Last Commit**

Equivalent:

```bash
git reset --soft HEAD~1
```

---

## Example 10: Resolve Merge Conflict in VS Code

When conflict occurs, VS Code shows:

```text
<<<<<<< HEAD
current code
=======
incoming code
>>>>>>> feature
```

Buttons appear:

* ✅ Accept Current Change
* ✅ Accept Incoming Change
* ✅ Accept Both
* ✅ Compare Changes

After resolving:

1. Save file
2. Stage
3. Commit

---

## Example 11: Git History & Log

### View File History

* Right-click file → **Open Timeline**

### View Repo History

* Install **GitLens** extension (recommended)

Equivalent:

```bash
git log
```

---

## Useful VS Code Git Shortcuts

| Action          | Shortcut           |
| --------------- | ------------------ |
| Source Control  | `Ctrl + Shift + G` |
| Command Palette | `Ctrl + Shift + P` |
| Commit          | `Ctrl + Enter`     |
| Toggle Terminal | `Ctrl + ``         |
| Stage File      | Click ➕            |
| Discard Changes | Right-click file   |

---

## Recommended Extensions for Git in VS Code

1. **GitLens** – commit history, blame, authorship
2. **Git Graph** – visual branching
3. **GitHub Pull Requests** – manage PRs inside VS Code

---

## When to Use VS Code Git vs CLI?

### Use VS Code Git when:

* Reviewing changes
* Small commits
* Conflict resolution
* Quick branching

### Use Git CLI when:

* Complex rebasing
* Cherry-pick
* Squash / reorder commits
* Automation scripts

---

## Summary

✔ VS Code makes Git **visual & beginner-friendly**  <br>
✔ Behind the scenes, it uses **Git CLI** <br>
✔ Perfect for **daily DevOps & development workflows** <br>
✔ Reduces command memorization 

---

