Deploying on Nuvla
==================

.. _libcloud-deploy:

Deploying a Simple Component
----------------------------

We will start by deploying a simple component.  (A "component" for
SlipStream is a single-VM deployment.) This is a minimal CentOS image
with the Libcloud library (and dependencies) installed.

 - When logged into Nuvla, open the `Centos-libcloud` application from
   the following link: https://nuv.la/module/Training/Centos-libcloud.
 - Click on the `Deploy` action.
 - Click on the `Deploy` button after filling in the form.

At the end of the process, you'll be redirected to the Nuvla
Dashboard. 

On the "Deploy Application Component" dialog you may optionally set
the Tag field with comma separated values to help you remember why the
component was deployed.  Because we will later use it when presenting
the :ref:`libcloud` feature, you may want to set the tags "libcloud,
training".


Deploying an Application
------------------------

For SlipStream an "application" consists of a deployment with multiple
virtual machines.  To assist with the lifecycle management of
applications, Nuvla will deploy one "orchestrator" machine per cloud. 

To deploy, the example application:

 - Ensure that you are logged into Nuvla.
 - Navigate to the `LAMP` application:
   https://nuv.la/module/apps/LAMP/lamp-deployment.
 - Click on the `Deploy` action.
 - Click on the `Deploy` button in the dialog.

You should not need to change anything in the deployment dialog,
although, as before, you may add tags to your deployment.  You will be
redirected to the Nuvla Dashboard at the end of the process. 




