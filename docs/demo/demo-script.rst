.. _platform-demo:

Demo
====

This section contains the information presented during the review
demonstration that took place at DESY on 9 October 2017.

Prepared Services and Machines
------------------------------

AAI Keycloak
~~~~~~~~~~~~

Group and role assignments are handled by the organization's
administrator(s) via `Keycloak <https://fed-id.nuv.la/auth/>`_.

For this demo, we are using the **SixSq organization** (tenant).  The
following **groups** have been created:

 - sixsq (tenant)
 - sixsq/project-1 (sub-tenant)
 - sixsq/project-2 (sub-tenant)

and in addition there is one **role** defined:

 - data-manager 

This indicates the people who will have **administrator access to
Onedata services**.

The following users will be used at different points in the demo,
mainly to show access rights.

+--------+-------------------------+------------+----------+------------------+
| tag    | user                    | federation | IdP      | groups           |
+========+=========================+============+==========+==================+
| dm     | 485879@vho-switchaai... | eduGAIN    | VHO      | all              |
+--------+-------------------------+------------+----------+------------------+
| user-a | loomis@unitedid.org...  | eduGAIN    | UnitedID | sixsq, project-1 |
+--------+-------------------------+------------+----------+------------------+
| user-b | bf2ea9c63f9276...       | Elixir AAI | Google   | sixsq, project-2 |
+--------+-------------------------+------------+----------+------------------+

Generation of API Key/Secret
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For the browser-based interfaces to Nuvla and Onedata services, you
can directly use the credentials for your Identity Provider in the
eduGAIN and Elixir AAI federations.

For API and command line interface access to Nuvla, the use of
**revocable API key/secret pairs are recommended**.  The process for
generating these key pairs can be found in the `online documentation
<http://hn-prototype-docs.readthedocs.io/en/latest/researcher/api-key.html>`_
and there is a `YouTube video <https://youtu.be/VUH4x5QKekQ>`_ showing
the process.

`Logging into Nuvla using the REST API
<http://ssapi.sixsq.com/#authentication>`_ is covered in the API
documentation.  For the demo, we'll use a simple script ``login.ch``::

  #!/bin/bash

  url=https://nuv.la/api/session
  args='--cookie-jar ~/cookies -b ~/cookies -sS'

  rm -f ~/cookies

  curl -XPOST $args $url \
  -d href=session-template/api-key \
  -d key=${KEY} \
  -d secret=${SECRET} > /dev/null 2>&1 

  curl -XGET $args $url

which assumes that the key and secret are in the environment.  The
document returns a JSON document with your session information.  This
can be filtered with ``jq``.  For example, the command::

  ./login.sh | jq .sessions[0].roles

will return your roles.  For the data manager role, this returns
something like this::

  "USER SixSq:uma_authorization SixSq:offline_access SixSq:/sixsq/project-1 SixSq:/sixsq SixSq:data-manager SixSq:/sixsq/project-2 ANON session/b48ea165-7765-4aaf-b72a-e97169ebd292"

notice the groups and roles defined above.

I've defined aliases for the following:

 - `as-dm`
 - `as-user-a`
 - `as-user-b`

to make switching between users convenient at the command line.

Onedata Services
~~~~~~~~~~~~~~~~

The data management infrastructure has been deployed ahead of time to
save time.  The deployment for the demo is shown in the following
figure.

.. figure:: ../images/onedata-deployment.png
   :alt: Onedata Deployment
   :width: 80%
   :align: center

The general instructions for `configuring the data management services
<http://hn-prototype-docs.readthedocs.io/en/latest/data-manager/service-deployment.html>`_
can be found in the online documentation. The specific `Onedata
deployment process <https://youtu.be/iyhGoatXUZ4>`_ for this demo is
shown in a YouTube video and the details are in the "data management
script" document.

The deployed service endpoints and locations are given in the
following table.

