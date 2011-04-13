=====================
Creating a release
=====================

------------
Motivation
------------

DIRAC is used by several virtual organizations. Some of them are creating their own modules for DIRAC. These modules require a certain version of DIRAC in order to function properly. Virtual organizations have to be able to create their own releases of their modules and install them seamlessly with *dirac-install*.

-------------------
Releases schema
-------------------

DIRAC modules are released and distributed in installation projects. Each installation project has a *releases.cfg* where the releases, modules and dependencies are defined. A single *releases.cfg* can take care of one or more modules. *releases.cfg* file follows a simplified schema of DIRAC's cfg format. It can have several sections, nested sections and options. Section *Releases* contains the releases definition. Each section in the *Releases* section defines a release. The name of the section will be the release name. Each release will contain a list of dependencies (if any) and a list of modules (if more than one). An example of a *release.cfg* for a single module is shown below::
 
 DefaultModules = MyExt
 
 Sources
 {
   MyExt = git://somerepohosting/MyExt.git
 }
 
 Releases
 {
   v1r2p3
   {
     depends = DIRAC:v5r12
   }
 
   v1r2p2
   {
     depends = DIRAC:v5r12p1
   }
 }

The *DefaultModules* option (outside any section) defines what modules will be installed by default if there's nothing explicitly specified at installation time. Because there is only one module defined in *DefaultModules* each release will try to install the *MyExt* module with the same version as the release name. Each release can require a certain version of any other installation project (DIRAC is also an installation project). 

An example with more than one module follows::

 DefaultModules = MyExt
 
 Sources
 {
   MyExt = git://somerepohosting/MyExt.git
   MyExtExtra = svn | http://someotherrepohosting/repos/randomname/MyExtExtra/tags
 }
 
 Releases
 {
   v1r2p3
   {
     Modules = MyExt:v1r2p1, MyExtExtra:v1r1p1
     Depends = DIRAC:v5r12p1
   }
 
   v1r2p2
   {
     Modules = MyExt:v1r2p1, MyExtExtra:v1r1
     Depends = DIRAC:v5r12
   }
 }
 
The *Modules* option can define explicitly which modules (and their version) to install. This is useful if a given VO is managing more than one module. In that scenario a release can be a combination of modules that can evolve independently. By defining releases as groups of modules with their versions the VO can ensure that a release is consistent for its modules. DIRAC uses this mechanism to ensure that the DIRAC Web will always be installed with a DIRAC version that it works with.

The *Sources* section defines where to extract the source code from for each module. *dirac-distribution* will assume that there's a tag in that source origin with the same name as the version of the module to be released. *dirac-distribution* knows how to handle several types of VCS. The ones supported are:

file
 A directory in the filesystem. *dirac-distribution* will assume that the directory especified contains the required module version of the module.
 
cvs
 The cvs root where to find the code. *dirac-distribution* will assume there's a tag with the same name name as the module version to be tagged.
 
svn
 A subversion url that contains a directory with the same name as the version to be tagged. If the module version is v1r0 and the url is http://host/extName, *dirac-distribution* will check out http://host/extName/v1r0 and assume it contains the module contents.
 
hg
 A mercurial repository. *dirac-distribution* will check out the a tag with the same name as the module version and assume it contains the module contents.
 
git
 A git repository. *dirac-distribution* will clone the repository and check out to a tag with the same name as the module version and assume it contains the module contents.
 
