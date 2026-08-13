# HashMap Internal Working (Java) - Complete Revision Notes

---

# 1. What is HashMap?

A **HashMap** is a data structure that stores data as **key-value pairs**.

```java
HashMap<Integer, String> map = new HashMap<>();

map.put(101, "John");
map.put(102, "Alice");

System.out.println(map.get(101)); // John
```

Characteristics

- Stores key-value pairs 
- Keys are unique
- Values can be duplicated
- Allows one null key
- Allows multiple null values
- Not synchronized
- Average Time Complexity
    - put() → O(1)
    - get() → O(1)
    - remove() → O(1)

Worst Case

- O(n) (Linked List)
- O(log n) (Red Black Tree after treeification)

---

# 2. Internal Structure

Internally HashMap stores an array called **table**.

```
table
  |
  +------------------------------+
  | Bucket 0                     |
  | Bucket 1                     |
  | Bucket 2                     |
  | Bucket 3                     |
  | Bucket 4                     |
  | ...                          |
  +------------------------------+
```

Each position in the array is called a **Bucket**.

Each bucket can contain

- Nothing
- One Node
- Linked List
- Red Black Tree

---

# 3. Node Structure

Each bucket stores Node objects.

Java's internal Node class

```java
static class Node<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;
}
```

Each node contains

```
hash
key
value
next pointer
```

Visual

```
Node

+----------------+
| hash           |
| key            |
| value          |
| next ----------> next node
+----------------+
```

---

# 4. Complete put() Flow

When

```java
map.put("Apple", 100);
```

HashMap performs

```
key
 ↓
hashCode()
 ↓
hash()
 ↓
bucket index
 ↓
Go to bucket
 ↓
Empty?
      Yes → Insert
      No
        ↓
Compare keys
        ↓
Equal?
      Yes → Replace value
      No
        ↓
Collision
        ↓
Linked List
        ↓
Tree if required
```

---

# 5. hashCode()

Every Java object inherits

```java
public int hashCode()
```

Purpose

Produces an integer representing the object.

Example

```java
String s = "Apple";

System.out.println(s.hashCode());
```

Possible Output

```
63476538
```

Important

HashCode

- can be negative
- can be positive
- not unique

Two different objects **can have same hashCode**

Example

```
"Aa"
"BB"
```

Both produce same hashCode.

This is called

## Collision

---

# 6. Why not use hashCode directly?

hashCode can be

```
423456789

-19384723

983742234
```

HashMap may only have

```
16 buckets
```

Need to convert huge integer into bucket number.

---

# 7. hash()

Java doesn't directly use hashCode.

It first improves the hash.

Implementation

```java
h ^ (h >>> 16)
```

Example

```
hashCode

101011011001010101

↓

Shift right

000000001010110110

↓

XOR

101011010011111011
```

Purpose

Better distribution.

Avoids clustering.

---

# 8. Bucket Index Calculation

Formula

```
bucket index

=

(capacity - 1) & hash
```

Not

```
hash % capacity
```

Because

Bitwise AND is much faster.

Example

Capacity

```
16
```

Capacity -1

```
15

1111
```

Hash

```
1011010110
```

```
1111
AND
1011010110

↓

0110

↓

Bucket 6
```

---

# 9. Bucket

A bucket is simply one position in the table array.

```
Bucket 0

Bucket 1

Bucket 2

Bucket 3

Bucket 4
```

Each bucket stores

```
Node

OR

Linked List

OR

Tree
```

---

# 10. Collision

Collision happens when

Two different keys map to same bucket.

Example

```
HashMap Capacity

16

Both keys

↓

Bucket 5
```

```
Bucket 5

John

↓

Alice

↓

Bob
```

---

# 11. Why Collision Happens?

Because

Infinite possible keys

↓

Limited buckets

↓

Eventually two keys share same bucket.

Impossible to avoid completely.

Good hash functions only reduce collisions.

---

# 12. Collision Resolution

Java HashMap uses

## Separate Chaining

Meaning

