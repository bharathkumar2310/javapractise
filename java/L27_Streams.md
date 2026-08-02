STREAMS :

    Stream is a pipeline used to process data from a source (like a collection) through a sequence of operations (like filter, map, reduce) in a functional style without modifying the source. 
    It allows you to perform complex data processing tasks in a concise and readable way.
    
    In Streams pipeline means a chain of operations through which data flows. 
    Each operation transforms the data and passes it to the next operation in the chain.


Streams in Java (programming language) (introduced in Java SE 8) were created to make data processing easier, shorter, and more powerful when working with collections like List, Set, or Map.

![img.png](../Images/Streams1.png)

Uses :

| Benefit             | Explanation           |
| ------------------- | --------------------- |
| Less code           | No loops needed       |
| Readable            | Express logic clearly |
| Functional style    | Uses lambdas          |
| Parallel processing | Easy multi-threading  |
| Pipeline processing | Efficient data flow   |

      Streams in Java (programming language) do not store data themselves.
      They process data from another source like a List, Set, or array.
      
      So the idea is:
      
      Streams do not hold elements in memory like collections do.
      They take elements from a source and process them one by one.


1️⃣ Main Purpose of Streams

        Streams allow you to process collections of data in a declarative and functional way.
        Instead of writing how to do the iteration, you just say what you want to do.

2️⃣ Problem Before Streams

        Before streams, we used loops.
        Example: Print even numbers.

        List<Integer> list = List.of(1,2,3,4,5,6);

        for(Integer n : list){
            if(n % 2 == 0){
                System.out.println(n);
            }
        }

Problems:

        More code
        Harder to read
        Harder to combine operations

3️⃣ With Streams

        list.stream()
        .filter(n -> n % 2 == 0)
        .forEach(System.out::println);

Much shorter and cleaner.





4️⃣ Major Uses of Streams
1. Filtering Data

Selecting specific elements.

Example: Get even numbers.

    list.stream()
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);

Used in:

    searching
    validation
    filtering records

2. Transforming Data (Mapping)

Example: Multiply numbers by 2.

    list.stream()
    .map(n -> n * 2)
    .forEach(System.out::println);

Used in:

        converting data
        formatting values
        DTO mapping

3. Aggregation (Calculations)

Example: Sum of numbers.

    int sum = list.stream()
    .reduce(0, Integer::sum);

Used for:

        totals
        averages
        statistics

4. Collecting Results

Convert stream results into another collection.

        List<Integer> even = list.stream()
        .filter(n -> n % 2 == 0)
        .toList();

Used for:

    building new collections
    transforming data sets

5. Sorting Data

       list.stream()
       .sorted()
       .forEach(System.out::println);

Used in:

        ordering records
        ranking results

6. Parallel Processing

        Streams allow easy parallel execution.
        
            list.parallelStream()
            .forEach(System.out::println);
        
        Uses multiple CPU cores.


**_Stream Pipeline Structure:**_

A Stream Pipeline has 3 parts:

    Source → Intermediate Operations → Terminal Operation
1️⃣ Source

    Where the data comes from.

Examples

        List
        Set
        Array
        Files

List<Integer> list = List.of(1,2,3,4,5);
2️⃣ Intermediate Operations

    These transform the stream and return another stream.

Examples:

        filter()
        map()
        sorted()
        distinct()

Example

        list.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * 2)

Important:

    They do not execute immediately because they are lazy.
    They build the pipeline

3️⃣ Terminal Operation

    This triggers execution of the pipeline.

Examples:

    forEach()
    collect()
    count()
    reduce()

Example

    list.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * 2)
    .forEach(System.out::println);

![img_1.png](../Images/Stream2.png)

How Pipeline Actually Works Internally

Streams process one element at a time.

Example:

      list.stream()
      .filter(n -> n % 2 == 0)
      .map(n -> n * 2)
      .forEach(System.out::println);

For list [1,2,3,4]

Actual flow:

      1 → filter → rejected
      2 → filter → map → print
      3 → filter → rejected
      4 → filter → map → print

Not like this:

      filter all → map all → print all

This makes Streams very memory efficient.

![img_2.png](../Images/Stream3.png)


![img.png](../Images/StreamCreation1.png)

![img_1.png](../Images/StreamCreation2.png)



1. Primitive Streams

