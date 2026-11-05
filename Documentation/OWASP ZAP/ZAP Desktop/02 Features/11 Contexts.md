---
title: "Contexts"
source: https://www.zaproxy.org/docs/desktop/start/features/contexts/
tags: [owasp-zap, zap-desktop]
---

# Contexts

Part of [[00 Features|Features]]
← Previous: [[10 Callbacks|Callbacks]]
Next: [[12 Custom Page|Custom Page]] →

Contexts are a way of relating a set of URLs together.

You can define any contexts you like, but it is expected that a context will correspond to a web application.

It is recommended that you define a new contexts for each web application that makes up the system you are testing, and set them in scope as you test each one.

Contexts are defined as a set of regular expressions (regexs) which are applied to all of the URLs in the [[26 Sites Tree]].

**Note:** The regular expressions must match the whole URL.

You can configure contexts via:

- Right click menu items in the in the Sites or History tab
- The [Session Contexts dialogs](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/contexts/).

## See also

- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP
- [[29 Structural Modifiers]] — controls which change how ZAP represents the structure of the application

