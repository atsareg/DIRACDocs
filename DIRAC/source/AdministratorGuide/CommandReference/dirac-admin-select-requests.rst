{{{
#!rst

dirac-admin-select-requests
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

  Select requests from the request management system

Usage::

  dirac-admin-select-requests [option|cfgfile] ... 

 

Options::

  -    --JobID=          : WMS JobID for the request (if applicable) 

  -    --RequestID=      : ID assigned during submission of the request 

  -    --RequestName=    : XML request file name 

  -    --RequestType=    : Type of the request e.g. 'transfer' 

  -    --Status=         : Request status 

  -    --Operation=      : Request operation e.g. 'replicateAndRegister' 

  -    --RequestStart=   : First request to consider (start from 0 by default) 

  -    --Limit=          : Selection limit (default 100) 

  -    --OwnerDN=        : DN of owner (in double quotes) 

  -    --OwnerGroup=     : Owner group 
}}}