Normally, a stream stores objects.

      Stream<Integer>
      Stream<Long>
      Stream<Double>

But Java has primitive types:

      int
      long
      double

If we use Stream<Integer>, every int is boxed into an Integer.

      5  → Integer(5)
      6  → Integer(6)

This boxing/unboxing has some overhead.

To avoid that, Java provides specialized primitive streams:

      IntStream
      LongStream
      DoubleStream

These work directly with primitives.

Example

Instead of

      Stream<Integer> stream = Stream.of(1, 2, 3);

we can write

      IntStream stream = IntStream.of(1, 2, 3);
      
      No boxing happens.

range()
      
      IntStream.range(1, 5)
      .forEach(System.out::println);

Output

      1
      2
      3
      4

Notice:

Ending value is excluded.

Think of it like a for loop:

      for(int i = 1; i < 5; i++)
rangeClosed()
      
      IntStream.rangeClosed(1, 5)
      .forEach(System.out::println);

Output

      1
      2
      3
      4
      5

Equivalent to

      for(int i = 1; i <= 5; i++)
      Sum
      
      Instead of
      
      int sum = 0;
      
      for(int i = 1; i <= 100; i++)
      sum += i;
      
      Use
      
      int sum = IntStream.rangeClosed(1, 100)
      .sum();
      Average
      OptionalDouble avg =
      IntStream.of(10, 20, 30)
      .average();
      Max
      OptionalInt max =
      IntStream.of(3, 8, 2, 10)
      .max();
      Convert to Stream<Integer>
      
      Sometimes an API expects Stream<Integer>.

         Stream<Integer> stream =
         IntStream.range(1, 5)
         .boxed();

boxed() converts

      int
      
      to
      
      Integer
2. Streams from String

There are two common ways.

(A) Character Stream
String s = "JAVA";

      IntStream stream = s.chars();

Why IntStream?

      Because Java stores characters internally as Unicode code points.

Output

      74
      65
      86
      65

These are ASCII/Unicode values.

Convert back to characters
      
      s.chars()
      .mapToObj(c -> (char) c)
      .forEach(System.out::println);

Output

      J
      A
      V
      A

Count vowels
      
      long count =
      s.chars()
      .filter(c ->
      "AEIOUaeiou".indexOf(c) != -1)
      .count();

(B) Word Stream

Suppose

      String sentence =
      "Java Spring Boot";

Split first.

      Stream<String> stream =
      Arrays.stream(sentence.split(" "));
      
      Now stream contains
      
      Java
      Spring
      Boot
      
      Example
      
      Arrays.stream(sentence.split(" "))
      .forEach(System.out::println);

Output

      Java
      Spring
      Boot

Convert words to uppercase
      
      Arrays.stream(sentence.split(" "))
      .map(String::toUpperCase)
      .forEach(System.out::println);

Output

      JAVA
      SPRING
      BOOT

Interview examples
      
      Sum of even numbers
      int sum =
      IntStream.rangeClosed(1, 10)
      .filter(x -> x % 2 == 0)
      .sum();
      Count digits in a string
      long count =
      "abc123xyz".chars()
      .filter(Character::isDigit)
      .count();
      Count words
      long words =
      Arrays.stream(sentence.split(" "))
      .count();
      Interview summary
      Primitive Stream	Purpose
      IntStream	Stream of int values (avoids boxing)
      LongStream	Stream of long values
      DoubleStream	Stream of double values
      range()	Start inclusive, end exclusive
      rangeClosed()	Both ends inclusive
      boxed()	Converts primitive stream to object stream
      String Stream	Purpose
      chars()	Produces an IntStream of character Unicode values
      Arrays.stream(str.split(" "))	Produces a Stream<String> of words

1️⃣ What Happens When You Call Intermediate Operations

Example:

      list.stream()
      .filter(n -> n % 2 == 0)
      .map(n -> n * 2)
      .forEach(System.out::println);

Steps internally:

   Step 1 — Create Stream
   list.stream()

A Stream object is created with:
      
      Source = list
      Pipeline = empty

Step 2 — Add filter()

   .filter(n -> n % 2 == 0)
      
      The stream does not execute filtering.
      Instead it adds a stage to the pipeline.

Conceptually:

      Source = list
      Pipeline = [filter]

