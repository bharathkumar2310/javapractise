1. Why does toMap() exist?

Imagine you have a list of employees.

      List<Employee> employees = List.of(
      new Employee(1, "John", 50000),
      new Employee(2, "Alice", 70000),
      new Employee(3, "Bob", 60000)
      );

Suppose you want

      1 -> John
      2 -> Alice
      3 -> Bob

Instead of writing

      Map<Integer, String> map = new HashMap<>();
      
      for (Employee e : employees) {
      map.put(e.getId(), e.getName());
      }

Streams allow

      Map<Integer, String> map =
      employees.stream()
      .collect(Collectors.toMap(
      Employee::getId,
      Employee::getName
      ));

Purpose:

Convert a stream into a Map by specifying how to generate the key and value.

2. All Overloads

There are 4 overloads.

Overload 1
      
      public static <T, K, U>
      Collector<T, ?, Map<K, U>> toMap(
      Function<? super T, ? extends K> keyMapper,
      Function<? super T, ? extends U> valueMapper)

Example

      employees.stream()
      .collect(Collectors.toMap(
      Employee::getId,
      Employee::getName
      ));

Produces

      1 -> John
      2 -> Alice
      3 -> Bob

Overload 2
      
      public static <T, K, U>
      Collector<T, ?, Map<K, U>> toMap(
      Function<? super T, ? extends K> keyMapper,
      Function<? super T, ? extends U> valueMapper,
      BinaryOperator<U> mergeFunction)

Used when duplicate keys exist.

Example

      List<String> list = List.of(
      "apple",
      "ant",
      "ball"
      );

Suppose

      key = first character

Without merge

      a -> apple
      a -> ant

Duplicate.

Throws

      IllegalStateException

With merge

      Map<Character, String> map =
      list.stream()
      .collect(Collectors.toMap(
      s -> s.charAt(0),
      s -> s,
      (oldValue, newValue) -> oldValue
      ));

Result

      a -> apple
      b -> ball

Overload 3
      
      public static <T,K,U,M extends Map<K,U>>
      Collector<T, ?, M> toMap(
      Function<? super T, ? extends K> keyMapper,
      Function<? super T, ? extends U> valueMapper,
      BinaryOperator<U> mergeFunction,
      Supplier<M> mapSupplier)

Allows choosing the Map implementation.

Example

      TreeMap<Integer,String> map =
      employees.stream()
      .collect(Collectors.toMap(
      Employee::getId,
      Employee::getName,
      (a,b)->a,
      TreeMap::new
      ));

Now result is sorted.

Overload 4

People often think there are four overloads because toUnmodifiableMap() has similar signatures, but Collectors.toMap() itself has only the three overloads shown above.

3. Generic Signature Explained

         Let's simplify the first overload.

            <T,K,U>

Meaning

      T = Stream element
      
      K = Key type
      
      U = Value type

Example

Employee

↓

id

↓

name

So

T = Employee

K = Integer

U = String
4. Return Type
   Collector<T, ?, Map<K,U>>

Don't panic.

Read it like

Input

↓

Employee

↓

Collector

↓

Map<Integer,String>

Final output

Map<Integer,String>
5. Parameter 1 — keyMapper
   Function<T,K>

Job:

Employee

↓

return key

Example

Employee::getId

or

e -> e.getId()

Produces

1

2

3
6. Parameter 2 — valueMapper
   Function<T,U>

Job

Employee

↓

return value

Example

Employee::getName

Produces

John

Alice

Bob
7. Parameter 3 — mergeFunction

Needed only if duplicate keys are possible.

BinaryOperator<U>

Receives

oldValue

newValue

Returns

final value

Example

(oldValue,newValue)->oldValue

Keep first.

Example

(oldValue,newValue)->newValue

Keep latest.

Example

(oldValue,newValue)
->oldValue+","+newValue

Produces

A,B,C
8. Parameter 4 — mapSupplier
   Supplier<Map>

Normally

HashMap

If you want

TreeMap

LinkedHashMap

ConcurrentHashMap

pass

TreeMap::new

or

LinkedHashMap::new
9. Internal Working

Suppose

Employee(1,John)

Employee(2,Alice)

Employee(3,Bob)

Flow

Employee

↓

keyMapper

↓

1
Employee

↓

valueMapper

↓

John

Collector internally performs

map.put(1, "John");

Next

map.put(2, "Alice");

Next

map.put(3, "Bob");

Essentially, it's doing what you would have written in a loop, with additional handling for duplicate keys and custom Map implementations.

10. What Happens on Duplicate Keys?

Example

List<String> words = List.of(
"apple",
"ant"
);
Collectors.toMap(
s -> s.charAt(0),
s -> s
)

Produces

a

a

Without merge function

Exception
IllegalStateException:
Duplicate key

Interviewers ask this frequently.

11. Common Merge Strategies

Keep first

(oldValue, newValue) -> oldValue

Keep latest

(oldValue, newValue) -> newValue

Longest string

(a, b) -> a.length() > b.length() ? a : b

Highest salary

BinaryOperator.maxBy(
Comparator.comparing(Employee::getSalary)
)

Lowest salary

BinaryOperator.minBy(
Comparator.comparing(Employee::getSalary)
)

Merge lists

(list1, list2) -> {
list1.addAll(list2);
return list1;
}

(Though for grouping multiple values under a key, groupingBy() is usually a better fit.)

12. Interview Questions Based on toMap()

Be able to solve:

List<Employee> → Map<id, Employee>
List<Employee> → Map<id, name>
Handle duplicate IDs
Keep latest employee
Keep highest salary per ID
Convert list to TreeMap
Convert list to LinkedHashMap
Merge duplicate values
Create frequency map (can also be done with groupingBy(counting()))
Index list elements
13. Common Mistakes
1. Duplicate key
   Collectors.toMap(...)

