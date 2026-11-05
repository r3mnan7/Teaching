---
title: "Modes"
source: https://www.zaproxy.org/docs/desktop/start/features/modes/
tags: [owasp-zap, zap-desktop]
---

# Modes

Part of [[00 Features|Features]]
← Previous: [[17 Marketplace|Marketplace]]
Next: [[19 Notes|Notes]] →

ZAP has a ‘mode’ which can be:

- Safe - no potentially dangerous operations permitted.
- Protected - you can only perform (potentially) dangerous actions on URLs in the [[23 Scope|scope]].
- Standard - does not restrict anything.
- ATTACK - new nodes that are in [[23 Scope|scope]] are [[05 Active Scan|actively scanned]] as soon as they are discovered.

It is recommended that you use the Protected mode to ensure that you only attack sites that you mean to.

The mode can be changed via the [toolbar](https://www.zaproxy.org/docs/desktop/ui/tltoolbar/) (or the ZAP API) and is persisted between sessions.

Examples of the things that will not be possible in either Safe mode or in Protected mode when not acting on URLs in the Scope:

- Spidering
- Active Scanning
- Fuzzing
- Force Browsing
- Breaking (intercepting)
- Resending requests

You can define the [[22 Scan Policy]] to be used for the Attack mode the [Options Active Scan screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/options/ascan/).

## See also

- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP

