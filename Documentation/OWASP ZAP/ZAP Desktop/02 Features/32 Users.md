---
title: "Users"
source: https://www.zaproxy.org/docs/desktop/start/features/users/
tags: [owasp-zap, zap-desktop]
---

# Users

Part of [[00 Features|Features]]
← Previous: [[31 Tags|Tags]]
Next: [[03 A Basic Penetration Test|A Basic Penetration Test]] →

Users are the ZAP representations of websites/webapps’ users. They allow certain actions to be performed from the point of view of an user of the webapps. For each **[[11 Contexts|Context]]**, a set of Users can be defined, which can then be used in actions related to the context. Most commonly, during various scans the request messages can be sent from the point of view of a User.

The concept of Users is tightly tied to the concepts of [[25 Session Management]] and [[06 Authentication]]. When a User is first used somewhere in ZAP, an authentication is performed (according to the Authentication Method defined for the Context) and a Session is created and configured for this user (according to the Session Management defined for the Context). After that, requests sent from the point of view of a User are modified (if necessary) and sent in such a way that the web server identifies them as being sent by an authenticated webapp/website user. If anytime a User appears to be unauthenticated (as determined by the [[08 Authentication Verification Strategies|Authentication Verification Strategy]]), a new authentication is performed and the Session is updated accordingly.

In order to perform the authentication of a user on a website / in a webapp, the Authentication Method defines how the authentication is done (the process), while the necessary credentials (the exact identifiers) are dependent on the user, so, in ZAP, they are configured in the Users.

## Configured via

- [Session Contexts Dialog](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/contexts/#users)

## See also

- [Youtube tutorial](https://youtu.be/cR4gw-cPZOA) — of the Authentication, Session Management and Users Management features of ZAP [external link to <https://youtu.be/cR4gw-cPZOA>].
- [[06 Authentication|Authentication Overview]] — for an overview of Authentication in ZAP
- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP
- [Session Contexts Dialog](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/contexts/) — for an overview of the Session Properties

