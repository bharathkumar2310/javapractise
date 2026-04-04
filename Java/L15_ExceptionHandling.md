Exception : 

![img.png](../Images/Exc1.png)

![img.png](../Images/ExcEx1.png)

    
    At runtime when JVM executes byte code(converting byte code to machine code) it has codes to find custom exception like it checks if divisor = 0
    then create a new Arithmetic Exception object 
    
    Once it creates it will look into its stack trace in LIFO order and checks if the exception is handled via 
    catch block , if yes it will execute the catch block and continue excuting, else it will terminate the app while printing exception obj


🔹 1️⃣ What Happens When Exception Propagates

```java

public class Test {
    static void a() { b(); System.out.println("After b in a"); }
    static void b() { c(); System.out.println("After c in b"); }
    static void c() { int x = 10 / 0; System.out.println("After x in c"); }

    public static void main(String[] args) { 
        try { a(); } catch (ArithmeticException e) { System.out.println("Caught"); }
        System.out.println("End"); 
    }
}

```

}
🔹 2️⃣ Step-by-Step Execution

    main() calls a()
    a() calls b()
    b() calls c()
    c() executes 10 / 0 → exception occurs

JVM creates ArithmeticException object
JVM starts stack unwinding:

    Looks for a matching catch in c() → none → pops c() frame
    Looks in b() → none → pops b() frame
    Looks in a() → none → pops a() frame
    Looks in main() → finds catch (ArithmeticException) → jumps to catch block

🔹 3️⃣ What Happens to the Remaining Statements

All statements after the exception in the methods that were popped are skipped

In the example:

        System.out.println("After x in c"); → skipped
        System.out.println("After c in b"); → skipped
        System.out.println("After b in a"); → skipped
        Only System.out.println("Caught"); in main() runs
        Then System.out.println("End"); runs



![img.png](../Images/ExcHeirarchy.png)



🔹 2️⃣ What is an Error?
        
        Definition: An Error is a serious problem that the JVM itself encounters, usually beyond the program’s control.
        Errors are usually not recoverable, meaning handling them is generally not practical.
        Error is mostly unchecked/runtime exception

Examples:
        
        StackOverflowError → thread stack full
        OutOfMemoryError → heap full
        InternalError, UnknownError


----> We cannot catch errors because if u ctach a stack over flow error and continue still the issue is there
----> We only can do code changes or change in JVM setting , we cannot catch/ handle it


===> There are 2 types of exceptions

1. Checked/Compile Time Exception
2. Unchecked/ Run Time Exception

1. Checked/Compile Time Exception :

       ---> The compiler will not compile the code if this exceptions are not handled .
       ---> For example u r reading a file even if the file is present or not u need to handle it 
       ---> Compiler just checks there is a chance for this exception and is it handled or not, If not handled it will not compile code
       ---> JVM will create exception for this
       ---> Compiler will force to handle them
       ---> TO avoid compiler from compiling all compile time exceptions stack trace must either been caught(catch)  or declared(throw)


2. Runtime exception/Unchecked Exception :

        ---> COmpiler will compile even if an exception is there
        ---> COmpiler will not force us to handle them
        ---> JVM will create exception and if we handle it is ok else it will terminate the program


Since runtime exception compiler does not force u you no need to throw the exception JVM will automatically trace back to the caught stack trace
But for compile time exception u need to throw till a caller handles it because compiler doesnot trace back it needs to know while creating exception JVm doesnot need throws

![img.png](../Images/RE1.png)

![img_1.png](../Images/RE2.png)


![img_2.png](../Images/RE3.png)

![img_3.png](../Images/RE4.png)


Illegal arg occurs when we pass the same type of data type but the argument is not a valid argument
For ex parseInt(String) expects String but it should be "1", "2" like that not "String" this exception is thrown by the parseInt() method
This is basically a custom exception thrown by the parseInt() itself.

![img_4.png](../Images/CT5.png)

![img.png](../Images/CT1.png)

![img_1.png](../Images/CT2.png)

![img_2.png](../Images/CT3.png)

![img_3.png](../Images/CT4.png)



