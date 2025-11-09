- **Version Control System (VCS):** Tracks and manages changes in code.
- **Distributed:** Every developer has the full repo (not just files, but complete history).
- **Snapshots, not diffs:** Each commit is a snapshot of the whole project; unchanged files are just references.
- **Content-addressable storage:** Every object (file, commit, tree) has a unique SHA-1 hash → integrity guaranteed.

- A `Remote Repo` is a version of your project hosted on the internet.
- GIT config can be changed at local (current repo) , global ( current user ) 
  and system (all user level). 

- Repo = Working directory + `.git/` folder (history & metadata).
- Start points:
    - `git init` → create a brand-new repo (empty history).
    - `git clone <url>` → copy an existing repo (with full history).

_Repo anatomy:
- **Working Directory** → actual project files.
- **Staging Area (Index)** → prep zone before committing.
- **.git directory** → stores snapshots, branches, configs.

``` bash
# staging files 
git add <file>
git add --all
git restore --staged <file>

#commit files
git commit -a -m "commit message"
git commit --amend --no-edit

# History of files
git log
git log --oneline
git show <commit>
git diff           # unstaged changes
git diff --staged  # staged changes

# Help
git <command> --help  # man pages
git <command> -h      # concise exp 
```

- New files that are not tracked yet cannot be commited with -am 
- amend flag can be used to quickly add staged changes to last commit.
- Tags are used to label imp commits with extra info such as releases, mile stones.
- History can be streamlined to search on author, time slice and files changed per commit.

#### Internals of GIT

- Blob (Binary large object) is the content of file which has a hash name. 
- Tree is a data structure that maps filenames to blob hashes.
- commit contains pointer to tree , metadata that include parent info

`commit object`-->`parent commit hash` 
			 --> `tree object`(represent root dir snapshot) 
				--> file name 1 -> `blobHash1` :  content of fn1
				--> file name 2 -> `blobHash2`:  content of fn2

- New commit will just have their modified file blob hashes changed.
- New blobs are generated for each file irrespective of size of changes made in file.
- blobs can be stored as loose objects or delta compressed and bundled into `packfiles` , which comprise older blob plus new changes instead of new blob for storage optms. They are just a better storage optimization for trasport and network rather than new mechanism.
- git diff commit1 commit2 just compares trees of both objects and displays the same. 

#### SSH


1. **You create a keypair (once per identity/machine).**

- You run:
```bash
ssh-keygen -t ed25519 -C "sairaorg@laptop"
```
- Files appear:
	- Private: `~/.ssh/id_ed25519` (guard this like mithril)
	- Public: `~/.ssh/id_ed25519.pub` (safe to share)

2. **You teach GitHub your public key (account-level).**

- Copy the `.pub` content into GitHub → Settings → SSH and GPG keys.
- Now GitHub can recognize “this private key holder == your account.”

2. **(Optional) You load the key into an agent (quality of life).**