Throws

IllegalStateException

if duplicate keys occur and no merge function is provided.

2. Thinking toMap() groups values

It doesn't.

Wrong expectation

A

↓

apple

ant

toMap() cannot automatically create

A -> [apple, ant]

For that use

Collectors.groupingBy(...)
3. Assuming insertion order

By default, toMap() returns a HashMap, whose iteration order is not guaranteed.

If order matters, use

LinkedHashMap::new
14. toMap() vs groupingBy()

Suppose

apple

ant

ball

toMap()

a -> apple

❌ duplicate key on "ant"

groupingBy()

a -> [apple, ant]

b -> [ball]

Rule of thumb:

One value per key → toMap()
Many values per key → groupingBy()
15. Practice Problems

Start with these in order:

List<Employee> → Map<Integer, Employee>
List<Employee> → Map<Integer, String>
Handle duplicate employee IDs by keeping the first.
Handle duplicate employee IDs by keeping the latest.
Handle duplicate employee IDs by keeping the employee with the highest salary.
Convert a list to a TreeMap.
Convert a list to a LinkedHashMap.
Convert List<String> to Map<String, Integer> where the value is the string length.
Build a frequency map using toMap().
Compare the toMap() solution with the equivalent groupingBy() solution.


---------------------------------------------------------------------------------------------------------------------
1. Why does groupingBy() exist?

The purpose of toMap() is:

One key → One value

Example:

1 -> John
2 -> Alice
3 -> Bob

But what if multiple employees belong to the same department?

John   -> IT
Alice  -> IT
Bob    -> HR
David  -> HR
Mike   -> IT

Can toMap() do this?

IT -> John
IT -> Alice   // ❌ Duplicate key

No. It throws an exception because toMap() expects a single value for each key (unless you provide a merge function).

What we actually want is:

IT -> [John, Alice, Mike]
HR -> [Bob, David]

This is exactly what groupingBy() is designed for.

One key → Many values

2. Mental Model

Think of sorting students into classrooms.

Students:

Rahul
John
Alice
Bob
David

Departments:

Rahul -> IT
John  -> IT
Alice -> HR
Bob   -> HR
David -> Finance

groupingBy() creates "boxes":

IT
├── Rahul
└── John

HR
├── Alice
└── Bob

Finance
└── David

Every time it sees a department:

If the department doesn't exist, create a new list.
Add the employee to that list.
3. Syntax

There are three overloads.

Overload 1
      
      Collectors.groupingBy(
      classifier
      )
Overload 2
      
      Collectors.groupingBy(
      classifier,
      downstreamCollector
      )
Overload 3

      
      Collectors.groupingBy(
      classifier,
      mapFactory,
      downstreamCollector
      )

We'll understand each one deeply.

4. Overload 1

Signature:
      
      public static <T,K>
      Collector<T, ?, Map<K, List<T>>> groupingBy(
      Function<? super T, ? extends K> classifier)

Don't memorize it.

Read it.

Input
↓

Employee

↓

classifier

↓

Department

↓

Output

      Map<Department, List<Employee>>

Example

      Map<String, List<Employee>> map =
      employees.stream()
      .collect(Collectors.groupingBy(Employee::getDepartment));

Suppose

      John  IT
      Alice IT
      Bob   HR
      David HR

Output:

      IT
      John
      Alice
      
      HR
      Bob
      David

Internally, it's conceptually similar to:

Map<String, List<Employee>> map = new HashMap<>();

      for (Employee e : employees) {
      
          map.computeIfAbsent(
              e.getDepartment(),
              k -> new ArrayList<>()
          ).add(e);
      
      }

You don't write this manually because groupingBy() does it for you.

5. What is the classifier?

         The classifier answers one question:

How should I group these elements?

Examples:

      Group by department
      
      Employee::getDepartment
      
      Group by age
      
      Employee::getAge
      
      Group by city
      
      Employee::getCity
      
      Group strings by length
      
      String::length
      
      Group numbers by even/odd
      
      n -> n % 2
      
      Whatever the classifier returns becomes the key in the map.

6. Internal Working

Imagine this input:

         John   IT
         Alice  IT
         Bob    HR
         David  HR
         Mike   IT

Initially:

{}

Read John:

IT not found

Create list

IT -> [John]

Read Alice:

IT exists

IT -> [John, Alice]

Read Bob:

HR not found

HR -> [Bob]

Read David:

HR -> [Bob, David]

Read Mike:

IT -> [John, Alice, Mike]

Final map:

IT -> [John, Alice, Mike]
HR -> [Bob, David]
7. Return Type

This surprises many people.

Map<K, List<T>>

Notice:

The values are always lists in the first overload.

For example:

Map<String, List<Employee>>

because every group stores all matching elements.

8. Interview Examples

   Group employees by department

         employees.stream()
         .collect(Collectors.groupingBy(Employee::getDepartment));
         Group strings by length
         words.stream()
         .collect(Collectors.groupingBy(String::length));

Input:

cat
dog
apple
ball
hi

Output:

2 -> [hi]

3 -> [cat, dog]

4 -> [ball]

5 -> [apple]
Group numbers by remainder
numbers.stream()
.collect(Collectors.groupingBy(n -> n % 3));

Input:

1 2 3 4 5 6 7

Output:

0 -> [3, 6]

1 -> [1, 4, 7]

2 -> [2, 5]
9. groupingBy() vs toMap()

Suppose:

apple
ant
ball
toMap()
Collectors.toMap(
s -> s.charAt(0),
s -> s
)

Fails:

Duplicate key 'a'
groupingBy()
Collectors.groupingBy(
s -> s.charAt(0)
)

Output:

a -> [apple, ant]

b -> [ball]

This is the biggest conceptual difference between the two.

10. Interview Rule

Whenever you hear:

