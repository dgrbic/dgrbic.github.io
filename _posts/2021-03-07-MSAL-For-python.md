---
layout: post
title:  "MSAL for Python"
tags: python msal office365
---

Hello, my name is Dragan Grbić, and this is my first post in new blog, named Dev Kitchen.

In this post I wanted to introduce MSAL library for Python, which can be used to authenticate to the Outlook365.

## Rant (or introduction?)

I've started experimenting with MSAL out of frustration with state of Linux email clients in combination with Microsoft Outlook service, which is used in the company I work in.

Be it user interface from last century or lack of functionality (lack of group mail support, for instance), no email client I've tried gave me comparable experience to the Windows Outlook application.

So, my intention was to at least explore the possibility of building modern Outlook alternative for Linux. Of course, I cannot promise that this task will materialize in actual product, as for me this is side project, and I am able to work on it out of working hours, while balancing my private life with it. But at least I will explore some new (for me) technologies and while exploring, I will write about it and share with you.

## And now... let's begin

First thing the email application should do is authentication with the server, without it accessing the user's account, mail messages, calendar and all other content available on Outlook would be impossible.

And this is the point we should start to talk about MSAL, as this library is created to help us  exactly in this kind of scenarios.

Let me explain what the MSAL is. This acronym stands for [Microsoft Authentication Library](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-overview).
Basically, it is a wrapper around OAuth protocol, written by Microsoft with intention to be used with Microsoft services, such as AzureAD and Office365.

The library is available for several platforms and languages, such as .NET Framework, JavaScript, Android etc.

## MSAL Basics

You can refer to the [MSAL documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-overview) for instructions how to register your application, which is prerequisite for the successful authentication.

After registration you should have the following information:
```
client ID (in the form of UUID string)
client secret (random looking string)
```
Make sure that you have this information (especially client secret) written somewhere, as 
there is no way to recover the lost key. 

Of course, you can always create the new one.

Also, note the Redirect URI you have configured for your application. You may leave [default value](https://login.microsoftonline.com/common/oauth2/nativeclient), or change for whatever suits your needs. We will return to this later on.

## Authenticating scenarios

The above mentioned documentation lists a number of possible scenarios for authenticating, however none of them quite fits the needs of user friendly desktop application for accessing Outlook365 services.

Our imagined application isn't Single Page Web app, in fact it is not Web app at all.

Desktop application sounds like something we are looking for, but Python MSAL library does not support the ``PublicClientApp.AcquireTokenInteractive()`` method.

Looking at the Mobile app samples, more or less the same conclusion can be made - Python library does not have the method for interactively obtain the token.

Which is not surprise, as it would require the web browser interaction, which (obviously) isn't included in standard Python library, and support for that would introduce dependency on some third party library, which may not be available on every platform Python (and, of course MSAL) can run.

So what is left? 

MSAL supports two basic types of clients: [public clients and confidential clients](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-client-applications).

Looking at the description of client types, it is obvious that desktop mail client would be a public client, so let's take a dip to the Python implementation of said library.

And `MSAL.Python`, indeed, supports the `PublicClientApplication`, similar to Dot.NET version from Desktop application sample, but as we already noticed, `AcquireTokenInteractive()` is missing.

How can we acquire the token, then? 

Here is the list of methods for acquiring the authentication code:
- `acquire_token_by_device_flow()`
- `acquire_token_by_username_password()`
- `acquire_token_by_username_password_federated()`
- `acquire_token_by_authorization_code()`
- `acquire_token_silent()`
- `acquire_token_silent_with_error()`
- `acquire_token_by_refresh_token()`

From this list, we can (for now) forget about `acquire_token_silent()` (both versions) and `acquire_token_by_refresh_token()`, as they will be discussed later, as they are not intended to obtain the token interactively.

In the next post, I will take a closer look at remaining options, create some code for testing, and (hopefully) we will be able to authenticate and read some data from Outlook365 server.


