Introduction
============

This training will cover three core activities for users of the RHEA
Consortium's hybrid cloud infrastructure:

#. Deploying cloud applications via Nuvla,
#. Taking advantage of the Libcloud API for multi-cloud management, and
#. using Onedata services for managing data.

After this training, the users should have a general idea of how these
activites can be accomplished on the platform. Further details can be
found in the rest of the `platform documentation
<http://hn-docs.rtfd.io/>`_.

Prerequisites
-------------

To follow the complete end-user tutorial, you must have the following:

 - A laptop with access to the Internet (probably via wifi),
 - An SSH client installed on your laptop (Linux/Mac OS: installed by
   default, Windows: use `Putty
   <https://www.ssh.com/ssh/putty/windows/install>`_),
 - An **OpenSSH** keypair (`Linux/Mac OS
   <https://www.ssh.com/ssh/keygen/>`_, `Windows
   <https://www.ssh.com/ssh/putty/windows/puttygen>`_) , and
 - An identity from a procurer's organization that can be used through
   the eduGAIN or Elixir AAI identify federations.

If you don't have these, consult the referenced documentation or ask
your administrator. 


Logging in to Nuvla
-------------------

The tutorial will use a set of student accounts that have been created
ahead of time. 

 - To log in to Nuvla, visit https://nuv.la with your browser.
 - Click the `Log in` button.
 - On the next popup window select the "SixSq Realm" next to the
   "SixSq/RHEA Federated Login" button.
 - Click on the "SixSq/RHEA Federated Login" button.

At this point, you will be redirected to the Keycloak server that
allows Federated Identity management via the eduGAIN and Elixir AAI
identity federations.

From the Keycloak login page:

 - Enter your student credentials (username=`studentXX`,
   password=`training`).
 - Click the `Log In` button.

At the end of the process you will be redirected to the App Store page
of Nuvla and offered a tutorial.  You can click away the tutorial
dialog, as we'll not be using that here.

.. _ssh:

Setting SSH keys
----------------

So that you can log into virtual machines that you deploy, Nuvla must
have a copy of your public SSH key.  To add your public SSH
public key to Nuvla:

 - Go to the upper right menu with your `StudentXX` username and
   choose "Profile".
 - Click the `Edit` button.
 - Open the `General Section` by clicking on the section header.
 - In the Default cloud list, choose `exoscale-ch-gva`.
 - Copy your SSH public key (in Linux usually in
   ``~/.ssh/id_rsa.pub``) to the `SSH Public Key(s) (one per line)`
   text area.
 - Click the `Save` action.

Be sure to copy the full contents of your public SSH key as a **single
line of text**!
