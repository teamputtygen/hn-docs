.. _quota:

Quota
=====

Nuvla protects users from over use, by applying a quota mechanism.  Users can query
their quota and request increases to their group or organisation managers.

When deploying new resources, Nuvla checks if the requested deployment will see the
user exceeding the quota. Only if this is not the case, will the deployment be
authorised.

The quota now includes the following artefacts:

 * CPU
 * RAM
 * Disk
 
For more details on how to use this feature can be found in the `monitoring cloud activities`_.

.. note:: This feature is very flexible and will be upgraded to include other monitored artefacts such as storage (e.g. object store). 

.. _`monitoring cloud activities`: http://ssapi.sixsq.com/#virtual-machines
