idea：relational database is not the best option here

we can use redis to implement the push/pull  pub/sub function



Concept:

Feed: each news is a feed

Feed Flow: each user screen is a feed flow

Timeline: a type of feed flow

Rank: a kind of feed flow

Aggregate: a kind of feed flow with similar topic



Features:

complex relation

unstable relation

Write/read: 1:100



Storage:

account relation: nosql relation is clear but data is huge

feed data: huge data set. Stable relational db + redis



Solution:

选用推拉结合的方式，推模式更加简单。

![](https://yqfile.alicdn.com/ac5465cc451372d20cafeca586a51b7560a46c1c.png "feed\_arch")

