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
proton:~ goran$ mkdir /Users/goran/butler-auth
proton:~ goran$ cd /Users/goran/butler-auth
proton:butler-auth goran$ mkdir -p config/certificate
proton:butler-auth goran$
proton:butler goran$ wget https://raw.githubusercontent.com/ptarmiganlabs/butler-auth/master/src/docker-compose.yml
--2021-01-02 22:12:52--  https://raw.githubusercontent.com/ptarmiganlabs/butler-auth/master/src/docker-compose.yml
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.84.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.84.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 618 [text/plain]
Saving to: 'docker-compose.yml'

docker-compose.yml                         100%[========================================================================================>]     618  --.-KB/s    in 0s

2021-01-02 22:12:52 (10.7 MB/s) - 'docker-compose.yml' saved [618/618]


proton:butler-auth goran$ cat docker-compose.yml
# docker-compose.yml
version: '3.3'
services:
  butler:
    image: ptarmiganlabs/butler:3.0.0
    container_name: butler
    restart: always
    ports:
      - "8080:8080"     # REST API available on port 8180 to services outside the container
      - "9997:9997"     # UDP port for session connection events
      - "9998:9998"     # UDP port for task failure events
    volumes:
      # Make config file accessible outside of container
      - "./config:/nodeapp/config"
    environment:
      - "NODE_ENV=production"
    logging:
      driver: json-file
      options:
        max-file: "5"
        max-size: "5m"
proton:butler goran$

```

At this point you should

1. Export certificates from the Qlik Sense QMC. Export a full set of certificates in PEM format, no password on the certificates.
2. Copy the certificates to the ./config/certificate directory.
3. Copy the [template config file](https://github.com/ptarmiganlabs/butler-auth/blob/master/src/config/production_template.yaml) from the GitHub repository to the ./config directory, modify it as needed based on your system(s) and which Butler Auth features you want enabled, and rename it to for example `production.yaml`.  
You can name the config file anything, but its name has to match the NODE_ENV environment variable, as set it the `docker-compose.yml` file.
4. *Optional.* Copy the template schedule file to the location specified in Butler Auth's config file. This is only needed if you manually want to add schedules. If using the API to create schedules, there is no need to first manually create a schedules file (the schedule file will be created by Butler Auth in this case).

When done, you should see something like this:

```bash
proton:butler-auth goran$ pwd
/Users/goran/butler-auth
proton:butler goran$ ls -la
total 8
drwxr-xr-x   4 goran  staff   128 Sep 26 16:36 .
drwxr-xr-x+ 59 goran  staff  1888 Sep 26 16:24 ..
drwxr-xr-x   4 goran  staff   128 Sep 26 16:36 config
-rw-r--r--   1 goran  staff   565 Sep 26 16:25 docker-compose.yml
proton:butler-auth goran$
proton:butler-auth goran$ ls -la config/
total 8
drwxr-xr-x  4 goran  staff   128 Sep 26 16:36 .
drwxr-xr-x  4 goran  staff   128 Sep 26 16:36 ..
drwxr-xr-x  6 goran  staff   192 Sep 26 16:36 certificate
-rw-r--r--  1 goran  staff  1861 Sep 26 16:36 production.yaml
proton:butler-auth goran$
proton:butler-auth goran$ ls -la config/certificate/
total 32
drwxr-xr-x  6 goran  staff   192 Sep 26 16:36 .
drwxr-xr-x  4 goran  staff   128 Sep 26 16:36 ..
-rw-r--r--@ 1 goran  staff  1166 Sep 26 16:36 client.pem
-rw-r--r--@ 1 goran  staff  1702 Sep 26 16:36 client_key.pem
-rw-r--r--@ 1 goran  staff  1192 Sep 26 16:36 root.pem
proton:butler-auth goran$
```

At this point everything is ready and you can start the Butler Auth container using docker-compose:

```bash
proton:butler-auth goran$ docker-compose up
Creating network "butler_default" with the default driver
Pulling butler-auth (ptarmiganlabs/butler:1.0.0)...
3.0.0: Pulling from ptarmiganlabs/butler
9a0b0ce99936: Already exists
db3b6004c61a: Already exists
f8f075920295: Already exists
6ef14aff1139: Already exists
0bbd8b48260f: Already exists
524be717efb1: Already exists
da330b3729a7: Already exists
2c9863d012f5: Already exists
06cd084e76f0: Already exists
2c5533f377d0: Pull complete
307c3aa5e73e: Pull complete
10617d6f19b9: Pull complete
ad221e369f17: Pull complete
b1f6c19b1af6: Pull complete
Digest: sha256:b3c17c93c1779d62e21db5f3f7691f524ab0c21d8b0814cab41a66e814702a17
Status: Downloaded newer image for ptarmiganlabs/butler:3.0.0
Creating butler ... done
Attaching to butler
butler    | 2019-11-18T21:51:17.630Z info: --------------------------------------
butler    | 2019-11-18T21:51:17.634Z info: Starting Butler
butler    | 2019-11-18T21:51:17.635Z info: Log level is: debug
butler    | 2019-11-18T21:51:17.636Z info: App version is: 3.0.0
butler    | 2019-11-18T21:51:17.637Z info: --------------------------------------
butler    | 2019-11-18T21:51:17.638Z debug: Client cert: /nodeapp/config/certificate/client.pem
butler    | 2019-11-18T21:51:17.639Z debug: Client cert key: /nodeapp/config/certificate/client_key.pem
butler    | 2019-11-18T21:51:17.639Z debug: CA cert: /nodeapp/config/certificate/root.pem
butler    | 2019-11-18T21:51:17.673Z debug: Registering REST endpoint /v2/activeUserCount
butler    | 2019-11-18T21:51:17.676Z debug: Registering REST endpoint /v2/activeUsers
butler    | 2019-11-18T21:51:17.679Z debug: Registering REST endpoint /v2/slackPostMessage
butler    | 2019-11-18T21:51:17.680Z debug: Registering REST endpoint /v2/createDir
butler    | 2019-11-18T21:51:17.681Z debug: Registering REST endpoint /v2/createDirQVD
butler    | 2019-11-18T21:51:17.681Z debug: Registering REST endpoint /v2/mqttPublishMessage
butler    | 2019-11-18T21:51:17.682Z debug: Registering REST endpoint /v2/senseStartTask
butler    | 2019-11-18T21:51:17.683Z debug: Registering REST endpoint /v2/senseAppDump
butler    | 2019-11-18T21:51:17.684Z debug: Registering REST endpoint /v2/senseListApps
butler    | 2019-11-18T21:51:17.684Z debug: Registering REST endpoint /v2/butlerping
butler    | 2019-11-18T21:51:17.685Z debug: Registering REST endpoint /v2/base62ToBase16
butler    | 2019-11-18T21:51:17.686Z debug: Registering REST endpoint /v2/base16ToBase62
butler    | 2019-11-18T21:51:17.687Z debug: Server for UDP server: butler
butler    | 2019-11-18T21:51:17.688Z debug: REST server host: butler
butler    | 2019-11-18T21:51:17.688Z debug: REST server port: 8080
butler    | 2019-11-18T21:51:17.696Z info: UDP server listening on 172.21.0.2:9998
butler    | 2019-11-18T21:51:17.699Z info: REST server listening on http://172.21.0.2:8080
butler    | 2019-11-18T21:51:17.700Z info: UDP server listening on 172.21.0.2:9997

```

What you see on your screen will depend on which Butler Auth version you are using and what features are enabled.
