
## Wrapper Class

        A **Wrapper class** is an object-version of a primitive data type.

Java has 8 primitive types, and each has a corresponding wrapper class:

| Primitive | Wrapper Class |
|-----------|---------------|
| `int`     | `Integer`     |
| `long`    | `Long`        |
| `short`   | `Short`       |
| `byte`    | `Byte`        |
| `float`   | `Float`       |
| `double`  | `Double`      |
| `char`    | `Character`   |
| `boolean` | `Boolean`     |

    A wrapper **wraps** the primitive value inside an **object**.


# ✅ 1. **Why Wrapper Classes Exist**

### **Reason 1 — Objects are needed in many Java APIs**

    Java Collections (ArrayList, HashMap, etc.) **cannot store primitives**.
    `List<int> list = new ArrayList<>();  // ❌ Not allowed List<Integer> list = new ArrayList<>(); // ✔ Works`
    Because collections only store **Objects**, not primitive values.

---

### **Reason 2 — Wrappers provide utility methods**

Example:

    `int x = Integer.parseInt("10"); boolean b = Boolean.parseBoolean("true");`

Primitives cannot do this; wrappers add **extra functionality**.

---


### **Reason 4 — Null support**

Primitives cannot be `null`.

`int a = null;  // ❌ ERROR Integer a = null; // ✔ Valid`

In databases, JSON, REST APIs → fields may be null → wrapper required.

---


Wrapper classes make primitives “object-friendly.”



# ✅ **Why Collections Do NOT Support Primitives**

The **root cause**:  
👉 **Java Generics work only with Objects** — not primitives.

Collections like:
    
    - `List<T>`
      - `Set<T>`
      - `Map<K, V>`


are all implemented using **Generics**.




# ✅ **Why must Generic type parameter `T` be a reference type?**

(i.e., why `T` cannot be `int`, `double`, etc.)

---

# #1️⃣ **Because of How Generics Are Implemented — TYPE ERASURE**

Java generics exist only at **compile time**, not at runtime.

Example:

        `List<Integer> list = new ArrayList<>();`
        After compilation (bytecode), this becomes:
        
        `List list = new ArrayList();  // T → Object`
        ➡ **All generics collapse to `Object` at runtime.**

---

### ❗ Now imagine:

If Java allowed:

`List<int> list;`

After type erasure this becomes:

`List list; // internally stores Object[]`

But `int` **cannot be stored in Object[]** because:

- `int` is not an `Object`

- primitives cannot be cast to Object

- primitives have no reference identity


So **type erasure makes it impossible for generics to work with primitives.**

👉 **This is the REAL root cause.**





# 🧠 **Autoboxing & Unboxing (Very Important)**

Java automatically converts:
### Primitive → Wrapper (Autoboxing)

    `int x = 5; Integer y = x; // autoboxing`

### Wrapper → Primitive (Unboxing)

`Integer n = 10; int m = n; // unboxing`



---

# ✅ **📌 Full Important Implementation of `Integer` (Explained Line by Line)**

---

# 🧩 **Class Declaration**

`public final class Integer extends Number implements Comparable<Integer> {`

### ✔ `final`

Cannot be subclassed → ensures immutability.

### ✔ `extends Number`

Allows converting to:

- `int`
- `double`
- `float`
- `long`


### ✔ `implements Comparable<Integer>`

Allows sorting, comparing integers.

---

# 🧩 **Internal Field**

`private final int value;`

### ✔ `final`

Value cannot be changed → Immutable.

---

# 🧩 **Constructor** (Not recommended)

`public Integer(int value) {     this.value = value; }`

This **always creates a new object** → does NOT use cache.

Better: use `Integer.valueOf()`.

---

# 🧩 **valueOf() — MOST IMPORTANT (Caching)**

```
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high) {
        return IntegerCache.cache[i + (-IntegerCache.low)];
    }
    return new Integer(i);
}

```

### ⭐ What it does:

