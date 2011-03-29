===================================
DIRAC Server Installation
===================================

The procedure described here outlines the installation of the DIRAC components on a host machine, a 
DIRAC server. There are two distinct cases of installations:

  - *Primary server installation*. This the first installation of a fresh new DIRAC system. No functioning
    Configuration Service is running yet (:ref:`install_primary_server`).
  - *Additional server installation*. This is the installation of additional hosts connected to an already 
    existing DIRAC system, with the Master Configuration Service already up and running on another 
    DIRAC server (:ref:`install_additional_server`).

The primary server installation should install and start at least the Configuration Service which is the
backbone for the entire DIRAC system. The SystemAdministrator Service, once installed, allows remote
management of the DIRAC components on the server. In multi-server installations DIRAC components are 
distributed among a number of servers installed using the procedure for additional host installation.

For all DIRAC installations any number of client installations is possible.

Requirements
-----------------------------------------------

*Server:*

      - 9130-9200 ports should be open in the firewall for the incoming TCP/IP connections (this is the 
        default range if predefined ports are used, the port on which services are listening can be 
        configured by the DIRAC administrator);
      - For the server hosting the portal, ports 80 and 443 should be open and redirected to ports 
        8080 and 8443 respectively, i.e. setting iptables appropriately; 
      - Grid host certificates in pem format;
      - At least one of the servers of the installation must have updated CAs and CRLs files;
      - If gLite third party services are needed (for example, for the pilot job submission via WMS 
        or for data transfer using FTS) gLite User Interface must be installed and the environment set up 
        by "sourcing" the corresponding script, e.g. /etc/profile.d/grid-env.sh.

*Client:*

      - User certificate and private key in .pem format in the $HOME/.globus directory with correct 
        permissions.
      - User certificate loaded into the Web Browser (currently supported browsers are: Mozilla Firefox, Chrome 
        and Safari)

Server preparation
---------------------------------

Any host running DIRAC server components should be prepared before the installation of DIRAC following 
the steps below. This procedure must be followed for the primary server and for any additional server installations.

 - As *root* create a *dirac* user account. This account will be used to run all the DIRAC components::

      adduser -s /bin/bash -d /home/dirac dirac

 - As *root*, create the directory where the DIRAC services will be installed::

      mkdir /opt/dirac
      chown -R dirac:dirac /opt/dirac 

 - As *root*, check that the system clock is exact. Some system components are generating user certificate proxies 
   dynamically and their validity can be broken because of the wrong system date and time. Properly configure
   the NTP daemon if necessary.

 - As *dirac* user, create directories for security data and copy host certificate::

      mkdir -p /opt/dirac/etc/grid-security/
      cp hostcert.pem hostkey.pem /opt/dirac/etc/grid-security

 - As *dirac* user, create a directory or a link pointing to the CA certificates directory, for example::

      ln -s /etc/grid-security/certificates  /opt/dirac/etc/grid-security/certificates    

   (this is only mandatory in one of the servers. Others can be synchronized from this one using DIRAC tools.)

 - As *dirac* user download the install_site.sh script::

      mkdir /home/dirac/DIRAC
      cd /home/dirac/DIRAC
      wget -np http://lhcbproject.web.cern.ch/lhcbproject/dist/Dirac_project/install_site.sh

.. _install_primary_server:

Primary server installation
----------------------------

