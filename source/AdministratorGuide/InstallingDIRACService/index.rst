===================================
DIRAC Server Installation
===================================

The procedure described here outlines the installation of the DIRAC services on a host machine.
There are two distinct cases of service installations:

  - Primary installation. This the first installation of a fresh new system. No functioning
    Configuration Service is running yet.
  - Additional host installation. This is the installation of services on additional hosts 
    with the Master Configuration Service already up and running on another host. 
  
The primary service installation should install and start the Configuration Service which is a
backbone for the entire system. The SystemAdministrator service once installed will allow remote
management of the DIRAC components on the host. Multi-host installations are possible sharing
services in a single DIRAC setup using additional host installation procedure 

Requirements
-----------------------------------------------

    Server:
  

      - 9130-9200 ports should be open in the firewall for the incoming TCP/IP connections
      - Ports 80 and 443 should be open and redirected to ports 8080 and 8443 respectively by 
        setting appropriately the iptables
      - Host certificates in pem format 
      
    If gLite third party services are supposed to be used, for example, for the pilot job submission,
    gLite UI must be installed 

    Client:

      - User certificate in format .pem into $HOME/.globus directory with correct permissions.
      - User certificate loaded into Web Browser

Host preparation
---------------------------------

The host for the DIRAC services should be prepared for the service installation following the steps
below. This is the procedure necessary for both primary and additional host installations.

 - As root create a dirac user account. This is the account which will be used to run all the DIRAC services:::
 
      adduser -s /bin/bash -d /home/dirac dirac
      
  - As root, create the directory where the DIRAC services will be installed:::

      mkdir /opt/dirac
      chown -R dirac:dirac /opt/dirac 

  - As dirac user, create directories for security data and copy host certificate:::

      mkdir -p /opt/dirac/etc/grid-security/
      cp hostcert.pem hostkey.pem /opt/dirac/etc/grid-security
      
  - As dirac user, create a directory or a link pointing to the CA certificates directory, for example::
  
      ln -s /etc/grid-security/certificates  /opt/dirac/etc/grid-security/certificates    

  - As dirac user download the install_site.sh::
     
      mkdir /home/dirac/DIRAC
      cd /home/dirac/DIRAC
      wget -np http://lhcbproject.web.cern.ch/lhcbproject/dist/Dirac_project/install_site.sh
      
  - Check that the system clock is exact. Some system components are generating user certificate proxies dynamically and their
    validity can be broken because of the wrong system date and time.    

Primary host installation
----------------------------

The installation consists of setting up a minimal set of services which will allow to use
the SystemAdministrator interface to complete the installation. The following steps should
be taken:
 
  - Editing the installation configuration file. This file contains all
    the necessary information describing the installation. It can contain also any other configuration
    data. By editing the configuration file one can describe the complete DIRAC service or
    just a subset for initial setup. Below is an example of a commented configuration file.
    This file corresponds to a minimal DIRAC service configuration which allows to start
    using the system. ::


      # This section determines which DIRAC components will installed and where
    
            LocalInstallation
            {
               # DIRAC release version
               Release = v5r8
               # LCG software package version. Specify this option only if you need the gLite
               # UI installation
               LcgVer = 2009-08-13
               # Set this flag to yes if each DIRAC software update will be installed
               # in a separate directory, not overriding the previous ones
               UseVersionsDir = yes
               # The directory of the DIRAC software installation
               TargetPath = /opt/dirac
               # DIRAC extensions to be installed
               Extensions = LHCb, Web
               # Flag determining whether the Web Portal will be installed
               WebPortal = yes
            
               # Site name   
               SiteName = DIRAC.CERN.ch
               # Setup name
               Setup = LHCb-NewProduction
               # Default name of system instances 
               InstanceName = NewProduction
               # Flag to skip CA checks when talking to services
               SkipCAChecks = yes
               # Flag to use the server certificates
               UseServerCertificate = yes
               # Configuration Server URL
               ConfigurationServer = dips://dirac.cern.ch:9135/Configuration/Server
               # Flag to set up the Configuration Server as Master 
               ConfigurationMaster = yes
               # Configuration Name
               ConfigurationName = LHCb-NewProd 
            
               # Name of the Admin user (default: None )
               AdminUserName = atsareg
               # DN of the Admin user certificate (default: None )
               AdminUserDN = /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Andrei Tsaregorodtsev
               # Email of the Admin user (default: None )
               AdminUserEmail = atsareg@in2p3.fr
               # Name of the Admin group (default: dirac_admin )
               AdminGroupName = dirac_admin 
            
               # Name of the installation host (default: the current host )
               Host = dirac.cern.ch
               # DN of the host certificate (default: None )
               HostDN = /DC=ch/DC=cern/OU=computers/CN=volhcb29.cern.ch
            
               # List of DIRAC Systems to be installed
               Systems = Configuration,Framework
               # List of Services to be installed
               Services  = Configuration/Server
               Services += Framework/SystemAdministrator

            }

  - Run install_site.sh giving the edited CFG file as the argument:::
  
      ./install_site.sh install.cfg
      
  - If the installation is successful, in the end of the script execution you will see the report
    of the status of running LHCb services, e.g.:::
          
                                  Name : Runit    Uptime    PID
                  Configuration_Server : Run          41    30268
         Framework_SystemAdministrator : Run          21    30339
                             Web_httpd : Run           5    30828
                            Web_paster : Run           5    30829
        
