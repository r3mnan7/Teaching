---
title: "Contribute to development"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part3/contribution.html
tags: [juiceshop, owasp, companion-guide]
---

# Contribute to development

Part of [[01 Getting involved|Getting involved]]
← Previous: [[03 Donations|Donations]]
Next: [[05 Codebase 101|Codebase 101]] →

If you would like to contribute to OWASP Juice Shop but need some idea what task to address, the best place to look is in the GitHub issue lists at [https://github.com/juice-shop/juice-shop/issues](https://github.com/juice-shop/juice-shop/issues).

<img src="../resources/part3-contribution-help_wanted-label.png" alt="help wanted" /> <img src="../resources/part3-contribution-good_first_issue-label.png" alt="good first issue" />

- Issues labelled with **help wanted** indicate tasks where the project team would very much appreciate help from the community

- Issues labelled with **good first issue** indicate tasks that are isolated and not too hard to implement, so they are well-suited for new contributors

The following sections describe in detail the most important rules and processes when contributing to the OWASP Juice Shop project.

## Tips for newcomers

If you are new to application development - particularly with Angular and Express.js - it is recommended to read the [[05 Codebase 101|Codebase 101]] to get an overview what belongs where. It will lower the entry barrier for you significantly.

## Version control

The project uses `git` as its version control system and GitHub as the central server and collaboration platform. OWASP Juice Shop resides in the following repository:

[https://github.com/juice-shop/juice-shop](https://github.com/juice-shop/juice-shop)

### Forking & cloning

You can clone the original repository with `git clone https://github.com/juice-shop/juice-shop.git` but in order to contribute to the project via [Pull Requests](#_pull_requests) you probably want to [fork the repository instead](https://github.com/juice-shop/juice-shop/fork) and then clone that fork to work on with

``` text
git clone https://github.com/<your GitHub username>/juice-shop.git
```

### Branching model

OWASP Juice Shop is maintained in a simplified [Gitflow](http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/) fashion, where all active development happens on the `develop` branch while `master` is used to deploy stable versions to the [Heroku demo instance](https://juice-shop.herokuapp.com) and later create tagged releases from.

Feature branches are only used for long-term tasks that could jeopardize regular releases from `develop` in the meantime. Likewise prototypes and experiments must be developed on an individual branch or a distinct fork of the entire project.

### Versioning

Any release from `master` is tagged with a unique version in the format `vMAJOR.MINOR.PATCH`, for example `v1.3.0` or `v4.1.2`.

> Given a version number `MAJOR.MINOR.PATCH`, increment the:
>
> 1. `MAJOR` version when you make incompatible API changes,
>
> 2. `MINOR` version when you add functionality in a backwards-compatible manner, and
>
> 3. `PATCH` version when you make backwards-compatible bug fixes.[^1]

The current version of the project (omitting the leading `v`) must be manually maintained in the following three places:

- `/package.json` in the `"version"` property

- `/frontend/package.json` in the `"version"` property

- `/Dockerfile` in the `LABEL` named `org.opencontainers.image.version`

All other occurrences of the version (i.e. packaged releases & the menu bar of the application itself) are resolved through the `"version"` property of `/package.json` automatically.

### Pull requests

Using Git-Flow means that PRs have the highest chance of getting accepted and merged when you open them on the `develop` branch of your fork. That allows for some post-merge changes by the team without directly compromising the `master` branch, which is supposed to hold always be in a release-ready state.

It is usually not a big deal if you accidentally open a PR for the `master` branch. GitHub added the possibility to change the target branch for a PR afterwards some time ago.

In case you want to open a PR before actually being finished with your work (e.g. because you’d like to see some intermediate CI/CD results) please tag the PR as *Draft* during its creation or later in its settings. This will natively prevent premature merges on GitHub from happening, while allowing your current work status to get reviewed already.

## Contribution guidelines

The ten minimum requirements for code contributions are:

1. All Pull Requests *must* be based off the `develop` branch.

2. The code *must* be compliant with the configured ESLint rules based on the [JS Standard Code Style](http://standardjs.com).

3. All PRs *should* have a dedicated scope (e.g. not mixing code refactorings with delivering a new feature) and reasonable size.

4. All new and changed code *should* have a corresponding unit and/or integration test.

5. New and changed challenges *must* have a corresponding e2e test.

6. Noise (e.g. unnecessary comments) generated by AI tools *must* be removed before opening a PR.

7. All [status checks](#%3Cem%3Econtinuous_integration_deployment) _must pass for the last commit within your PR.

8. All Git commits within a PR *must* be [signed off](https://git-scm.com/docs/git-commit#Documentation/git-commit.txt—s) to indicate the contributor’s agreement with the [Developer Certificate of Origin](https://developercertificate.org/).

9. [[06 Helping with translations|Contributing to translations]] *should* be done via [Crowdin](https://crowdin.com/project/owasp-juice-shop) and *not* via GitHub PRs.

10. Particularly low-effort contributions (e.g. incomplete typo fixes in a single file, trivial text changes, code formatting) or any forms of potential "contribution farming" *must not* be submitted as PRs. Doing so more than once *might* be [considered spamming and handled accordingly](#_handling_of_spam_prs).

### Linting

![[part3-contribution-badge.svg]]

``` highlightjs
npm run lint
```

The `npm run lint` script verifies code compliance with

- the `eslintrc.js` rules derived from `standard` for all server-side JavaScript code

- the `frontend/eslintrc.js` rules derived from `standard-with-typescript` for the frontend TypeScript code

- the `frontend/stylelintrc.js` rules derived from `stylelint-config-sass-guidelines` for the frontend SCSS stylesheets

If PRs deviate from this coding style, they will break the CI/CD pipeline and will not be merged until refactored to match the coding rules.

|  |  |
|---|---|
|  | In case your PR is failing from style guide issues try running `npm run lint:fix` over your code - this will fix many syntax issues automatically without breaking your code. |

### Testing

``` highlightjs
npm test                           # run all unit tests
npm run frisby                     # run all API integration tests
npm start & npm run cypress:run &  # run all end-to-end tests
```

Pull Requests are verified to pass all the following test stages during the [continuous integration build](https://github.com/juice-shop/juice-shop/actions). It is recommended that you run these tests on your local computer to verify they pass before submitting a PR. New features should be accompanied by an appropriate number of corresponding tests to verify they behave as intended.

#### Unit tests

There is a full suite containing isolated unit tests

- for all client-side code in `frontend/src/app/**/*.spec.ts`

- for the server-side routes and libraries in `test/server/*Spec.ts`

``` highlightjs
npm test
```

#### Integration tests

The integration tests in `test/api/*Spec.ts` verify if the backend for all normal use cases of the application works. All server-side vulnerabilities are also tested.

``` highlightjs
npm run frisby
```

These tests automatically start a server and run the tests against it. A working internet connection is recommended.

#### End-to-end tests

The e2e test suite in `cypress/integration/e2e/*Spec.ts` verifies if all client- and server-side vulnerabilities are exploitable. It passes only when all challenges are solvable on the score board.

``` highlightjs
npm start & npm run cypress:run &
```

The end-to-end tests require a locally installed Google Chrome browser and internet access to be able to pass.

If you have a web proxy configured via `HTTP_PROXY` environment variable, the end-to-end tests [will honor this setting](https://docs.cypress.io/guides/references/proxy-configuration). This can be useful to e.g. run the tests through tools like [ZAP](https://www.zaproxy.org/) or Burpsuite.

#### Refactoring Safety Net (RSN) for Coding Challenges

When changing existing code that is part of a Coding Challenge, you should run the Refactoring Safety Net (RSN) to detect accidental or forgotten changes in fix option files or the original code snippet. See [[03 Coding challenges|Refactoring Safety Net]] for details.

``` highlightjs
npm run rsn
```

#### Smoke tests

The shell script `test/smoke/smoke-test.sh` performs some *very basic* checks on the availability of expected UI content and API endpoints. During CI/CD it is used to verify if the packaged distribution and Docker image start properly.

To manually use it on a packaged distribution run the following in your local repository clone root folder:

``` highlightjs
npm install --production && grunt package
cd dist && tar -zxf juice-shop-*.tgz && cd juice-shop_*
npm start &
../../test/smoke/smoke-test.sh http://localhost:3000
```

### Manually testing packaged distributions

During releases the application will be packaged into `.zip`/`.tgz` archives for another easy setup method. When you contribute a change that impacts what the application needs to include, make sure you test this manually on your system.

``` highlightjs
npm install --production && grunt package
```

Then take the created archive from `/dist` and follow the steps described above in [Packaged Distributions](https://github.com/juice-shop/juice-shop#packaged-distributions--) to make sure nothing is broken or missing.

### Development mode for Angular frontend

Running `npm install` over and over for frontend code or view changes can be very time-consuming. Juice Shop can be run in a development mode provided through Angular CLI to avoid this. Run `npm run serve` from the root project folder and navigate to [http://localhost:4200](http://localhost:4200) instead of the usual port `3000`. Whenever you change code in the `frontend/src` folder, the UI will recompile the affected bit and auto-reload the browser window for you.

|  |  |
|---|---|
|  | Please note that the backend is still running on [http://localhost:3000](http://localhost:3000) in this mode and that changes in the backend code are not automatically applied. |

### Developing in a GitHub codespace

If you have access to [GitHub Codespaces](https://github.com/features/codespaces) (which is in closed beta at the time of writing this), you can run an almost complete development environment for OWASP Juice Shop in the Cloud. It allows you to program and run the application entirely from your browser. The author has tested this to work very well even on a weak Chromebook.

1. Go to [https://github.com/codespaces](https://github.com/codespaces).

2. Click *New codespace* and select `juice-shop/juice-shop` as *Repository* and `develop` as *Branch*. Then click *Create codespace*.

3. Your codespace will be set up and launched. It automatically installs some plugins to make contributing easier our of the box:

    - Angular Language Service

    - ESLint

    - npm

    - stylelint

4. After the container initializes, all application dependencies are automatically installed. This sometimes runs into some hang-up, so you might have to run `npm install` from the codespace terminal again if you see errors on `npm start` or ESLint complains about missing plugins.

5. That’s it! You’re ready for developing on OWASP Juice Shop!

|  |  |
|---|---|
|  | Please note that the client-side [Unit tests](#_unit_tests) and [End-to-end tests](#_end_to_end_tests) will not work on GitHub Codespaces due to the lack of a Chrome installation in the underlying container. |

### AI Tools Disclosure

All Pull Requests must disclose whether AI tools were used to generate or assist in the creation of code, documentation, or any other content within the contribution. The [Pull Request template](https://github.com/juice-shop/juice-shop/blob/master/.github/PULL_REQUEST_TEMPLATE.md) includes a dedicated section for this purpose.

|  |  |
|---|---|
|  | PRs lacking a truthful disclosure of used AI tools will be closed and might even be [handled as spam](#_handling_of_spam_prs). |

### Developer Certificate of Origin

> The Developer Certificate of Origin (DCO) is a lightweight way for contributors to certify that they wrote or otherwise have the right to submit the code they are contributing to the project. Here is the full [text of the DCO](https://developercertificate.org/), reformatted for readability:
>
> > By making a contribution to this project, I certify that:
> >
> > \(a\) The contribution was created in whole or in part by me and I have the right to submit it under the open source license indicated in the file; or
> >
> > \(b\) The contribution is based upon previous work that, to the best of my knowledge, is covered under an appropriate open source license and I have the right under that license to submit that work with modifications, whether created in whole or in part by me, under the same open source license (unless I am permitted to submit under a different license), as indicated in the file; or
> >
> > \(c\) The contribution was provided directly to me by some other person who certified (a), (b) or (c) and I have not modified it.
> >
> > \(d\) I understand and agree that this project and the contribution are public and that a record of the contribution (including all personal information I submit with it, including my sign-off) is maintained indefinitely and may be redistributed consistent with this project or the open source license(s) involved.
>
> Contributors sign-off that they adhere to these requirements by adding a Signed-off-by line to commit messages.
>
> ``` text
> This is my commit message
>
> Signed-off-by: Random J Developer <random@developer.example.org>
> ```
>
> Git even has a `-s` command line option to append this automatically to your commit message:
>
> ``` text
> $ git commit -s -m 'This is my commit message'
> ```
>
> [^2]

## Continuous integration & deployment

The CI/CD and release pipelines for OWASP Juice Shop are set up as GitHub Action workflows:

[https://github.com/juice-shop/juice-shop/actions](https://github.com/juice-shop/juice-shop/actions)

### CI/CD Pipeline

On every push to GitHub, a workflow consisting of several jobs is triggered on GitHub. Not only direct pushes to the `master` and `develop` branches are built, but Pull Requests from other branches or forks as well. This helps the project team to assess if a PR can be safely merged into the codebase. While unit and integration tests are executed on different combinations of Node.js and OS, the e2e tests are only run on the officially preferred Node.js version 24.x in order to avoid unnecessary feedback delays.

![[part3-contribution-ci-workflow.png]]

### Release Pipeline

For tag-builds (i.e. versions to be released) another workflow is triggered which packages the [[02 Running OWASP Juice Shop|release_artifacts for Linux, MacOS and Windows for each supported Node.js version]] and attach these to the release page on GitHub and also published Docker images for the released version.

![[part3-contribution-release-workflow.png]]

### Handling of spam PRs

On top of the occasional low-effort-contribution PR, a [significant number of Pull Requests](http://stats.owasp-juice.shop/#spamChart) to [https://github.com/juice-shop/juice-shop](https://github.com/juice-shop/juice-shop) are opened by GitHub users e.g. when "playing" with SCA / SAST tools or other automation tooling. Sometimes those users notice their mistake and close the PR right away, sometimes they don’t. Independent of who closed the PR (i.e. the original submitter or a Juice Shop Project Leader) it will be [marked with the **spam** label](https://github.com/juice-shop/juice-shop/pulls?q=is%3Apr+label%3Aspam).

#### Banning of spammers

The Project Leaders must be able to concentrate on the actual contributions to the project and not be kept busy by dealing with accidental or intentional spammers. Therefore, the following applies:

<table>
<tr>
<td></td>
<td>Users who open a <strong>spam</strong> PR will be put on an <strong>indefinite ban</strong> for interaction with the entire [https://github.com/juice-shop](https://github.com/juice-shop) organization.
There is no distinction between submitting e.g. accidental SAST-tool experiments, non-accidental low-effort PRs, contribution farming during events like Hacktoberfest or Google Summer of Code, or PRs with obviously malicious or abusive intent as detailed in the Juice Shop’s [Code of Conduct](https://github.com/juice-shop/juice-shop/blob/master/CODE_OF_CONDUCT). There is only one banlist for all of these cases, and the ban is applied immediately after a PR is labelled as <strong>spam</strong> without any further warning.</td>
</tr>
</table>

#### Redemption from banishment

In the unlikely case that a user ended on the banlist by (human or automation) mistake or without comprehensible cause, they may contact <bjoern.kimminich@owasp.org> and request to be removed from the banlist.

------------------------------------------------------------------------

[^1]: [http://semver.org](http://semver.org)

[^2]: [https://probot.github.io/apps/dco/](https://probot.github.io/apps/dco/)

[[03 Donations|Donations]] [[05 Codebase 101|Codebase 101]]

