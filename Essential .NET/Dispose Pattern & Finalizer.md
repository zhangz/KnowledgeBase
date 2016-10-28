## Finalization
Fortunately for us, most types need only memory to operate. However, some types require more than just memory to be useful; some types require the use of a native resource in addition to memory.

Unmanaged resources are not managed by the CLR or by the garbage collector (such as kernel object handles, database connections, unmanaged memory etc.). Their allocation and deallocation are not governed by GC rules. If a type wrapping a native resource gets GC'd, the GC will reclaim the memory used by the object in the managed heap; but the native resource, which the GC doesn't know anything about, will be leaked.

Freeing unmanaged resources requires an additional feature called **finalization**, which allows an object to execute some code after the object has been determined to be garbage but before the object's memory is reclaimed from the managed heap.​ This automatic mechanism *cannot* be deterministic because it must rely on the garbage collector to discover whether an object is referenced. The GC's non-deterministic nature, in turn, implies that finalization will be **non-deterministic**.

​Only reference type can override the protected `Finalize` method defined by `System.Object` to indicate that it requires automatic finalization. When the garbage collector determines that an object is garbage, it calls the object's `Finalize` method (if it is overridden).

​It only makes sense for reference types to define a finalization behavior, because value types are eligible for garbage collection only when they are boxed. When a value type is allocated on the stack, it is never added to the finalization queue.

There is no guarantee of when finalizers will be called, and indeed if you have enough memory, if it will ever be called. Also, the CLR doesn't make any guarantees as to the order in which `Finalize` methods are called. So you should avoid writing a `Finalize` method that accesses other objects whose type defines a `Finalize` method; those other objects could have been finalized already.

You should only release unmanaged resources in the finalizer, since managed references may be invalid.

It is highly recommended that developers avoid overriding Object’s `Finalize` method. If you are creating a managed type that wraps a native resource, you should first derive a class from a special base class called `System.Runtime.InteropServices.SafeHandle`. The `SafeHandle`-derived classes are extremely useful because they ensure that the native resource is freed when a GC occurs, e.g.`SafeWaitHandle`.

### Finalization and Performance
- Objects that need finalization live longer than objects that do not.
- Objects that need finalization cause collateral damage.
- Objects needing finalization create work for the finalizer thread.

##### Resurrection​
Finalization provides an opportunity for **resurrection**. The primary risk is that other objects referenced by your object might have an invalid state, because their finalizers might have run already.
One of the applicable scenarios for using resurrection is object pooling. Returning the object to the pool can be performed deterministically or *delayed to finalization time, which is a classic scenario for using resurrection.*

### Finalization Internals
When an application creates a new object, the new operator allocates the memory from the heap. If the object’s type defines a `Finalize` method, a pointer to the object is placed on the **finalization queue** just before the type's instance constructor is called. The finalization queue is an internal data structure controlled by the garbage collector. Each entry in the list points to an object that should have its `Finalize` method called before the object's memory can be reclaimed.

Even though `System.Object` defines a Finalize method, the CLR knows to ignore it; that is, when constructing an instance of a type, if the type’s `Finalize` method is the one inherited from `System.Object`, the object **isn't** considered finalizable. One of the derived types must **override** Object’s `Finalize` method.

When a garbage collection occurs, the garbage collector scans the **finalization queue** looking for references to objects which are determined to be garbage. When a reference is found, the reference is removed from the **finalization queue** and appended to the **f-reachable queue**.

The memory occupied by objects, which didn’t have a `Finalize` method, will be reclaimed. However, the memory occupied by objects which have `Finalize` methods couldn't be reclaimed.

A special high-priority CLR thread is dedicated to calling `Finalize` methods. When the **f-reachable** queue is empty (the usual case), this thread sleeps. But when entries appear, this thread wakes, removes each entry from the queue, and then calls each object’s `Finalize` method. In the future, the CLR may use multiple finalizer threads. So you should avoid writing any code that assumes that Finalize methods will be called serially. ​
> Why doesn't the GC execute the object's finalizer instead of deferring the work to an asynchronous thread? The primary risk associated with running finalizers during GC is that finalizers (which are user-defined by their nature) can take a long time to complete, thus blocking the garbage collection process, which in turn blocks all application threads.

