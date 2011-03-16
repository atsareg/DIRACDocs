===================================
System Administrator Console
===================================

The System Administrator Console (SAC) is the interface which allows a system administrator to connect
to any DIRAC server which is running a SystemAdministrator service. This interface allows to perform
all the system maintenance tasks remotely.  

Starting SAC
---------------

The SAC is invoked using dirac-admin-sysadmin-cli command for a given DIRAC server, for example::

    dirac-admin-sysadmin-cli --host volhcb01.cern.ch

This starts a special shell with a number of commands defined. There is a help available to see the
the list of commands and get info about particular commands::

    volhcb01.cern.ch>help
    
    Documented commands (type help <topic>):
    ========================================
    add   execfile  install  restart  show   stop  
    exec  exit      quit     set      start  update
    
    volhcb01.cern.ch>help set
    
            Set the host to be managed
        
            usage:
            
              set host <hostname>
              
Getting information
---------------------

The following command shows information about the host setup and currently used DIRAC software and extensions::

    volhcb03.cern.ch >show info
    
    Setup: LHCb-Certification
    DIRAC version: v5r12-pre9
    LHCb version v5r11p10
    LHCbWeb version v1r1
    
One can look up details of the software installed with the following command::

     volhcb01.cern.ch>show software
    
    {'Agents': {'Configuration': ['CE2CSAgent', 'UsersAndGroups'],
                'DataManagement': ['TransferAgent',
                                   'LFCvsSEAgent',
                                   'ReplicationScheduler',
                                   'FTSRegisterAgent',    
                                   ...
                                   
It will show all the components for which the software is available on the host, so these components can be 
installed and configured for execution. The information is grouped by component type ( Agents or Services ) and by
system. See below for how to setup the DIRAC components for running.

The status of the installed components can be obtained like::

    volhcb01.cern.ch> show status
    
       System                      Name                         Type         Setup    Installed Runit    Uptime    PID
    --------------------------------------------------------------------------------------------------------------------
    ResourceStatus               ResourceStatus               service        SetUp    Installed Down    2532910        0
    WorkloadManagement           SandboxStore                 service        SetUp    Installed Run        8390    20510
    WorkloadManagement           JobMonitoring                service        SetUp    Installed Run        8390    20494  
    ...
    
The output of the command shows for each component its system, name and type as well as the status information:

- Setup status shows if the component is set up for running on the host. It can take two values: SetUp/NotSetup ;
- Installed status shows if the component is installed on the host. This means that it is configured to run with 
  Runit system     
    
Managing DIRAC services and agents
-----------------------------------    
    
Updating the DIRAC installation                                     
---------------------------------    
              
          