Now the basic services - Configuration and SystemAdministrator - are installed. The rest of the installation can proceed using 
the DIRAC Administrator interface, either command line ( CLI ) or using Web Portal ( eventually, not available yet ).      

.. _setting_with_CLI:

Setting up DIRAC services using SystemAdministrator CLI 
-------------------------------------------------------

To use the SystemAdministrator CLI, you will need first to install the DIRAC Client software on some machine.
To install the DIRAC Client, follow the procedure described in the User Guide.

  - Start admin command line interface using administrator DIRAC group:::

       proxy-init -g dirac_admin
       dirac-admin-sysadmin-cli --host <HOST_NAME>
       
       where the HOST_NAME is the name of the DIRAC service host

  - Add instances of DIRAC systems which service will be running on the host, for example:::
  
      add instance WorkloadManagement NewProduction

  - Install MySQL database. You have to enter two passwords one is the root password for MySQL itself and another one is the 
    password for user who will own the DIRAC databases, in our case the user name is Dirac:::

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
accomplished with a single CLI command:::
 
      execfile <command_file> 

At this point all the services should be running with their default configuration parameters. 
To change the components configuration parameters

  - Login into web portal and choose dirac_admin group, you can change configuration file following those links::

      Systems -> Configuration -> Manage Configuration

  - In the server all the logs of the services are stored in separate files and can be checked using the following command
  
      tail -f  /opt/dirac/startup/<INSTANCE>_<Service or Agent>/log/current

Additional host installation
-----------------------------

To add a new host to the already existing DIRAC Setup the procedure is similar to the one for the
primary installation. You should perform all the preliminary steps to prepare the host for the
installation. One additional operation is the new host registration in the already functional
Configuration Service.

Then you edit the installation configuration file:::

  # This section determines which DIRAC components will installed and where
    
            LocalInstallation
            {
               # DIRAC release version, if omitted, the default version will be used
               Release = v5r8
               # LCG software package version. Specify this option only if you need the gLite
               # UI installation
               LcgVer = 2009-08-13
               # Set this flag to yes if each DIRAC software update will be installed
               # in a separate directory, not overriding the previous ones
               UseVersionsDir = yes
               # The directory of the DIRAC software installation
               TargetPath = /opt/dirac
               # DIRAC extensions to be installed
               Extensions = LHCb
               # Flag determining whether the Web Portal will be installed
               WebPortal = no
            
               # Site name   
               SiteName = DIRAC.CERN.ch
               # Setup name
               Setup = LHCb-NewProduction
               # Default name of system instances 
               InstanceName = NewProduction
               # Flag to skip CA checks when talking to services
               SkipCAChecks = yes
               # Flag to use the server certificates
               UseServerCertificate = yes
               # Configuration Server URL of already functional service 
               ConfigurationServer = dips://dirac.cern.ch:9135/Configuration/Server
               # Flag to set up the Configuration Server as Master 
               ConfigurationMaster = no
            
               # Name of the installation host (default: the current host )
               Host = dirac-aux.cern.ch
               # DN of the host certificate (default: None )
               HostDN = /DC=ch/DC=cern/OU=computers/CN=dirac-aux.cern.ch
            
               # List of DIRAC Systems to be installed
               Systems = Framework
               # List of Services to be installed
               Services = Framework/SystemAdministrator

            }  

Now run install_site.sh giving the edited CFG file as the argument:::
  
      ./install_site.sh install.cfg
      
If the installation is successful, the SystemAdministrator service will be up and running on the
host. You can now set up the required components as described in :ref:`setting_with_CLI`       
