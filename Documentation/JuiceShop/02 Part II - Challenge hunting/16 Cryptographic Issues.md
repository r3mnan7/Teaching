---
title: "Cryptographic Issues"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part2/cryptographic-issues.html
tags: [juiceshop, owasp, companion-guide]
---

# Cryptographic Issues

Part of [[01 Challenge hunting|Challenge hunting]]
← Previous: [[15 Broken Anti-Automation|Broken Anti-Automation]]
Next: [[17 Observability Failures|Observability Failures]] →

## Challenges covered in this chapter

| Name | Description | Difficulty |
|---|---|---|
| Forged Coupon | Forge a coupon code that gives you a discount of at least 80%. | ⭐⭐⭐⭐⭐⭐ |
| Imaginary Challenge | Solve challenge \#999. Unfortunately, this challenge does not exist. | ⭐⭐⭐⭐⭐⭐ |
| Nested Easter Egg | Apply some advanced cryptanalysis to find *the real* easter egg. | ⭐⭐⭐⭐ |
| Premium Paywall | Unlock Premium Challenge to access exclusive content. | ⭐⭐⭐⭐⭐⭐ |
| Weird Crypto | Inform the shop about an algorithm or library it should definitely not use the way it does. | ⭐⭐ |

## Forge a coupon code that gives you a discount of at least 80%

This is probably one of the hardest challenges in the OWASP Juice Shop. As you learned during [[06 Walking the happy path|the "happy path" tour]], the web shop offers a *Coupon* field to get a discount on your entire order during checkout. The challenge is to get a discount of at least 80% on an order. As no coupons with this high a discount are published, it is up to you to forge your own.

- Try either a) a knowledgeable brute force attack or b) reverse engineering or c) some research in the cloud.

- One viable solution would be to reverse-engineer how coupon codes are generated and craft your own 80% coupon by using the same (or at least similar) implementation.

- Another possible solution might be harvesting as many previous coupon as possible and look for patterns that might give you a leverage for a brute force attack.

- If all else fails, you could still try to blindly brute force the coupon code field before checkout.

## Solve challenge \#999

The OWASP Juice Shop is *so broken* that even its convenience features (which have nothing to do with the e-commerce use cases) are designed to be vulnerable. One of these features is [[04 Challenge tracking|Automatic saving and restoring hacking progress]] after a server crash or a few days pause.

In order to not mess with the *real challenges* accidentally, the challenge is to fake a signal to the application that you successfully solved challenge \#999 - which does not exist.

- You need to trick the hacking progress persistence feature into thinking you solved challenge \#999.

- Find out how saving and restoring progress is done behind the scenes.

- Deduce from all available information (e.g. the package.json.bak) how the application encrypts and decrypts your hacking progress.

- Other than the user’s passwords, the hacking progress involves an additional secret during its encryption.

- What would be a really stupid mistake a developer might make when choosing such a secret?

## Apply some advanced cryptanalysis to find the real easter egg

Solving the [[08 Broken Access Control|Find the hidden easter egg]] challenge was probably no as satisfying as you had hoped. Now it is time to tackle the taunt of the developers and hunt down *the real* easter egg. This follow-up challenge is basically about finding a secret URL that - when accessed - will reward you with an easter egg that deserves the name.

- You might have to peel through several layers of tough-as-nails encryption for this challenge.

- Make sure you solve Find the hidden easter egg first.

- You might have to peel through several layers of tough-as-nails encryption for this challenge.

## Unlock Premium Challenge to access exclusive content

These days a lot of seemingly free software comes with hidden or follow-up costs to use it to its full potential. For example: In computer games, letting players pay for *Downloadable Content* (DLC) after they purchased a full-price game, has become the norm. Often this is okay, because the developers actually *added* something worth the costs to their game. But just as often gamers are supposed to pay for *just unlocking* features that were already part of the original release.

This hacking challenge represents the latter kind of "premium" feature. *It only exists to rip you hackers off!* Of course you should never tolerate such a business policy, let alone support it with your precious Bitcoins!

That is why the actual challenge here is to unlock and solve the "premium" challenge *bypassing the paywall* in front of it.

- You do not have to pay anything to unlock this challenge! Nonetheless, donations are very much appreciated.

- There is no inappropriate, self-written or misconfigured cryptographic library to be exploited here.

- How much protection does a sturdy top-quality door lock add to your house if you put the key under the door mat? Or hide the key in the nearby plant pot? Or tape the key to the underside of the mailbox?

- Once more: You do not have to pay anything to unlock this challenge!

> Side note: The Bitcoin address behind the taunting *Unlock* button is actually a valid address of the author. So, if you’d like to donate a small amount for the ongoing maintenance and development of OWASP Juice Shop — feel free to actually use it! More on [[03 Donations|donations in part 3]] of this book.

## Inform the shop about an algorithm or library it should definitely not use the way it does

To fulfil this challenge you must identify a cryptographic algorithm (or crypto library) that either

- should not be used *at all*

- or is a *bad choice* for a given requirement

- or is used in an *insecure way*.

> Initially confined to the realms of academia and the military, cryptography has become ubiquitous thanks to the Internet. Common every day uses of cryptography include mobile phones, passwords, SSL, smart cards, and DVDs. Cryptography has permeated everyday life, and is heavily used by many web applications.
>
> Cryptography (or crypto) is one of the more advanced topics of information security, and one whose understanding requires the most schooling and experience. It is difficult to get right because there are many approaches to encryption, each with advantages and disadvantages that need to be thoroughly understood by web solution architects and developers. In addition, serious cryptography research is typically based in advanced mathematics and number theory, providing a serious barrier to entry.
>
> The proper and accurate implementation of cryptography is extremely critical to its efficacy. A small mistake in configuration or coding will result in removing a large degree of the protection it affords and rending the crypto implementation useless against serious attacks.
>
> A good understanding of crypto is required to be able to discern between solid products and snake oil. The inherent complexity of crypto makes it easy to fall for fantastic claims from vendors about their product. Typically, these are “a breakthrough in cryptography” or “unbreakable” or provide "military grade" security. If a vendor says "trust us, we have had experts look at this,\`" chances are they weren’t experts![^1]

- Report one of five possible answers via the "Customer Feedback" form.

- Cryptographic functions only used in the "Apply some advanced cryptanalysis to find the real easter egg" challenge do not count as they are only a developer’s prank and not a serious security problem.

------------------------------------------------------------------------

[^1]: [https://wiki.owasp.org/index.php/Guide_to_Cryptography](https://wiki.owasp.org/index.php/Guide_to_Cryptography)

[[15 Broken Anti-Automation|Broken Anti-Automation]] [[17 Observability Failures|Observability Failures]]

