---
layout: post
title: User Error &#35;1 - The Wrong Redirect (a.k.a The Case of the Disappearing Auth Dialog)
---

"Caress the detail, the divine detail" - Vladimir Nabokov

I found myself spinning on a problem recently trying to authenticate a Xamarin Forms app that I'm building. I should have heeded Mr. Nabakov's wise words because, as you will find out and as is frequently the case, this was user error on my behalf. 

# The Situation #
My app uses Azure Mobile App services on the backend to store data in a table. Up until now, while playing around on my dev box, I've been storing everything anonymously. By storing records without a user identity at this point, I'm not mapping data back to a particular user. In the early stages of my prototyping I ignore goals such as world domination and instead test out ideas as simply as possible. That is the rapid phase of my work and I quickly moved on to actually considering that more than one person may indeed use this gem of an app some day. Long story short, I wanted to introduce some form of user identity to make sure users only get access to their own data. This is where user authentication comes in. 

In Azure Mobile App Services, you can authenticate using a myriad of supported providers including Azure Active Directory, Facebook, Google and Twitter. For this app, I chose Microsoft Account as the authentication provider. This means that anyone with a Microsoft account can log into my app and the app will have a unique identifier for that authenticated user. The Azure documentation does a good job walking you through setting up authentication. I followed the instructions given in the article [How to configure your App Service application to use Microsoft Account login](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication). 

With auth now configured on both the backend and my client, I took my app for a spin. My expectation, was that the user would be presented with a dialog to sign in using their Microsoft Account. Alas, this happened:

<p align="center">
<img  src="/images/auth-dialog-disappears.gif" alt="Authentication dialog disappearing"/>
</p>

The preceding animation illustrates that I was close to seeing a dialog, but not close enough. When I tap the **login** button in my app an authentication dialog appears briefly but then disappears abruptly before having a chance to interact with it. :confused:

You can also check whether authentication is working in your app by navigating to a url that looks like this ```http://myapp.azurewebsites.net/microsoftaccount``` where ```myapp``` is the name of your app. Doing so for my app revealed the following sad news:

<p align="center">
<img src="/images/bp-1/browser-error.png" width="60%" height="60%" alt="Browser error"/>
</p>

(Just noticed how many tabs I have open in that browser. :astonished:)
The error message is very misleading, giving me the impression that my account was experiencing issues. Given that I was able to read email, browse to OneDrive and do so much more at the time, I knew this was a red herring.  My usual next step in a situation like this is to start debugging but, for some reason, I was compelled to go back and read the instructions I had followed to get to this point. I was glad I did. 

Step 6 of the instructions in [How to configure your App Service application to use Microsoft Account login](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication) contains a very important note that I've copied here for you to read:

<p align="center">
<img src="/images/bp-1/redirect-note.png" width="60%" height="60%" alt="Browser error"/>
</p>

It is very clear about the format of redirect Uri that is expected. 
Going back to my app registration I noticed that my redirect Uri was defined as follows:
<p align="center">
<img src="/images/bp-1/wrong-uri.png" width="60%" height="60%" alt="Browser error"/>
</p>

As you can see, I did not append ```/.auth/login/microsoftaccount/callback``` to the end of my Uri. So, I went ahead and remedied that as follows:

<p align="center">
<img src="/images/bp-1/right-uri.png" width="60%" height="60%" alt="Browser error"/>
</p>
That looks much more like the Uri guidance from the previous note. 

Now when I try to login through the browser I get the following wonderful message.

<p align="center">
<img src="/images/bp-1/browser-success-2.png" width="60%" height="60%" alt="Browser error"/>
</p>

The ultimate test of course is to run my app again and try to login. Drum roll please ....

<p align="center">
<img  src="/images/auth-diag-working.gif" width="50%" height="50%" alt="Authentication dialog working"/>
</p>

Success - a complete authentication dialog that will accept my Microsoft Account credentials, ask for my consent and then log me in. Once I have signed in successfully, I have a unique user Id that I can then use as a key in my table of data to identify data as belonging to a partiular user. 

# Last Word #
It turns out there is no good substitute for reading the instructions. I could have probably just tweeted out my mistake instead of writing this post, but I do have my reasons for telling you this story. Yes, I could have avoided all of this by being more attentive but a common issue we all face is that error messages and instructions can dig holes deeper for us. I'm a software engineer and content developer by trade, so I empathize when these experiences don't tie together seamlessly. 

# References #
[How to configure your App Service application to use Microsoft Account login](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication)
