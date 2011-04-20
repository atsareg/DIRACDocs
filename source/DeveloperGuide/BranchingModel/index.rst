====================================
DIRAC branching model
====================================

DIRAC uses Git to manage it's source code. Git is a distributed version control system (DVCS). That means that there's no central repository like the one CVS/Subversion use. Each developer has a copy of the whole repository. Because there are lots of repositories, code changes travel across different repositories all the time by merging changes from different branches and repositories. In any centralised VCS branching/merging is an advanced topic. In Git branching and merging are daily operations. That allows to manage the code in a much more easy and efficient way.

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
 
Both options are distributed VCS. Seems that by being distributed they are forced to have a powerful branching/merging mechanism. Git is more powerful than Mercurial but it is a bit user-friendly. Both are great DVCS. 

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




