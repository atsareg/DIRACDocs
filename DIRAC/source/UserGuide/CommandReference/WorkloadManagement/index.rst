
============================
Workload Management
============================


Before run those commands, please load the DIRAC environment, the steps are explained at: [http://mareela.in2p3.fr:9200/dirac/wiki/DiracEnvironmentUsers]

----

Command : **dirac-wms-job-attributes**

Used to  : Get job attributes

Options : *dirac-wms-job-attributes.py <JobID> |[<JobID>]*

Example : ::

   (DIRAC3-user)$ dirac-wms-job-attributes 228
   {'AccountedFlag': 'False',
   'ApplicationNumStatus': '0',
   'ApplicationStatus': 'Unknown',
   'CPUTime': '0.0',
   'DIRACSetup': 'EELA-Production',
   'DeletedFlag': 'False',
   'EndExecTime': '2009-09-03 10:22:12',
   'FailedFlag': 'False',
   'HeartBeatTime': '2009-09-03 10:22:12',
   'ISandboxReadyFlag': 'True',
   'JobGroup': 'NoGroup',
   'JobID': '228',
   'JobName': 'Unknown',
   'JobSplitType': 'Single',
   'JobType': 'MPI',
   'KilledFlag': 'False',
   'LastUpdateTime': '2009-09-03 10:22:23',
   'MasterJobID': '0',
   'MinorStatus': 'Execution Complete',
   'OSandboxReadyFlag': 'False',
   'Owner': 'hamar',
   'OwnerDN': '/O=GRID-FR/C=VE/O=ULA/OU=CECALCULA/CN=Vanessa Hamar',
   'OwnerGroup': 'eela_user',
   'RescheduleCounter': '0',
   'RescheduleTime': 'None',
   'RetrievedFlag': 'True',
   'Site': 'EELA.UFRJ.br',
   'StartExecTime': 'None',
   'Status': 'Done',
   'SubmissionTime': '2009-09-03 10:20:23',
   'SystemPriority': '0',
   'UserPriority': '1',
   'VerifiedFlag': 'True'}


----

Command : **dirac-wms-job-get-input**

Used to  : Get input data from a job.

Options : *dirac-wms-job-get-input.py <JobID> |[<JobID>]*

Example : ::

   (DIRAC3-user)$ dirac-wms-job-get-input 228
   2009-09-08 12:15:30 UTC dirac-wms-job-get-input.py/DiracAPI  INFO: Files retrieved and extracted in /lhcb/users/hamar/InputSandbox228
   Job input sandbox retrieved in InputSandbox228/

----

Command : **dirac-wms-job-get-jdl**

Used to  : Get jdl from a job.

Options : *dirac-wms-job-get-jdl.py <JobID> |[<JobID>]*

Example : ::

   dirac-wms-job-get-jdl 228
   {'Arguments': 'mpi.sh',
   'CPUNumber': '2',
   'CPUTime': '86400',
   'DIRACSetup': 'EELA-Production',
   'Executable': '/bin/sh',
   'InputSandbox': ['mpi.sh', 'mpi-test.c', 'mpi-test.o'],
   'JobID': '228',
   'JobRequirements': ['[',
                     'OwnerDN',
                     '=',
                     '/O=GRID-FR/C=VE/O=ULA/OU=CECALCULA/CN=Vanessa',
                     'Hamar;',
                     'Setup',
                     '=',
                     'EELA-Production;',
                     'CPUTime',
                     '=',
                     '0;',
                     'OwnerGroup',
                     '=',
                     'eela_user;',
                     'UserPriority',
                     '=',
                     '1;',
                     'JobTypes',
                     '=',
                     'MPI',
                     ']'],
   'JobType': 'MPI',
   'MyProxyServer': 'px.eela.ufrj.br',
   'OutputSandbox': ['mpi-test.o', 'mpi-test.err', 'mpi-test.out'],
   'Owner': 'hamar',
   'OwnerDN': '/O=GRID-FR/C=VE/O=ULA/OU=CECALCULA/CN=Vanessa Hamar',
   'OwnerGroup': 'eela_user',
   'OwnerName': 'hamar',
   'Priority': '1',
   'StdError': 'mpi-test.err',
   'StdOutput': 'mpi-test.out',
   'UseMyProxy': 'False'}

----

Command : **dirac-wms-job-get-output**

Used to  : Get job output files into local machine.

Options : *dirac-wms-job-get-output.py <JobID> |[<JobID>]*

Example : ::

   (DIRAC3-user)$ dirac-wms-job-get-output 228
   2009-09-08 12:22:40 UTC dirac-wms-job-get-output.py/DiracAPI  INFO: Files retrieved and extracted in /lhcb/users/hamar/228
   Job output sandbox retrieved in 228/

----

Command : **dirac-wms-job-get-output-data**

Used to  : Get job output data.

Options : *dirac-wms-job-get-output-data.py <JobID> |[<JobID>]*

Example:::

   (DIRAC3-user)$dirac-wms-job-get-output-data 228

----

Command : **dirac-wms-job-kill**

Used to  : Kill a job.

Options :*dirac-wms-job-kill.py <JobID> |[<JobID>]*

Example : ::

   (DIRAC3-user)$ dirac-wms-job-kill 202
   Killed job 202

----

Command : **dirac-wms-job-logging-info**

Used to  : Get job logging info.

Options :*dirac-wms-job-logging-info.py <JobID> |[<JobID>]*

Example : ::

   (DIRAC3-user)$ dirac-wms-job-logging-info 228
   Status                        MinorStatus                   ApplicationStatus             DateTime
   Received                      Job accepted                  Unknown                       2009-09-03 10:20:23
   Received                      False                         Unknown                       2009-09-03 10:20:38
   Checking                      JobSanity                     Unknown                       2009-09-03 10:20:38
   Checking                      JobScheduling                 Unknown                       2009-09-03 10:20:38
   Waiting                       Pilot Agent Submission        Unknown                       2009-09-03 10:20:38
   Matched                       Assigned                      Unknown                       2009-09-03 10:21:22
   Matched                       Job Received by Agent         Unknown                       2009-09-03 10:21:42
   Matched                       Submitted To CE               Unknown                       2009-09-03 10:21:50
   Running                       Job Initialization            Unknown                       2009-09-03 10:21:52
   Running                       Downloading InputSandbox      Unknown                       2009-09-03 10:21:56
   Running                       Application                   Unknown                       2009-09-03 10:22:04
   Completed                     Application Finished SuccessfullyUnknown                    2009-09-03 10:22:12
   Completed                     Uploading Output Sandbox      Unknown                       2009-09-03 10:22:14
   Completed                     Output Sandbox Uploaded       Unknown                       2009-09-03 10:22:19
   Done                          Execution Complete            Unknown                       2009-09-03 10:22:19

----

Command : **dirac-wms-job-peek**

Used to  : Get job peek info.

Options : *dirac-wms-job-peek.py <JobID> |[<JobID>]*

Example : ::

   dirac-wms-job-peek 228
   2009-09-08 12:43:28 UTC dirac-wms-job-peek.py/DiracAPI  INFO: ==================================================================================
   2009-09-08 12:43:28 UTC dirac-wms-job-peek.py/DiracAPI  INFO: Last 8 lines of application output from JobWrapper on 2009-09-03   10:22:10.959989 :
   2009-09-08 12:43:28 UTC dirac-wms-job-peek.py/DiracAPI  INFO: CPU Total for job is 00:00:00 (h:m:s)
   2009-09-08 12:43:28 UTC dirac-wms-job-peek.py/DiracAPI  INFO: ==================================================================================
   2009-09-08 12:43:28 UTC dirac-wms-job-peek.py/DiracAPI  INFO: --------------------------------------
   2009-09-08 12:43:28 UTC dirac-wms-job-peek.py/DiracAPI  INFO: Hello world! from processor 1 out of 2
   2009-09-08 12:43:28 UTC dirac-wms-job-peek.py/DiracAPI  INFO: Hello world! from processor 0 out of 2


----

Command : **dirac-wms-job-reschedule**

Used to  : Reschedule a job.

Options : *dirac-wms-job-reschedule.py <JobID> |[<JobID>]*

 Example:::

   (DIRAC3-user)$ dirac-wms-job-reschedule 228
   Rescheduled job 228


----

Command : **dirac-wms-jobs-select-output-search**

Used to  : Get job output.

Options : *dirac-wms-jobs-select-output-search.py string to search*

Example : ::

   (DIRAC3-user)$ dirac-wms-jobs-select-output-search MPI
   2009-09-08 12:47:44 UTC dirac-wms-jobs-select-output-search.py/DiracAPI  INFO: Files retrieved and extracted in /lhcb/users/hamar/201
   2009-09-08 12:47:45 UTC dirac-wms-jobs-select-output-search.py/DiracAPI  INFO: Files retrieved and extracted in /lhcb/users/hamar/202

----

Command : **dirac-wms-job-status**

Used to  : Get job status.

Options : *dirac-wms-job-status.py <JobID> |[<JobID>]*

Example : ::

   (DIRAC3-user)$ dirac-wms-job-status 228
   228 {228: {'Status': 'Waiting', 'MinorStatus': 'Pilot Agent Submission', 'Site': 'ANY'}}

----

Command : **dirac-wms-job-submit**

Used to  : Submit a job.

Options : *dirac-wms-job-submit.py <Path to JDL file> |[<Path to JDL file>]*

Example : ::

   (DIRAC3-user)$ dirac-wms-job-submit testjob.jdl
   sendFiles: sizeLimit = 0
   JobID = 231

----

Command : **dirac-wms-job-delete**

Used to  : Delete a job.

Options : *dirac-wms-job-delete.py <JobID> |[<JobID>]*

Example : ::

   (DIRAC3-user)$ dirac-wms-job-delete 201
   Deleted job 201


----

Command : **dirac-wms-job-parameters**

Used to  : Get job parameters.

Options : *dirac-wms-job-parameters.py <JobID> |[<JobID>]*

Example : ::

    (DIRAC3-user)$ dirac-wms-job-parameters 228
    {'AgentCompatiblePlatforms': 'slc4_ia32_gcc34',
     'AgentLocalSE': 'None',
     'CPUScalingFactor': '0.0',
     'JobPath': 'JobPath,JobSanity,JobScheduling,TaskQueue',
     'JobSanityCheck': 'Job: 228 JDL: OK,InputData: No input LFNs,  Input Sandboxes: 0, OK.',
     'LocalBatchID': 'dc228',
     'MatcherServiceTime': '3.09944152832e-06',
     'NormCPUTime(s)': '0.0',
     'OutputSandboxMissingFiles': 'mpi-test.err',
     'PilotAgent': 'DIRAC version v4r16 build 3',
     'TotalCPUTime(s)': '0.52'}

----

Command : **dirac-wms-select-jobs**

Used to  : Select jobs

Options : ::

 -o:  --option=  :  Option=value to add

 -s:  --section=  :  Set base section for relative parsed options

 -c:  --cert=  :  Use server certificate to connect to Core Services

 -h  --help  :  Shows this help

 --Status=  :  Primary status

 --MinorStatus=  :  Secondary status

 --ApplicationStatus=  :  Application status

 --Site=  :  Execution site

 --Owner=  :  Owner (DIRAC nickname)

 --JobGroup=  :  Select jobs for specified job group

 --Date=  :  Date in YYYY-MM-DD format, if not specified default is today


Example  :  ::

   (DIRAC3-user)$ dirac-wms-select-jobs
   ==> Selected 6 jobs with conditions: Date = Today
   201, 202, 228, 229, 230, 231