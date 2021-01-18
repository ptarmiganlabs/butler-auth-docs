---
title: "Which config file to use"
linkTitle: "Which config file?"
weight: 10
description: >
  Butler Auth can be configured using different config files. Here you learn to control which one is used.
---

A description of the config file format is available [here](/docs/reference/config-file/).

## Select which config file to use

Butler Auth uses configuration files in [YAML](https://en.wikipedia.org/wiki/YAML) format. The config files are stored in the `src/config` folder.  

Trying to run Butler Auth with the default config file (the one included in the files download from GitHub) will not work - you need to adapt it to your server environment.

The name of the config file matters. Butler Auth looks for an environment variable called "NODE_ENV" and then tries to load a config file named with the value found in NODE_ENV.

For example:

* NODE_ENV=production

* Butler will look for a config file `config/production.yaml`.

### Setting environment variables

The method for setting environment variables varies between different operating systems:

On Windows:

    set NODE_ENV=production

Mac OS or Linux

    export NODE_ENV=production

If using Docker, the NODE_ENV environment varible is set in the docker-compose.yml file (as already done in the [sample docker-compose file](https://github.com/ptarmiganlabs/butler-auth/blob/master/src/docker-compose.yaml).)
