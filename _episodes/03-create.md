---
title: Creating a Repository
teaching: 10
exercises: 0
questions:
- "Where does Git store information?"
objectives:
- "Create a local Git repository."
keypoints:
- "`git init` initializes a repository."
---

Once Git is configured, we can start using it.  Let's create a
directory for our work and then move into that directory:

~~~
$ mkdir metadata
$ cd metadata
~~~
{: .bash}

Then we tell Git to make `metadata` a [repository]({{ page.root
}}/reference/#repository)—a place where Git can store versions of our
files:

~~~
$ git init
~~~
{: .bash}

If we use `ls` to show the directory's contents, it appears that
nothing has changed:

~~~
$ ls
~~~
{: .bash}

But if we add the `-a` flag to show everything, we can see that Git
has created a hidden directory within `metadata` called `.git`:

~~~
$ ls -a
~~~
{: .bash}

~~~
.	..	.git
~~~
{: .output}

Git stores information about the project in this special
sub-directory.  If we ever delete it, we will lose the project's
history.

We can check that everything is set up correctly by asking Git to tell
us the status of our project:

~~~
$ git status
~~~
{: .bash}

~~~
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
~~~
{: .output}

> ## Places to Create Git Repositories
>
> You start a new project, `csv`, related to the `metadata` project.
> You enter the following sequence of commands to create one Git
> repository inside another:
>
> ~~~
> $ cd             # return to home directory
> $ mkdir metadata # make a new directory ’metadata’
> $ cd metadata    # go into ’metadata’
> $ git init       # make the ’metadata’ directory a Git repository
> $ mkdir csv      # make a sub-directory metadata/csv
> $ cd csv         # go into metadata/csv
> $ git init       # make the csv sub-directory a Git repository
> ~~~
> {: .bash}
>
> Why is it a bad idea to do this? (Notice here that the `metadata`
> project is now also tracking the entire `csv` repository.)
>
> How can you undo your last `git init`?
>
> > ## Solution
> >
> > Git repositories can interfere with each other if they are "nested" in the
> > directory of another: the outer repository will try to version-control
> > the inner repository. Therefore, it's best to create each new Git
> > repository in a separate directory. To be sure that there is no conflicting
> > repository in the directory, check the output of `git status`. If it looks
> > like the following, you are good to go to create a new repository as shown
> > above:
> >
> > ~~~
> > $ git status
> > ~~~
> > {: .bash}
> > ~~~
> > fatal: Not a git repository (or any of the parent directories): .git
> > ~~~
> > {: .output}
> >
> > Note that we can track files in directories within a Git:
> >
> > ~~~
> > $ touch good.csv bad.csv            # create files
> > $ cd ..                             # return to planets directory
> > $ ls csv                            # list contents of the csv directory
> > $ git add csv/*                     # add all contents of metadata/csv
> > $ git status                        # show files in staging area
> > $ git commit -m "add big files"     # commit metadata/csv to planets Git repository
> > ~~~
> > {: .bash}
> >
> > Similarly, we can ignore entire directories, such as the `csv` directory:
> >
> > ~~~
> > $ nano .gitignore # open the .gitignore file in the texteditor to add the csv directory
> > $ cat .gitignore # if you run cat afterwards, it should look like this:
> > ~~~
> > {: .bash}
> >
> > ~~~
> > /csv
> > ~~~
> > {: .output}
> >
> > To recover from this little mistake, you can just remove the `.git`
> > folder in the ’csv’ subdirectory. To do so, run the following command from inside the 'csv’ directory:
> >
> > ~~~
> > $ rm -rf csv/.git
> > ~~~
> > {: .bash}
> >
> > But be careful! Running this command in the wrong directory, will remove
> > the entire git-history of a project you might wanted to keep. Therefore, always check your current directory using the
> > command `pwd`.
> {: .solution}
{: .challenge}
