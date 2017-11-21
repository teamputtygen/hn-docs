
Onedata Deployment
==================

The following demonstrates the deployment of Onedata based Data
Management infrastructure for HNSciCloud.  The deployment is done on
three cloud sites: OTC, Exoscale GVA and Exoscale DK.

A video of the `Onedata deployment process
<https://youtu.be/iyhGoatXUZ4>`_ can be found on YouTube.

We will deploy and configure:

 - Onezone in Exoscale GVA
 - Oneprovider and GlusterFS in OTC
 - Oneprovider and GlusterFS in Exoscale GVA
 - Oneprovider and GlusterFS in Exoscale DK

Everything deployed in Exoscale DK is viewed as Buyers Group simulated
site.

.. figure:: ../images/onedata-deployment.png
   :alt: Onedata Deployment
   :width: 80%
   :align: center

`Nuvla service <https://nuv.la>`_ will be used for all the
deployments.

Prerequisites
-------------

1. Create the following buckets ``sixsq-data``, ``sixsq-project-1``,
   ``sixsq-project-2`` in object storages of

   - Exoscale https://portal.exoscale.ch/storage
   - OTC https://auth.otc.t-systems.com/authui/login#/login -> Object Storage Service

   Later the buckets will correspondingly be mapped to ``data``,
   ``project-1`` and ``project-2`` spaces in Onedata.

2. Configure groups, roles and Data Manager user in KeyCloak.
  - Groups
     - ``/sixsq`` set as default in the realm
     - ``/sixsq/project-1``
     - ``/sixsq/project-2``
  - Roles: ``data-manager``
  - Assign the groups and the role to the Data Manager user.

Deployment
----------

1. Deploy Onedata on two cloud sites - Exoscale GVA and OTC - using
   https://nuv.la/module/HNSciCloud/onedata/onedata.  This is the
   deployment of the standard Onedata stack for two cloud sites.
  - Onedata on Exoscale GVA
  - Oneprovider on Exoscale GVA
  - Oneprovider on OTC

  Documentation for deployment
  http://hn-prototype-docs.readthedocs.io/en/latest/data-manager/service-deployment.html#one-click-deployment-of-onezone-and-oneproviders-on-exoscale-and-otc

2. Deploy GlusterFS using
   https://nuv.la/module/HNSciCloud/GlusterFS/gluster-cluster on
  - Exoscale GVA (VM size 2/4/100)
  - OTC (VM size 2/4/100)
  - Exoscale DK (VM size 2/4/400)

  Documentation for deployment
  http://hn-prototype-docs.readthedocs.io/en/latest/data-manager/service-deployment.html#glusterfs-cluster-deployment-from-nuvla

3. After deployment in step 1. is ready, deploy Oneprovider BG
   https://nuv.la/module/HNSciCloud/onedata/oneprovider for BG site.

   Documentation for deployment
   http://hn-prototype-docs.readthedocs.io/en/latest/data-manager/service-deployment.html#oneprovider

4. As Data Manager, authenticate to Onedata provisioned in step 1.
   Create the following set of spaces that will be used by the
   project.
  - ``data``
  - ``project-1``
  - ``project-2``
  - ``cache-exo``
  - ``cache-otc``

5. Populate GlusterFS BG with test data: 3 x 2000 50MB files.

  - On GlusterFS BG instance run the following script

::

    #!/bin/bash
    
    # Local sub-tree not shared with cloud.
    mkdir -p /bricks/brick1/local
    date > /bricks/brick1/local/test-file.txt

    # Sub-tree shared with cloud.
    # Oneprovider will be mounting spaces into 'shared*/' sub-directories.

    base=/bricks/brick1/cloud

    # cloud/
    #       shared/
    #              d-data/test-{1..2000}.file
    #       shared-p1/
    #                 d-p1/test-{1..2000}.file
    #       shared-p2/
    #                 d-p2/test-{1..2000}.file

    for d in shared/d-data shared-p1/d-p1 shared-p2/d-p2; do
       dir=$base/$d
       mkdir -p $dir
       echo == $dir ==
       for n in {1..2000}; do
           dd if=/dev/zero of=$dir/test-$n.file bs=50MB count=1 &>/dev/null
       done
       for n in {1..2000}; do
          date +%s%N >> $dir/test-$n.file
       done
    done