The installation consists of setting up a set of services, agents and databases for the
required DIRAC functionality. The SystemAdministrator interface can be used later to complete 
the installation by setting up additional components. The following steps should
be taken:
 
  - Editing the installation configuration file. This file contains all
    the necessary information describing the installation. By editing the configuration 
    file one can describe the complete DIRAC server or
    just a subset for the initial setup. Below is an example of a commented configuration file.
    This file corresponds to a minimal DIRAC server configuration which allows to start
    using the system::

      #
      # This section determines which DIRAC components will be installed and where
      #
      LocalInstallation
      {
        #
        #   These are options for the installation of the DIRAC software
        #
        #  DIRAC release version (this is an example, you should find out the current 
        #  production release)
        Release = v5r13
        #  Python version os the installation
        PythonVersion = 26
        #  To install the Server version of DIRAC (the default is client)
        InstallType = server
        #  LCG python bindings for SEs and LFC. Specify this option only if your installation
        #  uses those services
        # LcgVer = 2010-11-20
        #  If this flag is set to yes, each DIRAC update will be installed
        #  in a separate directory, not overriding the previous ones
        UseVersionsDir = yes
        #  The directory of the DIRAC software installation
        TargetPath = /opt/dirac
        #  DIRAC extensions to be installed (Web is required if you are installing the Portal on 
        #  this server).
        #  For each User Community their own extension might be necessary here: 
        #   i.e. LHCb, LHCbWeb for LHCb
        Extensions = Web

        #
        #   These are options for the configuration of the installed DIRAC software
        #   i.e., to produce the initial dirac.cfg for the server
        #
        #  Give a Name to your User Community, it does not need to be the same name as in EGI, 
        #  it can be used to cover more than one VO in the grid sense
        VirtualOrganization = Name of your VO
        #  Site name   
        SiteName = DIRAC.HostName.ch
        #  Setup name
        Setup = MyDIRAC-Production
        #  Default name of system instances 
        InstanceName = Production
        #  Flag to use the server certificates
        UseServerCertificate = yes
        #  Configuration Server URL (This should point to the URL of at least one valid Configuration 
        #  Service in your installation, for the primary server it should not used)
        #  ConfigurationServer = dips://myprimaryserver.name:9135/Configuration/Server
        #  Flag to set up the Configuration Server as Master (use only in the primary server)
        ConfigurationMaster = yes
        #  Configuration Name
        ConfigurationName = MyConfiguration

        #
        #   These options define the DIRAC components to be installed on "this" DIRAC server.
        #
        #
        #  The next options should only be set for the primary server,
        #  they properly initialize the configuration data
        #
        #  Name of the Admin user (default: None )
        AdminUserName = atsareg
        #  DN of the Admin user certificate (default: None )
        #  In order the find out the DN that needs to be included in the Configuration for a given 
        #  host or user certificate the following command can be used::
        #
        #          openssl x509 -noout -subject -enddate -in <certfile.pem>
        #
        AdminUserDN = /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Andrei Tsaregorodtsev
        #  Email of the Admin user (default: None )
        AdminUserEmail = atsareg@in2p3.fr
        #  Name of the Admin group (default: dirac_admin )
        AdminGroupName = dirac_admin 
        #  DN of the host certificate (*) (default: None )
        HostDN = /DC=ch/DC=cern/OU=computers/CN=volhcb29.cern.ch
        # Define the Configuration Server as Master for your installations
        ConfigurationMaster = yes
        
        #
        #  The following options define components to be installed
        #
        #  Name of the installation host (default: the current host )
        #  Used to build the URLs the services will publish
        # Host = dirac.cern.ch
        Host = 
        #  List of Services to be installed
        Services  = Configuration/Server
        Services += Framework/SystemAdministrator
        #  Flag determining whether the Web Portal will be installed
        WebPortal = yes
        #
        #  The following options defined the MySQL DB connectivity
        #
        Database
        {
          #  User name used to connect the DB server
          User = [default: Dirac]
          #  Password for database user acess. Must be set for SystemAdministrator Service to work
          Password = XXXX
          #  Password for root DB user. Must be set for SystemAdministrator Service to work
          RootPwd = YYYY
          #  location of DB server. Must be set for SystemAdministrator Service to work
          Host = [default: localhost]
          #  There are 2 flags for small and large installations Set either of them to True/yes when appropriated
          # MySQLSmallMem:        Configure a MySQL with small memory requirements for testing purposes
          #                       innodb_buffer_pool_size=200MB
          # MySQLLargeMem:        Configure a MySQL with high memory requirements for production purposes
          #                       innodb_buffer_pool_size=10000MB
         }
       }

  - Run install_site.sh giving the edited configuration file as the argument. The configuration file must have
    .cfg extension (CFG file)::

      ./install_site.sh install.cfg
      
  - If the installation is successful, in the end of the script execution you will see the report
    of the status of running DIRAC services, e.g.::
          
                                  Name : Runit    Uptime    PID
                  Configuration_Server : Run          41    30268
         Framework_SystemAdministrator : Run          21    30339
                             Web_httpd : Run           5    30828
                            Web_paster : Run           5    30829
        
Now the basic services - Configuration and SystemAdministrator - are installed. The rest of the installation can proceed using 
the DIRAC Administrator interface, either command line (System Administrator Console) or using Web Portal (eventually, 
not available yet).

It is also possible to include any number of additional systems, services, agents and databases to be installed by "install_site.sh".

.. _install_additional_server:

Additional server installation
------------------------------------

