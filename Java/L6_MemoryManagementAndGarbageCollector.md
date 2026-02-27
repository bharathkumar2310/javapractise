                JVM
                │
    ┌───────────┴───────────┐
    │                       │
Class Loader          Runtime Data Areas
Subsystem                   │
│                       │
│               ┌───────┴────────┐
│               │                │
│             Heap            Method Area
│               │                │
│             Stack          Metaspace
│
└──────────────→ Execution Engine
│
┌─────┼─────┐
│     │     │
Interpreter JIT  GC
│
Code Cache



JVM Memory
│
├── Heap (Shared)
│     ├── Young Gen (Eden + S0 + S1)
│     ├── Old Gen
│     └── String Pool
│
├── Method Area (Shared)
│     ├── Class Metadata
│     ├── Method Metadata
│     ├── Static Variables
│     ├── Runtime Constant Pool
│     └── Bytecode
│
├── Stack (Per Thread)
│     └── Stack Frames
│
├── PC Register (Per Thread)
│
└── Native Method Stack




Process Memory
│
├── JVM Managed Memory
│     ├── Runtime Data Areas
│     │     ├── Heap
│     │     ├── Metaspace
│     │     ├── Stack
│     │     ├── PC Register
│     │     └── Native Stack
│     │
│     ├── Code Cache
│     ├── Direct Memory
│     └── GC Structures
│
└── Other OS Memory





**_RUNTIME DATA AREAS :_**

1. Stack : 


---> Each thread in Java consists of seperate Stack
---> Stack is not shared between 2 threads
---> Whenever a method is called a new stack frame is created

---> A Stack Frame is: A single method execution block inside the stack.

Top of Stack
┌──────────────┐
│ methodB()    │  ← Stack Frame
├──────────────┤
│ methodA()    │  ← Stack Frame
├──────────────┤
│ main()       │  ← Stack Frame
└──────────────┘
Bottom of Stack


Each stack frame contains:

    1️⃣ Local Variables
    2️⃣ Method Parameters
    3️⃣ Operand Stack
    4️⃣ Return Address(the method which it needs to return)
    5. Reference variables

         --> Strong reference
         --> Weak reference
         --> Soft reference

        ---> Reference variables hold the address of the objects like below
       User u = new User()
        u → 0x7f12a0

    The return variable is added inside the caller method's stack frame befor this method gets popped
    Variables are visible only within the scope (that is within the stack frame)
    When goes out of stack(method gets completed) variables gets deleted in LIFO order


When stack memory becomes full it throws java.lang.StackOverFlow error


🔹 1️⃣ Strong References (Default)

What it is: The normal references you use every day.

Syntax:

        User u = new User();  // strong reference

GC Behavior:

        As long as a strong reference exists, object is never garbage collected.

Example:

    User u = new User();
    u = null;  // Now object is eligible for GC

Default in Java, used 99% of the time.

🔹 2️⃣ Soft References

    Purpose: Useful for caching objects.

Syntax:

    SoftReference<User> softUser = new SoftReference<>(new User());

GC Behavior:

    Object is collected only if JVM needs memory
    Otherwise, it stays in memory

Use Case:

    Caches that can be cleared if memory is low, e.g., image cache, computed values

Example:

    SoftReference<byte[]> cache = new SoftReference<>(new byte[1024*1024]);
    System.gc();  // JVM may or may not clear it depending on memory

🔹 3️⃣ Weak References

Purpose:

    Allow objects to be collected more aggressively than soft references.

Syntax:

    WeakReference<User> weakUser = new WeakReference<>(new User());

GC Behavior:

    Object is eligible for GC as soon as there are no strong references
    Used in maps or listeners where you don’t want memory leaks

Example:

    WeakHashMap<String, User> map = new WeakHashMap<>();
    map.put("key", new User());  // entry will disappear when key is no longer strongly referenced

🔹 4️⃣ Phantom References

For completeness (less commonly used):

    Purpose: Track objects after finalization but before actual memory is reclaimed

Syntax:

    PhantomReference<User> phantom = new PhantomReference<>(new User(), referenceQueue);

GC Behavior:

    Cannot get the object (get() always returns null)
    Mainly used to clean up resources manually


HEAP : 

    ---> A shared memory area where all Java objects and arrays are allocated at runtime.
    ---> It is managed by JVM Garbage Collector
    ---> Shared among all threads


