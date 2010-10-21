===================================
DIRAC Server Installation
===================================

The procedure described here outlines the installation of the DIRAC services on a host machine.
The primary service installation should install and start the Configuration Service which is a
backbone for the entire system. SystemAdministrator service once installed will allow remote
management of the DIRAC components on the host. Multi-host installations are also possible sharing
services in a single DIRAC setup. 

Requirements
-----------------------------------------------

    Server:
  

      - 9130-9200 ports should be open in the firewall for the incoming TCP/IP connections
      - Ports 80 and 443 should be open and redirected to ports 8080 and 8443 respectively by setting appropriately the iptables
      - Host certificates in pem format 
      
    If gLite third party services are supposed to be used, for example, for the pilot job submission,
    gLite UI should installed 

    Client:

      - User certificate in format .pem into $HOME/.globus directory with correct permissions.
      - User certificate loaded into Web Browser


Installing basic services
-------------------------------------------

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
 
  - Now this is the time to edit the installation configuration file. This file contains all
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
               # LCG software package version
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
               # Server or client type installation
               InstallType = server
            
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
               ConfigurationServer = dips://volhcb29.cern.ch:9135/Configuration/Server
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
               Host = volhcb29.cern.ch
               # DN of the host certificate (default: None )
               HostDN = /DC=ch/DC=cern/OU=computers/CN=volhcb29.cern.ch
               # Type of the installation host Normal or Power (default: Normal )
               HostType = Power   
            
               # The list of DIRAC Systems to be installed
               Systems = Configuration,Framework
               # The list of Services to be installed
               Services = Configuration/Server,Framework/SystemAdministrator
               # The list of Agents to be installed
               Agents = 
            }

  - Run install_site.sh giving the edited CFG file as the argument:::
  
      ./install_seed.sh install.cfg
      
  - If the installation is successful, in the end of the script execution you will see the report
    of the status of running LHCb services, e.g.:::
          
                                  Name : Runit    Uptime    PID
                  Configuration_Server : Run          41    30268
         Framework_SystemAdministrator : Run          21    30339
                             Web_httpd : Run           5    30828
                            Web_paster : Run           5    30829
        
Now the basic services - Configuration and SystemAdministrator - are installed. The rest of the installation can proceed using 
the DIRAC Administrator interface, either command line ( CLI ) or using Web Portal ( eventually, not ready yet ).      

Setting up DIRAC services using SystemAdministrator CLI 
-------------------------------------------------------

To use the SystemAdministrator CLI, you will need first to install the DIRAC Client software on some machine.
To install the DIRAC Client, follow the procedure described here [link to the client installation]

  - Start admin command line interface using diracAdmin DIRAC group:::

       proxy-init -g diracAdmin
       dirac-admin-sysadmin-cli --host <HOST_NAME>
       
       where the HOST_NAME is the name of the DIRAC service host

  - Install MySQL database. You have to enter two passwords one is the root password for MySQL itself and another one is the 
    password for user who will own the DIRAC databases, in our case the user name is Dirac:::

      install mysql
      MySQL root password:
      MySQL Dirac password:

  - Install databases:::

      install db ComponentMonitoringDB
      install db FileCatalogDB
      install db JobDB
      install db PilotAgentsDB
      install db ProxyDB
      install db RequestDB
      install db SandboxMetadataDB
      install db SystemLoggingDB
      install db TaskQueueDB
      install db UserProfileDB
      install db NotificationDB
      install db MPIJobDB
      install db AccountingDB
      install db JobLoggingDB 

  - Install services and agents:::

      install service WorkloadManagement JobMonitoring
      install service WorkloadManagement JobManager
      install service WorkloadManagement JobStateUpdate
      install service DataManagement StorageElement
      install service Framework SystemLogging
      install service Framework Monitoring
      install service RequestManagement RequestManager
      install service Framework SystemLoggingReport
      install service WorkloadManagement WMSAdministrator
      install service DataManagement FileCatalog
      install service Framework ProxyManager
      install service Framework SecurityLogging
      install service Framework Notification
      install service Framework UserProfileManager
      install service Framework BundleDelivery
      install service Framework SystemAdministrator
      install service WorkloadManagement Matcher
      install service WorkloadManagement MPIService
      install service WorkloadManagement SandboxStore 
      install service Accounting DataStore
      install agent Framework CAUpdateAgent
      install agent Framework SystemLoggingDBCleaner
      install agent Framework TopErrorMessagesReporter
      install agent WorkloadManagement JobCleaningAgent
      install agent WorkloadManagement MightyOptimizer
      install agent WorkloadManagement StalledJobAgent
      install agent WorkloadManagement TaskQueueDirector
      install agent WorkloadManagement PilotStatusAgent
      install agent WorkloadManagement PilotMonitoringAgent
      install agent Configuration CE2CSAgent
 
 At this point all the services should be running with their default configuration parameters. 

  - Login into web portal and choose diracAdmin group, you can change configuration file following those links::

      Systems -> Configuration -> Manage Remote Configuration

  - In the server all the logs of the services are stored in separately files, can be checked using the following command
  
      tail -f  /opt/dirac/startup/<INSTANCE>_<Service or Agent>/log/current

Configuration
--------------------

  Missing Variables::

    - VOMSRole
    - VOMSVO
    -  Operatios -> EMail -> Production
    - EnableAutoMerge = yes ??
    - 
    
  Variables to change::
   
    - RB
    
  Voms certificate::
  
    /opt/dirac/etc/grid-security/vomsdir
  
  