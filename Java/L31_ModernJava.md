Java 17 

Sealed Classes :

    A sealed class is a class that restricts which other classes can extend or implement it.
    It provides more control over the class hierarchy and allows for better encapsulation.

🔹 3. Why is it Needed? (Core Problem)
❌ Problem before sealed classes

Before Java 17:

    class Shape {}

👉 ANYONE could extend it:

    class Triangle extends Shape {}
    class RandomShape extends Shape {}

This causes:

🚨 Issues:

    No control over inheritance
    Hard to reason about all subclasses
    Unsafe for pattern matching (switch)

Breaks domain modeling

🔹 4. What Problem It Solves

    ✅ 1. Controlled Inheritance

    You explicitly define allowed subclasses.
    sealed class Shape permits Circle, Rectangle {}

👉 Now ONLY Circle and Rectangle allowed.

✅ 2. Better Domain Modeling

Example:

    sealed class Payment permits Cash, Card, UPI {}

👉 Real-world system has fixed types.

    No one can randomly create:
    class CryptoPayment extends Payment {} ❌ (not allowed)

✅ 3. Safer Pattern Matching (VERY IMPORTANT for interviews)

With sealed classes, compiler KNOWS all subclasses.

    static double area(Shape s) {
    return switch (s) {
    case Circle c -> Math.PI * c.radius * c.radius;
    case Rectangle r -> r.length * r.width;
    };
    }

👉 No default needed
👉 Compiler ensures ALL cases handled


🔹 5. Types of Subclasses

When extending a sealed class, subclass must be:

| Type         | Meaning                      |
| ------------ | ---------------------------- |
| `final`      | Cannot be extended further   |
| `sealed`     | Restricts further subclasses |
| `non-sealed` | Opens it for anyone          |



If a class extends or implements a sealed class/interface, it must explicitly declare one of these:

    final
    sealed
    non-sealed

👉 Sealed classes are compile-time enforcement, not runtime.


![img.png](../Images/SC1.png)

![img_1.png](../Images/SC2.png)

![img_2.png](../Images/SC3.png)

![img_3.png](../Images/SC4.png)

![img_4.png](../Images/SC5.png)

![img_5.png](../Images/SC6.png)

![img_6.png](../Images/Sc7.png)

---------------------------------------------------------------------------------------------------------------------

Java 16

Pattern Matching for instanceof (Preview in Java 16, Standard in Java 17)


👉 instanceof is an operator used to check:

        “Is this object an instance of a particular class (or interface)?”

say we have a class Parent and Class child extends Parent

    Parent p = new Child();
    System.out.println(p instanceof Parent); // true
    System.out.println(p instanceof Child);  // true
    System.out.println(p instanceof String); // false

    Parent p = new Parent();
    System.out.println(p instanceof Parent); // true
    System.out.println(p instanceof Child);  // false
    System.out.println(p instanceof Object); // true

    Child c = new Child();
    System.out.println(c instanceof Parent); // true
    System.out.println(c instanceof Child);  // true
    System.out.println(c instanceof Object); // true





Pattern Matching for instanceof allows you to both check the type and cast it in a single step, making code more concise and readable.
👉 It’s a feature (Java 16+) that lets you check type + cast + use the variable in one step.


    Before Java 16:

    if (obj instanceof String) {
        String s = (String) obj; // Manual cast
        System.out.println(s.length());
    }

    After Java 16:
    if (obj instanceof String s) { // Pattern variable 's' declared
        System.out.println(s.length()); // No cast needed
    }   

Benefits:

    ✅ 1. Concise Code: Eliminates boilerplate casting code.
    ✅ 2. Safer: Compiler ensures correct type handling.
    ✅ 3. Readable: Clear intent of type checking and usage.


![img_7.png](../Images/PMi1.png)

![img_8.png](../Images/PMi2.png)

![img_9.png](../Images/PMi3.png)

![img_10.png](../Images/PMi4.png)

![img_11.png](../Images/PMi5.png)

🔹 BUT ⚠️ Important Catch (with pattern matching)

When you use pattern matching, things change:

    if (obj instanceof String s || obj instanceof Integer i) {
    System.out.println(s); // ❌ Compile error
    }

Because it can be s or i but printing s is wrong if it’s actually an Integer.

👉 ❌ This gives compile error

-------------------------------------------------------------------------------------------------------------------------------------


Java 14 : Switch Expressions (Preview in Java 14, Standard in Java 15)


    👉 Switch expressions (introduced in Java 14) allow switch to return values, remove the need for break statements, support multiple case labels, enforce exhaustiveness, and eliminate fall-through bugs using arrow syntax. They also support yield for block cases.    


