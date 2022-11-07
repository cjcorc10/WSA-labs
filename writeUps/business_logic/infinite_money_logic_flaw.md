# Infinite Money Logic Flaw

>  This lab has a logic flaw in its purchasing workflow. To solve the lab, exploit this flaw to buy a "Lightweight l33t leather jacket".
>  You can log in to your own account using the following credentials: wiener:peter 

In this lab we are required to purchase a $1337 jacket and start with only $100 in store credit. Immediately after signing in we see a gift card field allowing users to redeem gift card codes for store credit. 

![image](https://user-images.githubusercontent.com/79766677/200416956-00e91e2d-cbbd-460e-a7d0-b9746161b9cc.png)

There are also gift cards available at the home screen for purchase:

![image](https://user-images.githubusercontent.com/79766677/200417155-2b3d9ad4-820c-4505-9667-c8b9f34b7562.png)

Also on the home screen of the application is a newsletter signup that gives you a coupon code for 30% off of purchases after signing up. 

I am going to pruchase a gift card with the coupon code to observe the behaviour of the application.

![image](https://user-images.githubusercontent.com/79766677/200417886-77dd6208-82c2-4658-8b65-0fa62ad7b9b3.png)

After purchasing a gift card we are given a coupon code which redeems for $10. The flaw in this application is that we can keep on using the same coupon code for repeated purchases.

burp capture of response:
![image](https://user-images.githubusercontent.com/79766677/200418424-bd6e2b96-5fa3-4f66-8ca7-b26f7dde9776.png)


Since we can use this promotion however much we want, this allows us to purchase the gift cards for $3 less than their redeemable value repeatedly until we accrue enough store credit to purchase the coveted l33t jacket.

To repeat this process we need to use burp's macros so that we can dynamically redeem the new gift card code after purchasing.

Burp macros can be created in Project Options > Macros. We want to capture the following requests made for the macro to work:

  POST /cart
  
       adds 1 gift card to cart
  POST /cart/coupon
      
       applys coupon code for 30% off
  POST /cart/checkout 
      
       purchases the gift card
  GET /cart/order-confirmation?oder-confirmed=true
      
       confirms order of gift card
  POST /gift-card
      
       redeems code to account

![image](https://user-images.githubusercontent.com/79766677/200419340-10bf38bb-f82d-4e8c-8e2c-c717288943eb.png)

After selecting the requests in the right order we need to make the macro dynamic by capturing the response to the confirmation request. This response holds the gift card redeemable code. We will use this in the last POST request to redeem the code.

To capture this value we use "Configure Item" in the confirmation request and select the generated gift card code.

![image](https://user-images.githubusercontent.com/79766677/200419983-1d29a2f1-08ab-4c4a-b535-51540b22da05.png)

After capturing the gift card code we need to make sure the POST request that redeems the gift card uses the newly purchased gift card value. To do this we use "Configure Item" on the request and select the parameter to be derived from prior request:

![image](https://user-images.githubusercontent.com/79766677/200420249-b68f85ac-cd2c-439e-a1d2-feb056a29035.png)

To run our newly created macro we need to create a Session handling rule for when it is run. We will select our rule action to be our macro and the scope to be ran on all URL's.

![image](https://user-images.githubusercontent.com/79766677/200420486-99cec216-7a80-4f0e-a640-a10255e2bff1.png)

![image](https://user-images.githubusercontent.com/79766677/200420517-3be89765-b70a-477a-b73a-b1353e64e751.png)

Our macro can now be run with all our Burp Tools. All that is left to do is use intruder to issue a request to the site with a NULL payload. It's important to limit the resource pool to 1 thread otherwise we could end up with too many gift cards in our cart and not enough money to purchase them. I just used a GET /my-account request from ealier and I've generated 400 payloads, so that after they are comlplete I should have enough funds to purchase the jacket.

![image](https://user-images.githubusercontent.com/79766677/200421916-8518bd0c-20f9-4e35-b1a6-3bc1eee5970c.png)

