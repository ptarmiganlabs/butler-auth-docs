---
title: 'Security considerations'
linkTitle: 'Security considerations'
weight: 100
description: >
    Security is important! Keep these things in mind when deploying Butler Auth.
---

{{% pageinfo %}}
Security is a whole field in its ownn.  
What's considered secure in one company might be considered insecure in another.

It's a good idea to review your tools and services every now and then, for example making sure they are updated to include the latest secutity patches etc.

When in doubt, be paranoid.
{{% /pageinfo %}}

## Security considerations

{{% alert title="Security/Disclosure" color="warning" %}}
If you discover a serious bug with Butler Auth that may pose a security problem, please disclose it confidentially to security@ptarmiganlabs.com first, so that it can be assessed and hopefully fixed prior to being exploited. Please do not raise GitHub issues for security-related doubts or problems.
{{% /alert %}}

- Configure the firewall on the server where Buter Auth is running to only accept connections from IPs that matter. While convenient and tempting to leave servers open with respect to network traffic, it's usually a very bad idea. Reduce the attack vectors whenever possible.

- Butler Auth uses various Node.js modules from [npm](https://www.npmjs.com/). If concerned about security, you should review these dependencies and decide whether there are issues in them or not.

- Same thing with Butler Auth itself - while efforts have been made to make it secure, you need to decide for yourself whether the level of security is enough for your use case.

    Butler is continuously checked for security vulnerabilities by using GitHub security audit, [Snyk](https://snyk.io/), npm audit and other tools.  

    That said, all software - including Butler Auth - has bugs and security issues.

- Once again: ***YOU*** are responsible for determining if Butler Auth meets ***YOUR*** security requirements. The legal text is found in the [MIT license](/docs/legal-stuff/).

## Butler Auth talking to Qlik Sense

Butler Auth uses https for all communication with Sense, using Sense's certificates for authentication.
