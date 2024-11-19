# OWASP_Juice_Shop

This room uses the Juice Shop vulnerable web application to learn how to identify and exploit common web application vulnerabilities.

![juice shop](/juiceshop.png)

## Open for Business

Within this room, we will look at OWASP's TOP 10 vulnerabilities in web applications. You will find these in all types of web applications. But for today we will be looking at OWASP's own creation, Juice Shop!


The FREE Burpsuite rooms 'Burpsuite Basics'  and 'Burpsuite Repeater'  are recommended before completing this room!
~

Juice Shop is a large application so we will not be covering every topic from the top 10.

We will, however, cover the following topics which we recommend you take a look at as you progress through this room.

            <------------------------------------------------->

                                                Injection

                                                Broken Authentication

                                                Sensitive Data Exposure

                                                Broken Access Control

                                                Cross-Site Scripting XSS

## Lets go on an Adventure

Before we get into the actual hacking part, it's good to have a look around. In Burp, set the Intercept mode to off and then browse around the site. This allows Burp to log different requests from the server that may be helpful later.

This is called walking through the application, which is also a form of reconnaissance!

Question #1: What's the Administrator's email address?

![This is the shop](/owaspadventure.png)

The reviews show each user's email address. Which, by clicking on the Apple Juice product, shows us the Admin email!

![admin email](/owaspadmin.png)

    admin@juice-sh.op

Question #2: What parameter is used for searching? 

Click on the magnifying glass in the top right of the application will pop out a search bar.

We can then input some text and by pressing Enter will search for the text which was just inputted.

Now pay attention to the URL which will now update with the text we just entered.

![search parameter](/q_parameter.png)

        q

Question #3: What show does Jim reference in his review? 

Jim did a review on the Green Smoothie product. We can see that he mentions a replicator.

![Jim reference](/replicator.png)

If we google "replicator" we will get the results indicating that it is from a TV show called Star Trek

![Jim reference](/replicator1.png)

    Star Trek 

## Inject the juice

![login form](/login_form.png)