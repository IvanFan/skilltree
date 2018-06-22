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

account relation: nosql relation is clear but data is huge redis

feed data: huge data set. Stable relational db + redis

Solution:

选用推拉结合的方式，推模式更加简单。

![](https://yqfile.alicdn.com/ac5465cc451372d20cafeca586a51b7560a46c1c.png "feed\_arch")

## 发布Feed流程 {#24}

当你发布一条Feed消息的时候，流程是这样的：

1. Feed消息先进入一个队列服务。
2. 先从关注列表中读取到自己的粉丝列表，以及判断自己是否是大V。
3. 将自己的Feed消息写入个人页Timeline（发件箱）。如果是大V，写入流程到此就结束了。
4. 如果是普通用户，还需要将自己的Feed消息写给自己的粉丝，如果有100个粉丝，那么就要写给100个用户，包括Feed内容和Feed ID。
5. 第三步和第四步可以合并在一起，使用BatchWriteRow接口一次性将多行数据写入TableStore。
6. 发布Feed的流程到此结束。

## 读取Feed流流程 {#25}

当刷新自己的Feed流的时候，流程是这样的：

1. 先去读取自己关注的大V列表
2. 去读取自己的收件箱，只需要一个GetRange读取一个范围即可，范围起始位置是上次读取到的最新Feed的ID，结束位置可以使当前时间，也可以是MAX，建议是MAX值。由于之前使用了主键自增功能，所以这里可以使用GetRange读取。
3. 如果有关注的大V，则再次并发读取每一个大V的发件箱，如果关注了10个大V，那么则需要10次访问。
4. 合并2和3步的结果，然后按时间排序，返回给用户。

至此，使用推拉结合方式的发布，读取Feed流的流程都结束了。

## 更简单的推模式 {#26}

如果只是用推模式了，则会更加简单：

* 发布Feed：

  1. 不用区分是否大V，所有用户的流程都一样，都是三步。

* 读取Feed流：

  1. 不需要第一步，也不需要第三步，只需要第二步即可，将之前的2 + N\(N是关注的大V个数\) 次网络开销减少为 1 次网络开销。读取延时大幅降级。



所以这里设计的时候就需要重点关心下面两点：

* 单个模块的不可用，不应该阻止整个关键的读Feed流路径，如果大V的无法读取，但是普通用户的要能返回，等服务恢复后，再补齐大V的内容即可。
* 当模块无法承受这么大流量的时候，模块不应该完全不可服务，而应该能继续提供最大的服务能力，超过的拒绝掉。

那么如何优化呢？

* 不使用大V/普通用户的优化方式，使用活跃用户/非活跃用户的优化方式。这样的话，就能把用户群A和部分用户群B分流到其他更分散的多个路径上去。而且，就算读3路径不可用，仍然对活跃用户无任何影响。
* 完全使用推模式就可以彻底解决这个问题，但是会带来存储量增大，大V微博发送总时间增大，从发给第一个粉丝到发给最后一个粉丝可能要几分钟时间（一亿粉丝，100万行每秒，需要100秒），还要为最大并发预留好资源，如果使用表格存储，因为是云服务，则不需要考虑预留最大额度资源的问题。



