---
layout: post
title: Online Aggregation for Large MapReduce Jobs 笔记
categories: [mapreduce, online aggregation, hyracks]
---
# {{page.title}}
[Online Aggregation for Large MapReduce Jobs](http://hyracks.googlecode.com/files/OnlineAggregationForLargeMapReduceJobs.pdf) 是VLDB 11上的一篇文章。 作者在Rice做了一个talk，[视频](http://vimeo.com/29806431) 和 [slides](http://prezi.com/4u9xyqzhc7qy/online-aggregation-for-large-mapreduce-jobs/) 可点击链接。此外，相应的[源代码](http://code.google.com/p/hyracks-online-aggregation/) 已经释放。

和[MapReduce Online (HOP)](http://www.eecs.berkeley.edu/Pubs/TechRpts/2009/EECS-2009-136.pdf)相比，这篇文章的着眼点在于建立相应的统计模型，在online aggregation的过程中，利用sample结果估计最终结果，并给出相应的置信度。另外一大亮点是整个工作是基于[hyracks](http://code.google.com/p/hyracks) 的，相信将来会有越来越多的论文是在Hyracks上完成。 



