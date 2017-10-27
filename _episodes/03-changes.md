---
title: Creating a Repository and Tracking Changes
teaching: 20
exercises: 0
questions:
- "Where does Git store information?"
- "How do I record changes in Git?"
- "How do I check the status of my version control repository?"
- "How do I record notes about what changes I made and why?"
objectives:
- "Create a local Git repository."
- "Go through the modify-add-commit cycle for one or more files."
- "Explain where information is stored at each stage of Git commit workflow."
- "Distinguish between descriptive and non-descriptive commit messages."
keypoints:
- "`git init` initializes a repository."
- "`git status` shows the status of a repository."
- "Files can be stored in a project's working directory (which users see), the staging area (where the next commit is being built up) and the local repository (where commits are permanently recorded)."
- "`git add` puts files in the staging area."
- "`git commit` saves the staged content as a new commit in the local repository."
- "Always write a log message when committing changes."
---

Change your working directory to the `cats-as-data-master` directory you created
in the [setup section]({{ page.root }}/setup), and initialize a git repository
in it:

~~~
$ cd ~/Downloads/cats-as-data-master # or wherever you unzipped it
$ git init
~~~
{: .bash}

If we use `ls` to show the directory's contents, it appears that nothing has
changed.  But if we add the `-a` flag to show everything, we can see that Git
has created a hidden directory called `.git`:

~~~
$ ls -a
~~~
{: .bash}

~~~
.  ..  cats-human-situations.csv  .git  images  README.md
~~~
{: .output}

Git stores information about the project in this special sub-directory.  If we
ever delete it, we will lose the project's history.

We can check that everything is set up correctly by asking Git to tell us the
status of our project:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.md
        cats-human-situations.csv
        images/

nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

The "untracked files" message means that there are files in the directory that
Git isn't keeping track of.  We can tell Git to track a file using `git add`:

~~~
$ git add README.md
~~~
{: .bash}

and then check that the right thing happened:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        cats-human-situations.csv
        images/
~~~
{: .output}

Git now knows that it's supposed to keep track of `README.md`, but it
hasn't recorded these changes as a commit yet.  To get it to do that,
we need to run one more command:

~~~
$ git commit -m "add notes on metadata"
~~~
{: .bash}

~~~
[master (root-commit) c118dee] add notes on metadata
 1 file changed, 6 insertions(+)
 create mode 100644 readme.md
~~~
{: .output}

When we run `git commit`, Git takes everything we have told it to save
by using `git add` and stores a copy permanently inside the special
`.git` directory.  This permanent copy is called a [commit]({{
page.root }}/reference/#commit) (or [revision]({{ page.root
}}/reference/#revision)) and its short identifier is `c118dee`.  Your
commit may have another identifier; as to how this identifier is
chosen, we’ll get to that in a minute.

We use the `-m` flag (for "message") to record a short, descriptive,
and specific comment that will help us remember later on what we did
and why.  If we just run `git commit` without the `-m` option, Git
will launch `nano` (or whatever other editor we configured as
`core.editor`) so that we can write a longer message.

[Good commit messages][commit-messages] start with a brief (<50
characters) summary of changes made in the commit.  If you want to go
into more detail, add a blank line between the summary line and your
additional notes.

If we want to know what we've done recently, we can ask Git to show us the
project's history using `git log`:

~~~
$ git log
~~~
{: .bash}

~~~
commit 73f865ac9f0b3e790328d11ec6cd7653208a5a3e
Author: Catsy Cline <ccline@pawson.edu>
Date:   Thu Aug 22 09:51:46 2013 -0400

    add notes on metadata
~~~
{: .output}

`git log` lists all commits made to a repository in reverse
chronological order.  The listing for each commit includes the
commit's full identifier (which starts with the same characters as the
short identifier printed by the `git commit` command earlier), the
commit's author, when it was created, and the log message Git was
given when the commit was created.