Step 3 — Add map()

   .map(n -> n * 2)

Again, nothing executes.
Pipeline becomes:
      
      Source = list
      Pipeline = [filter → map]

Each call returns another Stream object representing the next stage.

2️⃣ When Terminal Operation Happens
.forEach(System.out::println)

Now the stream says:
      
      Pipeline is complete
      Start processing

      Execution begins.

3️⃣ How Elements Flow Through Pipeline

List:

[1,2,3,4]

Actual execution:

      1 → filter → rejected
      2 → filter → map → print
      3 → filter → rejected
      4 → filter → map → print

So operations are applied element by element.

4️⃣ How Stream Remembers Operations

Internally the stream builds something like:

      StreamStage
      ↓
      StreamStage
      ↓
      StreamStage

Each stage stores:

operation type (filter, map, etc.)

lambda function

Conceptually:
      
      Stage1 : filter(n -> n % 2 == 0)
      Stage2 : map(n -> n * 2)
      Stage3 : forEach(System.out::println)

5️⃣ Real Internal Classes (High Level)

Java internally uses classes like:

      AbstractPipeline
      ReferencePipeline
      StatelessOp
      TerminalOp

Each intermediate operation creates a new pipeline stage object that holds:

      previousStage + currentOperation
      So the stream remembers the whole chain.





| Operation                 | Functional Interface       | Method Signature                                                       | Used For                             | Example                                      | Stateful / Stateless            |
| ------------------------- | -------------------------- | ---------------------------------------------------------------------- |--------------------------------------| -------------------------------------------- | ------------------------------- |
| **filter()**              | `Predicate<T>`             | `Stream<T> filter(Predicate<? super T> predicate)`                     | Select elements matching a condition | `stream.filter(x -> x > 10)`                 | Stateless                       |
| **map()**                 | `Function<T,R>`            | `Stream<R> map(Function<? super T,? extends R> mapper)`                | Transform elements                   | `stream.map(x -> x * 2)`                     | Stateless                       |
| **mapToInt()**            | `ToIntFunction<T>`         | `IntStream mapToInt(ToIntFunction<? super T>)`                         | Convert to `IntStream`               | `stream.mapToInt(x -> x.getAge())`           | Stateless                       |
| **mapToLong()**           | `ToLongFunction<T>`        | `LongStream mapToLong(ToLongFunction<? super T>)`                      | Convert to `LongStream`              | `stream.mapToLong(x -> x.getSalary())`       | Stateless                       |
| **mapToDouble()**         | `ToDoubleFunction<T>`      | `DoubleStream mapToDouble(ToDoubleFunction<? super T>)`                | Convert to `DoubleStream`            | `stream.mapToDouble(x -> x.getPrice())`      | Stateless                       |
| **flatMap()**             | `Function<T,Stream<R>>`    | `Stream<R> flatMap(Function<? super T,? extends Stream<? extends R>>)` | Flatten nested streams               | `list.stream().flatMap(x -> x.stream())`     | Stateless                       |
| **flatMapToInt()**        | `Function<T, IntStream>`   | `IntStream flatMapToInt(...)`                                          | Flatten to `IntStream`               | `stream.flatMapToInt(x -> Arrays.stream(x))` | Stateless                       |
| **distinct()**            | —                          | `Stream<T> distinct()`                                                 | Remove duplicate elements            | `stream.distinct()`                          | **Stateful**                    |
| **sorted()**              | `Comparator<T>` (optional) | `Stream<T> sorted()` / `sorted(Comparator)`                            | Sort elements                        | `stream.sorted()`                            | **Stateful**                    |
| **peek()**                | `Consumer<T>`              | `Stream<T> peek(Consumer<? super T>)`                                  | Debug / inspect elements             | `stream.peek(System.out::println)`           | Stateless                       |
| **limit()**               | —                          | `Stream<T> limit(long maxSize)`                                        | Restrict number of elements          | `stream.limit(5)`                            | **Stateful (short-circuiting)** |
| **skip()**                | —                          | `Stream<T> skip(long n)`                                               | Skip first n elements                | `stream.skip(3)`                             | **Stateful**                    |
| **takeWhile()** (Java 9+) | `Predicate<T>`             | `Stream<T> takeWhile(Predicate)`                                       | Take elements while condition true   | `stream.takeWhile(x -> x < 10)`              | **Stateful**                    |
| **dropWhile()** (Java 9+) | `Predicate<T>`             | `Stream<T> dropWhile(Predicate)`                                       | Skip while condition true            | `stream.dropWhile(x -> x < 10)`              | **Stateful**                    |
| **unordered()**           | —                          | `BaseStream unordered()`                                               | Remove ordering constraint           | `stream.unordered()`                         | Stateless                       |
| **parallel()**            | —                          | `BaseStream parallel()`                                                | Convert stream to parallel           | `stream.parallel()`                          | Stateless                       |
| **sequential()**          | —                          | `BaseStream sequential()`                                              | Convert to sequential stream         | `stream.sequential()`                        | Stateless                       |


Stateless Operations

Process one element at a time

      filter
      map
      flatMap
      peek
      mapToInt / mapToLong / mapToDouble

They do not store previous elements.


Stateful Operations

Need previous elements or buffering

      sorted
      distinct
      limit
      skip
      takeWhile
      dropWhile

They may store elements internally before continuing the pipeline.


![img.png](../Images/IntermediateStream1.png)

![img_1.png](../Images/IntermediateStream2.png)

![img_2.png](../Images/IntermediateStream3.png)

![img_3.png](../Images/IntermediateStream4.png)

![img_4.png](../Images/IntermediateStream5.png)

![img_5.png](../Images/IntermediateStream6.png)

![img_6.png](../Images/IntermediateStream7.png)

![img_7.png](../Images/IntermediateStream8.png)


TERMINAL OPERATIONS :

      Terminal operations are the operations that trigger execution of the stream pipeline and produce a final result or side effect.
      After a terminal operation runs, the stream cannot be reused.


| Operation            | Functional Interface               | Method Signature (Simplified)                              | Used For                                                            | Example                                      | Return Type           |
| -------------------- | ---------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------- | -------------------------------------------- | --------------------- |
| **forEach()**        | `Consumer<T>`                      | `void forEach(Consumer<? super T> action)`                 | Perform action on each element                                      | `stream.forEach(System.out::println)`        | `void`                |
| **forEachOrdered()** | `Consumer<T>`                      | `void forEachOrdered(Consumer<? super T>)`                 | Process elements in encounter order (important in parallel streams) | `stream.forEachOrdered(System.out::println)` | `void`                |
| **toArray()**        | `IntFunction<A[]>`                 | `Object[] toArray()` / `<A> A[] toArray(IntFunction<A[]>)` | Convert stream to array                                             | `stream.toArray()`                           | `Object[]`            |
| **reduce()**         | `BinaryOperator<T>` / `BiFunction` | `Optional<T> reduce(BinaryOperator<T>)`                    | Combine elements into one value                                     | `stream.reduce((a,b) -> a+b)`                | `Optional<T>`         |
| **collect()**        | `Collector<T,A,R>`                 | `<R,A> R collect(Collector)`                               | Convert stream to collection or custom result                       | `stream.collect(Collectors.toList())`        | `Collection / Object` |
| **min()**            | `Comparator<T>`                    | `Optional<T> min(Comparator)`                              | Find smallest element                                               | `stream.min(Integer::compare)`               | `Optional<T>`         |
| **max()**            | `Comparator<T>`                    | `Optional<T> max(Comparator)`                              | Find largest element                                                | `stream.max(Integer::compare)`               | `Optional<T>`         |
| **count()**          | —                                  | `long count()`                                             | Count number of elements                                            | `stream.count()`                             | `long`                |
| **anyMatch()**       | `Predicate<T>`                     | `boolean anyMatch(Predicate)`                              | Check if any element matches condition                              | `stream.anyMatch(x -> x > 10)`               | `boolean`             |
| **allMatch()**       | `Predicate<T>`                     | `boolean allMatch(Predicate)`                              | Check if all elements match condition                               | `stream.allMatch(x -> x > 10)`               | `boolean`             |
| **noneMatch()**      | `Predicate<T>`                     | `boolean noneMatch(Predicate)`                             | Check if no elements match condition                                | `stream.noneMatch(x -> x > 10)`              | `boolean`             |
| **findFirst()**      | —                                  | `Optional<T> findFirst()`                                  | Get first element                                                   | `stream.findFirst()`                         | `Optional<T>`         |
| **findAny()**        | —                                  | `Optional<T> findAny()`                                    | Get any element (good for parallel streams)                         | `stream.findAny()`                           | `Optional<T>`         |
| **iterator()**       | —                                  | `Iterator<T> iterator()`                                   | Convert stream to iterator                                          | `stream.iterator()`                          | `Iterator<T>`         |
| **spliterator()**    | —                                  | `Spliterator<T> spliterator()`                             | Create spliterator for parallel processing                          | `stream.spliterator()`                       | `Spliterator<T>`      |


![img.png](../Images/TerminalOp1.png)

![img_1.png](../Images/TerminalOp2.png)

![img_2.png](../Images/TerminalOp3.png)

![img_3.png](../Images/TerminalOp4.png)

![img_4.png](../Images/TerminalOp5.png)


![img_5.png](../Images/TerminalOp6.png)



PARALLEL STREAMS :

      Parallel streams allow you to process data in parallel using multiple CPU cores.
      They can significantly improve performance for large data sets and computationally intensive operations.
      However, they may not always be faster than sequential streams, especially for small data sets or simple operations.
      Parallel Streams allow a stream to process elements using multiple threads at the same time instead of a single thread.
      Normally, streams are sequential (one element processed after another).
      Parallel streams split the data and process parts simultaneously using multiple CPU cores.



1️⃣ Who Splits the Data?

      Splitting is done by java.util.Spliterator.

Spliterator has a method:

      trySplit()

This method divides the data source into smaller parts.

Example idea:

      [1,2,3,4,5,6,7,8]
      |
      trySplit()

Result:

[1,2,3,4]   [5,6,7,8]
2️⃣ What Does ForkJoinPool Do?

After splitting:

   ForkJoinPool assigns chunks to worker threads
   Threads execute the operations in parallel.

So roles are:


| Component       | Responsibility                             |
| --------------- | ------------------------------------------ |
| `Spliterator`   | Splits the data                            |
| `ForkJoinPool`  | Executes tasks in parallel                 |
| Stream pipeline | Applies operations (`map`, `filter`, etc.) |


![img.png](../Images/ParallelStream1.png)

![img_1.png](../Images/ParallelStream2.png)



1️⃣ Convert Stream → List

      List<Integer> list =
      stream.collect(Collectors.toList());
Result type

      List<T>
2️⃣ Convert Stream → Set

      Set<Integer> set =
      stream.collect(Collectors.toSet());
Result

      Set<T>
3️⃣ Convert Stream → Map

      Map<Integer, String> map =
      stream.collect(
         Collectors.toMap(
            x -> x,
            x -> "Value " + x
         )
      );

Example

      Stream: 1,2,3
      Map: {1=Value 1, 2=Value 2, 3=Value 3}

Here:

      Lambda	Meaning
      x -> x	key
      x -> "Value"+x	value
4️⃣ Convert Stream → String (joining)

      String result =
      stream.collect(Collectors.joining(","));

Example

      Stream: ["A","B","C"]

      Result: "A,B,C"
5️⃣ Group elements

      Map<Integer, List<String>> map =
      names.stream()
      .collect(Collectors.groupingBy(String::length));

Example

      ["Java","Go","Python"]

Result

      {
      2=[Go],
      4=[Java],
      6=[Python]
      }
6️⃣ Count elements

      long count =
      stream.collect(Collectors.counting());
7️⃣ Sum values

      int sum =
      numbers.stream()
      .collect(Collectors.summingInt(x -> x));


| Collector      | Result          |
| -------------- | --------------- |
| `toList()`     | List            |
| `toSet()`      | Set             |
| `toMap()`      | Map             |
| `joining()`    | String          |
| `groupingBy()` | Map with groups |
| `counting()`   | Count           |
| `summingInt()` | Sum             |

```java
public static <T> Collector<T, ?, List<T>> toList() {
return Collector.of(
   ArrayList::new,                 // supplier
   List::add,                      // accumulator
   (left, right) -> {              // combiner
        left.addAll(right);
         return left;
   }
);}
```

Accumulator uses the functional interface BiConsumer<A,T> from java.util.function.BiConsumer.
Combiner uses the functional interface BinaryOperator<A> from java.util.function.BinaryOperator.
Combiner is used in parallel processing to merge partial results from different threads.
collect() method internally uses supplier to create a new collection, accumulator to add elements, and combiner to merge results in parallel processing.


🔹 Method Variants (IMPORTANT)
1️⃣ Without identity
Optional<T> reduce(BinaryOperator<T> accumulator)
Example
List<Integer> list = Arrays.asList(1, 2, 3, 4);

Optional<Integer> sum = list.stream()
.reduce((a, b) -> a + b);

System.out.println(sum.get()); // 10
2️⃣ With identity (MOST USED)
T reduce(T identity, BinaryOperator<T> accumulator)
Example
int sum = list.stream()
.reduce(0, (a, b) -> a + b);

System.out.println(sum); // 10

👉 0 is the starting value (identity)

3️⃣ With identity + combiner (Parallel streams)
U reduce(U identity,
BiFunction<U, ? super T, U> accumulator,
BinaryOperator<U> combiner)

Used mainly in parallel streams

🔹 Step-by-step intuition

Example:

list = [1, 2, 3, 4]
.reduce(0, (a, b) -> a + b)


Can Lambda access local variables?
Yes, but they must be:
👉 final or effectively final

What is "effectively final"?
Variable not declared final but never modified.

int x = 10;
x = 20; ❌ not allowed in lambda


What is short-circuiting?
Stops processing early
Example:

findFirst()
anyMatch()



1. Primitive Streams in Java

Instead of Stream<int>, Java gives:

Primitive	Stream Type
int	IntStream
long	LongStream
double	DoubleStream
🔹 2. Why not Stream<int>?

Java generics work only with objects, not primitives.

So this is ❌ invalid:

Stream<int> stream; // Not allowed
🔹 3. How to use primitive streams?
✅ Example
IntStream stream = IntStream.of(1, 2, 3, 4);
✅ From array
int[] arr = {1, 2, 3};

IntStream stream = Arrays.stream(arr);
✅ Range
IntStream.range(1, 5);      // 1,2,3,4
IntStream.rangeClosed(1,5); // 1,2,3,4,5
🔹 4. Why primitive streams are important?

👉 Avoid boxing/unboxing overhead

Without primitive stream ❌
Stream<Integer> stream = list.stream();
int → Integer (boxing)
Slower
With primitive stream ✅
IntStream stream = list.stream().mapToInt(Integer::intValue);
Faster
Memory efficient
🔹 5. Special methods in primitive streams

Primitive streams have extra useful methods:

IntStream.of(1,2,3).sum();
IntStream.of(1,2,3).average();
IntStream.of(1,2,3).max();

These are not available directly in Stream<Integer>

🔹 6. Conversions
Object → Primitive
list.stream().mapToInt(Integer::intValue);
Primitive → Object
IntStream.of(1,2,3).boxed();
🔥 Interview Trap Question

👉 Q: Which is better?

list.stream().map(x -> x * 2)

vs

list.stream().mapToInt(x -> x * 2)

✅ Answer:

mapToInt() is better when working with numbers


❌ Stream.range() → not available
✅ IntStream.range() → available


| Feature              | `partitioningBy`                  | `groupingBy`                            |
| -------------------- | --------------------------------- | --------------------------------------- |
| **Purpose**          | Split data into 2 groups          | Group data into multiple categories     |
| **Condition type**   | Boolean (`true / false`)          | Any key (Integer, String, Object, etc.) |
| **Return type**      | `Map<Boolean, List<T>>`           | `Map<K, List<T>>`                       |
| **Number of groups** | Always **2**                      | **0 to many**                           |
| **Key values**       | Only `true` and `false`           | Anything (length, name, id, etc.)       |
| **Use case**         | Binary classification             | General grouping                        |
| **Empty groups**     | Always present (`true` & `false`) | Only created if data exists             |
| **Flexibility**      | Limited                           | Very flexible                           |
| **Performance**      | Slightly optimized for 2 groups   | General-purpose                         |


1. reduce()

Think of it as repeatedly combining elements until only one result remains.

Example:

1 2 3 4

Reduce with addition:

1 + 2 = 3
3 + 3 = 6
6 + 4 = 10

Final answer:

10
Syntax 1
Optional<T> reduce(BinaryOperator<T> accumulator)

Example:

Optional<Integer> sum =
Stream.of(1, 2, 3, 4)
.reduce((a, b) -> a + b);

