---
published: true
comments: true
---
I recently had the pleasure of attending [Steve Borosh's](https://twitter.com/424f424f) "Enterprise Attack Initial Access" course offered through [Antisyphon](https://www.antisyphontraining.com/enterprise-attack-initial-access-w-steve-borosh/). This is my review.

Over the past year I've worked in a role that was more advisory/management than technical and performed only one phishing assessment during that time. While I have a few good phishing tricks up my sleeve, I'm rusty and haven't kept up with the latest techniques. I decided to take this course because I'm preparing to change jobs soon and go back to a more hands-on role and needed to get up to speed quickly. Based on Steve's reputation in the offensive security community, I knew this was going to be a good course.

## Course Description

>This exciting course focuses on using the latest offensive attack methodology against an enterprise spanning cloud and on-premise targets. Beginning from an unprivileged external adversary, students will use cutting-edge techniques to gain access. Students will learn various password spraying techniques to access target services while attempting to avoid detection. Students will build infrastructure to host various payloads using unique services to bypass common proxy configurations and network restrictions. Students will also utilize Command and Control frameworks and payloads for compromising target hosts with both common and obscure communications channels for implants. Students will simulate gaining entry to an enterprise through various ingress channels using novel techniques. Students will utilize Elastic EDR to exercise offensive payloads.

>The course is a beginning to intermediate level course designed to introduce new topics and techniques to both offensive security newcomers and professionals alike. The course is structured to walk students through the different phases of an attack against an enterprise with a hybrid cloud and on-premise environment. The labs are simple in design and are created for students to reproduce in the environment they’ve created during the course.

## Day 1

Day one was mostly introductory, with labs on setting up initial local virtual machines and cloud infrastructure and concluded with Recon of your target. I've been in pentesting for over 5 years and I learned some new recon techniques here.

* Introduction & Course Overview - This module will cover the course overview and introduction to the learning objectives.

* Labs - This module covers the lab environment overview and requirements. Students will host their own labs throughout the course. This allows students to recreate the scenarios after the course.

* Intrastructure - This module covers an overview of various offensive infrastructures implementations and how to build them.

* Recon - Students will learn to enumerate details about a target organization to build layered offensive scenarios. Microsoft 365 will be the target of choice for enumerating details about hybrid or cloud based enterprises.

## Day 2

* Password spraying - Students will utilize various methods for spraying passwords against target services and attempt to avoid detection. Students view first hand how various methods affect sign-in logs that may tip off defenders.

* Command and Control - 
Students will learn how to strategically design their infrastructure tailored for the target enterprise. Students may bring their own C2 or follow along as the course will be utilizing the open source Sliver project. (We used Mythic C2 instead of Sliver in the class I attended) Additionally, this module will cover how to setup Azure C2 redirection and C2 hardening.

* Payload Generation - The “How do I bypass X product?” question is arguably the most often posed question in infosec discord channels. There is no one bypass to rule them all. Students learn to use a combination of open source tools and custom code compilations to create effective payloads that can auto-deploy persistence as well.

  * This section didn't dive deep into manually modifying payloads. It included how to bypass "Mark of the Web" on downloaded files so that your victim doesn't get that scary warning when they execute your payload. It also included using open source and free tools to use to encrypt and wrap your payloads to avoid detection by Antivirus. This course didn't cover manual malware modification techniques but that is a deep subject worthy of it's own course.

## Day 3

* Payload Delivery - With several code execution payloads prepared and ready for deployment, students will learn to host them on reputable domain services and even proxy payloads direct from their own VM to the target’s internal network.

* Setup Elastic EDR Agent - Students will install an EDR agent on their Windows VM for testing.

## Day 4

* Phishing - Students will setup infrastructure and templates for phishing campaigns. Students will learn how to abuse the target enterprise’s own email protections against them. With minimal effort, students will be spoofing emails and payloads into the target enterprise. This module will also focus heavily on phishing with Microsoft Device Codes. Students will learn how to use legitimate Microsoft.com websites to harvest sessions for them and stay persistent in their cloud environment.

* Using Microsoft 365 - Students will use Azure and Microsoft 365 attack scenarios to abuse access to Microsoft 365 for profit.

## Conclusion

This course is a steal at only $545! You get 16 hours of instruction from Steve Borosh, a "playbook" (Steve's step-by-step Notion notebook on phishing tactics), course slides, six months of access to the class recordings (very useful if you can't watch the stream in real time), certificate of participation (useful for SANS CEU's), and six months of access to the Antisyphon Cyber Range. Access to Steve's tips, tricks, and wisdom on conducting phishing assessments was priceless!

Edited to add: Technology and tools related to the cloud and phishing changes frequently and Steve did a great job of troubleshooting and fixing any issues that popped up during the course while managing to keep it on track and on schedule.

BTW, [Antisyphon](https://www.antisyphontraining.com/) offers a lot of great courses that are very affordable (compared to SANS and OffSec), so I recommend that you go check them out. [Ralph May's](https://twitter.com/ralphte1) "[HackerOps](https://www.antisyphontraining.com/hackerops-w-ralph-may/)" course on Offensive DevOps complements the "Enterprise Attack Initial Access" course really well. The infrastructure setup steps are a little time-consuming and very similar from one assessment to another, so this process could be automated with DevOps processes learned from Ralph's course.