To add a new server to an already existing DIRAC Installation the procedure is similar to the one above. 
You should perform all the preliminary steps to prepare the host for the installation. One additional 
operation is the registration of the new host in the already functional Configuration Service.

  - Then you edit the installation configuration file::

      #
      # This section determines which DIRAC components will be installed and where
      #
      LocalInstallation
      {
        #
        #   These are options for the installation of the DIRAC software
        #
        #  DIRAC release version (this is an example, you should find out the current 
        #  production release)
        Release = v5r13
        #  To install the Server version of DIRAC (the default is client)
        InstallType = server
        #  LCG python bindings for SEs and LFC. Specify this option only if your installation
        #  uses those services
        # LcgVer = 2010-11-20
        #  If this flag is set to yes, each DIRAC update will be installed
        #  in a separate directory, not overriding the previous ones
        UseVersionsDir = yes
        #  The directory of the DIRAC software installation
        TargetPath = /opt/dirac
        #  DIRAC extensions to be installed (Web is required if you are installing the Portal on 
        #  this server).
        #  For each User Community their own extension might be necessary here: 
        #   i.e. LHCb, LHCbWeb for LHCb
        Extensions = 

        #
        #   These are options for the configuration of the previously installed DIRAC software
        #   i.e., to produce the initial dirac.cfg for the server
        #
        #  Give a Name to your User Community, it does not need to be the same name as in EGI, 
        #  it can be used to cover more than one VO in the grid sense
        VirtualOrganization = Name of your VO
        #  Site name   
        SiteName = DIRAC.HostName2.ch
        #  Setup name
        Setup = MyDIRAC-Production
        #  Default name of system instances 
        InstanceName = Production
        #  Flag to use the server certificates
        UseServerCertificate = yes
        #  Configuration Server URL (This should point to the URL of at least one valid Configuration 
        #  Service in your installation, for the primary server it should not used)
        ConfigurationServer = dips://myprimaryserver.name:9135/Configuration/Server
        ConfigurationServer += dips://localhost:9135/Configuration/Server
        #  Configuration Name
        ConfigurationName = MyConfiguration

        #
        #   These options define the DIRAC components being installed on "this" DIRAC server.
        #   The simplest option is to install a slave of the Configuration Server and a 
        #   SystemAdministrator for remote management.
        #
        #  The following options defined components to be installed
        #
        #  Name of the installation host (default: the current host )
        #  Used to build the URLs the services will publish
        # Host = dirac.cern.ch
        Host = 
        #  List of Services to be installed
        Services  = Configuration/Server
        Services += Framework/SystemAdministrator

  - Now run install_site.sh giving the edited CFG file as the argument:::
  
        ./install_site.sh install.cfg

If the installation is successful, the SystemAdministrator service will be up and running on the
server. You can now set up the required components as described in :ref:`setting_with_CLI`

Post-Installation step
---------------------------

In order to make the DIRAC components running we use the *runit* mechanism (http://smarden.org/runit/). For each component that 
must run permanently (services and agents) there is a directory created under */opt/dirac/startup* that is 
monitored by a *runsvdir* daemon. The installation procedures above will properly start this daemon. In order 
to ensure starting the DIRAC components at boot you need to add a hook in your boot sequence. A possible solution
is to add an entry in the */etc/initab*::

      SV:123456:respawn:/opt/dirac/sbin/runsvdir-start

Together with a script like (it assumes that in your server DIRAC is using *dirac* local user to run)::

      #!/bin/bash
      source /opt/dirac/bashrc
      RUNSVCTRL=`which runsvctrl`
      chpst -u dirac $RUNSVCTRL d /opt/dirac/startup/*
      killall runsv svlogd
      killall runsvctrl
      /opt/dirac/pro/mysql/share/mysql/mysql.server stop  --user=dirac
      sleep 10
      /opt/dirac/pro/mysql/share/mysql/mysql.server start --user=dirac
      sleep 20
      RUNSVDIR=`which runsvdir`
      exec chpst -u dirac $RUNSVDIR -P /opt/dirac/startup 'log:  DIRAC runsv'

The same script can be used to restart all DIRAC components running on the machine.

.. _setting_with_CLI:

Setting up DIRAC services and agents using the System Administrator Console
----------------------------------------------------------------------------

To use the :ref:`system-admin-console`, you will need first to install the DIRAC Client software on some machine.
To install the DIRAC Client, follow the procedure described in the User Guide.

  - Start admin command line interface using administrator DIRAC group::

      dirac-proxy-init -g dirac_admin
      dirac-admin-sysadmin-cli --host <HOST_NAME>

      where the HOST_NAME is the name of the DIRAC service host

  - At any time you can use the help command to get further details::

      dirac.pic.es >help
      
      Documented commands (type help <topic>):
      ========================================
      add   execfile  install  restart  show   stop  
      exec  exit      quit     set      start  update
      
      Undocumented commands:
      ======================
      help

  - Add instances of DIRAC systems which service or agents will be running on the server, for example::

      add instance WorkloadManagement Production

  - Install MySQL database. You have to enter two passwords one is the root password for MySQL itself (if not already done in the server installation) 
    and another one is the password for user who will own the DIRAC databases, in our case the user name is Dirac::

      install mysql
      MySQL root password:
      MySQL Dirac password:

  - Install databases, for example:::

      install db ComponentMonitoringDB

  - Install services and agents, for example:::

      install service WorkloadManagement JobMonitoring
      ...
      install agent Configuration CE2CSAgent

Note that all the necessary commands above can be collected in a text file and the whole installation can be 
accomplished with a single command:::

      execfile <command_file> 
      
Component Configuration and Monitoring
---------------------------------------- 

At this point all the services should be running with their default configuration parameters. 
To change the components configuration parameters

  - Login into web portal and choose dirac_admin group, you can change configuration file following these links::

      Systems -> Configuration -> Manage Configuration

  - In the server all the logs of the services and agents are stored and rotated in 
    files that can be checked using the following command::

      tail -f  /opt/dirac/startup/<System>_<Service or Agent>/log/current

