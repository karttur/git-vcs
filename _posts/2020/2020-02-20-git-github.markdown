---
layout: post
title: Remote repositories with GitHub
categories: git
excerpt: "Setup remote repositories with GitHub"
tags:
  - GitHub
  - git
  - remote repository
  - clone
  - fork
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2020-11-20 11:27'
modified: '2020-11-12'
comments: true
share: true
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

**NOTE, 1 October 2020 the label 'master' was replaced by 'main' for the default repo branch.**

## Introduction

This post covers connecting a local git clone to a remote master at [gitHub.com](https://github.com). The post is inspired by the Youtube [Introduction to Git - Remotes](https://www.youtube.com/watch?v=Gg4bLk8cGNo) by David Mahler, and the written post [‘Git’ with the Program: Getting Started with GitHub](https://thenewstack.io/git-with-the-program-getting-started-with-github/).

## Prerequisites

Apart from having [git installed](../git-commandline-install) you must also have a [secure shell (SSH) connection](../git-SSH-connect) if you want to follow this tutorial in detail. You can also use HTTPS for connecting to GitHub.com, then you do not need an SSH setup, but you need need to give your GitHub credentials when you want to connect for your local machine to your online GitHub account. If you do not have an online GitHub account, the next step is to get one.

## Create GitHub account + repo

[GitHub](https://github.com) is free for private users that are satisfied with public repositories. You just sign up, give your email and once you have confirmed your register via your email, and given answers to some simple questions about experiences and interests, you will land at the page "Create a new repository".

![github-nav-new-repo](../../images/github-nav-new-repo.png){: .pull-right}

If you do not get to the page "Create a new repository", use the navigation menu next to the avatar to select "New Repository" (as shown to the right).

<br />

Go ahead and create you first repo, the figure below shows the fields to fill.

<figure>
<img src="../../images/github-create-new-repo-filled.png">
<figcaption> Github - Create a new repository.</figcaption>
</figure>

You have to give the repo a name. Also fill in a <span class='textbox'>Description</span> and let the repository be _Public_ (to use _Private_ repos comes with a fee). Click the check boxes  for _Add a README_ file, _Add .gitignore_ and _Choose a license_ as appropriate. Click <span class='button'>Creating repository ...</span> and behind the scenes GitHub is _staging_ and _commiting_.

![github-dismiss-github-action-popup](../../images/github-dismiss-github-action-popup.png){: .pull-right}

When ready, GitHub will open a page with your repo. If there is a pop-up window inquiring about GitHub Actions (as shown to the right), just click <span class='button'>Dismiss</span>.

Your first GitHub repo is created, including one (1) _commit_ at branch _master_ that locked in the <span class='file'>README.md</span> markdown document. All summarised on the repo page shown below.

<figure>
<img src="../../images/github-my-first-repo-commit01.png">
<figcaption> Github - My first repo with a single commit.</figcaption>
</figure>

## Grab the SSH for a repo

Towards the right side, approximately mid down there is a <span class='button'>Code</span> button (shown below - in older version it was labelled [Clone or Download]). Click on it.

![github-repo-clone-download-set-SSH](../../images/github-repo-clone-download.png){: .pull-right}

In the pop-out window select "SSH" and the setting will change to **Clone with SSH**. And then copy the string in the textbox starting with <span class='textbox'>git@github.com...</span>. This string will allow us to connect from a local git repo directly to the remote repo on GitHub.

## Clone to local repo

Open a <span class='app'>Terminal</span> window and <span class='terminalapp'>cd</span> to the parent directory where you want to create the _clone_ of your remote (GitHub) repository. Then type (but do **not** execute):

<span class='terminal'>$ git clone </span>

If you followed the instructions in the previous section, you can now just paste the SSH link from your clipboard to the command line:

<span class='terminal'>$ git clone git@github.com:thomasgumbricht/my-first-repo.git</span>

Execute the command and your remote repo ("my-first-repo") should clone to your local machine. If this is the first time you use the SSH key, you will be prompted the following statement and question:
```
Cloning into 'my-first-repo'...
The authenticity of host 'github.com (140.82.118.4)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)?
```

Answer with a full <span class='terminal'>yes</span>

```
Warning: Permanently added 'github.com,140.82.118.4' (RSA) to the list of known hosts.
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
Receiving objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
```

### Local repo settings

The post on [Local git control](../git-local-use) explains how to [Check and setup user name](../git-local-use). For the cloned repo ("my-first-repo") you can setup a local user and email that matches the SSH key. As I created this repo as a trial, I need to set my user and email. First <span class='terminalapp'>cd</span> to the clone of the repo, and then set user and email:

<span class='terminal'>$ cd my-first-repo<br>$ git config user.name thomasgumbricht<br>$ git config user.email thomas.karttur@gmail.com</span>

Verify your configuration by displaying your configuration file:

<span class='terminal'>$ cat .git/config</span>

### git remotes

The command <span class='terminalapp'>git remote</span> returns the remotes for the repo. With the terminal window still pointing to the cloned repo ("my-first-repo"), type:

<span class='terminal'>git remote</span>

```
origin
```

The name _origin_ is like a git alias for the default remote repository. To see the full paths (urls) and the rights that we have for remotes, type:

<span class='terminal'>git remote -v</span>

```
origin	git@github.com:thomasgumbricht/my-first-repo.git (fetch)
origin	git@github.com:thomasgumbricht/my-first-repo.git (push)
```

### git log

In some of the earlier posts the following command was introduced as a means to get a condensed graphical view of the branches and the _commit_ history:

<span class='terminal'>$ git log \-\-all \-\-decorate \-\-oneline \-\-graph</span>

```
* 9f30e5b (HEAD -> master, origin/master, origin/HEAD) Initial commit
```

For the _clone_ we are working with the _HEAD_ pointer points towards two new items: _origin/master_ and _origin/HEAD_. As _origin_ can be seen as an alias for the primary remote repo, the pointing is towards the remote (GitHub) repo. As _HEAD_ is pointing both towards the local _master_ and the remote _master_, our local repo is in sync with the GitHub repo.

### Edit origin/master

To try the different functions for linking a remote and a local repository, return to your [online GitHub account](https://github.com) and the _origin/master_ repo ("my-first-repo"). When in that repo, click the <span class='button'>Create new file</span> button.

<figure>
<img src="../../images/github-create-new-file-click.png">
<figcaption> Github - click the Create new file button.</figcaption>
</figure>

In the window that opens, give the name of the new document in the textbox that says <span class='textbox'>Name your file...</span>. Then enter a few lines in the area for <span class='textbox'>Edit new file.</span>.

<figure>
<img src="../../images/github-create-new-file-page.png">
<figcaption> Github - Create new file.</figcaption>
</figure>

When done, scroll down towards the bottom of the page, GitHub has created a default (short) message. If you accept that message just click the <span class='button'>Commit new file</span> button.

<figure>
<img src="../../images/github-create-new-file-commit.png">
<figcaption> Github - Commit new file.</figcaption>
</figure>

The edits you made are _staged_ and _commited_ by GitHub, and you are returned to the front page of your online repo. It now says you have two commits, and in the list of contents you can see two files, each with its own _commit_.

<figure>
<img src="../../images/github-my-first-repo-commit02.png">
<figcaption> Github - My first repo with two commits.</figcaption>
</figure>

### git fetch

Return to the <span class='app'>Terminal</span> window that points to your local clone. Your local copy should now be behind the online  (_orign/master_). However, your local git is not yet aware of the latest online _commit_. You can check that out by either <span class='terminalapp'>git status</span> or <span class='terminalapp'>git log</span>. For your local git clone to catch up with online changes, you need to run <span class='terminalapp'>git fetch</span>.

<span class='terminal'>$ git fetch</span>
or<br>
<span class='terminal'>$ git fetch origin</span>

```
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:thomasgumbricht/my-first-repo
   9f30e5b..9cd2c08  master     -> origin/master
```

Check the status of your local repo:

<span class='terminal'>$ git status</span>

```
On branch master
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

You can explore the situation graphically:

<span class='terminal'>$ git log \-\-all \-\-decorate \-\-oneline \-\-graph</span>

```
* 9cd2c08 (origin/master, origin/HEAD) Create Chapters.md
* 9f30e5b (HEAD -> master) Initial commit
```

The local _master_ is trailing _origin/master_.

### git merge

The post on [git branches](../git-branches) outlines how to use <span class='terminalapp'>git merge</span>. As you have not done any edits to your local (receiving) repo, the _merge_ of the _origin/master_ can be done as a fast-forward _merge_ (straight update of the _HEAD_ pointer to the latest _commit_):

<span class='terminal'>$ git merge origin/master</span>

```
Updating 9f30e5b..9cd2c08
Fast-forward
 Chapters.md | 5 +++++
 1 file changed, 5 insertions(+)
 create mode 100644 Chapters.md
```

You can check that your local clone is in sync with _orgin/master_ by either <span class='terminalapp'>git status</span> or <span class='terminalapp'>git log</span>.

<button id= "toggleStatus01" onclick="hiddencode('Status01')">Hide/Show git status response</button>

<div id="Status01" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

<button id= "toggleLog01" onclick="hiddencode('Log01')">Hide/Show git log \-\-all \-\-decorate \-\-oneline \-\-graph response</button>

<div id="Log01" style="display:none">

{% capture text-capture %}
{% raw %}
```
* 9cd2c08 (HEAD -> master, origin/master, origin/HEAD) Create Chapters.md
* 9f30e5b Initial commit
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### git pull

The <span class='terminalapp'>git pull</span> command combines <span class='terminalapp'>git fetch</span> and <span class='terminalapp'>git merge</span> into a single command. Both online [GithHub](https://github.com) repos and the [<span class='app'>Github Desktop</span> app](https://desktop.github.com) have predefined functions for _pull_ (rather than _fetch_ + _merge_). It is, nevertheless, often better to use _fetch_ + _merge_.

### git push

The command [<span class='terminalapp'>git push</span>](https://git-scm.com/docs/git-push) is used for sending local clone changes to remote repos. To test it, make some edits to the local copy of <span class='file'>Chapters.md</span>. You can use any editor, for instance <span class='terminalapp'>pico</span>:

<span class='terminal'>$ pico Chapters.md</span>

```
...
## Add another chapter
```

Hit [ctrl]+[X] to exit <span class='terminalapp'>pico</span> and save the edits by pressing <span class='terminal'>Y</span> when asked.

_commit_ the changes with the combined command:

<span class='terminal'>$ git commit \-am \"Added chapter to Chapters.md\"</span>

```
[master 0b4547f] Added chapter to Chapters.md
 1 file changed, 2 insertions(+)
```

<button id= "toggleStatus02" onclick="hiddencode('Status02')">Hide/Show git status response</button>

<div id="Status02" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

<button id= "toggleLog02" onclick="hiddencode('Log02')">Hide/Show git log \-\-all \-\-decorate \-\-oneline \-\-graph response</button>

<div id="Log02" style="display:none">

{% capture text-capture %}
{% raw %}
```
* 0b4547f (HEAD -> master) Added chapter to Chapters.md
* 9cd2c08 (origin/master, origin/HEAD) Create Chapters.md
* 9f30e5b Initial commit
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

The local clone of the repo is now 1 _commit_ ahead of _origin/master_. To put them in sync again, <span class='terminalapp'>git push</span> the local changes to the remote repo:

<span class='terminal'>$ git push origin master</span>

or afer 20201001

<span class='terminal'>$ git push origin main</span>


```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 339 bytes | 169.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:thomasgumbricht/my-first-repo.git
   9cd2c08..0b4547f  master -> master
```

<button id= "toggleStatus03" onclick="hiddencode('Status03')">Hide/Show git status response</button>

<div id="Status03" style="display:none">

{% capture text-capture %}
{% raw %}
```
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

<button id= "toggleLog03" onclick="hiddencode('Log03')">Hide/Show git log \-\-all \-\-decorate \-\-oneline \-\-graph response</button>

<div id="Log03" style="display:none">

{% capture text-capture %}
{% raw %}
```
* 0b4547f (HEAD -> master, origin/master, origin/HEAD) Added chapter to Chapters.md
* 9cd2c08 Create Chapters.md
* 9f30e5b Initial commit
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

A quick look at your [GitHub online](https://github.com) repo (remember to refresh the page) reveals that you have now three commits, with the latest message being "Added chapter to Chapters.md".

<figure>
<img src="../../images/github-my-first-repo-commit03.png">
<figcaption> Github - My first repo with three commits.</figcaption>
</figure>

## Resources

[Pro Git - Everything you need to know about git](https://git-scm.com/book/en/v2/) by Scott Chacon and Ben Straub (20200219).

[Learn Git with Bitbucket Cloud](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud)

Youtube tutorial [Introduction to Git - Remotes](https://www.youtube.com/watch?v=Gg4bLk8cGNo) by D. Mahler (20170621)
