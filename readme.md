
## Table of Contents

- [What is git?](#what-is-git)
- [Setting up the local git](#setting-up-the-local-git)
- [Git Init](#git-init)
- [Adding files to the staging area](#adding-files-to-the-staging-area)
- [Remove file from staging area (Undo It)](#remove-file-from-staging-area-undo-it)
- [Status of the current commit](#status-of-the-current-commit)
- [Commit](#commit)
  - [Head](#head)
  - [Modifying the existing commit](#modifying-the-existing-commit)
- [Git Log: Logs of the commits](#git-log-logs-of-the-commits)
- [Git reset: Undo commits with reset](#git-reset-undo-commits-with-reset)
- [Git Revert](#git-revert)
- [Branches](#branches)
  - [Merging a branch](#merging-a-branch)
  - [Git Merging Scenario - 2](#git-merging-scenario---2)
- [Git conflicts](#git-conflicts)
- [Git cherry-pick](#git-cherry-pick)
  - [Difference in cherry-pick and merge](#difference-in-cherry-pick-and-merge)
- [Connecting Local Repo to Remote Repo](#connecting-local-repo-to-remote-repo)
  - [Revisiting git log](#revisiting-git-log)
- [Git Cloning](#git-cloning)
- [Git Fetch and Pull](#git-fetch-and-pull)
  - [Fetch](#fetch)
  - [Pull](#pull)
  - [Using rebase (optional)](#using-rebase-optional)
- [Git Stash](#git-stash)
- [.gitignore](#gitignore)


### What is git
- Git is a distributed version control system that tracks versions of files. It is often used to control source code by programmers who are developing software collaboratively.

### Setting up the local git
```sh
git config --global user.name "Your Name"
git config --global user.email "youremail@mail.com"
```

### Git Init:
```bash
git init
# or
git init --inital-branch <branch_name>
```

### Adding files to the staging area
```bash
git add .
#####
git add <file_name>
```

### Remove file from staging area (Undo It)
```bash
git rm --cached <file_name1> <file_name2>  #... other files
```

- To remove the .git folder
```bash
rm -rf .git
```

### Status of the current commit:
```
git status
```

> Git does not track empty folders, only if some files are present

- Verbose (detailed info)
```bash
git status -v # detailed
# or
git status -s # short
```

### Commit
```bash
git commit -m "<message>"
```
> Now git is tracking the files which are committed, means which were in staging area before commit command.

<img width="2213" height="793" alt="1" src="https://github.com/user-attachments/assets/ee69a4a4-b2c5-4e2b-ae4c-cdf74e213f78" />


#### Head:
-> Points to the latest commit on the branch
-> We can always change the position of the head.

<img width="2113" height="821" alt="2" src="https://github.com/user-attachments/assets/8a31405d-8371-4a8b-8e3f-248807d500bd" />


> If you had made changes in the ***tracked file*** only then you don't need to add first in *staging area*  by using `add` and `commit` separate commands.
```bash
git commit -a -m "<commit_message>"
```


#### Modifying the existing commit

--> If you have already last commit present and you want to make changes in file of that commit but not creating a new commit, (To modify a existing commit)
```bash
# add to staging area
git add <file_name>

#modify the existing commit
git commit --amend  # prompts to update the commit message
# or
git commit --amend --no-edit # uses the existing commit message but does the same work as above
```

- Adding a signature to the commit
```sh
git commit -s -m "<msg>"
```
- Results as:
```sh
git log
""" 
commit 2dafvrrd59e2251xzfaee473e38 (HEAD -> master)
Author: User <user@gmail.com>
Date:   Sat Jul 19 20:34:47 2025 +0530

    <msg>
    
    Signed-off-by: Sadanand Miskin <user@gmail.com>
""" 
```


> Can you create an empty commit?
> -> Yes.

```sh
git commit --allow-empty -m "<message>"
```

> Why do you need a empty commit?
> If ever I want to test my CI pipeline over Jenkins etc. .Without creating a new commit or modifying one just creating an empty commit to trigger and test my CI pipeline.\


### Git Logs

- Get the list of commits
```bash
git log
```

- Get only number of commits
```sh
git log -n 2 # top 2 commmits
```

- Get short version of logs
```sh
git log --pretty=short 
# or 
# --pretty=full ,for full
# --pretty=online
```

- Other method to log (Not much needed)
```sh
git log --pretty=format:"%h %n %an %ae"
# hash , message , author-name (you can use any one also)
```

- To know what changes happened in each commit
```sh
git log -p
"""
commit 0c26b65f9b4ede5957ddc09303d78b41c21d3b4f
Author: Sadanand Miskin <user@gmail.com>
Date:   Sat Jul 19 20:32:18 2025 +0530

    created a.txt again

diff --git a/a.txt b/a.txt
index e69de29..ced8e84 100644
--- a/a.txt
+++ b/a.txt
@@ -0,0 +1 @@
"""
```

> How to display commits of last one week?
```sh 
git log --since="1 week ago" 
# yesterday, 1 month ago, 1 hour ago
``` 

> From this-day to that-day?
```sh
git log --since="2024-05-06" --until="2025-05-01"
```

> By a User:
```sh
git log --author="<author_name>"
```

> Grep inside git
```sh
git log --grep="<to_find_string>"
```


### Git reset

- `git reset` provides us options:
	- Unstage the file changes
	- Undo the commit
	- Discard/Delete the commit

> There are 3 methods in `get reset`:
- soft
- mixed
- hard


<img width="1515" height="623" alt="3" src="https://github.com/user-attachments/assets/5bc45a85-5501-4772-aeb9-5ab9c3affd8f" />
- Suppose I have 3 commits with file changes and I want to change the recent commit

> *Soft*: When using `soft` the changes are moved back to staging area.
```sh
git reset --soft <C2_commit_id>
```

```sh
git diff --staged
"""
diff --git a/a.txt b/a.txt
index ced8e84..1a51ebe 100644
--- a/a.txt
+++ b/a.txt
@@ -1 +1,2 @@
 hello from 
+you know what
"""
```

> *Mixed*:  It will bring back to Non staging area.
```sh
git reset --mixed <c2_commit_id>
```

> Hard: The changes you made in C3 will be discarded and deleted and head points to C2
```sh
git reset --hard <c2_commit_id>
```

### Git Revert
- The difference in `git revert` and `git reset` is that:
	- `git reset`:  It modifies the current commit and there is no backup to a commit.
	- `git revert`:  If you want to undo any commit, then it creates a new commit object without modifying the current (Not loosing the data).

- with example:
<img width="1627" height="618" alt="4" src="https://github.com/user-attachments/assets/e836f5b6-fa11-4d47-9dbe-94a169af1d4a" />

```sh
git revert c3
# creates a new commit object as c4
```

```sh
git checkout <commit_id>
# You can place head to any commit
```

> *Note*: `git reset` is a powerful command, when used may affect all who has the access to the branch as it modifies and undo the commit which leads to loosing of data. Hence `git revert` is used to create a backup of the data

> Use git revert if you want to keep your commit changes stagnant.

### Branches

> Branches are always associated with commit not when `git init`, default as master.

- Create a branch
```sh
git branch <new_branch>
```

> You can create a new branch from where the `head` is pointing in `master` branch

<img width="2663" height="2998" alt="5" src="https://github.com/user-attachments/assets/a991ec4d-1ade-47a3-9702-7026075c9de2" />


- As you can see here the `new` branch is one commit behind the `master` branch or `master` is one commit ahead.
- where as `new2` branch is in sync with the `master`.

- Checking out to other branch
```sh
git checkout <new_branch_name>
```

or use:
```sh
git switch <branchname>
```

> Branches provides us isolation to work independently without worrying about affecting to the master branch.
> From which branch you created a new branch that branch will be taken as reference

<img width="2685" height="1939" alt="6" src="https://github.com/user-attachments/assets/da610a07-e878-4680-a9de-e3ccc41a10f0" />

- Here `new_branch_2` is head but created from reference of `new_branch_1` and it is 2 commits ahead of master branch.

- Deleting a branch:
```sh
git branch -d <branch_name>
```

##### Merging a branch:
```sh
git checkout master
```
`
```sh
git merge new_branch_2
```

- Now both master and new_branch_2 are in sync.

<img width="2282" height="1901" alt="image" src="https://github.com/user-attachments/assets/2dfdd2fe-0721-4f0d-a7cc-9f2590caf3f1" />

- To merge any branch to the master branch, first we need to be in master branch
```sh
git switch master
```

- Then use merge command, Whatever the commit is in the Feature branch that will be added to master branch
```sh
git merge Feature
```

#### Git Merging Scenario - 2
<img width="4720" height="3752" alt="8" src="https://github.com/user-attachments/assets/4b906433-5389-40de-a2fe-796da89f03e5" />


• **Initial Setup**: Start with main branch at commit C1, then C2 becomes the HEAD

• **Feature Branch Creation**: 
  - `git branch feature1` creates a new branch from C2
  - `git branch feature2` creates another branch from C2

• **Independent Development**:
  - Feature1 branch: Add commit CF1 with message "in f1"
  - Feature2 branch: Add commit CF2 with message "in f2"

• **Merging Process**:
  - **Step 1**: `git merge feature1` - Merges feature1 into main, HEAD moves to CF1
  - **Step 2**: HEAD moves to feature2 branch at CF2
  - **Step 3**: `git merge feature2` - Creates a new merge commit CF12 combining both features

• **Final State**: 
  - All features are integrated into main branch
  - CF12 represents the merged state containing changes from both feature branches
  - New updates can be added to the dashed box for future development

>This demonstrates a typical feature branch workflow where multiple developers work on separate features before merging them back into the main branch.


### Git conflicts
<img width="5614" height="3018" alt="9" src="https://github.com/user-attachments/assets/25d2152d-264b-4a21-8c86-28e4d5335016" />

• **Same Starting Point**: Both feature1 and feature2 branches created from commit C2

• **Conflicting Changes**:
- Feature1 modifies `file.txt` line 5: `"Hello World"`
- Feature2 modifies `file.txt` line 5: `"Hi Universe"`
- Both branches change the **same line** in the **same file**

• **First Merge Success**: `git merge feature1` works fine - no conflict yet

• **Second Merge Conflict**: `git merge feature2` fails because:
- Git can't decide which version to keep
- Same line has different content from two sources

• **Conflict Markers**: Git adds conflict markers to the file:
- `<<<<<<< HEAD` - Current branch content
- `=======` - Separator
- `>>>>>>> feature2` - Incoming branch content

• **Resolution Process**:
- **Edit** the file manually to choose/combine changes
- **Remove** conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
- **Stage** the resolved file: `git add file.txt`
- **Commit** the resolution: `git commit -m "Resolve merge conflict"`

• **Final Result**: Creates merge commit (CM) that successfully combines both branches with resolved conflicts

> Conflicts occur when the same lines in the same files are modified differently in both branches being merged.


### Git cherry-pick
Apart from using `git merge` we have another method to merge the other branches with `master`.

- check out to master branch:
```sh
git checkout master
```

- cherry-pick the commit id of a feature branch
```sh
git cherry-pick <any_feature_branch_commit_id_tobe_merged>
```
> Thus a new commit object is created in master branch from feature branch


#### Difference in cherry-pick and merge:
- If suppose you are working on `feature` branch and you did `4` commits, but 3 commits are ready and the 4th one is not.
- For a feature in your application, So you want to include the commit from 3rd commit into your `master` branch, you can use `cherry-pick` and use 3rd commit Id from feature branch and use it in master.
- But if you use `git merge` it will merge all the commits, simply the recent commit from `feature` branch into `master`.
- But in this case the 4th commit from the feature branch is not ready to be used in master.
- Hence we need to use `git cherry-pick`, Which we can use any commit id to be used in master.
> Cherry-Picking is like to pick any commit from a branch and merge it with master. But 'Merging' is like taking all the commits from a branch and merging it with master where master has history to all the commits linked from that branch, but cherry-pick doesn't.

> Best use of cherry-pick is when we have a feature commit ready and other is not, but it is required for updation.

### Connecting Local Repo to Remote Repo

- Remote Repositories:
	- GitHub
	- GitLab
	- BitBucket

> In a new project folder

- Adding a connection with remote repo
```sh
git remote add origin <url>
```

- After that, init-add-commit in your master branch, then push it to remote repo
```sh
git push -u origin <branch>
```
> After pushing you have a remote tracking branch created which tracks the remote repo and its contents - goes by `origin/<branch>`

#### Revisiting git log
- This command just refers to the local
```sh
git branch
```

- For remote branches
```sh
git branch -r
```

### Git Cloning
- Cloning a remote repository
```sh
git clone <repo_url.git>
```

- Get verbose about the remote
```sh
git remote -v
```

### Git Fetch and Pull

- Lets say your project has `master` which is local branch and `origin/master` which is remote tracking branch after you pushed into remote repo.

#### Fetch
- It is used to download the changes from remote repository to your local repository
```sh
git fetch
#or
git fetch origin
```

> *Note*: It will update only the remote tracking branches i.e `origin/master` but not added to `master`

For syncing the changes in master branch we do,
```sh
git checkout master 
# then
git merge origin/master
```

- To fetch changes of a certain branch which needed
```sh
git fetch origin <branch_name>
```

- If a project has multiple remote repositories like github, gitlab etc and to get all the changes of these in my local repo
```sh
git fetch --all
```

- Whichever remote tracking branches are deleted, it will remove from local also
```sh
git fetch --prune
```

#### Pull
 - `git pull` is nothing but the combination of 2 commands
	 - `git fetch`
	 - `git merge`

```sh
git pull
```

>*Note*: Use git pull only to the protected branches like 'master' , 'main'

> You can pull from any remote branch to any local branch just by using
```sh
git checkout <local_branch>
# and
git pull origin <remote_branch>
```

#### using rebase (optional)
- For a cleaner branch history

> git pull:
```sh
Before:
A---B---C (remote has commit C)
     \
      D (your local commit with new file)

After:
A---B---C (origin/main)
     \   \
      D---M (your main, M is merge commit)


>$ git pull --rebase

Before:
A---B---C (remote)
     \
      D (your local commit)

After:
A---B---C---D' (linear history, your commit moved on top)
```
### Git Stash

If you have a file in a branch which is already been tracked by git and later you have modified the file and its in staging area, Later you want to move to another branch without committing the changes, then git throws an error like `commit or stash the file as it will be overwritten`.
Hence we use `git stash` to keep store of  temporary changes in a space  until commit.

- Use git stash when:
	- when have modified changes and don't want to commit
	- moving to other branch with those modified changes

```sh
git stash list
# stash@{0}: WIP on master: 75b3f1d ok
```

- Returning back and want to apply those changes
```sh
git stash apply <stash_id> # stash@{0}
```

- Clear stash list
```sh
git stash clear
```

### .gitignore

Certain files and folders need not to be tracked, like `node_modules` `.env`

***Credits:** These git commands were compiled from the comprehensive tutorial by `Tech with Jatin` YouTube channel.*
