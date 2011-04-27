======================================
Developing Services
======================================

Service Handler
-------------------

All the DIRAC Services are built in the same framework where developers should provide
a Service Handler by inheriting the base RequestHandler class. An instance of the Service Handler
is created each time the service receives a client query. Therefore, the handler data members 
are only valid for one query. If the service state should be preserved, this should be done
using global variables or a database back-end. 

Creating a Service Handler is best illustrated by the example below which is presenting a fully 
functional although a simple service:: 

    ########################################################################
    # $HeadURL: $
    ########################################################################
    
    """ SimpleMessage Service is an example of how to build services in the DIRAC framework 
    """
    
    __RCSID__ = "$Id: $"
    
    from types import *
    from DIRAC.Core.DISET.RequestHandler import RequestHandler
    from DIRAC import gLogger, S_OK, S_ERROR
    
    currentMessage = ""
    messageAuthor = ""
    
    def initializeSimpleMessageHandler( serviceInfo ):
    
      global currentMessage = "No Message"
      messageAuthor = "No Author" 
      return S_OK()
    
    class SimpleMessageHandler( RequestHandler ):
    
      def initialize(self):
        """ Handler initialization
        """
        credDict = self.getRemoteCredentials()
        self.user = credDict[ 'username' ]
        self.messageOfTheDay = self.getCSOption('MessageOfTheDay','NoMessageOfTheDay')
    
      types_sendMessage = [ StringTypes ]
      def export_sendMessage(self,message):
        """ Send a message to the service
        """
        global currentMessage, messageAuthor
        currentMessage = message
        messageAuthor = self.user
        return S_OK(message) 
        
      types_getMessage = []
      def export_getMessage(self):
        """ Get a message from the service
        """
        global currentMessage, messageAuthor
        resultDict = {'Message':currentMessage,
                      'Author':messageAuthor,
                      'MessageOfTheDay':self.messageOfTheDay}
        return S_OK(resultDict)   

Let us walk through this code to see which elements should be provided.

First, the comment line with the SVN keyword ''$HeadURL: $'' is provided. This line will 
be substituted by the SVN to show the author and the date of the last commit. 

Next comes the documentation string describing the service purpose and behavior. It is
followed by the ''__RCSID__'' global module variable which is assigned the value of the
''$Id: $'' SVN keyword.

Several import statements will be clear from the subsequent code.

The Service name is SimpleMessage. The ''initializeSimpleMessageHandler'' method is
called once when the Service is created. Here one can put creation and initialization
of the global variables if necessary.

Now comes the definition of the SimpleMessageHandler class. One can define initialize()
method with no arguments although this is not necessary. The details of the caller can
be obtained using the "getRemoteCredentials()" method of the base RequestHandler class.
The other useful method is getCSOption() which allows to extract options from the Service
section in the Configuration Service.

For each service interface method it is necessary to define *types_<method_name>* class 
variable of the List type. Each element of the List is one or a list of possible types 
of the method arguments in the same order as defined in the method definition. The types 
are imported from the ''types'' standard python module.             

The name of each method which will be accessible to the clients has export_ prefix. Note that
the clients will call the method without this prefix. Otherwise, it is an ordinary class method
which takes the arguments provided by the client and returns the result to the client. The result
must always be returned as an S_OK or S_ERROR structure.

Default Service Configuration parameters
------------------------------------------

The Service Handler is written. It should be placed to the Service directory of one
of the DIRAC System directories in the code repository, for example FrameworkSystem. 
The default Service Configuration parameters should be added to the corresponding 
System ConfigTemplate.cfg file. In our case the Service section in the ConfigTemplate.cfg 
will look like the following::

  Services
  {
    SimpleMessage
    {
      Port = 9188
      MessageOfTheDay = The weather is fine today
      Authorization
      {
        Default = all
        sendMessage = ServiceAdministrator
      }
    }
  }  
  
Note that you should choose the port number on which the service will be listening which
is not conflicting with other services. This is the default value which can be changed later
in the Configuration Service. The Port parameter should be specified for all the services.
The MessageOfTheDay is this service specific option.

The Authorization section specifies access writes to all the Service interface methods.
In our case by default the service is available for everybody. But the sendMessage interface
method can only be called by a member of the DIRAC group which has ServiceAdministrator
property.  

Installing the Service
------------------------

Once the Service is ready it should be installed. The DIRAC Server installation is described
in [[[[here]]]. If you are adding the Service to an already existing installation it is
sufficient to execute the following in this DIRAC instance::

  > dirac-install-service Framework SimpleMessage
  
This command will do several things:

  * It will create the SimpleMessage Service directory in the standard place and will set 
    it up under the ''runit'' control - the standard DIRAC way of running permanent processes. 
  * The SimpleMessage Service section will be added to the Configuration System. So, its
    address and parameters will be available to clients.
    
The Service can be also installed using the SystemAdministrator CLI interface::

  > install service Framework SimpleMessage      
  
The SystemAdministrator interface can also be used to remotely control the Service, start or
stop it, uninstall, get the Service status, etc.       

Calling the Service from a Client
-----------------------------------

Once the Service is installed and running it can be accessed from the clients in the way
illustrated by the following code snippet::

  from DIRAC.Core.DISET.RPCClient import RPCClient
  
  simpleMessageService = RPCClient('Framework/SimpleMessage')
  result = simpleMessageService.getMessage()
  if not result['OK']:
    print "Error while calling the service:", result['Message']
  else:
    for key,value in result['Value'].items():
      print key,value
      
Note that the service is always returning the result in the form of S_OK/S_ERROR structure.        
 