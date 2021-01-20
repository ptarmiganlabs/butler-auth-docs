---
title: "The Butler family"
description: "Please meet the Butlers. They're a nice, wild bunch!" 
weight: 20
---

Butler Auth is part of a group of tools that all aim to improve, simplify or extend various aspects of Qlik products.  
Most tools focus on Qlik Sense Enterprise on Windows, but some have wider use within the Qlik ecosystem.

All members of the Butler family can be downloaded from Ptarmigan Labs' [GitHub page](https://github.com/ptarmiganlabs).  
Development of the Butler tools is sponsored by Ptarmigan Labs AB, which is an IT consultancy company in Stockholm, Sweden.

Projects with production grade release status are (as of this writing):

### Butler Auth

Makes it easier to add strong/flexible/custom authentication flows to Qlik Sense Enterprise on Windows.  
Out of the box Butler Auth supports Google, Microsoft, Facebook, Okta, Auth0, LDAP and a few more authentication providers.

Butler Auth is designed to be extensible. It's therefore relatively easy to add new authentication providers.

[butler-auth.ptarmiganlabs.com](https:/butler-auth.ptarmiganlabs.com) (This site!)

### Butler

The original Butler. Offers various utilities that make it easier to develop Sense apps, as well as simplifying day 2 operations.

[butler.ptarmiganlabs.com](https:/butler.ptarmiganlabs.com)

### Butler SOS

Real-time operational metrics for Qlik Sense.  
It simplifies day 2 operations ([[1](https://www.infoworld.com/article/3442754/why-de-risking-day-2-operations-is-a-smart-business-strategy.html)], [[2](https://dzone.com/articles/defining-day-2-operations)]) and is a must-have if you are responsible for a Sense environment with more than a dozen or so users.

Key features include

* Storing operational metrics to InfluxDB, for later viewing in Grafana dashboards.
* Extract warnings and errors from Sense. Makes it much easier to detect (and act!) on issues as they happen, rather than in retrospect much later.

[butler-sos.ptarmiganlabs.com](https://butler-sos.ptarmiganlabs.com)

### Butler CW

Butler Cache Warmer. Cache warming is the process of proactively forcing Sense apps to be loaded into the Sense server's memory, so they are readily available when users open them.

Once again - if your Sense environment serve more than a dozen users, you should consider a cache warming tool.

[github.com/ptarmiganlabs/butler-cw](https://github.com/ptarmiganlabs/butler-cw)

### Butler App Duplicator

No matter if you are a single developer creating Sense apps, or have lots of developers doing this, having app templates is a good idea:

* Lowered barrier of entry for new Sense developers.
* Productivity boost when developing Sense apps.
* Encouraging a common coding style and standard across all apps.

[github.com/ptarmiganlabs/butler-app-duplicator](https://github.com/ptarmiganlabs/butler-app-duplicator)

### Butler Spyglass

This tool is mainly of interest if you have lots of QVDs and apps, but when that's the case it's of paramount importance to understand what apps use which QVDs. In other words what data lineage looks like.

Butler Spyglass also extracts full load scripts for all Sense apps, creating a historical record of all load scripts for all Sense apps.

[github.com/ptarmiganlabs/butler-spyglass](https://github.com/ptarmiganlabs/butler-spyglass)

### Butler Notifier

This tool makes it easy to tap into the Qlik Sense notification API. From there you can get all kinds of notifications, including task reload failures and changes in session state (user login/logout etc).

[github.com/ptarmiganlabs/butler-notifier](https://github.com/ptarmiganlabs/butler-notifier)

### Butler Icon Uploader

Visual looks is important when it comes to analytics, and this holds true also for Sense apps.
The Butler Icon Uploader makes it easy to upload icon libraries (for example Font Awesome) to Qlik Sense Enterprise.  
With such icons available, Sense app developers can then use professional quality sheet and app icons in their Sense apps.ß

[github.com/ptarmiganlabs/butler-icon-upload](https://github.com/ptarmiganlabs/butler-icon-upload)
