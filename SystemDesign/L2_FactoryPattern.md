Factory Pattern :

    The Factory Design Pattern is a Creational Design Pattern.

Simple definition

    The Factory Pattern provides a way to create objects without exposing the object-creation logic directly to the client.
    The factory pattern is a creational design pattern that abstracts object creation by delegating instantiation logic to a separate function, method, or class rather than calling constructors directly
    The Factory Method is a creational design pattern that defines an interface for creating objects but lets subclasses decide which object to instantiate. It promotes loose coupling by delegating object creation to a method, making the system more flexible and extensible.
    
    Subclasses override the factory method to create specific object types, enabling flexible object creation.
    Supports easy addition of new product types and improves maintainability and adaptability at runtime.



1. Why do we need Factory Pattern?

        Imagine we have different notification types:

        Email
        SMS
        Push Notification

Without Factory:

        String type = "EMAIL";
        
        Notification notification;
        
        if (type.equals("EMAIL")) {
        notification = new EmailNotification();
        } else if (type.equals("SMS")) {
        notification = new SMSNotification();
        } else if (type.equals("PUSH")) {
        notification = new PushNotification();
        }

Problem

    The object creation logic is scattered in the client.

If we add:

    WhatsApp Notification

we may need to modify multiple places.

| Problem                     | How Factory helps              |
| --------------------------- | ------------------------------ |
| Code changes in many places | Centralizes creation           |
| Tight coupling              | Client depends on abstraction  |
| Complex object creation     | Hides creation logic           |
| Runtime object selection    | Factory decides implementation |
| Changing implementation     | Usually changes only factory   |


2. Factory Pattern Solution

   Step 1: Create an interface

       interface Notification {
       void send();
       }
       Step 2: Create implementations
       class EmailNotification implements Notification {
    
       public void send() {
       System.out.println("Sending Email");
       }
       }
       class SmsNotification implements Notification {
    
       public void send() {
       System.out.println("Sending SMS");
       }
       }
       class PushNotification implements Notification {
    
       public void send() {
       System.out.println("Sending Push Notification");
       }
       }
       Step 3: Create the Factory
       class NotificationFactory {
    
       public static Notification createNotification(String type) {
    
            if (type.equalsIgnoreCase("EMAIL")) {
                return new EmailNotification();
            }
    
            if (type.equalsIgnoreCase("SMS")) {
                return new SmsNotification();
            }
    
            if (type.equalsIgnoreCase("PUSH")) {
                return new PushNotification();
            }
    
            throw new IllegalArgumentException("Invalid notification type");
       }
       }
       Step 4: Client uses Factory
       public class Main {
    
       public static void main(String[] args) {
    
            Notification notification =
                    NotificationFactory.createNotification("EMAIL");
    
            notification.send();
       }
       }

Output:

    Sending Email



---------------------------------------------------------------------------------------------------------------------------------


Without Factoy :


    interface Transport {
    void deliver();
    }
    
    class Truck implements Transport {
    public void deliver() {
    System.out.println("Delivering by Truck");
    }
    }
    
    class Ship implements Transport {
    public void deliver() {
    System.out.println("Delivering by Ship");
    }
    }
    
    class DeliveryService {
    
        public void processDelivery(String type) {
    
            Transport transport;
    
            if (type.equals("ROAD")) {
                transport = new Truck();
            } else if (type.equals("SEA")) {
                transport = new Ship();
            } else {
                throw new IllegalArgumentException("Invalid type");
            }
    
            transport.deliver();
        }
    }
    
    public class Main {
    
        public static void main(String[] args) {
    
            DeliveryService service = new DeliveryService();
    
            service.processDelivery("ROAD");
        }
    }



Simple Factory :


        interface Transport {
        void deliver();
        }
        
        
        // PRODUCTS
        
        class Truck implements Transport {
        
            public void deliver() {
                System.out.println("Delivering by Truck");
            }
        }
        
        class Ship implements Transport {
        
            public void deliver() {
                System.out.println("Delivering by Ship");
            }
        }
        
        
        // SIMPLE FACTORY
        
        class TransportFactory {
        
            public Transport createTransport(String type) {
        
                if (type.equals("ROAD")) {
                    return new Truck();
                }
        
                if (type.equals("SEA")) {
                    return new Ship();
                }
        
                throw new IllegalArgumentException("Invalid type");
            }
        }
        
        
        // BUSINESS LOGIC
        
        class DeliveryService {
        
            private TransportFactory factory =
                    new TransportFactory();
        
            public void processDelivery(String type) {
        
                // Ask factory to create the object
                Transport transport =
                        factory.createTransport(type);
        
                // Use the object
                transport.deliver();
            }
        }
        
        
        // CLIENT
        
        public class Main {
        
            public static void main(String[] args) {
        
                DeliveryService service =
                        new DeliveryService();
        
                service.processDelivery("ROAD");
            }
        }




