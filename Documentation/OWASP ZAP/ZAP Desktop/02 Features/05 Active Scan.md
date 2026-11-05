---
title: "Active Scan"
source: https://www.zaproxy.org/docs/desktop/start/features/ascan/
tags: [owasp-zap, zap-desktop]
---

# Active Scan

Part of [[00 Features|Features]]
← Previous: [[04 API|API]]
Next: [[06 Authentication|Authentication]] →

Active scanning attempts to find potential vulnerabilities by using known attacks against the selected targets.

Active scanning is an attack on those targets.

You should NOT use it on web applications that you do not own.

In order to facilitate identifying ZAP traffic and Web Application Firewall exceptions, ZAP is accompanied by a script “AddZapHeader.js” which can be used to add a specific header to all traffic that passes through or originates from ZAP. eg: X-ZAP-Initiator: 3

It should be noted that active scanning can only find certain types of vulnerabilities.

Logical vulnerabilities, such as broken access control, will not be found by any active or automated vulnerability scanning.

Manual penetration testing should always be performed in addition to active scanning to find all types of vulnerabilities.

Active scanning is configured using the [Options Active Scan screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/options/ascan/).

The rules that run are configured via [[22 Scan Policy|Scan Policies]] - you can have as many of these as you like.

## Accessed via

- [Active Scan tab](https://www.zaproxy.org/docs/desktop/ui/tabs/ascan/) — ‘New Scan’ button
- [Sites tab](https://www.zaproxy.org/docs/desktop/ui/tabs/sites/) — ‘Attack/Active Scan…’ right click menu item
- [History tab](https://www.zaproxy.org/docs/desktop/ui/tabs/history/) — ‘Attack/Active Scan…’ right click menu item

## See also

- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP
- [[20 Passive Scan|Passive scanning]]
- [Scan Policy Manager Dialog](https://www.zaproxy.org/docs/desktop/ui/dialogs/scanpolicymgr/) — which allows you to manage the scan policies
- [[01 Scanner Rules]] — supported by default

