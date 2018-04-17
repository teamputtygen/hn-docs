KeyCloak Administration
=======================

The KeyCloak server for the RHEA Consortium allows you to manage the
users (and their rights) in your tenant.  All the Buyers Group
organizations have at least one tenant administrator with rights to
make changes in Keycloak.

The KeyCloak server can be found at the address
https://fed-id.nuv.la. 

.. figure:: ../../images/kc-realm.png
   :alt: KC realm settings
   :width: 80%
   :align: center

Keycloak is extremely flexible with respect to the user management
processes.  We have created some simple workflows, but would like to
refine them to make them more relevant for the Buyers Group
organizations.

The workflow for accepting users to your tenant 
You can accept users from any identity provider
connected to eduGAIN or Elixir AAI. You can also create "local" users
on Keycloak for people who do not have credentials for an identity
provider connected to one of those federations.

Default Policies
----------------

By default, any authenticated user from eduGAIN or Elixir AAI can log
into Nuvla/Onedata using any tenant they choose.  The tenant
administrator can set various policies to change who can access their
tenant and what rights users have. 

Users
-----

When you have logged into Keycloak as an administrator, you can see an
overview of your users by clicking on `Users` in the lefthand menu
bar.

This menu option brings you to the user list page.  In the search box
you can type in a full name, last name, or email address you want to
search for in the user database.

For a particular user, you can:

 - Enable or disable the user.
 - See role and group membership.
 - View any active sessions.

Changes you make, take effect **the next time the user logs in through
Keycloak**.  Note that session cookies, cached sessions in Keycloak,
cached session in the Identity Providers, etc. may affect when changes
really take effect. 

Add a Local User
^^^^^^^^^^^^^^^^

Most users will authenticate via an external identity
provider. However, you may need to create a "local" user for someone
who does not have credentials for an identity provider in the eduGAIN
or Elixir AAI identity federations. "Local" means a user created
directly in Keycloak.

On the right side of the user list, you should see an `Add User`
button. Click that to start creating your new user.

.. figure:: ../../images/kc-users.png
   :alt: KC users
   :width: 80%
   :align: center

When viewing a user, you can manage their credentials on the
"Credentials" tab.  You can also assign groups and roles via separate
tabs.

Blacklisting and Whitelisting
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We provide two simple policies for limiting access to your tenant:
white and black lists.  Although crude, they are effective for
limiting access to your tenant. 

See :ref:`blacklisting` and :ref:`whitelisting`


Blocking New Users
^^^^^^^^^^^^^^^^^^

This is useful for allowing existing users to continue using the
system, but block any new users. 

See :ref:`block`

Managing Groups and Roles
^^^^^^^^^^^^^^^^^^^^^^^^^

Groups and roles defined within Keycloak are transitted to Nuvla and
Onedata and can be used for authorization decisions. 

See :ref:`groups` and :ref:`roles`
