---
layout: post
title: Install git for command line
categories: git
excerpt: Install Git command line tool
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2020-01-31 T18:17:25.000Z'
modified: '2020-11-12'
comments: true
share: true
figure15: github-framework_karttur_15_new-other
figure16: github-framework_karttur_16_pydev-package
figure17: github-framework_karttur_17_pydev-package2
---

**NOTE, 1 October 2020 the label 'master' was replaced by 'main' for the default repo branch.**

## Introduction

This post summarises how to install the command-line tool <span class='terminalapp'>git</span>.

## Prerequisites

This tutorial relies heavily on the <span class='app'>Terminal</span> command tool, if you are not acquainted with the command line, the [Command line crash course (pdf)](https://www.computervillage.org/articles/CommandLine.pdf) will teach you most things you need to know.

## git for command-line use

Check out if you have the command line tool for
[<span class='terminalapp'>git</span>](https://git-scm.com) installed by opening a <span class='app'>Terminal</span> window and type:

<span class='terminal'>$ git version</span>

If your system does not have <span class='terminalapp'>git</span> installed you can install it via [<span class='app'>GitHub Desktop</span>](https://desktop.github.com) or download it from the [git official download page](https://git-scm.com/downloads).

If your version is outdated compared to the [git official download page](https://git-scm.com/downloads), you can instead use git itself for updating:

<span class='terminal'>$ git clone https://github.com/git/git</span>

### Check and setup user name

Every git project (repository, or repo for short) is linked to a user name. If you have a dominating, or single, git user on your machine you can add a global user name to your local machine git. If you have more than one git user you can set the user name in each local repo. The local user over-rides any global user, so you can set both types in the same machine. For more information, please vist the GitHub page [Setting your username in Git](https://help.github.com/en/articles/setting-your-username-in-git).

#### Global git user name

To check if you have a global git user name set, open a <span class='app'>Terminal</span> window and type:

<span class='terminal'>$ git config \-\-global user.name</span>

If you had no global user but require one, set the git global user name with the command:

<span class='terminal'>$ git config \-\-global user.name _username_</span>

#### Local (repository) user names

To set local (per repo) user names you have to sequentially change directory <span class='terminal'>cd</span> to each repo that requires a local user and then check/set the user name.

<span class='terminal'>$ git config user.name</span>

<span class='terminal'>$ git config user.name _repo-username_</span>

### Check and set email

See the official GitHub page on [Setting your commit email address](https://help.github.com/en/articles/setting-your-commit-email-address) to understand your alternatives for open or restricted email addresses.

The principles for setting your git email address is similar to setting the git user name. Here is, for example, how to check and set a global email:

<span class='terminal'>$ git config \-\- user.email</span>

<span class='terminal'>$ git config \-\- user.email _email@example.com_</span>

For local (per repo) setting of email you have to sequentially change directory to each local repo for which to set the email.

### Password

Transferring data from your local machine to an online git account with for instance [GitHub.com](https://github.com), you can use either HTTPS or SSH. HTTPS is safer and more widely accepted, and also the format that GitHub.com recommends. If these alternatives are totally new for you, never mind. Just go ahead and git will use HTTPS without you knowing it.

#### HTTPS

There are different options regarding how to set your password if you are cloning using HTTPS. If you use the <span class='app'>GitHub Desktop</span> app for pulling, committing and pushing staged changes, you do not need a separate git password. The same is true if you use, for example, <span class='app'>Eclipse</span> for cloning. In both cases your password will be stored by the app you use. You can also choose to give the password each time it is requested by git. The last option is [Caching your GitHub password in Git](https://help.github.com/en/articles/caching-your-github-password-in-git).

#### SSH

SSH (means secure shell) is more cmplicated but will facilitate if you use the git command line tool (rather than [<span class='app'>GitHub Desktop</span>](https://desktop.github.com) and GitHub.com itself directly). All the other posts in this blog deal with using the git command line tool. Thus the next post in this series deals with [git Secure Shell (SSH)](../git-SSH-connect).

### Online resources

To try <span class='terminalapp'>git</span> out and learn about git control on [GitHub.com](https://github.com/) the youtube video [Github Tutorial For Beginners](https://www.youtube.com/watch?v=0fKg7e37bQE) is instructive.
