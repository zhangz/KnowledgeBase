## When - 什么时候触发？
Garbage collections occur whenever generation 0 is full.
Partial/full GC的触发条件 - gen 0, gen 1, gen 2, large object heap, GC.Collect(), low memory
Full GC: Gen0, Gen1, Gen2 and LOH
OOM的触发条件: fragmentation in LOH
减少GC次数的优化策略 - object pool, value type, dispose
mid-life crisis

Small .NET objects are allocated onto the Small Object Heaps (SOH). There are three of these: Generation 0, Generation 1, and Generation 2. Objects move up these Generations based on their age.

New objects are placed on Gen 0. When Gen 0 becomes full, the .NET Garbage Collector (GC) runs, disposing of objects which are no longer needed and moving everything else up to Gen 1. If Gen 1 becomes full the GC runs again, but also moves objects in Gen 1 up to Gen 2.

A full GC run happens when Gen 2 becomes full. This clears unneeded Gen 2 objects, moves Gen 1 objects to Gen 2, then moves Gen 0 objects to Gen 1, and finally clears anything which isn't referenced. After each GC run, the affected heaps are compacted, to keep memory which is still in use together.

Objects larger than 85 KB are allocated onto the Large Object Heap (LOH). 

## What - 回收什么？
从gc root搜索不到，而且经过第一次标记、清理后，仍然没有复活的对象

GC roots - all reference type variables:

- Local variables
- Static variables
- GC handles
- f-reachable queue

## What - GC发生时做什么事情？
- Mark
- Sweep
- Compact
Large Object Heap
GC Mode
Suspend other threads？
Finalizer

## Object Size Limitation
There is a 2GB maximum single object restriction in the .NET CLR, even in 64bit. This is by design.
.NET 4.5 has the option in x64 to explicitly allow objects to be larger than 2GB by setting `gcAllowVeryLargeObjects` in the app.config. With this new option, it is possible to get `UInt32.MaxValue` elements in a multi-dimensional array, though a single dimensional array is still limited to 2,146,435,071 elements (2,147,483,591 for single byte arrays or arrays of a struct containing nothing ut a byte).

The new rules with this option are:

- The maximum number of elements in an array is `UInt32.MaxValue`.
- The maximum index in any single dimension is 2,147,483,591 (0x7FFFFFC7) for byte arrays and arrays of single-byte structures, and 2,146,435,071 (0X7FEFFFFF) for other types.
- The maximum size for strings and other non-array objects is unchanged.



