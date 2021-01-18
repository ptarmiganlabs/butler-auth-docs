---
title: "Auth providers"
linkTitle: "Auth providers"
weight: 60
description: >
  How to configure the authentication providers.
---

{{% alert title="Mandatory" color="primary" %}}
These settings are mandatory.

You could in theory disable all the authentication providers, but then you don't need Butler Auth to begin with.  

In other words: Enable at least one provider.
{{% /alert %}}

## What's this?

A core concept of Butler Auth is that of *authentication providers*, or just providers.

A provider's purpose is to offer a way to authenticate users.  
Once users are authenticated they are forwarded to Qlik Sense.

Providers can be individually enabled and disabled, i.e. there are no dependencies between providers.  
Likewise, there are no limitations on how many or which providers can be enabled at the same time.

Each provider has it's own settings in the config file.  
If new providers are added to Butler Auth they will need to be configured in the config file.

## Settings in main config file

```yaml
---
ButlerAuth:
  ...
  ...
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
  ...
  ...
```