"Group..."
"Categorize..."
"Bucket..."
"Organize by..."

your first thought should be:

Collectors.groupingBy(...)

Examples:

Group employees by department
Group students by grade
Group books by author
Group orders by customer
Group words by length
Group numbers by parity


groupingBy() — Overload 2
Signature
      
      public static <T, K, A, D>
      Collector<T, ?, Map<K, D>> groupingBy(
      Function<? super T, ? extends K> classifier,
      Collector<? super T, A, D> downstream)

Don't try to memorize this.

Understand it.

Why do we need a second overload?

Suppose we have

      John    IT      50000
      Alice   IT      70000
      Bob     HR      60000
      David   HR      55000
      Mike    IT      80000

Using overload 1

      Collectors.groupingBy(Employee::getDepartment)

Result

      IT
      John
      Alice
      Mike
      
      HR
      Bob
      David

This is good.

But suppose the interviewer asks

      Count employees in each department.

Do we really need the whole employee list?

No.

We only need

IT -> 3
HR -> 2

That's why overload 2 exists.

What is a Downstream Collector?

      Think of it as a collector that works inside each group.

Flow:

      Employee Stream
      
      ↓
      
      Group by Department
      
      ↓
      
      IT
      HR
      Finance
      
      ↓
      
      Apply another collector to each group
      
      ↓
      
      Result
      
      So instead of returning
      
      IT -> [John, Alice, Mike]
      
      it can return
      
      IT -> 3
      
      or
      
      IT -> John,Alice,Mike
      
      or
      
      IT -> Highest Salary Employee
      
      or
      
      IT -> Average Salary

The downstream collector decides what each group's value becomes.

Visual Flow

Suppose

      John    IT
      Alice   IT
      Bob     HR
      David   HR
      Mike    IT

Step 1

      Group
      
      ↓
      
      IT
      
      John
      Alice
      Mike
      
      HR
      
      Bob
      David
      
      Step 2
      
      Now apply a collector to each group.
      
      Example
      
      Collectors.counting()
      
      Result
      
      IT -> 3
      
      HR -> 2
      Example 1 — counting()
      Map<String, Long> map =
      employees.stream()
      .collect(Collectors.groupingBy(
      Employee::getDepartment,
      Collectors.counting()
      ));
      
      Output
      
      IT -> 3
      HR -> 2
      Finance -> 5
      Internal Working
      
      Without Streams
      
      Map<String, Long> result = new HashMap<>();
      
      for(Employee e : employees){
      
          result.put(
              e.getDepartment(),
              result.getOrDefault(
                  e.getDepartment(),
                  0L
              ) + 1
          );
      
      }

groupingBy(..., counting()) does this automatically.

Example 2 — mapping()

Suppose

      John IT
      Alice IT
      Bob HR

We don't want employees.

We only want names.

      Map<String, List<String>> map =
      employees.stream()
      .collect(Collectors.groupingBy(
      Employee::getDepartment,
      Collectors.mapping(
      Employee::getName,
      Collectors.toList()
      )
      ));

Result

      IT
      
      John
      Alice
      
      HR
      
      Bob
      
      Notice
      
      Instead of
      
      IT
      
      Employee
      
      Employee
      
      we get
      
      IT
      
      String
      
      String
      
      because mapping() converts each employee into its name before collecting.

Example 3 — maxBy()

Highest salary employee per department.

      Map<String, Optional<Employee>> map =
      employees.stream()
      .collect(Collectors.groupingBy(
      Employee::getDepartment,
      Collectors.maxBy(
      Comparator.comparing(Employee::getSalary)
      )
      ));

Output

      IT -> Mike
      
      HR -> Bob
      
      Why Optional<Employee>?

Because maxBy() returns an Optional<T>.

Example 4 — minBy()
      
      Collectors.groupingBy(
      Employee::getDepartment,
      Collectors.minBy(
      Comparator.comparing(Employee::getSalary)
      )
      )

Result

      IT -> John
      
      HR -> David
Example 5 — summarizingInt()
      
      Collectors.groupingBy(
      Employee::getDepartment,
      Collectors.summarizingInt(
      Employee::getSalary
      )
      )

Returns

      IT
      
      count
      
      sum
      
      min
      
      max
      
      average

One collector gives you all salary statistics for each department.

Example 6 — joining()
      
      Collectors.groupingBy(
      Employee::getDepartment,
      Collectors.mapping(
      Employee::getName,
      Collectors.joining(", ")
      )
      )

Output

IT -> John, Alice, Mike

HR -> Bob, David

Example 7 — toSet()
      
      Collectors.groupingBy(
      Employee::getDepartment,
      Collectors.mapping(
      Employee::getName,
      Collectors.toSet()
      )
      )

Removes duplicate names within each department.

Generic Signature Explained
Map<K,D>

Notice carefully.

Earlier

Map<K,List<T>>

Now

Map<K,D>

What is D?

The downstream collector decides.

Examples

If downstream is

Collectors.counting()

then

D = Long

Result

Map<String, Long>

If downstream is

Collectors.maxBy(...)

then

D = Optional<Employee>

Result

Map<String, Optional<Employee>>

If downstream is

Collectors.mapping(..., Collectors.toSet())

then

D = Set<String>

Result

Map<String, Set<String>>

The return type changes based on the downstream collector.



groupingBy() — Overload 3


Signature
      
      public static <T, K, A, D>
      Collector<T, ?, Map<K, D>> groupingBy(
      Function<? super T, ? extends K> classifier,
      Collector<? super T, A, D> downstream)

Don't try to memorize this.

Understand it.

Why do we need a second overload?

Suppose we have
      
      John    IT      50000
      Alice   IT      70000
      Bob     HR      60000
      David   HR      55000
      Mike    IT      80000

Using overload 1

Collectors.groupingBy(Employee::getDepartment)