Each bucket stores a linked list.

Example

```
Bucket 3

↓

John

↓

Alice

↓

Bob

↓

Mike
```

Searching

Compare

```
hash

↓

equals()

↓

next

↓

equals()

↓

next
```

---

# 13. equals()

Suppose

```
hashCode

↓

Same bucket
```

Need to know

Same object?

or

Different object?

HashMap calls

```java
equals()
```

Example

```java
String a = new String("Java");

String b = new String("Java");

System.out.println(a == b);
```

Output

```
false
```

But

```java
System.out.println(a.equals(b));
```

Output

```
true
```

HashMap checks

```
hash same?

↓

equals()

↓

Replace

or

Insert
```

---

# 14. hashCode() + equals()

Always remember

```
hashCode()

↓

Find bucket

↓

equals()

↓

Find exact key
```

Never

```
equals()

↓

bucket
```

---

# 15. Complete Search Process

```
map.get(key)

↓

hashCode()

↓

hash()

↓

bucket index

↓

Go to bucket

↓

Compare hash

↓

Compare equals()

↓

Found

↓

Return value
```

---

# 16. Why equals after hash?

Example

```
Bucket 5

↓

John

↓

Alice

↓

Bob
```

All are inside same bucket.

Need equals()

Otherwise cannot identify exact key.

---

# 17. Capacity

Capacity means

Number of buckets.

Default

```
16
```

```
Buckets

0

1

2

...

15
```

Capacity is always

```
Power of 2
```

Examples

```
16

32

64

128

256
```

Never

```
17

29

41
```

Reason

Efficient bucket calculation using bitwise AND.

---

# 18. Load Factor

Load factor decides

"When should HashMap resize?"

Default

```
0.75
```

Formula

```
Load Factor

=

Number of Entries

/

Capacity
```

Example

Capacity

```
16
```

Entries

```
12
```

Load factor

```
12 / 16

=

0.75
```

---

# 19. Threshold

Threshold determines

"When resizing happens."

Formula

```
Threshold

=

Capacity × Load Factor
```

Default

```
Capacity

16

×

0.75

=

12
```

Meaning

After inserting 12 entries

Next insertion triggers resize.

---

# 20. Resizing (Rehashing)

Suppose

```
Capacity

16

Threshold

12
```

Insert

```
13th element
```

HashMap

```
Creates new array

↓

Capacity doubles

↓

32 buckets

↓

Recalculate bucket

↓

Move every node
```

---

# 21. Why Rehash?

Because

Bucket index depends on capacity.

Old

```
index

=

15 & hash
```

New

```
31 & hash
```

Bucket changes.

Every node must be redistributed.

---

# 22. Cost of Resizing

Time Complexity

```
O(n)
```

Because every element is moved.

Fortunately

Resize doesn't happen often.

---

# 23. Treeification

If too many collisions occur

Linked list becomes slow.

Java converts linked list into

```
Red Black Tree
```

Conditions

```
Bucket size >= 8

AND

Capacity >= 64
```

Otherwise

HashMap prefers resizing instead.

---

# 24. Why Tree?

Searching in Linked List

```
O(n)
```

Searching in Red Black Tree

```
O(log n)
```

Example

Linked List

```
John

↓

Alice

↓

Bob

↓

Mike

↓

Steve

↓

David
```

Tree

```
         John
       /      \
    Alice      Mike
    /   \      /   \
 Bob Steve  Tom  David
```

---

# 25. Untreeification

If elements become few

Tree converts back into Linked List.

Condition

```
Bucket size <= 6
```

---

# 26. Complete put() Internal Algorithm

```
put(key, value)

↓

key.hashCode()

↓

hash()

↓

bucket index

↓

Bucket empty?

│
├── Yes
│      Insert Node
│
└── No
       │
       Compare hash
       │
       Compare equals
       │
       Same key?
       │
       ├── Yes
       │      Replace value
       │
       └── No
              Add node
              │
              Bucket size >= 8?
              │
              ├── Yes
              │     Capacity >=64?
              │
              │     ├── Yes
              │     │      Treeify
              │     │
              │     └── No
              │            Resize
              │
              └── No
                     Continue
```

