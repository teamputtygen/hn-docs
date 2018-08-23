Nuvla & Cloud Accounts
======================

To follow this training, you must have accounts on Nuvla and Exoscale.
You will create both accounts and configure them so that they can work
together.


Exoscale Account
----------------

Start by registering with Exoscale to create an account there.  You
will have been given a voucher code that will allow you to create an
account with 30€ of credit.  This is more than enough to follow the
training.

The account you create belongs to you. You are welcome to continue
using the account (and any remaining credit) after the training. When
the credit is gone, you can add more credit or tie a credit card to
the account.

Follow the :ref:`voucher-redemption` instructions for creating an
account with the voucher code you received.

.. warning::
   
   On the last step, choose **"for personal projects"**, not "for team
   projects".

The process requires email validation, so you will need access to your
mail client. After the email validation, you should be able to log
into the `Exoscale portal <https://portal.exoscale.ch>`_ with the
email address and password you provided.

When you are logged into the Exoscale portal, do the
:ref:`exoscale-ssh-config` to allow you to log into your virtual
machines via SSH.


Nuvla Account
-------------

You will register with Nuvla using your federated identity from
KIT. Follow the :ref:`nuvla-registration` instructions to create your
account.



At the end of the process you will be redirected to the App Store page
of Nuvla and offered a tutorial.  You can click away the tutorial
dialog, as we'll not be using that here.

Nuvla Account Configuration
---------------------------

TBD...

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
