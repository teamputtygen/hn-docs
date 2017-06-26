Deploy Docker Containers
========================

Docker_ is a convenient system for defining, packaging, and deploying
applications.  It uses container technologies to allow portability
between operating systems and to achieve short start-up latencies.  By
default it will use images from the `Docker Hub`_, an open registry of
containers.

On typical IaaS cloud infrastructures, you must first deploy a virtual
machine, install Docker (Docker Compose), and then start your
container.  SixSq provides a `Docker Compose Recipe`_ on Nuvla_ to
make the installation of software and the deployment of containers
easy. 

To launch the Docker Compose virtual machine, find the Docker-Compose
component either in the App Store or the Workspace.

 - Click on "Deploy"
 - Choose the cloud you want to use, and
 - Wait for the virtual machine to start.

Once the machine is ready, you can then log into the machine via SSH
via the Service URL link or manually.  Once you are on the machine,
you can then use Docker and Docker Compose as you normally would.

For example, a simple "Hello World" example:

.. code-block:: bash
                
    $ docker run hello-world 
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    b04784fba78d: Pull complete 
    Digest: sha256:f3b3b28a45160805bb16542c9531888519430e9e6d6ffc09d72261b0d26ff74f
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    ... 

This will show you a message and indicate that the installation is
working correctly.  It also will provide some pointers for doing more
complicated (and useful) tasks.

You can also try something similar from the `Docker Compose Getting
Started`_ page. Following the instructions there, you can deploy a
simple web application that provides a welcome message with a counter
of how many times it has been loaded.

.. code-block:: bash

    ~/composetest$  docker-compose up 
    Creating network "composetest_default" with the default driver
    Building web
    Step 1/5 : FROM python:3.4-alpine
    3.4-alpine: Pulling from library/python
    acb474fa8956: Pull complete

    ...

    redis_1  | 1:M 26 Jun 07:32:59.588 * The server is now ready to accept connections on port 6379
    web_1    |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
    web_1    |  * Restarting with stat
    web_1    |  * Debugger is active!
    web_1    |  * Debugger PIN: 241-972-540

Note that instead of using `localhost` or `0.0.0.0`, you will need to
use the **IP address of the virtual machine** (with the 5000 port!)
from your remote browser.  If everything worked correctly, you should
see a message like "Hello World! I have been seen 2 times.".

From here you might want to look at the entries for deploying Docker
Swarm or Kubernetes. 

.. _Docker: https://www.docker.com

.. _`Docker Hub`: https://hub.docker.com

.. _`Docker Compose Recipe`: https://nuv.la/module/apps/Containers/docker-compose

.. _Nuvla: https://nuv.la

.. _`Docker Compose Getting Started`: https://docs.docker.com/compose/gettingstarted/






