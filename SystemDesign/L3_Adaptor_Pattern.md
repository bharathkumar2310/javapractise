Adapter :

    Adapter Design Pattern is a structural pattern that acts as a bridge between two incompatible interfaces, allowing them to work together. 
        It is especially useful for integrating legacy code or third-party libraries into a new system.
    
    It enables classes with incompatible interfaces to collaborate without modifying their source code.
    It promotes code reusability by allowing existing functionality to be used in new systems.
    It can be implemented in two ways: Class Adapter (using inheritance) and Object Adapter (using composition).
    The Adapter Pattern allows two incompatible interfaces to work together.



The problem in Java

    Suppose our application expects this interface:

    interface PaymentProcessor {
    void pay(double amount);
    }

Our application works with:

    PaymentProcessor processor;
    processor.pay(100);

But now we want to use an external/legacy payment system:

    class LegacyPaymentGateway {
    
        public void makePayment(double amount) {
            System.out.println("Payment made: " + amount);
        }
    }

❌ Problem:

Our application expects:

    pay()

But the legacy system provides:

    makePayment()

The interfaces are incompatible.

Solution: Adapter Pattern

We create an adapter.

    Client
    ↓
    PaymentProcessor
    ↓
    PaymentAdapter
    ↓
    LegacyPaymentGateway
Step 1: Target Interface

This is what our application expects.

        interface PaymentProcessor {
        void pay(double amount);
        }
Step 2: Existing incompatible class (Adaptee)
        
        class LegacyPaymentGateway {
        
            public void makePayment(double amount) {
                System.out.println("Legacy payment completed: " + amount);
            }
        }
Step 3: Adapter
    
    class PaymentAdapter implements PaymentProcessor {
    
        private LegacyPaymentGateway legacyGateway;
    
        public PaymentAdapter(LegacyPaymentGateway legacyGateway) {
            this.legacyGateway = legacyGateway;
        }
    
        @Override
        public void pay(double amount) {
            legacyGateway.makePayment(amount);
        }
    }

The adapter converts:

    pay()
    ↓
    makePayment()

Step 4: Client
    
    public class Main {
    
        public static void main(String[] args) {
    
            LegacyPaymentGateway legacyGateway =
                    new LegacyPaymentGateway();
    
            PaymentProcessor processor =
                    new PaymentAdapter(legacyGateway);
    
            processor.pay(1000);
        }
    }
Output
        Legacy payment completed: 1000.0
        The important idea ⭐
        
        The client only knows:
        
        PaymentProcessor
        
        It does not care that internally we are using:
        
        LegacyPaymentGateway
        
        The adapter handles the incompatibility.






    class RunnableAdapter implements Callable<Object> {
    
        private Runnable runnable;
    
        RunnableAdapter(Runnable runnable) {
            this.runnable = runnable;
        }
    
        @Override
        public Object call() {
            runnable.run();
            return null;
        }
    }



submit() with Runnable

ExecutorService has:

    Future<?> submit(Runnable task);

    But internally, execution mechanisms may work with a different abstraction such as Callable.

The incompatibility

        Runnable:
        
        void run();
        
        Callable:
        
        V call() throws Exception;

They have different methods and return types.

    So Java can conceptually adapt a Runnable into a Callable.

    Runnable
    |
    | Adapter
    ↓
    Callable

For example:

    Runnable task = () -> {
    System.out.println("Hello");
    };

executorService.submit(task);

    Internally, Java can wrap/adapt that task so it fits the mechanism used to produce a Future.

Conceptually:
    
    class RunnableAdapter implements Callable<Object> {
    
        private Runnable runnable;
    
        RunnableAdapter(Runnable runnable) {
            this.runnable = runnable;
        }
    
        @Override
        public Object call() {
            runnable.run();
            return null;
        }
    }

So yes, submit(Runnable) → internally adapting/wrapping it into another compatible form is a practical Adapter Pattern example.