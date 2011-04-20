====================================
Setting up a development environment
====================================

-----------------------------
Before jumping into the code
-----------------------------

DIRAC source code is maintained in GIT. It is really recommended to be familiar with how to work with GIT. So before jumping into the code it is highly recommended to go though a GIT tutorial. The recommended ones are:

 - For an GIT shallow overview the GIT community book at http://book.git-scm.com/ . Do not be alarmed by the word *book*. It's more a tutorial on the basics of GIT. 
 - For really knowing what's going on read http://progit.org/book/ . It'll make using GIT a painless and nice experience.
 
Once you're familiar with how GIT works. You're ready to clone the DIRAC source repository. DIRAC is release repository is hosted at https://github.com/DIRACGrid/DIRAC . From there you have two options:

 - Easy way: 
  1. Register at *github.com* and set up your account.
  2. On *github.com* fork the DIRAC repository by going to https://github.com/DIRACGrid/DIRAC and clicking the *Fork* button on the top right part of the page.
  3. By forking a repository *github.com* will create a https://github.com/yourusername/DIRAC repository where you are the administrator.
  4. Clone that repository in your work space
  5. Start hacking DIRAC
  6. Push changes to your *github.com* repository
  7. Issue a pull request to DIRAC by going to https://github.com/yourusername/DIRAC, switching to the branch you want DIRAC to pull changes from and clicking the pull request button. Please issue pull requests from new feature or fix branches. Do not issue pull requests from your master branch.
  
 - *Do-it-yourself* way:
  1. Clone DIRAC repository from https://github.com/DIRACGrid/DIRAC to a **bare** repository where you have write access and the rest of the world has read access. For naming purposes we'll call this repository your **public** repository. Public access can be via http or git. Remote access can be via ssh. For instance you can create this repository somewhere everyone has http access and you can access via ssh.
  2. Clone your bare repository to your work space. We'll call this repository your **private** repository.
  3. Start hacking DIRAC
  4. Push your changes to your **public** repository
  5. Send a pull request via mail stating from which URL branch and revision should the pull be done.
  
 
The first approach requires registering on *github.com* but it is far easier to set up than the second approach. We recommend the first one.

--------------------------------------
Setting up your development workspace
--------------------------------------

DIRAC developers tend to use eclipse for developing DIRAC. It is not mandatory but it is recommended. The following steps will try to guide you on setting up a development workspace for DIRAC in eclipse. If you don't need/want eclipse just follow the next section and skip the rest.

Checking out the source
=========================

First you need to check out all the sources you need to start working on DIRAC or on any extension. So go to a clean directory (from now on I'll call that directory *devRoot*) and:
 
 1. Go to your *devRoot* directory
 2. Check out DIRAC source code. DIRAC source is hosted on *github.com*. But depending on which option you went for in the previous *Before jumping into code* section you need to proceed differenty:
  - If you went for the *easy way* just clone your DIRAC repo (remember you are in a clean directory where you will set up the development environment) ``git clone git@github.com/yourusername/DIRAC.git`` will create a *devRoot/DIRAC* for you.
  - If you want for the *Do-it-yourself* way, clone your public repo with ``git clone yourgitpublicrepourl DIRAC``
  - If you don't intend to develop DIRAC and just need it for developing extensions do ``git clone https://github.com/DIRACGrid/DIRAC.git``
 3. If you need DIRACWeb do the same with the repo at https://github.com/DIRACGrid/DIRACWeb
 4. If you need to check out any extension do so in the *devRoot* directory. For instance ``svn co svn+ssh://svn.cern.ch/reps/lbdirac/LHCbDIRAC/trunk/LHCbDIRAC LHCbDIRAC``
 5. Repeat step 4 for each extension you need
 6. Deploy DIRAC scripts by running ``DIRAC/Core/scripts/dirac-deploy-scripts.py``
 7. Compile the DIRAC Externals by running ``scripts/dirac-compile-externals -t server -j 2``. This may take a while.
 8. Configure DIRAC by executing ``scripts/dirac-configure -S setupyouwanttorun -C configurationserverslist -n sitename -H``
 
You're ready to hack DIRAC!

Configuring Eclipse
=====================

A couple extensions are required to be able to use eclipse for developing DIRAC. To install them go to *Help->Install new software->top right button "Add..." -> Insert name and URL* and then select the software to install in the list.

 - *pyDev* : Use http://pydev.org/updates as the URL to install from. For more info go to http://pydev.org/updates
 - *EGit* : Git team provider for eclipse. Use http://download.eclipse.org/egit/updates as the URL. For more info go to http://www.eclipse.org/egit/
 
Now you need to configure the pyDev plugin. Go to *Window->Preferences* (*Eclipse->preferences if you're in a MacOSX box). In the preferences pane go to *Pydev->Editor*, select 2 as the tab length and click "Replace tabs with spaces when typing". In *Pydev->Editor->Code Style->Code formatter* check all the boxes. 
 
For Egit you simply need to configure your name and mail. Go to the preferences pane and then go to *Team->Git->Configuration* and add two entries: *user.name* with your name and *user.email* with your email.

That's it! Eclipse is configured now :)


Creating a development workspace in Eclipse
=============================================

All that remains is to import these directories as projects in Eclipse. To import DIRAC:

 1. File -> Import...
 2. Git -> Projects from Git and click *Next*.
 3. In the "Import Projects from Git" click *Add*.
 4. In the "Add Git Repositories", click *Browse* and select the DIRAC source code folder you cloned into before. Then click *Search* and the *.git* directory in the DIRAC source code directory should appear. Select it and click *OK*.
 6. In the "Import Projects from Git" pane the DIRAC folder should now appear. Select it and click *Next*.
 7. Select "Import as General Project" and click *Next*.
 8. Write the name of the project and "Finish".
 
If you want to add DIRACWeb to eclipse repeat the same steps with the Web source directory. For additional extensions, add them as projects to Eclipse. You'll have to look on how to do it depending on your team provider.

That's it! You have a nice development workspace set up :)
 
