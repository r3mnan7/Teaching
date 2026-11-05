---
title: "Data Driven Content"
source: https://www.zaproxy.org/docs/desktop/start/features/ddc/
tags: [owasp-zap, zap-desktop]
---

# Data Driven Content

Part of [[00 Features|Features]]
← Previous: [[12 Custom Page|Custom Page]]
Next: [[14 Globally Excluded URLs|Globally Excluded URLs]] →

Data driven content is type of [[29 Structural Modifiers|Structural Modifier]] which identifies URL paths that represent data.

In ’traditional’ web applications the structure of the application is typically defined by the URL paths and the data is contained in the URL parameters and POST data.

URLs like:

- <https://www.example.com/app/aaa?ddd=eee>
- <https://www.example.com/app/aaa?ddd=fff>
- <https://www.example.com/app/bbb?ddd=eee>

are represented in the [[26 Sites Tree]] as two ’nodes’ in the tree:

- Sites
  - <https://www.example.com>
    - app
      - GET:aaa(ddd)
      - GET:bbb(ddd)

The Sites tree is very important as it reflects ZAP’s understanding of the application structure.
If it is not a good representation of the structure then ZAP will not be able to attack the application effectively.

Some applications include data in URL paths.
For example:

- <https://www.example.com/app/company1/aaa?ddd=eee>
- <https://www.example.com/app/company2/aaa?ddd=fff>
- <https://www.example.com/app/company3/aaa?ddd=ggg>

These 3 URLs represent the same page but with different data, but by default ZAP will represent them as three separate nodes:

- Sites
  - <https://www.example.com>
    - app
      - company1
        - GET:aaa(ddd)
      - company2
        - GET:aaa(ddd)
      - company3
        - GET:aaa(ddd)

This is a problem because ZAP will now attack all 3 pages when it only needs to attack one of them.
In this case attacking the same page 3 times is not a big problem, but if you have hundreds or thousands of pages like this will significantly increase the time it takes to scan the application.

In ZAP terms the ‘company’ nodes are ‘data driven content’ - URL path elements that contain data instead of representing part of the application structure.
You can define data driven content by adding the application to a [[11 Contexts|Context]] and then configuring them via the [Sites tab](https://www.zaproxy.org/docs/desktop/ui/tabs/sites/) ‘Flag as Context -> *Context Name* Data driven node’ right click menu item
Once you have done this the pages will be correctly represented as 1 node:

- Sites
  - <https://www.example.com>
    - app
      - «company»
        - GET:aaa(ddd)

The characters « and » are used to indicate that this is a ‘special’ node and the node name (in this case ‘company’) can be set by you to indicate what that node represents.

## Accessed via

- [Sites tab](https://www.zaproxy.org/docs/desktop/ui/tabs/sites/) — ‘Flag as Context -> *Context Name* Data driven node’ right click menu item
- [History tab](https://www.zaproxy.org/docs/desktop/ui/tabs/history/) — ‘Flag as Context -> *Context Name* Data driven node’ right click menu item

## See also

- [UI Overview](https://www.zaproxy.org/docs/desktop/ui/) — for an overview of the user interface
- [[00 Features]] — provided by ZAP
- [[30 Structural Parameters]] — which identify parameters that represent application structure instead of user data
- [Session Context Structure screen](https://www.zaproxy.org/docs/desktop/ui/dialogs/session/context-struct/)

