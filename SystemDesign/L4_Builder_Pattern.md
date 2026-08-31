Builder Pattern :

    The Builder Pattern is a creational design pattern used to construct complex objects step by step.
    Instead of passing many values through a constructor, you use a builder to gradually configure the object and finally create it.

😵 Problem without Builder

Imagine a User class:

        class User {
        private String name;
        private int age;
        private String email;
        private String phone;
        private String address;
        
            public User(String name, int age, String email,
                        String phone, String address) {
                this.name = name;
                this.age = age;
                this.email = email;
                this.phone = phone;
                this.address = address;
            }
        }

Creating an object:

        User user = new User(
        "Bharath",
        25,
        "bharath@gmail.com",
        "9876543210",
        "Chennai"
        );

Problems 🤔

1. Too many constructor parameters

        It's difficult to remember:

        name → age → email → phone → address

You might accidentally do:

        new User("Bharath", 25, "9876543210",
        "bharath@gmail.com", "Chennai");

The values can easily be placed in the wrong order.

2. Optional parameters

        Maybe only these are required:

            name
            email

        But age, phone, and address are optional.

Then what do we do?

    new User("Bharath", 25, "bharath@gmail.com", null, null);

😐 Not very clean.

3. Too many constructor combinations

You might start creating constructors like:

        User(String name)
        
        User(String name, String email)
        
        User(String name, String email, String phone)
        
        User(String name, String email, String phone, String address)

This is called the Telescoping Constructor Problem.


✅ Solution: Builder Pattern

With Builder:

    User user = new User.Builder()
    .name("Bharath")
    .age(25)
    .email("bharath@gmail.com")
    .phone("9876543210")
    .build();

Now it is very clear:

    name = Bharath
    age = 25
    email = bharath@gmail.com
    phone = 9876543210

You can also skip optional fields:

    ~~User user = new User.Builder()
    .name("Bharath")
    .email("bharath@gmail.com")
    .build();~~




    class User {
    
        private String name;
        private int age;
        private String email;
        private String phone;
    
        private User(Builder builder) {
            this.name = builder.name;
            this.age = builder.age;
            this.email = builder.email;
            this.phone = builder.phone;
        }
    
        public static class Builder {
    
            private String name;
            private int age;
            private String email;
            private String phone;
    
            public Builder name(String name) {
                this.name = name;
                return this;
            }
    
            public Builder age(int age) {
                this.age = age;
                return this;
            }
    
            public Builder email(String email) {
                this.email = email;
                return this;
            }
    
            public Builder phone(String phone) {
                this.phone = phone;
                return this;
            }
    
            public User build() {
                return new User(this);
            }
        }
    }


| Situation                | Why Builder helps                     |
| ------------------------ | ------------------------------------- |
| Many parameters          | Avoids huge constructors              |
| Optional fields          | Easily skip unwanted fields           |
| Complex object creation  | Build step by step                    |
| Readability              | `.name().age().email()` is clear      |
| Parameter order problems | Named methods avoid confusion         |
| Immutable objects        | Builder can construct the object once |
