.. _system-admin-console:

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


Show setup command allows administrators to know which components, Services and Agents are setup up in the host::

    mardirac1.in2p3.fr >show setup
    {'Agents': {'Configuration': ['CE2CSAgent'],
                'Framework': ['TopErrorMessagesReporter',
                              'SystemLoggingDBCleaner',
                              'CAUpdateAgent'],
                'WorkloadManagement': ['JobHistoryAgent',
                                       'InputDataAgent',
                                       'StalledJobAgent',
                                       'TaskQueueDirector',
                                       'MightyOptimizer',
                                       'PilotStatusAgent',
                                       'JobCleaningAgent',
                                       'StatesAccountingAgent']},
     'Services': {'Accounting': ['ReportGenerator', 'DataStore'],
                  'Configuration': ['Server'],
                  'Framework': ['Monitoring',
                                'BundleDelivery',
                                'SecurityLogging',
                                'Notification',
                                'UserProfileManager',
                                'SystemAdministrator',
                                'ProxyManager',
                                'SystemLogging'],
                  'RequestManagement': ['RequestManager'],
                  'WorkloadManagement': ['JobMonitoring',
                                         'WMSAdministrator',
                                         'SandboxStore',
                                         'Matcher',
                                         'JobStateUpdate',
                                         'JobManager']}}

SAC also allow which databases are installed::

    mardirac1.in2p3.fr >show database
    MySQL root password:

                DataLoggingDB : Not installed
            SandboxMetadataDB : Installed
                        JobDB : Installed
                     MPIJobDB : Not installed
                FileCatalogDB : Installed
             TransformationDB : Not installed
                 JobLoggingDB : Installed
                UserProfileDB : Installed

Show the status of the MySQL server::

    mardirac1.in2p3.fr >show mysql

                     FlushTables : 1
                      OpenTables : 47
             NumberOfSlowQueries : 0
               NumberOfQuestions : 24133
                          UpTime : 15763
                 NumberOfThreads : 13
                   NumberOfOpens : 203
                QueriesPerSecond : 1.530

Is also possible to check logs for services and agents using SAC::

    mardirac1.in2p3.fr>show log WorkloadManagement JobMonitoring
    2011-03-16 14:28:15 UTC WorkloadManagement/JobMonitoring  INFO: Sending records to security log service...
    2011-03-16 14:28:15 UTC WorkloadManagement/JobMonitoring  INFO: Data sent to security log service
    2011-03-16 14:29:15 UTC WorkloadManagement/JobMonitoring  INFO: Sending records to security log service...
    2011-03-16 14:29:15 UTC WorkloadManagement/JobMonitoring  INFO: Data sent to security log service



Managing DIRAC services and agents
-----------------------------------

Using SAC the installation of DIRAC components (DBs, Services, Agents) and MySQL Server can be done.

Usage:::

    install mysql
    install db <database>
    install service <system> <service>
    install agent <system> <agent>

To install MySQL server::

    mardirac1.in2p3.fr >install mysql
    Installing MySQL database, this can take a while ...
    MySQL Dirac password:
    MySQL: Already installed

Installation of Databases for services can be added::

    mardirac1.in2p3.fr >install db MPIJobDB
    Adding to CS WorkloadManagement/MPIJobDB
    Database MPIJobDB from EELADIRAC/WorkloadManagementSystem installed successfully

Addition of new services::

    mardirac1.in2p3.fr >install service WorkloadManagement MPIService
    service WorkloadManagement_MPIService is installed, runit status: Run

Addition of new agents::

    mardirac1.in2p3.fr >install agent Configuration CE2CSAgent
    agent Configuration_CE2CSAgent is installed, runit status: Run

Start services or agents or database server::

Usage::

    start <system|*> <service|agent|*>
    start mysql

For example, start a service::

    mardirac1.in2p3.fr >start WorkloadManagement MPIService

    WorkloadManagement_MPIService started successfully, runit status:

    WorkloadManagement_MPIService : Run

Restart services or agents or database server::

Usage::

    restart <system|*> <service|agent|*>
    restart mysql

Restarting all the services::

   mardirac1.in2p3.fr >restart *
   All systems are restarted, connection to SystemAdministrator is lost

Restarting a specific service::

   mardirac1.in2p3.fr >restart WorkloadManagement MPIService

   WorkloadManagement_MPIService started successfully, runit status:

   WorkloadManagement_MPIService : Run

Stop services or agents or database server::

Usage::

    stop <system|*> <service|agent|*>
    stop mysql

Stop all the services::

   mardirac1.in2p3.fr >stop *

Stop a specific service or agent::

   mardirac1.in2p3.fr >stop WorkloadManagement MPIService

   WorkloadManagement_MPIService stopped successfully, runit status:

   WorkloadManagement_MPIService : Down




Updating the DIRAC installation
---------------------------------

The SAC allows to update the software on the target host to a given version.

Usage::

    update <version>

For example::

    $ dirac-admin-sysadmin-cli --host mardirac1.in2p3.fr
    DIRAC Root Path = /home/vanessa/DIRAC-v5r12
    mardirac1.in2p3.fr >update v5r12p7
    Software update can take a while, please wait ...
    Software successfully updated.
    You should restart the services to use the new software version.
    mardirac1.in2p3.fr >restart *
    All systems are restarted, connection to SystemAdministrator is lost
    mardirac1.in2p3.fr >quit

If the administrator needs to continue working with SAC, it must be started again.