System.out.println(sum.get());
Execution
a=1 b=2 → 3

a=3 b=3 → 6

a=6 b=4 → 10
Syntax 2 (Identity)
T reduce(T identity,
BinaryOperator<T> accumulator)

Example:

int sum =
Stream.of(1, 2, 3, 4)
.reduce(0, (a, b) -> a + b);

Execution:

0 + 1 = 1
1 + 2 = 3
3 + 3 = 6
6 + 4 = 10

Output:

10

The identity is the initial value.

Product
int product =
Stream.of(2, 3, 4)
.reduce(1, (a, b) -> a * b);

Execution:

1 * 2 = 2
2 * 3 = 6
6 * 4 = 24



1. To a Collection ⭐⭐⭐⭐⭐
   Collectors.toList()
   Collectors.toSet()
   Collectors.toCollection(ArrayList::new)

Example:

List<String> list =
names.stream()
.collect(Collectors.toList());

Think:

Collect into a collection.

2. To a Map ⭐⭐⭐⭐⭐
   Collectors.toMap()

Example:

Map<Integer, String> map =
employees.stream()
.collect(Collectors.toMap(
Employee::getId,
Employee::getName
));

Think:

Convert objects into key-value pairs.

3. Grouping ⭐⭐⭐⭐⭐
   Collectors.groupingBy()

Example:

Map<String, List<Employee>> map =
employees.stream()
.collect(
Collectors.groupingBy(
Employee::getDepartment
));

Result:

IT
Ram
John

HR
Alice

Think:

Put similar things into buckets.

4. Partitioning ⭐⭐⭐⭐

Only 2 groups.

Collectors.partitioningBy()

Example:

Map<Boolean, List<Integer>> map =
list.stream()
.collect(
Collectors.partitioningBy(
n -> n % 2 == 0
));

Result:

true
2 4 6

false
1 3 5

Think:

True bucket and False bucket.

5. Joining ⭐⭐⭐⭐

For Strings.

Collectors.joining()

Example:

String s =
Stream.of("Java", "Spring", "Boot")
.collect(Collectors.joining("-"));

Output

Java-Spring-Boot

Think:

Merge strings together.

6. Statistics ⭐⭐⭐⭐
   counting()
   summingInt()
   averagingInt()
   maxBy()
   minBy()
   summarizingInt()

Example

int total =
employees.stream()
.collect(
Collectors.summingInt(Employee::getSalary)
);

Average

double avg =
employees.stream()
.collect(
Collectors.averagingInt(Employee::getSalary)
);

Think:

Calculate numbers.

The only ones interviewers usually expect
toList()
toSet()
toMap()

groupingBy()

partitioningBy()

joining()

counting()

summingInt()

averagingInt()

mapping()   (occasionally)

That's enough for most backend interviews.

Memory Trick
Collectors

│
├── Collection
│     ├── toList()
│     ├── toSet()
│     └── toCollection()
│
├── Map
│     └── toMap()
│
├── Group
│     ├── groupingBy()
│     └── partitioningBy()
│
├── String
│     └── joining()
│
└── Statistics
├── counting()
├── summingInt()
├── averagingInt()
├── maxBy()
└── minBy()



1. Method References (Most common)
   Map<Integer, String> map =
   employees.stream()
   .collect(Collectors.toMap(
   Employee::getId,
   Employee::getName
   ));
2. Lambda Expressions

Exactly the same logic.

Map<Integer, String> map =
employees.stream()
.collect(Collectors.toMap(
e -> e.getId(),
e -> e.getName()
));
3. Store the Entire Object

Instead of mapping to the name:

Map<Integer, Employee> map =
employees.stream()
.collect(Collectors.toMap(
Employee::getId,
e -> e
));

Result:

1 -> Employee(...)
2 -> Employee(...)



Group by Age
Map<Integer, List<Employee>> map =
employees.stream()
.collect(Collectors.groupingBy(Employee::getAge));

Result

23
Ram
John

25
Alice
Group and Count

Suppose you only want the number of employees in each department.

Map<String, Long> map =
employees.stream()
.collect(
Collectors.groupingBy(
Employee::getDepartment,
Collectors.counting()
));

Output

