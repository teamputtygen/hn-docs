Create Components and Applications
==================================

SlipStream (and Nuvla) allow you to define portable cloud components
and applications. For SlipStream "components" are single virtual
machine services and "applications" are multiple virtual machine
services make up of one or more components.

To ensure that the components and applications are portable,
SlipStream uses an application model that described customized images
as a set of deltas (recipes) that describe the changes from "native
images".  

In other words, components define a template, including resources
characteristics such as default CPU, RAM and disk.  They also define recipes
(aka scripts) that are executed as part of the deployment, to transform
the image into a fully configured VM.
The native images are normally minimal operating system images that
are have consistent behavior across clouds.

The SlipStream tutorial contains an `extensive section`_ on how to create
your own components and applications.  Please refer to that
tutorial. For cases where quick start up times are important, you can
also `build images`_ on clouds that support it.

.. _`extensive section`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/module-3.html

.. _`build images`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/faster-deployment.html 