---

# 27. Complete get() Internal Algorithm

```
get(key)

↓

hashCode()

↓

hash()

↓

bucket index

↓

First Node

↓

hash matches?

↓

equals() matches?

↓

Yes

↓

Return value

↓

Else

↓

Next Node

↓

Repeat

↓

Not Found

↓

Return null
```

---

# 28. Time Complexity

| Operation   | Average   | Worst                            |
|-------------|-----------|----------------------------------|
| put         | O(1)      | O(n) linked list / O(log n) tree |
| get         | O(1)      | O(n) linked list / O(log n) tree |
| remove      | O(1)      | O(n) linked list / O(log n) tree |
| containsKey | O(1)      | O(n) linked list / O(log n) tree |
| resize      | O(n)      | O(n)                             |

---

# 29. Important Interview Points

### Why capacity is always power of 2?

So bucket index can be calculated using:

```text
(capacity - 1) & hash
```

instead of `%`, making it faster and ensuring better distribution.

---

### Why use hash() instead of hashCode() directly?

To mix high-order bits into low-order bits using:

```java
h ^ (h >>> 16)
```

This improves bucket distribution and reduces collisions.

---

### Why call equals() after hashCode()?

- `hashCode()` narrows down the bucket.
- `equals()` identifies the exact key within that bucket.

---

### Can two unequal objects have the same hashCode()?

**Yes.**

This is called a **collision**.

---

### Can two equal objects have different hashCodes()?

**No.**

If:

```java
a.equals(b) == true
```

then:

```java
a.hashCode() == b.hashCode()
```

must also be true.

---

### Can two objects have the same hashCode() but different equals()?

**Yes.**

HashMap handles this using separate chaining (linked list) or treeification.

---

### When does HashMap resize?

When:

```
number of entries > threshold
```

where:

```
threshold = capacity × load factor
```

---

### Default Values

| Property                           | Value |
|------------------------------------|-------|
| Initial Capacity                   | 16    |
| Load Factor                        | 0.75  |
| Threshold                          | 12    |
| Treeify Threshold                  | 8     |
| Untreeify Threshold                | 6     |
| Minimum Capacity for Treeification | 64    |

---

# 30. One-Page Revision Flow

```text
                put(key, value)

                     │
                     ▼
              key.hashCode()
                     │
                     ▼
             hash = h ^ (h >>> 16)
                     │
                     ▼
      bucketIndex = (capacity - 1) & hash
                     │
                     ▼
               Go to Bucket
                     │
          ┌──────────┴──────────┐
          │                     │
      Bucket Empty?          Bucket Occupied
          │                     │
      Insert Node          Compare hash
                                  │
                             Compare equals()
                                  │
                      ┌───────────┴───────────┐
                      │                       │
                 Same Key?               Different Key
                      │                       │
               Replace Value          Append to chain
                                              │
                                   Bucket size ≥ 8?
                                              │
                                     Capacity ≥ 64?
                                              │
                                  Yes → Treeify
                                  No  → Resize

------------------------------------------------------------

Default Capacity      : 16
Load Factor           : 0.75
Threshold             : Capacity × Load Factor
Treeify Threshold     : 8
Untreeify Threshold   : 6
Treeify Min Capacity  : 64

hashCode()
      ↓
hash()
      ↓
bucket index
      ↓
bucket
      ↓
linked list / red-black tree
      ↓
equals()
      ↓
return value
```

# 31. Common Interview Questions

