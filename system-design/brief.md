## What's system design

Conceptual Design

Logical Design

Physical Design

From top to down

## What's a good design

Healthiness

* execution
* communication

Simplicity

* no more;no less
* understandable

## Please design the system

## Please evaluate query per second

## Please scale your system

## Please design Netflix/Youtube/Spotify

Example: media player

Crack the design in 5 steps:

**input**

1. scenario: case/interface
2. necessary: constrain/hypothesis

**output**

1. application: service/algorithms
2. kilobit:data

**extra**

1. evolve

### scenario: case/interface

**step 1 : enumerate  枚举**

* register/login
* play music
* music recommendation

**step 2: sort 把最重要的功能找出来**

Top1:play music

* get channels
* select a channel
* play the music

## necessary: constrain/hypothesis

**step1: Ask**

the number of daily users: 1,000,000

**step 2: Predict**

**User:**

concurrent user = user per day /5 = 200,000

peak user = concurrent user \* 3 = 600,000

Max peak user in 3 months = peak user \* 10 = 6,000,000

**Traffic:**

request of new music per user: 1 message /min

music size = 3 mb

max peak traffic = Max peak user in 3 months \* music size /60 = 300GB/s

**Memory:**

memory per user = 10kb

max daily memory = the number of daily users \* memory per user \* 10 = 100GB

## application: service/algorithms

Step1: replay the case, add a service for each request

Step2: merge the service

![](/assets/Screen Shot 2018-07-02 at 3.10.25 pm.png)

## kilobit:data

Step1: append data set for each request under the service

Step2: choose the storage type

user data less write sensitive high requirement MYSQL

![](/assets/Screen Shot 2018-07-02 at 3.14.04 pm.png)

## Evolve

**Step1: Analysis**

**With:**

Better: constrain

Broader: new cases

Deeper: more details

**From the view of:**

Performance

Scalability  

Robustness/availability

