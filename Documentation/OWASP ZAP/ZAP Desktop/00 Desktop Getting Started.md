---
title: "Desktop Getting Started"
source: https://www.zaproxy.org/docs/desktop/start/
tags: [owasp-zap, zap-desktop]
---

# Desktop Getting Started

Next: [[01 Scanner Rules|Scanner Rules]] →

The quickest way to get going with ZAP is to use the Quick Start add-on, which is installed by default.

This allows you to enter a URL which ZAP will first spider and then [[05 Active Scan|active scan]].

For a more in depth test you should explore your application using your browser or automated regression tests while proxying through ZAP.

At its heart ZAP is a [[16 Manipulator-in-the-middle Proxy|manipulator-in-the-middle proxy]].

To get the most out of ZAP you need to configure your browser or functional tests to connect to the web application you wish to test through ZAP.

If required you can also configure ZAP to connect through another proxy - this is often necessary in a corporate environment.

You can quickly and easily launch browsers that are pre-configured to proxy through ZAP via the Quick Start tab. This can launch any of the most common browsers that you have installed with new profiles.

If you would like to use any of your browsers with an existing profile, for example with other browser add-ons installed, then you will need to manually configure your browser to proxy via ZAP.

If you know how to set up proxies in your web browser then go ahead and give it a go!

If you are unsure then have a look at the [[04 Configuring Proxies|Configuring proxies]] section.

Once you have configured ZAP as your browser’s proxy then try to connect to the web application you will be testing.

If you can not connect to it then check your proxy settings again. You will need to check your browser’s proxy settings, and also ZAP’s proxy settings.

Its also worth checking that the application that you are trying to test is running!

When you have successfully connected to your application via your browser then have a look at ZAP again. You should now see one or more lines in the [Sites](https://www.zaproxy.org/docs/desktop/ui/tabs/sites/) and [History](https://www.zaproxy.org/docs/desktop/ui/tabs/history/) tabs.

If so we’re in business. If not then you’ll need to check your proxy settings again.

The next thing to do is to start a [[03 A Basic Penetration Test|basic penetration test]].

## See also

- [[04 Configuring Proxies]] — for details of how to set up ZAP as a proxy in your web browser
- [Introduction](https://www.zaproxy.org/docs/desktop/) — the introduction to ZAP
- [[00 Features]] — provided by ZAP
- [[01 Scanner Rules]] — supported by default

## Official Videos

- [ZAPCon 2022: ZAP for Everybody](https://www.youtube.com/watch?v=32W_hm30dsg) (44:05)

