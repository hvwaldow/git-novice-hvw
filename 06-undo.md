---
title: Undoing Things
teaching: 8
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand "undoing" with respect to history changes
- Be able to "go back" in various situations

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I get back to a previous commit?
- How can I undo changes to the staging area?
- How can I get a specific version of a file into the working-tree?

::::::::::::::::::::::::::::::::::::::::::::::::::

In the previou episode we introduced `git restore` and `git revert`. Here we add
`git reset` to the mix and look more closely at this important topic.

:::::::::::::::::::::::::::::::::::::: callout

## `git reset`

We look at the form  

`git reset [--soft | --hard | --mixed] <commit>`

This command operates on your commit history and changes it. It sets HEAD to
\<commit\>. You get rid of all commits that came after \<commit\>. **They are
also not accessible anymore: You throw them away!**

+ Option `--hard` will synchronize both staging are **and working tree** to that state. You loose changes you made in your working tree after \<commit\>.

+ Option `--mixed` will synchronize the staging are to that state, but leave your working tree untouched. You could add and commit to undo the reset.
  Option `--mixed` is the default

+ Option `--soft` will also leave the staging area untouched. That means the
  changes you removed from the commit-history are already staged to re-commit.

::::::::::::::::::::::::::::::::::::::

Add some more toppings to the pizza recipe:

~~~bash
$ echo "Jalapenos" >>pizza.md
$ git commit -am "add spicy"
$ echo "Anchovis" >>pizza.md
$ git commit -am "add frutti di mare"
~~~
 Check the current history:   
 `git log`

::::::::::::::::::::::::::::::::::: challenge
 
## Sea food allergy!
 
Remove the latest commit from the commit history. Make it so, that you have the
latest version of `pizza.md` still available and can correct the offending line like so:
 
`Anchovis (if you are allergic, we substitute with mozzarella)`
 
Check what happens to the commit history whenever you have changed it.
 
:::::::::::::::::::::: solution
 
 ~~~bash
 $ git reset 39a67879
 $ git log
 $ git status
 $ nano pizza.md
 $ git commit -am "offer seafood replacement"
 $ git log
 ~~~
 
::::::::::::::::::::::
::::::::::::::::::::::::::::::::::

**Changing history is only ok if nobody else has seen the original history yet (when collaborating)**

:::::::::::::::::::::::::::::::::: callout

## `git restore [--staged] [-s <commit>] <file or directory>`

+ Per default restores the working tree from the staging area (which equals last commit = HEAD  if nothing is staged) 
+ Use `--staged` to restore the staging area from HEAD.
+ Use `-s <commit>` to select a specific commit as **s**ource.
+ Restore whole sub-directories (recursively) by giving a \<directory\> instead of a \<file\>  
  **In particular, restore the whole repo by using the dot: `.`**

::::::::::::::::::::::::::::::::::

Delete pizza by accident:  
`rm pizza.md`  
`ls`

Restore pizza:  
`git restore pizza.md`

Remove everything by accident:  
`git restore .`  
`ls`

::::::::::::::::::::::::::::::::::: challenge
 
## Undo changes to the staging area

By mistake stage a change that you don't want to end up as a commit:

`echo "guacamole stinks" >>guacamole.md`  
`git add guacamole.md`  
`git status`  
 
+ Restore the staging area from HEAD (the latest commit).
+ Make sure you don't stage the offending line gain, by mistake. 

 
:::::::::::::::::::::: solution
 
`git restore --staged guacamole.md`  
`git status`  
`git restore guacamole.md`  
 
::::::::::::::::::::::
::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git reset` rewrites history. Use with care! Don't change shared history!  
- `git restore` lets you update working tree or staging area to arbitrary commits (versions) of files or directory trees.
::::::::::::::::::::::::::::::::::::::::::::::::::