1. How does `HashMap` compute the bucket index?
2. Why is the capacity always a power of 2?
3. Why doesn't `HashMap` use `hashCode()` directly?
4. Explain the role of `equals()` in `HashMap`.
5. What is a collision, and how does `HashMap` resolve it?
6. What is separate chaining?
7. When does a bucket convert into a Red-Black Tree?
8. Why is the minimum capacity for treeification 64?
9. What is the difference between capacity, load factor, and threshold?
10. What happens internally during a resize (rehashing)?
11. What are the average and worst-case time complexities of `put()` and `get()`?
12. What contract must `equals()` and `hashCode()` follow?
13. Why can two different objects have the same `hashCode()`?
14. How does `HashMap` replace the value for an existing key?
15. What happens if you mutate a key after inserting it into a `HashMap`?



# hashCode() and Hashing in Java — Complete Notes

---

# 1. What is `hashCode()`?

`hashCode()` is a method defined in Java's `Object` class.

```java
public int hashCode()
```

It returns a **32-bit integer** that represents an object.

Example

```java
String s = "Apple";

System.out.println(s.hashCode());
```

Output

```
63476538
```

> **Important:** The hash code is **not** the memory address of the object.

---

# 2. Why do we need `hashCode()`?

Suppose we have millions of objects stored.

Without hashing, searching would require comparing every object.

```
John

↓

Alice

↓

Bob

↓

David ✔
```

Time Complexity

```
O(n)
```

Very slow.

Instead, Java converts the object into an integer.

```
Object

↓

hashCode()

↓

63476538
```

Now Java can quickly determine where the object belongs.

---

# 3. Why convert an object into a number?

Suppose we have

```java
Student s = new Student(101, "John");
```

The JVM cannot directly calculate where to store this object.

It first converts the object into an integer.

```
Student

↓

hashCode()

↓

987654321
```

Numbers are much easier and faster for computers to process.

---

# 4. Real-Life Analogy

Imagine a library with one million books.

Without a catalog

```
Shelf 1

↓

Shelf 2

↓

Shelf 3

↓

...

↓

Shelf 850
```

With a catalog

```
Harry Potter

↓

Shelf 27
```

`hashCode()` acts like the catalog number.

---

# 5. Is `hashCode()` unique?

No.

Different objects can produce the same hash code.

Example

```
"Aa"

↓

2112

"BB"

↓

2112
```

This is called a **collision**.

---

# 6. Java Contract

If

```java
a.equals(b)
```

is true,

then

```java
a.hashCode() == b.hashCode()
```

must also be true.

However,

Two different objects **may** have the same hash code.

---

# 7. Does `hashCode()` identify the object?

No.

It only narrows the search.

Actual equality is checked using

```java
equals()
```

Flow

```
hashCode()

↓

Find bucket

↓

equals()

↓

Exact object
```

---

# 8. What is Hashing?

Many developers confuse these.

They are different.

| hashCode() | Hashing |
|------------|----------|
| Method | Technique |
| Produces integer | Uses integer |
| Object → int | int → Bucket |
| Implemented by class | Performed by HashMap |

---

# 9. Complete Hashing Flow

```
Object

↓

hashCode()

↓

hash()

↓

Bucket Index

↓

Bucket

↓

equals()

↓

Exact Object
```

---

# 10. Why Hash?

Hashing is done because

Searching

```
O(n)
```

is slow.

Hashing converts searching into

```
O(1)
```

average time.

---

# 11. Why doesn't HashMap use `hashCode()` directly?

This is one of the most asked interview questions.

Suppose

```
hashCode

=

11111111111111110000000000000000
```

Capacity

```
16
```

Bucket calculation

```java
index = (capacity - 1) & hash;
```

Capacity-1

```
15

=

0000....1111
```

Only the **last four bits** are used.

The last four bits here are

```
0000
```

So

```
Bucket 0
```

Another hash code

```
10000000000000000000000000000000
```

also has

```
0000
```

at the end.

Again

```
Bucket 0
```

Many different hash codes end up in the same bucket.

This creates collisions.

---

# 12. The Problem

Many classes generate hash codes where

```
High bits

contain useful information

↓

Low bits

contain zeros
```

But HashMap uses

```
Low bits
```

for bucket calculation.

So useful information gets ignored.

---

# 13. The Solution — `hash()`

HashMap improves the hash using

