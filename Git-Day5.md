# Git Day 5 Notes

## 1. Merge conflict - most important interview concept

A merge conflict happens when two different branches modify the same file with different content and Git cannot decide which version to keep automatically.

### Real scenario
- Developer 1 works on `branch1`
- Developer 2 works on `branch2`
- Both edit the same file: `file1`
- Developer 1 writes Java code in `file1`
- Developer 2 writes Python code in `file1`

When you merge `branch2` into `branch1`, Git sees the same file with conflicting changes.

### Demo

```bash
mkdir PTM && cd PTM
git init

touch file1
vi file1
# Write: Java

git add file1
git commit -m "Commit 1 - Java code on branch1"

git branch -M master branch1
# rename master to branch1

git checkout -b branch2
vi file1
# Replace with Python code
git add file1
git commit -m "Commit 2 - Python code on branch2"

git checkout branch1
vi file1
# Change to Java version 2.0
git add file1
git commit -m "Commit 3 - Java 2.0 on branch1"

git merge branch2
```

Git output will look like this:

```bash
Auto-merging file1
CONFLICT (content): Merge conflict in file1
Automatic merge failed; fix conflicts and then commit the result.
```

### How to resolve merge conflict

You must resolve it manually.

```bash
vi file1
```

Git will add markers like:

```text
<<<<<<< HEAD
Java version 2.0
=======
Python
>>>>>>> branch2
```

You must:
1. Open the conflicted file
2. Delete the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
3. Keep the code you want
4. Remove unwanted content
5. Save the file
6. Run:

```bash
git add file1
git commit -m "Resolved merge conflict"
```

Important point:
- Merge conflict is resolved manually.
- There is no automatic universal fix.

### Interview answer
Q: What is a merge conflict?
A: It happens when two branches change the same file in different ways and Git cannot merge them automatically.

Q: How do you resolve it?
A: Manually edit the file, remove conflict markers, keep the required code, then `git add` and `git commit`.

---

## 2. Cherry-pick - pick only required commits

Cherry-pick lets you apply only specific commits from one branch to another, instead of merging everything.

### Definition
- Merge brings all changes from one branch
- Cherry-pick brings only selected commits/files

### Real-life analogy
If you are picking cherries from a basket, you pick only the good ones you need — not all of them.

### Command

```bash
git cherry-pick <commit-id>
```

### Example

```bash
git checkout branch1

touch Java1.html Java2.html Java3.html
git add Java*
git commit -m "Java commits"

touch python1.html python2.html python3.html
git add python*
git commit -m "Python commits"

touch php1.html php2.html php3.html
git add php*
git commit -m "PHP commits"

git log --oneline --all
```

Now move to `branch2`:

```bash
git checkout branch2
```

If you want only the Python commit in `branch2`, do:

```bash
git cherry-pick <python-commit-id>
```

This will bring only the Python-related changes, not the Java or PHP content.

### Difference between merge and cherry-pick

- `git merge branch1` -> merges all changed files/commits
- `git cherry-pick <commit-id>` -> merges only selected commit(s)

### Multiple commits

```bash
git cherry-pick <commit-id-1> <commit-id-2>
```

### Interview answer
Q: What is cherry-pick?
A: It is used to merge only selected commits from one branch into another by using the commit ID.

---

## 3. Git log and git show - track file history

### To list commits

```bash
git log --oneline
git log --oneline --all
git log --oneline --graph --all
```

### To show details of a specific commit

```bash
git show <commit-id>
```

This shows:
- author
- date
- message
- code diff
- changed files

### To view file-specific history

```bash
git show <file-name>
```

Example:

```bash
git show python1.html
```

This shows how many times the file changed, which branch it belongs to, the diffs, and the commit history for that file.

### To show patch details

```bash
git log --stat
git log --stat --oneline
git log -p
git log --patch
git log --stat -p
```

`-p` means patch, which shows the actual lines changed in each commit.

### Diff command

```bash
git diff <file-name>
```

This is useful for unstaged changes, but for committed changes, `git show` is more useful.

### Use case in real projects
- check who changed a file
- how many times it was modified
- what exact changes were introduced
- which commit broke a deployment

---

## 4. Git admin / manager-level work in real projects

This is real-world GitHub usage and is important for understanding how teams work.

