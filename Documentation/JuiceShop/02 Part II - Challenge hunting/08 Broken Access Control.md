---
title: "Broken Access Control"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part2/broken-access-control.html
tags: [juiceshop, owasp, companion-guide]
---

# Broken Access Control

Part of [[01 Challenge hunting|Challenge hunting]]
← Previous: [[07 Improper Input Validation|Improper Input Validation]]
Next: [[09 Security Misconfiguration|Security Misconfiguration]] →

> Most computer systems are designed for use with multiple users. Privileges mean what a user is permitted to do. Common privileges include viewing and editing files, or modifying system files.
>
> Privilege escalation means a user receives privileges they are not entitled to. These privileges can be used to delete files, view private information, or install unwanted programs such as viruses. It usually occurs when a system has a bug that allows security to be bypassed or, alternatively, has flawed design assumptions about how it will be used. Privilege escalation occurs in two forms:
>
> - Vertical privilege escalation, also known as *privilege elevation*, where a lower privilege user or application accesses functions or content reserved for higher privilege users or applications (e.g. Internet Banking users can access site administrative functions or the password for a smartphone can be bypassed.)
>
> - Horizontal privilege escalation, where a normal user accesses functions or content reserved for other normal users (e.g. Internet Banking User A accesses the Internet bank account of User B)[^1]

## Challenges covered in this chapter