6. Configure storages and spaces on Oneprovider BG.

  - Give provider a friendly name: ``OP-SixSq-site``

  - Add GlusterFS BG storage
     - storage name: ``gfs-bg-shared``
     - volume name: ``data``
     - Relative mountpoint in volume: ``cloud/shared/``

  - Support ``data`` space by ``gfs-bg-shared`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on, Import
       storage data with continuous update.

  - Add GlusterFS BG storage
     - storage name: ``gfs-bg-p1``
     - volume name: ``data``
     - Relative mountpoint in volume: ``cloud/shared-p1/``

  - Support ``project-1`` space by ``gfs-bg-p1`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on, Import
       storage data with continuous update.

  - Add GlusterFS BG storage
     - storage name: ``gfs-bg-p2``
     - volume name: ``data``
     - Relative mountpoint in volume: ``cloud/shared-p2/``

  - Support ``project-2`` space by ``gfs-bg-p2`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on, Import
       storage data with continuous update.

  - Validation.
     - Go to Onezone and refresh the main page. You should see
       provider ``OP-SixSq-site``.
     - Go to the new provider and then to files. You should see the
       directories with files under all the spaces: ``data``,
       ``project-1``, ``project-2``.  The files are on the
       ``OP-SixSq-site`` and provided by GlusterFS.

7. Configure storages and spaces on Oneprovider EXO.

  - Give provider a friendly name: ``OP-Exoscale``

  - Add S3 EXO storage
     - storage name: ``s3-exo-data``
     - bucket name: ``sixsq-data``
     - S3 server: sos.exo.io

  - Support ``data`` space by ``s3-exo-data`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on

  - Add S3 EXO storage
     - storage name: ``s3-exo-p1``
     - bucket name: ``sixsq-project-1``
     - S3 server: sos.exo.io

  - Support ``project-1`` space by ``s3-exo-p1`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on

  - Add S3 EXO storage
     - storage name: ``s3-exo-p2``
     - bucket name: ``sixsq-project-2``
     - S3 server: sos.exo.io

  - Support ``project-2`` space by ``s3-exo-p2`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on

  - Add GlusterFS EXO storage
     - storage name: ``gfs-exo``
     - volume name: ``data``

  - Support ``cache-exo`` space by ``gfs-exo`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on
 
  - Validation.
     - Go to Onezone and refresh the main page. You should see
       provider ``OP-Exoscale``.
     - You should see the provider supports the following spaces:
       ``data``, ``project-1``, ``project-2``, ``cache-exo``.

8. Configure storages and spaces on Oneprovider OTC.

  - Give provider a friendly name: ``OP-OTC``

  - Add S3 OTC storage
     - storage name: ``s3-otc-data``
     - bucket name: ``sixsq-data``
     - S3 server: obs.eu-de.otc.t-systems.com

  - Support ``data`` space by ``s3-otc-data`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on

  - Add S3 OTC storage
     - storage name: ``s3-otc-p1``
     - bucket name: ``sixsq-project-1``
     - S3 server: obs.eu-de.otc.t-systems.com

  - Support ``project-1`` space by ``s3-otc-p1`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on

  - Add S3 OTC storage
     - storage name: ``s3-otc-p2``
     - bucket name: ``sixsq-project-2``
     - S3 server: obs.eu-de.otc.t-systems.com

  - Support ``project-2`` space by ``s3-otc-p2`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on

  - Add GlusterFS OTC storage
     - storage name: ``gfs-otc``
     - volume name: ``data``

  - Support ``cache-otc`` space by ``gfs-otc`` storage
     - Get space support request token from Oneprovider
     - Support space: ``Insecure`` on, ``Mount in root`` on
 
  - Validation.
     - Go to Onezone and refresh the main page. You should see
       provider ``OP-OTC``.
     - You should see the provider supports the following spaces:
       ``data``, ``project-1``, ``project-2``, ``cache-otc``.

9. Share storage spaces with user groups.
  - As Data Manger user login to Onezone.
  - Add ``/sixsq`` group into ``data``, ``cache-exo`` and
    ``cache-otc`` spaces.
  - Add ``/sixsq/project-1`` group into ``project-1`` space.
  - Add ``/sixsq/project-2`` group into ``project-2`` space.
