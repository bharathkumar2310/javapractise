What are Concurrent Collections in Java?

    Concurrent collections are special thread-safe versions of standard Java collections designed to work correctly when multiple threads access or modify them at the same time.

They are part of java.util.concurrent and include things like:

    ConcurrentHashMap
    CopyOnWriteArrayList
    ConcurrentLinkedQueue
    BlockingQueue (like ArrayBlockingQueue, LinkedBlockingQueue)



ConcurrentHashMap :


Problem with HashMap

1. Problem: Race condition (lost updates)

        Two threads update the same key at the same time.

Code (HashMap)
    
    Map<String, Integer> map = new HashMap<>();
    
    map.put("A", 1);
    map.put("A", 2);

Now imagine:
    
    Thread-1 reads value = 1
    Thread-2 reads value = 1
    Thread-1 writes 2
    Thread-2 writes 3

👉 Final value becomes 3, but one update is lost logically.

Why?

put() is NOT atomic:

    read
    compute
    write
    happens in multiple steps



2. Problem: Data corruption during resize (VERY serious)

        HashMap internally resizes when load increases.

What happens in multithreading:

While resizing:

        one thread is moving nodes
        another thread is inserting/updating

👉 This can corrupt the internal linked list / tree structure.

Worst case (older Java versions):
    
    Infinite loop in linked list
    CPU goes to 100%
    Map becomes unusable



3. Iterator is fail-fast (ConcurrentModificationException)
        
        Map<String, Integer> map = new HashMap<>();
        map.put("A", 1);
        map.put("B", 2);
        
        for (String key : map.keySet()) {
        map.put("C", 3); // modification during iteration
        }
Output:
    
    Exception in thread "main" java.util.ConcurrentModificationException
    Why?
    
    HashMap iterator checks a modification counter (modCount).
    If structure changes → it fails fast.


Java 7 used to insert node at first during resizing and element at tail during put() so infinite loop happened
Java 8 made it consistant to add from one end only so no infinfite loop


---------------------------------------------------------------------------------------------------------------------------

Why Collections.synchronizedMap() is NOT enough

    Map<String, Integer> map = Collections.synchronizedMap(new HashMap<>());

    This only adds one global lock

Problem:
    
    synchronized(map) {
    // entire map locked
    }

So:

    only one thread at a time
    performance becomes bottleneck
    iteration still needs manual synchronization


---------------------------------------------------------------------------------------------------------------------------

1. get(key)
   map.get("A");
   Steps
1. Calculate hash
2. Find bucket index
3. Read bucket
4. Traverse list/tree
5. Return value
   Uses
   CAS          ❌
   synchronized ❌

Only volatile reads.

2. put(key, value) (Empty Bucket)
   map.put("A", 10);
   Steps
1. Calculate hash
2. Find bucket index
3. Bucket is null
4. CAS(null → new Node)
5. Done
   Uses
   CAS          ✅
   synchronized ❌
3. put(key, value) (Collision)

Suppose:

A → B → C
Steps
1. Calculate hash
2. Find bucket
3. Bucket not empty
4. Lock bucket
5. Traverse list/tree
6. Update existing key OR append node
7. Unlock bucket
   Uses
   CAS          ❌
   synchronized ✅


Suppose:

map.put("A", 10);
Step 1: Calculate hash
int hash = spread(key.hashCode());

Example:

"A" -> hash = 12345

No lock.
No CAS.

Step 2: Find bucket index

Suppose table length is 16.

index = (length - 1) & hash

Example:

12345 -> Bucket 9

No lock.
No CAS.

Step 3: Read bucket
Node<K,V> f = table[9];

Possible outcomes:

Case A: null
Case B: normal node
Case C: MOVED (resize in progress)
Case A: Bucket is empty
Bucket 9 = null
Step 4A

Try:

CAS(null -> new Node("A",10))
If CAS succeeds
Bucket 9 -> A

Done.

If CAS fails

Another thread inserted first.

Thread A sees null
Thread B inserts
Thread A CAS fails

Thread A retries from the bucket-read step.

Case B: Bucket is not empty

Suppose:

Bucket 9

X -> Y -> Z
Step 4B

Acquire bucket lock.

Conceptually:

synchronized(firstNode) {
...
}

Now no other thread can modify this bucket.

Step 5B

Traverse bucket.

X
↓
Y
↓
Z

Check:

Does key already exist?
Step 6B

If key exists:

Update value

Example:

A=10

becomes

A=20
Step 7B

If key doesn't exist:

Append new node.

Before:

X -> Y -> Z

After:

X -> Y -> Z -> A
Step 8B

Release bucket lock.

Done.

Case C: Bucket contains MOVED

This means:

Resize in progress

The bucket has already been transferred.

Step 4C

Thread helps resize.

Conceptually:

