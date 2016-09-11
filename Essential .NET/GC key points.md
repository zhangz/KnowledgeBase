# When 什么时候触发？
Garbage collections occur whenever generation 0 is full.
Partial/full GC的触发条件 - gen 0, gen 1, gen 2, large object heap, GC.Collect(), low memory
OOM的触发条件
减少GC次数的优化策略 - object pool, value type, dispose
mid-life crisis

# What 回收什么？
从gc root搜索不到，而且经过第一次标记、清理后，仍然没有复活的对象
GC roots:
all reference type variables:
- Local variables
- Static variables
- GC handles
- f-reachable queue

# What GC发生时做什么事情？
- Mark
- Sweep
- Compact
Large Object Heap
GC Mode
Suspend other threads？
Finalizer



