---
layout: post
title: User Error &#35;1 - The Wrong Redirect (a.k.a The Case of the Disappearing Auth Dialog)
---

"Caress the detail, the divine detail" - Vladimir Nabokov

I found myself spinning on a problem recently trying to authenticate a Xamarin Forms app I am building. I should have heeded Mr. Nabakov's wise words because, as you will find out and as is frequently the case, this was user error on my behalf. 

# The Situation #
My app uses Azure Mobile App services on the backend to store data in a table. Up until now, while playing around on my dev box, I've been storing everything anonymously. By storing records without a user identity at this point, I'm obviously not mapping data back to a particular user. In the early stages of my prototyping I ignore goals such as world domination and test out ideas as simply as possible. That is the rapid phase of my work and I quickly moved on to actually considering that more than one person may indeed use this gem of an app some day. Long story short, I want to introduce some form of user identity to make sure users only get access to their own data. This is where user authentication comes in. 

In Azure App App Services, you can authenticate using a myriad of supported providers including Azure Active Directory, Facebook, Google and Twitter. For this app, I chose Microsoft Account as the authentication provider. This means that anyone with a Microsoft account can log into my app from a device and the app will have a unique identifier for that authenticated user. The Azure documentation walks you through setting up authentication and I followed the instructions given in the article [How to configure your App Service application to use Microsoft Account login](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication). 

With auth setup on both the backend and my client, I took my app for a spin. My expectation, was that the user would be presented with a dialog to sign in using their Microsoft Account. Alas, this happened:

![Authentication dialog disappearing](/images/auth-dialog-disappears.gif)

The preceding animation illustrates that I was close to seeing a dialog, but not close enough. As you can see, an authentication dialog appears but then disappears abruptly before we see all the information and before having a chance to interact with it. 


[With auth setup on both the backend and client side, I took my app for a spin]

[Expectation]

[Actuals]



[Web page image too]

# The solution - get your redirect URI right! #

[ Show article note ]

[Show what I changed]

[Show result]




# Last Word #
It turns out there is no good substitute for reading the instructions. 

# References #
[How to configure your App Service application to use Microsoft Account login](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication)
