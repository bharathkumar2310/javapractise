🔥 SECTION 1: OOP + CONCEPTUAL (Interview-style)

1. What is a String internally?
        
           It’s a class (java.lang.String)
           Internally backed by char[] (Java 8) or byte[] (Java 9+)
        
        👉 Follow-up:
        
        Why was it changed to byte[]? → memory optimization (Compact Strings)
Compact Strings feature
        
        Introduced in Java 9 (called Compact Strings)
        
        If string contains only Latin characters → stored as 1 byte per char (LATIN1)
        If it contains complex characters (like emojis, some Asian scripts) → stored as 2 bytes (UTF-16)

So Java dynamically chooses the storage.

   2. Why is String immutable?
        
           Key reasons (interview gold):
        
           Security (used in class loading, URLs, DB connections)
           Thread safety
           String pool optimization
           HashMap key reliability
        
           👉 Follow-up trap:
        
           “How is immutability achieved?”

                   final class
                   private final fields
                   no setters
                   returns new object on modification
   
  3. What is String Constant Pool (SCP)?
    
          Special memory area inside heap
          Stores unique string literals
    
       👉 Key idea:
    
       Avoids duplicate objects → memory efficient


4. Heap vs SCP — why both?
    
       SCP → reuse literals
       Heap → dynamic objects (new keyword)

   5. Difference:
            
                  String s1 = "java";              // SCP
                  String s2 = new String("java"); // Heap + SCP
            
            👉 Important:
            
            new always creates a new object

6. What does intern() do?

        Moves string to SCP (or returns existing reference)

7. String vs StringBuilder vs StringBuffer

   | Feature     | String | StringBuilder | StringBuffer |
   | ----------- | ------ | ------------- | ------------ |
   | Mutable     | ❌      | ✅             | ✅            |
   | Thread-safe | ✅      | ❌             | ✅            |
   | Performance | Slow   | Fast          | Medium       |


👉 Interview follow-up:

    Why StringBuilder faster? → no synchronization

8. Is String thread-safe?

        👉 Yes (immutable)

9. Can we make String mutable?

         👉 ❌ Not directly (but can use reflection — advanced trick)

   10. Why String is final?

        👉 Prevent subclassing → ensures immutability/security

⚡ 2. OUTPUT-BASED QUESTIONS (VERY IMPORTANT)
  11.

        String s1 = "java";
        String s2 = "java";
        System.out.println(s1 == s2);

👉 true (same SCP reference)

12. 

      String s1 = new String("java");
      String s2 = "java";
      System.out.println(s1 == s2);

👉 false

13.
String s1 = new String("java");
String s2 = s1.intern();
System.out.println(s1 == s2);

👉 false (heap vs SCP)

14.
String s1 = "ja" + "va";
String s2 = "java";
System.out.println(s1 == s2);

👉 true (compile-time optimization)

15. ⚠️ VERY IMPORTANT
    String s1 = "ja";
    String s2 = s1 + "va";
    String s3 = "java";
    System.out.println(s2 == s3);
        
        👉 Since s1 is a variable, Java cannot decide at compile time
        👉 So concatenation happens at runtime

👉 false (runtime concatenation)

16. ⚠️ TRICK

    final String s1 = "ja";
    String s2 = s1 + "va";
    String s3 = "java";
    System.out.println(s2 == s3);

👉 true (compiler optimizes final)
    
    s1 becomes a compile-time constant
    👉 Its value cannot change

17.
String s = "hello";
s.concat("world");
System.out.println(s);

👉 hello (immutable)

18.
StringBuilder sb = new StringBuilder("hello");
sb.append("world");
System.out.println(sb);

👉 helloworld

19.
String s = "hello";
s.replace('l','x');
System.out.println(s);

👉 hello

20.
String s1 = "abc";
String s2 = "abc";
System.out.println(s1.hashCode() == s2.hashCode());

👉 true

🧠 3. SCENARIO-BASED (INTERVIEW FAVORITE)

21. Why Strings are used as HashMap keys?

        Immutable → hashCode won’t change
    22. What if String was mutable in HashMap?

            👉 Data retrieval would break (bucket mismatch)

 23. You see too many duplicate strings in memory. Fix?
    
                Use intern()
                Use String pool effectively
   24. When to use StringBuilder?

            Frequent modifications (loops, concatenation)
 25. Why not use String in loops?
           s = s + "a";

           👉 Creates new object every time → memory waste


⚡ 5. Why char[] preferred over String for passwords?

        👉 Because:
        
        String is immutable → stays in memory until GC
        char[] can be manually cleared
        password[0] = '\0';

⚙️ 7. equals() vs contentEquals()

👉 equals() → compares String objects
👉 contentEquals() → compares with StringBuffer/StringBuilder

⚙️ 8. compareTo() vs equals()

👉 equals() → checks equality
👉 compareTo() → lexicographical comparison

"abc".compareTo("abd") → negative



------------------------------------------------------------------------------------------------------------------------------------