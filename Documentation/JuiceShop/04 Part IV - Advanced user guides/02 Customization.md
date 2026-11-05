---
title: "Customization"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part4/customization.html
tags: [juiceshop, owasp, companion-guide]
---

# Customization

← Previous: [[01 Troubleshooting|Troubleshooting]]
Next: [[03 Hosting platform for multiple users|Hosting platform for multiple users]] →

One of the core usage scenarios for OWASP Juice Shop is in employee trainings in order to facilitate security awareness. With its not entirely serious user roster and product inventory the application might not be suited for all audiences alike.

In some particularly traditional domains or conservative enterprises it would be beneficial to have the demo application look and behave more like an internal application.

OWASP Juice Shop can be customized in its product inventory and look & feel to accommodate this requirement. It also allows to add an arbitrary number of fake users to make demonstrations - particularly those of `UNION-SQL` injection attacks - even more impressive. Furthermore the Challenge solved!-notifications can be turned off in order to keep the impression of a "real" application undisturbed.

## How to customize the application

The customization is powered by a YAML configuration file placed in `/config`. To run a customized OWASP Juice Shop you need to:

1. Place your own `.yml` configuration file into `/config`

2. Set the environment variable `NODE_ENV` to the filename of your config without the `.yml` extension

    - On Windows: `set NODE_ENV=nameOfYourConfig`

    - On Linux: `export NODE_ENV=nameOfYourConfig`

3. Run `npm start`

You can also run a config directly in one command (on Linux) via `NODE_ENV=nameOfYourConfig npm start`. By default, the `config/default.yml` config is used which generates the original OWASP Juice Shop look & feel and inventory. Please note that it is not necessary to run `npm install` after switching customization configurations.

### Overriding `default.yml` in Docker container

In order to override the default configuration inside your Docker container with one of the provided configs, you can pass in the `NODE_ENV` environment variable with the `-e` parameter:

``` highlightjs
docker run -d -e "NODE_ENV=bodgeit" -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```

In order to inject your own configuration, you can use `-v` to mount the `default.yml` path inside the container to any config file on your outside file system:

``` highlightjs
docker run -d -e "NODE_ENV=myConfig" -v /tmp/myConfig.yml:/juice-shop/config/myConfig.yml -p 3000:3000 --name juice-shop bkimminich/juice-shop
```

### Overriding `default.yml` in Vagrant box

Currently it is not possible to override the default configuration in the Vagrant box. It is not set up in a way where it could pass the `NODE_ENV` environment variable to the Docker container that is spun up inside the box.

## YAML configuration file

The YAML format for customizations is very straightforward. Below you find its schema along with an excerpt of the default settings.

### `server` section

Offers technical configuration options for the web server hosting the application.

| Property | Description | Default |
|---|---|---|
| `port` | Port to launch the server on. Would be overwritten by `PORT` environment variable. | `3000` |
| `basePath` | When proxied in a subdirectory, this base path is used *during redirects*. The actual web server’s base path is not affected. Would be overwritten by `BASE_PATH` environment variable. | `''` |

### `application` section

Defines customization options for texts, colors, images, URLs etc. within the application.

