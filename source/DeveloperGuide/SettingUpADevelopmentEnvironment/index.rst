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