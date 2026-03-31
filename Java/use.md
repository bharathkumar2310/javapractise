
## 💡 What is **OOP (Object-Oriented Programming)**?

**OOP** — or **Object-Oriented Programming** — is a programming paradigm (a style of structuring code) based on the concept of **objects**, which represent **real-world entities**.  
Each object has:

- **Data (attributes or fields)**
- **Behavior (methods or functions)**
### 🎯 Goal of OOP:

To make software **modular**, **reusable**, **scalable**, and **easier to maintain** by modeling real-world systems.
**Modular** means **dividing a big program into small, independent, reusable pieces called _modules_**.`

```
class Car {
    // attributes
    String color;
    int speed;

    // behavior
    void drive() {
        System.out.println("Car is driving");
    }
}

public class Main {
    public static void main(String[] args) {
        Car car1 = new Car(); // object creation
        car1.color = "Red";
        car1.speed = 120;
        car1.drive(); // calling method
    }
}

```


## 🔄 What is **Procedural Programming**?

**Procedural programming** is a paradigm where programs are built using **functions (procedures)** and **data structures** separately.  
It focuses on **what to do** and **how to do it** step-by-step.

```
#include <stdio.h>

int speed = 120;

void drive() {
    printf("Car is driving at %d km/h", speed);
}

int main() {
    drive();
    return 0;
}

```

Here:

- The code focuses on **functions (procedures)**, not on **objects**.
- Data (`speed`) and behavior (`drive`) are **separate**.


| Feature                 | **Procedural Programming**                | **Object-Oriented Programming (OOP)**                |
| ----------------------- | ----------------------------------------- | ---------------------------------------------------- |
| **Basic Unit**          | Function or Procedure                     | Object (Instance of Class)                           |
| **Focus**               | Focuses on actions (functions)            | Focuses on data (objects)                            |
| **Data Handling**       | Data and functions are separate           | Data and functions are encapsulated together         |
| **Data Access**         | Any function can access data              | Access controlled via encapsulation (private/public) |
| **Reusability**         | Code reuse via functions                  | Code reuse via classes and inheritance               |
| **Security**            | Less secure (data easily accessible)      | More secure (data hiding via access modifiers)       |
| **Examples**            | C, Pascal                                 | Java, C++, Python, C#                                |
| **Extensibility**       | Hard to extend and maintain large systems | Easy to extend using inheritance and polymorphism    |
| **Real-World Modeling** | Poor mapping to real-world entities       | Directly models real-world entities as objects       |



---------------------------------------------------------------------------------------

## 💡 What is a **Class**?

A **class** is a **blueprint or template** for creating objects.  
It defines **how an object should look** (its data/attributes) and **how it should behave** (its methods).


## 🚗 What is an **Object**?

An **object** is a **real-world instance** of a class.  
It is created from the class blueprint and represents **a specific entity** with actual data.

👉 Think of it like **building a real car** from the “Car” blueprint.


--------------------------------------------------------------------------------

## 💡 What is **Abstraction**?

**Abstraction** means **hiding the complex internal details** and **showing only the essential features** of an object.
It focuses on **what an object does**, not **how it does it**.


|Purpose|Description|
|---|---|
|✅ **Hide complexity**|Users don’t need to know how something works internally.|
|✅ **Increase security**|Internal logic is hidden from outside interference.|
|✅ **Improve modularity**|Code becomes cleaner and easier to maintain.|
|✅ **Enhance flexibility**|You can change the implementation without affecting users.|
Security : By exposing **only the necessary methods** and hiding the rest, users can’t accidentally call dangerous internal methods

## 🌍 **Real-World Example: ATM Machine**

When you go to an ATM and press **“Withdraw ₹1000”**,  
you don’t care how the ATM:

- Connects to the bank server
- Verifies your PIN
- Updates your account balance
- Dispenses cash

You only see **simple buttons (Withdraw, Check Balance, Deposit)** —  
the **complex logic is hidden** from you.

That’s **Abstraction**.
✅ **You only see:** What operations are available.


## 1️⃣ **Spring Data JPA Repositories**

```
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name);
}

```

**What’s happening:**
- You only **declare an interface** (`UserRepository`) — this is **abstraction**.

- You **don’t write any SQL or implementation code**.

- Spring provides the **actual implementation at runtime**.


✅ **Abstraction point:** You interact only with the interface; Spring hides the implementation of CRUD

#### There are **two main ways** to achieve abstraction in Java: **abstract classes** and **interfaces**.



--------------------------------------------------------------------------------------

## 💡 **What is Encapsulation?**

**Encapsulation** is the concept of **wrapping data (variables) and methods (functions) that operate on that data into a single unit (class)** and **restricting direct access to some of the object’s components**.

- Typically, **variables are `private`**
- Access is provided through **public methods** (`getters` and `setters`)
-
> Think of it as putting a **protective shield around your data** so only controlled access is allowed.


| Use                      | Explanation                                          |
| ------------------------ | ---------------------------------------------------- |
| **Data Hiding**          | Protects internal state from unintended modification |
| **Improves Security**    | Sensitive data can’t be accessed directly            |
| **Code Maintainability** | Changing internal logic doesn’t affect external code |
| **Controlled Access**    | Validations can be applied before changing data      |
| **Modularity**           | Each class is a self-contained unit                  |


## 🌍 **Real-World Examples**

### 1️⃣ Bank Account

- You can **deposit or withdraw** money, but you **cannot directly change the balance**.

```
class BankAccount {
    private double balance; // hidden data

