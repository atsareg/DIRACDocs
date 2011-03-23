======================================
Developing Commands
======================================

Commands are one of the main interface for users. Commands are called "scripts" in DIRAC lingo. 

Where to place them
______________________

All scripts should live in the scripts directory of their parent system. For instance 

  dirac-wms-job-submit

will live in 

  DIRAC/WorkloadManagementSystem/scripts/dirac-wms-job-submit.py

When DIRAC is installed, all scripts will be placed in the scripts directory and stripped of the extension. This way users can see all the scripts in a single place and makes it easy to include all the scripts in the system PATH variable.

--------------
Coding scripts
--------------

Users need to specify parameters to scripts to define what they want to do. To do so, they pass arguments when calling the script. The first thing any script has to do is define what options and arguments the script accepts. Once the valid arguments are defined, the script can parse the command line. An example follows::

    #!/usr/bin/env python
    
    import sys
    from DIRAC import S_OK, S_ERROR, gLogger
    from DIRAC.Core.Base import Script
    
    #Define a simple class to hold the script parameters
    class Params:
    
      def __init__( self ):
        self.raw = False
        self.pingsToDo = 1
      
      def setRawResult( self, value ):
        self.raw = True
        return S_OK()
    
      def setNumOfPingsToDo( self, value ):
        try:
          self.pingsToDo = max( 1, int( value ) )
        except ValueError:
          return S_ERROR( "Number of pings to do has to be a number" )
        return S_OK()
    
    #Instance the params class
    cliParams = Params()
    
    #Register accepted switches and their callbacks
    Script.registerSwitch( "r", "showRaw", "show raw result from the query", cliParams.setRawResult )
    Script.registerSwitch( "p:", "numPings=", "Number of pings to do (by default 1)", cliParams.setNumOfPingsToDo )
    
    #Define a help message
    Script.setUsageMessage( '\n'.join( [ 'Ping a list of services and show the result',
                                         'Usage:',
                                         '  %s [option|cfgfile] <system name to ping>+' % Script.scriptName,
                                         '  Specifying a system is mandatory' ] ) )
    
    #Parse the command line and initialize DIRAC
    Script.parseCommandLine( ignoreErrors = False )

    #Get the list of services
    servicesList = Script.getPositionalArgs()

    #Do stuff
	if len( servicesList ) == 0:
      gLogger.fatal( "No services defined" )
      sys.exit(1)

    #Import the required DIRAC modules
    from DIRAC.Interfaces.API.DIRAC import DIRAC
    #Do stuff...


Let's follow the example step by step. First we import the required modules from DIRAC. S_OK and S_ERROR are the default way DIRAC modules return values or errors. Script module is the initialization and command line parser that scripts use to initialize themselves. *No other DIRAC module should be imported here*.

Once the required modules are imported. A parameters class is defined. This class has the values for all switches with all their default values. When the class is instanced, the parameters get the default values in the constructor function. It also has a set of functions that will be called for each switch that is specified in the command line. We'll come back to that later.

Then the list of valid switches and what to do in case they are called is defined. Each switch definition has 4 parameters:

#. Short switch form. It has to be one letter. Optionally it can have ':' after the letter. If the switch has ':' it requires one parameter with the switch. A valid combination for the previous example would be '''-r -p2'''. That means show raw results and make 2 pings.
#. Long switch form. '=' is the equivalent of ':' for the short form.
#. Definition of the switch. This text will appear in the script help.
#. Function to call if the user uses the switch

There are several reserved switches that DIRAC uses by default and cannot be overwritten by the script. Those are:

* -h and --help show the script help
* -d and --debug enables debug level for the script
* -s and --section changes the default section in the configuration for the script
* -o and --option set the value of an option in the configuration
* -c and --cert use certificates to connect to services

After defining the switches, the "parseCommandLine" function has to be called. This method initializes DIRAC and parses the command line. **It is really important to call this function before importing any other DIRAC module**. The callbacks defined will be called when parsing the command line if necessary. *Even if the switch is not supposed to receive a parameter, the callback has to receive a value*.

Once the command line has been parsed and DIRAC is properly initialized. The rest of the required DIRAC modules can be imported and the script logic can take place.

