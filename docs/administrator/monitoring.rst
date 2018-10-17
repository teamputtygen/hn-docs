.. _monitoring:

Monitoring
==========

Nuvla provides a dedicated API resource for `monitoring cloud
activities`_.

Refreshed continuously (~every minute), data gets collected from all
the configured clouds. The monitored data is then assembled to include
for each virtual machines, the following:

 * vCPU
 * RAM
 * Disk
 * Service offer

This information is collected for both cloud resources provisioned via
Nuvla and provisioned directly to the different clouds (e.g. API,
portal, CLI), by-passing Nuvla.  This means that Nuvla provides a
unified global view over all compute resources deployed, independently
of it being used as the deployment engine or not.

Further, the monitoring resource inherits the basic CIMI
functionality, which means it contains specific and precise ACLs, such
that only specific users, groups and organisation members with the
appropriate rights have access to this information.

This resource is key for other new features, such as the
:ref:`accounting` feature.

The REST resource providing this functionality is called
*virtual-machine* and can be used, for example, to query the following
information:

 * Count all virtual machines
 * Count all virtual machines belonging to a give user (requires
   privileged rights)
 * Count all virtual machines launched directly with the cloud,
   by-passing Nuvla
 * Simulate what virtual machines a given user sees (requires
   privileged rights)
 * Group currently running virtual machines, by billing period
   (e.g. billed by minute vs hour)
 * List all virtual machines belonging to a given deployment
 * Count of running virtual machines, grouped by cloud

From the list above, *priviledged rights* include administration
rights (super user), as well as group and organisation owners.

.. _`monitoring cloud activities`: http://ssapi.sixsq.com/#virtual-machines