1. If integer is inside cache range (-128 to +127):

    - returns _pre-created_ object → fast!

2. Else:

    - creates a new Integer object.


### Why?

Performance + memory optimization.

---

# 🧩 **Integer Cache (Static Inner Class)**

```
private static class IntegerCache {
    static final int low = -128;
    static final int high = 127;
    static final Integer[] cache = new Integer[(high - low) + 1];

    static {
        for (int i = low; i <= high; i++) {
            cache[i - low] = new Integer(i);
        }
    }
}

```

### 🔥 What happens here:

- JVM pre-creates 256 Integer objects (`-128` to `+127`)

- Stored inside `cache[]`

- Used when you call `Integer.valueOf()`


---

# 🧩 **equals() Implementation**

```
public boolean equals(Object obj) {
    if (obj instanceof Integer) {
        return value == ((Integer) obj).intValue();
    }
    return false;
}

```

### ✔ Logic:

- Checks if the other object is Integer

- Compares primitive values

- Avoids reference comparison


---

# 🧩 **hashCode()**

`public int hashCode() {     return value; }`

### ✔ Why?

Hash of an Integer should be the integer itself.

---

# 🧩 **compareTo()**

```
public int compareTo(Integer anotherInteger) {
    return compare(this.value, anotherInteger.value);
}

public static int compare(int x, int y) {
    return (x < y) ? -1 : (x == y ? 0 : 1);
}

```
### ✔ Used for sorting, TreeSet, TreeMap

---

# 🧩 **toString()**

`public String toString() {     return Integer.toString(value); }`

---

# 🧩 **Static parseInt()**

```
public static int parseInt(String s) throws NumberFormatException {
    return parseInt(s, 10);
}

```
---

# 🧩 **Static parseInt(String, int radix)**


```
public static int parseInt(String s, int radix) throws NumberFormatException {
    if (s == null) throw new NumberFormatException("null");

    int result = 0;
    boolean negative = false;
    int i = 0, len = s.length();
    int limit = -Integer.MAX_VALUE;

    char firstChar = s.charAt(0);
    if (firstChar == '-') {
        negative = true;
        limit = Integer.MIN_VALUE;
        i++;
    }

    while (i < len) {
        int digit = Character.digit(s.charAt(i++), radix);
        if (digit < 0) throw new NumberFormatException();
        result = result * radix - digit;
    }

    return negative ? result : -result;
}

```
### ✔ How it works:

- Manually converts each char to a digit

- Builds integer through multiplication

- Uses a negative accumulator to avoid overflow

- Throws `NumberFormatException` for invalid characters


This is **faster** than regex-based parsing.

---

# 🧩 **intValue(), longValue(), doubleValue(), floatValue()**

(from Number class)

```
public int intValue() {
    return value;
}

public long longValue() {
    return (long) value;
}

public float floatValue() {
    return (float) value;
}

public double doubleValue() {
    return (double) value;
}

```

### ✔ Purpose:

Cast integer wrapper to other primitive types.

---

---

# 🧩 **decode() — converts string to Integer**

```
public static Integer decode(String nm) {
    int radix = 10;
    int index = 0;
    boolean negative = false;

    if (nm.startsWith("-")) {
        negative = true;
        index++;
    }

    if (nm.startsWith("0x", index) || nm.startsWith("0X", index)) {
        radix = 16;
        index += 2;
    } else if (nm.startsWith("#", index)) {
        radix = 16;
        index++;
    } else if (nm.startsWith("0", index) && nm.length() > 1) {
        radix = 8;
    }

    return Integer.valueOf(parseInt(nm.substring(index), radix) * (negative ? -1 : 1));
}

```

### ✔ Supports:

- Decimal

- Hex (`0x10`, `#10`)

- Octal (`010`)




--------------------------------------------------------------------------------------



# ✅ **1. Using `Integer.valueOf(int)` → BEST (recommended)**

`Integer a = Integer.valueOf(10);`

### ✔ Uses **Integer Cache**