WITH FACTORY METHOD PATTERN :


    interface Transport {
    void deliver();
    }
    
    
    // PRODUCTS
    
    class Truck implements Transport {
    
        public void deliver() {
            System.out.println("Delivering by Truck");
        }
    }
    
    
    class Ship implements Transport {
    
        public void deliver() {
            System.out.println("Delivering by Ship");
        }
    }
    
    
    // CREATOR
    
    abstract class Logistics {
    
        // Factory Method
        abstract Transport createTransport();
    
    
       

    }
    
    
    // CONCRETE CREATOR
    
    class RoadLogistics extends Logistics {
    
        @Override
        Transport createTransport() {
            return new Truck();
        }
    }
    
    
    class SeaLogistics extends Logistics {
    
        @Override
        Transport createTransport() {
            return new Ship();
        }
    }
    
    
    // CLIENT
    
    public class Main {
    
        public static void main(String[] args) {
    
            Logistics logistics =
                    new RoadLogistics();
    
            logistics.processDelivery();
        }
    }



ISSUES WITH BASIC FACTORY :


1️⃣ Factory violates Open/Closed Principle

Suppose you add a new transport:

    class Plane implements Transport {
    public void deliver() {
    System.out.println("Delivering by Plane");
    }
    }

Now you must modify the existing factory:

    class TransportFactory {
    
        public Transport createTransport(String type) {
    
            if (type.equals("ROAD")) {
                return new Truck();
            }
    
            if (type.equals("SEA")) {
                return new Ship();
            }
    
            if (type.equals("AIR")) {  // NEW CODE
                return new Plane();
            }
    
            throw new IllegalArgumentException();
        }
    }

So:

    Add new product
    ↓
    Modify existing Factory ❌

Ideally:

    Add new product
    ↓
    Add new code without modifying existing code ✅

This is the main problem.

2️⃣ The Factory becomes a God Class

Imagine eventually you have:

    Truck
    Ship
    Plane
    Train
    Drone
    Bike
    Courier

Your factory becomes:

    if ROAD → Truck
    if SEA → Ship
    if AIR → Plane
    if RAIL → Train
    if DRONE → Drone
...

One class now knows about every concrete implementation.

As the application grows:

    More Products
    ↓
    Bigger Factory
    ↓
    Harder to maintain


1. Hide object creation logic
2. Reduce coupling
3. Centralize object creation




ABSTRACT FACTORY 



        // ================= PRODUCT INTERFACES =================
        
        interface Button {
        void paint();
        }
        
        interface Checkbox {
        void paint();
        }
        
        
        // ================= WINDOWS PRODUCTS =================
        
        class WindowsButton implements Button {
        
            @Override
            public void paint() {
                System.out.println("Windows Button");
            }
        }
        
        class WindowsCheckbox implements Checkbox {
        
            @Override
            public void paint() {
                System.out.println("Windows Checkbox");
            }
        }
        
        
        // ================= MAC PRODUCTS =================
        
        class MacButton implements Button {
        
            @Override
            public void paint() {
                System.out.println("Mac Button");
            }
        }
        
        class MacCheckbox implements Checkbox {
        
            @Override
            public void paint() {
                System.out.println("Mac Checkbox");
            }
        }
        
        
        // ================= ABSTRACT FACTORY =================
        
        interface GUIFactory {
        
            Button createButton();
        
            Checkbox createCheckbox();
        }
        
        
        // ================= WINDOWS FACTORY =================
        
        class WindowsFactory implements GUIFactory {
        
            @Override
            public Button createButton() {
                return new WindowsButton();
            }
        
            @Override
            public Checkbox createCheckbox() {
                return new WindowsCheckbox();
            }
        }
        
        
        // ================= MAC FACTORY =================
        
        class MacFactory implements GUIFactory {
        
            @Override
            public Button createButton() {
                return new MacButton();
            }
        
            @Override
            public Checkbox createCheckbox() {
                return new MacCheckbox();
            }
        }
        
        
        // ================= CLIENT APPLICATION =================
        
        class Application {
        
            private Button button;
            private Checkbox checkbox;
        
            public Application(GUIFactory factory) {
        
                // Client doesn't know concrete classes
                button = factory.createButton();
                checkbox = factory.createCheckbox();
            }
        
            public void paint() {
        
                button.paint();
                checkbox.paint();
            }
        }
        
        
        // ================= MAIN =================
        
        public class Main {
        
            public static void main(String[] args) {
        
                GUIFactory factory;
        
                String os = "WINDOWS";
        
                if (os.equals("WINDOWS")) {
                    factory = new WindowsFactory();
                } else {
                    factory = new MacFactory();
                }
        
                Application application = new Application(factory);
        
                application.paint();
            }
        }