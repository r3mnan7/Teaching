### Title slide

CS 361, Lecture 4: AppSec Part 2

One round trip. Five places to lose it.

Browser, Request, Server, Data, Response

### Cold open: the internet used to be fun

Image: the "old internet vs. new internet" meme. Left side, a GeoCities-era page: tiled starfield background, rainbow `<marquee>` text, a spinning "Under Construction" GIF, a hit counter reading 000417, a guestbook link, a webring badge. Right side, a 2026 landing page: white background, one sans-serif headline, a hero photo of three people laughing at a laptop, a cookie banner, a "Sign in with Google" button, and a pop-up asking for an email address.

Image: a MySpace profile circa 2006. Glitter background, custom cursor, a song autoplaying from the profile player, a "Top 8" friends grid, an About Me section in three fonts and four colors.

Image: five current websites from five different industries, side by side, visually indistinguishable from one another.

The rant, roughly three minutes:

- There was a time when the internet belonged to people. You wanted a website, you got a website. It had a tiled background of flames, or stars, or your dog. It had a MIDI file of a song you liked that started playing the second the page loaded, at full volume, at 2 a.m., while your parents were asleep.
- It had a hit counter, so you could see that 417 people had visited, and 400 of them were you. It had a guestbook. It had a webring: a little banner at the bottom that said "this is one of 40 sites about Star Trek ship schematics, click here for the next one," and you clicked, and it was a stranger's site about Star Trek ship schematics, and it was *also* covered in flames.
- Every page was under construction, forever, and that was the point. Nothing was finished, because a person was still building it.
- On MySpace, your profile was yours. You picked the background. You picked the font. You picked a font color that matched the background so nobody could read your About Me, which was a choice, and you made it. And your Top 8 was a political document. Friendships ended over the Top 8.
- A whole generation of web developers learned the trade by pasting code they didn't understand into a box labeled "About Me," because they needed the glitter to go *behind* the text. Nobody assigned it. Nobody graded it. People learned HTML and CSS because they wanted their corner of the internet to look like them.
- Now look at the internet. Every website is the same website. White background. One sans-serif font. Three people laughing at a laptop. A headline that says "Reimagine your workflow." A button that says "Get started." You cannot tell whether it sells accounting software, dog food, or a cryptocurrency.
- Before you can read anything, a cookie banner. Then a pop-up for your email. Then "Sign in with Google." You can't change anything. You get one picture, cropped to a circle. Your "page" is a row in someone else's database, rendered in someone else's template, with someone else's ads down the side.
- The internet used to be a garage full of weird stuff people built. Now it's an airport.

Okay. But seriously.

There's a reason the internet looks like an airport, and it isn't *only* that the marketing department won. Look at that MySpace profile again. "Paste whatever HTML and CSS you want into this box, and we'll show it to everyone who visits" was the most fun feature the web ever had. It is also, almost word for word, the definition of the most dangerous software weakness in the world. In October 2005, one 19-year-old put a few lines into that box, and it took MySpace down in under a day. Some of the blandness is the scar tissue.

### Last time, this time

Lecture 3 gave five words for how one program on one machine turns a bug into harm: Trust, Assume, Break, Steer, Impact. Most software written today is not one program on one machine. It is a web application: a browser on a computer you don't control, talking across a network you don't control, to a server you do control, with a database behind it, and a hundred libraries you didn't write holding it all up. The Part 1 spine still applies at every step, but a web app has a shape of its own, and its bugs cluster at specific spots in that shape. This lecture follows one HTTP request around the loop and stops at each place it can go wrong.

Two things to hold onto from Part 1. First, "input pasted into a command" was the most-exploited weakness of 2025. The two most famous web bugs are that exact pattern at two specific stops: SQL injection is input pasted into a command at the Data stop, where the command language is SQL; cross-site scripting is input pasted into a command at the Response stop, where the command language is HTML and JavaScript. Second, "the client belongs to the user." Today adds the other half: the user's browser also belongs to whoever can get code to run inside it.

### The spine: Browser, Request, Server, Data, Response

Five stops describe one round trip. They appear on every slide. Each part of the lecture is one stop, and each ends with a real attack and the question that stop should make you ask.

| Stop | What happens there | The question to ask |
| --- | --- | --- |
| Browser | The user's browser holds the page, the cookies, and any script the page brought with it. | Who else can run code in here? |
| Request | The browser sends an HTTP request across the network: method, path, headers, cookies, body. | Who can write any part of this? |
| Server | The application decides who the sender is, what they may do, and what the input means. | What does the code decide, and what does it skip? |
| Data | The application turns the request into a query against a database or another backend. | Where do data and commands share a sentence? |
| Response | The server sends back HTML, JSON, and headers. The browser interprets and obeys them. | What will the browser do with this? |

Every stop is a trust boundary in the Part 1 sense: a place where data crosses from something you don't control into something you do, or the reverse. The web just has more of them, in a fixed order, and attackers know the order.

### Opening hook: "but most of all, samy is my hero"

October 4, 2005, 12:34 p.m. Samy Kamkar, 19 years old, adds a few lines to his MySpace profile \[2\]. MySpace let users decorate their profiles with their own HTML and CSS, and it tried to make that safe with a filter: it stripped `<script>` tags, removed the word "javascript," and deleted quotation marks and a list of dangerous words like `innerHTML` and `onreadystatechange` \[1\].

Samy got around every one of those. He put his JavaScript inside a CSS `url()` value, which the filter allowed and which Internet Explorer would execute. He split the forbidden word across a line break, `java\nscript`, and the browser quietly joined it back together. He built quotation marks at run time from character codes so there were none in the stored text for the filter to find, and he split other banned words across string concatenations \[1\].

When anyone viewed his profile, the code ran in the viewer's browser, inside the viewer's own logged-in MySpace session. It did three things. It sent a request to add Samy as a friend. It appended "but most of all, samy is my hero" to the viewer's own list of heroes. And it appended a copy of itself to the viewer's profile, so the next person to view *that* profile ran it too \[1\]. MySpace had an anti-forgery token on its forms; the worm simply fetched the page containing the token first, extracted it, and included it, because it was running inside MySpace's own origin and could read anything MySpace served \[1\].

| Time, Oct. 4–5, 2005 | Friend requests |
| --- | --- |
| 12:34 p.m., release | 0 |
| 8:35 a.m. next morning | 221 |
| 1:30 p.m. | 6,373 |
| 6:20 p.m. | 917,084 |
| A few minutes later | 1,005,831 |

More than a million friend requests in under 20 hours, every one from a different logged-in user \[2\]. Around 7 p.m. MySpace went offline. When it came back the worm was gone, but "samy is my hero" stayed on profiles across the site \[2\]. Samy later pleaded guilty to a felony and was sentenced to probation with restrictions on his computer use.

Nothing in memory was corrupted. No exploit mitigation had to be beaten. One profile field that the server stored and the browser trusted, and the fastest-spreading worm in history up to that point.

| Stop | What the worm did there |
| --- | --- |
| Browser | The viewer's browser ran Samy's script with every power of a logged-in MySpace user \[1\]. |
| Request | The script's requests carried the viewer's own session cookie, so MySpace treated them as the viewer's actions. The anti-forgery token was no obstacle because the script could read it \[1\]. |
| Server | The profile editor accepted HTML and CSS and tried to remove the dangerous parts with a list of banned words \[1\]. |
| Data | The worm's code was saved into the profile database, where it waited for the next viewer. |
| Response | MySpace sent the stored profile to every viewer as live HTML, exactly as saved \[1\]. |

Guiding question for the lecture: every stop had a defense available in 2005, and none was used. Twenty-one years later, cross-site scripting is still the number one entry on the MITRE Corporation's list of the most dangerous software weaknesses, the CWE Top 25 (Common Weakness Enumeration, the catalog of software weakness types that MITRE maintains for the U.S. government) \[3\]. Why does a mistake this old still work?

### The historical picture

The web was not designed to run applications. It was designed to serve documents, and every security feature it has was bolted on after someone found a way to abuse the one before it.

- 1995: Netscape ships cookies and JavaScript in the same year. Cookies give HTTP a memory; JavaScript lets a page run code. Everything in this lecture descends from those two features and the same-origin policy that was added to keep them apart.
- 1998: A researcher writing as "rain forest puppy" publishes the first widely read description of SQL injection, against Microsoft's web server and SQL Server \[27\].
- 2000: The CERT Coordination Center at Carnegie Mellon, the original computer emergency response team, publishes its first advisory on what would be called cross-site scripting. The attack already existed; the name arrived with the advisory.
- 2003: The first OWASP Top 10, from the Open Worldwide Application Security Project, the nonprofit whose ranked list of web risks became the industry's shared vocabulary. Injection and cross-site scripting (XSS) are on it. They have been on every edition since \[4\].
- 2005: Samy. Stored XSS plus the browser's automatic cookies equals a worm.
- 2010: Firesheep, a free Firefox extension, lets anyone on a coffee-shop Wi-Fi click a name and take over that person's Facebook session, because most sites still only encrypted the login page. The industry's move to HTTPS everywhere starts here.
- 2014: Heartbleed (Part 1). A memory bug in the library that encrypts most of the web.
- 2017: Equifax. An unpatched web framework and 145.5 million people \[15\].
- 2019: Capital One and First American. Not a bug in anybody's code: a server that would fetch any URL, and a server that never asked who was asking \[9\], \[11\].
- 2020: Chrome makes `SameSite=Lax` the default for cookies, closing the simplest form of cross-site request forgery for everyone at once \[18\].
- 2021: Log4Shell. A logging library in a large share of the world's Java software will fetch and run code from a URL in a log line \[16\].
- 2023: MOVEit and Citrix Bleed. A SQL injection in a file-transfer product reaches 2,773 organizations; a memory leak hands out live session tokens to anyone who asks \[12\], \[14\], \[7\].
- 2024: Polyfill.io. A script on 100,000 sites changes owners, then changes behavior \[6\].
- 2025: OWASP adds Software Supply Chain Failures to its Top 10 at number three \[4\]. Brave shows an AI browser reading a Reddit comment and handing over the user's account \[17\].
- 2026: The npm ecosystem has a year. The Axios HTTP library, downloaded over 100 million times a week, ships two malicious versions in March \[28\]. In August, ChainDrop, a self-propagating worm, uses stolen publishing tokens to republish more than 400 packages with itself inside \[29\]. And Verizon's 2026 breach report finds that for the first time in its 19-year history, exploiting a vulnerability has overtaken stolen credentials as the leading way attackers get in \[30\].

The pattern across thirty years: the attacks don't get cleverer so much as the targets get bigger. Samy reached one site. MOVEit reached thousands of organizations through one product. ChainDrop reached every project that installed any of 400 packages. The same five stops, at larger and larger scale.

### Roadmap

- Part 1: Browser
- Part 2: Request
- Part 3: Server
- Part 4: Data
- Part 5: Response
- Part 6: Around the app
- Part 7: AI and web application security

## 1. Browser

The browser is where the page lives. It is a program running on hardware you don't own, with input you don't control, next to tabs you can't see. Two facts explain everything in this part: the browser's security model protects *origins*, not users, and anything that gets to run inside your origin has all of your origin's power.

### What the browser enforces: the same-origin policy

An origin is scheme plus host plus port: `https://moodle.calvin.edu:443`. Two pages are the same origin only if all three match \[22\]. The same-origin policy is the browser's one big rule: script running in a page from origin A cannot read the contents, cookies, or responses of origin B. It is the reason a news site open in one tab cannot read your bank account open in another, and it is the reason the web can be used at all.

What the policy does not do is stop a page from *sending* requests to other origins. Any page can contain a form that posts to your bank, or an image tag that fetches from your bank, and the browser will send the request. It will even attach your bank cookies to it, because the request is going to the bank and the cookies belong to the bank. Reading the response is blocked; sending the request is not. Much of this part lives in that gap.

