## Managed Heap Basics
Your application's memory is limited by the process's virtual address space. In a 32-bit process, you can allocate close to 1.5 gigabytes (GB) and in a 64-bit process, you can allocate close to 8 terabytes.

C#'s `new` operator causes the CLR to perform several steps.

### Managed Heap
There is a managed heap for each managed process. All threads in the process allocate memory for objects on the same heap.

The CLR manages two separate heaps for allocation
- The large object heap (LOH)
- The small object heap (SOH)

Small .NET objects are allocated onto the SOH. There are three of these: Generation 0, Generation 1, and Generation 2. Objects move up these Generations based on their age.

### Generations
​The managed heap is partitioned into three regions:
##### Generation 0
Objects in generation 0 are newly constructed objects that the garbage collector has never examined. After a garbage collection, generation 0 survivors are promoted to generation 1. As always, generation 0 is empty immediately after a garbage collection and is where new objects will be allocated.
​It usually starts with a budget between 256 KB-4 MB and might grow slightly if the need arises.

##### Generation 1
A typical starting budget for generation 1 ranges from 512 KB-4 MB. Objects in generation 1 are examined only when generation 1 reaches its budget. Note that the only natural trigger for a collection in generation 1 is a prior collection in generation 0, as objects are promoted from generation 0 to generation 1. Short-lived objects with finalizers are guaranteed to reach generation 1.

##### Generation 2
Surviving objects from generation 1 are promoted to generation 2. This promotion reflects the fact that they are now considered old objects. The size of generation 2 can extend the entire memory space dedicated for the OS process, i.e., up to 2 GB of memory on a 32-bit system, or up to 8 TB of memory on a 64-bit system.

One of the primary risks in generational model is that temporary objects creep into generation 2 and die shortly afterwards; this is the **mid-life crisis**. It is extremely important to ensure that temporary objects do not reach generation 2.

​If you see a high proportion of memory in Gen 2, it's an indicator that memory is being held onto for a long time, and that you may have a memory problem. 
​
### Large Object Heap (LOH)
It ​is a special area reserved for large objects. Large objects are objects that occupy more than 85KB of memory. This threshold applies to the object itself, and not to the size of the entire object graph rooted at the object, so that an array of 1,000 strings (each 100 characters in size) is not a large object because the array itself contains only 4-byte or 8-byte references to the strings, but an array of 50,000 integers is a large object.
​
LOH is only collected during a generation 2 collection. Large objects are GC’ed at every Gen2 GC even when the GC was not triggered by lack of space in LOH. Note that there isn’t a GC that only collects large objects.

The CLR treats large objects slightly differently than how it treats small objects:
- Large objects are not allocated within the same address space as small objects, they ​are allocated from the LOH directly
- The GC doesn't compact large objects because of the time it would require to move them in memory.
- Large objects are immediately considered to be part of generation 2; they are never in generation 0 or 1.

**.NET 4.5.1 now supports compacting the LOH, `GCSettings.LargeObjectHeapCompactionMode` property.**

Usually large objects are large strings (like XML or JSON) or byte arrays that you use for I/O operations, such as reading bytes from a file or network into a buffer so you can process it.

One effective strategy is **pooling large objects** and reusing them instead of releasing them to the GC. Since fewer allocations and collections take place on the LOH, fragmentation is less likely to occur and the program’s performance is likely to improve. The cost of maintaining the pool might be smaller than the cost of performing full collections. Another possible approach (if arrays of the same type are involved) is allocating a very large object and manually breaking it into chunks as they are needed.

## Garbage Collection
The CLR uses a **reference tracking algorithm** instead of reference counting algorithm which cannot handle circular references well. This algorithm only cares about reference type variables, because only these variables can refer to an object on the heap. Value type variables contain the value type instance directly.

The CLR's GC is a **generational garbage collector**. It makes the following assumptions:
- Young .NET objects are expected to die quickly
- old .NET objects are expected to live longer
- Collecting a portion of the heap is faster than collecting the whole heap.

*The definitions of "young" and "old" depend on the frequency of garbage collections the application induces.*

Types of collection
- Partial collection
- Full collection

