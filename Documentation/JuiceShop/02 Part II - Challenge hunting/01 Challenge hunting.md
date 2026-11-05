---
title: "Challenge hunting"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part2/README.html
tags: [juiceshop, owasp, companion-guide]
---

# Challenge hunting

← Previous: [[06 Walking the happy path|Walking the happy path]]
Next: [[02 Finding the Score Board|Finding the Score Board]] →

This part of the book can be read from end to end as a *hacking guide*. Used in that way you will be walked through various types of web vulnerabilities and learn how to exploit their occurrences in the Juice Shop application. Alternatively you can start hacking the Juice Shop on your own and use this part simply as a reference and *source of hints* in case you get stuck at a particular challenge.

In case you want to look up hints for a particular challenge, the following tables lists all challenges of the OWASP Juice Shop grouped by their difficulty and in the same order as they appear on the Score Board.

|  |  |
|---|---|
|  | The challenge hints found in this release of the companion guide are compatible with v20.3.0 of OWASP Juice Shop. |

|  |  |  |  |
|---|---|---|---|
| Name | Description | Hints | Solution |
| Access Log | Gain access to any access log file of the server. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Admin Registration | Register as a user with administrator privileges. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Admin Section | Access the administration section of the store. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| AI Debugging | Reveal some behind-the-scenes information on the chatbot as a non-admin user. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Allowlist Bypass | Enforce a redirect to a page you are not supposed to redirect to. | [[14 Unvalidated Redirects\|💡]] | [[01 Challenge solutions\|📕]] |
| API-only XSS | Perform a *persisted* XSS attack with `` <iframe src="javascript:alert(`xss ``)">\` without using the frontend application at all. | [[10 Cross Site Scripting (XSS)\|💡]] | [[01 Challenge solutions\|📕]] |
| Arbitrary File Write | Overwrite the Legal Information file. | [[12 Vulnerable Components\|💡]] | [[01 Challenge solutions\|📕]] |
| Bjoern’s Favorite Pet | Reset the password of Bjoern’s OWASP account via the Forgot Password mechanism with *the truthful answer* to his security question. | [[04 Broken Authentication\|💡]] | [[01 Challenge solutions\|📕]] |
| Blockchain Hype | Learn about the Token Sale before its official announcement. | [[13 Security through Obscurity\|💡]] | [[01 Challenge solutions\|📕]] |
| Blocked RCE DoS | Perform a Remote Code Execution that would keep a less hardened application busy forever. | [[11 Insecure Deserialization\|💡]] | [[01 Challenge solutions\|📕]] |
| Bonus Payload | Use the bonus payload `<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>` in the *DOM XSS* challenge. | [[10 Cross Site Scripting (XSS)\|💡]] | [[01 Challenge solutions\|📕]] |
| CAPTCHA Bypass | Submit 10 or more customer feedbacks within 10 seconds. | [[15 Broken Anti-Automation\|💡]] | [[01 Challenge solutions\|📕]] |
| Change Bender’s Password | Change Bender’s password into *slurmCl4ssic* without using SQL Injection or Forgot Password. | [[04 Broken Authentication\|💡]] | [[01 Challenge solutions\|📕]] |
| Chatbot Prompt Injection | Trick the chatbot into generating a coupon code for you despite its coupon policy saying otherwise. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Christmas Special | Order the Christmas special offer of 2014. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Client-side XSS Protection | Perform a *persisted* XSS attack with `` <iframe src="javascript:alert(`xss ``)">\` bypassing a client-side security mechanism. | [[10 Cross Site Scripting (XSS)\|💡]] | [[01 Challenge solutions\|📕]] |
| Confidential Document | Access a confidential document. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Cross-Site Imaging | Stick cute cross-domain kittens all over our delivery boxes. | [[09 Security Misconfiguration\|💡]] | [[01 Challenge solutions\|📕]] |
| CSP Bypass | Bypass the Content Security Policy and perform an XSS attack with `` <script>alert(`xss ``)\</script>\` on a legacy page within the application. | [[10 Cross Site Scripting (XSS)\|💡]] | [[01 Challenge solutions\|📕]] |
| CSRF | Change the name of a user by performing Cross-Site Request Forgery from another origin. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Database Schema | Exfiltrate the entire DB schema definition via SQL Injection. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Deluxe Fraud | Obtain a Deluxe Membership without paying for it. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Deprecated Interface | Use a deprecated B2B interface that was not properly shut down. | [[09 Security Misconfiguration\|💡]] | [[01 Challenge solutions\|📕]] |
| DOM XSS | Perform a *DOM* XSS attack with `` <iframe src="javascript:alert(`xss ``)">\`. | [[10 Cross Site Scripting (XSS)\|💡]] | [[01 Challenge solutions\|📕]] |
| Easter Egg | Find the hidden easter egg. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Email Leak | Perform an unwanted information disclosure by accessing data cross-domain. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Empty User Registration | Register a user with an empty email and password. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Ephemeral Accountant | Log in with the (non-existing) accountant *acc0unt4nt@juice-sh.op* without ever registering that user. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Error Handling | Provoke an error that is neither very gracefully nor consistently handled. | [[09 Security Misconfiguration\|💡]] | [[01 Challenge solutions\|📕]] |
| Expired Coupon | Successfully redeem an expired campaign coupon code. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Exposed credentials | A developer was careless with hardcoding unused, but still valid credentials for a testing account on the client-side. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Exposed Metrics | Find the endpoint that serves usage data to be scraped by a popular monitoring system. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Extra Language | Retrieve the language file that never made it into production. | [[15 Broken Anti-Automation\|💡]] | [[01 Challenge solutions\|📕]] |
| Five-Star Feedback | Get rid of all 5-star customer feedback. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Forged Coupon | Forge a coupon code that gives you a discount of at least 80%. | [[16 Cryptographic Issues\|💡]] | [[01 Challenge solutions\|📕]] |
| Forged Feedback | Post some feedback in another user’s name. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Forged Review | Post a product review as another user or edit any user’s existing review. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Forged Signed JWT | Forge an almost properly RSA-signed JWT token that impersonates the (non-existing) user *rsa_lord@juice-sh.op*. | [[12 Vulnerable Components\|💡]] | [[01 Challenge solutions\|📕]] |
| Forgotten Developer Backup | Access a developer’s forgotten backup file. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Forgotten Sales Backup | Access a salesman’s forgotten backup file. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Frontend Typosquatting | Inform the shop about a *typosquatting* imposter that dug itself deep into the frontend. (Mention the exact name of the culprit) | [[12 Vulnerable Components\|💡]] | [[01 Challenge solutions\|📕]] |
| GDPR Data Erasure | Log in with Chris' erased user account. | [[04 Broken Authentication\|💡]] | [[01 Challenge solutions\|📕]] |
| GDPR Data Theft | Steal someone else’s personal data without using Injection. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Greedy Chatbot Manipulation | Convince the chatbot to give you a coupon of 50% or more. Because apparently a 10% max policy is just a suggestion when you ask nicely enough. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| HTTP-Header XSS | Perform a *persisted* XSS attack with `` <iframe src="javascript:alert(`xss ``)">\` through an HTTP header. | [[10 Cross Site Scripting (XSS)\|💡]] | [[01 Challenge solutions\|📕]] |
| Imaginary Challenge | Solve challenge \#999. Unfortunately, this challenge does not exist. | [[16 Cryptographic Issues\|💡]] | [[01 Challenge solutions\|📕]] |
| Leaked Access Logs | Dumpster dive the Internet for a leaked password and log in to the original user account it belongs to. (Creating a new account with the same password does not qualify as a solution.) | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Leaked API Key | Inform the shop about a leaked API key. (Mention the exact key in your comment) | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Leaked Unsafe Product | Identify an unsafe product that was removed from the shop and inform the shop which ingredients are dangerous. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Legacy Typosquatting | Inform the shop about a *typosquatting* trick it has been a victim of at least in v6.2.0-SNAPSHOT. (Mention the exact name of the culprit) | [[12 Vulnerable Components\|💡]] | [[01 Challenge solutions\|📕]] |
| Local File Read | Gain read access to an arbitrary local file on the web server. | [[12 Vulnerable Components\|💡]] | [[01 Challenge solutions\|📕]] |
| Login Admin | Log in with the administrator’s user account. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Login Amy | Log in with Amy’s original user credentials. (This could take 93.83 billion trillion trillion centuries to brute force, but luckily she did not read the "One Important Final Note") | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Login Bender | Log in with Bender’s user account. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Login Bjoern | Log in with Bjoern’s Gmail account without previously changing his password, applying SQL Injection, or hacking his Google account. | [[04 Broken Authentication\|💡]] | [[01 Challenge solutions\|📕]] |
| Login Cloud Admin | Log in with the cloud admin’s user account *without* using SQL Injection. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Login Jim | Log in with Jim’s user account. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Login MC SafeSearch | Log in with MC SafeSearch’s original user credentials without applying SQL Injection or any other bypass. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Login Support Team | Log in with the support team’s original user credentials without applying SQL Injection or any other bypass. | [[09 Security Misconfiguration\|💡]] | [[01 Challenge solutions\|📕]] |
| Manipulate Basket | Put an additional product into another user’s shopping basket. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Mass Dispel | Close multiple "Challenge solved"-notifications in one go. | [[18 Miscellaneous\|💡]] | [[01 Challenge solutions\|📕]] |
| Memory Bomb | Drop some explosive data into a vulnerable file-handling endpoint. | [[11 Insecure Deserialization\|💡]] | [[01 Challenge solutions\|📕]] |
| Meta Geo Stalking | Determine the answer to John’s security question by looking at an upload of him to the Photo Wall and use it to reset his password via the Forgot Password mechanism. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Mint the Honey Pot | Mint the Honey Pot NFT by gathering BEEs from the bee haven. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Misplaced IaC Files | Access the misplaced Infrastructure as Code files that were accidentally included in the web application deliverable. | [[09 Security Misconfiguration\|💡]] | [[01 Challenge solutions\|📕]] |
| Misplaced Signature File | Access a misplaced SIEM signature file. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Missing Encoding | Retrieve the photo of Bjoern’s cat in "melee combat-mode". | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Multiple Likes | Like any review at least three times as the same user. | [[15 Broken Anti-Automation\|💡]] | [[01 Challenge solutions\|📕]] |
| Nested Easter Egg | Apply some advanced cryptanalysis to find *the real* easter egg. | [[16 Cryptographic Issues\|💡]] | [[01 Challenge solutions\|📕]] |
| NFT Takeover | Take over the wallet containing our official Soul Bound Token (NFT). | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| NoSQL DoS | Let the server sleep for some time. (It has done more than enough hard work for you) | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| NoSQL Exfiltration | All your orders are belong to us! Even the ones which don’t! | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| NoSQL Manipulation | Update multiple product reviews at the same time. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Outdated Allowlist | Let us redirect you to one of our crypto currency addresses which are not promoted any longer. | [[14 Unvalidated Redirects\|💡]] | [[01 Challenge solutions\|📕]] |
| Password Hash Leak | Obtain the password (hash) of the currently logged-in user directly from a REST API endpoint. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Password Strength | Log in with the administrator’s user credentials without previously changing them or applying SQL Injection. | [[04 Broken Authentication\|💡]] | [[01 Challenge solutions\|📕]] |
| Payback Time | Place an order that makes you rich. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Poison Null Byte | Bypass a security control with a Poison Null Byte to access a file not meant for your eyes. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Premium Paywall | Unlock Premium Challenge to access exclusive content. | [[16 Cryptographic Issues\|💡]] | [[01 Challenge solutions\|📕]] |
| Privacy Policy | Read our privacy policy. | [[18 Miscellaneous\|💡]] | [[01 Challenge solutions\|📕]] |
| Privacy Policy Inspection | Prove that you actually read our privacy policy. | [[13 Security through Obscurity\|💡]] | [[01 Challenge solutions\|📕]] |
| Product Tampering | Change the `href` of the link within the OWASP SSL Advanced Forensic Tool (O-Saft) product description into *[https://owasp.slack.com](https://owasp.slack.com)*. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Reflected XSS | Perform a *reflected* XSS attack with `` <iframe src="javascript:alert(`xss ``)">\`. | [[10 Cross Site Scripting (XSS)\|💡]] | [[01 Challenge solutions\|📕]] |
| Repetitive Registration | Follow the DRY principle while registering a user. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Reset Bender’s Password | Reset Bender’s password via the Forgot Password mechanism with *the truthful answer* to his security question. | [[04 Broken Authentication\|💡]] | [[01 Challenge solutions\|📕]] |
| Reset Bjoern’s Password | Reset the password of Bjoern’s internal account via the Forgot Password mechanism with *the truthful answer* to his security question. | [[04 Broken Authentication\|💡]] | [[01 Challenge solutions\|📕]] |
| Reset Jim’s Password | Reset Jim’s password via the Forgot Password mechanism with *the truthful answer* to his security question. | [[04 Broken Authentication\|💡]] | [[01 Challenge solutions\|📕]] |
| Reset Morty’s Password | Reset Morty’s password via the Forgot Password mechanism with *his obfuscated answer* to his security question. | [[15 Broken Anti-Automation\|💡]] | [[01 Challenge solutions\|📕]] |
| Reset Uvogin’s Password | Reset Uvogin’s password via the Forgot Password mechanism with *the original answer* to his security question. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Retrieve Blueprint | Deprive the shop of earnings by downloading the blueprint for one of its products | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Score Board | Find the carefully hidden 'Score Board' page. | [[02 Finding the Score Board\|💡]] | [[01 Challenge solutions\|📕]] |
| Security Advisory | The Juice Shop is susceptible to a known vulnerability in a library, for which an advisory has already been issued, marking the Juice Shop as *known affected*. A fix is still pending. Inform the shop about a suitable checksum as proof that you did your due diligence. | [[18 Miscellaneous\|💡]] | [[01 Challenge solutions\|📕]] |
| Security Policy | Behave like any "white hat" should before getting into the action. | [[18 Miscellaneous\|💡]] | [[01 Challenge solutions\|📕]] |
| Server-side XSS Protection | Perform a *persisted* XSS attack with `` <iframe src="javascript:alert(`xss ``)">\` bypassing a server-side security mechanism. | [[10 Cross Site Scripting (XSS)\|💡]] | [[01 Challenge solutions\|📕]] |
| SSRF | Request a hidden resource on server through server. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| SSTi | Infect the server with juicy malware by abusing arbitrary command execution. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Steganography | Rat out a notorious character hiding in plain sight in the shop. (Mention the exact name of the character) | [[13 Security through Obscurity\|💡]] | [[01 Challenge solutions\|📕]] |
| Successful RCE DoS | Perform a Remote Code Execution that occupies the server for a while without using infinite loops. | [[11 Insecure Deserialization\|💡]] | [[01 Challenge solutions\|📕]] |
| Supply Chain Attack | Inform the development team about a danger to some of *their* credentials. (Send them the URL of the *original report* or an assigned CVE or another identifier of this vulnerability) | [[12 Vulnerable Components\|💡]] | [[01 Challenge solutions\|📕]] |
| System Prompt Extraction | Extract the chatbot’s system prompt using prompt injection, then submit your findings (including any confidential sections) via the Customer Complaint form. | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Two Factor Authentication | Solve the 2FA challenge for user "wurstbrot". (Disabling, bypassing or overwriting his 2FA settings does not count as a solution) | [[04 Broken Authentication\|💡]] | [[01 Challenge solutions\|📕]] |
| Unsigned JWT | Forge an essentially unsigned JWT token that impersonates the (non-existing) user *jwtn3d@juice-sh.op*. | [[12 Vulnerable Components\|💡]] | [[01 Challenge solutions\|📕]] |
| Upload Size | Upload a file larger than 100 kB. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| Upload Type | Upload a file that has no .pdf or .zip extension. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |
| User Credentials | Retrieve a list of all user credentials via SQL Injection | [[03 Injection\|💡]] | [[01 Challenge solutions\|📕]] |
| Video XSS | Embed an XSS payload `` </script><script>alert(`xss ``)\</script>\` into our promo video. | [[10 Cross Site Scripting (XSS)\|💡]] | [[01 Challenge solutions\|📕]] |
| View Basket | View another user’s shopping basket. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Visual Geo Stalking | Determine the answer to Emma’s security question by looking at an upload of her to the Photo Wall and use it to reset her password via the Forgot Password mechanism. | [[05 Sensitive Data Exposure\|💡]] | [[01 Challenge solutions\|📕]] |
| Vulnerable Infrastructure | Inform the shop about the use of a known vulnerable infrastructure component. (Identify the component as specifically as possible in your complaint) | [[12 Vulnerable Components\|💡]] | [[01 Challenge solutions\|📕]] |
| Vulnerable Library | Inform the shop about a high-severity vulnerability. (Mention the exact library name and version in your comment) | [[12 Vulnerable Components\|💡]] | [[01 Challenge solutions\|📕]] |
| Wallet Depletion | Withdraw more ETH from the new wallet than you deposited. | [[18 Miscellaneous\|💡]] | [[01 Challenge solutions\|📕]] |
| Web3 Sandbox | Find an accidentally deployed code sandbox for writing smart contracts on the fly. | [[08 Broken Access Control\|💡]] | [[01 Challenge solutions\|📕]] |
| Weird Crypto | Inform the shop about an algorithm or library it should definitely not use the way it does. | [[16 Cryptographic Issues\|💡]] | [[01 Challenge solutions\|📕]] |
| XXE Data Access | Retrieve the content of `C:\Windows\system.ini` or `/etc/passwd` from the server. | [[06 XML External Entities (XXE)\|💡]] | [[01 Challenge solutions\|📕]] |
| XXE DoS | Give the server something to chew on for quite a while. | [[06 XML External Entities (XXE)\|💡]] | [[01 Challenge solutions\|📕]] |
| Zero Stars | Give a devastating zero-star feedback to the store. | [[07 Improper Input Validation\|💡]] | [[01 Challenge solutions\|📕]] |

## Challenge Solutions

In case you are getting frustrated with a particular challenge, you can refer to the [[01 Challenge solutions|Challenge solutions]] appendix where you find explicit instructions how to successfully exploit each vulnerability. It is highly recommended to use this option only as a last resort. You will learn **a lot more** from hacking entirely on your own or relying only on the hints in this part of the book.

Hoping you have a great time solving these challenges.

[[06 Walking the happy path|Walking the "happy path"]] [[02 Finding the Score Board|Finding the Score Board]]

## Pages in this section

- [[02 Finding the Score Board|Finding the Score Board]]
- [[03 Injection|Injection]]
- [[04 Broken Authentication|Broken Authentication]]
- [[05 Sensitive Data Exposure|Sensitive Data Exposure]]
- [[06 XML External Entities (XXE)|XML External Entities (XXE)]]
- [[07 Improper Input Validation|Improper Input Validation]]
- [[08 Broken Access Control|Broken Access Control]]
- [[09 Security Misconfiguration|Security Misconfiguration]]
- [[10 Cross Site Scripting (XSS)|Cross Site Scripting (XSS)]]
- [[11 Insecure Deserialization|Insecure Deserialization]]
- [[12 Vulnerable Components|Vulnerable Components]]
- [[13 Security through Obscurity|Security through Obscurity]]
- [[14 Unvalidated Redirects|Unvalidated Redirects]]
- [[15 Broken Anti-Automation|Broken Anti-Automation]]
- [[16 Cryptographic Issues|Cryptographic Issues]]
- [[17 Observability Failures|Observability Failures]]
- [[18 Miscellaneous|Miscellaneous]]
