---
title: "Single user"
linkTitle: "Single user"
weight: 20
description: >
  
---
## Butler Auth configuration

The settings in the config file are:

```yaml
singleUser:                         # "Single user" provider
    enable: false
    userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
    userId: goran                   # A user that already exists in Qlik Sense. All access to Sense will be done using this user.
```

| Field | Description |
|-|-|
| enable | Enable or disable this authentication provider. true/false. |
| userDirectory | The Qlik Sense Enterprise user directory that will be used once the user has been authenticated by the authentication provider. |
| userId | Qlik Sense user ID that all logins will use. |