### ✔ Does NOT create new object for values -128 to +127

### ✔ Fastest and most memory-efficient

### ✔ Used by autoboxing internally

---

# ✅ **2. Using Autoboxing → (actually becomes valueOf internally)**

`Integer b = 10;  // Autoboxing`

Compiler converts this to:

`Integer b = Integer.valueOf(10);`

### ✔ Same as valueOf

### ✔ Uses caching

### ✔ Most used in real-world code

---

# ❗ **3. Using `new Integer(int)` → Worst (creates new object every time)**

`Integer c = new Integer(10);`

### ❌ Does NOT use caching

### ❌ Always allocates new object

### ❌ Deprecated after Java 9

---

# ❗ **4. Using `new Integer(String)` → Also always creates new object**

`Integer d = new Integer("10");`

### ❌ Slow

### ❌ Parses string

### ❌ Does NOT use cache

---

# ⚡ **5. Using `Integer.parseInt()` → returns primitive int, NOT Integer**

`int e = Integer.parseInt("10");`

If you want Integer:

`Integer e2 = Integer.valueOf(Integer.parseInt("10"));`

---

# ⚡ **6. Using `Integer.decode(String)`**

`Integer f = Integer.decode("10");     // decimal Integer g = Integer.decode("0x10");   // hex Integer h = Integer.decode("010");    // octal`

### ✔ Supports hex, oct, decimal

### ✔ Uses valueOf internally → uses cache

---

# ⚡ **7. Using `Integer.valueOf(String)`**

`Integer i = Integer.valueOf("10");`

### ✔ Parses then uses cache

Equivalent to:

`Integer i = Integer.valueOf(Integer.parseInt("10"));`


# ✅ **3. Wrapper Class Immutability**

All wrapper objects are **immutable** (value cannot change).

Why?

- Thread-safe

- Supports caching

- Behavior similar to String


Learn:

- How immutability affects comparison

- How immutability avoids bugs


---

# ✅ **4. Wrapper Class Caching (Very Important)**

Only for:

- `Integer`

- `Byte`

- `Short`

- `Long`

- `Character` (0–127)


### **Integer Cache Range**

`-128 to 127`


```
Integer a = 100;
Integer b = 100;
a == b → true  

Integer x = 200;
Integer y = 200;
x == y → false

```




----------------------------------------------------------------------------------

# 🔹 1️⃣ Parsing & Conversion Methods (MOST IMPORTANT)

### `parseInt()`

`int x = Integer.parseInt("123");`

- Converts `String` → primitive `int`

- Throws `NumberFormatException` if invalid


`Integer.parseInt("101", 2); // binary → decimal (5)`

---

### `valueOf()`

`Integer x = Integer.valueOf("123");`

- Returns an `Integer` object

- Uses **Integer cache (-128 to 127)**


📌 Preferred over `new Integer()`

---

### `decode()`

`Integer x = Integer.decode("0x10"); // 16`

Supports:

- Decimal

- Hex (`0x`)

- Octal (`0`)


---

# 🔹 2️⃣ Comparison Methods

### `compare(int x, int y)`

`Integer.compare(10, 20); // -1`

Used in:

`stream.max(Integer::compare)`

---

### `compareUnsigned(int x, int y)`

`Integer.compareUnsigned(-1, 1); // > 0`

Treats ints as **unsigned**

---

# 🔹 3️⃣ Math / Utility Methods

### `sum(int a, int b)`

`Integer.sum(10, 20); // 30`

---

### `max(int a, int b)`

`Integer.max(10, 20); // 20`

---

### `min(int a, int b)`

`Integer.min(10, 20); // 10`

📌 Used in `reduce()` operations

---

# 🔹 4️⃣ Bit Manipulation Methods (INTERVIEW FAVORITE 🔥)

### `bitCount(int i)`

`Integer.bitCount(7); // 3 (111)`

---

### `highestOneBit(int i)`

