1. What are Wrapper Classes in Java?
        
        Wrapper classes convert primitive data types into objects.
        
        Primitive	Wrapper Class
        byte	   Byte
        short	  Short
        int	       Integer
        long	  Long
        float	  Float
        double	  Double
        char	 Character
        boolean	 Boolean
        
        Example:
        
        int x = 10;
        Integer obj = Integer.valueOf(x);

2. Why do we need Wrapper Classes?
        
           Primitive types are not objects.
        
           Wrapper classes are needed because:
        
           Collections work only with objects
           Generics require objects
           Utility methods are available
           Needed for null values
           Used in serialization/frameworks

Example:

List<Integer> list = new ArrayList<>();
3. What is Autoboxing?

        Automatic conversion of primitive → wrapper object.
        
        int x = 5;
        Integer obj = x; // autoboxing
        
        Compiler converts it internally:
        
        Integer obj = Integer.valueOf(x);
4. What is Unboxing?

        Automatic conversion of wrapper object → primitive.
        
        Integer obj = 10;
        int x = obj; // unboxing
        
        Internally:
        
        int x = obj.intValue();

   5. Difference between Primitive and Wrapper Classes?
    
          Primitive	          Wrapper
          Not object	           Object
          Faster	                 Slower
          Less memory	        More memory
          Cannot store null	Can store null
          Used for performance	Used in collections/frameworks
   
   6. Why are Wrapper Classes Immutable?
    
            Once created, value cannot change.
    
            Integer a = 10;
            a = 20;
    
            New object is created.
    
            Benefits:
    
            Thread safety
            Security
            Hashing reliability

 7. Difference between Integer.parseInt() and Integer.valueOf()?

                   parseInt	                     valueOf
                   Returns primitive int	Returns Integer object
                   Faster	                 May use caching
    
            Example:
    
            int x = Integer.parseInt("10");
    
            Integer y = Integer.valueOf("10");

 8. What is Integer Caching?
    
             Java caches Integer objects from:
            
             -128 to 127
            
             Example:
            
             Integer a = 100;
             Integer b = 100;
            
             System.out.println(a == b); // true
            
             But:
            
             Integer a = 200;
             Integer b = 200;
            
             System.out.println(a == b); // false

    9. Why does Integer cache only -128 to 127?

            Because small numbers are used frequently.
        
            Improves:
        
            Memory usage
            Performance
   10. Difference between == and .equals() for Wrapper Classes?

                     ==
    
             Compares references.
    
             .equals()
    
             Compares values.
    
             Example:
    
             Integer a = 200;
             Integer b = 200;
    
             System.out.println(a == b); // false
             System.out.println(a.equals(b)); // true

11. Why does this print true?
    Integer a = 127;
    Integer b = 127;

System.out.println(a == b);

Because Integer cache is used.

Both point to same object.

12. Why does this print false?
    Integer a = 128;
    Integer b = 128;

System.out.println(a == b);

128 is outside cache range.

Different objects are created.

13. Can Wrapper Classes store null?

Yes.

Integer x = null;

Primitive cannot:

int x = null; // error
14. What happens here?

            Integer x = null;
            int y = x;
        
        Throws:
        
        NullPointerException
        
        Because unboxing happens internally:
        
        x.intValue()

15. Why are Wrapper Classes slower than primitives?

Because:
    
    Objects are created
    Heap memory used
    Boxing/unboxing overhead

16. Which is better for performance?

Primitive types.

int sum = 0;

Better than:

Integer sum = 0;
17. Can we use Wrapper Classes in switch?

Yes.

Integer x = 10;

switch(x) {
case 10:
System.out.println("Ten");
}

Unboxing occurs automatically.

18. What happens internally in Autoboxing?
    Integer x = 5;

Converted to:

Integer x = Integer.valueOf(5);
19. What happens internally in Unboxing?
    Integer x = 5;
    int y = x;

Converted to:

int y = x.intValue();
20. Difference between new Integer(10) and Integer.valueOf(10)?
    new Integer()	valueOf()
    Creates new object always	Uses cache
    More memory	Optimized

Example:

