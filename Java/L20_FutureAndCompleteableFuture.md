Async (Asynchronous) means:

    A task runs independently, and the caller does not wait for it to finish.
    The caller continues execution immediately.
    Async is about behavior, not about number of threads.

When does Java become async?

Only when you explicitly introduce it:
      
      ✅ Using Thread
      new Thread(() -> {
      System.out.println("Async Task");
      }).start();

✅ Using ExecutorService
      
      ExecutorService executor = Executors.newFixedThreadPool(2);
      executor.submit(() -> System.out.println("Async"));

✅ Using CompletableFuture (Modern way)

      CompletableFuture.runAsync(() -> {
      System.out.println("Async Task");
      });

FUTURE :

    ---> is an interface that represents the result of an asynchronous computation.
When you submit a task to:

    Thread
    ExecutorService
    ThreadPoolExecutor

The task runs in another thread (async).

But how do you:

    Get the result?
    Check if it finished?
    Cancel it?
👉 That’s what Future provides.

![img.png](../Images/Future.png)

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws Exception {

        ExecutorService executor = Executors.newFixedThreadPool(2);

        Future<String> future = executor.submit(() -> {
            Thread.sleep(2000);
            return "Hello World";
        });

        System.out.println("Doing something else...");

        String result = future.get();  // blocks until result ready
        System.out.println("Result: " + result);

        executor.shutdown();
    }
}

```


2. Result stored safely
   Stored in FutureTask
   Uses volatile + synchronization internally
3. future.get() does 2 critical things:
   ✅ (1) WAITS (blocking)
   Main thread pauses until result is ready

METHODS :


![img_1.png](../Images/FutureMethods.png)



![img.png](../Images/FutureEx.png)



Future<?> future = exec.submit(Runnable runnable)

---> Here submit method returns runnable future which is an interface which extends Future and Runnable

```java
 public Future<?> submit(Runnable task) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<Void> ftask = newTaskFor(task, null);
        execute(ftask);
        return ftask;
    }


```

```java

public interface RunnableFuture<V> extends Runnable, Future<V> {
    void run();
}

```

Here Future Task is the implementation of RunnableFuture

In the neTaskFor() method the runnable is wrapped as a future task and returned

```java
public class FutureTask<V> implements RunnableFuture<V> {


    private volatile int state;
    private static final int NEW = 0;
    private static final int COMPLETING = 1;
    private static final int NORMAL = 2;
    private static final int EXCEPTIONAL = 3;
    private static final int CANCELLED = 4;
    private static final int INTERRUPTING = 5;
    private static final int INTERRUPTED = 6;

    private Callable<V> callable;


    public FutureTask(Runnable runnable, V result) {
        this.callable = Executors.callable(runnable, result);
        this.state = NEW;       // ensure visibility of callable
    }

    protected <T> RunnableFuture<T> newTaskFor(Runnable runnable, T value) {
        return new FutureTask<T>(runnable, value);
    }
}

```

Future Task contains a field called callable so the runnable is wrapped as RunnableAdaptor which implements callable and overrides call()
method . Inside the call method() it calls runnable.run()


Executors.callable(.....)

```java

public static <T> Callable<T> callable(Runnable task, T result) {
        if (task == null)
            throw new NullPointerException();
        return new RunnableAdapter<T>(task, result);
    }

```


```java
private static final class RunnableAdapter<T> implements Callable<T> {
    private final Runnable task;
    private final T result;

    RunnableAdapter(Runnable task, T result) {
        this.task = task;
        this.result = result;
    }

    public T call() {
        task.run();
        return result;
    }
}
```

Inside execute method it calls futureTask.run()

```java

public void run() {
        if (state != NEW ||
            !RUNNER.compareAndSet(this, null, Thread.currentThread()))
            return;
        try {
            Callable<V> c = callable;
            if (c != null && state == NEW) {
                V result;
                boolean ran;
                try {
                    result = c.call();
                    ran = true;
                } catch (Throwable ex) {
                    result = null;
                    ran = false;
                    setException(ex);
                }
                if (ran)
                    set(result);
            }
        } finally {
            // runner must be non-null until state is settled to
            // prevent concurrent calls to run()
            runner = null;
            // state must be re-read after nulling runner to prevent
            // leaked interrupts
            int s = state;
            if (s >= INTERRUPTING)
                handlePossibleCancellationInterrupt(s);
        }
    }

    

