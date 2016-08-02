## Managed Heap Basics
Your application's memory is limited by the process's virtual address space. In a 32-bit process, you can allocate close to 1.5 gigabytes (GB) and in a 64-bit process, you can allocate close to 8 terabytes.

C#'s `new` operator causes the CLR to perform several steps.

## Garbage Collection
The CLR uses a **reference tracking algorithm** instead of reference counting algorithm which cannot handle circular references well. This algorithm only cares about reference type variables, because only these variables can refer to an object on the heap. Value type variables contain the value type instance directly.

GC roots: all reference type variables.

### Garbage Collection Algorithm
When the CLR starts a GC, the CLR first suspend all threads in the process. Then the CLR performs the **marking phase** of the GC. Then the CLR looks at all active roots to see which objects they refer to. The GC will set a bit in the sync block index field of an object in order to prevents infinite loop from occurring in the case where you havea circular reference. Now that the CLR knows which objects must survive and which objects can be deleted, it begins the GC's **compacting phase**. When compact memory, the CLR ensure that every root refers to the same object it did before. After the compaction phase is complete, the CLR resumes all the application's threads. Note that objects in the large object heap do not get conmpacted, therefore fragmentation is possible with the large object heap. If there is no address space left in the processes to allocate a new GC segment, the `new` operator that attempted to allocate more memory ends up throwing an `OutOfMemoryException`.

A static field keeps whatever object it refers to forever or until the AppDomain that the types are loaded into is unloaded. A commom way to leak memory is to have a static field refer to a collection object and then to keep adding items to the collection.

### Generation
The CLR's GC is a **generational garbage collector**. It makes the following assumptions:
- The newer an object is, the shorter its lifetime will be.
- The older an object is, the longer its lifetime will be.
- Collecting a portion of the heap is faster than collecting the whole heap.

Generation in Managed Heap:
- Generation 0: objects in generation 0 are newly constructed objects that the garbage
collector has never examined. After a garbage collection, generation 0 survivors are promoted to generation 1. As always, generation 0 is empty immediately after a garbage collection and is where new objects will be allocated.
- Generation 1: objects in generation 1 are examined only when generation 1 reaches its budget. Any objects that were in generation 1 that survived the collection are now in generation 2.
- Generation 2: objects in generation 2 are objects that the garbage collector has examined two or
more times.

When starting a garbage collection, the garbage collector must decide which generations to examine. when the CLR initializes, it selects budgets for all three generations. So if allocating a
new object causes generation 0 to surpass its budget, a garbage collection must start. When starting a garbage collection, the garbage collector also sees how much memory is occupied by generation 1. If generation 1 occupies much less than its budget, the garbage collector examines only the objects in generation 0.

The garbage collector dynamically modifies budget of all generations after every collection.

Microsoft's performance tests show that it takes less than 1 millisecond to perform a garbage collection of generation 0.

### Garbage Collection Triggers
- The CLR triggers a GC when it detects that generation 0 has filled its budget, This is the
most common trigger of a GC.
- Code explicitly calls `System.GC`'s static `Collect` method
- Windows is reporting low memory conditions
- The CLR is unloading an `AppDomain`
- The CLR is shutting down

Garbage collections do not only occur when memory is full or close to full. Instead, garbage collections occur whenever generation 0 is full. 

### Large Objects
A large object is 85,000 bytes or more in size. The CLR treats large objects slightly differently than how it treats small objects:
- Large objects are not allocated within the same address space as small objects
- The GC doesn't compact large objects because of the time it would require to move them in memory.
- Large objects are immediately considered to be part of generation 2; they are never in generation 0 or 1.

Usually large objects are large strings (like XML or JSON) or byte arrays that you use for I/O operations, such as reading bytes from a file or network into a buffer so you can process it.

### Garbage Collection Modes
When the CLR starts, it selects a GC mode, and this mode cannot change during the lifetime of the
process. There are two basic GC modes:
- Workstation GC
	- Fine-tuned for **client-side** applications. It is optimized to provide for **low-latency** GCs in order to minimize the time an application's threads are suspended so as not to frustrate the end user.
	- Concurrent GC
- Server GC
	- Fine-tuned for **server-side** applications. It is optimized for **throughput** and **resource utilization**.
	- This GC mode causes the managed heap to be split into several sections, one per CPU. When a garbage collection is initiated, the garbage collector dedicates one special thread per CPU; each thread collects its own section in parallel with the other threads. 

By default, applications run with the **Workstation GC** mode. A stand-alone application can tell the CLR to use the Server GC mode by creating a configuration file that contains a `gcServer` element. However, if the server application is running on a uniprocessor machine, then the CLR will always use Workstation GC mode.

```csharp
GCSettings.IsServerGC
```

In addition to the two modes, the GC can run in two sub-modes: **concurrent** (the default) or **nonconcurrent**.

In concurrent mode, the GC has an additional background thread that marks objects concurrently while the application runs. If you have a lot of free memory, the garbage collector won't compact the heap; this improves performance but grows your application's working set. When using the concurrent garbage collector, you'll typically find that your application is consuming more memory than it would with the non-concurrent garbage collector. (more details in Chapter 21 - CLR via C#)

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
- **LowLatency** - Use this latency mode during short-term, time-sensitive operations (like drawing animations). The GC will avoid doing any generation 2 collections (except if calling GC.Collect() or Windows tells the CLR that system memory is low).
- **SustainedLowLatency** - Use this latency mode to avoid long GC pauses for the bulk of your application's execution. A trading application that must respond immediately to price changes is an example of this kind of application.

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
It allows you to "attach" additional informationto existing, managed, non-dynamic CLR objects at runtime without needing to modify the class itself. It can be understand just as a dictionary, where both the keys and the values are weakly referenced, and a value is kept alive as long as the key is alive. The key is not kept alive by the ConditionalWeakTable, unlike a `Dictionary`. Unlike `WeakReference`, set the value to null will not make the value get garbage collected if the key object is still alive. It is fully thread-safe.

The class is used to implement the dependency property mechanism used by XAML. DLR use it to implement dynamic languages on the .NET runtime. Also can be useful when need to track memory leaks.
 
- Caveat 1: **Restrictions on TKey**. Be careful what type you specify for TKey. I stronly recommend that you only use types that use `reference equality`. This means that I don’t recommend you use `string`. `string` not only uses value equality, but also has a complex *interning* feature. Remember, `ConditionalWeakTable` tracks object **instances**, not object **values**.
- Caveat 2: **IDisposable is ignored on TValue**. `ConditionalWeakTable` will **not** dispose any `IDisposable` values attached to object instances. They will (eventally) be finalized, but the standard restrictions on finalizers apply.

http://connectedproperties.codeplex.com/