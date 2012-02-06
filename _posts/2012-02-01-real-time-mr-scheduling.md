---
layout: post
title: Real Time MapReduce Scheduling 笔记
categories: [mapreduce, scheduling]
---
# {{page.title}}

[Real-time mapreduce scheduling](http://repository.upenn.edu/cis_reports/942/) 是Pennsylvania 2010的一篇Technical Report。这里所谓的Real-time scheduling是指sheduling M/R jobs, 使得特定的Job能在指定的deadline之前完成。 所以作者的工作之一就是在task level找出影响execution predictability的因子。这些因子可以使得sheduling更加细颗粒度。

作者讨论了real time scheduling的目标： 
1. hard real time jobs 必须满足deadline要求
2. 使得满足soft real time 的job最多

Contribution：
1. 在EC2上所做的一系列测试以及对相关参数的讨论 
2. CSP formulation 
3. 对几种启发式算法的讨论

Related Work提到两大块内容： Hadoop performance相关的分析工作（包括性能分析工具以及调度算法），completion time esitimation的相关工作。 

## EC2上所做的一系列测试以及对相关参数的讨论
相关参数是指影响execution predictability的相关参数，这里强调了task level completion time。

1. slot-to-core ratio
2. multiple concurrent jobs
3. data placement
4. heart beat interval

文章给出的数据只讨论了第一种情况下task completion time 的方差（篇幅关系？ 其实应该都给出的）。

## real time scheduling as a CSP
### offline 
CSP formulation 
依据四个参数： slot-to-core ratio，data placement，heart beat interval以及新引入的CPU处理能力的权重。
在一系列assumption的基础上，提出了8个constains。。 。然后在job数量increase的前提下，整个问题顺利的变成了NP-hard。
文章讨论了两种启发式算法以及对CSP的refinement。
### online
作者讨论他们ongoing work
1. enhancement of MR execution model。这个和Hadoop 0.23里面所做的工作类似，就是消除map/reduce的区别，使得slot不再分为map slot和reduce slot
2. online 启发式scheduling，详见paper

## future directions
1. 分层scheduling 算法（这个在real time computing里面是常用的技术） 以及real time VM
2. 当系统里面的job都是soft real time job时（对hadoop集群来说，这简直是一定的！），建立probablistic model

总的来说，这篇文章是Technical Report，还有很多ongoing的work没有完成，期待他们最后的工作总结。









