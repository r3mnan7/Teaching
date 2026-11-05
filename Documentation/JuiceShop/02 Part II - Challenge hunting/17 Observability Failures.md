---
title: "Observability Failures"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part2/observability-failures.html
tags: [juiceshop, owasp, companion-guide]
---

# Observability Failures

Part of [[01 Challenge hunting|Challenge hunting]]
← Previous: [[16 Cryptographic Issues|Cryptographic Issues]]
Next: [[18 Miscellaneous|Miscellaneous]] →

## Challenges covered in this chapter

| Name | Description | Difficulty |
|---|---|---|
| Access Log | Gain access to any access log file of the server. | ⭐⭐⭐⭐ |
| Exposed Metrics | Find the endpoint that serves usage data to be scraped by a popular monitoring system. | ⭐ |
| Leaked Access Logs | Dumpster dive the Internet for a leaked password and log in to the original user account it belongs to. (Creating a new account with the same password does not qualify as a solution.) | ⭐⭐⭐⭐⭐ |
| Misplaced Signature File | Access a misplaced SIEM signature file. | ⭐⭐⭐⭐ |

## Gain access to any access log file of the server

> An access log is a list of all the requests for individual files that people have requested from a Web site. These files will include the HTML files and their imbedded graphic images and any other associated files that get transmitted. The access log (sometimes referred to as the "raw data") can be analyzed and summarized by another program.
>
> In general, an access log can be analyzed to tell you:
>
> The number of visitors (unique first-time requests) to a home page The origin of the visitors in terms of their associated server’s domain name (for example, visitors from .edu, .com, and .gov sites and from the online services) How many requests for each page at the site, which can be presented with the pages with most requests listed first Usage patterns in terms of time of day, day of week, and seasonally Access log keepers and analyzers can be found as shareware on the Web or may come with a Web server.[^1]

The Juice Shop application server is writing access logs, which can contain interesting information that competitors might also be interested in.

- Who would want a server access log to be accessible through a web application?

- Normally, server log files are written to disk on server side and are not accessible from the outside.

- One particular file found in the folder you might already have found during the "Access a confidential document" challenge might give you an idea who is interested in such a public exposure.

- Drilling down one level into the file system might not be sufficient.

## Find the endpoint that serves usage data to be scraped by a popular monitoring system

The popular monitoring system being referred to in the challenge description is [Prometheus](https://github.com/prometheus/prometheus):

> Prometheus is an open-source systems monitoring and alerting toolkit originally built at SoundCloud. Since its inception in 2012, many companies and organizations have adopted Prometheus, and the project has a very active developer and user community. It is now a standalone open source project and maintained independently of any company. To emphasize this, and to clarify the project’s governance structure, Prometheus joined the Cloud Native Computing Foundation in 2016 as the second hosted project, after Kubernetes. [^2]

- Try to guess what URL the endpoint might have.

- The Juice Shop serves its metrics on the default path expected by Prometheus

- Guessing the path is probably just as quick as taking the RTFM route via [https://prometheus.io/docs/introduction/first_steps](https://prometheus.io/docs/introduction/first_steps)

## Dumpster dive the Internet for a leaked password and log in to the original user account it belongs to

The company behind the Juice Shop failed miserably at implementing any data loss prevention measures for itself. This challenge simulates a seemingly harmless data leak that - upon closer inspection - subsequently allows an account takeover.

> **Data loss prevention software** detects potential data breaches/data ex-filtration transmissions and prevents them by monitoring, detecting and blocking sensitive data while in use (endpoint actions), in motion (network traffic), and at rest (data storage).
>
> The terms "data loss" and "data leak" are related and are often used interchangeably. Data loss incidents turn into data leak incidents in cases where media containing sensitive information is lost and subsequently acquired by an unauthorized party. However, a data leak is possible without losing the data on the originating side. Other terms associated with data leakage prevention are information leak detection and prevention (ILDP), information leak prevention (ILP), content monitoring and filtering (CMF), information protection and control (IPC) and extrusion prevention system (EPS), as opposed to intrusion prevention system. [^3]

- As the challenge name implies, your task is to find some leaked access logs which happen to have a fairly common format.

- A very popular help platform for developers might contain breadcrumbs towards solving this challenge.

- The actual log file was copied & paste onto a platform often used to share data quickly with externals or even just internal peers.

- Once you found and harvested the important piece of information from the log, you could employ a technique called Password Spraying to solve this challenge.

> Password spraying refers to the attack method that takes a large number of usernames and loops them with a single password. We can use multiple iterations using a number of different passwords, but the number of passwords attempted is usually low when compared to the number of users attempted. This method avoids password lockouts, and it is often more effective at uncovering weak passwords than targeting specific users.[^4]

## Access a misplaced SIEM signature file.

> Security information and event management (SIEM) technology supports threat detection and security incident response through the real-time collection and historical analysis of security events from a wide variety of event and contextual data sources. It also supports compliance reporting and incident investigation through analysis of historical data from these sources. The core capabilities of SIEM technology are a broad scope of event collection and the ability to correlate and analyze events across disparate sources.[^5]

The misplaced signature file is actually a rule file for [Sigma](https://github.com/Neo23x0/sigma), a generic signature format for SIEM systems:

> Sigma is a generic and open signature format that allows you to describe relevant log events in a straight forward manner. The rule format is very flexible, easy to write and applicable to any type of log file. The main purpose of this project is to provide a structured form in which researchers or analysts can describe their once developed detection methods and make them shareable with others.
>
> Sigma is for log files what Snort is for network traffic and YARA is for files.[^6]

- You need to trick a security mechanism into thinking that the file you want has a valid file type.

- If you solved one of the other four file access challenges, you already know where the SIEM signature file is located.

- Simply reuse the trick that already worked for the files above.

------------------------------------------------------------------------

[^1]: [https://searchsecurity.techtarget.com/definition/access-log](https://searchsecurity.techtarget.com/definition/access-log)

[^2]: [https://prometheus.io/docs/introduction/overview/](https://prometheus.io/docs/introduction/overview/)

[^3]: [https://en.wikipedia.org/wiki/Data_loss_prevention_software](https://en.wikipedia.org/wiki/Data_loss_prevention_software)

[^4]: [https://resources.infosecinstitute.com/password-spraying/](https://resources.infosecinstitute.com/password-spraying/)

[^5]: [https://www.gartner.com/it-glossary/security-information-and-event-management-siem/](https://www.gartner.com/it-glossary/security-information-and-event-management-siem/)

[^6]: [https://github.com/Neo23x0/sigma#what-is-sigma](https://github.com/Neo23x0/sigma#what-is-sigma)

[[16 Cryptographic Issues|Cryptographic Issues]] [[18 Miscellaneous|Miscellaneous]]

