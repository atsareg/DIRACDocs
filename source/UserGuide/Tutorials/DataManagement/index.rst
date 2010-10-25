===============================================
DATA MANAGEMENT
===============================================

Requirements
-----------------------------------------------
- Have access to a DIRAC client installation
- Have a valid proxy


Getting information
-----------------------------------------------

SE Availables
@@@@@@@@@@@@@@

- The first thing is to know which Storage Elements are available for the VO, run the command::

        dirac-dms-show-se-status

  By example::

        $dirac-dms-show-se-status
        2010-10-17 20:32:40 UTC dirac-dms-show-se-status.py  INFO: Storage Element               Read Status    Write Status
        2010-10-17 20:32:40 UTC dirac-dms-show-se-status.py  INFO: DIRAC-USER                         Active          Active
        2010-10-17 20:32:40 UTC dirac-dms-show-se-status.py  INFO: IN2P3-disk                         Active          Active
        2010-10-17 20:32:40 UTC dirac-dms-show-se-status.py  INFO: IPSL-IPGP-disk                     Active          Active
        2010-10-17 20:32:40 UTC dirac-dms-show-se-status.py  INFO: IRES-disk                          Active          Active
        2010-10-17 20:32:40 UTC dirac-dms-show-se-status.py  INFO: LPSC-disk                          Active          Active
        2010-10-17 20:32:40 UTC dirac-dms-show-se-status.py  INFO: M3PEC-disk                         Active          Active
        2010-10-17 20:32:40 UTC dirac-dms-show-se-status.py  INFO: ProductionSandboxSE                Active          Active



Uploading a file to the Grid
-----------------------------------------------

**Adding a file**

- Next step is to register the file into the DIRAC file catalog and copy the file in a Storage Element. Execute the command::

        dirac-dms-add-file <LFN> <FILE> <SE>

  Output must be like this::

        $dirac-dms-add-file LFN:/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt Test-Lyon.orig M3PEC-disk
        2010-10-17 20:31:22 UTC dirac-dms-add-file.py  INFO: ReplicaManager.putAndRegister: Checksum information not provided. Calculating adler32.
        2010-10-17 20:31:22 UTC dirac-dms-add-file.py  INFO: ReplicaManager.putAndRegister: Checksum calculated to be 2ec4058b.
        2010-10-17 20:31:22 UTC dirac-dms-add-file.py  WARN: StorageElement.isValid: The 'operation' argument is not supplied. It should be supplied in the future.
        2010-10-17 20:31:24 UTC dirac-dms-add-file.py  INFO: SRM2Storage.__putFile: Using 1 streams
        2010-10-17 20:31:24 UTC dirac-dms-add-file.py  INFO: SRM2Storage.__putFile: Executing transfer of file:Test-Lyon.orig to srm://se0.m3pec.u-bordeaux1.fr:8446/srm/managerv2?SFN=/dpm/m3pec.u-bordeaux1.fr/home/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt
        2010-10-17 20:31:30 UTC dirac-dms-add-file.py  INFO: SRM2Storage.__putFile: Successfully put file to storage.
        2010-10-17 20:31:31 UTC dirac-dms-add-file.py ERROR: StorageElement.getPfnForProtocol: Requested protocol not available for SE. DIP for M3PEC-disk
        2010-10-17 20:31:32 UTC dirac-dms-add-file.py  INFO: ReplicaManger.putAndRegister: Sending accounting took 0.5 seconds
        {'Failed': {},
         'Successful': {'/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt': {'put': 8.3242118358612061,
                                                                                    'register': 0.51048803329467773}}}

  Note: The output of this command must be successful before continue with the other commands.


Obtaining data information
-----------------------------------------------

Metadata
@@@@@@@@@@

- After a file is registered into DIRAC File Catalog the metadata could be consulted any time with::

        dirac-dms-catalog-metadata <LFN>

  By example metadata for Test-Lyon.txt file is::

        $dirac-dms-catalog-metadata /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt
        FileName                                                                                             Size       GUID                                     Status   Checksum
        /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt                                               15         1D6155B6-0405-BAB0-5552-7913EFD734A7     1        2ec4058b

File Data Size
@@@@@@@@@@@@@@@

