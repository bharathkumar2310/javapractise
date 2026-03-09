**_MAP :**_

![img_1.png](../Images/Map1.png)


        Map is not a Collection — it’s a separate interface in java.util.
        Stores key-value pairs.
        Keys are unique, values can be duplicate.
        Useful when you need to associate data with a unique key, e.g., a dictionary or lookup table.

![img.png](../Images/Map2.png)


| Implementation      | Key points                                               |
| ------------------- | -------------------------------------------------------- |
| `HashMap`           | Hash table-based, allows null key/value, unordered       |
| `LinkedHashMap`     | Maintains insertion order                                |
| `TreeMap`           | Sorted according to keys (natural order or `Comparator`) |
| `Hashtable`         | Legacy, synchronized, no null key/value                  |
| `ConcurrentHashMap` | Thread-safe, high-concurrency alternative to `HashMap`   |


| Method                                         | Return type     | Description                                                |
| ---------------------------------------------- | --------------- | ---------------------------------------------------------- |
| `V put(K key, V value)`                        | V               | Adds key-value pair; returns previous value if key existed |
| `V get(Object key)`                            | V               | Returns value associated with key, or null if none         |
| `V remove(Object key)`                         | V               | Removes key-value pair; returns previous value             |
| `boolean containsKey(Object key)`              | boolean         | Checks if key exists                                       |
| `boolean containsValue(Object value)`          | boolean         | Checks if value exists                                     |
| `Set<K> keySet()`                              | Set<K>          | Returns a set of all keys                                  |
| `Collection<V> values()`                       | Collection<V>   | Returns a collection of all values                         |
| `Set<Map.Entry<K,V>> entrySet()`               | Set<Entry<K,V>> | Returns a set of key-value mappings                        |
| `void putAll(Map<? extends K, ? extends V> m)` | void            | Adds all mappings from another map                         |
| `int size()`                                   | int             | Returns number of key-value mappings                       |
| `boolean isEmpty()`                            | boolean         | Checks if map is empty                                     |
| `void clear()`                                 | void            | Removes all mappings                                       |


![img_2.png](../Images/Map3.png)

![img_3.png](../Images/Map4.png)



HASHMAP :

    HashMap<K,V> is a hash table-based implementation of the Map interface.
    Stores key-value pairs, keys are unique, values can be duplicate.
    Hashing is used to store and retrieve elements efficiently.

| Property           | Description                                                                      |
| ------------------ | -------------------------------------------------------------------------------- |
| Null key/value     | Allows **one null key** and **multiple null values**                             |
| Ordering           | **No guaranteed order** (unordered)                                              |
| Thread-safety      | **Not synchronized** (use `ConcurrentHashMap` for thread-safe version)           |
| Performance        | Average **O(1)** for `get()` and `put()`; worst-case O(n) if many collisions     |
| Internal structure | Uses **array of buckets** + **linked list or tree (from Java 8)** for collisions |
| Load factor        | Default 0.75 → when threshold exceeds, **capacity doubles** (rehash occurs)      |
| Capacity           | Default initial capacity = 16 (number of buckets)                                |



HASHMAP INTERNALS :

    HashMap uses an array of buckets to store key-value pairs.
    Each bucket can contain multiple entries (key-value pairs) that hash to the same index (collision).
    Before Java 8, collisions were handled with a linked list; from Java 8, if a bucket has too many entries, it converts to a balanced tree for better performance.
    Each entries was a node containing key, value, hash, and pointer to next node (for linked list) or left/right child (for tree).

    When adding a key-value pair, HashMap calculates the hash of the key, determines the bucket index, and either adds it to the bucket or updates existing entry if key already exists.
    
What is hash of a key?

hashCode() is a method defined in java.lang.Object:

    public int hashCode()

It returns an integer (32-bit) representing the object’s hash value.
Used in hash-based collections (HashMap, HashSet, Hashtable) to decide bucket placement.

Contract of hashCode():
    
    If two objects are equal (equals() returns true) → they must have same hashCode.
    If two objects are not equal → hashCodes can be same (collision possible).

2️⃣ How HashMap Uses hashCode

    Compute hashCode for the key → key.hashCode()
    Spread bits to reduce collisions internally → h ^ (h >>> 16)
    Modulo with table size (number of buckets) → index = (n-1) & hash

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```


        If 2 or more keys have same hashCode → they go to same bucket → collision occurs.
        Bucket can contain multiple entries → handled with linked list or tree.
        Collision means multiple keys map to same bucket index → performance degrades if many collisions occur (worst-case O(n)).


**_Load Factor and Rehashing :**_

    Load factor (LF) = measure of how full a hash table can get before it is resized.
    threshold = capacity × LF
     capacity = number of buckets (initially 16)
    default load factor (LF) = 0.75
     threshold = 16 × 0.75 = 12 → when size exceeds 12, HashMap resizes.(by default )

    Default load factor = 0.75 → when exceeded, HashMap resizes (doubles capacity) and rehashes all entries.
    Rehashing means recalculating bucket index for each entry based on new capacity → can be expensive operation.

    
    Higher LF → less memory, more collisions.
    Lower LF → more memory, fewer collisions


TREEIFY_THRESHOLD :

    If a bucket has more than 8 entries (TREEIFY_THRESHOLD), it converts from a linked list to a  balanced binary search tree (Red-Black Tree) for better performance.
    This reduces worst-case time complexity from O(n) to O(log n) for that bucket.






HASHTABLE :

Exactly like HashMap but:

    Legacy class (before Java 1.2)
    Synchronized (thread-safe) → all methods are synchronized
    Does not allow null key or null value
    Generally slower than HashMap due to synchronization overhead




