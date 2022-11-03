# Password Reset Poisioning via Middleware

> This lab is vulnerable to password reset poisoning. The user carlos will carelessly click on any links in emails that he receives. To solve the lab, log in to Carlos's account. You can log in to your own account using the following credentials: wiener:peter. Any emails sent to this account can be read via the email client on the exploit server. 

I have read about this type of vulnerability here:


> https://www.skeletonscribe.net/2013/05/practical-http-host-header-attacks.html


In this article James Kettle describes explains how some web applications dynamically generate their password reset URL's based on the HOST header in the reset request. If the changing the host header is forbidden then we can try using the X-Forwarded-From header, which will override the HOST header.


I am going to run through the process of resetting a password for wiener to observe the behaviour of the server during a standard password reset.


When requesting a reset password link, the user supplies a parameter with their username in the POST request.


image here



