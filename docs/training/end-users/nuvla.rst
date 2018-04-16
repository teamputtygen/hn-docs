Deploying on Nuvla
==================

.. _libcloud-deploy:

Deploying a Simple Component
----------------------------

We will start by deploying a simple component.  A "component" for
SlipStream is a single-VM deployment. The example we use here is a
minimal CentOS image with the Libcloud library (and dependencies)
installed.

 - When logged into Nuvla, open the `Centos-libcloud
   <https://nuv.la/module/Training/Centos-libcloud>`_ application
 - Click on the `Deploy` action.
 - Click on the `Deploy Application Component` button after optionally
   providing tags.

On the "Deploy Application Component" dialog you may optionally set
the Tag field with comma separated values to help you remember why the
component was deployed.  Because we will later use it when presenting
the :ref:`libcloud` feature, you may want to set the tags "libcloud,
training".

At the end of the process, you'll be redirected to the Nuvla
Dashboard.  At the bottom, you should be able to see the
"Centos-libcloud" component being deployed.  This will take a couple
of minutes to complete.



Deploying an Application
------------------------

For SlipStream an "application" consists of a deployment with multiple
virtual machines.  To assist with the lifecycle management of
applications, Nuvla will deploy one "orchestrator" machine per cloud. 

To deploy, the example application:

 - Ensure that you are logged into Nuvla.
 - Navigate to the `Docker Swarm
   <https://nuv.la/module/apps/Containers/docker-swarm/swarm>`_
   application.
 - Click on the `Deploy` action.
 - Click on the `Deploy` button in the dialog.

You should not need to change anything in the deployment dialog,
although, as before, you may add tags to your deployment.

You will again be redirected to the Nuvla Dashboard at the end of the
process.  This application will run a Docker Swarm cluster with one
Master and one Worker.  (You can change the number of workers in the
deployment dialog.) This will also take a few minutes to complete.

If you have time, you can log into the master node and run a container
on this swarm. 
