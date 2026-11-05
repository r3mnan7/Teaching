---
title: "Anti CSRF Tokens"
source: https://www.zaproxy.org/docs/desktop/start/features/anticsrf/
tags: [owasp-zap, zap-desktop]
---

# Anti CSRF Tokens

Part of [[00 Features|Features]]
← Previous: [[02 Alerts|Alerts]]
Next: [[04 API|API]] →

Anti CSRF tokens are (pseudo) random parameters used to protect against Cross Site Request Forgery (CSRF) attacks.

However they also make a penetration testers job harder, especially if the tokens are regenerated every time a form is requested.

ZAP detects anti CSRF tokens purely by attribute names - the list of attribute names considered to be anti CSRF tokens is configured using the [Options Anti CSRF screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/options/anticsrf/).

When ZAP detects these tokens it records the token value and which URL generated the token.

Other tools, like the [[05 Active Scan|active scanner]], have options which cause ZAP to automatically regenerate the tokens when required.

## See also

- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP

