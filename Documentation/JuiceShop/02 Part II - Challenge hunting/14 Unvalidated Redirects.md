---
title: "Unvalidated Redirects"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part2/unvalidated-redirects.html
tags: [juiceshop, owasp, companion-guide]
---

# Unvalidated Redirects

Part of [[01 Challenge hunting|Challenge hunting]]
← Previous: [[13 Security through Obscurity|Security through Obscurity]]
Next: [[15 Broken Anti-Automation|Broken Anti-Automation]] →

## Challenges covered in this chapter

| Name | Description | Difficulty |
|---|---|---|
| Allowlist Bypass | Enforce a redirect to a page you are not supposed to redirect to. | ⭐⭐⭐⭐ |
| Outdated Allowlist | Let us redirect you to one of our crypto currency addresses which are not promoted any longer. | ⭐ |

## Enforce a redirect to a page you are not supposed to redirect to

This challenge is about *redirecting* to an entirely disallowed different location.

- You have to find a way to beat the allowlist of allowed redirect URLs.

- You can find several places where redirects happen in the OWASP Juice Shop.

- The application will only allow you to redirect to allowlisted (previously referred to as whitelisted) URLs.

- Tampering with the redirect mechanism might give you some valuable information about how it works under to hood.

> White list validation involves defining exactly what *is* authorized, and by definition, everything else is not authorized.[^1]

## Let us redirect you to one of our crypto currency addresses

Some time ago the Juice Shop project accepted donations via Bitcoin, Dash and Ether. It never received any, so these were dropped at some point.

- When removing references to those addresses from the code the developers have been a bit sloppy.

- More particular, they have been sloppy in a way that even the Angular Compiler was not able to clean up after them automatically.

- It is of course not sufficient to just visit any of the crypto currency links directly to solve the challenge.

------------------------------------------------------------------------

[^1]: [https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html#whitelisting-vs-blacklisting](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html#whitelisting-vs-blacklisting)

[[13 Security through Obscurity|Security through Obscurity]] [[15 Broken Anti-Automation|Broken Anti-Automation]]

