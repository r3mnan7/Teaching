---
tags: [cs361]
---
### Title slide

Trust, Assume, Break, Steer, Impact

### The spine: Trust, Assume, Break, Steer, Impact

Five words describe how most software bug turns into harm. They appear on every slide. The lecture walks through them in order, and each step ends with a real attack.

| Step | Meaning | Recipe |
| --- | --- | --- |
| Trust | Data from outside the program crosses into it. | Who brought the ingredient in? |
| Assume | The code takes something about that data for granted: its size, its type, who sent it, what it means. | What did the chef assume about the box? (e.g., "It's flour, and it weighs 5 lbs"). |
| Break | The assumption is wrong, and the program reads, writes or runs something it was never meant to. | What happens when it's 50 lbs or gunpowder? (Overflow or explosion). |
| Steer | An attacker shapes that misbehavior so the program does what they want. | Does the kitchen just make a mess (DoS), or did the stranger swap the recipe instructions (RCE)? |
| Impact | The result: a crash, leaked data, or the attacker's commands running. | How much of the restaurant burns down? |

### Opening hook: "512 bytes"

November 2, 1988, 38 years ago today. A Cornell graduate student, Robert Morris, released a self-spreading program that slowed or knocked out a large part of the early internet. One way it spread was a buffer overflow in fingerd, the "finger daemon" \[12\]. fingerd answered one kind of request from anywhere on the network: who is this user, and are they logged in? Morris became the first person convicted under the U.S. Computer Fraud and Abuse Act. Guiding question for the whole lecture: one service trusted that a request would fit in 512 bytes. Why does the same kind of mistake still work in 2026?

|  | Request buffer (512 bytes) | Return address |
| --- | --- | --- |
| Normal request | "who is alice?" | Back to fingerd's main loop |
| The worm's request | The worm's own small program, plus padding | The address of that program, inside the buffer |

Step by step:

1. fingerd read each request with gets(), a C library function that copies input into a buffer with no length limit \[12\].
2. The buffer held 512 bytes. The worm sent more than that.
3. The extra bytes ran past the buffer and overwrote the return address.
4. The new return address pointed back into the buffer, where the worm had placed a small program of its own.
5. When the function returned, the machine ran that program, which opened a shell and pulled down the rest of the worm \[12\].

| Step | The Morris worm |
| --- | --- |
| Trust | Network requests from any machine on the internet reached fingerd |
| Assume | "The data fits": a request will never be longer than 512 bytes |
| Break | gets() copied the whole request, overflowing the buffer |
| Steer | The overwritten return address pointed back into the buffer, at the worm's own code |
| Impact | Control of the machine, then spread: every infected machine attacked others |

### Roadmap

- Part 1: Trust
- Part 2: Assume
- Part 3: Break
- Part 4: Steer
- Part 5: Impact
- Part 6: AI and AppSec

## 1. Trust

A trust boundary is the spot in the code where outside data comes in. Nobody labels it. It is usually just the line right after a read.

### What goes wrong: an unchecked attack surface

Step by step:

1. The program reads bytes from outside: a file, a network message, user input.
2. It hands them straight to the code that parses them and acts on them.
3. Nothing in between validates them. That handoff is the trust boundary, and it is where the checks belong.

#### Where outside data comes in

| Source | Why programmers trust it |
| --- | --- |
| What the user types or passes in | "The user is the one running it" |
| Files and settings | "We wrote that file" |
| Network messages | "Our own app sends them" |
| Browser/client state | "Our frontend already checked it" |
| Other programs and services | "It's our own code on the other side" |
| Updates from your own company | "It came from us" |

- The last two are the dangerous ones because they do not feel like input.
- A digital signature proves who sent the data, not that the data is correct.

### How to defend: validate input and shrink the attack surface

- Check input once, at the door, and reject anything incomplete or malformed. The rest of the program then only sees data that already passed.
- Keep that checking in one place, not scattered through the code.
- Give the code that handles outside data as few permissions as possible. Chrome runs web pages this way, so a bug in one tab reaches very little.
- Keep code out of the kernel unless it truly needs to be there. Microsoft has announced a way for security software to run outside the Windows kernel \[4\].