`Integer.highestOneBit(10); // 8`

---

### `lowestOneBit(int i)`

`Integer.lowestOneBit(10); // 2`

---

### `numberOfLeadingZeros(int i)`

`Integer.numberOfLeadingZeros(1); // 31`

---

### `numberOfTrailingZeros(int i)`

`Integer.numberOfTrailingZeros(8); // 3`

---

### `rotateLeft(int i, int distance)`

`Integer.rotateLeft(1, 1); // 2`

---

### `rotateRight(int i, int distance)`

`Integer.rotateRight(2, 1); // 1`

---

### `reverse(int i)`

`Integer.reverse(2);`

---

### `reverseBytes(int i)`

`Integer.reverseBytes(0x12345678);`

---

### `signum(int i)`

`Integer.signum(-5); // -1`

---

# 🔹 5️⃣ String Representation Methods

### `toString(int i)`

`Integer.toString(10); // "10"`

---

### `toBinaryString(int i)`

`Integer.toBinaryString(10); // "1010"`

---

### `toHexString(int i)`

`Integer.toHexString(255); // "ff"`

---

### `toOctalString(int i)`

`Integer.toOctalString(8); // "10"`

---

# 🔹 6️⃣ Unsigned Operations (Advanced)

### `toUnsignedLong(int i)`

`long x = Integer.toUnsignedLong(-1);`

---

### `divideUnsigned(int dividend, int divisor)`

`Integer.divideUnsigned(-1, 2);`

---

### `remainderUnsigned(int dividend, int divisor)`

`Integer.remainderUnsigned(-1, 2);`

---

# 🔹 7️⃣ Constants

`Integer.MAX_VALUE Integer.MIN_VALUE Integer.SIZE        // 32 bits Integer.BYTES       // 4 bytes`

---

# 🧠 Interview Cheat Sheet (IMPORTANT)

### MOST USED

- `parseInt()`

- `valueOf()`

- `compare()`

- `sum()`

- `max()`

- `min()`


### STREAM / FUNCTIONAL

- `sum`

- `max`

- `min`

- `compare`


### BIT LEVEL

- `bitCount`

- `highestOneBit`

- `numberOfLeadingZeros`



🔴 1. == vs .equals() (MUST be crystal clear)

You mentioned it, but interviewer goes deeper:

Integer a = 100;
Integer b = 100;

System.out.println(a == b); // true
Integer x = 200;
Integer y = 200;

System.out.println(x == y); // false

👉 Question:
Why behavior changes?

✔ You must instantly say:

Cache (-128 to 127)
Outside → new object
🔴 2. Null + Unboxing (VERY COMMON QUESTION)
Integer x = null;
int y = x;   // 💥 NullPointerException

👉 WHY?

✔ Because:

JVM calls x.intValue()
null → method call → crash

👉 This is frequently asked

🔴 3. Autoboxing Hidden Cost (Advanced but important)
Integer sum = 0;

for (int i = 0; i < 1000; i++) {
sum += i;
}

👉 Problem:

Repeated boxing/unboxing
Creates many objects

✔ Better:

int sum = 0;
🔴 4. Wrapper in Collections vs Primitive Performance

👉 Interview question:

Why not always use Integer instead of int?

✔ Answer:

Wrapper → object → heap allocation
Primitive → stack → faster
More memory + GC overhead
🔴 5. Integer.valueOf() vs new Integer()

You wrote it well, but interviewer may ask:

👉 Why new Integer() is bad?

✔ Because:

Always creates new object
Breaks caching
Deprecated
🔴 6. Edge Case Question (VERY IMPORTANT)
Integer a = 128;
Integer b = 128;

System.out.println(a == b); // ?

👉 You must instantly answer:
❌ false

🔴 7. Streams + Wrapper (MODERN INTERVIEW)
List<Integer> list = List.of(1, 2, 3);

int sum = list.stream()
.mapToInt(Integer::intValue)
.sum();

👉 WHY mapToInt?