+-------------+------------------+---------------------------------------+
| service     | location         | endpoint                              |
+=============+==================+=======================================+
| Onezone     | Exoscale GVA     | https://159.100.243.60                |
+-------------+------------------+---------------------------------------+
| Oneprovider | Exoscale GVA     | https://159.100.242.90                |
+-------------+------------------+---------------------------------------+
| Oneprovider | OTC              | https://80.158.20.81                  |
+-------------+------------------+---------------------------------------+
| Oneprovider | BG (Exoscale DK) | https://159.100.247.56                |
+-------------+------------------+---------------------------------------+
| GlusterFS   | BG (Exoscale DK) | ssh://159.100.249.39 (**alias gfs**)  |
+-------------+------------------+---------------------------------------+


Federated AAI and Credentials (2)
---------------------------------

 - Deploy VMs with Oneclient and show that protections by groups are enforced.
 - Verify that files from the BG site can be accessed via VMs in Exoscale and OTC.
 - Verify that VMs cannot be accessed by non-authorized users.

Data Access Controls
~~~~~~~~~~~~~~~~~~~~

Deploy a basic machine with Oneclient installed on Exoscale and
OTC. To do this, use Nuvla to deploy the `Oneclient components
<https://nuv.la/module/HNSciCloud/onedata>`_.

To save time six machines have already been deployed.  One on both
clouds for each test user.  An alias has also been provided for the
GlusterFS file system on the "simulated procurer site", which will be
needed later.

+--------+----------+------------+-----------------+
| user   | cloud    | alias      | host            |
+========+==========+============+=================+
| dm     | OTC      | dm-otc     | 80.158.21.24    |
+--------+----------+------------+-----------------+
| dm     | Exoscale | dm-exo     | 159.100.241.68  |
+--------+----------+------------+-----------------+
| user-a | OTC      | user-a-otc | 80.158.18.127   |
+--------+----------+------------+-----------------+
| user-a | Exoscale | user-a-exo | 159.100.242.30  |
+--------+----------+------------+-----------------+
| user-b | OTC      | user-b-otc | 80.158.18.171   |
+--------+----------+------------+-----------------+
| user-b | Exoscale | user-b-exo | 159.100.242.6   |
+--------+----------+------------+-----------------+

The Onedata spaces are mounted in the directory ``/mnt/onedata``. When
logging into these machines, the permissions will allow:

 - The data manager to see all spaces (sixsq, project-a, project-b,
   cache-exo, cache-otc).
 - user-a to see all spaces _except_ project-b.
 - user-b to see all spaces _except_ project-a.

All of the data is initially on the BG site. Accessing it from any
other node works transparently. In addition, this causes the file to
be copied to a local cache.

Creating a file on one site makes it visible from all the other sites.

Virtual Machine Access Controls
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Direct access to virtual machines is controlled through SSH. Only the
keys provided by the user are added automatically to the deployed
machines by Nuvla (and the underlying cloud infrastructures).  Trying
to access a virtual machine with the wrong SSH key will fail.

::
   
  # wrong key
  $ ssh -F /dev/null -i ~/.ssh/id_rsa_scissor 159.100.241.68

  ECDSA key fingerprint is SHA256:DIpROW+tGWTX/6ruqLElLH5iPLLbwGOM82fmZcWccLw.
  Are you sure you want to continue connecting (yes/no)? yes
  Warning: Permanently added '159.100.241.68' (ECDSA) to the list of known hosts.
  Permission denied (publickey,gssapi-keyex,gssapi-with-mic).

When managing virtual machines through Nuvla, the service will only
allow you to control the machines that you have deployed.  If you
share credentials, you will be able to list the machines started by
others with the same credentials, but you cannot delete or change
those machines.

**The general access control framework used for CIMI resources will
also be applied to the deployments in the future, allowing richer
control over the management of deployments.**

Transparent Data Access (1)
---------------------------

 - Verify that User A can put data in the "simulated procurer site"
   directly and then access it via Onedata from a VM.
 - Verify that User B can put data into a space that User A cannot
   access and that User B can access the file transparently, but User
   A cannot.

The deployment has a "simulated procurer site" deployed in the DK
region of Exoscale.  Machines in this site are accessible only via the
WAN and not via Géant.

