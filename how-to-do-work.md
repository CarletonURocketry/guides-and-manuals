# How to Do Work
## How to Contribute to CUInSpace

Please make sure that you fully understand the content of this guide before you
begin working on CUInSpace projects. This will detail how to pick an issue to
work on, assign it to yourself, and work on it.

Here are the steps to follow:

1. Pick an Issue
2. Assign it to Yourself
3. Create a New Branch in Your Local Repository
4. Do Some Work
5. Push Your Work Upstream
6. Create A Pull Request
7. Repeat from Step 4 if the Pull Request was Denied
8. Else, repeat from Step 1.

## Pick an Issue

On GitHub there is a feature called "Issues". It is essentially a place where
repository maintainers and contributors can post things that need to be done.
Anything from bug fixes to feature requests are allowed. Let's take our
2019-avionics repository as an example:

![Images Board](/images/2019-avionics-issues.png)

As you can see, there are a number of issues in the issues tab of this
repository.

Each issue has some tags which give a developer or maintainer a quick idea of
what the issue may be. For example, we can see that most of these issues are
enhancements (i.e. feature requests), one is a bug, and it's pretty evenly
divided among easy issues and advanced issues.

You can click on an issue to view more information about what needs to be done
and what has already been done as well as some information about what skills the
issue may involve.

## Assign it to Yourself

In order to assign an issue to yourself, click the check box next to the issue,
then click on the Assign menu in the top right, then you can choose to assign
the issue to yourself which will make your picture appear next to that issue so
others can see that you are working on it.

![Issue Selection](/images/2019-avionics-issue-selection.png)

## Create a New Branch in Your Local Repository

**NOTE:** If a branch already exists with the name of the component, then please
continue using that one rather than creating a new branch. You can view a list
of branches using the command `git branch --list`.

Once you have chosen an issue, you should create a new branch in the repository
with the name of the component of that issue. For example, if you are working on
the "Driver for 1-Wire Bridge" issue, you would create a branch called
"1-wire-bridge". You may do this using the command `git checkout -b
1-wire-bridge`. When you push your work to the remote repository, this branch
should be automatically created on GitHub for you.

## Do Some Work

This is the hardest part. Now you must work on your issue. Make sure that you
are following the coding standards too!

Feel free to make as many commits as you feel is necessary, the git guide will
come in handy here.

## Push Your Work Upstream

You may also push your work to the remote repository as you see fit. Anything
you push there will stay in your branch and will not be automatically merged
into master. It is a good idea to make at least occasional pushes since, if your
hard drive dies, you will lose all of the work you have done locally that you
haven't pushed (about once every hour during a sprint or once every day during
regular development is typically okay depending on how much you've accomplished
and how critical the changes are).

## Create a Pull Request

This is how you get your work into our final product. Creating a pull request is
as simple as navigating to the Pull Requests tab in the repository and clicking
the "New Pull Request" button.

![Pull Request Tab](/imags/new-pull-request.png)

Once you click this button, you will be presented with a page where you can
select which branch you want to merge into which other branch. You should change
the branch on the right to the branch you are working on and leave the left one
on "master". This will present you with a list of commits, files changed, and
will let you know if the branches can be merged automatically or not (don't
worry if they cannot be, we will work out the merge conflicts).

![Pull Request Branch Selected](/images/pull-request.png)

Once you've clicked the "Create Pull Request" button, you are presented with a
little comments box where you can explain your changes and anything else
necessary. Follow the template pre-layed out for you in that box.

Once you have filled out the information, click "Create Pull Request" and the
pull request will be submitted for review. At this point, your work will be
evaluated, tested, and your pull request will either be accepted and merged into
master or some additional feedback will be given detailing what needs to change
in order for your work to be accepted. Make those changes and post a new comment
stating that you have done so to trigger another review. This will repeat until
the pull request is accepted.

**IMPORTANT NOTE:** You must make sure that, before you submit a pull request,
your code compiles without warnings or errors and that it runs on the target
device. A feature must be, for all intents and purposes, fulfilled to completion
for the code to be merged into master. This is how we avoid broken things.
