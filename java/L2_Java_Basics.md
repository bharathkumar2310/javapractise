**_STATIC :_**

    
    The static keyword in Java is used for memory management and belongs to the class rather than any specific instance. 
    It allows members (variables, methods, blocks, and nested classes) to be shared among all objects of a class.
    
    Memory is allocated only once when the class is loaded.
    No object creation is needed to access static members; use the class name directly.
    Static methods and variables can’t access non-static members directly.
    Static methods can’t be overridden because they belong to the class, not instances.

1. Static Variables
   A static variable is also known as a class variable. It is shared among all instances of the class and is used to store data that should be common for all objects.


        class Student {
        
            int rollNo;
            String name;
            
            // static variable
            static String Training_Center
                = "GFG"; 
        
            Student(int r, String n){
                
                rollNo = r;
                name = n;
            }
        
            void display(){
                
                System.out.println(rollNo + " " + name + " "
                                   + Training_Center);
            }
        }
        
        public class GFG{
        
            public static void main(String[] args){
                
                Student s1 = new Student(101, "Ravi");
                Student s2 = new Student(102, "Amit");
        
                s1.display();
                s2.display();
            }
        }

Output
101 Ravi GFG
102 Amit GFG

2. Static Blocks

        A static block is executed only once when the class is first loaded into memory. It is often used to initialize static variables or perform configuration tasks before the main method executes.


class Geeks{

    // static variable
    static int a = 10;
    static int b;
    
    // static block
    static{
        
        System.out.println("Static block initialized.");
        b = a * 4;
    }

    public static void main(String[] args)
    {
       System.out.println("from main");
       System.out.println("Value of a : "+a);
       System.out.println("Value of b : "+b);
    }
}




Output
Static block initialized.
from main
Value of a : 10
Value of b : 40



3. Static Methods

        A static method belongs to the class rather than to any object. It can be called directly using the class name.

        Can access only static data directly.
        Cannot access instance variables or methods directly.
        Cannot use this or super keywords.

        class Geeks{
        
            // static variable
            static int a = m1();
        
            // static block
            static{
                
                System.out.println("Inside static block");
            }
        
            // static method
            static int m1(){
                
                System.out.println("From m1");
                return 20;
            }
        
            public static void main(String[] args){
                
                System.out.println("Value of a: " + a);
                System.out.println("From main");
            }
        }


Output
From m1
Inside static block
Value of a: 20
From main


4. Static Nested Classes
   A static nested class is a class declared as static inside another class. It can be accessed without creating an object of the outer class.



class Outer {

    static class Inner{

        void show(){
            
            System.out.println(
                "Static Nested Class Method");
        }
    }

    public static void main(String[] args)
    {
        Outer.Inner obj = new Outer.Inner();
        obj.show();
    }
}

Output
Static Nested Class Method
Note: The Inner class is accessed using the outer class name (Outer.Inner).



-------------------------------------------------------------------------------------------------------------------------------------


**_FINAL :_**

    
    The final keyword in Java is used to restrict modification. It can be applied to variables, methods, and classes. 
    The final keyword has different meanings based on where it is applied:
    - Final Variables: A final variable can only be assigned once. Once a final variable has been assigned a value, it cannot be changed. This is often used to create constants.
    It can be applied to variables, methods, and classes. The final keyword has different meanings based on where it is applied:
    The final keyword is a non-access modifier used to restrict modification. 
      - It applies to variables (value cannot change) methods (cannot be overridden) and classes (cannot be extended).
      - It helps create constants, control inheritance and enforce fixed behavior.


Characteristics of final keyword in Java

        Final variables hold a value that cannot be reassigned after initialization.
        Final methods cannot be overridden by subclasses.
        Final classes cannot be extended.
        Initialization rules require that a final variable must be assigned exactly once either at declaration or inside constructors or initializer blocks.
        Reference final variables cannot change which object they point to though the internal state of the object can change.
        Static final variables represent constants shared across all objects.
        Blank final variables are declared without initialization and must be assigned once before use.
        Local final variables inside methods must be initialized within their block.


| Feature                  | `static`                                           | `final`                                                |
| ------------------------ | -------------------------------------------------- | ------------------------------------------------------ |
| **Meaning**              | Belongs to the **class**, not object               | Cannot be **changed / overridden / extended**          |
| **Memory**               | Stored in **method area (class level)**            | Depends (variable/object), but value becomes constant  |
| **Access**               | Accessed using **ClassName.member**                | Access depends on modifier (can be instance or static) |
| **Purpose**              | Share common data across objects                   | Make things **immutable / constant**                   |
| **With Variables**       | One copy shared across all objects                 | Value cannot be reassigned                             |
| **With Methods**         | Cannot be overridden (only hidden)                 | Cannot be overridden                                   |
| **With Classes**         | Cannot be applied to top-level class (only nested) | Class cannot be extended (no inheritance)              |
| **Initialization**       | At class loading time                              | Must be initialized once                               |
| **Object Dependency**    | No object needed                                   | Depends (can be instance or static)                    |
| **Keyword Combination**  | Can be combined with `final`                       | Can be combined with `static`                          |
| **Inheritance Behavior** | Inherited but not polymorphic                      | Prevents inheritance/overriding                        |
| **Binding**              | Compile-time binding                               | Compile-time restriction                               |


🔥 What is static final in Java?

        static final is a combination of two keywords:
        
        static → belongs to the class (one copy shared)
        final → value cannot be changed once assigned
        
        👉 So together:
        
        static final = a constant value shared across all objects and cannot be modified



| Feature                  | `this`                           | `super`                                  |
| ------------------------ | -------------------------------- | ---------------------------------------- |
| **Meaning**              | Refers to **current object**     | Refers to **parent class object**        |
| **Usage Context**        | Within same class                | Used in subclass                         |
| **Access**               | Current class variables/methods  | Parent class variables/methods           |
| **Constructor Call**     | Calls **same class constructor** | Calls **parent class constructor**       |
| **Method Call**          | Calls current class method       | Calls parent class method                |
| **Variable Access**      | Current class variable           | Parent class variable                    |
| **Inheritance Needed?**  | ❌ No                             | ✅ Yes                                    |
| **First Statement Rule** | `this()` must be first           | `super()` must be first                  |
| **Default Behavior**     | No default insertion             | Compiler inserts `super()` automatically |
