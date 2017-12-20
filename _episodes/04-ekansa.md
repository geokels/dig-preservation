---
title: Exploring History
teaching: 25
exercises: 0
questions:
- "How can I identify old versions of files?"
- "How do I review my changes?"
- "How can I recover old versions of files?"
objectives:
- "Explain what the HEAD of a repository is and how to use it."
- "Identify and use Git commit numbers."
- "Compare various versions of tracked files."
- "Restore old versions of files."
keypoints:
- "`git diff` displays differences between commits."
- "`git checkout` recovers old versions of files."
---

As we saw in the previous lesson, we can refer to commits by their
identifiers.  You can refer to the _most recent commit_ of the working
directory by using the identifier `HEAD`.

We've been adding one line at a time to `README.md`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make another change to `README.md`. Open the file in your editor, and add another line:

~~~
If you would like, you can also put images of dogs and hexagonal software logos here.
~~~
{: .output}

Your `README.md` should now look like:
~~~
# about your new image collection

The `cats-human-situations.csv` file contains metadata for three image objects.
The original metadata from the source institutions has been abbreviated and made
messier so you have something to clean up!

The images are in the `images/` subdirectory of this repository.
If you would like, you can also put images of dogs and hexagonal software logos here.

~~~
{: .output}

The real goodness in all this is when you can refer to previous commits.
We do that by adding `~1` to refer to the commit one before `HEAD`.

~~~
$ git diff HEAD~1 README.md
~~~
{: .bash}

If we want to see the differences between older commits we can use `git diff`
again, but with the notation `HEAD~1`, `HEAD~2`, and so on, to refer to them:

~~~
$ git diff HEAD~2 README.md
~~~
{: .bash}

~~~
diff --git a/README.md b/README.md
index f2a73f2..ef91fa5 100755
--- a/README.md
+++ b/README.md
@@ -3,3 +3,6 @@
 The `cats-human-situations.csv` file contains metadata for three image objects.
 The original metadata from the source institutions has been abbreviated and made
 messier so you have something to clean up!
+
+The images are in the `images/` subdirectory of this repository.
+If you would like, you can also put images of dogs and hexagonal software logos here.

~~~
{: .output}

We could also use `git show` which shows us what changes we made at an older commit as
well as the commit message, rather than the _differences_ between a commit and our working
directory that we see by using `git diff`.

~~~
$ git show HEAD~2 README.md
~~~
{: .bash}

~~~
commit e46e6eb89b9b4c7d95e9ea21337d9b29f78930b8
Author: Catsy Cline <ccline@pawson.edu>
Date:   Sun Nov 5 14:40:08 2017 -0600

    add notes on metadata, and the metadata

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..f2a73f2
--- /dev/null
+++ b/README.md
@@ -0,0 +1,5 @@
+# about your new image collection
+
+The `cats-human-situations.csv` file contains metadata for three image objects.
+The original metadata from the source institutions has been abbreviated and made
+messier so you have something to clean up!
~~~
{: .output}

In this way, we can build up a chain of commits.  The most recent end of the
chain is referred to as `HEAD`; we can refer to previous commits using the `~`
notation, so `HEAD~1` (pronounced "head minus one") means "the previous commit",
while `HEAD~123` goes back 123 commits from where we are now.

We can also refer to commits using those long strings of digits and letters that
`git log` displays.  Our first commit was given the ID
`e46e6eb89b9b4c7d95e9ea21337d9b29f78930b8`, so let's try this (your starting
commit may have a different hash!):

~~~
$ git diff e46e6eb89b9b4c7d95e9ea21337d9b29f78930b8 README.md
~~~
{: .bash}

~~~
diff --git a/README.md b/README.md
index f2a73f2..ef91fa5 100644
--- a/README.md
+++ b/README.md
@@ -3,3 +3,6 @@
 The `cats-human-situations.csv` file contains metadata for three image objects.
 The original metadata from the source institutions has been abbreviated and made
 messier so you have something to clean up!
+
+The images are in the `images/` subdirectory of this repository.
+If you would like, you can also put images of dogs and hexagonal software logos here.
~~~
{: .output}

That's the right answer, but typing out random 40-character strings is
annoying, so Git lets us use just the first few characters, e.g.: `git diff 1ecad50 README.md`.

All right! So we can save changes to files and see what we've changed—now how
can we restore older versions of things?  Let's suppose we accidentally
overwrite our file. Open `README.md` in your editor and replace the contents
with the following line:

