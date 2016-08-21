## HashSet\<T\> vs. List\<T\>
- HashSet contains only unique members.
- HashSet is optimised for searching (contains, remove) - hash lookups are fast.
- HashSet provides some very useful set operations such as IsSubsetOf and Overlaps, both can be achieved on the other collection types using LINQ but HashSet provides an optimized, ready-made solution
- HashSet doesn't implement `IList` only `ICollection`
- You cannot use indices with a HashSet, only enumerators.
- List has faster speed at adding new items, given that HashSet ensures that are no duplicated, there is much more overhead when dealing with reference types!
- HashSet.Add will skip a new item if it's deemed equal to one of the existing items and return false.
- Dictionary.Add will throw an exception if the new key being added is deemed equal to one of the existing keys. However, if you use the Dictionary‘s indexer instead, it will replace the existing item if the new item is deemed equal to it.
- List.Add will simply add the same item twice.

Use a List if you just need a List to keep track of items, and when you want to iterate through the collection; use a Dictionary if you require hash lookup against some value; use a hash set if you need to perform set operations (e.g. set comparison, determine subset/superset relationship) frequently.

Ref: http://theburningmonk.com/2011/03/hashset-vs-list-vs-dictionary/

## HybridDictionary

Implements `IDictionary` by using a `ListDictionary` while the collection is small, and then switching to a `Hashtable` when the collection gets large.

## Dictionary vs IDictionary 
Avoid enumeration allocation via interface. Enumerating Dictionary doesn't allocate; however enumerating via the IDictionary interface causes a lot of extra allocations. When accessed via the interface, you're using the interface method that's typed to return `IEnumerator<KeyValuePair<TKey,TValue>>` rather than `Dictionary<TKey, TValue>.Enumerator`, so the struct gets boxed.

http://mattwarren.org/2016/02/17/adventures-in-benchmarking-memory-allocations/

## Collections

| Collection | Details | Insertion Time | Deletion Time | Lookup Time | Sorted | Index Access |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| List\<T\> | Automatically resizable Array | Amortized O(1) | O(n) | O(n) | No | Yes |
| LinkedList\<T\> | Doubly-linked list | O(1) | O(1) | O(n) | No | No |
| Dictionary\<K,V\> | Hash table | O(1) | O(1) | O(1) | No | No |
| HashSet\<T\> | Hash table | O(1) | O(1) | O(1) | No | No |
| Queue\<T\> | Automatically resizable cyclic array | Amortized O(1) | O(1) | -- | No | No |
| Stack\<T\> | Automatically resizable array | Amortized O(1) | O(1) | -- | No | No |
| SortedDictionary\<K,V\> | Red-black tree | O(log n) | O(log n) | O(log n) | Yes (keys) | No |
| SortedList\<K,V\> | Sorted resizable array | O(n)* | O(n) | O(log n) | Yes (keys) | Yes |
| SortedSet\<T\> | Red-black tree | O(log n) | O(log n) | O(log n) | Yes | No |
*\* Unless the data are inserted in sort order, in which case O(1).*

The implementation details of some collections:

- Arrays are highly inflexible, non-resizable, and make it difficult to implement some operations efficiently, but they are known for having the most minimal overhead of any other collection implementation.
- Iterating a `List<T>` in a `foreach` loop takes slightly longer than in a `for` loop, because `foreach` enumeration must verify that the underlying collection hasn’t been changed throughout the loop.
- Internal collection layout affects cache performance. 
- The `SortedSet<T>` is implemented in terms of a binary search tree with n nodes for n elements. The `List<T>` classes is implemented in terms of a contiguous array of n elements. On a 32-bit system, storing n value types of size s in a sorted set requires (20 + s)n bytes of storage, whereas the list requires only sn bytes.
- Some collections have additional requirements of their elements to achieve satisfactory performance. For example, any implementation of a hash table requires access to a good hash code for the hash table’s elements.
- Amortized O(1) cost collections, when executed well, are hardly distinguishable from true O(1) cost collections. `List<T>.Add` may sometimes incur a significant memory allocation cost and run for a duration of time that is linear in the number of list elements.
- The ubiquitous space-time tradeoff is present in collection design and even the choice of which collections to include in the .NET Framework. `SortedList<K,V>` offers very compact and sequential storage of elements at the expense of linear time insertion and deletion, whereas `SortedDictionary<K,V>` occupies more space and is non-sequential, but offers logarithmic bounds on all operations.
- strings are also a simple collection type — a collection of characters. Internally, the `System.String` class is implemented as an immutable, non-resizable array of characters. All operations on strings result in the creation of a new object. This is why creating long strings by concatenating thousands of smaller strings together is extremely inefficient.

