===============================================
DIRAC Client Installation
===============================================


Installing DIRAC client
-------------------------

- Create a directory to install DIRAC and change to the new directory, by example::

        $ mkdir DIRAC5
        $ cd DIRAC5

- Download dirac-install script from::

        http://cern.ch/lhcbproject/dist/Dirac_project/dirac-install

   By example::

        $ wget -np http://cern.ch/lhcbproject/dist/Dirac_project/dirac-install
        --2010-10-19 10:55:33--  http://cern.ch/lhcbproject/dist/Dirac_project/dirac-install
        Resolving cern.ch... 137.138.144.169
        Connecting to cern.ch|137.138.144.169|:80... connected.
        HTTP request sent, awaiting response... 302 Found
        Location: http://lhcbproject.web.cern.ch/lhcbproject/dist/Dirac_project/dirac-install [following]
        --2010-10-19 10:55:33--  http://lhcbproject.web.cern.ch/lhcbproject/dist/Dirac_project/dirac-install
        Resolving lhcbproject.web.cern.ch... 137.138.142.195
        Connecting to lhcbproject.web.cern.ch|137.138.142.195|:80... connected.
        HTTP request sent, awaiting response... 200 OK
        Length: 22627 (22K) [text/plain]
        Saving to: `dirac-install'100%[===========================================================================================================================================================>] 22,627      --.-K/s   in 0.03s
        2010-10-19 10:55:33 (772 KB/s) - `dirac-install' saved [22627/22627]

- Add execution permission to dirac-install::

        $ chmod 700 dirac-install

- Run installation script::

        dirac-install -V <VO>

  By Example::

        $ ./dirac-install -V vo.formation.idgrilles.fr
        2010-10-19 08:57:12 UTC dirac-install [INFO]  Downloading CFG.py
        2010-10-19 08:57:12 UTC dirac-install [INFO]  Getting defaults from http://lhcbproject.web.cern.ch/lhcbproject/dist/DIRAC3/vo.formation.idgrilles.fr_defaults.cfg
        2010-10-19 08:57:12 UTC dirac-install [INFO]  Getting defaults from http://lhcbproject.web.cern.ch/lhcbproject/dist/DIRAC3/defaults.cfg
        2010-10-19 08:57:12 UTC dirac-install [INFO]  LCG bindings version 2009-08-13 is requested
        2010-10-19 08:57:12 UTC dirac-install [INFO]  Installing package DIRAC version v5r10-pre2
        2010-10-19 08:57:15 UTC dirac-deploy-scripts [INFO]  Scripts will be deployed at /afs/in2p3.fr/home/h/hamar/DIRAC5/scripts
        2010-10-19 08:57:15 UTC dirac-deploy-scripts [INFO]  Inspecting DIRAC module
        2010-10-19 08:57:15 UTC dirac-install [INFO]  Using platform: Linux_x86_64_glibc-2.5
        2010-10-19 08:58:08 UTC dirac-install [INFO]  Creating /afs/in2p3.fr/home/h/hamar/DIRAC5/bashrc
        2010-10-19 08:58:08 UTC dirac-install [INFO]  DIRAC release v5r10-pre2 successfully installed


Configuring DIRAC Client
----------------------------

- Load DIRAC environment::

        $source bashrc

- Configure DIRAC client::

        dirac-configure <CFG>

  For vo.formation.idgrilles.fr::

        $ dirac-configure vo.formation.idgrilles.fr_defaults.cfg
        2010-10-19 09:34:47 UTC Framework ERROR: Can't load /afs/in2p3.fr/home/h/hamar/DIRAC5/etc/dirac.cfg file
        2010-10-19 09:34:47 UTC Framework  WARN: Short option - has been already defined
        2010-10-19 09:34:47 UTC Framework  WARN: Short option - has been already defined
        2010-10-19 09:34:47 UTC Framework  WARN: Short option - has been already defined
        2010-10-19 09:34:47 UTC Framework  WARN: Short option - has been already defined
        2010-10-19 09:34:47 UTC Framework  WARN: Short option - has been already defined
        2010-10-19 09:34:47 UTC dirac-configure.py  INFO: Executing: /afs/in2p3.fr/home/h/hamar/DIRAC5/DIRAC/Core/scripts/dirac-configure.py vo.formation.idgrilles.fr_defaults.cfg
        2010-10-19 09:34:47 UTC dirac-configure.py  INFO: Checking DIRAC installation at "/afs/in2p3.fr/home/h/hamar/DIRAC5"
        2010-10-19 09:34:48 UTC dirac-configure.py/BundleDelivery  INFO: Current hash for bundle CAs in dir /afs/in2p3.fr/home/h/hamar/DIRAC5/etc/grid-security/certificates is ''
        2010-10-19 09:34:48 UTC dirac-configure.py/BundleDelivery  INFO: Synchronizing dir with remote bundle
        2010-10-19 09:34:49 UTC dirac-configure.py/BundleDelivery  INFO: Dir has been synchronized
        2010-10-19 09:34:49 UTC dirac-configure.py/BundleDelivery  INFO: Current hash for bundle CRLs in dir /afs/in2p3.fr/home/h/hamar/DIRAC5/etc/grid-security/certificates is ''
        2010-10-19 09:34:52 UTC dirac-configure.py/BundleDelivery  INFO: Synchronizing dir with remote bundle
        2010-10-19 09:34:53 UTC dirac-configure.py/BundleDelivery  INFO: Dir has been synchronized
        2010-10-19 09:34:53 UTC dirac-configure.py  INFO: Creating VOMSDIR/VOMSES files
        2010-10-19 09:34:53 UTC dirac-configure.py  INFO: Created vomsdir file /afs/in2p3.fr/home/h/hamar/DIRAC5/etc/grid-security/vomsdir/vo.formation.idgrilles.fr/cclcgvomsli01.in2p3.fr.lsc
        2010-10-19 09:34:53 UTC dirac-configure.py  INFO: Created vomses file /afs/in2p3.fr/home/h/hamar/DIRAC5/etc/grid-security/vomses/vo.formation.idgrilles.fr


- After run configuration, configuration file must look like the example::

        $ more etc/dirac.cfg
        LcgVer = 2009-08-13
        DIRAC
        {
          Setup = Dirac-Production
          Configuration
          {
            Servers = dips://dirac.in2p3.fr:9135/Configuration/Server
          }
          Security
          {
            UseServerCertificate = no
          }
        }
        LocalSite
        {
          FileCatalog = FileCatalog
        }

