---
title: 'Choosing an authentication solution'
linkTitle: 'Choosing an auth solution'
weight: 10
description: >
    Authentication is about ensuring people are who they claim to be. This can be done in many ways with their respective pros, cons and security concerns.
---

There aren't really any right or wrong here.  
What's right for your company might be unacceptable for another company, and vice versa.

This page is intended to make you think and consider the options you have.

## Commercial service or open source?

The authentication providers supported by Butler Auth generally belong to either of two categories:

- Commercial identify and authentication providers such as Okta and Auth0.  
  These companies have entire teams dedicated to creating secure solutions for their customers.
  But the transparency is non-existing. They will for sure use open standards like OAuth 2.0, OpenID etc - but you will never know exactly what the implementations look like.  
  In other words: You will never be able to audit the code responsible for your authentication flow - you have to trust that these companies know what they are doing.

- Open source libraries provide full transparency. You can audit them if/when needed.  
  The downside is that these libraries are sometimes developed by individuals or companies that no longer actively maintain them. Which in worst case *could* mean that new security issues are discovered in the standard authentication protocol, but the open source libraries are not quickly updated - or not updated at all.
  While this is rare, it's a real possibility.

## Passport.js

Butler Auth relies heavily on the [authentication strategies](http://www.passportjs.org/packages/) available via [Passport.js](http://www.passportjs.org/).

This is an open source authentication library boasting 500+ various kinds of authentication variants.

Now, there will be duplicates (several strategies all offering Google authentication) and most of those strategies will be rather obscure - with the associated risk of not being supported.  
The big ones - Google, Microsoft, generic OAuth 2.0, Auth0, LDAP, OpenID etc - are generall well supported and kept up to date.  
For that reason they are also used in Butler Auth.

{{% alert title="Note!" color="warning" %}}
If you do not trust the various authentication modules that are part of Passport.js, you should not use them via Butler Auth.  

Butler Auth is simply a thin wrapper around those modules.
{{% /alert %}}

## Recommendation

So how should you reason if you have the luxury of deciding yourself what authentication solution to use?

- If low cost is the main focus you should go with the big, established service providers. Google, Microsoft, GitHub, Amazon etc. By using open source authentication modules you won't have any cost for the software or service.  
These are reasonably easy to set up and get working.

- If you have high security requirements and prefer (or must use) open source, Keycloak is a good option.

  Keycloak can however be challenging to set up properly, at least if you also want to lock it down to meet specific security requirements. It's like a Swiss army knife of authentication, with many, many tools in it...  
  There is simply a risk of configuring it incorrectly and not achieve the desired security level as a result.

- If you can accept some costs associated with authentication you could use one of the commercial services.
  With some of them you can still use Google/Microsoft/... authentication if so desired, but you offload that integration work to the service provider.