Result
      
      IT
      John
      Alice
      Mike
      
      HR
      Bob
      David

This is good.

But suppose the interviewer asks

Count employees in each department.

Do we really need the whole employee list?

No.

We only need

IT -> 3
HR -> 2

That's why overload 2 exists.

What is a Downstream Collector?

Think of it as a collector that works inside each group.

Flow:

Employee Stream

↓

Group by Department

↓

IT
HR
Finance

↓

Apply another collector to each group

↓

Result

So instead of returning

IT -> [John, Alice, Mike]

it can return

IT -> 3

or

IT -> John,Alice,Mike

or

IT -> Highest Salary Employee

or

IT -> Average Salary

The downstream collector decides what each group's value becomes.

Visual Flow

Suppose

John    IT
Alice   IT
Bob     HR
David   HR
Mike    IT

Step 1

Group

↓

IT

John
Alice
Mike

HR

Bob
David

Step 2

Now apply a collector to each group.

Example

Collectors.counting()

Result

IT -> 3

HR -> 2
Example 1 — counting()
Map<String, Long> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.counting()
));

Output

IT -> 3
HR -> 2
Finance -> 5
Internal Working

Without Streams

Map<String, Long> result = new HashMap<>();

for(Employee e : employees){

    result.put(
        e.getDepartment(),
        result.getOrDefault(
            e.getDepartment(),
            0L
        ) + 1
    );

}

groupingBy(..., counting()) does this automatically.

Example 2 — mapping()

Suppose

John IT
Alice IT
Bob HR

We don't want employees.

We only want names.

Map<String, List<String>> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.mapping(
Employee::getName,
Collectors.toList()
)
));

Result

IT

John
Alice

HR

Bob

Notice

Instead of

IT

Employee

Employee

we get

IT

String

String

because mapping() converts each employee into its name before collecting.

Example 3 — maxBy()

Highest salary employee per department.

Map<String, Optional<Employee>> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.maxBy(
Comparator.comparing(Employee::getSalary)
)
));

Output

IT -> Mike

HR -> Bob

Why Optional<Employee>?

Because maxBy() returns an Optional<T>.

Example 4 — minBy()
Collectors.groupingBy(
Employee::getDepartment,
Collectors.minBy(
Comparator.comparing(Employee::getSalary)
)
)

Result

IT -> John

HR -> David
Example 5 — summarizingInt()
Collectors.groupingBy(
Employee::getDepartment,
Collectors.summarizingInt(
Employee::getSalary
)
)

Returns

IT

count

sum

min

max

average

One collector gives you all salary statistics for each department.

Example 6 — joining()
Collectors.groupingBy(
Employee::getDepartment,
Collectors.mapping(
Employee::getName,
Collectors.joining(", ")
)
)

Output

IT -> John, Alice, Mike

HR -> Bob, David
Example 7 — toSet()
Collectors.groupingBy(
Employee::getDepartment,
Collectors.mapping(
Employee::getName,
Collectors.toSet()
)
)

Removes duplicate names within each department.

Generic Signature Explained
Map<K,D>

Notice carefully.

Earlier

Map<K,List<T>>

Now

Map<K,D>

What is D?

The downstream collector decides.

Examples

If downstream is

Collectors.counting()

then

D = Long

Result

Map<String, Long>

If downstream is

Collectors.maxBy(...)

then

D = Optional<Employee>

Result

Map<String, Optional<Employee>>

If downstream is

Collectors.mapping(..., Collectors.toSet())

then

D = Set<String>

Result

Map<String, Set<String>>

The return type changes based on the downstream collector.

groupingBy() — Overload 3
Signature
public static <T, K, D, A, M extends Map<K, D>>
Collector<T, ?, M> groupingBy(
Function<? super T, ? extends K> classifier,
Supplier<M> mapFactory,
Collector<? super T, A, D> downstream)

Don't memorize it. Let's simplify it.

classifier
+
mapFactory
+
downstream

Think of it as:

How should I group?

Which Map implementation should I use?

What should I do with each group?

Why do we need this overload?

Let's compare.

Overload 1
Collectors.groupingBy(Employee::getDepartment)

Returns

HashMap
Overload 2
Collectors.groupingBy(
Employee::getDepartment,
Collectors.counting()
)

Still returns

HashMap

Notice something?

You never chose the Map implementation.

Java silently creates a HashMap.

Sometimes that's fine.

Sometimes it's not.

Suppose we want sorted departments

Departments

HR
Finance
IT
Admin

HashMap might print

IT
Admin
HR
Finance

Order is not guaranteed.

Instead we want

Admin
Finance
HR
IT

For that we need

TreeMap::new
Syntax
Collectors.groupingBy(
classifier,
mapFactory,
downstream
)

Example

Collectors.groupingBy(
Employee::getDepartment,
TreeMap::new,
Collectors.counting()
)
Example 1 — TreeMap
Map<String, Long> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
TreeMap::new,
Collectors.counting()
));

Output

Admin -> 2
Finance -> 1
HR -> 5
IT -> 8

Departments are sorted alphabetically because TreeMap sorts its keys.

Internal Working

Instead of

Map<String, Long> map = new HashMap<>();

Java does

Map<String, Long> map = new TreeMap<>();

Everything else works exactly the same.

Example 2 — LinkedHashMap

Suppose employees appear in this order

IT
HR
Finance
Admin

Using

LinkedHashMap::new
Collectors.groupingBy(
Employee::getDepartment,
LinkedHashMap::new,
Collectors.counting()
)

Output

IT
HR
Finance
Admin

Insertion order is preserved.

Example 3 — HashMap

Explicitly

Collectors.groupingBy(
Employee::getDepartment,
HashMap::new,
Collectors.counting()
)

Same as the default behavior.

Usually unnecessary.

