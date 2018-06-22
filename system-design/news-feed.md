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

# 实践 {#30}

接下来我们来实现一个消息广场的功能。很多App中都有动态或消息广场的功能，在消息广场中一般有两个Tab，一个是关注人，一个是广场，我们这里重点来看关注人。

要实现的功能如下：

* 用户之间可以相互关注
* 用户可以发布新消息
* 用户可以查看自己发布的消息列表
* 用户可以查看自己关注的人的消息

采取前面的方案：

* 使用TableStore作为存储和推送系统
* 采用Timeline的显示方式，希望用户可以认真看每条Feed
* 采用推模式

## 角色 {#31}

接着，我们看看角色和每个角色需要的功能：

* 发送者

  * 发送状态：add\_activity\(\)

* 接收者

  * 关注：follow\(\)
  * 读取Feed流：get\_activity\(\)

Feed消息中至少需要包括下面内容：

* 消息：

  * 发送人：actor
  * 类型：verb，比如图片，视频，文本
  * 文本文字：message

## 架构图 {#32}

![](https://yqfile.alicdn.com/778795c6c911b155561ad551dfda9d4afee7f842.png "feed2")

* 发布新消息

  * 接口：add\_activity\(\)
  * 实现：

    * get\_range接口调用关注列表，返回粉丝列表。
    * batch\_write\_row接口将feed内容和ID批量写入个人页表（发件箱）和所有粉丝的关注页表（收件箱），如果量太大，可以多次写入。或者调用异步batch\_write\_row接口，目前C++ SDK和JAVA SDK提供异步接口。

* 关注

  * 接口：follow\(\)
  * 实现：

    * put\_row接口直接写入一行数据\(关注人，粉丝\)到关注列表和粉丝列表（粉丝，关注人）即可。

* 获取Feed流消息

  * 接口：get\_activity\(\)
  * 实现：

    * 从客户端获取上次读取到的最新消息的ID：last\_id
    * 使用get\_range接口读取最新的消息，起始位置是last\_id，结束位置是MAX。
    * 如果是读取个人页的内容，访问个人页表即可。如果是读取关注页的内容，访问关注页表即可。

## 计划 {#33}

上面展示了如何使用表格存储TableStore的API来实现。这个虽然只用到几个接口，但是仍然需要学习表格存储的API和特性，还是有点费时间。

为了更加易用性，我们接下来会提供Feeds流完整解决方案，提供一个LIB，接口直接是add\_activity\(\)，follow\(\)和get\_activity\(\)类似的接口，使用上会更加简单和快捷。

# 扩展 {#34}

前面讲述的都是Timeline类型的Feed流类型，但是还有一种Feed流类型比较常见，那就是新闻推荐，图片分享网站常用的Rank类型。  
我们再来回顾下Rank类型擅长的领域：

* 潜在Feed内容非常多，用户无法全部看完，也不需要全部看完，那么需要为用户选出她最想看的内容，典型的就是图片分享网站，新闻推荐网站等。

我们先来看一种架构图：

![](https://yqfile.alicdn.com/996e90e2e725e16ef78e9803898eb5277dcb9f8a.png "rank1")

* 这种Rank方式比较轻量级，适用于推拉结合的场景。
* 写流程基本一样
* 读流程里面会先读取所有的Feed内容，这个和Timeline也一样，Timeline里面的话，这里会直接返回给用户，但是Rank类型需要在一个排序模块里面，按照某个属性值排序，然后将所有结果存入一个timeline cache中，并返回分数最高的N个结果，下次读取的时候再返回\[N+1, 2N\]的结果。

再来看另外一种：  
![](https://yqfile.alicdn.com/f7b38cc6c04fdb16200bdcb559b55190d060df19.png "rank2")

* 这种比较重量级，适用于纯推模式。
* 写流程也和Timeline一样。
* 每个用户有两个收件箱：

  * 一个是关注页Timeline，保存原始的Feed内容，用户无法直接查看这个收件箱。
  * 一个是rank timeline，保存为用户精选的Feed内容，用户直接查看这个收件箱。

* 写流程结束后还有一个数据处理的流程。个性化排序系统从原始Feed收件箱中获取到新的Feed 内容，按照用户的特征，Feed的特征计算出一个分数，每个Feed在不同用户的Timeline中可能分数不一样的，计算完成后再排序然后写入最终的rank timeline。
* 这种方式可以真正为每个用户做到“千人千面”。

上述两种方式是实现Rank的比较简单，常用的方式。

