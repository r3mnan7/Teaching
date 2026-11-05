---
title: "A Basic Penetration Test"
source: https://www.zaproxy.org/docs/desktop/start/pentest/
tags: [owasp-zap, zap-desktop]
---

# A Basic Penetration Test

← Previous: [[32 Users|Users]]
Next: [[04 Configuring Proxies|Configuring Proxies]] →

A basic penetration test is made up of the following steps:

### Explore

Use your browser to explore all of the functionality provided by the application.
Follow all links, press all buttons and fill in and submit all forms.
If the applications supports multiple roles then do this for each of the roles.
For each role save the ZAP session in a different file and start a new session before you start using the next role.

### Spider

Use the spider to find URLs that you have either missed or that are hidden. You can also use the AJAX Spider add-on to improve the results and crawl the dynamic-built links.
Explore any links found.

### Forced Browse

Use the forced browse scanner to find unreferenced files and directories (requires “Forced Browse” add-on).

### Active Scan

Use the [[05 Active Scan|active scanner]] to find basic vulnerabilities.

### Manual Test

The above steps will find basic vulnerabilities.
However to find more vulnerabilities you will need to manually test the application.
See the OWASP Testing Guide for more details.
Future versions of the ZAP Desktop User Guide will describe how ZAP can be used to help this process.

## See also

- [[00 Desktop Getting Started|Getting Started]] — for details of how to start using ZAP
- [Introduction](https://www.zaproxy.org/docs/desktop/) — the introduction to ZAP

## External Links

- <https://www.owasp.org/wstg> — OWASP Testing Guide

## Official Videos

- [ZAPCon 2022: Drive-By Pentesting with ZAP Scripts](https://www.youtube.com/watch?v=AqZdqAK9S2g) (38:19)

