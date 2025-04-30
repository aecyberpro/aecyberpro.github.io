---
layout: post
title: Account Takeover in Danphe Health Hospital Management System EMR
description: >
  Any authenticated user can takeover other accounts, including the admin account, due to Broken Object Level Authorization on the /api/SecuritySettings/ResetPassword endpoint.
sitemap: true
---

According to [Danphe Health](https://opensource-emr.github.io/hospital-management-emr/), *"Danphe EMR is a enterprise web-based application which covers all day to day aspects of Hospital management end to end. Its currently live 50 plus hospitals in Asia(India,Nepal and Bangladesh)"*.

The `/api/SecuritySettings/ResetPassword` route seems to be intended for administrators to reset user passwords since the function is found in the web interface under "Settings" -> "Security" and doesn't require knowledge of the user's existing password. This vulnerability allows any authenticated user, regardless of role, to reset the password and takeover the account of any user, including the site admin. Considering the purpose of the application, exploitation of this vulnerability may result in exposure of Protected Health Information (PHI).

Secure code review by the researcher discovered that the ResetPassword route inherits basic authentication functionality from the CommonController base class, which has the `[DanpheDataFilter()]` attribute. This verifies the user is logged in (has a valid JWT token). It doesn't verify the user has admin rights or can only modify their own password.

## Steps to recreate the vulnerability

1. Proxy the browser through Burp Suite
2. Login as admin
2. Navigate to Settings -> Security, click on the `ResetPassword` button to the right of any user
3. Change any user's password.
4. In Burp Suite, find the PUT request to endpoint `/api/SecuritySettings/ResetPassword` and send it to the Repeater tool
5. Logout and login as any non-admin user.
6. Copy the Authorization: Bearer token from the current user session
7. Revisit the request sent to the Repeater tool
8. Replace the Bearer token with the non-admin user's token
9. Submit the request and notice the 200 response
10. Login with the user and password for the account who's password you reset

## Timeline

3/28/2025: Reported the vulnerability to the developer

4/30/2025: No further communication was received from the developer

4/30/2025: Applied to Mitre for a CVE