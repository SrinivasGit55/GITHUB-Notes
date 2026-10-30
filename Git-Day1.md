# Git Day 1 Notes

## Where Do We Come in SDLC?

**SDLC Steps:** Plan -> Code -> Build -> Test -> Deploy -> Operate -> Monitor

- **Plan (Step 1):** Done by **Technical Architect**. They design the blueprint of the project - what cloud, what architecture, what platform. Not our job as DevOps.
- **Code (Step 2):** Done by **Developers**. They write source code and work on Git/GitHub.
- **Our Link as DevOps Engineer:** We deploy the code. So we need to know where the source code is stored and how it is managed. That's why we learn Git. We won't write Java code, but we will write Ansible, Dockerfile, Kubernetes manifests, Terraform files and store them on Git too.

## What is Git?

**Full Form: Global Information Tracker**

- **Global:** Code written in India can be tracked from Australia / Canada. Anywhere around the globe.
- **Information:** Your source code.
- **Tracker:** To follow up / track the changes.

Also called as **VCS - Version Control System** or **Source Code Management (SCM) Tool**.

**VCS Meaning:** It keeps source code separately for each version.
Example: Version 1 = 100 lines of code stored in Repository 1. Version 2 = 200 lines stored in Repository 2. Version 3 = 300 lines in Repository 3.
**Repository = Folder which contains source code.**

## Core Features of Git [Interview Points]

1. **Used to track files:** It tracks what kind of changes are happening.
2. **Maintains multiple versions of the same file:** Example: `index.html` is a basic file for every application. V1 has index.html, V2 also has index.html, V3 also has index.html. File is same but version is different. Git can track it.
3. **Platform Independent:** Can run on Windows, Linux, macOS.
4. **Free and Open Source:** No need to pay. Source code is publicly available on the internet.
5. **Handles Larger Projects Efficiently:** Can handle projects like Instagram, WhatsApp.
6. **Generation:** 3rd Generation Version Control System.
7. **Language:** Written in **C Language**. 50% C, rest Shell, Python, Tcl.
8. **Invented By:** **Linus Torvalds** - the same person who invented Linux. Came to market in **2005** (not 2004).

**Note:** Git is also called as **Jit** by foreign clients due to accent. Same thing.

## History of Version Control Systems

- **SCCS (Source Code Control System) - 1972:** Year C language was born. Limitation: Could track only 1 file, 1 user.
- **RCS (Revision Control System):** Could track multiple files, but still single user.
- **CVS (Concurrent Version Control System):** Could track multiple files & folders, but user limitation again.
- **SVN (Subversion) - 2000:** Could track multiple files, multiple folders, multiple users. People used it like a pro.
- **GIT - 2005:** Became more popular than SVN.

## Centralized vs Distributed - Most Important Interview Question

### CVCS - Centralized Version Control System
Example: **SVN**

Definition: Can store code on **single repo / single place**. If that repository is deleted, entire source code is gone.

### DVCS - Distributed Version Control System
Example: **GIT**

Definition: Can store code on **multiple places / multiple repos**. If code is deleted in one place, we still have it in second and third place. That's why Git is preferred.

## Why Store Separately? Concept of ROLLBACK

**Scenario:** Client wants a banking application. Developer writes code, deploys V1 - works fine for 1 year.
Client wants new features. Developer writes V2 and releases it. Now V2 has a bug - If you withdraw Rs. 100, it shows Rs. 2 Lakhs.

If you try to troubleshoot it will take 1 hour or 1 day. In that 1 day, 100 users withdrawing money will see wrong info.

Solution: **Rollback**.
**Rollback = Going back to the previous version of the application.**

When can we rollback? Only if we have the previous version's source code stored separately. That's why Git stores each version separately.

## Architecture - 3 Stages of Git

1. **Working Directory:** The place where you write the source code. Where you work.
   - Also called as Folder / Directory.
2. **Staging Area:** The area where we **track files**.
3. **Repository (Repo):** The place where we **store the tracked source code**. Local repository is `.git`.

## Practical Implementation

**Flow:** `Create a file -> Add (track) -> Commit (store)`

```bash
# 1. Create a Virtual Machine and connect
# 2. Create a folder
mkdir swiggy
cd swiggy

# 3. Install Git
yum install git -y
# yum = Yellow Dog Updator Modified (Package Manager for Linux)
# install = action
# git = package name
# -y = yes

# 4. Check Version
git -v
git --version  # e.g., 2.40

# 5. Initialize Git (MUST DO)
git init
# This creates .git folder - your local repository
# ls -a will show .git
# If you don't give git init, commands will not work

# 6. Create a file (Working Directory)
vim index.html  # or touch java1
# You can copy login page code from W3Schools or just write <h1>my app</h1>
# In real time, developers give this file

# 7. Check Status (Optional, for clarity)
git status
# Untracked = Red color
# Tracked = Green color

# 8. Add to Staging Area
git add index.html
# Command: Working Directory -> Staging Area
git status # Now green

# 9. Commit to Repository
git commit -m "commit 1" index.html
# Command: Staging Area -> Repository
# -m = message

# 10. Second File Example
touch java1
git add java1
git commit -m "commit 2" java1

touch java2
git add java2
git commit -m "commit 3" java2

# 11. Check Logs
git log  # shows all 4 commits with Author, Date, Message
git log -2  # shows last 2 commits
git log --oneline  # shows commits in single line - short format
git log -2 --oneline  # last 2 commits in single line
```

### Summary of Commands

- To create folder: `mkdir`
- To install git: `yum install git -y`
- To check version: `git -v`
- To init repo: `git init`
- To create file: `touch` or `vim`
- To check status: `git status`
- To track: `git add <file>`
- To store: `git commit -m "message" <file>`
- To see commits: `git log`

---

This was the Git Day 1 learning session. Keep practicing the commands to become comfortable with version control.



