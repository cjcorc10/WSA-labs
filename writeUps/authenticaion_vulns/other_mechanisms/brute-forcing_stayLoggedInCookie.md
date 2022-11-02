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

If we make a request to fetch our account we can see our cookies include:

![image](https://user-images.githubusercontent.com/79766677/199612225-b8e571c9-c89e-4632-9313-821487795a04.png)

The stay-logged-in cookie is in ascii, so I suspect it could be base64 encoded and after decoding it the username is revealed with what may be the password hashed:

![image](https://user-images.githubusercontent.com/79766677/199610816-6145af7a-02e1-4c31-bcd7-e5a75d5311e1.png)

After testing various hashing algorithms I discovered that MD5 was used to hash the password, so we know how the cookie is crafted and can work on brute-forcing carlos' password.

Before attempting the brute-force I want to try and brute-force wieners password to observe how the server responds. PortSwigger provided a password list to use for brute-forcing, so I am using that with Burp Intruder. We need to define rules for the payload to be processed in reverse order of how we decrypted the cookie.

![image](https://user-images.githubusercontent.com/79766677/199611411-049354dd-1f83-4e1e-a1ce-335524ba3ae7.png)

From the results we can determine that both the status code and length will differ if we have the correct cookie:

![image](https://user-images.githubusercontent.com/79766677/199611783-a23d23ff-d194-4c45-b52d-f3e76e855cb6.png)

To brute-force for Carlos we only need to change the requested id parameter from wiener to carlos and the prefix on the cookie. 

Our attack is successful and we can now sign in as Carlos

![image](https://user-images.githubusercontent.com/79766677/199612534-18a265c1-8b7d-4d2f-a0f7-ec50db9b3c21.png)