The "local" storage of the site is residing in a GlusterFS
cluster. The machine is located at **159.100.249.39** (alias gfs). The
GlusterFS data:

 - Root of the "bricks" is `/bricks`.
 - The local area `/bricks/brick1/local` is not shared via Onedata.
 - The cloud area `/bricks/brick1/cloud` is shared.

The shared directories are mapped to spaces as follows:

 - `shared` --> `sixsq`
 - `shared-p1` --> `project-a`
 - `shared-p2` --> `project-b`

Now created files in all three areas via the root account on the
GlusterFS node.  These files should be visible (or not) in other
Onedata services in accordance with the usual access controls.

::

  $ ## create 1 MB files for permissions demo

  $ dd bs=1024 count=1000 </dev/urandom > /bricks/brick1/cloud/shared/user-all.dat
  $ dd bs=1024 count=1000 </dev/urandom > /bricks/brick1/cloud/shared-p1/user-a.dat
  $ dd bs=1024 count=1000 </dev/urandom > /bricks/brick1/cloud/shared-p2/user-b.dat

You should see that the files are visible on the deployed Oneclient
VMs and that the checksums for the individual files are the same as
those on the GlusterFS node.

This showed the protection at the level of spaces. **Onedata is also
capable of adding ACLs to files and directories.** This allows an even
finer-grained access control.

Orchestration (3)
-----------------

 - Deploy Kubernetes via API/GUI
 - Show that containers can take advantage of transparent data access
 - Demonstrate a significant number of containers running within the
   deployed infrastructure

To emphasize, SlipStream (and Nuvla) emphasize orchestration on cloud
infrastructures by encouraging the portable description of cloud
applications and facilitating the automated deployment of those
applications across clouds.

In addition, Nuvla contains a number of predefined applications that
allow you to deploy **scalable** container-based infrastructures.
Kubernetes is demonstrated here, but Docker, Docker Swarm, and Mesos
are also available.  Fission, a Function-as-a-Service platform based
on containers is also available.

Given the size of the desired Kubernetes infrastructures, three large
clusters have been deployed before the meeting.  A `video of the
Kubernetes deployment <https://youtu.be/NgMhQit2F5g>`_ can be found on
YouTube. **The video also shows how the Kubernetes deployment can be
scaled up and down.**

The pod that will be run within Kubernetes for this demonstration has:

 - Oneclient installed so that the data management infrastructure can
   be used.
 - IOPing installed to make micro-benchmarks of the data access
   speeds.
 - Creates a benchmark resource within Nuvla for each IOPing run.
 - Shows how the benchmarks can be selected for aggregating values
   over the benchmark documents.

The details for each step are in the following sections.

Log into the Cluster(s)
~~~~~~~~~~~~~~~~~~~~~~~

The pod will be deployed directly from the head node of each
Kubernetes cluster. The IP addresses for those nodes are as follows:

+----------+-----+-------+------------------+
| cloud    | vms | cores | endpoint         |
+==========+=====+=======+==================+
| exoscale | 105 | 420   | 159.100.243.172  |
| otc      | 105 | 420   | 80.158.16.116    |
+----------+-----+-------+------------------+

To show what nodes are available in the cluster, do the following::

  $ kubectl get nodes

There should be one line for each node and they should all be in the
'Ready' state.

Download the Pod Definition
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The yaml file for the `pod definition
<https://raw.githubusercontent.com/SixSq/getting-started/master/scenarios/oneclient_fio/oneclient-io_pod.yaml>`_
can be found on GitHub.  Download this file.

Note that the pod definition uses the registry deployed by the
consortium **registry.nuv.la**.

Set Parameters
~~~~~~~~~~~~~~

Modify the parameters in the yaml file for the deployment:

 - $OD\_ACCESS_TOKEN$: Onedata access token
 - $OP_HOST$: Local Oneprovider host
 - $PATH$: Path, using ``/mnt/data``
 - $CLOUD$: cloud name
 - $KEY$: Nuvla API key
 - $SECRET$: Nuvla API secret

You'll need to provide correct Nuvla credentials to allow the upload
of the benchmark resources.