* Validate on the receiving end, even when the sender already validated.
* Treat configuration, rules and update files as input, with the same checks as anything a user sends.

- Threat modeling: at design time, map where data crosses into the program and decide what each crossing must check. It is the standard first step in a secure development lifecycle.

### Practical application: the Morris worm's front door

- fingerd accepted a request from any machine on the internet and copied it straight into its 512-byte buffer \[12\].
- Nothing checked the request before the program used it: not its length, not who sent it.
- The service itself did almost nothing. But it ran on most Unix machines, so the same unchecked door was open on all of them.
- What would have stopped it: check every request at the boundary before using it, and turn off network services nobody needs, which shrinks the attack surface.

Question: A smart thermostat downloads its heating schedule from the manufacturer's cloud every night. Where are the trust boundaries, and what should the thermostat check before it uses that schedule?

## 2. Assume

Once data is inside, the code treats it as having a certain shape. Almost every bug comes down to one false assumption, and each one has a matching fix.

### What goes wrong: missing input validation

| The code assumes | What goes wrong | Weakness |
| --- | --- | --- |
| The data fits | Reads or writes run past the end of a buffer | Out-of-bounds read or write |
| The data came from us | Deserializing an object from incoming bytes runs code the sender chose | Deserialization of untrusted data |
| Only the right people call this | A sensitive function has no authentication check | Missing authentication |
| The input means what it looks like | "../" in a file path escapes the intended folder; ";" in a shell command starts a new command | Path traversal; OS command injection |
| Nothing changes while I am using it | Data is checked, then changed before it is used | Time-of-check to time-of-use race |

### Trusting client-side state

Another common unsafe assumption is that the server can believe values sent by the browser or mobile app.

Frontend checks are useful for usability: they help honest users avoid mistakes. But they are not a security boundary. The client is controlled by the user, which means request parameters, hidden fields, cookies, local storage, and mobile-app traffic can all be modified before they reach the server.

| Client-side value | Unsafe assumption | What an attacker might change |
| --- | --- | --- |
| Hidden form field: `price=499.00` | "The checkout page calculated the price." | `price=4.99` |
| Cookie: `role=user` | "The cookie tells us the user's role." | `role=admin` |
| URL parameter: `accountId=123` | "Users only request their own account." | `accountId=124` |
| Local storage: `isPremium=false` | "The frontend controls feature access." | `isPremium=true` |
| JWT claims | "The token says the user is admin." | Accepting unsigned, weakly signed, expired, or incorrectly validated claims |

The rule is: validate and enforce security decisions on the receiving end. The server should recompute prices, check authorization using server-side records, verify token signatures, reject expired or malformed tokens, and deny access by default.

#### The most common one: input pasted into a command

In 2025, this was the weakness behind more attacked bugs than any other \[5\].

| The program builds | The shell (command line) sees |
| --- | --- |
| backup Sam | 1. backup Sam |
| backup x; run\_attacker\_script | 1. backup x, then 2. run\_attacker\_script |

Step by step:

1. The program concatenates user input into a command string and passes it to the shell.
2. The shell treats ";" as a separator between commands.
3. The attacker's input becomes a second command, running with the program's permissions.
4. The fix: skip the shell and pass the input as a separate argument, so it is never parsed as part of a command.

#### Two pieces of code, two readings

- Bugs also hide between pieces of code that read the same data differently: a validator accepts input that the code using it cannot handle, or a check reads a value one way while the code that acts on it reads it another way.

### How to defend: secure coding practices

- Check sizes and counts before reading or writing, and reject input that does not match.
- Never deserialize untrusted data into arbitrary objects. Accept plain data, such as JSON, and build the object yourself.
- Put an authentication check in front of every sensitive function, and deny by default.
- Normalize a file path before checking it, then confirm the result is still inside the allowed folder.
- Do not build shell commands from input. Run the program directly and pass input as a separate argument.
- Check and use in one step: open the file, then check what you opened, instead of checking a name and opening it later.
- Treat compiler warnings as errors, and run tests with sanitizers, which catch undefined behavior while the program runs.
- Use the same parser to validate data and to use it, so the two cannot disagree.