The **f-reachable** queue is considered a **root**, just as static fields are roots. So a reference in the f-reachable queue keeps the object it refers to reachable and is not garbage (the object is resurrected).  The garbage collector compacts the reclaimable memory, which promotes the **resurrected** object to an older generation. And now, the special finalization thread empties the freachable queue, executing each object’s Finalize method. The next time the garbage collector is invoked on the older generation, it will see that the finalized objects are truly garbage. The memory for the object is simply reclaimed. 

The bad thing about finalizers is that at least two garbage collections are required to reclaim memory used by objects that require finalization. In reality, more than two collections will be necessary because the objects get promoted to another generation. To make matters worse, everything the object refers to, directly and indirectly, will also remain in memory. 

If the object is on the LOH, it survives until a full GC is performed twice which, under a "healthy" .NET app, a single full collection is performed approx. 1 in every 100 collections. If there is lots of pressure on the LOH Heap and GC, full GC's will fire more often. This is undesirable for performance reasons since full GC's take much more time to complete. Then there is also a dependency on what kind of GC you're running under and if you are using LatencyModes (be very careful with this). Even if you're running Background GC (this has replaced Concurrent GC in CLR 4.0), the ephemeral collection (Gen0 and Gen1) still blocks/suspends threads.

### Thread Safety
The CLR uses a special, high-priority dedicated thread to call Finalize methods. Because the finalizer thread is just a simple thread that runs managed code (it calls the finalizers), it can block and even dead lock.

During collection, the GC suspends all threads, even the finalizer queue. Once it is done, the GC thaws out the threads that it froze, and signals the finalizer thread "hey, you've got work to do". So when the finalizer thread starts running, the threads that were frozen by the GC are starting up again. If a user thread is waiting on a lock taken out by the finalizer thread, and the finalizer thread is waiting on a lock taken out by the user thread, then you've got a deadlock. 

It is vitally important that finalizers do as little work as possible. Finalizers are an extremely complex and dangerous cleanup mechanism and you should avoid them if you possibly can.

Note that a proper implementation of the `Dispose` pattern should call `GC.SuppressFinalize()`   to avoid execution of the Finalizer. The Finalizer thread in the CLR up to and including .NET 4.6 is single-threaded. Reduce the burden on this thread.

The *finalizer* will be called automatically.
-- to confirm
Actually, I don't believe the GC calls Object.Finalize at all if it's not overridden. The object is determined to effectively not have a finalizer, and finalization is suppressed - which makes it more efficient, as the object doesn't need to be on the finalization/freachable queues. – Jon Skeet

## Dispose Pattern
​
As the GC is **non-deterministic**, any object which requires **deterministic** finalization must implement the `IDisposable` interface, with a single `Dispose` method. This method should perform deterministic finalization to release the resources you are using - managed or unmanaged.

The Garbage collecter will never directly execute a `Dispose` method for you. It must be **explicity** called if resources are to be released, such as within a `using` or `try finally` block. Often we can't trust our clients to call Dispose deterministically, and must provide a backup behavior to prevent a resource leak. This can be done using automatic finalization, but brings up a new problem: if the client calls Dispose and later the finalizer is invoked, we will release the resources twice. What we need is a mechanism for instructing the garbage collector that the unmanaged resources have already been released and that automatic finalization is no longer required for a particular object. This can be done using the `GC.SuppressFinalize` method. The object still remains in the finalization queue, but most of the finalization cost is not incurred because the object's memory is reclaimed immediately after the first collection, and it is never seen by the finalizer thread.

