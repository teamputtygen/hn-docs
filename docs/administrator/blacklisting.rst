Blacklisting Users
==================

One of the desired user management features for the HNSciCloud project is the ability to block users. This is an action that can be carried by account managers, at any time, removing the user's access to all client applications in the respective tenant.

There are several ways an account manager can reduce the privileges or even block a user:

Deny User Access to Specific Resources
--------------------------------------

In some cases, the account manager might only want to grant or remove privileges from a certain user, which will later translate on a forbidden access to a specific resource.

Assuming that all the ACLs have been defined, the account manager should use the roles and groups available in Keycloak to manage the users' access to the deployed resources.

These actions are documented in the `Manage Groups`_ and `Managing Roles`_ sections.



Blocking a Local User
---------------------

On the occasional scenario where there are local user accounts setup through Keycloak, the account manager can block these users through 3 different ways:

Disable the account
~~~~~~~~~~~~~~~~~~~

Users with disabled accounts cannot login. The account manager should go the user's view and toggle the *User Enabled* button:

.. image:: ../images/kc-disableuser.png
   :alt: Disable local user
   :align: center

Change the password
~~~~~~~~~~~~~~~~~~~

An obvious way to block a user is to simply disable his/her credentials or even change the password:

.. image:: ../images/kc-disablecredentials.png
   :alt: Disable user credentials
   :align: center

Remove the user
~~~~~~~~~~~~~~~

Finally, a definite and also obvious way to block a user, is simply delete the account:

.. image:: ../images/kc-deleteuser.png
   :alt: Delete a user
   :align: center



Blocking an External User
-------------------------

In most cases, users will be registering into Keycloak (and consequently into the corresponding client applications and services) through external IdPs. These kind of accounts do not have an associated password and therefore it is recommended to have a different approach when blocking users.

To block a user from an external IdP, account manager should add their unique usernames into a **BLACKLISTED_USERNAMES** list, which is crosschecked against every user during login. If a user is blacklisted, he'll be able to register into Keycloak but his/her login will be interrupted and a "blacklisted" message will be shown.

To blacklist a user, account manager should first find the respective username from the users list. Let's assume we want to block user *foo@bar.comhttp://myidp.com/login!etc*. Account managers should then do:

1. go to *Authentication*
2. select in which federation should the user be blacklisted
3. click on *Config* from the *blacklist* script.

.. image:: ../images/kc-blacklistuser.png
   :alt: Modify authentication flow
   :align: center

This will open a script where account managers can then find a global variable (a list) called *BLACKLISTED_USERNAMES*. The username to block shall then be put into that list, as a string, like shown below:

.. image:: ../images/kc-addtolist.png
   :alt: Blacklist username
   :align: center

Then just click **Save**, and that's it. For any other users to be blocked, the procedure is the same, just appending their username to the list above.

---

**IMPORTANT NOTE**: blocking a user does not revoke nor destroy his/her current session. Blocking actions only affect users on their next login.


.. _`Manage Groups`: ../groups.html
.. _`Managing Roles`: ../roles.html
