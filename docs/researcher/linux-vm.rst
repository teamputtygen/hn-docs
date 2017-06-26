Start a Linux VM
================

The main interface for managing virtual machine deployments is
Nuvla_.  In general, the procedure is straightforward:

 - Navigate to the component/image you want to deploy,
 - Click on the "deploy" button,
 - Choose the cloud/offer to use in the deployment dialog, and
 - Provision the image by clicking the "deploy" button.

You will then be taken to the dashboard where you can follow the
progress of your deployment.  Detailed documentation can be found on
the SlipStream documentation website, specifically the `deployment
documentation`_.

Once the deployment reaches the "ready" state, you can log into your
virtual machine via SSH.  Be sure that you have configured your Nuvla
account with your public SSH key. 

The common "native" images that are supported across clouds can be
found in the ``examples/images`` project within the Workspace. In
general, Ubuntu 14/16, Debian 7/8, and CentOS 7 are well supported
across clouds.

Other Linux distributions may be supported as well. Either you can ask
SixSq (through support) to extend the list of available images or
create your own `native image`_ component.

.. _Nuvla: https://nuv.la

.. _`deployment documentation`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/images.html#deploy-a-vm

.. _`native image`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/images.html#native-images
