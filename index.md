---
layout: lesson
root: .
---

Catsy Cline and Luke Skywhisker have been asked to reconcile and normalize their digital 
object metadata prior to migrating into Hyrax.  They want to be able to work on the project 
at the same time, but they have run into problems doing this in the past.  If they take 
turns, each one will spend a lot of time waiting for the other to finish, but if they work
on their own copies and share changes back and forth things may be lost, overwritten, or 
duplicated.

A colleague suggests using [version control]({{ page.root }}/reference/#version-control) to
manage their work. Version control is better than sharing files back and forth:

*   Nothing that is committed to version control is ever lost, unless
    you work really, really hard at it. Since all old versions of
    files are saved, it's always possible to go back in time to see
    exactly who changed what on a particular day, or what version of a
    program was used to generate a particular set of results.

*   As we have this record of who made what changes when, we know who to ask
    if we have questions later on, and, if needed, revert to a previous
    version, much like the "undo" feature in an editor.

*   When several people collaborate in the same project, it's possible to
    accidentally overlook or overwrite someone's changes. The version control
    system automatically notifies users whenever there's a conflict between one
    person's work and another's.

Teams are not the only ones to benefit from version control: individuals can also benefit 
immensely. Keeping a record of what was changed, when, and why is extremely useful for 
any person if they ever need to come back to the project later on (e.g., a year later,
when memory has faded). It can also be helpful when creating formal project documentation 
following the conclusion of a project, or when planning similar projects in the future.

Version control is the lab notebook of the digital world: it's what
professionals use to keep track of what they've done and to
collaborate with other people.  Every large software development
project relies on it, and most programmers use it for their small jobs
as well.  And it isn't just for software: books,
papers, documentation, metadata, and anything that changes over time or needs
to be shared can and should be stored in a version control system.

> ## Prerequisites
>
> In this lesson we use Git from the Unix Shell.
> Some previous experience with the shell is expected,
> *but isn't mandatory*.
{: .prereq}
