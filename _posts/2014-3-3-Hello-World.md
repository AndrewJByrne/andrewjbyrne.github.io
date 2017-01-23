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

<p align="center">
<img  src="/images/auth-dialog-disappears.gif" alt="Authentication dialog disappearing"/>
</p>

The preceding animation illustrates that I was close to seeing a dialog, but not close enough. When I tap the **login** button in my app an authentication dialog appears but then disappears abruptly before we see all the information and before having a chance to interact with it. :confused:

You can also check whether authentication is working in your app by navigating to a url that looks like this ```http://myapp.azurewebsites.net/microsoftaccount```. Doing so for my app revealed the following sad news:

<p align="center">
<img src="/images/bp-1/browser-error.png" alt="Browser error"/>
</p>

(Just noticed how many tabs I have open in that browser. :astonished:)
The error message is very misleading, giving me the impression that my account was experiencing issues. Given that I was able to read email, browse to OneDrive and do so much more at the time, I knew this was a kind of red herring.  My usual next step in a situation like this is to debug but for some reason I was compelled to go back and read the instructions I had followed to get to this point. I was glad I did. 

Step 6 of the instructions in [How to configure your App Service application to use Microsoft Account login](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication) contains a very important note as shown in the following snip from those pages:

<p align="center">
<img src="/images/bp-1/redirect-note.png" alt="Browser error"/>
</p>

Going back to my app registration I noticed that my redirect Uri was defined as follows:
<p align="center">
<img src="/images/bp-1/wrong-uri.png" alt="Browser error"/>
</p>

As you can see, I did not append ```/.auth/login/microsoftaccount/callback``` to the end of my Uri. So, I went ahead and remedied that as follows:

<p align="center">
<img src="/images/bp-1/right-uri.png" alt="Browser error"/>
</p>

Now when I try to login through the browser I get the following wonderful message.

<p align="center">
<img src="/images/bp-1/browser-success.png" alt="Browser error"/>
</p>

The ultimate test of course is to run my app again and try to login. Drum roll please ....

<p align="center">
<img  src="/images/auth-diag-working.gif" alt="Authentication dialog working"/>
</p>

# Last Word #
It turns out there is no good substitute for reading the instructions. 

# References #
[How to configure your App Service application to use Microsoft Account login](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication)
