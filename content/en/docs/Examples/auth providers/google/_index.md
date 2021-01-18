---
title: "Google"
linkTitle: "Google"
weight: 20
description: >
  Using Google credentials with Butler Auth.
---

Google offers authentication using OAuth 2.0.

## Butler Auth configuration

The settings in the config file are:

```yaml
google:                             # "Google" OAuth2 provider
    enable: false
    userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
    userIdShort: true               # If true, the email domain will be removed. I.e. "joe.smith@domain.com" will be changed to "joe.smith".
    clientId: <Client ID>
    clientSecret: <Client secret>
```

| Field | Description |
|-|-|
| enable | Enable or disable this authentication provider. true/false. |
| userDirectory | The Qlik Sense Enterprise user directory that will be used once the user has been authenticated by the authentication provider. |
| userIdShort | The provider will return the user's email address. If `userIdShort` is set to `true`, the @ character and email domain will be stripped from the email address returned by the provider. For example, "joe@company.com" would become just "joe". true/false. |
| clientId | Client ID from Google |
| clientSecret | Client secret from Google |

## Google configuration

General steps to set up Google for use with Butler Auth.

### Create application

1. Log in to [Google Cloud](https://console.developers.google.com). The [dashboard](https://console.cloud.google.com/home/dashboard) provides a good overview of your Google cloud assets.
2. Go to the "Credentials" section ([https://console.developers.google.com/apis/credentials](https://console.developers.google.com/apis/credentials)) of the [API & Services](https://console.developers.google.com/apis/dashboard) page. Make sure to select the correct project in the drop-down at the top of the page.
3. Create a new client ID:
   1. "CREATE CREDENTIALS" (button at top) > "OAuth client ID" > Application typ "Web application"
   2. Enter desired name in the "Name" field.
   3. Add valid callback URIs. The one responsible for Butler Auth is `https://<FQDN of Butler Auth>:<Butler Auth REST port>/oauth2callback`. Note that Butler Auth's REST API port can be changed in the main config file of Butler Auth. At least one URI is required.  
   Click "Create".
   4. Take note of the client ID and secret. Copy them to the corresponding fields in Buther Auth's config file.
4. Set up an [OAuth consent screen](https://console.developers.google.com/apis/credentials/consent) that will be shown to users when they authenticate to Qlik Sense using their Google credentials.
