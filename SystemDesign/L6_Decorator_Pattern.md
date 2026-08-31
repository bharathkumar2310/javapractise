What is the Decorator Method Design Pattern?
    
    The Decorator design pattern is a structural pattern used in object-oriented programming to add new functionality to objects dynamically without altering their structure. 
    In Java, this pattern is often employed to extend the behavior of objects in a flexible and reusable way.
    The Decorator Pattern lets you add new behavior to an object dynamically without changing its original class.
    Think of it as wrapping an object with additional functionality.

🎯 Real-world example

Imagine ordering coffee:

        ☕ Basic Coffee → ₹100
        🥛 Milk → ₹20
        🍫 Chocolate → ₹30

Instead of creating classes like:

    Coffee
    CoffeeWithMilk
    CoffeeWithChocolate
    CoffeeWithMilkAndChocolate
    CoffeeWithMilkAndChocolateAnd...

We use decorators:

    Coffee
    ↑
    MilkDecorator
    ↑
    ChocolateDecorator

Each decorator wraps the previous object.

1️⃣ The problem without Decorator

Suppose:

    interface Coffee {
    String getDescription();
    double getCost();
    }

A basic implementation:

    class SimpleCoffee implements Coffee {
    
        public String getDescription() {
            return "Simple Coffee";
        }
    
        public double getCost() {
            return 100;
        }
    }

Now we want:

Milk
Chocolate
Whipped Cream

Without Decorator, combinations explode:

    SimpleCoffee
    CoffeeWithMilk
    CoffeeWithChocolate
    CoffeeWithMilkAndChocolate
    CoffeeWithMilkAndWhippedCream
    CoffeeWithChocolateAndWhippedCream
    CoffeeWithMilkChocolateAndWhippedCream

❌ Too many classes.

2️⃣ Decorator solution

Step 1: Component Interface

    Both the original object and decorators implement the same interface.

    interface Coffee {
    String getDescription();
    double getCost();
    }

Step 2: Concrete Component

The original object:

    class SimpleCoffee implements Coffee {
    
        @Override
        public String getDescription() {
            return "Simple Coffee";
        }
    
        @Override
        public double getCost() {
            return 100;
        }
    }

Step 3: Create an abstract Decorator

The decorator also implements Coffee.

But importantly, it contains another Coffee object.

    abstract class CoffeeDecorator implements Coffee {
    
        protected Coffee coffee;
    
        public CoffeeDecorator(Coffee coffee) {
            this.coffee = coffee;
        }
    }

This is the most important part:

CoffeeDecorator HAS-A Coffee
3️⃣ Concrete Decorators
🥛 Milk
    
    class MilkDecorator extends CoffeeDecorator {
    
        public MilkDecorator(Coffee coffee) {
            super(coffee);
        }
    
        @Override
        public String getDescription() {
            return coffee.getDescription() + ", Milk";
        }
    
        @Override
        public double getCost() {
            return coffee.getCost() + 20;
        }
    }
🍫 Chocolate
    
    class ChocolateDecorator extends CoffeeDecorator {
    
        public ChocolateDecorator(Coffee coffee) {
            super(coffee);
        }
    
        @Override
        public String getDescription() {
            return coffee.getDescription() + ", Chocolate";
        }
    
        @Override
        public double getCost() {
            return coffee.getCost() + 30;
        }
    }
4️⃣ Using the Decorators
    
    public class Main {
    
        public static void main(String[] args) {
    
            Coffee coffee = new SimpleCoffee();
    
            coffee = new MilkDecorator(coffee);
    
            coffee = new ChocolateDecorator(coffee);
    
            System.out.println(coffee.getDescription());
            System.out.println(coffee.getCost());
        }
    }
Output
Simple Coffee, Milk, Chocolate
150.0
🔥 What is actually happening?

This:

    Coffee coffee = new SimpleCoffee();

Creates:

    SimpleCoffee

Then:

    coffee = new MilkDecorator(coffee);

Creates:

    MilkDecorator
    ↓ wraps
    SimpleCoffee

Then:

    coffee = new ChocolateDecorator(coffee);

Creates:

    ChocolateDecorator
    ↓
    MilkDecorator
    ↓
    SimpleCoffee

When you call:

    coffee.getCost();

The call flows like this:

    ChocolateDecorator.getCost()
    ↓
    MilkDecorator.getCost()
    ↓
    SimpleCoffee.getCost()

Calculation:

    SimpleCoffee       = 100
    MilkDecorator      = +20
    ChocolateDecorator = +30
    --------------------------------
    Total              = 150
    🧠 Core idea

The Decorator Pattern follows:

    Composition over inheritance

Instead of creating hundreds of subclasses:

    CoffeeWithMilk
    CoffeeWithChocolate
    CoffeeWithMilkChocolate

We dynamically compose objects:

    new ChocolateDecorator(
    new MilkDecorator(
    new SimpleCoffee()
    )
    );