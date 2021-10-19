---
layout: post
title: "Managing superprojects"
categories: git
excerpt: Cloning, managing and updating superprojects and submodules
tags:
  - git
  - superproject
  - submodules
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2021-02-11'
modified: '2021-02-11'
comments: true
share: true
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

## Introduction

This post covers cloning, managing and updating superprojects containing linked submodules. It is a follow-up to the post [git submodules](../git-submodules).

## Prerequisits

You must have setup a git superproject with submodules as outlined in the post on [git submodules](../git-submodules).

## git submodule command suite

Managing submodules linked to repos that change (i.e. with _commited_ updates) is rather complex. Including a suite of special submodule commands (see [https://git-scm.com/docs/git-submodule](https://git-scm.com/docs/git-submodule)):

```
git submodule [--quiet] [--cached]
git submodule [--quiet] add [<options>] [--] <repository> [<path>]
git submodule [--quiet] status [--cached] [--recursive] [--] [<path>…​]
git submodule [--quiet] init [--] [<path>…​]
git submodule [--quiet] deinit [-f|--force] (--all|[--] <path>…​)
git submodule [--quiet] update [<options>] [--] [<path>…​]
git submodule [--quiet] set-branch [<options>] [--] <path>
git submodule [--quiet] set-url [--] <path> <newurl>
git submodule [--quiet] summary [<options>] [--] [<path>…​]
git submodule [--quiet] foreach [--recursive] <command>
git submodule [--quiet] sync [--recursive] [--] [<path>…​]
git submodule [--quiet] absorbgitdirs [--] [<path>…​]
```

### git submodule init

The <span class='terminalapp'>git submodule init</span> command prepares the local repo configuration for submodule processing. It is required for the more advanced submodule management, including as outlined in this post.

## Cloning a Project with Submodules

We will start the hands-on exercise by cloning a GitHub repo (superproject) with submodules. When you clone such a project, by default you get the directories that contain submodules, but none of the files within. To retrieve also the submodule content you must run two additional commands: <span class='terminalapp'>git submodule init</span> to initialize the local configuration, and <span class='terminalapp'>git submodule update</span> to fetch all the data for the submodules. Let us get started with the hands-on.

### Cloning Karttur's GeoImagine Framework

[Karttur's GeoImagine Framework](https://karttur.github.io/geoimagine/) is available as a git superproject at [GithHub.com](https://github.com/karttur/geoimagine02-framework). To clone the Framework, go the [GitHub repo](https://karttur.github.io/geoimagine/), click the green <span class="button">Code</span> button and select the **HTTPS** alternative. Click the copy sign to the right of the text to copy.

<figure>
<img src="../../images/github_clone_geoimagine02-framework.png">
<figcaption> Github - get code for cloning repo.</figcaption>
</figure>

Start a <span class='app'>Terminal</span> session and change directory (<span class='terminalapp'>cd</span>) to the parent folder where you want to clone the Framework. Write the command <span class='terminalapp'>git clone</span> and paste in the string that you copied from the online repo.

<span class='terminal'>$ git clone https://github.com/karttur/geoimagine02-framework.git</span>

The cloning will normally take only a few seconds, as you are only retrieving an empty shell at this stage.

<span class='terminalapp'>cd</span> in to the cloned repo:

<span class='terminal'>$ cd geoimagine02-framework</span>

If you inspect the content of the cloned repo, you will discover that the submodules are represented as empty directories. As outlined above, you need to run two commands to retrieve the submodules (run the commands from the top directory of the cloned superproject):

<span class='terminal'>$ git submodule init</span>

and

<span class='terminal'>$ git submodule update</span>

If you again inspect the content of the repo, you will find that the submodules are filled up with their respective original content. You can confirm that by:

<span class='terminal'>$ git status</span>

### Identify repo and branch of a submodule

<span class='terminalapp'>cd</span> to the submodule folder (not necessarily the same name as you can use an alias). Get the origin

<span class='terminal'>$ git remote show origin</span>

```
* remote origin
  Fetch URL: https://github.com/karttur/geoimagine02-setup_db
  Push  URL: https://github.com/karttur/geoimagine02-setup_db
  HEAD branch: main
  Remote branch:
    main tracked
  Local branch configured for 'git pull':
    main merges with remote main
  Local ref configured for 'git push':
    main pushes to main (local out of date)
```

or

<span class='terminal'>$ git config --get remote.origin.url</span>

```
https://github.com/karttur/geoimagine02-setup_db
```

## Pulling updates from remote repos to local submodules

With your favourite browser, go to the online repo and check it out. You can manually compare the files available, or the content of a particular file.

Having established that there are differences, try

<span class='terminal'>$ git status</span>

```
$ git status
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

To update your submodule, follow the instruction in the returned message and use <span class='terminalapp'>git pull</span> (or <span class='terminalapp'>git fetch</span> followed by <span class='terminalapp'>git merge</span>)

<span class ='terminal'>$ git pull</span>

```
t$ git pull
Updating a174fd9..4f8409e
Fast-forward
 __init__.py       |  9 +++++++--
 paramjson_mini.py | 81 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 setup_db_class.py | 84 +++++++++++-------------------------------------------------------------------------
 setup_db_main.py  | 15 ++++++++++++---
 version.py        |  4 +++-
 5 files changed, 114 insertions(+), 79 deletions(-)
 create mode 100644 paramjson_mini.py
 ```

 Return (<span class='terminalapp'>cd</span>) to the superproject top directory

 <span class='terminal'>$ git diff</span>

 ```
 $ git diff
diff --git a/karttur_v202003/geoimagine/setup_db b/karttur_v202003/geoimagine/setup_db
index a174fd9..4f8409e 160000
--- a/karttur_v202003/geoimagine/setup_db
+++ b/karttur_v202003/geoimagine/setup_db
@@ -1 +1 @@
-Subproject commit a174fd9ae93291e3ba22dd1843571b352be448dc
+Subproject commit 4f8409ee7d5b35c9b3cef5b3f3a007fdbcfd3324
```

or

<span class='terminal'>$ git status</span>

```
$ git status
On branch main
Your branch is up to date with 'origin/main'.
```

If you have many submodules, repeating this sequence takes time and then there is the risk of missing submodules in the process.

### Pulling all submodules with a single command I

You can at any time check if there are any updates to the origin repo (and branch) by combining the <span class='terminalapp'>foreach</span> command (outlines above) and <span class='terminalapp'>git status</span>

<span class='terminal'>$ git submodule foreach git status</span>

This will loop over all defined submodules and check the status against the origin of each individual submodule (**not** against the origin of the superproject). The loop reports the individual status for each submodule.

```
'''
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
...
...
Your branch is up to date with 'origin/main'.
...
```

If the command identifies only fast-forward mergers you can use another <span class='terminalapp'>foreach</span> command to pull all the changes made in the origins of each submodule

<span class='terminal'>$ git submodule foreach git pull</span>

or, if all submodules point towards the branch origin main

<span class='terminal'>$ git submodule foreach git pull origin main</span>

or use the special command

<span class='terminal'>$ git pull --recurse-submodules</span>.

The latter command has the advantage of also including nested submodules if any of the submodules in the repository have submodules themselves.

For later versions of git (1.8.2 or above) the recommendation is to use the command

<span class='terminal'>$ git submodule update --init --recursive</span>

the very first time you attempt an update, and then use:

<span class='terminal'>$ git submodule update --recursive</span>

Adding the option _\-\-remote_ has the added benefit of respecting any "non default" branches specified in the <span class='file'>.gitmodules</span> or <span class='file'>.git/config</span> files.

<span class='terminal'>$ git submodule update --recursive --remote</span>

### git status reporting detached HEAD

If after updating the status of the submodules

<span class='terminal'>$ git submodule foreach git status</span>

report detached HEAD, attach the HEAD to the main branch (or other branch) by the command:

<span class='terminal'>$ git submodule foreach git checkout main</span>

If your submodules are checked against differently named branches you have to manually go into each submodule and customize the checkout accordingly.

### Alternative to git status

In my setup, <span class=terminalapp>git status</span> does not always capture remote changes. To force a more in depth check on the difference between the superproject submodules and their respective remote repos, use the command:

<span class='terminal'>$ git submodule foreach git remote show origin</span>

```
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/user/repo
  Push  URL: https://github.com/user/repo
  HEAD branch: main
  Remote branch:
    main tracked
  Local branch configured for 'git pull':
    main merges with remote main
  Local ref configured for 'git push':
    main pushes to main (local out of date)
```

if the last line in the returned message says "(local out of date)", then you need to use one of the alternatives for pulling the changes to your local clone listed above.

## Push submodule changes

You can only push changes to a remote repo if you have the correct credentials. For your local clone you can always stage and commit any changes, but then your local clone will differ and any remote update will forge a 3-way merge. This section explains how to push changes to a remote repo that you control. It is done as for any other normal repo and thus this part contains nothing in particular on submodules.

### Push individual submodule (repo) content

The individual submodules constituting your superproject can only be altered from the cloned repo of the submodule (**not** from within the clone of the superproject repo). If you do not remember the original name of your submodule (as aliases are allowed) or which branch you are on, use the <span class='terminalapp'>git remote</span> command as explained above.

<span class='terminalapp'>cd</span> to the local repo that also constitute the submodules (**not** in the superproject, but the original repo attached as a submodule to the superproject). Check the status of the repo in the usual manner:

<span class='terminal'>$ git status</span>

```
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   karttur_v202003/geoimagine/setup_db (new commits)

no changes added to commit (use "git add" and/or "git commit -a")
```

To staga commit and push changes to the remote origin use the normal sequence of <span class='terminalapp'>git</span> commands:

<span class='terminal'>$ git add .
<br>$ git commit m \"updated this and that\"
<br>$ git push origin main</span>

The response from

<span class='terminal'>$ git status</span>

should be that your branch is up to date and that there is nothing to commit.

## Superproject update

Return the <span class='app'>Terminal</span> window to the top directory of the superproject. Check the status:

<span class='terminal'>$ git status</span>

```
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

the superproject as such is not updated and each submodule is linked to its origin repo by older commits. To update the superproject you must first pull the changes for each submodule, then you can stage, commit and push the superproject.

### Pull submodule changes to superproject local clone

Assuming that the original repos constituting the submodules of your superproject have been updated, you need to update the superproject itself. Each submodel of your superproject is attached to a specific commit in the original repo to which the submodule is linked. From the perspective of the submodule, the linked repo is the "origin". The next step is thus to pull the changes of each submodule within the superproject. Pull them to be updated to the latest commit of its "origin" repo and branch.

#### Pull changes to single submodule

Let us start by updating a single submodule. In your terminal window, return to the local superproject repo that contains the submodules, then to directory where you saved the submodule corresponding to the updated repo (note that it need not be the same name) and show the origin.

<span class='terminal'>$ cd [superproject]
<br>$ cd [path/to/submodule]
<br>$ git remote show origin</span>

```
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/karttur/geoimagine02-ktpandas
  Push  URL: https://github.com/karttur/geoimagine02-ktpandas
  HEAD branch: main
  Remote branch:
    main tracked
  Local branch configured for 'git pull':
    main merges with remote main
  Local ref configured for 'git push':
    main pushes to main (local out of date)
```

The last line tells you that _(local out of date)_, indicating that you need to pull updates from the remote origin.

If you use the ordinary <span class='terminalapp'>git status</span> command, the lagging behind in the local repo might not be seen.

```
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

To update your superproject, either use the recommended sequence of <span class='terminalapp'>git fetch</span> followed by <span class='terminalapp'>git merge</span>, or <span class='terminalapp'>git pull</span> that combines the two.

```
$ git pull
remote: Enumerating objects: 11, done.
remote: Counting objects: 100% (11/11), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 8 (delta 4), reused 8 (delta 4), pack-reused 0
Unpacking objects: 100% (8/8), done.
From https://github.com/karttur/geoimagine02-projects
   969abd5..f20aa92  main       -> origin/main
Updating 969abd5..f20aa92
Fast-forward
 __init__.py        |   5 +++-
 process_project.py |   9 ++++--
 projTRMM.py        |  22 ++++++++++++++
 proj_xml2json.py   | 197 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 4 files changed, 230 insertions(+), 3 deletions(-)
 create mode 100644 projTRMM.py
 create mode 100644 proj_xml2json.py
```

In the example above, 2 new files belonging to the submodule were added.

#### Status of superproject

Return to the top directory of the superproject.

<span class='terminal'>$ cd [superproject]</span>

and run the status check

<span class='terminal'>$ git status</span>

```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   karttur_v202003/geoimagine/projects (new commits)

no changes added to commit (use "git add" and/or "git commit -a")
```

You can get more in depth information by testing the difference (<span class='terminalapp'>git diff</span>)

<span class='terminal'>$ git diff</span>

#### Stage and commit

Your superproject is ready for being staged and committed, and the locked in changes pushed to origin.

<span class='terminal'>$ git add .
<br>$ git commit -m "submodule update"
<br>$ git push origin main</span>


<span class='terminal'>$ git log --all --decorate --oneline --graph</span>

```
$ git log --all --decorate --oneline --graph

$ git log --all --decorate --oneline --graph
* f20aa92 (HEAD -> main, origin/main, origin/HEAD) added projTRMM.py
* 664722e updated import structure
* 969abd5 initial commit
* ef57112 Initial commit
(base) Thomass-Air:projects thomasgumbricht$ pwd
/Users/thomasgumbricht/GitHub/geoimagine02-framework/karttur_v202003/geoimagine/projects
```

### Pulling all submodules with a single command II

Instead of updating single submodules, you can pull all submodules with a single commands as described above. Note, however, that the <span class='terminalapp'>git status</span> command does not work, but you can instead combine the <span class='terminalapp'>git remote show origin</span> with <span class='terminalapp'>git submodule foreach</span>:

<span class='terminal'>$ git submodule foreach git remote show origin</span>

if the command reveals any "(local out of date)", just execute

<span class='terminal'>$ git submodule foreach git pull</span>

or any of the other alternatives achieving similar effects listed above (Pulling all submodules with a single command I).

Remember that you have to stage, commit and push the superproject itself, but that is the same regardless of how many submodules you updated.

## Resources

[Git Tools - Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

[Submodules at Atlassian Bitbucket](https://www.atlassian.com/git/tutorials/git-submodule)

[Pro Git - Everything you need to know about git](https://git-scm.com/book/en/v2/) by Scott Chacon and Ben Straub (20200219).

[Fork a github repo that has submodule?](https://www.reddit.com/r/git/comments/qatxi/fork_a_github_repo_that_has_submodule/)

[Git Submodules basic explanation](https://gist.github.com/gitaarik/8735255)
