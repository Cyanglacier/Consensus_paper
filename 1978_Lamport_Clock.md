> What usually matters is not that all processes agree on exactly what time it is, but rather that they agree
> on the order in which events occur.
>
> 问题的关键不在对事件的具体发生时间达成一致，而在于对事件的先后顺序达成一致。
>
> -- Distributed Systems 3e by Steen & Tanenbaum
>
> -- 《分布式系统（第3版）》Steen、Tanenbaum

> since we cannot synchronize clocks perfectly across a distributed system, we cannot in general use physical
> time to find out the order of any arbitrary pair of events occurring within it.
>
> 由于无法完美同步一个分布式系统里的时钟，因此一般不能用物理时间来决定系统中任意一对事件的先后顺序。
>
> -- Distributed Systems: Concepts and Design 5e by CDKB_12
>
> -- 《分布式系统：概念与设计（第5版）》CDKB_12

The concept of one event happening before another in a distributed system is examined, and is shown to define
a partial ordering of the events. A distributed algorithm is given for synchronizing a system of logical clocks
which can be used to totally order the events. The use of the total ordering is illustrated with a method for
solving synchronization problems. The algorithm is then specialized for synchronizing physical clocks, and a
bound is derived on how far out of synchrony the clocks can become.

研究了分布式系统中一个事件发生在另一个事件之前的概念，并且定义了事件之间的偏序关系。给出了一个分布式算法，用于
同步系统中的逻辑时钟，从而对事件进行全序排序。通过一个解决同步问题的方法展示了全序关系的用处。接着将这个算法用
来同步物理时钟，并且推导出时钟之间非同步状态所能达到的上限。

> http://betathoughts.blogspot.jp/2007/06/brief-history-of-consensus-2pc-and.html
>
> The issue is that in a distributed system you cannot tell if event A happened before event B, unless A caused B
> in some way. Each observer can see events happen in a different order, except for events that cause each other,
> ie there is only a partial ordering of events in a distributed system. Lamport defines the "happens before"
> relationship and operator, and goes on to give an algorithm that provides a total ordering of events in a
> distributed system, so that each process sees events in the same order as every other process.
>
> 问题在于，在一个分布式系统中，你无法确定事件A是否在事件B之前发生，除非A以某种方式引发B。每个观察者观察到的事件顺序
> 都可能是不一样的，也就是说分布式系统中的事件只存在偏序关系。兰伯特定义了“在之前发生”的关系和操作符，进而给出一个算
> 法，用来为分布式系统中的事件提供一个全序关系，从而使每个进程观察到的事件顺序都是一样的。
>
> Lamport also introduces the concept of a distributed state machine: start a set of deterministic state machines
> in the same state and then make sure they process the same messages in the same order. Each machine is now a
> replica of the others. The key problem is making each replica agree what is the next message to process: a
> consensus problem. This is what the algorithm for creating a total ordering of events does, it provides an agreed
> ordering for the delivery of messages. However, the system is not fault tolerant; if one process fails that others
> have to wait for it to recover.
>
> 兰伯特还引入了分布式状态机的概念：确保一组确定状态机以相同的状态开始，以同样的顺序处理同样的消息。这样每个状态机都
> 是其它状态机的一份拷贝。关键问题是要让每份拷贝在要处理的下条消息上达成一致，即共识问题。这就是为事件创建全序关系的
> 算法所做的事情，它为消息投递提供了一致认可的顺序。尽管如此，这个系统并不具备容错性；一旦一个进程失败，其它进程就要
> 等待它恢复。

译者注：有意思的是，兰伯特的原文并没有强调阐述分布式状态机的概念，为此兰伯特事后还特地发表了评论。