- Data size command shows the size in gigabytes of one file.::

        dirac-dms-data-size <LFN>

  By example::

        $dirac-dms-data-size  /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt
        2010-10-17 20:31:37 UTC dirac-dms-data-size.py  INFO: ------------------------------
        2010-10-17 20:31:37 UTC dirac-dms-data-size.py  INFO: Files          |      Size (GB)
        2010-10-17 20:31:37 UTC dirac-dms-data-size.py  INFO: ------------------------------
        2010-10-17 20:31:37 UTC dirac-dms-data-size.py  INFO: 1              |            0.0
        2010-10-17 20:31:37 UTC dirac-dms-data-size.py  INFO: ------------------------------

Copying a file into UI
@@@@@@@@@@@@@@@@@@@@@@

- Retrieve the file previously uploaded into SE using the command::

        dirac-dms-get-file <LFN>

  Output must be like shows below::

        $dirac-dms-get-file /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt
        2010-10-17 20:31:42 UTC dirac-dms-get-file.py  INFO: SRM2Storage.__getFile: Using 1 streams

        {'Failed': {},
         'Successful': {'/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt': '/afs/in2p3.fr/home/h/hamar/Tests/DMS/Test-Lyon.txt'}}


Getting the file URL
@@@@@@@@@@@@@@@@@@@@@

- Try to get the file URL is the information of the file in the Storage Element.::

        dirac-dms-lfn-accessURL <LFN>

  By example::

        dirac-dms-lfn-accessURL /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt M3PEC-disk
        {'Failed': {},
         'Successful': {'/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt': 'rfio://dpm-disk2.m3pec.u-bordeaux1.fr//data4/vo.formation.idgrilles.fr/2010-10-17/Test-Lyon.txt.353184.0'}}


File Metadata
@@@@@@@@@@@@@@

- File metadata returns all the information about the file::

        dirac-dms-lfn-metadata <LFN>

  By example::

        $dirac-dms-lfn-metadata /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt
        2010-10-17 20:32:00 UTC dirac-dms-lfn-metadata.py/DiracAPI  INFO: Metadata Lookup Time: 1.01 seconds
        {'Failed': {},
         'Successful': {'/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt': {'Checksum': '2ec4058b',
                                                                           'ChecksumType': 'Adler32',
                                                                           'CreationDate': datetime.datetime(2010, 10, 17, 20, 31, 31),
                                                                           'CreationDate': datetime.datetime(2010, 10, 17, 20, 31, 31),
                                                                           'FileID': 15L,
                                                                           'GID': 2,
                                                                           'GUID': '1D6155B6-0405-BAB0-5552-7913EFD734A7',
                                                                           'Mode': 509,
                                                                           'ModificationDate': datetime.datetime(2010, 10, 17, 20, 31, 31),
                                                                           'Owner': 'vhamar',
                                                                           'OwnerGroup': 'dirac_user',
                                                                           'Size': 15L,
                                                                           'Status': 1,
                                                                           'UID': 2}}}


Data Replication
-----------------------------------------------

Replicating a file
@@@@@@@@@@@@@@@@@@@

- The command to replicate a existing file in DIRAC file catalog is::

        dirac-dms-lfn-replicas <LFN> <SE>

  One ouput is showed below::

        $dirac-dms-lfn-replicas /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt
        2010-10-17 20:32:03 UTC dirac-dms-lfn-replicas.py/DiracAPI  INFO: Replica Lookup Time: 1.42 seconds
        {'Failed': {},
          'Successful': {'/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt': {'M3PEC-disk': 'srm://se0.m3pec.u-bordeaux1.fr/dpm/m3pec.u-bordeaux1.fr/home/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt'}}}


Replicating Metadata
@@@@@@@@@@@@@@@@@@@@@

- Next step is replicate metadata::

        dirac-dms-replica-metadata <LFN>

  By example::

        dirac-dms-replica-metadata /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt M3PEC-disk
        File                                                                                                 Migrated Cached   Size (bytes)
        /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt                                               0        1        15


Replicating a File
@@@@@@@@@@@@@@@@@@@