helpTransfer(...)

It may move some buckets.

Step 5C

After helping, retry operation using the new table.

Final Step

After successful insertion:

Update count.

Internally ConcurrentHashMap updates its size-related counters using CAS-style mechanisms.

Then it checks:

Threshold exceeded?

If yes:

Start resize

using CAS on resize control state.

Entire Flow
put()

1. hash calculation
2. bucket index calculation
3. read bucket

   |
   +--> bucket null?
   |         |
   |         +--> CAS insert
   |
   +--> bucket MOVED?
   |         |
   |         +--> help resize
   |
   +--> normal bucket
   |
   +--> synchronized(bucket)
   +--> traverse
   +--> update/insert
   +--> unlock

4. update counters
5. maybe trigger resize

-------------------------------------------------------------------------------------------------------------------------------

Problems with HashMap in Multithreading

Assume:

      Map<Integer, String> map = new HashMap<>();
      HashMap is not thread-safe.

Problem 1: Lost Updates

      Suppose bucket is empty.
      
      Thread A
      map.put(1, "A");
      Thread B
      map.put(2, "B");
      
      Both execute simultaneously.
      
      Internally both threads:
      
      Calculate hash
      Find bucket
      Insert node
      Update size
      
      Since none of this is synchronized:
      
      Thread A updates size
      Thread B updates size
      
      One update can overwrite another.

Result:

      Expected size = 2
      
      Actual size = 1 (possible)

Data corruption.

Problem 2: Two Threads Writing Same Bucket

      Suppose:
      
      Bucket 5
      
      contains:
      
      A

Now:

      Thread A
      put(B)
      Thread B
      put(C)

Both hash to Bucket 5.

Possible sequence:

      A -> B
      
      A -> C

      One insertion disappears.
      
      Final structure becomes inconsistent.

Problem 3: Resize Corruption

      This was the biggest historical issue.
      
      Suppose:
      
      Capacity = 16
      Size = 12
      
      Next insertion triggers resize.
      
      Thread A
      put(1, "A")
      
      Starts resize.
      
      Creates:
      
      New Table (32 buckets)
      Thread B
      put(2, "B")
      
      Also starts resize.
      
      Now both are moving nodes simultaneously.
      
      Result may be:
      
      Lost entries
      Corrupted bucket chains
      Wrong size


What happens during resize?

      HashMap:
      
      Creates a new bucket array.
      Visits every old bucket.
      Recalculates bucket positions.
      Moves nodes to the new array.
      Replaces old table reference with new table.

      Thread A
      
      Creates:
      
      newTableA
      
      Starts moving nodes.
      
      Thread B
      
      Creates:
      
      newTableB
      
      Starts moving nodes.
      
      Now both are modifying the same linked lists from the old table.
      
      This can lead to:
      
      Lost nodes
      Broken links
      Corrupted bucket chains

In older JDKs this could even create infinite loops during traversal.

Problem 4: Read While Write
      Thread A
      map.get(key);
      Thread B
      map.put(key2, value);
      
      at same time.
      
      Reader may see:
      
      Partially updated bucket
      Old value
      Inconsistent state
      
      because visibility is not guaranteed.

Why not simply use synchronizedMap?
      
      Map<Integer,String> map =
      Collections.synchronizedMap(
      new HashMap<>());
      
      This works.
      
      But:
      
      Every operation
      ↓
      Same lock
      
      Example:
      
      Thread A -> get()
      Thread B -> get()
      Thread C -> put()
      
      All compete for one lock.
      
      Only one thread runs at a time.
      
      Poor scalability.


      so synchronizedMap() works like w cretae a common Obj and lock it wherever needed using synchronized(obj)
      whereas hashtavle is ptting synchronized on each method


1. LostUpdate
   Thread A

Suppose bucket is empty
Uses CAS:

      CAS(bucket5, null, NodeA)

Success.

Bucket becomes:

      Bucket5 -> A
Thread B

Now tries:

CAS(bucket5, null, NodeB)

      Fails because bucket is no longer null.
      Then it goes to the locked path and inserts correctly.

Final:

      Bucket5 -> A -> B

No lost update.


Problem 2: Two Threads Writing Same Bucket

      
      Thread A
      
      Finds bucket 5.
      
      Locks bucket head.
      
      Conceptually:
      
      synchronized(bucketHead)
      
      Now:
      
      Bucket5 LOCKED by A
      Thread B
      
      Also wants bucket 5.
      
      Must wait.
      
      A inserts:
      
      A -> B -> C
      
      Unlocks.
      
      B acquires lock.
      
      Sees latest chain:
      
      A -> B -> C
      
      Adds:
      
      A -> B -> C -> D
      
      Final:
      
      A -> B -> C -> D
      
      Nothing lost.

Problem 3: Reads During Writes


