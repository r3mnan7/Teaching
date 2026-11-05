---
title: "Why the Juice Shop exists"
source: https://pwning.owasp-juice.shop/companion-guide/latest/introduction/motivation.html
tags: [juiceshop, owasp, companion-guide]
---

# Why the Juice Shop exists

Part of [[01 Introduction|Introduction]]
← Previous: [[01 Introduction|Introduction]]
Next: [[03 Architecture overview|Architecture overview]] →

To the unsuspecting user the Juice Shop just looks like a small online shop which sells - *surprise!* - fruit & vegetable juice and associated products. Except for the entirely overrated payment and delivery aspect of the e-commerce business, the Juice Shop is fully functional. But this is just the tip of the iceberg. The Juice Shop contains 116 challenges of varying difficulty where you are supposed to exploit underlying security vulnerabilities. These vulnerabilities were intentionally planted in the application for exactly that purpose, but in a way that actually happens in "real-life" web development as well!

Your hacking progress is tracked by the application using immediate push notifications for successful exploits as well as a score board for progress overview. Finding this score board is actually one of the (easiest) challenges! The idea behind this is to utilize [gamification](https://en.wikipedia.org/wiki/Gamification) techniques to motivate you to get as many challenges solved as possible - similar to unlocking achievements in many modern video games.

Development of the Juice Shop started in September 2014 as the author’s personal initiative, when his employer needed a more modern security training exercise environment for an in-house web application. The previously used environment was still from the era of server-side rendered ASP/JSP/Servlet and did not reflect the reality of current web technology. The Juice Shop was developed as open-source software without any corporate branding right from the beginning. By the end of 2014, most of the current e-commerce functionality was up and running

- along with an initial number of planted vulnerabilities. Over the years more variants of vulnerabilities were added. In parallel, the application was kept up-to-date with the latest web technology (e.g. WebSockets and OAuth 2.0) and frontend frameworks (i.e. by migrating from AngularJS with Bootstrap to Angular with Material Design). Some of these additional capabilities brought the chance to add corresponding vulnerabilities - and the list of challenges has been growing ever since.

Apart from the hacker and awareness training use case, penetration testing tools and automated security scanners are invited to use the Juice Shop as a sort of guinea pig-application to check how well their products cope with JavaScript-heavy application frontends and REST APIs.

### Why *OWASP* Juice Shop?

> The Open Worldwide Application Security Project (OWASP) is a nonprofit foundation that works to improve the security of software. Our programming includes:
>
> Community-led open source software projects Over 275 local chapters worldwide Tens of thousands of members Industry-leading educational and training conferences We are an open community dedicated to enabling organizations to conceive, develop, acquire, operate, and maintain applications that can be trusted. All of our projects, tools, documents, forums, and chapters are free and open to anyone interested in improving application security. The OWASP Foundation launched on December 1st, 2001, becoming incorporated as a United States non-profit charity on April 21, 2004. [^1]

Two years after its inception the Juice Shop was submitted and accepted as an *OWASP Tool Project* by the [Open Worldwide Application Security Project](https://owasp.org) in September 2016. This move increased the overall visibility and outreach of the project significantly, as it exposed it to a large community of application security practitioners.

Once in the OWASP project portfolio it took only eight months until Juice Shop was promoted from the initial *Incubator* maturity level to *Lab Projects* level. By the end of July 2018 the Juice Shop was promoted to the final *Flagship* maturity stage for OWASP projects.

![[introduction-motivation-Flagship_big.jpg]]

### Why the name "Juice Shop"?

In German there is a dedicated word for *dump*, i.e. a store that sells lousy wares and does not exactly have customer satisfaction as a priority: *Saftladen*. Reverse-translating this separately as *Saft* and *Laden* yields *juice* and *shop* in English. That is where the project name comes from. The fact that the initials *JS* match with those commonly used for *JavaScript* was purely coincidental and not related to the choice of implementation technology.

### Why the logo?

Other than the name, the Juice Shop logo was designed explicitly with *JavaScript* in mind:

![[introduction-motivation-JS_Shield.png]]

The author’s idea was to convert one of the (unofficial but popular) *JavaScript* shield-logos into a **leaking juice box** because it had a quite matching shape for this shenanigans:

![[introduction-motivation-JuiceShop_RetroLogo.png]]

In 2017 the logo received a facelift and a spin-off when the Juice Shop introduced its Capture-the-flag extension (which is discussed later in this book in context of the Multi Juicer platform in [[03 Hosting platform for multiple users|Hosting platform for multiple users]] and the dedicated chapter [[04 Hosting a CTF event|Hosting a CTF event]]).

![[introduction-motivation-JuiceShop_Logo_2017.png]] ![[introduction-motivation-JuiceShopCTF_Logo_2017.png]]

In 2026 the logos received their first update since 2017. The shapes in the artwork were kept exactly the same, only the color palette was updated to a more vibrant and "popping" one.

![[introduction-motivation-JuiceShop_Logo.png]] ![[introduction-motivation-JuiceShopCTF_Logo.png]]

### Why yet another vulnerable web application?

A considerable number of vulnerable web applications already existed before the Juice Shop was created. The [OWASP Vulnerable Web Applications Directory (VWAD)](https://owasp.org/www-project-vulnerable-web-applications-directory/) maintains a list of these applications. When the Juice Shop came to life there were only *server-side rendered* applications in the VWAD, but *Rich Internet Application (RIA)* or *Single Page Application (SPA)* style applications were already a commodity at that time. Juice Shop was meant to fill that gap.

Many of the existing vulnerable web applications were very rudimentary in their functional scope. So the aim of the Juice Shop was also to give the impression of a functionally complete e-commerce application that could actually exist like this in the wild.

------------------------------------------------------------------------

[^1]: [https://owasp.org/about/](https://owasp.org/about/)

[[01 Introduction|Introduction]] [[03 Architecture overview|Architecture overview]]