```
in that run it calls callable.call() which is runnableAdaptor.call() ---> which internally calls runnable.run()


2. exec.submit(Callable call)

----> for submit of callable runnable Adaptor is not used in FutureTask.run() it calls callable.call() and the callable task directly executes


Limitations of Future ❗ (VERY IMPORTANT)

      ❌ Blocking (get())
      ❌ No chaining
      ❌ No callbacks
      ❌ Hard to combine multiple tasks
      ❌ Poor exception handling

👉 This is why CompletableFuture was introduced

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------



**_COMPLETABLE FUTURE :_**


    ---> CompletableFuture is an advanced implementation of Future
    ---> A future result that can be manually completed and chained with other async computations.
    ---> It implements both: Future and CompletionStage

![img_1.png](../Images/CompFuture.png)

ADVANTAGES :


1. Future is blocking , say for example when we give future.get() main thread has to wait for async operation to complete
   But with completable future we have chainig operations like thenAccept, thenCompose etc which alllows us to work with async results without making main thread wait




1. SupplyAsync()

✅ What it does:

        Takes a Supplier<U>
        Runs it asynchronously
        Returns a CompletableFuture<U>

Completes the future with the result of supplier.get()


CompletableFuture wraps the Supplier inside a Runnable implementation (like AsyncSupply), and inside run(), it calls supplier.get().


```java

import java.util.concurrent.CompletableFuture;

