
.. _hpcaas:

HPCaaS
======

High Performance Computing (HPC) involves the execution of a
CPU-intensive application with a particular set of input parameters
and input data sets. Because of the large CPU requirements, these
applications normally use the Message Passing Interface (MPI) to
create a high-performance platform from a sizable number of discrete
machines.

These types of applications are naturally job or task-based and
historically have been run on batch systems such as Torque_, SLURM_,
or HTCondor_.  This job-based focus, however, fundamentally conflicts
with the virtual machine focus of IaaS cloud infrastructures.

There are two approaches for running HPC applications on the
HNSciCloud-RHEA hybrid cloud platform:

 1. Direct use of virtual machines on the hybrid cloud platform to run
    a single MPI-based HPC job on a "raw cluster". 
 2. Deployment of a dedicated batch system within the hybrid cloud
    platform to manage multiple HPC jobs.

Of the two approaches, the first is strongly preferred because it
avoids the incidental complexity of managing a batch system.

Raw Cluster
-----------

In this case, a collection of virtual machines are provisioned and
joined together via SSH to create a raw cluster, dedicated to running
a single job.

The advantages of this approach are:

 - The resources can be tuned precisely to the requirements of the
   calculation.
 - The user avoids the administrative overhead in having to deploy and
   manage a batch cluster.
 - Questions of authentication and authorization are sidestepped
   because the deployed cluster serves a single user and single job.

The only apparent disadvantage is the overhead in provisioning the
resources.  Most cloud infrastructure can now provision all resources
within a few minutes, so the overheads are already small. Taking into
account that typical HPC jobs last hours or days, the overhead is
miniscule in comparison.

An example application has been provided that demonstrates how to
create a parameterized SlipStream application that creates and uses a
raw cluster.  The example uses the FDMNES application from the
European Synchrotron Radiation Facility (ESRF_).

The architecture of the application is shown in the figure below.
There is a single master node and any number of worker nodes.  The
application will automatically configure all of the nodes so that the
"mpiuser" account can control all of the workers nodes via SSH.

.. image:: ../images/fdmnes-layout.png
           :scale: 80%
           :align: center
           :alt: "Raw Cluster" Application Architecture
                 

The example application will download and install a tarball containing
the application to be run from a Onedata deployment.  It will then
start the application with the ``mpirun`` command, wait for the job to
complete, and then provide the results in the application report, as
well as pushing them to the Onedata system.

Batch System
------------

If you prefer instead to use a proper batch system to handle your HPC
application, then you can deploy a proper batch system on the cloud.
An example SlipStream application for a `SLURM cluster`_ is provided.

To deploy the application, follow the usual SlipStream procedure,
clicking on "Deploy..." to start the cluster.  Once the cluster has
started, you can log into the head node as root.  You can then use the
usual command ``qsub`` to start your jobs.

Note that the cluster only creates a single unix user for running
jobs.  If you want to have a multple user system, you'll need to
create a new Torque deployment, add the required users, and manage
access for those users to the head node.

Note that the Torque application, as defined, is not a scalable.  That
means that you cannot change the number of worker nodes dynamically
after the Torque application starts.  You can also create a new Torque
application to create a scalable version.


.. _Torque: http://www.adaptivecomputing.com/products/open-source/torque/ 

.. _SLURM: https://slurm.schedmd.com/overview.html

.. _HTCondor: https://research.cs.wisc.edu/htcondor/ 

.. _ESRF: http://www.esrf.eu/

.. _`SLURM cluster`: https://nuv.la/module/apps/Torque/torque-deployment