Launch the Pod
~~~~~~~~~~~~~~

The number of replicas is also a parameter in the file. The default is
1, so we'll change this to 10 initially and then something larger when
everything is shown to work.

Start the pod with the following command::

  $ kubectl create -f oneclient-io_pod.yaml

  replicationcontroller "oneclient-ioping-benchmark-rc" created

  $ kubectl get rc 

  kubectl get rc 
  NAME                            DESIRED   CURRENT   READY     AGE
  oneclient-ioping-benchmark-rc   10        10        10        35s


Collect and Analyze Benchmarks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check that the benchmarks are being created in Nuvla. A script like
the following will work::

  #!/bin/bash

  curl --cookie-jar ~/cookies -b ~/cookies -sS \
       -X PUT -H 'content-type:application/x-www-form-urlencoded' \
       https://nuv.la/api/service-benchmark \
       -d '$last=5' \
       -d '$orderby=created:desc' \
       -d '$aggregation=count:id'

This will show the last 5 benchmark resources.  You can look to see
what metrics are being collected.

Then you can collect statistics over them with the following::

  #!/bin/bash

  curl --cookie-jar ~/cookies -b ~/cookies -sS \
       -X PUT -H 'content-type:application/x-www-form-urlencoded' \
       https://nuv.la/api/service-benchmark \
       -d '$last=0' \
       -d '$orderby=created:desc' \
       -d '$aggregation=count:id' \
       -d '$aggregation=avg:ioping:iops' \
       -d '$aggregation=avg:ioping:bytes_per_second'

You can filter this to get reasonable output with ``jq``::

  $ ./benchmarks-stats.sh | jq .aggregations
  {
    "count:id": {
      "value": 1189
    },
    "avg:ioping:bytes_per_second": {
      "value": 2868820.260188088
    },
    "avg:ioping:iops": {
      "value": 2.7460815047021945
    }
  }

With the information in the benchmark much more detailed information
could be obtained concerning the latencies and bandwidths between
various points in the system.

HPCaaS (7)
----------

 - Use of FDMNES, an OpenMPI-based HPC application.
 - Deployment of cluster of different sizes via SlipStream Libcloud
   driver
 - Transparently access input files and executables via Onedata
 - Publishing of result log to Onedata infrastructure
 - Show resources used during deployment with SlipStream monitoring
   infrastructure
 - Show resource utilization for individual deployments with
   SlipStream metering (accounting) resources

FDMNES 
~~~~~~

The example HPC application is the FDMNES application from ESRF, which
uses OpenMPI over a cluster of compute nodes. The following figure
shows the deployed components and the primary interactions between
them.

.. figure:: ../images/fdmnes-layout.png
   :alt: FDMNES Layout Deployment and Interactions
   :width: 80%
   :align: center

By default, there will be 1 master and 2 client nodes deployed.  On
Exoscale the default size is "Large" (4 CPU, 8 GB RAM) and on OTC the
default size is "c1.large" (4 CPU, 4 GB RAM).

As for most example applications for HNSciCloud, a `SlipStream
application <https://nuv.la/module/HNSciCloud/fdmnes/fdmnes-demo>`_
has been created for this deployment.

Deploy with Libcloud
~~~~~~~~~~~~~~~~~~~~

This will be deployed and controlled via the `SlipStream Libcloud
driver <https://slipstream.github.io/slipstream-libcloud-driver/>`_.

The following script will be used to deploy the FDMNES
application. The default configuration will be deployed on both clouds
and then larger one with more and larger machines.