> http://lamport.azurewebsites.net/pubs/pubs.html#time-clocks
>
> It didn't take me long to realize that an algorithm for totally ordering events could be used to implement any
> distributed system. A distributed system can be described as a particular sequential state machine that is
> implemented with a network of processors. The ability to totally order the input requests leads immediately
> to an algorithm to implement an arbitrary state machine by a network of processors, and hence to implement any
> distributed system.  So, I wrote this paper, which is about how to implement an arbitrary distributed state machine.
> As an illustration, I used the simplest example of a distributed system I could think of--a distributed mutual
> exclusion algorithm.
>
> 我很快意识到事件的全序排序算法可以用来实现任何一个分布式系统。一个分布式系统可以被描述为由一组处理器网络实现的一个
> 特别的顺序状态机。当我们具备对输入请求进行全序排序的能力后，我们立即就可以得到一个算法，可以用一组处理器网络来实现
> 任意一个状态机，由此实现任何的分布式系统。因此我写的这篇文章是关于如何实现任意的分布式状态。作为示例，我试用了我能
> 想到的分布式系统的最简单的例子——一个分布式互斥锁算法。
>
> This is my most often cited paper. Many computer scientists claim to have read it. But I have rarely encountered
> anyone who was aware that the paper said anything about state machines. People seem to think that it is about
> either the causality relation on events in a distributed system, or the distributed mutual exclusion problem.
> People have insisted that there is nothing about state machines in the paper. I've even had to go back and reread
> it to convince myself that I really did remember what I had written.
>
> 这是我被引用得最多的论文。许多计算机科学家声称读过这篇文章。但我发现几乎没有任何一个人意识到这篇文章讲述了状态机的
> 东西。大家好像觉得它讲的是分布式系统中事件之间的关系的随意性，或是分布式互斥锁问题。大家坚持认为这篇文章没有讲关于
> 状态机的任何东西。我甚至不得不回头重新将它读一遍以确认我确实记得自己写的东西。

译者总结：

* 问题
  - 一致性不是简单的让两个节点最终对一个值的结果一致, 很多时候还需要对这个值的变化历史在不同节点上的观点也要一致
  - 不能简单的以接受到消息的时间作为事件的顺序判断的依据。
  - 物理时间的不可依赖，导致事件的先后关系在分布式系统中退化为事件先后顺序的偏序关系(partital order), 问题的核心演变成
    如何找出因果关系来取消对物理时钟的依赖。（如果一致性继续弱化的情况下，最终一致（弱一致性）连因果关系都不需要）
* 论文：Time, Clock and Ordering of Events in a Distributed System
   * https://lamport.azurewebsites.net/pubs/pubs.html#time-clocks
* Lamport Clock
  - 是针对事件发生历史的逻辑时钟, 它让我们能够把所有的历史事件找到偏序关系, 而且不仅在各自节点的逻辑时间参考系内顺序
    一致, 全局上的顺序也是一致的。

![screen shot 2017-10-25 at 6 53 06 pm](https://user-images.githubusercontent.com/1134993/31995139-d625ea56-b948-11e7-9b81-468651250f7f.png)

P/Q/R是三个节点, 从下往上代表物理时间的流逝, p1, p2, q1, q2, r1, r2 …. 表示事件，波浪线表示事件的发送, 比如p1->q2表示
P把p1事件发送给了Q, Q接受此消息作为q2事件。

![screen shot 2017-10-25 at 6 54 20 pm](https://user-images.githubusercontent.com/1134993/31995186-01abe57c-b949-11e7-91c5-f3e3d159e965.png)

![screen shot 2017-10-25 at 6 51 11 pm](https://user-images.githubusercontent.com/1134993/31995072-998fbe28-b948-11e7-8d43-510a3e766b9a.png)

* 两种一致性模型（Seq，linear）
   - Sequential Consistency (1979)
     * How to Make a Multiprocessor Computer That Correctly Executes Multiprocess Programs
     * https://lamport.azurewebsites.net/pubs/pubs.html#new-approach
     * https://lamport.azurewebsites.net/pubs/pubs.html#multi
   - linearizability Consistency (1990, M.P. Herlihy & J.M Wing)
     * 线性一致性：Seq的前提下，加强进程间的操作排序
