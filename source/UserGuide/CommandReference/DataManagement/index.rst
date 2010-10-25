
=====================
DATA MANAGEMENT
=====================

Before run those commands, please load the DIRAC environment, the steps are explained at: [http://mareela.in2p3.fr:9200/dirac/wiki/DiracEnvironmentUsers]

----

Command  : **dirac-dms-add-file**

Used to  : This command permit copy and register a file into SE and LFC respectively.

Options :  *dirac-dms-add-file.py <LFN> <FILE PATH> <DIRAC SE> [<GUID>]*

*dirac-dms-add-file.py (<options>|<cfgFile>)*

Example:  ::

   dirac-dms-add-file -o LogLevel=FATAL LFN:/prod.vo.eu-eela.eu/user/h/hamar/UserEnv.csh UserEnv.csh UFRJ-disk
   {'Failed': {},
    'Successful': {'/prod.vo.eu-eela.eu/user/h/hamar/UserEnv.csh': {'put': 25.789539813995361,
                                                                           'register': 8.6198098659515381}}}
----

Command  : **dirac-dms-lfc-metadata**

Used to  : The output of this command shows the size and GUID of a file registered into LFC.

Options  : *dirac-dms-lfc-metadata.py <lfn | fileContainingLfns>*

Example : ::

   $dirac-dms-lfc-metadata /prod.vo.eu-eela.eu/user/h/hamar/LFC.sh
   FileName                                                                                             Size        GUID                                     Status   Checksum
   /prod.vo.eu-eela.eu/dirac/user/h/hamar/LFC.sh                                                       175        A01B1B3B-FFE0-2295-C29A-8B28DBDC9C3D     -        27a6382b

----

Command  : **dirac-dms-lfn-replicas**

Used to  : This command returns the replicas of a file registered into the LFC.

Options  : *dirac-dms-lfn-replicas.py <LFN> [<LFN>]*

 Example:::

   $ dirac-dms-lfn-replicas /prod.vo.eu-eela.eu/user/h/hamar/1.sh
   2009-09-07 08:39:29 UTC dirac-dms-lfn-replicas.py/DiracAPI  INFO: Replica Lookup Time: 2.31 seconds
   {'Failed': {},
    'Successful': {'/prod.vo.eu-eela.eu/user/h/hamar/1.sh': {'grid008.cecalc.ula.ve': 'srm://grid008.cecalc.ula.ve/dpm/cecalc.ula.ve/home/prod.vo.eu-eela.eu/generated/2009-09-01/filefd82d5de-53cb-4826-958c-cfabf2655a57',
                                                                    'lnx105.eela.if.ufrj.br': 'srm://lnx105.eela.if.ufrj.br/dpm/eela.if.ufrj.br/home/prod.vo.eu-eela.eu/generated/2009-08-12/filed5519e59-611b-4190-ad20-d4d77bf4b0af'}}}

----

Command  : **dirac-dms-pfn-metadata**

Used to  : The return of this command is the information of a file stored in a specific SE.[[BR]]

Options  : *dirac-dms-pfn-metadata.py <PFN> <SE>*

 Example: ::

   $ dirac-dms-pfn-metadata srm://lnx105.eela.if.ufrj.br/dpm/eela.if.ufrj.br/home/prod.vo.eu-eela.eu/prod.vo.eu-eela.eu   /dirac/user/h/hamar/LFC.sh UFRJ-disk
   2009-09-07 12:38:19 UTC dirac-dms-pfn-metadata.py  INFO: Succeded setting request    Accounting.DataStore.1252327098.64.0.394925959055 at dips://mareela.in2p3.fr:9143/RequestManagement/RequestManager
   {'Failed': {},
    'Successful': {'srm://lnx105.eela.if.ufrj.br/dpm/eela.if.ufrj.br/home/prod.vo.eu-eela.eu/prod.vo.eu-eela.eu/dirac/user/h/hamar /LFC.sh': {'Cached': 1,
                                                                                                                                           'Directory': False,                                                                                                                                     'File': True,
                                                                                                                                           'Lost': 0,
                                                                                                                                           'Migrated': 0,
                                                                                                                                           'Permissions': 436,
                                                                                                                                           'Size': 175L}                                                                                                                                      'Unavailable': 0}
----



Command : *dirac-dms-get-file*

Used to: Copy a file from SE to User Interface

Options  : *dirac-dms-get-file.py <LFN> [<LFN>]*


 Example:::

   $ dirac-dms-get-file /prod.vo.eu-eela.eu/user/h/hamar/LFC.sh
   2009-09-07 12:24:34 UTC dirac-dms-get-file.py  INFO: Succeded setting request Accounting.DataStore.1252326273.74.0.0482830676775 at dips://mareela.in2p3.fr:9143/RequestManagement/RequestManager
   {'Failed': {},
    'Successful': {'/prod.vo.eu-eela.eu/user/h/hamar/LFC.sh': 'LFC.sh'}}

----

Command  : **dirac-dms-remove-catalog-replicas**

Used to  : With this command the user can remove a file replica from a SE.

Options  : *dirac-dms-remove-replicas.py <LFN | fileContainingLFNs> SE*

Example:::

   $dirac-dms-remove-catalog-replicas /prod.vo.eu-eela.eu/user/h/hamar/1.sh UFRJ-disk
   Successfully removed UFRJ-disk replica of /prod.vo.eu-eela.eu/user/h/hamar/1.sh

----

Command  : **dirac-dms-replicate-lfn**

Used to  : This commands permit replicate a file from one SE to another.

Options  : *dirac-dms-replicate-lfn.py <LFN> <DESTINATION SE> [<SOURCE SE> <LOCAL CACHE>]*

*dirac-dms-replicate-lfn.py (<options>|<cfgFile>)*

Example: ::

   $dirac-dms-replicate-lfn -o LogLevel=FATAL /prod.vo.eu-eela.eu/user/h/hamar/UserEnv.sh UNIANDES-disk
   {'Failed': {},
    'Successful': {'/prod.vo.eu-eela.eu/user/h/hamar/UserEnv.sh': {'register': 3.2478630542755127,
                                                                          'replicate': 63.71432900428772}}

----

Command  : **dirac-dms-user-lfns**

Used to  : This command return the number of files registered into the LFC.

Options  : *dirac-dms-user-lfns.py (<options>|<cfgFile>)*

 Options:

::

    -o:  --option=  :  Option=value to add
    -s:  --section=  :  Set base section for relative parsed options
    -c:  --cert=  :  Use server certificate to connect to Core Services
    -h:  --help  :  Shows this help
    -d:  --Days=  :       Match files older than number of days [0]
    -m:  --Months=  :     Match files older than number of months [0]
    -y:  --Years=  :      Match files older than number of years [0]
    -w:  --Wildcard=  :   Wildcard for matching filenames [*]
    -b:  --BaseDir=  :    Base directory to begin search [/lhcb/user/initial/username]

 Example:::

   $ dirac-dms-user-lfns
   2009-09-07 11:01:27 UTC dirac-dms-user-lfns.py  INFO: Will search for files in /prod.vo.eu-eela.eu/user/h/hamar
   2009-09-07 11:01:33 UTC dirac-dms-user-lfns.py  INFO: /prod.vo.eu-eela.eu/user/h/hamar: 2 files, 0 sub-directories
   2009-09-07 11:01:33 UTC dirac-dms-user-lfns.py  INFO: 2 matched files have been put in -prod.vo.eu-eela.eu-dirac-user-h-hamar.lfns

----

Command  : **dirac-dms-lfn-accessURL**

Used to  : This command permit to the user know the rfio of a file stored in a specific SE.[[BR]]

Options  : *dirac-dms-lfn-accessURL.py <LFN> <SE>*

Example  : ::

   $ dirac-dms-lfn-accessURL /prod.vo.eu-eela.eu/user/h/hamar/LFC.sh UFRJ-disk
   2009-09-07 13:41:22 UTC dirac-dms-lfn-accessURL.py  INFO: Succeded setting request  Accounting.DataStore.1252330881.65.0.537978640081 at dips://mareela.in2p3.fr:9143/RequestManagement/RequestManager
   {'Failed': {},
    'Successful': {'/prod.vo.eu-eela.eu/user/h/hamar/LFC.sh': 'rfio://lnx105.eela.if.ufrj.br//storage/dpm/prod.vo.eu-eela.eu /2009-09-07/LFC.sh.152291.0'}}

----

Command  : **dirac-dms-pfn-accessURL**

Used to  : The output of this command shown rfio of a file stored in a SE.

Options  : *dirac-dms-pfn-accessURL.py <PFN> <SE>*

Example  : ::

   $dirac-dms-pfn-accessURL srm://lnx105.eela.if.ufrj.br/dpm/eela.if.ufrj.br/home/prod.vo.eu-eela.eu/prod.vo.eu-eela.eu/dirac/user/h/hamar/LFC.sh UFRJ-disk -o LogLevel=FATAL
   {'Failed': {},
    'Successful': {'srm://lnx105.eela.if.ufrj.br/dpm/eela.if.ufrj.br/home/prod.vo.eu-eela.eu/prod.vo.eu-eela.eu/dirac/user/h/hamar /LFC.sh': 'rfio://lnx105.eela.if.ufrj.br//storage/dpm/prod.vo.eu-eela.eu/2009-09-07/LFC.sh.152372.0'}


----

Command  : **dirac-dms-lfn-metadata**

Used to  : Output of this command shows all the metadata of a file registered into LFC.

Options  : *dirac-dms-lfn-metadata.py <LFN> [<LFN>]*

Example  : ::

   $ dirac-dms-lfn-metadata /prod.vo.eu-eela.eu/user/h/hamar/LFC.sh
   2009-09-07 14:13:27 UTC dirac-dms-lfn-metadata.py/DiracAPI  INFO: Metadata Lookup Time: 3.33 seconds
   {'Failed': {},
    'Successful': {'/prod.vo.eu-eela.eu/dirac/user/h/hamar/LFC.sh': {'CheckSumType': 'AD',
                                                                   'CheckSumValue': '27a6382b',
                                                                   'CreationTime': 'Mon Sep  7 14:20:29 2009',
                                                                   'GUID': 'A01B1B3B-FFE0-2295-C29A-8B28DBDC9C3D',
                                                                   'ModificationTime': 'Mon Sep  7 14:20:29 2009',
                                                                   'NumberOfLinks': 1,
                                                                   'Permissions': 436,
                                                                   'Size': 175L,
                                                                   'Status': '-'}}}

----

Command  : **dirac-dms-remove-files**

Used to  : This command could be used to remove replicas of a file registered into LFC.

*dirac-dms-remove-replicas.py <LFN | fileContainingLFNs>*

Example  :  ::

   $ dirac-dms-remove-files /prod.vo.eu-eela.eu/user/h/hamar/UserEnv.csh -o LogLevel=FATAL
   Successfully removed /prod.vo.eu-eela.eu/user/h/hamar/UserEnv.csh

----

Command  : **dirac-dms-remove-catalog-replicas**

Used to  : This command permit users remove replicas from a SE.

Options  : *dirac-dms-remove-replicas.py <LFN | fileContainingLFNs> SE*

Example  : ::

   $  dirac-dms-remove-catalog-replicas /prod.vo.eu-eela.eu/user/h/hamar/LFC.sh UFRJ-disk
   2009-09-07 14:31:00 UTC dirac-dms-remove-catalog-replicas.py  INFO: ReplicaManager.__removeCatalogReplica: Successfully removed replica. /prod.vo.eu-eela.eu/dirac/user/h/hamar/LFC.sh
   Successfully removed UFRJ-disk replica of /prod.vo.eu-eela.eu/user/h/hamar/LFC.sh

----

Command  : **dirac-dms-remove-lfn**

Used to  : Remove a LFN from the LFC the user could use this command.

Options  : *dirac-dms-remove-lfn.py <LFN> [<LFN>]*

Example  : ::

   $ dirac-dms-remove-lfn /prod.vo.eu-eela.eu/user/h/hamar/LFC.sh
   {'Failed': {},
    'Successful': {'/prod.vo.eu-eela.eu/user/h/hamar/LFC.sh': {'LcgFileCatalogCombined': True}}}

----

Command  : **dirac-dms-remove-pfn**

Used to  : With this command the user remove a file from an SE.

Options  : *dirac-dms-remove-pfn.py <PFN> <SE>*

Example  : ::

   (DIRAC3-user)$dirac-dms-remove-pfn srm://lnx105.eela.if.ufrj.br/dpm/eela.if.ufrj.br/home/prod.vo.eu-eela.eu/prod.vo.eu-eela.eu/dirac/user/h/hamar/LFC.sh UFRJ-disk
   2009-09-07 14:39:11 UTC dirac-dms-remove-pfn.py  INFO: Succeded setting request  Accounting.DataStore.1252334351.16.0.754142315293 at dips://mareela.in2p3.fr:9143/RequestManagement/RequestManager
   {'Failed': {},
    'Successful': {'srm://lnx105.eela.if.ufrj.br/dpm/eela.if.ufrj.br/home/prod.vo.eu-eela.eu/prod.vo.eu-eela.eu/dirac/user/h/hamar/LFC.sh': {'Cached': 1,
                                                                                                                                           'Directory': False,
                                                                                                                                           'File': True,
                                                                                                                                           'Lost': 0,
                                                                                                                                           'Migrated': 0,
                                                                                                                                           'Permissions': 436,
                                                                                                                                           'Size': 175L,
                                                                                                                                           'Unavailable': 0}
----

Command  : **dirac-dms-replica-metadata**

Used to  : This command shows metadata of a file.

Options  : *dirac-dms-replica-metadata <lfn | fileContainingLfns> SE*

Example  : ::

   (DIRAC3-user)$dirac-dms-replica-metadata /prod.vo.eu-eela.eu/user/h/hamar/LFC.sh.orig UFRJ-disk
   File                                                                                                 Migrated Cached   Size (bytes)
   /prod.vo.eu-eela.eu/user/h/hamar/LFC.sh.orig                                                  0        1        175

