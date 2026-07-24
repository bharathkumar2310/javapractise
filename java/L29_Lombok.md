![img.png](../Images/Lombok1.png)

![img_1.png](../Images/Lombok2.png)

or download jar and cop it to lib folder and right click jar and add as library

![img_2.png](../Images/Lombok3.png)

![img_3.png](../Images/Lombok4.png)

![img_4.png](../Images/Lombok5.png)

![img_5.png](../Images/Lombok6.png)


![img.png](../Images/Lomb1.png)


List<Employee> employees = service.getEmployees();
val employees = service.getEmployees();

![img_1.png](../Images/Lomb2.png)

![img_2.png](../Images/Lomb3.png)

![img_3.png](../Images/Lomb4.png)

![img_4.png](../Images/Lomb5.png)

![img.png](../Images/Lomb6.png)

![img_1.png](../Images/Lomb7.png)

![img_2.png](../Images/Lomb8.png)


![img_3.png](../Images/Lomb9.png)


![img_4.png](../Images/Lomb10.png)

![img_5.png](../Images/Lomb11.png)


![img.png](../Images/Lomb12.png)

![img_1.png](../Images/Lomb13.png)

![img_2.png](../Images/Lomb14.png)

![img_3.png](../Images/Lomb15.png)

![img_4.png](../Images/Lomb16.png)


![img_5.png](../Images/Lomb17.png)


@RequiredArgsConstructor generates a constructor for:

All final fields that are not initialized.
            Or 
All fields annotated with @NonNull that are not initialized.

![img_6.png](../Images/Lomb18.png)

![img_7.png](../Images/Lomb19.png)

![img_8.png](../Images/Lomb20.png)


![img_9.png](../Images/Lomb21.png)

![img_10.png](../Images/Lomb22.png)

@Value : 

private final fields
+
public constructor
+
no setters

![img_11.png](../Images/Lomb23.png)

![img.png](../Images/Lomb24.png)


@Slf4j is also a Lombok annotation.

Lombok generates:

    private static final Logger log =
    LoggerFactory.getLogger(EmployeeService.class); 

behind the scenes.

So instead of writing:

    private static final Logger log =
    LoggerFactory.getLogger(EmployeeService.class);

you simply write:

    @Slf4j
    public class EmployeeService {
    }

-----------------------------------------------------------------------------------------------------------------------------------

Records :(Introduced in java 16+)

Records behave in the same way as @Value of Java
But you dont need external dependency, record is a java feature

    public record Employee(String name, int age) {}


equals to 

        public final class Employee { ... }

A record already extends java.lang.Record.
It generates everything exactly same as @Value
diff is getter is getName() in Lombok whereas name() in record
Lombok is a third-party library, not part of Java itself.


private final fields + public reqArgsConstructor + public getters + no setters+ public equals and hashcode + public toString

We can validate in constructors

    public record Employee(String name, int age) {
    
        public Employee {
            if (age < 0) {
                throw new IllegalArgumentException();
            }
        }
    }

Custom Methods  and static fields are allowed but instance fields are not allowed

Records were introduced to reduce boilerplate for immutable data carrier classes.
A record automatically generates private final fields, a canonical constructor, accessor methods, equals(), hashCode(), and toString(). 
are implicitly final, extend java.lang.Record, cannot have additional instance fields, but can have static fields, methods, constructors, and can implement interfaces. 
They are intended for immutable data representation and are similar in purpose to Lombok's @Value annotation, but are a built-in Java language feature.


We can have custom constructors but it must delegate to canonical constructor

    `public record Employee(String name, int age) {
    
        public Employee(String name) {
            this(name, 0);
        }
    }`