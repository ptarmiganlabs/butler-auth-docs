---
title: "Installing Butler Auth in a Docker container"
linkTitle: "Docker"
weight: 30
description: >
  How to install Butler Auth as a Docker container.
---

Prerequisites for running Butler Auth in Docker:

| What | Comment |
| ---- | ------- |
| Qlik Sense Enterprise on Windows | *Mandatory.* Butler Auth is developed with Qlik Sense Enterprise on Windows in mind. |
| Docker | *Mandatory.* A Docker runtime environment on any supported platform. This means you can run Butler Auth on any platform where Docker is available, including Linux, Mac OS, Windows and most cloud providers. Kubernetes is also a great option for running Butler Auth! |
| [InfluxDB](https://www.influxdata.com/time-series-platform/) | *Optional.* A database for realtime information, used to store metrics around Butler's own memory usage over time (if this feature is enabled). |

## Installation steps

The following steps give some guidance on how to get Butler Auth running on Docker.  
Here Mac OS has been used, things will look different on Linux and Windows.
<!-- TODO: update Docker install instructions -->
```bash
➜  ~ mkdir /Users/goran/butler-auth
➜  ~ cd /Users/goran/butler-auth
➜  butler-auth mkdir -p config/certificate
➜  butler-auth mkdir sessions
➜  butler-auth mkdir log
➜  butler-auth wget https://raw.githubusercontent.com/ptarmiganlabs/butler-auth/master/src/docker-compose.yaml
--2021-01-19 07:07:23--  https://raw.githubusercontent.com/ptarmiganlabs/butler-auth/master/src/docker-compose.yaml
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.0.133, 151.101.192.133, 151.101.128.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.0.133|:443... connected.
HTTP request sent, awaiting response... 404 Not Found
2021-01-19 07:07:24 ERROR 404: Not Found.
➜  butler-auth
➜  butler-auth cat docker-compose.yaml
# docker-compose.yml
version: '3.3'
services:
  butler-auth:
    image: ptarmiganlabs/butler-auth:1.0.0
    container_name: butler-auth
    restart: always
    ports:
      - "8081:8081"       # http/web server used to serve sample login forms included in Butler Auth
      - "8761:8761"       # REST API called by Qlik Sense Enterprise when users should be authenticated
    volumes:
      # Make config file and logs accessible outside of container
      - "./config:/nodeapp/config"
      - "./log:/nodeapp/log"
      - "./sessions:/nodeapp/sessions"
      - "/path/to/tls/cert:/nodeapp/config/tls"
    environment:
      - "NODE_ENV=production"
    logging:
      driver: json-file
      options:
        max-file: "5"
        max-size: "5m"
➜  butler-auth
```

At this point you should

1. Export certificates from the Qlik Sense QMC. Export a full set of certificates in PEM format, with or without passwords on the certificates. If a password is used it must also be specified in Butler Auth's config file.
2. Copy the certificates to the ./config/certificate directory.
3. Copy the [template config file](https://github.com/ptarmiganlabs/butler-auth/blob/master/src/config/production_template.yaml) from the GitHub repository to the ./config directory, modify it as needed based on your system(s) and which Butler Auth features you want enabled.  
Rename it to for example `production.yaml`.  
You can actually name the config file anything, but its name has to match the NODE_ENV environment variable, as set it the `docker-compose.yaml` file.
4. If using the `local-file` auth provider, you also need a corresponding YAML file where user info is stored. There is a [template file](https://github.com/ptarmiganlabs/butler-auth/blob/master/src/config/users.yaml) in the GitHub repository.
5. If using TLS to secure Butler Auth (you should!), the volumes entry `/path/to/tls/cert:/nodeapp/config/tls` in `docker-compose.yaml` must point to your TLS certificates.

When done, you should see something like this:

```bash
➜  butler-auth tree
.
├── config
│   ├── certificate
│   │   ├── client.pem
│   │   ├── client_key.pem
│   │   └── root.pem
│   ├── production.yaml
│   └── users.yaml
├── docker-compose.yaml
├── log
└── sessions

4 directories, 6 files
➜  butler-auth
```

At this point everything is ready and you can start the Butler Auth container using docker-compose (IP addresses and URLs have been slightly scrambled below):

```bash
➜  butler-auth docker-compose up
Creating network "butler-auth_default" with the default driver
Pulling butler-auth (ptarmiganlabs/butler-auth:1.0.0)...
1.0.0: Pulling from ptarmiganlabs/butler-auth
22f9b9782fc3: Already exists
072739d44e4f: Already exists
5111f27e9600: Already exists
dc22afea6082: Already exists
0ad0b403cda0: Already exists
bca65cadbc25: Already exists
c1e57ccc1a03: Already exists
2571476d0e73: Already exists
e3719000bb2c: Already exists
d09cb7e3b7d4: Pull complete
76d111860f8b: Pull complete
c30b9b6a8b26: Pull complete
e75f642798c7: Pull complete
5b06a9fb8f94: Pull complete
Digest: sha256:545e81b4a638cb2f50b7718723cd60528e91a237349429279a90928c95fa420f
Status: Downloaded newer image for ptarmiganlabs/butler-auth:1.0.0
Creating butler-auth ... done
Attaching to butler-auth
butler-auth    | 2021-01-19T06:22:18.070Z info: CONFIG: Influxdb enabled: true
butler-auth    | 2021-01-19T06:22:18.075Z info: CONFIG: Influxdb host IP: 1.2.3.4
butler-auth    | 2021-01-19T06:22:18.075Z info: CONFIG: Influxdb host port: 8086
butler-auth    | 2021-01-19T06:22:18.076Z info: CONFIG: Influxdb db name: butlerauth
butler-auth    | 2021-01-19T06:22:18.413Z info: AUTH-LOCALFILE: Setting up endpoints.
butler-auth    | 2021-01-19T06:22:18.415Z info: AUTH-LOCALFILE: Loading user list.
butler-auth    | 2021-01-19T06:22:18.419Z info: AUTH-LOCALFILE: Successfully loaded users from file.
butler-auth    | 2021-01-19T06:22:18.420Z debug: AUTH-LOCALFILE: Users loaded from file: [
butler-auth    |   {
butler-auth    |     "username": "anna",
butler-auth    |     "fullName": "Anna Svenson",
butler-auth    |     "password": "aaa",
butler-auth    |     "comment": "Root admin user"
butler-auth    |   },
butler-auth    |   {
butler-auth    |     "username": "joe",
butler-auth    |     "fullName": "Joe Jonson",
butler-auth    |     "password": "bbb",
butler-auth    |     "comment": "Regular user"
butler-auth    |   }
butler-auth    | ]
butler-auth    | 2021-01-19T06:22:18.421Z info: AUTH-LDAP: Setting up endpoints.
butler-auth    | 2021-01-19T06:22:18.421Z info: AUTH-GOOGLEOAUTH: Setting up endpoints.
butler-auth    | 2021-01-19T06:22:18.422Z info: AUTH-FACEBOOK: Setting up endpoints.
butler-auth    | 2021-01-19T06:22:18.423Z info: AUTH-MICROSOFT: Setting up endpoints.
butler-auth    | 2021-01-19T06:22:18.424Z info: AUTH-OKTA: Setting up endpoints.
butler-auth    | 2021-01-19T06:22:18.425Z info: AUTH-KEYCLOAK: Setting up endpoints.
butler-auth    | 2021-01-19T06:22:18.425Z info: AUTH-AUTH0: Setting up endpoints.
butler-auth    | 2021-01-19T06:22:18.426Z debug: HEARTBEAT: Setting up heartbeat to remote: http://healthcheck.mycompany.com/ping/12345678-1234-1234-1234-b10b81583522
butler-auth    | 2021-01-19T06:22:18.428Z info: --------------------------------------
butler-auth    | 2021-01-19T06:22:18.428Z info: Starting Butler authenticator
butler-auth    | 2021-01-19T06:22:18.428Z info: Log level: debug
butler-auth    | 2021-01-19T06:22:18.428Z info: App version: 1.0.0
butler-auth    | 2021-01-19T06:22:18.428Z info: --------------------------------------
butler-auth    | 2021-01-19T06:22:18.458Z info: MAIN: REST server now listening on butler-auth:8761
butler-auth    | 2021-01-19T06:22:18.459Z info: MAIN: Web server now listening on butler-auth:8081
butler-auth    | 2021-01-19T06:22:18.478Z info: CONFIG: Found InfluxDB database: butlerauth
butler-auth    | 2021-01-19T06:22:18.597Z debug: HEARTBEAT: Sent heartbeat to http://healthcheck.mycompany.com/ping/12345678-1234-1234-1234-b10b81583522

```

What you see on your screen will depend on which Butler Auth version you are using and what features are enabled.
