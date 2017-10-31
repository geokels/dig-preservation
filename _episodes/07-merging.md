---
title: Merging branches
teaching:
exercises:
questions:
- "How do I merge one branch into another?"
objectives:
- "Understand how Git performs merges."
- "Perform a merge."
keypoints:
- "Merging applies commits from one branch to another."
- "Merge commits are used to glue two histories together."
---

In most cases, when you create a new branch, the plan is to eventually take the
changes you make on that branch and bring them back to `master` (or whichever
branch you started on).  Sometimes you will keep a branch around forever; for
example, you might have a permanent branch for a previous version of your
project (for example, the MODS 2.x schema).  But here we’ll go over the common
case: making a temporary branch, adding commits to it, and then merging it back
into `master`.

Start by re-creating the `title-caps` branch from the last episode, or checking
it out if you still have it:

~~~
$ git checkout -b title-caps
Switched to new branch 'title-caps'

$ git branch
  cleanup-plan
  master
* title-caps
~~~
{: .bash}

The student worker who was supposed to do this part of the cleanup didn’t show
up today, so you have to do it.  Open up your text editor, fix any titles that are in all caps, then
commit:

~~~
$ git add cats-human-situations.csv
$ git commit -m "fix capitalization of titles"

[title-caps 31170d3] fix capitalization of titles
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~
{: .bash}

As we saw in the previous episode, this change only exists on this branch.  When
we switch back to the `master` branch, the all-caps title is still there.

What we need is to _merge_ the change we made on the `title-caps` branch into
the `master` branch.  The first step in doing that is switching to the `master`
branch:

~~~
$ git checkout master
Switched to branch 'master'
~~~
{: .bash}

Now let’s say you notice a mistake in the README that you want to fix.  You can
go ahead and do that now; the `title-caps` branch will be there when you’re
ready to go back to it.  Maybe the fact that the README’s header isn’t
capitalized has been bugging you.  Whatever the case, make a change to
`README.md` and commit it to master:

~~~
$ git add README.md
$ git commit -m "capitalize README header"

[master 4268839] capitalize README header
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~
{: .bash}

That’s better.  Now let’s merge the `title-caps` fix, using `git merge` to bring
in the commits from the other branch:

~~~
$ git merge title-caps
~~~
{: .bash}

An editor window will appear.  Don’t be alarmed!  It will look like this:

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
`master` branch has a commit that `title-caps` doesn’t (we made that tweak to
the README), the histories of the two branches aren’t identical.  And since
the hashes of a commit is determined in part by the _previous commit_, a
different history means a different hash.  So we cannot just add our commit
`31170d3` (“fix capitalization of titles”) directly onto the `master` branch.

This is what merge commits are for: to glue together these divergent histories.
In your editor, save and quit to confirm the commit message.  You should see
the merge complete successfully:

~~~
Merge made by the 'recursive' strategy.
 cats-human-situations.csv | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~
{: .bash}

Now if you look at the last three commits on the `master` branch, all
our changes are present:

~~~
$ git log -n 3

commit 036009eb8b53c40dc8e55461925cfbc49dd5e766 (HEAD -> master)
Merge: 4268839 31170d3
Author: Alex Dunn <dunn.alex@gmail.com>
Date:   Fri Oct 27 14:54:56 2017 -0700

    Merge branch 'title-case'

    * title-case:
      fix capitalization of titles

commit 42688392476469bc0db59734045246aade014ccc
Author: Alex Dunn <dunn.alex@gmail.com>
Date:   Fri Oct 27 14:53:36 2017 -0700

    capitalize README header

commit 31170d3d22a49c9a8d90b01aef6092ee4ca3033e (title-caps)
Author: Alex Dunn <dunn.alex@gmail.com>
Date:   Fri Oct 27 14:15:25 2017 -0700

    fix capitalization of titles
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

~~~
=> git branch -d title-caps
Deleted branch title-caps (was 31170d3).
~~~
{: .bash}