When starting a garbage collection, the garbage collector must decide which generations to examine. When the CLR initializes, it selects budgets for all three generations. If allocating a new object causes generation 0 to surpass its budget, a garbage collection must start. When starting a garbage collection, the garbage collector also sees how much memory is occupied by generation 1. If generation 1 occupies much less than its budget, the garbage collector examines only the objects in generation 0.

The garbage collector dynamically modifies budget of all generations after every collection.

Microsoft's performance tests show that it takes less than 1 millisecond to perform a garbage collection of generation 0.

### GC roots
all reference type variables:
- Local variables
- Static variables
- GC handles
- f-reachable queue

A static field keeps whatever object it refers to forever or until the AppDomain that the types are loaded into is unloaded. A commom way to leak memory is to have a static field refer to a collection object and then to keep adding items to the collection.

We do not need the local reference to remain an active root until the end of its enclosing method. For example, after a xml document was loaded and displayed, we might want to introduce additional code within the same method that no longer requires the document to be kept in memory. This code might take a long time to complete, and if a garbage collection occurs in the meantime, we would like the document’s memory to be reclaimed. Garbage collector provides this **eager root collection** facility. So breaking your code into smaller methods and using less local variables is not just a good design measure or a software engineering technique. With the .NET garbage collector, it can provide a performance benefit as well because you have less local roots! It means less work for the JIT when compiling the method, less space to be occupied by the root IP tables, and less work for the GC when performing its stack walk.

### Garbage Collection Algorithm
When the CLR starts a GC, the CLR first suspend all threads in the process (not true in concurrent GC). Then the CLR performs the **mark phase** of the GC, during which it resolves all objects that are still referenced by the application (live objects). The GC will set a bit in the sync block index field of an object in order to prevents infinite loop from occurring in the case where you havea circular reference. Now that the CLR knows which objects must survive and which objects can be deleted, ​the collector moves to the **sweep phase**, at which time it reclaims the space occupied by unused objects.​ Finally, the collector concludes with the **compact phase**, in which it shifts live objects so that they remain consecutive in memory (makes the CPU cache much more efficient). After the compaction phase is complete, the CLR resumes all the application's threads. 

**Mark phase** can be concurrent:
- False negative
- False positive

Note that objects in the large object heap do not get compacted, therefore fragmentation is possible with the large object heap. If there is no address space left in the processes to allocate a new GC segment, the `new` operator that attempted to allocate more memory ends up throwing an `OutOfMemoryException`.

### Garbage Collection Triggers
- Generation 0 has filled its budget, This is the most common trigger of a GC.
- The large object heap reaches a threshold.
- Code explicitly calls `GC.Collect` method
- Windows is reporting low memory conditions
- An `AppDomain` is unloaded
- The process (or the CLR) is shutting down

Garbage collections do not only occur when memory is full or close to full. Instead, garbage collections occur whenever generation 0 is full. 

### Garbage Collection Modes
When the CLR starts, it selects a GC mode, and this mode cannot change during the lifetime of the process. There are two basic GC modes:

#### Workstation GC
Fine-tuned for **client-side** applications. It is optimized to provide for **low-latency** GCs in order to minimize the time an application's threads are suspended so as not to frustrate the end user.

This GC flavor introduced has a major drawback. Although during a generation 2 concurrent GC application threads are allowed to continue allocating, they can only allocate as much memory as would fit into generations 0 and 1. As soon as this limit is encountered, application threads must block and wait for the collection to complete.

**Background GC,** introduced in CLR 4.0, enables the CLR to perform a garbage collection in generations 0 and 1 even though a full collection was underway. To allow this, the CLR creates two GC threads: a foreground GC thread and a background GC thread. The background GC thread executes generation 2 collections in the background, and periodically checks for requests to perform a quick collection in generations 0 and 1. When a request arrives, the background GC thread suspends itself and yields execution to the foreground GC thread, which performs the quick collection and unblocks the application threads.

