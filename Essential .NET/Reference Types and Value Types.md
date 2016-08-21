# Differences Between Value Types and Reference Types

Reference types in .NET include classes, delegates, interfaces, and arrays.
Value types in .NET include structs and enums. The primitive types, such as int, float, decimal, are value types. Developers are free to define custom value types using the `struct` keyword.

## Semantic Differences
Reference types enjoy *reference semantics*, where the identity of the object is considered before its content.
Value types enjoy *value semantics*, where objects don't have an identity, are not accessed through references, and are treated according to their content.


| Criteria | Reference Types | Value Types |
| --- | --- | --- |
| Passing an object to a method | Only the reference is passed; changes propagate to all other references | The object’s content are copied into the parameter (unless using the ref or out keywords); changes do not affect any code outside of the method |
| Assigning a variable to another variable | Only the reference is copied; two variables now contain references to the same object | The contents are copied; the two variables contain identical copies of unrelated data |
| Comparing two objects using operator== | The references are compared; two references are equal if they reference the same object | The contents are compared; two objects are equal if their content is identical on a field-by-field level |

## Storage, Allocation, and Deallocation Differences
Reference types are allocated exclusively from the managed heap, an area of memory managed by the .NET garbage collector.
Stand-alone value types are usually allocated from the stack of the executing thread. However, value types can be embedded in reference types, in which case they are allocated on the heap, and can be boxed, transferring their storage to the heap.

#### Differences Between Stack and Heap
There isn’t that much of a difference between stacks and heaps in a .NET process. Stacks and heaps are nothing more than ranges of addresses in virtual memory, and there is no inherent advantage in the range of addresses reserved to the stack of a particular thread compared to the range of addresses reserved for the managed heap. Accessing a memory location on the heap is neither faster nor slower than accessing a memory location on the stack.
There are several considerations that might, in certain cases, support the claim that memory access to stack locations is faster, overall, than memory access to heap locations. Among them:

- On the stack, temporal allocation locality (allocations made close together in time) implies temporal access locality (objects allocated together are accessed together), the sequential stack storage tends to perform better with respect to CPU caches and operating system paging systems.
- Memory density on the stack tends to be higher than on the heap because of the reference type overhead. Higher memory density often leads to better performance, e.g., because more objects fit in the CPU cache.
- Thread stacks tend to be fairly small – the default maximum stack size on Windows is 1MB, and most threads tend to actually use only a few stack pages. On modern systems, the stacks of all application threads can fit into the CPU cache, making typical stack object access extremely fast. (Entire heaps, on the other hand, rarely fit into CPU caches.)

With that said, you should not be moving all your allocations to the stack! Thread stacks on Windows are limited, and it is easy to exhaust the stack by applying injudicious recursion and large stack allocations.

## Reference Type Internals

### Memory Density Overhead
On 32-bit systems, objects in the heap are aligned to the nearest 4 byte multiple. This implies that an object with only a single byte member would still occupy 12 bytes in the heap, due to alignment (in fact, even a class with no instance fields will occupy 12 bytes when instantiated). There are several differences where 64-bit systems are introduced. First, the method table pointer field (a pointer that it is) occupies 8 bytes of memory, and the sync block index takes 8 bytes as well. Second, objects in the heap are aligned to the nearest 8 byte multiple. This implies that an object with only a single byte member in a 64-bit heap would occupy a whopping 24 bytes of memory.

### Method Table

### Dispatching Non-virtual Methods
For non-virtual methods, there is no need to use the method table for method dispatch: the code address of the invoked method is known when the JIT compiles the method dispatch.

Method inlining: method calls to small or simple methods are replaced by the method’s body. The performance difference between inlined and non-inlined method calls can be vast (less instructions), especially when the methods are very simple.

### Dispatching Virtual Methods
It is necessary to use the method table for virtual method dispatching. The only way to bind to the right method is to determine at runtime the actual type of the object referenced by the variable, and dispatch the virtual method based on that type information. This is exactly what the method table enables the JIT compiler to do.

Virtual methods *prevent* inlining because inlining can occur only when the compiler knows at compile-time (in the case of the JIT compiler, at JIT-time) which method is about to be invoked.

A class or method is `sealed` can be used to effectively remove the need for virtual method dispatch.

### Dispatching Static and Interface Methods
Dispatching static methods is fairly easy: there is no need to load an object reference, and simply calling the method would suffice. Because the invocation does not proceed through a method table, the JIT compiler uses the same trick as for non-virtual instance method: the method dispatch is indirect through a special memory location which is updated after the method is JIT compiled.

The JIT uses several tricks to effectively inline interface methods.

### Sync Blocks And The lock Keyword

## Value Type Internals
Value types exhibit superb memory density, which makes them a great candidate for large collections, but are not equipped with features required of objects such as polymorphism, synchronization support(lock), and reference semantics.

The primary reason for using value types is their excellent memory density and lack of overhead.

Customize value type layout: `StructLayout` and `FieldOffset`.

`System.ValueType` is a reference type. Value types are types derived from `System.ValueType`. It overrides the Equals and GetHashCode virtual methods inherited from `System.Object`.

### Virtual Methods on Value Types
The CLR forbids inheritance relationships between value types, making it impossible to define new virtual methods on value types. However, value types come equipped with virtual methods inherited from `System.Object`. There are several of them: Equals, GetHashCode, ToString, and Finalize. It means that given an instance of a value type, we should be able to dispatch the virtual method successfully, even though it doesn’t have a method table pointer!

### Boxing
A typical interface method dispatch requires the object’s method table pointer, which would solicit boxing where value types are concerned.
A conversion from a value type instance to an interface type variable requires boxing.

Using value types through interfaces raises a potential concern if the value types are mutable. As always, modifying the boxed copy will not affect the original, which can lead to unexpected behavior.
The common recommendation to make value types immutable, and allow modification only by making more copies.

### The GetHashCode Method
Several properties from the `GetHashCode` method:

1. If two objects are equal, their hash codes are equal.
2. If two objects are not equal, it should be unlikely that their hash codes are equal.
3. `GetHashCode` should be fast (although often it is linear in the size of the object).
4. An object’s hash code should not change.

The reasoning behind property (4) is as follows: suppose you have the point (5, 5) and embed it in a hash table, and further suppose that its hash code was 10. If you modify the point to (6, 6) – and its hash code is also modified to 12 – then you will not be able to find in the hash table the point you inserted to it. But this is not a concern with value types, because you cannot modify the object you inserted into the hash table – the hash table stores a copy of it, which is inaccessible to your code. With reference types, content-based equality becomes a concern.

The CLR provides a default `GetHashCode` implementation for reference types that rely on the object’s identity as their equality criterion.
Reference types using the default `Equals` and `GetHashCode` implementation need not concern themselves with any of the four properties stressed above – they get them for free. However, if your reference type should choose to override the default equality behavior (this is what System.String does, for example), then you should consider making your reference type immutable if you use it as a key in a hash table.

### Best Practices for Using Value Types
- Use value types if your objects are small and you intend to create a great many of them.
- Use value types if you require high-density memory collections.
- Override Equals, overload Equals, implement `IEquatable<T>`, overload operator ==, and overload operator != on your value types.
- Override GetHashCode on your value types.
- Consider making your value types immutable.


