# 🔀 Git Pull Request (PR) 

A **Pull Request (PR)** is a way to **request that your code changes be reviewed and merged** into another branch (usually `main` or `develop`) in a Git repository.

> In simple words:
> **“I have made some changes — please review them and merge them into the main code.”**

Pull Requests are mainly used on **GitHub / GitLab / Bitbucket** (not a core Git command).

---

## 🔹 Why Pull Requests Are Used

* ✅ Code review before merging
* ✅ Team collaboration
* ✅ Catch bugs early
* ✅ Maintain clean `main` branch
* ✅ Discuss changes (comments, suggestions)

---

## 🔹 Typical Pull Request Workflow

```text
main branch
   |
   |---- create feature branch
   |---- make changes
   |---- push branch
   |---- open Pull Request
   |---- review + approve
   |---- merge PR
```

---

## 🔹 Step-by-Step Pull Request Example (GitHub)

### 🎯 Scenario

You want to add a **new login feature** to a project.

---

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/org/my-app.git
cd my-app
```

---

### 2️⃣ Create a Feature Branch

```bash
git checkout -b feature/login
```

> Always create PRs from a **separate branch**, not directly from `main`.

---

### 3️⃣ Make Code Changes

```bash
vim login.js
```

Example change:

```js
function login(user) {
  return user ? "Login Successful" : "Login Failed";
}
```

---

### 4️⃣ Stage and Commit Changes

```bash
git add .
git commit -m "Add login functionality"
```

---

### 5️⃣ Push Branch to Remote

```bash
git push origin feature/login
```

---

### 6️⃣ Create Pull Request on GitHub

1. Go to GitHub repository
2. Click **"Compare & pull request"**
3. Select:

   * **Base branch**: `main`
   * **Compare branch**: `feature/login`
4. Add:

   * Title: `Add login functionality`
   * Description: Explain what you changed
5. Click **Create Pull Request**

---

## 🔹 What Happens in a Pull Request?

### 📌 PR Shows:

* Files changed
* Code diff (old vs new)
* Commits included
* Comments & discussions
* CI/CD status (if configured)

---

## 🔹 Code Review Example

Reviewer comment:

```text
Can you add validation for empty username?
```

You update code → commit again → push

```bash
git add .
git commit -m "Add username validation"
git push origin feature/login
```

> 🚀 PR automatically updates — no need to create a new PR.

---

## 🔹 Approving & Merging the Pull Request

Once approved:

### Merge Options on GitHub

| Merge Type           | What it Does                    |
| -------------------- | ------------------------------- |
| **Merge commit**     | Keeps all commits               |
| **Squash and merge** | Combines all commits into one   |
| **Rebase and merge** | Linear history, no merge commit |

Example:

```text
Squash and merge
```

---

## 🔹 After PR Is Merged

```bash
git checkout main
git pull origin main
git branch -d feature/login
```

Delete remote branch (optional):

```bash
git push origin --delete feature/login
```

---

## 🔹 Pull Request vs Git Merge

| Pull Request          | Git Merge            |
| --------------------- | -------------------- |
| Collaboration feature | Git command          |
| Used on GitHub/GitLab | Used locally         |
| Enables code review   | No review by default |
| UI-based              | CLI-based            |

---

## 🔹 Best Practices for Pull Requests

✅ Keep PRs small
✅ Write clear titles & descriptions
✅ One feature per PR
✅ Link issues/tickets
✅ Squash commits before merge
✅ Don’t commit directly to `main`

---

## 🔹 Real DevOps Example (CI/CD)

PR triggers pipeline:

```text
Pull Request → Tests → Security Scan → Approval → Merge → Deploy
```

---

## 🔹 One-Line Summary

> **A Pull Request is a formal way to ask your team to review and merge your code into a shared branch.**

