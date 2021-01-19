---
title: "REST/web server"
linkTitle: "REST/web server"
weight: 50
description: >
  How to configre up the REST and web servers that Butler Auth manage.
---

{{% alert title="Mandatory" color="primary" %}}
These settings are mandatory.

Do note though that Butler Auth expects the configuration properties below to exist in the config file, but will *ignore their values* if the related features are disabled.
{{% /alert %}}

## What's this?

Butler Auth exposes a REST server which Qlik Sense will call when a user connects to a Qlik Sense virtual proxy.  
There are then different REST API endpoints for different authentication providers.
The REST server is also responding to the callbacks that happen as part of for example an OAuth 2.0 authentication flow. When the authentication provider has given green light, that provider's callback endpoint on the REST server is called.

Secondly, a http server is used to serve a sample web based login interface.  
This web site is for inspiration only. It has hard coded URLs that need to be changed before it is useful in your own environment. Still, it can serve as a basis for your own, custom/branded login page for Qlik Sense.  
Each authentication provider that rely on some kind of web interface for entering user name and password has a `url` property the main YAML config file (for example `ButlerAuth.authProvider.ldap.url`). Those property can be used to direct authentication requests to your own, custom login user interfaces.

The settings below control the behvaiour of those two servers.

## Settings in main config file

```yaml
---
ButlerAuth:
  ...
  ...
  # Server and https settings
  server:
    rest:                   # The REST server is the authentication server called by Qlik Sense.
      host: <FQDN>          # Hostname of the REST server. Would be container name when running under Docker, otherwise something like qliksenseauth.mydomain.com.
      port: 8761            # Port where the REST server will listen
      rateLimit:            # Used to limit number of authentication requests during a given time window. Used to prevent brute forcing attacks.
        enable: false       # Enable/disable rate limiting for this REST API. true/false.
        windowSize: 60000   # How long (milliseconds) is the time window we're capping # of logins for? Default 5 min if not specified.
        maxCalls: 100       # Max # of logins from Qlik Sense during time window above. Default 100 if not specified.
      tls:                  # Many 3rd party auth providers require the OAuth 2 server to use https. 
        enable: true        # Enable/disable https. While always recommended, https is strictly not needed for local file authentication, for example.
        cert: /path/to/certfile.cer    # TLS certificate file 
        key: /path/to/certfile.key     # TLS key file
        password:           # Passphrase for TLS key file, if used
    web:                    # The web/http server used to serve the sample login pages included in Butler Auth
      host: <FQDN>          # FQDN of the http server 
      port: 8081            # Port where the http server will listen
      tls:                  # Used to secure the http server with TLS. I.e. https.
        enable: true
        cert: /path/to/certfile.cer
        key: /path/to/certfile.key
        password: 
  ...
  ...
```