### Cookies: the browser is the session courier

HTTP has no memory. Each request stands alone, and the server has no built-in way to know that this request and the one three seconds ago came from the same person. A session cookie solves that: you log in once, the server generates a long random token and sends it in a `Set-Cookie` header, and the browser sends it back with every later request to that site. The server looks the token up and knows who you are.

The browser attaches cookies automatically. It does not ask which page wrote the request, whether the user meant to send it, or whether the request is going out over an encrypted connection. Each of those gaps has an attribute the server can set to close it, and each attribute exists because someone abused the gap first.

| Attribute | What it does |
| --- | --- |
| `Secure` | Only sent over HTTPS. Without it, the cookie goes out in plaintext the first time the user types `http://`. |
| `HttpOnly` | Script in the page cannot read it. An XSS payload that tries `document.cookie` gets nothing. |
| `SameSite=Lax` or `Strict` | Not sent on requests that start from another site. Chrome made `Lax` the default in 2020, which closed the simplest cross-site request forgery for every site that had never heard of it \[18\]. |
| `Path` and `Domain` | Limit which URLs and subdomains receive it. A cookie scoped to `.calvin.edu` goes to every subdomain, including the one a student runs. |
| `Max-Age` and `Expires` | How long the browser keeps it. A session cookie with no expiry lives until the browser closes, which on a phone can be months. |

### What goes wrong: client state, revisited

