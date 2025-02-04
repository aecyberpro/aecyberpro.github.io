---
layout: post
title: How Spammers and Scammers Hide The External Email Banner
description: >
  This article explains what I learned while discovering how an email spammer hid the "External Email" enterprise banner.
sitemap: true
---

I recently noticed that some spam emails that landed in my inbox were missing the "EXTERNAL EMAIL" banners. They didn't appear to be malicious. They simply were trying to get me to sign up for their webinar, or so it seemed from the email content. I deleted them before realizing that I hadn't seen the usual external email banner. This piqued my interest and I decided to do an analysis to find how they removed the banner.

After some searching, I found this code that hides the banner:

```html
<!--[if gte mso 9]>
<v:background xmlns:v="urn:schemas-microsoft-com:vml" fill="t">
    <v:fill type="tile" size="100%,100%" color="#ffffff"/>
</v:background>
<![endif]-->
<div class="hse-body-background" lang="en" style="background-color:#eaf0f6" bgcolor="#eaf0f6">
```

Let's break this down:

1. `<!--[if gte mso 9]>` 
   - This is a conditional comment that targets Microsoft Outlook versions 9 and above
   - Only Microsoft email clients will process this code

2. `<v:background xmlns:v="urn:schemas-microsoft-com:vml" fill="t">`
   - Declares a VML (Vector Markup Language) background element
   - `xmlns:v` defines the VML namespace
   - `fill="t"` tells it to fill the entire space

3. `<v:fill type="tile" size="100%,100%" color="#ffffff"/>`
   - Sets up how the background should be filled
   - `type="tile"` makes it repeat if needed
   - `size="100%,100%"` makes it cover the full width and height
   - `color="#ffffff"` sets it to white, which covers up the banner

4. `<div class="hse-body-background" lang="en" style="background-color:#eaf0f6" bgcolor="#eaf0f6">`
   - This div provides a fallback background for non-Outlook clients
   - Uses both style attribute and bgcolor for maximum compatibility

The VML code creates a white overlay that sits on top of everything else in the email, including any injected banner. Because it uses VML, which is specifically supported in Microsoft Outlook (where many corporate users read their email), it's particularly effective at hiding the banner.

This technique works because:
1. Most banner injection systems add the banner within the email body
2. VML rendering in Outlook places this background on top of other content
3. The white color makes it appear seamless with the rest of the email

This sophisticated technique specifically targets enterprise email environments where Outlook is common and external email banners are used.