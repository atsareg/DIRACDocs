=====================================
dirac-admin-get-site-protocols
=====================================

Usage::

  dirac-admin-get-site-protocols.py (<options>|<cfgFile>)* 

 

Options::

  -    --Site=           : Site for which protocols are to be checked 

Example::

  $ dirac-admin-get-site-protocols --Site LCG.IN2P3.fr

  Summary of protocols for StorageElements at site LCG.IN2P3.fr

  StorageElement               ProtocolsList

  IN2P3-disk                    file, root, rfio, gsiftp