::
   
  #!/usr/bin/env python

  #
  # convenience modules
  #
  import os
  from pprint import pprint as pp

  #
  # require modules for the slipstream driver
  #
  import slipstream.libcloud.compute_driver
  from libcloud.compute.providers import get_driver

  #
  # create the driver itself
  #
  slipstream_driver = get_driver('slipstream')

  #
  # log into Nuvla using an API key/secret
  # API key and secret are taken from the environment
  #
  ss = slipstream_driver(os.environ["KEY"], 
                         os.environ["SECRET"], 
                         ex_login_method='api-key')


  #
  # deploy a k8s cluster with defined number of nodes
  # on the specified cloud
  #
  image = ss.get_image('HNSciCloud/fdmnes/fdmnes-demo')

  #
  # change the cloud (location) here
  #
  connector = os.environ["CONNECTOR"]
  location = filter((lambda x: x.id==connector), ss.list_locations())[0]
  pp(location)

  #
  # change the size here
  #

  offer = None

  # 12/64/50 Mega Exoscale
  #offer="service-offer/25e47696-bd4f-481d-9352-dcebe087a3de"

  # 16/64/200 OTC
  #offer="service-offer/7b7fb2a4-ec2d-43b0-81ec-e284f05257c7"

  pp(ss.list_sizes(location))

  if offer is not None:
      node_size = filter((lambda x: x.id==offer), ss.list_sizes(location))[0]
  else:
      node_size = None

  pp(node_size)

  access_token = os.environ["ONEDATA_TOKEN"]
  provider_hostname = os.environ["ONEDATA_OP_HOST"]
  onedata_space = os.environ["ONEDATA_SPACE"]

  multiplicity = None
  #multiplicity = {"master": 1, "client": 4}

  params = {"master": {"access-token": access_token,
                       "provider-hostname": provider_hostname,
                       "onedata-space": onedata_space},
            "client": {"access-token": access_token,
                       "provider-hostname": provider_hostname,
                       "onedata-space": onedata_space}}

  node = ss.create_node(image=image,
                        size=node_size,
                        location=location,
                        ex_multiplicity=multiplicity,
                        ex_parameters=params,
                        ex_keep_running='never',
                        # ex_tolerate_failures=allowed_failures,
                        ex_scalable=False)

  print node.id

  #
  # terminate the cluster when finished
  #
  #ss.destroy_node(node)

Transparent Data Access
~~~~~~~~~~~~~~~~~~~~~~~

The input data files and executable are taken from Onedata.  You can
see the input tarball (with the input files and executable) in the
directory ``data/fdmnes``.

The results logs will appear in ``data/fdmnes/logs``.  They have a
naming that shows the cloud (IP Address), start time, slots, and
execution time.

Current Usage for Deployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We can use the SlipStream REST API to show the current resource
utilization, in general and by deployment.  The following script shows
how to use the CIMI filtering capabilities to select resources and
then to aggregate the values.

::
   
  #!/bin/bash

  deployment_id=$1

  curl --cookie-jar ~/cookies -b ~/cookies -sS \
       -X PUT -H 'content-type:application/x-www-form-urlencoded' \
       https://nuv.la/api/virtual-machine \
       -d '$filter=deployment/href="run/'${deployment_id}'"' \
       -d '$last=0' \
       -d '$aggregation=count:id' \
       -d '$aggregation=sum:serviceOffer/resource:vcpu' \
       -d '$aggregation=sum:serviceOffer/resource:ram'

As you can see from the URL, this works for virtual machine resources.
This will be extended to data and other resources in the future.

The result returns a JSON document.  The interesting content is in the
"aggregations" key.

::
   
  $ ./usage.sh c7cccb79-7156-4f39-9e3d-6db5272034e1 | jq .aggregations 
  {
    "sum:serviceOffer/resource:vcpu": {
      "value": 13
    },
    "count:id": {
      "value": 4
    },
    "sum:serviceOffer/resource:ram": {
      "value": 25088
    }
  }

Metering (Accounting) Information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Historical information about resource usage is kept by the "metering"
resources. The API for the metering works nearly identically to that
for the resource utilization.  Simply the URL changes.

Use the following script to see the resource utilization for a given
deployment.

::
   
  #!/bin/bash

  deployment_id=$1

  curl --cookie-jar ~/cookies -b ~/cookies -sS \
       -X PUT -H 'content-type:application/x-www-form-urlencoded' \
       https://nuv.la/api/metering \
       -d '$filter=deployment/href="'${deployment_id}'"' \
       -d '$last=0' \
       -d '$aggregation=count:id' \
       -d '$aggregation=sum:price' \
       -d '$aggregation=sum:serviceOffer/resource:vcpu' \
       -d '$aggregation=sum:serviceOffer/resource:ram'

