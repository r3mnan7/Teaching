---
title: "Sensitive Data Exposure"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part2/sensitive-data-exposure.html
tags: [juiceshop, owasp, companion-guide]
---

# Sensitive Data Exposure

Part of [[01 Challenge hunting|Challenge hunting]]
← Previous: [[04 Broken Authentication|Broken Authentication]]
Next: [[06 XML External Entities (XXE)|XML External Entities (XXE)]] →

## Challenges covered in this chapter

| Name | Description | Difficulty |
|---|---|---|
| Confidential Document | Access a confidential document. | ⭐ |
| Email Leak | Perform an unwanted information disclosure by accessing data cross-domain. | ⭐⭐⭐⭐⭐ |
| Exposed Credentials | A developer was careless with hardcoding unused, but still valid credentials for a testing account on the client-side. | ⭐⭐ |
| Forgotten Developer Backup | Access a developer’s forgotten backup file. | ⭐⭐⭐⭐ |
| Forgotten Sales Backup | Access a salesman’s forgotten backup file. | ⭐⭐⭐⭐ |
| GDPR Data Theft | Steal someone else’s personal data without using Injection. | ⭐⭐⭐⭐ |
| Leaked API Key | Inform the shop about a leaked API key. (Mention the exact key in your comment) | ⭐⭐⭐⭐⭐ |
| Leaked Unsafe Product | Identify an unsafe product that was removed from the shop and inform the shop which ingredients are dangerous. | ⭐⭐⭐⭐ |
| Login Amy | Log in with Amy’s original user credentials. (This could take 93.83 billion trillion trillion centuries to brute force, but luckily she did not read the "One Important Final Note") | ⭐⭐⭐ |
| Login Cloud Admin | Log in with the cloud admin’s user account *without* using SQL Injection. | ⭐⭐⭐⭐ |
| Login MC SafeSearch | Log in with MC SafeSearch’s original user credentials without applying SQL Injection or any other bypass. | ⭐⭐ |
| Meta Geo Stalking | Determine the answer to John’s security question by looking at an upload of him to the Photo Wall and use it to reset his password via the Forgot Password mechanism. | ⭐⭐ |
| NFT Takeover | Take over the wallet containing our official Soul Bound Token (NFT). | ⭐⭐ |
| Password Hash Leak | Obtain the password (hash) of the currently logged-in user directly from a REST API endpoint. | ⭐⭐ |
| Reset Uvogin’s Password | Reset Uvogin’s password via the Forgot Password mechanism with *his original answer* to his security question. | ⭐⭐⭐⭐ |
| Retrieve Blueprint | Deprive the shop of earnings by downloading the blueprint for one of its products. | ⭐⭐⭐⭐⭐ |
| Visual Geo Stalking | Determine the answer to Emma’s security question by looking at an upload of her to the Photo Wall and use it to reset her password via the Forgot Password mechanism. | ⭐⭐⭐⭐ |

## Access a confidential document

Somewhere in the application you can find a file that contains sensitive information about some - potentially hostile - takeovers the Juice Shop top management has planned.

- Analyze and tamper with links in the application that deliver a file directly.

- The file you are looking for is not protected in any way. Once you found it you can also access it.

## Perform an unwanted information disclosure by accessing data cross-domain

Somewhere in the application there is an API endpoint which will allow data to be accessed cross domain. Usually the same-origin policy would prevent this but this endpoint has a special feature enabled which will allow cross domain access under certain circumstances.

- Try to find and attack an endpoint that responds with user information. SQL Injection is not the solution here.

- What ways are there to access data from a web application cross-domain?

- This challenge uses an old way which is no longer recommended.

## A developer was careless with hardcoding unused but still valid credentials

- Have a look at the client-side code in the dev console.

## Access a developer’s forgotten backup file

During an emergency incident and the hotfix that followed, a developer accidentally pasted an application configuration file into the wrong place. Downloading this file will not only solve the *Access a developer’s forgotten backup file* challenge but might also prove crucial in several other challenges later on.

- You need to trick a security mechanism into thinking that the file you want has a valid file type.

- Analyze and tamper with links in the application that deliver a file directly.

- The file is not directly accessible because a security mechanism prevents access to it.

- You need to trick the security mechanism into thinking that the file has a valid file type.

- For this challenge there is only one approach to pull this trick.

## Access a salesman’s forgotten backup file

A salesperson as accidentally uploaded a list of (by now outdated) coupon codes to the application. Downloading this file will not only solve the *Access a salesman’s forgotten backup file* challenge but might also prove useful in another challenge later on.

- You need to trick a security mechanism into thinking that the file you want has a valid file type.

- Analyze and tamper with links in the application that deliver a file directly.

- The file is not directly accessible because a security mechanism prevents access to it.

- You need to trick the security mechanism into thinking that the file has a valid file type.

## Steal someone else’s personal data without using Injection

In order to comply with GDPR, the Juice Shop offers a *Request Data Export* function for its registered customers. It is possible to exploit a flaw in the feature to retrieve more data than intended. Injection attacks will not count to solve this one.

- Trick the regular Data Export to give you more than actually belongs to you.

- You should not try to steal data from a "vanilla" user who never even ordered something at the shop.

- As everything about this data export functionality happens on the server-side, it won’t be possible to just tamper with some HTTP requests to solve this challenge.

- Inspecting various server responses which contain user-specific data might give you a clue about the mistake the developers made.

