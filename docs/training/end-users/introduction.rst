Introduction
============

The tutorial will use a set of student accounts that have been created
ahead of time. 

 - To log in to Nuvla, browse to the page: https://nuv.la.
 - Click the `Log in` button.
 - On the next popup window click the SixSq/RHEA Federated Login and
   then choose the `SixSq Realm` from the list then click on SixSq
   realm button.

At this point, you will be redirected to the Keycloak server that
allows Federated Identity management via the eduGAIN and Elixir AAI
identity federations.

From the Keycloak login page:

 - Enter your student credentials; your username is one of `studentXX`
   and the password is set to `training`.
 - Accept the Terms and Conditions.

At the end of the process you will be redirected to the App Store page
of Nuvla.

.. _ssh:

Setting SSH keys
----------------

So that you can log into virtual machines that you deploy, Nuvla must
have a copy of your public SSH key.  This describes how to add an SSH
public key to Nuvla. 

 - Go to the upper right menu with your `StudentXX` username and
   choose "Profile".
 - Click the `Edit` button.
 - Open the `General Section` by clicking on the section header.
 - In the Default cloud list, choose `exoscale-ch-gva`.
 - Copy your SSH public key to the `SSH Public Key(s) (one per line)`
   text area.
 - Click the `Save` action.

Be sure to copy the full contents of your public SSH key as a **single
line of text**!

