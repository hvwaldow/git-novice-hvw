---
title: Configure Git
teaching: 10
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Configure `git` the first time it is used on your computer.
- Understand the meaning of the `--global` configuration flag.
- Know how to look at the Git configuration
- Know where and how to change the Git configuration.
- Know where to get Git help

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I configure Git before using it for the first time?
- Where can I get help about Git concepts and commands?

::::::::::::::::::::::::::::::::::::::::::::::::::

When you use Git on a new computer for the first time, you need to configure a
few things. Only five of them are really necessary or strongly recommended:

1. Your name
2. Your email address
3. Your preferred text editor 
4. Deal with line endings
5. Your preferred initial name for a [branch](../learners/reference.md#branch)
   (explained later)

::::::::::::::::::::::::::::::::::::::::: callout
Git commands usually take the form  

`git subcommand options parameters`

where options and parameters may be optional.
:::::::::::::::::::::::::::::::::::::::::

## Set your name and email

```bash
$ git config --global user.name "FirstName LastName"
$ git config --global user.email "myemail@provider.tld"
```
Please use your own name and email address.  

**Use the email address you used to register for GitHub or the forge of your choice, because these services use the email address to assign commits to accounts.**

**You can use a fanatasy-name for "Firstname LastName" if you like.**

:::::::::::::::::::::::::::::::::::::::::::: spoiler

### Hidden email address for GitHub

GitHub for one lets you keep your email address private.

- Go to the [email settings](https://github.com/settings/emails) and switch "*Keep my email addresses private*" on.
- Set your local Git email address to `id+ghusername@users.noreply.github.com` as instructed there.

::::::::::::::::::::::::::::::::::::::::::::

## The editor of choice

Occasionally, Git fires up an editor for you. Choose which one!

~~~bash
git config --global core.editor "nano"
~~~

The common denominator for an editor in this course is *nano*. If you are used to and have installed a different text editor, feel free to configure that one.

Examples:

| Editor                                | Configuration command | 
| :-----------                          | :------------------------------ |
| Atom                                  | `$ git config --global core.editor "atom --wait"`                      | 
| BBEdit (Mac, with command line tools) | `$ git config --global core.editor "bbedit -w"`                      | 
| Sublime Text (Mac)                    | `$ git config --global core.editor "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl -n -w"`                      | 
| Sublime Text (Win, 32-bit install)    | `$ git config --global core.editor "'c:/program files (x86)/sublime text 3/sublime_text.exe' -w"`                      | 
| Sublime Text (Win, 64-bit install)    | `$ git config --global core.editor "'c:/program files/sublime text 3/sublime_text.exe' -w"`                      | 
| Notepad (Win)                         | `$ git config --global core.editor "c:/Windows/System32/notepad.exe"`                      | 
| Notepad++ (Win, 32-bit install)       | `$ git config --global core.editor "'c:/program files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`                      | 
| Notepad++ (Win, 64-bit install)       | `$ git config --global core.editor "'c:/program files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`                      | 
| Gedit (Linux)                         | `$ git config --global core.editor "gedit --wait --new-window"`                      | 
| Emacs                                 | `$ git config --global core.editor "emacs"`                      | 
| Vim                                   | `$ git config --global core.editor "vim"`                      | 
| VS Code                               | `$ git config --global core.editor "code --wait"`                      | 

It is possible to reconfigure the text editor for Git whenever you want to change it.

## Homogeneous line endings

Line endings (pressing  <kbd>Enter</kbd> or <kbd>↵</kbd> or <kbd>Return</kbd>) are encoded differently in Windows than in other operating systems.

Because comparing files and figuring out if and how they differ is an important thing for Git, we want to avoid different line-endings when Windows users collaborate with non-Windows users.

Configure Git to do all necessary conversions automagically:

### On MacOS or Linux

~~~bash
git config --global core.autocrlf input
~~~
### On Windows

~~~bash
git config --global core.autocrlf true
~~~

## Default Git branch naming

A *branch* in Git refers to a specific sequence of changes, a line of development. For new learners in this lesson, it's enough to know that branches exist and have names. By default, Git will create a branch called `master`
when you create a new repository with `git init` (as explained in the next Episode).

However, the majority of large software projects and also major code forges such as GitHub, GitLab.com and codeberg.org have changed this default name for the first branch to "*main*", because the "master/slave" terminology is considered offensive and antiquated today.

Change your default branch name to "*main*" to avoid unnecessary friction:

~~~bash
$ git config --global init.defaultBranch main
~~~

The five commands we just ran above only need to be run once: the flag `--global` tells Git
to use the settings for every project, in your user account, on this computer.

Let's review those settings and test our `core.editor` right away:

```bash
$ git config --global --edit
```

Let's close the file without making any additional changes.  Remember, since typos in the config file will cause
issues, it's safer to view the configuration with:

```bash
$ git config --list --global
```

And if necessary, change your configuration using the
same commands to choose another editor or update your email address.
This can be done as many times as you want.

:::::::::::::::::::::::::::::::::::::::::  callout

## Git Help and Manual

Always remember that if you forget the subcommands or options of a `git` command, you can access the
relevant list of options typing `git <command> -h` or access the corresponding Git manual by typing
`git <command> --help`, e.g.:

```bash
$ git config -h
$ git config --help
```

While viewing the manual, remember the `:` is a prompt waiting for commands and you can press <kbd>Q</kbd> to exit the manual.

More generally, you can get the list of available `git` commands and further resources of the Git manual typing:

```bash
$ git help
```

Descriptions from the Git reference documentation are very often overwhelming, because of the many options, parameters, dense language, rich and confusing terminology.

- **Search for "EXAMPLES" in the manual for a command (type `/examples`)**.
- **Look in the [Git Pro Book](https://git-scm.com/book/en/v2)**

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::: challenge

The command to create a new empty Git repository is `git init` (more on that later).

What exactly is the command to create a new empty Git repository where name of the initial branch is "*my_initial_branch*" instead of "*main*"?

::::::::::::::::::::::::::::::: solution

Both `git init -h` and `git init --help` will tell you that the command is

~~~bash
$ git init --initial-branch my_initial_branch
~~~
::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use `git config` with the `--global` option to configure a user name, email address, editor, and other preferences once per machine.

- Use `git <subcommand> -h` or `git <subcommnd> --help` to get information about that subcommand.

::::::::::::::::::::::::::::::::::::::::::::::::::


