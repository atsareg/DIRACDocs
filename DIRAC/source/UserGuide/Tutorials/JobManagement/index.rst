==================================
Worload Management
==================================


Requirements
----------------------------------

- Have access to a DIRAC Client installation
- Have a valid proxy


APIs
----------------------------------

Submitting Jobs using APIs
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@


- First step, create an API specifying job requirements.

  testAPI.py::

        from DIRAC.Interfaces.API.Dirac import Dirac
        from DIRAC.Interfaces.API.Job import Job

        j = Job()
        j.setCPUTime(500)
        j.setExecutable('/bin/echo hello')
        j.setExecutable('/bin/hostname')
        j.setExecutable('/bin/echo hello again')
        j.setName('API')

        dirac = Dirac()
        jobID = dirac.submit(j)
        print 'Submission Result: ',jobID

- Send the Job using the script::

        python testAPI.py

        $ python testAPI-Submission.py
        2010-10-20 12:05:49 UTC testAPI-Submission.py/DiracAPI  INFO: <=====DIRAC v5r10-pre2=====>
        2010-10-20 12:05:49 UTC testAPI-Submission.py/DiracAPI  INFO: Will submit job to WMS
        {'OK': True, 'Value': 196}



Retrieving Job Status
@@@@@@@@@@@@@@@@@@@@@@

- Create script Status-API.py::

        from DIRAC.Interfaces.API.Dirac import Dirac
        from DIRAC.Interfaces.API.Job import Job
        import sys
        dirac = Dirac()
        jobid = sys.argv[1]
        print dirac.status(jobid)


- Execute script::

        python Status-API.py <Job_ID>

        $python Status-API.py 196
        {'OK': True, 'Value': {196: {'Status': 'Done', 'MinorStatus': 'Execution Complete', 'Site': 'LCG.IRES.fr'}}}



Retrieving Job Output
@@@@@@@@@@@@@@@@@@@@@

- Example Output-API.py::

        from DIRAC.Interfaces.API.Dirac import Dirac
        from DIRAC.Interfaces.API.Job import Job
        import sys
        dirac = Dirac()
        jobid     = sys.argv[1]
        print dirac.getOutput(jobid)

- Execute script::

        python Output-API.py <Job_ID>

        $python Output-API.py 196

Local submission mode
@@@@@@@@@@@@@@@@@@@@@@

- Load python shell::

        bash-3.2$ python
        Python 2.5.5 (r255:77872, Mar 25 2010, 14:17:52)
        [GCC 4.1.2 20080704 (Red Hat 4.1.2-46)] on linux2
        Type "help", "copyright", "credits" or "license" for more information.
        >>> from DIRAC.Interfaces.API.Dirac import Dirac
        >>> from DIRAC.Interfaces.API.Job import Job
        >>> j = Job()
        >>> j.setExecutable('/bin/echo hello')
        {'OK': True, 'Value': ''}
        >>> Dirac().submit(j,mode='local')
        2010-10-22 14:41:51 UTC /DiracAPI  INFO: <=====DIRAC v5r10-pre2=====>
        2010-10-22 14:41:51 UTC /DiracAPI  INFO: Executing workflow locally without WMS submission
        2010-10-22 14:41:51 UTC /DiracAPI  INFO: Executing at /afs/in2p3.fr/home/h/hamar/Tests/APIs/Local/Local_zbDHRe_JobDir
        2010-10-22 14:41:51 UTC /DiracAPI  INFO: Preparing environment for site DIRAC.Client.fr to execute job
        2010-10-22 14:41:51 UTC /DiracAPI  INFO: Attempting to submit job to local site: DIRAC.Client.fr
        2010-10-22 14:41:51 UTC /DiracAPI  INFO: Executing: /afs/in2p3.fr/home/h/hamar/DIRAC5/scripts/dirac-jobexec jobDescription.xml -o LogLevel=info
        Executing StepInstance RunScriptStep1 of type ScriptStep1 ['ScriptStep1']
        StepInstance creating module instance  ScriptStep1  of type Script
        2010-10-22 14:41:53 UTC dirac-jobexec.py/Script  INFO: Script Module Instance Name: CodeSegment
        2010-10-22 14:41:53 UTC dirac-jobexec.py/Script  INFO: Command is: /bin/echo hello
        2010-10-22 14:41:53 UTC dirac-jobexec.py/Script  INFO: /bin/echo hello execution completed with status 0
        2010-10-22 14:41:53 UTC dirac-jobexec.py/Script  INFO: Output written to Script1_CodeOutput.log, execution complete.
        2010-10-22 14:41:53 UTC /DiracAPI  INFO: Standard output written to std.out
        {'OK': True, 'Value': 'Execution completed successfully'}

- Exit python shell

- List directory where you load the python shell, outputs must be automatically downloaded::

        bash-3.2$ ls
        Local_zbDHRe_JobDir  Script1_CodeOutput.log  std.err  std.out
        bash-3.2$ more Script1_CodeOutput.log
        <<<<<<<<<< /bin/echo hello Standard Output >>>>>>>>>>

        hello