    public BankAccount(double initialBalance) {
        balance = initialBalance;
    }

    public void deposit(double amount) {
        if(amount > 0) balance += amount;
    }

    public void withdraw(double amount) {
        if(amount > 0 && amount <= balance) balance -= amount;
    }

    public double getBalance() {
        return balance;
    }
}

```

**Why encapsulation:**

- `balance` is **private** → can’t be changed directly
- All operations go through **controlled methods** → safe access


Encapsulation is achieved by:

1. **Making class variables private**
2. **Providing public getter and setter methods** to access or modify the private variables


------------------------------------------------------------------------------------

## 💡 **What is Inheritance?**

**Inheritance** is an **OOP concept** where a **class (child/subclass)** can **inherit fields and methods from another class (parent/superclass)**.

- Promotes **code reusability**
- Supports **hierarchical relationships**
- Enables **polymorphism** (a subclass can be treated as its parent type)


|Type|Description|Example|
|---|---|---|
|**Single Inheritance**|Child inherits from a single parent|`Child extends Parent`|
|**Multilevel Inheritance**|Chain of inheritance|`GrandChild extends Child extends Parent`|
|**Hierarchical Inheritance**|Multiple children inherit from a single parent|`Child1 extends Parent`, `Child2 extends Parent`|
|**Multiple Inheritance** (Not directly in Java)|Child inherits from multiple parents (use interfaces)|`class Child implements Interface1, Interface2`|



|Advantage|Description|
|---|---|
|**Code Reusability**|No need to write common fields/methods again|
|**Method Overriding / Polymorphism**|Child can provide its own implementation|
|**Logical Hierarchy**|Represents real-world relationships|
|**Extensibility**|Easily extend functionality without changing existing code|


**Animals / Biology Example**

- **Parent:** Animal
    - Common features: eat(), sleep(), breathe()
- **Child:** Dog, Cat, Bird
    - Dog → bark()
    - Cat → meow()
    - Bird → fly()


**Why inheritance?**

- All animals share basic features → **reusability**
- Each species has **specialized features** → **extension**


## 2️⃣ **Why is Multiple Inheritance Not Allowed?**
### A. **Damond Problem**
- Consider this scenario:
  `A    / \   B   C    \ /     D`

- **B and C inherit from A**, D inherits from **both B and C**

- **Problem:** If `A` has a method `show()`, and both `B` and `C` override it, **which version should D inherit?**
```
class A { void show() { System.out.println("A"); } }
class B extends A { void show() { System.out.println("B"); } }
class C extends A { void show() { System.out.println("C"); } }
// class D extends B, C {} // ❌ Conflict!

```



- **Ambiguity arises** → compiler doesn’t know whether to call `B.show()` or `C.show()`

- This is called the **diamond problem**.


## 2️⃣ **Interfaces in Java Solve Diamond Problem**

- Interfaces **do not have method implementations** (before Java 8)
- Even with **default methods (Java 8+)**, **ambiguity is resolved explicitly**
  In default method compiler will force u to override




## 1️⃣ **Basic Rules**

When you create an object of a **child class**:

```
class Parent {
    Parent() { System.out.println("Parent constructor"); }
}

class Child extends Parent {
    Child() { System.out.println("Child constructor"); }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child();
    }
}

```

```
Parent constructor
Child constructor

```


**What happens internally:**

1. **Memory allocation**: Java allocates memory for the **entire object**, including **Parent part** + **Child part**.
2. **Parent constructor call**: The **Parent constructor executes first** (even if not explicitly called) using `super()`.
3. **Child constructor call**: Then the **Child constructor executes**.

> **Rule:** Parent constructor always runs **before** child constructor.



```
Parent p = new Child();

```
Only methods available in Parent can be accessed (unless overridden)

Object contains both parent and child parts internally

Used in polymorphism

```
p.display();   // calls overridden method if present in child
// p.childMethod(); // ❌ not allowed

```

--------------------------------------------------------------------------------



## 💡 **What is Polymorphism?**

**Polymorphism** means **“many forms”**.

In OOP:

> A single entity (method, object, or operator) can behave differently in different contexts.

- It allows **one interface to be used for multiple forms of behavior**.

- Polymorphism is tightly connected with **inheritance** and **interfaces**.




![[Pasted image 20251110095802.png]]


|Purpose|Explanation|
|---|---|
|**Code Reusability**|Same method name works for different types of objects|
|**Flexibility / Extensibility**|Add new functionality without changing existing code|
|**Maintainability**|Reduce multiple if-else or switch cases|
|**Dynamic Behavior**|Runtime decisions can be made automatically based on object type|

### **A. Compile-Time Polymorphism (Static)**

- Achieved by **method overloading** or **operator overloading**

- Determined **at compile time**


**Example: Method Overloading**

```
class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
}

Calculator calc = new Calculator();
System.out.println(calc.add(2, 3));       // calls int version
System.out.println(calc.add(2.5, 3.5));   // calls double version

```

✅ Same method name (`add`) behaves differently based on **parameter type**.

### **Rule 1: Must have different parameters**

- You can overload a method if the **number of parameters** or **types of parameters** differ.
```
    class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
}