![img_12.png](../Images/sw1.png)

![img_13.png](../Images/sw2.png)

![img_14.png](../Images/sw3.png)

![img_15.png](../Images/sw4.png)

![img_16.png](../Images/sw5.png)

![img_17.png](../Images/sw6.png)

![img_18.png](../Images/sw7.png)

![img_19.png](../Images/sw8.png)

![img_20.png](../Images/sw9.png)

![img_21.png](../Images/sw10.png)

![img_22.png](../Images/sw11.png)


1. Removes the need for boilerplate case blocks if 2 or more cases does same thing
2. Removes the need for break statements
3. Allows switch to be used as an expression that returns a value
4. Supports multiple labels for a single case (e.g., case 1, 2, 3:)
5. Improves readability and reduces chances of fall-through bugs (forgetting break means say we have a enum and we forget for one case, it will give compile error) Only applies to switch expressions (arrow syntax)
6. Supports pattern matching (with instanceof) in switch cases (Java 17+)
7. Allows yield keyword to return values from cases in switch expressions


If we don’t write default in modern switch, what happens?

👉 It depends on whether the switch is exhaustive or not.

🔸 1. If switch is NOT exhaustive ❌
int x = 3;

int result = switch (x) {
case 1 -> 10;
case 2 -> 20;
};

👉 ❌ Compile-time error

“Switch expression does not cover all possible input values”

🔸 2. Why does this happen?

👉 Because switch expression MUST return a value for ALL inputs

So Java forces you to handle:

all cases OR
default
🔸 3. Fix using default ✅
int result = switch (x) {
case 1 -> 10;
case 2 -> 20;
default -> 0;
};

✔ Now safe
✔ Handles unknown values

🔸 4. When default is NOT needed ✅

👉 When compiler KNOWS all possibilities

Example: enum
enum Day { MON, TUE }

int result = switch (day) {
case MON -> 1;
case TUE -> 2;
};

✔ No default needed
👉 Because all enum constants are covered

🔸 5. With sealed classes (VERY IMPORTANT)
sealed interface Shape permits Circle, Rectangle {}

final class Circle implements Shape {}
final class Rectangle implements Shape {}
int x = switch (shape) {
case Circle c -> 1;
case Rectangle r -> 2;
};

✔ No default needed
👉 Compiler knows all subclasses

🔸 6. What if a different case comes at runtime?

👉 It cannot happen if compiler ensured exhaustiveness

That’s the key idea:

🔥 Modern switch shifts errors from runtime → compile time

🔹 Final Rule
Scenario	Need default?
int / String / Object	✅ YES
enum (all values covered)	❌ NO
sealed class (all types covered)	❌ NO
🔹 Interview One-Liner

👉 In switch expressions, default is required unless the switch is exhaustive (like with enums or sealed classes), otherwise it results in a compile-time error.

🔥 Key Insight (VERY IMPORTANT)

👉 Old switch:

Missing case → runtime bug

👉 New switch:

Missing case → compile-time error

If you want, I can give:
🔥 tricky questions combining sealed + switch exhaustiveness
⚔️ cases where compiler still forces default even when you think it's exhaustive

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


Java 21 Pattern Matching for switch (Preview in Java 21, Standard in Java 22)

    👉 Pattern Matching for switch (introduced in Java 21) extends pattern matching to switch statements, allowing you to match on types and deconstruct objects directly in switch cases. It supports record patterns, type patterns, and allows for more concise and powerful switch logic. It also enforces exhaustiveness, ensuring all cases are handled.

![img_23.png](../Images/sw12.png)

![img_24.png](../Images/sw13.png)

![img_25.png](../Images/sw14.png)

![img_26.png](../Images/sw15.png)

![img_27.png](../Images/sw16.png)

![img_28.png](../Images/sw17.png)

![img_29.png](../Images/sw18.png)

![img_30.png](../Images/sw19.png)

![img_31.png](../Images/sw20.png)

🔸 1. Basic Idea (MUST KNOW)

    👉 Switch can now match types, not just values.

    Object obj = "Hello";
    
    switch (obj) {
    case String s -> System.out.println(s.length());
    case Integer i -> System.out.println(i * 2);
    default -> System.out.println("Unknown");
    }

👉 This replaces:

    instanceof
    manual casting

🔸 2. Type Patterns (CORE CONCEPT)

    case String s ->

👉 Means:

    check type
    cast
    assign variable

✔ Same as:

    if (obj instanceof String s)
