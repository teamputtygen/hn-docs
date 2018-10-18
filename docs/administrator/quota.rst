Quota
=====

Nuvla protects users from overuse by applying a quota mechanism.  The
same feature gives administrators a powerful tool to control usage on
the platform. The feature takes the form of a Quota resource that
provides fine grain control over how many resources can be consumed,
by users, groups, etc.

When deploying new resources, Nuvla checks if the requested deployment
will result in the user exceeding her quota. Only if this is not the
case, will the deployment be accepted. (If more than one quota
applies, then they must all pass for the request to be accepted.)

.. note:: The current usage when evaluating a quota will include all
          workloads: those deployed through Nuvla and those deployed
          directly on the cloud.  Nuvla, however, can only block
          deployment requests that pass through Nuvla.

The quotas and quota status can be found on the `"Quota" page
<https://nuv.la/webui/quota>`_ of the new browser interface. Hovering
over a Quota indicator will provide more details.

.. image:: ../images/quota-page-with-hover.png
   :scale: 75 %
   :align: center

The organization's administrator can find more more details on this
feature and how to access Quota resources from the API in the `Quota
API documentation`_.

Because a malformed quota can have a severe impact on a large number
of users, all quota changes or additions must be handled through Nuvla
:ref:`support` and requested by an organization's administrator.

.. note:: Quotas defined on the Exoscale cloud are periodically
          queried and then updated on Nuvla. As for all quotas, these
          can be seen in the browser interface and through the API.

.. _`Quota API documentation`: https://ssapi.sixsq.com/#quota-(cimi)
