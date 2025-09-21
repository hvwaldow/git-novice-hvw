---
title: Remotes in GitHub
teaching: 45
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain what remote repositories are and why they are useful.
- Push to or pull from a remote repository.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I share my changes with others on the web?

::::::::::::::::::::::::::::::::::::::::::::::::::

Version control really comes into its own when we begin to collaborate with
other people.  We already have most of the machinery we need to do this; the
only thing missing is to copy changes from one repository to another.

Systems like Git allow us to move work between any two repositories.  In
practice, though, it's easiest to use one copy as a central hub, and to keep it
on the web rather than on someone's laptop.  Most programmers use hosting
services like [GitHub](https://github.com), [Codeberg](https://codeberg.org) or
[GitLab](https://gitlab.com/) to hold those main copies.

Let's start by sharing the changes we've made to our current project with the
world. To this end we are going to create a *remote* repository that will be linked to our *local* repository.

## 1\. Create a remote repository

::::::::::::::::::::::::::::::: callout

## Forges

Saas offer such as [GitHub](https://github.com),
[Codeberg](https://codeberg.org) [GitLab.com](https://gitlab.com/),
[BitBucket](https://bitbucket.org/product/),
[sourcehut](https://sourcehut.org/), [SourceForge](https://sourceforge.net)
cater mostly to software developers and provide a central platform to manage Git
(or other VCS) repositories. They all have a different set of features
additional to Git, that help with project management, community organization,
CI/CD, ... These features are different for each platform.

+ There are commercial ones, such as GitHub and non-profit ones, such as Codeberg.
+ There are some based on proprietary code, such as GitHub, and some that are
  based on Open Source software, that can be deployed by everybody, such as GitLab
  and Codeberg (with [Forgejo](https://forgejo.org/)).
+ There are really many GitLab instances all over the academic landscape, you
  surely have access to one.
+ The most successful one and the one that gives you most exposure is also the
  one used by most large Open Source projects and the one that puts you at the
  grace of a company with a questionable ethical track-record that collaborates
  with an authoritarian regime.
  
  In the following we are using GitHub as an example anyway. You should be able
  with little effort to transfer this to any other common forge.

:::::::::::::::::::::::::::::::::

Log in to [GitHub](https://github.com), then click on the icon in the top right corner to
create a new repository called `recipes`:

![](fig/github-create-repo-01.png){alt='The first step in creating a repository on GitHub: clicking the "create new" button'}

Name your repository "recipes" and then click "Create Repository".

Note: Since this repository will be connected to a local repository, it needs to be empty. Leave
"Initialize this repository with a README" unchecked, and keep "None" as options for both "Add
.gitignore" and "Add a license." See the "GitHub License and README files" exercise below for a full
explanation of why the repository needs to be empty.

![](fig/github-create-repo-02.png){alt='The second step in creating a repository on GitHub: filling out the new repository form to provide the repository name, and specify that neither a readme nor a license should be created'}

As soon as the repository is created, GitHub displays a page with a URL and some
information on how to configure your local repository:

![](fig/github-create-repo-03.png){alt='The summary page displayed by GitHub after a new repository has been created. It contains instructions for configuring the new GitHub repository as a git remote'}

This effectively does the following on GitHub's servers:

```bash
$ mkdir recipes
$ cd recipes
$ git init
```

If you remember back to the earlier [episode](04-changes.md) where we added and
committed our earlier work on `guacamole.md`, we had a diagram of the local repository
which looked like this:

![](fig/git-staging-area.svg){alt='A diagram showing how "git add" registers changes in the staging area, while "git commit" moves changes from the staging area to the repository'}

Now that we have two repositories, we need a diagram like this:

![](fig/git-freshly-made-github-repo.svg){alt='A diagram illustrating how the GitHub "recipes" repository is also a git repository like our local repository, but that it is currently empty'}

Note that our local repository still contains our earlier work on `guacamole.md`, but the
remote repository on GitHub appears empty as it doesn't contain any files yet.

## 2\. Connect local to remote repository

Now we connect the two repositories.  We do this by making the
GitHub repository a [remote](../learners/reference.md#remote) for the local repository.
The home page of the repository on GitHub includes the URL string we need to
identify it:

![](fig/github-change-repo-string.png){alt='A screenshot showing that clicking on "SSH" will make GitHub provide the SSH URL for a repository instead of the HTTPS URL'}

Click on the 'SSH' link to change the [protocol](../learners/reference.md#protocol) from HTTPS to SSH.

:::::::::::::::::::::::::::::::::::::::::  callout

## HTTPS vs. SSH

We use SSH here because, while it requires some additional configuration, it is a
security protocol widely used by many applications.  The steps below describe SSH at a
minimum level for GitHub.


::::::::::::::::::::::::::::::::::::::::::::::::::

![](fig/github-find-repo-string.png){alt='Clicking the "Copy to Clipboard" button on GitHub to obtain the repository\'s URL'}

Copy that URL from the browser, go into the local `recipes` repository, and run
this command:

```bash
$ git remote add origin git@github.com:<username>/recipes.git
```

`origin` is a local name used to refer to the remote repository. It could be called
anything, but `origin` is a convention that is often used by default in Git.

We can check that the command has worked by running `git remote -v`:

```bash
$ git remote -v
```

```output
origin   git@github.com:<user>/recipes.git (fetch)
origin   git@github.com:<user>/recipes.git (push)
```

We'll discuss remotes in more detail in the next episode, while
talking about how they might be used for collaboration.

## 3\. SSH Background and Setup

Before you can connect to a remote repository, you need to set up a way for you
computer to authenticate with GitHub so it knows it's you trying to connect to
your remote repository.

We are going to set up the method that is commonly used by many different services.  This method is called Secure Shell Protocol (SSH).  SSH is an extremely popular and versatile network protocol that allows secure communication between computers.

SSH uses a *key pair*. This is two keys that work together to validate access. One key is publicly known and called the *public key*, and the other key called the *private key* is kept private.

You can think of the public key as a padlock, and only you have the key (the private key) to open it. You use the public key where you want a secure method of communication, such as your GitHub account.  You give this padlock, or public key, to GitHub and say "lock the communications to my account with this so that only computers that have my private key can unlock communications and send git commands as my GitHub account."

We now do the minimum required to set up the SSH keys and add the public key to a GitHub account.

First we check if this has already been done on the computer you're on. Because
generally speaking, this setup only needs to happen once and then you can forget
about it.


Check what key pairs already exist on your computer:

```bash
$ ls -al ~/.ssh
```

Your output is going to look a little different depending on whether or not SSH
has ever been set up on the computer you are using.

If you have not set up SSH on you computer, the output will be something like that:

```output
ls: cannot access '/c/Users/<username>/.ssh': No such file or directory
```

If SSH has been set up on the computer you're using, the public and private key
pairs will be listed. The file names are either `id_ed25519`/`id_ed25519.pub` or
`id_rsa`/`id_rsa.pub` depending on how the key pairs were set up.

If they don't exist yet, set them up like so:

### 3\.1 Create an SSH key pair

Create an SSH key pair, loke so, where the `-t` option specifies which type of algorithm to use.

```bash
$ ssh-keygen -t ed25519
```

If you are using a legacy system that doesn't support the Ed25519 algorithm, use:

~~~bash
$ ssh-keygen -t rsa -b 4096
~~~

~~~output
Generating public/private ed25519 key pair.
Enter file in which to save the key (/c/Users/<username>/.ssh/id_ed25519):
~~~

We want to use the default file, so just press <kbd>Enter</kbd>.

```output
Created directory '/c/Users/<user>/.ssh'.
Enter passphrase (empty for no passphrase):
```

In the real word, it is strongly recommended to secure your key with a
passphrase. It prevents anyone getting physical or remote access to you machine
from automatically also controlling your account at the forge.

Here, we leave the passphrase empty though, for efficiency:   
Just hit <kbd>Enter</kbd>.

```output
Enter same passphrase again:
```

And <kbd>Enter</kbd> again.

```output
Your identification has been saved in /c/Users/<username>/.ssh/id_ed25519
Your public key has been saved in /c/Users/<username>/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:SMSPIStNyA00KPxuYu94KpZgRAYjgt9g4BA4kFy3g1o yourusername@yourhostname
The key's randomart image is:
+--[ED25519 256]--+
|^B== o.          |
|%*=.*.+          |
|+=.E =.+         |
| .=.+.o..        |
|....  . S        |
|.+ o             |
|+ =              |
|.o.o             |
|oo+.             |
+----[SHA256]-----+
```

The "identification" is actually the private key. You should never share it.  The public key is appropriately named.

Now that we have generated the SSH keys, we will find the SSH files when we check.

```bash
ls -al ~/.ssh
```

```output
drwxr-xr-x 1 Alfredo   197121   0 Jul 16 14:48 ./
drwxr-xr-x 1 Alfredo   197121   0 Jul 16 14:48 ../
-rw-r--r-- 1 Alfredo   197121 419 Jul 16 14:48 id_ed25519
-rw-r--r-- 1 Alfredo   197121 106 Jul 16 14:48 id_ed25519.pub
```

### 3\.2 Copy the public key to GitHub

Now we have a SSH key pair and we can run this command to check if GitHub can read our authentication.

```bash
ssh -T git@github.com
```

```output
The authenticity of host 'github.com (192.30.255.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
```

Right, we forgot that we need to give GitHub our public key!

First, we need to copy the public key.  Be sure to include the `.pub` at the end, otherwise you're looking at the private key.

```bash
cat ~/.ssh/id_ed25519.pub
```

```output
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDmRA3d51X0uu9wXek559gfn6UFNF69yZjChyBIU2qKI a.linguini@ratatouille.fr
```

Now, going to GitHub.com, click on your profile icon in the top right corner to
get the drop-down menu. Click "Settings", then on the settings page, click "SSH
and GPG keys", on the left side "Access" menu. Click the "New SSH key" button on
the right side. Now, you can add a title (something that helps you identify the
machine, where the private key is), paste your SSH key into the field, and click
the "Add SSH key" to complete the setup.

Now that we've set that up, let's check our authentication again from the command line.

```bash
$ ssh -T git@github.com
```

```output
Hi \<User\>! You've successfully authenticated, but GitHub does not provide shell access.
```

Good! This output confirms that the SSH key works as intended. We are now ready to push our work to the remote repository.

## 4\. Push local changes to a remote

Now that authentication is setup, we can return to the remote.  This command will push the changes from
our local repository to the repository on GitHub:

```bash
$ git push -u origin main
```

The parameter `-u` or `--set-upstream` sets remote the branch as defualt for
future `git push` and `git pull`.

```output
Enumerating objects: 16, done.
Counting objects: 100% (16/16), done.
Delta compression using up to 8 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (16/16), 1.45 KiB | 372.00 KiB/s, done.
Total 16 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), done.
To https://github.com/alflin/recipes.git
 * [new branch]      main -> main
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Proxy

If the network you are connected to uses a proxy, there is a chance that your
last command failed with "Could not resolve hostname" as the error message. To
solve this issue, you need to tell Git about the proxy:

```bash
$ git config --global http.proxy http://user:password@proxy.url
$ git config --global https.proxy https://user:password@proxy.url
```

When you connect to another network that doesn't use a proxy, you will need to
tell Git to disable the proxy using:

```bash
$ git config --global --unset http.proxy
$ git config --global --unset https.proxy
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Password Managers

If your operating system has a password manager configured, `git push` will
try to use it when it needs your username and password.  For example, this
is the default behavior for Git Bash on Windows. If you want to type your
username and password at the terminal instead of using a password manager,
type:

```bash
$ unset SSH_ASKPASS
```

in the terminal, before you run `git push`.  Despite the name, [Git uses
`SSH_ASKPASS` for all credential
entry](https://git-scm.com/docs/gitcredentials#_requesting_credentials), so
you may want to unset `SSH_ASKPASS` whether you are using Git via SSH or
https.

You may also want to add `unset SSH_ASKPASS` at the end of your `~/.bashrc`
to make Git default to using the terminal for usernames and passwords.


::::::::::::::::::::::::::::::::::::::::::::::::::

Our local and remote repositories are now in this state:

![](fig/github-repo-after-first-push.svg){alt='A diagram showing how "git push origin" will push changes from the local repository to the remote, making the remote repository an exact copy of the local repository.'}

We can pull changes from the remote repository to the local one as well:

```bash
$ git pull
```

```output
Already up-to-date.
```

Pulling has no effect in this case because the two repositories are already
synchronized.  If someone else had pushed some changes to the repository on
GitHub, though, this command would download them to our local repository.

:::::::::::::::::::::::::::::::::::::::  challenge

## GitHub GUI

Browse to your `recipes` repository on GitHub.
Under the Code tab, find and click on the text that says "XX commits" (where "XX" is some number).
Hover over, and click on, the three buttons to the right of each commit.
What information can you gather/explore from these buttons?
How would you get that same information in the shell?

:::::::::::::::  solution

## Solution

The left-most button (with the picture of a clipboard) copies the full identifier of the commit
to the clipboard. In the shell, `git log` will show you the full commit identifier for each
commit.

When you click on the middle button, you'll see all of the changes that were made in that
particular commit. Green shaded lines indicate additions and red ones removals. In the shell we
can do the same thing with `git diff`. In particular, `git diff ID1..ID2` where ID1 and
ID2 are commit identifiers (e.g. `git diff a3bf1e5..041e637`) will show the differences
between those two commits.

The right-most button lets you view all of the files in the repository at the time of that
commit. To do this in the shell, we'd need to checkout the repository at that particular time.
We can do this with `git checkout ID` where ID is the identifier of the commit we want to
look at. If we do this, we need to remember to put the repository back to the right state
afterwards!



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Uploading files directly in GitHub browser

Github also allows you to skip the command line and upload files directly to
your repository without having to leave the browser. There are two options.
First you can click the "Upload files" button in the toolbar at the top of the
file tree. Or, you can drag and drop files from your desktop onto the file
tree. You can read more about this [on this GitHub page](https://help.github.com/articles/adding-a-file-to-a-repository/).


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## GitHub Timestamp

Create a remote repository on GitHub. Push the contents of your local
repository to the remote. Make changes to your local repository and push these
changes. Go to the repo you just created on GitHub and check the
[timestamps](../learners/reference.md#timestamp) of the files. How does GitHub
record times, and why?

:::::::::::::::  solution

## Solution

GitHub displays timestamps in a human readable relative format (i.e. "22 hours ago" or "three
weeks ago"). However, if you hover over the timestamp, you can see the exact time at which the
last change to the file occurred.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Push vs. Commit

In this episode, we introduced the "git push" command.
How is "git push" different from "git commit"?

:::::::::::::::  solution

## Solution

When we push changes, we're interacting with a remote repository to update it with the changes
we've made locally (often this corresponds to sharing the changes we've made with others).
Commit only updates your local repository.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## GitHub License and README files

In this episode we learned about creating a remote repository on GitHub, but when you initialized
your GitHub repo, you didn't add a README.md or a license file. If you had, what do you think
would have happened when you tried to link your local and remote repositories?

:::::::::::::::  solution

## Solution

In this case, we'd see a merge conflict due to unrelated histories. When GitHub creates a
README.md file, it performs a commit in the remote repository. When you try to pull the remote
repository to your local repository, Git detects that they have histories that do not share a
common origin and refuses to merge.

```bash
$ git pull origin main
```

```output
warning: no common commits
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/alflin/recipes
 * branch            main     -> FETCH_HEAD
 * [new branch]      main     -> origin/main
fatal: refusing to merge unrelated histories
```

You can force git to merge the two repositories with the option `--allow-unrelated-histories`.
Be careful when you use this option and carefully examine the contents of local and remote
repositories before merging.

```bash
$ git pull --allow-unrelated-histories origin main
```

```output
From https://github.com/alflin/recipes
 * branch            main     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
README.md | 1 +
1 file changed, 1 insertion(+)
create mode 100644 README.md
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- A local Git repository can be connected to one or more remote repositories.
- Use the SSH protocol to connect to remote repositories.
- `git push` copies changes from a local repository to a remote repository.
- `git pull` copies changes from a remote repository to a local repository.

::::::::::::::::::::::::::::::::::::::::::::::::::
