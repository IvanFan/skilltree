

Say you're looking at the traffic for facebook profiles. You have billions of hits. You want to find which profiles are accessed the most often. You could keep a count for each profile, but then you'd have a very large number of counts to keep track of, the vast majority of which would be meaningless.

With lossy counting, you periodically remove very low count elements from the table. The most-frequently accessed profiles would almost never have low counts anyway, and if they did, they wouldn't be likely to stay there for long.

The algorithm basically involves grouping the inputs into blocks or chunks and counting within each chunk. Then you reduce the count for each element by one, dropping any elements whose counts drop to zero.

The most-frequently hit profiles will get on your count and stay there. Any profiles that aren't hit very often will drop to zero in a few blocks and you won't have to track them any more.

Note that the final results are order-dependent, giving heavier weight to the counts processed last. In some cases, this makes perfect sense and is an upside rather than a downside. \(If you want to know basically which profiles are the most popular_now_, you want to weigh accesses today more than accesses last month.\)

There are a large number of refinements to the algorithm. But the basic idea is this -- to find the heavy hitters without having to track every element, periodically purge your counts of any elements that don't seem likely to be heavy hitters based on the data so far.



我们之前在[数据流基本问题--确定频繁元素（一）](http://blog.csdn.net/dm_ustc/article/details/45895851)中提到了频繁元素的一个计算问题（找出出现次数超过m/k的元素），里面的算法返回的结果里肯定包含出现次数超过m/k的元素，但是也可能包含不超过m/k的元素（false positive）。对于这个缺点，必须得进行额外一次的重新扫描，以确定最终答案。我们只允许进行一次的扫描，那么该怎么去做呢？这里我们简单讨论下lossy counting算法和sticky sampling算法。

一、lossy counting算法

其实问题是一样的，这里换个说法来叙述。我们定义两个参数，一个是支持度阈值s，一个是允许错误范围参数![](http://latex.codecogs.com/gif.latex?%5Cepsilon)（![](http://latex.codecogs.com/gif.latex?%5Cepsilon%20%5Cll%20s)）。如果我们想要找出出现频率超过0.1%的元素，则s=0.1%。我们要找到出现次数超过s\*m的元素。算法返回的结果里面允许一定的错误，但是允许的范围是![](http://latex.codecogs.com/gif.latex?%5Cepsilon)\*m，也就是说结果中不会出现次数少于\(s-![](http://latex.codecogs.com/gif.latex?%5Cepsilon)\)\*m的元素。我们一般设![](http://latex.codecogs.com/gif.latex?%5Cepsilon)为s的1/10。

算法的大致过程很简单，描述如下：将整个数据流切分成多个窗口。

![](https://img-blog.csdn.net/20150527202926888?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZG1fdXN0Yw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


对于第一个窗口，先统计每个元素及其出现次数，处理结束后，每个元素的出现次数都减一。

![](https://img-blog.csdn.net/20150527202817022?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZG1fdXN0Yw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


然后继续处理后面的窗口。每处理完一个窗口对保存的所有元素的出现次数都减一。

![](https://img-blog.csdn.net/20150527203025950?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZG1fdXN0Yw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


算法输出最终计数超过\(s-![](http://latex.codecogs.com/gif.latex?%5Cepsilon)\)\*m的元素。

如果整个流的长度为m，窗口大小w=1/![](http://latex.codecogs.com/gif.latex?%5Cepsilon)。这样任意元素元素统计的最大误差是![](http://latex.codecogs.com/gif.latex?%5Cepsilon)m。该做法空间复杂度为1/![](http://latex.codecogs.com/gif.latex?%5Cepsilon) \* log\(![](http://latex.codecogs.com/gif.latex?%5Cepsilon)\*m\)。其实lossy counting的误差比[数据流基本问题--确定频繁元素（一）](http://blog.csdn.net/dm_ustc/article/details/45895851)中方法小，是以使用更多空间作为代价的。

