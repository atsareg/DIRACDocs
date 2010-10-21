==========================================
DIRAC Setup Structure
==========================================

The DIRAC basic components are *Services*, *Databases* and *Agents*. 

*Services* 
  passive components listening to incoming client requests and reacting accordingly by
  serving requested information or performing requested actions. Services themselves
  can be clients of other Services.
  
*Databases*
  Services are keeping their state in the Databases.    

*Agents*
  active components which are running continuously invoking periodically their execution 
  methods. Agents are animating the whole system by sending requests to the DIRAC or
  third party Services. 
  
*Services*, *Databases* and *Agents* are combined together to form *Systems*. A *System*
is delivering a complex functionality providing a solution for a given class of tasks.
Examples of *Systems* are Workload Management System or Configuration System. *Systems* 
can have several instances within a given DIRAC installation. Each instance 
of a *Service*, *Database* or *Agent* can belong to just one instance of their *System*.
The DIRAC software is structured by *Systems*, keeping codes of their respective *Services*, 
*Databases* and *Agents* together.     
     
The highest level of the DIRAC component hierarchy is *Setup*. *Setups* are combining
together instances of *Systems* to provide a complete functionality for the users. A
given user community can use several "Setups". For example, there can be "Production"
*Setup* together with "Test" or "Certification" *Setups* used for development and testing
of the new functionality. An instance of a *System* can belong to one or more *Setups*,
in other words, different *Setups* can share some *System* instances.

Each *System* or *Setup* instance has a distinct name. The mapping of the *Systems* and
*Setups* is described in the Configuration of the DIRAC installation in the "Setups"
section. 

*ToDo*
  - image illustrating the structure