.. _docker-swarm:

Deploy Docker Swarm
===================

Running Docker in "Swarm" mode allows you to deploy and control a
cluster of machines running the Docker Engine. The Docker Swarm
component in Nuvla_ automates the installation, configuration, and
deployment of a swarm.

The `Docker Swarm`_ component can be found in the WorkSpace or App
Store on Nuvla (or by clicking the link!).  After finding the
component, you can deploy a swarm by:

 - Clicking on "deploy",
 - Choosing whether the deployment is `scalable`_,
 - Choosing the number of masters and workers, and
 - Launching the swarm by clicking on "Deploy" in the deployment
   dialog.

Once the deployment is ready, you can check the status of the swarm
with:

.. code-block:: bash

    $ docker info

    ...

    Swarm: active
     NodeID: ra0mpoiy6pnv7j6xnlbav3e12
     Is Manager: true
     ClusterID: 8gux86j2845pzs9uuwftgqnfu
     Managers: 1
     Nodes: 4

    ...

If you are logged into a manager, you should see that the "Is Manager"
flag is true.  Here a 5 node deployment was started with 1 manager and
4 workers.

You can get more details about the swarm by looking at the node status
from a manager machine:

.. code-block:: bash

    $ docker node ls
    ID                           HOSTNAME                                     STATUS  AVAILABILITY  MANAGER STATUS
    o476smd7wm3s97rin0u6nn84q    worker2310ef996-af16-4815-8ba7-c88d630d95f4  Ready   Active        
    ra0mpoiy6pnv7j6xnlbav3e12 *  master1310ef996-af16-4815-8ba7-c88d630d95f4  Ready   Active        Leader
    vl3c4ypaguwglypr5qko6zkgu    worker3310ef996-af16-4815-8ba7-c88d630d95f4  Ready   Active        
    vvz3beena6t4200bhag331d5b    worker1310ef996-af16-4815-8ba7-c88d630d95f4  Ready   Active        

You should see a listing like the one above, if everything has worked
correctly.  The cluster is ready to be used.

To deploy a service to the swarm, you can follow the `docker swarm
service`_ tutorial.  From the manager node, you can deploy, list,
inspect and remove the services as follows:

.. code-block:: bash

    $ docker service create --replicas 1 --name helloworld alpine ping docker.com

    gzhyxwm0jp4ddesv56g9gcgv7

    $ docker service inspect --pretty gzhyxwm0jp4d 

    ID:		gzhyxwm0jp4ddesv56g9gcgv7
    Name:		helloworld
    Service Mode:	Replicated
     Replicas:	1
    Placement:
    UpdateConfig:
     Parallelism:	1
     On failure:	pause
     Max failure ratio: 0
    ContainerSpec:
     Image:		alpine:latest@sha256:b09306f2dfa3c9b626006b2f1ceeeaa6fcbfac6037d18e9d0f1d407260cb0880
     Args:		ping docker.com 
    Resources:
    Endpoint Mode:	vip

    $ docker service ps gzhyxwm0jp4d 
    ID            NAME          IMAGE          NODE                                         DESIRED STATE  CURRENT STATE               ERROR  PORTS
    x9twksry8knc  helloworld.1  alpine:latest  master1310ef996-af16-4815-8ba7-c88d630d95f4  Running        Running about a minute ago         

    $ docker service rm gzhyxwm0jp4d
    gzhyxwm0jp4d

See the Docker Swarm documentation for scaling and other management
actions for your Docker applications.

.. _Nuvla: https://nuv.la

.. _`Docker Swarm`: https://docs.docker.com/engine/swarm/

.. _scalable: http://ssdocs.sixsq.com/en/latest/tutorials/ss/module-4.html

.. _`docker swarm service`: https://docs.docker.com/engine/swarm/swarm-tutorial/deploy-service/ 
