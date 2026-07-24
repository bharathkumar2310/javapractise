1. What is OOP?

👉 Programming style based on:

Objects
Classes
Encapsulation
Inheritance
Polymorphism
Abstraction
2. Why OOP?

👉 Benefits:

Reusability
Maintainability
Scalability
Security
Real-world modeling
🔥 2. ENCAPSULATION (VERY IMPORTANT)
3. What is encapsulation?

👉 Wrapping:

data + methods together

AND

👉 restricting direct access using:

private
4. Why use encapsulation?

👉 Protects internal state

Example:

class Employee {
private int salary;

    public void setSalary(int salary) {
        if(salary > 0)
            this.salary = salary;
    }
}
5. Encapsulation vs Abstraction
   Encapsulation	Abstraction
   Hides data	Hides implementation
   Achieved using private	Achieved using abstract/interface
6. Can encapsulation exist without abstraction?

👉 Yes

🧬 3. INHERITANCE
7. What is inheritance?

👉 Acquiring parent properties into child

class Animal {}
class Dog extends Animal {}
8. Why inheritance?

👉 Code reuse

9. Types of inheritance in Java

✅ Supported:

Single
Multilevel
Hierarchical

❌ Not supported using classes:

Multiple inheritance
10. Why multiple inheritance not supported?

👉 Diamond problem ambiguity

Example:

class A {
void show(){}
}

class B extends A {}
class C extends A {}
class D extends B, C {} // ambiguity
11. Can interfaces achieve multiple inheritance?

👉 Yes

Because:

interfaces don’t maintain state traditionally
ambiguity resolved explicitly
12. IS-A vs HAS-A
    IS-A
    Dog IS-A Animal
    HAS-A
    Car HAS-A Engine

👉 HAS-A = composition

13. Composition vs Inheritance (VERY IMPORTANT)

👉 Prefer composition when:

relationship can change
loose coupling needed

Interview favorite:

“Favor composition over inheritance”

⚡ 4. POLYMORPHISM
14. What is polymorphism?

👉 One thing behaving differently

15. Types of polymorphism
    Compile-time

👉 Method overloading

Runtime

👉 Method overriding

⚙️ METHOD OVERLOADING
16. What is overloading?

Same method name, different parameters

add(int a, int b)
add(double a, double b)
17. Can return type alone overload method?

👉 ❌ No

18. Can we overload static methods?

👉 ✅ Yes

19. Can constructors be overloaded?

👉 ✅ Yes

🔥 METHOD OVERRIDING
20. What is overriding?

Child provides new implementation

class Animal {
void sound(){}
}

class Dog extends Animal {
void sound(){}
}
21. Rules for overriding
    Same method signature
    IS-A relationship
    Cannot reduce visibility
22. Can private methods be overridden?

👉 ❌ No

23. Can static methods be overridden?

👉 ❌ No → method hiding

24. Can final methods be overridden?

👉 ❌ No

25. Why @Override annotation useful?

👉 Compile-time checking

🧠 5. ABSTRACTION
26. What is abstraction?

👉 Hiding implementation details

27. Abstract class vs Interface
    Abstract Class	Interface
    Can have constructors	Cannot
    Can have state	Mostly constants
    Single inheritance	Multiple inheritance
28. When use abstract class?

👉 Common base behavior/state

29. When use interface?

👉 Contract/common capability

30. Can abstract class have constructor?

👉 ✅ Yes

Used for parent initialization

31. Can interface have methods with body?

👉 ✅ Default/static methods (Java 8)

32. Why default methods introduced?

👉 Backward compatibility

⚡ 6. IMPORTANT KEYWORDS
33. this keyword

👉 Current object reference

34. super keyword

👉 Parent reference

35. Difference:
    this()
    super()
    this()	super()
    same class constructor	parent constructor
36. Can both exist together?

👉 ❌ No
Must be first statement

🧪 7. OUTPUT/TRICKY QUESTIONS
37.
class Parent {
static void show() {
System.out.println("Parent");
}
}

class Child extends Parent {
static void show() {
System.out.println("Child");
}
}

Parent p = new Child();
p.show();

👉 Output:

Parent

👉 Because static methods are resolved at compile time

38.
class Parent {
void show() {
System.out.println("Parent");
}
}

class Child extends Parent {
void show() {
System.out.println("Child");
}
}

Parent p = new Child();
p.show();

👉 Output:

Child

👉 Runtime polymorphism

39.
class Test {
Test() {
this(10);
System.out.println("A");
}

Test(int x) {
System.out.println("B");
}
}

👉 Output:

B
A
💻 8. CODING/DESIGN QUESTIONS
40. Design:
    Employee Management System

Use:

inheritance
abstraction
encapsulation
41. Design:
    Payment System
    UPI
    Card
    NetBanking

👉 Use polymorphism

42. Why composition preferred over inheritance?

👉 Loose coupling

43. Design a notification system
    Email
    SMS
    Push

👉 Interface-based design

⚠️ 9. VERY COMMON INTERVIEW TRAPS
44. Constructor overriding possible?

👉 ❌ No

45. Constructor inheritance possible?

👉 ❌ No

46. Can abstract class be final?

👉 ❌ Contradiction

47. Can interface be instantiated?

👉 ❌ No

48. Can object be created for abstract class?

👉 ❌ No

49. Can interface extend interface?

👉 ✅ Yes

50. Can class extend multiple interfaces?

👉 ✅ Yes

🔥 10. HIGH-IMPACT INTERVIEW QUESTIONS

Very high probability:

51. Explain runtime polymorphism with memory perspective
52. Why Java doesn’t support multiple inheritance?
53. Difference between abstraction and encapsulation
54. Why prefer interface over abstract class sometimes?
55. Explain SOLID using OOP concepts

(We’ll cover deeply later)