And the results of this are visible in the returned JSON file.

::
   
  $ ./metering.sh c7cccb79-7156-4f39-9e3d-6db5272034e1 | jq .aggregations 
  {
    "sum:serviceOffer/resource:vcpu": {
      "value": 106
    },
    "count:id": {
      "value": 34
    },
    "sum:serviceOffer/resource:ram": {
      "value": 201728
    },
    "sum:price": {
      "value": 0.027280366433333335
    }
  }

Reporting and Accounting (6)
----------------------------

 - Show current usage by tenant, subtenant, and user.
 - Show historical usage by tenant, subtenant, and user.
 - Show how quotas are defined, evaluated, and enforced.

Overview
~~~~~~~~

The mechanisms for monitoring the current resource utilization,
accounting for usage over time, and enforcing limits on resource usage
are closely related.

The infrastructure to support these functions works like this:

 - Probes maintain a representation of the current global state of the
   system in a set of documents.
 - The metering infrastructure takes frequent, regular snapshots of
   the global state, augmenting the information with, for example,
   pricing information.
 - Quotas compare the current global state with a resource request to
   verify that a request is within the stated limits.
 - "Billing" selects subsets of the metering documents to produce
   time-based reports by tenant, sub-tenant, user, etc.

The mechanisms are completely general and can be applied to any
resource for which there is a probe.  Currently only a probe for
virtual machines is implemented, but probes for storage are planned.

Current Usage
~~~~~~~~~~~~~

As you've seen in the HPCaaS demonstration, the current resource usage
can be obtained by selecting resource documents and aggregating over
values.

The filtering can take into account any field in the resource
document, including tenant, user, etc.

For example to see my current usage, I can use a query like the
following::

  #!/bin/bash

  user_id="$1"
  cloud_id="$2"

  curl --cookie-jar ~/cookies -b ~/cookies -sS \
       -X PUT -H 'content-type:application/x-www-form-urlencoded' \
       https://nuv.la/api/virtual-machine \
       --data-urlencode '$filter=deployment/user/href="user/'${user_id}'"' \
       -d '$last=0' \
       -d '$aggregation=count:id' \
       -d '$aggregation=sum:serviceOffer/resource:vcpu' \
       -d '$aggregation=sum:serviceOffer/resource:ram'

  # add for cloud filtering
  #     --data-urlencode '$filter=connector/href="connector/'${cloud_id}'"' \

::
   
  $ ./current-usage.sh '485879@vho...' | jq .aggregations 
  {
    "sum:serviceOffer/resource:vcpu": {
      "value": 905
    },
    "count:id": {
      "value": 227
    },
    "sum:serviceOffer/resource:ram": {
      "value": 1437696
    }
  }

To show the value for all cloud and all visible resources, remove the
filter on the ``deployment/user/href`` value.  To see for only a
particular cloud add the filter on the ``connector/href`` value.

Historical Usage
~~~~~~~~~~~~~~~~

Historical views of the resource utilization can be obtained in an
analogous way from the metering resources.  The script looks very
similar, but there are a few of differences:

 - The URL changes to "https://nuv.la/api/metering".
 - The ``snapshot-time`` field is added to allow for selection of
   arbitrary time periods.
 - The ``price`` field is added when the resource price information is
   known, allowing a calculation of the approximate cost of resources.

So concretely, the filter can select on time and the aggregations can
now include price.

::
   
  #!/bin/bash

  user_id="$1"
  cloud_id="$2"

  curl --cookie-jar ~/cookies -b ~/cookies -sS \
       -X PUT -H 'content-type:application/x-www-form-urlencoded' \
       https://nuv.la/api/metering \
       --data-urlencode '$filter=deployment/user/href="user/'${user_id}'"' \
       -d '$filter=snapshot-time>="2017-10-01T00:00:00.000Z"' \
       -d '$filter=snapshot-time<="2017-10-08T00:00:00.000Z"' \
       -d '$last=0' \
       -d '$aggregation=count:id' \
       -d '$aggregation=sum:price' \
       -d '$aggregation=sum:serviceOffer/resource:vcpu' \
       -d '$aggregation=sum:serviceOffer/resource:ram'

  # add for cloud filtering
  #     --data-urlencode '$filter=connector/href="connector/'${cloud_id}'"' \

