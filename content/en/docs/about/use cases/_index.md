+++ 
title = "Use cases" 
description = "How can Butler Auth be used?"
weight = 30
+++

{{% alert title="Disclaimer" color="warning" %}}
All your use of Qlik Sense Enterprise on Windows is subject to the licensing and other terms you have received from Qlik.  
These may in theory differ between Qlik's customers and also change over time.

You are yourself responsible for complying with terms.

If Butler Auth offers or enables features that do not align with the licesning terms you have received from Qlik, it is still *your* responsibility to comply with Qlik's terms and conditions.

None of the companies and/or individuals that have at some point contributed to Butler Auth, should at any time, in any way, be held liable for how you use Butler Auth.
{{% /alert %}}

### Log into QSEoW using one of the major cloud providers' directory service

If you already use Microsoft Azure, Google, Okta or Auth0 as identify provider, you probably want to do the same also for Qlik Sense.

### Log into QSEoW using LDAP for as both user and credentials

A very common setup (possibly *the* most common) is to use Active Directory for storing user info and credentials, and then talk to AD using the LDAP protocol.

QSEoW natively supports syncing *user names* from some service using LDAP, this does not include passwords though.  
This is actually not authentication at all, it's just a way to sync user names and attributes from a directory service to QSEoW, using the LDAP protocol.

LDAP can however also be used to *authenticate* users, and Butler Auth supports this.

One use case is thus to get both the Qlik Sense user directory info (i.e. user names and attributes) *and* authenticate users via LDAP.

### Authenticate against Keycloak for a fully open source setup

[Keycloak](https://www.keycloak.org/) is a very powerful, open source identity provider.

In situations where open source solutions are preferred or mandated the combination of Keycloak and Butler Auth provides a 100% open source authentication solution for QSEoW.

### Increaseed redundancy by offering sign-in via multiple channels

If you have multiple authentication solutions Butler Auth can be used to support several (or all) of them.

For example, let's assume Okta is the primary authentication provider in your company but you also use Google's tools (GMail, Sheets, Docs etc).

Butler Auth can then be configured for both Okta and Google authentication, with the effect that users can use either service when logging into Sense.

### Use a single user account for all access to QSEoW

Sometimes you want all access to Qlik Sense to happen via a specific user account.

Admittedly, this might not be a very common scenario. But consider a trade show or other event where people should be able to test some Qlik Sense based application - ideally without having to log in at all.

You could solve this by using a Qlik Sense license that allows for anonymous users - but that's probably overkill if the purpose is a simple demo event.

Another option is to use Butler Auth to force all access to Sense to use a single Sense account.  
Given Sense's limits on number of simultaneous sessions, only a handful (or less) people can try that Sense app at once - but it might be good enough for this specific use case.

Another use case would be to show a dashboard on a set of monitors in the office.

*January 2021 note: Given how rarely people are in offices nowadays and the non-existing nature of trade shows, the use cases above might be pretty theoretical during coming months/years...*

