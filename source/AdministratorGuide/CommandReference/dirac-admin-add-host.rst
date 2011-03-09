===========================
dirac-admin-add-host
===========================

Add or Modify a Host info in DIRAC

Usage::

dirac-admin-add-host [option|cfgfile] ... Property=<Value> ...

Arguments::

 Property=<Value>: Other properties to be added to the User like (Responsible=XXXX) 

 

Options::

  -H:  --HostName:       : Name of the Host (Mandatory) 

  -D:  --HostDN:         : DN of the Host Certificate (Mandatory) 

  -P:  --Property:       : Property to be added to the Host (Allow Multiple instances or None) 