| Item                          | Stored Where | Notes                              |
| ----------------------------- | ------------ | ---------------------------------- |
| Objects                       | Heap         | All instance variables live here   |
| Arrays                        | Heap         | Even primitive arrays              |
| String literals (String Pool) | Heap         | Inside special pool area           |
| Class instances               | Heap         | References to them stored in stack |


---> Garbage Collector is used to clean unreferenced heap objects

---> Heap is divided into 2 parts : Young Generation and Old Generation
---> Young Generation is divided into Eden , S0, S1(S0 and S1 are called survivor space)


Whenever we create an object newly the object is stored in eden
Whenever Garbage Collector runs it does something called mark and sweep algortihm

First it marks all the objects from young generation which doesnot have any reference , 
Then it removes the marked objects from memory 
Then it sweeps all the remaining objects from eden to S0 with age = 1
After removing the objects from memory and sweeping there will be holes in middle where memory is free so it does compaction i.e gps are filled with subsequent memory and the gaps are always at the end

The second time GC runs same thing happens when sweeping it also sweeps S0 to S1 with age = 2 and nextime S1 to S0 with incrementing ages

When the age reaches some threshold these objects are moved from young generation to old generation


This process of Garbage Collector doing mark and sweep in young generation is called minor Garbage collection


This process of Garbage Collector marking and removing objects in older generation is called major Garbage collection
Major gc occurs less frequently than minor gc

    When heap gets full it throws Out of Memory error


Types of GC;

1. Serial GC :

---> Only one thread is used for GC
----> GC is expensive since all other threads is passed when GC happens
----> If one thread is there will be more pause time

2. Parallel GC :


---> Multiple threads is used for GC
---> It will reduce the pause time very much lesser

3. COncurrent Mark and Sweep :

----> It tries to run concurrntly both GC thread and other threads but does not gaurantee 100%
---> It doesnot do compaction
--->Next allocation of a large object may fail if there’s no large enough contiguous space, even if total free memory is enough.

4. G1 Garbage COllection:

---> SImilar to Concurrent Mark and Sweep with compaction



🔹 1️⃣ Manual GC Invocation

In Java, you can hint to the JVM that you want garbage collection:

    System.gc();
    
    or
    
    Runtime.getRuntime().gc();

This does not guarantee that GC will run immediately.
It’s only a request, JVM may ignore it.
Modern JVMs generally manage GC automatically for efficiency.

🔹 3️⃣ When JVM Runs GC Automatically

    JVM triggers GC based on memory usage and garbage accumulation. Some common triggers:

1️⃣ Heap is getting full

    If Eden space (Young Generation) or Old Generation is full, JVM runs Minor or Major GC.

2️⃣ Allocation Failure

    When new cannot allocate enough contiguous memory in heap.

3️⃣ Soft Reference cleanup

    JVM may run GC to clear soft references if memory is low.


Java 9 and above uses G1 GC by default



----------------------------------------------------------------------------------------------------------------------------------



🔹 1️⃣ What is Metaspace?

    Metaspace is a special area of memory in the JVM that stores class metadata.
    Introduced in Java 8 (replacing PermGen)
    Unlike PermGen, it lives in native memory (outside the heap)
    Managed by the JVM automatically
    No fixed size by default; can grow as needed, limited by OS memory

| Component                            | Description                                                                                       |
| ------------------------------------ | ------------------------------------------------------------------------------------------------- |
| **Class Metadata**                   | Information about loaded classes: methods, fields, modifiers, superclass, interfaces, annotations |
| **Method Metadata**                  | Bytecode for methods, method descriptors, and related info                                        |
| **Runtime Constant Pool**            | Class-level constants (strings, numbers, method references, etc.)                                 |
| **Static Variables**                 | Class-level static fields                                                                         |
| **Native pointers / auxiliary data** | JVM internal data structures for classes                                                          |


    Method Area = what the JVM conceptually requires to store class-level info.
    Metaspace = the physical implementation of Method Area in modern JVMs (Java 8+).

   Here only static variable metadat is present that is this class contains this staic variables
   Actual value is stored in heap in class Object

💡 Analogy:

    Method Area → blueprint of a house (concept)
    Metaspace → the actual land and building where the blueprint is realized


🔹 1️⃣ What is PC Register?

        PC = Program Counter Register
        It is a small memory area per thread that stores the address of the current bytecode instruction being executed.
        Each thread in Java has its own PC register.

🔹 3️⃣ What Does It Store?

It stores:

        The address (or index) of the current bytecode instruction
        Only for Java methods
        If a thread is executing a native method, the PC register value is undefined.