Sending Multiple Jobs
@@@@@@@@@@@@@@@@@@@@@@

- Create a Test-API-Multiple.py script, by example::

        from DIRAC.Interfaces.API.Dirac import Dirac
        from DIRAC.Interfaces.API.Job import Job

        j = Job()
        j.setCPUTime(500)
        j.setExecutable('/bin/echo hello')
        j.setExecutable('/bin/hostname')
        j.setExecutable('/bin/echo hello again')
        for i in range(20):
          j.setName('API_%d' % i)
          dirac = Dirac()
          jobID = dirac.submit(j)
          print 'Submission Result: ',jobID

- Execute the API::

          python Test-API-Multiple.py

          $ python Test-API-Multiple.py
          2010-10-20 10:25:02 UTC Test-API-Multiple.py/DiracAPI  INFO: <=====DIRAC v5r10-pre2=====>
          2010-10-20 10:25:02 UTC Test-API-Multiple.py/DiracAPI  INFO: Will submit job to WMS
          Submission Result:  {'OK': True, 'Value': 176}
          2010-10-20 10:25:04 UTC Test-API-Multiple.py/DiracAPI  INFO: <=====DIRAC v5r10-pre2=====>
          2010-10-20 10:25:04 UTC Test-API-Multiple.py/DiracAPI  INFO: Will submit job to WMS
          Submission Result:  {'OK': True, 'Value': 177}
          2010-10-20 10:25:06 UTC Test-API-Multiple.py/DiracAPI  INFO: <=====DIRAC v5r10-pre2=====>
          2010-10-20 10:25:06 UTC Test-API-Multiple.py/DiracAPI  INFO: Will submit job to WMS
          Submission Result:  {'OK': True, 'Value': 178}



Using APIs to create JDL files.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

- Create a Test_API-JDL.py::

          from DIRAC.Interfaces.API.Job import Job
          j = Job()
          j.setName('APItoJDL')
          j.setOutputSandbox(['*.log','summary.data'])
          j.setInputData(['/vo.formation.idgrilles.fr/user/v/vhamar/test.txt','/vo.formation.idgrilles.fr/user/v/vhamar/test2.txt'])
          j.setOutputData(['/vo.formation.idgrilles.fr/user/v/vhamar/output1.data','/vo.formation.idgrilles.fr/user/v/vhamar/output2.data'],OutputPath='MyFirstAnalysis')
          j.setSystemConfig("")
          j.setCPUTime(21600)
          j.setDestination('LCG.IN2P3.fr')
          j.setBannedSites(['LCG.ABCD.fr','LCG.EFGH.fr'])
          j.setLogLevel('DEBUG')
          j.setExecutionEnv({'MYVARIABLE':'TEST'})
          j.setExecutable('echo $MYVARIABLE')
          print j._toJDL()

- Run the API::

          $ python test_API-JDL.py

              Origin = "DIRAC";
              Priority = "1";
              Executable = "$DIRACROOT/scripts/dirac-jobexec";
              ExecutionEnvironment = "MYVARIABLE=TEST";
              StdError = "std.err";
              LogLevel = "DEBUG";
              BannedSites =
                  {
                      "LCG.ABCD.fr",
                      "LCG.EFGH.fr"
                  };
              StdOutput = "std.out";
              Site = "LCG.IN2P3.fr";
              SystemConfig = "";
              OutputPath = "MyFirstAnalysis";
              InputSandbox = "jobDescription.xml";
              Arguments = "jobDescription.xml -o LogLevel=DEBUG";
              JobGroup = "vo.formation.idgrilles.fr";
              OutputSandbox =
                  {
                      "*.log",
                      "summary.data",
                      "Script1_CodeOutput.log",
                      "std.err",
                      "std.out"
                  };
              MaxCPUTime = "21600";
              JobName = "APItoJDL";
              InputData =
                  {
                      "LFN:/vo.formation.idgrilles.fr/user/v/vhamar/test.txt",
                      "LFN:/vo.formation.idgrilles.fr/user/v/vhamar/test2.txt"
                  };
              JobType = "User";
              OutputData =
                  {
                      "/vo.formation.idgrilles.fr/user/v/vhamar/output1.data",
                      "/vo.formation.idgrilles.fr/user/v/vhamar/output2.data"
                  }



As you can see the parameters added to the job object are represented in the JDL.


JDLs
------------------------------------

Simple Jobs
@@@@@@@@@@@@

- testJob.jdl::

        Executable = "/bin/ls";
        StdOutput = "StdOut";
        StdError = "StdErr";
        OutputSandbox = {"StdOut","StdErr"};


Jobs with Input Sandbox
@@@@@@@@@@@@@@@@@@@@@@@@