```


✅ Valid: parameter list is different (int vs double)

---

### **Rule 2: Return type alone cannot overload a method**

- If two methods have **same name and parameters** but different return types → ❌ NOT allowed.

```
int add(int a, int b) { return a + b; }
double add(int a, int b) { return a + b; } // ❌ Compile-time error

```

The **compiler** decides **which function to call** _before_ the program runs.


---

### **B. Run-Time Polymorphism (Dynamic)**

- Achieved by **method overriding**
- Determined **at runtime** based on actual object type
- Often used with **upcasting**


**Example:**

```
class Animal {
    void sound() { System.out.println("Animal makes sound"); }
}

class Dog extends Animal {
    void sound() { System.out.println("Dog barks"); }
}

class Cat extends Animal {
    void sound() { System.out.println("Cat meows"); }
}

public class Main {
    public static void main(String[] args) {
        Animal a1 = new Dog();
        Animal a2 = new Cat();

        a1.sound();  // Dog barks
        a2.sound();  // Cat meows
    }
}

```

✅ Same method call (`sound()`) behaves differently **depending on object type**.


## 1️⃣ **What’s happening here?**

### **Step 1: Upcasting**

- `a1` is of type `Animal` (parent class)
- `new Dog()` creates a **Dog object** (child class)
- **Assigning child object to parent reference** is called **upcasting**

✅ Same applies for `a2 = new Cat()`

---

### **Step 2: Memory Layout**

- Even though the reference is of type `Animal`, the **actual object is Dog**.
- Memory allocated contains both **Animal fields** + **Dog fields** (if any).


---

### **Step 3: Method Calls (Dynamic Polymorphism)**

`a1.sound(); // Calls Dog’s overridden sound() 
 a2.sound(); // Calls Cat’s overridden sound()`

- Java determines at **runtime** which `sound()` method to call → **runtime polymorphism**.

- **Rule:** The **object’s actual type** decides the method execution, not the reference type.


---

### **Step 4: Access Limitation**

- Only **methods/fields in the parent (Animal) are directly accessible** through `a1` or `a2`
- Child-specific methods cannot be called directly:


`a1.sound();      // ✅ works a1.bark();       // ❌ compile-time error, Animal doesn’t have bark()`

- To call child-specific methods, you need **downcasting**:


`((Dog)a1).bark(); // ✅ cast to Dog first`

---

## 2️⃣ **Why is this Useful?**

- Lets you **write generic code for all animals**:


```
Animal[] animals = { new Dog(), new Cat(), new Lion() };
for(Animal a : animals) {
    a.sound(); // calls the correct sound() for each type
}

```

- You don’t need **if-else for each animal type** → **polymorphism simplifies code**.
-------------------------------------------------------------------------------

# **1️⃣ IS-A Relationship**

### **Definition**

> **IS-A relationship** represents **inheritance**.  
> It means **one class is a type of another class**.
- Formed using **`extends`** (for classes) or **`implements`** (for interfaces).
- Shows **hierarchical relationship**.

```
class Animal {
    void eat() { System.out.println("Animal eats"); }
}

class Dog extends Animal {
    void bark() { System.out.println("Dog barks"); }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat();  // inherited from Animal
        d.bark(); // own method
    }
}

```

Here, **Dog IS-A Animal**.


# **HAS-A Relationship**

### **Definition**

> **HAS-A relationship** represents **composition or aggregation**.  
> It means **one class contains another class as a field**.

- Shows **part-of relationship**.
- Formed by **including objects of other classes as fields**.

```
class Engine {
    void start() { System.out.println("Engine starts"); }
}

class Car {
    private Engine engine; // Car HAS-A Engine

    Car() {
        engine = new Engine(); // composition
    }

    void startCar() {
        engine.start();
        System.out.println("Car starts");
    }
}

public class Main {
    public static void main(String[] args) {
        Car c = new Car();
        c.startCar();
    }
}

```




![[Pasted image 20251110095955.png]]






## 2️⃣ Aggregation Example — _Weak “has-a”_

👉 Relationship: **Student has-a Department**

Even if the student is deleted, the department can still exist independently.

### ✅ Example
```
class Department {
    String name;

    Department(String name) {
        this.name = name;
    }
}

class Student {
    String name;
    Department department; // Aggregation

    Student(String name, Department department) {
        this.name = name;
        this.department = department;
    }

    void displayInfo() {
        System.out.println(name + " belongs to " + department.name + " department");
    }
}

public class AggregationExample {
    public static void main(String[] args) {
        Department cs = new Department("Computer Science");
        Student s1 = new Student("Bharath", cs);

        s1.displayInfo();

        // Even if s1 is null, Department still exists
        s1 = null;
        System.out.println("Department still exists: " + cs.name);
    }
}

```

### 🧾 Output:

`Bharath belongs to Computer Science department Department still exists: Computer Science`

✅ `Department` is independent of `Student`  
That’s **aggregation**.

---

## 🧱 3️⃣ Composition Example — _Strong “has-a”_

👉 Relationship: **Car has-a Engine**

If the car is destroyed, its engine **must** be destroyed too — it cannot exist without the car.

### ✅ Example

