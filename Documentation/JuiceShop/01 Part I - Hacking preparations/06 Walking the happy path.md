---
title: "Walking the happy path"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part1/happy-path.html
tags: [juiceshop, owasp, companion-guide]
---

# Walking the happy path

Part of [[01 Hacking preparations|Hacking preparations]]
← Previous: [[05 Hacking exercise rules|Hacking exercise rules]]
Next: [[01 Challenge hunting|Challenge hunting]] →

When investigating an application for security vulnerabilities, you should *never* blindly start throwing attack payloads at it. Instead, **make sure you understand how it works** before attempting any exploits.

> Before commencing security testing, understanding the structure of the application is paramount. Without a thorough understanding of the layout of the application, it is unlikely that it will be tested thoroughly. Map the target application and understand the principal workflows.[^1]

A good way to gain an understanding for the application, is to *actually use it* in the way it was meant to be used by a normal user. In regular software testing this is often called "happy path" testing.

> Also known as the "sunny day" scenario, the happy path is the "normal" path of execution through a use case or through the software that implements it. Nothing goes wrong, nothing out of the normal happens, and we swiftly and directly achieve the user’s or caller’s goal.[^2]

The OWASP Juice Shop is a rather simple e-commerce application that covers the typical workflows of a web shop. The following sections briefly walk you through these "happy path" use cases.

### Browse products

When visiting the OWASP Juice Shop you will begin on the landing page `#/` which initially displays all products offered in the shop. Clicking on the logo in the top left corner of the screen will always bring you back to this screen (or more precisely, to its alias `#/search`).

![[part1-happy-path-all-products.png]]

This is of course the "bread & butter" screen for any e-commerce site. When you click on the small "eye"-button next to the price of a product, an overlay screen will open showing you that product details including a list of customer reviews for that product (if available). You can also enter a new (or edit an existing) product review in this dialog. Authenticated users can upvote reviews they like.

![[part1-happy-path-product-details.png]]

You can use the *Search…* box in the navigation bar at the top of the screen to filter the table for specific products by their name and description. Using the controls at the bottom of the table, you can navigate through the result list that exceeds the *Items per page* limit.

![[part1-happy-path-search-results.png]]

### User login

While you can put items into the shopping cart as an anonymous user, you will eventually not be able to submit an order without an account. You first have to log in to the shop with your user credentials on the `#/login` page. There you can either log in with your existing credentials (if you are a returning customer) or with your Google account.

![[part1-happy-path-login.png]]

### User registration

In case you are a new customer, you must first register by following the corresponding link on the login screen to `#/register`. There you must enter your email address and a password to create a new user account. With these credentials you can then log in… and finally start shopping! During registration, you also choose and answer a security question that will let you recover the account if you ever forget your password.

![[part1-happy-path-user-registration.png]]

### Forgot Password

By providing your email address, the answer to your security question, and a new password, you can recover an otherwise inaccessible account.

![[part1-happy-path-forgot-password.png]]

### Choosing products to purchase

Using the "Add to Basket"-button on any item in the products table lets you add that product into your shopping basket. Clicking it multiple times will let you add more items of the same type, unless you reach a purchase-per-user limit or run into a stock shortage. The *Your Basket* button in the navigation bar will bring you to the `#/basket` page, where you can do several things before actually confirming your purchase:

- increase ("+") or decrease ("-") the quantity of individual products in the shopping basket

- remove products from the shopping basket with the "trashcan"-button

![[part1-happy-path-your-basket.png]]

### Checkout

During checkout, you will be guided through a series of steps to set your delivery address, desired delivery method, and credit card.

![[part1-happy-path-select-address.png]] ![[part1-happy-path-delivery-method.png]] ![[part1-happy-path-payment.png]]

|  |  |
|---|---|
|  | Do not enter any real credit card or address data anywhere in the Juice Shop! Always remember that it is not a real shop, and it is intentionally riddled with security and privacy flaws! |

In the *Add a coupon* section you can redeem a code for a discount. Unfold the *Other payment options* section to see links with donation and merchandise links of the Juice Shop open source project.

Finally, you can click the *Checkout* button to issue an order. You will be forwarded to a confirmation of your order right away. It also includes a link to a printable PDF confirmation for your order and as well as a tracking link.

![[part1-happy-path-order-completion.png]] ![[part1-happy-path-order-confirmation.png]]

### User Menu

Clicking the user icon right next to the application logo & title, you will give you access to several secondary use cases of the Juice Shop. This menu is obviously only available when you are logged in with your user account.

|  |  |
|---|---|
|  | We will cover only a fraction of the available functionality from the user menu in the following sub-sections. It is recommended to explore the rest on your own before diving into any hacking exercises. |

![[part1-happy-path-user-menu.png]]

![[part1-happy-path-user-menu2.png]]

#### User Profile

Clicking you your email address in the user menu, you will get to the *User Profile* screen on `/profile+`. Visiting it might break your user experience a bit, as it looks slightly less sophisticated than the rest of the shop’s UI. It is fully functional nonetheless, as it allows you to upload a `JPG`-format picture of yourself (or link an existing Gravatar) and choose a username for your account.

![[part1-happy-path-user-profile.png]]

#### My saved addresses

This page lists your saved addresses and provides you with the ability to edit or delete already saved addresses as well as add new ones.

![[part1-happy-path-stored-address.png]]

![[part1-happy-path-add-address.png]]

|  |  |
|---|---|
|  | Do not enter real address data here! |

#### My Payment Options

