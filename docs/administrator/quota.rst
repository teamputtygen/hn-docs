Quota
=====

The quota feature of Nuvla provides administrators with a powerful tool to control
usage on the hybrid-cloud platform.  This now takes the form of an extended quota
resource, which provides fine grain control over who is allowed to consume how many
resources, of which type and on which cloud.

The administrator can define a range of ACLs for each quota.  The Nuvla monitoring
engine tracks usage and compares this usage with the most applicable quota. This
allows the provisioning engine to ensure that users do not exceed their quota.

Since several quotas can be concurrently applicable at any time (e.g. user, group or
even general tenant level), the biggest quota available will be applied when
assessing if a user has exceeded its quota or not.

To change quotas, organisation and group owners, or user, must place a request with
Nuvla support desk. Future releases will allow owners to change quotas themselves,
within the limits set by administrators.

`quota API <http://ssapi.sixsq.com/#quota>`_.
