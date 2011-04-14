===========================
dirac-admin-allow-se
===========================

Enable using one or more Storage Elements

Usage::

   dirac-admin-allow-se SE1 [SE2 ...]

 

 

Options::

  -r   --AllowRead       :       Allow only reading from the storage element 

  -w   --AllowWrite      :      Allow only writing to the storage element 

  -S:  --Site=           :         Allow all SEs associated to site 

Example::

  $ dirac-admin-allow-se M3PEC-disk
  $
