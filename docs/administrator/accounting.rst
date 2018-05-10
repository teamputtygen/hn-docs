.. _accounting:

Accounting
==========

Nuvla provides a new `dedicated API`_ resource for accounting.

The new feature regularly snapshots the global state of deployed
resources provided by the :ref:`monitoring` resource, to build a
historical view of usage. In the process, it also pulls other valuable
information such as pricing from the corresponding service offers.

This functionality, coupled with the extensive CIMI query and
filtering capabilities, provides together a powerful, yet simple, way
to extract accounting information from Nuvla.

The `dedicated API`_ documentation provides several examples of
queries in order to extract the following type of queries, such as:

 * Pricing for any from/to time interval (full days)
 * Extract daily, weekly and monthly consumption aggregation
   (e.g. CPU, RAM, Disk)
 * Extract peak usage over a given time interval, grouped by cloud,
   for a user, group and/or organisation
 * Sum CPU, RAM and/or disk usage, for a given time interval and for a
   cloud, a user, group and/or organisation

Again, these are only example of possible queries.  This feature can
also be used to plot trends, trigger alerts and much more.

.. _`dedicated API`: http://ssapi.sixsq.com/#accounting
