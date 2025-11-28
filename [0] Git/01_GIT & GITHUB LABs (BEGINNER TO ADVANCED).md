

---
---
---

Create a detailed, beginner-friendly, step-by-step lab manual that covers all major Git and GitHub concepts. The document should be descriptive, well-structured, and hands-on. For each topic, include explanations, real-world examples, terminal commands with expected outputs, common mistakes, and practice exercises. Organize the lab content from basic to advanced topics, including installation, repository management, branching, merging, resolving conflicts, working with remotes, pull requests, GitHub workflows, and collaboration best practices.

---
---

Below is a **complete, beginner-friendly, step-by-step Git & GitHub Lab Manual** that you can use for self-study, classroom labs, or training workshops.  
It includes **explanations, real-world examples, terminal commands + expected outputs, common mistakes, and practice exercises**, organized from basic → advanced.

---

# **GIT & GITHUB LAB MANUAL (BEGINNER TO ADVANCED)**

_A practical, hands-on guide to version control and collaboration._

---

# **TABLE OF CONTENTS**

1. **Introduction**
    
2. **Lab 1 — Installing Git**
    
3. **Lab 2 — Configuring Git**
    
4. **Lab 3 — Creating and Managing a Local Repository**
    
5. **Lab 4 — Tracking Changes (add, commit, status, log)**
    
6. **Lab 5 — .gitignore**
    
7. **Lab 6 — Branching**
    
8. **Lab 7 — Merging**
    
9. **Lab 8 — Resolving Merge Conflicts**
    
10. **Lab 9 — Working with Remote Repositories**
    
11. **Lab 10 — Using GitHub (Cloning, Pushing, Pulling)**
    
12. **Lab 11 — Forking Repos**
    
13. **Lab 12 — Pull Requests**
    
14. **Lab 13 — GitHub Workflow (Issues, Discussions, Code Review)**
    
15. **Lab 14 — Collaboration Best Practices**
    
16. **Appendix — Suggested Practice Projects**
    

---

# **INTRODUCTION**

Git is a **distributed version control system** that lets developers track changes in code, collaborate safely, and maintain detailed project history. GitHub is a **cloud-based hosting service** that stores Git repositories and provides collaboration features like pull requests, issues, and CI/CD workflows.

This manual takes you from **zero experience** to becoming **collaboration-ready**.

---

# **LAB 1 — Installing Git**

## **Objective**

Install Git on your system and verify the installation.

## **Steps**

### **Windows**

