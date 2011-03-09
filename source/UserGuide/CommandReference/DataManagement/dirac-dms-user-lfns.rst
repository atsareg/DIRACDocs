==========================
dirac-dms-user-lfns
==========================

Get the list of all the user files.

Usage::

  dirac-dms-user-lfns [option|cfgfile] ... 

 

Options::

  -D:  --Days=           : Match files older than number of days [0] 

  -M:  --Months=         : Match files older than number of months [0] 

  -Y:  --Years=          : Match files older than number of years [0] 

  -w:  --Wildcard=       : Wildcard for matching filenames [*] 

  -b:  --BaseDir=        : Base directory to begin search (default /[vo]/user/[initial]/[username]) 

  -e   --EmptyDirs       : Create a list of empty directories 