- To replicate a file into another SE, the next command must be executed::

        dirac-dms-replicate <LFN> <SE>

  The output must be similar to::

        $dirac-dms-replicate /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt DIRAC-USER
        2010-10-17 20:32:10 UTC dirac-dms-replicate.py  WARN: StorageElement.isValid: The 'operation' argument is not supplied. It should be supplied in the future.
        2010-10-17 20:32:10 UTC dirac-dms-replicate.py  INFO: ReplicaManager.__initializeReplication: Destination Storage Element verified.
        2010-10-17 20:32:10 UTC dirac-dms-replicate.py  INFO: ReplicaManager.__initializeReplication: Successfully obtained replicas for LFN.
        2010-10-17 20:32:11 UTC dirac-dms-replicate.py  INFO: ReplicaManager.__initializeReplication: File size determined to be 15.
        2010-10-17 20:32:11 UTC dirac-dms-replicate.py  INFO: ReplicaManager.__initializeReplication: Destination site not banned.
        2010-10-17 20:32:11 UTC dirac-dms-replicate.py  INFO: ReplicaManager.__initializeReplication: Replication initialization successful.
        2010-10-17 20:32:11 UTC dirac-dms-replicate.py  INFO: ReplicaManager.__resolveBestReplicas: Obtained current banned sources.
        2010-10-17 20:32:11 UTC dirac-dms-replicate.py  INFO: ReplicaManager.__resolveBestReplicas: M3PEC-disk is available for use.
        2010-10-17 20:32:11 UTC dirac-dms-replicate.py  WARN: StorageElement.isValid: The 'operation' argument is not supplied. It should be supplied in the future.
        2010-10-17 20:32:11 UTC dirac-dms-replicate.py ERROR: StorageElement.getPfnForProtocol: Requested protocol not available for SE. DIP for M3PEC-disk
        2010-10-17 20:32:11 UTC dirac-dms-replicate.py  INFO: ReplicaManager.__resolveBestReplicas: Source file size determined to be 15.
        2010-10-17 20:32:13 UTC dirac-dms-replicate.py  INFO: SRM2Storage.__getFile: Using 1 streams
        2010-10-17 20:32:13 UTC dirac-dms-replicate.py  INFO: SRM2Storage.__getFile: Executing transfer of srm://se0.m3pec.u-bordeaux1.fr:8446/srm/managerv2?SFN=/dpm/m3pec.u-bordeaux1.fr/home/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt to file:/afs/in2p3.fr/home/h/hamar/Tests/DMS/Test-Lyon.txt
        2010-10-17 20:32:21 UTC dirac-dms-replicate.py  INFO: ReplicaManager.__replicate: Replication successful.



Removing a replica
@@@@@@@@@@@@@@@@@@@

- To remove a replica from a SE where the file was previously replicated, run the command::

        dirac-dms-remove-replicas <LFN> <SE>

  By example::

        dirac-dms-remove-replicas /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt DIRAC-USER
        2010-10-17 20:32:23 UTC dirac-dms-remove-replicas.py  INFO: ReplicaManager.__removePhysicalReplica: Successfully issued removal request.
        Successfully removed DIRAC-USER replica of /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt

Replicating a Logical File Name
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

- The command used to perform this action::

        dirac-dms-replicate-lfn <LFN> <SE>

  By example::

        $dirac-dms-replicate-lfn /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt DIRAC-USER
        2010-10-17 20:32:27 UTC dirac-dms-replicate-lfn.py  WARN: StorageElement.isValid: The 'operation' argument is not supplied. It should be supplied in the future.
        2010-10-17 20:32:27 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.__initializeReplication: Destination Storage Element verified.
        2010-10-17 20:32:28 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.__initializeReplication: Successfully obtained replicas for LFN.
        2010-10-17 20:32:28 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.__initializeReplication: File size determined to be 15.
        2010-10-17 20:32:28 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.__initializeReplication: Destination site not banned.

        2010-10-17 20:32:28 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.__initializeReplication: Replication initialization successful.
        2010-10-17 20:32:28 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.__resolveBestReplicas: Obtained current banned sources.
        2010-10-17 20:32:28 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.__resolveBestReplicas: M3PEC-disk is available for use.
        2010-10-17 20:32:28 UTC dirac-dms-replicate-lfn.py  WARN: StorageElement.isValid: The 'operation' argument is not supplied. It should be supplied in the future.
        2010-10-17 20:32:28 UTC dirac-dms-replicate-lfn.py ERROR: StorageElement.getPfnForProtocol: Requested protocol not available for SE. DIP for M3PEC-disk
        2010-10-17 20:32:28 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.__resolveBestReplicas: Source file size determined to be 15.
        2010-10-17 20:32:31 UTC dirac-dms-replicate-lfn.py  INFO: SRM2Storage.__getFile: Using 1 streams
        2010-10-17 20:32:31 UTC dirac-dms-replicate-lfn.py  INFO: SRM2Storage.__getFile: Executing transfer of srm://se0.m3pec.u-bordeaux1.fr:8446/srm/managerv2?SFN=/dpm/m3pec.u-bordeaux1.fr/home/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt to file:/afs/in2p3.fr/home/h/hamar/Tests/DMS/Test-Lyon.txt
        2010-10-17 20:32:38 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.__replicate: Replication successful.
        2010-10-17 20:32:38 UTC dirac-dms-replicate-lfn.py  WARN: StorageElement.isValid: The 'operation' argument is not supplied. It should be supplied in the future.
        2010-10-17 20:32:38 UTC dirac-dms-replicate-lfn.py ERROR: StorageElement.getPfnForProtocol: Requested protocol not available for SE. SRM2 for DIRAC-USER
        2010-10-17 20:32:39 UTC dirac-dms-replicate-lfn.py  INFO: ReplicaManager.replicateAndRegister: Successfully registered replica.
        {'Failed': {},
         'Successful': {'/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt': {'register': 0.50833415985107422,
                                                                                   'replicate': 11.878520965576172}}}

