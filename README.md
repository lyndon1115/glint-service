glint-service 
=============
The glint-service comprises of glint(uses glance to copy images around) and glint-horizon(which is horizon, but altered the images tab to include image managment for remote repositories)

Pre-req 

1. ensure you have sudo/root on your installation machine

2. check that ports 8080 8483 9494 are open on your test machine 
(glint uses 9494, glint-horizon uses 8080, and to secure horizon glint (even for testing) we use stunnel on 8483)

3. Since glint is registered with openstack as a service, you will need openstack administrative access. This is because the installation uses keystone to add the glint service and register it's endpoint url. This makes glint searchable to other services looking for it. In this case glint-horizon will be searching for it!

   a. login to the openstack horizon interface as admin
   
   b. goto Access & Security pages
   
   c. select API Access tab
   
   d. click Download openstack rc file (then copy to server your using to host glint)


Make sure the file is accessible from the filesystem used to install glint, the glint setup script will read in the 
file and set the environment variables ... you will be prompted for the admin password during the setup of glint at some point

PIP installation Method

Get Glint Services Configuration Scripts

1.sudo pip install -i https://testpypi.python.org/pypi glint-service

Change the Glint Configuration Files for your Setup

2.sudo vim /etc/glint/glint_services.yaml

3.sudo vim /etc/glint/glint_setup.yaml

Double check that your ports are open and accessible

4.sudo vim /etc/sysconfig/iptables

Configure and Start the Service

5.sudo python -m glint-service.glint_setup.py configure

done ...

... we can try an install

Install and Setup

1.Get a copy of the installation scripts (this is all you need)

git clone https://github.com/hep-gc/glint-service.git



2.Edit the Following conf files

   cd glint-service

   i) glint\_services.yaml - replace with your own base-urls for auth-url (keystone) glance\_url (glance) and give a unique name for your own (local) glance repo  -- I like rats;> - this conf file is for the glint service
  
   ii) glint\_setup.yaml 
      - replace glint-installation-auth with the file location of your openstack admin rc file (just leave it if you already sourced the admin rc file already)
      - replace glint-service-url with the url of your host machine (the machine you are running these scripts on)


3.Start Download, Install and Setup

sudo python glint_setup.py install




(this takes about 10-15 minutes)

* you will be prompted for your sudo password

* you will be prompted for you openstack admin password ~5 mins into install:: (unless it was already setup in the environment: i.e. you executed user@glint$  source openstack-admin.rc   #before sudo python glint_setup.py install)



you should be able to use glint through our horizon-glint interface running on 9494

To Login to glint-hoirzon, use:

https://glint_horizon_host_machine:8483/auth/login/

*you will have to accept the certficate which was autogenerated during installation . the certificate should be in /etc/stunnel directory of your host machine


To uninstall:

sudo python glint_setup.py uninstall

* you will need your openstack admin.rc file because the glint service and endpoint need to be removed from keystone
