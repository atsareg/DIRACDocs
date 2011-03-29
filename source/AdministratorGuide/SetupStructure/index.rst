==========================================
DIRAC Setup Structure
==========================================

The DIRAC basic components are *Databases*, *Services* and *Agents*. With these pieces we form *Systems*
that are the minimal functional component of a DIRAC installation.

  **Databases** keep the persistent status of the System. Services and Agents use it as a kind of shared memory.

  **Services** are passive components listening to incoming client requests and reacting accordingly by
  serving requested information from the database backend or inserting requests on the 
  database backend. Services themselves can be clients of other Services from the same 
  DIRAC system or from other systems.

  **Agents** are the active components which are running continuously invoking periodically their execution 
  methods. Agents are animating the whole system by executing actions, inserting new requests 
  to the DIRAC or some third party Services. 
  
*Services*, *Databases* and *Agents* are combined together to form *Systems*. A **System**
is delivering a complex functionality to the rest of DIRAC, providing a solution for a given class of tasks.
Examples of *Systems* are Workload Management System or Configuration System.

To achieve a functional DIRAC installation, cooperation from different *Systems* is required. A 
concrete list of active components from a set of DIRAC system offering a functionality to the 
end user form a DIRAC **Setup**. All DIRAC client installation will point to a given DIRAC *Setup* while 
server installation belong to a DIRAC *Instance* that can be shared by various *Setups*.

The highest level of the DIRAC component hierarchy is thus a *Setup*. **Setups** are combining
together instances of *Systems* to provide a complete functionality for the users. A
given user community may have several *Setups*. For example, there can be "Production"
*Setup* together with "Test" or "Certification" *Setups* used for development and testing
of the new functionality. An instance of a *System* can belong to one or more *Setups*,
in other words, different *Setups* can share some *System* instances. All these setups will 
share the same *Configuration* which allows them to access the same resources.

Each *System* or *Setup* instance has a distinct name. The mapping of the *Systems* and
*Setups* is described in the Configuration of the DIRAC installation in the "Setups"
section. 

*ToDo*
  - image illustrating the structure