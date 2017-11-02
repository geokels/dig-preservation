---
layout: page
title: "Instructor Notes"
permalink: /guide/
---
## [Automated Version Control]({{ page.root }}/01-basics/)

Use slides

### Key Points
* Version control is like an unlimited "undo".
* Version control allows many people to work in parallel.

## [Setting Up Git]({{ page.root }}/02-setup/)

_Slide: Setting Up Git_

### Key Points
* "Use `git config` to configure a user name, email address, editor, and other preferences once per machine."
* "Use `git help` or `git help <command>` if you get stuck!"

## [Creating a Repository]({{ page.root }}/03-create/)
### Section Overview:
- "Where does Git store information?"
- "How do I record changes in Git?"
- "How do I check the status of my version control repository?"
- "How do I record notes about what changes I made and why?"

### Instructions:
- Change to `cats-as-data` directory
~~~
$ cd ~/Desktop/cats-as-data # or wherever you unzipped it
~~~
{: .bash}

- **New Command**: `git init` to initialize our git repository
~~~
$ git init
~~~
{: .bash}

- If we list the files, we can not see a hidden `.git` directory. Git stores all information about our project in here.
  If we ever delete it, we will lose our project history
~~~
$ ls -a # or dir /ah on windows
~~~
{: .bash}

- **New Command**: `git status` to check current state of the repository

- `git status` highlights:
1. Untracked files (git sees them)
2. No commits yet (our repository exists, but no commits)

- **New Command** `git add` to tell Git to track files
~~~
$ git add README.md
~~~
{: .bash}

- Let's check `git status` again
~~~
$ git status
~~~
{: .bash}

- `git status` highlights:
1. Git sees README.md as a `new file`
2. The `Changes to be committed` is because we haven't recorded this as a commit yet

- Now add cats metadata csv file
~~~
$ git add cats-human-situations.csv
~~~
{: .bash}

- **New Command**: `git commit` to commit the changes to our repository
~~~
$ git commit -m "add notes on metadata, and the metadata"
~~~
{: .bash}

- `git commit` highlights:
1. Git saves everything from the `git add` command in a commit
2. A commit is given an identifier that is unique, as it is a hash of the contents of the commit (explain)
3. The `-m` flag stands for message. used to record short description of the changes
4. If the `-m` flag is not specified, our chosen editor will pop up and we'll be asked to add the commit message there.

- Describe good commit messages

- **New Command**: `git log` to view our project history
~~~
$ git log
~~~
{: .bash}

- `git log` highlights:
1. Lists all commits to a repo in reverse chronological order
2. Each commit is listed with full identifier, author, created date, and commit message

- Run `ls` and notice that there is just one copy of `README.md`
- This is because Git saves all history in `.git` folder. So your working directory stays clean and up to date

- Add more information to `README.md` with your editor
~~~
The images are in the `images/` subdirectory of this repository.
~~~
{: .bash}

- Run `git status` - Notice we see it see the file it knows about is modified

- `no changes added to commit.` - We haven't done a `git add` for the file yet

- Always good practice to review changes before saving them

- **New Command** `git diff` to compare our changes with the most recent commit
~~~
$ git diff
~~~
{: .bash}

- `git diff` highlights:
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

- We're comfortable with our changes, so let's commit:
~~~
$ git add README.md
$ git commit -m "readme: where are the files?"
~~~
{: .bash}

- Terminology: Diff vs Patch vs Commit (slide?)

- Add `images` folder to repository and show diff
~~~
$ git add images
$ git diff
~~~
{: .bash}

- Note no output, so introduce `git --staged`
~~~
$ git diff --staged
~~~
{: .bash}

- Commit `images`
~~~
$ git commit -m "add pictures of cats"
~~~
{: .bash}

- Review `git log`

## [Exploring History]({{ page.root }}/04-history/)
### Section Overview:
- "How can I identify old versions of files"
- "How do I review changes?"
- "How can I recover old versions of files"

### Instructions:

- Introduce `HEAD` (pointer to most recent commit)

- Open `README.md` and add a new line and save:
~~~
If you would like, you can also put images of dogs and hexagonal software logos here.
~~~
{: .bash}

- Introduce tilda `~` syntax for showing older commits
~~~
$ git diff HEAD~1 README.md
$ git diff HEAD~2 README.md
$ git diff HEAD~2
~~~
{: .bash}

