🔥 1. What is Multithreading?
❓ Question

What is multithreading?

✅ Answer (interview-ready)
    
    Multithreading is the ability of a program to run multiple threads concurrently, allowing better CPU utilization and improved performance, especially for I/O-bound or parallel tasks.
    
    👉 Real-world:
    In Spring Boot, each HTTP request is handled by a separate thread.

🔥 2. Process vs Thread
❓ Question

Difference between process and thread?
    
    ✅ Answer
    Process = independent program with its own memory
    Thread = lightweight unit inside a process, sharing memory
    
    👉 Key point:
    
    Threads are faster but can cause synchronization issues due to shared memory.

🔥 3. Ways to Create Threads
❓ Question

How do you create threads?
    
    ✅ Answer
    Extend Thread
    Implement Runnable (preferred)
    Use Callable + Future
    Use ExecutorService (best practice)

👉 Interview line:

    In real applications, we don’t create threads manually; we use thread pools.

🔥 4. Thread Lifecycle
❓ Question

Explain thread lifecycle.
    
    ✅ Answer
    
    States:
    
    NEW
    RUNNABLE
    BLOCKED
    WAITING
    TIMED_WAITING
    TERMINATED
    
    👉 Example:
    
    sleep() → TIMED_WAITING
    waiting for lock → BLOCKED
🔥 5. Race Condition
❓ Question

What is race condition?
    
    ✅ Answer
    
    When multiple threads modify shared data simultaneously, leading to inconsistent results.
    
    👉 Example:
    
    count++; // not atomic
    
    👉 Interview tip:
    
    Always mention “non-atomic operation”

🔥 6. Synchronization
❓ Question

What is synchronization?

✅ Answer

    A mechanism to ensure only one thread accesses critical section at a time.

❓ Synchronized method vs block?
    
    Method → locks entire method
    Block → locks specific section (better performance)
❓ What is intrinsic lock?

    Every object in Java has a monitor lock used by synchronized.

🔥 7. volatile (VERY IMPORTANT)
❓ Question

What is volatile?

✅ Answer
    
    Ensures:
    
    Visibility (latest value visible to all threads)
    Prevents CPU caching issues
    
    👉 Important:
    ❌ Does NOT ensure atomicity
    
    ❓ When to use?
    Flags
    Status variables

🔥 8. Atomic Variables
    ❓ Question
    
    How to handle atomic operations?
    
    ✅ Answer
    
    Use classes from java.util.concurrent.atomic
    
    Example:
    
    AtomicInteger count = new AtomicInteger(0);
    count.incrementAndGet();
🔥 9. Deadlock
❓ Question

What is deadlock?
        
        ✅ Answer
        
        When two or more threads are waiting indefinitely for each other’s locks.
        
        ❓ Conditions for deadlock?
        Mutual exclusion
        Hold and wait
        No preemption
        Circular wait
        ❓ How to prevent?
        Lock ordering
        TryLock with timeout
        Avoid nested locks

🔥 10. wait vs sleep
❓ Question
✅ Answer

wait()	sleep()
Releases lock	Does NOT release lock
Object class	Thread class
Used for communication	Used for delay

🔥 11. notify vs notifyAll
    
    notify() → wakes one thread
    notifyAll() → wakes all threads
    
    👉 Interview tip:
    
    Prefer notifyAll() to avoid missed signals

🔥 12. Executor Framework (VERY IMPORTANT)
❓ Question

Why ExecutorService?
    
    ✅ Answer
    Thread creation is expensive
    Thread pools reuse threads
    Better performance and control

❓ Types of thread pools?
    
    Fixed
    Cached
    Single
    Scheduled
    ❓ Example use case?
    
    Handling multiple API requests efficiently

🔥 13. Callable vs Runnable

Runnable	Callable
No return	Returns result
No exception	Can throw exception
🔥 14. Future
❓ Question

What is Future?
    
    ✅ Answer
    
    Represents result of asynchronous computation
    
    Future<Integer> f = executor.submit(task);
    f.get(); // blocking

🔥 15. Locks (Advanced)

❓ synchronized vs Lock?
synchronized	Lock
Automatic	Manual
Less flexible	More control
No timeout	Timeout possible
❓ ReentrantLock?

Same thread can acquire lock multiple times

🔥 16. ReadWriteLock
    
    Multiple readers allowed
    Only one writer
    
    👉 Use when reads >> writes

🔥 17. Concurrent Collections
❓ Why needed?
    
    Normal collections → not thread-safe
    
    ❓ Examples:
    ConcurrentHashMap
    CopyOnWriteArrayList

🔥 18. ThreadLocal (VERY IMPORTANT)
❓ Question
    
    What is ThreadLocal?
    
    ✅ Answer
    
    Each thread gets its own independent copy of variable
    
    👉 Real-world:
    
    Spring Security context
    Transactions

🔥 19. Producer-Consumer Problem
❓ Question

Explain producer-consumer.
    
    ✅ Answer
    Producer produces data
    Consumer consumes data
    Shared buffer
    
    👉 Best solution:
    
    BlockingQueue

🔥 20. ForkJoinPool
❓ Question

What is ForkJoin?
    
    ✅ Answer
    
    Used for parallel processing by splitting tasks recursively
    
    👉 Example:
    Parallel streams use it internally

🔥 21. CompletableFuture (IMPORTANT)
❓ Question

Why CompletableFuture?
    
    ✅ Answer
    Non-blocking async programming
    Chain multiple tasks
🔥 22. Thread Starvation
❓ Question

What is starvation?
    
    ✅ Answer
    
    When a thread never gets CPU because others dominate
    🎯 1. Avoid Thread Priorities (or use carefully)
    Java allows thread priorities (MIN_PRIORITY to MAX_PRIORITY)
    But they are not strictly guaranteed
    
    👉 Problem:
    
    High-priority threads may dominate CPU
    
    👉 Solution:
    
    Don’t rely on priorities for correctness
    
    🎯 2. Use Fair Locks (ReentrantLock)
    ReentrantLock lock = new ReentrantLock(true); // fairness = true
    
    👉 What it does:
    
    Gives lock to longest waiting thread (FIFO)
    
    👉 Interview line:
    
    “Fair locks prevent starvation by ensuring first-come-first-serve access.”

    Use execuotrs they ensure fairness internally

🔥 23. Livelock
❓ Question
    
    What is livelock?
    
    ✅ Answer
    
    Threads are active but not making progress

🔥 24. Context Switching
❓ Question

What is context switching?
    
    ✅ Answer
    
    CPU switching between threads → overhead

🔥 25. Real-world Questions (VERY IMPORTANT)
❓ How does Spring handle threads?
    
    Uses thread pool (Tomcat)
    Each request → one thread

❓ How to debug multithreading issues?
    
    Logs
    Thread dumps
    Profiling tools


🔥 What is a BlockingQueue?

    A BlockingQueue is a thread-safe queue used in multithreading where:
    
    👉 If the queue is empty → consumer waits
    👉 If the queue is full → producer waits

🔥 How it works (intuition)

Imagine:

    Producer → adds items
    Consumer → removes items
    Case 1: Queue is EMPTY
    Consumer calls take()
    👉 It waits automatically until data comes
    Case 2: Queue is FULL
    Producer calls put()
    👉 It waits automatically until space is free


    “Producer–Consumer is a classic multithreading problem where one thread (producer) generates data and another thread (consumer) processes it, and both share a common buffer.”
    🔥 The Problem
    ⚠️ 1. If producer is too fast
    Buffer becomes full
    Need to stop producer
    ⚠️ 2. If consumer is too fast
    Buffer becomes empty
    Need to wait for producer