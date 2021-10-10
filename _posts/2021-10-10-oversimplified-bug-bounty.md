---
layout: post
author: ashikka
title: Oversimplified - Bug Bounty
---

<div style="text-align: center">
<a href="https://ibb.co/QDG7YMx"><img src="https://i.ibb.co/nPSqQjK/1.jpg" alt="1" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
    What finding vulnerabilities looks like in movies
</div>
</div>

Undoubtedly, most of us believe that finding vulnerabilities in software looks something like the image above. “Hacking” has always been glorified through movies/shows which has led us to believe a common misconception.

<div style="text-align: center">
<a href="https://ibb.co/ww4F3bW"><img src="https://i.ibb.co/yYqcGMy/2.png" alt="2" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
   What finding vulnerabilities is actually like
</div>
</div>

What if I told you, you can spots bugs in applications using something like this. Thanks to a workshop I recently attended, BugeDex, conducted by [CloudSEK](https://cloudsek.com/) and the [Computer Society of India — VIT Chapter](https://csivit.com/), I was introduced to an amazing tool: [BeVigil](https://bevigil.com/), which helped me overcome this misconception of mine.

# What are vulnerabilities?

According to the most basic definition as provided by Wikipedia:

> A **software bug** is an error, flaw or fault in a computer program or system that causes it to produce an incorrect or unexpected result, or to behave in unintended ways.

<div style="text-align: center">
<a href="https://ibb.co/0cvhwZP"><img src="https://i.ibb.co/f2L4ctm/3.jpg" alt="3" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
  A software bug
</div>
</div>

But let’s try to understand this with a simple example. Say, you need to divide 2 numbers and print the output. How would you do that?

```python
# Python program to divide two numbers

number1=int(input("Enter the first number: "))
number2=int(input("Enter the second number: "))

result=number1/number2

print(result)
```

The program gives us an error as we forgot an important **edge case**. We should check if the second input number is 0 otherwise our program will crash. Software bugs are usually something that developers tend to overlook while coding.

<div style="text-align: center">
<a href="https://imgbb.com/"><img src="https://i.ibb.co/grQWhfg/4.png" alt="4" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
  Edge case output
</div>
</div>

# How to avoid bugs?

The answer is simple: **Testing**.

Software testing is a technique for determining if the actual software product meets the expected criteria and ensuring that it is defect-free. It entails the use of manual or automated techniques to assess one or more attributes of interest by executing software/system components. In contrast to real requirements, software testing’s goal is to find flaws, gaps, and missing requirements.

We test mainly for:
1. Functional vulnerabilities
2. Security vulnerabilities
3. Usability
4. Compatibility

## Testing Environments

As we all know, the development team for a software application/product follows a series of procedures that are carried out at various stages of the Software Development Life Cycle (SDLC). Software testing is an essential element in the SDLC since it assures the product’s quality.
There are three types of testing environments: 1) Black-Box Testing, 2) Gray-Box Testing, 3) and White-Box Testing.

<div style="text-align: center">
<a href="https://ibb.co/xf123Xq"><img src="https://i.ibb.co/GtWdFnQ/5.png" alt="5" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
  The three kinds of testing environments
</div>
</div>

1. **Black-Box Testing** <br />
This is an approach that involves testing the functionality of software applications without knowing the internal code structure, implementation details, or internal pathways. Black Box Testing is a type of software testing that focuses on the input and output of software applications and is fully driven by software requirements and specifications. <br /> <br />
For example, if you download a mobile application for the first time ever and you use it, you are essentially a black box tester as you have no previous knowledge about the internal implementation of the application.

2. **White-Box Testing** <br />
White-Box Testing is a software evaluating approach that involves testing the product’s underlying structure, architecture, and code in order to validate input-output flow and enhance the design, usability, and security. Because code is visible to testers in white box testing, it is also known as Clear-Box Testing. <br /><br />
For example, let us consider a simple function to add two numbers. <br /> <br />
```
sum (int a, int b) {            ------------  sum is a function 
    int result = a + b; 
    If (result> 0)
    	Print ("Positive", result)
    Else
    	Print ("Negative", result)
    }                           -----------   End of the source code
```
The goal of White-Box testing in software engineering is to verify all the decision branches, loops, statements in the code. <br /> <br />
The White-Box test cases would be:
- A = 1, B = 1
- A = -1, B = -3
<br /> <br />
3. **Gray-Box Testing** <br />
A software testing technique to test a software product or application with partial knowledge of the internal structure of the application. The purpose of Gray- Box testing is to search and identify the defects due to improper code structure or improper use of applications.<br /><br />
For example, while testing websites feature like links or orphan links, if the tester encounters any problem with these links, then he can make the changes straight away in HTML code and can check in real-time.

# Let’s Go Bug Hunting!

With the new weapons, I had acquired in the workshop, I set out to squash some bugs.