- **New Command**: `git show` to see changes made in an older commit with all information (by who, when, etc.)
~~~
$ git show HEAD~2 README.md
~~~
{: .bash}

- Tilda syntax works great for going back a few commits. Beyond that you can use the commit identifiers. We only need ot
  use the first few characters
~~~
$ git log #(find oldest commit hash and take first 5)
$ git diff <first 5 characters> README.md
~~~
{: .bash}

- Now we're going to talk about restoring an older version. Open editor and replace the contents of `README.md` with
  anything. Junk text.

- Run `git status` (notice it tells us how to revert)
- **New Command**: `git checkout` to bring back previous version of `README.md`
~~~
$ git checkout HEAD README.md
~~~
{: .bash}

- Notice our original changes are gone. We have the most recently commited version returned.

### Key Points
* `git diff` displays differences between commits.
* `git checkout` recovers old versions of files.

## [Ignoring Things]({{ page.root }}/05-ignore/)
### Section Overview:
- "How can I tell Git to ignore files I don't want to track?"

### Instructions:
- Provide context. Things we don't want to track. We have such a file in `cats-as-data`. Highlight Git sees the file
  now.
~~~
$ git status
~~~
{: .bash}

- We want to ignore `cats-human-situations.md` because it's a derivative

- Let's open `.gitignore` and add the filename
~~~
cats-human-situations.md
~~~
{: .bash}

- Run `git status` again. Notice Git no longer lists the file.
~~~
$ git status
~~~
{: .bash}

- We do want to track the `.gitignore` file, so others have the same experience/behavior.
~~~
$ git add .gitignore
$ git commit -m "Add the ignore file"
$ git status
~~~
{: .bash}

### Key Points
* The `.gitignore` file tells Git what files to ignore
* There are other patterns you can use to ignore files and folders.

## [Working with Branches]({{ page.root }}/06-branching/)
### Section Overview:
- "What are branches and why should I use them?"
- "How do I merge one branch into another?"

### Instructions:

_Slide: Working with Branches_

- **New Command**: `git branch` to look at current branches. Asterisk by branch name tells you what branch you're on.
~~~
$ git branch
~~~
{: .bash}

- Create a branch to work on cleanup. Fix titles in all caps
~~~
$ git branch title-caps
~~~
{: .bash}

- Look at branches again
~~~
$ git branch
~~~
{: .bash}

- Still on the `master` branch. You can also check this with `git status`

- If you need to rename a branch, you can use the `-m` move flag.
~~~
$ git branch -m title-cads title-caps
~~~
{: .bash}

- Let's switch to our new branch with `git checkout`
~~~
$ git checkout title-caps
~~~
{: .bash}

- Tip: This can be done in one go with the `-b` flag
~~~
$ git checkout -b title-caps
~~~
{: .bash}

- Open editor and fix the all caps title, save, close, commit:
~~~
$ git add cats-human-situations.md
$ git commit -m "fix all caps in titles"
~~~
{: .bash}

- Let's look at the log with `--oneline` flag
~~~
$ git log --oneline
~~~
{: .bash}

- Switch back to `master` and look at the log. New commit not on `master`
~~~
$ git checkout master
$ git log --oneline
~~~
{: .bash}

- We realize we want to add a clean up plan to the `README.md` file. Open editor and add the following:
~~~
Cleanup plan:
- Get rid of all caps in titles
- Standardize dates
- Standardize dimensions
~~~
{: .bash}

- Save file, add, and commit changes:
~~~
$ git add README.md
$ git commit -m "add cleanup plan"
~~~
{: .bash}

- Let's view a graph version of the log
~~~
$ git log --graph --oneline --decorate --all
~~~
{: .bash}

### Merging Branches
- Intro and context (bringing changes back into `master`)

- Sometimes branches live permanently

- Title edits exist in `title-caps`, not in `master`. Need to merge `title-caps` into `master`. Checkout `master`.
~~~
$ git checkout master
~~~
{: .bash}

- **New Command**: `git merge` to bring commits into `master`.
~~~
$ git merge title-caps
~~~
{: .bash}

- If editor window pops up, it's OK! We need to provide a merge commit message

- Look at recent commits on master
~~~
$ git log --oneline
~~~
{: .bash}

- We can now safely delete the `title-caps` branch. To verify changes were merged, we can run `branch` with `--merged`
  flag from `master`
~~~
$ git checkout master
$ git branch --merged
$ git branch -d title-caps
~~~
{: .bash}

