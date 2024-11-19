# OWASP_Juice_Shop

This room uses the Juice Shop vulnerable web application to learn how to identify and exploit common web application vulnerabilities.

![juice shop](/juiceshop.png)

## Open for Business

Within this room, we will look at **OWASP's TOP 10 vulnerabilities** in web applications. You will find these in all types of web applications. But for today we will be looking at OWASP's own creation, Juice Shop!


The FREE Burpsuite rooms **Burpsuite Basics**  and **Burpsuite Repeater**  are recommended before completing this room!
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

**Question #1: What's the Administrator's email address?**

![This is the shop](/owaspadventure.png)

The reviews show each user's email address. Which, by clicking on the Apple Juice product, shows us the Admin email!

![admin email](/owaspadmin.png)

    admin@juice-sh.op

**Question #2: What parameter is used for searching?**

Click on the magnifying glass in the top right of the application will pop out a search bar.

We can then input some text and by pressing Enter will search for the text which was just inputted.

Now pay attention to the URL which will now update with the text we just entered.

![search parameter](/q_parameter.png)

        q

**Question #3: What show does Jim reference in his review?**

Jim did a review on the Green Smoothie product. We can see that he mentions a replicator.

![Jim reference](/replicator.png)

If we google "replicator" we will get the results indicating that it is from a TV show called Star Trek

![Jim reference](/replicator1.png)

    Star Trek 

## Inject the juice

![login form](/login_form.png)

This task will be focusing on injection vulnerabilities. Injection vulnerabilities are quite dangerous to a company as they can potentially cause downtime and/or loss of data. Identifying injection points within a web application is usually quite simple, as most of them will return an error. There are many types of injection attacks, some of them are:

**SQL Injection**

SQL Injection is when an attacker enters a malicious or malformed query to either retrieve or tamper data from a database. And in some cases, log into accounts.

**Command Injection**

Command Injection is when web applications take input or user-controlled data and run them as system commands. An attacker may tamper with this data to execute their own system commands. This can be seen in applications that perform misconfigured ping tests.

**Email Injection**

Email injection is a security vulnerability that allows malicious users to send email messages without prior authorization by the email server. These occur when the attacker adds extra data to fields, which are not interpreted by the server correctly.

**Question #1: Log into the administrator account!**

After we navigate to the login page, enter some data into the email and password fields.

![Log into the administrator account](/intercept.png)

 Before clicking **submit**, make sure **Intercept mode** is on.

This will allow us to see the data been sent to the server!

![password input intercept](/intercept1.png)

We will now change the "a" next to the email to: **' or 1=1--** and forward it to the server.

![sql injection success](/intercept_injection.png)

__Why does this work?__

The character **'** will close the brackets in the SQL query
**OR** in a SQL statement will return true if either side of it is true. As **1=1** is always true, the whole statement is true. Thus it will tell the server that the email is valid, and log us into user id 0, which happens to be the administrator account.

The **'--** character is used in SQL to comment out data, any restrictions on the login will no longer work as they are interpreted as a comment. This is like the # and // comment in python and javascript respectively.

![Admin Login bypass](/admin_shop.png)


    32a5e0f21372bcc1000a6088b93b458e41f0e02a

**Question #2: Log into the Bender account!**

Similar to what we did in Question #1, we will now log into Bender's account! Capture the login request again, but this time we will put: **bender@juice-sh.op'--** as the email.

![bander account](/bander.png)

Now, forward that to the server!

But why don't we put the **1=1**?

Well, as the email address is valid (which will return true), we do not need to force it to be true. Thus we are able to use **'--** to bypass the login system. Note the **1=1** can be used when the email or username is not known or invalid.

![bander account](/bander_account.png)

    fb364762a3c102b2db932069c0e6b78e738d4066