| Name | Description | Difficulty |
|---|---|---|
| AI Debugging | Reveal some behind-the-scenes information on the chatbot as a non-admin user. | ⭐⭐ |
| Admin Section | Access the administration section of the store. | ⭐⭐ |
| CSRF | Change the name of a user by performing Cross-Site Request Forgery from another origin. | ⭐⭐⭐ |
| Easter Egg | Find the hidden easter egg. | ⭐⭐⭐⭐ |
| Five-Star Feedback | Get rid of all 5-star customer feedback. | ⭐⭐ |
| Forged Feedback | Post some feedback in another user’s name. | ⭐⭐⭐ |
| Forged Review | Post a product review as another user or edit any user’s existing review. | ⭐⭐⭐ |
| Manipulate Basket | Put an additional product into another user’s shopping basket. | ⭐⭐⭐ |
| Product Tampering | Change the `href` of the link within the OWASP SSL Advanced Forensic Tool (O-Saft) product description into *[https://owasp.slack.com](https://owasp.slack.com)*. | ⭐⭐⭐ |
| SSRF | Request a hidden resource on server through server. | ⭐⭐⭐⭐⭐⭐ |
| View Basket | View another user’s shopping basket. | ⭐⭐ |
| Web3 Sandbox | Find an accidentally deployed code sandbox for writing smart contracts on the fly. | ⭐ |

## Reveal some behind-the-scenes information on the chatbot as a non-admin user

The support chatbot has a debugging feature that is intended to be used only by administrators. This feature shows the internal tool calls and system messages, providing valuable insight into what the AI is doing behind the scenes.

- The chatbot has a debugging feature that shows how it interacts with its tools. It is only supposed to be visible for admins.

- Access control for the debugging feature is only implemented on the client-side.

- Find the cookie that controls the visibility of tool calls and set it to \<code>true\</code>.

- If you see the tool calls but the challenge is not marked as solved, you might be still logged in as a user with admin privileges.

## Access the administration section of the store

Just like the score board, the admin section was not part of your "happy path" tour because there seems to be no link to that section either. In case you were already [[03 Injection|logged in with the administrator account]] you might have noticed that not even for him there is a corresponding option available in the main menu.

- It is just slightly harder to find than the score board link.

- Knowing it exists, you can simply guess what URL the admin section might have.

- Alternatively, you can try to find a reference or clue within the parts of the application that are not usually visible in the browser.

- It is probably just slightly harder to find and gain access to than the score board link.

- There is some access control in place, but there are at least three ways to bypass it.

## Change the name of a user by performing Cross-Site Request Forgery from another origin

This challenge requires you to craft the code for the Cross-Site Request Forgery (CSRF) attack within [http://htmledit.squarefree.com](http://htmledit.squarefree.com), which represents another [origin](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) than the Juice Shop. You can enter your HTML and Script code in the upper half of the page, while the result is instantly displayed in the lower half.

- Find a form which updates the username and then construct a malicious page on the mentioned "another origin".

- Take a look at what happens when you change the username within the profile page.

- Search for information about CSRF attacks and look out for examples that can be applied to this challenge.

- If "another origin" is cross-site, you probably need an older browser version for this challenge. If it is cross-domain, you should be able to make it work in a modern browser.

|  |  |
|---|---|
|  | Please also be aware that the challenge is designed to be solved only with the specified online HTML editor. This means that Juice Shop will not recognize the challenge as solved when you are using another origin, such as for example JSFiddle or CodePen. |

## ⚠️ Important information about browser compatibility

There is an ongoing initiative, called "Incrementally Better Cookies", which aims to reduce the impact of CSRF attacks by changing the default cookie handling of browsers. It is strongly recommended to understand these changes before attempting this challenge, as otherwise it might not be solvable:

- An overview of the planned changes to `SameSite` can be found in [https://web.dev/samesite-cookie-recipes/](https://web.dev/samesite-cookie-recipes/).

- At the time of this writing, the challenge should work fine with Firefox 96.x or older.

- If you are in doubt, you can check the behavior of your browser using the website [https://samesite-sandbox.glitch.me/](https://samesite-sandbox.glitch.me/) - if you see the text "set ❌" in the first row below the column "Cross-site?", your browser uses the old default and should be able to solve the challenge.

- Although Chrome can be temporarily switched back to the old behavior by starting it with the option `--disable-features=SameSiteByDefaultCookies`, the CSRF attack will probably not execute anymore in Chrome 80.x or newer.

## Find the hidden easter egg

> An Easter egg is an intentional inside joke, hidden message, or feature in an interactive work such as a computer program, video game or DVD menu screen. The name is used to evoke the idea of a traditional Easter egg hunt.[^2]

- If you solved one of the four file access challenges, you already know where to find the easter egg.

- Simply reuse the trick that already worked for the files above.

|  |  |
|---|---|
|  | When you open the easter egg file, you might be a little disappointed, as the developers taunt you about not having found **the real** easter egg! Of course finding **that** is [[16 Cryptographic Issues\|a follow-up challenge]] to this one. |

## Get rid of all 5-star customer feedback

If you successfully solved above [admin section challenge](#_access_the_administration_section_of_the_store) deleting the 5-star feedback is very easy.

- Once you found admin section of the application, this challenge is almost trivial.

- Nothing happens when you try to delete feedback entries? Check the JavaScript console for errors!

## Post some feedback in another user’s name

The Juice Shop allows users to provide general feedback including a star rating and some free text comment. When logged in, the feedback will be associated with the current user. When not logged in, the feedback will be posted anonymously. This challenge is about vilifying another user by posting a (most likely negative) feedback in his or her name!

- You can solve this by tampering with the user interface or by intercepting the communication with the RESTful backend.

- To find the client-side leverage point, closely analyze the HTML form used for feedback submission.

- The backend-side leverage point is similar to some of the XSS challenges found in OWASP Juice Shop.

## Post a product review as another user or edit any user’s existing review

The Juice Shop allows users to provide reviews of all the products. A user has to be logged in before they can post any review for any of the products. This challenge is about vilifying another user by posting a (most likely bad) review in his or her name!

- Observe the flow of product review posting and editing and see if you can exploit it.

- This challenge can be solved by using developers tool of your browser or with tools like postman.

- Analyze the form used for review submission and try to find a leverage point.

- This challenge is pretty similar to "Post some feedback in another user’s name" challenge.

## Put an additional product into another user’s shopping basket

[View another user’s shopping basket](#_view_another_users_shopping_basket) was only about spying out other customers. For this challenge you need to get your hands dirty by putting a product into someone else’s basket that cannot be already in there!

- Have an eye on the HTTP traffic while placing products in the shopping basket.

- Adding more instances of the same product to someone else’s basket does not qualify as a solution. The same goes for stealing from someone else’s basket.

- This challenge requires a bit more sophisticated tampering than others of the same ilk.

## Change the href of the link within the O-Saft product description

The *OWASP SSL Advanced Forensic Tool (O-Saft)* product has a link in its description that leads to that projects wiki page. In this challenge you are supposed to change that link so that it will send you to [http://kimminich.de](http://kimminich.de) instead. It is important to exactly follow the challenge instruction to make it light up green on the score board:

- Theoretically there are three possible ways to beat this challenge: a) broken admin functionality, b) holes in RESTful API or c) possibility for SQL Injection.

- In practice two of these three ways should turn out to be dead ends.

## Request a hidden resource on server through server

This Server-side Request Forgery challenge will come back to the malware you used in [[03 Injection|Infect the server with juicy malware by abusing arbitrary command execution]].

- Reverse engineering something bad can make good things happen.

- Using whatever you find inside the malware directly will not do you any good.

- For this to count as an SSRF attack you need to make the Juice Shop server attack itself.

- Do not try to find the source code for the malware on GitHub. Take it apart with classic reverse-engineering techniques instead.

> In a Server-Side Request Forgery (SSRF) attack, the attacker can abuse functionality on the server to read or update internal resources. The attacker can supply or a modify a URL which the code running on the server will read or submit data to, and by carefully selecting the URLs, the attacker may be able to read server configuration such as AWS metadata, connect to internal services like http enabled databases or perform post requests towards internal services which are not intended to be exposed. [^3]

## View another user’s shopping basket

This horizontal privilege escalation challenge demands you to access the shopping basket of another user. Being able to do so would give an attacker the opportunity to spy on the victims shopping behaviour. He could also play a prank on the victim by manipulating the items or their quantity, hoping this will go unnoticed during checkout. This could lead to some arguments between the victim and the vendor.

- Try out all existing functionality involving the shopping basket while having an eye on the HTTP traffic.

- There might be a client-side association of user to basket that you can try to manipulate.

- In case you manage to update the database via SQL Injection so that a user is linked to another shopping basket, the application will not notice this challenge as solved.

## Find an accidentally deployed code sandbox

- It is just as easy as finding the Score Board.

------------------------------------------------------------------------

[^1]: [https://en.wikipedia.org/wiki/Privilege_escalation](https://en.wikipedia.org/wiki/Privilege_escalation)

[^2]: [https://en.wikipedia.org/wiki/Easter_egg_(media](https://en.wikipedia.org/wiki/Easter_egg_(media))

[^3]: [https://owasp.org/www-community/attacks/Server_Side_Request_Forgery](https://owasp.org/www-community/attacks/Server_Side_Request_Forgery)

[[07 Improper Input Validation|Improper Input Validation]] [[09 Security Misconfiguration|Security Misconfiguration]]