🔸 3. Null Handling ⚠️ (VERY IMPORTANT)

    Old switch → NPE
    New switch → allows null
    
    switch (obj) {
    case null -> System.out.println("Null value");
    case String s -> System.out.println(s);
    default -> {}
    }

    👉 If you don’t handle null → ❌ NPE

🔸 4. Dominance Rule ⚠️ (COMMON TRAP)

👉 Order matters!

    switch (obj) {
    case Object o -> {}     // ❌ too general
    case String s -> {}     // unreachable
    }

❌ Compile error

✔ Correct:

    case String s -> {}
    case Object o -> {}

🔸 5. Exhaustiveness (VERY IMPORTANT)

👉 Switch must cover all cases

    switch (obj) {
    case String s -> "text";
    }

❌ Compile error

✔ Must include:

    default -> ...

OR use sealed classes (next point 👇)

🔸 6. Sealed Classes + Switch (🔥 HOT TOPIC)

👉 BEST use case

    sealed interface Shape permits Circle, Rectangle {}
    
    final class Circle implements Shape {}
    final class Rectangle implements Shape {}
    static int area(Shape s) {
    return switch (s) {
    case Circle c -> 1;
    case Rectangle r -> 2;
    };
    }

👉 No default needed ✅
👉 Compiler knows all types

🔸 7. Guarded Patterns (Advanced ⚠️)
case String s && s.length() > 5 -> System.out.println("Long string");

    👉 Adds condition on top of type

🔸 8. No Fall-through (IMPORTANT)

    👉 Same as switch expressions

    case String s -> ...
    case Integer i -> ...

    ✔ No break needed
    ✔ No accidental fall-through

🔸 9. Scope of Variables
case String s -> System.out.println(s);

👉 s exists only inside that case

🔸 10. Comparison with instanceof
Feature	instanceof	switch pattern
Multiple types	❌ messy	✅ clean
Readability	❌	✅
Exhaustiveness	❌	✅



👉 “Old switch → NPE, New switch → allows null”

It means:

🔸 1. OLD behavior (before enhanced switch)

    String str = null;
    
    switch (str) {
    case "Hello":
    System.out.println("Hi");
    }

    👉 ❌ Immediately throws NullPointerException

✔ You had NO way to handle null inside switch

🔸 2. NEW behavior (Java 14+ enhanced switch)

    String str = null;
    
    switch (str) {
    case null -> System.out.println("Null value");
    case "Hello" -> System.out.println("Hi");
    default -> {}
    }

👉 ✅ Now Java allows case null

    ✔ You can explicitly handle null
    ✔ No exception

🔹 What this line means exactly

👉 “If you don’t handle null → NPE”

Example:

    String str = null;
    
    switch (str) {
    case "Hello" -> System.out.println("Hi");
    default -> System.out.println("Other");
    }

👉 ❌ Still throws NullPointerException

-----------------------------------------------------------------------------------------------------------------------------------------











































🔸 2. In Java (current level)
Example:
if (obj instanceof String s)

👉 Pattern = String s

This pattern says:

“Match any object that is of type String, and bind it to s”

🔸 3. In switch
switch (obj) {
case String s -> System.out.println(s);
}

👉 Again, String s is a pattern

we are checking any pattern of obj type String matches so pattern matching 


🔹 What does “non-sealed breaks exhaustiveness” mean?

👉 It means:

If any subclass in a sealed hierarchy is non-sealed, the compiler can no longer guarantee all possible types → so switch is NOT exhaustive anymore.

🔸 Step 1: Normal sealed (exhaustive ✅)
sealed interface Shape permits Circle, Rectangle {}

final class Circle implements Shape {}
final class Rectangle implements Shape {}
int area = switch (shape) {
case Circle c -> 1;
case Rectangle r -> 2;
};

👉 ✅ Works
👉 No default needed

✔ Because compiler knows:

Only Circle and Rectangle exist
🔸 Step 2: Introduce non-sealed ❌
sealed interface Shape permits Circle, Rectangle {}

non-sealed class Circle implements Shape {}
final class Rectangle implements Shape {}

👉 Now:

int area = switch (shape) {
case Circle c -> 1;
case Rectangle r -> 2;
};

👉 ❌ Compile-time error

🔹 Why does this happen?

Because:

👉 Circle is non-sealed

Which means:

class SpecialCircle extends Circle {}
class AnotherCircle extends Circle {}

👉 These are now possible!

🔥 So compiler thinks:

“Wait… there could be MORE types of Shape I don’t know about”

👉 So it loses exhaustiveness guarantee

🔹 Fix

You MUST add default:

int area = switch (shape) {
case Circle c -> 1;
case Rectangle r -> 2;
default -> 0;
};

✔ Now safe again