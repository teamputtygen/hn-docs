Create Components and Applications
==================================

SlipStream (and Nuvla) allow you to define portable cloud components
and applications. For SlipStream "components" are single virtual
machine services and "applications" are multiple virtual machine
services make up of one or more components.

To ensure that the components and applications are portable,
SlipStream uses an application model that described customized images
as a set of deltas (recipes) that describe the changes from "native
images".  The native images are minimal operating system images that
are have consistent behavior across clouds.

The SlipStream tutorial contains an `extensive section`_ on how to create
your own components and applications.  Please refer to that
tutorial. For cases where quick start up times are important, you can
also `build images`_ on clouds that support it.

.. _`extensive section`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/module-3.html

.. _`build images`: http://ssdocs.sixsq.com/en/latest/tutorials/ss/faster-deployment.html 

