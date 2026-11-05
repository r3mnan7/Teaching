# Lab: Working Through OWASP Juice Shop with OWASP ZAP

**Scope:** AI/LLM hacks are out of scope for this lab. Skip any challenge that involves the Juice Shop chatbot, such as *Chatbot Prompt Injection* and *Greedy Chatbot Manipulation*.

**Environment:** This lab runs in the cyber range. You will use the pre-built Kali box and the Juice Shop instance hosted there.

- Cyber range access: [insert cyber range login instructions]
- Kali box: [insert Kali box access details]
- Juice Shop URL: [insert cyber range Juice Shop URL]

## Getting Started with ZAP

ZAP (Zed Attack Proxy) is a free, open-source web application security testing tool maintained by Checkmarx. It sits between your browser and the application, so you can inspect and modify the traffic that passes between them.

- **Only test systems you have permission to test.** Active attacks can damage an application's functionality or data. Safe Mode reduces that risk, at the cost of some functionality.
- **Quick Start** runs an automated scan on a target URL. It crawls the site, passively scans responses, and then actively attacks the pages and parameters it finds.
- **Manual Explore** opens a browser through ZAP so you can browse the application yourself. Automated tools miss login-protected pages and can't fill in forms meaningfully, so manual exploration is important.
- **The main window** has a menu bar, a toolbar, a Sites tree, a workspace, an Information window, and a footer that shows alert counts. Press F1 or use the Help menu for help.
- **The Alerts tab** lists findings by risk level. Selecting an alert shows its URL and details.

For more background, read the official [ZAP Getting Started guide](https://www.zaproxy.org/getting-started/).

**Class documentation:** [insert link to class documentation on GitHub]

## Part 1: Installing and Setting Up OWASP ZAP on Your Kali Box

1. Log in to your Kali box in the cyber range.
2. Update the package index and upgrade installed packages:
   ```bash
   sudo apt update -y && sudo apt upgrade -y
   ```
3. Install ZAP:
   ```bash
   sudo apt install zaproxy
   ```
4. Launch ZAP:
   ```bash
   zaproxy
   ```
5. When prompted, choose whether to persist the session. Persisting it is recommended, so you are protected against data loss if the system is rebooted or shut down.
6. Click **Manual Explore** and enter the Juice Shop URL from the environment section above.
7. Enabling the HUD is optional. This guide assumes you did not enable it. The in-app tutorial explains how to use the HUD.
8. Click **Launch Browser**. The browser should open and load Juice Shop.

## Part 2: Lab Objectives

Juice Shop includes a scoreboard that tracks your progress.

**Challenge 1 (required): Find the Score Board.** Finding the scoreboard is the first challenge. Complete it before any other challenge. You will need the scoreboard visible for your screenshot.

Take a screenshot of the scoreboard. That screenshot determines your grade:

| Scoreboard screenshot shows | Points |
|---|---|
| Scoreboard only, no challenges complete | 4 |
| Scoreboard and one challenge complete | 10 |
| Scoreboard and two challenges complete | 14 |
| Scoreboard and three challenges complete | 20 (maximum) |
| Scoreboard and five challenges complete | Extra credit: 10 points |

Extra credit is all or nothing: you must complete all five challenges to receive it.

[Confirm the grade for four challenges. It is listed here as 20, the maximum, but you may want it shown.]

**Screenshot requirements:** [Describe what the screenshot must show. For example: the Score Board page, the browser address bar, and the solved-challenge badges all visible in one frame.]

## Part 3: Using ZAP to Solve Challenges

ZAP is useful for many challenges. A few starting points:

- **Break** (the intercept toggle on the toolbar) pauses requests so you can inspect and change them before they reach the server.
- **Manual Request Editor** lets you edit a captured request and resend it.
- **History** lists every request and response in the session, so you can search for values you saw in the browser.
- **Sites** shows the site map that ZAP has discovered.

## Submission

[Describe how and when to submit the scoreboard screenshot, and the deadline.]
