When dealing with custom data structures funny things can happen. This is especially true if multiple threads can mutate state and you have several locks playing in the mix. Doing a code review is quite challenging with such code but still some bugs slip through. After quite a lot of debugging the final bug turned out to have nothing to do with multithreading but a problematic IComparable implementation. What will the following small program print as output?

    using System;
    using System.Collections.Generic;
    namespace SortedSetTest
    {
    	class Data : IComparable<Data>
    	{
    		public byte[] Payload
    		{
    			get;
    			set;
    		}
    		public int CompareTo(Data other)
    		{
       			return this.Payload == other.Payload ? 0 : 1;
    		}
    	}
    	class Program
    	{
    		static void Main(string[] args)
    		{
    			var set = new SortedSet<Data>();
    			var payload = new byte[10];
    			var payload2 = new byte[20];
    			var payload3 = new byte[30];
    			var d1 = new Data { Payload = payload };
    			var d2 = new Data { Payload = payload2 };
    			var d3 = new Data { Payload = payload3 };
    			set.Add(d1);
    			set.Add(d2);
    			set.Add(d3);
    			set.Remove(d1);
    			set.Remove(d3);
    			set.Remove(d2);
    			Console.WriteLine($"Final Count: {set.Count}");
    		}
    	}
    }

You can choose from

- 0
- 1
- 2
- 3
- Other
- Exception

And the winner is:

	Final Count: 1

This was unexpected. If you break the rules of the IComparable interface you can get from never terminating sorts up to silent data structure corruption everything. The generic interface description is not particularly helpful but the non generic version spells it out [explicitly](https://msdn.microsoft.com/en-us/library/system.icomparable.compareto.aspx):

*1. A.CompareTo(A) must return zero.
2. If A.CompareTo(B) returns zero, then B.CompareTo(A) must return zero.
3. If A.CompareTo(B) returns zero and B.CompareTo(C) returns zero, then A.CompareTo(C) must return zero.
4. If A.CompareTo(B) returns a value other than zero, then B.CompareTo(A) must return a value of the opposite sign.
5. If A.CompareTo(B) returns a value x not equal to zero, and B.CompareTo(C) returns a value y of the same sign as x, then A.CompareTo(C) must return a value of the same sign as x and y.
6. By definition, any object compares greater than (or follows) null, and two null references compare equal to each other.*

If you break any of the rules you will get undefined behavior which depends entirely on the collection class you use. You must remember to never ever take shortcuts while implementing the IComparable interface. In my case the rule number 4 and 6 were violated.

Proof 4: A,B are non null arrays

- A.CompareTo(B) = 1
- B.CompareTo(A) = 1 Cannot be according to rule 4!

The spirit of this rule is that in order to sort things you need to support the < operator which requires that property. The problem with SortedSet<T> is that it uses a red black tree for data storage. Since the < comparison is broken the tree operations which rely on a working comparison operator can break it subtle ways like sometimes to forget to remove an existing item from the collection.

So how can this be fixed? That all depends on what you treat as equal. If you care only about array sizes and not their contents then your CompareTo method becomes

	public int CompareTo(Data other)
	{
        int lret = 0;
        if( Object.ReferenceEquals(Payload, other.Payload))
        {
        	lret = 0;
        }
        else if (other.Payload == null)
        {
            lret = 1;
        }
        else if (Payload == null)
        {
            lret= -1;
        }
        else
        {
            lret = Payload.Length.CompareTo(other.Payload.Length);
        }
        return lret;
    }

That fixed version will also follow rule 6 for null values which is also good practice. For some time I have thought that SortedSet<T> was broken but as usual the BCL classes are ok but the fault was in our code. How hard can it be to write a comparison method? It turns out there are 6 rules to be followed which are quite a lot for a seemingly simple function.