✔ Avoids boxing overhead



| Method                          | Description                | Return Type |
| ------------------------------- | -------------------------- | ----------- |
| `parseInt(String s)`            | String → primitive int     | `int`       |
| `parseInt(String s, int radix)` | String (base) → int        | `int`       |
| `valueOf(int i)`                | int → Integer (uses cache) | `Integer`   |
| `valueOf(String s)`             | String → Integer           | `Integer`   |
| `valueOf(String s, int radix)`  | String (base) → Integer    | `Integer`   |
| `decode(String nm)`             | Hex/Oct/Dec → Integer      | `Integer`   |

| Method          | Description      | Return Type |
| --------------- | ---------------- | ----------- |
| `intValue()`    | Integer → int    | `int`       |
| `longValue()`   | Integer → long   | `long`      |
| `floatValue()`  | Integer → float  | `float`     |
| `doubleValue()` | Integer → double | `double`    |
| `byteValue()`   | Integer → byte   | `byte`      |
| `shortValue()`  | Integer → short  | `short`     |



| Method                  | Description      | Return Type |
| ----------------------- | ---------------- | ----------- |
| `compare(int x, int y)` | Compare two ints | `int`       |
| `compareTo(Integer o)`  | Compare objects  | `int`       |
| `equals(Object o)`      | Value comparison | `boolean`   |



| Method              | Description  | Return Type |
| ------------------- | ------------ | ----------- |
| `sum(int a, int b)` | Add two ints | `int`       |
| `max(int a, int b)` | Maximum      | `int`       |
| `min(int a, int b)` | Minimum      | `int`       |



| Field               | Description |
| ------------------- | ----------- |
| `Integer.MAX_VALUE` | 2³¹ - 1     |
| `Integer.MIN_VALUE` | -2³¹        |
| `Integer.SIZE`      | 32 bits     |
| `Integer.BYTES`     | 4 bytes     |



```java

public class IntegerWrapperDemo {

    public static void main(String[] args) {

        // =========================
        // 1. Parsing & Conversion
        // =========================
        int p1 = Integer.parseInt("123");
        int p2 = Integer.parseInt("101", 2); // binary → decimal
        Integer v1 = Integer.valueOf(10);
        Integer v2 = Integer.valueOf("20");
        Integer d1 = Integer.decode("0x10"); // hex

        System.out.println("Parsing:");
        System.out.println(p1 + " " + p2 + " " + v1 + " " + v2 + " " + d1);

        // =========================
        // 2. Primitive Conversion
        // =========================
        Integer num = 10;
        int i = num.intValue();
        long l = num.longValue();
        double d = num.doubleValue();

        System.out.println("\nPrimitive Conversion:");
        System.out.println(i + " " + l + " " + d);

        // =========================
        // 3. Comparison
        // =========================
        Integer a = 10, b = 20;

        System.out.println("\nComparison:");
        System.out.println(Integer.compare(a, b)); // -1
        System.out.println(a.compareTo(b));        // -1
        System.out.println(a.equals(b));           // false

        // =========================
        // 4. Math / Utility
        // =========================
        System.out.println("\nMath:");
        System.out.println(Integer.sum(10, 20));
        System.out.println(Integer.max(10, 20));
        System.out.println(Integer.min(10, 20));

        // =========================
        // 5. String Conversion
        // =========================
        System.out.println("\nString Conversion:");
        System.out.println(Integer.toString(10));
        System.out.println(Integer.toBinaryString(10));
        System.out.println(Integer.toHexString(255));

        // =========================
        // 6. Bit Manipulation
        // =========================
        int x = 10;

        System.out.println("\nBit Operations:");
        System.out.println(Integer.bitCount(x));
        System.out.println(Integer.highestOneBit(x));
        System.out.println(Integer.lowestOneBit(x));
        System.out.println(Integer.numberOfLeadingZeros(x));

        // =========================
        // 7. Unsigned Operations
        // =========================
        int neg = -1;

        System.out.println("\nUnsigned:");
        System.out.println(Integer.toUnsignedLong(neg));
        System.out.println(Integer.compareUnsigned(-1, 1));

        // =========================
        // 8. Object Methods
        // =========================
        Integer obj1 = 10;
        Integer obj2 = 10;

        System.out.println("\nObject Methods:");
        System.out.println(obj1.hashCode());
        System.out.println(obj1.equals(obj2));

        // =========================
        // 9. Cache Behavior
        // =========================
        Integer c1 = 100;
        Integer c2 = 100;

        Integer c3 = 200;
        Integer c4 = 200;

        System.out.println("\nCache:");
        System.out.println(c1 == c2); // true
        System.out.println(c3 == c4); // false

        // =========================
        // 10. Autoboxing / Unboxing
        // =========================
        Integer auto = 50; // autoboxing
        int unbox = auto;  // unboxing

        System.out.println("\nAutoboxing:");
        System.out.println(auto + " " + unbox);

        // =========================
        // 11. Null Unboxing (Exception)
        // =========================
        try {
            Integer nullVal = null;
            int crash = nullVal; // 💥 NPE
        } catch (Exception e) {
            System.out.println("\nNull Unboxing Error: " + e);
        }

        // =========================
        // 12. Arithmetic (Unboxing)
        // =========================
        Integer n1 = 10;
        Integer n2 = 20;

        int sum = n1 + n2; // unboxing happens

        System.out.println("\nArithmetic:");
        System.out.println(sum);
    }
}

```