## Concurrent Collections

| Collection | Most Similar To | Details | Synchronization |
| --- | --- | --- | --- |
| ConcurrentStack\<T\> | Stack\<T\> | Singly linked list | Lock-free (CAS), exponential backoff when spinning |
| ConcurrentQueue\<T\> | Queue\<T\> | Linked list of array segments (32 elements each) | Lock-free (CAS), brief spinning when dequeueing an item that was just enqueued |
| ConcurrentBag\<T\> | -- |  Thread-local lists, work stealing | Usually none for local lists, Monitor for work stealing |
| Concurrent Dictionary\<K,V\> | Dictionary\<K,V\> | Hash table: buckets and linked lists |  <div>For updates: Monitor per group of hash table buckets (independent of other buckets)</div> <div>For reads: none</div> |

- The `ConcurrentQueue<T>` class manages a linked list of array segments, which allows it to emulate an unbounded queue with a bounded amount of memory. Enqueuing or dequeuing items involves only incrementing pointers into array segments. Synchronization is required in several locations, e.g., to ensure that items are not dequeued before the enqueuing thread has completed the enqueue operation. However, all synchronization is CAS-based. 
-  The `ConcurrentBag<T>` class manages a list of items in no specific order. Items are stored in thread-local lists; adding or removing items to a thread-local list is done at the head of the list and usually requires no synchronization. When threads have to steal items from other threads’ lists, they steal from the tail of the list, which causes contention only when there are fewer than three items in the list. 
-  The `ConcurrentDictionary<K,V>` uses a classic hash table implementation with chaining (linked lists hanging off each bucket). Locking is managed at the bucket level—all operations on a certain bucket require a lock, of which there is a limited amount determined by the constructor’s concurrencyLevel parameter. Operations on buckets that are associated with different locks can proceed concurrently with no contention. Finally, all read operations do not require any locks, because all mutating operations are atomic (e.g., inserting a new item into the list hanging off its bucket).

## Cache Considerations
Choosing the right collection is not only about its performance considerations. The way data are laid out in memory is often more critical to CPU-bound applications than any other criterion, and collections affect this layout greatly.

The main factor behind carefully examining data layout in memory is the CPU cache.
- Cache line 
- Cache hit
- Cache miss
- cache miss ratio

## Implement IEnumerable\<T\>
Naïvely implementing `IEnumerable<T>` on your collection sentences your callers to pay the price of interface method invocation when they enumerate it. Consider the following code snippet, which enumerates a `List<int>`:

```csharp
List<int> list = ...; 
IEnumerator<int> enumerator = list.GetEnumerator(); 
long product = 1; 
while (enumerator.MoveNext()) {
	product *= enumerator.Current; 
}
```

There are two interface method invocations per iteration here (MoveNext, Current), which is an unreasonable overhead for traversing a list and finding the product of its elements. Inlining interface method invocations is not trivial, and if the JIT fails to inline them successfully, the price is steep.

There are several approaches that can help avoid the cost of interface method invocation:
Interface methods, when invoked directly on a value type variable, can be dispatched directly. Therefore, if the enumerator variable in the above example had been a value type (and not `IEnumerator<T>`), the interface dispatch cost would have been prevented. This can only work if the collection implementation could return a value type directly from its `GetEnumerator` method, and the caller would use that value type instead of the interface. 

To achieve this, `List<T>` has an explicit interface implementation of `IEnumerable<T>.GetEnumerator`, which returns `IEnumerator<T>`, and another public method called `GetEnumerator`, which returns `List<T>.Enumerator` — an inner value type:

```csharp
public class List<T> : IEnumerable<T>
{ 
	public Enumerator GetEnumerator() {
		return new Enumerator(this); 
	} 
	
	IEnumerator<T> IEnumerable<T>.GetEnumerator() {
		return new Enumerator(this); 
	}
	...
	public struct Enumerator : IEnumerator<T> { ... }
}
```

An alternative would be to make the enumerator a reference type, but repeat the same explicit interface implementation trick on its MoveNext method and Current property. This would also allow callers using the class directly to avoid interface invocation costs.


