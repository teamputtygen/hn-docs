
Credential Management
=====================

The HNSciCloud prototype provided by the Rhea consortium consists of a
number of independent systems integrated into a coherent platform.
The underlying systems use a variety of different credentials and
tokens to manage users' access to services.  To a large degree, the
system hides the complexity of the credential management from the
user, allowing for seamless access to services.

Nonetheless, it is useful to be aware of the credentials and tokens
that are being used and how they can be revoked in the case of
problems.

The general flow for authentication within the platform is the
following:

 - Users request access to the system via Nuvla or Onedata.
 - Users are redirected to Keycloak, which handles the interactions
   with external Identity Provides in the eduGAIN and Elixir AAI
   federations.
 - For successful authentication requests, Keycloak generates an
   OpenID Connect (OIDC) token which is transmitted to Nuvla or
   Onedata.
 - The services (Nuvla or Onedata) create "service tokens" that are
   then given to users for a single session.

For the interactions with the underlying cloud infrastructures, the
services manage access, mapping requests to the cloud using
appropriate credentials.

The details for each type of token are provided below.

KeyCloak Tokens
---------------

Keycloak manages the interactions with the external Identity Providers
from the eduGAIN and Elixir AAI federations.  The users' credentials
are never revealed to the platform services and hence are not cached
by Keycloak or other services.

For successful authentication requests, Keycloak generates an OpenID
Connect (OIDC) token that is transmitted to Nuvla or Onedata. Keycloak
caches the OIDC token, allowing re-authentication requests from Nuvla
or Onedata to succeed without forcing the user to re-authenticate with
the external Identity Provider.

The cached token is removed after 30 minutes of inactivity (i.e. no
further authentication requests) and kept for a maximum of 10
hours. The cached token can be removed at any time by accessing
Keycloak and explicitly logging out.


Nuvla Access Token (Cookies)
------------------------------

Once a client (either through the API or browser) authenticates with
Nuvla, a short-lived credential is generated and transmitted to the
client as an HTTP cookie.  Further client interactions with Nuvla, use
this cookie.

The credential contains the user's identity as well as "role"
information (which includes defined groups, roles, and
entitlements). It also contains client information, such as the
client's IP address. This information is signed with a certificate
held by Nuvla and is verified on each interaction with the server.
This signature (and verification) avoids attack cases where the user
identity and roles are modified by the user (or another actor).

These credentials cannot be revoked.  Anyone with access to the cookie
can access Nuvla as the user, as long as they are making requests from
the IP address embedded in the cookie. However, these credentials have
a short lifetime (15 minutes?), so the vulnerability window is small.

Nuvla API Keys and Secrets
----------------------------

Although users can use the username/password of their account to
access Nuvla through the API, this is **strongly
discouraged**. Instead, users can generate API keys and secrets for
use with the Nuvla REST API.

When generating a new API key and secret (usually via the browser
interface), the user can optionally specify a time-to-live (TTL). The
API key and secret will automatically expire after the TTL has
passed. If the user does not specify a TTL, then the API key and
secret will be valid indefinitely. The identity, groups, roles,
etc. will be copied from the current user (the one creating the API
key/secret).

Note that the plain-text secret is returned to the user when the API
key/secret pair is created. **The plain-text secret is not stored on
the server and cannot be recovered after the first interaction to
create the credential.**

Any API key/secret pair (with or without a TTL) can be revoked at any
time. Logins via a revoked or expired API key/secret pair are
disallowed immediately, although any Nuvla access tokens (cookies)
will continue to be valid until they expire.

If an API key/secret pair is compromised, then the user can simply
revoke the API key/secret, preventing any further access to the
system. The user should:

 * Check for unknown API keys/secrets, revoking them if found.
 * Stop/delete any cloud resources started with the revoked key(s).

Nuvla administrators can also revoke API keys. Organization managers
can remove any resources associated with the user or the compromised
API key(s).

User Account
--------------

If a user account is compromised, then the organization manager can
disable the user account through the KeyCloak interface. This prevents
new logins via KeyCloak. The organization manager must also request
that the Nuvla account be disabled to prevent further access through
local credentials (such as the API keys).

The organization manager should review the allocated cloud resources
for that user and stop/delete any suspicious (or all) resources.

Normally, normal users will not see the underlying cloud credentials
and hence do not need to be revoked.  However, if these are
compromised, then the cloud-level API keys can be reset. This may
perturb other applications that are running with the same credentials.

Organization (Tenant)
----------------------

Organization managers can disable all logins for an organization (if
they still have access to Keycloak). In addition, they can stop/delete
any (suspicious) cloud resources associated with the organization.

In addition, the Nuvla administrators must be contacted (through the
usual support channel) to disable an organization fully. They will
revoke all API keys for users within that organization and disable the
associated accounts. If the managers no longer have access to the
system, then the clean up of cloud resources can also be performed by
the Nuvla administrators.

Group (Sub-tenant)
--------------------

The organization managers can disable a group, preventing anyone from
logging in with group privileges. In addition, the Nuvla
administrators should be contacted so that any API keys authorizing
the group can also be revoked. The organization must review the
allocated cloud resources and stop/delete any suspicious (or all)
resources.