```java
hash = h ^ (h >>> 16);
```

Flow

```
Original hashCode

↓

Shift higher bits

↓

Mix with lower bits

↓

Better hash

↓

Bucket
```

---

# 14. Why shift by 16?

A Java int has

```
32 bits
```

```
31................16 15.............0

+------------------+-----------------+

| High 16 bits     | Low 16 bits     |

+------------------+-----------------+
```

HashMap mainly uses the lower bits.

So Java copies information from the upper half into the lower half.

---

# 15. Understanding `>>>`

Suppose

```
h

11110000111100001010101010101010
```

After

```java
h >>> 16
```

```
00000000000000001111000011110000
```

The higher bits are now present in the lower half.

---

# 16. Why XOR (`^`)?

Java could have used

- AND
- OR
- Addition
- Multiplication

Instead it chose XOR.

---

## Why not AND?

```
1 & 0 = 0

1 & 1 = 1

0 & 0 = 0
```

Most bits become zero.

Poor mixing.

---

## Why not OR?

```
1 | 0 = 1

1 | 1 = 1
```

Once a bit becomes 1,

information is lost.

---

## Why not Addition?

Addition causes

- Carries
- Overflow
- More CPU work

Not suitable for lightweight mixing.

---

## Why XOR?

Rules

```
0 ^ 0 = 0

0 ^ 1 = 1

1 ^ 0 = 1

1 ^ 1 = 0
```

Example

```
High

1010

Low

0101

↓

1010

^

0101

↓

1111
```

Now the lower bits depend on

- original lower bits
- higher bits

Exactly what we want.

---

# 17. Doesn't XOR lose information?

Excellent observation.

Example

```
1 ^ 1 = 0
```

From the result

```
0
```

we cannot know whether it came from

```
0 ^ 0
```

or

```
1 ^ 1
```

So yes,

**XOR loses information.**

But HashMap does **not** need reversible hashing.

Its goal is

```
Good distribution

NOT

Recover original hash.
```

---

# 18. What if hash value is small?

Suppose

```
hashCode

=

5
```

Binary

```
00000000000000000000000000000101
```

Shift

```
00000000000000000000000000000000
```

XOR

```
5 ^ 0

=

5
```

Nothing changes.

Is that bad?

No.

Because the lower bits already contain useful information.

---

# 19. When does hash() help?

Suppose

```
hashCode

=

65536
```

Binary

```
00000000000000010000000000000000
```

Without mixing

```
Bucket

=

0
```

After

```java
h ^ (h >>> 16)
```

The lower bits become

```
...0001
```

Now

```
Bucket

=

1
```

Better distribution.

---

# 20. Is hash() applied to lower bits too?

Yes.

Suppose

```
High

AAAAAAAAAAAAAAAA

Low

BBBBBBBBBBBBBBBB
```

Shift

```
0000000000000000

AAAAAAAAAAAAAAAA
```

XOR

```
AAAAAAAAAAAAAAAA

BBBBBBBBBBBBBBBB

^

AAAAAAAAAAAAAAAA

↓

AAAAAAAAAAAAAAAA

(BBBBBBBBBBBBBBBB XOR AAAAAAAAAAAAAAAA)
```

Notice

- Upper 16 bits remain unchanged.
- Lower 16 bits are mixed.

---

# 21. Why only lower bits?

Bucket calculation is

```java
(capacity - 1) & hash
```

Suppose capacity is

```
16
```

Then

```
15

=

1111
```

Only

```
Last 4 bits
```

are used.

So Java wants the lower bits to contain information from the entire hash.

---

# 22. Why power of 2 capacity?

HashMap capacities are

```
16

32

64

128
```

Never

```
17

25

30
```

Reason

Bucket calculation

```java
(capacity - 1) & hash
```

works efficiently only for powers of two.

---

# 23. Why not use `%`?

Instead of

```java
hash % capacity
```

Java uses

```java
(capacity - 1) & hash
```

because

- Faster
- Single CPU instruction
- Requires power-of-two capacity

