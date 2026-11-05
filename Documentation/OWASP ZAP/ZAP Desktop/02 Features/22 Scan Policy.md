---
title: "Scan Policy"
source: https://www.zaproxy.org/docs/desktop/start/features/scanpolicy/
tags: [owasp-zap, zap-desktop]
---

# Scan Policy

Part of [[00 Features|Features]]
← Previous: [[21 Software Bill of Materials|Software Bill of Materials]]
Next: [[23 Scope|Scope]] →

A scan policy defines exactly which [[01 Scanner Rules|rules]] are run as part of an [[05 Active Scan|active scan]].

It also defines how these rules run influencing how many requests are made and how likely potential issues are to be flagged.

You can define as many scan policies as you like and select the most appropriate one when you start the scan via the [Active Scan Dialog](https://www.zaproxy.org/docs/desktop/ui/dialogs/advascan/).

You can define the default scan policy to be used for active scans and for the [[18 Modes|Attack mode]] via the [Options Active Scan screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/options/ascan/).

Active scanning is an attack on those targets.

You should NOT use it on web applications that you do not own.

It should be noted that active scanning can only find certain types of vulnerabilities.

Logical vulnerabilities, such as broken access control, will not be found by any active or automated vulnerability scanning.

Manual penetration testing should always be performed in addition to active scanning to find all types of vulnerabilities.

Active scanning is configured using the [Options Active Scan screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/options/ascan/).

You can also define as many scan policies as you like - these define exactly which rules are run and how they work.

## Configured via

- [Scan Policy Manager Dialog](https://www.zaproxy.org/docs/desktop/ui/dialogs/scanpolicymgr/) — which allows you to manage the scan policies

## See also

- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP
- [[20 Passive Scan|Passive scanning]]
- [[01 Scanner Rules]] — supported by default

