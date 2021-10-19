---
layout: post
title: "git cheat sheet"
categories: git
excerpt: GitHub/git cheat sheet for GeoImagine Framework
tags:
  - Git
  - GitHub
  - Command line
  - cheat sheet
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2021-01-11 20:17'
modified: '2021-01-11 20:17'
comments: true
share: true
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

## Introduction

This post is a cheat sheet for how to create a repo on [GitHub.com](https://github.com) and then maintain that repo using [git command line tool](https://karttur.github.io/git-vcs/).

## Create GitHub account + repo

[GitHub](https://github.com) is free for private users that are satisfied with public repositories. You just sign up, give your email and once you have confirmed your register via your email, and given answers to some simple questions about experiences and interests, you will land at the page "Create a new repository".

![github-nav-new-repo](https://karttur.github.io/git-vcs//images/github-nav-new-repo.png){: .pull-right}

If you do not get to the page "Create a new repository", use the navigation menu next to the avatar to select "New Repository" (as shown to the right).

<br />

Go ahead and create you first repo, the figure below shows the fields to fill.

<figure>
<img src="https://karttur.github.io/git-vcs//images/github-create-new-repo-filled.png">
<figcaption> Github - Create a new repository.</figcaption>
</figure>

You have to give the repo a name. Also fill in a <span class='textbox'>Description</span> and let the repository be _Public_ (to use _Private_ repos comes with a fee). Click the check boxes  for _Add a README_ file, _Add .gitignore_ and _Choose a license_ as appropriate (for Karttur´s GeoImagine Framework pages I use BSD 3). Click <span class='button'>Creating repository ...</span> and behind the scenes GitHub is _staging_ and _commiting_.

![github-dismiss-github-action-popup](https://karttur.github.io/git-vcs/images/github-dismiss-github-action-popup.png){: .pull-right}

When ready, GitHub will open a page with your repo. If there is a pop-up window inquiring about GitHub Actions (as shown to the right), just click <span class='button'>Dismiss</span>.

Your GitHub repo is created, including one (1) _commit_ at branch _master_ that locked in the <span class='file'>README.md</span> markdown document. All summarised on the repo page shown below.

<figure>
<img src="https://karttur.github.io/git-vcs/images/github-my-first-repo-commit01.png">
<figcaption> Github - My first repo with a single commit.</figcaption>
</figure>

### Grab the HTTPS/SSH for a repo

Towards the right side, approximately mid down there is a <span class='button'>Code</span> button (shown below). Click on it.

![github-repo-clone-download-set-SSH](https://karttur.github.io/git-vcs//images/github-repo-clone-download.png){: .pull-right}

In the pop-out window select either "HTTPS" or "SSH", dependent on your security solution when setting up <span class='terminalapp'>git</span>. And then copy the string in the textbox starting. This string will allow us to connect from a local git repo directly to the remote repo on GitHub.

## Terminal operations

From here on all operations will rely on the <span class='app'>Terminal</span>. You can return to to your online GitHub repo at any time to validate that the the changes that are down locally and then pushed to the online repo are really synchronised.

### Clone to local repo

Open a <span class='app'>Terminal</span> window and <span class='terminalapp'>cd</span> to the parent directory where you want to create the local _clone_ of your remote (GitHub) repository. Then type (but do **not** execute):

<span class='terminal'>$ git clone </span>

If you followed the instructions in the previous section, you can now just paste ([ctrl-v]) the HTTPS/SSH link from your clipboard to the command line (for example):

<span class='terminal'>$ git clone git@github.com:"name"/"repo".git</span>

Execute the command and your remote repo ("repo") should clone to your local machine.

<span class='terminalapp'>cd</span> to new repo:

<span class='terminal'>$ cd "repo"</span>

Create the standard <span class='file'>.gitignore</file>, using for instance <span class='terminalapp'>pico</span>:

<span class='terminal'>$ pico .gitignore/span>

and copy + paste the following items to the <span class='file'>.gitignore</file> file:

```
.DS_Store
__pycache__/
```

Close and save <span class='file'>.gitignore</file>.

### Set user and email

If you installed <span class ='terminalapp'>git</span> you most likely also created a global user and global password, and an SSH key (as outlined in my instructions for [git command line](https://karttur.github.io/git-vcs/)). If your local repo uses another GitHub account and another email, you have to set a local user and password (your terminal must be in the repo for which you are setting the credentials):

<span command='terminal'>$ git config user.name "Your Name"</span>

<span command='terminal'>$ git config user.email "you@example.com"</span>

### Edit the readme.md

Edit the readme <span class='file'>readme.md</span> file if required. You can use <span class='terminalapp'>pico</span>:

<span class='terminal'>$ pico readme.md/span>.

But it is a very basin text editor so better use a more advanced one, like [Atom](https://atom.io/).

### Add codes and data

Create or copy the code and data that you want to store into your newly created "repo".


Add all the files:

<span class='terminal'>$ git add .</span>

Commit

<span class='terminal'>$ git commit -m "initial commit"</span>

Push

<span class='terminal'>$ git push origin main</span>

or

<span class='terminal'>$ git push origin gh-pages</span>

Check that your local clone and the remote main are synchronised:

<span class='terminal'>$ git status</span>

With a bit more bling to it:

<span class='terminal'>$ git log --all --decorate --oneline --graph</span>

## Browser

Use your browser to look at your new repo