---

# 24. Complete Internal Flow

```
Object

↓

hashCode()

↓

32-bit integer

↓

hash()

h ^ (h >>> 16)

↓

Improved hash

↓

(capacity-1) & hash

↓

Bucket Index

↓

Bucket

↓

equals()

↓

Exact Object

↓

Return Value
```

---

# 25. Time Complexity

| Operation | Average |
|-----------|----------|
| hashCode() | O(1) |
| hash() | O(1) |
| Bucket Calculation | O(1) |
| get() | O(1) |
| put() | O(1) |

Worst Case

Linked List

```
O(n)
```

Tree

```
O(log n)
```

---

# 26. HashMap vs JWT Hashing

People often confuse these.

## HashMap

Purpose

```
Performance
```

Flow

```
Object

↓

hashCode()

↓

Bucket

↓

Fast Lookup
```

---

## JWT

Purpose

```
Security
```

Flow

```
Header + Payload

↓

HMAC / SHA-256

↓

Signature

↓

Detect Tampering
```

HashMap hashing is for **finding data quickly**.

JWT hashing is for **verifying integrity**.

---

# 27. Interview Questions

### What is `hashCode()`?

A method that returns a 32-bit integer representing an object.

---

### Why do we need `hashCode()`?

To convert an object into an integer so hash-based data structures can efficiently locate the bucket where it should be stored or searched.

---

### Is `hashCode()` unique?

No.

Different objects can have the same hash code.

---

### Why doesn't HashMap use `hashCode()` directly?

Because the lower bits of many hash codes may be poorly distributed.

HashMap improves the hash using

```java
h ^ (h >>> 16)
```

to reduce collisions.

---

### Why use XOR?

Because it efficiently mixes high-order bits into low-order bits with minimal CPU overhead, improving the distribution of keys across buckets.

---

### Does XOR lose information?

Yes.

But HashMap doesn't need a reversible operation.

It only needs a better statistical distribution.

---

### Why shift by 16?

To move the higher 16 bits into the lower 16 bits, because the bucket index calculation primarily depends on the lower bits.

---

### Why are capacities powers of two?

So the bucket index can be computed efficiently using:

```java
(capacity - 1) & hash
```

instead of the slower modulo operation.

---

# 28. One-Page Revision

```
Object
   │
   ▼
hashCode()
   │
   ▼
32-bit Hash
   │
   ▼
hash = h ^ (h >>> 16)
   │
   ▼
Mix High Bits into Low Bits
   │
   ▼
(capacity - 1) & hash
   │
   ▼
Bucket Index
   │
   ▼
Bucket
   │
   ▼
equals()
   │
   ▼
Exact Object
   │
   ▼
Return Value

-----------------------------------

hashCode() → Generates hash value

hash() → Improves distribution

Bucket Index → Chooses bucket

equals() → Finds exact key

-----------------------------------

HashMap Hashing → Performance

JWT Hashing → Security
```






Who computes the hashCode()?

Every Java class inherits:

    public int hashCode()

from Object.

There are two cases.

Case 1: Class does NOT override hashCode()

Example

    class Student {
    int id;
    String name;
    }
    Student s1 = new Student();
    Student s2 = new Student();
    
    System.out.println(s1.hashCode());
    System.out.println(s2.hashCode());

Here, Object.hashCode() is used.

The JVM generates a hash value for each object. The exact algorithm is not specified by the Java specification and can vary between JVM implementations. You should think of it as an identity-based hash, not as the memory address.

Case 2: Class overrides hashCode()

Example

    class Student {
    int id;
    String name;
    
        @Override
        public int hashCode() {
            return Objects.hash(id, name);
        }
    }

Now the hash code depends on

    id
    name
How does String.hashCode() work?

This is important for interviews.

For a string

"ABC"

Java computes

    'A' * 31² +
    'B' * 31¹ +
    'C' * 31⁰

The actual implementation is:

    public int hashCode() {
    int h = 0;
    
        for (char c : value) {
            h = 31 * h + c;
        }
    
        return h;
    }
