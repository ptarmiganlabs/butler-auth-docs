---
title: "Logging"
linkTitle: "Logging"
weight: 10
description: >
  Logging can be done to disk and/or console, with customisable logging levels and location of log files.
---

{{% alert title="Mandatory" color="primary" %}}
These settings are mandatory.

While logging to disk files is optional, logging to console will always happen.  
You should also set a log level.

Do note though that Butler expects the configuration properties below to exist in the config file, but will *ignore their values* if the related features are disabled.
{{% /alert %}}

## What's this?

Logging tells you what Butler Auth has done in the past.  
Who logged in when, if you like.

Temporarily increasing logging verbosity can also be very useful when setting up new authentication providers.  
Don't forget to dial back the logging level once everthing works though!

## Settings in config file

```yaml
---
ButlerAuth:
  ...
  ...
  # Logging configuration
  logLevel: info      # Log level. Possible log levels are silly, debug, verbose, info, warn, error
  fileLogging: false  # true/false to enable/disable logging to disk file
  logDirectory: log   # Subdirectory where log files are stored. Absolute or relative path accepted.
  ...
  ...
```
