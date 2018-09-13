---
layout: post
title: "Git - My Cheat List"
categories:
  - cheatlist
tags:
  - en
  - python
  - git
  - cheatlist
featured-img: code
last_modified_at: 2017-03-09T14:25:52-05:00
---


# Summary

* [How to rebase a forked repository](#rebase-fork)
* [How to add a default editor to git](#default-editor)
* [How to amend to some old commit?](#amend-old-commit) 

# My Git Cheat List


<h2 id='rebase-fork'>How to rebase a forked repository</h2>

On the cloned repository

```
$ git checkout master
$ git remote add upstream git@github.com.../other/repo.git
$ git fetch upstream
$ git rebase upstream/master
$ git checkout my-branch
$ git rebase master
```

<h2 id='default-editor'>How to add a default editor to git</h2>

```
$ git config --global core.editor vim
```


<h2 id='amend-old-commit'>How to amend to some old commit?</h2>

```
$ git log
```

Get the hash of the commit you wish to edit

```
$ git rebase -i 123456
```

It will open your editor with the commit options written as *pick*

![](https://i.imgur.com/6jbkv2b.png)

Change it to *edit*

![](https://i.imgur.com/vbPbIAe.png)

Your branch name is now changed to the commit's hash. Edit your files then:

```
$ git add yourfile
$ git commit --amend
```

Once you have finished all changes:

```
$ git rebase --continue 
```

Everything is back to normal. Since you made a rebase, you need a "forced" push:

```
$ git push -f 
```