Example

String

ABC

ASCII values

A = 65

B = 66

C = 67

Calculation

h = 0

h = 31*0 + 65

= 65

h = 31*65 + 66

= 2081

h = 31*2081 + 67

= 64578

That becomes the hash code.

Why multiply by 31?

This is another favorite interview question.

Java designers chose 31 because:

    It is a prime number.
    It provides good hash distribution.
    Multiplication by 31 can be optimized by the JVM:
    31 * x
    
    =
    
    (x << 5) - x
    
    since
    
    32x - x = 31x

This was historically a useful optimization.

How does Objects.hash() work?

Example

Objects.hash(id, name)

Internally (simplified), it does something like:

Arrays.hashCode(new Object[]{id, name});

And combines the field hashes.

Conceptually:

result = 1

result = 31 * result + id.hashCode()

result = 31 * result + name.hashCode()


------------------------------------------------------------------------




You override hashCode() because the default implementation from Object is identity-based, while you often want objects to be considered equal based on their data (state).

    If you override equals() but don't override hashCode(), collections like HashMap and HashSet will not work correctly.

Example

Suppose you have:

    class Student {
    int id;
    String name;
    }

Create two objects:

    Student s1 = new Student(1, "John");
    Student s2 = new Student(1, "John");

Logically:

    s1 and s2 represent the same student.
Without overriding anything

equals() comes from Object:

System.out.println(s1.equals(s2));

Output:

    false

Why?

Because Object.equals() is effectively:

    return this == obj;

It checks reference equality, not data.

Similarly,

    System.out.println(s1.hashCode());
    System.out.println(s2.hashCode());

might produce:

    1836019240
    325040804

Different hash codes because they are different objects.

Now override equals()
    
    @Override
    public boolean equals(Object obj) {
    Student s = (Student) obj;
    return id == s.id &&
    Objects.equals(name, s.name);
    }

Now:

    s1.equals(s2)

returns

    true

because their data is the same.

But don't override hashCode()

Now imagine:

    HashSet<Student> set = new HashSet<>();

    set.add(s1);
    
    System.out.println(set.contains(s2));

What do you expect?

Most people say:

    true

because equals() says they're equal.

Actual result:

    false
Why?

When you call:

    set.contains(s2);

HashSet first computes:

    s2.hashCode()

Since hashCode() is still inherited from Object, s1 and s2 have different hash codes.

Example:

    s1.hashCode() = 1200
    
    s2.hashCode() = 4500
    
    So HashSet looks in Bucket 4500, but s1 was stored in Bucket 1200.
    
    It never even reaches the equals() comparison.

contains(s2)

↓

hashCode()

↓

Bucket 4500

↓

Empty

↓

false
Correct implementation
    
    @Override
    public int hashCode() {
    return Objects.hash(id, name);
    }

Now:

    s1.hashCode() == s2.hashCode()

Both objects go to the same bucket.

Then HashSet calls:

    equals()

which returns true.

Everything works correctly.


    @Override
    public boolean equals(Object obj) {
    
        if (this == obj)
            return true;
    
        if (obj == null)
            return false;
    
        if (getClass() != obj.getClass())
            return false;
    
        Student other = (Student) obj;
    
        return id == other.id &&
               Objects.equals(name, other.name);
    }



Hahscode Object


    public class Object {
    
        public int hashCode() {
            // Native JVM implementation
    
            // Conceptually:
            return identityHashCode(this);
        }
    }

Overriden Hashcode


    @Override
    public int hashCode() {
    return Objects.hash(id, name);
    }


What does Objects.hash() do?

Its implementation is approximately:

        public static int hash(Object... values) {
        return Arrays.hashCode(values);
        }

And Arrays.hashCode() (simplified) is:

    public static int hashCode(Object[] values) {
    
        int result = 1;
    
        for (Object value : values) {
            result = 31 * result +
                     (value == null ? 0 : value.hashCode());
        }
    
        return result;
    }