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
