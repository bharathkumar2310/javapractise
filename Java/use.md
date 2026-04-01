## 1️⃣ What is a Singleton?

A **Singleton** is a **design pattern** (**Creational Design Pattern**.)where a class is designed to have:

1. **Only one instance** throughout the JVM.
2. **Global access point** to that instance.

✅ Use cases:

- Logger
- Configuration manager
- Thread pool manager
- Cache manager


---

## 2️⃣ Key Properties

| Property                 | Explanation                                 |
| ------------------------ | ------------------------------------------- |
| Single instance          | Only one object of the class exists         |
| Global access            | Accessible from anywhere in the application |
| Controlled instantiation | Constructor is **private**                  |
## **1️⃣ What is Singleton?**

A **singleton** is a class that **allows only one instance to be created** in the entire application.

- Example: `Runtime`, `Desktop`, `Logger`.

- **Key idea:** Single point of control.


---

## **2️⃣ Why we need Singleton?**

### **A. Single Point of Control**

- Some resources should only have **one instance**, e.g.:

    - Database connection pool manager

    - Configuration manager

    - Logging utility


`Logger logger = Logger.getInstance(); logger.log("Hello");`

- If multiple instances existed, you could get **inconsistent logs or configs**.


---

### **B. Resource Efficiency**

- Avoids **unnecessary object creation**.

- Example:


`ConnectionPool pool = ConnectionPool.getInstance();`

- Only **one pool** manages all DB connections → saves memory and CPU.

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1sd2f258ef18f04c648cc9097d45586384)



# ✅ **1. Why `static`?**

### ✔️ Because the object must belong to the _class_, not to objects

If the instance is NOT static, then you need to create an object of the class to access it — which defeats the purpose of Singleton.

Key rules of Singleton:

- only **one instance** in JVM

- instance should be created **without creating any other object**

- should be accessed globally using `ClassName.getInstance()`


A `static` variable:

- lives for the whole JVM lifetime

- belongs to the class, loaded once

- is shared by all threads


So `static` ensures:

`Only one instance exists`

---

# ✅ **2. Why `private`?**

### ✔️ Because it must NOT be accessible directly from outside

If it was `public`:

`public static Singleton instance = new Singleton();`

Any code could modify it:

`Singleton.instance = null;        // BAD Singleton.instance = new Singleton(); // BAD`

Then you lose Singleton behavior.

Also, if instance is `public`, callers can do:

`Singleton.instance`

Which bypasses any logic you might want in `getInstance()`.

Keeping it **private** ensures:

- no one can overwrite it

- no one can create multiple instances

- full control is with the class

   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1s25753708b7784f37b8065ec3b95fdb0e)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1s8de36cefbf934dceae1d13d9e4a5b917)


- ✅ Simple, thread-safe
- ❌ Instance created even if never used (may waste memory)
 -------------------------------------------------------------------------------
 

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1sf0103575b449497a9dbd553221060169)


- ✅ Only created when first needed

- ❌ **Not thread-safe** — multiple threads can create multiple instances

------------------------------------------------------------------------------------


   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1s5ed791d584014a54a83889fded7ee429)

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1sf209acef571b4be7a3f0ac3168ca1fa9)


- - ✅ Thread-safe

- ❌ Synchronization slows down performance
----------------------------------------------------------------------------------------


   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1se9730476898d4f4a871b1522afbe87bf)


![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1s6127b6f9bcc144638c262f8909fd560e)

- ✅ Thread-safe, performant

- `volatile` ensures **proper visibility** across threads

  ---------------------------------------------------------------------------------


   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1sa26edb64b6e14297ada2c479a8acdd4e)

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1s66ae24a360594d628daa1ac0525006e1)


- ✅ Lazy-loaded

- ✅ Thread-safe (classloader guarantees)

- ✅ Preferred modern approach

-------------------------------------------------------------------------------------




   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1s9eab2fc2ddd04d4694aab8a2f4841ef4)

- ✅ Serialization-safe

- ✅ Thread-safe by default

- ✅ Cannot be broken by reflection

--------------------------------------------------------------------------------

## **1️⃣ The Problem With Reflection in Normal Singletons**

For a normal singleton:
```
public class EagerSingleton {
    private static final EagerSingleton INSTANCE = new EagerSingleton();
    private EagerSingleton() { }
    public static EagerSingleton getInstance() { return INSTANCE; }
}

```

- The constructor is **private**, so normally you cannot create a new instance.

- **Reflection** can bypass it:


```
`Constructor<EagerSingleton> cons = EagerSingleton.class.getDeclaredConstructor();
cons.setAccessible(true);  // bypass private
EagerSingleton obj = cons.newInstance(); // Creates a new instance!

