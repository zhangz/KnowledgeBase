## Dispose
As the GC is non-deterministic, the Dispose Pattern is available so that you can release the resources you are using - managed or unmanaged. A class should implement `IDisposable` when it is necessary to cleanup resources.

The Garbage collecter will never directly execute a `Dispose` method for you. It must be **explicity** called if resources are to be released, such as within a `using` or `try finally` block. Implementing `IDisposable` provides a way for users of your class to release resources early, instead of waiting for the garbage collector.

Calling `Dispose()` on an object provides for the following:
- Reduce strain on the GC for the process.
- Reduce the app's memory pressure.
- Reduce the chance of an OutOfMemoryException (OOM) if the LOH gets fragmented and the object is on the LOH. Rememeber that the LOH is never compacted!
- Keep the object out of the Finalizable and f-reachable Queues if it has a Finalizer.
- Make sure your resources (managed and unmanaged) are cleaned up in a deterministic fashion.

`Dispose()` can have an effect on promotion and reachability.
- You can explicitly remove rooted references by setting the resource to 'null'.
- You can unregister/detach event handlers in Dispose(). Remember that these are also rooted references.
- If you don't call `Dispose()` when using a COM object (using `Marshal.ReleaseComObject()`) the memory will be pinned and thus cannot be collected and therefore promoted to the next generation. 
- Calling SuppressFinalize() in a Dispose implementation will stop a Finalizer from being called. If a Finalizer does not need to be called, the object will not be placed into the f-reachable queue, and thus not be promoted since it will take at least another full collection to have it collected.

Here is the code for properly implementing the `IDisposable` pattern in a **base class**:

```csharp
//TODO remember to make this class inherit from IDisposable
 
/// <summary>
/// Gets or sets a value indicating whether this instance is disposed.
/// </summary>
/// <value>
///  <c>true</c> if this instance is disposed; otherwise, <c>false</c>.
/// </value>
/// <remarks>Default initialization for a bool is 'false'</remarks>
private bool IsDisposed { get; set; }
 
/// <summary>
/// Implementation of Dispose according to .NET Framework Design Guidelines.
/// </summary>
/// <remarks>Do not make this method virtual.
/// A derived class should not be able to override this method.
/// </remarks>
public void Dispose()
{
    Dispose(true);
 
    // This object will be cleaned up by the Dispose method.
    // Therefore, you should call GC.SupressFinalize to
    // take this object off the finalization queue 
    // and prevent finalization code for this object
    // from executing a second time.
 
    // Always use SuppressFinalize() in case a subclass
    // of this type implements a finalizer.
    GC.SuppressFinalize(this);
}
 
/// <summary>
/// Overloaded Implementation of Dispose.
/// </summary>
/// <param name="isDisposing"><c>true</c> to release both managed and unmanaged resources; 
/// <c>false</c> to release only unmanaged resources.</param>
/// <remarks>
/// <list type="bulleted">Dispose(bool isDisposing) executes in two distinct scenarios.
/// <item>If <paramref name="isDisposing"/> equals true, the method has been called directly
/// or indirectly by a user's code. Managed and unmanaged resources
/// can be disposed.</item>
/// <item>If <paramref name="isDisposing"/> equals <c>false</c>, the method has been called 
/// by the runtime from inside the finalizer and you should not reference
/// other objects. Only unmanaged resources can be disposed.</item></list>
/// </remarks>
protected virtual void Dispose(bool isDisposing)
{
    // TODO If you need thread safety, use a lock around these 
    // operations, as well as in your methods that use the resource.
    try
    {
        if (!this.IsDisposed)
        {
            // Explicitly set root references to null to expressly tell the GarbageCollector
            // that the resources have been disposed of and its ok to release the memory 
            // allocated for them.
            if (isDisposing)
            {
                // Release all managed resources here
                // Need to unregister/detach yourself from the events. Always make sure
                // the object is not null first before trying to unregister/detach them!
                // Failure to unregister can be a BIG source of memory leaks
                if (someDisposableObjectWithAnEventHandler != null)
                {                 
                    someDisposableObjectWithAnEventHandler.SomeEvent -= someDelegate;
                    someDisposableObjectWithAnEventHandler.Dispose();
                    someDisposableObjectWithAnEventHandler = null;
                }
                // If this is a WinForm/UI control, uncomment this code
                //if (components != null)
                //{
                //    components.Dispose();
                //}
            }
            // Release all unmanaged resources here  
            // (example)             if (someComObject != null && Marshal.IsComObject(someComObject))
            {
                Marshal.FinalReleaseComObject(someComObject);
                someComObject = null;
            } 
        }
    }
    finally
    {
        this.IsDisposed = true;
    }
}

//TODO Uncomment this code if this class will contain members which are UNmanaged
///// <summary>Finalizer for MyDisposableClass</summary>
///// <remarks>This finalizer will run only if the Dispose method does not get called.
///// It gives your base class the opportunity to finalize.
///// DO NOT provide finalizers in types derived from this class.
///// All code executed within a Finalizer MUST be thread-safe!</remarks>
//  ~MyDisposableClass()
//  {
//     Dispose( false );
//  }
```

