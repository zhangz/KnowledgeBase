Async getting started guide:
https://docs.microsoft.com/en-us/dotnet/articles/csharp/async
https://docs.microsoft.com/en-us/dotnet/articles/standard/async

https://ayende.com/blog/174689/the-cost-of-the-async-state-machine
The problem is most people haven't written code comparable to what async/await does so they don't truly understand it.
Async is not intended to perform better; it's intended to use your scarce server resources better.
To put it in web terms: each individual request might get marginally slower, but you can handle many more of them simultaneously. Which is not a good thing for scenarios that require single raw throughput. Async/await is usually a very poor choice for maximizing compute throughput. Raw threads are a much better choice. Aside from the async/await machinery cost, there's also the slow growth of the .NET thread pool (where tasks run by default), and the allocations and GC costs.
async+await is to avoid callback hell; Tasks are to use resources better, different things but used together.
See the attached result for using TPL. Same as SYNC, much faster than async. http://twitter.com/kdawg02/status/750484848641445888/photo/1
![](attachments/TPLvsAsync.jpg)

Maybe it’s time to write a blog post about, async/await, async io, tasks, iocp, and the thread pool.

The async machinery isn’t free, and in some cases, the performance overhead of using async is actually significantly higher cost than using the standard blocking model. There is also the issue of the debugability of the solution, if you have a lot of async work going on, it is very hard to see what the state of the overall system is.

For requests that are common, short and most of the work is either getting the data from the client / sending the data to the client, with a short (mostly CPU bound) work, we can use async operations, because they free a thread to do useful work (processing the next request) while we are spending most of our time in doing I/O with the remote machine.

For high performance stuff, where we have a single request doing quite a lot of stuff, or long living, we typically want to go the other way. We want to have a dedicated thread for this operation, and we want to do blocking I/O. The logic is that this operation isn’t going to be doing much while we are waiting for the I/O, so we might as well block the thread and just wait for it in place. We can rely on buffering to speed things up, but there is no point in giving up this thread for other work, because this is rare operation that is we want to be able to explicitly track all the way through.

If you have a lot of requests that would be spending a lot of time in I/O, that is a good place for async, since this can reduce the amount of threads and CPU work that is done. However, if you are doing on trivial computation in async, that is likely to be detrimental.

My first instinct would be something like seeing how much time I'm going to be spending waiting for the I/O, and how many concurrent operations I'm going to have. If this is a web server, and I'm reading a lot of requests, async makes a lot of sense. If this is a single / low thread application that does heavy process, probably not make sense.