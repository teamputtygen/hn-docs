Account Configuration
=====================

To use Nuvla to provision the data management services or cloud
applications on the IaaS cloud infrastructures, you must configure
your Nuvla account.  To access your user profile, click on "Profile"
link under your username.

.. figure:: ../images/nuvlaUserProfile.png
   :alt: Accessing Your User Profile
   :width: 100%
   :align: center

To update your user profile, click on the "Edit..." on the right side
below the page header.


Remote Machine Access
---------------------

To allow you have remote access to the (Linux) virtual machines that
you deploy, you should provide a public SSH key. Once this key has
been added to your profile, Nuvla will automatically configure all
deployed virtual machines with this key, giving you 'root' access to
your deployed machines. The instructions for creating an SSH key pair
and configuring your profile can be found in the `Remote Machine
Access`_ section of the SlipStream documentation.  This documentation
also describes the installation of a "Remote Desktop Connection"
client for accessing Windows machines.

Cloud Provider Configuration
----------------------------

Exoscale
~~~~~~~~

The configuration of your account for Exoscale should have been done
for you, when you initially contacted SixSq support or your realm
administrator.  If this is not the case but you have your Exoscale
credentials, you can follow the `Exoscale Cloud Configuration`_
instructions.  If you do not have credentials, contact the SixSq
support. 

Open Telekom Cloud
~~~~~~~~~~~~~~~~~~

The account manager for each Buyers Group organization is responsible
for creating user accounts.  If you don't have one, then contact your
account manager.

Once you have your account, you'll have to configure your Nuvla
account with an access key and secret key.

Preparing your OTC account
^^^^^^^^^^^^^^^^^^^^^^^^^^

You must setup a Virtual Private Cloud (VPC) and create a security
group before trying to start a virtual machine. First, create a new
security group:

1. From the interface, hover over the "Virtual Private Cloud" and
   click on the "+" sign on the right.
2. The default values should be correct, click on "Create Now".
3. You should have a "success" message and then be redirected to the VPC dashboard.

From here, you now need to create a new security group. We'll create
one that is entirely open.

1. Click on the "Security Groups" box.
2. Click on the "+ Create Security Group" button.
3. Choose a name, then click on "OK".
4. Click on "Modify" next to the new security group in the list.
5. Click on the new security group to see the details.
6. Click on "Add Rule".
7. Open everything to incoming access, use ports 0-65535 and click
   "OK".

Your account should now be ready to start a virtual machine.

Configuration Nuvla with OTC
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**TBD...**

.. _`Remote Machine Access`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/appendix.html#remote-machine-access

.. _`Exoscale Cloud Configuration`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/prerequisites.html#exoscale