###### Concurrent Workstation GC
- It is the default flavor, has background GC.​​
- Under concurrent workstation GC, there is a separate, dedicated GC thread marked with THREAD_PRIORITY_HIGHEST that executes the garbage collection from start to finish. Moreover, the CLR can decide that it wants some phases (e.g. mark phase) of the garbage collection process to run concurrently with application threads.​
- UI applications using concurrent workstation GC should exercise great care to prevent garbage collections from occurring on the UI thread. This boils down to performing allocations on background threads, and refraining from explicitly calling GC.Collect on the UI thread.
- If the garbage collection is triggered by the UI thread, the UI thread is blocked until GC completes (because it needs memory), and there are other application threads competing for resources with the GC. This can actually lower the UI responsiveness because the UI thread is blocked until GC completes, and there are other application threads competing for resources with the GC.
- If you have a lot of free memory, the garbage collector won't compact the heap; this improves performance but grows your application's working set. When using the concurrent garbage collector, you'll typically find that your application is consuming more memory than it would with the non-concurrent garbage collector.

###### Non-concurrent Workstation GC
- It does not have background GC.
- It ​suspends the application threads during both the mark and sweep phases.​
- The primary usage scenario for non-concurrent workstation GC is the case mentioned in the previous section, when the UI thread tends to trigger garbage collections. In this case, non-concurrent GC might provide better responsiveness, because background threads won’t be competing with the garbage collection for which the UI thread is waiting, thus releasing the UI thread more quickly.

##### Server GC
Fine-tuned for **server-side** applications. It is optimized for **throughput** and **resource utilization**.

An application that uses the server GC has the following characteristics:
- There is a separate managed heap for each processor in the affinity mask of the .NET process. The purpose of this separation is to minimize contention on the managed heap while allocations are made.
- There is a GC thread for each processor that is in the affinity mask of the .NET process. This allows each thread to perform garbage collection in parallel on the managed heap assigned to its processor.
- During both phases of garbage collection, all application threads are suspended. This allows GC to complete in a timely fashion and allows application threads to continue processing requests as soon as possible. It maximizes throughput at the expense of latency: some requests might take longer to process while a garbage collection is in progress, but overall the application can process more requests because less context switching is introduced while GC is in progress.
- The only limitation imposed on using the server GC flavor is the number of physical processors on the machine. If there is just one physical processor on the machine, the only available GC flavor is the workstation GC.

Its also has concurrent (has background GC) and non-concurrent (does not have background GC) mode.

#### Configuration 
By default, applications run with the **Workstation GC** mode. A stand-alone application can tell the CLR to use the Server GC mode by creating a configuration file that contains a `gcServer` element. However, if the server application is running on a uniprocessor machine, then the CLR will always use Workstation GC mode.

```csharp
GCSettings.IsServerGC
```

You can tell the CLR not to use the concurrent collector by setting a configuration file

```xml
<configuration>
	<runtime>
		<gcConcurrent enabled="false"/>
	</runtime>
</configuration>
```

The GC mode cannot change while the process runs. However, your application can have some control over the garbage collection by using the `GCSettings.GCLatencyMode` property.
- **Batch** (default for the Server GC mode) - Turns off the concurrent GC.
- **Interactive** (default for the Workstation GC mode) - Turns on the concurrent GC.
- **LowLatency** - ​Supported only on concurrent workstation GC. Use this latency mode during short-term, time-sensitive operations (like drawing animations). The GC will avoid doing any generation 2 collections (except if calling GC.Collect() or Windows tells the CLR that system memory is low).
- **SustainedLowLatency** - ​Supported on both server and workstation GC. Use this latency mode to avoid long GC pauses for the bulk of your application's execution. A trading application that must respond immediately to price changes is an example of this kind of application.

Under `LowLatency` mode, it is more likely that your application could get an `OutOfMemoryException` thrown. Therefore, stay in this mode for as short a time as possible, avoid allocating many
objects, avoid allocating large objects, and set the mode back to `Batch` or `Interactive` by using a constrained execution region (CER). Also, remember that the latency mode is a process-wide setting and threads may be running concurrently. These other threads could even change this setting while another thread is using it.

```csharp
private static void LowLatencyDemo()
{
	GCLatencyMode oldMode = GCSettings.LatencyMode;
	RuntimeHelpers.PrepareConstrainedRegions();
	try
	{
		GCSettings.LatencyMode = GCLatencyMode.LowLatency;
		// Run your code here...
	}
	finally
	{
		GCSettings.LatencyMode = oldMode;
	}
}
```

### Monitoring Application Memory Usage
The GC class offers the following static methods, which you can call to see how many collections have occurred of a specific generation or how much memory is currently being used by objects in the managed heap.