> Implementing `IDisposable` provides a way for users of your class to release resources early, instead of waiting for the garbage collector. Note that calling `Dispose` does not delete the managed object from the managed heap.

Calling `Dispose()` on an object provides for the following:
- Reduce strain on the GC for the process.
- Reduce the app's memory pressure.
- Reduce the chance of an OutOfMemoryException (OOM) if the LOH gets fragmented and the object is on the LOH. Rememeber that the LOH is never compacted!
- Keep the object out of the Finalizable and f-reachable Queues if it has a Finalizer.
- Make sure your resources (managed and unmanaged) are cleaned up in a deterministic fashion.

`Dispose()` can have an effect on promotion and reachability.
- You can explicitly remove rooted references by setting the resource to `null`.
- You can unregister/detach event handlers in Dispose(). Remember that these are also rooted references.
- If you don't call `Dispose()` when using a COM object (using `Marshal.ReleaseComObject()`) the memory will be pinned and thus cannot be collected and therefore promoted to the next generation. 
- Calling SuppressFinalize() in a Dispose implementation will stop a Finalizer from being called. If a Finalizer does not need to be called, the object will not be placed into the f-reachable queue, and thus not be promoted since it will take at least another full collection to have it collected.

When defining your own type that implements the `IDisposable` interface, be sure to write code in all of your methods and properties to throw a `System.ObjectDisposedException` if the object has been explicitly cleaned up. A `Dispose` method should never throw an exception; if it's called multiple times, it should just return.

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
//     Dispose(false);
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
It is possible to have multiple threads call `Dispose` on a single object simultaneously. However, the design guidelines state that Dispose does not have to be thread-safe. The reason is because code should be calling Dispose only if the code knows for a fact that no other thread is using the object.

Use `Interlocked.Exchange` on a 'disposed' flag to ensure that one thread's Dispose attempt happens and the other is silently ignored. This's a good starting point. Unfortunately, if one considers what Dispose actually does, things are much more complicated.

**The real purpose of Dispose is not to do something to the object being disposed, but rather to clean up other entities to which that object holds references**. For Dispose to be thread-safe, those other entities would to allow Dispose to clean them up at the same time as other threads might be doing other things with them. Some objects can handle such usage; others cannot.

One particular vexing example: Objects are allowed to have events with RemoveHandler methods that are not thread-safe. Consequently, any Dispose method which cleans up event handlers should only be called from the same thread as the one in which the subscriptions were created.

### Relationship between IDisposable and Memory Management
Calling `Dispose()` does **not** trigger a GC and the GC **never** calls `Dispose()`. `IDisposable` allows for release of **resources** associated with your object (if implemented correctly). `Dispose()` lets the GC know that you are done with this object and its **memory** can be reclaimed at the **next collection for the generation where that object lives**. Calling `Dispose()` has a direct correlation to the work that the GC has to do.

## References
Chapter 21 - Working with Type Requiring Special Cleanup, CLR via C#.
- http://stackoverflow.com/questions/7520881/is-it-bad-practice-to-depend-on-the-net-automated-garbage-collector
- http://stackoverflow.com/questions/4267729/what-happens-if-i-dont-call-dispose-on-the-pen-object/5555243#5555243
- http://dave-black.blogspot.jp/2011/03/how-do-you-properly-implement.html
- http://blogs.microsoft.co.il/blogs/sasha/archive/2010/06/30/sta-objects-and-the-finalizer-thread-tale-of-a-deadlock.aspx
- https://blogs.msdn.microsoft.com/blambert/2009/07/24/a-simple-and-totally-thread-safe-implementation-of-idisposable/
- http://blog.stephencleary.com/2009/08/first-rule-of-implementing-idisposable.html
- https://www.zhihu.com/question/51592470
- https://ericlippert.com/2015/05/18/when-everything-you-know-is-wrong-part-one/
- https://blogs.msdn.microsoft.com/maoni/2004/11/04/clearing-up-some-confusion-over-finalization-and-other-areas-in-gc/