🔹 1️⃣ IOException

      Occurs during input/output operations (file, network, streams)
      Compiler forces you to handle or declare


```java
import java.io.*;

public class TestIO {
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("file.txt"); // may throw FileNotFoundException
    }
}
```

FileNotFoundException is a subclass of IOException → checked


🔹 2️⃣ ClassNotFoundException

      Occurs when trying to load a class at runtime via reflection
      Compiler forces handling
```java
public class TestClass {
    public static void main(String[] args) throws ClassNotFoundException {
        Class.forName("com.example.UnknownClass"); // may throw ClassNotFoundException
    }
}
```

🔹 3️⃣ SQLException

      Occurs during database operations
      Checked exception → must handle
```java
import java.sql.*;

public class TestDB {
public static void main(String[] args) throws SQLException {
Connection conn = DriverManager.getConnection("jdbc:mysql://localhost/test", "user", "pass");
}
}

```

🔹 4️⃣ FileNotFoundException

      Subclass of IOException
      Thrown if file does not exist while reading

```java
import java.io.*;

public class TestFile {
public static void main(String[] args) throws FileNotFoundException {
FileInputStream fis = new FileInputStream("nofile.txt");
}
}
```

🔹 5️⃣ InterruptedException

      Occurs when a thread is interrupted
      Must handle or declare

```java
public class TestThread {
public static void main(String[] args) throws InterruptedException {
Thread.sleep(1000); // may throw InterruptedException
}
}
```

If some other thread calls Thread.currentThread().interrupt(),
it needs to be caught to ensure us knowing that some thread interrupted it

![img_5.png](../Images/EH6.png)

![img.png](../Images/EH1.png)

![img_1.png](../Images/EH2.png)

![img_2.png](../Images/EH3.png)


![img_3.png](../Images/EH4.png)


![img_4.png](../Images/EH5.png)


![img_6.png](../Images/EH7.png)


![img_7.png](../Images/EH8.png)
![img_8.png](../Images/EH9.png)


![img_9.png](../Images/EH10.png)

![img_10.png](../Images/EH11.png)


![img_11.png](../Images/EH12.png)
![img_12.png](../Images/EH13.png)





```

Throwable()
Throwable(String message)
Throwable(String message, Throwable cause)
Throwable(Throwable cause)
Throwable(String message, Throwable cause,
          boolean enableSuppression,
          boolean writableStackTrace)


String getMessage()
String getLocalizedMessage()



Throwable getCause()
Throwable initCause(Throwable cause)


void printStackTrace()
void printStackTrace(PrintStream s)
void printStackTrace(PrintWriter s)
StackTraceElement[] getStackTrace()
void setStackTrace(StackTraceElement[] stackTrace)
Throwable fillInStackTrace()


// from Object class

String toString()
boolean equals(Object obj)
int hashCode()
Class<?> getClass()

```


Exception


```
Exception()
Exception(String message)
Exception(String message, Throwable cause)
Exception(Throwable cause)
Exception(String message, Throwable cause,
          boolean enableSuppression,
          boolean writableStackTrace)


```

RunTimeException

```

RuntimeException()
RuntimeException(String message)
RuntimeException(String message, Throwable cause)
RuntimeException(Throwable cause)
RuntimeException(String message, Throwable cause,
                 boolean enableSuppression,
                 boolean writableStackTrace)

```


Error


```
Error()
Error(String message)
Error(String message, Throwable cause)
Error(Throwable cause)
Error(String message, Throwable cause,
      boolean enableSuppression,
      boolean writableStackTrace)
```

Whenever Jvm encounters a exception say x/0 it creates a ArithmeticException obj

ArithmeticException("divideByZero")---> ArithmeticExpression(msg) ---> Here cause will be null

When we are cathing it

catch(Exception e) {
  throw new RuntimeException("Invalid", e);
}


here Arithmetic Exception obj becomes the cause for our newly created obj


boolean writableStackTrace ---> By default it will be false 
By default JVM will store all the stack trace like method, class line no etc
If set to false it wont save


👉 JVM can propagate checked exceptions
👉 But compiler does not allow it unless you declare throws