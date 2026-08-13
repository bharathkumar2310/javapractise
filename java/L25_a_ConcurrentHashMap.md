# ConcurrentHashMap (Java 8+) - Complete Revision Notes

---

# What is ConcurrentHashMap?

`ConcurrentHashMap` is a **thread-safe implementation of the Map interface**.

It allows **multiple threads** to perform read and write operations concurrently with high performance.

Package:

```java
java.util.concurrent.ConcurrentHashMap
```

---

# Why not HashMap?

HashMap is **not thread-safe**.

Example

```java
Map<Integer, String> map = new HashMap<>();

Thread T1:
map.put(1, "A");

Thread T2:
map.put(2, "B");
```

Possible problems

- Race condition
- Lost updates
- Corrupted internal structure (especially during resize)
- Infinite loop (before Java 8)
- Inconsistent reads

---

# One Solution

```java
Collections.synchronizedMap(new HashMap<>());
```

Problem

Entire map is locked.

```
Thread 1
    │
    ▼
 Entire Map Locked
    │
Thread 2 waits
Thread 3 waits
Thread 4 waits
```

Only one thread works at a time.

Poor scalability.

---

# Better Solution

```
ConcurrentHashMap
```

It allows

- Multiple readers simultaneously
- Multiple writers on different buckets
- Much less locking

Higher throughput.

---

# Internal Structure (Java 8)

Internally it is still based on

```
Array of Buckets
```

Each bucket contains

- Linked List
- Red-Black Tree (after treeification)

Exactly like HashMap.

```
Table

Bucket0

Bucket1

Bucket2

↓

Node

↓

Node

↓

TreeNode
```

Difference is synchronization.

---

# Java 7 vs Java 8

## Java 7

Used

```
Segments
```

```
ConcurrentHashMap

↓

Segment[]

↓

HashEntry[]
```

Each segment had one lock.

Only one thread per segment.

---

## Java 8

Segments removed.

Uses

```
Node[]
```

Each bucket is synchronized independently.

Much better concurrency.

---

# Internal Node

Simplified

```java
static class Node<K,V> {

    final int hash;

    final K key;

    volatile V value;

    volatile Node<K,V> next;
}
```

Notice

```
volatile
```

Used for visibility.

---

# put()

Example

```java
map.put("A", 100);
```

Flow

```
hashCode()

↓

spread()

↓

Bucket Index

↓

Bucket Empty?

↓

YES

↓

CAS Insert

↓

Done
```

If bucket already contains nodes

```
Acquire lock on first node

↓

Traverse

↓

Update/Add

↓

Release lock
```

Only that bucket is locked.

Entire map is NOT locked.

---

# CAS (Compare-And-Swap)

First insertion uses

```
CAS
```

instead of locking.

CAS means

```
Current value

↓

Still null?

↓

YES

↓

Insert

↓

Done

NO

↓

Retry
```

No lock needed.

Very fast.

---

# What is CAS?

CPU instruction.

```
Expected Value

↓

Current Value

↓

Same?

↓

YES

↓

Replace

↓

Success

NO

↓

Failure
```

Used heavily in concurrent programming.

---

# Why CAS?

Without CAS

```
Thread1

↓

Lock

↓

Insert

↓

Unlock
```

With CAS

```
Thread1

↓

Atomic Hardware Instruction

↓

Done
```

Much faster.

---

# Read Operation

```
get()
```

Never locks.

Flow

```
hashCode()

↓

spread()

↓

Bucket

↓

Traverse

↓

Return Value
```

Reads are almost lock-free.

---

# Write Operation

```
put()

remove()

replace()
```

Lock only one bucket.

Not entire table.

---

# Why use volatile?

Example

```
Thread1

map.put(1,"ABC")
```

Without volatile

Thread2 may still see

```
null
```

because of CPU cache.

Using

```java
volatile V value;
```

guarantees visibility.

---

# spread()

Instead of directly using

```java
hashCode()
```

ConcurrentHashMap does

```java
spread(hash)
```

Simplified

```java
hash ^ (hash >>> 16)
```

Exactly like HashMap.

Purpose

Mix higher bits into lower bits.

Better bucket distribution.

---

# Bucket Index

Computed as

```java
(hash & (table.length - 1))
```

Exactly same as HashMap.

---

# Treeification

If bucket length becomes

```
>=8
```

and table size

```
>=64
```

Linked list

↓

Red Black Tree

Lookup

```
O(n)

↓

O(log n)
```

---

# Resizing

When load factor exceeded

```
Resize

↓

Capacity ×2
```

Unlike HashMap

Multiple threads help in resizing.

Uses

```
ForwardingNode
```

to indicate bucket already moved.

---

# ForwardingNode

Suppose resize starts.

