---
title: "Passport.js"
linkTitle: "Passport.js"
weight: 20
description: >
  Butler Auth uses Passport.js for most authentication providers.
---

## Passport.js

[Passport.js](http://www.passportjs.org/) is an open source library focusing on authentication.

From their own site:

{{% pageinfo color="primary" %}}
Passport is authentication middleware for Node.js. Extremely flexible and modular, Passport can be unobtrusively dropped in to any Express-based web application. A comprehensive set of strategies support authentication using a username and password, Facebook, Twitter, and more.
{{% /pageinfo %}}

Butler Auth uses Passport.js for many (but not all) authentication strategies.

Given that Passport.js uses a consistent interface for all authentication strategies it's usually quite easy to add additional authentication providers to Butler Auth.
