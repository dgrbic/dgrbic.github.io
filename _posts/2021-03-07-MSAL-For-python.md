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

Let me explain what the MSAL is. This acronym stands for [Microsoft Authentication Library]("https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-overview").
Basically, it is a wrapper around OAuth protocol, written by Microsoft with intention to be used with Microsoft services, such as AzureAD and Office365.

The library is available for several platforms and languages, such as .NET Framework, JavaScript, Android etc.

## MSAL Basics

You can refer to the [MSAL documentation]("https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-overview") for instructions how to register your application, which is prerequisite for the successful authentication.

After registration you should have the following information:
```
client ID (in the form of UUID string)
client secret (random looking string)
```
Make sure that you have this information (especially client secret) written somewhere, as 
there is no way to recover the lost key. 

Of course, you can always create the new one.

Also, note the Redirect URI you have configured for your application. You may leave [default value]("https://login.microsoftonline.com/common/oauth2/nativeclient), or change for whatever suits your needs. We will return to this later on.