### Practical application: Ivanti VPN, 2024

Ivanti Connect Secure lets employees connect to their company network from home. In December 2023, the security firm Volexity noticed strange activity inside a customer's network, traced it back to the Ivanti VPN, and found its logs wiped \[21\]. A China-linked group had chained two bugs to run commands on it without a password \[21\]. The U.S. government ordered federal agencies to act within days \[22\].

| What the attacker asked for | What the login check saw | Where the request ended up |
| --- | --- | --- |
| /public/../../admin | Starts with /public, so no login needed | /admin |

Step by step:

1. The authentication check skips login for any URL path that starts with a public prefix.
2. The attacker's path starts with that prefix, then uses "../.." to move up two directories into the admin area.
3. The check looks at the raw path, sees the public prefix, and lets the request through.
4. The server then normalizes the path, resolving "../..", and runs the admin endpoint.
5. That endpoint concatenates part of the request into a shell command, so a ";" lets the attacker run their own command.

| Step | In this attack | Defense that would have stopped it |
| --- | --- | --- |
| Trust | Network messages from anyone on the internet go straight to the VPN's web service | Give code that handles outside data as few permissions as possible |
| Assume | "The input means what it looks like" (the path) and "only the right people call this" (the admin feature) | Normalize the path before checking it; authenticate every sensitive function, deny by default |
| Break | Path traversal skips the login check; then input is pasted into a command | Do not build shell commands from input |
| Steer | Logic bugs steer themselves: the attacker just writes the command | None of the memory defenses apply |
| Impact | Control: the attacker's commands run on the VPN, with a way into the company network \[21\] | Hold as little power as possible |

Question: A photo app lets users name their uploads, then runs a command-line tool on that file to make a thumbnail. Which of the five assumptions could the code be making, and what would an attacker name their file?

## 3. Break

Break is the moment the wrong assumption meets real data. In a logic bug, a check gets skipped or input becomes a command. In a memory bug, the program reads or overwrites memory that belongs to something else.What goes wrong

#### Logic breaks

- In a logic bug, nothing in memory is corrupted. The program does exactly what the code says; the code just says the wrong thing.
- The break is either a skipped check (a missing login check, a path that escapes its folder) or data turning into code (input pasted into a command, a deserialized object that runs code).
- These happen in every language, including Python and Java. In 2025, input pasted into a command was the most common weakness behind exploited bugs, and missing authentication, deserialization and path traversal were all in the top six \[5\].

#### Memory in one minute

- The stack holds each function call's local variables and its return address: where execution goes when the function returns.
- The heap holds memory the program allocates while it runs (malloc, new) and frees later.
- C and C++ do no bounds checking on arrays and do not track whether freed memory is still in use. Java and Python do, which is why these bugs live mostly in C and C++.

#### Four memory bugs

| Bug | What happens |
| --- | --- |
| Out-of-bounds read | Code reads past the end of a buffer and gets whatever is stored next |
| Buffer overflow | Code writes past the end of a buffer, overwriting whatever is stored next |
| Use-after-free | Code keeps using a pointer after its memory was freed and reused for something else |
| Integer overflow | A size calculation passes the integer's maximum and wraps to a small value, so the buffer allocated is too small `uint8_t size = 255 + 1 → wraps to 0 bytes allocated` |

### How to defend: memory-safe languages and security testing

- Use a memory-safe language for new code. Python, Java and Rust check every index, so an out-of-bounds read like Heartbleed cannot happen by accident.
- It works: as Android wrote new code in safer languages, memory bugs fell from 76% of its security bugs in 2019 to under 20% in 2025 \[15\], \[16\]. Rust is now officially part of the Linux kernel \[17\].
- It does not fix everything: no language stops a missing login check or input pasted into a command.
- Fuzzing: a program that throws millions of random, broken inputs at your code to see what crashes. Google's OSS-Fuzz has found over 13,000 security bugs this way \[18\]. A fuzzer's whole job is to try the inputs nobody thought of.

### Practical application: Heartbleed, 2014