Part 1 covered the hidden price field, the `role=admin` cookie, the `accountId` parameter, local storage, and JWT claims (JSON Web Token, a signed token that carries claims such as a user's identity and role). The rule was that frontend checks are for usability, not security, because the client belongs to the user. That rule stands, and today adds the second half. The browser belongs to the user, and the browser also belongs to anyone who can get script to run in it. If an attacker's code is running in the user's browser inside your origin, then from the server's point of view that code *is* the user. Everything the user could do, the attacker's code can do, and the server cannot tell the difference.

### What goes wrong: cross-site scripting

Cross-site scripting (XSS) is when an attacker gets a website to run the attacker's JavaScript in other users' browsers. It is number one on MITRE's 2025 CWE Top 25, the list of the most dangerous software weaknesses \[3\]. The name is historical: early attacks loaded the script from another site, but today the script usually comes from the vulnerable site itself.

There are three kinds, distinguished by how the attacker's text travels.

| Kind | Where the attacker's text travels | Example |
| --- | --- | --- |
| Reflected | In the request. The server echoes it straight back in the response. | A search page that prints "Results for: `<script>…`". The attacker sends the victim a link. |
| Stored | Saved by the server. Served to everyone who views it later. | A profile, a comment, a product review, a support ticket. Samy. |
| DOM-based (Document Object Model, the browser's in-memory representation of the page that JavaScript reads and changes) | Never touches the server. JavaScript already in the page reads part of the URL and writes it into the page. | `document.getElementById('msg').innerHTML = location.hash` |

Reflected XSS needs the victim to click something, so it is usually delivered by phishing. Stored XSS needs nothing; the victim just visits a page they would have visited anyway. DOM-based XSS is the one that survives a perfectly secure server, because the server never saw the dangerous text; it went from the address bar to the page without a round trip.

### What script in your origin can do

Once an attacker's script is running in your origin, the same-origin policy is on their side. Their script is same-origin. There is nothing for the policy to block.

- Read anything on the page, including data the page loaded in the background that the user never saw.
- Read cookies, unless they are `HttpOnly`, and send them anywhere. With the session cookie, the attacker is logged in as the victim from their own machine.
- Make requests to your server as the user. The browser attaches the session. The server sees a logged-in user doing something a logged-in user is allowed to do.
- Rewrite the page. Replace the login form with one that posts to the attacker. Add a "your session expired, please re-enter your password" box. The URL bar still says your site.
- Record keystrokes, which is how card-skimming works on checkout pages.
- Copy itself into anything the user can save. That is how one profile became a million.

`HttpOnly` stops the cookie theft and nothing else. The script does not need the cookie; it can make the requests directly, with the browser attaching the cookie for it. This is why the real fix for XSS is at the Response stop, where the script gets into the page in the first place.

### What goes wrong: the scripts you invited in

Every `<script src="https://cdn.example.com/widget.js">` tag runs that script with every power of your origin, exactly as if you had written it. Analytics, chat widgets, ad tags, A/B testing tools (which show two versions of a page to compare results), fonts that come with JavaScript, polyfills for old browsers. A typical commercial site loads dozens. In trusting each one you are trusting the company that wrote it, the CDN that serves it (content delivery network, a third-party service that hosts files close to users), whoever buys that company, and whoever buys the domain when the company forgets to renew it.

Polyfill.io, June 2024. Polyfill was an open-source library that added modern features to old browsers, loaded from `cdn.polyfill.io` by more than 100,000 sites, including JSTOR, Intuit, and the World Economic Forum \[6\]. In February 2024 a Chinese company bought the domain and the GitHub account. In June the script began redirecting visitors to a sports-betting site, but carefully: only mobile browsers, never Windows or Mac desktops, never visitors with admin cookies, with a probability that varied by hour of day, and only after a delay if an analytics script was present, so that the people most likely to notice were the people least likely to see it \[6\]. The script was generated dynamically per request based on the visitor's headers, so what a security researcher saw was not what a victim saw \[6\].

Magecart is the collective name for a family of criminal groups that put card-skimming JavaScript on checkout pages, either by compromising the site itself or by compromising a third-party script the site loads. The customer types into the real form. The script copies each keystroke to a server the attacker controls. The order goes through normally. Nobody notices for weeks.

### What goes wrong: cross-site request forgery

The browser sends your cookies with any request to a site, regardless of which page started the request. Cross-site request forgery, CSRF, is the attacker's page using that fact.

The attacker's page contains a form whose action is `https://bank.example/transfer`, with hidden fields `to=attacker` and `amount=500`, and a line of JavaScript that submits it on load. The victim visits the attacker's page while logged in to the bank, in another tab or just earlier that day. The browser submits the form to the bank, attaches the bank cookie, and the bank sees a logged-in user asking to send $500. CSRF is number three on the 2025 CWE Top 25 \[3\].

Two cousins use the same fact in different ways.

- Clickjacking: the attacker loads your site inside an invisible frame positioned over a "Click here to win" button on their page. The user clicks what looks like the prize button and actually clicks your site's "Delete account" or "Confirm payment" button, with their own session.
- Open redirect: `https://yoursite.edu/login?next=https://evil.example`. The user sees the first half of the URL, trusts it, and is handed to the attacker's site, often a copy of your login page. This is not a browser bug; it is your server forwarding the user anywhere the `next` parameter says.

### How to defend: make the browser work for you

The browser will enforce a great deal on your behalf, but only when the server asks.

- Session cookies get `Secure`, `HttpOnly`, and `SameSite=Lax` or `Strict`. Three attributes close most of this part. `SameSite` alone stops the simple auto-submitting CSRF form, because the cookie is not attached to a request that started on another site.
- Anti-CSRF tokens for anything that changes state. The server puts a random token in every form, and checks it on submission. The attacker's page cannot read the token because of the same-origin policy, which is finally working for you instead of against you. Every major framework does this; the common failure is a developer turning it off for one endpoint because it was "breaking the API."
- Subresource Integrity (SRI) for scripts loaded from elsewhere: `<script src="…" integrity="sha384-…">`. The browser computes the hash of what it downloaded and refuses to run it if the hash doesn't match. Polyfill.io's new owner could not have served different code to any page using SRI; the page would have refused it \[6\].
- Host scripts yourself, and load none from third parties on pages that handle passwords or payment cards. If a vendor insists on loading from their CDN, that is a reason to find another vendor for the checkout page.
- `frame-ancestors` in Content Security Policy, or the older `X-Frame-Options`, tells the browser nobody may put your site in a frame. That ends clickjacking.
- Validate redirect targets. Only redirect to paths on your own site, or to a short whitelist of exact hosts. Never to whatever is in the parameter.

### Practical application: British Airways, 2018

From June 22 to September 5, 2018, about 429,000 British Airways (BA) customers had their payment card details copied as they typed them into BA's own checkout page \[5\]. The attack began nowhere near the website. The attackers used credentials belonging to an employee of a cargo contractor to log in to BA's Citrix remote-access gateway, which had no multi-factor authentication despite BA's own policy requiring it. They broke out of the restricted Citrix environment into the wider network, found a file containing domain administrator credentials stored in plaintext in a script, and used it to reach the web servers \[5\].

There they edited one JavaScript file that BA's site served to every customer. The modified script watched the payment form, copied name, address, card number, expiry, and CVV (the card verification value, the three-digit code on the back) as the customer typed, and sent them to `baways.com`, a domain the attackers had registered so that the traffic would look like BA's own \[5\]. The real transaction completed normally. Fifteen days passed before a third party told BA, because BA had no file integrity monitoring on its web servers and did not detect the change \[5\].

The regulator also found a second problem on the way through: a testing feature left switched on since December 2015 had been logging full card numbers and CVVs in plaintext, a separate failure of both testing and data minimization \[5\]. The fine was £20 million, reduced from a proposed £183 million \[5\].

| Stop | In this attack | Defense that would have helped |
| --- | --- | --- |
| Browser | A script in BA's own origin read the checkout form as customers typed | Content Security Policy forbidding inline script and unlisted script sources on payment pages |
| Request | Card data went to `baways.com` in a request the customer's browser made | A Content Security Policy (CSP) `connect-src` and `form-action` whitelist, so the browser refuses to send data to an unlisted domain |
| Server | The website's JavaScript file was changed on the server | File integrity monitoring; alerts on any change to served code |
| Data | A test log had been collecting CVVs for three years | Never log secrets; audit what logs contain |
| Response | BA served the modified script to every customer for 15 days | SRI on served scripts; change detection on responses |

Question: A campus events site adds a free "live chat with the organizers" widget by pasting one `<script>` tag from the vendor into every page, including the ticket checkout. What can that vendor's script see and do, and what would you ask for before approving it?

## 2. Request

Everything in an HTTP request is text the client wrote. The browser writes most of it honestly, but the browser is only one client. `curl`, a Python script, and an intercepting proxy are others, and they write whatever the attacker types. "Our app doesn't send that" is not a defense, because your app is not the only thing that can send a request.

### Anatomy of a request

```
POST /api/transfer HTTP/1.1
Host: bank.example
Cookie: session=7f3a9c…
Referer: https://bank.example/account
User-Agent: Mozilla/5.0 …
X-Forwarded-For: 10.0.0.5
Content-Type: application/json

{"to": "alice", "amount": 500}
```

Every line is attacker-writable: the method, the path, the query string, each header, the cookie, and the body. The only thing in the request the server can rely on is the session token, and only because the server generated it, it is long and random, and the server can look it up. Everything else is a claim.

### What goes wrong: trusting headers

Developers trust headers because the browser usually fills them in honestly, and the browser is what they tested with.

| Header | Why code trusts it | How it breaks |
| --- | --- | --- |
| `Referer` | "Only our own pages link here, so check that the referrer is us." | The client sets it. An attacker's script or proxy sets it to whatever passes. Many privacy tools strip it entirely, so legitimate users fail the check. |
| `X-Forwarded-For` | "That's the client's real IP address, and we whitelist admin access by IP." | The client can include this header in the request. Only the copy your own proxy adds is meaningful, and only if the proxy overwrites what came in rather than appending to it. |
| `Host` | "That's our own domain name. We build absolute links from it." | A password-reset email with a link built from the `Host` header sends the reset token to whatever domain the attacker put in the header. |
| `Content-Type` | "They said it's JSON, so parse it as JSON." | The body can be anything. Parse as what you expect, and reject anything else. |
| `User-Agent` | "Bots identify themselves." | Nobody has to. |

The rule: a header is a claim. Make security decisions only on things the client cannot forge: the session the server issued, the TLS connection, and your own server-side records.

### What goes wrong: the wire

Without HTTPS, everything in the request is readable by anyone on the same network: the hotel Wi-Fi, the campus network, the internet service provider. That includes the session cookie, and whoever has the session cookie is the user. In 2010 a free Firefox extension called Firesheep made this a one-click attack: it listened to the coffee-shop network, listed everyone's Facebook and Twitter sessions by name, and let you become any of them by double-clicking. Most sites at the time encrypted the login page and nothing else, so the password was safe and the session that the password unlocked was not. That extension did more for HTTPS adoption than a decade of security advice.

Partial HTTPS is still a leak. One `http://` image on an otherwise secure page, one redirect chain that starts in plaintext, and the cookie goes out in the clear on that one request. HTTP Strict Transport Security, HSTS, is a response header that tells the browser "never talk to me over plain HTTP again, for the next year." After the browser has seen it once, it upgrades every request to that site to HTTPS itself, before the request leaves the machine, so there is no plaintext request for anyone to intercept \[23\].

### What goes wrong: the session is the login

After login, the server never sees the password again. It sees the session token. Whoever holds the token is the user. Multi-factor authentication happened once, before the token was issued; it does not protect the token afterward. That makes the token the most valuable thing in the request, and there are many ways to lose it.

- Predictable tokens. Sequential numbers, timestamps, the user ID encoded in base64. Anyone who can guess the pattern can guess the next user's session.
- Tokens in the URL. `https://site/account?session=7f3a9c` lands in the browser history, in proxy logs, in the server's own access logs, and in the `Referer` header sent to every third-party resource on the page. The analytics vendor now has your users' sessions.
- No expiry, or expiry measured in months. A stolen token works until the user logs out, and most users never log out.
- Logout that clears the cookie in the browser but leaves the token valid on the server. The user thinks they logged out. The attacker who copied the cookie earlier is still in.
- Session fixation. The attacker gets a token issued to *themselves* before the victim logs in, plants it in the victim's browser via a link or a cookie set from a subdomain, and waits. If the server keeps using that same token after the victim authenticates, the attacker's token is now a logged-in session.
- A server bug that leaks memory containing other users' tokens. That is Citrix Bleed, below.

### What goes wrong: abuse at request speed

Requests are cheap, and anything the server will do once it will do ten thousand times a minute for anyone who asks.

- Credential stuffing: billions of leaked username-and-password pairs from other sites' breaches, tried against your login at machine speed. Lecture 7 covers it in depth. The request-side defense is rate limiting, and it is the same defense as everything else here.
- Enumeration: "No account with that email address" versus "Incorrect password" tells the attacker which emails are accounts before they guess a single password. Response *timing* tells them the same thing if the server checks the password only when the account exists.
- Resource exhaustion: a search with no result limit, a report endpoint that takes thirty seconds of CPU, an upload with no size cap, a password hash deliberately tuned to be slow and no rate limit on the login page. The attacker doesn't need to break anything; they just ask a lot.

### What goes wrong: two parsers, two opinions

Part 1 ended its secure coding list with "use the same parser to validate data and to use it, so the two cannot disagree." On the web, one request is parsed by several programs in a row before it reaches your code: a load balancer, a web application firewall, a reverse proxy, the web server, then the framework. If any two of them disagree about where a request ends or what a path means, an attacker can write one request that the front sees as harmless and the back sees as something else.

- Verb tampering: the access rule says "POST to `/admin` requires login." The attacker sends `GET /admin`, or `HEAD`, or a made-up method, and the rule doesn't match while the handler still runs.
- Path confusion: Ivanti's login check in Part 1 saw `/public/../../admin` as public and the server resolved it to the admin area. Two components, two readings of one path.
- Request smuggling: the proxy and the server use different headers to measure the body length, so the proxy forwards what it thinks is one request and the server reads it as one and a half. The leftover half becomes the beginning of the *next* request on that connection, which belongs to whichever user comes next. Advanced, but the disease is the same one: two parsers, two opinions.

### How to defend: fix the request

- HTTPS everywhere, plus HSTS, with no exceptions for "just the marketing pages." A single plaintext page on the domain is a place to steal the cookie.
- Let the framework generate session tokens: long, random, from a cryptographic generator. Never design your own.
- Rotate the token at login, which kills fixation; expire it, in hours for sensitive applications with an idle timeout; invalidate it on the server at logout, not just in the browser; and rotate it again on any privilege change such as a password reset.
- Tokens live in cookies with `Secure`, `HttpOnly`, and `SameSite`. Never in the URL. Never in local storage, which any script can read.
- Headers are claims, not facts. No authorization decision rests on `Referer`, `X-Forwarded-For`, or `Host`. Configure your own proxy to overwrite those headers on the way in, and read only its copy.
- Rate limit logins, password resets, searches, uploads, and every endpoint that costs you more than it costs the sender. Return the same message and take the same time whether or not the account exists.
- Deny by default on method and path. "Everything under `/admin` requires login, for any method" instead of listing the methods you thought of.
- Normalize the request once, at the edge, and make every layer behind it agree on the normalized form.

### Practical application: Citrix Bleed, 2023

Citrix NetScaler is the appliance that sits in front of many organizations' VPNs and web applications, handling logins and sessions for everything behind it. In October 2023 Citrix disclosed CVE-2023-4966 (Common Vulnerabilities and Exposures, the public numbering system for known vulnerabilities) \[7\]. A single crafted request, with no login, caused the appliance to reply with a chunk of its own memory, and that memory held other users' live session tokens \[8\]. The Heartbleed lesson from Part 1, that a leak is a prize on its own, meets today's lesson, that the token is the login.

With a stolen token the attacker simply was that user. Multi-factor authentication had already happened for the real user when the token was issued, so the appliance saw a valid session and asked no further questions \[7\]. Within weeks the LockBit ransomware operation and its affiliates were using the bug against Boeing, a major law firm, and others. The Cybersecurity and Infrastructure Security Agency (CISA), the U.S. government's civilian cyber defense agency, included an unusual instruction in its advisory: patching was not enough, because sessions stolen before the patch stayed valid afterward, so administrators had to kill every active session as well \[7\].

| Stop | In this attack | Defense that would have helped |
| --- | --- | --- |
| Browser | Not involved. The attacker used a script, not a browser. | |
| Request | One unauthenticated request with an over-long header | Validate the request at the boundary (Part 1's first lesson) |
| Server | Out-of-bounds read of appliance memory (Part 1 again) | Memory-safe code; fuzzing the request parser |
| Data | Live session tokens stored in the same memory as request handling | Keep secrets in a separate process the request handler cannot read |
| Response | The reply carried other users' tokens | Short session lifetimes; bind sessions to a client property; kill all sessions on patch \[7\] |

Question: A university portal's "forgot password" page builds the reset link as `"https://" + request.Host + "/reset?token=" + token` and emails it to the student. What does an attacker send, and what do they get?

## 3. Server

The server is the only stop in the loop you fully control, which means every decision that matters has to be made here, and made again even when the client already made it. Three decisions, every request: who is sending this, what may they do, and what does the input mean. Most of the OWASP Top 10 lives in this part. The 2025 list's number one is Broken Access Control, and Security Misconfiguration, Injection, Insecure Design, and Authentication Failures are all in the top seven \[4\].

### What goes wrong: authentication, briefly

Default credentials still shipping on the admin console. Admin pages reachable without a login because nobody linked to them. Password-reset flows that trust a guessable security question, send a four-digit code with no rate limit, or build the link from the `Host` header. Lecture 7 covers authentication in depth. Today's point is narrower and comes straight from Part 1: authentication is a check, and checks get skipped. Every sensitive function needs its own check in front of it, not a check on the page that links to it.

### What goes wrong: broken access control

OWASP's number one web application risk in 2021 and again in 2025 \[4\]. "Missing Authorization" is fourth on the 2025 CWE Top 25 \[3\]. The failure is simple to state: logged in is not the same as allowed. The server knows the user is Alice. It forgets to ask whether Alice may see record 124.

| Shape | What it looks like | What's missing |
| --- | --- | --- |
| Insecure direct object reference (IDOR) | `/api/invoices/1001`, so try `/api/invoices/1002` | A check that invoice 1002 belongs to this user |
| Missing function-level check | `/admin/users` has no login check because "there's no link to it from the user pages" | A check on the function, not on the link |
| Role in the request | A profile update that includes `{"role": "admin"}` and the server saves it | Server-side roles; ignore any role the client claims |
| Mass assignment | The framework binds every field in the JSON body straight onto the database model, so the attacker adds `"isAdmin": true` or `"balance": 1000000` | A whitelist of the fields the user is allowed to set |
| Forced browsing | Guess `/reports/2026-Q3-board-draft.pdf` | Authorization on every file, not obscurity of the name |
| Step skipping | Go straight to `/checkout/confirm` without visiting `/checkout/pay` | Workflow state kept on the server; each step verifies the previous one |

The Part 1 table had `accountId=123` changed to `124`. That row is the entire First American breach, below.

### What goes wrong: business logic

In a logic bug, the code does exactly what it says. What it says is wrong. These never show up in a scanner because the scanner doesn't know what the application is *for*.

- Negative quantity: order −3 laptops and the total is a refund.
- A coupon applied twice because the "used" flag is set after the discount is computed rather than before.
- Two requests arriving at the same instant both pass the check "balance is at least $500" before either one subtracts. Part 1's time-of-check-to-time-of-use race, with a bank balance instead of a file.
- The price sent from the client, which was Part 1's `price=4.99`, and still ships in new applications every year.

Finding these takes a person who understands the business and asks what happens if steps are reordered, repeated, or skipped.

### What goes wrong: input that means more than it looks like

Part 1's assumption "the input means what it looks like" has a dozen web-shaped forms. Several are Part 1 weaknesses that simply arrive over HTTP; the rest are new.

| Weakness | The input | What the server does with it |
| --- | --- | --- |
| OS command injection | A filename, a hostname to ping, an image to convert | Pastes it into a shell command (Part 1) |
| Path traversal | `../../etc/passwd`, `..\..\web.config` | Opens it as a file (Part 1) |
| Insecure deserialization | A serialized object in a cookie or a form field | Rebuilds the object, running whatever that object's class runs on load (Part 1) |
| Server-side template injection | `{{7*7}}` typed into a "display name" that later goes through a template engine | Renders it as template code. If the page shows `49`, the attacker can run code on the server. |
| XML external entities (XXE) | An XML upload that declares `<!ENTITY x SYSTEM "file:///etc/passwd">` and references `&x;` | The XML parser fetches the file and pastes its contents into the document |
| File upload | `resume.php`, `photo.jpg.html`, a file named `../../index.html`, a 4 GB file | Stores it under the web root, then serves or executes it |
| Server-side request forgery (SSRF) | A URL the server will fetch on the user's behalf: "import from URL," a webhook, an image proxy, PDF-from-URL | The server makes the request, from inside your network, with the server's own credentials |

### SSRF: the server as the attacker's browser

Many features need the server to fetch a URL the user supplies: pull an avatar from a link, generate a preview of a pasted URL, fetch a feed. If the server will fetch any URL, the attacker supplies `http://localhost:8080/admin`, or `http://10.0.0.12/`, or on a cloud server, `http://169.254.169.254/latest/meta-data/iam/security-credentials/`.

That last address is the cloud provider's metadata service. It answers only to requests from inside the machine, and it hands out the credentials the machine is running with. A server that will fetch any URL will fetch that one and return the result to the attacker. Firewalls do not help, because the request is coming from a trusted machine on the inside. This is how Capital One lost 100 million records \[10\].

### What goes wrong: misconfiguration

OWASP's number two in 2025 \[4\]. Nothing in the code is wrong; the settings are.

- Debug mode left on in production. Every error page shows file paths, source code, local variables, and in some frameworks a live interactive console.
- Directory listing on, so `/uploads/` shows every file anyone has ever uploaded.
- Default accounts, sample applications, and admin consoles still installed from the framework's starter kit.
- `Access-Control-Allow-Origin: *` together with credentials, or an origin copied back from the request header. That is the same-origin policy switched off by the server, so any site can read your users' data.
- A cloud storage bucket set to public "just for the demo."
- Secrets in the repository: API keys, database passwords, cloud credentials in a config file that was never supposed to be public, and then the repository was.

### How to defend: decide on the server, deny by default

- Authorize every access to every object. "Does this record belong to this user?" is a database query, not an assumption. Put the check in one place, a middleware or policy layer that every endpoint goes through, so it cannot be forgotten on the new endpoint written at 5 p.m. on a Friday.
- Deny by default. A new endpoint, a new file, a new HTTP method is locked until someone writes the rule that opens it.
- Indirect references. Show the user a random identifier or a per-user index rather than the database primary key. This does not replace the check, but it removes the "add one and see" attack.
- Whitelist bindable fields. Never bind a request body straight onto a model. List the fields a user may set, and ignore the rest.
- Keep workflow state on the server. The client tells you what step it wants; the server knows what step it is on.
- Make money and inventory operations atomic: one database transaction that checks and subtracts, never two requests that each check.
- File uploads: determine the type from the file's content, not its name; rename it to a random name; store it outside the web root; serve it through a handler that sets `Content-Disposition: attachment` and never executes; cap the size.
- SSRF: whitelist fetch destinations by hostname; resolve the name and reject private, loopback, and link-local addresses after resolution, since a hostname can resolve to anything; on AWS require version 2 of the Instance Metadata Service (IMDSv2), which demands a session token that a simple one-shot fetch cannot obtain \[25\], \[24\].
- Configure parsers safely: external entity resolution off, deserialization of untrusted input off, template engines given data and never user text as the template itself.
- Run the application with as little as it needs. Its own operating system account, its own database account, its own cloud role, each with only the permissions that one application's job requires. Part 1's least privilege, applied three times at once.
- Treat production configuration as a checklist: debug off, directory listings off, defaults removed, secrets in a vault, CORS explicit (Cross-Origin Resource Sharing, the headers that tell the browser which other origins may read your responses).

### Practical application: Capital One, 2019

On March 22 and 23, 2019, an attacker copied data on about 100 million people in the United States and 6 million in Canada from Capital One's cloud storage: credit card applications going back to 2005, with names, addresses, dates of birth, and self-reported income, plus about 140,000 Social Security numbers and 80,000 linked bank account numbers \[9\]. Capital One called it a "configuration vulnerability" \[9\].

The configuration was a web application firewall, a product whose whole job is to inspect requests on the way to the application. The firewall could be made to send requests on the attacker's behalf, and one of those requests went to the cloud metadata service, which answered with the credentials of the role the firewall was running as \[10\]. That role could list and read the storage buckets holding the data, far more than a firewall needs. The attacker, a former engineer at the cloud provider, had built a tool to scan for exactly this misconfiguration across many companies' accounts; Capital One was one of more than thirty \[10\]. She was convicted of wire fraud and computer intrusion in 2022. Capital One paid an $80 million regulatory fine and $190 million to settle customer lawsuits \[10\].

Two defenses, either one sufficient: a server that would not fetch arbitrary URLs, or a role that could not read 100 million records.

| Stop | In this attack | Defense that would have helped |
| --- | --- | --- |
| Browser | Not involved | |
| Request | A request that told the firewall what URL to fetch | Validate the destination at the boundary |
| Server | SSRF: the firewall fetched the metadata URL from its own network position, and its role had far more access than its job required | Whitelist fetch destinations; block link-local addresses; IMDSv2; least-privilege role |
| Data | Bucket contents readable by that one role | Separate roles per data set; monitor bulk reads |
| Response | Credentials returned to the attacker, then 100 million records | Alert on metadata-service access from a web process; alert on unusual bucket listing |

### Practical application: First American, 2019

First American, one of the largest title-insurance companies in the United States, ran a website for sharing scanned closing documents with buyers, sellers, and lenders. Each document had a number in its URL, and the site served whatever number you asked for. Change the number, get a stranger's mortgage paperwork: bank account and routing numbers, Social Security numbers, driver's license images, tax records \[11\]. More than 800 million images dating back to 2003, reachable by anyone who had a link to any one of them \[11\].

The company's own security staff had found the flaw months before it became public and it was not fixed under the company's own vulnerability policy \[11\]. The U.S. Securities and Exchange Commission's (SEC) charge was not about the bug. It was about senior executives never being told about it before they signed the company's public filings \[11\]. There is no code to exploit here. The code works. It just never asks who is asking.

Question: A student-records app exposes `GET /api/transcripts?studentId=…`. Students log in through the university's single sign-on. The developer says it's fine because the student ID isn't shown anywhere on the page. Walk through what an attacker does and what the fix is.

## 4. Data

Where data and commands share a sentence. SQL injection is number two on the 2025 CWE Top 25 \[3\]. It was first described in 1998 \[27\]. It is the reason MOVEit happened in 2023. The defense has been known for the entire 28 years, and it is one line of code.

### What goes wrong: SQL injection

The code builds a query by gluing strings together:

```
query = "SELECT * FROM users WHERE name = '" + name + "'"
```

The attacker's name is `' OR '1'='1`. The database receives:

```
SELECT * FROM users WHERE name = '' OR '1'='1'
```

The database cannot tell where the programmer's SQL ends and the user's input begins, because the program handed it one string. It parses the whole thing as a query, and `'1'='1'` is always true, so the query returns every user. This is exactly Part 1's "input pasted into a command." The shell is a database now.

### What SQL injection gives the attacker

| Technique | What it does |
| --- | --- |
| Comment out the rest | `admin' --`: the rest of the query, including the password check, becomes a comment. Log in as admin. |
| UNION | `' UNION SELECT username, password FROM users --`: append another table's contents to whatever the page was going to show. |
| Stacked queries | `'; DROP TABLE orders; --`: run a second statement, where the database driver allows it. |
| Blind, boolean | When the page shows no query output, ask yes-or-no questions: `' AND SUBSTRING(password,1,1)='a'` and watch whether the page changes. One character at a time. Tools automate it in seconds. |
| Blind, time-based | When nothing visible changes at all: `' AND IF(…, SLEEP(5), 0)` and time the response. A five-second delay means yes. |
| Error-based | Make the database put the answer inside its own error message, which the page helpfully displays. |
| Out of the database | Some databases can read and write files on the server or run operating system commands. Write a web shell into the web root, and the attacker now has a terminal on the server. That is what Cl0p did to MOVEit \[12\]. |

### What goes wrong: the cousins

The pattern is "data and commands in one sentence." SQL is the most common sentence, not the only one.

| Backend | The injection |
| --- | --- |
| NoSQL (MongoDB) | Send `{"username": "admin", "password": {"$gt": ""}}` and the query becomes "password greater than an empty string," which is always true. No quotes needed. |
| LDAP (Lightweight Directory Access Protocol, the query language for corporate user directories) | `*)(uid=*))(|(uid=*` in a directory lookup rewrites the filter. |
| ORM query languages (object-relational mapper, a library that turns objects in code into database rows) | Hibernate's HQL (Hibernate Query Language) and its relatives can be concatenated exactly like SQL, and developers assume the ORM made them safe. |
| GraphQL | Not injection, but the same trust failure: introspection reveals the whole schema, and nested or batched queries turn one request into thousands of database hits. |
| Stored procedures | Safe only if the procedure does not build dynamic SQL inside itself. Many do. |
| Second-order | Input stored safely on the way in, then used unsafely later. A username `admin'--` sits harmlessly in the table until a nightly job builds a query with it. |

### Why it is still here in 2026

- Legacy code, written before anyone taught this, still serving traffic.
- The parts that cannot be parameterized: `ORDER BY` a column the user picks, a table name, a `LIMIT`. Developers parameterize the easy parts and concatenate the hard ones.
- Internal tools "nobody outside will ever see," until someone inside is phished.
- Code written fast. Part 1 cited Veracode's finding that 45% of AI-generated code introduced a known weakness \[26\].
- The 2026 Verizon breach report found that for the category of weaknesses covering SQL injection and cross-site scripting, organizations took a median of just over thirteen months to fix even half of what they knew about \[30\]. The attackers' tools find it in minutes.

### How to defend: keep data and commands in separate lanes

- Parameterized queries, also called prepared statements. This is the fix.

```
query = "SELECT * FROM users WHERE name = ?"
db.execute(query, [name])
```

  The database receives the command and the data separately. The input is never parsed as SQL no matter what it contains; `' OR '1'='1` is just a name that nobody has. This is the same idea as Part 1's "run the program directly and pass the input as a separate argument," applied to a database \[20\].

- ORMs and query builders, used the way they were designed. Every ORM has an escape hatch, a `raw()` or `execute()` that takes a string, and that is where to look first in code review.
- For the parts that cannot be parameterized, map the input to a fixed set: `{"name": "name", "date": "created_at"}`, look up what the user sent, and reject anything that isn't a key. The user's text never reaches the query.
- Validation is the second lock, not the first. Type, length, format, range. It would have rejected `' OR 1=1`, and it would also have rejected a legitimate `O'Brien`, which is why it cannot be the only defense.
- Least privilege for the database account. The web application's account reads and writes its own tables and nothing else: no `DROP`, no file access, no operating-system commands, no access to the users table from the product-search code. A separate read-only account for reports. Injection through a read-only account leaks data; injection through an administrator account owns the server.
- Database errors never reach the user. Log them with full detail; show the user "something went wrong." Error-based injection needs the error.
- Protect what is stored: hash passwords (Lecture 5), encrypt the columns that would hurt most if they leaked, and keep the database off the internet entirely, reachable only from the application servers.
- Secrets out of the code. The database password lives in a vault or in the environment the application reads at startup, never in a file in the repository.
- Watch the database. An application that normally returns 20 rows and suddenly returns 2 million, or that runs a query shaped like nothing it has ever sent, is an alert.

### Practical application: MOVEit Transfer, 2023

MOVEit Transfer is a product that companies put on the internet so that partners, customers, and employees can exchange large files securely. Payroll providers, banks, state agencies, universities, and pension funds all ran it \[13\]. Its web interface had a SQL injection that an unauthenticated attacker could reach \[12\], \[13\].

The Cl0p ransomware group began exploiting it on May 27, 2023, before Progress Software knew the bug existed, timed for the U.S. Memorial Day weekend when fewer people would be watching \[12\]. They did not stop at the database. They used the injection to install a web shell they called LEMURLOOT into the application, and then used the web shell to pull every file off every server they reached \[12\]. No ransomware was deployed; they stole the data and extorted the owners, publishing the names of companies that refused to pay.

By mid-2024, the count stood at 2,773 organizations and about 95.8 million people \[14\]. Most of those people had never heard of MOVEit. It was their payroll vendor's, or their state's motor vehicle department's, or their university's, and the bug was one string concatenation in one product.

| Stop | In this attack | Defense that would have helped |
| --- | --- | --- |
| Browser | Not involved | |
| Request | An unauthenticated request to the web interface | Authentication on every sensitive function (Part 1) |
| Server | The application built SQL from the request | Parameterized queries |
| Data | The database account could do far more than serve one application, including writing to the server | Least-privilege database account; no file-system access from the database |
| Response | A web shell served from the application's own web root | File integrity monitoring on the web root; egress monitoring for bulk downloads |

Question: A library catalog lets patrons sort search results by clicking a column header. The link is `/search?q=rust&sort=title`, and the code does `"… ORDER BY " + sort`. The developer says it's safe because the parameter is "only ever one of the four column names." What does an attacker put in `sort`, and what are two different ways to fix it?

## 5. Response

What the server says, the browser does. Part 1 taught the trust boundary on the way in. The web has a second one on the way out: data crosses from your program into a browser that will interpret it as instructions, in HTML, JavaScript, CSS, and URLs. A username is data in your database. In an HTML page it is markup. If it contains `<script>`, the browser does not know you meant it as a name.

### The other trust boundary

Cross-site scripting is a response bug. The attacker's text may enter at any stop, as a search term, a profile field, a comment, a URL fragment, but the mistake is made here, where the server writes it into the page without making it safe for the language it lands in. Every XSS defense that is not at the Response stop is a mitigation of damage. The fix is here.

### What goes wrong: one string, five languages

The same user input needs different treatment depending on where in the page it lands, because each location is parsed by a different grammar \[19\].

| Where it lands | What breaks it | What safe looks like |
| --- | --- | --- |
| HTML body: `<p>NAME</p>` | `<`, `>`, `&` | `&lt;script&gt;` |
| HTML attribute: `<input value="NAME">` | A `"` followed by a new attribute: `" onfocus="alert(1)` | Always quote the attribute, and encode `"` inside it |
| JavaScript: `var n = "NAME";` | A `"` to end the string, or `</script>` to end the script block | Encode for JavaScript strings, or better, never put data inside script; put it in a `data-` attribute and have the script read it |
| URL: `<a href="/u?name=NAME">` | `javascript:` as the whole URL | URL-encode the value; check that the scheme is `http` or `https` |
| CSS: `style="color: NAME"` | `url()`, `expression()` | Don't accept user CSS; if you must, whitelist exact values |

MySpace's filter tried to recognize bad things. It missed `java\nscript` and it missed JavaScript inside CSS `url()` \[1\]. A blacklist asks "is this one of the bad things I know about?" Encoding asks "is this definitely data?" Only the second question has a complete answer, and it is the same answer every time.

### What goes wrong: the framework's escape hatches

Modern frameworks encode by default. React, Vue, Angular, Django templates, Jinja2 with autoescape on, Razor, Blade: putting a variable into a template encodes it for HTML, and the developer never thinks about it. This is why stored XSS is much rarer in applications written since 2015 than in applications written in 2005.

Each framework also has a door marked "I know what I'm doing": React's `dangerouslySetInnerHTML`, Vue's `v-html`, Jinja2's `|safe`, Blade's `{!! !!}`, Razor's `@Html.Raw`, and plain `innerHTML` in any JavaScript. Every one of those is the first place to look in a code review, because every one is a developer saying "this text is HTML and I trust it," and the question is where the text came from.

DOM-based XSS skips the server entirely. JavaScript in the page reads a *source* the attacker can control, `location.hash`, `location.search`, `document.referrer`, a `postMessage` from another window, and writes it to a *sink* that interprets it, `innerHTML`, `eval`, `document.write`, `setTimeout` with a string. The server encoded everything perfectly and it did not matter, because the dangerous text never went to the server. Veracode's 2025 test of AI code generators found they failed to defend against XSS in 86% of the tasks that called for it \[26\].

### What goes wrong: saying too much

The response carries more than the page, and much of it tells an attacker what to try next.

- Stack traces and debug pages: file paths, framework and library versions, the failing SQL, sometimes source code.
- Version banners: `Server: Apache/2.4.49` tells the attacker exactly which published exploit to use.
- Different errors for different causes: "user not found" versus "wrong password"; a `403` for a path that exists versus a `404` for one that doesn't, which lets an attacker map every admin endpoint without logging in.
- API responses that return the whole object and let the frontend decide what to show. The browser received the password hash, the home address, and the other user's email; the page just didn't display them. The attacker reads the response, not the page.
- Comments, source maps, and `TODO: remove auth bypass before launch` shipped to production.
- Cacheable sensitive pages. A shared computer's browser cache, or a CDN in front of the site, serves one user's account page to the next user.

### How to defend: encode at the point of output

- Encode for the context, at the moment of writing out. Not on the way in, because at that point you don't know where the data will land, and not with a blacklist. Use the framework's template engine and leave autoescape on.
- Treat the escape hatches as code-review triggers. Every `innerHTML`, every `|safe`, every `dangerouslySetInnerHTML` needs either a one-line justification showing the content is not user-controlled, or a sanitizer.
- When users need rich HTML, a forum post with bold text and links, run it through a real HTML sanitizer that parses the input and rebuilds it from a whitelist of allowed tags and attributes. DOMPurify in the browser, Bleach in Python, the OWASP Java HTML Sanitizer. Do not write your own. MySpace wrote its own.
- In DOM code, write text with `textContent`, not `innerHTML`. Chrome's Trusted Types feature can make the browser refuse to accept a plain string at any HTML sink, which turns DOM XSS into a type error at development time.
- Respond with exactly the fields the client needs. Build response objects on purpose rather than serializing the database row.
- Generic errors to the user, full detail to the log. The same message, the same status code, and the same response time whether the user exists or not.
- Strip version banners and turn debug mode off. Both are one line in the configuration.

### How to defend: headers that tell the browser what to enforce

The browser will enforce rules on your behalf, but only if the response asks it to. These are a few lines of configuration in the web server or framework, and most sites set none of them \[21\].

| Header | What it tells the browser |
| --- | --- |
| `Content-Security-Policy` | "Only run scripts from these sources. No inline script. Only send data to these domains." A script that XSS injected into the page does not match the policy and does not run. Samy's inline code and the Magecart skimmer's request to `baways.com` would both have been blocked. |
| `Content-Security-Policy-Report-Only` | The same policy, but the browser logs violations to a URL you specify instead of blocking. This is how you roll out CSP on a live site without breaking it. |
| `X-Content-Type-Options: nosniff` | "This file is what I say it is." Stops the browser from guessing that an uploaded `.jpg` is really JavaScript and running it. |
| `X-Frame-Options` or CSP `frame-ancestors` | "Nobody may put me in a frame." Ends clickjacking. |
| `Referrer-Policy` | "Don't tell other sites which page the user came from." Stops tokens in URLs from leaking to third parties. |
| `Strict-Transport-Security` | "HTTPS only, from now on" \[23\]. |
| `Cache-Control: no-store` | "Don't keep this page." For anything with personal data in it. |
| `Set-Cookie` attributes | `Secure; HttpOnly; SameSite` (Part 1). |

CSP is the seatbelt: it does not stop the bug, it limits what the bug can do. The real work of CSP is removing inline scripts and `onclick` attributes from your pages so that the policy can be strict enough to matter.

### Practical application: Samy, revisited

| Stop | 2005 | The defense |
| --- | --- | --- |
| Browser | The script ran in every viewer's session | CSP with no inline script, so the browser refuses to run code embedded in a profile; `HttpOnly` on the session cookie |
| Request | The script's requests carried the viewer's session, and the anti-forgery token was readable because the script was same-origin | `SameSite` cookies help against outside pages, but nothing at this stop stops same-origin script. This is why XSS defeats CSRF defenses, and why the Response fix matters most. |
| Server | Profile HTML filtered with a list of banned words \[1\] | A parsing sanitizer with a tag whitelist, or no user-supplied HTML at all |
| Data | The worm stored as profile content | Nothing needed. Stored content is harmless if it is treated as data on the way out. |
| Response | Stored content rendered as live HTML to every viewer | Encode on output. One change in one template, and the worm becomes a paragraph of text that says `<div style=…>` to a million people. |

Question: A course Q&A board lets students post answers with bold, italics, links, and images. The frontend is React, so the developer says XSS is "handled by the framework." What specific things would you want to see in the code and in the response headers before you believed that?

## 6. Around the app

The parts you didn't write. A typical web application is mostly other people's code: the framework, the ORM, the template engine, the JSON parser, the logging library, and the several hundred packages those depend on. You wrote the thin layer on top. A bug in any of the rest is a bug in your application, and you find out when the vendor publishes it, if you are watching, or when the attacker does, if you are not.

### What goes wrong: the code you imported

OWASP added "Software Supply Chain Failures" to its Top 10 in 2025, at number three, replacing the older and narrower "Vulnerable and Outdated Components" \[4\]. The change in name reflects a change in the attacks. The old problem was running a library with a known bug and not patching it. The new problem is that the library itself is the attack, delivered through the same package manager you use to install everything else.

Polyfill.io in Part 1 was the client-side version: a script on a CDN changed owners and then changed behavior \[6\]. The server-side version has a famous name from 2017, and 2026 has given it two new ones.

### Practical application: Equifax, 2017

Apache Struts is a Java framework for building web applications. In March 2017 the Struts project published a fix for a bug that let anyone run commands on the server by putting them in a request header. Exploit code was public within days, and attackers were scanning the internet for vulnerable servers within the week.

Equifax ran Struts in its consumer dispute portal, the website where people contest errors on their credit reports. Equifax's own security team circulated the patch notice internally, but the portal was not on the list of systems that got patched, and a scan meant to find unpatched instances did not find it \[15\]. Attackers got in through the portal in mid-May and spent 76 days inside, moving from the web server to databases that the portal had no business reaching, running about 9,000 queries, and pulling data out in small encrypted chunks \[15\].

Equifax had a device that was supposed to inspect encrypted traffic leaving the network for exactly this kind of exfiltration. Its certificate had expired ten months earlier, so it had been inspecting nothing \[15\]. When the certificate was finally renewed in late July, the device saw the suspicious traffic within a day. At least 145.5 million people's names, Social Security numbers, birth dates, and addresses were taken \[15\].

The U.S. Government Accountability Office's (GAO) report identified four factors, and any one of them fixed would have been enough: identification of the vulnerable system, detection of the intrusion, segmentation between the web portal and the databases, and data governance, meaning knowing what data lived where and limiting what any one system could reach \[15\].

### Log4Shell, 2021: the one everyone had

December 2021. Log4j is the logging library in a large share of the world's Java software. If a specially formed string appeared in anything the application logged, Log4j would fetch code from a URL in that string and run it \[16\]. Anything a user could get into a log line was an attack: a username, a `User-Agent` header, a chat message, a Minecraft server's chat box.

Most organizations did not know which of their applications contained Log4j, because it was a dependency of a dependency of something they bought \[16\]. The lesson was not "patch Log4j." The lesson was that you cannot patch what you cannot list.

### Practical application: npm, 2026

The JavaScript package registry, npm, is where nearly every web frontend and most Node.js backends get their code. In 2026 it was attacked twice in ways that the whole ecosystem felt.

Axios, March 31, 2026. Axios is the HTTP client library used by a large fraction of all JavaScript applications, with over 100 million downloads a week. Two malicious versions, 1.14.1 and 0.30.4, were published to npm. They added one new dependency, `plain-crypto-js`, that the Axios code never actually imported; its only purpose was an install script that ran automatically whenever anyone ran `npm install`. The script downloaded a remote-access trojan matched to the developer's operating system, gave the attacker full control of the machine, added a Windows registry key to survive reboots, and then deleted its own loader and renamed a clean file over the install hook to hide what it had done \[28\]. Microsoft attributed it to a North Korean state group whose specialty is stealing cryptocurrency, and the malicious versions had none of the project's normal signs of a legitimate release: no trusted-publisher binding, no matching commit or tag in the repository \[28\].

ChainDrop, August 2026. A self-propagating worm hidden in npm packages. It ran as a preinstall hook, before the package finished installing, and stole everything on the machine that could be used to publish or deploy: npm tokens, GitHub tokens, AWS and Kubernetes credentials, SSH keys, shell histories, and the secrets of CI pipelines (continuous integration, the automated build-and-test systems). Then it used the stolen npm token to list every package that identity could publish, downloaded each one, inserted itself plus a preinstall hook, bumped the version number, and republished. One stolen token meant malicious releases across every package that maintainer controlled, and every developer who installed any of them became the next maintainer it stole from. More than 400 packages from unrelated publishers were compromised, with no matching source commit for most of the malicious versions because the published files were edited directly \[29\]. The worm also wrote configuration files for AI coding assistants into repositories it could reach, creating a developer-to-developer path through tools that run code on a developer's behalf \[29\].

Samy was a worm that spread through profiles. ChainDrop is a worm that spreads through the build. Twenty-one years apart, the mechanism is identical: run inside something trusted, use its credentials to copy yourself somewhere else that is trusted.

### How to defend: dependency hygiene

- Know what you have. A software bill of materials (SBOM) lists every package and version including the ones your packages pulled in, and it is generated by the build, not maintained by hand. This is the answer to Log4Shell's "do we even have it?"
- Lock files and pinned versions, so a build today uses exactly the code a build yesterday used, and a surprise new version doesn't ride in on a routine install. npm's `min-release-age` setting, added in response to the 2026 attacks, refuses to install a version published less than a set number of days ago, which gives the ecosystem time to notice \[29\].
- Automated scanning against vulnerability databases in the build pipeline and continuously afterward. Dependabot, Renovate, Snyk, OWASP Dependency-Check: pick one and turn it on.
- A patch policy with a clock on it. Critical and internet-facing: days, not months. The Equifax fix was available for two months \[15\]. The 2026 Verizon report found that only 26% of vulnerabilities on CISA's known-exploited list were fully fixed within the year, down from 38% the year before \[30\].
- Disable install scripts by default (`npm install --ignore-scripts`) and enable them only for packages that need them. Both 2026 attacks ran as install hooks \[28\], \[29\].
- Verify what you fetch: package signatures and provenance, SRI for scripts loaded from a CDN, and attention to projects that change maintainers or domains that change owners \[6\].
- Fewer dependencies. The safest dependency is the one you didn't add. Polyfill's own author said modern browsers no longer need it at all \[6\].
- Treat the developer's laptop and the build server as production. Both 2026 attacks were after the credentials on those machines, not the application they were building \[28\], \[29\].

### Web application firewalls: what they are and are not

A web application firewall (WAF) sits in front of the application, reads each request, and blocks the ones that match known attack patterns: `' OR 1=1`, `<script>`, `../`, known bot signatures, requests arriving too fast from one address.

What it is for: a layer that catches the common, lazy attacks before they reach your code; virtual patching, where you write a rule that blocks a known bug today while you fix the code this week; and bot and rate control.

What it cannot do: it does not understand your application. It cannot tell that invoice 1002 belongs to someone else, that a negative quantity is wrong, or that `sort=title` is fine while an encoding of the same attack that it hasn't seen is not. Attackers test their payloads against the same commercial WAFs you can buy, and the public lists of bypasses are long. The Capital One intrusion went *through* the WAF; the firewall was the server that made the SSRF request \[10\]. A control is also attack surface. The WAF is the lock on the gate, not a reason to leave the front door open.

### Logging and monitoring: knowing it happened

OWASP 2025's number nine is Security Logging and Alerting Failures \[4\]. British Airways skimmed cards for 15 days, Equifax's attackers were inside for 76, and First American's documents were open for years. Not one was found by the victim's own monitoring.

What to log, with who, what, when, and from where: logins and failed logins, password resets, access-control denials, input-validation failures, administrative actions, and any change to the files the web server serves.

What never to log: passwords, session tokens, card numbers, CVVs. BA's test log collected CVVs for three years \[5\].

What to alert on: one session hitting many `403`s in a row, which is someone walking through object IDs; a spike in `500`s or database errors, which is someone probing for injection; a web process making a request to the cloud metadata service; a file change in the web root; a query returning a million rows at 3 a.m.

Logs go somewhere the web server itself cannot delete. Ivanti's attackers in Part 1 wiped the local logs first. Lecture 9 covers centralizing, tuning, and triaging.

### Testing: finding it before they do

| Method | What it does | What it finds | What it misses |
| --- | --- | --- | --- |
| Static analysis (SAST) | Reads the source code | Concatenated SQL, `innerHTML` with user data, missing encoders, hard-coded secrets | Logic bugs; anything whose data flow it can't trace |
| Dynamic analysis (DAST) | Attacks the running application the way a scanner would | Reflected XSS, injection, missing headers, misconfiguration | Anything behind a login it can't drive; business logic |
| Dependency scanning | Compares the package list to vulnerability databases | Known-vulnerable versions | Zero-days; your own code; a malicious package that isn't in the database yet |
| Manual testing, penetration testing | A person with a proxy and the application's purpose in mind | IDOR, logic flaws, chained bugs | Scale; it is a snapshot of one week |
| Bug bounty | Many outsiders, paid per finding | What your own testers didn't think of | Whatever nobody has reported yet |

Run the automated ones in the build so that a new concatenated query fails the build before it merges. OWASP ZAP (Zed Attack Proxy) is free, and it is what you will point at the lab's deliberately vulnerable application. The point of the lab is to watch the request go out and the response come back, not just to read the finding.

Question: A small nonprofit runs its website on WordPress with about 30 plugins, hosted by a volunteer who updates it when he remembers. They have no budget for security. Which three things from this part would you tell them to do first, and why those three?

## 7. AI and web application security

This part comes after the walkthrough because it is not a sixth stop on the loop. It is the same five stops, with an AI model standing in one of the roles. The model is now writing the web application, sitting inside it reading untrusted content, and, on both the attacking and the defending side, finding the bugs. Each of those is a role a person or a program used to fill, and the failures are the ones the walkthrough already named, arriving faster. The part ends with the largest web vulnerability of 2026, which an AI helped find.

### What goes wrong: the AI writes the web app

Part 1 cited Veracode's 2025 finding that 45% of AI-generated code introduced a known weakness, and that for cross-site scripting the models failed 86% of the time \[26\]. The Cloud Security Alliance's April 2026 research note gathered the studies since: a Checkmarx test found up to 70% of assistant-written code insecure, and a December 2025 test that had five agentic coding tools build the same application counted 69 vulnerabilities across 15 applications, with broken authorization logic the most common critical finding \[31\]. The failures cluster at the Server stop, for a reason worth saying out loud: the models are decent at the lines a scanner would flag, a parameterized query or an encoded output, and weak at the decisions that depend on knowing what the application is for, which is exactly where Part 3 said the hard bugs live. Who may read this record, which fields a user may set, whether the database should be reachable at all: the model was never told, so it does not build the check.

"Vibe coding," building a working application by describing it to an AI and never reading the code, produces those bugs at scale, and 2026 supplied the breaches. Moltbook, a social network for AI agents, launched in late January; its founder said publicly that an AI assistant had written the code, not him. Within days, reporters and then Wiz researchers found the production database reachable with no authentication and the database key sitting in the frontend JavaScript served to every visitor, exposing roughly 1.5 million agent tokens, 35,000 email addresses, and private messages \[32\]. That is Part 3's misconfiguration and Part 5's "saying too much" in one file. On the Lovable platform, a May 2025 scan of 1,645 apps found 170 exposing personal data because the database's per-row access control was never switched on, and in spring 2026 a broken-access-control flaw in the platform itself exposed users' source code and credentials and sat open for 48 days after it was reported \[31\]. Part 3's number one risk, generated automatically.

### What goes wrong: the AI inside the web app

Part 1's EchoLeak was an assistant that followed instructions hidden in an email. The web version is any application that hands a model something a stranger wrote: a page to summarize, a support ticket to triage, a review to analyze. The model cannot reliably separate the developer's instructions from instructions buried in that content, and the Open Worldwide Application Security Project ranks this, prompt injection, as the number one risk for applications built on large language models. It is the same disease as input pasted into a command: data turns into instructions.

The sharpest version is an AI agent with a browser. Agentic browsers read pages for you and act for you, with your logged-in sessions. In August 2025, Brave's security team demonstrated the problem against the Perplexity Comet browser: they put hidden instructions in a Reddit comment, and when a user asked Comet to summarize the page, the agent treated the hidden text as its own task and used the user's own logged-in sessions to gather account details and hand them to the attacker, with the user having clicked only "Summarize" \[17\]. Every stop on the loop is in that one incident, with the model as the confused deputy: page content (Response) became a Request the agent made with the user's own cookies (Browser), against a Server that saw a valid session. The same-origin policy, the session cookie, and multi-factor authentication all held, and none mattered, because the thing making the requests was authorized to make them. Brave's recommended defenses are Part 1's, restated for a browser: keep the user's instructions separate from page content, treat both page content and the model's output as untrusted, confirm that a proposed action matches what the user asked, require explicit approval for anything sensitive, and keep agentic browsing isolated from logged-in sessions \[17\].

### What goes wrong: the AI finds the bug, on both sides

The 2026 Verizon Data Breach Investigations Report found that exploiting a vulnerability has become the leading way attackers get in, 31% of breaches, passing stolen credentials for the first time in the report's 19 years, and that attackers now use generative AI for vulnerability research, target selection, and tooling \[30\]. The defenders use the same models: in June 2026 OpenAI launched Patch the Planet with Trail of Bits, pointing security models at critical open-source software with human review, and reported in its first months hundreds of issues across 19 projects, dozens of confirmed vulnerabilities in the BSD operating systems, and several exploitable bugs each in Chrome's and Safari's engines \[33\]. Google and Wiz ran parallel efforts with Gemini.

The bugs these efforts find are real and get patched, so the net effect is good, but it moves a clock that was already working against defenders. Verizon found only 26% of the vulnerabilities on the U.S. government's known-exploited list were fully fixed within the year, down from 38%, and a median of thirteen months to fix even half of the injection-and-cross-site-scripting class \[30\]. When a serious bug becomes public, patching takes weeks to months; AI is shortening the time from disclosure to working exploit toward hours. PHP2Shell is the case study for both halves of that clock.

### Practical application: PHP2Shell, 2026

PHP is the language behind roughly 70% of the websites whose server-side language is known: WordPress, Drupal, Joomla, MediaWiki, Magento, and most of the long tail of the web \[31\], \[35\]. On October 21, 2026, the PHP project released fixes for two vulnerabilities, CVE-2026-109618 and CVE-2026-109619, each rated 9.2 out of 10, that could allow remote code execution (RCE, the attacker's commands running on the server) without a login \[36\]. The researchers named the pair PHP2Shell. What makes it a fitting close for this lecture is where the bug lives: not in any application's code but in the PHP engine beneath all of them, so a flawless application is still exposed. The flaw traces to a 2012 version, meaning essentially every PHP release of the last decade carried it \[35\].

The bug class is the one from Part 1, at web scale. It is a memory-safety failure, a use-after-free: when a request hits a fatal error partway through, such as running out of memory, the engine can be interrupted before it finishes cleaning up, and because one worker process serves many requests in a row, a later request can end up using memory the earlier one already released \[35\]. The Part 1 defenses are exactly the ones that apply: a memory-safe language would not have this class of bug, and the practical fix for everyone else is to install the patched engine and restart the workers, because nothing in the application above it can repair a flaw in the engine below it \[36\].

The discovery is the AI story. The two bugs were reported independently by a solo researcher working through OpenAI's Patch the Planet, by Wiz Research using its AI tooling alongside Google's Gemini, and confirmed with the PHP security team over a coordinated disclosure that ran from August to the October patch \[33\], \[35\]. Wiz's own summary was that a single researcher, with frontier models and the right tools, did in about a week what would previously have taken a world-class team months \[35\]. The researcher who found the first bug was candid about the limits, too: the models did not simply surface the flaw from a prompt, and in fact drifted toward newer, more prominent code until he steered them deliberately toward PHP's own decades-old bug history \[36\]. The defensive win was real. So was the clock: when details leaked about a day early, a major infrastructure provider reported attack traffic against its customers before the patch was even out, and watched attackers reshaping their payloads to get past the emergency rules it had deployed \[34\]. A bug that took humans-plus-AI two months to responsibly disclose was being probed within hours of becoming public.

Question: PHP2Shell lived in the PHP engine, so an application with perfectly written code was still vulnerable, and the only real fix was to patch the engine and restart. For a team that runs a dozen WordPress sites, what would you want in place *before* the next bug like this so that "patch everything in hours" is actually possible?

## 8. Lab 3 preview

## 9. Wrap-up and project assignment

### Close: five sentences to leave with

- The browser belongs to the user, and to whoever can run script in it; the same-origin policy protects origins, not people.
- Every byte of a request is a claim the client made, and the session token is the only one the server should believe.
- Logged in is not allowed: the server decides who may do what to which object, every time, from its own records.
- Data and commands travel in separate lanes, or the input becomes the command.
- Output is a trust boundary too: encode for where it lands, and tell the browser what to enforce.

## 10. Penetration testing guide: exploiting the covered weaknesses

The attacker sees the same website the user sees, but also watches the HTTP requests behind each click. OWASP ZAP sits between the browser and the server, and its History list shows the request the browser sent and the response the server returned. The first move is ordinary: search, log in, open a basket, change a profile, upload a file. The second move is to change one part of that captured request and see whether the server crosses a trust boundary. A useful finding has a clear before and after, not merely an unusual error.

Each table below asks the same six questions of every attack: What normal action produces the request? What clue makes that request interesting? What single change tests it? Which Kali tool helps? What result would show why the weakness matters? The examples use familiar store actions, such as tracking an order or uploading a support attachment; a particular target application may offer only some of these features. `URL` means the exact address captured in ZAP, and `http://127.0.0.1:3000` stands in for an example application's address. `curl -i` prints response headers, `-H` sets a header, `-d` sends data, and `-G --data-urlencode` puts encoded data in the URL. A terminal request only shows up in ZAP if it is sent through ZAP's proxy, for example `curl -x http://127.0.0.1:8080 ...` \[37\]. These examples are for applications one has permission to test.

MITRE's CWE numbers name the specific software mistake. The broader MITRE ATT&CK technique [T1190](https://attack.mitre.org/techniques/T1190/) covers exploiting a public-facing application generally; it names the target, not which mistake to test for.

### Browser

| Attack | Start in the ZAP browser | What to look for | Change or test by hand | Kali tool or command | Why it matters; expected result |
| --- | --- | --- | --- | --- | --- |
| Reflected XSS ([CWE-79](https://cwe.mitre.org/data/definitions/79.html)) | Sign in, place a test order, open its tracking page, and select the tracking request with its order ID in ZAP History. | The ID you supplied comes back in that request's response and appears on the tracking page. | Replace the ID first with `CS361TEST`, then with a harmless HTML/JavaScript test such as `<img src=x onerror=alert(1)>`; open the resulting tracking page. | ZAP shows the request and response. `curl -i 'http://127.0.0.1:3000/rest/track-order/CS361TEST'` shows whether the server returns the marker. | An alert means the server returned user text that the browser ran as code. Seeing the tag only as text is not XSS. |
| Stored XSS ([CWE-79](https://cwe.mitre.org/data/definitions/79.html)) | Submit feedback containing `CS361TEST`; reload the feedback view and find both the POST that saved it and the later GET that displayed it. | Text submitted earlier appears on a later page view. | Save the same harmless alert test; view the saved entry from a second test account. | ZAP History links the submission request to the later page request. | If the later viewer's browser runs the payload, the server stored attacker text and served it as code. It can also run for the author. |
| DOM-based XSS ([CWE-79](https://cwe.mitre.org/data/definitions/79.html)) | Type `CS361TEST` in the top search box. Find the search request in ZAP, then compare its response with the finished search page. | The text appears in the visible page but not in the server's response. | Put a harmless HTML test in the part of the URL the page reads; compare the raw response with the finished page. | ZAP Response and browser developer tools (Elements and Console). | The browser's own JavaScript turned URL text into active page content. The server never needed to echo the payload. |
| Third-party script compromise ([CWE-829](https://cwe.mitre.org/data/definitions/829.html)) | Open checkout with dummy payment details. In ZAP History or the browser Network tab, find the outside script the page loads and any requests made after the form is filled. | The fetched script has changed from a trusted copy, or checkout triggers an unexpected request to that script's operator. | In an isolated compromised version, enter dummy checkout data and compare the resulting network traffic with the normal version. | Browser Network tab and ZAP History show the script and any new request; save the script and run `sha256sum script.js` to compare copies. | If a changed outside script sends the dummy data away, it has used the page's authority to skim checkout information. An outside script by itself is not proof of compromise. |
| Cross-site request forgery, CSRF ([CWE-352](https://cwe.mitre.org/data/definitions/352.html)) | Sign in and change your display name. Find the profile-update POST in ZAP History and note its cookie, body, and any anti-forgery value. | The request relies on a cookie and has no effective value proving that the user intended the change. | Submit the same form from a page on a different site while still signed in; check whether the setting changed. | ZAP compares the normal and cross-site requests, including cookies and anti-forgery values. | If the browser sends the cookie and the change succeeds, another site made the user's browser act as the user. A ZAP replay alone does not prove a cross-site attack. |
| Clickjacking ([CWE-1021](https://cwe.mitre.org/data/definitions/1021.html)) | Open a signed-in profile page with a "Save" or "Delete" button. Select the page's GET response in ZAP and inspect its framing headers. | The page can be shown inside an `<iframe>` on a different site; framing headers do not block it. | Put the page in a frame beneath a harmless-looking button and click it with a test account. | `curl -I URL` checks `Content-Security-Policy: frame-ancestors` and `X-Frame-Options`; the browser and ZAP show the actual click and request. | If a disguised click activates the real button, an attacker can trick a user into taking an action. Missing headers alone are only a clue. |
| Open redirect ([CWE-601](https://cwe.mitre.org/data/definitions/601.html)) | Click a link that returns you to another page after login or checkout. Find the `302` response in ZAP and read its `Location` header. | The response's `Location` header is built from a URL in the request. | Replace the destination with a harmless outside address such as `https://example.org`. | ZAP Response or `curl -i 'URL?next=https%3A%2F%2Fexample.org'`. | If the site's own link sends the browser outside the site, it can be used to disguise a phishing destination or redirect a sensitive flow. |

### Request

| Attack | Start in the ZAP browser | What to look for | Change or test by hand | Kali tool or command | Why it matters; expected result |
| --- | --- | --- | --- | --- | --- |
| Header spoofing ([CWE-346](https://cwe.mitre.org/data/definitions/346.html)) | Open an account-only page or a support endpoint. Select its request in ZAP and inspect the `Referer`, `X-Forwarded-For`, `Content-Type`, and `User-Agent` headers. | The server appears to rely on `Referer`, `X-Forwarded-For`, `Content-Type`, or `User-Agent`. | Change one header while keeping the same account and request body. Compare both the response and the actual action. | ZAP Request Editor; `curl -i -H 'X-Forwarded-For: 127.0.0.1' URL` tests one claimed IP. | Headers supplied by a client are claims. Access gained merely by changing one shows the server trusted a claim it should have checked elsewhere. |
| Host header injection ([CWE-640](https://cwe.mitre.org/data/definitions/640.html)) | Request a reset email for your own account. Find the reset POST in ZAP and compare its `Host` header with the host in the emailed link. | The emailed reset link uses the host named by the incoming request. | Replay the reset request with a different `Host` value and inspect your own reset email. | ZAP Request Editor or `curl -i -H 'Host: test.example' -d 'email=OWN_EMAIL' RESET_URL`. | If the tokenized link points to `test.example`, a victim who clicks it could send the reset token to that host. Changing the header without changing the email proves nothing yet. |
| Session hijacking ([CWE-613](https://cwe.mitre.org/data/definitions/613.html)) | Sign in, open your account page, and sign out. Find the login response, account request, and logout request in ZAP; copy only your own token. | Tokens are guessable, appear in URLs, never expire, or remain valid after logout. | Compare several tokens you created; look for a pattern. Replay your saved token after logout or after the stated timeout. Inspect whether a URL token appears in history or an actual `Referer` request. | ZAP History and `curl -i -b 'session=OLD_TOKEN' ACCOUNT_URL` (or copy the site's authorization header). | A working old or predicted token lets its holder act as the user without another login. A pattern or missing cookie flag alone is a clue, not proof of takeover. |
| Session fixation ([CWE-384](https://cwe.mitre.org/data/definitions/384.html)) | Open the site before signing in, then sign in. Compare the session ID in the two ZAP requests and the login response. | The same ID survives the login. | Use the pre-login ID in a separate test browser after the account logs in. | ZAP compares `Set-Cookie` before and after login; `curl -i -b 'session=PRELOGIN_ID' ACCOUNT_URL` tests reuse. | If the old ID opens the signed-in account, someone who planted that ID earlier could inherit the user's session. |

### Server

A basket number illustrates the central server-side question: logged in as whom, and allowed to open which basket? If Alice's token with Bob's basket number returns Bob's products, login worked but ownership was never checked. If the server returns `403`, confirm that Alice's own basket still works with the same token; otherwise the token itself might simply be invalid.

| Attack | Start in the ZAP browser | What to look for | Change or test by hand | Kali tool or command | Why it matters; expected result |
| --- | --- | --- | --- | --- | --- |
| IDOR, an object access failure ([CWE-639](https://cwe.mitre.org/data/definitions/639.html)) | Create two accounts, put different products in their baskets, and open each basket. Find the two `/rest/basket/ID` requests in ZAP. | The object number appears in the URL or request body. | Keep account A's token and substitute account B's object number. | ZAP Request Editor; `curl -i -H 'Authorization: Bearer A_TOKEN' http://127.0.0.1:3000/rest/basket/B_ID`. | B's data in A's response proves the server checked the login but failed to check ownership of the object. |
| Mass assignment ([CWE-915](https://cwe.mitre.org/data/definitions/915.html)) | Change your display name and save it. In ZAP History, open the profile-update request and read the JSON fields the browser sent. | The request sends a JSON object that might be copied straight into the account record. | Add a field the form did not offer, such as `"isAdmin":true`, then fetch the account again. | ZAP Request Editor; a captured JSON request is more reliable than inventing a new path. | If the extra field is saved, the server let the caller change a field the user interface never offered. |
| Forced browsing ([CWE-862](https://cwe.mitre.org/data/definitions/862.html)) | Download one order receipt. In ZAP Sites, look at the neighboring file paths and try to work out how another receipt's name would be formed. | Paths or file names are guessable, while some pages have no visible link. | Send a direct request to an unlinked but plausible path as a guest or ordinary user. | ZAP Sites tree and `curl -i http://TARGET/KNOWN_PATH`. | A protected page opening directly shows that hiding its link was mistaken for an access check. |
| OS command injection ([CWE-78](https://cwe.mitre.org/data/definitions/78.html)) | Use a website's "check connection" or "ping this host" form. Find the request carrying the hostname in ZAP History. | Punctuation in that input changes the server's output. | Add `; echo CS361TEST` to the captured input. | ZAP Request Editor; `curl -G --data-urlencode 'host=127.0.0.1; echo CS361TEST' PING_URL`. | The marker in command output suggests the server pasted user input into a shell command. This probe only makes sense if that feature runs a command. |
| Path traversal ([CWE-22](https://cwe.mitre.org/data/definitions/22.html)) | Download a receipt or support attachment. Find its GET request in ZAP and identify the filename chosen by the browser. | The server appears to build a file path from that name. | Add `../` to request a harmless file outside the intended folder. | ZAP Request Editor or `curl --path-as-is -i 'http://TARGET/DOWNLOAD/../../BENIGN_FILE'`. | Receiving the outside file proves the path escaped the directory the application meant to allow. |
| Insecure deserialization ([CWE-502](https://cwe.mitre.org/data/definitions/502.html)) | Choose "remember me" at login, if offered. Find the next request in ZAP and check whether a long cookie contains an encoded object. | Decoding it reveals object properties or class names. | Change one harmless property and resend it. | ZAP Decoder identifies the format; ZAP Request Editor replays the change. | A changed server-side value or a class-related error suggests the server turned caller-controlled bytes back into an object. Code execution requires an additional, format-specific weakness. |
| Server-side template injection, SSTI ([CWE-1336](https://cwe.mitre.org/data/definitions/1336.html)) | Set your display name to `CS361TEST`, or submit it to a page preview. Find the save request and the page that prints your name. | Template symbols are calculated instead of displayed. | Submit `{{7*7}}` and compare with a normal name. | ZAP Request Editor or `curl -G --data-urlencode 'name={{7*7}}' PREVIEW_URL`. | Seeing `49` suggests the server interpreted user text as a template expression. Other template engines use other symbols. |
| XML external entities, XXE ([CWE-611](https://cwe.mitre.org/data/definitions/611.html)) | Import an XML order or attach an XML support document. Find the upload POST and its response in ZAP History. | The server parses the XML and may follow an external entity. | Add `<!ENTITY p SYSTEM "file:///etc/hostname">` and `&p;` to a small XML document. | ZAP captures the upload and resends the modified XML. | The server's hostname in the response means the XML parser read a file on the server. No result might only mean the file was never parsed as XML. |
| Malicious file upload ([CWE-434](https://cwe.mitre.org/data/definitions/434.html)) | Attach a permitted small file to a complaint. Find the multipart upload POST in ZAP and note the returned filename or download URL. | The server trusts the filename or claimed file type, or serves uploads from a code-executing folder. | Change the filename, MIME type, and contents one at a time; begin with a harmless marker file. | ZAP shows the multipart upload; `curl -F 'file=@marker.txt;filename=marker.html' UPLOAD_URL` tests an alternate name. | Acceptance of a forbidden type shows weak checks. Running an uploaded script from the web root is a separate, stronger outcome. |
| Server-side request forgery, SSRF ([CWE-918](https://cwe.mitre.org/data/definitions/918.html)) | Set a profile image by pasting an image URL. Find the POST carrying that URL in ZAP, then watch which machine fetches the image. | The server, rather than the browser, visits the supplied URL. | Give it a URL for a small web server you control and check that server's log. | `python3 -m http.server 9001` starts a listener; submit `http://REACHABLE_TESTER_IP:9001/marker` through ZAP. | A request in the log proves the application fetched an attacker-chosen URL. That may let it reach internal services that an outside browser cannot reach. |

### Data

| Attack | Start in the ZAP browser | What to look for | Change or test by hand | Kali tool or command | Why it matters; expected result |
| --- | --- | --- | --- | --- | --- |
| SQL injection ([CWE-89](https://cwe.mitre.org/data/definitions/89.html)) | Search for `apple` and find `GET /rest/products/search?q=apple` in ZAP. Save that normal response before changing `q`. | A quote causes a database error, or changing the input changes which records appear. | Compare a condition that must be true (`AND 1=1`) with one that must be false (`AND 1=2`), adjusting quotes and parentheses to fit the query. A repeatable difference is stronger evidence than one error. | `sqlmap -u 'http://127.0.0.1:3000/rest/products/search?q=apple' -p q --batch --risk=1 --level=1` tests one parameter \[38\]. For a login form, save ZAP's raw request and use `sqlmap -r request.txt -p FIELD --batch`. | The input has become part of the SQL command rather than a value to search for. Depending on what the query returns and the database account can do, that can bypass login, read other tables, or change data. |

The lecture's SQL techniques — commenting out the rest of a query, UNION, stacked statements, boolean or time-based blind tests, and error-based output — are different ways to use that same mistake. A database file write can become a web shell only if the database user can write to the web folder and the web server executes the written file. Those are additional conditions, not automatic results of every SQL injection.

### Response

| Attack | Start in the ZAP browser | What to look for | Change or test by hand | Kali tool or command | Why it matters; expected result |
| --- | --- | --- | --- | --- | --- |
| XSS from unencoded output ([CWE-79](https://cwe.mitre.org/data/definitions/79.html)) | Enter `CS361TEST` in a search, profile, or feedback field. Find the request and inspect the exact spot where its response places that text. | The marker lands in an HTML body, quoted attribute, JavaScript string, URL, or CSS rule without being made safe for that place. | Match the probe to its location: `<img src=x onerror=alert(1)>` for HTML; a quote followed by an event handler for an attribute; a string-ending quote for JavaScript; `javascript:alert(1)` only when an entire link URL is controlled. For CSS, a harmless style change proves CSS was interpreted; modern browsers do not generally execute JavaScript from CSS `url()`. | ZAP Response shows the exact bytes; browser Elements, Console, and Network show how the browser used them. | If a harmless alert runs, user text became browser code. The same text can be safe in one location and dangerous in another, which is why the response must encode for its actual context. |

### Sources

1. [Samy Kamkar, Technical explanation of The MySpace Worm](https://sa.my/myspace/tech.html)
2. [Samy Kamkar, "I'll never get caught. I'm Popular." (October 2005)](https://sa.my/myspace/)
3. [MITRE, 2025 CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/archive/2025/2025_cwe_top25.html)
4. [OWASP, OWASP Top 10:2025](https://owasp.org/Top10/2025/)
5. [UK Information Commissioner's Office, Penalty Notice: British Airways plc (October 16, 2020)](https://ico.org.uk/media/action-weve-taken/mpns/2618421/ba-penalty-20201016.pdf)
6. [Sansec, Polyfill supply chain attack hits 100K+ sites (June 25, 2024)](https://sansec.io/research/polyfill-supply-chain-attack)
7. [CISA, #StopRansomware: LockBit 3.0 Ransomware Affiliates Exploit CVE 2023-4966 Citrix Bleed Vulnerability (AA23-325A)](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-325a)
8. [Assetnote, Citrix Bleed: Leaking Session Tokens with CVE-2023-4966 (October 2023)](https://www.assetnote.io/resources/research/citrix-bleed-leaking-session-tokens-with-cve-2023-4966)
9. [Capital One, Information on the Capital One Cyber Incident (2019)](https://www.capitalone.com/digital/facts2019/)
10. [U.S. Department of Justice, Former Seattle tech worker convicted of wire fraud and computer intrusions (June 17, 2022)](https://www.justice.gov/usao-wdwa/pr/former-seattle-tech-worker-convicted-wire-fraud-and-computer-intrusions)
11. [U.S. Securities and Exchange Commission, SEC Charges Issuer With Cybersecurity Disclosure Controls Failures (June 15, 2021)](https://www.sec.gov/newsroom/press-releases/2021-102)
12. [CISA, #StopRansomware: CL0P Ransomware Gang Exploits CVE-2023-34362 MOVEit Vulnerability (AA23-158A)](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-158a)
13. [Progress Software, MOVEit Transfer Critical Vulnerability (May 31, 2023)](https://community.progress.com/s/article/MOVEit-Transfer-Critical-Vulnerability-31May2023)
14. [Emsisoft, Unpacking the MOVEit Breach: Statistics and Analysis](https://www.emsisoft.com/en/blog/44123/unpacking-the-moveit-breach-statistics-and-analysis/)
15. [U.S. Government Accountability Office, Data Protection: Actions Taken by Equifax and Federal Agencies in Response to the 2017 Breach (GAO-18-559, August 2018)](https://www.gao.gov/products/gao-18-559)
16. [CISA, Apache Log4j Vulnerability Guidance (December 2021)](https://www.cisa.gov/news-events/news/apache-log4j-vulnerability-guidance)
17. [Brave, Agentic Browser Security: Indirect Prompt Injection in Perplexity Comet (August 20, 2025)](https://brave.com/blog/comet-prompt-injection/)
18. [Chromium Blog, SameSite Cookie Changes in February 2020: What You Need to Know (February 3, 2020)](https://blog.chromium.org/2020/02/samesite-cookie-changes-in-february.html)
19. [OWASP Cheat Sheet Series, Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
20. [OWASP Cheat Sheet Series, SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
21. [MDN Web Docs, Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)
22. [MDN Web Docs, Same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
23. [IETF, RFC 6797: HTTP Strict Transport Security (HSTS) (November 2012)](https://www.rfc-editor.org/rfc/rfc6797)
24. [OWASP Cheat Sheet Series, Server-Side Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)
25. [AWS Security Blog, Add defense in depth against open firewalls, reverse proxies, and SSRF vulnerabilities with enhancements to the EC2 Instance Metadata Service (November 19, 2019)](https://aws.amazon.com/blogs/security/defense-in-depth-open-firewalls-reverse-proxies-ssrf-vulnerabilities-ec2-instance-metadata-service/)
26. [Veracode, AI-Generated Code Poses Major Security Risks in Nearly Half of All Development Tasks (July 30, 2025)](https://www.veracode.com/press-release/ai-generated-code-poses-major-security-risks-in-nearly-half-of-all-development-tasks-veracode-research-reveals/)
27. [rain forest puppy, NT Web Technology Vulnerabilities, Phrack 54 (December 25, 1998)](http://phrack.org/issues/54/8.html)
28. [Microsoft Security Blog, Mitigating the Axios npm supply chain compromise (April 1, 2026)](https://www.microsoft.com/en-us/security/blog/2026/04/01/mitigating-the-axios-npm-supply-chain-compromise/)
29. [Microsoft Security Blog, ChainDrop supply chain compromise: Anatomy of a self-propagating worm (August 4, 2026)](https://www.microsoft.com/en-us/security/blog/2026/08/04/chaindrop-supply-chain-compromise-anatomy-self-propagating-worm/)
30. [Verizon, 2026 Data Breach Investigations Report (May 2026)](https://www.verizon.com/business/resources/reports/dbir/)
31. [Cloud Security Alliance, Research Note: AI Code Generation and Vulnerability Debt (April 6, 2026)](https://labs.cloudsecurityalliance.org/research/csa-research-note-ai-codegen-vulnerability-debt-20260406-csa/)
32. [Wiz, Hacking Moltbook: The AI Social Network Any Human Can Control (February 2026)](https://www.wiz.io/blog/exposed-moltbook-database-reveals-millions-of-api-keys)
33. [OpenAI, Patch the Planet (June 22, 2026)](https://openai.com/blog/patch-the-planet)
34. [Cloudflare, How we protected customers from the PHP2Shell vulnerabilities (October 2026)](https://blog.cloudflare.com/php-vulnerabilities-waf-protection/)
35. [Wiz Research, PHP2Shell: AI uncovers critical RCE vulnerabilities affecting 70% of the Internet (October 21, 2026)](https://www.wiz.io/blog/php2shell-php-rce)
36. [The PHP Group, Security advisory: Critical remote code execution vulnerabilities, update now (October 21, 2026)](https://www.php.net/archive/2026.php#2026-10-21-5)
37. [OWASP, Zed Attack Proxy (ZAP)](https://www.zaproxy.org/)
38. [sqlmap, Automatic SQL injection and database takeover tool](https://sqlmap.org/)