~~~
Dogs and hexagonal software logos as data.
~~~
{: .output}

`git status` now tells us that the file has been changed,
but those changes haven't been staged:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore
        cats-human-situations.md

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

We can put things back the way they were by using `git checkout`:

~~~
$ git checkout HEAD README.md
~~~
{: .bash}

As you might guess from its name,`git checkout` checks out (i.e., restores) an
old version of a file. In this case, we're telling Git that we want to recover
the version of the file recorded in `HEAD`, which is the last saved commit. If
we want to go back even further, we can use a commit identifier instead:

~~~
$ git checkout e46e6e README.md
~~~
{: .bash}

You can now open the `README.md` file in your editor to confirm you have the
original version of the file again.  Git has, of course, noticed:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	cats-human-situations.csv

nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

Again, we can put things back the way they were by using `git checkout`:

~~~
$ git checkout -f master README.md
~~~
{: .bash}

> ## Don't Lose Your HEAD
>
> Above we used
>
> ~~~
> $ git checkout e46e6e README.md
> ~~~
> {: .bash}
>
> to revert `README.md` to its state after the commit `e46e6e`.
> If you forget `README.md` in that command, Git will tell you that "You are in
> 'detached HEAD' state." In this state, you shouldn't make any changes.
> You can fix this by reattaching your head using ``git checkout master``
{: .callout}

It's important to remember that we must use the commit number that identifies
the state of the repository *before* the change we're trying to undo.  A common
mistake is to use the number of the commit in which we made the change we're
trying to get rid of.  In the example below, we want to retrieve the state from
before the most recent commit (`HEAD~1`), which is commit `2199d6`:

![Git Checkout](../fig/git-checkout.svg)

> ## Simplifying the Common Case
>
> If you read the output of `git status` carefully,
> you'll see that it includes this hint:
>
> ~~~
> (use "git checkout -- <file>..." to discard changes in working directory)
> ~~~
> {: .bash}
>
> As it says,
> `git checkout` without a version identifier restores files to the state saved in `HEAD`.
> The double dash `--` is needed to separate the names of the files being recovered
> from the command itself:
> without it,
> Git would try to use the name of the file as the commit identifier.
{: .callout}

The fact that files can be reverted one by one tends to change the way people organize
their work. If everything is in one large document, it's hard (but not impossible) to undo
changes to the introduction without also undoing changes made later to the conclusion.
If the introduction and conclusion are stored in separate files, on the other hand,
moving backward and forward in time becomes much easier.

> ## Recovering Older Versions of a File
>
> Jennifer has made changes to the Python script that she has been working on for weeks, and the
> modifications she made this morning "broke" the script and it no longer runs. She has spent
> ~ 1hr trying to fix it, with no luck...
>
> Luckily, she has been keeping track of her project's versions using Git! Which commands below will
> let her recover the last committed version of her Python script called
> `data_cruncher.py`?
>
> 1. `$ git checkout HEAD`
>
> 2. `$ git checkout HEAD data_cruncher.py`
>
> 3. `$ git checkout HEAD~1 data_cruncher.py`
>
> 4. `$ git checkout <unique ID of last commit> data_cruncher.py`
>
> 5. Both 2 and 4
{: .challenge}

> ## Reverting a Commit
>
> Jennifer is collaborating on her Python script with her colleagues and
> realizes her last commit to the group repository is wrong and wants to
> undo it.  Jennifer needs to undo correctly so everyone in the group
> repository gets the correct change.  `git revert [wrong commit ID]`
> will make a new commit that undoes Jennifer's previous wrong
> commit. Therefore `git revert` is different than `git checkout [commit
> ID]` because `checkout` is for local changes not committed to the
> group repository.  Below are the right steps and explanations for
> Jennifer to use `git revert`, what is the missing command?
>
> 1. ________ # Look at the git history of the project to find the commit ID
>
> 2. Copy the ID (the first few characters of the ID, e.g. 0b1d055).
>
> 3. `git revert [commit ID]`
>
> 4. Type in the new commit message.
>
> 5. Save and close
{: .challenge}