public class Main {
    public static void main(String[] args) {

        System.out.println("Main thread: " + Thread.currentThread().getName());

        CompletableFuture<String> future =
                CompletableFuture.supplyAsync(() -> {
                    System.out.println("Async thread: " + Thread.currentThread().getName());

                    try {
                        Thread.sleep(2000); // simulate long task
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    return "Hello from Async Task";
                });

        future.thenAccept(result ->
                System.out.println("Result: " + result)
        );

        System.out.println("Main thread continues...");
    }
}

```

Here by default fork join pool is used by Completable Future
ForJoin Pool uses daemon threads by default 

We can provide custome executors too


![img.png](../Images/CF1.png)

![img_1.png](../Images/CF2.png)



2. thenApply?

thenApply is a completion stage method of
CompletableFuture that:

    Attaches a function to a CompletableFuture
    Executes after the previous stage completes normally
    Accepts a Function<T, U> (takes input, produces output)
    Returns a new CompletableFuture<U> representing the transformed result
(takes function functional interface as parameter)

----> It Applies a function to the reult of the prvios async completable future obj
----> Returns a new COmpetable Future Obj


![img_2.png](../Images/CF3.png)

![img_3.png](../Images/CF4.png)

![img_4.png](../Images/CF5.png)


```java
public void method1() throws InterruptedException {
        ThreadPoolExecutor exec = new ThreadPoolExecutor(2,3,5, TimeUnit.SECONDS,new ArrayBlockingQueue<>(5), Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy());
        System.out.println(Thread.currentThread().getName());

        CompletableFuture<Integer> fut = CompletableFuture.supplyAsync(() -> {
            System.out.println(Thread.currentThread().getName());
            return 1;
        });

//        Thread.sleep(500);
        CompletableFuture<String> fut1 = fut.thenApply((val) -> {
            System.out.println(Thread.currentThread().getName());

            return val + "World";
        });
    }

```


Here supply async will be done by seperate ForkJoinPool thread
And when it comes to accept by the time main hits then apply if the supply async task has been completed then main thread will execute the thenApply
else the same thread which executed supplyAsync() will execute this


thenApplyAsync() : Similar to thenApply() but not hte same thread will execute thenApplyAsync either a new thread or the existing thread which has returned to the pool will excecute it




1️⃣ What is thenCompose and thenCompose Async?

thenCompose is used when your continuation itself returns a CompletableFuture.
It flattens nested futures into a single CompletableFuture
Avoids having a CompletableFuture<CompletableFuture<T>>

Helps chain dependent async tasks sequentially

![img_5.png](../Images/CF6.png)



1️⃣ What is thenAccept?

      thenAccept is a completion stage method that:
      Runs after a previous CompletableFuture completes normally
      Takes a Consumer<T> → consumes the result of the previous stage without producing a new result
      Returns a CompletableFuture<Void>
      Usually used for side effects like printing, logging, saving to DB, etc.

```java
import java.util.concurrent.*;

public class ThenAcceptExample {
   public static void main(String[] args) {
      CompletableFuture<Integer> fut = CompletableFuture.supplyAsync(() -> {
         System.out.println("Supply Thread: " + Thread.currentThread().getName());
         return 42;
      });

      fut.thenAccept(val -> {
         System.out.println("thenAccept Thread: " + Thread.currentThread().getName());
         System.out.println("Result: " + val);
      });

      // Wait for async tasks to finish
      fut.join();
   }
}


```
![img.png](../Images/CF7.png)

1️⃣ What is thenCombine?

      thenCombine is used to combine results of two independent CompletableFutures when both complete normally
      It takes a BiFunction<T, U, V> → combines the two results into one new value
      Returns a new CompletableFuture<V> containing the combined result


```java

import java.util.concurrent.*;

public class ThenCombineExample {
    public static void main(String[] args) {
        CompletableFuture<Integer> fut1 = CompletableFuture.supplyAsync(() -> {
            System.out.println("fut1 Thread: " + Thread.currentThread().getName());
            return 10;
        });

        CompletableFuture<Integer> fut2 = CompletableFuture.supplyAsync(() -> {
            System.out.println("fut2 Thread: " + Thread.currentThread().getName());
            return 20;
        });

        CompletableFuture<Integer> combined = fut1.thenCombine(fut2, (x, y) -> {
            System.out.println("thenCombine Thread: " + Thread.currentThread().getName());
            return x + y;
        });

        combined.thenAccept(sum -> System.out.println("Sum: " + sum));

        // Wait for completion
        combined.join();
    }
}



```

![img_1.png](../Images/CF8.png)



-------------------------------------------------------------------------------------------------------------


| Feature            | submit()    | execute()       |
| ------------------ | ----------- | --------------- |
| Return             | Future      | void            |
| Exception handling | Captured    | Lost            |
| Use case           | Need result | Fire-and-forget |


🔹 5. Multiple Futures
👉 16. allOf()

Wait for all futures

      CompletableFuture.allOf(f1, f2, f3);

👉 17. anyOf()

      Returns first completed future


| Feature   | get()   | join()    |
| --------- | ------- | --------- |
| Exception | Checked | Unchecked |
| Usage     | Legacy  | Preferred |


👉 join() is available in: CompletableFuture



🔹 1. Exception Handling in Future
👉 How exceptions are handled?
      
      Future<Integer> future = executor.submit(() -> {
      throw new RuntimeException("Error");
      });
      
      try {
      future.get();
      } catch (Exception e) {
      System.out.println(e);
      }

🔥 What actually happens?

      Exception is captured internally
When you call get():

      It throws ExecutionException
      Actual exception is wrapped inside

👉 How to access real exception?

      catch (ExecutionException e) {
      Throwable actual = e.getCause();
      }
❗ Problems with Future
      
      ❌ Only handled at get() (blocking point)
      ❌ No async handling
      ❌ No fallback mechanism
      ❌ No chaining
      ❌ Wrapped exceptions → messy

🔹 2. Exception Handling in CompletableFuture

👉 Much more powerful & flexible

✅ 1. exceptionally() (Most common)
      
      CompletableFuture<Integer> cf =
      CompletableFuture.supplyAsync(() -> {
      throw new RuntimeException("Error");
      });
      
      cf.exceptionally(ex -> {
      System.out.println(ex);
      return 0;
      });

✔ Provides fallback value
✔ Non-blocking

✅ 2. handle() (VERY IMPORTANT)
      
      cf.handle((result, ex) -> {
      if (ex != null) return 0;
      return result;
      });

✔ Gets both:
      
      result
      exception

✔ Can recover or transform

✅ 3. whenComplete()
      
      cf.whenComplete((res, ex) -> {
      System.out.println("Done");
      });
      
      ✔ Just observes
      ❌ Cannot change result

🔹 3. Comparison Table 🔥

| Feature           | Future             | CompletableFuture   |
| ----------------- | ------------------ | ------------------- |
| Handling style    | Blocking (`get()`) | Non-blocking        |
| Exception type    | ExecutionException | Original exception  |
| Chaining          | ❌                  | ✔                   |
| Fallback support  | ❌                  | ✔ (`exceptionally`) |
| Flexible handling | ❌                  | ✔ (`handle`)        |
