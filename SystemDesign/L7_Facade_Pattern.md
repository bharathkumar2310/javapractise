FACADE PATTERN :

    The Facade Design Pattern is a structural pattern that provides a simple and unified interface to a complex subsystem. 
    It hides the internal complexity of the system, making it easier to use and maintain.

❌ Problem without Facade

    Suppose you want to place an order.

    The client needs to interact with multiple services:

    PaymentService paymentService = new PaymentService();
    InventoryService inventoryService = new InventoryService();
    ShippingService shippingService = new ShippingService();

    inventoryService.checkStock();
    
    paymentService.makePayment();
    
    shippingService.shipProduct();

The client now knows:

    Which services exist
    The order in which to call them
    How they interact

This creates tight coupling with the subsystem.

✅ Solution: Facade

    Create one class that hides the complexity.

    OrderFacade orderFacade = new OrderFacade();

    orderFacade.placeOrder();

Internally:

    Client
    |
    v
    OrderFacade
    |
    ├── InventoryService
    ├── PaymentService
    └── ShippingService
Java Example

1️⃣ Subsystem classes
    
    class InventoryService {
    
        public boolean checkStock(String productId) {
            System.out.println("Checking stock...");
            return true;
        }
    }
    class PaymentService {
    
        public void makePayment() {
            System.out.println("Processing payment...");
        }
    }
    class ShippingService {
    
        public void shipProduct() {
            System.out.println("Shipping product...");
        }
    }
2️⃣ Facade
    
    class OrderFacade {
    
        private InventoryService inventoryService;
        private PaymentService paymentService;
        private ShippingService shippingService;
    
        public OrderFacade() {
            this.inventoryService = new InventoryService();
            this.paymentService = new PaymentService();
            this.shippingService = new ShippingService();
        }
    
        public void placeOrder(String productId) {
    
            boolean available = inventoryService.checkStock(productId);
    
            if (!available) {
                System.out.println("Product not available");
                return;
            }
    
            paymentService.makePayment();
    
            shippingService.shipProduct();
    
            System.out.println("Order placed successfully!");
        }
    }
3️⃣ Client
    
    public class Main {
    
        public static void main(String[] args) {
    
            OrderFacade orderFacade = new OrderFacade();
    
            orderFacade.placeOrder("P101");
        }
    }

Output:

    Checking stock...
    Processing payment...
    Shipping product...
    Order placed successfully!
🤔 What exactly does Facade do?

Facade mainly does two things:

1. Simplifies the client

Instead of:

    inventory.checkStock();
    payment.makePayment();
    shipping.shipProduct();

The client does:

orderFacade.placeOrder();
2. Hides subsystem complexity

        The client doesn't need to know:
        
        Which service to call
        In what order
        How services coordinate
⭐ Important Point

    Facade does not necessarily prevent access to the underlying classes.

You can still do:

    PaymentService payment = new PaymentService();
    payment.makePayment();
    
    Facade simply provides a convenient simplified interface.

Real-world examples in Java/Spring
🔥 SLF4J as a facade

When you write:

    logger.info("Application started");

SLF4J provides a common/simple interface while different logging implementations can work underneath.

🔥 Spring JdbcTemplate

Instead of manually handling:

    Connection
    → PreparedStatement
    → Execute Query
    → ResultSet
    → Close resources

You use a simpler interface:

    jdbcTemplate.query(...);

This is very facade-like, hiding much of the underlying complexity.


    Your Application
    |
    v
    SLF4J Logger
    |
    v
    Logging Implementation
    (Logback / Log4j2)
    |
    ├── Format the message
    ├── Add timestamp
    ├── Add thread name
    ├── Check log level
    ├── Write to console
    └── Write to file