Usage monitoring in Nuvla
=========================

Resource Monitoring
-------------------

In the standard Nuvla browser interface, you can open the usage page
by clicking `Usage` under the profile menu at the top.  You can also
see `this page <https://nuv.la/webui/usage>`_ in the new browser
interface, CUBIC.

.. figure:: ../../images/usage-menu.png
   :alt: Nuvla usage menu
   :width: 50%
   :align: center

All the resources being used, that are visible from a defined
credential in Nuvla, can be seen here.  This includes resources
started directly on the underlying clouds. 

Usage Information
-----------------

.. figure:: ../../images/usage-filter.png
   :alt: Filtering usage
   :width: 100%
   :align: center

1. Click the filter icon to select parameters
2. Choose a period in the calendar, either a custom or predefined one.
3. Choose cloud provider(s).
4. Update the results with the search button.

Users will be able to see their own usage.  Group administrators or
SlipStream administrators will be able to see more information. 

Deploy a component and notice its impact on usage
--------------------------------------------------

You can check that the information is live by deploying a cloud
application and checking that the usage grows.  For a demonstration,
I'll deploy a large Docker Swarm to see that the value grows with
time.