Some cases job input data or executables can be stored locally but is necessary to transfer it to the grid to run the jobs, in this case InputSandbox is the attribute that can be used to move the files to the grid.

- Modify your testJob.jdl::

        Executable = "testJob.sh";
        StdOutput = "StdOut";
        StdError = "StdErr";
        InputSandbox = {"testJob.sh"};
        OutputSandbox = {"StdOut","StdErr"};

- And create a simple shell script.

  testJob.sh::

        #!/bin/bash
        /bin/hostname


Input Sandbox from SE
@@@@@@@@@@@@@@@@@@@@@@

In case than the data, programs, etc are stored in a Storage Element it can be part of InputSandbox. InputSandbox can be declared as a list, separated by colons and each file between "".

- Use the following JDL::

        Executable = "testJob.sh";
        StdOutput = "StdOut";
        StdError = "StdErr";
        InputSandbox = {"testJob.sh","LFN:/vo.formation.idgrilles.fr/user/v/vhamar/test.txt"};
        OutputSandbox = {"StdOut","StdErr"};


- Copy the file to the Storage Element::

        bash-3.2$ dirac-dms-add-file LFN:/vo.formation.idgrilles.fr/user/v/vhamar/test.txt test.txt M3PEC-disk
        2010-10-17 17:15:04 UTC dirac-dms-add-file.py  WARN: ReplicaManager.__getClientCertGroup: Proxy information does not contain the VOMs information.
        2010-10-17 17:15:05 UTC dirac-dms-add-file.py  INFO: ReplicaManager.putAndRegister: Checksum information not provided. Calculating adler32.
        2010-10-17 17:15:05 UTC dirac-dms-add-file.py  INFO: ReplicaManager.putAndRegister: Checksum calculated to be cc500ba0.
        2010-10-17 17:15:06 UTC dirac-dms-add-file.py  WARN: StorageElement.isValid: The 'operation' argument is not supplied. It should be supplied in the future.
        2010-10-17 17:15:06 UTC dirac-dms-add-file.py  INFO: SRM2Storage.__putFile: Using 1 streams
        2010-10-17 17:15:06 UTC dirac-dms-add-file.py  INFO: SRM2Storage.__putFile: Executing transfer of file:test.txt to srm://se0.m3pec.u-bordeaux1.fr:8446/srm/managerv2?SFN=/dpm/m3pec.u-bordeaux1.fr/home/vo.formation.idgrilles.fr/user/v/vhamar/test.txt
        2010-10-17 17:15:13 UTC dirac-dms-add-file.py  INFO: SRM2Storage.__putFile: Successfully put file to storage.
        2010-10-17 17:15:13 UTC dirac-dms-add-file.py ERROR: StorageElement.getPfnForProtocol: Requested protocol not available for SE. DIP for M3PEC-disk
        2010-10-17 17:15:14 UTC dirac-dms-add-file.py  INFO: ReplicaManger.putAndRegister: Sending accounting took 0.5 seconds
        {'Failed': {},
         'Successful': {'/vo.formation.idgrilles.fr/user/v/vhamar/test.txt': {'put': 7.5088520050048828,
                                                                      'register': 0.40918898582458496}}}

Jobs and Data Output
@@@@@@@@@@@@@@@@@@@@@

- In case the output files needs to be stored into a Storage Element, its necesary to add OutputSE and OutputData options::

        Executable = "testJob.sh";
        StdOutput = "StdOut";
        StdError = "StdErr";
        InputSandbox = {"testJob.sh"};
        OutputSandbox = {"StdOut","StdErr"};
        OutputSE = "M3PEC-disk";
        OutputData = {"StdOut"};


Managing Jobs
-------------------------------------------

Submitting a Job
@@@@@@@@@@@@@@@@@

- After creation of JDL file the next step is submit a job, using the command:::

        dirac-wms-job-submit

  By example::

        bash-3.2$ dirac-wms-job-submit Simple.jdl
        2010-10-17 15:34:36 UTC dirac-wms-job-submit.py/DiracAPI  INFO: <=====DIRAC v5r10-pre2=====>
        2010-10-17 15:34:36 UTC dirac-wms-job-submit.py/DiracAPI  INFO: Will submit job to WMS
        JobID = 11


Retrieving Job Status
@@@@@@@@@@@@@@@@@@@@@@

- The next step is to know which is the job status using the command::

        dirac-wms-job-status <Job_ID>

        bash-3.2$ dirac-wms-job-status 11
        JobID=11 Status=Waiting; MinorStatus=Pilot Agent Submission; Site=ANY;

Retrieving Job Output
@@@@@@@@@@@@@@@@@@@@@@@

- And finally, after job status is done retrieve job outputs:::

        dirac-wms-job-output <Job_ID>


Killing a Job
@@@@@@@@@@@@@@

- Submit a new job and cancel it using the command show below::

        dirac-wms-job-kill <Job_ID>

        bash-3.2$ dirac-wms-job-kill 12
        Killed job 12