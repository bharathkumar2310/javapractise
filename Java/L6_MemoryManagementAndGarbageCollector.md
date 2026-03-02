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

![img.png](../Images/Memory.png)


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


🔹 1️⃣ What is PC Register and COunters?
      
      CPU Register: Program Counter (PC) → points to next machine instruction (interpreter or JIT)
      JVM Counter: bytecodePC → tracks the next bytecode instruction inside the interpreter

        PC = Program Counter Register
        It is a small memory area per thread that stores the address of the current bytecode instruction being executed.
        Each thread in Java has its own PC register.

🔹 3️⃣ What Does It Store?

It stores:

        The address (or index) of the current bytecode instruction
        Only for Java methods
        If a thread is executing a native method, the PC register value is undefined.

🔹 What REALLY happens

Every Java thread has:

      CPU Program Counter (real hardware register)
      JVM Bytecode PC (a value stored inside the thread structure)

🔸 In Interpreter Mode

      Hardware level
      CPU PC → points to interpreter machine code
      JVM level
      Thread structure contains:
      
         bytecodePC → points to next bytecode instruction

The interpreter does:
      
      read bytecode[bytecodePC]
      increment bytecodePC
      execute handler

So:

      CPU executes interpreter code

Interpreter reads bytecode using bytecodePC

🔸 In JIT Mode

After compilation:

      Method converted to native machine code
      Stored in Code Cache
      CPU PC now jumps directly to compiled code

Now:

      There is no bytecode loop anymore for that method.


🔹 JVM-Level PC vs CPU PC
              ConceptName	            Points To	              Where Stored
CPU-level	Program Counter (PC)	Next machine instruction	CPU register
JVM-level	Bytecode PC / Counter	Next bytecode instruction	Thread’s JVM data structure





**_Code Segment And Data Segment :**_ 


      ---> Code Segment and Data Segment are not controlled by JVM rather managed directly by OS
      ---> When you run a Java code
      The OS loads:
         The JVM executable (compiled C/C++ machine code)
         That machine code goes into the code segment  
         It will be like read byte code of our app
         and it interprets byte code and has machine code instructions based on our code
         Interprettor contains machine code to handle data based on our byte code


      Your Java bytecode is stored as data, not in the code segment.
      
      Only when JIT happens:
      
            Your Java method gets compiled into machine code
            That machine code goes into Code Cache
            Not the original OS code segment


      ---> Data Segment contains the static and global variables of JVM executbblae(C/C++ code)
      
      🏗 What Happens During Interpretation?
      
      Suppose bytecode says:
      
      iadd
      That is just a number inside the .class file.
      CPU cannot execute iadd.
      So what happens?
      
         CPU executes JVM interpreter code (from code segment)
         Interpreter reads bytecode
         Interpreter switches on opcode
         Interpreter runs corresponding native logic
      
      
      🏗 Step 1: JVM Is Just a Native Program
      
      The JVM (for example java.exe on Windows or java on Linux) is:
      
            Written in C/C++
            Compiled by a C/C++ compiler
            Converted into machine instructions (0s and 1s)
            So it becomes a normal executable file like:
      
      java.exe
      
      At this point, it is just a binary file stored on disk.
      
      🧠 Step 2: When You Run java MyProgram
      
      When you type:java MyProgram
      
      The OS does this:
      
            Creates a new process
            Loads the java executable into memor
            Maps its sections into memory
      The memory layout looks like:
      
      Process Memory
      ---------------------------------
      Code Segment     ← JVM machine instructions
      Data Segment     ← JVM global variables
      Heap             ← dynamic memory
      Stack            ← thread stacks
      
      The important part:
      
         The Code Segment is marked as executable memory
      
      ⚡ Step 3: How CPU Starts Executing It
      
      Every process has an entry point.
      For JVM, that entry point is a native function like:
      int main(int argc, char* argv[])
      When OS loads the process:
      
         It sets the CPU instruction pointer (IP / RIP register)
         Points it to the JVM’s main function
         CPU begins executing machine instructions from the Code Segment
      
      So:
      
      CPU → fetch instruction from code segment
      CPU → decode
      CPU → execute
      
      This continues instruction by instruction.
      
      🔥 Step 4: Inside JVM Execution
      
      Now JVM native code starts running.
      
      It does:
      
      Initializes memory areas (Heap, MetaSpace, Code Cache)
      Creates main Java thread
      Loads your .class file
      Starts interpreting bytecode
      So even when interpreting:
      
            CPU executes JVM machine instructions
            JVM reads bytecode
            JVM performs operation
      
      The CPU is always executing native JVM machine code.
      
      🧠 Extremely Important Realization
      
      The CPU never executes:
      
            Java source code ❌
            Bytecode ❌
      
      It executes only:
      
            Native machine instructions from executable memory

      
      🔥 1️⃣ Is Bytecode Ever Converted to Machine Code?
      
      It depends.
      
      ✅ If JIT is enabled (which it normally is)
      
      Then:
      
            Bytecode is converted into machine code
            by the JIT compiler.
            That machine code is stored in the Code Cache and executed directly by the CPU.
            So bytecode is sometimes converted to machine code.
      ✅ If Running Only in Interpreter Mode
      
      Then:
      
      Bytecode is NOT converted into machine code.
      
      Instead:
      
         CPU executes interpreter machine code
         Interpreter reads bytecode
         Interpreter performs actions
         No new machine code is generated for your method.