```
class Engine {
    void start() {
        System.out.println("Engine started...");
    }
}

class Car {
    private Engine engine; // Composition — Car owns Engine

    Car() {
        engine = new Engine(); // Engine is created when Car is created
    }

    void startCar() {
        engine.start();
        System.out.println("Car started...");
    }
}

public class CompositionExample {
    public static void main(String[] args) {
        Car car = new Car();
        car.startCar();
        // When car is destroyed, its engine will also be gone (no independent existence)
    }
}

```
### 🧾 Output:

`Engine started... Car started...`

✅ The `Engine` object only exists **inside** the `Car`.  
No car = no engine.  
That’s **composition**.




-----------------------------------------------------------------------------------------

## **1️⃣ What is Coupling?**

### 🔹 **Definition:**

**Coupling** is the **degree of dependency between two classes or modules.**  
It tells you **how strongly one class is connected to another**.

- **Tightly coupled** → One class is heavily dependent on another.

- **Loosely coupled** → Classes are independent; they communicate through interfaces or abstractions.


---

### ✅ **Example:**

#### ❌ **Tight Coupling**

```
class Engine {
    void start() { System.out.println("Engine starting..."); }
}

class Car {
    Engine engine = new Engine();  // tightly bound to specific Engine
    void startCar() {
        engine.start();
    }
}

```

- If we replace `Engine` with `ElectricEngine`, we must modify `Car` class.

- Not flexible — violates _Open/Closed Principle_.


---

#### ✅ **Loose Coupling**

```
interface Engine {
    void start();
}

class PetrolEngine implements Engine {
    public void start() { System.out.println("Petrol engine starting..."); }
}

class Car {
    private Engine engine;
    Car(Engine engine) { this.engine = engine; } // injected dependency
    void startCar() { engine.start(); }
}

public class Test {
    public static void main(String[] args) {
        Engine e = new PetrolEngine();
        Car car = new Car(e); // flexible dependency
        car.startCar();
    }
}

```
- Now `Car` depends only on **interface**, not implementation.

- You can swap in `DieselEngine`, `ElectricEngine`, etc.  
  ✅ **Loose coupling → better flexibility and testability.**


---

### 🧠 **Interview Answer (Short Form):**

> “Coupling refers to how dependent classes are on each other.  
> We aim for **low coupling** to make the code flexible, maintainable, and easier to test.”

---

## 🧱 **2️⃣ What is Cohesion?**

### 🔹 **Definition:**

**Cohesion** refers to **how closely related and focused the responsibilities of a class or module are.**

- **High cohesion** → Class does one specific task well.

- **Low cohesion** → Class does too many unrelated things.


---

### ✅ **Example:**

#### ❌ **Low Cohesion**

```
class Utility {
    void readFile() {}
    void connectDatabase() {}
    void sendEmail() {}
}

```
- Unrelated responsibilities all inside one class.

- Hard to maintain or test.


---

#### ✅ **High Cohesion**

```
class FileManager {
    void readFile() {}
}

class DatabaseManager {
    void connectDatabase() {}
}

class EmailService {
    void sendEmail() {}
}

```
- Each class focuses on **one job** → high cohesion.














---
# ACCESSMODIFIERS



#### SUPER AND THIS








---------------------------------------------------------------------------------------


### **25. What is dynamic method dispatch?**

Mechanism by which a call to an overridden method is resolved at runtime.

### **20. What is composition and aggregation?**

- **Aggregation** – “Has-a” relationship, weaker association (object can exist independently).  
  _Example:_ Department has Employees.

- **Composition** – Strong “Has-a” relationship (object cannot exist without the other).  
  _Example:_ Human has Heart.


### **21. What is coupling and cohesion?**

- **Coupling** – Degree of interdependence between modules. (Low is better)

- **Cohesion** – How closely related the functions within a class are. (High is better)

### **23. What is the difference between shallow copy and deep copy?**

- **Shallow copy** → Copies object reference only.

- **Deep copy** → Copies actual data (creates new instance).


|Modifier|Scope|
|---|---|
|`private`|Within same class|
|_default_|Within same package|
|`protected`|Same package + subclasses|
|`public`|Everywhere|



## 🧩 **1️⃣ When would you prefer Composition over Inheritance?**

### 🔹 **Definition Recap:**

- **Inheritance** → “_is-a_” relationship  
  (e.g., `Dog` _is a_ `Animal`)

- **Composition** → “_has-a_” relationship  
  (e.g., `Car` _has an_ `Engine`)


---

### 💡 **Why choose Composition over Inheritance**

|Aspect|Inheritance|Composition|
|---|---|---|
|Relationship|“is-a”|“has-a”|
|Reuse|Reuses behavior by extending a class|Reuses behavior by including an object|
|Flexibility|Tightly coupled (change in parent affects child)|Loosely coupled (you can swap parts)|
|Runtime changes|Fixed at compile-time|Can change behavior at runtime|
|Example|`class Dog extends Animal`|`class Car { Engine engine; }`|



## 🧠 **2️⃣ What is `instanceof` in Java?**

### 🔹 **Definition:**

`instanceof` is a **keyword** used to test **whether an object is an instance of a specific class or subclass**.

### ✅ **Example:**

```
class Animal {}
class Dog extends Animal {}

public class Test {
    public static void main(String[] args) {
        Dog d = new Dog();

        System.out.println(d instanceof Dog);     // true
        System.out.println(d instanceof Animal);  // true
        System.out.println(d instanceof Object);  // true
        System.out.println(d instanceof String);  // false
    }
}

```


## 🧩 **1️⃣ `this` Keyword in Java**

### 🔹 **Definition:**

`this` is a **reference variable** that refers to the **current object** of the class — the object on which the method or constructor was called.

## 🧱 **2️⃣ `super` Keyword in Java**

### 🔹 **Definition:**

`super` is used to **refer to the immediate parent class** (superclass) members — variables, methods, and constructors.






A **POJO** stands for **Plain Old Java Object**.  
It’s a simple Java object that **doesn’t depend on any special framework, library, or external restrictions** — just a plain class that represents data or behavior.


### 🧠 **Purpose of POJOs**

- To **represent data** (e.g., `User`, `Employee`, `Product`).
- To keep your **application decoupled** from frameworks.
- To make code **clean, readable, and testable**.


![[Pasted image 20251012222542.png]]



## 🧠 1. What is an `enum`?

`enum` (short for **enumeration**) is a **special Java type** that represents a **fixed set of constants**.

Think of it as a **type-safe way** to define a group of related values.

For example: directions, days of the week, states, colors, etc.

---

### ✅ Example

`public enum Day {     MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY }`

- Here, `Day` is an **enum type**.

- `MONDAY, TUESDAY, ...` are the **constants**.

- You cannot create any value outside these constants.

## Features of `enum`

1. **Type-safe**  
   You cannot assign an invalid value:

   `Day today = "FUNDAY"; // ❌ Compile-time error`

2. **Enum constants are objects**  
   Each constant is actually an **instance of the enum class**.

3. **Can have fields, constructors, and methods**


✅ Every enum constant is a **final, singleton, immutable instance**.


![[Pasted image 20251012222621.png]]


![[Pasted image 20251012223427.png]]


public enum ---> should be always public or default so that can e accessed from diff class

constructor should always be private ----> so that we cannot mannually create obj

Example:


```
enum Color {
    RED, GREEN, BLUE;
}

```


Internally this is like


```
public final class Color extends Enum<Color> {

    // 1. Public static final objects for each constant
    public static final Color RED = new Color("RED", 0);
    public static final Color GREEN = new Color("GREEN", 1);
    public static final Color BLUE = new Color("BLUE", 2);

    // 2. Private array holding all constants (used by values())
    private static final Color[] VALUES = { RED, GREEN, BLUE };

    // 3. Private constructor
    private Color(String name, int ordinal) {
        super(name, ordinal); // calls Enum constructor
    }

    // 4. values() method to get all constants
    public static Color[] values() {
        return VALUES.clone(); // returns a copy of constants array
    }

    // 5. valueOf() method to get constant by name
    public static Color valueOf(String name) {
        for (Color c : VALUES) {
            if (c.name().equals(name)) return c;
        }
        throw new IllegalArgumentException("No enum constant " + name);
    }
}

```


Every colour RED , GREEN, BLUE is an instance of Color

Every enum by default will have an ordinal that is value 0,1,2 in the above example
Java converts this enum into a **final class** that extends `java.lang.Enum`.

![[Pasted image 20251012224659.png]]


![[Pasted image 20251012230115.png]]




Enum with Custom Values

```
public enum EnumSample{  

		MONDAY( 101 ,  "1st day of the week"),  
		
		TUESDAY(102 , "2nd day of the week"),  
		
		WEDNESDAY(103 , "3rd days of the week"),  
		
		THURSDAY(104 , "4th day of the week"),  
		
		FRIDAY(105 , "5th day of the week"),  
		
		SATURDAY(106 , "its 1st WeekOff"),  
		
		SUNDAY(107 , "its 2nd WeekOff");  
		
		private int val;  
		
		private String comment;  
		
		EnumSample(int val, String comment){  
		
			this.val  = val;  
			
			this.comment = comment;  
		
		}  
		
		public int getVal(){  
		
			return val;  
		
		}  
		
		public String getComment(){  
		
			return Comment;  
		
		}  
		
		public static EnumSample getEnumFromValue(int Value){  
			
			for(EnumSample sample : EnumSample.Values() ){  
			
				if(sample.val==value){  
				
					return sample;  
				
				}  
			
			}  
		
		return null;  
		
		}  

}
```



![[Pasted image 20251012230811.png]]

![[Pasted image 20251012230820.png]]


✅ Each constant is **still an object of `Operation`**, but internally it may be a **compiler-generated anonymous subclass** if you override methods.



![[Pasted image 20251012230958.png]]




![[Pasted image 20251012231011.png]]

![[Pasted image 20251012231024.png]]


![[Pasted image 20251012231035.png]]

![[Pasted image 20251012231050.png]]


![[Pasted image 20251012231138.png]]


![[Pasted image 20251012231150.png]]


![[Pasted image 20251012231159.png]]





## 🧩 2️⃣ The Hidden Constructor Parameters

Every enum constant automatically gets two **hidden parameters**:

|Hidden Field|Description|
|---|---|
|`name`|the name of the constant (`"NORTH"`)|
|`ordinal`|the order in which it’s declared (0 for first, 1 for second, etc.)|

So when the compiler creates:

`Direction.NORTH`

It actually becomes:

`new Direction("NORTH", 0);`

---

## 🧠 3️⃣ What If You Add Your Own Constructor?

When you do this:

```
public enum Direction {
    NORTH("Up"), SOUTH("Down");
    private final String desc;

    Direction(String desc) { this.desc = desc; }
}

```

Then **your constructor** is added _on top of_ the hidden one.  
But Java still calls `super(name, ordinal)` internally before your constructor runs.

So effectively:

```
private Direction(String name, int ordinal, String desc) {
    super(name, ordinal);
    this.desc = desc;
}

```
---

## 🔍 4️⃣ You Can’t Call the Constructor Yourself

Because:

- Enum constructors are **always private** (or package-private).

- You can’t do `new Direction()` — only the JVM creates them.

- The compiler instantiates them exactly once at class loading time.


That’s why all enum instances are **singletons**.



-------------------------------------------------------------------------------------


## 🧩 1️⃣ `System.out.println(d)` Calls `toString()`

When you print any object in Java:

`System.out.println(d);`

it actually calls:

`System.out.println(d.toString());`

---

## ⚙️ 2️⃣ `Enum` Class Defines `toString()`

All enums implicitly extend `java.lang.Enum`, and that class defines:

`public abstract class Enum<E extends Enum<E>> implements Comparable<E>, Serializable {     private final String name;     private final int ordinal;      public final String name() {         return name;     }      public String toString() {         return name;     } }`

So by default,  
👉 `toString()` just returns the `name` of the enum constant — which is `"NORTH"`, `"SOUTH"`, etc.

---

## ✅ 3️⃣ So These Are Equivalent

`System.out.println(d);          // Calls toString() → "NORTH" System.out.println(d.toString()); // Same → "NORTH" System.out.println(d.name());     // Same → "NORTH"`

All three print the same thing **by default**.

---

## ⚙️ 4️⃣ You Can Override `toString()` If You Want Custom Output

Example:
```
public enum Direction {
    NORTH("Upwards"), SOUTH("Downwards"), EAST("Rightwards"), WEST("Leftwards");

    private final String desc;

    Direction(String desc) {
        this.desc = desc;
    }

    @Override
    public String toString() {
        return desc;
    }
}

```


Now:

`System.out.println(Direction.NORTH); // Upwards System.out.println(Direction.NORTH.name()); // NORTH System.out.println(Direction.NORTH.toString()); // Upwards`

✅ You can see the difference now:

- `.name()` → always gives the exact identifier name (`"NORTH"`)

- `.toString()` → can be customized (default = same as `name()`)


---

## 🧠 5️⃣ Internal Flow (When You Call `valueOf()` and Print)

Here’s what happens line-by-line:

`Direction d = Direction.valueOf("NORTH");`

1. The compiler calls the auto-generated static method:

```
    public static Direction valueOf(String name) {
    return Enum.valueOf(Direction.class, name);
}

```

2. `Enum.valueOf()` looks up `"NORTH"` in the internal constant map.

3. It returns the same singleton instance `Direction.NORTH`.

4. When you print `d`, it calls `toString()` → `"NORTH"`.





----------------------------------------------------------------------------------------


## 🚨 **19. What happens if you clone an Enum?**

You **can’t clone** an enum.  
The `clone()` method is overridden in `Enum` to throw `CloneNotSupportedException`.


🧭 16. Can Enums be used in collections like HashMap or Set? Yes — and very efficiently. Because enum constants are immutable and have a fast hashCode

```
enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY }

public class EnumMapExample {
    public static void main(String[] args) {
        EnumMap<Day, String> schedule = new EnumMap<>(Day.class);

        schedule.put(Day.MONDAY, "Start strong");
        schedule.put(Day.FRIDAY, "Finish work");

        for (Day d : schedule.keySet()) {
            System.out.println(d + " → " + schedule.get(d));
        }
    }
}

```







--------------------------------------------------------------------------------------



# 🧭 ENUM USAGE IN SPRING BOOT

---

## 1️⃣ **Representing Fixed States or Roles**

Enums are perfect for defining **constants** like:

- Order statuses

- User roles

- Payment methods

- Error codes


### Example – User Roles

`public enum Role {     ADMIN,     USER,     GUEST }`

### Usage in Entity:

```
@Entity
public class User {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @Enumerated(EnumType.STRING)  // Important: store name, not ordinal
    private Role role;
}

```

👉 Why `EnumType.STRING`?

- `EnumType.ORDINAL` saves numbers (0,1,2...) — breaks if order changes.

- `EnumType.STRING` saves actual names ("ADMIN", "USER") — safer and readable.


---

## 2️⃣ **Mapping Enum in JPA / Hibernate**

### Example – Order Status

`public enum OrderStatus {     NEW,     PROCESSING,     COMPLETED,     CANCELLED }`

### Entity:

```
@Entity
public class Order {
    @Id
    @GeneratedValue
    private Long id;

    @Enumerated(EnumType.STRING)
    private OrderStatus status;
}

```

### Repository:
```
@Repository
public interface OrderRepository extends JpaRepository<Order, Long> {
    List<Order> findByStatus(OrderStatus status);
}

```


### Usage:

`List<Order> completed = orderRepository.findByStatus(OrderStatus.COMPLETED);`

✅ Type-safe  
✅ No magic strings  
✅ IDE autocompletion

---

## 3️⃣ **Enums in REST APIs**

### Controller Example

```
@RestController
@RequestMapping("/orders")
public class OrderController {

    @GetMapping("/status/{status}")
    public String getOrdersByStatus(@PathVariable OrderStatus status) {
        return "Orders with status: " + status;
    }
}

```

If you hit:

`GET /orders/status/COMPLETED`

You’ll get:

`Orders with status: COMPLETED`

🧠 Spring automatically converts the path variable `"COMPLETED"` to the enum constant `OrderStatus.COMPLETED`.

---

## 4️⃣ **Enums in Request Body (JSON)**

### Example DTO

```
`public class PaymentRequest {
    private PaymentType type;
    private double amount;
}

public enum PaymentType {
    CREDIT_CARD, DEBIT_CARD, UPI
}

```

### Controller

```
@PostMapping("/pay")
public String pay(@RequestBody PaymentRequest request) {
    return "Paid using " + request.getType();
}

```

### Request JSON:

`{   "type": "UPI",   "amount": 1500 }`

✅ Automatically converted from JSON string `"UPI"` → `PaymentType.UPI`.





![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s4827bbe7774a43579219656353f13fa0)

## **What is an Interface in Java?**

An **interface** in Java is a **contract** or **blueprint** that defines **what methods a class must implement**, **but not how**.
It contains **abstract methods** (and optionally default or static methods) that a class **agrees to implement**.


1️⃣ **Abstraction** – Hides implementation details, shows only method definitions.  
2️⃣ **Loose Coupling** – Depends on interface, not concrete class (easy to switch implementations).  
3️⃣ **Polymorphism** – One interface type can refer to many implementing objects.  
4️⃣ **Multiple Inheritance (of Type)** – A class can implement multiple interfaces.  
5️⃣ **Code Reusability** – Common behavior defined once, reused across classes.

   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s9d101d410a254b7c85893012a6b1f25b)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s5ca28c5e0c2f44f192b03d21c88198c3)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s2927fcff047b408b889eb77a2c11db34)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s7b4fc48aff334affb137ab806b40bfd1)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s97a59e6ec9ad4d60b7e0ab59a2f568e4)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1sec481a0c1e8644958fd185a584bc08a8)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s0fa95af59ea245aba9543c5030515605)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1sf8f1100cf1f34c4795b894ea18b95559)

## 💡 **1️⃣ Class Level**

|Feature|Interface|Abstract Class|
|---|---|---|
|**Access Modifier Allowed**|Only `public` or **package-private** (default)|`public`, `protected`, or **package-private**|
|**Can be declared `final`?**|❌ No (cannot be instantiated or final)|❌ No (must be extendable)|
|**Can be declared `abstract`?**|Implicitly abstract (no need to mention)|Must explicitly use `abstract` keyword|





---

## 💡 **2️⃣ Methods**

|Feature|Interface|Abstract Class|
|---|---|---|
|**Access Modifier (Abstract method)**|Always `public` (implicitly)|Can be `public`, `protected`, or package-private|
|**Default methods (Java 8+)**|Must be `public`|N/A|
|**Static methods (Java 8+)**|Must be `public`|Any modifier (`public`, `protected`, `private`)|
|**Private methods (Java 9+)**|✅ Allowed (only for reuse inside interface)|✅ Allowed|
|**Final methods**|❌ Not allowed|✅ Allowed|
|**Abstract methods count**|All methods are abstract by default (unless default/static)|Some can be abstract, others concrete|

{ } }`

---

## 💡 **3️⃣ Fields (Variables)**

|Feature|Interface|Abstract Class|
|---|---|---|
|**Access Modifier**|Always `public static final` (constants)|Can be any (`private`, `protected`, `public`)|
|**Value Requirement**|Must be initialized at declaration|Optional (can be uninitialized)|
|**Mutable?**|❌ No (since `final`)|✅ Yes (if not final)|


---

## ⚙️ **4️⃣ Object Rules**

|Feature|Interface|Abstract Class|
|---|---|---|
|**Instantiation**|❌ Not possible|❌ Not possible|
|**Implements / Extends**|Implemented by a class (`implements`)|Extended by a class (`extends`)|
|**Multiple inheritance**|✅ Allowed (class can implement many interfaces)|❌ Not allowed (only one abstract class)|
|**Constructors**|❌ Not allowed|✅ Allowed|

   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s5f432b40f7384cba9c4275dcaea980d5)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1sbd9d9920d5dd4c0a96394da9db6e714e)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1sbd24d9357e82480c88d436fa9f59a65d)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s499a64163acf4110a7c1cd4b92ceddcf)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s0e24e3cd96664f70878fead57d696f44)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s816c34f42b6b4f28824eceab2f9e9f22)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1sdea5359b5afd494fa2c3e78d5e741dd7)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s1c64acf972404b25a45ae140571e54bc)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s2fec6579be394f3eaeca827b4a7307d2)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s802868e154e5498e8b976457816ac471)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1s43194d01407745c98b634177efe77b14)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/u3i1s033d318172e442b8b8f242767b93776c/resources/u3i1sbbc6d666a3c5445096ca62729ee684a5)




## **1️⃣ Can abstract classes have constructors?**

✅ **Yes**, abstract classes **can have constructors**.

`abstract class Vehicle {     Vehicle() {         System.out.println("Vehicle constructor called");     }     abstract void start(); }`

- You **can define constructors** in an abstract class.

- The constructor **runs when a subclass object is created**, not for the abstract class itself.


---

## **2️⃣ Why can’t you create an object of an abstract class?**

- Abstract classes are **incomplete** — they may have **abstract methods with no body**.

- If Java allowed `new Vehicle()`, there would be **no implementation for `start()`**, so what would the JVM run?

- **Cannot instantiate something that is incomplete**.


`Vehicle v = new Vehicle(); // ❌ Compilation Error`

---

## **3️⃣ Then what’s the purpose of a constructor in an abstract class?**

- **To initialize fields and run code** when a **subclass object** is created.

```
class Car extends Vehicle {
    Car() {
        super(); // Calls Vehicle constructor
        System.out.println("Car constructor called");
    }
    void start() {
        System.out.println("Car started");
    }
}

public class Test {
    public static void main(String[] args) {
        Vehicle v = new Car();
        // Output:
        // Vehicle constructor called
        // Car constructor called
    }
}

```


✅ Notice: `Vehicle()` runs **through the subclass**, never directly.



![[Pasted image 20251015110826.png]]



![[Pasted image 20251015110844.png]]


A nested class can have private but a top level class can be public or default only.
Private means it is acessible within a class if u declare a class as private u say that class is
private to what? it looks meaningless

Similarly for static also


Making a constructor `final` is meaningless because **constructors cannot be overridden**, so the compiler forbids it.

- **`static`** means: _belongs to the class, not an instance_.
- **Constructors are used to create instances**.
- If you made a constructor `static`, it would exist **without an instance**, which **contradicts its purpose**.



| Feature         | public | protected | default | private | final | static | abstract |
| --------------- | ------ | --------- | ------- | ------- | ----- | ------ | -------- |
| Top-level class | ✅      | ❌         | ✅       | ❌       | ✅     | ❌      | ❌        |
| Concrete method | ✅      | ✅         | ✅       | ✅       | ✅     | ✅      | ❌        |
| Constructor     | ✅      | ✅         | ✅       | ✅       | ❌     | ❌      | ❌        |
-- A **private method** is visible **only within the class it is declared**.
- Subclasses **cannot see it**, so they **cannot override it**.
-----------------------------------------------------------------------------------------------------

An **abstract class** in Java is a **class that cannot be instantiated directly** and is intended to serve as a **base class for other classes**. It can contain **abstract methods (without a body) and concrete methods (with a body)**.

![[Pasted image 20251015112449.png]]
An **abstract class can exist without any abstract methods**.

| Feature        | public | protected | default | private           | final             | static                    | abstract                      |
| -------------- | ------ | --------- | ------- | ----------------- | ----------------- | ------------------------- | ----------------------------- |
| Abstract class | ✅      | ❌         | ✅       | ❌                 | ❌                 | ❌ (top-level), ✅ (nested) | ✅                             |
| Method         | ✅      | ✅         | ✅       | ✅ (concrete only) | ✅ (concrete only) | ✅ (concrete only)         | ✅ (must override in subclass) |
| Constructor    | ✅      | ✅         | ✅       | ✅                 | ❌                 | ❌                         | ❌                             |


![[Pasted image 20251015112519.png]]
![[Pasted image 20251015112540.png]]

------------------------------------------------------------------------------------------


![[Pasted image 20251015115152.png]]


![[Pasted image 20251015115213.png]]


----------------------------------------------------------------------------------




![[Pasted image 20251015120925.png]]

![[Pasted image 20251015121000.png]]



![[Pasted image 20251015121022.png]]



![[Pasted image 20251015121042.png]]



![[Pasted image 20251015121100.png]]

![[Pasted image 20251015121119.png]]




![[Pasted image 20251015121150.png]]



![[Pasted image 20251015121206.png]]



![[Pasted image 20251015121227.png]]



![[Pasted image 20251015121247.png]]


![[Pasted image 20251015121315.png]]


-------------------------------------------------------------------------------------

|Class Type|public|protected|default|private|final|static|abstract|Notes|
|---|---|---|---|---|---|---|---|---|
|Top-level concrete|✅|❌|✅|❌|✅|❌|❌|Only public or package-private|
|Top-level abstract|✅|❌|✅|❌|❌|❌|✅|Cannot be final, static top-level not allowed|
|Nested concrete|✅|✅|✅|✅|✅|✅|❌|Can be private, static allowed|
|Nested abstract|✅|✅|✅|✅|❌|✅|✅|Can be private/protected/public; static allowed|


|Method Type|public|protected|default|private|final|static|abstract|Notes|
|---|---|---|---|---|---|---|---|---|
|Concrete method|✅|✅|✅|✅|✅|✅|❌|Can combine static/final|
|Abstract method|✅|✅|✅|❌|❌|❌|✅|Must be overridden by subclass|
|Nested class methods|Same rules as above|||||||Inherited rules apply|


| Constructor Type | public | protected | default | private | final | static | Notes                                       |
| ---------------- | ------ | --------- | ------- | ------- | ----- | ------ | ------------------------------------------- |
| Concrete class   | ✅      | ✅         | ✅       | ✅       | ❌     | ❌      | Private used for Singleton/factory patterns |
| Abstract class   | ✅      | ✅         | ✅       | ✅       | ❌     | ❌      | Called via `super()` from subclass          |