Example 4 — ConcurrentHashMap
Collectors.groupingBy(
Employee::getDepartment,
ConcurrentHashMap::new,
Collectors.counting()
)

Useful when working with concurrent collectors or when a concurrent map is specifically needed.



-------------------------------------------------------------------------------------------------------------------------------------------------

1. Why does partitioningBy() exist?

Suppose we have employees:

John    25
Alice   32
Bob     18
David   45
Mike    27

The interviewer asks:

Divide employees into adults and minors.

There are only two possible groups.

true
Alice
David
Mike
John

false
Bob

Notice something?

There are only two buckets.

true

false

That's exactly what partitioningBy() is designed for.

Mental Model

Think of a security gate.

Every employee reaches the gate.

The gate asks ONE question.

Is salary > 50000?

Answer is either

YES

or

NO

Nothing else.

Every element goes into one of two boxes.

true

false

Unlike groupingBy(), there can never be a third bucket.

groupingBy vs partitioningBy

Suppose

John IT
Alice HR
Bob Finance
Mike IT

Grouping

Collectors.groupingBy(Employee::getDepartment)

Produces

IT

HR

Finance

Number of groups?

Depends on data.

Could be

3

5

10

100

No limit.

Partitioning

Collectors.partitioningBy(
e -> e.getSalary() > 50000
)

Produces

true

false

Always exactly 2 groups.

Signature

There are only two overloads.

Overload 1
public static <T>
Collector<T, ?, Map<Boolean, List<T>>> partitioningBy(
Predicate<? super T> predicate)

Notice

Not Function.

Not classifier.

It takes

Predicate<T>

because a Predicate always returns

true

or

false
Return Type
Map<Boolean, List<T>>

Notice carefully.

Keys are always

true

false

Unlike groupingBy

Department

Age

City

Length

Partitioning has only

true

false
Example 1

Employees

John 50000

Alice 70000

Bob 40000

Mike 90000
Map<Boolean,List<Employee>> map =
employees.stream()
.collect(Collectors.partitioningBy(
e -> e.getSalary() >= 60000
));

Output

true

Alice

Mike

false

John

Bob
Internal Working

Imagine Java doing

Map<Boolean,List<Employee>> map =
new HashMap<>();

Initially

true

↓

[]

false

↓

[]

Read John

50000 >=60000 ?

No

Add

false

↓

John

Read Alice

70000 >=60000 ?

Yes
true

↓

Alice

Continue

Result

true

Alice

Mike

false

John

Bob
Why Predicate?

Predicate means

boolean test(T t);

Example

e -> e.getSalary() > 50000

returns

true

or

false

That boolean becomes the key.

More Examples

Group even and odd

Collectors.partitioningBy(
n -> n % 2 == 0
)

Result

true

2
4
6

false

1
3
5

Strings

Collectors.partitioningBy(
s -> s.length() > 5
)

Output

true

Elephant

Computer

false

Dog

Cat

Pen

Employees

Collectors.partitioningBy(
Employee::isActive
)

Output

true

Active employees

false

Inactive employees
Can groupingBy() do the same?

Yes.

Collectors.groupingBy(
e -> e.getSalary()>50000
)

Produces

true

false

So why does partitioningBy() exist?

Difference

groupingBy()

Can return

0 groups

1 group

2 groups

100 groups

depending on the classifier.

partitioningBy()

Always returns

true

false

Even if one partition is empty.

Example

All employees have salary greater than 1000.

Collectors.partitioningBy(
e -> e.getSalary()>1000
)

Output

true

All employees

false

[]

Notice

The false key is still present.

With groupingBy()

Collectors.groupingBy(
e -> e.getSalary()>1000
)

Output

true

All employees

There is no false key because no element belonged there.

This is one of the most common interview questions.

Performance

Since there are only two buckets,

Java can optimize this collector.

It doesn't need to keep creating new groups like groupingBy().

That's why, when the condition is truly binary, partitioningBy() is usually the better semantic choice.

Common Interview Uses

Partition employees

Active

Inactive

Partition numbers

Even

Odd

Partition students

Passed

Failed

Partition orders

Delivered

Pending

Partition people

Adult

Minor
Mental Formula

Whenever the interviewer says

Divide...

Split...

Separate...

Passed vs Failed...

Active vs Inactive...

Male vs Female...

True vs False...

Your brain should immediately think

Collectors.partitioningBy(...)
Overload 2

Just like groupingBy(), partitioningBy() also has a second overload.

public static <T, D>
Collector<T, ?, Map<Boolean, D>> partitioningBy(
Predicate<? super T> predicate,
Collector<? super T, ?, D> downstream)

Notice the similarity:

groupingBy

classifier

+

downstream

becomes

partitioningBy

predicate

+

downstream

So instead of

true

↓

List<Employee>

you can produce

true

↓

Count

or

true

↓

Highest Salary

or

true

↓

Average Salary

or

true

↓

Employee Names

The downstream collector works exactly the same way as it does with groupingBy(). Since you've already mastered downstream collectors conceptually, the second overload will feel very familiar.



--------------------------------------------------------------------------------------------------------------------------------------------------------------------


Why mapping() next?

This is where most candidates get confused.

Suppose you have:

Employee

After grouping:

IT

Employee
Employee
Employee

But the interviewer asks:

Group employees by department, but return only their names.

You don't want:

IT -> [Employee, Employee]

You want:

IT -> [John, Alice]

That's exactly why mapping() exists.

Example:

employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.mapping(
Employee::getName,
Collectors.toList()
)
));

Notice what's happening:

Employee
↓ mapping(Employee::getName)
String
↓ toList()
List<String>
↓
Map<String, List<String>>

Think of mapping() as the map() intermediate operation, but used inside a downstream collector.

Then filtering()

Suppose the interviewer says:

Group employees by department, but only include employees with salary > 50,000.

Without filtering(), you might write:

employees.stream()
.filter(e -> e.getSalary() > 50000)
.collect(...);

But that filters before grouping.

Sometimes you want to group everything first, then filter within each group.

That's why Collectors.filtering() exists.

Then flatMapping()

Suppose:

Employee

has

List<String> skills;

You want:

IT

Java
Spring
Docker
Kafka

instead of

IT

[Java, Spring]
[Docker]
[Kafka]

That's where flatMapping() shines.

The Most Important Mental Model

The next three collectors are all downstream collectors.

groupingBy()

↓

Employee

↓

mapping()

↓

String

↓

filtering()

↓

Keep some Strings

↓

flatMapping()

↓

Flatten nested collections

↓

toList()

Once you understand this pipeline, the rest of the collector API becomes much easier.


After grouping it is like treating each list as a stream and we do map, filter or flatMap and collect.

That's 95% correct.

The more accurate way to think is:

Stream<Employee>

        ↓

groupingBy(Employee::getDepartment)

        ↓

IT -----------------------------> downstream collector
HR -----------------------------> downstream collector
Finance ------------------------> downstream collector

The downstream collector is applied independently to each group.

Example 1: mapping()
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.mapping(
Employee::getName,
Collectors.toList()
)
));

Think of it as Java internally doing:

IT Group

Employee
Employee
Employee

↓

map(Employee::getName)

↓

John
Alice
Mike

↓

toList()

↓

[John, Alice, Mike]

Then HR:

Employee
Employee

↓

map(Employee::getName)

↓

Bob
David

↓

toList()

↓

[Bob, David]

Exactly like running:

itEmployees.stream()
.map(Employee::getName)
.toList();

for every group.

Example 2: filtering()
Collectors.filtering(
e -> e.getSalary() > 50000,
Collectors.toList()
)

IT group

John 50000

Alice 70000

Mike 80000

↓

filter

↓

Alice

Mike

↓

toList()

↓

[Alice, Mike]
Example 3: flatMapping()

Suppose

Employee

↓

List<String> skills

IT group

John

[Java, Spring]

Alice

[Kafka, Docker]

↓

flatMap()

↓

Java

Spring

Kafka

Docker

↓

toList()

Exactly like

itEmployees.stream()
.flatMap(e -> e.getSkills().stream())
.toList();

----------------------------------------------------------------------------------------------------------------------------------------------------------



Excellent. Now we'll move to Collectors.counting().

This is one of the easiest collectors, but it appears in many interview questions, especially with groupingBy().

Mastering Collectors.counting()
1. Why does counting() exist?

Suppose you have

IT
John
Alice
Mike

HR
Bob
David

The interviewer asks

Count employees in each department.

You don't need the employees.

You only need

IT -> 3

HR -> 2

Instead of writing loops, we use

Collectors.counting()
2. Signature

There is only one overload.

public static <T>
Collector<T, ?, Long> counting()

Notice

Collector<T, ?, Long>

The result is always

Long

NOT

Integer

This is a common interview question.

3. Return Type

Always

Long

Example

Long count =
employees.stream()
.collect(Collectors.counting());
4. Example Without Grouping
   List<String> names =
   List.of("John","Alice","Bob");
   Long count =
   names.stream()
   .collect(Collectors.counting());

Result

3

Equivalent to

names.stream().count();
5. Why not use stream().count()?

Good interview question.

These are equivalent:

employees.stream().count();

and

employees.stream()
.collect(Collectors.counting());

Both return

Long

So why does counting() exist?

Because it is a Collector.

That means it can be used inside another collector.

For example

Collectors.groupingBy(
Employee::getDepartment,
Collectors.counting()
)

You cannot write

groupingBy(..., stream().count())

because count() is a terminal operation, not a collector.

6. Example with groupingBy()
   Map<String, Long> map =
   employees.stream()
   .collect(Collectors.groupingBy(
   Employee::getDepartment,
   Collectors.counting()
   ));

Input

IT
John

IT
Alice

HR
Bob

IT
Mike

Output

IT -> 3

HR -> 1

-------------------------------------------------------------------------------------------------------------------------------------------------


1. Why do we need maxBy() and minBy()?

Suppose you have employees:

John    IT      50000
Alice   IT      70000
Mike    IT      90000
Bob     HR      60000
David   HR      55000

The interviewer asks:

Find the highest-paid employee in each department.

Expected output:

IT -> Mike
HR -> Bob

Instead of writing loops, we use:

Collectors.maxBy(...)
2. Signature

There is only one overload.

public static <T>
Collector<T, ?, Optional<T>> maxBy(
Comparator<? super T> comparator)

Notice the return type:

Optional<T>

This is the first thing interviewers expect you to know.

3. Why Optional<T>?

Suppose the stream is empty.

Example:

List<Employee> employees = List.of();

What is the maximum employee?

There isn't one.

Instead of returning null, Java returns:

Optional.empty()

If a maximum exists:

Optional<Employee>

contains the employee.

4. Basic Example
   Optional<Integer> max =
   List.of(10, 20, 5, 30)
   .stream()
   .collect(Collectors.maxBy(
   Integer::compareTo
   ));

Result:

30
5. Using a Comparator

Suppose

class Employee {
String name;
int salary;
}

Highest salary:

Optional<Employee> emp =
employees.stream()
.collect(Collectors.maxBy(
Comparator.comparing(
Employee::getSalary
)
));

Flow:

Employee

↓

Compare salary

↓

Largest salary wins

↓

Optional<Employee>
6. Example with groupingBy()

This is the real interview use case.

Map<String, Optional<Employee>> result =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.maxBy(
Comparator.comparing(
Employee::getSalary
)
)
));

Input:

IT
John 50000

IT
Alice 70000

IT
Mike 90000

HR
Bob 60000

HR
David 55000

Output:

IT -> Mike

HR -> Bob

Notice the type:

Map<String, Optional<Employee>>

because each group may theoretically be empty.

--------------------------------------------------------------------------------------------------------------------------------------------

Collectors.summarizingInt()

This collector is a favorite in interviews because it computes multiple statistics in a single pass.

1. Why do we need it?

Suppose the interviewer asks:

Calculate:

Total employees
Total salary
Minimum salary
Maximum salary
Average salary

Without summarizingInt(), you'd need multiple operations:

count()
sum()
min()
max()
average()

That means multiple traversals (or more complex code).

Instead, we use one collector.

2. Signature

Only one overload.

public static <T>
Collector<T, ?, IntSummaryStatistics>
summarizingInt(
ToIntFunction<? super T> mapper
)

Notice the return type:

IntSummaryStatistics
3. What is ToIntFunction?

Unlike mapping(), which returns any type, this mapper must return an int.

Example:

Employee::getSalary

or

String::length

Both return an int.

4. Return Type

The collector returns:

IntSummaryStatistics

This object contains five values.

count
sum
min
max
average
5. Example
   IntSummaryStatistics stats =
   employees.stream()
   .collect(Collectors.summarizingInt(
   Employee::getSalary
   ));

Suppose salaries are

50000
70000
60000
90000

Then

stats.getCount();      // 4

stats.getSum();        // 270000

stats.getMin();        // 50000

stats.getMax();        // 90000

stats.getAverage();    // 67500.0

Everything is calculated in one traversal.

6. Internal Working

Conceptually, Java does:

count++;

sum += salary;

min = Math.min(min, salary);

max = Math.max(max, salary);

When the stream ends:

average = sum / count;

All in one pass.

7. With groupingBy()

Very common interview question.

Map<String, IntSummaryStatistics> result =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.summarizingInt(
Employee::getSalary
)
));

Suppose

IT

50000

70000

90000

HR

60000

55000

Result:

IT

count = 3

sum = 210000

min = 50000

max = 90000

average = 70000
HR

count = 2

sum = 115000

min = 55000

max = 60000

average = 57500

Notice the return type:

Map<String, IntSummaryStatistics>
8. With partitioningBy()
   Map<Boolean, IntSummaryStatistics> result =
   employees.stream()
   .collect(Collectors.partitioningBy(
   e -> e.getSalary() > 50000,
   Collectors.summarizingInt(
   Employee::getAge
   )
   ));

Now you get age statistics separately for:

High salary employees
Low salary employees
9. IntSummaryStatistics API

These are the methods you should know for interviews.

Method	Returns
getCount()	Number of elements (long)
getSum()	Total sum (long)
getMin()	Minimum (int)
getMax()	Maximum (int)
getAverage()	Average (double)
10. Example with Strings
    IntSummaryStatistics stats =
    List.of("cat", "apple", "dog")
    .stream()
    .collect(Collectors.summarizingInt(
    String::length
    ));

Lengths:

3

5

3

Result:

count = 3

sum = 11

min = 3

max = 5

average = 3.67
11. Related Collectors

There are three versions.

Collectors.summarizingInt(...)
Collectors.summarizingLong(...)
Collectors.summarizingDouble(...)

The only difference is the numeric type.

12. Common Interview Questions
    Salary statistics by department
    Collectors.groupingBy(
    Employee::getDepartment,
    Collectors.summarizingInt(Employee::getSalary)
    )

------------------------------------------------------------------------------------------------------------------------------------------------


Mastering Collectors.joining()
1. Why do we need it?

Suppose we have

John
Alice
Bob
Mike

The interviewer asks

Convert them into

John, Alice, Bob, Mike

Instead of writing loops and manually adding commas, we use

Collectors.joining()
2. Important Point

joining() only works with CharSequence (like String).

It does not work directly with:

List<Integer>
List<Employee>

For those, first convert to String using mapping() or map().

3. Overloads

There are 3 overloads.

Overload 1
Collectors.joining()

Signature

Collector<CharSequence, ?, String>

No delimiter.

Example

List<String> words =
List.of("Java","Spring","Boot");

String result =
words.stream()
.collect(Collectors.joining());

Output

JavaSpringBoot
Overload 2
Collectors.joining(delimiter)

Signature

Collectors.joining(CharSequence delimiter)

Example

String result =
words.stream()
.collect(Collectors.joining(", "));

Output

Java, Spring, Boot
Overload 3
Collectors.joining(
delimiter,
prefix,
suffix
)

Example

String result =
words.stream()
.collect(Collectors.joining(
", ",
"[",
"]"
));

Output

[Java, Spring, Boot]
4. Internal Working

Think of Java doing

StringBuilder sb =
new StringBuilder();

Read

Java
Java

Read

Spring
Java, Spring

Read

Boot
Java, Spring, Boot

Finally

sb.toString();

Java uses a StringBuilder internally, making it much more efficient than repeated string concatenation.

5. Example with mapping()

Suppose

class Employee {

    String name;

    String department;

}

Interviewer asks

Group employees by department and return names separated by commas.

Map<String,String> result =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.mapping(
Employee::getName,
Collectors.joining(", ")
)
));

Output

IT

John, Alice, Mike

HR

Bob, David

This is one of the most common interview combinations.

6. Example with partitioningBy()
   Map<Boolean,String> map =
   employees.stream()
   .collect(Collectors.partitioningBy(
   Employee::isActive,
   Collectors.mapping(
   Employee::getName,
   Collectors.joining(", ")
   )
   ));

Output

true

John, Alice

false

Bob, David
7. Example with Stream
   List<String> cities =
   List.of(
   "Chennai",
   "Delhi",
   "Mumbai"
   );

String result =
cities.stream()
.collect(Collectors.joining(" -> "));

Output

Chennai -> Delhi -> Mumbai
8. Important Limitation

