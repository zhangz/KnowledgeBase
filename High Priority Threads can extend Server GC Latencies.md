## High Priority Threads can extend Server GC Latencies ##

***The problem***

Before going into the problem details, I want to highlight some key characteristics about Server GC mode.

ServerGC mode is designed with scalability in mind.  In this mode, the CLR tries to take advantage of every CPU available to your process.  The CLR creates a dedicated, hard affintized GC thread for each available logical CPU

Hard affinity is achieved through the SetThreadAffinityMask API.  The reason GC threads are hard affinitaized is to avoid thread migration between cores, which could be very expensive on multi-core machines.  Avoiding thread migration enhances the throughput of the Server GC.

In ServerGC mode, all GC threads work in parallel during the GC activity to collect the GC heap, however as many parallel algorithms, sometimes there is a need to join all the threads at some safe points to synchronize some state.

If one or more threads don’t reach the safe point for some reason, the other GC threads will wait for them.  All the managed threads in the process are suspended waiting for the GC to finish during this time.

Recently, I investigated a customer scenario where a GC thread was holding the rest of GC threads at a safe point. The issue was that one of the GC thread was taking much longer (3-5 seconds) than the other threads to reach the safe point and join with other GC threads. This affected the overall application throughput, and increased the per-request latency.  Latency is a VERY important factor for server applications and it was critical to address this problem.

The challenge was to find out what was preventing this thread from joining the other GC threads.

It turned out that a real-time priority thread – sharing the same logical CPU with the affected GC thread – was running for an extended period of time, preventing the GC thread from getting scheduled.

Luckily enough, the real-time priority thread was created from one of the customer’s components, so the customer had control to eliminate the need for this thread.

Setting the high-priority thread back to normal priority allowed the GC thread to get scheduled promptly, eliminating the long latency per request.

The overall latency introduced by GC dropped from 3-5 seconds, to few hundred milliseconds.

Most CPUs in the system were idle during the time the high-priority thread was running, so in theory the OS scheduler could have given more preference to our hard affintized thread over the real-time thread, and schedule the real-time thread on any Idle CPU in the system.

After consulting with the Kernel team, it turns out that this is a known issue in the W2k3 scheduler that could be improved, and in fact, we have made improvements in **Win2k8+** OSes.  The bad news was the customer was still running Win2k3.

***Conclusion***

Raising thread priorities to real-time is not recommended, it could have severe effects on the overall system performance, and it has a negative effect on hard-affinitized threads.

Although we have done some scheduler improvement to mitigate this issue on Win2k8+, it is highly recommended not to alter the thread priorities for your application threads.