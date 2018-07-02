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

## Let's  do a micro design :

Question: please implement user recommendation module:

each user like a set of music:

u1 = { m1m2m3m4m5m6 }

u2 = {m3 m5m6 }

Similarity\(u1, u2\) = 3

For a user, find his top1 similar user:

```
class Recommander:
    findTopOneSimilar(user)
```

Necessary:

ASK:

total user per day = 100,000,000

total music = 1,000,000,000

peak user = 6,000,000

Participation rate = 1-30% we pick 5%

Calculation Frequency = 1/10 min/user \(10 mins calculate once\)

Predict:

Peak QPS \(Query per second\): peak user \*  Participation rate/ 10 min /60 sec = 500/s

performance query = 2s/query

Max capability = 5 qps

n^3 的算法估算是秒

n^2 百十毫秒

n 十几毫秒

Extension:

[https://www.ibm.com/developerworks/cn/web/1103\_zhaoct\_recommstudy1/](https://www.ibm.com/developerworks/cn/web/1103_zhaoct_recommstudy1/)

**基于人口统计学的推荐机制Demographic-based Recommendation: based on user profile**

Pros:

no need for history data so new user has not cold start problem

it doesn't depend on the item so it can be used between different fields

Cons:

rough categorization

**基于内容的推荐**

基于内容的推荐是在推荐引擎出现之初应用最为广泛的推荐机制，它的核心思想是根据推荐物品或内容的元数据，发现物品或者内容的相关性，然后基于用户以往的喜好记录，推荐给用户相似的物品。图 3 给出了基于内容推荐的基本原理。

##### 图 3. 基于内容推荐机制的基本原理 {#fig3}

![](https://www.ibm.com/developerworks/cn/web/1103_zhaoct_recommstudy1/image007.jpg "图 3. 基于内容推荐机制的基本原理")

这种基于内容的推荐机制的好处在于它能很好的建模用户的口味，能提供更加精确的推荐。但它也存在以下几个问题：

1. 需要对物品进行分析和建模，推荐的质量依赖于对物品模型的完整和全面程度。在现在的应用中我们可以观察到关键词和标签（Tag）被认为是描述物品元数据的一种简单有效的方法。
2. 物品相似度的分析仅仅依赖于物品本身的特征，这里没有考虑人对物品的态度。
3. 因为需要基于用户以往的喜好历史做出推荐，所以对于新用户有“冷启动”的问题。

### 基于协同过滤的推荐 {#minor4.3}

**基于用户的协同过滤推荐**

基于用户的协同过滤推荐的基本原理是，根据所有用户对物品或者信息的偏好，发现与当前用户口味和偏好相似的“邻居”用户群，在一般的应用中是采用计算“K- 邻居”的算法；然后，基于这 K 个邻居的历史偏好信息，为当前用户进行推荐。下图 4 给出了原理图。基于用户的协同过滤推荐机制和基于人口统计学的推荐机制都是计算用户的相似度，并基于“邻居”用户群计算推荐，但它们所不同的是如何计算用户的相似度，基于人口统计学的机制只考虑用户本身的特征，而基于用户的协同过滤机制可是在用户的历史偏好的数据上计算用户的相似度，它的基本假设是，喜欢类似物品的用户可能有相同或者相似的口味和偏好。

##### 图 4. 基于用户的协同过滤推荐机制的基本原理 {#fig4}

![](https://www.ibm.com/developerworks/cn/web/1103_zhaoct_recommstudy1/image009.jpg "图 4. 基于用户的协同过滤推荐机制的基本原理")

**基于项目的协同过滤推荐**

基于项目的协同过滤推荐的基本原理也是类似的，只是说它使用所有用户对物品或者信息的偏好，发现物品和物品之间的相似度，然后根据用户的历史偏好信息，将类似的物品推荐给用户，基于项目的协同过滤推荐和基于内容的推荐其实都是基于物品相似度预测推荐，只是相似度计算的方法不一样，前者是从用户历史的偏好推断，而后者是基于物品本身的属性特征信息。

**基于模型的协同过滤推荐**

基于模型的协同过滤推荐就是基于样本的用户喜好信息，训练一个推荐模型，然后根据实时的用户喜好的信息进行预测，计算推荐。

基于协同过滤的推荐机制是现今应用最为广泛的推荐机制，它有以下几个显著的优点：

1. 它不需要对物品或者用户进行严格的建模，而且不要求物品的描述是机器可理解的，所以这种方法也是领域无关的。
2. 这种方法计算出来的推荐是开放的，可以共用他人的经验，很好的支持用户发现潜在的兴趣偏好

而它也存在以下几个问题：

1. 方法的核心是基于历史数据，所以对新物品和新用户都有“冷启动”的问题。
2. 推荐的效果依赖于用户历史偏好数据的多少和准确性。
3. 在大部分的实现中，用户历史偏好是用稀疏矩阵进行存储的，而稀疏矩阵上的计算有些明显的问题，包括可能少部分人的错误偏好会对推荐的准确度有很大的影响等等。
4. 对于一些特殊品味的用户不能给予很好的推荐。
5. 由于以历史数据为基础，抓取和建模用户的偏好后，很难修改或者根据用户的使用演变，从而导致这个方法不够灵活。

### 混合的推荐机制 {#minor4.4}

在现行的 Web 站点上的推荐往往都不是单纯只采用了某一种推荐的机制和策略，他们往往是将多个方法混合在一起，从而达到更好的推荐效果。关于如何组合各个推荐机制，这里讲几种比较流行的组合方法。

1. 加权的混合（Weighted Hybridization）: 用线性公式（linear formula）将几种不同的推荐按照一定权重组合起来，具体权重的值需要在测试数据集上反复实验，从而达到最好的推荐效果。
2. 切换的混合（Switching Hybridization）：前面也讲到，其实对于不同的情况（数据量，系统运行状况，用户和物品的数目等），推荐策略可能有很大的不同，那么切换的混合方式，就是允许在不同的情况下，选择最为合适的推荐机制计算推荐。
3. 分区的混合（Mixed Hybridization）：采用多种推荐机制，并将不同的推荐结果分不同的区显示给用户。其实，Amazon，当当网等很多电子商务网站都是采用这样的方式，用户可以得到很全面的推荐，也更容易找到他们想要的东西。
4. 分层的混合（Meta-Level Hybridization）: 采用多种推荐机制，并将一个推荐机制的结果作为另一个的输入，从而综合各个推荐机制的优缺点，得到更加准确的推荐。

## Performance  & Scalability & Availability:

## Performance:

improve the performance with Inverted index:

inverted index: 传统的正排索引指的是doc-&gt;word的映射，然而在实际工作中，仅仅只有正排索引是远远不够的，比如我想知道某个word出现在那些doc当中，就需要遍历所有的doc，这在实时性要求比较严的系统中是不能接受的。因此，就出现了倒排索引（inverted index ）[https://zh.wikipedia.org/wiki/倒排索引](https://zh.wikipedia.org/wiki/倒排索引)

![](/assets/Screen Shot 2018-07-02 at 5.49.14 pm.png)

performance query = 0.2s/query

Max capability = 5 qps

![](/assets/Screen Shot 2018-07-02 at 5.51.02 pm.png)'

new version

improve the availability

![](/assets/Screen Shot 2018-07-02 at 6.01.29 pm.png)

how many machine you need?

multi thread: 8 cores CPU

## System Design with more machines

在一台机器上挂着一个manager，manger搞定anything

manger  copy a new dataset

database cluster

## 

## Please evaluate query per second

## Please scale your system

## Please design Netflix/Youtube/Spotify



