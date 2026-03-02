**_MULTI THREADING:_**


----> MUltiThreading is a technique by which multiple tasks run simultaneously (i.e) multiple threads are executed within a process simultaneously
----> It looks like multiple threads are executed parallely but it is not parallel execution context switching happens


Threads share the same memory:

    All threads of a process share code, data, and resources.
    Each thread has its own stack (local variables, execution context).

Concurrency vs Parallelism:

    Concurrency: Threads take turns executing on a single CPU core.
    Parallelism: Threads run simultaneously on multiple CPU cores.

Lightweight:

    Creating threads is cheaper than creating separate processes because threads share resou
rces.






Uses of Multithreading

        Improved Performance:
        Run multiple tasks simultaneously to utilize CPU efficiently.
        Example: A video player can decode video, play audio, and handle user input simultaneously.
        
        Responsive Applications:
        In GUI apps, background tasks like downloading files or fetching data can run in a thread while the main thread keeps the interface responsive.
        
        Resource Sharing:
        Threads can easily share data and resources of the parent process without inter-process communication (IPC).
        
        Real-time Applications:
        Useful in systems that need to perform multiple operations at once, e.g., embedded systems, games, or stock trading platforms.
        
        Server Handling Multiple Clients:
        Web servers use threads to handle multiple client requests concurrently.
        When multithreading helps even on 1 core
        
        I/O-bound tasks:
        If Task A is waiting for a disk or network, the CPU can switch to Task B instead of idling.
        This reduces idle CPU time and improves throughput.
        
        GUI responsiveness:
        User interface thread can remain responsive while background tasks run in separate threads.


When multithreading helps even on 1 core

    I/O-bound tasks:
    If Task A is waiting for a disk or network, the CPU can switch to Task B instead of idling.
    This reduces idle CPU time and improves throughput.
    
    GUI responsiveness:
    User interface thread can remain responsive while background tasks run in separate threads.



![img.png](../Images/MultiThreading.png)



Thread Creation :

2 ways : extend thread class, implement runnable interface


![img.png](../Images/threadcreat1.png)

![img_1.png](../Images/threadcreat2.png)

![img_2.png](../Images/threadcreat3.png)


![img_3.png](../Images/threadcreat4.png)

![img_4.png](../Images/threadcreat5.png)


**_Thread Lifecycle :**_


![img.png](../Images/threadLc1.png)

![img_1.png](../Images/ThreadLc2.png)


    wait() ----> Thread will release monitor lock and wil go to waiting state, Any other thread has to use notify/notify ALl to change from wait to runnable/running
    
    notify() ----> any thread hitting notify will make any random thread which is in wait state to blocked (will wait till lock is released)
    
    notifyAll() ----> any thread htiiting notifyAll will make all the threads in wait state to blocked (will wait till lock is released)

    sleep() ---> It will make the therad to go for timed wait it doesnot release monitor lock , after time it becomes runnable

In Java, wait(), notify(), and notifyAll() only work when the thread holds the monitor lock on the object, which means the thread must be inside a synchronized block or method on that object.


![img.png](../Images/mLex1.png)
![img_1.png](../Images/mLex2.png)
![img.png](../Images/Mlex3.png)
![img_1.png](../Images/MLex4.png)


stop() ---> it will move the thread to terminated. All locks held by the thread are released.
suspend() ---> it will make the thread to suspend,  state will be runnable but no monitor lock is releaes
resume()  ---> resumes the suspended thread

here both stop() and suspend() will cause deadlock situtions

DEADLOCK :

🛑 What is Deadlock?

Deadlock is a situation in multithreading where:

    Two or more threads are blocked forever, each waiting for a resource (lock) held by another thread.
    No thread can make progress.
    System appears “frozen” for those threads.


join()  ----> It makes the current thread to wait for any other thread which the current thread invokes join on


THREAD PRIORITY : 

Thread priority is a hint to the thread scheduler about which threads are more important and should get more CPU time compared to others.
In Java, every thread has a priority value.
Thread scheduler may use this priority to decide the order of execution.
Important: Priority is not guaranteed — actual scheduling depends on the OS and JVM implementation.

| Constant               | Value | Meaning          |
| ---------------------- | ----- | ---------------- |
| `Thread.MIN_PRIORITY`  | 1     | Lowest priority  |
| `Thread.NORM_PRIORITY` | 5     | Default priority |
| `Thread.MAX_PRIORITY`  | 10    | Highest priority |

🛡 What is a Daemon Thread?

    A daemon thread is a background thread that runs in the background to perform supporting tasks.
    Key property: The JVM does not wait for daemon threads to finish when shutting down.
    Contrast: Non-daemon (user) threads keep the JVM alive until they complete.
    
    🔹 Characteristics of Daemon Threads
    
    Background service threads: Usually used for tasks like garbage collection, logging, or monitoring, autosaving
    
    JVM exit behavior:
        
        JVM exits automatically when only daemon threads remain.
        If there are any non-daemon threads running, JVM waits for them.
        Cannot block JVM shutdown: They are not essential for program completion.
    
    Inherited property: If a thread is created by a daemon thread, it inherits the daemon status.

![img_2.png](../Images/ThMeth.png)


