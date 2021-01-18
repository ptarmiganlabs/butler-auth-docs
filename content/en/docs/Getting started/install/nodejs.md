---
title: 'Running Butler Auth as a native Node.js application'
linkTitle: 'Node.js app'
weight: 40
description: >
    How to install Butler Auth as a Node.js application.
---

While Qlik Sense Enterprise is a Windows only system, Butler Auth runs on any OS where Node.js is available.  
Butler Auth has been succesfully used using standard Node.js - during development and production - on Windows, Linux (Debian and Ubuntu tested) and Mac OS.

## Running as Node.js app

Prerequisites:

| What             | Comment |
| ---------------- | ------- |
| Sense Enterprise | _Mandatory._ Butler Auth is developed with Qlik Sense Enterprise on Windows in mind. |
| [Node.js](https://nodejs.org) | _Mandatory._ |
| [InfluxDB](https://www.influxdata.com/time-series-platform/) | *Optional.* A database for realtime information, used to store metrics around Butler Auth's own memory usage over time (if this feature is enabled). |

## Installation steps

- **Install node.js**  
    Butler Auth was developed and tested using the 64 bit version of [Node.js](https://nodejs.org/en/download/). The most recent LTS (Long Term Support) version of Node.js is a good choice for running Butler. At time of this writing, Node.js 14.5 is used.

- **Decide where to install Butler Auth**  
    It is usually a good starting point to run Butler Auth on a Sense server.

    On the other hand, you might want to keep the Sense servers as clean as possible (with respect to software running on them). If that is a priority you should install Butler Auth on some other server. A small "utility" or "misc" server can often be useful for running various add-on services to Qlik Sense.

    The bottom line is that Butler Auth can run on any server, as long as Node.js is available and there is network connectivity to the actual Qlik Sense server(s).

- **Download Butler Auth**  
    Download the repository zip from the [releases page](https://github.com/ptarmiganlabs/butler-auth/releases) file or clone the Butler Auth repository using your git tool of choice. Both options achieve the same thing, i.e. a directory such as d:\node\butler-auth, which is then Butler Auth's root directory.

- **Install node dependencies**  
    From a Windows command prompt (assuming the Butler ZIP file/repository was saved to d:\\node\\butler-auth):

      d:
      cd \node\butler-auth\src
      npm install

    This will download and install all Node.js modules used by Butler Auth.

