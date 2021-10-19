---
layout: post
title: git submodules
categories: git
excerpt: "Creating git submodules"
tags:
  - submodules
  - git
  - remote repository
  - clone
  - fork
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2020-02-22 11:27'
modified: '2020-11-12'
comments: true
share: true
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

**NOTE, 1 October 2020 the label 'master' was replaced by 'main' for the default repo branch.**

## Introduction

Karttur's GeoImagine Framework available at [Github](https://github.com/karttur) is built from an (almost) empty "superproject" repository with around 40 individual repositories attached as submodules. Each submodule (or repo) contains a customised Python package. Together all the submodules constitute the complete Framework. This post explains the git concept of submodules, and details how Karttur's GeoImagine Framework is organised online.

The tutorial on [Submodules at Atlassian Bitbucket](https://www.atlassian.com/git/tutorials/git-submodule) contains all the essentials on git submodules. The [reference page for git submodule](https://git-scm.com/docs/git-submodule) is more barebone. [Pro Git - Everything you need to know about git](https://git-scm.com/book/en/v2/) by Scott Chacon and Ben Straub is the most complete reference source.

If you are primarily interested in git submodules I recommend a more general instruction, like [Using submodules in Git - Tutorial](https://www.vogella.com/tutorials/GitSubmodules/article.html), [Git Submodules basic explanation](https://gist.github.com/gitaarik/8735255), [Working with submodules](https://github.blog/2016-02-01-working-with-submodules/) or the youtube introduction [Git Tutorial: All About Submodules](https://www.youtube.com/watch?v=8Z4Cmhji_FQ).

## Prerequisits

You need to install git for command line use as outlined in the parallel post on [Local git control](../git-local-use). You do not need a  [GitHub account](https://github.com) to follow this post unless you want to try to manage changes between an online account linked as a submodule to a local superproject.

## Working with submodules

Submodules are repositories that are linked to a superproject repository. All that is required is to setup links to other repos in the hidden file <span class='file'>.gitmodules</span> in the root of the superproject. The submodule(s) as such can be located at any url. <span class='terminalapp'>git</span> supports adding, updating, synchronizing, and cloning submodules. This post is rather a barebone manual for how to create a superproject with non-complicated submodules. How to update and manage superprojects is the topic of the post [Managing superproject](../git-managing-superproject).

### Create three repos

To test the function of submodules, create three directories and initiate <span class='terminalapp'>git</span> in each of them:

- container-repo
- ch01-introduction
- ch02-the_sun

Change directory (<span class='terminalapp'>cd</span>) to the parent folder where you want to create your test repos, then execute the following commands:

<span class='terminal'>$ mkdir container-repo
<br>$ cd container-repo
<br>$ git init
<br>$ cd ..
<br>$ mkdir ch01-introduction
<br>$ cd ch01-introduction
<br>$ git init
<br>$ cd ..
<br>$ mkdir ch02-the_sun
<br>$ cd ch02-the_sun
<br>$ git init
<br>$ cd ..
</span>

### Create README and content

Continuing our example with chapters in a book, let us use the _container-repo_ as a superproject with only a README file:

<span class='terminal'>$ cd container-repo
<br>$ pico README.md</span>
```
## Parent repository for chapters in book on ...

This repo is an (almost) empty container linking to other repos (submodules) as defined in the hidden .gitmodules files.
```

Hit [ctrl]+[X] to exit <span class='terminalapp'>pico</span> and save the edits by pressing <span class='terminal'>Y</span> when asked.

_stage_ and _commit_ the _container-repo_ and return to the parent folder.

<span class='terminal'>$ git add .
<br>$ git commit \-am \"Initial commit\"
<br>$ cd ..
</span>

The submodules in this example are the chapters in the book. To avoid keeping track of the chapters except as submodules, the directories are named as the chapters, but with the prefix "ch-nn" to also force a correct ordering.

Add a README and another md file with the chapter title and an outline of the content to each chapter repo, for example:

<span class='terminal'>$ cd ch01-introduction
<br>$ pico README.md</span>
```
## Chapter 1 in book on ...
```
Hit [ctrl]+[X] to exit <span class='terminalapp'>pico</span> and save the edits by pressing <span class='terminal'>Y</span> when asked.

<span class='terminal'>$ pico ch01-introduction.md</span>
```
## Introduction

Narrative with a personal touch, skip long pieces on what you wanted to do. Just write what you did.
```
Hit [ctrl]+[X] to exit <span class='terminalapp'>pico</span> and save the edits by pressing <span class='terminal'>Y</span> when asked.

_stage_ and _commit_ the changes made to _ch01-introduction_ and return to the parent folder.

<span class='terminal'>$ git add .
<br>$ git commit \-am \"Initial commit\"
<br>$ cd ..
</span>

Repeat the same steps for the 2nd chapter. The content of the markdown (md) files does not matter, as long as they contain some text.

### Add submodule

The command for adding a submodule requires only the source path (or SSH or HTTPS path if you are adding an online repo). Optionally you can state the local path in the receiving repo (i.e. the superproject). If no receiving path is given the submodule will be saved in the root of the receiving repo under its original name, <span class='terminalapp'>git submodule add source_path [receiving_path]</span>.

#### Add local repo as submodule

As our receiving repo (_container-repo_) and the submodules (_ch01-xx_ and _ch02-yy_) are saved in parallel directories, you can give the path using the relative syntax: _../ch01-xx_ etc. Thus, to add the local repo _ch01-introduction_ as a submodule at the root of the repo _container-repo_ without changing its name:

<span class='terminal'>$ cd container-repo<br>
$ git submodule add ../ch01-introduction</span>

<button id= "toggleStatus01" onclick="hiddencode('Status01')">Hide/Show Git status response</button>

<div id="Status01" style="display:none">

If you run the <span class='terminalapp'>git status</span> command, you will see that the submodule is added to the root. Also the file <span class='file'>.gitmodules</span> has been added. This file contains the link between the submodule and its origin.
{% capture text-capture %}
{% raw %}

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   .gitmodules
	new file:   ch01-introduction

```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

or if you want to save the submodule under a predefined path in the receiving repo:

<span class='terminal'>$ git submodule add ../ch02-the_sun submodules/ch02-sun</span>

Check the status of your superproject:

<span class='terminal'>$ git status</span>

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   .gitmodules
	new file:   ch01-introduction
	new file:   submodules/ch02-sun

```

The first submodule that you added (_ch01-introduction_) is located directly under the root; the second submodule (_ch02-sun_) under the path _/submodules/_ (and with the submodule having a different name comparaed to the source repo). Then also the file <span class='file'>.gitmodules</span> has been added. This file contains the link between the submodules and their origin:

```
[submodule "ch01-introduction"]
        path = ch01-introduction
        url = ../ch01-introduction
[submodule "submodules/ch02-sun"]
        path = submodules/ch02-sun
        url = ../ch02-the_sun
```

_stage_ and _commit_ the local submodules to save them in your history tree:

<span class='terminal'>$ git add .<br>
$ git commit \-am \"initial commit for local submodules\"</span>

<button id= "toggleStatus02" onclick="hiddencode('Status02')">Hide/Show Git status response</button>

<div id="Status02" style="display:none">

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

#### Add remote repo as submodule

To add a remote repo from GitHub.com, open your web browser and navigate to the repo you want to add as a submodule. For the repo ("my-first-repo") you created in the post on [Remote repositories with GitHub](../git-github). If you did not create this repo, you can use the GitHub example repo [the Spoon-Knife project](https://github.com/octocat/Spoon-Knife).

![github-dismiss-github-action-popup](../../images/github-clone-button-4-fork.png){: .pull-right}
With the GitHub repo you want to add as a submodule in your browser, click the <span class='button'>Clone or Download</span> button. Copy the link to the repo as shown to the right. Then paste the link to the command:

<br />

<span class='terminal'>$ git submodule add git@github.com:"YourGitHubAccount"/my-first-repo.git</span>

```
remote/my-first-repo
Cloning into '/Users/"YourGitHubAccount"/temp/git-dummy/container-repo/remote/my-first-repo'...
git sremote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 9 (delta 1), reused 4 (delta 1), pack-reused 0
Receiving objects: 100% (9/9), 1.64 KiB | 840.00 KiB/s, done.
Resolving deltas: 100% (1/1), done.
```

<button id= "toggleStatus03" onclick="hiddencode('Status03')">Hide/Show Git status response</button>

<div id="Status03" style="display:none">

{% capture text-capture %}
{% raw %}

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   .gitmodules
	new file:   remote/my-first-repo
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

<span class='terminal'>$ git add .<br>
$ git commit \-am \"initial commit for remote submodules\"</span>

<button id= "toggleStatus04" onclick="hiddencode('Status04')">Hide/Show Git status response</button>

<div id="Status04" style="display:none">

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

Put your terminal window in the root of your _container-repo_ and list (<span class='terminalapp'>ls</span>) the content:

<span class='terminal'>$ ls</span>

```
README.md		ch01-introduction	remote			submodules
```

This corresponds to the commands executed above.

### Edit submodule repo

Return to one of the repos that you added as a submodule. Edit one of the text files, or add a new file. _stage_ and _commit_ the changes. Make sure that you have no outstanding content by running <span class='terminalapp'>git status</span>. Here is an example:

<span class='terminal'>$ cd ..
<br>$ cd ch01-introductio
<br>$ pico ch01-introduction.md</span>
```
...
Add a text string ...
```
Hit [ctrl]+[X] to exit <span class='terminalapp'>pico</span> and save the edits by pressing <span class='terminal'>Y</span> when asked.

<button id= "toggleStatus12" onclick="hiddencode('Status12')">Hide/Show git status response</button>

<div id="Status12" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   ch01-introduction.md

no changes added to commit (use "git add" and/or "git commit -a")
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

<span class='terminal'>$ git commit \-am \"edited ch01-introduction.md\"</span>

```
[master 60b6923] edited ch01-introduction.md
 1 file changed, 2 insertions(+)
```

<button id= "toggleStatus13" onclick="hiddencode('Status13')">Hide/Show git status response</button>

<div id="Status13" style="display:none">

{% capture text-capture %}
{% raw %}
```
[master 60b6923] edited ch01-introduction.md
 1 file changed, 2 insertions(+)
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Pull changes to submodule

The _commited_ changes in the repo _ch01-introduction_ are not yet reflected in the submodule built from this repo. In your terminal window, return to the superproject repo that contains the submodules, then to location (directory) where you saved _ch01-introduction_  as a submodule(in my example above I did not give any path and the sublodule is saved under its original name), and then <span class='terminalapp'>pull</span>.

<span class='terminal'>$ cd ..
<br>$ cd container-repo
<br>$ cd ch01-introduction
<br>$ git pull</span>
```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From /Users/thomasgumbricht/temp/git-dummy/ch01-introduction
   ac86f33..60b6923  master     -> origin/master
Updating ac86f33..60b6923
Fast-forward
 ch01-introduction.md | 2 ++
 1 file changed, 2 insertions(+)
```

Return to the root of the repo containing the submodules and check <span class='terminalapp'>status</span>:

<span class='terminal'>$ cd ..
<br>$ git status</span>

```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   ch01-introduction (new commits)

no changes added to commit (use "git add" and/or "git commit -a")
```

Even if you have pulled the changes in the original repo linked to the submodule (_ch01-introduction_), these changes are not saved to the superproject history tree. You have to run a _commit_:

<span class='terminal'>$ git commit -am "pulled ch01-introduction"</span>

<button id= "toggleStatus14" onclick="hiddencode('Status14')">Hide/Show git status response</button>

<div id="Status14" style="display:none">

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

## Resources

[Submodules at Atlassian Bitbucket](https://www.atlassian.com/git/tutorials/git-submodule)

[Pro Git - Everything you need to know about git](https://git-scm.com/book/en/v2/) by Scott Chacon and Ben Straub (20200219).

[Fork a github repo that has submodule?](https://www.reddit.com/r/git/comments/qatxi/fork_a_github_repo_that_has_submodule/)
