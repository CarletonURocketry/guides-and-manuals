# CUIS Git Guide

## Setting Up Your Git Environment

First, make sure that Git is installed on your computer. A guide to installing
Git on various operating systems can be found here (although Linux is the
easiest to get it working on):

[Installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

### Configuring Git

Git stores configuration settings in three different locations:

1. /etc/gitconfig -- Stores system-wide Git settings
2. ~/.gitconfig or ~/.config/git/config -- User-specific Git settings
3. .git/config -- Repository-specific settings (inside the .git directory of
your repositories)

The more local the configuration, the more precedence it has over global
configuration options. If you define "user.name" differently in your
~/.gitconfig than in /etc/gitconfig, the value in ~/.gitconfig will take
precedence for that user.

Configuration is done using the command `git config` as seen below.

### Basic Configuration

The following information, your name and email address, are important to set
because this is information embedded in every commit.

`git config --global user.name "<Your Name>"`

`git config --global user.email "<Your Email>"`

This will set your name and email system-wide. To make this information local to
a repository, replace "--global" with "--local".

Configuring the default editor which is invoked by Git when, for example,
writing commit messages or Git emails can be done by issuing the following
command:

`git config --global core.editor <Your Editor>`

The command `git config --list` can be used to see all options which have been
set.

## The Three States of Git

### Working Directory

The working directory is a single checkout of one commit or one version of the
repository. It is all the files that you can see and that which you are able to
currently make changes to.

### Staging Area

The staging area is the place to put files which have changes that you wish to
commit to the repository. It is a representation of all the changes that will be
included in the next commit. You place files in this area using the command `git
add <filename>`.

### Committed

Committed files/changes go into the .git directory (the repository). These are
files and changes which are a part of the repository. Commits form the basis of
your repository and each commit acts like a snapshot of the project at the time
the commit was made.

## The Basics of Using Git

To get more information on specific git commands, you can issue the command:
`git help <action>` or `man git-<action>`. A simple list of the options for an
action can be accessed by adding `-h` to the end of a command. For example: `git
add -h`.

### Creating or Cloning a Repository

If you want to take an existing directory and start version-controlling it,
ensure your current working directory is the top level directory of the project
and issue the command `git init`. This will create a .git directory in the
project's directory and provide all of Git's functionality for that project.

To clone an existing repository—which will download the entire repository's
history and set of files—issue the following command: `git clone <url>`. <url>
can be the path to a directory on your computer, or it can be the URL of a
remote Git repository. This will place the downloaded repository into a
directory within the current directory.

### Editing Files

Files, as seen by Git, exist in four different states:

    1. Untracked
    2. Unmodified
    3. Modified
    4. Staged

The command `git status` gives information about which files are where. A file
which is labelled under "Changes to be committed" is a staged file. Files which
are Untracked or Modified can be added to the staging area using `git add
<filename>`. Files in the staging area are files to be included
in the next commit. It is important to note that when you use `git add` to stage
a file, the current version of that file is set to be committed. If you then
make further changes to that file before committing, those new changes will not
be committed until you run `git add` again.

### Ignoring Files

Git has a convenient way to ignore files. If you use a command like `git add *`
to quickly add several files to the staging area, you may not want to add things
like object files or compiled programs. The .gitignore file provides an easy way
to specify what to exclude from your repository. The file is a plain text file
with each line specifying a file or a group of files to exclude. For example, a
.gitignore file which looks like the following:

```
    *.o
    *.exe
    TODO.txt
```

Will tell Git to exclude all files with the .o and .exe extensions as well as
the single file called TODO.txt.

## Making Commits

Commits are the basis of a version control system. In Git, they represent
snapshots of the changes made to your codebase. It is because of this that
making good commits is very important since commits are a form of documentation
for the changes made. If you make a change to your code and something breaks,
you have a record of what change was made and where. You also have the ability
to easily revert that change.

A commit takes all of the files in the staging area and snapshots them, adding
them to the chain of commits that represent your project history. Each commit
forms a single link in the chain and represents a changeset. Each commit has an
author with corresponding email address, a subject line, a more detailed
message, and some other meta-information like the hash and diff information
(which is out of the intended scope of this guide).

A commit can be made using the command `git commit`. The commit history can be
viewed using the command `git log`. A useful set of flags to pass to the log
command which will show you commits as a graph is `--all --online --graph
--decorate`.

### What to Commit

When making a commit, you want to keep them as atomic as possible. This means
you want to make your commits describe one change and only one change. A change
can be anything from fixing a single bug, to adding a whole feature, to breaking
up a code refactoring session into bite-sized portions. What constitutes
a change can itself change depending on what you are doing but the important
question to ask is: "Does it make sense to split this commit up into two or more
separate commits?" If so, then you should split that commit up.

Here is an example:
**A bad, non-atomic commit:**

```
Fix button not working in startup screen and add background image".
```

**Good, atomic commits:**

```
"Fix button not working". "Add background image".
```

### Writing a Good Commit Message

Writing a good commit message is the most important part of making a commit
since commits document the history of your project. A good commit message should
be detailed, fit the style and standards of your project, and should not attempt
to hide information or lie about the changes made.

When you run the `git commit` command, it will open up your chosen text editor
so that you can write a message. This message consists of a subject line and
a body (similar to an email) separated by a blank line. The subject should be
limited to 50 characters in length, capitalized, and should be a very short
summary of the change made.

The 50 character limit helps the commits to be readable, concise, fit on one
line and services such as GitHub will display 50 character subjects without
truncating them with ellipses. If you go over 50 characters, consider that maybe
your commit isn't atomic enough, or find a better, more concise way to describe
the change. It is generally okay if you go over by a few characters, but try to
refrain from it as the `git log` command described above will look very ugly if
the output has to wrap long subject lines. There should be no periods, question
marks, exclamation marks, etc. in the subject line.

The body of the commit message is the place to go into detail about the what and
the why of the change (but not the how, because that is already described by the
commit itself). It should be limited to 72 characters per line but can be as
many lines long as you wish (just please be reasonable). We limit the line
length to 72 characters because this makes the body of the message print nicely
centred on an 80 column display.

Commits should be written using the imperative grammatical mood. This means you
should write your message like "Fix broken button element" instead of "Fixed
broken button element". We do this because this is how Git itself writes its
automated commit messages (e.g. when doing a merge commit) and doing it this way
makes the commit history look and feel consistent. Think about your commits
like: "This commit will [do something]".

Here is an example:

**A bad commit:**

```
fixed button on startup page to make it not segfault when it was clicked with
text that was too long on the dialog box.
```

**A good commit:**

```
Fix cancel button buffer overflow on startup page

The button on the startup page is not working properly. It causes a segmentation
fault when it is clicked if the text entered into the dialog box is over the
expected character limit. Rewrite the handler function to use snprintf because
snprintf has built-in buffer protections.
```

## Branching and Workflows in Git

One of the great advantages that Git has over other version control systems is
that it makes branches very easy to work with. Branches are a way to make
a series of commits without directly changing the codebase that others are
working on. For example, you can make a separate branch to test an experimental
feature. If you decide that it is a good feature, you can merge it into the main
codebase. If it is an awful, broken, mess of a feature that never should have
seen the light of day and makes you rethink your life choices, you can just
delete the branch and pretend the feature never existed.

When a Git repository is initialized using the command `git init`, it will be
initially set up with one branch, the "master" branch. This is the main branch
of your project and is most often used as the branch in which releases of
software reside. The centralized workflow is a workflow which mostly ignores the
use of other branches and dictates that commits should be made solely to the
master branch.

There are many different ways to use branches. These are typically called
workflows. Examples of workflows include the centralized workflow, gitflow, and
the feature-branch workflow. There are many more, and you should choose the one
that fits your project structure the best. There is no one-size-fits-all
workflow.

### Managing Branches

Branches can be created using the command `git checkout -b <branch_name>`. This
will create a new branch and change your working area over to that branch. Also,
it is important to note that you can switch between branches using the command
`git checkout <branch_name>`. You can then make changes in this branch the same
way as in any other branch.

Branches can be deleted using the command `git branch -d`.

Two branches may be merged using the command `git merge <branch_name>`. This
will merge the branch `<branch_name>` into whichever branch you are currently
working on. If you wanted to merge branch `test` onto `master`, you would `git
checkout master` and then `git merge test`.

### Pushing to and Pulling from a Remote Repository

A remote repository is just a repository that isn't the repository in your
current directory. A remote repository can exist somewhere on the internet, or
in a different place on your computer. Either way, there are a set of useful
commands for synchronizing your changes with these repositories so that you can
work on your projects with others. Usually a remote repository is referred to in
commands as "origin" since this is the default name for a remote repository in
Git (for example, the master branch in the remote repository would then be
referred to as "origin/master".)

The basic things done with remote repositories are pushing and pulling changes.
You can pull changes that have been pushed to a remote repository using the
command `git pull <repository_url> <branch_name>`. This will fast-forward your
current repository so that it is now up to date with the remote one or it will
attempt to perform a merge if you have made local changes that don't exist on
the remote repository. In the case of a merge, you may have to fix merge
conflicts (a good guide to doing so can be found in the links below).

Pushing to a repository is as easy as the command `git push <remote_name>
<branch_name>` where `branch_name` is the name of the branch you want to push to
`remote_name`. If changes exist on the remote branch that are newer than your
changes, git will prompt you to pull and merge your changes with those on the
remote repository and then you may push after that merge is complete.

## Overview of Commands

* `git init`     -- Initialize a repository in the current working directory
* `git clone`    -- Clone a remote repository and create a copy on your computer
* `git config`   -- Change configuration options
* `git status`   -- View the status of the repository
* `git add`      -- Add a file to the staging area
* `git commit`   -- Commit the files in the staging area
* `git checkout` -- Change the working area to the specified branch or commit
* `git branch`   -- Manage branches
* `git pull`     -- Pull from a remote repository
* `git push`     -- Push to a remote repository

## Further Commands and Reading

Git is very complex and very powerful. If you want to learn more, check out
these links:

* https://git-scm.com/book/en/v2/
* https://www.atlassian.com/git/tutorials
* https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/

Also, some other commands which may be useful but which haven't been touched on
in this guide include:

`git diff` ,`git tag`, `git rebase`, `git remote`.