| H | A | T | p | a | s | s | w | o | r | d | ... |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| sent | sent | sent | leaked | leaked | leaked | leaked | leaked | leaked | leaked | leaked | leaked |

Step by step:

1. A heartbeat message contains a payload and a length field. The server echoes the payload back to show the connection is alive.
2. The server uses the length field to decide how much to copy, without checking it against the payload's actual size.
3. The attacker sends a few bytes and claims up to 64 KB.
4. The server copies 64 KB starting at the payload, so the reply includes whatever was stored after it: other users' data, passwords, private keys.
5. The fix: compare the claimed length to the bytes actually received, and drop the message if they do not match.

| Step | Heartbleed | Defense that would have stopped it |
| --- | --- | --- |
| Trust | A heartbeat message from anyone on the internet | Validate input at the boundary |
| Assume | "The data fits": the stated length matches the payload | Check the stated length against the bytes actually received |
| Break | Out-of-bounds read | A memory-safe language; fuzzing the message parser |
| Steer | None needed: the leaked memory is the prize | Exploit mitigations do not stop a read |
| Impact | Leak: private keys, passwords and other users' data \[11\] | Hold as little power as possible: keep secret keys in a separate process the network code cannot read |

Question: A game server written in C copies each chat message into a 256-byte buffer. The official game client never sends more than 200 characters. An attacker writes their own client. What happens, and which bug is it?

## 4. Steer

Steer is what turns a bug into an attack: the attacker shapes the bad input so the program does what they want. For logic bugs this is free. For memory bugs it is the hard part, and modern compilers, operating systems and chips all fight it.

### What goes wrong: exploitation

#### Logic bugs steer themselves

- If user input reaches the command line, the attacker just types the command they want.
- If a login check is missing, the attacker is already in.
- There is nothing to get around, which is why attackers prefer these bugs \[1\].

#### Steering a memory bug

Before:

| Name buffer (16 bytes) | Return address |
| --- | --- |
| Sam | Back to the caller |

After a name that is too long:

| Name buffer (16 bytes) | Return address |
| --- | --- |
| AAAAAAAAAAAAAAAA | An address the attacker chose |

Step by step:

1. The function's stack frame has a 16-byte buffer for the name, and right after it, the return address.
2. The copy does not check the input's length.
3. Input longer than 16 bytes overflows the buffer and overwrites the return address with a value the attacker chose.
4. When the function returns, execution jumps to that address.
5. The attacker now controls what the program runs next.

### How to defend: exploit mitigations

#### Built into your compiler and operating system

These are on by default in modern compilers and operating systems.

| Defense | What it does | How attackers got around it |
| --- | --- | --- |
| Stack canary | The compiler puts a secret value between the buffer and the return address, like a tamper seal, and checks it before returning. If an overflow broke the seal, the program stops | Find a separate bug that reveals the secret |
| Non-executable memory | Data in memory is not allowed to run as code | Reuse small pieces of the program's own code instead |
| Address randomization | The program loads at a random spot in memory each time, so the attacker does not know where to jump | Find a separate bug that leaks one address, then work out the rest |

- The pattern: each workaround needs a second bug, usually one that leaks memory. That is why leaks like Heartbleed matter even on their own.
- Newer chips check memory in hardware. The iPhone 17, for example, catches many memory bugs the moment they happen \[20\].

#### How attackers got around non-executable memory

- Once memory holding data could not run as code, attackers stopped bringing their own code and started stitching together pieces of code the program already has. That is why non-executable memory alone is not a complete fix, and why newer defenses check where the program is allowed to jump.

* Newer defenses check where returns and jumps are allowed to go, which breaks the code-reuse trick. Windows and Linux both support them on current processors.
* These protections only help if they are on. Keep the compiler's default protections in every build, and treat a build that turns them off as a red flag.

#### The limit

- All of these defenses turn control back into a crash. That helps against attackers, but not when the crash is the disaster.
- They also do nothing for logic bugs.

### Practical application: the Morris worm, revisited

- The worm overflowed fingerd's buffer and pointed the return address back into the buffer, where its own code sat \[12\].
- In 1988, none of today's defenses existed.

