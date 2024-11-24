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

***SQL Injection***

SQL Injection is when an attacker enters a malicious or malformed query to either retrieve or tamper data from a database. And in some cases, log into accounts.

***Command Injection***

Command Injection is when web applications take input or user-controlled data and run them as system commands. An attacker may tamper with this data to execute their own system commands. This can be seen in applications that perform misconfigured ping tests.

***Email Injection***

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

## Who broke my lock?

![Weak password authentication](/reset.png)

In this task, we will look at exploiting authentication through different flaws. When talking about flaws within authentication, we include mechanisms that are vulnerable to manipulation. These mechanisms, listed below, are what we will be exploiting.

Weak passwords in high privileged accounts

Forgotten password pages

***Question #1: Bruteforce the Administrator account's password!***

We have used SQL Injection to log into the Administrator account but we still don't know the password. Let's try a brute-force attack! We will once again capture a login request, but instead of sending it through the proxy, we will send it to Intruder.

Go to Positions and then select the ***Clear §*** button. In the password field place two ***§*** inside the quotes. To clarify, the ***§ §*** is not two sperate inputs but rather Burp's implementation of quotations e.g. "". The request should look like the image below.

***![Burp interception](/interceptor_burp.png)***

A failed request will receive a ***401*** Unauthorized

Whereas a successful request will return a 200 OK

![Burp intruder](/intruder.png)

Once completed, login to the account with the password.

    c2110d06dc6f81c67cd8099ff0ba601241f1ac0e

***Question #2: Reset Jim's password!***

Believe it or not, the reset password mechanism can also be exploited! When inputted into the email field in the Forgot Password page, Jim's security question is set to "Your eldest siblings middle name?".

