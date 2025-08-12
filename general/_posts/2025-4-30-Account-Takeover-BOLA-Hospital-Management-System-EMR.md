---
layout: post
title: Account Takeover in Danphe Health Hospital Management System EMR version 3.2
description: >
  Any authenticated user can takeover other accounts, including the admin account, due to Broken Function Level Authorization on the /api/SecuritySettings/ResetPassword endpoint.
sitemap: true
---

According to [Danphe Health](https://opensource-emr.github.io/hospital-management-emr/), *"Danphe EMR is a enterprise web-based application which covers all day to day aspects of Hospital management end to end. Its currently live 50 plus hospitals in Asia(India,Nepal and Bangladesh)"*.

The `/api/SecuritySettings/ResetPassword` route seems to be intended for administrators to reset user passwords since the function is found in the web interface under "Settings" -> "Security" and doesn't require knowledge of the user's existing password. This vulnerability allows any authenticated user, regardless of role, to reset the password and takeover the account of any user, including the site admin. Considering the purpose of the application, exploitation of this vulnerability may result in exposure of Protected Health Information (PHI).

Secure code review by the researcher discovered that the ResetPassword route inherits basic authentication functionality from the CommonController base class, which has the `[DanpheDataFilter()]` attribute. This verifies the user is logged in (has a valid JWT token). It doesn't verify the user has admin rights or can only modify their own password. The affected software version is 3.2. The vulnerability was patched in build 3.11.11.

## Steps to recreate the vulnerability

1. Proxy the browser through Burp Suite
2. Login as admin
2. Navigate to Settings -> Security, click on the `ResetPassword` button to the right of any user
3. Change any user's password
4. In Burp Suite, find the PUT request to endpoint `/api/SecuritySettings/ResetPassword` and send it to the Repeater tool. Example request:m![Example PUT request](/assets/img/blog/DanphePUT.png)
5. Logout and login as any non-admin user
6. Copy the Authorization: Bearer token from the current user session
7. Revisit the request sent to the Repeater tool
8. Replace the Bearer token with the non-admin user's token
9. Submit the request and notice the 200 response
10. Login with the username and password of the account that was reset

## Timeline

3/28/2025: I reported the vulnerability to the developer

3/29/2025: The developer acknowledged the vulnerability.

4/30/2025: No further communication has been received from the developer. I published the vulnerability.

4/30/2025: Applied to MITRE for a CVE

6/9/2025: Developer reported remediation in build 3.11.11

8/12/2025: MITRE assigned CVE-2025-50594