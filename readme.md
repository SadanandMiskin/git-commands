
## Table of Contents

- [What is git?](#what-is-git?)
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


### What is git?
- Git is a distributed version control system that tracks versions of files. It is often used to control source code by programmers who are developing software collaboratively.

### # Setting up the local git
```sh
git config --global user.name "Your Name"
git config --global user.email "youremail@mail.com"
```

###  # Git Init:
```bash
git init
# or
git init --inital-branch <branch_name>
```

### # Adding files to the staging area
```bash
git add .
#####
git add <file_name>
```

### # Remove file from staging area (Undo It)
```bash
git rm --cached <file_name1> <file_name2>  #... other files
```

- To remove the .git folder
```bash
rm -rf .git
```

### # Status of the current commit:
```
git status
```

- -> Git does not track empty folders, only if some files are present

- Verbose (detailed info)
-> ```bash
```bash
git status -v # detailed
# or
git status -s # short
```

### # Commit
```bash
git commit -m "<message>"
```
-> Now git is tracking the files which are committed, means which were in staging area before commit command.

<img width="2213" height="793" alt="1" src="https://github.com/user-attachments/assets/ee69a4a4-b2c5-4e2b-ae4c-cdf74e213f78" />


#### Head:
-> Points to the latest commit on the branch
-> We can always change the position of the head.

<img width="2113" height="821" alt="2" src="https://github.com/user-attachments/assets/8a31405d-8371-4a8b-8e3f-248807d500bd" />


-> If you had made changes in the ***tracked file*** only then you don't need to add first in *staging area*  by using `add` and `commit` separate commands.
```bash
git commit -a -m "<commit_message>"
```


#### # Modifying the existing commit

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
> -> If ever I want to test my CI pipeline over Jenkins etc. .Without creating a new commit or modifying one just creating an empty commit to trigger and test my CI pipeline.\


### # Git Logs

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


### # Git reset

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

### # Git Revert
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

### # Branches

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


![7](https://github.com/user-attachments/assets/476db53a-c704-41d4-9e4d-5e6b577d1402)<svg xmlns="http://www.w3.org/2000/svg" version="1.1" height="870.5px" width="935.9999999999999px" viewBox="-10 -10 955.9999999999999 890.5" content="&lt;mxGraphModel dx=&quot;1553&quot; dy=&quot;1338&quot; grid=&quot;1&quot; gridSize=&quot;10&quot; guides=&quot;1&quot; tooltips=&quot;1&quot; connect=&quot;1&quot; arrows=&quot;1&quot; fold=&quot;1&quot; page=&quot;0&quot; pageScale=&quot;1&quot; pageWidth=&quot;850&quot; pageHeight=&quot;1100&quot; math=&quot;0&quot; shadow=&quot;0&quot;&gt;&lt;root&gt;&lt;mxCell id=&quot;0&quot;/&gt;&lt;mxCell id=&quot;1&quot; parent=&quot;0&quot;/&gt;&lt;mxCell id=&quot;4&quot; style=&quot;edgeStyle=none;html=1;strokeColor=#007FFF;&quot; parent=&quot;1&quot; source=&quot;3&quot; edge=&quot;1&quot;&gt;&lt;mxGeometry relative=&quot;1&quot; as=&quot;geometry&quot;&gt;&lt;mxPoint x=&quot;240&quot; y=&quot;120&quot; as=&quot;targetPoint&quot;/&gt;&lt;Array as=&quot;points&quot;/&gt;&lt;/mxGeometry&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;3&quot; value=&quot;&amp;lt;font style=&amp;quot;font-size: 36px;&amp;quot;&amp;gt;C1&amp;lt;/font&amp;gt;&quot; style=&quot;ellipse;whiteSpace=wrap;html=1;aspect=fixed;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;200&quot; y=&quot;-50&quot; width=&quot;80&quot; height=&quot;80&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;5&quot; style=&quot;edgeStyle=none;html=1;strokeColor=#007FFF;&quot; parent=&quot;1&quot; source=&quot;6&quot; edge=&quot;1&quot;&gt;&lt;mxGeometry relative=&quot;1&quot; as=&quot;geometry&quot;&gt;&lt;mxPoint x=&quot;240&quot; y=&quot;290&quot; as=&quot;targetPoint&quot;/&gt;&lt;Array as=&quot;points&quot;/&gt;&lt;/mxGeometry&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;6&quot; value=&quot;&amp;lt;font style=&amp;quot;font-size: 36px;&amp;quot;&amp;gt;C2&amp;lt;/font&amp;gt;&quot; style=&quot;ellipse;whiteSpace=wrap;html=1;aspect=fixed;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;200&quot; y=&quot;120&quot; width=&quot;80&quot; height=&quot;80&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;23&quot; style=&quot;edgeStyle=none;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;shape=arrow;strokeColor=#00B55B;&quot; parent=&quot;1&quot; source=&quot;8&quot; edge=&quot;1&quot;&gt;&lt;mxGeometry relative=&quot;1&quot; as=&quot;geometry&quot;&gt;&lt;mxPoint x=&quot;240&quot; y=&quot;610&quot; as=&quot;targetPoint&quot;/&gt;&lt;/mxGeometry&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;8&quot; value=&quot;&amp;lt;font style=&amp;quot;font-size: 36px;&amp;quot;&amp;gt;C3&amp;lt;/font&amp;gt;&quot; style=&quot;ellipse;whiteSpace=wrap;html=1;aspect=fixed;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;200&quot; y=&quot;290&quot; width=&quot;80&quot; height=&quot;80&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;9&quot; value=&quot;&quot; style=&quot;verticalLabelPosition=bottom;html=1;verticalAlign=top;align=center;strokeColor=none;fillColor=#00BEF2;shape=mxgraph.azure.cloud_services_configuration_file;pointerEvents=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;330&quot; y=&quot;-35&quot; width=&quot;47.5&quot; height=&quot;50&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;10&quot; value=&quot;&quot; style=&quot;verticalLabelPosition=bottom;html=1;verticalAlign=top;align=center;strokeColor=none;fillColor=#00BEF2;shape=mxgraph.azure.cloud_services_configuration_file;pointerEvents=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;330&quot; y=&quot;135&quot; width=&quot;47.5&quot; height=&quot;50&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;11&quot; value=&quot;&quot; style=&quot;verticalLabelPosition=bottom;html=1;verticalAlign=top;align=center;strokeColor=none;fillColor=#00BEF2;shape=mxgraph.azure.cloud_services_configuration_file;pointerEvents=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;400&quot; y=&quot;135&quot; width=&quot;47.5&quot; height=&quot;50&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;12&quot; value=&quot;&quot; style=&quot;verticalLabelPosition=bottom;html=1;verticalAlign=top;align=center;strokeColor=none;fillColor=#00BEF2;shape=mxgraph.azure.cloud_services_configuration_file;pointerEvents=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;330&quot; y=&quot;300&quot; width=&quot;47.5&quot; height=&quot;50&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;13&quot; value=&quot;&quot; style=&quot;verticalLabelPosition=bottom;html=1;verticalAlign=top;align=center;strokeColor=none;fillColor=#00BEF2;shape=mxgraph.azure.cloud_services_configuration_file;pointerEvents=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;400&quot; y=&quot;300&quot; width=&quot;47.5&quot; height=&quot;50&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;15&quot; style=&quot;edgeStyle=none;html=1;strokeColor=#0000FF;&quot; parent=&quot;1&quot; source=&quot;14&quot; edge=&quot;1&quot;&gt;&lt;mxGeometry relative=&quot;1&quot; as=&quot;geometry&quot;&gt;&lt;mxPoint x=&quot;20&quot; y=&quot;490&quot; as=&quot;targetPoint&quot;/&gt;&lt;mxPoint x=&quot;20&quot; y=&quot;600&quot; as=&quot;sourcePoint&quot;/&gt;&lt;/mxGeometry&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;14&quot; value=&quot;&amp;lt;font style=&amp;quot;font-size: 36px;&amp;quot;&amp;gt;CF&amp;lt;/font&amp;gt;&quot; style=&quot;ellipse;whiteSpace=wrap;html=1;aspect=fixed;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;-20&quot; y=&quot;610&quot; width=&quot;80&quot; height=&quot;80&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;17&quot; style=&quot;edgeStyle=none;html=1;exitX=0.5;exitY=0;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;strokeColor=#0000CC;&quot; parent=&quot;1&quot; source=&quot;16&quot; target=&quot;8&quot; edge=&quot;1&quot;&gt;&lt;mxGeometry relative=&quot;1&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;16&quot; value=&quot;&amp;lt;font style=&amp;quot;font-size: 36px;&amp;quot;&amp;gt;C3&amp;lt;/font&amp;gt;&quot; style=&quot;ellipse;whiteSpace=wrap;html=1;aspect=fixed;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;-20&quot; y=&quot;410&quot; width=&quot;80&quot; height=&quot;80&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;18&quot; value=&quot;&quot; style=&quot;shape=singleArrow;whiteSpace=wrap;html=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;90&quot; y=&quot;290&quot; width=&quot;100&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;19&quot; value=&quot;&amp;lt;h2&amp;gt;Master&amp;lt;/h2&amp;gt;&quot; style=&quot;text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=#00CC66;fillColor=#FFFFFF;shadow=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;-10&quot; y=&quot;290&quot; width=&quot;80&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;20&quot; value=&quot;&quot; style=&quot;verticalLabelPosition=bottom;html=1;verticalAlign=top;align=center;strokeColor=none;fillColor=#00BEF2;shape=mxgraph.azure.cloud_services_configuration_file;pointerEvents=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;-183.75&quot; y=&quot;560&quot; width=&quot;47.5&quot; height=&quot;50&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;21&quot; value=&quot;&amp;lt;h2&amp;gt;feature&amp;lt;/h2&amp;gt;&quot; style=&quot;text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=#00CC66;fillColor=#FFFFFF;shadow=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;-210&quot; y=&quot;620&quot; width=&quot;80&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;22&quot; value=&quot;&quot; style=&quot;shape=singleArrow;whiteSpace=wrap;html=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;-120&quot; y=&quot;625&quot; width=&quot;100&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;24&quot; value=&quot;&quot; style=&quot;ellipse;whiteSpace=wrap;html=1;aspect=fixed;strokeColor=#FF9933;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;200&quot; y=&quot;610&quot; width=&quot;80&quot; height=&quot;80&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;25&quot; value=&quot;&quot; style=&quot;shape=singleArrow;whiteSpace=wrap;html=1;rotation=-180;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;290&quot; y=&quot;620&quot; width=&quot;100&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;26&quot; value=&quot;&amp;lt;h2&amp;gt;Master&amp;lt;/h2&amp;gt;&quot; style=&quot;text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=#00CC66;fillColor=#FFFFFF;shadow=1;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;400&quot; y=&quot;620&quot; width=&quot;80&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;27&quot; value=&quot;My App&quot; style=&quot;sketch=0;pointerEvents=1;shadow=0;dashed=0;html=1;strokeColor=none;labelPosition=center;verticalLabelPosition=bottom;verticalAlign=top;outlineConnect=0;align=center;shape=mxgraph.office.concepts.folder;fillColor=#2072B8;&quot; parent=&quot;1&quot; vertex=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;210&quot; y=&quot;-180&quot; width=&quot;60&quot; height=&quot;50&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;28&quot; value=&quot;&amp;lt;font style=&amp;quot;font-size: 36px;&amp;quot;&amp;gt;git branch feature&amp;lt;/font&amp;gt;&quot; style=&quot;text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=#009999;fillColor=#00CC00;&quot; vertex=&quot;1&quot; parent=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;-330&quot; y=&quot;390&quot; width=&quot;300&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;29&quot; value=&quot;&amp;lt;font style=&amp;quot;font-size: 36px;&amp;quot;&amp;gt;git switch master&amp;lt;br&amp;gt;&amp;lt;/font&amp;gt;&quot; style=&quot;text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=#009999;fillColor=#00CC00;&quot; vertex=&quot;1&quot; parent=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;305&quot; y=&quot;420&quot; width=&quot;290&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;mxCell id=&quot;30&quot; value=&quot;&amp;lt;font style=&amp;quot;font-size: 36px;&amp;quot;&amp;gt;git merge feature&amp;lt;/font&amp;gt;&quot; style=&quot;text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=#009999;fillColor=#00CC00;&quot; vertex=&quot;1&quot; parent=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;305&quot; y=&quot;510&quot; width=&quot;300&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;/root&gt;&lt;/mxGraphModel&gt;"><style type="text/css"></style><path d="M 570.5 210 L 570.5 293.63" fill="none" stroke="#007fff" stroke-miterlimit="10" pointer-events="none"/><path d="M 570.5 298.88 L 567 291.88 L 570.5 293.63 L 574 291.88 Z" fill="#007fff" stroke="#007fff" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="570.5" cy="170" rx="40" ry="40" fill="rgb(255, 255, 255)" stroke="rgb(0, 0, 0)" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 78px; height: 1px; padding-top: 170px; margin-left: 532px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: normal; overflow-wrap: normal;"><font style="font-size: 36px;">C1</font></div></div></div></foreignObject></g><path d="M 570.5 380 L 570.5 463.63" fill="none" stroke="#007fff" stroke-miterlimit="10" pointer-events="none"/><path d="M 570.5 468.88 L 567 461.88 L 570.5 463.63 L 574 461.88 Z" fill="#007fff" stroke="#007fff" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="570.5" cy="340" rx="40" ry="40" fill="rgb(255, 255, 255)" stroke="rgb(0, 0, 0)" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 78px; height: 1px; padding-top: 340px; margin-left: 532px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: normal; overflow-wrap: normal;"><font style="font-size: 36px;">C2</font></div></div></div></foreignObject></g><path d="M 565.5 550 L 575.5 550 L 575.5 760 L 585.5 760 L 570.5 790 L 555.5 760 L 565.5 760 Z" fill="none" stroke="#00b55b" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="570.5" cy="510" rx="40" ry="40" fill="rgb(255, 255, 255)" stroke="rgb(0, 0, 0)" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 78px; height: 1px; padding-top: 510px; margin-left: 532px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: normal; overflow-wrap: normal;"><font style="font-size: 36px;">C3</font></div></div></div></foreignObject></g><path d="M 660.5 150.76 C 660.85 147.72 663.23 145.33 666.23 145 L 698.14 145 L 708 154.95 L 708 188.79 C 707.83 192.12 705.23 194.79 701.95 195 L 666.23 195 C 663.27 194.68 660.91 192.37 660.5 189.38 Z M 664.38 188.48 C 664.38 189.69 665.22 190.72 666.38 190.96 L 702.03 190.96 C 703.16 190.7 703.97 189.67 703.97 188.48 L 703.97 157.27 L 696 157.27 L 696 149.09 L 666.38 149.09 C 665.31 149.36 664.54 150.3 664.48 151.41 Z M 672.95 161.01 L 694.95 161.01 L 694.95 165.15 L 672.95 165.15 Z M 672.95 168.89 L 694.95 168.89 L 694.95 173.03 L 672.95 173.03 Z M 672.95 176.77 L 694.95 176.77 L 694.95 180.96 L 672.95 180.96 Z" fill="#00bef2" stroke="none" pointer-events="none"/><path d="M 660.5 320.76 C 660.85 317.72 663.23 315.33 666.23 315 L 698.14 315 L 708 324.95 L 708 358.79 C 707.83 362.12 705.23 364.79 701.95 365 L 666.23 365 C 663.27 364.68 660.91 362.37 660.5 359.38 Z M 664.38 358.48 C 664.38 359.69 665.22 360.72 666.38 360.96 L 702.03 360.96 C 703.16 360.7 703.97 359.67 703.97 358.48 L 703.97 327.27 L 696 327.27 L 696 319.09 L 666.38 319.09 C 665.31 319.36 664.54 320.3 664.48 321.41 Z M 672.95 331.01 L 694.95 331.01 L 694.95 335.15 L 672.95 335.15 Z M 672.95 338.89 L 694.95 338.89 L 694.95 343.03 L 672.95 343.03 Z M 672.95 346.77 L 694.95 346.77 L 694.95 350.96 L 672.95 350.96 Z" fill="#00bef2" stroke="none" pointer-events="none"/><path d="M 730.5 320.76 C 730.85 317.72 733.23 315.33 736.23 315 L 768.14 315 L 778 324.95 L 778 358.79 C 777.83 362.12 775.23 364.79 771.95 365 L 736.23 365 C 733.27 364.68 730.91 362.37 730.5 359.38 Z M 734.38 358.48 C 734.38 359.69 735.22 360.72 736.38 360.96 L 772.03 360.96 C 773.16 360.7 773.97 359.67 773.97 358.48 L 773.97 327.27 L 766 327.27 L 766 319.09 L 736.38 319.09 C 735.31 319.36 734.54 320.3 734.48 321.41 Z M 742.95 331.01 L 764.95 331.01 L 764.95 335.15 L 742.95 335.15 Z M 742.95 338.89 L 764.95 338.89 L 764.95 343.03 L 742.95 343.03 Z M 742.95 346.77 L 764.95 346.77 L 764.95 350.96 L 742.95 350.96 Z" fill="#00bef2" stroke="none" pointer-events="none"/><path d="M 660.5 485.76 C 660.85 482.72 663.23 480.33 666.23 480 L 698.14 480 L 708 489.95 L 708 523.79 C 707.83 527.12 705.23 529.79 701.95 530 L 666.23 530 C 663.27 529.68 660.91 527.37 660.5 524.38 Z M 664.38 523.48 C 664.38 524.69 665.22 525.72 666.38 525.96 L 702.03 525.96 C 703.16 525.7 703.97 524.67 703.97 523.48 L 703.97 492.27 L 696 492.27 L 696 484.09 L 666.38 484.09 C 665.31 484.36 664.54 485.3 664.48 486.41 Z M 672.95 496.01 L 694.95 496.01 L 694.95 500.15 L 672.95 500.15 Z M 672.95 503.89 L 694.95 503.89 L 694.95 508.03 L 672.95 508.03 Z M 672.95 511.77 L 694.95 511.77 L 694.95 515.96 L 672.95 515.96 Z" fill="#00bef2" stroke="none" pointer-events="none"/><path d="M 730.5 485.76 C 730.85 482.72 733.23 480.33 736.23 480 L 768.14 480 L 778 489.95 L 778 523.79 C 777.83 527.12 775.23 529.79 771.95 530 L 736.23 530 C 733.27 529.68 730.91 527.37 730.5 524.38 Z M 734.38 523.48 C 734.38 524.69 735.22 525.72 736.38 525.96 L 772.03 525.96 C 773.16 525.7 773.97 524.67 773.97 523.48 L 773.97 492.27 L 766 492.27 L 766 484.09 L 736.38 484.09 C 735.31 484.36 734.54 485.3 734.48 486.41 Z M 742.95 496.01 L 764.95 496.01 L 764.95 500.15 L 742.95 500.15 Z M 742.95 503.89 L 764.95 503.89 L 764.95 508.03 L 742.95 508.03 Z M 742.95 511.77 L 764.95 511.77 L 764.95 515.96 L 742.95 515.96 Z" fill="#00bef2" stroke="none" pointer-events="none"/><path d="M 350.5 790 L 350.5 676.37" fill="none" stroke="#0000ff" stroke-miterlimit="10" pointer-events="none"/><path d="M 350.5 671.12 L 354 678.12 L 350.5 676.37 L 347 678.12 Z" fill="#0000ff" stroke="#0000ff" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="350.5" cy="830" rx="40" ry="40" fill="rgb(255, 255, 255)" stroke="rgb(0, 0, 0)" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 78px; height: 1px; padding-top: 830px; margin-left: 312px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: normal; overflow-wrap: normal;"><font style="font-size: 36px;">CF</font></div></div></div></foreignObject></g><path d="M 350.5 590 L 524.68 512.59" fill="none" stroke="#0000cc" stroke-miterlimit="10" pointer-events="none"/><path d="M 529.48 510.45 L 524.5 516.5 L 524.68 512.59 L 521.66 510.1 Z" fill="#0000cc" stroke="#0000cc" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="350.5" cy="630" rx="40" ry="40" fill="rgb(255, 255, 255)" stroke="rgb(0, 0, 0)" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 78px; height: 1px; padding-top: 630px; margin-left: 312px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: normal; overflow-wrap: normal;"><font style="font-size: 36px;">C3</font></div></div></div></foreignObject></g><path d="M 420.5 491 L 500.5 491 L 500.5 470 L 520.5 500 L 500.5 530 L 500.5 509 L 420.5 509 Z" fill="rgb(255, 255, 255)" stroke="rgb(0, 0, 0)" stroke-miterlimit="10" pointer-events="none"/><rect x="320.5" y="470" width="80" height="60" fill="#000000" stroke="#000000" pointer-events="none" transform="translate(2,3)" opacity="0.25"/><rect x="320.5" y="470" width="80" height="60" fill="#ffffff" stroke="#00cc66" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 1px; height: 1px; padding-top: 500px; margin-left: 361px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: nowrap;"><h2>Master</h2></div></div></div></foreignObject></g><path d="M 146.75 745.76 C 147.1 742.72 149.48 740.33 152.48 740 L 184.39 740 L 194.25 749.95 L 194.25 783.79 C 194.08 787.12 191.48 789.79 188.2 790 L 152.48 790 C 149.52 789.68 147.16 787.37 146.75 784.38 Z M 150.63 783.48 C 150.63 784.69 151.47 785.72 152.63 785.96 L 188.28 785.96 C 189.41 785.7 190.22 784.67 190.22 783.48 L 190.22 752.27 L 182.25 752.27 L 182.25 744.09 L 152.63 744.09 C 151.56 744.36 150.79 745.3 150.73 746.41 Z M 159.2 756.01 L 181.2 756.01 L 181.2 760.15 L 159.2 760.15 Z M 159.2 763.89 L 181.2 763.89 L 181.2 768.03 L 159.2 768.03 Z M 159.2 771.77 L 181.2 771.77 L 181.2 775.96 L 159.2 775.96 Z" fill="#00bef2" stroke="none" pointer-events="none"/><rect x="120.5" y="800" width="80" height="60" fill="#000000" stroke="#000000" pointer-events="none" transform="translate(2,3)" opacity="0.25"/><rect x="120.5" y="800" width="80" height="60" fill="#ffffff" stroke="#00cc66" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 1px; height: 1px; padding-top: 830px; margin-left: 161px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: nowrap;"><h2>feature</h2></div></div></div></foreignObject></g><path d="M 210.5 826 L 290.5 826 L 290.5 805 L 310.5 835 L 290.5 865 L 290.5 844 L 210.5 844 Z" fill="rgb(255, 255, 255)" stroke="rgb(0, 0, 0)" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="570.5" cy="830" rx="40" ry="40" fill="rgb(255, 255, 255)" stroke="#ff9933" pointer-events="none"/><path d="M 620.5 821 L 700.5 821 L 700.5 800 L 720.5 830 L 700.5 860 L 700.5 839 L 620.5 839 Z" fill="rgb(255, 255, 255)" stroke="rgb(0, 0, 0)" stroke-miterlimit="10" transform="rotate(-180,670.5,830)" pointer-events="none"/><rect x="730.5" y="800" width="80" height="60" fill="#000000" stroke="#000000" pointer-events="none" transform="translate(2,3)" opacity="0.25"/><rect x="730.5" y="800" width="80" height="60" fill="#ffffff" stroke="#00cc66" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 1px; height: 1px; padding-top: 830px; margin-left: 771px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: nowrap;"><h2>Master</h2></div></div></div></foreignObject></g><path d="M 572.32 6.66 L 579.2 0.81 C 579.83 0.31 580.56 0.01 581.43 0 L 596.95 0 C 598.57 0 600.5 1.24 600.5 3.37 L 600.5 6.66 Z M 540.5 50 L 540.5 12.41 C 540.5 10.57 542.29 9.17 544.02 9.17 L 600.5 9.17 L 600.5 50 Z" fill="#2072b8" stroke="none" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe flex-start; justify-content: unsafe center; width: 1px; height: 1px; padding-top: 57px; margin-left: 571px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: nowrap;">My App</div></div></div></foreignObject></g><rect x="0.5" y="570" width="300" height="60" fill="#00cc00" stroke="#009999" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 1px; height: 1px; padding-top: 600px; margin-left: 151px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: nowrap;"><font style="font-size: 36px;">git branch feature</font></div></div></div></foreignObject></g><rect x="635.5" y="600" width="290" height="60" fill="#00cc00" stroke="#009999" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 1px; height: 1px; padding-top: 630px; margin-left: 781px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: nowrap;"><font style="font-size: 36px;">git switch master<br /></font></div></div></div></foreignObject></g><rect x="635.5" y="690" width="300" height="60" fill="#00cc00" stroke="#009999" pointer-events="none"/><g><foreignObject pointer-events="none" width="100%" height="100%" style="overflow: visible; text-align: left;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 1px; height: 1px; padding-top: 720px; margin-left: 786px;"><div data-drawio-colors="color: rgb(0, 0, 0); " style="box-sizing: border-box; font-size: 0px; text-align: center;"><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; pointer-events: none; white-space: nowrap;"><font style="font-size: 36px;">git merge feature</font></div></div></div></foreignObject></g></svg>

- To merge any branch to the master branch, first we need to be in master branch
```sh
git switch master
```

- Then use merge command, Whatever the commit is in the Feature branch that will be added to master branch
```sh
git merge Feature
```

#### # Git Merging Scenario - 2
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


### # Git conflicts
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


### # Git cherry-pick
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


#### # Difference in cherry-pick and merge:
- If suppose you are working on `feature` branch and you did `4` commits, but 3 commits are ready and the 4th one is not.
- For a feature in your application, So you want to include the commit from 3rd commit into your `master` branch, you can use `cherry-pick` and use 3rd commit Id from feature branch and use it in master.
- But if you use `git merge` it will merge all the commits, simply the recent commit from `feature` branch into `master`.
- But in this case the 4th commit from the feature branch is not ready to be used in master.
- Hence we need to use `git cherry-pick`, Which we can use any commit id to be used in master.
> Cherry-Picking is like to pick any commit from a branch and merge it with master. But 'Merging' is like taking all the commits from a branch and merging it with master where master has history to all the commits linked from that branch, but cherry-pick doesn't.

> Best use of cherry-pick is when we have a feature commit ready and other is not, but it is required for updation.

### # Connecting Local Repo to Remote Repo

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

#### # Revisiting git log
- This command just refers to the local
```sh
git branch
```

- For remote branches
```sh
git branch -r
```

### # Git Cloning
- Cloning a remote repository
```sh
git clone <repo_url.git>
```

- Get verbose about the remote
```sh
git remote -v
```

### # Git Fetch and Pull

- Lets say your project has `master` which is local branch and `origin/master` which is remote tracking branch after you pushed into remote repo.

#### # Fetch
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

#### # Pull
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

#### # using rebase (optional)
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
```

>git pull --rebase
```sh
Before:
A---B---C (remote)
     \
      D (your local commit)

After:
A---B---C---D' (linear history, your commit moved on top)
```
### # Git Stash

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

### # .gitignore

Certain files and folders need not to be tracked, like `node_modules` `.env`

***Credits:** These git commands were compiled from the comprehensive tutorial by `Tech with Jatin` YouTube channel.*