```

✅ This **breaks singleton guarantee**.

---

## **2️⃣ Why Enum is Safe from Reflection**

Consider:

`public enum SingletonEnum {     INSTANCE; }`

- Enum constructors are **implicitly private**.

- More importantly, **the JVM enforces special rules for enum instantiation**.


### **Rules enforced by JVM:**

1. **Cannot call enum constructors reflectively.**

    - If you try:

```
Constructor<SingletonEnum> cons = SingletonEnum.class.getDeclaredConstructor();
cons.setAccessible(true);
SingletonEnum obj = cons.newInstance(); // Throws exception!

```

- JVM throws:


`java.lang.IllegalArgumentException: Cannot reflectively create enum objects`

2. **Why JVM does this:**

    - The **`java.lang.Enum` class constructor** checks internally if the class being instantiated is an enum.

    - If it is, **it prevents reflective instantiation**.

    - This is hardcoded in **JVM and `Enum` class**.

-------------------------------------------------------------------------------------------

## What is an Immutable Class?

An **immutable class** is a class whose **state cannot be changed once it’s created**.

- Once an object of an immutable class is created, its **fields cannot be modified**.

- Any modification-like operation creates a **new object**.
   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1s84157ef2ed954169b4cd8b050289266a)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1s9df8524dbe564521816246781a756bbf)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s340edd3a883b4ef7ac90874731c865b0/resources/u3i1s8bcadab57dfe4c52b08edde230ce5b59)



## 1️⃣ `final` reference to a mutable list

`final List<String> list = new ArrayList<>(); list.add("Hello"); // ✅ allowed list.add("World"); // ✅ allowed`

- `final` **prevents reassigning the reference**:


`list = new ArrayList<>(); // ❌ compilation error`

- But the **object itself is mutable**, so you **can add, remove, or modify elements**.


✅ So here, you **can add extra data**.

---

## 2️⃣ Immutable list (cannot add/remove)

### Using `Collections.unmodifiableList`:

`List<String> list = new ArrayList<>(); List<String> immutableList = Collections.unmodifiableList(list);  immutableList.add("Hello"); // ❌ throws UnsupportedOperationException`

### Using Java 9+ `List.of()`:

`List<String> immutableList = List.of("A", "B", "C"); immutableList.add("D"); // ❌ throws UnsupportedOperationException`

- Here, the **object itself is immutable**, so you **cannot add or remove elements**, regardless of whether the reference is `final` or not.
- ------------------------------------------------------------------------------

# ✅ **1. What is an Unmodifiable List?**

It’s **NOT** a new list.

It is a **wrapper** around an existing list that:

- Allows reading (get, size, contains)

- **Blocks ALL modifications** (add, remove, clear)

- Throws **UnsupportedOperationException** for any modifying operation


---

# 🔍 **2. How is it implemented internally?**

Inside Java, there is a class like this:
```
static class UnmodifiableList<E> implements List<E> {
    final List<? extends E> list;

    UnmodifiableList(List<? extends E> list) {
        this.list = Objects.requireNonNull(list);
    }
}

```

### ✔ It stores the original list inside

### ✔ It does NOT copy the list

### ✔ It delegates read operations

### ✔ It throws exceptions for write operations

---

# 🧠 **3. How each method works internally**

### **Read methods → delegated**

```
public E get(int index) {
    return list.get(index);   // delegate to original list
}

public int size() {
    return list.size();
}

```

### **Write methods → blocked**

```
public void add(int index, E element) {
    throw new UnsupportedOperationException();
}

public boolean add(E e) {
    throw new UnsupportedOperationException();
}

public E remove(int index) {
    throw new UnsupportedOperationException();
}

public void clear() {
    throw new UnsupportedOperationException();
}

```

This applies to:

- add()

- addAll()

- remove()

- removeAll()

- retainAll()

- set()

- sort()

- replaceAll()


All modifying operations **just throw exceptions.**



---------------------------------------------------------------------------------------------


## Wrapper Class


A **Wrapper class** is an object-version of a primitive data type.

Java has 8 primitive types, and each has a corresponding wrapper class:

|Primitive|Wrapper Class|
|---|---|
|`int`|`Integer`|
|`long`|`Long`|
|`short`|`Short`|
|`byte`|`Byte`|
|`float`|`Float`|
|`double`|`Double`|
|`char`|`Character`|
|`boolean`|`Boolean`|

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

### **Reason 3 — Needed for Reflection & Generics**

Reflection, annotations, and generics expect objects.

`Method m = obj.getClass().getMethod("test", Integer.class);`

You cannot pass primitive types directly in reflection metadata.

---

### **Reason 4 — Null support**

Primitives cannot be `null`.

`int a = null;  // ❌ ERROR Integer a = null; // ✔ Valid`

In databases, JSON, REST APIs → fields may be null → wrapper required.

---

### **Reason 5 — Object features: Comparable, serialization**

Primitives cannot:

- be serialized

- compared using `.equals()`

- passed in generics

- stored in collections

- used in streams API


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