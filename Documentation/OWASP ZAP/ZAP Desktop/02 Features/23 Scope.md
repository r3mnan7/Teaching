---
title: "Scope"
source: https://www.zaproxy.org/docs/desktop/start/features/scope/
tags: [owasp-zap, zap-desktop]
---

# Scope

Part of [[00 Features|Features]]
← Previous: [[22 Scan Policy|Scan Policy]]
Next: [[24 Scripts|Scripts]] →

The Scope is the set of URLs you are testing, and is defined by the [[11 Contexts]] you have specified.

By default nothing is in scope.

The Scope potentially changes:

- What you can do, when you are in Protected [[18 Modes|mode]]
- What is shown in the [History tab](https://www.zaproxy.org/docs/desktop/ui/tabs/history/)
- Protected - user can only perform (potentially) dangerous actions on URLs in the Scope
- Standard - as in previous releases, user can do anything
- ATTACK - new nodes that are in Scope are [[05 Active Scan|actively scanned]] as soon as they are discovered

It is recommended that you define a new [[11 Contexts|Context]] for each web application that makes up the system you are testing, and set them in scope as you test each one.

## See also

- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP

