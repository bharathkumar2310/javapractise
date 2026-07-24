1. What are Generics in Java?

        Generics allow classes, interfaces, and methods to work with different data types while providing compile-time type safety.
        List<String> names = new ArrayList<>();
        Instead of storing Object, generics specify the exact type.

2. Why were Generics introduced?

Before Java 5:
        
        List list = new ArrayList();
        list.add("Hello");
        
        String s = (String) list.get(0);

Problems:
    
    Explicit casting required
    Runtime ClassCastException
    No compile-time type safety

Generics solve this.

3. Advantages of Generics
    
       Compile-time type safety
       Eliminates casting
       Better readability
       Reusable code
       Detects errors early

4. What is Type Safety?

       Compiler ensures only correct types are inserted.
    
       List<String> list = new ArrayList<>();
       list.add("abc");
    
       // list.add(10); // compile error

5. What is Type Erasure?

       Java removes generic type information during compilation.
    
       List<String>
       List<Integer>
    
       Both become:
    
       List
    
       at runtime.
    
       This is called type erasure.

6. Why does Java use Type Erasure?

           Backward compatibility with older Java versions.
           Old JVMs knew nothing about generics.

7. What information remains after type erasure?

             Generic type parameters are removed
             Bounds remain
    
           Example:
        
           class Box<T extends Number>
        
           becomes roughly:
        
           class Box {
           Number value;
           }

8. Can Generics work with Primitive Types?

          No.
        
          List<int> // invalid
        
          Use wrapper classes:
        
          List<Integer>
Main Reason

        Generics are implemented internally like this:
        
        List<T>
        
        after compilation becomes roughly:
        
        List<Object>
        
        But primitives:
        
        int
        double
        char
        
        are not objects.
        
        So this cannot work:
        
        Object obj = 10; // primitive cannot directly become Object
        
        Java first converts primitive into wrapper object:
        
        Integer obj = 10; // autoboxing
        Therefore
        
        This is invalid:
        
        List<int> list = new ArrayList<>();
        
        because generics expect reference types.
        
        Correct:
        
        List<Integer> list = new ArrayList<>();
        Why didn't Java make generics support primitives separately?
        
        Because generics were added in Java 5 and needed:
        
        backward compatibility
        old JVM support
        
        So Java used:
        
        type erasure
        existing object model
        
        instead of creating completely new runtime generic system.


9. Difference between Generic Class and Generic Method

           Generic Class
           class Box<T> {
           T value;
           }
        
        T belongs to whole class.
        
        Generic Method
        public <T> void print(T value) {
        System.out.println(value);
        }
        
        T belongs only to method.

10. Difference between:

               public <T> void put(T t)
        
        and
        
        public void put(T t)
        First one
        
        Method-level generic.
        
        public <T> void put(T t)
        
        Method introduces its own type parameter.
        
        Can accept any type.
        
        Second one
        
        Uses class-level generic.
        
        class Box<T> {
        public void put(T t)
        }
        
        Method uses class's T.

11. What is a Raw Type?

        Using generic class without type argument.
        List list = new ArrayList();
        Raw types disable generic type checking.

12. Why are Raw Types dangerous?

        Compiler cannot ensure type safety.
    
        List list = new ArrayList();
        list.add("abc");
        list.add(10);
    
        String s = (String) list.get(1); // runtime exception

13. Why does Java still allow Raw Types?

        Backward compatibility.
        Old code must continue working.

14. What is an Unbounded Type Parameter?

        class Box<T>

        T can be any type.

15. What is a Bounded Type Parameter?

        Restricts allowed types.
    
        class Box<T extends Number>
    
        Only subclasses of Number.

16. Can we use multiple bounds?

        Yes.
    
        class Test<T extends Number & Comparable<T>>
    
        Class first, interfaces later.

17. Why class must come first in bounds?

        Java does not support multiple inheritance of classes.
    
        In Java generics, multiple bounds are written like this:
    
        class Test<T extends A & B & C>
    
        Here:
    
        A can be a class
        B, C must be interfaces
    
        But if a class is present, it must come first.
    
        Valid
        class Test<T extends Number & Comparable<T>>
        Number → class
        Comparable → interface
        Invalid
        class Test<T extends Comparable<T> & Number>
    
        Compile error.
    
        Why?
    
        Because Java does NOT support multiple inheritance of classes.
    
        A class can extend only ONE class.
    
        Example:
    
        class A {}
        class B {}
    
        class C extends A, B {} // invalid
    
        Java forbids this.
    
        Now connect this to generics
    
        When you write:
    
        <T extends Number & Comparable<T>>
    
        Java interprets it conceptually as:
    
        class Temp extends Number implements Comparable
    
        which is valid because:
    
        extend one class
        implement many interfaces
    
        But if you write:
    
        <T extends Comparable<T> & Number>
    
        Compiler would conceptually see:
    
        class Temp extends Comparable ??? extends Number ???
    
        This structure becomes ambiguous because:
    
        first bound determines superclass after type erasure
        interfaces are treated differently
        MOST IMPORTANT INTERNAL REASON
    
        After type erasure:
    
        <T extends Number & Comparable<T>>
    
        becomes roughly:
    
        Number
    
        The first bound becomes the erased type.
    
        So compiler needs:
    
        at most one class
        and it must be first
    
        because JVM/class inheritance model allows only one superclass.

18. What are Wildcards?

        Represent unknown type.
    
        List<?>

19. Types of Wildcards

        Unbounded → <?>
        Upper bounded → <? extends T>
        Lower bounded → <? super T>
20. What is Unbounded Wildcard?

                List<?>
    
        Can hold any type list.
    
        Mostly read-only.

21. What is Upper Bounded Wildcard?
        List<? extends Number>

        Accepts:
    
        List<Integer>
        List<Double>
    
        Used for reading

21a. 

    1. <?> → Mostly Read-Only
    List<?> list
    
    You cannot add elements:
    
    list.add("abc"); // error
    list.add(10);    // error
    
    Because compiler does not know actual type.
    
    Could be:
    
    List<String>
    List<Integer>
    
    Only allowed:
    
    list.add(null);
    
    But reading is allowed:
    
    Object obj = list.get(0);
    2. <? extends T> → Read-Only (Producer)
    List<? extends Number> list
    
    You can read safely:
    
    Number n = list.get(0);
    
    But cannot add:
    
    list.add(10); // error
    
    Because compiler doesn't know exact subtype.
    
    Could be:
    
    List<Integer>
    List<Double>
    
    This is why it's considered read-only.
    
    3. <? super T> → NOT Read-Only
    List<? super Integer> list
    
    This is writable.
    
    You CAN add:
    
    list.add(10);
    list.add(new Integer(20));
    
    Because anything above Integer can store Integer safely.
    
    But reading is restricted:
    
    Object obj = list.get(0);
    
    Compiler only guarantees Object.
    
    Easy Memory Trick (PECS)
    Producer Extends
    <? extends T>
    
    Good for reading.
    
    Consumer Super
    <? super T>
    
    Good for writing.


22. Why can't we add elements to List<? extends Number>?

        Compiler doesn't know exact subtype.
        List<? extends Number> list
        
        Could be:
        
        List<Integer>
        List<Double>
        
        Adding Integer may break type safety.
        
        Only null allowed.

23. What is Lower Bounded Wildcard?
    List<? super Integer>

        Accepts:
        
        List<Integer>
        List<Number>
        List<Object>
        
        Used for writing.

24. PECS Principle

        Producer Extends Consumer Super.
    
        Producer → extends
        Consumer → super

25. Explain PECS with example

            Producer
            List<? extends Number>

            Produces numbers for reading.

        Consumer
        List<? super Integer>
    
        Consumes integers for writing.

26. Difference between extends and super

            Feature	    extends	        super
            Used for	Reading	       Writing
            Direction	Upper bound	    Lower bound
            Safe to add	  No	             Yes
            Safe to read	Yes	       Object only

------------------------------------------------------------------------------------------------------------------------------------

PECS = Producer Extends, Consumer Super

This is the rule used to decide:
    
    when to use extends
    when to use super

in Java generics wildcards.

FullForm
Term	Meaning
PE	Producer Extends
CS	Consumer Super
Why "Producer" and "Consumer"?

Think about how the collection is being used.

    1. Producer → Gives/Produces data to you
    
    If you are mainly reading from collection:
    
    Use:
    
    <? extends T>
    
    because collection is producing values.
    
    Example
    List<? extends Number> list
    
    This list produces numbers:
    
    Number n = list.get(0);
    
    Safe.
    
    Why can't we add?
    
    Because compiler doesn't know exact subtype.
    
    Could be:
    
    List<Integer>
    
    or
    
    List<Double>
    
    If Java allowed:
    
    list.add(10);
    
    and actual list was List<Double>,
    type safety breaks.
    
    So extends lists are effectively read-only.

    Producer Meaning
    
    Collection is source of data.
    
    You take values OUT.

2. Consumer → Accepts/Consumes data

        If you are mainly writing into collection:
        
        Use:
        
        <? super T>
        
        because collection consumes values.
        
        Example
        List<? super Integer> list
        
        You can safely add:
        
        list.add(10);
        
        because:
        
        Integer
        Number
        Object
        
        all can hold Integer.
        
        Why reading is limited?
        
        When retrieving:
        
        Object obj = list.get(0);
        
        Compiler only guarantees Object.
        
        Because actual type may be:
        
        List<Integer>
        List<Number>
        List<Object>
        Consumer Meaning

Collection is destination of data.

    You put values IN.
    
    Visual Understanding
    Producer (extends)
    List<? extends Number>
    Collection  --->  You
       gives data
    
    You READ from it.
    
    Consumer (super)
    List<? super Integer>
    You  --->  Collection
        put data
    
    You WRITE into it.
    
    Real Interview Example
    Copy Method
    <T> void copy(List<? super T> dest,
                  List<? extends T> src)
    Why src uses extends?
    
    Because source produces values.
    
    We read from source.
    
    Why dest uses super?
    
    Because destination consumes values.
    
    We write into destination.
    
    Example
    List<Integer> src = List.of(1,2,3);
    List<Number> dest = new ArrayList<>();
    
    copy(dest, src);
    
    Safe because:
    
    source produces Integers
    destination can consume Integers



```
import java.util.*;

public class Main {

    public static <T> void copy(
            List<? super T> dest,
            List<? extends T> src) {

        for (T item : src) {
            dest.add(item);
        }
    }

    public static void main(String[] args) {

        List<Integer> source = Arrays.asList(1, 2, 3);

        List<Number> destination = new ArrayList<>();

        copy(destination, source);

        System.out.println(destination);
    }
}

```






---------------------------------------------------------------------------------------------------------------------------

Variance

Variance describes:

    how inheritance behaves between generic types.

Suppose:

    Integer extends Number

Question:
Does this imply:

    List<Integer> extends List<Number> ?

Java says:

    NO.

This is called invariant.

1. Invariant
   Definition
        
        If:
        
        A extends B
        
        but:
        
        Container<A>
        
        is NOT subtype of:
        
        Container<B>
        
        then it is invariant.
        
        Java Generics are Invariant
        Integer extends Number
        
        BUT:
        
        List<Integer> != List<Number>
        Why?
        
        To prevent type safety problems.
        
        Dangerous Situation
        
        Suppose Java allowed:
        
        List<Number> nums = new ArrayList<Integer>();
        
        Then this would compile:
        
        nums.add(3.14);
        
        because 3.14 is a Number.
        
        But actual list is:
        
        ArrayList<Integer>
        
        Now Double entered Integer list.
        
        Type safety destroyed.
        
        Runtime Disaster
        
        Later:
        
        Integer x = nums.get(0);
        
        Could retrieve Double.
        
        Boom:
        ClassCastException
        
        Therefore Java forbids this
        List<Integer> -> List<Number>
        
        This is invariance.

   2. Covariant
      Definition
        
           If:
        
           A extends B
        
           AND:
        
           Container<A> extends Container<B>
        
           then it is covariant.
        
           Arrays are Covariant in Java
           Integer[] ints = new Integer[5];
        
           Number[] nums = ints;
        
           Allowed.
        
           But arrays become unsafe
           nums[0] = 3.14;
        
           Runtime error:
        
           ArrayStoreException
        
           because actual array is Integer[].






-------------------------------------------------------------------------------------------------------------------------------------

27. Why is Generic Type Invariant?

    List<Integer> != List<Number>

Otherwise:

List<Number> nums = new ArrayList<Integer>();
nums.add(3.14);

Would break type safety.

28. Difference between Arrays and Generics

Arrays are covariant:

Number[] arr = new Integer[5];

Generics are invariant:

List<Number> list = new ArrayList<Integer>(); // invalid
29. Why are Arrays covariant?

For backward compatibility.

But they are runtime type-safe.(JVM throws error)

30. Why are Generics invariant?

To ensure compile-time type safety.

------------------------------------------------------------------------------------------------------------------------------

Heap Pollution

Heap pollution happens when:

    a variable of a parameterized type refers to an object that is not of that parameterized type.

In simpler words:
    
    Compiler thinks one type exists,
    but actual object contains another type.

Usually caused by:
    
    raw types
    unsafe casts
    varargs with generics
    Classic Example
    List<String> list = new ArrayList<>();

Compiler believes:

    this list contains only Strings.

Now use raw type:

    List raw = list;

Raw type disables generic checks.

    Now:raw.add(10);

Compiler allows it.

    But actual list becomes:

    ["hello", 10]
    inside a List<String>.
    
    This is heap pollution.

Runtime Failure

Later:

    String s = list.get(0);
    Compiler internally inserts:
    
    String s = (String) list.get(0);
    
    If retrieved element is Integer:
    
    ClassCastException

Full Example
    
    import java.util.*;
    
    public class Main {
    public static void main(String[] args) {
    
            List<String> names = new ArrayList<>();
    
            names.add("Java");
    
            List raw = names;
    
            raw.add(100);
    
            for (String s : names) {
                System.out.println(s);
            }
        }
    }

Output:
    
    Java
    ClassCastException
Why called "Heap" Pollution?

    Objects are stored in heap memory.
    Heap now contains wrong type object inside supposedly type-safe structure.
    
    So:
    
    heap got polluted with invalid type.
    
    Why Generics normally prevent this?
    
    Compiler checks types:
    
    List<String> list = new ArrayList<>();
    list.add(10); // compile error
    
    But raw types bypass compiler safety.

Main Causes of Heap Pollution
Cause	            Example
Raw types	        List raw
Unsafe casts	 (List<String>) obj
Generic varargs	        List<String>...
Mixing arrays + generics	Generic arrays
Another Example (Unsafe Cast)
Object obj = new ArrayList<Integer>();

    List<String> list = (List<String>) obj;
    
    Compiler warning only.
    
    Now:
    
    list.add("Hello");
    
    Actual object is ArrayList<Integer>.
    
    Heap polluted.

Why Compiler Gives Warning Instead of Error?

    Because Java supports backward compatibility and type erasure.
    Some unsafe operations cannot be fully verified at compile time.
    
    So compiler warns:
    
    "Unchecked operation"

Generic Varargs Heap Pollution
    
    static void test(List<String>... lists)
    
    Internally varargs become array:
    
    List<String>[]
    
    But arrays are covariant + reifiable.
    
    This can create heap pollution.
    
    That’s why compiler gives warning.





----------------------------------------------------------------------------------------------------------------------------------------

31. What is Heap Pollution?

        When variable of parameterized type refers to wrong type object.
        Usually caused by raw types.

32. Example of Heap Pollution
    
                List<String> list = new ArrayList();
    
        List raw = list;
        raw.add(10);

String s = list.get(0); // runtime exception
33. What is Reifiable Type?

        Type fully available at runtime.

Examples:
    
    String
    Integer[]
    Raw types

34. What is Non-Reifiable Type?

        Type info removed at runtime.
        
        List<String>
35. Why can't we create Generic Arrays?
    new T[10] // invalid

Because generic type erased at runtime.

Could break type safety.

36. Why is this illegal?
    List<String>[] arr = new List<String>[10];

Could cause heap pollution.

37. Can Generic Class extend Throwable?

No.

class MyException<T> extends Exception // invalid

Because JVM exception handling requires exact runtime type.

38. Can we create static generic fields?

No.

class Box<T> {
static T value; // invalid
}

Static belongs to class, not object.

39. Can static methods use class-level generic type?

No.

class Box<T> {
static void print(T t) // invalid
}

Because static context exists before object creation.

40. How can static methods use generics then?

Method-level generic.

static <T> void print(T t)
41. Can we instantiate type parameter?
    new T() // invalid

Type erased at runtime.

42. How to instantiate generic type then?

        Using reflection:

        clazz.getDeclaredConstructor().newInstance();
43. Can we use instanceof with generics?

No.

if(obj instanceof List<String>) // invalid

Because generic info erased.

Allowed:

obj instanceof List
44. Can overloaded methods differ only by generic type?

No.

void print(List<String>)
void print(List<Integer>)

Both erase to same signature.

45. What is Bridge Method?

Compiler-generated method to preserve polymorphism after type erasure.

46. Bridge Method Example
    class Parent<T> {
    T get()
    }
    class Child extends Parent<String> {
    String get()
    }

Compiler creates bridge method internally.

47. What is Generic Constructor?

Constructor with own type parameter.

class Test {
<T> Test(T t) {
}
}
48. What is Diamond Operator?
    List<String> list = new ArrayList<>();

Compiler infers type.

Introduced in Java 7.

49. What is Type Inference?

Compiler automatically determines generic type.

50. Explain Generic Interface
    interface Repository<T> {
    void save(T t);
    }
51. Implementing Generic Interface
    class UserRepo implements Repository<User>
52. Generic Method Type Inference
    <T> T get(T value)

Compiler infers type from arguments.

53. What is Recursive Type Bound?
    class Test<T extends Comparable<T>>

Used heavily in collections.

54. Why Comparable uses recursive bound?

Ensures object compares with same type.

55. What is <?> actually?

Unknown type.

Not same as Object.

56. Difference between List<Object> and List<?>
    List<Object>

Can add any object.

List<?>

Cannot add elements except null.

Read-only behavior.

57. Why is List<String> not subtype of List<Object>?

Generics are invariant.

58. Can generic types be used in enums?

Enums themselves cannot be generic.

enum Test<T> // invalid
59. Can interfaces be generic?

Yes.

interface MyInterface<T>
60. Can methods inside non-generic class be generic?

Yes.

class Test {
<T> void print(T t)
}
61. What happens internally for:
    List<String> list = new ArrayList<>();

After erasure:

List list = new ArrayList();

Compiler inserts casts automatically.

62. How compiler ensures type safety?

Compiler tracks generic type during compilation and inserts casts.

Example:

String s = list.get(0);

Compiler internally generates:

String s = (String) list.get(0);
63. Why runtime ClassCastException can still happen?

Because raw types bypass compile-time checks.

64. Why does this fail?
    class C<U> extends B<T>

Because T not declared.

Compiler knows only U.

Correct:

class C<T> extends B<T>

or

class C<U> extends B<U>
65. Why does this fail?
    class C extends B<T>

T undefined.

Raw type would be:

class C extends B
66. Difference between parseInt, valueOf, and intValue
    parseInt

Returns primitive.

int x = Integer.parseInt("10");
valueOf

Returns Integer object.

Integer x = Integer.valueOf("10");

Uses Integer cache.

intValue

Converts wrapper to primitive.

Integer x = 10;
int y = x.intValue();
67. What is Integer Cache?

Java caches Integer objects from:

-128 to 127

to improve performance and memory.

68. Why only certain Integer values cached?

Small integers are most frequently used.

Caching huge range wastes memory.

69. Difference between:
    new Integer(200)

and

Integer.valueOf(200)

new always creates new object.

valueOf may reuse cached object.

70. Is this true?
    Integer a = Integer.valueOf("200");
    int b = a.intValue();

a == b

Yes.

Because wrapper unboxed to primitive.

Primitive comparison occurs.

71. Can generic methods be overloaded?

Yes, if erased signatures differ.

72. Why generic exceptions not allowed?

Runtime type info unavailable after erasure.

73. What is capture of wildcard?

Compiler creates temporary hidden type for wildcard.

74. Why helper methods used with wildcards?

To capture wildcard type safely.

75. Explain Collections.copy
    <T> void copy(List<? super T> dest,
    List<? extends T> src)

Source produces → extends

Destination consumes → super

Perfect PECS example.

76. What is F-bounded polymorphism?

Recursive bounds:

<T extends Comparable<T>>
77. Why can't we use primitives in generics?

Generics implemented using type erasure and work only with objects.

78. What are common real-world uses of Generics?
    Collections
    Spring repositories
    Optional<T>
    CompletableFuture<T>
    Streams
    ResponseEntity<T>
79. How are generics used in Spring?
    JpaRepository<User, Long>

Reusable repository logic.

80. Most Important Interview One-Liners
    Why generics?

Compile-time type safety.

Why type erasure?

Backward compatibility.

Why no generic arrays?

Runtime type mismatch risk.

Why no primitives?

Generics work only with objects.

Why invariant?

Prevent unsafe insertion.

Why extends?

Reading/producer.

Why super?

Writing/consumer.

Why raw types dangerous?

Bypass compiler checks.

Why bridge methods?

Preserve polymorphism after erasure.