OUTPUT :
        
        Parsing:
        123 5 10 20 16
        
        Primitive Conversion:
        10 10 10.0
        
        Comparison:
        -1
        -1
        false
        
        Math:
        30
        20
        10
        
        String Conversion:
        10
        1010
        ff
        
        Bit Operations:
        2
        8
        2
        28
        
        Unsigned:
        4294967295
        1
        
        Object Methods:
        10
        true
        
        Cache:
        true
        false
        
        Autoboxing:
        50 50
        
        Null Unboxing Error: java.lang.NullPointerException
        
        Arithmetic:
        30
        


Why exactly -128 to 127 (256 values)?

    👉 It is NOT random
    👉 It is a design + JVM optimization decision

🧠 1. These values are MOST frequently used

In real programs:

    Loop counters → 0, 1, 2, ...
    Array indexes → small positives
    Flags → -1, 0, 1
    ASCII values → 0–127

👉 So most integers used are in:

-128 to 127
⚖️ 2. Memory vs Performance Trade-off
        
        Caching means:
        
        ✔ Reuse objects → faster
        ❌ Store objects → memory cost
        
        So JVM chooses a sweet spot:
        
        Range	Benefit
        Small (256 values)	✔ High reuse
        Large (millions)	❌ Wasted memory

👉 256 is cheap + effective

🔢 3. Why exactly 256 numbers?

    Because:
    
    -128 to 127 = 256 values
    
    👉 This aligns with:
    
    1 byte range (8-bit signed)
Very common low-level representation
⚙️ 4. JVM Optimization History

Early Java designers:

        Observed usage patterns
        Chose byte-range caching as optimal default

👉 That’s why:

    Integer
    Byte
    Short
    Long
    Character (0–127)

all have similar small caches



setter

🔥 So why is it thread-safe?

        👉 Because no thread can modify it
        
        No race condition
        No inconsistent state
        No synchronization needed
        because of immutability


| Feature     | `Integer.parseInt()`    | `Integer.valueOf()`                             |
| ----------- | ----------------------- | ----------------------------------------------- |
| Return type | `int` (primitive)       | `Integer` (object)                              |
| Output      | primitive value         | wrapper object                                  |
| Caching     | ❌ No                    | ✔ Yes (-128 to 127)                             |
| Performance | Slightly faster         | Slightly slower (object creation if not cached) |
| Usage       | When you need primitive | When you need object (collections, APIs)        |
