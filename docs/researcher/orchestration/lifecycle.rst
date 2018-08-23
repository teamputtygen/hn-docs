
Application Lifecycle
=====================

App Store
---------

The first place to look for interesting components (single virtual
machine services) and applications (multiple machine services) is the
`Nuvla App Store`_.

.. figure:: ../images/nuvlaAppStore.png
   :alt: Support Desk Diagram
   :width: 100%
   :align: center

Within the `Nuvla Workspace`_, there are other applications of interest:

 - ``examples/images``: Minimal distributions of common operating
   systems. Usually used as the basis for other components.
 - ``apps``: Curated list of applications that can be used as examples
   for your own applications.
 - ``HNSciCloud``: `This`_ workspace contains several prearranged components and applications to facilitate the testing and evaluation process, including for example:

    - ``HNSciCloud/Benchmarking``: Both generic and HNSciCloud-specific benchmarks for evaluating the system. Relevant for Test Cases 2.2, 5.1 and 11.4.3.
    - ``HNSciCloud/Images``: A subset of ``examples/images``, containing only the HNSciCloud specific operating systems.
    - ``HNSciCloud/VMProvisioningPersonalisation``: An App for testing the provisioning and contextualization of a VM, according to Test Case 2.5.
    - ``HNSciCloud/S3EndpointTest-Exoscale_OTC``: An App for testing S3 in both Exoscale and OTC, according to Test Case 2.3.
    - ``HNSciCloud/HDF5_IO``: An App for verifying HDF5 compliance with the VMs' local storage in the cloud, according to Test Case 4.1.

Other application definitions will appear over time.  If you have
specific needs, contact SixSq support to request new ones.

.. _`Nuvla App Store`:  https://nuv.la/appstore
.. _`This`: https://nuv.la/module/HNSciCloud
.. _`Nuvla Workspace`: https://nuv.la/module


.. _`Nuvla`: https://nuv.la

.. _`https://nuv.la/webui/login`: https://nuv.la/webui/login

.. _`SixSq's Federated Identity Portal`: https://fed-id.nuv.la/auth

.. _`Keycloak`: http://www.keycloak.org/

.. _`simpleSAMLphp`: https://simplesamlphp.org/

.. _`support@sixsq.com`: support@sixsq.com

.. _`here`: ../administrator/index.html

.. _`Remote Machine Access`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/appendix.html?highlight=Remote%20Machine%20access#remote-machine-access
