====================================
DIRAC branching model
====================================

DIRAC uses Git to manage it's source code. Git is a distributed version control system (DVCS). That means that there's no central repository like the one CVS/Subversion use. Each developer has a copy of the whole repository. Because there are lots of repositories, code changes travel across different repositories all the time by merging changes from different branches and repositories. In any centralised VCS branching/merging is an advanced topic. In Git branching and merging are daily operations. That allows to manage the code in a much more easy and efficient way. This document is heavily inspired on `A successful Git branching model <http://nvie.com/posts/a-successful-git-branching-model/>`_


Why Git?
==========

DIRAC started using CVS. It was a pain. Directories could not be removed, each file had a version number, it was slow... And migrated to Subversion. It definitely was an improvement compared to CVS. But Subversion has some important defficiencies. Tags and banchs do not exist as such, they are directories in a tree structure like a filesystem. That makes tagging and branching error prone. It's easy to merge from a branch that's not the one you intend to, because branches and tags don't have a name, they are a full path. Plus it is enervatingly slow.

We evaluated different alternatives to Subversion. Our requirements were:

 - Cheap and easy branch/merge
 - Fast
 - Well supported by a community

That reduced the possibilities to two different VCS:

 - Git (http://git-scm.com/)
 - Mercurial (http://mercurial.selenic.com/)
 
Both options are distributed VCS. Seems that by being distributed they are forced to have a powerful branching/merging mechanism. Git is more powerful than Mercurial, but Mercurial is a bit more user-friendly than Git. Both are great DVCS. 

In the end we decided to use Git. Although mercurial is more user-friendly, Git seems to have better branching mechanism and remote repository handling. 

How decentralization works
===========================

Git doesn't have a centralized repository like CVS or Subversion do. Each developer has it's own repository. That means that commits, branches, tags... everything is local. Almost all Git operations are blazingly fast. By definition only one person works with one repository directly. But people don't develop alone. Git as a set of operations to send and bring information to/from remote repositories. Users work with their local repositories and only communicate with remote repositories to publish their changes or to bring other developer's changes to their repository. In Git *lingo* sending changes to a repository is called *pull* and bringing changes is *push*.

Git *per-se* doesn't have a central repository but to make things easier we'll define a repository that will hold the releases and stable branches for DIRAC. Developers will bring changes from that repository to synchronize their code with the DIRAC releases. To send changes to be released, users will have to push their changes to a repository where the integration manager can pull the changes from, and send a *pull request*. A *pull request* is telling the release manager where to get the changes from to integrate them into the next DIRAC release.

.. figure:: integrationModel.png
    :align: left
    :alt: Schema on how changes flow between DIRAC and users
     
    How to publish and retrieve changes to DIRAC (via `Pro Git Book <http://progit.org/book/>`_)

Developers use the *developer private* repositories for their daily work. When they want something to be integrated, they publish the changes to their *developer public* repositories and send a *pull request* to the integration manager. The integration manager will pull the changes to his/her own repository, and publish them in the *blessed repository* where the rest of the developers can pull the new changes to their respective *developer private* repositories.


Decentralized but centralized
==============================

Although Git is a distributed VCS, it works best if developers use a single repository as the central "truth" repository. Note that this repository is *only considered* to be the central one. We will refer to this repository as *release* since all releases will be generated from this repository.

Each developer can only pull from the *release* repository. Developers can pull new release patches from the *release* repository into their *private repositories*, work on a new feature, bugfix.... and then push the changes to their *public* repository. Once there are new changes in their public repositories, they can issue a *pull request* so the changes can be included in central *release* repository.

----------------------------------------------
How to set up the *release* remote repository
----------------------------------------------

To set up the *release* repository as a remote repository do::

 $ git remote add release repourl
 $ git fetch release
 From repourl
  * [new branch]      integration -> release/integration
  * [new branch]      master     -> release/master

 
The main branches
====================

The central *release* repository holds many branches. But there are only two branches with an infinite life time:

 - master
 - integration
 
Branch *release/master* is considered to be the main branch where the code is **always** required to be in a *production-ready* state.

Branch *release/integration* holds all the changes that are ready to go to the next release. Release managers use this branch to accumulate changes and test the integration between different merges. When the source code in *releases/integration* reaches a stable point and is ready to be released, all of the changes should be merged into *release/master* and then tagged with a new release number.

Supporting branches
=====================

Next to the main branches, there can be other type of branches such as:

 - Feature branches
 - Release branches
 - Hotfix branches
 
Each of these branches has a specific purpose and should follow certain rules to ease managing them. They aren't special in any technical way. It's just the way they are used that categorizes them. They are plain git branches.

Developer's pull requests should always be requested from one of these branches. *Don't* issue a pull request from the developer's *master* branch. This way it's easier to know what's is going to be merged.

Release and hotfix branches will not be explained in detail. They are branches for helping release managers create a new release. Release branches are branches the release manager create from the *release/integration* branch before merging back into *release/master* to finish polish the details before actually making the release. Hotfix branches exist so if there's a hot fix required in any release. A branch can be created from a release tag, develop the fix and then merge the fix back to all the required places.

------------------
Feature branches
------------------

They can branch from *release/master* and will merge back to *release/integration*. Their name should start with *feature-\** and shouldn't be named *master* or *integration*. 

Feature branches are used to develop new features for a future release. A feature branch will exist as long as the feature is in development but will eventually be merged into *release/integration* or discarded in case the feature is no longer relevant. Feature branches tipically exist in the developer repositories not in the *release* repository.

Creating a feature branch
--------------------------

When starting work on a new feature, branch of from the *release/master branch*::
 
  $ git checkout -b feature-somename release/master
  Branch feature-somename set up to track remote branch master from release.
  Switched to a new branch 'feature-somename'

Merging back a feature into *integration*
-------------------------------------------

Only the release managers should do this. Once a feature is ready to be integrated and the developer issues a pull request on a feature branch, the release manager will integrate the changes into the *release/integration* branch. To do so::

  $ git checkout integration
  Switched to branch 'integration'
  $ git remote add pullrepo repourl
  $ git fetch pullrepo branchtopull
  From pullrepo
   * branch            branchtopull -> FETCH_HEAD
  $ git merge --no-ff pullrepo/branchtopull
  Updating ea1b82a..05e9557
  (Summary of changes)
  $ git push release integration
 
The --no-ff flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature. 

In the latter case, it is impossible to see from the Git history which of the commit objects together have implemented a feature—you would have to manually read all the log messages. Reverting a whole feature (i.e. a group of commits), is a true headache in the latter situation, whereas it is easily done if the --no-ff flag was used.