### A. Collaborators

A repo manager or team lead can give push access to other developers.

GitHub flow:
1. Go to your repository
2. Open `Settings`
3. Choose `Collaborators`
4. Add a developer GitHub username
5. The developer accepts the invitation
6. Now they can push code to the same repo

Example:

```bash
git clone <manager-repo-url>
cd repo
# edit files
git add .
git commit -m "developer work"
git push origin main
```

### B. Project management

GitHub Projects are used to assign work to developers.

Example board items:
- `Write code for PTM service`
- Status: `To Do`, `In Progress`, `Done`
- Owner: assigned developer
- Deadline: date or sprint

This is similar to Jira and helps teams track work.

### C. Wiki pages

Wiki pages are used for documentation and repository purpose explanation.

Example:
- repository description
- project overview
- architecture notes
- setup instructions

In GitHub:
1. Open repo
2. Go to `Wiki`
3. Click `Create new page`
4. Add documentation
5. Save

### D. Branch protection

You can protect important branches like `main`, `master`, or `release` so they cannot be deleted or force-pushed.

GitHub steps:
1. `Settings`
2. `Branches`
3. Add branch protection rule
4. Select the branch pattern
5. Enable protection options such as:
   - protect this branch
   - require PR review
   - do not allow force push
   - do not allow deletions

This prevents accidental branch damage.

### E. Delete a repository

GitHub UI:
1. Go to `Settings`
2. Scroll to `Danger Zone`
3. Click `Delete this repository`
4. Confirm by typing the repo name

---

## 5. Git revert - undo a merge or commit safely

Git revert is used to undo a commit by creating a new commit that reverses the change.

### Example

```bash
git checkout branch2
git merge branch1
# by mistake, branch1 changes are merged
ls -l
# PHP files are present

git revert <commit-id>
# or: git revert HEAD
```

This creates a revert commit that undoes the previous merge.

### Use case
- you merged by mistake
- you need to undo the last merge safely
- you want to preserve history instead of deleting history

### Important note
- `git revert` is safe for shared/public branches
- it creates a new commit rather than rewriting history

### Difference between revert and restore

- `git revert` -> undo a previous commit by creating a reverse commit
- `git restore` -> restore file content or unstage files

---

## 6. Clone vs fork vs pull vs fetch

### A. `git clone` vs `git pull`

- `git clone <url>`: first-time full repo download from GitHub to local machine
- `git pull origin <branch>`: update your local repo with the latest changes from GitHub

### B. `git clone` vs `git fork`

- `git clone`: GitHub to local machine
- `git fork`: GitHub account to another GitHub account

Example:
- Clone: copy repo to your laptop
- Fork: copy one GitHub repo into your own GitHub account

### C. `git pull` vs `git fetch`

- `git pull`: download + merge automatically
- `git fetch`: download changes but do not merge yet

Example:

```bash
git fetch origin
git log HEAD..origin/main
```

This lets you inspect changes before merging.

### D. `git revert` vs `git restore`

- `git revert`: undo a commit safely by creating a new commit
- `git restore`: restore file from last commit or unstage a file

---

## 7. Interview questions - complete Git module

### Theory questions
1. What is Git and why do we use it?
2. What are the stages of Git?
3. Difference between CVCS and DVCS?
4. What is branching?
5. How do you check commit history?
6. What is `.gitignore`?
7. What is a branch protection rule?
8. What is a pull request?

### Difference questions
1. `git pull` vs `git push`
2. `git pull` vs `git fetch`
3. `git merge` vs `git rebase`
4. `git merge` vs `git cherry-pick`
5. `git clone` vs `git pull`
6. `git clone` vs `git fork`
7. `git revert` vs `git restore`

### Scenario questions
1. What is a merge conflict and how do you resolve it?
2. What is stash and when do you use it?
3. How do you restore a deleted branch?
4. How do you recover a deleted tracked file?

---

## 8. Final note

This Git module covers the major workflow used in real DevOps and software engineering projects:
- branch creation and switching
- merge and rebase
- merge conflicts
- cherry-pick
- logs and file history
- branch protection and admin work
- stash, restore, and revert

Practice all commands manually. Then answer the above questions in your own words using short, crisp interview points instead of long Google definitions.
