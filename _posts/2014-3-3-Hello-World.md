---
layout: post
title: User Error &#35;1 - The Wrong Redirect
---

"Caress the detail, the divine detail" - Vladimir Nabokov

I found myself spinning on a problem recently trying to authenticate a Xamarin Forms app I am building. I should have heeded Mr. Nabakov's wise words because, as you will find out and as is usually the case, this was user error on my behalf. 

# The Situation #
I am building an app that uses Azure Mobile App services on the backend to store data in a table. Up until now, while playing around on my dev box, I've been storing everything anonymously. Up to this point there was no way,and no need for me, to map data back to a particular user. In early stages of my prototyping I ignore goals such as world domination and test out ideas as simply as possible. That is a rapid phase of my work and I quickly moved on to actually considering that more than one person may indeed use this gem of an app some day. Long store short, to make sure users only get access to their own data, the data needs to be identifiable per user. I therefore want some form of user identity and this is where authentication comes in. 
![Authentication dialog disappearing](/images/auth-dialog-disappears.gif)


# Last Word #
It turns out there is no godo substitute than reading the instructions. 
