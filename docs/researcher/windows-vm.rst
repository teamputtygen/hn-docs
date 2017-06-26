Start a Windows VM
==================

The main interface for managing deployments is Nuvla_.  The procedure
is straightforward and the same as for Linux VMs:

 - Navigate to the component/image you want to deploy,
 - Click on the "deploy" button,
 - Choose the cloud/offer to use in the deployment dialog, and
 - Provision the image by clicking the "deploy" button.

You will then be taken to the dashboard where you can follow the
progress of your deployment.  Detailed documentation can be found on
the SlipStream documentation website, specifically the `deployment
documentation`_.

Once the deployment reaches the "ready" state, you can log into your
virtual machine via Microsoft Remote Desktop.

The supported Windows images can be found in the ``examples/images``
project within the Workspace.

.. _Nuvla: https://nuv.la

.. _`deployment documentation`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/images.html#deploy-a-vm
