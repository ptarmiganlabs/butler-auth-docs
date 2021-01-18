---
title: "Local file"
linkTitle: "Local file"
weight: 20
description: >
  Use a local file as your user directory.
---
## Butler Auth configuration

The settings in the config file are:

```yaml
localFile:                          # "Local file" provider reads user data from a file on disk
    enable: false                    
    userDirectory: lab              # Qlik Sense user directory that will be used for the authenticated user
    userFile: ./config/users.yaml   # YAML file containing usernames and passwords
```

| Field | Description |
|-|-|
| enable | Enable or disable this authentication provider. true/false. |
| userDirectory | The Qlik Sense Enterprise user directory that will be used once the user has been authenticated by the authentication provider. |
| userFile | A YAML file containing usernames and passwords. |

## Userfile format

The file containing user credentials is YAML-formatted:

```yaml
users:
  - username: anna
    fullName: Anna Svenson
    password: aaa
    comment: Root admin user
  - username: joe
    fullName: Joe Jonson
    password: bbb
    comment: Regular user
```

{{% alert title="Remember!" color="warning" %}}
Make sure to secure this file properly.

Anyone with access to this file can log into Qlik Sense as any of the users in the file.
{{% /alert %}}