::
   
  $ ./historical-usage.sh '485879@vho...' | jq .aggregations 
  {
    "sum:serviceOffer/resource:vcpu": {
      "value": 34061
    },
    "count:id": {
      "value": 30228
    },
    "sum:serviceOffer/resource:ram": {
      "value": 94775808
    },
    "sum:price": {
      "value": 94.92273246906117
    }
  }

Quotas
~~~~~~

Quotas place limits on resource utilization.  For the hybrid cloud
system, each quota is a separate document that describes

 - The type of resource to use (i.e. the resource collection),
 - A filter to select the documents to include,
 - The field/method on which to aggregate, and
 - A numerical limit. 

The quota resource also contains an action called ``collect`` that
will calculate the current values for the quota.  The URL for this
action is included in the quota document itself.  The returned
document is the quota document augmented with "currentAll" and
"currentUser" fields.

The "currentAll" field includes all resources falling under the given
quota, whereas the "currentUser" provides the user's contribution to
the "currentAll" value.  The limit is always compared to the
"currentAll" value.

Any quota resource that is visible to a user is applied to that user.
**That is, the resource ACL determines who the quota to whom
applies.**

You can list the visible quotas with a simple command::

  $ curl --cookie-jar ~/cookies -b ~/cookies -sS \
         -X GET \
         https://nuv.la/api/quota

using your credentials.  This will list all quotas that apply to you.

Concretely a quota for limiting the number of virtual machines
deployed by an individual user would look like (with uninteresting
fields stripped)::

  {
      "name" : "RAM (VHO)",
      "description" : "limits total RAM usage for VHO account",

      "resource" : "VirtualMachine",
      "selection" : "deployment/user/href='user/485879@vho-switchaai.chhttps://aai-logon.vho-switchaai.ch/idp/shibboleth!https://fed-id.nuv.la/samlbridge/module.php/saml/sp/metadata.php/sixsq-saml-bridge!uays4u2/dk2qefyxzsv9uiicv+y='"

      "aggregation" : "sum:serviceOffer/resource:ram",
      "limit" : 10240,

      "acl" : {
        "owner" : { "...": "..." },
        "rules" : [ 
        { "...": "..." }, 
        {
          "principal" : "485879@vho-switchaai.chhttps://aai-logon.vho-switchaai.ch/idp/shibboleth!https://fed-id.nuv.la/samlbridge/module.php/saml/sp/metadata.php/sixsq-saml-bridge!uays4u2/dk2qefyxzsv9uiicv+y=",
          "right" : "VIEW",
          "type" : "USER"
        } ]
      },
      "operations" : [ {
        "rel" : "http://sixsq.com/slipstream/1/action/collect",
        "href" : "quota/6448b707-eae3-4646-878c-6dce3682a526/collect"
      } ],
  }

By sending an HTTP POST request to the (relative) "collect" URL, you
can get the current usage information related to this quota.

::

  $ curl --cookie-jar ~/cookies -b ~/cookies -sS \
         -X POST \
         https://nuv.la/api/quota/6448b707-eae3-4646-878c-6dce3682a526/collect | \
          jq .currentAll,.currentUser,.limit 
  1437696
  1437696
  10240

**This shows that I've completely blown my quota!**

The next release of SlipStream will include the ability to
parameterize the quotas and to filter directly over groups.

Thanks!
-------

Just a reminder that the `main documentation
<http://hn-prototype-docs.readthedocs.io/en/latest/>`_ can be found in
ReadTheDocs and that there is a `knowledge base
<http://support.sixsq.com/support/solutions>`_ in Freshdesk.

Feedback on improving and expanding the documentation is welcome!