IT -> 2
HR -> 2
Finance -> 1
Group and Sum Salary
Map<String, Integer> map =
employees.stream()
.collect(
Collectors.groupingBy(
Employee::getDepartment,
Collectors.summingInt(Employee::getSalary)
));

Output

IT -> 120000

HR -> 80000
Group and Average Salary
Map<String, Double> map =
employees.stream()
.collect(
Collectors.groupingBy(
Employee::getDepartment,
Collectors.averagingInt(Employee::getSalary)
));

Output

IT -> 60000

HR -> 40000


groupingBy() → many groups
partitioningBy() → exactly 2 groups (true and false)



Example

Suppose you have:

List<Integer> list = List.of(1,2,3,4,5,6);

Now partition into even and odd numbers.

Map<Boolean, List<Integer>> map =
list.stream()
.collect(Collectors.partitioningBy(n -> n % 2 == 0));

Result:

true
[2,4,6]

false
[1,3,5]

Notice the keys are always:

true
false
Compare with groupingBy()
Collectors.groupingBy(n -> n % 2 == 0)

also produces:

true
false

So why have partitioningBy()?

Because Java knows there can only be two groups, making it simpler and optimized for boolean conditions.

Employee Example

Employees:

Ram   60000
John  40000
Bob   80000

Partition by salary > 50000

Map<Boolean, List<Employee>> map =
employees.stream()
.collect(
Collectors.partitioningBy(
e -> e.getSalary() > 50000
));

Result

true
Ram
Bob

false
John
Count in Each Partition
Map<Boolean, Long> map =
employees.stream()
.collect(
Collectors.partitioningBy(
e -> e.getSalary() > 50000,
Collectors.counting()
));

Output

true  -> 2
false -> 1
Sum Salary in Each Partition
Map<Boolean, Integer> map =
employees.stream()
.collect(
Collectors.partitioningBy(
e -> e.getSalary() > 50000,
Collectors.summingInt(Employee::getSalary)
));

Output

true  -> 140000
false -> 40000



These are called downstream collectors. They are usually used with collect() or inside groupingBy()/partitioningBy().

Let's understand each one.

1. counting()

Counts the number of elements.

long count = employees.stream()
.collect(Collectors.counting());

Equivalent to:

long count = employees.stream().count();

Output:

5
With groupingBy()
Map<String, Long> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.counting()
));

Output

IT -> 3
HR -> 2
2. summingInt()

Adds integer values.

Suppose

Ram    50000
John   60000
Bob    40000
int total =
employees.stream()
.collect(Collectors.summingInt(Employee::getSalary));

Output

150000
With grouping
Map<String,Integer> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.summingInt(Employee::getSalary)
));

Output

IT -> 110000
HR -> 40000
3. averagingInt()

Finds the average.

double avg =
employees.stream()
.collect(Collectors.averagingInt(Employee::getSalary));

Output

50000

Grouped:

Map<String,Double> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.averagingInt(Employee::getSalary)
));

Output

IT -> 55000

HR -> 40000

What is collectingAndThen()?

It lets you:

Collect the stream normally, and then
Apply one final transformation to the collected result.

Think of it as:

Stream
↓
Collector
↓
Intermediate Result
↓
One Final Operation
↓
Final Result
Syntax
Collectors.collectingAndThen(
downstreamCollector,
finisher
)
downstreamCollector → How to collect the stream (toList(), toSet(), groupingBy(), etc.)
finisher → A function applied to the collected result.
Example 1: Make the List Unmodifiable

Without collectingAndThen():

List<String> list =
names.stream()
.collect(Collectors.toList());

This list can be modified:

list.add("Java"); // ✅

With collectingAndThen():

List<String> list =
names.stream()
.collect(Collectors.collectingAndThen(
Collectors.toList(),
Collections::unmodifiableList
));

Now:

list.add("Java"); // ❌ UnsupportedOperationException
Example 2: Get the Size of the Collected List
int size =
names.stream()
.collect(Collectors.collectingAndThen(
Collectors.toList(),
List::size
));

Execution:

Stream
↓
toList()
↓
["Java", "Spring", "Boot"]
↓
size()
↓
3
Example 3: Sort After Collecting
List<Integer> sorted =
list.stream()
.collect(Collectors.collectingAndThen(
Collectors.toList(),
l -> {
Collections.sort(l);
return l;
}
));

First it collects into a list, then sorts that list.