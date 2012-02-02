---
layout: post
title: Improving mapreduce performance in heterogeneous environments 笔记
categories: [mapreduce, scheduling]
---
# {{page.title}}

[Improving mapreduce performance in heterogeneous environments](http://academic.research.microsoft.com/Publication/4727664/improving-mapreduce-performance-in-heterogeneous-environments) 是OSDI 2008的一篇论文。

文章的主要围绕如何改进异构环境Hadoop中的speculative execution

文章指出了Hadoop在设计speculative execution时的一系列隐含假设

1. Nodes can perform work at roughly the same rate.
2. Tasks progress at a constant rate throughout time.
3. 在原本free的slot上运行speculative task不产生额外的代价
4. task progress score如实的反应了真实进度，尤其对于reduce task而言。（在原设计中，reduce task的copy/sort/reduce各占1/3权重）
5. Tasks tend to finish in waves, 运行的慢的有可能就是straggler.
6. Tasks in the same category (map or reduce) require roughly the same amount of work.

异构环境环境下1/2显然不成立，同构环境下3/4/5也不不一定成立。

文章讨论了EC2环境，虽然CPU/Mem有isolation，但是co-location的VM会竞争disk/network IO，作者观察到过2.5倍左右的性能差距。 
对于3/4/5,文章也进行了讨论。

## LATE scheduler

LATE scheduler主要是对speculatieve task进行了改进： 首先LATE提供了一个plugable的接口，可以利用这个接口估计task的completion time。然后，总是speculatively execute所估计出来最后执行完毕的task。

估计task的结束时间时，用了一个相对简单但是很有效的方法;

ProgressRate= ProgressScore/T， where T is the amount of time the task has been run-ning for
Estimate the time to completion = (1 − ProgressScore)/ProgressRate. 

在选择执行speculative task的node时候，要确保是在fast node（高于特定threshold）上执行的。（每个node维护一个score，即这个node上所有完成/进行中的progress score之和）。

总结下来，LATE这样工作

If a node asks for a new task and 系统中现存的speculative task低于 SpeculativeCap:
– Ignore the request if the node’s total progress is below SlowNodeThreshold.
– Rank currently running tasks that are not currently being speculated by estimated time left.
– Launch a copy of the highest-ranked task with progress rate below SlowTaskThreshold.

文章讨论了SpeculativeCap/SlowTaskThreshold的取值，比如SpeculativeCap为10%，SlowTaskThreshold为25%

文章特别指出了估计task结束时间的时候，reduce task比较特殊（reduce task的copy phase会占用相当长的时间）。 






