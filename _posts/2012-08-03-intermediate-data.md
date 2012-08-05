---
layout: post
title: On Availability of intermediate data in cloud computations 笔记
categories: [nosql, key-value]
---
# {{page.title}}

[On Availability of intermediate data in cloud computations](http://static.usenix.org/event/hotos09/tech/full_papers/ko/ko.pdf) 这是来自U of illinois at Urbnana-Champaingn的一篇论文, HotOS'09。

文章讨论了介绍了分布式（或者云计算）环境中data flow programming的特点，multiple stages, stage之间有intermediate data。 中间结果有两大特性：

1. 和传统的intermediate data类似，比如short-lived, used immediately, write once and read once. 
2. 新特性： large scale, computation-barrier 

然后文章指出intermediate data的管理是个新课题。当前的解决方案就是写在本地磁盘，然后消费者在需要的时候远程获取。错误冗余方案也很简单，重新跑一下相应的task就ok了。 这里的假设是重新生成中间结果的代价很小。但是这个假设不见得成立，可能有cascaded re-execution 或者 every stage from the fault point need to be re-executed。后面举了若干例子。 显然需要一个针对中间结果的存储管理系统。 

Sec 2开始正经的讨论。 分别讨论了intermediate data的特性，系统失败造成的影响，以及一个intermediate data管理系统需求分析。Sec 2.3 总结了两点需求： data availablity， minimal interference。显然这是两个冲突的需求。强调availablilty必然会带来interference，反之亦然。

文章评估了3种不同的方案

1. 现有方案
2. intermediate data写到HDFS里面去
3. 用TCP-Nice做background replication

想必TCP-Nice的方案效果不好，因为文中都没有评估结果。 文中指出了使用方案3的问题： 

1. TCP-Nice是以network utilization 为代价的
2. TCP-Nice不够快（TCP-Nice自己network flow的priority比较低）

最后，文章展望了可能的设计：

1. Master/Slave 需要有个master来协调intermediate data存储
2. 不同的replication policy。 基于空闲带宽的，基于deadline的，基于cost的。

总之，这是个好文章，提出了ISS的概念，但是并没有提出好玩的实现。


 




