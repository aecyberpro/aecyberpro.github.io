---
layout: post
title: Going the Extra Mile - BXXS Edition
description: >
  As a pentester, you're going to be going up against external or web app scopes that have been beat up by pentesters and vulnerability scanners. This is about using Blind Cross-Site Scripting (BXSS) in going the extra mile.
sitemap: true
---

This morning I was thinking back to a pentest I did on a credit union's customer-facing website some years ago. Their website had been tested yearly and I wasn't expecting to find much there. About three days into a five day test, all I had to show for findings was low severity stuff. I had been trying to pop Cross-Site Scripting (XSS) and some of my payloads were getting reflected but not popping an alert. I put in a lot of time trying to escape the context surrounding my payload, without success.

I took a step back and thought about what I could do that nobody had thought of before. I wondered if the business had a backend system where the developers and system administrators could view my profile and transactions from the frontend. I reached out to my contact and asked for a call. On this call I asked them to take a look at my profile and transactions through any backend systems. It turned out that the XSS payloads that weren't firing on the frontend were triggered when someone viewed them on the backend. During the call I spun up the BeEF XSS framework and captured screenshots of my payload hooking the admin when my profile and transactions were viewed. 

Another place where blind XSS can help you go the extra mile and impress customers is when testing thick apps. I spent some time working in pentesting Critical National Infrastructure (CNI), such as oil, gas, electric, and defense companies. I frequently was tasked with pentesting thick apps. I always asked if there was a backend web app that was integrated with the thick app, and usually there was. As part of my testing I'd insert BXSS payloads in the thick app and then view it through the backend web app or ask my client contact to look into it. Frequently it would result in finding bling XSS.

I no longer have to ask the client point of contact to check for my XSS popups or use the BeEF framework when testing for blind XSS. There's a better and easier way. Now I use [xsshunter-express](https://github.com/mandatoryprogrammer/xsshunter-express).

While you're thinking about WHERE to put your blind XSS payload, consider where your payload may get executed on backend systems. Some places are your user agent, your profile name, the typical 2nd street address form field. When you submit your payloads, add something to the path which will identify where you submitted it. For example, append `/domainname-searchbar` to the payload URL. Don't include any slashes in this extra tag. Xsshunter-express will serve the payload regardless of what you append to it and in the report it will show this URL which helps you to identify where you inserted the payload that successfully fired.

Next, think about HOW it's going to get executed. Customer support is the first thing that comes to mind, such as if the website has a CS chat system. Another is contacting technical support through email or phone call. Don't lie to them or phish them. Simply ask about how to use some feature because you don't understand what to do. Then watch your xsshunter-express dashboard.

Here's an example of what you'll see when you get an alert in the xsshunter-express console.

![xsshunter-express example 1](/assets/img/blog/xsshunter-express-1.png)

![xsshunter-express example 1](/assets/img/blog/xsshunter-express-2.png)

![xsshunter-express example 1](/assets/img/blog/xsshunter-express-3.png)

![xsshunter-express example 1](/assets/img/blog/xsshunter-express-4.png)

Note that in addition to the screenshot of the victim's browser, you also will capture any non http-only cookies.

In xsshunter-express you can also add additional JavaScript payloads to execute as well as insert the relative path of any pages where you want to collect data from the victim.

In closing, I want to give a shout-out to the [Critical Thinking](https://www.criticalthinkingpodcast.io/) podcast. I listen to them in the morning during my workouts. Check them out for some really great content about hacking web apps. I got the inspiration for this post while listening to the episode where they interviewed Nahamsec and he talked about blind XSS and I thought about how I'd been testing for this bug for years.

Thanks for reading and happy hacking.