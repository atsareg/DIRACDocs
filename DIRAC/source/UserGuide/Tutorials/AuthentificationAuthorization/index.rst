========================================
Authentication and Authorization
========================================

Requeriments
----------------

- Valid User Certificate


Managing Certificates
----------------------------------------

Donwloading Certificate from browser
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

- Get the certificate from the browser:

  - Firefox:

      Preferences -> Advanced -> View Certificates -> Select your certificate -> Backup


  - Explorer:

      Tools -> Internet Options ->Content -> Certificates -> Certificates ->Import/Export

Converting Certificates from p12 to pem format
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

- Run dirac-cert-converter script to convert your cert p12 to::

      dirac-cert-convert.sh <USERCERT>.p12

  Output of this command must look like::

      $ dirac-cert-convert.sh usercert.p12
      Creating globus directory
      Converting p12 key to pem format
      Enter Import Password:
      MAC verified OK
      Enter PEM pass phrase:
      Verifying - Enter PEM pass phrase:
      Converting p12 certificate to pem format
      Enter Import Password:
      MAC verified OK
      Information about your certificate:
      subject= /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Vanessa Hamar
      issuer= /C=FR/O=CNRS/CN=GRID2-FR
      Done

- Check than your certificate was converted is in .globus directory, in pem format and with the correct permision::

      $ ls -la ~/.globus
      total 16
      drwxr-xr-x  2 hamar marseill 2048 Oct 19 13:01 .
      drwxr-xr-x 42 hamar marseill 4096 Oct 19 13:00 ..
      -rw-r--r--  1 hamar marseill 6052 Oct 19 13:00 usercert.p12
      -rw-r--r--  1 hamar marseill 1914 Oct 19 13:01 usercert.pem
      -r--------  1 hamar marseill 1917 Oct 19 13:01 userkey.pem


Managing Proxies
-------------------------------

Before run any command into the grid is mandatory to have a valid proxy. The procedure to create a valid proxy using DIRAC commands is showed below.

Creating a user proxy
@@@@@@@@@@@@@@@@@@@@@@@

- First, in the machine where DIRAC client is installed charge DIRAC environment running the following commands::

        cd $DIRAC_PATH
        source bashrc

- After you charged the environment you are able to start your proxy with the command::

        proxy-init --group dirac_<GROUP>


  By example, adding debug option the output must be like::

        $proxy-init --debug --group dirac_user
        2010-10-17 14:44:38 UTC Framework  WARN: Short option -c: has been already defined
        2010-10-17 14:44:38 UTC Framework  WARN: Long option --cert= has been already defined
        Enabling debug output
        Enter Certificate password:
        Proxy lifetime will be 24:00
        User cert is /afs/in2p3.fr/home/h/hamar/.globus/usercert.pem
        User key  is /afs/in2p3.fr/home/h/hamar/.globus/userkey.pem
        Proxy will be written to /tmp/x509up_u40885
        DIRAC Group will be set to dirac_user
        Proxy strength will be 1024
        Contacting CS...
        Checking DN /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Vanessa Hamar
        Username is vhamar
        Creating proxy for vhamar@dirac_user (/O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Vanessa Hamar)
        Uploading user pilot proxy with group dirac_pilot...
        Loading user proxy
        Uploading proxy on-the-fly
        Cert file /afs/in2p3.fr/home/h/hamar/.globus/usercert.pem
        Key file  /afs/in2p3.fr/home/h/hamar/.globus/userkey.pem
        Loading cert and key
        User credentials loaded
        Uploading...
        done

Getting proxy information
@@@@@@@@@@@@@@@@@@@@@@@@@@

- Check than your proxy was correctly created and  DIRAC group and VOMS fqan are set correctly, running the command::

        dirac-proxy-info

  By example::

        $dirac-proxy-info
        subject      : /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Vanessa Hamar/CN=proxy/CN=proxy
        issuer       : /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Vanessa Hamar/CN=proxy
        identity     : /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Vanessa Hamar
        timeleft     : 23:53:55
        DIRAC group  : dirac_user
        path         : /tmp/x509up_u40885
        username     : vhamar
        VOMS         : True
        VOMS fqan    : ['/vo.formation.idgrilles.fr']


- In this moment, your proxy must be uploaded in the server to check that::

        dirac-proxy-get-uploaded-info

  In this case the output shows user DN, group, expiration time and persistent flag.::


        $ dirac-proxy-get-uploaded-info
        Checking for DNs /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Vanessa Hamar
        --------------------------------------------------------------------------------------------------------
        | UserDN                                          | UserGroup   | ExpirationTime      | PersistentFlag |
        --------------------------------------------------------------------------------------------------------
        | /O=GRID-FR/C=FR/O=CNRS/OU=CPPM/CN=Vanessa Hamar | dirac_user  | 2011-06-29 12:04:25 | True           |
        --------------------------------------------------------------------------------------------------------



- You can also go to the web portal and follow the links to check uploaded proxies::

        System -> Framework -> Manage Proxy

  Using the portal you have the option to delete your proxies.

Uploading proxy to DIRAC server
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

- Check the validity of your proxy. In case you need to upload your proxy to DIRAC server, a special command is available::

        dirac-proxy-upload