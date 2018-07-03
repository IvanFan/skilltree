Divide conquer

Q: many user complain they can not use our serivce

Solution:

Define

Measure

Analysis

Improve

Control

Problem Definition

when defining the problem we also need to select the closed relevant measure

Failure rate:

percentage of user who cannot use the service properly

= number of users who cannot use single service/ number of total user

Mission:reduce the failure rate

How to identity the total of users?

session cookie

how to identity the user behavior?

log trace user activities

![](/assets/Screen Shot 2018-07-03 at 10.42.37 am.png)

then we need to check the failure rate of each step during the request of service

how to trace the DNS failure rate? use client side code

![](/assets/Screen Shot 2018-07-03 at 10.48.44 am.png)

case1: hosts file has been changed by others

case2: DNS analysis by ISP \(internet service provider\)

why DNS cannot be 100%? some company may deny particular ip services

Analysis the web failure rate:

analysis by time

solution1: add more load balancer

solution2: reduce the size of web pages \(CDN min js compress images merge image together\)

solution3: lazy load \(load when required\)

![](/assets/Screen Shot 2018-07-03 at 11.16.38 am.png)

server side render =&gt; for SEO and first page speed

![](/assets/Screen Shot 2018-07-03 at 11.21.40 am.png)

![](/assets/Screen Shot 2018-07-03 at 11.33.41 am.png)

14: a CDN server has failed

15: sync the clock between different servers

16: file size  large timeout : compress files optimise CDN

## CDN

IP distance

ISP location

ping ISP nodes and optimise

how do you know you have fixed the problem?

A/B test

![](/assets/Screen Shot 2018-07-03 at 11.55.18 am.png)

A/B test: team developer. business logic. tool =&gt; A/B test

what happened when your visit google.com in browser

how to increase the speed to visit a webpage

how to design '秒杀' system

filter the request flow

push into first level queue 

from first level queue, push into second level queue with order

