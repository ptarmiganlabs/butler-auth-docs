---
title: "Heartbeats"
linkTitle: "Heartbeats"
weight: 20
description: >
  Heartbeats provide a way to monitor that Butler Auth is running and working as intended.  

  Butler can send periodic heartbeat messages to a monitoring tool, which can then alert if Butler Auth hasn't checked in as expected.
---

{{% alert title="Optional" color="primary" %}}
These settings are optional.

If you don't need this feature just disable it and leave the default values in the config as they are.

Do note though that Butler Auth expects the configuration properties below to exist in the config file, but will *ignore their values* if the related features are disabled.
{{% /alert %}}

## What's this?

A tool like Butler Auth should be viewed as mission critical, at least if the Qlik Sense environment as such is considered mission critical.

But how can you know whether Butler Auth itself is working?  
Somehow Butler Auth itself should be monitored.

Butler Auth (and most other tools in the Butler family) has a **heartbeat** feature.  
It sends periodic messages to a monitoring tool, which can then alert if Butler hasn't checked in as expected.

[Healthchecks.io](https://healthchecks.io/) is an example of such as tool. It's open source but also has a SaaS option if so preferred. Highly recommended!

More info on using Healthchecks.io with the Butler family can be found [in this blog post](https://ptarmiganlabs.com/blog/2020/07/26/black-box-monitoring-of-butler-tools-monitoring-the-monitor/).

## Settings in main config file

```yaml
---
Butler:
  ...
  ...
  # Heartbeats can be used to send "I'm alive" messages to any other tool, e.g. a infrastructure monitoring tool
  # The concept is simple: The remoteURL will be called at the specified frequency. The receiving tool will then know 
  # that Butler Auth is alive.
  heartbeat:
    enable: false
    remoteURL: http://my.monitoring.server/some/path/
    frequency: every 1 minute         # https://bunkat.github.io/later/parsers.html
  ...
  ...
```
