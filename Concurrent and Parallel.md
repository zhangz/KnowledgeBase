# Concurrent and Parallel
什么是并发(Concurrency，Concurrent)，什么是并行(parallism，Parallel)？这两者有什么区别？本文收录一下我听过的、我见过的、我看过的一些人的看法。仅供参考：

## Paul Butcher

Paul Butcher在他的《Seven Concurrency Models in Seven Weeks 》里开篇就谈到：

An alternative way of thinking about this is that concurrency is an aspect of the problem domain—your program needs to handle multiple simultaneous (or near-simultaneous) events. Parallelism, by contrast, is an aspect of the solution domain—you want to make your program faster by processing different portions of the problem in parallel

也就是说，并发是问题域，并行是解决域。问题是并发的，解决方法是并行的。

## Rob Pike

Rob是Go语言之父，《The Unix Programming Environment》 和 《The Practice of Programming》（最近正在重读这本小册子）的作者。他有一个经典的解释：

Concurrency is about dealing with lots of things at once.

Parallelism is about doing lots of things at once.

嘿嘿，这个有点意思，不过只能意会了。

## Paul E. McKenney

又是一个Paul。不过这个Paul大叔是IBM的研究人员，写了一本非常幽默并且有深度的书：《Is Parallel Programming Hard, And, If So, What Can You Do About It?》在这本书（v2015.01.31a版本）的第319页，有一个附录，介绍 What is the Difference Between “Concurrent” and “Parallel”?时说：

From a classic computing perspective, “concurrent” and “parallel” are clearly synonyms. However, this has not stopped many people from drawing distinctions between the two

说明Paul大叔认为，其实这两者是一回事，有些人非得区分。好吧，那就区分吧。这些人是如何区分呢？有两个perspective：

The first perspective treats “parallel” as an abbreviation for “data parallel,” and treats “concurrent” as pretty much everything else

也就是说，concurrency有很强的interdependencies，它们之间可能要做各种通信，基于比如说locks啊，transactions啊，等等同步机制。相比，parallel中组件的相互依赖就很少。新浪微博网友 @小恶魔提利昂就持这种观点，他说：“并发任务处理的时候，会在并发处理时候可以交换信息，有CSP式的，也可以内存共享式的，但是在外部看到的效果是若干核或若干线程/协程同时对应这些并发任务。并行处理的话，同时处理的任务要做到上下文环境都是隔离的。”

Now, this second perspective can be thought of as making the workload match the available scheduler, with parallel workloads able to operate on a simple scheduler and concurrent workloads requiring more sophisticated schedulers.

恩，第二个角度就是需不需要复杂的scheduler。

但是Paul大叔说，这两个视角很可能是不可兼得或者说矛盾滴。此话怎讲？

考虑每个CPU一个线程的基于lock通信的程序。是Concurrency吗？从第一个角度讲，是的，用lock啊，各种同步各种通信啊。从第二个角度看，又不是。

以上就是Paul McKenney大致的观点。

## yebangyu

yebangyu是博主，yebangyu.org公司CEO兼站长兼董事长兼老板，苦逼屌丝底层搬砖码农。恩，就是我了。

个人观点：

1，首先，持Paul McKenney的观点，没必要区分这两个词。

2，Concurrency这个单词含有类“occur”的词根，表示发生，con代表共同、一起，指共同发生的意思。而parallel词根是para，表示相同的、类似、平行的、差不多的。因为，也可以认为问题是同时发生的，解决方法是平行处理。

3，写书在取书名的时候需要区分。如果你是讲MPI、Open MP这类技术，建议用并行或者说Parallel Computing。如果是讲lock free、multi-thread这些共享内存编程的，建议用Concurrency或者Concurrency Programming。

那么，Go语言这种CSP类型的Channel的呢？个人认为，都可以吧。

