**_PROCESS :_**

    ---> Process is an instance of program being executed and process are managed by OS
    ---> Whenever we run a Java Program Process gets created and OS allocates memory to process and also a new JVM instance is created and loaded into process and also byte code is loaded to process
    
    ---> When a program runs, these things physically exist in RAM and CPU:
    
            Memory allocated in RAM
            CPU registers holding values
            Program counter
            Stack frames
            Heap memory
            OS kernel data structure (PCB)
            All these are real.
    
    The OS groups all of this together and says:
    “This entire execution unit = Process”

![img.png](../Images/process.png)



**_THREADS:**_


    ---> A thread is a lightweight unit of execution within a process
    ---> A thread is a single flow of execution (sequence of instructions) executed by the CPU within a process.
    ---> Whenever we run a Java prog process is create with only one thread known as main Thread


![img.png](../Images/Thread.png)


🔹 What is a JVM Thread?

    A JVM thread:
    
        Is created using new Thread() in Java
        Managed partly by JVM
    
    Has:
    
        Java stack
        Program counter
        Thread state (NEW, RUNNABLE, BLOCKED, etc.)

An OS thread:

    Is created by the operating system
    Scheduled by OS scheduler
    
    Has:
    
        CPU registers
        Native stack
        Kernel-level structure
    
    It is managed entirely by the OS kernel.



![img.png](../Images/ThreadMemoryModel.png)


How They Work Together

        CPU executes JVM interpreter/JIT machine code.
        Interpreter code may read/write JVM “registers” (memory) for temporary values.
        CPU PC register points to interpreter instructions, not to JVM bytecode or JVM registers.
        JVM bytecodePC (counter) tells interpreter which bytecode instruction to read next.
        So the JVM “registers” are simulated, while CPU registers are real hardware.
        JVM register is used to store intermediate values so that it will be helpful for context switching



-------------------------------------------------------------------------------------------------------------------------------------------------



1. JVM main thread

When you run:

public class Test {
public static void main(String[] args) {
System.out.println("Hello");
}
}

JVM creates its main thread object for Thread.currentThread() → main.

This is the java.lang.Thread object representing the main thread in the JVM.

So yes, the JVM itself has a Thread object for the main thread.

2. OS thread for main

When the JVM process starts, the OS creates a main thread for that process (all processes start with a main thread at the OS/kernel level).

JVM links the Thread object to this OS thread.

So the main thread is both a JVM thread object and a native OS thread.

In other words:

Concept	Exists?	Notes
JVM main Thread	✅ Yes	java.lang.Thread object
OS main Thread	✅ Yes	Created by OS when process starts
Linked?	✅ Yes	JVM thread object references the OS thread underneath
3. Memory & Execution

OS stack: allocated by OS for main thread.

JVM stack: JVM may use the OS stack for its frames (method calls, locals) or maintain additional Java stack frames.

Registers, TCB, state: all managed by OS.

JVM thread object: holds Java-level info like thread name, priority, ThreadLocals, thread state.

So when your main method runs:

CPU executes OS main thread instructions.

JVM interprets those instructions as running on main Thread object.

Any new threads you create are separate OS threads, each with its own JVM thread object.


-------------------------------------------------------------------------------------------------------------------------------------------



so whenever we create athread JVM will just create a thread obj and Os will create the actual thread and it is linked 
and JVM will jave some memory in its allocaetd memory for each thread and OS will have seperate memory for each thread
SO for a thread JVM will allocate seperate memory and OS will allocate seperate memory

---------------------------------------------------------------------------------------------------------------------------------------------------


✅ 1️⃣ Thread creation

    Whenever we create a thread JVM thread obj is created which is linked to native thread created by OS

✔️ Correct (for HotSpot JVM)

        Java uses 1:1 threading model:
        Java Thread object (heap)
        ↕
        Native OS thread

The JVM calls the OS to create a real native thread.

⚠️ 2️⃣ “Each thread will have JVM counters for pointing to next byte code in meta space”

Almost correct, but wording needs fixing.
Correct version:

Each Java thread has:

        ✅ JVM Program Counter (PC)
        Points to the next bytecode instruction
        NOT pointing to Metaspace
        Points to bytecode inside the method area (Metaspace)

So:

Bytecode is stored in Metaspace
JVM PC points to the current instruction inside that bytecode



⚠️ 3️⃣ “JVM registers to store intermediate thread values”

Not exactly correct terminology.
There are no general “JVM registers” like CPU registers.

Instead, each thread has:

        ✔️ Operand Stack (inside each stack frame)
        ✔️ Local Variable Array
        ✔️ Java Stack

These store intermediate values during execution.

So replace: JVM registers

With: Operand stack and local variables inside stack frames

⚠️ 4️⃣ “PC registers which is physical hardware”

This is where things mix.

There are TWO PCs:

🔹 JVM PC (Logical)

        One per Java thread
        Tracks next bytecode instruction
        Stored in JVM thread structure

🔹 CPU PC (Hardware register)

        Exists inside CPU core
        Points to next machine instruction
        Used when native machine code executes

So:

    ❌ JVM PC is NOT physical hardware
    ✔️ CPU PC is hardware

⚠️ 5️⃣ “Caches for each thread”

        Not correct.
        CPU cache (L1/L2/L3):
        Belongs to CPU core
        NOT per thread
Important:

        ✅ L1 cache → private per core
        ✅ L2 cache → usually private per core
        ✅ L3 cache → shared across cores
        Multiple threads may use same cache when scheduled on same core

Threads do NOT have their own hardware caches.