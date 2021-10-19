---
layout: post
title: git branches
categories: git
excerpt: "git branches in local repositories"
tags:
  - mac osx
  - git
  - local
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2020-02-18 11:27'
modified: '2020-11-12'
comments: true
share: true
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

**NOTE, 1 October 2020 the label 'master' was replaced by 'main' for the default repo branch.**

## Introduction

This post covers git branches. The main references for this post are the [Atlassian Bitbucket pages on git branches](https://www.atlassian.com/git/tutorials/using-branches) and the Youtube [Introduction to Git - Branching and Merging](https://www.youtube.com/watch?v=FyAAIHHClqI) by David Mahler.

## Prerequisites

This tutorial is a continuation of my posts on [Local git control](../git-local-use) and [Shared master local git control](../git-local-bare-master).

## Create new repo

The post on [Local git control](../git-local-use) introduced git repositories (repos) as directories with added functions. Open a <span class='app'>Terminal</span> window and navigate to the parent folder where you want to create your repository for this hands-on tutorial, then execute the following commands:

<span class='terminal'>$ mkdir git-test-repo<br>$ cd git-test-repo<br>$ git init<br>$ pico README.md</span>

and add a message to the <span class='file'>README.md</span> file:

```
Test repository for branching
```
Hit [ctrl]+[X] to exit <span class='terminalapp'>pico</span> and save the edits by pressing <span class='terminal'>Y</span> when asked.

Add another text file, for example <span class='file'>Chapters.md</span> (<span class='terminal'>$ pico Chapters.md</span>), add some lines of text and save and exit. In my example I added the following text:

```
## Ch1 The Sun

## Ch2 Milakovitch cycles

## Ch3 Physical and biogeochimcal climate forcing
```

Also add a <span class='file'>.gitignore</span> file:

<span class='terminal'>$ pico .gitignore</span>
```
.DS_Store
*.log
log/
logs/
```
and then save and exit.

If you list all (<span class='terminalapp'>ls -a</span>) the content of your _working directory_ (the file system in your repo):

<span class='terminal'>$ ls -a</span>

 you should see four entries
 ```
 .git		.gitignore	Chapters.md	README.md
 ```

 Both <span class='file'>.git</span> and <span class='file'>.gitignore</span> are hidden (each start with a dot ".").

## git status

Run a <span class='terminalapp'>git status</span> command:

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	Chapters.md
	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

You have "No commits yet", and all of the added content is in red (not shown above though) - they have not been _staged_ either.

## git stage

_stage_ (add to the tracking system) all of the content in your repo, followed by a status check:

<span class='terminal'>$ git add .<br>$ git status</span>

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   .gitignore
	new file:   Chapters.md
	new file:   README.md

```

The color of the file names is now green (not shown above), git is telling us that files are ready to be _commited_, or how to go about to _unstage_.

## git commit

When you _commit_ something in git, you lock in the changes, like taking a snapshot. The status of your entire (_staged_) project will be stored (in the history, or _HEAD_ tree), and you can always go back to this exact content as long as your repo exists. To commit all files and folders that have, at any tine, been _staged_:

<span class='terminal'>$ git commit \-am \"initial commit\"</span>

```
[master (root-commit) 51a7ae7] initial commit
 3 files changed, 10 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 Chapters.md
 create mode 100644 README.md
 ```

 The hexadecimal code in the return message (51a7ae7) is the identifier from the _commit_ (or snapshot) history tree. If you want to return to the versions of the included (_staged_) files in this snapshot you refer to this code. The details for how to do that is covered in the [parallel post on Local git control](../git-local-use).

### git branch

When you created the git repo (with the command <span class='terminalapp'>git init</span>) the default branch _master_ was created. And all the operations so far has been reated to the _master_ branch. git keeps track of which branch is active at any particular time through a pointer called _HEAD_ in the history tree. _HEAD_ follows the active branch (not the _commit_) and is called a symbolic pointer. Each branch have its own pointer, that points at the most recent _commit_ for that branch. This means that _HEAD_ indirectly, via the branch pointer, also points at the overall latest _commit_.

All changes that you do in the working directory tree, like ordinary editing, writing, copying, deleting etc, have no effect on any branch. Neither does _staging_ effect branches. It is only when you _commit_ changes that you attach the _staged_ changes to the active branch.

To list all the _branches_ in your repo:

<span class='terminal'>$ git branch</span>
```
* master
```
The branch your are working with is indicate by a \*, in our case there is only one branch _master_.

To create a new branch:

<span class='terminal'>$ git branch b001</span>

If you then again use the basic _branch_ command:

<span class='terminal'>$ git branch</span>

You should now have a second branch.
```
* master
  new-branch
```

### git log

The <span class='terminalapp'>git log</span> command return details on your _commits_ and _branches_:

<span class='terminal'>$ git log</span>

```
commit 51a7ae7af28bb6264df12fbb7e1df9dc5673326f (HEAD -> master, b001)
Author: Karttur <thomas.gumbricht@karttur.com>
Date:   Thu Feb 20 13:52:25 2020 +0100

    initial commit
```

The first returned line tells us the full hexadecimal of the latest commit and towards where the _HEAD_ pointer is looking (HEAD -> master, b001). As _HEAD_ points both towards _master_ and _b001_ we are at the bifurcation point of the branching, but nothing is _commited_ after the branching. You also get information on who did this and when, and the message that went with the _commit_. The new branch that we added have not yet had any commits and contains nothing.

A more condensed alternative is:

<span class='terminal'>$ git reflog</span>

```
51a7ae7 (HEAD -> master, b001) HEAD@{0}: commit (initial): initial commit
```

And then you can add different parameters to the <span class='terminalapp'>git log</span> command, for instance:

<span class='terminal'>$ git log \-\-all \-\-decorate \-\-oneline \-\-graph</span>

For now this command generates a condensed summary:

```
* 51a7ae7 (HEAD -> master, b001) initial commit
```

But it will give a more illustrative picture of the bracnhes once we start working with them.

### git checkout and switch branch

In git jargon, a "checkout" switches between different versions of either files, commits or branches - it was used in the post on [Local git control](../git-local-use) to restore deleted files. It can also be used for "checking out" from one branch and "checking in" on another. The [Atlassian Bitbucket page on git checkout](https://www.atlassian.com/git/tutorials/using-branches/git-checkout) explains in detail. To switch from the current branch, _master_ in our case, to the b001 branch we just created:

<span class='terminal'>$ git checkout b001</span>

```
Switched to branch 'b001'
```

<button id= "toggleBranch01" onclick="hiddencode('Branch01')">Hide/Show git branch response</button>

<div id="Branch01" style="display:none">

{% capture text-capture %}
{% raw %}
```
  master
* b001
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

<span class='terminal'>$ git reflog</span>
```
51a7ae7 (HEAD -> b001, master) HEAD@{0}: checkout: moving from master to b001
51a7ae7 (HEAD -> b001, master) HEAD@{1}: commit (initial): initial commit
```

The only thing that has actually happened in the repo is that  _HEAD_ now points towards the branch b001.

#### git branch and checkout combined

You can use the _checkout_ command to create a new branch on the fly:

<span class='terminal'>$ git checkout -b b002</span>

```
Switched to a new branch 'b002'
```

This will create a third branch, b002, with the same bifucation point from _master_ as b001.

<button id= "toggleBranch02" onclick="hiddencode('Branch02')">Hide/Show git branch response</button>

<div id="Branch02" style="display:none">

{% capture text-capture %}
{% raw %}
```
b001
* b002
master
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Create new document

While in the branch b002 create a new markdown document:

<span class='terminal'>$ pico ch1-The-Sun.md</span>

and add some notes in it. Save the edits, exit and return to the command line. Check the status:

<span class='terminal'>$ git status</span>

```
On branch b002
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	ch1-The-Sun.md

nothing added to commit but untracked files present (use "git add" to track)

```

Temporarily change back to your _master_ branch and try the _status_ command:

<span class='terminal'>$ git checkout master<br>$ git status</span>

And the message will be the same! It is only when _commited_ that any changes get attached to the active branch.

Return to the branch you were in when editing the file "ch1-The-Sun.md".

<span class='terminal'>$ git checkout b002</span>

#### stage and commit to branch

With _HEAD_ pointing towards branch b002, _stage_ and _commit_ the changes:

<span class='terminal'>$ git add .<br>git commit \-m \"created ch1-The-Sun.md\"</span>

```
[b002 d6e4774] created ch1-The-Sun.md
 1 file changed, 14 insertions(+)
 create mode 100644 ch1-The-Sun.md
```

<button id= "toggleStatus31" onclick="hiddencode('Status31')">Hide/Show git status response</button>

<div id="Status31" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch b002
nothing to commit, working tree clean
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Edit existing document

_checkout_ to change branch to b001:

<span class='terminal'>$ git checkout b001</span>

```
Switched to branch 'b001'
```

Open the document <span class='file'>Chapters.md</span> and add or edit the text:

<span class='terminal'>$ pico Chapters.md</span>

```
...
## Add another chapter
```
Save and exit, and then _commit_ (the document is already _staged_ just add the parameter -a to _commit_):

<span class='terminal'>$ git commit \-am \"added chapter in Chapters.md\"</span>

```
[b001 878d2a9] added chapter in Chapters.md
 1 file changed, 2 insertions(+)
```

Make sure you have a clean working trees and nothing to commit by:

<span class='terminal'>$ git status</span>

```
On branch b001
nothing to commit, working tree clean
```

### git log

First try:

<span class='terminal'>$ git reflog</span>

which returns an extended summary of the recent events:

```
878d2a9 (HEAD -> b001) HEAD@{0}: commit: added chapter in Chapters.md
51a7ae7 (master) HEAD@{1}: checkout: moving from b002 to b001
d6e4774 (b002) HEAD@{2}: commit: created ch1-The-Sun.md
51a7ae7 (master) HEAD@{3}: checkout: moving from b001 to b002
51a7ae7 (master) HEAD@{4}: checkout: moving from master to b001
51a7ae7 (master) HEAD@{5}: commit (initial): initial commit
```

Then try the condensed, graphical alternative introduced above:

<span class='terminal'>$ git log \-\-all \-\-decorate \-\-oneline \-\-graph</span>
```
* 878d2a9 (HEAD -> b001) added chapter in Chapters.md
| * d6e4774 (b002) created ch1-The-Sun.md
|/  
* 51a7ae7 (master) initial commit
```

In the response to the last command, you can see that we have three different branches with unique content (from bottom to top):

+ commit-id:51a7ae7; branch: master; message: "initial commit"
+ commit-id:d6e4774; branch: b002; message: "created ch1-The-Sun.md"
+ commit-id:878d2a9; branch: b001; message: "added chapter in Chapters.md"

The _HEAD_ pointer shows that b001 is the singularly active branch.

With b001 as the active branch type:

<span class ='terminal'>$ cat Chapters.md</span>

```
## Ch1 The Sun

## Ch2 Milakovitch cycles

## Ch3 Physical and biogeochimcal climate forcing

## Add another chapter
```
You can see that the line added ("## Add another chapter") in the _commit_ related to b001 (878d2a9) is included. If you check out to b002 and run the same command:

<span class ='terminal'>$ git checkout b002<br>$ cat Chapters.md</span>

```
## Ch1 The Sun

## Ch2 Milakovitch cycles

## Ch3 Physical and biogeochimcal climate forcing
```
The added line is **not** there.

## git merge

Merging is the git process of joining a bifurcation back into the mainstream. In our example we have three unique _branches_, a _master_ and the two branch-offs b001 and b002 that bifurcated at the same _commit_ but contain different edits. Both b001 and b002 are "ahead of" _master_. We need to _merge_ both side branches back into _master_. The _merge_ command is at the core of git, but also requires some more careful considerations, the [Atlassian Bitbucket reference of git merge](https://www.atlassian.com/git/tutorials/using-branches/git-merge) will probably come in handy.

### Fast-Forward merge

A Fast-Forward merge happens when the _master_ pointer still points at the _commit_ that was the bifurcation point of the branch to merge with _master_. All that is actually needed in such a case is to "fast forward" the _master_ pointer to the pointer of the branch to merge. If you look at the log list above, the _HEAD_ pointer is to b001 and the b001 pointer is towards the _commit_ 878d2a9. Thus if you execute the command <span class='terminal'>$ git merge</span> on these two branches (with _master_ the receiving branch) all that is required is for _master_ to catch up with b001.

Execute <span class='terminalapp'>git status</span> to ensure that _HEAD_ is pointing to the correct merge-receiving branch (_master_ in our case). If needed, execute <span class='terminalapp'>git checkout \<receiving\></span> command  to switch to the receiving branch.

To explore what the differences are between _master_ and b001 and use <span class='terminalapp'>git diff</span>:

<span class='terminal'>$ git checkout master<br>$ git diff master..b001</span>

```
diff --git a/Chapters.md b/Chapters.md
index a80f1ec..01338d4 100644
--- a/Chapters.md
+++ b/Chapters.md
@@ -3,3 +3,5 @@
 ## Ch2 Milakovitch cycles

 ## Ch3 Physical and biogeochimcal climate forcing
+
+## Add another chapter
```
The response tells us that the line "## Add another chapter" will be added to the file <span class='file'>Chapters.md</span>. Execute the merge:

<span class='terminal'>$ git merge b001 \-m \"merging b0001 to master\"></span>

```
Updating 51a7ae7..878d2a9
Fast-forward (no commit created; -m option ignored)
 Chapters.md | 2 ++
 1 file changed, 2 insertions(+)
```

The response reports "Fast-forward" and that as a consequence "no commit created; -m option ignored". Confirm the content of <span class='file'>Chapters.md</span>:

<span class='terminal'>$ cat Chapters.md</span>

It should now contain the added line ("## Add another chapter"). Also check the difference betwen _master_ and b001:

<span class='terminal'>$ git diff master..b001</span>

### git branch \-\-merged

Before deleting the branch that is now merged with _master_ you can test the command <span class='terminalapp'>git branch \-\-merged</span> that reports branches that are integrated into _master_:

<span class='terminal'>$ git branch \-\-merged</span>

```
  b001
* master
```

You can now safely delete branch b001:

<span class='terminal'>git branch -d b001</span>

```
Deleted branch b001 (was 878d2a9).
```

If you look above (or execute log command), you will see that the _commit_ 878d2a9 is where _master_ is now pointing.

### 3-way merge

Check the branching of your repo by executing the command:

<span class='terminal'>$ git log \-\-all \-\-decorate \-\-oneline \-\-graph</span>

```
* 878d2a9 (HEAD -> master) added chapter in Chapters.md
| * d6e4774 (b002) created ch1-The-Sun.md
|/  
* 51a7ae7 initial commit
```

The result is very similar to before the merging of b001 into _master_. The only difference is that _HEAD_ now points at _master_. And that is exactly the result of the Fast Forward merge we just did. But as both remaining branches (_master_ and b002) contain edits compared to the bifurcation point, we can not merge these two using Fast Forward. Instead you have to perform a 3-way merge.

Make sure you are in the _master_ branch:

<span class='terminal'>$ git status</span>

```
On branch master
nothing to commit, working tree clean
```

Execute the merge:

<span class='terminal'>$ git merge b002</span>

As you did not give a message (-m) git suggests a default message, either edit or accept it. The _merge_ will complete with a new message:

```
Merge made by the 'recursive' strategy.
 ch1-The-Sun.md | 14 ++++++++++++++
 1 file changed, 14 insertions(+)
 create mode 100644 ch1-The-Sun.md
```

The reported result tell us that "Merge made by the 'recursive' strategy." Check out the graphical representation of what happened:

<span class='terminal'>$ git log \-\-all \-\-decorate \-\-oneline \-\-graph</span>

```
*   5a76be7 (HEAD -> master) Merge branch 'b002'
|\  
| * d6e4774 (b002) created ch1-The-Sun.md
* | 878d2a9 added chapter in Chapters.md
|/  
* 51a7ae7 initial commit
```

Check the merge status:

<span class='terminal'>$ git branch \-\-merged</span>

```
  b001
* master
```

It is safe to delete branch b002:

<span class='terminal'>git branch \-d b001</span>

## Merge conflicts

Merge conflicts appear when the same text (or line) in the file has been edited in multiple copies of the same file. In this section you will create a new branch (b003) and make (conflicting) edits to <span class='file'>Chapters.md</span> in both _master_ and b003:

<span class='terminal'>$ git checkout \-b b003<br>$ pico Chapters.md</span>

Edit the last chapter title:
```
## Add another chapter -> ## Ch4 Clouds and water vapor
```

_commit_ the changes to b003:

<span class='terminal'>$ git commit \-am \"b003 edits to Chapters.md\"</span>

_checkout_ to _master_ and make a different edit to <span class='file'>Chapters.md</span>:

<span class='terminal'>$ git checkout master<br>$ pico Chapters.md</span>

```
## Add another chapter -> ## Ch4 Sea ice and ocean color
```

_commit_ the changes to _master_:

<span class='terminal'>$ git commit \-am \"master edits to Chapters.md\"</span>

Check out the flow of _commits_ for our branches:

<span class='terminal'>$ git log \-\-all \-\-decorate \-\-oneline \-\-graph</span>

```
* ba31829 (HEAD -> master) master edits to Chapters.md
| * 79d1120 (b003) b003 edits to Chapters.md
|/  
*   5a76be7 Merge branch 'b002'
```

To merge b003 into _master_ requires a 3-way merge. Make sure you are in the _master_ branch and then start the merge:

<span class='terminal'>$ git merge b003</span>

```
Auto-merging Chapters.md
CONFLICT (content): Merge conflict in Chapters.md
Automatic merge failed; fix conflicts and then commit the result.
```

This merge could not be automatically solved, and a conflict is reported. First execture a <span class='terminalapp'>git status</span>:

<span class='terminal'>$ git status</span>

```
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   Chapters.md

no changes added to commit (use "git add" and/or "git commit -a")
```

<span class='terminalapp'>git status</span> gives us an escape route, to abort the merge with the command <span class='terminalapp'>git merge \-\-abort</span>. But git has also, behind the scenes, prepared the working directory version of <span class='file'>Chapters.md</span> to highlight the conflicts for you. Thus open the file and inspect the conflicts:

<span class='terminal'>$ pico Chapters.md</span>

```
## Ch1 The Sun

## Ch2 Milakovitch cycles

## Ch3 Physical and biogeochimcal climate forcing

<<<<<<< HEAD
## Ch4 Sea ice and ocean color
=======
## Ch4 Clouds and water vapor
>>>>>>> b003
```

The lines above and below the equal signs ("=======") are where you find conflicts. Above are the edits from the _HEAD_ pointer (pointing to the receiving branch) and below are the edits from the branch to merge. You can edit this file, while also removing the git markers, for example like this:

```
## Ch1 The Sun

## Ch2 Milakovitch cycles

## Ch3 Physical and biogeochimcal climate forcing

## Ch4 Clouds and water vapor
```

When you are satisfied, _stage_ the edited file:

<span class='terminal'>$ git add Chapters.md</span>

<button id= "toggleStatus41" onclick="hiddencode('Status41')">Hide/Show git status response</button>

<div id="Status41" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   Chapters.md

```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

_commit_ the changes, if you do not give a message (-m) git suggests a default, or you can enter a message interactively:

<span class='terminal'>$ git commit</span>

<button id= "toggleStatus42" onclick="hiddencode('Status42')">Hide/Show git status response</button>

<div id="Status42" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
nothing to commit, working tree clean
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

<span class='terminal'>$ git log \-\-all \-\-decorate \-\-oneline \-\-graph</span>

```
*   f3a0b75 (HEAD -> master) Merge branch 'b003'
|\  
| * 79d1120 (b003) b003 edits to Chapters.md
* | ba31829 master edits to Chapters.md
|/  
*   5a76be7 Merge branch 'b002'
```

You can safely delete b003:

<span class='terminal'>git branch -d b003</span>

## Resources

Youtube tutorial [Introduction to Git - Branching and Merging](https://www.youtube.com/watch?v=FyAAIHHClqI) by David Mahler (20170918)

[Atlassian Bitbucket pages on git branches](https://www.atlassian.com/git/tutorials/using-branches)
