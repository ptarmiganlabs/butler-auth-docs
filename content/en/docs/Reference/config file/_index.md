---
title: "Config file syntax"
linkTitle: "Config file syntax"
weight: 10
description: >
  Description of Butler Auth's config file.
---

## Main config file

The `production_template.yaml` config file looks like this:

```yaml
ButlerAuth:
    # Logging configuration
    logLevel: info      # Log level. Possible log levels are silly, debug, verbose, info, warn, error
    fileLogging: false  # true/false to enable/disable logging to disk file
    logDirectory: log   # Subdirectory where log files are stored. Absolute or relative path accepted.

    # Heartbeats can be used to send "I'm alive" messages to any other tool, e.g. a infrastructure monitoring tool
    # The concept is simple: The remoteURL will be called at the specified frequency. The receiving tool will then know 
    # that Butler Auth is alive.
    heartbeat:
        enable: false
        remoteURL: http://my.monitoring.server/some/path/
        frequency: every 1 minute         # https://bunkat.github.io/later/parsers.html

    # Docker health checks are used when running Butler Auth as a Docker container. 
    # The Docker engine will call the container's health check REST endpoint with a set interval to determine
    # whether the container is alive/well or not.
    # If you are not running Butler Auth in Docker you can safely disable this feature. 
    dockerHealthCheck:
        enable: false    # Control whether a REST endpoint will be set up to serve Docker health check messages
        port: 12398      # Port the Docker health check service runs on (if enabled)

    # Uptime monitor
    uptimeMonitor:
        enable: false                    # Should uptime messages be written to the console and log files?
        frequency: every 15 minutes      # https://bunkat.github.io/later/parsers.html
        logLevel: info                   # Starting at what log level should uptime messages be shown?
        storeInInfluxdb: 
            enable: false
            hostIP: <IP or host name>
            hostPort: 8086
            auth:
                enable: false
                username: user_joe
                password: joesecret
            dbName: butlerauth
            instanceTag: DEV              # Tag that can be used to differentiate data from multiple Butler instances
            # Default retention policy that should be created in InfluxDB when Butler creates a new database there. 
            # Any data older than retention policy threshold will be purged from InfluxDB.
            retentionPolicy:
                name: 10d
                duration: 10d

    # Server and https settings
    server:
        rest:                       # The REST server is the authentication server called by Qlik Sense.
            host: <FQDN>            # FQDN of the REST server. E.g. qliksenseauth.mydomain.com. Most 3rd party auth providers demand a proper FQDN here, rather than just an IP. 
            port: 8761              # Port where the REST server will listen
            rateLimit:              # Used to limit number of authentication requests during a given time window. Used to prevent brute forcing attacks.
                windowSize: 60000   # How long (milliseconds) is the time window we're capping # of logins for? Default 5 min if not specified.
                maxCalls: 100       # Max # of logins from Qlik Sense during time window above. Default 100 if not specified.
            tls:                    # Many 3rd party auth providers require the OAuth 2 server to use https. 
                enable: true        # Enable/disable https. While always recommended, https is strictly not needed for local file authentication, for example.
                cert: /path/to/certfile.cer    # TLS certificate file 
                key: /path/to/certfile.key     # TLS key file
                password:           # Passphrase for TLS key file, if used
        web:                        # The web/http server used to serve the sample login pages included in Butler Auth
            host: <FQDN>            # FQDN of the http server 
            port: 8081              # Port where the http server will listen
            tls:                    # Used to secure the http server with TLS. I.e. https.
                enable: true
                cert: /path/to/certfile.cer
                key: /path/to/certfile.key
                password: 

    # Auth providers are responsible for authenticating users before they are forwarded to Qlik Sense Enterprise.
    authProvider:       
        localFile:                          # "Local file" provider reads user data from a file on disk
            enable: false                    
            userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
            userFile: ./config/users.yaml   # YAML file containing usernames and passwords

        ldap:                               # "LDAP" provider
            enable: false
            userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
            ldapServer:                     # Information about the LDAP server to authenticate against
                host: <ldap(s)://ldap.mydomain.com>    # Both normal (ldap://) and secure (ldaps://) LDAP is supported
                port: 636                   # Usually 389 for LDAP and 636 for LDAPS
                bindUser: '<domain\username>'       # Service account used to log into the LDAP server
                bindPwd: <password>         # Password of service account
                searchBase: '<dc=...,dc=...,dc=...>'    # Base path from which authentication attempts will start
                searchFilter: '(&(objectcategory=person)(objectclass=user)(|(samaccountname={{username}})(mail={{username}})))' # Filter used to get info about users in LDAP server
                tls: 
                    # Settings here will override default TLS behaviour. 
                    # Useful for example if your cert is for another domain wrt the host name of the LDAP server.
                    # If a setting is empty it will simply be ignored by Butler Auth.

                    # Necessary if the LDAP server isusing a self-signed certificate
                    # Should point to a PEM coded CA certificate file.
                    ca: 

        singleUser:                         # "Single user" provider
            enable: false
            userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
            userId: goran                   # A user that already exists in QLik Sense. All access to Sense will be done using this user.

        google:                             # "Google" OAuth2 provider
            enable: false
            userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
            userIdShort: true               # If true, the email domain will be removed. I.e. "joe.smith@domain.com" will be changed to "joe.smith".
            clientId: <Client ID>
            clientSecret: <Client secret>

        facebook:                           # "Facebook" OAuth2 provider
            enable: false
            userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
            userIdShort: true               # If true, the email domain will be removed. I.e. "joe.smith@domain.com" will be changed to "joe.smith".
            clientId: <Client ID>
            clientSecret: <Client secret>

        microsoft:                          # "Microsoft" OAuth2 provider
            enable: false
            userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
            userIdShort: true               # If true, the email domain will be removed. I.e. "joe.smith@domain.com" will be changed to "joe.smith".
            clientId: <client ID>
            clientSecret: <Client>

        okta:                               # "Facebook" provider
            enable: false
            userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
            userIdShort: true               # If true, the email domain will be removed. I.e. "joe.smith@domain.com" will be changed to "joe.smith".
            clientId: <Client ID>
            clientSecret: <Client secret>
            oktaDomain: <Okta domain from Google admin console>  # E.q. https://myid-123456.okta.com'
            idp: 

        keycloak:                           # "Keycloak" provider
            enable: true
            userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
            userIdShort: true               # If true, the email domain will be removed. I.e. "joe.smith@domain.com" will be changed to "joe.smith".
            clientId: <Client ID>
            clientSecret: <Client secret>
            host: <FQDN of Keycloak server> # E.g. https://keycloak.mydomain.com
            realm:                          # E.g. ptarmiganlabs
            authorizationURL: <URL>         # E.g. https://keycloak.mydomain.com/auth/realms/<myrealm>/protocol/openid-connect/auth
            tokenURL: <URL>                 # E.g. https://keycloak.mydomain.com/auth/realms/<myrealm>/protocol/openid-connect/token
            userInfoURL: <URL>              # E.g. https://keycloak.mydomain.com/auth/realms/<myrealm>/protocol/openid-connect/userinfo

        auth0:                              # "Auth0" provider
            enable: true
            userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
            userIdShort: true               # If true, the email domain will be removed. I.e. "joe.smith@domain.com" will be changed to "joe.smith".
            issuerBaseURL: <FQDN>           # E.g. mycompany.eu.auth0.com
            baseURL: <URL>                  # FQDN of Butler Auth, e.g. https://qliksenseauth.mycompany.com:8761'
            clientId: <Client ID>
            clientSecret: <Client secret>

    # Settings relating to Qlik Sense
    qlikSense:
        # Certificates to use when calling Sense APIs. Get these from the Certificate Export in QMC.
        certFile:
            # If running under Docker, use these paths:
            # clientCert: /nodeapp/config/certificate/client.pem
            # clientCertKey: /nodeapp/config/certificate/client_key.pem
            # clientCertCA: /nodeapp/config/certificate/root.pem

            # If running as a native Node.js app, point to where the cert files are stored:
            clientCert: /path/to/client.pem
            clientCertKey: /path/to/client_key.pem
            clientCertCA: /path/to/root.pem
            clientCertPassphrase: 
```

### Comments

* The default location for cert/key exports is (assuming a standard install of Qlik Sense) `C:\ProgramData\Qlik\Sense\Repository\Exported Certificates\<name specified during certificate export>`

  The files needed by Butler Auth are `client.pem`, `client_key.pem` and `root.pem`.