Some of the VCS URLs may not explicitly define which VCS has to be used (for instance http://... it can be a subversion or mercurial repository). In that case the option value can take the form ``<vcsName> | <vcsURL>``. In that case *dirac-distribution* will use that VCS to check out the source code.

When installing, a project name can be given. If it is given *dirac-install* will try to install that project instead of the DIRAC project. *dirac-install* will have a mapping to discover where to find the *releases.cfg* based on the project name. Any VO can modify *dirac-install* to directly include their repositories inside *dirac-install* in their module source code, and use their modified version. DIRAC developers will also maintain a project name to *releases.cfg* location mapping in the DIRAC repository. Any VO can also notify the DIRAC developers to update the mapping in the DIRAC repository so *dirac-install* will automatically find the project's *releases.cfg* without any change to *dirac-install*.

If a project is given, all modules inside that *releases.cfg* have to start with the same name as the project. For instance, if *dirac-install* is going to install project LHCb, all modules inside LHCb's *releases.cfg* have to start with LHCb. 

*dirac-distribution* will generate a set of tarballs, *md5* files and a ``release-<projectName>-<version>.cfg``. Once generated, they have to be upload to the install project source of tarballs where *dirac-install* will try to pick them up.
 

How to make a distribution
-----------------------------

Just execute *dirac-distribution* with the appropiate flags. For instance::

 dirac-distribution -r v6r0 -l DIRAC 
 
You can also pass the releases.cfg to use via command line using the *-C* switch. *dirac-distribution* will generate a set of tarballs, release and md5 files. Please copy those to your installation source so *dirac-install* can find them. 

--------------------------------
Installation
--------------------------------

When installing, *dirac-install* requires a release version and optionally a project name. If the project name is given *dirac-install* will try to load the project's versioned ``release-<projectName>-<version>.cfg`` instead of the DIRAC's one (this file is generated by *dirac-distribution* when generating the release). *dirac-install* has several mechanisms on how to find the URL where the released tarballs and releases files for each project are. *dirac-install* will try the following steps:

1. Load DIRAC's default global locations. This file contains the default values and paths for each project that DIRAC knows of and it's maintained by DIRAC developers.
2. Load the required project's *default.cfg*. DIRAC's default global locations has defined where this file is for each project. It can be in a URL that is maintained by the project's developers/maintainers.
3. If an option called *InstallBaseURL* is defined in the project's *default.cfg* then use that as the base URL to download the releases and tarballs files for the projects.
4. If an option called *<projectName>/InstallBaseURL* is defined in DIRAC's default global locations, use it.
5. If it's defined inside *dirac-install*, use it.
6. If not found then the installation is aborted.

The ``release-<projectName>-<version>.cfg`` file will specify which module and version to install. All modules that are defined inside a ``release-<projectName>-<version>.cfg`` will be downloaded from the same parent URL. For instance, if the ``release-<projectName>-<version>.cfg``  is in ``http://diracgrid.org/releases/releases.cfg`` and DIRAC v5r14 has to be installed, *dirac-install* will try to download it from ``http://diracgrid.org/releases/DIRAC-v5r14.tar.gz``.

If nothing else is defined, *dirac-install* will only install the modules defined in *DefaultModules* option. To install other modules that are defined in the ``release-<projectName>-<version>.cfg`` the *-e* flag has to be used. 

Once all the modules defined in the ``release-<projectName>-<version>.cfg``  are installed. *dirac-install* will try to load the dependencies. The *depends* option defines on which projects the installed project depends on. That will trigger loading that ``release-<projectName>-<version>.cfg``  and process it as the main one was processed. *dirac-install* will try to resolve recursively all the dependencies either until all the required modules are installed or until there's a mismatch in the requirements. If after resolving all the ``release-<projectName>-<version>.cfg``  an module is required to be installed with more than one version, an error will be raised and the installation stopped.


-----------------------------------
Reference of *releases.cfg*  schema
-----------------------------------

::

 #List of modules to be installed by default for the projec
 DefaultModules = MyExt
 
 #Section containing where to find the source code to generate releases
 Sources
 {
   #Source URL for module MyExt
   MyExt = git://somerepohosting/MyExt.git
   MyExtExtra = svn | http://someotherrepohosting/repos/randomname/MyExtExtra/tags
 }
 
 #Section containing the list of releases
 Releases
 {
   #Release v1r2p3
   v1r2p3
   {
     #(Optional) Contains a comma separated list of modules for this release and their version in format
     # *extName(:extVersion)? (, extName(:extVersion)?)** . 
     #If this option is not defined, modules defined in *DefaultExtensions* will be installed with the same version as the release.
     Modules = MyExt:v1r2p1, MyExtExtra:v1r1p1
     
     #(Optional) Comma separated list of projects on which this project depends in format 
     # *projectName(:projectVersion)? (, projectName(:projectVersion)?)**. 
     #Defining this option triggers installation on the depended project. 
     #This is useful to install the proper version of DIRAC on which a set of modules depend.
     Depends = DIRAC:v5r12p1
   }
 
   v1r2p2
   {
     Modules = MyExt:v1r2p1, MyExtExtra:v1r1
   }
 }
 
-----------------------------------
Reference of *default.cfg*  schema
-----------------------------------

::

 #Where to download the release tarballs and definitions
 InstallBaseURL = http://myhost/somepath/
 
 #(Everything in here is optional) Default values for dirac-install
 Defaults
 {
   #Release to install if not defined via command line
   Release = v1r4
   #Modules to install by default
   ModulesToInstall = MyExt
   #Type of externals to install (client, client-full, server)
   ExternalsType = client
   #Python version to install (25/26)
   PythonVersion = 26
   #Version of lcg bundle to install
   LcgVer = 2010-11-20
   #Install following DIRAC's pro/versions schema
   UseVersionDir = False
   #Force building externals
   BuildExternals = False
   #Build externals if the required externals is not available
   BuildIfNotAvailable = False
   #Enable debug logging
   Debug = False
 }


Common pitfalls
------------------

Installation will find a given *releases.cfg*  by looking up the project name. All modules defined inside a *releases.cfg*  have to start with the same name as the project. For instance, if the project is *MyVO*, all modules inside have to start with *MyVO*. *MyVOWeb*, *MyVOSomething* and MyVO are all valid module names inside a *MyVO* *releases.cfg* 
