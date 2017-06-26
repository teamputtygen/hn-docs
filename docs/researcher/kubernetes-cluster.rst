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
``kubectl`` command to deploy and control services on the Kubernetes
cluster.

If you are unfamiliar with Kubernetes, you can follow the tutorials
and look at the reference documentation on the Kubernetes_ website.

For a simple test deployment of nginx_ (a web server), create a file
``nginx.json`` with the following contents:

.. code-block:: json

    {
        "kind": "Pod",
        "apiVersion": "v1",
        "metadata":{
            "name": "nginx",
            "namespace": "default",
            "labels": {
                "name": "nginx"
            }
        },
        "spec": {
            "containers": [{
                "name": "nginx",
                "image": "nginx",
                "ports": [{"hostPort": 80,"containerPort": 80}]
            }]
        }
    }

You can then deploy this web server with the command:

.. code-block:: bash

    $ kubectl create -f nginx.json
    pod "nginx" created

You can then discover what node is running the webserver with:

.. code-block:: bash
                
    $ kubectl describe pod nginx

    $ kubectl describe pod nginx | grep Node: 
    Node:		159.100.240.64/159.100.240.64

In this case the server is running on the node ``159.100.240.64``.
You can point a browser to this node and then you should see the nginx
welcome page.

Afterwards, you can run the command:

.. code-block:: bash 

    $ kubectl delete pod nginx
    pod "nginx" deleted

to clean up the deployment.


.. _Nuvla: https://nuv.la

.. _Kubernetes: https://kubernetes.io

.. _`Kubernetes component`: https://nuv.la/module/apps/Containers/kubernetes/kubernetes

.. _scalable: http://ssdocs.sixsq.com/en/latest/tutorials/ss/module-4.html

.. _nginx: https://nginx.org/en/