In Task 2, we found that Jim might have something to do with Star Trek. Googling "Jim Star Trek" gives us a wiki page for Jame T. Kirk from Star Trek.
![Jimmy Googling "Star](/jim.png)

Looking through the wiki page we find that he has a brother.
![jim family](/jim_family.png)

Looks like his brother's middle name is **Samuel**

Inputting that into the Forgot Password page allows you to successfully change his password.
You can change it to anything you want!

Password successfully changed

![jims password change](/Password_changed.png)

***flag:***

    094fbc9b48e525150ba97d05b942bbf114987257

## AH! Don't look!

![Dont Look](/Dont_look.png)

A web application should store and transmit sensitive data safely and securely. But in some cases, the developer may not correctly protect their sensitive data, making it vulnerable.

Most of the time, data protection is not applied consistently across the web application making certain pages accessible to the public. Other times information is leaked to the public without the knowledge of the developer, making the web application vulnerable to an attack.

***Question #1: Access the Confidential Document!***

![Boring Staffs](/boring_staffs.png)

Navigate to the About Us page, and hover over the "Check out our terms of use".

![ftp path on About Us](/legal_md.png)

![Visit ftp path](/ftp.png)

We will download the acquisitions.md and save it. It looks like there are other files of interest here as well.

After downloading it, navigate to the home page to receive the flag!

        edf9281222395a1c5fee9b89e32175f1ccf50c5b


***Question #2: Log into MC SafeSearch's account!***

***Click these image below for video recording***
[![Rapper who is very concerned about password search](/interceptor_burp.png)](https://youtu.be/v59CX2DiX0Y)

After watching the video there are certain parts of the song that stand out.

He notes that his password is "Mr. Noodles" but he has replaced some "vowels into zeros", meaning that he just replaced the o's into 0's.

We now know the password to the ***mc.safesearch@juice-sh.op*** account is "***Mr. N00dles***"

    66bdcffad9e698fd534003fbb3cc7e2b7b55d7f0

***Question #3: Download the Backup file!***

We will now go back to the  http://MACHINE_IP/ftp/ folder and try to download package.json.bak. But it seems we are met with a 403 which says that only .md and .pdf files can be downloaded.

![package.json.bak](/express.png)

To get around this, we will use a character bypass called ***Poison Null Byte***. A Poison Null Byte looks like this: **%00**.

Note: as we can download it using the url, we will need to encode this into a url encoded format.

The Poison Null Byte will now look like this: **%2500**. Adding this and then a .md to the end will bypass the **403** error!

![Null Byte Poisoning](/Null_Byte_Poison.png)

A Poison Null Byte is actually a NULL terminator. By placing a NULL character in the string at a certain byte, the string will tell the server to terminate at that point, nulling the rest of the string.

    bfc1e6b4a16579e85e06fee4c36ff8c02fb13795

## Who's flying this thing?

![Who's flying this thing](/profile_edit.png)

Modern-day systems will allow for multiple users to have access to different pages. Administrators most commonly use an administration page to edit, add and remove different elements of a website. You might use these when you are building a website with programs such as Weebly or Wix.

***Horizontal Privilege Escalation***

Occurs when a user can perform an action or access data of another user with the same level of permissions.

***Vertical Privilege Escalation***

Occurs when a user can perform an action or access data of another user with a higher level of permissions.

![Broken Access](/broken_access.png)

***Question #1: Access the administration page!***

First, we are going to open the Debugger on Firefox.

(Or Sources on Chrome.)

This can be done by navigating to it in the Web Developers menu.

![web inspector](/web_inspector.png)

We are then going to refresh the page and look for a javascript file for main-es2015.js

We will then go to that page at: ***http://MACHINE_IP/main-es2015.js***


To get this into a format we can read, click the ***{ }*** button at the bottom  

Now search for the term "admin"

You will come across a couple of different words containing "admin" but the one we are looking for is "path: administration"

![Administrators page](/administrator.png)

To get this into a format we can read, click the { } button at the bottom  

Now search for the term "admin"

You will come across a couple of different words containing "admin" but the one we are looking for is "path: administration"

    946a799363226a24822008503f5d1324536629a0

***Question #2: View another user's shopping basket!***

Login to the Admin account and click on '**Your Basket**'. Make sure **Burp** is running so you can capture the request!

Forward each request until you see: ***GET /rest/basket/1 HTTP/1.1***

![Burp Basket Intercept Request](/backet.png)

Now, we are going to change the number 1 after /basket/ to 2

![Basket request change](/request_change.png)

It will now show you the basket of UserID 2. You can do this for other UserIDs as well, provided that they have one!

![UserID 2 Basket](//user_2_basket.png)

    41b997a36cc33fbe4f0ba018474e19ae5ce52121

Question #3: Remove all 5-star reviews!

Navigate to the  ***http://MACHINE_IP/#/administration*** page again and click the bin icon next to the review with 5 stars!

![Five star review Deletion](/5_Star_reviews.png)

    50c97bcce0b895e446d61c83a21df371ac2266ef

## Where did that come from?

![Where did that come from?](/where.png)

**XSS** or **Cross-site scripting** is a vulnerability that allows attackers to run javascript in web applications. These are one of the most found bugs in web applications. Their complexity ranges from easy to extremely hard, as each web application parses the queries in a different way.

There are three major types of XSS attacks:

### DOM (Special)

***DOM XSS*** **(Document Object Model-based Cross-site Scripting)** uses the HTML environment to execute malicious javascript. This type of attack commonly uses the <script></script> HTML tag.

### Persistent (Server-side)

Persistent XSS is javascript that is run when the server loads the page containing it. These can occur when the server does not sanitise the user data when it is uploaded to a page. These are commonly found on blog posts.

### Reflected (Client-side)

Reflected XSS is javascript that is run on the client-side end of the web application. These are most commonly found when the server doesn't sanitise search data. XSS or Cross-site scripting is a vulnerability that allows attackers to run javascript in web applications. These are one of the most found bugs in web applications. Their complexity ranges from easy to extremely hard, as each web application parses the queries in a different way.

***Question #1: Perform a DOM XSS!***