Nodes use:

      volatile
      references internally.
      Writer finishes update.
      Then publishes it safely.

Readers see:

      Old valid state
      
      or
      
      New valid state

      Never a partially visible state.
      
      And readers don't need locks.

Problem 4: Resize Corruption

This is the hardest one.

Suppose:

      Capacity = 16

Needs resize to: 32


ConcurrentHashMap

When resize starts:

      Old Table
      ↓
      New Table

Special marker nodes are placed.

Think:

      Bucket already moved
      
      or
      
      Bucket currently moving

If another thread arrives:

Instead of corrupting data:

      It helps transfer buckets
      
      or
      
      Uses the new table

      The resize process is coordinated.
      
      No corruption.



ConcurrentHashMap Resize

The key idea is:

      Only one resize operation is initiated.
      Other threads do not start independent resizes.

Instead they may help complete the same resize.

Step 1: Table becomes full

Suppose:

      Capacity = 16
      Threshold exceeded

A thread doing put() notices this.

      Thread A
      
      starts resize.

Step 2: Create new table
      Old Table (16 buckets)
      
      ↓
      
      New Table (32 buckets)
      
      A special internal structure is created to track the transfer.

Step 3: Mark buckets being transferred

      When a bucket is moved, CHM places a special marker (called a forwarding node).
      
      Conceptually:
      
      Old Bucket 5
      
      A -> B -> C
      
      after transfer:
      
      Bucket 5
      
      [FWD]
      
      meaning:
      
      "This bucket has already been moved."
      Step 4: Another thread arrives
      
      Suppose:
      
      Thread B
      
      does a put().
      
      It notices:
      
      Resize already in progress
      
      Instead of starting another resize:
      
      Thread B helps transfer buckets.
      
      So:
      
      Thread A transfers buckets 0-7
      
      Thread B transfers buckets 8-15
      
      They cooperate.
      
      Step 5: Bucket ownership
      
      Multiple threads do NOT transfer the same bucket simultaneously.
      
      Internally CHM maintains a transfer index.
      
      Conceptually:
      
      transferIndex = 15
      
      Thread A claims:
      
      15
      14
      13
      
      Thread B claims:
      
      12
      11
      10
      
      etc.
      
      Each bucket is transferred exactly once.
      
      Why no corruption?
      
      Because:
      
      Bucket 5
      
      is transferred by exactly one thread.
      
      After transfer:
      
      Bucket 5 -> ForwardingNode
      
      Any other thread seeing it knows:
      
      Already moved.
      
      No second transfer.
      
      What if a get() happens during resize?
      
      Suppose:
      
      map.get(key);
      
      and the bucket contains:
      
      ForwardingNode
      
      The reader understands:
      
      Data moved to new table.
      
      and follows the reference to the new table.
      
      So reads continue working.
      
      What if a put() happens during resize?
      
      If a thread encounters:
      
      ForwardingNode
      
      it knows:
      
      Resize in progress.
      
      and proceeds using the new table or helps transfer.


      In HashMap, during resize, nodes are not copied. The same nodes are moved from old table to new table by changing their next pointers, which can cause unsafe intermediate states in multithreaded access. In ConcurrentHashMap, nodes are also reused, but transfer is coordinated using CAS and forwarding nodes, ensuring that threads either see the old consistent table or are redirected safely to the new table without corruption.
Problem 5: Global Lock Bottleneck

Suppose:

      Collections.synchronizedMap(...)
      Thread A
      
      Works on Bucket 5.
      
      Thread B
      
      Works on Bucket 100.
      
      Even though buckets are unrelated:
      
      Same lock
      
      So B waits.

ConcurrentHashMap
      
      Thread A
      
      Locks Bucket 5.
      
      Thread B
      
      Locks Bucket 100.
      
      Different buckets.
      
      Both proceed simultaneously.
      
      Example:
      
      Bucket5  -> locked by A
      
      Bucket100 -> locked by B
      
      No interference.
      
      Higher throughput.

Problem 6: Check-Then-Act Race

Common bug:

      if(map.get(key) == null) {
      map.put(key, value);
      }
      Thread A
      get() => null
      Thread B
      get() => null

Both insert.

      Race condition.

ConcurrentHashMap

Provides atomic operations:

      map.putIfAbsent(key, value);
      
      or
      
      map.computeIfAbsent(...)
      
      Internally locking/CAS ensures:
      
      Only one thread wins


      map.putIfAbsent("apple", expensiveOperation());
      expensiveOperation() runs before checkiing if Absent
      
      map.computeIfAbsent(
      "apple",
      k -> expensiveOperation()
      );
      expensiveOperation() runs after checkiing if Absent
      
      
      computeIfAbsent() → may use additional synchronization/coordination even when the key is absent, because it must make the whole "check + compute + insert" operation atomic.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


