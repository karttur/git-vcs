---
layout: post
title: Local git control
categories: git
excerpt: "git control in local machine"
tags:
  - mac osx
  - git
  - local
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2020-02-16 11:27'
modified: '2020-11-12'
comments: true
share: true
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

**NOTE, 1 October 2020 the label 'master' was replaced by 'main' for the default repo branch.**

## Introduction

This post covers git Version Control System (VCS) for local machines. If you are a git (command line) newbie, you can view the Youtube [Introduction to Git - Core Concepts](https://www.youtube.com/watch?v=uR6G2v_WsRA&t=475s) by David Mahler or if you prefer to read, the blog post [Tutorial: Git for Absolutely Everyone](https://thenewstack.io/tutorial-git-for-absolutely-everyone/) by Michelle Gienow. This post is inspired by these two sources.

The tutorial series [Learn Git with Bitbucket Cloud](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud) is more in depth and links to relevant pages in that tutorial series are given in different sections below. The most comprehensive source available is probably the book [Pro Git - Everything you need to know about git](https://git-scm.com/book/en/v2/) by Scott Chacon and Ben Straub.

Every git project is bound to a repository (or repo for short) that always resides inside a directory (synonym: folder). A repo can either be a _master_ (~original) or a _clone_ (~copy). Unfortunately the term _master_ can mean several different things in git, but you just have to learn to live with it. The difference between an ordinary directory and a repository is that the repository contains a system of, usually hidden, files and folders that track, log and save changes in the directory. These hidden git files allow the use of the git toolbox for tracking, merging and restoring different versions of the documents in the working directory.

A repository consists of three “trees” maintained and tracked by git. The first tree is the _working directory_ which holds the "normal" files, it is like an ordinary directory with sub-directories and files. The second tree is the _index_ (or _staging_ area) and the third tree is the _head_ (or history) tree which contains all _commits_ and also points to the last _commited_ versions. _stage_ and _commit_ are git jargon, and you can picture them as meaning adding and locking (saving) changes. In git you can always go back to locked savings and explore who made what changes at which stage.

Two useful git commands for keeping track of what has been done and what is pending, are: <span class='terminalapp'>git log</span> or <span class='terminalapp'>git reflog</span> for tracking the git command history, and <span class='terminalapp'>git status</span> to see what is pending.

## Prerequisites

git must be installed, if you need assistance look at the post on [install git for command line](../git-commandline-install). This tutorial relies heavily on the <span class='app'>Terminal</span> command tool, if you are not acquainted with the command line, the [Command line crash course (pdf)](https://www.computervillage.org/articles/CommandLine.pdf) will teach you most things you need to know.

## Create directory with git control

Create a new, empty, directory on a local drive. To use the <span class='app'>Terminal</span>, first change directory (<span class='terminalapp'>cd</span>) to the parent folder under where you want to create the new directory.

<span class='terminal'>$ cd path/to/parent/directory</span>

You can also just type <span class='terminal'>$ cd</span> in the <span class='app'>Terminal</span> window, then open a <span class='app'>Finder</span> window, navigate the the parent folder and drag the directory icon of the parent to the <span class='app'>Terminal</span> window. Make sure the <span class='app'>Terminal</span> points towards the parent directory, and create the new directory with the <span class='terminalapp'>mkdir</span> command:

<span class='terminal'>$ mkdir git-test-dir</span>

<span class='terminalapp'>cd</span> to the newly created directory:

<span class='terminal'>$ cd git-test-dir</span>

## git Init

The default initialisation [command <span class='terminalapp'>git init</span>](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-init) converts a directory to a _master_ repository with a hidden <span class='file'>.git</span>  directory. _master_ repos are intended for development work, with the content typically _cloned_ by other users; edits made in _cloned_ repos, however, can not be _pushed_ back into the _master_. If you want a local _master_ repo that is more of a container and where development actually takes place in _clones_, then you should have a look in the parallel post on [Shared master local git control](../git-local-bare-master). For this post, with the _master_ being the site of development, turn your newly created directory into a repository:

<span class='terminal'>$ git init</span>

The response will be something like

```
Initialized empty git repository in path/to/git-test-dir/.git/
```

If you now check the content of your directory by listing (<span class='terminalapp'>ls</span>) its content:

<span class='terminal'>$ ls</span>

it will contain no (visible) file. If you instead use list all, <span class='terminalapp'>ls -a</span>, you will see the hidden <span class='file'>.git</span> folder:

<span class='terminal'>$ ls -a</span>

You can explore the <span class='file'>.git</span> folder by changing directory <span class='terminalapp'>cd</span> and, listing <span class='terminalapp'>ls</span>. But that is beyond this tutorial. All the version control and tracking that you do using git will be registered under the hidden directory.

Link to in depth exploration of [git init at Atlassian Bitbucket](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-init).

## Add a readme.md

A good practice is to add a _README_ file (they are commonly named with capitals to be noted) to any repository. It will typically be a simple text file (not from any word processor), or perhaps a _markdown_ (or md) file. The latter is a text file with some layout possibilities when published online for instance, but md files can also be just simple text files. Create a file called _README.md_ using the <span class='terminalapp'>touch</span> command.

<span class='terminal'>$ touch README.md</span>

Then you can use the command line editor <span class='terminalapp'>pico</span> to add some information:

<span class='terminal'>$ pico README.md</span>

```
Repo for project on ...
```

Hit [ctrl]+[X] to exit <span class='terminalapp'>pico</span> and save the edits by pressing <span class='terminal'>Y</span> when asked.

## Create a document

With your git repo (the directory with the hidden <span class='file'>.git</span> directory) setup, create a simple document, for instance a markdown file with an outline of chapters or sections. This first version is going to become your initial _master_. You can use <span class='terminalapp'>pico</span> without having created the file before:

<span class='terminal'>$ pico Chapters.md</span>

```
# Chapters in book on ...
...
```

## Clone I

The core idea of git is to have one established _master_ document (or project) and then share or work on copies (or clones). The differences between a _clone_ and the _master_ can then be reviewed and edits accepted or rejected (or edited further). Creating copies from the _master_ is done with the command <span class='terminalapp'>git clone _master-directory_ [_clone-directory_] </span>. If you omit the _clone-directory_ from the command, the clone will end up inside the _master-directory_:

<span class='terminal'>$ git clone path/to/git-test-dir</span>

You can only do this once, as the cloning process will detect that the sub-directory is not empty if you try to run the command more then once. Thus it is often better to explicitly state the target, or clone, where I prefer to give both a date and version for easier identification:

<span class='terminal'>$ git clone path/to/git-test-dir path/to/git-test-dir-YYYYMMDD-vX</span>

When you execute the <span class='terminalapp'>git clone</span> command you will get the following return at the <span class='app'>terminal</span> prompt:

<span class='terminal'>warning: You appear to have cloned an empty repository.<br>done.</span>

To actually get any content with the cloning you must add (_stage_ in the git jargon) the files (or folders) you want to clone, and then _commit_ to the changes, while also including a message.

Link to [Bitbucket Atlassian in depth page on git clone command](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-clone).

### git init + git clone

As an alternative to <span class='terminalapp'>git clone</span> into an empty (or non-existing) directory, you can create the git repo first, and then clone into this repo; the sequence of commands then become to create a directory, initialise git and then clone:

<span class='terminal'>$ mkdir git-clone-dir<br>$ cd git-clone-dir<br>$ git init<br>$ git clone path/to/git-test-dir</span>

### git remote

regardless of how you created your _clone_ repo, you can check its connections (which _master_ it is related to) by the <span class='terminalapp'>git remote</span> command:

<span class='terminal'>$ git remote \-v</span>

The response should be something like:
```
origin	path/to/git-test-dir (fetch)
origin	path/to/git-test-dir (push)
```
where _origin_ is the default alias given to the _master_ repo where _fetch_ and _push_ operates. _fetch_ and _push_ are git commands for retrieving and sending data, they will be explained below.

## Stage: git add

Make sure your <span class='app'>Terminal</span> window points to your original directory (_master_), then check the status:

<span class='terminal'>$ git status</span>

If you created the two markdown (<span class='file'>.md</span>) files outlined above, you should get the following response in the terminal window:

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	Chapters.md
	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

This tells us that we have two new (untracked) files that have not been added to the tracking process. In the git jargon, adding a file to the tracking process is called _staging_, but the command is <span class='terminalapp'>git add</span>. To _stage_ the two existing documents run the commands:

<span class='terminal'>$ git add Chapters.md<br>
$ git add README.md</span>

or to _stage_ all files in one go:

<span class='terminal'>$ git add \*</span>

or to _stage_ all files and folders:

<span class='terminal'>$ git add .</span>

It is enough to _stage_ a file once, you can then tell _commit_ to lock snapshots of all files that were ever _staged_ to be included. This will clarified in the next section.

More details on [Saving changes and git add, at Atlassian BitBucket](https://www.atlassian.com/git/tutorials/saving-changes).

## git commit

If you again run the <span class='terminal'>$ git status</span> command, you will see that the file names have changed color from red to green (not shown here, but in the terminal window), and that the message is different:

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   Chapters.md
	new file:   README.md
```

The message tells us how to _unstage_ a file (if we regret the _staging_), and that we have not done any _commits_, yet. To actually execute the _commit_ you need to give a message (summary is compulsory, extended is optional).

<span class='terminal'>$ git commit</span>

On Mac OSX this command opens the text editor <span class='terminalapp'>Vim</span>, and the terminal window will look similar to this (I have added my commit message below though):

```
\# Please enter the commit message for your changes. Lines starting
\# with '\#' will be ignored, and an empty message aborts the commit.
\#
\# On branch master
\#
\# Initial commit
\#
\# Changes to be committed:
\#       new file:   Chapters.md
\#       new file:   README.md
Initial commit \# One line summary added by me
\# Blank line separating and summary and full message
Initial commit for test project, 20200218 \# first line of full message
Only contains Chapters.md and README.md
~
~
```

Enter the message for your _commit_, typically you enter a one line summary, followed by a blank line and then the full message. The latter can be skipped. Writing informative summaries is an art, summarised(!) in the post [The Art of the Commit](https://alistapart.com/article/the-art-of-the-commit/) by David Demaree.

To exit <span class='terminalapp'>Vim</span> and save the edits, hold down the [SHIFT] key and type [ZZ] or [:wq!]. If you do not want to save and just exit, instead type [:q!]. if saving the edits, you should be returned to the ordinary terminal prompt, and get a message like this:

```
[master (root-commit) f8a8843] Initial commit
 2 files changed, 8 insertions(+)
 create mode 100644 Chapters.md
 create mode 100644 README.md
```

git identifies each unique commit by attaching a long (40 characters) hexadecimal number to every commit. Above, the code “f8a8843” constitutes the first 7 characters that identifies the commit.

If you again execute the command <span class='terminal'>$ git status</span>, the response should be similar to:

```
On branch master
nothing to commit, working tree clean
```

You can simplify the _commit_ process by adding the parameter -m followed by the message, and git will bypass the interactive message request:

<span class='terminal'>git commit \-m \"Initial commit\"</span>

#### git commit -a[m]

You can include all files that where ever _staged_ (added to the tracking system), bypassing the _staging_ by adding the parameter -a. However, this only affects files that have previously been _staged_. New files that have not been _staged_ are excluded.

<span class='terminal'>$ git commit \-a</span>

To both include the _staging_ (of all files with a history of _staging_) and the _commit_ message the command becomes:

<span class='terminal'>$ git commit \-am \"Initial commit\"</span>

More details can be found on [Atlassian BitBucket git commit page](https://www.atlassian.com/git/tutorials/saving-changes/git-commit).

## git checkout

In git jargon, a "checkout" switches between different versions of either files, commits or branches - you will encounter _checkout_ for several tasks. You can also use it to discard un-staged changes. Add some text to <span class='file'>Chapters.md</span>, you can use <span class='terminalapp'>pico</span> as shown above. Save the edits and then check status.

<span class='terminal'>$ git status</span>

```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   Chapters.md

no changes added to commit (use "git add" and/or "git commit -a")
```

git tells us to <span class='terminal'>use \"git checkout \-\- \<file\>...\" to discard changes in working directory</span>

So let us just do that and then check the status again:

<span class='terminal'>$ git checkout \-\- Chapters.md<br>$ git status</span>

```
On branch master
nothing to commit, working tree clean
```

If you open <span class='file'>Chapters.md</span> you will see that the line you added is not there. It has not been deleted, instead the working directory version of <span class='file'>Chapters.md</span> has been replaced with the latest _commited_ version.

## Undo stage (git reset)

Repeat the editing of the <span class='file'>Chapters.md</span> document (do **not** _checkout_), but this time _stage_ the file :

<span class='terminal'>$ git add Chapters.md</span>

<button id= "toggleStatus21" onclick="hiddencode('Status21')">Hide/Show git status response</button>

<div id="Status21" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   Chapters.md

```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

Try the command <span class='terminalapp'>git diff</span>:

<span class='terminal'>$ git diff</span>

It should return nothing, instead try:

<span class='terminal'>$ git diff --staged</span>

and you should see the difference between the _stage_ and the most recent _commit_, or put differently, the differnce between the stage tree and the history tree:
```
diff --git a/Chapters.md b/Chapters.md
index 611a054..fc92164 100644
--- a/Chapters.md
+++ b/Chapters.md
@@ -13,3 +13,5 @@
 ### Cloud changes 1970 - 2020

 ### Glacier changes 1970 -2020
+
+### dummy
```

To remove the _staged_ version of <span class='file'>Chapters.md</span> and replace it with the latest _commited_ version (called: _HEAD_, where _HEAD_ is rather a pointer) run the <span class='terminalapp'>git reset</span> command:

<span class='terminal'>$ git reset HEAD Chapters.md</span>

```
Unstaged changes after reset:
M	Chapters.md
```
<button id= "toggleStatus22" onclick="hiddencode('Status22')">Hide/Show git status response</button>

<div id="Status22" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   Chapters.md

no changes added to commit (use "git add" and/or "git commit -a")

```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

You can now proceed and check out <span class='file'>Chapters.md</span> as in the previous section.

<span class='terminal'>$ git checkout \-\- Chapters.md<br>$ git status</span>

## .gitignore

In the next section you are going to use the operating system (OS) for interfering in the working directory tree. This might leave traces from the OS, and git will acknowledge these changes as something to keep track of. To prevent that from happening you can create a <span class='file'>.gitignore</span> file, and in that file list the files, file types and folders that git should ignore:

<span class='terminal'>$ pico .gitignore</span>

I added the hidden Mac OSX file <span class='file'>.DS_store</span> and all kinds of <span class='file'>log</span> files and folders:
```
.DS_Store
*.log
log/
logs/
```
Hit [ctrl]+[X] to exit <span class='terminalapp'>pico</span> and save the edits by pressing <span class='terminal'>Y</span> when asked.

You have to _stage_ and _commit_ the <span class='file'>.gitignore</span> file:

<span class='terminal'>$ git add .gitignore<br>$ git commit \-m \".gitignore\"</span>

## Delete file with operating system

Remove (delete) the file <span class='file'>Chapters.md</span> from the working directory. If you want to use the command line then type:

<span class='terminal'>$ rm Chapters.md</span>

<button id= "toggleStatus23" onclick="hiddencode('Status23')">Hide/Show git status response</button>

<div id="Status23" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    Chapters.md

no changes added to commit (use "git add" and/or "git commit -a")
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

To _stage_ and _commit_ the changes in one command:

<span class='terminal'>$ git commit \-am \"deleting Chapters.md\"</span>

```
[master 41704db] deleting Chapters.md
 1 file changed, 15 deletions(-)
 delete mode 100644 Chapters.md
```

<button id= "toggleStatus24" onclick="hiddencode('Status24')">Hide/Show git status response</button>

<div id="Status24" style="display:none">

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

## Restore earlier/deleted file

To restore the deleted file <span class='file'>Chapters.md</span>, first check out the <span class='terminalapp'>git log</span> for this file:

<span class='terminal'>$ git log \-\- Chapters.md</span>

In the returned message we can see that the last changes we made to <span class='file'>Chapters.md</span> where in the _commit_ with the id starting "3f294b9":

```
commit 41704dbe25e51f3fc49302c171af55dcc6b38475 (HEAD -> master)
Author: Karttur <thomas.gumbricht@karttur.com>
Date:   Thu Feb 20 11:27:23 2020 +0100

    deleting Chapters.md

commit 3f294b9e27e9184cf17b0acf4d61268181d67fe8
Author: Karttur <thomas.gumbricht@karttur.com>
Date:   Tue Feb 18 15:22:07 2020 +0100

    chapters.md extended

commit f8a88432ba928f9e8e2f245f5c13306b9641443a
Author: Karttur <thomas.gumbricht@karttur.com>
Date:   Tue Feb 18 11:23:08 2020 +0100

    Initial commit

    Initial commit for test project, 20200218
    Only contains Chapters.md and README.md
```

To retrieve the last version of <span class='file'>Chapters.md</span> from the history tree, use the command <span class='terminalapp'>git checkout</span> introduced above:

<span class='terminal'>$ git checkout 3f294b9 \-\- Chapters.md</span>

<button id= "toggleStatus25" onclick="hiddencode('Status25')">Hide/Show git status response</button>

<div id="Status25" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   Chapters.md

```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

List (<span class='terminalapp'>ls</span>) the files in your working directory, and <span class='file'>Chapters.md</span> should be back. Because you only removed the file using the operating system (not git), git still has the file <span class='file'>Chapters.md</span> in the tracking system. Thus, to _commit_ the changes, simply type

<span class='terminal'>$ git commit \-m \"Restoring Chapters.md\"</span>

<button id= "toggleStatus26" onclick="hiddencode('Status26')">Hide/Show git status response</button>

<div id="Status26" style="display:none">

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

## git rm

The command <span class='terminalapp'>git rm</span> is the git equivalent of the operating system <span class='terminalapp'>rm</span>. The difference is that with <span class='terminalapp'>git rm</span> you also remove the file from the _staging_ tree.

<span class='terminal'>$ git rm Chapters.md</span>

<button id= "toggleStatus27" onclick="hiddencode('Status27')">Hide/Show git status response</button>

<div id="Status27" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    Chapters.md

```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

The removal is already staged, and you can _commit_:

<span class='terminal'>$ git commit \-m \"deleting Chapters.md\"</span>

Restore the file exactly as done above done above, and then:

<span class='terminal'>$ git checkout 3f294b9 \-\- Chapters.md</span>

<button id= "toggleStatus28" onclick="hiddencode('Status28')">Hide/Show git status response</button>

<div id="Status28" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   Chapters.md

```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

[git rm](https://www.atlassian.com/git/tutorials/undoing-changes/git-rm) manual on Atlassian Bitbucket.

## Clone II

You have now both _staged_ and _commited_ changes made to the _master_ repo. if you now run the <span class='terminalapp'>git clone</span> command, the target (or _clone_) directory will contain the _staged_ and _commited_ files (folders).  Note that you can not clone into a target directory that already exists as a clone, then you instead _pull_ the content from the _master_ - as explained in the next section. You can only clone into an empty target directory.

<span class='terminal'>$ git clone path/to/git-test-dir path/to/git-test-dir-YYYYMMDD-vY</span>

The message returned in the terminal window should now be something like:

```
Cloning into 'path/to/git-test-dir-YYYYMMDD-vY'...
done.
```

And if you explore the _clone_ it should contain identical copies of the two files in the _master_ repo.

### git pull

When you created your first _clone_ (in the section Clone I towards the beginning of this post) nothing was actually copied (or cloned) as you had not _staged_ and _commited_ any changes. To copy any changes made in the _master_ to an existing _clone_ also involves two steps: _fetch_ and _merge_. As explained on the [Atlassian Bitbucket in depth page on _git fetch_](https://www.atlassian.com/git/tutorials/syncing/git-fetch) _fetch_ is the 'safe' version of _pull_; it will not download files to working directory, only affect the history (_HEAD_) tree. To complete the update you must run _merge_ following _fetch_.

The _pull_ command combines _fetch_ and _merge_. Thus you can _pull_ any _staged_ and _commited_ changes by executing the <span class='terminalapp'>git pull</span> command from the _clone_. Technically <span class='terminalapp'>git pull</span> fetches the files (folders) from the _master_ (whether remote or local) to your _clone_ (or local) _working directory_ tree while also updating the history tree with the _pulled_ changes.

Change directory, <span class='terminalapp'>cd</span> to the _clone_ you want to update:

<span class='terminal'>$ cd path/to/git-test-dir-YYYYMMDD-vX</span>

and then execute the <span class='terminalapp'>git pull</span> command:

<span class='terminal'>$ git pull</span>

The terminal response should be something like:

```
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (4/4), done.
From /path/to/git-test-dir
 * [new branch]      master     -> origin/master
```

If you directly re-run the _pull_ command:

<span class='terminal'>$ git pull</span>

The response will state that you are update:

```
Already up to date.
```

If you now explore the initial _clone_, it also should contain identical copies of the two files in the _master_ repo.

Details on [git pull at Atlassian Bitbucket](https://www.atlassian.com/git/tutorials/syncing/git-pull).

## Edit _master_ and try clone again

Return to your _master_ repo and edit the <span class='file'>Chapters.md</span> document by adding a few more chapters to your project. You can use any text editor, including <span class ='terminalapp'>pico</span> or <span class ='terminalapp'>Vim.</span>. Save your edits.

In a terminal window, <span class='terminalapp'>cd</span> back to your _master_ repo (the directory "git-test-dir").

<button id= "toggleStatus01" onclick="hiddencode('Status01')">Hide/Show git status response</button>

<div id="Status01" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   Chapters.md

no changes added to commit (use "git add" and/or "git commit -a")
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>


First _stage_ the changes you made:

<span class='terminal'>$ git add Chapters.md</span>


<button id= "toggleStatus02" onclick="hiddencode('Status02')">Hide/Show git status response</button>

<div id="Status02" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   Chapters.md

```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

then _commit_ all added (-a) changes with a new message (-m):

<span class='terminal'>git commit \-am \"chapters.md extended\"</span>

```
[master 628ee9d] chapters.md extended
 1 file changed, 2 insertions(+)
 ```

<button id= "toggleStatus03" onclick="hiddencode('Status03')">Hide/Show git status response</button>

<div id="Status03" style="display:none">

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

With your edits in _master_ both _staged_ and _commited_, <span class='terminalapp'>cd</span> back to your clone (the directory "git-test-dir-YYYYMMDD-vX"), and execute  _pull_:

<span class='terminal'>$ git pull</span>

```
remote: Enumerating objects: 8, done.
remote: Counting objects: 100% (8/8), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 6 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (6/6), done.
From /path/to/git-test-dir
   f8a8843..628ee9d  master     -> origin/master
Updating f8a8843..628ee9d
Fast-forward
 Chapters.md | 6 ++++++
 1 file changed, 6 insertions(+)
```

Your local clone will now be updated to reflect the _master_. To check that out you can open the file "/path/to/git-test-dir-YYYYMMDD-vX/Chapters.md" to see that it contains the changes you made in the _master_ copy.

## Edit _master_ and _clone_, and clone again

To see what happens when you have edited both the _master_ and the _clone_, add a few more chapters to both copies of the document <span class='file'>Chapters.md</span> (i.e. in the _master_ and the _clone_ repos). Add different chapters in the two versions so that they are both changed and different.

Return to the _master_ repo and _stage_ and _commit_ the changes (as in the previous section). Then again try to _pull_ from the _clone_:

<span class='terminal'>$ git pull</span>

```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From /path/to/git-test-dir
   628ee9d..3f294b9  master     -> origin/master
Updating 628ee9d..3f294b9
error: Your local changes to the following files would be overwritten by merge:
	Chapters.md
Please commit your changes or stash them before you merge.
Aborting
```

As you see from the message returned in the Terminal window, the _pull_ was aborted because the changes you made to the _clone_ copy would be overwritten by the _master_ and lost. The solution is to analyse the differences with <span class='terminalapp'>git diff</span> and then fix them either manually or with <span class='terminalapp'>git stash</span>.

### git diff

When you have overlapping changes in both the _master_ and the _clone_ this result in conflicting differences. You will not be able to _pull_ or _push_ any changes before solving the differences. To analyse the difference, use the command <span class='terminalapp'>git diff</span>, in our example you should execute the command from the _clone_ repository:

<span class='terminal'>$ git diff</span>

```
diff --git a/Chapters.md b/Chapters.md
index dbda5d7..1c9c6a8 100644
--- a/Chapters.md
+++ b/Chapters.md
@@ -11,3 +11,5 @@
 ### Albedo changes 1970 - 2020

 ### Cloud changes 1970 - 2020
+
+### Arctic sea ice changes 1970 - 2020
```

As I edited the documents (<span class='file'>Chapters.md</span>), the lines "### Albedo changes 1970 - 2020" and "### Cloud changes 1970 - 2020" occur in the version residing in _master_, whereas the copy residing in the _clone_ lack these two lines but instead include the last (green fonted) line with a plus sign "+" in front (### Arctic sea ice changes 1970 - 2020).

If you try the <span class='terminalapp'>git status</span> command in the _clone_, you will get some hints:

<span class='terminal'>$ git status</span>

```
On branch master
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   Chapters.md

no changes added to commit (use "git add" and/or "git commit -a")
```

You have neither _staged_ nor _commited_ the changes that you did in the _clone_, and when you are trying <span class='terminalapp'>git pull</span> the un-commited changes prevent the edited _master_ file to be copied.

You can get more details by specifying what _diff_ you want to analyse:

<span class='terminal'>$ git diff --staged</span>

There is a lot more you can [analyse with <span class='terminalapp'>git diff</span>](https://www.atlassian.com/git/tutorials/saving-changes/git-diff). But the result above is sufficient for manually correcting (harmonizing or transferring all changes to one of the two copies). But you can also do it by _stashing_.

#### git stash

<span class='terminalapp'>git stash</span> temporarily shelves (or stashes) changes made in a repo and allows both to continue on other tasks and return to the stashed changes at a later time. You can solve the conflict with the two different versions in the previous section by _stashing_ your _clone_. Make sure your terminal window points to the _clone_ and execute:

<span class='terminal'>$ git stash</span>

```
Saved working directory and index state WIP on master: 628ee9d chapters.md extended
```

You can now apply the _pull_ command to get the changes in the _master_ repo to the _clone_:

<span class='terminal'>$ git pull</span>

```
Updating 628ee9d..3f294b9
Fast-forward
 Chapters.md | 2 ++
 1 file changed, 2 insertions(+)
```

If you look at the content of <span class='file'>Chapters.md</span> in your _clone_ it will (again) be identical to the original in _master_. But you have _stashed_ changes that you need to attend to. As your _clone_ is no longer identical to the _stashed_ version, there will be a conflict when you merge them. But the conflicting lines will be clearly marked and you need to manually edit the conflicts. To retrieve the _stashed_ version you can either choose to automatically delete (or pop) it after merging, or keep it for _stashing_ multiple times. To keep the _stash_:

<span class='terminal'>$ git stash apply</span>

and to delete it after applying:

<span class='terminal'>$ git stash pop</span>

In both cases you will get the same result reported in the terminal window:

```
Auto-merging Chapters.md
CONFLICT (content): Merge conflict in Chapters.md
```
If you open the copy of <span class='file'>Chapters.md</span> in your _clone_ you will see how git recorded the conflicts:

```
<<<<<<< Updated upstream
### Glacier changes 1970 -2020
=======
### Arctic sea ice changes 1970 - 2020
>>>>>>> Stashed changes
```

In our simple example we want the copy to include both the upstream update and the _stashed_ changes, so we simply delete the comments:

```
### Glacier changes 1970 -2020

### Arctic sea ice changes 1970 - 2020
```

and save the changes.

When you are finished you can drop the _stash_:

<span class='terminal'>$ git stash drop</span>

The [capabilities of git stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash) are much larger than outlined above.

## git push

The <span class='terminalapp'>git push</span> command is used to transfer data from a _clone_ repo to the _master_ repo (or from a local to a remote repo). In this post, however, we set up the _master_ as the primary development environment, and you will not be able to _push_ changes from the _clone_ to the _master_. To be able to do that you need to setup the _master_ while omitting the hidden working directory. How to go about creating such a git repository is the topic of the parallel post on [Shared master local git control](../git-local-bare-master).

## Resources

[A Visual Git Reference](http://marklodato.github.io/visual-git-guide/index-en.html) by M Lodatao (2010).

[Pro Git - Everything you need to know about git](https://git-scm.com/book/en/v2/) by Scott Chacon and Ben Straub (20200219).

[Learn Git with Bitbucket Cloud](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud)

Youtube tutorial [Introduction to Git - Core Concepts](https://www.youtube.com/watch?v=uR6G2v_WsRA&t=475s) by D. Mahler (20170621)

[Better understanding Git’s work flow in order to properly deal with merge conflicts — Part I](https://medium.com/@talgoldfus/better-understanding-gits-work-flow-in-order-to-properly-deal-with-merge-conflicts-part-i-760a366fc997) by Ted Goldfus (20160617)
