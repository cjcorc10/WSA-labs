# Password Reset Poisioning via Middleware

> This lab is vulnerable to password reset poisoning. The user carlos will carelessly click on any links in emails that he receives. To solve the lab, log in to Carlos's account. You can log in to your own account using the following credentials: wiener:peter. Any emails sent to this account can be read via the email client on the exploit server. 

I have read about this type of vulnerability here:


> https://www.skeletonscribe.net/2013/05/practical-http-host-header-attacks.html


In this article James Kettle describes explains how some web applications dynamically generate their password reset URL's based on the HOST header in the reset request. If the changing the host header is forbidden then we can try using the X-Forwarded-Host header, which will override the HOST header.


I am going to run through the process of resetting a password for wiener to observe the behaviour of the server during a standard password reset.


When requesting a reset password link, the user supplies a parameter with their username in the POST request.


![image](https://user-images.githubusercontent.com/79766677/199639542-85cda09a-2b79-48fc-a47d-f197214d5a5c.png)


This is the request that triggers the generation of the reset URL. Chaning the Host header results in a 403 forbidden response, but the server does allow the X-Forwarded-Host header. Adding our controlled domain to that header causes the application to generate the reset URL with our domain. We also need to change the parameter to carlos.

![image](https://user-images.githubusercontent.com/79766677/199640021-80fa3b56-5f33-42ac-9c21-ff878ab489f4.png)


Now looking back in our logs of our domain, we see that carlos clicked on the reset link and we now have his generated token:

![image](https://user-images.githubusercontent.com/79766677/199640349-8a92f7f9-41ab-45c1-baa7-f4a1e6c4c9d4.png)


All thats left to do now is to go to the reset page with Carlos' token and reset his password.

![image](https://user-images.githubusercontent.com/79766677/199640721-ed6d1509-595a-4f61-8946-2a0cb21a1821.png)