1. Download Git from: [https://git-scm.com](https://git-scm.com/)
    
2. Run installer → keep defaults (recommended).
    
3. Open **Git Bash**.
    

### **Mac**

```bash
brew install git
```

### **Linux**

```bash
sudo apt install git
```

---

## **Verify installation**

```bash
git --version
```

### Expected output

```
git version 2.45.1
```

---

## **Common mistakes**

❌ Running git commands in PowerShell with incorrect syntax  
✔ Use Git Bash or ensure Git is added to PATH.

---

## **Exercise**

1. Install Git.
    
2. Verify version.
    
3. Open Git Bash or Terminal.
    

---

# **LAB 2 — Configuring Git**

## **Objective**

Set your identity for commits.

## **Commands**

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"
```

### Check settings

```bash
git config --list
```

### Expected output (example)

```
user.name=John Doe
user.email=johndoe@gmail.com
```

---

## **Common mistakes**

- Using a different email than your GitHub account (may cause identity mismatch).
    

---

## **Exercise**

1. Configure Git with your name and email.
    
2. Change default editor to VS Code.
    

---

# **LAB 3 — Creating and Managing a Local Repository**

## **Objective**

Create your first repo.

### Create a project folder

```bash
mkdir my-first-repo
cd my-first-repo
```

### Initialize a Git repository

```bash
git init
```

### Expected output

```
Initialized empty Git repository in /home/user/my-first-repo/.git/
```

### Check repo status

```bash
git status
```

---

## **Common mistakes**

- Running `git init` in the wrong folder.
    
- Creating nested repos (avoid having `.git` folders inside each other).
    

---

## **Exercise**

1. Create a new repository.
    
2. Explore the hidden `.git` folder.
    

---

# **LAB 4 — Tracking Changes (add, commit, status, log)**

## **Objective**

Track files, commit changes, and view history.

### Create a file

```bash
echo "Hello Git" > hello.txt
```

### Check status

```bash
git status
```

### Add file to staging

```bash
git add hello.txt
```

### Commit changes

```bash
git commit -m "Add hello.txt"
```

### View commit history

```bash
git log --oneline
```

### Expected output

```
a13c2b1 Add hello.txt
```

---

## **Common mistakes**

- Forgetting to stage files (`git add`) before committing.
    
- vague commit messages like “fix stuff”.
    

---

## **Exercise**

1. Create multiple files.
    
2. Stage and commit each change separately.
    
3. View commit history.
    

---

# **LAB 5 — Using .gitignore**

## **Objective**

Ignore files that should not be tracked.

### Create `.gitignore`

```
*.log
node_modules/
.env
```

### Example

```
echo "API_KEY=123" > .env
```

### Check

```bash
git status
```

Expected: `.env` should not appear.

---

## **Common mistakes**

- Wrong file name (`gitignore.txt` instead of `.gitignore`).
    
- Adding .gitignore AFTER files are already tracked.
    

---

## **Exercise**

1. Create two files: secret.env and debug.log
    
2. Add patterns to ignore them.
    

---

# **LAB 6 — Branching**

## **Objective**

Create and switch between branches.

### Create a branch

```bash
git branch feature-login
```

### Switch to it

```bash
git checkout feature-login
```

OR (modern)

```bash
git switch feature-login
```

### Create + switch in one step

```bash
git switch -c feature-payment
```

### View branches

```bash
git branch
```

---

## **Common mistakes**

- Editing files on the wrong branch.
    
- Forgetting to switch back to `main`.
    

---

## **Exercise**

1. Create three branches.
    
2. Add a unique feature to each.
    

---

# **LAB 7 — Merging**

## **Objective**

Merge branches into main.

### Switch to main

```bash
git switch main
```

### Merge

```bash
git merge feature-login
```

Expected output (fast-forward example):

```
Updating 1f41e2a..b701239
Fast-forward
```

---

## **Common mistakes**

- Attempting to merge while on the wrong branch.
    
- Not committing changes before merging.
    

---

## **Exercise**

1. Merge one branch into main.
    
2. Observe changes.
    

---

# **LAB 8 — Resolving Merge Conflicts**

## **Objective**

Fix conflicts manually.

### Create conflicting change on main

```
Hello from main branch
```

### Create conflicting change on feature branch

```
Hello from feature branch
```

### Merge and see conflict

```
<<<<<<< HEAD
Hello from main branch
=======
Hello from feature branch
>>>>>>> feature
```

### Resolve manually:

Choose the desired version and delete conflict markers.

---

## **Common mistakes**

- Leaving conflict markers inside the file.
    
- Forgetting to `git add` after resolving.
    

---

## **Exercise**

1. Create an intentional conflict.
    
2. Resolve it.
    
3. Commit the fix.
    

---

# **LAB 9 — Working with Remote Repositories**

## **Objective**

Link local repo to GitHub.

### Add remote

```bash
git remote add origin https://github.com/username/my-first-repo.git
```

### Verify

```bash
git remote -v
```

---

## **Common mistakes**

- Incorrect URL (SSH vs HTTPS mismatch).
    
- Adding multiple origins.
    

---

## **Exercise**

1. Create GitHub repo.
    
2. Connect local repo.
    

---

# **LAB 10 — Cloning, Pushing, Pulling**

## **Objective**

Work with remote changes.

### Clone

```bash
git clone https://github.com/user/project.git
```

### Push

```bash
git push origin main
```

### Pull

```bash
git pull origin main
```

---

## **Common mistakes**

- Forgetting to pull before pushing.
    
- Merge conflicts after pulling.
    

---

## **Exercise**

1. Clone a repo.
    
2. Make a change.
    
3. Push it.
    

---

# **LAB 11 — Forking (GitHub)**

## **Objective**

Work with someone else's repo.

### Steps

- Click **Fork** on GitHub.
    
- Clone the fork.
    
- Make changes.
    
- Push to your fork.
    

---

## **Exercise**

Fork a sample open-source repo and explore.

---

# **LAB 12 — Pull Requests**

## **Objective**

Submit changes to original repo.

### Steps

1. Push changes to a branch.
    
2. On GitHub → **Pull Request** → **New PR**.
    
3. Provide description.
    
4. Submit.
    

---

## **Exercise**

Create a PR in your own repo (main ← new branch).

---

# **LAB 13 — GitHub Workflows (Issues, Code Review, Actions)**

## **Objective**

Use GitHub’s collaboration tools.

### Issues

- Create a bug report issue.
    
- Assign yourself.
    

### Code Review

- Review your own PR (comment, request changes).
    

### GitHub Actions

- Explore prebuilt workflows.
    

---

## **Exercise**

1. Create an issue.
    
2. Link it to a PR.
    

---

# **LAB 14 — Collaboration Best Practices**

## **Essential guidelines**

✔ Always create feature branches  
✔ Write clear commit messages  
✔ Pull frequently to avoid conflicts  
✔ Use `.gitignore` properly  
✔ Request code reviews  
✔ Never push directly to `main` on team projects

---

# **APPENDIX — PRACTICE PROJECTS**

## **Beginner**

- Personal notes repo
    
- Simple calculator app
    

## **Intermediate**

- Portfolio website with branches for each new feature
    
- Markdown documentation project
    

## **Advanced**

- Collaborative open-source contribution
    
- Multi-branch large application
    
- GitHub Actions CI pipeline
    

---

If you’d like, I can also generate:

📘 A downloadable PDF version  
📚 A version formatted as a classroom handout  
🧪 Additional practice labs with solutions  
📺 Video-style step-by-step mini lessons

Would you like any of these?

---
---
---

generate A version formatted as a classroom handout, Additional practice labs with solutions & Video-style step-by-step mini lessons

---

Below is your **complete classroom-ready bundle**, consisting of:

1. **A professionally formatted classroom handout**
    
2. **Additional practice labs with solutions**
    
3. **Video-style step-by-step mini lessons (script + storyboard format)**
    

All content is clean, structured, and ready for printing, LMS upload, or instructor presentation.

---

# =========================================

# **📘 1. CLASSROOM HANDOUT — Git & GitHub Essentials**

# =========================================

**TITLE:** _Git & GitHub: Classroom Handout_  
**AUDIENCE:** Beginner–Intermediate  
**DURATION:** 2–3 lab sessions

---

# **Section 1 — What Is Git?**

Git is a distributed version control system that lets teams track changes to files, collaborate, and review history.

**Key Terms**

- **Repository (repo)** – project folder tracked by Git
    
- **Commit** – snapshot of project state
    
- **Branch** – parallel line of development
    
- **Merge** – bring branches together
    
- **Remote** – online copy of your repo (GitHub)
    

---

# **Section 2 — Installing & Configuring Git**

### Install Git

- Windows/Mac/Linux: [https://git-scm.com](https://git-scm.com/)
    

### Configure identity

```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

---

# **Section 3 — Creating & Managing Repositories**

### Create a repo

```
mkdir project
cd project
git init
```

### Check status

```
git status
```

### Add + Commit

```
git add file.txt
git commit -m "Add file.txt"
```

---

# **Section 4 — Branching & Merging**

### Create branch

```
git switch -c feature-1
```

### Merge

```
git switch main
git merge feature-1
```

---

# **Section 5 — Working With Remotes**

### Add remote

```
git remote add origin https://github.com/user/repo.git
```

### Push / Pull

```
git push origin main
git pull origin main
```

---

# **Section 6 — Pull Requests on GitHub**

1. Push your branch
    
2. Open GitHub → **Pull Requests**
    
3. Click **New Pull Request**
    
4. Describe change
    
5. Request review
    
6. Merge after approval
    

---

# **Section 7 — Best Practices**

- Use clear commit messages
    
- Create feature branches for each change
    
- Pull before you push
    
- Avoid editing on `main`
    
- Use `.gitignore`
    
- Keep PRs small & focused
    

---

# =========================================

# **📗 2. ADDITIONAL PRACTICE LABS (WITH SOLUTIONS)**

# =========================================

Below are **five extended labs** for practicing real-world situations.  
Each includes **objective, instructions, expected output, and solution**.

---

# **LAB A — Rewriting Commit History (Beginner → Intermediate)**

### **Objective**

Fix incorrect commit messages using interactive rebase.

### **Instructions**

1. Create three files and commit with poor messages like:
    
    - “update”
        
    - “stuff”
        
    - “more stuff”
        
2. Rewrite messages using rebase.
    

### **Commands**

```
git rebase -i HEAD~3
```

### In the editor

Change:

```
pick 84fd111 update
```

To:

```
reword 84fd111 update
```

### **Expected**

Rewrite messages to:

- “Add login page”
    
- “Implement header UI”
    
- “Fix CSS spacing”
    

---

## **Solution Summary**

Interactive rebase opens a list of commits. Mark any commit as `reword`, save, enter the corrected message, and continue until rebase completes.

---

# **LAB B — Resolving Merge Conflicts in Multiple Files**

### **Objective**

Simulate a multi-file conflict.

### **Instructions**

1. Create branch `feature-text`.
    
2. Modify `index.html` and `style.css`.
    
3. Switch to main and modify the same lines differently.
    
4. Merge.
    

### **Expected Conflicts**

Both files show `<<<<<<< HEAD` markers.

### **Solution Summary**

Remove conflict markers and manually choose the correct combined versions.  
Then:

```
git add .
git commit
```

---

# **LAB C — Forking & Contributing to an Open-Source Project**

### **Objective**

Perform an open-source style contribution workflow.

### **Instructions**

1. Fork a repo (e.g., a small JavaScript library).
    
2. Clone your fork.
    
3. Create branch: `fix-typo-docs`.
    
4. Update documentation.
    
5. Push and create PR.
    

### **Expected**

A GitHub PR with a clear summary and linked issue number.

---

## **Solution Summary**

Students demonstrate:

- Forking
    
- Branching
    
- Committing
    
- Pushing
    
- Opening PRs
    

---

# **LAB D — GitHub Issues + Project Boards**

### **Objective**

Track tasks using Issues & GitHub Projects.

### **Instructions**

1. Create an Issue titled “Add responsive nav bar”.
    
2. Add labels: `enhancement`.
    
3. Convert it to a task card on a Project Board.
    
4. Link a pull request to auto-close the issue.
    

### **Expected**

PR description includes:

```
Closes #1
```

---

# **LAB E — CI/CD with GitHub Actions (Beginner Pipeline)**

### **Objective**

Automate tests on push events.

### **Instructions**

1. Create `.github/workflows/test.yml`
    
2. Add simple workflow:
    

```
name: Run Tests
on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run test script
        run: echo "All tests passed!"
```

---

## **Expected**

Workflow passes on GitHub → Actions tab.

---

# =========================================

# **📺 3. VIDEO-STYLE MINI LESSONS (SCRIPT + STORYBOARD)**

# =========================================

Each mini-lesson is structured like a YouTube tutorial.  
Use them as scripts, narration guides, or teaching slides.

---

# **🎬 MINI LESSON 1 — “What Is Git?” (2 minutes)**

### **Scene 1 — Intro (00:00–00:10)**

**Visual:** Developer typing, files changing.  
**Narration:**  
"Imagine working on a project where you can undo mistakes, experiment freely, and collaborate with anyone. That’s what Git lets you do."

---

### **Scene 2 — Core Ideas (00:10–01:00)**

**Visual:** Diagrams of snapshots and branches.  
**Narration:**  
"Git tracks changes as snapshots called commits. You can create branches to try new features safely. And merge them when ready."

---

### **Scene 3 — Why GitHub? (01:00–01:40)**

**Visual:** GitHub UI.  
**Narration:**  
"GitHub stores your repositories online so you can collaborate, review code, and automate workflows using GitHub Actions."

---

### **Scene 4 — Summary (01:40–02:00)**

**Narration:**  
"Git is version control. GitHub is collaboration. Together, they power modern software development."

---

# **🎬 MINI LESSON 2 — “Your First Commit” (3 minutes)**

### **Scene 1 — Create Project**

Show terminal:

```
mkdir hello-git
cd hello-git
git init
```

### **Scene 2 — Add File**

```
echo "Hello Git" > hello.txt
git add hello.txt
git commit -m "Add greeting"
```

### **Narration:**

"You’ve just created your first commit — your first snapshot."

---

# **🎬 MINI LESSON 3 — “Branches Explained Visually” (3 minutes)**

### **Scene: Whiteboard animation**

Narration:  
"Branches let you develop features independently. Think of them as alternate timelines for your project."

Terminal demo:

```
git switch -c feature-login
```

"Now you’re safely editing without affecting main."

---

# **🎬 MINI LESSON 4 — “Push, Pull, and PRs” (4 minutes)**

### **Visual timeline** showing remote vs local.

Commands:

```
git push origin feature-login
```

Narration:  
"Once you push your changes, you can open a Pull Request on GitHub. PRs let teammates review your work before merging."

---

# **🎬 MINI LESSON 5 — “Fixing Merge Conflicts” (4 minutes)**

Show conflict markers:

```
<<<<<<< HEAD
Version A
=======
Version B
>>>>>>> feature
```

Narration:  
"Conflicts happen when Git can’t auto-merge. Just pick the correct version, delete the markers, and commit the fix."

---

# =========================================

# **✨ OPTIONAL EXTRAS I CAN GENERATE FOR YOU**

# =========================================

If you'd like, I can also produce:

✅ Printable PDF versions  
✅ PowerPoint / Google Slides based on the mini-lessons  
✅ Instructor answer keys  
✅ Student worksheets  
✅ GitHub Classroom-compatible assignments  
✅ A full 2–5 day curriculum

Just tell me what you want!

---
---
---

