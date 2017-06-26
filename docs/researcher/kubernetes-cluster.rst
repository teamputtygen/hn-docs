.. _kubernetes-cluster:

Deploy a Kubernetes Cluster
===========================

Kubernetes_ is an open-source system for automating deployment,
scaling, and management of containerized applications. With Nuvla_,
you can deploy your own Kubernetes cluster automatically. 

The `Kubernetes component`_ can be found in the WorkSpace or App Store
on Nuvla (or by clicking the link!).  After finding the component, you
can deploy a cluster by:

 - Clicking on "deploy",
 - Choosing whether the deployment is `scalable`_,
 - Choosing the number of workers, and
 - Launching the cluster by clicking on "Deploy" in the deployment
   dialog.

Once the cluster is ready, you can SSH into the master node.  From
here, list all the nodes and check their current status with:

.. code-block:: bash

    $ kubectl get nodes 
    NAME              STATUS    AGE
    159.100.240.160   Ready     2m
    159.100.240.193   Ready     2m
    159.100.240.64    Ready     2m

You should see output similar to the above if all the nodes have been
correctly deployed and configured.

After checking the status of the cluster, you can then use the
`kubectl` command to deploy and control services on the Kubernetes
cluster.  If you are unfamiliar with Kubernetes, you can follow the
`Kubernetes Hello World`_ tutorial that shows all of the core features
of container application management.

.. _Nuvla: https://nuv.la

.. _Kubernetes: https://kubernetes.io

.. _`Kubernetes component`: https://nuv.la/module/apps/Containers/kubernetes/kubernetes

.. _`Kubernetes Hello World`: http://www.toadworld.com/platforms/oracle/w/wiki/11683.creating-a-hello-world-kubernetes-application

.. _scalable: http://ssdocs.sixsq.com/en/latest/tutorials/ss/module-4.html

