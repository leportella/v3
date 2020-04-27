---
layout: post
title: "Git - My Cheat Sheet"
categories:
  - english
  - cheatsheet
tags:
  - en
  - python
  - git
  - cheatlist
  - python
  - community 
  - pyladies
  - technology
  - tecnologia
  - programador
  - programadora
  - developer
  - mulheres na tecnologia
  - woman in tech
  - girls in tech
  - computação
  - ciência de computação
  - software development
  - software engineering
  - engenharia de software
  - desenvolvimento
  - auto-ensino
  - self-taught engineer
  - code
  - Django
  - software
  - career
  - tech career
  - open-source
  - no cs degree
  - cs
  - computer science
featured-img: code
permalink: cheatsheet-git.html
redirect_from: /cheatlist/2018/01/05/git-cheat-list.html
last_modified_at: 2017-03-09T14:25:52-05:00
---


# Summary

* [Undo commits](#undo-commits)
* [How to rebase a forked repository](#rebase-fork)
* [How to add a default editor to git](#default-editor)
* [Delete changes on conflict while rebasing](#conflict)
* [How to amend to some old commit?](#amend-old-commit) 
* [How to amend delete merged branches?](#delete-merged-branches) 

# My Git Cheat List


<h2 id='undo-commits'>Undo commits</h2>

1 commit: 

```
$ git reset HEAD~ 
```

2 commits

```
$ git reset HEAD~~ 
```


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

<h2 id='conflict'>Delete changes on conflict while rebasing</h2>

If you generated an automatic file, pushed it but then there was a conflict because the file was updated by someone else, sometimes the easiest thing is to just throw away your changes and run the script again. To do that while you are rebasing you just

```
$ git rebase master

... CONFLICT: file.ext

$ git checkout file.ext
$ git add file.ext

$ run_automatic_script
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

<center>
![](https://i.imgur.com/6jbkv2b.png)
</center>
</br>

Change it to *edit*

<center>
![](https://i.imgur.com/vbPbIAe.png)
</center>
</br>

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



<h2 id='delete-merged-branches'>How to amend delete merged branches?</h2>

This delete all merged branch. Do NOT use `-D`.

```
$ for branch in `git branch | grep -v master`; do git branch -d $branch; done
```