| Property | Description | Default |
|---|---|---|
| `domain` | Domain used for all user email addresses. | `'juice-sh.op'` |
| `name` | Name as shown in title and menu bar. | `'OWASP Juice Shop'` |
| `logo` | Filename in `frontend/dist/frontend/assets/public/images` *or* a URL[^1] of an image which will first be downloaded to that folder and then used as a logo. | `JuiceShop_Logo.png` |
| `favicon` | Filename in `frontend/dist/frontend/assets/public` *or* a URL[^1] of an image in `.ico` format which will first be download to that folder and then used as a favicon. | `favicon_v2.ico` |
| `theme` | Name of the color theme used to render the UI. Options are `bluegrey-lightgreen`, `blue-lightblue`, `deeppurple-amber`, `indigo-pink`, `pink-bluegrey`, `purple-green`, `deeporange-indigo`, `lime-green`, and `neon-fire`. See [Material Color Themes](#_material_color_themes) for a sample screenshot of each theme. | `bluegrey-lightgreen` |
| `showVersionNumber` | Shows or hides the software version from the title. | `true` |
| `showGitHubLinks` | Shows or hides the *"GitHub"* button in the navigation and side bar as well as the info box about contributing on the *Score Board*. | `true` |
| `localBackupEnabled` | Enabled or disables the local backup feature for hacking progress and filters/settings on the *Score Board*. | `true` |
| `numberOfRandomFakeUsers` | Represents the number of random user accounts to be created on top of the pre-defined ones (which are required for several challenges). | `0`, meaning no additional users are created |
| `altcoinName` | Defines the name of the (fake) crypto currency that is offered on the *Token Sale* screen. | `Juicycoin` |
| `privacyContactEmail` | The email address shown as contact in the *Privacy Policy*. | `donotreply@owasp-juice.shop` |
| `customMetricsPrefix` | Prefix for all custom Prometheus metrics. Must be a lowercase letter single world by Prometheus conventions. | `juiceshop` |
| [`chatBot` subsection](#_chatbot_subsection) | Specifies all characteristics of the bot answering user questions in the *Support Chat*. |  |
| [`social` subsection](#_social_subsection) | Specifies all social links embedded on various screens such as *About Us* or the *Photo Wall*. |  |
| [`recyclePage` subsection](#_recyclepage_subsection) | Defines custom elements on the *Request Recycling Box* page. |  |
| [`welcomeBanner` subsection](#_welcomebanner_subsection) | Defines a dismissable welcome banner that can be shown when first visiting the application. |  |
| [`cookieConsent` subsection](#_cookieconsent_subsection) | Defines the cookie consent dialog shown in the bottom right corner. |  |
| [`securityTxt` subsection](#_securitytxt_subsection) | Defines the attributes for the `security.txt` file based on the [https://securitytxt.org/](https://securitytxt.org/) Internet draft. |  |
| [`promotion` subsection](#_promotion_subsection) | Defines the attributes required for the `/promotion` screen where a marketing video with subtitles is rendered that hosts the [[10 Cross Site Scripting (XSS)\|XSS Tier 6]] challenge. |  |
| [`easterEggPlanet` subsection](#_eastereggplanet_subsection) | Defines the customizations for the 3D-rendered planet easter egg. |  |
| [`googleOauth` subsection](#_googleoauth_subsection) | Defines the client identifier and allowed redirect URIs for Google OAuthintegration. |  |

#### `chatBot` subsection

Specifies all characteristics of the bot answering user questions in the *Support Chat*.

| Property | Description | Default |
|---|---|---|
| `name` | Name the chat bot introduces itself with. | `Juicy the Smart Assistant` |
| `avatar` | Filename in `frontend/dist/frontend/assets/public/images` *or* a URL[^1] of an image which will first be download to that folder and then used as a chat bot avatar. | `'JuicyChatBot.png'` |
| `model` | The LLM model identifier used by the chat bot. | `'gemma4:e4b'` |
| `llmApiUrl` | URL of the OpenAI-compatible API endpoint for the chat bot LLM. Can be pointed at any compatible provider. | `'http://localhost:11434/v1'` |
| `llmMaxRetries` | Number of connection attempts to the OpenAI-compatible API endpoint before giving up and returning an error. | `2` |
| `sampleQuestions` | List of questions randomly chosen from when clicking the *Get a suggestion* button of the chatbot. Each can be the key of a localized string in `frontend/src/assets/i18n/en.json` or custom free-text. | `- 'CHATBOT_PROMPT_RECOMMENDATION_SUMMER_PARTY' - 'CHATBOT_PROMPT_RECOMMENDATION_POPULAR' - 'CHATBOT_PROMPT_RECOMMENDATION_SUGAR_FREE' - 'CHATBOT_PROMPT_RECOMMENDATION_START_DAY' - 'CHATBOT_PROMPT_RECOMMENDATION_SEASONAL'` |

|  |  |
|---|---|
|  | Any OpenAI-compatible endpoint can be used by changing `llmApiUrl` (e.g. OpenAI, Azure OpenAI, or self-hosted alternatives). API keys for authenticated endpoints are set via the `LLM_API_KEY` environment variable. |

#### `social` subsection

Specifies all social links embedded on various screens such as *About Us* or the *Photo Wall*.

| Property | Description | Default |
|---|---|---|
| `blueSkyUrl` | URL used as the BlueSky link promising coupon codes on the *My Payment Options* screen and on the *About Us* screen. | `'https://bsky.app/profile/owasp-juice.shop'` |
| `mastodonUrl` | URL used as the Mastodon link on the *About Us* screen. | `'https://fosstodon.org/@owasp_juiceshop'` |
| `twitterUrl` | URL used as the Twitter link on the *About Us* screen. | `'https://twitter.com/owasp_juiceshop'` |
| `facebookUrl` | URL used as the Facebook link on the *About Us* screen. | `'https://www.facebook.com/owasp.juiceshop'` |
| `slackUrl` | URL used as the Slack link on the *About Us* screen. | `'https://owasp.org/slack/invite'` |
| `redditUrl` | URL used as the Reddit link promising coupon codes on the *My Payment Options* screen and on the *About Us* screen. | `'https://www.reddit.com/r/owasp_juiceshop'` |
| `pressKitUrl` | URL used as the link to logos and media files on the *About Us* screen. | `'https://github.com/OWASP/owasp-swag/tree/master/projects/juice-shop'` |
| `questionnaireUrl` | URL used as the link to a user questionnaire on the *Score Board* screen. | `~` |

#### `recyclePage` subsection

Defines custom elements on the *Request Recycling Box* page.

| Property | Description | Default |
|---|---|---|
| `topProductImage` | Filename in `frontend/dist/frontend/assets/public/images/products` to use as the image on the top of the info column on the page. | `fruit_press.jpg` |
| `bottomProductImage` | Filename in `frontend/dist/frontend/assets/public/images/products` to use as the image on the bottom of the info column on the page. | `apple_pressings.jpg` |

#### `welcomeBanner` subsection

Defines a dismissable welcome banner that can be shown when first visiting the application.

| Property | Description | Default |
|---|---|---|
| `showOnFirstStart` | Shows or hides the banner. | `true` |
| `title` | Defines the headline of the banner. | `Welcome to OWASP Juice Shop!` |
| `message` | Defines the body of the banner. Can contain arbitrary HTML. | `<p>Being a web application with a vast number of intended security vulnerabilities, the <strong>OWASP Juice Shop</strong> is supposed to be the opposite of a best practice or template application for web developers: It is an awareness, training, demonstration and exercise tool for security risks in modern web applications. The <strong>OWASP Juice Shop</strong> is an open-source project hosted by the non-profit <a href='https://owasp.org' target='_blank'>Open Worldwide Application Security Project (OWASP)</a> and is developed and maintained by volunteers. Check out the link below for more information and documentation on the project.</p><h1><a href='https://owasp-juice.shop' target='_blank'>https://owasp-juice.shop</a></h1>` |

#### `cookieConsent` subsection

Defines the cookie consent dialog shown in the bottom right corner.

| Property | Description | Default |
|---|---|---|
| `backgroundColor` | Color of the cookie banner itself. | `'#546e7a'` |
| `textColor` | Color of the `message` shown in the cookie banner. | `'#ffffff'` |
| `buttonColor` | Defines the color of the button to dismiss the banner. | `'#558b2f'` |
| `buttonTextColor` | Color of the `dismissText` on the button. | `'#ffffff'` |
| `message` | Explains the cookie usage in the application. | `'This website uses fruit cookies to ensure you get the juiciest tracking experience.'` |
| `dismissText` | The text shown on the button to dismiss the banner. | `'Me want it!'` |
| `linkText` | Caption of the link that is shown after the `message` to refer to further information. | `'But me wait!'` |
| `linkUrl` | URL that provides further information about cookie usage. | `'https://www.youtube.com/watch?v=9PnbKL3wuH4'` |

#### `securityTxt` subsection

Defines the attributes for the `security.txt` file based on the [https://securitytxt.org/](https://securitytxt.org/) Internet draft.

| Property | Description | Default |
|---|---|---|
| `contact` | An email address, phone number or URL to report security vulnerabilities to. Can be fake obviously. | `mailto:donotreply@owasp-juice.shop` |
| `encryption` | URL to a public encryption key for secure communication. Can be fake obviously. | `https://keybase.io/bkimminich/pgp_keys.asc?fingerprint=19c01cb7157e4645e9e2c863062a85a8cbfbdcda` |
| `acknowledgements` | URL a "hall of fame" page. Can be fake obviously. | `/#/score-board` |

#### `promotion` subsection

Defines the attributes required for the `/promotion` screen where a marketing video with subtitles is rendered that hosts the [[10 Cross Site Scripting (XSS)|XSS Tier 6]] challenge.

| Property | Description | Default |
|---|---|---|
| `video` | Name of a file with `video/mp4` content type in `frontend/dist/frontend/assets/public/videos` *or* URL[^1] of an image to download to that folder and then use as the promotion video. | `owasp_promo.mp4` |
| `subtitles` | Name of a [Web Video Text Tracks Format](https://www.w3.org/TR/webvtt1/) file in `frontend/dist/frontend/assets/public/videos` *or* URL[^1] of an image to download to that folder and then use as the promotion video. | `owasp_promo.vtt` |

#### `easterEggPlanet` subsection

Defines the customizations for the 3D-rendered planet easter egg.

| Property | Description | Default |
|---|---|---|
| `name` | Name of the 3D planet "easter egg" as shown in the page title. | `Orangeuze` |
| `overlayMap` | Filename in `frontend/dist/frontend/assets/private` *or* URL[^1] of an image to download to that folder and then use as an overlay texture for the 3D planet "easter egg". | `orangemap2k.jpg` |

#### `googleOauth` subsection

Defines the client identifier and allowed redirect URIs for Google OAuth integration.

| Property | Description | Default |
|---|---|---|
| `clientId` | Client identifier of the Google Cloud Platform application to handle OAuth 2.0 requests from OWASP Juice Shop. | `'1005568560502-6hm16lef8oh46hr2d98vf2ohlnj4nfhq.apps.googleusercontent.com'` |
| [`authorizedRedirects` sub-sequence](#_authorizedredirects_sub_sequence) | Sub-list for the redirect URIs authorized for Google OAuth. | `- { uri: 'https://demo.owasp-juice.shop' }- { uri: 'https://juice-shop.herokuapp.com' }- { uri: 'https://preview.owasp-juice.shop' }- { uri: 'https://juice-shop-staging.herokuapp.com' }- { uri: 'https://juice-shop.wtf' }- { uri: 'http://localhost:3000', proxy: 'https://local3000.owasp-juice.shop' }- { uri: 'http://127.0.0.1:3000', proxy: 'https://local3000.owasp-juice.shop' }- { uri: 'http://localhost:4200', proxy: 'https://local4200.owasp-juice.shop' }- { uri: 'http://127.0.0.1:4200', proxy: 'https://local4200.owasp-juice.shop' }- { uri: 'http://192.168.99.100:3000', proxy: 'https://localmac.owasp-juice.shop' }- { uri: 'http://192.168.99.100:4200', proxy: 'https://localmac.owasp-juice.shop' }- { uri: 'http://penguin.termina.linux.test:3000', proxy: 'https://localchromeos.owasp-juice.shop' }- { uri: 'http://penguin.termina.linux.test:4200', proxy: 'https://localchromeos.owasp-juice.shop' }` |

##### `authorizedRedirects` sub-sequence

Defines the allowed redirect URIs and their optional proxy for Google OAuth integration.

| Property | Description | Conditions | Default |
|---|---|---|---|
| `uri` | URI authorized on [Google Cloud Platform](https://console.cloud.google.com/apis/credentials) the Juice Shop is expected to be running on. | mandatory |  |
| `proxy` | Proxy URI authorized on [Google Cloud Platform](https://console.cloud.google.com/apis/credentials) that will itself redirect back to the original `uri`. Necessary for addresses not allowed as Google OAuth redirect targets, such as `localhost` or IP addresses. | optional | `null` |

### `challenges` section

Defines configuration options for the hacking challenges within the Juice Shop.

<table class="tableblock frame-all grid-all stretch">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th class="tableblock halign-left valign-top">Property</th>
<th class="tableblock halign-left valign-top">Description</th>
<th class="tableblock halign-left valign-top">Default</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>showSolvedNotifications</code></p></td>
<td class="tableblock halign-left valign-top"><p>Shows or hides all instant <em>"challenge solved"</em>-notifications. Recommended to set to <code>false</code> for awareness demos.</p></td>
<td class="tableblock halign-left valign-top"><p><code>true</code></p></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>showHints</code></p></td>
<td class="tableblock halign-left valign-top"><p>Shows or hides hints for each challenge on hovering over/clicking a corresponding badge on the score board.</p></td>
<td class="tableblock halign-left valign-top"><p><code>true</code></p></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>showMitigations</code></p></td>
<td class="tableblock halign-left valign-top"><p>Shows or hides a mitigation link for each solved challenge on the score board (if available).</p></td>
<td class="tableblock halign-left valign-top"><p><code>true</code></p></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>codingChallengesEnabled</code></p></td>
<td class="tableblock halign-left valign-top"><p>Shows or hides the associated [[04 Challenge tracking|Coding challenge]] button for each challenge (if available) on the score board. Can be <code>never</code>, <code>solved</code> (to show but disable the corresponding button until the hacking challenge is solved) or <code>always</code>.</p></td>
<td class="tableblock halign-left valign-top"><p><code>solved</code></p></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>restrictToTutorialsFirst</code></p></td>
<td class="tableblock halign-left valign-top"><p>Disables all <em>Score Board</em> filter options and hides those of the 116 challenges without a tutorial until all challenges with a tutorial have been solved.</p></td>
<td class="tableblock halign-left valign-top"><p><code>false</code></p></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>overwriteUrlForProductTamperingChallenge</code></p></td>
<td class="tableblock halign-left valign-top"><p>URL that should replace the original URL defined in <code>urlForProductTamperingChallenge</code> for the [[08 Broken Access Control|Product Tampering]] challenge.</p></td>
<td class="tableblock halign-left valign-top"><p><code>https://owasp.slack.com</code></p></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>overwriteUrlForCsrfChallenge</code></p></td>
<td class="tableblock halign-left valign-top"><p>URL that should replace the original URL defined in <code>overwriteUrlForCsrfChallenge</code> for the [[08 Broken Access Control|CSRF]] challenge.</p></td>
<td class="tableblock halign-left valign-top"><p><code>http://htmledit.squarefree.com</code></p></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>xssBonusPayload</code></p></td>
<td class="tableblock halign-left valign-top"><p>This XSS payload is expected during the [[10 Cross Site Scripting (XSS)|Bonus Payload]] challenge.</p></td>
<td class="tableblock halign-left valign-top"><p><code>'&lt;iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&amp;color=%23ff5500&amp;auto_play=true&amp;hide_related=false&amp;show_comments=true&amp;show_user=true&amp;show_reposts=false&amp;show_teaser=true"&gt;&lt;/iframe&gt;'</code></p></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>safetyMode</code></p></td>
<td class="tableblock halign-left valign-top"><p>If set to <code>disabled</code>, it enables all [[04 Challenge tracking|potentially dangerous challenges]] regardless of any harm they might cause when running in a containerized environment. If set to <code>auto</code>, safety mode is enabled if Juice Shop considers the envrironment its running in to be a production environent (which are more likely to be exposed to internet traffic). When set to <code>enabled</code>, safety mode is enabled in any environment. ☠️ <strong>Change at your own risk!</strong></p></td>
<td class="tableblock halign-left valign-top"><p><code>auto</code></p>
<p></p></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>metricsIgnoredUserAgents</code></p></td>
<td class="tableblock halign-left valign-top"><p>List of user agent substrings. Requests to the <code>/metrics</code> endpoint containing any of these substrings in their User-Agent header will not trigger the [[05 Sensitive Data Exposure|Exposed Metrics]] challenge. This allows integration with monitoring tools without automatically solving the challenge.</p></td>
<td class="tableblock halign-left valign-top"><pre class="highlightjs highlight"><code>- &#39;Prometheus&#39;
- &#39;Alloy&#39;
- &#39;promscrape&#39;
- &#39;otelcol&#39;</code></pre></td>
</tr>
</tbody>
</table>

### `hackingInstructor` section

Allows enabling and customizing the [[04 Challenge tracking|Hacking Instructor]] tutorial mode.

| Property | Description | Default |
|---|---|---|
| `isEnabled` | Shows or hides the [[04 Challenge tracking\|Hacking Instructor]] links from the *Score Board* and *Welcome Banner*. | `true` |
| `avatarImage` | Filename in `frontend/dist/frontend/assets/public/images` *or* a URL[^1] of an image which will first be download to that folder and then used as an avatar in the tutorial speech bubbles. | `juicyBot.png` |

### `products` sequence

List of product mappings which, when specified, replaces **the entire list** of default products.

<table class="tableblock frame-all grid-all stretch">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th class="tableblock halign-left valign-top">Property</th>
<th class="tableblock halign-left valign-top">Description</th>
<th class="tableblock halign-left valign-top">Conditions</th>
<th class="tableblock halign-left valign-top">Default</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>name</code></p></td>
<td class="tableblock halign-left valign-top"><p>Name of the product.</p></td>
<td class="tableblock halign-left valign-top"><p>mandatory</p></td>
<td class="tableblock halign-left valign-top"></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>description</code></p></td>
<td class="tableblock halign-left valign-top"><p>Description of the product.</p></td>
<td class="tableblock halign-left valign-top"><p>optional</p></td>
<td class="tableblock halign-left valign-top"><p><code>'Lorem ipsum dolor sit amet, consectetuer adipiscing elit.'</code></p></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>price</code></p></td>
<td class="tableblock halign-left valign-top"><p>Price of the product.</p></td>
<td class="tableblock halign-left valign-top"><p>optional</p></td>
<td class="tableblock halign-left valign-top"><p>A random price of 1-10</p></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>deluxePrice</code></p></td>
<td class="tableblock halign-left valign-top"><p>Price of the product for <em>Deluxe Membership</em> customers.</p></td>
<td class="tableblock halign-left valign-top"><p>optional</p></td>
<td class="tableblock halign-left valign-top"><p>Same as the regular price</p></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>quantity</code></p></td>
<td class="tableblock halign-left valign-top"><p>Available quantity of product in stock.</p></td>
<td class="tableblock halign-left valign-top"><p>optional</p></td>
<td class="tableblock halign-left valign-top"><p>Random quantity of 30-100 items</p></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>limitPerUser</code></p></td>
<td class="tableblock halign-left valign-top"><p>Maximum purchase limit for regular customers. Does not apply to <em>Deluxe Membership</em> holders.</p></td>
<td class="tableblock halign-left valign-top"><p>optional</p></td>
<td class="tableblock halign-left valign-top"><p><code>null</code></p></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>image</code></p></td>
<td class="tableblock halign-left valign-top"><p>Filename in <code>frontend/dist/frontend/assets/public/images/products</code> <em>or</em> URL[^1] of an image to download to that folder and then use as a product image.</p></td>
<td class="tableblock halign-left valign-top"><p>optional</p></td>
<td class="tableblock halign-left valign-top"><p><code>undefined.png</code></p></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>deletedDate</code></p></td>
<td class="tableblock halign-left valign-top"><p>Deletion date of the product in <code>YYYY-MM-DD</code> format.</p></td>
<td class="tableblock halign-left valign-top"><p>optional</p></td>
<td class="tableblock halign-left valign-top"><p><code>null</code></p></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>urlForProductTamperingChallenge</code></p></td>
<td class="tableblock halign-left valign-top"><p>Sets the original link of the product which is the target for the [[08 Broken Access Control|Product Tampering]] challenge. Overrides <code>deletedDate</code> with <code>null</code>.</p></td>
<td class="tableblock halign-left valign-top"><p>must be defined on exactly one product</p></td>
<td class="tableblock halign-left valign-top"></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>useForChristmasSpecialChallenge</code></p></td>
<td class="tableblock halign-left valign-top"><p>Marks a product as the target for [[03 Injection|the "christmas special" challenge]]. Overrides <code>deletedDate</code> with <code>2014-12-27</code>.</p></td>
<td class="tableblock halign-left valign-top"><p>must be <code>true</code> on exactly one product</p></td>
<td class="tableblock halign-left valign-top"></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>fileForRetrieveBlueprintChallenge</code></p></td>
<td class="tableblock halign-left valign-top"><p>Filename in <code>frontend/dist/frontend/assets/public/images/products</code> <em>or</em> URL[^1] of a file download to that folder and then use as the target for the [[05 Sensitive Data Exposure|Retrieve Blueprint]] challenge. If a filename is specified but the file does not exist in <code>frontend/dist/frontend/assets/public/images/products</code> the challenge is still solvable by just requesting it from the server.<br />
<strong>Tip:</strong> To make this challenge realistically solvable, include some kind of hint to the blueprint file’s name/type in the product image’s <code>Exif</code> metadata.</p></td>
<td class="tableblock halign-left valign-top"><p>must be defined on exactly one product</p></td>
<td class="tableblock halign-left valign-top"><p><code>JuiceShop.stl</code> on <em>OWASP Juice Shop Logo (3D-printed)</em></p></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p><code>exifForBlueprintChallenge</code></p></td>
<td class="tableblock halign-left valign-top"><p>List of keywords that are supposed to appear as EXIF properties on the image of the [[05 Sensitive Data Exposure|Retrieve Blueprint]] challenge product.<br />
<strong>Note:</strong> Will be used in automatic testing for presence of the EXIF metadata but does not have any effect at runtime.</p></td>
<td class="tableblock halign-left valign-top"><p>must be defined on the product with <code>fileForRetrieveBlueprintChallenge</code></p></td>
<td class="tableblock halign-left valign-top"><p><code>- OpenSCAD</code> on <em>OWASP Juice Shop Logo (3D-printed)</em></p></td>
</tr>
<tr class="odd">
<td class="tableblock halign-left valign-top"><p><code>keywordsForPastebinDataLeakChallenge</code></p></td>
<td class="tableblock halign-left valign-top"><p>List of keywords which are all mandatory to mention in a feedback or complaint to solve the [[05 Sensitive Data Exposure|Leaked Unsafe Product]] challenge. Overrides <code>deletedDate</code> with <code>2019-02-1</code>.<br />
<strong>Tip:</strong> To make this challenge realistically solvable, provide the keywords on e.g. PasteBin in an obscured way that works well with the "dangerous ingredients of an unsafe product" narrative.</p></td>
<td class="tableblock halign-left valign-top"><p>must be defined on exactly one product</p></td>
<td class="tableblock halign-left valign-top"></td>
</tr>
<tr class="even">
<td class="tableblock halign-left valign-top"><p>reviews sub-sequence</p></td>
<td class="tableblock halign-left valign-top"><p>Sub-list which adds reviews to a product.</p></td>
<td class="tableblock halign-left valign-top"><p>optional</p></td>
<td class="tableblock halign-left valign-top"><p><code>~</code></p></td>
</tr>
</tbody>
</table>

#### `reviews` sub-sequence

Sub-list which adds reviews to a product.

| Property | Description | Conditions |
|---|---|---|
| `text` | Text of the review. | mandatory |
| `author` | Reference by `key` from `data/static/users.yml` to the author of the review | mandatory |

### `memories` sequence

List which, when specified, replaces all default *Photo Wall* entries except a hard-coded one needed to solve the [[07 Improper Input Validation|Retrieve the photo of Bjoern’s cat in "melee combat-mode"]] challenge.

| Property | Description | Conditions |
|---|---|---|
| `image` | Filename in `frontend/dist/frontend/assets/public/images/uploads/` *or* URL[^1] of an image to download to that folder and then use as a *Photo Wall* image. | mandatory |
| `caption` | Text to show when hovering over the image or sending a Tweet about it. | mandatory |
| `user` | Reference by `key` from `data/static/users.yml` to the owner of the photo upload. | mandatory *(exceptions see below)* |
| `geoStalkingMetaSecurityQuestion` | ID of the security question associated with the [[05 Sensitive Data Exposure\|Meta Geo Stalking]] challenge. | must be defined on exactly one memory together with `geoStalkingMetaSecurityAnswer` |
| `geoStalkingMetaSecurityAnswer` | Answer to the security question associated with the [[05 Sensitive Data Exposure\|Meta Geo Stalking]] challenge. Should be retrievable via the meta data of the assosicated image. | must be defined on exactly one memory together with `geoStalkingMetaSecurityQuestion` |
| `geoStalkingVisualSecurityQuestion` | ID of the security question associated with the [[05 Sensitive Data Exposure\|Visual Geo Stalking]] challenge. | must be defined on exactly one memory together with `geoStalkingVisualSecurityAnswer` |
| `geoStalkingVisualSecurityAnswer` | Answer to the security question associated with the [[05 Sensitive Data Exposure\|Visual Geo Stalking]] challenge. Should be retrievable via some (not too obvious) visual clue in the assosicated image. | must be defined on exactly one memory together with `geoStalkingVisualSecurityQuestion` |

### `ctf` section

Section to enable and configure the Capture-the-Flag mode built into OWASP Juice Shop.

| Property | Description | Default |
|---|---|---|
| `showFlagsInNotifications` | Shows or hides the CTF flag codes in the *"challenge solved"*-notifications. Is ignored when `application.showChallengeSolvedNotifications` is set to `false`. | `false` |
| `showCountryDetailsInNotifications` | Determines if the country (from `countryMapping`) mapped to the solved challenge is displayed in the notification. Can be `none`, `name`, `flag` or `both`. Only useful for CTFs using [[04 Hosting a CTF event\|FBCTF]]. | `none` |
| [`countryMapping` sub-mapping](#_countrymapping_sub_mapping) | List of mappings which associates challenges to countries on the challenge map of [[04 Hosting a CTF event\|FBCTF]]. Only needed for CTFs using [[04 Hosting a CTF event\|FBCTF]]. | `~` |
| [`systemWideNotifications` subsection](#_systemwidenotifications_subsection) | Configures an optional system-wide broadcast notification banner for CTF participants. When set up, the frontend polls an external URL and displays the current notification as a dismissable card. | `~` |

#### `countryMapping` sub-mapping

List of mappings which associates challenges to countries on the challenge map of [[04 Hosting a CTF event|FBCTF]]. Only needed for CTFs using [[04 Hosting a CTF event|FBCTF]]:

- Challenge `key` from `data/static/challenges.yml`

  - `name` the name of the country

  - `code` the two-letter ISO code of the country

|  |  |
|---|---|
|  | When specifying `countryMapping`, it is mandatory to map **all challenges** in order to produce a valid configuration file. It is recommended to use `config/fbctf.yml` as a template for that purpose. |

#### `systemWideNotifications` subsection

This optional subsection enables system-wide broadcast notifications for CTF participants. When configured, the Juice Shop frontend polls an external URL at a regular interval and displays the current notification as a dismissable card — similar in style to the *"Challenge Solved"* notification.

The notification endpoint is **not** served by Juice Shop itself and must be provided externally by the CTF organizer (e.g. a simple static JSON file served via a web server, or a small API endpoint).

|  |  |
|---|---|
|  | [MultiJuicer](https://github.com/juice-shop/MultiJuicer) has built-in support for serving system-wide notifications, available starting with [MultiJuicer v9.2.0](https://github.com/juice-shop/multi-juicer/releases/tag/v9.2.0). |

| Property | Default | Description |
|---|---|---|
| `url` | `~` | URL of an external endpoint returning a JSON response of the form `{ "message": "…", "enabled": true, "updatedAt": "<ISO 8601 timestamp>" }`. When not set, the feature is disabled entirely and the frontend renders nothing. |
| `pollFrequencySeconds` | `~` | Polling interval in seconds. Required (and must be a positive number) when `url` is set. Juice Shop will fail validation on startup if `url` is set but this value is missing, zero, or negative. |

The external endpoint must return a JSON object with the following fields:

| Field | Type | Description |
|---|---|---|
| `message` | String | The notification text to display to all participants. |
| `enabled` | Boolean | When `false`, the notification is hidden regardless of the message content. |
| `updatedAt` | String | ISO 8601 timestamp used to determine whether a notification has changed since the user last dismissed it. If the timestamp changes, the dismissed card will reappear. |

Example response from the notification endpoint:

``` highlightjs
{
  "message": "Hello CTF participants! Flag submission closes at 18:00 UTC.",
  "enabled": true,
  "updatedAt": "2026-02-14T12:44:38.423Z"
}
```

The notification card is shown when `enabled` is `true` and `message` is a non-empty string. Once dismissed by the user, it will not reappear unless the `updatedAt` timestamp changes. If the poll request fails (network error, non-2xx response), the card is hidden silently.

## Configuration example

``` highlightjs
server:
  port: 3000
application:
  domain: juice-sh.op
  name: 'OWASP Juice Shop'
  logo: JuiceShop_Logo.png
  favicon: favicon_js.ico
  theme: bluegrey-lightgreen
  showVersionNumber: true
  showGitHubLinks: true
  localBackupEnabled: true
  numberOfRandomFakeUsers: 0
  altcoinName: Juicycoin
  privacyContactEmail: donotreply@owasp-juice.shop
  customMetricsPrefix: juiceshop
  chatBot:
    name: 'Juicy'
    avatar: 'JuicyChatBot.png'
    model: 'gemma4:e4b'
    llmApiUrl: 'http://localhost:11434/v1'
    llmMaxRetries: 2
    sampleQuestions:
      - 'CHATBOT_PROMPT_RECOMMENDATION_SUMMER_PARTY'
      - 'CHATBOT_PROMPT_RECOMMENDATION_POPULAR'
      - 'CHATBOT_PROMPT_RECOMMENDATION_SUGAR_FREE'
      - 'CHATBOT_PROMPT_RECOMMENDATION_START_DAY'
      - 'CHATBOT_PROMPT_RECOMMENDATION_SEASONAL'
  social:
    blueSkyUrl: 'https://bsky.app/profile/owasp-juice.shop'
    mastodonUrl: 'https://fosstodon.org/@owasp_juiceshop'
    twitterUrl: 'https://twitter.com/owasp_juiceshop'
    facebookUrl: 'https://www.facebook.com/owasp.juiceshop'
    slackUrl: 'https://owasp.org/slack/invite'
    redditUrl: 'https://www.reddit.com/r/owasp_juiceshop'
    pressKitUrl: 'https://github.com/OWASP/owasp-swag/tree/master/projects/juice-shop'
    questionnaireUrl: ~
  recyclePage:
    topProductImage: fruit_press.jpg
    bottomProductImage: apple_pressings.jpg
  welcomeBanner:
    showOnFirstStart: true
    title: 'Welcome to OWASP Juice Shop!'
    message: "<p>Being a web application with a vast number of intended security vulnerabilities, the <strong>OWASP Juice Shop</strong> is supposed to be the opposite of a best practice or template application for web developers: It is an awareness, training, demonstration and exercise tool for security risks in modern web applications. The <strong>OWASP Juice Shop</strong> is an open-source project hosted by the non-profit <a href='https://owasp.org' target='_blank'>Open Worldwide Application Security Project (OWASP)</a> and is developed and maintained by volunteers. Check out the link below for more information and documentation on the project.</p><h1><a href='https://owasp-juice.shop' target='_blank'>https://owasp-juice.shop</a></h1>"
  cookieConsent:
    backgroundColor: '#546e7a'
    textColor: '#ffffff'
    buttonColor: '#558b2f'
    buttonTextColor: '#ffffff'
    message: 'This website uses fruit cookies to ensure you get the juiciest tracking experience.'
    dismissText: 'Me want it!'
    linkText: 'But me wait!'
    linkUrl: 'https://www.youtube.com/watch?v=9PnbKL3wuH4'
  securityTxt:
    contact: 'mailto:donotreply@owasp-juice.shop'
    encryption: 'https://keybase.io/bkimminich/pgp_keys.asc?fingerprint=19c01cb7157e4645e9e2c863062a85a8cbfbdcda'
    acknowledgements: '/#/score-board'
  promotion:
    video: JuiceShopJingle.mp4
    subtitles: JuiceShopJingle.vtt
  easterEggPlanet:
    name: Orangeuze
    overlayMap: orangemap2k.jpg
  googleOauth:
    clientId: '1005568560502-6hm16lef8oh46hr2d98vf2ohlnj4nfhq.apps.googleusercontent.com'
    authorizedRedirects:
      - { uri: 'https://demo.owasp-juice.shop' }
      - { uri: 'https://juice-shop.herokuapp.com' }
      - { uri: 'https://preview.owasp-juice.shop' }
      - { uri: 'https://juice-shop-staging.herokuapp.com' }
      - { uri: 'https://juice-shop.wtf' }
      - { uri: 'http://localhost:3000', proxy: 'https://local3000.owasp-juice.shop' }
      - { uri: 'http://127.0.0.1:3000', proxy: 'https://local3000.owasp-juice.shop' }
      - { uri: 'http://localhost:4200', proxy: 'https://local4200.owasp-juice.shop' }
      - { uri: 'http://127.0.0.1:4200', proxy: 'https://local4200.owasp-juice.shop' }
      - { uri: 'http://192.168.99.100:3000', proxy: 'https://localmac.owasp-juice.shop' }
      - { uri: 'http://192.168.99.100:4200', proxy: 'https://localmac.owasp-juice.shop' }
      - { uri: 'http://penguin.termina.linux.test:3000', proxy: 'https://localchromeos.owasp-juice.shop' }
      - { uri: 'http://penguin.termina.linux.test:4200', proxy: 'https://localchromeos.owasp-juice.shop' }
challenges:
  showSolvedNotifications: true
  showHints: true
  showMitigations: true
  codingChallengesEnabled: solved # Options: never  solved  always
  restrictToTutorialsFirst: false
  safetyMode: auto # Options: enabled disabled auto
  overwriteUrlForProductTamperingChallenge: 'https://owasp.slack.com'
  overwriteUrlForCsrfChallenge: 'http://htmledit.squarefree.com'
  xssBonusPayload: '<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>'
  metricsIgnoredUserAgents:
    - 'Prometheus'
    - 'Alloy'
    - 'promscrape'
    - 'otelcol'
hackingInstructor:
  isEnabled: true
  avatarImage: juicyBot.png
products:
  -
    name: 'Apple Juice (1000ml)'
    price: 1.99
    description: 'The all-time classic.'
    image: apple_juice.jpg
    reviews:
      - { text: 'One of my favorites!', author: admin }
# ~~~~~ ... ~~~~~~
  -
    name: 'OWASP SSL Advanced Forensic Tool (O-Saft)'
    description: 'O-Saft is an easy to use tool to show information about SSL certificate and tests the SSL connection according given list of ciphers and various SSL configurations.'
    price: 0.01
    image: orange_juice.jpg
    urlForProductTamperingChallenge: 'https://www.owasp.org/index.php/O-Saft'
  -
    name: 'Christmas Super-Surprise-Box (2014 Edition)'
    description: 'Contains a random selection of 10 bottles (each 500ml) of our tastiest juices and an extra fan shirt for an unbeatable price!'
    price: 29.99
    image: undefined.jpg
    useForChristmasSpecialChallenge: true
  -
    name: 'OWASP Juice Shop Sticker (2015/2016 design)'
    description: 'Die-cut sticker with the official 2015/2016 logo. By now this is a rare collectors item. <em>Out of stock!</em>'
    price: 999.99
    image: sticker.png
    deletedDate: '2017-04-28'
# ~~~~~ ... ~~~~~~
  -
    name: 'OWASP Juice Shop Logo (3D-printed)'
    description: 'This rare item was designed and handcrafted in Sweden. This is why it is so incredibly expensive despite its complete lack of purpose.'
    price: 99.99
    image: 3d_keychain.jpg
    fileForRetrieveBlueprintChallenge: JuiceShop.stl
    exifForBlueprintChallenge:
      - OpenSCAD
# ~~~~~ ... ~~~~~~
memories:
  -
    image: 'magn(et)ificent!-1571814229653.jpg'
    caption: 'Magn(et)ificent!'
    user: bjoernGoogle
  -
    image: 'my-rare-collectors-item!-[̲̅$̲̅(̲̅-͡°-͜ʖ-͡°̲̅)̲̅$̲̅]-1572603645543.jpg'
    caption: 'My rare collectors item! [̲̅$̲̅(̲̅ ͡° ͜ʖ ͡°̲̅)̲̅$̲̅]'
    user: bjoernGoogle
ctf:
  showFlagsInNotifications: false
  showCountryDetailsInNotifications: none
  countryMapping: ~
  systemWideNotifications:
    url: https://example.com/api/notifications
    pollFrequencySeconds: 60
```

### Overriding default settings

When creating your own YAML configuration file, you can rely on the existing default values and only overwrite what you want to change. The provided `config/ctf.yml` file for capture-the-flag events for example is as short as this:

``` highlightjs
application:
  logo: JuiceShopCTF_Logo.png
  favicon: favicon_ctf.ico
  showVersionNumber: false
  showGitHubLinks: false
  welcomeBanner:
    showOnFirstStart: false
challenges:
  showHints: false
  safetyMode: disabled
hackingInstructor:
  isEnabled: false
ctf:
  showFlagsInNotifications: true
```

### Testing customizations

You can validate your custom configuration file against the schema by running `npm run lint:config -- -f /path/to/myConfig.yml`. This validation automatically happens on server startup as well.

To verify if your custom configuration will not break any of the challenges, you should run the end-to-end tests via `npm start & npm run cypress:open &`. If they pass, all challenges will be working fine!

## Material Color Themes

The `application.theme` property allows certain pre-defined color schemes. The table below shows sample screenshots for each of these.

| Theme | Sample screenshot |
|---|---|
| `bluegrey-lightgreen` | ![[part4-customization-material-theme_bluegrey-lightgreen.png]] |
| `blue-lightblue` | ![[part4-customization-material-theme_blue-lightblue.png]] |
| `deeppurple-amber` | ![[part4-customization-material-theme_deeppurple-amber.png]] |
| `indigo-pink` | ![[part4-customization-material-theme_indigo-pink.png]] |
| `pink-bluegrey` | ![[part4-customization-material-theme_pink-bluegrey.png]] |
| `purple-green` | ![[part4-customization-material-theme_purple-green.png]] |
| `deeporange-indigo` | ![[part4-customization-material-theme_deeporange-indigo.png]] |
| `lime-green` | ![[part4-customization-material-theme_lime-green.png]] |
| `neon-fire` | ![[part4-customization-material-theme_neon-fire.png]] |

## Provided customizations

Several fully re-themed customizations are provided out of the box by OWASP Juice Shop for demonstration purposes. We also offer an [18-step customzation tutorial](https://github.com/juice-shop/juice-shop-tutorials/blob/master/customization/README.md) to create your own. Some out-of-the-box examples provided with OWASP Juice Shop:

![[part4-customization-theme_mozilla.png]]

![[part4-customization-theme_oss.png]]

Furthermore, the following convenience customizations are provided out-of-the-box to simplify usage of OWASP Juice Shop in specific use cases and situations:

- [CTF-mode](https://github.com/juice-shop/juice-shop/blob/master/config/ctf.yml): Keeps the Juice Shop in its default layout but disabled hints while enabling CTF flag codes in the *"challenge solved"*-notifications. Refer to [[04 Hosting a CTF event|Hosting a CTF event]] to learn more about running a CTF-event with OWASP Juice Shop. 🚩

- [Quiet mode](https://github.com/juice-shop/juice-shop/blob/master/config/quiet.yml): Keeps the Juice Shop in its default layout but hides all *"challenge solved"*-notifications, GitHub ribbon and challenge hints. 🔇

- [Tutorial mode](https://github.com/juice-shop/juice-shop/blob/master/config/tutorial.yml): Restricts the user to first solve all challenges with [[04 Challenge tracking|Hacking Instructor]] tutorials before the entire *Score Board* gets unlocked and filterable. 🏫 Hidden challenges can still be solved and users will receive corresponding success notifications!

- [Unsafe mode](https://github.com/juice-shop/juice-shop/blob/master/config/unsafe.yml): Keeps everything at default settings except *enabling* all [[04 Challenge tracking|potentially dangerous challenges]] even in containerized environments. ☠️ **Use at your own risk!**

## Proprietary customization

Below you find screenshots of a custom theme the author of this book created for awareness and demo sessions at work. It was presented to various groups and individuals, from Project Manager rounds over workers council members up to the company CEO at that time.

![[part4-customization-theme_kn1.png]]

![[part4-customization-theme_kn2.png]]

![[part4-customization-theme_kn3.png]]

![[part4-customization-theme_kn4.png]]

![[part4-customization-theme_kn5.png]]

![[part4-customization-theme_kn6.png]]

## Limitations

- When running a customization (except `default.yml`) that overwrites the property `application.domain`, the description of the challenges *Ephemeral Accountant*, *Forged Signed JWT* and *Unsigned JWT* will always be shown in English.

- Configurations (except `default.yml`) do not support translation of custom product names and descriptions as of v20.3.0.

- Several [[04 Challenge tracking|Hacking Instructor]] scripts depend on product inventory and product reviews that might not exist in the required form when you overwrote the default `products` list. Consider turning off the tutorials by setting `hackingInstructor.isEnabled` to `false` in that case.

## Additional Browser tweaks

Consider you are doing a live demo with a highly customized corporate theme. Your narrative is, that this *really* is an upcoming eCommerce application *of that company*. [[06 Walking the happy path|Walking the "happy path"]] might now lure you into two situations which could spoil the immersion for the audience.

#### Coupon codes on social media

If you configured the `twitterUrl`/`facebookUrl` as the company’s own account/page, you will most likely not find any coupon codes posted there. You will probably fail to convince the social media team to tweet or retweet some coupon code for an application that does not even exist!

![[part4-customization-coupon_immersion-spoiler.png]]

#### OAuth Login

Another immersion spoiler occurs when demonstrating the *Log in with Google* functionality, which will show you the application name registered on Google Cloud Platform: *OWASP Juice Shop*! There is no way to convince Google to show anything else for obvious trust and integrity reasons.

![[part4-customization-oauth_immersion-spoiler.png]]

|  |  |
|---|---|
|  | Since v10.0.0 you can overwrite the [`googleOauth` subsection](#_googleoauth_subsection) to use your own application on Google Cloud Platform for handling OAuth. This is a relatively high effort, so maybe you want to kill two birds with one stone instead as described in the next section. |

### On-the-fly text replacement

You can solve both of the above problems *in your own Browser* by replacing text on the fly when the Twitter, Facebook or Google-Login page is loaded. For Chrome [Word Replacer II](https://chrome.google.com/webstore/detail/word-replacer-ii/djakfbefalbkkdgnhkkdiihelkjdpbfh?hl=en) is a plugin that does this work for you with very little setup effort. For Firefox [FoxReplace](https://addons.mozilla.org/en-US/firefox/addon/foxreplace/) does a similar job. After installing either plugin you have to create two text replacements:

1. Create a replacement for `OWASP Juice Shop` (as it appears on Google-Login) with your own application name. Best use `application.name` from your configuration.

2. Create another replacement for a complete or partial Tweet or Facebook post with some marketing text and an actual coupon code. You can get valid coupon codes from the OWASP Juice Shop Twitter feed: [https://twitter.com/owasp_juiceshop](https://twitter.com/owasp_juiceshop).

    ![[part4-customization-word_replacer_ii.png]]

3. Enable the plugin and verify your replacements work:

![[part4-customization-coupon_immersion-replacement.png]]

![[part4-customization-oauth_immersion-replacement.png]]

------------------------------------------------------------------------

[^1]: Only pure server-side URLs pointing directly to the raw file resource are supported. URLs relying on e.g. client-side processing such as fragment identifiers or those requiring interactive session authentication and cookies will cause download failures when the server attempts to fetch the asset.

[[01 Troubleshooting|Troubleshooting]] [[03 Hosting platform for multiple users|Hosting platform for multiple users]]

