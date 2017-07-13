Blocking New User Logins
========================

A useful user management feature is the ability to freeze the authentication process for one's tenant, making it impossible for new users to sign in into `Nuvla`_.

This is a different process from blacklisting users, as this will completely block any unregistered users from signing in, while letting existing users authenticate normally.

Each account manager can achieve this within his/her own tenant by following these steps:

1. on the *Authentication* panel of the Keycloak portal, create a new Authentication Flow

.. image:: ../images/kc-auth-flow.png
   :alt: New authentication flow
   :align: center

.. image:: ../images/kc-save-flow.png
  :alt: Save authentication flow
  :align: center

.. image:: ../images/kc-created-flow.png
   :alt: Created authentication flow
   :align: center

2. do not add any other executions or flows under this new flow. Leaving it empty will basically tell Keycloak not to do anything when summoned
3. since for HNSciCloud user are signing in from external Identity Providers, go to the *Identity Providers* panel in Keycloak and chose one from the list

.. image:: ../images/kc-idp.png
   :alt: IdP
   :align: center

4. for the *First Login Flow* configuration parameter, select the new Authentication Flow you've created above

.. image:: ../images/kc-first-auth-flow.png
   :alt: Change first authentication flow
   :align: center

5. repeat steps 3 and 4 for all the Identity Providers in the tenant


And that's it!


From this point on, new user signing in to `Nuvla`_ (or even to Keycloak directly) will get an *"Invalid"* login (see below) and will not be registered.

.. image:: ../images/kc-blocked-user.png
   :alt: Blocked new user
   :align: center


To stop this blockage, just revert the First Login Flow to what it was before, re-doing steps 3 and 4 above, but this time moving the *First Login Flow* back to **first broker login**.

.. _`Nuvla`: https://nuv.la