## Inform the shop about a leaked API key

> Public REST services without access control run the risk of being farmed leading to excessive bills for bandwidth or compute cycles. API keys can be used to mitigate this risk. They are also often used by organisation to monetize APIs; instead of blocking high-frequency calls, clients are given access in accordance to a purchased access plan.
>
> API keys can reduce the impact of denial-of-service attacks. However, when they are issued to third-party clients, they are relatively easy to compromise.[^1]

- The API call is part of a scheduled process "behind the scenes", i.e. completely unrelated to the web application.

- Check the Juice Shop’s social media channels for regularly scheduled content being posted, possibly even indicating that it was automatically created.

- Find out which part of the content might come from the response of an API call.

- Find the place where the API call happens — as stated above, it is not in the web application — and then look for the API key itself.

## Identify an unsafe product that was removed from the shop and inform the shop which ingredients are dangerous

Similar to [[17 Observability Failures|Dumpster dive the Internet for a leaked password and log in to the original user account it belongs to]] this challenge further highlights the risks from a lack of data loss prevention.

- You must first identify the "unsafe product" which ist not available any more in the shop.

- Solving the "Order the Christmas special offer of 2014" challenge might give it to you as by-catch.

- The actual data you need to solve this challenge was leaked on the same platform that was involved in the "Dumpster dive the Internet for a leaked password and log in to the original user account it belongs to" challenge.

- Google is a particularly good accomplice in this challenge.

## Log in with Amy’s original user credentials

This challenge is similar to [[04 Broken Authentication|Log in with the administrators user credentials without previously changing them or applying SQL Injection]] in the sense that only using her original credentials will work as a challenge solutions.

![[part2-sensitive-data-exposure-amy_and_kif.jpg]]

- This challenge will make you go after a needle in a haystack.

- As with so many other characters from Futurama this challenge is of course about logging in as Amy from that show.

- Did you know that Amy is married to an alien named Kif?

- The challenge description contains a few sentences which give away some information how Amy decided to strengthen her password.

- Obviously, Amy - being a little dimwitted - did not put nearly enough effort and creativity into the password selection process.

## Log in with the cloud admin’s user account

This challenge is another one where you need to find some credentials or a way to authenticate as a specific user without being able to just exploit a technical vulnerability like SQL Injection.

- Brute forcing the password will not work. The password is too strong.

- There might be another way to authenticate besides using the correct password.

- Look for sensitive information that was accidentally deployed alongside the application.

- Some Terraform file in the infrastructure directory accessible from the web application contains sensitive information that can be used to forge authentication tokens.

## Log in with MC SafeSearch’s original user credentials

Another user login challenge where only the original password is accepted as a solution. Employing SQL Injection or other attacks does not count.

- After watching the music video of this song, you should agree that even ⭐⭐ is a slightly exaggerated difficulty rating for this challenge.

  ![[part2-sensitive-data-exposure-protect-ya-passwordz.jpg]]

## Determine the answer to John’s security question

Who would have guessed that a simple walk in the park could lead to an account compromise. People these days are not careful with what they post online and are not aware of the possible consequences it can have when people exploit that.

- Take a look at the meta data of the corresponding photo.

- Make use of tools that can inspect the metadata of images.

- Use this information to answer the security question of the John, who enjoys hiking in the park.

## Take over the wallet containing our official Soul Bound Token

- Find the seed phrase posted accidentally.

## Obtain the password (hash) of the currently logged-in user directly from a REST API endpoint

While modern web applications often separate the frontend from the backend via a REST API, developers sometimes forget that the API might return more data than the frontend actually needs or displays. This challenge is about identifying such an endpoint that leaks the password hash of the logged-in user.

- You need to understand what happens "behind the scenes", so make sure to use your DevTools or a proxy to inspect network traffic.

- Look for an API endpoint that already returns some user information.

- An overly generic solution for data retrieval can backfire if not properly safeguarded.

## Reset Uvogin’s password via the Forgot Password mechanism

With the amount of personal information that people tend to reveal online, security questions are hardly reliable anymore.

- You might have to do some OSINT on his social media personas to find out his honest answer to the security question.

- People often reuse aliases online. You might be able to find something by looking online for Uvogin’s name or slight variations of it based on his unique writing habits.

- You might be able to find some existing OSINT tools to help you in this investigation.

## Deprive the shop of earnings by downloading the blueprint for one of its products

Why waste money for a product when you can just as well get your hands on its blueprint in order to make it yourself?

- Check for products which seem like a natural fit for being based on a blueprint.

- You might want to pay attention to the images of the identified product candidates.

- For your inconvenience the blueprint was not misplaced into the same place like so many others forgotten files covered in this chapter.

|  |  |
|---|---|
|  | If you are running the Juice Shop with a custom theme and product inventory, the product to inspect will be a different one. The tooltip on the Score Board will tell you which one to look into. |

## Determine the answer to Emma’s security question

It is also possible to determine where a picture was taken by looking at visual clues within the image. A certain user has uploaded a picture of his old workplace. Take a look at what his security question is and see if you can find the answer by looking at his uploaded image.

- Take a look at the details in the photo to determine the location of where it was taken.

------------------------------------------------------------------------

[^1]: [https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#api-keys](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#api-keys)

[[04 Broken Authentication|Broken Authentication]] [[06 XML External Entities (XXE)|XML External Entities (XXE)]]