| Defense | Effect on the Morris worm |
| --- | --- |
| Stack canary | The overwrite is caught before fingerd returns; the service crashes instead of running the worm |
| Non-executable memory | The worm's code sits in the buffer, which is data, so it is not allowed to run |
| Address randomization | The worm cannot know where the buffer is, so it does not know what return address to write |
| A length-limited read or a memory-safe language | The overflow never happens at all |

Question: A C program on a public server has a buffer overflow, and it was built with stack canaries, non-executable memory and address randomization all turned on. What can an attacker still do with that bug, and what can they no longer do?

## 5. Impact

The same bug can end three ways: a crash, a leak, or an attacker in control. How bad it gets depends on design choices made long before the bug showed up.

### What goes wrong: blast radius

| Outcome | What happens | Example |
| --- | --- | --- |
| Crash | The program or the whole computer goes down | CrowdStrike \[2\] |
| Leak | Secrets get out: passwords, keys, login sessions | Heartbleed \[11\] |
| Control | The attacker's commands run with the program's permissions | Ivanti, 2024: attackers ran their own commands on the VPN \[21\] |

- Two things decide how bad it gets: how much power the failing code holds, and how many machines get the failure at once.
- CrowdStrike maxed out both: code running in the kernel, pushed to every customer at the same moment \[2\].

### How to defend: secure design

#### Fail safe and fail secure

- Fail safe: when input is bad, reject that one piece, log it, and keep running on what you already have, instead of crashing.

|  | Fail hard | Fail safe |
| --- | --- | --- |
| A bad rule arrives | The program uses it anyway and crashes | The program notices and skips that one rule |
| Result | Everything stops | Everything else keeps running |
| For the sensor's users | Every computer running the sensor goes down | One detection rule skipped |

Step by step:

1. The fail-hard version uses the input without validating it, so one malformed file takes the whole program down.
2. The fail-safe version validates first.
3. If validation fails, it logs the error with enough detail to debug it.
4. It rejects that one rule and keeps running with the rules it already loaded.

- Fail secure (also called fail closed): when a security check itself errors out, the safe answer is "no." Code that grants access whenever the check errors out hands attackers a way in.

#### Hold as little power as possible

- Run with the fewest permissions the job needs. A web server needs administrator rights only to open its network port; good servers open the port, then drop those rights before reading any input.
- Then a bug in the input handling runs without administrator rights.

#### Don't send a bad version to everyone at once

- Staged rollout: release to 1% of users, watch for crashes, then 10%, 50%, 100%. If something breaks, stop, and most users never see it.
- CrowdStrike sent its file to every customer at once. Afterward, it committed to staged releases and to letting customers choose when updates arrive \[2\].
- This applies to every developer who ships anything: an app update, a website change, a settings file.

### Practical application: CrowdStrike, 2024

July 19, 2024. No attacker was involved. Crowdstrike pushed a routine update to its Falcon sensor EDR, the sensor read memory that was not there, and about 8.5 million Windows computers crashed and would not restart \[2\], \[3\].

- The software: CrowdStrike's Falcon sensor, security software that runs in the Windows kernel so it can watch everything on the machine \[2\].
- The update: Channel File 291, a data file with new detection rules, not new code. It went to every customer at once \[2\].
- The bug: a new rule expected 21 input values. The code that called it passed only 20, and the rule read the 21st anyway, past the end of the array \[2\].
- Why testing missed it: the validator that checks these files had its own bug and passed this one \[2\].
- Impact: every affected machine crashed on every boot and had to be fixed by hand. Mac and Linux machines were not affected \[2\].

| Step | CrowdStrike | Defense that would have stopped it |
| --- | --- | --- |
| Trust | The company's own rules file went straight into kernel software | Treat your own updates as input, and validate them when they load |
| Assume | "The data fits": 21 values will be there | Check counts before reading |
| Break | Out-of-bounds read | Bounds checks; fuzzing the rule parser |
| Steer | Nobody. The bad data was an accident | Exploit mitigations do not help: the crash itself was the damage |
| Impact | A crash on every boot, on about 8.5 million computers, each fixed by hand \[2\], \[3\] | Fail safe: reject the rule instead of crashing; staged rollout; run outside the kernel \[4\] |

