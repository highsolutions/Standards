# Git essentials

## Installation

### Creating new repo

	git init --bare

### Download (clone) existing repo

	git clone ADDRESS .

Current folder has to be empty. The same can be achieved by:

	git init --bare
	git remote add origin ADDRESS
	git fetch
	git push origin master

`Fetch` command can be omitted, when *master* branch is chosen.

To not provide password each time you pull/push, install private key or use OAuth in ADDRESS, e.g. (https://USERNAME:PASSWORD@github.com/highsolutions/REPONAME).

## Operations on local repo

### Adding files (as staged) to commit

	git add FILE

This command will add selected file to the nearest commit.

	git add -A
	
This command will add all files (staged, modified and untracked) to the nearest commit.

	git add -p

Console will display each change in files and allows for deciding whether put this into commit or not.

	git add -i

This command displays interactive menu and allow for:

 1. checking (s)tatus,  2. (u)pdate file, 3. (r)evert file, 4. (a)dd untracked files,
 5. (p)atch changes, 6. (d)iff changes

### Removing files from repo

	git rm FILE

This will also delete file from disk.

### Revert state of file

	git checkout SHA FILE

This command will set content of FILE to state from SHA commit (`--` use for HEAD).

### Revert state of branch

	git reset

This command will reset changes in current branch. Content of files will be the same as after commit, adding files to commit operation will be reverted.

	git reset --soft HEAD^

Undo last commit but keep changes in files to edit for commit them again with change.

	git reset --hard HEAD~3

This command will delete last 3 commits and set next one commit as HEAD. 

### Save changes (commit)

	git commit -m "MESSAGE"

This will add staged files to repo.

	git commit -am "MESSAGE"

This will add all new and modified files and create a commit. Beware, untracked files won't be added.

**Message of commit should be explanatory.** Commits name like *fix*, *quickfix*, *merge* should be avoided. On remote branch are forbidden. Keep history of commits clean and clear. On local branch of course you can named them like this, but before push, please `rebase` them.

Prefix message commit with `[WIP]` (Works In Progress) to mark this commit as not finished and that breaks something in repo.

### Add changes to last commit

	git commit --amend --no-edit

This command will add changes to previous commit without changing message.

If last commit is already on remote server you can use force push to update it there also:

	git push origin HEAD -f

Beware that this commands changes SHA of commits history and can cause problems when other developers pull previous commit already.

### Remove commit from remote repo

    git push origin +SHA:master

This command will set commit with SHA as HEAD on *master* branch on remote repo.
Beware that this operation can create conflicts with other developers who will be detached from HEAD on remote repo!

### Mark important commits, mark releases

	git tag NUMBER

This command will tag current commit with NUMBER (e.g. v0.9.1). Use this to mark new releases or introduction new feature.

## Branches

### Create new branch

	git branch new_branch
	
### Checkout and create new branch at once

	git checkout -b new_branch

It's the same as:

	git branch new_branch
	git checkout new_branch

### Go back to default branch

	git checkout -

This command change current branch to default branch of you repo (mostly *master*). You don't have to remember this name.

### Update list of branches from remote repo

    git fetch origin


### Create new remote branch from local environment

	git push origin master:new_branch_name

## Checking status of repo

### Checking status

	git status

Displays state of repo: active branch, which files are stages, modified, deleted and untracked.

### Checking history of commits

	git log

Displays list of commits from newest with details: SHA, date, author and message.

	git log --oneline -3

Displays list of last 3 commits, every commit in one line (SHA and message).

### Check history of all operations

	git reflog

This command displays history of every operation (rebase, cherry-pick, checkout, etc.)

## Operations on commits

### Merge one branch into another

	git merge another_branch

This command will merge commits from *another_branch* to current one.

### Merge few commits into one or drop commit

	git rebase -i SHA

Provide SHA of commit that is the first commit you don't want to update. Instead of SHA you can use `HEAD~3` notation or `HEAD^^^` -> 3 commits behind HEAD (de facto - number after ~ or number of carets means how many commits will be available for rebasing).

When editor show up, you can select operations for displayed commits:

 - **(p)ick** - use commit  - default value
 - **(r)eword** - change message
 - **(e)dit** - edit commit
 - **(s)quash** - put commit into the previous (newest) one
 - **(f)ixup** - also put commit into the previous one, but skips message merge
 - **(d)rop** - delete commit (be careful!)

Don't do it when commits are already on remote repo. This will make conflicts with others. Do it before pushing to remote repo.

### Merge only one commit from one branch to another

	git log --oneline -3 another_branch
	git cherry-pick SHA

This commands display 3 last commits from *another_branch* branch and then merge given commit into current branch. 

### Find a commit that introduce a bug and you cannot find it in code

	git bisect start
	git bisect bad         # Current version is bad
	git bisect good SHA    # SHA is known to be good (can be also a tag)

This commends starts bisecting repo to find bug in code. It'll selects middle commit (in the middle of range between commits marked as good and bad). Then you need to check if bug is still present. You have two choices now:

	git bisect bad     # bug still exists
	git bisect good    # no bug!

Repeat operation until the first commit introducing bug left. You will get SHA of first commit with bug. Clear bisect operation, go back to HEAD and launch *rebase*:

	git bisect reset
	git rebase -i SHA^

Where *SHA* means here SHA of commit with bug. Editor will open and this will be the last commit on the list. Use *edit* operation and amend commit. Done!

## Aliases for popular complex commands

	git config --global alias.logo log --oneline
	git config --global alias.lola log --graph --oneline --decorate --all
	git config --global alias.append commit --amend --no-edit
	git config --global alias.ada add -A
	git config --global alias.cam commit -am

Use them only when you are not pair programming.

## Workflow

### Development on simple project

1. Use *dev* branch for all commits. Make this branch default one.
2. When there is few developers on project, every one should work on working branch (=own or feature branch) and merge commits to *dev* branch when some functionality is finished.
3. When project is ready for deployment, merge *dev* into *master* branch, which will be cloned on production server.
4. When state of website will be tested and ready for another deployment, merge changes from *dev* to *master*.

### Development on complex project

1. Create *dev* branch (default), *preview* (for testing) and *master* (for production).
2. All development commits are stored in *dev* branch.
3. Every developer is working on working branches. Each branch should be prefixed by developer initials (e.g. hs_new_module). Names of branches should be short and descriptive.
4. If module is complex and there are working more than 1 developer, should be created feature branch (special branches for new functionality, module or refactor).  Each feature branch should be prefixed by *feature_*. Working branches of developers should be created from feature branch then.
5. When works on working/feature branch is finished, developer creates Pull Request in GitHub, with description of changes and ask Team Leader or Senior Developer for code review.
6. After code review, Pull Request is accepted and merged into *dev*.
7. When system is stable, tested and working, state from *dev* can be merged into *preview*.
8. When system is ready for deployment, merge changes from *preview* into *master*.

*Master* branch should be cloned on production server and *preview* branch on staging server for test purposes. Code on *master* branch HAS to work always. On *preview* one SHOULD, and if not HAS to be fixed immediately.

If system should be prepared for many countries or many clients and every instance will be slightly differ (other modules, different default language), create adequate number of *preview* and *master* branches with suffix with country/client identifier (e.g. master-dk).