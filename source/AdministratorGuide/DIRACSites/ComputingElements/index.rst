=================================
DIRAC Computing Elements
=================================

Direct access to the site computing clusters is done by sending pilot jobs in a similar way as 
it is done for the grid sites. The pilot jobs are sent by a specialized agent called *SiteDirector*.

The *SiteDirector* is usually serving one or several sites and can run as part of the central service
installation or as an on-site component. At the initialization phase it gets description of the site's 
capacity and then runs in a loop performing the following operations:

- Check if there are tasks in the DIRAC TaskQueue eligible for running on the site;
- If there are tasks to run, check the site current occupancy in terms of numbers of already running
  or waiting pilot jobs;
- If there is a spare capacity on the site, submit a number of pilot jobs corresponding to the
  number of user jobs in the TaskQueue and the number of slots in the site computing cluster;
- Monitor the status of submitted pilot jobs, update the PilotAgentsDB accordingly;
- Retrieve the standard output/error of the pilot jobs.  

*SiteDirector* is submitting pilot jobs with credentials of a user entitled to run *generic* pilots
for the given user community. The *generic* pilots are called so as they are capable of executing
jobs on behalf of community users.

SiteDirector Configuration
----------------------------

The *SiteDirector* configuration is defined in the standard way as for any DIRAC agent. It belongs
to the WorkloadManagement System and its configuration section is:

   /Systems/WorkloadManagement/<instance>/Agents/SiteDirector
   
The following parameters are defined in the *SiteDirector* configuration with some example values::

  SiteDirector
  {
    # agent cycle length in seconds
    PollingTime = 60
    
    # one or more site names served by the agent
    Site = LCG.CNAF.it, LCG.NIKHEF.it
    
    # one or more CE names served by the agent. All CEs defined for the sites by default
    CEs = torqsrv.ihep.ac.cn
    
    # Types of the CEs served by the agent
    CETypes = SSHTorque
    
    # The DN of the user with whose credentials the pilots are submitted
    GenericPilotDN = /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Andrei Tsaregorodtsev
    
    # The group of the user with whose credentials the pilots are submitted
    GenericPilotGroup = lhcb_pilot
    
    # The agent working directory
    WorkDirectory = /opt/dirac/work/SiteDirector/SSHTorque 
    
    # Flag to perform pilot status updates
    UpdatePilotStatus = True
    
    # Flag to perform pilot output retrieval
    GetPilotOutput = True 
    
    # Flag to send pilot info to the Accounting service
    SendPilotAccounting = True
  }      
  
Computing Elements
-------------------

DIRAC can use different computing resources via specialized clients called *ComputingElements*. 
Each computing resource is accessed using an appropriate *ComputingElement* class derived from a common
base class. 

The *ComputingElements* should be properly described to be useful. The configuration
of the *ComputingElement* is located in the inside the corresponding site section in the
/Resources section. An example of a site description is given below::

  Resources
  {
    Sites
    {
      # Site administrative domain
      LCG
      {
        # Site section
        LCG.CNAF.it
        {
          # Site name
          Name = CNAF
          
          # List of valid CEs on the site
          CE = ce01.infn.it, ce02.infn.it
          
          # Section describing each CE
          CEs
          {
            # Specific CE description section
            ce01.infn.it
            {
              # Type of the CE
              CEType = CREAM
              
              # Submission mode should be "direct" in order to work with SiteDirector
              # Otherwise the CE will be eligible for the use with third party broker, e.g.
              # gLite WMS
              SubmissionMode = direct
              
              # Section to describe various queue in the CE
              Queues
              {
                long
                {
                  ...
                }
              }
            }
          }
        }
      }
    }
  }


This is the general structure in which specific CE descriptions are inserted. The CE configuration
is part of the general DIRAC configuration. It can be placed in the general Configuration Service
or in the local configuration of the DIRAC installation. Some CE parameters are confidential, e.g.
password of the account used for the SSH tunnel access to a site. The confidential parameters
should be stored in the local configuration in protected files. 

The *SiteDirector* is getting the CE descriptions from the configuration and uses them according
to their specified capabilities and preferences. Configuration options specific for different types
of CEs are describe in the subsections below

CREAM Computing Element
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

A commented example follows::

   # Section placed in the */Resources/Sites/<domain>/<site>/CEs* directory
   ce01.infn.it  
   {
     CEType = CREAM
     SubmissionMode = direct
     
     
     Queues
     {
       # The queue section name should be the same as in the BDII description
       long
       {
         # Max CPU time in HEP'06 unit secs
         CPUTime = 10000
         # Max total number of jobs in the queue
         MaxTotalJobs = 5
         # Max number of waiting jobs in the queue
         MaxWaitingJobs = 2
       }
     }
   }

Torque Computing Element
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

A commented example follows::

   # Section placed in the */Resources/Sites/<domain>/<site>/CEs* directory
   ce01.infn.it  
   {
     CEType = Torque
     SubmissionMode = direct
     
     
     Queues
     {
       # The queue section name should be the same as the name of the actual batch queue
       long
       {
         # Max CPU time in HEP'06 unit secs
         CPUTime = 10000
         # Max total number of jobs in the queue
         MaxTotalJobs = 5
         # Max number of waitin jobs in the queue
         MaxWaitingJobs = 2
         # Flag to include pilot proxy in the payload sent to the batch system
         BundleProxy = True
         # Directory on the CE site where the pilot standard output stream will be stored
         BatchOutput = /home/dirac_ssh/localsite/output
         # Directory on the CE site where the pilot standard output stream will be stored
         BatchError = /home/dirac_ssh/localsite/error
         # Directory where the payload executable will be stored temporarily before
         # submission to the batch system
         ExecutableArea = /home/dirac_ssh/localsite/submission
         # Flag to remove the pilot output after it was retrieved
         RemoveOutput = True
       }
     }
   }   
   
SSHTorque Computing Element
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

A commented example follows::

   # Section placed in the */Resources/Sites/<domain>/<site>/CEs* directory
   ce01.infn.it  
   {
     CEType = SSHTorque
     SubmissionMode = direct
     
     # Parameters of the SSH conection to the site
     SSHHost = lphelc1.epfl.ch
     SSHUser = dirac_ssh
     # if SSH password is no given, the public key connection is assumed
     SSHPassword = XXXXXXXXX
     
     Queues
     {
       # The queue section name should be the same as the name of the actual batch queue
       long
       {
         # Max CPU time in HEP'06 unit secs
         CPUTime = 10000
         # Max total number of jobs in the queue
         MaxTotalJobs = 5
         # Max number of waitin jobs in the queue
         MaxWaitingJobs = 2
         # Flag to include pilot proxy in the payload sent to the batch system
         BundleProxy = True
         # Directory on the CE site where the pilot standard output stream will be stored
         BatchOutput = /home/dirac_ssh/localsite/output
         # Directory on the CE site where the pilot standard output stream will be stored
         BatchError = /home/dirac_ssh/localsite/error
         # Directory where the payload executable will be stored temporarily before
         # submission to the batch system
         ExecutableArea = /home/dirac_ssh/localsite/submission
         # Extra options to be passed to the qsub job submission command
         SubmitOptions = 
         # Flag to remove the pilot output after it was retrieved
         RemoveOutput = True
       }
     }
   }      