Question: A hospital's medication cabinet gets a corrupted drug list from its server. Should it lock (fail secure) or keep dispensing from its last good list (fail safe)? Who gets hurt by each choice?

## 6. AI and AppSec

AI touches application security from two directions. It writes a growing share of the code, and AI features inside applications are a new kind of input handling. Both fit the same spine.

### What goes wrong: AI-written code

- In Veracode's 2025 test of more than 100 AI models on 80 coding tasks, 45% of the generated code introduced a known weakness. Java failed more than 70% of the time, and newer or larger models did no better on security than older or smaller ones \[26\].
- In a Stanford study, developers using an AI assistant wrote less secure code than those without one, and were more likely to believe their code was secure \[27\].
- AI-written code fails the same ways human code does: missing input validation, input pasted into commands, unchecked sizes. It just produces them faster.

### What goes wrong: AI as a new trust boundary

- An application that feeds outside content (emails, web pages, documents) to an AI model creates a new trust boundary. The model cannot reliably tell the developer's instructions apart from instructions hidden in that content.
- This is prompt injection, number one on OWASP's 2025 list of risks for AI applications \[28\]. It is the same failure as input pasted into a command: data turns into instructions.

### How to defend: treat AI like any other untrusted source

- Review AI-generated code the way you would review a new teammate's: same code review, same tests, same fuzzing.
- Treat model output as untrusted input. Validate it before it reaches a shell, a file path or another system.
- Give AI features and agents the least privilege they need, and require a person to approve sensitive actions such as sending data or running commands.
- Use AI on defense too: in 2025, Google's AI agent Big Sleep found a bug that attackers were preparing to use, before they could \[19\].

### Practical application: EchoLeak, 2025

Researchers showed that an email with hidden instructions could get Microsoft 365 Copilot to send a user's sensitive data to an outside server, with no click from the user. Microsoft fixed it as CVE-2025-32711 \[29\].

Question: A company adds an AI assistant that reads customer support emails and can issue refunds. What email would an attacker send, and which defense from this section stops it?

## 7. Lab 2 preview

## 8. Wrap-up and HW 3

### Close: six sentences to leave with

- Anything that comes into code from outside is input.
- Memory-safe languages remove whole kinds of bugs; testing and fuzzing only find them sooner.
- A bug becomes an attack when someone can steer it.
- Attackers go where steering is easiest.
- Bugs will ship, so limit how much any one bug can reach and how far a bad update can spread

### Sources

