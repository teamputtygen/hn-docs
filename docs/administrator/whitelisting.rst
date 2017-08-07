Whitelisting Users
==================

The ability to only permit login to a predefined set of users is also possible available in Keycloak.

As for `blacklisting`_, the account manager can also define a list of usernames that are allowed to login through IdPs into Keycloak and respective clients.

By default new users are not entitled with anything else than the roles to manage and view their own user account, but obviously this can be changed by the account manager by either setting default groups and roles or assigning these to users individually.


Whitelisting a Local User
-------------------------

Local user registration is not enabled in the current SixSq Fed-ID portal, thus new local users have to be manually added by the account manager by creating new users through the "**Users**" section in the Keycloak UI.

.. image:: ../images/kc-addUser.png
   :alt: Add local user
   :align: center


Allowing an External User
-------------------------

**By default, whitelisting for external users is disabled.**

In cases where the account manager wants to restrict the login process to a set of known users coming from either eduGAIN or ELIXIR, he/she needs to enable the whitelisting capability in the authentication flows in Keycloak:

.. image:: ../images/kc-enableWhitelist.png
   :alt: Enable whitelisting
   :align: center

**PLEASE NOTE:** *blacklisting has priority over whitelisting, so if a user is both whitelisted and blacklisted, he/she will not be allowed to login and be considered blacklisted anyway.*

Once enabled, account managers can add users to their whitelist in a similar way as it is done for the blacklist. Let's assume we want to add user *foo@bar.comhttp://myidp.com/login!etc* to the whitelist:

1. go to *Authentication*
2. select in which federation should the user be blacklisted
3. click on *Config* from the *whitelist* script.

.. image:: ../images/kc-configureWhitelist.png
   :alt: Config whitelist
   :align: center

This will open a script where account managers can then find a global variable (a list) called *WHITELISTED_USERNAMES*. The username to allow shall then be put into that list, as a string, like shown below:

.. image:: ../images/kc-addtowhitelist.png
   :alt: Whitelist username
   :align: center

Then just click **Save**, and that's it. That user can now login, provided the username is not blacklisted.



.. _`blacklisting`: ../blacklisting.html