This does not compile.

List<Integer> numbers =
List.of(1,2,3);

numbers.stream()
.collect(Collectors.joining(", "));

Why?

Because

joining()

expects

CharSequence

and

Integer

is not a CharSequence.

Correct solution:

numbers.stream()
.map(String::valueOf)
.collect(Collectors.joining(", "));
9. Generic Signature


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------



Before Learning Collectors.reducing()

Most people confuse these two:

Stream.reduce(...)

and

Collectors.reducing(...)

They are related, but they're used in different places.

Stream.reduce() → Terminal operation on the whole stream.
Collectors.reducing() → A Collector, mainly used as a downstream collector.
Why do we need Collectors.reducing()?

Suppose the interviewer asks:

Find the total salary of each department.

Without collectors:

employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
// How do we sum salaries here?
));

We can't write:

stream.reduce(...)

inside groupingBy().

We need a Collector.

That's why Collectors.reducing() exists.

Overloads

There are 3 overloads.

Overload 1
Collectors.reducing(
BinaryOperator<T> op
)

Signature

Collector<T, ?, Optional<T>>

Returns:

Optional<T>

because there may be no elements.

Example

Largest number

Optional<Integer> max =
List.of(10,20,30)
.stream()
.collect(Collectors.reducing(
Integer::max
));

Flow

10

↓

20

↓

30

↓

Integer.max()

↓

30
Overload 2
Collectors.reducing(
identity,
BinaryOperator<T>
)

Signature

Collector<T, ?, T>

Now there is no Optional.

Why?

Because the identity acts as the initial value.

Example

Integer sum =
List.of(1,2,3,4)
.stream()
.collect(Collectors.reducing(
0,
Integer::sum
));

Flow

0

↓

1

↓

3

↓

6

↓

10

Output

10
Identity Rules

Identity should satisfy:

identity OP x = x

Examples

Addition

0

Multiplication

1

String concatenation

""
Overload 3 ⭐⭐⭐⭐⭐

This is the one interviewers actually use.

Collectors.reducing(
identity,
mapper,
BinaryOperator
)

Signature

Collector<T, ?, U>

Notice something new.

Input

T

Output

U

because we first map.

Example

Employees

John 50000

Alice 70000

Mike 90000

Total salary

Integer total =
employees.stream()
.collect(Collectors.reducing(
0,
Employee::getSalary,
Integer::sum
));

Flow

Employee

↓

Salary

↓

50000

70000

90000

↓

sum

↓

210000
Real Interview Example

Department salary totals

Map<String,Integer> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.reducing(
0,
Employee::getSalary,
Integer::sum
)
));

Output

IT

210000

HR

115000
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------







1. Collectors.averagingInt()

This is the easiest one because you already know summarizingInt().

Purpose

Calculate the average of an int property.

Signature

Only one overload.

public static <T>
Collector<T, ?, Double> averagingInt(
ToIntFunction<? super T> mapper)

Notice the return type:

Double
Example
Double avgSalary =
employees.stream()
.collect(Collectors.averagingInt(
Employee::getSalary
));

Output

67500.0
With groupingBy()

Very common interview question.

Map<String, Double> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.averagingInt(
Employee::getSalary
)
));

Output

IT -> 70000.0

HR -> 57500.0
Related Collectors

Exactly like summarizing.

Collectors.averagingInt(...)
Collectors.averagingLong(...)
Collectors.averagingDouble(...)
Internal Working

Conceptually

sum += salary;

count++;

average = sum / count;
Interview Pattern

Whenever you hear

Average salary
Average marks
Average age

Think

Collectors.averagingInt(...)
2. Collectors.collectingAndThen()

This is probably the most elegant collector in the API.

Why does it exist?

Suppose

List<String> names =
employees.stream()
.collect(Collectors.toList());

Now you want

Collections.unmodifiableList(names);

Instead of doing two separate steps, Java lets you do it in one collector.

Mental Model
Collect first

↓

Then perform one final operation

That's literally what the name means:

collecting

AND THEN

do something
Signature

Only one overload.

public static <T, A, R, RR>
Collector<T, A, RR> collectingAndThen(
Collector<T, A, R> downstream,
Function<R, RR> finisher)

Think of it as:

Collect

↓

Result R

↓

Apply Function<R, RR>

↓

Final Result RR
Example 1

Without it

List<String> list =
names.stream()
.collect(Collectors.toList());

List<String> immutable =
Collections.unmodifiableList(list);

With it

List<String> immutable =
names.stream()
.collect(Collectors.collectingAndThen(
Collectors.toList(),
Collections::unmodifiableList
));
Example 2 (Very Important)

Remember

Collectors.maxBy(...)

returns

Optional<Employee>

Suppose interviewer wants

Map<String, Employee>

not

Map<String, Optional<Employee>>

Solution

Map<String, Employee> map =
employees.stream()
.collect(Collectors.groupingBy(
Employee::getDepartment,
Collectors.collectingAndThen(
Collectors.maxBy(
Comparator.comparing(Employee::getSalary)
),
Optional::get
)
));

Flow

Employees

↓

maxBy()

↓

Optional<Employee>

↓

Optional::get

↓

Employee

Final type

Map<String, Employee>

instead of

Map<String, Optional<Employee>>

This is a classic interview example.

Example 3

Sort after collecting.

List<String> sorted =
names.stream()
.collect(Collectors.collectingAndThen(
Collectors.toList(),
list -> {
list.sort(String::compareTo);
return list;
}
));
Example 4

Convert List to Set after collecting.

Collectors.collectingAndThen(
Collectors.toList(),
HashSet::new
)
Mental Model

Whenever you see

Collectors.collectingAndThen(
collector,
function
)

Read it as

Run the collector.

Take its result.

Apply one final transformation.

Return the transformed result.