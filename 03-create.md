---
title: Creating a Repository
teaching: 10
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Create a local Git repository.
- Describe the purpose of the `.git` directory.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Where does Git store information?
- How to tell what is going on in a repository?
- What defines a Git repository?

::::::::::::::::::::::::::::::::::::::::::::::::::

Once Git is configured, we can start using it.

## Initialization

We will help Alfredo with his new project, create a repository with all his recipes.

First, let's create a new directory in the `Desktop` folder for our work and then change the current working directory to the newly created one:

```bash
$ cd ~/Desktop
$ mkdir recipes
$ cd recipes
```

Then we tell Git to make `recipes` a [repository](../learners/reference.md#repository) -- a place where Git can store versions of our files:

```bash
$ git init
```

It is important to note that `git init` will create a repository that
can inclde subdirectories and their files---there is no need to create
separate repositories nested within the `recipes` repository, whether
subdirectories are present from the beginning or added later. Also, note
that the creation of the `recipes` directory and its initialization as a
repository are completely separate processes.

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

```bash
$ ls
```

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `recipes` called `.git`:

```bash
$ ls -a
```

```output
.	..	.git
```
## The Git database

This special subdirectory, `.git`, contains the Git database: All the information about the project, including the tracked files and sub-directories located within the project's directory.

If this file exists, the directory is a Git *repository*. If it doesn't, it isn't.

If you delete the `.git` subdirectory, you lose the project's history and have a
normal directory again.

You don't have to worry what is inside `.git`. It is completely managed by Git (unless much more advanced features are useed). Treat it as a black box.

## Status of a Git repository

We can now start using one of the most important git commands, which is particularly helpful to beginners. `git status` tells us the status of our project, and better, a list of changes in the project and options on what to do with those changes. We can use it as often as we want, whenever we want to understand what is going on.

```bash
$ git status
```

```output
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

If you are using a different version of `git`, the exact
wording of the output might be slightly different.

:::::::::::::::::::::::::::::::::::::::  challenge

## What makes a repository?

Without creating a new directory or leaving `recipes`, figure out what `git status` responds, if called when you are not in a Git repository.

:::::::::::::::::::: solution
## Solution

~~~bash
$ rm -rf .git   #remove .git
$ git status
~~~
~~~output
fatal: not a git repository (or any of the parent directories): .git
~~~

::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::

## Clone a Git repository

It is very common to copy a repository from somewhere else to your computer to work with it. GitHub is still the most prominet place for shared Git repositories world-wide, and fortunately hosts repositories containing recipes that are freely accessible, e.g. here:  
https://github.com/cr-workshop-exercises/recipe-book.git

The second possibility to get a Git repository is to *clone* an existing one. This creates a local copy of the whole repository with all its history.

~~~bash
$ cd ..
$ git clone https://github.com/cr-workshop-exercises/recipe-book.git
$ cd recipe-book
$ ls -al
~~~

To get an idea how the history of a repository can look after some development, look at its history:

~~~bash
$ git log
$ git log --graph --oneline
~~~

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git init` initializes a repository.
- Git stores all of its repository data in the `.git` directory.

::::::::::::::::::::::::::::::::::::::::::::::::::