> ## Where Are My Changes?
>
> If we run `ls` at this point, we will still see just one file called
> `README.md`.  That's because Git saves information about files'
> history in the special `.git` directory mentioned earlier so that
> our filesystem doesn't become cluttered (and so that we can't
> accidentally edit or delete an old version).
{: .callout}

Now suppose we add more information to the file.  Type the text below
at the end of the `README.md` file:

~~~
The images are in the `images/` subdirectory of this repository.
~~~
{: .output}

~~~
$ cat readme.md
~~~
{: .bash}

~~~
# about your new image collection

The `cats-human-situations.csv` file contains metadata for three image objects. The original metadata from the source institutions has been abbreviated and made messier so you have something to clean up!

Image credits:
- The black cat, March 1896 (http://ark.digitalcommonwealth.org/ark:/50959/w0892s818). Courtesy Boston Public Library via Digital Commonwealth.
- Puzzums in costume (http://photos.lapl.org/carlweb/jsp/DoSearch?index=z&databaseID=968&terms=0000068045). Courtesy Los Angeles Public Library.
- A Xmas shopper (http://digitalcollections.nypl.org/items/510d47e1-43b5-a3d9-e040-e00a18064a99). Courtesy New York Public Library.

The images are in the `images/` subdirectory of this repository.
~~~
{: .output}

When we run `git status` now, it tells us that a file it already knows
about has been modified:

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

        cats-human-situations.csv
        images/

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

The last line is the key phrase: "no changes added to commit".  We
have changed this file, but we haven't told Git we will want to save
those changes (which we do with `git add`) nor have we saved them
(which we do with `git commit`).  So let's do that now. It is good
practice to always review our changes before saving them. We do this
using `git diff`.  This shows us the differences between the current
state of the file and the most recently saved version:

~~~
$ git diff
~~~
{: .bash}

~~~
diff --git a/README.md b/README.md
index acbad52..17e3cdd 100644
--- a/README.md
+++ b/README.md
@@ -6,3 +6,5 @@ Image credits:
 - The black cat, March 1896 (http://ark.digitalcommonwealth.org/ark:/50959/w0892s818). Courtesy Boston Public Library via Digital Commonwealth.
 - Puzzums in costume (http://photos.lapl.org/carlweb/jsp/DoSearch?index=z&databaseID=968&terms=0000068045). Courtesy Los Angeles Public Library.
 - A Xmas shopper (http://digitalcollections.nypl.org/items/510d47e1-43b5-a3d9-e040-e00a18064a99). Courtesy New York Public Library.
+
+The images are in the `images/` subdirectory of this repository.
~~~
{: .output}

The output is cryptic because it is actually a series of
machine-readable commands for modifying one version of a file
(`a/README.md`) and producing another (`b/README.md`).  If we break it
down into pieces:

1.  The first line tells us that Git is producing output similar to
    the Unix `diff` command comparing the old and new versions of the
    file.
2.  The second line tells exactly which versions of the file Git is
    comparing; `acbad52` and `17e3cdd` are unique identifiers for
    those versions.
3.  The third and fourth lines once again show the name of the file
    being changed.
4.  The remaining lines are the most interesting, they show us the
    actual differences and the lines on which they occur.  In
    particular, the `+` marker in the first column shows where we
    added a line.  If we had instead removed lines, there would be `-`
    markers instead.

Because the output of `git diff` is a representation of the difference
between two files, it’s called (unsurprisingly) a diff.

After reviewing our change, it's time to commit it:

~~~
$ git add README.md
$ git commit -m "readme: where are the files?"
~~~
{: .bash}

> ## Terminology: diff vs. patch vs. commit
>
> What the version control system is concerned with is changes between states (of documents, images, etc.)
>
> - **diff** – a diff, broadly speaking, is a representation of the
>   difference between two states.
>
> - **patch** – a patch is just a diff with some metadata attached, such
>   as the author of the change and the time of authorship.  You can
>   generate something like the below with `git format-patch HEAD~1..HEAD`:
>
>     ~~~
>     From 7248345bacf4593e35c24398a4c375815ee006fc Mon Sep 17 00:00:00 2001
>     From: Catsy Cline <ccline@pawson.edu>
>     Date: Mon, 2 Oct 2017 12:43:54 -0700
>     Subject: [PATCH] readme: where are the files?
>
>     ---
>      README.md | 2 ++
>      1 file changed, 2 insertions(+)
>
>     diff --git a/README.md b/README.md
>     index acbad52..17e3cdd 100644
>     --- a/README.md
>     +++ b/README.md
>     @@ -6,3 +6,5 @@ Image credits:
>      - The black cat, March 1896 (http://ark.digitalcommonwealth.org/ark:/50959/w0892s818). Courtesy Boston Public Library via Digital Commonwealth.
>      - Puzzums in costume (http://photos.lapl.org/carlweb/jsp/DoSearch?index=z&databaseID=968&terms=0000068045). Courtesy Los Angeles Public Library.
>      - A Xmas shopper (http://digitalcollections.nypl.org/items/510d47e1-43b5-a3d9-e040-e00a18064a99). Courtesy New York Public Library.
>     +
>     +The images are in the `images/` subdirectory of this repository.
>     --
>     2.14.2
>     ~~~
>
> - **commit** – a commit is a patch that is managed by a version
>   control system.
>
> Viewed in this light, Git is just a tool for managing patches.  And
> the mysterious identifiers we’ve been seeing are actually [SHA1
> checksums of the patch headers!](https://gist.github.com/masak/2415865)
{: .callout}

You might be wondering why Git insists that we add files to the set we
want to commit before actually committing anything. This it because it
allows us to commit our changes in stages and capture changes in
logical portions rather than only large batches.  For example, suppose
we're adding a few citations to our supervisor's work to our thesis.
We might want to commit those additions, and the corresponding
addition to the bibliography, but *not* commit the work we're doing on
the conclusion (which we haven't finished yet).

To allow for this, Git has a special *staging area* where it keeps
track of things that have been added to the current [changeset]({{
page.root }}/reference/#changeset) but not yet committed.

> ## Staging Area
>
> If you think of Git as taking snapshots of changes over the life of
> a project, `git add` specifies *what* will go in a snapshot (putting
> things in the staging area), and `git commit` then *actually takes*
> the snapshot, and makes a permanent record of it (as a commit).  If
> you don't have anything staged when you type `git commit`, Git will
> prompt you to use `git commit -a` or `git commit --all`, which is
> kind of like gathering *everyone* for the picture!  However, it's
> almost always better to explicitly add things to the staging area,
> because you might commit changes you forgot you made. (Going back to
> snapshots, you might get the extra with incomplete makeup walking on
> the stage for the snapshot because you used `-a`!)  Try to stage
> things manually, or you might find yourself searching for "git undo
> commit" more than you would like!
{: .callout}

![The Git Staging Area](../fig/git-staging-area.svg)

Let's watch as our changes to a file move from our editor to the staging area
and into long-term storage.  First, we'll add more files to our repository:

~~~
$ git add images
$ git diff
~~~
{: .bash}

There is no output: as far as Git can tell, there's no difference
between what it's been asked to save permanently and what's currently
in the directory.  However, if we run `git diff --staged`, we’ll see
something interesting:

~~~
$ git diff --staged
diff --git a/images/00064183.jpg b/images/00064183.jpg
new file mode 100644
index 0000000..111d4ec
Binary files /dev/null and b/images/00064183.jpg differ
diff --git a/images/12_04_000583.jpg b/images/12_04_000583.jpg
new file mode 100644
index 0000000..6d6a306
Binary files /dev/null and b/images/12_04_000583.jpg differ
diff --git a/images/822811.jpg b/images/822811.jpg
new file mode 100644
index 0000000..0b8dd21
Binary files /dev/null and b/images/822811.jpg differ
~~~
{: .bash}

Since git is a command-line application, it doesn’t have native support for
visualizing the difference between binary files like images.  There are
[applications](https://kaleidoscopeapp.com/) that can show diffs of images, and
[GitHub can diff
images](https://help.github.com/articles/rendering-and-diffing-images/), but for
now we’ll be satisfied with seeing that we are adding binary data to our
repository.

~~~
$ git commit -m 'add pictures of cats'
~~~
{: .bash}

Now we can look at the history of what we've done so far:

~~~
$ git log
~~~
{: .bash}

~~~
commit c769a0d20bf6b3060cd3c752f4def937880fa4eb (HEAD -> master)
Author: Catsy Cline <ccline@pawson.edu>
Date:   Wed Oct 25 15:57:06 2017 -0700

    add pictures of cats

commit 06156ef7588b1359b467bd277b290f04b03fd094
Author: Catsy Cline <ccline@pawson.edu>
Date:   Wed Oct 25 15:47:58 2017 -0700

    readme: where are the files?

commit 73f865ac9f0b3e790328d11ec6cd7653208a5a3e
Author: Catsy Cline <ccline@pawson.edu>
Date:   Wed Oct 25 15:44:38 2017 -0700

    add notes on metadata
~~~
{: .output}

> ## Directories
>
> Two important facts you should know about directories in Git.
>
> 1. Git does not track directories on their own, only files within them.
> Try it for yourself:
>
> ~~~
> $ mkdir directory
> $ git status
> $ git add directory
> $ git status
> ~~~
> {: .bash}
>
> Note, our newly created empty directory `directory` does not appear in
> the list of untracked files even if we explicitly add it (_via_ `git add`) to our
> repository. This is the reason why you will sometimes see `.keep` files
> in otherwise empty directories. Unlike `.gitignore`, these files are not special
> and their sole purpose is to populate a directory so that Git adds it to
> the repository. In fact, you can name such files anything you like.
>
> {:start="2"}
> 2. If you create a directory in your Git repository and populate it with files,
> you can add all files in the directory at once by:
>
> ~~~
> git add <directory-with-files>
> ~~~
> {: .bash}
>
{: .callout}

To recap, when we want to add changes to our repository,
we first need to add the changed files to the staging area
(`git add`) and then commit the staged changes to the
repository (`git commit`):

![The Git Commit Workflow](../fig/git-committing.svg)

> ## Committing Multiple Files
>
> The staging area can hold changes from any number of files
> that you want to commit as a single snapshot.
>
> 1. Add more text to `README.md`
> 2. Tell git to begin tracking `cats-human-situations.csv`
> 3. Add changes from both files to the staging area,
> and commit those changes.
>
> > ## Solution
> >
> > First we make our changes.  First add some text to the README (whatever you
> feel is missing):
> > ~~~
> > $ nano README.md # or whichever editor you prefer
> > ~~~
> > {: .bash}
> >
> > Now you can add both files to the staging area. We can do that in one line:
> >
> > ~~~
> > $ git add README.md cats-human-behavior.csv
> > ~~~
> > {: .bash}
> > Or with multiple commands:
> > ~~~
> > $ git add README.md
> > $ git add cats-human-behavior.csv
> > ~~~
> > {: .bash}
> > Now the files are ready to commit. You can check that using `git status`. If you are ready to commit use:
> > ~~~
> > $ git commit -m "i forgot about adding the metadata!"
> > ~~~
> > {: .bash}
> > ~~~
> > [master 5988bdc] i forgot to add the metadata!
> >  2 files changed, 5 insertions(+), 1 deletion(-)
> >  create mode 100644 cats-human-situations.csv
> > ~~~
> > {: .output}
> >
> > (Note that your commit hash will not be `5988bdc`; it will depend on what
> > you added to `README.md`.)
> >
> {: .solution}
{: .challenge}

[commit-messages]: http://chris.beams.io/posts/git-commit/
