# Performance recommendations for Unity #

## Garbage collections ##
When writing managed code, thinking about the performance impact of the garbage collector is important. The fact that you have a garbage collector does not mean you don't have to worry about memory management, it just changes how you think about it.

While the GC doesn't typically contribute all that much to overall cycles spent or power usage, it does cause unpredictable spikes that can make you drop several frames in a row which leads to holograms looking "glitchy" or unstable. This is true even for Generation 0 collection. So while guidance for other kinds of apps might be that short-lived allocations are cheap, this is not true for applications that need to update at 60 frames per second.

A key aspect of managing the GC is to plan for it up front. It's usually much harder to fix high GC usage after-the-fact than it is to take some extra care up front.

**Tips for avoiding allocations.** The ideal way to avoid allocations (and thus collections) is to allocate everything you need at the startup of the application and just reuse that data while the app runs. There are plenty of cases where it's not possible to avoid allocations (for example APIs that return allocated objects), so this isn't actually possible in practice, but it's a good goal to shoot for.

There are also some unintuitive reasons why allocations may occur even when you're not directly allocating something, here are some tips to avoid that:

- Do not use LINQ, as it causes heavy allocations.
- Do not use lambdas, as they cause allocations.
- Beware of boxing! A common case for that is passing structs to a method that takes an interface as a parameter. Instead, make the method take the concrete type (by ref) so that it can be passed without allocation.
- Prefer structs to classes whenever you can.
- Default implementations for value equality and GetHashcode uses reflection, which is not only slow but also performs a lot of allocations.
- Avoid foreach loops on everything except raw arrays. Each call on a non-array allocates an Enumerator. Prefer regular for loops whenever possible.

**Other garbage collections concerns.** Another key concept to be aware of is that GC time is largely proportional to the number of references in the heap. Thus, it's preferable to store data as structs instead of objects. For example, instead of referring to an object by reference, you might allocate a bunch of those types of objects as a shared array of structs and then refer to them by index. This is not as important for long-lived types (e.g. the ones you allocate at startup and keep around for the duration of the application), since they will rapidly move into the oldest generation and stay there (where you hopefully don't have many collections at all), but worth keeping in mind whenever it's easy to do.

Memory utilization is also a key factor for garbage collection performance. Generally speaking, garbage collections gets significantly costlier the less free space you have available. Typically if you're doing a lot of allocations (and thus garbage collecting a lot) you'd want to keep at least half the heap free. One consequence of this is that you might think you're not doing so badly with respect to garbage collection early on, but then as more and more content gets added to your application and free memory gets used up, you could start experiencing significant GC issues all of a sudden.

Consider enabling [Sustained Low Latency](https://msdn.microsoft.com/en-us/library/system.runtime.gcsettings.latencymode.aspx) mode in the .NET garbage collector. This will cause the GC to try much harder to avoid stopping your application threads, which can reduce the number of pauses. Be aware that this can only take you so far, and will work best if you have a low rate of allocations to start with.

## General CPU performance tips ##
Aside from GC, you also need to be aware of the general CPU cost of updating your scene. Here are a few things that you may consider to keep things efficient:

- Optimize for cache coherency. Cache misses are orders of magnitude more expensive than most CPU instructions, so avoiding random jumping through memory can have a *huge* impact on performance. Flat arrays of compact structs that are processed in order is ideal.
- Avoid any synchronous loading code or other long running operations. For this reason you should make sure that any long running operation is asynchronous.
- Avoid interfaces and virtual methods in hot code such as inner loops. Interfaces in particular are much slower than a direct call. Virtual functions are not as bad, but still significantly slower than a direct call.
- Looping over a native array with direct indexing is by far the fastest way to process a list of items. List<T> is significantly slower, even worse if you use foreach, and even worse if you cast to IList first. Note, looping over a collection isn't that slow in the first place, so this mainly matters in hot spots of the code.
- Consider caching often-used components. For example, if you often need to access the Rigid Body of an object, just grab it once and reference it with a private variable rather than looking it up each time.
- Avoid the foreach construct. This will sometimes allocate an IEnumerable, and just generally introduce iteration overhead. It's usually much faster to explicitly iterate over a concrete collection type.