COPY ON WRITE ARRAY LIST :


Problems with ArrayList :


Problem 1: ConcurrentModificationException

Suppose:

      List<Integer> list = new ArrayList<>();

      list.add(1);
      list.add(2);
      list.add(3);   

Thread A
      
      for(Integer x : list) {
      System.out.println(x);
      }

Thread B (at the same time)

      list.add(4);

What happens?

      Thread A starts iterating.
      Thread B modifies the list.
      Iterator detects that the list structure changed.

Result:

      java.util.ConcurrentModificationException

This is the most common issue.


Problem 2: Lost Updates

Suppose the list is:

      [10, 20]

      Thread A
      list.add(30);
      Thread B
      list.add(40);

Both execute simultaneously.

Internally, ArrayList may:

      Read current size = 2
      Store element at index 2
      Increment size

Possible sequence:

      Thread A reads size = 2
      Thread B reads size = 2
      
      Thread A stores 30 at index 2
      Thread B stores 40 at index 2
      
      Thread A increments size
      Thread B increments size

Final list:

      [10, 20, 40]
      
      or some other corrupted state.
      
      One update can overwrite the other.

Problem 3: Data Corruption During Resize

Suppose capacity is full:

      [1, 2, 3]
      
      and capacity = 3.
      
      Thread A
      list.add(4);
      
      Triggers resize.
      
      Thread B
      list.add(5);
      
      Also triggers resize.
      
      Both threads:
      
      Create bigger arrays.
      Copy old elements.
      Replace internal array reference.

The final internal state may become inconsistent because resizing is not synchronized.

This can lead to:

      Missing elements
      Wrong size
      Corrupted data

Problem 4: Reading While Writing

      Suppose:
      
      Thread A
      System.out.println(list.get(5));
      Thread B
      list.remove(0);
      
      simultaneously.
      
      Thread A may:
      
      Read stale data
      Read partially updated data
      Get unexpected exceptions
      
      because no synchronization guarantees visibility.


How copy on Write solves this :

1. ConcurrentModificationException during iteration
   

      Initial array:
      
      Array-A
      [1, 2, 3]
      Thread A
      
      Creates iterator.
      
      Iterator stores reference:
      
      snapshot -> Array-A
      
      Thread B
      
      list.add(4);
      
      Internally:
      
            Lock acquired
      
            Create Array-B
            [1, 2, 3, 4]
            
            list.array = Array-B
            
            Lock released
      
      Now:
      
            Thread A -> Array-A
            [1,2,3]
            
            Current list -> Array-B
            [1,2,3,4]
      
      Thread A keeps reading its snapshot.

Output:

      1
      2
      3

No exception.

2. Lost Updates

      Thread A
      add(30)

Internal:

      Acquire Lock

      Current:
      [10,20]
      
      Create:
      [10,20,30]

Replace reference

      Release Lock
      Thread B
      
      Tries to enter:
      
      add(40)
      
      But lock is held.
      
      Waits.
      
      After A completes:
      
      Current:
      [10,20,30]
      
      B acquires lock.
      
      Creates:
      
      [10,20,30,40]
      
      Replaces reference.
      
      Final:
      
      [10,20,30,40]
      
      No lost update.

3. Resize Corruption

      
      Thread A
      
      Gets lock.
      
      Creates:
      
      [1,2,3,4]
      
      Updates reference.
      
      Releases lock.
      
      Thread B
      
      Gets lock afterwards.
      
      Sees latest array:
      
      [1,2,3,4]
      
      Creates:
      
      [1,2,3,4,5]
      
      Updates reference.
      
      Final:
      
      [1,2,3,4,5]
      
      No corruption.

4. Reader while Writer

      
      Current:
      
      Array-A
      [10,20,30]
      
      Thread A:
      
      reads Array-A
      
      Thread B:
      
      lock
      
      create Array-B
      
      [20,30]
      
      replace reference
      
      unlock
      
      Result:
      
      Thread A still reads Array-A
      
      and gets:
      
      20
      
      safely.
      
      Future readers see:
      
      Array-B
      [20,30]
      
      No partial state is visible.



      for ex : arr  ----> [1, 2, 3]
      
      while reading cretaes snapshot and reference it to the same arr
      
      arr   
                   ----> [1, 2, 3]
      snapshot
      
      {here both arr and snapshot point to same arr)
      
      
      and read happens from sanpshot
      
      If a thread writes it creates new arr
      
      newArr = [1,2,3]{this doesnot points to arr
      
      and add the element 
      new arr = [1,2,3,4]{now still arr and snapshot points to prev array not thi sone)
      
      then this happens arr = new arr
      
      but the prev read will be still happening from the old snapshot 


--------------------------------------------------------------------------------------------------------------------------