Here is the code for properly implementing the IDisposable pattern in a **derived class**:

```csharp
/// <summary>
/// Gets or sets a value indicating whether this instance is disposed.
/// </summary>
/// <value>
///  <c>true</c> if this instance is disposed; otherwise, <c>false</c>.
/// </value>
/// <remarks>Default initialization for a bool is 'false'</remarks>
private bool IsDisposed { get; set; }

/// <summary>
/// Overloaded Implementation of Dispose.
/// </summary>
/// <param name="isDisposing"><c>true</c> to release both managed and unmanaged resources; 
/// <c>false</c> to release only unmanaged resources.</param>
/// <remarks>
/// <list type="bulleted">Dispose(bool isDisposing) executes in two distinct scenarios.
/// <item>If <paramref name="isDisposing"/> equals true, the method has been called directly
/// or indirectly by a user's code. Managed and unmanaged resources
/// can be disposed.</item>
/// <item>If <paramref name="isDisposing"/> equals <c>false</c>, the method has been called
/// by the runtime from inside the finalizer and you should not reference
/// other objects. Only unmanaged resources can be disposed.</item></list>
/// </remarks>
protected override void Dispose(bool isDisposing)
{
    // TODO If you need thread safety, use a lock around these 
    // operations, as well as in your methods that use the resource.
    try
    {
        if (!this.IsDisposed)
        {
            // Explicitly set root references to null to expressly tell the GarbageCollector
            // that the resources have been disposed of and its ok to release the memory 
            // allocated for them.
            if (isDisposing)
            {
                // Release all managed resources here

                // Need to unregister/detach yourself from the events. Always make sure
                // the object is not null first before trying to unregister/detach them!
                // Failure to unregister can be a BIG source of memory leaks
                if (someDisposableObjectWithAnEventHandler != null)
                {                 
                    someDisposableObjectWithAnEventHandler.SomeEvent -= someDelegate;
                    someDisposableObjectWithAnEventHandler.Dispose();
                    someDisposableObjectWithAnEventHandler = null;
                }
                // If this is a WinForm/UI contrlol, uncomment this code
                //if (components != null)
                //{
                //    components.Dispose();
                //}
            }

            // Release all unmanaged resources here
            // (example)
            if (someComObject != null && Marshal.IsComObject(someComObject))
            {
                Marshal.FinalReleaseComObject(someComObject);
                someComObject = null;
            }
        }
    }
    finally
    {
        this.IsDisposed = true;

        // explicitly call the base class Dispose implementation
        base.Dispose(isDisposing);
    }
}
```

### Thread Safety
Use `Interlocked.Exchange` on a 'disposed' flag to ensure that one thread's Dispose attempt happens and the other is silently ignored. This's a good starting point, and it should have been part of the standard Dispose pattern (the CompareExchange should have been done in the sealed base-class wrapper method, to avoid the need for every derived class to use its own private disposed flag). Unfortunately, if one considers what Dispose actually does, things are much more complicated.

**The real purpose of Dispose is not to do something to the object being disposed, but rather to clean up other entities to which that object holds references**. For Dispose to be thread-safe, those other entities would to allow Dispose to clean them up at the same time as other threads might be doing other things with them. Some objects can handle such usage; others cannot.

One particular vexing example: Objects are allowed to have events with RemoveHandler methods that are not thread-safe. Consequently, any Dispose method which cleans up event handlers should only be called from the same thread as the one in which the subscriptions were created.

