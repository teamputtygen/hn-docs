
Deploying Data Management Services
==================================

Data management service are based on Onedata technology. For general
overview of Onedata and it's core concepts including zones, providers
and spaces plese refer to the `official documentation
<https://onedata.org/#/home/documentation/doc/getting_started/what_is_onedata.html>`_.

Onezone
-------

**Deploying Onezone via Nuvla**

TODO

**Configuration with Keycloak**

In order to enable federated identity management for users it is
necessary to enable KeyCloak IdP in Onezone configuration. In order to
do this, an entry has to be added to the ``auth.config`` file in
``/opt/onedata/onezone`` folder.

.. code-block:: erlang

   [
    {basicAuth, []},

    {rhea, [
        % Standard config
        {auth_module, auth_keycloak},
        {app_id, <<"OnezoneTest">>},
        {app_secret, <<"8712ed65- ... -270a30291e76">>},
        % Provider specific config
        {xrds_endpoint, <<"https://fed-id.nuv.la/auth/realms/onedata/.well-known/openid-configuration">>}
     ]}
   ]

More information on setup of various IdP's with Onezone can be found
in the `official documentation
<https://onedata.org/#/home/documentation/doc/administering_onedata/openid_configuration.html>`_.

Oneprovider
-----------

**S3 Oneprovider on cloud via Nuvla**

After deploying Oneprovider VM via Nuvla, it is necessary to add an S3
storage to the Oneprovider using Onepanel administration service,
running on the same host as Oneprovider. In order to open Onepanel
service go to: ``https://ONEPROVIDER_IP:9443`` and login using
administrator credentials.

.. image:: images/onepanel-admin-login.png

After login, go to **Storages** tab and press **Add storage**
button. Depending on whether the S3 bucket is on Exoscale or OTC,
different configuration options must be specified:

- **Exoscale**

.. image:: images/exoscale-s3-storage.png

- **OBS**

.. image:: images/obs-s3-storage.png

**GlusterFS Oneprovider on cloud via Nuvla**

.. image:: images/gluster-storage.png

**Oneprovider in BG organization via Nuvla**

TODO

**Oneprovider in BG organization manually**

When deploying Oneprovider on custom storage resources it is necessary
to add the storage using Onepanel administrative interface.

Currently the following storage backends are supported:

- POSIX (this includes any storage which can be mounted to Oneprovider
  VM such as Lustre or NFS)

- GlusterFS

- S3

- Ceph

- Openstack Swift

Each of the storage types requires different parameters to be
configured properly, which can be found in the `official documentation
<https://onedata.org/#/home/documentation/doc/administering_onedata/storage_configuration.html>`_.

Managing Spaces
---------------

Space can be seen as a virtual directory, which contents are stored on
distributed storage resources provisioned by storage providers. Each
space must have at least one provider supporting it with a non-zero
storage space (quota). The effective quota available to a single space
is the sum of storage quotas dedicated to this space by all storage
providers supporting it.

Creating spaces
~~~~~~~~~~~~~~~

Spaces in Onedata can be seen as virtual volumes or buckets, where an arbitrary
directory and file hierarchy can be created.

To create a new data space follow these steps:

- In the Onezone Web Interface unfold Data space management tab
  located on the left menubar

.. image:: images/spacestabhome.png
   :scale: 50 %

- Click Create new space button

- Provide new space name in the text edit field and confirm

New space will appear in the list of spaces designated with a unique ID.

Supporting spaces with Oneprovider instances
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default new space has no storage resources associated with it. In
order to add storage quota to a space, generate a space support token
by clicking on `Get support` option under space name, copy the
presented token and send the token to the administrator of the
Oneprovider instance whose the storage resources should be assigned to
this space.