My files
@@@@@@@@@@@@

- To know all the files stored by the user::

        dirac-dms-user-lfns

  After run the command a file with all the LFN must be in the directory, the file name is asociated with the user's VO::

        $dirac-dms-user-lfns
        2010-10-17 20:32:43 UTC dirac-dms-user-lfns.py  INFO: Will search for files in /vo.formation.idgrilles.fr/user/v/vhamar
        {'OK': True, 'Value': {'Successful': {'/vo.formation.idgrilles.fr/user/v/vhamar': {'Files': {'/vo.formation.idgrilles.fr/user/v/vhamar/test.txt': {'MetaData': {'Status': 1, 'Size': 34L, 'ChecksumType': 'Adler32', 'Checksum': 'cc500ba0', 'UID': 2, 'OwnerGroup': 'dirac_admin', 'Owner': 'vhamar', 'GID': 1, 'Mode': 509, 'ModificationDate': datetime.datetime(2010, 10, 17, 17, 15, 14), 'CreationDate': datetime.datetime(2010, 10, 17, 17, 15, 14), 'Type': 'File', 'FileID': 14L}}, '/vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt': {'MetaData': {'Status': 1, 'Size': 15L, 'ChecksumType': 'Adler32', 'Checksum': '2ec4058b', 'UID': 2, 'OwnerGroup': 'dirac_user', 'Owner': 'vhamar', 'GID': 2, 'Mode': 509, 'ModificationDate': datetime.datetime(2010, 10, 17, 20, 31, 31), 'CreationDate': datetime.datetime(2010, 10, 17, 20, 31, 31), 'Type': 'File', 'FileID': 15L}}}, 'SubDirs': {}, 'Links': {}}}, 'Failed': {}}}
        2010-10-17 20:32:44 UTC dirac-dms-user-lfns.py  INFO: /vo.formation.idgrilles.fr/user/v/vhamar: 2 files, 0 sub-directories
        2010-10-17 20:32:44 UTC dirac-dms-user-lfns.py  INFO: 2 matched files have been put in vo.formation.idgrilles.fr-user-v-vhamar.lfns


Removing replicas
@@@@@@@@@@@@@@@@@@

- To remove replicas use the command::

        dirac-dms-remove-replicas <LFN> <SE>

 By example::
        $dirac-dms-remove-replicas /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt DIRAC-USER
        2010-10-17 20:32:48 UTC dirac-dms-remove-replicas.py  INFO: ReplicaManager.__removePhysicalReplica: Successfully issued removal request.
        2010-10-17 20:32:49 UTC dirac-dms-remove-replicas.py  INFO: ReplicaManager.__removeCatalogReplica: Removed 1 replicas
        Successfully removed DIRAC-USER replica of /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt

**Removing Files**

- Please remove all the files created during the T.P, using this command::

        dirac-dms-remove-files <LFN>

 By example::

        $dirac-dms-remove-files  /vo.formation.idgrilles.fr/user/v/vhamar/Test-Lyon.txt
        2010-10-17 20:32:52 UTC dirac-dms-remove-files.py  INFO: ReplicaManager.__removePhysicalReplica: Successfully issued removal request.
        2010-10-17 20:32:53 UTC dirac-dms-remove-files.py  INFO: ReplicaManager.__removeCatalogReplica: Removed 1 replicas
        2010-10-17 20:32:54 UTC dirac-dms-remove-files.py  INFO: Successfully removed 1 files