Integer a = new Integer(10);
Integer b = new Integer(10);

System.out.println(a == b); // false
21. Why is new Integer() discouraged?

Because:

Always creates new object
Wastes memory
Caching not used
22. Are Wrapper Classes Thread Safe?

Yes, because they are immutable.

23. Can Wrapper Classes be used in Collections?

Yes.

List<Integer> list = new ArrayList<>();

Collections cannot store primitives directly.

24. Why Generics don't support primitives?

Generics work only with objects.

Hence:

List<Integer>

Not:

List<int> // invalid
25. Which Wrapper Classes support caching?
    Byte
    Short
    Integer
    Long
    Character
    Boolean
26. What is the cache range for Character?
    0 to 127
27. Is String a Wrapper Class?

No.

String is a regular immutable class.

28. Why Wrapper Classes extend Number class?

Numeric wrappers extend:

Number

Example:

Integer
Double
Float

This provides methods like:

intValue()
doubleValue()
29. Which Wrapper Classes do not extend Number?
    Character
    Boolean
30. Can Wrapper Classes be final?

They already are final.

Example:

public final class Integer

Cannot be inherited.

MOST IMPORTANT INTERVIEW TRICK QUESTIONS
31. Output?
    Integer a = 100;
    Integer b = 100;

System.out.println(a == b);

Output:

true

Due to Integer cache.

32. Output?
    Integer a = 128;
    Integer b = 128;

System.out.println(a == b);

Output:

false

Outside cache range.

33. Output?
    Integer a = 100;
    int b = 100;

System.out.println(a == b);

Output:

true

Because a gets unboxed.

34. Output?
    Integer a = null;
    System.out.println(a + 10);

Output:

NullPointerException

Due to unboxing.

35. Output?
    Integer a = 10;
    Integer b = 10;

a++;

System.out.println(a == b);

Output:

false

Because:

a++

creates new Integer object.

VERY IMPORTANT INTERVIEW POINTS
Memorize These
Wrapper classes are:
Immutable
Final
Thread-safe
Integer cache:
-128 to 127
==
compares references
.equals()
compares values
Autoboxing

primitive → object

Unboxing

object → primitive

Collections + Generics

require wrapper classes

Common Interview Follow-Up
Why did Java introduce Autoboxing?

Before Java 5:

Integer obj = Integer.valueOf(10);

After Java 5:

Integer obj = 10;

Simplifies code.

Advanced Interview Question
Why can Wrapper Classes cause memory issues in loops?

Example:

Integer sum = 0;

for(int i = 0; i < 100000; i++) {
sum += i;
}

Each iteration:

unboxing
arithmetic
boxing again

Creates many objects.

Primitive is better:

int sum = 0;



        public static Integer valueOf(int i) {
        
            if(i >= -128 && i <= 127) {
                return IntegerCache.cache[i + 128];
            }
        
            return new Integer(i);
        }


        public static int parseInt(String s) {
        
            int result = 0;
        
            for(int i = 0; i < s.length(); i++) {
        
                char ch = s.charAt(i);
        
                int digit = ch - '0';
        
                result = result * 10 + digit;
            }
        
            return result;
        }



        private final int value;
        public int intValue() {
            return value;
        }


32. Why is Integer cache possible only because Integer is immutable?

        If Integer were mutable:
        
        Integer a = 10;
        Integer b = 10;
        
        Both could point to same cached object.
        
        If one changes:
        
        a.setValue(20);
        
        Then b also becomes 20 accidentally.
        
        So caching requires immutability.


    Output?
    Integer a = Integer.valueOf("10");
    int b = Integer.parseInt("10");
    
    System.out.println(a == b);
    
    Output:
    
    true
    
    Because:
    
    a.intValue() == b
    
    unboxing occurs automatically.



    Integer a = Integer.valueOf("200");
    int b = a.intValue();
    
    System.out.println(a == b);
    
    true


Here:

        left side = Wrapper (Integer)
        right side = primitive (int)
        
        So Java performs:
        
        unboxing
        
        Internally:
        
        a.intValue() == b
        
        becomes:
        
        200 == 200
        
        Hence:
        
        true