---
title: Cultivating a Culture of Digital Preservation Inside Academic Libraries and Out
teaching: 5
exercises: 0
questions:
- "What can I bring back to my institution?"
objectives:
- "Understand the current culture of digital preservation."
- "Understand the difference between digital collections and institutional repositories."
keypoints:
-   "Faculty are a key component of the users that academic libraries serve and have power to influence the University administration's decision to support library services."
---

### Kelsey George
#### University of Nevada, Las Vegas 
#### Cataloging and Metadata Strategies Librarian 
(as of February 2018)

When we use Git on a new computer for the first time,
we need to configure a few things. Below are a few examples
of configurations we will set as we get started with Git:

*   our name and email address,
*   to colorize our output,
*   what our preferred text editor is,
*   and that we want to use these settings globally (i.e. for every project)

On a command line, Git commands are written as `git verb`,
where `verb` is what we actually want to do. So here is how
Catsy sets up her new laptop:

~~~
$ git config --global user.name "Catsy Cline"
$ git config --global user.email "ccline@pawson.edu"
$ git config --global color.ui "auto"
~~~
{: .bash}

Please use your own name and email address instead of Catsy's. This
user name and email will be associated with your subsequent Git
activity, which means that any changes pushed to
[GitHub](http://github.com/), [BitBucket](http://bitbucket.org/),
[GitLab](http://gitlab.com/) or another Git host server in a later
lesson will include this information.

For these lessons, we will be interacting with
[GitHub](http://github.com/) and so the email address used should be
the same as the one used when setting up your GitHub account. If you
are concerned about privacy, please review [GitHub's instructions for
keeping your email address private][git-privacy].  If you elect to use
a private email address with GitHub, then use that same email address
for the `user.email` value, e.g. `username@users.noreply.github.com`
replacing `username` with your GitHub one. You can change the email
address later on by using the `git config` command again.

You might also want to configure Git to use your favorite text editor, following
this table:

| Editor             | Configuration command                            |
|:-------------------|:-------------------------------------------------|
| Atom | `$ git config --global core.editor "atom --wait"`|
| nano               | `$ git config --global core.editor "nano -w"`    |
| Text Wrangler (Mac)      | `$ git config --global core.editor "edit -w"`    |
| Sublime Text (Mac) | `$ git config --global core.editor "subl -n -w"` |
| Sublime Text (Win, 32-bit install) | `$ git config --global core.editor "'c:/program files (x86)/sublime text 3/sublime_text.exe' -w"` |
| Sublime Text (Win, 64-bit install) | `$ git config --global core.editor "'c:/program files/sublime text 3/sublime_text.exe' -w"` |
| Notepad++ (Win, 32-bit install)    | `$ git config --global core.editor "'c:/program files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`|
| Notepad++ (Win, 64-bit install)    | `$ git config --global core.editor "'c:/program files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`|
| Kate (Linux)       | `$ git config --global core.editor "kate"`       |
| Gedit (Linux)      | `$ git config --global core.editor "gedit --wait --new-window"`   |
| Scratch (Linux)       | `$ git config --global core.editor "scratch-text-editor"`  |
| emacs              | `$ git config --global core.editor "emacs"`   |
| vim                | `$ git config --global core.editor "vim"`   |

It is possible to reconfigure the text editor for Git whenever you want to change it.

> ## Exiting Vim
>
> Note that `vim` is the default editor for for many programs, if you
> haven't used `vim` before and wish to exit a session, type `Esc`
> then `:q!` and `Enter`.
{: .callout}

The four commands we just ran above only need to be run once: the flag
`--global` tells Git to use the settings for every project, in your
user account, on this computer.

You can check your settings at any time:

~~~
$ git config --list
~~~
{: .bash}

You can change your configuration as many times as you want: just use
the same commands to choose another editor or update your email
address.

> ## Line Endings
>
> Finally we need to configure how git will handle line endings in our files.
>
> If you are on a Windows machine, type in the following:
> ~~~
> $ git config --global core.autocrlf true
> ~~~
> {: .bash}
>
> Windows users, please note that you will likely receive the
> following warning when adding files to git. Don't worry about this!
>
> ~~~
> "warning: CRLF will be replaced by LF in file."
> ~~~
> {: .output}
>
> If you are on a Mac OS/Linux machine, type in the following:
> ~~~
> $ git config --global core.autocrlf input
> ~~~
> {: .bash}
{: .callout}

> ## More Windows-specific things you may wish to know
>
> Copying and pasting text to and from the terminal on Windows is done with
> right-clicking as opposed to the usual `ctrl-c`/`ctrl-v` combo.  Right-click
> highlighted text to copy it (if you’re in the Git bash shell you’ll have to
> specifically select “Copy”), then right-click and select “Paste” to paste.
{: .callout}

> ## Proxy
>
> In some networks you need to use a
> [proxy](https://en.wikipedia.org/wiki/Proxy_server). If this is the
> case, you may also need to tell Git about the proxy:
>
> ~~~
> $ git config --global http.proxy proxy-url
> $ git config --global https.proxy proxy-url
> ~~~
> {: .bash}
>
> To disable the proxy, use
>
> ~~~
> $ git config --global --unset http.proxy
> $ git config --global --unset https.proxy
> ~~~
> {: .bash}
{: .callout}

> ## Git Help and Manual
>
> Always remember that if you forget a `git` command, you can access
> the list of commands and the Git manual by using `help`:
>
> ~~~
> $ git help
> $ git help config
> ~~~
> {: .bash}
{: .callout}

[git-privacy]: https://help.github.com/articles/keeping-your-email-address-private/
