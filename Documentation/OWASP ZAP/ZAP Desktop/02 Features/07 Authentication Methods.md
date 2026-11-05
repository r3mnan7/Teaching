---
title: "Authentication Methods"
source: https://www.zaproxy.org/docs/desktop/start/features/authmethods/
tags: [owasp-zap, zap-desktop]
---

# Authentication Methods

Part of [[00 Features|Features]]
← Previous: [[06 Authentication|Authentication]]
Next: [[08 Authentication Verification Strategies|Authentication Verification Strategies]] →

ZAP handles multiple types of authentication (called **Authentication Methods** ) that can be used for websites / webapps. Each **[[11 Contexts|Context]]** has an Authentication Method defined which dictates how authentication is handled. The authentication is used to create Web Sessions that correspond to authenticated webapp [[32 Users]].

Authentication methods can be used in multiple places around ZAP. Some of the examples include:

- defining Users and automatic login
- detection of authenticated/unauthenticated states
- performing automatic re-authentication

Multiple authentication methods have been implemented and the system supports easy addition of new methods, according to user needs. They main ones are described below.

## Manual Authentication

This method allows users to perform the authentication manually (e.g. authenticate in the browser while proxy-ing through ZAP) and then select the corresponding HTTP session. As the actual authentication is being performed by you, this method does not support re-authentication in case the webapp logs a user out. However if you do define an [[08 Authentication Verification Strategies|Authentication Verification Strategy]] then ZAP will maintain the authentication [[28 Statistics|statistics]].

When using this authentication method, configuring a User for the context require choosing an authenticated HTTP session.

## Form-Based Authentication

This method is used for websites / webapps where authentication is done by submitting a form or performing a GET request to a ’login url' using a ‘username/password’ pair of authentication credentials. Re-authentication is possible. Configuration can be done using the [Session Contexts Authentication screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/context-auth/) or using the contextual PopupMenu: *Flag as… Form-Based Authentication Login Request*.

When using this authentication method, configuring a User for the context requires setting up the *username/password* pair of credentials that are used for the form based authentication.

If the application requires submitting the anti-CSRF token presented in the login page, ZAP will handle it automatically. Make sure the token name is configured in [Options Anti CSRF screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/options/anticsrf/).

## JSON-Based Authentication

This method is used for websites / webapps where authentication is done by submitting a JSON object to a ’login url’ using a ‘username/password’ pair of authentication credentials. Re-authentication is possible. Configuration can be done using the [Session Contexts Authentication screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/context-auth/) or using the contextual PopupMenu: *Flag as… JSON-Based Authentication Login Request*.

When using this authentication method, configuring a User for the context requires setting up the *username/password* pair of credentials that are used for the authentication.

## HTTP/NTLM Authentication

This method is used for websites / webapps where authentication is enforced using the HTTP or NTLM Authentication mechanisms employing HTTP message headers. Three authentication schemes are supported: Basic, Digest and NTLM. Re-authentication is possible, as the authentication headers are sent with every authenticated request. Configuration can be done using the [Session Contexts Authentication screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/context-auth/).

When using this authentication method, configuring a User for the context requires setting up the *username/password* pair of credentials that are used for the HTTP/NTLM authentication.

## Script-Based Authentication

This method is useful for websites / webapps where the authentication is a more complex one and some custom scripts that handle the authentication process are beneficial. To use this method, you must first define an Authentication [[24 Scripts|script]] which sends messages or performs other actions as needed by your web-application. This script is then selected for use for a given Context and it is called whenever an authentication is performed. Re-authentication is possible. Configuration can be done using the [Session Contexts Authentication screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/context-auth/).

When using this authentication method, configuring a User for the context requires setting up the a set of parameters defined in the script. For more details, see the provided Authentication Script examples.

## Configured via

- [Session Contexts Authentication screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/context-auth/)

## See also

- [[06 Authentication]] — overview
- [Youtube tutorial](https://youtu.be/cR4gw-cPZOA) — of the Authentication, Session Management and Users Management features of ZAP [external link to <https://youtu.be/cR4gw-cPZOA>].
- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP
- [Session Contexts Dialog](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/contexts/) — for an overview of the Session Properties
- [[32 Users]] — for an overview of Users
- [[03 Anti CSRF Tokens|Anti-CSRF tokens]] — for an overview of anti-CSRF tokens