I began by searching up one app that I frequently use on the BeVigil panel at [https://bevigil.com](https://bevigil.com). BeVigil reported an average security rating for the app and instantly showed me a list of vulnerabilities in the app.

<div style="text-align: center">
<a href="https://imgbb.com/"><img src="https://i.ibb.co/kD6cRM0/6.png" alt="6" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
  BeVigil reported a score of 5.9 for the app
</div>
</div>

<div style="text-align: center">
<a href="https://ibb.co/FBqwbVv"><img src="https://i.ibb.co/NLnj6Sh/7.png" alt="7" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
  It also found a list of vulnerabilities in the app!
</div>
</div>

I started by reviewing each of these vulnerabilities. The first two on the list looked like they were only relevant to devices that were rooted, so I moved on to the next ones.

The next vulnerability dealt with intercepting android Intents that the application triggers. I wasn’t really sure if this was really exploitable.

The last vulnerability on the list dealt with some kind of secure data being stored in “shared preferences” by the app. A quick google search about shared preferences told me that it was basically a sort of local database for android applications to persist data between app restarts. BeVigil had a handy feature that actually let me see the exact line in the decompiled source where the app stored data in this DB:

<div style="text-align: center">
<a href="https://ibb.co/MNsxvmW"><img src="https://i.ibb.co/F7KLrFN/8.png" alt="8" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
  Amazing, I can view the exact line in the source code where the app accesses shared prefs!
</div>
</div>

Scrolling around the code, I found that the app was using these keys to actually implement a feature called [Certificate Pinning](https://www.digicert.com/blog/certificate-pinning-what-is-certificate-pinning/#:~:text=What%20is%20certificate%20pinning%3F,entity%20certificates%20of%20their%20choice.). Pinning basically lets you restrict which certificates the app trusts while making web requests. This was probably an additional security measure implemented by the app to stop (or at least prevent) users from intercepting network calls made by the app using tools like Wireshark.

Then, I thought about something [Mr. Syed Shahrukh Ahmad](https://www.linkedin.com/in/ACoAABltRaIBFlYh_4aUplSaRyEYzSwjkNQwpGQ), CTO of Cloudsek had mentioned in his talk:

> Whenever you try to look for vulnerabilities, try to think from the perspective of the person who developed the app, and how they must have gone about implementing the application.

I knew that the app must be communicating with some kind of a backend server to retrieve user credentials, but the BeVigil panel didn't seem to show any such URLs, except firebase endpoints. This is when I decided to do a little digging of my own!

I started by obtaining an APK of the application, then uploaded it on this online decompiler I found ([http://www.javadecompilers.com/apk](http://www.javadecompilers.com/apk)). It gave me a zip file to download, and instantly I could see some java files in the result:

<div style="text-align: center">
<a href="https://ibb.co/PDF6bwH"><img src="https://i.ibb.co/MBZCjM0/9.png" alt="9" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
 That’s a lot of gibberish
</div>
</div>

Rather than sit and scroll through each of these files, I knew what I was looking for, I wanted to find any traces of the app communicating with some kind of backend server. Since URLs start with “http”, I tried searching for this string on the decompiled source, and found a few matches:

<div style="text-align: center">
<a href="https://ibb.co/T4htdx5"><img src="https://i.ibb.co/dPKDxwy/10.png" alt="10" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
 One of the URLs found by the search
</div>
</div>

I tried using the “curl” CLI tool to send a post request to this route:

```bash
curl -X POST 'http://cannot.be.dis.clo:3001/sed/somePath'
```

I got back the following response:

```json
{"timestamp":"2021-10-10T22:21:08.606+0000","status":400,"error":"Bad Request","message":"Required request body is missing: public java.lang.String org.vtop.mobile.service.ApplicationMobileController.spot(java.util.Map<java.lang.String, java.lang.String>) throws org.json.JSONException","path":"/webapi/somePath"}%
```

Clearly, I was missing a parameter. Looking back at the decompiled source code, I could notice that the code seemed to be passing a parameter called “regNo” into a hashmap. I tried passing this as a parameter on the curl request:

```bash
curl -X POST 'http://cannot.be.dis.clo:3001/sed/somePath' -H 'Content-Type: application/json' --data '{"regNo":"123"}' | jq
```

And voila! It worked!! The route response returned back some data:

<div style="text-align: center">
<a href="https://ibb.co/6wkBV77"><img src="https://i.ibb.co/mT381VV/11.png" alt="11" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
</div>
</div>

This was a vulnerability, as I knew that the data the route returned was normally only accessible once the user has logged into the app with his/her credentials. The fact that I was able to retrieve this data just by supplying any random value as the parameter, meant that the backend endpoint was missing authentication!

# Final Thoughts
Cybersecurity is a very vast field and very crucial in the present times since almost everything is online. As for upcoming software engineers, I’m sure we have written a lot of code that was possibly vulnerable to some exploits without ever realizing about them. This workshop gave me a great insight into how to test my software for the presence of fatal bugs, and how to fix them.

I was overjoyed that I had discovered my first bug at the end of this workshop. Moreover, in the journey, I had learned so many new things: *Intents, Shared Preferences, Certificate Pinning, Decompiling android applications, Using “curl”*. I am sure these will be valuable in finding even more bugs in the future!

By the end of the workshop, I truly felt like a…

<div style="text-align: center">
<a href="https://ibb.co/4fVt6m2"><img src="https://i.ibb.co/0XG2gDK/12.gif" alt="12" border="0"></a>
<br />
<div style="margin:10px 0px; color: grey;">
</div>
</div>
