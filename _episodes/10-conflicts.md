---
title: Conflicts
teaching: 15
exercises: 0
questions:
- "What do I do when my changes conflict with someone else's?"
objectives:
- "Explain what conflicts are and when they can occur."
- "Resolve conflicts resulting from a merge."
keypoints:
- "Conflicts occur when two or more people change the same file(s) at the same time."
- "The version control system does not allow people to overwrite each other's changes 
blindly, but highlights conflicts so that they can be resolved."
---

As soon as people can work in parallel, it's likely someone's going to step on someone
else's toes.  This will even happen with a single person: if you are updating a file on two
computers or two branches, you could make different changes to each copy. Version control 
helps us manage these [conflicts]({{ page.root }}/reference/#conflicts) by giving us tools 
to [resolve]({{ page.root }}/reference/#resolve) overlapping changes.

Even if you prefer working on the command line, this is one area where you might want to 
consider using a GUI tool. Command line Git is not great at resolving merge conflicts. So
that you can decide for yourself, we have included both the command line instructions and
an illustration of resolving a merge conflict using GitKraken below.

To see how we can resolve conflicts, we must first create one.  So that you can practice 
resolving merge conflicts on your own, you are going to create a conflict between two
branches in your local repository. However, you could also test this out by having both
you and a collaborator make a change to the same file in a remote repository.

To start, create a new branch in your local repository called `update-plan`. The file 
`README.md` currently looks like this in both branches of your repository.

~~~
# about your new image collection

The `cats-human-situations.csv` file contains metadata for three image objects.
The original metadata from the source institutions has been abbreviated and made
messier so you have something to clean up!

The images are in the `images/` subdirectory of this repository.

Cleanup plan:
- Get rid of all caps in titles
- Standardize dates
- Standardize dimensions
~~~
{: .output}

Let's add a line to the copy in `master` branch. Open README.md in your text editor and 
then add the following text at the bottom of the file:

~~~
- Reconcile subjects
~~~
{: .output}

Save the file and commit your changes.

~~~
$ git add README.md
~~~
{: .bash}
~~~
$ git commit -m "add subject reconcillation to plan"
[master 9025b6e] add subject reconcillation to plan
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~
{: .output}

Now let's switch to the `update-plan` branch and make a different change to that copy of the
`README.md` *without* incorporating the change from the `master` branch first.

~~~
$ git checkout update-plan
~~~
{: .bash}
~~~
Switched to branch 'update-plan'
~~~
{: .output}

Open `README.md` in your text editor and then add the following text at the bottom of the 
file:

~~~
- Add access permissions
~~~
{: .output}

Save the file and commit your changes. Notice that it was no problem to commit the changes
on the `update-plan` branch, but what happens if we try and merge the two?

First, switch back to `master` branch.

~~~
$ git checkout master
~~~
{: .bash}
~~~
Switched to branch 'master'
~~~
{: .output}

Now let's try to merge the `update-plan` branch.

~~~
$ git merge update-plan
~~~
{: .bash}
~~~
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
~~~
{: .output}

![The Conflicting Changes](../fig/conflict.svg)

Git tells us there is a conflict, and marks that conflict in the affected file. Git 
detects that the changes made in one copy overlap with those made in the other and stops 
us from trampling on our previous work. 

Open `README.me` in your text editor.

~~~
# about your new image collection

The `cats-human-situations.csv` file contains metadata for three image objects.
The original metadata from the source institutions has been abbreviated and made
messier so you have something to clean up!

The images are in the `images/` subdirectory of this repository.

Cleanup plan:
- Get rid of all caps in titles
- Standardize dates
- Standardize dimensions
<<<<<<< HEAD
- Reconcile subjects.
=======
- Add access permissions
>>>>>>> update-plan
~~~
{: .output}

Our change on the `master` branch, the one in `HEAD`—is preceded by <<<<<<<. Git has then
inserted ======= as a separator between the conflicting changes and marked the end of the 
content from the `update-plan` branch with >>>>>>> followed by the name of the branch. 

It is now up to us to edit this file to remove these markers and reconcile the changes. 
We can do anything we want: keep the change made in `master`, keep the change made in 
`update-plan`, write something new to replace both, or get rid of the change entirely. 
Since these are both tasks we want to do, let's keep them both. Update the file so it 
looks like this:

~~~
# about your new image collection

The `cats-human-situations.csv` file contains metadata for three image objects.
The original metadata from the source institutions has been abbreviated and made
messier so you have something to clean up!

The images are in the `images/` subdirectory of this repository.

Cleanup plan:
- Get rid of all caps in titles
- Standardize dates
- Standardize dimensions
- Reconcile subjects.
- Add access permissions
~~~
{: .output}

To finish merging, we add save and close the file. Then add `README.md` to the changes 
being made by the merge:

~~~
git add README.md
~~~
{: .bash}

Let's use `git status` to see what is going on.

~~~
$ git status
~~~
{: .bash}
~~~
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   README.md
~~~
{: .output}

Now let's commit those changes to complete the merge.

~~~
git commit -m "Merge changes from update-plan branch"
~~~
{: .bash}
~~~
[master 7bc42af] Merge changes from update-plan branch
~~~
{: .output}


Git's ability to resolve conflicts is very useful, but conflict resolution
costs time and effort, and can introduce errors if conflicts are not resolved
correctly. If you find yourself resolving a lot of conflicts in a project,
consider these technical approaches to reducing them:

- Pull from upstream more frequently, especially before starting new work
- Use topic branches to segregate work, merging to master when complete
- Make smaller more atomic commits
- Where logically appropriate, break large files into smaller ones so that it is
  less likely that two authors will alter the same file simultaneously

Conflicts can also be minimized with project management strategies:

- Clarify who is responsible for what areas with your collaborators
- Discuss what order tasks should be carried out in with your collaborators so
  that tasks expected to change the same lines won't be worked on simultaneously
- If the conflicts are stylistic churn (e.g. tabs vs. spaces), establish a
  project convention that is governing and use code style tools (e.g.
  `htmltidy`, `perltidy`, `rubocop`, etc.) to enforce, if necessary
  
## Resolving Merge Conflict with GUI
Let's see how one would resolve the same merge conflict using GitKraken.



## Conflicts When Working with Remotes

If your merge conflict is with a remote repository because you are attempting to push 
changes prior to pulling in new changes locally, you will receive a message similar to
this:

~~~
$ git push origin master
~~~
{: .bash}
~~~
To https://github.com/ccline/cats-as-data.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/ccline/cats-as-data.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
~~~
{: .output}

What you have to do in this situation is pull the changes from the remote, merge them into 
the copy you're currently working in, and then push that. Start by pulling:

~~~
$ git pull origin master
~~~
{: .bash}
~~~
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 3 (delta 1)
Unpacking objects: 100% (3/3), done.
From https://github.com/ccline/cats-as-data
 * branch            master     -> FETCH_HEAD
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
~~~
{: .output}

`git pull` tells us there’s a conflict, and marks that conflict in the affected file the
same way we saw above when we tried to merge our two local branches. To fix you would 
repeat the same steps you followed above.

First you would open the file in your text editor and remove the markers and reconcile the
changes. You'd then finish merging by adding and committing those changes.

~~~
git add README.md

git commit -m "Merge changes from remote"
~~~
{: .bash}
~~~
[master 7bc42af] Merge changes from remote
~~~
{: .output}

Finally, you'd push those changes back to the remote repository.

~~~
$ git push origin master
~~~
{: .bash}
~~~
Counting objects: 10, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 697 bytes, done.
Total 6 (delta 2), reused 0 (delta 0)
To https://github.com/ccline/cats-as-data.git
   dabb4c8..2abf2b1  master -> master
~~~
{: .output}

## Conflicts on Non-textual Files

What does Git do when there is a conflict in an image or some other non-textual file
that is stored in version control?

When there is a conflict on an image or other binary file, git prints a message like this:

~~~
$ git pull origin master
~~~
{: .bash}
~~~
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/ccline/cats-as-data.git
* branch            master     -> FETCH_HEAD
6a67967..439dc8c  master     -> origin/master
warning: Cannot merge binary files: 822811.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
Auto-merging 822811.jpg
CONFLICT (add/add): Merge conflict in 822811.jpg
Automatic merge failed; fix conflicts and then commit the result.
~~~
{: .output}

The conflict message here is mostly the same as it was for `README.md`, but
there is one key additional line:

~~~
warning: Cannot merge binary files: 822811.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
~~~

Git cannot automatically insert conflict markers into a non-text file as it 
does for text files. So, instead of editing the file, we must check out
the version we want to keep. Then we can add and commit this version.

On the key line above, Git has conveniently given us commit identifiers
for the two versions of `822811.jpg`. Our version is `HEAD`, and our Collaborator's
version is `439dc8c0...`. If we want to use our version, we can use
`git checkout`:

~~~
$ git checkout HEAD 822811.jpg
$ git add 822811.jpg
$ git commit -m "Use un-cropped version image instead of cropped"
~~~
{: .bash}

~~~
[master 21032c3] Use un-cropped version image instead of cropped
~~~
{: .output}

If instead we want to use our Collaborator's version, we can use `git checkout` with
the Collaborators's commit identifier, `439dc8c0`:

~~~
$ git checkout 439dc8c0 822811.jpg
$ git add 822811.jpg
$ git commit -m "Use cropped version instead of un-cropped"
~~~
{: .bash}

~~~
[master da21b34] Use cropped version instead of un-cropped
~~~
{: .output}

We can also keep *both* images. The catch is that we cannot keep them
under the same name. But, we can check out each version in succession
and *rename* it, then add the renamed versions. First, check out each
image and rename it:

~~~
$ git checkout HEAD 822811.jpg
$ git mv 822811.jpg 822811-uncropped.jpg
$ git checkout 439dc8c0 822811.jpg
$ mv 822811.jpg 822811-cropped.jpg
~~~
{: .bash}

Then, remove the old `822811.jpg` and add the two new files:
 
~~~
$ git rm 822811.jpg
$ git add 822811-uncropped.jpg
$ git add 822811-cropped.jpg
$ git commit -m "Use two images: cropped and un-cropped"
~~~
{: .bash}

~~~
[master 94ae08c] Use two images: cropped and un-cropped
2 files changed, 0 insertions(+), 0 deletions(-)
create mode 100644 822811-uncropped.jpg
rename 822811.jpg => 822811-cropped.jpg (100%)
~~~
{: .output}

Now both images are checked into the repository, and `822811.jpg` no longer exists.

