---
title: Working with branches
teaching: 20
exercises: 0
questions:
- "What are branches and why should I use them?"
objectives:
- "Understand how Git implements branching."
- "Create and checkout a new branch."
keypoints:
- "`git branch` to view current branches"
- "`git branch branch-name` to create a new branch"
- "`git checkout branch-name` to switch to a branch"
- "`git branch -m branch-name` to rename a branch"
- "`git branch -d delete-me` to delete a branch"
- "Checking out a branch changes the “active” files in your repository directory."
---

Branches are one of the most powerful features in Git and they are very easy to
use. Branches are great because they are easy to create, easy to delete, and
easy to work with. They allow you to experiment and try new ideas without
worrying about having to undo a bunch of commits if it doesn't work out. You can just
create a new branch and test out your ideas there.

Let's say that you want to try out a new reconciliation process for the subjects
in your project, but you have no idea what the results will be. Rather than
experimenting with the metadata in your master branch, you create a
`subject_recon` branch and test it out there. If you are happy with the results,
then you can incorporate those changes back into the master branch
through a process called merging. If not, you can delete the branch and move on with your
work.

Branches are also great for isolating sections of work, for collaborating with
others, and managing workflows. For example, you have a student who will be
working with you on your project and you have asked them to ensure that none of the
titles are in all caps. You create a branch for them to work on the titles while
you continue to work on subject reconciliation. Once they finish their work, you can
review their changes before merging their changes back into the master branch.

When you create a branch, you are still going to have just one directory of
files in your project. All of the files that you are working with are still
going to be in same place. When you switch branches Git takes all of the files in your
project directory and swaps them out so that they match what is on that branch.

Let's look at an illustration.

![Branching Illustrated](../fig/branching.gif)

You are on your `master` branch, and have made several commits to it. Then you decide you
want to test out your new reconciliation process. You create and switch to the
`subject_recon` branch and commit your changes.

These new commits are not on the `master` branch. Each time you switch between the
branches, git switches out the files in your project directory. You can continue to make
commits to `master`, but they will not appear in the `subject_recon` branch.

If you are happy with your experiment, you can incorporate those changes into `master` by
merging the `subject_recon` branch. During this process Git will create a new commit that
merges the changes from the `subject_recon` branch. You can then delete the
`subject_recon` branch, or continue to make changes, periodically merging them
back in.

You don't have to always create branches from, or merge changes to, the `master` branch.
You can have many branches and merge changes between those branches.

## Viewing Branches

Start by looking at current branches on local machine using the `git branch` command.

~~~
$ git branch
* master
~~~
{: .bash}

The asterisk tells you which branch you are on, also referred to as the branch that
is checked out.

## Creating Branches

You create a branch using `git branch` followed by the name of the new branch.

You can name the branch whatever you want, but keep it simple: letters, numbers,
dashes, and underscores. You cannot use spaces or special characters. Make the names
useful and meaningful so you know what they refer to when you look at your list of
branches. Many choose to name their branches with the issue number from their ticketing
system.

Now create a branch to begin working on the metadata cleanup. Let's start by fixing the
titles that are in all caps.

~~~
$ git branch title-caps
~~~
{: .bash}

When you hit return it will create the new branch.

Let's take a look by using git branch again.

~~~
$ git branch
* master
  title-caps
~~~
{: .bash}

You can see by the asterisk that you are still on the master branch. You can also see
which branch you are on by using 'git status'.

~~~
$ git status
On branch master
nothing to commit, working tree clean
~~~
{: .bash}

## Switching Branches

To switch to your new branch, you use the `git checkout` command.

~~~
$ git checkout title-caps
Switched to branch 'title-caps'
~~~
{: .bash}

You should see a confirmation message of the switch, but you can also confirm using git
branch.

~~~
$ git branch
  master
* title-caps
~~~
{: .bash}

>Tip: You can create and checkout a branch at the same time using `git checkout`
>followed by `-b` and the name of your new branch.

Right now the branches are identical, because no additional commits have been added to
either branch. If we switch between them, no files will change in our project directory.
Let's go ahead and make a commit on the new branch.

Open `cats-human-situations.csv` file in your text editor (you may need to right click
if your computer defaults to opening .csv files in Excel) and fix the all caps title. Save
and close the file, then commit your changes.

~~~
$ git add cats-human-situations.csv

$ git commit -m "fix all caps in titles"
[title-caps 118d855] fix all caps in titles
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~
{: .bash}

Let's take a look at our log using `git log --oneline`.

~~~
git log --oneline
118d855 fix all caps in titles
fd7cdc7 i forgot about adding the metadata
9fef33a add pictures of cats
a173d54 readme: where are the files?
c08d380 add notes on metadata
~~~
{: .bash}

Now switch back to master using `git checkout` and view the log again.

~~~
$ git checkout master
Switched to branch 'master'

$ git log --oneline
fd7cdc7 i forgot about adding the metadata
9fef33a add pictures of cats
a173d54 readme: where are the files?
c08d380 add notes on metadata
~~~
{: .bash}

The commit you just made is not on the `master` branch. Reopen the
`cats-human-situations.csv` file. The change you just made is not there.

You just realized that you wanted to add a clean-up plan to the `README.md` file
in your `master` branch before beginning the cleanup\. Open `README.md` in your text
editor and then add the following text at the bottom of the file:

~~~
Cleanup plan:
- Get rid of all caps in titles
- Standardize dates
- Standardize dimensions
~~~
{: .output}

Save the file and commit your changes.

~~~
$ git add README.md

$ git commit -m "add cleanup plan"
[master e4a5780] add cleanup plan
 1 file changed, 6 insertions(+), 1 deletion(-)
~~~
{: .bash}

Let's now view a simple graph version of your branches using
`git log --graph --oneline --decorate --all`. This is a great command for getting an
overview of the current state of your branches.

~~~
$ git log --graph --oneline --decorate --all
* e4a5780 (HEAD -> master) add cleanup plan
| * 118d855 (title-caps) fix all caps in titles
|/
* fd7cdc7 i forgot about adding the metadata
* 9fef33a add pictures of cats
* a173d54 readme: where are the files?
* c08d380 add notes on metadata
~~~
{: .bash}

## Renaming Branches

You can rename a branch using the `git branch -m` (move) command.

Let's rename the `title-caps` branch to use the issue number `MIGRATE-1` from your
ticketing system.

~~~
$ git branch -m title-caps MIGRATE-1

$ git branch
  master
* MIGRATE-1
~~~
{: .bash}





## Deleting Branches

Now let's learn how to delete a branch. Switch to the master branch.

~~~
$ git checkout master
Switched to branch 'master'
~~~
{: .bash}

Now create a new branch called `delete-me` using the `git branch` command.

~~~
$ git branch delete-me

$ git branch
  MIGRATE-1
  title-caps
  delete-me
* master
~~~
{: .bash}

Notice that I did not create and checkout the branch at the same time. This is important
because you cannot delete the branch you are on.

Delete your new branch using the `git branch -d` command followed by the name of
the branch.

~~~
$ git branch -d delete-me
Deleted branch delete-me (was 74eab99).
~~~
{: .bash}

Now the branch is gone.
