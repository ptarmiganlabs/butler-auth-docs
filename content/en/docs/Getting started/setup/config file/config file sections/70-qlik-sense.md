---
title: "Qlik Sense"
linkTitle: "Qlik Sense"
weight: 70
description: >
  
---

{{% alert title="Mandatory" color="primary" %}}
These settings are mandatory.
{{% /alert %}}

## What's this?

These settings tell Butler Auth where to find the certificates it needs to communicate with Qlik Sense Enterprise.

## Settings in main config file

```yaml
---
ButlerAuth:
  ...
  ...
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
    ...
    ...
```
