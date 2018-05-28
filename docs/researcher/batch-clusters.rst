
.. _batch:

Batch Clusters
==============

High Performance Computing (HPC) involves the execution of a
CPU-intensive application with a particular set of input parameters
and input data sets. Because of the large CPU requirements, these
applications normally use the Message Passing Interface (MPI) to
create a high-performance platform from a sizable number of discrete
machines.

These types of applications are naturally job or task-based and
historically have been run on batch systems such as Slurm_, or
HTCondor_.  These systems can be run within cloud infrastrutures,
although they generally lead to a significant amount of incidental
complexity and service management overheads.

Slurm
-----

An example SlipStream application for a `Slurm cluster`_ is
provided. This example deploys a fully functioning Slurm cluster with
one master node and multiple workers (two by default). All the nodes
participate in an NFS file system exported by the master node.

To deploy the cluster, navigate to the "slurm-cluster" application
within Nuvla and click the ``Deploy...`` action. You can choose which
cloud infrastructure to use, the number of workers, and their size
from the deployment dialog.

.. figure:: images/slurm-dialog.png
   :alt: Slurm Deployment Dialog
   :width: 60%
   :align: center

You can also deploy this application from the command line using the
``ss-execute`` command::

  $ ss-login --username=<username> --password=<password>
  $ ss-execute \
      --parameters=worker:multiplicity=4 \
      --keep-running=on-success \
      apps/BatchClusters/slurm/slurm-cluster
  https://nuv.la/run/98f42dca-98e8-4265-875e-90ddf81d6fca

Use the ``--help`` option to find out how to set other parameters for
the ``ss-execute`` command.

Once the deployment is in the "Ready" state, you can log into the
master node to use the cluster.  The SSH key from your user profile
will have been added to the ``root`` and ``tuser`` accounts.

 - Log into the ``root`` account to adjust the packages available on
   the server or to change the configuration.  You can also log into
   the worker nodes to do the same, if necessary.

 - Log into the ``tuser`` account to run your jobs.  This is a normal
   user account with fewer privileges.  All the usual Slurm
   commands are available.

Although both accounts are available to you, normally you will use the
``tuser`` account for running your calculations.

When your calculations have completed, you can release the resources
assigned to the cluster by either clicking the ``Terminate`` action
from the deployment detail page in the web application or using the
command line::

  $ ss-terminate 98f42dca-98e8-4265-875e-90ddf81d6fca

The command line will wait for the full termination of the run. 

.. warning:: **All** the resources, including local storage, will be
             released.  Be sure to copy your results off the master
             node to your preferred persistent storage.


.. _Slurm: https://slurm.schedmd.com/overview.html

.. _HTCondor: https://research.cs.wisc.edu/htcondor/ 

.. _Slurm cluster: https://nuv.la/module/apps/BatchClusters/slurm/slurm-cluster
