# Intro to Git Cheatsheet
This document is meant to serve as a cheatsheet for users learning git that I have compiled over my time as a student.\
See Docs [here](https://git-scm.com/doc) for more in-depth information

### Notes
- Arguments contained in `{--brackets}` are considered optional
- If an argument is prefixed with a `$`, it is mandatory ie) `${argument}`

## Basic Practice 

### Initialize repo
	~$ git init {--bare}
			
Creates a repository to begin tracking
 - If initalized with `--bare` flag establishes respository to be used for remote work

### Stage files to next commit
	~$ git add ${filename}

### Removing Files
	~$ git rm {--cached} ${file}
- Removes file from filesystem and tracking
- If `--cached` flag, only remove $file from tracking not filesystem

### Commit changes to repo
##### Commit often, it saves you later
Will open default editor for you to commit with message (this is not optional)

	~$ git commit {--all or -a}
Flag commit all staged changes)
	
	~$ git commit --amend 
Commits staged changes to last commit (incase you forgot something)

### Check status of repo
	~$ git status
Behavior:
* Files staged to be changed
* Files not included in repo

## Branching
### Making a new branch
	~$ git branch {-d, -D} ${newBranchName}
Behavior: 
- Creates a new branch with copies of original files. Note this does not checkout the new branch
- if using `-d` flag followed by a branch name will delete that branch 
- if using `-D` flag forcefully deletes that branch
- Note these deleted branches are local changes that must be committed

<!-- -->
### Checkout file
	~$ git checkout ${filename} {branch}
Behavior:
- If branch no specified, reverts uncommitted changes to the modified file 
- Brings `$filename` from `$branch` into current branch as uncommitted changes
- Moves `HEAD` to `$branch` if filename unspecified

<!--  -->
### Checkout Branch
	~$ git checkout BranchName
Behavior:
- Switches to `$BranchName`

### Merge Target to Current
	~$ git merge ${TargetBranch}
- Merges `$TargetBranch` to the currently checkedout branch
- If conflict between branches will show diff between branches and ask with changes you want to keep

### Rebasing

	~$ git rebase ${branch} #ACTION dependent on working branch 
Let feature = non master branch\
Let release = master branch or branch that does not recieve many updates\

- Case: bringing release changes to feature branch
	will advance feature branch to latest release commit and append any changes made in feature

- Case: bringing feature changes to release branch
	will bring all feature commits from last identical commit to release

Visualize:

	master  : m1 - m2 - m3
		       |
	feature :      m2 - f1 - f2

	We want to move f1 and f2 commits to m3 without overriding the m3 commit

	(in feature)
	git rebase master

	master  : m1 - m2 - m3
		 	    |
	feature :      	    m3 - f1 - f2

	now we can either merge to master 
	or
	(in master)
	git rebase feature

	master  : m1 - m2 - m3 - f1 - f2
		 	    |
	feature :      	    m3 - f1 - f2


### .gitignore

Adding files to a .gitignore file will make file not added to the repo not be flagged by git status

Files can be exculed using:
- Unix wildcards. 
  - ex) `*.txt`
- Directories 
  - ex) `{directory_name}/`
- Files 
  -  ex) `placeholder.exe`
- Exception to pattern `!` 
  - ex) `!something.txt` (even though files ending in .txt are ignored something.txt is tracked)

### Cleaning Untracked Files
Secnario: Your program generates some output files in your working directory that you dont plan on tracking. 

	~git clean ${-dfi} {-e ${pattern}} # at least one required
Flags:
- `-d`, 
  remove untracked directories aswell as untracked files

- `-f, --force`, unless force is specified, no files will be deleted

- `-i --interactive`,
interactively clean git repo

- `-e {pattern}, --exclude {pattern}`,
		exclude pattern from removal
	
### Looking at Changes
	~$ git diff
Behavior: to see unstaged changes compared to commited files in repo

	~$ git log 
- shows changes between commits


### Remote Git

	~$ git remote add ${remoteName} ${url}
- adds remote repository named `$remoteName` at `$url` 

	~$ git remote remove ${remoteName}
- removes `$remoteName` as a commitable repository

#### Sharing Changes

	~$ git push ${remoteName} {--delete} ${RemoteBranch}
- Pushes changes to online repo for others to clone or fetch	
- If `--delete` before the branch you want to push will delete the branch locally 

<!--  -->

	~$ git fetch ${remote branch name} {-all}
 - Clones remote branches to local to be checkout out


<!--  -->
	~$ git pull ${remoteName} ${remoteBranchName}
- Fetch then merge `$remoteBranchName` into `HEAD`

	