Old bucket

```
Bucket5

↓

ForwardingNode
```

Other threads see

```
ForwardingNode

↓

Go to New Table
```

Allows cooperative resizing.

---

# Synchronization

Only bucket locked.

```
Bucket1

LOCKED

Bucket2

FREE

Bucket3

FREE
```

Another thread can work on Bucket2.

Much faster than synchronizedMap.

---

# Null Keys?

```
HashMap

↓

1 null key

Multiple null values
```

```
ConcurrentHashMap

↓

No null key

No null value
```

Reason

```
map.get(key)

↓

null
```

Could mean

```
Key absent

OR

Stored value is null
```

Concurrent operations cannot distinguish.

So nulls are prohibited.

---

# Atomic Methods

## putIfAbsent()

```java
map.putIfAbsent("A",100);
```

Only inserts if absent.

---

## replace()

```java
map.replace("A",200);
```

Atomic replacement.

---

## remove(key,value)

```java
map.remove("A",100);
```

Removes only if value matches.

---

## compute()

```java
map.compute(key,
(k,v)->v+1);
```

Atomic update.

---

## computeIfAbsent()

```java
map.computeIfAbsent(
id,
k->new User()
);
```

Very common.

Only one thread computes.

---

## merge()

```java
map.merge(
key,
1,
Integer::sum
);
```

Useful for counters.

---

# Iterators

HashMap

```
Fail Fast
```

ConcurrentHashMap

```
Weakly Consistent
```

Means

No

```
ConcurrentModificationException
```

Iterator may or may not reflect latest updates.

---

# Size()

```
size()
```

May not always be perfectly accurate during heavy concurrent updates.

Eventually becomes correct.

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| get() | O(1) Average |
| put() | O(1) Average |
| remove() | O(1) Average |
| Tree Lookup | O(log n) |
| Resize | O(n) |

---

# HashMap vs ConcurrentHashMap

| Feature | HashMap | ConcurrentHashMap |
|----------|----------|------------------|
| Thread Safe | ❌ | ✅ |
| Null Key | ✅ One | ❌ |
| Null Value | ✅ | ❌ |
| Locking | None | Bucket Level |
| Reads | Unsafe | Mostly Lock Free |
| Writes | Unsafe | Fine Grained Lock |
| Treeification | ✅ | ✅ |
| Resize | Single Thread | Cooperative |
| Iterator | Fail Fast | Weakly Consistent |

---

# synchronizedMap vs ConcurrentHashMap

| synchronizedMap | ConcurrentHashMap |
|-----------------|------------------|
| Entire map locked | Bucket level locking |
| Poor scalability | High scalability |
| Reads also synchronized | Reads mostly lock-free |
| Slower | Faster |

---

# Interview Questions

## Why ConcurrentHashMap?

To allow concurrent access with high performance while maintaining thread safety.

---

## Why no null keys?

Because

```
map.get(key)

↓

null
```

cannot distinguish

- Key absent
- Value is null

during concurrent execution.

---

## Does ConcurrentHashMap lock entire map?

No.

Only the bucket being modified is locked.

---

## Is get() synchronized?

No.

Reads are mostly lock-free.

---

## Does it use Red-Black Tree?

Yes.

After

```
Bucket Size >=8

AND

Capacity >=64
```

---

## Does it use hashCode()?

Yes.

Flow

```
hashCode()

↓

spread()

↓

Bucket Index

↓

Bucket

↓

Node

↓

Value
```

---

## Java 7 vs Java 8

Java 7

```
Segment Locking
```

Java 8

```
Bucket Level Locking

+

CAS

+

volatile

+

ForwardingNode
```

---

# Complete Internal Flow

```
put(key,value)

        │
        ▼
hashCode()
        │
        ▼
spread(hash)
        │
        ▼
Bucket Index
        │
        ▼
Bucket Empty?
   │            │
  Yes          No
   │            │
 CAS Insert   Lock Bucket
   │            │
   ▼            ▼
 Done     Traverse Nodes
               │
               ▼
         Insert/Update
               │
               ▼
        Treeify if needed
               │
               ▼
             Done
```

---

# Important Numbers

| Property | Value |
|----------|-------|
| Default Capacity | 16 |
| Load Factor | 0.75 |
| Treeify Threshold | 8 |
| Untreeify Threshold | 6 |
| Min Capacity For Treeify | 64 |

---

# Must Remember

- Thread-safe Map implementation
- Uses bucket-level locking (Java 8+)
- Uses CAS for first insertion
- Uses volatile for visibility
- Reads are mostly lock-free
- Uses Red-Black Tree after threshold
- Uses spread(hash) to improve distribution
- No null keys or null values
- Weakly consistent iterators
- Better performance than Collections.synchronizedMap()