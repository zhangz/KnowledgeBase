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