This page lists your saved cards and provides you with the ability to delete already saved cards or to add new ones.

![[part1-happy-path-stored-card.png]]

|  |  |
|---|---|
|  | Do not enter real credit card data here! |

#### Juice Shop Wallet

This page allows you to add money to your wallet and to check the existing balance. All the bonuses on your purchase are directly credited to your wallet.

![[part1-happy-path-wallet.png]]

|  |  |
|---|---|
|  | You can switch to a Web3 wallet with the "Try out our new Crypto Wallet"-link. You need to have one installed in your browser e.g. via MetaMask plugin to do so. This is only needed if you would like to do the Web3 challenges of Juice Shop. |

#### Order History

This page allows you to view the details of all your current and previous orders and the status of their delivery.

![[part1-happy-path-order-history.png]]

You can drill down into a specific order by clicking the truck-icon. Aprt from the products in that order you will now also see its delivery status, expected delivery date, and how many bonus points you earned.

![[part1-happy-path-track-orders-result.png]]

|  |  |
|---|---|
|  | Just as there was no "real" payment was happening, you will hopefully understand that there is no "real" order delivery happening - no matter what the order tracking dialog suggested. |

##### Privacy Policy

This page informs you about the policies regarding the collection, use, and disclosure of personal data when you use the OWASP Juice Shop and the choices you have when it comes to your data.

![[part1-happy-path-privacy-policy.png]]

##### Request Data Export

This page allows you to obtain a copy of all your data saved in the Juice Shop.

![[part1-happy-path-request-data-export.png]]

##### Request Data Erasure

This page allows you to request a complete erasure of your account and any associated data from the Juice Shop.

![[part1-happy-path-data-erasure-request.png]]

#### Change user password

If you are currently logged in you will find the obligatory *Change Password* button in the navigation bar. On the `#/privacy-security/change-password` page you can then choose a new password. To prevent abuse you have of course to supply your current password to legitimate this change.

![[part1-happy-path-change-password.png]]

##### 2FA Configuration

This page allows you to secure your account with an additional factor by providing you with a barcode to scan.

![[part1-happy-path-2fa-configuration.png]]

##### Last Login IP

This page displays the IP from which your account was last logged in.

![[part1-happy-path-last-login-ip.png]]

#### Request Recycling Box

When logged in you will furthermore see a *Recycle* button that brings you to the `#/recycle` page. This is a very innovative feature that allows eco-friendly customers to order pre-stamped boxes for returning fruit pressing leftovers to the Juice Shop.

![[part1-happy-path-request-recycling-box.png]]

For greater amounts of pomace the customer can alternatively order a truck to come by and pick it up at a chosen future date.

![[part1-happy-path-recycling-pickup.png]]

### Menu

The sidebar menu button left of the application logo reveals some more options to choose from.

![[part1-happy-path-menu.png]]

#### Customer Feedback

Customers are invited to leave feedback about their shopping experience with the Juice Shop. Simply visit the `#/contact` page by clicking the *Customer Feedback* menu item. You might recognize that it is also possible to leave feedback as an anonymous user. The contact form is very straightforward with a free text *Comment* field and a *Rating* on a 1-5 stars scale. To prevent abuse, you have to solve a simple mathematical problem before being allowed to submit your feedback.

![[part1-happy-path-contact-us.png]]

#### Complaint

The *Complaint?* menu item is shown only to logged in users. It brings you to the `#/complain` page where you can leave a free text *Message* and attach an *Invoice* file in case you had some issues with a recent order at the Juice Shop.

![[part1-happy-path-file-complaint.png]]

#### AI Chat

In the *AI Chat* you can talk to an LLM-powered chatbot and get answers to questions like product prices, deluxe membership benefits, and more.

![[part1-happy-path-support-chat.png]]

#### About Us

Like every proper enterprise, the OWASP Juice Shop has of course an `#/about` page titled *About Us*. There you find a summary of the interesting history of the shop along with a link to its official Terms of Use document. Additionally, the page displays a fancy illustrated slideshow of all [customer feedback](#_customer_feedback). Beneath that you can find all important social media contact information of the shop.

![[part1-happy-path-about-us.png]]

#### Photo Wall

The OWASP Juice Shop also has an `#/photo-wall` page titled *Photo Wall* which allows its users to share their memories with other customers of the Juice Shop.

![[part1-happy-path-photo-wall.png]]

#### Deluxe Membership

The OWASP Juice Shop offers a deluxe membership to its customers which provides them with exclusive offers, free fast delivery, and an unrestricted purchase of the items they like.

![[part1-happy-path-deluxe-membership.png]]

### Language selection

From a dropdown menu in the navigation bar you can select a multitude of languages you want the user interface to be displayed in. Languages marked with a "flask"-icon next to them offer only rudimentary or partial translation.

![[part1-happy-path-language-selection.png]]

|  |  |
|---|---|
|  | If you want to know more about (or even help with) the localization of OWASP Juice Shop, please refer to the [[06 Helping with translations\|Help with translation]] chapter in part III of this book. |

------------------------------------------------------------------------

[^1]: [https://wiki.owasp.org/index.php/Map_execution_paths_through_application_%28OTG-INFO-007%29](https://wiki.owasp.org/index.php/Map_execution_paths_through_application_%28OTG-INFO-007%29)

[^2]: [http://xunitpatterns.com/happy%20path.html](http://xunitpatterns.com/happy%20path.html)

[[05 Hacking exercise rules|Hacking exercise rules]] [[01 Challenge hunting|Challenge hunting]]

