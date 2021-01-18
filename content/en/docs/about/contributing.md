---
title: Contribution guidelines
linkTitle: Contribution guidelines
weight: 60
description: >
    How to contribute to Butler Auth.
---

{{% pageinfo %}}
Butler Auth is an open source project, using the [MIT license](https://choosealicense.com/licenses/mit/).

This means that all source code, documentation etc is available as-is, at no cost.

**It however also means that anyone interested can - and is encouraged to - contribute to the project!**

{{% /pageinfo %}}

Butler Auth is developed in [Node.js](https://nodejs.org), with support from various [NPM](https://www.npmjs.com/) modules.  
Specifically, most authentication providers are implemented using [Passport.js](http://www.passportjs.org/).

We use [Hugo](https://gohugo.io/) to format and generate this documentation site and the [Docsy](https://github.com/google/docsy) theme for styling and site structure.  
Hugo is an open-source static site generator that provides us with templates, content organisation in a standard directory structure, and a website generation engine. You write the pages in Markdown (or HTML if you want), and Hugo wraps them up into a website.

All submissions, including submissions by project members, require review.  
We use GitHub pull requests for this purpose. Consult [GitHub Help](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/about-pull-requests) for more information on using pull requests.

## Discussions

We use [GitHub's discussion boards](https://github.com/ptarmiganlabs/butler-auth/discussions/1) to discuss ideas, features, issues and everything else relating to Butler Auth.

## Creating an issue

If you've found a problem - or have a feature suggestion - with Butler Auth itself or the documentation, but you're not sure how to fix it yourself, please create an issue in the [Butler Auth repo](https://github.com/ptarmiganlabs/butler-auth/issues/new). You can also create an issue about a specific doc page by clicking the **Create Issue** button in the top right hand corner of the page.

{{% alert title="Security/Disclosure" color="warning" %}}
If you discover a serious bug with Butler Auth that may pose a security problem, please disclose it confidentially to security@ptarmiganlabs.com first, so that it can be assessed and hopefully fixed prior to being exploited. Please do not raise GitHub issues for security-related doubts or problems.
{{% /alert %}}
