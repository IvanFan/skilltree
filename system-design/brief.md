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
