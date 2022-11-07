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
      > adds 1 gift card to cart
  POST /cart/coupon
      > applys coupon code for 30% off
  POST /cart/checkout 
      > purchases the gift card
  GET /cart/order-confirmation?oder-confirmed=true
      > confirms order of gift card
  POST /gift-card
      > redeems code to account