### Key Points
* "`git branch` to view current branches"
* "`git branch branch-name` to create a new branch"
* "`git checkout branch-name` to switch to a branch"
* "`git branch -m branch-name` to rename a branch"
* "`git merge branch-name` to merge a branch"
* "`git branch -d delete-me` to delete a branch"
* "Checking out a branch changes the “active” files in your repository directory."
* "Merging applies commits from one branch to another."
* "Merge commits are used to glue two histories together."

## [Remotes]({{ page.root }}/07-remotes/)
### Section Overview:
- "How do I share my changes with others on the web?"

### Instructions:
- Log in to Github

- Create repository

- Copy `https` url of the repo

- Add the repository as the `origin` remote. `origin` is a convention.
~~~
$ git remote add origin https://github.com/your-username/cats-as-data.git
$ git remote -v
~~~
{: .bash}

- **New Command**: `git push` to push our local repository content to our Github repository. Remote version of `commit`
~~~
$ git push origin master
~~~
{: .bash}

- **New Command**: `git pull` to pull changes from our Github repository to our local repository. Pulling now has no
  effect.
~~~
$ git pull origin master
~~~
{: .bash}

### Key Points
* "A local Git repository can be connected to one or more remote repositories."
* "Use the HTTPS protocol to connect to remote repositories until you have learned how to set up SSH."
* "`git push` copies changes from a local repository to a remote repository."
* "`git pull` copies changes from a remote repository to a local repository."

## [Collaborating]({{ page.root }}/08-collaborating/)
### Section Overview:
- "How can I use version control to collaborate with other people?"

### Instructions:
- _Slide: Collaborating_

- Pair up. Though this can be done solo as well!

- Owner and Collaborator roles.

- Owner: give Collaborator access to Github repo (Settings -> Collaborators

- Collaborator: **New Command**: `git clone` to get local copy of the owner's repository to make changes. Replace
  `ccline` with owner's username.
~~~
$ git clone https://github.com/ccline/cats-as-data.git ~/Desktop/ccline-cats-as-data
~~~
{: .bash}

- Check in! And view diagram of repositories

- Collaborator: Open project `README.md` file. Choose a part of the cleanup plan to work on.

- Collaborator: create branch and make changes
~~~
$ git checkout -b standardize-dates
$ git add cats-human-situations.csv
$ git commit -m "Standardize all dates"
~~~
{: .bash}

- Check in!

- Let's `push` our changes to the Owner's repo:
~~~
$ git push origin standardize-dates
~~~
{: .bash}

- Collaborator: Create Pull Request (demo)

- Check in!

- Owner: Review Pull Request and Merge

- Owner: get changes locally and view `csv`
~~~
$ git pull origin master
$ git log
~~~
{: .bash}

- Review basic workflow

### Key Points
* "`git clone` copies a remote repository with a remote called `origin` automatically setup."
* "We can collaborate with others on Github using Pull Requests, and the branching workflow we already learned."

## [Conflicts]({{ page.root }}/10-conflicts/)
### Section Overview:
- "What do I do when my changes conflict with someone else's?"

### Instructions:
- Introduction/Context

- Create new branch to work on:
~~~
$ git checkout master
$ git branch update-plan
~~~
{: .bash}

- Add line to our cleanup plan in the `README.md`(in `master`):
~~~
- Reconcile subjects
~~~
{: .bash}

- Save file, add and commit
~~~
$ git add README.md
$ git commit -m "add subject reconciliation to plan"
~~~
{: .bash}

- Switch to `update-plan` branch
~~~
$ git checkout update-plan
~~~
{: .bash}

- Add line to our cleanup plan in the `README.md`(in `update-plan`):
~~~
- Add access permissions
~~~
{: .bash}

- Now let's switch back to `master` and try to merge `update-plan`
~~~
$ git checkout master
$ git merge update-plan
~~~
{: .bash}

- Review Merge image

- Open `README.md` to review merge conflicts to resolve

- Update `README.md` to include both changes

- Add `README.md`, check `git status`, and `commit`
~~~
$ git add README.md
$ git status
$ git commit -m "Merge changes from update-plan branch"
~~~
{: .bash}

### Key Points
* "Conflicts occur when two or more people change the same file(s) at the same time."
* "The version control system does not allow people to overwrite each other's changes blindly, but highlights conflicts
  so that they can be resolved."
