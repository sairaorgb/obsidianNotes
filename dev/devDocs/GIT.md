
## What Git Is

- **Version Control System (VCS):** Tracks and manages changes in code.
- **Distributed:** Every developer has the full repo (not just files, but complete history).
- **Snapshots, not diffs:** Each commit is a snapshot of the whole project; unchanged files are just references.
- **Content-addressable storage:** Every object (file, commit, tree) has a unique SHA-1 hash → integrity guaranteed.

## Repositories

- Repo = Working directory + `.git/` folder (history & metadata).
- Start points:
    - `git init` → create a brand-new repo (empty history).
    - `git clone <url>` → copy an existing repo (with full history).

_Repo anatomy:
- **Working Directory** → actual project files.
- **Staging Area (Index)** → prep zone before committing.
- **.git directory** → stores snapshots, branches, configs.

## Snapshots (Recording History)

- File states:
    - _Untracked_ → Git doesn’t know this file.
    - _Staged_ → queued for next commit.
    - _Committed_ → saved in history.
- Commands:
    - `git add <file>` → move changes → staging area.
    - `git commit -m "message"` → move staged changes → history (new snapshot).

## Inspecting History

- **`git status`** → current situation (branch, staged, unstaged, untracked files).
- **`git log`** → see commit history.
    - `--oneline` → condensed view.
    - `--graph --oneline --decorate` → visualize branches.
- **`git diff`** → see exact changes.
    - `git diff` → unstaged changes.
    - `git diff --staged` → staged vs last commit.
    - `git diff <commit1> <commit2>` → compare two snapshots.

## Branches

- **Branch** = movable pointer (bookmark) to a commit.
- **Default branch:** usually `main` or `master`.
- As you commit, the branch pointer moves forward.
- **Commands:**
    - `git branch` → list branches.
    - `git branch <name>` → create a new branch.
    - `git switch <name>` → move to branch.
    - `git switch -c <name>` → create + switch.
    - `git checkout <name>` → (older way) switch branch/commit.
- **HEAD:**
    - Points to current branch (e.g. `HEAD -> main`).
    - Detached HEAD = checked out a commit directly, not a branch.

