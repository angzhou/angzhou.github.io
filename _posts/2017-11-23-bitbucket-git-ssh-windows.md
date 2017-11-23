---
layout: post
title: Get bitbucket to work with git-bash on Windows
tags: git ssh
---

Add public key to bitbucket account setting `SSH keys`.

On Windows, add a `System` environment variable:

    GIT_SSH -> C:\Program Files\PuTTY\plink.exe

Run peagent.exe, load the private key.

At the first time, run putty, enter git@bitbucket.org, then open, cache the key.

Now key is cached, we can run `git clone` from git-bash without problem.