> ## Understanding Workflow and History
>
> What is the output of cat CONTRIBUTING.md at the end of this set of commands?
>
> ~~~
> $ cd cats-as-data
> $ nano CONTRIBUTING.md #input the following text: Please submit changes in a Pull Request
> $ git add CONTRIBUTING.md
> $ nano CONTRIBUTING.md #add the following text: Don't forget cat images, please.
> $ git commit -m "Add CONTRIBUTING.md to project"
> $ git checkout HEAD CONTRIBUTING.md
> $ cat CONTRIBUTING.md #this will print the contents of CONTRIBUTING.md to the screen
> ~~~
> {: .bash}
>
> 1.
>
> ~~~
> Please submit changes in a Pull Request
> ~~~
> {: .output}
>
> 2.
>
> ~~~
> Don't forget cat images, please.
> ~~~
> {: .output}
>
> 3.
>
> ~~~
> Please submit changes in a Pull Request
> Don't forget cat images, please.
> ~~~
> {: .output}
>
> 4.
>
> ~~~
> Error because you have changed CONTRIBUTING.md without committing the changes
> ~~~
> {: .output}
>
> > ## Solution
> >
> > Line by line:
> > ~~~
> > $ cd cats-as-data
> > ~~~
> > {: .bash}
> > Enters into the 'cats-as-data' directory
> >
> > ~~~
> > $ nano CONTRIBUTING.md #input the following text: Please submit changes in a Pull Request
> > ~~~
> > {: .bash}
> > We created a new file and wrote a sentence in it, but the file is not tracked by git.
> >
> > ~~~
> > $ git add CONTRIBUTING.md
> > ~~~
> > {: .bash}
> > Now the file is staged. The changes that have been made to the file until now will be committed in the next commit.
> >
> > ~~~
> > $ nano CONTRIBUTING.md #add the following text: Don't forget cat images, please.
> > ~~~
> > {: .bash}
> > The file has been modified. The new changes are not staged because we have not added the file.
> >
> > ~~~
> > $ git commit -m "Add CONTRIBUTING.md to project"
> > ~~~
> > {: .bash}
> > The changes that were staged (Please submit changes in a Pull Request) have been committed. The changes that were not staged (Don't forget cat images, please.) have not. Our local working copy is different than the copy in our local repository.
> >
> > ~~~
> > $ git checkout HEAD CONTRIBUTING.md
> > ~~~
> > {: .bash}
> > With checkout we discard the changes in the working directory so that our local copy is exactly the same as our HEAD, the most recent commit.
> >
> > ~~~
> > $ cat CONTRIBUTING.md #this will print the contents of CONTRIBUTING.md to the screen
> > ~~~
> > {: .bash}
> > If we print CONTRIBUTING.md we will get answer 2.
> >
> {: .solution}
{: .challenge}

> ## Checking Understanding of `git diff`
>
> Consider this command: `git diff HEAD~3 README.md`. What do you predict this command
> will do if you execute it? What happens when you do execute it? Why?
>
> Try another command, `git diff [ID] README.md`, where [ID] is replaced with
> the unique identifier for your most recent commit. What do you think will happen,
> and what does happen?
{: .challenge}

> ## Getting Rid of Staged Changes
>
> `git checkout` can be used to restore a previous commit when unstaged changes have
> been made, but will it also work for changes that have been staged but not committed?
> Make a change to `README.md`, add that change, and use `git checkout` to see if
> you can remove your change.
{: .challenge}

> ## Explore and Summarize Histories
>
> Exploring history is an important part of git, often it is a challenge to find
> the right commit ID, especially if the commit is from several months ago.
>
> Imagine the `cats-as-metadata` project has more than 50 files.
> You would like to find a commit with specific text in `README.md` is modified.
> When you type `git log`, a very long list appeared,
> How can you narrow down the search?
>
> Recall that the `git diff` command allow us to explore one specific file,
> e.g. `git diff README.md`. We can apply the similar idea here.
>
> ~~~
> $ git log README.md
> ~~~
> {: .bash}
>
> Unfortunately some of these commit messages are very ambiguous e.g. `update files`.
> How can you search through these files?
>
> Both `git diff` and `git log` are very useful and they summarize different part of the history for you.
> Is that possible to combine both? Let's try the following:
>
> ~~~
> $ git log --patch README.md
> ~~~
> {: .bash}
>
> You should get a long list of output, and you should be able to see both commit messages and the difference between each commit.
>
> Question: What does the following command do?
>
> ~~~
> $ git log --patch HEAD~3 *.md
> ~~~
> {: .bash}
{: .challenge}
