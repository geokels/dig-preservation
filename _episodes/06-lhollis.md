---
title: Working with Branches
teaching: 30
exercises: 0
questions:
- "What are branches and why should I use them?"
- "How do I merge one branch into another?"
objectives:
- "Understand how Git implements branching."
- "Create and checkout a new branch."
- "Understand how Git performs merges."
- "Perform a merge."
- "Delete a branch."

keypoints:
- "`git branch` to view current branches"
- "`git branch branch-name` to create a new branch"
- "`git checkout branch-name` to switch to a branch"
- "`git branch -m branch-name` to rename a branch"
- "`git merge branch-name` to merge a branch"
- "`git branch -d delete-me` to delete a branch"
- "Checking out a branch changes the “active” files in your repository directory."
- "Merging applies commits from one branch to another."
- "Merge commits are used to glue two histories together."
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
you can incorporate those changes back into the master branch through a process called
merging. If not, you can delete the branch and move on with your work.

Branches are great for isolating sections of work, for collaborating with
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

>Tip: If need to, you can rename a branch using the `git branch -m` (move) command. The
syntax is >`git branch -m old-name new-name`.
>
>~~~
>$ git branch -m title-cads title-caps
>~~~
>{: .bash}

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

Now let's say you just realized that you wanted to add a clean-up plan to the `README.md`
file in your `master` branch before beginning the cleanup. You can
go ahead and do that now; the `title-caps` branch will be there when you’re
ready to go back to it. Open `README.md` in your text editor and then add the following
text at the bottom of the file:

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


## Merging Branches

In most cases, when you create a new branch, the plan is to eventually take the
changes you make on that branch and bring them back to `master` (or whichever
branch you started on).  Sometimes you will keep a branch around forever; for
example, you might have a permanent branch with sample files.  But here we’ll go over the
common case: making a temporary branch, adding commits to it, and then merging it back
into `master`.

As we learned earlier, the title edits only exist in the `title-caps` branch. When
we switch back to the `master` branch, the all-caps title is still there.

What we need is to _merge_ the change we made on the `title-caps` branch into
the `master` branch. You should already be on the `master` branch, but if you are not,
switch back to it now.

~~~
$ git checkout master
Switched to branch 'master'
~~~
{: .bash}

Now let’s merge the `title-caps` fix, using `git merge` to bring
in the commits from the other branch:

~~~
$ git merge title-caps
~~~
{: .bash}

If an editor window will appears, don’t be alarmed!  It will look like this:

~~~
Merge branch 'title-caps'

* title-caps:
  fix capitalization of titles

# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
~~~
{: .bash}

This is just Git asking you to provide a commit message.

You might be wondering, “I’m just adding already-existing commits to my `master`
branch!  They already have commit messages, why do I need to write a new one?”

What’s happening here is that Git is creating a _merge commit_.  Because our
`master` branch has a commit that `title-caps` doesn’t (we added our clean-up plan to
the README), the histories of the two branches aren’t identical.  And since
the hashes of a commit is determined in part by the _previous commit_, a
different history means a different hash.  So we cannot just add our commit
`118d855` (“fix all caps in titles”) directly onto the `master` branch.

This is what merge commits are for: to glue together these divergent histories.
In your editor, save and quit to confirm the commit message.  You should see
the merge complete successfully:

~~~
Merge made by the 'recursive' strategy.
 cats-human-situations.csv | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~

Now if you look at the last three commits on the `master` branch, all
our changes are present:

~~~
$ git log --oneline
afb1372 Merge branch 'title-caps'
e4a5780 add cleanup plan
118d855 fix all caps in titles
fd7cdc7 i forgot about adding the metadata
9fef33a add pictures of cats
a173d54 readme: where are the files?
c08d380 add notes on metadata
~~~
{: .bash}

The `title-caps` branch can now be deleted.  If you want to double-check whether
all the changes from a particular branch have been merged into `master`, you can
run `git branch --merged` from the `master branch`:

~~~
$ git checkout master

$ git branch --merged
* master
  title-caps
~~~
{: .bash}

`master` itself is there since technically, it has been merged into itself.
But more importantly, this proves that there’s nothing left on `title-caps` that
isn’t also on your `master` branch, so it’s safe to discard:

## Deleting Branches

Now let's learn how to delete a branch by deleting our newly merged `title-caps` branch.
We are already on the `master` branch, but if we were not, we would need to check it out
first -- you cannot delete the branch you are currently on.

You delete a branch using `git branch -d` followed by the name of
the branch.

~~~
$ git branch -d title-caps
Deleted branch title-caps (was 118d855).
~~~
{: .bash}

Your title-caps branch is now gone.
