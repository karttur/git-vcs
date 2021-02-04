---
layout: post
title: Shared master local git control
categories: git
excerpt: "Setup local git version control with shared master"
tags:
  - mac osx
  - git
  - local
  - shared master
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2020-02-17 11:27'
modified: '2020-11-12'
comments: true
share: true
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

**NOTE, 1 October 2020 the label 'master' was replaced by 'main' for the default repo branch.**

## Introduction

This post covers git version control with a shared _master_ repository on local machines. The shared _master_ is not used for active development, but is more of a container for holding versions that stem from _clones_. The development only happen in the _clones_ and the _master_ just receives _pushes_ and send out _pull_ requests from _clones_.

This post is mainly based on different parts from the tutorial series [Learn Git with Bitbucket Cloud](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud). Links to relevant pages are given in different sections below.

## Prerequisites

You need to install git for command line use as outlined in the parallel post on [Local git control](../git-local-use).

## Create directory with git control

Create a new, empty, directory. To use the <span class='app'>Terminal</span> first change directory (<span class='terminalapp'>cd</span>) to the parent folder under where you want to create the new directory.

<span class='terminal'>$ cd path/to/parent/directory</span>

You can also just type <span class='terminal'>$ cd</span> in the <span class='app'>Terminal</span> window, then open a <span class='app'>Finder</span> window, navigate the the parent folder and drag the directory icon of the parent to the <span class='app'>Terminal</span> window.

You are now ready to create a new directory, that will be converted to a shared _master_ repository in the next section. By convention, shared repos end with _.git_ as an extension of the directory name. For this tutorial, you are going to create a shared repo, and if you want to follow the convention you should name it, for instance _git-test-dir.git_.

Make sure the <span class='app'>Terminal</span> points towards the parent directory, and create the new directory with the <span class='terminalapp'>mkdir</span> command:

<span class='terminal'>$ mkdir git-test-dir.git</span>

<span class='terminalapp'>cd</span> to the newly created directory:

<span class='terminal'>$ cd git-test-dir.git</span>

## Git init

To create a shared _master_ you must add the parameter _\-\-bare_ to the _init_ command:

<span class='terminal'>$ git init \-\-bare</span>

The response will be something like

```
Initialized empty Git repository in path/to/git-test-dir.git
```

For details on [Git init and the \-\-bare option](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-init).

If you now check the content of your directory by listing (<span class='terminalapp'>ls</span>) its content:

<span class='terminal'>$ ls</span>

You will see a list of files and folders:
```
HEAD		config		description	hooks		info		objects		refs
```
In an "ordinary" repo, all of these files and folders are hidden (under the folder <span class='file'>.git</span>)  _repo_, but not so in a _\-\-bare_ repo used as a _master_ container.

## Add a readme.md

Add a _README_ file to the shared _master_ repo:

<span class='terminal'>$ pico README.md</span>

```
Shared master repo for project on ...
```

Hit [ctrl]+[X] to exit <span class='terminalapp'>pico</span> and save the edits by pressing <span class='terminal'>Y</span> when asked.

## Clone I

Change directory to the parent folder of your newly created repo:

<span class='terminal'>$ cd..</span>

Now _clone_ the _master_ with the command <span class='terminalapp'>git clone path/to/master path/to/clone</span>, with either relative or absolute paths. As your terminal window is in the parent folder of your shared _master_ you can create a parallel _clone_ like this:

<span class='terminal'>$ git clone git-test-dir.git/ git-test-dir_dev</span>

```
Cloning into 'git-test-dir_dev'...
warning: You appear to have cloned an empty repository.
done.
```

Link to [Bitbucket Atlassian in depth page on git clone command](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-clone).

## Create document in clone

Create a document in your _clone_ repo, for instance a markdown (<span class='file'>.md</span>) md file. I have created a file named <span class='file'>Chapter.md</span>, with the following content:

```
# Chapters in book on climate change evidence from Remote Sensing

## Introduction

## The sun

## Earth's energy balance

## Physical feedbacks

### Albedo changes 1970 - 2020

```

Once you have created the document, make sure your terminal window is in your _cloned_ repo and type:

<span class='terminal'>$ git status</span>

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	Chapters.md

nothing added to commit but untracked files present (use "git add" to track)
```

## Stage and commit

As in the previous post on [Local git control](../git-local-use), you have to _stage_ and then _commit_ any changes before you can _pull_ or _push_ them. First _stage_ the new file:

<span class='terminal'>$ git add Chapters.md</span>

<button id= "toggleStatus01" onclick="hiddencode('Status01')">Hide/Show Git status response</button>

<div id="Status01" style="display:none">

{% capture text-capture %}
{% raw %}

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   Chapters.md


```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

Then _commit_ all (-a) while also giving a message (-m):

<span class='terminal'>$ git commit \-am \"initial commit\"</span>

```
[master (root-commit) 5e660be] initial commit
 1 file changed, 11 insertions(+)
 create mode 100644 Chapters.md
```

<button id= "toggleStatus02" onclick="hiddencode('Status02')">Hide/Show Git status response</button>

<div id="Status02" style="display:none">

{% capture text-capture %}
{% raw %}

```
On branch master
Your branch is based on 'origin/master', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)

nothing to commit, working tree clean
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

More details on from the Atlassian BitBucket pages on [Saving changes and Git add](https://www.atlassian.com/git/tutorials/saving-changes) and [Git commit](https://www.atlassian.com/git/tutorials/saving-changes/git-commit)

## Git Push

Your edits in the _clone_ are now _staged_ and _commited_, and you can _push_ them to the shared _master_:

<span class='terminal'>$ git push</span>

```
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 355 bytes | 355.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To /path/tp/git-test-dir.git/
 * [new branch]      master -> master
```

## Clone II

Let us create a second _clone_ of the shared _master_. This time we can use absolute paths and ignore where the terminal window is pointing:

<span class='terminal'>$ git clone path/to/git-test-dir.git/ path/to/git-test-dir_share</span>

```
Cloning into 'path/to/git-test-dir_share'...
done.
```

This time the returned message does **not** state that we _cloned_ an empty repository. If you check the content of the new _clone_ ("git-test-dir_share") you will see that it contains the file you created and edited in the development _clone_ and then pushed to the shared _master_.

## Resources

[Learn Git with Bitbucket Cloud](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud)

[Pro Git - Everything you need to know about git](https://git-scm.com/book/en/v2/) by Scott Chacon and Ben Straub (20200219).