- Start + add:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```
- Result: you won’t retype the passphrase each push.

4. **(Optional) You set `~/.ssh/config` for nice aliases.**
```ssh
Host github
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
```
- Lets you use `git@github:owner/repo.git` instead of the full domain, and picks the right key if you have many.

5. **You set your repo’s remote to SSH.**

```bash
git remote add origin git@github.com:owner/repo.git
# or with alias:
git remote set-url origin git@github:owner/repo.git
```

6. **You run a Git command that needs auth.**

- Example:
```bash
git push origin main
```

7. **Git invokes SSH under the hood.**

- Git → calls your system `ssh` client → reads `~/.ssh/config` (if present).
- Resolves:
	- Host/HostName (`github` → `github.com`)
	- User (`git`)
	- IdentityFile (which private key to try)
	- Port (defaults to 22)


7. **First contact: server identity check (host key).**

- SSH checks `~/.ssh/known_hosts` for `github.com`’s host key.
- First time, you’re asked to trust and save it. Prevents man-in-the-middle shenanigans later.

7. **Authentication dance (public-key challenge).**

- GitHub says: “Prove you own the private key that matches a public key on your account.”
- Your SSH client either:
	- Uses the private key file directly, or
	- Asks the **ssh-agent** to sign the challenge (private key never leaves memory).
- Signature validates → GitHub links that key to _your GitHub account_.

7. **Authorization (repo-level permissions).**

- GitHub checks: does this account have rights to `owner/repo` for this action?
	- If yes → proceed.
	- If no → `ERROR: Permission denied (publickey).` or similar.

7. **The Git operation happens.**

- For `push`: your commits are packed and shipped.
- For `fetch/pull`: you download the new objects.

##### TL;DR mental model

- **Files**: private key on your machine; public key in your GitHub account.
- **Pipe**: Git → SSH client → GitHub.
- **Gatechecks**: known_hosts (server identity), then key auth (your identity), then repo permission (authorization).
- **Agent**: convenience valet holding your unlocked key so you don’t keep typing passphrases.

#### Branches and Merging

``` bash
git checkout branch-name or git switch branch-name     # Switch branches
git branch                           # List all branches
git branch -d branch-name            # Delete a branch (merged)
git branch -D branch-name            # force delete an unmerged branch
git branch -m old-name new-name      # Rename a branch;
```
- A branch is just a label pointing to a commit. They dont contain commits rather a log starts from latest commit and displays all its parent commits.
- HEAD is a special ref and acts as a context variable that points to current branch name. 
- if a specific commit is checked out , you will be in detached head state.
- Commits made thereby from a detached head are in dangling state not getting attached into current branch. A new branch can be created to include them .They can be recovered from git reflog or else they might get pruned by garbage collector. 
``` bash
git checkout <commit-id>
git branch experiment                 # included dangling commit to a branch
git reflog 
git branch rescue <dangling-commit>   # recovers dangling commits
```

- During branch creation , a new branch that points to current head is created and subsequently current head is changed to commit pointed by branch when switch is done.
- cases while switching 
	- clean working tree -> switch happens
	- unstaged changes with no conflict -> changes are carried over
	- unstaged changes with confict -> git refuses 
	- commited  changes -> commits are saved in curr branch and switch happens.

- commits are a DAG (directed acyclic graph) , git checks for reachability (ancestry) of target branch head from current branch head before deleting(-d) or merging target branch.

**Merging** 
- Fast forward  happens if main's tip is behind target's tip , git can just move pointer forward.
- when branches are diverged , a new merge commit is added that has two parents as both branch heads.
- If there are confilcts, git marker appear in code that needs to be manually resolved and added commited. custom stratagies like ours ( ignores other br changes ), theirs ( ignores current branch changes ) , resolve exsist.
``` text
<<<<<<< HEAD
code from main
==========
code from feature
>>>>>>>> feature
```
``` bash
git merge --no-ff       # force merge commit even if ff possible
git merge --squash      # combine all branch commits into one
git merge --abort       # revert to pre-merge state while resolving conflicts.
```

- `rebase` is a similar mechansim to merge. But it finds the common ancestor between current branch and target branch , and refactors current branch commits after parent into new commits and places them after the target branch head. 
- rebase only changes current branch. so it should be merged again into target branch which can be done through fast forward owing to above logic.
- commit hashes get lost during this process so rebasing should be done only at local level but not at remote where those commits could be shared.
- conflicts caused are resolved manually and commited again.

#### Remotes

- local branches that are pushed into remote servers have an extra remote tracking branch at local level which can just be read but not written.
- Remote tracking brances reflect the state of remote branch you last time pulled or fetched.
  ( main --------> origin/main ). origin is the remote we've setup before. 
- if yrBranch has commits that are not pushed  -  yrBranch is ahead of origin/yrBranch
  if origin/yrBranch has new commits from remote - yrBranch is behind of origin/yrBranch
- git fetch       -  updates remote tracking branch
  git pull         -  updates remote tracking and subsequently merge/rebase into local one
  git push       -  moves the actual remote branch ( doesnt update remote tracking )

- `git pull <remote> <branch>` remote is the root address where it should search for It could be origin or smtng . branch is the specific branch on that remote 
``` bash
# pulls from main branch of origin remote  and merges in curr
git pull origin main

# pulls from dev branch of forkremote remote and merges in curr
git pull forkremote dev
  
# pulls from origin/feature which is upstream 
git branch --set-upstream-to=origin/feature 
git pull

# cat .git/config
[branch "feat"]
	remote = origin
	merge  = refs/heads/feature
```
- when args are omitted , git pull checks for upstream in .git/config if they are not present it gives an error. But git fetch without args updates all remote tracking branches.
- Git fetch (Git pull too) usually updates all the remote tracking branches that includes adding new branches (refs quirk). But ghost branches need to be pruned seperately
- git pull does default merges but can be configured to rebase.
- git push has the same syntax with lil different quirks. 
- In general case, local branch which is source and remote branch which gets pushed into have same names. so : can be dropped and single name is sufficient.
``` bash
# push statement
git push <remote> <local branch>:<remote branch>

# when local and remote branch have same name
git push <remote> <branch>

# deleting a branch on remote
git push <remote> :<remote branch>
git push <remote> --delete <remote branch>

```
- git push usually happens only if out local branch can be merged into remote through fast forward . In the case of divergent branches you are supposed to git pull first and then push.
- git push --force-with-lease pushes commits overwriting any conflicting commits on the remote branch, but only if the remote branch has not been updated since the last time you pulled from it. ( protects others work )
```bash
# force push after a rebase (not recommended)
git push --force origin feature-branch
# force pushed but checks if remote tracking branch is updated.
git push --force-with-lease origin feature-branch
```

- caution need to be exercies while rebasing remote branch commits while merging during pr's.
- If the feat branches are rebased and merged into main , whosoever using old commits will have divergent branches with same commits may having different hashes.

- git remote branch management
``` bash
# delets non exsistent remote tracking branch on remote
git fetch --prune      

# adds new branch remote trackers and deleted ghost trackers
git remote update --prune

# removes branch from remo, others will lose it on fetch 
git push origin --delete feature-x  

# renaming remotes
git remote rename origin fork

# replacing remote url 
git remote set-url origin git@github.com:user/repo.git

# multiple remotes (Open source)
git fetch upstream
git rebase upstream/main
git push origin main
``` 
- 

#### forks 

- fork the repo which creates a copy of it in your remote (github).
- clone the forked repo into your local machine which has origin set as original repo address.
- rename origin as upstream and create a remote named origin with address same as clone.
- changes can be commited and pushed to your clone of forked repo using origin, which thereby can be pushed to orignal repo using a pull request.