```csharp
Int32 CollectionCount(Int32 generation);
Int64 GetTotalMemory(Boolean forceFullCollection);
```

You can also see how much memory is being used by individual AppDomains as opposed to the whole process.

When you install the .NET Framework, it installs a set of performance counters that offer a lot of real-time statistics about the CLR’s operations. To monitor the CLR’s garbage collector, select the `.NET CLR Memory performance` object, which contains a set of counters.

Another great tool for analyzing the memory and performance of your application is PerfView. This tool can collect Event Tracing for Windows (ETW) logs and process them. 

Finally, you should look into using the SOS Debugging Extension (SOS.dll), which can often offer great assistance when debugging memory problems and other CLR problems.

#### `GCHandle`

#### `WeakReference<T>` 
If you want to keep a reference to something as long as it is used elsewhere, you can use a weak reference. It can be useful for pointing to objects which should be available for GC if they are not actively in use.

However, if the program uses large number of small objects, weak references can negatively affect memory usage. Weak references make your code more complicated and potentially error prone. There are runtime overheads with using weak references. If you use a weak references for something that your application is highly likely to need in the future, you may incur the cost of repeatedly recreating it. 

#### `ConditionalWeakTable<TKey,TValue>` 
It allows you to "attach" additional information to existing, managed, non-dynamic CLR objects at runtime without needing to modify the class itself. It can be understand just as a dictionary, where both the keys and the values are weakly referenced, and a value is kept alive as long as the key is alive. The key is not kept alive by the ConditionalWeakTable, unlike a `Dictionary`. Unlike `WeakReference`, set the value to null will not make the value get garbage collected if the key object is still alive, because the TKey has a strong reference to the TValue. It is fully thread-safe.

The class is used to implement the dependency property mechanism used by XAML. DLR use it to implement dynamic languages on the .NET runtime. Also can be useful when need to track memory leaks.
 
- Caveat 1: **Restrictions on TKey**. Be careful what type you specify for TKey. I stronly recommend that you only use types that use `reference equality`. This means that I don’t recommend you use `string`. `string` not only uses value equality, but also has a complex *interning* feature. Remember, `ConditionalWeakTable` tracks object **instances**, not object **values**.
- Caveat 2: **IDisposable is ignored on TValue**. `ConditionalWeakTable` will **not** dispose any `IDisposable` values attached to object instances. They will (eventally) be finalized, but the standard restrictions on finalizers apply.

http://connectedproperties.codeplex.com/

### Garbage Collection Performance Best Practices

- Temporary objects should be short-lived.
- Large objects should be long-lived or pooled. Avoid mid-life crisis.
- References between generations should be kept to a minimum.
- Prefer deterministic finalization and implement `IDisposable` to ensure that clients know what to expect from your class. Use `GC.SuppressFinalize` within your `Dispose` implementation to ensure that the finalizer isn't called if it isn't necessary.
- Value types used as local stack variables have no deallocation cost.
- Value types embedded in reference types minimize the cost of both phases of garbage collections: less objects to mark, and reduce the overhead of copying many small objects around at sweep phase.
- Value types reduce the application’s memory footprint because they occupy less memory. Value types embedded in reference types exhibit locality of access.
- Value types reduce references between generations, because fewer references are
introduced into the object graph.
- Finalization is a backup mechanism for automatically releasing unmanaged resources in a non-deterministic fashion. Prefer deterministic finalization to automatic finalization whenever possible, but offer clients both alternatives.

##### Object Pooling
Object pooling can provide a performance boost if the allocation and deallocation costs (not including initialization and uninitialization) are more expensive than managing the lifetime of the objects manually. 

An example: http://referencesource.microsoft.com/#System.ServiceModel/System/ServiceModel/Channels/BufferManager.cs,2d794266eb592bf0

https://github.com/microsoft/microsoft.io.recyclablememorystream

Implementing an efficient pool requires taking at least the following factors into consideration:
- Synchronization around the allocation and deallocation operations must be kept to a
minimum. For example, a lock-free (wait-free) data structure could be used to implement
the pool.
- The pool should not be allowed to grow indefinitely, implying that under certain
circumstances objects will be returned to the mercy of the garbage collector.
- The pool should not be exhausted frequently, implying that a growth heuristic is needed
to balance the pool size based on the frequency and amount of allocation requests.

