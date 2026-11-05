---
title: "Tags"
source: https://www.zaproxy.org/docs/desktop/start/features/tags/
tags: [owasp-zap, zap-desktop]
---

# Tags

Part of [[00 Features|Features]]
← Previous: [[30 Structural Parameters|Structural Parameters]]
Next: [[32 Users|Users]] →

A tag is short piece of text that you wish to associate with a request.

A request can have multiple tags associated with it.

Tags can be added and changed using the [Manage History Tags dialog](https://www.zaproxy.org/docs/desktop/ui/dialogs/managetags/).

They can also be automatically added via [[20 Passive Scan|Passive scanning]] - this allows you to automatically tag pages which match a particular regular expression.

Tags are shown in the [History tab](https://www.zaproxy.org/docs/desktop/ui/tabs/history/) and can be filtered on using the [History Filter dialog](https://www.zaproxy.org/docs/desktop/ui/dialogs/hist_filter/).

### Alert Tags

If a History tag starts with `ALERT-TAG:` then the tag will also be associated with any alerts raised against that request. The prefix will be dropped, and if the remaining string contains an `=` character then the alert tag key will be the string up to the first `=` and the value will be the rest of the string. If there is no `=` then the key will be the string after the prefix and the value will be blank.
A History tag of `ALERT-TAG:AAA=BBB` will result in an Alert tag with a key of `AAA` and a value of `BBB`, while a History tag of `ALERT-TAG:CCC` will result in an Alert tag with a key of `CCC` and a blank value.

## See also

- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP

