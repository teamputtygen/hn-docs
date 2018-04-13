.. _libcloud:

Libcloud
=========

From your Dashbord, identify the Centos-libcloud component you have previously deployed
(when :ref:`libcloud-deploy`) and click on its `Service URL` link


Using the libcloud compute driver for Slipstream
-------------------------------------------------
For the browser-based interfaces to Nuvla and Onedata services, you can directly
use the credentials for your Identity Provider in the eduGAIN and Elixir AAI federations.
For API and command line interface access to Nuvla, the use of revocable API key/secret pairs are recommended.


Enabling Account Password
^^^^^^^^^^^^^^^^^^^^^^^^^

For users authenticating via eduGAIN and Elixir AAI, **you must
enable a password for your Nuvla account**. To do this, see :ref:`enabling-pwd`


Generating API key/secret on Nuvla
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Define the following alias::

  $ alias ss-curl="curl --cookie-jar ~/cookies -b ~/cookies -sS"



- Create a json file defining the nuvla session with your <username> and <password>::

    cat > session-create-internal.json <<EOF
    {
      "sessionTemplate" : {
                            "href" : "session-template/internal",
                            "username" : "<username>",
                            "password" : "<password>"
                           }
    }
    EOF

- Generate a `cookies` file for the session::

     ss-curl https://nuv.la/api/session \
         -D - \
         -o /dev/null \
         -XPOST \
         -H content-type:application/json \
         -d@session-create-internal.json

- You may check that a `cookies` file is actually created::

  $ cat ~/cookies

- Credential Creation::

    cat > create.json <<EOF
    {
      "credentialTemplate" : {
                               "href" : "credential-template/generate-api-key",
                               "ttl" : 86400
                              }
    }
    EOF

NB : The ttl parameter for the API key/secret lifetime (TTL) is optional.
If not provided, the credential will not expire (but can still be revoked at anytime.)
The TTL value is in seconds, so the above time corresponds to 1 day.

To actually create the new credential::

  $ ss-curl https://nuv.la/api/credential \
   -X POST \
   -H 'content-type: application/json' \
   -d @create.json
  {
    "status" : 201,
    "message" : "created credential/05797630-c1e2-488b-96cd-2e44acc8e286",
    "resource-id" : "credential/05797630-c1e2-488b-96cd-2e44acc8e286",
    "secretKey" : "..."
  }

- Store KEY and SECRET as environment variable

Copy the secret (secretKey) that is returned from the server and export it::

  $ export SECRET=<...>

NB : This secret is not stored on the server and cannot be recovered.

The `key` is the value of `resource-id` (without the `credential\ ` prefix).
Example::

  $ export KEY=05797630-c1e2-488b-96cd-2e44acc8e286


Using Libcloud for Nuvla deployment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- You will need the latest version of the `slipstream-libcloud-driver`::

    pip install slipstream-libcloud-driver

- Open a python session::

  $ python

- Import convenience modules::

    import os
    from pprint import pprint as pp

- require modules for the slipstream driver::

    import slipstream.libcloud.compute_driver
    from libcloud.compute.providers import get_driver

- create the driver itself::

    slipstream_driver = get_driver('slipstream')

- Log into Nuvla using API key and secret::

    # KEY and SEare taken from the environment

    ss = slipstream_driver(os.environ["KEY"],
                           os.environ["SECRET"],
                           ex_login_method='api-key')

- Optionally check you can list available images from App Store::

    pp(ss.list_images(ex_path='examples/images'))


- Complete application (node) deployment (WordPress server)::

     # Get the WordPress image
     image = ss.get_image('apps/WordPress/wordpress')

- Set WordPress Title::

     wordpress_title = 'WordPress deployed by SlipStream through Libcloud'

-  Create the dict of parameters to (re)define::

     parameters = dict(wordpress_title=wordpress_title)

-  Create the Node::

     node = ss.create_node(image=image, ex_parameters=parameters)

- Wait the node to be ready::

     ss.ex_wait_node_in_state(node)

- Update the node::

     node = ss.ex_get_node(node.id)

-  Print the WordPress URL::

     print node.extra.get('service_url')

- Destroy the node (i.e terminate a deployment)::

     ss.destroy_node(node)





Using Libcloud directly on Exoscale
-----------------------------------

- Open a python session::

  $ python

- Import convenience modules::

    import os
    from pprint import pprint as pp

- Require module for the driver::

    from libcloud.compute.providers import get_driver

- Set variables for expected deployment::

    location_name = 'ch-gva-2'
    image_name = 'Linux CentOS 7.4 64-bit 10G Disk (2018-01-08-d617dd)'
    size_name = 'Micro'
    deployment_name='libcloud-example'

- Set your Exoscale Key and Secret::

    key=....
    secret=...

- create the driver::

    exoscale_driver = get_driver('exoscale')

- Log into Exoscale using API key and secret::

    exo = exoscale_driver(key,secret)

- Get location::

     locations = {l.name: l for l in exo.list_locations()}
     location = locations.get(location_name)

- Get image::

    images = {i.extra['displaytext']: i for i in exo.list_images(location=location)}
    image = images.get(image_name)

- Specifiy expected size::

     sizes = {s.name: s for s in exo.list_sizes()}
     size = sizes.get(size_name)

- Deploy the node::

   # Last parameter is optional, but is set here to allow SSH connectivity to the instance
   node = exo.create_node(name=deployment_name, size=size, image=image, location=location, ex_security_groups=['slipstream_managed'] )

At this stage you may check the instance from Exoscale portal

.. figure:: ../../images/libcloud-exo.png
   :alt: Libcloud on Exoscale
   :width: 100%
   :align: center


- Display some results::

   pp(node)
   pp(node.public_ips)
   pp(node.extra['password'])

- Display help message for SSH connection to the running instance::

     msg =""" SSH command :
     $ ssh centos@{}
     # NB : password is {}"""

     print msg.format(node.public_ips[0], node.extra['password'])


- Destroy the node (i.e terminate the deployment)::

     exo.destroy_node(node)
