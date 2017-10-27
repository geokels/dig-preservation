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
- "`git branch -d delete_me` to delete a branch"
- "Checking out a branch changes the “active” files in your repository directory."
---

Branches are one of the most powerful features in Git and they are super easy to
use. Branches are great because they are easy to create, easy to delete, and
easy to work with. They allow you to experiment and try new ideas without
worrying about having to undo a bunch of commits in your master branch if it
doesn't work out. You can just create a new branch and test out your ideas
there.

Let's say that you want to try out a new reconciliation process for the subjects
in your project, but you have no idea what the results will be. Rather than
experimenting with the metadata in your master branch, you create a
`subject_recon` branch and test it out there. If you are happy with the results,
then you can incorporate those changes back into the master branch
through a process called merging, which we will talk about in the next section.
If not, you can delete the branch and move on with your work.

Branches are also great for isolating sections of work, for collaborating with
others, and managing workflows. For example, you have a student who will be
working with you on your project. You have asked them to ensure that none of the
titles are in all caps. You create a branch for them to work on the titles while
you continue to work on subject reconciliation. Once they are completed with
their work, you can review their changes before merging their changes back into the master
branch.

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

You don't have to create branches from, or merge changes to, the `master` branch. You can
have many branches and merge changes between those branches.

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

The branch name can branch whatever you want, but keep it simple: letters, numbers,
dashes, and underscores. You cannot use spaces or special characters. Make the names
useful and meaningful so you know what they refer to when you look at your list of
branches. Many choose to name their branches with the issue number from their ticketing
system.

Now create a branch to begin working on your plan for metadata cleanup.

~~~
$ git branch cleanup-plan
~~~
{: .bash}

When you hit return it will create the new branch.

Let's take a look by using git branch again.

~~~
$ git branch
  cleanup-plan
* master
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
$ git checkout cleanup-plan
Switched to branch 'cleanup-plan'
~~~
{: .bash}

You should see a confirmation message of the switch, but you can also confirm using git
branch.

~~~
$ git branch
* cleanup-plan
  master
~~~
{: .bash}

Right now the branches are identical, because no additional commits have been added to
either branch. If we switch between them, no files will change in our project directory.
Let's go ahead and make a commit on the new branch.

Type the text below at the end of the `README.md` file:

~~~
Cleanup plan:
- Get rid of all caps
- Standardize dates
- Standardize dimensions
~~~
{: .output}

Save the file and commit your changes.

~~~
$ git add README.md
$ git commit -m "add initial cleanup plan

[cleanup-plan 74eab99] add initial cleanup plan
 1 file changed, 5 insertions(+)
cat12-l0aj:cats-as-data-master crissmeyer$
~~~
{: .bash}

Now switch back to master using `git checkout` and view the log.

~~~
$ git checkout master
Switched to branch 'master'

$ git log --oneline
288d50b i forgot about adding the metadata
7321090 add pictures of cats
53671cf readme: where are the files?
69f1a5b add notes on metadata
~~~
{: .bash}

The commit you just made is not on the master branch. Reopen the `README.md` file. The
change you just made is not there.Close the file, switch the branch, and reopen the same
file. Your change is now there.

~~~
$ git checkout cleanup-plan
Switched to branch 'cleanup-plan'
~~~
{: .bash}

## Combining Creating & Checking Out

You can create and checkout a branch at the same time using `git checkout`
followed by `-b` and the name of your new branch.

Now create a branch to begin cleaning up the titles in your metadata.

~~~
$ git checkout -b title-caps
Switched to new branch 'title-caps'

$ git branch
  cleanup-plan
  master
* title-caps
~~~
{: .bash}

Let's look at the log again.

~~~
$ git log --oneline
74eab99 add initial cleanup plan
288d50b i forgot about adding the metadata
7321090 add pictures of cats
53671cf readme: where are the files?
69f1a5b add notes on metadata>
~~~
{: .bash}

You'll notice that because you were on the `cleanup-plan` branch when we made our new
branch, the new branch was created off of `cleanup-plan`.

## Tip!

Use `git log --graph --oneline --decorate --all` to view a simple graph version of your
branches.
~~~
$ git log --graph --oneline --decorate --all
* 74eab99 (HEAD -> title-caps, cleanup-plan) add initial cleanup plan
* 288d50b (master) i forgot about adding the metadata
* 7321090 add pictures of cats
* 53671cf readme: where are the files?
* 69f1a5b add notes on metadata
~~~
{: .bash}


## Renaming Branches

You can rename a branch using the `git branch -m` (move) command.

You've decided to use a ticketing system to track your work. Use the the move command
to rename your `title-caps` branch.

~~~
$ git branch -m title-caps MIGRATE-1

$ git branch
* MIGRATE-1
  cleanup-plan
  master
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
  cleanup-plan
  delete-me
* master
~~~
{: .bash}

Notice that I did not create and checkout the branch at the same time. This is important
because you cannot delete the branch you are on.

Delete your new branch using the `git branch -d` command followed by the name of
the branch.

~~~
$ git branch -d delete_me
Deleted branch delete-me (was 74eab99).
~~~
{: .bash}

Now the branch is gone.