1. [Google Threat Intelligence Group, Look What You Made Us Patch: 2025 Zero-Days in Review (March 5, 2026)](https://cloud.google.com/blog/topics/threat-intelligence/2025-zero-day-review/)
2. [CrowdStrike, External Technical Root Cause Analysis: Channel File 291 (August 6, 2024)](https://www.crowdstrike.com/wp-content/uploads/2024/08/Channel-File-291-Incident-Root-Cause-Analysis-08.06.2024.pdf)
3. [Microsoft, Helping our customers through the CrowdStrike outage (July 20, 2024)](https://blogs.microsoft.com/blog/2024/07/20/helping-our-customers-through-the-crowdstrike-outage/)
4. [Microsoft, The Windows Resiliency Initiative: Building resilience for a future-ready enterprise (June 26, 2025)](https://blogs.windows.com/windowsexperience/2025/06/26/the-windows-resiliency-initiative-building-resilience-for-a-future-ready-enterprise/)
5. [MITRE, 2025 CWE Top 10 KEV Weaknesses](https://cwe.mitre.org/top25/archive/2025/2025_kev_list.html)
6. [Fortinet PSIRT, FG-IR-24-423: Missing authentication in fgfmsd (CVE-2024-47575)](https://fortiguard.fortinet.com/psirt/FG-IR-24-423)
7. [Palo Alto Networks, CVE-2024-3400 PAN-OS: Arbitrary File Creation Leads to OS Command Injection Vulnerability in GlobalProtect](https://security.paloaltonetworks.com/CVE-2024-3400)
8. [Fortra, FI-2025-012: Deserialization Vulnerability in GoAnywhere MFT's License Servlet](https://www.fortra.com/security/advisories/product-security/fi-2025-012) and [Microsoft, Investigating active exploitation of CVE-2025-10035 (October 6, 2025)](https://www.microsoft.com/en-us/security/blog/2025/10/06/investigating-active-exploitation-of-cve-2025-10035-goanywhere-managed-file-transfer-vulnerability/)
9. [Dirty COW (CVE-2016-5195)](https://dirtycow.ninja/)
10. [LWN, Fun with NULL pointers, part 1 (July 20, 2009)](https://lwn.net/Articles/342330/)
11. [OpenSSL Security Advisory, TLS heartbeat read overrun, CVE-2014-0160 (April 7, 2014)](https://openssl-library.org/news/secadv/20140407.txt)
12. [RFC 1135, The Helminthiasis of the Internet (December 1989)](https://www.rfc-editor.org/rfc/rfc1135.html)
13. [Google Project Zero, A deep dive into an NSO zero-click iMessage exploit: Remote Code Execution (December 15, 2021)](https://projectzero.google/2021/12/a-deep-dive-into-nso-zero-click.html)
14. [Microsoft Security Response Center, A proactive approach to more secure code (July 16, 2019)](https://www.microsoft.com/en-us/msrc/blog/2019/07/a-proactive-approach-to-more-secure-code)
15. [Google Security Blog, Eliminating Memory Safety Vulnerabilities at the Source (September 25, 2024)](https://security.googleblog.com/2024/09/eliminating-memory-safety-vulnerabilities-Android.html)
16. [Google, Rust in Android: move fast and fix things (November 13, 2025)](https://blog.google/security/rust-in-android-move-fast-fix-things/)
17. [LWN, The state of the kernel Rust experiment (December 13, 2025)](https://lwn.net/Articles/1050174/)
18. [OSS-Fuzz repository](https://github.com/google/oss-fuzz)
19. [Google, A summer of security: empowering cyber defenders with AI (July 15, 2025)](https://blog.google/technology/safety-security/cybersecurity-updates-summer-2025/)
20. [Apple Security Research, Memory Integrity Enforcement (September 9, 2025)](https://security.apple.com/blog/memory-integrity-enforcement/)
21. [Volexity, Active Exploitation of Two Zero-Day Vulnerabilities in Ivanti Connect Secure VPN (January 10, 2024)](https://www.volexity.com/blog/2024/01/10/active-exploitation-of-two-zero-day-vulnerabilities-in-ivanti-connect-secure-vpn/)
22. [CISA, ED 24-01: Mitigate Ivanti Connect Secure and Ivanti Policy Secure Vulnerabilities](https://www.cisa.gov/news-events/directives/ed-24-01-mitigate-ivanti-connect-secure-and-ivanti-policy-secure-vulnerabilities)
23. [CISA, #StopRansomware: LockBit 3.0 Ransomware Affiliates Exploit CVE 2023-4966 Citrix Bleed Vulnerability (AA23-325A)](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-325a)
24. [Assetnote, Citrix Bleed: Leaking Session Tokens with CVE-2023-4966](https://www.assetnote.io/resources/research/citrix-bleed-leaking-session-tokens-with-cve-2023-4966)
25. [GitHub Security Lab, Sign in as anyone: Bypassing SAML SSO authentication with parser differentials (March 12, 2025)](https://github.blog/security/sign-in-as-anyone-bypassing-saml-sso-authentication-with-parser-differentials/)
26. [Veracode, AI-Generated Code Poses Major Security Risks in Nearly Half of All Development Tasks (July 30, 2025)](https://www.veracode.com/press-release/ai-generated-code-poses-major-security-risks-in-nearly-half-of-all-development-tasks-veracode-research-reveals/)
27. [Perry, Srivastava, Kumar and Boneh, Do Users Write More Insecure Code with AI Assistants? (ACM CCS 2023)](https://arxiv.org/abs/2211.03622)
28. [OWASP Gen AI Security Project, LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
29. [Microsoft Security Response Center, CVE-2025-32711: M365 Copilot Information Disclosure Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-32711)