### Relationship between IDisposable and Memory Management
Calling `Dispose()` does **not** trigger a GC and the GC **never** calls `Dispose()`. `IDisposable` allows for release of **resources** associated with your object (if implemented correctly). `Dispose()` lets the GC know that you are done with this object and its **memory** can be reclaimed at the **next collection for the generation where that object lives**. Calling `Dispose()` has a direct correlation to the work that the GC has to do.

## Finalization
There is no guarantee of when finalizers will be called, and indeed if you have enough memory, if it will ever be called.

You should only release unmanaged resources in the finalizer, since managed references may be invalid. ---? it become root object again, so all managed obj should be still live. Need to confirm.

### Finalization and Performance
- Objects that need finalization live longer than objects that do not.
- Objects that need finalization cause collateral damage.
- Objects needing finalization create work for the finalizer thread.

The bad thing about finalizers is that it causes your object to survive a garbage collection. GC has no way of calling the finalizer during garbage collection. So objects that have to be finalized are moved to the finalizer queue and promotes it into next generation on the heap. And everything the object refers to, directly and indirectly, will also remain in memory. 

The finalizer will run once the finalizer thread is available (if this takes too long the object might actually be promoted a second time). Since objects that live in generation 2 are collected about 10 times as less as objects that live in generation 1, and gen 1 ten times as less as gen 0, it can take some time for such object is finally garbage collected. This means your objects hang around longer, and potentially force more garbage collections.

If disposable object is created and it has a Finalizer, then it will get sent directly to the Finalization Queue and live for at least 1 *extra* GC collection. Once an object is placed in the Finalization Queue it will survive at least 2 additional GC collections - once for the Finalization Queue, once for the f-reachable Queue. Which means it will be promoted to Gen1 if it was in Gen0, and to Gen2 if it was in Gen1. If the object is on the LOH, so it survives until a full GC is performed twice which, under a "healthy" .NET app, a single full collection is performed approx. 1 in every 100 collections. If there is lots of pressure on the LOH Heap and GC, full GC's will fire more often. This is undesirable for performance reasons since full GC's take much more time to complete. Then there is also a dependency on what kind of GC you're running under and if you are using LatencyModes (be very careful with this). Even if you're running Background GC (this has replaced Concurrent GC in CLR 4.0), the ephemeral collection (Gen0 and Gen1) still blocks/suspends threads.

### Thread Safety
Because the finalizer thread is just a simple thread that runs managed code (it calls the finalizers), it can block and even dead lock.

During collection, the GC suspends all threads, even the finalizer queue. Once it is done, the GC thaws out the threads that it froze, and signals the finalizer thread "hey, you've got work to do". So when the finalizer thread starts running, the threads that were frozen by the GC are starting up again. If a user thread is waiting on a lock taken out by the finalizer thread, and the finalizer thread is waiting on a lock taken out by the user thread, then you've got a deadlock. 

It is vitally important that finalizers do as little work as possible. Finalizers are an extremely complex and dangerous cleanup mechanism and you should avoid them if you possibly can.

Note that a proper implementation of the `Dispose` pattern should call `GC.SuppressFinalize()`   to avoid execution of the Finalizer. The Finalizer thread in the CLR up to and including .NET 4.6 is single-threaded. Reduce the burden on this thread.

The *finalizer* will be called automatically.
-- to confirm
Actually, I don't believe the GC calls Object.Finalize at all if it's not overridden. The object is determined to effectively not have a finalizer, and finalization is suppressed - which makes it more efficient, as the object doesn't need to be on the finalization/freachable queues. – Jon Skeet

## References
https://msdn.microsoft.com/en-us/library/ms973837.aspx
http://stackoverflow.com/questions/7520881/is-it-bad-practice-to-depend-on-the-net-automated-garbage-collector
http://stackoverflow.com/questions/4267729/what-happens-if-i-dont-call-dispose-on-the-pen-object/5555243#5555243
http://dave-black.blogspot.jp/2011/03/how-do-you-properly-implement.html
http://blogs.microsoft.co.il/blogs/sasha/archive/2010/06/30/sta-objects-and-the-finalizer-thread-tale-of-a-deadlock.aspx
https://blogs.msdn.microsoft.com/blambert/2009/07/24/a-simple-and-totally-thread-safe-implementation-of-idisposable/