=====================
Creating a release
=====================

------------
Motivation
------------

DIRAC is used by several virtual organizations. Some of them are creating their own extensions for DIRAC. These extensions require a certain version of DIRAC in order to function properly. Virtual organizations have to be able to create their own releases of their extensions and install them seamlessly with dirac-install.

-------------------
Releases schema
-------------------

Each extension has a releases.ini where the releases and dependencies are defined. A single releases.ini can take care of one or more extensions. releases.ini file follows the Windows INI format. It can have several sections but not nested sections. Each section in the file defines a release. The name of the section will be the release name. Each release will contain a list of dependencies (if any) and a list of extensions (if more than one). An example of a release.ini for a single extension is shown below::
 
 defaultExtensions = MyExt
 
 [v1r2p3]
 depends = DIRAC:v5r12p.*
 
 [v1r2p2]
 depends = DIRAC:v5r12p1

The *defaultExtensions* option (outside any release section) defines what will be installed by default if there's nothing explicitly specified at installation time. Because there is only one extension defined in *defaultExtensions* each release will try to install the extension *MyExt* with the same version as the release name. Each release can require a certain version of any other extension or DIRAC. This requirement accepts any python regular expression. 

An example with more than one extension follows::

 defaultExtensions = MyExt
 
 [v1r2p3]
 extensions = MyExt:v1r2p1, MySecondExt:v1r1p1
 depends = DIRAC:v5r12p.*
 
 [v1r2p2]
 extensions = MyExt:v1r2p1, MySecondExt:v1r1
 depends = DIRAC:v5r12p1

The extensions option can define explicitly which extensions (and their version) to install. This is useful if a given VO is managing more than one extension. In that scenario a release can be a combination of extensions that can evolve independently. By defining releases as groups of extensions with their versions the VO can ensure that a release is consistent for its extensions. DIRAC uses this mechanism to ensure that the DIRAC Web will always be installed with a DIRAC version that it works with.

In DIRAC lingo a releases.ini is a project. When installing, a project name can be given. If it is given dirac-install will try to install that project instead of the DIRAC project. dirac-install will have a mapping to discover where to find the releases.ini based on the project name. Any VO can modify dirac-install to directly include their repositories inside dirac-install in their code repositories, and use their modified version. DIRAC will also have a mapping in the DIRAC repository. Any VO can also notify the DIRAC developers to update the mapping in the DIRAC repository.

If a project is given, all extensions inside that releases.ini have to start with the same name as the project. For instance, if dirac-install is going to install project LHCb, all extensions inside LHCb's releases.cfg have to start with LHCb. 
 
--------------------------------
Installation
--------------------------------

When installing dirac-install requires a release version and a project name. If the project name is given dirac-install will try to load the project's releases.ini instead of the default DIRAC's releases.ini. The releases.ini will specify which extension and version to install. All extensions that are defined inside a releases.ini will be downloaded from the same parent URL. For instance, if the releases.cfg is in ``http://diracgrid.org/releases/releases.ini`` and DIRAC v5r14 has to be installed, dirac-install will try to download it from ``http://diracgrid.org/releases/DIRAC-v5r14.tar.gz``.

If nothing else is defined, dirac-install will only install the extensions defined in *defaultExtensions* option. To install other extensions that are defined in the releases.ini the *-e* flag has to be used. 

Once all the extensions defined in the releases.ini are installed. dirac-install will try to load the dependencies. The *depends* option defines on which projects the installed project depends on. That will trigger loading that releases.ini and process it as the main one was processed. dirac-install will try to resolve recursively all the dependencies either until all the required extensions are installed or until there's a mismatch in the requirements. If after resolving all the releases.ini an extension is required to be installed with more than one version, an error will be raised and the installation stopped.

-----------------------------
How to make a distribution
-----------------------------

Still to do :P Whenever I finish the distribution scripts I'll finish this part. 

----------------------------------
Reference of releases.ini options
----------------------------------

Outside any section
----------------------

defaultExtensions
	List of extensions to be installed by default for the project

Sections
------------

Each section inside a releases.ini defines a release. The section name is the release version.

extensions : (Optional)
	Contains a comma separated list of extensions for this release and their version in format *extName(:extVersion)? (, extName(:extVersion)?)** . If this option is not defined, extensions defined in *defaultExtensions* will be installed with the same version as the release.

depends : (Optional) 
	Comma separated list of projects on which this project depends in format *projectName(:projectVersion)? (, projectName(:projectVersion)?)**. Defining this option triggers installation on the depended project. This is useful to install the proper version of DIRAC on which a set of extensions depend.

Common pitfalls
------------------

Installation will find a given releases.ini by looking up the project name. All extensions defined inside a releases.ini have to start with the same name as the project. For instance, if the project is *MyVO*, all extensions inside have to start with *MyVO*. *MyVOWeb*, *MyVOSomething* and MyVO are all valid extension names inside a *MyVO* releases.ini