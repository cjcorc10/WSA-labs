# Brute Forcing a Stay Logged in Cookie

> This lab allows users to stay logged in even after they close their browser session. The cookie used to provide this functionality is vulnerable to brute-forcing. 

We are given valid user credentials for user wiener and are tasked with brute-forcing a cookie value for the user carlos.

> User credentials: wiener:peter
>
> Victims username: carlos

We want to figure out how the cookie value is assigned and try to figure out if its a guessable formula to create the cookie. The first thing to do is sign in as wiener, so that we can view the cookie that the web server distributes to his profile.

![image](https://user-images.githubusercontent.com/79766677/199610146-88df05c4-f749-43dd-8e83-2c0ddccfa6e5.png)

After signing in the web server responds with the cookie values

![image](https://user-images.githubusercontent.com/79766677/199610319-12624968-84c8-4471-b610-88dc9c048f48.png)

