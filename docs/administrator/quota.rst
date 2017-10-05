Quota
=====

Nuvla protects users from overuse by applying a quota mechanism.  The same feature
gives administrators a powerful tool to control usage on the hybrid-cloud platform. The
feature takes the form of an extended quota resource, which provides fine grain
control over who is allowed to consume how many resources, of which type and on which cloud.

When deploying new resources, Nuvla checks if the requested deployment will result in the
user exceeding its quota. Only if this is not the case, will the deployment be
authorised. Administrators can set quotas, including:

 * CPU
 * RAM
 * Disk

Beyond these simple limits, the quota feature offers great flexibility, such as defining
limits based on aggregations such as *count*, *sum* and *max* on any attributes of a virtual
machine.

The administrator can define a range of ACLs for each quota.  The Nuvla monitoring
engine tracks usage and compares this usage with the most applicable quota. This
therefore allows the provisioning engine to ensure that users do not exceed their quota.

Since several quotas can be concurrently applicable at any time (e.g. user, group or
even general tenant level), the most appropriate quota available will be applied when
assessing if a user has or is about to exceeded its quota or not.

To change quotas, the organisation (tenant), the group (sub-tenant) owners, or the user must place a request with
Nuvla :ref:`support` desk. Future releases will allow owners to change quotas themselves,
within the limits set by administrators.

More details on how to use this feature can be found in the `monitoring cloud activities`_
API documentation.

.. _`monitoring cloud activities`: http://ssapi.sixsq.com/#virtual-machines
