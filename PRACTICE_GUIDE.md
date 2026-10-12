# Practice Guide: 21 Everyday Git Commands

Work through these exercises in order. Type each command yourself rather than copy-pasting the whole section. Replace placeholders such as `<file>` with real file names. Commands below assume Bash in Ubuntu/WSL or a terminal with Git installed.

## Part 1 — Prepare the repository

### 1. Create a GitHub repository
On GitHub, create a **new empty repository** named `git-21-commands-practice`. Do not add a README or .gitignore on GitHub.

### 2. Put this starter project on GitHub
In your terminal, go to the unzipped folder and run:

```bash
git init
git add .
git commit -m "Initial commit: add practice app"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/git-21-commands-practice.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username. If Git asks you to authenticate, follow GitHub's current sign-in/token instructions. Never paste a token into this guide or share it.

### 3. Practice `git clone`
Move to the parent directory, then clone your own repository into a second folder:

```bash
cd ..
git clone https://github.com/YOUR-USERNAME/git-21-commands-practice.git cloned-practice
cd cloned-practice
```

You now have a working copy downloaded from GitHub. Continue in `cloned-practice`.

## Part 2 — Getting started

### Command 1: `git clone <repo-url>`
Already practiced above. It downloads a repository into a new folder. Try cloning into another folder name, such as `clone-again`.

### Command 2: `git status`
```bash
git status
```
Look for the current branch and whether the working tree is clean. Run this often.

## Part 3 — Branches

### Command 3: `git checkout main`
```bash
git checkout main
```
Switches to the `main` branch. If Git says you are already on `main`, that is okay.

### Command 4: `git pull`
```bash
git pull
```
Downloads and integrates changes from the tracked remote branch. Use it while on `main`.

### Command 5: `git checkout -b feat/update-homepage`
```bash
git checkout -b feat/update-homepage
```
Creates a new branch and switches to it. Do your practice change on this branch, not directly on `main`.

### Command 6: `git branch`
```bash
git branch
```
Lists local branches. The `*` marks the branch you are currently on.

## Part 4 — Make changes

Edit `index.html`: change `Status: first version` to `Status: feature branch version`. Save it.

### Command 7: `git diff`
```bash
git diff
```
Shows changes that are not staged yet. Read the output and identify the changed line.

### Command 8: `git add <file>`
```bash
git add index.html
```
Stages that one file for the next commit.

### Command 9: `git add -u`
First modify the tracked file `CHANGELOG.md` (for example, add `- Practiced Git staging.` under the current release), then run:
```bash
git add -u
```
This stages modifications and deletions to tracked files. It does not stage new untracked files.

### Command 10: `git commit -m "ENG-12345: update homepage status"`
```bash
git status
git commit -m "ENG-12345: update homepage status"
```
A commit saves staged changes in your local repository. The `ENG-12345` prefix is only an example ticket ID; use a message that honestly describes your change.

### Command 11: `git log --oneline -5`
```bash
git log --oneline -5
```
Shows up to five recent commits in compact form.

## Part 5 — Share your work

### Command 12: `git push -u origin feat/update-homepage`
```bash
git push -u origin feat/update-homepage
```
Publishes the new branch and sets its upstream, so future pushes can usually use just `git push`.

### Command 13: `git push`
Make another small edit to `index.html`, then:
```bash
git add index.html
git commit -m "Update homepage description"
git push
```
After pushing, open the repository on GitHub and create a pull request from `feat/update-homepage` into `main`. Review the changed files before creating it. You can also use `gh pr create` if GitHub CLI is installed and authenticated.

## Part 6 — Keep up to date

### Command 14: `git fetch origin`
```bash
git fetch origin
```
Downloads remote references without changing your current files or merging anything.

### Command 15: `git pull --rebase`
Switch to your feature branch and run:
```bash
git checkout feat/update-homepage
git pull --rebase
```
This fetches and replays your local commits on top of the tracked remote branch. If there are no new changes, Git may say it is already up to date.

### Understand non-fast-forward (safe simulation)
To learn the idea, make a change on GitHub's `main` branch using the web editor, then try pushing a separate local commit to `main` only in a throwaway practice clone. A push may be rejected because the remote has commits your local branch does not have. Usually fetch/pull and integrate the remote changes, resolve conflicts if needed, then push. Do not force-push to a shared branch.

## Part 7 — Undo / oops

### Command 16: `git restore <file>`
**Warning: this discards unstaged changes to that file.** First edit `CHANGELOG.md`, save it, and inspect the change:
```bash
git diff
git restore CHANGELOG.md
git status
```
The edit should be gone.

### Command 17: `git restore --staged <file>`
Edit `CHANGELOG.md`, then stage it:
```bash
git add CHANGELOG.md
git status
git restore --staged CHANGELOG.md
git status
```
It is now unstaged, but your file edit remains.

### Command 18: `git commit --amend`
Only amend the most recent commit if it is yours and has **not** been shared with others. To practice safely, make and commit a small change first:
```bash
echo "- Practiced commit amend." >> CHANGELOG.md
git add CHANGELOG.md
git commit -m "Add practice note"
git commit --amend -m "Add Git practice note"
git log --oneline -5
```
This replaces the latest local commit. Avoid amending commits already pushed to a shared branch.

## Part 8 — Handy extras

### Command 19: `git show <commit>`
```bash
git log --oneline -5
git show HEAD
```
`HEAD` means the commit currently checked out. You can also use a commit ID from `git log`.

### Command 20: `git show origin/main:path/to/file`
```bash
git fetch origin
git show origin/main:index.html
```
Displays the version of `index.html` stored on the remote-tracking `origin/main` branch without switching branches. If `origin/main` is not available, check that `main` has been pushed and run `git fetch origin`.

### Command 21: `git worktree add ../my-wt -b feat/second-worktree origin/main`
```bash
git fetch origin
git worktree add ../my-wt -b feat/second-worktree origin/main
```
Creates a second working directory on a separate branch, based on `origin/main`. Inspect it with:
```bash
cd ../my-wt
git status
cd ../cloned-practice
git worktree list
```
If the branch or folder already exists, choose a new branch/folder name. When finished, return to the main working folder and remove the extra worktree with `git worktree remove ../my-wt` (only after saving anything you need).

## Final self-test

Without looking at the guide, explain or demonstrate:
- What is the difference between `git fetch` and `git pull`?
- What does `git add` do compared with `git commit`?
- What does `-u` do in `git push -u origin branch-name`?
- What is the difference between `git restore file` and `git restore --staged file`?
- Why should you avoid amending a commit already shared with teammates?
- What does the `*` mean in `git branch`?
- What is a pull request, and why do teams use one?

## Important safety reminders
- Run `git status` before committing or pushing.
- Review `git diff` before staging changes.
- `git restore <file>` discards unstaged edits.
- `git commit --amend` changes the latest commit; avoid rewriting shared history.
- Never upload secrets, passwords, API keys, access tokens, or private files.
